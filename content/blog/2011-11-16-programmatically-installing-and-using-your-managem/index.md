---
title: "Programmatically Installing and Using Your Management Certificate with the New .publishsettings File"
date: 2011-11-16T00:00:00-08:00
draft: false
description: "From the whiteboard to the keyboard"
---

Earlier this week we released the [Windows Azure SDK 1.6](http://blogs.msdn.com/b/windowsazure/archive/2011/11/14/updated-windows-azure-sdk-amp-windows-azure-hpc-scheduler-sdk.aspx), which includes a lot of great updates to the emulators, tools for Visual Studio, and libraries. One of my favorite additions is a new way to get a management certificate installed into Windows Azure and onto your machine. You can now browse to <https://windows.azure.com/download/publishprofile.aspx> and login with your Live ID; this process will do two things:

1. Generate a management certificate that is installed into Windows Azures on your behalf.
2. Prompts you to download a .publishsettings file which includes an encoded version of your certificate and all of your subscription IDs.

The new tools for Visual Studio let you easily important this file and immediately start working with your subscriptions from within Visual Studio. It’s a *much* simpler experience than in the past. In fact, on this weeks episode of the Cloud Cover Show (not yet published) Steve and I cover how to use this file from within your own code. While Steve beat me to it and [published a great blog post](http://blog.smarx.com/posts/calling-the-windows-azure-service-management-api-with-the-new-publishsettings-file) showing some of the things you can do, I thought I’d take this a slightly different way and show you a couple different things:

- How to install the certificate into your personal certificate store (which is exactly what Visual Studio is doing).
- How to use the certificate from your person certificate store to make calls to the Service Management API.

The code is very similar. Take a look:

```
var publishSettingsFile = @"C:\temp\CORP DPE Account-11-16-2011-credentials.publishsettings";
XDocument xdoc = XDocument.Load(publishSettingsFile);

var managementCertbase64string = xdoc.Descendants("PublishProfile").Single().Attribute("ManagementCertificate").Value;
var importedCert = new X509Certificate2(Convert.FromBase64String(managementCertbase64string));
```

Now that we’ve imported the certificate, we can extract some information. I’ll grab the certificate thumbprint, which uniquely identifies the certificate—we’ll use it later in the post.

```
thumbprint = importedCert.Thumbprint;
```

Additionally, I can grab my subscription ID from the .publishsettings file – this we will also use later.

```
string subscriptionId = xdoc.Descendants("Subscription").First().Attribute("Id").Value;
```

Now, we can take our X509Certificate2 and install it directly into our certificate store.

```
X509Store store = new X509Store(StoreName.My); store.Open(OpenFlags.ReadWrite); store.Add(importedCert); store.Close();
```

After running this code, you can see that the certificate has been installed into my personal certificate store.

![CertMgr](https://wadewegner.blob.core.windows.net/wordpress/2011/11/CertMgr.png)

If you select the certificate you’ll see that it’s the same certificate with the same thumbprint.

![certificate](https://wadewegner.blob.core.windows.net/wordpress/2011/11/certificate.png)

Since the certificate is now loaded into the certificate store I can delete the .publishsettings file – I no longer need it. (It’s also a credential that I don’t want to let anyone else get their hands on.)

Now I have the following resources available to me:

- My X509 certificate loaded in my personal certificate store.
- The thumbprint for the certificate (which we’ll use to identify the right certificate).
- My Windows Azure subscription ID.

With this information we can do the exact same thing Steve shows in his post except without the .publishsettings file.

```
X509Store store = new X509Store(StoreName.My);
store.Open(OpenFlags.ReadWrite);

X509Certificate2 managementCert = store.Certificates.Find(X509FindType.FindByThumbprint, thumbrprint, false)[0];

var req = (HttpWebRequest)WebRequest.Create(string.Format("https://management.core.windows.net/{0}/services/hostedservices", subscriptionId));
req.Headers["x-ms-version"] = "2011-10-01";
req.ClientCertificates.Add(managementCert);

XNamespace xmlns = "http://schemas.microsoft.com/windowsazure";

Console.WriteLine(string.Join("\n", XDocument.Load(req.GetResponse().GetResponseStream()).Descendants(xmlns + "ServiceName").Select(n => n.Value).ToArray()));
```

Essentially, we can grab the certificate out of the certificate store using the thumbprint and then make the exact same call to the service management API.

The console output below shows that I’m able to get a list of all my hosted services:

![Console](https://wadewegner.blob.core.windows.net/wordpress/2011/11/Console.png)

It’s as simple as that!

I’m not sure that this post applies to everyone—in fact, most of you may find it boring or cryptic—but for those of you that are building content or tools you’ll probably find this a really simple way to automate a lot of the pieces. I know that my team plans to use these techniques in a lot of places to simply the experience of getting started with Windows Azure.

I hope this helps!
