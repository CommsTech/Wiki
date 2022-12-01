---
title: HeavyScript
description: Modular script to manage truenas scale docker/K8s
published: true
date: 2022-08-30T04:38:46.749Z
tags: Server, Media Management, Script, Truenas
editor: markdown
dateCreated: 2022-08-30T04:27:17.152Z
---
# HeavyScript

HeavyScript is a script created by Heavy, with the idea of being modular, it includes many options and cool features.

[github Repo](https://github.com/Heavybullets8/heavy_script)


### Why This Script?

Some other scripts are derived from this one, a lot of my logic, a lot of my regex etc.

So I do not see the point in using someone else's script when you can get the latest and greatest straight from the source

> However, it would be unfair to say that I came up with this all on my own, I did not, some of the ideas were taken from other projects, I just would like to _think_ I implemented them better, but I guess that's up to you to decide

#### Focuses

- Stability
> Everything is tested first on my Virtual Machine(running nightlies), then tested on my main server, and then finally after a while it is pushed to main, for everyone.

- Modularity
> It is your machine, I wont force you to do any option you might not want to
>> Self script updates are optional, Rolling back applications is optional, etc


## The Menu

The menu is a nice shortcut to any HeavyScript utilities you could need


![!Menu: Overview](images/menu_overview.png)

You can access the menu simply by running 

```
bash heavyscript.sh
```

or

```
bash /path/to/script/heavyscript.sh
```

<br >

All of the options do just what you would expect

1. Opens the help menu, so you can see all of the options for the script
2. Lists all of your internal DNS names and their ports
3. Opens the Mount and Unmount feature, so you can mount your volumes
4. Creates a `ix-applications` backup
5. Opens the menu to restore your `ix-applications` dataset to a previous version
6. Opens the menu to delete an `ix-applications` backup
7. Updates HeavyScript to the latest github version
8. Opens a menu to create, then run a HeavyScript update


<br >
<br >

## Listing DNS Names

This is useful for when you cannot figure out the internal DNS, and port that you need to use when linking applications together

![!DNS: Overview](images/dns.png)

> This is what it looks like, as you can see it shows the DNS name, followed by the port

<br >

__Get to this by either using __

```
bash heavy_script.sh --dns
```

or by using the menu

```
bash heavy_script.sh 
```

<br >
<br >

## Mounting PVC

Finally, an easy to use mounting feature, so you don't have to do that long process yourself

If you don't know, lots of applications use containerized storage, which is unaccessible from your filesystem on your server UNTIL you mount the containers volume. The way to do that manually is long, and a pain, so I created a function within HeavyScript to do it all for you

<br >

The first menu appears like this:

![!Mount Overview](images/mount1.png)
> If you choose `1`, it will list all of your applications, and their volumes as shown below

> If you choose `2`, it will unmount ALL of the volumes that were mounted with the script

<br >

And the second menu:

![!Mount Overview](images/mount2.png)
> Type the number associated with the volume you wish to mount, it will:
>> 1. Shutdown that application
>> 2. Mount it under `/mnt/heavyscript/`

<br >

To unmount, simply open the menu again and choose option 2 to unmount everything
> You NEED to do this before attempting to start the application

__Get to this by either using __

```
bash heavy_script.sh --mount
```

or by using the menu

```
bash heavy_script.sh 
```


<br >
<br >


## Restoring Backup

This is useful for when you absolutely destroyed all of your applications, or messed up your kubernetes cluster

![!Restore: Overview](images/restore.png)
> Lists out each backup from newest to oldest

> After selecting a backup by typing the number to the left of it and pressing enter, HeavyScript will prompt you to confirm your decision before actually restoring that backup

This process takes a long time, so just be patient


__Get to this by either using __

```
bash heavy_script.sh --restore
```

or by using the menu

```
bash heavy_script.sh 
```

<br >
<br >

## Deleting Backup

This is useful for when you need to delete backups one by one, from other scripts, or if you simply just have too many backups


![!Delete: Overview](images/delete.png)

> Lists out each backup from newest to oldest

> After selecting a backup by typing the number to the left of it and pressing enter, HeavyScript will prompt you to confirm your decision before actually deleting that backup

__Get to this by either using __

```
bash heavy_script.sh --delete-backup
```

or by using the menu

```
bash heavy_script.sh 
```

<br >
<br >

## Updating Applications

This is something I am very proud of, it allows you to create a `bash heavy_script.sh` update with interactive options

<br >

The first Menu:

![!Update: Overview](images/update1.png)
> Type the number associated with the option you want

<br >

It will then ask how many applications you would like to update at once 

![!Update: Overview](images/update2.png)
> Type a number greater than 0

<br >

Finally, you can make your selections

![!Update: Overview](images/update3.png)
> After you make a selection, it will appear under `Current Choices`

> After you finish up, you can type `00` and the update with your options will begin!

<br >

__Get to this by__

Using the menu

```
bash heavy_script.sh 
```


#### Flag Table


| Flag 	| Example                	| Parameter       	| Description                                                                                                                                                                                                                	|
|------	|------------------------	|-----------------	|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------	|
| -U   	| -U <br>-U 5            	| None or Integer 	| Update applications, ignoring major version changes<br>_Optionally, you can supply a number after the argument to update multiple applications at once_                                                                    	|
| -u   	| -u<br>-u 5             	| None or Integer 	| Update applications, do NOT update if there was a major version change<br>_Optionally, you can supply a number after the argument to update multiple applications at once_                                                 	|
| -b   	| -b 14                  	| Integer         	| Backup `ix-appliactions` dataset<br>_Creates backups up to the number you've chosen_                                                                                                                                       	|
| -i   	| -i nextcloud -i sonarr 	| String          	| Applications listed will be ignored during updating<br>_List one application after another as shown in the example_                                                                                                        	|
| -r   	| -r                     	| None            	| Monitors applications after they update<br>If the app does not become "ACTIVE" after either:<br>The custom Timeout, or Default Timeout,<br>rollback the application.                                                       	|
| -v   	| -v                     	| None            	| Verbose Output<br>_Look at the bottom of this page for an example_                                                                                                                                                         	|
| -S   	| -S                     	| None            	| Shutdown the application prior to updating it                                                                                                                                                                              	|
| -t   	| -t 150                 	| Integer         	| Set a custom timeout to be used with either:<br>`-m` <br>_Time the script will wait for application to be "STOPPED"_<br>or<br>`-(u\|U)` <br>_Time the script will wait for application to be either "STOPPED" or "ACTIVE"_ 	|
| -s   	| -s                     	| None            	| Sync Catalogs prior to updating                                                                                                                                                                                            	|
| -p   	| -p                     	| None            	| Prune old/unused docker images                                                                                                                                                                                             	|

<br >
<br >

## Further Explanation

### Update Options

There are two options for updating

```
-U
```

This option does not care about major version changes, and will update even if there is one
> A major version change example:
>> `3.14.2` ---> `4.14.2` 

<br >

```
-u
```

This option will NOT update if there was a major version change
> `3.14.2` ---> `4.14.2` 
>> In this case, this application would NOT update, this is useful because a lot of the time, major version changes include breaking changes

<br >

#### Asynchronous Updates

Both of the update options have the ability to update multiple applications at one time

You can do this by placing a number after the `-u` or `-U`

Example:

```
-U 5
```

or

```
-u 5
```

- This will keep __up to __ 5 applications updating at one time
> You can place any whole number after the flag, except for 0

<br >

If you wish to NOT have multiple applications updating at once, simply use:

```
-u 1
```

or

```
-u
```
> If you do not place a number after `-u`, it will default to 1

<br >
<br >

### Backing Up

This does NOT backup individual applications (this is already taken care of when updating your app, a snapshot is taken, this is what allows you to rollback applications)

Instead, the script will back up your entire `ix-applications` dataset

> I will talk more about why this is useful in another section

<br >

You can backup your dataset prior to updating by:

```
-b 14
```
> You can use any whole number after `-b` besides 0

The number you choose is the __MAXIMUM__ number of `HeavyScript` backups that will be on your system
> It is important to not have this number be so high, if you have too many backups your system will slow down

<br >

Any number of backups that EXCEEDS the number you choose will be deleted
> If you started with 18 backups, and run `-b 14`, the 5 oldest backups will be deleted, because they exceed 14

<br >
<br >

### Ignoring

If you have an application that seems to break after every update, add it to the ignore list

Adding an application to the ignore list will completely ignore it when it comes to updating, neither `-u` or `-U` will update that application
> Just make sure you spell it right, capitalization does not matter however. 

<br >

Ignored applications MUST be added one by one

Example:

```
-i sonarr -i radarr -i nextcloud
```
> As you can see I am spacing out each `-i` away from each other
>> Using something like `-i sonarr radarr nextcloud` will NOT work, each app needs its own flag

_Note: The name you type after `-i` is the exact name YOU gave the application when creating it_

<br >
<br >

### Rolling Back

This is by far one of my favorite features, especially since a lot of the updates that have been pushed lately seem to fail, this has saved me a ton of time

```
-r
```

Will rollback an application to its previous version if it does not become active after your timeout

Example:
> Nextcloud is going from `3.1.14` to `3.1.15`, it fails to come back up for whatever reason, it will be rolled back to `3.1.14`

<br >
<br >

### Verbose

Sounds just as it is, it will produce more of an output when updating apps

<br >
<br >

### Shutdown Before Update

This will shutdown the individual application before it sends it the update command

Some people think this is a safer way to update applications, so I have it as an option, I personally don't use it

```
-S
```

Example:
> Jackett has an update, it is `active`, before updating Jackett the script will send it the shutdown command and wait for it to be fully `stopped` before continuing with the update


<br >
<br >

### Timeout

This is used in two places in the script

1. How long HeavyScript waits for the application to be `Active` before rolling it back
> If you are using `-r` anyway

2. How long HeavyScript waits for the application to Shutdown before it times out

<br >

Example:
```
-t 400
```
> Must use a whole number

> The number is in seconds

> If unset, the script uses 500 by default

<br >
<br >

### Sync

This just syncs the catalog prior to updating
> Syncing the catalog will ensure the latest application versions are being pulled down from the repository


<br >
<br >


### Prune

Prunes Old/Unused docker images
> If this is not done regularly, you can waste a lot of space with old images


<br >

### Source Code
The source code is, and always will be free and open source

[Github Link](https://github.com/Heavybullets8/heavy_script)