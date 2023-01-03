---
author: admin
categories:
- Commerce Server
comments: true
date: "2007-08-11T16:19:16Z"
slug: commerce-server-2007-development-environments
title: Commerce Server 2007 Development Environments
wordpress_id: 47
---

![Commerce Server 2007](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/CommerceServer2007DevelopmentEnvironment_9127/CS2007_thumb.png)

I often get asked about how development environments should be configured and what practices should be implemented when building a Commerce Server 2007 solution. I have found this to be a difficult question to answer, as everyone has their own preferences and "best-practices" when it comes to development environments. Additionally, we are often constrained by the following variables:

  * Relative importance placed on time, cost, and quality
  * Available development and QA skills
  * Software development methodology
  * Scope and function of the application

Over my career I've come up with a number of things I try to have when building a Commerce Server solution (or any enterprise-level application).

### Development Environments

I believe that developers need a place to integrate their code, QA and UAT require a sandbox independent of development, that you must have a staging environment to prepare your data for production, and that production should be completely isolated from your other environments.

While I don't always get my way, I try to always have the following environments defined (in addition to production, of course):

  * Development (each developer has their own local development environment)
  * Development integration
  * Quality Assurance (QA) / User Acceptance Testing (UAT)
  * Staging

The lack of even one of these environments puts everything at risk. For example, if you do not have a development integration environment you'll find that most of the defects QA discovers will relate to simple integration, and that they will be challenged in performing functional testing of your application. Similarly, if your development and QA environments share resources (such as databases, etc), you'll constantly trip over yourself and find it very difficult to make forward progress.

> _Note that I make the assumption you will have a separate QA team and initiative. Ignore this at your own risk! Never trust developers to QA their own code. I'm a developer myself, and while I always strive for perfection I have always been more successful when working with a good QA team._

Many of these environments can be virtualized. There's nothing wrong with having your development integration and QA/UAT environments running on VMWare or Virtual Server (sometimes it's nice, as you can blow it away and revert to an earlier image).

### Development Practices and Techniques

A well-architected and designed development environment is useless if you implement sloppy development practices. It's not enough to isolate your environments - you have to put techniques and practices in place to effectively use them.

In addition to demanding these environments, I always _**require **_the following practices and techniques:

  * Source control and versioning
  * Continuous integration
  * Commerce Server Staging (CSS) and build automation
  * Unit testing & test-driven development (TDD)

It's absolutely imperative that you have a source control solution. Without it you run so many risks that I can't even begin to describe the pains you will feel later on. It doesn't really matter which source control solution you use, so long as it is stable, reliable, and you back it up! Don't neglect to backup your source control databases.

Continuous integration (CI) is a practice whereby a development team frequently integrates their work (e.g. source code). CI should be automated so that it runs regularly. The whole point is that this automated process can find integration errors very quickly so that it reduces the pains you will feel down the road. There's nothing worse than having your developers work in isolated environments for a significant period of time only to discover that the their code won't compile once integrated.

Commerce Server Staging (CSS) is the best and preferred way to migrate your web application and business data between environments. You can setup the CSS services on your Commerce Server machines (not in production, mind you) so that you can deploy your code changes and business data from environment to environment. It is useful to have a specific staging environment that is identical (or as close as possible) to production, so that your business users can manipulate the data in the staging environment rather than run the risk of screwing up production because they're playing with the data.

I firmly believe in unit testing and test-driven development (TDD). TDD is a technique in which test cases are written and then code is subsequently written that can pass the test. Unit tests are written for each aspect of the code and then automated so that you can get quick and effective feedback to confirm that your software is well-written. While this technique requires discipline and (for some developers) unlearning certain habits, it is a powerful way of writing enterprise-level applications better and faster.

### Conclusion

It's no easy or simple task to setup a development environment for a Commerce Server 2007 solution (or any enterprise-level application for that matter). However, if you spend the time and energy up front to establish a good development environment, you will reap the rewards later on.

A very powerful message was driven into me at one of the first IT shops I worked for: **design excellence, and rigorous attention to detail** (thanks, [RGI](http://www.rimrockgroup.com/)). While I consider it criminal for developers to not religiously adhere to this creed, we are all human and often err or stray from the path. Putting together a solid development environment and implementing good software techniques and practices can help save us from ourselves.

I would very much like to hear about everyone else's experiences with Commerce Server development environments (or any development environment). What have you found to be successful? **Please leave a message!**
