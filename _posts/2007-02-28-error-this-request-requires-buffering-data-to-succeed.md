---
author: admin
comments: true
date: 2007-02-28 16:39:32+00:00
layout: post
slug: error-this-request-requires-buffering-data-to-succeed
title: Error "This request requires buffering data to succeed."
wordpress_id: 114
categories:
- BizTalk Server
---

If you get the following error with an HTTP send port in BizTalk 2006, you may have an authentication issue:

	A message send to adapter "HTTP" on send port "SendPort1" with URI http://localhost/vd/BTSHTTPReceive.dll?QueryString is suspended.
 
	Error details: This request requires buffering data to succeed.

Check the following to see if it's the source of the problem:

1. Go to the properties of your send port.

2. Click on "Configure..." next to your HTTP type.

3. Choose the "Authentication" tab.

4. Make sure your authentication is set correctly (i.e. valid username and password).

It's best to not use anonymous authentication. Basic is good for development and testing, but the preferred method is either Kerberos or Single Sign-On.

Best of luck!