---
author: admin
categories:
- .NET 2.0
- Windows Mobile
comments: true
date: "2007-07-11T08:58:36Z"
slug: configuring-and-testing-a-windows-mobile-50-development-environment
title: Configuring and Testing a Windows Mobile 5.0 Development Environment
wordpress_id: 60
---

So, I've finally joined the 21st century, and purchased a [Samsung Blackjack](http://www.samsungblackjack.com/). After waiting a two _agonizing_ days, it finally arrived. Man, it's a beauty!




![Samsung Blackjack](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/WindowsMobile5.0Development_11CC7/samsungblackjack_1.gif)




I'll spare you a review of all it's features (this is covered in [agonizing detail elsewhere](http://www.google.com/search?hl=en&rls=com.microsoft%3Aen-us%3AIE-SearchBox&rlz=1I7GGIG&q=samsung+blackjack+reviews)), but there are a couple tidbits I'd like to share.




The Blackjack runs Windows Mobile 5.0 with the Messaging and Security Feature Pack. One of the neatest features that this supports is Exchange ActiveSync w/ Direct Push Technology. Fortunately, the company I work for has Exchange 2007 setup to push emails directly to smart phones, and this was the first thing I setup. Worked like a charm! Very easy.




> 

> 
> _Note: I know that Windows Mobile 6.0 is out. If you have an application that runs Windows Mobile 6.0, then good for you! Unfortunately, the Blackjack is still on 5.0, which is why this post is geared towards 5.0._




The second thing I looked into was how to setup a development environment that allows me to develop applications that I can deploy and run on my Blackjack. Turns out, it's pretty simple.




My searches lead me to the [Windows Mobile 5.0 Developer Resource Kit](http://www.microsoft.com/downloads/details.aspx?FamilyID=3baa5b7d-04c1-4ec2-83dc-61b21ec5fe57&DisplayLang=en). This kit contains (almost) everything you need to develop Windows Mobile 5.0 applications for a SmartPhone and Pocket PC. I suggest you review the details surrounding the resource kit, and then download it and try it yourself.




As I said, my goal here is to setup a development environment and deploy a _simple_ application to my Blackjack. In order to consider this a successful test, I don't need the application to actually do anything except run. Here are the steps I took.


> _Note: while this is written specifically for the Blackjack, the following steps will (largely) work for any kind of SmartPhone running Windows Mobile 5.0._




  1. Install all the prerequisites. I already had all the required prerequisites installed (e.g. Visual Studio 2005 and ActiveSync). I haven't yet tried any of the optional installations.

  2. Download the [Windows Mobile 5.0 Developer Resource Kit](http://www.microsoft.com/downloads/details.aspx?FamilyID=3baa5b7d-04c1-4ec2-83dc-61b21ec5fe57&DisplayLang=en). This kit has everything you need to get going.

  3. Run the MSI and install the kit.

  4. Once the kit is installed, run the Windows Mobile 5.0 Developer Resource Kit application. You can run this by clicking **Start** --> **All Programs** --> **Windows Mobile 5.0 Developer Resource Kit** --> **Windows Mobile 5.0 Developer Resource Kit**.

  5. Click **Install the Developer Tools** --> **Install the Tools**, and then in the detail pane slide down and click **Windows Mobile 5.0 SDK for Smartphone** (or Pocket PC, if you prefer).  
  
![Windows Mobile 5.0 Developer Resource Kit](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/WindowsMobile5.0Development_11CC7/image_3.png)  


  6. This opens up File Explorer in the folder C:Program FilesWindows Mobile 5.0 Developer Resource KitcontentDeveloper ToolsWindows Mobile 5.0 SDKs. From here, click Windows Mobile 5.0 SDK for Smartphone.msi or Windows Mobile 5.0 SDK for Pocket PC.msi.

  7. Once the installation completes, Visual Studio will have been updated to include a new project type called "Windows Mobile 5.0 Smartphone" under "Visual Basic" and "Visual C#". Open Visual Studio 2005, and click **File** --> **New** --> **Project**.  
  
![Windows Mobile 5.0 Smartphone project](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/WindowsMobile5.0Development_11CC7/image_4.png)  


  8. Select "Device Application" and click **OK**. Yes, I know the name and location are poor. Change them if you want; remember, this is just a quick test/demo.

  9. The project template includes a Form1.cs file. The designer shows a form embedded in a phone shell. Drop a label on the form that says something (e.g. "Hello world!").  
  
![image](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/WindowsMobile5.0Development_11CC7/image_thumb_3.png) 


  10. Make sure your Blackjack is connected to your computer via your USB cable, and confirm that ActiveSync is able to communicate and synchronize with your Blackjack.

  11. Samsung and Microsoft lock the Blackjack phone so that applications from unsigned publishers (like us!) will not run or deploy to the Blackjack phone. In order to resolve this, take a look at these two discussions: [Application Unlock Your Blackjack](http://blackjacksmartzone.spreebb.com/index.php?showtopic=13) and [Samsung Blackjack tips and tricks](http://blogs.vertigosoftware.com/jatwood/archive/2007/01/12/Samsung_Blackjack_tips_and_tricks.aspx) (the latter has some great tips). I chose the method in the first link, and ran AppUnlock.cab. Worked great.

  12. Go to **Build** --> **Deploy DeviceApplication1**. A dialog box asks you where to deploy your application. Choose **Windows Mobile 5.0 Smartphone Device**. Click Deploy.  
  
![Deploy Smartphone device](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/WindowsMobile5.0Development_11CC7/image_8.png)   


  13. The initial deployment will push a lot of CAB files used to run your .NET application. Make sure you approve all these cabs from your Blackjack. You will only have to do this the first time.

  14. Since our application was very simple, we'll have to browse to the executable through the file explorer. Click **Start** --> **Applications** --> **File Explorer**. Browse to Program FilesDeviceApplication1, and click **DeviceApplication1**.

  15. As we have not signed the application, you will have to confirm that you want to run the application (I'll write a post some other time explaining how to sign the application). Click **Yes** to continue.

  16. Lo and behold, our application runs and displays "Hello World!".



Yes, a completely useless and standard example, but it does serve to highlight how easy it is to setup a development environment that can publish applications to your SmartPhone (e.g. your Blackjack).




Now all I need to do is figure out a useful tool to build ...




I hope this helps!
