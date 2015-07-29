---
author: admin
comments: true
date: 2011-08-31 15:29:19+00:00
layout: post
slug: windows-azure-toolkits-for-devices-now-with-android
title: Windows Azure Toolkits for Devices - Now With Android!
wordpress_id: 1486
categories:
- Android
- Eclipse
- iOS
- Objective-C
- Toolkit
- Windows Azure
- Windows Phone
- Xcode
---

I am **_tremendously _**pleased to share that today we have released the [Windows Azure Toolkit for Android](https://github.com/microsoft-dpe/wa-toolkit-android)! We announced [our intentions to build a toolkit for Android](http://blogs.technet.com/b/microsoft_blog/archive/2011/05/09/microsoft-announces-windows-azure-toolkits-for-ios-android-and-windows-phone.aspx) back in May, and it had always been our intention to release this summer (we only missed by a week or so).

 

In addition to this release of Android, we have also:

 

  
  * Released the [Windows Azure Toolkit for Windows Phone (v1.3.0)](http://watwp.codeplex.com/)
   
  * Released the [Windows Azure Toolkit for iOS (v1.2.1)](https://github.com/microsoft-dpe/wa-toolkit-ios)
   
  * Released a new Windows Phone sample called BabelCam both as source code and [to the Windows Phone Marketplace](http://windowsphone.com/s?appid=576327f5-785c-4997-bc01-dc9fe4423f88)
 

These releases complete our coverage of the three device platforms we intended to cover earlier this year when we started our work – Windows Phone, iOS, and Android.

 

![Windows Azure Toolkits for Devices](https://wadewegner.blob.core.windows.net/wordpress/2011/08/DevicesToolkits.jpg)

 

It’s my belief that cloud computing provides a significant opportunity for mobile device developers, as it gives you the ability to write applications that target the same services and capabilities regardless of the device platform. Furthermore, I believe that Windows Azure is the best place to host these services. Take a look at the post [Microsoft Releases the Windows Azure Toolkit for Android](http://blogs.technet.com/b/microsoft_blog/archive/2011/08/31/microsoft-releases-the-windows-azure-toolkit-for-android.aspx) for examples of how American Airlines and Linxter are using the toolkits and Windows Azure to build great cross-device applications!

 

In addition to releasing the the Android toolkit, we have released some important updates to the [Windows Phone](http://watwp.codeplex.com/) and [iOS (i.e. iPhone & iPad)](https://github.com/microsoft-dpe/wa-toolkit-ios) toolkits for Windows Azure. Since I have so many things to cover in this post, let me break it all down in various sections (click the links to jump to the section of choice):

 

**_Android_**

 

Today we released version 0.8 of the Windows Azure Toolkit for Android. This version includes native libraries that provide support for storage and authN/Z, a sample application, and unit tests. Everything is built in Eclipse and uses the [Android SDK](http://developer.android.com/sdk/index.html).

 

Here’s the project structure:

 

  
  * library       
![Eclipse library project](https://wadewegner.blob.core.windows.net/wordpress/2011/08/Projects1.jpg)
   
  * simple (sample application)       
![Eclipse sample project](https://wadewegner.blob.core.windows.net/wordpress/2011/08/Projects2.jpg)
   
  * tests       
![Eclipse test project](https://wadewegner.blob.core.windows.net/wordpress/2011/08/Projects3.jpg)
 

The library project includes the full source code to the storage client and authentication implementations.

 

Once you configure your workspace in Eclipse, you can run the simple sample application within the Android emulator.

 

![Starting the Android Emulator](https://wadewegner.blob.core.windows.net/wordpress/2011/08/StartingEmulator.jpg)

 

From here you choose to either connect directly to Windows Azure storage using your account name and key or through your proxy services running in Windows Azure. To set your account name and key, modify the ProxySelector.java file …

 

![ProxySelector](https://wadewegner.blob.core.windows.net/wordpress/2011/08/ProxySelector.jpg)

 

… found here:

 

![FileLocation](https://wadewegner.blob.core.windows.net/wordpress/2011/08/FileLocation.jpg)

 

As with the Windows Azure Toolkits for Windows Phone and iOS, we recommend you do **_not_** put the storage account name and key in your application source code. Instead, use a set of secure proxy services running in Windows Azure. You can use the [Cloud Ready Packages for Devices](https://github.com/microsoft-dpe/wa-toolkit-cloudreadypackages) which contain a set of pre-built services ready to deploy to Windows Azure.

 

As you can see from it’s name, the shipping sample is designed to be simple – do not consider this a best practice from a UI perspective. However, it does should you fully how to implement the storage and authentication libraries. For another alternative at the library implementations, take a look at the unit tests.

 

**_Windows Phone_**

 

Today we released version 1.3.0 of the Windows Azure Toolkit for Windows Phone. This release includes a number of long awaited features and updates, including:

 

  
  * Support for SQL Azure as a membership provider.
   
  * Support for SQL Azure as a data source through an OData service.
   
  * Upgraded the web applications to ASP.NET MVC 3.
   
  * Support for the Windows Azure Tools for Visual Studio 1.4 and Windows Phone Developer Tools 7.1 RC.
   
  * Lots of little updates and bug fixes.
 

I’m most excited about the support for SQL Azure.

 

![SQL Azure Support](https://wadewegner.blob.core.windows.net/wordpress/2011/08/SQLAzureSupport.jpg)

 

The new project wizard now lets you choose where you want to store data in Windows Azure – you can both Windows Azure storage and SQL Azure database!

 

You can choose to enter your SQL Azure credentials into the wizard (which places them securely in your Windows Azure project, not the device) or use a SQL Server instance locally for development.

 

![Using SQL Azure Locally](https://wadewegner.blob.core.windows.net/wordpress/2011/08/SQLAzureLocally.jpg)

 

Once you’ve finished walking through the wizard, you may not immediately notice anything different – that’s a good thing! However, in the background all the membership information has been stored in a SQL database, and you’ll see in the application a new tab for your SQL Azure data that’s consumed through an OData service.

 

![SQL Azure in the Phone Emulator](https://wadewegner.blob.core.windows.net/wordpress/2011/08/SQLAzureInThePhoneEmulator.jpg)

 

Moving forward we plan to only target the Windows Phone Developer Tools 7.1 releases (i.e. no more WP 7.0). However, we have archived all the 7.0 samples, and will continue to ship them as part of the toolkit as long as the Windows Phone Marketplace accepts 7.0 applications. You can find them organized in two different folders: WP7.0 and WP7.1.

 

![Samples](https://wadewegner.blob.core.windows.net/wordpress/2011/08/image14.png)

 

**_iOS_**

 

Over the last few weeks, and with the help of [Scott Densmore](http://scottdensmore.typepad.com/), we have made a series of important updates to the [Windows Azure Toolkit for iOS](https://github.com/microsoft-dpe/wa-toolkit-ios) – namely, bug fixes and project restructuring!

 

Over the last few weeks, as more and more developers used our iOS libraries, we started getting reports of memory leaks. Scott has spent a considerable amount of time tracking these down, and all these updates have been checked into the repo. Additionally, we have spent some time refactoring our github repos, and you’ll now find everything related to the Windows Azure Toolkit for iOS in a single repository:

 

![github repo for iOS](https://wadewegner.blob.core.windows.net/wordpress/2011/08/watoolkitforios.jpg)

 

This gives us a lot more flexibility for releases, as well as making sure that the resources are easy to use and consume.

 

**_What’s Next?_**

 

Oh no, we’re not done! There’s still a lot we want to do. We continue to get great feedback from customers and partners using these toolkits.

 

Over the next few months, here are the things we’ll focus on:

 

  
  * Continue to update the Windows Azure Toolkits for iOS and Android so that they are in full parity with the Windows Azure Toolkit for Windows Phone.
   
  * Samples, samples, and more samples! We want to have a great set of samples that work across all three device platforms. We’ve got a good start with BabelCam, but we need to bring it to iOS and Android, and then build more!
   
  * Continue to support and fix the toolkits.
 

Your feedback is invaluable, so please continue to send it our way!
