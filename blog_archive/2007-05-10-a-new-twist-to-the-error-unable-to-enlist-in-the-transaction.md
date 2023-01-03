---
author: admin
categories:
- BizTalk Server
- Windows Server 2003
comments: true
date: "2007-05-10T19:10:41Z"
slug: a-new-twist-to-the-error-unable-to-enlist-in-the-transaction
title: 'A new twist to the error: "Unable to enlist in the transaction."'
wordpress_id: 88
---

I learned a new twist to the following error message today:

	The adapter "SQL" raised an error message. Details "Unable to enlist in the transaction. (Exception from HRESULT: 0x8004D00A)".

This can occur in a scenario where your BizTalk server attempts to connect to a separate SQL Server database and execute a distributed transaction. In order for this to work the BizTalk server must have the ability to "hand-off" the transaction to SQL Server. Typically, when you receive this error, it's because the Distributed Transaction Coordinator (DTC) service is disabled or network DTC access is disabled. These are the default settings in Windows Server 2003. Take a look at the following article:

[http://support.microsoft.com/?kbid=816701](http://support.microsoft.com/?kbid=816701)

There's an additional twist to this scenario. I found myself in a situation where I was receiving this error but had DTC setup correctly on all the machines in my BizTalk group and SQL Server cluster. Nevertheless, the distributed transactions failed. Then I found the following article:

[http://msdn2.microsoft.com/en-us/library/aa561924.aspx](http://msdn2.microsoft.com/en-us/library/aa561924.aspx)

Turns out that, in order for DTC to work, the SQL Server must be able to resolve the NetBios name of the client (the BizTalk servers, in this case). If it cannot resolve the NetBios name the transaction will fail. In my environment, a firewall prevented the ability to resolve the NetBios name to an IP address thereby preventing the distributed transaction from processing.

To resolve this, I updated the HOST files on the SQL Server cluster so that they were able to resolve the NetBios names to an IP address. Literally, moments after saving the HOST file, all my records started getting written into the database.

As I said, a different twist to a common problem.

I hope this helps!