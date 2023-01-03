---
author: admin
categories:
- IAAS
- PAAS
- Windows Azure
comments: true
date: "2012-06-07T16:43:54Z"
slug: early-thoughts-on-the-windows-azure-announcements
title: Early Thoughts on the Windows Azure Announcements
wordpress_id: 1830
---

Today’s release marks a significant milestone for Windows Azure. To date, Windows Azure has been a platform that allows developers to build and run applications across Microsoft’s global datacenters – the key emphasis has been on “applications”. Windows Azure has not been a platform for providing the underlying infrastructure for running your own virtual machine – this has been a key pain point for many customers looking to move to the cloud that Microsoft has heard loud and clear. Today’s announcement makes it clear that Windows Azure is more than just a Platform-as-a-Service provider.

 

In my opinion, there are three significant components of today’s announcements worth delving into deeper:

 

  
  * New Infrastructure-as-a-Service (IaaS) capabilities.
   
  * Free (or low-cost) hosting with Windows Azure Websites.
   
  * Enhanced cloud networking capabilities that support VPN connections between an on-premises corporate network and Windows Azure.
 

Until now, Microsoft has never competed directly with Amazon EC2 with respects to IaaS nor with cloud platforms like Heroku. The new IaaS and Websites capabilities, combined with the ability to extend on-premises networks to the cloud, provides a number of ways that Windows Azure can now distinguish itself from other platforms and—in my opinion—will drive many new enterprises and a large number of developers to adopt Windows Azure.

 

**Infrastructure-as-a-Service**

 

Windows Azure has long had the concept of a “Virtual Machine role” but the fundamental problem has been the inability to persist changes made to the virtual machine image provided by the customer (i.e. the guest VM) during reboots or recycling. Supporting VM persistence in Windows Azure means that the guest VM will not lose these updates. This unlocks many workloads that previously did not work in Windows Azure – certainly products like SharePoint and SQL Server but also custom line-of business applications that previously were difficult to move to Windows Azure.

 

In addition to VM persistence, Windows Azure will also give customers the ability to run Linux VMs. There’s been a lot of interest and speculation regarding Microsoft’s strategy moving forward with Linux and open source. I think Microsoft recognizes that their customers run more than just Windows in their enterprise, and this is an opportunity for Windows Azure to run as many workloads as possible. We’ve seen this shift in Microsoft in a number of different ways – support for Node.js and Java in Windows and Windows Azure, the creation of a new interoperability subsidiary, and many more. The cloud provides a way to make it easier to connect all of these different platforms and technologies, and my take is that Microsoft is trying to make Windows Azure the best and simplest place to run your applications regardless of the platform or technology.

 

**Windows Azure Websites**

 

It’s exciting to see Microsoft continue to evolve its strategy with Windows Azure to make it increasingly accessible to the breadth of developers out there.     
Windows Azure Websites is a hosting platform for web applications. It provides a number of different deployment and runtime options beyond the existing Web Role, including:

 

  
  * Target both Microsoft and non-Microsoft technologies already running in the environment, including SQL Azure, MySQL, PHP, Node.js, and (of course) .NET.
   
  * Deploy via Git, Web Deploy, FTP, or TFS.
   
  * Run in a high-density / multitenant VM for little-to-no cost or choose a dedicated deployment path.
 

In addition to providing simpler and more consistent ways to deploy applications across different hosting platforms (e.g. Windows Azure, Windows Server, and hosting providers), Windows Azure Websites provides a way for Microsoft to bring thousands—perhaps even hundreds of thousands—of new developers to the platform with the offer of little-to-no cost hosting.

 

**Cloud Networking**

 

Windows Azure Virtual Networks allows a company to connect their cloud applications and solutions to their local network. This occurs at the networking layer through standard VPN devices. Coupled with IaaS support, this provides a ton of flexibility with respects to the kinds of workloads a customer moves to Windows Azure. Don’t want to move your sensitive SQL Server database? You don’t need to. Setup a VPN to your applications in Windows Azure and let them communicate directly back to your applications that live on-premises.

 

There’s certainly a lot more to talk about – new services, portal, SDK, tools, and so much more! These thoughts are pretty early—in fact, I write this before today’s MEET Windows Azure event—and there’s so much more to talk about!
