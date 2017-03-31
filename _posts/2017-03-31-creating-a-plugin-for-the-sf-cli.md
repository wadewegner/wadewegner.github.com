---
layout: post
title: "Creating a Plugin for the Salesforce CLI"
description: "A peek into the mechanics of creating a customer plugin for the Salesforce command-line interface (CLI)."
categories: 
- salesforce
- salesforce dx
- sfdx
- cli
- salesforce cli
---

```
This post is valid for the Salesforce DX (SFDX) pilot. Please note that details may change and I'll do my best to update accordingly.
```

Recently I highlighted a neat way to [use open source software with Salesforce DX](http://www.wadewegner.com/2017/03/salesforce-dx-strike/). Since then I've used Appiphony's custom base components many, many times in demos and apps I've built.

It didn't take long for me to get tired of removing the Git details when pulling the OSS from Github. So I created [sfdx-oss-plugin]( https://github.com/wadewegner/sfdx-oss-plugin). With this command, you can run `sfdx wadewegner:source:oss -r wadewegner/Strike-Components -p .` to grab all the source listed in the [SFDX oss manifest](https://github.com/wadewegner/Strike-Components/blob/master/sfdx-oss-manifest.json) into whatever directory you specify. It also makes it really easy to create a SFDX oss manifest file with the command `sfdx wadewegner:source:create -p .`, which creates the JSON file for everything in the specified path.

Pretty nifty, eh?

This highlights one of the advantages building our Salesforce CLI on the Heroku CLI. Not only do we get to take advantage of frequent updates (both for features and security) of the Heroku CLI, but we can also take advantage of plugin extensibility.

For the most part, building a plugin for the Salesforce CLI is exactly the same as [building a plugin for the Heroku CLI](https://devcenter.heroku.com/articles/developing-cli-plugins). I think you'll find [sfdx-oss-plugin](https://github.com/wadewegner/sfdx-oss-plugin) a very good reference.

One significant change we've made is the introduction of a namespace. You can see it on [index.js#L12](https://github.com/wadewegner/sfdx-oss-plugin/blob/master/index.js#L12) and in the following sample:

```
exports.namespace = {
    name: 'wadewegner',
    description: 'commands from Wade Wegner'
};
```

This organizes all the commands in my plugin under `sfdx wadewegner`.

Overtime, things will change. I'll be sure to update this post accordingly.

I hope you've found this post useful and that you're enjoying the Salesforce DX pilot! To add yourself to the Salesforce DX pilot waitlist, enter your info here: [http://go.pardot.com/l/27572/2017-01-23/6gh89x](http://go.pardot.com/l/27572/2017-01-23/6gh89x)