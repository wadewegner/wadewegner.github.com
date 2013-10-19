---
author: admin
comments: true
date: 2010-08-09 05:31:32+00:00
layout: post
slug: using-the-expression-encoder-sdk-to-encode-lots-of-videos
title: Using the Expression Encoder SDK to encode lots of videos
wordpress_id: 695
categories:
- .NET
- C#
- Expression Encoder
- Tips
---

I spent a good deal of time this weekend importing hours and hours family videos off our Mini DV cassettes. Lots of fun, and LOTS of video! Based on the size of these files, I was quite close to running out of room on my Windows Media Center. So, I decided to encode the files as WMVs. Huge size reduction with very little quality loss.

 

I decided to use Microsoft Expression Encoder 3 – a great tool. The best part is that there’s an SDK and set of assemblies that you can use in your own applications.

 

>   
> 
> Note: if you are using a 64-bit machine, be sure to set the platform target of your application to x86, or else you will get compilation errors from the Encoder assemblies.

 

Below you’ll find the application I wrote. Let me explain my goals:

 

  
  * Multi-thread the application to encode more than one video at a time. 
   
  * Leverage the multitude of cores in my machine. 
   
  * Limit the number of threads (I chose the core count as a baseline). 
   
  * Use source video and audio source to reduce quality lose. 
 

In order to do this, I had to do two things: 1) find a way to pass in the file name into thread, and 2) keep track of the number of threads and limit them to the number of cores in the machine.

 

I spent a bit of time looking for a good approach. In the end, I chose to use the delegate ParameterizedThreadStart, which takes a parameter of type object. This way, I can create a thread using an instance of this delegate instead of just ThreadStart, and the overload to Thread.Start allows me to specify a value that is passed to this new thread. (Be careful, though, as it only accepts a single parameter (although it can be a collection) and isn’t type-safe.) Additionally, with this approach I was able to leverage a counter, and sleep whenever the counter is going to exceed the number of cores in my machine.

 

