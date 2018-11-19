---
author: JD Long
date: 2010-07-16 22:07:12+00:00
draft: false
title: Starting an EC2 Machine Then Setting Up a Socks Proxy... From R!
type: post
url: /2010/07/starting-an-ec2-machine-then-setting-up-a-socks-proxy-from-r/
tags:
- aws
- ec2
- proxy
- R
---

[![](https://www.cerebralmastication.com/wp-content/uploads/2010/07/firewallkat.jpg)
](https://www.cerebralmastication.com/wp-content/uploads/2010/07/firewallkat.jpg)I do some work from home, some work from an office in Chicago and some work on the road. It's not uncommon for me to want to tunnel all my web traffic through a VPN tunnel. In one of my previous blog posts I [alluded to using Amazon EC2 as a way to get around your corporate IT](https://www.cerebralmastication.com/2009/11/using-amazon-ec2-to-thwart-crappy-internal-it-services/) mind control voyeurs service providers. This tunneling method is one of the 5 or so ways I have used EC2 to set up a tunnel. I used to fire these tunnels up manually using the [Amazon AWS Management Console](https://console.aws.amazon.com) then opening a shell prompt and entering:


<blockquote>

>     
>     ssh -i ~/MyPersonalKey.pem -D 9999 root@ec2-184-73-41-72.compute-1.amazonaws.com
> 
> 
</blockquote>


the -i switch tells ssh to use my RSA identity file stored in ~/MyPersonalKey.pem

the machine name (ec2-184-73-41-72.compute-1.amazonaws.com) I get from the AWS Management Console

the -D is the magic. -D opens an dynamic port forwarding tunnel between my Linux box and the EC2 machine. This is, for all intent and purposes, an encrypted SOCKS4 proxy on port 9999 of localhost. Then I just have to change my proxy settings in Firefox to use use a SOCKS host.

Now that's all pretty easy. And I like easy. But it's not easy ENOUGH. You see, I'm lazy. I'm not just lazy in the "I'll do it mañana" sort of way, but in the "I'm too damn lazy to click my mouse 5 times" way.

So I want this easier. Well, I can make the proxy settings in Firefox easier through the use of the [Quick Proxy extension for Firefox](https://addons.mozilla.org/en-US/firefox/addon/1557/). That's a good start. It turns on and off the proxy with a single mouse click. But I still have to go into the AWS management web site, fire up a machine then log in via SSH. Let's make that part easier!

While it's not simple to install and configure, the EC2 command line tools are going to be required in order to make a script that fires up EC2 and then connects to the instance with ssh. I struggled getting the tools to run until I found [this tutorial](http://linuxsysadminblog.com/2009/06/howto-get-started-with-amazon-ec2-api-tools/).

Your file locations and names may be different than the tutorial. Change appropriately. I followed the tutorial instructions but I created a key named ec2ApiTools which will come in handy later.

After you get the EC2 tool up and running and you can do something like list the available AMIs without an error you can stop with the tutorial. I've been doing a lot of shell scripting lately so I said to myself, "Self, let's script the ssh connection in R!" For the record, I always end my impredicative in an explanation point which I verbally pronounce as, "BANG!" As a result, when I talk to myself it sounds like two 10 year old boys playing cops and robbers. Anyhow, I did script it with R using Rscript. Because I'm a man who listens to myself.

And since you were kind enough to slog through my channeling the drunken ghost of James Joyce, here's my script:

[gist id="478930"]

If you're reading this in an RSS reader of for some other reason don't see an R script above, [here's your link](http://gist.github.com/478930#file_start_ec2_instance_ssh.r).

The only two EC2 API commands I use in the script are  _ec2-run-instances_ which starts the instance and _ec2-describe-instances_ which gives me a list of running instances and their details.The rest of the script is simply parsing the output and figuring out which instances was started last.

I've now set up a launcher panel item that starts the script. Then when I see the xterm window come up I click the little red button in the lower right corner of my browser which switches on the Firefox proxy. Then I'm safe to surf [Soldier of Fortune Magazine](http://www.sofmag.com/) without the interference of my corp firewall.
