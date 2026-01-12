---
title: "Passive Federation with Windows Azure and ADFS v2"
date: 2009-10-09T00:00:00-08:00
draft: false
description: "From the whiteboard to the keyboard"
---

***Warning:* Please note that this post was written prior to the launch of Windows Identity Foundation. Most of the steps outlined below are no longer accurate nor necessary.**

One of the critical elements a company needs to consider when moving to the cloud is how they will leverage their existing identity stores. Most companies have made significant investments in various identity solutions (i.e. providing for SSO, identity consolidation, federating with partners, etc.) and it’s imperative to ensure that applications and services in the cloud can take advantage of these resources.

***Background***

The practice of enabling the portability of identity information across otherwise autonomous security domains, called identity federation, is no longer a luxury - it’s a necessity. The inability to allow users to access resources in different datacenters, with various trading partners, or on the Web, can quickly cripple a companies productivity (not to mention user satisfaction). Historically, providing for the needs of Web-based single sign on (SSO) and cross-domain resource access has been very difficult to accomplish. It may have required the replication of identity stores in a host of one-off scenario, or even (gasp!) compromising security best practices in order to satisfy a business need.

More recently, various kinds of identity architecture have made identity federation much easier. Practices like [claims-based authentication](http://www.infoq.com/news/2009/10/Guide-Claim-Based-Identity), standards like [WS-Federation](http://en.wikipedia.org/wiki/WS-Federation), and so forth allow companies to establish trust domains between different organization and parties much easier. See the following illustration from [InfoQ](http://www.infoq.com/news/2009/10/Guide-Claim-Based-Identity):

![Claims-based identity with tokens](https://wadewegner.blob.core.windows.net/wordpress/2009/10/image61.png)

In this picture, the application implicitly trusts tokens that come from the issuer. Consequently, rather than having to authenticate the users identity, the application can delegate that responsibility to the issuer, and instead focus on parsing the information received in the token. These tokens contain claims, which are nothing more than bits of information about a person, such as a name, email address, membership in a particular role, and so on. If your application trusts the issuer of the claim, you can leverage the information without having to query the identity store directly.

While it is possible to build a customer claims issuer (or more appropriately called a [security token service](http://msdn.microsoft.com/en-us/library/ms730908.aspx)), there are a host of solutions available today that make this quite easy. Microsoft’s Active Directory Federation Services (ADFS) is a great example, and work implicitly with identities stored in active directory. ADFS allows for federated identity by implementing claims-based authentication and establishing trust between different organizations and parties.

[ADFS v2](http://msdn.microsoft.com/en-us/security/aa570351.aspx) (which was previously codenamed “Geneva” Server") expands this capability by bringing claims-based identity federation to cloud-based applications that live in on the web, in the enterprise, or across an organization. The [“Geneva” Beta 2 datasheet](http://download.microsoft.com/download/7/D/0/7D0B5166-6A8A-418A-ADDD-95EE9B046994/Geneva_Beta.2_datasheet_v3.pdf) provides the following description that I think is a great summation of ADFS v2:

> “Geneva” is Microsoft’s next generation identity and access management platform built on Active Directory® directory services. “Geneva” provides claims-based access and single sign-on for on-premises and cloud-based applications in the enterprise, across organizations, and on the Web.
>
> “Geneva” leverages claims which describe identity attributes and can be used to drive application and other system behaviors with an open architecture that implements the industry’s shared Identity Metasystem vision.

A few things I’d like to point out about ADFS v2 that make it very powerful:

- Open standards. ADFS v2 is based on SAML 2.0, so it can interoperate with products from all kinds of vendors and platforms.
- Automation. ADFS v2 handles the federation of identities by setting up the trust relationships.
- Developer productivity. Since developers interact with trusted claims, they don’t have to spend time worry about the particulars of the identity provider.

In this post, I want to highlight how you can use ADFS v2 to solve many of these identity challenges as you move to the cloud. Additionally, I will also highlight how the Windows Identity Foundation (previously codenamed “Geneva” Framework), or WIF, is used to make your applications claims-aware and minimize the amount of work required by developers.

To explore ADFS v2 and WIF in more detail - and I strongly encourage you do! - please take a look at the following resources and blogs:

- [Claims Based Identity & Access Control Guide](http://claimsid.codeplex.com/)
- [Microsoft Code Name “Geneva”](http://www.microsoft.com/forefront/geneva/en/us/)
- [Claims-Based Identity Whitepaper](http://download.microsoft.com/download/7/d/0/7d0b5166-6a8a-418a-addd-95ee9b046994/Introducing_Geneva_Beta1_Whitepaper.pdf)
- [“Geneva” Framework Whitepaper](http://www.microsoft.com/downloads/details.aspx?FamilyID=9ca5c685-3172-4d8f-81cb-1a59bdc9f7e3&displaylang=en)

Now, before you go through this post and start building this solution for yourself, download the **[Windows Identity Foundation and Windows Azure Passive Federation code](http://code.msdn.microsoft.com/wifwazpassive)**. Run the executable; immediately after the code is installed/copied on to your machine a browser window will open and load the documentation. Read the *entire* guide. There is a ton of good information in the documentation. Next, run through the whole guide and run the demos. I’m not going to restate anything mentioned in the guide in this post, since the guide does a fabulous job. This post assumes that you’ve gone through and leveraged the WIF & WA passive federation guide.

A few additional assumptions I’d like to call out:

- You have an ADFS v2 server (today you’d use “Geneva” Server Beta 2).
- You have installed Windows Identity Foundation.
- You are running Visual Studio 2008 (although 2010 will probably work fine).
- You have installed the Windows Azure SDK and Windows Azure Tools for Visual Studio.
- You have a Windows Azure account and project.

Okay, let’s get started!

***Building the Application***

For a comprehensive examination of many of the below steps, please refer to the [overview and walkthrough](http://code.msdn.microsoft.com/wifwazpassive) installed in the guide you installed above. For the sake of brevity, I have focused on the execution of the process rather than a thorough explanation of how it works. The guide provides a lot of valuable information.

1. To start, create your Windows Azure project. I created a “Federated Identity Demo” pr
   oject with the service name “fedid”. Be sure and keep track of the service name you choose, as you will use it when creating your certificate.
2. You must now create the certificate that you’ll use for SSL. Open the folder where you installed the WIF & WA federation guide. In this folder you will find a folder called assets. Open a Visual Studio 2008 Command Prompt (choose to runas administrator) and run the command:

   CreateCert.cmd

For example, this is what I ran:

![CreateCert.cmd](https://wadewegner.blob.core.windows.net/wordpress/2009/10/image26.png)

Note: you will have to enter the password “abc!123” multiple times. This is documented in the guide. Additionally, you will be prompted to install a certificate on your machine – click Yes.

![Certification Authority](https://wadewegner.blob.core.windows.net/wordpress/2009/10/image33.png)

This process will create a number of files in the Assets folder, including:

- fedid.cloudapp.net.cer
- fedid.cloudapp.net.pfx
- fedid.cloudapp.net.pvk
- encoder.out

3. Create a new Visual Studio 2008 solution. Add a Cloud Service project, and create a Web Role.
4. Update the ServiceDefinition.csdef file to update the following:

- HTTPS protocol
- Port 443
- enableNativeCodeExecution: true

It should look like the following after the update:

```
<span style="color: #0000ff"><?</span>xml version="1.0" encoding="utf-8"<span style="color: #0000ff">?></span>

<span style="color: #0000ff"><</span><span style="color: #800000">ServiceDefinition</span> <span style="color: #ff0000">name</span>=<span style="color: #0000ff">"CloudService"</span> <span style="color: #ff0000">xmlns</span>=<span style="color: #0000ff">"http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition"</span><span style="color: #0000ff">></span>

  <span style="color: #0000ff"><</span><span style="color: #800000">WebRole</span> <span style="color: #ff0000">name</span>=<span style="color: #0000ff">"Web"</span> <span style="color: #ff0000">enableNativeCodeExecution</span>=<span style="color: #0000ff">"true"</span><span style="color: #0000ff">></span>

    <span style="color: #0000ff"><</span><span style="color: #800000">InputEndpoints</span><span style="color: #0000ff">></span>

      <span style="color: #008000"><!-- Must use port 80 for http and port 443 for https when running in the cloud --></span>

      <span style="color: #0000ff"><</span><span style="color: #800000">InputEndpoint</span> <span style="color: #ff0000">name</span>=<span style="color: #0000ff">"HttpIn"</span> <span style="color: #ff0000">protocol</span>=<span style="color: #0000ff">"https"</span> <span style="color: #ff0000">port</span>=<span style="color: #0000ff">"443"</span> <span style="color: #0000ff">/></span>

    <span style="color: #0000ff"></</span><span style="color: #800000">InputEndpoints</span><span style="color: #0000ff">></span>

    <span style="color: #0000ff"><</span><span style="color: #800000">ConfigurationSettings</span> <span style="color: #0000ff">/></span>

  <span style="color: #0000ff"></</span><span style="color: #800000">WebRole</span><span style="color: #0000ff">></span>

<span style="color: #0000ff"></</span><span style="color: #800000">ServiceDefinition</span><span style="color: #0000ff">></span>
```

5. Right-click the Cloud Service project, and choose Properties. Select the SSL tab. Enable both checkboxes, and select the certificate you generated from the store. Here’s what it looks like for me once I finished updating:

![SSL tab](https://wadewegner.blob.core.windows.net/wordpress/2009/10/image15.png)

6. Add the following references to your Web Role project:

- System.IdentityModel
- Microsoft.IdentityModel
- Microsoft.IdentityModelPlus (this is in the assets folder that was created by the guide)

Make sure to set Copy Local true for Microsoft.IdentityModel and Microsoft.IdentityModelPlus so that they are added to your Windows Azure package.

7. Create a Global.asax file.
8. Update the Global.asax file so that you have the following using statements:

   using System.IdentityModel.Selectors;

   using System.IdentityModel.Tokens;

   using System.Security.Cryptography.X509Certificates;

   using Microsoft.IdentityModel;

   using Microsoft.IdentityModel.Configuration;

   using Microsoft.IdentityModel.Tokens;

   using Microsoft.IdentityModel.Web;

   using Microsoft.IdentityModel.Web.Configuration;

   using Microsoft.IdentityModelPlus.Configuration;

   using Microsoft.IdentityModelPlus.Tokens;
9. Add the following “ServiceConfiguration\_Created” method:

   void ServiceConfiguration\_Created(object sender, ServiceConfigurationCreatedEventArgs args)

   {

   ```
    ServiceConfiguration configuration = args.ServiceConfiguration;

    List<CookieTransform> sessionTransforms = <span style="color: #0000ff">new</span> List<CookieTransform>(<span style="color: #0000ff">new</span> CookieTransform[]

    {

        <span style="color: #0000ff">new</span> DeflateCookieTransform(), <span style="color: #0000ff">new</span> MachineKeyProtectionTransform()

    });

    SessionSecurityTokenHandler sessionHandler = <span style="color: #0000ff">new</span> SessionSecurityTokenHandler(sessionTransforms.AsReadOnly(), <span style="color: #0000ff">new</span> MruSecurityTokenCache());

    configuration.SecurityTokenHandlers.AddOrReplace(sessionHandler);

    MicrosoftIdentityModelPlusSection plusConfiguration = MicrosoftIdentityModelPlusSection.Current;

    <span style="color: #0000ff">if</span> (plusConfiguration != <span style="color: #0000ff">null</span> && plusConfiguration.ServiceCertificate.ElementInformation.IsPresent)

    {

        X509Certificate2 serviceCertificate = plusConfiguration.ServiceCertificate.GetCertificate();

        SecurityToken serviceToken = <span style="color: #0000ff">new</span> X509SecurityToken(serviceCertificate);

        SecurityTokenResolver serviceResolver = SecurityTokenResolver.CreateDefaultSecurityTokenResolver(

            <span style="color: #0000ff">new</span> List<SecurityToken>(<span style="color: #0000ff">new</span> SecurityToken[] { serviceToken }).AsReadOnly(), <span style="color: #0000ff">false</span>);

        configuration.ServiceTokenResolver = serviceResolver;

    }
   ```

   }
10. Add the following “WSFederationAuthenticationModule\_RedirectingToIdentityProvider” method:

    void WSFederationAuthenticationModule\_RedirectingToIdentityProvider(object sender, RedirectingToIdentityProviderEventArgs e)

    {

    ```
    Uri reqUrl = Request.Url;

    StringBuilder wreply = <span style="color: #0000ff">new</span> StringBuilder();

    wreply.Append(reqUrl.Scheme);     <span style="color: #008000">// e.g. "http"</span>

    wreply.Append("<span style="color: #8b0000">://</span>");

    wreply.Append(Request.Headers["<span style="color: #8b0000">Host</span>"] ?? reqUrl.Authority);

    wreply.Append(Request.ApplicationPath);

    <span style="color: #0000ff">if</span> (!Request.ApplicationPath.EndsWith("<span style="color: #8b0000">/</span>"))

    {

        wreply.Append("<span style="color: #8b0000">/</span>");

    }

    e.SignInRequestMessage.Reply = wreply.ToString();
    ```

    }
11. Update the “Application\_Start” method so that it creates event handlers for the two previous methods:

    protected void Application\_Start(object sender, EventArgs e)

    {

    ```
    FederatedAuthentication.ServiceConfigurationCreated += <span style="color: #0000ff">this</span>.ServiceConfiguration_Created;

    FederatedAuthentication.WSFederationAuthenticationModule.RedirectingToIdentityProvider +=

        <span style="color: #0000ff">new</span> EventHandler<RedirectingToIdentityProviderEventArgs>(WSFederationAuthenticationModule_RedirectingToIdentityProvider);
    ```

    }
12. You must now make some significant updates to the Web.Config file. This is probably the most challenging step, as it requires a number of updates. While I will walk through the details of the updates below, I am also including a [copy of my Web.Config file for your review on SkyDrive](http://cid-716f83c58a3bf96f.skydrive.live.com/self.aspx/Blog/ADFSandWA/Web.config). Please feel free to use it as a reference. Also, please note that (for simplicity) the code shown below includes values that my demo uses. You will have to update with your own values, and I have tried to call out where this is necessary.
13. Add the following to the  …

    «/span>section name=“microsoft.identityModel” type=“Microsoft.IdentityModel.Configuration.MicrosoftIdentityModelSection, Microsoft.IdentityModel, Version=0.6.1.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35” />

    «/span>section name=“microsoft.identityModelPlus” type=“Microsoft.IdentityModelPlus.Configuration.MicrosoftIdentityModelPlusSection, Microsoft.IdentityModelPlus” requirePermission=“false” />
14. Add the app key for the federation metadata location. You will have to update this value with your own XML file published by your ADFS v2 server.

    «/span>appSettings>

    «/span>add key=“FederationMetadataLocation” value=“[https://corp2.sts.microsoft.com/FederationMetadata/2007-06/FederationMetadata.xml"](https://corp2.sts.microsoft.com/FederationMetadata/2007-06/FederationMetadata.xml%22) />

    </appSettings>
15. After connectionStrings, add the following location path.

    «/span>location path=“FederationMetadata”>

    «/span>system.web>

    ```
    <span style="color: #0000ff"><</span><span style="color: #800000">authorization</span><span style="color: #0000ff">></span>

      <span style="color: #0000ff"><</span><span style="color: #800000">allow</span> <span style="color: #ff0000">users</span>=<span style="color: #0000ff">"*"</span> <span style="color: #0000ff">/></span>

    <span style="color: #0000ff"></</span><span style="color: #800000">authorization</span><span style="color: #0000ff">></span>
    ```

    </system.web>

    </location>
16. Add the Microsoft.IdentityModel assembly.

    «/span>add assembly=“Microsoft.IdentityModel, Version=0.6.1.0, Culture=neutral, PublicKeyToken=31BF3856AD364E35” />
17. Configure the authorization and authentication. You may have to change existing values.

    «/span>authorization>

    «/span>deny users=”?" />

    </authorization>

    «/span>authentication mode=“None” />
18. Add the WSFederationAuthenticationModule and SessionAuthenticationModule modules to  … .

    «/span>add name=“WSFederationAuthenticationModule” type=“Microsoft.IdentityModel.Web.WSFederationAuthenticationModule, Microsoft.IdentityModel, Version=0.6.1.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35” />

    «/span>add name=“SessionAuthenticationModule” type=“Microsoft.IdentityModel.Web.SessionAuthenticationModule, Microsoft.IdentityModel, Version=0.6.1.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35” />
19. Add the WSFederationAuthenticationModule and SessionAuthenticationModule modules to  … .

    «/span>add name=“WSFederationAuthenticationModule” type=“Microsoft.IdentityModel.Web.WSFederationAuthenticationModule, Microsoft.IdentityModel, Version=0.6.1.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35” preCondition=“managedHandler” />

    «/span>add name=“SessionAuthenticationModule” type=“Microsoft.IdentityModel.Web.SessionAuthenticationModule, Microsoft.IdentityModel, Version=0.6.1.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35” preCondition=“managedHandler” />
20. Add the microsoft.identityModel section.

    «/span>microsoft.identityModel>

    «/span>service>

    ```
    <span style="color: #0000ff"><</span><span style="color: #800000">audienceUris</span><span style="color: #0000ff">></span>

      <span style="color: #0000ff"><</span><span style="color: #800000">add</span> <span style="color: #ff0000">value</span>=<span style="color: #0000ff">"https://fedid.cloudapp.net/"</span> <span style="color: #0000ff">/></span>

    <span style="color: #0000ff"></</span><span style="color: #800000">audienceUris</span><span style="color: #0000ff">></span>

    <span style="color: #0000ff"><</span><span style="color: #800000">federatedAuthentication</span><span style="color: #0000ff">></span>

      <span style="color: #0000ff"><</span><span style="color: #800000">wsFederation</span> <span style="color: #ff0000">passiveRedirectEnabled</span>=<span style="color: #0000ff">"true"</span> <span style="color: #ff0000">issuer</span>=<span style="color: #0000ff">"https://corp2.sts.microsoft.com/FederationPassive/"</span> <span style="color: #ff0000">realm</span>=<span style="color: #0000ff">"https://fedid.cloudapp.net/"</span> <span style="color: #ff0000">requireHttps</span>=<span style="color: #0000ff">"true"</span> <span style="color: #0000ff">/></span>

      <span style="color: #0000ff"><</span><span style="color: #800000">cookieHandler</span> <span style="color: #ff0000">requireSsl</span>=<span style="color: #0000ff">"true"</span> <span style="color: #0000ff">/></span>

    <span style="color: #0000ff"></</span><span style="color: #800000">federatedAuthentication</span><span style="color: #0000ff">></span>

    <span style="color: #0000ff"><</span><span style="color: #800000">applicationService</span><span style="color: #0000ff">></span>

      <span style="color: #0000ff"><</span><span style="color: #800000">claimTypeRequired</span><span style="color: #0000ff">></span>

        <span style="color: #0000ff"><</span><span style="color: #800000">claimType</span> <span style="color: #ff0000">type</span>=<span style="color: #0000ff">"http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name"</span> <span style="color: #ff0000">optional</span>=<span style="color: #0000ff">"true"</span> <span style="color: #0000ff">/></span>

        <span style="color: #0000ff"><</span><span style="color: #800000">claimType</span> <span style="color: #ff0000">type</span>=<span style="color: #0000ff">"http://schemas.microsoft.com/ws/2008/06/identity/claims/role"</span> <span style="color: #ff0000">optional</span>=<span style="color: #0000ff">"true"</span> <span style="color: #0000ff">/></span>

      <span style="color: #0000ff"></</span><span style="color: #800000">claimTypeRequired</span><span style="color: #0000ff">></span>

    <span style="color: #0000ff"></</span><span style="color: #800000">applicationService</span><span style="color: #0000ff">></span>

    <span style="color: #0000ff"><</span><span style="color: #800000">issuerNameRegistry</span> <span style="color: #ff0000">type</span>=<span style="color: #0000ff">"Microsoft.IdentityModel.Tokens.ConfigurationBasedIssuerNameRegistry, Microsoft.IdentityModel, Version=0.6.1.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35"</span><span style="color: #0000ff">></span>

      <span style="color: #0000ff"><</span><span style="color: #800000">trustedIssuers</span><span style="color: #0000ff">></span>

        <span style="color: #0000ff"><</span><span style="color: #800000">add</span> <span style="color: #ff0000">thumbprint</span>=<span style="color: #0000ff">"A4010FC094ECEDE6C94EDE36315ADB3EEC876C8A"</span> <span style="color: #ff0000">name</span>=<span style="color: #0000ff">"CN=corp2.sts.microsoft.com, OU=IAM, O=Microsoft, L=Redmond, S=wa, C=US"</span> <span style="color: #0000ff">/></span>

      <span style="color: #0000ff"></</span><span style="color: #800000">trustedIssuers</span><span style="color: #0000ff">></span>

    <span style="color: #0000ff"></</span><span style="color: #800000">issuerNameRegistry</span><span style="color: #0000ff">></span>
    ```

    </service>

    </microsoft.identityModel>

You will have to make a number of updates:

- Update “fedid” in “<https://fedid.cloudapp.net>” to your Windows Azure service name.
- Update the wsFederation issuer to your ADFS v2 issuer.
- Update the wsFederation realm so that it uses your Windows Azure service name.
- Ensure that the claimTypeRequired claimTypes are valid.
- Update the trustedIssures so that you have your ADFS v2 thumbprint and name. I did this by using tools that come with the Windows Identity Foundation tools. You can right-click an existing ASP.NET Web site project and select “Modify STS Reference…”. This will launch the Federation Utility wizard, and will help construct pieces of the Web.Config. On the second step, you can select “Use an Existing STS” and point it to your ADFS v2 endpoint. The tool will then generate a lot of the configuration for you, including the trustedIssuers thumbprint and name (see the image below).

![Federation Utility wizard](https://wadewegner.blob.core.windows.net/wordpress/2009/10/image331.png)

Note: this doesn’t need to be a publicly exposed ADFS server. In fact, if you don’t have a production IP address to use, you can download one sample (simple) IP STS included in the code downloaded from <http://claimsid.codeplex.com/>.

21. You need to add the Microsoft.IdentityModelPlus section to the Web.Config file. The CreateCert.cmd script you ran to generate the script has actually generated this section for you. You can get it out of the encoder.out file in the assets folder.

    «/span>microsoft.identityModelPlus>«/span>serviceCertificate>«/span>certificate name=“fedid.cloudapp.net” password=“abc!123” encodedType=“pfx” encodedValue=“MIIGygIBAzCCBoYGCSqGSIb3DQEHAaCCBncEggZzMIIGbzCCA8AGCSqGSIb3DQEHAaCCA7EEggOtMIIDqTCCA6UGCyqGSIb3DQEMCgECoIICtjCCArIwHAYKKoZIhvcNAQwBAzAOBAg+lz0NqTYEFwICB9AEggKQ/shUmEoIDJpT0uOaQIkOfUsLfLwUNHtkm58I/fFb7OztUkISIHZm5QR3b9yH7mqg7VLj0LndB6r2+T2RZpq5U+jnlptBxyENzz3RAMIt8S7vVtT9L0OwiDXXeMWRXZpBGK/dwfwofMBsCCFV2FeLGyso2sUhqP7tstTWhybjM0JDSzVre9s/HGaQl8R+buPeGZWnZjOCqOYBNIoAxrPD1DjMgt2Q1CChk23rdirKnBFnp2khy+o2sV02PTJI8K4/QFgKTYOm+LbJG8rWuFi3U1aLxF7xUqfwCkNwaEyPodWBcAkhlWnKlbg77tPlhaONbYEJw9XhY4Ekwniu8WrNbl08xsKEVVNUZfTMYU4OnuusFxHsf0weuhrwlMnCQAMQkDUICWqDX2afV/oU5MnvDwYC8HftfaKhR68unfX+D1iS3ZspSpjrtQbdJU3GHsZmruqR1gmjbzwSJa0rf/ch4BWA+D4ciSlVFVFR+WBPB1fbLUbWv+3bLnwDCAmk2vNQ9YFQKWVYU4Ax9PulyX5Mu5ggmb0Hxl5wdaO2syiiIkylKNYmyreQ81WNRLwJmGKSmTZPJAovmT9YgH2kNdaC9K/9ZNbbyJSRTJNQix7zivrG5IS5n9OCLSNwoP9lK56ME3phYOa80F8r/lWsBP4otTaZQFC5g8uRnGcQ1WJcNEEghi1l0q4PM5Mtl3L2zaqQGa8pJqgaAcOyOC85rOTwQtnJEHUquH/yjqyZ8koCo44T9JHd7lVJKKJb2Fnwz1zVOM/rPoUZBwEJTuHBqlnVgyBldZDxUGg56L2My8ozhQ7dYWqsxfFe+fPzDNvjPvpMregGBqhEcP3W8YCzAOSyS0d7QN/MZNi4+6QwypRuckUxgdswEwYJKoZIhvcNAQkVMQYEBAEAAAAwXQYJKwYBBAGCNxEBMVAeTgBNAGkAYwByAG8AcwBvAGYAdAAgAFMAdAByAG8AbgBnACAAQwByAHkAcAB0AG8AZwByAGEAcABoAGkAYwAgAFAAcgBvAHYAaQBkAGUAcjBlBgkqhkiG9w0BCRQxWB5WAFAAdgBrAFQAbQBwADoAYwA2ADkANQA2ADIANAAzAC0AMQAyAGEAMAAtADQAMAA5ADcALQBhADIAOABjAC0AYgA1ADAAMQA3ADEAMQA3ADIAZgA1ADAwggKnBgkqhkiG9w0BBwagggKYMIIClAIBADCCAo0GCSqGSIb3DQEHATAcBgoqhkiG9w0BDAEGMA4ECHcGonbqlG+xAgIH0ICCAmADkVVOEzWeS3cNaqf0Zmv0xIszELOp4G62Bwl0HNd/uoKDnVBekLocMhlvLlgHwko6cCxCZ9LD0Hn7g9mK8P4VTGNk8G2I3CelBTy0nnHFw7DswRZxYluMQ4A4LHLuG6jo4B/rCaV2JaTDXmfiNvIAdIzIkFCV2Ue6ROSW4VdZFcbDdqjxXdZY4b6MuZMBLc3YLPYqKvyMuCfDYPIkA5jNxSJ6ODdV+Ed5DNuDWwO5fOjep7nqVwhU3J3V063SRBcF+KYZwb1RVE+SWxvOEb1nv2uoJdD9GPWIiH5E+3oAFayJU1APOC15jHJ+YXFq/i+/XnXI+JkkxUyVVLrJIDT7uIZSkv7zieVoTJK8Ze8V4gLge081f+wxy2RcQwajeSdR1YKzrVKzxrR7wfGC+R1oH1ldjZM7hw3+2C/UR5R6bHqt7D9C2R9mxfFUufEiFG5SoUItv+ZFHd9/x7Oe8TpcWY/yrwSkHr4UKBDYzfzgB8Q0LmCZfEt8MWWGutKL6OzCfBNZ4QM2Ltn2mrodD5kE2udAcocXBPTp/DAngMFxsIe7iNHpw/ulEjJx8EYPrtDvf8C/2y5APBYdEdqVozHlUMo8MWO2xx39hktyIqTYaMkTIwgFXCAaf3ZyZVOlj5YSqXiZpKmSU1RcNCTnYTN5uPjEurL1U24uPB/jVM6WjNO/azVAp5o+3PU2095I+X/Rtzfbou8o00tGosf42ETVA62A6OrfuKI1kNu5CHoRtr3PsMk3vCRc7FKbq86I5jW7jNmolmqOzQ0w0SxPPzxbouoikPtKFDmmSU81lTlFtzA7MB8wBwYFKw4DAhoEFLq6NYeOz/n0Dmg+5fWFBQZ6cOQRBBQ4290pYD9MhQO1jzFVfX3DeYLNiAICB9A=" /></serviceCertificate></microsoft.identityModelPlus>

Note: this is only necessary because currently you cannot install certificates in Windows Azure. Expect this to change in the future.

22. Finally, as a way of confirming that everything is working and that the claims you expect are actually sent to your application, update the Default.aspx.cs file so that you iterate through the claims and write them to the browser.

    using System;

    using System.Collections.Generic;

    using System.Linq;

    using System.Web;

    using System.Web.UI;

    using System.Web.UI.WebControls;

    using Microsoft.IdentityModel.Claims;

    using System.Threading;

    namespace Web

    {

    ```
    <span style="color: #0000ff">public</span> partial <span style="color: #0000ff">class</span> _Default : System.Web.UI.Page

    {

        <span style="color: #0000ff">protected</span> <span style="color: #0000ff">void</span> Page_Load(<span style="color: #0000ff">object</span> sender, EventArgs e)

        {

            <span style="color: #0000ff">try</span>

            {

                IClaimsIdentity ici =

                    Thread.CurrentPrincipal.Identity <span style="color: #0000ff">as</span> IClaimsIdentity;

                <span style="color: #0000ff">foreach</span> (Claim c <span style="color: #0000ff">in</span> ici.Claims)

                    Response.Write(c.ClaimType + "<span style="color: #8b0000"> - </span>" + c.Value + "<span style="color: #8b0000"><br/></span>");

            }

            <span style="color: #0000ff">catch</span> (Exception ex)

            {

                Response.Write(ex.ToString());

            }

        }

    }
    ```

    }

And that’s it! Assuming that you have correctly followed the above steps, you’re ready to test!

Now, when you hit the endpoint on your ADFS v2 service, you will get redirected to <https://fedid.cloudapp.net> (or whatever your URL is) because that’s what you specified as the realm for your identity federation. This means that, to test this locally, you need to update your hosts file so that fedid.cloudapp.net temporarily resolves locally. Otherwise, it will redirect to the cloud. I know we’ve spend years abhorring the hosts file, but sometimes it just works.

![hosts](https://wadewegner.blob.core.windows.net/wordpress/2009/10/image39.png)

When you’re ready to run this in the cloud, simply package up your Windows Azure solution and publish. Remember that you will only be able to successfully test this in production, as the staging account uses a GUID for the URL alias. Also, if you’ve updated your hosts file, but sure and undo your changes so that you no longer resolve locally.

I hope you found this article valuable. Good luck!
