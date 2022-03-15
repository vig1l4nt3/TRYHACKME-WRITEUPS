#DNS-IN-DETAIL :
----

Task 1  What is DNS? :
----

What does DNS stand for?
>DOMAIN NAME SYSTEM 

----

Task 2  Domain Hierarchy :
----

What is the maximum length of a subdomain?
>63

Which of the following characters cannot be used in a subdomain ( 3 b _ - )?
>_

What is the maximum length of a domain name?
>253

What type of TLD is .co.uk?
>ccTDL

----

Task 3  Record Types :
----

What type of record would be used to advise where to send email?
>MX

What type of record handles IPv6 addresses?
>AAAA

----

Task 4  Making A Request :
----

What field specifies how long a DNS record should be cached for?
>TTL

What type of DNS Server is usually provided by your ISP?
>RECURSIVE

What type of server holds all the records for a domain?
>AUTHORITATIVE

----

Task 5  Practical :
----

What is the CNAME of shop.website.thm?
>shops.myshopify.com

What is the value of the TXT record of website.thm?
>THM{7012BBA60997F35A9516C2E16D2944FF}

What is the numerical priority value for the MX record?
>30

What is the IP address for the A record of www.website.thm?
>10.10.10.10

-------------------------------------------------------------------------------------------

#HTTP-IN-DETAILS :
----

Task 1  What is HTTP(S)? :
----

What does HTTP stand for?
>HYPER TEXT TRANSFER PROTOCOL

What does the S in HTTPS stand for?
>SECURE

On the mock webpage on the right there is an issue, once you've found it, click on it. What is the challenge flag?
>THM{INVALID_HTTP_CERT}

----

Task 2  Requests And Responses :
----

What HTTP protocol is being used in the above example?
>HTTP/1.1

What response header tells the browser how much data to expect?
>CONTENT-LENGTH

----

Task 3  HTTP Methods :
----

What method would be used to create a new user account?
>POST

What method would be used to update your email address?
>PUT

What method would be used to remove a picture you've uploaded to your account?
>DELETE

What method would be used to view a news article?
>GET

----

Task 4  HTTP Status Codes :
----

What response code might you receive if you've created a new user or blog post article?
>201

What response code might you receive if you've tried to access a page that doesn't exist?
>404

What response code might you receive if the web server cannot access its database and the application crashes?
>503

What response code might you receive if you try to edit your profile without logging in first?
>401

----

Task 5  Headers :
----

What header tells the web server what browser is being used?
>USER-AGENT

What header tells the browser what type of data is being returned?
>CONTENT-TYPE

What header tells the web server which website is being requested?
>HOST

----

Task 6  Cookies :
----

Which header is used to save cookies to your computer?
>SET-COOKIE

----

Task 7  Making Requests :
----

Make a GET request to /room
>THM{YOU'RE_IN_THE_ROOM}

Make a GET request to /blog and using the gear icon set the id parameter to 1 in the URL field
>THM{YOU_FOUND_THE_BLOG}

Make a DELETE request to /user/1
>THM{USER_IS_DELETED}

Make a PUT request to /user/2 with the username parameter set to admin
>THM{USER_HAS_UPDATED}

POST the username of thm and a password of letmein to /login
>THM{HTTP_REQUEST_MASTER}

-------------------------------------------------------------------------------------------

#HOW WEBSITE WORKS :
----

Task 1 – How Websites Work :
----

What term best describes the side your browser renders a website?
>Client side

----

Task 2 – HTML :
----

Let’s play with some HTML! On the right-hand side, you should see a box that renders HTML – If you enter some HTML into the box and click the green “Render HTML Code” button, it will render your HTML on the page; you should see an image of some cats.
>No answer needed

One of the images on the cat website is broken – fix it, and the image will reveal the hidden text answer!
>HTMLHERO

Add a dog image to the page by adding another img tag (<img>) on line 11. The dog image location is img/dog-1.png
>DOGHTML

----

Task 3 – Javascript :
----

Click the “View Site” button on this task. On the right-hand side, add JavaScript that changes the demo element’s content to “Hack the Planet”
>JSISFUN

Add the button HTML from this task that changes the element’s text to “Button Clicked” on the editor on the right, update the code by clicking the “Render HTML+JS Code” button and then click the button.
>No answer needed

----

Task 4 – Sensitive Data Exposure :
----

View the website on this task. What is the password hidden in the source code?
>testpasswd

----

Task 5 – HTML Injection :
----

View the website on this task and inject HTML so that a malicious link to http://hacker.com is shown.
>HTML_INJ3CTI0N

-------------------------------------------------------------------------------------------

#PUTTING IT ALL TOGETHER :
----

Task 1 – Putting It All Together :
----

I’ve read this…
>No need to answer

----

Task 2 – Other Components :
----

What can be used to host static files and speed up a clients visit to a website?
>CDN

What does a load balancer perform to make sure a host is still alive?
>health 

What can be used to help against the hacking of a website?
>WAF

----

Task 3 – How Web Servers Work :
----

What does web server software use to host multiple sites?
>VIRTUAL

What is the name for the type of content that can change?
>DYNAMIC

Does the client see the backend code? Yay/Nay
>NAY

----

Task 4 – Quiz :
----

Flag
>THM{YOU_GOT_THE_ORDER}

-------------------------------------------------------------------------------------------