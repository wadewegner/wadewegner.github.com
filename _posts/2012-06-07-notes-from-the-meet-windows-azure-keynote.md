---
author: admin
comments: true
date: 2012-06-07 21:35:15+00:00
layout: post
slug: notes-from-the-meet-windows-azure-keynote
title: Notes from the MEET Windows Azure Keynote
wordpress_id: 1836
categories:
- IAAS
- Linux
- PAAS
- Windows Azure
---

Excellent keynote today by Scott Guthrie and the Windows Azure team! I don’t know if it’s because I’m seeing it on the other side for the first time or if it’s due to the amazing new capabilities introduced into Windows Azure, but I loved every second of it! Within a few minutes Scott had jumped into a demo and it was nonstop thereafter!

 

I’ve been waiting for almost a year to write this blog post.

 

**_Keynote with Scott Guthrie_**

 

![WP_000321](https://wadewegner.blob.core.windows.net/wordpress/2012/06/WP_000321_thumb.jpg)

 

In order to get this post out quickly I decided to share my rough notes from the keynote. Over the coming days and weeks I will certainly expand this with more information.

 

**Scott Guthrie:**

 

  
  * Today’s release “elevates Windows Azure to a new level”
   
  * Capabilities like Windows Azure Web Sites and open source libraries open the platform to many more developers
   
  * Three pillars of Windows Azure:
        
    * Flexible
     
    * Open
     
    * Solid
      
  * Windows Azure has pioneered Platform-as-a-Service
   
  * Today we’re enabling Infrastructure-as-a-Service (IaaS)
   
  * Support of Linux is how we’re embracing openness in a new way
   
  * Supporting more languages, protocols, and SDKs
   
  * Open-source libraries and SDKs on Github under the Apache 2.0 license
 

**Demo: New Windows Azure website**

 

  
  * There’s a new Linux and Mac installer for Node.js
   
  * Spent time optimizing the portal to work across platforms
   
  * Full monitoring and statistics through the portal
   
  * Everything done through the portal is communicating to APIs thereby allowing you to do everything through the command line
   
  * IaaS
        
    * Create a new virtual machine
     
    * Anything installed on the machine will persist; full durable VM
     
    * Image gallery
            
      * Linux distribution built into the portal
          
    * We have a number of SDKs to download, including Mac
            
      * installs an Azure utility and wires up a BASH shell
       
      * including ASCII art
       
      * SSH’d into Ubuntu machine
           
  * Virtual Networks
        
    * Not just upload VMs into the cloud
     
    * Integrate into your existing networks and VMs you already have
     
    * Shipping virtual private networking
     
    * A wizard to create an address space and subnet; we’ll virtualize these addresses
     
    * You can provide DNS servers in the cloud or on-premises
    

**Scott Guthrie on VMs:**

 

  
  * Virtual machine portability
        
    * Between the cloud and different environments
     
    * All VMs are running a VHD format which is an open spec
     
    * Because it’s the same file format you can take a VM and move to Windows Azure or even move it back; you don’t have to export or convert the VHDs
     
    * This allows you to run in your data center, other service providers, or within Windows Azure: flexibility, portability, and no lock-in
      
  * VM persistent drive
        
    * Mount durable drives to the virtual machines
     
    * Reliable and consistent
     
    * When you mount a drive the disk is backed with the Windows Azure storage system
            
      * Triple replicate the content; no interruption of service if there’s a failure
       
      * Continuous storage and geo-replication
         

**Partner: RightScale**

 

  
  * Michael Crandell, CEO
   
  * Mission: to build a broad cloud management platform
   
  * Web-based environment that allows companies to manage everything from development to delivery of applications running on cloud infrastructures
   
  * Think of us as a bridge between the apps and the infrastructures you want to run on (public, private, or hybrid)
   
  * Partner: worked with the Windows Azure team to interact with the new IaaS APIs
   
  * Brought to Windows Azure the full set of RightScale capabilities
   
  * When run by RightScale the solution includes auto scale
   
  * Why Windows Azure?
        
    * Microsoft knows how to run global cloud infrastructure with a high degree of operational excellence
     
    * IaaS + PaaS is an industry first – provides an entire environment for the development and deployment of apps that are compelling, from simple to complex
     
    * Openness – a clear commitment of openness at multiple levels
    

**Scott Guthrie on Web sites:**

 

  
  * Benefits
        
    * Build with ASP.NET, Node.js, or PHP
     
    * Deploy in seconds with FTP, Git, or TFS
     
    * Start for free, scale up as your traffic grows
      
  * Bill Staples demo
   
  * Opposite end of the cloud spectrum
        
    * don’t want to focus on the platform; don’t want to install the frameworks
      
  * Visual Studio & .NET
        
    * Quickly created a website
     
    * Downloaded a publishing profile that includes all the connection information
     
    * Opens up Visual Studio and ASP.NET MVC 4
     
    * Import the profile and publish
     
    * Support in both VS 20120 and VS 2012
     
    * Built and published into the cloud in just a few seconds
     
    * Updated a string and republished
            
      * Preview shows the deltas between local and cloud
       
      * Just push the change
           
  * Mac with Node.js
        
    * Instead of the portal use the commandline
     
    * Use the azure command
     
    * Uses git to deploy
     
    * One update connects to Mongo running in a Linux VM
      
  * Sometimes it’s nice to build an application without writing code
        
    * Best of Windows Azure and the power of open source
     
    * MySQL: worked with ClearDB to provide MySQL as a service 
      
  * Scale
        
    * Scale out
            
      * By default, website is running as a Shared Website
       
      * You can increase instance count
          
    * Scale up
            
      * Move from Shared to Reserved
       
      * Only my websites are routed
           
  * 10 free shared websites
 

**Scott Guthrie on Cloud Services:**

 

  
  * Another model for building infinitely scalable applications and services
   
  * The traditional PaaS offering that Windows Azure has had since release
   
  * Creates a multi-tier applications using the website Bill created and adding a worker role
   
  * Process
        
    * Upload a service package (which is essentially a zip file) into the cloud
     
    * Fabric controller provisions the machines and deploys my bits and brings it into rotation through load balancer
    

**Scott Guthrie on Building Blocks:**

 

  
  * Overall message is to enable developers to focus on applications and less about infrastructure
   
  * Application building blocks
        
    * big data
     
    * database
     
    * storage
     
    * traffic
     
    * caching
     
    * messaging
     
    * identity
     
    * media
     
    * CDN
     
    * networking
      
  * Delivered by MSFT and partners
   
  * Language support for: .NET, Node.JS, Java, PHP, Python
        
    * Libraries you can consume natively
     
    * Libraries on Github under Apache2
      
  * SQL Database
        
    * Relational SQL database
     
    * Clustered for high availability
     
    * Fully managed service
     
    * SQL Reporting support
      
  * Blob storage
        
    * Features
            
      * Highly available, scalable and secure file system
       
      * Blobs can be exposed publically over http
       
      * Continuous geo-replication across datacenters
          
    * Real-time data showed in portal
     
    * SQLs surfaced in the portal
      
  * Cache
        
    * implementing Memcached protocol support
     
    * Use the distributed service or run it in your own instances
      
  * Identity
        
    * Integrate with enterprise identity
     
    * Enable single sign-on within your apps
     
    * Enterprise Graph REST API
     
    * 93% of Fortune 1000 use Active Directory
      
  * Service Bus
        
    * Secure messaging and relay capabilities
     
    * Easily build hybrid apps
     
    * Enable loosely coupled solutions
     
    * Cross platform SDKs
      
  * Media Service
        
    * Create, manage, and distribute content
     
    * Target any device or media format
     
    * Ingest, Encode, Protect, Stream
      
  * Marketplace integration
   
  * Azure in 89 countries and territories
