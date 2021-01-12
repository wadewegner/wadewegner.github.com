---
categories:
- Salesforce
- Force.com
- rest api
- Toolkit
- Windows 8
- Windows Phone 8
- OAuth
- Token Refresh
date: "2014-02-05T00:00:00Z"
description: 'Three important updates for the Salesforce Toolkits for .NET: refresh
  tokens, async suffix, and sample applications.'
tags: []
title: Support for Refresh Tokens, Async Suffix, and Updated Sample Apps
---

Over the past two days I've committed and published a number of updates to the [Salesforce Toolkits for .NET](http://www.wadewegner.com/2014/01/announcing-the-salesforce-toolkits-for-net/) worth sharing. But first, a special thanks to [Rocky Assad](https://github.com/fourq) and [Delmer Johnson](https://github.com/DelmerJohnson) for the contributions they have made to the toolkits over the past couple weeks. It's fantastic to see such passionate involvement from members of the Salesforce developer community!

There are three updates I'd like to discuss in this post:

1. Support for refresh tokens
2. The addition of an "Async" suffix
3. Updated sample applications

Without futher ado ...

### Support for refresh tokens

I added support for using the refresh token to acquire a fresh access token in the <span class="inline-code">DeveloperForce.Common</span> library and NuGet package. The syntax is really quite simple.

<script src="https://gist.github.com/wadewegner/8828039.js?file=CallTokenRefreshAsync.cs"></script>

This is a simple operation. The two methods enabling this to work are <span class="inline-code">TokenRefreshAsync</span> and <span class="inline-code">FormatRefreshTokenUrl</span>.

<script src="https://gist.github.com/wadewegner/8828039.js?file=FormatRefreshTokenUrl.cs"></script>

The <span class="inline-code">FormatRefreshTokenUrl</span> method helps to create the URL with all the proper values. It is a public static method, so you can use it independently of <span class="inline-code">TokenRefreshAsync</span>. Of course, <span class="inline-code">TokenRefreshAysnc</span> uses it as well.

<script src="https://gist.github.com/wadewegner/8828039.js?file=TokenRefreshAsync.cs"></script>

Notice that <span class="inline-code">TokenRefreshAsync</span> creates the same <span class="inline-code">AuthToken</span> used by the other authentication methods, so it's easy to incorporate.

**Refresh Token Reminder:** The refresh token may have an indefinite lifetime, persisting until explicitly revoked by the end-user. The client application can store the refresh token, using it to periodically obtain fresh access tokens, but should be careful to protect it against unauthorized access, since, like a password, it can be repeatedly used to gain access to the resource server.

### The addition of an "Async" suffix

While I may not agree with the it, the [Task-based Asynchronous Pattern](http://msdn.microsoft.com/en-us/library/hh873175.aspx) (TAP) is very clear about the naming convention for asynchronous methods. While I believe that async should be the default (and synchronous should be the odd ball) the reality is that this is not the convention for public libraries.

Consequently, all of the methods in the toolkits have been updated to include an **Async** suffix.

I don't like introducing changes like this and will strive to keep them to the minimum. Fortunately, this this is simply a naming change and shouldn't be difficult for you to update.

### Updated sample applications

All of our sample applications have been updated. Make sure to take a look: [https://github.com/developerforce/Force.com-Toolkit-for-NET/tree/master/samples](https://github.com/developerforce/Force.com-Toolkit-for-NET/tree/master/samples)

- [Simple Console](https://github.com/developerforce/Force.com-Toolkit-for-NET/tree/master/samples/SimpleConsole)
- [Web Server OAuth](https://github.com/developerforce/Force.com-Toolkit-for-NET/tree/master/samples/WebServerOAuthFlow)
- [Windows 8 OAuth](https://github.com/developerforce/Force.com-Toolkit-for-NET/tree/master/samples/Windows8OAuth)
- [Windows Phone OAuth](https://github.com/developerforce/Force.com-Toolkit-for-NET/tree/master/samples/WindowsPhoneOAuth)

In particular, take a look at the **Windows 8 OAuth Sample** to see how the token refresh is implemented.

I hope you find these updates useful!