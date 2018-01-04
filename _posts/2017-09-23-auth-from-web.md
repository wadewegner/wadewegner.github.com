---
layout: post
title: "Authenticate to Your Dev Hub from the Web"
description: ""
categories: 
- salesforce
- salesforce dx
- sfdx
- dev hub
- oauth
- authentication
---

In my last post I introduced the [Deploy to Salesforce DX]() button, and alluded to a lot of neat things that are worth highlighting. In this post, I want to showcase a useful authentication flow that [Thomas Dvornak]() enabled via the Salesforce command line interface (CLI).

If you've spent any amount of time with Salesforce DX so far, you know that you first authenticate to your Dev Hub so that you can create scratch orgs. Using the CLI, the actions are:

```
sfdx force:auth:web:login -d

sfdx force:org:create -s -f config/your-scratch-config.json
```

These two actions 1) authenticate you to your Dev Hub and specify that it's the default Dev Hub for your operations (that's the `-d` flag) and then

Behind the scenes, the `force:org:create` command is executed against the Dev Hub, meaning the CLI passes your Dev Hub's auth token to the Dev Hub. At the same time, the CLI is using a Connected App that is managed by Salesforce. Any time you perform an OAuth flow against Salesforce you're doing so against a Connected App. To simplify the CLI, we use that Connected App by default. Notice that you are able to specify your own Connected App by using the ` -i, --clientid CLIENTID` flag on the `force:web:auth:login` command.

# image of auth

Hopefully this is straightforward, and certainly it reduces the complexity around using the CLI.

Now, for a moment, think about the Deploy to Salesforce DX application. It's letting YOU log into the website to perform actions against your own Dev Hub. That means that, via a Web Login OAuth flow, we need to create an auth token that we can use in the CLI to perform actions.

This is where it gets a bit more complex.

For this to work, there 