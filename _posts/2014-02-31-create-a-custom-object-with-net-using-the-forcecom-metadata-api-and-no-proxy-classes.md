---
layout: post
title: "Create a Custom Object with .NET using the Force.com Metadata API ... and No Proxy Classes!"
description: "Ever want to create a custom object or field in Salesforce using the APIs? Tired of having to generate massive WSDLs to perform simple operations? In this post we'll look at how we can create custom objects and fields by managing the SOAP requests ourselves."
categories:
- IoT
- printer
- Heroku
- Twilio
- Raspberry Pi
- python
tags: []
---

Recently I've written a lot about using the Force.com REST API with .NET. I've also created a [Force.com Toolkit for .NET](https://github.com/developerforce/Force.com-Toolkit-for-NET), a [Chatter Toolkit for .NET](https://github.com/developerforce/Chatter-Toolkit-for-NET), and a set of [sample applications](https://github.com/developerforce/Force.com-Toolkit-for-NET/tree/master/samples) that make it easier for developers to interact with these APIs. Using the REST API you can write custom integrations with your applications in Salesforce, interacting with all of your metadata through simple HTTP calls using JSON.

There are, however, a few limits to what the REST API can accomplish. In particular, one of the requests and/or questions I see over and over is how to create custom objects and fields using the APIs. If you look at the [Developer Forum](https://developer.salesforce.com/forums/) you’ll see lots of people (especially .NET developers) ask these questions.

The answer is relatively simple: use the [Metadata API](http://www.salesforce.com/us/developer/docs/api_meta/).

Of course, the reality is that there’s more to it. In particular, if you’ve only used the REST APIs (or it’s been years since you’ve had to use SOAP, as was the case with yours truly), this may present a challenge as the Metadata API is SOAP-based and the SOAP API has a different authentication mechanism than the REST APIs. Of course, you're welcome to use your OAuth Token acquired RESTfully in the Metadata API (and SessionId in the REST API) if you desire.

But not to fear. The utility of these APIs is worth the few extra minutes and lines of code.

When starting with these SOAP APIs, most developers will typically download the WSDL files, generate class proxies, and code against them. This is a fine approach and there’s a healthy amount of examples that show how this works.

That said, generating full class proxies against the WSDLs can be a bit heavy-handed when we want to simply do a few basic operations, like create a custom object with some fields. Furthermore, if you’re like me, you may desire more insight into what’s happening in the code and the HTTP requests, and don’t want to rely on all the code that’s automatically generated for me. Personally, I have no problem writing a little more code if it means I have more control and insight.

In this post, I plan to discuss the following through narrative, raw HTTP requests/responses examples, and (of course) code:

1. Demystify the Enterprise and Partner APIs.
2. Explain and demonstrate the SOAP login process.
3. Explain and demonstrate how to use the Metadata API to create a custom object and field.

All this without generating proxy classes using the WSDLs.

Ready to get started? Let’s first look at the Enterprise or Partner APIs.

### SOAP API: Enterprise and Partner WSDLs

The SOAP API lets you create, retrieve, update, or delete records within Salesforce. There are over 20 different API calls that let you perform a number of important tasks and operations. In many ways, it’s similar to the REST APIs (except it’s SOAP ... sorry for the obvious).

As part of the SOAP API, there are two different web services you can use, referred to as Enterprise and Partner. They are functionality equivalent and share the same operations yet differ in how developers interact with them. Each allows you to generate  WSDL file to simplify your interactions. Understanding the differences between these two web services and WSDLs is important.

The Enterprise WSDL is a strongly typed version of the API, meaning that an object in Force.com will have an equivalent object in Java, .NET, or whatever environment is accessing the API. The benefit of this model, where your code is compiled against a specific schema, is that coding is generally easier and more straightforward (as you don't need to deal with any underlying XML structures) and safer in the sense that data and schema dependencies are resolved at compile time, not at runtime. The downside of that strongly typed model is that it only works on a single Force.com account's schema, as it is bound to all of the unique objects and fields that exist in that account's data model. This means that when the metadata in your organization changes you’ll need to update your WSDL and regenerate your proxy classes.

What if you create a solution that you want to work across multiple accounts from different customers? In that case, you need a weakly typed representation of the data. This is is precisely what the Partner WSDL provides.

With the Partner WSDL, the developer is responsible for marshaling data in the correct object representation (which typically involves diving into the XML), but is freed from being dependent on any particular data model or Force.com account.

Put simply, if you are a company using the API exclusively on your own account, you'll typically want the Enterprise WSDL. If you are an ISV using the API across many accounts, you'll typically want the Partner WSDL.

Regardless of which path you use, you’ll have to use the SOAP API to authenticate and use the Metadata API. If this is your only goal, it doesn’t really matter if you use the Enterprise or Partner web service.

For a detailed account of the SOAP APIs please refer to the [SOAP API developer documentation](http://www.salesforce.com/us/developer/docs/api/).

### Authenticating with the SOAP API

To use the SOAP or Metadata APIs you need to login and start a client session. To do this, you must login and obtain a sessionId and and server URL. The sessionId is used in subsequent calls to the web services to identify your session and the server URL identifies the appropriate URI with which you make your subsequent requests.

Note: don’t forget to append your security token to your password if logging in from a location unknown to Salesforce. You will be blocked!

There are two different URIs for logging in, depending on whather you’re using the Enterprise or Partner web service.

- **Enterprise**: https://login.salesforce.com/services/Soap/c/[version]/[organizationId]
- **Partner**: https://login.salesforce.com/services/Soap/u/[version]/[organizationId]

Here’s an example of what a raw login request looks like for the Enterprise web service:

<script src="https://gist.github.com/wadewegner/9139536.js?file=EnterpriseLogin.xml"></script>

The request is nearly identical for the Partner webservice:

<script src="https://gist.github.com/wadewegner/9139536.js?file=PartnerLogin.xml"></script>

I’m sharing the raw requests in this post as I think it is profoundly more useful than just showing how to create the request in a single language. The reality is you can use any language that supports HTTP communication and, even if you do just need the .NET code, you’ll have a better understanding of the process if you know what the raw request looks like.

That said, how do we create these requests in code without using the proxy classes generated by the WSDLs? Let’s see.

I wanted to write a single method that worked against both the Enterprise and Partner API. Required? Absolutely not. If you just want to use one you can easily refactor the following code.

<script src="https://gist.github.com/wadewegner/9140481.js?file=SoapLogin.cs"></script>

When you make the request you’ll get a response. Whether you use the Enterprise or Partner webservice you’ll get <span class="inline-code">loginResponse</span> that pretty much looks like this:

<script src="https://gist.github.com/wadewegner/9139536.js?file=LoginResponse.xml"></script>

The only difference between the Enterprise and Partner web service is that the Enterprise web service will give a response with the following namespace ...

&nbsp;&nbsp;&nbsp;&nbsp;<span class="inline-code">xmlns="urn:enterprise.soap.sforce.com"</span>

... whereas the Partner web service will give a response with this namespace ...

&nbsp;&nbsp;&nbsp;&nbsp;<span class="inline-code">xmlns="urn:partner.soap.sforce.com"

Again, let me reiterate that the <span class="inline-code">loginResponse</span> information you receive is identical, so which one you use doesn’t matter; there’s no material difference when it comes to logging in.

There’s a lot of information in these responses. For our purposes in this post, we really only need the <span class="inline-code">sessionId</span> and <span class="inline-code">metadataServerUrl</span>. As previously mentioned, the <span class="inline-code">sessionId</span> allows us to authenticate when making a call to the various SOAP APIs and the <span class="inline-code">metadataServerUri</span> is the URL we need to use when interacting with the metadata API during subsequent calls.

So, let’s look at what’s required in order to deserialize all this information into .NET objects and then grab the information.

First, we need to define these objects. This is a little tricky in .NET, as we need to attribute our classes with the XML namespace in order to appropriately deserialize. I took an approach of defining two classes that are nearly identical yet live in different namespaces.

<script src="https://gist.github.com/wadewegner/9140481.js?file=LoginResult.cs"></script>

<script src="https://gist.github.com/wadewegner/9140481.js?file=UserInfoResult.cs"></script>

If you choose an approach where you simply use either the Enterprise or Partner web service you can simply choose one or the other.

The code for serializing and returning the response is pretty straightforward.

<script src="https://gist.github.com/wadewegner/9140481.js?file=HandleLoginResponse.cs"></script>

At this point we now have all the information required in order to interact with the Metadata API and create our custom objects and fields.

### Creating a Custom Object using the Metadata API

It sure has taken us a long time to get here. However, using the above, you now have the ability to easily authenticate to the SOAP APIs. Throw these methods into a library and save them - I’m sure you’ll use them over and over.

What does the SOAP look like for creating a custom object?

<script src="https://gist.github.com/wadewegner/9139536.js?file=CreateCustomObject.xml"></script>

For SOAP this is pretty standard. We have a SOAP envelope and, within the body, we’re passing in the metadata used to create our custom object.

We’ll get the following response from the Metadata API:

<script src="https://gist.github.com/wadewegner/9139536.js?file=CustomObjectResponse.xml"></script>

One thing to point out is that this SOAP request only created the custom object. We don’t have any fields yet! We need to issue another request in order to create the fields.

Here’s how we add an additional field (you’ll see it’s unsurprisingly similar):

<script src="https://gist.github.com/wadewegner/9139536.js?file=CreateCustomField.xml"></script>

The surrounding SOAP envelope when creating a custom field is identical to the SOAP envelope used for creating a custom object. What changes is the metadata in the body.

So, what’s an efficient way to approach this in code? Here’s the approach I’ve taken.

Let’s first create a method that handles the surrounding SOAP envelope, makes the HTTP request, and then returns the raw result.

<script src="https://gist.github.com/wadewegner/9140481.js?file=CreateCustom.cs"></script>

You can see that this approach lets us easily pass in the metadata required to either create an object or field, and will build our SOAP envelope appropriately.

From here, let’s create a method for creating the custom object metadata, call our Create method above, and then deserialize the response into an appropriate .NET object.

<script src="https://gist.github.com/wadewegner/9140481.js?file=CreateCustomObject.cs"></script>

We can do the same for a custom field.

<script src="https://gist.github.com/wadewegner/9140481.js?file=CreateCustomField.cs"></script>

Both methods above are extremely similar. With a little more refactoring we might easily simplify this some more, but I’ll leave that to you. (You need something to work on, right?)

Now, given everything we’ve created above, how might we run this?

<script src="https://gist.github.com/wadewegner/9140481.js?file=Main.cs"></script>

Pretty slick. As you can see, the only things you need to bring to this approach are your username, password, and organization id. The first two should be easy for you to get. How do you get your organization id? Easy. Click on **Setup** | **Company Profile** | **Company Information**, and in the right hand column towards the bottom you’ll find your **Organization ID**.

### Conclusion

So, how can we confirm that this work?

![Custom Object and Field](https://raw.github.com/wadewegner/wadewegner.github.com/master/content/2014/02/CustomObjectAndField.png)

Log into your Salesforce organization. Under recent items you should see the custom object and fields you’ve created. Go ahead and click on the custom object and you can confirm that it has the custom field.

<INSERT SCREENSHOT>

There you have it! A reasonably simple approach to establishing a session with the Enterprise and/or Partner APIs and using the Metadata API ... all without having to generate class proxies from the WSDL!

You can grab all the code on Github: [https://github.com/wadewegner/CustomObjectsUsingSOAP](https://github.com/wadewegner/CustomObjectsUsingSOAP)

I hope you found this useful.
