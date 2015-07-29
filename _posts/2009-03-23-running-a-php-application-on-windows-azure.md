---
author: admin
comments: true
date: 2009-03-23 22:31:16+00:00
layout: post
slug: running-a-php-application-on-windows-azure
title: Running a PHP application on Windows Azure
wordpress_id: 650
categories:
- Azure
- PHP
- Windows Azure
---

I recently returned from [MIX09](http://live.visitmix.com/) – what a fantastic event! So many great things were announced; so many that there’s no way I can list them all here. Please visit [http://live.visitmix.com/](http://live.visitmix.com/) for access to the keynote videos and all the breakout sessions. I would also recommend reading the [day one recap](http://archive.visitmix.com/blogs/News/MIX08-Day-1-Keynote-Recap/) and the [day two recap](http://www.visitmix.com/Opinions/MIX09-Keynote-and-Partner-Highlights).

One of the major announcements was new capabilities for the Windows Azure CTP. This includes the support for FastCGI, Full Trust, and Geo-location. Please take a look at the [Windows Azure team’s blog for all the announcements](http://blogs.msdn.com/windowsazure/archive/2009/03/18/windows-azure-delivers-new-ctp-capabilities.aspx).

In this post, I want to walk you through the step you need to take to run a PHP application on Windows Azure. As you’ll see, it’s quite simple and straightforward – after a few configuration updates and additional steps, you can run nearly any PHP application. This walkthrough assumes that you have installed the latest bits for Windows Azure. If you are just getting started with Azure, please visit [http://www.azure.com/](http://www.azure.com/) – this is the primary landing page for the Azure Services Platform. If you want to get the updated bits that allows for Full Trust and FastCGI, you can grab the bits at the [Windows Azure Tools for Microsoft Visual Studio March 2009 CTP page](http://www.microsoft.com/downloads/details.aspx?familyid=59E8FC0C-C399-4AB7-8A93-882D8E74B67A&displaylang=en).



	
  1. Create a new project. Select **File** –> **New** –> **Project**.

	
  2. Select **Cloud Service** project type and select the **Blank Cloud Service** template. Specify the name/location, and click **OK**.
[![New Windows Azure project](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/RunningaPHPapplicationonWindowsAzure_E6D6/image_thumb_1.png)](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/RunningaPHPapplicationonWindowsAzure_E6D6/image_4.png)

	
  3. Add a **Cgi Web Role **project. Right-click the project (e.g. CloudService1) and select **New Web Role Project**.
[![Add a web role project](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/RunningaPHPapplicationonWindowsAzure_E6D6/image_thumb_2.png)](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/RunningaPHPapplicationonWindowsAzure_E6D6/image_6.png)

	
  4. Specify the **Name** and click **OK**.
[![Add a CGI web role](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/RunningaPHPapplicationonWindowsAzure_E6D6/image_thumb_3.png)](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/RunningaPHPapplicationonWindowsAzure_E6D6/image_8.png)

	
  5. By default the **Web.roleconfig** file is opened in the new project. Remove the comments and update the **application fullPath **to "%RoleRoot%phpphp-cgi.exe".
[![image](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/RunningaPHPapplicationonWindowsAzure_E6D6/image_thumb_4.png)](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/RunningaPHPapplicationonWindowsAzure_E6D6/image_10.png)

	
  6. Now we need to actually grab the PHP executable that will bet invoked. Browse to [http://www.php.net/downloads.php](http://www.php.net/downloads.php) and grab the non-thread-safe Windows binary (at the time of writing, it is “PHP 5.2.9-1 Non-thread-safe zip package”).

	
  7. You might as well configure PHP to run on your Windows environment. Rather than specifying all these steps, please see [Simon Guest’s post on hosting PHP applications in 5 easy steps](http://simonguest.com/blogs/smguest/archive/2009/03/09/Using-Windows-7-to-host-PHP-applications-in-5-easy-steps_2100_.aspx). Follow the steps in his post, and at the end you should be able to confirm that PHP works in IIS 7.

	
  8. Copy all the files in the PHP folder you downloaded into your Cgi Web Role project. XCopy works really well. For example, open a command prompt in the root of your Cgi Web Role project and type:
**xcopy /s php php**
When asked, specify “D for directory”. This will copy everything from c:php into a folder called php in your Cgi Web Role project. Note that this is where we pointed Windows Azure to in step 5.

	
  9. Select your Cgi Web Role project, show all files, and add the new php folder into your project. This way it will get packaged up with the rest of your application and deployed to Windows Azure.

	
  10. Open up the **Web.config** file and specify a new default document. This will tell Windows Azure to open up our PHP page by default. Under the <system.webServer> element, add a <defaultDocument>:
[![defaultDocument](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/RunningaPHPapplicationonWindowsAzure_E6D6/image_thumb_5.png)](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/RunningaPHPapplicationonWindowsAzure_E6D6/image_12.png)

	
  11. Additionally, we need to setup a handler in the Web.config file. Add the following code to the <handlers> element in the web.Config: [![FastCGI](http://www.wadewegner.com/content/2009/03/FastCGI.jpg)](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/RunningaPHPapplicationonWindowsAzure_E6D6/image_22.png)

	
  12. In order for this to function, we have to enable native code execution in Windows Azure. To do so, open the **ServiceDefinition.csdef** file and set the **enableNativeCodeExecutiion** flag to true:
[![enableNativeCodeExecution](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/RunningaPHPapplicationonWindowsAzure_E6D6/image_thumb_6.png)](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/RunningaPHPapplicationonWindowsAzure_E6D6/image_14.png)

	
  13. Now, let’s create a new PHP file. Right-click on your Cgi Web Role and add a new item. Select the Text File template and change the name to index.php. Click **Add**.
[![index.php](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/RunningaPHPapplicationonWindowsAzure_E6D6/image_thumb_7.png)](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/RunningaPHPapplicationonWindowsAzure_E6D6/image_16.png)

	
  14. To test, create some PHP code. A quick, but effective, test is to simply echo “Hello World”. In the index.php file, write the following code:
[![hello world](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/RunningaPHPapplicationonWindowsAzure_E6D6/image_thumb_8.png)](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/RunningaPHPapplicationonWindowsAzure_E6D6/image_18.png)

	
  15. Press F5, and your browser should open and show you the following page:
[![Hello World!](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/RunningaPHPapplicationonWindowsAzure_E6D6/image_thumb_9.png)](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/RunningaPHPapplicationonWindowsAzure_E6D6/image_20.png)


And that’s all it takes!

While this is a simple demonstration, I hope it impresses upon you the impact that allowing native code execution has in Windows Azure. With native code execution, and fast CGI support, you can now leverage all the rich content created in the PHP community on Windows Azure. The story even gets better with the new relational capabilities of SQL Data Services, but I’ll save that for another post.

I hope this helps!
