---
title: "Mahalo a Aloha, Salesforce"
date: 2021-01-13T14:56:45-08:00
draft: false
---

I never thought I’d be writing this blog post, yet here I am. After an incredible five years at Salesforce, it’s time to say “Mahalo a Aloha,” or thank you and goodbye.  

I have so much gratitude for my colleagues at Salesforce, our customers, and of course, the fantastic Salesforce community. Never in my career have I felt the sense of mission that I have at Salesforce. The desire to advance the “state-of-the-art” for Salesforce developers was a passion reciprocated by my colleagues, supported by leadership, and always appreciated by our customers.

I thought it’d be fitting to use this opportunity to celebrate a few of my favorite accomplishments while recognizing some of the people who were key in their delivery and who’ve meant so much to me on this journey. I’ve learned so much from them and, because of their professionalism and our time together, I've grown as a leader and a person.

### Salesforce DX

So, what is Salesforce DX? Ha! After five years, I’m not sure I can quickly answer this question anymore.

The journey started with a recognition by product and engineering leadership that it was too hard for developers—particularly ISVs—to build and manage their apps on the Salesforce platform. But it turned into so much more. We’ve seen some incredible innovation released under the banner of Salesforce DX, most of it focused on making development more modern and predictable. And what’s so rewarding about this work is how foundational it has been. Without it, other advances like Lightning Web Components, building Salesforce Functions, and all kinds of developer productivity delivered from us and our partners either wouldn’t have been possible or looked very different.

I’d be remiss if I didn’t include a callout to the Dreamforce Developer Keynote from 2016. What an exciting time! I was so honored to join Srini Tallapragada and John Vogt — a good friend and one of my longtime DX product partners here at Salesforce — to introduce a fundamentally new approach to building apps on Salesforce. I honestly can’t believe they let me and John Vogt go on so long, but I still think the demo was absolutely amazing! Definitely still worth watching, even after five years.

