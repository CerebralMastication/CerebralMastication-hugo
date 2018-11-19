---
author: JD Long
date: 2013-01-31 21:14:33+00:00
draft: false
title: Installing & Debugging ODBC on Mac OS X
type: post
url: /2013/01/installing-debugging-odbc-on-mac-os-x/
tags:
- odbc
- pandas
- python
- SQL
---

[
![](https://www.cerebralmastication.com/wp-content/uploads/2013/01/Screenshot_1_31_13_10_06_AM1.png)
](https://www.cerebralmastication.com/wp-content/uploads/2013/01/Screenshot_1_31_13_10_06_AM1.png)

[](https://www.cerebralmastication.com/wp-content/uploads/2013/01/Screenshot_1_31_13_10_06_AM1.png)I just spent nearly two full days in a bare knuckle brawl with my Macbook Pro trying to get it to talk to a corporate MS SQL Server. I had abandoned MSSQL more than a year ago in favor of PostgreSQL because of how much easier it is to work with PostgreSQL from a non-Microsoft stack. At that point I was R running on Linux and soon R running on OS X. As part of me changing roles at the company I work for, I've joined a team where everyone else uses Python. So I'm now trying to play nice with the Python guys. In addition to Python, I need to talk to corporate servers which happen to be Microsoft SQL Server.

So enough about why I was banging my head against the Mac ODBC wall. Here's some things I didn't understand until I fought with them for a day. Maybe I can save you some pain.

[![](https://www.cerebralmastication.com/wp-content/uploads/2013/01/flow1.png)
](https://www.cerebralmastication.com/wp-content/uploads/2013/01/flow1.png)

The diagram above shows the sort of stack I now have working. But when I started I didn't even understand how the pieces fit together. Keep in mind that for better or worse, I'm using Macports and in some ways I really like letting Macports do the installation of things. Yes, I know Homebrew rocks and yada yada yada, but I'm not currently using Homebrew. I'm using Macports. So the things I describe below are highly biased to Macports, but much of it is highly applicable to any install method.

It's pretty obvious that when debugging something not working we should move through each piece and make sure each little chunk works before going to the next piece. Of course in real life we just flail, randomly try things, and cuss a lot. Until we get all red in the face and take a step back. So let's start at the beginning:

The installing process should look something like the following:

**SQL Server:** Make sure you can log into SQL Server from a windows box on the same network which you are ultimately trying to connect from. And make sure you log in using the same credentials which you want to use elsewhere.

**FreeTDS:** This is the driver which sits between the Mac ODBC layer and MS SQL Server. FreeTDS does the talking to MS SQL. So it's really important, obviously. I ultimatly installed FreeTDS using the following Macports command:

    
    sudo port install freetds +mssql +odbc +universal


the +odbc bit installs unixODBC and the +mssql and +universal bits are totally mysterious to me. The command line tool tsql comes with FreeTDS. In the debugging section I'll comment more on using isql.

For what it's worth, the current version as of this writing is freetds @0.92.405_0

**UnixODBC: **Comes along with FreeTDS. unixODBC includes the command line tool isql which is a lot like tsql. More on that below.

**pyodbc:** (**Sept 2013 update**: Macport of pyodbc is now named py27-pyodbc, the way God intended.) I didn't think there was a Macport for pyodbc.  The naming pattern for python packages in Mac is pythonVersion-packagename. So, for example, Pandas is py27-pandas for the Python 2.7 version of Pandas. So I tried the following:

    
    sudo port install py27-pyodbc


Which fails to find the package. So I pulled the tar ball and spend hours yesterday trying to get pyodbc to compile and work in Mac OS X including fighting with the setup.py and trying to learn about build directories and other black magic things about Python. Then I stumbled on a discussion of using the Macports to install pyodbc. It turns out that the Macport of pyodbc is called, confusingly, simply odbc. So the install command is this:

    
    sudo port install py27-odbc


OK, so if I had simply installed pyodbc properly with Macports I probably could have saved myself 4 hours of pain. But I did learn some things along the way. Let me capture a few of those so hopefully future adventurers will be spared some pain.

**Debugging:**

**Problem: **At one point I kept getting the following type of error in Python:

    
    pyodbc.Error: ('00000', '[00000] [iODBC][Driver Manager]dlopen({SQL Server}, 6): image not found (0) (SQLDriverConnect)')


**Solution: **The clue here is the [iODBC] bit. iODBC is the default ODBC manager which now comes with Mac OS X. iODBC is a slightly less desirable ODBC manager than unixODBC. Despite elaborate comments found online where folks say, "iODBC works fine for me" most folks agree to use unixODBC and many have tried and failed to make iODBC work as expected. I spent a lot of time trying to figure out how to replace iODBC with unixODBC. It turns out the choice of whether to use iODBC or unixODBC happens when pyodbc is built and installed. Once I dropped back and installed freetds and pydobc as outlined above, I moved on to errors like the following:

    
    ProgrammingError: ('42000', "[42000] [unixODBC][FreeTDS][SQL Server]Login failed for user 'sa'. (18456) (SQLDriverConnectW)")


You can see from that error message that I'm getting errors back through unixODBC and FreeTDS. Huge progress!

**Problem:** How can I tell if FreeTDS is properly installed?

**Solution: ****Use the tsql command line program to connect to your DB. You'll need to do something like the following:**

    
    tsql -S myserver -U username -P mypassword


**The errors returned by tsql are fairly uninformative. You'll likely see simply **

    
    There was a problem connecting to the server


**or **

** **

    
    Error 100 (severity 11):	unrecognized msgno




The good news is that FreeTDS only has one configuration file and only a couple of things are important in that file. The config file for FreeTDS (if installed by Macports) is /opt/local/etc/freetds/freetds.conf




The two things in freetds.conf which I found **REALLY** matter are port number and tds version. For my SQL Server 2008 I needed to set tds version to 7.2 by editing the [global] section of freetds.conf to look like:






    
    [global]
         # TDS protocol version
         tds version = 7.2







Then I found that I also had to include port number for every server I wanted to connect to. This should not be needed because tsql allows passing port number using the -p switch. In my experience the -p switch resulted in a failure to connect, but putting the port number for the server in the freetds.conf file works. Note that my SQL Server instance uses the default port number so I didn't think I would need this at all. But I do. My entry looks like this:



    
    [MYSERVER]
       host = MYSERVER
       port = 1433


If you are connecting to all different vintages of SQL Server you may need to override the global tds version by using a different version in the server specific section. If you want some clues as to which TDS version you should be using, start with [this chart from FreeTDS](http://freetds.schemamania.org/userguide/choosingtdsprotocol.htm). If you get the port settings right and the names right, you'll likely get a > prompt after you use tsql to connect to the DB. If you get a > prompt, you're in pretty good shape!

**Problem:** How can I tell if unixODBC is properly installed?

**Solution: ****First, make sure FreeTDS is installed properly. Seriously. Don't skip that. After you're able to connect using tsql, we need to configure some text files in order to move forward. unixODBC has three types of files to edit (paths assume installation using Macports):**

Global driver configuration:

    
    /opt/local/etc/odbcinst.ini


Global DSN configuration file:

    
    /opt/local/etc/odbc.ini


Local DSN configuration file:

    
    ~/.odbc.ini


I elected to not even bother to create the Local DSN file and do everything from the two global files. What can I say? I'm global, that's just how I roll.

The global driver config file (/opt/local/etc/odbcinst.ini) needs to contain a link to the FreeTDS driver. Mine looks like this:

    
    [FreeTDS]
    Description=FreeTDS Driver for Linux & MSSQL on Win32
    Driver=/opt/local/lib/libtdsodbc.so
    Setup=/opt/local/lib/libtdsodbc.so
    UsageCount=1


From what I can tell, Macports does **NOT** create this file and it has to be manually created.

The DSN configuration file does not strictly have to be created. It's possible to connect without DSN entries. But for the sake of testing, I highly recommend setting up at least one server DSN. You can find the DSN format in a lot of places online. Mine looks like this:

    
    [MYSERVER]
    Description         = Test to SQLServer
    Driver              = FreeTDS
    Trace               = Yes
    TraceFile           = /tmp/sql.log
    Database            = TechnicalProvisions
    Servername          = MYSERVER
    UserName            = myusername
    Password            = mypassword
    Port                = 1433
    Protocol            = 7.2
    ReadOnly            = No
    RowVersioning       = No
    ShowSystemTables    = No
    ShowOidColumn       = No
    FakeOidIndex        = No


After you set up the odbcinst.ini and the odbc.ini you are ready to test unixODBC using isql. To connect do your version of the following at the commend prompt:

    
    isql MYSERVER myusername mypassword


**YOU MUST INCLUDE YOUR USERNAME AND PASSWORD in the command line.** Yes, even though the username and password are in the DSN entry, they must be included in the command line. I lost the better part of an hour trying to change fix a broken connection that wasn't broken, only because I didn't include my username and password. The only error isql returns when username and password are missing is:

    
    [ISQL]ERROR: Could not SQLConnect


Yeah, that's part of why this crap is hard.

Now if you've got isql connecting, you're ready to move on to testing pyodbc in Python. If you've installed pyodbc as outlined above and then set up your ODBC files and FreeTDS and tested each one, this should be a snap! I use Pandas for its DataFrame structure. And after getting each of the previous bits working, I was able to do this:

    
    import pyodbc
    import pandas
    import pandas.io.sql as psql
    cnxn = pyodbc.connect('DSN=MYSERVER;UID=myusername;PWD=mypassword' )
    cursor = cnxn.cursor()
    sql = ("SELECT * FROM dbo.pandasTest")
    df = psql.frame_query(sql, cnxn)


And it worked! But worth noting, just like on the command line, if I failed to include my username and password, I would get a failed connection. In my case it was this:

    
    ---------------------------------------------------------------------------
    Error                                     Traceback (most recent call last)
    in ()
    3 import pandas.io.sql as psql
    4
    ----> 5 cnxn = pyodbc.connect('DSN=MYSERVER' )
    6 cursor = cnxn.cursor()
    7 sql = ("SELECT * FROM dbo.pandasTest")
    Error: ('08001', '[08001] [unixODBC][FreeTDS][SQL Server]Unable to connect to data source (0) (SQLDriverConnectW)')


Which, not unlike the isql error, is not particularly helpful.

Good luck. And may the ODBC be with you!
