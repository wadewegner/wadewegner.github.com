---
author: admin
comments: true
date: 2007-06-19 00:45:51+00:00
layout: post
slug: two-great-blogging-widgets-clustrmap-and-whosamungus
title: 'Two great blogging widgets: ClustrMap and whos.amung.us'
wordpress_id: 70
categories:
- Widgets
---

I'm finally taking the time to highlight two blogging widgets that I have been using on my blog: [ClustrMap](http://www2.clustrmaps.com/) and [whos.amung.us](http://whos.amung.us/). These aren't the only widgets I use, but of the half-dozen scattered throughout my blog these are (arguably) the least well-known (at least, I don't see them very often).

The first is [ClustrMap](http://www2.clustrmaps.com/). ClustrMap is a mapping widget that embeds itself into your blog and visually displays the location of your various visitors. ClustrMap provides HTML source that renders as a map, and when it loads it increments a counter and displays the locations of all the visitors to your page. Clicking on the map zooms to a big world map, and allows you to zoom into the continents.

I use this in my sidebar, toward the bottom of my blog. Here's what it looks like:

![ClustrMap example](http://images.wadewegner.com/wordpress/content/binary/cluster.gif)

It reminds me of a small-scale version of the map Google Analytics provides. Here's an example of the script you would embed within your HTML:

	<a href="http://www2.clustrmaps.com/counter/maps.php?url=http://www.wadewegner.com" temp_href="http://www2.clustrmaps.com/counter/maps.php?url=http://www.wadewegner.com" id="clustrMapsLink">
	    
		<img src="http://www2.clustrmaps.com/counter/index2.php?url=http://www.wadewegner.com" style="border:0px;" alt="Locations of visitors to this page" title="Locations of visitors to this page" id="clustrMapsImg" onError="this.onError=null;this.src='http://clustrmaps.com/images/clustrmaps-back-soon.jpg';document.getElementById('clustrMapsLink').href='http://clustrmaps.com'" />

	</a>

Very easy to use and setup. It takes about a day for it to gather enough information and draw a map, so don't expect it to work immediately. If you click on the map you can get a few statistic about the site. Here's an example:

![ClustrMap stats](http://images.wadewegner.com/wordpress/content/binary/stats.gif)

The second widget is [whos.amung.us](http://whos.amung.us/). Have you ever wanted to know how many users are reading your Web site or blog in real time? This widget is the answer. Again, they provide HTML script that you can embed within your Web site, and it renders as an image that counts the number of users on your Web site. Clicking on the images takes you to their Web page, which shows you exactly which pages your readers are on, as well as how long ago they arrived.

Here's what the widget looks like:

[![counter](http://whos.amung.us/cwidget/ds0261t1/7cbafcffffff)](http://whos.amung.us/show/ds0261t1)

Here's the page that shows the URLs your readers are currently at (it's also neat that the links refresh themselves every few seconds):

![whos.amung.us stats](http://images.wadewegner.com/wordpress/content/binary/links.gif)

Also, [whos.amung.us](http://whos.amung.us/) provides a functional Color Wheel that allows you to change the colors of the widget:

![whos.amung.us color map](http://images.wadewegner.com/wordpress/content/binary/wheel.gif)

As with ClustrMap, the code for this widget is very simple:
    
	<a href=http://whos.amung.us/show/ds0261t1>
	    
		<img src="http://whos.amung.us/cwidget/ds0261t1/7cbafcffffff" temp_src="http://whos.amung.us/cwidget/ds0261t1/7cbafcffffff" alt="counter" width="81" height="29" border="0" />
	    
	</a>

Neither of these tools, [ClustrMap](http://www2.clustrmaps.com/) or [whos.amung.us](http://whos.amung.us/), require you to setup an account or provide any personal information. Simply paste the HTML code into your Web page, and off you go!

What tools and/or widgets do you use on your blog or Web site that are useful?