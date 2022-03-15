DAY 10 - DON'T BE SELFISH [NETWORKING]
----

# STORY :
----

>>The Best Festival Company (TBFC) has since upscaled its IT infrastructure after last year's attack for all the other elves to use, including a VPN server and a few other services. You breathe a sigh of relief..."That's it, Me, Elf McEager saved the Christmas of 2020! I can't wait to---"

>>But suddenly, a cold shiver runs down your spine, interrupting your monologue...

>>You suddenly recall that Elf McSkidy had set up a Samba file server just before the attack occurred - could this have been hacked too?!  What about our data...Oh no, quick! Find out what usernames may have been leaked and attempt to login to the server yourself, noting down any vulnerabilities found to report back to Elf McSkidy.

----

# Today's Learning Objectives:
----
>>Learn about the basics of network file sharing protocols before getting hands-on with Samba, where you will be enumerating "tbfc-smb-01" : a vulnerable Samba server to gain un-authorised access.

----

# What is Samba & where is it Used?
----

>>Whilst we learnt about one of the most commonplace protocols that are used for file-sharing on Day 10, we'll be covering an alternative technology for file-sharing that is most used within organisation/company networks. 
>>Offering encryption as standard, this technology consists of two protocols:
  * SMB (Server Message Block) - Natively supported by Windows and not Linux
  * NFS (Network File System) - Natively supported by Linux and not Windows

>>Protocols such as SMB send "requests" and "responses" when communicating with each other, as illustrated below:

>>What makes Samba so popular and useful is that it removes the differences between these two protocols, meaning that the two operating systems can now share resources including files amongst each other. 
>>Simply, Samba connects to a "share" (think of this as a virtual folder) and is capable of day-to-day activities like deleting, moving or uploading files.

>>Samba is flexible in the sense it can be useful for both you and me or businesses with thousands of employees. 
>>For example, employees can access documents from a central computer rather than each employee storing their own copy. 
>>As previously mentioned, this technology is encrypted enabling sensitive data like username and passwords used in the authentication process (and the data itself)  to be communicated between client/server securely.

>>Unlike FTP, other IT devices such as network printers can also be shared between client/server.

----

# Searching for Samba Shares
----

>>We're going to be using the enum4linux tool that is already provided to you on the THM AttackBox. Let's get our hands dirty!

   * Open a terminal prompt and navigate to enum4linux: 
    cd /root/Desktop/Tools/Miscellaneous
   * Run enum4linux and list all the possible options we could use, take time to study these for anything interesting:  
    ./enum4linux.pl -h

>>Note how we can use options like -S to list shares or -U (note the uppercase) to list possible users. 
>>In my example, I want to find out who can be used to access the server through Samba: ./enum4linux.pl -U MACHINE_IP

>>Note how enum4linux has discovered four users in my example...One of these users may have a weak password such as "password123" that we can log in with and access sensitive data as.
1. jjohns
2. lbutton
3. jfrost
4. cmnatic

>>And as a result of further enumeration with enum4linux, we've discovered the following three shares!
1. homes
2. share1
3. IPC$

>>Now it's your turn, scan your Instance (MACHINE_IP) to answer Question #1 and Question #2. Remember the options that we can use with enum4linux.pl !

----

# Connecting to a Share
----

>>We've already learnt two key pieces of information from the previous section:

  * Usernames to authenticate as
  * Shares that we can access (remembering that shares most likely contain data)

>>However, a very common and easy to cause vulnerability by administrators is wrong permissions. 
>>You may be able to access a share and its data without logging in at all, such as we will demonstrate below:

1. Remember that the IP address of the Samba server is that of the Instance  you deployed (MACHINE_IP)

2. Use the smbclient tool to begin accessing the Samba server and its shares, replacing "sharename" with the name of the share you wish to access: 

smbclient //REPLACE_INSTANCE_IP_ADDRESS/**sharename**

3. You will be asked for a password, the easiest password is no password! We can just press "Enter" to test this theory. If successful, this means that the share requires no authentication and we are now logged in.

>>For example, accessing "share1" on another device:

>>You can use the help command to list some of the commands you can run whilst connected to the Samba share. Here's a quick rundown of the fundamentals:

Command 		Description
1.ls 				List files and directories in the current location
2.cd   <directory> 	Change our working directory
3.pwd 				Output the full path to our working directory
4.more <filename>   Find out more about the contents of a file. To close the open file, you press :q
5.get  <filename> 	Download a file from a share
6.put  <filename> 	Upload a file from a share

----

# Conclusion, where to go from here and additional Material:
----

>>You've learned the fundamentals of how a very commonplace protocol used by computing devices works, and ultimately, can be leveraged through the use of enumeration and misconfiguration. With this said, you might be surprised to learn that even printers can use the protocols behind Samba

----

# MY DOCUMENTATION :
----

>>USING ENUM4LINUX TO ENUMERATE THE GIVEN_IP AND FINDING THE USERLIST[-U] OF THE SHARE.

