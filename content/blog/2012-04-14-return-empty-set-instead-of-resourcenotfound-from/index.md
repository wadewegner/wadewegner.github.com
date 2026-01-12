---
title: "Return Empty Set Instead of ResourceNotFound from Table Storage"
date: 2012-04-14T00:00:00-08:00
draft: false
description: "From the whiteboard to the keyboard"
---

This past week I’ve been working on a little project – amazing how less email equates to more time for other endeavors – and I was surprised when I received a DataServiceQueryException when querying table storage in the local storage emulator. I was querying based on partition and row keys and, if no data matched the statement, I received an HTTP 404: Resource Not Found exception.

I was initially puzzled. Shouldn’t I receive an empty set or null instead?

Of course, I had forgotten that this is by design. The DataServiceContext will throw a DataServiceQueryException if there’s no data to return. To receive an empty set it’s necessary to set the IgnoreResourceNotFoundException property to true.

Here’s a simplified version of the code:

```
string connectionString = "UseDevelopmentStorage=true";

var context = CloudStorageAccount.Parse(connectionString).CreateCloudTableClient)
    .GetDataServiceContext();
context.IgnoreResourceNotFoundException = true;

var results = context.CreateQuery<TableEntity>("tableName")
    .Where(e => e.PartitionKey == partitionKey && e.RowKey == rowKey).AsTableServiceQuery();

var key = results.FirstOrDefault();
```

Problem solved. No DataServiceQueryException!

Something to keep in mind when working with the Windows Azure table storage service. I almost didn’t blog about but decided that it was worth a few minutes effort. Probably something to add to your Windows Azure development checklist (you have one, right?).
