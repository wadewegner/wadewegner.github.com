---
categories:
- salesforce
- salesforce dx
- sfdx
- cli
- salesforce cli
- oauth 2.0
- jwt bearer flow
date: "2017-04-03T00:00:00Z"
description: The JWT Bearer Flow enables you to perform headless authentication against
  your Salesforce org. In Salesforce DX this facilitates the automation of scripts
  without requiring an interactive login. In this post, you'll learn how to set it
  up.
title: Using the OAuth 2.0 JWT Bearer Flow to Support the Salesforce DX CLI
---

The Salesforce DX (SFDX) CLI makes it easy to run commands as a developer. It also makes it really easy to create scripts that facilitate automation. A great example is the following tweet from Pat Patterson:

<center><blockquote class="twitter-tweet"  data-lang="en"><p lang="en" dir="ltr"><a href="https://twitter.com/hashtag/SalesforceDX?src=hash">#SalesforceDX</a> is awesome! I can create a scratch org for testing from a bash script. Great job, <a href="https://twitter.com/WadeWegner">@WadeWegner</a> &amp; team! <a href="https://t.co/Nu3yI2UQH0">pic.twitter.com/Nu3yI2UQH0</a></p>&mdash; Pat Patterson (@metadaddy) <a href="https://twitter.com/metadaddy/status/846893287495512064">March 29, 2017</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script></center>

The praise for SFDX is awesome. Thanks, Pat. But that's not my purpose for sharing. This script does some awesome stuff. But notice the first command:

```bash
sfdx force:auth:jwt:grant --clientid ${CLIENT_ID} \
    --jwtkeyfile ${JWT_KEY_FILE} --username ${HUB_USERNAME} \
    --setdefaultdevhubusername > /dev/null
```

This command logs into the developer hub using only the client ID, the username, and the JWT key file; it doesn't require you to interactively login. This is important when you want your scripts to run automatically. For this to work, your local SFDX workspace (i.e. client-side) needs a private key for signing the JWT bearer token payload. The server-side needs a Connected App containing a certificate generated from that private key.

The key to the use of the JWT bearer flow is that it supports the RSA SHA256 algorithm, which uses an uploaded certificate as the signing secret. This gives us the ability to authenticate the CLI without having to interactively login. Perfect for automated builds and scripting. Read the Salesforce documentation for more details on the [OAuth 2.0 JWT Bearer Token Flow](https://help.salesforce.com/articleView?id=remoteaccess_oauth_jwt_flow.htm&type=0).

In this post, we'll create both the key and the certificate, as well as setup the Connected App to facilitate the `sfdx force:auth:jwt:grant` command.

[Update 4/6/17: you can now install my plugin (`sfdx plugins:install sfdx-oss-plugin`) and run the command `sfdx wadewegner:connectedapp:create -u <username|alias> -n <name> -r` to get through step #5. You'll still need to complete steps #6 and #7.]

1. Follow [these instructions](https://devcenter.heroku.com/articles/ssl-certificate-self) to generate a private key and a certificate signing request. The resulting `server.key` file contains your private key, and we’re going to use that as input when signing the JWT bearer token payload.

2. Execute the [subsequent instructions](https://devcenter.heroku.com/articles/ssl-certificate-self#generate-ssl-certificate) to generate a self-signed SSL certificate.

3. Log into your **Developer Hub**, and navigate to **Setup**.  Go to **App Manager**, then click the **New Connected App** button on the upper-left. (You're using the Lightning Experience, right?)

4. Create a new Connected App with information and click **Save**. Ensure you do the following:
    
    * Set the **Callback URL** to: `http://localhost:1717/OauthRedirect`

    * Check **Use digital signatures** and upload your `server.crt` certificate.

    * Add the following **OAuth Scopes**: `basic`, `api`, `web`, `refresh_token`
    
5. Save your **Consumer Key**.

6. Click the **Manage** button, then **Edit Policies**. Under the OAuth policies subsection, change the **Permitted Users** combo box to be `Admin approved users are pre-authorized`. Then click **Save**.

7. Click **Manage Profiles** OR **Manage Permission Sets**, and select the profiles and/or perm sets that should be allowed to be pre-authorized to use this Connected App. (For perm sets, create a perm set without any particular permissions, assign it your user, then assign that perm set to the connected app.) Then click **Save**.

When this is setup correctly, running the command will look like the following:

```bash
sfdx force:auth:jwt:grant --clientid ${CLIENT_ID} \
    --jwtkeyfile ${JWT_KEY_FILE} --username ${HUB_USERNAME} \
    --setdefaultdevhubusername

Successfully authorized <USERNAME> with org id 00D460000001PSmEAM
```

You can see this in action in a [sample Travis CI](https://travis-ci.org/wadewegner/sfdx-travisci) build I have setup for [https://github.com/wadewegner/sfdx-travisci](https://github.com/wadewegner/sfdx-travisci).

I hope this post helps! Special thanks to [George Murnock (gmurnock__c)](https://twitter.com/gmurnock__c) for putting together a really helpful document that is the basis of this post.