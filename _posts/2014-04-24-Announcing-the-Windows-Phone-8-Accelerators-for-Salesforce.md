---
layout: post
title: "Announcing the Windows Phone 8 Accelerators for Salesforce"
description: "The accelerators for Windows Phone 8 make it easier to build new Windows Phone 8 apps using the Salesforce Toolkits for .NET"
categories:
- Salesforce
- Force.com
- Accelerators
- Toolkit
- NuGet
- Visual Studio
---

I am pleased to announce a set of accelerators designed to make it easier to create new applications using the Salesforce Toolkits for .NET. You can 

The [Salesforce Toolkits for .NET](http://www.wadewegner.com/2014/01/announcing-the-salesforce-toolkits-for-net/), released earlier this year, allows you to interact with the Force.com and Chatter REST APIs using native .NET code on multiple platforms and devices. Many developers and companies have already benefited from these libraries and have built new ASP.NET web applications, Windows Phone 8 apps, and even Windows 8 store apps.

Nevertheless, one of the requests I kept receiving was to create additional samples that make it easier to get started; little *nuggets* of code that *accelerate* the process. These accelerators are the answer to those requests by delivering functionality through small, composable NuGet packages.

This short video shows how quick and easy it is to build a Windows Phone 8 app that connects to Salesforce.

<iframe width="560" height="315" src="//www.youtube.com/embed/i-vYcyZL9Yo" frameborder="0" allowfullscreen></iframe>

Let's dig a little deeper and look at exactly what is happening.

### Using the DeveloperForce.WindowsPhone8.Login NuGet

Using the [DeveloperForce.WindowsPhone8.Login](https://www.nuget.org/packages/DeveloperForce.WindowsPhone8.Login/) NuGet is pretty straightforward and only involves a few steps.

1. Setup a **Connected App** in Salesforce ([instructions here](https://help.salesforce.com/HTViewHelpDoc?id=connected_app_create.htm&language=en_US)). Be sure to **Enable OAuth Settings**, at least choose **Full access (full)** and set the **Callback URL** to: <span class="inline-code">sfdc://success</span>.

2. Install the NuGet Package via the command line ...

		Install-Package DeveloperForce.WindowsPhone8.Login

	... or using **Manage NuGet Packages** in Visual Studio. When you install this NuGet package you also get the **DeveloperForce.Force** and **DeveloperForce.Common** NuGet packages, which are defined as dependencies.

3. Set **LoginPage.xaml** (installed with the NuGet) as the default page by changing **Navigation Page** in **WMAppManifest.xml** to: <span class="inline-code">Pages\LoginPage.xaml</span>

4. Open **Pages\LoginPage.xaml.cs** and change the <span class="inline-code">ConsumerKey</span> value to the one specified by your **Connected App**.

5. Run the application.

As you can see, using the NuGet is quite simple takes away the guess work and trouble of setting up the user login experience. Nevertheless, it's worth noting a few things about how it all works.

* **App_SFDC.xaml.cs** is a partial class that extends the functionality of <span class="inline-code">App</span> by defining three public properties: <span class="inline-code">AccessToken</span>, <span class="inline-code">RefreshToken</span>, and <span class="inline-code">InstanceUrl</span>. These values are returned to us and set after the user logs in. Having these as public properties gives us the ability to share these around the app so that we can use them when making subsequent REST API calls.

	<script src="https://gist.github.com/wadewegner/11270012.js?file=properties.cs"></script>

* The **LoginPage.xaml** has a <span class="inline-code">WebBrowser</span> control. The user will log into Salesforce from within this control.

	<script src="https://gist.github.com/wadewegner/11270012.js?file=WebBrowser"></script>

* There is a set of constants defined at the top of the **LoginPage.xaml.cs**: <span class="inline-code">AuthorizationEndpointUrl</span>, <span class="inline-code">ConsumerKey</span>, <span class="inline-code">CallbackUrl</span>, and <span class="inline-code">ApiVersion</span>. The only value you must change is <span class="inline-code">ConsumerKey</span> as by default it is set to nonsense.

* The <span class="inline-code">LoginPage</span> constructor enables scripts on the <span class="inline-code">WebBrowser</span> control. This is required for the login process to work.

* The <span class="inline-code">OnNavigatedTo</span> event fires when the user is navigated to the login page. It constructs a URL to the Salesforce login page (using a helper from the **DeveloperForce.Common** NuGet) and then navigates the <span class="inline-code">WebBrowser</span> control to the page. This is where the user logs in and authorizes the application.

	<script src="https://gist.github.com/wadewegner/11270012.js?file=OnNavigatedTo.cs"></script>

* After the user logs in the <span class="inline-code">WebBrowser_Navigating</span> method is fired. If the return URI has the "sfdc://success" callback URI then we know that the login has been successful. The returned querystring contains the <span class="inline-code">AccessToken</span>, <span class="inline-code">RefreshToken</span>, and <span class="inline-code">InstanceUrl</span>; these values are grabbed and stored as global variables.

	<script src="https://gist.github.com/wadewegner/11270012.js?file=WebBrowser_Navigating.cs"></script>

* Lastly, the user is redirected back to the **MainPage.xaml** after a successful login. (Which you can see on line 19 in the code snippet above.)

That's all it takes. After you login you'll be back on a blank **MainPage.xaml** but you'll have your <span class="inline-code">AccessToken</span> available to use when calling the Salesforce1 Platform REST APIs, which is a great transition to the **DeveloperForce.WindowsPhone8.Samples.Accounts** NuGet package accelerator.

### Using the DeveloperForce.WindowsPhone8.Samples.Accounts NuGet

An access token is worthless if you don't do anything with it.

Using the [DeveloperForce.WindowsPhone8.Samples.Accounts](https://www.nuget.org/packages/DeveloperForce.WindowsPhone8.Samples.Accounts/) NuGet makes it easy to create a page where you can query and view your account information. It is designed to demonstrate how to use the access token and instance url retrieved during the login process.

To use this NuGet, just follow these quick steps:

1. Install the NuGet Package via the command line ...

		Install-Package DeveloperForce.WindowsPhone8.Samples.Accounts

	... or using **Manage NuGet Packages** in Visual Studio.

2. Instead of navigating the user to the **MainPage.xaml** after logging in, navigate them to **Pages\AccountsPage.xaml**. In the <span class="inline-code">WebBrowser_Navigating</span> event on **LoginPage.xaml.cs** change URI as follows:

	<script src="https://gist.github.com/wadewegner/11270012.js?file=accountspage.cs"></script>

That's it! Now run it and see it in action.

As with the previous accelerator, it will help if you know a few things about how this works. Here's the code that makes it function.

<script src="https://gist.github.com/wadewegner/11270012.js?file=PhoneApplicationPage_Loaded.cs"></script>

* You can see that we first ensure we have the <span class="inline-code">InstanceUrl</span> and <span class="inline-code">AccessToken</span> variables available.

* Next we create an instance of the <span class="inline-code">ForceClient</span>.

* We perform an asynchronous query using a simple SOQL select statement.

* Lastly, we take the records and bind them to an accounts list on the page.

It's as simple as that!

It is my hope that these accelerators help get you over the initial hurdles that exist when building new applications that integrate with Salesforce. As always, if you have additional ideas (or find bugs!) you can open a [new issue on Github](https://github.com/developerforce/Force.com-Toolkit-for-NET/issues) or (if you're so inclinded) take a tab at it yourself and [send me a pull request](http://help.github.com/send-pull-requests/).

I hope this helps!
