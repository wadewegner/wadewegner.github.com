---
title: "A Command-line Interface for Force.com"
date: 2013-11-18T00:00:00-08:00
draft: false
description: "From the whiteboard to the keyboard"
---

Released this week at Dreamforce is a new command-line interface (CLI) tool for Force.com. This tool is open-sourced and available at <http://github.com/heroku/force>. This tool lets you quickly interact with the Force.com APIs and opens up interesting opportunities for scripting and automating tasks on Force.com.

![Apex code example](http://wadewegner.blob.core.windows.net/wordpress/2013-11-18-ApexCode.png)

You can immediately download the precompiled binaries for the force command-line interface and start using it today:

- Linux: [32-bit](https://godist.herokuapp.com/projects/heroku/force/releases/current/linux-386/force) | [64-bit](https://godist.herokuapp.com/projects/heroku/force/releases/current/linux-amd64/force)
- OS X: [32-bit](https://godist.herokuapp.com/projects/heroku/force/releases/current/darwin-386/force) | [64-bit](https://godist.herokuapp.com/projects/heroku/force/releases/current/darwin-amd64/force)
- Windows: [32-bit](https://godist.herokuapp.com/projects/heroku/force/releases/current/windows-386/force.exe) | [64-bit](https://godist.herokuapp.com/projects/heroku/force/releases/current/windows-amd64/force.exe)

Simply download the binary and run it from the command-line.

```
Usage: force <command> [<args>]

Available commands:
   login     Log in to force.com
   logout    Log out from force.com
   whoami    Show information about the active account
   sobject   Manage sobjects
   field     Manage sobject fields
   record    Create, modify, or view records
   export    Export metadata to a local directory
   import    Import metadata from a local directory
   query     Execute a SOQL query
   apex      Execute anonymous Apex code
   version   Display current version
   update    Update to the latest version
   help      Show this help

Run 'force help [command]' for details.
```

Let’s take a look at a few simple examples.

To start, you need to login so that you are able to execute commands against Force.com.

```
> force login
```

When you initiate the login experience the tool will open up your browser so that you can enter your username and password.

![Login](http://wadewegner.blob.core.windows.net/wordpress/2013-11-17-Login.png)

Using some internal trickery (you can take a look [here](https://github.com/heroku/force/blob/master/force.go#L374) to get in on the secret) the browser is able to return the OAuth token to the command-line. Now the command-line is able to make authenticated calls to force.com.

Once logged in, you’re ready to go.

```
> force whoami | grep Username
Username: wwegner@64demo.com
```

Just in case you forget.

Let’s create a new object.

```
> force sobject create MyNewObject Description:string
```

This creates a new object called **MyNewObject** with a string field called **Description**.

You can confirm it is created.

```
> force sobject list | grep MyNewObject
MyNewObject__c
```

Okay. So, in addition to these simple commands, you can also run apex code. You will likely not use this by hand all of the time, but this provides a great opportunity for scripting.

Let’s give it a try.

```
> force apex
MyNewObject__c newObject = new MyNewObject__c();
newObject.Description__c = 'description';
insert newObject;
```

Now, hit Control-D to submit the code for execution.

```
>> Executing code...

28.0 APEX_CODE,DEBUG;APEX_PROFILING,INFO
Execute Anonymous: MyNewObject__c newObject = new MyNewObject__c();
Execute Anonymous: newObject.Description__c = 'description';
Execute Anonymous: insert newObject;
08:34:11.084 (84957822)|EXECUTION_STARTED
08:34:11.084 (84970827)|CODE_UNIT_STARTED|[EXTERNAL]|execute_anonymous_apex
08:34:11.300 (223609040)|CUMULATIVE_LIMIT_USAGE
08:34:11.300|LIMIT_USAGE_FOR_NS|(default)|
  Number of SOQL queries: 0 out of 100
  Number of query rows: 0 out of 50000
  Number of SOSL queries: 0 out of 20
  Number of DML statements: 1 out of 150
  Number of DML rows: 1 out of 10000
  Number of code statements: 3 out of 200000
  Maximum CPU time: 0 out of 10000
  Maximum heap size: 0 out of 6000000
  Number of callouts: 0 out of 10
  Number of Email Invocations: 0 out of 10
  Number of fields describes: 0 out of 100
  Number of record type describes: 0 out of 100
  Number of child relationships describes: 0 out of 100
  Number of picklist describes: 0 out of 100
  Number of future calls: 0 out of 10

08:34:11.300|CUMULATIVE_LIMIT_USAGE_END

08:34:11.223 (223678184)|CODE_UNIT_FINISHED|execute_anonymous_apex
08:34:11.223 (223688365)|EXECUTION_FINISHED
```

Pretty fantastic. The possibilities are almost endless.

Now, if you really want to have some fun, you can fork the [fork the heroku/github repository](https://github.com/heroku/force/fork) and start to extend the command-line interface itself! Let me show you how to quickly get started.

The force command-line interface is built using the [Go programming language](http://golang.org/). Go is a fun C-derived language originally created at Google. There’s a good book called [An Introduction to Programming in Go](http://www.golang-book.com/) that provides a good overview of the language.

In order to build the source make sure you have the following setup.

- The [Go language runtime](http://golang.org/pkg/runtime/). Download and install this tool. Make sure you can execute it from the command-line (i.e. it’s in your path).
- [Git](http://git-scm.com/downloads) and [Mercurial](http://mercurial.selenic.com/downloads/). The force command-line Go package has dependencies on packages that are hosted on Github and Bitbucket.

Go uses packages for builds so that it’s easier to manage dependencies.

The first step is to create and set your GOPATH - this is where go will place packages you get later on. I have mine set simply:

```
> echo $GOPATH
/Users/wwegner/go
```

Ideally you should add this to your ~/.bash\_profile but you can also quickly set it by typing:

```
export GOPATH="/Users/wwegner/go"
```

Once you have set the GOPATH you can grab the Go package for the force command-line interface.

```
> go get -u github.com/heroku/force
```

When this finishes you’ll find you have three folders in your go folder.

![Go folder](http://wadewegner.blob.core.windows.net/wordpress/2013-11-18-GoFolder.png)

If you explore these folders you’ll find that they include all the dependences you need in order to successfully build and install the force command-line tool locally.

Fantastic.

Now, clone your forked heroku/force repository so that you have the bits pulled down locally. Browse to the folder and type the following:

```
> go build
```

Since you have your GOPATH setup correctly you will find that this will create a **force** binary in your local directory. You can easily run it.

```
> ./force login
```

And you’re all set! Now, you can start to update the \*.go files, build a new binary, and away you go.

Hopefully you’ve found this a useful introduction to the force command-line interface. I’m sure we’ll find more ways to benefit from this tool over time!
