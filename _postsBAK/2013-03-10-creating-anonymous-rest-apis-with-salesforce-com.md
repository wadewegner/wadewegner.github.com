--- 
layout: post
title: Creating Anonymous Apex REST APIs with Force.com
categories: 
- Apex
- Force.com
- REST API
tags: []

status: publish
type: post
published: true
meta: 
  _wpas_done_all: "1"
  _topsy_long_url: http://www.wadewegner.com/2013/03/creating-anonymous-rest-apis-with-salesforce-com/
  topsy_short_url: http://bit.ly/13PZV5K
  dsq_thread_id: "1129227384"
  _edit_last: "1"
---

The Force.com REST API lets you integration with Force.com applications using standard HTTP methods. This API provides a way to expose the data you have within your Force.com application to external applications – both mobile and non-mobile. A few useful bits of information related to these REST APIs:

- Use standard HTTP verbs: GET, POST, PUT, PATCH, DELETE, and HEAD.</li>
- You can use either HTTP or HTTPS.</li>
- Use standard security to authenticate your REST calls via OAuth 2.0.</li>
- Serialize your data in either XML or JSON format.</li>

Force.com provides this capability out-of-the-box (OOTB) for objects you create and supports standard CRUD (create, read, update, and delete) against your objects. While this is great, there are a few scenarios that are not supported OOTB in Force.com – complex data queries (i.e. joins) via the API, custom logic or rules, and anonymous access.

In this post I’ll show you how you can create REST APIs using Apex REST in order to anonymous access to your APIs.

Following is a detailed tutorial:

1.  Start by creating a new Force.com developer account. Why create a new account? Well, why not? It provides a good sandbox for testing things out and you can have as many as you want. Note that the although the login you create looks like an email address it doesn’t have to be your email address – it’s simply a login. You can create a nonsensical login – i.e. anonymous@restcall.com – and just throw it away when you’re done.

2.  Let’s create a custom object. Under <strong>App Setup</strong> click <strong>Schema Builder</strong>. Click the <strong>Elements</strong> tab and drag an <strong>Object</strong> into the schema canvas.

    ![image](http://www.wadewegner.com/wp-content/uploads/Create-a_7D04/image.png)

3.  For the <strong>Label</strong> choose “Widget”, for the <strong>Plural Label</strong> choose “Widgets”, and for the <strong>Record Name</strong> choose “Name". Click <strong>Save</strong>.

    <a href="http://www.wadewegner.com/wp-content/uploads/Create-a_7D04/image_3.png"><img style="background-image: none; padding-left: 0px; padding-right: 0px; display: inline; padding-top: 0px; border-width: 0px;" title="image" alt="image" src="http://www.wadewegner.com/wp-content/uploads/Create-a_7D04/image_thumb_3.png" width="481" height="612" border="0" /></a>

4. It’s time to create the Apex REST API. Close the Schema Builder and under <strong>App Setup</strong> expand <strong>Developer</strong> and click <strong>Apex Classes</strong>. Click <strong>New</strong> to create a new class.

5. Paste the following code into <strong>Apex Class Editor</strong>:

    {% highlight c %}
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
    }{% endhighlight %}
6.  To access these APIs we have to create a site and provide access to this Apex class. Force.com sites enable you to create public websites and applications. Under <strong>App Setup</strong> expand <strong>Develop</strong> and click <strong>Sites.</strong>

7.  Choose a Force.com domain name. Ensure it is available, click the checkbox, then click <strong>Register My Force.com Domain</strong>.

    <a href="http://www.wadewegner.com/wp-content/uploads/Create-a_7D04/image_4.png"><img style="background-image: none; padding-left: 0px; padding-right: 0px; display: inline; padding-top: 0px; border-width: 0px;" title="image" alt="image" src="http://www.wadewegner.com/wp-content/uploads/Create-a_7D04/image_thumb_4.png" width="497" height="152" border="0" /></a>

8. Once your site is registered click the <strong>New</strong> button.

9. Give yourself a <strong>Site Label</strong>, <strong>Site Name</strong>, and choose an <strong>Active Site Home Page</strong>. Since no one will use this site as a website I’ve chosen <strong>FileNotFound</strong>. Click the <strong>Save</strong> button.

10. By default the site is not active. Click the <strong>Activate</strong> button or else you will not be able to access your APIs.

    <a href="http://www.wadewegner.com/wp-content/uploads/Create-a_7D04/image_5.png"><img style="background-image: none; padding-left: 0px; padding-right: 0px; display: inline; padding-top: 0px; border-width: 0px;" title="image" alt="image" src="http://www.wadewegner.com/wp-content/uploads/Create-a_7D04/image_thumb_5.png" width="567" height="69" border="0" /></a>

11. Next we have to grant anonymous access to our Apex REST API. Click the <strong>Public Access Settings</strong> button.

    <a href="http://www.wadewegner.com/wp-content/uploads/Create-a_7D04/image_6.png"><img style="background-image: none; padding-left: 0px; padding-right: 0px; display: inline; padding-top: 0px; border-width: 0px;" title="image" alt="image" src="http://www.wadewegner.com/wp-content/uploads/Create-a_7D04/image_thumb_6.png" width="567" height="69" border="0" /></a>

