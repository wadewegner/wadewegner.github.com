---
layout: post
title: "Better Support for HttpClient in the Salesforce Toolkits for .NET"
description: ""
categories:
- Salesforce
- Force.com
- rest api
- Toolkit
tags: []
---
{% include setup %}

If you [follow me on Twitter](http://twitter.com/WadeWegner), chances are you heard we released a set of Salesforce Toolkits for .NET last week. The reception has been extraordinary, and I'm so excited to see the applications developers build using these toolkits!

Take a look at the various annoucements and posts:

- **Wade Wegner**: [Announcing the Salesforce Toolkits for .NET](http://www.wadewegner.com/2014/01/announcing-the-salesforce-toolkits-for-net/)

- **Developer Force Team Blog**: [Announcing the Salesforce Toolkits for .NET](http://blogs.developerforce.com/developer-relations/2014/01/announcing-the-salesforce-toolkits-for-net.html)

- **Richard Seroter**: [Using the New Salesforce Toolkit for .NET](http://seroter.wordpress.com/2014/01/16/using-the-new-salesforce-toolkit-for-net/)

- **Nathen Dress**: [Force.com Toolkit for .NET Initial Review](http://blog.sonomapartners.com/2014/01/forcecom-toolkit-for-net-initial-review.html)

(Incidently, if you know of other posts I've missed, please let me know!)

Just because we released doesn't mean I've stopped working. In fact, I've been monitoring feedback via Twitter and Github, and today made a few worthwhile optimizations. It was rightly pointed out to me by [Darrel Miller](https://twitter.com/darrel_miller) that the <span class="inline-code">ServiceHttpClient</span> did not [optimally use the HttpClient library](https://twitter.com/darrel_miller/status/420685723243536384). Specifically:

- The <span class="inline-code">Developer.Common</span> library created a new <span class="inline-code">HttpClient</span> each time. This can be an extremely expensive operation.

- There was no way to modify the <span class="inline-code">HttpRequestMessage</span> class.

- There was no way to use the <span class="inline-code">HttpClient</span> across <span class="inline-code">ServiceHttpClient</span> and <span class="inline-code">AuthenticationClient</span.

To fix this, a number of changes were made to the <span class="inline-code">Developer.Common</span> library.

1) The <span class="inline-code">ServiceHttpClient</span> now [requires the HttpClient in the constructors](https://github.com/developerforce/Common-Libraries-for-NET/blob/master/src/CommonLibrariesForNET/ServiceHttpClient.cs#L20). Given that, for most operations, the <span class="inline-code">DeveloperForce.Force</span> and <span class="inline-code">DeveloperForce.Chatter</span> libraries manage this, you will likely see no changes to any of your code. However, if you ever want to create your own <span class="inline-code">HttpClient</span> and use it to make multiple calls against the Force.com or Chatter REST APIs, you can now create it yourself and pass it in.

2) Similarly, the <span class="inline-code">AuthenticationClient</span> class allows you to pass in the <span class="inline-code">HttpClient</span>. However, it is not required, as I'm making the assumption some people simply want to make a call to get the token, and don't care if the <span class="inline-code">HttpClient</span> is disposed.

3) Both the <span class="inline-code">ServiceHttpClient</span> and <span class="inline-code">AuthenticationClient</span> now implement <span class="inline-code">IDisposable</span>. This means you can use <span class="inline-code">Using</span> statements to dispose the <span class="inline-code">HttpClient</span> or you can call <span class="inline-code">Dispose</span> yourself.

All these updates have been released via our NuGet packages.

Here's what it looks like in action:

<script src="https://gist.github.com/wadewegner/62cbfebce913a4e66fa0.js"></script>

I hope you find this a useful update!

Many thanks to [Darrel Miller](https://twitter.com/darrel_miller) for all his great feedback!