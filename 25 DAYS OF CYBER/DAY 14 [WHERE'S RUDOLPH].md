DAY 14 - WHERE'S RUDOPLH? [OSINT]
----

# STORY :
----

'Twas the night before Christmas and Rudolph is lost
Now Santa must find him, no matter the cost
You have been hired to bring Rudolph back
How are your OSINT skills? Follow Rudolph's tracks...

----

# Task 1
----

While hunting and searching for any hints or clues
Santa uncovers some details and shares the news
Rudolph loved to use Reddit and browsed aplenty
His username was 'IGuidetheClaus2020'

>>Many OSINT investigations start with only a username. A user's posting history can possibly lead to further information. Sometimes, it's the smallest of clues that help us out. Comb through Rudolph's Reddit history and answer questions #1-5 below. You may need to use partial clues with a search engine to fill in the gaps.

----

## Learning Objectives:

1) Identify important information based on a user's posting history.
2) Utilize outside resources, such as search engines, to identify additional information, such as full names and additional social media accounts.

----

## Additional Resources:
----

>>While Rudolph's posting history is enough for us to identify that he has other social media accounts, sometimes we are not that lucky. Great tools exist that allow us to search for user accounts across social media platforms. Sites, such as https://namechk.com/, https://whatsmyname.app/ and https://namecheckup.com/ will identify other possible accounts quickly for us. Tools, such as https://github.com/WebBreacher/WhatsMyName and https://github.com/sherlock-project/sherlock do this as well. Simply enter a username, hit search, and comb through the results. It's that easy!

----

# Task #2
----

Well it looks like you have uncovered Rudolph's Twitter
Now we can read into all of his chitter
Go through his profile and give it some views
The deeper you dig, the better the clues

>>By finding another account belonging to our user, we open up the possibility of gathering even more information. Utilize the information found on Rudolph's Twitter account to answer questions #6-11.

----

## Learning Objectives:
----

1) Identify important information based on a user's posting history.
2) Use reverse image searching to identify where a photo was taken and possibly identify additional information, such as other user accounts.
3) Utilize image EXIF data to uncover critical details, such as exact photo location, camera make and model, the date the photo was taken, and more.
4) Use discovered emails to search through breached data to possibly identify user passwords, name, additional emails, and location.

----

## Additional Resources

>>This task was created to identify common critical steps in an OSINT investigation. Reverse image searching can help not only identify where an image was taken, but it can assist with identifying websites where that photo exists as well as similar photos (possibly from the same photoset), which can be incredibly useful in an investigation. While Google Images is used in our example, other sites should also be utilized to be as thorough as possible. No one site is perfect when it comes to reverse image searching (or any tool for that matter). Sites like https://yandex.com/images/ , https://tineye.com/ and https://www.bing.com/visualsearch?FORM=ILPVIS are great as well. Additionally, do not neglect the possibility of EXIF data existing in an image. While a lot of sites strip this data, not all do. It never hurts to look and can provide a wealth of information when the data is still there.

>>Finally, breached data can be incredibly useful from an investigative standpoint. Breach data does not just include passwords. It often has full names, addresses, IP information, password hashes, and more. We can often use this information to tie to other accounts. For example, say we find an account with the email of v3ry1337h4ck3r@gmail.com. If we search that email for breached data, we might find a password or hash associated with it. If unique enough, we can search that password or hash in a breach database and use it to identify other possible accounts. We can do the same with usernames, IPs, names, etc. The possibilities are vast and one email address can lead to a slew of information.

>>Websites such as https://haveibeenpwned.com/ will help identify if an account has ever been breached and will, at a minimum, inform us if an account existed at one point. However, it does not provide any password information. Free sites such as http://scylla.sh/ will provide password information and are easy to search through. The data on free sites can tend to be older and not up to date with the latest breach information, but these sites are still a powerful resource. Lastly, paid sites such as https://dehashed.com/ offer up to date information and are easily searchable at affordable rates.

----

# Wrapping Up
----

It looks like finding Rudolph was a bit too easy
His OPSEC would make any security pro queasy
To the Windy City, Rudolph was tracked
Christmas is saved, we brought Rudolph back

----

# MY DOCUMENTATION :
----

>>JPG  :https://tcm-sec.com/wp-content/uploads/2020/11/lights-festival-website.jpg

>>TOOL :http://exif.regex.info/exif.cgi

```INFORMATION-FOUND-WITH-EXIF-TOOL
Basic Image Information
Target image:	https://tcm-sec.com/wp-content/uploads/2020/11/lights-festival-website.jpg
Copyright:	{FLAG}ALWAYSCHECKTHEEXIFD4T4
User Comment:	Hi. :)
Location:	
Latitude/longitude:	41° 53' 30.5" North,   87° 37' 27.4" West
( 41.891815, -87.624277 )

Though the photo is not related to Jeffrey's blog, as an aside, you may want to see photos on his blog that might be near this location.

Map via embedded coordinates at: Google, Yahoo, WikiMapia, OpenStreetMap, Bing (also see the Google Maps pane below)

Timezone guess from earthtools.org: 6 hours behind GMT
File:	650 × 510 JPEG
51,161 bytes (50 kilobytes)
Color Encoding:	
WARNING: No color-space metadata and no embedded color profile: Windows and Mac web browsers treat colors randomly.
Images for the web are most widely viewable when in the sRGB color space and with an embedded color profile. See my Introduction to Digital-Image Color Spaces for more information.
```

>>EMAIL :rudolphthered@hotmail.com   

>>TOOL  :https://scylla.sh/ 
```PASSWORD
spygame
```

----

# ANSWER THE FOLLOWING :
----

1.What URL will take me directly to Rudolph's Reddit comment history?
>>https://www.reddit.com/user/IGuidetheClaus2020/comments

2.According to Rudolph, where was he born?
>>Chicago

3.Rudolph mentions Robert.  Can you use Google to tell me Robert's last name?
>>May

4.On what other social media platform might Rudolph have an account?
>>Twitter

5.What is Rudolph's username on that platform?
>>IGuideClaus2020

6.What appears to be Rudolph's favorite TV show right now?
>>Bachelorette

7.Based on Rudolph's post history, he took part in a parade.  Where did the parade take place?
>>Chicago

8.Okay, you found the city, but where specifically was one of the photos taken?
>>41.891815, -87.624277

9.Did you find a flag too?
>>{FLAG}ALWAYSCHECKTHEEXIFD4T4

10.Has Rudolph been pwned? What password of his appeared in a breach?
>>spygame

11.Based on all the information gathered.  It's likely that Rudolph is in the Windy City and is staying in a hotel on Magnificent Mile.  What are the street numbers of the hotel address?
>>540

----