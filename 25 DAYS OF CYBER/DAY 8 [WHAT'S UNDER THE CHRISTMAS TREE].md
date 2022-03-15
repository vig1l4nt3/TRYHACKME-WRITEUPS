DAY 8 - WHAT'S UNDER THE CHRISTMAS TREE?
----

# STORY :
----

>>After a few months of probation, intern Elf McEager has passed with glowing feedback from Elf McSkidy. During the meeting, Elf McEager asked for more access to The Best Festival Company's (TBFC's) internal network as he wishes to know more about the systems he has sworn to protect.
>>Elf McSkidy was reluctant to agree. However, after Elf McEager's heroic actions in recovering Christmas, Elf McSkidy soon thought this was a good idea. This was uncharted territory for Elf McEager - he had no idea how to begin finding out this information for his new responsibilities. Thankfully, TBFC has a wonderful up-skill program covering the use of Nmap for ElfMcEager to enrol in.

----

# Today's Learning Objectives:
----

>>We're going to be exploring the use of Nmap in our information gathering stage to build a picture of the services running on a remote computer, and to understand how these may be useful to use. We'll also be showing how Nmap scans can be detected and blocked by the use of firewalls.

----

# Intro to Nmap:
----

An open-source, extensible, and importantly free tool, Nmap is one of the industry standard's that everyone should have in their toolkit. Nmap is capable of a few things that are essential in the Information Gathering stages of a penetration testing methodology such as the Penetration Testing Execution Standard (PTES), including:

  * Host discovery
  * Vulnerability discovery
  * Service/Application discovery

----

# Basic Nmap Functionality
----

>>We'll quickly glaze over the basics of getting started with Nmap, the scan types, and the syntax for these types accordingly. We'll apply our networking knowledge learned yesterday in "Day 7 - The Grinch Really Did Steal Christmas" to help understand the differences between TCP and UDP scanning.

## TCP Scanning
----

>>There are two common TCP scan types that you'll be using in Nmap. On the surface they seem to perform the same thing, however, they're very different. Before we break this down, let's illustrate TCP/IP's three-way-handshake again and recap the three stages of a "normal" three-way-handshake:

  * SYN
  * SYN/ACK
  * ACK

> Connect Scan - nmap -sT <ip>
> SYN Scan - nmap -sS <ip>

----

## SYN Scan:
----

>>The most favourable type of scan, Nmap uses the TCP SYN scan (-sS) if the scan is run with both administrative privileges and a different type isn't provided. Unlike a connect scan, this scan type doesn't fulfil the "three-way-handshake" as what would normally take place. Instead, after the "SYN/ACK" is received from the remote host, a "RST" packet is sent by the host that we are scanning from (never completing the connection).

>>This scan type is the most favourable method as Nmap can use all the information gathered throughout the handshake to determine port status based on the response that is given by the IP address that is being scanned:

   * SYN/ACK = open
   * RST = Closed
   * Multiple attempts = filtered

>>Not only this but also since fewer packets are sent across a network, there is less likelihood of being detected.

## Connect Scan:
----

>>Unlike a SYN scan, administrative privileges aren't required for this scan to run. This is as a result of Nmap using the Berkeley Sockets API which has quickly formed to be the standard method of how services like web applications communicate with an operating system. As a result of more packets being sent by Nmap, this scan is easier to detect and takes a longer time to complete. Moreover, as the "three-way-handshake" completes as if it were a normal connection, it can be logged a lot more conveniently.

----

# Nmap Timing Templates

>>Nmap allows the user to determine Nmap's performance. Measured in aggressiveness, a user can use one of six profiles [0 to 5] to change the rate at which Nmap scans at. With -T0 being the stealthiest, this profile scans one port every 5 minutes, whereas -T5 is considered both the most aggressive and potential to be inaccurate. This is because the -T5 waits a mere 0.3 seconds for the remote device to respond to a handshake. Factors such as those listed below determine how accurate these scans are:

  * The resource usage a remote server is under. The higher usage, the slower it is to send a response to Nmap.
  * The quality of the connection. If you have a slow or unstable connection, you are likely to miss responses if you are sending many packets at once.

>>Generally speaking, you will want to use low-aggressive profiles for real-world scenarios, however, in a lab environment where noise doesn't matter - high-aggressive profiles prove to be the quickest. For perspective, Nmap uses -T3 if no profile is provided. In a pentesting situation, you'd be inclined to use a lower value such as within in a lab environment, a higher value-T4 will suffice as stealth is not as critical.

----

# An Introduction to Nmap's Scripting Engine
----

>>A recent addition to Nmap is the Nmap Scripting Engine or NSE for short. This feature introduces a "plug-in" nature to Nmap, where scripts can be used to automate various actions such as:

  * Exploitation
  * Fuzzing
  * Bruteforcing

