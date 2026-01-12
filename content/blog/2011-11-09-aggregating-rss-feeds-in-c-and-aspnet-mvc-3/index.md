---
title: "Aggregating RSS Feeds in C# and ASP.NET MVC 3"
date: 2011-11-09T00:00:00-08:00
draft: false
description: "From the whiteboard to the keyboard"
---

I’m working on a Windows Phone project that requires me to surface up multiple RSS feeds as a single source. I needed a way to do this quickly and easily, and with a little help from friends on Twitter (particularly a suggestion from [@bertcraven](http://twitter.com/bertcraven)) I found a nice way to accomplish this using the **SyndicationFeed** in **System.ServiceModel.Syndication**.

I’ve detailed the steps below, but if you want to get to the heart of it then here’s the code to get this working:

```
SyndicationFeed mainFeed = new SyndicationFeed();
List<string> feeds = new List<string>();

feeds.Add("http://feeds2.feedburner.com/WadeWegner");
feeds.Add("http://www.nickharris.net/feed/");
feeds.Add("http://feeds.feedburner.com/ntotten");
feeds.Add("http://michaelwasham.com/feed/");
feeds.Add("http://blogs.msdn.com/b/hpctrekker/rss.aspx");

foreach (var feed in GetRssFeeds())
{
    Uri feedUri = new Uri(feed); SyndicationFeed syndicationFeed;
    using (XmlReader reader = XmlReader.Create(feedUri.AbsoluteUri))
    {
        syndicationFeed = SyndicationFeed.Load(reader);
    }
    syndicationFeed.Id = feed;

    SyndicationFeed tempFeed = new SyndicationFeed(
        mainFeed.Items.Union(syndicationFeed.Items).OrderByDescending(u => u.PublishDate));

    mainFeed = tempFeed;
}
```

It’s really quite simple – once you know how to do it!

As you iterate through the list of feeds we use LINQ to union the feeds together – in the end this produces a main feed that has all the contents. Along the way we sort the elements in a descending order based on the **PublishDate** – otherwise you’ll just get blocks from each of the feeds and nothing is sorted according to the date publish. Once this is done you end up with a main feed that you can use.

For me I wanted to create a service that published the aggregated feed – I chose to use ASP.NET MVC 3 for this new feed. Here are steps you can follow in order to get this working in ASP.NET MVC 3.

1. Create a new ASP.NET MVC 3 Web Application. I’ve called mine **RssFeed**.

   ![NewProject](https://wadewegner.blob.core.windows.net/wordpress/2011/11/NewProject_thumb.jpg)
2. Choose an **Internet Application** using the **Razor** view engine and **HTML5 semantic** markup.
3. Add **System.ServiceModel** as a reference in the application. We’ll use this with **SyndicationFeed**.
4. Create an empty controller. I’ve called mine the **RssController**.

   ![RssFeed](https://wadewegner.blob.core.windows.net/wordpress/2011/11/RssFeed_thumb.jpg)
5. We’re going to define our own **ActionResult** implementation that can emit RSS by deriving from **ActionResult**. Inspiration and original source comes from [this post](http://www.developerzen.com/2009/01/11/aspnet-mvc-rss-feed-action-result/) on Developer Zen.

   ```
    public class RssActionResult : ActionResult
    {
        public SyndicationFeed Feed { get; set; }

        public override void ExecuteResult(ControllerContext context)
        {

            context.HttpContext.Response.ContentType = "application/rss+xml";
            Rss20FeedFormatter rssFormatter = new Rss20FeedFormatter(Feed);

            using (XmlWriter writer = XmlWriter.Create(context.HttpContext.Response.Output))
            {
                rssFormatter.WriteTo(writer);
            }
        }
    }
   ```
6. We can now update the **Index** method to use the **RssActionResult** instead of the default **ActionResult** implementation.

   ```
    public RssActionResult Index() { return new RssActionResult(); }
   ```
7. Define a method that returns all the feeds with which you want to aggregate. You can pull from many different places – I recommend SQL Azure – but for the purposes of this demo you can just use a generic list of strings.

   ```
    private static List<string> GetRssFeeds()
    {
        List<string> feeds = new List<string>();

        feeds.Add("http://feeds2.feedburner.com/WadeWegner");
        feeds.Add("http://www.nickharris.net/feed/");
        feeds.Add("http://feeds.feedburner.com/ntotten");

        return feeds;
    }
   ```
8. Now we can update our Index method to iterate through the feeds and aggregate them into a single SyndicationFeed that is sorted (descending) by the publish date.

   ```
    public RssActionResult Index()
    {
        SyndicationFeed mainFeed = new SyndicationFeed();

        foreach (var feed in GetRssFeeds())
        {
            Uri feedUri = new Uri(feed);
            SyndicationFeed syndicationFeed;
            using (XmlReader reader = XmlReader.Create(feedUri.AbsoluteUri))
            {
                syndicationFeed = SyndicationFeed.Load(reader);
            }

            syndicationFeed.Id = feed;
            SyndicationFeed tempFeed = new SyndicationFeed(
                mainFeed.Items.Union(syndicationFeed.Items).OrderByDescending(u => u.PublishDate));

            mainFeed = tempFeed;
        }

        return new RssActionResult() { Feed = mainFeed };
    }
   ```
9. Now, hit F5 and run. Browse to http://localhost:/rss to see the aggregated RSS feed.

   ![RssFeed](https://wadewegner.blob.core.windows.net/wordpress/2011/11/RssFeed_thumb.jpg)

And that’s it!

There’s certainly more you can do with this – in fact, given the cost it takes to aggregate a large number of feeds, I’ve started to take the aggregated feed and store it in Windows Azure blob storage attached to the Content Delivery Network (CDN). The code to do this is similar to the following:

```
StringBuilder builder = new StringBuilder();

using (XmlWriter writer = XmlWriter.Create(builder))
{
    mainFeed.SaveAsRss20(writer);
    string rssFeed = builder.ToString();
}

// write to Windows Azure blob storage
```

You might consider doing something similar.

I hope this helps!
