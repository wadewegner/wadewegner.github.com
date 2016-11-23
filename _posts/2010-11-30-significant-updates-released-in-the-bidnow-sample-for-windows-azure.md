---
author: admin
comments: true
date: 2010-11-30 17:16:20+00:00
layout: post
slug: significant-updates-released-in-the-bidnow-sample-for-windows-azure
title: Significant Updates Released in the BidNow Sample for Windows Azure
wordpress_id: 823
categories:
- Access Control
- Caching
- OData
- Windows Azure
- Windows Azure AppFabric
- Windows Azure Platform
- Windows Identity Foundation
- Windows Phone 7
---

Click here if you want to quickly grab the [latest version of the BidNow sample](http://code.msdn.microsoft.com/BidNowSample/Release/ProjectReleases.aspx?ReleaseId=4160). Click here if you want to see a [live demo of the application](http://bnow-sample.cloudapp.net/).

 

![BidNowSample](https://wadewegner.blob.core.windows.net/wordpress/2010/11/BidNowSample.png) 

There have been a host of announcements and releases over the last month for the Windows Azure Platform. It started at the [Professional Developers Conference (PDC)](http://microsoftpdc.com/) in Redmond, where we announced new services such as [Windows Azure AppFabric Caching](http://www.wadewegner.com/2010/10/new-services-and-enhancements-with-the-windows-azure-appfabric/) and [SQL Azure Reporting](http://www.microsoft.com/en-us/sqlazure/reporting.aspx), as well as new features for Windows Azure, such as VM Role, Admin mode, RDP, Full IIS, and a new portal experience. Yesterday, the Windows Azure team released the [Windows Azure SDK 1.3 and Tools for Visual Studio](http://www.microsoft.com/downloads/en/details.aspx?FamilyID=7a1089b6-4050-4307-86c4-9dadaa5ed018&displaylang=en) and updates to the [Windows Azure portal](http://windows.azure.com/) (for all the details, see the [team post](http://blogs.msdn.com/b/windowsazure/archive/2010/11/29/just-released-windows-azure-sdk-1-3-and-the-new-windows-azure-management-portal.aspx)). These are significant releases that provide a lot of enhancements for Windows Azure.

 

One of the difficulties that come with this many updates in a short period of time is keeping up with the changes and updates. It can be challenging to understand how these features and capabilities come together to help you build better web applications.

 

This is why my team has released sample applications like [BidNow](http://code.msdn.microsoft.com/BidNowSample), [FabrikamShipping SaaS](http://code.msdn.microsoft.com/fshipsaassource), and [myTODO](http://code.msdn.microsoft.com/mytodo).

 

**_What’s in the BidNow Sample?_**

 

Just today I posted the [latest version of BidNow on the MSDN Code Gallery](http://code.msdn.microsoft.com/BidNowSample/Release/ProjectReleases.aspx?ReleaseId=4160). BidNow has been significantly updated to leverage many pieces of the Windows Azure Platform, including many of the new features and capabilities announced at PDC and that are a part of the Windows Azure SDK 1.3. This list includes:

 

  
  * Windows Azure **(updated)**              
    * Updated for the Windows Azure SDK 1.3 
       
    * Separated the Web and Services tier into two web roles 
       
    * Leverages Startup Tasks to register certificates in the web roles 
       
    * Updated the worker role for asynchronous processing 
       
   
  * SQL Azure** (new)**              
    * Moved data out of Windows Azure storage and into SQL Azure (e.g. categories, auctions, buds, and users) 
       
    * Updated the DAL to leverage Entity Framework 4.0 with appropriate data entities and sources 
       
    * Included a number of scripts to refresh and update the underlying data 
       
   
  * Windows Azure storage** (update)**              
    * Blob storage only used for auction images and thumbnails 
       
    * Queues allow for asynchronous processing of auction data 
       
   
  * Windows Azure AppFabric Caching **(new)**              
    * Leveraging the Caching service to cache reference and activity data stored in SQL Azure 
       
    * Using local cache for extremely low latency 
       
   
  * Windows Azure AppFabric Access Control **(new)**              
    * BidNow.Web leverages WS-Federation and Windows Identity Foundation to interact with Access Control 
       
    * Configured BidNow to leverage Live ID, Yahoo!, and Facebook by default 
       
    * Claims from ACs are processed by the _ClaimsAuthenticationManager_ such that they are enriched by additional profile data stored in SQL Azure 
       
   
  * OData** (new)**              
    * A set of OData services (i.e. WCF Data Services) provide an independent services layer to expose data to difference clients 
       
    * The OData services are secured using Access Control 
       
   
  * Windows Phone 7 **(new)**              
    * A Windows Phone 7 client exists that consumes the OData services 
       
    * The Windows Phone 7 client leverages Access Control to access the OData services 
       
 

Yes, there are a lot of pieces to this sample, but I think you’ll find that it mimics many real world applications. We use Windows Azure web and worker roles to host our application, Windows Azure storage for blobs and queues, SQL Azure for our relational data, AppFabric Caching for reference and activity data, Access Control for authentication and authorization, OData for read-only services, and a Windows Phone 7 client that consumes the OData services. That’s right - in addition to the Windows Azure Platform pieces, we included a set of OData services that are leveraged by a Windows Phone 7 client authenticated through Access Control.

 

**_High Level Architecture_**

 

Here’s a high level look at the application architecture:

 

![image](https://wadewegner.blob.core.windows.net/wordpress/2010/11/image7.png)

 

**Compute**: BidNow consists of three distinct Windows Azure roles – a web role for the website, a web role for the services, and a worker role for long running operations. The BidNow.Web web role hosts Web Forms that interact with the underlying services through a set of WCF Service clients. These services are hosted in the BidNow.Services.Web web role. The worker role, BidNow.Worker, is in charge of handling the long running operations that could hurt the websites performance (e.g. image processing).

 

**Storage**: BidNow uses three different types of storage. There’s a relational database, hosted in SQL Azure, that contains all the auction information (e.g. categories, auctions, and bids) and basic user information, such as Name and Email address (note that user credentials are not stored in BidNow, as we use Access Control to abstract authentication). We use Windows Azure Blob storage to store all the auction images (e.g. the thumbnail and full image of the products), and Windows Azure Queues to dispatch asynchronous operations. Finally, BidNow uses Windows Azure AppFabric Caching to store reference and activity data used by the web tier.

 

**Authentication & Authorization**: In BidNow, users are allowed to navigate through the site without any access restrictions. However, to bid on or publish an item, authentication is required. BidNow leverages the identity abstraction and federation aspects of Windows Azure AppFabric Access Control to validate the users identity against different identity providers – by default, BidNow leverages Windows Live Id, Facebook, and Yahoo!. BidNow uses Windows Identity Foundation to process identity tokens received from Access Control, and then leverages claims from within those tokens to authenticate and authorize the user.

 

**Windows Phone 7**: In today’s world it’s a certainty that your website will have mobile users. Often, to enrich the experience, client applications are written for these devices. The BidNow sample also includes a Windows Phone 7 client that interacts with a set of OData services that run within the web tier. This sample demonstrates how to build a Windows Phone 7 client that leverages Windows Azure, OData, and Access Control to extend your application.

 

Of course, there’s a lot more that you’ll discover one you dig in. Over the next few weeks, I’ll write and record a series of blog posts and web casts that explore BidNow in greater depth.

 

**_Getting Started_**

 

To get started using BidNow, be sure you have the following prerequisites installed:

 

  
  * Windows 7, Vista SP1 or Windows Server 2008 
   
  * Windows PowerShell 
   
  * [Windows Azure Software Development Kit 1.3](http://go.microsoft.com/fwlink/?LinkID=128752)
   
  * [Windows Azure AppFabric SDK 2.0](http://go.microsoft.com/fwlink/?LinkID=184288)
   
  * Internet Information Services 7.0 (or greater) 
   
  * [Microsoft .NET Framework 4](http://www.microsoft.com/downloads/en/details.aspx?FamilyID=9CFB2D51-5FF4-4491-B0E5-B386F32C0992)
   
  * [Microsoft SQL Express 200](http://www.microsoft.com/express/Database/InstallOptions.aspx)8 
   
  * [Microsoft Visual Web Developer 2010 Express](http://go.microsoft.com/?linkid=7653519), or [Microsoft Visual Studio 2010](http://www.microsoft.com/downloads/en/details.aspx?FamilyID=26bae65f-b0df-4081-ae6e-1d828993d4d0)
   
  * [Windows Identity Foundation Runtime](http://www.microsoft.com/downloads/en/details.aspx?FamilyID=EB9C345F-E830-40B8-A5FE-AE7A864C4D76)
   
  * [Windows Identity Foundation SDK 4.0](http://www.microsoft.com/downloads/details.aspx?familyid=C148B2DF-C7AF-46BB-9162-2C9422208504)
 

As you’ve come to expect, BidNow includes a Configuration Wizard that includes a Dependency Checker (for all the items listed immediately above) and a number of Setup Script you can walk through to configure BidNow. At the conclusion of the wizard you can literally hit F5 and go.

 

![image](https://wadewegner.blob.core.windows.net/wordpress/2010/11/image8.png)

 

Of course, don’t forget to [download the BidNow Sample](http://code.msdn.microsoft.com/BidNowSample/Release/ProjectReleases.aspx?ReleaseId=4160). For a detailed walkthrough on how to get started with BidNow, please see the [Getting Started with BidNow wiki page](http://code.msdn.microsoft.com/BidNowSample/Wiki/View.aspx?title=Getting%20Started%20with%20BidNow).
