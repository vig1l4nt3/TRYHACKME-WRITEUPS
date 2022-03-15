DAY 20 - POWERSHELLF TO THE RESCUE [BLUE TEAMING]
----

# STORY :
----
Someone is mischievous at The Best Festival Company. The contents within the stockings have been removed. A clue was left in one of the stockings that hints that the contents have been hidden within Elfstation1. McEager moves quickly and attempts to RDP into the machine. Yikes! He is unable to log in.

Luckily, he has been learning PowerShell, and he can remote into the workstation using PowerShell over SSH.

Task: Use the PowerShell console to navigate throughout the endpoint to find the hidden contents to reveal what was hidden in the stockings.

----

# TASKS :
----

>>You will use SSH to connect to the remote machine.

>>The command to run to connect to the remote machine: ssh -l mceager MACHINE_IP

>>Note that your IP address will be different. When prompted, enter the password: r0ckStar!

>>If you logged in successfully, you will see the following prompt.

>>Before we begin, launch PowerShell and navigate to the Documents folder.

>>Note: The virtual machine may take up to 3 minutes to load.

----

# BASICS OF POWERSHELL :
----

>>The official explanation of PowerShell is: "PowerShell is a cross-platform task automation and configuration management framework, consisting of a command-line shell and scripting language. Unlike most shells, which accept and return text, PowerShell is built on top of the .NET Common Language Runtime (CLR), and accepts and returns .NET objects. This fundamental change brings entirely new tools and methods for automation."

>>PowerShell has grown in popularity in the last few years among defenders and especially attackers. Knowing PowerShell is a necessary skill. If you have only heard of PowerShell but never dabbled with it, fret not, today you will.

>>Recall from the definition above that PowerShell is a command-line shell. We must enter commands into the command prompt to instruct PowerShell on what we want it to do for us. PowerShell commands are known as cmdlets.

>>To list the contents of the current directory we are in, we can use the Get-ChildItem cmdlet.  There are various other options we can use with this cmdlet to enhance its capabilities further.

  * -Path Specifies a path to one or more locations. Wildcards are accepted.
  * -File / -Directory To get a list of files, use the File parameter. To get a list of directories, use the Directory parameter. You can use the Recurse parameter with File and/or Directory parameters.
  * -Filter Specifies a filter to qualify the Path parameter.
  * -Recurse Gets the items in the specified locations and in all child items of the locations.
  * -Hidden To get only hidden items, use the Hidden parameter.
  * -ErrorAction SilentlyContinue Specifies what action to take if the command encounters an error.

>>For example, if you want to view all of the hidden files in the current directory you are in, you can issue the following command: Get-ChildItem -File -Hidden -ErrorAction SilentlyContinue

>>Another useful cmdlet is Get-Content. This will allow you to read the contents of a file.

>>You can run this command as follows: Get-Content -Path file.txt

>>You can run numerous operations with the Get-Content cmdlet to give you more information about the particular file you are inspecting. Such as how many words are in the file and the exact positions for a particular string within a file.

>>To get the number of words contained within a file, you can use the Get-Content cmdlet and pipe the results to the Measure-Object cmdlet.

>>You run this command as follows: Get-Content -Path file.txt | Measure-Object -Word

>>To get the exact position of a string within the file, you can use the following command:  (Get-Content -Path file.txt)[index]

>>The index is the numerical value that is the location of the string within the file. Since indexes start at zero, you typically need to subtract one from the original value to extract the string at the correct position. This is not necessary for this exercise.

>>To change directories, you can use the Set-Location cmdlet.

>>For example, Set-Location -Path c:\users\administrator\desktop  will change your location to the Administrator's desktop.

>>The last cmdlet that is needed to solve this room is Select-String. This cmdlet will search a particular file for a pattern you define within the command to run.

>>An example execution of this command is: Select-String -Path 'c:\users\administrator\desktop' -Pattern '* .pdf'

>>Note: You can always use the Get-Help cmdlet to obtain more information about a specific cmdlet. For example, Get-Help Select-String

----

# MY DOCUMENTATION :
----

>>CONNECTING TO THE MACHINE WITH ssh AND ACCESSING powershell.