[![image](https://user-images.githubusercontent.com/746259/104525680-8690a500-55b5-11eb-8c9f-437dbf400271.png)](https://youtu.be/RMKFTLtn0-E?t=1881)

> The video above will jump directly to my part in the keynote. Don’t you just love seeing how supportive Sarah Franklin is before and after I speak? She’s amazing and I’m so blessed to have had the fortune to work with her.

A lot of people came together to make Salesforce DX possible. It would be a travesty to not call them all out, so I’m not even going to try. Except, yeah, I’m going to try.

Anastasia Shpurik, Andy Fawcett, Carolyn Grabill, Carolyn James, Chad Labrosse, Chris Peterson, Chris Wall, Claire Bianchi, Dave Carroll, Dileep Burki, Emily Kapner, Federico Recio, George Murnock, Ginny Peterson, Greg Wester, Gunnar Wagenknecht, Jeff Bartolotta, Jiahan Ericsson, Jim Spagnola, Joe McTee, John Vogt, Josh Kaplan, Karen Fidelak, Kate Bierbaum, Kim Shain, Kyle Herbig, Luis Campos-Guajardo, Lydia Mann, Martha Morgan, Mike Boilen, Mike Hoefer, Mike Miller, Mike Olson, Nathan Totten, Nick Chen, Peter Hale, Peter Wisnovsky, Raj Advani, Reid Elliott, Rohit Mehta, Ruth Sears, Shane McLaughlin, Thomas Dvornik, Vladimir Gerasimov, Zay Guan, and Zayne Turner.

You are all amazing! I have meaningful memories of working with all of you, and it was a privilege.

(The best part about this list is I accept pull requests … seriously, please ping me to remind me of the people I’m missing!)

I also want to acknowledge four amazing partners in this journey: Jim Wunderlich, the principal architect who had the vision to advocate for the work we did; Scott Musson, the engineering leader who’s leadership enabled us to successfully build and ship these capabilities; Angela McCole, our brave and tireless TPM who kept us aligned and always moving forward; and Natala Menezes, an amazing product marketer who for years has helped us to better understand and articulate what it is we’re doing for developers.

(But geez, Natala, why did you make me wear that suit jacket?)

Seriously, consider everything we introduced that year:

- Salesforce CLI
- Force IDE
- Scratch Orgs
- Salesforce DX Source Definition and Projects
- Next Generation Packaging
- Support for VCS
- Support for CI/CD and automation in general

But it all didn’t come together overnight. I’d be lying if I said it all worked just the way we wanted from the beginning. Some things have taken longer than others to deliver on. Transforming a 16-year-old company’s developer experience doesn’t change overnight, but I still believe we focused on the right goals. And our commitment to removing the pain and challenges our customers felt was sincere — we reviewed and tried to understand every bug or defect reported to us, and I’m proud to say that what we have today is stable and works as expected.

### Salesforce CLI and OCLIF

I’m seriously a CLI junkie. I just love writing scripts and automation.

<img width="600" alt="sfdx-cli-automation" src="https://user-images.githubusercontent.com/746259/34654565-87851aa8-f3b2-11e7-9745-83181d2dd234.gif">

One of the first major ways I feel I contributed personally to the Salesforce developer experience was when I learned that the CLI we were building wasn’t based on the great work the engineers at Heroku had put into their CLI. For those of you who’ve used Heroku, you know that their CLI is amazing and ultimately has led to the Open CLI Framework (OCLIF) that’s now adopted by many companies. To not use what Heroku created would have not only made things a lot harder, but would have made integration of the two developer experiences even more difficult down the road. I think we ultimately made the right call.

I had a lot of fun with our CLI. The best part is how easy it is to build plugins. They’re so powerful. I built a few.

- https://github.com/wadewegner/sfdx-waw-plugin
- https://github.com/wadewegner/sfdx-wsdl2apex-plugin
- https://github.com/wadewegner/sfdx-platformencryption
- https://github.com/wadewegner/sfdx-userassist-plugin

But my learning didn’t stop there. I even dug into Zsh and Bash autocompletion and had fun building some powerful ways to provide tab completion in the CLI.

- https://github.com/wadewegner/salesforce-cli-zsh-completion
- https://github.com/wadewegner/salesforce-cli-bash-completion

Claire Bianchi and Thomas Dvornik are my heroes and their continued focus on the CLI is incredible. What’s great is the team now builds not only the SFDX CLI, but also the Heroku CLI and OCLIF. I’d be remiss, though, if I didn’t call-out the original PM for the CLI, and that’s John Vogt. Back in the day, the core function of the CLI was to move metadata in and out of an org, facilitating all the “magic” performed by Salesforce DX. We’d never have done it without John.

### Salesforce Extensions for Visual Studio Code

Over three years ago I wrote a vision document called the “Developer Tools Strategy” that outlined a new approach to our tools. A big part of it was adopting the tools that developers loved and were designed for the web. VS Code was an easy choice for us as developers, but candidly was a hard sell to leadership. But I’m proud to say we made our case and, well, the rest is history.

I’m blown away by all the outstanding extensions we’ve built.

![image](https://user-images.githubusercontent.com/746259/104526186-ad031000-55b6-11eb-8fc4-17115af3bbb4.png)

And it’s not even just our extensions, but there’s now an entire ecosystem of developers building tooling for VS Code and Salesforce Developers. I think we did the right thing to take advantage of VS Code rather than building our own proprietary IDE.

Behind the scenes, we standardized on the Language Server Protocol for providing code completion, navigation, diagnostics, and more, for Salesforce proprietary frameworks/languages like Apex, Visualforce, Lightning, and LWC. These investments not only supported our use of VS Code, but are now paying off with respect to our work on Code Builder, which is the soon-to-come Web IDE.

The team is now in the very capable hands of product manager Stephanie Maddox, but distinguished alums include Nate Totten, Dave Carroll, Josh Kaplan, and Greg Wester. And I’d be remiss to not call out Nick Chen, one of my favorite people and an incredible engineer and technologist here at Salesforce. Nick has had his hands in almost all of the work our teams have done, and we’re better for it!

### Heroku Buildpack for Salesforce

I remember Josh Kaplan telling me a powerful story. He was watching a Developer Keynote at Dreamforce (perhaps 2015?), as a Salesforce employee, and saw someone — maybe Dave Carroll or Jesper Jorgenson — demo Heroku Pipelines for the first time. Josh thought to himself, “I want that for Salesforce Developers!”

He was so right. And boy did we come close. So close!

Check this out if interested: https://github.com/heroku/salesforce-buildpack

![pipelines](https://user-images.githubusercontent.com/746259/104529828-74b3ff80-55bf-11eb-9032-6ea0c2241535.png)

I got my hands dirty on this project. And it was fun to do it along with Chris Wall, who shared my passion. Doug Ayers, a wonderful technologist and educator, ran with the work we did and put together all kinds of training materials. I’m forever indebted to his work. Definitely check out this amazing work from him: https://douglascayers.com/2018/08/10/continuous-delivery-with-salesforce-dx-and-heroku-pipelines/

Of course, there’s more work to do. This is more of an integration or proof-of-concept than anything else. Which leads me into what I think is one of the most exciting and impactful products we’re working on ...

### DevOps Center

I recognize that DevOps Center isn’t yet Generally Available (GA) but we already have customers using it as part of a pilot and the reception and feedback has been really positive.

A consequence of all the great work we did for Salesforce DX is we moved the release management and DevOps needle substantially for programmatic developers but, in many ways, left low-code developers and Salesforce Admins behind. The tools they currently use, such as Change Sets, have many flaws and unfortunately don't work well with Salesforce DX. The whole idea of working as a team was unfortunately made more complex right from the very beginning.

![image](https://user-images.githubusercontent.com/746259/104529314-4a157700-55be-11eb-800d-9679a45c9eb0.png)

DevOps Center reimagines the development lifecycle for the team. It creates meaningful abstractions over Git so that non-programmatic developers — even if they don’t understand how to perform a Git commit or know what a pull request is — to engage in these activities so that the apps they’re building, and the source/metadata they’ve affected, lives in the same repository as the programmatic developers. Not only is this incredibly powerful, but it’s desperately needed. It allows non-programmatic developers to partake in CI and CD in ways that, today, are much more difficult.

I strongly recommend reading this great blog post by Mike Gerholdt entitled, [Why the New DevOps Center in Salesforce Is Awesome for Admins](https://admin.salesforce.com/blog/2020/new-devops-center-is-awesome-for-admins).

I’m sad I won’t see this product through GA. But I’ll keep an eye on it, and continue to root for Karen Fidalek — the amazing PM who’s led this team — as she brings it to customers.

### Work.com

I will forever be amazed, humbled, and appreciative of all the people who came together to help us ship Work.com this past summer. Allow me a moment to share some history.

I won’t repeat what we all know except to say that 2020 caught us all by surprise. I remember all the work our teams put into our V2MOM for FY21, only to have things change overnight. (Don’t know what a V2MOM is? Read [this post](https://www.salesforce.com/blog/how-to-create-alignment-within-your-company/) from Benioff.) By March of last year, Marc and other leaders had already identified that this was an opportunity for us to do good work by doing good. 

The first vision of what we could do was written by none other than Bill Patterson. It was inspiring, scrappy, and more than anything, audacious in its scope and timing. Marc was challenging us to ship a brand new product to help our customers support their employees during the pandemic and he wanted it tomorrow.

The rest is history. While at times stressful and confusing, a large cross-functional group came together and shipped the first version of Work.com in less than two months. The entire company mobilized around this effort. It was incredible. I saw the very best work from my colleagues. All of us were dealing with the pandemic in our own way, and I think that made the need to deliver something that could help even more important to us.

<img width="784" alt="workcom" src="https://user-images.githubusercontent.com/746259/104527403-7da1d280-55b9-11eb-80d7-849be8e2fbdc.png">

I’d like to call out a few amazing people who were instrumental in Work.com.

While many of us were still scratching our heads and trying to figure out what to do, Ashley McDermott — an amazing TPM partner I’m lucky to have worked with for the last couple years — took charge and started running daily standups. She helped us confront open questions head on and bring clarity. Diem Nguyen took what Ashley started, and scaled it to incredible levels, driving the largest cross-functional working group I’ve ever been a part of. I’m in awe of Diem’s confident and patient leadership at scale. And of course, my partner in crime, Taksina Eammano. Taks was able to mobilize parts of the company I didn’t even know existed, and at the same time engaged in countless customer conversations and brought so much amazing insight to the team.

I learned so much by watching these amazing women perform their jobs with humility and excellence. 

### Salesforce Functions

Before I talk about Salesforce Functions, let me take a moment to talk about one of my best hires of all time … none other than Andrew Fawcett. Andy is a unique combination of incredible technical depth, an ability to see connections that don’t yet exist, humility, and a strong commitment to people and customers. With his deep knowledge of all things Salesforce Platform, and insatiable desire to build incredible things, Andy was able to see connections well ahead of most of us. And his patient yet unwavering leadership has literally moved mountains.

Case in point, Salesforce Functions.

![image](https://user-images.githubusercontent.com/746259/104528021-0b31f200-55bb-11eb-864d-3b4f01276132.png)

There’s a lot to be excited about with respect to Salesforce Functions:

- Unlocking languages beyond Apex (no this isn’t a knock on Apex)
- Running outside of the core platform to get more elasticity
- Availability of libraries and packages you can build upon
- New billing construct and pay for what you use

But a lot goes into building the underlying runtime that will unlock years and years of innovation. The brilliant engineers and architects at Heroku are building a platform that lives within the Salesforce trust boundary. That means identity, native access to Salesforce data, multitenancy … all these things are core advantages to developers running their code with Salesforce.

While Salesforce Functions as a deliverable may seem like a small thing given the years it’s taken to ship, consider everything built to support it. There will be “fast follows” of many incredible capabilities based on and built on this infrastructure, and it’s so exciting to see how far we’ve come!

### #OnePlatform

When Sarah Franklin presented the Platform keynote at Dreamforce 2019, she mentioned something we’ve talked about called #OnePlatform. This has gone by many different names internally, but the idea is simple: bring the best of Heroku and Force.com together so that our developers have a consistent application model with which to build apps for Salesforce.

It turns out that, in practice, this is harder than it sounds.

Many who read this post (and let’s be honest, few will get here) will know that behind a simple hashtag is so much more. The transformation that so many of us were and are a part of has been technically, operationally, and emotionally challenging. But I believe taking on these hard things is not only important but a responsibility. There’s more work to do, but this past year has seen so many consequential changes and I have a lot of confidence in the team to continue this transformation.

### Can We Finally Wrap This Up?

I really hope you all forgive me for taking so much time to reflect on the last five years. I’m a bit shocked by how much I had to say so far, and yet, there’s so much more.

- **Next Generation Sandboxes**: Rohit Mehta, the Sandbox engineering team, and the CRM infra team who helped make this possible — Subho Chatterjee, Nat Wyatt, Robert Frankus, and Rajkumar Irudayaraj — are all heroes. The underlying technology that makes these instant copies and clones possible is so mind blowing.
- **Unlocked and Managed 2GPs**: I had no idea what a journey it would be to reinvent packaging on the platform. 2GPs are the future, without a doubt. But getting there has been so hard. And this is one of the reasons I have so much respect and admiration for Dileep Burki, who has continued to advance the cause for as long as I’ve been at Salesforce. I know we’ve said this many times, but we really are on the precipice of the promise 2GPs bring to Salesforce. 
- **Package Push Upgrades API and Feature Management**: While these two things aren’t directly related, they are important capabilities Dileep and team have brought to ISV developers. And these two are really great representations of why it’s so rewarding to build platforms, because we built these capabilities, and we have seen all the powerful ways Salesforce Developers have used them.
- **Metadata One**: Who loves gaps in APIs? Who wants scratch orgs that don’t reflect your production org? Not this developer! This is one of those programs that you rarely hear about but it is so important, removing all the papercuts that make building things hard. Karen Fidelak did an amazing job driving this program from the very beginning and really transformed the Salesforce DX experience in the process.
- **Dependency API**: The origins of this work is really interesting. We saw customers struggle to decompose their “happy soup” and make sense of their metadata. Andy Fawcett and Mike Boilen dove right in and it was clear there was more value here than we realized. Just look at what Vladimir Gerasimov has done since, including the “find my field” button. The best part is simply to see what amazing things developers do when you create an API like this one — it’s always a surprise and delight!
- **Scratch Org Shape and Snapshots**: Highly anticipated, and a long time coming, these two capabilities of scratch orgs are must-haves for serious developers. With Rohit’s leadership, they’re so close to GA!
- **Data Mask**: I learned so much on the Data Mask journey, and was fortunate to learn from so many people — Josh Alexander (the professor!), Blake Poutra (CEO of Phennecs), Rohit Mehta, Brenda Niem … and really, a huge team of awesome people. And I’d be remiss to not mention the role Amanda Grady and Assaf Ben-Gur had in identifying the opportunity, and then the grace to support others to take it to the next level.
- **Apex improvements**: Too many to share (and often internal), but without the strong leadership and devotion of Chris Peterson, developers would have so much more work to do!
- **Apex Compiler**: Speaking of Apex, did you know we completely rebuilt the Apex Compiler and released it and, had we not mentioned it, no one would have known? A work of art. 
- **Security Center**: Another great example of product leaders identifying customer challenges and figuring out ways to bring meaningful products to market. John Whelan saw the opportunity and, working with engineering, put together a plan to make it a reality.

I’m humbled to have been a part of doing such extraordinary things with all of you.

If I may (and why not, it's my blog), to close I’d also like to mention a few people directly and thank them.

**Adam Seligman**: You’ve always believed in me and helped to create opportunities for me. You answer the phone or message immediately and always with a big heart. Thanks for your help (twice!) getting my career going at Salesforce.

**Adrian Kunzle**: While I had many reasons for joining Salesforce, working for you was perhaps the largest. I benefited a lot learning from you and I appreciate how you always provided support and mentorship. You had a vision for the Salesforce Platform that I always believed in, and I am proud for my part in helping to see it realized.

**Andy Fawcett**: As I said above, you were the right person at the right time. Seeing the impact you have had at Salesforce has been a pleasure, and I’m so thankful for the vision you’ve shared with all of us.

**Angela McCole**: In many ways, we started on this journey together as partners! So many of the successes shared above were your successes, and I am so thankful for all the great things we got to do together!

**Ashley McDermott**: You’ve been an awesome partner as we worked together to do incredible things. I value your integrity, your work ethic and professionalism, but mostly how you’re a wonderful person who inspires us all.

**Bill Patterson**: I wish we had spent more time working together. I will forever value your leadership and mentorship while on the work.com journey. Your willingness to take the time and set all other things aside to provide support is uncommon and I’ll remain forever grateful.

**Dimitrua Jackson**: I’ve never had an assistant before and at first I really didn’t know what to do (or not to do). But you so quickly became an indispensable part of my life, and I value you so much as a human being and a friend. Thank you for everything you’ve done for me and our teams!

**Doug Scott**: You provided a ton of leadership and steering at the helm of an enormous ship, yet always took the time to meet, share your candid thoughts, and reflect on the opportunities that lay ahead. Thank you!

**Guy Jenkins**: If I could go back to earlier naive Wade and give him a piece of advice, it would be to partner more closely with you sooner. Your ability to sift through the noise to find signal is unparalleled, and every engagement or partnership with you or your team has made things exponentially better!

**John Stormer**: I have always appreciated and respected the way you bring a smile and positivity to everything you do, even though you have an enormous charter and a seemingly impossible task.

**John Vogt**: We’ve definitely been together through a lot. We’ve had a lot of good times, and definitely our share of frustrations. But the friendship we’ve built and the respect I have for you will forever remain. It’s been awesome working with you all these years.

**Mike Rosenbaum**: To this day I still don’t know why, but you gave me a huge opportunity because you saw something in me. I’ll never forget it and will remain forever indebted.

**Rick Reicker**: You’re the busiest person I know, yet you bring a smile and somehow keep it all together. If there’s any one person who’s been responsible for keeping the platform together and functioning through all the changes over the last few years, it’s been you.

**Sarah Franklin**: I’m proud to call you my boss, a supportive colleague, and of course a friend. From the beginning you were a champion for me and believed in me. I’ve learned a lot from you — fearless and transparent leadership, the value of preparation and practice (lol!), and how you’ll never go wrong when you keep the needs of your customers front and center.

**Scott Musson**: I feel like you and I have been through so much together over the years, and I'm proud to have done it with you. You've been a source of stability, pragmatism, and the often needed kick in the pants. I really value your wisdom, experience, and friendship.

Okay, I think I’ve said enough. Maybe too much. But if you made it this far, I hope it’s clear to you what an impact this great company and these great people have had on me. I’ll never be the same, and I’ll always remain grateful.

🙏