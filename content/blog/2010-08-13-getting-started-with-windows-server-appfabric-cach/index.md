---
title: "Getting Started with Windows Server AppFabric Cache"
date: 2010-08-13T00:00:00-08:00
draft: false
description: "From the whiteboard to the keyboard"
---

I struggled today to find a good “Getting started with Windows Server AppFabric Cache” tutorial – either my \_search fu \_failed me or it simply doesn’t exist. Nevertheless, I was able to piece together the information I needed to get started.

I recommend you break this up into three steps:

- Installing Windows Server AppFabric
- Configuring Windows Server AppFabric Cache
- Testing Windows Server AppFabric Cache with Sample Apps

I think this article will serve as a good tutorial on getting started, and we can refer back to it as the basis for more advanced scenarios.

**Installing Windows Serve AppFabric**

1. Get the [Web Platform Installer](http://www.microsoft.com/web/downloads/platform.aspx).
2. Once it is installed and opened, select **Options**.

   ![Options](https://wadewegner.blob.core.windows.net/wordpress/2010/08/clip_image0024.jpg)
3. Under **Display additional scenarios** select **Enterprise**.

   ![Enterprise](https://wadewegner.blob.core.windows.net/wordpress/2010/08/clip_image0026.jpg)
4. Now you’ll see an Enterprise tab. Select it, and choose **Windows Server AppFabric**. Click **Install**. This will start a multi-step process for installing Windows Server AppFabric (which in my case required two reboots to complete).

   ![Windows Server AppFabric](https://wadewegner.blob.core.windows.net/wordpress/2010/08/clip_image0028.jpg)

**Configuring Windows Server AppFabric Cache**

1. Open the **Windows Server AppFabric Configuration Wizard** (Start –> Windows Server AppFabric –> Configure AppFabric).
2. Click **Next** until you reach the **Caching Service** step. Check **Set Caching Service configuration**, select **SQL Server AppFabric Caching Service Configuration Store Provider** for the configuration provider, and click **Configure**.

   ![Set Caching Service](https://wadewegner.blob.core.windows.net/wordpress/2010/08/image3.png)
3. Check **Create AppFabric Caching Service configuration database**, confirm the \*\*Server \*\*name, and specify a **Database** name. Click **OK**.

   ![Specify Database](https://wadewegner.blob.core.windows.net/wordpress/2010/08/image4.png)
4. When asked if you want to continue, click **Yes**.

   ![Configuration Database confirmation](https://wadewegner.blob.core.windows.net/wordpress/2010/08/image5.png)
5. You will receive confirmation that your database was created and registered.

   ![Success](https://wadewegner.blob.core.windows.net/wordpress/2010/08/image6.png)
6. On the **Cache Node** step, confirm the selected port nodes.

   ![Cache Node ports](https://wadewegner.blob.core.windows.net/wordpress/2010/08/image7.png)
7. You will be asked to continue and apply the configuration settings; select **Yes**.

   ![Continue](https://wadewegner.blob.core.windows.net/wordpress/2010/08/image8.png)
8. On the last step you’ll click **Finish**.
9. Open up an elevated \*\*Windows PowerShell \*\*window.
10. Add the Distributed Cache administration module

    ```
    Import-Module DistributedCacheAdministration
    ```
11. Set the context of your Windows PowerShell session to the desired cache cluster with Use-CacheCluster. You can run this without parameters to use the connection parameters provided during configuration.

    ```
    Use-CacheCluster
    ```
12. Grant your user account access to the cache cluster as a client. Specify your user and domain name.

    ```
    Grant-CacheAllowedClientAccount domainusername
    ```
13. Verify your user account has been granted access.

    ```
    Get-CacheAllowedClientAccounts
    ```
14. Start the cluster.

    ```
    Start-CacheCluster
    ```

**Testing Windows Server AppFabric Cache with Sample Apps**

1. Grab a copy of the [Microsoft AppFabric Samples](http://go.microsoft.com/fwlink/?LinkID=167153), which are a series of *very* short examples.
2. Extract the samples locally.
3. Open up **CacheSampleWebApp.sln** (..SamplesCacheCacheSampleWebApp).
4. Right-click the **CreateOrder.aspx** file and select **Set As Start Page**.
5. Hit **F5** to start the solution.
6. Confirm that the cache is functioning by creating a sample order, getting the sample order, and updating the sample order.

Once you accomplish these three steps, you’ll have the basis for building more complex caching solutions.

I hope this helps!
