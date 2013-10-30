---
layout: post
title: "Configuring the Force.com IDE with Eclipse and Using Git"
description: ""
categories:
- Salesforce.com
- Force.com
- Eclipse
- Git
tags: []
---
{% include setup %}

Today I needed to configure Eclipse to use the Force.COM IDE. In the past I've edited Apex and Visualforce code either directly inline ...

![Inline APEX development](http://wadewegner.blob.core.windows.net/wordpress/2013/10/2013-10-30-Inline.png)

... or using the [Developer Console](http://wiki.developerforce.com/page/Developer_Console). Both are decent for quick and dirty updates, but when it comes to applying best practices - i.e. automated builds, source control, and so forth - I don't want to live in the browser.

There's a good wiki article available on the Developer Force website which explains [how to setup Eclipse with the Force.com IDE](http://wiki.developerforce.com/page/Force.com_IDE_Installation), so I won't repeat those instructions. Instead, I want to focus on two things:

1. Provide some context to people that may not have a Java (nor Eclipse) background.
2. Provide a perspective on configuring git (and either Github or Bitbucket) to provide source control for your project.

Nothing earth shattering but hopefully useful.

**Setting Up Eclipse with the Force.com IDE**

As I said above, review this article: [http://wiki.developerforce.com/page/Force.com\_IDE_Installation](http://wiki.developerforce.com/page/Force.com_IDE_Installation)

Eclipse and the Force.com IDE require the Java Runtime Environment (JRE). The most current version (as of this post) of the JRE is **jre-7u45**. However, you will find that you must have JRE SE 6 installed for Eclipse to run.

![Install JRE 6 for Eclipse](http://wadewegner.blob.core.windows.net/wordpress/2013/10/2013-10-30-SoftwareUpdate.png)

There is no need to remove JRE SE 7 if you already have it installed. Go ahead and install JRE SE 6 - they should work side-by-side.

You may have noticed I threw in another acronym - the **SE** in **JRE SE**. SE stands for Standard Edition. You may also come across JDK, which stands for the Java Development Kit. You don't necessarily need the JDK but note that the JDK already includes the JRE.

Setting up Eclipse isn't particularly difficult, but I noticed a few quirks when installing it on OSX Version 10.9 (i.e. Mavericks) - using a different OS version (or Windows) may have different results.

When you download Eclipse (I downloaded **Eclipse 4.3 "Kepler" IDE for Java Developers**) the first thing you'll notice is that it is zipped and there isn't an installer. Take the following steps to set it up. (Incidentally, I was annoyed to see that I had to create and register an account to download Eclipse. Yuck!)

1. Double-click the file to unzip it. If desired, rename the folder to "Eclipse".

2. Copy/drag the "Eclipse" folder into your Applications folder. Be careful to not drag the folder into another folder in the Applications folder.

3. The OS will attempt to protect you from an application downloaded from the Internet. You may see the following warning:

	![](http://wadewegner.blob.core.windows.net/wordpress/2013/10/2013-10-30-UnidentifiedDeveloper.png)

	To resolve this, open the **Eclipse** folder, hold the Control button and click **Eclipse** and select **Open**. You'll be asked to confirm. Once you do you won't have any problems opening it again. If you try to do this after you've added Eclipse to the dock you'll find that it prevents you from opening it.

	If you are prompted to install the JRE SE 6 at this point, do it.

4. I like to have Eclipse on the dock for easy access. Drag the Eclipse icon into your dock - you will now be able to launch Eclipse by clicking on the icon in the dock.

Aside from these notes, just follow the instructions on the wiki article.

**Setting Up Git with the Force.com IDE**

I looked hard to find best practices for using Git with Eclipse and the Force.com IDE. I didn't find much. The two things I found worth mention are:

* [How To Use Git, GitHub and the Force.com IDE with Open Source Labs Apps](How To Use Git, GitHub and the Force.com IDE with Open Source Labs Apps) : This is a decent article by [Reid Carlburg](https://twitter.com/ReidCarlberg), but it's dated and it's likely teams have found better ways to manage the Git integration. I actually pinged Reid about this article and he pointed me to [Mavensmate](http://mavensmate.com/) which looks interesting.

* [EGit](http://www.eclipse.org/egit/) : EGit is an Eclipse provider for Git. It looks like a great tool for folks who want to use an integrated tool in the Eclipse IDE. Personally, I prefer to live in the console / Git Bash (or as some friends call it, "the dark place"), so EGit isn't the answer for me.

So, what are good practices for using Git with Eclipse and the Force.com IDE? For now, I'll stick with using the console and git commands. This is where I'm most comfortable.

Here are some of the steps I took.

1. I prefer to have my workspaces in the same folder as the rest of my source/projects. Consequently, I setup my workspace under a **src** folder.

	![Workspace](http://wadewegner.blob.core.windows.net/wordpress/2013/10/2013-10-30-Workspace.png)

2. Now create a new repository (you can use Github, Bitbucket, or your preferred git provider).

3. Now that you have a repository, initialize git and setup a remote. Go to the folder and run the following commands in the git bash (I've given an example for Bitbucket and Github - just choose one or the other):

		git init
		git remote add origin ssh://git@bitbucket.org/[YourUsername]/[YourRepository].git
		git remote add origin ssh://git@github.com:[YourUsername]/[YourRepository].git

4. Now you simply have to add, commit, and push your files:

		git add -A
		git commit -m "New Force.com project"
		git push origin master

And you're done. Your Force.com project is now part of your git repository. Just make sure you commit your changes often and appropriately.

I'll leave this post with two questions for you:

* What best practices for Git and Force.com development has your team found?

* How have you setup your .gitignore file? I've dug around and haven't found any examples.

Please leave your responses in the comments below. As I get more experience developing with these tools, I'm sure the notes above will change. I'll try to keep this post updated.

I hope this helps!