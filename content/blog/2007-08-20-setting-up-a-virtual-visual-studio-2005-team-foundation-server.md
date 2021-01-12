---
author: admin
categories:
- Team Foundation Server
- Virtual Server 2005
comments: true
date: "2007-08-20T02:32:38Z"
slug: setting-up-a-virtual-visual-studio-2005-team-foundation-server
title: Setting up a virtual Visual Studio 2005 Team Foundation Server
wordpress_id: 41
---

Although it has its share of issues, I am a big fan of Visual Studio 2005 Team Foundation Server (known as TFS for short). Often you'll see people only leverage it for a source code repository, but if that's all you are using it for then you are missing out. In addition to storing source code, TFS also supports:

  * Code analysis

  * Build automation

  * Support for continuous integration

  * Automated testing

  * Reporting

  * Project tracking

I have found TFS to a very useful tool for all my custom development projects, especially Commerce Server 2007. From a developer's perspective, I find that I am much more excited about cod analysis, build automation, continuous integration, and testing; the project tracking and management functions are great, but it's the developer tools that really tickle my fancy. As such, I have included around 15 to 20 pages of content in my book Professional Commerce Server 2007 on how to successfully integrate Commerce Server 2007 and TFS.

To support my writing efforts, I needed to have a TFS environment available during the writing of these chapters. I virtualize nearly all my non-production environments, and it was no different when I setup this server.

To save myself some time, I started with one of my base servers that had the following characteristics (it is extremely handy to have one of these servers archived):

  * Windows Server 2003 Standard R2 with Service Pack 2 (and all updates applied)

  * IIS installed and configured

  * .NET 2.0 installed with latest hotfixes

  * Visual Studio 2005 Team Developer installed w/ Service Pack 1

  * SQL Server 2005 Database Services with Service Pack 2

This gave me a huge head start in setting up my TFS environment. For the rest, you should utilize the [Visual Studio 2005 Team Foundation Installation Guide](http://go.microsoft.com/fwlink/?LinkId=40042). Below are some notes I took on what I did to finish the installation and configuration of TFS.


### Install and Configure TFS


(Wow, for some reason Live Writer did an awful job with this list - the RSS feed is a mess! Sorry!)

  1. Changed the computer name from "BaseServer to "TFServer." (reboot)

  2. Updated SQL Server to use the new computer name. Ran the following script:

	sp_dropserver 'BaseServer'  
	GO   
	sp_addserver 'TFServer', local   
	GO

  3. Deleted all the SQL Server remote logins that I didn't need.

  4. Restarted SQL Server 2005. Don't forget to do this; otherwise, when you run SELECT @@SERVERNAME the old computer name will return.

  5. I think joined "TFServer" to my local domain. (reboot)

  6. You should use domain accounts to manage TFS. I created the following domain accounts:

    * TFSSetup (make this account part of the local Administrators group on TFS)

    * TFSService

    * TFSReports

  7. Confirmed that IIS 6.0 has ASP.NET enabled, and also confirmed that FrontPage Server 2002 Extensions is not installed.

  8. TFS makes use of more than SQL Server 2005 Database Services. I Installed the following additional SQL Server components:

    * Analysis Services

    * Reporting Services

    * Integration Services

  9. Shutdown the Analysis, Reporting, and Integration services.

  10. Installed Service Pack 2 for SQL Server 2005 to upgrade additional components. The Database Services had already been upgraded, so I didn't have to touch it.

  11. Installed Hotfix for Microsoft .NET Framework 2.0 (KB913393) - this is avail on the TFS CD.

  12. Installed Windows SharePoint Services 2.0 with Service Pack 2

    * Go to: [http://go.microsoft.com/fwlink/?linkid=55087](http://go.microsoft.com/fwlink/?linkid=55087) to download the bits.

    * Run the following at the command-line (allows for a silent installation): stsv2.exe /C:"setupsts.exe /remoteSql=yes /provision=no /qn+"

  13. I decided to reboot the machine; all the services I previously stopped are turned back on, and then I can login as <domain>TFSSetup.

  14. Configured Reporting Services:


	![image_thumb[6]](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/ConfiguringavirtualTFServer_7070/image_thumb6_thumb.png)


    * Click Start -> Microsoft SQL Server 2005 -> Configuration Tools -> Reporting Services Configuration.

    * Do the following:


      * Create a Report Server Virtual Directory

      * Create a Report Manager Virtual Directory

      * Apply the Web Service Identity

      * Configure the Database Setup


    * I left the remaining steps alone.




  15. Installed TFS.





    * Selected a Single-Server Installation.

    * Pass the System Health Check .

    * Used <domain>TFSService to run TFS.

    * Used <domain>TFSReports to run Reporting.

    * Enabled Team Foundation Alerts:


      * Configured local server.

      * Setup from e-mail address.

  16. Backed-up the Reporting Services Encryption key:


	![image](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/ConfiguringavirtualTFServer_7070/image2_thumb.png)

    * Ran the Reporting Services Configuration Tool

    * Clicked Encryption Keys, and then Backup.




  17. I then browsed to the following URL: [http://localhost:8080/services/v1.0/Registration.asmx](http://localhost:8080/services/v1.0/Registration.asmx)


    * Clicked GetRegistrationEntries and then clicked Invoke.

    * Ensured that the type was "VSTF" within the XML output.


  18. Installed Team Explorer (on the TFS CD).

  19. Installed Visual Studio Team Foundation Service Quiescence GDR:


    * This is a feature pack for TFS.

    * Download from: [http://go.microsoft.com/fwlink/?LinkId=79225](http://go.microsoft.com/fwlink/?LinkId=79225)

    * Required before you can install SP1.

  20. Installed Visual Studio 2005 Team Foundation Server Service Pack 1.

    * Download from: [http://go.microsoft.com/fwlink/?LinkId=79224](http://go.microsoft.com/fwlink/?LinkId=79224)

  21. Installed Team Foundation Build.

    * Opened the "build" folder on the TFS CD.

    * Ran Setup.exe.

    * Used <domain>TFSSERVICE as the service account.

  22. Ran Visual Studio 2005 Team Foundation Server Service Pack 1 again to update Team Foundation Build.

  23. One final reboot.


Now, to verify that everything was installed and configured properly, I performed confirmed that I was able to create a new Team Project.

### Create a Team Project in TFS

  1. Open up Visual Studio 2005

  2. Click View -> Team Explorer

  3. Click Add Existing Team Project


	![image](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/ConfiguringavirtualTFServer_7070/image4_thumb.png)

  4. Select your Team Foundation Server, and click OK.

	![image](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/ConfiguringavirtualTFServer_7070/image3_thumb.png)

  5. Right-click your Team Foundation Server, and click New Team Project.


	![image](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/ConfiguringavirtualTFServer_7070/image_thumb_6.png)
	
  6. Follow the wizard and create a new Team Project.

All together, this process took around 3 hours. Granted, this would have taken a lot longer if I hadn't already had a virtual machine that was mostly configured.

I hope this helps!