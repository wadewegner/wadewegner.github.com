---
author: admin
categories:
- BizTalk Server
- Tools
- Troubleshooting
comments: true
date: "2007-08-13T21:48:34Z"
slug: warning-the-cid-values-for-both-test-machines-are-the-same
title: 'Warning: the CID values for both test machines are the same'
wordpress_id: 46
---

I came across a <strike>frustrating</strike> interesting problem today where the BizTalk Server 2006 Configuration wizard would fail every time I applied a new configuration.

Unlike a typical BizTalk developer environment (which usually consists of BizTalk and SQL Server on the same machine), this environment consisted of two separate machines: one BizTalk Server 2006 and one SQL Server 2005 (i.e. the BizTalk databases are stored on the SQL Server). Additionally, this is a virtual environment and both machines were cloned from the same base Windows Server 2003 R2 template.

I was installing the following components ...

  * Enterprise Single Sign-On (SSO) 
  * Group 
  * BizTalk Runtime 
  * MSMQT 

It would successfully install ENTSSO, but would fail when installing the group. The log file reported the following error:

> Failed to configure with error message [Exception of type 'System.EnterpriseServices.TransactionProxyException' was thrown.]

The following [Google search](http://www.google.com/search?hl=en&rls=com.microsoft%3Aen-us%3AIE-SearchBox&rlz=1I7GGIG&q=biztalk+Exception+of+type+%27System.EnterpriseServices.TransactionProxyException%27+was+thrown.) suggested to me that the underlying problem was with MSDTC (_aren't all BizTalk problems?_). I checked, and double-checked, the MSDTC properties on both servers and couldn't find anything wrong with the configuration. So, I had to pull out the big guns.

I downloaded [DTCPing](http://www.microsoft.com/downloads/details.aspx?FamilyID=5e325025-4dcd-4658-a549-1d549ac17644&DisplayLang=en) (a very handy tool for debugging DTC issues) and ran it on both machines (make sure to read the instructions on how to use DTCPing as it is not straightforward). In the generated log file I noticed the following warning:

> WARNING: the CID values for both test machines are the same while this problem won't stop DTCping test, MSDTC will fail for this ...

A [Google search](http://www.google.com/search?sourceid=navclient&ie=UTF-8&rlz=1T4GGIG_enUS227US227&q=the+CID+values+for+both+test+machines+are+the+same+) on this warning helped me to understand that the underlying problem is that the CID values stored for MSDTC were not changed during the cloning process. But of course!

If you're experiencing this problem, check the following registry key on both of your machines. Are the keys identical?

> HKEY_CLASSES_ROOTCID

Mine were. Here's the steps I took successfully reinstall MSDTC so that the CID values were unique. Run this procedure on both machines:

  1. Use Add Windows Components, and remove Network DTC.        

  2. Go to the command line and run: MSDTC -uninstall        

  3. Go to the registry and delete the MSDTC keys in HKLM/Software/Microsoft/Software/MSDTC, HKLM/System/CurrentControlSet/Services/MSDTC, and HKEY_CLASSES_ROOTCID (if they're still there).        

  4. Reboot        

  5. Go to the command line and run: MSDTC -install        

  6. Use Add Windows Components, and add Network DTC.        

  7. Go to the command line and run: net start msdtc 

After running this on both servers I was able to confirm that the CID values were unique. And, sure enough, when I next applied my configuration to BizTalk Server 2006 everything worked perfectly.

I hope this helps!
