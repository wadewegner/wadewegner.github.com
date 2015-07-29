---
author: admin
comments: true
date: 2007-05-28 03:34:04+00:00
layout: post
slug: oracle-for-microsoft-developers-part-i-installing-oracle-92-on-windows-server-2003
title: 'Oracle for Microsoft Developers, Part I: Installing Oracle 9.2 on Windows
  Server 2003'
wordpress_id: 83
categories:
- Oracle
- Windows Server 2003
---

There comes a time in every Microsoft developers life that he/she will have to work with an Oracle database. I hope that you find this to be a good experience; my experiences have thus far been a mixed bag, mostly because of my own ignorance. Nevertheless, I've picked up a few things here and there, and figured that it would be worthwhile to post some of the top tips and tricks I've learned over the years.

In this post, I want to discuss how to setup an Oracle database server. I am going to follow-up with a post on how to create an Oracle database. I use those terms loosely, because the terminology within the Oracle world is different from the SQL Server world. I promise I'll do my best to get it right, but chances are I'll make a mistake or two - please feel free to point out my gaffes.

So, without further ado, let's see go through the steps needed to install Oracle 9.2 (sorry, I don't have 10+) on Windows Server 2003 32-bit (note: there are many differences between 32-bit and 64-bit Windows, and my experience has shown that the following steps will not work in 64-bit Windows. Perhaps I'll follow-up in the future if I figure out how to get it to work ...)

1. Secure a copy of Oracle 9.2. I assume that you have a copy available through work.

2. Start the Oracle Universal Installer. Click the Next button.  

  ![](https://wadewegner.blob.core.windows.net/wordpress/content/binary/01.gif)  

3. Confirm the installation destination (you'll need at least 3 GBs available for the installation) and click Next.  

  ![](https://wadewegner.blob.core.windows.net/wordpress/content/binary/02.gif)  

4. Select "Oracle9i Database 9.2.0.1.0", and click Next.  

  ![](https://wadewegner.blob.core.windows.net/wordpress/content/binary/03.gif)  

5. Select "Enterprise Edition", and click Next. (Why? Well, I don't have any compelling reason here - it's just what I've used to get things working ...)  

  ![](https://wadewegner.blob.core.windows.net/wordpress/content/binary/04.gif)  

6. Select "General Purpose", and click Next.  

  ![](https://wadewegner.blob.core.windows.net/wordpress/content/binary/05.gif)  

7. Leave the Port Number as 2030, and click Next.  

  ![](https://wadewegner.blob.core.windows.net/wordpress/content/binary/06.gif)  

8. Define the Global Database Name. I choose "orcl", but feel free to choose whatever. Make sure that the SID is unique - it's probably easiest to make it the same value as the Global Database Name. Click Next.  

  ![](https://wadewegner.blob.core.windows.net/wordpress/content/binary/07.gif)  

9. Specify the Directory for Database Files. I choose to leave the default value. Click Next.  

  ![](https://wadewegner.blob.core.windows.net/wordpress/content/binary/08.gif)  

10. Unless you have a reason to change it, leave the "default character set" selected, and click Next.  

  ![](https://wadewegner.blob.core.windows.net/wordpress/content/binary/09.gif)  

11. On the Summary screen, click Install. This installation process will take a little while. Be patience. Read one of my other posts, while you're waiting.  

  ![](https://wadewegner.blob.core.windows.net/wordpress/content/binary/10.gif)  

12. The next screen to appear is the Database Configuration Assistance. On this screen, specify the SYS and SYSTEM passwords. Do NOT click OK yet.  

  ![](https://wadewegner.blob.core.windows.net/wordpress/content/binary/11.gif)  

13. Before you click OK, click the Password Management Button. Specify passwords for SYS, SYSTEM, and DBSNMP (SCOTT too, if you'd like).  

  ![](https://wadewegner.blob.core.windows.net/wordpress/content/binary/12.gif)  

14. Once you get to the End of Installation, click Exit.  

  ![](https://wadewegner.blob.core.windows.net/wordpress/content/binary/13.gif)  

15. After you click Exit, the Oracle Enterprise Management Console will start. Check and make sure you can log into your Oracle database server by expanding databases, right-click ORCL (or whatever you called it) and click Connect. Enter the "system" Username, and change Connect as from "Normal" to "SYSDBA". Click OK.  

  ![](https://wadewegner.blob.core.windows.net/wordpress/content/binary/14.gif)  

16. If you were able to successfully connect to the Oracle database server, then restart Windows. There's a number of reasons to do this, including updates to the PATH environment variable.

And that's it!

Now, I realize that this installation procedure pretty much accepts the defaults, and clicks next. However, at least you have the benefit of knowing that this procedure does work, and will prepare you for the next step: configuring Oracle databases, tablespaces, users, and user access.

Best of luck!