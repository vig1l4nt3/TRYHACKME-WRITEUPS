DAY 1 - A CHRISTMAS CRISIS [WEB EXPLOITAITON] 
----

# THE WEB :
----

>>>The Internet is one of those things that everyone uses, but few people bother to learn about. As hackers, it is vital that we understand what exactly the web is, and how it works.

>>When you open up your web browser and navigate to a website, it seems so simple, but what is really happening behind the scenes?
>>First of all, your computer communicates with a known DNS (Domain Name System) server to find out where the website can be found on the internet. 
>>The DNS server will then return an IP address for the remote server. This can be used to go directly to the website. 
>>You can think of the internet as being quite like the planet itself -- we have lots of locations, all over the world. These places all have a street address -- this is akin to the domain name of a website (i.e. tryhackme.com, or google.com); but they also have co-ordinates which can be used to pinpoint their location with absolute accuracy. These co-ordinates are like the IP address of a website.
>>If you know the street address of a location, you can enter it into Google Maps and be given the exact coordinates, which can then be put into a SatNav to take you there with pinpoint accuracy!
>>In the same way, your browser is given the address of a website (i.e. tryhackme.com). It sends this address off to a DNS server, which tells it the "co-ordinates" (the IP address) of the site. 
>>Your computer doesn't understand the original, human-readable domain name, but it does understand what an IP address is! The IP can then be used to find the server across the internet, allowing your computer to request the content of the website. Of course, in reality, this is a highly simplified analogy, so a more in-depth explanation of this process can be found here.

----

# HTTP(S) :
----

>>>Once your computer knows where it can find the target website, it sends something called a HTTP (Hypertext Transfer Protocol) request to the webserver.

>>This is just a standard network request, but it is formatted in a way that both your web browser and the server can understand. 
>>In practice, this means adding certain "headers" to the request which identify it as a HTTP request, and tell the server a variety of other information about the request, as well as your own browser. 
>>Amongst many other headers, HTTP requests always have a method and a target. 
>>These specify what to retrieve from the server (the target), and how to retrieve it (the method). 
>>The method most commonly used to retrieve information is called the GET method. 
>>When sending data to the server, it's more common to use a method called POST.
>>Once the content has been retrieved from the server, your browser reads the retrieved code and renders it as a web page. 
>>This usually means taking the layout of the page from a HTML (Hyper Text Markup Language) document, styling it with a connected CSS (Cascading Style Sheets) file, then adding any dynamic content with one or more connected Javascript files.
>>HTTP has one inherent disadvantage: namely, it is not secure. Anyone can see what you're requesting, and what's being sent back to you. 
>>For this reason, HTTPS (Hypertext Transfer Protocol Secure) was invented. This works in exactly the same way as standard HTTP but provides an encrypted connection (the functionality of which is beyond the level of this dossier)

----

# COOKIES 
----

>>>HTTP is an inherently stateless protocol. This means that no data persists between connections; your computer could make two requests immediately after each other, and, without relying on separate software, the web server would have no way to know that it was you making both the requests. This begs the important question: if HTTP is stateless, then how do login systems work? 

>>The web server must have a way to identify that you have the right level of access, and it can hardly ask you to enter your password every time you request a new page!
>>The answer is cookies -- tiny little pieces of information that get stored on your computer and get sent to the server along with every request that you make. 
>>Authentication (or session) cookies are used to identify you (these will be very important in your mission today!). The server receives your request with the attached cookie, and checks the cookie to see what level of access you are allowed to have. It then returns a response appropriate to that level of access.
>>For example: A standard user should be able to see (but not interact with) our control panel; but Santa should be able to access everything! 
>>Cookies are also often used for other purposes such as advertising and storing user preferences (light/dark theme, for example); however, this will not be important in your task today. Any site can set cookies with a variety of properties -- the most important of these for today's task are the name and value of the cookies, both of which will always be set. 
>>It's worth noting that a site can only access cookies that are associated with its own domain (i.e. google.com can't access any cookies stored by tryhackme.com, and vice versa).
>>It's important to note that cookies are stored locally on your computer. This means that they are under your control -- i.e. you can add, edit, or delete them as you wish. 
>>There are a few ways to do this, however, it's most commonly done by using your Browser Developer Tools, which can be accessed in most browsers by pressing F12, or Ctrl + Shift + I. With the developer tools open, navigate to the Storage tab in FireFox, or the Application tab in Chrome/Edge and select the Cookies menu on the left hand side of the console.
>>In the above image you can see a test cookie for a website. The important attributes "Name" and "Value" are shown. 
>>The name of a cookie is used to identify it to the server. 
>>The value of the cookie is the data stored by the server. In this example the server would be looking for a cookie called "Cookie Name". It would then retrieve the value "CookieValue" from this cookie.
>>These values can be edited by double-clicking on them, which is great if you can edit a session or authorisation cookie, as this can lead to an escalation of privileges, assuming you have access to an Administrator's authorisation cookie.

