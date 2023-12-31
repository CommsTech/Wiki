---
title: Jellyfin
description: Jellyfin Streaming
published: true
date: 2022-07-18T02:41:39.777Z
tags:
  - networking
  - Streaming
  - Media
  - Server
editor: markdown
dateCreated: 2022-07-18T02:41:39.777Z
aliases:
---
# Jellyfin



Mine  is hosted at https://jellyfin.commsnet.org


## Restoring Jellyfin admin for ubuntu
**note if your using the jellyfin truecharts container the process might require an external application known as [sqlitebrowser](https://sqlitebrowser.org/dl/) we will go through this process later
accidentally overwrote my admin user with authentik login... heres how to recover

### Step 1 - ssh into the jellyfin server

'''
sudo apt-get install sqlite3
'''

### Step 2 - Identify the users uid
'''
sudo sqlite3 /var/lib/jellyfin/data/jellyfin.db "select id from users where username='REPLACE';"
'''


### Step 3 - Make the user an administrator
**replacing REPLACE with the UUID you found in step 2:



## Restoring Admin for TrueCharts Image

### Step 1 - download [sqlitebrowser](https://sqlitebrowser.org/dl/), copy the jellyfin.db typically located at /data/jellyfin.db, and open it inside dbbrowser

### Step 2 - Identify the users uid

go to the execute SQL tab and execute
'''
select id from users where username='REPLACE'
'''
copy the ID


### Step 3 - Make the user an administrator
**replacing REPLACE with the UUID you found in step 2:

take the ID from step 2 and run the following command **replacing ID with the ID from step 2
'''
update permissions set value=1 where kind=0 and permission_permissions_guid='ID'
'''

### Step 4 - copy the modified db back into the truenas folder

### Step 5 - boot the jellyfin server
