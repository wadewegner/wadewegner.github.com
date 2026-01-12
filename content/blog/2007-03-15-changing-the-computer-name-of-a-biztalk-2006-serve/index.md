---
title: "Changing the computer name of a BizTalk 2006 server"
date: 2007-03-15T00:00:00-08:00
draft: false
description: "From the whiteboard to the keyboard"
---

I recently found myself in a situation where I had to change the computer name of a machine I had configured as a BizTalk 2006 server. The machine was actually a virtual machine that was going to be used for development, and we made numerous copies for all the developers. Consequently, each developer needed to have a computer with a unique name.

After researching the appropriate way to do this, all I could find were very manual steps. For instance, here are some of the intial things I tried:

**Note: I do NOT recommend using this approach.**

- Rename the computer
- Add a host entry to the old computer name; add a SQL alias for the old computer name
- Execute the following in SQL: sp\_dropserver [computerName]
- Drop and recreate all SQL logins
- Update the following registry keys
  - HKEY\_LOCAL\_MACHINESOFTWAREMicrosoftBizTalk Server3.0AdministrationMgmtDbServer
  - HKEY\_LOCAL\_MACHINESOFTWAREMicrosoftBusinessRules3.0
- Update the following database tables
  - BizTalkHWSDb.HWS\_Core
  - BizTalkMgmtDb.adm\_MessageBox
  - BizTalkMgmtDb.adm\_OtherDatabases
  - BizTalkMgmtDb.adm\_Group
  - BizTalkMgmtDb.adm\_Server
  - BizTalkMgmtDb.TDDS\_Sources
  - BizTalkMgmtDb.TDDS\_Destinations
- Run the following command: ssoconfig -setdb computerName SSODB

While this worked, it was NOT an ideal solution. I still noticed a few issues in the BizTalk configuration, etc.

Once I had a night to think about this, I realized that there’s a MUCH easier way to go about doing this. Here’s the process:

1. Unconfigure the current BizTalk configuration via BizTalk Server Configuration. This essentially removes all the existing BizTalk databases.
2. Go to MSSQL, and delete all the BizTalk databases and SQL logins
3. Change the computer name, and restart.
4. Run the following two commands in MSSQL
   1. sp\_addserver [newComputerName]
   2. sp\_dropserver [oldComputerName]
5. Configure a new BizTalk configuration via BizTalk Server Configuration
6. Go into BizTalk Server Administration and remove the old BizTalk group, and add the new BizTalk group.

This solution has worked perfectly for our development environments.

Best of luck!
