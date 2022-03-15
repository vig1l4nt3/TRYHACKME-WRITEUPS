INTRODUCTORY NETWORK 
----

Task 1 : INTRODUCTION
----

> No need to answer.

----

Task 2 : The OSI Model: An Overview
----

Which layer would choose to send data over TCP or UDP?
> 4

Which layer checks received packets to make sure that they haven't been corrupted?
> 2

In which layer would data be formatted in preparation for transmission?
> 2

Which layer transmits and receives data?
> 1

Which layer encrypts, compresses, or otherwise transforms the initial data to give it
as standardlized format?
> 6

Which layer tracks communications between the host and receiving computers
> 5

Which layer accepts communication requests from applications?
> 7

Which layer handles logical addressing?
> 3

When sending data over TCP, what would you call the "bite-sized" pieces of data?
> Segments

Which layer would the FTP protocol communicate with?    
> 7

Which transport layer protocol would be best suited to transmit a live video?
> UDP

----

Task 3 : Encapsulation
----

How would you refer to data at layer 2 of the encapsulation process (with the OSI model)?
> Frames

How would you refer to data at layer 4 of the encapsulation process (with the OSI model),if
the UDP protocol has been selected?
> Datagrams

What process would a computer perform on a received message?
> De-encapsulation

Which is the only layer of the OSI model to add a trailer during encapsulation?
> Data link

Does encapsulation provide an extra layer of security (Aye/Nay)?
> Aye

----

Task 4 : The TCP/IP Model
----

Which model was introduced first, OSI or TCP/IP?
> TCP/IP

Which layer of the TCP/IP model covers the functionality of the Transport layer of the OSI
model (Full Name)?
> Transport

Which layer of the TCP/IP model covers the functionality of the Session layer of the OSI 
model (Full Name)?
> Application

The Network Interface layer of the TCP/IP model covers the functionality of two layers in 
the OSI model. These layers are Data Link, and?.. (Full Name)?
> Physical

Which layer of the TCP/IP model handles the functionality of the OSI network layer?
> Internet

What kind of protocol is TCP?
> connection-based

What is SYN short for?
> synchronise

What is the second step of the three way handshake?
> SYN/ACK

What is the short name for the "Acknowledgement" segment in the three-way handshake?
> ACK

----

Task 5 : [NETWORKING TOOL] : PING 
----

What command would you use to ping the bbc.co.uk website?
> ping bbc.co.uk

Ping muirlandoracle.co.uk
What is the IPv4 address?
> 217.160.0.152

What switch lets you change the interval of sent ping requests?
> -i

What switch would allow you to restrict requests to IPv4?
> -4

What switch would give you a more verbose output?
> -v

----

Task 6 : [NETWORKING TOOL] : TRACEROUTE
----

Use traceroute on tryhackme.com
Can you see the path your request has taken?
> No need to answer.

What switch would you use to specify an interface when using Traceroute?
> -i

What switch would you use if you wanted to use TCP SYN requests when tracing the route?
> -t

[Lateral Thinking] Which layer of the TCP/IP model will traceroute run on by default 
(Windows)?
> Internet
 
----

Task 7 : [NETWORKING TOOL] : WHOIS
----

Perform a whois search on facebook.com
> No need to answer.

What is the registrant postal code for facebook.com?
> 94025

When was the facebook.com domain first registered?
> 29/03/1997

Perform a whois search on microsoft.com
> No need to answer.

Which city is the registrant based in?
> Redmond

[OSINT] What is the name of the golf course that is near the registrant address for 
microsoft.com?
> Bellevue Golf Course

What is the registered Tech Email for microsoft.com?
> msnhst@microsoft.com

----

Task 8 : [NETWORKING TOOL] : DIG
----

What is DNS short for?
> Domain Name System

What is the first type of DNS server your computer would query when you search for
domain?
> recursive

What type of DNS server contains records specific to domain extensions (i.e. .com, 
.co.uk*, etc)*? Use the long version of the name.
> Top-Level Domain

Where is the very first place your computer would look to find the IP address of a domain?
> local cache

[Research] Google runs two public DNS servers. One of them can be queried with the IP 
8.8.8.8, what is the IP address of the other one?
> 8.8.4.4

If a DNS query has a TTL of 24 hours, what number would the dig query show?
> 86400

----

