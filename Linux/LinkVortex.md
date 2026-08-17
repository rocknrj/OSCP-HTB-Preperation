10.10.11.47 leads to linkvortex.htb
	add to /etc/hosts
dirbuster reveals some interesting pages :
	linkvortex.htb/author/admin
	linkvortex.htb/rss
	linkvortex.htb/author/admin/rss
	- I see Ghost 5.58 version which leads to CVE-2023-40028 : Arbritrary file read
		- https://github.com/0xDTC/Ghost-5.58-Arbitrary-File-Read-CVE-2023-40028
	- **ALWAYS CHECK FOR robots.txt in url**
		- robots.txt shows a /ghost
- **NEW TOOL ffuf for FUZZING**
	- we use for web discovery
		- SET FUZZ for where you want tot check in url
		- grep to find status 200 response
	- ffuf -u http://linkvortex.htb/ -w /usr/share/seclists/Discovery/Web-Content/big.txt   -H "Host:FUZZ.linkvortex.htb" | grep "Status: 200"
		- we find dev
			- says launching soon
			- enumerate this in dirsearch (dirbuster doesnt find for some reason)
				- we find a lot of .git 
- **NEW TOOL GITHACK**
	- python GitHack-master/GitHack.py http://dev.linkvortex.htb/.git/ | grep -v "File not found"
		- we see 
			- [OK] Dockerfile.ghost
			- [OK] ghost/core/test/regression/api/admin/authentication.test.js
		- We check authentication.test.js
			- we find a password OctopiFociPilfer45
			- we know user is admin and since site is linkvortex.htb the email would be admin@linkvortex.htb
			- Login Successful
		- We also check Dockerfile.ghost
			- under copy the config we see a file path /var/lib/ghost/config.production.json
- Going back to CVE we exploit it with the credentials we have now obtianed.
	- we can now check files there
		- Checked /etc/passwd and /etc/shadow..nothing important
		- from the Dockerfile.ghost we enter that path /var/lib/ghost/config.production.json
			- We find a user and password
				- bob@linkvortex.htb
				- fibber-talented-worth
- ssh into machine with these creds. Get user.txt
	- sudo -l
		- /usr/bin/bash /opt/ghost/clean_symlink.sh *.png
	- We check file content of /opt/ghost/clean_symlink.sh
		- The main part of the code states if CHECK_CONTENT is not false and the links are valid we can see the contents of the png file
	- **Now we need to create a link to to get the final flag**
		- I initially tried ln -s /root/root.txt test.png (which is usually where root.txt is stored) followed by sudo CHECK_CONTENT=true (**because thats has to be checked for content to be shown**) /usr/bin/bash /opt/ghost/clean_symlink.sh /home/bob/test.png
			- but it did not show the content.
		- So I then linked /root/root.txt to test.txt
		- and then i linked /home/bob/text.txt (path must be shown) test.png
		- Then I passed the command and the root.txt file content was shown
			- sudo CHECK_CONTENT=true  /usr/bin/bash /opt/ghost/clean_symlink.sh /home/bob/test.png

