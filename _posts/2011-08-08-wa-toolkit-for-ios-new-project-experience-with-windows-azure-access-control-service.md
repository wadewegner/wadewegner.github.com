---
author: admin
comments: true
date: 2011-08-08 23:01:29+00:00
layout: post
slug: wa-toolkit-for-ios-new-project-experience-with-windows-azure-access-control-service
title: 'WA Toolkit for iOS: New Project Experience with Windows Azure Access Control
  Service'
wordpress_id: 1454
categories:
- iOS
- Toolkit
- Windows Azure
---

The v1.2 release of the Windows Azure Toolkit for iOS includes support for Windows Azure Access Control Service (ACS). The enables iOS developers to create applications that can use and rely on federated identity providers such as Windows Live, Google Accounts, and Facebook Connect.

 

This walkthrough document covers configuring the ACS service and creating a new iOS project using XCode to authenticate against this service. This is accomplished using the following three steps in this document:

 

  
  1. Manually Configure ACS (Access Control Service).
   
  2. Create New Project and Import Library
   
  3. Expanding the solution with the Cloud Ready Package
 

In order to run through this tutorial, you’ll need an active Windows Azure subscription. Further information about creating a new account for Windows Azure can be found on [_http://www.windowsazure.com_](http://www.windowsazure.com)

 

**Step 1 – Manually Configure ACS (Access Control Service)**

 

This section will describe how to configure ACS manually for a new iOS application. If you have already deployed the Cloud Ready package and configured the service using the Cloud Ready Configuration Wizard, you can skip to Step 2.

 

First, access the Windows Azure portal by navigating to [_http://windows.azure.com_](http://windows.azure.com) and signing in with your credentials.

 

![iOSACS1](https://wadewegner.blob.core.windows.net/wordpress/2011/08/iOSACS1.jpg)

 

Click on the **Service Bus, Access Control & **Caching menu item and select the **Access Control** menu item under the **AppFabric** folder. Select an active Windows Azure subscription and click on the **New** button in the toolbar.

 

![iOSACS2](https://wadewegner.blob.core.windows.net/wordpress/2011/08/iOSACS2.jpg)

 

The new service namespace dialog will open. Ensure that **Access Control** is selected, and enter a unique namespace and country/region for the service.

 

![iOSACS3](https://wadewegner.blob.core.windows.net/wordpress/2011/08/iOSACS3.jpg)

 

The ACS namespace will now be created. This might take a few minutes. Wait until the namespace is showing in an **Active** state.

 

![iOSACS4](https://wadewegner.blob.core.windows.net/wordpress/2011/08/iOSACS4.jpg)

 

Once this is complete, highlight the newly created service and click on the **Access Control Service** button in the toolbar. This will launch the Access Control Service portal.

 

Within the portal, click on** Identity Providers **and add the identity providers you would like to use for your application.

 

![iOSACS5](https://wadewegner.blob.core.windows.net/wordpress/2011/08/iOSACS5.jpg)

 

The default is Windows Live ID, but you can add other preconfigured providers (such as Google and Yahoo!) as well as external identity systems configured to use WS-Federation.

 

Once you have added the required providers, click on the **Relying Parties** section of the portal.

 

![iOSACS6](https://wadewegner.blob.core.windows.net/wordpress/2011/08/iOSACS6.jpg)

 

Click on the **Add** button and enter the following information for the relying party application:

 

**Name** – a given name for your application          
**Realm** – a unique ID for your application. For this walkthrough, we’ll be using uri:wazmobiletoolkit           
**Return URL** – you can leave this blank          
**Error URL** – you can leave this blank          
**Token Format** – select SWT          
**Token Lifetime** – Feel free to change the default from 600 seconds.

 

Select the identity providers that you would like to use, and then under the **Token Signing Settings** section, click on the **Generate** button to create a new symmetric key that will be used for this application.

 

Finally, click on the **Save** button. This will create the Relying Party Application.

 

Next, go into the **Rule Groups** section of the portal and select the default rule group that was created for the application.

 

[![iOSACS7](https://wadewegner.blob.core.windows.net/wordpress/2011/08/iOSACS7_thumb.jpg)](https://wadewegner.blob.core.windows.net/wordpress/2011/08/iOSACS7.jpg)

 

Click on the **Generate** link in order to generate a set of default rules for this rule group.

 

![iOSACS8](https://wadewegner.blob.core.windows.net/wordpress/2011/08/iOSACS8.jpg)

 

Select the providers that you wish to use, and click on the **Generate** button. Once this is complete, you should see a set of rules.

 

![iOSACS9](https://wadewegner.blob.core.windows.net/wordpress/2011/08/iOSACS9.jpg)

 

After this step is complete, ACS has now been configured correctly to be used with your iOS application. Make a note of your Service Namespace (found at the top of the portal) and Realm as you’ll need these in the next step of the walkthrough.

 

**Step 2 – Create New Project and Import Library**

 

The following has been tested with XCode 4.0.2 or higher.

 

Before you begin, you should download the latest version of the Windows Azure Toolkit for iOS library from [_http://github.com/microsoft-dpe/watoolkitios-lib_](http://github.com/microsoft-dpe/watoolkitios-lib). Save the zip file in a place that can be found later.

 

To start, launch XCode 4 and create a new project.

 

![iOSACS10](https://wadewegner.blob.core.windows.net/wordpress/2011/08/iOSACS10.jpg)

 

From the project template dialog, select a **View-based application** and click on the **Next** button.

 

![iOSACS11](https://wadewegner.blob.core.windows.net/wordpress/2011/08/iOSACS11.jpg)

 

Enter a **Product Name** and **Company Identifier** and click on the **Next** button to continue. Select a directory to use for the project file and return to the IDE.

 

Next, locate the version of the Windows Azure toolkit for iOS library that you downloaded earlier. In the download will be a zip file containing two versions of the library (one for the device, one for the simulator) and some header files for the project.

 

Right click on your project and select the **Add Files to…** menu option.

 

![iOSACS12](https://wadewegner.blob.core.windows.net/wordpress/2011/08/iOSACS12.jpg)

 

Locate the .a file (for the simulator) and header files and add them to your project. You may want to create a new group (called lib) to store these in.

 

![iOSACS13](https://wadewegner.blob.core.windows.net/wordpress/2011/08/iOSACS13.jpg)

 

Now we need to add a reference to a library required for XML parsing. To do this, click on the top most project file, click on the target in the 2nd column of the IDE, and select **Build Phases** from the tab menu.

 

![iOSACS14](https://wadewegner.blob.core.windows.net/wordpress/2011/08/iOSACS14.jpg)

 

In the main window, expand the **Link Binary with Libraries** option.

 

Ensure that the libwatoolkitios.a file has been automatically added as a reference, click the + button to add a new library, and select the libxml2.dylib library from the drop down list.

 

![iOSACS15](https://wadewegner.blob.core.windows.net/wordpress/2011/08/iOSACS15.jpg)

 

Click on the **Add** button to add a reference to this library for your project.

 

Before we start adding any code, we need to add a couple of required linker flags to the project. To do this, click on the **Build Settings** tab (next to Build Phases).

 

In the search box, type “other linker” to filter the settings.

 

![iOSACS16](https://wadewegner.blob.core.windows.net/wordpress/2011/08/iOSACS16.jpg)

 

You should see a setting called **Other Linker Flags**. Double click on the right side of this row to add new flags.

 

Click on the + button to add two flags. The first is **–ObjC** and the second is **–all_load**. Once complete, your linker flags should look like the following screenshot:

 

![iOSACS17](https://wadewegner.blob.core.windows.net/wordpress/2011/08/iOSACS17.jpg)

 

Click on the **Done** button to save these settings. The project is now configured correctly to reference the Windows Azure Toolkit library.

 

To test that the library works, click on the project’s **[ProjectName]AppDelegate.m** file. Add the following #import statement at the top of the class:

 

#import "WACloudAccessControlClient.h"

 

Next, search for a method called **didFinishLaunchingWithOptions **and after the **[self.window makeKeyAndVisible]** line, enter the following code.

 

NSLog(@"Intializing the Access Control Client...");            
WACloudAccessControlClient *acsClient = [WACloudAccessControlClient accessControlClientForNamespace:@"iostest-walkthrough" realm:@"uri:wazmobiletoolkit"];            
[acsClient showInViewController:self.viewController allowsClose:NO withCompletionHandler:^(BOOL authenticated)            
{             
if (!authenticated)          
{              
NSLog(@"Error authenticating");          
}            
else          
{              
NSLog(@"Creating the authentication token...");            
WACloudAccessToken *token = [WACloudAccessControlClient sharedToken];          
/* Do something with the token here! */          
}              
}];

 

Replace the namespace and realm in the first line with the service namespace and realm for your own service, as created in Step 1.

 

As you can see from the above, the code creates a new instance of the access control client, requests that the client shows itself in the current view controller, and then extracts a token.

 

Build and run the application in the iOS Simulator.

 

Once the application starts, you should be prompted to select an identity provider from the list that you configured in your ACS service.

 

![iOSACS18](https://wadewegner.blob.core.windows.net/wordpress/2011/08/iOSACS18.jpg)

 

Pick one of the providers, and enter a valid set of credentials.

 

![iOSACS19](https://wadewegner.blob.core.windows.net/wordpress/2011/08/iOSACS19.jpg)

 

Click on the **Remember me** checkbox if you want to skip this step when running this application again, and click on the **Sign in** button.

 

The first time the application is run, you’ll be prompted to authorize the application to access your provider data.

 

[![iOSACS20](https://wadewegner.blob.core.windows.net/wordpress/2011/08/iOSACS20_thumb.jpg)](https://wadewegner.blob.core.windows.net/wordpress/2011/08/iOSACS20.jpg)

 

Click on the **Allow** button to continue. The login window will now disappear and you’ll be returned to your application.

 

In the debug window, you should see the following two logs:

 

2011-07-22 10:12:26.284 iostest-walkthrough[25838:207] Intializing the Access Control Client...            
2011-07-22 10:12:36.359 iostest-walkthrough[25838:207] Creating the authentication token...

 

The final message indicates that the access token was retrieved and can be used for further use. The **WACloudAccessToken** (derived from **[WACloudAccessControlClient sharedToken]**) contains an NSDictionary of claims and other properties that can be stored within your application. Using these properties on future calls can be used to identify returning users to your application.

 

Congratulations! You’ve successfully built an application that can use the Windows Azure ACS service for federated identity!

 

**Step 3 – Expanding the solution with the Cloud Ready Package**

 

If you have deployed the Cloud Ready package and configured your ACS service with the Cloud Ready Configuration Utility, you can also extend your sample by using the returned **WACloudAccessToken** to securely access blob, table, and queue storage in Windows Azure.

 

To do this, create a new **WAAuthenticationCredential** object using the token, use this to initialize the **WACloudStorageClient** and then make calls to the storage type. 

 

The following code example shows how this can be done.

 

WAAuthenticationCredential *credential = [WAAuthenticationCredential authenticateCredentialWithProxyURL:[NSURL URLWithString:@"[URL OF YOUR CLOUD READY PACKAGE]"] accessToken:[WACloudAccessControlClient sharedToken]];          
NSLog(@"Creating the storage client...");

 

WACloudStorageClient *storageClient = [WACloudStorageClient storageClientWithCredential:credential];          
[WACloudStorageClient ignoreSSLErrorFor:@"[FIRST PART OF THE URL FOR YOUR CLOUD READY PACKAGE]"];          
NSLog(@"Accessing table storage...");

 

[storageClient fetchTablesWithCompletionHandler:^(NSArray *tables, NSError *error)          
{           
if (!error)          
{            
NSLog(@"%i Tables were found!", [tables count]);            
UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Table Storage" message:[NSString stringWithFormat:@"%i tables were found!",[tables count]] delegate:nil cancelButtonTitle:@"OK" otherButtonTitles:nil];          
[alert show];          
[alert release];          
}            
else            
{            
NSLog(@"%@", [error localizedDescription]);          
}            
}];

 

Replace the URL strings in the above code with the correct URLs for your deployed Cloud Ready package. If everything works, after you sign in to the application you should see an alert box showing the number of tables in your storage account.