Here’s all the code. For this to function, I imported the following assemblies (yes, you need Expression Encoder 3):

 

  
  * Microsoft.Expression.Encoder 
   
  * Microsoft.Expression.Encoder.Types 
   
  * Microsoft.Expression.Encoder.Utilities 
   
  * WindowsBase 
 
    
    <span style="color: #008000">// This method is used to look-up the core the thread is using</span>
    
    
    [DllImport("<span style="color: #8b0000">kernel32.dll</span>", CharSet = CharSet.Auto)]
    
    
    <span style="color: #0000ff">public</span> <span style="color: #0000ff">static</span> <span style="color: #0000ff">extern</span> <span style="color: #0000ff">int</span> GetCurrentProcessorNumber();
    
    
     
    
    
    <span style="color: #0000ff">static</span> <span style="color: #0000ff">string</span> inputFolder = @"<span style="color: #8b0000">C:tempVideos</span>";
    
    
    <span style="color: #0000ff">static</span> <span style="color: #0000ff">string</span> outputFolder = @"<span style="color: #8b0000">C:tempOutputVideo</span>";
    
    
    <span style="color: #0000ff">static</span> <span style="color: #0000ff">int</span> count;
    
    
    <span style="color: #0000ff">static</span> <span style="color: #0000ff">int</span> maxNum;
    
    
     
    
    
    <span style="color: #0000ff">static</span> <span style="color: #0000ff">void</span> Main(<span style="color: #0000ff">string</span>[] args)
    
    
    {
    
    
        <span style="color: #008000">// Start the counter at zero</span>
    
    
        count = 0;
    
    
        <span style="color: #008000">// Grab the processor count</span>
    
    
        maxNum = Environment.ProcessorCount;
    
    
        <span style="color: #008000">// Iterate through the AVI files</span>
    
    
        <span style="color: #0000ff">foreach</span> (var fileName <span style="color: #0000ff">in</span> System.IO.Directory.GetFiles(inputFolder, "<span style="color: #8b0000">*.avi</span>"))
    
    
        {
    
    
            <span style="color: #008000">// Sleep/wait for a core to free up</span>
    
    
            <span style="color: #0000ff">while</span> (count > (maxNum - 1))
    
    
            {
    
    
                Thread.Sleep(500);
    
    
            }
    
    
            <span style="color: #008000">// Increment the counter</span>
    
    
            count++;
    
    
            <span style="color: #008000">// Create the thread with the delegate</span>
    
    
            Thread t = <span style="color: #0000ff">new</span> Thread(<span style="color: #0000ff">new</span> ParameterizedThreadStart(EncodeFile));
    
    
            <span style="color: #008000">// Start the thread, passing in the file name</span>
    
    
            t.Start(fileName);
    
    
        }
    
    
    }
    
    
     
    
    
    <span style="color: #0000ff">public</span> <span style="color: #0000ff">static</span> <span style="color: #0000ff">void</span> EncodeFile(<span style="color: #0000ff">object</span> ofileName)
    
    
    {
    
    
        <span style="color: #0000ff">string</span> fileName = (<span style="color: #0000ff">string</span>)ofileName;
    
    
        MediaItem mediaItem = <span style="color: #0000ff">new</span> MediaItem(fileName);
    
    
        mediaItem.OutputFormat = <span style="color: #0000ff">new</span> WindowsMediaOutputFormat();
    
    
        <span style="color: #008000">// Use source video profile if available</span>
    
    
        <span style="color: #0000ff">if</span> (mediaItem.SourceVideoProfile != <span style="color: #0000ff">null</span>)
    
    
        {
    
    
            mediaItem.OutputFormat.VideoProfile = mediaItem.SourceVideoProfile;
    
    
        }
    
    
        <span style="color: #0000ff">else</span>
    
    
        {
    
    
            mediaItem.OutputFormat.VideoProfile = <span style="color: #0000ff">new</span> AdvancedVC1VideoProfile()
    
    
            {
    
    
                Size = mediaItem.MainMediaFile.VideoStreams[0].VideoSize,
    
    
                Bitrate = <span style="color: #0000ff">new</span> ConstantBitrate(1000)
    
    
            };
    
    
        }
    
    
        <span style="color: #008000">// Use source audio profile if available</span>
    
    
        <span style="color: #0000ff">if</span> (mediaItem.SourceAudioProfile != <span style="color: #0000ff">null</span>)
    
    
        {
    
    
            mediaItem.OutputFormat.AudioProfile = mediaItem.SourceAudioProfile;
    
    
        }
    
    
        <span style="color: #0000ff">else</span>
    
    
        {
    
    
            mediaItem.OutputFormat.AudioProfile = <span style="color: #0000ff">new</span> WmaAudioProfile();
    
    
        }
    
    
        <span style="color: #008000">// Create a job and the media item for the video we wish to encode.</span>
    
    
        Job job = <span style="color: #0000ff">new</span> Job();
    
    
        job.MediaItems.Add(mediaItem);
    
    
        <span style="color: #008000">// Set up the progress callback function</span>
    
    
        job.EncodeProgress
    
    
            += <span style="color: #0000ff">new</span> EventHandler<EncodeProgressEventArgs>(OnProgress);
    
    
        <span style="color: #008000">// Set up the completed callback function</span>
    
    
        job.EncodeCompleted
    
    
            += <span style="color: #0000ff">new</span> EventHandler<EncodeCompletedEventArgs>(job_EncodeCompleted);
    
    
        <span style="color: #008000">// Set the output directory and encode</span>
    
    
        job.OutputDirectory = outputFolder;
    
    
        <span style="color: #008000">// Do not create a job subfolder</span>
    
    
        job.CreateSubfolder = <span style="color: #0000ff">false</span>;
    
    
        <span style="color: #008000">// Encode</span>
    
    
        job.Encode();
    
    
    }
    
    
     
    
    
    <span style="color: #0000ff">static</span> <span style="color: #0000ff">void</span> job_EncodeCompleted(<span style="color: #0000ff">object</span> sender, EncodeCompletedEventArgs e)
    
    
    {
    
    
        <span style="color: #008000">// Decrement the counter</span>
    
    
        count--;
    
    
    }
    
    
     
    
    
    <span style="color: #0000ff">static</span> <span style="color: #0000ff">void</span> OnProgress(<span style="color: #0000ff">object</span> sender, EncodeProgressEventArgs e)
    
    
    {
    
    
        <span style="color: #008000">// Write out information</span>
    
    
        Console.WriteLine(
    
    
            count.ToString() + "<span style="color: #8b0000"> : </span>" +
    
    
            GetCurrentProcessorNumber().ToString() + "<span style="color: #8b0000"> : </span>" +
    
    
            System.Threading.Thread.CurrentThread.ManagedThreadId.ToString() + "<span style="color: #8b0000"> : </span>" +
    
    
            e.Progress + "<span style="color: #8b0000"> : </span>" +
    
    
            e.CurrentItem.ActualOutputFileName);
    
    
    }









Good stuff.





I’m running it from the console, so you will have to make some modifications if you want it to run in a more sophisticated application. Works for me, though – I just start it up at the end of the day. Here you can see the information that’s written out to the console (note the variety of cores leveraged):





![Console Output](http://images.wadewegner.com/wordpress/2010/08/console.png)





It’s fun to see my machine working this hard. Every core is pegged.





![Pegged Cores](http://images.wadewegner.com/wordpress/2010/08/image2.png)





Hope someone finds this useful. Anyone see a better way to approach this?
