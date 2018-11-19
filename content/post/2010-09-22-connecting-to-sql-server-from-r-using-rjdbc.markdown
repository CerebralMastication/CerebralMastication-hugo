---
author: JD Long
date: 2010-09-22 18:00:26+00:00
draft: false
title: Connecting to SQL Server from R using RJDBC
type: post
url: /2010/09/connecting-to-sql-server-from-r-using-rjdbc/
tags:
- howto
- R
- sql server
---

[![](https://www.cerebralmastication.com/wp-content/uploads/2010/09/sql_server_2008_logo-300x187.png)
](https://www.cerebralmastication.com/wp-content/uploads/2010/09/sql_server_2008_logo.png)A few months ago I switched my laptop from Windows to Ubuntu Linux. I had been connecting to my corporate SQL Server database using RODBC on Windows so I attempted to get ODBC connectivity up and running on Ubuntu. ODBC on Ubuntu turned into an exercise in futility. I spent many hours over many days and never was able to connect from R on Ubuntu to my corp SQL Server.

[Joshua Ulrich](http://www.fosstrading.com/) was kind enough to help me out by pointing me to [RJDBC](http://www.rforge.net/RJDBC/) which scared me a little (I'm easily spooked) because it involves Java. The only thing I know about Java is every time I touch it I [spend days trying to get environment variables](http://stackoverflow.com/questions/3311940/r-rjava-package-install-failing) loaded just exactly the way it wants them. But Josh assured me that it was really not that hard. Here's the short version:

[Download the RJDBC driver from Microsoft](http://www.microsoft.com/downloads/en/details.aspx?FamilyID=a737000d-68d0-4531-b65d-da0f2a735707&displaylang=en). There's Win and *nix versions, so grab which ever you need. Unpack the driver in a known location (I used /etc/sqljdbc_2.0/). Then access the driver from R like so:

    
    require(RJDBC)
    drv <- JDBC("com.microsoft.sqlserver.jdbc.SQLServerDriver",
      "/etc/sqljdbc_2.0/sqljdbc4.jar") 
      conn <- dbConnect(drv, "jdbc:sqlserver://serverName", "userID", "password")
    #then build a query and run it
    sqlText <- paste("
       SELECT * FROM myTable
      ", sep="")
    queryResults <- dbGetQuery(conn, sqlText)


I have a few scripts that I want to run on both my Ubuntu laptop and my Windows Server. To accommodate that I made my scripts compatible with both by doing the following to my drv line:

    
    if (.Platform$OS.type == "unix"){
             drv <- JDBC("com.microsoft.sqlserver.jdbc.SQLServerDriver",
             "/etc/sqljdbc_2.0/sqljdbc4.jar")
    } else {
             drv <- JDBC("com.microsoft.sqlserver.jdbc.SQLServerDriver",
            "C:/Program Files/Microsoft SQL Server JDBC Driver 3.0/sqljdbc_3.0
             /enu/sqljdbc4.jar")
     }


Obviously if you unpacked your drivers in different locations you'll need to molest the code to fit your life situation.

**EDIT: **A MUCH better place to put the JDBC drivers in Ubuntu would be the /opt/ path as opposed to /etc/ which I used above. In Ubuntu the /opt/ directory is where one should put user executables and /etc/ should be reserved for packages installed by apt. I'm not familiar with all the conventions in Ubuntu (or even Linux in general) so I didn't realize this until I got some reader feedback. 

Be forewarned, RJDBC is pretty damn slow and it appears to no longer be in active development. For my use case, RODBC was clearly faster. But RJDBC works for me in Ubuntu and that was my biggest need.
