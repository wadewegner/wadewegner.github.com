---
author: admin
categories:
- Architecture
- Azure
- Cloud
- Windows Azure
comments: true
date: "2009-11-04T14:24:12Z"
slug: how-to-leverage-the-roleentrypoint-in-an-azure-web-role
title: How to Leverage the RoleEntryPoint in an Azure Web Role
wordpress_id: 397
---

![Windows Azure](https://wadewegner.blob.core.windows.net/wordpress/2009/11/WindowsAzure.png)

 One of the advantages to the approach our teams building the Windows Azure Platform have taken is flexibility. Recently, when I spoke at the [Day of Cloud presentation](http://www.wadewegner.com/index.php/2009/10/16/presenting-on-the-windows-azure-platform-at-the-day-of-cloud/), I recall Don Schwarz from Google making these two points (you can see video of his talk [here](http://www.blip.tv/file/2786812)):

 

  
  * You can’t spin up your own threads in Google App Engine. 
   
  * You build your applications according to how Google thinks your apps should be built (the argument being that Google knows how to run highly available services at scale, which I think is a fair statement). 
 

Now to be fair, there are good reasons for this – the Google App Engine has a number of very good use cases (Don Schwarz demonstrated one of them when showing the audience a multiplayer game running on Google App Engine).

 

>   
> 
> Note: I’d like state for the record that I mean no criticism of other cloud vendors (i.e. Amazon, Google, and SalesForce). I think each of them have a place in the market, and exhibit various strengths. That said, I do believe that the Windows Azure Platform stands out as the only real platform that can bridge the chasm between cloud services and on-premises software. (Note to self: back this statement up in a future blog post.)

 

I would argue, however, that most enterprise developers require a little more flexibility when building out enterprise class applications. The Windows Azure Platform provides this flexibility (I mean, come on – sometimes you just want to execute some native code!).

 

I decided to test how far I could take this flexibility in Windows Azure.

 

I am a big fan of [Worker Roles](http://msdn.microsoft.com/en-us/library/dd179341.aspx) in Windows Azure. I really like the idea of having an asynchronous, “headless” service that’s working for me in the background. I wanted to see if I could spin up an equivalent to a Worker Role in a [Web Role](http://msdn.microsoft.com/en-us/library/dd179341.aspx), so that in addition to having my Web application running in a Web Role I could also get a service running in the background.

 

Why would I want to do this? Well, here are a few of reasons:

 

  
  * Asynchronous logging service running in every Azure instance (whether a web or worker role); this service might take information from the event log and write it to an Azure table. 
   
  * Clean-up operations (i.e. temporary scratch writes, etc.) 
   
  * Opening and managing socket connections to the service bus (this is my favorite scenario). 
 

The list could go on. Hopefully you get the point.

 

So, how would you do this? Again, given the flexibility of the platform, there are many ways you can do this. Below you’ll find one simple way.

 

1. Create a new cloud services project, and add a web role.

 

2. Create a new class and inherit from Microsoft.ServiceHosting.ServiceRuntime.RoleEntryPoint. You'll have to override the Start and RoleStatus methods.

 
    
    <span style="color: #0000ff">using</span> Microsoft.ServiceHosting.ServiceRuntime;
    
    
    <span style="color: #0000ff">using</span> System.Threading;
    
    
    <span style="color: #0000ff">namespace</span> Web
    
    
    {
    
    
        <span style="color: #0000ff">public</span> <span style="color: #0000ff">class</span> MyWebWorkerRole : RoleEntryPoint
    
    
        {
    
    
            <span style="color: #0000ff">public</span> <span style="color: #0000ff">override</span> <span style="color: #0000ff">void</span> Start()
    
    
            {
    
    
                <span style="color: #0000ff">while</span> (<span style="color: #0000ff">true</span>)
    
    
                {
    
    
                    RoleManager.WriteToLog("<span style="color: #8b0000">Information</span>", "<span style="color: #8b0000">The worker role is running.</span>");
    
    
                    Thread.Sleep(5000);
    
    
                }
    
    
            }
    
    
            <span style="color: #0000ff">public</span> <span style="color: #0000ff">override</span> RoleStatus GetHealthStatus()
    
    
            {
    
    
                <span style="color: #0000ff">return</span> RoleStatus.Healthy;
    
    
            }
    
    
        }
    
    
    }





3. Create a Global.asax page.





4. Create a method where you initialize and start and initialize your class that inherits from RoleEntryPoint.




    
    <span style="color: #0000ff">private</span> <span style="color: #0000ff">void</span> StartWorkerRole()
    
    
    {
    
    
        MyWebWorkerRole myWebWorkerRole = <span style="color: #0000ff">new</span> MyWebWorkerRole();
    
    
        myWebWorkerRole.Initialize();
    
    
        myWebWorkerRole.Start();
    
    
    }





5. Create local threading variables that you will use to run your method.




    
    <span style="color: #0000ff">public</span> <span style="color: #0000ff">class</span> Global : System.Web.HttpApplication
    
    
    {
    
    
        System.Threading.ThreadStart ts;
    
    
        System.Threading.Thread t;
    
    
        ...





5. Update the Application_BeginRequest method to invoke your method on a new thread. Be sure and check to see if the thread is already running, otherwise it will get started multiple times.




    
    <span style="color: #0000ff">protected</span> <span style="color: #0000ff">void</span> Application_BeginRequest(<span style="color: #0000ff">object</span> sender, EventArgs e)
    
    
    {
    
    
        <span style="color: #0000ff">if</span> (t == <span style="color: #0000ff">null</span>)
    
    
        {
    
    
            ts = <span style="color: #0000ff">new</span> System.Threading.ThreadStart(StartWorkerRole);
    
    
            t = <span style="color: #0000ff">new</span> System.Threading.Thread(ts);
    
    
            t.Start();
    
    
        }
    
    
    }





And that should do it. Now you have a class that will run asynchronously in all your Web Role instances. And since it inherits from the RoleEntryPoint, you can leverage this technique for both your Web and Worker Role instances. I’d recommend placing this class in a separate class library and adding a reference to it from your various projects.





> 
  
> 
> Note: It is highly probable that there will be some changes to the APIs and assemblies come PDC. While these concepts will stay valid, the underlying code will probably change. If this happens, I’ll be sure and provide an update.
> 
> 






I hope this helps!
