---
title: "Using Web Deploy with Windows Azure for Rapid Development"
date: 2010-12-17T00:00:00-08:00
draft: false
description: "From the whiteboard to the keyboard"
---

***Update:** Ryan Dunn has just posted [Using Web Deploy with Windows Azure](http://dunnry.com/blog/2010/12/20/UsingWebDeployWithWindowsAzure.aspx) that shows a way to use plugins to simplify the process of using Web Deploy with Windows Azure. He also provides a lot of nice commentary and context around the solution. Definitely worth reading.*

While building your web application, have you ever deployed to Windows Azure and then realized you forgot to make a change? Or forgot to include an update? Or maybe you deployed and realized you made a simple mistake, and you want to quickly update it? We all have. You’ll then find yourself making the change, creating a new package, and uploading/deploying it. Then you wait.

Now, in the grand scheme of things, waiting 10 minutes is ***not*** a big deal – think about everything you’re getting that you don’t already have at your disposal. That said, during development and QA, it can be frustrating to have to wait while your role instance upgrades or restarts.

Fortunately, with the updates provided in the Windows Azure SDK 1.3, we can benefit from an existing technology called Web Deploy to make our lives ***much*** easier.

A few caveats first:

1. This technique should ***only*** be used for development purposes.
2. You can only update a single role instance with this technique.
3. Since you are not updating the Windows Azure package you may lose your changes at anytime.

Be sure and understand the above caveats. This is ***only*** for development purposes.

Okay, ready to get started? Here are the steps to follow.

1. Create a new **Windows Azure Project** called **WindowsAzureWebDeploy**. Add an ASP\*\*.NET MVC 2 Web Role\*\* called \*\*MvcWebRole\*\* to the solution. It’s up to you if you want a unit test project.
2. Created a folder called **Startup** in your **MvcWebRole** project.
3. Create three files in this folder: **CreateUser.cmd**, **EnableWebAdmin.cmd**, and **InstallWebDeploy.cmd**. For each of these files, change the **Copy to Output Directory** value to **Copy always** and the **Build Action** value to **Content**.
4. Within the **Startup** folder, create a folder called **webpicmd**.
5. Download **WebPICmdLine** [here](http://go.microsoft.com/?linkid=9752821). For more information, see the MSDN article [Using the WebPICmd Command-Line Tool](http://msdn.microsoft.com/en-us/library/gg433092.aspx).
6. Unzip the file **webpicmdline\_ctp.zip** into the **webpicmd** folder.
7. In Visual Studio, add the following four files into the solution. For each of these files, change the **Copy to Output Directory** value to **Copy always**.

- Microsoft.Web.Deployment.dll
- Microsoft.Web.PlatformInstaller.dll
- Microsoft.Web.PlatformInstaller.UI.dll
- WebPICmdLine.exe

8. Update \*\*CreateUser.cmd\*\* to include the following code. Be sure to change “webdeployuser” and “password” to different values:

```
    net user webdeployuser password /add
    net localgroup administrators webdeployuser /add
    exit /b 0
```

9. Update **InstallWebDeploy.cmd** to include the following code:

   ```
    "%~dp0\webpicmd\WebPICmdLine.exe" /Products: WDeploy /xml:https://www.microsoft.com/web/webpi/2.0/RTM/WebProductList.xml /log:webdeploy.txt
    net stop wmsvc
    net start wmsvc
   ```
10. Update **EnableWebAdmin.cmd** to include the following code:

    Code: EnableWebAdmin.cmd

    ```
    start /w ocsetup IIS-ManagementService
    reg add HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WebManagement\Server /v EnableRemoteManagement /t REG_DWORD /d 1 /f
    net start wmsvc
    sc config WMSVC start= auto
    exit /b 0
    ```
11. Open the **ServiceDefinition.csdef** file in the **WindowsAzureWebDeploy** project.
12. Add a new **InputEndpoint** named **mgmtsvc** with the following values:

    ```
    <Endpoints>
      <InputEndpoint name="Endpoint1" protocol="http" port="80" />
      <InputEndpoint name="mgmtsvc" protocol="tcp" port="8172" localPort="8172" />
    </Endpoints>
    ```
13. Create three **Startup Tasks** to call the command files in the **MvcWebRole**.

    ```
    <Startup>
      <Task commandLine="Startup\EnableWebAdmin.cmd" executionContext="elevated" taskType="simple" />
      <Task commandLine="Startup\InstallWebDeploy.cmd" executionContext="elevated" taskType="simple" />
      <Task commandLine="Startup\CreateUser.cmd" executionContext="elevated" taskType="simple" />
    </Startup>
    ```
14. That’s it!

***Update (12/21/2010):** I have made a few updates above to the scripts based on feedback and testing.*

At this point, you are ready to deploy your application. When the role instance starts-up, the three start-up tasks will run and 1) create an user, 2) install web deploy through the Web Platform installer, and 3) enable web administration. This process can take several minutes to complete.

Once deployed, your application should look like this:

![Pre-Web Deploy](https://wadewegner.blob.core.windows.net/wordpress/2010/12/image10.png)

Now it’s time to setup your **MvcWebRole** project to publish directly to your role instance through web deploy. First, though, make a quick change to your application so that, after you deploy, you can verify that the new bits are deployed.

Update **Views –> Home –> Index.aspx** and replace the existing header with:

Code: Updated Index.aspx

```
<h2>Deployed with Web Deploy</h2>
```

Now, let’s publish the update.

1. Right-click **MvcWebRole** and click **Publish…**.
2. Create a profile name (e.g. **MvcWebRole**).
3. Add your full DNS name as the **Service URL** (e.g. **mywebdeploy.cloudapp.net**).
4. Add the **Site/application** value. This is essentially \_**yourrolename**\_\_IN\_0\_Web (e.g. **MvcWebRole\_IN\_0\_Web**). Don’t forget the “\_Web” at the end.
5. Check the **Mark the IIS application on destination** checkbox.
6. Remove the check from the **Leave extra files on the destination (do not delete)** checkbox.
7. Check the **Allow untrusted certificate** checkbox.
8. Add your **User name** (e.g. “webdeployuser”).
9. Add your **Password**.
10. Check the **Save password** checkbox.
11. Click **Publish**.

Once complete, your **Publish Web** window should look something like this:

![image](https://wadewegner.blob.core.windows.net/wordpress/2010/12/image11.png)

After you get the message **Publish succeeded**, refresh your page. It should now look like this:

![DeployedWithWebDeploy](https://wadewegner.blob.core.windows.net/wordpress/2010/12/DeployedWithWebDeploy.png)

And there you have it! Within seconds you’ve deployed updates to your application running in Windows Azure.

Now, do you remember the caveats above? If nothing else, remember that this is ***only*** for development purposes. I don’t want to hear that you used this in production then lost all your changes when a role instance was restarted.

I hope this helps!
