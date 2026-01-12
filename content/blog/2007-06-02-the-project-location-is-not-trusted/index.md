---
title: "The project location is not trusted …"
date: 2007-06-02T00:00:00-08:00
draft: false
description: "From the whiteboard to the keyboard"
---

Having recently purchased a new server, I found myself configuring and setting up the various virtual machines I use for software development. After I setup my development virtual machine, I moved some of my projects over to the D drive and double-clicked one of the project files. After the IDE opened up, but before the project loaded, I received the following dialog message:

![NotTrusted](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/Theprojectlocationisnottrusted_CE6F/NotTrusted_thumb.gif)

```
"The project location is not trusted. Running the application may result in security exceptions when it attempts to perform actions which require full trust."
```

This is something I’ve had to setup and configure many times, but since it’s not something I do every day I had to go look up the command to add my D drive as a group with full trust. To accomplish this, do the following:

1. Select Start -> All Programs -> Microsoft Visual Studio 2005 -> Visual Studio Tools -> Visual Studio 2005 Command Prompt. This opens up a command prompt with all the Visual Studio program folders in the PATH (type “path” to see).
2. Type the following command:

   ```
    caspol -q -machine -addgroup 1 -url file://d:/* FullTrust -name "D Drive"
   ```

That’s it. You have now added the D drive to your .NET runtime security policy with the Full Trust permission set. You will not receive any security exceptions when running your .NET applications.

If you open up the .NET Framework 2.0 Configuration MMC, and browse to My Computer -> Runtime Security Policy -> Machine -> Code Groups -> All\_Code, you will see the “D Drive” code group.

![](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/Theprojectlocationisnottrusted_CE6F/Config.gif)

I hope this helps!
