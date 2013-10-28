---
author: admin
comments: true
date: 2011-11-17 05:18:58+00:00
layout: post
slug: outsourcing-user-authentication-in-a-windows-phone-application
title: Outsourcing User Authentication in a Windows Phone Application
wordpress_id: 1663
categories:
- Access Control Service
- NuGet
- Windows Azure
- Windows Phone
---

Yesterday I shared all the [NuGet packages we’re building](http://www.wadewegner.com/2011/11/nuget-packages-for-windows-azure-and-windows-phone-developers/) to make it easy to build Windows Phone and Windows Azure applications. Today I wanted to share how easy it is to build a Windows Phone application that leverages the [Windows Azure Access Control service](http://www.microsoft.com/windowsazure/features/accesscontrol/).

The **Phone.Identity.AccessControl.BasePage** NuGet package includes a control for Window Phone that allows your phone applications to outsource user authentication to the Windows Azure Access Control service (ACS). This service enables your users to login by reusing their existing accounts from identity providers such as Windows Live ID, Google, Yahoo, Facebook, and even Active Directory. If you want to know more about ACS you can take a look at the dedicated hands-on labs in the [Windows Azure Platform Training Course](http://msdn.microsoft.com/gg271268).

Using this NuGet package and the included control for ACS in your Windows Phone applications takes care of all the runtime interactions with ACS. Additionally, this package provides a base login page that uses the control and is easy to setup in your phone application. All that is left for you to do is to configure your ACS namespace via the management portal (i.e. specifying your preferences such as the identity providers you want to enable in your application) and integrate the login page into your existing Windows Phone application.

For more information on setting up ACS take a look at the resources at [http://acs.codeplex.com/](http://acs.codeplex.com/)

To help simplify the process below, I’m making the assumption you already have ACS setup and configured. I’ll be using the following values in the below sample (no guarantee that they’ll be available when you read this post but I’ll do my best):

* **namespace**: watwindowsphone 

* **realm**: uri:watwindowsphone 

Without further ado, here are the steps to build a Windows Phone application that outsources authentication to ACS:

1. Create a new **Windows Phone OS 7.1** application.         

	![WindowsPhoneOS71](http://images.wadewegner.com/wordpress/2011/11/WindowsPhoneOS71.jpg)

2. From the **Package Manager Console** type the following to install the ACS base login page NuGet package for Windows Phone: Install-Package Phone.Identity.AccessControl.BasePage          

	![InstallPackage](http://images.wadewegner.com/wordpress/2011/11/InstallPackage.jpg)

3. Update the **AccessControlResources.xaml** resources file to use your ACS namespace and the realm you have configured.
       
		static ChannelFactory<ICustomerChannel> customersChannelFactory; 
		static ICustomerChannel customersChannel;
		
		public static IEnumerable<Customer> GetCustomers() 
		{ 
		    List<Customer> customers = null;
		
		    if (customersChannelFactory == null) 
		    { 
		        Uri serviceUri = ServiceBusEnvironment.CreateServiceUri("sb", "MYSERVICENAMESPACE", "Customer"); 
		        customersChannelFactory = new ChannelFactory<ICustomerChannel>("RelayEndpoint", new EndpointAddress(serviceUri))
		
		        int tries = 0; 
		        while (tries++ < 3) 
		        { 
		            try 
		            { 
		                if (customersChannel == null) 
		                {
		                    customersChannel = customersChannelFactory.CreateChannel(); 
		                    customersChannel.Open(); 
		                }
		
		                return customersChannel.GetCustomers(); 
		            } 
		            catch (CommunicationException) 
		            {
		                customersChannel.Abort(); 
						customersChannel = null; 
		            } 
		        }
		            
		        return customers; 
		    }
		}

4. Update the **WMAppManifest.xml** file so that the default page is the **LoginPage.xaml**. This way the user will come to the login page before the **MainPage.xaml**.

	![WMAManifiest](http://images.wadewegner.com/wordpress/2011/11/WMAManifiest.jpg)

5. Update the **LoginPage.xaml.cs** so that the user is navigated to the **MainPage.xaml **upon successfully logging into the application. Make sure to update **Line 23 **and **Line 33**. 

		this.NavigationService.Navigate(new Uri("/MainPage.xaml", UriKind.Relative));
  
6. Let’s display some information from the Simple Web Token. Add a **TextBlock** control to the **MainPage.xaml **page.

		<Grid x:Name="ContentPanel" Grid.Row="1" Margin="12,0,12,0"> <TextBlock Name="DisplayLoginInfo" /> </Grid>
  
7. Add a Loaded event for the **MainPage.xaml**. In this event you’ll want to load the **simpleWebTokenStore** out of the application resources. You can then use it to grab resources like the name identifier or various other claim types (like Name). Finish by updating the **DisplayLoginInfo**textblock. 

		using Microsoft.WindowsAzure.Samples.Phone.Identity.AccessControl;
	
		...
	
		var simpleWebTokenStore = Application.Current.Resources["swtStore"] as SimpleWebTokenStore;
		var userNameIdentifier = simpleWebTokenStore.SimpleWebToken.NameIdentifier;
		var name = simpleWebTokenStore.SimpleWebToken.Claims[ClaimTypes.Name];
		
		this.DisplayLoginInfo.Text = "Identifier: " + userNameIdentifier + Environment.NewLine + "Name: " + name;

8. Run the application. I’d recommend using Facebook, Google, or Yahoo! for the identity providers, as Live ID does not provide the name claim type in the SWT token.

	![LoginExperience](http://images.wadewegner.com/wordpress/2011/11/LoginExperience.jpg)

And that’s it! You can now take advantage of the Identifier claim (and others) in your phone application for many things – tracking users, displaying additional user information, and so forth. Additionally, you can use these claims to authenticate against additional services running in Windows Azure – I’ll cover this token in a future post.

The **Phone.Identity.AccessControl.BasePage **NuGet package makes it really easy for you to take advantage of the Windows Azure Access Control service within your applications. ACS provides a great way for you to leverage your users existing identity providers when using your application.

I hope this helps!