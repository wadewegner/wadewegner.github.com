---
author: admin
comments: true
date: 2011-11-09 19:48:06+00:00
layout: post
slug: aggregating-rss-feeds-in-c-and-asp-net-mvc-3
title: Aggregating RSS Feeds in C# and ASP.NET MVC 3
wordpress_id: 1573
categories:
- ASP.NET
- C#
- MVC3
---

I’m working on a Windows Phone project that requires me to surface up multiple RSS feeds as a single source. I needed a way to do this quickly and easily, and with a little help from friends on Twitter (particularly a suggestion from [@bertcraven](http://twitter.com/bertcraven)) I found a nice way to accomplish this using the **SyndicationFeed** in **System.ServiceModel.Syndication**.

 

I’ve detailed the steps below, but if you want to get to the heart of it then here’s the code to get this working:

 
    
    <span style="color: #2b91af">SyndicationFeed </span>mainFeed = <span style="color: blue">new </span><span style="color: #2b91af">SyndicationFeed</span>();
    <span style="color: #2b91af">List</span><<span style="color: blue">string</span>> feeds = <span style="color: blue">new </span><span style="color: #2b91af">List</span><<span style="color: blue">string</span>>();
    <span style="clear: both">
    feeds.Add(<span style="color: #a31515">"http://feeds2.feedburner.com/WadeWegner"</span>);
    feeds.Add(<span style="color: #a31515">"http://www.nickharris.net/feed/"</span>);
    feeds.Add(<span style="color: #a31515">"http://feeds.feedburner.com/ntotten"</span>);
    feeds.Add(<span style="color: #a31515">"http://michaelwasham.com/feed/"</span>);
    feeds.Add(<span style="color: #a31515">"http://blogs.msdn.com/b/hpctrekker/rss.aspx"</span>);
    <span style="clear: both">
    <span style="color: blue">foreach </span>(<span style="color: blue">var </span>feed <span style="color: blue">in </span>GetRssFeeds())
    {
        <span style="color: #2b91af">Uri </span>feedUri = <span style="color: blue">new </span><span style="color: #2b91af">Uri</span>(feed);
        <span style="color: #2b91af">SyndicationFeed </span>syndicationFeed;
        <span style="color: blue">using </span>(<span style="color: #2b91af">XmlReader </span>reader = <span style="color: #2b91af">XmlReader</span>.Create(feedUri.AbsoluteUri))
        {
            syndicationFeed = <span style="color: #2b91af">SyndicationFeed</span>.Load(reader);
        }
    
        syndicationFeed.Id = feed;
    
        <span style="color: #2b91af">SyndicationFeed </span>tempFeed = <span style="color: blue">new </span><span style="color: #2b91af">SyndicationFeed</span>(
            mainFeed.Items.Union(syndicationFeed.Items).OrderByDescending(u => u.PublishDate));
        mainFeed = tempFeed;
    }





It’s really quite simple – once you know how to do it!





As you iterate through the list of feeds we use LINQ to union the feeds together – in the end this produces a main feed that has all the contents. Along the way we sort the elements in a descending order based on the **PublishDate** – otherwise you’ll just get blocks from each of the feeds and nothing is sorted according to the date publish. Once this is done you end up with a main feed that you can use.





For me I wanted to create a service that published the aggregated feed – I chose to use ASP.NET MVC 3 for this new feed. Here are steps you can follow in order to get this working in ASP.NET MVC 3.






  
  1. Create a new ASP.NET MVC 3 Web Application. I’ve called mine **RssFeed**.   
[![NewProject](http://images.wadewegner.com/wordpress/2011/11/NewProject_thumb.jpg)](http://images.wadewegner.com/wordpress/2011/11/NewProject.jpg)


  
  2. Choose an **Internet Application** using the **Razor** view engine and **HTML5 semantic** markup. 


  
  3. Add **System.ServiceModel** as a reference in the application. We’ll use this with **SyndicationFeed**. 


  
  4. Create an empty controller. I’ve called mine the **RssController**.   
[![RssFeed](http://images.wadewegner.com/wordpress/2011/11/RssFeed_thumb.jpg)](http://images.wadewegner.com/wordpress/2011/11/RssFeed.jpg)


  
  5. We’re going to define our own **ActionResult** implementation that can emit RSS by deriving from **ActionResult**. Inspiration and original source comes from [this post](http://www.developerzen.com/2009/01/11/aspnet-mvc-rss-feed-action-result/) on Developer Zen. 

    
    
    <span style="color: blue">public class </span><span style="color: #2b91af">RssActionResult </span>: <span style="color: #2b91af">ActionResult
    </span>{
        <span style="color: blue">public </span><span style="color: #2b91af">SyndicationFeed </span>Feed { <span style="color: blue">get</span>; <span style="color: blue">set</span>; }
    
        <span style="color: blue">public override void </span>ExecuteResult(<span style="color: #2b91af">ControllerContext </span>context)
        {
            context.HttpContext.Response.ContentType = <span style="color: #a31515">"application/rss+xml"</span>;
    
            <span style="color: #2b91af">Rss20FeedFormatter </span>rssFormatter = <span style="color: blue">new </span><span style="color: #2b91af">Rss20FeedFormatter</span>(Feed);
            <span style="color: blue">using </span>(<span style="color: #2b91af">XmlWriter </span>writer = <span style="color: #2b91af">XmlWriter</span>.Create(context.HttpContext.Response.Output))
            {
                rssFormatter.WriteTo(writer);
            }
        }
    }


  


  
  6. We can now update the **Index** method to use the **RssActionResult** instead of the default **ActionResult** implementation. 

    
    
    <span style="color: blue">public </span><span style="color: #2b91af">RssActionResult </span>Index()
    {
        <span style="color: blue">return new </span><span style="color: #2b91af">RssActionResult</span>();
    }


  


  
  7. Define a method that returns all the feeds with which you want to aggregate. You can pull from many different places – I recommend SQL Azure – but for the purposes of this demo you can just use a generic list of strings. 
    
    
    <span style="color: blue">private static </span><span style="color: #2b91af">List</span><<span style="color: blue">string</span>> GetRssFeeds()
    {
        <span style="color: #2b91af">List</span><<span style="color: blue">string</span>> feeds = <span style="color: blue">new </span><span style="color: #2b91af">List</span><<span style="color: blue">string</span>>();
    
        feeds.Add(<span style="color: #a31515">"http://feeds2.feedburner.com/WadeWegner"</span>);
        feeds.Add(<span style="color: #a31515">"http://www.nickharris.net/feed/"</span>);
        feeds.Add(<span style="color: #a31515">"http://feeds.feedburner.com/ntotten"</span>);
        
        <span style="color: blue">return </span>feeds;
    }


  


  
  8. Now we can update our Index method to iterate through the feeds and aggregate them into a single SyndicationFeed that is sorted (descending) by the publish date. 
    
    
    <span style="color: blue">public </span><span style="color: #2b91af">RssActionResult </span>Index()
    {
        <span style="color: #2b91af">SyndicationFeed </span>mainFeed = <span style="color: blue">new </span><span style="color: #2b91af">SyndicationFeed</span>();
    
        <span style="color: blue">foreach </span>(<span style="color: blue">var </span>feed <span style="color: blue">in </span>GetRssFeeds())
        {
            <span style="color: #2b91af">Uri </span>feedUri = <span style="color: blue">new </span><span style="color: #2b91af">Uri</span>(feed);
            <span style="color: #2b91af">SyndicationFeed </span>syndicationFeed;
            <span style="color: blue">using </span>(<span style="color: #2b91af">XmlReader </span>reader = <span style="color: #2b91af">XmlReader</span>.Create(feedUri.AbsoluteUri))
            {
                syndicationFeed = <span style="color: #2b91af">SyndicationFeed</span>.Load(reader);
            }
    
            syndicationFeed.Id = feed;
    
            <span style="color: #2b91af">SyndicationFeed </span>tempFeed = <span style="color: blue">new </span><span style="color: #2b91af">SyndicationFeed</span>(
                mainFeed.Items.Union(syndicationFeed.Items).OrderByDescending(u => u.PublishDate));
            mainFeed = tempFeed;
        }
    
        <span style="color: blue">return new </span><span style="color: #2b91af">RssActionResult</span>() { Feed = mainFeed };
    }


  


  
  9. Now, hit F5 and run. Browse to http://localhost:<port>/rss to see the aggregated RSS feed.   
[![RssFeed](http://images.wadewegner.com/wordpress/2011/11/RssFeed_thumb.jpg)](http://images.wadewegner.com/wordpress/2011/11/RssFeed.jpg)





And that’s it!





There’s certainly more you can do with this – in fact, given the cost it takes to aggregate a large number of feeds, I’ve started to take the aggregated feed and store it in Windows Azure blob storage attached to the Content Delivery Network (CDN). The code to do this is similar to the following:




    
    <span style="color: #2b91af">StringBuilder </span>builder = <span style="color: blue">new </span><span style="color: #2b91af">StringBuilder</span>();
    <span style="color: blue">using </span>(<span style="color: #2b91af">XmlWriter </span>writer = <span style="color: #2b91af">XmlWriter</span>.Create(builder))
    {
        mainFeed.SaveAsRss20(writer);
        <span style="color: blue">string </span>rssFeed = builder.ToString();
    
    </span>}
    <span style="color: green">// write to Windows Azure blob storage</span>




    
    <font face="Calibri">You might consider doing something similar.</font>




    
    <font face="Calibri">I hope this helps!</font>
