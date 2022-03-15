DAY 21 - TIME FOR SOME ELFORENSICS [BLUE TEAMING]
----

# STORY :
----

>>One of the 'little helpers' logged into his workstation only to realize that the database connector file has been replaced, and he can't find the naughty list anymore. Furthermore, upon executing the database connector file, a taunting message was displayed, hinting that the file was moved to another location.

>>McEager has been notified, and he will put the pieces together to find the database connector file.

>>Task: Find where the database connector file is hidden using forensic-like investigative techniques.

----

# CREDS :
----

>>For Server provide (MACHINE_IP) as the IP address provided to you for the remote machine. The credentials for the user account is:
   * User name: littlehelper
   * User password: iLove5now!

----

# TASKS :
----

>>We will continue our journey with Powershell. With Powershell, we can obtain file hashes of files on the endpoint.

>>A file hash, or simply a hash, is a mathematical algorithm that analyzes the data of the file and outputs a value, which is its hash.  File hashes let us know whether a file is legitimate or not based on its verified file hash. If the file has been replaced or altered, the file hash will be different. There are exceptions to this rule, but we will not dive into that. For now, it's safe to know that a file hash acts like a signature for a file.

>>With PowerShell, we can obtain the hash of a file by running the following command: Get-FileHash -Algorithm MD5 file.txt

>>By comparing the verified file hash to the above cmdlet's output, you will know whether the file is authentic.

>>At this point, you should be confident that the file sitting in the Documents folder is not legitimate. If you run the file, you can see that not much information is given, only the hint that the original file was moved to another location within the endpoint.

>>Another tool you can use to inspect within a binary file (.exe) is Strings.exe. Strings scans the file you pass it for strings of a default length of 3 or more characters. You can use the Strings tool to peek inside this mysterious executable file. The tool is located within C:\Tools.

>>The command to run for the Strings tool to scan the mysterious executable: c:\Tools\strings64.exe -accepteula file.exe

>>In the output, you should notice a command related to ADS. You know this by the end of the Powershell command -Stream.

>>Alternate Data Streams (ADS) is a file attribute specific to Windows NTFS (New Technology File System). Every file has at least one data stream ($DATA) and ADS allows files to contain more than one stream of data. Natively Window Explorer doesn't display ADS to the user. There are 3rd party executables that can be used to view this data, but Powershell gives you the ability to view ADS for files.

>>Malware writers have used ADS to hide data in an endpoint, but not all its uses are malicious. When you download a file from the Internet unto an endpoint there are identifiers written to ADS to identify that it was downloaded from the Internet.

>>The command to view ADS using Powershell: Get-Item -Path file.exe -Stream *

>>There are a few lines of output when you run this command. Pay particularly close attention to Stream and Length.

>>Recall that the database connector file is an executable file, and it's hidden within an alternate data stream for another file. We can use a built-in Windows tool, Windows Management Instrumentation, to launch the hidden file.

>>The command to run to launch the hidden executable hiding within ADS: wmic process call create $(Resolve-Path file.exe:streamname)

>>Note: You must replace file.exe with the actual name of the file which contains the ADS, and streamname is the actual name of the stream displayed in the output.>>

----

# MY DOCUMENTATION :
-----

>>GO TO THE DOCUMENTS FOLDER AND LIST THE CONTENTS THERE WE HAVE 2 FILES ONE IS A TEXT FILE AND ANOTHER IS THE EXECUTABLE.
>>TO GET THE HASH VALUE OF THE FIRST FILE USE "Get-Content" TO PRINT THE VALUE.
```

PS C:\Users\littlehelper> cd .\Documents\
PS C:\Users\littlehelper\Documents> ls


    Directory: C:\Users\littlehelper\Documents


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----       11/23/2020  11:21 AM             63 db file hash.txt
-a----       11/23/2020  11:22 AM           5632 deebee.exe

PS C:\Users\littlehelper\Documents> Get-Content '.\db file hash.txt'
Filename:       db.exe
MD5 Hash:       596690FFC54AB6101932856E6A78E3A1
```

