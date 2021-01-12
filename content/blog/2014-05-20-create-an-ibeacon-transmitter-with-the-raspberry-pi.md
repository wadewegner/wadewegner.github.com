---
categories:
- iBeacon
- BLE
- Raspberry Pi
date: "2014-05-20T00:00:00Z"
description: Ever want to transmit a BLE signal that you can detect from your iPhone
  or Android device? Here's how you can do it with the Raspberry PI.
title: Create an iBeacon Transmitter with the Raspberry Pi
---

iBeacon is the Apple trademark for a low-powered, low-cost transmitter that can notify nearby iOS 7 devices of its presence. It uses Bluetooth Low Energy (BLE), also called Bluetooth Smart, technology to transmit an advertisement that includes a universally unique identifier (UUID). Android devices can receive iBeacon advertisements and, fortunately for those of us who like to hack around, it doesn't take too much work to setup a Raspberry Pi to emit iBeacon advertisements.

![Raspberry Pi transmitting as an iBeacon](https://cloud.githubusercontent.com/assets/746259/3030868/149b9a9a-e046-11e3-9014-1138428103fe.jpg)

Why would you want to do this? Well, if the act of configuring a Raspberry Pi itself isn't enough of a reward, consider some of the following use cases:

* Host a digital scavenger hunt by placing iBeacons with different clues scattered around a conference.
* Customize retail offers based on your location in a store and proximity to certain products.
* Alert you while you sit at a bar enjoying your favorite craft beer when someone rides off with your bicycle outside.

I particularly enjoy the last use case. Simply tag your bag with a BLE transmitter and then, if it leaves the proximity/region, get alerted!

If you look around the Internet you'll find hundreds of additional use cases and ideas. The point is, this is cool technology with a lot of different applications and uses.

In this post we're going to look at how you can start to play around with this using that universal toy for the hacker: the Raspberry Pi.

## Required Hardware

Here's the hardware I use. You don't have to purchase from [Adafruit](http://www.adafruit.com), but to have the best results I'd recommend you try to get the same hardware I used; otherwise your mileage may vary.

* [Raspberry Pi Model B 512MB RAM](https://www.adafruit.com/products/998)
* [SD/MicroSD Memory Card (4 GB SDHC)](https://www.adafruit.com/products/102)
* [Bluetooth 4.0 USB Module (v2.1 Back-Compatible)](https://www.adafruit.com/products/1327)
* [5V 1A (1000mA) USB port power supply](https://www.adafruit.com/products/501)
* [USB cable - A/MicroB](https://www.adafruit.com/products/592)
* [Miniature WiFi (802.11b/g/n) Module](https://www.adafruit.com/products/814) (optional)
* Raspberry Pi Case (optional)

The WiFi and Pi Case are optional but I strongly encourage you to get them. They'll make your life a lot easier.

## Setup the Raspberry Pi as an iBeacon Transmitter

Over the past week I've had to setup and configure numerous Raspberry Pi's as iBeacons. Yes, there are a lot of tutorials, but sadly none worked perfectly for me. As so often happens when you're on the bleeding edge, things change and instructions stagnate. Additionally, I think some of the tutorials are simply wrong or poorly document the process.

Here are the steps I've taken to, repeatedly, get Raspberry Pi's setup as iBeacon transmitters. If you follow these instructions with the above hardware I'm reasonably confident you can get this up and running with no problems.

### Step 1: Download an Operating System Image

There are numerous OS images you can use with the Raspberry Pi. I chose to use the **Raspbian Debian Wheezy** image. The steps below work on the **2014-01-07** release.

Download the image from the official downloads page: [http://www.raspberrypi.org/downloads/](http://www.raspberrypi.org/downloads/)

### Step 2: Write the Image to the SD Card

There are [numerous tutorials](http://www.raspberrypi.org/documentation/installation/installing-images/README.md) available for this step.

If you're using a Mac I recommend Ray Viljoen's [Raspberry PI SD Installer OS X](https://github.com/RayViljoen/Raspberry-PI-SD-Installer-OS-X) script. Works great.

### Step 3: Connect to the Raspberry Pi

Again, you have numerous options for connecting to the device. I personally like to use the [Console Cable](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-5-using-a-console-cable?view=all) to connect directly from my Mac but you can also connect with Ethernet & SSH or connect a monitor and keyboard.

It's worth noting that your default user name is `pi` and password is `raspberry`.

### Step 4: Initial Device Configuration

Once you connect to the device you should first configure a few settings. Run `sudo raspi-config` and complete the following six steps.

1. Choose **Expand Filesystem**.
2. You should **Change User Password**.
3. Choose **Internationalisation Options** then **I1 Change Locale**. I removed the existing setting and chose **enUS.UTF-8 UTF-8**.
4. Choose **Internationalisation Options** then **I2 Change Timezone**. Set accordingly.
5. Choose **Advanced Options** then **A2 Hostname**. Choose something unique.
6.  Choose **Advanced Options** then **A4 SSH**. It's likely enabled already but you may as well make sure.

Reboot the Raspberry Pi (`sudo shutdown -r now`) and reconnect.

### Step 5: Setup WiFi (Optional)

While not absolutely necessary, I always setup WiFi on my Raspberry Pi so that it doesn't require Ethernet or a console cable for me to connect via SSH. When you have a lot of devices this is particularly important.

I use the [Miniature WifFi (802.11b/g/n) Module](https://www.adafruit.com/products/814).

You'll need to update your `interfaces` so that your WiFi connects to your wireless router. Run `sudo nano /etc/network/interfaces` (or whatever editor you want to use) and update accordingly.

	auto lo
	 
	iface lo inet loopback
	iface eth0 inet dhcp
	 
	allow-hotplug wlan0
	auto wlan0
	 
	iface wlan0 inet dhcp
	  wpa-ssid "ssid"
	  wpa-psk "password"

Be sure to set your own **ssid** and **password** values (and yes, capitalization matters).

If you're connected over Ethernet you'll likely want to restart with `sudo shutdown -r now` but if you're using your Console Cable you can simply restart the networking with `sudo /etc/init.d/networking restart`. If you then run `ifconfig` you should see you now have an IP address. At this point I typically start connecting using SSH.

### Step 6: Install Required Libraries

There are a number of open source libraries and tools used in order to send the iBeacon data from the Raspberry Pi. Install the following libraries:

	sudo apt-get install libusb-dev 
	sudo apt-get install libdbus-1-dev 
	sudo apt-get install libglib2.0-dev --fix-missing
	sudo apt-get install libudev-dev 
	sudo apt-get install libical-dev
	sudo apt-get install libreadline-dev

This only takes a few minutes.

### Step 7: Download, Make, & Install BlueZ Library

The BlueZ libraries provide support for the core Bluetooth layers and protocols. We have to download, make, and then install the libraries. Run the following commands sequentially:

	sudo mkdir bluez
	cd bluez
	sudo wget www.kernel.org/pub/linux/bluetooth/bluez-5.18.tar.gz
	sudo gunzip bluez-5.18.tar.gz
	sudo tar xvf bluez-5.18.tar
	cd bluez-5.18
	sudo ./configure --disable-systemd
	sudo make
	sudo make install
	sudo shutdown -r now

The `sudo make` command can take a long time to complete. Just let it run. As long as you've followed these instructions exactly as I've described it should complete without any problem.

![makestep](https://cloud.githubusercontent.com/assets/746259/3031070/26af888e-e048-11e3-85af-5b6fafa16335.png)

Congratulations. You've successfully setup your Raspberry Pi to act as an iBeacon transmitter. Of course, it's not yet transmitting a signal so let's set that up now.

## Setup the Raspberry Pi as an iBeacon Transmitter

At this point we need to start transmitting a signal. Here's where, depending on what you're looking to accomplish, you may decide to take a different path. For me, I want my Raspberry Pi iBeacon to do the following:

* Initialize and broadcast a signal.
* Advertise itself but don't allow connections.
* Broadcast a unique transmission but let me have more than one identifiable iBeacons.

The broadcast has three important identifiers:

* `proximityUUID`: a unique UUID that distinguishes your iBeacons from other iBeacons.
* `major`: used to group related sets of iBeacons.
* `minor`: used to identify a iBeacon within a group.

What's nice about the Raspberry Pi as a iBeacon is you can choose all these values yourself. Perhaps the easiest way to generate a UUID is to run the following command:

`python -c 'import sys,uuid;sys.stdout.write(uuid.uuid4().hex)'|pbcopy && pbpaste && echo`

You should get a value such as 636f3f8f64914bee95f7d8cc64a863b5 as the output. This is a 128-bit UUID that our app will use to look for our iBeacon. 

Okay, let's reconnect (if needed) to our Raspberry Pi and run the following commands:

	cd bluez/bluez-5.18
	sudo tools/hciconfig hci0 up
	sudo tools/hciconfig hci0 leadv 3
	sudo tools/hciconfig hci0 noscan

Now, run `tools/hciconfig` and confirm it's up and running. Your output should look similar to this:

	hci0:	Type: BR/EDR  Bus: USB
			BD Address: 00:1A:7D:DA:71:13  ACL MTU: 310:10  SCO MTU: 64:8
			UP RUNNING 
			RX bytes:1136 acl:0 sco:0 events:61 errors:0
			TX bytes:855 acl:0 sco:0 commands:61 errors:0

It's now time use the `hcitool` to configure the transmission to use our UUID. Additionally, for this initial device, we're going to set the `major` and `minor` values as 0 by attaching `00 00 00 00` to the end of our UUID. Finally, the last byte we'll add is the `RSSI` value of `C8`.

The command should look like the following:

	sudo tools/hcitool -i hci0 cmd 0x08 0x0008 1E 02 01 1A 1A FF 4C 00 02 15 63 6F 3F 8F 64 91 4B EE 95 F7 D8 CC 64 A8 63 B5 00 00 00 00 C8

You might be wondering about some of the other values. Well, the whole thing breaks down like this:

* Setup the add packet flags:

		1E
		02 		# Number of bytes that follow in first AD structure
		01  	# Flags AD type
		1A  	# Flags value 0x1A = 000011010  
				  bit 0 (OFF) LE Limited Discoverable Mode
				  bit 1 (ON) LE General Discoverable Mode
				  bit 2 (OFF) BR/EDR Not Supported
				  bit 3 (ON) Simultaneous LE and BR/EDR to Same Device Capable (controller)
				  bit 4 (ON) Simultaneous LE and BR/EDR to Same Device Capable (Host)
		1A  	# Number of bytes that follow in second (and last) AD structure
		
* Define vendor specific values:

		FF 		# Manufacturer specific data AD type
		4C 00 	# Company identifier code (0x004C == Apple)
		02 		# Byte 0 of iBeacon advertisement indicator
		15 		# Byte 1 of iBeacon advertisement indicator

* Our specific UUID values:

		63 6F 3F 8F 64 91 4B EE 95 F7 D8 CC 64 A8 63 B5 # our iBeacon proximity uuid
		00 00 	# Major 
		00 00 	# Minor 
		C8 00 	# Calibrated Tx power

Run the command. Congrats, you're broadcasting!

## Detect Your iBeacon Signal

In a later post I'll show you how to write your own iOS application to detect iBeacon signals. For now, let's grab a free application to find our signal.

For an iOS device I recommend **Locate iBeacon**. It's easy to use. I don't have an Android, but I'm sure there are many out there.

When you open the app first select **iBeacon Transmitter** and hit the **+** symbol/button to configure our Raspberry Pi iBeacon.

* `Name`: Choose a unique name
* `Proximity UUID`: 636f3f8f-6491-4bee-95f7-d8cc64a863b5 (must he a hex version of our UUID)
* `Major`: 0
* `Minor`: 0
* `Power`: (leave blank)

Save these values, then select the **Locate iBeacons** button. You should see your iBeacon!

<table style="margin-bottom:10px;"><tr><td style="padding-right:5px;">
<img src="https://cloud.githubusercontent.com/assets/746259/3030919/956ab73c-e046-11e3-9682-c00043149c67.png" alt="Authorize" style="border-style: solid;border-width:1px;border-color:#767676;">
</td><td style="padding-right:5px;">
<img src="https://cloud.githubusercontent.com/assets/746259/3030922/98ed92e4-e046-11e3-848b-e9cd98157e45.png" alt="Authorize" style="border-style: solid;border-width:1px;border-color:#767676;">
</td><td>
<img src="https://cloud.githubusercontent.com/assets/746259/3030923/9c220620-e046-11e3-816f-d05624b5e676.png" alt="Authorize" style="border-style: solid;border-width:1px;border-color:#767676;">
</td></tr></table>

Of course, if you have more than one Raspberry Pi, you can get them both to broadcast. Follow the instructions above except change the `mino` value to 1 (01 00 in hex):

	sudo tools/hcitool -i hci0 cmd 0x08 0x0008 1E 02 01 1A 1A FF 4C 00 02 15 63 6F 3F 8F 64 91 4B EE 95 F7 D8 CC 64 A8 63 B5 00 00 01 00 C8

Pretty awesome!

I hope you've found this article on configuring a Raspberry Pi as an iBeacon transmitter useful! If you have questions or issues please feel free to leave a comment.