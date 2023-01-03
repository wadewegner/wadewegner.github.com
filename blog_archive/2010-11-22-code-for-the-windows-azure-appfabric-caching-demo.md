---
author: admin
categories:
- Caching
- PDC10
- Windows Azure
- Windows Azure AppFabric
comments: true
date: "2010-11-22T18:03:41Z"
slug: code-for-the-windows-azure-appfabric-caching-demo
title: Code for the Windows Azure AppFabric Caching demo
wordpress_id: 798
---

This post is long overdo. At PDC10 I recorded the session [Introduction to Windows Azure AppFabric Caching](http://player.microsoftpdc.com/Session/1f607983-c6eb-4d9f-b644-55247e8adda6), where I introduced the new Caching service. As part of the presentation I gave a demo where I updated an existing ASP.NET web application running in a Windows Azure web role to use the caching service. For details on the caching service, please [watch the presentation](http://player.microsoftpdc.com/Session/1f607983-c6eb-4d9f-b644-55247e8adda6).

 

In this presentation I showed three uses of the caching service:

 

  
  1. Using the Caching service as the session state provider. 
   
  2. Using the Caching service as an explicit cache for reference data stored in SQL Azure. 
   
  3. Using the Caching service along with the local cache feature to store resource data in the web client. 
 

If you want to see this application running in Windows Azure, take a look here: [http://cachedemo.cloudapp.net/](http://cachedemo.cloudapp.net/)

 

I had meant to immediately post the code to this application so that you can try it out yourself, but alas PDC10 and TechEd EMEA set me back quite a bit. So, without further ado, here are the files you’ll need:

 

 

    
**CacheService.zip** contains the solution and projects required to run the application. **NothwindProducts.zip** contains the SQL scripts required to setup the SQL Azure (if you want to use SQL Server, you’ll have to update the connection string accordingly) database you’ll need to use.

 

You’ll have to update a few items in the **web.config** file. Search and replace the following items:

 

  
  * YOURCACHE – get this from the [AppFabric portal](http://portal.appfabriclabs.com/)
   
  * YOURTOKEN – get this from the [AppFabric portal](http://portal.appfabriclabs.com/)
   
  * YOURDATABASE (2 instances) – get this from the [SQL Azure portal](http://sql.azure.com/)
   
  * YOURUSERNAME – get this from the [SQL Azure portal](http://sql.azure.com/)
   
  * YOURPASSWORD – get this from the [SQL Azure portal](http://sql.azure.com/)
 

Once you have made these updates, hit F5 to run locally.

 

**Please note**: when you run this locally against the Caching service, you will encounter significant network latency! This is to be expected, as you are making a number of network hops to get to the data center in order to reach your cache. To get the best results, deploy your application to **South Central US** as this is where the AppFabric LABS portal creates your cache.

 

If you have any problems or questions, please let me know.
