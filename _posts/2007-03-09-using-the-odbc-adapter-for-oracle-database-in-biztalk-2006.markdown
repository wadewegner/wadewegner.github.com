---
author: admin
comments: true
date: 2007-03-09 03:57:11+00:00
layout: post
slug: using-the-odbc-adapter-for-oracle-database-in-biztalk-2006
title: Using the "ODBC Adapter for Oracle Database" in BizTalk 2006
wordpress_id: 108
categories:
- BizTalk Server
---

Having used the "Microsoft BizTalk Adapters for Host Systems" to connect to a DB2 back-end, I thought I was ready for any adapter challenge. Turns out that using the "Microsoft BizTalk Adapters for Enterprise Applications" to connect to Oracle was a little trickier than I thought it would be.

To start, I couldn't figure out where to find the Oracle adapter! If you read the BizTalk 2006 Pricing and Licensing page ([http://www.microsoft.com/biztalk/howtobuy/default.mspx](http://www.microsoft.com/biztalk/howtobuy/default.mspx)) you'll see that "ODBC Adapter for Oracle Database" is included in the license! To me that insinuates no additional installation or download is required, but that's not the case. There are actually two prerequisites to using the Oracle adapter.

First, you need the Oracle client installed on your machine. Yes, in retrospect, this seems perfectly obvious (but, in hindsight, what isn't?). Second, you need to acquire a copy of "Microsoft BizTalk Adapters for Enterprise Applications" and install the "Microsoft BizTalk Adapter for Oracle(r) Database".

With regards to the Oracle client, there is a good article on MSDN (also found in the help documentation) that will walk you through the installation and configuration of the Oracle client ([http://msdn2.microsoft.com/en-us/library/aa547634.aspx](http://msdn2.microsoft.com/en-us/library/aa547634.aspx)). I recommend using this tutorial. The installation for the "Microsoft BizTalk Adapters for Enterprise Applications" is straightforward, and shouldn't give you any problems.

Once the two of these applications are installed (and you've registered the appropriate TNS names for Oracle connectivity), you must create an ODBC connection. To do this, do the following:

1. Administrative Tools -> Data Sources (ODBC)

2. Click the System DSN tab, followed by Add

3. Select "Oracle in OraHome92" (yours may be slightly different)

4. You'll now have to configure your ODBC connection. Make sure to specify the Data Source Name, the TNS Service Name, and the User ID. When you click "Test Connection" you will be prompted for the password. Make sure the test passes.

5. Click "OK", and "OK" to exit.

Now that the ODBC connection is created, we have to setup the Oracle adapter in the BizTalk Administrative Console. Under the BizTalk Group, expand "Platform Settings" and "Adapters". Notice that there isn't an Oracle adapter. You'll have to right-click on "Adapters" and choose "New" --> "Adapter...". From here you can choose the "Oracle(r) Database" adapter, and specify a name. I like "OracleDB". The default Host will be added automatically for both Send and Receive.

Now that all the prerequisites are out of the way, we have to go about first creating a receive port/location or a send port. Now, in my experience, this is somewhat backwards. Typically I first choose "Add Generated Item", "Add Adapter Metadata", and then select the adapter I want to use. With Oracle, you must first create your Receive Port/Location or Send Port, and then use it to define your schema.

Here's the process I use to create a Recieve Port/Location

1. Open the "BizTalk Administration Console" (I'm not a big fan of doing this in Visual Studio 2005)

2. Create a new Receive Port (let's do One-Way for simplicity), and call it "ReceiveDataFromOracleDB"

3. Create a new One-way Receive Location on the "ReceiveDataFromOracleDB" port. Specify a decent name -- "ReceiveDataFromOracleDB-ORA" isn't the best, but will do

4. Select the "OracleDB" adapter you defined earlier, and click "Configure". Specify the following values

  1. Password

  2. PATH -- with a default installation of Oracle, this would be "C:Oracleora92bin" (yours may be slightly different)

  3. Service name -- this is the ODBC connection you created

  4. User name

5. At this point I'd recommend not specifying anything else, and clicking okay. This will save the settings, and allow you to continue forward. Having clicked okay, go ahead and immediately click "Configure" again to return to the properties.

6. Click "Managing Events". This will call the Oracle Adapter Wizard, and confirm that you have correctly configured everything. If you are unable to browse the Oracle database, then you have made a mistake somewhere along the way. Click OK or Cancel to exit.

At this point, just leave your Receive Location alone. The next step will be to use the Receive Port/Location to generate a schema of an Oracle table or stored procedure.

Enter into Visual Studio 2005, and load your BizTalk project (I won't explain how to do this -- if you don't already have one, then create one). A a new item, choose "Add Generated Items ...", then select "Add Adapter Metadata". This will invoke the "Add Adapter Wizard". Select "Oracle(r) Database" from the list of registered adapters, then choose your receive location in the "Port:" dropdown list. Click Next. You will now have the ability to browse the Oracle database, and choose either your stored procedure or table. For simplicity sake, we'll choose a table. The adapter will generate the schemas required to actually retrieve data from the Oracle table. Build your project and deploy.

Once deployed, you can perform the final steps to this process. Go back to your Receive Location, and click the Configure button. Once again, click "Managing Events". This time, choose the same table that you specified in the adapter wizard. Click OK. Now, create a Send Port that uses the File adapter, and subscribe to the Receive Port (this is, in my opinion, the easiest way to test).

If everything has been done correctly, you should see XML files dump into your folder. If they don't, then look into the application log and try to figure out what the problem is. Sadly, that's beyond the scope of this post.

I hope this has helped. Please let me know if I've missed anything.

Best of luck!