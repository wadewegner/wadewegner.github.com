---
layout: post
title: "Using Salesforce DX with Trailhead"
description: "You should use Salesforce DX with Trailhead. Not only does it reinforce best practices for development, it will help you to really understand what it is you're doing. In this article, we'll look at the development workflow and what you need to do to be successful."
categories: 
- salesforce
- salesforce dx
- sfdx
- trailhead
- cli
---

As you've likely seen on Twitter, I've really gotten into [Trailhead](trailhead.com) these past few months. In particular, the Superbadges. I not only enjoy the challenge of working through the project, but regularly discover I don't know nearly as much as I think I do. There's a ton of useful and practical information in Trailead just waiting for us all to discover.

Of course, I have other motivations for doing Trailhead. I find it's an amazing way to validate various workflows with [Salesforce DX](https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev/sfdx_dev_intro.htm). I've logged at least a dozen bugs or feature requests based on trying to get something in Trailhead to work with various things, such as the Salesforce CLI, Scratch Orgs, source synch, or continuous integration.

Numerous people have asked me to share my workflow and tips for getting started. So, please pardon the more tutorial-style post as I write this all out. Also, before you get started, be sure to review the [Salesforce DX Setup Guide](https://developer.salesforce.com/docs/atlas.en-us.sfdx_setup.meta/sfdx_setup/sfdx_setup_intro.htm) to ensure you have everything all set!

I thought it'd be good to use the [Quick Start: Build Your First App](https://trailhead.salesforce.com/projects/quickstart-devzone-app) project as the example for this tutorial.

## Step 1: Log into your TPO using the CLI

The first thing you'll want to do is log into your trailhead playground org (TPO) using the [Salesforce CLI](https://developer.salesforce.com/tools/sfdxcli). Doing this will allow you to target your TPO and deploy your metadata to pass challenges.

1. Reset your TPO password. You'll find all the information in the [Get Your Trailhead Playground Usernamd and Password](https://trailhead.salesforce.com/modules/trailhead_playground_management/units/get-your-trailhead-playground-username-and-password) module. You have to do this because Trailhead created the TPO for you and you they set the password.

2. Log into your TPO using your username & password: `sfdx force:auth:web:login -a tpo`. Notice we've aliased it `tpo` so that it's easier to target in the future. Once done, you can test this by opening up your TPO with `sfdx force:org:open -u tpo` and it'll pop right up.

Okay, that complete's the initial setup. Now it's time to dig in.

## Step 2: Create a project & scratch org 

I like to create a new project for each trail/project/superbadge. This lets me keep all the required source in a place where it's easy to re-use and also allows me to easily push to a version control system (VCS). (You're using VCS, right?)

1. Create a project. For this quick start, I'd do: `sfdx force:project:create -n trailblazerapp`. This will create a new directory with all the requisite files for using Salesforce DX.

2. Depending on the challenge, you may want/need to change the default scratch org configuration file. Take a look at [the docs](https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev/sfdx_dev_scratch_orgs_def_file.htm) to see if there's anything you want to do.

3. Create your scratch org. I do something like `sfdx force:org:create -s -f config/project-scratch-def.json`. This not only creates the scratch org, but `-s` makes it the default for the project, meaning all other commands I invoke will default to this scratch org.

## Step 3: Complete the challenge in the scratch org and pull the source

You'll see that the [Create the Trailblazer App](https://trailhead.salesforce.com/projects/quickstart-devzone-app/steps/devzone-app-1) module has you complete a number of steps. You'll want to do all of this in the scratch org, not your TPO. Then, when you've completed the steps, you can pull it all out of the scratch org so that you have a local copy of all your changes.

This is the eye-opening moment for most people when they start using Salesforce DX. It's really simple to make changes in your scratch org then externalize them out of the org.

1. Open your scratch org: `sfdx force:org:open`

2. Complete all the steps required for the challenge.

3. Pull the changes: `sfdx force:source:pull`

Now you'll see you'll see your source in the `force-app/main/default` folder:

![image](https://user-images.githubusercontent.com/746259/34566625-fc9a041c-f113-11e7-868f-f06ed147bdbd.png)

If you haven't done so before, poke around and take a look. This is what metadata looks like!

## Step 4: Deploy to your TPO and verify

At this point, you're ready to verify you completed the challenge. But the metadata isn't yet in your TPO. No problem, all the MDAPI commands are available in the CLI. And, since we already logged into your TPO, it's really simple to deploy it.

1. Convert your source into the MDAPI format. Simple: `sfdx force:source:convert -d mdapiout`. This will create a new folder called `mdapiout`. If you look in it, it will look similar to your source, but there are subtle differences. You can learn about [source file format](https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev/sfdx_dev_source_file_format.htm), and it's benefits, in the docs.

2. Now, deploy the metadata into your TPO with `sfdx force:mdapi:deploy -d mdapiout --wait 100 -u tpo`. Notice we've targeted the `mdapiout` folder, we're using `--wait` so that it's synchronous and with `-u` we've targeted our TPO.

    You'll see something like:

    ```
    ❯ sfdx force:mdapi:deploy -d mdapiout --wait 100 -u tpo
    9185 bytes written to /var/folders/9t/rvzqw72n7jj03c4pccmq3vrj7b3gyz/T/mdapiout.zip using 44.179ms
    Deploying /var/folders/9t/rvzqw72n7jj03c4pccmq3vrj7b3gyz/T/mdapiout.zip...

    === Status
    Status:  Pending
    jobid:  0Af4100001aS6FLCA0
    Component errors:  0
    Components deployed:  0
    Components total:  0
    Tests errors:  0
    Tests completed:  0
    Tests total:  0
    Check only: false

    Deployment finished in 9000ms

    === Result
    Status:  Succeeded
    jobid:  0Af4100001aS6FLCA0
    Completed:  2018-01-04T14:02:11.000Z
    Component errors:  0
    Components deployed:  11
    Components total:  11
    Tests errors:  0
    Tests completed:  0
    Tests total:  0
    Check only: false
    ```

    And that's it!

3. Click the `Verify` button. If you completed everything successfully, you'll pass.

Two important notes:

#### Profiles are messy and need clean-up

Let's face it, they are. It's better to use permission sets if you can. But, you'll notice that when you create schema or do just about anything else, it updates profiles. Did you notice them when you pulled them out of the scratch org?

Often times, when you pull from the scratch org, you'll pull profiles that don't exist in your target org. This happens. When it does, simply remove the unwanted profiles from your source (not the MDAPI format) and convert it again into MDAPI format.

Ideally, try to avoid profiles. Sadly, since they're so pervasive, Trailhead often assumes (and even checks) for the existence of the profile, so sometimes you can't avoid it.

#### Script this step to make it easier

I've created an alias that makes it really easy to deploy to my TPO.

```
alias mdd='function _blah(){ sfdx force:source:convert -d mdapiout && sfdx force:mdapi:deploy -d mdapiout --wait 100 -u $1 && rm -rf mdapiout };_blah'
```

With this alias, I can convert, deploy, and clean all with one command: `mdd tpo`

Definitely a lot easier!

## Wrap it up

That's pretty much it! You can do this over and over again. The best part is, as I mentioned, you can setup version control and push all your source.

In your project, it's as simple as the following:

```
git init
git remote add origin git@github.com:wadewegner/th-trailblazerapp.git
git add -A
git commit -m "It works"
git push origin master
```

And then you'll have a nice repo of your code refer to in the future: [https://github.com/wadewegner/th-trailblazerapp](https://github.com/wadewegner/th-trailblazerapp)

Hope this helps!