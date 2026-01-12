---
title: "Use social web providers in less than 5 minutes"
date: 2010-08-06T00:00:00-08:00
draft: false
description: "From the whiteboard to the keyboard"
---

Significant [Access Control Service](http://www.microsoft.com/windowsazure/appfabric/) (ACS) updates released to AppFabric LABS environment today. Take a look at the [Windows Azure AppFabric team blog](http://blogs.msdn.com/b/windowsazureappfabric/) for the formal announcement. I’d rather show you a quick demo.

```
        ![]()             ![](Preview.png)



                               ![Get Microsoft Silverlight](http://go2.microsoft.com/fwlink/?LinkId=108181)
                           [                 ![Get Microsoft Silverlight]()             ](http://go2.microsoft.com/fwlink/?LinkID=124807)
```

Quick summary of new capabilities in the Access Control Service:

- Integration with Windows Identity Foundation (WIF) and tooling
- Out-of-the-box support for popular web identity providers including: Windows Live ID, OpenID, Google, Yahoo, and Facebook
- Out-of-the-box support for Active Directory Federation Server v2.0
- Support for OAuth WRAP, WS-Trust, and WS-Federation protocols
- Support for the SAML 1.1, SAML 2.0, and Simple Web Token (SWT) token formats
- Integrated and customizable Home Realm Discovery that allows end-users to choose their identity provider
- An OData-based Management Service that provides programmatic access to ACS configuration
- A Web Portal that allows administrative access to ACS configuration

Try this out yourself. Use Visual Studio, install [Windows Identity Foundation](http://msdn.microsoft.com/en-us/evalcenter/dd440951.aspx), and go to <https://portal.appfabriclabs.com/>.

For detailed information and a more verbose walkthrough (i.e. including explanations), listen to [Justin Smith’s walkthrough and interview](http://acs.codeplex.com/wikipage?title=Videos&referringTitle=Home).
