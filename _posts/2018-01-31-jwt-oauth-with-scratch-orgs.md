---
layout: post
title: "Authenticate to your Scratch Orgs using the OAuth 2.0 JWT Bearer Flow"
description: "Advanced automation and continuous integration designs often separate the creation of a scratch org from the use of the scratch org. In this scenario, you need a way to authenticate to the newly created scratch org. Fortunately, you can use a Connected App configured for the OAuth 2.0 JWT Bearer Flow in your Dev Hub to perform a non-interactive login into a scratch org created from that Dev Hub. In this post we'll look at the steps required to get this to work."
categories: 
- salesforce
- salesforce dx
- sfdx
- oauth
- jwt
- connected app
- ci
- scratch org
---

A different, if not more accurate or better, name for this blog post could be "Authenticate to a Scratch Org You Didn't Create and Whose Password You Don't Know." I even went out to Twitter to ask for help and, well, [Peter Churchill](https://twitter.com/britishboyindc) did not disappoint:

<center><blockquote class="twitter-tweet" data-conversation="none" data-lang="en"><p lang="en" dir="ltr">&quot;I used JWT Bearer Flow with my Scratch Orgs and Dev Hub - you won&#39;t believe what happened next!&quot;</p>&mdash; Peter Churchill (@britishboyindc) <a href="https://twitter.com/britishboyindc/status/958702877475790848?ref_src=twsrc%5Etfw">January 31, 2018</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script></center>

But, as you can see, I went the boring and conservative route. Naming things, even blog posts, is hard. Anyway ...

One of my more popular blog posts recently is [Using the OAuth 2.0 JWT Bearer Flow to Support the Salesforce DX CLI](http://www.wadewegner.com/2017/04/using-the-oauth-2-jwt-bearer-flow-to-support-the-salesforcedx-cli/). Not only is JWT Bearer Flow a powerful capability for automating continuous integration (you're not using usernames and passwords, are you?), but setting it up is rather complex and the blog post details the required steps as simply as possible. In this post, I want to build the capability of the JWT Bearer Flow and introduce a capability of the Dev Hub that makes it easier for you to authenticate to newly created Scratch Orgs, even when you're not on the machine that initially created the scratch org.

Consider, if you will, the following: you have a script that automates these steps every morning:

1. Creates 50 new scratch orgs (possibly with different scratch org configuration settings).
2. Installs 3rd party packages that are used/required.
3. Pushes unpackaged source/metadata required (e.g. custom objects).
4. Assigns permsets.
5. Imports sample data.

This isn't a fictional scenario, as there are a number of ISVs and customers already doing this today as a way to bootstrap environments. But, invariably, the next question is: how do I let a developer, or separate CI system, authenticate to the newly created scratch org?

Turns out, you can setup a Connected App in your Dev Hub to support the JWT OAuth flow that you can then use to authenticate to your scratch org. That's right, you don't have to create the Connected App in the scratch org itself (which is good, as you can't automate it). Using the Connected App in the Dev Hub, you can use the same certificate key and `force:auth:jwt:grant` command to authenticate directly to a scratch org.

As I said, this is a powerful capability for CI systems that create scratch orgs and then need to auth into them but don't have access to access token stored in `~/.sfdx` during the scratch org creation process.

Without further ado, let's go started.

### Create the certificate & private key

The first step step is to create the certiricate and private key you're going to use. I prefer to do this in a place that's both secure and not related to any specific project I'm working on. This way I can make everything work independent of my current project.

    cd ~/ && mkdir .certs && cd .certs

    openssl genrsa -des3 -passout pass:x -out server.pass.key 2048

    openssl rsa -passin pass:x -in server.pass.key -out server.key

    rm server.pass.key

    openssl req -new -key server.key -out server.csr

    openssl x509 -req -sha256 -days 365 -in server.csr -signkey server.key -out server.crt

When finished, you'll have the files you need later in the process. And, while I have these files setup to use locally, it's entirely likely you'll want to put them on a server to make them more accessible. It's also not uncommon to encrypt them and use them with systems like Travis CI (see our [Continuous Integration Using Salesforce DX](https://trailhead.salesforce.com/trails/sfdx_get_started/modules/sfdx_travis_ci) Trailhead module for details).

### Create the Connected App

We're going to use a Connected App in our Dev Hub org. This is part of the magic. By having this Connected App in our Dev Hub and setup with JWT Grant, we can use this flow with all of the Scratch Orgs we create.

To make things easier, I like to use my CLI plugin: `sfdx-waw-plugin` (feel free to contribute here: [https://github.com/wadewegner/sfdx-waw-plugin](https://github.com/wadewegner/sfdx-waw-plugin)). It includes commands for creating the Connected App (and minimizes the manual steps).

Install the `sfdx-waw-plugin` to and look at the command:

```bash
sfdx plugins:install sfdx-waw-plugin

sfdx waw:connectedapp:create --help
```

You'll want to run this command against your Dev Hub; I've aliased mine as `HubOrg` (not sure why).

    sfdx waw:connectedapp:create -c http://localhost:1717/OauthRedirect -e [YOUR_EMAIL_ADDRESS] -s Basic,Api,Web,RefreshToken -n jwtrulz -u HubOrg

When you run this command it will output information.

```json
{
   "fullName":"jwtrulz",
   "contactEmail":"wade.wegner@salesforce.com",
   "description":"generated by waw:connectedapp:create",
   "label":"jwtrulz",
   "oauthConfig":{
      "callbackUrl":"http://localhost:1717/OauthRedirect",
      "consumerKey":"[YOUR_CONSUMER_KEY]",
      "scopes":[
         "Basic",
         "Api",
         "Web",
         "RefreshToken"
      ],
      "consumerSecret":"[YOUR_CONSUMER_SECRET]"
   }
}
```

Grab both the `consumerKey` and the `consumerSecret`.

Now we're going to open our Dev Hub so that we can finish setting up the JWT Bearer Flow. (As I mentioned before, you can't automate all of this through our APIs today.)

    sfdx force:org:open -u HubOrg

We're going to complete the setup of the Connected App. (Unfortunately, there aren't any APIs exposed to assist you in this process).

1. Open **App Manager**, find your Connected App.

2. Click **Edit**. Turn on **Use digital signatures** and choose your **server.crt**.

    ![Use Digital Signatures](https://user-images.githubusercontent.com/746259/35624777-d75b3892-0644-11e8-89c2-4aa83e18d50b.png)

3. Click **Manage** then **Edit Policies**. Update **Permitted Users**.

    ![Edit Policies](https://user-images.githubusercontent.com/746259/35624805-ef677694-0644-11e8-9f66-40050f404cea.png)

4. Click **Manage Profiles** and add your permset.

If you need a refresher for any of these steps, see my blog post [Using the OAuth 2.0 JWT Bearer Flow to Support the Salesforce DX CLI](http://www.wadewegner.com/2017/04/using-the-oauth-2-jwt-bearer-flow-to-support-the-salesforcedx-cli/).

### Log into your Dev Hub with `jwt:grant`

For this to work, you'll want to log into your Dev Hub using `force:auth:jwt:grant`, not `force:auth:web:login`. 

    sfdx force:auth:jwt:grant —clientid [YOUR_CONSUMER_KEY] —username [YOUR_DEVHUB_LOGIN] —jwtkeyfile ~/.certs/server.key —setdefaultdevhubusername -a HubOrgJWT

You should see something like:

    Successfully authorized YOUR_DEVHUB_LOGIN] with org ID [YOUR_ORG_ID].

**Tip:** Notice I've changed my alias here to `HubOrgJWT`. This way I have my normal `HubOrg` which used `force:auth:web:login` and one with JWT. It's not entirely necessary, but I like to have have them both.

### Create a new scratch org and login with `jwt:grant`

Okay, now it's time for the payoff!

Go ahead and create a new Scratch Org:

    sfdx force:project:create -n jwtrulz && cd jwtrulz

    sfdx force:org:create -s -f config/project-scratch-def.json

Copy down the username to the newly screated scratch org. Let's pretend it's `test-qyczf1ot1p6j@wade.wegner_company.net`.

Now, we want to demonstrate that you can authenticate into the scratch org using the JWT Grant flow and the certificate -- nothing else. If you're doing this on your machine, and you want to guarantee that you're not cheating, you can delete the JSON file that's storing the creds for the newly created scratch org: `rm ~/.sfdx/test-qyczf1ot1p6j@wade.wegner_company.net.json`. Not necessary, but it'll help prove I'm not pulling the wool over your eyes.

Are you ready? Okay, let's login to your scratch org with the `jwt:grant` command:

    sfdx force:auth:jwt:grant --clientid [YOUR_CONSUMER_KEY] --username [YOUR_SCRATCHORG_USERNAME] --jwtkeyfile ~/.certs/server.key --setdefaultdevhubusername --instanceurl https://test.salesforce.com

You shoudl see:

    Successfully authorized YOUR_SCRATCHORG_USERNAME] with org ID [YOUR_SCRATCHORG_ID].

BOOM!

Yes, this is the kind of stuff that I really enjoy. And I know there are a few of you out there who dig this too.

I hope you've found this post helpful! It will definitely provide new and powerful ways for you to automate your continuous integration systems and provide more flexible ways for you to design some of your automation.

I hope this helps!