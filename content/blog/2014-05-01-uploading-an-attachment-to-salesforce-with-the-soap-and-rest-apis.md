---
categories:
- python
- rest api
- soap api
- force.com
- salesforce
date: "2014-05-01T00:00:00Z"
description: Earlier today a colleague asked me if I had an example of using Python
  to create an attachment in Salesforce. I didn't, but that didn't stop me from creating
  a couple.
title: Uploading an Attachment to Salesforce with the SOAP and REST APIs
---

Earlier today a colleague asked me if I had an example of using Python to create an attachment in Salesforce. I didn't, but that didn't stop me from creating a couple. Sadly, our documentation here isn't the greatest. You can look at the [Insert or Update Blob Data](http://www.salesforce.com/us/developer/docs/api_rest/Content/dome_sobject_insert_update_blob.htm) for the REST API or [Attachment](http://www.salesforce.com/us/developer/docs/api/Content/sforce_api_objects_attachment.htm) for our SOAP API. If you're like me, neither document is good enough; I had to continue to dig around to figure out exactly what I needed.

Nevertheless, I got it working. I'd like to share with you two examples: uploading attachments with the REST API and uploading attachments with the SOAP API. For both examples I will use Python.

Why Python?

Well, not only is it fun, but I think Python is also remarkably easy to read. It shouldn't be too much work for you to adjust to your language of choice.

A few things worth nothing before we go to far:

1. You will need to `base64` encode your file.
2. An attachment *attaches* to an object in Salesforce (i.e. Account, Contact, etc.). You'll need to know the `Id` (called the `ParentId`) to which you want to attach.
3. Python has some great libraries that I'm going to use. This has the added benefit of keeping the code really simple and focused on the task of uploading the attachment.

Without further ado ...

#### Python and Attachments with the REST API

<script src="https://gist.github.com/wadewegner/df609a495df2e4bd7a07.js?file=upload_rest.py"></script>

To run this script you'll need to do the following:

* Create a `text.txt` file in the same directory as the Python script.
* Add your user name, password, and security token.
* Install `simple_salesforce` with `pip install simple_salesforce`. I recommend you first use [virtualenv](https://pypi.python.org/pypi/virtualenv) to manage your environment.
* Update the `instance` value (or extract it from the login info).

The code is pretty straightforward. [simple_salesforce](https://pypi.python.org/pypi/simple-salesforce/) is great when you want to quickly work with the REST API.

#### Python and Attachments with the SOAP API

<script src="https://gist.github.com/wadewegner/df609a495df2e4bd7a07.js?file=upload_soap.py"></script>

To run this script you'll need to do the following:

* Create a `text.txt` file in the same directory as the Python script.
* Add your user name, password, and security token.
* Install `beatbox` with `pip install beatbox`. I recommend you first use [virtualenv](https://pypi.python.org/pypi/virtualenv) to manage your environment.

Again, this code is pretty straightforward. [beatbox](https://pypi.python.org/pypi/beatbox/20.0) is a fantastic way to interact with the SOAP API.

It's likely over time I'll learn more that I should add to this post. For now, I'll leave it as it is; hopefully someone finds it useful.

Enjoy!