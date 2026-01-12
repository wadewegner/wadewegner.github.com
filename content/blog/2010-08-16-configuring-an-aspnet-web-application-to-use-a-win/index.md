---
title: "Configuring an ASP.NET Web Application to Use a Windows Server AppFabric Cache for Session State"
date: 2010-08-16T00:00:00-08:00
draft: false
description: "From the whiteboard to the keyboard"
---

Last week I spent some time [setting up Windows Server AppFabric Cache](http://www.wadewegner.com/2010/08/getting-started-with-windows-server-appfabric-cache/) in anticipation of additional tasks this week. The first task is configuring an ASP.NET web application to use Windows Server AppFabric Caching for the Session State Provider. This allows the web application to spread session objects across the entire cache cluster, resulting in greater scalability.

Below is a walkthrough on how to configure this scenario. In addition to this post, I recommend you take a look at [this article on MSDN](http://msdn.microsoft.com/en-us/library/ee790859.aspx).

1. Thoroughly review [Getting Started with Windows Server AppFabric Cache](http://www.wadewegner.com/2010/08/getting-started-with-windows-server-appfabric-cache/) to get everything setup.
2. Open up the **Cache PowerShell** console (Start –> Windows Server AppFabric –> Caching Administration Windows PowerShell). This will automatically import the DistributedCacheAdministration module and use the CacheCluster.
3. Start the Cache Cluster (if not already started). Run the following command in the PowerShell console:

   ```
    Start-CacheCluster
   ```
4. Create a new cache that you will leverage for your session state provider. Run the following command in the PowerShell console:

   ```
    New-Cache MySessionStateCache
   ```
5. Create a **new ASP.NET Web Application** in **Visual Studio 2010** targeting .NET 4.0. This will create a sample project, complete with master page which we’ll leverage later on.
6. Add references to the **Microsoft.ApplicationServer.Caching.Client** and **Microsoft.ApplicationServer.Caching.Core**. To do this, use the following steps (thanks to [Ron Jacobs for the insight](http://blogs.msdn.com/b/rjacobs/archive/2010/03/04/how-to-add-a-reference-to-microsoft-applicationserver-caching-client.aspx)):

   1. Right-click on your project and select **Add Reference**.
   2. Select the **Browse** tab.
   3. Enter the following folder name, and press enter:

      ```
       %windir%\Sysnative\AppFabric
      ```
   4. Locate and select both **Microsoft.ApplicationServer.Caching.Client** and **Microsoft.ApplicationServer.Caching.Core** assemblies.
7. Add the **configSections** element to the **web.config** file as the very first element element in the configuration element:

   ```
    <configSections>

    <section name="dataCacheClient" type="Microsoft.ApplicationServer.Caching.DataCacheClientSection,
        Microsoft.ApplicationServer.Caching.Core, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35"
        allowLocation="true" allowDefinition="Everywhere"/>

    </configSections>
   ```
8. Add the **dataCacheClient** element to the **web.config** file, after the configSections element. Be sure to replace **YOURHOSTNAME** with the name of your cache host. In the PowerShell console you can get the HostName (and CachePort) by starting or restarting your cache).

   ```
    <dataCacheClient>
        <hosts>
            <host
                name="YOURHOSTNAME"
                cachePort="22233"/>
        </hosts>
    </dataCacheClient>
   ```
9. Add the **sessionState** element to the **web.config** file in the system.web element. Be sure that the **cacheName** is the same as the cache you created in step 4.

   ```
    <sessionState mode="Custom" customProvider="AppFabricCacheSessionStoreProvider">
        <providers>
            <add name="AppFabricCacheSessionStoreProvider"
                type="Microsoft.ApplicationServer.Caching.DataCacheSessionStoreProvider"
                cacheName="MySessionStateCache"
                sharedId="SharedApp"/>
        </providers>
    </sessionState>
   ```
10. Now, we need a quick and easy way to test this. There are many ways to do this, below is mine. I loaded data into session, then created a button that writes the session data into a JavaScript alert. Quick and easy:

    ```
    protected void Page_Load(object sender, EventArgs e)
    {
        // Store information into session
        Session["PageLoadDateTime"] = DateTime.Now.ToString();

        // Reference the ContentPlaceHoler on the master page
        ContentPlaceHolder mpContentPlaceHolder =
            (ContentPlaceHolder)Master.FindControl("MainContent");

        if (mpContentPlaceHolder != null)
        {
            // Register the button
            mpContentPlaceHolder.Controls.Add(GetButton("btnDisplayPageLoadDateTime", "Click Me"));
        }
    }

    // Define the button
    private Button GetButton(string id, string name)
    {
        Button b = new Button();
        b.Text = name;
        b.ID = id;
        b.OnClientClick = "alert('PageLoadDateTime defined at " + Session["PageLoadDateTime"] + "')";
        return b;
    }
    ```
11. Now, hit **control-F5** to start your project. After it loads, click the button labeled “Click Me” – you should see the following alert:

    ![ASP.NET using Cache for Session State](https://wadewegner.blob.core.windows.net/wordpress/2010/08/image9.png)

That’s it! You have now configured your ASP.NET web application to leverage Windows Server AppFabric Cache to store all Session State.

While I was putting this together, I encountered two errors. I figured I’d share them here, along with resolution, in case any of you encounter the same problems along the way.

```
Configuration Error

Description: An error occurred during the processing of a configuration
file required to service this request. Please review the specific error
details below and modify your configuration file appropriately.
Parser Error Message: ErrorCode<ERRCA0009>:SubStatus<ES0001>:Cache
referred to does not exist. Contact administrator or use the Cache
administration tool to create a Cache.
```

If you received the above error message, it’s likely that the cacheName specified in the sessionState element is wrong. Update the cacheName to reflect the cache you created in step #4.

```
Configuration Error

Description: An error occurred during the processing of a configuration
file required to service this request. Please review the specific error
details below and modify your configuration file appropriately.
Parser Error Message: ErrorCode<ERRCA0017>:SubStatus<ES0006>:There is
a temporary failure. Please retry later. (One or more specified Cache
servers are unavailable, which could be caused by busy network or
servers. Ensure that security permission has been granted for this
client account on the cluster and that the AppFabric Caching Service
is allowed through the firewall on all cache hosts. Retry later.)
```

If you received the above error message, it’s likely that the host name specified in the dataCacheClient is wrong. Update the dataCacheClient host name to reflect the name of your host. Note: it’s likely that it’s just your machine name.

Hope this help!
