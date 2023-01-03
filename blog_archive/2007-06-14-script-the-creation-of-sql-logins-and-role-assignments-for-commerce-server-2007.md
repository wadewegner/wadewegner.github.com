---
author: admin
categories:
- Commerce Server
- Scripts
- SQL Server
comments: true
date: "2007-06-14T16:46:59Z"
slug: script-the-creation-of-sql-logins-and-role-assignments-for-commerce-server-2007
title: Script the creation of SQL logins and role assignments for Commerce Server
  2007
wordpress_id: 73
---

One of the more manually intense steps in the installation of Commerce Server is setting up the appropriate SQL logins and Database Role User Mapping. This task can easily take 30 - 60 minutes to complete if done manually. Futhermore, it's likely that this is the easiest step to make a mistake, which will cause problems for you down the road.

So, to make this process quicker and less prone to errors, I've created an SQL script that you can run against your database. This script performs two tasks:

1. Creates the SQL login accounts (e.g. COMPUTERNAMECatalogWebSvc).

2. Associates the SQL login accounts to database roles.

Note: this script is only for SQL Server 2005. SQL Server 2000 uses a different set of database roles.

Take a look the _Grant Web Applications and Window Services Access to the Databases_ section of the [Installation Guide](http://go.microsoft.com/fwlink/?LinkID=57268) for Commerce Server 2007, and you'll see that the number of role assignments is quite extensive.

Rather than pasting the entire SQL script into this post, I am only going to upload the .SQL file. You can modify this file as necessary in order to adapt it to your environment (e.g. changing the computer name, you may have different names for logins, or don't need services like the direct mailer).

[CreateCSLoginsAndAssignRoles.sql.txt (12.54 KB)](https://wadewegner.blob.core.windows.net/wordpress/content/binary/CreateCSLoginsAndAssignRoles.sql.txt) (just remove the .txt extension)

I hope this helps!

**[Update]**

I found it useful to create an abbreviated verion of this script that is used for adding new sites. Whereas the script "CreateCSLoginsAndAssignRoles.sql.txt" is for brand new installations of Commerce Server 2007, the following script is useful for when you add a new site and re-use users and logins.

[CreateCSLoginsAndAssignRolesForNewSites.sql.txt (8.42 KB)](https://wadewegner.blob.core.windows.net/wordpress/content/binary/CreateCSLoginsAndAssignRolesForNewSites.sql.txt) (just remove the .txt extension)