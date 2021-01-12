---
author: admin
categories:
- Force.com
comments: true
date: "2013-03-09T00:59:17Z"
slug: a-few-tips-on-getting-started-with-salesforce-com
title: A Few Tips on Getting Started with Force.com
wordpress_id: 1991
---

Today I got the opportunity to attend a Salesforce.com “hack day” in San Francisco. Quite fun, and I learned a lot from [Adam Seligman](https://twitter.com/adamse), [Dave Carroll](https://twitter.com/dcarroll), [Pat Patterson](https://twitter.com/metadaddy), and [Akhilesh Gupta](https://twitter.com/akhileshgupta). This is a great team: extremely personable, clearly excited about their platform and the technology, and willing to pull up their sleeves and write some code. My kind of people.

During the hack day I acquired a few tips for getting started. Nothing earthshattering but useful nonetheless.

1. Setup a [developer account](http://www.developerforce.com/events/regular/registration.php). It’s quick, easy, and free. You can get a lot of them.

2. Don’t use your production account for development. This may sound trite but I can tell you from first-hand experience that you don’t want to start hacking in production. The Salesforce.com development lifecycle has process for [deploying from one environment to another](http://www.salesforce.com/us/developer/docs/dev_lifecycle/Content/deploy.htm). Use it. Today I didn’t follow this advice and it cost me quite a bit of time. See tip #1 for getting a proper developer account.

3. Simplify the password requirements for your developer account. First, under your name, choose **Setup**.

	![image](http://www.wadewegner.com/content/A-Few-Tips-on-Getting-Started-with-S.com_E309/image_thumb.png)

	Then type “password” in the search box and click **Password Policies**.

	![image](http://www.wadewegner.com/content/A-Few-Tips-on-Getting-Started-with-S.com_E309/image_thumb_3.png)

	Choose some less restrictive policies. It’s up to you – personally, I don’t want my developer account password expiring all the time. **Note**: I do not recommend you do this on your production account.

	![image](http://www.wadewegner.com/content/A-Few-Tips-on-Getting-Started-with-S.com_E309/image_thumb_4.png)

4.	Change the default **IP Restrictions**. Again, for my developer account, I don’t want the platform continually challenging me if I log in from different places. After you browse to **Setup** search for **Profile**. Click on **Profiles**.

	![image](http://www.wadewegner.com/content/A-Few-Tips-on-Getting-Started-with-S.com_E309/image_thumb_5.png)

	Scroll down until your see **System Administrators** and select it.

	![image](http://www.wadewegner.com/content/A-Few-Tips-on-Getting-Started-with-S.com_E309/image_thumb_6.png)
	
	Scroll down to **IP Address Ranges** and click **New**. Enter default ranges 0.0.0.0 to 255.255.255.255 and click **Save**.

	![image](http://www.wadewegner.com/content/A-Few-Tips-on-Getting-Started-with-S.com_E309/image_thumb_7.png)


That’s all for now. For those of you who are pro’s on the platform this is likely not new or exciting. I’m sure there are more tips I’ll learn over time and, when I do, I’ll try to update this list.

I hope this helps!
