---
title: Nextcloud
description: Nextcloud information
published: true
date: 2022-08-30T04:38:46.749Z
tags: Server, Media Management, Storage, Collaberation
editor: markdown
dateCreated: 2022-08-30T04:27:17.152Z
---
# Nextcloud

[Source Website](https://nextcloud.com/)



Setup WEBDAV drive mapping

### Mapping drives with Windows Explorer[](https://docs.nextcloud.com/server/latest/user_manual/en/files/access_webdav.html#mapping-drives-with-windows-explorer "Permalink to this heading")

To map a drive using the Microsoft Windows Explorer:

1.  Open Windows Explorer on your MS Windows computer.
    
2.  Right-click on **Computer** entry and select **Map network drive…** from the drop-down menu.
    
3.  Choose a local network drive to which you want to map Nextcloud.
    
4.  Specify the address to your Nextcloud instance, followed by **/remote.php/dav/files/USERNAME/**.
    

> For example:
> 
> https://nextcloud.commsnet.org/remote.php/dav/files/YOURuserNAME/



### LDAP Integration in Nextcloud

|   |   |
|---|---|
||Configure LDAP Integration to NextCloud to login with LDAP user accounts.|
|[1]|[Install OpenLDAP Server, refer to here](https://www.server-world.info/en/note?os=Ubuntu_20.04&p=openldap&f=1).|
|[2]|Configure NextCloud to access to LDAP Server from PHP scripts.|

|   |
|---|
|root@dlp:~# <br><br>[apt](https://www.server-world.info/en/command/html/apt.html) -y install php-ldap<br><br>root@dlp:~# <br><br>[systemctl](https://www.server-world.info/en/command/html/systemctl.html) restart php7.4-fpm|

|   |   |
|---|---|
|[3]|Login to NextCloud Web with admin account and open [Apps].|

|   |
|---|
|![](https://www.server-world.info/en/Ubuntu_20.04/nextcloud/img/59.png)|

|   |   |
|---|---|
|[4]|Select [Your apps] on the left pane and Click [Enable] button on [LDAP user and group backend] section.|

|   |
|---|
|![](https://www.server-world.info/en/Ubuntu_20.04/nextcloud/img/60.png)|

|   |   |
|---|---|
|[5]|After enabling [LDAP user and group backend], open settings again and select [Administration] - [LDAP / AD integration] on the left pane.|

|   |
|---|
|![](https://www.server-world.info/en/Ubuntu_20.04/nextcloud/img/61.png)|

|   |   |
|---|---|
|[6]|Input LDAP server's information to connect.  <br>Like this example to use OpenLDAP on Ubuntu Server, it's OK to input server's hostname or IP addreess and Base DN only.  <br>Click [Test Base DN] button and if [Configuration OK] message is displayed like follows, that's OK, Click [Continue] to proceed.<br><br>For [User DN] and its [Password] fields, generally input a LDAP user account to bind LDAP server. However, OpenLDAP on Ubuntu server, anonymous bind is enabled by default as following setting, so it does not need to input them.  <br>⇒ olcAccess: {0}to attrs=userPassword by self write by anonymous auth by * none|

|   |
|---|
|![](https://www.server-world.info/en/Ubuntu_20.04/nextcloud/img/62.png)|

|   |   |
|---|---|
|[7]|Configure on [Users] tab.  <br>It's OK with default setting if using OpenLDAP on Ubuntu Server like this example.  <br>Confirm [Configuration OK] and Click [Continue] to proceed.|

|   |
|---|
|![](https://www.server-world.info/en/Ubuntu_20.04/nextcloud/img/63.png)|

|   |   |
|---|---|
|[8]|Configure on [Login Attributes] tab.  <br>It's OK with default setting if using OpenLDAP on Ubuntu Server like this example.  <br>Confirm [Configuration OK] and Click [Continue] to proceed.|

|   |
|---|
|![](https://www.server-world.info/en/Ubuntu_20.04/nextcloud/img/64.png)|

|   |   |
|---|---|
|[9]|Configure on [Groups] tab.  <br>If you'd like to limit groups they can search LDAP directory, configure here.  <br>But it's OK with default setting if using OpenLDAP on Ubuntu Server like this example.  <br>Confirm [Configuration OK] and finish configuration for admin account.|

|   |
|---|
|![](https://www.server-world.info/en/Ubuntu_20.04/nextcloud/img/65.png)|

|   |   |
|---|---|
|[10]|Move to Login form and specify a LDAP user.|

|   |
|---|
|![](https://www.server-world.info/en/Ubuntu_20.04/nextcloud/img/66.png)|

|   |   |
|---|---|
|[11]|If configuration OK, it's possbile to login to NextCloud with LDAP users like follows.|

|   |
|---|
||