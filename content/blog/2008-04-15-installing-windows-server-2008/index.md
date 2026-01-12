---
title: "Installing Windows Server 2008"
date: 2008-04-15T00:00:00-08:00
draft: false
description: "From the whiteboard to the keyboard"
---

I have long been a proponent of working and developing in an environment that matches your production systems. I’ve found that developing on the same O/S takes away a lot of the unknowns and guess work that occurs when you deploy from a traditional workstation O/S (like XP or Vista) to a server O/S (like Windows Server 2003 or 2008). Don’t get me wrong, Windows XP and Vista are fantastic O/S’s and appropriate for all kinds of development; yet, when you’re working with products like BizTalk, SharePoint, or Commerce Server, it make sense to use the same server O/S.

Windows Server 2003 has been one of my favorite O/S’s. It’s stable, fast, and powerful. I’ve built all kinds of custom .NET, SharePoint, BizTalk, and Commerce Server applications on Windows Server 2003 and I have never been unhappy or displeased with the O/S. So, if I’ve always been happy with Windows Server 2003, why am I talking about Windows Server 2008?

This little post is probably the wrong place to get into a full discussion regarding the features and benefits of Windows Server; however, let me mention a few things that convinced me:

- [IIS 7](http://learn.iis.net/). There have been some tremendous changes to IIS in this latest version. I’ll post about this another time; there’s too much for this post.
- [Hyper-V](http://www.microsoft.com/windowsserver2008/en/us/virtualization-consolidation.aspx). Hyper-V is a virtualization system for x64 versions of Windows Server 2008. It’s cool stuff, and again too much to get into here. Never heard about this? Go read about it!
- [x64](http://www.microsoft.com/servers/64bit/overview.mspx). All I can say is that I was always unhappy with the x64 version of Windows Server 2003.
- Roles and Features. I’ll talk more about these in another post; roles and features are similar to what we’ve used in previous versions, only better!

There are many more reasons; this published list of the [Top 10 Reasons to upgrade to Windows Server 2008](http://www.microsoft.com/windowsserver2008/en/us/why-upgrade.aspx) is a great start.

Alright, convinced? Well, if for some reason you’re still not convinced then I invite you to witness how slick and easy the installation process is with Windows Server 2008. Actually, I found that it’s very much like the Windows Vista installation.

1. Insert your bootable DVD or map the ISO file to your virtual machine. Boot the machine.

![Windows Server 2008 Installation (1)](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/InstallingWindowsServer2008_12C45/Windows%20Server%202008%20Installation%20(1)_1.jpg)

2. Once the lightweight O/S has booted, click **Install Now**.

![Windows Server 2008 Installation (2)](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/InstallingWindowsServer2008_12C45/Windows%20Server%202008%20Installation%20(2)_2.jpg)

3. Depending on the flavor of your DVD / ISO, you will have various options to select from. Select the operating system you want to install, and click **Next**.

![Windows Server 2008 Installation (3)](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/InstallingWindowsServer2008_12C45/Windows%20Server%202008%20Installation%20(3)_1.jpg)

4. You are next presented with the license terms. Be sure and read these terms! Once you have finished, select **I accept these terms**, and click\*\* Next\*\*.

![Windows Server 2008 Installation (4)](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/InstallingWindowsServer2008_12C45/Windows%20Server%202008%20Installation%20(4)_1.jpg)

5. You must next select the type of installation. Again, depending on your flavor, you may have different options. I always prefer to perform a fresh installation. That’s just how I role. Make your selection, and click **Next**.

![Windows Server 2008 Installation (5)](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/InstallingWindowsServer2008_12C45/Windows%20Server%202008%20Installation%20(5)_1.jpg)

6. Next, you have to select the disk partition to which the operating system is installed. Make your selection, and click **Next**.

![Windows Server 2008 Installation (6)](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/InstallingWindowsServer2008_12C45/Windows%20Server%202008%20Installation%20(6)_1.jpg)

7. Now comes the impressive part. The installation for Windows Server 2008 is very much like Windows Vista - fast! That’s because the operating system is largely unpacked rather than installed. On the first screen you’ll notice that the zip is copied over to the disk partition …

![Windows Server 2008 Installation (7)](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/InstallingWindowsServer2008_12C45/Windows%20Server%202008%20Installation%20(7)_2.jpg)

8. … and then it is expanded.

![Windows Server 2008 Installation (8)](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/InstallingWindowsServer2008_12C45/Windows%20Server%202008%20Installation%20(8)_1.jpg)

9. Oops. It went so fast I missed a screen shot. Honestly, **Installing Updates** is typically the slowest
   part of the installation, although this is more true for Vista than Server 2008 as Vista has more updates to install at this point in time. (No, this is not a reflection or comment on the quality of Vista!)

![Windows Server 2008 Installation (9)](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/InstallingWindowsServer2008_12C45/Windows%20Server%202008%20Installation%20(9)_1.jpg)

10. At this point I was asked to reboot. Just do what it tells you to do. It’s smarter than we are.

![Windows Server 2008 Installation (10)](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/InstallingWindowsServer2008_12C45/Windows%20Server%202008%20Installation%20(10)_1.jpg)

11. And then the installation completes. Yippee!

![Windows Server 2008 Installation (11)](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/InstallingWindowsServer2008_12C45/Windows%20Server%202008%20Installation%20(11)_1.jpg)

12. Before you can log into your new fresh installation of Windows Server 2008, you are told to change the administrator password …

![Windows Server 2008 Installation (12)](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/InstallingWindowsServer2008_12C45/Windows%20Server%202008%20Installation%20(12).jpg)

13. … and then you can set a new password. It struck me odd that this was considered “changing” the password, since there doesn’t appear to have been one before. Oh well. Semantics. This installation still rocks. Click the little arrow, and then …

![Windows Server 2008 Installation (13)](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/InstallingWindowsServer2008_12C45/Windows%20Server%202008%20Installation%20(13)_1.jpg)

14. … your desktop is prepared!

Congratulations, you’ve just installed Windows Server 2008! Altogether, I was able to go through this process in about 30 minutes on my Dell D830. Not too shabby! In the effort of full disclosure, here are the specifications for my machine:

> Processor: Intel Core 2 Duo, 2.40 GHz
> Memory (RAM): 4.00 GB
> System type: 64-bit Operating System

Really, when you sit back and reflect on what operating system installations were like back in the old days (the 90’s were the old days, right?) it’s amazing how much more sophisticated this process has become. It’s elegant, intuitive, and fast.

While you may not be as impressed with this as I am (in which case you must try it on your own, as I know you’ll become as fervent as I am) I promise you that some of my next posts will get your blood flowing. I plan to show how easy it is to add roles (e.g. a Web Server with IIS) and features (e.g. .NET Frameworks or PowerShell) to Windows Server 2008. Neat stuff, and again, very intuitive.

I hope this helps!
