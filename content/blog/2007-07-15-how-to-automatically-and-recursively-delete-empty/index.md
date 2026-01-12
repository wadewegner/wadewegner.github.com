---
title: "How to automatically and recursively delete empty directories"
date: 2007-07-15T00:00:00-08:00
draft: false
description: "From the whiteboard to the keyboard"
---

Years ago I made a serious mistake when I installed, and briefly used, iTunes – I gave it the ability to reorganize my music folders! Since then, all my nicely organized and sorted directories of (legally obtained) digital music have been disorganized and confused.

For example, I used to have a directory named “U2” where all my music for the group U2 was stored. However, iTunes decided that my organization structure was too simple, and broke it out into folders derived from the contributing artists. Now I have “Bono & The MDH Band”, “Bono\_Daniel Lanois”, “Bono\_Gavin Friday\_Maurice Seezer”, etc. It’s a real mess. Additionally, it left the folder “U2” with a bunch of empty subdirectories! How rude!

I’ve made an effort, as of late, to try and clean-up this mess. In doing so, I’ve ended up with a lot of empty directories that previously contained music and/or subdirectories. After manually deleting a couple directories, I decided I needed to automate the process. I was surprised to find that this process is not nearly as straightforward as I thought it would.

In going about this process, I decided to do the following:

- Allow it to scan the directories first, and tell me which folders are empty (allows me to double-check!)
- Ignore all hidden files (this is because Windows Media Player and iTunes create hidden album art which I don’t want to preserve)
- Log empty directories, deleted directories and errors
- Scan first, delete second
- Ignore read-only attributes (all my music is marked as read-only to prevent someone from doing what I’m doing)
- Delete a directory if it is made empty because it’s subdirectories are deleted

Let me break down how I accomplished these things.

To scan for empty directories, I created a method that starts with a predefined directory, and recursively iterates through all the subdirectories. During this iteration, I scan the folder to see if it contains any files using the GetFiles method. Rather than specify files with a specific file extension (like .mp3 or .wma) I decided to get all files by using the “*.*” pattern (directory happens to be DirectoryInfo object).

```
<span style="FONT-SIZE: 11px; COLOR: black; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent"><span style="FONT-SIZE: 11px; COLOR: blue; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">using</span> System.IO;

...

FileInfo[] files;
files <span style="FONT-SIZE: 11px; COLOR: red; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">=</span> directory.GetFiles(<span style="FONT-SIZE: 11px; COLOR: #666666; FONT-FAMILY: Courier New; BACKGROUND-COLOR: #e4e4e4">"*.*"</span>);</span>
```

However, many of the files are hidden (because WMP and iTunes creates album art that’s hidden), so I decided to exclude hidden files. I did this by checking the hidden attribute of the file, and if it was turned on I ignored the file. If non-hidden files were found, I marked a local boolean value as true, indicating the folder contains files.

```
<span style="FONT-SIZE: 11px; COLOR: black; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent"><span style="FONT-SIZE: 11px; COLOR: blue; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">foreach</span> (FileInfo file <span style="FONT-SIZE: 11px; COLOR: blue; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">in</span> files)
{
     <span style="FONT-SIZE: 11px; COLOR: green; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">// only look for files that are not marked as hidden</span>
     <span style="FONT-SIZE: 11px; COLOR: blue; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">if</span> (!((file.Attributes & FileAttributes.Hidden) == FileAttributes.Hidden))
     {
         notEmpty <span style="FONT-SIZE: 11px; COLOR: red; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">=</span> <span style="FONT-SIZE: 11px; COLOR: blue; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">true</span>;
     }
}</span>
```

Next, I want to check to see if the directory contains subdirectories, and if those subdirectories are empty. To do this I get all the subdirectories and call the same method that is currently executing. This provides a level of recursion that allows me to iteratively go through every subdirectory in a directory. Since the existence of a subdirectory means that the directory is not empty, I automatically mark the current directory as not empty.

