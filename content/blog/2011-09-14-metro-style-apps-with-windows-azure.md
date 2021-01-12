---
author: admin
categories:
- Access Control Service
- Azure
- Windows 8
- Windows Azure
comments: true
date: "2011-09-14T17:53:52Z"
slug: metro-style-apps-with-windows-azure
title: Metro Style Apps with Windows Azure
wordpress_id: 1512
---

I love building keynote applications! I had the great fortune to work with [John Shewchuk](http://www.microsoft.com/presspass/exec/techfellow/Shewchuk/default.mspx) – Technical Fellow at Microsoft – as he demonstrated a vision for how identity in Windows Azure can enable great experiences in Windows 8. I wanted to quickly provide some background on the components of the sample application he showed called **Margie’s Travel**.

Margie’s Travel is a sample travel application that demonstrates how you can track and manage your trips across multiple Windows 8 machines using a combination of technologies in Windows Azure and Windows 8.

The application is a Metro styled app built on HTML5, CSS, and JavaScript. Additionally, this application was rapidly built by using the templates and samples found in the [Windows Azure Toolkit for Windows 8](http://watwindows8.codeplex.com/).

When the application is launched, the user needs to login. Rather than creating yet another identity store, or mapping directly to a specific identity provider, Margie’s Travel uses the [Windows Azure Access Control Service](http://www.microsoft.com/windowsazure/features/accesscontrol/).

![Margie's Travel](https://wadewegner.blob.core.windows.net/wordpress/2011/09/Image1_thumb3_thumb.png)

When you click the login button, the application first checks the Windows PasswordVault to see if the credential (which includes the token) exists:

    var vault = new Windows.Security.Credentials.PasswordVault();
    var cred = vault.retrieve(url, username);

If this exists, the application will login. If not, the the application calls out to the Access Control Service to get a list of identity providers from which the user can select.

![Windows Azure Access Control Service](https://wadewegner.blob.core.windows.net/wordpress/2011/09/Image2_thumb1_thumb.png)

This code is also very simple to write in JavaScript:

    var request = new XMLHttpRequest();
    request.open("GET", IPSFeedURL("https://ACSNAMESPACE.accesscontrol.windows.net"), false);
    request.send(null);
    var jsonString = request.responseText;
    var jsonlist = ParseIPList(jsonString);

    BindJsonToList(jsonlist);
    
Once the users makes the selection, the Windows Web Authentication Broker invoked. This allows us to use a consistent and secure method for handling authentication. The login page for the selected identity provider is rendered in the broker.

![Windows Web Auth Broker](https://wadewegner.blob.core.windows.net/wordpress/2011/09/Image3_thumb2_thumb.png)

Once the user logs in, the Access Control Service token is return to the Web Auth Broker. The application is able to take the credential and store it into the Windows Web Vault. This gives us a consistent SSO experience so that upon subsequent launches thee user does not need to log in again.

To store the credential, we simply take the various components, create a new PasswordCredential, and add it to the vault.

    var cred = new Windows.Security.Credentials.PasswordCredential(
        url, username, token);

    vault.add(cred);

Furthermore, since the Web Broker can synchronize across trusted devices using Windows Live, the token is automatically synchronized to any trusted device so that you can get SSO across multiple devices.

![Rich Data in Margie's Travel](https://wadewegner.blob.core.windows.net/wordpress/2011/09/Image4_thumb1_thumb.png)

Once logged in, the application will call out to additional Web services in Windows Azure (like the GetTravelerInfo() method) so that we can validate the users credentials before returning the results.

In addition, this token can be used to call out to additional services in Windows Azure, to get rich pictures from Bing, specific data from the Windows Azure DataMarket and Wolfram Alpha, and even weather information.

![Data from Windows Azure DataMarket and Bing](https://wadewegner.blob.core.windows.net/wordpress/2011/09/Image5_thumb1_thumb.png)

All of this is made possible by unique features and capabilities provided by Windows Azure and Windows 8.

If you want to give this a try, and learn more about how all this works, download the [Windows Azure Toolkit for Windows 8](http://watwindows8.codeplex.com/). Additionally, take a look at posts by [Nick Harris](http://blogs.msdn.com/b/windowsazure/archive/2011/09/14/announcing-the-windows-azure-toolkit-for-windows-8.aspx) and [Vittorio Bertocci](http://blogs.msdn.com/b/vbertocci/archive/2011/09/14/using-acs-in-metro-style-applications.aspx).

I hope this helps!