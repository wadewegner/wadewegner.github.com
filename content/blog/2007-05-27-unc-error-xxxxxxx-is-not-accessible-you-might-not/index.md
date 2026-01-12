---
title: "UNC error: XXXXXXX is not accessible. You might not have permission to use this network resource. Access is denied."
date: 2007-05-27T00:00:00-08:00
draft: false
description: "From the whiteboard to the keyboard"
---

Have you ever received the following error when you tried to access a UNC path?

![](https://wadewegner.blob.core.windows.net/wordpress/content/binary/UNCError.gif)

```
_XXXXXXX_ is not accessible. You might not have permission to use this network resource. Access is denied.
```

I found that if I logged into some of our production servers, I was unable to connect to a UNC path on the file server (a different machine), even though I was able to access these resources locally. The only difference I could find was that in order to access the production servers I had to use a different login than my local login (e.g. my local login had acess to the file share, but not the login I used for the production boxes). And, as I do not have administrative rights in this domain (nor were any administrators available to assist) I couldn’t change the rights to the file server to give my production user access.

So, I needed to find a way to access the file share as my local account. Unfortunately, I couldn’t log into the production server with my local account (a good thing, actually), and I couldn’t get Explorer.exe to runas my local account while on the server.

Then I decided to try and mount the share as a driver. Here are the steps I took:

1. Opened up My Computer, the click Tools –> Map Network Drive …
2. For the folder, specify your UNC share. Then, instead of clicking finish, click the “Connect using a different user name”.

   ![](https://wadewegner.blob.core.windows.net/wordpress/content/binary/UNCError2.gif)
3. In the Connect As screen, specify your local account that has the ability to access the UNC share.

   ![](https://wadewegner.blob.core.windows.net/wordpress/content/binary/UNCError3.gif)
4. Click OK, then Finish.

These steps mount the UNC share using your local account. As a result, I was able to access the file server on the production server, which I previously hadn’t been able to do.

If you find yourself in a similar situation, these steps may provide you with a temporary work around.

I hope this helps!
