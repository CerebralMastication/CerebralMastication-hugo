---
author: JD Long
date: 2009-02-26 14:41:04+00:00
draft: false
title: Data Analysis Workflow... Part 1 of Infinity
type: post
url: /2009/02/data-analysis-workflow-part-1-of-infinity/
tags:
- R
- workflow
---

One of the many things that I sit around pondering when I should be doing productive things is the idea of analytical workflow. I have only worked with one analytical guru who I felt really gave thought and structure to workflow and its impact on analyist productivity. When I talk about workflow I mean the whole process from the time the analytical guy thinks, "Hey, I need to understand the velocity of new purchases between different types of sales campaigns." until he writes down his findings in a presentation or even just a notebook. In the middle I assume this guy extracts some data from a warehouse or live system, does some work on said data, tests some theories, does more stuff, goes and gets coffee, comes back and plays some flash games, goes home and does it again the next day.

Today I was reading over at Data Evolution about [a presentation on how Google and Facebook use R](http://dataspora.com/blog/predictive-analytics-using-r/). The following was a summary of what Bo Cowgill of Google said about his workflow:


<blockquote>The typical workflow that Bo thus described for using R was: (i) pulling data with some external tool, (ii) loading it into R, (iii) performing analysis and modeling within R, (iv) implementing a resulting model in Python or C++ for a production environment.</blockquote>


I found this interesting as I have been masticating on the idea of learning Python for some time. I have run into situations where R was slow, but generally I have solved those through rethinking my algorithm. I'm not really a good programmer in R (or any other language for that matter), but I do want/need/like the statistical functions and ease of plotting in R. If I do learn Python I'll certainly use it to call R... but maybe I should just stick to R.

This has nothing to do with workflow, but the most thought provoking insights in the article above came from Itamar Rosenn at Facebook:


<blockquote>Itamar’s team used recursive partitioning (via the [rpart](http://cran.r-project.org/web/packages/rpart) package) to infer that just two data points are significantly predictive of whether a user remains on Facebook: (i) having more than one session as a new user, and (ii) entering basic profile information.

... [they also] found that activity at three months was predicted by variables related to three classes of behavior: (i) how often a user was reached out to by others, (ii) frequency of third party application use, and (iii) what Itamar termed “receptiveness” — related to how forthcoming a user was on the site.</blockquote>


So Facebook really wants new users to put more info into FB, use it more, and play with third party apps. I guess that logic is why LinkedIn is always telling me I am only 90% complete on my profile and I would be 95% if I would just, yada yada yada... The more info I put into their walled garden, the more I will play there. And the more ads I will see. Makes sense to me. I guess I follow the same model when I try to get my clients to use my services more and more... I want to be sticky too. But not in a bad way.
