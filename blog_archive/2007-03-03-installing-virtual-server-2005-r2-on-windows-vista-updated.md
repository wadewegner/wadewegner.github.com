---
author: admin
categories:
- Virtual Server 2005
- Windows Vista
comments: true
date: "2007-03-03T00:14:56Z"
slug: installing-virtual-server-2005-r2-on-windows-vista-updated
title: Installing Virtual Server 2005 R2 on Windows Vista [Updated]
wordpress_id: 113
---

I came across a great blog entry today that provided the following script. This script will setup IIS with the proper configuration to allow Virtual Server 2005 R2 to function. Just run this in the command line (or create a batch file) before installing Virtual Server.

	start /w pkgmgr /l:log.etw /iu:IIS-WebServerRole;IIS-WebServer;IIS-CommonHttpFeatures;IIS-StaticContent;IIS-DefaultDocument;IIS-DirectoryBrowsing;IIS-HttpErrors;IIS-HttpRedirect;IIS-ApplicationDevelopment;IIS-ASPNET;IIS-NetFxExtensibility;IIS-ASP;IIS-CGI;IIS-ISAPIExtensions;IIS-ISAPIFilter;IIS-ServerSideIncludes;IIS-HealthAndDiagnostics;IIS-HttpLogging;IIS-LoggingLibraries;IIS-RequestMonitor;IIS-HttpTracing;IIS-CustomLogging;IIS-ODBCLogging;IIS-Security;IIS-BasicAuthentication;IIS-WindowsAuthentication;IIS-DigestAuthentication;IIS-ClientCertificateMappingAuthentication;IIS-IISCertificateMappingAuthentication;IIS-URLAuthorization;IIS-RequestFiltering;IIS-IPSecurity;IIS-Performance;IIS-HttpCompressionStatic;IIS-HttpCompressionDynamic;IIS-WebServerManagementTools;IIS-ManagementConsole;IIS-ManagementScriptingTools;IIS-ManagementService;IIS-IIS6ManagementCompatibility;IIS-Metabase;IIS-WMICompatibility;IIS-LegacyScripts;IIS-LegacySnapIn;IIS-FTPPublishingService;IIS-FTPServer;IIS-FTPManagement;WAS-WindowsActivationService;WAS-ProcessModel;WAS-NetFxEnvironment;WAS-ConfigurationAPI

Note: this will pretty much install everything under IIS. I recommend that you either go through and remove some of the above settings, or go in afterwards and do so.

Additionally, make sure that when you run the installer, select "Run as Administrator".

Thanks to [Grant Holliday](http://www.holliday.com.au/blog/installing-virtual-server-2005-on-vista-5365.html) for the tip!

[Updated 06/21/07]

A good friend and former colleague of mine, [Rich Finn](http://rfinn.spaces.live.com/), had a little trouble getting this to work based on my steps above (I'll have to review them again). After he finally got the configuration correct, he sent me the following screen shot. I hope this helps someone!

![](https://wadewegner.blob.core.windows.net/wordpress/content/binary/iis.gif)