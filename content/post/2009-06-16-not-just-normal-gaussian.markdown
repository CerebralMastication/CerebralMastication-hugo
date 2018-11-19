---
author: JD Long
date: 2009-06-16 16:26:46+00:00
draft: false
title: Not Just Normal... Gaussian
type: post
url: /2009/06/not-just-normal-gaussian/
tags:
- graph
- howto
- R
---

[caption id="" align="alignleft" width="188" caption="Pretty Normal"][![Pretty Normal](http://upload.wikimedia.org/wikipedia/commons/thumb/8/8c/Standard_deviation_diagram.svg/325px-Standard_deviation_diagram.svg.png)
](http://en.wikipedia.org/wiki/Normal_distribution#Standard_deviation_and_confidence_intervals)[/caption]

Dave, over at The Revolutions Blog,[ posted about the big 'ol list of graphs](http://blog.revolution-computing.com/2009/06/graphs-created-with-r-on-wikimedia-commons.html) created with R that are over at [Wikimedia Commons](http://commons.wikimedia.org/wiki/Category:Created_with_R). As I was scrolling through the list I recognized the standard normal distribution from the [Wikipedia article on the same topic](http://en.wikipedia.org/wiki/Normal_distribution#Standard_deviation_and_confidence_intervals).

Below is the fairly simple source code with lots of comments. [Here's the source](http://commons.wikimedia.org/wiki/File:Standard_deviation_diagram.svg). Run it at home... for fun and profit.


<blockquote>

>     
>     # # External package to generate four shades of blue
>     # library(RColorBrewer)
>     # cols <- rev(brewer.pal(4, "Blues"))
>     cols <- c("#2171B5", "#6BAED6", "#BDD7E7", "#EFF3FF")
>     
>     # Sequence between -4 and 4 with 0.1 steps
>     x <- seq(-4, 4, 0.1)
>     
>     # Plot an empty chart with tight axis boundaries, and axis lines on bottom and left
>     plot(x, type="n", xaxs="i", yaxs="i", xlim=c(-4, 4), ylim=c(0, 0.4),
>          bty="l", xaxt="n", xlab="x-value", ylab="probability density")
>     
>     # Function to plot each coloured portion of the curve, between "a" and "b" as a
>     # polygon; the function "dnorm" is the normal probability density function
>     polysection <- function(a, b, col, n=11){
>         dx <- seq(a, b, length.out=n)
>         polygon(c(a, dx, b), c(0, dnorm(dx), 0), col=col, border=NA)
>         # draw a white vertical line on "inside" side to separate each section
>         segments(a, 0, a, dnorm(a), col="white")
>     }
>     
>     # Build the four left and right portions of this bell curve
>     for(i in 0:3){
>         polysection(   i, i+1,  col=cols[i+1]) # Right side of 0
>         polysection(-i-1,  -i,  col=cols[i+1]) # Left right of 0
>     }
>     
>     # Black outline of bell curve
>     lines(x, dnorm(x))
>     
>     # Bottom axis values, where sigma represents standard deviation and mu is the mean
>     axis(1, at=-3:3, labels=expression(-3*sigma, -2*sigma, -1*sigma, mu,
>                                         1*sigma,  2*sigma,  3*sigma))
>     
>     # Add percent densities to each division, between x and x+1
>     pd <- sprintf("%.1f%%", 100*(pnorm(1:4) - pnorm(0:3)))
>     text(c((0:3)+0.5,(0:-3)-0.5), c(0.16, 0.05, 0.04, 0.02), pd, col=c("white","white","black","black"))
>     segments(c(-2.5, -3.5, 2.5, 3.5), dnorm(c(2.5, 3.5)), c(-2.5, -3.5, 2.5, 3.5), c(0.03, 0.01))
> 
> 
</blockquote>
