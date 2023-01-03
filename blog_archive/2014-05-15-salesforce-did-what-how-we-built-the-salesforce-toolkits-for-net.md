---
categories:
- salesforce
- force.com
- toolkit
- rest api
- nuget
- travis ci
- mono.net
- xamarin
- teched
date: "2014-05-15T00:00:00Z"
description: Learn how the Salesforce Toolkits for .NET were built in this TechEd
  session by Wade Wegner. You'll see Wade build, from scratch, a new SDK using the
  same techniques and design principals that built the released toolkit.
title: Salesforce Did What? How We Built the Salesforce Toolkits for .NET
---

I had the great pleasure this week to speak at [TechEd North America 2014](http://northamerica.msteched.com/) on how we built the [Salesforce Toolkits for .NET](http://www.wadewegner.com/2014/01/announcing-the-salesforce-toolkits-for-net/). It was a blast, and I wouldn't be surprised if I was the first Salesforce employee to speak at TechEd. Many thanks to my friends at Microsoft for the opportunity.

Here is the [video recording of the talk](http://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/DEV-B308) on Channel 9:

<iframe src="http://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/DEV-B308/player?h=405&w=720&format=html5" style="height:405px;width:720px;" allowFullScreen frameBorder="0" scrolling="no"></iframe>

The highlight of the talk for me was that, rather than explain through slides, I built a brand new SDK during the talk and committed it live to [Github in this repository](https://github.com/wadewegner/TechEd14SDK). Take a look at the repository and use whatever your find valuable!

While I gave a quick overview of the toolkits and SDKs we built for Salesforce, the focus of the talk was on *how* they were built. Consequently, I broke the talk into different segments and focused on the following items:

**Segment 1**: Create a new project with authentication capabilities.

* Portable class libraries
* Extensibility
* Async/Await

**Segment 2**: Create tests to validate our new library.

* Functional tests
* Unit tests

**Segment 3**: Allow our library to run with Mono.NET on non-Windows platforms.

* Mono.NET & Xamarin
* Continuous integration with Travis CI

**Segment 4**: Extend our library with additional functionality for developers.

* Anonymous types
* Dynamics

**Segment 5**: Package and deploy our library.

* NuGet

**Segment 6**: How to grow community involvement with our project.

* Community
* Licenses
* Issues, features, and bugs in Github
* Pull requests

I hope you find this presentation valuable! I had a blast and thoroughly enjoyed it.