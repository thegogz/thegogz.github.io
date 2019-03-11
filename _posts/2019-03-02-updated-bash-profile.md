---
layout: post
title:  "Updated SFDX Aware Bash Profile"
tags: [sfdx, bash, shell, git-bash, salesforce, git]
---

I knew there was something missing from the last version. So here's the latest
- Renamed some of the strings to make it clearer what they do
- Hide the scratch org when there is none
- Display the path and as I'm using git bash include the branch when I'm in a repo

Ends up looking like this

![Updated Bash Shell](/images/bash-shell-2019-03-updated.png)

And the updated `.bash_profile`

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

  echoString="$bldwht""Devhub: $bldgrn";
  if [ ! $defaultdevhubusername = "null" ]
  then
    echoString=$echoString$defaultdevhubusername"$txtylw (local)"
  else
    echoString=$echoString$globaldefaultdevhubusername"$txtylw (global)"
  fi
  echo "\n"$echoString

  echoString="$bldwht""Default Scratch Org: $bldgrn"
  if [ ! $defaultusername = "null" ]
  then
    echoString=$echoString$defaultusername"$txtylw (local)"
  elif [ "$globaldefaultusername" = "null" ]
  then
    echoString=""
  else
    echoString=$echoString$globaldefaultusername"$txtylw (global)"
  fi
  echo $echoString
}

print_before_the_prompt () {
    printf "$(get_usernames)"
}

PROMPT_COMMAND=print_before_the_prompt
PS1='\n\[\033[1;33m\]\w\[\033[36m\]`__git_ps1`\[\033[0m\]\n$ '

```