```
<span style="FONT-SIZE: 11px; COLOR: black; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent"><span style="FONT-SIZE: 11px; COLOR: green; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">// look through directories recursively</span>
DirectoryInfo[] dirs <span style="FONT-SIZE: 11px; COLOR: red; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">=</span> directory.GetDirectories(<span style="FONT-SIZE: 11px; COLOR: #666666; FONT-FAMILY: Courier New; BACKGROUND-COLOR: #e4e4e4">"*.*"</span>);
<span style="FONT-SIZE: 11px; COLOR: blue; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">foreach</span> (DirectoryInfo dir <span style="FONT-SIZE: 11px; COLOR: blue; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">in</span> dirs)
{
     scanForEmptyDirectories(dir);
     notEmpty <span style="FONT-SIZE: 11px; COLOR: red; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">=</span> <span style="FONT-SIZE: 11px; COLOR: blue; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">true</span>;
}</span>
```

Next, I add the directory path to a generic string collection so that I can later iterate through and delete the directories. The code here is very simple:

```
<span style="FONT-SIZE: 11px; COLOR: black; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent"><span style="FONT-SIZE: 11px; COLOR: blue; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">private</span> <span style="FONT-SIZE: 11px; COLOR: blue; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">static</span> List<<span style="FONT-SIZE: 11px; COLOR: blue; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">string</span>> directoriesToDelete;
directoriesToDelete <span style="FONT-SIZE: 11px; COLOR: red; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">=</span> <span style="FONT-SIZE: 11px; COLOR: blue; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">new</span> List<<span style="FONT-SIZE: 11px; COLOR: blue; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">string</span>>();

...

<span style="FONT-SIZE: 11px; COLOR: blue; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">if</span> (!notEmpty)
{
    <span style="FONT-SIZE: 11px; COLOR: green; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">// add folder to collection</span>
    directoriesToDelete.Add(directory.FullName);
}</span>
```

This accomplished, I needed to write a method that logs empty directories, the deletion of directories, and errors. I created a method that uses the StreamWriter and appends text to a file. Very simple.

```
<span style="FONT-SIZE: 11px; COLOR: black; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent"><span style="FONT-SIZE: 11px; COLOR: blue; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">public</span> <span style="FONT-SIZE: 11px; COLOR: blue; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">static</span> <span style="FONT-SIZE: 11px; COLOR: blue; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">void</span> logMessage(<span style="FONT-SIZE: 11px; COLOR: blue; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">string</span> logMessage)
{
    <span style="FONT-SIZE: 11px; COLOR: blue; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">using</span> (StreamWriter writer <span style="FONT-SIZE: 11px; COLOR: red; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">=</span> File.AppendText(logFile))
    {
        writer.WriteLine(logMessage);
        writer.Flush();
    }
}</span>
```

This method is called throughout the application, and writes any pertinent information to a log file (as you’ll see below).

Now, to delete the directories, I iterate through my generic string collection. Since some of my directories are marked as read-only, I elected to change the attributes of all directories marked for deletion to normal. This removes the read-only flag, if it exists. Next, I performed a similar operation on the files left in the directory (e.g. hidden files) and removed the read-only flag. Once these two steps are complete, I can then delete the folder using the Delete method on the Directory object.

