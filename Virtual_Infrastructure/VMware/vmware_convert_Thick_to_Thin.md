# OW TO CONVERT THICK PROVISION LAZY ZEROED DISK TO THIN ON VMWARE ESXI?

written by [Cyril Kardashevsky](https://theitbros.com/author/administrator/) May 18, 2023

The reason why the Administrator can start to look for a solution on disk conversion from Thick to Thin on VMware ESXi is that thick virtual disks are taking up more storage space than they need. By default, the type of the newly created virtual disk on VMWare ESXi is **Thick Provision Lazy Zeroed** format, adversely affecting the system’s future performance. It’s better to use Thin Provision or Thick Provision Eager Zeroed (in this case, the entire disk space is filled with zeros, slowing the process of creating a disk but increasing the speed of work in the future).

Table of Contents

- [Types of Virtual Disk in VMware vSphere ESXi](https://theitbros.com/convert-thick-provision-lazy-zeroed-disk-to-thin-vmware-esxi/#penci-Types-of-Virtual-Disk-in-VMware-vSphere-ESXi "Types of Virtual Disk in VMware vSphere ESXi")
    - [Thick Provision Lazy Zeroed](https://theitbros.com/convert-thick-provision-lazy-zeroed-disk-to-thin-vmware-esxi/#penci-Thick-Provision-Lazy-Zeroed "Thick Provision Lazy Zeroed")
    - [Thick Provision Eager Zeroed](https://theitbros.com/convert-thick-provision-lazy-zeroed-disk-to-thin-vmware-esxi/#penci-Thick-Provision-Eager-Zeroed "Thick Provision Eager Zeroed")
    - [Thin Provision](https://theitbros.com/convert-thick-provision-lazy-zeroed-disk-to-thin-vmware-esxi/#penci-Thin-Provision "Thin Provision")
- [Changing the Thick to Thin Provisioning of a Virtual Disk Using Storage VMotion](https://theitbros.com/convert-thick-provision-lazy-zeroed-disk-to-thin-vmware-esxi/#penci-Changing-the-Thick-to-Thin-Provisioning-of-a-Virtual-Disk-Using-Storage-VMotion "Changing the Thick to Thin Provisioning of a Virtual Disk Using Storage VMotion")
- [How to Change Disk Provisioning Type using ESXI CLI?](https://theitbros.com/convert-thick-provision-lazy-zeroed-disk-to-thin-vmware-esxi/#penci-How-to-Change-Disk-Provisioning-Type-using-ESXI-CLI "How to Change Disk Provisioning Type using ESXI CLI?")
- [Converting Thick to Thin Disk Using VMware Converter](https://theitbros.com/convert-thick-provision-lazy-zeroed-disk-to-thin-vmware-esxi/#penci-Converting-Thick-to-Thin-Disk-Using-VMware-Converter "Converting Thick to Thin Disk Using VMware Converter")
- [FAQs Section:](https://theitbros.com/convert-thick-provision-lazy-zeroed-disk-to-thin-vmware-esxi/#penci-FAQs-Section "FAQs Section:")
- [Wrapping Up](https://theitbros.com/convert-thick-provision-lazy-zeroed-disk-to-thin-vmware-esxi/#penci-Wrapping-Up "Wrapping Up")

## Types of Virtual Disk in VMware vSphere ESXi

VMWare uses three types of disks that allow you to choose the disk format option of the resulting hard disk:

1. Thick Provision Lazy Zeroed
2. Thick Provision Eager Zeroed
3. Thin Provision

### Thick Provision Lazy Zeroed

With a thick provisioned virtual disk that is lazy zeroed disks, all space of the virtual disk is allocated upon creation, while the blocks are not cleared of the data that was there earlier. When the virtual machine first accesses a new block, it is cleared.

With a thick provisioned disk configured as lazy zeroed, the first time a block is accessed on such a disk, I/O performance for the clear operation is lost. The VM then accessed such a data block next time, the performance is identical to Eager zeroed Thick. This type of disk with thick provisioning is created for virtual machines that are using the VMware vSphere client by default.

### Thick Provision Eager Zeroed

With thick provision eager zeroed disks, all space of such disk is allocated at the time of creation, and all storage blocks are cleared of the data that was there earlier. The advantage of such disk is performance and security, the disadvantage is the long creation time;

### Thin Provision

With a thin provisioned virtual disk, these are initially created with minimum size and grow as they fill up with data to the specified vmdk file size. When a new block is filled up with the data, it is cleared beforehand. Such disks are the least productive (need some time for allocating and cleaning a new block), but the most optimal in terms of saving space on the storage. Thin provisioned disks allow storing more virtual machines in the same VMFS datastore.

Note that the virtual machine will stop working if the disk space on the datastore is exhausted and the thin disk cannot be expanded.

> **_Hint_**_. Using PowerCLI, you can find out used and provisioned disk size for a specific virtual machine:_
> 
> Get-VM -Name VMName1 | Select Name,@{N=’Used (GB)’;E={[math]::Round($_.UsedSpaceGB,2)}},@{N=’Provisioned (GB)’;E={[math]::Round($_.ProvisionedSpaceGB,2)}} | Format-List

There are three different ways for converting Thick to Thin Provision Disk:

- Using Storage vMotion feature of a VMware vSphere;
- Using CLI (the only one way to convert disk on a standalone VMware ESXi host);
- Using VMWare Converter.

> **_Note_**_. Check [how to convert VMDK to VHDX (VMWare to Hyper-V)](https://theitbros.com/how-to-convert-vmdk-to-vhdx/)._

## Changing the Thick to Thin Provisioning of a Virtual Disk Using Storage VMotion

Using the **Storage vMotion** feature of a VMware vSphere, you can relocate any virtual machine with a disk to a different VMFS datastore (different LUN) without interruption of services and applications in a running VM. During a migration of VM disks between storages, you can also change the type of the disk.

To get the current disk provisioning type for the specific VM, you have to open its settings in vSphere Client and check the value of the disk type field. In our case, the type of virtual disk is **Thick provision lazy zeroed**.

![esxi convert thick to thin](https://theitbros.com/wp-content/uploads/2020/11/convert-thick-to-thin-vmware.png "esxi convert thick to thin")

To change the disk type, you should move VM files to another datastore.

> **_Tip_**_. If you have only 1 VMFS datastore, you can create a temporary additional NFS file share on any Windows server and_ [_connect it to the ESXi server via NFS_](https://theitbros.com/mount-windows-folder-into-vmware-esxi/)_._

Open the Virtual Machine action menu and select **Migrate**.

![convert thick to thin vmware](https://theitbros.com/wp-content/uploads/2020/11/thick-provision-lazy-zeroed-to-thin.png "convert thick to thin vmware")

Select the migration type **Change storage only**.

![thick provision lazy zeroed](https://theitbros.com/wp-content/uploads/2020/11/vmware-convert-thick-to-thin.png "thick provision lazy zeroed")

Now in the **Select virtual disk format** field, choose which format you want to convert the vmdk during Storage vMotion. Select **Thin Provision** as a virtual machine disk format and choose one of the available VMFS/NFS datastore on which you want to move the VM files. Click **Next**.

![vmware convert thick to thin](https://theitbros.com/wp-content/uploads/2020/11/esxi-convert-thick-to-thin.png "vmware convert thick to thin")

The new **Relocate virtual machine** task will appear in the vSphere **Resent task** pane. Wait until the virtual machine files will be moved to the new storage under the configured VM folder, including the virtual machine base disk and any snapshots.

> **Referer**. Check our article about [sharing a disk between multiple VMs](https://theitbros.com/share-disk-between-vms-on-vmware-esxi/) on ESXi.

![esxi thick to thin provisioning](https://theitbros.com/wp-content/uploads/2020/11/vmware-thick-to-thin.png "esxi thick to thin provisioning")

Now make sure that the type of the disc is changed to **Thin provision**.

> **_Tip_**_. When migrating a virtual machine to any storage other than the VMFS file system, virtual disks are always converted to_ **_Thick_** _format and will take up their maximum size._

## How to Change Disk Provisioning Type using ESXI CLI?

The way of changing VMWare virtual disk provisioning type using CLI involves copying the current vmdk file to a new thin-provisioned disk, and removing the old thick disk.

At first, you should power off your VM. To connect to the ESXi host CLI over SSH, you have to enable SSH service in the ESXi settings and allow incoming connection on TCP/22 port (open on the ESXi host: **Configuration** > **Services**). Select the **SSH** service and press **Start**.

![convert thick to thin vmware esxi](https://theitbros.com/wp-content/uploads/2020/11/change-thick-to-thin-provisioning-vmware.png "convert thick to thin vmware esxi")

Connect to ESXi server as root using a favorite SSH client (for example, Putty), and change directory to the vmfs/volumes:

# cd /vmfs/volumes

List the available datastores:

# ls –lh

And go to the folder, containing your VM files.

![how to change thick to thin provisioning vmware](https://theitbros.com/wp-content/uploads/2020/11/vmkfstools-thick-to-thin.jpeg "how to change thick to thin provisioning vmware")

List the files containing in the directory:

# ls

We are interested in the file w10rtm-test.vmdk. To convert this file from Thick to Thin, run the following command:

# vmkfstools -i w10rtm-test.vmdk -d thin w10rtm-test-thin.vmdk

> **_Note_**_. Before starting disk conversion, make sure you have enough space to convert the virtual machine’s disk from thin to thick._

![vmware convert thick to thin without vcenter](https://theitbros.com/wp-content/uploads/2020/11/word-image-14449-8.png "vmware convert thick to thin without vcenter")

> **_Hint_**_. Several examples of creating disks of a certain type from ESXi cli using the built-in vmkfstools tool:_
> 
> # vmkfstools –c 40G –d thick thick.vmdk
> 
> # vmkfstools –c 40G –d zeroedthick zeroedthick.vmdk
> 
> # vmkfstools –c 40G –d eagerzeroedthick eagerzeroedthick.vmdk

After conversion completes, delete the original Thick disk file w10rtm-test.vmdk:

# rm w10rtm-test.vmdk

And rename the resulting Thin disk:

# mv w10rtm-test-thin.vmdk w10rtm-test.vmdk

Now using vSphere Client or vSphere Web client, open the VM settings and delete the old disk (Select Remove from virtual machine). After that add new disk: Add new device > Existing Hard Disk > Use an existing virtual disk > select file w10rtm-test.vmdk on your datastore > Next > Ok.

![thick provision lazy zeroed to thin](https://theitbros.com/wp-content/uploads/2020/11/vmware-change-thick-to-thin.png "thick provision lazy zeroed to thin")

Now, you can power on your VM.

## Converting Thick to Thin Disk Using VMware Converter

**VMware Converter Standalone** is a free application that can be installed on Windows to convert physical machines (P2V) or virtual machines (V2V) to VMWare format.

Using VMware Converter, you can convert a VM to a new VM on the same ESXi host, but with a new disk format (you can even reduce the provisioned size of disks, which is not supported in vSphere Client).

> **_Note_**_. Check [how to install Windows on VMWare ESXi](https://theitbros.com/install-windows-10-on-vmware-esxi-6-0/)._

1. Download and install VMware Converter Standalone on any Windows computer. Run the conversion wizard;
2. Specify that vSphere Virtual Machine needs to be converted;
3. Specify vCenter/ESXi hostname and credentials to access it;
4. At the Source Data step, you can specify the new size of the virtual disks and the disk provisioning type. Choose **Thin**;
5. The converter will create a new VM with disks of the required size. Copy the data and reduce the size of the guest OS file system partition.

![convert vmdk to thin provision](https://theitbros.com/wp-content/uploads/2020/11/convert-thick-provisioned-to-thin-vmware.png "convert vmdk to thin provision")

## FAQs Section:

1. **What are the different types of virtual disks that VMWare uses?** VMWare uses three types of disks: Thick Provision Lazy Zeroed, Thick Provision Eager Zeroed, and Thin Provision.
2. **What’s the difference between Thick Provision Lazy Zeroed, Thick Provision Eager Zeroed, and Thin Provision?** The Thick Provision Lazy Zeroed format allocates all virtual disk space upon creation, but the blocks aren’t cleared of earlier data. Thick Provision Eager Zeroed also allocates all space at creation but clears all storage blocks of the data that was there earlier. Thin Provision, on the other hand, starts with minimum size and grows as they fill up with data.
3. **Why would you want to convert a Thick to Thin Provision Disk?** Thick virtual disks take up more storage space than necessary. By converting Thick to Thin, you optimize the use of storage space.
4. **What methods can be used for converting Thick to Thin Provision Disk?** There are three methods: using the Storage vMotion feature of a VMware vSphere, using CLI, or using VMWare Converter.
5. **What considerations should be made when converting disks?** Ensure that you have enough space to convert the disk from thin to thick. Also, be aware that the virtual machine will stop working if the disk space on the datastore is exhausted and the thin disk cannot be expanded.
6. **How can I check the current disk provisioning type for a specific VM?** You can open its settings in vSphere Client and check the value of the disk type field.
7. **What is the advantage of using VMware Converter for disk conversion?** Using VMware Converter, you can convert a VM to a new VM on the same ESXi host, but with a new disk format. It also allows you to reduce the provisioned size of disks, which is not supported in vSphere Client.

## Wrapping Up

The ability to convert Thick Provision Lazy Zeroed disks to Thin Provisioned ones on VMware ESXi offers an effective way of optimizing your storage utilization. Using either the Storage vMotion feature of VMware vSphere, CLI or VMware Converter, you can change the provisioning of a virtual disk, thus maximizing the performance of your system in the future.

Remember, however, that as with any technical process, each method comes with its own considerations. When using the CLI, for example, ensure you have sufficient space for the conversion process and be aware of the impact on your VM if disk space on the datastore is exhausted.

Additionally, remember that when migrating a virtual machine to any storage other than the VMFS file system, virtual disks will always be converted to Thick format and will take up their maximum size.

With a clear understanding of these disk provisioning types and conversion methods, you’ll be able to efficiently use provisioned space and support the smooth functioning of your VMware ESXi environment.