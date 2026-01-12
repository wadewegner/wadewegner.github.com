---
title: "Better Support for HttpClient in the Salesforce Toolkits for .NET"
date: 2014-01-21T00:00:00-08:00
draft: false
description: "From the whiteboard to the keyboard"
---

If you [follow me on Twitter](http://twitter.com/WadeWegner), chances are you heard we released a set of Salesforce Toolkits for .NET last week. The reception has been extraordinary, and I’m so excited to see the applications developers build using these toolkits!

Take a look at the various annoucements and posts:

- **Wade Wegner**: [Announcing the Salesforce Toolkits for .NET](http://www.wadewegner.com/2014/01/announcing-the-salesforce-toolkits-for-net/)
- **Developer Force Team Blog**: [Announcing the Salesforce Toolkits for .NET](http://blogs.developerforce.com/developer-relations/2014/01/announcing-the-salesforce-toolkits-for-net.html)
- **Richard Seroter**: [Using the New Salesforce Toolkit for .NET](http://seroter.wordpress.com/2014/01/16/using-the-new-salesforce-toolkit-for-net/)
- **Nathen Dress**: [Force.com Toolkit for .NET Initial Review](http://blog.sonomapartners.com/2014/01/forcecom-toolkit-for-net-initial-review.html)

(Incidently, if you know of other posts I’ve missed, please let me know!)

Just because we released doesn’t mean I’ve stopped working. In fact, I’ve been monitoring feedback via Twitter and Github, and today made a few worthwhile optimizations. It was rightly pointed out to me by [Darrel Miller](https://twitter.com/darrel_miller) that the ServiceHttpClient did not [optimally use the HttpClient library](https://twitter.com/darrel_miller/status/420685723243536384). Specifically:

- The Developer.Common library created a new HttpClient each time. This can be an extremely expensive operation.
- There was no way to modify the HttpRequestMessage class.
- There was no way to use the HttpClient across ServiceHttpClient and AuthenticationClient.

To fix this, a number of changes were made to the DeveloperForce.Common library.

1. The ServiceHttpClient now [requires the HttpClient in the constructors](https://github.com/developerforce/Common-Libraries-for-NET/blob/master/src/CommonLibrariesForNET/ServiceHttpClient.cs#L20). Given that, for most operations, the DeveloperForce.Force and DeveloperForce.Chatter libraries manage this, you will likely see no changes to any of your code. However, if you ever want to create your own HttpClient and use it to make multiple calls against the Force.com or Chatter REST APIs, you can now create it yourself and pass it in.
2. Similarly, the AuthenticationClient class allows you to pass in the HttpClient. However, it is not required, as I’m making the assumption some people simply want to make a call to get the token, and don’t care if the HttpClient is disposed.
3. Both the ServiceHttpClient and AuthenticationClient now implement IDisposable. This means you can use Using statements to dispose the HttpClient or you can call Dispose yourself.

All these updates have been released via our NuGet packages.

Here’s what it looks like in action:

I hope you find this a useful update!

Many thanks to [Darrel Miller](https://twitter.com/darrel_miller) for all his great feedback!
