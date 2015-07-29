---
author: admin
comments: true
date: 2007-06-14 02:40:11+00:00
layout: post
slug: add-%e2%80%9cvsnet-command-prompt-here%e2%80%9d-to-windows-explorer
title: Add “VS.NET Command Prompt Here” to Windows Explorer
wordpress_id: 76
categories:
- Scripts
- Tools
---

I was browsing one of my new favorite websites, [Microsoft's Script Repository](http://www.microsoft.com/technet/scriptcenter/scripts/default.mspx?mfr=true), when I came upon the [Add "Command Prompt Here" to Windows Explorer"](http://www.microsoft.com/technet/scriptcenter/scripts/desktop/explorer/dmexvb01.mspx) Web page. This script adds a "Command Prompt Here" command to the Windows Explorer system menu, so that if you select the command, a command window will open up in the same folder. Nifty, eh? Yes, I know, this has been around for quite awhile and is nothing new. But, with a little twist, this can become a lot more useful.

Personally, I never use "cmd.exe" by itself. I always use the "Visual Studio 2005 Command Prompt", as it has all the useful and fun PATHs already added to it. So, with a small tweak to the script, we get the following enhancement:

![](https://wadewegner.blob.core.windows.net/wordpress/content/binary/VSNETCMD.GIF)

Very handy!

Here's the script (it's so simple that I'm embarassed to share it!):

	Set objShell = CreateObject("WScript.Shell")  
	  
	objShell.RegWrite "HKCRFolderShellMenuTextCommand", _  
	"cmd.exe ""C:Program FilesMicrosoft Visual Studio 8VCvcvarsall.bat"" x86 /k cd " & chr(34) & "%1" & chr(34)  
	objShell.RegWrite "HKCRFolderShellMenuText", "VS.NET Command Prompt Here"

As I said, pretty simple, but oh so useful!

[CommandPromptHere.vbs (.29 KB)](https://wadewegner.blob.core.windows.net/wordpress/content/binary/CommandPromptHere.vbs)

I hope this helps!