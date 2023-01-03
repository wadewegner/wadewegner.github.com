---
author: admin
categories:
- BizTalk Server
- WCF
comments: true
date: "2007-09-11T07:02:39Z"
slug: a-few-tips-when-using-wcf-with-biztalk-server-2006-r2
title: A few tips when using WCF with BizTalk Server 2006 R2
wordpress_id: 32
---

As I was putting together the demonstration for the talk I'm giving tomorrow on [BizTalk Server R2 and the WCF Adapters](http://www.wadewegner.com/2007/09/10/PresentingToTheDenverBizTalkUserGroup.aspx) I had to do a few things to get some basic examples working, as this was a brand new virtual environment.  I thought I'd document some of the actions I had to take.

**1. Registering the "WCF-BasicHttp" adapter.**

After creating and deploying an orchestration for use with the "WCF-BasicHttp" adapter, I received the following error:


> The Messaging Engine failed to register the adapter for "WCF-BasicHttp" for the receive location "/SampleWCFDemo/SampleWCFDemo_Process_ProcessPort.svc". Please verify that the receive location exists, and that the isolated adapter runs under an account that has access to the BizTalk databases.


I had to take two steps to resolve the problem.  First, create a new application pool and set the identity to the service account used by the isolated host instance.  Second, give the service account used by the isolated host instance read/write access to the C:WindowsTemp directory.

**2. Can't find the "SvcUtil.exe" console application.**

Turns out that I had forgotten to install the [Windows SDK and .NET Framework 3.0 Runtime Components](http://www.wadewegner.com/ct.ashx?id=f1e3ac17-07e5-4225-b29e-ec4baf0edeff&url=http%3a%2f%2fwww.microsoft.com%2fdownloads%2fdetails.aspx%3fFamilyId%3dC2B1E300-F358-4523-B479-F53D234CDCCF%26displaylang%3den).  See my previous post on how to [setup a BizTalk Server 2006 R2 development environment](http://www.wadewegner.com/2007/09/10/SettingUpABizTalkServer2006R2Beta2DevelopmentEnvironment.aspx) for more information.

**3. "SvcUtil.exe" failed when generating the service metadata.**

When I ran the command "C:Program FilesMicrosoft SDKsWindowsv6.0BinSvcUtil.exe"
http://localhost/SampleWCFDemo/SampleWCFDemo_Process_ProcessPort.svc?wsdl I ended up getting the following error:


> Error: Cannot obtain Metadata from http://localhost/SampleWCFDemo/SampleWCFDemo_Process_ProcessPort.svc?wsdl

If this is a Windows (R) Communication Foundation service to which you have access, please check that you have enabled metadata publishing at the specified address.  For help enabling metadata publishing, please refer to the MSDN documentation at http://go.microsoft.com/fwlink/?LinkId=65455. 

WS-Metadata Exchange Error
    URI: http://localhost/SampleWCFDemo/SampleWCFDemo_Process_ProcessPort.svc?wsdl
    Metadata contains a reference that cannot be resolved: 'http://localhost/SampleWCFDemo/SampleWCFDemo_Process_ProcessPort.svc?wsdl'.
    Content Type application/soap+xml; charset=utf-8 was not supported by service http://localhost/SampleWCFDemo/SampleWCFDemo_Process_ProcessPort.svc?wsdl .  The client and service bindings may be mismatched.
    The remote server returned an error: (415) Cannot process the message because the content type 'application/soap+xml; charset=utf-8' was not the expected type 'text/xml; charset=utf-8'.. 

HTTP GET Error
    URI: http://localhost/SampleWCFDemo/SampleWCFDemo_Process_ProcessPort.svc?wsdl


Additionally, when I loaded the WSDL in Internet Explorer, I got the message:


> Metadata publishing for this service is currently disabled.


Turns out that I had to change the service behavior to allow the metadata to be published.  You can do this by modifying the Web.Config file of the service so that "httpHelpPageEnabled" is set to true:


> <behavior name="ServiceBehaviorConfiguration">
    <serviceMetadata httpGetEnabled="true" httpsGetEnabled="false" />
</behavior>


Changing the value from "false" to "true" resolves the problem.

...

Those were the three issues that came up tonight.  I'm sure there will be more, and I'll post the resolutions when I can.

I hope this helps!
