---
author: JD Long
date: 2010-08-31 15:17:27+00:00
draft: false
title: Even Simpler Multivariate Correlated Simulations
type: post
url: /2010/08/even-simpler-multivariate-correlated-simulations/
tags:
- howto
- R
- risk
---

[![](https://www.cerebralmastication.com/wp-content/uploads/2010/08/Screenshot-Untitled-Window-3.png)
](https://www.cerebralmastication.com/wp-content/uploads/2010/08/Screenshot-Untitled-Window-3.png)So after yesterday's post on [Simple Simulation using Copulas](https://www.cerebralmastication.com/2010/08/stochastic-simulation-with-copulas-in-r/) I got a very nice email that basically begged the question, "Dude, why are you making this so hard?" The author pointed out that if what I really want is a Gaussian correlation structure for Gaussian distributions then I could simply use the mvrnorm() function from the MASS package. Well I did a quick


<blockquote>?mvrnorm</blockquote>


and, I'll be damned, he's right! The advantage of using a copula is the ability to simulate correlation structures where the correlation is different for different levels of values. So that gives the flexibility to make the tails of the distributions more correlated, for example. But my example yesterday was purposefully simple... so simple that a copula was not even needed.

After creating my sample data all I really needed to do was this:


<blockquote>myDraws <- mvrnorm(1e5, mu=mean(ourData), Sigma=cov(ourData))</blockquote>


So I  took my example from yesterday and updated it using the mvrnorm() code and, as is my custom, put up a [Github gist.](http://gist.github.com/559082) The code is embedded below as well. I added a little ggplot2 code at the end that will create a facet plot of the 4 distributions showing the shape of the distributions of both the starting data and the simulated data. The plot in the upper left of this post is the ggplot output.

_**EDIT: **_The email hipping me to this was sent by [Dirk Eddelbuettel](http://dirk.eddelbuettel.com) who's been very helpful to me more times than I can count. I had omitted his name initially. However after confirming with Dirk, he told me it was OK to mention him by name in this post.

[gist id="559082"]
