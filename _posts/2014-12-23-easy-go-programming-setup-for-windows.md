---
layout: post
title: "Easy Go Programming Setup for Windows"
description: "I've spent a fair amount of time recently building Go apps. Every time I've set it up on Windows I've had to re-learn the steps. This post is a short tutorial for setting up Go on Windows."
categories: 
- golang
- setup
---

I've had to do this more than once recently, so I figured I'd document the simple steps for setting up the Go programming language on Windows. Most of this is simple and straightforward. The only tricky part I found is setting up your GOPATH, which defines a convention for storing and building Go code you write and acquire from open source code repositories.

### 5 Simple Steps ###

Follow these five simple steps to install Go.

1. Make sure you have both Git [[download](http://git-scm.com/download/win)] and Mercurial [[download](http://mercurial.selenic.com/wiki/Download)] installed. With Go programming you'll make heavy use of open source repositories.

2. **Download** and **install** the latest 64-bit Go MSI distributable (which sets most of the environmental variables for you). [https://golang.org/dl/](https://golang.org/dl/)

	![go1-4-windows-amd64-msi](https://cloud.githubusercontent.com/assets/746259/5536386/184c0a8c-8a4e-11e4-828c-dd3320fbdd41.png)

	To make things simple, use the default installation path at `C:\Go`

3. Ensure the **Go binaries** (found in `C:\Go\bin`) are in your `Path` system environment variables. To check click `System`, `Advanced system settings`, `Environment Variables...` and open `Path` under `System variables`:

	![gopath](https://cloud.githubusercontent.com/assets/746259/5536474/965ff18a-8a4f-11e4-853f-aede7735a6fd.png)

	An easy way to confirm is to open the command line and type `go version`:

	![gopathcmd](https://cloud.githubusercontent.com/assets/746259/5536483/e51af5c2-8a4f-11e4-8b7f-dddcf0a32548.png)

4. Setup your **Go workspace**. This consists of three folders at the root:

	    bin/
		pkg/
		src/

	I create a `C:\Projects\Go` folder as my root Go workspace: 

	![goworkspace](https://cloud.githubusercontent.com/assets/746259/5536646/909c8c06-8a52-11e4-8cb0-1ba9b5077f8a.png)

5. Create the **GOPATH environment variable** and reference your Go workspace path. To add, click `System`, `Advanced system settings`, `Environment Variables...` and click `New...` under `System variables`:

	![gopathenvvar](https://cloud.githubusercontent.com/assets/746259/5536717/6ef223da-8a53-11e4-96bf-3cbbd9acf589.png)

	Set the variable name to `GOPATH` and value to your Go workspace path (e.g. `C:\Projects\Go`):

	![gopathvalue](https://cloud.githubusercontent.com/assets/746259/5536757/feb28c94-8a53-11e4-9bf4-02728abe34e5.png)

	You can quickly check to ensure your path has been set by opening the command line and typing `echo %GOPATH%` and check the output:

	![gopathecho](https://cloud.githubusercontent.com/assets/746259/5536824/39de5004-8a55-11e4-8140-ee39858cc1f4.png)

And that's all it takes! You're ready to get started.

### Verify ###

Want to quickly test and ensure this is all working as expected? Open the command line and type the following:

{% highlight bat %}

go get github.com/golang/example/hello
%GOPATH%/bin/hello

{% endhighlight %}

You should see the output as "Hello, Go examples!" (refreshingly, not your typical hello world):

![gohelloworld](https://cloud.githubusercontent.com/assets/746259/5536857/ad39c088-8a55-11e4-9080-c87d62ca55c8.png)

I hope this helps!