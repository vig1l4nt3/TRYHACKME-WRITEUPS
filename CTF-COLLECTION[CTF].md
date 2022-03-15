# Task 1 Author note
----

>>Just another random CTF room created by me. Well, the main objective of the room is to test your CTF skills. For your information, vol.1 consists of 20 tasks and all the challenges are extremely easy. Stay calm and Capture the flag. :)

>>Note: All the challenges flag are formatted as THM{flag}, unless stated otherwise

----

# Task 2 What does the base said?
----

>>Can you decode the following?

>>VEhNe2p1NTdfZDNjMGQzXzdoM19iNDUzfQ==

```bash
┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ echo -n VEhNe2p1NTdfZDNjMGQzXzdoM19iNDUzfQ== | base64 -d
THM{ju57_d3c0d3_7h3_b453}  
```

----

# Task 3 Meta meta
----

>>Meta! meta! meta! meta...................................

```bash
┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ strings Findme.jpg  
JFIF
Exif
0231
0100
THM{3x1f_0r_3x17}
```
----

# Task 4 Mon, are we going to be okay?
----

>>Something is hiding. That's all you need to know.

```bash
┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ steghide --extract -sf Extinction.jpg 
Enter passphrase: 
wrote extracted data to "Final_message.txt".
┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ cat Final_message.txt 
It going to be over soon. Sleep my child.

THM{500n3r_0r_l473r_17_15_0ur_7urn}
```
----

# Task 5 Erm......Magick
----

>>Huh, where is the flag? THM{wh173_fl46}

----

# Task 6 QRrrrr
----

>>Such technology is quite reliable.

```bash
# Go to this 'https://zxing.org/w/decode.jspx' to decode. 
Decode Succeeded
Raw text    

THM{qr_m4k3_l1f3_345y}
```

----

# Task 7 Reverse it or read it?
----

>>Both works, it's all up to you.

```bash
┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ cat hello.hello 
THM{345y_f1nd_345y_60}Hello there, wish you have a nice day
```

----

# Task 8 Another decoding stuff
----

>>Can you decode it?

>>3agrSy1CewF9v8ukcSkPSYm3oKUoByUpKG4L

```bash
┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ echo -n 3agrSy1CewF9v8ukcSkPSYm3oKUoByUpKG4L | base58 -d      
THM{17_h45_l3553r_l3773r5}    
```
----

# Task 9 Left or right
----

>>Left, right, left, right... Rot 13 is too mainstream. Solve this

>>MAF{atbe_max_vtxltk}

```bash

# Go to this link https://gchq.github.io/CyberChef/#recipe=ROT13(true,true,true,33)&input=TUFGe2F0YmVfbWF4X3Z0eGx0a30 to decode , it uses rot33 encryption.
THM{hail_the_caesar}
```

----

# Task 10 Make a comment
----

>>No downloadable file, no ciphered or encoded text. Huh .......

```bash
# Inspect the html and search for this .room-task-desc-data
THM{4lw4y5_ch3ck_7h3_c0m3mn7} 
```
----

# Task 11 Can you fix it?
----

>>I accidentally messed up with this PNG file. Can you help me fix it? Thanks, ^^

```bash
# Corrupted header
┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ xxd spoil.png |head
00000000: 2333 445f 0d0a 1a0a 0000 000d 4948 4452  #3D_........IHDR
00000010: 0000 0320 0000 0320 0806 0000 00db 7006  ... ... ......p.
00000020: 6800 0000 0173 5247 4200 aece 1ce9 0000  h....sRGB.......
00000030: 0009 7048 5973 0000 0ec4 0000 0ec4 0195  ..pHYs..........
00000040: 2b0e 1b00 0020 0049 4441 5478 9cec dd79  +.... .IDATx...y
00000050: 9c9c 559d eff1 cf79 9e5a bb7a 5f92 7477  ..U....y.Z.z_.tw
00000060: f640 4802 0920 1150 c420 bba2 88a8 805c  .@H.. .P. .....\
00000070: 1906 7c5d 64c0 79e9 752e 03ce 38e3 0e8e  ..|]d.y.u...8...
00000080: 2f75 e63a 23ea 8c0c e830 8e03 6470 c191  /u.:#....0..dp..
00000090: cd80 880c 4b20 0909 184c 42b6 4ed2 e9f4  ....K ...LB.N...
          
# Correcting the header                      
┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ printf '\x89\x50\x4E\x47' | dd of=spoil.png bs=4 conv=notrunc
1+0 records in
1+0 records out
4 bytes copied, 0.00230398 s, 1.7 kB/s

# Valig png      
┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ xxd spoil.png |head                                          
00000000: 8950 4e47 0d0a 1a0a 0000 000d 4948 4452  .PNG........IHDR
00000010: 0000 0320 0000 0320 0806 0000 00db 7006  ... ... ......p.
00000020: 6800 0000 0173 5247 4200 aece 1ce9 0000  h....sRGB.......
00000030: 0009 7048 5973 0000 0ec4 0000 0ec4 0195  ..pHYs..........
00000040: 2b0e 1b00 0020 0049 4441 5478 9cec dd79  +.... .IDATx...y
00000050: 9c9c 559d eff1 cf79 9e5a bb7a 5f92 7477  ..U....y.Z.z_.tw
00000060: f640 4802 0920 1150 c420 bba2 88a8 805c  .@H.. .P. .....\
00000070: 1906 7c5d 64c0 79e9 752e 03ce 38e3 0e8e  ..|]d.y.u...8...
00000080: 2f75 e63a 23ea 8c0c e830 8e03 6470 c191  /u.:#....0..dp..
00000090: cd80 880c 4b20 0909 184c 42b6 4ed2 e9f4  ....K ...LB.N...

THM{y35_w3_c4n}
```
----

