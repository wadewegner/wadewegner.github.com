---
author: admin
categories:
- .NET 2.0
comments: true
date: "2007-05-04T18:35:30Z"
slug: unable-to-open-shim-database-version-registry-key-v205072700000
title: Unable to open shim database version registry key - v2.0.50727.00000.
wordpress_id: 90
---

If you've worked with .NET 2.0 long enough, you've probably come across the following error:

	The description for Event ID ( 0 ) in Source ( .NET Runtime ) cannot be found. The local computer may not have the necessary registry information or message DLL files to display messages from a remote computer. You may be able to use the /AUXSOURCE= flag to retrieve this description; see Help and Support for details. The following information is part of the event: Unable to open shim database version registry key - v2.0.50727.00000.

This behavior is caused because the ASP.NET application needs read/write access to a specific registry key but has only read access.

There is a hotfix available for this error: [http://support.microsoft.com/kb/918642](http://support.microsoft.com/kb/918642)

Note the following fine print:

_A supported hotfix is now available from Microsoft. But the hotfix is intended only to correct the problem that is described in this article. Apply this hotfix only to systems that are experiencing this specific problem. This hotfix may receive additional testing. Therefore, if you are not severely affected by this problem, we recommend that you wait for the next Visual Studio 2005 service pack that contains this hotfix._

This error is more of an annoyance because it fills up the application log. It shouldn't actually affect any of your applications. If you don't mind ignoring the error, then just ignore it and wait until the next service pack resolves the problem.

Best of luck!