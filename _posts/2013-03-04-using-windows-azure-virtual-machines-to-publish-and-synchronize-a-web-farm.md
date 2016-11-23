---
author: admin
comments: true
date: 2013-03-04 04:47:29+00:00
layout: post
slug: using-windows-azure-virtual-machines-to-publish-and-synchronize-a-web-farm
title: Using Windows Azure Virtual Machines to Publish and Synchronize a Web Farm
wordpress_id: 1977
categories:
- IAAS
- PowerShell
- Virtual Machine
- Web Deploy
- Windows Azure
---

Lately I’ve been attempting to try out a number of different techniques for publishing web applications – both websites and web APIs – to the cloud. I’m being purposely vague when saying “the cloud” – it could be Windows Azure, AWS, or even a traditional hosting provider. It’s likely you saw a [great post by Michael Washam](http://michaelwasham.com/2012/08/13/publishing-and-synchronizing-web-farms-using-windows-azure-virtual-machines/) that provides a solution for using Windows Azure virtual machines along with Web Deploy and a few PowerShell scripts. What’s below is a technique adapted from Michael’s post and detailed for each of the steps. What I like about this approach is that, aside from the machine preparation, the deployment technique is consistent across different platforms.  

There are certainly a few things missing below: scripts for scaling up/down, scripts for running Windows update, and so forth. I’ll tackle these in future posts. My point here is to highlight a different way to look at tackling a common problem.

What are some of the advantages to this approach?

* Near instant deployment.  
* Fully automated.  
* Consistent development model with other platforms.  
* Consistent deployment model with other platforms.

I’m sure there are more. There are some limitations as well – in particular, you’re bound by the number of roles you can have in your Cloud Service, which today is (I think) 25. This means that, at most, you can only scale out to 25 virtual machines. I imagine this limitation will be removed at some point (or perhaps you could get around it by using virtual networks).

Regardless, give it a try.

**Note**: You can find the PowerShell scripts used below in this gist here: [https://gist.github.com/wadewegner/5080142](https://gist.github.com/wadewegner/5080199).

# Image Preparation

In this first step you’ll create and customize a virtual machine that you’ll then sysprep and turn into a disk that we’ll use later on.  

##### _Create the Base Virtual Machine_

This first part takes the longest. This is because you’re preparing the disk you’ll use moving forward. Note that you only do it once – this is not something you have to do over and over again.  

1. Create a Virtual Machine.
 
	![WAIIASImage1](http://www.wadewegner.com/content/acc7ea03346c_12181/image_thumb.png) 

2. Enter the VM information. Use the default “Windows Server 2012 Datacenter” image. Size doesn’t matter. This VM will sysprepped and used as a disk image for creating future virtual machines. Be sure to note the location you use and the password you create - you'll need them!

	![WAIIASImage2](http://www.wadewegner.com/content/acc7ea03346c_12181/image_thumb_3.png)
	
3. Start the VM. Note that VMs create a Cloud Service. 

4. RDP into the VM once it has provisioned and started.

	![WAIIASImage3](http://www.wadewegner.com/content/acc7ea03346c_12181/image_thumb_4.png)

5. Click to **Add roles and features**.  

	![image](http://www.wadewegner.com/content/acc7ea03346c_12181/image_thumb_5.png)

6. Leave defaults and click **Next** until you get to Server Roles.  

7. Choose **Application Server** and **Web Server (IIS)**. Click **Next**.  

	![image](http://www.wadewegner.com/content/acc7ea03346c_12181/image_thumb_6.png)

8. Under **Features** be sure to select **ASP.NET 4.5**.  

	![image](http://www.wadewegner.com/content/acc7ea03346c_12181/image_thumb_7.png) 

9. Click **Next** until you get to **Role Services** under **Web Server Role (IIS).**

10. Add **ASP.NET 4.5** under **Application Development**. This will add the other default values.  

	![image](http://www.wadewegner.com/content/acc7ea03346c_12181/image_thumb_8.png)

11. Click **Next** until you get to the last step. Click **Install**. Wait until the operation completes.  

**_Install and Use Windows Azure PowerShell Cmdlets_**

12. From the machine, download the Windows Azure PowerShell Cmdlets. These can be found at: [http://www.windowsazure.com/en-us/downloads/](http://www.windowsazure.com/en-us/downloads/).  

13. Install the cmdlets. This can take 5-15 minutes.  

	![image](http://www.wadewegner.com/content/acc7ea03346c_12181/image_thumb_9.png)

14. Create the folder **c:\\Scripts**.  

15. Run the following script in **Windows PowerShell ISE** to download your publish settings file:

		Get-AzurePublishSettingsFile 

16. Save or move this file into the **c:\\Scripts** folder and rename to **credentials.publishsettings**.  

17. Run the following script in **Windows PowerShell ISE** to change the execution policy:  
Set-ExecutionPolicy Unrestricted  

**_Setup Web Deploy_**

18. Download **Web Deploy 3.0** from the following link: [http://download.microsoft.com/download/1/B/3/1B3F8377-CFE1-4B40-8402-AE1FC6A0A8C3/WebDeploy_amd64_en-US.msi](http://download.microsoft.com/download/1/B/3/1B3F8377-CFE1-4B40-8402-AE1FC6A0A8C3/WebDeploy_amd64_en-US.msi). **Do not install**. To simplify, download to C:\\.  

19. Open a **Command Prompt**.  

20. Run the following command:  

		C:\>msiexec /I webdeploy_amd64_en-us.msi /passive ADDLOCAL=ALL LISTENURL=http://+:8080/  

	![image](http://www.wadewegner.com/content/acc7ea03346c_12181/image_thumb_10.png)

**_Configure Firewall Settings_**

21. Run **Windows Firewall with Advanced Security**.  

22. Select **Inbound Rules** and click **New Rule**.  

23. Select **Port** and click **Next**.  

24. Level TCP selected and enter **8080** for the **Specific local ports**.  

	![image](http://www.wadewegner.com/content/acc7ea03346c_12181/image_thumb_11.png)

25. Keep the defaults and click **Next** until prompted to give a name. Choose a name (e.g. Web Deploy Port) and click **Finish**.  

**_System Preparation_**

26. Open a **Command Prompt** as an administrator.  

27. Change the directory to: %windir%\\system32\\sysprep  

28. Run **sysprep.exe**.  

	![image](http://www.wadewegner.com/content/acc7ea03346c_12181/image_thumb_12.png)

29. Ensure the following options are selected: **Enter System Out-of-Box Experience (OOBE)**, **Generalize**, and **Shutdown**.

	![image](http://www.wadewegner.com/content/acc7ea03346c_12181/image_thumb_13.png)

**_Capture the Virtual Machine_**

30. Return to your Virtual Machine Instances tab in the portal: [https://manage.windowsazure.com/#Workspace/VirtualMachineExtension/vms](https://manage.windowsazure.com/#Workspace/VirtualMachineExtension/vms).  

31. Wait until you see that the status of your virtual machine is **Stopped**.  

	![image](http://www.wadewegner.com/content/acc7ea03346c_12181/image_thumb_14.png)

32. Capture the image by clicking the **CAPTURE** button below.  

	![clip_image024](http://www.wadewegner.com/content/acc7ea03346c_12181/clip_image024_thumb.png)  

33. Select an image name (e.g. WS2012-WebFarmImage) and check that you have sysprepped the machine. Click the button.  

	![image](http://www.wadewegner.com/content/acc7ea03346c_12181/image_thumb_15.png) 

34. When the capture operation completes you’ll see the disk image available under **Images** in the portal: [https://manage.windowsazure.com/#Workspaces/VirtualMachineExtension/images](https://manage.windowsazure.com/#Workspaces/VirtualMachineExtension/images)  

	![image](http://www.wadewegner.com/content/acc7ea03346c_12181/image_thumb_16.png)

# Virtual Machine Deployment

You’ll run these steps from your own PC.  

1. Open the **Windows PowerShell ISE** to run the following scripts. It’s always a good idea to save them somewhere to use later.  

2. You might need to import your publish settings. If so, download your publish settings file, save it, then import it.

		Get-AzurePublishSettingsFile

		Import-AzurePublishSettingsFile C:\\credentials.publishsettings

3. Get the subscription name to which you’ll deploy. Run the following script:

		Get-AzureSubscription -Current  

	![image](http://www.wadewegner.com/content/acc7ea03346c_12181/image_thumb_17.png)

4. You need to get the storage account that contains your disk image. You’ll need to use the same storage account for the disks attached to the web servers in your web farm. Select the **Images** tab under **Virtual Machines** on the portal. Find your image and look at the location. The first part of the URL is the storage account name.  

	![image](http://www.wadewegner.com/content/acc7ea03346c_12181/image_thumb_18.png)

5. It’s now time to deploy your web farm using the disk image you created in the previous section. Review the following script and update appropriately. 

		$imgname = 'WS2012-WebFarmImage'
		$cloudsvc = 'DemoWebFarm'
		$pass = 'Password'
		$subscriptionName = 'Windows Azure MSDN - Visual Studio Ultimate'
		$storageAccount = 'portalvhds9dvbvvff5hdg3'
		$location = 'East US'

		Set-AzureSubscription -SubscriptionName $subscriptionName -CurrentStorageAccount $storageAccount

		$iisvm1 = New-AzureVMConfig -Name 'iis1' -InstanceSize Small -ImageName $imgname |
		    Add-AzureEndpoint -Name web -LocalPort 80 -PublicPort 80 -Protocol tcp -LBSetName web -ProbePath '/' -ProbeProtocol http -ProbePort 80 |
		    Add-AzureEndpoint -Name webdeploy -LocalPort 8080 -PublicPort 8080 -Protocol tcp | 
		    Add-AzureProvisioningConfig -Windows -Password $pass
    
		$iisvm2 = New-AzureVMConfig -Name 'iis2' -InstanceSize Small -ImageName $imgname |
		    Add-AzureEndpoint -Name web -LocalPort 80 -PublicPort 80 -Protocol tcp -LBSetName web -ProbePath '/' -ProbeProtocol http -ProbePort 80 |
		    Add-AzureProvisioningConfig -Windows -Password $pass
    
		$iisvm3 = New-AzureVMConfig -Name 'iis3' -InstanceSize Small -ImageName $imgname |
		    Add-AzureEndpoint -Name web -LocalPort 80 -PublicPort 80 -Protocol tcp -LBSetName web -ProbePath '/' -ProbeProtocol http -ProbePort 80 |
		    Add-AzureProvisioningConfig -Windows -Password $pass    
    
		New-AzureVM -ServiceName $cloudsvc -VMs $iisvm1, $iisvm2, $iisvm3 -Location $location

Notes on script: 

  * **$imgname**: This is the name of the disk image you created. 

  * **$cloudsvc**: When you deploy it will create a cloud service. This is the name for the cloud service. 

  * **$pass**: This is the password for the virtual machine you created. 

  * **$subscriptionName**: This is your subscription name. 

  * **$location**: The datacenter you want to deploy into. Note: this has to be the same data center as your storage account. 

It is _important_ to note that the setup for $iisvm1 is different than $iisvm2 and $iisvm3. That’s because we’re adding an additional endpoint named **webdeploy** on 8080. We’ll use this public port for deploying via web deploy. It is not opened on the other machines. 

6. It’s time to run the script. Note: the script does not rollback. If you have values that are incorrect you’ll have to clean them up manually. If all goes well, you’ll see progress indicators like the following:  

	![image](http://www.wadewegner.com/content/acc7ea03346c_12181/image_thumb_19.png)

	![image](http://www.wadewegner.com/content/acc7ea03346c_12181/image_thumb_20.png)

7. You can now look in the portal and you’ll see the following machines Running/Provisioning.  

	![image](http://www.wadewegner.com/content/acc7ea03346c_12181/image_thumb_21.png)

8. You can browse the URL, for example: [http://demowebfarm.cloudapp.net](http://demowebfarm.cloudapp.net). You’ll get the standard IIS page:  

	![image](http://www.wadewegner.com/content/acc7ea03346c_12181/image_thumb_22.png)

# Setup Synchronization across Virtual Machine

1. Remote into **iis1**. From the portal, select **iis1** under **Virtual Machine Instances** and click the **CONNECT** button. This will download an RDP file. Save this and use it for connecting to the machine later.  

	![clip_image039](http://www.wadewegner.com/content/acc7ea03346c_12181/clip_image039_thumb.png) 


2. Open **Windows PowerShell ISE**. 


3. Click **New** to create a new script. 


4. Copy the following script and save it as **C:\\Scripts\\Sync.ps1**. DO NOT RUN IT.

		$subscriptionName = 'Windows Azure MSDN - Visual Studio Ultimate'
		$storageAccount = 'portalvhds9dvbvvff5hdg3'
		$cloudsvc = ' DemoWebFarm' 
		$publishSettings = 'C:\\Scripts\\credentials.publishsettings'

		Import-Module 'C:\\Program Files (x86)\\Microsoft SDKs\\Windows Azure\\PowerShell\\Azure\\Azure.psd1'

		Import-AzurePublishSettingsFile $publishSettings
		Set-AzureSubscription -SubscriptionName $subscriptionName -CurrentStorageAccount $storageAccount

		$publishingServer = (gc env:computername).toLower()

		Get-AzureVM -ServiceName $cloudsvc | foreach { 
		    if ($_.Name.toLower() -ne $publishingServer) {
		       $target = $_.Name + ":8080"
		       $source = $publishingServer + ":8080"

		       $exe = "C:\\Program Files\\IIS\\Microsoft Web Deploy V3\\msdeploy.exe"
		       [Array]$params = "-verb:sync", "-source:contentPath=C:\\Inetpub\\wwwroot,computerName=$source", "-dest:contentPath=C:\\Inetpub\\wwwroot,computerName=$target";

		        & $exe $params;
		    }   
		}


**_Create a Scheduled Task_**

5. Open **Task Scheduler**. 

6. Click **Create Task ... **

	![image](http://www.wadewegner.com/content/acc7ea03346c_12181/image_thumb_23.png)

7. On the **General **tab set the **Name** and **Run wehather user is logged on or not**.  

	![image](http://www.wadewegner.com/content/acc7ea03346c_12181/image_thumb_24.png)

8. On the **Triggers** tab click **New**. Create a **Daily** trigger and **Repeat task every** 2 minutes. Click **OK**.  

	![image](http://www.wadewegner.com/content/acc7ea03346c_12181/image_thumb_25.png)

9. On the **Actions** tab click **New**. Set the following values:

	**Program/script**: C:\\Windows\\System32\\WindowsPowerShell\\v1.0\\powershell.exe  
	**Add arguments**: -File C:\\Scripts\\Sync.ps1  

	![image](http://www.wadewegner.com/content/acc7ea03346c_12181/image_thumb_26.png)

10. You should now see this as a scheduled task.  

	![image](http://www.wadewegner.com/content/acc7ea03346c_12181/image_thumb_27.png)

11. You can wait for this to run or run it manually. It should run successfully.  

	![image](http://www.wadewegner.com/content/acc7ea03346c_12181/image_thumb_28.png)

**_Deploy a Website through Web Deploy_**

1. Create a new **ASP.NET MVC 4 Web Applications**. (In actually it can be anything.) Choose an **Internet Application**.  

	![image](http://www.wadewegner.com/content/acc7ea03346c_12181/image_thumb_29.png)


2. Update the **HomeController.cs** code: 

		public ActionResult Index()
		{
    		string machineName = Environment.MachineName;
    		ViewBag.Message = "Computer Name: " + machineName;

		    return View();
		}

3. Right-click on your application and click **Publish**.  

	![image](http://www.wadewegner.com/content/acc7ea03346c_12181/image_thumb_30.png)

4. In the dropdown select **New**.  

	![image](http://www.wadewegner.com/content/acc7ea03346c_12181/image_thumb_31.png)

5. Give your publish profile a name. Click OK.  

	![image](http://www.wadewegner.com/content/acc7ea03346c_12181/image_thumb_32.png)

6. Update all the fields:  

	![image](http://www.wadewegner.com/content/acc7ea03346c_12181/image_thumb_33.png)

	**Server**: This is the DNS name of the cloud service with port 8080. 
	
	**Site name**: This needs to be “Default Web Site”. 
	
	**User name**: This is the administrator account. 
	
	**Password**: The password you set. 
	
	**Destination URL**: The same as server without the ports. 

7. Click **Publish.**

8. In Visual Studio you should see that it deployed successfully.  

	![image](http://www.wadewegner.com/content/acc7ea03346c_12181/image_thumb_34.png)

	**WARNING**: Don’t be alarmed if the browser opens and you don’t see your site. Remember that we set synchronization at two minutes and you have a 66% chance of hitting iis2 or iis3. 

9. If you go back to your RDP session with **iis1** and browse to C:\\inetpub\\wwwroot you’ll see all your content has been deployed. 

10. Either way two minutes or manually invoke your synchronization task. Hit the website again and start refreshing:  

	![image](http://www.wadewegner.com/content/acc7ea03346c_12181/image_thumb_35.png) 

	![image](http://www.wadewegner.com/content/acc7ea03346c_12181/image_thumb_36.png)

That’s it!

Yeah, that might seem like a lot but realize that most of the work detailed here – such as the image preparation – you’ll only have to do one time. The rest of the time you’re using tools like PowerShell and Visual Studio.

Enjoy!