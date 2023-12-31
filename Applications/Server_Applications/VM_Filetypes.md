---
title: Virtual Machine Formats
description: 
published: true
date: 2022-07-25T13:15:54.706Z
tags: 
editor: markdown
dateCreated: 2022-05-21T15:28:21.146Z
aliases:
---
# [Virtual Machine Formats](https://christitus.com/virtual-machine-formats/ "See on original website")

[![✇](https://freshrss.commsnet.org/f.php?e35a1391)Linux on Chris Titus Tech | Tech Content Creator](https://freshrss.commsnet.org/i/?get=f_7 "Filter") 

December 18th 2022 at 7:00 pm

This Guide goes over transferring and what each type of virtual machine is used for.

## The Types

### VDI - Virtualbox

The only time I’ve seen this format is in Virtualbox instances. They are always converted when moved to other hypervisors. The VDI packaging is open source and considered efficient.

### VMDK - VMWare

This is the market leader and what you will see the most in virtualization environments. Virtualbox and QEMU can run them without conversion.

### VHD - Microsoft

Leave it to Microsoft to make the worst container. VHD only holds a 2 TB capacity and why they created VHDX which improves this to 64 TB. Its not as performant as other containers, but gets the job done. The main benefit to these is they are read natively inside Windows without the need for extra software. You can mount and look at partition layouts with native windows tools like Disk Management.

### QCOW2 - QEMU

QEMU is by far my favorite hypervisor because of the versatility it gives me. QCOW2 is the file format you will use and it fantastic. There is a conversion tool that comes with QEMU that will easily convert any image. I’d recommend first converting the image to OVA as it is the most portable and versatile. Here is a sample script to convert to qcow2.

```
tar -xvf original.ova
qemu-img convert -O qcow2 original.vmdk original.qcow2
```

### OVF and OVA - The Best Way to Share VMs

This standard is widely adopted from VMWare, but you will be able to ingest these files into any hypervisor. VMWare, Virtualbox, and XenServer (XCP-ng) do this natively, where QEMU needs a little more conversion, but simple to use. Here is the [official website](https://docs.vmware.com/en/VMware-vSphere/7.0/com.vmware.vsphere.vm_admin.doc/GUID-AE61948B-C2EE-436E-BAFB-3C7209088552.html) right up on these files.

The TLDR is use OVA as it grabs all the information from the VMs (VMDK, OVF, and other information) and packages it all into one file for you. This is by far the best way to share a Virtual Machine.