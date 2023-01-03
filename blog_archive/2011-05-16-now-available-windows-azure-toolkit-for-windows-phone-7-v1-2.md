---
author: admin
categories:
- Access Control Service
- Toolkit
- Windows Azure
- Windows Phone 7
comments: true
date: "2011-05-16T05:48:11Z"
slug: now-available-windows-azure-toolkit-for-windows-phone-7-v1-2
title: 'NOW AVAILABLE: Windows Azure Toolkit for Windows Phone 7 v1.2'
wordpress_id: 1228
---

Here it is – the [Windows Azure Toolkit for Windows Phone 7 v1.2](http://watoolkitwp7.codeplex.com/)!

 

As I mentioned last week when I spoke about [Updates Coming Soon to the Windows Azure Toolkit for Windows Phone 7](http://www.wadewegner.com/2011/05/updates-coming-soon-to-the-windows-azure-toolkit-for-windows-phone-7/), we have some really important and valuable additions to the toolkit.

 

  
  * Support and tooling for the Access Control Service 2.0 
   
  * Support for Windows Azure Storage Queues 
   
  * Updated UI/UX for the management web application 
 

These are significant updates – particularly the support for ACS. Given the number of updates since version 1.0 – don’t forget that we added Microsoft Push Notification support, and more, in version 1.1 – I decided to redo the Getting Started with the Windows Azure Toolkit for Windows Phone 7 video.

 

 

I highly recommend you take a look at the following resources to learn more:

 

  

      

        
![GettingStartedWAZToolkitWP7v12_512_c](https://wadewegner.blob.core.windows.net/wordpress/2011/05/GettingStartedWAZToolkitWP7v12_512_c.jpg)

[Getting Started with the Windows Azure Toolkit for Windows Phone 7](http://channel9.msdn.com/posts/Getting-Started-with-the-Windows-Azure-Toolkit-for-Windows-Phone-7-v12)

           

by [Wade Wegner](http://www.wadewegner.com/)

        
              

        
![WAZWP7ACS1_512_ch9632](https://wadewegner.blob.core.windows.net/wordpress/2011/05/WAZWP7ACS1_512_ch9632.jpg)

[Getting Started with ACS and the Windows Azure Toolkit for Windows Phone 7](http://channel9.msdn.com/Shows/Identity/Getting-Started-with-ACS-and-the-Windows-Azure-Toolkit-for-Windows-Phone-7)

           

by [Vittorio Bertocci](http://blogs.msdn.com/b/vbertocci/)

        
           

 

  
  * Vittorio Bertocci: [Bring Your Active Directory in Your Pockets with ACS, OAuth 2.0 and the New Windows Azure Toolkit for Windows Phone 7](http://blogs.msdn.com/b/vbertocci/archive/2011/05/15/bring-your-active-directory-in-your-pockets-with-acs-oauth-2-0-and-the-new-windows-azure-toolkit-for-windows-phone-7.aspx)
   
  * Vittorio Bertocci: [Windows Azure Toolkit for Windows Phone 7 1.2 will Integrate with ACS](http://blogs.msdn.com/b/vbertocci/archive/2011/05/09/windows-azure-toolkit-for-windows-phone-7-1-2-will-integrate-with-acs.aspx)
   
  * Wade Wegner: [Using Windows Azure for Windows Phone 7 Push Notification Support](http://www.wadewegner.com/2011/05/using-windows-azure-for-windows-phone-7-push-notification-support/)
 

We also have a fantastic set of articles on CodePlex that you should take a look at:

 

  
  * [Setup and Configuration](http://watoolkitwp7.codeplex.com/wikipage?title=Setup%20and%20Configuration&referringTitle=Documentation)
   
  * [Toolkit Content](http://watoolkitwp7.codeplex.com/wikipage?title=Toolkit%20Content&referringTitle=Documentation)
   
  * Getting Started             
    * [Creating a New Windows Phone 7 Cloud Application](http://watoolkitwp7.codeplex.com/wikipage?title=Creating%20a%20New%20Windows%20Phone%207%20Cloud%20Application)
       
    * [Running and Going Through the Windows Phone 7 Cloud Application](http://watoolkitwp7.codeplex.com/wikipage?title=Running%20and%20Going%20Through%20the%20Windows%20Phone%207%20Cloud%20Application)                      
      * Starting the Application 
           
      * Authenticating the User (ASP.NET Membership Authentication) 
           
      * Authenticating the User (ACS Authentication) 
           
      * Sending Microsoft Push Notifications 
           
      * Sending Apple Push Notifications 
           
      * Working with Tables, Blobs, and Queues 
               
       
   
  * User Authentication             
    * [How to Choose Between ASP.NET Membership and ACS](http://watoolkitwp7.codeplex.com/wikipage?title=Choosing%20the%20Access%20Control%20Strategy)
       
    * [How to Obtain Namespace and Management Keys](http://watoolkitwp7.codeplex.com/wikipage?title=Obtain%20Namespace%20and%20Management%20Key)
       
   
  * [Troubleshooting](http://watoolkitwp7.codeplex.com/wikipage?title=Troubleshooting&referringTitle=Documentation)
   
  * [Change Log](http://watoolkitwp7.codeplex.com/wikipage?title=Change%20Log)
 

**Version 1.2 Updates**

 

In version 1.1 we introduced support for Microsoft Push Notification Services. In version 1.2 we default to adding this service, but we give you the option of excluding if it’s not required. Additionally, we also let you choose whether you want to support Apple Push Notification Services in now:

 

![PushNotification](https://wadewegner.blob.core.windows.net/wordpress/2011/05/PushNotification.png)

 

Then, you can easily use the [Windows Azure Toolkit for iOS](http://www.wadewegner.com/2011/05/windows-azure-toolkit-for-ios/) to work with this service running in Windows Azure.

 

As mentioned extensively by Vittorio, you can also choose to use ACS instead of the simple ASP.NET membership service.

 

![ACS](https://wadewegner.blob.core.windows.net/wordpress/2011/05/ACS.png)

 

Take a look at [this article](http://watoolkitwp7.codeplex.com/wikipage?title=Choosing%20the%20Access%20Control%20Strategy) if you’re trying to determine which type of user authentication you should use. If you go with ACS, this produces a very nice login experience where you can choose one of your existing identity providers.

 

![WP7](https://wadewegner.blob.core.windows.net/wordpress/2011/05/WP7.png)

 

As with Blobs and Tables, we now provide full support for Windows Azure Queues. This allows you to enqueue and dequeue messages from your application.

 

![image](https://wadewegner.blob.core.windows.net/wordpress/2011/05/image13.png)

 

Finally, we were not particularly pleased with the out-of-the-box ASP.NET theme, so we updated it. Inspired by the Metro Design guidelines for Windows Phone 7, we came up with something nice and fresh.

 

![image](https://wadewegner.blob.core.windows.net/wordpress/2011/05/image14.png)

 

**Breaking Changes**

 

We’ve come far along enough now that it’s more important for us to track changes, in particular when they are breaking changes. If you used version 1.0 or 1.1 of this toolkit, I highly recommend you take a look at the [Change Log](http://watoolkitwp7.codeplex.com/wikipage?title=Change%20Log). If you’ve started to use the toolkit for building applications, there are potentially a couple changes you should review. The two I’ll call out here are:

 

  
  * In the **AuthenticationService** we changed the **LoginModel** class to **Login**. This means that you may have to update how authenticate to the membership service. 
   
  * We changed the **CreateUserModel** to **RegistrationUser**, and changed the name of its **UserName** property to **Name**. This class is used by the **AuthenticationService** to register new users. 
 

An affect of these changes could be an error when interacting with user data stored in table storage. For local development, the best thing to do would be to reset local storage so that you don’t have the old schema stored in a table.

 

![image_22](https://wadewegner.blob.core.windows.net/wordpress/2011/05/image_22.png)

 

**Next Steps**

 

We’ll have at least a couple more releases of this toolkit over the next month or two, and I’ll share the details with you as soon as they are locked. For now, please be sure to let us know if you think we should target other scenarios. Submit your [feedback on CodePlex](http://watoolkitwp7.codeplex.com/discussions) and join the discussion!

 

I hope this helps!
