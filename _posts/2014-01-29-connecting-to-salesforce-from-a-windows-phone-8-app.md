---
layout: post
title: "Connecting to Salesforce from a Windows Phone 8 App"
description: ""
categories:
- Salesforce
- Force.com
- rest api
- Toolkit
- Windows Phone
- OAuth
tags: []
---
{% include setup %}

One of the top requests I've received since [releasing the Salesforce Toolkits for .NET](http://www.wadewegner.com/2014/01/announcing-the-salesforce-toolkits-for-net/) has been a Windows Phone 8 sample application. I was very happy to oblige and today I'm pleased to share with you the [Windows Phone 8 OAuth](https://github.com/developerforce/Force.com-Toolkit-for-NET/tree/master/samples/WindowsPhoneOAuth) sample application. This sample makes use of the <span class="inline-code">Developerforce.Force</span> and <span class="inline-code">Developerforce.Common</span> NuGet packages.

Since the .NET libraries in the toolkits are all portable class libraries developers are able to build applications targeting .NET 4 & 4.5, Windows Phone 8, Windows 8 & 8.1, and Silverlight 5.

![Visual Studio Phone App](https://f.cloud.github.com/assets/746259/2034797/fa1a98d6-8930-11e3-987b-c11f2c77feb2.png)

This sample application demonstrates the following:

1. Obtaining an access token using the User-Agent flow.
2. Manging the callback in a Windows Phone 8 app.
3. Using the access token to make a call to the Force.com REST API.

Let's break all this down and walk through the pertinent pieces.

### Creating Your Connected App

A Connected App is an application that allows you to connect to Salesforce identity and data APIs. Connected Apps use the OAuth 2.0 protocol to authenticate and acquire access tokens for use with the Force.com REST API. Additionally, the Connected App gives you granular control over the access you want to give users.

We're going to use the Connected App to give our mobile application access to the Salesforce APIs.

You can create a Connected App in your developement environment by going to **Setup** > **Create** > **Apps**. The last pane reads **Connected Apps**. Click **New** to create a new Connected App.

Supply the following information:

- **Connected App Name**: WindowsPhoneOAuth Sample
- **Contact Email**: your@emailaddress.com

Click the **Enable OAuth Settings** checkbox and perform the following actions:

- **Callback URL**: sfdc://success
- **Selected OAuth Scopes**: add them all

When complete, it should look something like this:

<img src="https://f.cloud.github.com/assets/746259/2034037/1bb7ae04-8925-11e3-8271-d2c99f4d80d9.png" alt="Connected App settings" style="border-style: solid;border-width:1px;border-color:#767676;">

Go ahead and save your Connected App. Salesforce will create a **Consumer Key** and **Consumer Secret**. All we need for our purposes is the **Consumer Key**.

<img src="https://f.cloud.github.com/assets/746259/2034118/5565bf82-8926-11e3-9f55-a7d3b555fc37.png" alt="Consumer Key" style="border-style: solid;border-width:1px;border-color:#767676;">

**Note**: Identity is a complex topic and it's important you know what you're doing. Just as you wouldn't store a username-password on a mobile device you shouldn't store the Consumer Secret on a mobile device. It is not as hard as you'd think to extract this information. Consequently, we will use the User-Agent flow to protect our Connected App secrets yet still let our users get a valid access token. For more details I recommend you review [Obtaining an Access Token in a Native Application (User-Agent Flow)](http://wiki.developerforce.com/page/Digging_Deeper_into_OAuth_2.0_on_Force.com#Obtaining_an_Access_Token_in_a_Native_Application_.28User-Agent_Flow.29).

### Updating the WindowsPhoneOAuth Sample

There's only change you'll have to make in the sample application. In <span class="inline-code">MainPage.xaml.cs</span> update the <span class="inline-code">ConsumerKey</span> string to use the **Consumer Key** created by your Connected App. Make this change and hit F5 and give it a spin.

<div class="row-fluid">
<div class="span4">
<strong>Login to Salesforce</strong>

<img src="https://f.cloud.github.com/assets/746259/2034280/9ce3b5e2-8928-11e3-9a0c-753c65ef917b.png" alt="login">

</div>
<div class="span4">
<strong>Authorize Access</strong>

<img src="https://f.cloud.github.com/assets/746259/2034281/9e48d9b2-8928-11e3-9bd7-7bba48bbee60.png" alt="authorize">

</div>
<div class="span4">
<strong>See Your Accounts</strong>

<img src="https://f.cloud.github.com/assets/746259/2034282/9fe3f284-8928-11e3-8849-94ee1fee1843.png" alt="accounts">

</div>
</div>

That's it!

Of course, there's quite a bit going on behind the scenes that makes this possible. Let's take a look.

### How Does this Work?

The first step is to create the URL to the Authorization Server. Fortunlately this is fairly simple as the 
<span class="inline-code">Developerforce.Common</span> library provides a helper method called <span class="inline-code">FormatAuthUrl</span> you can use.


<script src="https://gist.github.com/wadewegner/8697510.js?file=CreateAndBrowseAuthURL.cs"></script>

Note that we use the <span class="inline-code">ResponseTypes.Token</span> response type, indicating our desire to use the User-Agent auth flow. The URL will look something like this:

```
https://login.salesforce.com/services/oauth2/authorize?response_type=token&client_id=3MVG9A2kN3Bn17hsEyMqRTTaEfW.t4ssmYD2zPrrftW7vokEg0kCWj3H_NwryefANj37hbxV_KyB0Qd2NLySH&redirect_uri=sfdc://success&display=touch&immediate=False&state=&scope=
```

The important values included her are:

- **response_type**: By specifying **token** we indicate the User-Agent auth flow.
- **client_id**: This is the **Consumer Key** from the Connected App.
- **redirect_uri**: This is the **Callback Url** from the Connected App.
- **display**: We've specified **touch** so that we get a mobile friendly login page.

Once the URL has been constructed we then tell a browser control to navigate to the URL.

Now, once we have logged in, the authorization server will redirect back with information we want to collect. We need a way to handle that callback.

Fortunately (or unfortunately, in some cases) Windows Phone 8 has what's called **URI Association Schemes** that auto launches apps. Since we're returning a URN with the value "sfdc://success" the URI association scheme tries to get involved and launch an application. We want to surpress this behavior while also hooking into it to get the values sent as part of the redirection.

To do this we'll do a few things:

1. Register a protocol extension that is associated to "sfdc" (from our redirect URI).

	<script src="https://gist.github.com/wadewegner/8697510.js?file=WMAppManifest.xml"></script>

2. Create a <span class="inline-code">AuthorizationUriMapper</span> that will allow us to override the default <span class="inline-code">MapUri</span> behavior and get access to the information in the URN.

	<script src="https://gist.github.com/wadewegner/8697510.js?file=AssociationUriMapper.cs"></script>

3. Register our <span class="inline-code">AuthorizationUriMapper</span>

	<script src="https://gist.github.com/wadewegner/8697510.js?file=InitializePhoneApplication.cs"></script>

As a result of our <span class="inline-code">AuthorizationUriMapper</span>, we'll reload the <span class="inline-code">MainPage.xaml</span> and pass in the <span class="inline-code">AccessToken</span> and <span class="inline-code">InstanceUrl</span> needed to make a call the the resource server. Now making a call to the Force.com REST API to get a list of accounts is simple:

<script src="https://gist.github.com/wadewegner/8697510.js?file=LoadAccounts.cs"></script>

And there we have it! Access to the Force.com REST API (actually, any Salesforce API) using an access token acquired without putting any of our secrets in jeopardy.

A special thank you to several people who helped me as I built this sample: [Pat Patterson](http://twitter.com/metadaddy) for helping debug the auth flow, [Matteo Pagani](http://twitter.com/qmatteoq) for helping with the ListBox binding, and [Ginny Caughey](http://twitter.com/gcaughey) & [Darrel Miller](http://twitter.com/darrel_miller) for their help understanding the crazy URI assocation schemes.

I hope you find this sample useful. Good luck!