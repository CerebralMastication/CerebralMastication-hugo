---
author: JD Long
date: 2009-12-11 19:30:55+00:00
draft: false
title: Struggling with apply() in R
type: post
url: /2009/12/struggling-with-apply-in-r/
tags:
- apply
- plyr
- R
- video
---

It's common knowledge that I struggle wrapping my head around the apply functions in R. That is illustrated very clearly in the [following discussion ](http://stackoverflow.com/questions/1355355/how-to-avoid-a-loop-in-r-selecting-items-from-a-list)on Stack Overflow:

[![apply_struggle](https://www.cerebralmastication.com/wp-content/uploads/2009/12/apply_struggle.PNG)
](http://stackoverflow.com/questions/1355355/how-to-avoid-a-loop-in-r-selecting-items-from-a-list)

Dirk's comment is actually spot on. I've asked the same damn question at least 4-5 times. Only I didn't really understand it was the same question. That's one of the problems of not really being good at something; it's hard to think abstractly about it. I'm not really good at R, so sometimes I don't realize that multiple concepts are related. As I talk with other new users of R it's clear that unless they come from a programming language with an apply-esque construct they likely are struggling with R. I think most of the confusion comes from a) not understanding what data format apply() is going to return and b) not understanding anonymous functions.

With this in mind I did a little screencast illustrating how this struggle plays out for a new users. I also show why I use the plyr package for much of the stuff other folks use apply() for.

Any feedback you have is appreciated. This is my first stab at a screencast, so I am still trying to figure out the best approach/method as well as how many drinks puts me on the [Ballmer Peak](http://xkcd.com/323/).



**EDIT**: it's been pointed out that I misuse some terminology a number of times. I should have named my year vector "yearVector." By calling it "yearList" I then refer to the vector as a list. I was using "list" in the vernacular, but since list is a specific R data structure it is confusing that I named a vector a name with "list" in it.
