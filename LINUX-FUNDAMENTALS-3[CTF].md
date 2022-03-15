Task 1 : Introduction
----

Let's proceed!
> No need to answer

----

Task 2 : Deploy Your Linux Machine
----

I've logged into the Linux Fundamentals Part 3 machine using SSH and have deployed the 
AttackBox successfully!
> No need to answer

----

Task 3 : Terminal Text Editors
----

Create a file using Nano
> No need to answer

Edit "task3" located in "tryhackme"'s home directory using Nano. What is the flag?
> THM{TEXT_EDITORS}

----

Task 4 :  General/Useful Utilities
----

Ensure you are connected to the deployed instance (10.10.208.192)
> No need to answer 

Now, use Python 3's "HTTPServer" module to start a web server in the home directory of the 
"tryhackme" user on the deployed instance.
> No need to answer

Download the file http://10.10.208.192:8000/.flag.txt onto the TryHackMe AttackBox
What are the contents?
> THM{WGET_WEBSERVER}

Create and download files to further apply your learning -- see how you can read the 
documentation on Python3's "HTTPServer" module. 
Use Ctrl + C to stop the Python3 HTTPServer module once you are finished.
> No need to answer

----

Task 5 : Processes 101
----

Read me!
> No need to answer

If we were to launch a process where the previous ID was "300", what would the ID of this 
new process be?
> 301

If we wanted to cleanly kill a process, what signal would we send it?
> SIGTERM

Locate the process that is running on the deployed instance (10.10.208.192). What flag is 
given?
> THM{PROCESSES}

What command would we use to stop the service "myservice"?
> systemctl stop myservice

What command would we use to start the same service on the boot-up of the system?
> systemctl enable myservice

What command would we use to bring a previously backgrounded process back to the foreground?
> fg

----

Task 6 : Maintaining Your System : Automation
----

Ensure you are connected to the deployed instance and look at the running crontabs.
> No need to answer

When will the crontab on the deployed instance (10.10.208.192) run?
> @reboot

----

Task 7 : Maintaining Your System : Package Management
----

Since TryHackMe instances do not have an internet connection...this task only requires you 
to read through the material.
> No need to answer

----

Task 8 : Maintaining Your System : Logs
----

Look for the apache2 logs on the deployable Linux machine
> No need to answer

What is the IP address of the user who visited the site?
> 10.9.232.111 

What file did they access?
> catsanddogs.jpg

----