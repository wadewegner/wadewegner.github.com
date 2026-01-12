---
title: "SQL Azure Migration Wizard"
date: 2009-09-02T00:00:00-08:00
draft: false
description: "From the whiteboard to the keyboard"
---

In my opinion, [SQL Azure](http://www.microsoft.com/azure/sql.mspx) is the “secret sauce” of the [Windows Azure Platform](http://www.azure.com/).  Through SQL Azure, you have the ability to store your data in a true cloud-based relational database.  It’s as simple as deploying your database to SQL Azure and updating your connection string to point to the new database - done!  I think this really provides a core differentiator between the Windows Azure Platform and other cloud vendors.

Nevertheless, despite all the benefits you get from SQL Azure today, we’re still in CTP - SQL Azure is not yet released!  Consequently, there are a few places where it’s a bit rough around the edges.  In particular, the tooling through SQL Server Management Studio leaves a bit to be desired and some of the differences between what’s supported in SQL Server 2005/2008 versus SQL Azure can be a bit confusing.  It took me hours to manually modify the SQL script of a moderately complex database so that I could deploy it into SQL Azure.  What was missing was a good tool that would help with this process.  Fortunately, my good friend and coworker George Huey has made this process much easier by creating the [SQL Azure Migration Wizard](http://sqlazuremw.codeplex.com/).

At it’s core, the [SQL Azure Migration Wizard](http://sqlazuremw.codeplex.com/) help you migrate your SQL Server databases into SQL Azure.  Point to your local database, walk through the wizard, and the end result is a SQL script you can deploy to SQL Azure - in fact, the tool will even deploy it for you.  I used this tool against the same database that took me hours to manually migrate and the process only took a few minutes.

George has made the [SQL Azure Migration Wizard](http://sqlazuremw.codeplex.com/) available on CodePlex - you can download the source code and/or the binaries.

I would like to point out that, while the [SQL Azure Migration Wizard](http://sqlazuremw.codeplex.com/) is a fabulous tool that will save you a lot of time when migrating your databases to SQL Azure, it’s a temporary solution.  The SQL Server and SQL Azure teams will solve many of these problems before SQL Azure releases later this year.

Personally, I’d like to thank George Huey for all his hard work putting together this tool.  If you try out the tool and agree, or if you have suggestions on how the tool can provide even more value, please leave some [feedback on the CodePlex site](http://sqlazuremw.codeplex.com/).

If you’d like to learn more about SQL Azure, and start using a relational database in the cloud, please take a look at the following resources:

- [Try SQL Azure Database CTP Today](http://blogs.msdn.com/ssds/archive/2009/08/18/9874133.aspx) & [SQL Azure Invitation Codes](http://english.zachskylesowens.net/2009/08/19/sql-azure-invitation-codes/)
- [SQL Azure on MSDN](http://msdn.microsoft.com/en-us/library/ee336279.aspx)
- [TSQL Support in SQL [Azure]](http://blogs.msdn.com/ssds/archive/2009/07/07/9823115.aspx)

I hope this helps!
