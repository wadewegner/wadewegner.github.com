---
layout: post
title: "Use Workbench with your Scratch Org"
description: "The Salesforce Workbench is a powerful open-source tool used by both admins and developers. But how can you use it with your Salesforce DX scratch org? In this post I'll explain how to use the Salesforce CLI and two powerful commands to make it possible!"
categories: 
- salesforce
- salesforce dx
- sfdx
- cli
- workbench
- scratch org
---

The [Salesforce Workbench](https://workbench.developerforce.com) is a powerful set of tools for developers and admins to use with their org. You can easily describe, query, manipulate, and migrate data and metadata through the web browser, using a combination of Partner, Bulk, REST, Streaming, Metadata, and Tooling APIs. It's completely [open-sourced on Github](https://github.com/ryanbrainard/forceworkbench) by Ryan Brainard ([@RyanBrainard](https://twitter.com/ryanbrainard)).

And if all of this isn't enough to convince you of its value, I can almost guarantee that Josh Kaplan ([@JoshSfdx](https://twitter.com/joshsfdc)) is playing around in Workbench right now!

![Josh Kaplan](https://cloud.githubusercontent.com/assets/746259/25318133/f82383cc-283c-11e7-9887-26c89e9ce32a.jpeg)

Recently, I found the need to use the Workbench while working on a side project. Of course, I'm using Salesforce DX and scratch orgs. Scratch orgs provide the perfect environment to quickly development my app and, when I'm done, I delete it. I also use the new source synchronization capabilities to externalize all my source so I can easily commit it in a Github repository.

If you're one of our Salesforce DX pilot participants, you might be asking yourself: uh, how are you able to use your Scratch org with Workbench when you don't know your scratch org password, it's my domain, and it's not running on production? Well, I'm glad you asked! And not to fear, there are two commands in the Salesforce CLI you can use to make this work!

First, assuming you're in your project workspace, we're going to run the command `sfdx force:user:password:generate` to generate a random password. If you're not in your project workspace, you use the `targetusername` (or `-u`) to specify the scratch org (and consequently the user).

```
> sfdx force:user:password:generate

Successfully set the password "XXXXXXXX" for user 
scratchorgXXXXXXXXXXXXXX@wade.wegnercompany.com. You can see the password
again by running "force:org:describe".
```

Now you know the password! What's more, it's encrypted and stored locally, so running `sfdx force:org:describe` will remind you of the password if you forget it.

Second, you'll need to know the "my domain" used with the scratch org. This is also generated randomly when the scratch org is created. To get it, you run the same command: `sfdx force:org:describe`:

```
Key              Value
───────────────  ───────────────────────────────────────────────────────────
Access Token     XXXXXXXXX...
Client Id        SalesforceDevelopmentExperience
Created Date     2017-04-23
Dev Hub Id       XXXXXXXXX...
Edition          Developer Edition
Expiration Date  2017-05-01
Id               XXXXXXXXX...
Instance Url     https://dream-business-XXXX-dev-ed.cs70.my.salesforce.com
Password         XXXXXXXX
Scratch Org      true
Username         scratchorgXXXXXXXXXXXXXX@wade.wegnercompany.com
```

As you can see, this command includes all kinds of useful information.

Now, armed with this information, it's easy to log into the Workbench.

1. Change the **Environment** to `Sandbox`.

2. Click **I agree to the terms of service**.

    ![Workbench](https://cloud.githubusercontent.com/assets/746259/25318071/74bed640-283b-11e7-8021-a6a60fb13ae0.png)

3. Click **Login with Salesforce**.

4. Click **Use Custom Domain**.

5. Paste your **Instance Url** into the **Custom Domain** box and click **Continue**.

    ![Custom Domain](https://cloud.githubusercontent.com/assets/746259/25318084/d9dd2450-283b-11e7-8a12-501838b32dde.png)

6. Paste your **Username** and **Password** in the box, and click **Log In to Sandbox**.

Voilà! You're using Workbench with your Scratch Org.

A useful tip with useful commands. I hope this helps!