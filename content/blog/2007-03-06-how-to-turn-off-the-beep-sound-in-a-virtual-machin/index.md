---
title: "How to turn off the beep sound in a virtual machine in Virtual Server 2005"
date: 2007-03-06T00:00:00-08:00
draft: false
description: "From the whiteboard to the keyboard"
---

Since I use virtual machines (VMs) for development, I often find myself having to turn off the beep service on my VMs, or risk the rather of my neighbors. There are a couple of ways to do this:

1. Stop the beep on/off for the particular session. Obviously, the problem with this is that once you restart your machine, the beep service will restart. Nevertheless, you can type the the following at the command line:

   ```
    net stop beep
   ```

   and …

   ```
    net start beep
   ```
2. Turn off the beep service. This will turn it off even after it is rebooted (make sure you get the spacing exact – it actually matters!):

   ```
    sc config beep start= disabled
   ```

   **Note:** I think that if you turn off the beep service it won’t take effect until after a reboot. Consequently, I recommend doing using the net stop beep after you turn off the service.

I believe that you can also accomplish this by modifying the registry, but I’ve never had to do that (these methods always work), so you’ll have to look elsewhere to find out more.

Best of luck!