>>TO GET THE HASH VALUE OF THE EXECUTABLE USE "Get-FileHash" TO GET HASH FILE OF THE EXE AND SET THE ALGORITHM VALUE TO MDS WITH THESE COMMAND "-Algorithm MD5" AND THE NAME OF THE 2 FILE.
```
PS C:\Users\littlehelper\Documents> Get-FileHash -Algorithm MD5 .\deebee.exe

Algorithm       Hash                                                                   Path
---------       ----                                                                   ----
MD5             5F037501FB542AD2D9B06EB12AED09F0                                       C:\Users\littlehelper\Documen...
```

>>IF WE RUN THAT EXECUTABLE FILE IT SAYS "NOTHING IS HERE".
>>TO GET THE HIDDEN STRINGS IN THAT FILE USE THE TOOL "STRINGS64.EXE" WHICH IS LOCATED IN THE TOOLS FOLDER.
>>TO USE THAT TOOL USE THIS COMMANDS :
  1.NAVIGATE TO THE "C:\Tools\" FOLDER AND IN THAT GO TO TOOLS.
  2.THEN USE THE STRINGS64.EXE AND USE PARAMETER TO ACCEPT AND EXECUTE IT WITH "TARGET FILE NAME"."C:\Tools\stringd64.exe -accepteula ./filename.exe"
>>WITH THIS WE CAN INSPECT THE STRINGS IN THAT EXECUTABLE.   
```
PS C:\Users\littlehelper\Documents> C:\Tools\strings64.exe -accepteula .\deebee.exe

Strings v2.53 - Search for ANSI and Unicode strings in binary images.
Copyright (C) 1999-2016 Mark Russinovich
Sysinternals - www.sysinternals.com

!This program cannot be run in DOS mode.
SLH
.text
`.rsrc
@.reloc
&*"
BSJB
v4.0.30319
#Strings
#US
#GUID
#Blob
c.#l.+x.3x.;x.Cl.K~.Sx.[x.c
<Module>
mscorlib
Thread
deebee
Console
ReadLine
WriteLine
Write
GuidAttribute
DebuggableAttribute
ComVisibleAttribute
AssemblyTitleAttribute
AssemblyTrademarkAttribute
TargetFrameworkAttribute
AssemblyFileVersionAttribute
AssemblyConfigurationAttribute
AssemblyDescriptionAttribute
CompilationRelaxationsAttribute
AssemblyProductAttribute
AssemblyCopyrightAttribute
AssemblyCompanyAttribute
RuntimeCompatibilityAttribute
deebee.exe
System.Threading
System.Runtime.Versioning
Program
System
Main
System.Reflection
Sleep
Clear
.ctor
System.Diagnostics
System.Runtime.InteropServices
System.Runtime.CompilerServices
DebuggingModes
args
Object
Accessing the Best Festival Company Database...
Done.
Using SSO to log in user...
Loading menu, standby...
THM{f6187e6cbeb1214139ef313e108cb6f9}
Set-Content -Path .\lists.exe -value $(Get-Content $(Get-Command C:\Users\littlehelper\Documents\db.exe).Path -ReadCount 0 -Encoding Byte) -Encoding Byte -Stream hidedb
Hahaha .. guess what?
Your database connector file has been moved and you'll never find it!
I guess you can't query the naughty list anymore!
>;^P
z\V
WrapNonExceptionThrows
deebee
Copyright
  2020
$c8374a1e-384f-4cf2-b8c0-81f74ec36ab2
1.0.0.0
.NETFramework,Version=v4.0
FrameworkDisplayName
.NET Framework 4
RSDS
*fF
J:\code\aoc\deebee\deebee\obj\Debug\deebee.pdb
_CorExeMain
mscoree.dll
VS_VERSION_INFO
VarFileInfo
Translation
StringFileInfo
000004b0
Comments
CompanyName
FileDescription
deebee
FileVersion
1.0.0.0
InternalName
deebee.exe
LegalCopyright
Copyright
  2020
LegalTrademarks
OriginalFilename
deebee.exe
ProductName
deebee
ProductVersion
1.0.0.0
Assembly Version
1.0.0.0
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<assembly xmlns="urn:schemas-microsoft-com:asm.v1" manifestVersion="1.0">
  <assemblyIdentity version="1.0.0.0" name="MyApplication.app"/>
  <trustInfo xmlns="urn:schemas-microsoft-com:asm.v2">
    <security>
      <requestedPrivileges xmlns="urn:schemas-microsoft-com:asm.v3">
        <requestedExecutionLevel level="asInvoker" uiAccess="false"/>
      </requestedPrivileges>
    </security>
  </trustInfo>
