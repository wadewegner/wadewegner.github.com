---
author: admin
comments: true
date: 2007-07-28 20:37:35+00:00
layout: post
slug: commerce-server-staging-error-operation-must-use-an-updateable-query
title: 'Commerce Server Staging error: Operation must use an updateable query'
wordpress_id: 50
categories:
- Commerce Server
- Tools
- Troubleshooting
---

I encountered an error today while setting up a project within the Commerce Server Staging. I haven't encountered it before, which leads me to believe that I must have done something abnormal on my development virtual machine.


> _[Commerce Server Staging](http://msdn2.microsoft.com/en-us/library/ms942744.aspx) (CSS) is a service that allows you to synchronize content and data between many separate environments. Utilizing CSS, you can reliably update both Web content and business data from the source to one or more destinations._

Using the CSS MMC, I created a simple project that stages my StarterSite content from my local directory (e.g. C:\Inetpub\wwwroot\StarterSite) to a temporary folder (e.g. C:\TempDestination). It was meant to be the start of a proof-of-concept for a more complex network topology. However, I almost immediately received an error when I attempted to start replication:

[![CSS: Unspecified Error](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/CommerceServerStagingerrorOperationmustu_CDAE/image_thumb_1.png)](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/CommerceServerStagingerrorOperationmustu_CDAE/image_1.png)

I just love errors like this! Gives me something to research and figure out ...

Fortunately, more valuable information was added to the application log:
 
	Event Type: Error  
	Event Source: Commerce Server Staging  
	Event Category: None  
	Event ID: 61208  
	Computer: CS2007  
	Description:  
	Error occurred with the database StagingLog.mdb. Error is: System.Data.OleDb.OleDbException: Operation must use an updateable query.  
	at System.Data.OleDb.OleDbCommand.ExecuteCommandTextForSingleResult(tagDBPARAMS dbParams, Object& executeResult)  
	at System.Data.OleDb.OleDbCommand.ExecuteCommandText(Object& executeResult)  
	at System.Data.OleDb.OleDbCommand.ExecuteCommand(CommandBehavior behavior, Object& executeResult)  
	at System.Data.OleDb.OleDbCommand.ExecuteReaderInternal(CommandBehavior behavior, String method)  
	at System.Data.OleDb.OleDbCommand.ExecuteNonQuery()  
	at Microsoft.CommerceServer.Staging.Internal.ProjectDeploymentLog.ModifyLog(String projectName, String status).

While this isn't the most explicit error, it provided enough information to track down the problem.

You might be surprised to see that this error refers to an Access database. CSS makes use of Access database files to [store events and information](http://msdn2.microsoft.com/en-us/library/bb219263.aspx) it audits during the staging process. Specifically, there are these two Access database files:

  * StagingLog.mdb
    * Found here: C:Program FilesMicrosoft Commerce Server 2007StagingData
    * Stores internal replication information used by CSS
  * events.mdb
    * Found here: C:Program FilesMicrosoft Commerce Server 2007StagingEvents
    * Stores staging information that is made available to reports

Searching on the keywords "[mdb operation must use an updateable query](http://www.google.com/search?hl=en&rlz=1T4GGIG_enUS227US227&q=mdb+operation+must+use+an+updateable+query)" led me to [KB article 175168](http://support.microsoft.com/kb/175168). This article indicated that the problem is most likely caused by a user not having the proper permissions to open the StagingLog.mdb file.

In comes [File Monitor](http://www.microsoft.com/technet/sysinternals/utilities/filemon.mspx) (aka FileMon), by [SysInternals](http://www.microsoft.com/technet/sysinternals/default.mspx), to the rescue. Seriously folks, save yourself the agony and get to know the tools available from Microsoft and SysInternals.

I was able to capture the following "Access Denied" message via File Monitor:

[![image](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/CommerceServerStagingerrorOperationmustu_CDAE/image_thumb_3.png)](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/CommerceServerStagingerrorOperationmustu_CDAE/image_3.png)

**Solution:** Turns out that the NT AUTHORITY\NETWORK SERVICE has been attempting to open the StagingLog.mdb file to no avail. After giving the NT AUTHORITY\NETWORK SERVICE the rights to Modify the StagingLog.mdb file ...

[![image](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/CommerceServerStagingerrorOperationmustu_CDAE/image_thumb_4.png)](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/CommerceServerStagingerrorOperationmustu_CDAE/image_4.png)

... I am now able to successfully start my CSS project and stage my StarterSite data to a separate folder.

**UPDATE:** Rather than only giving access to the StagingLog.mdb file, give the NETWORK SERVICE account access to the entire Data folder where the MDB file is located. Otherwise it will have issues trying to write out an LDF (Access log file) file when updates are made.

Hopefully this not only helps someone resolve this particular problem, but also shows how useful it is to equip yourself with good debugging tools!

Good luck!