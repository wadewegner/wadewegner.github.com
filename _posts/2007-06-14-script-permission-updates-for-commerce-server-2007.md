---
author: admin
comments: true
date: 2007-06-14 04:21:33+00:00
layout: post
slug: script-permission-updates-for-commerce-server-2007
title: Script permission updates for Commerce Server 2007
wordpress_id: 75
categories:
- Commerce Server
- Scripts
---

Okay, so here's another useful script.

Commerce Server 2007 requires you to give service accounts write permissions to various files and folders. This script assigns the write permissions to the Catalog Web service, the Temporary ASP.NET folder, and the Windows Temporary folder. These permission allow you to run the Business User applications through the Business Management Web services.

In order to run this script, you must have the XCACLS.vbs file available. Learn more about this VBScript [here](http://support.microsoft.com/kb/825751), and download it [here](http://download.microsoft.com/download/f/7/8/f786aaf3-a37b-45ab-b0a2-8c8c18bbf483/xcacls_installer.exe).

Here's the script:

	' ==========================================================  
	' Author: Wade Wegner  
	' Create date: 06/13/2007  
	' Description: Automate the task of assigning permissions  
	' File Name: AssignCSPermissions.vbs  
	' ==========================================================  
	  
	' Declare the users  
	Dim users(4)  
	users(0) = "RunTimeUser"  
	users(1) = "CatalogWebSvc"  
	users(2) = "MarketingWebSvc"  
	users(3) = "OrdersWebSvc"  
	users(4) = "ProfilesWebSvc"  
	  
	' Run the Load method  
	Load  
	  
	Sub Load()  
		  
		' Write permissions to the catalog auth role  
		strObject = "C:\Inetpub\wwwroot\CatalogWebService\CatalogAuthorizationStore.xml"  
		UpdatePermissions strObject, users(1)  
		  
		' Write permissions to temporary ASP.NET folder  
		strObject = "C:\WINDOWS\Microsoft.NET\Frameworkv2.0.50727\Temporary ASP.NET Files"  
		For Each user IN users  
			UpdatePermissions strObject, user  
		Next   
		  
		' Write permissions to the Windows temporary folder  
		strObject = "C:WINDOWSTemp"  
		For Each user IN users  
			UpdatePermissions strObject, user  
		Next   
	  
	End Sub  
	  
	' Update the permissions of the folder/file  
	Sub UpdatePermissions(strLocation, strUser)  
		  
		Set objShell = CreateObject("Wscript.Shell")   
		' Make sure to have the xcacls.vbs file available. Download from:  
		' http://download.microsoft.com/download/f/7/8/f786aaf3-a37b-45ab-b0a2-8c8c18bbf483/xcacls_installer.exe  
		objShell.Run "xcacls.vbs """ + strLocation + """ /G " + strUser + ":XW /E", 2, True
		  
	End Sub

Note that, in order to run this script, you may have to run "cscript.exe /h:cscript" from the command prompt, which changes the default scripting engine from Wscript to Cscript.

After running this script, you will have updated the permissions and you didn't have to do it manually!

[AssignCSPermissions.vbs.txt (1.5 KB)](http://images.wadewegner.com/wordpress/content/binary/AssignCSPermissions.vbs.txt)

I hope this helps!