</assembly>
```

>>ADS IS THE FILE ATTRIBUTE SPECIFIC TO WINDOWS NTFS(NEW TECHNOLOGY FILE SYSTEM).ADS ALLOW FILES CONTAINS MORE THAN ONE DATA STREAM OF DATA.MALWARE WRITERS USE ADS TO HIDE DATA IN AN ENDPOINT. 
>>TO VIEW THE ADS(ALTERNATE DATA STREAM) DATA IN POWERSHELL USE "Get-Item" TO GET THE ITEM AND SPECIFY THE PARAMETER PATH "-Path" AND SPECIFY THE FILE NAME ".\Filename.exe" AND SPECIFY THE STREAM PARAMETER "-Stream" AND REGEX " * " TO SELECT ALL INFORMATION IN ALL DATA STREAMS.
" Get-Item -Path .\Filename.exe -Stream * "  
>>THERE IS A STREAM "hidedb" AND LENGTH "6144".

```
PS C:\Users\littlehelper\Documents> Get-Item -Path .\deebee.exe -Stream *

PSPath        : Microsoft.PowerShell.Core\FileSystem::C:\Users\littlehelper\Documents\deebee.exe::$DATA
PSParentPath  : Microsoft.PowerShell.Core\FileSystem::C:\Users\littlehelper\Documents
PSChildName   : deebee.exe::$DATA
PSDrive       : C
PSProvider    : Microsoft.PowerShell.Core\FileSystem
PSIsContainer : False
FileName      : C:\Users\littlehelper\Documents\deebee.exe
Stream        : :$DATA
Length        : 5632

PSPath        : Microsoft.PowerShell.Core\FileSystem::C:\Users\littlehelper\Documents\deebee.exe:hidedb
PSParentPath  : Microsoft.PowerShell.Core\FileSystem::C:\Users\littlehelper\Documents
PSChildName   : deebee.exe:hidedb
PSDrive       : C
PSProvider    : Microsoft.PowerShell.Core\FileSystem
PSIsContainer : False
FileName      : C:\Users\littlehelper\Documents\deebee.exe
Stream        : hidedb
Length        : 6144
```

>>THE DATABASE CONNECTOR FILE IS AN EXECUTABLE AND IT IS HIDDEN WITHIN AN ALTERNATE DATA STREAM FOR ANOTHER FILE.WE CAN USE THE WINDOWS BUILT-IN TOOL CALLED "WINDOWS MANAGEMENT INSTRUMENTATION" TO LAUNCH THE HIDDEN FILE.
>>USE THIS COMMAND TO RUN AND LAUNCH THE HIDDEN EXECUTABLE HIDING WITHIN THE ADS.
  1."wmic" - THE TOOL NAME
  2."process" - PROCESS THE TOOL
  3."call" - CALL THE TOOL
  4."create" - CREATE THE PATH TO EXECUTE
  5."Resolve-Path" - RESOLVE THIS PATH OF THE FILENAME
  6.".\Filename" - SPECIFY THE FILENAME
  7."streamname" - SPECIFY THE STREAMNAME
>>"wmic process call create $(Resolve-Path .\Filename:streamname)"

```
PS C:\Users\littlehelper\Documents> wmic process call create $(Resolve-Path .\deebee.exe:hidedb)
Executing (Win32_Process)->Create()
Method execution successful.
Out Parameters:
instance of __PARAMETERS
{
        ProcessId = 1196;
        ReturnValue = 0;
};
```

>>THIS IS THE HIDDEN FILE IN THE ADS.
>>THERE WE GOT OUR FLAG.
```
Choose an option:
1) Nice List
2) Naughty List
3) Exit

THM{088731ddc7b9fdeccaed982b07c297c}

Select an option:

```

----

# ANSWER THE FOLLOWING :
----

Read the contents of the text file within the Documents folder. What is the file hash for db.exe?
>>596690FFC54AB6101932856E6A78E3A1

What is the file hash of the mysterious executable within the Documents folder?
>>5F037501FB542AD2D9B06EB12AED09F0

Using Strings find the hidden flag within the executable?
>>THM{f6187e6cbeb1214139ef313e108cb6f9}

What is the flag that is displayed when you run the database connector file?
>>THM{3088731ddc7b9fdeccaed982b07c297c}

----