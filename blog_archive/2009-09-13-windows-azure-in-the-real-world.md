---
author: admin
categories:
- Azure
- SQL Azure
- User Groups
- Windows Azure
comments: true
date: "2009-09-13T15:39:34Z"
slug: windows-azure-in-the-real-world
title: Windows Azure in the Real World
wordpress_id: 175
---

![Joseph Paradi, Innovation Lead for Internal IT at Accenture](http://www.wadewegner.com/content/2009/09/JosephPresenting-300x178.jpg)
Last week I presented at the [Wisconsin .NET Users Group](http://www.wi-ineta.org/) for the first time, along with [Joseph Paradi](http://www.linkedin.com/pub/joseph-paradi/2/586/9a6). I had a great time, and I really appreciate the hospitality of the group - great facility, attentive audience, and excellent questions. The title of our talk was **_Windows Azure in the Real World_**; here's the abstract for the presentation:





> 
  
> 
> Does this sound familiar? Your boss is asking about cloud computing and Windows Azure, but you're not sure how to separate the hype from the reality. Or perhaps you've heard about Windows Azure and had a chance to try it out, but you still don't quite understand how or why to use it. Or maybe you've been using Windows Azure since PDC in 2008, but you'd like a clearer picture of the roadmap and pricing. If any of these points resonate with you, or if you have different questions and concerns, please join [Wade Wegner](http://www.wadewegner.com/), Architect Evangelist with Microsoft, and Joseph Paradi, Innovation Lead with Accenture, as they provide you with an update on the Windows Azure Platform and show you how companies like Accenture are using the cloud today. Additionally, Wade and Joseph will discuss the migration of existing internal applications to Windows Azure, securing applications through claims-based authentication and passive federation with Geneva Server, using relational databases in the cloud with SQL Azure, migrating data to the cloud through tools like SSIS, and more.
> 
> 






Our intent was to provide the user group with an update to the [Windows Azure Platform](http://www.azure.com/) and show how some companies (like [Accenture](http://www.accenture.com/)) are starting to explore the migration of internal IT applications to the cloud. I am hopeful we were successful.





During the presentation, I promised that I would put together a blog post and share the user group recording (thanks to Brennan and Scott for recording), useful links, and our decks. You'll find all three below.





**_Useful links_**






  
  * [Sign-up for a Windows Azure account](http://www.windowsazure.com)


  
  * [Sign-up for a SQL Azure account](http://msdn.microsoft.com/en-us/sqlserver/dataservices/)


  
  * [SQL Azure Migration Wizard](http://sqlazuremw.codeplex.com/)


  
  * [Windows Azure MSDN](http://msdn.microsoft.com/en-us/azure/cc994380.aspx)


  
  * [Windows Identity Foundation and Windows Azure passive federation](http://code.msdn.microsoft.com/wifwazpassive)


  
  * [ADFS 2 (i.e. Geneva Server)](http://msdn.microsoft.com/en-us/security/aa570351.aspx)





**_Update to the Windows Azure Platform_**












**_Joseph Paradi's "Real World Azure"_**












**_User Group Recordings_**





Rather than dropping a two hour presentation - which absolutely no one would watch or find useful - I have taken the time to extract some of the most important pieces of the presentation. This is a bit of an experiment on my part. I hope you find this useful; please feel free to provide me with feedback.





**Why should you care?**






        
            
            
            
            
            
            
            
             


                    ![](1 - WI .NET UG_Thumb.jpg)
                    ![](Preview.png)
                    


                    


                    ![Get Microsoft Silverlight](http://go2.microsoft.com/fwlink/?LinkId=108181)
                    

                    [
                        ![Get Microsoft Silverlight]()
                    ](http://go2.microsoft.com/fwlink/?LinkID=124807)
             


        







**"The Pillars of Concern"**






        
            
            
            
            
            
            
            
             


                    ![](1 - WI .NET UG_Thumb.jpg)
                    ![](Preview.png)
                    


                    


                    ![Get Microsoft Silverlight](http://go2.microsoft.com/fwlink/?LinkId=108181)
                    

                    [
                        ![Get Microsoft Silverlight]()
                    ](http://go2.microsoft.com/fwlink/?LinkID=124807)
             


        






Quick notes:






  
  * "The Pillars of Concern"
    
      
    * Authentication 


      
    * Authorization 


      
    * Data synchronization 


      
    * Security of data 


      
    * Application integration 


      
    * Operations / management 

    
  


  
  * Ultimate goal: reduce the cost/effort to move to Azure while proving we can 





**Demo Infrastructure**






        
            
            
            
            
            
            
            
             


                    ![](1 - WI .NET UG_Thumb.jpg)
                    ![](Preview.png)
                    


                    


                    ![Get Microsoft Silverlight](http://go2.microsoft.com/fwlink/?LinkId=108181)
                    

                    [
                        ![Get Microsoft Silverlight]()
                    ](http://go2.microsoft.com/fwlink/?LinkID=124807)
             


        






Quick notes:






  
  * The users are out there somewhere on the Internet 


  
  * Windows Azure Platform
    
      
    * OrgChart application runs on Windows Azure 


      
    * Database for OrgChart runs on SQL Azure 

    
  


  
  * On-premises
    
      
    * Corporate active directory 


      
    * "Geneva" Server 


      
    * Lookup applications runs locally 


      
    * Database for Lookup applications is local 

    
  


  
  * There is no copy of active directory in the Microsoft data center; it all stays local 





**Demo of the application running on Windows Azure**






        
            
            
            
            
            
            
            
             


                    ![](1 - WI .NET UG_Thumb.jpg)
                    ![](Preview.png)
                    


                    


                    ![Get Microsoft Silverlight](http://go2.microsoft.com/fwlink/?LinkId=108181)
                    

                    [
                        ![Get Microsoft Silverlight]()
                    ](http://go2.microsoft.com/fwlink/?LinkID=124807)
             


        






Quick notes:






  
  * [Passive federation with Geneva Server](http://bloggingabout.net/blogs/mveken/archive/2008/11/24/introduction-to-geneva-using-passive-federation.aspx)

    
      
    * Application says it doesn't have the SAML token, redirects user to the on-premises Geneva Server 


      
    * All authentication is done locally; none of the active directory credentials are passed to Azure 


      
    * A SAML token is generated and passed back to Azure 

    
  


  
  * Using things like ASP.NET AJAX controls on Azure 


  
  * Also shows integration into Office Communications Server (OCS) 


  
  * Demonstration of single sign on (SSO) via Geneva Server 





**Using SSIS packages to migrate data into SQL Azure**






        
            
            
            
            
            
            
            
             


                    ![](1 - WI .NET UG_Thumb.jpg)
                    ![](Preview.png)
                    


                    


                    ![Get Microsoft Silverlight](http://go2.microsoft.com/fwlink/?LinkId=108181)
                    

                    [
                        ![Get Microsoft Silverlight]()
                    ](http://go2.microsoft.com/fwlink/?LinkID=124807)
             


        






Quick notes:






  
  * Standard SSIS package; canonical example 


  
  * Execution process
    
      
    * SQL Task to truncate table 


      
    * Data Flow task to move data from local database server to SQL Azure 

    
  


  
  * Joseph emphasized that you can leverage existing tools and skills today when targeting Azure 





**Taking a look at Geneva Server**






        
            
            
            
            
            
            
            
             


                    ![](1 - WI .NET UG_Thumb.jpg)
                    ![](Preview.png)
                    


                    


                    ![Get Microsoft Silverlight](http://go2.microsoft.com/fwlink/?LinkId=108181)
                    

                    [
                        ![Get Microsoft Silverlight]()
                    ](http://go2.microsoft.com/fwlink/?LinkID=124807)
             


        






Quick notes:






  
  * Created a "relying party" for the Windows Azure application 


  
  * Defined the attributes to pass back in the SAML 2.0 token 


  
  * Because it's all standards based, you can easily swap out with other SAML 2.0 providers 





**Review of the demo; what did we see?**






        
            
            
            
            
            
            
            
             


                    ![](1 - WI .NET UG_Thumb.jpg)
                    ![](Preview.png)
                    


                    


                    ![Get Microsoft Silverlight](http://go2.microsoft.com/fwlink/?LinkId=108181)
                    

                    [
                        ![Get Microsoft Silverlight]()
                    ](http://go2.microsoft.com/fwlink/?LinkID=124807)
             


        






Quick notes:






  
  * Demonstrated authentication via [passive federation](http://bloggingabout.net/blogs/mveken/archive/2008/11/24/introduction-to-geneva-using-passive-federation.aspx)


  
  * Showed the use of claims as a way to authorize users 


  
  * Optimized the SSIS package for data synchronization 


  
  * Some of the challenges you'll face when moving to the cloud are no different from what you'll face when moving to a traditional hoster or ASP 


  
  * Then end-user doesn't care where the application runs; Geneva Server can help make this seamless with SSO 





**What are the gaps?**






        
            
            
            
            
            
            
            
             


                    ![](1 - WI .NET UG_Thumb.jpg)
                    ![](Preview.png)
                    


                    


                    ![Get Microsoft Silverlight](http://go2.microsoft.com/fwlink/?LinkId=108181)
                    

                    [
                        ![Get Microsoft Silverlight]()
                    ](http://go2.microsoft.com/fwlink/?LinkID=124807)
             


        






Quick notes:






  
  * Data security is specific to each organization; different requirements based on vertical, industry, etc. 


  
  * "No different than if I decided to farm out my hosting to someone else."


  
  * Lacking some capabilities around operations management; looking for integration into SCOM 





**What did it take to migrate the application?**






        
            
            
            
            
            
            
            
             


                    ![](1 - WI .NET UG_Thumb.jpg)
                    ![](Preview.png)
                    


                    


                    ![Get Microsoft Silverlight](http://go2.microsoft.com/fwlink/?LinkId=108181)
                    

                    [
                        ![Get Microsoft Silverlight]()
                    ](http://go2.microsoft.com/fwlink/?LinkID=124807)
             


        






Quick notes:






  
  * Quick steps
    
      
    * Application was original an ASP.NET 2.0 website; migrated to a ASP.NET 3.5 SP1 web application 


      
    * ["Geneva"-ify your application](http://code.msdn.microsoft.com/wifwazpassive); added support for claims 


      
    * Define your relying party in Geneva Server 


      
    * Create the database schema in [SQL Azure](http://www.microsoft.com/azure/sql.mspx) (use the [SQL Azure Migration Wizard](http://sqlazuremw.codeplex.com/)) 


      
    * Migrate the data using SSIS packages 

    
  


  
  * Initial application migration took about 40 hours of effort 





**Why is this so cool?**






        
            
            
            
            
            
            
            
             


                    ![](1 - WI .NET UG_Thumb.jpg)
                    ![](Preview.png)
                    


                    


                    ![Get Microsoft Silverlight](http://go2.microsoft.com/fwlink/?LinkId=108181)
                    

                    [
                        ![Get Microsoft Silverlight]()
                    ](http://go2.microsoft.com/fwlink/?LinkID=124807)
             


        






Quick notes:






  
  * Leverage the developer and IT pro skills you already have 


  
  * If you want to migrate an application to Azure tomorrow, you can do it very quickly 


  
  * Don't worry about the plumbing - just build your application 


  
  * Lots of guidance and tooling provided by Microsoft 


