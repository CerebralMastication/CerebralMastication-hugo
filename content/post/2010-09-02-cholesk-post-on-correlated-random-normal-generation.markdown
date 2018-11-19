---
author: JD Long
date: 2010-09-02 18:03:21+00:00
draft: false
title: Third, and Hopefully Final, Post on Correlated Random Normal Generation (Cholesky
  Edition)
type: post
url: /2010/09/cholesk-post-on-correlated-random-normal-generation/
tags:
- howto
- R
- risk
---

[caption id="attachment_825" align="alignleft" width="250" caption="André-Louis Cholesky is my homeboy"][![](https://www.cerebralmastication.com/wp-content/uploads/2010/09/39-cholesky-250x300.jpg)
](http://www.sabix.org/bulletin/b39/vie.html)[/caption]

When I did a [brief post three days ago](https://www.cerebralmastication.com/2010/08/stochastic-simulation-with-copulas-in-r/) I had no plans on writing two more posts on correlated random number generation. But I've gotten a couple of emails, a few comments, and some Twitter feedback. In response to my first post, [Gappy, calls me out](https://www.cerebralmastication.com/2010/08/stochastic-simulation-with-copulas-in-r/comment-page-1/#comment-5068) and says, "the way mensches do multivariate (log)normal variates is via Cholesky. It’s simple, instructive, and fast."  And I think we're all smart enough to read through Mr. Gappy's comment and see that he's saying I'm a complicated, opaque, and slow, גוי‎. My wife called and said his list would be more accurate if he added 'emotionally detached.' I have no idea what she means.

At any rate, in response to Gappy's comment, here is the third verse (same as the first). The crux of the change is the following lines:

    
    
    <blockquote>
    # shift the mean of ourData to zero
    ourData0 <- as.data.frame(sweep(ourData,2,colMeans(ourData),"-"))
    
    #Cholesky Decomposition of the covariance matrix
    C <- chol(nearPD(cov(ourData0))$mat)
    
    #create a matrix of random standard normals
    Z <- matrix(rnorm(n * ncol(ourData)), ncol(ourData))
    
    #multiply the standard normals by the transpose of the Cholesky
    X <- t(C) %*% Z
    
    myDraws <- data.frame(as.matrix(t(X)))
    names(myDraws) <- names(ourData)
    
    # we still need to shift the means of the samples.
    
    # shift the mean of the draws over to match the starting data
    myDraws <- as.data.frame(sweep(myDraws,2,colMeans(ourData),"+"))
    
    </blockquote>
    
    


_**Edit: **When I first publishes this example, I didn't shift the means prior to taking the cov(). I've sense corrected that.  Also thanks to @fdaapproved on Twitter who pointed out that I can replace the loop above with myDraws <- as.data.frame(sweep(t(X),2,colMeans(ourData),"+"))_

This method, which uses Cholesky decomposition, is how I initially learned to create correlated random draws. I think this method is comparable to the mvrnorm() method. mvrnorm() is handy because it wraps everything above in one single line of code. But the above method is reliant only on the Matrix package and that's only for the nearPD() function. If you are familiar with the guts of the mvrnorm() function and the chol() function, I'd love for you to comment on any technical differences. I looked briefly at the code for both and quickly realized my matrix math was rusty enough that it was going to take a while for me to sort through the code.

If you want the whole script you can find it embedded below [and on Github](http://gist.github.com/562567).

[gist id="562567"]
