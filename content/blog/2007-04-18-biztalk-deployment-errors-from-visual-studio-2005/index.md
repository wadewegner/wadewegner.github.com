---
title: "BizTalk deployment errors from Visual Studio 2005"
date: 2007-04-18T00:00:00-08:00
draft: false
description: "From the whiteboard to the keyboard"
---

So, I was having fun building some Orchestrations and Pipelines in BizTalk 2006, when all of a sudden I started getting some awful errors when I went to deploy from Visual Studio 2005. Here are the errors I received:

```
Object reference not set to an instance of an object.

at Microsoft.BizTalk.Deployment.BizTalkAssembly.GetConnectedPorts(BtsCatalogExplorer explorer, String pipelineFullyQualifiedName)
at Microsoft.BizTalk.Deployment.BizTalkAssembly.RemovePortsUsingPipelines()
at Microsoft.BizTalk.Deployment.BizTalkAssembly.RemovePipelines()
at Microsoft.BizTalk.Deployment.BizTalkAssembly.Remove()
at Microsoft.BizTalk.Deployment.BizTalkAssembly.RemoveNamedModule(String server, String database, String assemblyName, String assemblyVersion, String assemblyCulture, String assemblyPublicKeyToken, ApplicationLog log)

Unspecified exception: "Object reference not set to an instance of an object."

Failed to add resource(s). Change requests failed for some resources. BizTalkAssemblyResourceManager failed to complete end type change request.
Object reference not set to an instance of an object.
```

All I did was update a pipeline that was used by an Orchestration! I tried all kinds of things to get this to work: restarted host instances, restarted the Enterprise Single Sign-On service, performed a full stop on all my applications, and even deleted all my project assemblies from the GAC. Nothing worked except for rebooting.

That is, nothing worked until I decided to actually delete the pipeline from BizTalk application. Once I manually deleted the pipeline, my deployments started working again.

Very strange behavior!

If anyone can explain to me why BizTalk behaves this way, I would greatly appreciate it.
