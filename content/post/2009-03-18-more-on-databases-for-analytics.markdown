---
author: JD Long
date: 2009-03-18 13:56:56+00:00
draft: false
title: More on Databases for Analytics
type: post
url: /2009/03/more-on-databases-for-analytics/
tags:
- column oriented
- database
- LucidDB
- SAS
- sql server
---

Lately I have been [struggling for what type of database ](https://www.cerebralmastication.com/?p=212)to use for my analytics work. SQL Server is a really good database but I always get the feeling it was[ ](http://)built for stuff other than what I want to do with it. I keep feeling like I am digging post holes with a spade and I keep thinking, "there should be a tool that does this better."

Yesterday I was listening to the IT Conversations Podcast and heard a presentation given by [Michael Stonebraker from the Money:Tech conference](http://itc.conversationsnetwork.org/shows/detail4009.html). Stonebraker (I keep wanting to call him Ballbuster) talks about how the elephant database engines (SQL Server, Oracle, DB2, etc) are never going to make it in financial trading engines because their approach to databasing (like freebasing only with data) is antiquated. It hit me when I was listening to the presentation that I don't really know about the different types of databases. I've always wondered why SAS can be so damn fast at certain database functions and Oracle or SQL Server so slow at the same thing. I understand a little better now. I've been trying to [get SQL Server to act ](http://stackoverflow.com/questions/571750/make-sql-server-faster-at-manipulating-data-turn-off-transaction-logging)like some other kind of DB, when in reality I just need a different type of DB... like a column oriented DB, maybe.

Since I don't need the type of super high performance DB that Ballbuster talks about, I am wondering if I can efficiently use some type of open source [Column Oriented DB](http://en.wikipedia.org/wiki/Column-oriented_DBMS). But I have a lot of investment in learning SQL which I would like to not lose. Looks like I need to research my options.

Below are some sources for database technology information beyond the big three elephants:



	  * [John Sichi's blog](http://thinkwaitfast.blogspot.com/). He is the co-founder of [LucidDB](http://www.luciddb.org/)
	  * [Julian Hyde's Blog ](http://julianhyde.blogspot.com/)- Founder of the [Mondrian ](http://sourceforge.net/projects/mondrian/)open-source OLAP engine, and chief architect of the [SQLstream ](http://www.sqlstream.com/index.html)streaming SQL engine
	  * [The Database Column ](http://www.databasecolumn.com/)- A multi-author blog on database technology and innovation.

More to come on this topic...
