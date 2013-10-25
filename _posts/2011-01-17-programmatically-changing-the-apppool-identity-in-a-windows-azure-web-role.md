---
author: admin
comments: true
date: 2011-01-17 18:49:39+00:00
layout: post
slug: programmatically-changing-the-apppool-identity-in-a-windows-azure-web-role
title: Programmatically Changing the AppPool Identity in a Windows Azure Web Role
wordpress_id: 964
categories:
- AppPool
- ASP.NET
- Web Role
- Windows Azure
---

With the Windows Azure SDK 1.3 comes full support for IIS in web roles, giving your web roles the ability to access the full range of web service features available in IIS. This provides a lot of powerful capabilities, some of which are outlined [here](http://blogs.msdn.com/b/windowsazure/archive/2010/12/02/new-full-iis-capabilities-differences-from-hosted-web-core.aspx).

By default, the AppPool runs as the NetworkService. Go ahead and create a new Windows Azure Project, choose a web role, and hit F5 to run it in IIS. A new AppPool is created with a GUID for the name, and you’ll see that the Identity is NetworkService.

[![DefaultAppPoolIdentity](http://images.wadewegner.com/wordpress/2011/01/DefaultAppPoolIdentity_thumb.png)](http://images.wadewegner.com/wordpress/2011/01/DefaultAppPoolIdentity.png)

In most cases this is fine, and the NetworkService identity is sufficient. However, there are some cases where you many need to change the AppPool identity. Here at Microsoft, for example, we have an internal proxy that manages all our network traffic. In order for traffic to move through the proxy, the underlying identity has to be an authenticated domain user. Consequently, any requests sent by the NetworkService result in errors when trying to access network resources (e.g. Windows Azure storage, AppFabric service namespaces, and SQL Azure).

Since we cannot proactively change the settings used to create the AppPool, we need to make the change immediately following it’s creation but before our application starts to run. Consequently, we’ll use the OnStart() method in the WebRole’s role entry point (i.e. WebRole.cs).

First, you’ll need to add a reference to two assemblies:
  
  1. System.DirectoryServices 
   
  2. Microsoft.Web.Administration (found in C:WindowsSystem32inetsrv) 

Now, update the OnStart() method with the following code:

	1. public override bool OnStart()
	2. {
	3.     // For information on handling configuration changes
	4.     // see the MSDN topic at http://go.microsoft.com/fwlink/?LinkId=166357.
	5.   
	6.     var webApplicationProjectName = "Web";
	7.     var appPoolUser = "northamerica\wwegner";
	8.     var appPoolPass = "password";
	9.     var metabasePath = "IIS://localhost/W3SVC/AppPools";
	10.   
	11.     using (ServerManager serverManager = new ServerManager())
	12.     {
	13.         var appPoolName = serverManager.Sites[RoleEnvironment.CurrentRoleInstance.Id + "_" + webApplicationProjectName].Applications.First().ApplicationPoolName;
	14.   
	15.         using (DirectoryEntry appPools = new DirectoryEntry(metabasePath))
	16.         {
	17.             using (DirectoryEntry devFabricAppPool = appPools.Children.Find(appPoolName, "IIsApplicationPool"))
	18.             {
	19.                 if (devFabricAppPool != null)
	20.                 {
	21.                     devFabricAppPool.InvokeSet("AppPoolIdentityType", new Object[] { 3 });
	22.                     devFabricAppPool.InvokeSet("WAMUserName", new Object[] { appPoolUser });
	23.                     devFabricAppPool.InvokeSet("WAMUserPass", new Object[] { appPoolPass });
	24.                     devFabricAppPool.Invoke("SetInfo", null);
	25.   
	26.                     devFabricAppPool.CommitChanges();
	27.                 }
	28.             }
	29.         }
	30.     }
	31.   
	32.     return base.OnStart();
	33. }

A few things to point out:
  
* Line #6: Specify the name of your web role project as webApplicationProjectName. This is used on line #13 to look up the newly created AppPool. 

[![image](http://images.wadewegner.com/wordpress/2011/01/image_thumb.png)](http://images.wadewegner.com/wordpress/2011/01/image.png)

* Lines #7 & #8: Update to use your own username & password. 

* Line #21: The AppPoolIdentityType of 3 means we’re providing a custom username & password. 

Once you update your code and run again, you’ll see that your AppPool is updated with the new identity.

[![NewAppPoolIdentity](http://images.wadewegner.com/wordpress/2011/01/NewAppPoolIdentity_thumb.png)](http://images.wadewegner.com/wordpress/2011/01/NewAppPoolIdentity.png)

Now, whenever your application makes a request through the network, it will originate from your domain account (or whatever you specify) instead of the NetworkService.

**WARNING**: I should point out that there is one negative side effect that I’ve noticed when changing the identity of the AppPool programmatically – it will prevent the debugger from working normally. I don’t (yet) know of a way to resolve this, but I’m still looking.

I hope this helps!