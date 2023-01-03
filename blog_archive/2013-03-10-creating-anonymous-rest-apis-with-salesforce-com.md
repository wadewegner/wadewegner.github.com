---
author: admin
categories:
- Apex
- Force.com
- REST API
comments: true
date: "2013-03-10T21:23:17Z"
slug: creating-anonymous-rest-apis-with-salesforce-com
title: Creating Anonymous Apex REST APIs with Force.com
wordpress_id: 1993
---

The Force.com REST API lets you integration with Force.com applications using standard HTTP methods. This API provides a way to expose the data you have within your Force.com application to external applications – both mobile and non-mobile. A few useful bits of information related to these REST APIs:
	
  * Use standard HTTP verbs: GET, POST, PUT, PATCH, DELETE, and HEAD.
  * You can use either HTTP or HTTPS.
  * Use standard security to authenticate your REST calls via OAuth 2.0.
  * Serialize your data in either XML or JSON format.

Force.com provides this capability out-of-the-box (OOTB) for objects you create and supports standard CRUD (create, read, update, and delete) against your objects. While this is great, there are a few scenarios that are not supported OOTB in Force.com – complex data queries (i.e. joins) via the API, custom logic or rules, and anonymous access.

In this post I’ll show you how you can create REST APIs using Apex REST in order to anonymous access to your APIs.

Following is a detailed tutorial:

1.	Start by creating a new Force.com developer account. Why create a new account? Well, why not? It provides a good sandbox for testing things out and you can have as many as you want. Note that the although the login you create looks like an email address it doesn’t have to be your email address – it’s simply a login. You can create a nonsensical login – i.e. anonymous@restcall.com – and just throw it away when you’re done.

2.	Let’s create a custom object. Under **App Setup** click **Schema Builder**. Click the **Elements** tab and drag an **Object** into the schema canvas.

	![image](http://www.wadewegner.com/content/Create-a_7D04/image_thumb.png)
	
3.	For the **Label** choose “Widget”, for the **Plural Label** choose “Widgets”, and for the **Record Name** choose “Name". Click **Save**.

	![image](http://www.wadewegner.com/content/Create-a_7D04/image_thumb_3.png)
	
4.	It’s time to create the Apex REST API. Close the Schema Builder and under **App Setup** expand **Developer** and click **Apex Classes**. Click **New** to create a new class.

5.	Paste the following code into **Apex Class Editor**:

        @RestResource(urlMapping='/Widgets/*')
        global class WidgetController {

            @HttpGet
            global static List<Widget__c> getWidgets() {
                List<Widget__c> widgets = [SELECT Name from Widget__c];
                return widgets;
            }

            @HttpPost 
            global static String createNewWidget(String Name) {
                Widget__c w = new Widget__c();
                w.Name = Name;
                insert w;

                return 'Done';
           }

            @HttpDelete
            global static String deleteWidgetById() {
                String Id = RestContext.request.params.get('Id');
                List<Widget__c> w = [ Select ID from Widget__c where Id= :Id];

                delete w;

                return 'Deleted Widget';
            }

            @HttpPut
            global static String updateWidget(String Id, String NewName) {
                Widget__c w = [ Select ID, Name from Widget__c where Id= :Id];

                w.Name = NewName;
                update w;

                return 'Widget Updated';
            }
        }
6.	To access these APIs we have to create a site and provide access to this Apex class. Force.com sites enable you to create public websites and applications. Under **App Setup** expand **Develop** and click **Sites.**

7. 	Choose a Force.com domain name. Ensure it is available, click the checkbox, then click **Register My Force.com Domain**.

	![image](http://www.wadewegner.com/content/Create-a_7D04/image_thumb_4.png)

8.	Once your site is registered click the **New** button.

9.	Give yourself a **Site Label**, **Site Name**, and choose an **Active Site Home Page**. Since no one will use this site as a website I’ve chosen **FileNotFound**. Click the **Save** button.

10.	By default the site is not active. Click the **Activate** button or else you will not be able to access your APIs.

	![image](http://www.wadewegner.com/content/Create-a_7D04/image_thumb_5.png)

11.	Next we have to grant anonymous access to our Apex REST API. Click the **Public Access Settings** button.

	![image](http://www.wadewegner.com/content/Create-a_7D04/image_thumb_6.png)

12.	Scroll down the **Custom Object Permissions** and click the **Edit** button underneath the **Widgets**.

	![image](http://www.wadewegner.com/content/Create-a_7D04/image_thumb_7.png)

	Scroll down again to **Custom Object Permissions **(yeah, I know …) and check all the boxes. Click **Save**.

	![image](http://www.wadewegner.com/content/Create-a_7D04/image_thumb_8.png)

13.	Scroll down to **Enabled Apex Class Access** and click the **Edit** button. Select the **WidgetController** and click the **Add** button. Click **Save**.

	![image](http://www.wadewegner.com/content/Create-a_7D04/image_thumb_9.png)

14.	Browse to [http://www.hurl.it/](http://www.hurl.it/). This is a great website for making HTTP requests and will let us test the GET, POST, PATCH, and DELETE.

15.	Right now our object doesn’t have any data. Before we make a Get request let’s add some data. Enter [https://wadeapitest-developer-edition.na15.force.com/services/apexrest/Widgets](https://wadeapitest-developer-edition.na15.force.com/services/apexrest/Widgets) in the URL (note: the first part of this URL is from step 7 above). Change to **POST **(and if necessary click the **set post body** link. Add the following text into the body:

		{"Name":"Widget1"}

	Click **+ add header** and enter “Content-Type” for the **name** and “application/json; charset=UTF-8” for the value. Click the **Send** button.

	![image](http://www.wadewegner.com/content/Create-a_7D04/image_thumb_10.png)
	
	You should get a response like the following:

	![image](http://www.wadewegner.com/content/Create-a_7D04/image_thumb_11.png)

16.	Change from **POST** to **GET** and click **Send.** You should get the following response:

	![image](http://www.wadewegner.com/content/Create-a_7D04/image_thumb_12.png)

	Copy the **Id** value – we’ll use this in a moment.

17.	Change from **GET** to **PUT**, enter the following information in the body, and click **Send**.

		{"Id":"a00i0000001RdL4AAK","NewName":"Widget1a"}

	You should get the following response:

	![image](http://www.wadewegner.com/content/Create-a_7D04/image_thumb_13.png)

	If you quickly flip back to **GET** and click **Send** you’ll see that your record has been updated.

18. Finally, let’s delete the record. Change the URL to [https://wadeapitest-developer-edition.na15.force.com/services/apexrest/Widgets?Id=a01i0000000Z4jzAAC](https://wadeapitest-developer-edition.na15.force.com/services/apexrest/Widgets?Id=a01i0000000Z4jzAAC) (again, use your site name and the proper Id value), change from **PUT** (or **GET**) to **DELETE**, and click **Send**. Now the record is gone!

That’s it!

The beauty of this technique is that any platform or language that can communicate over this HTTP methods – i.e. GET, POST, PUT, and DELETE – can execute CRUD operations against your custom object without having to authenticate. Pretty cool!

Now, should you always allow for CRUD over an anonymous set of APIs? Probably not. Just because I’ve shown you how to do it doesn’t mean you should. However, I’m sure you can think of some scenarios when the aforementioned capabilities are useful.

I hope this helps!
