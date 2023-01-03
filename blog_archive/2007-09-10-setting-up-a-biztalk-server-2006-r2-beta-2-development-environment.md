---
author: admin
categories:
- BizTalk Server
- WCF
comments: true
date: "2007-09-10T22:37:11Z"
slug: setting-up-a-biztalk-server-2006-r2-beta-2-development-environment
title: Setting up a BizTalk Server 2006 R2 (beta 2) development environment
wordpress_id: 33
---

In preparation for the talk I am giving tomorrow on [BizTalk Server R2 and the WCF Adapters](http://www.wadewegner.com/2007/09/10/PresentingToTheDenverBizTalkUserGroup.aspx), I thought I would post some notes on how to setup and configure a BizTalk Server 2006 R2 development environment.
The installation is really quite straightforward.  Start with a relatively clean machine (I suggest you use a virtual machine, as these are all beta bits).  My (virtual) machine had the following installed before I started the R2 installation:



	
  * Windows Server 2003 R2 Standard Edition with Service Pack 2

	
  * IIS 6.0

	
  * Microsoft .NET 1.0, 1.1, 2.0, 3.0

	
  * Microsoft Visual Studio 2005 Team Developer Edition with Service Pack SP1

	
  * SQL Server 2005 with Service Pack SP2


Additionally, you need to install the Windows SDK and .NET Framework 3.0 Runtime Components.  This gives you the documentation, samples, header files, libraries, and tools you need to develop applications that run on Windows.  You can download this from: [http://www.microsoft.com/downloads/details.aspx?FamilyId=C2B1E300-F358-4523-B479-F53D234CDCCF&displaylang=en](http://www.microsoft.com/downloads/details.aspx?FamilyId=C2B1E300-F358-4523-B479-F53D234CDCCF&displaylang=en).  Note: it takes awhile, so be patient.  Also, I removed the documentation and samples, which makes the download and installation a lot faster.
From here you just need to download the R2 bits.  To find out how you can obtain BizTalk Server 2006 R2 (beta 2), take a look at this article: [http://support.microsoft.com/kb/936046](http://support.microsoft.com/kb/936046).  Or, if you want me to save you the work and take you directly to the bits, you can go here: [https://connect.microsoft.com/Downloads/DownloadDetails.aspx?SiteID=65&DownloadID=6014](https://connect.microsoft.com/Downloads/DownloadDetails.aspx?SiteID=65&DownloadID=6014).
The BizTalk 2006 R2 Beta 2 release includes the following components:



	
  * BizTalk core engine

	
  * EDI

	
  * RFID

	
  * LOB Adapters

	
  * BizTalk Accelerators (HL7, SWIFT, RosettaNet)


The components are available by downloading and installing the following files:

	
  * BTS06R2_Beta2.exe

	
  * BTS06R2_Beta2_Accelerators.zip

	
  * BizTalk LOB Adapters Sp1 Beta2.exe

	
  * CustomSOAPHeaderPipeline.zip


Note: if you only want the R2 functionality (e.g. WCF, etc) you only need BTS06R2_Beta2.exe.
After I downloaded all the files, I first extracted the contents of BTS06R2_Beta2.exe to a temporary folder (choose a folder you can remember, like C:TempBTS06R2_Beta2).  Next, I ran Setup.exe from that folder.  Click Next until you get to the Component Installation screen.  You'll see that there are a few differences from the standard BizTalk components.  Here's a peak:

![BizTalk Server 2006 R2 Installation Wizard](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/SettingupaBizTalkServer2006R2development_CDB5/BizTalkR2InstallationWizard_thumb_1.gif)

I decided to leave the default settings.  Feel free to do what you want.  Continue to click Next until you get to the Summary screen.  Review your selections and click Install (you may also want to set your auto-login credentials to save time).
After it installs you have to run the BizTalk Configuration Tool.  The configuration is roughly the same as it is for BizTalk Server 2006:

![BizTalk Server 2006 R2 Configuration](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/SettingupaBizTalkServer2006R2development_CDB5/BizTalkR2Configuration_thumb.gif)

I don't plan on using many of the features at the moment (e.g. BAM, HWS, etc.) so I only installed the following features:

![BizTalk Server 2006 R2 Features](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/SettingupaBizTalkServer2006R2development_CDB5/BizTalkR2Features_thumb.gif)

And that's it!  At this point, you will have BizTalk 2006 R2 (beta 2) installed and functioning.  Open up the BizTalk Server 2006 Administration Console and take a look at the adapters now available to you:

![BizTalk Server 2006 R2 Adapters](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/SettingupaBizTalkServer2006R2development_CDB5/BizTalkAdapters_thumb.gif)

Specifically, you now can use the following adapters:



	
  * WCF-BasicHttp

	
  * WCF-Custom

	
  * WCF-CustomIsolated

	
  * WCF-NetMsmq

	
  * WCF-NetNamedPipe

	
  * WCF-NetTcp

	
  * WCF-WSHttp


I hope this helps you explore the new functionality available to you in BizTalk Server 2006 R2.  Enjoy!
