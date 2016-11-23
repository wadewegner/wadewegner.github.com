---
author: admin
comments: true
date: 2010-11-25 05:00:32+00:00
layout: post
slug: using-the-saml-credentialtype-to-authenticate-to-the-service-bus
title: Using the SAML CredentialType to Authenticate to the Service Bus
wordpress_id: 815
categories:
- Access Control
- SAML
- Service Bus
- Windows Azure
- Windows Azure AppFabric
---

The [Windows Azure AppFabric Service Bus](http://msdn.microsoft.com/en-us/library/ee732537.aspx) uses a class called [TransportClientEndpointBehavior](http://msdn.microsoft.com/en-us/library/microsoft.servicebus.transportclientendpointbehavior.aspx) to specify the credentials for a particular endpoint. There are four options available to you: Unauthenticated, SimpleWebToken, SharedSecret, and SAML. For details, take a look at the [CredentialType](http://msdn.microsoft.com/en-us/library/dd582752.aspx) member. The first three are pretty well described and documented – in fact, if you’ve spent any time at all looking at the Service Bus, you’ve probably seen the following code:

 

SharedSecret Credential Type

  1. TransportClientEndpointBehavior relayCredentials = new TransportClientEndpointBehavior();
  2. relayCredentials.CredentialType = TransportClientCredentialType.SharedSecret;
  3. relayCredentials.Credentials.SharedSecret.IssuerName = issuerName;
  4. relayCredentials.Credentials.SharedSecret.IssuerSecret = issuerSecret;

 

    
This code specifies that the client credential is provided as a self-issued shared secret that is registered and managed by [Access Control](http://msdn.microsoft.com/en-us/library/ee732536.aspx), along with the Issuer Name and Issuer Secret that you can grab (or generate) in the AppFabric portal. Quick and easy, but only marginally better than a username/password.

 

The use of a SAML token is more interesting, and in fact lately I’ve seen a number of requests for a sample. Unfortunately, it doesn’t appear that anyone has ever created a sample on how this should work. It’s one thing to say that the above code would change to the following …

 

Saml Credential Type

  1. TransportClientEndpointBehavior samlCredentials = new TransportClientEndpointBehavior();
  2. samlCredentials.CredentialType = TransportClientCredentialType.Saml;

   
   
 

… and something else to provide a sample that works. What makes this challenging is the creation of the SAML token – this is a complex process, and difficult to setup, test, or fake. Fortunately for you, I have put together a sample on how to use a SAML token as the CredentialType of the TransportClientEndpointBehavior.

 

**DISCLAIMER**: I am not an identity expert. You should look to people like [Justin Smith](http://blogs.msdn.com/b/justinjsmith/), [Vittorio Bertocci](http://blogs.msdn.com/b/vbertocci/), [Hervey Wilson](http://www.dynamic-cast.com/), and [Maciej ("Ski") Skierkowski's](http://blogs.msdn.com/b/sskier/) for specific identity guidance. I’m simply leveraging much of the material, samples, and code they’ve written in order to enable this particular scenario.

 

In order to get this sample to work I’ve a number of frameworks. You’ll need to download and install these to get everything to work properly.

 

  
  * [.NET 4.0 Framework](http://www.microsoft.com/net/) – I’ve only tested this with Visual Studio 2010 and the .NET 4.0 Framework. 
   
  * [Windows Azure AppFabric SDK V1.0](http://www.microsoft.com/downloads/en/details.aspx?FamilyID=39856a03-1490-4283-908f-c8bf0bfad8a5&displaylang=en) – this SDK provides you with all the assemblies you’ll need in order to use the Service Bus. 
   
  * [Windows Identity Foundation](http://www.microsoft.com/downloads/en/details.aspx?FamilyID=eb9c345f-e830-40b8-a5fe-ae7a864c4d76&displaylang=en) – WIF allows you to externalize use access from applications via claims (which is what we want to do). 
   
  * [Windows Identity Foundation SDK](http://www.microsoft.com/downloads/en/details.aspx?familyid=C148B2DF-C7AF-46BB-9162-2C9422208504&displaylang=en) – the WIF SDK provides you with templates and code samples to get going. Technically I don’t think we’re using this, but I have it installed and I’m not going to uninstall it to test. 
 

I also used a number of samples in order to build this sample. You won’t need to download and install these in order to use this sample – I’m providing the source of the modified code below – but you might want to take a look at them anyways.

 

  
  * [Windows Azure AppFabric SDK Samples (C#)](http://www.microsoft.com/downloads/en/details.aspx?FamilyID=39856a03-1490-4283-908f-c8bf0bfad8a5&displaylang=en) – specifically, I used the Service Bus **EchoSample** solution and Access Control **AcmBrowser **application. 
   
  * [Fabrikam Shipping SaaS Demo](http://code.msdn.microsoft.com/fshipsaassource) – this is a great sample application, and I suggest you take a look. I personally used the SelfSTS and TestClient solutions – more on this in a moment. 
 

Okay, now that we’ve got the various components and samples described, let’s get to the sample.

 

**_Overview_**

 

When I first started writing this post, I thought it’d be short and sweet. As you can see, it’s anything but. However, when you look through everything, I think you’ll see that there’s quite a bit to this sample.

 

There are three major pieces in this sample:

 

  
  1. Using a security token service (STS) to generate a SAML token. 
   
  2. Configuring the Access Control service to accept the SAML token and authorize our WCF service to Send or Listen across the Service Bus. 
   
  3. Modifying the EchoSample WCF service to get a SAML token and send it to Access Control. 
 

Let’s walk through the pieces.

 

**_The Security Token Service_**

 

Remember my disclaimer above? Since I’m not an identity guy, I decided to use a tool built by an identity guy to act as my STS. [Vittorio](http://blogs.msdn.com/b/vbertocci/archive/2010/08/23/selfsts-when-you-need-a-saml-token-now-right-now.aspx) has created a fabulous tool called [SelfSTS](http://code.msdn.microsoft.com/SelfSTS), which exposes a minimal WS-Federation STS endpoint. In the [Fabrikam Shipping SaaS Demo Source Code](http://code.msdn.microsoft.com/fshipsaassource), he provided an updated version of the SelfSTS tool that will also issue via WS-Trust, along with a sample on how to acquire the token and send it to ACS.

 

If you download FabrikamShippingSaaS and install it in the default location, you can find these two assets in the following locations:

 

  
  * Updated SelfSTS: C:FabrikamShippingSaaSassetsSelfSTSSelfSTS.sln
   
  * Token acquisition and ACS submittal example: C:FabrikamShippingSaaScodeTestClient.sln
 

Definitely take a look at these resources. I highly encourage you to use these as much and as often as you like – the SelfSTS tool is invaluable when working with Access Control and WIF. In the TestClient solution, the method we’re most interested in is the GetSAMLToken method found in the Program.cs file. Take a look at it, but realize that we’re going to make a few changes to it for our example.

 

For the purposes of this sample, I took the SelfSTS project and included it in the EchoSampleSAML solution that you can find below. The only update I made to the tool was to extract a method I called Start – which encapsulates all of the logic needed to start the tool – and call it in the Main method.

 

Go ahead and run the SelfSTS project in the EchoSampleSAML solution below by right-clicking and choosing Debug –> Start New Instance. It looks like this:

 

![SelfSTS](https://wadewegner.blob.core.windows.net/wordpress/2010/11/image1.png)

 

There are two items that we’ll leverage below:

 

  
  * The Endpoint: [http://localhost:8000/STS/Issue/](http://localhost:8000/STS/Issue/)
   
  * The “Metadata” URL: [http://localhost:8000/STS/FederationMetadata/2007-06/FederationMetadata.xml](http://localhost:8000/STS/FederationMetadata/2007-06/FederationMetadata.xml)
 

Quickly stop the SelfSTS and click “Edit Claim Types and Values”.

 

![Edit Claims](https://wadewegner.blob.core.windows.net/wordpress/2010/11/EditClaims.png)

 

These are the claims that the SelfSTS will return. I’ve highlighted “joe” because we’re going to use this claim below when configuring Access Control. Make a mental note, because I want you to be aware of where “joe” comes from.

 

Go ahead and keep this tool running the entire time – you’ll leverage it throughout this process.

 

**_Configuring Access Control_**

 

The production Access Control services does not provide a UI in the portal to configure issuers and rules. Consequently, we’ll use the AcmBrowser tool found within the Windows Azure AppFabric SDK Samples. Depending on where you unzip the samples, you’ll find it here:

 

  

C:WindowsAzureAppFabricSDKSamples_V1.0-CSAccessControlExploringFeaturesManagementAcmBrowser

 

When you open the solution you’ll have to upgrade it for Visual Studio 2010. Start the application. You’ll see something like the following:

 

![AcmBrowser](https://wadewegner.blob.core.windows.net/wordpress/2010/11/AcmBrowser.png)

 

This is a very handy tool, but unfortunately it takes a little time to get used to it. The very first thing to do is enter your Service Namespace and Management Key (see A above). You can get these off of the AppFabric portal (i.e. [http://appfabric.azure.com](http://appfabric.azure.com)) in the following places:

 

![Service Namespace and Management Key](https://wadewegner.blob.core.windows.net/wordpress/2010/11/ServiceNamespace.png)

 

**Note:** Make sure to put a “-sb” at the end of your Service Namespace in the AcmBrowser tool. This is because the tool calls the Access Control Management Endpoint, which by convention is your service namespace with a “-sb” appended to it.

 

Once you’ve entered these values, click the “Load from Cloud” button (see B above). This will grab your existing settings and load them into the Resources list. I recommend you click the “Save” button (see C above) to save a copy of them locally – this way you’ll have something to revert to if you make a mistake.

 

Okay, now that you have the basics loaded into AcmBrowser, here are the steps to setup Access Control:

 

  
  1. Right-click “Issuers” and click “Create”. Enter the following values and press OK.             
    * Display Name: SelfSTS 
       
    * Algorithm: FedMetadata 
       
    * Endpoint URI: [http://localhost:8000/STS/FederationMetadata/2007-06/FederationMetadata.xml](http://localhost:8000/STS/FederationMetadata/2007-06/FederationMetadata.xml) (from the SelfSTS)             
           
![image](https://wadewegner.blob.core.windows.net/wordpress/2010/11/image2.png)             

       
   
  2. Expand Scopes –> ServiceBus Scope for … –> Rules. 
   
  3. Create a rule that will allow “joe” to Send messages to the Service Bus endpoint (remember “joe” from above?). Right-click “Rules” and click “Create”. Enter the following values and press OK.             
    * Display Name: test send 
       
    * Type: Simple 
       
    * Input Claim                     
      * Issuer: SelfSTS 
           
      * Type: [http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name](http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name)
           
      * Value: joe 
               
       
    * Output Claim                     
      * Type: net.windows.servicebus.action 
           
      * Value: Send                
               
![test send](https://wadewegner.blob.core.windows.net/wordpress/2010/11/image3.png)                 

               
       
   
  4. Create a rule that will allow “joe” to Send messages to the Service Bus endpoint (remember “joe” from above?). Right-click “Rules” and click “Create”. Enter the following values and press OK.             
    * Display Name: test listen 
       
    * Type: Simple 
       
    * Input Claim                     
      * Issuer: SelfSTS 
           
      * Type: [http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name](http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name)
           
      * Value: joe 
               
       
    * Output Claim                     
      * Type: net.windows.servicebus.action 
           
      * Value: Listen                
               
![test listen](https://wadewegner.blob.core.windows.net/wordpress/2010/11/image4.png)                 

               
       
   
  5. Now you need to update Access Control. Unfortunately, AcmBrowser doesn’t update Access Control, so you have to clear then save.             
    * Click the “Clear Service Namespace in Cloud” button (see D above). 
       
    * Click the “Save to Cloud” button (see E above). 
       
 

That’s it for Access Control. We’ve setup the SelfSTS as a trusted Issuer, and we’ve defined two rules – Send and Listen – for the claim “name” with a value of “joe”.

 

**_Modifying the EchoSample to Use _**

 

The rest of this sample is really pretty straightforward – we’ve either already done most of the work, or we’ll leverage a few code snippets from different resources. The first thing we’ll do is add a GetSAMLToken method to the Service project in the EchoSample. Add the following method in the Program.cs file:

 

Getting the SAML Token

	  1. private static string GetSAMLToken(string identityProviderUrl, string identityProviderCertificateSubjectName, string username, string password, string appliesTo)
	  2. {
	  3.     GenericXmlSecurityToken token = null;
	
	  5.     var binding = new WS2007HttpBinding();
	
	  7.     binding.Security.Mode = SecurityMode.Message;
	  8.     binding.Security.Message.ClientCredentialType = MessageCredentialType.UserName;
	  9.     binding.Security.Message.EstablishSecurityContext = false;
	  10.     binding.Security.Message.NegotiateServiceCredential = true;
	
	  12.     using (var trustChannelFactory = new WSTrustChannelFactory(binding, new EndpointAddress(new Uri(identityProviderUrl), new DnsEndpointIdentity(identityProviderCertificateSubjectName), new AddressHeaderCollection())))
	  13.     {
	  14.         trustChannelFactory.Credentials.UserName.UserName = username;
	  15.         trustChannelFactory.Credentials.UserName.Password = password;
	  16.         trustChannelFactory.Credentials.ServiceCertificate.Authentication.CertificateValidationMode = X509CertificateValidationMode.None;
	
	  18.         trustChannelFactory.TrustVersion = TrustVersion.WSTrust13;
	
	  20.         WSTrustChannel channel = null;
	
	  22.         try
	  23.         {
	  24.             var rst = new RequestSecurityToken(WSTrust13Constants.RequestTypes.Issue);
	  25.             rst.AppliesTo = new EndpointAddress(appliesTo);
	  26.             rst.KeyType = KeyTypes.Bearer;
	
	  28.             channel = (WSTrustChannel)trustChannelFactory.CreateChannel();
	  29.             token = channel.Issue(rst) as GenericXmlSecurityToken;
	  30.             ((IChannel)channel).Close();
	  31.             channel = null;
	
	  33.             trustChannelFactory.Close();
	
	  35.             string tokenString = token.TokenXml.OuterXml;
	  36.             return tokenString;
	  37.         }
	  38.         finally
	  39.         {
	  40.             if (channel != null)
	  41.             {
	  42.                 ((IChannel)channel).Abort();
	  43.             }
	
	  45.             if (trustChannelFactory != null)
	  46.             {
	  47.                 trustChannelFactory.Abort();
	  48.             }
	  49.         }
	  50.     }
	  51. }

 

    
This method is almost identical to the GetSAMLToken found in FabrikamShippingSaaS’s TestClient. Here are the changes I made:

 

  
  * Line 1: I changed the return type from SecurityToken to string. This is because the TransportClientEndpointBehavior will want the SAML token as a string. 
   
  * Lines 5-10: I moved the code from the GetSecurityTokenServiceBinding method into the GetSamleToken method. This is simply for readability in this blog post. 
   
  * Line 32: I removed the following code: rst.Entropy = new Entropy(CreateEntropy());
   
  * Line 35: Added this line to grab the token out of the GenericXmlSecurityToken object. 
   
  * Line 36: Returning the tokenString instead of the token.          
 

Now, there are a few values that we’re going to start in the App.config file. Here they are:

 

App.Config file

	  1. <appSettings>
	  2.   <add key="identityProviderCertificateSubjectName" value="adventureWorks" />
	  3.   <add key="identityProviderUrl" value="http://localhost:8000/STS/Username" />
	  4.   <add key="appliesTo" value="https://{0}-sb.accesscontrol.windows.net/WRAPv0.9" />
	  5.   <add key="serviceNamespace" value="YOURSERVICENAMESPACE" />
	  6.   7.   <!-- this is the user and pwd of the user in AdventureWorks IdP -->
	  8.   <add key="username" value="joe" />
	  9.   <add key="password" value="p@ssw0rd" />
	  10. </appSettings>

 

    
Replace **YOURSERVICENAMESPACE** with your actual service namespace.

 

Now, let’s create some static variables for these values in the Program.cs file:

 

Static Variables

	  1. static string identityProviderUrl = ConfigurationManager.AppSettings["identityProviderUrl"];
	  2. static string identityProviderCertificateSubjectName = ConfigurationManager.AppSettings["identityProviderCertificateSubjectName"];
	  3. static string username = ConfigurationManager.AppSettings["username"];
	  4. static string password = ConfigurationManager.AppSettings["password"];
	  5. static string serviceNamespace = ConfigurationManager.AppSettings["serviceNamespace"];
	  6. static string appliesTo = String.Format(ConfigurationManager.AppSettings["appliesTo"], serviceNamespace);

 

    
Now, we’re going to change a number of things within the Main method. First, remove the Console.Write() and Console.ReadLine() sections – we’re grabbing those out of the App.Config file. Next, we need to grab the SAML token. Write the following line:

 

Code Snippet

	  1. string samlToken = GetSAMLToken(identityProviderUrl, identityProviderCertificateSubjectName, username, password, appliesTo);

 

    
This will call the GetSAMLToken method and pass along all the values required in order to generate the SAML token. When this runs, it will call out to SelfSTS and pass along the username/password specified in the App.Config file and, in return, get a SAML token that includes all the claims we want – including the “name”.

 

Now, we have to take this SAML token as use it with the TransportClientEndpointBehavior. Remove the existing TransportClientEndpointBehavior, and replace it with the following code:

 

Code Snippet

	  1. TransportClientEndpointBehavior samlCredentials = new TransportClientEndpointBehavior();
	  2. samlCredentials.CredentialType = TransportClientCredentialType.Saml;
	  3. samlCredentials.Credentials.Saml.SamlToken = samlToken;

 

    
We’re now creating a new TransportClientEndpointBehavior with the CredentialType of TransportClientCredentialType.Saml, and setting the SamlToken property to our samlToken. Mission accomplished! (Note: since I changed the variable name to samlCredentials, but sure and update the code below where the TransportClientEndpointBehavior is added as a behavior.)

 

Feels good. Now all we have to do is run it!

 

**_Wrapping Up_**

 

Alright, so now we’re ready to run it. Grab the source code below. When you run it, be sure and update YOURSERVICENAMESPACE (two instances) and YOURISSUERSECRET (one instance). Also, be sure to start the projects in the following order:

 

  
  1. SelfSTS – we need to make sure our STS is running first. 
   
  2. Service – when this starts it will grab the SAML token from SelfSTS, then connect to the Service Bus endpoint as a listener. 
   
  3. Client – when this starts it will use the SharedSecret to connect to the Service Bus as a sender. 
 

When the Service starts and successfully connects as a listener, it should look like this:

 

![image](https://wadewegner.blob.core.windows.net/wordpress/2010/11/image5.png)

 

After you startup the Client and echo some text, you’ll see the text reflected in the console window:

 

![image](https://wadewegner.blob.core.windows.net/wordpress/2010/11/image6.png)

 

And that’s it!

 

Now, the whole purpose of this is to highlight how to leverage the SAML token within the TransportClientEndpointBehavior class. Obviously you’ll want to use something like ADFS 2.0 and not the SelfSTS in a production, but fortunately I can refer you to my **disclaimer** above and wish you good luck!

 

Hope this helps!

 

Special thanks to [Vittorio Bertocci](http://blogs.msdn.com/b/vbertocci/) and [Maciej ("Ski") Skierkowski](http://blogs.msdn.com/b/sskier/) for their help, and [Scott Seely](http://www.scottseely.com/Blog.aspx) for asking the question that got me to pull this together.
