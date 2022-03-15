DAY 2 - THE ELF STRIKES BACK [WEB EXPLOITATION]
----

# GET Parameters and URLs :
----

>>We looked briefly at the differences between GET and POST requests in the previous dossier; however, the emphasis was on the POST requests used in a login form. 
>>The server you'll be testing today employs a concept called "GET parameters". 
>>Just as POST requests can be used to send information to the server, so too can GET requests be used; however, there is one important difference. 
>>With POST requests the data being sent is included in the "body" of the request. 
>>With GET requests, the data is included in the URL as a "parameter". This is best demonstrated with an example: https://www.thebestfestivalcompany.xyz/index.php?snack=mincePie

There are 7 different parts which make up this URL. Let's look at each of them in turn:

>>First up we have the protocol "(https://)". This specifies whether the request should be made using HTTP, or HTTPS. In our example, we are using HTTPS.

>>Next we have the subdomain "(www)". This is traditionally "www" (World Wide Web) to signify that the target is a website; however, this is not essential. Indeed, subdomains can be basically anything you want; for example, a lot of websites use things like "assets", or "api" to indicate different subdomains with different uses.
(e.g. https://api.thebestfestivalcompany.xyz)

>>The next part of the URL is the domain "(thebestfestivalcompany)". Domains need to be registered and are used as links between a "memorable word or phrase", and an "IP address". In other words, they're used to give a name to the server running a website.

>>Next up we have the "TLD (Top Level Domain) -- .xyz, for our example". 
 * Top-level domains are used by DNS to determine where to look if they want to find your domain. Previously top-level domains had specific uses (and many still do!), but this is not always the case these days. 
 * For example, .co.uk  indicates a company based in the UK, .com indicates a company based in the US.

>>We then have the resource that we're selecting -- in this case that is the homepage of the website: "index.php" . As a side note, all homepages must be called "index" in order to be correctly served by the web server without having to be specified fully, unless this parameter has been changed from the default in the webserver configuration. 
This is how you can go to https://tryhackme.com without having to specify that you want to receive the home page -- the index page is served automatically because you didn't specify!

>>The final two aspects of the URL are the most important for our current topic: they both relate to "GET parameters". 
 * First up we have " ?snack= "  
 * Here " ? "  is used to specify that a " GET parameter is forthcoming ". We then have the "parameter name" : " snack " . This is used to "identify the parameter to the server". We then have an equals sign " (=) " , "indicating that the value will come next".

>>Finally, we have the value of the "GET parameter: mincePie" , because who doesn't like mince pies, right?
  * It's important to note exactly "which part of the URL is the GET parameter". Specifically, we are talking about " ?snack=mincePie ". 
  * "If there was more than one parameter, we would separate them with an ampersand (&)" . 
  * For example: " ?snack=mincePie&drink=hotChocolate ". 
  * In this way we can send multiple values to the server, distinguished by keys (i.e. "mincePie" is identified by the key: "snack").

----

# File Uploads :
----

>>There are countless uses for "file uploads" in the modern internet -- profile pictures, school/university submissions, diagrams, pictures of your dog, you name it! Whilst file uploads are very common, they're also very easy to implement in an insecure fashion. For this reason, it's important that we understand the gravity of the attack vector.

>>When you have the ability to upload files to a server, you have a path straight to 
" RCE (Remote Command Execution) ". 
 * An upload form with no restrictions would mean that you could upload a script that, when executed, connects back to your attacking machine and gives you the ability to run any command you want. 
 * It would be very unusual to find a file upload with no filtering; but it's much less uncommon to find a file upload that employs flawed filtering techniques which can be circumvented to upload a malicious script. 
 * The script has to be written in a language which the server can execute. PHP is usually a good choice for this, as most websites are still written with a PHP back end.  

One of the most common types of filter and its bypass:

>>File Extension Filtering: 
 * As the name suggests extension filtering checks the file extension of uploaded files. 
 * This is often done by specifying a list of allowed extensions, then checking the uploaded file against the list. If the extension is not in the allowlist, the upload is rejected.

>>So, what's the bypass? 
 * Well, the answer is that it depends entirely on how the filter is implemented. 
 * Many extension filters split a filename at the dot (.) and check what comes after it against the list. This makes it very easy to bypass by uploading a double-barrelled extension (e.g. .jpg.php). The filter splits at the dot(s), then checks what it thinks is the extension against the list. 
 * If jpg is an allowed extension then the upload will succeed and our malicious PHP script will be uploaded to the server.

>>When implementing an upload system, it's good practice to upload the files to a directory that can't be accessed remotely. Unfortunately, this is often not the case, and scripts are uploaded to a subdirectory on the webserver (often something like /uploads, /images, /media, or /resources). 
>>For example, we might be able to find the uploaded script at https://www.thebestfestivalcompany.xyz/images/shell.jpg.php.

----

# Reverse Shells :
----

>>Let's assume that we've found somewhere to upload our malicious script, and we've bypassed the filter -- what then? 
 * There are a few paths we can take: the most common of which is uploading a reverse shell.
 * This is a script that creates a network connection from the server, to our attacking machine. 
 * The majority of webservers are written with a PHP back end, which means we need a PHP reverse shell script -- there happens to be one already on Kali/AttackBox at /usr/share/webshells/php/php-reverse-shell.php 
 * (Note: if you're not using Kali or the provided AttackBox, the same script can be found here.

>>Copy the webshell out into your current directory (cp /usr/share/webshells/php/php-reverse-shell.php .), then open it with your text editor of choice.
 * Scroll down to where it has $ip and $port (both marked with // CHANGE THIS). 
 * Set the IP to your TryHackMe IP Address (which can be found in the green bubble on the navbar, if you're using the AttackBox, or by running ip a show tun0 if you're using your own Linux VM with the OpenVPN connection pack) -- making sure to keep the double-quotes. 
 * Set the port to 443 with no double quotes, then save and exit the file. 
 * Congratulations, you now have a fully configured PHP reverse shell script!

>>PHP reverse shells can be very easily activated when stored in an accessible location: simply navigate to the file in your browser to execute the script (and send the reverse shell):

>>We first upload our shell. We then navigate to it in our browser, causing the server to send a reverse shell back to our waiting listener. 

----

# Reverse Shell Listeners :
----
>>We've spoken at length about reverse shell listeners, but what are they? 
 * In short, a reverse shell listener is used to open a network socket to receive a raw connection -- like the one created by a reverse shell being executed! The simplest form of listener is created by using a program called netcat, 
 * Which is installed on both Kali and the AttackBox by default. We can create a listener for an uploaded reverse shell by using this command: sudo nc -lvnp 443. 
 * This essentially creates a listener on port 443. 
 * In a real-world environment, you would want to use a common port such as 443 that is not filtered by firewalls in most scenarios, increasing the chances our reverse shell connects back.. 
 * Once netcat has been setup, our reverse shell will be able to connect back to this when activated.

----

# Putting it all together :
----

>>This was a lot of information, so let's put it all together and look at the full process for exploiting a file upload vulnerability in a PHP web application:

1. Find a file upload point.
2. Try uploading some innocent files -- what does it accept? (Images, text files, PDFs, etc)
3. Find the directory containing your uploads.
4. Try to bypass any filters and upload a reverse shell.
5. Start a netcat listener to receive the shell
6. Navigate to the shell in your browser and receive a connection! 

----

# MY DOCUMENTATION :
----

>>FIRST OPEN THE GIVEN IP ADDRESS TO THE BROWSER AND SEARCH IT, IT WILL DISPLAY "LOG INTO YOUR ID" AND BELOW THAT IT DISPLAYS THE "?id=YOUR_ID" AS A HINT.

>>NOW GO TO SEARCH BAR "http://"GIVEN_IP"/?id=ODIzODI5MTNiYmYw" TO ACCESS THE UPLOADING AREA.TO KNOW WHAT FORMAT ARE ALLOWED USE "CTRL+U" TO VIEW THE PAGE SOURCE THERE IT DISPLAYS THE ACCEPTED FORMATS.

>>WE NEED A PHP-REVERSE SHELL.TO GET THE REVERSE CONNECTION OF THE TARGET MACHINE.COPY THE "REVERSE SHELL" IN THE USR DIRECTORY TO THE CURRENT WORKING DIRECTORY AND RENAMED IT AS .jpeg.php . WE RENAMED IT BECAUSE THE PAGE WERE RESTRICTED TO USE NORMAL ".php" EXTENSION SO WE RENAMED IT .   
```bash
cp /usr/share/webshells/php/php-reverse-shell.php ./shell.jpeg.php
```

>>NOW WE NEED TO SET THE LISTENING THM_IP AND PORT NUMBER.SO WE NEED TO nano INTO THE shell.jpeg.php AND SET IP AND PORT.SAVE AND EXIT IT.

>>NOW GO TO WEBSITE AND SELECT THE FILE AND SUBMIT IT.

>>NAVIGATE TO THE UPLOADS DIRECTORY OF THE WEBSITE AND THERE WE SEE THE FILE WE UPLOADED.

>>CREATE NETCAT SESSION TO LISTEN ON THE PORT-NUMBER WE SPECIFIED ON THE shell.jpeg.php.
```bash
nc -lvnp 9001
```   

>>GO TO THE WEBSITE'S UPLOAD DIRECTORY AND CLICK ON THE FILE WE UPLOADED ,NOW WE WILL GET THE REVERSE CONNECTION TO OUR MACHINE.NOW GO TO var/www/ TO GET THE flag.txt . 
```bash
cd /var/www/
ls
cat flag.txt
```

----

# ANSWER THE FOLLOWING :
----

1.What string of text needs adding to the URL to get access to the upload page?
>>?id=ODIzODI5MTNiYmYw

2.What type of file is accepted by the site?
>>.jpeg

Bypass the filter and upload a reverse shell.
In which directory are the uploaded files stored?
>>/uploads/

Activate your reverse shell and catch it in a netcat listener!
>>No answer needed

What is the flag in /var/www/flag.txt?
>>

----