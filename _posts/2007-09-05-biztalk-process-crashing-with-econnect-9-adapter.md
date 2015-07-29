---
author: admin
comments: true
date: 2007-09-05 18:51:06+00:00
layout: post
slug: biztalk-process-crashing-with-econnect-9-adapter
title: BizTalk process crashing with eConnect 9 adapter
wordpress_id: 35
categories:
- BizTalk Server
- eConnect
- Troubleshooting
---

I was able to use the XML sample documents that come with eConnect (C:Program FilesMicrosoft Great PlainseConnect9XML Sample DocumentsIncoming) to write some test data into my GP database using the eConnect 9.0 adapters for BizTalk Server 2006. I setup a test environment using the TWO database for Great Plains, and through the adapter was able to insert data. Feeling that I had made some good progress, I shutdown my machine (they are all virtualized environments) and went home for the night.




When I started everything up the next day I tried to reproduce this behavior so that I could move on with my integration. However, every time I tried to write the document into GP through the adapter, I got the following error:




[![BizTalk DW Reporting](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/BizTalkprocesscrashingwitheConnect9adapt_8E58/Error_thumb.gif)](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/BizTalkprocesscrashingwitheConnect9adapt_8E58/Error.gif)




> 

> 
> Event Type: Error  
Event Source: BizTalk DW Reporting  
Event ID: 1000  
Description: Faulting application btsntsvc.exe, version 3.5.1602.0, stamp 4410e6b9, faulting module kernel32.dll, version 5.2.3790.4062, stamp 46264680, debug? 0, fault address 0x0000bee7. 




This had me stumped for a little while. I decided to use a useful tool I received from Microsoft called the Direct Document Sender.NET (I've attached the file at the bottom of this post) to try and diagnose the problem. This tool allows you to post an XML document directly to eConnect so that you can test out the functionality without any 3rd party application (e.g. BizTalk Server).


Using this application, I tried to post the XML file again. This time I received a much more useful error:


> 

> 
> System.Runtime.InteropServices.COMException (0x8000401A): The server process could not be started because the configured identity is incorrect. Check the username and password.




Aha! This was a much more useful error!


I opened up Component Services (Start --> Administrative Tools --> Component Services), and browsed to Computers --> My Computer --> COM+ Applications --> **eConnect 9 for Great Plains**. The Microsoft Great Plains eConnect Version 9 COM Plus Package has a tab entitled **Identity** which allows you to define the user account under which the application runs. Turns out that _somehow_ my account was switched from the user I specified at installation to the interactive user system account:


[![Component](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/BizTalkprocesscrashingwitheConnect9adapt_8E58/Component_thumb.gif)](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/BizTalkprocesscrashingwitheConnect9adapt_8E58/Component.gif)


And this was the root cause of my problem. eConnect requires integrated security and uses the user specified in this identity tab to access the GP database. Consequently, the user specified must have access to the appropriate database on the GP server and also be a part of the DYNGRP role.




So, I went ahead and added my user to the DYNGRP role, made sure it had the appropriate access, and then updated account used by the COM+ package:




[![Component2](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/BizTalkprocesscrashingwitheConnect9adapt_8E58/Component2_thumb.gif)](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/BizTalkprocesscrashingwitheConnect9adapt_8E58/Component2.gif)




Having made these changes, I tested with the Document Sender.NET application - worked the first time! Confidently I tested with BizTalk, and sure enough everything started to work!




I'm not sure why it initially worked and then stopped working after I restarted the machines. I'm not sure if the account credentials changed or if something else was happening the first time. All I know is that you have to use an appropriate user that has access to the database and is a member of the appropriate role, otherwise you'll get the errors I listed above.




I hope this helps!




[DirectDoc9.zip (7.42 KB)](https://wadewegner.blob.core.windows.net/wordpress/content/binary/DirectDoc9.zip)
