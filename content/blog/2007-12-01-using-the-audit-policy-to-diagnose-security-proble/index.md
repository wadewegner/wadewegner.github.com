---
title: "Using the Audit Policy to diagnose security problems"
date: 2007-12-01T00:00:00-08:00
draft: false
description: "From the whiteboard to the keyboard"
---

A neat trick to help you diagnose troublesome security problems. Modify your the audit settings for process tracking, so that successes and failures are logged in your Security log.

1. Go to **Start** -> **Run**.
2. Type: gpedit.msc
3. Expand **Local Computer Policy** -> **Computer Configuration** -> **Windows Settings** -> **Security Settings** -> **Local Policies** -> **Audit Policy**.

![image](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/UsingtheAuditPolicytodiagnosesecuritypro_74CD/image_thumb_2.png)
4. Review the “Audit process tracking” policy.
5. Right-click the “Audit process tracking” policy, and select **Properties**.
6. On the Local Security Setting folder, check the “Success” and “Failure” checkboxes under “Audit these attempts”.

![image](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/UsingtheAuditPolicytodiagnosesecuritypro_74CD/image_thumb_4.png)

7. Click **OK** to continue.

If you define this policy setting, you can specify whether to audit successes, audit failures, or not audit the event type at all. Success audits generate an audit entry when the process being tracked succeeds. Failure audits generate an audit entry when the process being tracked fails.

These audits are now tracked in the Security log in the Event Viewer. Here’s an example of a “Detailed Tracking” event.

![Event](https://wadewegner.blob.core.windows.net/wordpress/content/binary/WindowsLiveWriter/UsingtheAuditPolicytodiagnosesecuritypro_74CD/Event_thumb.jpg)

Some additional details can be found [on TechNet](http://technet2.microsoft.com/windowsserver/en/library/0a642c0c-387a-44f5-bfd9-951b87fd13801033.mspx?mfr=true).

Pretty easy to configure, and very useful when you’re trying to figure out why applications are not running appropriately and you think it might be related to security issues.
