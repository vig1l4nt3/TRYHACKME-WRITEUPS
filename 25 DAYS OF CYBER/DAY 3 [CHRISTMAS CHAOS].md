DAY 3 - CHRISTMAS CHAOS  [WEB EXPLOITATION]
----

# Learning Objectives :
----

# Understanding Authentication
# Understand the use of default credentials and why they're dangerous
# Bypass a login form using BurpSuite

----

# Authentication :
----

>>Authentication is a process of verifying a users' identity, normally by credentials (such as a username, user id or password); to put simply, authentication involves checking that somebody really is who they claim to be. 
>>Authorization (which is fundamentally different to authentication, but often used interchangeably) determines what a user can and can't access; authorization is covered in tomorrow walkthrough, today's task focuses on authentication and some common flaws.

----

# Default Credentials
----

>>You've probably purchased (or downloaded a service/program) that provides you with a set of credentials at the start and requires you to change the password after it's set up (usually these credentials that are provided at the start are the same for every device/every copy of the software). The trouble with this is that if it's not changed, an attacker can look up (or even guess) the credentials.

>>What's even worse is that these devices are often exposed to the internet, potentially allowing anyone to access and control it.  
  * In 2018 it was reported that a botnet (a number of internet-connected devices controlled by an attacker to typically perform DDoS attacks) called "Mirai" took advantage of Internet of Things (IoT) devices by remotely logging, configuring the device to perform malicious attacks at the control of the attackers; 
  * The Mirai botnet infected over 600,000 IoT devices mostly by scanning the internet and using default credentials to gain access.

>>In fact, companies such as Starbucks and the US Department of Defense have been victim to leaving services running with default credentials, and bug hunters have been rewarded for reporting these very simple issues responsibly (Starbucks paid $250 for the reported issue):
 * https://hackerone.com/reports/195163 - Starbucks, bug bounty for default credentials.
 * https://hackerone.com/reports/804548 - US Dept Of Defense, admin access via default credentials.
>>In 2017, it was reported that 15% of all IoT devices still use default passwords.

>>SecLists is a collection of common lists including usernames, passwords, URLs and much more. A password list known as "rockyou.txt" is commonly used in security challenges, and should definitely be a part of your security toolkit.

----

# Dictionary Attacks using BurpSuite
----

>>A dictionary attack is a method of breaking into an authenticated system by iterating through a list of credentials. If you have a list of default (or the most common) usernames and passwords, you can loop through each of them in hopes that one of the combinations is successful.

>>You can use a number of tools to perform a dictionary attack, 
>>One notable one being Hydra (a fast network logon cracker) and BurpSuite, 
>>An industry-standard tool used for web application penetration testing. 
>.Given day 3 is about web exploitation, we'll show you how to use BurpSuite to perform a dictionary attack on a web login form.

1. Start BurpSuite, you can do this on the AttackBox by clicking BurpSuite logo in the icon tray.

2. Once this has loaded, you want to "Intercept" your traffic by proxying it through the BurpSuite, which will then forward the request to the intended destination (in our case it will be a website) This will give you the ability to analyse and modify your browsers traffic.
 * Click on the FoxyProxy browser extension, and select "Burp" - this will now proxy your traffic to BurpSuite.
 * Go to the BurpSuite application and click the Proxy tab, then click the button "Intercept is on".

3. Navigate to your chosen website, as you're intercepting your traffic, you will see BurpSuite has held your request and will not forward it on until you tell it to. Let's go to our web application and submit your details into a given form, in our case its a generic login form.

4. This captured request will show up in the Proxy tab. Right-click it, and click "Send to Intruder"; BurpSuite has a lot of functionality to repeat modify and manipulate requests, Burp Intruder is a tool to automate customize web attacks. We will use intruder to loop through and submit a login request using a list of default credential, in the hopes that one of the usernames and passwords in the list is correct.

