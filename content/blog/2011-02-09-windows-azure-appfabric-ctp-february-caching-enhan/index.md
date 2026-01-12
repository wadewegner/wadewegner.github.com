---
title: "Windows Azure AppFabric CTP February, Caching Enhancements, and the New LABS Portal"
date: 2011-02-09T00:00:00-08:00
draft: false
description: "From the whiteboard to the keyboard"
---

I’m excited to tell you about the Windows Azure AppFabric CTP February release. There are a number of enhancements and improvements coming with this release, including:

- New **Silverlight-based LABS portal**, bringing consistency with the production Windows Azure portal.
- Ability to select either a **128MB or 256MB cache size**.
- Ability to **dynamically upgrade or downgrade** your cache size.
- **Improved diagnostics** with client side tracing and client request tracking capabilities.
- Overall **performance improvements**.

When you land on the new portal page you’ll see an experience consistent with the production Windows Azure portal – in fact, this release moves us one step closer to merging the Windows Azure AppFabric portal experiences into the same portal as Windows Azure and SQL Azure.

In the LABS portal, the CTP environment only lights-up **Service Bus, Access Control & Caching**.

![Picture 0](https://wadewegner.blob.core.windows.net/wordpress/2011/02/Picture-0.png)

Once you select **Service Bus, Access Control & Caching**, you’ll land on the primary Windows Azure AppFabric page, where you can get access to SDKs, documentation, and other help & resources.

![Picture 1](https://wadewegner.blob.core.windows.net/wordpress/2011/02/Picture-1.png)

Be sure download the updated Windows Azure AppFabric SDK – you can find it at <http://go.microsoft.com/fwlink/?LinkID=184288> (or click the link on the portal).

> **Note**: Make sure you don’t have the Windows Server AppFabric Cache already installed on your machine. While the API is symmetric, the current assemblies are not compatible. In fact, the Windows Server AppFabric GAC’s the Caching assemblies, so your application will load the wrong assemblies. This will be resolved by the time the service goes into production, but for now it causes a bit of friction.

![image](https://wadewegner.blob.core.windows.net/wordpress/2011/02/image.png)

To create a Cache, select the **Cache** under **AppFabric** …

![image](https://wadewegner.blob.core.windows.net/wordpress/2011/02/image1.png)

… and then click **New Namespace**. In doing so you will have to create a labs subscription.

![Picture 2.5](https://wadewegner.blob.core.windows.net/wordpress/2011/02/Picture-2.5.png)

Note that if you do not select **Cache** and instead stay at the top of the tree on **AppFabric** you will only create **Access Control** and **Service Bus** in your namespace.

In the CTP, you only have control over the **Service Namespace** and the **Cache Size** – the other options are bound to the defaults.

![Picture 3a](https://wadewegner.blob.core.windows.net/wordpress/2011/02/Picture-3a.png)

For the option size, your choices are **128MB Cache** and **256MB Cache**. When this service moves into production, more cache sizes will be made available. Once you have made your selection, click **OK** to create your cache.

![Picture 6](https://wadewegner.blob.core.windows.net/wordpress/2011/02/Picture-6.png)

It only takes about 10-15 seconds to provision your cache in the background. When complete, you have a fully functional, distributed cache available to your applications. That’s all it takes! This is the magic of the Platform-as-a-Service model – if you were to have done this yourself in a set of Windows Azure instances, I guarantee it would take you a LOT more than 10-15 seconds.

![Picture 7](https://wadewegner.blob.core.windows.net/wordpress/2011/02/Picture-7.png)

Once the cache is provisioned, you can expand the **Properties** to get specific information about your cache. This provides you useful information such as **Namespace**, **Service URL**, **Service Port**, and **Authentication Token**.

![Picture 10a](https://wadewegner.blob.core.windows.net/wordpress/2011/02/Picture-10a.png)

There are a few more capabilities now available from the portal – specifically, **Access Control Service**, **View Client Configuration**, and **Change Cache Size**. If you take a look at **Change Cache Size**, you’ll see that you can switch between a 128MB and 256 MB Cache once a day (note that the functionality is disabled if you’ve already performed this operation).

![Picture 9a](https://wadewegner.blob.core.windows.net/wordpress/2011/02/Picture-9a.png)

In addition to changing the cache size, you can also grab the client configuration XML from **View Client Configuration**.

![Picture 8a](https://wadewegner.blob.core.windows.net/wordpress/2011/02/Picture-8a.png)

This configuration is extremely valuable for when you’re developing your application, as you can leverage this code directly in your App.Config or Web.Config file. In fact, go ahead

To begin, let’s create a simple Console Application using C#. Once created, be sure and update the project so that it targets the full .NET Framework instead of the Client Profile. You’ll also need to add the Caching assemblies, which can typically be found under C:Program FilesWindows Azure AppFabric SDKV2.0AssembliesCache. For now, add the following two assemblies:

- Microsoft.ApplicationServer.Caching.Client
- Microsoft.ApplicationServer.Caching.Core

Open up the App.Config and paste in the configuration from the portal. Once added, it should look like the following:

```
  1. <?xml version="1.0"?>
  2. <configuration>
  3.   <configSections>
  4.     <section name="dataCacheClient" type="Microsoft.ApplicationServer.Caching.DataCacheClientSection, Microsoft.ApplicationServer.Caching.Core" allowLocation="true" allowDefinition="Everywhere"/>
  5.   </configSections>
  6.   <dataCacheClient deployment="Simple">
  7.     <hosts>
  8.       <host name="YOURCACHE.cache.appfabriclabs.com" cachePort="22233" />
  9.     </hosts>
  10.     <securityProperties mode="Message">
  11.       <messageSecurity
  12.           authorizationInfo="YOURTOKEN">
  13.       </messageSecurity>
  14.     </securityProperties>
  15.   </dataCacheClient>
  16. </configuration>
```

Now let’s update the Main method of our Console application with a very simple demonstration:

```
  1. static void Main(string[] args)
  2. {
  3.     // setup the DataCacheFactory using the app.config settings
  4.     DataCacheFactory dataCacheFactory = new DataCacheFactory();

  6.     // get the default cache client
  7.     DataCache dataCache = dataCacheFactory.GetDefaultCache();

  9.     // enter a value to store in the cache
  10.     Console.Write("Enter a value: ");
  11.     string value = Console.ReadLine();

  13.     // put your value in the cache
  14.     dataCache.Put("key", value);

  16.     // get your value out of the cache
  17.     string response = (string)dataCache.Get("key");

  19.     // write the value
  20.     Console.WriteLine("Your value: " + response);
  21. }
```

You can see that all we have to do is create a new instance of the DataCacheFactory, and all the configuration settings in the app.config file are read in by default. The end result is a simple Console application:

![Picture 11](https://wadewegner.blob.core.windows.net/wordpress/2011/02/Picture-11.png)

A simple demonstration, but hopefully valuable.

Please take a look at the [Windows Azure AppFabric CTP February release](http://portal.appfabriclabs.com/) as soon as possible, and explore the new capabilities and features provided in the Caching service.
