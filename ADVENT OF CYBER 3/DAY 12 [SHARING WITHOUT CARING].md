# DAY 12 - SHARING WITHOUT CARING [NETWORKING]  
----

# STORY:
----

>>Grinch Enterprises has been leaving traces of how their hackers have been accessing data from the system - you’ve found a unique server they use. We need your help to find out what method they’ve been using to extract any data.

We have noticed that MACHINE_IP is generating unusual traffic. We highly suspect that Grinch Enterprises are using it to access our data. We will use Nmap to discover the services are running on their server.

----

# TASKS :
----

1.Scan the target server with the IP MACHINE_IP. Remember that MS Windows hosts block pings by default, so we need to add -Pn, for example, nmap -Pn MACHINE_IP for the scan to work correctly. How many TCP ports are open?
>>7

Network File System (NFS) is a protocol that allows the ability to transfer files between different computers and is available on many systems, including MS Windows and Linux. Consequently, NFS makes it easy to share files between various operating systems.

2.In the scan results you received earlier, you should be able to spot NFS or mountd, depending on whether you used the -sV option with Nmap or not. Which port is detected by Nmap as NFS or using the mountd service?
>>2049

Now that we have discovered an NFS service is listening, let’s check what files are being shared. We can do this using the command showmount. In the terminal below, we run showmount -e MACHINE_IP. The -e or --exports show the NFS server’s export list.
Pentester Terminal

```           
pentester@TryHackMe$ showmount -e MACHINE_IP
Export list for MACHINE_IP:
/share        (everyone)
/my-notes     (noone)
```        

As we can see in the terminal output above, we have two shares, /share and /my-notes. After you have started the attached machine, use the AttackBox terminal to discover the shares on MACHINE_IP.

3.How many shares did you find?
>>4

4.How many shares show “everyone”?
>>3

Let’s try to mount the shares we have discovered. We can create a directory on the AttackBox using mkdir tmp1, where tmp1 is the directory’s name. Then we can use this directory  we created to mount the public NFS share using: mount MACHINE_IP:/my-notes tmp1.
Pentester Terminal

```           
pentester@TryHackMe$ mount MACHINE_IP:/my-notes tmp1
mount.nfs: access denied by server while mounting MACHINE_IP:/my-notes
```
We can see that the mounting has failed. my-notes is not public and requires specific authentication mechanisms that we don’t have access to. Let’s try again with the other folder, share.
```
Pentester Terminal
           
pentester@TryHackMe$ mount MACHINE_IP:/share tmp1
```
We didn’t get any error messages, so it was a success. Let’s go inside the share to see what’s inside it using cd tmp1, then ls.
```
Pentester Terminal           
pentester@TryHackMe$ ls
132-0.txt  2680-0.txt
```
There are two text files. We can open the file using any text editor such as nano FILENAME or something quicker such as less FILENAME.

5.What is the title of file 2680-0.txt?
>>meditation

6.It seems that Grinch Enterprises has forgotten their SSH keys on our system. One of the shares contains a private key used for SSH authentication (id_rsa). What is the name of the share?
>>/confidential

7.We can calculate the MD5 sum of a file using md5sum FILENAME. What is the MD5 sum of id_rsa?
>>3e2d315a38f377f304f5598dc2f044de

----