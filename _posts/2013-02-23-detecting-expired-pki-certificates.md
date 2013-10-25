---
author: admin
comments: true
date: 2013-02-23 19:06:27+00:00
layout: post
slug: detecting-expired-pki-certificates
title: Detecting Expired PKI Certificates
wordpress_id: 1887
categories:
- C#
- Certificates
- PKI
- SSL
- Windows Azure
---

Yesterday Windows Azure experienced a worldwide disruption in many services due to an expired PKI certificate for Windows Azure storage. Mary Jo Foley’s article [Windows Azure storage issue: Expired HTTPS certificate possibly at fault](http://www.zdnet.com/windows-azure-storage-issue-expired-https-certificate-possibly-at-fault-7000011705/) provides the best coverage of the event as it unfolded. You can also take a look at a few threads on the [Windows Azure forum](http://social.msdn.microsoft.com/Forums/en-US/windowsazuredata/thread/751c85c5-b3b5-43ba-9d5b-770472ad79e1) and [Stack Overflow](http://stackoverflow.com/questions/15033020/windows-azure-storage-certificate-expired) that provide a lot of commentary on the event. The effects of this disruption rippled through most of the other Windows Azure services. Even if you modified your application to use HTTP instead of HTTPS it’s likely you still had issues given that the rest of the platform was crippled by the expired certificate.

It’s disappointing this happened but highlights a pretty common situation. This has nothing to do with the merits of the Windows Azure storage service or any other parts of the platform – this is an operations management issue, plain and simple. The irony is that, as a number of folks [including Lars Wilhelmsen](https://twitter.com/larsw/status/305375364534902784) have pointed out, there are tools like Microsoft SCOM that provide a Certificate Management Pack that can notify operations of expiring certificates. I can’t imagine the operations team at Windows Azure doesn’t use some kind of tool to manage expiring certificates.

As a developer, I found myself curious to see just how hard it is to determine the expiration of a certificate by checking the URI. Turns out, it’s pretty simple by using System.Net.ServicePoint which provides connection management for HTTP/S connections.

	private string GetSSLExpiryDate()
	{
    	string url = "https://www.aditicloud.com/";
    	var request = WebRequest.Create(url) as HttpWebRequest;
    	var response = request.GetResponse();

    	if (request.ServicePoint.Certificate != null)
    	{
        	return request.ServicePoint.Certificate.GetExpirationDateString();
    	}
    	else
    	{
        	return string.Empty;
    	}
	}

Pretty simple. What’s hard is the practice of managing and tracking these sorts of things.

I would expect that Microsoft will ensure that this kind of problem never happens again. It’s embarrassing yet solvable. Yet it exposes an issue that most of us will also have to account for – expiring certificates. If it can happen to Microsoft, it can happen to us too.