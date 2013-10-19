---
author: admin
comments: true
date: 2007-03-23 02:03:37+00:00
layout: post
slug: programming-with-commerce-server-in-two-modes-inproc-mode-and-web-service-mode
title: 'Programming with Commerce Server in two modes: InProc mode and Web Service
  mode'
wordpress_id: 99
categories:
- Commerce Server
---

There's a concept in Commerce Server (and many other products, for that matter) that's important to understand. There are two ways to go about programming against the core systems (i.e. Catalog, Inventory, Profiles, Orders, and Marketing) in Commerce Server--InProc mode and Web Service mode. Vinayak, a developer on the Commerce Server team, wrote an excellent blog about this concept (last October) as it [relates to the catalog system](http://blogs.msdn.com/vinayakt/archive/2006/08/30/731135.aspx). There are also some great code examples.

When you are programming with the Inproc mode, the assembly is loaded into the callers AppDomain. This is fine if you have the ability to run your code on the actual web server, but what if you can't? This is where the Web Service mode becomes useful. In this mode, the specific Commerce Server functionality is now available remotely via a set of system specific web services.

So what are the differences? When would you use one over the other? Here are things to consider:

  * Inproc mode is obviously fast than Web Server mode; if speed and load are a factor, consider using Inproc mode 
  * If you performing an action from a remote machine (let's say you're using the business management system) then it makes sense to use the Web Service mode

With regards to actual coding, the only difference between these two modes is the way in which the context object is created. Otherwise, the object model and methods available are identical.

Best of luck!
