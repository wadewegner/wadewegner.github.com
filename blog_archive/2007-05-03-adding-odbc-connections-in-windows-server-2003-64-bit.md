---
author: admin
categories:
- BizTalk Server
- Windows Server 2003
- x64
comments: true
date: "2007-05-03T15:51:45Z"
slug: adding-odbc-connections-in-windows-server-2003-64-bit
title: Adding ODBC connections in Windows Server 2003 64-bit
wordpress_id: 91
---

We've been deploying a BizTalk 2006 solution to a x64 system and, while BizTalk 2006 itself has no problems, I've had a lot of problems related to the BizTalk Adapters for Host Systems and the BizTalk Adapters for Enterprise Applications. I plan to write a number of posts in the near future discussing these problems (once we have them all resolved). In the meantime, I wanted to share this little tidbit I've learned about ODBC on 64-bit windows.

It's important for you to know if your application is going to run as a 32-bit or 64-bit application. There are two different repositories for ODBC connections, based on the drivers you've installed and the client that will run them. 32-bit applications will only see ODBC connections from the 32-bit side, and 64-bit applications will only see ODBC connections from the 64-bit side.

32-bit applications register ODBC connections to the following registry key:

	HKEY_LOCAL_MACHINESOFTWAREWow6432NodeODBCODBC.INI

64-bit applications register ODBC connections to the following registry key:

	HKEY_LOCAL_MACHINESOFTWAREODBCODBC.INI

Adding these keys is initially tricky, but if you understand the above then it should make sense. Typically, you go to Start --> Administrative Tools --> Data Sources (ODBC) to create your ODBC connections. And this is fine if you want to create 64-bit ODBC connections. Any ODBC connection created using the Data Sources (ODBC) link will get created in the HKEY_LOCAL_MACHINESOFTWAREODBCODBC.INI key. This is because it calls the program %WINDIR%System32odbcad32.exe. However, this program will not create 32-bit ODBC connections.

To create 32-bit ODBC connections, you have to run %WINDIR%SysWOW64odbcad32.exe. Adding an ODBC connection through this application will create the ODBC connection in the HKEY_LOCAL_MACHINESOFTWAREWow6432NodeODBCODBC.INI key, and allow your 32-bit applications to see and use the ODBC connection.

I'm still getting used to the concept of WOW and how it runs 32-bit applications on an x64 system. This information may not be news to some of you, but it presented a challenge to me.

I hope this helps!