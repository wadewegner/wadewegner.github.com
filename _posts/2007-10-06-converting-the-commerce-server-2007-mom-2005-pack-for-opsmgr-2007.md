---
author: admin
comments: true
date: 2007-10-06 21:55:00+00:00
layout: post
slug: converting-the-commerce-server-2007-mom-2005-pack-for-opsmgr-2007
title: Converting the Commerce Server 2007 MOM 2005 Pack for OpsMgr 2007
wordpress_id: 28
categories:
- Commerce Server
- MOM 2005
- OpsMgr 2007
---

_Note: For those of you that want to skip the explanation, and simply get a converted management pack for Commerce Server 2007, scroll down to download a MOM 2005 pack converted for OpsMgr 2007 for Commerce Server 2007._

I previously blogged about [Commerce Server 2007 and Operational Monitoring](http://www.wadewegner.com/2007/09/16/CommerceServer2007AndOperationalMonitoring.aspx), and indicated that there is no native OpsMgr 2007 pack for Commerce Server 2007 (although there will be one some day). For now, we have to convert the MOM 2005 pack. Fortunately, this is a pretty straightforward process, although there is the potential for things to to fail (learn from my experience!).

In order to convert management packs from MOM 2005 to OpsMgr 2007, you must have the following installed on your server:

  * **Operations Manager 2007 **(hopefully this is obvious)
  * The **MOM 2005 User Interfaces** (found on your MOM 2005 disk)
  * The **MOM 2005 to OpsMgr 2007 Migration tool** (found on your OpsMgr 2007 disk)

Now, before you go and start installing all these bits, let me share my experience. You **should** install the **MOM 2005 User Interfaces** first, followed by **Operations Manager 2007**, and lastly the **MOM 2005 to OpsMgr 2007 Migration tool**. Here's why -- when I originally attempted to convert convert the 2005 pack I started by installing OpsMgr first and then tried to install the migration tool. The installer told me that in order to install the migration tool I first needed the MOM 2005 UI. Fair enough, I thought, so I went to install the UI tool. However, every time the installer went to "Check Prerequisites" I got the following error:

[![Please verify that the CD or network share is available.](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/ConvertingtheCommerceServer2007MOMPackto_C74C/MOM2005CDError_thumb.gif)](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/ConvertingtheCommerceServer2007MOMPackto_C74C/MOM2005CDError_2.gif)

No matter what I did, I got this error (although this error never occurred on any other machine that didn't have OpsMgr 2007 installed). I couldn't find any information on this error (evidently I'm the only person to experience this problem), but I think it's some kind of installation failure because OpsMgr 2007 was already installed. I went to test this theory by trying to uninstall OpsMgr 2007, but then I got a "Fatal error during installation" error when trying to uninstall. Ugly!

Fortunately, this was a virtual machine so I scrapped it and started over. This time I installed the bits in the following order:

  1. The **MOM 2005 User Interfaces**
  2. **Operations Manager 2007 **
  3. The **MOM 2005 to OpsMgr 2007 Migration tool**

This worked perfectly for me, and I never experience the above error.

Once everything is installed, you can begin the conversion process. Open **Start** --> **All Programs** --> **System Center Operations Manager 2007** -->** Migration Tool**. The migration tool is a wizard that will walk you through the process, and is very easy. Simply point to the extracted 2005 management pack file (which you can [download here](http://www.microsoft.com/downloads/details.aspx?FamilyID=20ac6a26-02cc-4dee-95e5-39ee6dabd751&DisplayLang=en)) and either migrate it directly to your OpsMgr group or to a file.

If you have chosen to migrate it to file (like I did), you must then import it into OgsMgr 2007. Open **Start** --> **All Programs** --> **System Center Operations Manager 2007** -->** Operations Console**, choose the **Administration** tab, right-click **Device Management**, select **Import Management Packs**, and point to your converted MOM pack. It's as easy as that!

Now when you go to the Monitoring tab, you'll see the following:

[![Commerce Server 2007 Monitoring](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/ConvertingtheCommerceServer2007MOMPackto_C74C/CSMonitoring_thumb.gif)](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/ConvertingtheCommerceServer2007MOMPackto_C74C/CSMonitoring_2.gif)

All you need to do now is provision the agents on your Commerce Server 2007 machine, and away you go! Very simple, and very useful.

I hope this helps!

[Microsoft_Commerce_Server_2007.XML (2.18 MB)](https://wadewegner.blob.core.windows.net/wordpress/content/binary/Microsoft_Commerce_Server_2007.XML)
