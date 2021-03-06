---
categories:
- salesforce
- salesforce dx
- sfdx
- scratch org
- mobile sdk
- react native
date: "2018-02-01T00:00:00Z"
description: Salesforce DX provides a lot of great tools for development that, frankly,
  will significantly improve the life of a mobile app developer who's building an
  integration to Salesforce. In this post I'll show you how to use the Mobile SDK
  as a way to quickly get started on a React Native app and then use Salesforce DX
  to make it easy to build everything else in Salesforce.
title: Build a Native Mobile App Using Salesforce DX and the Mobile SDK
aliases: 
- /2018/02/mobile-sdk-with-salesforce-dx/
---

This week I had the opportunity to demo how developers can combine project-based app development with Salesforce DX, the Salesforce CLI, Scratch Orgs, and the Mobile SDK to make -- at least, in my humble opinion -- a pretty compelling native app dev experience with Salesforce. In this post I'll share some of the things I did with the hope that, after you look at it, you'll want to try it out too.

### Setup the CLI Plugin for the Mobile SDK

Did you know the Salesforce Mobile team created a CLI plugin? Oh yes they did!

To install it, run the following command:

```bash
$ sfdx plugins:install sfdx-mobilesdk-plugin
```

Now if you run `sfdx mobilesdk --help` you'll see all the SDK commands are available.

In case you're wondering, these commands are no different than the `forceios`, `forcedriod`, `forcehybrid`, and `forcereact` commands. This plugin is simply a wrapper to include all these commands in a single CLI. A big improvement, in my opinion.

> Note: If you're interested in learning details about the mobile SDK, I suggest you take a look at the [Develop with Mobile SDK](https://trailhead.salesforce.com/en/trails/mobile_sdk_intro) Trail. I'm not going to get into details about how it operates and how to set it up. These modules can do a much better job.

### Setup Your Workspace

Lots of ways you can approach this. Here's my recommendation:

```
myproject/
├── force
└── mobile
```

Yes, looks simple. Don't worry, there will be more folders.

`force` will contain our Salesforce DX project and all the associated source and metadata. `mobile` will contain our mobile application and all the files generated by the Mobile SDK.

So, start off by creating `myproject`:

```bash
$ mkdir myproject && cd myproject
```

Now, you might as well initialize it as a git repo. Yes, that's right -- a single git repo for both your mobile app and your Salesforce project.

```bash
$ git init
$ git remote add origin <yourremote>
```

Next, let's create the Salesforce DX project. The first time I tried this out, I did everything manually: created my project, then my scratch org, created the connected app, grabbed the data, and so forth. But if you know anything about me, I'm a lazy developer. Which means I hate to do the same thing more than once. Consequently, I created a script that not only setup my Salesforce DX project for me, but also outputs all the values I'm going to need to connect the mobile app to my Scratch Org for development.

> To use the script below, grab my CLI plugin which allows you to create Connected Apps through the CLI: `sfdx plugins:install sfdx-waw-plugin`

Here's a script you can use. Create a file like `setup.sh` (and make it executable `chmod +x setup.sh`) and add the following to it:

```bash
# set the callback URL
callbackUrl="sfdx://success"

# create force project
project="$(sfdx force:project:create -n force)";

# open folder
cd force

# create a scratch org
org="$(sfdx force:org:create -s -f config/project-scratch-def.json)";

# Create a connected app
connectedApp="$(sfdx waw:connectedapp:create -c $callbackUrl -e wade.wegner@gmail.com -n mobile -l mobile -d mobile -s Api,Web,RefreshToken)";
consumerKey="$(echo ${connectedApp} | jq -r .oauthConfig.consumerKey)"

echo "Consumer Key: " $consumerKey

# generate a password
password="$(sfdx force:user:password:generate)"

# get values
display="$(sfdx force:org:display --json)"
result="$(echo ${display} | jq -r .result)"
password="$(echo ${result} | jq -r .password)"
username="$(echo ${result} | jq -r .username)"
instanceUrl="$(echo ${result} | jq -r .instanceUrl)"

# echo values
echo "Username: " $username
echo "Password: " $password
echo "Instance URL: " $instanceUrl
echo "Callback URL: " $callbackUrl

# return to root
cd ..
```

