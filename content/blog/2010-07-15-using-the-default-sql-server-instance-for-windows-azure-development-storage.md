---
author: admin
categories:
- SQL Server
- Windows Azure
comments: true
date: "2010-07-15T18:46:35Z"
slug: using-the-default-sql-server-instance-for-windows-azure-development-storage
title: Using the default SQL Server instance for Windows Azure development storage
wordpress_id: 657
---

This tip isn’t new, but it’s still useful. I found myself building a new development box this week, and I didn’t want to use SQLExpress for the Windows Azure development storage. Instead, I wanted to use the default instance for SQL Server.

 

It’s pretty simple to do this – after you install the Windows Azure SDK and Tools, go to a command prompt and browse to the following folder: C:Program FilesWindows Azure SDKv1.2bindevstore (or wherever you installed the SDK). From there, use the DSInit.exe tool:

 

  

    
    
    DSInit.exe /sqlInstance:.















Remember that the . is a reference to the default instance. If you want to target an instance name, you can use:
    







  


    
    
    DSInit.exe /sqlInstance:YourInstanceName











Now you’ll see that





![DevelopmentStorageDb20090919](https://wadewegner.blob.core.windows.net/wordpress/2010/07/image2.png)





Note: this tip is also helpful for when you get the error message “Failed to create database ‘DevelopmentStorageDb20090919’” during the automatic configuration of Windows Azure development storage.
