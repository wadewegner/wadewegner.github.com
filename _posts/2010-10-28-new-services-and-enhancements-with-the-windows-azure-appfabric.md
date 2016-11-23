---
author: admin
comments: true
date: 2010-10-28 17:46:44+00:00
layout: post
slug: new-services-and-enhancements-with-the-windows-azure-appfabric
title: New Services and Enhancements with the Windows Azure AppFabric
wordpress_id: 793
categories:
- Caching
- Composite Application
- Composition Model
- Integration
- Service Bus
- Windows Azure AppFabric
---

 

![Windows Azure AppFabric](https://wadewegner.blob.core.windows.net/wordpress/2010/10/image1.png)

Today’s an exciting day! During the keynote this morning at PDC10, Bob Muglia announced a wave of new building block services and capabilities for the Windows Azure AppFabric. The purpose of the Windows Azure AppFabric is to provide a comprehensive cloud platform for developing, deploying and managing applications, extending the way you build Windows Azure applications today. At PDC09, we announced both Windows Azure AppFabric and Windows Server AppFabric, highlighting a commitment to deliver a set of complimentary services both in the cloud and on-premises. While this has long been an aspiration, we haven’t yet delivered on it – until today!

 

Let me quickly enumerate the some of the new building block services and capabilities: 

 

  
  * **Caching (CTP at PDC)** – an in-memory, distributed application cache to accelerate the performance of Windows Azure and SQL Azure-based applications. This Caching service is the complement to Windows Server AppFabric Caching, and provides a symmetric development experience across the cloud and on-premises. 
   
  * **Service Bus Enhancements (CTP at PDC)** – enhanced to add durable messaging support, load balancing for services, and an administration protocol. Note: this is not a replacement of the live, commercial Service Bus offering, but instead a set of enhancements provided in the AppFabric LABS portal. 
   
  * **Integration (CTP in CY11)** – common BizTalk Server integration capabilities (e.g. pipeline, transforms, adapters) as a service on Windows Azure. 
   
  * **Composite Application (CTP in CY11)** – a multi-tenant, managed service which consumes the .NET based Composition Model definition and automates the deployment and management of the end-to-end application. 
 

As part of the end-to-end environment for composite applications, there are a number of supporting elements:

 

  
  * **AppFabric Composition Model and Tools (CTP in CY11) **– a set of .NET Framework extensions for composing applications on the Windows Azure platform. The Composition Model provides a way to describe the relationship between the services and modules used by your application. 
   
  * **AppFabric Container (CTP in CY11) **– a multitenant, high density host optimized for services and mid-tier components. During the keynote, James Conard showcased a standard, .NET WF4 running in the container. 
 

Want to get up to speed quickly? Take a look at these resources for Windows Azure AppFabric:

 

  
  * Official website: [www.microsoft.com/appfabric/azure](http://www.microsoft.com/appfabric/azure)
   
  * Team Blog: [http://blogs.msdn.com/b/windowsazureappfabric/](http://blogs.msdn.com/b/windowsazureappfabric/)
   
  * MSDN website: [http://msdn.microsoft.com/en-us/windowsazure/netservices.aspx](http://msdn.microsoft.com/en-us/windowsazure/netservices.aspx)
   
  * LABS Portal: [http://portal.appfabriclabs.com/](http://portal.appfabriclabs.com/)
 

Watch these great sessions from PDC10 which cover various pieces of the Windows Azure AppFabric:

 

  
  * [Composing Applications with AppFabric Services](http://player.microsoftpdc.com/Session/c3c5f2d9-0481-4be1-9742-4dfa4de184d0)
   
  * [Connecting Cloud & On-Premises Apps with the Windows Azure Platform](http://player.microsoftpdc.com/Session/fe7e140b-de62-4768-9306-23d0bdcabc5c)
   
  * [Identity & Access Control in the Cloud](http://player.microsoftpdc.com/Session/0099d03d-bbc4-4612-87e1-f7d4da8b8a78)
   
  * [Building High Performance Web Applications in the Windows Azure Platform](http://player.microsoftpdc.com/Session/1b08b109-c959-4470-961b-ebe8840eeb84)
   
  * [Introduction to Windows Azure AppFabric Caching](http://player.microsoftpdc.com/Session/1f607983-c6eb-4d9f-b644-55247e8adda6)
   
  * [Microsoft BizTalk Server 2010 and Roadmap](http://player.microsoftpdc.com/Session/3ca2a630-f859-4589-8a6b-33009b5e963b)
   
  * [Service Bus Enhancements](http://player.microsoftpdc.com/Session/1f7d009e-29cb-4a15-a1bf-91ffd115c54d)
 

Also, be sure to check out this interview with Karandeep Anand, Principal Group Program Manager with Application Platform Services, as he talks about the new Windows Azure AppFabric Caching service.

Early next week my team, the Windows Azure Platform Evangelism team, will release a **new version of the Windows Azure Platform Training Kit **– in this kit we’ll have updated hands-on-labs for the Caching service and the Service Bus enhancements. Be sure and take a look!

 

Of course, I’ll have more to share over the new few days and weeks.
