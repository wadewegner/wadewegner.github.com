---
author: admin
comments: true
date: 2011-02-18 06:00:48+00:00
layout: post
slug: running-multiple-websites-in-a-windows-azure-web-role
title: Running Multiple Websites in a Windows Azure Web Role
wordpress_id: 1062
categories:
- Windows Azure
---

Prior to the release of the [Windows Azure 1.3 SDK](http://www.microsoft.com/downloads/en/details.aspx?FamilyID=7a1089b6-4050-4307-86c4-9dadaa5ed018), Web roles hosted your application in [IIS Hosted Web Core (HWC)](http://technet.microsoft.com/en-us/library/cc735238(WS.10).aspx). While there were certainly ways to extend HWC, for the most part you were stuck with a single application bound to no more than a single HTTP or HTTPS endpoint. This model enabled only a minimal number of configurations, and prevented you from fully benefiting from the full capabilities of IIS. Here’s what the **ServiceDefinition.csdef** file looks like when using HWC:

 
    
      <span style="color: blue"><?</span><span style="color: #a31515">xml </span><span style="color: red">version</span><span style="color: blue">=</span>"<span style="color: blue">1.0</span>" <span style="color: red">encoding</span><span style="color: blue">=</span>"<span style="color: blue">utf-8</span>"<span style="color: blue">?>
      <</span><span style="color: #a31515">ServiceDefinition </span><span style="color: red">name</span><span style="color: blue">=</span>"<span style="color: blue">WindowsAzureProject15</span>" 
         <span style="color: red">xmlns</span><span style="color: blue">=</span>"<span style="color: blue">http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition</span>"<span style="color: blue">>
        <</span><span style="color: #a31515">WebRole </span><span style="color: red">name</span><span style="color: blue">=</span>"<span style="color: blue">WebRole1</span>"<span style="color: blue">>
          <</span><span style="color: #a31515">Endpoints</span><span style="color: blue">>
            <</span><span style="color: #a31515">InputEndpoint </span><span style="color: red">name</span><span style="color: blue">=</span>"<span style="color: blue">Endpoint1</span>" <span style="color: red">protocol</span><span style="color: blue">=</span>"<span style="color: blue">http</span>" <span style="color: red">port</span><span style="color: blue">=</span>"<span style="color: blue">80</span>" <span style="color: blue">/>
          </</span><span style="color: #a31515">Endpoints</span><span style="color: blue">>
          <</span><span style="color: #a31515">Imports</span><span style="color: blue">>
            <</span><span style="color: #a31515">Import </span><span style="color: red">moduleName</span><span style="color: blue">=</span>"<span style="color: blue">Diagnostics</span>" <span style="color: blue">/>
          </</span><span style="color: #a31515">Imports</span><span style="color: blue">>
        </</span><span style="color: #a31515">WebRole</span><span style="color: blue">>
      </</span><span style="color: #a31515">ServiceDefinition</span><span style="color: blue">>
    </span>






While you can still use this code today and run HWC in your Web role, you’ll miss you on a lot of great capabilities.





One of the capabilities that, surprisingly, people still doesn’t seem aware of in Windows Azure is the ability to **run multiple websites in a Web Role**. In fact, this capability is easy to add once we add seven additional lines of code.




    
      <span style="color: blue"><?</span><span style="color: #a31515">xml </span><span style="color: red">version</span><span style="color: blue">=</span>"<span style="color: blue">1.0</span>" <span style="color: red">encoding</span><span style="color: blue">=</span>"<span style="color: blue">utf-8</span>"<span style="color: blue">?>
      <</span><span style="color: #a31515">ServiceDefinition </span><span style="color: red">name</span><span style="color: blue">=</span>"<span style="color: blue">WindowsAzureProject15</span>" 
         <span style="color: red">xmlns</span><span style="color: blue">=</span>"<span style="color: blue">http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition</span>"<span style="color: blue">>
        <</span><span style="color: #a31515">WebRole </span><span style="color: red">name</span><span style="color: blue">=</span>"<span style="color: blue">WebRole1</span>"<span style="color: blue">>
          <</span><span style="color: #a31515">Sites</span><span style="color: blue">>
            <</span><span style="color: #a31515">Site </span><span style="color: red">name</span><span style="color: blue">=</span>"<span style="color: blue">Web</span>"<span style="color: blue">>
              <</span><span style="color: #a31515">Bindings</span><span style="color: blue">>
                <</span><span style="color: #a31515">Binding </span><span style="color: red">name</span><span style="color: blue">=</span>"<span style="color: blue">Endpoint1</span>" <span style="color: red">endpointName</span><span style="color: blue">=</span>"<span style="color: blue">Endpoint1</span>" <span style="color: blue">/>
              </</span><span style="color: #a31515">Bindings</span><span style="color: blue">>
            </</span><span style="color: #a31515">Site</span><span style="color: blue">>
          </</span><span style="color: #a31515">Sites</span><span style="color: blue">>
          <</span><span style="color: #a31515">Endpoints</span><span style="color: blue">>
            <</span><span style="color: #a31515">InputEndpoint </span><span style="color: red">name</span><span style="color: blue">=</span>"<span style="color: blue">Endpoint1</span>" <span style="color: red">protocol</span><span style="color: blue">=</span>"<span style="color: blue">http</span>" <span style="color: red">port</span><span style="color: blue">=</span>"<span style="color: blue">80</span>" <span style="color: blue">/>
          </</span><span style="color: #a31515">Endpoints</span><span style="color: blue">>
          <</span><span style="color: #a31515">Imports</span><span style="color: blue">>
            <</span><span style="color: #a31515">Import </span><span style="color: red">moduleName</span><span style="color: blue">=</span>"<span style="color: blue">Diagnostics</span>" <span style="color: blue">/>
          </</span><span style="color: #a31515">Imports</span><span style="color: blue">>
        </</span><span style="color: #a31515">WebRole</span><span style="color: blue">>
      </</span><span style="color: #a31515">ServiceDefinition</span><span style="color: blue">>
    </span>






The difference here is found in lines **#4** through **#10**. We now have a **<Sites>** element that includes a default **<Site> **named “Web”. When run (locally or in Windows Azure), this translates into an actual web site running in IIS.





[![Deployment](http://images.wadewegner.com/wordpress/2011/11/Deployment_thumb.jpg)](http://images.wadewegner.com/wordpress/2011/11/Deployment.jpg)





You can see that our website is now running in IIS. And what’s nice is that the syntax used in the **ServiceDefinition.csdef **file is nearly identical to how this is traditionally accomplished in the [**system.applicationHost**](http://www.iis.net/ConfigReference/system.applicationHost/sites) – there’s not much new we have to learn.





**Run the Same Project in Two Sites in the Web Role**





So, how can we take this further and run a second website in a Web role? It’s as simple as adding an additional **<Site>** to the **<Sites>** element.





Copy lines **#5** through **#9 **above, and past them below line **#9**. You’re going to have to make a few updates:






  
  1. Change the site name. You can’t have multiple sites with the same name. 


  
  2. You’ll have to specify a physical directory for the second site. 





> 
  
> 
> **Note:** the only reason we don’t have to specify a physical path for the first site is because “Web” is consider a special case, and Visual Studio knows that it’s referring to the path of the Web role project. You can confirm this by renaming “Web” to “Web1” – once you do this you’ll have to specify a physical path for the site.
> 
> 







  
  1. Create a host header entry for the binding element. The reason for this is because both sites are listening on port 80. This is the same behavior you’ll find in IIS – you cannot have more than one site listening on the same port without adding a host header. 





Once you make these modifications, your <Sites> element will look something like this:




    
      <span style="color: blue"><</span><span style="color: #a31515">Sites</span><span style="color: blue">>
        <</span><span style="color: #a31515">Site </span><span style="color: red">name</span><span style="color: blue">=</span>"<span style="color: blue">Web</span>"<span style="color: blue">>
          <</span><span style="color: #a31515">Bindings</span><span style="color: blue">>
            <</span><span style="color: #a31515">Binding </span><span style="color: red">name</span><span style="color: blue">=</span>"<span style="color: blue">Endpoint1</span>" <span style="color: red">endpointName</span><span style="color: blue">=</span>"<span style="color: blue">Endpoint1</span>" <span style="color: blue">/>
          </</span><span style="color: #a31515">Bindings</span><span style="color: blue">>
        </</span><span style="color: #a31515">Site</span><span style="color: blue">>
        <</span><span style="color: #a31515">Site </span><span style="color: red">name</span><span style="color: blue">=</span>"<span style="color: blue">Web2</span>" <span style="color: red">physicalDirectory</span><span style="color: blue">=</span>"<span style="color: blue">..\WebRole1</span>"<span style="color: blue">>
          <</span><span style="color: #a31515">Bindings</span><span style="color: blue">>
            <</span><span style="color: #a31515">Binding </span><span style="color: red">name</span><span style="color: blue">=</span>"<span style="color: blue">Endpoint1</span>" <span style="color: red">endpointName</span><span style="color: blue">=</span>"<span style="color: blue">Endpoint1</span>" <span style="color: red">hostHeader</span><span style="color: blue">=</span>"<span style="color: blue">www.litware.com</span>" <span style="color: blue">/>
          </</span><span style="color: #a31515">Bindings</span><span style="color: blue">>
        </</span><span style="color: #a31515">Site</span><span style="color: blue">>
      </</span><span style="color: #a31515">Sites</span><span style="color: blue">>
    </span>






While this syntax is correct, [www.litware.com](http://www.litware.com) will not locally resolve to our application. To make this work, we have to update our hosts file to assist in correctly resolving the address to 127.0.0.1 (the loopback address). Update the hosts file (C:WindowsSystem32driversetchosts) to include the following hostnames:




    
    127.0.0.1 www.fabrikam.com
    127.0.0.1 www.contoso.com
    127.0.0.1 www.litware.com






After you’ve done this, you can confirm that they’re resolving correctly by pinging one of the addresses:





[![image](http://images.wadewegner.com/wordpress/2011/02/image_thumb3.png)](http://images.wadewegner.com/wordpress/2011/02/image3.png)





The reply should come from 127.0.0.1.





With this complete, hit **F5** and run again. You’re now running the same project in two different websites – take a look at IIS:





[![image](http://images.wadewegner.com/wordpress/2011/02/image_thumb4.png)](http://images.wadewegner.com/wordpress/2011/02/image4.png)





In fact, if you take a look at the bindings on **Web2**, you’ll see that the host name has been set to [www.litware.com](http://www.litware.com).





[![image](http://images.wadewegner.com/wordpress/2011/02/image_thumb5.png)](http://images.wadewegner.com/wordpress/2011/02/image5.png)





And if you type [http://www.litware.com:81/](http://www.litware.com:81/) (or whatever port the compute emulator is using), you’ll see the website display.





[![image](http://images.wadewegner.com/wordpress/2011/02/image_thumb6.png)](http://images.wadewegner.com/wordpress/2011/02/image6.png)





This is pretty cool, especially when want to use the same application for multiple websites – you can handle the requests in such a way that you either pull from different databases or even display different CSS files to make the website display differently.





> 
  
> 
> **Note:** Some of you may have noticed above that IIS showed [www.litware.com](http://www.litware.com) as running under port 5100. You may be wondering why it’s not on port 81, which is what we’re browsing to in IE. This is because it’s the compute emulator that’s listening on port 81, and then routing to port 5100. This is necessary so that incase we’re running more than one instance of our web application, both of which have the same host header, they’re not running on the same port. Nifty, eh?
> 
> 






Now we can even go further. Let’s take a look at how we can take a completely separate web application and run it in the same Web role.





**Run Two Different Projects in the Web Role**





Open up a new instance of Visual Studio 2010, and create a new Web Application. I’d recommend you make a few changes to make it easily recognizable (e.g. change “Welcome to ASP.NET!” to something else). Be sure to **Build** your solution (failure to do this will cause problems later). Copy the **Project Folder** for the project, and then return to the **ServiceDefinition.csdef** file in your Windows Azure project.





Create a new **<Site>** element where the **physicalDirectory** value is the location of the **Project Folder** you copied a moment ago. Also, update the site name and the **hostHoder**. It should look something like this:




    
      <span style="color: blue"><</span><span style="color: #a31515">Site </span><span style="color: red">name</span><span style="color: blue">=</span>"<span style="color: blue">Web3</span>" <span style="color: red">physicalDirectory</span><span style="color: blue">=</span>"<span style="color: blue">c:\\Projects\\WebApplication7\\WebApplication7</span>"<span style="color: blue">>
        <</span><span style="color: #a31515">Bindings</span><span style="color: blue">>
          <</span><span style="color: #a31515">Binding </span><span style="color: red">name</span><span style="color: blue">=</span>"<span style="color: blue">Endpoint1</span>" <span style="color: red">endpointName</span><span style="color: blue">=</span>"<span style="color: blue">Endpoint1</span>" <span style="color: red">hostHeader</span><span style="color: blue">=</span>"<span style="color: blue">www.fabrikam.com</span>" <span style="color: blue">/>
        </</span><span style="color: #a31515">Bindings</span><span style="color: blue">>
      </</span><span style="color: #a31515">Site</span><span style="color: blue">>
    </span>






We’ve now create a third website, but this time – instead of pointing to the Web role project in our solution – we’ve pointed to a completely separate project that lives outside of our solution. Hit F5 and take a look at IIS:





[![image](http://images.wadewegner.com/wordpress/2011/02/image_thumb7.png)](http://images.wadewegner.com/wordpress/2011/02/image7.png)





We now have three sites running, the third which includes the files from “WebApplication7”. Try it out by updating the URL to [http://www.fabrikam.com:81/](http://www.fabrikam.com:81/) and you’ll see:





[![image](http://images.wadewegner.com/wordpress/2011/02/image_thumb8.png)](http://images.wadewegner.com/wordpress/2011/02/image8.png)





You can see that this is the not the Web role project itself, but the second project we created. The packaging tools know to include all the files needed from the **physicalDirectory** location and make them a part of the CSPKG that is ultimately given to the fabric controller.





This is pretty awesome, cause it demonstrates that you can run two completely different websites in the same Web role with a minimal amount of work.





Now, let’s take this even one step further … because we can.





**Run a Virtual Application Within a Site**





There may be times when you want to run a **Virtual Application** within your site (see [Wenlong Dong’s post Virtual Application vs Virtual Directory](http://blogs.msdn.com/b/wenlong/archive/2006/11/22/virtual-application-vs-virtual-directory.aspx) for some interesting information). Given that the semantics of the <Sites> element is similar to system.applicationHost, it’s not hard to accomplish.





Create a brand new **Web Application**, and this time update **<h2>** with a different description (e.g. “This is my virtual application!”). Build the solution, then grab the project’s path. Return to your Windows Azure project and the **ServiceDefinition.csdef **file. We’re going to add this virtual application to the “Web3” site. We do this by adding a **<VirtualApplication>** element with name and **physicalDirectory** values. Here’s what it will look like:




    
      <span style="color: blue"><</span><span style="color: #a31515">Site </span><span style="color: red">name</span><span style="color: blue">=</span>"<span style="color: blue">Web3</span>" 
            <span style="color: red">physicalDirectory</span><span style="color: blue">=</span>"<span style="color: blue">c:\\Projects\\WebApplication7\\WebApplication7</span>"<span style="color: blue">>
        <</span><span style="color: #a31515">VirtualApplication </span><span style="color: red">name</span><span style="color: blue">=</span>"<span style="color: blue">VirtualApp</span>" 
            <span style="color: red">physicalDirectory</span><span style="color: blue">=</span>"<span style="color: blue">c:\\Projects\\WebApplication8\\WebApplication8</span>" <span style="color: blue">/>
        <</span><span style="color: #a31515">Bindings</span><span style="color: blue">>
          <</span><span style="color: #a31515">Binding </span><span style="color: red">name</span><span style="color: blue">=</span>"<span style="color: blue">Endpoint1</span>" <span style="color: red">endpointName</span><span style="color: blue">=</span>"<span style="color: blue">Endpoint1</span>" <span style="color: red">hostHeader</span><span style="color: blue">=</span>"<span style="color: blue">www.fabrikam.com</span>" <span style="color: blue">/>
        </</span><span style="color: #a31515">Bindings</span><span style="color: blue">>
      </</span><span style="color: #a31515">Site</span><span style="color: blue">>
    </span>






The **<VirtualApplication>** element is on line **#3**. Go ahead and hit F5, and take a look at IIS:





[![image](http://images.wadewegner.com/wordpress/2011/02/image_thumb9.png)](http://images.wadewegner.com/wordpress/2011/02/image9.png)





You can see that now, in Web3, our virtual application called **VirtualApp** is running. And if we browse to [http://www.fabrikam.com:81/virtualapp/](http://www.fabrikam.com:81/virtualapp/), we’ll see that it is indeed “WebApplication8” that’s running.





[![image](http://images.wadewegner.com/wordpress/2011/02/image_thumb10.png)](http://images.wadewegner.com/wordpress/2011/02/image10.png)





So there you have it!





You’ve now seen how to:






  
  1. Run multiple websites that run off of the same project in the same Web role. 


  
  2. Run multiple websites that use different projects in the same Web role. 


  
  3. Run a virtual application within a website that uses a different project in a Web role. 





Of course, when you deploy this to Windows Azure, you’ll want to use your own domain name and set the host headers accordingly. For tips on how to do this – especially as as it relates to setting up CNAMEs for your domain – take a look at Steve Marx’s post on [Custom Domain Names in Windows Azure](http://blog.smarx.com/posts/custom-domain-names-in-windows-azure).
