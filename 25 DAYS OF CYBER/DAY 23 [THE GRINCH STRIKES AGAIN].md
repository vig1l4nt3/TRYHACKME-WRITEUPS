DAY 23 - THE GRINCH STRIKES AGAIN [BLUE TEAMING]
----

# STORY :
----

>>The mayhem at Best Festival Company continues. McEager receives numerous emails and phone calls about a possible ransomware attack affecting all the endpoints in the network. McEager knows that the endpoints which are infected with the malware don't have any backup copies but luckily on his workstation he has backups enabled.

>>Task: Investigate the malware and restore the files to their original state.

----

# CREDS :
----

For Server provide (MACHINE_IP) as the IP address provided to you for the remote machine. The credentials for the user account is:
   * User name: administrator
   * User password: sn0wF!akes!!!

----

# TASKS :
----

>>Ransomware is a real threat that enterprise defenders and casual computer users need to defend & prepare against. According to Wikipedia, ransomware is a type of malware that threatens to publish the victim's data or perpetually block access to it unless a ransom is paid. It can be a frightening experience to log into a machine only to realize that malware has encrypted all of your important documents.

>>There are numerous security products that can be implemented in the security stack to catch this type of malware. If ransomware infects an endpoint, depending on the actual malware, there might be a decryptor made available by a security vendor. If not then you must rely on backups in order to restore your machines to the last working state, along with its files. Windows has a built-in feature that can assist with that.

>>The Volume Shadow Copy Service (VSS) coordinates the actions that are required to create a consistent shadow copy (also known as a snapshot or a point-in-time copy) of the data that is to be backed up.  (official definition)

>>Malware writers know of this Windows feature and write code in their malware to look for these files and delete them. Doing so makes it impossible to recover from a ransomware attack unless you have an offline/off-site backup. Not all malware deletes the volume shadow copies though.

>>Before diving into VSS on the endpoint let's talk briefly regarding the Task Scheduler.

>>The Task Scheduler enables you to automatically perform routine tasks on a chosen computer. Task Scheduler does this by monitoring whatever criteria you choose (referred to as triggers) and then executing the tasks when those criteria are met. (official definition)

>>Malware writers might have the malware create a scheduled task in order for the malware to run at a specific desired day/time or trigger. The Task Scheduler utility has been conveniently been placed in the taskbar for you. To view, the scheduled tasks click on Task Scheduler Library.  You should see 2 scheduled tasks of interest: 1 with a weird name and the other related to VSS. Click on any of the scheduled tasks to populate more information about it, such as Triggers and Actions.

>>At this point you should realize that VSS is enabled and thanks to the scheduled task you know the ID of the volume.

>>The command to interact with VSS is vssadmin. Running the command alone will display brief information on how to run the utility with additional commands. Two commands of particular interest are List Volumes and List Shadows.

>>If you run vssadmin list volumes you will see that the C:\ drive has a different volume name/id. There must be another volume on the endpoint.

>>You can use Disk Management to check into that. Disk Management is a system utility in Windows that enables you to perform advanced storage tasks. (official definition) As with the other utilities, Disk Management has been placed in the taskbar for your convenience.

>>As you can see there is another volume but you're unable to view it within Windows Explorer. Right-click the partition to view its properties. Now, look at the Security tab. Confirm that the volume name/id from the Task Scheduler and vssadmin output is similar to the object name of this partition.  Also, notice there is a tab titled Shadow Copies. Review the information and close the Properties window.

>>In order to see this partition within Windows Explorer, you must assign it a drive letter. Right-click the partition and select Change Drive Letter and Paths.  Click Add.  In the dropdown choose a letter, such as Z, and click OK.  At the top, in the Volume column, you should now see that the partition has a letter assigned to it. Open Windows Explorer to navigate to the partition.

>>In a previous challenge, you managed to view hidden content in folders via the command-line. You can do the same within Windows Explorer. In the menu, select View, and checkmark  Hidden Items. You should now see any hidden content right within Windows Explorer.

>>Back to VSS, to restore files to a previous version, simply right-click the folder and select Properties then select the Previous Versions tab.  Select which shadow copy you would like to restore and click the Restorebutton. Accept the confirmation to restore the shadow copy. Close the Properties window and drill into the folder to find the restore file(s).

----

# MY DOCUMENTATION :
----

>>THIS IS THE RANSOME NOTE FOUND IN THE DESKTOP.
```RANSOME-NOTE
As you were calmly looking at your documents I encrypted all the workstations at Best Festival Company just now. Including yours McEager! Send me lots and lots of money to my bitcoin address (bm9tb3JlYmVzdGZlc3RpdmFsY29tcGFueQ==) and MAYBE I'll give you the key to decrypt. >:^p
```
```
ENCODED : bm9tb3JlYmVzdGZlc3RpdmFsY29tcGFueQ==
DECODED : nomorebestfestivalcompany
```


>>TO CHECK THE VSS(VOLUME SHADOW COPY SERVICE) USE "vssadmin" TO INTERACT WITH IT.
>>THEN LIST THE SHADOWS AND LIST THE VOLUMES.
>>TAKE THE VOLUME-ID FROM THE LIST SHADOWS

