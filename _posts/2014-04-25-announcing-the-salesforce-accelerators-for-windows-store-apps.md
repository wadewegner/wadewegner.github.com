---
layout: post
title: "Announcing the Salesforce Accelerators for Windows Store Apps"
description: "The accelerators for Windows Store apps make it easier to build new Windows Store apps using the Salesforce Toolkits for .NET"
categories:
- Salesforce
- Force.com
- Accelerators
- Toolkit
- NuGet
- Visual Studio
- Windows 8
---

Yesterday I was pleased to announce the [Salesforce Accelerators for Windows Phone 8 Apps](http://www.wadewegner.com/2014/04/Announcing-the-Windows-Phone-8-Accelerators-for-Salesforce/). Today I'm excited to tell you about the **Salesforce Accelerators for Windows Store Apps**:

* [DeveloperForce.Windows8.Login](https://www.nuget.org/packages/DeveloperForce.Windows8.Login/)
* [DeveloperForce.Windows8.Samples.Accounts](https://www.nuget.org/packages/DeveloperForce.Windows8.Samples.Accounts/)

These accelerators exist to make it easier for you to get started building Windows 8 store apps and work directly with the [Salesforce Toolkits for .NET](http://www.wadewegner.com/2014/01/announcing-the-salesforce-toolkits-for-net/). Take a look at this short video to see just how easy it is to get started.

<iframe width="640" height="360" src="//www.youtube.com/embed/WnSSJxm9KI0" frameborder="0" allowfullscreen></iframe>

Let's look in greater detail at how all of this works.

### Using the DeveloperForce.Windows8.Login NuGet

Using the [DeveloperForce.Windows8.Login]() NuGet is pretty straightforward and only involves a few steps.

1. Setup a **Connected App** in Salesforce ([instructions here](https://help.salesforce.com/HTViewHelpDoc?id=connected_app_create.htm&language=en_US)). Be sure to **Enable OAuth Settings**, at least choose **Full access (full)** and set the **Callback URL** to <span class="inline-code">sfdc://success</span>.

2. Install the NuGet Package via the command line ...

		Install-Package DeveloperForce.Windows8.Login

	... or using **Manage NuGet Packages** in Visual Studio. When you install this NuGet package you also get the **DeveloperForce.Force** and **DeveloperForce.Common** NuGet packages, which are defined as dependencies.

3. Open **MainPage_SFDC.xaml.cs** and change the <span class="inline-code">ConsumerKey</span> value to the one specified by your **Connected App**.

4. Open **MainPage.xaml.cs** wire up the <span class="inline-code">Page_Loaded</span> event. (You can also do this from the designer, as shown in the video.)

	<script src="https://gist.github.com/wadewegner/11298871.js?file=MainPage.cs"></script>

5. Make the <span class="inline-code">MainPage_Loaded</span> asynchronous and call the <span class="inline-code">GetAccessToken</span> method.

	<script src="https://gist.github.com/wadewegner/11298871.js?file=MainPage_Loaded.cs"></script>

6. Run the application.

As you can see, using the NuGet is quite simple and takes away the guess work and trouble of setting up the user login experience. Nevertheless, it's worth noting a few things about how it all works.

* **App_SFDC.xaml.cs** is a partial class that extends the functionality of <span class="inline-code">App</span> by defining three public properties: <span class="inline-code">AccessToken</span>, <span class="inline-code">RefreshToken</span>, and <span class="inline-code">InstanceUrl</span>. These values are returned to us and set after the user logs in. Having these as public properties gives us the ability to share these around the app so that we can use them when making subsequent REST API calls.

	<script src="https://gist.github.com/wadewegner/11298871.js?file=App_SFDC.cs"></script>

* **MainPage_SFDC.xaml.cs** is a partial class that extends the default **MainPage.xaml.cs** through the <span class="inline-code">GetAccessToken</span> method. It contains all the logic for interacting with the <span class="inline-code">WebAuthenticationBroker</span> which manages the authentication process for Windows Store Apps.

* The <span class="inline-code">GetAccessToken</span> method first creates a set of URIs for the login process. It then initiates the <span class="inline-code">WebAuthenticationBroker</span> process by calling <span class="inline-code">AuthenticateAsync</span>. If the response comes back as a success the response URI is decoded and the <span class="inline-code">AccessToken</span>, <span class="inline-code">RefreshToken</span>, and <span class="inline-code">InstanceUrl</span> values are specified in the <span class="inline-code">app</span> variable.

	<script src="https://gist.github.com/wadewegner/11298871.js?file=GetAccessToken.cs"></script>

* Once the login process completes the **MainPage.xaml** is loaded.

That's all it takes. After you login you'll be back on a blank **MainPage.xaml** but you'll have your <span class="inline-code">AccessToken</span> available to use when calling the Salesforce1 Platform REST APIs, which is a great transition to the **DeveloperForce.Windows8.Samples.Accounts** NuGet package accelerator.

### Using the DeveloperForce.Windows8.Samples.Accounts NuGet

An access token is worthless if you don't do anything with it.

Using the [DeveloperForce.Windows8.Samples.Accounts](https://www.nuget.org/packages/DeveloperForce.Windows8.Samples.Accounts/) NuGet makes it easy to create a page where you can query and view your account information. It is designed to demonstrate how to use the access token and instance url retrieved during the login process.

To use this NuGet, just follow these quick steps:

1. Install the NuGet Package via the command line ...

		Install-Package DeveloperForce.Windows8.Samples.Accounts

	... or using **Manage NuGet Packages** in Visual Studio.

2. After calling <span class="inline-code">GetAccessToken</span> navigate the user to the newly added **AccountsPage.xaml**.

	<script src="https://gist.github.com/wadewegner/11298871.js?file=AccountsPage.cs"></script>

That's it! Now run it and see it in action.

As with the previous accelerator, it will help if you know a few things about how this works. Here's the code that makes it function.

<script src="https://gist.github.com/wadewegner/11298871.js?file=Page_Loaded_AccountsPage.cs"></script>

* You can see that we first set our <span class="inline-code">app</span> variables so that we can get values like <span class="inline-code">AccessToken</span> and <span class="inline-code">InstanceUrl</span>.

* Next we create an instance of the <span class="inline-code">ForceClient</span>.

* We perform an asynchronous query using a simple SOQL select statement.

* Lastly, we take the records and bind them to an accounts grid on the page.

It's as simple as that!

It is my hope that these accelerators help get you over the initial hurdles that exist when building new applications that integrate with Salesforce. As always, if you have additional ideas (or find bugs!) you can open a [new issue on Github](https://github.com/developerforce/Force.com-Toolkit-for-NET/issues) or (if you're so inclinded) take a tab at it yourself and [send me a pull request](http://help.github.com/send-pull-requests/).

I hope this helps!