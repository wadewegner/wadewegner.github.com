---
author: admin
categories:
- Silverlight
comments: true
date: "2007-07-23T15:17:41Z"
slug: setting-up-a-silverlight-development-environment
title: Setting up a Silverlight development environment
wordpress_id: 51
---

This weekend I spent some time playing with Silverlight and Orcas. Needless to say, I am very impressed with both. Here's a sample [Hangman](http://apps.wadewegner.com/hangman) application I created in about three hours.




I thought I'd go ahead and document the steps I took when I setup my development environment. I'll try to come back here and update the content, so that it stays pertinent as updates are released.




**Setting up a Silverlight development environment**






  1. Make sure to read everything you can on [Silverlight](http://silverlight.net/), as well as the [Get Started](http://silverlight.net/GetStarted/) Silverlight page (I've listed a number of good blogs at the bottom of this post).

  2. Install Microsoft Silverlight 1.0 Beta ([for Windows](http://go.microsoft.com/fwlink/?LinkID=89015&clcid=0x409)). This is the runtime that's required to experience Silverlight applications.

  3. Install Microsoft Silverlight 1.1 Alpha ([for Windows](http://go.microsoft.com/fwlink/?LinkID=88986&clcid=0x409)). This is the runtime that's required to experience Silverlight applications written with .NET.

  4. Reboot.

  5. Install [Microsoft Visual Studio codename "Orcas" Beta 1](http://go.microsoft.com/fwlink/?LinkID=89146&clcid=0x409). Soon to be Visual Studio 2008, this is the next evolution of Visual Studio 2005. It's pretty sweet.

  6. Reboot.

  7. Optional: Install Microsoft MSDN Library for Visual Studio codename "Orcas". Watch out - during the installation of the MSDN Library, it took about five minutes for it to complete this step. Be patient, and let it finish.  
![MSDN Library install](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/8cd2f9f4af11_7EDC/image3_thumb.png)

  8. Reboot.

  9. Install [Microsoft ASP.NET Futures (May 2007)](http://go.microsoft.com/fwlink/?LinkID=89147&clcid=0x409). This provides you with ASP.NET controls for Silverlight.

  10. Install [Expression Blend 2 May Preview](http://go.microsoft.com/fwlink/?LinkID=79076&clcid=0x409). This is a design tool that allows a user to interact with Silverlight. Note: this is different from Expression Blend that can be found on MSDN. Make sure to get this download. Also, [Tim Heuer](http://timheuer.com/blog/) pointed me to [http://www.microsoft.com/Expression/products/download.aspx?key=blend2maypreview](http://www.microsoft.com/Expression/products/download.aspx?key=blend2maypreview), which provides a product key and longer trial (180-day evaluation). Note: when I went to install Expression Blend 2, I was only given two choices: Vista, or Windows XP. My development VM is Windows Server 2003 Standard R2. I chose Windows XP, and haven't had any problems.  
![Expression Blend 2 choice](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/8cd2f9f4af11_7EDC/image4_thumb.png)

  11. Install [Microsoft Silverlight Tools Alpha for Visual Studio codename "Orcas" Beta 1](http://go.microsoft.com/fwlink/?LinkID=89149&clcid=0x409). This is an add-on that allows you to create Silverlight applications using .NET.

  12. Download [Microsoft Silverlight 1.0 Beta Software Development Kit (SDK)](http://go.microsoft.com/fwlink/?LinkID=89144&clcid=0x409). This is a zip file that contains documentation, samples along with templates for Visual Studio, and has also a “Go Live” license that enables building commercial applications. I unzipped it to C:Program FilesMicrosoft SilverlightSDKs.

  13. Download [Microsoft Silverlight 1.1 Alpha Software Development Kit (SDK)](http://go.microsoft.com/fwlink/?LinkID=89145&clcid=0x409). This is another zip file that contains documentation and samples Silverlight Web experiences that target Silverlight 1.1 Alpha.



That should be enough to get your environment up and running. Once that's complete, watch this Silverlight walk-through: [http://silverlight.net/quickstarts/silverlight10/xaml.aspx](http://silverlight.net/quickstarts/silverlight10/xaml.aspx).




Here are some good Silverlight blogs I've found (send me email, or leave me a comment, if you know of more):






  * [Tim Heuer - Method ~ of ~ failed](http://timheuer.com/blog/)

  * [Tim Sneath - Musing of a Client Platform Technical Evangelist](http://blogs.msdn.com/tims/default.aspx)

  * [Silverlight Blogs](http://silverlight.net/blogs/)

  * [Mike Ormond's Blog - In my world, things would be simpler than this ...](http://blogs.msdn.com/mikeormond/default.aspx)



Also, here are a few interesting posts:




  * [Porting to Linux](http://tirania.org/blog/archive/2007/Jun-21.html)

  * [Google Gears and Silverlight](http://nerddawg.blogspot.com/2007/06/google-gears-and-silverlight.html)

  * [Why Silverlight is important](http://www.techcrunch.com/2007/05/01/take-time-to-understand-silverlight-its-important/)

  * [Strategy behind Silverlight](http://jeremy.zawodny.com/blog/archives/009000.html)



I hope someone finds this useful! Leave a comment if you've developed something cool, so that I can check it out!
