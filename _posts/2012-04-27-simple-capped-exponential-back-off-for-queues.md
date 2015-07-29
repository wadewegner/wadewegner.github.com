---
author: admin
comments: true
date: 2012-04-27 21:15:36+00:00
layout: post
slug: simple-capped-exponential-back-off-for-queues
title: Simple Capped Exponential Back-Off for Queues
wordpress_id: 1805
categories:
- Azure
- Best Practices
- Queues
- Windows Azure
---

Recently [Steve Marx](http://blog.smarx.com/) and I spent a few hours working on a best practices document for Windows Azure. As expected, this was a fun and educational experience – plenty of goofing around, but also some really good discussion on things to think about when building applications for Windows Azure. One of the items we discussed is a better approach for sleeping inside the Worker Role when pulling from queues. Rather than defaulting to a retry every 10 seconds we decided that the best approach is to exponentially back-off on your queue reads while capping it with an upper bound.

The primary value of this is to decrease the number of storage transactions when reading from your queue, and therefore reduce both bandwidth and transaction costs.

There are plenty of other good posts on this topic that provide a lot more detailed justification and rationale for this approach:

* [Advanced scenarios with Windows Azure Queues](http://www.developerfusion.com/article/120619/advanced-scenarios-with-windows-azure-queues/)  

* [Cloud Lesson Learned: Exponential Backoff](http://geekswithblogs.net/hroggero/archive/2011/05/26/cloud-lesson-learned-exponential-backoff.aspx)  

* [Windows Azure : Messaging with the queue - Patterns for message processing](http://programming4.us/desktop/2910.aspx)

The logic and approach is deceptively simple and I thought I’d share a really simple, yet effective, example. (Incidentally, credit goes to Steve for very quickly putting together the basis of this really simple example.)

Here’s the code:
	
	string queueName = "queuetest";
	int minInterval = 1; 
	int interval = minInterval;
	int exponent = 2; 
	int maxInterval = 60;
	
	CloudStorageAccount account = CloudStorageAccount.DevelopmentStorageAccount; 
	CloudQueueClient queueClient = account.CreateCloudQueueClient(); 
	CloudQueue queue = queueClient.GetQueueReference(queueName); 
	queue.CreateIfNotExist();
	        
	while (true)
	{
	    var msg = queue.GetMessage(); if (msg != null)
	    { 
	        // do something queue.DeleteMessage(msg); interval = minInterval;
	                
	        Trace.WriteLine(string.Format("Interval reset to {0} seconds", interval));
	    }
	    else 
	    { 
	        Trace.WriteLine(string.Format("Sleep for {0} seconds", interval)); 
	        Thread.Sleep(TimeSpan.FromSeconds(interval)); 
	        interval = Math.Min(maxInterval, interval * exponent);
	    }
	}

As I said, really simple. The magic is in the last line where we check to see which is smaller – the maximum interval or the product of the interval and the exponent. At some point the product of the interval and exponent grows larger than the maximum interval, and consequently the interval value is set to the maximum interval.

Here’s the output in the Windows Azure Compute Emulator:
    
	Sleep for 1 seconds 
	Sleep for 2 seconds 
	Sleep for 4 seconds 
	Sleep for 8 seconds 
	Sleep for 16 seconds 
	Sleep for 32 seconds 
	Sleep for 60 seconds 
	Sleep for 60 seconds 

	...

Now, the application will continue to sleep until it finds a message in the queue, at which point the interval is reset back to one. To test this I used the Azure Storage Explorer and created a new queue message.

[![AzureStorageExplorerQueue](https://wadewegner.blob.core.windows.net/wordpress/2012/04/AzureStorageExplorerQueue_thumb.jpg)](https://wadewegner.blob.core.windows.net/wordpress/2012/04/AzureStorageExplorerQueue.jpg)

Once the message is created the output is as follows:
    
	Interval reset to 1 seconds 
	Sleep for 1 seconds
	Sleep for 2 seconds
	Sleep for 4 seconds
	Sleep for 8 seconds

	...

And so forth.

You can find all the source code for this sample in my [CappedExponentialBackOff repository](https://github.com/wadewegner/CappedExponentialBackOff) on GitHub.

Pretty simple but quite useful. I hope this helps!