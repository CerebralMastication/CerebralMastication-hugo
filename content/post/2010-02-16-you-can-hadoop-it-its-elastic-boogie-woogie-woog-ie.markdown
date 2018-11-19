---
author: JD Long
date: 2010-02-16 18:31:23+00:00
draft: false
title: You can Hadoop it! It's elastic! Boogie woogie woog-ie!
type: post
url: /2010/02/you-can-hadoop-it-its-elastic-boogie-woogie-woog-ie/
tags:
- Hadoop
- howto
- R
---

[caption id="attachment_594" align="alignleft" width="261" caption="This blog's name in Chinese! "][![](https://www.cerebralmastication.com/wp-content/uploads/2010/02/bad_egg.png)
](https://www.cerebralmastication.com/wp-content/uploads/2010/02/bad_egg.png)[/caption]

I just came back from the future and let me be the first to tell you this: Learn some Chinese. And more than just cào nǐ niáng  (肏你娘) which your friend in grad school told you means "Live happy with many blessings". Trust me, I've been hanging with Madam Wu and she told me it doesn't mean that.

So how did I travel to the future to visit with Madam Wu, you ask? Well the short answer is Hadoop. Yeah, the cute little elephant. [As I have told you before](https://www.cerebralmastication.com/2010/02/using-the-r-multicore-package-in-linux-with-wild-and-passionate-abandon/), multicore makes your R code run fast by using worm holes to shoot your results back from the future. Well Hadoop actually takes you to the future on the back of an elephant and you can bring your own results back! I couldn't make this up if I tried, so you know it's true! And what's fantastic about all of this is Hadoop works with R! And Amazon will let you rent a time traveling elephant through their [Elastic MapReduce service](http://aws.amazon.com/elasticmapreduce/)! I think Amazon coined the term "Time Travel as a Service" or TTaaS  generally pronounced as "ta-tas" in [the industry](http://www.savethetatas.com/). If you are a CTO be sure and use this in your next "vision statement" pitch so everyone will know you're hip to all this cloud stuff.

So you use R and you want to travel into the future on the back of an elephant to visit Madam Wu and get your model results back, don't you? Well it's a damn good thing you read this blog because I'm going to give you the keys to the Wu dynasty and a little 福寿 while we're at it.

I've never had an original thought in my life so I started with [this discussion ](http://developer.amazonwebservices.com/connect/thread.jspa?messageID=128995&#128995)over at the AMZN E M/R discussion forum. Peter Skomoroch from [Data Wrangling ](http://www.datawrangling.com/)gives a very good example with all the data and code provided so you can run it yourself.  Pete's example really shakes the  yáng guǐzi, as we say in the future. In addition I read the documentation for David Rosenberg's [HadoopStreaming package](http://docs.google.com/viewer?url=http%3A%2F%2Fcran.r-project.org%2Fweb%2Fpackages%2FHadoopStreaming%2FHadoopStreaming.pdf) which was good for insight, but I didn't use the package as it's really focused on the 'big data' problem.

[caption id="attachment_639" align="alignleft" width="208" caption="That elephant is so freaking cute! "][![](https://www.cerebralmastication.com/wp-content/uploads/2010/02/hadoop-elephant.jpeg)
](https://www.cerebralmastication.com/wp-content/uploads/2010/02/hadoop-elephant.jpeg)[/caption]

Prior to my foray into time travel, I knew that Hadoop could be used to process big text files and do something like rip out all the links and count them. But I thought that Hadoop was all about processing big data. I never paid attention to the big Hadoop elephant in the room because I don't have big data. I have big CPU hogging models (mostly slow because I don't code worth a shit). What got me reconsidering my world view was [John Myles White](http://www.johnmyleswhite.com/)'s comment on my [multicore post ](https://www.cerebralmastication.com/2010/02/using-the-r-multicore-package-in-linux-with-wild-and-passionate-abandon/)earlier. John encouraged me to look into running my simulations on AMZN's E M/R service using Hadoop streaming. So instead of giving Hadoop  a big fat text file to parse, I just gave it a text file with 10,000 rows each containing an integer from 1:10,000. Then I refactored my R code to read a line from stdin, trim it down to just the integer, and then go run the simulation with that number. When done I had it serialize the resulting model output and return that to stdout. Hadoop takes care of chopping up the input and pulling together the output.

I learned a few "gotchas" or, as we say in the future: 臭婊子(I think that should be plural). I'll do a whole blog post on gotchas soon, but here's the bullet points:



	  * AMZN is currently running the version of Debian Linux named Lenny which has version 2.7.1 of R installed. No matter what the documentation says, don't let Lenny tend to the rabbits.
	  * Test all code by firing up an interactive Pig instance and logging in as 'hadoop'. Instead of running Pig, run R and test your code. And as it says in the FAQ: "The Pig don't care either way. " Which, despite sounding like buggery, is the truth.
	  * If your code runs inside of R on a Hadoop instance, drop back to the command line on the Hadoop instance and run 'cat infile.txt | yourMapper.R | sort | yourReducer.R > outfile.txt'. This pipes your input file into your mapper file which does it's thing and then pipes the results to your reducer file which then "pumps up the jam" into an output file.  What you see in the outfile.txt is what Hadoop will produce. So it you don't like what you see, you better do some more coding.
	  * You CAN load packages into R in a Hadoop instance running in AMZN E M/R. There are a few caveats, of course:


	  1. Your package has to work in R 2.7.1. (until AMZN upgrades to the next stable version of Debian.
	  2. As far as I can tell, all the output has to come out of stdout. So if you want to end up with R objects which you use for other things, you should get comfortable with the serialize() command and reading text files back into R. Which, as you can see [from this question](http://stackoverflow.com/questions/2258511/r-serialize-objects-to-text-file-and-back-again), I am not yet comfortable with.
	  3. There will be multiple instances of R running on every machine. So if they are all trying to download a package to the same directory, you are going to get file lock errors. One solution is to have each R instance create a directory for packages that includes the PID of the R instances. That way there's no possibility for a conflict! Here's an example of how I load the Hmisc package:


	  * You'll probably want to provide some data to R. This is done by uploading your files to S3 and then passing the "-cacheFile" option to Hadoop. To get the plyr package to load in R 2.7.1 I had to edit the package. I then uploaded the altered package thusly:



<blockquote>-cacheFile s3n://rdata/plyr_0.1.9.tar.gz#plyr_0.1.9.tar.gz</blockquote>


More to come later. I've gotta get back to the future.

[caption id="attachment_631" align="alignleft" width="304" caption="You hold the elephant and I'll plug this in. "][![](https://www.cerebralmastication.com/wp-content/uploads/2010/02/christopher_lloyd.jpg)
](https://www.cerebralmastication.com/wp-content/uploads/2010/02/christopher_lloyd.jpg)[/caption]
