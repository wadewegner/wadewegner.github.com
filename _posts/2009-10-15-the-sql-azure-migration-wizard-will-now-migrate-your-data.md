---
author: admin
comments: true
date: 2009-10-15 17:50:21+00:00
layout: post
slug: the-sql-azure-migration-wizard-will-now-migrate-your-data
title: The SQL Azure Migration Wizard will now migrate your data!
wordpress_id: 301
categories:
- Azure
- SQL Azure
- Tools
---

![George Huey, when he's not writing code!](https://wadewegner.blob.core.windows.net/wordpress/2009/10/GeorgeHuey.jpg) 

Last month I [blogged about the SQL Azure Migration Wizard](http://www.wadewegner.com/index.php/2009/09/01/sql-azure-migration-wizard/) created by George Huey. This tool helps you to migrate your SQL Server database into [SQL Azure](http://www.microsoft.com/azure/sql.mspx) and is [available up on Codeplex](http://sqlazuremw.codeplex.com/). To date, this tool has been downloaded almost a thousand times!

 

The number one request for the migration wizard was the ability to replicate your data up to SQL Azure (in addition to the SQL schema, which it already does). Unfortunately, the original SQL Azure CTP didn't support BCP, and the data migration process was very difficult and required an SSIS package to copy the data into SQL Azure.

 

With yesterday's release of SQL Azure CTP 2, however, [SQL Azure now supports BCP](http://www.wadewegner.com/index.php/2009/10/15/sql-azure-database-is-feature-complete/)!

 

Minutes after CTP 2 went live, George published a new version of migration wizard that takes advantage of BCP to enable you to migrate not only your SQL Server 2005 / 2008 database objects but your data as well! migration wizard allows you to turn on or off data migration via the application configuration file or during runtime via the options page. When you select data migration, the export and import process can be quite lengthy. migration wizard kicks off the export and import process on a back ground thread and will display the BCP results in the program window. At any time during the process, you can hit the cancel button to cancel the background process. As migration wizard processes information, it will display the results to the program window.

 

Here are the release notes for version 1.0 (and 1.1):

 

  
  * Added data migration via BCP. Note that when you specify your SQL Azure username, specify your user name as "username@server". Also note that data migration only works on the latest release of SQL Azure (Server Location: South Central US).
   
  * Modified App.Config to allow you to specify your Options. For example: If you do not want to migrate data, you can turn this off by modifying the App.Config file and changing ScriptData to false.
   
  * Added a cancel button so that you can cancel while processing.
   
  * Added a scroll toggle so that during processing you can keep the control from scrolling down to the bottom.
   
  * Fixed an error in BCP command to allow the passing of the SQL Server instance name.
   
  * Added color to the SQL results to better identify error messages.
 

If you're using SQL Azure, go and grab the new version of the [SQL Azure Migration Wizard](http://sqlazuremw.codeplex.com/)! For detailed instructions on how to use the wizard, take a look at the [SQL Azure Migration Wizard whitepaper](http://sqlazuremw.codeplex.com/Release/ProjectReleases.aspx?ReleaseId=32334#DownloadId=86938).