# Task 12 Read it
----

>>Some hidden flag inside Tryhackme social account.

```bash
# Use this operators on google : site:reddit.com intitle:tryhackme intext:THM
New room Coming soon! : r/tryhackme - Reddit
https://www.reddit.com › comments › eizxaq › new_ro...
02-Jan-2020 — THM{50c14l_4cc0un7_15_p4r7_0f_051n7}
```
----

# Task 13 Spin my head :
----

>>What is this?

++++++++++[>+>+++>+++++++>++++++++++<<<<-]>>>++++++++++++++.------------.+++++.>+++++++++++++++++++++++.<<++++++++++++++++++.>>-------------------.---------.++++++++++++++.++++++++++++.<++++++++++++++++++.+++++++++.<+++.+.>----.>++++.

```bash
# Use this url to decode https://www.splitbrain.org/_static/ook/ this
THM{0h_my_h34d}
```

----

# Task 14 An exclusive!
----

>>Exclusive strings for everyone!

S1: 44585d6b2368737c65252166234f20626d
S2: 1010101010101010101010101010101010

```bash
# Use this url to decode https://xor.pw/# this

I. Input: 44585d6b2368737c65252166234f20626d

II. Input: 1010101010101010101010101010101010

III. Output: THM{3xclu51v3_0r}
```

----

# Task 15 Binary walk
----

>>Please exfiltrate my file :)

```bash
┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ binwalk -e hell.jpg 

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             JPEG image data, JFIF standard 1.02
30            0x1E            TIFF image data, big-endian, offset of first image directory: 8
265845        0x40E75         Zip archive data, at least v2.0 to extract, uncompressed size: 69, name: hello_there.txt
266099        0x40F73         End of Zip archive, footer length: 22
                                                                                                                    
┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ cd _hell.jpg.extracted        
                                                                                                                            
┌[rawbot@pwnbox]─[/home/rawbot/_hell.jpg.extracted]
└╼[★]$ ls
40E75.zip  hello_there.txt
                                                                                                                            
┌[rawbot@pwnbox]─[/home/rawbot/_hell.jpg.extracted]
└╼[★]$ cat hello_there.txt           
Thank you for extracting me, you are the best!

THM{y0u_w4lk_m3_0u7}
```
----

# Task 16 Darkness
----

>>There is something lurking in the dark.

```bash
┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ wget -q https://github.com/eugenekolo/sec-tools/raw/master/stego/stegsolve/stegsolve/stegsolve.jar
                                                                                                                            
┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ ls
cybersecurity  Desktop    Downloads  Music     Public         Templates
dark.png       Documents  gitclones  Pictures  stegsolve.jar  Videos
                                                                                                                            
┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ java -jar stegsolve.jar                          
Picked up _JAVA_OPTIONS: -Dawt.useSystemAAFontSettings=on -Dswing.aatext=true

# Found the flag in the blue plane1
THM{7h3r3_15_h0p3_1n_7h3_d4rkn355}
```
----

# Task 17 A sounding QR
----

>>How good is your listening skill?

P/S: The flag formatted as THM{Listened Flag}, the flag should be in All CAPS

```bash
# Go to this url and upload the file https://zxing.org/
# It shows the reult with the url https://soundcloud.com/user-86667759/thm-ctf-vol1
SOUNDINGQR
# Listen to the sound plays that is the flag add THM before that words.
```
----

