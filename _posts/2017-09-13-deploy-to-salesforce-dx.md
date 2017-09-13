---
layout: post
title: "Deploy to Salesforce DX"
description: "Deploy to Salesforce DX, an open-source and community-driven tool for automating Salesforce DX deployments from public repositories to Scratch Orgs"
categories: 
- salesforce
- salesforce dx
- sfdx
- cli
- deploy
- scratch org
- oss
- community
---

If you're a Salesforce developer, it's likely you've heard us talk about [Salesforce DX](https://developer.salesforce.com/platform/dx) these past months. (If not, you're missing out and you should checkout the [official documentation](https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev/sfdx_dev_intro.htm).) Since TrailheaDX, everyone has been able to use the new tools and services we've made available as part of the Salesforce DX beta. Personally, I've thoroughly enjoyed building all kinds of plugins for the Salesforce CLI.

- My primary plugin with lots of functionality: [https://github.com/wadewegner/sfdx-waw-plugin](https://github.com/wadewegner/sfdx-waw-plugin)
- Generate code snippets from templates: [https://github.com/wadewegner/sfdx-code-gen](https://github.com/wadewegner/sfdx-code-gen)
- Convert WSDL to Apex: [https://github.com/wadewegner/sfdx-wsdl2apex-plugin](https://github.com/wadewegner/sfdx-wsdl2apex-plugin0)

One of our top goals has been to make deployment incredibly easy. With the Salesforce CLI, deploying your source is as easy as typing `sfdx force:source:push` and, within a few moments, your Scratch Org is refreshed with the latest source. This has proved immensely popular with developers looking for better development experiences.

Yet, during the beta, we've seen a number of use cases for providing the ability to deploy source into a Scratch Org directly from a source control repository like Github:

1) You want to quickly try out the latest and greatest from a [Christophe Coenraets](https://twitter.com/ccoenraets) or [René Winkelmeyer](https://twitter.com/muenzpraeger) blog post. (You know you do!)

2) You're not able to access a machine where you can install the Salesforce CLI. (Maybe you're at a conference?)

3) Laziness. (It's okay, it happens to me too.)

Regardless of the reasons, a few of us agree and thought it worth doing something about it.

Without further ado, I'm **pleased to introduce [Deploy to Salesforce DX](https://deploy-to-sfdx.com)**, an open-source and community-driven tool for automating Salesforce DX deployments from public repositories to Scratch Orgs.

## Background

I started working on this tool a couple months ago. When I began, there were a few blocking issues that prevented it from working. Many thanks to [Thomas Dvornik](https://twitter.com/amphro), one of the lead engineers on the Salesforce CLI team, who added the ability to use access tokens minted from a separate Web Server OAuth flow to authenticate to the Dev Hub. (More on this in a future blog post, as it's an incredibly useful feature.)

Additionally, the first few iterations of this tool significantly lacked polish and simplicity. Without the amazing UX design and development work of [Ben Snyder](https://twitter.com/benjisnyder) this tool would be an ugly mess.

Last, but not least, I'm grateful for the contribution from [Andrew Fawcett](https://twitter.com/andyinthecloud). As the official owner of the "Deploy to Salesforce" button, it pleases me that he found it worthwhile to contribute some useful code to this project, and I'm hopeful that he (and others) continue to contribute.

What's the end game for **Deploy to Salesforce DX**?

That's a good question. I'm as curious as you. This started off as a project to see if it was possible. I had numerous questions. Can this run on Heroku? *(Yes, in fact it can, powered by the [salesforce-cli-buildpack](https://github.com/wadewegner/salesforce-cli-buildpack); more on this in another post!)* Can we orchestrate a Salesforce DX deployment in one click? *(Yes, look at the .salesforcedx.yaml file to see the instructions.)* How hard is it to invoke the CLI itself from code? *(Turns out, it's not too bad!)* Ultimately, my goal with **Deploy to Salesforce DX** is to show what's possible. If it serves nothing more than to let people know all the crazy/awesome things you can do with Salesforce DX, I'll consider it a success.

I have fully open-source this application, so you can see how it works. Andy and I have already started putting together a set of feature requirements, so feel free to [contribute your own](https://github.com/wadewegner/deploy-to-sfdx/issues). I'd also love it if you contributed, so feel free to fork and send me a pull request.

## Try it out

The website itself explains how to get started, to head on over to [https://deploy-to-sfdx.com/](https://deploy-to-sfdx.com/). There are also a few Github repositories out there that already support the tool:

- [https://github.com/forcedotcom/sfdx-dreamhouse](https://github.com/forcedotcom/sfdx-dreamhouse)
- [https://github.com/forcedotcom/sfdx-simple](https://github.com/forcedotcom/sfdx-simple)
- [https://github.com/ccoenraets/northern-trail](https://github.com/ccoenraets/northern-trail)
- [https://github.com/muenzpraeger/salesforce-einstein-platform-apex](https://github.com/muenzpraeger/salesforce-einstein-platform-apex)

Want to add this to your own Github repository? Please do! It's as easy as adding the following tag to your repositories repo:

```
[![Deploy](https://deploy-to-sfdx.com/dist/assets/images/DeployToSFDX.svg)](https://deploy-to-sfdx.com/)
```

That will give you a lovely button that users can click on to deploy your source directly into one of their Scratch Orgs.

![Deploy](https://deploy-to-sfdx.com/dist/assets/images/DeployToSFDX.svg "Deploy")

If you want to further customize your deployment, you can add a `.salesforcedx.yaml` file in your repo. It'll look something like this:

```
scratch-org-def: config/project-scratch-def.json
assign-permset: false
run-apex-tests: true
delete-scratch-org: false
show-scratch-org-url: true
```

This instructs the service to perform additional or custom activities. Definitely useful if you've got a Lightning app or you want to run a set of tests.

## Word of warning

Before I close, I want to remind everyone that this is not an official Salesforce service or tool. While I am a Salesforce employee, I've built this service on the side. I don't offer any official support, although I will do my very best to [respond to issues](https://github.com/wadewegner/deploy-to-sfdx/issues). Since the tool only interacts with Scratch Orgs, the risk is minimal. That said, consider yourself warned.

I hope you find this tool useful. It was a lot of fun to build. Thanks again to all the individuals who helped along the way!