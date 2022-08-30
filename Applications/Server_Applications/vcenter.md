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