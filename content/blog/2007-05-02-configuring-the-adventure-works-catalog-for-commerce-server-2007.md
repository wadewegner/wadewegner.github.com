---
author: admin
categories:
- Commerce Server
comments: true
date: "2007-05-02T20:23:28Z"
slug: configuring-the-adventure-works-catalog-for-commerce-server-2007
title: Configuring the Adventure Works catalog for Commerce Server 2007
wordpress_id: 92
---

I thought I'd share this quick and dirty way to load the Adventure Works catalog into Commerce Server 2007 so that you can use it with the Starter Site. This catalog, along with the Start Site, is a great way to familiarize yourself with well designed catalog and how it interacts with Start Site.

This procedure assumes that you've downloaded the Starter Site, and unpacked it to the default location. Also, it assumes that you've setup the Business Management Applications to interact correctly with the Commerce Server web services, which in turn assumes you've configured security for the web services through Azman.msc appropriately.

1. Open up the Commerce Server Catalog Manager.

2. Under the Tasks pane, click Import a Catalog. This launches the Import Product Catalog Wizard. Click Next to continue.

3. On the File Location screen, click the Browse button, and browse to the following XML file: C:Program FilesMicrosoft Commerce Server 2007SdkSamplesCatalogAdvWorksCatalog.xml. This XML file is an export of the Adventure Works catalog.

4. Change the type from Incremental to Replace. Most likely you don't already have this catalog, but if you do this it will ensure that it replaces the current catalog.

5. Click the Advanced check box, just so that in the next few steps you can see the advanced options. Click the Next button to continue.

6. On the first Advanced Import Properties screen, confirm that the Adventure Works Catalog is selected and click the Next button to continue.

7. On the second Advanced Import Properties screen, ensure that all import properties are selected. Leave all the default selections and click the Next button to continue.

8. On the Import Catalog Summary screen, review your summary. If everything looks good, click the Create button. The import will take minute or so to complete.

9. Click View --> Status to pull up the Progress Status window. The import runs asynchronously, so you can watch the status of the catalog import. Once you see that the catalog has been imported successfully, return to the Catalogs view and click the Refresh button. You should now see the "Adventure Works Catalog" under your catalogs explorer.

Make sure that your CatalogSets allow anonymous and registered users to view your new catalog. This CatalogSets determine which catalogs users have access to view and interact. Click the CatalogSets link under Views, and double-click the Anonymous User Default CatalogSet. Click the Add All Catalogs check box, and click the Refresh button. You should see your Adventure Works catalog in the Included Catalogs list box. Click Save and Continue, and do the same thing for the Registered User Default CatalogSet.

Now that you have successfully imported the Adventure Works catalog and setup the CatalogSets, click the Refresh Site Cache button. If you have problems updating the Site Cache, be sure to read the following article: [http://msdn2.microsoft.com/en-us/library/aa546079.aspx](http://msdn2.microsoft.com/en-us/library/aa546079.aspx). An alternative is to run IISRESET from the command line, which forces the Web site to reload its cache, but I recommend you configure your sites appropriately so that the Refresh Site Cache functions.

Before you try to run the Starter Site and see browse your catalog, let's grab some some of the files within the AdvWorksImages.cab from the C:\Program Files\Microsoft Commerce Server 2007\Sdk\Samples\Catalog folder.

1. Create an "Images" folder under the Start Site (by default, it is found at: C:InetpubwwwrootStarterSite).

2. Extract the images from the AdvWorksImages.cab file into this new folder. You can do this by double-clicking the cab file, selecting and copying all the files, and paste them into the new folder.

Now you're ready to browse the Adventure Works catalog. Take the time to browse products on the Web site, as well as the catalog itself. Try creating some product relationships for cross- and up-sell opportunities, as well as new products. Be sure to refresh your site cache, or you won't see your changes reflected on the Web site.

If you want to be able to add items to your cart, then you'll have to add your Adventure Works catalog to an inventory catalog. Under the Catalog Manager, either create a new inventory catalog, or add your Adventure Works catalog to the default catalog. Once this is done, you'll then have to go back to your Adventure Works catalog and add inventory quantities to your items. I'll save this process for a future blog.

Best of luck!