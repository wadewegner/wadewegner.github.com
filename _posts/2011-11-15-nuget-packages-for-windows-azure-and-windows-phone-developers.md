---
author: admin
comments: true
date: 2011-11-15 22:21:30+00:00
layout: post
slug: nuget-packages-for-windows-azure-and-windows-phone-developers
title: NuGet Packages for Windows Azure and Windows Phone Developers
wordpress_id: 1619
categories:
- ACS
- NuGet
- Push Notification
- Windows Azure
- Windows Phone
---

If you’ve been paying attention to the [Windows Azure Toolkit for Windows Phone](http://watwp.codeplex.com/) (or my [twitter feed](http://twitter.com/WadeWegner)) the last couple weeks, you’ve probably noticed something about NuGet packages. We’ve been building a lot of Windows Phone and Windows Azure NuGet packages that, when composed together, give you the ability to quickly build some cool applications. To highlight this, here’s a short video that shows how you can enable push notification support in a brand new Windows Phone 7.1 project—and send notifications from a new ASP.NET MVC 3 Web Application running in Windows Azure—in less than two minutes!

 

 

Here’s a look at how to use the storage NuGet packages to quickly (and securely) upload a picture from the Windows Phone to Windows Azure blob storage.

 

 

All of this is made possible by delivering functional, discrete, and composable NuGet packages. I’ve gotten a lot of feedback (positive and _constructive_, but fortunately mostly positive) about the [Windows Azure Toolkit for Windows Phone](http://watwp.codeplex.com/), and invariably people have said that it’s too hard to decompose the sample applications – often times people just want Push Notification support or user management, and it’s too hard to get rid of the rest.

 

I think the NuGet packages make it very easy to do two things:

 

  
  1. Build brand new applications that quickly can get some advanced capabilities 
   
  2. Quickly update existing applications to get some desirable enhancements 
 

This is largely possible because we can easily manage and deliver dependencies through NuGet. Let us handle the hard stuff – you focus on building out cool applications.

 

In this post I’d like to provide a list and description of the NuGet packages we’ve delivered so far – I imagine I’ll update this post many times to keep it accurate. I don’t plan to show exactly how to use these packages—I’ll save that for many future posts—but instead I want to use this post as a reference and guidepost moving forward.

 

So, without further ado, I’d like to introduce you to the two kinds of NuGet packages we have today: Client Side NuGet Packages, and Server Side NuGet Packages.

 

**_Client Side NuGet Packages_**

 

The first set of NuGet packages to be aware of are the NuGet packages for Windows Phone, designed to target Windows Phone OS 7.1 project types.

 

![Windows Phone OS 7.1](https://wadewegner.blob.core.windows.net/wordpress/2011/11/Windows-Phone-OS-7.1_thumb.jpg) 

You can use these NuGets in a number of interesting ways. For example, you can quickly incorporate the [Access Control Service](http://www.microsoft.com/windowsazure/features/accesscontrol/) into your phone applications using the following NuGet packages:

 

  
  * **[Phone.Identity.AccessControl](http://www.nuget.org/List/Packages/Phone.Identity.AccessControl)**: Access Control Service Log In control for Windows Phone. 
   
  * **[Phone.Identity.AccessControl.BasePage](http://www.nuget.org/List/Packages/Phone.Identity.AccessControl.BasePage)**: Base log in page for Windows Phone that uses the Access Control Service Log In control. 
 

If you’re not using ACS but instead want simple username/password, you can quickly incorporate membership into your phone applications using the following NuGet packages:

 

  
  * **[Phone.Identity.Membership](http://www.nuget.org/List/Packages/Phone.Identity.Membership)**: Membership Sign In control for Windows Phone. 
   
  * **[Phone.Identity.Membership.BasePage](http://www.nuget.org/List/Packages/Phone.Identity.Membership.BasePage)**: Base log in and register pages for Windows Phone that use the Membership Log In control 
 

To get full support for Push Notifications (including Mango updates like deep linking) you can easily incorporate Push Notifications using the following NuGet packages:

 

  
  * **[Phone.Notifications](http://www.nuget.org/List/Packages/Phone.Notifications)**: Class library for Windows Phone to communicate with the Push Notification Registration Cloud Service. 
   
  * **[Phone.Notifications.BasePage](http://www.nuget.org/List/Packages/Phone.Notifications.BasePage)**: Base notifications page for Windows Phone to register / unregister with the Push Notification Registration Cloud Service for receiving push notification messages. 
 

In scenarios where you’d want to secure your notification services using the Access Control Service, you can use the following packages:

 

  
  * **[Phone.Notifications.AccessControl](http://www.nuget.org/List/Packages/Phone.Notifications.AccessControl)**: This package enables communication with the Push Notification Registration Cloud Service using Windows Azure Access Control Service (ACS) for authentication, by adding a set of base pages to the phone application.         
       
The dependencies and relationships between these NuGets are as follows:   
![Phone.Notifications.AccessControl](https://wadewegner.blob.core.windows.net/wordpress/2011/11/Phone.Notifications.AccessControl.png)
 

In scenarios where you’d want to secure your notification services using traditional membership, you can use the following packages:

 

  
  * **[Phone.Notifications.Membership](http://www.nuget.org/List/Packages/Phone.Notifications.Membership)**: This package enables communication with the Push Notification Registration Cloud Service using Membership for authentication, by adding a set of base pages to the phone application.         
       
The dependencies and relationships between these NuGets are as follows:   
![Phone.Notifications.Membership](https://wadewegner.blob.core.windows.net/wordpress/2011/11/Phone.Notifications.Membership.png)
 

**__**

 

**__**

 

**[Updated 1/6/12]**

 

Recently we added a set of client side NuGet packages that can communicate with the Windows Azure storage service – either directly (using the storage account information) or through proxy services running in Windows Azure. Here are the packages for the client:

 

  
  * **[Phone.Storage](http://www.nuget.org/packages/Phone.Storage)**: Class library for Windows Phone to communicate with Windows Azure storage services directly (using the storage account information) or through Proxy Cloud Services (using custom authentication mechanisms). 
   
  * **[Phone.Storage.Sample](http://www.nuget.org/packages/Phone.Storage.Sample)**: Sample application for Windows Phone that shows how to use the Windows Azure Storage Client Library for Windows Phone. 
   
  * **[Phone.Storage.AccessControl](http://www.nuget.org/packages/Phone.Storage.AccessControl)**: This package enables communication with the Windows Azure Storage Proxy Cloud Services using Windows Azure Access Control Service (ACS) for authentication.         
![image](http://www.wadewegner.com/content/2012/01/image6.png)
   
  * **[Phone.Storage.Membership](http://www.nuget.org/packages/Phone.Storage.Membership)**: This package enables communication with the Windows Azure Storage Proxy Cloud Services using Membership for authentication.         
![image](http://www.wadewegner.com/content/2012/01/image7.png)
 

**_Server Side NuGet Packages_**

 

Over the years our team has built a lot of libraries for Windows Azure that we regularly use for samples, demos, hands-on labs, and so forth. We’ve continued to refine these libraries and have started to expose some of them as discrete NuGet packages. Here are some of them:

 

  
  * **[WindowsAzure.Common](http://www.nuget.org/List/Packages/WindowsAzure.Common)**: Class library that provides common helpers tools for Windows Azure. 
   
  * **[Storage.Providers](http://www.nuget.org/List/Packages/Storage.Providers)**: ASP.NET Providers (Membership, Roles, Profile and Session State Store) for Windows Azure Tables. 
   
  * **[MpnsRecipe](http://www.nuget.org/List/Packages/MpnsRecipe)**: Class library to communicate with the Microsoft Push Notification Service (MPNS). 
 

If you plan to manage users through ASP.NET membership, we have a NuGet package that will handle everything in your Windows Azure project:

 

  
  * **[WindowsAzure.Identity.Membership](http://www.nuget.org/List/Packages/WindowsAzure.Identity.Membership)**: Class library that includes the Membership Authentication Cloud Service. 
 

We have a set of WebAPI services that work with the Phone.Notifications NuGet packages for handling the Channel URIs and push notification registration services:

 

  
  * **[WindowsAzure.Notifications](http://www.nuget.org/List/Packages/WindowsAzure.Notifications)**: This package contains a class library with the Push Notification Registration Cloud Service, and a WebActivator enabled class with the default configuration. 
   
  * **[WindowsAzure.Notifications.Sql](http://www.nuget.org/List/Packages/WindowsAzure.Notifications.Sql)**: Class library that provides storage in a SQL Azure or SQL Server database for the Push Notification Registration Cloud Service. 
   
  * **[WindowsAzure.Notifications.Client.Sql](http://www.nuget.org/packages/WindowsAzure.Notifications.Client.Sql)**: This package contains a class library with a Table Context to access the Push Notification Registration Cloud Service's SQL Server tables where the registered enpoints are stored. This package can be used from a worker role to query registered endpoints and send notifications. 
   
  * **[WindowsAzure.Notifications.Client.AzureTables](http://www.nuget.org/packages/WindowsAzure.Notifications.Client.AzureTables)**: This package contains a class library with a Table Context to access the Push Notification Registration Cloud Service's Azure Tables where the registered enpoints are stored. This package can be used from a worker role to query registered endpoints and send notifications. 
 

If you plan to use the Phone.Notifications.AccessControl NuGet package and secure the communications channel with ACS, then you can use this NuGet package:

 

  
  * **[WindowsAzure.Notifications.AccessControl](http://www.nuget.org/List/Packages/WindowsAzure.Notifications.AccessControl)**: This package enables authentication using Windows Azure Access Control Service (ACS) for the Push Notification Registration Cloud Service. You just need to configure a Relying Party Application with Simple Web Token (SWT) in your ACS namespace, and configure its settings accordingly in the Web.config.         
       
The dependencies and relationships between these NuGets are as follows:   
![WindowsAzure.Notifications.AccessControl](http://www.wadewegner.com/content/2012/01/image2.png)
 

If you plan to use the Phone.Notifications.Membership NuGet package and secure the communications channel with membership, then you can use this NuGet package:

 

  
  * **[CloudServices.Notifications.Membership](http://www.nuget.org/List/Packages/CloudServices.Notifications.Membership)**: This package enables authentication using the Membership provider for the Push Notification.         
       
The dependencies and relationships between these NuGets are as follows:   
![WindowsAzure.Notifications.Membership](http://www.wadewegner.com/content/2012/01/image3.png)
 

When working with Push Notifications, you need some kind of client to generate and send notifications. We’ve built some simple scaffolding that you can use during development (or production?) to generate and send notifications:

 

  
  * **[WindowsPhone.Notifications.ManagementUI.Sample](http://www.nuget.org/List/Packages/WindowsPhone.Notifications.ManagementUI.Sample)**: A sample MVC Area containing the Management UI for sending Push Notifications to Windows Phone Devices. 
 

**[Updated 1/6/12]**

 

Recently we added a set of client side NuGet packages that can communicate with the Windows Azure storage service – either directly (using the storage account information) or through proxy services running in Windows Azure. Here are the packages for the services:

 

  
  * **[WindowsAzure.Storage](http://www.nuget.org/packages/WindowsAzure.Storage)**: This client library enables working with the Windows Azure storage services which include the blob service for storing binary and text data, the table service for storing structured non-relational data, and the queue service for storing messages that may be accessed by a client. 
   
  * **[WindowsAzure.Storage.Proxy](http://www.nuget.org/packages/WindowsAzure.Storage.Proxy)**: This package contains a class library with the Windows Azure Storage Proxy Cloud Services, and a _WebActivator_ enabled class with the default configuration. 
   
  * **[WindowsAzure.Storage.AccessControl](http://www.nuget.org/packages/WindowsAzure.Storage.AccessControl)**:This package enables authentication using Windows Azure Access Control Service (ACS) for the Windows Azure Storage Proxy Cloud Services. You just need to configure a Relying Party Application with Simple Web Token (SWT) in your ACS namespace, and configure its settings accordingly in the Web.config. 
   
  * **[WindowsAzure.Storage.Proxy.AccessControl](http://www.nuget.org/packages/WindowsAzure.Storage.Proxy.AccessControl)**: This package enables authentication using Windows Azure Access Control Service (ACS) for the Windows Azure Storage Proxy Cloud Services. You just need to configure a Relying Party Application with Simple Web Token (SWT) in your ACS namespace, and configure its settings accordingly in the Web.config.         
![WindowsAzure.Storage.AccessControl](http://www.wadewegner.com/content/2012/01/image4.png)
   
  * **[WindowsAzure.Storage.Membership](http://www.nuget.org/packages/WindowsAzure.Storage.Membership)**: This package enables authentication using the Membership provider for the Windows Azure Storage Proxy Cloud Services. You just need to make sure to have a valid Membership provider configured in your Web.config. 
   
  * **[WindowsAzure.Storage.Proxy.Membership](http://www.nuget.org/packages/WindowsAzure.Storage.Proxy.Membership)**: This package enables authentication using the Membership provider for the Windows Azure Storage Proxy Cloud Services. You just need to make sure to have a valid Membership provider configured in your Web.config.         
![WindowsAzure.Storage.Membership](http://www.wadewegner.com/content/2012/01/image5.png)
 

That’s it!

 

It’s a lot of resources, I know. The intent of this post isn’t to necessarily provide you with the guidance on how to use all these NuGets, but rather to explain what we have available. I plan to write a lot of blog posts that highlight real scenarios and use cases for these NuGet packages, so I’ll refer back to this post quite often. In the meantime, I hope it gives you a feel for how we’re thinking about engineering and delivering resources for Windows Phone and Windows Azure moving forward.

 

I hope this helps!
