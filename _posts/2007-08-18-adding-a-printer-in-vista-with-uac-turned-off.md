---
author: admin
comments: true
date: 2007-08-18 16:24:29+00:00
layout: post
slug: adding-a-printer-in-vista-with-uac-turned-off
title: Adding a printer in Vista with UAC turned off
wordpress_id: 43
categories:
- Rant
- Windows Vista
---

One of the first things I do after installing Vista is turn off UAC (ooh, I can already feel the criticism). I just can abide having to authorize every single action I take on the computer. It's one thing to ask me to confirm a change to IIS or a service running on my machine, but don't make me authorize the deletion of files on the operating system! If I clicked delete and confirmed the deletion, just delete it!

Previously, if you wanted to add a network printer that ran on Windows XP or 2003, you had to have UAC turned on. This means that I would have to turn on UAC, reboot my machine, add the printer, turn off UAC, and then reboot my machine again. Just to add a printer. Brilliant!

Fortunately Microsoft just released two updates:

  * [Windows Vista Performance Update](http://support.microsoft.com/?kbid=938979)
  * [Windows Vista Reliability Update](http://support.microsoft.com/?kbid=938194)

The first of the two specifically addresses the following "issue":

> _If User Account Control is disabled on the computer, you cannot install a network printer successfully. This problem occurs if the network printer is hosted by a Windows XP-based or a Windows Server 2003-based computer._

Finally!

To test this, I installed both updates (rebooting after each on), and then attempted to add a network printer. Previously, clicking the Install driver button caused failed if UAC was turned off.

[![Adding a network printer in Vista](http://images.wadewegner.com/wordpress/content/binary/WindowsLiveWriter/AddingaprinterinVistawithUACturnedoff_8EAB/printer1_thumb.jpg)](http://images.wadewegner.com/wordpress/content/binary/WindowsLiveWriter/AddingaprinterinVistawithUACturnedoff_8EAB/printer1.jpg)

After installing the updates, however, it works exactly as one would expect it to work.

It only took six or seven months, but it seems this annoying "issue" was finally resolved. Thank goodness!
