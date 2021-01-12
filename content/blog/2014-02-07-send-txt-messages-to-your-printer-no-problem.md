---
categories:
- IoT
- printer
- Heroku
- Twilio
- Raspberry Pi
- python
date: "2014-02-07T00:00:00Z"
description: How do you send a text message from your phone and have it print on a
  thermal printer that's sitting on your kitchen counter? With the Internet of Things,
  and services like Heroku and Twilio coupled with hardware like the Raspberry Pi,
  anything's possible.
tags: []
title: Send a Text Message to Your IoT Thermal Printer? No Problem. (And Print Your
  Lunch Menu Too!)
---

How do you send a text message from your phone and have it print on a thermal printer that's sitting on your kitchen counter?

![iot-beer](https://f.cloud.github.com/assets/746259/2115102/87aa3efe-9048-11e3-9222-b32b825909ef.jpg)

Hang on, we'll get to that shortly!

Shortly after Dreamforce last year I was turned on to Adafruit Industries by my teammate [Reid Carlberg](https://twitter.com/ReidCarlberg). I've since built a number of cool little gadgets, including the [Brain Machine](https://www.adafruit.com/products/287) and the [TV B Gone](http://www.adafruit.com/products/73). Both projects were a ton of fun and a great way to get started with soldering and simple circuitry. It's been great fun having the TV B Gone when I travel. There's nothing more satisfying than turning off loud, annoying TVs (invariably on CNN) at the airport when you're trying to read or work.

I don't recall exactly how/where I first heard of it, but I was immediately drawn to the [Internet of Things Printer for Raspberry Pi](http://learn.adafruit.com/pi-thermal-printer). I think it's the combination of a number of things that got me interested:

- A small **thermal printer** that doesn't require an ink cartridge.
- It's powered by a **Raspberry Pi**, providing lots of opportunities for coding.
- A miniature USB **WiFi** allowing it to run almost anywhere.

And of course, having three young kids, I thought of so many different ways the printer could be fun and useful (although, to be 100% honest, I haven't found all that much utility in it yet, but it sure has been fun).

The construction of the printer was fun. It took me a couple of days, but I'm slow.

<div class="row">
	<div class="col-sm-6" style="padding-right:0px;">
		<img border="0" alt="" src="https://f.cloud.github.com/assets/746259/2114554/da19da0a-903e-11e3-9977-7cc67eb248da.jpg">
	</div>
	<div class="col-sm-6" style="padding-right:0px;">
		<img border="0" alt="" src="https://f.cloud.github.com/assets/746259/2114556/dd0e28ba-903e-11e3-808c-eee837a68be6.jpg">
	</div>
</div>
<p />

Simply follow the [Adafruit tutorial](http://learn.adafruit.com/pi-thermal-printer). It's fantastic and almost impossible to go wrong if you follow the directions.

The Adafruit tutorials also show you how setup some Python scripts to provide simple functions on the IoT printer, including:

- **Daily weather scripts**. Pretty useful at providing a local forecast.
- **Twitter script**. Prints everytime someone tweets you. I immediately disabled this script.
- **Daily Sudoku puzzle**. I love Sudoku and have kept this one running.

But, honestly, beyond this it's up to you to figure out interesting uses for the printer.

Not a problem for me. I had two ideas:

1. Figure out a way to let people send messages to the printer though simple text (SMS) messages.

2. Print out my kids daily school lunch menu. This had always been a huge annoyance as we'd have to load the school website almost every day to look it up.

Given that the thermal printer is powered by a **Raspberry Pi**, giving you the ability to write just about any kind of code you can think of in various languages, these ideas proved pretty simple.

### Printing text messages on the IoT thermal printer

You may ask yourself, why would you want to send a text message to a printer? The best answer I can come up with is, because I can! As with most things, it's solving the challenge that provides the satisfaction, not the function itself.

As you've no doubt guessed, I used a number of cloud services to make it possible:

- [Twilio](http://www.twilio.com/). Seriously one of the best developer services I've ever used. It's so much fun to bridge your applications with a phone (whether through SMS, MMS, or IVR).

- [Heroku](http://www.heroku.com/). A cloud platform-as-a-service (PaaS) and part of the Salesforce1 Platform.

- [CloudAMQP](http://www.cloudamqp.com/). A managed RabbitMQ server in the cloud, acquired as a [Heroku Add-On](https://addons.heroku.com/).

And, of course, some Python code on the Raspberry Pi.

Here's how the three cloud services work:

1. You text a message to a phone number. This phone number is owned by Twilio and I'm effectively "renting" it. They receive the message and send it (and some additional metadata) to a web service I have running in Heroku. This is set by specifying the **Messaging Request URL** on your Twilio number. (Note: Please refer to Twilio's fantastic developer documentation if you have any problems.)

	<img src="https://f.cloud.github.com/assets/746259/2115239/f2396b48-904b-11e3-855b-a4f7c03497c0.png" alt="Twillio Message Request URL" style="border-style: solid;border-width:1px;border-color:#767676;padding-left:10px;">

2. Our web service running in Heroku receives the message and extracts the phone number and message. (Note: Heroku also has fantastic developer documentation. Please refer to it to see how to create and deploy your web service.)

3. Using the data received from Twilio, we write a message to **CloudAMQP**, queuing it up for later retrieval.

4. Before finishing, the web service on Heroku creates and sends a Twilio response so that the sender knows their message was successfully received.

	Here's the code for the web service running in Heroku:

	<script src="https://gist.github.com/wadewegner/8872919.js?file=run.py"></script>

At this point we have our message sitting in a queue. This is perfect, as it allows us to write a little code on our printer to periodically check the queue and, if a message is found, print it!

On the Raspberry Pi, there are two things I did:

1. I created a script called <span class="inline-code">amqp.py</span> that checks for messages in the CloudAMQP queue and, if found, formats and prints the message.

	<script src="https://gist.github.com/wadewegner/8872919.js?file=amqp.py"></script>

2. Modified <span class="inline-code">/etc/rc.local</span> to automatically run the <span class="inline-code">amqp.py</span> script when the Raspberry Pi is booted.

		cd /home/pi/Python-Thermal-Printer
		python amqp.py &

Now the printer automatically run the <span class="inline-code">amqp.py</span> script on start-up and, whenever a message is delivered, it will print it!

Let me tell you, it's super cool when you see messages start to pour in!

![printerworks](https://f.cloud.github.com/assets/746259/2115364/947f6fc2-904e-11e3-9c81-c495e80a82b7.jpg)

Text away!

### Getting the school lunch menu

As I said, I had grown tired of having to load up the school lunch menu every morning. We'd routinely have to browse to [this lunch menu](http://dcsd.nutrislice.com/menu/eldorado/lunch/). It gets tiring after the 100th time.

So, with the help of my friend [Steve Marx](http://twitter.com/smarx) I created a Python script that does the following:

1. Downloads all the HTML from the lunch menu website.

2. Using a REGEX to extract the lunch menu data.

3. Iterates through the data until it finds today's date.

4. Formats and prints the menu on the printer.

Take a look at the code:

<script src="https://gist.github.com/wadewegner/8872919.js?file=lunch.py"></script>

Simple (except for the REGEX) and effective.

Here it is in action:

![2014-02-07 16 23 51](https://f.cloud.github.com/assets/746259/2115402/5f1da8a2-904f-11e3-91f6-bdfb78e09912.jpg)

Mmm, there's nothing quite like Cowboy Mac!

There you have it! A solution guaranteed to reduce your daily stress and make your children incredibly happy!

I hope you found this solution interesting. If you do something like this yourself definitely let me know! You can find all the source code for this solution in the following gist: [https://gist.github.com/wadewegner/8872919](https://gist.github.com/wadewegner/8872919)
