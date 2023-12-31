---
title: vcenter
description: VMwares Virtual Center
published: true
date: 2022-07-18T02:41:39.777Z
tags:
  - networking
  - Virtual
  - Machine
  - Server
editor: markdown
dateCreated: 2022-07-18T02:41:39.777Z
aliases:
---
# Vcenter 6.7


#### Error # 503 Service Unavailable (Failed to connect to endpoint: [N7Vmacore4Http20NamedPipeServiceSpecE:0x00007f2e3401aa80] _serverNamespace = / action = Allow _pipeName =/var/run/vmware/vpxd-webserver-pipe)

 #### Solution:

1.  SSH into the vCSA and login as root and execute the **“shell”** command to get shell access.
2.  Run the **“df -h”** command and verify the **“/storage/archive”** mount is at 90+% of use.
3.  Access the vCenter Server that the vCSA instance is running on and increase Disk 13 of the vCSA VM Hardware by a significant amount.
4.  In the SSH session to the vCSA, run the autogrow script **“/usr/lib/applmgmt/support/scripts/autogrow.sh”** .
5.  Run the **“df -h”** command and verify the **“/storage/archive”** mount Use percentage has decreased.
6.  After some time has passed, verify the vSphere Client System Configuration Node Health is **“Good”** .
7.  Verify the vCenter Server Appliance VAMI Health Status for Storage is **“Good”** .

**You can also try the following Shell script**
```
#!/bin/bash
cd /etc/ssl/certs
mkdir /tmp/pems
mkdir /tmp/OLD-CRLS-CAs
mv *.pem /tmp/pems && mv *.* /tmp/OLD-CRLS-CAs
h=$(/usr/lib/vmware-vmafd/bin/vecs-cli entry list --store TRUSTED_ROOT_CRLS --text | grep Alias | cut -d : -f 2)
for hh in "echo "${h[@]}"";do echo "Y" | /usr/lib/vmware-vmafd/bin/vecs-cli entry delete --store TRUSTED_ROOT_CRLS --alias $hh;done
mv /tmp/pems/* .
for l in `ls *.pem`;do ln -s $l ${l/pem/0};done
service-control --stop vmafdd && service-control --start vmafdd
```

