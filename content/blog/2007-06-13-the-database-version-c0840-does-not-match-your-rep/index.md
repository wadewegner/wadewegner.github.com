---
title: "The database version (C.0.8.40) does not match your reporting services installation."
date: 2007-06-13T00:00:00-08:00
draft: false
description: "From the whiteboard to the keyboard"
---

As I was preparing a new virtual machine for Commerce Server 2007 (one capable of utilizing the Commerce Server Adapters with BizTalk Server 2006 and Data Warehousing), I ran into a problem when configuring SQL Server Reporting Services.

I had taken a virtual machine that I use as my base installation (e.g. Windows Server 2003 R2, IIS, .NET 2.0, Visual Studio 2005, and the SQL Server 2005 Database Engine) and was following the Commerce Server 2007 [Pre-install Requirements and Procedures](http://download.microsoft.com/download/3/a/4/3a46267f-9640-4623-86a1-c63bbfea9e1d/Installation%20and%20Configuration%20Guide%20for%20Microsoft%20Commerce%20Server%202007.html#Prepare_Install) from the [Installation Guide](http://go.microsoft.com/fwlink/?LinkID=57268) when I encoutered the following error:

```
The database version (C.0.8.40) does not match your reporting services installation. You must upgrade your reporting services database.
Couldn't generate the upgrade script. There is no upgrade script available for this version.
```

Turns out that that I had installed Service Pack 2 after I originally installed the Database Engine but before I installed Reporting Services. Consequently, when I went to configure a reporting database under the Database Setup step it complained that the versions were incorrect.

To resolve the problem, I simply ran the [Microsoft SQL Server 2005 Service Pack 2](http://www.microsoft.com/downloads/details.aspx?FamilyId=d07219b2-1e23-49c8-8f0c-63fa18f26d3a&DisplayLang=en) installation again, and updated the Reporting Services instance. This allowed me to finish the configuration of SQL Reporting Services without any problems.

A simple little problem, but one that had me baffled for a little while (it’s probably just the sleep deprivation caused by [the little one](http://www.wadewegner.com/PermaLink,guid,7b293d04-a61d-4baa-a224-0f24639f4ecc.aspx)).

I hope this helps!
