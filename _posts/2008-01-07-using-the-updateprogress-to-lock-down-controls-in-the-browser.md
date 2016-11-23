---
author: admin
comments: true
date: 2008-01-07 03:51:57+00:00
layout: post
slug: using-the-updateprogress-to-lock-down-controls-in-the-browser
title: Using the UpdateProgress to lock down controls in the browser
wordpress_id: 17
categories:
- .NET 3.5
- AJAX
- ASP.NET
---

Finally, back to some writing some code! Between writing my book and some of my more recent projects, I haven't had a chance to write a lot of code.

I'm currently porting a web-based timesheet application to an ASP.NET AJAX Futures Web Application with the .NET Framework 3.5. In writing the application, I'm trying to adhere to a number of practices:

1. Separation of concerns 

2. Test driven development (TDD) to support changing business requirements 

3. Intuitive and fool proof user interface 
 
It's because of #3 that I am going with an ASP.NET AJAX application.

ASP.NET AJAX uses an UpdatePanel control to support partial-page updates; essentially, controls contained within the ContentTemplate property can be updated without refreshing the entire page. The ASP.NET WebForm also contains a ScriptManager control which allows the UpdatePanel to participate in partial-page updates without requiring custom client script code.

The following code shows these controls working together:

	<asp:ScriptManager ID="ScriptManager1" runat="server" />

	<div id="main">
		<asp:UpdatePanel ID="UpdatePanel1" runat="server">
			<ContentTemplate>
				<asp:Button ID="Button1" runat="server" Text="Button" OnClick="Button1_Click" />
			</ContentTemplate>
		</asp:UpdatePanel>
	</div>

Pretty simple.

When the button is clicked, the page does not appear to postback; instead, the button click invokes an asynchronous postback in which the page updates are limited to the controls in the UpdatePanel. The server sends back HTML markup for only the affected elements to the browser. Within the browser, the client PageRequestManager class performs Document Object Model (DOM) manipulation to replace existing HTML with updated markup.

Simple, but very cool stuff! What's nice with ASP.NET 3.5 and Visual Studio 2008 is that you don't have to install anything else in order to build ASP.NET AJAX applications -- it's already built in!

Now, if it takes awhile for the server to process the postback (e.g. complex rules or badly written code <grin>), the user may not realize that the server is processing the request. This can lead to all kinds of issues with users that are not savvy or familiar with web applications (multiple clicks, moving off the page, etc.). Consequently, I want to tell the user that the server is processing the request _and_ disable the controls on the page. Let's break this down into two steps: show a message, and disable the user's interaction with the controls.

You can use the UpdateProgress control alone with the UpdatePanel to provide a message to the user during the postback. This is very simple -- put the UpdateProgress control within the UpdatePanel like so:

	<asp:UpdateProgress ID="UpdateProgress1" runat="server">
		<ProgressTemplate>
			Update in progress. Please wait ...
		</ProgressTemplate>
	</asp:UpdateProgress>

This will display the "Update in progress. Please wait ..." message to the the user while the server is processing the request. However, it doesn't prevent the user from continuing to interact with the web application. To provide this type of functionality, we will use the PageRequestManager to invoke some JavaScript while also using CSS and DHTML to lock down the UI.

First, we'll add a little more to our UpdateProgress control:
	
	<ProgressTemplate>
		<div id="blur" />
		<div id="progress">
			Update in progress. Please wait ...
		</div>
	</ProgressTemplate>

We'll use the "blur" and "progress" controls to _overlay _the controls in the UI while also providing a message to the user. To provide the functionality we require, we need to use the following CSS elements:

	#blur
	{
		width: 100%;
		background-color: black;
		moz-opacity: 0.5;
		khtml-opacity: .5;
		opacity: .5;
		filter: alpha(opacity=50);
		z-index: 120;
		height: 100%;
		position: absolute;
		top: 0;
		left: 0;
	}
	#progress
	{
		z-index: 200;
		background-color: White;
		position: absolute;
		top: 0pt;
		left: 0pt;
		border: solid 1px black;
		padding: 5px 5px 5px 5px;
		text-align: center;
	}

The purpose of the "blur" control is to provide a tag that lays over everything in the browser. Since the opacity is 0.5 (and 50), it appears gray while allowing the user to continue to see the controls behind it. However, since the "blur" control exists between the user and the other controls, the user cannot interact with any other controls.

Now, the tricky thing is that we need to run a JavaScript function to manipulate the size and positioning of the "blur" and "progress" controls; essentially, we want the "blur" to cover 100% of the browser window, and the "progress" to sit in a box in the center. The key part is hooking in a JavaScript call to the initialization of the PageRequestManager request. To do this, you can add the following JavaScript _after_ the ScriptManager:
	
	<asp:ScriptManager ID="ScriptManager1" runat="server" />
	<script language="javascript" type="text/javascript">
		Sys.WebForms.PageRequestManager.getInstance().add_initializeRequest(
			
			function() {
				// code here
			}
		)
	</script>
		
This JavaScript allows you to add JavaScript code that will process during the initialization of the postback -- a perfect place for us to grab the information we need. Here's what the complete code looks like:

	Sys.WebForms.PageRequestManager.getInstance().add_initializeRequest(
		function () {
			if (document.getElementById) {
				var progress = document.getElementById('progress');
				var blur = document.getElementById('blur');
				
				progress.style.width = '300px';
				progress.style.height = '30px';
				blur.style.height = document.documentElement.clientHeight;
				progress.style.top = document.documentElement.clientHeight/3 - progress.style.height.replace('px','')/2 + 'px';
				progress.style.left = document.body.offsetWidth/2 - progress.style.width.replace('px','')/2 + 'px';
			}
		}
	)

The exact implementation isn't that important -- what I think is important is that you can hook into the PageRequestManager to invoke some JavaScript.

Now, to help test, you can use System.Threading to make the button click sleep for two seconds:

	protected void Button1_Click(object sender, EventArgs e)
	{
		System.Threading.Thread.Sleep(2000);
	}

Okay, now to test. Here's the page prior to the postback:

![image](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/UsingtheUpdateProgresstodisablepagecontr_11308/image_10.png)

Once the page is clicked, the experience changes to the following while the server is processing the request:

![image](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/UsingtheUpdateProgresstodisablepagecontr_11308/image_18.png)

And yes, this works in Firefox too:

![image](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/UsingtheUpdateProgresstodisablepagecontr_11308/image_16.png)

As I mentioned before, the "blur" and "progress" controls act as a screen over all the other controls, and since they are part of the ProgressTemplate these controls they are only active during server processing. You can confirm this by removing the Sleep method -- you won't even see you the "blur" and "progress" controls, as they are not needed.

This is only one little part of what I'm trying to do with this new application -- hopefully you find it interesting! Of course, I hope you can make it all look a little prettier than what I threw together for this post!

I hope this helps!