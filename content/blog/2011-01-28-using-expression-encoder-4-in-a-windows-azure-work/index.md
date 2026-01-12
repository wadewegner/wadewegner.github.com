---
title: "Using Expression Encoder 4 in a Windows Azure Worker Role"
date: 2011-01-28T00:00:00-08:00
draft: false
description: "From the whiteboard to the keyboard"
---

One of the most common workloads I hear about from customers and developers is video encoding in Windows Azure. There are certainly many ways to perform video encoding in Windows Azure – in fact, at [PDC 2009 I was joined by Mark Richards](http://www.microsoftpdc.com/2009/SVC22) of Origin Digital who discussed their [video transcoding solution on Windows Azure](http://www.prnewswire.com/news-releases/origin-digital-builds-on-the-windows-azure-platform-to-support-cloud-computing-for-media-management-and-publishing-70366432.html) – but until the release of Windows Azure 1.3 SDK, and in particular Startup Tasks and elevated privileges, it was complicated.

As I looked into this workload, I decided that I wanted to prove out a few things in a small spike:

1. It is *not* necessary to use the VM Role. The VM Role was introduced to make the process of migrating existing Windows Server applications to Windows Azure easier and faster, especially when this involves long, non-scriptable or fragile installation steps. In this particular case, I was pretty sure I could script the setup process using Startup Tasks.
2. It is possible to use the freely available Expression Encoder 4.
3. It is possible to use the Web Platform Installer to install Expression Encoder 4.

Before I go any farther, make sure you review two great blog posts by Steve Marx – be sure to read these as they are a great starting point for Startup Tasks:

- [Introduction to Windows Azure Startup Tasks](http://blog.smarx.com/posts/introduction-to-windows-azure-startup-tasks)
- [Windows Azure Startup Tasks: Tips, Tricks, and Gotchas](http://blog.smarx.com/posts/windows-azure-startup-tasks-tips-tricks-and-gotchas)

Okay, now that everyone’s familiar with Startup Tasks, let’s jump into what it takes to get the video encoding solution working.

I decided to put the encoding service in a Worker Role – it makes the most sense, as all I want the role to do is encode my videos. Consequently, I added a Startup Task to the Service Definition that points to a script:

```
1.  <Startup>
2.    <Task commandLine="Startup\InstallEncoder.cmd" executionContext="elevated" taskType="background" />
3.  </Startup>
```

Note that I have the task type defined as “background”. As Steve explains in his post, this makes it easy during development to debug; however, when you’re ready to push this to production, be sure to change this to “simple” so that the script runs to completion before any other code runs.

The next thing to do is add the tools to our project that are needed to configure the instance and install Encoder. Here’s what you need:

- [WebPICmdLine](http://blogs.iis.net/satishl/archive/2011/01/26/webpi-command-line.aspx) – the executable and it’s assemblies
- InstallEncoder.cmd – this is our script that’s used to set everything up
- PsExec.exe (optional) – I like to include this for debugging purposes (e.g. psexec –s -i cmd)

As I mentioned in my post on [Web Deploy with Windows Azure](http://www.wadewegner.com/2010/12/using-web-deploy-with-windows-azure-for-rapid-development/), make sure to mark these files as “Copy to Output Directory” in Visual Studio. I like to organize these files like this:

![image](https://wadewegner.blob.core.windows.net/wordpress/2011/01/image2.png)

Now, let’s take a look at the InstallEncoder.cmd script, as that’s where the real work is done. Here’s the script:

```
1.  REM : Install the Desktop Experience
2.  ServerManagerCMD.exe -install Desktop-Experience -restart -resultPath results.xml
3.  REM : Make a folder for the AppData
4.  md "%~dp0appdata"
5.  REM : Change the location of the Local AppData
6.  reg add "hku.defaultsoftwaremicrosoftwindowscurrentversionexploreruser shell folders" /v "Local AppData" /t REG_EXPAND_SZ /d "%~dp0appdata" /f
7.  REM : Install Encoder
8.  "%~dp0webpicmdWebPICmdLine.exe" /accepteula /Products: ExpressionEncoder4 /log:encoder.txt
9.  REM : Change the location of the Local AppData back to default
10. reg add "hku.defaultsoftwaremicrosoftwindowscurrentversionexploreruser shell folders" /v "Local AppData" /t REG_EXPAND_SZ /d %%USERPROFILE%%AppDataLocal /f
11. REM : Exit gracefully
12. exit /b 0
```

Let’s break it down:

- **Line #2:** Turns out that you need to install the Desktop Experience in the instance (it’s Windows Server 2008, after all) before you can run Expression Encoder 4. You can do this easily with ServerManagerCMD.exe, but you’ll have to restart the machine for it to take affect.
- **Line #4:** Create a new folder called “appdata”. (Incidentally, %~dp0 refers the directory where the batch file lives.)
- **Line #6:** Update the “Local AppData” folder in the registry to temporarily use our “appdata” folder.
- **Line #8:** Install Expression Encoder 4 using the [WebPICmdLine](http://blogs.iis.net/satishl/archive/2011/01/26/webpi-command-line.aspx). Note that with the latest drop you’ll need to add the “/accepteula” flag to avoid user interaction.
- **Line #10:** Revert the “Local AppData” back to the default.
- **Line #12:** Exit gracefully, so that Windows Azure doesn’t think there was an error.

If you need to restart your machine before you’ve installed everything in your startup task script, as I did, make sure the script is idempotent. Specifically in this case, since we restart the machine after installing the Desktop Experience, line #2 will get run again when the machine starts up. Fortunately ServerManagerCMD.exe has no problem running when the Desktop Experience is installed.

This is all that’s required to setup the Worker Role. At this point, it has all the software required in order to start encoding videos. Pretty slick, huh?

Unfortunately, there’s one other thing to be aware of – the Expression Encoder assemblies are all 32-bit, and consequently they will not run when targeting x64 or Any CPU. This means that you will need to start a new process to host the Encoder assemblies and our encoding process. Not a big deal. Here’s what you can do.

Create a new Console project – I called mine “DllHostx86” based on some guidance I gleamed from [Hani Tech’s post](http://blogs.msdn.com/b/haniatassi/archive/2009/03/20/using-a-32bit-dll-in-the-windows-azure.aspx) – and add the Expression Encoder assemblies:

![image](https://wadewegner.blob.core.windows.net/wordpress/2011/01/image3.png)

Update the platform target so that it’s explicitly targeting x86 – this is required with these assemblies, as I’ve noted elsewhere in my blog (see [Using the Expression Encoder SDK to encode lots of videos](http://www.wadewegner.com/2010/08/using-the-expression-encoder-sdk-to-encode-lots-of-videos/)).

![image](https://wadewegner.blob.core.windows.net/wordpress/2011/01/image4.png)

Now you can write the code in the Program.cs file to encode the videos using the Encoder APIs. This itself is pretty easy and straightforward, and I invite you to review the post I mention above to see how. Also, if you want to do something kind of fun you can add a [text overlay on the video](http://www.wadewegner.com/2011/01/overlay-text-on-video-using-expression-encoder-4/), just to show you can.

The next step is to make sure that the DllHostx86 executable is available to include as part of our Worker Role package – we’ll want to run it as a separate process outside of the Worker Role. Simply update the output path so that it points to a folder that exists in your Worker Role project.

![image](https://wadewegner.blob.core.windows.net/wordpress/2011/01/image5.png)

This way the any build will create an executable that exists within the Worker Role project folder, and consequently can be included within the Visual Studio project (be sure to set them to copy to the output directory). The end result will look like this:

![image](https://wadewegner.blob.core.windows.net/wordpress/2011/01/image6.png)

Now, all we have to do is write some code that will create a new process and execute our DllHostx86.exe file.

```
1.  private void ExecuteEncoderHost()
2.  {
3.      string dllHostPath = @"Redist\DllHostx86.exe";
4.
5.      ProcessStartInfo psi = new ProcessStartInfo(dllHostPath);
6.      Trace.WriteLine("Starting DllHostx86.exe process", "Information");
7.
8.      using (Process dllHost = new Process())
9.      {
10.         dllHost.StartInfo = psi;
11.         dllHost.Start();
12.         dllHost.WaitForExit();
13.     }
14. }
```

While I think this is pretty straightforward, a few comments:

- **Line #5:** If you want to pass parameters to the applications (which I ultimately do in my solution) you can use an overload of ProcessStartInfo to pass them in.
- **Line #12:** Since my simple spike isn’t meant to handle multiple a lot of these processes at once, I use WaitForExit() to ensure that the encoding process completes before continuing.

And there you have it! That’s all you need to do to setup a Windows Azure Worker Role to use Expression Encoder 4 to encode videos!

If you want to see an example of this running, you can try out my initial spike: <http://encoder.cloudapp.net/>.

This is just a simple, but hopefully effect, sample that illustrates how simple it is to setup the Worker Role using Startup Tasks. With this solution in place, you can scale out as many Worker Roles as you deem necessary to process video – in fact, at one point I scaled up to 10 instances and watched them all encoding my videos. In fact, I bet you could use some of the tips I illustrate in [Using the Expression Encoder SDK to Encode Lots of Videos](http://www.wadewegner.com/2010/08/using-the-expression-encoder-sdk-to-encode-lots-of-videos/) to run multiple encoding session at a time per instance. Pretty nifty.

I hope this helps!
