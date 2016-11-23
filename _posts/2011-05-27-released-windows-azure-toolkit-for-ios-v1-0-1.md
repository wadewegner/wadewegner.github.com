---
author: admin
comments: true
date: 2011-05-27 16:40:48+00:00
layout: post
slug: released-windows-azure-toolkit-for-ios-v1-0-1
title: 'RELEASED: Windows Azure Toolkit for iOS V1.0.1'
wordpress_id: 1273
categories:
- iOS
- Toolkit
- Windows Azure
---

![Windows Azure Toolkits for Devices](https://wadewegner.blob.core.windows.net/wordpress/2011/05/image15.png)

Last night we released an update to the [Windows Azure Toolkit for iOS](https://github.com/microsoft-dpe). This release is a minor update that includes the refactoring of code for better conformance to standards, some bug updates, and a few additions to the sample application. You can take a look at the updates in each of the three repositories:

 

  
  * [https://github.com/microsoft-dpe/watoolkitios-lib](https://github.com/microsoft-dpe/watoolkitios-lib)
   
  * [https://github.com/microsoft-dpe/watoolkitios-samples](https://github.com/microsoft-dpe/watoolkitios-samples)
   
  * [https://github.com/microsoft-dpe/watoolkitios-doc](https://github.com/microsoft-dpe/watoolkitios-doc)
 

One of the important updates is guidance—via the sample application—on how to use Windows Azure Queues.

 

![Queues](https://wadewegner.blob.core.windows.net/wordpress/2011/05/Queues.png)

 

Here’s an exhaustive list of the updates.

 

  
  * Refactored the classes for better conformance with a **Cocoa framework**: [http://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html](http://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html)
   
  * Renamed **TableEntity valueForKey** and **setValue:forKey** to **objectForKey** and **setObject:forKey**
   
  * Renamed **Block** methods to **CompletionHandler**
   
  * Renamed "**get**" methods to "**fetch**" methods for better conformance
   
  * Converted **Base64** helper methods to proper categories on **NSString** and **NSData**
   
  * Refactored sample client to support above library changes
   
  * Included example of **queue support in the sample client**
   
  * Fixed **bug** in queue class when sending messages containing non-escaped characters (<, >, &, ')
   
  * Fixed **bug** in queue class with delegate not firing on message completion
   
  * Updated **walkthrough documentation** for above class refactoring
   
  * Updated compile HTML class documentation for above class refactoring
 

What’s nice about this release is that some of these [updates came from the community](http://www.wadewegner.com/2011/05/merged-a-pull-request-for-the-ios-toolkit/) and some of them came from us – a model I hope to continue as we move forward with these toolkits.

 

When you visit the GitHub repositories, be sure to look for and download **v1.0.1.zip **or the **v1.0.1 tag.**

 

![Release](https://wadewegner.blob.core.windows.net/wordpress/2011/05/Release.png)

   

We have more updates in the pipeline, so expect to hear more soon.

 

I hope this helps!
