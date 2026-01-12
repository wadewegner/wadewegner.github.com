---
title: "Silverlight: “Error Invoking Service” when calling a web service"
date: 2007-07-21T00:00:00-08:00
draft: false
description: "From the whiteboard to the keyboard"
---

I was writing a Silverlight application today, when I came across the following error when trying to invoke a web service method from my Silverlight project:

> “Error invoking service”

After digging around a little bit, I noticed the following above my web method:

> // To allow this Web Service to be called from script, using ASP.NET AJAX, uncomment the following line.
> // [System.Web.Script.Services.ScriptService]

“Ahh!” I said, as I uncommented the class attribute.

Unfortunately, when I tried to compile I was told:

> The type or namespace name ‘Script’ does not exist in the namespace System.Web' (are you missing an assembly reference?)

So, I went to “Add Reference”, added “System.Web.Extensions” to my project, and now it compiles! And, after testing my web service call from my Silverlight project, the data from the web services is returned to my Silverlight project as expected.

Nice!
