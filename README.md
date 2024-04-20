# CrackMacOSScreenTime

​	This repository contains all the tool you need, if you follow this tutorial, to crack the macOS Screen Time.



## 0x00 Preparations

- A MacOS Machine (Mine is arm64 macOS Monterey 12.6)
- Navicat Premium or sth like that (SQLite editor)



## 0x01 Workflow

​	What we're doing is basically killing the `ScreenTimeAgent` process, modify the content, refresh the database, enter settings and then kill the agent again before it fetches backup.

​	First of all, we're going to prepare a script that kills the agent.

​	Secondly, we'll go through the database structure.

​	At last, some troubleshooting.

## 0x02 Preparing the script

​	--img.1--

​	First, launch Activity Monitor and search for "ScreenTimeAgent"

​	--img.2--

​	Then, in Terminal, execute:

​	(Environment: UNIX Shell launched on /bin/zsh)

```shell
killall -9 ScreenTimeAgent;
```

​	We can try this script in the terminal:

​	--img.3--

​	we can then search for "ScreenTimeAgent" again in Activity Monitor:

​	--img.4--

​	It's killed!

## 0x03 Preparing a service

​	We'll need to execute that script quickly, so we'll create a service.

​	--img.5--

​	In Automator: cmd+n and choose "Service", then click Choose

​	If you've used Automator before, set "no input", add a action for executing shell script, and enter the script, save. 

​	For those who haven't:

​	Find the service configuration (Workflow recieves xx in xx)

​	and set it to Workflow receives no input in any application:

 	--img.6--

​	Search for Run Shell Script in the sidebar

​	Drag the "Run Shell Script" item into the area that says "Drag actions or files here to build you workflow." and enter:

```shell
killall -9 ScreenTimeAgent;
```

​	--img.7--

​	cmd+s Save, save as "ShutDownST"

​	You can choose another name, just remember it

## 0x04 Binding a shortcut

​	Open System Preferences

​	Open Keyboard and switch to the "Shortcuts" tab

​	--img.8--

​	Select Services and scroll to find the service that we've just created (ShutDownST for me), 

Img 9

and click on "none" and "Add Shortcut"

type a shortcut:

img 10



## 0x05 Modifying the database

Finder > Go > Go to Folder... (cmd+g)

type in `/private/var/folders/28/fzrckrvx64734v6nq_drvc280000gn/0/com.apple.ScreenTimeAgent/Store/`

img 11

Enter

img 12

Open `RMAdminStore-Local.sqlite`

(I'm using Navicat Premium)

First double click the database's name "RMAdminStore-Local"

Then "tables"

Then find the row on the right that says "ZUSAGETIMEDITEM"

--img 13--

and it should look sth like that:

--img 14--

