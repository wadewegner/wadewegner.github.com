---
author: admin
comments: true
date: 2007-03-14 14:25:06+00:00
layout: post
slug: how-to-stop-the-high-pitched-noise-emitted-by-dell-latitudes-updated
title: How to stop the high-pitched noise emitted by Dell Latitudes [Updated]
wordpress_id: 106
---

I've been trying to explain to people how the high-pitched noise emitting from my Dell Latitude D820 (which is a great computer) has been driving me crazy! Indeed, after complaining about this noise, most people actually think I have GONE crazy. But, it turns out that it's actually a known issue. Here's an article published by Dell:

[http://support.dell.com/support/topics/global.aspx/support/dsn/en/document?c=us&docid=0A7D5CD2E17F5125E0401E0A55176204&journalid=B141AFF2D12811DA8253D5412E975AB8&l=en&s=gen](http://support.dell.com/support/topics/global.aspx/support/dsn/en/document?c=us&docid=0A7D5CD2E17F5125E0401E0A55176204&journalid=B141AFF2D12811DA8253D5412E975AB8&l=en&s=gen)

Personally, I find it quite humorous that Dell blames the problem on Piezoelectricity, and even goes so far as to link to Wikipedia for more information. But, this article led me on the correct path to solving the problem.

Having read the article, I noticed that my Bluetooth device shuts down shortly after my machine boots up. Turns out, it's because a power saving checkbox was enabled. As a result, the Bluetooth device turns off, and this evidently allows the processor to move into a C3 state, thus causing the high-pitched noise.

To resolve this, go to your Device Manager, right-click "Dell Wireless 350 Bluetooth Module" (at least, that's what it's called on my machine), and select properties. Select the "Power Management" tab, and uncheck the "Allow the computer to turn off this device to save power" checkbox.

![](http://images.wadewegner.com/wordpress/content/binary/Bluetooth.gif)

Once you've done this, the noise will stop!

Best of luck!

**[Update]**

Since writing this post, Dell has come out with an updated version of the Latitude 820 Bios (version A06). You can get this update [here](http://support.dell.com/support/downloads/download.aspx?c=us&l=en&s=gen&releaseid=R152940&SystemID=Latitude%20D820&servicetag=J3D6YB1&os=WW1&osl=en&deviceid=10433&devlib=0&typecnt=0&vercnt=5&catid=1&impid=-1&formatcnt=1&libid=1&fileid=203807). I haven't tried it yet, but it's possible that this Bios update will help!

**[Update 2]**

I installed version A06 and STILL the high-pitched noise was emitted by the laptop. Fortunately, the steps above still resolve the issue.