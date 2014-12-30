---
layout: post
title: "4 Simple Steps to Run Go Language in Azure Websites"
description: "A few weeks ago I wrote a Go web app and wanted to find a good place to host it. Turns out, Azure Websites is a great fit! Follow these four simple steps to try it out."
categories: 
- golang
- azure
- azure websites
---

A few weeks ago I wrote a Go Language web app and wanted to find a good place to host it. Given I work with the team that builds Azure Websites, it seemed only fitting to host it in Azure Websites. Turns out, it works great! However, there aren't any resources available that walk you through the steps. Consequently, I wrote the following tutorial.

(Incidentally, if you want to start to use Go Langauge and run Windows you'll want to review my post [Easy Go Programming Setup for Windows](http://www.wadewegner.com/2014/12/easy-go-programming-setup-for-windows/).)

Is this the absolutely authority on running Go Language in Azure Websites? No, but it works. I plan to publish a few more articles on more complex scenarios in the future.

### 4 Simple Steps ###

1. Create your Azure Website in the [Azure Portal](https://portal.azure.com). Without a doubt the easiest step.

2. Download and unzip the Go executables. In the Azure Portal select your website blade and scroll down until you see the `Console` tile.

	![Open Console on the Azure websites blade](https://cloud.githubusercontent.com/assets/746259/5574198/146ffd92-8f76-11e4-9975-f9e4cd51b9fe.png)

	In the console to the right, type the following commands:

	`curl -O https://storage.googleapis.com/golang/go1.4.windows-386.zip`

	`unzip go1.4.windows-386.zip`

	![unzipped](https://cloud.githubusercontent.com/assets/746259/5574286/0ac47f78-8f78-11e4-91c3-1a5f4a6540ae.png)

	Be patient with the console as these two operations may take a few moments. It's purposefully simple.

	Note: I'm not sure why it returns `Bad Request` but so far it doesn't seem to cause any issues.

3. Create a `server.go` file in the `wwwroot` folder. This is a super simple Go file which will reply for any URL on the website.

		package main

		import (
		    "fmt"
		    "net/http"
		    "os" 
		)

		func handler(w http.ResponseWriter, r *http.Request) {
		    fmt.Fprintf(w, "You just browsed page (if blank you're at the root): %s", r.URL.Path[1:])
		}

		func main() {
		    http.HandleFunc("/", handler)
		    http.ListenAndServe(":"+os.Getenv("HTTP_PLATFORM_PORT"), nil)
		}

	As a convenience, feel free to use `curl` and download this directly from [my public gist](https://gist.github.com/wadewegner/52a925a7b1607a48d796). Otherwise, you should consider using Kudu or git to get your files into your Azure Website.

	`curl -L http://bit.ly/1xtYNRU --output server.go`

	Once downloaded, type `type server.go` to confirm you grabbed everything correctly.

4. Create a `Web.Config` file in the `wwwroot` folder. We will use the `httpPlatformHandler` simpilar to [running Tomcat in Azure Websites](http://azure.microsoft.com/en-us/documentation/articles/web-sites-java-custom-upload/).

		<?xml version="1.0" encoding="UTF-8"?>
		<configuration>
		    <system.webServer>
		        <handlers>
		            <add name="httpplatformhandler" path="*" verb="*" modules="httpPlatformHandler" resourceType="Unspecified" />
		        </handlers>
		        <httpPlatform processPath="d:\home\site\wwwroot\go\bin\go.exe" 
		                      arguments="run d:\home\site\wwwroot\server.go" 
		                      startupTimeLimit="60">
		            <environmentVariables>
		              <environmentVariable name="GOROOT" value="d:\home\site\wwwroot\go" />
		            </environmentVariables>
		        </httpPlatform>
		    </system.webServer>
		</configuration>

	Again, you can use the following curl command to download the `Web.Config` file:

	`curl -L http://bit.ly/1wx4JFY --output Web.Config`

That is it!

For now, try it out here: [http://golang.azurewebsites.net/hello](http://golang.azurewebsites.net/hello) 

Is there more we can do here? Absolutely, and I intend to cover some additional topics in future posts. However, for now, it feels good knowing that we can push our Go Language code to Azure Websites.

Special thanks to [Rajith Mukkai Ramachandra](https://twitter.com/ranjithtweeets) for his help and answering all my questions quickly and thoroughly!