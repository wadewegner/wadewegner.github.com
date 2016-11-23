---
author: admin
comments: true
date: 2011-05-06 22:48:41+00:00
layout: post
slug: windows-azure-toolkit-for-ios
title: Getting Started with the Windows Azure Toolkit for iOS
wordpress_id: 1160
categories:
- iOS
- Objective-C
- Toolkit
- Windows Azure
- Xcode
---

**IMPORTANT UPDATE:**

> Since the initial launch of the [Windows Azure Toolkit for iOS](http://github.com/microsoft-dpe/wa-toolkit-ios), we’ve not only updated & changed the library, but we’ve also renamed the repository. For the latest information, please visit our github repository at [http://github.com/microsoft-dpe/](http://github.com/microsoft-dpe/) – from here you can get access to the iOS library, Android library, and all the additional tools we’ve made available.        

I am extremely excited to announce the immediate availability of the Windows Azure Toolkit for iOS!

This first release of the Windows Azure Toolkit for iOS provides an easy and convenient way of accessing Windows Azure storage from iOS-based applications. As with the [Windows Azure Toolkit for Windows Phone 7](http://watoolkitwp7.codeplex.com/) we will continue to bring additional capabilities to the toolkit, such as push notifications, Access Control Service, and more.

![iOSImage1](https://wadewegner.blob.core.windows.net/wordpress/2011/05/iOSImage1.png) ![iOSImage2](https://wadewegner.blob.core.windows.net/wordpress/2011/05/iOSImage2.png) ![iOSImage3](https://wadewegner.blob.core.windows.net/wordpress/2011/05/iOSImage3.png)

You can get the toolkit—and all the source code—on github:

* Source code & zip: [https://github.com/microsoft-dpe/watoolkitios-lib](https://github.com/microsoft-dpe/watoolkitios-lib)

* Sample application: [https://github.com/microsoft-dpe/watoolkitios-samples](https://github.com/microsoft-dpe/watoolkitios-samples)

* Documentation: [https://github.com/microsoft-dpe/watoolkitios-doc](https://github.com/microsoft-dpe/watoolkitios-doc)

The toolkit works in two ways: the toolkit can be used to access Windows Azure storage directly, or alternatively, can go through a proxy service. The proxy service code is the same code as used in the [Windows Azure Toolkit for Windows Phone 7](http://watoolkitwp7.codeplex.com/) and negates the need for the developer to store the Azure storage credentials locally on the device.

The release of the Windows Azure Toolkit for iOS is a significant milestone, and reinforces my opinion that Windows Azure is a great place to run services for mobile applications.

**Setting up your Windows Azure services**

To quickly get your mobile services up and running in Windows Azure, take a look at the [Cloud Ready Package for Devices](https://github.com/downloads/microsoft-dpe/watoolkitios-lib/cloudready.devices.zip) (found under downloads in [https://github.com/microsoft-dpe/watoolkitios-lib](https://github.com/microsoft-dpe/watoolkitios-lib)).

The Cloud Ready Package for Devices is designed to make it easier for you to build mobile applications that leverage cloud services running in Windows Azure. Instead of having to open up Visual Studio and compile a solution with the services you want to use, we provide you with the Windows Azure CSPKG and CSCFG files prebuilt - all you need to do is update the configuration file to point to your account.

In this video, you'll see how easy it is to deploy this package to Windows Azure regardless of your operating system (e.g. Windows 7 or OSX) and target device (e.g. Windows Phone 7, iOS, or Android).

**Unpacking the v1.0.0 library zip file**

You can download the [compiled storage library on github](https://github.com/downloads/microsoft-dpe/watoolkitios-lib/v1.0.0.zip) (found under downloads in [https://github.com/microsoft-dpe/watoolkitios-lib](https://github.com/microsoft-dpe/watoolkitios-lib)). When you upzip the zip file, you’ll find several folders:

* /4.3-device – the library binary for iOS 4.3 (device) 

* /4.3-simulator – the library binary for iOS 4.3 (simulator) 

* /include – the headers for the library 

**Creating your first project using the toolkit**

If you are not familiar with XCode, this is a short tutorial for getting your first project up and running. Launch **XCode 4** and create a new project:

![](https://wadewegner.blob.core.windows.net/wordpress/2011/05/clip_image0024_thumb.jpg)

Select a **View-based application** and click **Next**.

Give the project a name and company. For the purposes of this walkthrough, we’ll call it “FirstAzureProject”. Do not include Unit Tests.

 

![](https://wadewegner.blob.core.windows.net/wordpress/2011/05/clip_image0044_thumb.jpg)

 

Pick a folder to save the project to, and uncheck the source code repository checkbox.

 

When the project opens, right click on the Frameworks folder and select “Add Files to…”

 

![clip_image006[4]_thumb](https://wadewegner.blob.core.windows.net/wordpress/2011/05/clip_image0064_thumb.jpg)

 

Locate the **libwatoolkitios.a** library file from the download package folder (from either the simulator or device folder), and add it to the Frameworks folder.

 

![image](https://wadewegner.blob.core.windows.net/wordpress/2011/05/image8.png)

 

Now, click on the top most project (FirstAzureProject) in the left hand column. Click on the target in the second column. Click on the “Build Settings” header in the third column. Ensure that the “All” button is selected to show all settings.

 

In the search box, type in “header search” and look for an entry called “Header Search Paths”:

 

![image](https://wadewegner.blob.core.windows.net/wordpress/2011/05/image9.png)

 

Double-click on this line (towards the right of the line), and click on the “+” button in the lower left.

 

![image](https://wadewegner.blob.core.windows.net/wordpress/2011/05/image10.png)

 

Add the path to where the folder containing the header files are located (this is the include folder from the download). For example, "~/Desktop/v1.0.0/include" if you have extracted the folder on your desktop. Be sure to encapsulate in quotes if you have spaces in the path.

 

![image](https://wadewegner.blob.core.windows.net/wordpress/2011/05/image11.png)

 

Now, click on the “Build Phases” tab and expand the “Link Binary with Libraries” section:

 

![image](https://wadewegner.blob.core.windows.net/wordpress/2011/05/image12.png)

 

Click on the “+” button in the lower left, and scroll down until you find a library called “libxml2.2.7.3.dylib”. Add this library to your project.

 

**Testing Everything Works**

 

Now that you’ve added all of the required references, let’s test that the library can be called. To do this, double click on the [ProjectName]AppDelegate.m file (e.g. FirstAzureProjectAppDelegate.m), and add the following imports to the class:

	#import "AuthenticationCredential.h"
	#import "CloudStorageClient.h"

Perform a build. If the build succeeds, the library is correctly added to the project. If it fails, it is recommended to go back and check the header search paths.

Assuming it builds, in the .m file, add the following declarations after the @synthesize lines:
	
	AuthenticationCredential *credential;
	
	CloudStorageClient *client;

Now, add the following lines after the [self.window makeKeyAndVisible] line in the didFinishLaunchingWithOptions method:
	 
	credential = [AuthenticationCredential credentialWithAzureServiceAccount:@"ACCOUNT_NAME" accessKey:@"ACCOUNT_KEY"];
	client = [CloudStorageClient storageClientWithCredential:credential];
	
	[client getBlobContainersWithBlock:^(NSArray* containers, NSError* error)
	{
		if (error)
		{
			NSLog(@"%@",[error localizedDescription]);
		}
		else
		{
			NSLog(@"%i containers were found...",[containers count]);
		}
	}];

Be sure to replace ACCOUNT_NAME and ACCOUNT_KEY with your Windows Azure storage account name and key, available on the Windows Azure portal ([https://manage.windowsazure.com)](https://manage.windowsazure.com).

Build and run the project. You should something similar to the following output in the debug window:

	2011-05-06 18:18:46.001 FirstAzureProject[27456:207] 2 containers were found...  

The last line shows that this account has 2 containers. This will of course vary, depending on how many blob containers you have setup in your own Windows Azure account.

**Doing more with the toolkit**

Feel free to explore the class documentation to explore more of the toolkit API. To help, here are some additional examples:

In [ProjectName]AppDelegate.m class, add the following headers:

	#import "AuthenticationCredential.h"
	#import "CloudStorageClient.h"
	#import "BlobContainer.h"
	#import "Blob.h"
	#import "TableEntity.h"
	#import "TableFetchRequest.h"

In the **didFinishLaunchingWithOptions** method, after the **[self.window makeKeyAndVisible]** line, try testing a few of the following commands. Again, running the project will return results into the debugger window.

To authenticate using account name and key:
	
	credential = [AuthenticationCredential credentialWithAzureServiceAccount:@"ACCOUNT_NAME" accessKey:@"ACCOUNT_KEY"];

To authenticate instead using the proxy service from the Windows Phone 7 toolkit, you can use the following:
	
	credential = [AuthenticationCredential authenticateCredentialWithProxyURL:[NSURL URLWithString:@"PROXY_URL"] user:@"USERNAME" password:@"PASSWORD" withBlock:^(NSError *error)
	{
		if (error)
		{
			NSLog(@"%@",[error localizedDescription]);
		}
		else
		{
			NSLog(@"Successfully logged in");
		}
	}];

Replace the PROXY_URL, USERNAME, and PASSWORD with the information required to access your proxy service.

To create a new client using the credentials:

	client = [CloudStorageClient storageClientWithCredential:credential];

To list all blob containers (this method is not supported via the proxy server):

	// get all blob containers
	[client getBlobContainersWithBlock:^(NSArray *containers, NSError *error)
	{
		if (error)
		{
			NSLog(@"%@",[error localizedDescription]);
		}
		else
		{
			NSLog(@"%i containers were found...",[containers count]);
		}
	}];

To get all blobs within a container (this also is not supported by the proxy):

	// get all blobs within a container
	[client getBlobs:@"images" withBlock:^(NSArray *blobs, NSError *error)
	{
		if (error)
		{
			NSLog(@"%@",[error localizedDescription]);
		}
		else
		{
			NSLog(@"%i blobs were found in the images container...",[blobs count]);
		}
	}];

To get all tables from storage (this works with both direct access and proxy):

	// get all tables
	[client getTablesWithBlock:^(NSArray* tables, NSError* error) 
		{
		if (error)
		{
			NSLog(@"%@",[error localizedDescription]);
		}
		else
		{
			NSLog(@"%i tables found",[tables count]);
		}
	}];

To create a table (works with both direct access and proxy):

	// create table
	[client createTableNamed:@"wadestable" withBlock:^(NSError *error)
	{
		if (error)
		{
			NSLog(@"%@",[error localizedDescription]);
		}
		else
		{
			NSLog(@"Table created");
		}
	}];

To delete a table (works with both direct access and proxy):

	//delete a table
	[client deleteTableNamed:@"wadestable" withBlock:^(NSError *error)
	{
		if (error)
		{
			NSLog(@"%@",[error localizedDescription]);
		}
		else
		{
			NSLog(@"Table was deleted");
		}
	}];

To get entities for a table (works with both account key and proxy):

	// get entities for table developers
	TableFetchRequest* fetchRequest = [TableFetchRequest fetchRequestForTable:@"Developers"];
	
	[client getEntities:fetchRequest withBlock:^(NSArray *entities, NSError *error)
	{
		if (error)
		{
			NSLog(@"%@",[error localizedDescription]);
		}
		else
		{
			NSLog(@"%i entities found in the developer table",[entities count]);
		}
	}];

To get entities for a table using predicate (works with both account key and proxy):

	// get entities for table developers with predicate request
	NSError* error = nil;
	NSPredicate* predicate = [NSPredicate predicateWithFormat:@"Name = 'Wade' || Name = 'Vittorio' || Name = 'Nathan'"];
	TableFetchRequest* anotherFetchRequest = [TableFetchRequest fetchRequestForTable:@"Developers" predicate:predicate error:&error];
	
	[client getEntities:anotherFetchRequest withBlock:^(NSArray *entities, NSError *error)
	{
		if (error)
		{
			NSLog(@"%@",[error localizedDescription]);
		}
		else
		{
			NSLog(@"%i entities returned by this request",[entities count]);
		}
	}];

**Doing even more with the toolkit**

If you are looking to explore the toolkit further, I recommend looking at the sample application that can be found in the [watoolkitios-samples](https://github.com/microsoft-dpe/watoolkitios-samples) project. This project demonstrates all of the functionality of the toolkit, including creating, uploading, and retrieving entities from both table and blob storage.