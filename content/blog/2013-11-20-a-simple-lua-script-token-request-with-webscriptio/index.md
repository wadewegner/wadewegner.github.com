---
title: "A Simple Lua Script Token Request with Webscript.io"
date: 2013-11-20T00:00:00-08:00
draft: false
description: "From the whiteboard to the keyboard"
---

Yesterday I spent some time writing a [Python token request script against the Force.com APIs](http://www.wadewegner.com/2013/11/forcecom-token-requests-with-python/). Today I thought I’d share a short code snippet for doing the same thing but this time using [Lua](http://www.lua.org/).

Why Lua? Why not!

No, I have a good reason for getting this to work in Lua. I’m a big fan of [Webscript.io](http://webscript.io) which provides a tremendously simple way to create web APIs. Webscripts are all written in Lua.

The first step to using webscripts against the Force.com APIs is to make a token request. So, let’s do it.

Before you give this a try be sure to read my previous post on [making a Force.com token request](http://www.wadewegner.com/2013/11/forcecom-token-requests-with-python/) and review the section where I discuss the **secret key**. You will need this in order to authenticate against the Force.com API.

Without further ado, here’s the code.

Lua is such a cute language, isn’t it?

Okay, so where should you run this? Why don’t you give [Webscript.io](http://webscript.io) a try.

You can use the code I wrote above without any modifications (of course, you need to plug in your own values). You might also want to add the following line at the end to log the **access\_token**.

```
log(tokenResponse.access_token)
```

To execute your webscript simply browse to the generated endpoint. After it (quickly) executes, simply review the latest **Request Log**. You should see a log similar to the following:

![Webscript log](http://wadewegner.blob.core.windows.net/wordpress/2013-11-20-WebscriptLog.png)

That’s it!

You can see the the image above that we get three things from Webscript.io:

1. A detailed log of the HTTP request the webscript made.
2. A detailed log of the HTTP response the webscript received.
3. The access\_token output.

Fantastic!

I love how easy it is to try things out with webscripts. Once you give it a try I’m sure you will too.
