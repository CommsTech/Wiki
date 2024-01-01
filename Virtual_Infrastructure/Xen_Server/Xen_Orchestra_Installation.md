---
title: Xen Orchestra Installation
description: Xen Orchestra Installation procedures
dateCreated: 2022-08-30T04:27:17.152Z
published: true
editor: markdown
tags: XEN, Virtual Management
dateModified: 
---
# Xen_Orchestra_Installation
## Chris Titus Tech has an amazing walkthrough with a video. Check it out @ [How to Install Xen Orchestra](https://christitus.com/how-to-install-xen-orchestra/ "See on original website")


# Step-by-Step Guide

1.  SSH into XenServer 

2.  Type `bash -c "$(curl -s http://xoa.io/deploy)"`
    -  A Prompt will appear
    -  Enter the IP you wish to use for you xen orchestra server
3.  SSH into the Xen Orchestra Server
    -   login with username and password of `xoa`
    -   set new password for console
    -   register xoa from username and password that you used on their website [https://xen-orchestra.com/](https://xen-orchestra.com/)
        -   `sudo xoa-updater --register`
    -   check for updates
        -   `sudo xoa-updater &#8211;upgrade`
    -   reboot if any updates/upgrades were performed
4.  Open a browser and login to Xen Orchestra website
    -   login username `admin@admin.net` password `admin`
5.  Add your XenServers to the Xen Orchestra Instance 

 *-note if you are using default self-signed certificates on XenServer enable “unauthorized certificates” and then click disconnected*