>>Take for example the FTP ProFTPD Backdoor script. This script attempts to exploit an FTP service that is running ProFTPD version 1.3.3c, the version of which is fingerprinted by Nmap itself.
>>We can provide the script that we want to use by using the --script flag in Nmap like so:nmap --script ftp-proftpd-backdoor -p 21 <ip_address>

----

# Additional Scan Types:
----

>>Not only can we use the Nmap's TCP Scan, but Nmap also boasts a combination of these types for various actions that are useful to us during the information gathering stage. I have assorted these into the table below, giving a brief explanation of their purpose.

>>(Where x.x.x.x == MACHINE_IP )

Flag 	Usage Example 	Description
-A 		nmap -A x.x.x.x 	Scan the host to identify services running by matching against Nmap's 
							database with OS detection
-O 		nmap -O x.x.x.x 	Scan the host to retrieve and perform OS detection
-p 		nmap -p 22 x.x.x.x 	Scan a specific port number on the host. A range of ports can also be 
							provided (i.e. 10-100) by using the first and last value of the range like so: nmap -p 10-100 x.x.x.x
-p- 	nmap -p- x.x.x.x 	Scan all ports (0-65535) on the host
-sV 	nmap -sV x.x.x.x 	Scan the host using TCP and perform version fingerprinting	

----

# Defending against Nmap Scans :
----

>>The practice of security through obscurity doesn't work here. Whilst it may seem logical to attempt to hide a service by changing its port number to something other than the standard (such as changing SSH from port 22 to 2222), the service will still be fingerprinted during an Nmap scan (albeit slightly later on). Unfortunately, you cannot get the best of both worlds in having a service available yet hidden.
>>Fortunately, open-source Intrusion Detection (IDS) & Prevention Systems (IPS) such as Snort and Suricata allows blue-teamers to protect their networks using network monitoring. For example, you would install these services on firewalls such as pfSense:
>>Rulesets such as the emerging threats for Snort and Suricata are capable of detecting and blocking a wide variety of potentially malicious traffic - including:
* Malware traffic
* Reverse shells
* Metasploit payloads
* Nmap scans
>>If properly configured, a majority of Nmap scans can be detected. This is especially true when using an aggressive timing template such as -T4 or -T5. Let's take a look at the following Nmap scan being detected: nmap -A 192.168.1.171
>>Starting an Nmap scan to the pfSense firewall.
>>After returning to pfSense a few seconds later, we notice that alerts are being generated by Snort:
>>Viewing newly created alerts by Snort as a result of the Nmap scan.
>>Even with a timing template of-T3, Snort is capable of detecting the port scan, where after 6 alerts (in this case) the attacker is then blocked by the firewall.
>>After 6 alerts, Snort blocks the IP address running the Nmap scan from contacting the pfSense firewall.
>>Confirming that the IP address running the Nmap scan can no longer contact the pfSense firewall.

----

# Challenge
----

>>Deploy and use Nmap to scan the instance attached to this task. Take a note of the IP address to scan: MACHINE_IP and enumerate it for Elf McEager!
>>Optional bonus: As a result of Elf McEager managing to recover Christmas in "Day 7 - The Grinch Really Did Steal Christmas", TBFC's website has been restored for all the elves to visit. Can you find it? I hear it's quite the read... You must add MACHINE_IP tbfc.blog to your /etc/hosts file before the application will load like below:

----

# MY DOCUMENTATION :
----

