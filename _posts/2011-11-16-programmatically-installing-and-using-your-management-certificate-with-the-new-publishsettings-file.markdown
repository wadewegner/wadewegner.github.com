---
author: admin
comments: true
date: 2011-11-16 21:11:10+00:00
layout: post
slug: programmatically-installing-and-using-your-management-certificate-with-the-new-publishsettings-file
title: Programmatically Installing and Using Your Management Certificate with the
  New .publishsettings File
wordpress_id: 1642
categories:
- Service Management API
- Tools
- Windows Azure
- X509Certificates
---

Earlier this week we released the [Windows Azure SDK 1.6](http://blogs.msdn.com/b/windowsazure/archive/2011/11/14/updated-windows-azure-sdk-amp-windows-azure-hpc-scheduler-sdk.aspx), which includes a lot of great updates to the emulators, tools for Visual Studio, and libraries. One of my favorite additions is a new way to get a management certificate installed into Windows Azure and onto your machine. You can now browse to [https://windows.azure.com/download/publishprofile.aspx](https://windows.azure.com/download/publishprofile.aspx) and login with your Live ID; this process will do two things:

 

  
  1. Generate a management certificate that is installed into Windows Azures on your behalf. 
   
  2. Prompts you to download a .publishsettings file which includes an encoded version of your certificate and all of your subscription IDs. 
 

The new tools for Visual Studio let you easily important this file and immediately start working with your subscriptions from within Visual Studio. It’s a _much_ simpler experience than in the past. In fact, on this weeks episode of the Cloud Cover Show (not yet published) Steve and I cover how to use this file from within your own code. While Steve beat me to it and [published a great blog post](http://blog.smarx.com/posts/calling-the-windows-azure-service-management-api-with-the-new-publishsettings-file) showing some of the things you can do, I thought I’d take this a slightly different way and show you a couple different things:

 

  
  * How to install the certificate into your personal certificate store (which is exactly what Visual Studio is doing). 
   
  * How to use the certificate from your person certificate store to make calls to the Service Management API. 
 

The code is very similar. Take a look:

 

  
    
    <span style="color: blue">var </span>publishSettingsFile = 
        <span style="color: #a31515">@"C:\\temp\\CORP DPE Account-11-16-2011-credentials.publishsettings"</span>;
    <span style="clear: both">
    <span style="color: #2b91af">XDocument </span>xdoc = <span style="color: #2b91af">XDocument</span>.Load(publishSettingsFile);
    <span style="clear: both">
    <span style="color: blue">var </span>managementCertbase64string =
        xdoc.Descendants(<span style="color: #a31515">"PublishProfile"</span>).Single().Attribute(<span style="color: #a31515">"ManagementCertificate"</span>).Value;
    <span style="clear: both">
    <span style="color: blue">var </span>importedCert = <span style="color: blue">new </span><span style="color: #2b91af">X509Certificate2</span>(
        <span style="color: #2b91af">Convert</span>.FromBase64String(managementCertbase64string));






Now that we’ve imported the certificate, we can extract some information. I’ll grab the certificate thumbprint, which uniquely identifies the certificate—we’ll use it later in the post.






  
    
    <span style="color: blue">string </span>thumbprint = importedCert.Thumbprint;






Additionally, I can grab my subscription ID from the .publishsettings file – this we will also use later.






  
    
    <span style="color: blue">string </span>subscriptionId = xdoc.Descendants(<span style="color: #a31515">"Subscription"</span>).First().Attribute(<span style="color: #a31515">"Id"</span>).Value;






Now, we can take our X509Certificate2 and install it directly into our certificate store.






  
    
    <span style="color: #2b91af">X509Store </span>store = <span style="color: blue">new </span><span style="color: #2b91af">X509Store</span>(<span style="color: #2b91af">StoreName</span>.My);
    store.Open(<span style="color: #2b91af">OpenFlags</span>.ReadWrite);
    store.Add(importedCert);
    store.Close();






After running this code, you can see that the certificate has been installed into my personal certificate store.





![CertMgr](http://images.wadewegner.com/wordpress/2011/11/CertMgr.png)





If you select the certificate you’ll see that it’s the same certificate with the same thumbprint.





![certificate](http://images.wadewegner.com/wordpress/2011/11/certificate.png)





Since the certificate is now loaded into the certificate store I can delete the .publishsettings file – I no longer need it. (It’s also a credential that I don’t want to let anyone else get their hands on.)





Now I have the following resources available to me:






  
  * My X509 certificate loaded in my personal certificate store. 


  
  * The thumbprint for the certificate (which we’ll use to identify the right certificate). 


  
  * My Windows Azure subscription ID. 





With this information we can do the exact same thing Steve shows in his post except without the .publishsettings file.






  
    
    <span style="color: #2b91af">X509Store </span>store = <span style="color: blue">new </span><span style="color: #2b91af">X509Store</span>(<span style="color: #2b91af">StoreName</span>.My);
    store.Open(<span style="color: #2b91af">OpenFlags</span>.ReadWrite);
    <span style="color: #2b91af">X509Certificate2 </span>managementCert = 
        store.Certificates.Find(<span style="color: #2b91af">X509FindType</span>.FindByThumbprint, thumbrprint, <span style="color: blue">false</span>)[0];
    <span style="clear: both">
    <span style="color: blue">var </span>req = (<span style="color: #2b91af">HttpWebRequest</span>)<span style="color: #2b91af">WebRequest</span>.Create(
        <span style="color: blue">string</span>.Format(<span style="color: #a31515">"https://management.core.windows.net/{0}/services/hostedservices"</span>, 
        subscriptionId));
    <span style="clear: both">
    req.Headers[<span style="color: #a31515">"x-ms-version"</span>] = <span style="color: #a31515">"2011-10-01"</span>;
    req.ClientCertificates.Add(managementCert);
    <span style="clear: both">
    <span style="color: #2b91af">XNamespace </span>xmlns = <span style="color: #a31515">"http://schemas.microsoft.com/windowsazure"</span>;
    <span style="clear: both">
    <span style="color: #2b91af">Console</span>.WriteLine(<span style="color: blue">string</span>.Join(<span style="color: #a31515">"\n"</span>,
        <span style="color: #2b91af">XDocument</span>.Load(req.GetResponse().GetResponseStream())
        .Descendants(xmlns + <span style="color: #a31515">"ServiceName"</span>).Select(n => n.Value).ToArray()));


  

  

Essentially, we can grab the certificate out of the certificate store using the thumbprint and then make the exact same call to the service management API.



  

The console output below shows that I’m able to get a list of all my hosted services:



  

![Console](http://images.wadewegner.com/wordpress/2011/11/Console.png)



  

It’s as simple as that!



  

I’m not sure that this post applies to everyone—in fact, most of you may find it boring or cryptic—but for those of you that are building content or tools you’ll probably find this a really simple way to automate a lot of the pieces. I know that my team plans to use these techniques in a lot of places to simply the experience of getting started with Windows Azure.



  

I hope this helps!
