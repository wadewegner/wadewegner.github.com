---
author: admin
comments: true
date: 2007-06-20 02:50:33+00:00
layout: post
slug: clickonce-deployment-and-the-commerce-server-2007-business-management-applications
title: ClickOnce Deployment and the Commerce Server 2007 Business Management Applications
wordpress_id: 68
categories:
- .NET 2.0
- Commerce Server
---

As a Commerce Server developer, you are bound to come across a situation where the smart client business user applications (e.g. the Catalog Manager, Catalog and Inventory Schema Manager, Customer and Orders Manager, and Marketing Manager) do not satisfy a business requirement, need to reflect a company's brand or style, or your business users simply want it changed.

Fortunately, you can get the full C# source code for the business user applications (except for the Catalog and Inventory Schema Manager; see [Max's](http://blogs.msdn.com/maxakbar/) comment in [this thread](http://forums.microsoft.com/MSDN/ShowPost.aspx?PostID=1141608&SiteID=1)) through the [Commerce Server 2007 Partner SDK](http://www.microsoft.com/commerceserver/partners/sdk/default.mspx). This gives you the ability to modify the business user applications and be a hero to all your colleagues and clients (chuckle!).

Yeah, yeah, this is all old news. However, I just came across a great idea from [Søren Spelling Lund](http://www.publicvoid.dk/default.aspx) in which he talks about his experiences using ClickOnce Deployment along with the business user applications.

### What is ClickOnce Deployment?

[ClickOnce deployment](http://msdn2.microsoft.com/en-us/library/t71a733d(VS.80).aspx) is a technology designed to ease the difficulty in creating self-updating Windows-based applications. Using the Publish Wizard in Visual Studio 2005 (or mage.exe, mageui.exe, or MSBuild), you can publish your application in three different ways: to media (such as a CD-ROM), to a network file share, or to a Web page.

When an application is published, two files are created: an application manifest, and a deployment manifest. The application manifest describes the contents of the application, including the assemblies, dependencies, and the files that make up the application. The deployment manifest describes how the application is deployed, the location of the application manifest, and the version of the application that should be run by the clients.

Here's an example of an application published to a Web page (this particular example is from the SpaceWar SDK for the XNA framework - yes, I truly aspire to be an [XBOX 360 game developer](http://msdn2.microsoft.com/en-us/xna/default.aspx)):

[![Example of application published to a Web page via ClickOnce](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/ClickOnceDeploymentoftheCommerceServer20_DCB4/ClickOnce_thumb.gif)](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/ClickOnceDeploymentoftheCommerceServer20_DCB4/ClickOnce.gif)

(Note the deployment version number.)

Once the end-user installs the application from the deployment location, the application is, by default, added to the Start menu and the Add/Remove programs group in the Control Panel. **Nothing is added to the Program Files folder, the registry, or the desktop.** When I first played around with ClickOnce this last part caught me unaware -- I couldn't figure out where my application had been installed! Also, no administrative rights are required to install the application.

Now, remember the deployment version number? The best part about this technology is that, if a developer publishes a new version of the application, the deployment version number is incremented. Consequently, the next time the end-user runs the application they are **presented with an opportunity to update their application**. What's neat is that, as a developer, you actually have a lot of control: you can require an update, and you can even require that a user rolls back to an earlier version of the application (not that any of us would ever have to roll back to an earlier version ...).

All-in-all, it's neat stuff. And, while it's not a perfect solution and [has it's own problems](http://www.google.com/search?hl=en&rlz=1T4GGIG_enUS227US227&q=clickonce+problems), you can very rapidly integrate it into your application and quickly reap the rewards. Oh, and you can easily become a hero to your colleagues and clients (are you starting to sense a pattern?).

### Integrate ClickOnce Deployment into the Partner SDK?

So, we're back to the smart client business user applications. I'm sure you're now asking yourself, what does this have to do with the Commerce Server 2007 business user applications (or maybe not, since I alluded to it above)?

[Søren Spelling Lund](http://www.publicvoid.dk/default.aspx) posted an article about integrating ClickOnce Deployment into the business user applications. It's a great idea, and one I will definitely use in the future. Additionally, he takes the time to point out a problem him and his colleagues ran into when they tried to run the ClickOnce installer on the Customer and Orders Manager. Essentially, it appears that the project file included a <TargetZone> element that interfered with the ClickOnce installer. Removing the <TargetZone> element resolved the problem. Check out [his blog article](http://www.publicvoid.dk/ClickOnceDeploymentOfCommerceServer2007BusinessTools.aspx) for the full details.

What are the benefits of integrating ClickOnce Deployment into the Partner SDK?

  * Ease - Easily deployment of business user applications to end-users
  * Versioning - Make sure your end-users are running the latest and greatest
  * Safety - The ability to roll back to a previous version of the application (just in case!)
  * Security - The applications run in a security context that prevents users from doing malicious things
  * Fame - Yes, you too can be a hero ...

Thanks to Søren for a great idea and a great post!

What great ideas have you put into play? Please let me know!
