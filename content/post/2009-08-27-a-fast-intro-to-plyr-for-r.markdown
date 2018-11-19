---
author: JD Long
date: 2009-08-27 20:00:52+00:00
draft: false
title: A Fast Intro to PLYR for R
type: post
url: /2009/08/a-fast-intro-to-plyr-for-r/
tags:
- plyr
- R
---

![pliers](https://www.cerebralmastication.com/wp-content/uploads/2009/08/pliers.jpg)
I'm not dead yet! Although it has been rumored that I am. The new job is going great and I'm thrilled to be with a new firm doing interesting work alongside smart people. It makes me seem smarter by simple association.

There's been a lot going on recently in the R user community. There was an[ R flash mob of Stack Overflow](http://en.oreilly.com/oscon2009/public/schedule/detail/10432) which resulted in a noticeable increase in the number of [R questions and answers](http://stackoverflow.com/questions/tagged/r) in SO. I've been blown away by the quality of the [participants](http://stackoverflow.com/questions/tagged?tagnames=r&sort=stats&pagesize=50). There has also been increased quality discussions on Twitter which are being [tagged with #rstats](http://twitter.com/#search?q=%23rstats). These changes in the community have [not gone unnoticed](http://www.iq.harvard.edu/blog/sss/archives/2009/08/the_changing_na.shtml).

Recently I posted a question about how to do a 'group by' in a regression with R. I had a way I had been doing this but I was suspicious there was a better way. [One of the answers](http://stackoverflow.com/questions/1169539/linear-regression-and-group-by-in-r/1214432#1214432) proposed using the PLYR package. I think I had seen the plyr package a few times but never really understood it. Although I didn't select this as my top answer, it prompted me to look into PLYR more. What I discovered was really interesting.

The [PLYR package ](http://had.co.nz/plyr/)is a tool for doing split-apply-combine (SAC) procedures. I'm very fluent in SQL so the best analogy for me was the GROUP BY statement in SQL. PLYR adds very little new functionality to R. What it does do is take the process of SAC and make it cleaner, more tidy and easier. I think I'm not the only one who wants a clean and tidy SAC. Here's a quick example of making some summary stats using PLYR:

    
    # install.packages("plyr") #run this if you don't have the package already
     library(plyr)
    
    #make some example data
    dd<-data.frame(matrix(rnorm(216),72,3),c(rep("A",24),rep("B",24),rep("C",24)),c(rep("J",36),rep("K",36)))
    colnames(dd) <- c("v1", "v2", "v3", "dim1", "dim2")
    
    #ddply is the plyr function
    ddply(dd, c("dim1","dim2"), function(df)mean(df$v1))


result:


<blockquote>

>     
>         dim1 dim2          V1
>         1    A    J  0.02554362
>         2    B    J -0.15839675
>         3    B    K -0.06077399
>         4    C    K -0.02326776
> 
> 
</blockquote>


PLYR functions have a neat naming convention. The first two letters of the function tells the input and output data types, respectively. The one I use the most is ddply which takes a data frame in and spits out a data frame.  Let me see if I can explain what ddply is doing. The first argument, dd, is the input data frame. The next argument is the "group by" variables. Since I want to group by two variables I send them as a vector (that's what the c() bit does). What threw me for a loop initially was the third argument, the function. What I found myself trying (unsuccessfully) was just using mean(v1) as the third argument. If I did that, R would spit at me and bring the marital status of my parents into question. I discovered that the problem was the ddply function was splitting the data by my 'group by' variables and then it wanted to pass each of the resulting data frames to a function. So what does it mean to pass a data frame to mean(v1)? Yeah, it means Jack Crap, that's what it means. So in one of the PLYR examples I saw they were using these inline functions. The idea behind function(df)mean(df$v1) is to create a function to which we can pass a data frame and get out a meaningful result. The subset (or split) of the data gets passed to the function and that subset is then known as df. mean(df$v1) calculates the mean of v1 and returns an answer. ddply holds on to the answers of each split and then reassembles them all in the end. Slick, ey?

As with most things in R the idea can be extended to a vector of functions in order to perform many operations on each split:

    
    ddply(dd, c("dim1","dim2"), function(df)c(mean(df$v1),mean(df$v2),mean(df$v3),sd(df$v1),sd(df$v2),sd(df$v3)))


The result looks like this:


<blockquote>

>     
>     dim1 dim2          V1        V2         V3        V4        V5       V6
>     1    A    J  0.02554362 0.3400250  0.1206980 0.9326424 1.0044120 1.100762
>     2    B    J -0.15839675 0.3662559 -0.1784193 0.7447807 0.8752162 1.105258
>     3    B    K -0.06077399 0.5184403 -0.2076024 1.0385107 1.0609706 1.153153
>     4    C    K -0.02326776 0.2639328  0.1352895 0.7940938 0.9025207 1.072460
> 
> 
</blockquote>


Pretty nifty.

The author of PLYR is Hadley Wickham who is also the man behind [GGPLOT2](http://had.co.nz/ggplot2/). If you like PLYR or GGPLOT2 then you should immediately [buy Hadley's GGPLOT2 book on Amazon](http://www.amazon.com/gp/product/0387981403?ie=UTF8&tag=hadlwick-20&linkCode=as2&camp=1789&creative=390957&creativeASIN=0387981403). But be sure and use the link on this site or the link on [Hadley's site ](http://had.co.nz/ggplot2/book/)so he can get Amazon associate payment. The authors I have talked to told me they get more from the Associate program than they get from publishing royalties.

My father is a retired pilot turned crop farmer. He ALWAYS carries a pair of pliers in a nylon pouch on his belt. I can see that Hadley's PLRY package is going to become my proverbial 'belt pliers.'

Of course if I wrote an R package I'd have to name it [Super RamBar](http://www.paratech.us/html/FET/Crw/CrwSRB/ParatechNFSRB.htm), cause that's just how I roll.
