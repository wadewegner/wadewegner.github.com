---
layout: post
title: "Build a Windows 8 App for Salesforce With No Code Using Project Siena"
description: "Project Siena lets business users build rich business applications for Windows 8. See how you can build a Windows 8 application using Project Siena that connects to data in Salesforce."
categories: 
- salesforce
- force.com
- REST
- WADL
- Project Siena
---

**Microsoft Project Siena (Beta)** is a tool that allows a business user (i.e. non-developer) to quickly building rich business applications for Windows 8. You can learn all about the tool on the [Project Siena website](http://www.microsoft.com/en-us/projectsiena/default.aspx) and by watching a fantastic set of tutorials on the [Microsoft Project Siena (Beta) Channel 9 Blog](http://channel9.msdn.com/Blogs/Microsoft-Project-Siena).

Having recently left Salesforce to join Microsoft, I was curious about how to build a Project Siena app that integrated into Salesforce. Project Siena uses the Web Application Description Language, or WADL, to connect to 3rd party SaaS services. WADL is an open web standard that is for REST what WSDL is for SOAP, meaning it describes the service resources and relationships.

The release of the [WADL Generator for Project Siena (Alpha)](http://gallery.technet.microsoft.com/WADL-Generator-for-Siena-cc311ee9) makes the [generation of the WADL file](http://social.technet.microsoft.com/wiki/contents/articles/23838.project-siena-creating-a-wadl-configuration-file.aspx) extremely easy.

Here's a video that shows how a **developer** can define the relationship with Salesforce and create a WADL file that a **business user** can use within a Project Siena application.

<iframe src="http://channel9.msdn.com/Blogs/Wade-Wegner-s-Channel-9-Blog/Create-a-Windows-8-App-with-Salesforce-Data-Using-Project-Siena/player?h=405&w=720" style="height:405px;width:720px;" allowFullScreen frameBorder="0" scrolling="no" ></iframe>

(Watch in full screen for the best experience.)

I think this is a pretty compelling approach to enabling a business user to build a rich application that allows them to interact with different services.

This is only the tip of the iceburg. Take a look at the other [Project Siena tutorials](http://channel9.msdn.com/Blogs/Microsoft-Project-Siena) to get more ideas about how you can use Project Siena to build applications.