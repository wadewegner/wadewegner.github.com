---
author: admin
categories:
- C#
- Gaming
- Xbox
- XNA
comments: true
date: "2010-01-06T16:50:16Z"
slug: getting-started-with-xna-development
title: Getting Started with XNA Development
wordpress_id: 504
---

![Starflight](https://wadewegner.blob.core.windows.net/wordpress/2010/01/image.png) 

I love gaming. Long before the death match 95 tournament with Windows 95 that [featured Bill Gates in a trench coat](http://www.youtube.com/watch?v=xh0JM7pD4qM&feature=related), I was playing computer games. Two of the first games I remember playing were [The Ancient Art of War](http://en.wikipedia.org/wiki/The_Ancient_Art_of_War) and [Montezuma’s Revenge](http://en.wikipedia.org/wiki/Montezuma%27s_Revenge_(video_game)) – simple but fun. I soon became a Sierra devotee, and devoured games such as [Kings Quest](http://en.wikipedia.org/wiki/Kings_Quest), [Heroes Quest](http://en.wikipedia.org/wiki/Quest_for_glory) (renamed Quest for Glory), [Space Quest](http://en.wikipedia.org/wiki/Space_Quest), [Police Quest](http://en.wikipedia.org/wiki/Police_Quest), and [Leisure Suit Larry](http://en.wikipedia.org/wiki/Leisure_suit_larry) (just don’t tell my dad). However, my favorite all time game was [Starflight](http://en.wikipedia.org/wiki/Starflight) from Binary Systems – this game had everything: space battles, exploration, great story line, and it required strategy and a lot of invested time.

> As an aside, I have seen the Starflight and Starflight 2 executables, along with some of the other games I’ve mentioned, floating around the Internet for awhile. It is possible to use [DOSBox](http://www.dosbox.com/) – an x86 emulator with DOS – to play these games.

 

The common theme to all of this, of course, is that my gaming platform was the PC – while I had a Nintendo, I never really considered it a decent gaming platform. All of the console platforms of the day – Sega, Atari, Nintendo - paled in comparison to the PC as a gaming platform. Even today, I prefer the PC to a console. Despite this prejudice, I have come to love my Xbox 360 – not only is it a great gaming platform, but it’s the media hub of our entire house.

 

![XNA](https://wadewegner.blob.core.windows.net/wordpress/2010/01/image1.png)

For a long time I’ve wanted to try my hand at making a game, but I’m a developer not a designer. Additionally, I know nothing about DirectX, OpenGL, and other multimedia/gaming APIs. While I’ve known about the [XNA](http://en.wikipedia.org/wiki/Microsoft_XNA) since it released in 2006, I never made the time to try it out. For those of you that aren’t familiar with XNA, it’s a framework with a set of tools that facilitates the development and management of computer games. The best part is it uses C#!

 

Now, there’s a lot that goes into making a game. I’m a complete noob, and I’ll try to share the things I learn on this blog as I go. As I see it, the first step is actually getting your tools and environment setup. It is this process that I intend to outline in this article. The process is a bit confusing, but what I’ve been able to piece together are the following three steps:

 

  
  * Setting up your XNA account 
   
  * Configuring your Xbox
   
  * Deploying your first “game” 
 

I should also mention that my goal here is to build and deploy a “game” to the Xbox 360. This may seem contradictory given my stated preference for the PC above, but there’s more to the Xbox 360 than just games, and eventually I’d like to build applications for the Xbox (not just games).

 

#### Setting Up Your XNA Account

 

The first thing to do is get your XNA account all setup. This allows you to deploy games to your Xbox 360. It will also cost a little $$.

 

1. Download **XNA Game Studio 3.1**. I grabbed it via my MSDN membership, but I believe you can find it elsewhere too. You can get it for [Visual C# Express here](http://creators.xna.com/downloads).

 

2. Go to the **XNA Creators Club Online** and setup an account. **Note: It’s very important that you use the same account that’s associated to XBOX Live.** Review the different membership options – you will start off as Registered (after creating an account) and you want Premium.

 

  ![XNA Membership Options](https://wadewegner.blob.core.windows.net/wordpress/2010/01/image2.png)

 

4. Sign up for [Premium Membership](http://creators.xna.com/SendToXboxCom.aspx). There area few ways to do this – if you’re lucky enough to have a Redeem Code, then use it here.

 

  ![XNA Membership Types](https://wadewegner.blob.core.windows.net/wordpress/2010/01/image3.png)

 

5. After you have setup your membership, verify by selecting **Creators** and choose **My Profile**.

 

  ![My Profile](https://wadewegner.blob.core.windows.net/wordpress/2010/01/image4.png)

 

6. Your membership should now be **premium**.

 

  ![My Profile - Premium](https://wadewegner.blob.core.windows.net/wordpress/2010/01/image5.png)

 

While straightforward, I never found this process nicely spelled out and defined.

 

#### Configuring Your Xbox

 

The next thing to do is setup your Xbox such that you can deploy your games to it. This one took me a bit longer to figure out because of all the various steps.

 

1. Go to your Xbox 360 and turn it on.

 

2. Log into your Xbox Live account on your Xbox 360. Remember how you used the same Live ID in step #2 above? This is why you did so.

 

3. The next step is to setup the XNA Game Studio Connect software on your Xbox . Follow these steps:

 

  
  * Select **Game Marketplace**
   
  * Select **Explore Game Content**
   
  * Select **Browse**
   
  * Select title **X**
   
  * Select title **XNA Creators Club**
   
  * Select **All Downloads**
   
  * Select **XNA Game Studio Connec
t**
   
  * Select **Confirm Download**
   
  * Select **Play Now**
 

4. The application will provide you a **connection key** that you’ll use to register your Xbox with your development environment. Write down the key.

 

5. Return to your development machine.

 

6. Open up the **XNA Game Studio Device Center**.

 

7. Click **Add Device**.

 

  ![Add Device](https://wadewegner.blob.core.windows.net/wordpress/2010/01/image6.png)

 

8. Select the **XBOX 360 **icon.

 

  ![XBOX 360](https://wadewegner.blob.core.windows.net/wordpress/2010/01/image7.png)

 

9. Choose an **Xbox 360 Name**.

 

  ![Name](https://wadewegner.blob.core.windows.net/wordpress/2010/01/image8.png)

 

10. Enter the **connection key **you wrote down.

 

  ![Connection Key](https://wadewegner.blob.core.windows.net/wordpress/2010/01/image9.png)

 

11. When you’re complete, click Finish. You should now see your Xbox as a listed device.

 

  ![XNA Game Studio Device Center](https://wadewegner.blob.core.windows.net/wordpress/2010/01/image10.png)

 

Okay, everything’s setup. Now to build and deploy a game.

 

#### Deploying Your First “Game”

 

1. Take a look at this great [“Hello World!” in XNA](http://www.nazspace.com/wp/2007/12/04/hello-world-in-xna/) post by Nezeeh. Follow the steps – they are very simple.

 

2. Ensure that the **XNA Game Studio Connect** application is still running on your XBOX.

 

  ![XNA Game Studio Connect](https://wadewegner.blob.core.windows.net/wordpress/2010/01/Image1.jpg)

 

3. Deploy your application from Visual Studio. It will push the package to the XBOX 360 and should look something like this:

 

  ![Hello World!](https://wadewegner.blob.core.windows.net/wordpress/2010/01/Image2.jpg)

 

And there you have it! Your “game” is running. I apologize for the poor quality pictures – I used my phone as it was easy and available.

 

All in all, this process shouldn’t take you more than an hour. I spent about three hours trying to figure it all out, but I guess I’m just slow. I hope you find this valuable.

 

Happy gaming!
