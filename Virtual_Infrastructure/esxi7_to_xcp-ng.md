---
title: ESXI to XCP-ng importer
description: 
dateCreated: 2023-31-12T04:33:51.250Z
published: 
editor: markdown
tags:
  - Virtualization
  - Hypervisor
  - Data_Management
dateModified: 
---
# HOW-TO: CONVERT ESXI 7 VM TO OVA FOR XCP-NG (XEN ORCHESTRA)

![How-To: Convert ESXI 7 VM to OVA for XCP-NG (XEN Orchestra)](https://jarrodstech.net/wp-content/uploads/2022/10/doneloading-1280x640.jpg)

Recently I’ve been trying XCP-NG and XEN Orchestra alongside ESXI. This has meant I have needed to move some of my VM’s to across to XEN. The process of moving from ESXI without vSphere was a little convoluted. Luckily, it’s not too hard once you know the steps.

## INSTALL VMWARE WORKSTATION

We need VMware workstation just for the OVF to OVA conversion tool that it provides. We won’t actually use the software. Download and install it from the VMware website. [https://www.vmware.com/au/products/workstation-pro/workstation-pro-evaluation.html](https://www.vmware.com/au/products/workstation-pro/workstation-pro-evaluation.html)

## EXPORT THE CURRENT VM FROM ESXI

1. Log into ESXI and shut down the VM![](https://jarrodstech.net/wp-content/uploads/2022/10/shutdown-1024x506.jpg)
2. Select the “Actions” menu. Then select “Export”![](https://jarrodstech.net/wp-content/uploads/2022/10/export-1024x545.jpg)
3. Ensure you select the .nvram option also![](https://jarrodstech.net/wp-content/uploads/2022/10/Screenshot-2022-10-01-123641.jpg)
4. Now in downloads you should have the files downloaded. (Ensure that if pop ups were blocked that you enable and redownload the missing files.)![](https://jarrodstech.net/wp-content/uploads/2022/10/doanloads.jpg)

## CONVERT THE FILES TO AN OVA

Now we will convert the 4 files into a single OVA file that XEN can import

1. Open CMD and go to the VMware tools folder – cd C:\Program Files (x86)\VMware\VMware Workstation\OVFTool![](https://jarrodstech.net/wp-content/uploads/2022/10/downloadsfolder-1024x270.jpg)
2. Now we will run the conversion tool. Make sure you replace the username with your username and the ovf file name with the name of your VM.
3. ovftool.exe C:\Users\username\Downloads\XenOrchestra.ovf C:\Users\username\Downloads\XenOrchestra.ova ![](https://jarrodstech.net/wp-content/uploads/2022/10/processing-1024x577.jpg)
4. It will show a few errors at the start but just ignore them and let the conversion finish
5. You should receive Transfer Completed and Completed successfully. There will be a few warnings.![](https://jarrodstech.net/wp-content/uploads/2022/10/done-1024x575.jpg)

## IMPORT INTO XEN ORCHESTRA

1. Log into XEN Orchestra and select Import > VM![](https://jarrodstech.net/wp-content/uploads/2022/10/xenimport.jpg)
2. Select the Pool and Storage location. Then upload the OVA file we created earlier![](https://jarrodstech.net/wp-content/uploads/2022/10/import-1024x387.jpg)
3. After uploading the OVA you can change the name, ram and network settings.
4. Select Import![](https://jarrodstech.net/wp-content/uploads/2022/10/importfinal-1024x518.jpg)
5. When done you will be presented with a VM screen![](https://jarrodstech.net/wp-content/uploads/2022/10/doneimport-1024x316.jpg)
6. Select start from the top right
7. Select console to watch the start up![](https://jarrodstech.net/wp-content/uploads/2022/10/doneloading-1024x376.jpg)

## IMPORTANT NOTE – NETWORKING

This process will break the networking on Ubuntu/ Debian. You will have to re-setup your network interface.

In Debian you can usually change the /etc/networks/interfaces file to match the new eth0 interface

Ubuntu usually uses the /etc/netplan folder. There will be a network config file there, where you can change the interface to eth0.