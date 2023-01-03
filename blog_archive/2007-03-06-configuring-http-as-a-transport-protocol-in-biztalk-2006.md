---
author: admin
categories:
- BizTalk Server
comments: true
date: "2007-03-06T03:01:22Z"
slug: configuring-http-as-a-transport-protocol-in-biztalk-2006
title: Configuring HTTP as a transport protocol in BizTalk 2006
wordpress_id: 110
---

I've been working on a project where we have two independent BizTalk 2006 clusters -- one at the headquarters, and one (actually, a few) at some a remote office. In order to send messages back and forth, we are using HTTP as the transport protocol.

For example, we first query an AS400/DB2 database (via the HOST adapters for BizTalk 2006) through a receive port/location. This is picked up by a subscribing send port, which posts it via HTTP to the remote office. That remote office has an receive port/location configured to listen on HTTP, via the BTSHTTPReceive.dll ISAPI extension. This is then picked up by a send port that then inserts the data into an MSSQL database.

Having set this up a few times now, I've found that I keep making the same mistakes. Consequently, here's my short-list of items to watch for when setting up HTTP for both send/receive in BizTalk 2006:

* Make sure your application pool is configured to run under the context of your BizTalk use account

* Confirm that your virtual directory has Scripts and Executables execute permissions

* Copy the file "BTSHTTPReceive.dll" into your virtual directory from the folder "C:Program FilesMicrosoft BizTalk Server 2006HttpReceive"

* Make sure to configure a "Web Service Extension" in IIS for the "BTSHTTPReceive.dll" file

* Remove anonymous access to your virtual directory, and use either Integrated or Basic authentication (preferrably Integrated)

* Make sure your send port utilizes the same security context, so that it can post to the site

* Confirm that your BizTalkUser account belongs to the BizTalk Server Administrators group

For your send port, configure your destination with a querystring value, like this: [http://localhost/transport/BTSHTTPReceive.dll?MessageType](http://localhost/transport/BTSHTTPReceive.dll?MessageType). I like to append the message type, but that's up to you. For your receive location, configure your virtual directory plus ISAPI extension to something like: /transport/BTSHTTPReceive.dll?MessageType.

That's all I can think of off the top of my head.

Best of luck!