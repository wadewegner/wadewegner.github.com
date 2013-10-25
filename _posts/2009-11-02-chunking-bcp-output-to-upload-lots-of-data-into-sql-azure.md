---
author: admin
comments: true
date: 2009-11-02 17:28:44+00:00
layout: post
slug: chunking-bcp-output-to-upload-lots-of-data-into-sql-azure
title: Chunking BCP output to upload lots of data into SQL Azure
wordpress_id: 353
categories:
- Azure
- Cloud
- SQL Azure
- SQL Server
- Windows Azure
---

[_![SQL Azure](http://images.wadewegner.com/wordpress/2009/11/SQLAzure_thumb.png)_](http://images.wadewegner.com/wordpress/2009/11/SQLAzure.png)_Note: This is a guest post from [George Huey](http://www.linkedin.com/pub/george-huey/0/4b0/375), Architect Evangelist in the Developer and Platform Evangelism group._

 

When you upload your data into [SQL Azure](http://www.microsoft.com/windowsazure/sqlazure/), SQL Azure replicates your data to three different locations in order to provide triple redundancy. Therefore, it needs a little more time to get the data in the proper places.

 

One of the things that we found out during a series of Windows Azure Platform Migration Labs held in the [Chicago MTC](http://www.microsoft.com/mtc/locations/Chicago.mspx) is that you cannot upload hundreds of thousands of records without giving SQL Azure time to catch up. Consequently, you have to chunk your data and give SQL Azure time to process each chunk before uploading the next chunk of data.

 

The tool that we used for migrating our customer databases to SQL Azure was the [SQL Azure Migration Wizard](http://sqlazuremw.codeplex.com/). The migration wizard uses BCP to download data from an on premise SQL Server database and then uses BCP to upload the data to SQL Azure. BCP allows you to specify the first row (-F), the last row (-L), and the batch size (-b). These options will allow you to chunk the data beginning uploaded to SQL Azure. For example:

 
    
    BCP MyDb.dbo.Transactions out Transactions.dat -E -q -n –T 





The above command extracts data from table Transactions in the database MyDb. At the end of the BCP output, you will find the number of records copied to file (for example: 2,524,520 rows copied).





In order to upload in chunks, you would do something like this:




    
    BCP  MyDb.dbo.Transactions <a style="color: #0000ff" href="http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=in&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99">in</a>  Transactions.dat -E -q -n –b 5000 –F1 –L250000 -S  tcp:azureserver.ctp.<a style="color: #0000ff" href="http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=database&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99">database</a>.windows.net -U admin@azureserver -P password
    
    
    BCP  MyDb.dbo.Transactions <a style="color: #0000ff" href="http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=in&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99">in</a>  Transactions.dat -E -q -n –b 5000 –F250001 –L500000 -S  tcp:azureserver.ctp.<a style="color: #0000ff" href="http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=database&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99">database</a>.windows.net -U admin@azureserver -P password
    
    
    BCP  MyDb.dbo.Transactions <a style="color: #0000ff" href="http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=in&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99">in</a>  Transactions.dat -E -q -n –b 5000 –F500001 –L750000 -S  tcp:azureserver.ctp.<a style="color: #0000ff" href="http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=database&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99">database</a>.windows.net -U admin@azureserver -P password
    
    
    BCP  MyDb.dbo.Transactions <a style="color: #0000ff" href="http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=in&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99">in</a>  Transactions.dat -E -q -n –b 5000 –F750001 –L1000000 -S  tcp:azureserver.ctp.<a style="color: #0000ff" href="http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=database&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99">database</a>.windows.net -U admin@azureserver -P password
    
    
    BCP  MyDb.dbo.Transactions <a style="color: #0000ff" href="http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=in&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99">in</a>  Transactions.dat -E -q -n –b 5000 –F1000001 –L1250000 -S  tcp:azureserver.ctp.<a style="color: #0000ff" href="http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=database&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99">database</a>.windows.net -U admin@azureserver -P password
    
    
    BCP  MyDb.dbo.Transactions <a style="color: #0000ff" href="http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=in&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99">in</a>  Transactions.dat -E -q -n –b 5000 –F1250001 –L1500000 -S  tcp:azureserver.ctp.<a style="color: #0000ff" href="http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=database&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99">database</a>.windows.net -U admin@azureserver -P password
    
    
    BCP  MyDb.dbo.Transactions <a style="color: #0000ff" href="http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=in&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99">in</a>  Transactions.dat -E -q -n –b 5000 –F1500001 –L1750000 -S  tcp:azureserver.ctp.<a style="color: #0000ff" href="http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=database&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99">database</a>.windows.net -U admin@azureserver -P password
    
    
    BCP  MyDb.dbo.Transactions <a style="color: #0000ff" href="http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=in&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99">in</a>  Transactions.dat -E -q -n –b 5000 –F1750001 –L2000000 -S  tcp:azureserver.ctp.<a style="color: #0000ff" href="http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=database&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99">database</a>.windows.net -U admin@azureserver -P password
    
    
    BCP  MyDb.dbo.Transactions <a style="color: #0000ff" href="http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=in&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99">in</a>  Transactions.dat -E -q -n –b 5000 –F2250001 –L2500000 -S  tcp:azureserver.ctp.<a style="color: #0000ff" href="http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=database&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99">database</a>.windows.net -U admin@azureserver -P password
    
    
    BCP  MyDb.dbo.Transactions <a style="color: #0000ff" href="http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=in&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99">in</a>  Transactions.dat -E -q -n –b 5000 –F2500001 –L2524520 -S  tcp:azureserver.ctp.<a style="color: #0000ff" href="http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=database&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99">database</a>.windows.net -U admin@azureserver -P password
    





Note, you will have to put some kind of delay between BCP commands to give SQL Azure time to store the data (say 15 seconds). You will probably find that sometimes the 15 seconds is not enough time and that, during the upload of one of your BCP chunks, SQL Azure might shut it down. If that happens you will see something like this happen:




    
    5000 <a style="color: #0000ff" href="http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=rows&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99">rows</a> sent <a style="color: #0000ff" href="http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=to&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99">to</a> <a style="color: #0000ff" href="http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=SQL&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99">SQL</a> Server. Total sent: 145000
    
    
    5000 <a style="color: #0000ff" href="http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=rows&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99">rows</a> sent <a style="color: #0000ff" href="http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=to&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99">to</a> <a style="color: #0000ff" href="http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=SQL&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99">SQL</a> Server. Total sent: 150000
    
    
    5000 <a style="color: #0000ff" href="http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=rows&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99">rows</a> sent <a style="color: #0000ff" href="http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=to&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99">to</a> <a style="color: #0000ff" href="http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=SQL&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99">SQL</a> Server. Total sent: 155000
    
    
    <a style="color: #0000ff" href="http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=SQLState&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99">SQLState</a> = S1000, NativeError = 21
    
    
    Error = [Microsoft][<a style="color: #0000ff" href="http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=SQL&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99">SQL</a> Server Native Client 10.0][<a style="color: #0000ff" href="http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=SQL&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99">SQL</a> Server]Warning: Fatal error 40501 occurred <a style="color: #0000ff" href="http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=at&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99">at</a> Oct 30 2009  4:15PM. Note the error <a style="color: #0000ff" href="http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=and&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99">and</a> <a style="color: #0000ff" href="http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=time&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99">time</a>, <a style="color: #0000ff" href="http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=and&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99">and</a> contact your system administrator.
    
    
    BCP copy <a style="color: #0000ff" href="http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=in&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99">in</a> failed 





From the above BCP output, you will see that a total of 155,000 rows were sent before SQL Azure closed the connection. Thus you would have to adjust your next BCP command to start at your –F value + 155000.





While this process works reasonably well, it can make the process of uploading data a little tedious if you have a large number of tables with a large number of records per table. In order to simplify the process, we have modified the [SQL Azure Migration Wizard](http://sqlazuremw.codeplex.com/) to do all of this work for you. It allows you to specify the chunk size, the batch size, and the time to wait between BCP chunks in SQLAzureMW.exe.Config. It also catch BCP errors and adjust for records processed and then retry.





Try it out, review the source code, and be sure to provide some good feedback!