```bash
root@ip-10-10-117-63:~# ssh -l mceager 10.10.15.71
The authenticity of host '10.10.15.71 (10.10.15.71)' can't be established.
ECDSA key fingerprint is SHA256:El2kBbgMNy7A+isJWuKJ0ZAdILuM5VC7KY9lnSBmoiU.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '10.10.15.71' (ECDSA) to the list of known hosts.
mceager@10.10.15.71's password: 
Microsoft Windows [Version 10.0.17763.737] 
(c) 2018 Microsoft Corporation. All rights reserved. 

mceager@ELFSTATION1 C:\Users\mceager>powershell
Windows PowerShell  
Copyright (C) Microsoft Corporation. All rights reserved.

PS C:\Users\mceager> 
```

>>TO GET THE FIRST FILE GO TO THE DOCUMENTS DIRECTORY AND CHECK FOR THE HIDDEN FILE WITH "-Hidden", "-File" AND CAT THE FILE CONTENTS.

```bash
PS C:\Users\mceager> cd .\Documents\

PS C:\Users\mceager\Documents> ls -File -Hidden


    Directory: C:\Users\mceager\Documents


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a-hs-        12/7/2020  10:29 AM            402 desktop.ini
-arh--       11/18/2020   5:05 PM             35 e1fone.txt

PS C:\Users\mceager\Documents> cat .\e1fone.txt
All I want is my '2 front teeth'!!! 
```

>>GO TO THE DESKTOP DIRECTORY TO GET THE SECOND FILE AND SEARCH FOR THE HIDDEN FILE. IN THIS WE GOT A DIECTORY .GO TO THAT DIRECTORY LIST THE CONTENTS CAT THE TEXT FILE.

```bash
PS C:\Users\mceager\Documents> cd ..
PS C:\Users\mceager> cd .\Desktop\

PS C:\Users\mceager\Desktop> ls -Hidden


    Directory: C:\Users\mceager\Desktop


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d--h--        12/7/2020  11:26 AM                elf2wo
-a-hs-        12/7/2020  10:29 AM            282 desktop.ini


PS C:\Users\mceager\Desktop>  

PS C:\Users\mceager\Desktop> cd .\elf2wo\ 
PS C:\Users\mceager\Desktop\elf2wo> ls 


    Directory: C:\Users\mceager\Desktop\elf2wo


Mode                LastWriteTime         Length Name                
----                -------------         ------ ----                
-a----       11/17/2020  10:26 AM             64 e70smsW10Y4k.txt    


PS C:\Users\mceager\Desktop\elf2wo> cat .\e70smsW10Y4k.txt 
I want the movie Scrooged <3! 
```

>>GO TO THE WINDOWS DIRECTORY TO GET THE THIRD FILE IF WE LIST THE CONTENTS NOTHING LIKE THE FILES MENTIONED WAS THERE.
>>I THINK IN MANY WAYS THE MAIN OR IMPORTANT FILES WERE STORED IN SYSTEM32 FOLDER.GO TO THE LOCATION AND LIST THE HIDDEN FILES THERE WE GOT THE DIRECTORY .
>>GO TO THAT LIST THAT CONTENTS THERE WE HAVE 2 TEXT FILES WHICH HAS LOT OF CONTENT.
>>TO KNOW HOW MANY WORDS WERE IN THE 1.txt USE THIS COMMAND "Measure-Object -Word" TO GET THE TOTAL WORDS IN THE FILE.
>>NEXT TO GET THE SPECIFIC VALUE IN THE GIVEN INDEX NUMBERS USE THIS SQUARE BRACKETS "[]" TO SPECIFY THE INDEX VALUE.
>>THEN GO TO THE 2.txt FILE TO GET THE FINAL ANSWER THIS,HERE WE NEED TO GET A SPECIFIC STRING IN THE FILE SO USE "Select-String -Pattern 'VALUE_HERE'" THIS COMMAND TO SPECIFY THE PATTERN AND VALUE TO GET THE ANSWER.

