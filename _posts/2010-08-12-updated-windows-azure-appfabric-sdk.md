---
author: admin
comments: true
date: 2010-08-12 22:36:45+00:00
layout: post
slug: updated-windows-azure-appfabric-sdk
title: Updated Windows Azure AppFabric SDK
wordpress_id: 710
categories:
- QFE
- Windows Azure AppFabric
---

![Windows Azure AppFabric](https://wadewegner.blob.core.windows.net/wordpress/2010/08/AppFabric2.png)

Today the Windows Azure AppFabric team released an [update to the SDK](http://www.microsoft.com/downloads/details.aspx?FamilyID=39856a03-1490-4283-908f-c8bf0bfad8a5&displaylang=en). This release is what’s called a QFE (quick fix engineering) update that’s intended to address bugs or breaking issues. To my knowledge, this is the first QFE for the Windows Azure AppFabric. In this case, the QFE fixed one bug:

 
> Adding a Ws2007HttpRelayBinding endpoint that uses TransportWithMessageCredential security mode to a ServiceHost that also exposes a local MEX endpoint (not visible through the Service Bus) using the mexHttpsBinding, causes the local MEX endpoint to stop working. Invoking the MEX endpoint results in an InvalidOperationException.

 

If you are not experiencing this bug, there is no need for you to download the new version of the SDK.

 

Here’s the [post from the Windows Azure AppFabric team blog](http://blogs.msdn.com/b/windowsazureappfabric/archive/2010/08/12/new-version-of-the-windows-azure-appfabric-sdk-available-for-download.aspx).

 
