---
layout: post
title:  "Creating Custom Bash Commands in Windows - Git Bash"
date:   2019-07-17 11:26:00 -0600
categories:
  - bash
  - windows
---

How to create custom commands in Git Bash.

There are two ways to customize commands in Git Bash. 

1. Alias:

   I use Git Bash as my primary terminal window at work on my Windows machine. It's super inefficient to have to type out long commands when you want to use them. For example: 

   ```bash
   $ py -m pip install -u package-name
   ```

   versus

   ```bash
   $ pip-install package-name
   ```

2. Custom Bash Script: 

   It is also inefficient if you find yourself running the same order of commands over and over to get a desired result from a workflow. For example I used to shut down local servers on different ports by running:

   ```bash
   $ netstat -ano | findstr :8000
   returns pid 1325
   $ tskill 1325
   ```

   To simplify I created a bash script (which I will share below), and now I can just run:

   ```bash
   $ kill-server 8000
   ```

   Saves a lot of time.

I continue to recognize long commands or repeated orders of multiple commands, that I use on a daily basis and could benefit from shortening to a single command, whether it be by alias or a new custom command script. 

Sections:

* [Create an Alias](#create-an-alias)
  * [Temporary Alias](#temporary-alias)
  * [Persistent Alias](#persistent-alias)
* [Creating a Custom Bash Script](#creating-a-custom-bash-script)
* [Wrapping Up](#wrapping-up)

## Create an Alias ##

#### Temporary Alias: ####

You can create temporary aliases in the git bash terminal window by typing a command similar to the following: 

*Note: Do not add any whitespace around "=" but do include a space between `alias` and the alias name*

```bash
$ alias new-alias-name="echo this is an alias"
```

You can then try running:

```bash
$ new-alias-name
this is an alias
```

However, if you exit out of the terminal and open it again or open a separate terminal window, the alias is gone and will not run without being reinitialized. 

#### Persistent Alias: ####

If you want your alias to stick around and be usable after your current session with the terminal do the following.

1. Open `~/.bashrc` in the editor of your choice. I like sublime. "`.bashrc` is a [shell script](http://en.wikipedia.org/wiki/Shell_script) that Bash runs whenever it is started interactively."- [Stackoverflow Thread](https://unix.stackexchange.com/a/129144). This is why `.bashrc` is a good place to stick aliases that will not disappear after each session.

   ```bash
   subl ~/.bashrc
   ```

2. Edit this file to add aliases. When the terminal is opened `.bashrc` will run and then these aliases will be available through each session.  There is a software tool installed on my machine called "GeoProcessor" and when developing this tool I need to start the software by manually running a script inside one of the directories in the project folder. This requires a lot of navigating to different directories and running all these separate commands. Below is an example of how to shorten it with an alias in my `.bashrc` file. 

   *Note: Do not add any whitespace around "=" but do include a space between `alias` and the alias name*

   ```bash
   alias start_geop='cd ~/owf-dev/GeoProcessor/git-repos/owf-app-geoprocessor-python/scripts/ && ./gpdev.bat --ui && cd'
   ```

   Now I can start GeoProcessor by simply typing `start_geop` which cuts back on a lot of navigating around my machines directories and running different commands.

3. Restart your terminal window and when reopening it you should be able to run your alias. If it is not running there may be an error in your file and make sure to pay attention the whitespace rule I mentioned above.



## Create a Custom Bash Script ##

If instead you find that you are running *multiple* commands over and over again, you may benefit from writing a script to do the work for you. 

1. Open Git Bash with administrator privileges (***Start*** > Search "Git Bash" > right click > ***Run as administrator***). Once Git Bash is running you should be able to navigate to the following folder: `cd '/c/Program Files/Git/usr/bin'`.  

2. Once inside this directory you can open a new file that should be titled as the command you intend to run. For example: I created the file "kill-server" and now I can run `$ kill-server` as a command. If you are not running in administrator then you will be prevented from saving any files in this location. Example:

   ```bash
   #kill-server
   #!/bin/bash
   
   RED='\e[31m'
   NOCOLOR='\033[0m'
   GREEN='\e[92m'
   
   echo -e "Getting PID's from port :$1..."
   PIDINFO=`netstat -ano | findstr :$1`
   
   if [ $# -eq 0 ]
     then
       echo -e "${RED}No port number specified"
       echo -e "${NOCOLOR}Usage: kill-server [port-number]"
       exit 1
   fi
   
   # Convert to array
   PIDINFOARR=($PIDINFO)
   
   # Get the PID
   PID=$(echo "${PIDINFOARR[4]}" | sed '')
   # Kill the server
   echo -e "Kill server :$1..."
   if tskill "${PID}"; then
   	echo -e "${GREEN}Server port /:$1 with PID: ${PID} succesfully terminated."
   else
   	echo -e "${RED}Server port /:$1 not succesfully terminated. It might be because server is not running."
   	exit 1
   fi
   ```

3. Restart Git Bash and when reopening it you should be able to now run your new bash script. Now I can just run `$ kill-server 8000`. 



## Wrapping Up ##

This post is short and simple but for me these two ways of creating custom commands have been very convenient and have helped tremendously in my efficiency at work and at home. I hope this helps do the same for someone in the future if you stumbled across this article.

