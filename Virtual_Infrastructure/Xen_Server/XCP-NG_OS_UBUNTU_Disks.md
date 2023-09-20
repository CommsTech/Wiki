
# Increase a disk in XCP-ng - OS UBUNTU

Only works if Ubuntu is setup to use LVM!

Steps to follow:

1. Stop the running VM from within the VM.
2. Go into Xen Orchestra and increase disk size of the running VM.
3. Start the VM again.
4. Check partitions: `sudo lsblk`
5. Grow the relevant partition: `sudo growpart /dev/xvda 1`
6. Check again: `sudo lsblk`
7. `fdisk -l`
8. `pvresize /dev/xvda1`
9. `lvdisplay`
10. `lvextend -l +100%FREE /dev/ubuntu-vg/ubuntu-lv`
11. `resize2fs /dev/ubuntu-vg/ubuntu-lv`
12. final check `df -h`