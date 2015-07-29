---
author: admin
comments: true
date: 2009-06-26 17:57:12+00:00
layout: post
slug: architecting-your-data-access-layer-with-the-entity-framework
title: Architecting Your Data Access Layer with the Entity Framework
wordpress_id: 154
categories:
- Architecture
- Entity Framework
---

I had the pleasure to co-present with one of my fellow evangelists, [Dave Bost](http://davebost.com/blog/), on architecting and developing with the [ADO.NET Entity Framework](http://www.bing.com/search?q=ADO.NET+Entity+Framework+&form=QBRE&qs=n) this week. I focused on application architecture topics while Dave focused on developing applications.

I think we can all agree – and if not, please let me know why – that the Entity Framework provides some awesome capabilities – mapping your conceptual schema to your data schema, isolation from the relational database and database schema, [change tracking and identity resolution](http://msdn.microsoft.com/en-us/library/bb896269.aspx), full query comprehension and [optimization](http://blogs.msdn.com/adonet/archive/2008/03/27/ado-net-entity-framework-performance-comparison.aspx), and more. Yet, despite these features, it is difficult to figure out how and where to include the Entity Framework in your application architecture.

In preparation for this talk, I spent a lot of time looking at different strategies and architectures for using your [data access layer](http://www.bing.com/search?q=data+access+layer&mkt=en-us&FORM=IE8SRC) (DAL) and the Entity Framework – or any [O/RM tool](http://www.bing.com/search?q=object-relational+mapping&form=QBRE&qs=n), for that matter. In the end, I settled on three approaches – certainly there are other patterns and approaches, but I found these to be the most relevant:


1. Entity Framework as the DAL 

2. Full encapsulation of the Entity Framework 

3. Partial encapsulation of the Entity Framework 

Let me spell it out in a little more detail, as well as highlight what I feel are the different pro’s and con’s.

# Entity Framework as the Data Access Layer

With this approach, you opt to use the Entity Framework as your data access layer. This is a perfectly legitimate approach, and comes with a host of benefits in many situations. Here’s a picture of the architecture:

![EF](https://wadewegner.blob.core.windows.net/wordpress/2010/04/EF.png)

As you can see, in this architecture aspects of the Entity Framework – like entities and the entity framework object context – are scattered throughout the various tiers. This approach has many benefits, but it also introduces some challenges.

- Pro’s:
  
  * Excellent isolation from the DB schema 
   
  * Independence from the relational database 
   
  * Object services, namely identity and change tracking 
   
  * Full query comprehension and optimization 
 
- Con’s:
  
  * In .NET 3.5 SP1 the “object first” approach is not well supported (updated in EF 4.0) 
   
  * Difficult to switch your O/RM, as you are tightly coupled to the Entity Framework 
   
  * Limited support for data validation 
 

If you aren’t offended by the scattering of your entities and entity contexts throughout your application tiers, then this is a perfectly valid architecture. That said, most people probably recognize that technology changes, and an application that works perfectly well with Entity Framework today may need to move to a newer technology in the future. Consequently, you may want to look at an approach that encapsulates the Entity Framework, so that you separate your concerns and have high cohesion in your tiers without tight coupling.

# Full Encapsulation of the Entity Framework

There are many benefits to encapsulation. We have talked and read about ensuring a [separation of concerns](http://www.bing.com/search?q=separation+of+concerns&mkt=en-us&FORM=IE8SRC) – e.g. [high cohesion](http://en.wikipedia.org/wiki/Cohesion_(computer_science)), [loose coupling](http://en.wikipedia.org/wiki/Loose_coupling) – for years now, and for good reason. Typically we want to reduce the tight coupling throughout application so that tools like Entity Framework don’t proliferate through all the tiers of our application. This helps make our code more testable, and allows us to utilize techniques such as [Inversion of Control](http://en.wikipedia.org/wiki/Inversion_of_control) and [Dependency Injection](http://en.wikipedia.org/wiki/Dependency_injection).

At the same time, fully encapsulating a tool like Entity Framework also comes with some costs. If the real value of this tool is the ability to empower developers to quickly build powerful applications that are fully optimized against your database, then full encapsulation may inhibit some of the capabilities.

- Pro’s:
  
  * Simplified queries 
   
  * POCO objects 
   
  * ORM & database agnostic 
   
  * No references to Entity Framework 
   
  * Isolation of SQL queries 
 
- Con’s:
  
  * Loss of change tracking and identity management with Entity Framework 
   
  * Additional object materialization of business objects 
   
  * No query composition; simple LINQ enumeration queries 

Again, this is not a full laundry list, but hopefully it provides some of the different benefits and challenges. Fortunately, there’s a third path you can take, which is a hybrid of the two – I call this partial encapsulation.

# Partial Encapsulation of the Entity Framework

When you decide to partially encapsulate the Entity Framework, you acknowledge the benefits of the separation of concerns and do what you can to loosely couple your tiers. But you also try to preserve the advantages of a tool like the Entity Framework. In this scenario, you might expose your entities to the different tiers of your application, but encapsulate the rest of the Entity Framework in your DAL. When you return data from the DAL, you would simply use one of the three techniques: IEnumerable, IQueryable, or ObjectQuery. Other than the last technique, you aren’t exposing aspects of the Entity Framework – other than the entities themselves – to any other tiers. This preserves the loose coupling, but allows you to leverage the real value of the Entity Framework.

- Pro’s:
  
  * Query composition 
   
  * Identity resolution & changing tracking 
   
  * Some independence from O/RM tool 

- Con’s:
  
  * Use of Entity Framework entities has implications on business layer 
   
  * Lack of isolation of SQL queries – spread throughout the various tiers 

Partial encapsulation is a pragmatic approach to this problem, and it’s a technique the I have seen many customers and partners utilize very successfully. In the end, the approach you choose depends on your situation. All three of these approaches are perfectly valid, but aren’t necessarily valid in every situation.

Below are the slides I used during the presentation. I am sure that this is a presentation I’ll give again in the future, so expect to see updates and/or changes.

Slides: [ADO.NET Entity Framework & Your Data Access Layer](http://www.slideshare.net/wwegner/adonet-entity-framework-your-data-access-layer?type=powerpoint)    

I’d love to hear your feedback, so please leave me a comment or message.

I hope this helps!