---
author: admin
comments: true
date: 2012-09-21 22:02:42+00:00
layout: post
slug: getting-the-application-id-and-hardware-id-in-windows-store-applications
title: Getting the Application ID and Hardware ID in Windows Store Applications
wordpress_id: 1875
categories:
- Windows 8
- Windows Store
tags:
- Windows 8
- Windows Store
---

So far I’ve really enjoyed developing applications for Windows 8. I still can’t claim to be particularly good at XAML design work yet, but I’m getting the hang of Windows Store Apps in C# using Windows RT. That said, there are have been a number of times when I’ve been lost and had to hit MSDN and search engines in order to figure things out. What makes this particularly challenging is that much of the information you’ll find is old and not appropriate for Windows 8 RTM. Consequently, I thought I’d share with you at least one thing I learned today.

Today I wanted to do two things.

  1. Get an ID specific to the application.
  2. Get an ID specific to the device/hardware (also called App Specific Hardware ID or ASHWID).

There are a lot of different reasons for wanting this information. For me, I’m storing this information in a Windows Azure table along with a [Windows Notification Service (WNS)](http://msdn.microsoft.com/en-us/library/windows/apps/hh913756.aspx) Channel URI so that I can choose the right application and device to send notifications.

While trying to figure out how to get the Application ID wasn’t particularly difficult – that is, if you find the RTM documentation – I did struggle to figure out how to get the device/hardware ID. Finally, I found an [answer on Stack Overflow](http://stackoverflow.com/questions/12528186/how-do-i-get-a-unique-identifier-for-a-machine-running-windows-8-in-c) that helped.

Here’s how to get the application ID with C#:
    
    <span style="background: white; color: blue">string </span><span style="background: white; color: black">appId = </span><span style="background: white; color: #2b91af">CurrentApp</span><span style="background: white; color: black">.AppId.ToString();</span>




Note that during development the GUID comes back as "00000000-0000-0000-0000-000000000000". Once released through the Windows Store you will get a specific value.




Getting the ASHWID is a bit more difficult. Prior to the RTM release a lot of folks created their own GUID and stored it in the Windows.Storage.ApplicationData.Current.LocalSettings. This is a reasonable hack but of course the user can delete local storage and then your application would change the value – not good if you’re depending on something unique.




Fortunately the RTM release includes the GetPackageSpecificToken class that can return the ASHWID. Of course, I looked at some [guidance](http://msdn.microsoft.com/en-us/library/windows/apps/jj553431) and [MSDN method documentation](http://msdn.microsoft.com/en-us/library/windows/apps/windows.system.profile.hardwareidentification.getpackagespecifictoken) on getting the ASHWID and never found a good sample on how to get the ASHWID and store it as a string. Consequently, I hope this short snippet – again, found on Stack Overflow, helps:
    
    <span style="background: white; color: blue">private string </span><span style="background: white; color: black">GetHardwareId()
    {
        </span><span style="background: white; color: blue">var </span><span style="background: white; color: black">token = </span><span style="background: white; color: #2b91af">HardwareIdentification</span><span style="background: white; color: black">.GetPackageSpecificToken(</span><span style="background: white; color: blue">null</span><span style="background: white; color: black">);
        </span><span style="background: white; color: blue">var </span><span style="background: white; color: black">hardwareId = token.Id;
        </span><span style="background: white; color: blue">var </span><span style="background: white; color: black">dataReader = Windows.Storage.Streams.</span><span style="background: white; color: #2b91af">DataReader</span><span style="background: white; color: black">.FromBuffer(hardwareId);
    
        </span><span style="background: white; color: blue">byte</span><span style="background: white; color: black">[] bytes = </span><span style="background: white; color: blue">new byte</span><span style="background: white; color: black">[hardwareId.Length];
        dataReader.ReadBytes(bytes);
    
        </span><span style="background: white; color: blue">return </span><span style="background: white; color: #2b91af">BitConverter</span><span style="background: white; color: black">.ToString(bytes);
    }  </span>




From here you can just load it into a string and do whatever you desire.
    
    <span style="background: white; color: blue">string </span><span style="background: white; color: black">deviceId = GetHardwareId(); </span>




Now you’ll get a value that’s something like 03-00-F0-7E-03-00-76-F3-05-00-5C-54-05-00-8A-DE-06-00-01-00-04-00-54-49-04-00-C2-4A-04-00-DE-4D-01-00-A4-52-02-00-2E-B2-09-00-42-88 that you can store for future use.




Nothing groundbreaking here but hopefully this saves you a few minutes.
