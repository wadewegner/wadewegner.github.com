---
author: admin
comments: true
date: 2010-05-05 16:19:44+00:00
layout: post
slug: what-is-the-azure-appfabric
title: What is the Azure AppFabric?
wordpress_id: 566
categories:
- AppFabric
- Azure
- Azure AppFabric
- Windows Azure
---

If you take a look at the official [Windows Azure platform website](http://www.microsoft.com/windowsazure/), you’ll see two definitions for the Windows Azure platform AppFabric (hereafter referred to as the Azure AppFabric) prominently called out:

 

  
  * “… connects cloud services and on-premises applications.” 
   
  * “… helps developers connect applications and services in the cloud or on-premises.” 
 

While the purpose of the Azure AppFabric seems clear to me – **_enable developers to connect applications and services_** – there are a couple things that generally cause confusion: execution and branding. I plan to talk about how to use the Azure AppFabric quite a bit in the future, but in this post I want to address the branding.

 

The Azure AppFabric has been rebranded numerous times. This isn’t surprising given that it has largely been a community technology preview, but it has lead to some confusion.

 

So, some brief history …

 

>   
> 
> Note: this is based entirely on my cyber-sleuthing and personal experience. I’m sure I have gaps and perhaps an inadvertent inaccuracy, so as I get corrected I’ll update. I didn’t join Microsoft until early 2008, so the early days of the Azure AppFabric precedes my Microsoft employment.

 

In April 2007, the BizTalk Server team [announced](http://blogs.msdn.com/biztalk_server_team_blog/archive/2007/04/26/biztalk-services-are-live.aspx) that the CTP release of **_BizTalk Services _**was live. They had created an Internet Services Bus (ISB) that allowed developers to create “Internet scale composite applications more rapidly.” Clemens Vasters described this [new ISB in a post](http://blogs.msdn.com/clemensv/archive/2007/04/25/internet-service-bus.aspx). Later, in July 2007, the BizTalk Server team talked about [Hosted Workflows in BizTalk](http://blogs.msdn.com/biztalk_server_team_blog/archive/2007/07/22/more-detail-on-biztalk-services-and-hosting-wf-workflows-in-biztalk.aspx) – an exciting extension to the ISB announcement. Over time, Access Control was added into the mix as well.

 

Soon, word of _**Project Zurich**_ started hitting the airways. Mary-Jo Foley wrote about [“’Zurich,’ Microsoft’s elastic cloud”](http://www.zdnet.com/blog/microsoft/ozzie-foreshadows-zurich-microsofts-elastic-cloud/1503) back in July 2008, describing it as an initiative to “extend Microsoft’s .NET application development technologies to the Internet ‘cloud.’” Close, but not quite right. My first introduction to Project Zurich came while working on a project with [RedPrairie on a supply chain proof-of-concept](http://www.microsoft.com/presspass/events/pdc/docs/RedPrarieParnerRelease.doc), that ultimately culminated in a [Bob Muglia keynote demonstration at PDC 2008](http://channel9.msdn.com/pdc2008/KYN01/) (around 59 minutes).

 

At the Professional Developers Conference (PDC) 2008 the platform was rebranded **_.NET Services_** and included as part of the **_Azure Services Platform_**. You can actually still see some of the [.NET Services branding on this BizTalk Service page](http://www.microsoft.com/biztalk/en/us/netservices.aspx). By the fall of 2008, .NET Services had emerged as a mature platform (even though still in CTP) consisting of an Internet Service Bus, an Access Control Service (ACS), and Workflow Service. In June 2009, the .NET Services team announced that they were [pulling the Workflow Service](http://blogs.msdn.com/netservicesannounce/archive/2009/06/12/upcoming-important-changes-to-net-workflow-service.aspx). As Windows Workflow Foundation in .NET 4.0 evolved, it was clear that most customers wanted Workflow Services to also follow to the .NET 4.0 model (not .NET 3.5), which it was not. Consequently, .NET Services team pulled workflow and focused on the ISB and ACS.

 

At PDC 2009, .NET Services went through it’s most recent branding change, and was eventually launched in 2010 as the **_Windows Azure platform AppFabric_**. Of course, this is a _really_ long name, so most people just end up saying **_Windows Azure AppFabric_** or just **_Azure AppFabric_**.

 

The biggest challenge I see today with the name is that, at PDC 2009, we also rebranded “Dublin” and “Velocity” as the [Windows Server AppFabric](http://msdn.microsoft.com/en-us/windowsserver/ee695849.aspx) – almost too much name overloading, although there are some good reasons for it that will emerge over time. To make things clear, I’ll always say either **_Azure AppFabric_** or **_Server AppFabric_**.

 

If you really take a look at how this all has evolved, you can start to see how Microsoft’s cloud platform strategy has evolved over the last several years.

 

So, where does this leave us?

 

In my opinion, it leaves us with a technology that is a key differentiator in Microsoft’s cloud platform. I’m not just saying this – I really believe it, or I wouldn’t be [moving my family up to Redmond so that I can focus on it](http://www.wadewegner.com/2010/05/new-role-technical-evangelist-for-azure-appfabric/).

 

In closing, let’s be clear on two things – in Azure AppFabric, there’s both a Service Bus and Access Control Service.

 

>   
> 
> The **Service Bus **is an Internet-scale enterprise service bus that makes it easy to connect applications over the Internet. Services that register on the Service Bus can easily be discovered and accessed across any network topology.

 

>   
> 
> The **Access Controls Service** helps you build federated authorization into your applications and services, without the complicated programming that is normally required to secure applications that extend beyond organizational boundaries. 

 

Okay, now that I’ve spent a little time covering some history and the past, expect to see a major focus on what you can do today – and lots of code and examples.

 

Hope this helps.
