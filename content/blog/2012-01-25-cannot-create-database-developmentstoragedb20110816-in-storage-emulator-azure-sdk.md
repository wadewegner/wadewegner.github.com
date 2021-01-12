---
author: admin
categories:
- Scripts
- Storage Emulator
- Windows Azure
- Windows Azure Storage
comments: true
date: "2012-01-25T18:37:14Z"
slug: cannot-create-database-developmentstoragedb20110816-in-storage-emulator-azure-sdk
title: Cannot create database 'DevelopmentStorageDb20110816' for the Windows Azure
  Storage Emulator
wordpress_id: 1770
---

Have you seen this error before? If you’ve spent any time with the Windows Azure storage emulator it’s highly probable. Here’s the full text:

    Added reservation for http://127.0.0.1:10000/ in user account COMPUTER\User.
    Added reservation for http://127.0.0.1:10001/ in user account COMPUTER\User.
    Added reservation for http://127.0.0.1:10002/ in user account COMPUTER\User.

    Creating database DevelopmentStorageDb20110816...
    Cannot create database 'DevelopmentStorageDb20110816' : CREATE DATABASE permission
    denied in database 'master'.

    One or more initialization actions have failed. Resolve these errors before attempting
    to run the storage emulator again. These errors can occur if SQL Server was installed
    by someone other than the current user. Please refer to
    http://go.microsoft.com/fwlink/?LinkID=205140 for more details.

And an image of the error:

![Development Storage initialization error](https://wadewegner.blob.core.windows.net/wordpress/2012/01/SQLError.png)

This error can occur when running the storage emulator (or running DSINIT.exe) for the first time. The compute emulator needs to initialize itself, which includes creating a local SQL Server database that is used to store data for local Windows Azure storage. The above error indicates that there’s a permissions when trying to create the database.

There are a number of ways to resolve this issue and, like others, I have my favorite approach. I have a script that I run which will add the executing user to the SQL Server sysadmin role.

I've published the entire script here: [https://gist.github.com/1677788](https://gist.github.com/1677788). Simply download and unzip the file. Open up an elevated command prompt and execute the file (i.e. run **addselftosqlsysadmin.cmd**). Once the script is executed the user can successfully initialize the storage emulator.

I hope this helps!