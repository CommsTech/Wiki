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