---
title: "Commerce Server 2007 Upgrade Wizard (post SP1 install) Walkthrough"
date: 2007-06-23T00:00:00-08:00
draft: false
description: "From the whiteboard to the keyboard"
---

In my previous post, I detailed the process for [installing the Commerce Server 2007 SP1](http://www.wadewegner.com/PermaLink,guid,3e9bb1c9-4c0f-4468-af92-68ecf4db73d7.aspx). In this post, I have created a walkthrough for the Commerce Server 2007 Upgrade Wizard. The upgrade wizard should be run for all of your Commerce Server 2007 sites immediately after you have installed SP1. So, before you begin this process, be sure you have successfully installed SP1.

(Note: I created this walkthrough, along with verbose screen shots and text, largely for documentation I am delivering to clients. Please feel free to use these screen shots and/or text for your own documentation.)

A couple comments based on my experience installing SP1 and upgrading through the wizard:

- Immediately upgrade all of your Commerce Server 2007 sites. Do not reboot your machine before you run the Upgrade Wizard.
- Make sure SQL Server 2005 is running with the appropriate built-in account. For example, when I initially installed SP1 my SQL Server 2005 instance ran as “Local System”. However, post install, I had to switch the built-in account to “Network Service” for SQL Server 2005 to start-up properly. I will write a separate post about some issues I had the first time installing SP1.

That said, here’s the process for using the Upgrade Wizard to upgrade your Commerce Server 2007 sites.

1. Launch the Upgrade Wizard. You can launch this application automatically after installing SP1 or by running the Upgrade Wizard (Start -> All Programs -> Microsoft Commerce Server 2007 -> Tools -> Upgrade Wizard). Click Next to begin.

   ![CS 2007 Upgrade Wizard: Welcome](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/8f31dba0b1b5_A611/Upgrade1_1.gif)
2. On the Select Options screen, specify the log file path. I left the default location. Click Next to continue.

   ![CS 2007 Upgrade Wizard: Select Options](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/8f31dba0b1b5_A611/Upgrade2_1.gif)
3. On the Select a Commerce Site screen, specify the site you want to upgrade. Note: if you have more than one site, you can only upgrade on at a time; you can re-run the Upgrade Wizard to upgrade additional sites. Click Next to continue.

   ![CS 2007 Upgrade Wizard: Select a Commerce Site](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/8f31dba0b1b5_A611/Upgrade3_1.gif)
4. On the Upgrade Site Resources screen, you have the option to choose Migrate or Do Nothing for your various resources. Select the action Migrate for all your resources. Click Next to continue.

   ![CS 2007 Upgrade Wizard: Upgrade Site Resources](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/8f31dba0b1b5_A611/Upgrade4_1.gif)
5. The Upgrade Summary screen reviews the selections you have made. Review the selections, and click Next to start the upgrade.

   ![CS 2007 Upgrade Wizard: Upgrade Summary](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/8f31dba0b1b5_A611/Upgrade6_1.gif)

   ![CS 2007 Upgrade Wizard: Upgrade Process](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/8f31dba0b1b5_A611/Upgrade7_1.gif)
6. Once the upgrade is complete you will have to click the Next button to continue.

   ![CS 2007 Upgrade Wizard: Upgrade Process (summary)](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/8f31dba0b1b5_A611/Upgrade8_1.gif)
7. On the Summary screen, you can review the upgrade and click Main error log to review any errors. Click Next to continue.

   ![CS 2007 Upgrade Wizard: Summary](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/8f31dba0b1b5_A611/Upgrade9_1.gif)
8. You have successfully upgraded your Commerce Server 2007 site. Click Finish to complete the upgrade wizard.

   ![CS 2007 Upgrade Wizard: Complete](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/8f31dba0b1b5_A611/Upgrade10_1.gif)
9. After you click Finish you are reminded to restart. Click OK to continue.

   ![CS 2007 Upgrade Wizard: Restart notice](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/8f31dba0b1b5_A611/Upgrade11_1.gif)
10. If you have additional Commerce Server 2007 sites to upgrade, repeat steps 1-8.
11. Once you have upgraded all your Commerce Server 2007 sites, restart your computer.

The process is reasonably straightforward, and I encountered no problems.

Please be sure to share your experiences.

I hope this helps!
