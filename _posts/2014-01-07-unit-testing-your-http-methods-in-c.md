---
layout: post
title: "Unit Testing Your Http Methods in C#"
description: ""
categories:
- .NET
- C#
- Unit Tests
tags: []
---
{% include setup %}

I've been building a set of libraries recently that make HTTP calls into an API using the Microsoft.Net.Http NuGet package and library. I needed to find a way to test my methods and ensure that my HTTP Requests were constructed correctly. Fortunately, with the help of my friend and guru Ryan Dunn, I was able to build some fantastic unit tests using NUnit to make this happen.

The key to solving this challenge are using a callback method that takes our HttpClient ...

	Func<HttpClient> _builder;

... and a **DelegatingHandler** to return a mocked response. Let me show you how it works.

Let's take a relatively complex and real world example of a sample HTTP client.

	public class SampleHttpClient
	{
	    private const string UserAgent = "MyAgent";

	    private readonly string _instanceUrl;
	    private readonly string _apiVersion;
	    private readonly string _accessToken;

	    public SampleHttpClient(string instanceUrl, string apiVersion, string accessToken)
	    {
	        _instanceUrl = instanceUrl;
	        _apiVersion = apiVersion;
	        _accessToken = accessToken;
	    }

	    public async Task<T> HttpGet<T>(string query)
	    {
	        var url = FormatUrl(_instanceUrl, _apiVersion, query);

	        using (var client = new HttpClient())
	        {
	            client.DefaultRequestHeaders.UserAgent.ParseAdd(UserAgent);

	            var request = new HttpRequestMessage()
	            {
	                RequestUri = new Uri(url),
	                Method = HttpMethod.Get
	            };

	            request.Headers.Add("Authorization", "Bearer " + _accessToken);

	            var responseMessage = await client.SendAsync(request);
	            var response = await responseMessage.Content.ReadAsStringAsync();

	            if (responseMessage.IsSuccessStatusCode)
	            {
	                var jObject = JObject.Parse(response);

	                var r = JsonConvert.DeserializeObject<T>(jObject.ToString());
	                return r;
	            }

	            throw new Exception("Something happened!");
	        }
	    }

	    public static string FormatUrl(string instanceUrl, string apiVersion, string query)
	    {
	        return string.Format("{0}/{1}/{2}", instanceUrl, apiVersion, query);
	    }
	}

We can easily call this client and method to get a response.

	var originalSampleHttpClient = new OriginalSampleHttpClient("http://localhost:1899", "v1", "accessToken");
	var response = await originalSampleHttpClient.HttpGet<object>("querystring");

This will work fine when we have some kind of server that is listening locally at port 1899. However, there are a few problems:

1. Will this work when there's not an active server listening at port 1899?

2. How do I create a test tyhat ensure that my User Agent is set properly?

3. How do I check and ensure I'm attaching my authorization header properly?

4. How do I check that the URL is getting formatted properly?

And so on.

Let's see how we can make a few changes to make this a lot easier to test. 

First, we'll create a new callback for our **HttpClient**.

	private readonly Func<HttpClient> _builder;

Now, we only plan to inject our own HttpClient when we're running the unit tests. So, let's update the existing constructor to simply create the HttpClient but then create a second constructor that we can use to inject our HttpClient when running our Unit Tests.

	public SampleHttpClient(string instanceUrl, string apiVersion, string accessToken)
	{
	    _instanceUrl = instanceUrl;
	    _apiVersion = apiVersion;
	    _accessToken = accessToken;

	    _builder = () => new HttpClient();
	}

	public SampleHttpClient(string instanceUrl, string apiVersion, string accessToken, Func<HttpClient> builder )
	{
	    _instanceUrl = instanceUrl;
	    _apiVersion = apiVersion;
	    _accessToken = accessToken;

	    _builder = builder;
	}

This gives us an overload we can use during our unit test.

We now have to update our HttpGet method to use our **_builder** property instead of newing up it's own HttpClient. Simply update the using statement.

	...

	using (var client = _builder())
	{
		...
	}

	...

Now it will use whichever client is defined when the class is constructed.

This is a great start and makes our classes easier to test. Now let's look at what we have to do to write the unit test and create the delegating handler to give us a good response.

Let's first create our **DelegatingHandler**. 

	internal class SampleRouteHandler : DelegatingHandler
	{
	    Action<HttpRequestMessage> _testingAction;

	    public SampleRouteHandler(Action<HttpRequestMessage> testingAction)
	    {
	        _testingAction = testingAction;
	    }

	    protected override Task<HttpResponseMessage> SendAsync(HttpRequestMessage request, CancellationToken ct)
	    {
	        _testingAction(request);
	        var resp = new HttpResponseMessage(HttpStatusCode.OK)
	        {
	            Content = new JsonContent(new
	            {
	                Success = true,
	                Message = "Success"
	            })
	        };

	        var tsc = new TaskCompletionSource<HttpResponseMessage>();
	        tsc.SetResult(resp);
	        return tsc.Task;
	    }
	}

As you can see, it's pretty basic. The important thing to understand is that we're going to use this Delegating Handler when we construct the HttpClient that we inject through our unit test. You'll see that I'm returning **JsonContent** as part of the response. You'll need the following class for this to work.

	internal class JsonContent : HttpContent
	{
	    private readonly MemoryStream _stream = new MemoryStream();
	    public JsonContent(object value)
	    {

	        Headers.ContentType = new MediaTypeHeaderValue("application/json");
	        var jw = new JsonTextWriter(new StreamWriter(_stream)) { Formatting = Formatting.Indented };
	        var serializer = new JsonSerializer();
	        serializer.Serialize(jw, value);
	        jw.Flush();
	        _stream.Position = 0;

	    }
	    protected override Task SerializeToStreamAsync(Stream stream, TransportContext context)
	    {
	        return _stream.CopyToAsync(stream);
	    }

	    protected override bool TryComputeLength(out long length)
	    {
	        length = _stream.Length;
	        return true;
	    }
	}

With all this in place, we're ready to write our unit test.

	[Test]
	public async void Auth_CheckHttpRequestMessage_HttpGet()
	{
	    var client = new HttpClient(new SampleRouteHandler(r =>
	    {
	        Assert.AreEqual(r.RequestUri.ToString(), "http://localhost:1899/v1/querystring");

	        Assert.IsNotNull(r.Headers.UserAgent);
	        Assert.AreEqual(r.Headers.UserAgent.ToString(), "MyAgent");

	        Assert.IsNotNull(r.Headers.Authorization);
	        Assert.AreEqual(r.Headers.Authorization.ToString(), "Bearer accessToken");
	    }));

	    Func<HttpClient> builder = () => client;

	    var sampleHttpClient = new SampleHttpClient("http://localhost:1899", "v1", "accessToken", builder);

	    await sampleHttpClient.HttpGet<object>("querystring");
	}

You'll see that we create our HttpClient using the **SampleRouteHandler** that we previously defined. We're also using a lambda expression to run all our unit test assertions. Once the client is created we assign it and pass it into the **SampleHttpClient** overload that takes the HttpClient injection.

Run your unit test. Put breakpoints inside the **SampleRouteHandler** and notice that you'll break into these assertions after the **SampleHttpClient** calls **client.SendAsync(request)**. This is the perfect way to test and ensure that you've built your **HttpRequestMessage** properly.

You can find the full source code for this sample on Github here: https://github.com/wadewegner/Sample-UnitTestHttpClient

I hope this helps!