# Task 18 Dig up the past
----

>>Sometimes we need a 'machine' to dig the past

Targetted website: https://www.embeddedhacker.com/
Targetted time: 2 January 2020

```bash
# Use this url to find this https://web.archive.org/web/20200102131252/https://www.embeddedhacker.com/

Uncategorized   
THM flag
What did you just say? flag? THM{ch3ck_th3_h4ckb4ck}
```

----

# Task 19 Uncrackable!
----

>>Can you solve the following? By the way, I lost the key. Sorry >.<

MYKAHODTQ{RVG_YVGGK_FAL_WXF}

Flag format: TRYHACKME{FLAG IN ALL CAP}

```bash
# Use this url to find this https://gchq.github.io/CyberChef/#recipe=Vigen%C3%A8re_Decode('THM')&input=TVlLQUhPRFRRe1JWR19ZVkdHS19GQUxfV1hGfQ

# And use vigenere cipher then add the key THM
TRYHACKME{YOU_FOUND_THE_KEY}
````
----

# Task 20 Small bases
----

>>Decode the following text.

581695969015253365094191591547859387620042736036246486373595515576333693

```bash
┌[rawbot@pwnbox]─[/home/rawbot]
└╼[★]$ python3                                                      
Python 3.9.9 (main, Dec 16 2021, 23:13:29) 
[GCC 11.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> n = 581695969015253365094191591547859387620042736036246486373595515576333693
>>> h = hex(n)[2:]
>>> bytearray.fromhex(h).decode()
'THM{17_ju57_4n_0rd1n4ry_b4535}'
```

----

# Task 21 Read the packet
----

>>I just hacked my neighbor's WiFi and try to capture some packet. He must be up to no good. Help me find it.

```bash
## Found this in the no:1825 and follow the http stream

GET /flag.txt HTTP/1.1
Host: 192.168.247.140
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:60.0) Gecko/20100101 Firefox/60.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Connection: keep-alive
Upgrade-Insecure-Requests: 1
If-Modified-Since: Fri, 03 Jan 2020 04:36:45 GMT
If-None-Match: "e1bb7-15-59b34db67925a"
Cache-Control: max-age=0

HTTP/1.1 200 OK
Date: Fri, 03 Jan 2020 04:43:14 GMT
Server: Apache/2.2.22 (Ubuntu)
Last-Modified: Fri, 03 Jan 2020 04:42:12 GMT
ETag: "e1bb7-20-59b34eee33e0c"
Accept-Ranges: bytes
Vary: Accept-Encoding
Content-Encoding: gzip
Content-Length: 52
Keep-Alive: timeout=5, max=100
Connection: Keep-Alive
Content-Type: text/plain

THM{d0_n07_574lk_m3}

Found me!
```
----

# ANSWER THR FOLLOWING :
----

1.High five!
>>No answer needed

2.Feed me the flag!
>>THM{ju57_d3c0d3_7h3_b453}

3.I'm hungry, I need the flag.
>>THM{3x1f_0r_3x17}

4.It is sad. Feed me the flag.
>>THM{500n3r_0r_l473r_17_15_0ur_7urn}

5.Did you find the flag?
>>THM{wh173_fl46}

6.More flag please!
>>THM{qr_m4k3_l1f3_345y}

7.Found the flag?
>>THM{345y_f1nd_345y_60}

8.Oh, Oh, Did you get it?
>>THM{17_h45_l3553r_l3773r5}

9.What did you get?
>>THM{hail_the_caesar}

10.I'm hungry now... I need the flag
>>THM{4lw4y5_ch3ck_7h3_c0m3mn7} 

11.What is the content?
>>THM{y35_w3_c4n}

12.Did you found the hidden flag?
>>THM{50c14l_4cc0un7_15_p4r7_0f_051n7}

13.Can you decode it?
>>THM{0h_my_h34d}

14.Did you crack it? Feed me now!
>>THM{3xclu51v3_0r}

15.Flag! Flag! Flag!
>>THM{y0u_w4lk_m3_0u7}

16.What does the flag said?
>>THM{7h3r3_15_h0p3_1n_7h3_d4rkn355}

17.What does the bot said?
>>THM{SOUNDINGQR}

18.Did you found my past?
>>THM{ch3ck_th3_h4ckb4ck}

19.The deciphered text
>>TRYHACKME{YOU_FOUND_THE_KEY}

20.What is the flag?
>>THM{17_ju57_4n_0rd1n4ry_b4535}

21.Did you captured my neighbor's flag?
>>THM{d0_n07_574lk_m3}

----