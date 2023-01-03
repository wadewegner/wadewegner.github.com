---
author: admin
categories:
- Azure
- User Groups
comments: true
date: "2008-12-11T20:30:01Z"
slug: looking-to-explore-the-cloud-then-join-your-community
title: Looking to explore the Cloud? Then join your community!
wordpress_id: 648
---

![Windows Azure Cloud Computing User Group](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/FirstmeetingoftheChicagoWindowsAzureClou_C184/UG-1_thumb.png)

This past Tuesday was the inaugural meeting of the [**Windows Azure Cloud Computing User Group**](http://cloudcomputingusergroup.com/) in Chicago, IL. The mission of this new user group is to provide a forum for developers and architects to discuss Microsoft's vision of [**Software + Services**](http://www.microsoft.com/softwareplusservices/) (S+S), and in particular the [Azure Services Platform](http://www.azure.com/). This user group is sponsored by [Neudesic](http://www.neudesic.com), and the presenter for the first meeting was Bryce Calhoun, Director at Neudesic. Despite the snow and difficult driving conditions, the meeting was well attended by many members of the local community.

Bryce gave an excellent presentation on cloud computing that fostered some good discussion within the group. His presentation was divided into the following topics:

  * What's it all about?  
  * Why take it seriously?  
  * Software + Services  
  * Azure: Microsoft's Cloud Services Platform  
  * Developer Experience  
  * Business Models

Bryce also dropped into Visual Studio and built out a Windows Azure application that communicated with data stored in SQL Data Services - simple yet effective.

I was pleased when Bryce showed this quote from Steve Ballmer early in the presentation, as I think it really captures the value of engaging with this user group:

!["The software and services era is now. We are writing new software, we will be delivering betas and design previews, and the time to engage is now." Steve Ballmer](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/FirstmeetingoftheChicagoWindowsAzureClou_C184/UG-2_thumb.png)

The Azure Services Platform was announced at the [Microsoft Professional Developers Conference](http://www.microsoftpdc.com/) (PDC) this past October, and you can already start building applications today through the public CTP. Now is the time to explore the technology, try out new and different ways of writing software, _and provide feedback to Microsoft on what works and what doesn't work_! It is my hope that this user group becomes a forum to explore how the tools and technologies that are available through Azure help (or hinder!) businesses when they attempt to create compelling applications in the cloud.

Below are some of my notes from the presentation. This isn't an exhaustive set of notes, but rather a few pieces from the presentation that I found interesting and compelling. If you want a summary of the entire presentation, well, perhaps you should come to the next meeting.

**_Why take it seriously?_**

I really liked Bryce's discussion on why we should all take the cloud seriously. In particular, I felt that the following slide highlighted how all the big players - Microsoft, Google, Yahoo, Amazon, Oracle, etc. - are investing, to some degree, in the cloud.

![Big players investing in the cloud](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/FirstmeetingoftheChicagoWindowsAzureClou_C184/UG-5_2.png)

_**Software + Services**_

Bryce had a slide that discussed S+S that I found rather interesting. In this slide ...

![S+S choices](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/FirstmeetingoftheChicagoWindowsAzureClou_C184/UG-3_2.png)

... you can see that, on the left, we have a fairly standard architecture for on-premises software. You might find this in any enterprise data center, as it is specifically designed to handle the needs and scale of an enterprise. On the right you can see services in the cloud - notice how the various tiers are designed to scale horizontally and provide "on-demand" scaling. This architecture is significantly different from your standard on-premises architecture because you are now dealing with _Internet_-scale, something that most companies and enterprises have little experience at providing.

What's missing from this picture, in my opinion, is an illustration that shows that S+S is not an either/or proposition. Instead, S+S is about choice and flexibility. There's no expectation that businesses will move all their on-premises assets and applications into the cloud; instead, they will probably identity specific instances in which it may make sense to move an application, a database, or some other asset into the cloud, and then use technology (perhaps provided by [**Microsoft's .NET Services**](http://www.microsoft.com/azure/netservices.mspx)) to bridge their on-premises systems to their cloud-based systems. It's all about choice.

I really like this slide from Ray Ozzie's keynote presentation at PDC:

![S+S continuum](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/FirstmeetingoftheChicagoWindowsAzureClou_C184/UG-6_2.png)

You can see here that the technologies provided by Microsoft provide a continuum that gives you an on-premises platform as well a set of cloud services that complement the on-premises platform. You can use what makes sense and works in your scenario.

**_Business Models_**

The best source of information regarding the Azure business model (i.e. pricing and licensing) continues to be the [**Pricing & Licensing Overview**](http://www.microsoft.com/azure/pricing.mspx) page. There you can read about the four principles of the business model:

  1. Consumption-based model  
  2. Pricing attractive with the market  
  3. Market expansion opportunity for Microsoft partners  
  4. Easy access through the Web, or through existing channels and programs

Bryce made some good points regarding Opex (operational expenditure) and Capex (capital expenditure) when speaking to this slide:

![OPEX & CAPEX w/ and w/o cloud services](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/FirstmeetingoftheChicagoWindowsAzureClou_C184/UG-4_4.png)

This slide attempts to show that you can significantly reduce your costs when you no longer have to maintain a large infrastructure to support your on-premises software. Instead, if you move towards a utility compute model, you only pay for the services you need and let the provider absorb the cost of maintaining the data center.

I noticed a little healthy skepticism from the group, and rightly so. This is a new platform, and the onus is on Microsoft to show that they (er, we) can deliver on the promise of cloud computing. Some of the top concerns I heard from participants included:

  * The security provided by Azure storage and SQL Data Services  
  * The (perceived) disconnect between SQL Server and SQL Data Services and confusion regarding the roadmap 
  * The business model and lack of specific pricing and licensing details

It seems to me that these topics would make excellent topics for future posts, and I will try to do so. In the meantime, I encourage those of you that have these same concerns and questions to engage in your community, try out the new technology, and explore the capabilities of the Azure Services Platform - all you have to do is go to [http://www.azure.com/](http://www.azure.com/) to get started.

All in all, I am very excited about this new users group and the community it will create, and I appreciate Neudesic's efforts to launch these groups around the U.S (see [**http://cloudcomputingusergroup.com/**](http://cloudcomputingusergroup.com/) for details). The current plans are to have another Chicago user group meeting in January, most likely held at the Microsoft office in downtown Chicago. Once I hear something concrete I will be sure and let you know.

For those of you that attended the user group meeting (and even those of you that did not, for that matter), please feel free to reach out to me if you have any questions on the Azure Services Platform, and how you might start building some compelling applications. You can reach me through this blog.
