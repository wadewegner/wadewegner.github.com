---
author: admin
comments: true
date: 2010-08-25 04:36:37+00:00
layout: post
slug: sql-azure-adds-support-for-database-copy
title: SQL Azure Adds Support for Database Copy
wordpress_id: 764
categories:
- SQL Azure
- Windows Azure Platform
---

The SQL Azure team has announced SQL Azure Service Update 4, which includes database copy, an improved help system, and deployment of Microsoft Project Code-Named “Houston” to multiple data centers. See the [SQL Azure Team Blog](http://blogs.msdn.com/b/sqlazure/archive/2010/08/24/10053883.aspx) for the formal announcement.

 

Customers have been asking for a way to backup databases with SQL Azure for a long time, and the new database copy capabilities will provide a lot of support. From the SQL Azure Team Blog:

 

>   
> 
> **Support for database copy:** Database copy allows you to make a real-time complete snapshot of your database into a different server in the data center. This new copy feature is the first step in backup support for SQL Azure, allowing you to get a complete backup of any SQL Azure database before making schema or database changes to the source database. The ability to snapshot a database easily is our top requested feature for SQL Azure, and goes above and beyond our database center replication to keep your data always available. The MSDN Documentation with more information is entitled: [Copying Databases in SQL Azure](http://msdn.microsoft.com/en-us/library/ff951624.aspx).

 

Good stuff. So, if you want to rename a database, you can do this …

 

  
    
    <span style="color: #008000">// Rename a database</span>



  
    
    ALTER DATABASE database1 modify name=database2









    
… if you want to make a copy of a database, you can do this …






  
    
    <span style="color: #008000">// Create a consistent copy of a database</span>



  
    
    CREATE DATABASE database2 AS COPY OF database1









    
… and finally, if you want to track the copy progress of a database, you can do this …






  
    
    <span style="color: #008000">// Keep track of the copy progress</span>



  
    
    SELECT * FROM sys.dm_database_copies









    
There is also some great [“How-To” documentation](http://msdn.microsoft.com/en-us/library/ee621787.aspx) that’s been published to MSDN that you should take a look at:






  

[Guidelines for Connecting to SQL Azure Database](http://msdn.microsoft.com/en-us/library/ee336282.aspx) 

      
[How to: Connect to SQL Azure Using sqlcmd](http://msdn.microsoft.com/en-us/library/ee336280.aspx) 

      
[How to: Connect to SQL Azure Using ADO.NET](http://msdn.microsoft.com/en-us/library/ee336243.aspx) 

      
[How to: Connect to SQL Azure Through ASP.NET](http://msdn.microsoft.com/en-us/library/ee621781.aspx) 

      
[How to: Connect to SQL Azure Through WCF (ADO.NET) Data Services](http://msdn.microsoft.com/en-us/library/ee621789.aspx) 

      
[How to: Connect to SQL Azure Using PHP](http://msdn.microsoft.com/en-us/library/ff394110.aspx) 

      
[How to: Connect to SQL Azure Using the ADO.NET Entity Framework](http://msdn.microsoft.com/en-us/library/ff951633.aspx)






And don’t forget about “Houston”. [Microsoft project Microsoft Project Code-Named “Houston” (Houston)](http://blogs.msdn.com/b/sqlazure/archive/2010/07/26/10042571.aspx) is a light weight web-based database management tool for SQL Azure. Houston, which runs on top of Windows Azure is now available in multiple datacenters reducing the latency between the application and your SQL Azure database.





Enjoy!
