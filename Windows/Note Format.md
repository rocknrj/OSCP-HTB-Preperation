# Reconnaissance
- 
## Nmap Enumeration
- We pass the commands:
	```bash
nmap -sV -sC -vv 10.10.
nmap -sU --top-ports=10 -vv 10.10.

---OUTPUT-TCP---


---OUTPUT-UDP---


```

## Directory Enumeration
- Gobuster:
	- Directory
		```bash
gobuster dir

---OUTPUT---


```
		- Next Directory
			```bash

```
	- VHost
		```bash
gobuster vhost

---OUTPUT---


```
- Ffuf
	```bash
ffuf

---OUTPUT---


```
- Dirsearch
	```bash
dirsearch -u

---OUTPUT---


```
- Dirbuster
	- 

## Website Enumeration
- 
### Direct
- 

### Via BurpSuite
- 

--------------
## Initial Foothold in Website
- 

------------
## Privilege Escalation in Website
- 

----------
## Initial Foothold in Target

- 
----------
## Lateral Movement in Target
- 
-----------
## Privilege Escalation in Target
- 
-------
--------