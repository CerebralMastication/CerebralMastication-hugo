---
author: JD Long
date: 2010-06-28 15:38:42+00:00
draft: false
title: Bootstrapping the latest R into Amazon Elastic Map Reduce
type: post
url: /2010/06/bootstrapping-the-latest-r-into-amazon-elastic-map-reduce/
tags:
- EMR
- R
---

[![](https://www.cerebralmastication.com/wp-content/uploads/2010/06/boot.jpg)
](https://www.cerebralmastication.com/wp-content/uploads/2010/06/boot.jpg)I've been continuing to muck around with using R inside of Amazon Elastic Map reduce jobs. I've been working on abstracting the lapply() logic so that R will farm the pieces out to Amazon EMR. This is coming along really well, thanks in no small part to the [Stack Overflow [r] ](http://stackoverflow.com/questions/tagged/r)community. I have no idea how crappy coders like me got anything at all done before the Interwebs.

One of the immediate hurdles faced when trying to use AMZN EMR in anger is that the default version of R on EMR is 2.7.1. Yes, that is indeed the version that Moses taught the Israelites to use while they wandered in the desert. I'm impressed by your religious knowledge. At any rate, all kinds of things go to hell when you try to run code and load packages in 2.7.1. When I first started fighting with EMR the only solution was to backport my code and alter any packages so they would run in 2.7.1. Yes, that is, as Moses would say, a Nudnik. Nudnik also happens to be the pet name my neighbors have given me. They love me. Where was I? Oh yeah, Methusla's R version. Recently Amazon released a neat feature called "Bootstrapping" for EMR. Before you start thinking about sampling and resampling and all that  crap, let me clarify. This is NOT statistical bootstrapping. It's called bootstrapping because it's code that runs after each node boots up, but before the mapper procedure runs. So to get a more modern version of R loaded on to each node I set up a little script that updates the sources.list file and then installs the latest version of R. And since I'm a caring, sharing guy, here's my script:

[gist id="455962"]

And if that doesn't show up for some reason, you can find all[ 5 lines of its bash glory here over at github](http://gist.github.com/455962).

If you're not conveniently located in Chicago, IL you may want to change your R mirror location. The bootstrap action can be set up from the EMR web GUI or if you're firing the jobs off using the elastic-mapreduce command line tools you just add the following option: "--bootstrap-action s3://myBucket/bootstrap.sh" assuming myBucket is the bucket with your script in it and bootstrap.sh contains your bootstrap shell script. And then, as my buddies in Dublin say, "Bob's your mother's brother."

And before you ask, yes, this slows crap down. I'll probably hack together a script that will take the R binaries and other needed upgrades out of Amazon S3 and load them in a bootstrap action which will greatly speed things up. The above example has one clear advantage over loading binaries from S3: It works right now. And remember folks, code that works right now kicks code that "might work someday" right in the balls. And then mocks it while it cries.
