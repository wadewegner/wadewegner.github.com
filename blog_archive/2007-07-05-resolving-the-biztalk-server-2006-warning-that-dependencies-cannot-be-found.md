---
author: admin
categories:
- BizTalk Server
comments: true
date: "2007-07-05T15:32:51Z"
slug: resolving-the-biztalk-server-2006-warning-that-dependencies-cannot-be-found
title: Resolving the BizTalk Server 2006 warning that dependencies cannot be found
wordpress_id: 63
---

I just recently started a new BizTalk Server 2006 project in which I created a custom pipeline component to assist in the compression and decompression of messages. The plan is to have a receive location monitor a folder via the File adapter, and when a zip file is uploaded via FTP from a 3rrd party process, the receive location will use the custom pipeline component , via a receive pipeline, to decompress the file and publish the message to the MessageBox. Pretty slick!

> _See the WingtipToys solution in the SDK folder for a great heads-start to creating a compression / decompression pipeline component. You can find this solution in the following folder: C:Program FilesMicrosoft BizTalk Server 2006SDKScenariosPM._

So, to get this rolling, I added a C# project to my BizTalk solution. There are few things I configure on these C# library projects, as I need to utilize the output assembly in my Pipelines BizTalk project.

  1. Set the build output path to the following location (for the Debug configuration): C:Program FilesMicrosoft BizTalk Server 2006Pipeline Components. This way, every time the project is built, the debug assembly is placed in the Pipeline Components folder. From here, you can then reference the pipeline component appropriate through both Visual Studio 2005 (for your receive pipeline) and when you deploy it to BizTalk Server.  

  2. Uncheck the "Build" checkbox, for the C# library project, in the Solution Configuration. This way, when you rebuild or deploy the solution, the C# library project isn't also rebuilt. The reason is that you will most likely get errors or warnings indicating that the file is in use, and cannot be rebuilt. This is fine - you can simply do this manually.

Having made these changes, you now have a pretty flexible setup for moving forward.

When you reference the custom pipeline component assembly, you'll want to reference the assembly that has been built in the C:Program FilesMicrosoft BizTalk Server 2006Pipeline Components folder. This way, when you deploy your receive or send pipelines, BizTalk will continue to reference the same assembly.

However, having referenced this assembly, you will see these warnings the next time you build your solution:

> ------ Build started: Project: BizTalkApp, Configuration: Development .NET ------  
Updating references...  
The dependency 'Microsoft.BizTalk.Tracing' could not be found.  
The dependency 'Microsoft.BizTalk.Bam.EventObservation' could not be found.  
The dependency 'Microsoft.BizTalk.Streaming' could not be found.  
The dependency 'Microsoft.BizTalk.XPathReader' could not be found.  
Performing main compilation...

While these warnings are harmless, they are annoying. In order to "suppress" these warnings, select the referenced custom pipeline component assembly, and set Copy Local from True to False. Once you do this, these warnings will disappear.

I hope this helps!
