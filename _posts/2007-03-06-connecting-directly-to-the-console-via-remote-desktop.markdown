---
author: admin
comments: true
date: 2007-03-06 00:28:59+00:00
layout: post
slug: connecting-directly-to-the-console-via-remote-desktop
title: Connecting Directly to the Console via Remote Desktop
wordpress_id: 111
categories:
- Virtual Server 2005
---

Many applications require (or should have) direct access to the Windows Console, rather than a session on the the Windows Server. I encounter these kind of problems often, as I do all my development on virtual machines (running on Virtual Server 2005 R2) and use Remote Desktop to connect.

In order to accomplish this, call the Remote Desktop application with the following command:

	mstsc /console

This will attach the Remote Desktop application directly to the system console, preventing any issues that can result from attaching to a session.

If you use a short-cut to connect to Remote Desktop, rather than the command line, just choose the short-cut's link, and add "/console" to the target.

Best of luck!