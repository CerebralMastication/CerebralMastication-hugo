---
author: JD Long
date: 2010-01-19 23:33:01+00:00
draft: false
title: Remote Backup Fail and How to Silently Copy Files
type: post
url: /2010/01/remote-backup-fail-and-how-to-silently-copy-files/
tags:
- backup
- batch files
- rant
- windows
---

[![](https://www.cerebralmastication.com/wp-content/uploads/2010/01/pee-on-iron-mountain.jpg)
](https://www.cerebralmastication.com/wp-content/uploads/2010/01/pee-on-iron-mountain.jpg)Recently I've run into frustrations with [Iron Mountain Connected Backup](http://backup.ironmountain.com/) so I've been looking for alternatives.

**Alternatives:** I've been running [Jungle Disk](http://www.jungledisk.com/) at home and really like it. I could use that at work except I have not set up an Amazon or RackSpace account with my work credit card. But I am in Chicago and my database server/ file server is in Dallas TX. So I decided to just create a mirror on my laptop onto a shared drive on my server. There's lots of ways to do this, but the path I chose was to use [RoboCopy, a command line copy tool from Microsoft](http://en.wikipedia.org/wiki/Robocopy) that is part of the Windows Server 2003 Resource Kit. I'm running XP and I wanted the mirroring of my machine to be invisible, silent, and scheduled. To do this I found I needed to take the following steps:



	  1. Install RoboCopy
	  2. Create a batch file to mirror the directory I wanted
	  3. Create a windows script to call the batch silently
	  4. Schedule the windows script to run automagically

**Install RoboCopy:** Download the [Windows Server 2003 Resource Kit](http://www.microsoft.com/downloads/details.aspx?familyid=9d467a69-57ff-4ae7-96ee-b18c4790cffd&displaylang=en) and install it. Very easy.

**Create a batch file to run RoboCopy**: I named mine c:/backup.bat and it looks something like this:


<blockquote>

> 
> Set Source="C:\Documents and Settings\jdlong"
> 
> 

> 
> Set Dest="\\myDallasServer\backup\jdlong"
> 
> 

> 
> Robocopy %Source% %Dest% /MIR /Z /R:0  >nul
> 
> </blockquote>


This simply sets the source and destination and then runs RoboCopy with the /MIR (mirror) and /Z (restartable) switches invoked

**Create a windows script**: The problem with the batch file is that it is noisy when it runs. Even piping the output to nul it still produces a CMD window that stays up until it finishes running. That's where the Windows Script file comes into play. It calls the batch file but hides the CMD window. I created a file called c:\runBackup.vbs that has this in it:


<blockquote>Set WshShell = CreateObject("WScript.Shell")
WshShell.Run chr(34) & "C:\backup.bat" & Chr(34), 0
Set WshShell = Nothing</blockquote>




**Schedule the windows script:** Control Panel -> Scheduled Tasks. Then I created a new task that runs  c:\runBackup.vbs every night at 11PM. The only down side is that when I change my password I have to remember to change the password associated with the scheduled task or it will fail.




The only upside is that I figured out that Iron Mountain sucks prior to having data loss. I got lucky. Next week I am going to test my backup. And then test it every quarter after that. And I won't depend on my corporate IT do to my backups.
