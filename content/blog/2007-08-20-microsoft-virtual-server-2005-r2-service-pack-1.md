---
author: admin
categories:
- Virtual Server 2005
- Windows Vista
comments: true
date: "2007-08-20T05:46:13Z"
slug: microsoft-virtual-server-2005-r2-service-pack-1
title: Microsoft Virtual Server 2005 R2 Service Pack 1
wordpress_id: 38
---

Somehow I missed the announcement that SP1 was released for Virtual Server 2005 R2. Given that I run nearly all my development (and some non-development) environments on R2, I'm surprised I didn't catch wind of SP1!

You can download SP1 here: [http://www.microsoft.com/downloads/details.aspx?FamilyId=BC49C7C8-4840-4E67-8DC4-1E6E218ACCE4&displaylang=en](http://www.microsoft.com/downloads/details.aspx?FamilyId=BC49C7C8-4840-4E67-8DC4-1E6E218ACCE4&displaylang=en). It's interesting to note that Vista flavors are listed as supported host operating systems, yet include a "non-Production only" caveat.

I run Virtual Server 2005 R2 on Vista Business without any problems (well, if you [know some tricks](http://www.wadewegner.com/2007/03/03/InstallingVirtualServer2005R2OnWindowsVistaUpdated.aspx)). As I upgraded I noted the following:

  * When installing SP1, be sure to turn off the Virtual Server service, else you will receive the following warning:

  ![warning](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/MicrosoftVirtualServer2005R2ServicePack1_14E2A/warning_thumb.jpg)

  * It will detect the previous version and force you to upgrade:

  ![image](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/MicrosoftVirtualServer2005R2ServicePack1_14E2A/image_thumb_1.png)

  * I could be wrong, but I'm pretty sure that some of these options are new. Sorry, I haven't had the time to research them - when I do, I promise I'll post my findings.

  ![image](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/MicrosoftVirtualServer2005R2ServicePack1_14E2A/image_thumb_2.png)
  
The upgrade seemed to go without a problem. I was able to mount and start my virtual machines without any problems whatsoever.
