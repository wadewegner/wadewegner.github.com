---
author: admin
comments: true
date: 2011-08-07 20:59:32+00:00
layout: post
slug: using-windows-azure-blob-storage-and-cdn-with-wordpress
title: Using Windows Azure Blob Storage and CDN with WordPress
wordpress_id: 1351
categories:
- PHP
- Plugin
- Windows Azure
- WordPress
---

_**UPDATE**: In publishing this post, I encountered an issue with the plugin. Originally I used OneNote to create screen clippings of all the images in the post, then I pasted them into Live Writer. This is my normal practice for blog posts. Unfortunately, when I published, there ended up being a mismatch between the files stored in blob storage and those included in my blog post. However, when I updated the blog post and made actual JPEGs for each image (instead of screen clippings), everything worked fine. So, until I track down the underlying issue, simply save the image to a file first._

 

Serving up your blogs content through CDN can create a much better experience for your readers (additionally, I’m told it helps in page ranking, so search engines will rank based on response time). I personally use WordPress running on Windows Server (not yet Windows Azure), and using Windows Azure Blob Storage and CDN seemed daunting until I found a few resources to help. Below you’ll find a tutorial on setting this up yourself.

 

There are two resources you need to grab in order to easily set this up.

 

  
  * [Windows Azure SDK for PHP Developers v3.0.0](http://phpazure.codeplex.com/releases/view/66558)
   
  * [Windows Azure Storage for WordPress](http://wordpress.org/extend/plugins/windows-azure-storage/)
 

_**Important**: These two resources ship separately, which means that you must ensure you’re grabbing versions designed to work together. In my case, I’ve downloaded version 3.0.0 of the Windows Azure SDK for PHP v3.0.0 even thought v4.0.1 is the most recent drop._

 

These two resources provide documentation that _I assume _works well when running on Linux, but there’s hardly any documentation for those of us running WordPress on Windows Server (in my case Windows Server 2008 R2). Here are the steps I took to configure these resources.

 

_**Setting up the Windows Azure SDK for PHP Developers v3.0.0**_

 

Downloading the [v3.0.0 SDK](http://phpazure.codeplex.com/releases/view/66558) will yield a zip field named _PHPAzure-3.0.0.zip_. The first thing I did was to unzip it under C:\Program Files (x86)\PHPAzure-3.0.0. Why did I choose this folder structure? Two reasons:

 

  
  1. PHP itself is installed to C:\Program Files (x86)\, so I thought I’d keep this SDK near to PHP. 
   
  2. It’s possible in the future that I’ll want to use v4.0.1 along side version v.3.0.0 (although to be honest I don’t know if this is supported) and I didn’t want to get myself stuck by using a non-versioned folder. 
 

You should now have a folder that looks like this:

 

![CDNWordpress-1](https://wadewegner.blob.core.windows.net/wordpress/2011/08/CDNWordpress-1.jpg)

 

In order to take advantage of the SDK, you need to update your php.ini file and add the path to the library folder in the _include_path _variable. Browse to your PHP folder (in my case it is C:\Program Files (x86)\PHP\v5.2) and open the php.ini file.

 

Search for the include_path variable. It might help to use “include_path = “ for your search terms. In my case, this variable was commented out, so I had to both uncomment the variable and add my path. In the end, you should have something looking like this:

 

include_path = ".;c:\Program Files (x86)\PHPAzure-3.0.0"

 

Pretty reasonable. This is all you need to do with the Windows Azure SDK for PHP Developers.

 

**_Setting up Windows Azure Storage for WordPress_**

 

Now you can download the [Windows Azure Storage for WordPress](http://wordpress.org/extend/plugins/windows-azure-storage/) plugin. This time I unzipped the contents of the folder to my plugins folder at C:\Websites\WadeWegner\wp-content\plugins. Your folder path will most definitely be different; however, I will need to be under “wp-content\plugins” to work correctly.

 

Once unzipped, you should see the following:

 

![CDNWordpress-2](https://wadewegner.blob.core.windows.net/wordpress/2011/08/CDNWordpress-2.jpg)

 

Now, to get this to work, you also have to add the path to the SDK in the windows-azure-storage.php file. It took me a bit to figure out exactly what to update, but in the end it’s not particularly challenging.

 

We’re going to update the set_include_path method so that it includes the SDK library.

 

Initially, the code block looks like this:

 

	if (isset($_SERVER["APPL_PHYSICAL_PATH"])) {        
	set_include_path(         
	get_include_path() . PATH_SEPARATOR . $_SERVER["APPL_PHYSICAL_PATH"]         
	);         
	}

 

You should update it accordingly (where the updates are highlighted in yellow):

 

	if (isset($_SERVER["APPL_PHYSICAL_PATH"])) {        
	set_include_path(         
	get_include_path() . PATH_SEPARATOR . $_SERVER["APPL_PHYSICAL_PATH"] . PATH_SEPARATOR . 'C:\\Program Files (x86)\\PHPAzure-3.0.0\\library'         
	);         
	}

 

Be sure to use the correct path based on where you placed the SDK. Also, but sure use double backslashes, or it won’t work.

 

At this point you should be ready to activate and configure the plugin.

 

**_Activating and Configuring the Windows Azure Storage plugin._**

 

In WordPress, browse to your Installed Plugins. The Windows Azure Storage for WordPress plugin will be deactivated – activate it. If activation fails, then something above is incorrect.

 

Once activated, you’ll have a **Windows Azure** link under **Settings**. Browse to it.

 

Here’s where you’ll specify information specific to your Windows Azure storage account. In the [Windows Azure Platform Management Portal](http://windows.azure.com/), login and grab your storage name and key. You can get this by going to **Hosted Services, Storage Accounts & CDN –> Storage Accounts**. From here, select the appropriate storage account. Under properties you can get the name from the **Name** property, and you can get your key by clicking the **View** button under the **Primary access key** property.

 

Back in WordPress, enter your **Storage Account Name** and **Primary Access Key**. It should look something like this:

 

![CDNWordpress-3](https://wadewegner.blob.core.windows.net/wordpress/2011/08/CDNWordpress-3.jpg)

 

Of course, your values will be different.

 

Click the **Save Changes** button. Once you’ve saved, the **Default Storage Container** dropdown list will populate with storage containers listed in your storage account. Here’s what mine looks like:

 

![CDNWordpress-4](https://wadewegner.blob.core.windows.net/wordpress/2011/08/CDNWordpress-4.jpg)

 

I created a container called “wordpress”. If you don’t see a container you want to use, you’ll have to create it. To do this easily, I recommend you use a tool like [CloudXplorer](http://clumsyleaf.com/products/downloads). This tool is not only useful for creating the container, but setting the access control policy to “Public read access (blobs only)” – this is important because by default the container is marked “private” and is not accessible through the browser without the proper credentials.

 

When you’ve set the policy correctly, it should look like this:

 

![CDNWordpress-5](https://wadewegner.blob.core.windows.net/wordpress/2011/08/CDNWordpress-5.jpg)

 

Test this out and make sure you can browse to a file in your container and view it correctly.

 

Once you have your container, select it and click **Save Changes**.

 

Lastly, I recommend you check the **Use Windows Azure Storage for default upload** option so that by default your resources will go to Windows Azure storage instead of your server. Click **Save Changes** when complete.

 

At this point everything should be correctly setup such that when you publish a blog post – even with a tool such as Windows Live Writer – it will place your images and resources into your Windows Azure blob storage account. This is great! Test it out yourself (but I recommend you publish as a draft post just in case things don’t work correctly).

 

Now that you have this setup correctly, I think you should strongly consider using the CDN. The primary reason to do this is so that your site loads as fast as possible for your readers. Take a look at the article [WordPress + CDN == Fast Load Times](http://www.bloggingpro.com/archives/2009/07/07/wordpress-cdn-fast-load-times/) for more info.

 

**_Setting Up the CDN_**

 

While this is a bit more advanced of a topic, it is by no means difficult to accomplish.

 

The Windows Azure CDN is setup such that you can easily turn the CDN on for your storage account such that everything stored in blob storage can get served through the CDN. In the Windows Azure Platform Management Portal, select **Hosted Services, Storage Accounts & CDN –> CDN**. Select your storage account. In the upper left and corner of the portal, click the button **New Endpoint**.

 

![CDNWordpress-6](https://wadewegner.blob.core.windows.net/wordpress/2011/08/CDNWordpress-6.jpg)

 

A dialog window will open that gives you the opportunity to choose some settings:

 

![CDNWordpress-7](https://wadewegner.blob.core.windows.net/wordpress/2011/08/CDNWordpress-7.jpg)

 

You don’t need to change any of the settings (unless you want to do so). Click **OK**.

 

At this point it will start to create and propagate your CDN settings. This **will take awhile**, so don’t get impatient – it needs to propagate to the 26 (or more) CDN nodes worldwide.

 

Once this process has completed, you’ll see a note saying that the CDN endpoint has been enabled (again, note that the portal will say **Enabled** before everything has propagated worldwide – patience!).

 

![CDNWordpress-8](https://wadewegner.blob.core.windows.net/wordpress/2011/08/CDNWordpress-8.jpg)

 

Now, once this is done, you’ll be able to browse to resources in storage through the CDN URL, which will look something like this:

 

![Mug-195w](http://az29238.vo.msecnd.net/images/Mug-195w.jpg)

 

While this works perfectly well, I personally don’t like the non-customized URL. To fix this, you can use a custom domain name of your own choosing with the CDN. Take a look at Steve Marx’s post on [Using the New Windows Azure CDN with a Custom Domain](http://blog.smarx.com/posts/using-the-new-windows-azure-cdn-with-a-custom-domain). Warning: his post pre-dates the new portal design, but conceptually it’s still the same. Note that once you verify your account it can take another 60 minutes or so for your new custom domain name to propagate to the CDN nodes. The end result should look like this:

 

![mug](https://wadewegner.blob.core.windows.net/images/Mug-195w.jpg)

 

If you run nslookup on the custom domain name, you should see that it ultimately resolves to the CDN:

 

![CDNWordpress-9](https://wadewegner.blob.core.windows.net/wordpress/2011/08/CDNWordpress-9.jpg)

 

Great, now you have successfully enabled the CDN on your storage account along with a custom domain name. Now we have to perform one last update in WordPress.

 

**_Using the CDN with the WordPress Plugin_**

 

Last step is to configure the Windows Azure Storage for WordPress plugin to serve your content up via the CDN.

 

One the plugin settings page you’ll se a **CNAME** property you can specify. Set the value to your custom domain’s FQDN, like this: [http://images.wadewegner.com](http://images.wadewegner.com). It should look like this on the page:

 

![CDNWordpress-10](https://wadewegner.blob.core.windows.net/wordpress/2011/08/CDNWordpress-10.jpg)

 

Make sure to click **Save Changes**.

 

**_Test it Out!_**

 

The easiest way to test it out is to create a draft blog post with an image. I’ll use Windows Live Writer. Here’s a quick example.

 

![CDNWordpress-11](https://wadewegner.blob.core.windows.net/wordpress/2011/08/CDNWordpress-11.jpg)

 

When you go to post, but sure to use **Post draft to blog**. This way, if you screw up, no one will know.

 

After you post, take a look at the preview of the post. In particular, right-click on the image and look at the URL:

 

![CDNWordpress-12](https://wadewegner.blob.core.windows.net/wordpress/2011/08/CDNWordpress-12.jpg)

 

See how it’s getting served up from the custom CDN domain? Awesome!

 

Furthermore, take a look at blob storage. You’ll see that your resources/images where uploaded:

 

![CDNWordpress-13](https://wadewegner.blob.core.windows.net/wordpress/2011/08/CDNWordpress-13.jpg)

 

That’s it!

 

I hope you find this useful. I plan to go through all the resources on my blog soon and update them such that the images are served through the CDN, but I’ll save that for another post!
