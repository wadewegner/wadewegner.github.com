---
author: admin
categories:
- Virtual Server 2005
comments: true
date: "2006-11-30T22:09:56Z"
slug: setting-up-networking-over-wireless-with-virtual-server-2005-r2
title: Setting up networking over wireless with Virtual Server 2005 R2
wordpress_id: 117
---

I recently had to configure a virtual machine on Virtual Server 2005 R2 for Commerce Server 2007. While setting it up, I only had access to the Internet via my wireless adapter. Consequently, I had to do the following in order to get the virtual machine to connect to the Internet:

1. Install the Virtual Machine Additions.

2. On the host machine, go to "Add Hardware" in Control Panel. Click next, wait for it to search for hardware, then select "Yes" on the screen that follows. Scroll all the way down to the bottom of the "Installed Hardware" list and select "Add a new hardware device". Choose "Install the hardware that I manually select from a list (Advanced)". Choose "Network adapters" on the screen that follows.

3. Now you're presented with a list of network card manufacturers and network adapters. Choose "Microsoft" as the manufacturer and choose "Microsoft Loopback Adapter" as the network card. Click next and complete the wizard.

4. You should now see an additional network connection in the Network Connections control panel. It will be called something to the effect of "Local Area Connection 2", but you can determine which adapter is the one you just added either using the ToolTip or the details view (it should show up as Microsoft Loopback Adapter).

5. Now you need to share your internet connection with the network adapter you just added. Right click your real network card and click Properties. Go to the Advanced tab and check "Allow other network users to connect through this computer's internet connection". Choose your Loopback Adapter in the dropdown list, if applicable. Then click OK.

6. Make sure your virtual machine is shutdown. 

7. Create a new "Virtual Network". Choose your Loopback Adapter. Rename the virtual network to "Loopback Adapter".

8. Change the "Network adapters" for your virtual server, and select the new "Loopback Adapter".

9. Click OK and start the virtual machine up. You shouldn't have to do any work inside the virtual machine, so just wait for a while (it may take a while for it to get a connection).

You should now be able to connect to the internet!