---
author: admin
categories:
- Toolkit
- Windows Azure
- Windows Phone 7
comments: true
date: "2011-05-25T02:10:58Z"
slug: deploying-your-services-from-the-windows-azure-toolkit-for-windows-phone-7
title: Deploying Your Services from the Windows Azure Toolkit for Windows Phone 7
wordpress_id: 1259
---

One of the most common requests I’ve received for the [Windows Azure Toolkit for Windows Phone 7](http://watoolkitwp7.codeplex.com/) has been a tutorial for deploying the services to Windows Azure. It’s taken longer than it should have—my humblest apologies—but I’ve finally put one together. You’ll need to follow the following steps outlined below:

 

  
  * Create a Certificate (used for SSL) 
   
  * Export the PFX File 
   
  * Upload the PFX File to Windows Azure 
   
  * (optional) Supporting Apple Push Notifications 
   
  * Update and Deploy to Windows Azure 
   
  * Update Your Windows Phone Project 
 

If you have any problems or confusion regarding the steps below, please let me know.

****  

**Create a Certificate**

 

  
  1. Open the **Visual Studio Command Prompt** window as an administrator. 
   
  2. Change the directory to the location where you want to save the certificate file. 
   
  3. Type the following command, making sure to replace <_CertificateName>_ with your hosted service URL:       

      

        
    
    makecert -sky exchange -r -n "CN=<span style="color: #0000ff"><</span><span style="color: #800000">CertificateName</span><span style="color: #0000ff">></span>" -pe -a sha1 -len 2048 -ss My "<span style="color: #0000ff"><</span><span style="color: #800000">CertificateName</span><span style="color: #0000ff">></span>.cer"


      


    



    

For example:



    

![image](https://wadewegner.blob.core.windows.net/wordpress/2011/05/image24.png)



    

**Note:** The **makecert** tool will both create the .cer file and register the certificate in your Personal Certificates store. For more information, check the following article: [http://msdn.microsoft.com/en-us/library/gg432987.aspx](http://msdn.microsoft.com/en-us/library/gg432987.aspx).


  





****





**Export the PFX File**






  
  1. Open the Certificate Manager snap-in for the management console by typing **certmgr.msc** in the **Start** menu textbox.

      
![4](https://wadewegner.blob.core.windows.net/wordpress/2011/05/4.png)


  
  2. The new certificate was automatically added to the personal certificate store. Export the certificate by right-clicking it, pointing to **All Tasks**, and then clicking **Export**. 


  
  3. On the **Export Private Key** page, make sure to select **Yes, export the private key**. 

    

![image](https://wadewegner.blob.core.windows.net/wordpress/2011/05/image17.png)


  


  
  4. Choose a name and export the file to a .pfx file. 
    

![image](https://wadewegner.blob.core.windows.net/wordpress/2011/05/image18.png)


  


  
  5. Finish the wizard. 
    

**Note:** You now have a copy of the certificate (.pfx) with the private key. For more information, check the following article: [http://msdn.microsoft.com/en-us/library/gg432987.aspx](http://msdn.microsoft.com/en-us/library/gg432987.aspx).


  





****





**Upload the PFX File to Windows Azure**






  
  1. Open a browser, navigate to the Windows Azure Platform Management Portal at [https://windows.azure.com](https://windows.azure.com) and log in with your credentials. 


  
  2. Select **Hosted Services, Storage Accounts & CDN** and then **Hosted Services**. 


  
  3. Expand your hosted service project, and select the **Certificates** folder. 

    

![image](https://wadewegner.blob.core.windows.net/wordpress/2011/05/image19.png)


  


  
  4. Click **Add Certificate**. 


  
  5. Click **Browse** and select the PFX file you saved. Enter the password, and click **Create**. 

    

![image](https://wadewegner.blob.core.windows.net/wordpress/2011/05/image20.png)


  


  
  6. Once the certificate has been uploaded and created, copy the **Thumbprint** for later use. 

    

![image](https://wadewegner.blob.core.windows.net/wordpress/2011/05/image21.png)


  





**Supporting Apple Push Notifications**





During the project creation through the project template, you are given the option to support Apple Push Notifications through the Azure Web role.





![image](https://wadewegner.blob.core.windows.net/wordpress/2011/05/image22.png)





If you chose to support Apple Push Notifications, please follow these additional steps:






  
  1. In order to use the toolkit to send Apple push notifications, you should have obtained the appropriate SSL certificate for your iOS application. You can find more information on how to obtain an Apple development certificate in the [Provisioning and Development](http://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/ProvisioningDevelopment/ProvisioningDevelopment.html) article in the [Local and Push Notification Programming Guide](http://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Introduction/Introduction.html). 


  
  2. Upload the same Apple development certificate selected during the project template wizard to your hosted service. To do this, you can follow the same steps required to upload the SSL certificate described in the previous section. 





**Update and Deploy to Windows Azure**






  
  1. In the Windows Azure Project, double-click the role (e.g. WP7CloudApp1.Web) to open the role properties. 


  
  2. Select the **Settings** tab and update the **DataConnectionString** and **Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString** to point to your production storage account. 


  
  3. Select the **Certificates** tab, and set the **SslCertificate** property value to the **Thumbprint** you saved from the Windows Azure portal. This will replace the thumbprint for the localhost certificate with the thumbprint related to the certificate you uploaded into your hosted service. 


  
  4. Add the .cer file to your web application project. This way, you can point your phone users to download the certificate and install into the emulator. Right-click on the Web application project, choose Add Existing Item, select the .cer file you previously created, and add it to the project. 


  
  5. Open the .cer file properties and make sure that the **Build Action** is set to **Content**. 

    

![image](https://wadewegner.blob.core.windows.net/wordpress/2011/05/image23.png)


  


  
  6. Now you can publish your Windows Azure project to your hosted service. You can do this from the Windows Azure Platform Management Portal or directly from Visual Studio 2010. 





**Update Your Windows Phone Project to Consume Your Windows Azure Hosted Service**






  
  1. In your Windows Phone project, open the **App.xaml** file. 


  
  2. Update the following resources: 
    


        


          

            

**Key**


          


          

            

**Value**


          

        

        


          

            

**SSLCertificateUrl**


          


          

            

http://**<YourDNSPrefix>**.cloudapp.net:10080/**<CertificateName>**.cer


          

        

        


          

            

**SharedAccessSignatureServiceEndpoint**


          


          

            

https://** <YourDNSPrefix>**.cloudapp.net/SharedAccessSignatureService


          

        

        


          

            

**AzureStorageTableProxyEndpoint**


          


          

            

https://** <YourDNSPrefix>**.cloudapp.net/AzureTablesProxy.axd


          

        

        


          

            

**AzureStorageQueueProxyEndpoint**


          


          

            

https://** <YourDNSPrefix>**.cloudapp.net/AzureQueuesProxy.axd


          

        

        


          

            

**AuthenticationServiceEndpoint**


          


          

            

https://** <YourDNSPrefix>**.cloudapp.net/AuthenticationService


          

        

        


          

            

**PushNotificationServiceEndpoint**


          


          

            

https://** <YourDNSPrefix>**.cloudapp.net/PushNotificationService


          

        
      
  


  
  3. To test against the deployed services, right-click the Windows Phone project, navigate to the **Debug** context menu, and click on **Start new instance**. 





I hope this helps!
