---
author: JD Long
date: 2010-08-30 20:12:34+00:00
draft: false
title: Stochastic Simulation With Copulas in R
type: post
url: /2010/08/stochastic-simulation-with-copulas-in-r/
tags:
- howto
- R
- risk
---

[![You know we do! ](https://www.cerebralmastication.com/wp-content/uploads/2010/08/econModels.jpg)
](http://www.cafepress.com/+ringer_t,350602392)A friend of mine gave me a call last week and was wondering if I had a little R code that could illustrate how to do a [Cholesky decomposition](http://en.wikipedia.org/wiki/Cholesky_decomposition). He ultimately wanted to build a Monte Carlo model with correlated variables. I pointed him to a number of packages that do Cholesky decomp but then I recommended he consider just using a Gaussian [Copula ](http://en.wikipedia.org/wiki/Copula_%28statistics%29) and R for the whole simulation. For most of my copula needs in R, I use the [QRMlib package](http://cran.r-project.org/web/packages/QRMlib/index.html) which is a code companion to the book [Quantitative Risk Management: Concepts, Techniques and Tools](http://www.amazon.com/gp/product/0691122555?ie=UTF8&tag=riskthou-20&linkCode=as2&camp=1789&creative=390957&creativeASIN=0691122555) by Alexander J. McNeil, Rudiger Frey and Paul Embrechts. The book is only loosely coupled (pun intended) with the code in the QRMlib package. I really wish the book had been written with code examples and tight linkage between the book and the code. Of course I'm the type of guy who prefers code snip-its to mathematical notation.

I had some code where I used the QRMlib package, but it was really messy and fairly specific to my use case. So I whipped up very simple example of how to create correlated random draws from a multivariate distribution. In this example I used normally distributed marginals and Gaussian correlation to keep things simple and easy to follow. Rather than blogging through the code, I added a shit load (metric ass ton, if you're in Canada) of comments. The code is designed to be stepped through. So don't just run the whole blob and wonder what happened.

Walk through the code and if you find any errors be sure and let me know.

The code is embedded in a Github gist below, but if you are reading this in an aggregator (shout out to [R-Bloggers](http://www.r-bloggers.com/)) you'll need to [manually go to the gist](http://gist.github.com/557900).

[gist id="557900"]
