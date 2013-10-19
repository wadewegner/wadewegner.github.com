---
author: admin
comments: true
date: 2007-06-19 19:15:30+00:00
layout: post
slug: commerce-server-2007-starter-site-installation-error-error-importing-object-data-for-transaction-config
title: 'Commerce Server 2007 Starter Site installation error: Error importing object
  data for Transaction Config'
wordpress_id: 69
categories:
- Commerce Server
---

During the installation of the Commerce Server 2007 Starter Site, I received the following error in the Pup.log file:




> 

> 
> [10:57:50] Error importing object data for Transaction Config from file C:Documents and SettingsAdministratorLocal SettingsTempTransaction Config 8004E024:COM+ activation failed because the activation could not be completed in the specified amount of time. (Exception from HRESULT: 0x8004E024)




I also saw the following error in the Application Log, written at the exact same time:




> 

> 
> Failed to import orders configuration data. System.Runtime.InteropServices.COMException (0x8004E024): COM+ activation failed because the activation could not be completed in the specified amount of time. (Exception from HRESULT: 0x8004E024)  
at System.Runtime.InteropServices.Marshal.ThrowExceptionForHRInternal(Int32 errorCode, IntPtr errorInfo)  
at System.EnterpriseServices.Thunk.Proxy.CoCreateObject(Type serverType, Boolean bQuerySCInfo, Boolean& bIsAnotherProcess, String& uri)  
at System.EnterpriseServices.ServicedComponentProxyAttribute.CreateInstance(Type serverType)  
at System.Runtime.Remoting.Activation.ActivationServices.IsCurrentContextOK(Type serverType, Object[] props, Boolean bNewObj)  
at Microsoft.CommerceServer.Orders.DataManagement.ServerOrderSystem.ImportRegionCodes(String txnConfigResourceConnectionString, DataTable regionCodesTable)  
at Microsoft.CommerceServer.Orders.DataManagement.ServerOrderSystem.ImportConfigurationData(Stream stream, String txnConfigResourceConnectionString) 

> 
> For more information, see Help and Support Center at http://go.microsoft.com/fwlink/events.asp.




I should note that, rather than install the Starter Site to a virtual directory under the Default Web Site, I decided to create a new Web site to which I installed the Starter Site. The reason I did this is I already had the CSharpSite installed to the Default Web site, and I didn't want the two sites interfering with each other.




Also, to provide other information regarding this installation (just to show I'm not making an obvious mistake):






  * I ran it as the local Administrator account; full access to the system and SQL Server

  * Windows Server 2003 Standard R2; SQL Server 2005 Standard SP2; Commerce Server 2007 Developer edition

  * All hot fixes have been applied to the O/S

  * Instructions provided by the [Commerce Server 2007 Start Site Installation Guide](http://go.microsoft.com/fwlink/?LinkId=71818) were followed

  * The CSharpSite was previously installed successfully

  * Everything is running locally (CS 2007, SQL Server 2005, BizTalk 2006), so it's not as if DTC (or some nefarious networking issue) is causing problems



This was the only error I received during the installation. I took note of it, then continued onward.




Lo and behold, when I went to test the installation of the Starter Site, I received the following error:




> 

> 
> The specified value for the newOrderStatus attribute is not valid. The value provided was: 'NewOrder'. Please make sure an entry for this value exists in the AllowedStatus table in the transaction config database. 




So, I decided to investigate. Sure enough, the tables in the StarterSite_transactionconfig database were empty. Specifically, the AllowedStatus, RegionCodes, and StatusManager tables did not contain any data. So, I created scripts based on the data contained in these tables in the CSharpSite_transactionconfig database (which installed without any problems).




**[Update] Note: see below for the actual resolution to this problem. Although the following steps will correctly resolve the problem, there's a better way.**




For example, here's the script generated for the AllowedStatus table (it's a bit more extensive for the RegionCodes table -- e-mail me if you need a copy of the script):




> 

> 
> INSERT INTO [StarterSite_transactionconfig].[dbo].[AllowedStatus] ([Status]) VALUES ('Cancelled')  
INSERT INTO [StarterSite_transactionconfig].[dbo].[AllowedStatus] ([Status]) VALUES ('InProcess')  
INSERT INTO [StarterSite_transactionconfig].[dbo].[AllowedStatus] ([Status]) VALUES ('NewOrder')  
INSERT INTO [StarterSite_transactionconfig].[dbo].[AllowedStatus] ([Status]) VALUES ('Rejected')  
INSERT INTO [StarterSite_transactionconfig].[dbo].[AllowedStatus] ([Status]) VALUES ('Shipped')  
INSERT INTO [StarterSite_transactionconfig].[dbo].[AllowedStatus] ([Status]) VALUES ('Submitted')




After inserting the missing data into these three tables, and running iisreset from the command-line, the Starter Site ran without any problems.


I still plan to try and investigate the underlying cause of the problem ... when I learn more, I'll update this post. At the very least, I wanted to post this solution.


I hope this helps!


**[Update] The _actual_ solution to this problem.**


Shortly after going through the previous steps, I performed the [this Google search](http://www.google.com/search?hl=en&rls=com.microsoft%3Aen-us%3AIE-SearchBox&rlz=1I7GGIG&q=commerce+server+%22COM%2B+activation+failed+because+the+activation+could+not+be+completed+in+the+specified+amount+of+time.%22) and noticed the following in the [Commerce Server 2007 Readme file](http://download.microsoft.com/download/3/a/4/3a46267f-9640-4623-86a1-c63bbfea9e1d/Microsoft%20Commerce%20Server%202007%20Readme.html):


> 

> 
> Unpacking a site might not populate the AllowedStatus table  
The following error message might occur in the site packager log file:

> 
> "8004E024:COM+ activation failed because the activation could not be completed in the specified amount of time. (Exception from HRESULT: 0x8004E024)"

> 
> This error message indicates that Commerce Server could not populate the AllowedStatus table in the transactionconfig database. To populate the AllowedStatus table, open SQL Server Management Studio (open SQL Query Analyzer if you are using SQL Server 2000), and run the regiondata SQL script against the transactionconfig database. The regiondata script is located at %COMMERCE_SERVER_ROOT%SDKSamplesSiteCreate.




So, it appears to be a known issue (previously unknown to me, though!) and easy to resolve!


What's the moral of this story? Make sure to _thoroughly_ research errors before you try to hack the solution yourself (or blog about it), even if you accidentally get the resolution correct.


I hope this saves someone the 30 minutes it cost me!
