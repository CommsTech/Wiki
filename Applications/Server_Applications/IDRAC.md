---
title: IDRAC
description: 
published: true
date: 2022-07-25T13:15:54.706Z
tags: 
editor: markdown
dateCreated: 2022-05-21T15:28:21.146Z
aliases:
---
# [iDrac Dell Server (Off-Band Remote Access)](https://christitus.com/idrac-dell-server/ "See on original website")

[![✇](https://freshrss.commsnet.org/f.php?c590a1d4)Hardware on Chris Titus Tech | Tech Content Creator](https://freshrss.commsnet.org/i/?get=f_6 "Filter") 

March 24th 2015 at 2:43 pm

I recently had a server freeze up when I wasn’t on-site and needed to power cycle it. I had configured iDrac before but it was not responsive. Here are some useful commands that can configure, reboot, and reset the iDrac function on Dell Servers. The commands below are run from a command prompt on the server (unless otherwise stated) in the (C:\Program Files\Dell\SysMgt\idrac) path.

![iDrac](https://christitus.com/images/2015/03/idrac.png)

**Remote Commands (Workstation or another server)**

`racadm -r -u -p`

Example: `racadm -r 10.1.1.1 -u root -p calvin getsysinfo`

DEFAULT USER: root

DEFAULT PASS: calvin

**iDrac Information**

`racadm getsysinfo`

**Reset and Factory Defaults**

`racadm racreset` (soft Reset)

`racadm racresetcfg` (Hard Reset – Resets IP/Account settings back to factory default)

NOTE: Resets Default IP > 192.168.0.120

**Network Setup**

```
racadm config -g cfgLanNetworking -o cfgNicEnable 1
racadm config -g cfgLanNetworking -o cfgNicIpAddress 192.168.0.120
racadm config -g cfgLanNetworking -o cfgNicNetmask 255.255.255.0
racadm config -g cfgLanNetworking -o cfgNicGateway 192.168.0.120
racadm config -g cfgLanNetworking -o cfgNicUseDHCP 0
racadm config -g cfgLanNetworking -o cfgDNSServersFromDHCP 0
racadm config -g cfgLanNetworking -o cfgDNSServer1 192.168.0.5
racadm config -g cfgLanNetworking -o cfgDNSServer2 192.168.0.6
```

**User Setup**

```
racadm config -g cfgUserAdmin -o cfgUserAdminUserName -i 2 john
racadm config -g cfgUserAdmin -o cfgUserAdminPassword -i 2 123456
racadm config -g cfgUserAdmin -o cfgUserAdminEnable -i 2 1
```

To verify the new user, use one of the following commands:

```
racadm getconfig -u john
racadm getconfig –g cfgUserAdmin –i 2
```

From here you will use your web browser to enter the iDrac configuration GUI and finish setting up SMTP servers, alerts and other things. I only recommend the above from command line since it gives direct access when the web browser isn’t functions due to bad IP address or invalid username.