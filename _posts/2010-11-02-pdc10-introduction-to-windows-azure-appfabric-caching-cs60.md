---
author: admin
comments: true
date: 2010-11-02 16:29:57+00:00
layout: post
slug: pdc10-introduction-to-windows-azure-appfabric-caching-cs60
title: 'PDC10: Introduction to Windows Azure AppFabric Caching (CS60)'
wordpress_id: 796
categories:
- Caching
- PDC10
- Windows Azure AppFabric
---

In addition to building the [Composite Application keynote demo](http://player.microsoftpdc.com/Session/6f853fa2-06f6-45e5-ac25-18c31cc4ba32/9588.561) presented by [James Conard](http://www.jamesconard.com/) and Bob Muglia, I presented a session on the new [Windows Azure AppFabric Caching service](http://player.microsoftpdc.com/Session/1f607983-c6eb-4d9f-b644-55247e8adda6) that’s now available as a CTP release in the AppFabric [LABS environment](http://portal.appfabriclabs.com/). You can find the presentation here: [http://player.microsoftpdc.com/Session/1f607983-c6eb-4d9f-b644-55247e8adda6](http://player.microsoftpdc.com/Session/1f607983-c6eb-4d9f-b644-55247e8adda6)

 

[![Introduction to Windows Azure AppFabric Caching](https://wadewegner.blob.core.windows.net/wordpress/2010/11/image.png)](http://player.microsoftpdc.com/Session/1f607983-c6eb-4d9f-b644-55247e8adda6)

 

A few interesting notes on the Caching service:

 

  
  * It’s a distributed, in-memory, application cache provided entirely as a service – no installation, management, or deployment required
   
  * Low latency and high throughput (i.e. it’s fast)
   
  * Based off of Windows Server AppFabric Caching (codename “Velocity”), and the development experience is identical
   
  * Local cache support allows you to keep your data in-memory on the client, reducing network latency penalties
   
  * You can cache any managed object, regardless of the size; if using local cache you pay no serialization costs
   
  * Secured by the Access Control service
   
  * Pre-built providers for ASP.NET session state and page output caching
 

You’re going to hear a lot about the Caching service from me, as it fills a very significant gap in the Windows Azure Platform.

 

For some more information, you should also take a look at these resources:

 

  
  * [Karandeep Anand: Windows Azure AppFabric Caching](http://channel9.msdn.com/posts/Karandeep-Anand-Windows-Azure-AppFabric-Caching)
   
  * [Windows Azure AppFabric Caching Marketing](http://www.microsoft.com/en-us/appfabric/azure/middleware-services.aspx#Caching)
   
  * [Windows Azure AppFabric Overview](http://www.microsoft.com/en-us/appfabric/azure/product.aspx)
   
  * [Windows Azure AppFabric Team Blog](http://blogs.msdn.com/b/windowsazureappfabric/archive/2010/10/28/introduction-to-windows-azure-appfabric-caching-ctp.aspx)
 

And of course, visit [http://portal.appfabriclabs.com/](http://portal.appfabriclabs.com/) to try it out today!
