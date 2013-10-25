---
layout: post
title: "Migrated to Jekyll"
description: ""
categories:
- Jekyll
- Markdown
---
{% include setup %}

For quite some time I've wanted to move my blog off  WordPress and start to use [Jekyll](http://jekyllrb.com/) - in fact, I started looking into Jekyll back when I was still at Microsoft and had many conversations with [Steve Marx](http://blog.smarx.com/) about making the move. In this short post I want to discuss my motivations for making the switch and also highlight some of the unique things I did to make this quick and simple. I do not plan to get into the details of Jekyll and how it works - there are a lot of posts and articles available for you to review.

**Why Did You Move to Jekyll?**

I've already had a number of people ask my why I made the move.

* I want more control of my HTML. I found it extremely difficult to customize and control the rendering of content with WordPress.

* It's just HTML. No databases. No server-side rendering of content. It's simple.

* Because it's just HTML and so simple, you can host it almost anywhere. I've chosen Github Pages (for now), but you could easily host it on [Site44](http://www.site44.com/), [Heroku](https://www.heroku.com/), or even [Amazon S3](http://www.allthingsdistributed.com/2011/08/Jekyll-amazon-s3.html).

* Cost. I know there are cheaper ways to host WordPress, but I had been on GoDaddy for quite a long time. Moving to Jekyll and Github Pages removes the cost of hosting.

* Easy to have your entire blog - most importantly all your posts! - in source control. I simply commit and push my updates to Github and not only does it get published it also commits to my repository.

* Writing posts doesn't require any fancy tools - since everything is written in Markdown, I can use any text editing software I want. I use [Sublime Text](http://www.sublimetext.com/), but I could just as easily use Notepad.

* Like any other hacker, I wanted to learn something knew.

It's unlikely all of these points will resonate with you, but it's likely that a few will.

**A Few Tips on Migration**

So, now that you understand my motivation, let me share a few of the things I did to make the migration from WordPress to Jekyll easier.

1. Use a Mac. I hate saying this, but the reality is that this entire process will go easier if you use a Mac. I have tried twice in the past to migrate and failed both times from a PC. Even now that I have the migration complete I find that I'm unable to run the command ...

		bundle exec jekyll serve --watch

	... on the PC and get it to create the static files as it will fail to properly convert the markdown files. This works perfectly fine on both the Mac as well as in the Github Pages.

2. Use [ExitWP](https://github.com/thomasf/exitwp). I performed my migration against a WordPress export file. Originally I used the Jekyll importer for Wordpress but found that it did not do a good job of converting my HTML into markdown. I dug around a bit and found ExitWP - it ran like a champ and did a fantastic job of creating the markdown files. Simply clone the repository and use your WordPress.xml export file.

3. I decided to use [Jekyll Bootstrap](http://jekyllbootstrap.com/) as the starting point for a few reasons:
  * I love the simple elegance of Bootstrap, and it was already baked in.
  * It's 100% compatible with Github Pages.
  * There are some nice [Liquid](https://github.com/Shopify/liquid) templates for common things like Archives, Categories, Tags, and so forth.

  <br/>
	All this said, Jekyll Boostrap also had a ton of code for tools and includes that I'll never use. I ended up spending a good amount of time removing and triaging much of the code. I like it simple and neat. You can take a look at my repository for inspiration.

4. My blog has a lot of code snippets. Even though ExitWP did a good job of converting my HTML to markdown, I found that most of the code snippets did not get converted well. I've spent a decent amount of time going through the posts and updating. To make my life simpler, I decided to use Webscript and some simple jQuery code to give my readers the ability to notify me of a page with bad formatting.

	To implement this I simply created a link at the top of each blog post. 

	![](http://wadewegner.blob.core.windows.net/wordpress/2013/10/2013-10-25-PardonOurDust.JPG)

	When clicked, I perform a simple post to my Webscript ...

		$( "#target" ).click(function() {
		
		  $.post('http://MYCNAME.webscript.io/fixit', {
		      url: '{{ BASE_PATH }}{{ page.url }}', submittedDate: new Date().getTime()
		    }, function (data) {
		    
		      // capture response
		
		    });
		
		  reset();
		  alertify.alert("Thank you for letting me know! I'll try to get this fixed ASAP.");
		});

	... and then thank the user (using Prettify.js).

	![](http://wadewegner.blob.core.windows.net/wordpress/2013/10/2013-10-25-ThankYou.JPG)

	In my Webscript I store the URL in a JSON file. I have a second Webscript that will return the JSON file and show me all the URLs posted by users.

		[
		   {
		      "url":"/2013/03/creating-anonymous-rest-apis-with-salesforce-com/",
		      "submittedDate":"1382712106161"
		   },
		   {
		      "url":"/2011/02/running-multiple-websites-in-a-windows-azure-web-role/",
		      "submittedDate":"1382687836498"
		   },
		   {
		      "url":"/2011/02/running-multiple-websites-in-a-windows-azure-web-role/",
		      "submittedDate":"1382687828850"
		   },
		   {
		      "url":"/2007/05/a-new-twist-to-the-error-unable-to-enlist-in-the-transaction/",
		      "submittedDate":"1382676576905"
		   },
		   {
		      "url":"/2010/12/using-web-deploy-with-windows-azure-for-rapid-development/",
		      "submittedDate":"1382673263024"
		   }
		]

I'll dig into the topic of Webscript.io deeper in a separate post. Webscript.io is a fantastic tool that you should look into using - especially if you have a static website and don't want to host your own Web APIs. Suffice to say, this is an easy / simple way to use crowd-sourcing to help me identify posts that need to be fixed.

Aside from the above, I haven't done all that much. I love having all my code accessible and find that I spend more time experimenting with things I'd never do on Wordpress.

Hopefully this post explains my rationale for migrating to Jekyll and gives a few tips to help you, should you also decide to make the move.

I hope this helps!