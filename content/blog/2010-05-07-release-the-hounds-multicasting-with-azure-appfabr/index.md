---
title: "Release the hounds - Multicasting with Azure AppFabric"
date: 2010-05-07T00:00:00-08:00
draft: false
description: "From the whiteboard to the keyboard"
---

On an email thread today, someone was looking for suggestions on how to start a job simultaneously across multiple worker roles running in Windows Azure. For example, image you have ten worker roles already running and, through the command of an admin or user, you want to “release the hounds!”

Definitely an interesting scenario, and many different ways to approach it. Initial ideas and thoughts centered around using Windows Azure storage tables or blobs – in fact, [Steve Marx](http://blog.smarx.com/) quickly threw out some pseudo code highlighting a reasonable way to approach the problem:

```
1: while (blob.DownloadText() != “RELEASE THE HOUNDS!”)
2: Thread.Sleep(TimeSpan.FromSeconds(1));
3: // do the actual work
```

Then to release:

```
1: blob.UploadText(“RELEASE THE HOUNDS!”);
```

You could definitely take this approach and have success.

Of course, to me this scenario screamed multicasting with [*NetEventRelayBinding*](http://msdn.microsoft.com/en-us/library/microsoft.servicebus.neteventrelaybinding.aspx).

*NetEventRelayBinding* supports multiple listeners on the same URI, which means that you can have 1 or 1000 worker roles in Windows Azure all listening to the same URI – this gives you the ability to push out events to all listeners, as any message sent by a client gets distributed to all the listeners.

Clemens Vasters [sums *NetEventRelayBinding* it up nicely on his blog](http://vasters.com/clemensv/PermaLink,guid,92d78bee-2cfd-4a29-95ab-c5abb9b905e7.aspx):

> The *NetEventRelayBinding* doesn’t have an exact counterpart in the standard bindings. This binding provides access to the multicast publish/subscribe capability in the Relay. Using this binding, clients act as event publishers and listeners act as subscribers. An event-topic is represented by an agreed-upon name in the naming system. There can be any number of publishers and any number of subscribers that use the respective named rendezvous point in the Relay. Listeners can subscribe independent of whether a publisher currently maintains an open connection and publishers can publish messages irrespective of how many listeners are currently active – including zero. The result is a very easy to use lightweight one-way publish/subscribe event distribution mechanism that doesn’t require any particular setup or management.

So, the architecture might look something like this:

![Apologies for the crappy graphic](https://wadewegner.blob.core.windows.net/wordpress/2010/05/image1.png)

In this scenario, an admin sitting on a laptop can send a message to the Service Bus, which in turn relays the message to all the listeners. When the worker roles receive the message they will “release the hounds” and process whatever it is they need to process.

> Note: this approach is just as valid for listeners that don’t reside in Windows Azure. For example, if you have an application that is distributed across PCs and you want to send every client a message (without implementing some form of polling) this is the perfect approach.

A few comments on the code:

- I wrote this using Visual Studio 2010 RTM. Your mileage may vary.
- Make sure you have the [Windows Azure SDK](http://www.microsoft.com/downloads/details.aspx?familyid=DBA6A576-468D-4EF6-877E-B14E3C865D3A&displaylang=en) and the [Windows Azure AppFabric SDK](http://go.microsoft.com/fwlink/?LinkID=129448).
- The first thing you’re going to want to do is search on **YOURSERVICENAME** and **YOURISSUERSECRET** and replace with your own values.
- It’s initially configured to run locally. Just hit F5.
- When you run locally, three things will launch:

  1. The local development fabric, with two worker roles.
  2. A Windows Forms application with a big button (hey, it’s better than a console window!).
  3. A console window that displays traces from all your worker roles. This is especially useful for getting information from your worker roles once you’ve deployed to Windows Azure. I’ll blog on this another time.
- When you eventually deploy to Windows Azure, but sure to uncomment the  and  sections in the App.config, as the Windows Azure AppFabric SDK is not installed in Windows Azure, and it won’t understand ***NetEventRelayBinding***.

I personally think this is a pretty neat solution, and can enable a lot of advanced scenarios. I’d love to hear your feedback and comments.
