---
title: Cisco IOS Upgrades
description: 
published: 
tags:
---
# Cisco_IOS_Upgrades

Title: Cisco IOS Upgrade: Accessing the Embedded Switch Module on 3945e & 2951 Routers

## Introduction

This document outlines the process of accessing and performing an IOS upgrade on the embedded 3560e Ethernet switch in a Cisco 3945e or 2951 router.

## Prerequisites

Before proceeding with the IOS upgrade, ensure you have the following:

1. Access to the router via PuTTY or any other terminal emulator software.
2. The latest version of Cisco IOS image for your switch.
3. A backup of your current switch configuration.

## Accessing the Embedded Switch Module

Follow these steps to access the embedded switch module:

1. Log into the 3945e router via PuTTY or any terminal emulator software using the following command:
   
```
username@your_computer:~$ ssh [username]@[router_IP_address]
```

2. Once logged in, enter the enable mode by typing:
   
```
router# enable
router#
```
   
3. Enter the enable secret password when prompted:
   
```
router# enable
Password:
router#
```
   
4. To access the embedded switch module, use the following command:
   
```
router# enable vlan 1
Switch#
```
   
5. You are now in the embedded switch's CLI. Perform the IOS upgrade as follows:

## Upgrading the Embedded Switch Module IOS

1. Verify the current IOS version by typing:
   
```
Switch# show version
```
   
2. Copy the new IOS image to the router using a secure file transfer protocol like SCP or TFTP. For example, using SCP:
   
```
Switch# copy tftp flash:
Address or name of remote host []? [192.168.x.x]:
Source filename []? [CiscoIOS_image_name]
Destination filename [CiscoIOS_image_name]? [Enter]
```
   
3. set the boot variable
```
boot system flash:
```

4. Reload the switch and validate operation
```
switch# reload
```

6. if the system is fully operational Delete the old IOS image:
   
```
Switch# dir flash:
Switch# del flash: CiscoIOS_image_name
```
   