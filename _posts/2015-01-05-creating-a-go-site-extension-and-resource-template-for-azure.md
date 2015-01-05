---
layout: post
title: "Creating a Go Site Extention and Resource Template for Azure"
description: "Site extensions provide sophisticated extensibility for Azure Websites while the Azure Resource Manager automates the deployment and configuration of Azure resources. Combine them together, and you can automate many different website tasks, even the configuration of the Go runtime in Azure Websites."
categories: 
- golang
- azure
- azure websites
- azure resource manager
- resource template
- site extension
---

The New Year is a good time to set goals for yourself. I've challenged myself this year to explore the Go language and run some shipping apps using Go, which is likely no surprise given my recent posts on [setting up Go on Windows](http://www.wadewegner.com/2014/12/easy-go-programming-setup-for-windows/) and [running Go on Azure Websites](http://www.wadewegner.com/2014/12/4-simple-steps-to-run-go-language-in-azure-websites/). While most of my work with Go has been playing and figuring out what I know and, more importantly, don't know, I've also started some delibrate training and coding.

As part of playing around, I used Azure Websites to host my Go web apps. The last post I wrote showed my first [Go prototype on Azure Websites](http://www.wadewegner.com/2014/12/4-simple-steps-to-run-go-language-in-azure-websites/). While useful as a prototype, it's arguably not the most straightforward way to go about building your Go web app on Azure Websites. The principal issue is that the Go runtime is not available by default on Azure Websites; we have to deploy and configure the website to use the Go binaries. While my first prototype works, Azure has to capabilities you've likely never heard of which can help: Site Extensions and the Azure Resource Manager.

So, what did I do? I created a [site extension for Go](https://www.siteextensions.net/packages/golang/) and wrote a [resource template](https://github.com/wadewegner/azure-go-lang-site-extension/blob/master/scripts/Template.json) and [PowerShell script](https://github.com/wadewegner/azure-go-lang-site-extension/blob/master/scripts/Deploy.ps1) for deploying an Azure Website that installs the site extention. You can find everything in my [azure-go-lang-site-extension](https://github.com/wadewegner/azure-go-lang-site-extension) respository on Github.

Let's see how it works.

### Create the Site Extension ###

Site Extensions are exactly what the name suggests and provides ways to extend your Azure Website. You create a NuGet package with your code and changes, package it up, deploy it to [https://www.siteextensions.net/](https://www.siteextensions.net/) (actually, any NuGet feed), and later you can apply these packages to your Azure Website. For additional details please see the [Azure Web Site Extensions](http://azure.microsoft.com/blog/2014/06/20/azure-web-sites-extensions/) blog post and the [Azure Site Extensions wiki](https://github.com/projectkudu/kudu/wiki/Azure-Site-Extensions).

I used two important capabilities to get Go setup in the Azure Website: install/uninstall scripts and XML Document Transformation (XDT).

{% highlight bat %}

IF NOT EXIST "%WEBROOT_PATH%server.go" (
	cp server.go %WEBROOT_PATH%
)

IF NOT EXIST "%WEBROOT_PATH%go1.4.windows-386.zip" (
	cd /D %WEBROOT_PATH%
	curl -O https://storage.googleapis.com/golang/go1.4.windows-386.zip
	start unzip -o go1.4.windows-386.zip -d .
)

{% endhighlight %}

First, I wrote an install script called `install.cmd` which performs the following tasks:

1. Copies the `server.go` file into the webroot. This is the file which will serve the HTTP responses.

2. Downloads the Go SDK using curl.

3. Unzips the Go SDK.

Unzipping the Go SDK can take several minutes. Today, this causes issues when trying to install the site extension from the Azure Portal and the Azure Resource Manager, as it will timeout, throw an error, and continue to retry until it ultimately fails. Consequently, I use the command `start` to run the unzip operation asynchronously, allowing the `install.cmd` execution to return and not causing a timeout error. While this solves the problem it is not ideal for if the instance is restarted during the process of unzipping the SDK it could leave us in a faulted state. This is likely a short term issue and I'll update this post when it is resolved.

{% highlight xml %}

<?xml version="1.0" encoding="UTF-8"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform"> 
  <location path="%XDT_SITENAME%" xdt:Locator="Match(path)">
    <system.webServer>
      <handlers xdt:Transform="InsertIfMissing">
        <add name="httpplatformhandler" path="*" verb="*" modules="httpPlatformHandler" resourceType="Unspecified" xdt:Transform="InsertIfMissing" />
      </handlers>
      <httpPlatform processPath="d:\home\site\wwwroot\go\bin\go.exe"
                    arguments="run d:\home\site\wwwroot\server.go"
                    startupTimeLimit="60" xdt:Transform="InsertIfMissing">
        <environmentVariables>
          <environmentVariable name="GOROOT" value="d:\home\site\wwwroot\go" xdt:Transform="InsertIfMissing"/>
        </environmentVariables>
      </httpPlatform>
    </system.webServer>
  </location>
</configuration>

{% endhighlight %}

Next, I write an XML document transform for the `applicationHost.Config` file to tell IIS to use our Go binaries for responding to web requests. After the site extension is installed and the first web request is processed, the applicationHost.config file is updated to include the additional configuration details. It's important to point out the `<location> ... <location>` element, as this ensures our changes only apply to the correct website; otherwise, anything hosted on this machine is affected.

If you have questions on this configuration information, see my earlier post on [setting up Go on Azure websites](http://www.wadewegner.com/2014/12/4-simple-steps-to-run-go-language-in-azure-websites/).

Packaging all of this into a site extension is pretty easy as it's based on NuGet. I first created a [Nuspec file](https://github.com/wadewegner/azure-go-lang-site-extension/blob/master/GoLang.nuspec) to describe the package and then a [build script](https://github.com/wadewegner/azure-go-lang-site-extension/blob/master/build.cmd) which uses NuGet to create the package. The result is a NuPkg which I then upload to [https://www.siteextensions.net/](https://www.siteextensions.net/).

At this point you can log into the [Azure Portal](http://portal.azure.com/), create an Azure Website (or use an existing on), and install the Site Extension. On your website blade and scroll down until you see the `Extensions` tile.

![zeroextensions](https://cloud.githubusercontent.com/assets/746259/5613696/8d0ec790-94a5-11e4-9907-4f505b18bcf9.png)

Through the extensions blade you can browse available site extensions. You'll find `Go Lang for Azure Websites`. Install it, and you'll see it listed as one of your site extensions.

![installedextension](https://cloud.githubusercontent.com/assets/746259/5613717/e0e28e06-94a5-11e4-9b25-72d1eb5579f5.png)

Wait a few minutes and you now have a Go web app running in Azure Websites. Not too bad! Of course, we can make the last step of creating the Azure Website and installing the Site Extension easier with the Azure Resource Manager.

### Create the Resource Template ###

The Azure Resource Manager is a relatively new technology that provides a container for managing the lifecycle Azure resources (called a [resource group](http://azure.microsoft.com/en-us/documentation/articles/azure-preview-portal-using-resource-groups/)), a way to describe and execute the deployment and configuration of a set of Azure resources, and a management layer for these resources. As the resource manager is still in preview, documentation is harder to comeby. To learn more about the resource manager, I recommend you watch [Introduction to Azure Resource Manager](http://channel9.msdn.com/Events/Build/2014/2-607) from Build 2014 and read the [REST API Reference](http://msdn.microsoft.com/en-us/library/azure/dn790568.aspx) and [Template Language](http://msdn.microsoft.com/en-us/library/azure/dn835138.aspx) guides.

The resource template is nothing more than a JSON file that describes the Azure resources we want to deploy; in this case, it's an Azure Website along with our Go site extension. Let's look at the template.

{% highlight json %}

{
  "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "siteName": {
      "type": "string"
    },
    "hostingPlanName": {
      "type": "string"
    },
    "siteLocation": {
      "type": "string"
    },
    "sku": {
      "type": "string",
      "allowedValues": [
        "Free",
        "Shared",
        "Basic",
        "Standard"
      ],
      "defaultValue": "Free"
    },
    "workerSize": {
      "type": "string",
      "allowedValues": [
        "0",
        "1",
        "2"
      ],
      "defaultValue": "0"
    }
  },
  "variables": {
    "goSite": "[parameters('siteName')]"
  },
  "resources": [
    {
      "apiVersion": "2014-04-01",
      "name": "[parameters('hostingPlanName')]",
      "type": "Microsoft.Web/serverfarms",
      "location": "[parameters('siteLocation')]",
      "properties": {
        "name": "[parameters('hostingPlanName')]",
        "sku": "[parameters('sku')]",
        "workerSize": "0",
        "numberOfWorkers": 1
      }
    },
    {
      "apiVersion": "2014-04-01",
      "name": "[variables('goSite')]",
      "type": "Microsoft.Web/sites",
      "location": "[parameters('siteLocation')]",
      "dependsOn": [
        "[resourceId('Microsoft.Web/serverfarms', parameters('hostingPlanName'))]",
      ],
      "properties": {
        "name": "[variables('goSite')]",
        "webHostingPlan": "[parameters('hostingPlanName')]"
      },
      "resources": [
        {
          "apiVersion": "2014-04-01",
          "name": "web",
          "type": "config",
          "dependsOn": [
            "[resourceId('Microsoft.Web/Sites', variables('goSite'))]"
          ],
          "properties": {
            "appSettings": [
              { "Name": "SCM_SITEEXTENSIONS_FEED_URL", "Value": "http://www.siteextensions.net/api/v2/" },
            ]
          }
        },
        {
          "apiVersion": "2014-04-01",
          "name": "GoLang",
          "type": "siteextensions",
          "dependsOn": [
            "[resourceId('Microsoft.Web/Sites', variables('goSite'))]",
            "[resourceId('Microsoft.Web/Sites/config', variables('goSite'), 'web')]"
            ],
          "properties": { }
        },
      ]
    }
  ]
}

{% endhighlight %}

I'm not going to attempt to explain this entire template as that's well beyond the scope of this post. Take a look at this [guide on the template language](http://msdn.microsoft.com/en-us/library/azure/dn835138.aspx) if you want to learn more.

For our purposes, I want to highlight the last section:

{% highlight json %}

...

        {
          "apiVersion": "2014-04-01",
          "name": "GoLang",
          "type": "siteextensions",
          "dependsOn": [
            "[resourceId('Microsoft.Web/Sites', variables('goSite'))]",
            "[resourceId('Microsoft.Web/Sites/config', variables('goSite'), 'web')]"
            ],
          "properties": { }
        },

...

{% endhighlight %}

Essentially, here we are telling the resource manager that we want the `GoLang` site extension installed into our Azure Website. Simple but effective.

To execute this template, we use PowerShell. I wrote a script which makes this pretty simple.

{% highlight powershell %}

Switch-AzureMode -Name AzureResourceManager

$subName = "YOURSUBSCRIPTIONNAME"
$userName = "YOURORGIDUSERNAME"
$securePassword = ConvertTo-SecureString -String "YOURPASSWORD" -AsPlainText -Force
$name = "YOURRESOURCEGROUPNAME"
$location = "West US"
$templateFile = "Template.json"
$siteName = $name + "Site"
$hostingPlanName = $name

$cred = New-Object System.Management.Automation.PSCredential($userName, $securePassword)
Add-AzureAccount -Credential $cred 

Select-AzureSubscription -SubscriptionName $subName
New-AzureResourceGroup -Name $name -Location $location -TemplateFile $templateFile -siteName $siteName -hostingPlanName $hostingPlanName -siteLocation $location -Force
 
Switch-AzureMode -Name AzureServiceManagement

{% endhighlight %}

When you run this command it will create a resource group and run resource template, resulting in a new Azure Website with our site extension installed.

![psoutput](https://cloud.githubusercontent.com/assets/746259/5613872/dc0c8f7e-94a7-11e4-8313-a00b174f3daf.png)

The parameters `siteName`, `hostingPlanName`, and `siteLocation` are all defined within the resource template. For documentation on the `New-AzureResourceGroup` syntax you can review the [MSDN documentation](http://msdn.microsoft.com/en-us/library/dn654594.aspx).

And that's it, my friends. The end result is an Azure Website you can use to start playing around with Go.

There's a lot of stuff to think about in this blog post. While my goal here was to make it easy to setup Go inside Azure Websites, I hope you see this as an example of how you can take advantage of Site Extensions and the Azure Resource Manager to do some amazing things in Azure.

I hope this helps!