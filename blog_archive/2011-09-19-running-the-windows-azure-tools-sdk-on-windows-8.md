---
author: admin
categories:
- Tips
- Tools
- Troubleshooting
- Windows 8
- Windows Azure
- Windows Azure Platform
comments: true
date: "2011-09-19T06:52:53Z"
slug: running-the-windows-azure-tools-sdk-on-windows-8
title: Running the Windows Azure Tools &amp; SDK on the Windows Developer Preview
wordpress_id: 1538
---

Now that we can get our hands on the Windows Developer Preview of Windows 8, I’m sure everyone is excited to start building applications! For many of us, this means that we’ll want to install all of the great Windows Azure tools on our Windows Developer Preview machine – especially if we plan to take advantage of the new [Windows Azure Toolkit for Windows 8](http://watwindows8.codeplex.com/). However, there are a few things we’ll need to do in order to get things to work.

 

Quickly let me explain the three challenges you’ll run into:

 

  
  1. The current Windows Azure Tools for Visual Studio do not yet support Dev11 – you’ll have to install Visual Studio 2010. 
   
  2. There are a few quirks getting dependencies for IIS installed on Windows 8 using the Web Platform Installer. 
   
  3. After Visual Studio 2010 and the Windows Azure Tools for Visual Studio are installed on a machine with Dev11, the Windows Azure tools will grab an environmental path variable that points to 11.0 instead of 10.0. 
 

Fortunately, these things are pretty easy to resolve and will certainly get addressed in later builds.

 

Here’s what you’ll have to do to get the tools and SDK working with the Windows Developer Preview:

 

  
  1. Install **Microsoft .NET Framework 3.5.1**. Easiest way is to type “Windows Features”, select **Settings**, and select **Turn Windows features on or off**. Then simply check the checkbox. Click **OK** to install.         
![Step 1 - .NET 3.5.1](https://wadewegner.blob.core.windows.net/wordpress/2011/09/Step-1-.NET-3.5.1.png)
   
  2. Next we need to correctly configure IIS. Typically we’d do this through the Web Platform Installer, but this doesn’t work correctly on Windows 8. From **Turn Windows features on or off** you’ll need to do the following, then click **OK** to install.              
    * Check **Internet Information Services**. 
       
    * Expand **Internet Information Services** and **World Wide Web Services**. 
       
    * Expand **Application Development Features** and check **ASP.NET 2.0**, **ASP.NET 4.5**, and **CGI**. 
       
    * Expand **Common HTTP Features** and check **HTTP Redirection**. 
       
    * Expand **Health and Diagnostics** and check **Logging Tools**, **Request Monitor**, and **Tracing**. 
       
   
  3. Install the **[Web Platform Installer](http://www.microsoft.com/web/downloads/platform.aspx) (WebPI)**. 
   
  4. Install **Visual Web Developer 2010 Express** through **WebPI**. (You can use a different version of Visual Studio 2010, so long as it’s supported by the Windows Azure Tools for Visual Studio.) 
   
  5. Install **Windows Azure Tools for Microsoft Visual Studio 2010 – September 2011** through **WebPI**. 
 

Okay, now you have everything installed. However, before you trying running Visual Studio, you have to create a script to launch Visual Studio 2010. This is because the Windows Azure tools use an environmental variable that’s incorrectly pointing to version 11.0, and we’ll need to change it right before we launch to version 10.0 (for Visual Studio 2010).

 
    
    @ECHO OFF
    
    SET VisualStudioVersion=10.0
    
    SET VisualStudioPath="%ProgramFiles(x86)%\Microsoft Visual Studio 10.0\Common7\IDE"
    IF NOT EXIST %WINDIR%\SysWow64 SET VisualStudioPath="%ProgramFiles%\Microsoft Visual Studio 10.0\Common7\IDE"
    
    CD /D %VisualStudioPath%
    
    VWDExpress.exe





As you can see, the script changes the VisualStudioVersion to 10.0 then launches Visual Web Developer Express (there’s a little extra code to set the path correctly regardless of 32- or 64-bit versions of Windows).





Save the above script as a CMD file (i.e. OpenVisualStudio2010.cmd) and then make sure to right-click and **Run as administrator**! If you forget to run the script as administrator then Visual Studio won’t have the permissions needed to run the Windows Azure tools correctly.


![Running Instance](https://wadewegner.blob.core.windows.net/wordpress/2011/09/Running-Instance_thumb.png)
Look, it’s working!


**NOTE**: I found myself having to reboot Windows 8 in order to resolve a problem where the debugger wouldn’t attach. Not sure if this occurs every time, but in case you have a similar issue just try rebooting.





I hope this helps!
