---
author: admin
comments: true
date: 2010-05-12 03:56:48+00:00
layout: post
slug: host-wcf-services-in-iis-with-service-bus-endpoints
title: Host WCF Services in IIS with Service Bus Endpoints
wordpress_id: 627
categories:
- .NET 4.0
- Azure AppFabric
- Cloud
- Service Bus
- WCF
---

**Update:** To see how to leverage Windows Azure AppFabric Autostart to activate the WCF Service, please see [AutoStart WCF Services to Expose them as Service Bus Endpoints](http://www.wadewegner.com/2010/08/autostart-wcf-services-to-expose-them-as-service-bus-endpoints/).

  
Vishal Chowdhary, a Senior Test Lead on the Azure AppFabric team, recently posted a whitepaper on [hosting WCF services with Service Bus endpoints from IIS](http://code.msdn.microsoft.com/ServiceBusDublinIIS). This whitepaper provides two solutions to a (previously) significant challenge in hosting WCF services in IIS that connect to the Azure AppFabric Service Bus.

The primary challenge is activation. As Vishal writes, “For the on-premise WCF service to start receiving messages from the Service Bus in the cloud (aka Relay Service), the on-premises service opens an outbound port and creates a bidirectional socket for communication. It connects to the Service Bus, authenticates itself, and starts listening to calls from the relay service before the client sends its requests.” He goes on to say that “IIS/WAS relies on message-based activation & will launch the host only after the first request comes in.” Consequently, **until the first message is received by IIS the service will never establish a connection to the Service Bus**; with no connection to the Service Bus, **it will never receive a message**. A bit of a dilemma.

In the whitepaper, Vishal points out two ways to resolve this challenge:

  * IIS Application Warm-Up  
  * ASP.NET 4.0 Auto-Start 

In this post, I’m going to highlight exactly how to go about using **IIS Application Warm-Up to get a WCF service hosted in IIS 7.5 to receive messages from the Service Bus**. This post borrows heavily from Visha's whitepaper; I strongly suggest you spend the time to read the entire paper.

  1. If you’re using .NET 4.0, you must [setup .NET 4.0 to work with the Azure AppFabric SDK](http://www.wadewegner.com/2010/05/net-framework-4-0-and-the-azure-appfabric-sdk/).  
  2. Create a new ASP.NET Web Application project called **EchoSample** in Visual Studio 2010 using .NET 4.0.  
  3. To validate this approach, we want this project hosted in IIS. Right-click the project and choose Properties. Select the Web tab, and switch from **Use Visual Studio Development Server** to **Use Local IIS Web server** and click **Create Virtual Directory.**  
  4. Add the Microsoft.ServiceBus reference from the “C:%Program Files%Windows Azure platform AppFabric SDKV1.0Assemblies” folder.  
  5. You have to create a custom BehaviorExtensionElement for the ServiceRegistrySettings to make the discoverability policy ‘Public’ in the configuration file. Consequently, we need to create a class that we’ll call MyServiceRegistrySettingsElement that inherits the BehaviorExtensionElement. 
    
    <span style="color: #0000ff">public</span> <span style="color: #0000ff">class</span> MyServiceRegistrySettingsElement : BehaviorExtensionElement
    
    
    {
    
    
        <span style="color: #0000ff">public</span> <span style="color: #0000ff">override</span> Type BehaviorType
    
    
        {
    
    
            <span style="color: #0000ff">get</span> { <span style="color: #0000ff">return</span> <span style="color: #0000ff">typeof</span>(ServiceRegistrySettings); }
    
    
        }
    
    
        <span style="color: #0000ff">protected</span> <span style="color: #0000ff">override</span> <span style="color: #0000ff">object</span> CreateBehavior()
    
    
        {
    
    
            <span style="color: #0000ff">return</span> <span style="color: #0000ff">new</span> ServiceRegistrySettings()
    
    
            {
    
    
                DiscoveryMode = <span style="color: #0000ff">this</span>.DiscoveryMode,
    
    
                DisplayName = <span style="color: #0000ff">this</span>.DisplayName
    
    
            };
    
    
        }
    
    
        [ConfigurationProperty("<span style="color: #8b0000">discoveryMode</span>", DefaultValue = DiscoveryType.Private)]
    
    
        <span style="color: #0000ff">public</span> DiscoveryType DiscoveryMode
    
    
        {
    
    
            <span style="color: #0000ff">get</span> { <span style="color: #0000ff">return</span> (DiscoveryType)<span style="color: #0000ff">this</span>["<span style="color: #8b0000">discoveryMode</span>"]; }
    
    
            <span style="color: #0000ff">set</span> { <span style="color: #0000ff">this</span>["<span style="color: #8b0000">discoveryMode</span>"] = <span style="color: #0000ff">value</span>; }
    
    
        }
    
    
        [ConfigurationProperty("<span style="color: #8b0000">displayName</span>")]
    
    
        <span style="color: #0000ff">public</span> <span style="color: #0000ff">string</span> DisplayName
    
    
        {
    
    
            <span style="color: #0000ff">get</span> { <span style="color: #0000ff">return</span> (<span style="color: #0000ff">string</span>)<span style="color: #0000ff">this</span>["<span style="color: #8b0000">displayName</span>"]; }
    
    
            <span style="color: #0000ff">set</span> { <span style="color: #0000ff">this</span>["<span style="color: #8b0000">displayName</span>"] = <span style="color: #0000ff">value</span>; }
    
    
        }
    
    
    }






  6. Now, let’s add a new WCF Service called **EchoService** to the project. Remove the existing method in the ServiceContract and create the following GetData method in the IEchoService.cs file. 


    
    [OperationContract]
    
    
    <span style="color: #0000ff">string</span> GetData(<span style="color: #0000ff">int</span> <span style="color: #0000ff">value</span>);






  7. Also, update the EchoService.svc.cs with the implementation of the GetData method. 


    
    <span style="color: #0000ff">public</span> <span style="color: #0000ff">string</span> GetData(int value)
    
    
    {
    
    
        <span style="color: #0000ff">if</span> (value < 0)
    
    
            <span style="color: #0000ff">throw</span> <span style="color: #0000ff">new</span> ApplicationException("<span style="color: #8b0000">Negative values not allowed!!!</span>");
    
    
        Thread.Sleep(value);
    
    
        <span style="color: #0000ff">return</span> <span style="color: #0000ff">string</span>.Format("<span style="color: #8b0000">You entered: {0}</span>", value);
    
    
    }






  8. Now we need to update the web.config settings. This is fairly extensive. Be sure and replace YOUR_NAMESPACE, YOUR_ISSUER_NAME, and YOUR_ISSUER_SECRET with your own values. 


    
    <span style="color: #0000ff"><</span><span style="color: #800000">system.serviceModel</span><span style="color: #0000ff">></span>
    
    
      <span style="color: #0000ff"><</span><span style="color: #800000">extensions</span><span style="color: #0000ff">></span>
    
    
        <span style="color: #0000ff"><</span><span style="color: #800000">behaviorExtensions</span><span style="color: #0000ff">></span>
    
    
          <span style="color: #0000ff"><</span><span style="color: #800000">add</span> <span style="color: #ff0000">name</span>=<span style="color: #0000ff">"ServiceRegistrySettings"</span>
    
    
                <span style="color: #ff0000">type</span>=<span style="color: #0000ff">"EchoSample.MyServiceRegistrySettingsElement, EchoSample,
    
    
                      Version=1.0.0.0, Culture=neutral, PublicKeyToken=null"</span> <span style="color: #0000ff">/></span>
    
    
        <span style="color: #0000ff"></</span><span style="color: #800000">behaviorExtensions</span><span style="color: #0000ff">></span>
    
    
      <span style="color: #0000ff"></</span><span style="color: #800000">extensions</span><span style="color: #0000ff">></span>
    
    
      <span style="color: #0000ff"><</span><span style="color: #800000">services</span><span style="color: #0000ff">></span>
    
    
        <span style="color: #0000ff"><</span><span style="color: #800000">clear</span> <span style="color: #0000ff">/></span>
    
    
        <span style="color: #0000ff"><</span><span style="color: #800000">service</span> <span style="color: #ff0000">behaviorConfiguration</span>=<span style="color: #0000ff">"MyServiceTypeBehavior"</span>
    
    
                  <span style="color: #ff0000">name</span>=<span style="color: #0000ff">"EchoSample.EchoService"</span><span style="color: #0000ff">></span>
    
    
          <span style="color: #0000ff"><</span><span style="color: #800000">endpoint</span> 
    
    
             <span style="color: #ff0000">address</span>=<span style="color: #0000ff">"http://localhost/EchoSample/EchoService.svc/LocalEchoService"</span>
    
    
             <span style="color: #ff0000">binding</span>=<span style="color: #0000ff">"basicHttpBinding"</span>
    
    
             <span style="color: #ff0000">bindingConfiguration</span>=<span style="color: #0000ff">"BasicHttpConfig"</span>
    
    
             <span style="color: #ff0000">name</span>=<span style="color: #0000ff">"Basic"</span> <span style="color: #ff0000">contract</span>=<span style="color: #0000ff">"EchoSample.IEchoService"</span> <span style="color: #0000ff">/></span>
    
    
          <span style="color: #0000ff"><</span><span style="color: #800000">endpoint</span>
    
    
             <span style="color: #ff0000">address</span>=<span style="color: #0000ff">"https://YOUR_NAMESPACE.servicebus.windows.net/EchoServiceHttp/"</span>
    
    
             <span style="color: #ff0000">behaviorConfiguration</span>=<span style="color: #0000ff">"sharedSecretClientCredentials"</span>
    
    
             <span style="color: #ff0000">binding</span>=<span style="color: #0000ff">"basicHttpRelayBinding"</span>
    
    
             <span style="color: #ff0000">bindingConfiguration</span>=<span style="color: #0000ff">"HttpRelayEndpointConfig"</span>
    
    
             <span style="color: #ff0000">name</span>=<span style="color: #0000ff">"RelayEndpoint"</span>
    
    
             <span style="color: #ff0000">contract</span>=<span style="color: #0000ff">"EchoSample.IEchoService"</span> <span style="color: #0000ff">/></span>
    
    
          <span style="color: #0000ff"><</span><span style="color: #800000">endpoint</span>
    
    
             <span style="color: #ff0000">address</span>=<span style="color: #0000ff">"sb://YOUR_NAMESPACE.servicebus.windows.net/EchoServiceNetTcp/"</span>
    
    
             <span style="color: #ff0000">behaviorConfiguration</span>=<span style="color: #0000ff">"sharedSecretClientCredentials"</span>
    
    
             <span style="color: #ff0000">binding</span>=<span style="color: #0000ff">"netTcpRelayBinding"</span>
    
    
             <span style="color: #ff0000">bindingConfiguration</span>=<span style="color: #0000ff">"NetTcpRelayEndpointConfig"</span>
    
    
             <span style="color: #ff0000">name</span>=<span style="color: #0000ff">"RelayEndpoint"</span>
    
    
             <span style="color: #ff0000">contract</span>=<span style="color: #0000ff">"EchoSample.IEchoService"</span> <span style="color: #0000ff">/></span>
    
    
        <span style="color: #0000ff"></</span><span style="color: #800000">service</span><span style="color: #0000ff">></span>
    
    
      <span style="color: #0000ff"></</span><span style="color: #800000">services</span><span style="color: #0000ff">></span>
    
    
      <span style="color: #0000ff"><</span><span style="color: #800000">bindings</span><span style="color: #0000ff">></span>
    
    
        <span style="color: #0000ff"><</span><span style="color: #800000">basicHttpBinding</span><span style="color: #0000ff">></span>
    
    
          <span style="color: #0000ff"><</span><span style="color: #800000">binding</span> <span style="color: #ff0000">name</span>=<span style="color: #0000ff">"BasicHttpConfig"</span> <span style="color: #0000ff">/></span>
    
    
        <span style="color: #0000ff"></</span><span style="color: #800000">basicHttpBinding</span><span style="color: #0000ff">></span>
    
    
        <span style="color: #008000"><!--service bus binding--></span>
    
    
        <span style="color: #0000ff"><</span><span style="color: #800000">basicHttpRelayBinding</span><span style="color: #0000ff">></span>
    
    
          <span style="color: #0000ff"><</span><span style="color: #800000">binding</span> <span style="color: #ff0000">name</span>=<span style="color: #0000ff">"HttpRelayEndpointConfig"</span><span style="color: #0000ff">></span>
    
    
            <span style="color: #0000ff"><</span><span style="color: #800000">security</span> <span style="color: #ff0000">relayClientAuthenticationType</span>=<span style="color: #0000ff">"RelayAccessToken"</span> <span style="color: #0000ff">/></span>
    
    
          <span style="color: #0000ff"></</span><span style="color: #800000">binding</span><span style="color: #0000ff">></span>
    
    
        <span style="color: #0000ff"></</span><span style="color: #800000">basicHttpRelayBinding</span><span style="color: #0000ff">></span>
    
    
        <span style="color: #0000ff"><</span><span style="color: #800000">netTcpRelayBinding</span><span style="color: #0000ff">></span>
    
    
          <span style="color: #0000ff"><</span><span style="color: #800000">binding</span> <span style="color: #ff0000">name</span>=<span style="color: #0000ff">"NetTcpRelayEndpointConfig"</span><span style="color: #0000ff">></span>
    
    
            <span style="color: #0000ff"><</span><span style="color: #800000">security</span> <span style="color: #ff0000">relayClientAuthenticationType</span>=<span style="color: #0000ff">"RelayAccessToken"</span> <span style="color: #0000ff">/></span>
    
    
          <span style="color: #0000ff"></</span><span style="color: #800000">binding</span><span style="color: #0000ff">></span>
    
    
        <span style="color: #0000ff"></</span><span style="color: #800000">netTcpRelayBinding</span><span style="color: #0000ff">></span>
    
    
      <span style="color: #0000ff"></</span><span style="color: #800000">bindings</span><span style="color: #0000ff">></span>
    
    
      <span style="color: #0000ff"><</span><span style="color: #800000">behaviors</span><span style="color: #0000ff">></span>
    
    
        <span style="color: #0000ff"><</span><span style="color: #800000">endpointBehaviors</span><span style="color: #0000ff">></span>
    
    
          <span style="color: #0000ff"><</span><span style="color: #800000">behavior</span> <span style="color: #ff0000">name</span>=<span style="color: #0000ff">"sharedSecretClientCredentials"</span><span style="color: #0000ff">></span>
    
    
            <span style="color: #0000ff"><</span><span style="color: #800000">transportClientEndpointBehavior</span> <span style="color: #ff0000">credentialType</span>=<span style="color: #0000ff">"SharedSecret"</span><span style="color: #0000ff">></span>
    
    
              <span style="color: #0000ff"><</span><span style="color: #800000">clientCredentials</span><span style="color: #0000ff">></span>
    
    
                <span style="color: #0000ff"><</span><span style="color: #800000">sharedSecret</span> <span style="color: #ff0000">issuerName</span>=<span style="color: #0000ff">"YOUR_ISSUER_NAME"</span> 
    
    
                              <span style="color: #ff0000">issuerSecret</span>=<span style="color: #0000ff">"YOUR_ISSUER_SECRET"</span> <span style="color: #0000ff">/></span>
    
    
              <span style="color: #0000ff"></</span><span style="color: #800000">clientCredentials</span><span style="color: #0000ff">></span>
    
    
            <span style="color: #0000ff"></</span><span style="color: #800000">transportClientEndpointBehavior</span><span style="color: #0000ff">></span>
    
    
            <span style="color: #0000ff"><</span><span style="color: #800000">ServiceRegistrySettings</span> <span style="color: #ff0000">discoveryMode</span>=<span style="color: #0000ff">"Public"</span> <span style="color: #0000ff">/></span>
    
    
          <span style="color: #0000ff"></</span><span style="color: #800000">behavior</span><span style="color: #0000ff">></span>
    
    
        <span style="color: #0000ff"></</span><span style="color: #800000">endpointBehaviors</span><span style="color: #0000ff">></span>
    
    
        <span style="color: #0000ff"><</span><span style="color: #800000">serviceBehaviors</span><span style="color: #0000ff">></span>
    
    
          <span style="color: #0000ff"><</span><span style="color: #800000">behavior</span> <span style="color: #ff0000">name</span>=<span style="color: #0000ff">"MyServiceTypeBehavior"</span><span style="color: #0000ff">></span>
    
    
            <span style="color: #0000ff"><</span><span style="color: #800000">serviceMetadata</span> <span style="color: #ff0000">httpGetEnabled</span>=<span style="color: #0000ff">"true"</span> <span style="color: #0000ff">/></span>
    
    
            <span style="color: #0000ff"><</span><span style="color: #800000">serviceDebug</span> <span style="color: #ff0000">includeExceptionDetailInFaults</span>=<span style="color: #0000ff">"true"</span> <span style="color: #0000ff">/></span>
    
    
          <span style="color: #0000ff"></</span><span style="color: #800000">behavior</span><span style="color: #0000ff">></span>
    
    
        <span style="color: #0000ff"></</span><span style="color: #800000">serviceBehaviors</span><span style="color: #0000ff">></span>
    
    
      <span style="color: #0000ff"></</span><span style="color: #800000">behaviors</span><span style="color: #0000ff">></span>
    
    
    <span style="color: #0000ff"></</span><span style="color: #800000">system.serviceModel</span><span style="color: #0000ff">></span>

  



At this point, once you compile and build the solution, the service will not automatically connect to the Service Bus; this is because IIS/WAS waits until the first service call to activate. Consequently, if you load the WCF service in the browser it will activate the service and establish a connection to the Service Bus (you can confirm by checking [https://YOUR_NAMESPACE.servicebus.windows.net/](https://YOUR_NAMESPACE.servicebus.windows.net/)). So, we’re close, but not yet there.




To get the service to automatically establish the connection to the Service Bus, we’ll use the **Application Warm-Up** extension for IIS 7.5.






  1. Download and install the [Application Warm-Up extension for IIS 7.5](http://www.iis.net/download/applicationwarmup). This gives us the ability to proactively load and initialize processes before the first request arrives. In addition to improving responsiveness it also gives us the ability to connect our WCF service to the Azure AppFabric Service Bus. 

  2. In IIS, select your virtual application EchoSample. Double-click the **Application Warm-Up **feature, and click **Settings**. Check the **Start Application Pool ‘ASP.NET v4.0’ when service started** checkbox. 



[![Application Warm-Up settings](http://images.wadewegner.com/wordpress/2010/05/image_thumb.png)](http://images.wadewegner.com/wordpress/2010/05/image2.png)






  3. Add a new request to register EchoService.svc as a warm-up request for the application. Right-click and choose **Add Request**. Enter EchoService.svc, and click OK. You should now see it in the Request URL list. 



[![Application Warm-Up add request](http://images.wadewegner.com/wordpress/2010/05/image3_thumb.png)](http://images.wadewegner.com/wordpress/2010/05/image3.png)




[![Application Warm-Up](http://images.wadewegner.com/wordpress/2010/05/image6_thumb.png)](http://images.wadewegner.com/wordpress/2010/05/image6.png)




And that’s it! The service is now automatically started and “warmed up” by IIS. To test, recycle the Application Pool and restart the web site. Then, hit your Service Bus endpoint and confirm that you’re services are running.




![Publicly Listed Services](http://images.wadewegner.com/wordpress/2010/05/image_thumb1.png)




Now, even if we restart the computer, the WCF service will reestablish the connection to the Service Bus because IIS 7.5, through the Application Warm-Up Extensions, will automatically refresh.




Now, to complete the test, let’s build a quick Console application to connect to the WCF service via the Service Bus.






  1. Create a new Console Application project called Client. 

  2. Add a reference to the Microsoft.ServiceBus and System.ServiceModel. 

  3. Add a link to the IEchoService.cs file in the EchoSample project. Right-click the project, choose Add Existing, and change Add to Add as Link. 



![Add Existing Item - Add As Link](http://images.wadewegner.com/wordpress/2010/05/image_thumb2.png)






  4. Update the Program.cs file with the following code. Be sure and replace YOUR_NAMESPACE, YOUR_ISSUER_NAME, and YOUR_ISSUER_SECRET with your own values.


    
    <span style="color: #0000ff">class</span> Program
    
    
    {
    
    
        <span style="color: #0000ff">static</span> <span style="color: #0000ff">void</span> Main(<span style="color: #0000ff">string</span>[] args)
    
    
        {
    
    
            <span style="color: #008000">// Determine the system connectivity mode based on the command line</span>
    
    
            <span style="color: #008000">// arguments: -http, -tcp or -auto  (defaults to auto)</span>
    
    
            ServiceBusEnvironment.SystemConnectivity.Mode = GetConnectivityMode(args);
    
    
            <span style="color: #0000ff">string</span> serviceNamespace = "<span style="color: #8b0000">YOUR_NAMESPACE</span>";
    
    
            <span style="color: #0000ff">string</span> issuerName = "<span style="color: #8b0000">YOUR_ISSUER_NAME</span>";
    
    
            <span style="color: #0000ff">string</span> issuerSecret = "<span style="color: #8b0000">YOUR_ISSUER_SECRET</span>";
    
    
            <span style="color: #008000">// create the service URI based on the service namespace</span>
    
    
            Uri serviceUri = ServiceBusEnvironment.CreateServiceUri("<span style="color: #8b0000">sb</span>",
    
    
                serviceNamespace, "<span style="color: #8b0000">EchoServiceNetTcp</span>");
    
    
            <span style="color: #008000">//Uri serviceUri = ServiceBusEnvironment.CreateServiceUri(</span>
    
    
            <span style="color: #008000">//    "https", serviceNamespace, "EchoServiceHttp");</span>
    
    
            <span style="color: #008000">// create the credentials object for the endpoint</span>
    
    
            TransportClientEndpointBehavior sharedSecretServiceBusCredential =
    
    
                <span style="color: #0000ff">new</span> TransportClientEndpointBehavior();
    
    
            sharedSecretServiceBusCredential.CredentialType =
    
    
                TransportClientCredentialType.SharedSecret;
    
    
            sharedSecretServiceBusCredential.Credentials.SharedSecret.IssuerName =
    
    
                issuerName;
    
    
            sharedSecretServiceBusCredential.Credentials.SharedSecret.IssuerSecret =
    
    
                issuerSecret;
    
    
            <span style="color: #008000">// create the channel factory loading the configuration</span>
    
    
            <span style="color: #008000">//BasicHttpRelayBinding myBinding = new BasicHttpRelayBinding();</span>
    
    
            NetTcpRelayBinding myBinding = <span style="color: #0000ff">new</span> NetTcpRelayBinding();
    
    
            EndpointAddress myEndpoint = <span style="color: #0000ff">new</span> EndpointAddress(serviceUri);
    
    
            ChannelFactory<IEchoService> channelFactory =
    
    
                <span style="color: #0000ff">new</span> ChannelFactory<IEchoService>(myBinding, myEndpoint);
    
    
            <span style="color: #008000">// apply the Service Bus credentials</span>
    
    
            channelFactory.Endpoint.Behaviors.Add(sharedSecretServiceBusCredential);
    
    
            <span style="color: #008000">// create and open the client channel</span>
    
    
            IEchoService channel = channelFactory.CreateChannel();
    
    
            Console.WriteLine("<span style="color: #8b0000">Enter text to echo (or [Enter] to exit):</span>");
    
    
            <span style="color: #0000ff">string</span> input = Console.ReadLine();
    
    
            <span style="color: #0000ff">while</span> (input != String.Empty)
    
    
            {
    
    
                <span style="color: #0000ff">try</span>
    
    
                {
    
    
                    Console.WriteLine("<span style="color: #8b0000">Server echoed: {0}</span>",
    
    
                        channel.GetData(Int32.Parse(input)));
    
    
                }
    
    
                <span style="color: #0000ff">catch</span> (Exception e)
    
    
                {
    
    
                    Console.WriteLine("<span style="color: #8b0000">Error: </span>" + e.Message);
    
    
                }
    
    
                input = Console.ReadLine();
    
    
            }
    
    
            <span style="color: #0000ff">if</span> (((IClientChannel)channel).State != CommunicationState.Faulted)
    
    
                ((IClientChannel)channel).Close();
    
    
            <span style="color: #0000ff">else</span>
    
    
                ((IClientChannel)channel).Abort();
    
    
            channelFactory.Close();
    
    
        }
    
    
        <span style="color: #0000ff">static</span> ConnectivityMode GetConnectivityMode(<span style="color: #0000ff">string</span>[] args)
    
    
        {
    
    
            <span style="color: #0000ff">foreach</span> (<span style="color: #0000ff">string</span> arg <span style="color: #0000ff">in</span> args)
    
    
            {
    
    
                <span style="color: #0000ff">if</span> (arg.Equals("<span style="color: #8b0000">/auto</span>", StringComparison.InvariantCultureIgnoreCase)
    
    
                     || arg.Equals("<span style="color: #8b0000">-auto</span>", StringComparison.InvariantCultureIgnoreCase))
    
    
                {
    
    
                    <span style="color: #0000ff">return</span> ConnectivityMode.AutoDetect;
    
    
                }
    
    
                <span style="color: #0000ff">else</span> <span style="color: #0000ff">if</span> (arg.Equals("<span style="color: #8b0000">/tcp</span>", StringComparison.InvariantCultureIgnoreCase)
    
    
                     || arg.Equals("<span style="color: #8b0000">-tcp</span>", StringComparison.InvariantCultureIgnoreCase))
    
    
                {
    
    
                    <span style="color: #0000ff">return</span> ConnectivityMode.Tcp;
    
    
                }
    
    
                <span style="color: #0000ff">else</span> <span style="color: #0000ff">if</span> (arg.Equals("<span style="color: #8b0000">/http</span>", StringComparison.InvariantCultureIgnoreCase)
    
    
                     || arg.Equals("<span style="color: #8b0000">-http</span>", StringComparison.InvariantCultureIgnoreCase))
    
    
                {
    
    
                    <span style="color: #0000ff">return</span> ConnectivityMode.Http;
    
    
                }
    
    
            }
    
    
            <span style="color: #0000ff">return</span> ConnectivityMode.AutoDetect;
    
    
        }
    

  



When you run the console application, it will connect to your WCF service through the Service Bus. Run it to validate. You can also uncomment some of the code to use BasicHttpRelayBinding instead of NetTcpRelayBinding to try out a different configuration.




The ability to host a WCF service in IIS that exposes itself on the Service Bus is a significant milestone. This opens up a number of fantastic opportunities and scenarios that otherwise would have been extremely difficult to accomplish.
