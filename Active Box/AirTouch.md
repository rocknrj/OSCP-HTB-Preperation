### Nmap
```
nmap -sV -sC -vv 10.129.4.199

---OUTPUT---
Nmap scan report for 10.129.4.199
Host is up, received echo-reply ttl 63 (0.014s latency).
Scanned at 2026-02-25 09:18:47 EST for 2s
Not shown: 999 closed tcp ports (reset)
PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 62 OpenSSH 8.2p1 Ubuntu 4ubuntu0.11 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 bd:90:00:15:cf:4b:da:cb:c9:24:05:2b:01:ac:dc:3b (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCt5/czuvlRZ0Ueo5rURjmvlJDipbg3G8orjGjxa9ZuqUM5ZfZPBFKcRFji0HgJc6bQFTXDEXStqG5yxtieKu4LxNWyvuFtFawpQn+4v1qaA5j6E85Zh8qeE993mf+Q/Ea5YfIsZ/otloBj5UsOER8Y+t0/oybf2vVsBc4/925ekSL6Gk3p9BQRs2s4/n33+nEfq2C+bP4F8JkoUZgTPCV8MMat+mAc5t3hxQlUbAe2taiM8+Km8CEFaQkGdZDIPRaeYqLmrmRnNLtaOrYpzsea98Pt/54QICcusk0nsT39cXsbM5mW8bFpeEwXu+w/KRvtRkSg3QRWypilddyUBgEpAU4FEn8ifL2rbNIJ/C4NPNs2O1FzNi+E6twdRz1/p6ln0in3Y5PRXo4Y3Ln/PlqI8V1BrC8zfq7PIPuC4X7Agdq2ktnracnsL8oOhfLRWwrHaPOX2tZGA3dtRs1BiJbU3IiQQOf3IPnnQDc1lgNvlrYz7tFwrIvaSvCJWVZfIE0=
|   256 6e:e2:44:70:3c:6b:00:57:16:66:2f:37:58:be:f5:c0 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBIFdougpfxwAEIWPEa46kK7yuwcialkBHhi6CR0aNOdjjNuPKkbc8GGATnt0vr5eEoc9lsYRRnBoyhoHZMd4oGw=
|   256 ad:d5:d5:f0:0b:af:b2:11:67:5b:07:5c:8e:85:76:76 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIPp9qQHbtPkcaGbM4SnotIbktxIUaybHBXxDXKgyqYnK
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```
- UDP Scan:
```
nmap -sU --top-ports=10 -vv 10.129.4.199

---OUTPUT---
Nmap scan report for 10.129.4.199
Host is up, received echo-reply ttl 63 (0.026s latency).
Scanned at 2026-02-25 09:18:03 EST for 5s

PORT     STATE         SERVICE      REASON
53/udp   closed        domain       port-unreach ttl 63
67/udp   closed        dhcps        port-unreach ttl 63
123/udp  closed        ntp          port-unreach ttl 63
135/udp  open|filtered msrpc        no-response
137/udp  open|filtered netbios-ns   no-response
138/udp  closed        netbios-dgm  port-unreach ttl 63
161/udp  open          snmp         udp-response ttl 62
445/udp  closed        microsoft-ds port-unreach ttl 63
631/udp  closed        ipp          port-unreach ttl 63
1434/udp closed        ms-sql-m     port-unreach ttl 63

Read data files from: /usr/share/nmap
Nmap done: 1 IP address (1 host up) scanned in 5.67 seconds
           Raw packets sent: 50 (3.637KB) | Rcvd: 19 (1.715KB)
```

- Using snmpwalk I get some info including a default password:
```
snmpwalk -v 2c -c public 10.129.4.199

---OUTPUT---   
iso.3.6.1.2.1.1.1.0 = STRING: "\"The default consultant password is: RxBlZhLmOkacNWScmZ6D (change it after use it)\""
iso.3.6.1.2.1.1.2.0 = OID: iso.3.6.1.4.1.8072.3.2.10
iso.3.6.1.2.1.1.3.0 = Timeticks: (42952) 0:07:09.52
iso.3.6.1.2.1.1.4.0 = STRING: "admin@AirTouch.htb"
iso.3.6.1.2.1.1.5.0 = STRING: "Consultant"
iso.3.6.1.2.1.1.6.0 = STRING: "\"Consultant pc\""
iso.3.6.1.2.1.1.8.0 = Timeticks: (0) 0:00:00.00
iso.3.6.1.2.1.1.9.1.2.1 = OID: iso.3.6.1.6.3.10.3.1.1
iso.3.6.1.2.1.1.9.1.2.2 = OID: iso.3.6.1.6.3.11.3.1.1
iso.3.6.1.2.1.1.9.1.2.3 = OID: iso.3.6.1.6.3.15.2.1.1
iso.3.6.1.2.1.1.9.1.2.4 = OID: iso.3.6.1.6.3.1
iso.3.6.1.2.1.1.9.1.2.5 = OID: iso.3.6.1.6.3.16.2.2.1
iso.3.6.1.2.1.1.9.1.2.6 = OID: iso.3.6.1.2.1.49
..
..
..
```

- I can ssh into target 
```
ssh consultant@10.129.4.199

> RxBlZhLmOkacNWScmZ6D
```
![[Pasted image 20260225092829.png]]
- There is a diagram which I can take from the home folder:
![[Pasted image 20260225103239.png]]
- Checking sudo privileges I have all:
```
sudo -l

---OUTPUT---
Matching Defaults entries for consultant on AirTouch-Consultant:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User consultant may run the following commands on AirTouch-Consultant:
    (ALL) NOPASSWD: ALL
```
![[Pasted image 20260225092912.png]]
- I switch to root user. Also this feels like a container we need to escape.
```
sudo su
```
![[Pasted image 20260225092951.png]]

- In root directory I see an interesting folder `eaphammer`
	- can't find much
- Checking `ip a` we see a lot of interfaces:
![[Pasted image 20260225102943.png]]

- From this and the picture we can see that we would need to access Corp VLAN to compromise the target.