```bash
PS C:\Users\mceager> cd C:\Windows\ 
PS C:\Windows> ls 


    Directory: C:\Windows


Mode                LastWriteTime         Length Name                
----                -------------         ------ ----                
d-----        9/15/2018  12:19 AM                ADFS                
d-----        9/15/2018  12:19 AM                appcompat           
d-----         9/6/2019   5:31 PM                apppatch            
d-----        12/7/2020  10:50 AM                AppReadiness        
d-r---        9/15/2018   2:11 AM                assembly            
d-----        9/15/2018  12:19 AM                bcastdvr            
d-----        9/15/2018  12:19 AM                Boot                
d-----        9/15/2018  12:19 AM                Branding            
d-----        12/7/2020  11:16 AM                CbsTemp             
d-----        9/15/2018  12:19 AM                Containers          
d-----        9/15/2018  12:19 AM                Cursors             
d-----       11/23/2020   1:43 PM                debug               
d-----        9/15/2018  12:19 AM                diagnostics         
d-----        9/15/2018   2:08 AM                DigitalLocker       
d---s-        9/15/2018  12:19 AM                Downloaded Program  
                                                 Files
d-----        9/15/2018  12:19 AM                drivers
d-----        9/15/2018   2:08 AM                en-US
d-r-s-         9/6/2019   5:31 PM                Fonts
d-----        9/15/2018  12:19 AM                Globalization       
d-----        9/15/2018   2:08 AM                Help
d-----        9/15/2018  12:19 AM                IdentityCRL
d-----        9/15/2018   2:08 AM                IME
d-r---       11/23/2020   1:42 PM                ImmersiveControlPan 
                                                 el
d-----        1/21/2021   7:05 AM                INF
d-----        9/15/2018  12:19 AM                InputMethod
d-----        9/15/2018  12:19 AM                L2Schemas
d-----        9/15/2018  12:19 AM                LiveKernelReports   
d-----        12/3/2020   1:02 PM                Logs
d-r-s-        9/15/2018  12:19 AM                media
d-r---       11/20/2021   5:34 AM                Microsoft.NET       
d-----        9/15/2018  12:19 AM                Migration
d-----        9/15/2018  12:19 AM                ModemLogs
d-----        9/15/2018   2:09 AM                OCR
d-r---        9/15/2018  12:19 AM                Offline Web Pages   
d-----       11/23/2020   1:42 PM                Panther
d-----        9/15/2018  12:19 AM                Performance
d-----        9/15/2018  12:19 AM                PLA
d-----         9/6/2019   5:31 PM                PolicyDefinitions   
d-----       11/23/2020   1:42 PM                Prefetch
d-r---       11/23/2020   1:42 PM                PrintDialog
d-----        9/15/2018  12:19 AM                Provisioning        
d-----       11/25/2020   4:42 PM                Registration        
d-----        9/15/2018  12:19 AM                RemotePackages      
d-----        9/15/2018  12:19 AM                rescache
d-----        9/15/2018  12:19 AM                Resources
d-----        9/15/2018  12:19 AM                SchCache
d-----        9/15/2018  12:19 AM                schemas
d-----        9/15/2018  12:19 AM                security
d-----       11/23/2020   1:42 PM                ServiceProfiles     
d-----        9/15/2018  12:19 AM                ServiceState        
d-----        9/15/2018   2:09 AM                servicing
d-----        9/15/2018  12:21 AM                Setup
d-----         9/6/2019   5:31 PM                ShellComponents     
d-----         9/6/2019   5:31 PM                ShellExperiences    
d-----        9/15/2018  12:19 AM                SKB
d-----       11/23/2020   1:43 PM                SoftwareDistributio 
                                                 n
d-----        9/15/2018  12:19 AM                Speech
d-----        9/15/2018  12:19 AM                Speech_OneCore      
d-----        9/15/2018  12:19 AM                System
d-----       11/20/2021   5:30 AM                System32
d-----        9/15/2018  12:19 AM                SystemApps
d-----        9/15/2018  12:19 AM                SystemResources     
d-----       11/23/2020   1:45 PM                SysWOW64
d-----        9/15/2018  12:19 AM                TAPI
d-----       11/23/2020   1:42 PM                Tasks
d-----       11/20/2021   5:35 AM                Temp
d-----         9/6/2019   5:31 PM                TextInput
d-----        9/15/2018  12:19 AM                tracing
d-----        9/15/2018  12:19 AM                twain_32
d-----        9/15/2018  12:19 AM                Vss
d-----        9/15/2018  12:19 AM                WaaS
d-----        9/15/2018  12:19 AM                Web
d-----        12/7/2020  11:16 AM                WinSxS
-a----        9/15/2018  12:12 AM          78848 bfsvc.exe
-a--s-       11/20/2021   5:24 AM          67584 bootstat.dat        
-a----         9/6/2019   5:28 PM         232960 DfsrAdmin.exe       
-a----         9/6/2019   5:30 PM           1315 DfsrAdmin.exe.confi 
                                                 g
-a----         9/6/2019   5:28 PM        4353016 explorer.exe        
-a----         9/6/2019   5:29 PM        1071616 HelpPane.exe        
-a----        9/15/2018  12:12 AM          18432 hh.exe
-a----       11/23/2020   1:42 PM           1378 lsasetup.log        
-a----        9/15/2018  12:12 AM          43131 mib.bin
-a----         9/6/2019   5:29 PM         254464 notepad.exe
-a----       11/20/2021   5:20 AM          26250 PFRO.log
-a----       11/26/2020  11:32 AM          87616 PSSDNSVC.EXE        
-a----         9/6/2019   5:29 PM         358400 regedit.exe
-a----        9/15/2018  12:13 AM          30931 ServerStandard.xml  
-a----        9/15/2018  12:13 AM          30914 ServerStandardEval. 
-a----         9/6/2019   5:28 PM         132608 splwow64.exe        
-a----        9/15/2018  12:16 AM            219 system.ini
-a----        9/15/2018  12:13 AM          64512 twain_32.dll        
-a----        9/15/2018  12:16 AM             92 win.ini
-a----       11/20/2021   5:24 AM            276 WindowsUpdate.log   
-a----        9/15/2018  12:13 AM          11776 winhlp32.exe        
-a----        9/15/2018  12:12 AM         316640 WMSysPr9.prx        
-a----        9/15/2018  12:12 AM          11264 write.exe


PS C:\Windows> ls -Hidden  -Directory
 
    Directory: C:\Windows


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d--h--        9/15/2018  12:19 AM                ELAMBKUP
d--hs-       11/26/2020  11:32 AM                Installer

PS C:\Windows> ls -Hidden


    Directory: C:\Windows


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d--h--        9/15/2018  12:19 AM                ELAMBKUP
d--hs-       11/26/2020  11:32 AM                Installer
-arh--        9/15/2018  12:12 AM            670 WindowsShell.Manife 
                                                 st


PS C:\Windows>

PS C:\Windows> cd .\System32\ 
PS C:\Windows\System32>  
PS C:\Windows\System32> ls -Hidden -Directory 


    Directory: C:\Windows\System32


Mode                LastWriteTime         Length Name                
----                -------------         ------ ----                
d--h--       11/23/2020   3:26 PM                3lfthr3e            
d--h--       11/23/2020   2:26 PM                GroupPolicy         


PS C:\Windows\System32> cd .\3lfthr3e\ 
PS C:\Windows\System32\3lfthr3e> ls 
PS C:\Windows\System32\3lfthr3e> ls -Hidden 


    Directory: C:\Windows\System32\3lfthr3e 


Mode                LastWriteTime         Length Name                
----                -------------         ------ ----                
-arh--       11/17/2020  10:58 AM          85887 1.txt               
-arh--       11/23/2020   3:26 PM       12061168 2.txt


PS C:\Windows\System32\3lfthr3e> Get-Content -Path 1.txt | Measure-Object -Word
Lines Words Characters Property
----- ----- ---------- --------       
	  9999

PS C:\Windows\System32\3lfthr3e> (Get-Content -Path 1.txt)[551,6991] 
Red 
Ryder	  

PS C:\Windows\System32\3lfthr3e> Get-Content 2.txt | Select-String -Pattern 'redryder'

redryderbbgun 
```

----

# ANSWER THE FOLLOWING :
----

1.Search for the first hidden elf file within the Documents folder. Read the contents of this file. What does Elf 1 want?
>>2 front teeth

2.Search on the desktop for a hidden folder that contains the file for Elf 2. Read the contents of this file. What is the name of that movie that Elf 2 wants?
>>Scrooged

3.Search the Windows directory for a hidden folder that contains files for Elf 3. What is the name of the hidden folder? (This command will take a while)
>>3lfthr3e 

4.How many words does the first file contain?
>>9999

5.What 2 words are at index 551 and 6991 in the first file?
>>red ryder

6.This is only half the answer. Search in the 2nd file for the phrase from the previous question to get the full answer. What does Elf 3 want? (use spaces when submitting the answer)
>>red ryder bb gun

----