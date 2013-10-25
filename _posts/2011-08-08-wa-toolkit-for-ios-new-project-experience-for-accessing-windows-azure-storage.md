---
author: admin
comments: true
date: 2011-08-08 22:15:50+00:00
layout: post
slug: wa-toolkit-for-ios-new-project-experience-for-accessing-windows-azure-storage
title: 'WA Toolkit for iOS: New Project Experience for Accessing Windows Azure Storage'
wordpress_id: 1427
categories:
- iOS
- Toolkit
- Windows Azure
---

The Windows Azure Toolkit for iOS provides an easy and convenient way of accessing Windows Azure storage from iOS-based applications.

 

The toolkit works in two ways – the toolkit can be used to access Windows Azure storage directly, or alternatively, can go through a proxy service known as the Cloud Ready package. The Cloud Ready code is the same code as used in the WP7 toolkit for Windows Azure (found [_here_](http://watoolkitwp7.codeplex.com/)) and negates the need for the developer to store the Azure storage credentials locally on the device. If you are planning to test using the proxy server, you’ll need to download and deploy the server controls hosted on the Codeplex site.

 

**Unpacking the v1.2 library zip file**

****  

In the zip file, you’ll find several folders:

 

      
  * /4.3-device – the library binary for iOS 4.3 (device)
   
  * /4.3-simulator – the library binary for iOS 4.3 (simulator)
   
  * /include – the headers for the library
 

**Creating your first project using the toolkit**

 

If you are not familiar with XCode, this is a short tutorial for getting your first project up and running. Launch XCode 4 and create a new project:

 

![iOS1](http://images.wadewegner.com/wordpress/2011/08/iOS1.jpg)

 

Select a View-based application and click Next. 

 

Give the project a name and company. For the purposes of this walkthrough, we’ll call it “FirstAzureProject”. Do not include Unit Tests.

 

![iOS2](http://images.wadewegner.com/wordpress/2011/08/iOS2.jpg)

 

Pick a folder to save the project to, and uncheck the source code repository checkbox.

 

When the project opens, right click on the Frameworks folder and select “Add Files to…”

 

![iOS3](http://images.wadewegner.com/wordpress/2011/08/iOS3.jpg)

 

Locate the libwatoolkitios.a library file from the download package folder (from either the simulator or device folder), and add it to the Frameworks folder.

 

![iOS4](http://images.wadewegner.com/wordpress/2011/08/iOS4.jpg)

 

Now, click on the top most project (FirstAzureProject) in the left hand column. Click on the target in the second column. Click on the “Build Settings” header in the third column. Ensure that the “All” button is selected to show all settings. 

 

In the search box, type in “header search” and look for an entry called “Header Search Paths”

 

![iOS5](http://images.wadewegner.com/wordpress/2011/08/iOS5.jpg)

 

Double click on this line (towards the right of the line), and click on the “+” button in the lower left.

 

![iOS6](http://images.wadewegner.com/wordpress/2011/08/iOS6.jpg)

 

Add the path to where the folder containing the header files are located (this is the include folder from the download). For example, "~/Desktop/v1.0.1/include" if you have extracted the folder on your desktop. Be sure to encapsulate in quotes if you have spaces in the path.

 

![iOS7](http://images.wadewegner.com/wordpress/2011/08/iOS7.jpg)

 

Remaining in the “Build Settings” tab, now type in “other linker flags” in to the search bar. In the “Other Linker Flags” section, add two Flags. **–ObjC** and **–all_load**

 

![iOS8](http://images.wadewegner.com/wordpress/2011/08/iOS8.jpg)

 

Finally, click on the “Build Phases” tab and expand the “Link Binary with Libraries” section:

 

![iOS9](http://images.wadewegner.com/wordpress/2011/08/iOS9.jpg)

 

Click on the “+” button in the lower left, and scroll down until you find a library called “libxml2.2.dylib”. Add this library to your project.

 

**Testing Everything Works**

 

Now that you’ve added all of the required references, let’s test that the library can be called. To do this, double click on the [ProjectName]AppDelegate.m file (e.g. FirstAzureProjectAppDelegate.m), and add the following imports to the class:

 

#import "WAAuthenticationCredential.h"            
#import "WACloudStorageClient.h"

 

Perform a build. If the build succeeds, the library is correctly added to the project. If it fails, it is recommended to go back and check the header search paths.

 

Assuming it builds, in the .m file, add the following declarations after the @synthesize lines:

 

WAAuthenticationCredential *credential;            
WACloudStorageClient *client;

 

Now, add the following lines after the **[self.window makeKeyAndVisible]** line in the **didFinishLaunchingWithOptions** method:

 

credential = [WAAuthenticationCredential credentialWithAzureServiceAccount:@"ACCOUNT_NAME" accessKey:@"ACCOUNT_KEY"];            
client = [WACloudStorageClient storageClientWithCredential:credential];            
[client fetchBlobContainersWithCompletionHandler:^(NSArray* containers, NSError* error)            
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

 

Be sure to replace ACCOUNT_NAME and ACCOUNT_KEY with your Windows Azure storage account name and key, available on the Windows Azure portal ([_http://windows.azure.com_](http://windows.azure.com)). 

 

Build and run the project. You should something similar to the following output in the debug window:

 

2011-05-06 18:18:46.001 FirstAzureProject[27456:207] 2 containers were found...

 

The last line shows that this account has 2 containers. This will of course vary, depending on how many blob containers you have setup in your own Windows Azure account. 

 

**Doing more with the toolkit**

 

Feel free to explore the class documentation to explore more of the toolkit API. To help, here are some additional examples:

 

In [ProjectName]AppDelegate.m class, add the following headers:

 

#import "WAAuthenticationCredential.h"            
#import "WACloudStorageClient.h"            
#import "WABlobContainer.h"            
#import "WABlob.h"            
#import "WATableEntity.h"            
#import "WATableFetchRequest.h"            
#import "WAQueue.h"            
#import "WAQueueMessage.h"

 

In the **didFinishLaunchingWithOptions** method, after the **[self.window makeKeyAndVisible]** line, try testing a few of the following commands. Again, running the project will return results into the debugger window.

 

To authenticate using account name and key:

 

credential = [WAAuthenticationCredential credentialWithAzureServiceAccount:@"ACCOUNT_NAME" accessKey:@"ACCOUNT_KEY"];

 

To authenticate instead using the proxy service from the Windows Phone 7 toolkit, you can use the following:

 

credential = [WAAuthenticationCredential authenticateCredentialWithProxyURL:[NSURL URLWithString:@"PROXY_URL"] user:@"USERNAME" password:@"PASSWORD" withCompletionHandler:^(NSError *error)            
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

 

client = [WACloudStorageClient storageClientWithCredential:credential];

 

To list all blob containers (this method is not supported via the proxy server):

 

// get all blob containers            
[client fetchBlobContainersWithCompletionHandler:^(NSArray *containers, NSError *error)            
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

 

To get all tables from storage (this works with both direct access and proxy):

 

// get all tables            
[client fetchTablesWithCompletionHandler:^(NSArray* tables, NSError* error)            
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

 

[client createTableNamed:@"testtable" withCompletionHandler:^(NSError *error)            
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
[client deleteTableNamed:@"wadestable" withCompletionHandler:^(NSError *error)            
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
WATableFetchRequest* fetchRequest = [WATableFetchRequest fetchRequestForTable:@"Developers"];            
[client fetchEntities:fetchRequest withCompletionHandler:^(NSArray *entities, NSError *error)            
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
NSPredicate* predicate = [NSPredicate predicateWithFormat:@"Name = 'Wade' || Name = 'Nathan' || Name = 'Nick'"];            
WATableFetchRequest* anotherFetchRequest = [WATableFetchRequest fetchRequestForTable:@"Developers" predicate:predicate error:&error];

 

[client fetchEntities:anotherFetchRequest withCompletionHandler:^(NSArray *entities, NSError *error)            
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

 

If you are looking to explore the toolkit further, we would recommend looking at the sample application that can be found in the watoolkitios-samples project. This project demonstrates all of the functionality of the toolkit, including creating, uploading, and retrieving entities from both table and blob storage.

 

In addition, you might also want to check out the documentation outlining the ACS (Access Control Service) functionality of the toolkit.