```bash
root@ip-10-10-13-159:~# ./enum4linux.pl -U 10.10.46.215
WARNING: polenum.py is not in your path.  Check that package is installed and your PATH is sane.
WARNING: ldapsearch is not in your path.  Check that package is installed and your PATH is sane.
Starting enum4linux v0.8.9 ( http://labs.portcullis.co.uk/application/enum4linux/ ) on Sat Nov  6 06:34:06 2021

 ========================== 
|    Target Information    |
 ========================== 
Target ........... 10.10.46.215
RID Range ........ 500-550,1000-1050
Username ......... ''
Password ......... ''
Known Usernames .. administrator, guest, krbtgt, domain admins, root, bin, none


 ==================================================== 
|    Enumerating Workgroup/Domain on 10.10.46.215    |
 ==================================================== 
[+] Got domain/workgroup name: TBFC-SMB-01

 ===================================== 
|    Session Check on 10.10.46.215    |
 ===================================== 
[+] Server 10.10.46.215 allows sessions using username '', password ''

 =========================================== 
|    Getting domain SID for 10.10.46.215    |
 =========================================== 
Domain Name: TBFC-SMB-01
Domain Sid: (NULL SID)
[+] Can't determine if host is part of domain or part of a workgroup

 ============================= 
|    Users on 10.10.46.215    |
 ============================= 
index: 0x1 RID: 0x3e8 acb: 0x00000010 Account: elfmcskidy	Name: 	Desc: 
index: 0x2 RID: 0x3ea acb: 0x00000010 Account: elfmceager	Name: elfmceager	Desc: 
index: 0x3 RID: 0x3e9 acb: 0x00000010 Account: elfmcelferson	Name: 	Desc: 

user:[elfmcskidy] rid:[0x3e8]
user:[elfmceager] rid:[0x3ea]
user:[elfmcelferson] rid:[0x3e9]
enum4linux complete on Sat Nov  6 06:34:11 2021

```

>>FINDING SHARELIST[-S] 

```bash
root@ip-10-10-13-159:~# ./enum4linux.pl -S 10.10.46.215
WARNING: polenum.py is not in your path.  Check that package is installed and your PATH is sane.
WARNING: ldapsearch is not in your path.  Check that package is installed and your PATH is sane.
Starting enum4linux v0.8.9 ( http://labs.portcullis.co.uk/application/enum4linux/ ) on Sat Nov  6 06:36:43 2021

 ========================== 
|    Target Information    |
 ========================== 
Target ........... 10.10.46.215
RID Range ........ 500-550,1000-1050
Username ......... ''
Password ......... ''
Known Usernames .. administrator, guest, krbtgt, domain admins, root, bin, none


 ==================================================== 
|    Enumerating Workgroup/Domain on 10.10.46.215    |
 ==================================================== 
[+] Got domain/workgroup name: TBFC-SMB-01

 ===================================== 
|    Session Check on 10.10.46.215    |
 ===================================== 
[+] Server 10.10.46.215 allows sessions using username '', password ''

 =========================================== 
|    Getting domain SID for 10.10.46.215    |
 =========================================== 
Domain Name: TBFC-SMB-01
Domain Sid: (NULL SID)
[+] Can't determine if host is part of domain or part of a workgroup

 ========================================= 
|    Share Enumeration on 10.10.46.215    |
 ========================================= 
WARNING: The "syslog" option is deprecated

	Sharename       Type      Comment
	---------       ----      -------
	tbfc-hr         Disk      tbfc-hr
	tbfc-it         Disk      tbfc-it
	tbfc-santa      Disk      tbfc-santa
	IPC$            IPC       IPC Service (tbfc-smb server (Samba, Ubuntu))
Reconnecting with SMB1 for workgroup listing.

	Server               Comment
	---------            -------

	Workgroup            Master
	---------            -------
	TBFC-SMB-01          TBFC-SMB

[+] Attempting to map shares on 10.10.46.215
//10.10.46.215/tbfc-hr	Mapping: DENIED, Listing: N/A
//10.10.46.215/tbfc-it	Mapping: DENIED, Listing: N/A
//10.10.46.215/tbfc-santa	Mapping: OK, Listing: OK
//10.10.46.215/IPC$	[E] Can't understand response:
WARNING: The "syslog" option is deprecated
NT_STATUS_OBJECT_NAME_NOT_FOUND listing \*
enum4linux complete on Sat Nov  6 06:36:45 2021
```

>>USING SMBCLIENT TO LOG IN TO SERVER SHARE.NO PASSWORD NEEDED PRESS 'ENTER' AND LIST THE ITEMS.
>>FOUND TWO ITEM.ONE FILE AND ONE DIRECTORY.
>>USE "more" SEE THE CONTENTS IN THE TEXT FILE TO GET THE INFORMATION.
>>WITH THAT INFORMATION CHANGE DIRECTORY TO JINGLE-TUNE.THAT DIRECTORY HAS NOTHING.

```bash
root@ip-10-10-13-159:~# smbclient //10.10.46.215/tbfc-santa
WARNING: The "syslog" option is deprecated
Enter WORKGROUP\root's password: 
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Thu Nov 12 02:12:07 2020
  ..                                  D        0  Thu Nov 12 01:32:21 2020
  jingle-tunes                        D        0  Thu Nov 12 02:10:41 2020
  note_from_mcskidy.txt               N      143  Thu Nov 12 02:12:07 2020

		10252564 blocks of size 1024. 5368116 blocks available

smb: \> more note_from_mcskidy.txt
getting file \note_from_mcskidy.txt of size 143 as /tmp/smbmore.U9Kek2 (46.5 KiloBytes/sec) (average 46.5 KiloBytes/sec)

smb: \> cd jingle-tunes\
smb: \jingle-tunes\> ls
  .                                   D        0  Thu Nov 12 02:10:41 2020
  ..                                  D        0  Thu Nov 12 02:12:07 2020

		10252564 blocks of size 1024. 5368116 blocks available

```
----

# ANSWER THE FOLLOWING :
----

1.Using enum4linux, how many users are there on the Samba server (MACHINE_IP)?
>>3

2.Now how many "shares" are there on the Samba server?
>>4

3.Use smbclient to try to login to the shares on the Samba server (MACHINE_IP). What share doesn't require a password?
>>tbfc-santa

4.Log in to this share, what directory did ElfMcSkidy leave for Santa?
>>jingle-tunes

----