---
layout: post
title: "Update Records with Python & the Salesforce Bulk API"
description: "See how to update lots of records with the Salesforce Bulk API using Python."
categories: 
- python
- salesforce
- rest api
- soap api
- bulk api
- force.com
---

It's time to take a little break from .NET and enjoy the many different programming languages you can use when integrating to the Salesforce1 Platform. But first, a public service announcement from [Steve Marx](http://twitter.com/smarx):

<script src="https://gist.github.com/wadewegner/38c6752f76e6970c935d.js?file=steve.py"></script>

This week a colleague of mine asked me if I could help him by writing a script to update data he has in Salesforce (okay, he asked me weeks ago, and I only got to it yesterday). In this particular case, he is tracking articles that are written by different members of our team and includes fields such as date published, published url, as well as the # of tweets, likes, and LinkedIn shares. Each week he's able to use Salesforce to produce dashboards and reports that demonstrate how well (or poorly) articles are doing. The complication was that all the social stats (i.e. # of tweets, likes, and shares) were aggregated manually.

A perfect opportunity for scripting, no?

It's really not that difficult. All three of these particular services provide a simple API that returns a JSON package with the counts we need. For example, let's say I want to see counts/shares for my post announcing the [Salesforce Toolkits for .NET](http://localhost:4000/2014/01/announcing-the-salesforce-toolkits-for-net/):

* For Twitter I'd create the URL [https://cdn.api.twitter.com/1/urls/count.json?url=http://www.wadewegner.com/2014/01/announcing-the-salesforce-toolkits-for-net/](https://cdn.api.twitter.com/1/urls/count.json?url=http://www.wadewegner.com/2014/01/announcing-the-salesforce-toolkits-for-net/) and parse the following JSON:

		{
			"count" : 144,
			"url" : "http://www.wadewegner.com/2014/01/announcing-the-salesforce-toolkits-for-net/"
		}

* For Facebook I'd create the URL [http://graph.facebook.com/http://www.wadewegner.com/2014/01/announcing-the-salesforce-toolkits-for-net/](http://graph.facebook.com/http://www.wadewegner.com/2014/01/announcing-the-salesforce-toolkits-for-net/) and parse the following JSON:

		{
			"id": "http://www.wadewegner.com/2014/01/announcing-the-salesforce-toolkits-for-net/",
			"shares": 28
		}

* For LinkedIn I'd create the URL: [http://www.linkedin.com/countserv/count/share?url=http://www.wadewegner.com/2014/01/announcing-the-salesforce-toolkits-for-net/](http://www.linkedin.com/countserv/count/share?url=http://www.wadewegner.com/2014/01/announcing-the-salesforce-toolkits-for-net/) and parse the following JSON:

		IN.Tags.Share.handleCount({
			"count":18,
			"fCnt":"18",
			"fCntPlusOne":"19",
			"url":"http://www.wadewegner.com/2014/01/announcing-the-salesforce-toolkits-for-net/"
		});

Pretty simple, right?

So, now the challenge is using this information to update all the records in Salesforce. Let me quickly describe to you the two approaches I took. The first was fast and simple, but long term has some complications. The second took longer to create but ultimately provides the soundest approach.

#### Approach 1: Using the Salesforce REST API

I started with this approach. Basically, logged in using the Username-Password authentication flow, grabbed my access token, performed a SOQL query to get all the records with published URLs, looped through them, and then issues an update via the REST API with the proper social counts. Pretty simple.

The problem with this approach is that, eventually, I'm going to hit my [REST API limits](http://help.salesforce.com/apex/HTViewHelpDoc?id=integrate_api_rate_limiting.htm). Not only is my query an API call, but so too is every update. The plan is for this script to run many times throughout the day. It won't take too long for me to hit the limit.

Although it is a simple and elegant solution, ultimately it won't work.

#### Approach 2: Using the Salesforce Bulk API

As you'll see, this approach is a lot more involved. However, it's also rock solid and designed to beyond the scenarios supported by the REST API.

The Bulk API is optimized for loading or deleting large sets of data. You can use it to query, insert, update, upsert, or delete a large number of records asynchronously by submitting batches which are processed in the background by Salesforce.

The easiest way to use Bulk API is to enable it for processing records in Data Loader using CSV files. This avoids the need to write your own client application. However, you can also send it XML, which avoids having to construct or generate CSV files; perfect, in my opinion, for our scenario.

Okay, enough talk. Let's jump into the code.

#### Solution

There are, roughly, four steps involved in using the Bulk API:

1. Logging in.

2. Creating a job.

3. Adding a batch. (Or multiple batches.)

4. Closing the job (thereby starting it).

To facilitate these steps I created a file called `async.py` that had the four following methods.

<script src="https://gist.github.com/wadewegner/38c6752f76e6970c935d.js?file=login.py"></script>

You can see that this method does the following:

* It constructs an XML string that includes the user name and password.
* Defines some heads; in particular it includes the `SOAPAction` of `login`.
* A `requests` post is made to the URL, passing the headers and the encoded data.
* It returns the response to use.

Pretty simple and nothing surprising.

<script src="https://gist.github.com/wadewegner/38c6752f76e6970c935d.js?file=createJob.py"></script>

A few things worth noting here:

* We pass in the `instance` which we derive from the login response. This is used to call against the correct Salesforce datacenter.
* To successfully communicate with the API we need to validate ourselves with the `sessionId`, which was returned in the login response. This value is used in the header `X-SFDC-Session` to validate us against the API.
* To create the job we have to specify an `operation`, i.e. `insert`, `update`, `upsert`, and so on.
* We pass in the `object` we're working with. For example, we're using a custom object called `Byline__c`, so that's what we'll pass in.
* There are two `contentType` values: CSV and XML. We need to specify which one we're using.

<script src="https://gist.github.com/wadewegner/38c6752f76e6970c935d.js?file=addBatch.py"></script>

This is likely the most important, and undocumented, step - particularly when using XML.

* You will need to construct an XML string that includes all the objects you want to manipulate.
* For the purposes of this method I found it easier to create an `objects` parameter on `addBatch` and pass it in. We'll walk through this in detail shortly.
* Notice too that the url now includes the `jobId` that was returned from the `createJob` response.

<script src="https://gist.github.com/wadewegner/38c6752f76e6970c935d.js?file=closeJob.py"></script>

Lastly we close the job. This is a pretty simple operation.

Before we pull it all together, let's look at one more method I've created to facilitate the Bulk API operation. The reason I didn't include this method above is you'll see it's very specific to the particular use case; i.e. we're looping through a lot of data and constructing the `object` data we'll eventually pass into the `addBatch` method.

<script src="https://gist.github.com/wadewegner/38c6752f76e6970c935d.js?file=createObjectXml.py"></script>

Okay, there's a bit going on here. Once you look, though, you'll see that it's not too complex.

* The first thing to point out is that I'm using a library called `simple_salesforce` that provides a set of methods for interacting with the Salesforce REST API. Yes, I'm combining SOAP and REST APIs!
* I perform a `SOQL` query to get the records that have a published link. This is returned as a Python dictionary from the `query_all` method.
* Since it's a dictionary I can easily get all the records and iterate through them all.
* Using the URLs I identified above, I use `requests` to call each of the URLs to get the JSON responses. It's then simple to pull out the `count` or `shares`.
* Lastly, we add the data we pulled from the JSON responses and add it to the `objectXml` string that is ultimately returned. It's worth noting that the `Id` value is required; everything else added to this object is updated in Salesforce.

Pretty simple!

Okay, let's pull this all together.

<script src="https://gist.github.com/wadewegner/38c6752f76e6970c935d.js?file=script.py"></script>

As you can see, we're simply calling our methods and evaluating the responses to get data that we pass into other methods.

* Get the `sessionId` from the `login`. You could also grab the `instance` here with a simple regex.
* Create a job and grab the `jobId`.
* Create the `objectXml` data and pass it into `addBatch` to create the batch.
* Close the batch, which starts the execution.

Now, let's look at the job execution. Login into Salesforce and click **Setup** -> **Jobs**, and **Bulk Data Load Jobs**.

![Jobs](https://cloud.githubusercontent.com/assets/746259/2848136/99fcd1ba-d0b8-11e3-8594-ef30b283ae10.png)

You can click the individual job to see the full details.

And that's a wrap! I hope this helps explain the process so that, when you attempt to do something like this yourself, it won't take you as long as it took me.