```
<span style="FONT-SIZE: 11px; COLOR: black; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent"><span style="FONT-SIZE: 11px; COLOR: green; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">// iterate through the empty directories</span>
<span style="FONT-SIZE: 11px; COLOR: blue; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">foreach</span> (<span style="FONT-SIZE: 11px; COLOR: blue; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">string</span> directory <span style="FONT-SIZE: 11px; COLOR: blue; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">in</span> directoriesToDelete)
{
    <span style="FONT-SIZE: 11px; COLOR: blue; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">try</span>
    {
        DirectoryInfo dir <span style="FONT-SIZE: 11px; COLOR: red; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">=</span> <span style="FONT-SIZE: 11px; COLOR: blue; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">new</span> DirectoryInfo(directory);
        <span style="FONT-SIZE: 11px; COLOR: green; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">// set the attributes to normal; you cannot delete a readonly folder</span>
        dir.Attributes <span style="FONT-SIZE: 11px; COLOR: red; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">=</span> FileAttributes.Normal;

        <span style="FONT-SIZE: 11px; COLOR: blue; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">string</span>[] files <span style="FONT-SIZE: 11px; COLOR: red; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">=</span> Directory.GetFiles(directory);
        <span style="FONT-SIZE: 11px; COLOR: green; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">// iterate through the files</span>
        <span style="FONT-SIZE: 11px; COLOR: blue; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">foreach</span> (<span style="FONT-SIZE: 11px; COLOR: blue; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">string</span> file <span style="FONT-SIZE: 11px; COLOR: blue; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">in</span> files)
        {
            FileAttributes attributes <span style="FONT-SIZE: 11px; COLOR: red; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">=</span> File.GetAttributes(file);
            <span style="FONT-SIZE: 11px; COLOR: blue; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">if</span> ((attributes & FileAttributes.ReadOnly) !<span style="FONT-SIZE: 11px; COLOR: red; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">=</span> 0)
            {
                <span style="FONT-SIZE: 11px; COLOR: green; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">// remove the readonly attribute from the files</span>
                File.SetAttributes(file, ~FileAttributes.ReadOnly);
            }
        }

        <span style="FONT-SIZE: 11px; COLOR: green; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">// make sure to make the delete as recursive</span>
        Directory.Delete(directory, <span style="FONT-SIZE: 11px; COLOR: blue; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">true</span>);
        <span style="FONT-SIZE: 11px; COLOR: green; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">// log the deletion</span>
        logMessage(<span style="FONT-SIZE: 11px; COLOR: blue; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">string</span>.Format(<span style="FONT-SIZE: 11px; COLOR: #666666; FONT-FAMILY: Courier New; BACKGROUND-COLOR: #e4e4e4">"Deleted: {0} "</span>, directory));
    }
    <span style="FONT-SIZE: 11px; COLOR: blue; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">catch</span> (Exception e)
    {
        <span style="FONT-SIZE: 11px; COLOR: green; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">// log the error</span>
        logMessage(<span style="FONT-SIZE: 11px; COLOR: blue; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">string</span>.Format(<span style="FONT-SIZE: 11px; COLOR: #666666; FONT-FAMILY: Courier New; BACKGROUND-COLOR: #e4e4e4">"Error deleting: {0} "</span>, directory));
        logMessage(<span style="FONT-SIZE: 11px; COLOR: blue; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">string</span>.Format(<span style="FONT-SIZE: 11px; COLOR: #666666; FONT-FAMILY: Courier New; BACKGROUND-COLOR: #e4e4e4">"Error message: {0} "</span>, e.Message));
        logMessage(<span style="FONT-SIZE: 11px; COLOR: #666666; FONT-FAMILY: Courier New; BACKGROUND-COLOR: #e4e4e4">"Exiting application"</span>);
        <span style="FONT-SIZE: 11px; COLOR: green; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">// exit the console application</span>
        Environment.Exit(0);
    }
}</span>
```

Notice that I catch errors, log them, and then exit the application. Also, the try/catch is embedded in the directory loop, so that the directory path can be included in the log (if it were the other way around, the directory path would be out of scope and unavailable).

Finally, given the above code, it is possible to make a parent directory empty by deleting its child directories. To resolve this, I wrapped the execution of my code in a while statement that only exists when a given condition returns false. This condition value returns false only if there hasn’t been a single directory marked as empty. If any directory is marked as empty, the application performs an additional scan following the deletion of the directories it found empty. (Note: the runScan boolean value is a class-level variable that is set to true elsewhere in the code.)

```
<span style="FONT-SIZE: 11px; COLOR: black; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent"><span style="FONT-SIZE: 11px; COLOR: blue; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">while</span> (runScan)
{
     <span style="FONT-SIZE: 11px; COLOR: green; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">// assume that we won't have to run again</span>
     runScan <span style="FONT-SIZE: 11px; COLOR: red; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">=</span> <span style="FONT-SIZE: 11px; COLOR: blue; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">false</span>;

     <span style="FONT-SIZE: 11px; COLOR: green; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">// if the value returns true, then run it again</span>
     <span style="FONT-SIZE: 11px; COLOR: green; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">// it would return true </span>
     scanForEmptyDirectories(<span style="FONT-SIZE: 11px; COLOR: blue; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">new</span> DirectoryInfo(startingDirectory));
}</span>
```

All in all, this was a very quick and efficient way to eliminate my empty music folders. Your needs may be slightly different, so make changes accordingly.

I’ve gone ahead and included a zip file with the full source code for this console appliation. Let me know if you have thoughts and/or suggestions!

[EmptyFolders.zip (20.81 KB)](https://wadewegner.blob.core.windows.net/wordpress/content/binary/EmptyFolders.zip)
