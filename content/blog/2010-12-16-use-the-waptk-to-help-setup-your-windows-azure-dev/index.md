---
title: "Use the WAPTK to help setup your Windows Azure development environment"
date: 2010-12-16T00:00:00-08:00
draft: false
description: "From the whiteboard to the keyboard"
---

As awesome as it is to have a lot of great local development tools, it’s also be difficult to setup new development environments. Downloading and installing the Windows Azure SDK is really only one step – you also have to ensure that local services are configured correctly (e.g. IIS), you may need additional SDKs (e.g. Windows Identity Foundation SDK), setup additional tools (e.g. SSMS), and so on. Not only does this take time but also organizational skills.

So, is there anything that can help manage this process?

Yes. As part of the [Windows Azure Platform Training Kit](http://www.microsoft.com/downloads/en/details.aspx?FamilyID=413E88F8-5966-4A83-B309-53B7B77EDF78&displaylang=en) (WAPTK), we ship a Dependency Checker tool along with scripts that check your system for all the required software to complete the hands-on labs in the kit. I routinely use this tool to ensure that I have all the software required in order to build great applications for Windows Azure.

Try it out. First, grab latest version of the WAPTK [here](http://www.microsoft.com/downloads/en/details.aspx?FamilyID=413E88F8-5966-4A83-B309-53B7B77EDF78&displaylang=en). Then follow these steps.

1. Click the \*\*Prerequisites \*\*tab.
2. Click the \*\*Check dependencies \*\*link.
   ![image](https://wadewegner.blob.core.windows.net/wordpress/2010/12/image2.png)
3. If you are prompted to install the Dependency Checker tool, click **OK** to start the installation.
   ![image](https://wadewegner.blob.core.windows.net/wordpress/2010/12/image3.png)
4. Once the Dependency Checker tool is install, **hit F5** to refresh the page (this will allow the script to call to launch the Dependency Checker tool).
5. When prompted to allow the **ConfigurationWizard** to run, click the **Allow** button.
   ![image](https://wadewegner.blob.core.windows.net/wordpress/2010/12/image4.png)
6. Now that the Configuration Wizard has launched, click **Next** to begin.
   ![image](https://wadewegner.blob.core.windows.net/wordpress/2010/12/image5.png)
7. The first (and only) step is to check prerequisites for the Training Kit. Click **Next** to continue.
   ![image](https://wadewegner.blob.core.windows.net/wordpress/2010/12/image6.png)
8. The tool will scan your system and look for required software. When it finds that your system is missing required software, you are both notified and provided with a link to **Install** the software.
   ![Install](https://wadewegner.blob.core.windows.net/wordpress/2010/12/Install.png)
9. Clicking the **Install** link will generally launch a process to install the feature.
   ![image](https://wadewegner.blob.core.windows.net/wordpress/2010/12/image7.png)
10. In some cases you will have the option to download the missing feature or software. Click the **Download** links to launch a download. You will then have to walk through the installation process for that feature.
    ![Download](https://wadewegner.blob.core.windows.net/wordpress/2010/12/Download.png)
11. At any point you can click the **Rescan** button to scan your system again. Any updates you’ve made will be reflected on the scan.
    ![image](https://wadewegner.blob.core.windows.net/wordpress/2010/12/image8.png)
12. Once you have all of the required software, you’ll be able to complete the tool. However, if there is software you do not need or want to install, you can cancel at any time to finish.

I hope you find this useful! Please let me know if you have any feedback.
