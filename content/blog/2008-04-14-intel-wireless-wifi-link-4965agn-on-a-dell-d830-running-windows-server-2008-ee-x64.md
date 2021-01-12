---
author: admin
categories:
- Windows Server 2008
- x64
comments: true
date: "2008-04-14T19:56:17Z"
slug: intel-wireless-wifi-link-4965agn-on-a-dell-d830-running-windows-server-2008-ee-x64
title: Intel Wireless WiFi Link 4965AGN on a Dell D830 running Windows Server 2008
  EE x64
wordpress_id: 13
---

Today I decided to install the x64 version of Windows Server 2008 EE w/ Hyper-V on my Dell D830 laptop. I will post about the experience later (it was awesome!); for now, I want to specifically mention how I was able to get the wireless working.

 

I should have done my due diligence prior to installing Windows Server 2008, but I like to live dangerously! After the installation was complete (which was simple and fast), I noticed that the wireless adapter was not installed. I tried to update the driver manually, specifically telling it to check the web, but the search didn't find anything. I also checked both the Intel and Dell web sites to no available--I couldn't find anything for Windows Server 2008 and my wireless adapter on either web site (which is not all that surprising, since I doubt many people try to install a server O/S on their laptop).

 

Discouraged, I did a quick search and found a post from my colleague [Keith Combs](http://blogs.technet.com/keithcombs/default.aspx) discussing his experience [installing Windows Server 2008 EE on his Lenovo laptop](http://blogs.technet.com/keithcombs/archive/2008/02/01/installing-windows-server-2008-ee-on-a-lenovo-thinkpad-t61p.aspx). Amazingly, Lenovo laptops have the same Intel wireless adapter as the Dell; furthermore, Lenovo has also published the [device drivers](http://www-307.ibm.com/pc/support/site.wss/document.do?sitestyle=lenovo&lndocid=MIGR-67256)!

 

To make a long story short (too late, I know), I was able to download and install the Lenovo drivers on my Dell. Thus far (it's been about two hours) everything seems to be working perfectly!

 

Gotta love OEM!

 

Hopefully this saves a poor soul from a couple hours of digging. Good luck!
