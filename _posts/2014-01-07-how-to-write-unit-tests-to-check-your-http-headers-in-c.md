---
layout: post
title: "How to Write Unit Tests to Check Your HTTP Headers (and Other HTTP Request Stuff) in C#"
description: ""
categories:
- .NET
- C#
- Unit Tests
tags: []
---
{% include setup %}

I've been building a set of libraries recently that make HTTP calls into the [Force.com](http://www.salesforce.com/us/developer/docs/api_rest/index_Left.htm#StartTopic=Content/quickstart.htm) and [Chatter](http://www.salesforce.com/us/developer/docs/chatterapi/) REST APIs and I needed to find a way to test my methods and ensure that my HTTP Requests were constructed correctly. These libraries do a number of things to help make valid HTTP calls; any inadvertant changes (or introduced bugs) could cause a lot of problems. Fortunately, with help from [Ryan Dunn](http://twitter.com/dunnry), I was able to build some unit tests that are able to inspect my **HttpRequestMessage** an ensure that everything has been properly setup.

There are a few key pieces to solving this challenge:

1. Accepting a callback in our class's constructor for a special **HttpClient** used by our unit tests.
2. A custom **DelegatingHandler** that returns a mocked response for our special **HttpClient**.
3. Using a lambda expression within the unit test to make our assertions.

Let me show you how it works. Or, if you simply want to go directly to the code, you can get it here on Github: [https://github.com/wadewegner/Sample-UnitTestHttpClient](https://github.com/wadewegner/Sample-UnitTestHttpClient)

### The Initial Sample ###

Let's take a relatively complex and real world example of a sample HTTP client.

<script src="https://gist.github.com/wadewegner/56c2c900a92056757e72.js?file=OrigSampleHttpClient.cs"></script>

We can easily call this client and method to get a response.

<script src="https://gist.github.com/wadewegner/56c2c900a92056757e72.js?file=CallingOurSampleHttpClient.cs"></script>

Pretty straightforward so far, right? You probably already seen some of the problems.

### The Problems ###

This will work fine when we have some kind of server that is listening locally at port 1899. However, there are cases when this won't work so well. For example:

1. Will this work when there's not an active server listening at port 1899?

2. How do I create a test tyhat ensure that my User Agent is set properly?

3. How do I check and ensure I'm attaching my authorization header properly?

4. How do I check that the URL is getting formatted properly?

5. How do I make sure that a developer (probably myself) doesn't accidentally mess this up in the future?

And so on. We need to find a way to test these things in our unit tests.

### The Solution ###

Let's see how we can make a few changes to make this a lot easier to test. 

First, we'll create a new local propery that we'll use for our **HttpClient**. Not that it's a callback. More on that in a moment.

<script src="https://gist.github.com/wadewegner/56c2c900a92056757e72.js?file=HttpClientProperty.cs"></script>

We only plan to inject our own **HttpClient** into this class when we're running the unit tests. Let's update the existing constructor to simply create the **HttpClient** but then create a second constructor that we can use to inject our **HttpClient** when running our unit tests.

<script src="https://gist.github.com/wadewegner/56c2c900a92056757e72.js?file=NewConstructors.cs"></script>

This gives us an overload we can use during our unit test.

We now have to update our **HttpGet** method to use our **_builder** property instead of newing up it's own **HttpClient**. Simply update the using statement.

<script src="https://gist.github.com/wadewegner/56c2c900a92056757e72.js?file=UpdatingOurClientUsingStatement.cs"></script>

Now it will use whichever client is defined when the class is constructed.

This is a great start and makes our class easier to test. Now let's look at what we have to do to write the unit test and create the delegating handler to give us a good response.

Let's first create our **DelegatingHandler**. 

<script src="https://gist.github.com/wadewegner/56c2c900a92056757e72.js?file=SampleRouteHandler.cs"></script>

As you can see, it's pretty basic. The important thing to understand is that we're going to use this **DelegatingHandler** when we construct the **HttpClient** that we inject through our unit test. You'll see that I'm returning **JsonContent** as part of the response. You'll need the following class for this to work.

<script src="https://gist.github.com/wadewegner/56c2c900a92056757e72.js?file=JsonContent.cs"></script>

With all this in place, we're ready to write our unit test.

<script src="https://gist.github.com/wadewegner/56c2c900a92056757e72.js?file=UnitTests.cs"></script>

You'll see that we create our **HttpClient** using the **SampleRouteHandler** that we previously defined. We're also using a lambda expression to run all our unit test assertions. Once the client is created we assign it and pass it into the **SampleHttpClient** overload that takes the **HttpClient** injection.

Run your unit test. Put breakpoints inside the **SampleRouteHandler** and notice that you'll break into these assertions after the **SampleHttpClient** calls **client.SendAsync(request)**. This is the perfect way to test and ensure that you've built your **HttpRequestMessage** properly.

You can find the full source code for this sample on Github here: [https://github.com/wadewegner/Sample-UnitTestHttpClient](https://github.com/wadewegner/Sample-UnitTestHttpClient)

I hope this helps! Many thanks again to [Ryan Dunn](http://twitter.com/dunnry) for his help!