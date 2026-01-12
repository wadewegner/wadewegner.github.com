---
title: "Host WCF Services in IIS with Service Bus Endpoints"
date: 2010-05-12T00:00:00-08:00
draft: false
description: "From the whiteboard to the keyboard"
---

**Update:** To see how to leverage Windows Azure AppFabric Autostart to activate the WCF Service, please see [AutoStart WCF Services to Expose them as Service Bus Endpoints](http://www.wadewegner.com/2010/08/autostart-wcf-services-to-expose-them-as-service-bus-endpoints/).

Vishal Chowdhary, a Senior Test Lead on the Azure AppFabric team, recently posted a whitepaper on [hosting WCF services with Service Bus endpoints from IIS](http://code.msdn.microsoft.com/ServiceBusDublinIIS). This whitepaper provides two solutions to a (previously) significant challenge in hosting WCF services in IIS that connect to the Azure AppFabric Service Bus.

The primary challenge is activation. As Vishal writes, “For the on-premise WCF service to start receiving messages from the Service Bus in the cloud (aka Relay Service), the on-premises service opens an outbound port and creates a bidirectional socket for communication. It connects to the Service Bus, authenticates itself, and starts listening to calls from the relay service before the client sends its requests.” He goes on to say that “IIS/WAS relies on message-based activation & will launch the host only after the first request comes in.” Consequently, **until the first message is received by IIS the service will never establish a connection to the Service Bus**; with no connection to the Service Bus, **it will never receive a message**. A bit of a dilemma.

In the whitepaper, Vishal points out two ways to resolve this challenge:

- IIS Application Warm-Up
- ASP.NET 4.0 Auto-Start

In this post, I’m going to highlight exactly how to go about using **IIS Application Warm-Up to get a WCF service hosted in IIS 7.5 to receive messages from the Service Bus**. This post borrows heavily from Visha’s whitepaper; I strongly suggest you spend the time to read the entire paper.

1. If you’re using .NET 4.0, you must [setup .NET 4.0 to work with the Azure AppFabric SDK](http://www.wadewegner.com/2010/05/net-framework-4-0-and-the-azure-appfabric-sdk/).
2. Create a new ASP.NET Web Application project called **EchoSample** in Visual Studio 2010 using .NET 4.0.
3. To validate this approach, we want this project hosted in IIS. Right-click the project and choose Properties. Select the Web tab, and switch from **Use Visual Studio Development Server** to **Use Local IIS Web server** and click **Create Virtual Directory.**
4. Add the Microsoft.ServiceBus reference from the “C:%Program Files%\Windows Azure platform AppFabric SDK\V1.0\Assemblies” folder.
5. You have to create a custom BehaviorExtensionElement for the ServiceRegistrySettings to make the discoverability policy ‘Public’ in the configuration file. Consequently, we need to create a class that we’ll call MyServiceRegistrySettingsElement that inherits the BehaviorExtensionElement.

   ```
    public class MyServiceRegistrySettingsElement : BehaviorExtensionElement
    {
        public override Type BehaviorType
        {
            get { return typeof(ServiceRegistrySettings); }
        }
        protected override object CreateBehavior()
        {
            return new ServiceRegistrySettings()
            {
                DiscoveryMode = this.DiscoveryMode,
                DisplayName = this.DisplayName
            };
        }
        [ConfigurationProperty("discoveryMode", DefaultValue = DiscoveryType.Private)]
        public DiscoveryType DiscoveryMode
        {
            get { return (DiscoveryType)this["discoveryMode"]; }
            set { this["discoveryMode"] = value; }
        }
        [ConfigurationProperty("displayName")]
        public string DisplayName
        {
            get { return (string)this["displayName"]; }
            set { this["displayName"] = value; }
        }
    }
   ```
6. Now, let’s add a new WCF Service called **EchoService** to the project. Remove the existing method in the ServiceContract and create the following GetData method in the IEchoService.cs file.

   ```
    [OperationContract]
    string GetData(int value);
   ```
7. Also, update the EchoService.svc.cs with the implementation of the GetData method.

   ```
    public string GetData(int value)
    {
        if (value < 0)
            throw new ApplicationException("Negative values not allowed!!!");
        Thread.Sleep(value);
        return string.Format("You entered: {0}", value);
    }
   ```
8. Now we need to update the web.config settings. This is fairly extensive. Be sure and replace YOUR\_NAMESPACE, YOUR\_ISSUER\_NAME, and YOUR\_ISSUER\_SECRET with your own values.

   ```
    <system.serviceModel>
      <extensions>
        <behaviorExtensions>
          <add name="ServiceRegistrySettings"
          type="EchoSample.MyServiceRegistrySettingsElement, EchoSample,
    Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
        </behaviorExtensions>
      </extensions>
      <services>
        <clear />
        <service behaviorConfiguration="MyServiceTypeBehavior"
        name="EchoSample.EchoService">
          <endpoint
          address="http://localhost/EchoSample/EchoService.svc/LocalEchoService"
          binding="basicHttpBinding"
          bindingConfiguration="BasicHttpConfig"
          name="Basic" contract="EchoSample.IEchoService" />
          <endpoint
          address="https://YOUR_NAMESPACE.servicebus.windows.net/EchoServiceHttp/"
          behaviorConfiguration="sharedSecretClientCredentials"
          binding="basicHttpRelayBinding"
          bindingConfiguration="HttpRelayEndpointConfig"
          name="RelayEndpoint"
          contract="EchoSample.IEchoService" />
          <endpoint
          address="sb://YOUR_NAMESPACE.servicebus.windows.net/EchoServiceNetTcp/"
          behaviorConfiguration="sharedSecretClientCredentials"
          binding="netTcpRelayBinding"
          bindingConfiguration="NetTcpRelayEndpointConfig"
          name="RelayEndpoint"
          contract="EchoSample.IEchoService" />
        </service>
      </services>
      <bindings>
        <basicHttpBinding>
          <binding name="BasicHttpConfig" />
        </basicHttpBinding>
        <basicHttpRelayBinding>
          <binding name="HttpRelayEndpointConfig">
            <security relayClientAuthenticationType="RelayAccessToken" />
          </binding>
        </basicHttpRelayBinding>
        <netTcpRelayBinding>
          <binding name="NetTcpRelayEndpointConfig">
            <security relayClientAuthenticationType="RelayAccessToken" />
          </binding>
        </netTcpRelayBinding>
      </bindings>
      <behaviors>
        <endpointBehaviors>
          <behavior name="sharedSecretClientCredentials">
            <transportClientEndpointBehavior credentialType="SharedSecret">
              <clientCredentials>
                <sharedSecret issuerName="YOURISSUERNAME"
                issuerSecret="YOURISSUERSECRET" />
              </clientCredentials>
            </transportClientEndpointBehavior>
            <ServiceRegistrySettings discoveryMode="Public" />
          </behavior>
        </endpointBehaviors>
        <serviceBehaviors>
          <behavior name="MyServiceTypeBehavior">
            <serviceMetadata httpGetEnabled="true" />
            <serviceDebug includeExceptionDetailInFaults="true" />
          </behavior>
        </serviceBehaviors>
      </behaviors>
    </system.serviceModel>
   ```

At this point, once you compile and build the solution, the service will not automatically connect to the Service Bus; this is because IIS/WAS waits until the first service call to activate. Consequently, if you load the WCF service in the browser it will activate the service and establish a connection to the Service Bus (you can confirm by checking <https://YOUR_NAMESPACE.servicebus.windows.net/>). So, we’re close, but not yet there.

To get the service to automatically establish the connection to the Service Bus, we’ll use the **Application Warm-Up** extension for IIS 7.5.

1. Download and install the [Application Warm-Up extension for IIS 7.5](http://www.iis.net/download/applicationwarmup). This gives us the ability to proactively load and initialize processes before the first request arrives. In addition to improving responsiveness it also gives us the ability to connect our WCF service to the Azure AppFabric Service Bus.
2. In IIS, select your virtual application EchoSample. Double-click the \*\*Application Warm-Up \*\*feature, and click **Settings**. Check the **Start Application Pool ‘ASP.NET v4.0’ when service started** checkbox.

   ![Application Warm-Up settings](https://wadewegner.blob.core.windows.net/wordpress/2010/05/image2.png)
3. Add a new request to register EchoService.svc as a warm-up request for the application. Right-click and choose **Add Request**. Enter EchoService.svc, and click OK. You should now see it in the Request URL list.

   ![Application Warm-Up add request](https://wadewegner.blob.core.windows.net/wordpress/2010/05/image3.png)

   ![Application Warm-Up](https://wadewegner.blob.core.windows.net/wordpress/2010/05/image6.png)

And that’s it! The service is now automatically started and “warmed up” by IIS. To test, recycle the Application Pool and restart the web site. Then, hit your Service Bus endpoint and confirm that you’re services are running.

![Publicly Listed Services](https://wadewegner.blob.core.windows.net/wordpress/2010/05/image_thumb1.png)

Now, even if we restart the computer, the WCF service will reestablish the connection to the Service Bus because IIS 7.5, through the Application Warm-Up Extensions, will automatically refresh.

Now, to complete the test, let’s build a quick Console application to connect to the WCF service via the Service Bus.

1. Create a new Console Application project called Client.
2. Add a reference to the Microsoft.ServiceBus and System.ServiceModel.
3. Add a link to the IEchoService.cs file in the EchoSample project. Right-click the project, choose Add Existing, and change Add to Add as Link.

   ![Add Existing Item - Add As Link](https://wadewegner.blob.core.windows.net/wordpress/2010/05/image_thumb2.png)
4. Update the Program.cs file with the following code. Be sure and replace YOUR\_NAMESPACE, YOUR\_ISSUER\_NAME, and YOUR\_ISSUER\_SECRET with your own values.

   ```
    class Program
    {
        static void Main(string[] args)
        {
            // Determine the system connectivity mode based on the command line
            // arguments: -http, -tcp or -auto (defaults to auto)
            ServiceBusEnvironment.SystemConnectivity.Mode = GetConnectivityMode(args);

            string serviceNamespace = "YOUR_NAMESPACE";
            string issuerName = "YOURISSUERNAME";
            string issuerSecret = "YOURISSUERSECRET";

            // create the service URI based on the service namespace
            Uri serviceUri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoServiceNetTcp");

            // create the credentials object for the endpoint
            TransportClientEndpointBehavior sharedSecretServiceBusCredential = new TransportClientEndpointBehavior();
            sharedSecretServiceBusCredential.CredentialType = TransportClientCredentialType.SharedSecret;
            sharedSecretServiceBusCredential.Credentials.SharedSecret.IssuerName = issuerName;
            sharedSecretServiceBusCredential.Credentials.SharedSecret.IssuerSecret = issuerSecret;

            // create the channel factory loading the configuration
            NetTcpRelayBinding myBinding = new NetTcpRelayBinding();
            EndpointAddress myEndpoint = new EndpointAddress(serviceUri);
            ChannelFactory channelFactory = new ChannelFactory(myBinding, myEndpoint);

            // apply the Service Bus credentials
            channelFactory.Endpoint.Behaviors.Add(sharedSecretServiceBusCredential);

            // create and open the client channel
            IEchoService channel = channelFactory.CreateChannel();
            Console.WriteLine("Enter text to echo (or [Enter] to exit):");
            string input = Console.ReadLine();

            while (input != String.Empty)
            {
                try
                {
                    Console.WriteLine("Server echoed: {0}",
                    channel.GetData(Int32.Parse(input)));
                }
                catch (Exception e)
                {
                    Console.WriteLine("Error: " + e.Message);
                }
                input = Console.ReadLine();
            }
            if (((IClientChannel)channel).State != CommunicationState.Faulted)
                ((IClientChannel)channel).Close();
            else
                ((IClientChannel)channel).Abort();
            channelFactory.Close();
        }

        static ConnectivityMode GetConnectivityMode(string[] args)
        {
            foreach (string arg in args)
            {
                if (arg.Equals("/auto", StringComparison.InvariantCultureIgnoreCase)
                || arg.Equals("-auto", StringComparison.InvariantCultureIgnoreCase))
                {
                    return ConnectivityMode.AutoDetect;
                }
                else if (arg.Equals("/tcp", StringComparison.InvariantCultureIgnoreCase)
                || arg.Equals("-tcp", StringComparison.InvariantCultureIgnoreCase))
                {
                    return ConnectivityMode.Tcp;
                }
                else if (arg.Equals("/http", StringComparison.InvariantCultureIgnoreCase)
                || arg.Equals("-http", StringComparison.InvariantCultureIgnoreCase))
                {
                    return ConnectivityMode.Http;
                }
            }
            return ConnectivityMode.AutoDetect;
        }
    }
   ```

When you run the console application, it will connect to your WCF service through the Service Bus. Run it to validate. You can also uncomment some of the code to use BasicHttpRelayBinding instead of NetTcpRelayBinding to try out a different configuration.

The ability to host a WCF service in IIS that exposes itself on the Service Bus is a significant milestone. This opens up a number of fantastic opportunities and scenarios that otherwise would have been extremely difficult to accomplish.
