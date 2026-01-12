---
title: "The BizTalk Adapters for Enterprise Applications on Windows Server 2003 64-bit"
date: 2007-05-04T00:00:00-08:00
draft: false
description: "From the whiteboard to the keyboard"
---

As I mentioned in a previous blog, I’ve been setting up BizTalk 2006 in a 64-bit Windows Server 2003 environment. This BizTalk solution communicates to AS/400 and Oracle, so I’ve been using the Microsoft BizTalk Adapters for Enterprise Applications (for Oracle connectivity) as well as Microsoft BizTalk Adapters for Host Systems (for AS/400 DB2 connectivity).

Setting up these adapters has not gone extremely well in the 64-bit O/S (whereas a 32-bit O/S is quite simple to configure). I’ve been set back about a week because many different issues related to the 64-bit environment (and a few other issues). In this post, I want to explain the issues I’ve encountered with regards to the Oracle adapter in a Windows Server 2003 64-bit environment.

This post assume that you’ve already installed your Oracle client. Check out my post [Using the “ODBC Adapter for Oracle Database” in BizTalk 2006](http://www.wadewegner.com/PermaLink,guid,c99bc4ea-984d-418c-aff7-9462f994f0a9.aspx) for information on how to install the client. However, if you’re installing on a 64-bit machine, stop after you install the client. The 64-bit world is quite different …

1. Install the .NET Framework 1.1 and SP 1. The Oracle adapter requires the .NET Framework 1.1 and SP 1. My 64-bit environment didn’t have the .NET Framework 1.1 installed, so this was an additional step I had to take. This was the easiest of all the steps I had to take …
2. Create the ODBC connection. Turns out that you cannot simply run Data Sources (ODBC) from Administrative Tools. You have to run %WINDIR%\SysWOW64\odbc\ad32.exe, which invokes the 32-bit version of the Data Sources (ODBC) GUI. See my post [Adding ODBC connections in Windows Server 2003 64-bit](http://wadewegner.com/2007/05/adding-odbc-connections-in-windows-server-2003-64-bit/) for more information.
3. Install the Microsoft BizTalk Adapters for Enterprise Applications. I only installed Oracle (r) Database. Surprisingly, with the .NET Framework 1.1 and SP 1 installed, this goes very well.
4. Update registry settings for the Microsoft BizTalk Adapters for Enterprise Applications. Turns out that some of the registry keys that are written during the installation are wrong (or rather, they’re not interpreted correctly). Browse to the following registry key: HKEY\_LOCAL\_MACHINESOFTWAREWow6432NodeMicrosoftBizTalkAdapters. Two of the values, InstallDir and InstallPath, need to be changed. Do the following:

   1. Change InstallDir from “C:Program Files (x86)Microsoft BizTalk Adapters for Enterprise Applications” to “C:Progra~2Microsoft BizTalk Adapters for Enterprise Applications”.
   2. Change InstallPath from “C:Program Files (x86)Common FilesMicrosoft BizTalk Adapters for Enterprise Applications” to “C:Progra~2Common FilesMicrosoft BizTalk Adapters for Enterprise Applications”.

   Yes, for some reason, you have to change “Program Files (x86)” to “Progra~2”, probably because the Adapter isn’t written very well. If you don’t update these registry settings, you will most likely get the following error:

   ```
    The description for Event ID ( 0 ) in Source ( Microsoft BizTalk Adapters for Enterprise Applications ) cannot be found. The local computer may not have the necessary registry information or message DLL files to display messages from a remote computer. You may be able to use the /AUXSOURCE= flag to retrieve this description; see Help and Support for details. The following information is part of the event: Exception occurred:

    Error Code: 12154 (0x2f7a)
    08004 : [Oracle][ODBC][Ora]ORA-12154: TNS:could not resolve service name.
   ```
5. Update additional registry settings to fix permission errors if you are using domain groups for authentication.

Your domain groups will not have access to the Oracle adapter by default. In order to allow the runtimeagent.exe (which is the executable that is spawned and runs the Oracle adapter) to run appropriately, it needs to be able to access a registry key. See the following article for more information: <http://support.microsoft.com/?id=923650>.

Do the following to resolve this issue (from the article):

1. Locate the following registry key: HKEY\_LOCAL\_MACHINESoftwareWow6432NodeMicrosoftBizTalkAdaptersConfig

   Thanks to [Steef-Jan Wiggers](http://www.soa-thoughts.blogspot.com/) for noting that I forgot to display the 32-bit path in the Wow6432Node key.
2. Right-click the registry key that you located in step 1, and then click Permissions.
3. On the Security tab, click Add.
4. Type the domain group or the domain user account that is configured as the BizTalk host instance, and then click OK.
5. On the Security tab, click the domain group or the domain user account that you added in step 4, click to select the Read check box, and then click OK.

   If you don’t update these registry settings, you will most likely get the following (unhelpful) error:

   ```
    "RuntimeAgent: Error trapped in constructor: No connection could be made because the target machine actively refused it"

    Error transmitting message: No connection could be made because the target machine actively refused it
   ```
6. Update the security permissions on your Oracle folder.

This is the most bizarre step of all, and it’s not particular to the 64-bit environment. Oracle software requires that you give the Authenticated User privilege to the Oracle Home. In most cases, your Oracle agent will not be the Administrator account (or at least, I hope it’s not). However, there seems to be an issue with the permissions associated to Authenticated Users. Consequently, the agent you specify to run the Oracle “runtimeagent.exe” is unable to gain access to the folder. You might see the following error:

```
IM003 : Specified driver could not be loaded due to system error 5
```

To resolve this problem, you have to do the following:

1. Log on to Windows as a user with Administrator privileges.
2. Launch Windows Explorer from the Start Menu and and navigate to the ORACLE\_HOME folder. This is typically the “Ora92” folder under the “Oracle” folder (i.e. D:OracleOra92).
3. Right-click on the ORACLE\_HOME folder and choose the “Properties” option from the drop down list. A “Properties” window should appear.
4. Click on the “Security” tab of the “Properties” window.
5. Click on “Authenticated Users” item in the “Name” list (on Windows XP the “Name” list is called “Group or user names”).
6. Uncheck the “Read and Execute” box in the “Permissions” list under the “Allow” column (on Windows XP the “Permissions” list is called “Permissions for Authenticated Users”).
7. Re-check the “Read and Execute” box under the “Allow” column (this is the box you just unchecked).
8. Click the “Advanced” button and in the “Permission Entries” list make sure you see the “Authenticated Users” listed there with:

   ```
    Permission = Read & Execute

    Apply To = This folder, subfolders and files
   ```

   If this is NOT the case, edit that line and make sure the “Apply onto” drop-down box is set to “This folder, subfolders and files”. This should already be set properly but it is important that you verify this.
9. Click the “Ok” button until you close out all of the security properties windows. The cursor may present the hour glass for a few seconds as it applies the permissions you just changed to all subfolders and files.
10. Reboot your computer to assure that these changes have taken effect.

Yes, I was in as much shock as you are. Uncheck a flag, and then re-check it. I lost almost three days because of this bug.

That’s about it! A series of (mostly) undocumented steps that are required in order to get the Oracle adapter to function in a 64-bit environment. Fortunately, I had some great support from the escalation engineer’s with the Microsoft Product Support Services group.

I hope this post helps someone else avoid much of the pain and agony I had to go through …

Best of luck!
