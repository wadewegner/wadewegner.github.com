---
author: admin
comments: true
date: 2010-05-11 21:01:41+00:00
layout: post
slug: net-framework-4-0-and-the-azure-appfabric-sdk
title: Using the .NET Framework 4.0 with the Azure AppFabric SDK
wordpress_id: 603
categories:
- .NET 4.0
- Azure AppFabric
---

The other day I attempted to build a sample application that communicated with the [Azure AppFabric](http://www.microsoft.com/windowsazure/appfabric/) Service Bus by creating a Console application targeting the .NET Framework 4.0. After adding a reference to **Microsoft.ServiceBus** I was bewildered to see that my Service Bus bindings in the **system.ServiceModel** section were not recognized.

I soon realized that the issue was the machine.config file. When you install the Azure AppFabric SDK the relevant WCF extensions are added to the .NET Framework 2.0 machine.config file, which is shared by .NET Framework 3.0 and 3.5. However, .NET Framework 4.0 has its own machine.config file, and the SDK will not update the WCF extensions.

Fortunately, there’s an easy solution to this issue: use the CLR’s **requiredRuntime** feature.
  
1. Create a configuration file named **RelayConfigurationInstaller.exe.config** in the “C:%Program Files%Windows Azure platform AppFabric SDKV1.0Assemblies” folder with the following code:        
       
    <?xml version ="1.0"?>
    <configuration>  
    <startup>    
        <requiredRuntime safemode="true" imageVersion="v4.0.30319" version="v4.0.30319"/>  
    </startup>
    </configuration>
   
2. Open up an elevated Visual Studio 2010 Command Prompt, browse to the directory, and run: **RelayConfigurationInstaller.exe/ i**
 
Your .NET Framework 4.0 machine.config file will now have the required configuration settings for the Service Bus bindings. Thanks to Vishal Chowdhary for the insight!