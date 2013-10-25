---
layout: post
title: "PowerShell Script for Cleaning Up Your Windows Azure Resources"
description: ""
category:
- Windows Azure
- PowerShell
- Automation
---
{% include setup %}

> Special thanks to [Michael Washam](michaelwasham.com/‎) for helping me find and fix a bug in my code!

I find myself regularly creating resources in Windows Azure, such as cloud services, virtual machines, storage accounts, virtual networks, and the like. Most of the time this is part of writing scripts or testing out deployments, and I don't plan to keep them around indefinitely. The challenge this creates is how to cleanup all these resources. While it's possible to do this through the Windows Azure management portal you'll find in practice that it's slow and cumbersome.

Consequently, I've created a script which I use regularly to cleanup my resources.

There are times when I want to keep a particular cloud service or storage account around yet they belong to the same subscription as a lot of resources I want to delete. Fortunately it's easy to create an array of resource names and use the **-notin** operator in the script.

Before we start, a few quick tips:

1. Use the **Windows PowerShell ISE**. You'll find this makes life a lot easier.

2. Run the **Windows PowerShell ISE** as an administrator. I find it easiest to go to the **Advanced Properties** of the ISE and check the **Run as administrator** box.

	![Run as administrator](http://wadewegner.blob.core.windows.net/wordpress/2013/10/2013-10-25-RunAsAdmin.JPG)

3. Change the execution policy for PowerShell so that you can run your script in the ISE.

		Set-ExecutionPolicy RemoteSigned

4. Download the correct publish settings file from the Windows Azure management portal. The easiest way to do this is to run the following command in the ISE:

		Get-AzurePublishSettingsFile

Okay, now that this is out of the way, here's the script I use for removing unwanted resources while preserving those I want to keep.

	Import-AzurePublishSettingsFile "C:\temp\me.publishsettings"
	
	$sub = "Windows Azure MVP"
	
	Select-AzureSubscription -SubscriptionName $sub  
	Set-AzureSubscription -DefaultSubscription $sub
	
	$websitesToSave = "website1", "website2"
	$VMsToSave = "vm1", "vm2"
	$storageAccountsToSave = "storageaccount1"
	
	cls
	
	Get-AzureWebsite | Where {$_.Name -notin $websitesToSave} | Remove-AzureWebsite -Force
	
	Get-AzureService | Where {$_.Label -notin $VMsToSave} | Remove-AzureService -Force
	
	Get-AzureDisk | Where {$_.AttachedTo -eq $null} | Remove-AzureDisk -DeleteVHD
	
	Get-AzureStorageAccount | Where {$_.Label -notin $storageAccountsToSave} | Remove-AzureStorageAccount

	Get-AzureAffinityGroup | Remove-AzureAffinityGroup
	
	Remove-AzureVNetConfig
 

Notice that the first thing I do is ensure I've selected the correct subscription. Next I define the resources I do not want to remove.

When executed you'll see output similar to the following:

	WARNING: 12:17:44 PM - Removing cloud service vm3...
	WARNING: 12:17:46 PM - Removing Production deployment for webfarm9493uhp6np service
	WARNING: 12:18:19 PM - Removing cloud service vm4...
	WARNING: 12:18:21 PM - Removing Production deployment for webfarmcvotsjxpxa service
	WARNING: 12:18:54 PM - Removing cloud service vm5...
	WARNING: 12:18:57 PM - Removing Production deployment for webfarmwqpvdxl893 service
	WARNING: 12:22:30 PM - Removing cloud service vm6...
	
	OperationDescription         OperationId                            OperationStatus                                 
	--------------------         -----------                            ---------------                                 
	Remove-AzureDisk             8b8b5e5d-5c6b-2086-8882-d3c301004c2d   Succeeded                                       
	Remove-AzureDisk             64a75cf5-eb2c-2543-b32a-40fdf2b30a4e   Succeeded                                       
	Remove-AzureDisk             22af4853-e028-2b9b-9de2-cf58390ab833   Succeeded                                       
	Remove-AzureDisk             1df3ac85-4679-2edb-848e-7d8010d0cf25   Succeeded                                       
	Remove-AzureDisk             70e3551b-b0ab-2bcd-9e87-352e0a97e0c3   Succeeded                                       
	Remove-AzureDisk             3875a2af-9526-2f50-adf8-0ed99baaf675   Succeeded                                       
	Remove-AzureDisk             645615fa-c887-2431-8636-1b3e06b9e685   Succeeded                                       
	Remove-AzureDisk             c8dcd546-6161-23c6-8e24-e5e4725bce9d   Succeeded                                       
	Remove-AzureDisk             a79ff233-620a-23c4-89cc-34b963d55a01   Succeeded                                       
	Remove-AzureDisk             8b58db0d-aa6c-25d2-9939-f6c1912c8090   Succeeded                                       

	Remove-AzureStorageAccount   ffd7ef08-b913-2b20-aceb-ef06787a9674   Succeeded                                       
	Remove-AzureStorageAccount   26495110-66b0-230b-b98c-cb6d2b2b050e   Succeeded                                       
	Remove-AzureStorageAccount   9b85d71b-7f9a-2494-9eb3-9e00eb9b5820   Succeeded                                       
	Remove-AzureStorageAccount   806128ef-2990-2e27-bec0-c61ccb3fc7fe   Succeeded                                       
	Remove-AzureStorageAccount   850eb732-5df2-219c-82ed-e351a6b89f65   Succeeded                                       
	Remove-AzureStorageAccount   498b0283-4be3-2851-8abc-5b1da0108bf7   Succeeded                                       

    Remove-AzureAffinityGroup    07ddb3a8-7ef3-2e78-a30f-71753e77a6cb 	Succeeded                           

    Remove-AzureVNetConfig       d62374ca-c9c6-24b4-ae5d-31d93ada4623 	Succeeded 

**Note:** there's no way to specify a particular Virtual Network name.
Consequently, the best you can do is delete everything and, if the VNet is still in use it will throw an error. That's fine.

I hope it goes without saying that with **phenomenal cosmic power** comes great responsibility. You can _easily_ wreak havoc in your environments by deleting things you shouldn't. In fact, I would recommend you never run a script like this in a production environment; instead, focus on DEV/TEST environments.

Over time I'll like add additional resources to this list. For now I'm happy with the above as it removes most of the resources that have a monthly cost associated with them.

I hope this helps!