---
title: "Getting ready for the ‘Race to Market Challenge’"
date: 2009-09-03T00:00:00-08:00
draft: false
description: "From the whiteboard to the keyboard"
---

I am really excited about [The Race to Market Challenge](http://www.mobilethisdeveloper.com/), sponsored by the Windows Mobile team.  Nothing motivates me like a challenge that promises great prizes and glory - especially when the prize is a [Microsoft Surface](http://www.microsoft.com/surface/)!

![image](https://wadewegner.blob.core.windows.net/wordpress/2009/09/image10.png)

If that’s not enough to get you pumped up about developing a killer application, what will?

I’ve decided to build a mobile application myself (two, actually), and consequently needed to make sure that I had my development environment setup appropriately.  There are quite a few steps required in order to prepare your machine for Windows Mobile development, but it’s not too bad.

> I am running Windows 7 RTM 64 bit with Visual Studio 2008 Team Suite.  Your mileage may vary if you’re running a different O/S or version of Visual Studio.  That said, I strongly recommend you upgrade to [Windows 7](http://www.microsoft.com/windows/windows-7/default.aspx) and insist on [Team Suite](http://www.microsoft.com/visualstudio/en-us/products/teamsystem/default.mspx) - there are no substitutes for good tools.

Here are the steps I took in order to setup my machine:

1. Download and install [Microsoft Windows Mobile Device Center 6.1 Driver for Windows Vista (64-bit)](http://www.microsoft.com/downloads/details.aspx?familyid=4F68EB56-7825-43B2-AC89-2030ED98ED95&displaylang=en).  At the moment it doesn’t appear as if there’s a specific version for Windows 7.  This Windows Vista version seems to work just fine.
2. Download the Windows Mobile 6 SDKs and 6.5 tool kit.  Be sure and note the different terminology for the different phones - evidently there are distinctions between Windows Mobile Standard (previously Windows Mobile for Smartphone) and Windows Mobile Professional (previously Windows Mobile for Pocket PC).�

```
* Install [Windows Mobile 6 Professional and Standard Software Development Kits Refresh](http://www.microsoft.com/downloads/details.aspx?FamilyID=06111a3a-a651-4745-88ef-3d48091a390b&displaylang=en).

* Install [Windows Mobile 6.5 Developer Tool Kit](http://www.microsoft.com/downloads/details.aspx?displaylang=en&FamilyID=20686a1d-97a8-4f80-bc6a-ae010e085a6e).
```

3. I also wanted to install the Mobile Applications Blocks from patterns & practices.

```
* First install [SQL Server Compact 3.5 SP1 for Windows Mobile](http://www.microsoft.com/downloads/details.aspx?FamilyID=FCE9ABBF-F807-45D6-A457-AB5615001C8F&displaylang=en).  This is required for using the Data Access Block, Disconnected Agent, and Subscription Block.

* Install the [Mobile Application Blocks](http://mobile.codeplex.com/) through the [Community Drop Installer (05-21-2009)](http://mobile.codeplex.com/Release/ProjectReleases.aspx?ReleaseId=27708).
```

4. I am also a big fan of reference applications, and using them to evaluate best practices when developing on a new platform.  Consequently, I wanted to get the following two solution accelerators for Windows Mobile.

```
* Install [Microsoft Windows Mobile 6 Consumer Solution Accelerator](http://www.microsoft.com/downloads/details.aspx?FamilyID=ACB764DF-EF79-4B96-A47F-6C7D916473E5&displaylang=en).  This is a showcase mobile consumer application that assists a user traveling to a meeting.  Definitely worth reviewing to harness best practices.

* Install [Windows Mobile Line of Business Solution Accelerator 2008](http://www.microsoft.com/downloads/details.aspx?FamilyId=428E4C3D-64AD-4A3D-85D2-E711ABC87F04&displaylang=en).  This is a supply chain application that uses the .NET Compact Framework 3.5, SQL Server Compact 3.5, and has a great deal of useful documentation.
```

A lengthy process, but not too difficult.

Once I completed the setup, I wanted to confirm that I could actually build and emulate an application.  I decided to do this very quickly by creating a “Hello Windows Mobile!” application and deploy to one of the Windows Mobile 6.5 emulators.

Here are the steps I took:

1. You need to start a device emulator so that you have something to which you can deploy the application.  Run the Device Emulator Manager by selecting **Start -> All Programs -> Windows Mobile 6 SDK -> Tools -> Device Emulator Manager**.

```
* I Create a new smart device project.  This will create a new project and give you a blank windows mobile form.

* Open up Visual Studio, and click **File -> New -> Project**.

* Select **Visual C# -> Smart Device -> Smart Device Project**.  Name it whatever you like.

* From the New Smart Device Project screen select "Windows Mobile 6 Professional SDK" for your target platform and choose "Device Application".
```

2. From the Toolbox, drag a Label control onto the form and change the text to “Hello Windows Mobile!”
3. Debug the project by clicking F5 (or **Debug -> Start Debugging**).

```
* You will be prompted to choose an emulator.  I chose the USA Windows Mobile 6.5 Professional VGA Emulator under **Datastore -> Windows Mobile 6 Professional SDK**.

* Here's what it looks like - very cool:
```

![image](https://wadewegner.blob.core.windows.net/wordpress/2009/09/image11.png)

```
* After a few moments, your applications will get deployed to the device.  It may take a few minutes the first time as all the necessary pieces are deployed.  Once it has finished, you should see your application deployed.  It will look like this:
```

![image](https://wadewegner.blob.core.windows.net/wordpress/2009/09/image12.png)

4. When you close the emulator, go ahead and have it save the state.  This will speed things up when you go to deploy the next time.

In the end, it’s really exciting to have a fully functional development environment that can target all the Windows Mobile 6.X platforms.  Lots of potential for building applications that have a huge install base!

I really hope that you found this useful.  I strongly encourage you to sign up for the [Race to Market Challenge](http://www.mobilethisdeveloper.com/) and start building some great applications.
