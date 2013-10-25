---
author: admin
comments: true
date: 2010-12-21 18:10:50+00:00
layout: post
slug: web-deploy-with-windows-azure-on-restrictive-networks
title: Web Deploy with Windows Azure on Restrictive Networks
wordpress_id: 954
categories:
- Web Deploy
- Windows Azure
---

Some networks are more restrictive than others, and (much to my continual chagrin) this is especially true here at Microsoft. As I wrote and tested the post [Using Web Deploy with Windows Azure for Rapid Development](http://www.wadewegner.com/2010/12/using-web-deploy-with-windows-azure-for-rapid-development/), I found that I was not able to use Web Deploy on port 8172; turns out that we prevent SSL traffic on any port but 443. Fortunately, it’s pretty easy to resolve this restriction by modifying the [Input Endpoint values in the Service Definition](http://msdn.microsoft.com/en-us/library/ee758711(en-us).aspx#InputEndpoints).

 

Essentially, we need to map an external port (e.g. the port to which you will publish) to a local portal on the machine (e.g. the port with which Web Deploy listens). On the Microsoft network, this means that I’ll publish to port 443 but need it to resolve to a local port of 8172.

 

Before starting, review my post on [using Web Deploy with Windows Azure](http://www.wadewegner.com/2010/12/using-web-deploy-with-windows-azure-for-rapid-development/). There are only two differences, as noted below.

 

First, update the **InputEndpoint** values in your Service Definition file (ServiceDefinition.csdef) to map the external port to a local port.

   

  

Code: InputEndpoint

  1. <InputEndpoint name="mgmtsvc" protocol="tcp" port="443" localPort="8172" />

 

Second, after you’ve deployed to Windows Azure and your instance is up and running, you’ll need to update your publish settings for Web Deploy – specifically, the **Service URL**. Here are the values that I’ve gotten to work:

 

![WebDeployOn443](http://images.wadewegner.com/wordpress/2010/12/WebDeployOn443.png)

 

The screen shot cuts off the service URL. The full URL is:

 

  

https://**mywebdeploy**.cloudapp.net:**443**/msdeploy.axd

 

Of course, you’ll need to update these values to your DNS name and the port to which you’ll deploy.
