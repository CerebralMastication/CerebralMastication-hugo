---
author: JD Long
date: 2009-04-22 19:07:09+00:00
draft: false
title: Show Me Your Hadoop
type: post
url: /2009/04/show-me-your-hadoop/
---

[caption id="" align="alignleft" width="300" caption="Nice Elephant. You recon they pimp him out for kid's parties? Or adult parties, if he can mix a good sidecar. "][![Nice Elephant. You recon they pimp him out for kids parties? Or adult parties, if he can make a good sidecar. ](http://hadoop.apache.org/core/images/hadoop-logo.jpg)
](http://hadoop.apache.org/core/)[/caption]

At some point I need to run some experiments on this platform. In the meantime, here's the news. Amazon Web Services now has a Hadoop based MapReduce service called [Amazon Elastic MapReduce](http://aws.amazon.com/elasticmapreduce/). I thought MapReduce is what happened to me when I got a Blackberry with Google Maps and GPS integration. While having a GPS in my pocket all the time (in addition to being happy to see you) changed my life, [MapReduce ](http://en.wikipedia.org/wiki/Mapreduce)may change how I work. Not that I work that hard. MapReduce is a distributed analysis method that takes a scalable problem, maps these into a bunch of little problems, blows them all out to a grid of computers where they are crunched and then reduced to an answer. Yes, I know that is not literally how it is done, but save that for someone who cares. That explanation is good enough for this blog where we run fast and loose with facts and women. I read the 2004 [Google paper on MapReduce ](http://labs.google.com/papers/mapreduce.html)not too long ago. My basic conclusion was, "That's great. I am looking forward to the day when my version of Excel or R simply has that built in. I think we are pretty dang close to that point, at least for R.

Today I ran across a [blog post over at the Learn AWS blog ](http://learnaws.com/archives/162)where Eric Idontgivemylastname Lee gives a great little tutorial of how to use Hadoop on AWS (aka [Amazon Elastic MapReduce](http://aws.amazon.com/elasticmapreduce/)) to parse a whole bunch of text files and return some statistics about the contents of these files. His example is in Ruby, but he explicitly says the process supports R. Holy Mother of MapReduce! I wonder well some of my R code will scale on there? I need to work up a problem that should scale and see if I can get it to run on this utility.

As a total aside, yes, I know that I have not blogged in a while. I did, however, move my whole family (including three generations of women) from Virginia to Chicago. I just recently sobered up gotten caught up enough to post some drivel on this blog.
