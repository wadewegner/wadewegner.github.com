---
layout: post
title: "Preparing for the Salesforce  Million Hackathon"
description: ""
categories:
- dreamforce
- exacttarget
- force.com
- hackathon
- heroku
- salesforce
tags: []
---
{% include setup %}

*This post originally appeared on the [Salesforce Developer Relations blog](http://blogs.developerforce.com/developer-relations/2013/11/preparing-for-the-salesforce-1-million-hackathon.html) on November 12, 2013.*

As you prepare to build a mobile app for the [Salesforce $1 Million Hackathon](http://blogs.salesforce.com/company/2013/10/hackathon.html) next week, it may help to step back and reflect on the capabilities of the Salesforce Platform and how tapping into these capabilities will better enable you to compete. In particular, it will help to understand why, and how, access to rich customer data can give your application an edge – in both the hackathon and beyond.

**What is Salesforce?**

You likely know that Salesforce’s core product started with a cloud-based customer relationship management application (CRM). A CRM application is focused on optimizing a company’s relationship with its customers. The Salesforce CRM product, Sales Cloud, does this by capturing and aligning customer interactions which are otherwise fragmented, incomplete, or incoherent. In doing so, Sales Cloud helps to provide a coherent conversation between a business and its customers by providing a single view of the truth – one schema, always up-to-date, and always available. In other words, it allows companies to become more customer centric and transform into a “customer company.”

Access to this customer data is important to your applications. Over and over we see that the biggest roadblock to mobile initiatives is access to customer data. This is where the Salesforce Platform helps. Sales Cloud is built on what is generally called the Salesforce Platform. This platform provides tools for building general purpose business applications and information-based mobile and social apps. It is through this platform developers can get into the heart of the customer conversation. For your mobile apps, think of Salesforce as the mobile backend that provides access to your customer data.

You might ask yourself, as a developer: Why does this customer stuff matter to you and your application? How does a coherent customer conversation enable you to build a great mobile application? The point is that the more connected you are to your customers and their data, the better your reach, the more you understand the features they love (or hate), and the more you can enable them to achieve their goals. Every application is a chance to be better connected with your customers, and all of that data lives in Salesforce.

**What are the Salesforce Platform APIs?**

In my opinion, one of the most significant advantages to using the Salesforce Platform is the backend for your mobile app is the access to all of your customer data – it’s all there, living in Salesforce. Through standards-based APIs you can connect to this data however you desire. You can develop a native, hybrid, or HTML5 application. The platform supports popular JavaScript frameworks so you can leverage your existing web skills. You can even write applications in the language or framework of your choice, whether that’s Ruby, Node.js, Clojure, Java, Python, .NET, or even Scala. The choice is yours.

As you start to look at the APIs available through the Salesforce Platform, you should first understand a few of the platform principles:

* It provides for both declarative and programmatic development capabilities. You can build a full data schema, live in the cloud, and useful web UI with just mouse drags and clicks. Furthermore, you can go beyond the mouse and programmatically build against the platform with the languages and frameworks of your choice.

* You can connect to everything through open APIs—the same APIs that perform over 1.3 billion transactions a day.

* You have a single trusted identity that leverages standards such as SAML and OpenID Connect for single sign-on between your enterprise, the Salesforce Platform, and other service providers.

Let’s look at the components used for building applications that connect to the Salesforce Platform.

**Building Applications that Connect to the Salesforce Platform**

There are three main platform services you should consider for your applications: Force.com, Heroku, and ExactTarget.

With [Force.com](http://www.salesforce.com/platform/what/), you can create a general purpose business application in minutes. This is accomplished not only through declarative drag-and-drop tools but also open APIs and tools from leading languages and frameworks. All standard and custom objects automatically have REST interfaces through which developers can interact, making it easy to work with and manipulate customer data. For example, with Force.com you can easily define new objects that relate to customer data—such as invoices and products—and automatically take advantage of the open APIs available for CRUD opperations against those new objects. All this without writing any code – Force.com handles this for you. Furthermore, if you don’t want to build your own applications to interact with those APIs, you can leverage the tools in Force.com to automatically render pages for both mobile and browser-based users.

[Heroku](https://www.heroku.com/) gives you the ability to build, deliver, and manage all of your customer-facing applications. Whereas Force.com provides you with the ability to interact with your customer data through APIs, Heroku is the ideal place to build the applications that use this data. Need a place to host your HTML5 mobile application, while also providing you a secure means for interacting with your customer data in Sales Cloud? Heroku is the platform for you.

With [ExactTarget Fuel](http://www.exacttarget.com/products/platform)—a recent addition to the Salesforce Platform—you have access to a cross-channel marketing automation platform. Fuel provides the APIs and capabilities to open ExactTarget’s marketing cloud to your applications, giving you the ability to access off-the-shelf solutions for your own particular marketing needs. Do you want the ability to create new email marketing campaigns? Or perhaps a business application that reports on the progress of an existing campaign? Fuel provides these capabilities for ExactTarget.

Together, these three components provide a comprehensive set of tools that make for compelling ways to interact with the rich customer data provided by the Salesforce Platform.

**Getting Started**

Hopefully the above has convinced you of two points: that access to customer data provides significant benefits to your applications and that Force.com, Heroku, and ExactTarget Fuel are valuable components that provide you with a mobile backend to your customer data.

If you’re like me, this gets real only once you dive in and get started. Be sure to signup for a free [Force.com Developer Edition account](https://events.developerforce.com/signup) and [Heroku account](https://id.heroku.com/signup/devcenter). Additionally, there are many resources available to you as you begin:

* Look at building native & hybrid mobile apps for [iOS](http://www2.developerforce.com/en/mobile/getting-started/ios) or [Android](http://www2.developerforce.com/en/mobile/getting-started/android).
* Use the lightweight [JavaScript](https://github.com/developerforce/Force.com-JavaScript-REST-Toolkit) SDK.
* Build with popular frameworks like [Angular, Backbone.js, and KnockoutJS](http://www2.developerforce.com/en/mobile/getting-started/html5).
* Explore the [ExactTarget Fuel platform](https://code.exacttarget.com/getting-started) and review the underlying development concepts.
* Grab [Javascript library for Node.js](https://devcenter.heroku.com/articles/getting-started-with-nodejs) development in Heroku.

I hope this helps as you prepare for the Salesforce $1 Million Hackathon. Happy hacking!