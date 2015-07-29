---
author: admin
comments: true
date: 2007-06-14 22:09:41+00:00
layout: post
slug: do-you-yahoo-no-cant-say-that-i-do
title: Do you Yahoo!?  No, can't say that I do ...
wordpress_id: 72
categories:
- Rant
---

... and here's a good reason why:

![](https://wadewegner.blob.core.windows.net/wordpress/content/binary/Yahoo1.gif)

Every time I attempt to search, I get the error "Sorry, Unable to process request at this time -- error 999." This has been occurring for at least the past 30 minutes. However, I pinged a friend of mine, [Brad Younge](http://byounge.spaces.live.com/), and he can search just fine. Quite strange!

A quick search via [Google](http://www.google.com/search?sourceid=navclient&ie=UTF-8&rlz=1T4GGIG_enUS227US227&q=Sorry%2c+Unable+to+process+request+at+this+time+%2d%2d+error+999%2e) (and no, I've never received an "unable to process request at this time" error from Google) shows that a LOT of people have this problem.

So, I can safely say that no, I don't Yahoo!.

Anyone know why this happens?

**[Update]**

I found an excellent description of this error on [Murray's Binary Bit Bucket](http://www.murraymoffatt.com/software-problem-0011.html). It appears that the most common reason for this error is some sort of bandwidth limiting that Yahoo! put in place on their servers. The idea is to prevent both [DoS (Denial of Service) attacks](http://en.wikipedia.org/wiki/Denial-of-service_attack) and automated processes that would pummel their servers with requests.

Wonder why it targeted me?

**[Update 2]**

About an hour after posting I can now search again. We'll see if I can purposely cause this to occur again!!

**[Update 3]**

Yep, I was able to purposely cause Yahoo! to prevent my from issuing searches. I wrote a little application that does the following:

	string url = "http://search.yahoo.com/search?p=wade+wegner&fr=yfp-t-501&toggle=1&cop=mss&ei=UTF-8";  
	
	for (int i = 0; i < 1000; i++)  
	{  
		HttpWebRequest request = (HttpWebRequest)WebRequest.Create(url);  
		request.Method = "GET";  
		request.AllowAutoRedirect = false;  
		request.Proxy = WebProxy.GetDefaultProxy();  
		request.Proxy.Credentials = System.Net.CredentialCache.DefaultCredentials;  
		WebResponse response = request.GetResponse();   
	}

So, evidently Yahoo! doesn't appreciate it if you make 1000 calls in a short period of time. Interesting!