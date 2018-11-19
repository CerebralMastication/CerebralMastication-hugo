---
author: JD Long
date: 2009-06-22 14:14:55+00:00
draft: false
title: Who's Tweets Do I Read... Magic R Code Says...
type: post
url: /2009/06/whos-tweets-do-i-read-magic-r-code-says/
tags:
- R
- twitter
---

![tweetingme](https://www.cerebralmastication.com/wp-content/uploads/2009/06/tweetingme.png)


So one glace at my user logs shows the truth: no one gives a rat's rump that[ I just quit my job](https://www.cerebralmastication.com/?p=284); you just [love you some Twitter R code](https://www.cerebralmastication.com/?p=274). And I'm nothing but an attention whore, so come get some!

So in my[ last 'Twitter with R' post](https://www.cerebralmastication.com/?p=274) I gave you some code I'd written ripped off that allowed you to update your status from R. That's kinda cool, but really just for annoying your friends, tweeting when your code is finished running or, as Eva pointed out in the comments, maybe Tweeting the outcome of a routine. But R is good for analyzing data, plotting graphics, and cool stuff like that. Seems like under kill to just Tweet from it.

So let's make some pretty pictures and stuff. Or more specifically, let's plot a histogram of the last 200 Tweets you received and the people who sent them. An example of said histogram is above.

If you don't already have the libraries XML, lattice, and RCurl, you will need them:
`install.packages('RCurl')
install.packages('XML')
install.packages('lattice')`

Then once you get those bad boys, you can run this code:

`library("RCurl")
library("XML")
library("lattice")
#
#be sure and put your username and passy here
username<-"YourUserName"
passy<-"YourPass"
#
#This sets up the options for curl
#then makes the request from the Twitter API
#the count=200 option pulls the last 200 tweets from your friends
#the twitter api limits you to a max of 200.... yeah, well that's life
#
opts <- curlOptions(header = FALSE, userpwd = paste(username,":",passy,sep=""))
request <- "http://twitter.com/statuses/friends_timeline.xml?count=200"
timeline <- getURL(request,.opts = opts)
#
#Now let's beat up on the XML like it owes us money
doc <- xmlInternalTreeParse(timeline, useInternalNodes = TRUE)
#
#grab only the screen_names and make a list
xml_names_of_posters <- getNodeSet(doc, "/statuses/status/user/screen_name")
text_names_of_posters <- lapply(xml_names_of_posters,xmlValue)
#
#let's take it out of a list... just for kicks
Twitterbaters <- unlist(text_names_of_posters)
#
#and shove it into a data frame... seems like going around my``
#ass to get to my elbow, but I want to put it in a table eventually
#table is kinda like a cross tab. It calcs the frequency for me
posters_list_df<-as.data.frame(Twitterbaters)
Tweets = table(posters_list_df)
#
#lets graph this monkey business with lattice
#
NiftyChart<-barchart(~sort(Tweets), main=list("Who's Tweets Am I Getting?" ,cex=1),xlab=list("Number of Tweets",cex=1))
NiftyChart
update(NiftyChart, col="brown")
#`

**EDIT:** Look in the comments for a great base graphics solution from Paolo. He makes the same graph without Lattice.

**Now be sure and change the username and password **then run that mofo. So now you have a pretty picture like the one I made above. Pretty slick, no?

Special credit goes to @gappy3000 who tipped me off to making this with Lattice instead of ggplot because of the difficulty sorting with ggplot. @HarlanH for helping me know that my struggles with ggplot were not of my own making but were systemic.

The Twitter syntax I hacked together is from the [Twitter API documentation](http://apiwiki.twitter.com/Twitter-API-Documentation). Have fun! And come back later for more attenion whoring blogging from your's truly, @CMastication.

BTW, the reason I didn't structure this as a function is that you should be stepping through this one line at a time to figure out how it works. That's just harder with a function. So I did this for your own good. One day you'll thank me.
