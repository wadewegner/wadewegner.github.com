---
title: "Updates Coming Soon to the Windows Azure Toolkit for Windows Phone 7"
date: 2011-05-09T00:00:00-08:00
draft: false
description: "From the whiteboard to the keyboard"
---

I’ve been really pleased with our delivery cadence for the [Windows Azure Toolkit for Windows Phone 7](http://watoolkitwp7.codeplex.com/). We first launched (v1.0) on March 23, 2011 – this launch included VS project templates, class libraries for storage and membership services, a sample application, and documentation. We had excellent coverage by [Mary Jo Foley](http://www.zdnet.com/blog/microsoft/microsoft-delivers-toolkit-for-using-windows-azure-to-build-windows-phone-7-apps/8993), [InfoQ](http://www.infoq.com/news/2011/03/windows-azure-toolkit-phone7), and others. On April 12, 2011—during MIX11—we shipped an update (v1.1) that provided support for [Microsoft Push Notification Services running in Windows Azure](http://www.wadewegner.com/2011/05/using-windows-azure-for-windows-phone-7-push-notification-support/) (we also shipped some bug fixes). What’s significant about the second release is that we demonstrated momentum – we want to ship early and ship often. Furthermore, we are working hard on updates—many of which are based on your feedback—and we’re getting them out as quickly as we can.

We want to stay as transparent as possible regarding our release plans, and consequently I’d like to tell you a bit about three significant updates coming to the next release of the Windows Azure Toolkit for Windows Phone 7.

**Windows Azure Access Control Service 2.0**

With the help and guidance of [Vittorio Bertocci](http://blogs.msdn.com/b/vbertocci/) (aka Captain Identity) we are introducing integration with the Access Control Service (ACS) 2.0. The current (and previous) versions of the toolkit provide a very simple ASP.NET membership store in Windows Azure Tables. With the next release the new project wizard will give you the ability to choose between ASP.NET membership and the ACS.

![clip_image002](https://wadewegner.blob.core.windows.net/wordpress/2011/05/clip_image0026.gif)

The wizard only asks for the essentials and will then configure ACS automatically for you. The result is that, after the wizard completes, you can hit F5 and run.

For more details on the upcoming ACS integration, take a look at Vittorio’s post: [Windows Azure Toolkit for Windows Phone 7 1.2 will Integrate with ACS](http://blogs.msdn.com/b/vbertocci/archive/2011/05/09/windows-azure-toolkit-for-windows-phone-7-1-2-will-integrate-with-acs.aspx).

**More Style**

Inspired by the Metro Design Language of Windows Phone 7, we’ve significantly updated the web application UI. Take a look at the new look …

![image](https://wadewegner.blob.core.windows.net/wordpress/2011/05/image5.png)

… as compared to the old.

![image](https://wadewegner.blob.core.windows.net/wordpress/2011/05/image6.png)

**Support for Windows Azure Storage Queues**

One feature we “missed” in our initial releases—and to be honest it was a matter of prioritization—was support for Windows Azure storage queues. With our next release we will provide support for queues in our storage library and the sample application.

![image](https://wadewegner.blob.core.windows.net/wordpress/2011/05/image7.png)

We hope that you’ll find these three updates significant and worthwhile.

We plan to release version 1.2 of the toolkit at TechEd North America 2011. As we are still finalizing the bits and pulling together the release, I won’t commit just yet to the exact date but I promise we’ll try to release as soon as possible.
