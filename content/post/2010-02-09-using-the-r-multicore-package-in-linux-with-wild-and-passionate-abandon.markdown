---
author: JD Long
date: 2010-02-09 19:57:20+00:00
draft: false
title: Using the R multicore package in Linux with wild and passionate abandon
type: post
url: /2010/02/using-the-r-multicore-package-in-linux-with-wild-and-passionate-abandon/
tags:
- ec2
- howto
- R
---

[![](https://www.cerebralmastication.com/wp-content/uploads/2010/02/amd_mc_processing.jpg)
](https://www.cerebralmastication.com/wp-content/uploads/2010/02/amd_mc_processing.jpg)One of my primary uses for R is to build stochastic simulations of insurance portfolios and reinsurance treaties. It's not uncommon for each of my simulations to take 20 seconds or more to complete (if you're doing the math, that's 55 hours for 10K sims or, approximately 453 games of solitaire) . Initially I ran my sims in R running on an [Oracle VirtualBox ](http://www.virtualbox.org/)(Oracle now owns Virtualbox! *gasp* ) running Ubuntu. Lately I've moved to running my sims on EC2 machines. I'm not yet doing RMPI clustering, although that is on my roadmap. Currently I just fire up a couple of 8 core instances and run 5K sims on each one then FTP the results back to my desktop. It's not very sexy, but it gets the job done... I guess the same could be said of myself, except substitute "makes slurping sounds eating udon" in the place of "gets the job done."

When running processor intensive crap (that's a stochastic modeling term) the single threaded nature of R is painful. In Linux or Mac (i.e. NOT Windows) the [multicore package ](http://www.rforge.net/doc/packages/multicore/multicore.html)is a real godsend. I did a quick code review and, from what I can tell, multicore exploits worm holes to travel back in time and reports your results in a fraction of the time you would expect it to take. Seriously. I expect that as the code matures my computer will fill up with simulation results from simulations which I have not even coded yet. It's almost like magic, except without the rabbit and hat.

The crux of the package is a parallel-ized version of lapply() called mclapply(). I believe the mc stands for 'magic carpet' and is an allusion to the worm hole technology. So how does one harness this package for nefarious self interest doing parallel operations in R? The ultra short answer is: write your R code so that the most processor intensive bit is done with an lapply() function. Then replace the lapply() with mclapply().  Of course you have to load the multicore package before you run it. But that's basically it.

How I implement mcapply() is thusly: I build a table with all my random draws for my simulations. So if I have 20 variables and want to run 10,000 simulations then I'll build a data frame with all 200,000 values (generally 10K rows and 21 columns for 20 variables + and index). The index keeps track of the draw number. Then I have code that performs the 'valuation' based on a single observation of the 20 variables. I wrap the valuation step in a function and then call the valuation process 10,000 times with mclapply(). So it might look something like this:


<blockquote>myOutput <- mclapply( drawList, function(x) valuationReturns(drawNumber=x))</blockquote>


The drawList object is simply a list of the possible indexes (i.e. 1:10000). When the code has iterated over each value from drawList the results will be in the myOutput object. Tada!

I recommend the [htop program ](http://htop.sourceforge.net/)for tracking what's going on with processor utilization in Linux (I presume Mac too if you ask Steve Jobs nicely). If everything is cranking well, and you have 8 cores, you might see an image that looks something like this:

[![](https://www.cerebralmastication.com/wp-content/uploads/2010/02/r-on-ec21.png)
](https://www.cerebralmastication.com/wp-content/uploads/2010/02/r-on-ec21.png)

I don't understand time travel, but I've found that I have better luck if I set mc.preschedule=FALSE. Apparently prescheduled magic carpets are finicky. If I leave mc.preschedule to the default of TRUE then I find that often some of my cores go underutilized.

Let me know if you have other multicore tips and tricks.

If you want to give me shit for running my simulations as root, feel free. I'm impervious to your "best practices" mumbo jumbo. La la la la la la!! Not listening!

Special thanks to [John Cavazos over at the University of Delaware](http://www.cis.udel.edu/~cavazos/index.php?page=multicore-programming) from whom I stole the MC for Dummies image. John, your a gentleman and a humble scholar. Damn few of us left.
