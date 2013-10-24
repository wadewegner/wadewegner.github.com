---
author: admin
comments: true
date: 2007-06-13 21:24:52+00:00
layout: post
slug: script-the-creation-of-local-user-accounts-for-commerce-server-2007
title: Script the creation of local user accounts for Commerce Server 2007
wordpress_id: 77
categories:
- Commerce Server
- Scripts
- Tools
- Windows Server 2003
---

I'll be honest ... I'm lazy. I hate doing repetitive things over, and over, and over again. So, while I was going through and installing Commerce Server 2007 on a new virtual machine, I decided to script out the creation of the local user accounts. Before we get to the script, a little background ...

It is recommended that you create multiple accounts to handle the various roles within Commerce Server (such as the four web services, staging, etc). In a production environment, these should be created as Domain accounts; however, in development (or the virtual world) you may not have access to, or wish to use, a domain. Consequently, you can create these users as local accounts as well.

Below is a script that will go ahead and create these local users for you (if I have time I'll create a similar script for domain accounts). Copy the text (or download the link) and save it to a .vbs file. You should be able to simply double-click the file, and then open up Local Users and Groups under Computer Management to double-check.


		' =====================================================  
		' Author: Wade Wegner  
		' Create date: 06/13/2007  
		' Description: Automate the creation of CS 2007 users  
		' File Name: CreateCS2007LocalUsers.vbs  
		' =====================================================  
		  
		' Set the local computer name  
		strComputer = "."  
		  
		' Run the Load method  
		Load  
		  
		' Encapsulates the processing of this script  
		Sub Load()  
		  
		' Create the CS 2007 users  
		CreateUser "CatalogWebSvc","Pa$$w0rd","Account for running the Catalog Web service"  
		CreateUser "CSDMSvc","Pa$$w0rd","Account for running the Commerce Server Direct mailer service"  
		CreateUser "CSHealthMonitorSvc","Pa$$w0rd","Account for running the Commerce Server health Monitoring service"  
		CreateUser "CSLOB","Pa$$w0rd","Account for running the Commerce Server adapters"  
		CreateUser "CSStageSvc","Pa$$w0rd","Account for running the Commerce Server Staging service"  
		CreateUser "MarketingWebSvc","Pa$$w0rd","Account for running the Marketing Web service"  
		CreateUser "OrdersWebSvc","Pa$$w0rd","Account for running the Orders Web service"  
		CreateUser "ProfilesWebSvc","Pa$$w0rd","Account for running the Profiles Web service"  
		CreateUser "RunTimeUser","Pa$$w0rd","IIS account for accessing a Commerce Server site or application"  
		  
		MsgBox "Complete!"  
		  
		End Sub  
		  
		' Create the local user  
		Sub CreateUser(userName, password, description)  
		  
		' Check to see if the user exists; if so, then skip  
		If NOT CheckIfUserExists(userName) Then  
		Set objComputer = GetObject("WinNT://" & strComputer & "")  
		&
		nbsp;Set objUser = objComputer.Create("user", userName)  
		  
		objUser.SetPassword password  
		objUser.FullName = userName  
		objUser.Description = description  
		objUser.Put "UserFlags", 65600 ' Sets Password Never Expires to TRUE  
		' and sets User Can't Change Password to TRUE  
		objUser.SetInfo  
		Else  
		MsgBox userName & " already exists!"  
		End If  
		  
		End Sub  
		  
		' Check to see if user exists  
		Function CheckIfUserExists(userName)  
		  
		Set objComputer = GetObject("WinNT://" & strComputer & "")  
		objComputer.Filter = Array("user")  
		intFound = 0  
		  
		For Each User In objComputer  
		If lcase(User.Name) = lcase(userName) Then  
		intFound = 1   
		End If   
		Next  
		  
		If intFound = 1 Then  
		CheckIfUserExists = True  
		Else  
		CheckIfUserExists = False  
		End If  
		  
		End Function

And there you have it!

[CreateCS2007LocalUsers.vbs (2.46 KB)](http://images.wadewegner.com/wordpress/content/binary/CreateCS2007LocalUsers.vbs)

I hope someone else finds this useful!