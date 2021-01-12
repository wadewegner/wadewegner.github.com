---
author: admin
categories:
- Tips
- Tools
comments: true
date: "2008-09-09T21:23:57Z"
slug: installing-windows-from-a-bootable-usb-drive
title: Installing Windows From a Bootable USB Drive
wordpress_id: 9
---

I find myself doing this over and over again, so I figured it would be worthwhile to post. Below you’ll find a really useful way for installing a Windows O/S from a bootable USB device. I am particularly dependent on USB drives, as I have a Lenovo X61 Tablet that doesn’t have a CD/DVD-ROM (unless I’m docked, but I’m hardly in the office).  

A couple of notes:  

  * This assumes you’re running Windows Vista or Windows Server 2008  
  * I’ve tested by installing the following O/S’s: Windows XP SP3, Windows Vista, and Windows Server 2008  
  * You must have a machine capable of booting from a USB drive

Without further ado, here are the steps:  

  1. Open an elevated command prompt.  
  2. You must make your USB drive bootable. Type the following in the command prompt:   
  
diskpart  
list disk (FIND YOUR USB DISK)  
select disk 1 (OR WHATEVER YOUR DISK NUMBER IS)  
clean  
create partition primary  
select partition 1  
active  
format fs=NTFS  
assign  
exit   

  3. Mount your Windows Server ISO (or unpack it) and copy the contents of the CD/DVD onto your USB. Make sure you get all files, including hidden ones.

You should now be able to use this USB drive to install a new O/S. Good luck!
