---
author: admin
comments: true
date: 2010-06-24 04:14:44+00:00
layout: post
slug: real-world-patterns-for-cloud-computing-at-teched-na-2010
title: Real-World Patterns for Cloud Computing at TechEd NA 2010
wordpress_id: 635
categories:
- .NET 4.0
- Architecture
- Azure
- Windows Azure
---

![image](https://wadewegner.blob.core.windows.net/wordpress/2010/06/image.png)

It was an amazing [TechEd NA 2010](http://northamerica.msteched.com/), and I admit that it took me a few days to recover. Between the heat and humidity, great times with friends, and good food, I managed to spend a bit of time at the conference.

 

I had the pleasure of co-presenting with Jerome Schulist, a solutions architect at the [Tribune Company](http://www.tribune.com/). Jerome is one of the architects that engineered the solution that has allowed the Tribune Company to store and process terabytes of data on the Windows Azure platform. This solution involves a number of really interesting scenarios, including:

 

  
  * Parallelized upload of terabytes of digital content into Windows Azure blob storage using .NET Framework 4.0 
   
  * Best practices for uploading a massive amount of content 
   
  * Scaling strategy for Windows Azure blob storage through multiple storage accounts and a “round robin” pattern 
   
  * Content reprocessing with Windows Azure worker roles 
   
  * Automatic scale-out and scale-back of worker roles through queue lengths 
 

For detailed information on this solution, you can take a look at the [Tribune Company’s Windows Azure case study](http://www.microsoft.com/casestudies/Case_Study_Detail.aspx?CaseStudyID=4000007519).

 

As promised in the session, you can find the final code built during the session below. Just remember to update the config files with your own credentials.
