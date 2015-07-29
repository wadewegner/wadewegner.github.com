---
author: admin
comments: true
date: 2011-10-25 22:22:25+00:00
layout: post
slug: windows-azure-toolkit-for-windows-phone-v1-3-1
title: Windows Azure Toolkit for Windows Phone v1.3.1
wordpress_id: 1551
categories:
- Toolkit
- Windows Azure
- Windows Phone
---

Yesterday I released an update to the [Windows Azure Toolkit for Windows Phone](http://watwp.codeplex.com/). There are a few important updates in this release, so I encourage you to go and [download](http://watwp.codeplex.com/releases/view/75654) the new tools. We have a number of significant updates:

 

  
  * Upgraded Windows Azure projects to **Windows Azure Tools for Microsoft Visual Studio 2010 1.5 – September 2011**
   
  * Upgraded the tools tools to support the **Windows Phone Developer Tools RTW**
   
  * Update SQL Azure only scenarios to use **ASP.NET Universal Providers** (through the System.Web.Providers v1.0.1 NuGet package) 
   
  * Changed Shared Access Signature service interface to support more operations 
   
  * Refactored Blobs API to have a similar interface and usage to that provided by the Windows Azure SDK StorageClient library 
   
  * Added two new samples: **Tweet Your Blobs** and **CRUD SQL Azure**
 

In addition to the core updates listed above, we’ve also enhanced the setup and dependency checking experience. The experience is more tightly integrated with the Web Platform Installer, making it easier for you to use the toolkit.

 

[![image](https://wadewegner.blob.core.windows.net/wordpress/2011/10/image_thumb.png)](https://wadewegner.blob.core.windows.net/wordpress/2011/10/image.png)

 

Of all the updates, I’m most excited about the work we did to bring forward our blobs API to have parity with the primary Windows Azure StorageClient library. Now, regardless of whether you’re using the a web application or phone application, you’ll have a consistent API with which to use Windows Azure storage.
