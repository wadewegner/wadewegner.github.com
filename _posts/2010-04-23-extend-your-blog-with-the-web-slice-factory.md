---
author: admin
comments: true
date: 2010-04-23 21:43:19+00:00
layout: post
published: false
slug: extend-your-blog-with-the-web-slice-factory
title: Extend your blog with the Web Slice Factory
wordpress_id: 550
categories:
- IE
- Tips
---

One of my favorite personal uses for [IE8 web slices](http://www.microsoft.com/windows/internet-explorer/features/easier.aspx) is to extend the reach of my blog. Sure, there are a lot of great corporate examples of IE8 web slices – [Chicago Tribune](http://www.chicagotribune.com/news/local/), [McDonalds](http://www2.mcdonalds.com/mcnuggets/), [Apartments.com](http://www.apartments.com/index_QS.aspx), and [CareerBuilder.com](http://www.careerbuilder.com/Jobseeker/Jobs/JobResults.aspx) – but there are also some really helpful options for the occasional/professional blog poster too. For anyone running Internet Explorer 8, you have the option to install a web slice from my blog that lists out all of my posts. Additionally, it will visually notify you when I make write a new post.

 

Here’s how to use it. From my blog, users can get this functionality a couple different ways:

 

  
  1. Hover over the web slice discover section in the blog.        

  ![Take my posts with you](https://wadewegner.blob.core.windows.net/wordpress/2010/04/image.png)
   
  2. From the IE tool bar, add the web slice.        

  ![Add my web slice](https://wadewegner.blob.core.windows.net/wordpress/2010/04/image1.png)
 

Really easy, and really powerful. This means that readers have access to my latest posts _anywhere_ they are browsing.

 

The best part is that web slices can notify users of updates – anytime my RSS feed updates, the users with my web slice installed will receive a visual notification that there is new content. This creates a direct, **one-to-one channel between me and my readers** everywhere. Brilliant. Here’s what a notification looks like:

 

![IE8 web slice notification](https://wadewegner.blob.core.windows.net/wordpress/2010/04/image2.png)

 

And when you select it, you’ll see the contents of the web slice:

 

![Wade Wegner web slice](https://wadewegner.blob.core.windows.net/wordpress/2010/04/image3.png)

 

For the most part, web slices are pretty easy to configure – they’re basically a web page with meta data thrown around it. However, to get a slick RSS web slice setup like mine requires a bit of code. Here’s where the Web Slice Factory, built by [Giorgio Sardo](http://blogs.msdn.com/Giorgio/) and the Internet Explorer team, fits in.

 

The Web Slice Factory is exactly what it sounds like – a factory (or wizard) for creating web slices. Walk through the simple steps, and the end result is a nice web slice like the one you see on my blog. Here’s how to get started.

 

  
  1. Browse to [http://ieaddons.com/](http://ieaddons.com/) and click the **Join** link at the top. Follow the steps to create an account.       

  
   
  2. Browser to the Web Slice Factor page ([http://www.ieaddons.com/Admin/account/CreateSlice.aspx](http://www.ieaddons.com/Admin/account/CreateSlice.aspx)) to start the process.       

  
   
  3. You can setup three kinds of web slices: 1) Blog/Feed, 2) Twitter, and 3) Flicker. Choose which one you want to create. In this case, I’ll stick with Blog/Feed.        

  ![Pick a source](https://wadewegner.blob.core.windows.net/wordpress/2010/04/image4.png)
   
  4. Enter your blog / feed RSS URL into the box. Immediately upon entering the feed you’ll see the preview light up on the right side.        

  ![Personalize your Web Slice](https://wadewegner.blob.core.windows.net/wordpress/2010/04/image5.png)
   
  5. Customize the web slice to look the way you want it to look. To _really_ brand it the way you want it, use a background image with the logo and style you want.         

   
  6. When you’re ready, click **Save My Web Slice**. 
 

 

At this point you’re done. You can use some of the links provided to share with Twitter or Facebook, and you can install it right away.

 

But, how do you get this on your own website?

 

Pretty easily, actually. Basically, you need to wrap some meta data around a section of your website / blog to tell Internet Explorer 8 about your web slice. For example, here’s what I’ve done on my blog:

 

   

     
    
    <div class="hslice" id="WadeWegnerWebSlice"> 
	    <div style="display:none" class="entry-title">Wade Wegnerdiv> 
		    <div class="entry-content"> 
		      IE8 users, view my blog posts as a Web Slice in your browser. 
		    </div> 
	    	<a href="http://www.ieaddons.com/en/ie8slice/wsUpdate.aspx?id=1053" rel="feedurl" style="display:none">a> 
	    </div>
	</div>














That’s it. To get this to work with your own slice, do the following:






  
  * Change **WadeWegnerWebSlice** to **YourNameWebSlice**. This isn’t crucial to get it to work properly, but you might as well update it.

    


  


  
  * Change “Wade Wegner” in the entry-title to your name.
    


  


  
  * Update the entry-content to display the text you want to show.
    


  


  
  * See how the link poings to wsUpdate.aspx?id=1053 – you want to update it to use your own identifier. Go back to [http://ieaddons.com/](http://ieaddons.com/) and go to **Account Home**. From there, edit your web slice. In the URL you’ll see …&id=1053 – that’s where you can grab the identifier. Use it to update the URL. 





And that’s it. Once you make those changes and deploy them to your site, people can start installing your web slice and taking it with them.





Easy and powerful. Just what I like.
