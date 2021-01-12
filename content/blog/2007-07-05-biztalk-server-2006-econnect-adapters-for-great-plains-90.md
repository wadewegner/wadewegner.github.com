---
author: admin
categories:
- BizTalk Server
- eConnect
comments: true
date: "2007-07-05T17:34:14Z"
slug: biztalk-server-2006-econnect-adapters-for-great-plains-90
title: BizTalk Server 2006 eConnect Adapters for Great Plains 9.0
wordpress_id: 62
---

I've started working on a project that integrates into Great Plains 9.0 using eConnect. Not having done this using BizTalk Server 2006, I thought I'd document some of my observations and the steps I took.




To begin, I was able to get eConnect for GP (en_econnect_for_gp.zip) through my MSDN subscription. If you have Great Plains 9.0, then you should be able to get your hands on this package. When you unzip the package, you'll notice two files:






  * eConnectInstallAdminGuide.pdf - This is a great document that, if you are going to interact and develop with eConnect, I highly encourage you to read. There are three parts: eConnect Basics, Installation, and Administration. At the very least, developers should reach the first part, which includes an overview and architecture chapter.

  * Microsoft_Business_Solutions_eConnect.msi - This is the actual installation file. Note: it contains MUCH more than just the BizTalk Server adapters.



After reading through the eConnect guide, I ran the installation MSI file. A few highlights / comments:






  * I chose the "Custom" setup. I never choose Standard or Complete.

  * My sole purpose at this point is to install the BizTalk adapter with the eConnect schemas; follow the administration guide, instead of this post, if you're setting up for production.

  * There are a lot of features, including:


    * BizTalk Components - both 2004 and 2006

    * Business Objects - installs the Business Objects into SQL Server

    * COM+ Components - installs COM components and .NET assemblies

    * eConnect Incoming Service - incoming eConnect Win32 service

    * eConnect Outgoing Service - outgoing eConnect Win32 service

    * eConnect Replication Service - eConnect Win32 replication service

    * eConnect Help - help

    * eConnect Samples - .NET and VB6 examples

    * Queue Control - view documents in MSMQ queue

    * Schemas - eConnect XSD and XDR schemas


  * Not knowing any better, I decided to only include the following features:


    * BizTalk Components

    * eConnect Help

    * eConnect Samples

    * Schemas



Once you have installed eConnect 9.0, you must still install the adapters for BizTalk Server. Browser to the following folder: C:Program FilesMicrosoft Great PlainseConnect9BizTalkBizTalk 2006 (obviously, choose BizTalk 2004 if you are using 2004). From here you can run the BTS_eConnectAdapter.msi file and install the BizTalk Server 2006 adapter.




Once the installation for the adapter is complete, make sure you update your platform settings and add the adapter. Follow these steps:






  1. Open the BizTalk Server 2006 Administration Console.

  2. Go to BizTalk Group -> Platform Settings -> Adapters.

  3. Right-click the Adapters folder, and select New -> Adapter.

  4. Enter a name for the adapter (e.g. eConnect) and select the "Dynamics GP eConnect" adapter from the adapter list.

  5. Click OK. Make sure you restart your host instances.



Pretty simple and straightforward.




Now, browse to the following folder: C:Program FilesMicrosoft Great PlainseConnect9XML Schemas. Here you will notice all the XDR and XSD schemas. In particular, the "Incoming XSD Individual Schemas" and the "Incoming XSD Schemas" folders. The first has every schema in an individual XSD, whereas the second has them all in an eConnect.xsd file. Depending on you circumstances, you can import these schemas into your BizTalk projects, so that you can utilize them in Orchestrations, maps, or whatever else you choose. Additionally, once you deploy your projects assemblies, the schemas will be available within your BizTalk solution.




And finally, one other folder you may want to look at is C:Program FilesMicrosoft Great PlainseConnect9XML Sample DocumentsIncoming. This folder has a number of sample XML documents that you can use for testing. VERY useful stuff here!




Just a few things I've learned and observed while getting all this configured. I'm sure I'll post more as I continue to use the eConnect adapter.




I hope this helps!
