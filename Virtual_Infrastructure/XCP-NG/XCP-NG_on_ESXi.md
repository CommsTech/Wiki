---
title: XCP-NG on ESXi (nested)
description: 
dateCreated: 2023-12-30T02:41:39.777Z
published: 
editor: markdown
tags: 
dateModified: 
---
# XCP-NG_on_ESXi

## XCP-NG in VMware nested virtualization

Let’s take a look at installing [[XCP-NG]] in VMware and see how you can do this with VMware nested virtualization, installing the XCP-NG hypervisor in your [VMware vSphere](https://www.virtualizationhowto.com/2022/06/vmware-vsphere-and-vsan-announced-new-features/) environment is as simple as creating a new VMWare vSphere VM, enabling the CPU settings for nested virtualization and installing XCP-NG. Let’s see what that looks like.

[![Enable nested virtualization for your XCP NG virtual machine](https://www.virtualizationhowto.com/wp-content/uploads/2022/07/Enable-nested-virtualization-for-your-XCP-NG-virtual-machine.png)](https://www.virtualizationhowto.com/wp-content/uploads/2022/07/Enable-nested-virtualization-for-your-XCP-NG-virtual-machine.png)

Enable nested virtualization for your XCP NG virtual machine

Select the locality for the keyboard layout.

[![Choose the country for keyboard mapping](https://www.virtualizationhowto.com/wp-content/uploads/2022/07/Choose-the-country-for-keyboard-mapping.png)](https://www.virtualizationhowto.com/wp-content/uploads/2022/07/Choose-the-country-for-keyboard-mapping.png)

Choose the country for keyboard mapping

Press the OK button to move forward with the installation of XCP-NG.

[![Beginning the setup wizard](https://www.virtualizationhowto.com/wp-content/uploads/2022/07/Beginning-the-setup-wizard.png)](https://www.virtualizationhowto.com/wp-content/uploads/2022/07/Beginning-the-setup-wizard.png)

Beginning the setup wizard

Accept the EULA to move on with installation configuration.

[![Accept the EULA for XCP NG installation](https://www.virtualizationhowto.com/wp-content/uploads/2022/07/Accept-the-EULA-for-XCP-NG-installation.png)](https://www.virtualizationhowto.com/wp-content/uploads/2022/07/Accept-the-EULA-for-XCP-NG-installation.png)

Accept the EULA for XCP NG installation

Select the local disk you want to install XCP-NG on and the provisioning method.

[![Choose the disk on which to install XCP NG](https://www.virtualizationhowto.com/wp-content/uploads/2022/07/Choose-the-disk-on-which-to-install-XCP-NG.png)](https://www.virtualizationhowto.com/wp-content/uploads/2022/07/Choose-the-disk-on-which-to-install-XCP-NG.png)

Choose the disk on which to install XCP NG

Select the media from which you want to install XCP-NG.

[![Select your installation source for your XCP NG install](https://www.virtualizationhowto.com/wp-content/uploads/2022/07/Select-your-installation-source-for-your-XCP-NG-install.png)](https://www.virtualizationhowto.com/wp-content/uploads/2022/07/Select-your-installation-source-for-your-XCP-NG-install.png)

Select your installation source for your XCP NG install

You can choose to verify your media. Here I am skipping that step to save time. The media I am using is an ISO uploaded to the VMware vSphere datastore and selected in the virtual machine properties in the vSphere client.

[![Choose media verification option](https://www.virtualizationhowto.com/wp-content/uploads/2022/07/Choose-media-verification-option.png)](https://www.virtualizationhowto.com/wp-content/uploads/2022/07/Choose-media-verification-option.png)

Choose media verification option

Set an administrator password for the root account.

[![Set your admin password for XCP NG](https://www.virtualizationhowto.com/wp-content/uploads/2022/07/Set-your-admin-password-for-XCP-NG.png)](https://www.virtualizationhowto.com/wp-content/uploads/2022/07/Set-your-admin-password-for-XCP-NG.png)

Set your admin password for XCP NG

Choose your networking settings. You can set a static address or use DHCP. Also, you can choose the VLAN for the administrative interface also.

[![Configure networking for your XCP NG host](https://www.virtualizationhowto.com/wp-content/uploads/2022/07/Configure-networking-for-your-XCP-NG-host.png)](https://www.virtualizationhowto.com/wp-content/uploads/2022/07/Configure-networking-for-your-XCP-NG-host.png)

Configure networking for your XCP NG host

Select the geographic region for the XCP-NG server.

[![Choose your geographic region for XCP NG host](https://www.virtualizationhowto.com/wp-content/uploads/2022/07/Choose-your-geographic-region-for-XCP-NG-host.png)](https://www.virtualizationhowto.com/wp-content/uploads/2022/07/Choose-your-geographic-region-for-XCP-NG-host.png)

Choose your geographic region for XCP NG host

Set your locality for time zone purposes.

[![Choose your locality for time zone purposes](https://www.virtualizationhowto.com/wp-content/uploads/2022/07/Choose-your-locality-for-time-zone-purposes.png)](https://www.virtualizationhowto.com/wp-content/uploads/2022/07/Choose-your-locality-for-time-zone-purposes.png)

Choose your locality for time zone purposes

Choose how you want to configure the system time, using [NTP](https://www.virtualizationhowto.com/2020/08/ntp-error-upgrading-vcenter-server-vcsa-6-7-to-7-0/) or manually setting the time configuration.

[![Choose how you want to set the XCP NG host system time](https://www.virtualizationhowto.com/wp-content/uploads/2022/07/Choose-how-you-want-to-set-the-XCP-NG-host-system-time.png)](https://www.virtualizationhowto.com/wp-content/uploads/2022/07/Choose-how-you-want-to-set-the-XCP-NG-host-system-time.png)

Choose how you want to set the XCP NG host system time

Configure the NTP server you want to use for time configuration.

[![Configure your NTP servers](https://www.virtualizationhowto.com/wp-content/uploads/2022/07/Configure-your-NTP-servers.png)](https://www.virtualizationhowto.com/wp-content/uploads/2022/07/Configure-your-NTP-servers.png)

Configure your NTP servers

Choose the **install XCP-NG** button to begin the installation of XCP-NG.

[![Choose to install XCP NG and begin the install with configuration options chosen](https://www.virtualizationhowto.com/wp-content/uploads/2022/07/Choose-to-install-XCP-NG-and-begin-the-install-with-configuration-options-chosen.png)](https://www.virtualizationhowto.com/wp-content/uploads/2022/07/Choose-to-install-XCP-NG-and-begin-the-install-with-configuration-options-chosen.png)

Choose to install XCP NG and begin the install with configuration options chosen

The installation of XCP-NG begins.

[![Installation of XCP NG begins](https://www.virtualizationhowto.com/wp-content/uploads/2022/07/Installation-of-XCP-NG-begins.png)](https://www.virtualizationhowto.com/wp-content/uploads/2022/07/Installation-of-XCP-NG-begins.png)

Installation of XCP NG begins

Answer the question of whether you want to install any supplemental packs in your XCP-NG installation or not.

[![Answer question about installing supplemental packs](https://www.virtualizationhowto.com/wp-content/uploads/2022/07/Answer-question-about-installing-supplemental-packs.png)](https://www.virtualizationhowto.com/wp-content/uploads/2022/07/Answer-question-about-installing-supplemental-packs.png)

Answer question about installing supplemental packs

The installation completes and finalizes the settings.

[![XCP NG installation begins to finalize](https://www.virtualizationhowto.com/wp-content/uploads/2022/07/XCP-NG-installation-begins-to-finalize.png)](https://www.virtualizationhowto.com/wp-content/uploads/2022/07/XCP-NG-installation-begins-to-finalize.png)

XCP NG installation begins to finalize

Once the host reboots, you can browse to the IP of your XCP-NG server and choose the **Xen Orchestra Quick Deploy** option. Enter the root password for your XCP-NG server.

[![Beginning the installation of Xen Orchestra](https://www.virtualizationhowto.com/wp-content/uploads/2022/07/Beginning-the-installation-of-Xen-Orchestra.png)](https://www.virtualizationhowto.com/wp-content/uploads/2022/07/Beginning-the-installation-of-Xen-Orchestra.png)

Beginning the installation of Xen Orchestra

Choose the storage and networking options for the deployed Xen Orchestra Server.

[![Xen Orchestra VM quick deploy storage and networking options](https://www.virtualizationhowto.com/wp-content/uploads/2022/07/Xen-Orchestra-VM-quick-deploy-storage-and-networking-options.png)](https://www.virtualizationhowto.com/wp-content/uploads/2022/07/Xen-Orchestra-VM-quick-deploy-storage-and-networking-options.png)

Xen Orchestra VM quick deploy storage and networking options

Set up the admin account for XOA, register the account, and set the machine password. If you choose not to register your Xen Orchestra server, note you will not be able to pull down updates for it. However, this process is free and just requires you complete the registration process. Click **Deploy**.

[![Set the admin account register Xen Orchestra and set the XOA machine account](https://www.virtualizationhowto.com/wp-content/uploads/2022/07/Set-the-admin-account-register-Xen-Orchestra-and-set-the-XOA-machine-account.png)](https://www.virtualizationhowto.com/wp-content/uploads/2022/07/Set-the-admin-account-register-Xen-Orchestra-and-set-the-XOA-machine-account.png)

Set the admin account register Xen Orchestra and set the XOA machine account

After the XOA VM finishes deploying, browse to the IP you configured in the quick deploy process. Login with the admin password you configured.

[![Login to Xen Orchestra using the configured password](https://www.virtualizationhowto.com/wp-content/uploads/2022/07/Login-to-Xen-Orchestra-using-the-configured-password.png)](https://www.virtualizationhowto.com/wp-content/uploads/2022/07/Login-to-Xen-Orchestra-using-the-configured-password.png)

Login to Xen Orchestra using the configured password

If you browse to the VMs section, you should see your XOA VM running in your Xen Orchestra inventory.

[![Viewing running virtual machines using Xen Orchestra](https://www.virtualizationhowto.com/wp-content/uploads/2022/07/Viewing-running-virtual-machines-using-Xen-Orchestra.png)](https://www.virtualizationhowto.com/wp-content/uploads/2022/07/Viewing-running-virtual-machines-using-Xen-Orchestra.png)

Viewing running virtual machines using Xen Orchestra

## XCP-NG FAQs

- **What is XCP-NG?** XCP-NG is an open-source hypervisor that allows easily spinning up workloads using the solution with a nice UI, live migration capabilities, and other features. It is free to download, along with the management platform – Xen Orchestra.
- **What is Xen Orchestra?** Xen Orchestra is a free, open-source management platform for your XCP-NG servers. It provides many management capabilities from a modern web UI and allows configuring VMs, hosts, and even backups for your XCP-NG environment.
- **How do you install XCP-NG in VMware?** As we have shown in the post, it is fairly easy. You just need to create a new VM with nested virtualization (Exposed hardware-assisted virtualization) enabled, [[mount]] the XCP-NG ISO, and begin the installation. There are several screens to configure your way through, but all-in-all, it is straightforward to get XCP-NG along with Xen Orchestra up and running.

## Final Notes

Coming from VMware vSphere with vCenter Server, it is amazing how powerful vSphere is when compared to open-source solutions. However, many of these solutions are powerful in their own right, due to their open-source nature, full features, and the ability to quickly stand up a hypervisor to start running workloads. XCP-NG and Xen Orchestra are compelling solutions that can certainly be used in many use cases. As shown, you can easily get up and running with XCP-NG in VMware environments for labbing purposes and familiarizing yourself with other solutions out there.