12. Scroll down the <strong>Custom Object Permissions</strong> and click the <strong>Edit</strong> button underneath the <strong>Widgets</strong>.

    <a href="http://www.wadewegner.com/wp-content/uploads/Create-a_7D04/image_7.png"><img style="background-image: none; padding-left: 0px; padding-right: 0px; display: inline; padding-top: 0px; border-width: 0px;" title="image" alt="image" src="http://www.wadewegner.com/wp-content/uploads/Create-a_7D04/image_thumb_7.png" width="348" height="169" border="0" /></a>

    Scroll down again to <strong>Custom Object Permissions </strong>(yeah, I know …) and check all the boxes. Click <strong>Save</strong>.

    ![image](http://www.wadewegner.com/wp-content/uploads/Create-a_7D04/image_thumb_8.png)

13. Scroll down to <strong>Enabled Apex Class Access</strong> and click the <strong>Edit</strong> button. Select the <strong>WidgetController</strong> and click the <strong>Add</strong> button. Click <strong>Save</strong>.

    <a href="http://www.wadewegner.com/wp-content/uploads/Create-a_7D04/image_9.png"><img style="background-image: none; padding-left: 0px; padding-right: 0px; display: inline; padding-top: 0px; border-width: 0px;" title="image" alt="image" src="http://www.wadewegner.com/wp-content/uploads/Create-a_7D04/image_thumb_9.png" width="391" height="342" border="0" /></a>

14. Browse to <a href="http://www.hurl.it/">http://www.hurl.it/</a>. This is a great website for making HTTP requests and will let us test the GET, POST, PATCH, and DELETE.

15. Right now our object doesn’t have any data. Before we make a Get request let’s add some data. Enter <a title="https://wadeapitest-developer-edition.na15.force.com/services/apexrest/Widgets" href="https://wadeapitest-developer-edition.na15.force.com/services/apexrest/Widgets">https://wadeapitest-developer-edition.na15.force.com/services/apexrest/Widgets</a> in the URL (note: the first part of this URL is from step 7 above). Change to <strong>POST </strong>(and if necessary click the <strong>set post body</strong> link. Add the following text into the body:
<pre class="code"><span style="color: black;">{</span><span style="color: #a31515;">"Name"</span><span style="color: black;">:</span><span style="color: #a31515;">"Widget1"</span><span style="color: black;">}</span></pre>
Click <strong>+ add header</strong> and enter “Content-Type” for the <strong>name</strong> and “application/json; charset=UTF-8” for the value. Click the <strong>Send</strong> button.

    <a href="http://www.wadewegner.com/wp-content/uploads/Create-a_7D04/image_10.png"><img style="background-image: none; padding-left: 0px; padding-right: 0px; display: inline; padding-top: 0px; border-width: 0px;" title="image" alt="image" src="http://www.wadewegner.com/wp-content/uploads/Create-a_7D04/image_thumb_10.png" width="777" height="584" border="0" /></a>

    You should get a response like the following:

    ![image](http://www.wadewegner.com/wp-content/uploads/Create-a_7D04/image_thumb_11.png)

16. Change from <strong>POST</strong> to <strong>GET</strong> and click <strong>Send.</strong> You should get the following response:

    ![image](http://www.wadewegner.com/wp-content/uploads/Create-a_7D04/image_thumb_12.png)

Copy the <strong>Id</strong> value – we’ll use this in a moment.

17. Change from <strong>GET</strong> to <strong>PUT</strong>, enter the following information in the body, and click <strong>Send</strong>.
<pre class="code"><span style="color: black;">{</span><span style="color: #a31515;">"Id"</span><span style="color: black;">:</span><span style="color: #a31515;">"a00i0000001RdL4AAK"</span><span style="color: black;">,</span><span style="color: #a31515;">"NewName"</span><span style="color: black;">:</span><span style="color: #a31515;">"Widget1a"</span><span style="color: black;">}</span></pre>
You should get the following response:

    <a href="http://www.wadewegner.com/wp-content/uploads/Create-a_7D04/image_13.png"><img style="background-image: none; padding-left: 0px; padding-right: 0px; display: inline; padding-top: 0px; border-width: 0px;" title="image" alt="image" src="http://www.wadewegner.com/wp-content/uploads/Create-a_7D04/image_thumb_13.png" width="765" height="131" border="0" /></a>

    If you quickly flip back to <strong>GET</strong> and click <strong>Send</strong> you’ll see that your record has been updated.

18. Finally, let’s delete the record. Change the URL to <a title="https://wadeapitest-developer-edition.na15.force.com/services/apexrest/Widgets?Id=a01i0000000Z4jzAAC" href="https://wadeapitest-developer-edition.na15.force.com/services/apexrest/Widgets?Id=a01i0000000Z4jzAAC">https://wadeapitest-developer-edition.na15.force.com/services/apexrest/Widgets?Id=a01i0000000Z4jzAAC</a> (again, use your site name and the proper Id value), change from <strong>PUT</strong> (or <strong>GET</strong>) to <strong>DELETE</strong>, and click <strong>Send</strong>. Now the record is gone!

That’s it!

The beauty of this technique is that any platform or language that can communicate over this HTTP methods – i.e. GET, POST, PUT, and DELETE – can execute CRUD operations against your custom object without having to authenticate. Pretty cool!

Now, should you always allow for CRUD over an anonymous set of APIs? Probably not. Just because I’ve shown you how to do it doesn’t mean you should. However, I’m sure you can think of some scenarios when the aforementioned capabilities are useful.

I hope this helps!
