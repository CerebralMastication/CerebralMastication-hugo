---
author: JD Long
date: 2012-03-17 16:03:42+00:00
draft: false
title: Solving easy problems the hard way
type: post
url: /2012/03/solving-easy-problems-the-hard-way/
tags:
- R
---

There's a charming little brain teaser that's going around the Interwebs. It's got various forms, but they all look something like this:


<blockquote>This problem can be solved by pre-school children in 5-10 minutes, by programer – in 1 hour, by people with higher education … well, check it yourself! ![:)](http://girlsaregeeks.com/WPApp/wp-includes/images/smilies/icon_smile.gif)


8809=6
7111=0
2172=0
6666=4
1111=0
3213=0
7662=2
9313=1
0000=4
2222=0
3333=0
5555=0
8193=3
8096=5
7777=0
9999=4
7756=1
6855=3
9881=5
5531=0
2581=?</blockquote>


SPOILER ALERT...

The answer has to do with how many circles are in each number. So the number 8 has two circles in its shape so it counts as two. And 0 is one big circle, so it counts as 1. So 2581=2. Ok, that's cute, it's an alternative mapping of values with implied addition.

What bugged me was how might I solve this if the mapping of values was not based on shape. So how could I program a computer to solve this puzzle? I gave it a little thought and since I like to pretend I'm an econometrician, this looked a LOT like a series of equations that could be solved with an OLS regression. So how can I refactor the problem and data into a trivial OLS? I really need to convert each row of the training data into a frequency of occurrence chart. So instead of 8809=6 I need to refactor that into something like:

1,0,0,0,0,0,0,0,2,1 = 6

In this format the independent variables are the digits 0-9 and their value is the number of times they occur in each row of the training data. I couldn't figure out how to do the freq table so, as is my custom, I created a concise simplification of the problem and [put it on StackOverflow.com ](http://stackoverflow.com/q/9728038/37751)which  yielded a great solution. Once I had the frequency table built, it was simple a matter of a linear regression with 10 independent variables and a dependent with no intercept term.

My whole script, which you should be able to cut and paste into R, if you are so inclined, is the following:







    
    <span style="color: #666666; font-style: italic;">## read in the training data</span>
    <span style="color: #666666; font-style: italic;">## more lines than it should be because of the https requirement in Github</span>
    temporaryFile <span><-</span> <a href="http://inside-r.org/r-doc/base/tempfile"><span style="color: #003399; font-weight: bold;">tempfile</span></a><span style="color: #009900;">(</span><span style="color: #009900;">)</span>
    <a href="http://inside-r.org/r-doc/utils/download.file"><span style="color: #003399; font-weight: bold;">download.file</span></a><span style="color: #009900;">(</span><span style="color: #0000ff;">"https://raw.github.com/gist/2061284/44a4dc9b304249e7ab3add86bc245b6be64d2cdd/problem.csv"</span><span style="color: #339933;">,</span>destfile=temporaryFile<span style="color: #339933;">,</span> method=<span style="color: #0000ff;">"curl"</span><span style="color: #009900;">)</span>
    series <span><-</span> <a href="http://inside-r.org/r-doc/utils/read.csv"><span style="color: #003399; font-weight: bold;">read.csv</span></a><span style="color: #009900;">(</span>temporaryFile<span style="color: #009900;">)</span>
    
    <span style="color: #666666; font-style: italic;">## munge the data to create a frequency table</span>
    freqTable <span><-</span> <a href="http://inside-r.org/r-doc/base/as.data.frame"><span style="color: #003399; font-weight: bold;">as.data.frame</span></a><span style="color: #009900;">(</span> <a href="http://inside-r.org/r-doc/base/t"><span style="color: #003399; font-weight: bold;">t</span></a><span style="color: #009900;">(</span><a href="http://inside-r.org/r-doc/base/apply"><span style="color: #003399; font-weight: bold;">apply</span></a><span style="color: #009900;">(</span>series<span style="color: #009900;">[</span><span style="color: #339933;">,</span><span style="color: #cc66cc;">1</span><span>:</span><span style="color: #cc66cc;">4</span><span style="color: #009900;">]</span><span style="color: #339933;">,</span> <span style="color: #cc66cc;">1</span><span style="color: #339933;">,</span> <a href="http://inside-r.org/r-doc/base/function"><span style="color: #003399; font-weight: bold;">function</span></a><span style="color: #009900;">(</span>X<span style="color: #009900;">)</span> <a href="http://inside-r.org/r-doc/base/table"><span style="color: #003399; font-weight: bold;">table</span></a><span style="color: #009900;">(</span><a href="http://inside-r.org/r-doc/base/c"><span style="color: #003399; font-weight: bold;">c</span></a><span style="color: #009900;">(</span>X<span style="color: #339933;">,</span> <span style="color: #cc66cc;">0</span><span>:</span><span style="color: #cc66cc;">9</span><span style="color: #009900;">)</span><span style="color: #009900;">)</span><span>-</span><span style="color: #cc66cc;">1</span><span style="color: #009900;">)</span><span style="color: #009900;">)</span> <span style="color: #009900;">)</span>
    <a href="http://inside-r.org/r-doc/base/names"><span style="color: #003399; font-weight: bold;">names</span></a><span style="color: #009900;">(</span>freqTable<span style="color: #009900;">)</span> <span><-</span> <a href="http://inside-r.org/r-doc/base/c"><span style="color: #003399; font-weight: bold;">c</span></a><span style="color: #009900;">(</span><span style="color: #0000ff;">"zero"</span><span style="color: #339933;">,</span><span style="color: #0000ff;">"one"</span><span style="color: #339933;">,</span><span style="color: #0000ff;">"two"</span><span style="color: #339933;">,</span><span style="color: #0000ff;">"three"</span><span style="color: #339933;">,</span><span style="color: #0000ff;">"four"</span><span style="color: #339933;">,</span><span style="color: #0000ff;">"five"</span><span style="color: #339933;">,</span><span style="color: #0000ff;">"six"</span><span style="color: #339933;">,</span><span style="color: #0000ff;">"seven"</span><span style="color: #339933;">,</span><span style="color: #0000ff;">"eight"</span><span style="color: #339933;">,</span><span style="color: #0000ff;">"nine"</span><span style="color: #009900;">)</span>
    freqTable<span>$</span>dep <span><-</span> series<span style="color: #009900;">[</span><span style="color: #339933;">,</span><span style="color: #cc66cc;">5</span><span style="color: #009900;">]</span>
    
    <span style="color: #666666; font-style: italic;">## now a simple OLS regression with no intercept</span>
    myModel <span><-</span> <a href="http://inside-r.org/r-doc/stats/lm"><span style="color: #003399; font-weight: bold;">lm</span></a><span style="color: #009900;">(</span>dep <span>~</span> <span style="color: #cc66cc;">0</span> <span>+</span> zero <span>+</span> one <span>+</span> two <span>+</span> three <span>+</span> four <span>+</span> five <span>+</span> six <span>+</span> seven <span>+</span> eight <span>+</span> nine<span style="color: #339933;">,</span> <a href="http://inside-r.org/r-doc/utils/data"><span style="color: #003399; font-weight: bold;">data</span></a>=freqTable<span style="color: #009900;">)</span>
    <a href="http://inside-r.org/r-doc/base/round"><span style="color: #003399; font-weight: bold;">round</span></a><span style="color: #009900;">(</span>myModel<span>$</span>coefficients<span style="color: #009900;">)</span>








[Created by Pretty R at inside-R.org](http://www.inside-r.org/pretty-r)

The final result looks like this:

    
    
    <div id="_mcePaste">> round(myModel$coefficients)</div>
    <div id="_mcePaste">zero   one   two three  four  five   six seven eight  nine</div>
    <div id="_mcePaste">   1     0     0     0    NA     0     1     0     2     1</div>




So we can see that zero, six, and nine all get mapped to 1 and eight gets mapped to 2. Everything else is zero. And four is NA because there were no fours in the training data.




There. I'm as smart as a preschooler. And I have code to prove it.
