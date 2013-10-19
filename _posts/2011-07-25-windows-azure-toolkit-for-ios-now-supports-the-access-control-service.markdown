---
author: admin
comments: true
date: 2011-07-25 17:29:51+00:00
layout: post
slug: windows-azure-toolkit-for-ios-now-supports-the-access-control-service
title: Windows Azure Toolkit for iOS Now Supports the Access Control Service
wordpress_id: 1280
categories:
- Access Control Service
- iOS
- Toolkit
- Windows Azure
---

Today we released an update to our [Windows Azure Toolkit for iOS](https://github.com/microsoft-dpe/watoolkitios-lib) that provides some significant enhancements – in particular, we now provide support for using the [Windows Azure Access Control Server (ACS)](http://acs.codeplex.com/) from an iOS application. You can get all the bits here:

 

  
  * (updated) [watoolkitios-lib](https://github.com/microsoft-dpe/watoolkitios-lib)
   
  * (updated) [watoolkitios-samples](https://github.com/microsoft-dpe/watoolkitios-samples)
   
  * (updated) [watoolkitios-doc](https://github.com/microsoft-dpe/watoolkitios-doc)
   
  * (new) [watoolkitios-configutility](https://github.com/microsoft-dpe/watoolkitios-configutility)
   
  * (new) [cloudreadypackages](https://github.com/microsoft-dpe/cloudreadypackages)
 

We first released this toolkit on May 6th, and since then we’ve released two minor updates and even accepted a [merge request from the community](http://www.wadewegner.com/2011/05/released-windows-azure-toolkit-for-ios-v1-0-1/). This toolkit has been a real pleasure to work on. Not only has it been to break out of the traditional Microsoft stack and learn about new languages and environments, but it’s also been great to introduce a lot of Objective-C and iOS developers to the power of Windows Azure.

 

![ACS & iOS/iPhone](http://images.wadewegner.com/wordpress/2011/08/HomeRealm.png)

 

There are three key aspects to version 1.2 of the iOS toolkit:

 

  
  1. Cloud Ready Packages for Devices 
   
  2. Configuration Tool 
   
  3. Support for ACS 
 

These three pieces are incredibly important when trying to develop iOS applications that use Windows Azure; consequently, let me try and explain each of these components and how they help to make development easier.

 

**_Cloud Ready Packages for Devices_**

 

One of the biggest challenges when using Windows Azure for an iOS developer today is the inability to create a package that can be deployed to Windows Azure. To make this easier, we have pre-built four Cloud Ready Packages for Devices so that you – the iOS developer – don’t have to setup Windows 7 and run CSPACK. Instead, you simply have to download the most appropriate cloud ready package, update the .CSCFG file, then deploy through the Windows Azure Portal.

 

We have four “flavors” of the Cloud Ready Packages:

 

  
  * ACS + APNS – this version allows you to use the Access Control Service and register your certificate for the Apple Push Notification Service 
   
  * ACS – this version allows you to use the Access Control Service 
   
  * Membership + APNS – this version allows you to use a simple membership store in Windows Azure table storage for users and register your certificate for the Apple Push Notification Service 
   
  * Membership – this version allows you to use a simple membership store in Windows Azure table storage for users 
 

For more information on how to use and deploy these packages, take a look at this video on [deploying the Cloud Ready Packages for Devices](http://channel9.msdn.com/posts/Deploying-the-Cloud-Ready-Packages-for-Devices).

 

**_Configuration Tool_**

 

Along with the CSPKG you need a CSCFG to deploy your application to Windows Azure. The CSCFG file is an xml document that helps to describe elements of your application to Windows Azure so that it is able to correctly run your application.

 

In Visual Studio we have tools that make it easy to update the CSCFG file without having to open up the XML, but of course you cannot do this on a Mac. To make this easier, we created a tool that you can use on the Mac to walkthrough and generate the CSCFG file with all the appropriate details. Once created, you can use this CSCFG file along with the downloaded CSPKG file to deploy your application.

 

[![iosconig](http://images.wadewegner.com/wordpress/2011/07/iosconig_thumb.png)](http://images.wadewegner.com/wordpress/2011/07/iosconig.png)

 

In addition to creating the CSCFG file, the configuration tool will also updated ACS with all the appropriate settings so that you can build & run your application quickly. For all the details, please take a look at Vittorio Bertocci’s post on [Using the Windows Azure Access Control Service in iOS Applications](http://blogs.msdn.com/b/vbertocci/archive/2011/07/25/using-the-windows-azure-access-control-service-in-ios-applications.aspx).

 

**_Support for ACS_**

 

Everything I’ve described above is designed to make it easier for an iOS developer to quickly and easily use the Access Control Service. To use the library for authenticating to ACS, it’s really quite simple:

 

>   
>     
>     <span style="color: #2e0d6e">NSLog</span><span style="color: black">(</span><span style="color: #c41a16">@"Intializing the Access Control Client..."</span><span style="color: black">);
>     </span><span style="color: #3f6e74">WACloudAccessControlClient </span><span style="color: black">*acsClient = [</span><span style="color: #3f6e74">WACloudAccessControlClient </span><span style="color: #26474b">accessControlClientForNamespace</span><span style="color: black">:</span><span style="color: #c41a16">@"iostest-walkthrough" </span><span style="color: #26474b">realm</span><span style="color: black">:</span><span style="color: #c41a16">@"uri:wazmobiletoolkit"</span><span style="color: black">];
>     
>     [acsClient </span><span style="color: #26474b">showInViewController</span><span style="color: black">:</span><span style="color: #aa0d91">self</span><span style="color: black">.</span><span style="color: #3f6e74">viewController </span><span style="color: #26474b">allowsClose</span><span style="color: black">:</span><span style="color: #aa0d91">NO </span><span style="color: #26474b">withCompletionHandler</span><span style="color: black">:^(</span><span style="color: #aa0d91">BOOL </span><span style="color: black">authenticated) {
>         </span><span style="color: #aa0d91">if </span><span style="color: black">(!authenticated)
>         {
>              </span><span style="color: #2e0d6e">NSLog</span><span style="color: black">(</span><span style="color: #c41a16">@"Error authenticating"</span><span style="color: black">);
>         }
>         </span><span style="color: #aa0d91">else
>         </span><span style="color: black">{
>              </span><span style="color: #2e0d6e">NSLog</span><span style="color: black">(</span><span style="color: #c41a16">@"Creating the authentication token..."</span><span style="color: black">);
>              </span><span style="color: #3f6e74">WACloudAccessToken </span><span style="color: black">*token = [</span><span style="color: #3f6e74">WACloudAccessControlClient </span><span style="color: #26474b">sharedToken</span><span style="color: black">];
>              </span><span style="color: #007400">/* Do something with the token here! */
>         </span><span style="color: black">}
>     }];</span>
> 
> 






I’ll post more walkthroughs and documentation shortly.





As always, please let me know what you think of the release! Your feedback is important to us, especially as it pertains to prioritizing future features and capabilities.
