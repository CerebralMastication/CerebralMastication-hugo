---
author: JD Long
date: 2009-11-03 15:28:26+00:00
draft: false
title: Using Amazon EC2 to Thwart Crappy Internal IT Services
type: post
url: /2009/11/using-amazon-ec2-to-thwart-crappy-internal-it-services/
tags:
- ec2
- IT
- rant
---

[![ec2 tweet](https://www.cerebralmastication.com/wp-content/uploads/2009/11/ec2-tweet.PNG)
](http://twitter.com/CMastication/status/5294564298)

The alternative title of this blog post is "How to get your sorry ass fired by violating your internal IT policies." So keep that in mind as you read this.

I say lots of silly crap. Twitter allows me the pleasure of sharing this blather with the world. I was a little surprised that of all the things I have said over the last few months the above Tweet received the most discussion. Apparently this tweet captured the imagination and consternation of some fellow Tweeters. I had people follow up with me and basically ask, "what do you mean?" Twitter is good for a sound bite, but less so for an elaborate answer. Which brings us to this:

What are the top ways Amazon EC2 can allow a business user to escape the manipulative and counterproductive grip of corporate IT? Well I'm glad you asked!

**1) Over-restrictive web filtering policies**:  When I worked as a risk manager for a Fortune 500 insurance firm I was shocked on the first day when I could not search Google Groups. At the time Google Groups was one of my favorite resources for figuring out everything from SQL syntax to Excel formulas. The firm, like most firms, outsourced the filtering of web content. Apparently they signed up for "Super Freaking Restrictive" filtering. I could not even search the web for "Ubuntu" as all sites with the word Ubuntu in the title or with the world "Ubuntu" passed as a form submission were blocked. Apparently Ubuntu is not just a Linux distro, but also a militant organization of African computer programmers, or something. So how did I get around this with EC2? I would fire up an EC2 Ubuntu instance running Squid proxy before I left home, then ssh into the cloud from work and use a little SSH port forwarding to route my web traffic through the ssh connection and out via Squid. I set up my EC2 instance to listen for ssh on port 443 and my firm's firewall would let the connection pass as it assumed it was simply ssl traffic into Amazon. Brilliant!

**2) Under powered database servers: **At another point I was responsible for data analytics on a portfolio of insurance policies. I had to join together data from multiple systems (underwriting, admin, claims, etc.). The firm was an Oracle shop and none of the Oracle machines had enough user space for me to make the big ass join that had to be made in order to cobble together my analytics. For a while I hobbled along using PROC SQL in SAS to bring all the data together inside of SAS running on a PC. Finally I just gave up and built my own data mart in the cloud. And I could totally cut my internal IT politics out of the system. Whew, once the politics and begging for resources was over I could kick ass at analytics without having to beg borrow and plead for permissions and space.

**3) Failure to backup desktop machines / inadequate shared drive space: **Another experience I had was with a firm that decided it was a good policy to NOT back up desktop PCs at all. Each department was given shared drive space on a central server where "business critical" files were supposed to be kept (whatever the hell that means). Only the files on the central server were backed up. I was in the risk management department (ironically) and we had a whopping 100 MB allocated to us. Yes, this was 2004 and 100 MB was not enough to hold 2 years of risk reviews. Not to mention any ad hoc analysis and all the supporting documents. So everyone had their desktop drives, at least one USB drive, and no off site backup. It was during this period that I discovered [Jungle Disk ](http://www.jungledisk.com/)which allows client side encrypted data to be backed up to Amazon! Off site backup problem solved! And, once again, corp IT cut out of the system. (yes, this is a use of S3, not EC2) By the way, I paid for backups out of my own pocket because I felt it was very important. Well, I did have the firm buy me books which I happily kept when I left. We'll call it even.

Let me reiterate that all three of the above uses may have put me in direct violation of my corporate IT policies. And let me also state that ultimately I found a job at a firm where internal IT sees their job as helping the business units get crap done. If you are an IT professional and you find your self thinking, "damn, I have to make sure I restrict my users from all of these crafty uses of EC2" then, **jackass,you are the problem with your firm's IT department**. If you see your job as stopping users then you are a useless burden on your firm and you should be not only fired, but spat upon. The way to prevent users from doing these, and other "shadow IT" behaviors is to **provide the IT services that help your users be awesome!** If you do that then you don't have to worry about what your users are up to. They'll be too damn busy being awesome to have time to mess with Amazon EC2.

All the examples above took place at previous places of employment. I currently use Amazon EC2 in order to scale some of my analytics, but it is done with the knowledge and support of my internal IT team. They fully understand what I am doing and they want to help me be awesome at analysis. It's amazing how much less time I am wasting these days now that I don't have to be so creative about avoiding the manipulative and counterproductive intervention of my internal IT team.
