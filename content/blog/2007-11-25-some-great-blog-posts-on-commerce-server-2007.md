---
author: admin
categories:
- Commerce Server
comments: true
date: "2007-11-25T20:50:31Z"
slug: some-great-blog-posts-on-commerce-server-2007
title: Some great blog posts on Commerce Server 2007
wordpress_id: 22
---

There has been some excellent activity by some heavy-weights in the Commerce Server world. Sadly, I haven't been contributing very much on my blog, as I am desperately trying to complete [my book](http://www.wadewegner.com/2007/03/13/ProfessionalCommerceServer2007.aspx).




It started with [Søren Spelling Lund](http://www.publicvoid.dk/)'s two-part series (and maybe more?) on what it's like developing with Commerce Server 2007.






  * [Developing with Microsoft Commerce Server 2007 Part 1: Secure By Default](http://www.publicvoid.dk/DevelopingWithMicrosoftCommerceServer2007Part1SecureByDefault.aspx)



> 

> 
> In this post, Søren highlights the high-level of security that has gone into Commerce Server 2007, calling it "both a blessing and a curse." He attributes this to the flexibility and granularity of the security system, in addition to the complexity that comes with it. Commerce Server 2007 makes use of the Windows Authorization Manager for security. See the following links for more information: [Developing Applications Using Windows Authorization Manager](http://msdn2.microsoft.com/en-us/library/Aa480244.aspx), and [Managing Authorization Policies](http://msdn2.microsoft.com/en-us/library/ms914867.aspx).
> 
> 

> 
> Søren also highlights the Distributed Transaction Manager and use of MSDTC and System.Transaction in the .NET Framework 2.0.






  * [Developing with Microsoft Commerce Server 2007 Part 2: Three-Way Data Access](http://www.publicvoid.dk/DevelopingWithMicrosoftCommerceServer2007Part2ThreeWayDataAccess.aspx)



> 

> 
> Søren discusses three different data access systems for Commerce Server 2007, based on the subsystem with which you're working (i.e. the Profile system, the Catalog system, and the Order system). Take a look at his post for the specifics. I would also suggest you take a look at MSDN for some additional information on [developing with Commerce Server 2007](http://msdn2.microsoft.com/en-us/library/aa546089.aspx).




Not to be out-done by Søren, [Max Akbar](http://blogs.msdn.com/maxakbar/) took some time out of his busy schedule to post a great article on caching and Commerce Server 2007.






  * [A Few Things You Didn't Know About Commerce Server's Catalog Cache](http://blogs.msdn.com/maxakbar/archive/2007/11/22/a-few-things-you-didn-t-know-about-commerce-server-s-catalog-cache.aspx)



> 

> 
> Max highlights a number of important topics, including: the Catalog cache, Web.Config settings, refreshing the cache, the cache size, the cache location, how to use your own caching.
> 
> 

> 
> As always, Max's post is a great blend of information and code snippets.




Last, but certainly not least, [Tom Schultz](http://blogs.msdn.com/tschultz) contributed to the discussion of caching by highlighting a mixed-authentication solution for the SiteCacheRefresh HTTP handler.






  * [SiteCacheRefresh - Mixed Authentication Solution](http://blogs.msdn.com/tschultz/archive/2007/11/25/sitecacherefresh-mixed-authentication-solution.aspx)



> 

> 
> Tom shows how the SiteCacheRefresh HTTP handler provides Commerce Server with caching capabilities. He goes beyond this, however, when he points out that by default the ASP.NET site uses forms authentication. Since the web site can support either forms or windows authentication, a mixed authentication model is required. Tom shows you how to construct this by taking aspects of the Starter Site




All in all, great stuff!