Then, run it:

```bash
$ ./setup.sh
```

Boom! Neat, right? You're output should be something like ...

```bash
$ ./setup.sh
Consumer Key: <snip>
Callback URL: sfdx://success
Username:     test-ppvviujleloj@demo_company.net
Password:     **********
Instance URL: https://mello-yellow-999-dev-ed.cs90.my.salesforce.com
```

... which contains everything you need to connect with your mobile app.

Speaking of which, let's create that mobile app.

### Create Your Mobile App with the Mobile SDK

This step is pretty easy thanks to all the hard work done by the Salesforce Mobile SDK team. Make sure you're in the `myproject` folder and run the following CLI command:

```bash
$ sfdx mobilesdk:reactnative:create
    --platform ios
    --appname demo1
    --packagename com.wadewegner.demo1
    --organization WadeWegner.com
    --outputdir mobile
```

Of course, you likely want to change some of those values. But you get the idea.

That will create everything you need to build and run the native mobile applications. Additionally, the SDK includes a bunch of useful stuff to get you going fast (e.g. some simple pages, wiring up auth, etc).

Okay, now let's wire it together!

### Update the Mobile App to Use Your Scratch Org

Remember the info we echo'd to the console above? It's time to use it!

Open up Xcode (of course, if you targeted a different platform, use the right tool). You'll want to open the Xcode workspace (not the project) in Xcode. You'll find it at `myproject/mobile/ios/demo1.xcworkspace`.

Open the `AppDelegate.m` file and update the `RemoteAccessConsumerKey` to use the **Consumer Key** from above and `OAuthRedirectURI` to use the **Callback URL** from above. (Why we use different names for all these things I'll never know.)

That's it for the code! Now, run the app.

Before we launch the mobile app in Xcode, and if you created a React Native app like I did above, be sure to run `npm start` in the `myproject/mobile` folder. This ensures you have all the React "stuff" running for your app.

Okay -- now you can run the app in Xcode.

### Log Into the Mobile App

Let's go ahead and log into the mobile app and see it in action!

1. On the login screen, click "Use Custom Domain".

    <img src="https://user-images.githubusercontent.com/746259/35710831-8a82348c-076e-11e8-9894-c6c2121cb92b.png" width="200" alt="image" style="border-style: solid;border-width:1px;border-color:#767676;">

2. Paste in the **Instance URL** we echo'd above (be sure you don't copy in the `https://` as it won't remove it.)

    <img src="https://user-images.githubusercontent.com/746259/35710869-b3626b9c-076e-11e8-82fd-52a84cbae230.png" width="200" alt="image" style="border-style: solid;border-width:1px;border-color:#767676;">

3. Then type (or copy) the username and password.

    <img src="https://user-images.githubusercontent.com/746259/35710893-d4fd09c4-076e-11e8-9099-113ca98456e2.png" width="200" alt="image" style="border-style: solid;border-width:1px;border-color:#767676;">

4. Click the Allow button to use the Connected App for API access.

    <img src="https://user-images.githubusercontent.com/746259/35710910-e7c80edc-076e-11e8-9c9b-9bc742c747fb.png" width="200" alt="image" style="border-style: solid;border-width:1px;border-color:#767676;">

And presto, your mobile app is working against your scratch org!

<img src="https://user-images.githubusercontent.com/746259/35710925-f78a27a6-076e-11e8-8b20-b85e8952f97a.png" width="200" alt="image" style="border-style: solid;border-width:1px;border-color:#767676;">

Pretty neat!

# Conclusion

I hope this has demonstrated that Salesforce DX, and all the tools that comes with it, is a compelling way to combine Salesforce with native mobile apps built using the Mobile SDK. I think it's good, but I think we can do even better! I'm hoping to get the CLI plugin (and SDK) updated such that those values we echo'd from the script can be injected into the template (via the CLI command) such that it doesn't require any code updates. Wouldn't that be sweet?

I hope this helps, and let me know if you have additional ideas on improvement! You can find me on Twitter @WadeWegner.