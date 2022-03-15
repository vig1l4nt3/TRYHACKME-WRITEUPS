Task 1  Introduction and objectives :
----

Read and understand the information
>No need to answer

----

Task 2  How do we load websites? :
----

What request verb is used to retrieve page content?
>GET

What port do web servers normally listen on?
>80

What's responsible for making websites look fancy?
>CSS

----

Task 3  More HTTP - Verbs and request formats :
----

What verb would be used for a login?
>POST

What verb would be used to see your bank balance once you're logged in?
>GET

Does the body of a GET request matter? Yea/Nay
>NAY

What's the status code for "I'm a teapot"?
>418

What status code will you get if you need to authenticate to access some content, and you're unauthenticated?
>401

----

Task 4  Cookies, tasty! :
----

Read and try and understand this information. Check out the link for extra information
>No need to answer

----

Task 5  Mini CTF :
----

What's the GET flag?
>thm{162520bec925bd7979e9ae65a725f99f} [USING FIREFOX /ctf/get]

What's the POST flag?
>thm{3517c902e22def9c6e09b99a9040ba09} [USING CURL curl -X POST http://MACHINE-IP:8081/ctf/post --data flag_please]

What's the "Get a cookie" flag?
>thm{91b1ac2606f36b935f465558213d7ebd} [USING FIREFOX /ctf/getcookie and viewing the storage in the developer tools to see the cookie]
 
What's the "Set a cookie" flag?
>thm{c10b5cb7546f359d19c747db2d0f47b3} [USING CURL curl -v --cookie 'flagpls=flagpls' http://MACHINE-IP:8081/ctf/sendcookie]

----		