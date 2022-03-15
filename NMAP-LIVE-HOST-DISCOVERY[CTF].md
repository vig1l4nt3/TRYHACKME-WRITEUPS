# Task 1 Introduction
----

1.Some of these questions will require the use of a static site to answer the task questions, while others require the use of the AttackBox and the target VM.
>>No answer needed

----

# Task 2 Subnetworks
----

Send a packet with the following:
    * From computer1
    * To computer1 (to indicate it is broadcast)
    * Packet Type: “ARP Request”
    * Data: computer6 (because we are asking for computer6 MAC address using ARP Request)
2.How many devices can see the ARP Request?
>>4

3.Did computer6 receive the ARP Request? (Y/N)
>>N

Send a packet with the following:
    * From computer4
    * To computer4 (to indicate it is broadcast)
    * Packet Type: “ARP Request”
    * Data: computer6 (because we are asking for computer6 MAC address using ARP Request)
4.How many devices can see the ARP Request?
>>4

5.Did computer6 reply to the ARP Request? (Y/N)
>>Y

----

# Task 3 Enumerating Targets
----

6.What is the first IP address Nmap would scan if you provided 10.10.12.13/29 as your target?
>>10.10.12.8

7.How many IP addresses will Nmap scan if you provide the following range 10.10.0-255.101-125? 
>>6400

----

# Task 4 Discovering Live Hosts
----

Send a packet with the following:
    * From computer1
    * To computer3
    * Packet Type: “Ping Request”
8.What is the type of packet that computer1 sent before the ping?
>>arp request

9.What is the type of packet that computer1 received before being able to send the ping?
>>arp response

10.How many computers responded to the ping request?
>>1

Send a packet with the following:
    * From computer2
    * To computer5
    * Packet Type: “Ping Request”
11.What is the name of the first device that responded to the first ARP Request?
>>router

12.What is the name of the first device that responded to the second ARP Request?
>>computer5

13.Send another Ping Request. Did it require new ARP Requests? (Y/N)
>>N

----

# Task 5 Nmap Host Discovery Using ARP
----

We will be sending broadcast ARP Requests packets with the following options:
    * From computer1
    * To computer1 (to indicate it is broadcast)
    * Packet Type: “ARP Request”
    * Data: try all the possible eight devices (other than computer1) in the network: computer2, computer3, computer4, computer5, computer6, switch1, switch2, and router.
14.How many devices are you able to discover using ARP requests? * 
>>3

----

# Task 6 Nmap Host Discovery Using ICMP
----

15.What is the option required to tell Nmap to use ICMP Timestamp to discover live hosts?
>>-PP

16.What is the option required to tell Nmap to use ICMP Address Mask to discover live hosts?
>>-PM

17.What is the option required to tell Nmap to use ICMP Echo to discover life hosts?
>>-PE

----

# Task 7 Nmap Host Discovery Using TCP and UDP
----

18.Which TCP ping scan does not require a privileged account?
>>tcp syn ping

19.Which TCP ping scan requires a privileged account?
>>tcp ack ping

20.What option do you need to add to Nmap to run a TCP SYN ping scan on the telnet port?
>>-PS23

----

# Task 8 Using Reverse-DNS Lookup
----

21.We want Nmap to issue a reverse DNS lookup for all the possibles hosts on a subnet, hoping to get some insights from the names. What option should we add?
>>-R

----

# Task 9 Summary
----

22.Ensure you have taken note of all the Nmap options explained in this room. To continue learning about Nmap, please join the room Nmap Basic Port Scans, which introduces the basic types of port scans.
>>No answer needed

----