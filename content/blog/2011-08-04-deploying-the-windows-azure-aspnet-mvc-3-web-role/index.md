---
title: "Deploying the Windows Azure ASP.NET MVC 3 Web Role"
date: 2011-08-04T00:00:00-08:00
draft: false
description: "From the whiteboard to the keyboard"
---

Yesterday [Steve Marx](http://blog.smarx.com/) and I covered the new [Windows Azure Tools for Visual Studio 1.4](http://blogs.msdn.com/b/windowsazure/archive/2011/08/03/announcing-the-august-2011-release-of-the-windows-azure-tools-for-microsoft-visual-studio-2010.aspx) release on the [Cloud Cover Show](http://channel9.msdn.com/Shows/Cloud+Cover/) (will publish on Friday, 8/5). Since the tools shipped the same day as the show we literally only had an hour or so before the show to pull together a demonstration of some of the new capabilities. I think we did a reasonably good job, but I’d like to further clarify a few things in this post.

For a complete look at the updated tools, I recommend taking a look at posts by Technical Evangelists [Nathan Totten](http://ntotten.com/2011/08/windows-azure-tools-1-4-released/) and [Nick Harris](http://www.nickharris.net/2011/08/using-the-new-windows-azure-tools-v1-4-for-vs2010/). Additionally, you should review the [release documentation on MSDN](http://msdn.microsoft.com/en-us/library/ff683673.aspx).

One of the items I demonstrated on the show was deploying the new MVC 3 template to Windows Azure.

![Windows Azure ASP.NET MVC 3 Web Role](https://wadewegner.blob.core.windows.net/wordpress/2011/08/image10.png)

It’s great having this template built into the tools. No longer do I have to hit Steve Marx’s website to lookup the [requisite MVC 3 assemblies](http://blog.smarx.com/posts/asp-net-mvc-in-windows-azure).

Immediately after creating the projects I confirmed that my assemblies were all added and set to \*\*Copy Local = True \*\*by default (one of the nice aspects of having the template baked into the tools) and published to Windows Azure. I was a bit surprised when suddenly I got the [YSOD](http://en.wikipedia.org/wiki/Screen_of_death):

![YSOD](https://wadewegner.blob.core.windows.net/wordpress/2011/08/image1.png)

Naturally, I updated my **web.config** file with  and redeployed to Windows Azure.

![No SQL Express](https://wadewegner.blob.core.windows.net/wordpress/2011/08/image2.png)

In case you don’t want to click the image, the error is:

> *A network-related or instance-specific error occurred while establishing a connection to SQL Server. The server was not found or was not accessible. Verify that the instance name is correct and that SQL Server is configured to allow remote connections. (provider: SQL Network Interfaces, error: 26 - Error Locating Server/Instance Specified)"*

In retrospect, I should have expected this error as I received validation warnings within Visual Studio (one of the other nice updates in the 1.4 tools):

![Warning](https://wadewegner.blob.core.windows.net/wordpress/2011/08/image11.png)

I was quite baffled as to why MVC 3 suddenly would require SQL Express (or any flavor of SQL for that matter), until [Anders Hauge](http://social.msdn.microsoft.com/profile/anders%20hauge%20-%20msft/)—a PM on the Visual Studio team—clued me in on the fact that they have shipped the ASP.NET Universal Providers within the MVC 3 template.

> For information on these providers, see [Introducing System.Web.Providers - ASP.NET Universal Providers for Session, Membership, Roles and User Profile on SQL Compact and SQL Azure](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) by Scott Hanselman for a great introduction.

It turns out that, by default, the template uses the default session state provider in ASP.NET Universal Providers as the ASP.NET session state provider, which in turn uses SQL Express (by default) to store session state.

```
<span style="color: blue"><</span><span style="color: #a31515">sessionState </span><span style="color: red">mode</span><span style="color: blue">=</span>"<span style="color: blue">Custom</span>" <span style="color: red">customProvider</span><span style="color: blue">=</span>"<span style="color: blue">DefaultSessionProvider</span>"<span style="color: blue">>
  <</span><span style="color: #a31515">providers</span><span style="color: blue">>
    <</span><span style="color: #a31515">add </span><span style="color: red">name</span><span style="color: blue">=</span>"<span style="color: blue">DefaultSessionProvider</span>"
         <span style="color: red">type</span><span style="color: blue">=</span>"<span style="color: blue">System.Web.Providers.DefaultSessionStateProvider, System.Web.Providers, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35</span>"
         <span style="color: red">connectionStringName</span><span style="color: blue">=</span>"<span style="color: blue">DefaultConnection</span>"
         <span style="color: red">applicationName</span><span style="color: blue">=</span>"<span style="color: blue">/</span>" <span style="color: blue">/>
  </</span><span style="color: #a31515">providers</span><span style="color: blue">>
</</span><span style="color: #a31515">sessionState</span><span style="color: blue">>
</span>
```

Okay, so what’s the best way to get this to work?

Well, in my haste on the Cloud Cover Show, I simply uninstalled the ASP.NET Universal Provider package, which removed the custom session state provider. Of course, once I deployed, I no longer saw the YSOD. This is probably not the best way to go about fixing this, as the ASP.NET Universal Providers are really worth using. For a good analysis of the options, take a look at Nathan’s discussion on various ways to resolve this in his post [Windows Azure Tools 1.4 Released](http://ntotten.com/2011/08/windows-azure-tools-1-4-released/).

In retrospect, I wish I had simply created a SQL Azure database and used it instead. It’s pretty easy to do:

1. In the [Windows Azure Platform Management Portal](http://windows.azure.com/), select **Database** and create a new database. I called mine **UniversalProviders**.

![image](https://wadewegner.blob.core.windows.net/wordpress/2011/08/image4.png)

2. Select the database and click the **View …** button to grab/copy your connection string. Note: you’ll need to remember your SQL login password.

![image](https://wadewegner.blob.core.windows.net/wordpress/2011/08/image6.png)

3. In your solution, update the \*\*ApplicationServies \*\*and **DefaultConnection** connection strings using your SQL Azure connection string. Note: you must set \*\*MultipleActiveResultSets=True \*\*in the connection string, so be sure to add it back if you’ve copied the SQL Azure connection string from the portal.

```
<span style="color: blue"><</span><span style="color: #a31515">connectionStrings</span><span style="color: blue">>
  <</span><span style="color: #a31515">add </span><span style="color: red">name</span><span style="color: blue">=</span>"<span style="color: blue">ApplicationServices</span>"
       <span style="color: red">connectionString</span><span style="color: blue">=</span>"<span style="color: blue">Server=tcp:YOURDB.database.windows.net,1433;Database=UniversalProviders;User ID=YOURLOGIN@YOURDB;Password=YOURPWD;Trusted_Connection=False;Encrypt=True;MultipleActiveResultSets=True;</span>"
       <span style="color: red">providerName</span><span style="color: blue">=</span>"<span style="color: blue">System.Data.SqlClient</span>" <span style="color: blue">/>
  <</span><span style="color: #a31515">add </span><span style="color: red">name</span><span style="color: blue">=</span>"<span style="color: blue">DefaultConnection</span>"
       <span style="color: red">connectionString</span><span style="color: blue">=</span>"<span style="color: blue">Server=tcp:YOURDB.database.windows.net,1433;Database=UniversalProviders;User ID=YOURLOGIN@YOURDB;Password=YOURPWD;Trusted_Connection=False;Encrypt=True;MultipleActiveResultSets=True;</span>"
       <span style="color: red">providerName</span><span style="color: blue">=</span>"<span style="color: blue">System.Data.SqlClient</span>" <span style="color: blue">/>
</</span><span style="color: #a31515">connectionStrings</span><span style="color: blue">>
</span>
```

4. Deploy to Windows Azure.

Now, when we run the applications, it works!

![image](https://wadewegner.blob.core.windows.net/wordpress/2011/08/image7.png)

In fact, I recommend you use \*\*Database Manager \*\*from the portal …

![image](https://wadewegner.blob.core.windows.net/wordpress/2011/08/image8.png)

… to open up the database and see that, in fact, you now have a **dbo.Sessions** table in the database.

![image](https://wadewegner.blob.core.windows.net/wordpress/2011/08/image9.png)

What’s more, if you then click \*\*Log On \*\*and setup a user, you’ll see all the membership & roles work within the MVC application in Windows Azure!

![Picture](https://wadewegner.blob.core.windows.net/wordpress/2011/08/Picture.png)

Wrapping up, apologies for not digging into this in greater depth on the show – hopefully this post helps to clarify a few things.
