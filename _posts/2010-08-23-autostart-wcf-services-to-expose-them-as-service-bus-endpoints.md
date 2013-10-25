---
author: admin
comments: true
date: 2010-08-23 16:30:46+00:00
layout: post
slug: autostart-wcf-services-to-expose-them-as-service-bus-endpoints
title: AutoStart WCF Services to Expose them as Service Bus Endpoints
wordpress_id: 752
categories:
- Autostart
- Service Bus
- WCF
- Windows Azure AppFabric
- Windows Server AppFabric
---

A couple months ago I wrote a post on how to host [WCF services in IIS that expose themselves as endpoints on the Windows Azure AppFabric Service Bus](http://www.wadewegner.com/2010/05/host-wcf-services-in-iis-with-service-bus-endpoints/). The principal challenge in this scenario is that IIS/WAS relies on message-based activation and will only launch the host after the first request comes in. However, until the host is launched the service will not connect to the Service Bus, and consequently will never receive a message. A classic [catch-22](http://dictionary.reference.com/browse/Catch-22).

 

The solution I proposed was to leverage the Application Warm-Up Extension for IIS 7.5, which will proactively load and initialize processes before the first request arrives. While this is acceptable, I’ve found a better solution using the Windows Server AppFabric Autostart (thanks to conversations with [Ron Jacobs](blogs.msdn.com/b/rjacobs)).

                                                                     

            ![]()             ![](Preview.png)                                                         

            

                                   ![Get Microsoft Silverlight](http://go2.microsoft.com/fwlink/?LinkId=108181)              
                               [                 ![Get Microsoft Silverlight]()             ](http://go2.microsoft.com/fwlink/?LinkID=124807)                            

               

[Windows Server AppFabric Autostart](http://msdn.microsoft.com/en-us/library/ee677260.aspx) is a feature introduced in Windows 7 and Windows Server 2008 R2. The primary use cases are for reducing the latency incurred by the first message and to host WCF transports/protocols for which their are no listener adapters. As you can see, initializing the host so that it connects to the Service Bus is another benefit.

 

To set this up, ensure that you have installed Windows Server AppFabric on your machine. I personally recommend you use the [Web Platform Installer](http://www.microsoft.com/web/downloads/platform.aspx) to do this for you (I detail how to do this in the first part of my post on [Getting Started with Windows Server AppFabric Cache](http://www.wadewegner.com/2010/08/getting-started-with-windows-server-appfabric-cache/)). Once you have this installed, follow these steps:

 

  
  1. Open **IIS Manager**. Navigate to your web application. 
   
  2. Click on **Configure** in Actions pane. 
   
  3. Configure the application to either autostart all the services by choosing **Enabled** or specific services by choosing **Custom**. 
   
  4. If you specified **Custom**, navigate to the configuration panel for that specific service and turn autostart to **Enabled**. 
 

Pretty straightforward. I think you’ll like this solution, as it keeps everything within the AppFabric family.
