---
author: JD Long
date: 2009-03-09 21:37:57+00:00
draft: false
title: Choosing an SQL Engine for Analytics
type: post
url: /2009/03/chosing-an-sql-engine-for-analytics/
tags:
- database
- firebird
- mysql
- R
- sql server
- sqlite
---

I've been struggling for a while on which database to use for my working data. I used to use MS Access quite a lot. The problems with MS Access include but are not limited to:



	  * 2 GB file size limit, at least historically
	  * Versions change with each edition of MS Office
	  * Sort of tough to write SQL scripts
	  * Very little automation, ie compression, backup, etc.
	  * Windows only

I used Oracle for a few years as a result of my previous employer being an Oracle shop. I then switched to SQL Server when I changed jobs. A full blown client/server DB really does not make a lot of sense for much of what I do. I don't run a transactional data store. I don't need to have dozens of users hooked to the DB. And I do sometimes need access to my data when I am not hooked to the mother-ship. So I could run the free version of SQL Server on my laptop or run MySQL on my laptop, but both of these options rub me the wrong way. Why? I do a lot of data analysis in R which is RAM intensive. Running a DB server on my laptop means that some fraction of my RAM is going to be taken up by the db server software which is hanging out waiting for me to throw requests at it. I could manually hack around this by starting the server before I load data and then killing it after the data is loaded. That's just too big of a pain in my rectum. Oh yeah, one more design requirement: I want to be able to push the whole DB out to a storage blob at Amazon and pound on it using EC2 machines, running Linux. Plus I am cheap and don't want to pay a lot.

I'll probably end up with a model where I keep some master data sets on a client/server DB and then I will replicate chunks of that to my laptop into my serverless db. I'll probably also put output from my desktop db back into the server after analytic work is  done.

I knew about SQLite because of an [interview with its author, Richard Hipp on FLOSS Weekly](http://www.twit.tv/floss26). There's also a [video of Hipp talking at the Googleplex](http://video.google.com/videoplay?docid=-5160435487953918649). I wish that guy was my neighbor. He seems like the type of guy who would shovel your walk for you then apologize for not doing it perfectly by sending over homemade cookies. Unrelated to the cookies, I really like that SQLite is [weakly typed](http://en.wikipedia.org/wiki/Type_system#Strong_and_weak_typing).  I'm a free spirit like that.

I did some digging for SQLite alternatives and came up with [some stuff at StackOverflow](http://stackoverflow.com/questions/417917/alternatives-to-sqlite). You can read the post but it reminded me of Firebird. I'm immediately drawn to FireBird since their logo looks so dang much like the Ruger logo:

![downloads](https://www.cerebralmastication.com/wp-content/uploads/2009/03/downloads.jpg)


![fb-facts](https://www.cerebralmastication.com/wp-content/uploads/2009/03/fb-facts.png)


But is Firebird able to be run severless?  If I have to install a server then I would just as well run MySQL.

[Berkeley DB ](http://www.oracle.com/database/berkeley-db/index.html)seems like another option worth investigating, although I am not sure if I can use it without really embedding it in another program the way that I can with SQLite.

SQLite gets bonus points for having native R drivers meaning that I don't have to go through a connector technology like ODBC. This is important enough that I should probably make that a requirement. I think Berekley DB has support in R as well. I know for a fact that writing back to SQL Server through the R ODBC package (RODBC) is like pushing a car with a rope, but only slower. Plus I don't know how to make ODBC work on Linux. Not rocket science, I am sure, but still one more thing I would have to learn before I do that which I am paid to do.

I'm going to do some testing, but it looks like I should test real life performance of SQLlite and Firebird with my data.  More to come on this, I am sure.
