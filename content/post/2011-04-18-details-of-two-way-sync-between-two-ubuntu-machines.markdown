---
author: JD Long
date: 2011-04-18 20:48:32+00:00
draft: false
title: Details of two-way sync between two Ubuntu machines
type: post
url: /2011/04/details-of-two-way-sync-between-two-ubuntu-machines/
tags:
- ec2
- howto
- R
- workflow
---

[![](https://www.cerebralmastication.com/wp-content/uploads/2011/04/SyncDifferent.png)
](https://www.cerebralmastication.com/wp-content/uploads/2011/04/SyncDifferent.png)In a [previous post](https://www.cerebralmastication.com/2011/04/fast-two-way-sync-in-ubuntu/) I discussed my frustrations with trying to get Dropbox or Spideroak to perform BOTH encrypted remote backup and AND fast two way file syncing. This is the detail of how I set up for two machines, both Ubuntu 10.10, to perform two way sync where a file change on either machine will result in that change being replicated on the other machine.

I initially tried running Unison on BOTH my laptop and the server and had the server Unison set to sync with my laptop back through an SSH reverse proxy. After testing this for a while I discovered this is totally the wrong way to do it. The problem is that the Unison process makes temp directories and files in the file system of the target. So my Unison job on the laptop would be trying to syn files and, in the process, create temp files which would kick off a Unison sync on the sever which would make temp files on the laptop... I think you can see how convoluted this gets.

So a much better solution is to only run Unison from one machine (I chose my laptop) and have the other machine (server in my case) send an SSH command (over the aforementioned reverse proxy) to the laptop asking the laptop to kick off a Unison sync. This way all of the syncs happen from the laptop.

So, in short, both machines run lsyncd which monitors files for changes. I keep up an SSH tunnel with reverse port forwarding which forwards a remote machine port back to my laptop's port 22 (SSH). Unison need be installed ONLY on my laptop. When a change happens on my laptop, lsyncd fires off a Unison sync from my laptop that syncs it with the server. When a file changes on the server, the lsyncd job on the server makes a connection to my laptop via ssh and fires off a Unsion sync between my laptop and the server.

Here's an example of my lsyncd config scripts:

**Laptop:**


<blockquote>settings = {
logfile    = "/home/jal/lsyncd/laptop/lsyncd.log",
statusFile = "/home/jal/lsyncd/laptop/lsyncd.status",
maxDelays  = 15,
--nodaemon   = true,
}

runUnison2 = {
maxProcesses = 1,
delay = 15,
onAttrib  = "/usr/bin/unison -batch /home/jal/Documents ssh://12.34.56.78//home/jal/Documents",
onCreate  = "/usr/bin/unison -batch /home/jal/Documents ssh://12.34.56.78//home/jal/Documents",
onDelete  = "/usr/bin/unison -batch /home/jal/Documents ssh://12.34.56.78//home/jal/Documents",
onModify  = "/usr/bin/unison -batch /home/jal/Documents ssh://12.34.56.78//home/jal/Documents",
onMove    = "/usr/bin/unison -batch /home/jal/Documents ssh://12.34.56.78//home/jal/Documents",
}

sync{runUnison2, source="/home/jal/Documents"}</blockquote>


**Server:**


<blockquote>settings = {
logfile    = "/home/jal/lsyncd/server/lsyncd.log",
statusFile = "/home/jal/lsyncd/server/lsyncd.status",
maxDelays  = 15,
--nodaemon   = true,
}

runUnison2 = {
maxProcesses = 1,
delay = 15,
onAttrib  = "ssh localhost -p 5432 unison -batch  /home/jal/Documents ssh://12.34.56.78//home/jal/Documents",
onCreate  = "ssh localhost -p 5432 unison -batch  /home/jal/Documents ssh://12.34.56.78//home/jal/Documents",
onDelete  = "ssh localhost -p 5432 unison -batch  /home/jal/Documents ssh://12.34.56.78//home/jal/Documents",
onModify  = "ssh localhost -p 5432 unison -batch  /home/jal/Documents ssh://12.34.56.78//home/jal/Documents",
onMove    = "ssh localhost -p 5432 unison -batch  /home/jal/Documents ssh://12.34.56.78//home/jal/Documents",
}

sync{runUnison2, source="/home/jal/Documents"}</blockquote>


Keep in mind that I am using version 2 of lsyncd which can be downloaded here: [http://code.google.com/p/lsyncd/](http://code.google.com/p/lsyncd/)

The version of lsyncd available in the Ubuntu repo is version 1.x which does not use the same config format as I illustrate above. However, if you run into dependency issues with v2, the easiest thing to do is install the repo version which will install dependencies and then manually download and install v2 from the above URL.

My reverse port forwarding set up looks like this:


<blockquote>autossh -2 -4 -X -R 5432:localhost:22 12.34.56.78</blockquote>


the -R bit forwards remote port 5432 to my laptop's port 22 which is the ssh. So on my server if I run ssh localhost -p 5432 what actually happens is I am sshing from the remote machine to my laptop.

**Notes:**



	  * The IP address of my server in this example is 12.34.56.78.
	  * Don't try and sync the directories where the lsyncd logs are kept. That will results in an endless sync cycle as each machine keeps noticing changes endlessly. Don't ask me how I know this.
	  * The command to start the sync on the laptop is "lsyncd /home/jal/lsyncd/laptop/configfile" where configfile is the above lsyncd configuration file.
	  * lsyncd could, conceivably, tell Unison to sync only the part of the directory tree that changed. I have not been able to make that feature work right, however. And it only takes Unison a few seconds to sync, so I've not worried about it.

This has greatly sped up my [RStudio](http://rstudio.org) based workflow when doing analysis with R. Now when I change files on my server using RStudio they are immediately (well it waits 15 seconds) replicated to my local machine and vice versa!

Good luck and if you have any suggestions please post a comment!
