---
title: "Announcing the Salesforce Accelerators for Windows Phone 8 Apps"
date: 2014-04-24T00:00:00-08:00
draft: false
description: "From the whiteboard to the keyboard"
---

I am pleased to announce the **Salesforce Accelerators for Windows Phone 8 Apps**, a set of assets designed to make it easier to create new applications using the Salesforce Toolkits for .NET:

- [DeveloperForce.WindowsPhone8.Login](https://www.nuget.org/packages/DeveloperForce.Windows8.Login/)
- [DeveloperForce.WindowsPhone8.Samples.Accounts](https://www.nuget.org/packages/DeveloperForce.WindowsPhone8.Samples.Accounts/)

The [Salesforce Toolkits for .NET](http://www.wadewegner.com/2014/01/announcing-the-salesforce-toolkits-for-net/), released earlier this year, allow you to interact with the Force.com and Chatter REST APIs using native .NET code on multiple platforms and devices. Many developers and companies have already benefited from these libraries and have built new ASP.NET web applications, Windows Phone 8 apps, and even Windows 8 store apps.

Nevertheless, one of the requests I kept receiving was to create additional tools and samples that make it easier to get started; little *nuggets* of code that *accelerate* the process. These accelerators are the answer to those requests and deliver functionality through small, composable NuGet packages.

This short video shows how quick and easy it is to build a Windows Phone 8 app that connects to Salesforce.

Let’s dig a little deeper and look at exactly what is happening.

### [Using the DeveloperForce.WindowsPhone8.Login NuGet](#using-the-developerforcewindowsphone8login-nuget)

Using the [DeveloperForce.WindowsPhone8.Login](https://www.nuget.org/packages/DeveloperForce.WindowsPhone8.Login/) NuGet is pretty straightforward and only involves a few steps.

1. Setup a **Connected App** in Salesforce ([instructions here](https://help.salesforce.com/HTViewHelpDoc?id=connected_app_create.htm&language=en_US)). Be sure to **Enable OAuth Settings**, at least choose **Full access (full)** and set the **Callback URL** to sfdc://success.
2. Install the NuGet Package via the command line …

   ```
    Install-Package DeveloperForce.WindowsPhone8.Login
   ```

   … or using **Manage NuGet Packages** in Visual Studio. When you install this NuGet package you also get the **DeveloperForce.Force** and **DeveloperForce.Common** NuGet packages, which are defined as dependencies.
3. Set **LoginPage.xaml** (installed with the NuGet) as the default page by changing **Navigation Page** in **WMAppManifest.xml** to Pages\LoginPage.xaml
4. Open **Pages\LoginPage.xaml.cs** and change the ConsumerKey value to the one specified by your **Connected App**.
5. Run the application.

As you can see, using the NuGet is quite simple and takes away the guess work and trouble of setting up the user login experience. Nevertheless, it’s worth noting a few things about how it all works.

- **App\_SFDC.xaml.cs** is a partial class that extends the functionality of App by defining three public properties: AccessToken, RefreshToken, and InstanceUrl. These values are returned to us and set after the user logs in. Having these as public properties gives us the ability to share these around the app so that we can use them when making subsequent REST API calls.
- The **LoginPage.xaml** has a WebBrowser control. The user will log into Salesforce from within this control.
- There is a set of constants defined at the top of the **LoginPage.xaml.cs**: AuthorizationEndpointUrl, ConsumerKey, CallbackUrl, and ApiVersion. The only value you must change is ConsumerKey as by default it is set to nonsense.
- The LoginPage constructor enables scripts on the WebBrowser control. This is required for the login process to work.
- The OnNavigatedTo event fires when the user is navigated to the login page. It constructs a URL to the Salesforce login page (using a helper from the **DeveloperForce.Common** NuGet) and then navigates the WebBrowser control to the page. This is where the user logs in and authorizes the application.
- After the user logs in the WebBrowser\_Navigating method is fired. If the return URI has the “sfdc://success” callback URI then we know that the login has been successful. The returned querystring contains the AccessToken, RefreshToken, and InstanceUrl; these values are grabbed and stored as global variables.
- Lastly, the user is redirected back to the **MainPage.xaml** after a successful login. (Which you can see on line 18 in the code snippet above.)

That’s all it takes. After you login you’ll be back on a blank **MainPage.xaml** but you’ll have your AccessToken available to use when calling the Salesforce1 Platform REST APIs, which is a great transition to the **DeveloperForce.WindowsPhone8.Samples.Accounts** NuGet package accelerator.

### [Using the DeveloperForce.WindowsPhone8.Samples.Accounts NuGet](#using-the-developerforcewindowsphone8samplesaccounts-nuget)

An access token is worthless if you don’t do anything with it.

Using the [DeveloperForce.WindowsPhone8.Samples.Accounts](https://www.nuget.org/packages/DeveloperForce.WindowsPhone8.Samples.Accounts/) NuGet makes it easy to create a page where you can query and view your account information. It is designed to demonstrate how to use the access token and instance url retrieved during the login process.

To use this NuGet, just follow these quick steps:

1. Install the NuGet Package via the command line …

   ```
    Install-Package DeveloperForce.WindowsPhone8.Samples.Accounts
   ```

   … or using **Manage NuGet Packages** in Visual Studio.
2. Instead of navigating the user to the **MainPage.xaml** after logging in, navigate them to **Pages\AccountsPage.xaml**. In the WebBrowser\_Navigating event on **LoginPage.xaml.cs** change URI as follows:

That’s it! Now run it and see it in action.

As with the previous accelerator, it will help if you know a few things about how this works. Here’s the code that makes it function.

- You can see that we first ensure we have the InstanceUrl and AccessToken variables available.
- Next we create an instance of the ForceClient.
- We perform an asynchronous query using a simple SOQL select statement.
- Lastly, we take the records and bind them to an accounts list on the page.

It’s as simple as that!

It is my hope that these accelerators help get you over the initial hurdles that exist when building new applications that integrate with Salesforce. As always, if you have additional ideas (or find bugs!) you can open a [new issue on Github](https://github.com/developerforce/Force.com-Toolkit-for-NET/issues) or (if you’re so inclinded) take a tab at it yourself and [send me a pull request](http://help.github.com/send-pull-requests/).

I hope this helps!
