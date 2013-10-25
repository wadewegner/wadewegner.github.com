---
author: admin
comments: true
date: 2009-09-02 03:29:38+00:00
layout: post
slug: sql-azure-migration-wizard
title: SQL Azure Migration Wizard
wordpress_id: 155
categories:
- Azure
- SQL Azure
- Windows Azure
---

In my opinion, [SQL Azure](http://www.microsoft.com/azure/sql.mspx) is the "secret sauce" of the [Windows Azure Platform](http://www.azure.com/).  Through SQL Azure, you have the ability to store your data in a true cloud-based relational database.  It's as simple as deploying your database to SQL Azure and updating your connection string to point to the new database - done!  I think this really provides a core differentiator between the Windows Azure Platform and other cloud vendors.

Nevertheless, despite all the benefits you get from SQL Azure today, we're still in CTP - SQL Azure is not yet released!  Consequently, there are a few places where it's a bit rough around the edges.  In particular, the tooling through SQL Server Management Studio leaves a bit to be desired and some of the differences between what's supported in SQL Server 2005/2008 versus SQL Azure can be a bit confusing.  It took me hours to manually modify the SQL script of a moderately complex database so that I could deploy it into SQL Azure.  What was missing was a good tool that would help with this process.  Fortunately, my good friend and coworker George Huey has made this process much easier by creating the [SQL Azure Migration Wizard](http://sqlazuremw.codeplex.com/).

At it's core, the [SQL Azure Migration Wizard](http://sqlazuremw.codeplex.com/) help you migrate your SQL Server databases into SQL Azure.  Point to your local database, walk through the wizard, and the end result is a SQL script you can deploy to SQL Azure - in fact, the tool will even deploy it for you.  I used this tool against the same database that took me hours to manually migrate and the process only took a few minutes.

George has made the [SQL Azure Migration Wizard](http://sqlazuremw.codeplex.com/) available on CodePlex - you can download the source code and/or the binaries.  Additionally, he has put together the following screen cast to demonstrate how easy it is to use this tool.  Take a look:


        
            
            
            
            
            
            
            

             


                    ![](http://9puoxg.blu.livefilestore.com/y1pEmjy1ft6gmwROn80Y_0rZYusknHJSq7B_40r0ke-CuBxX4af_qv23TsiwxHDLGlenYmdy2iYoyhK0wSki2gYmA/SQLAzureMW_Thumb.jpg)
                    ![](http://9puoxg.blu.livefilestore.com/y1pwtSE8VmyuZfe1fDPMgoyhQbyviNGfoPIC4cG8gctI6xZMTv_UBZ5GL8jYWAwE4qKANSkFv_d6i-rnXJ2TyVuTXUrx63JWwHH/Preview.png)
                    


                    


                    ![Get Microsoft Silverlight](http://go2.microsoft.com/fwlink/?LinkId=108181)
                    

                    [
                        ![Get Microsoft Silverlight]()
                    ](http://go2.microsoft.com/fwlink/?LinkID=124807)
             


        


I would like to point out that, while the [SQL Azure Migration Wizard](http://sqlazuremw.codeplex.com/) is a fabulous tool that will save you a lot of time when migrating your databases to SQL Azure, it's a temporary solution.  The SQL Server and SQL Azure teams will solve many of these problems before SQL Azure releases later this year. 

Personally, I'd like to thank George Huey for all his hard work putting together this tool.  If you try out the tool and agree, or if you have suggestions on how the tool can provide even more value, please leave some [feedback on the CodePlex site](http://sqlazuremw.codeplex.com/).

If you'd like to learn more about SQL Azure, and start using a relational database in the cloud, please take a look at the following resources:



	
  * [Try SQL Azure Database CTP Today](http://blogs.msdn.com/ssds/archive/2009/08/18/9874133.aspx) & [SQL Azure Invitation Codes](http://english.zachskylesowens.net/2009/08/19/sql-azure-invitation-codes/)

	
  * [SQL Azure on MSDN](http://msdn.microsoft.com/en-us/library/ee336279.aspx)

	
  * [TSQL Support in SQL [Azure]](http://blogs.msdn.com/ssds/archive/2009/07/07/9823115.aspx)


I hope this helps!
