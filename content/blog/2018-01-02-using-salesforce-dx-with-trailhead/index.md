---
title: "Using Salesforce DX with Trailhead"
date: 2018-01-02T00:00:00-08:00
draft: false
description: "From the whiteboard to the keyboard"
---

![image](https://user-images.githubusercontent.com/746259/34568070-75637fae-f119-11e7-8551-9b036a9d7fd2.png)

As you’ve likely seen on Twitter, I’ve really gotten into [Trailhead](trailhead.com) these past few months. In particular, the Superbadges. I not only enjoy the challenge of working through the project, but regularly discover I don’t know nearly as much as I think I do. There’s a ton of useful and practical information in Trailead just waiting for us all to discover.

Of course, I have other motivations for doing Trailhead. I find it’s an amazing way to validate various workflows with [Salesforce DX](https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev/sfdx_dev_intro.htm). I’ve logged at least a dozen bugs or feature requests based on trying to get something in Trailhead to work with various things, such as the Salesforce CLI, Scratch Orgs, source synch, or continuous integration.

Numerous people have asked me to share my workflow and tips for getting started. So, please pardon the more tutorial-style post as I write this all out. Also, before you get started, be sure to review the [Salesforce DX Setup Guide](https://developer.salesforce.com/docs/atlas.en-us.sfdx_setup.meta/sfdx_setup/sfdx_setup_intro.htm) to ensure you have everything all set!

You’ll find the following in this post:

- Logging into your Trailhead playground org in the CLI
- Creating a project & scratch org
- Completing the challenge and pulling your source
- Deploying source to your TPO to verify
- Installing required packages
- Exporting and importing data
- Executing anonymous Apex
- Opening up a Lightning App

I thought it’d be good to use the [Quick Start: Build Your First App](https://trailhead.salesforce.com/projects/quickstart-devzone-app) project as the example for this tutorial.

### [Step 1: Log into your TPO using the CLI](#step-1-log-into-your-tpo-using-the-cli)

The first thing you’ll want to do is log into your trailhead playground org (TPO) using the [Salesforce CLI](https://developer.salesforce.com/tools/sfdxcli). Doing this will allow you to target your TPO and deploy your metadata to pass challenges.

1. Reset your TPO password. You’ll find all the information in the [Get Your Trailhead Playground Usernamd and Password](https://trailhead.salesforce.com/modules/trailhead_playground_management/units/get-your-trailhead-playground-username-and-password) module. You have to do this because Trailhead created the TPO for you and you they set the password.
2. Log into your TPO using your username & password: `sfdx force:auth:web:login -a tpo`. Notice we’ve aliased it `tpo` so that it’s easier to target in the future. Once done, you can test this by opening up your TPO with `sfdx force:org:open -u tpo` and it’ll pop right up. Note: you’ll likely end up having more than one TPO, so a good practice would be to give it an alias that’s specific to your task.

Okay, that completes the initial setup. Now it’s time to dig in.

### [Step 2: Create a project & scratch org](#step-2-create-a-project--scratch-org)

I like to create a new project for each trail/project/superbadge. This lets me keep all the required source in a place where it’s easy to re-use and also allows me to easily push to a version control system (VCS). (You’re using VCS, right?)

1. Create a project. For this quick start, I’d do: `sfdx force:project:create -n trailblazerapp`. This will create a new directory with all the requisite files for using Salesforce DX.
2. Depending on the challenge, you may want/need to change the default scratch org configuration file. Take a look at [the docs](https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev/sfdx_dev_scratch_orgs_def_file.htm) to see if there’s anything you want to do.
3. Create your scratch org. This assumes you already have your Dev Hub setup and ready to go (if not, [look here](https://developer.salesforce.com/docs/atlas.en-us.sfdx_setup.meta/sfdx_setup/sfdx_setup_enable_devhub.htm)). I do something like `sfdx force:org:create -s -f config/project-scratch-def.json`. This not only creates the scratch org, but `-s` makes it the default for the project, meaning all other commands I invoke will default to this scratch org.

### [Step 3: Complete the challenge in the scratch org and pull the source](#step-3-complete-the-challenge-in-the-scratch-org-and-pull-the-source)

You’ll see that the [Create the Trailblazer App](https://trailhead.salesforce.com/projects/quickstart-devzone-app/steps/devzone-app-1) module has you complete a number of steps. You’ll want to do all of this in the scratch org, not your TPO. Then, when you’ve completed the steps, you can pull it all out of the scratch org so that you have a local copy of all your changes.

This is the eye-opening moment for most people when they start using Salesforce DX. It’s really simple to make changes in your scratch org then externalize them out of the org.

1. Open your scratch org: `sfdx force:org:open`
2. Complete all the steps required for the challenge.
3. Pull the changes: `sfdx force:source:pull`

Now you’ll see you’ll see your source in the `force-app/main/default` folder:

![image](https://user-images.githubusercontent.com/746259/34566625-fc9a041c-f113-11e7-868f-f06ed147bdbd.png)

If you haven’t done so before, poke around and take a look. This is what metadata looks like!

### [Step 4: Deploy to your TPO and verify](#step-4-deploy-to-your-tpo-and-verify)

At this point, you’re ready to verify you completed the challenge. But the metadata isn’t yet in your TPO. No problem, all the MDAPI commands are available in the CLI. And, since we already logged into your TPO, it’s really simple to deploy it.

1. Convert your source into the MDAPI format. Simple: `sfdx force:source:convert -d mdapiout`. This will create a new folder called `mdapiout`. If you look in it, it will look similar to your source, but there are subtle differences. You can learn about [source file format](https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev/sfdx_dev_source_file_format.htm), and it’s benefits, in the docs.
2. Now, deploy the metadata into your TPO with `sfdx force:mdapi:deploy -d mdapiout --wait 100 -u tpo`. Notice we’ve targeted the `mdapiout` folder, we’re using `--wait` so that it’s synchronous and with `-u` we’ve targeted our TPO.

   You’ll see something like:

   ```
   $ sfdx force:mdapi:deploy -d mdapiout --wait 100 -u tpo
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

   And that’s it!
3. Click the `Verify` button. If you completed everything successfully, you’ll pass. If it fails, you have a few options:

   - Update your source, test in scratch org, and re-deploy. In most cases, this will work.
   - Maybe you defined the schema for a custom object wrong? You might need to manually (or automatically) delete it in the TPO before you re-deploy.
   - When all else fails, create a new TPO. It’s free and not too hard.

Two important notes:

#### [Profiles are messy and need clean-up](#profiles-are-messy-and-need-clean-up)

Let’s face it, they are. It’s better to use permission sets if you can. But, you’ll notice that when you create schema or do just about anything else, it updates profiles. Did you notice them when you pulled them out of the scratch org?

Often times, when you pull from the scratch org, you’ll pull profiles that don’t exist in your target org. This happens. When it does, simply remove the unwanted profiles from your source (not the MDAPI format) and convert it again into MDAPI format. A tip, shared by SFDX engineer [George Murnock](https://twitter.com/gmurnock__c) and platform architect [Shane McLaughlin](https://twitter.com/MShaneMc/status/948991161389273090), is to include `**profiles` to your `.forceignore` file. This will force you to use permsets.

Ideally, try to avoid profiles. Sadly, since they’re so pervasive, Trailhead often assumes (and even checks) for the existence of the profile, so sometimes you can’t avoid it.

#### [Script this step to make it easier](#script-this-step-to-make-it-easier)

I’ve created an alias that makes it really easy to deploy to my TPO.

```
alias mdd='function _blah(){ sfdx force:source:convert -d mdapiout && /
  sfdx force:mdapi:deploy -d mdapiout --wait 100 -u $1 && rm -rf mdapiout };_blah'
```

With this alias, I can convert, deploy, and clean all with one command: `mdd tpo`

Definitely a lot easier!

### [Additional things to know](#additional-things-to-know)

Certain modules will require you to do additional things.

#### [Installing required packages](#installing-required-packages)

As pointed out by [Bonny Hinners](https://twitter.com/SNUGSFBay/status/948927117248491520), there are times when you need to install some dependencies (typically via an unmanaged package but possibly through a labs app too) into your TPO to complete a challenge. This can give you custom objects and other metadata you’ll use within the challenge. You’ll need to do the same thing with your scratch org.

A good example is the [Apex Specialist Superbadge](https://trailhead.salesforce.com/en/super_badges/superbadge_apex). Notice that the pre-work includes an unmanaged package you need to install.

No problem, we can install this unmanaged package in the scratch org. Then, any changes we make, even if these changes are made against metadata that comes from the package, will get pulled out.

1. Figure out the package ID. Usually this is pretty simple, if not transparent. Notice the URL for the unmanaged package: `https://login.salesforce.com/packaging/installPackage.apexp?p0=04t36000000i5UM`. An ID that starts with `04t` is a package. Copy this package ID.
2. Create your scratch org the normal way we highlighted above.
3. Install the package into your scratch org: `sfdx force:package:install -i 04t36000000i5UM --wait 100`

That’s it! Now all the required schema and dependencies are in your scratch org.

#### [Exporting and importing data](#exporting-and-importing-data)

A few of the challenges will verify that you’ve created data in the TPO. Now, of course you could open the TPO and manually enter the data it’s looking for. But where’s the fun in that? Instead, do it in your scratch org, export it, and then import it into your TPO.

This is a great way to discover some of the useful data import/export commands in the CLI.

1. Open your scratch org and enter your data. Yes, you can do this through other tools, but let’s face it: the app you’ve built is likely the best interface for creating your data.
2. Use the CLI to write a query that will grab the data you added: `sfdx force:data:soql:query -q "SELECT Waypoint_Name__c FROM Waypoints__c"`. Ensure you get the data you’re expecting.
3. Export to JSON files and store in your project (other developers on your team will appreciate you doing this; trust me): `sfdx force:data:tree:export -q "SELECT Waypoint_Name__c FROM Waypoints__c" -p -d data`. Notice it’s the same query as before. Also, we’re using a plan definition file and exporting it into the `data` folder.
4. Now, import the data into your TPO: `sfdx force:data:tree:import -p data/Waypoint__c-plan.json -u tpo`. This will create the records in our TPO without you having to do it manually.

Pretty slick.

#### [Executing anonymous Apex code](#executing-anonymous-apex-code)

There are some things that either don’t fit into the model of source push/pull (e.g. connected apps) or will require you to take a different approach to updating in the org. So, get ready to bust out some anonymous Apex!

Here’s what I do:

1. I create a folder called `scripts`. This is where I’ll create my Apex scripts.
2. Create a file with an `.apex` extension in the `scripts` folder. Need to create a scheduled job? Call it `scheduledjob.apex`.
3. Write your code. Example:

   ```
   System.schedule('WarehouseCallout','0 0 13 * * ?' ,
       new WarehouseSyncSchedule())
   ```
4. Execute the script against your scratch org to test: `sfdx force:apex:execute -f scripts/scheduledjob.apex`. Note: sometimes to get this right, you’ll want to create a new scratch org to try again. I’ve gone through many iterations sometimes until I finally get it working the way I want.
5. Execute the script against your TPO: `sfdx force:apex:execute -f scripts/servicetoken.apex -u tpo`.

Notice how easy that is? And, even better, it’s completely repeatable!

#### [Opening up a Lightning App](#opening-up-a-lightning-app)

In many of the modules you’ll create a lightning app as a test harness called `harnessApp.app` (see [Create and Edit Lightning Components](https://trailhead.salesforce.com/modules/lex_dev_lc_basics/units/lex_dev_lc_basics_create) for an example). This test harness is used to test the Lightning Components as you create them. Super helpful!

Now, Trailhead assumes you’re doing your development in the Developer Console. If you’re using Salesforce DX, how do you easily open `harnessApp.app` to test your component? Well, the CLI let’s you target a specific page:

```
$ sfdx force:org:open -p c/harnessApp.app
```

This will directly open your `harnessApp.app` in the browser, without even having to login. Pretty cool, right?

Of course, if you want to test it in your TPO, just add your alias (e.g. `-u tpo`).

### [Wrap it up](#wrap-it-up)

That’s pretty much it! You can do this over and over again. The best part is, as I mentioned, you can setup version control and push all your source.

In your project, it’s as simple as the following:

```
$ git init
$ git remote add origin git@github.com:wadewegner/th-trailblazerapp.git
$ git add -A
$ git commit -m "It works"
$ git push origin master
```

And then you’ll have a nice repo of your code to refer to in the future: <https://github.com/wadewegner/th-trailblazerapp>

Hope this helps!

P.S. Thanks to George Murnock for literally finding (and pointing out) hundreds of typos.