----

## MY DOCUMENTATION :
----
>>FIRST LOGIN INTO THE PAGE AND REGISTERED WITH A USERNAME AND PASSWORD
USERNAME : prathap
PASSWORD : 12345
>>AFTER LOGING IN WITH THOSE CREDENTIALS, THE HOME PAGE HAS SOME CONTENTS BUT THERE ARE NOT ACCESSIBLE.
>>THEN GOING TO WEB DEVELOPER TOOLS TO SEE THE COOKIE INFORMATION THE NAME AND VALUE OF COOKIES WERE PRESENT. 
NAME  : auth
VALUE : 7b22636f6d70616e79223a22546865204265737420466573746976616c20436f6d70616e79222c2022757365726e616d65223a2270726174686170227d
>>THE COOKIE VALUE IS ENCODED IN HEXADECIMAL FORMAT AND DATA STORED IN JSON FORMAT. SO I DECODED THE VALUE WITH THE HEXADECIMAL TO TEXT CONVERTER.IT DISPLAYS 
DECODED COOKIE : {"company":"The Best Festival Comapny","username":"prathap"} 
>>TO BYPASS THE AUTHENTICATION WE NEED TO MANIPULATE THE COOKIE. SO WE NEED THE DECODED COOKIE DETAIL TO EDIT THE USERNAME AS "SANTA". AND AGAIN DECODE THE EDITED COOKIE TO HEXADECIMAL FORMAT. 
DECODED COOKIE : {"company":"The Best Festival Comapny","username":"santa"}   
ENCODED COOKIE : 7b22636f6d70616e79223a22546865204265737420466573746976616c20436f6d61706e79222c22757365726e616d65223a2273616e7461227d
>>NOW GO BACK TO THE LOGIN PAGE AND OPEN THE WEB DEVELOPER OPTION AND GO TO STORAGE AND OPEN COOKIES.THE COOKIES WILL BE BLANK, TO CREATE A NEW COOKIE THERE IS A ADD BUTTON.CREATE A NEW COOKIE ,THE GENERATED COOKIE IS A RANDOM COOKIE ,WE NEED TO EDIT THE NAME AS "auth" AND DELETE THE RANDOM COOKIE VALUE AND PASTE THE SANTA'S COOKIE VALUE, AND RELOAD THE PAGE.    
>>NOW WE BECAME AS ROOT [i.e SANTA] NOW THE HOME PAGE WILL DISPLAY ALL THE CONTENTS TO ACCCESS.NOW ENABLE ALL THE OPTIONS AT THE END OF THE PAGE THE "FLAG" WILL APPEAR.

----

# ANSWER THE FOLLOWING : 
----
1.Deploy your AttackBox (the blue "Start AttackBox" button) and the tasks machine (green button on this task) if you haven't already. Once both have deployed, open FireFox on the AttackBox and copy/paste the machines IP into the browser search bar.
>>No answer needed

Register for an account, and then login.
2.What is the name of the cookie used for authentication?
>>auth

3.In what format is the value of this cookie encoded?
>>Hexadecimal

4.Having decoded the cookie, what format is the data stored in?
>>Json

Figure out how to bypass the authentication.
5.What is the value of Santa's cookie?
>>7b22636f6d70616e79223a22546865204265737420466573746976616c20436f6d61706e79222c22757365726e616d65223a2273616e7461227d

Now that you are the santa user, you can re-activate the assembly line!
What is the flag you're given when the line is fully active?
>>THM{MjY0Yzg5NTJmY2Q1NzM1NjBmZWFhYmQy}

----