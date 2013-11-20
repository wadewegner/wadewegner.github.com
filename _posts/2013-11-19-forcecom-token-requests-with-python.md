---
layout: post
title: "Force.com Token Requests with Python"
description: ""
categories:
- Python
- Force.com
- REST
- Token
tags: []
---
{% include setup %}

Today I wrote some Python code for querying the Force.com REST APIs. It started pretty straightforward but I stumbled upon an important concept when I tried to run this Python from an untrusted location. More on this in a moment. Let's start at the beginning.

You first need to create a **Connected App**.

A connected app is an application that integrates with salesforce.com using APIs and uses either SAML and OAuth protocols to authenticate, provide Single Sign-On, and provide tokens for use with Salesforce APIs. In addition to standard OAuth capabilities, connected apps allow administrators to set various security policies and have explicit control over who may use the applications.

1. Login to your Salesforce organization: [https://login.salesforce.com/](https://login.salesforce.com/)

2. Create a **Connected App** by clicking **Create**, **App**, then **New** in the **Connected Apps** section.

3. Enter all the pertinent data and enable **API (Enable OAuth Settings)** with **Full access (full)**. You can likely change the access level to suit your needs, but full access will at least work.

4. Once created, grab your **Consumer Key** and **Consumer Secret**. You'll need them in a moment.

Using this information, you can now execute the following code.

<script src="https://gist.github.com/7557434.js?file=gettoken_local.py" type="text/javascript"></script>

Executing this Pyton code returns the following JSON output:

	{"issued_at": "1384921072679", "access_token": "00DG0000000hatg!AQ8AQIhjBSS9le6QOs_VMQ2iwTPNp78.bzo8JmcdtqtFXN7poIkg5dm6HxpeHPBQhhpw3GXs7383kAM0pWMSmaKlIxO7.GTu", "instance_url": "https://na11.salesforce.com", "id": "https://login.salesforce.com/id/00DG0000000hatgMAA/005G0000003TE91IAG", "signature": "oGbSI0OTkXgN00+kdsNExzMq1RQzZNFrQy9PeGaWuEI="}

Exactly what we'd expect. Or so we think.

Turns out, the fact that this code executes at all is because we've logged into Salesforce from our machine and we have inadvertantly completed one of two steps required for identify verification. Through the APIs it's not enough to simply provide the username and password. We need to do more.

To test this (if you're up for it), try to execute this from a different machine with a different IP address - for example, AWS. You will get the following error:

	authentication failure - Failed: API security token required

While you may find this annoying it's purposeful and important. The reality is that sometimes a username and password isn't enough. We need more protection. This error is in effect telling you that you're not calling from a trusted IP address.

There are three ways to fix it. The easy way, the somewhat more challenging way, and then the right way.

Let's take a look at the three ways. But don't use the first two. Seriously.

1. Set appropriate login ranges for the user profile. **Administration Setup**, **Manage Users**, **Users**. Find your login name and then click the link under **Profile**. In my case, this is **System Administrator**. Click **Login IP Ranges** then the **New* button. For giggles, add the range 0.0.0.0 to 255.255.255.255 as this will enable access from everywhere.

	Hopefully you see why this is not a good long term solution.

2. Set appropriate login ranges for the connected app. **App Setup**, **Create**, **Apps**. Click the connected app you created. Click **New** next to **IP Ranges**. Unlike the IP ranges for the user profile you cannot set a ridiculously large range; your have to be more specific and concrete. Figure out the IP range for your host and enter it here.

	This is also not a good solution although it's better than the previous. Fight the urge to do this and let's look at the right option.

3. Get a security token. This is the right thing to do and is called the [Username-Password OAuth Authentication Flow](http://www.salesforce.com/us/developer/docs/api_rest/Content/intro_understanding_username_password_oauth_flow.htm). Turns out, it's not hard to do this. **Personal Setup**, **My Personal Information**, **Reset My Security Token**. This will send a new security token to you via email.

	You must append the security token to their password. For example, if a user's password is "mypassword" and the security token is "XXXXXXXXXX", then the value provided for this parmeter must be "mypasswordXXXXXXXXXX".

	As you can see, this is not at all difficult.

So, knowing this and proceeding with option #3, we can make a minor change to our Python code.

<script src="https://gist.github.com/7557434.js?file=gettoken_remote.py" type="text/javascript"></script>

This code will run anywhere. Try it out.

I hope you found this interesting. I certainly learned a lot!

Special thanks to both [Pat Patterson](https://twitter.com/metadaddy) and [Steve Marx](https://twitter.com/smarx) for their help along the way.