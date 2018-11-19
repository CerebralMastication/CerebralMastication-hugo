---
author: JD Long
date: 2009-11-24 23:14:06+00:00
draft: false
title: Loading Big (ish) Data into R
type: post
url: /2009/11/loading-big-data-into-r/
tags:
- R
- SQL
- sqldf
- sqlite
---

So for the rest of this conversation big data == 2 Gigs. Done. Don't give me any of this 'that's not big, THIS is big' shit. There now, on with the cool stuff:

This week on twitter Vince Buffalo asked about loading a 2 gig comma separated file (csv) into R (OK, he asked about tab delimited data, but I ignored that because I use mostly comma data and I wanted to test CSV. Sue me.)

[![2gib](https://www.cerebralmastication.com/wp-content/uploads/2009/11/2gib.PNG)
](http://twitter.com/vsbuffalo/statuses/5987999475)

I thought this was a dang good question. What I have always done in the past was load my data into SQL Server or Oracle using an ETL tool and then suck it from the database to R using either native database connections or the RODBC package. [Matti Pastell (@mpastell) recommended ](http://twitter.com/mpastell/statuses/6002853376)using the [sqldf ](http://code.google.com/p/sqldf/)(SQL to data frame) package to do the import. I've used sqldf before, but only to allow me to use SQL syntax to manipulate R data frames. I didn't know it could import data, but that makes sense, given how sqldf works. How does it work? Well sqldf sets up an instance of the [sqlite ](http://www.sqlite.org/)database server then shoves R data into the DB, does operations on the tables, and then spits out an R data frame of the results. What I didn't realize is that we can call sqldf from within R and have it import a text file directly into sqlite and then return the data from sqlite directly into R using a pretty fast native connection. I did a little Googling and came up with [this discussion ](http://old.nabble.com/Re%3A-Memory-Experimentation%3A-Rule-of-Thumb-%3D-10-15-Times-the-Memory-to12076668.html#a12078165)on the R mailing list.

So enough background, here's my setup: I have a Ubuntu virtual machine running with 2 cores and 10 gigs of memory. Here's the code I ran to test:


<blockquote>bigdf <- data.frame(dim=sample(letters, replace=T, 4e7), fact1=rnorm(4e7), fact2=rnorm(4e7, 20, 50))
write.csv(bigdf, 'bigdf.csv', quote = F)</blockquote>


That code creates a data frame with 3 columns. I created a single letter text column, then two floating point columns. There are 40,000,000 records. When I run the write.csv step on my machine I get about 1.8GiB. That's close enough to 2 gigs for me. I created the text file and then ran rm(list=ls()) to kill all objects. I then ran gc() and saw that I had hundreds of megs of something or other (I have not invested the brain cycles to understand the output that gc() gives). So I just killed and restarted R. I then ran the following:


<blockquote>library(sqldf)
f <- file("bigdf.csv")
system.time(bigdf <- sqldf("select * from f", dbname = tempfile(), file.format = list(header = T, row.names = F)))</blockquote>


That code loads the CSV into an sqlite DB then executes a select * query and returns the results to the R data frame bigdf. Pretty straightforward, ey? Well except for the dbname = tempfile() bit. In sqldf you can choose where it makes the sqlite db. If you don't specify at all it makes it in memory which is what I first tried. I ran out of mem even on my 10GB box. So I read a little more and added the dbname = tempfile() which creates a temporary sqlite file on the disk. If I wanted to use an existing sqlite file I could have specified that instead.

So how long did it take to run? Just under 5 minutes.

So how long would the read.csv method take? Funny you should ask. I ran the following code to compare:


<blockquote>system.time(big.df <- read.csv('bigdf.csv'))</blockquote>


And I would love to tell you how long that took to run, but it's been running for half an hour all night and I just don't have that kind of patience.

-JD
