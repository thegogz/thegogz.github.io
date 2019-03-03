---
layout: post
title:  "Welcome to my blog"
tags: [sfdx, bash, shell, git-bash]
---

Ok so I've said before I'd do a blog and I never really get around to it. So here we go with another attempt at it.

This started last night when I was researching some interesting things to do with Salesforce DX and more specifically my bash shell.

I came accros [this](http://www.wadewegner.com/2017/04/show-the-salesforce-dx-org-config-in-your-bash-profile/) which with a bit of playing around I was able to get working.

Below is what it ended up looking like

![custom bash file](/images/bash-shell-2019-03.png)

Pretty cool huh?

Some things to note.
- I'm on windows and using git bash, so I had to manually install jq and make sure it was on my path
- I customized the shell further to add my working directory
- I changed some of the colouring from the original to suit myself better
- Below is what my `.bash_profile` ended up looking like (also I think this really should be `.bashrc`)

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
  echo $echoString"\n\e[0m"
}

print_before_the_prompt () {
    printf "$(get_usernames)"
}

PROMPT_COMMAND=print_before_the_prompt
PS1='\w\n$ '

```

I'm still not quite happy with it but I'll try to keep her updated with progress.