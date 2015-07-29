---
author: admin
comments: true
date: 2011-05-09 04:44:11+00:00
layout: post
slug: using-windows-azure-for-windows-phone-7-push-notification-support
title: Using Windows Azure for Windows Phone 7 Push Notification Support
wordpress_id: 1176
categories:
- Push Notification
- Toolkit
- Windows Azure
- Windows Phone 7
---

At MIX11 we released the [Windows Azure Toolkit for Windows Phone 7 v1.1.0](http://watoolkitwp7.codeplex.com/releases/view/61952), which included out-of-the-box support for the [Microsoft Push Notification Service](http://msdn.microsoft.com/en-us/library/ff402558%28VS.92%29.aspx) (MPNS) for Windows Phone. The MPNS provides an efficient way for an application to register itself for updates that are pushed directly to the phone rather than writing the application to frequently poll a web service to look for pending notifications. This has the benefit of reducing the impact on your phone battery, as polling results in the device radio turning on frequently.

 

When using the MPNS you need to setup your own web service that notifies Microsoft to send the notification. It’s your responsibility to setup this service – you have to create the notification channel, bind it to the correct type of notification, and ultimately host the web service. The Windows Azure Toolkit for Windows Phone 7 makes it really easy to not only host your web service in Windows Azure but also create the channel and setup the correct notification binding (largely thanks to the [Windows Push Notification Service Side Helper Library](http://windowsteamblog.com/windows_phone/b/wpdev/archive/2011/01/14/windows-push-notification-server-side-helper-library.aspx) from Yochay Kiriaty).

 

To get started, grab v1.1 of the toolkit from our CodePlex site: [http://watoolkitwp7.codeplex.com/](http://watoolkitwp7.codeplex.com/). If this is your first time, take a look at the [Getting Started documentation](http://watoolkitwp7.codeplex.com/wikipage?title=Getting%20Started&referringTitle=Documentation) and [webcast](http://channel9.msdn.com/posts/Getting-Started-with-the-Windows-Azure-Toolkit-for-Windows-Phone-7).

 

Once you start up the application, the first thing you’ll want to do is enable push notifications. To do this, check the box on the initial page to create the channel.

 

[![image](https://wadewegner.blob.core.windows.net/wordpress/2011/05/image_thumb.png)](https://wadewegner.blob.core.windows.net/wordpress/2011/05/image.png)

 

Once this has successfully registered, head over to the website and log in as the administrator. There’s now a Push Notifications tab where you’ll see your user registered:

 

[![image](https://wadewegner.blob.core.windows.net/wordpress/2011/05/image_thumb1.png)](https://wadewegner.blob.core.windows.net/wordpress/2011/05/image1.png)

 

From here you can the the push notification channel by sending all three push types: toast, tile, and raw. In that order, here’s what you’ll see (note: to see the tile message you must first pin the app to start):

 

[![image](https://wadewegner.blob.core.windows.net/wordpress/2011/05/image_thumb2.png)](https://wadewegner.blob.core.windows.net/wordpress/2011/05/image2.png) [![image](https://wadewegner.blob.core.windows.net/wordpress/2011/05/image_thumb3.png)](https://wadewegner.blob.core.windows.net/wordpress/2011/05/image3.png) [![image](https://wadewegner.blob.core.windows.net/wordpress/2011/05/image_thumb4.png)](https://wadewegner.blob.core.windows.net/wordpress/2011/05/image4.png)

 

Not bad for an out-of-the-box experience requiring ZERO updates!

 

To learn more about this version of the toolkit – or to see it live and in action – take a look at the session I presented at MIX11 entitled [Building Windows Phone 7 Applications with the Windows Azure Platform](http://channel9.msdn.com/Events/MIX/MIX11/SVC02).

 

 

Hope this helps!
