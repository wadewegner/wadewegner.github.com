---
layout: post
title: "Using the WebAuthenticationBroker for Salesforce Authentication on Windows 8"
description: ""
categories:
- Salesforce
- Force.com
- rest api
- Toolkit
- Windows 8
- OAuth
- WebAuthenticationBroker
tags: []
---

Two days ago I published a [sample app for Windows Phone 8](http://www.wadewegner.com/2014/01/connecting-to-salesforce-from-a-windows-phone-8-app/) that demonstrates how to authenticate users and make calls against the Force.com REST API using the [Salesforce Toolkits for .NET](http://www.wadewegner.com/2014/01/announcing-the-salesforce-toolkits-for-net/). The next obvious step was to build a sample application for Windows 8. **ACHIEVEMENT UNLOCKED!**

![Windows 8 Sample App](https://f.cloud.github.com/assets/746259/2053351/87effd60-8aa6-11e3-8e22-a33dd8b33bc2.png)

You can get the sample here: [https://github.com/developerforce/Force.com-Toolkit-for-NET/tree/master/samples/Windows8OAuth](https://github.com/developerforce/Force.com-Toolkit-for-NET/tree/master/samples/Windows8OAuth).

To get authentication working in Windows 8 we'll use the <span class="inline-code">WebAuthenticationBroker</span>, which manages the asynchronous authentication operation in a modal dialog window. First, the user is asked to login. This is nothing more than *secured* browser control that renders the Salesforce login page.

<img src="https://f.cloud.github.com/assets/746259/2053148/b33f0488-8aa2-11e3-877a-cb4cc9fce1e1.png" alt="Login" width="40%" style="border-style: solid;border-width:1px;border-color:#767676;">

Once the login information has been added, the user is then asked to authorize the application.

<img src="https://f.cloud.github.com/assets/746259/2053149/b43823ba-8aa2-11e3-8ec6-f40c02081902.png" alt="Authorize" width="40%" style="border-style: solid;border-width:1px;border-color:#767676;">

When this process completes, the user is returned back to the application. Programmatically we now have access to all the information returned to us from Salesforce. (Review [this article](https://help.salesforce.com/HTViewHelpDoc?id=remoteaccess_oauth_user_agent_flow.htm&language=en_US) if you need a refresher.)

Here's the code. When setting up your **Connected App** you can use the same directions I detailed in [this post](http://www.wadewegner.com/2014/01/connecting-to-salesforce-from-a-windows-phone-8-app/). Also, be sure to grab the <span class="inline-code">DeveloperForce.Common</span> NuGet package.

<script src="https://gist.github.com/wadewegner/8737853.js"></script>

A few important details to point out.

- **[Line 8]:** As of the writing of this post, you have to use the <span class="inline-code">DisplayTypes.Popup</span> display type instead of <span class="inline-code">DisplayTypes.Touch</span>. While the rendering isn't ideal for the modal dialog it's better than simply not working (which is what will happen if you use <span class="inline-code">DisplayTypes.Touch</span>).

- **[Line 11]:** Be sure to copy your **Callback Url** exactly as the <span class="inline-code">WebAuthenticationBroker</span> will look for this URI and only break when it finds it.

- **[Line 28]:** In addition to the **access_token**, **refresh_token**, and **instance_url** you can also get the **id**, **signature**, **issued_at**, and (if you passed it with your URL) **state**.

This is just the start for the Windows 8 sample. We'll add other features (such as refreshing the token and storing tokens securely) in the near future.

I hope this helps!