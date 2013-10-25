---
author: admin
comments: true
date: 2007-06-23 19:23:16+00:00
layout: post
slug: problem-with-the-commerce-server-2007-sp1-installation
title: Problem with the Commerce Server 2007 SP1 Installation
wordpress_id: 64
---

As was [announced yesterday](http://www.wadewegner.com/PermaLink,guid,1904b877-d1d4-4841-87d2-8b4ecd826965.aspx), Service Pack 1 has been released for Commerce Server 2007. This service pack includes support for Windows Vista, the ability to create [web application projects in Visual Studio 2005](http://msdn2.microsoft.com/en-us/asp.net/aa336618.aspx), performance and security enhancements, and a slew of bug fixes.

I have detailed the process for installing SP1 and upgrading your CS sites in the following posts: [Commerce Server 2007 Service Pack 1 (SP1) Walkthrough](http://www.wadewegner.com/PermaLink,guid,3e9bb1c9-4c0f-4468-af92-68ecf4db73d7.aspx) and [Commerce Server 2007 Upgrade Wizard (post SP1 install) Walkthrough](http://www.wadewegner.com/PermaLink,guid,d4fe3f01-c318-4958-b46b-4e2a4b8827c0.aspx).

While installing SP1 I encountered a rather strange problem. Let me try to explain how the problem occurred, and what I did to resolve it.

To begin, I decided to upgrade my Commerce Server 2007 virtual machine to SP1. To begin, I did the following:

1. Followed the steps I detail in my post [Commerce Server 2007 Service Pack 1 (SP1) Walkthrough](http://www.wadewegner.com/PermaLink,guid,3e9bb1c9-4c0f-4468-af92-68ecf4db73d7.aspx). In fact, these screen shots were taken during this initial installation of SP1.

2. After I installed SP1, I immediately launched the Upgrade Wizard.

3. For the most part, I followed the steps outlined in my post [Commerce Server 2007 Upgrade Wizard (post SP1 install) Walkthrough](http://www.wadewegner.com/PermaLink,guid,d4fe3f01-c318-4958-b46b-4e2a4b8827c0.aspx). However, instead of upgrading both my StarterSite and CSharpSite, I only upgraded my CSharpSite and then rebooted my machine.

Why did I reboot the machine after only upgrading one site? Well, I'm not exactly sure. Part of me wanted to see what would happen with only one site upgraded, and another part wanted to upgrade one site at a time while rebooting in-between. Regardless, I rebooted after installing only the CSharpSite.

After rebooting, here was my first indication that something was wrong.  
  
![At least one service or driver failed during system startup.](http://images.wadewegner.com/wordpress/content/binary/WindowsLiveWriter/IssueswithCommerceServer2007SP1Installat_A115/Error_1.gif)

So, I immediately went to the Event Viewer. I saw a number of errors involving the SQLSERVERAGENT, Commerce Server, and ENTSSO. Here's a look at the various errors in the log (the reboot occurred around 10:52 am ... some of these other errors were thrown from my playing around before I resolved the problem):

![Event Viewer errors](http://images.wadewegner.com/wordpress/content/binary/WindowsLiveWriter/IssueswithCommerceServer2007SP1Installat_A115/Errors_1.gif)

I should note that the system was previously very healthy, as I use this VM all the time while writing the book _[Professional Commerce Server 2007](http://www.wadewegner.com/PermaLink,guid,96042b54-9859-4ea8-8497-5dab8033f405.aspx)_ for WROX. In fact, I had been using the computer with no problems immediately before installing the service pack.

So, at this point it was clear that:

1. The SQL Server Agent was no longer able to start-up.  
  
	SQLServerAgent could not be started (reason: Unable to connect to server '(local)'; SQLServerAgent cannot start).  

2. Enterprise Single-Sign On (ENTSSO) was also having issues starting because it couldn't communicate to SQL Server 2005.  

	The SSO service failed to start.  
	
	Error Code: 0x800710D9, Unable to read from or write to the database.  

3. Commerce Server was complaining that I hadn't updated the StarterSite yet (this one is to be expected, as I hadn't updated it yet):  
  
		Error in Commerce Administration Object : Description - 'The following resources are old. They will not function correctly until they are upgraded:  
		Product Catalog (7.1.0)  
		Marketing (7.0.0)

Obviously, something here is wrong, and SQL Server seemed to be at the heart of it.

The next thing I did was try to connect to SQL Server 2005. Using the SQL Server Management Studio, I tried to login with my Windows credentials. I wasn't surprised to see that I couldn't:

![A connection was successfully established to the server, but then an error occured during the pre-login handshake. (provider: Shared Memory Provider, error: 0 - No process is on the other end of the pipe.) (Microsoft SQL Server, Error: 223)](http://images.wadewegner.com/wordpress/content/binary/WindowsLiveWriter/IssueswithCommerceServer2007SP1Installat_A115/SQLError_1.gif)

	A connection was successfully established to the server, but then an error occurred during the pre-login handshake. (provider: Shared Memory Provider, error: 0 - No process is on the other end of the pipe.) (Microsoft SQL Server, Error: 223)" (copied directly from dialog window)

I have never seen this error before, but wasn't surprised to see that I couldn't find anything anything on [Google](http://www.google.com/search?hl=en&rls=com.microsoft%3Aen-us%3AIE-SearchBox&rlz=1I7GGIG&q=%22A+connection+was+successfully+established+to+the+server%2C+but+then+an+error+occurred+during+the+pre-login+handshake.+%28provider%3A+Shared+Memory+Provider%2C+error%3A+0+-+No+process+is+on+the+other+end+of+the+pipe.%29+%28Microsoft+SQL+Server%2C+Error%3A+223%29%22) or [Live](http://search.live.com/results.aspx?q=%22A+connection+was+successfully+established+to+the+server%2C+but+then+an+error+occurred+during+the+pre-login+handshake.+%28provider%3A+Shared+Memory+Provider%2C+error%3A+0+-+No+process+is+on+the+other+end+of+the+pipe.%29+%28Microsoft+SQL+Server%2C+Error%3A+223%29%22&form=QBNO) (although, hopefully this post will come up in the future). Sure, various iterations of the first part can be found, but nothing seems applicable to this situation.

So, I decide to poke around. What bothered me was that SQL Server (MSSQLSERVER) was running yet I couldn't connect to it, and the SQL Server Agent (MSSQLSERVER) itself couldn't start. Even my attempts to manually start the service failed. Here's a look at SQL Server Configuration Manager:

![SQL Server Configuration Manager](http://images.wadewegner.com/wordpress/content/binary/WindowsLiveWriter/IssueswithCommerceServer2007SP1Installat_A115/Config_1.gif)

Also, I attempted to connect to Commerce Server by running the Upgrade Wizard ... it didn't start, nor did I receive an error. It simply didn't start.

For some reason, one thing jumped out at me -- I was running as LocalSystem rather than NT AUTHORITYNetworkService. Could this have something to do with it?

Not having any other ideas, I changed both SQL Server and SQL Server Agent to "Log on as" the "Network Service" (NT AUTHORITYNetworkService) instead of the LocalSystem account. I then restarted SQL Server 2005 and the SQL Server Agent. Lo and behold, the SQL Server Agent started-up!

![Config2](http://images.wadewegner.com/wordpress/content/binary/WindowsLiveWriter/IssueswithCommerceServer2007SP1Installat_A115/Config2_1.gif)

A little baffled, but encouraged that I was on track to resolving the problem (even if I don't understand the root cause), I tried to connect to SQL Server 2005. Sure enough, I was able to login to the database server and interact with the various databases. I also attempted to connect to the Commerce Server 2007 Upgrade Wizard, which immediat
ely started with no problems.

Since I wasn't too confident with the state of the computer (given that some services still weren't started), I decided to restart the computer. It restarted without a single error, and I was able to run the Upgrade Wizard successfully to upgrade the StarterSite.

At this point I don't quite know why there was a problem with the LocalSystem account after I installed SP1 for Commerce Server 2007. I intend to do a little more research to look into it.

Have any of you encountered this problem? Can you shed some light on the situation?

Thanks, and I hope this helps!
