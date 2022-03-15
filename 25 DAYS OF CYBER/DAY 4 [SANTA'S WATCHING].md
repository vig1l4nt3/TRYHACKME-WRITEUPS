DAY 4 - SANTA'S WATCHING  [WEB EXPLOITATION]
----

# STORY :
----
# Introduction & Story:

We're going to be taking a look at some of the fundamental tools used in web application testing. You're going to learn how to use Gobuster to enumerate a web server for hidden files and folders to aid in the recovery of Elf's forums. Later on, you're going to be introduced to an important technique that is fuzzing, where you will have the opportunity to put theory into practice.

Our malicious, despicable, vile, cruel, contemptuous, evil hacker has defaced Elf's forums and completely removed the login page! However, we may still have access to the API. The sysadmin also told us that the API creates logs using dates with a format of YYYYMMDD.

----

# What is Fuzzing?
----

>>To keep it simple, fuzzing can be argued as "fancy bruteforcing" to some degree.However, you can fuzz what you can't bruteforce.  
>>Fuzzing is using security tools to automate the input of data we provide into things such as websites or software applications. 
>>Fuzzing is an extremely effective process as computers can perform laborious actions like trying to find hidden files/folders, try different username and passwords much quicker then a human can (and is willing to do...)

>>Poorly built applications are often unable to handle data the way it is supposed to under intense load. 
>>Moreover, the data we're parsing to the application may be interpreted and executed (instead of being handled correctly i.e. system commands). 
>>We can use fuzzing to cause the application to trigger what's known as an error condition where this may be abused by a penetration tester or a bug bounty hunter.

----

# An Introduction to Using Gobuster :
----

>>Logically speaking, there are many pieces to a website that the average user doesn't see. 
>>They can be anything from a sitemap to a secret directory which contains important files. 
>>Unfortunately, this can cause developers to get a bit lazy, and not protect these directories, allowing anyone who finds out that they exist to steal the important data. 
>>"gobuster" is the tool that helps us discover these valuable directories if they exist. 
>>The idea behind the tool itself is simple, bruteforcing common paths to check if it's valid. 
>>Similar to how you would in your browser, albeit this tool is much, much quicker. Gobuster has three modes: dir, vhost and dns.

>>For the sake of today, we're going to be using gobuster in dir mode, as this is the most likely mode that you'll be using day-to-day. dir (short for directory) can be selected by using 
gobuster dir <rest of command>

* Let's use the table below to illustrate how wordlists work:

Original URL				Item in Wordlist	Final URL
http://example.com			backups				http://example.com/backups
http://loveucmnatic.thm		shepards			http://loveucmnatic.thm/shepards


>>Gobuster has a few other little tricks, it supports appending extensions which means you can bruteforce files as well. 
* We can use another handy little chart to visualize it and show an example later on:

Original URL	    Item in Wordlist  Specified Extension	Final URL
http://example.com	backup					php				http://example.com/backup.php
http://example.com	backup					txt				http://example.com/backup.txt
http://example.com	icecream				html			http://example.com/icecream.html

>>To find the data in the table above, we have used a command such as the following (assuming wordlist.txt had the words "backup" and "icecream"): 
```bash
gobuster dir -u http://example.com -w wordlist.txt -x php,txt,html.
```

>>Whilst Gobuster is considerably faster than alternatives such as Dirbuster on Kali Linux, it is still limited to the wordlists and options that you provide. 
>>The more appropriate your wordlist is to the target, the better your results will be. 
>>Wordlists such as "SecLists" have wordlists for specific applications and platforms. 
>>You can use the information gathered from enumerating to help determine what wordlist may be the most appropriate to use. Although, we will come onto refining those skills in a later day.
seclists1

>>However, let's detail a few of the common options below:
* Options	Description
1.  -u	    Used to specify which url to enumerate.Used to specify which wordlist that is appended on the url path i.e

2.  -w	     "http://url.com/word1"
 			 "http://url.com/word2"
 			 "http://url.com/word3.php"
3. -x	    Used to specify file extensions i.e "php,txt,html"

>>Recommended wordlist to use: big.txt

>>We have provided wordlists for you on the AttackBox located in "/usr/share/wordlists". 
>>This is also the default location for wordlists on pentesting distributions such as Kali Linux. 
>>To provide an example, "big.txt" is located at the file path "/usr/share/wordlists/dirb/big.txt". 

----

# An Introduction to Using wfuzz :
----

>>The premise behind wfuzz is simple. 
>>Occasionally you want a bit more information about how much data something within a web application returns. 
>>This could be anything from a file, a response code (i.e. 404 meaning the URL doesn't exist) or the parameters used in a form similar to the form you attacked in Day 2.

>>For example, let's say you are pentesting a note-taking application and you want to see if you can view notes by other users. 
>>One way you may want to achieve this is by FUZZing for usernames (with the knowledge that every valid user will have note.txt by default). 
>>Our wfuzz command would like the following: 
```bash
wfuzz -c -z file,/usr/share/wordlists/dirb/big.txt localhost:80/FUZZ/note.txt
```

>>Now wfuzz will query the webserver using the words iterated from the "big.txt" wordlist. To illustrate:
 * Query #1: http://localhost/admin/note.txt
 * Query #2: http://localhost/administrator/note.txt
 * Query #3: http://localhost/system/note.txt

>>Note how the "FUZZ" parameter is being replaced with the words from the wordlist. 
>>We'll outline some of the options that can be configured in wfuzz, however, it's worth knowing that will display results that are different to the parameters that we set. 
>>In [the picture] above we used the "--hw" option to hide all pages that have 57 words on them. Since wfuzz found a URL with only 8 words, it'll be displayed to us, as this is not 57 words.

>>It is important to know that you can FUZZ any part of the URL, meaning that you can test any parameters if you don't know them as well.
>>Some of the more useful options into the table below:

Options		Description
-c			Shows the output in color
-d			Specify the parameters you want to fuzz with, where the data is encoded for a HTML form
-z			Specifies what will replace FUZZ in the request. For example -z file,big.txt. We're 
            telling wfuzz to look for files by replacing "FUZZ" with the words within "big.txt"
--hc		Don't show certain http response codes. I.e. Don't show 404 responses that indicate the 
			file doesn't exist, or "200" to indicate the file does exist
--hl		Don't show for a certain amount of lines in the response
--hh		Don't show for a certain amount of characters


>>Let's bring this together and demonstrate some of these options. 
>>Let's say we wanted to fuzz an application on http://shibes.thm/login.php to find the correct credentials to the login form. 
>>After recalling our knowledge from Day 2, we know all about URL parameters! 
>>We can take a bit of a guess as to what parameters the login form may be using username and password, right? Worth a try! Our wfuzz command would look like so: 
```bash
wfuzz -c -z file,mywordlist.txt -d “username=FUZZ&password=FUZZ” -u http://shibes.thm/login.php
```

>>Where wfuzz will now iterate through the wordlist we provided and replace the "FUZZ" values specified in the "username" and "password" parameters.

----

# How to approach the challenge
----

>>Since we know there's theoretically an API directory we can use gobuster to enumerate the website and see if we can find anything. 
>>Then assuming we do find something, we should investigate it for interesting files. 
>>Let's say we then find what seems to hold the logs, we know we're searching by date, so we can infer that there's a good chance that we'll be using the date parameter to interact with the API. 
>>We also know that the API takes a date in the form of YYYYMMDD. 
>>A wordlist in that format can be found in the hint for this task, although if you want an extra challenge, you can try and build a wordlist in that format yourself.

>>Finally, API's may not return data if the proper parameters aren't passed, so with that knowledge, we can use the options in wfuzz to filter out parameters that don't return anything.

>>With all that in mind, we should be able to get a flag.

----

# MY DOCUMENTATION :
----
>>OPEN THE GIVEN-IP IN THE BROWSER.IT DISPLAYS "Y0u h4v3 b33n d3f4c3d y0ur f0rums ar3 g0ne"

>>USING GOBUSTER TO IDENTIFY THE HIDDEN DIRECTORIES IN THE WEBSITE.WITH THE RESULTS WE HAVE "api" DIRECTORY NAVIGATING TO THE DIRECTORY  "http://GIVEN-IP/api" .THERE WE SEE "site-log.php" .BUT IF WE GO TO THAT FILE THERE IS NOTHING DISPLAYED.
```bash
gobuster dir -u http://GIVEN-IP -w /usr/share/dirb/wordlists/big.txt
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://10.10.2.188
[+] Threads:        10
[+] Wordlist:       /usr/share/dirb/wordlists/big.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Timeout:        10s
===============================================================
2021/10/27 14:43:39 Starting gobuster
===============================================================
/.htaccess (Status: 403)
/.htpasswd (Status: 403)
/LICENSE (Status: 200)
/api (Status: 301)
/server-status (Status: 403)
===============================================================
2021/10/27 14:45:08 Finished
```
>>IN "APPORACHING CHALLENGE" THEY SPECIFY THAT "api" SEARCHING BY DATE HAS A GOOD CHANCE TO INFER.SO WE ARE USING "date" PARAMETER TO INTERACT WITH "api".WE KNOW THAT "api" TAKES DATE IN THE FORM OF "YYYYMMDD". WE KNOW YEAR "2020" BUT WE DON'T KNOW WHAT THE DAY AND MONTH.SO WE HAVE WORDLIST TO FUZZ AND FIND THE DATE TO ACCESS THE FLAG. 
```txt
20201100
20201101
20201102
20201103
20201104
20201105
20201106
20201107
20201108
20201109
20201110
20201111
20201112
20201113
20201114
20201115
20201116
20201117
20201118
20201119
20201120
20201121
20201122
20201123
20201124
20201125
20201126
20201127
20201128
20201129
20201130
20201201
20201202
20201203
20201204
20201205
20201206
20201207
20201208
20201209
20201210
20201211
20201212
20201213
20201214
20201215
20201216
20201217
20201218
20201219
20201220
20201221
20201222
20201223
20201224
20201225
20201226
20201227
20201228
20201229
20201230
20201231
```
>>WE USE "wfuzz" TOOL TO FUZZ THE WEBSITE AND FIND THE DATE WITH THE WORDLIST WE SPECIFIED WITH "-c" MEANS COLOR CODED , "-z" MEANS PAYLOAD "-u" MEANS SPEECIFY URL AND FINALLY "?date=FUZZ" WHICH WE NEED TO FUZZ.
```bash
root@ip-10-10-21-65:~# wfuzz -c -z file,wordlist.txt -u http://GIVEN-IP/api/site-log.php?date=FUZZ

Warning: Pycurl is not compiled against Openssl. Wfuzz might not work correctly when fuzzing SSL sites. Check Wfuzz's documentation for more information.

********************************************************
* Wfuzz 2.2.9 - The Web Fuzzer                         *
********************************************************

Target: http://GIVEN-IP/api/site-log.php?date=FUZZ
Total requests: 59

==================================================================
ID	   Response   Lines      Word         Chars     Payload    
==================================================================

000020:  C=200      0 L	       0 W	      0 Ch	  "20201119"
000001:  C=200      0 L	       0 W	      0 Ch	  "20201100"
000002:  C=200      0 L	       0 W	      0 Ch	  "20201101"
000004:  C=200      0 L	       0 W	      0 Ch	  "20201103"
000005:  C=200      0 L	       0 W	      0 Ch	  "20201104"
000006:  C=200      0 L	       0 W	      0 Ch	  "20201105"
000008:  C=200      0 L	       0 W	      0 Ch	  "20201107"
000009:  C=200      0 L	       0 W	      0 Ch	  "20201108"
000021:  C=200      0 L	       0 W	      0 Ch	  "20201120"
000022:  C=200      0 L	       0 W	      0 Ch	  "20201121"
000023:  C=200      0 L	       0 W	      0 Ch	  "20201122"
000030:  C=200      0 L	       0 W	      0 Ch	  "20201129"
000024:  C=200      0 L	       0 W	      0 Ch	  "20201123"
000025:  C=200      0 L	       0 W	      0 Ch	  "20201124"
000026:  C=200      0 L	       1 W	     13 Ch	  "20201125"
000027:  C=200      0 L	       0 W	      0 Ch	  "20201126"
000028:  C=200      0 L	       0 W	      0 Ch	  "20201127"
000029:  C=200      0 L	       0 W	      0 Ch	  "20201128"
000031:  C=200      0 L	       0 W	      0 Ch	  "20201130"
000032:  C=200      0 L	       0 W	      0 Ch	  "20201201"
000033:  C=200      0 L	       0 W	      0 Ch	  "20201202"
000034:  C=200      0 L	       0 W	      0 Ch	  "20201203"
000003:  C=200      0 L	       0 W	      0 Ch	  "20201102"
000007:  C=200      0 L	       0 W	      0 Ch	  "20201106"
000035:  C=200      0 L	       0 W	      0 Ch	  "20201204"
000036:  C=200      0 L	       0 W	      0 Ch	  "20201205"
000037:  C=200      0 L	       0 W	      0 Ch	  "20201206"
000038:  C=200      0 L	       0 W	      0 Ch	  "20201207"
000049:  C=200      0 L	       0 W	      0 Ch	  "20201218"
000039:  C=200      0 L	       0 W	      0 Ch	  "20201208"
000040:  C=200      0 L	       0 W	      0 Ch	  "20201209"
000041:  C=200      0 L	       0 W	      0 Ch	  "20201210"
000042:  C=200      0 L	       0 W	      0 Ch	  "20201211"
000043:  C=200      0 L	       0 W	      0 Ch	  "20201212"
000044:  C=200      0 L	       0 W	      0 Ch	  "20201213"
000045:  C=200      0 L	       0 W	      0 Ch	  "20201214"
000046:  C=200      0 L	       0 W	      0 Ch	  "20201215"
000047:  C=200      0 L	       0 W	      0 Ch	  "20201216"
000048:  C=200      0 L	       0 W	      0 Ch	  "20201217"
000050:  C=200      0 L	       0 W	      0 Ch	  "20201219"
000051:  C=200      0 L	       0 W	      0 Ch	  "20201220"
000052:  C=200      0 L	       0 W	      0 Ch	  "20201221"
000053:  C=200      0 L	       0 W	      0 Ch	  "20201222"
000054:  C=200      0 L	       0 W	      0 Ch	  "20201223"
000055:  C=200      0 L	       0 W	      0 Ch	  "20201224"
000056:  C=200      0 L	       0 W	      0 Ch	  "20201225"
000057:  C=200      0 L	       0 W	      0 Ch	  "20201226"
000058:  C=200      0 L	       0 W	      0 Ch	  "20201227"
000059:  C=200      0 L	       0 W	      0 Ch	  "20201228"
000010:  C=200      0 L	       0 W	      0 Ch	  "20201109"
000011:  C=200      0 L	       0 W	      0 Ch	  "20201110"
000012:  C=200      0 L	       0 W	      0 Ch	  "20201111"
000013:  C=200      0 L	       0 W	      0 Ch	  "20201112"
000014:  C=200      0 L	       0 W	      0 Ch	  "20201113"
000015:  C=200      0 L	       0 W	      0 Ch	  "20201114"
000016:  C=200      0 L	       0 W	      0 Ch	  "20201115"
000017:  C=200      0 L	       0 W	      0 Ch	  "20201116"
000018:  C=200      0 L	       0 W	      0 Ch	  "20201117"
000019:  C=200      0 L	       0 W	      0 Ch	  "20201118"

Total time: 0.874912
Processed Requests: 59
Filtered Requests: 0
Requests/sec.: 67.43527
```
>>HERE WE SEE THE DIFFERENCE "[ 000026:  C=200      0 L	       1 W	     13 Ch	  "20201125" ]" FROM THE OTHER.
>>HERE WE HAVE "13 CHARS" AND WORD 1. SO WE TRY IT WITH "site-log.php" TRY TO ACCESS THE FLAG WITH 
"http://GIVEN-IP/api/site-log.php?date=20201125" .
>>AFTER ACCESSING THAT WE HAVE THE "FLAG".

----

# ANSWER THE FOLLOWING :
----

1.Deploy your AttackBox (the blue "Start AttackBox" button) and the tasks machine (green button on this task) if you haven't already. Once both have deployed, open FireFox on the AttackBox and copy/paste the machines IP (MACHINE_IP) into the browser search bar.
>>No answer needed

Given the URL "http://shibes.xyz/api.php", what would the entire wfuzz command look like to query the "breed" parameter using the wordlist "big.txt" (assume that "big.txt" is in your current directory)
Note: For legal reasons, do not actually run this command as the site in question has not consented to being fuzzed!
>>wfuzz -c -z file,big.txt http://shibes.xyz/api.php?breed=FUZZ 

Use GoBuster (against the target you deployed -- not the shibes.xyz domain) to find the API directory. What file is there?
>>site-log.php

Fuzz the date parameter on the file you found in the API directory. What is the flag displayed in the correct post?
>>THM{D4t3_AP1}

----