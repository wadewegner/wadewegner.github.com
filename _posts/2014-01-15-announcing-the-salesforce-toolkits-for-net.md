---
layout: post
title: "Announcing the Salesforce Toolkits for .NET"
description: "I am extremely pleased to announce the availability of two Salesforce Toolkits for .NET developers: the Force.com Toolkit for .NET and the Chatter Toolkit for .NET. These toolkits provide an easy way for .NET developers to interact with the Force.com and Chatter REST APIs using a native .NET libraries."
categories:
- Salesforce
- Force.com
- Chatter
- rest api
- Toolkit
- NuGet
tags: []
---

I am extremely pleased to announce the availability of two Salesforce Toolkits for .NET developers: the [Force.com Toolkit for .NET](https://github.com/developerforce/Force.com-Toolkit-for-NET) and the [Chatter Toolkit for .NET](https://github.com/developerforce/Chatter-Toolkit-for-NET). These toolkits provide an easy way for .NET developers to interact with the Force.com and Chatter REST APIs using a native .NET libraries.

![salesforcetoolkits](https://f.cloud.github.com/assets/746259/1926314/ddc56dec-7e3f-11e3-9428-cb3958a2e0c4.png)

The easiest way to get started is to use the NuGet packages:

&nbsp;&nbsp;<span class="inline-code">Install-Package DeveloperForce.Force</span><br />
&nbsp;&nbsp;<span class="inline-code">Install-Package DeveloperForce.Chatter</span><br />

You can find a set of API examples and samples in the Github repositories.

### Design Principles

There were a few key design principles followed when designing and building these toolkits:

1. Provide support for as many modern Microsoft platforms as possible with the same core library. To accomplish this, the toolkits makes use of [Portable Class Librares](http://msdn.microsoft.com/en-us/library/gg597391.aspx) to enable use on the following platforms: Windows 7 (.NET 4, .NET 4.5), Windows 8, Windows Phone 8, and Silverlight 5.

2. Avoid performance bottlenecks and application blocking. To accomplish this, the toolkits make use of the [Async and Await pattern](http://msdn.microsoft.com/en-us/library/hh191443.aspx) for asynchronous programming.

3. Provide the support for key operations and enable the community participate in active development of the toolkits. You will see that our [policy for contributions](https://github.com/developerforce/Force.com-Toolkit-for-NET/blob/master/README.md#contributing-to-the-repository) is extremely permissive - we want you to participate!

4. Use NuGet packages as the primary delivery mechanism while allowing access to the underlying source code. Furthermore, give developer's access to the [latest development/test packages](https://github.com/developerforce/Force.com-Toolkit-for-NET/blob/master/README.md#devtest-packages).

5. Use a Build/CI environment to ensure compilation with checkins, run functional tests against a Development Edition org, run unit tests against the libraries, and generate our NuGet packages. We accomplished this using TeamCity - in fact, you can see an icon indicating the actual health of the builds on each of the repositories. 

These were the key items we felt were important for the toolkit.

### Why Did We Build Them?

The .NET community is extremely important to Salesforce. Not only do a large number of Salesforce developers identify themselves as .NET developers but many of our customers have made investments in the Microsoft development stack.

After joining Salesforce last year I was a bit surprised we did not provide native libraries for .NET developers. These toolkits are my first contribution towards making amends.

### How Do You Get Started?

The easiest way to get started today is to use the NuGet packages. Take a look at the repositories and review some of the sample code that shows you how to generate access tokens, make Force.come REST API calls, and use the Chatter API. You can take a look at the available [sample applications](https://github.com/developerforce/Force.com-Toolkit-for-NET/tree/master/samples) and I also recommend reviewing the [Force.com functional tests](https://github.com/developerforce/Force.com-Toolkit-for-NET/blob/master/src/ForceToolkitForNet.FunctionalTests/ForceClientTests.cs) and [Chatter functional tests](https://github.com/developerforce/Chatter-Toolkit-for-NET/blob/master/src/ChatterToolkitForNET.FunctionalTests/ChatterClientTests.cs) for insight into the libraries.

### What's Next?

There is certainly more to do. Here's a short list of additional ideas:

1. Continue to provide more support for API operations. We're only scratching the surface today.

2. Lots more samples. If you're at all interested in getting involved, this is a great way. I'd love to work with you.

3. Support for other APIs, such as the Streaming API, SOAP API.

4. Webinars and hackathons for .NET developers.

5. Lots more blogging and discussion about the toolkits and how to use them.

6. Did I mention sample applications?

At the end of the day, the purpose of the Salesforce Toolkits for .NET is to make it easier for .NET developers to build amazing applications using .NET and Salesforce. This release is just the first step; there's certainly more to do, and a lot of opportunity for you to get involved.

I hope you are excited about this release and that this helps! I look forward to working with you in the commmunity to make this an invaluable resource.
