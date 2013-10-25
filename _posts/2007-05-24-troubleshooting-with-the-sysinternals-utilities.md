---
author: admin
comments: true
date: 2007-05-24 01:47:56+00:00
layout: post
slug: troubleshooting-with-the-sysinternals-utilities
title: Troubleshooting with the Sysinternals utilities
wordpress_id: 86
categories:
- Tools
- Troubleshooting
---

As a consultant, I often have to spend a great deal of time diagnosing issues that are confronting my clients. Over the years I've used some of the tools available from [Sysinternals](http://www.microsoft.com/technet/sysinternals/default.mspx), but I honestly never put a lot of energy or time into really learning to use them. On my most recent project, however, I've found them to be tremendously helpful, and I'm now a confirmed believer!

There are a number of useful tools available, and I don't profess to have used or mastered them all. There are a few, however, that I strongly encourage you to familiarize yourself with:

  * [Process Monitor](http://www.microsoft.com/technet/sysinternals/ProcessesAndThreads/processmonitor.mspx) - This montioring tool monitors the file system, Registry, processes, threads, and DLL activity in real-time. Ever need to track down where data is getting written? Ever wonder why you try to write or read from the Registry and receive errors? This utility will help you track that down. Process monitor is a combination of two older tools (Filemon and Regmon), and adds a whole slew of additional features such as rich and non-destructive filtering, comprehensive event properties such session IDs and user names, reliable process information, full thread stacks with integrated symbol support for each operation, simultaneous logging to a file, and more.

[![](http://images.wadewegner.com/wordpress/content/binary/WindowsLiveWriter/TroubleshootingwiththeSysinternalsutilit_10F4D/ProcessMonitor_thumb.gif)](http://images.wadewegner.com/wordpress/content/binary/WindowsLiveWriter/TroubleshootingwiththeSysinternalsutilit_10F4D/ProcessMonitor%5B2%5D.gif)

[![](http://images.wadewegner.com/wordpress/content/binary/WindowsLiveWriter/TroubleshootingwiththeSysinternalsutilit_10F4D/ProcessMonitor2_thumb.gif)](http://images.wadewegner.com/wordpress/content/binary/WindowsLiveWriter/TroubleshootingwiththeSysinternalsutilit_10F4D/ProcessMonitor2%5B2%5D.gif)

  * [Process Explorer](http://www.microsoft.com/technet/sysinternals/Security/ProcessExplorer.mspx) - Find out what files, registry keys and other objects processes have open, which DLLs they have loaded, and more. This utility will even show you who owns each process. This tool is excellent for tracking down programs or processes that have locked files and directories. It also has some powerful search capability that shows you which processes have handles opened or DLLs loaded.

[![](http://images.wadewegner.com/wordpress/content/binary/WindowsLiveWriter/TroubleshootingwiththeSysinternalsutilit_10F4D/ProcessExplorer_thumb.gif)](http://images.wadewegner.com/wordpress/content/binary/WindowsLiveWriter/TroubleshootingwiththeSysinternalsutilit_10F4D/ProcessExplorer%5B2%5D.gif)

[![](http://images.wadewegner.com/wordpress/content/binary/WindowsLiveWriter/TroubleshootingwiththeSysinternalsutilit_10F4D/ProcessExplorer2_thumb.gif)](http://images.wadewegner.com/wordpress/content/binary/WindowsLiveWriter/TroubleshootingwiththeSysinternalsutilit_10F4D/ProcessExplorer2%5B2%5D.gif)

  * [BlueScreen Screen Saver](http://www.microsoft.com/technet/sysinternals/Miscellaneous/BlueScreen.mspx) - Okay, so this isn't a monitoring tool, but it's part of the Sysinternals suite and it's great! Just load this up as the screen saver for your favorite person at work, sit back, and enjoy the show!

![](http://images.wadewegner.com/wordpress/content/binary/bluescreenofdeath.gif)[](http://images.wadewegner.com/wordpress/content/binary/WindowsLiveWriter/TroubleshootingwiththeSysinternalsutilit_10F4D/bsod%5B2%5D.jpg)

Take the time to explore the various tools that are available on the [Sysinternals](http://www.microsoft.com/technet/sysinternals/default.mspx) Web site. There are a lot of tools that will make debugging and problem solving less of a hassle.

Best of luck!