---
author: admin
comments: true
date: 2011-08-19 03:34:15+00:00
layout: post
slug: using-elmah-in-windows-azure-with-table-storage
title: Using ELMAH in Windows Azure with Table Storage
wordpress_id: 1473
categories:
- ASP.NET
- ELMAH
- NuGet
- Windows Azure
---

In this week’s episode of [Cloud Cover](http://channel9.msdn.com/Shows/Cloud+Cover), [Steve](http://blog.smarx.com/) and I covered Logging, Tracing, and ELMAH in Windows Azure. Steve explored the first two topics while I looked into ELMAH in Windows Azure. You should make sure and take a look at his posts – they’re useful:

 

  
  * [Printf(“HERE”) in the Cloud](http://blog.smarx.com/posts/printf-here-in-the-cloud)
   
  * [Lightweight Tracing to Windows Azure Tables](http://blog.smarx.com/posts/lightweight-tracing-to-windows-azure-tables)
 

ELMAH (Error Logging Modules and Handlers) itself is extremely useful, and with a few simple modifications can provide a very effective way to handle application-wide error logging for your ASP.NET web applications. If you aren’t already familiar with ELMAH, take a look at the [ELMAH project page](http://code.google.com/p/elmah/).

 

**_NuGet_**

 

Before going any further, I thought I’d let you know that I’ve created a NuGet package that makes this __extremely__ easy to try. You can take a look at [ELMAH with Windows Azure Table Storage](http://nuget.org/List/Packages/WindowsAzure.ELMAH.Tables) on the NuGet gallery or immediately try this out with the following command:

 

Install-Package WindowsAzure.ELMAH.Tables

 

By default this NuGet package is configured to use the local storage emulator. If you want to use your actual Windows Azure storage account you can uncomment the following line in the Web.Config file:

 
    
    <span style="color: blue"><!--
    </span><span style="color: green"><errorLog 
        type="WebRole1.TableErrorLog, WebRole1" 
        connectionString="DefaultEndpointsProtocol=https;AccountName=YOURSTORAGEACCOUNT;
          AccountKey=YOURSTORAGEKEY" />
    </span><span style="color: blue">-->
    </span>






Incidentally, if you like NuGet, then you should check out Cory Fowler’s post on [must have NuGet packages for Windows Azure development](http://blog.syntaxc4.net/post/2011/08/01/Must-have-NuGet-Packages-for-Windows-Azure.aspx).





**_Demo_**





For those of you unfamiliar with ELMAH, I put together a simple demo. You can try it out on [http://elmahdemo.cloudapp.net/](http://elmahdemo.cloudapp.net/). Just enter a message (keep it clean, please!) and throw an exception.





[![ELMAHDemo](http://images.wadewegner.com/wordpress/2011/08/ELMAHDemo_thumb.jpg)](http://images.wadewegner.com/wordpress/2011/08/ELMAHDemo.jpg)





Click the **ELMAH** button to then load the handler. You’ll see all the errors logged with a lot of great detail.





[![ELMAHHandler](http://images.wadewegner.com/wordpress/2011/08/ELMAHHandler_thumb.jpg)](http://images.wadewegner.com/wordpress/2011/08/ELMAHHandler.jpg)





The great part is that these files are getting serialized into Windows Azure table storage. The benefit of this is you can read them from anywhere – in fact, you don’t have to even deploy the elmah.axd handler with your web application! You could run it locally.





Here’s what the files look like in table storage:





[![ELMAHInTables](http://images.wadewegner.com/wordpress/2011/08/ELMAHInTables_thumb.jpg)](http://images.wadewegner.com/wordpress/2011/08/ELMAHInTables.jpg)





**_How Does it Work?_**





The nice part is you can easily grab the NuGet package to view all the source code. There are two items of interest: ErrorEntity.cs and Web.Config.





In ErrorEntity.cs we first create our ErrorEntity:




    
    <span style="color: blue">public class </span><span style="color: #2b91af">ErrorEntity </span>: <span style="color: #2b91af">TableServiceEntity
    </span>{
        <span style="color: blue">public string </span>SerializedError { <span style="color: blue">get</span>; <span style="color: blue">set</span>; }
    
        <span style="color: blue">public </span>ErrorEntity() { }
        <span style="color: blue">public </span>ErrorEntity(<span style="color: #2b91af">Error </span>error)
            : <span style="color: blue">base</span>(<span style="color: blue">string</span>.Empty, (<span style="color: #2b91af">DateTime</span>.MaxValue.Ticks - <span style="color: #2b91af">DateTime</span>.UtcNow.Ticks).ToString(<span style="color: #a31515">"d19"</span>))
        {
            <span style="color: blue">this</span>.SerializedError = <span style="color: #2b91af">ErrorXml</span>.EncodeString(error);
        }
    }





Then we implement the ErrorLog abstract class from ELMAH to create a TableErrorLog class with all the implementation details.





    
    <span style="color: blue">public class </span><span style="color: #2b91af">TableErrorLog </span>: <span style="color: #2b91af">ErrorLog
    </span>{
        <span style="color: blue">private string </span>connectionString;
    
        <span style="color: blue">public override </span><span style="color: #2b91af">ErrorLogEntry </span>GetError(<span style="color: blue">string </span>id)
        {
            <span style="color: blue">return new </span><span style="color: #2b91af">ErrorLogEntry</span>(<span style="color: blue">this</span>, id, <span style="color: #2b91af">ErrorXml</span>.DecodeString(<span style="color: #2b91af">CloudStorageAccount</span>.Parse(
                connectionString).CreateCloudTableClient().GetDataServiceContext()
                .CreateQuery<<span style="color: #2b91af">ErrorEntity</span>>(<span style="color: #a31515">"elmaherrors"</span>).Where(e => e.PartitionKey == <span style="color: blue">string</span>.Empty 
                    && e.RowKey == id).Single().SerializedError));
        }
    
        <span style="color: blue">public override int </span>GetErrors(<span style="color: blue">int </span>pageIndex, <span style="color: blue">int </span>pageSize, <span style="color: #2b91af">IList </span>errorEntryList)
        {
            <span style="color: blue">var </span>count = 0;
            <span style="color: blue">foreach </span>(<span style="color: blue">var </span>error <span style="color: blue">in </span><span style="color: #2b91af">CloudStorageAccount</span>.Parse(connectionString).
                CreateCloudTableClient().GetDataServiceContext()
                .CreateQuery<<span style="color: #2b91af">ErrorEntity</span>>(<span style="color: #a31515">"elmaherrors"</span>)
                .Where(e => e.PartitionKey == <span style="color: blue">string</span>.Empty).AsTableServiceQuery()
                .Take((pageIndex + 1) * pageSize).ToList().Skip(pageIndex * pageSize))
            {
                errorEntryList.Add(<span style="color: blue">new </span><span style="color: #2b91af">ErrorLogEntry</span>(<span style="color: blue">this</span>, error.RowKey, 
                    <span style="color: #2b91af">ErrorXml</span>.DecodeString(error.SerializedError)));
                count += 1;
            }
            <span style="color: blue">return </span>count;
        }
    
        <span style="color: blue">public override string </span>Log(<span style="color: #2b91af">Error </span>error)
        {
            <span style="color: blue">var </span>entity = <span style="color: blue">new </span><span style="color: #2b91af">ErrorEntity</span>(error);
            <span style="color: blue">var </span>context = <span style="color: #2b91af">CloudStorageAccount</span>.Parse(connectionString)
                .CreateCloudTableClient().GetDataServiceContext();
            context.AddObject(<span style="color: #a31515">"elmaherrors"</span>, entity);
            context.SaveChangesWithRetries();
            <span style="color: blue">return </span>entity.RowKey;
        }
    
        <span style="color: blue">public </span>TableErrorLog(<span style="color: #2b91af">IDictionary </span>config)
        {
            connectionString = (<span style="color: blue">string</span>)config[<span style="color: #a31515">"connectionString"</span>] ?? <span style="color: #2b91af">RoleEnvironment
                </span>.GetConfigurationSettingValue((<span style="color: blue">string</span>)config[<span style="color: #a31515">"connectionStringName"</span>]);
            Initialize();
        }
    
        <span style="color: blue">public </span>TableErrorLog(<span style="color: blue">string </span>connectionString)
        {
            <span style="color: blue">this</span>.connectionString = connectionString;
            Initialize();
        }
    
        <span style="color: blue">void </span>Initialize()
        {
            <span style="color: #2b91af">CloudStorageAccount</span>.Parse(connectionString).CreateCloudTableClient()
                .CreateTableIfNotExist(<span style="color: #a31515">"elmaherrors"</span>);
        }
    }






Now, to leverage these assets, we update the Web.Config file to include an <elmah> ... </elmah> section that specifies our custom error log (and also allows remote access to the handler:




    
    <span style="color: blue"><</span><span style="color: #a31515">elmah</span><span style="color: blue">>
      <</span><span style="color: #a31515">security </span><span style="color: red">allowRemoteAccess</span><span style="color: blue">=</span>"<span style="color: blue">yes</span>" <span style="color: blue">/>
      <</span><span style="color: #a31515">errorLog </span><span style="color: red">type</span><span style="color: blue">=</span>"<span style="color: blue">WebRole1.TableErrorLog, WebRole1</span>" 
                <span style="color: red">connectionString</span><span style="color: blue">=</span>"<span style="color: blue">UseDevelopmentStorage=true</span>" <span style="color: blue">/>
    </</span><span style="color: #a31515">elmah</span><span style="color: blue">>
    </span>






That’s all there’s to it!





Of course, there are many other ways you could define your ErrorEntity and implement the TableErrorLog (i.e. you could extract more details into additional entities within your table), but this way is pretty effective.





I hope this helps!
