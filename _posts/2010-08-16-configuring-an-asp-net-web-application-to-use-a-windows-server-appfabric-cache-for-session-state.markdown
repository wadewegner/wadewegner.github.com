---
author: admin
comments: true
date: 2010-08-16 22:36:55+00:00
layout: post
slug: configuring-an-asp-net-web-application-to-use-a-windows-server-appfabric-cache-for-session-state
title: Configuring an ASP.NET Web Application to Use a Windows Server AppFabric Cache
  for Session State
wordpress_id: 749
categories:
- .NET 4.0
- ASP.NET
- Cache
- Windows Server AppFabric
---

Last week I spent some time [setting up Windows Server AppFabric Cache](http://www.wadewegner.com/2010/08/getting-started-with-windows-server-appfabric-cache/) in anticipation of additional tasks this week. The first task is configuring an ASP.NET web application to use Windows Server AppFabric Caching for the Session State Provider. This allows the web application to spread session objects across the entire cache cluster, resulting in greater scalability.

Below is a walkthrough on how to configure this scenario. In addition to this post, I recommend you take a look at [this article on MSDN](http://msdn.microsoft.com/en-us/library/ee790859.aspx).

  1. Thoroughly review [Getting Started with Windows Server AppFabric Cache](http://www.wadewegner.com/2010/08/getting-started-with-windows-server-appfabric-cache/) to get everything setup.  
  2. Open up the **Cache PowerShell** console (Start –> Windows Server AppFabric –> Caching Administration Windows PowerShell). This will automatically import the DistributedCacheAdministration module and use the CacheCluster.  
  3. Start the Cache Cluster (if not already started). Run the following command in the PowerShell console:   

    
    Start-CacheCluster



  4. Create a new cache that you will leverage for your session state provider. Run the following command in the PowerShell console: 

    
    New-Cache MySessionStateCache



  5. Create a **new ASP.NET Web Application** in **Visual Studio 2010** targeting .NET 4.0. This will create a sample project, complete with master page which we’ll leverage later on. 

  6. Add references to the **Microsoft.ApplicationServer.Caching.Client** and **Microsoft.ApplicationServer.Caching.Core**. To do this, use the following steps (thanks to [Ron Jacobs for the insight](http://blogs.msdn.com/b/rjacobs/archive/2010/03/04/how-to-add-a-reference-to-microsoft-applicationserver-caching-client.aspx)): 


    1. Right-click on your project and select **Add Reference**. 

    2. Select the **Browse** tab. 

    3. Enter the following folder name, and press enter: 

    
    %windir%SysnativeAppFabric



    4. Locate and select both **Microsoft.ApplicationServer.Caching.Client** and **Microsoft.ApplicationServer.Caching.Core** assemblies. 


  7. Add the **configSections** element to the **web.config** file as the very first element element in the configuration element: 

    
    <span style="color: #008000"><!--configSections must be the FIRST element --></span>
    
    
    <span style="color: #0000ff"><</span><span style="color: #800000">configSections</span><span style="color: #0000ff">></span>
    
    
      <span style="color: #008000"><!-- required to read the <dataCacheClient> element --></span>
    
    
      <span style="color: #0000ff"><</span><span style="color: #800000">section</span> <span style="color: #ff0000">name</span>=<span style="color: #0000ff">"dataCacheClient"</span>
    
    
            <span style="color: #ff0000">type</span>=<span style="color: #0000ff">"Microsoft.ApplicationServer.Caching.DataCacheClientSection,
    
    
              Microsoft.ApplicationServer.Caching.Core, Version=1.0.0.0, 
    
    
              Culture=neutral, PublicKeyToken=31bf3856ad364e35"</span>
    
    
            <span style="color: #ff0000">allowLocation</span>=<span style="color: #0000ff">"true"</span>
    
    
            <span style="color: #ff0000">allowDefinition</span>=<span style="color: #0000ff">"Everywhere"</span><span style="color: #0000ff">/></span>
    
    
    <span style="color: #0000ff"></</span><span style="color: #800000">configSections</span><span style="color: #0000ff">></span>



  8. Add the **dataCacheClient** element to the **web.config** file, after the configSections element. Be sure to replace **YOURHOSTNAME** with the name of your cache host. In the PowerShell console you can get the HostName (and CachePort) by starting or restarting your cache). 

    
    <span style="color: #0000ff"><</span><span style="color: #800000">dataCacheClient</span><span style="color: #0000ff">></span>
    
    
      <span style="color: #008000"><!-- cache host(s) --></span>
    
    
      <span style="color: #0000ff"><</span><span style="color: #800000">hosts</span><span style="color: #0000ff">></span>
    
    
        <span style="color: #0000ff"><</span><span style="color: #800000">host</span>
    
    
            <span style="color: #ff0000">name</span>=<span style="color: #0000ff">"YOURHOSTNAME"</span>
    
    
            <span style="color: #ff0000">cachePort</span>=<span style="color: #0000ff">"22233"</span><span style="color: #0000ff">/></span>
    
    
      <span style="color: #0000ff"></</span><span style="color: #800000">hosts</span><span style="color: #0000ff">></span>
    
    
    <span style="color: #0000ff"></</span><span style="color: #800000">dataCacheClient</span><span style="color: #0000ff">></span>



  9. Add the **sessionState** element to the **web.config** file in the system.web element. Be sure that the **cacheName** is the same as the cache you created in step 4. 

    
    <span style="color: #0000ff"><</span><span style="color: #800000">sessionState</span> <span style="color: #ff0000">mode</span>=<span style="color: #0000ff">"Custom"</span> <span style="color: #ff0000">customProvider</span>=<span style="color: #0000ff">"AppFabricCacheSessionStoreProvider"</span><span style="color: #0000ff">></span>
    
    
      <span style="color: #0000ff"><</span><span style="color: #800000">providers</span><span style="color: #0000ff">></span>
    
    
        <span style="color: #008000"><!-- specify the named cache for session data --></span>
    
    
        <span style="color: #0000ff"><</span><span style="color: #800000">add</span>
    
    
          <span style="color: #ff0000">name</span>=<span style="color: #0000ff">"AppFabricCacheSessionStoreProvider"</span>
    
    
          <span style="color: #ff0000">type</span>=<span style="color: #0000ff">"Microsoft.ApplicationServer.Caching.DataCacheSessionStoreProvider"</span>
    
    
          <span style="color: #ff0000">cacheName</span>=<span style="color: #0000ff">"MySessionStateCache"</span>
    
    
          <span style="color: #ff0000">sharedId</span>=<span style="color: #0000ff">"SharedApp"</span><span style="color: #0000ff">/></span>
    
    
      <span style="color: #0000ff"></</span><span style="color: #800000">providers</span><span style="color: #0000ff">></span>
    
    
    <span style="color: #0000ff"></</span><span style="color: #800000">sessionState</span><span style="color: #0000ff">></span>



  10. Now, we need a quick and easy way to test this. There are many ways to do this, below is mine. I loaded data into session, then created a button that writes the session data into a JavaScript alert. Quick and easy: 

    
    <span style="color: #0000ff">protected</span> <span style="color: #0000ff">void</span> Page_Load(<span style="color: #0000ff">object</span> sender, EventArgs e)
    
    
    {
    
    
        <span style="color: #008000">// Store information into session</span>
    
    
        Session["<span style="color: #8b0000">PageLoadDateTime</span>"] = DateTime.Now.ToString();
    
    
        <span style="color: #008000">// Reference the ContentPlaceHoler on the master page</span>
    
    
        ContentPlaceHolder mpContentPlaceHolder = 
    
    
            (ContentPlaceHolder)Master.FindControl("<span style="color: #8b0000">MainContent</span>");
    
    
        <span style="color: #0000ff">if</span> (mpContentPlaceHolder != <span style="color: #0000ff">null</span>)
    
    
        {
    
    
            <span style="color: #008000">// Register the button</span>
    
    
            mpContentPlaceHolder.Controls.Add(
    
    
                GetButton("<span style="color: #8b0000">btnDisplayPageLoadDateTime</span>", "<span style="color: #8b0000">Click Me</span>"));
    
    
        }
    
    
    }
    
    
    <span style="color: #008000">// Define the button</span>
    
    
    <span style="color: #0000ff">private</span> Button GetButton(<span style="color: #0000ff">string</span> id, <span style="color: #0000ff">string</span> name)
    
    
    {
    
    
        Button b = <span style="color: #0000ff">new</span> Button();
    
    
        b.Text = name;
    
    
        b.ID = id;
    
    
        b.OnClientClick = "<span style="color: #8b0000">alert('PageLoadDateTime defined at </span>" + 
    
    
            Session["<span style="color: #8b0000">PageLoadDateTime</span>"] + "<span style="color: #8b0000">')</span>";
    
    
        <span style="color: #0000ff">return</span> b;
    
    
    }



  11. Now, hit **control-F5** to start your project. After it loads, click the button labeled “Click Me” – you should see the following alert:   
[![ASP.NET using Cache for Session State](http://images.wadewegner.com/wordpress/2010/08/image_thumb6.png)](http://images.wadewegner.com/wordpress/2010/08/image9.png)



That’s it! You have now configured your ASP.NET web application to leverage Windows Server AppFabric Cache to store all Session State.




While I was putting this together, I encountered two errors. I figured I’d share them here, along with resolution, in case any of you encounter the same problems along the way.






    
    Configuration Error
    
    
      
    
    Description: An error occurred during the processing of a configuration
    
    
    file required to service this request. Please review the specific error
    
    
    details below and modify your configuration file appropriately.
    
    
    Parser Error Message: ErrorCode<ERRCA0009>:SubStatus<ES0001>:Cache
    
    
    referred to does not exist. Contact administrator or use the Cache
    
    
    administration tool to create a Cache.




  
If you received the above error message, it’s likely that the cacheName specified in the sessionState element is wrong. Update the cacheName to reflect the cache you created in step #4.



    
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




  
If you received the above error message, it’s likely that the host name specified in the dataCacheClient is wrong. Update the dataCacheClient host name to reflect the name of your host. Note: it’s likely that it’s just your machine name.




Hope this help!
