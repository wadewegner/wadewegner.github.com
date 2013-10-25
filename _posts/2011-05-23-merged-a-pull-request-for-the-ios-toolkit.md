---
author: admin
comments: true
date: 2011-05-23 20:02:09+00:00
layout: post
slug: merged-a-pull-request-for-the-ios-toolkit
title: Merged a Pull Request for the iOS Toolkit
wordpress_id: 1243
categories:
- GitHub
- iOS
- Windows Azure
---

[![Windows Azure Toolkits for Devices](http://images.wadewegner.com/wordpress/2011/05/image_thumb12.png)](http://images.wadewegner.com/wordpress/2011/05/image15.png)A few days ago, [Fredrik Olsson](http://www.peylow.se/) opened a pull request against our GitHub repository for the [Windows Azure Toolkit for iOS](https://github.com/microsoft-dpe/watoolkitios-lib). A [pull request](http://help.github.com/pull-requests/) provides a way to tell others about changes you’ve made to a repository – by sending me a pull request, Fredrik was notifying me of his updates so that I could review and, if appropriate, merge them to the main repository.

 

Today I merged Fredrik’s pull request. To me, this is exciting for two reasons: 1) it brings solid updates to our library, and 2) it emphasizes one of the core value propositions for using GitHub – **collaborative development! **This goes back to some of our initial conversations as we were planning these toolkits. We spent a lot of time discussing where to store the source code for the iOS toolkit, and we ultimately chose GitHub because we felt it gave us the best opportunity for receiving updates from the community – Fredrik’s pull request validates this choice!

 

Fredrick made some good updates to the library – some of them we planned to update soon, but a few we hadn’t. These updates include (in his own words):

 

  
  * Proper use of WA (Windows Azure) as prefix, to avoid name clashes for common names such as Queue, Blob, etc.
   
  * TableEntity used valueForKey: and setValue:forKey:, thus overriding KVO functionality from NSObject. Renamed to objectForKey: and setObject:forKey: as the implementation intends.
   
  * "Block" generally changed to CompletionHandler, blocks should be named after their intention, with fallback to "block" only for truly generic functionality such as collection enumerations.
   
  * Use of the verb "get" changed to "fetch". "Get" is by convention reserved for out-arguments of methods.
   
  * Base64 helper converted to proper categories on NSString and NSData, this is a pattern that should have been applied more broadly.
 

You can see the full [pull request](https://github.com/microsoft-dpe/watoolkitios-lib/pull/1) here (along with our commentary).

 

Thank you for your contributions, [Fredrik](http://www.peylow.se/). Keep them coming!
