---
title: Extend LVM in Ubuntu
description: 
dateCreated: 
published: 
editor: markdown
tags: 
dateModified: 
---
# How to Extend the Default Ubuntu LVM Partition

[John W Kerns](https://packetpushers.net/author/john-w-kerns/)November 12, 2021

![](https://packetpushers.net/wp-content/uploads/2021/11/1663633_disc-player_hard-disk_hard-drive_hardware_storage-device_icon.png)

**POV:** You’re a sysadmin who set up a one-off Linux machine for an app you needed, and now it’s out of disk space.

You originally spun up a VM, installed a recent [[Ubuntu]] OS, and just hit _Next, Next, Finish_ through the guided install. Linux is not your bread and butter, you usually deal in Windows, and you just need to get this done.

**Approx Reading Time:** 10 minutes

> Note: In my examples, I’m using an “all-defaults” setup of **Ubuntu 20.04 Server** with a single **100GB disk**.

# Linux LVM Briefly Explained

If you followed the default settings in the Ubuntu installation, then the storage for your Linux OS is probably using the Logical Volume Manager (LVM). LVM is an abstraction framework which exists between your physical (or virtual) disks and your Linux file system (which is likely [ext4](https://en.wikipedia.org/wiki/Ext4)). It is used to group separate block devices (partitions) together into Volume Groups (VGs), and then chop those VGs up into logical block devices, or Logical Volumes (LVs). LVs are the abstracted block devices upon which your usable file system resides.

Below is a good visualization of how LVM works. In this example, we have five different disks, each with a single partition mapped to Physical Volumes (PVs), all being grouped into a single Volume Group (VG). The Volume Group is chopped up into two different Logical Volumes (LVs), and each LV is being used for a filesystem.

![Linux Ubuntu LVM diagram](https://packetpushers.net/wp-content/uploads/2021/11/1-linux-ubuntu-lvm-diagram.jpg)

Using a similar visualization, the below diagram shows how the Ubuntu installer (using all default options) divided up my 100GB disk.

![Linux Ubuntu installer defaults](https://packetpushers.net/wp-content/uploads/2021/11/2-linux-ubuntu-installer-defaults.jpg)

# Ubuntu Installer Default Settings

When installing Ubuntu, it has you approve a storage layout in a couple different screens (shown below). By default this storage layout will have a couple small boot partitions, and a third partition, which will be used by your LVM to create your root filesystem. You should be able to see the consistency between the screens below and the diagram above.

![Ubuntu installer storage layout partitions](https://packetpushers.net/wp-content/uploads/2021/11/3-ubuntu-installer-storage-layout-partitions.jpg)  
![Ubuntu installer storage configuration partitions](https://packetpushers.net/wp-content/uploads/2021/11/4-ubuntu-installer-storage-configuration-partitions.jpg)

# Use Your Default Free Space

As you can see above: the Ubuntu installer (by default) **left almost half of my disk space unusable by the root file system!** I’ve looked around to find an explanation on why these are the default settings, but can’t find anything. Before extending your underlying hypervisor disk or storage volume, you may want to see if you have free space available and ready to be used to extend your existing file system. If you used the Ubuntu defaults during installation, then there is a good chance you have this free space.

Start by checking your root filesystem free space with df -h. As you can see I am only using **14%** of my **~49GB** volume, but we’ll pretend I’m close to 100% and need to make that 49GB volume larger.

![Ubuntu LVM check free space df-h](https://packetpushers.net/wp-content/uploads/2021/11/5-ubuntu-lvm-check-free-space-df-h.jpg)

To check for existing free space on your Volume Group (where it is left by the installer default settings), run the command vgdisplay and check for free space. Here you can see I have **49.25GB** of free space ready to be used. If you don’t have any free space, move on to the next section to use some free space from an extended physical (or virtual) disk.

![Ubuntu LVM: check volume group space vgdisplay](https://packetpushers.net/wp-content/uploads/2021/11/6-ubuntu-lvm-check-volume-group-space-vgdisplay.jpg)

To use up that free space on your Volume Group (VG) for your root Logical Volume (LV), first run the lvdisplay command and check the Logical Volume size, then run lvextend -l +100%FREE /dev/ubuntu-vg/ubuntu-lv to extend the LV to the maximum size usable, then run lvdisplay one more time to make sure it changed.

![Ubuntu LVM: extend LV size](https://packetpushers.net/wp-content/uploads/2021/11/7-ubuntu-lvm-extend-lv-size-lvextend.jpg)

At this point you have increased the size of the block volume where your root filesystem resides, but you still need to extend the filesystem on top of it. First, run df -h to verify your (almost full) root file system, then run resize2fs /dev/mapper/ubuntu--vg-ubuntu--lv to extend your filesystem, and run df -h one more time to make sure you’re successful.

![Ubuntu LVM: extend root filesystem resize 2fs](https://packetpushers.net/wp-content/uploads/2021/11/8-ubuntu-lvm-extend-root-filesystem-resize2fs.jpg)

And that’s it. You just allocated the free space left behind by the Ubuntu installer to your root filesystem. If this is still not enough space, continue on to the next section to allocate more space by extending an underlying disk.

# Use Space from Extended Physical (or Virtual) Disk

First you need to increase the size of the disk being presented to the Linux OS. This is most likely done by expanding the virtual disk in KVM/VMWare/Hyper-V or by adjusting your RAID controller / storage system to increase the volume size. You can often do this while Linux is running; without shutting down or restarting. I’ve extended my **100GB** disk to **200GB** for my example machine.

Once that is done, you may need to get Linux to rescan the disk for the new free space. Check for free space by running cfdisk and see if there is free space listed, use “q” to exit once you’re done.

![Linux increase disk size space cfdisk](https://packetpushers.net/wp-content/uploads/2021/11/9-linux-increase-disk-size-space-cfdisk.jpg)

If you don’t see free space listed, then initiate a rescan of /dev/sda  with echo 1>/sys/class/block/sda/device/rescan. Once done, rerun cfdisk and you should see the free space listed.

![Linux free partition space scan](https://packetpushers.net/wp-content/uploads/2021/11/10-linux-free-partition-space-scan.jpg)

Select your /dev/sda3 partition from the list and then select “**Resize**” from the bottom menu. Hit **ENTER** and it will prompt you to confirm the new size. Hit **ENTER** again and you will now see the /dev/sda3 partition with a new larger size.

Select “**Write**” from the bottom menu, type **yes** to confirm, and hit **ENTER**. Then use “**q**” to exit the program.

Now that the LVM partition backing the  /dev/sda3 Physical Volume (PV) has been extended, we need to extend the PV itself. Run

```
pvresize /dev/sda3
```

to do this and then use pvdisplay to check the new size.

![Ubuntu extend physical volume pvresize](https://packetpushers.net/wp-content/uploads/2021/11/11-ubuntu-extend-physical-volume-pvresize.jpg)

As you can see above, my PV has been increased from 98.5GB to 198.5GB. Now let’s check the Volume Group (VG) free space with vgdisplay.

![Ubuntu LVM: check vg space vgdisplay](https://packetpushers.net/wp-content/uploads/2021/11/12-ubuntu-lvm-check-vg-space-vgdisplay.jpg)

We can see above that the VG has 100GB of free space. Now let’s check the size of our upstream Logical Volume (LV) using lvdisplay, extend the LV to use up all the VG’s free space with 

```
lvextend -l +100%FREE /dev/ubuntu-vg/ubuntu-lv
```

, and then check the LV one more time with lvdisplay to make sure it has been extended.

![Ubuntu LVM: check LV size lvdisplay](https://packetpushers.net/wp-content/uploads/2021/11/13-ubuntu-lvm-check-lv-size-lvdisplay.jpg)

At this point, the block volume underpinning our root filesystem has been extended, but the filesystem itself has not been resized to fit that new volume. To do this, run df -h to check the current size of the file system, then run 

```
resize2fs /dev/mapper/ubuntu--vg-ubuntu--lv
```

to resize it, and df -h one more time to check the new file system available space.

![Ubuntu LVM: extend filesystem resize2fs](https://packetpushers.net/wp-content/uploads/2021/11/14-ubuntu-lvm-extend-filesystem-resize2fs.jpg)

And there you go. You’ve now taken an expanded physical (or virtual) disk and moved that free space all the way up through the LVM [abstraction layers](https://packetpushers.net/podcast/day-two-cloud-081-abstractions-should-save-typing-not-thinking/) to be used by your (critically full) root file system. Time to check it off the to-do list and move on to the next IT emergency.