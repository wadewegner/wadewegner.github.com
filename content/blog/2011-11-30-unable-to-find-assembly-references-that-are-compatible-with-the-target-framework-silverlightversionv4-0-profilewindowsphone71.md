---
author: admin
categories:
- NuGet
comments: true
date: "2011-11-30T18:18:18Z"
slug: unable-to-find-assembly-references-that-are-compatible-with-the-target-framework-silverlightversionv4-0-profilewindowsphone71
title: Unable to find assembly references that are compatible with the target framework
  'Silverlight,Version=v4.0, Profile=WindowsPhone71'
wordpress_id: 1730
---

Yes, that title is a mouthful. But this title is as cryptic as the error that can get generated when trying to install a NuGet package:
   
    Install-Package : Unable to find assembly references that are compatible with the target
    framework 'Silverlight,Version=v4.0,Profile=WindowsPhone71'.

    At line:1 char:16
    + Install-Package <<<<  Phone.Storage
        + CategoryInfo          : NotSpecified: (:) [Install-Package], InvalidOperationException
        + FullyQualifiedErrorId : NuGetCmdletUnhandledException,
          NuGet.PowerShell.Commands.InstallPackageCommand

Here’s what you’ll see in Visual Studio:

![NuGetErrorMessage](https://wadewegner.blob.core.windows.net/wordpress/2011/11/NuGetErrorMessage.jpg)

This is the error I get when trying to install one of our [NuGet packages](http://www.wadewegner.com/2011/11/nuget-packages-for-windows-azure-and-windows-phone-developers/) on a freshly built machine. Long story short, the reason is that I don’t have the latest NuGet Package Manager.

Take a look at Extension Manager and observe the version number associated with the NuGet Package Manager:

![OldNuGet](https://wadewegner.blob.core.windows.net/wordpress/2011/11/OldNuGet.jpg)

See that the version is 1.2? We need to update it to version 1.5. Head to [http://nuget.org](http://nuget.org) and from there click to install NuGet. Once you’ve completed the installation you should confirm that you now have version 1.5:

![NewNuGet](https://wadewegner.blob.core.windows.net/wordpress/2011/11/NewNuGet.jpg)

So now, when you try to install a NuGet package such as Phone.Storage you won’t have any problems.

![NuGetWorking](https://wadewegner.blob.core.windows.net/wordpress/2011/11/NuGetWorking.jpg)

I hope this helps!