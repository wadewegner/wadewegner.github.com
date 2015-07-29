---
author: admin
comments: true
date: 2007-08-16 18:40:40+00:00
layout: post
slug: systemnetwebexception-when-issuing-more-than-two-concurrent-webrequests
title: System.Net.WebException when issuing more than two concurrent WebRequest's
wordpress_id: 45
categories:
- .NET 2.0
- BizTalk Server
---

I got caught up yesterday with a problem I should have recognized right away, but for whatever reason I didn't put the pieces together until it was pointed out by one of my co-workers.

I was writing a BizTalk Server 2006 orchestration that, amongst other things, sends a request to a remote server. The remote server has an application installed that accesses these requests and initiates a few local (and unimportant) processes. Information is passed to this application via the URL which is unique with each request and constructed by the orchestration.

Rather than use a dynamic one-way send port with the HTTP adapter, I decided to create a .NET helper project (a C# class library) and create a static method that allows me to pass in the URL and wait for a response. The contents of the response itself is unimportant; all I require is a successful response.

I created the following class in my helper project (simplified for clarity):

	using System;
	using System.Collections.Generic; 
	using System.Text; 
	using System.Net; 
	
	namespace Messaging.Helper 
	{
		[Serializable] 
		public class Statics 
		{
			public void CallUrl(string url)
			{
				WebRequest request = WebRequest.Create(url);
				request.Method = "GET";
				request.GetResponse();
			}
		}
	}

As you can see, the static method uses the System.Net.WebRequest class to issue a request to the specified URL. To test this method, I created a command-line application that iterated 20 or so times and issued requests against the remote server. The code was similar to the following (simplified for clarity):

	using System;
	using System.Collections.Generic;
	using System.Text;
	using System.Net;
	using Messaging.Helper;
	
	namespace Messaging.ConsoleApp
	{
		class Program
		{
			static void Main(string[] args)
			{
				for (int i = 0; i < 20; i++)
				{
					Statics.CallUrl("http://www.someurl.com/");
				}
			}
		}
	}

I immediately noticed that this code failed after it issued two concurrent requests. No matter how many times I ran it, it always was successful the first two times and then timed-out and threw an error message similar to the following:

    System.Net.WebException was unhandled
    Message="The operation has timed out"
    Source="System"
    StackTrace:
      at System.Net.HttpWebRequest.GetResponse()
      at ConsoleApp.Program.Helper.CallUrl() in D:TestConsoleAppProgram.cs:line 41
      at ConsoleApp.Program.Main(String[] args) in D:TestConsoleAppProgram.cs:line 20
      at System.AppDomain.nExecuteAssembly(Assembly assembly, String[] args)
      at System.AppDomain.ExecuteAssembly(String assemblyFile, Evidence assemblySecurity,
         String[] args)
      at Microsoft.VisualStudio.HostingProcess.HostProc.RunUsersAssembly()
      at System.Threading.ThreadHelper.ThreadStart_Context(Object state)
      at System.Threading.ExecutionContext.Run(ExecutionContext executionContext,
         ContextCallback callback, Object state)
      at System.Threading.ThreadHelper.ThreadStart()

Yes, I should have immediately picked up on the fact that it succeeded the first two times but then consistently failed on each additional request. Instead, I tried all kinds of different scenarios (non-static methods, multi-threaded calls, etc.). Fortunately, a co-worker stopped by and reminded me that, by design, there is a limit on the number of connections you can have to any given server. By default, this **limitation is set to two!**

As soon as he said that I remembered the limitation (I guess I still have a few neurons that occasionally fire). Immediately I created an application configuration file for my command-line application and added the following (of course, I had to look-up the exact syntax):

<span class="lnum">   1:  </span><span class="kwrd"><?</span><span class="html">xml</span> <span class="attr">version</span><span class="kwrd">="1.0"</span> <span class="attr">encoding</span><span class="kwrd">="utf-8"</span> ?<span class="kwrd">></span>

	<?xml version="1.0" encoding="utf-8" ?>
	<configuration>
		<system.net>
			<connectionManagement>
			<add address="*" maxconnection="100" />
			</connectionManagement>
		</system.net>
	</configuration>

This allows for up to 100 simultaneous connections to any server. When I ran my command-line application again it worked perfectly -- 20 requests were issued to the URL.

After poking around a little bit, I found the actual specification in the HTTP/1.1 protocol that dictates this limitation. See [Hypertext Transfer Protocol - Section 8.1.4](http://www.w3.org/Protocols/rfc2616/rfc2616-sec8.html#sec8.1.4):


> "Clients that use persistent connections SHOULD limit the number of simultaneous connections that they maintain to a given server. A single-user client SHOULD NOT maintain more than 2 connections with any server or proxy. A proxy SHOULD use up to 2*N connections to another server or proxy, where N is the number of simultaneously active users. These guidelines are intended to improve HTTP response times and avoid congestion."

Ah, this explains why Internet Explorer only allows me to download two files at the same time!

Now, in order to allow my BizTalk orchestration to proper issue more than two concurrent connections (remember, it too is just calling .NET code), I needed to modify the BTSNTSvc.exe.config file that services the BizTalk host instances. This file is found in the BizTalk Server directory under Program Files:

[![BTSNTSvc.exe.config](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/e8c3b063ce21_A687/image_thumb.png)](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/e8c3b063ce21_A687/image.png)

Simply add the <system.net> ... </system.net> XML to the configuration file and then restart your host instances. This will allow you to issue more than two concurrent requests to any given web server.

I hope this helps save you some time you would have otherwise lost!