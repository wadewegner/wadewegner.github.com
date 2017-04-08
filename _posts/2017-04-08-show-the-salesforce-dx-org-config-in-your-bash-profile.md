---
layout: post
title: "Show the Salesforce DX Org Config in your Bash Prompt"
description: "Want to display a hint showing your default Salesforce DX org configuration in your bash prompt? You'll learn how in this post!"
categories: 
- salesforce
- salesforce dx
- sfdx
- cli
- bash
---

Using the capabilities of Salesforce DX you can enable some neat tricks. Such as displaying your org configuration in your Bash prompt:

![Bash](/content/2017/04/08/bash.gif)

But, first, a little background ...

The Salesforce DX CLI provides some powerful capabilities for tracking your Salesforce org. When you login with `sfdx force:auth:web:login` (`-a <alias>` lets you alias the org) your credentials are encrypted and stored locally. When running subsequent commands, you can append `-u <username|alias>` to the command to target the local org. Since the CLI stores the refresh token, it can generate a new access token automatically.

Two additional things you should understand are:

1. The dev hub org vs the default org

2. Global vs local orgs

The dev hub org is the production org you esignate as the hub to track all of your scratch orgs. (In the future the dev hub will do more.) Some of the CLI commands (i.e. `sfdx force:org:create`) run against the dev hub org. The default org is the org all other commands run against (i.e. `sfdx force:apex:execute`).

The Salesforce DX CLI lets you set global orgs for the dev hub and your default org. This means that when you run the commands without `-u` it will target those global orgs. That is, unless your local workspace has specified a local dev hub org or default org, in which case it will use the local orgs. You can set defaults using `sfdx force:config:set` or when you create a scratch org using `-s` (i.e. `sfdx force:org:create -s`).

While these capabilities are powerful, it can be difficult to keep track which org your currently tracking.

To solve this, I've updated my `.bash_profile` to display the dev hub org and default org. If it doesn't find a local org for the workspace it will display the global org.

Here's the `.bash_profile` code:

```bash
bldwht='\e[1;37m' # White
bldgrn='\e[1;32m' # Green
txtylw='\e[0;33m' # Yellow

get_usernames() {
  config="$(cat .sfdx/sfdx-config.json 2> /dev/null)";
  globalConfig="$(cat ~/.sfdx/sfdx-config.json)";

  defaultusername="$(echo ${config} | jq -r .defaultusername)"
  defaultdevhubusername="$(echo ${config} | jq -r .defaultdevhubusername)"
  globaldefaultusername="$(echo ${globalConfig} | jq -r .defaultusername)"
  globaldefaultdevhubusername="$(echo ${globalConfig} | jq -r .defaultdevhubusername)"

  echoString="$bldwht""hub: $bldgrn";
  if [ ! $defaultdevhubusername = "null" ]
  then
    echoString=$echoString$defaultdevhubusername"$txtylw (local)"
  else
    echoString=$echoString$globaldefaultdevhubusername"$txtylw (global)"
  fi
  echo "\n"$echoString

  echoString="$bldwht""default: $bldgrn"
  if [ ! $defaultusername = "null" ]
  then
    echoString=$echoString$defaultusername"$txtylw (local)"
  else
    echoString=$echoString$globaldefaultusername"$txtylw (global)"
  fi
  echo $echoString"\n"
}

print_before_the_prompt () {
    printf "$(get_usernames)"
}

PROMPT_COMMAND=print_before_the_prompt
PS1='-> '
```

It's not 100% perfect, but it's extremely useful. Now, the same information could be determine with `sfdx force:config:list` with the advantage that it's 100% accurate (my script isn't accurate in workspace subdirectories, for example), but there's still some latency we need to workout of the CLI. When that latency is resolved I'll update to use that command.

*Note: this script uses files directly that, frankly, you shouldn't use. Note that we may change these files at any point. Please do not base anything important on these files directly. Instead, use the CLI.*

I hope you find this useful!

Special thanks to [Chris Wall (@cwall)](https://twitter.com/cwall), an amazing architect and developer on the Salesforce DX team, for the inspiration!