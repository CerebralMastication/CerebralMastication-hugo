---
author: JD Long
date: 2010-11-30 19:51:17+00:00
draft: false
title: Controlling Amazon Web Services using rJava and the AWS Java SDK
type: post
url: /2010/11/controlling-amazon-web-services-using-rjava-and-the-aws-java-sdk/
tags:
- aws
- R
- rJava
- S3
---

![](http://awsmedia.s3.amazonaws.com/logo_aws.gif)
I've been messing around with using Amazon Web Services for a while. I've had some projects where I wanted to upload files to S3 or fire off EMR jobs. I've been controlling AWS services using a hodgepodge of command line tools and the R system() function to call the tools from the command line. This has some real disadvantages, however. Using the command line tools means each tool has to be configured individually which is painful on a new machine. It's also much harder to roll my R code up into a CRAN package because I have to check dependencies on the command line tools and ensure that the user has properly configured each tool. Clearly a pain in the ass.

So I was looking for more simple/elegant solutions. After thinking the [Boto](http://code.google.com/p/boto/) library for Python might be helpful, I realized that the easiest way to use that would be with [rJython](http://rjython.r-forge.r-project.org/) which meant having to interact with R, Python, AND Java. Considering I don't program in Python or Java, that seemed like a fair bit of complexity. Then I realized that the canonical implementation of the AWS API was the AWS Java SDK. The [rJava](http://www.rforge.net/rJava/) package makes interacting with Java from R a viable option.

Since I've never written a single line of Java code in my pathetic life, this was somewhat harder than it could have been. But with some help from [Romain Francois](http://romainfrancois.blog.free.fr/) I was able to cobble together "something that works." The code below gives a simple example of interfacing with S3. The example will look to see if a given bucket exists on S3, if not it will create the bucket. Then it will upload a single file from your PC into that bucket. You will have to [download the SDK](http://aws.amazon.com/sdkforjava/), unzip it in the location of your choice, and then change the script to reflect your configuration.

If you are running R in Ubuntu, you should install rJava using apt-get instead of using install.packages() from inside of R:


<blockquote>sudo apt-get install r-cran-rjava</blockquote>


Here's the codez. And a [direct link](https://gist.github.com/722230) for you guys reading this through an RSS reader:
[gist id="722230"]

I realize that Duncan Temple Lang has created the [RAmazonS3](http://www.omegahat.org/RAmazonS3/) package which can easily do what the above code sample does. The advantage of using rJava and the AWS Java SDK is the ability to apply the same approach to **ALL** the AWS services. And since Amazon maintains the SDK this guarantees that future AWS services and features will be supported as well.
