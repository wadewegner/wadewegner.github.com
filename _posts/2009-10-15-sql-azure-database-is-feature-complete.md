---
author: admin
comments: true
date: 2009-10-15 14:53:58+00:00
layout: post
slug: sql-azure-database-is-feature-complete
title: SQL Azure Database is Feature Complete!
wordpress_id: 289
categories:
- Azure
- SQL Azure
---

[![SQL Azure](https://wadewegner.blob.core.windows.net/wordpress/2009/10/image_2_thumb2.png)](https://wadewegner.blob.core.windows.net/wordpress/2009/10/image_22.png) As many of you probably already know, the CTP of [SQL Azure](http://www.microsoft.com/azure/sql.mspx) is now feature complete! Fantastic work by the SQL Azure product team, as it was only in August that they announced the opening of the SQL Azure Database CTP!

 

I recommend reading the entire announcement on the [SQL Azure team's blog](http://blogs.msdn.com/ssds/default.aspx). Here are some brief notes on the announcement:

 

  
  * **Details**             
    * CTP 2 (announced yesterday, October 15th) represents the complete set of features that will be available at commercial launch this November at [PDC](http://microsoftpdc.com/). 
       
    * SQL Azure Database is now running on one of the go-live production clusters. 
       
    * This environment will automatically roll into the fully supported production environment at PDC. 
       
   
  * **Key features added in CTP 2**             
    * **Firewall support **– You can now specify an allow list of IP addresses, preventing unauthorized users from reaching your SQL Azure databases. 
       
    * **Bulk Insert **– [SqlBulkCopy](http://msdn.microsoft.com/en-us/library/system.data.sqlclient.sqlbulkcopy.aspx), or BCP, is now supported. This makes data migration much faster. 
       
    * **Database Edition Selection **– You can now select either the Web Edition (1 GB) or Business Edition (10 GB) version of SQL Azure Database. 
       
    * **Additional T-SQL support** – A host of new functionality has been exposed. Take a look at [Transact-SQL Reference](http://msdn.microsoft.com/en-us/library/ee336281.aspx) for details. 
       
 

Also, you no longer get the "SET ANSI NULLS" error when connecting a query window via SSMS!

 

If you haven't tried SQL Azure yet, now's the time!
