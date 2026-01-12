---
title: "Managing the SecureString in the DataCacheSecurity Constructor"
date: 2011-02-09T00:00:00-08:00
draft: false
description: "From the whiteboard to the keyboard"
---

If you take a good look at the latest Release Notes for the [Windows Azure AppFabric CTP February Release](http://www.wadewegner.com/2011/02/windows-azure-appfabric-ctp-february/) – or if you jump directly into the new SDK and try to [programmatically configure the Caching client](http://www.wadewegner.com/2010/12/programmatically-configuring-the-caching-client/) – you’ll quickly discover that the **DataCacheSecurity** class now requires a **System.Security.SecureString** instead of a string.

The reason for this change in the **DataCacheSecurity** constructor is to reduce the time that an unencrypted string remains in memory. The expectation is that a user will read an authentication token in an encrypted file, and then construct the **SecureString** from it.

While this is well and good, it does make for some challenges when developing against the APIs. Consequently, you may want to create a method that takes your authorization token string and returns a **SecureString**.

> **Note**: Only use this method when you don’t need the to ensure that the authorization token stays out of memory. In many ways, the below method defeats the purpose of **SecureString**.

Here’s what the method can look like:

Code Snippet

1. static private SecureString createSecureString(string token)
2. {
3. SecureString secureString = new SecureString();
4. foreach (char c in token)
5. {
6. secureString.AppendChar(c);
7. }
8. secureString.MakeReadOnly();
9. return secureString;
10. }

Now, from your application, you can simply call this method and return the **SecureString** authorization token …

Code Snippet

1. SecureString authorizationToken = createSecureString(“TOKEN”);

… which you can then pass into the \*\*DataCacheSecurity \*\*constructor (as outlined in this [post](http://www.wadewegner.com/2010/12/programmatically-configuring-the-caching-client/)).

While this does incur some overhead, it is pretty minimal since you should only construct the **DataCacheFactory** once during the application life time.

I hope this helps!