5. Go to the Intruder tab, you should see your request. Here we will insert "positions" (telling Burp which fields to update when automating a request), select a list per position and start the attack.
 * Click the "Positions" tab, and clear the pre-selected positions.
 * Add the username and password values as positions (highlight the text and click "Add")
 * Select "Cluster Bomb" in the Attack type dropdown menu; this attack type iterates through each payloads sets in turn, so every combination of each set is tested.

6. We're going to tell each "Position" which Payload to use. In our example, we will select a list of usernames for the username field and a list of passwords for the password field.
 * Click the "Payloads" tab, select your Payload set (set 1 is the username field, set 2 is the password field) and add select your list in the "Payload Options" section (or manually add entries).
 * For set 1 (username), we will add a few common default username entries such as "admin", "root" and "user"
Burp Position Lists 
 * For set 2 (password), we will add a few common default passwords such as "password", "admin" and "12345"

7. Click the "Start Attack" button, this will loop through each position list in every combination. You can sort by the "Length" or "Status" to identify a successful login (typically all incorrect logins will have the same status or length, if a combination is correct it will be different.

----

# MY DOCUMENTATION :
----

>>OPEN THE WEB BROWSER AND SEARCH THE GIVEN "IP-ADDRESS". IT DISPLAYS THE LOGIN PAGE OF THE WEBSITE.THE WEBSITE DISPLAYS SANTA SLEIGH'S TRACKER AND LOGIN AREA. 

>>SIMPLY TRY TO USE A NAME AND PASSWORD "NAME" AND "12345" .NOW TURN ON THE FOXY PROXY AND INTERCEPT THE TRAFFIC AFTER YOU SET THE USER AND PASSWORD.   

>>GO TO BURPSUITE PROXY TAB AND SEND THAT TRAFFIC TO INTRUDER TAB.

>>GO TO THE POSITION TAB IN THE INTRUDER AND SET THE ATTACK TYPE TO CLUSTER BOMB[IT ITERATES THROUGH EACH PAYLOAD SETS IN TURN,SO EVERY COMBINATION OF EACH SET IS TESTED] AND SELECT THE AREA WERE TO USE THE PAYLOAD TO ATTACK WITH ADD ICON ON THE RIGHT SIDE.

>>THEN GO TO THE PAYLOAD TAB IN THE INTRUDER AND SELECT THE PAYLOAD 1 [LIST] AS
root
admin
user

>>PAYLOAD 2 AS
password
admin
12345

>>START ATTACK NOW THE NEW WINDOW WILL POP IN ,ON THAT WINDOW IT WILL TRY ALL THE USERNAME AND PASSWORD WE SPECIFY ON THE PAYLOAD. 

>>EVERY STATUS OF THE PAYLOAD SHOWS "302: and LENGTH "309" BUT THE "admin" and "12345" SHOW THE LENGTH DIFFERENT FROM THE OTHER. IT DISPLAYS "255" AS THE LENGTH

>>TRYING "admin" AND "12345" AS LOGIN CREDENTIALS IN THE LOGIN PAGE, IT LET ME IN. 

>>THE HOME PAGE DISPLAYS THE GPS RADAR AND BELOW THAT FLAG WAS GIVEN.

----

# ANSWER THE FOLLOWING :
----

1.Deploy your AttackBox (the blue "Start AttackBox" button) and the tasks machine (green button on this task) if you haven't already. Once both have deployed, open Firefox on the AttackBox and copy/paste the machines IP (MACHINE_IP) into the browser search bar.
>>No answer needed

Use BurpSuite to brute force the login form.  Use the following lists for the default credentials:
Username	Password
root		root
admin		password
user		12345
Use the correct credentials to log in to the Santa Sleigh Tracker app. Don't forget to turn off Foxyproxy once BurpSuite has finished the attack!
2.What is the flag?
>>THM{885ffab980e049847516f9d8fe99ad1a}

----