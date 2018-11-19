---
author: JD Long
date: 2009-06-04 15:22:21+00:00
draft: false
title: Twitter from R... Sure, why not!
type: post
url: /2009/06/twitter-from-r-sure-why-not/
tags:
- R
- twitter
---

![](http://www.bit-101.com/blog/wp-content/uploads/2008/07/whale.png)

So I have started following the #RStats tag in twitter. Prior to a week ago I had never Twitterbated so I thought I would give it a go since I am not one to shy away from new technology... much. I think of Twitter like a call in radio show where I get to cut off callers when they annoy me.

Well one of the interesting things I ran across was [this tweet ](http://twitter.com/JoFrhwld/statuses/2012755202)that pointed to [this page about posting tweets from R](http://pastie.org/367741).  The code example was missing a couple of things, so here's the cleaned up version:


<blockquote>

>     
>     
>     
>     
>     <span class="status-body"><span class="entry-content">install.packages("RCurl")
>     library("RCurl")</span></span>
>     <span class="meta meta_paragraph meta_paragraph_text">opts = curlOptions(header = FALSE, userpwd = "username:password", netrc = FALSE)
>     </span>
>     <span class="meta meta_paragraph meta_paragraph_text">update <- function(status){
>       method <- "<span class="markup markup_underline markup_underline_link">http://twitter.com/statuses/update.xml?status=</span>"
>       encoded_status <- URLencode(status)
>       request <- paste(method,encoded_status,sep = "")
>       postForm(request,.opts = opts)
>     }
>     update("My first tweet from R! @CMastication is my daddy! #RStats")
>     </span>
> 
> 

> 
> 
</blockquote>


This is a pretty good, and fairly self explanatory example of how to use RCurl to spew out some data. Don't be a scriptard and try to run this without changing 'username' and 'password.' If you do, I'm coming to your house and going to beat you with your own keyboard until you admit that I am, indeed, your daddy.

Am I planning on Tweeting from R? You have to be kidding. That's just silly. But I might need to use RCurl again. Although for long running scripts I may very well put code at the end of my scripts that sends out a Tweet when the code has finished running. Waaaay easier than setting up email and trying to ensure it runs from EC2 as well as my home machine, etc. The more I think about that, the more I am sure I'm a friggin genius. And of course, on every one of my script completion tweets I'm going to use the #RStats tag so everyone in twitterland will know that I am amazing. And a prick. All at the same time. I'm complex like that.

Oh yeah, I'm @CMastication on Twitter.
