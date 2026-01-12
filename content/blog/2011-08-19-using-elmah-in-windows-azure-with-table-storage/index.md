---
title: "Using ELMAH in Windows Azure with Table Storage"
date: 2011-08-19T00:00:00-08:00
draft: false
description: "From the whiteboard to the keyboard"
---

In this week’s episode of [Cloud Cover](http://channel9.msdn.com/Shows/Cloud+Cover), [Steve](http://blog.smarx.com/) and I covered Logging, Tracing, and ELMAH in Windows Azure. Steve explored the first two topics while I looked into ELMAH in Windows Azure. You should make sure and take a look at his posts – they’re useful:

- [Printf(“HERE”) in the Cloud](http://blog.smarx.com/posts/printf-here-in-the-cloud)
- [Lightweight Tracing to Windows Azure Tables](http://blog.smarx.com/posts/lightweight-tracing-to-windows-azure-tables)

ELMAH (Error Logging Modules and Handlers) itself is extremely useful, and with a few simple modifications can provide a very effective way to handle application-wide error logging for your ASP.NET web applications. If you aren’t already familiar with ELMAH, take a look at the [ELMAH project page](http://code.google.com/p/elmah/).

**NuGet**

Before going any further, I thought I’d let you know that I’ve created a NuGet package that makes this extremely easy to try. You can take a look at [ELMAH with Windows Azure Table Storage](http://nuget.org/List/Packages/WindowsAzure.ELMAH.Tables) on the NuGet gallery or immediately try this out with the following command:

```
Install-Package WindowsAzure.ELMAH.Tables
```

By default this NuGet package is configured to use the local storage emulator. If you want to use your actual Windows Azure storage account you can uncomment the following line in the Web.Config file:

```
<errorLog
    type="WebRole1.TableErrorLog, WebRole1"
    connectionString="DefaultEndpointsProtocol=https;AccountName=YOURSTORAGEACCOUNT;
    AccountKey=YOURSTORAGEKEY" />
```

Incidentally, if you like NuGet, then you should check out Cory Fowler’s post on [must have NuGet packages for Windows Azure development](http://blog.syntaxc4.net/post/2011/08/01/Must-have-NuGet-Packages-for-Windows-Azure.aspx).

**Demo**

For those of you unfamiliar with ELMAH, I put together a simple demo. You can try it out on <http://elmahdemo.cloudapp.net/>. Just enter a message (keep it clean, please!) and throw an exception.

![ELMAHDemo](https://wadewegner.blob.core.windows.net/wordpress/2011/08/ELMAHDemo_thumb.jpg)

Click the **ELMAH** button to then load the handler. You’ll see all the errors logged with a lot of great detail.

![ELMAHHandler](https://wadewegner.blob.core.windows.net/wordpress/2011/08/ELMAHHandler_thumb.jpg)

The great part is that these files are getting serialized into Windows Azure table storage. The benefit of this is you can read them from anywhere – in fact, you don’t have to even deploy the elmah.axd handler with your web application! You could run it locally.

Here’s what the files look like in table storage:

![ELMAHInTables](https://wadewegner.blob.core.windows.net/wordpress/2011/08/ELMAHInTables_thumb.jpg)

**How Does it Work?**

The nice part is you can easily grab the NuGet package to view all the source code. There are two items of interest: ErrorEntity.cs and Web.Config.

In ErrorEntity.cs we first create our ErrorEntity:

```
public class ErrorEntity : TableServiceEntity
{
    public string SerializedError { get; set; }
    public ErrorEntity() {}

    public ErrorEntity(Error error) : base(string.Empty, (DateTime.MaxValue.Ticks - DateTime.UtcNow.Ticks).ToString("d19"))
    {
        this.SerializedError = ErrorXml.EncodeString(error);
    }
}
```

Then we implement the ErrorLog abstract class from ELMAH to create a TableErrorLog class with all the implementation details.

```
public class TableErrorLog : ErrorLog
{
    private string connectionString;

    public override ErrorLogEntry GetError(string id)
    {
        return new ErrorLogEntry(this, id, ErrorXml.DecodeString(CloudStorageAccount.Parse(connectionString)
            .CreateCloudTableClient().GetDataServiceContext().CreateQuery<ErrorEntity>("elmaherrors")
            .Where(e => e.PartitionKey == string.Empty && e.RowKey == id).Single().SerializedError));
    }

    public override int GetErrors(int pageIndex, int pageSize, IList errorEntryList)
    {
        var count = 0;
        foreach (var error in CloudStorageAccount.Parse(connectionString).CreateCloudTableClient().GetDataServiceContext().CreateQuery<ErrorEntity>("elmaherrors").Where(e => e.PartitionKey == string.Empty).AsTableServiceQuery().Take((pageIndex + 1) * pageSize).ToList().Skip(pageIndex * pageSize))
        {
            errorEntryList.Add(new ErrorLogEntry(this, error.RowKey, ErrorXml.DecodeString(error.SerializedError)));
            count += 1;
        }
        return count;
    }

    public override string Log(Error error)
    {
        var entity = new ErrorEntity(error);
        var context = CloudStorageAccount.Parse(connectionString).CreateCloudTableClient().GetDataServiceContext();
        context.AddObject("elmaherrors", entity); context.SaveChangesWithRetries();

        return entity.RowKey;
    }

    public TableErrorLog(IDictionary config)
    {
        connectionString = (string)config["connectionString"] ?? RoleEnvironment.GetConfigurationSettingValue((string)config["connectionStringName"]);

        Initialize();
    }

    public TableErrorLog(string connectionString)
    {
        this.connectionString = connectionString; Initialize();
    }

    void Initialize()
    {
        CloudStorageAccount.Parse(connectionString).CreateCloudTableClient().CreateTableIfNotExist("elmaherrors");
    }
}
```

Now, to leverage these assets, we update the Web.Config file to include an  …  section that specifies our custom error log (and also allows remote access to the handler:

```
<elmah>
    <security allowRemoteAccess="yes" />
    <errorLog type="WebRole1.TableErrorLog, WebRole1" connectionString="UseDevelopmentStorage=true" />
</elmah>
```

That’s all there’s to it!

Of course, there are many other ways you could define your ErrorEntity and implement the TableErrorLog (i.e. you could extract more details into additional entities within your table), but this way is pretty effective.

I hope this helps!
