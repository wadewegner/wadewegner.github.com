---
title: "Script the creation and assignment of Windows groups for Commerce Server"
date: 2007-06-14T00:00:00-08:00
draft: false
description: "From the whiteboard to the keyboard"
---

Holy moly, I’ve gone script crazy!

Here’s another little script that helps with the installation of Commerce Server 2007 (perhaps when I’m all done, I’ll consolidate them all into an uber-script). This script creates the Business Management Administrator Windows groups, which are used to control authorization roles within the Authorization Manager.

This script creates four Windows groups (CatalogAdminGroup, MarketingAdminGroup, ProfilesAdminGroup, and OrdersAdminGroup) and then assigns users to those groups.

Without further ado, here’s the script:

```
' ===================================================================
' Author: Wade Wegner
' Create date: 06/14/2007
' Description: Automate the creation and assigning of Windows Groups
' File Name: CreateAndAssignCSGroups.vbs
' ===================================================================

' Set the local computer name. Unlike other examples, use the computer name,
' rather than "."; the AssignUserToGroup method requires the actual name
strComputer = "CS2007"

strCatalogAdminGroup = "CatalogAdminGroup"
strMarketingAdminGroup = "MarketingAdminGroup"
strProfilesAdminGroup = "ProfilesAdminGroup"
strOrdersAdminGroup = "OrdersAdminGroup"
strIISWorkerProcessGroup = "IIS_WPG"

' Run the Load method
Load

' Encapsulates the processing of this script
Sub Load()

    ' Create the windows groups
    CreateWindowsGroup strCatalogAdminGroup, "Catalog administration group"
    CreateWindowsGroup strMarketingAdminGroup, "Marketing administration group"
    CreateWindowsGroup strProfilesAdminGroup, "Profiles administration group"
    CreateWindowsGroup strOrdersAdminGroup, "Orders administration group"

    ' Add any users you desire
    AssignUserToGroup "Administrator", strCatalogAdminGroup
    AssignUserToGroup "Administrator", strMarketingAdminGroup
    AssignUserToGroup "Administrator", strProfilesAdminGroup
    AssignUserToGroup "Administrator", strOrdersAdminGroup

    ' This adds the various service accounts to the IIS_WPG group, so that the
    ' services can run as the identity for IIS app pools
    AssignUserToGroup "RunTimeUser", strIISWorkerProcessGroup
    AssignUserToGroup "CatalogWebSvc", strIISWorkerProcessGroup
    AssignUserToGroup "MarketingWebSvc", strIISWorkerProcessGroup
    AssignUserToGroup "OrdersWebSvc", strIISWorkerProcessGroup
    AssignUserToGroup "ProfilesWebSvc", strIISWorkerProcessGroup

    Msgbox "Complete!"

End Sub

' Create the Windows group
Sub CreateWindowsGroup(groupName, description)

    Set objComputer = GetObject("WinNT://" & strComputer & "")
    Set objGroup = objComputer.Create("group", groupName)

    objGroup.Description = description
    objGroup.SetInfo

End Sub

' Assign the user to the Windows group
Sub AssignUserToGroup(userName, groupName)

    Set objGroup = GetObject("WinNT://" & strComputer & "/" & groupName & ",group")
    Set objUser = GetObject("WinNT://" & strComputer & "/" & userName & ",user")

    objGroup.Add(objUser.ADsPath)

End Sub
```

Pretty straightforward. Nothing too fancy or flashy.

[CreateAndAssignCSGroups.vbs (1.98 KB)](https://wadewegner.blob.core.windows.net/wordpress/content/binary/CreateAndAssignCSGroups.vbs)

I hope someone fiinds this helpful!
