---
author: admin
comments: true
date: 2007-09-16 23:50:41+00:00
layout: post
slug: commerce-server-2007-and-operational-monitoring
title: Commerce Server 2007 and Operational Monitoring
wordpress_id: 30
categories:
- Commerce Server
- MOM 2005
---

Operational monitoring (OM) is a practice that is sorely underused, in my opinion. OM is a performance and event monitoring practice that allows you to respond to errors and gain insight into machines on your network. Microsoft has two flagship products for operational monitoring: Microsoft Operations Manager (MOM) 2005 and System Center Operations Manager (SCOM) 2007. Through MOM and SCOM you can monitor your Microsoft server products, such as SQL Server, Web Sites, and even Commerce Server 2007.

There are a number of benefits to using OM, including:

  * Receive notification of critical errors and warnings
  * Automatically respond to errors and warnings
  * Gain insight into performance and trends to preempt future problems

Both MOM and SCOM use an agent (a piece of software installed on the monitored machine) to track the performance and events on monitored machines. The agent watches a number of different sources on the computer, such as the Event Log, and then centralizes this information on the OM server where it is stored in a database. The OM server applies filters and rules, and will notify individuals and/or groups if necessary. Additionally, they can respond to events by running scripts or executables on the monitored machine.

While you can define your own events and thresholds to monitor your machines, you can also take advantage of pre-built management packs. These management packs are designed by the corresponding product team (e.g. the Commerce Server 2007 MOM pack is designed by the Commerce Server team) so that the monitored events and defined thresholds are most appropriate for the product.

### Installing MOM 2005

The process for installing MOM 2005 with Service Pack 1 is pretty straightforward (especially if you're installing it on one machine). Take a look at the following documentation for detailed steps:

[http://www.microsoft.com/technet/prodtechnol/mom/mom2005/Library/b7b0c768-64d1-486e-b9ed-7292c9e545f9.mspx?mfr=true](http://www.microsoft.com/technet/prodtechnol/mom/mom2005/Library/b7b0c768-64d1-486e-b9ed-7292c9e545f9.mspx?mfr=true)

There are four steps involved in installing MOM 2005:

  1. Installing SQL Server and MOM 2005
  2. Discovering computers and deploying agents
  3. Install MOM 2005 reporting (optional)
  4. Import management packs

I suggest that, before you install MOM 2005, you take advantage of the "Check Prerequisites" tool to make sure that you've properly configured your server. 

[![Check Prerequisites for MOM 2005](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/39b63fb25a66_8E5F/image_thumb.png)](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/39b63fb25a66_8E5F/image_2.png)

If you have problems installing agents on step two, take a look at the following KB article: [ttp://support.microsoft.com/kb/883335/en-us](http://support.microsoft.com/kb/883335/en-us). Turns out I had a DNS issue on one of the machines I wanted push an agent onto; after resolving the DNS issue I had no problems. 

Once you have successfully installed and configured MOM 2005 (steps 1 through 3) you are ready to import various management packs. 

### Configuring the MOM Pack for Commerce Server 2007

You must first download the [MOM 2005 pack for Commerce Server 2007](http://www.microsoft.com/downloads/details.aspx?FamilyID=20ac6a26-02cc-4dee-95e5-39ee6dabd751&DisplayLang=en). When you run the executable you must specify the location to place the files.

[![Commerce Server 2007 MOM Pack 2005](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/39b63fb25a66_8E5F/1_thumb.gif)](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/39b63fb25a66_8E5F/1_2.gif)

I suggest you use the default location for management packs: C:Program FilesMicrosoft Operations Manager 2005Management Packs. This way, when you run the import from the MOM 2005 Administrator Console, you won't have to change the default location.

To import the management pack you must do the following:

  1. Open the **MOM 2005 Administrator Console**
  2. Browse to **Microsoft Operations Manager (<Computer Name>) **-->** Management Packs**
  3. Right-click **Management Packs** and select **Import/Export Management Pack**.
  4. Choose the Import a management pack, and click **Next**.
  5. Lave the default location for the management packs, which is where you installed the Commerce Server 2007 MOM Pack. Click **Next** to continue.  
[![Management Pack Import/Export Wizard](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/39b63fb25a66_8E5F/2_thumb.gif)](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/39b63fb25a66_8E5F/2_2.gif)
  6. Select the **Microsoft Commerce Server 2007.akm** management pack to import. Click **Next** to continue.[![Management Pack Import/Export Wizard](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/39b63fb25a66_8E5F/3_thumb.gif)](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/39b63fb25a66_8E5F/3_2.gif)
  7. On the summary page, click **Finish**.

Once you have installed the management pack you must then specify your Commerce Server 2007 servers. To do this, you must do the following.

  1. Open the **MOM 2005 Administrator Console**
  2. Browse to **Microsoft Operations Manager (<Computer Name>) **-->** Management Packs **-->** Computer Groups **-->** Microsoft Commerce Server 2007**.
  3. Right-click **Microsoft Commerce Server 2007** and choose click **Properties**.
  4. Select the **Included Computers** tab and click **Add**.
  5. Select the Commerce Server computers you wish to monitor, and click **OK**.

Now your Commerce Server 2007 servers are being monitored by MOM 2005.

I would say that the "out-of-the-box" settings for the MOM pack are okay about 90% of the time. The key is to get a baseline for your environments and potentially fine tune the settings based on your findings.

### SCOM 2007 and the 2005 MOM Pack

According to the CS product team there is no native SCOM 2007 pack planned at this time. However, the MOM 2005 pack is supposed to work with SCOM 2007. While I haven't tried this myself, I've been told by multiple sources that it should work and that this is the supported model.

If I get the opportunity to test this I'll be sure to update this post.

### Additional Resources

In addition to the Commerce Server 2007 MOM Pack you may also want to use the following MOM packs:

  * [Web Sites and Services Management Pack for MOM 2005](http://www.microsoft.com/downloads/details.aspx?FamilyID=53bc39b6-756b-4f01-b0d2-a8ca9751011f&DisplayLang=en)
  * [SQL Server Management Pack for
 MOM 2005](http://www.microsoft.com/downloads/details.aspx?FamilyID=79f151c7-4d98-4c2b-bf72-ec2b4ae69191&DisplayLang=en)

While the Commerce Server 2007 MOM Pack tracks the health of your Commerce Server environment, it doesn't really monitor the health of your Web Site (i.e. IIS). Additionally, there are events and thresholds in the SQL Server pack that are also worthwhile.

At the very least, make sure you review the following white paper: [Microsoft Commerce Server 2007 MOM Pack White Paper](http://www.microsoft.com/downloads/info.aspx?na=40&p=1&SrcDisplayLang=en&SrcCategoryId=&SrcFamilyId=20ac6a26-02cc-4dee-95e5-39ee6dabd751&u=http%3a%2f%2fgo.microsoft.com%2ffwlink%2f%3fLinkId%3d70629).

Also, here are some good blogs/sites to track:

  * [Operations Manager Product Team Blog](http://blogs.technet.com/momteam/default.aspx)
  * [OpsMgr, SCE And MOM Blog](http://blogs.technet.com/cliveeastwood/default.aspx)
  * [System Center Community](http://momcommunity.com/Default.aspx)
  * [Scripting for Microsoft Operations Management (MOM)](http://www.microsoft.com/technet/scriptcenter/hubs/mom.mspx)

MOM 2005 and SCOM 2007 are great resources and you should definitely take the time to learn more about them.

I hope this helps!
