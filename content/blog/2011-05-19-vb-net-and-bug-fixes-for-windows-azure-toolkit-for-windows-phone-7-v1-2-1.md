---
author: admin
categories:
- Toolkit
- Windows Azure
- Windows Phone 7
comments: true
date: "2011-05-19T16:22:59Z"
slug: vb-net-and-bug-fixes-for-windows-azure-toolkit-for-windows-phone-7-v1-2-1
title: VB.NET and Bug Fixes for Windows Azure Toolkit for Windows Phone 7 (v1.2.1)
wordpress_id: 1237
---

Today’s release of the [Windows Azure Toolkit for Windows Phone 7](http://watoolkitwp7.codeplex.com/) (v1.2.1) has two important updates:

 

  
  * Support for Visual Basic 
   
  * Bug fixes 
 

You can download the latest drop here: [http://watoolkitwp7.codeplex.com/releases/view/61952](http://watoolkitwp7.codeplex.com/releases/view/61952)

 

**Visual Basic**

 

When we released on Monday we did not include updated project templates for Visual Basic – turns out that creating these project templates takes a long time, and we did not have enough cycles to complete it in time. However, we’ve had a few additional dates to finish this work, and we’re now providing Visual Basic support – this means can use this toolkit with Visual Basic and benefit from all the [updates provided as part of the 1.2 release](http://www.wadewegner.com/2011/05/now-available-windows-azure-toolkit-for-windows-phone-7-v1-2/).

 

![VBNET](https://wadewegner.blob.core.windows.net/wordpress/2011/05/VBNET1.png)

 

****

 

**Bug Fixes**

 

We also fixed a few bugs in this release. Many thanks to those of you who helped by reporting them so quickly!

 

  
  * **[Fixed]** The VS on-screen documentation for "Windows Phone 7 Empty Cloud App" template is missing.         
![clip_image002](https://wadewegner.blob.core.windows.net/wordpress/2011/05/clip_image0021.gif)
   
  * **[Fixed]** Modify VSIX installation scripts to avoid errors when users have PowerShell Profile Scripts.         
![clip_image002[5]](https://wadewegner.blob.core.windows.net/wordpress/2011/05/clip_image00251.gif)
   
  * **[Fixed]** The **CopyLocal** property of the **Microsoft.IdentityModel** assembly is set to **false** in the sample and project template solutions. It has now been set to **true**. 
   
  * When creating a new project using the project template wizard:             
    * **[Fixed]** If the user sets an invalid ACS Namespace and Management Key, the application shows an non-descriptive error and generates an inconsistent solution. 
       
    * **[Fixed]** Using a real Azure Storage account and not selecting Use HTTPS option makes the generated solution fail when running it. 
       
    * **[Fixed]** An error is displayed when creating a new 'Windows Phone Cloud Application' project in Visual Web Developer 2010 Express. 
       
    * **[Fixed]** An error is displayed when creating a new 'Windows Phone Cloud Application' project in Visual Studio 2010 Express for Windows Phone. 
       
    * **[Fixed]** The value of the **PushServiceName** setting is not replaced in the code generated with the project templates. 
       
   
  * **[Fixed]** The ACS sample was shipped with an invalid configuration and wrong instructions in the Readme document to configure them appropriately. 
   
  * **[Fixed]** The **ApplePushNotification** class is not used in the samples nor by the code generated with the project templates. 
   
  * **[Fixed]** The solutions for VWD and VPD do not build since some projects are missing. 
   
  * **[Fixed]** The error message shown in the 'login' page when there are connectivity issues is non-descriptive. 
 

Again, many thanks to those of you who helped by reporting these bugs. Keep the feedback coming!
