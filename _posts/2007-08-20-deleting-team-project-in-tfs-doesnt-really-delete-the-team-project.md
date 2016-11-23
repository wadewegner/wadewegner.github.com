---
author: admin
comments: true
date: 2007-08-20 02:46:45+00:00
layout: post
slug: deleting-team-project-in-tfs-doesnt-really-delete-the-team-project
title: Deleting Team Project in TFS doesn't really delete the Team Project
wordpress_id: 40
categories:
- Team Foundation Server
---

One of the things I learned while [setting up a virtual Team Foundation Server (TFS) environment](http://www.wadewegner.com/2007/08/20/SettingUpAVirtualVisualStudio2005TeamFoundationServer.aspx) is that there are definitely a few "gotchas." I encountered one of these while writing up some documentation on how to integrate a Web Application Project (WAP).

Before I began documenting the procedure, I went through and confirmed that everything would work. When I was finished, I figured I would delete the Team Project and start over. However, this didn't work as I expected. Here's what I did:

  1. Created Team Project.

  2. Added my WAP to the Team Project source control.

  3. Created a new Build Type.

  4. Ran the new build. Worked fine.

  5. Deleted the Team Project. 

    * Clicked Start -> All Programs -> Microsoft Visual Studio 2005 -> Visual Studio Tools -> Visual Studio 2005 Command Prompt

    * Typed the command: TFSDeleteProject /Server:<ServerName> <TeamProjectName>


  6. Created a new Team Project with the same name.

  7. Added the WAP to the Team Project source control.

  8. Created a new Build Type.

  9. Ran the build and received the following error:

![TF50608: Unable to retrieve information for security object $PROJECT:vsts:///Classification/TeamProject/<guid, it does not exist.](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/441054752d16_DDAD/image_thumb.png)

> TF50608: Unable to retrieve information for security object $PROJECT:vsts:///Classification/TeamProject/<guid, it does not exist.

Turns out that when you delete a Team Project, it isn't really deleted. Parts of the project, including version control and work items, are maintained in the Team Foundation Data Warehouse and aren't removed when the project is deleted. Consequently, when you recreate a Team Project there is lingering data.

This is pretty disappointing; it seems that, for all intents and purposes, TFS is corrupt. The only way I could move forward was to create a new Team Project with a different name.

Anyone have any insight?