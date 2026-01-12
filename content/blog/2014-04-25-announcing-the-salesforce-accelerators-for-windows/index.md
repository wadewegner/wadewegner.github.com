---
title: "Announcing the Salesforce Accelerators for Windows Store Apps"
date: 2014-04-25T00:00:00-08:00
draft: false
description: "From the whiteboard to the keyboard"
---

Yesterday I was pleased to announce the [Salesforce Accelerators for Windows Phone 8 Apps](http://www.wadewegner.com/2014/04/Announcing-the-Windows-Phone-8-Accelerators-for-Salesforce/). Today I’m excited to tell you about the **Salesforce Accelerators for Windows Store Apps**:

- [DeveloperForce.Windows8.Login](https://www.nuget.org/packages/DeveloperForce.Windows8.Login/)
- [DeveloperForce.Windows8.Samples.Accounts](https://www.nuget.org/packages/DeveloperForce.Windows8.Samples.Accounts/)

These accelerators exist to make it easier for you to get started building Windows 8 store apps and work directly with the [Salesforce Toolkits for .NET](http://www.wadewegner.com/2014/01/announcing-the-salesforce-toolkits-for-net/). Take a look at this short video to see just how easy it is to get started.

Let’s look in greater detail at how all of this works.

### [Using the DeveloperForce.Windows8.Login NuGet](#using-the-developerforcewindows8login-nuget)

Using the DeveloperForce.Windows8.Login NuGet is pretty straightforward and only involves a few steps.

1. Setup a **Connected App** in Salesforce ([instructions here](https://help.salesforce.com/HTViewHelpDoc?id=connected_app_create.htm&language=en_US)). Be sure to **Enable OAuth Settings**, at least choose **Full access (full)** and set the **Callback URL** to sfdc://success.
2. Install the NuGet Package via the command line …

   ```
    Install-Package DeveloperForce.Windows8.Login
   ```

   … or using **Manage NuGet Packages** in Visual Studio. When you install this NuGet package you also get the **DeveloperForce.Force** and **DeveloperForce.Common** NuGet packages, which are defined as dependencies.
3. Open **MainPage\_SFDC.xaml.cs** and change the ConsumerKey value to the one specified by your **Connected App**.
4. Open **MainPage.xaml.cs** wire up the Page\_Loaded event. (You can also do this from the designer, as shown in the video.)
5. Make the MainPage\_Loaded asynchronous and call the GetAccessToken method.
6. Run the application.

As you can see, using the NuGet is quite simple and takes away the guess work and trouble of setting up the user login experience. Nevertheless, it’s worth noting a few things about how it all works.

- **App\_SFDC.xaml.cs** is a partial class that extends the functionality of App by defining three public properties: AccessToken, RefreshToken, and InstanceUrl. These values are returned to us and set after the user logs in. Having these as public properties gives us the ability to share these around the app so that we can use them when making subsequent REST API calls.
- **MainPage\_SFDC.xaml.cs** is a partial class that extends the default **MainPage.xaml.cs** through the GetAccessToken method. It contains all the logic for interacting with the WebAuthenticationBroker which manages the authentication process for Windows Store Apps.
- The GetAccessToken method first creates a set of URIs for the login process. It then initiates the WebAuthenticationBroker process by calling AuthenticateAsync. If the response comes back as a success the response URI is decoded and the AccessToken, RefreshToken, and InstanceUrl values are specified in the app variable.
- Once the login process completes the **MainPage.xaml** is loaded.

That’s all it takes. After you login you’ll be back on a blank **MainPage.xaml** but you’ll have your AccessToken available to use when calling the Salesforce1 Platform REST APIs, which is a great transition to the **DeveloperForce.Windows8.Samples.Accounts** NuGet package accelerator.

### [Using the DeveloperForce.Windows8.Samples.Accounts NuGet](#using-the-developerforcewindows8samplesaccounts-nuget)

An access token is worthless if you don’t do anything with it.

Using the [DeveloperForce.Windows8.Samples.Accounts](https://www.nuget.org/packages/DeveloperForce.Windows8.Samples.Accounts/) NuGet makes it easy to create a page where you can query and view your account information. It is designed to demonstrate how to use the access token and instance url retrieved during the login process.

To use this NuGet, just follow these quick steps:

1. Install the NuGet Package via the command line …

   ```
    Install-Package DeveloperForce.Windows8.Samples.Accounts
   ```

   … or using **Manage NuGet Packages** in Visual Studio.
2. After calling GetAccessToken navigate the user to the newly added **AccountsPage.xaml**.

That’s it! Now run it and see it in action.

As with the previous accelerator, it will help if you know a few things about how this works. Here’s the code that makes it function.

- You can see that we first set our app variables so that we can get values like AccessToken and InstanceUrl.
- Next we create an instance of the ForceClient.
- We perform an asynchronous query using a simple SOQL select statement.
- Lastly, we take the records and bind them to an accounts grid on the page.

It’s as simple as that!

It is my hope that these accelerators help get you over the initial hurdles that exist when building new applications that integrate with Salesforce. As always, if you have additional ideas (or find bugs!) you can open a [new issue on Github](https://github.com/developerforce/Force.com-Toolkit-for-NET/issues) or (if you’re so inclinded) take a tab at it yourself and [send me a pull request](http://help.github.com/send-pull-requests/).

I hope this helps!
