---
title: "Programmatically Configuring the Caching Client"
date: 2010-12-02T00:00:00-08:00
draft: false
description: "From the whiteboard to the keyboard"
---

When using [Windows Azure AppFabric Caching](http://www.microsoft.com/en-us/appfabric/azure/middleware-services.aspx#Caching) you will have to configure the caching client to use a provisioned cache. In the past I’ve given examples of doing this [using an application configuration file](http://www.wadewegner.com/2010/11/code-for-the-windows-azure-appfabric-caching-demo/), but there are times where you may want to programmatically configure the caching client. Fortunately, it’s really quite easy and straight forward.

Caching Client Code

1. // define the cache details
2. string serviceUrl = “YOURNAMESPACE.cache.appfabriclabs.com”;
3. string authorizationToken = “YOURTOKEN”;
5. // declare an array for the cache host
6. List server = new List();
7. server.Add(new DataCacheServerEndpoint(serviceUrl, 22233));
9. // setup the DataCacheFactory configuration
10. DataCacheFactoryConfiguration conf = new DataCacheFactoryConfiguration();
11. conf.IsRouting = false;
12. conf.SecurityProperties = new DataCacheSecurity(authorizationToken);
13. conf.Servers = server;
14. conf.RequestTimeout = new TimeSpan(0, 0, 45);
15. conf.ChannelOpenTimeout = new TimeSpan(0, 0, 45);
17. // create the DataCacheFactor based on config settings
18. DataCacheFactory dataCacheFactory = new DataCacheFactory(conf);
20. // get the default cache client
21. DataCache dataCache = dataCacheFactory.GetDefaultCache();
23. // put and retrieve a test object
24. dataCache.Put(“key”, “1”);
25. string value = dataCache.Get(“key”) as string;
27. Console.WriteLine(“Your value: " + value);

Here are some of the more interesting pieces:

- Line 6: Create a DataCacheServerEndpoint list that will be used in the DataCacheFactoryConfiguration (see line 13).
- Line 7: Add the endpoint (defined by the service URL and port) to the list.
- Line 12: Set a DataCacheSecurity object using the authorization token for the SecurityProperties property.
- Line 13: Set the DataCacheServerEndpoint list to the Servers property.
- Line 18: Create a new DataCacheFactory and pass the DataCacheFactoryConfiguration object to the constructor.

Before getting started, but sure to [prepare Visual Studio](http://msdn.microsoft.com/en-us/library/gg278344.aspx) and [provision or access](http://msdn.microsoft.com/en-us/library/gg278349.aspx) your Cache to get the service URL and the authorization token.
