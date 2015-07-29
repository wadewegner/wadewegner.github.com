---
author: admin
comments: true
date: 2011-11-18 22:32:00+00:00
layout: post
slug: adding-push-notification-support-to-your-windows-phone-application
title: Adding Push Notification Support to Your Windows Phone Application
wordpress_id: 1697
categories:
- Deep Linking
- NuGet
- Push Notification
- Windows Azure
- Windows Phone
---

A couple days ago I wrote a post on [outsourcing user authentication in a Windows Phone application](http://www.wadewegner.com/2011/11/outsourcing-user-authentication-in-a-windows-phone-application/), demonstrating how easy it is to leverage the Windows Azure Access Control service in your Windows Phone application. The solution is built using a [set of NuGet packages](http://www.wadewegner.com/2011/11/nuget-packages-for-windows-azure-and-windows-phone-developers/) that our team has built for Windows Phone + Windows Azure – they provide a similar development experience by allowing you to better manage dependencies and compose great application experiences on the Windows Phone.

Today I want to show a similar way to build support for sending push notifications to Windows Phone applications.

Push notifications provide you a way to deliver information to your applications that are installed on someone’s Windows Phone. It can help provide a key way to differentiate your application from other applications – especially when you tap into tile notifications and take advantage of background tiles, deep linking, and the like. There are a lot of great blog posts on this topic:

* [Push Notifications Overview for Windows Phone](http://msdn.microsoft.com/en-us/library/ff402558%28v=VS.92%29.aspx)

* [Understanding Microsoft Push Notifications for Windows Phones](http://windowsteamblog.com/windows_phone/b/wpdev/archive/2010/05/03/understanding-microsoft-push-notifications-for-windows-phones.aspx) (somewhat old) 

* [Understanding How Microsoft Push Notification Works](http://windowsteamblog.com/windows_phone/b/wpdev/archive/2010/05/04/understanding-how-microsoft-push-notification-works-part-2.aspx) (somewhat old) 

* [Live Tiles](http://www.jeffblankenburg.com/2011/11/11/31-days-of-mango-day-11-live-tiles/) (part of the [31 Days of Mango](http://www.jeffblankenburg.com/2011/10/31/31-days-of-mango/) series) 
 
One problem I’ve observed with push notifications is most people aren’t sure what to do with the channel URI’s received from the Microsoft Push Notification Service (MPNS). Developers also don’t know where or how to send messages to the device—should it be a service, and if so, where does it run? This is where Windows Azure can provide a lot of help.

Through the Windows Azure Toolkit for Windows Phone, we’ve been providing push notification services for quite a long time. It’s a great solution, and one that has helped a lot of folks. However, it was also relatively difficult to take our samples and then update them such that they worked in your applications. This is where the NuGet packages come into play. We’ve completely refactored the underlying libraries and now deliver all the capabilities as individual NuGets – you can easily create a new Windows Phone application—or update an existing one—using these NuGets.

A few comments on how these NuGets collaborate with the Windows Phone and the MPNS:

1. **The Windows Phone Application registers in the MPNS**: The Windows Phone application opens a notification channel to the MPNS and indicates that it wishes to receive push notification messages. The MPNS creates a subscription endpoint associated with that particular channel and forwards it to the Windows Phone device (and the specific application) using the channel it’s just opened. The MPNS sends the endpoint to the application so that the application can send it to the service from which it plans to receive notifications. 

2. **The Windows Phone Client registers with the Web Role**: The Windows Phone application invokes a service in the Web Role to register itself with the subscription endpoint received from the MPNS. This endpoint is the URI to which the cloud application will perform the HTTP POSTs to send push notification messages to the device. 

3. **The Cloud Service sends a notification request to the MPNS**: The cloud services sends a notification request by doing a HTTP POST in a specific XML format defined by the MPNS protocol to the subscription endpoint associated with the device it wants to notify. 

4. **The MPNS sends the notification to the Windows Phone device**: The MPNS transforms the notification request it received to a proper Push Notification to send to the Windows Phone device associated with the endpoint where it received the notification request. The notification request can ask for a toast, a tile, or a raw notification. Once the device receives the push notification via the Push Client it will route the notification to the Shell, which will take an action according to the status of the application. If the application is not running, the shell will either update the application tile, or show a toast. If the application is running, it will send the notification to the already running application. 

This architectural picture should help explain the interactions:

![Architecture](https://wadewegner.blob.core.windows.net/wordpress/2011/11/Architecture.png)

As with the Windows Phone and ACS example, I want to walk you through the whole process. There’s certainly more that you can do, but I think you’ll agree that the following is quite compelling.

1. Create a new Windows Phone OS 7.1 application.        
![NewWindowsPhoneApplication7.1](https://wadewegner.blob.core.windows.net/wordpress/2011/11/NewWindowsPhoneApplication7.1.jpg)

2. From the **Package Manager Console** type the following to install the ACS base login page NuGet package for Windows Phone: Install-Package Phone.Notifications.BasePage.          
![Phone.Notifications.BasePage](https://wadewegner.blob.core.windows.net/wordpress/2011/11/Phone.Notifications.BasePage.jpg)

3. Update the** WMAppManifest.xml** file so that the default page is the **Push.xaml**. This way the user will come to the login page before the **MainPage.xaml.          
![WMAppManifest](https://wadewegner.blob.core.windows.net/wordpress/2011/11/WMAppManifest.jpg)**

4. Let’s create a page that we can use to demonstration deep linking with MPNS. Create a new **Windows Phone Portrait Page** called **DeepLinkPage.xaml** in the **Pages** folder. 

5. In the **ContentPanel** grid, add a **TextBlock **control. We’ll push a message to this control when sending a notification.       
    
		protected override void OnNavigatedTo(NavigationEventArgs e)
		{ 
			if (this.NavigationContext.QueryString.ContainsKey("message")) 
			{ 
				this.QueryString.Text = this.NavigationContext.QueryString["message"]; 
			} 
			else 
			{ 
				this.QueryString.Text = "no message was received."; 
			} 
		}

6. We need to write the handler that will write the code to the **TextBlock**. 

		protected override void OnNavigatedTo(NavigationEventArgs e) 
		{ 
			if (this.NavigationContext.QueryString.ContainsKey("message")) 
			{ 
				this.QueryString.Text = this.NavigationContext.QueryString["message"]; 
			} 
			else 
			{ 
				this.QueryString.Text = "no message was received."; 
			} 
		}

7. That’s all there is to do in the Windows Phone client. Now we have to write the services that will store the Channel URI generated by MPNS and allow us to send notifications to the device. Add a new **Windows Azure Project **to the solution. Select the **Internet Application** template using the **HTML 5 semantic markup**. 

	![WindowsAzureProject](https://wadewegner.blob.core.windows.net/wordpress/2011/11/WindowsAzureProject.jpg)
  
8. From the **Package Manager Console**, change the default project to **Web** (or whatever you called your MVC 3 web application), and then type the following to install the MPNS push notifications services and libraries into the web applications: Install-Package WindowsAzure.Notifications. The services installed will let clients register (and unregister) for receiving push notification messages. 

	![WindowsAzure.Notifications](https://wadewegner.blob.core.windows.net/wordpress/2012/01/WindowsAzure.Notifications.png)

9. At this point we don’t have any means for sending a notification. To make this easier, we’ve built a sample management UI for Windows Phone that allows you to manually send push notifications to registered devices. This registers a sample MVC Area called “Notifications” containing the UI and the MPNS Recipe for sending all the supported types of push notifications. 
    
	![WindowsPhone.Notifications.ManagementUI.Sample](https://wadewegner.blob.core.windows.net/wordpress/2011/11/WindowsPhone.Notifications.ManagementUI.Sample.jpg)

10. At this point you’re ready to roll! Hit **Control-F5 **to build and run. When your website starts browse to http://127.0.0.1:81/notifications and notice that you don’t have any clients currently registered. 

	![image](https://wadewegner.blob.core.windows.net/wordpress/2011/11/image.png)

11. In the Windows Phone emulator, click to **Enable push notifications**. Wait until you receive confirmation that the channel has successfully registered. 

	![RegisterChannel](https://wadewegner.blob.core.windows.net/wordpress/2011/11/RegisterChannel.jpg)

12. Reload the notifications page and you’ll see that you now have a registered channel. 

	![RegisteredChannel](https://wadewegner.blob.core.windows.net/wordpress/2011/11/RegisteredChannel.jpg)

13. Click the **Send Notification**. Select the **Raw** notification type, type a message, and click **send**. 

	![SendRawMessage](https://wadewegner.blob.core.windows.net/wordpress/2011/11/SendRawMessage.jpg)

14. In your application on the Windows Phone emulator you should receive the message. This is great – we’ve just received a push notification message in our application. 
    
	![RawMessageReceived](https://wadewegner.blob.core.windows.net/wordpress/2011/11/RawMessageReceived.jpg)

15. On the device emulator, click the Windows button, pan to the right, and pin the **PushNotifications** application to start. 

16. Now, from the website, click **Send Notification**. This time select a **Tile **notification type. Change the **Title**, set the **Count**, and choose a **Background Image**. Click **Send**. 

	![SendTileMessage](https://wadewegner.blob.core.windows.net/wordpress/2011/11/SendTileMessage.jpg)

17. Notice how the **title**, **tile** **background**, and **count** have now updated on the device! 

	![ReceivedTileMessage](https://wadewegner.blob.core.windows.net/wordpress/2011/11/ReceivedTileMessage.jpg)

18. Lastly, from the website, click **Send Notification**. This time select a **Toast **notification type. Set a **Title**, **Sub Title**, and set the **Target Page **to: /Pages/DeepLinkPage.xaml?message=Hello. Click **Send**. 

	![SendToastMessage](https://wadewegner.blob.core.windows.net/wordpress/2011/11/SendToastMessage.jpg)

19. You’ll receive a toast message (i.e “**Title** **Sub Title**”) on the device. Click it. This will open up the DeepLink.xaml page and pass along the message “Hello” that was sent in the toast. 

![ReceiveToastMessage](https://wadewegner.blob.core.windows.net/wordpress/2011/11/ReceiveToastMessage.jpg)

And that’s it! You can now quickly enrich your Windows Phone applications by leveraging push notifications.

While the Sample UI works great for development, you’ll most likely want to go a few steps further and write your own services or processes to generate notifications. Worker roles with queues work great in this space – I’ll definitely write about this in the future.

Long story short, the following three NuGet packages make it really easy to take advantage of Push Notifications on the Windows Phone using Windows Azure:
  
* Install-Package [Phone.Notifications.BasePage](http://www.nuget.org/List/Packages/Phone.Notifications.BasePage)

* Install-Package [WindowsAzure.Notifications](http://www.nuget.org/List/Packages/WindowsAzure.Notifications)

* Install-Package [WindowsPhone.Notifications.ManagementUI.Sample](http://www.nuget.org/List/Packages/WindowsPhone.Notifications.ManagementUI.Sample)

Give it a try and tell me if you agree.

I hope this helps!