```bash

nomorebestfestivalcompany


C:\Users\Administrator>vssadmin
vssadmin 1.1 - Volume Shadow Copy Service administrative command-line tool
(C) Copyright 2001-2013 Microsoft Corp.

Error: Invalid command.

---- Commands Supported ----

Add ShadowStorage     - Add a new volume shadow copy storage association
Create Shadow         - Create a new volume shadow copy
Delete Shadows        - Delete volume shadow copies
Delete ShadowStorage  - Delete volume shadow copy storage associations
List Providers        - List registered volume shadow copy providers
List Shadows          - List existing volume shadow copies
List ShadowStorage    - List volume shadow copy storage associations
List Volumes          - List volumes eligible for shadow copies
List Writers          - List subscribed volume shadow copy writers
Resize ShadowStorage  - Resize a volume shadow copy storage association
Revert Shadow         - Revert a volume to a shadow copy
Query Reverts         - Query the progress of in-progress revert operations.

C:\Users\Administrator>vssadmin list shadows
vssadmin 1.1 - Volume Shadow Copy Service administrative command-line tool
(C) Copyright 2001-2013 Microsoft Corp.

Contents of shadow copy set ID: {c7f55517-7b7c-4acc-b1f6-17fb21813a89}
   Contained 1 shadow copies at creation time: 12/11/2020 7:57:29 AM
      Shadow Copy ID: {733df35e-205b-4ea5-b5d9-efda871cc471}
         Original Volume: (\\?\Volume{7a9eea15-0000-0000-0000-010000000000}\)\\?\Volume{7a9eea15-0000-0000-0000-010000000000}\
         Shadow Copy Volume: \\?\GLOBALROOT\Device\HarddiskVolumeShadowCopy1
         Originating Machine: elfstation4
         Service Machine: elfstation4
         Provider: 'Microsoft Software Shadow Copy provider 1.0'
         Type: ClientAccessible
         Attributes: Persistent, Client-accessible, No auto release, No writers, Differential

C:\Users\Administrator>vssadmin list volumes
vssadmin 1.1 - Volume Shadow Copy Service administrative command-line tool
(C) Copyright 2001-2013 Microsoft Corp.

Volume path: \\?\Volume{f801713f-0000-0000-0000-100000000000}\
    Volume name: \\?\Volume{f801713f-0000-0000-0000-100000000000}\
Volume path: \\?\Volume{7a9eea15-0000-0000-0000-010000000000}\
    Volume name: \\?\Volume{7a9eea15-0000-0000-0000-010000000000}\
Volume path: C:\
    Volume name: \\?\Volume{f801713f-0000-0000-0000-602200000000}\    
```
>>NOW GO TO THE TASK SCHEDULER TO CHECK THE TASKS THAT ARE SCHEDULED.
>>THEN GO TO THE DISK MANAGEMENT TO SEE THE PARTITION CONTENTS THERE IT CONTAINS C:\,BACKUPS .
>>SINCE THE BACKUPS FOLDER DOES NOT HAVE THE DRIVE LRTTER WE CAN'T OPEN IT.SO RIGHT CLICK AND SELECT CHANGE DRIVE LETTER AND PATH.THEN CLICK ADD THEN CLICK THE ASSIGN THE FOLLOWING DRIVE LETTER AND ADD LETTER FROM 'A' TO 'Z'.SELECT ANY LETTER AS YOU WANT AND CLICK OK. NOW OUR DRIVE HAS BEEN SAVED.NOW WE CAN OPEN IT GO TO THE FILE BROWSER AND GO TO THE BACKUPS FOLDER AND THERE ARE TWO "DATABASE" AND "VSTOCKINGS".BUT THERE IS HIDDEN FILE TO SEE THAT GO TO VIEW AND SELECT HIDDEN TO VIEW IT.
>>NOW THERE IS A FILE "CONFIDENTIAL" FILE GO TO THAT FILE AND THERE WE GOT OUR "master-password.txt.grinch" ,CLICK ON THAT FILE IT WILL CONVERT FROM "txt.grinch" TO ".txt" TO GET THE PASSWORD .       



---- 

# ANSWER THE FOLLOWING :
----

Decrypt the fake 'bitcoin address' within the ransom note. What is the plain text value?
>>nomorebestfestivalcompany

At times ransomware changes the file extensions of the encrypted files. What is the file extension for each of the encrypted files?
>>.grinch

What is the name of the suspicious scheduled task?
>>opidsfsdf

Inspect the properties of the scheduled task. What is the location of the executable that is run at login?
>>C:\Users\Administrator\Desktop\opidsfsdf.exe

There is another scheduled task that is related to VSS. What is the ShadowCopyVolume ID?
>>7a9eea15-0000-0000-0000-010000000000

Assign the hidden partition a letter. What is the name of the hidden folder?
>>confidential

Right-click and inspect the properties for the hidden folder. Use the 'Previous Versions' tab to restore the encrypted file that is within this hidden folder to the previous version. What is the password within the file?
>>m33pa55w0rdIZseecure!

----