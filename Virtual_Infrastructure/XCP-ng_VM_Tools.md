
## Installation for methods for centos
## Method 1: Install from the guest tools ISO

This installation supports many distributions including the ones that doesn’t have guest tools in their upstream repositories.

Attach the guest tools ISO to the VM from Xen Orchestra or [XenCenter|XCP-ng Center](https://computingforgeeks.com/managing-xcp-ng-hypervisor-with-xencenter-xcp-ng-center/).

From XCP-ng Center console select the instance you wan to install VM Tools on. Then navigate to **“General”** and click on “**XCP-ng VM Tools not installed**” link

![](https://techviewleo.com/wp-content/uploads/2021/04/Install-XCP-ng-VM-Tools-CentOS-AlmaLinux-01.png?ezimgfmt=rs:696x374/rscb7/ng:webp/ngcb7)

In the next screen use “**Install XCP-ng VM Tools**” button to mount the disk on the VM.

![](https://techviewleo.com/wp-content/uploads/2021/04/Install-XCP-ng-VM-Tools-CentOS-AlmaLinux-02.png?ezimgfmt=rs:696x200/rscb7/ng:webp/ngcb7)

The disk mapping should be visible in the VM console.

![](https://techviewleo.com/wp-content/uploads/2021/04/Install-XCP-ng-VM-Tools-CentOS-AlmaLinux-03.png?ezimgfmt=rs:696x77/rscb7/ng:webp/ngcb7)

Login to the instance from the console or SSH and run the commands to mount guest tools ISO inside the VM:

```
sudo mount /dev/cdrom /mnt
```

Run the installer script:

```
sudo bash /mnt/Linux/install.sh -d rhel -m 8
```

Unmount guest tools cd rom:

```
sudo umount /dev/cdrom
```

## Method 2: Install XCP-ng VM Tools from OS online repositories

XCP-ng guest tools can be installed from the target distribution’s online repositories if the packages are available. On Red Hat based Linux distributions the packages required are available in the EPEL repository.

Use the commands in the guide below to enable EPEL repository on AlmaLinux 8 or CentOS 8 system;

[How To Enable EPEL Repository on AlmaLinux OS 8 / CentOS 8](https://techviewleo.com/enable-epel-repository-on-almalinux-os/)

Confirm EPEL repository has been enabled by running the command:

```
$ sudo yum repolist
repo id                                                     repo name
appstream                                                   AlmaLinux 8 - AppStream
baseos                                                      AlmaLinux 8 - BaseOS
epel                                                        Extra Packages for Enterprise Linux 8 - x86_64
epel-modular                                                Extra Packages for Enterprise Linux Modular 8 - x86_64
extras                                                      AlmaLinux 8 - Extras
powertools                                                  AlmaLinux 8 - PowerTools
```

Once the repository is enabled, install XCP-ng guest tools with the command below:

```
sudo yum install xe-guest-utilities-latest
```

Accept installation prompts:

```
Dependencies resolved.
==================================================================================================================================================================
 Package                                             Architecture                     Version                                Repository                      Size
==================================================================================================================================================================
Installing:
 xe-guest-utilities-latest                           x86_64                           7.21.0-1.el8                           epel                           1.1 M

Transaction Summary
==================================================================================================================================================================
Install  1 Package

Total download size: 1.1 M
Installed size: 3.8 M
Is this ok [y/N]: y
```

The service is not enabled by default, so enable it and start it:

```
sudo systemctl enable xe-linux-distribution
sudo systemctl start xe-linux-distribution
```

There is no need to reboot the VM after installation with either of the methods.