# DAY 4 - SANTA'S RUNNING BEHIND [WEB EXPLOITATION]  
----

# STORY:
----

>>McSysAdmin managed to reset everyone's access except Santa's! Santa's expected some urgent travel itinerary for his route over Christmas. Rumour has it that Santa never followed the password security recommendations. Can you use bruteforcing to help him access his accounts?

----

# What is authentication, and where is it Used?
----

>>Authentication is the process of verifying a user’s identity, establishing that they are who they say they are. Authentication can be proven using a variety of authentication means, including:
   1.A known set of credentials to the server and user such as a username and password
   2.Token authentication (these are unique pieces of encrypted text)
   3.Biometric authentication (fingerprints, retina data, etc.)

>>Authentication is often used interchangeably with authorisation, but it is very different. Authorisation is a term for the rules defining what an authenticated user can and cannot access. For example, a standard, the authenticated user will only be allowed access to some aspects of a website. An authenticated administrator will be able to access the entire thing – their level of authorisation determines this.

>>Authentication is used when there is a necessity to know who is accessing pieces of data or information to create accountability – protecting the Confidentiality element of the CIA triad.

----

# What is Fuzzing?
----

>>Put simply, fuzzing is an automated means of testing an element of a web application until the application gives a vulnerability or valuable information. When we are fuzzing, we provide information as we would typically when interacting with it, just at a much faster rate. This means that we can use extensive lists known as wordlists to test a web application’s response to various information.

>>For example, rather than testing a login form for a valid set of credentials one-by-one (which is exceptionally time-consuming), we can use a tool for fuzzing this login form to test hundreds of credentials at a much faster rate and more reliably.

----

# Fuzzing with Burp Suite
----

>>Please note that this task presumes that you are using the TryHackMe AttackBox. The AttackBox has the necessary software, tools and wordlists installed for you to complete today’s task – so it is highly recommended that you use it (to start it, scroll to the top of this page and click the blue "Start AttackBox" button, then follow the steps below).

1.Let’s launch FireFox by navigating to “Applications -> Internet-> Firefox”
2.Navigate to the login form in the web browser using the vulnerable IP address (http://MACHINE_IP) (note that the IP address in the screenshots is only an example, you will need to replace this with the MACHINE_IP)
3.Let’s launch Burp Suite by navigating to “Applications -> Other -> Burp Suite"
4.Navigate to the "Proxy" tab:
5.And press "Intercept On" in "Proxy -> Intercept"
6.Now we will need to return to our Firefox Browser and enable the FoxyProxy extension in Firefox. You can do this by pressing the “FoxyProxy” extension and pressing “Burp” like so:
7.Submit some dummy data on the login form, such as below. Please note that the web browser will hang:
8.We will need to return to Burp Suite, where we will see some data has now been returned to us. Right-click the window containing the data and press “Send to Intruder”
9.Navigate to the “Intruder” tab and select the following:
10.1.Click the “Positions” tab and clear the pre-selected positions by pressing "Clear $"
10.2.Add the value of the "username" parameter as the first position, sometimes we will already know the username, other times we will not. We can tell Burp to use a wordlist of usernames for this position as well. However, as the username is already known (i.e. cmnatic), we are just brute-forcing the password. (this can be done by highlighting the password parameter and clicking “Add $”)
10.3.Add the “password” value as the position (i.e. password123) (this can be done by highlighting the value in the password parameter and clicking “Add $”)
10.4.Select “Cluster Bomb” in the Attack type in the dropdown menu.
10.5.We will now need to provide some values for Burp Suite to fuzz the login form with passwords. We can provide a list manually, or we can provide a wordlist. I will be providing a wordlist.
11. Now after selecting our wordlist, we can see that some passwords have filled the “Payload Options” window:
12.Now let’s press the “Start Attack” button, this will begin fuzzing the login form. Look at the results and sort by “Length”. Unsuccessful passwords will return a certain length, and successful passwords will return another length. You will need to examine the results with different lengths to determine the correct password in today’s task.

----

# ANSWER THE FOLLOWING :
----

Access the login form at http://MACHINE_IP
>>No answer needed

Configure Burp Suite & Firefox, submit some dummy credentials and intercept the request. Use intruder to attack the login form.
>>No answer needed

What valid password can you use to access the "santa" account?
>>cookie

What is the flag in Santa's itinerary?
>>THM{SANTA_DELIVERS}

----