>>USING NMAP TO ENUMERATE THE GIVEN_IP ADDRESS.
```bash
root@ip-10-10-211-26:~# nmap -p- 10.10.78.84

Starting Nmap 7.60 ( https://nmap.org ) at 2021-11-01 06:44 GMT

root@ip-10-10-211-26:~# nmap -sC 10.10.78.84

Starting Nmap 7.60 ( https://nmap.org ) at 2021-11-01 06:45 GMT
Nmap scan report for ip-10-10-78-84.eu-west-1.compute.internal (10.10.78.84)
Host is up (0.00053s latency).
Not shown: 997 closed ports
PORT     STATE SERVICE
80/tcp   open  http
|_http-generator: Hugo 0.78.2
|_http-title: TBFC&#39;s Internal Blog
2222/tcp open  EtherNetIP-1
3389/tcp open  ms-wbt-server
MAC Address: 02:03:E5:1E:1B:BD (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 62.98 seconds

root@ip-10-10-211-26:~# nmap -Pn 10.10.78.84 

Starting Nmap 7.60 ( https://nmap.org ) at 2021-11-01 06:47 GMT
Nmap scan report for ip-10-10-78-84.eu-west-1.compute.internal (10.10.78.84)
Host is up (0.031s latency).
Not shown: 997 closed ports
PORT     STATE SERVICE
80/tcp   open  http
2222/tcp open  EtherNetIP-1
3389/tcp open  ms-wbt-server
MAC Address: 02:03:E5:1E:1B:BD (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 3.61 seconds

root@ip-10-10-211-26:~# nmap -sV 10.10.78.84 

Starting Nmap 7.60 ( https://nmap.org ) at 2021-11-01 06:48 GMT
Nmap scan report for ip-10-10-78-84.eu-west-1.compute.internal (10.10.78.84)
Host is up (0.00065s latency).
Not shown: 997 closed ports
PORT     STATE SERVICE       VERSION
80/tcp   open  http          Apache httpd 2.4.29 ((Ubuntu))
2222/tcp open  ssh           OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
3389/tcp open  ms-wbt-server xrdp
MAC Address: 02:03:E5:1E:1B:BD (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 8.42 seconds

root@ip-10-10-211-26:~# nmap --script http-title -p 80 10.10.78.84

Starting Nmap 7.60 ( https://nmap.org ) at 2021-11-01 06:57 GMT
Nmap scan report for ip-10-10-78-84.eu-west-1.compute.internal (10.10.78.84)
Host is up (0.00019s latency).

PORT   STATE SERVICE
80/tcp open  http
|_http-title: TBFC&#39;s Internal Blog
MAC Address: 02:03:E5:1E:1B:BD (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 0.74 seconds

```

>>ADDING THE IP_ADDRESS AND WEBSITE TO THE /etc/hosts FILE.
```bash
GNU nano 2.9.3                      /etc/hosts                      Modified  

127.0.0.1       localhost
127.0.1.1       tryhackme.lan   tryhackme
10.10.78.84     tbfc.blog
# The following lines are desirable for IPv6 capable hosts
::1     localhost ip6-localhost ip6-loopback
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
```

>>USE THE GIVE IP ADDRESS TO GET THIS RESPONSE.
```raw_ip-address_website-response
TBFC's Internal Blog
Reindeer of the Week
 Nov 25, 2020
Meet the Team
 Nov 25, 2020
Recruitment Drive

Hey fellow Elves! We\u2019re currently recruiting for the positions listed below. As always, please sned your reccomendations to your workshop manager - any successful referer will receieve a $150 bonus in their next pay packet. 1x HR Manager: We are seeking a new Elf McKaren. All applications must have 3 years prior experience in a similar role and be able to work under crunch time. 4x Stocking Fillers Our dispatch team is looking for new fresh-faces to bolster the ranks of fellow stocking fillers.
Read More
 Nov 25, 2020

Elf McEager  © 2020  /  TBFC's Internal Blog  \u2013 

Powered By Hugo  Theme Harbor
```

>>CLICK ON THE TBFC's INTERNAL BLOG REDIRECT TO THIS URL FROM THE IP_ADDRESS.
http://tbfc.blog/posts/recruitment-drive/ 
```response_after_click_on_the_TBFC's_Internal_Blog

Recruitment Drive

Hey fellow Elves! We\u2019re currently recruiting for the positions listed below. As always, please sned your reccomendations to your workshop manager - any successful referer will receieve a $150 bonus in their next pay packet.
1x HR Manager:

We are seeking a new Elf McKaren. All applications must have 3 years prior experience in a similar role and be able to work under crunch time.
4x Stocking Fillers

Our dispatch team is looking for new fresh-faces to bolster the ranks of fellow stocking fillers. No prior experience is necessary, howevver, all applicants much show a good work ethic during interview.
x2 Reindeer Consultants

We\u2019re looking for two Reindeer consultants to join us for Christmas 2020 to reign in our Reindeers. This is a part-time role in our North-Eastern Artic headquarters, prior Reindeer keeping experience must be demonstrated by all applicants.

Elf McEager  © 2020  /  TBFC's Internal Blog  \u2013 

Powered By Hugo  Theme Harbor

```

----

# ANSWER THE FOLLOWING :
----

1.When was Snort created?
>>1998

2.Using Nmap on MACHINE_IP , what are the port numbers of the three services running?  (Please provide your answer in ascending order/lowest -> highest, separated by a comma)
>>80,2222,3389

3.Run a scan and provide the -Pn flag to ignore ICMP being used to determine if the host is up
>>No answer needed 

4.Experiment with different scan settings such as-A and -sV whilst comparing the outputs given.
>>No answer needed 

5.Use Nmap to determine the name of the Linux distribution that is running, what is reported as the most likely distribution to be running?
>>Ubuntu

6.Use Nmap's Network Scripting Engine (NSE) to retrieve the "HTTP-TITLE" of the webserver. Based on the value returned, what do we think this website might be used for?
>>blog

7.Now use different scripts against the remaining services to discover any further information about them
>>No answer needed 

----