---
author: admin
categories:
- Entity Framework
- SQL Azure
comments: true
date: "2010-08-27T04:48:58Z"
slug: using-the-trustservercertificate-property-with-sql-azure-and-entity-framework
title: Using the ‘TrustServerCertificate’ Property with SQL Azure and Entity Framework
wordpress_id: 769
---

I’ve spent the last few days refactoring a web application to leverage SQL Server via Entity Framework 4.0 (EF4) in preparation for migrating it to SQL Azure. It’s a neat application, and a great example of how to fully encapsulate your data tier (the previous version had issues due to tight coupling between the data and web tier). More on this soon.

 

So, when I deployed my database to SQL Azure (using the SQL Azure Migration Wizard) I was confounded by the following error:

 

A connection was successfully established with the server, but then an error occurred during the pre-login handshake. (provider: SSL Provider, error: 0 - The certificate's CN name does not match the passed value.)

   
 

I was caught off guard by this error, as I was pretty sure my connection string was valid – after all, I had copied it directly from the SQL Azure portal. Then I realized that this was the first time I had attempted to use EF4 along with SQL Azure; my first thought was, “oh crap!”

 

After a little bit of frantic research I found the following [question on the SQL Azure forums](http://social.msdn.microsoft.com/Forums/en-US/ssdsgetstarted/thread/94d24825-c5f7-4fb1-8c9f-a43be3ebde70). Raymond Li of Microsoft made the suggestion to set the ‘TrustServerCertificate’ property to True. So, I updated my connection string from …

 

  
    
    <span style="color: #0000ff"><</span><span style="color: #800000">add</span> <span style="color: #ff0000">name</span>=<span style="color: #0000ff">"BidNowDataContext"</span>



  
    
    	<span style="color: #ff0000">connectionString</span>=<span style="color: #0000ff">"metadata=



  
    
    		res://*/BidNowDataContext.csdl|res://*/BidNowDataContext.ssdl|



  
    
    		res://*/BidNowDataContext.msl;



  
    
    	provider=System.Data.SqlClient;



  
    
    	provider connection string=&quot;



  
    
    		Server=tcp:SERVERNAME.database.windows.net;Database=BidNow;



  
    
    		User ID=USERNAME@SERVERNAME;Password=PASSWORD;



  
    
    		Trusted_Connection=False;Encrypt=True;&quot;"</span>



  
    
    	<span style="color: #ff0000">providerName</span>=<span style="color: #0000ff">"System.Data.EntityClient"</span> <span style="color: #0000ff">/></span>









    
… to …










  
    
    <span style="color: #0000ff"><</span><span style="color: #800000">add</span> <span style="color: #ff0000">name</span>=<span style="color: #0000ff">"BidNowDataContext"</span>



  
    
    	<span style="color: #ff0000">connectionString</span>=<span style="color: #0000ff">"metadata=



  
    
    		res://*/BidNowDataContext.csdl|res://*/BidNowDataContext.ssdl|



  
    
    		res://*/BidNowDataContext.msl;



  
    
    	provider=System.Data.SqlClient;



  
    
    	provider connection string=&quot;



  
    
    		Server=tcp:SERVERNAME.database.windows.net;Database=BidNow;



  
    
    		User ID=USERNAME@SERVERNAME;Password=PASSWORD;



  
    
    		Trusted_Connection=False;Encrypt=True;trustServerCertificate=true;



  
    
    		&quot;"</span>



  
    
    	<span style="color: #ff0000">providerName</span>=<span style="color: #0000ff">"System.Data.EntityClient"</span> <span style="color: #0000ff">/></span>









    
… and voilà! It worked!





Turns out that when Encryt=True and TrustServerCertificate=False, the driver will attempt to validate the SQL Server SSL certificate. By setting the property TrustServerCertificate=True the driver will not validate the SQL Server SSL certificate.





Of course, once I learned tried this I came across an article on MSDN called [How to: Connect to SQL Azure Using ADO.NET](http://msdn.microsoft.com/en-us/library/ee336243.aspx) to says to set the TrustServerCertificate property to False and the Encrypt property to True to prevent any man-in-the-middle attacks, so I guess I should include the following disclaimer: _**Use at your own risk!**_
