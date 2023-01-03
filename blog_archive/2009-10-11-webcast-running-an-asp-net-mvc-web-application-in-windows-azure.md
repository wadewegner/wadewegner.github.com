---
author: admin
categories:
- .NET 3.5
- ASP.NET
- Azure
- MVC
- Windows Azure
comments: true
date: "2009-10-11T16:59:42Z"
slug: webcast-running-an-asp-net-mvc-web-application-in-windows-azure
title: 'Webcast: Running an ASP.NET MVC Web Application in Windows Azure'
wordpress_id: 263
---

I have seen enough people ask about running ASP.NET MVC Web Applications in Windows Azure that I thought I'd put together a short, quick webcast that shows exactly the steps you need to take. With the further ado ...







            
            
            
            
            
            
            
            

             


                    ![](ASP.NET MVC in Windows Azure_Thumb.jpg)
                    ![](Preview.png)
                    


                    


                    ![Get Microsoft Silverlight](http://go2.microsoft.com/fwlink/?LinkId=108181)
                    

                    [
                        ![Get Microsoft Silverlight]()
                    ](http://go2.microsoft.com/fwlink/?LinkID=124807)
             


        





Before you try this yourself, make sure you satisfy the following dependencies:






  * Visual Studio 2008 with SP1 


  * [Windows Azure Tools for Visual Studio](http://www.microsoft.com/downloads/details.aspx?FamilyID=8d75d4f7-77a4-4adf-bce8-1b10608574bb&displaylang=en)


  * Microsoft ASP.NET MVC 




For those of you that have no desire to watch a four minute video, and would rather have a quick walkthrough, here you go:






  1. Create a blank cloud services project. Do not add any roles to the project. 


  2. Add a new ASP.NET MVC Web Application to the solution. 


  3. Add the ASP.NET MVC Web Application as a web role in the cloud services project. 


  4. Add the Microsoft.ServiceHosting.ServiceRuntime.dll assembly to the ASP.NET MVC Web Application. 


  5. Set the following MVC assemblies to Copy Local True.


    * System.Web.Abstractions 


    * System.Web.Mvc 


    * System.Routing 





  6. Run the application. 




I hope this helps!
