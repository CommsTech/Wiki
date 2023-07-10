---
title: ESXi
description: Virtual Machine Hypervisor
published: true
date: 2022-07-25T13:37:12.115Z
tags: Hypervisor, Software
editor: markdown
dateCreated: 2022-09-09T04:44:01.149Z
---
# ESXi

## Patching a Standalone ESXi Host

 I have a standalone ESXi host at work just now to spin up some VM’s before the new production hardware arrives.

![HPE Image](https://www.vgemba.net/assets/images/Build-Number-01.png)

From the build number you can see this is ESXi 6.5 U1 Express Patch 4. The latest when this blog was posted is build 7526125 - ESXi 6.5 U1e, so I wanted to patch it. I have not added this host to vCenter as I don’t have a spare license plus it’s a short term box to get some VM’s built. This of course means no vSphere Update Manager which would make things easy to get up to date.

First of all you need to download the relevant patch from the [Product Patches](https://my.vmware.com/group/vmware/patch#search) site on VMware. Choose ESXi (Embedded and Installable) and then the version. Click Search:

![VMware Patch](https://www.vgemba.net/assets/images/VMware-Patch-01.png)

You can see here build 7526125 is the latest available. Download the patch from the site. It is a Zip file which you can open and see what’s in it if you want.

There are two ways I will show how to patch. One is using esxcli and the other is PowerCLI.

## esxcli Method[Permalink](https://www.vgemba.net/vmware/Patching-Standalone-ESXi-Host/#esxcli-method "Permalink")

Upload the downloaded Zip to a datastore the host can access. Next ensure SSH access is enabled. To do this go to Manage, Services…TSM-SSH and click Start:

![Enable SSH](https://www.vgemba.net/assets/images/Enable-SSH.png)

Now we can SSH into the host. First thing to so is ensure all VM’s are powered down. Then put the host in maintenance mode:

```
[root@localhost:~] esxcli system maintenanceMode set --enable true
```

Next we can patch the host. The esxcli command for this is esxcli software vib update pointing it at the file that was uploaded to the datastore. We also need the –depot switch to tell the command we are installing a packaged update. In this example:

```
[root@localhost:~] esxcli software vib update --depot /vmfs/volumes/LocalHDD/ESXi650-201801001.zip
Installation Result
   Message: The update completed successfully, but the system needs to be rebooted for the changes to be effective.
   Reboot Required: true
   VIBs Installed: VMW_bootbank_igbn_0.1.0.0-15vmw.650.1.36.7388607, VMW_bootbank_misc-drivers_6.5.0-1.36.7388607, 
   VIBs Removed: Avago_bootbank_scsi-mpt2sas_15.10.07.00-1OEM.550.0.0.1331820, MEL_bootbank_nmlx4-core_3.15.5.5-1OEM.600.0.0.2768847, 
   VIBs Skipped: VMW_bootbank_ata-libata-92_3.00.9.2-16vmw.650.0.0.4564106, VMW_bootbank_ata-pata-amd_0.3.10-3vmw.650.0.0.4564106,
```

You can see the update was successful and a reboot is required. I snipped out some of the output as the amount of VIBs Installed, Removed and Skipped is quite lengthy.

You can then reboot the host using the command:

```
[root@localhost:~] reboot
```

The host is now patched. You can exit maintenance mode by connecting back in using SSH and running the command:

```
[root@localhost:~] esxcli system maintenanceMode set --enable false
```

## PowerCLI Method[Permalink](https://www.vgemba.net/vmware/Patching-Standalone-ESXi-Host/#powercli-method "Permalink")

There is a key difference when using PowerCLI in that you need to extract the Zip file before it’s uploaded to the datastore. So extract the Zip file to a datastore the Host can access.

Now we need to connect to the Host in PowerCLI:

```
PS C:\Users\cwestwater> Connect-VIServer -Server esxi-host -User root -Password Password1

Name                           Port  User
----                           ----  ----
esxi-host                   443   root
```

Now we use need to enter maintenance mode:

```
PS C:\Users\cwestwater> Set-VMHost -State Maintenance

Name                 ConnectionState PowerState NumCpu CpuUsageMhz CpuTotalMhz   MemoryUsageGB   MemoryTotalGB Version
----                 --------------- ---------- ------ ----------- -----------   -------------   ------------- -------
esxi-host            Maintenance     PoweredOn       2         213        7002           1.465           3.999   6.5.0
```

Now we can use the PowerCLI command [`Install-VMHostPatch`](https://www.vmware.com/support/developer/windowstoolkit/wintk40u1/html/Install-VMHostPatch.html). The only needs one argument supplied `-HostPath`. This is the location of the extracted Zip file:

```
PS C:\Users\cwestwater> Install-VMHostPatch -HostPath /vmfs/volumes/LocalHDD/ESXi650-201801001/metadata.zip
WARNING: The update completed successfully, but the system needs to be rebooted for the changes to be effective.
```

We have to reboot the Host:

```
PS C:\Users\cwestwater> Restart-VMHost -Confirm
```

Don’t forget to exit maintenance mode once it comes back up:

```
PS C:\Users\cwestwater> Set-VMHost -State Connected

Name                 ConnectionState PowerState NumCpu CpuUsageMhz CpuTotalMhz   MemoryUsageGB   MemoryTotalGB Version
----                 --------------- ---------- ------ ----------- -----------   -------------   ------------- -------
esxi-host            Connected       PoweredOn       2         150        7002           1.470           3.999   6.5.0
```

Both methods will then show that Host is running the new build:

![HPE Image](https://www.vgemba.net/assets/images/Build-Number-02.png)


### SNMP Setup
#### Enabling SSH Access on ESXi

SSH access on an ESXi host is needed to run ESXCLI commands on a host remotely. In order to enable SSH access to your ESXi host, you can use VMware Host Client. Open a web browser, enter the IP address of your ESXi host in the address bar, then enter credentials to log in.

In the **Navigator** pane, go to **Host > Manage** and click the **Services** tab.

Right-click **TSM-SSH** and, in the context menu, click **Start.**

On the screenshot below you see the started SSH server service on the ESXi host.

[![Starting the SSH server on an ESXi host](https://www.nakivo.com/blog/wp-content/uploads/2021/11/Starting-the-SSH-server-on-an-ESXi-host.jpg)](https://www.nakivo.com/blog/wp-content/uploads/2021/11/Starting-the-SSH-server-on-an-ESXi-host.jpg)

Now you can connect to the ESXi host from a machine with an SSH client installed. If you’re using Windows, you can use PuTTY, a free and convenient SSH client. In Linux, run the SSH client from the command line with the command:

**ssh your_username@host_ip_address**

Enter the IP address of your ESXi host and port TCP 22 (the default port number) in the session settings of the SSH client to connect to the ESXi host via SSH.

[![Connecting to the host via SSH to enable SNMP on ESXi](https://www.nakivo.com/blog/wp-content/uploads/2021/11/Connecting-to-the-host-via-SSH-to-enable-SNMP-on-ESXi.png)](https://www.nakivo.com/blog/wp-content/uploads/2021/11/Connecting-to-the-host-via-SSH-to-enable-SNMP-on-ESXi.png)

#### ESXi SNMP Configuration

Once SSH access to the ESXi host is established, you can configure VMware ESXi SNMP options. On ESXi hosts, SNMP can be configured only in [the command-line interface](https://www.nakivo.com/blog/most-useful-esxcli-esxi-shell-commands-vmware-environment/). The graphical user interface (GUI) allows you only to start, stop, and restart the SNMP service.

Run the command in the console (terminal) and check the SNMP status on the ESXi host:

``` 
esxcli system snmp get
```

SNMP is disabled by default. The output for disabled SNMP on ESXi is shown on the screenshot. Most of the parameters are empty and or not configured.

[![Checking ESXi SNMP status](https://www.nakivo.com/blog/wp-content/uploads/2021/11/Checking-ESXi-SNMP-status.png)](https://www.nakivo.com/blog/wp-content/uploads/2021/11/Checking-ESXi-SNMP-status.png)

##### Configuring parameters of an SNMP agent

Set SNMP parameters for an SNMP agent on the ESXi host. The SNMP agent is used to send notifications (SNMP traps and informs) to a monitoring server and receive GET, GETNEXT, and GETBULK requests.

Set the community name (“_public_” is the community name set by default). The community name in this example is “_nakivo_”.

```
esxcli system snmp set –-communities nakivo
```

Set the SNMP target. The SNMP target is a server on which monitoring software is installed to handle SNMP traps and collect monitoring information. In my example, the SNMP target is the machine running Ubuntu Linux (_192.168.101.209_). UDP 161 is the default port used for SNMP and this port is defined in my ESXi SNMP configuration:

```
esxcli system snmp set –-targets=192.168.101.209@161/nakivo
```

Specify a location, for example, the geographical location, address, datacenter, or a room where the server is located:

```
esxcli system snmp set –-syslocation “Server room”
```

Specify contact information. The system administrator’s email address can be defined for this parameter:

**esxcli system snmp set –-syscontact michaelbose@nakivo.com**

Enable SNMP on ESXi:

```
esxcli system snmp set --enable true
```

Check the SNMP status on the ESXi host again:

```
esxcli system snmp get
```

Now you can see that the parameters are configured.

[![ESXi SNMP status is enabled](https://www.nakivo.com/blog/wp-content/uploads/2021/11/ESXi-SNMP-status-is-enabled.png)](https://www.nakivo.com/blog/wp-content/uploads/2021/11/ESXi-SNMP-status-is-enabled.png)

The Engine ID is the unique identifier for the SNMP agent (used for SNMP v3). The Engine ID can be set with the command (optional):

**esxcli system snmp set --engineid 544a33209458**

SNMP status is _running_ now. You can also open VMware Host Client, go to **Host > Manage > Services**, and check the status of the **snmpd** service.

[![Starting the ESXi SNMP service on an ESXi host](https://www.nakivo.com/blog/wp-content/uploads/2021/11/Starting-the-ESXi-SNMP-service-on-an-ESXi-host.jpg)](https://www.nakivo.com/blog/wp-content/uploads/2021/11/Starting-the-ESXi-SNMP-service-on-an-ESXi-host.jpg)

Test current SNMP configuration.

```
esxcli system snmp test
```

[![Testing VMware ESXi SNMP configuration](https://www.nakivo.com/blog/wp-content/uploads/2021/11/Testing-VMware-ESXi-SNMP-configuration.png)](https://www.nakivo.com/blog/wp-content/uploads/2021/11/Testing-VMware-ESXi-SNMP-configuration.png)

If you edit SNMP settings after that, restart the SNMP agent with the command:

```
/etc/init.d/snmpd restart
```

As an alternative, you can restart ESXi SNMP in the VMware Host Client GUI in the **Services** tab. Right-click the service and click **Restart** in the context menu.

[![Restarting the VMware SNMP server service on an ESXi host](https://www.nakivo.com/blog/wp-content/uploads/2021/11/Restarting-the-VMware-SNMP-server-service-on-an-ESXi-host.jpg)](https://www.nakivo.com/blog/wp-content/uploads/2021/11/Restarting-the-VMware-SNMP-server-service-on-an-ESXi-host.jpg)

If you need to reset ESXi SNMP settings, use the command:

**esxcli system snmp set -r**

The command to disable SNMP on an ESXi host is:

**esxcli system snmp set –enable false**