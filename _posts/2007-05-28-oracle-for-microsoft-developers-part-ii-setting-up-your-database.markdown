---
author: admin
comments: true
date: 2007-05-28 13:35:19+00:00
layout: post
slug: oracle-for-microsoft-developers-part-ii-setting-up-your-database
title: 'Oracle for Microsoft Developers, Part II: Setting Up Your Database'
wordpress_id: 82
categories:
- Oracle
---

Yesterday, I detailed how to setup an Oracle database server on Windows Server 2003 in [Oracle for Microsoft Developers, Part I: Installing Oracle 9.2 on Windows Server 2003](http://www.wadewegner.com/PermaLink,guid,c9c89f05-a274-4033-9e3f-32113832bfa7.aspx). Today, I want to step back a little, take a look at some of the conceptual differences between Oracle and SQL Server, and then provide steps you can take to setup an Oracle database that you can use within your applications.

You may be saying to yourself, "Why should I care how to setup an Oracle database server and database? I work with SQL Server, .NET, and could care less! Let the DBAs concern themselves with Oracle!" Now, I can't discount your sentiments - I've been there myself! However, over the years, I've discovered two compelling reasons to take the time to learn more about Oracle: 1) it's always good to have more tools in your tool belt, and 2) getting DBAs to respond to your requests can occasionally be a long and arduous process. If you have the know-how and acumen to setup an Oracle environment for yourself, I believe you'll find your ability to deliver on your development projects will be greatly enhanced.

Okay, that said, let's take a look at some important Oracle concepts.

### Oracle Concepts

**Tablespace**

The Oracle tablespace is the lowest logical layer of the Oracle data structure. The tablespace consists of one or more datafiles, which are typically files on the operating system filesystem. The space specified for a tablespace is allocated on the filesystem, but is not used until somebody creates a file or saves some data.

Think of the Tablespace as functionally equivalent to the data file (MDF) and log file (LDF) in SQL Server

**Schema**

A schema is a collection of objects associated with the database. These objects are abstractions or logical structures that refer to database objects or structures. Schema objects consist of such things as clusters, indexes, packages, sequences, stored procedures, synonyms, tables, views, and so on.

I find it difficult to exactly correlate an Oracle schema to a concept in SQL Server. I suppose it would be safe to say that a schema is similar to an object within SQL Server (such as a table or view) that's attached to a particular owner (such as dbo).

**Data Dictionary**

The data dictionary is a set of tables Oracle uses to maintain information about the database. The data dictionary contains information about tables, indexes, clusters, and so on.

The data dictionary is similar to the system tables that exist within SQL Server.

**Database**

The physical layer of the database consists of three types of files: 

* One or more datafiles

* Two or more redo log files

* One or more control files

The logical layer of the database consists of the following elements: 

* One or more tablespaces.

* The database schema, which consists of items such as tables, clusters, indexes, views, stored procedures, database triggers, sequences, and so on.

The database is divided into one or more logical pieces known as tablespaces. A tablespace is used to logically group data together. Tablespaces consist of one or more datafiles. By using more than one datafile per tablespace, you can spread data over many different disks to distribute the I/O load and improve performance.

As part of the process of creating the database, Oracle automatically creates the SYSTEM tablespace for you. Although a small database can fit within the SYSTEM tablespace, it's recommended that you create a separate tablespace for user data. The SYSTEM tablespace is where the data dictionary is kept.

**Administrative accounts**

* INTERNAL - The INTERNAL account is provided mainly for backward compatibility with earlier versions of Oracle, but is still used for key functions such as starting up and shutting down the instance.

* SYS - Automatically created whenever a database is created. Used primarily to administer the data dictionary. This account is granted the DBA role, as well as CONNECT and RESOURCE roles.

* SYSTEM - Automatically created whenever a database is created. This account is used primarily to create tables and views important to the operation of the RDBMS

**Administrative roles**

* DBA

* OSOPER - Assigned to special accounts that need OS authentication, which can be done only when the database is open; allows users to run commands like STARTUP and SHUTDOWN, and ALTER DATABASE

* OSDBA - OSOPER plus additional permissions such as CREATE DATABASE or ADMIN OPTION

**Oracle instances**

An Oracle instances is identified by a SID (system identifier), and is functionality equivalent to a SQL Server database.

(I must credit the following Web sites for much of the previous content, as I certainly did not know much of this until I visited: [http://vsbabu.org/](http://vsbabu.org/), [http://www.fredshack.com/](http://www.fredshack.com/), and [http://www.rocket99.com/](http://www.rocket99.com/).)

### Setting Up Your Oracle Database

Now that we've provided a high-level discussion of Oracle, and some of the differences from SQL Server (and yes, I realize that this is not a comprehensive analysis or comparison - you can find plenty of those elsewhere), let's look at some of the steps you need to take in order to work with Oracle.

1. Install the Oracle database server. See [Oracle for Microsoft Developers, Part I: Installing Oracle 9.2 on Windows Server 2003](http://www.wadewegner.com/PermaLink,guid,c9c89f05-a274-4033-9e3f-32113832bfa7.aspx) for detailed steps on installing Oracle 9.2.

2. Use SQL*Plus and log into the Oracle database server. I prefer to login as "system" when defining tablespaces, users, etc.

3. Create a new tablespace. The following script creates a tablespace called "ts_name" at the root of the drive that Oracle is installed on (e.g. C:), has logging turned on, a default size of 32 MBs that automatically extends to 2 GBs.

    create tablespace ts_name
    logging
    datafile '/ts_name.dbf'
    size 32m
    autoextend on
    next 32m maxsize 2048m
    extent management local;

4. Create a new user (or user schema) that has access to this tablespace. The following script creates a user named "alfred" with a password of "pword".

    create user alfred identified by pword;

  You can also define the default and temporary tablespace for this uses, as well as the quota. Note: you can specify a numeric value for the quota (such as 10) or unlimited, which defines an unlimited quota.

    create user alfred identified by pword<br></br>  default tablespace ts_name temporary tablespace ts_name quota unlimited on ts_name; 

  Failure to define a quote for your user within the tablespace will prevent the user from actually writing data into the database. Make sure you don't miss this step!

5. Grant the user privileges, such as creating a session, table, views, etc.

    grant create session to alfred;
    grant create table to alfred;
    grant create view to alfred;
    grant create trigger to alfred;
    grant create procedure to alfred;
    grant create sequence to alfred;
    grant create synonym to alfred;

6. Close SQL*Plus, cause you're done!

At this point you should be set! You've created a tablespace, a user, and given the user the privileges necessary to create tables, procedures, and more!

Realize again that I am not an Oracle DBA, and I would never think about configuring a production environment for Oracle. The purpose of this blog is to familiarize Microsoft developers with Oracle, as it's very likely you'll have to work with it sooner or later. The ability to setup and define your development environment should not be underrated and marginalized, as it will not only help you with your delivery, but also make you more marketable!

Best of luck!