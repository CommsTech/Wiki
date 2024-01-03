---
title: ntopNG
description: 
dateCreated: 
published: 
editor: markdown
tags: 
dateModified: 
---
# ntopNG
## Configuring ntopng for Monitoring Network Interfaces

Once installed and running, delve deeper into ensuring it keeps an eye on your network traffic. You will create a custom ntopng configuration for monitoring network interfaces.

To configure, follow these steps:

1. Create a new custom configuration file (_/etc/ntopng/ntopng.conf.d/custom.conf_) using your preferred text editor.

Related:[Essential Tips for Installing & Using Sublime Text on Ubuntu](https://adamtheautomator.com/sublime-text-on-ubuntu/)

2. Next, adjust and insert the following configuration into the file, save the changes, and exit the editor.

This configuration makes it in the background as a system daemon and captures network activity on IPv4.

```
# Run ntopng as a non-root user, which was created during the installation.
--user=ntopng
# Run ntopng on specific Linux server IP address and HTTP port.
-w=192.168.5.100:3000
# The network interface name to monitor.
--interface=enp0s8
# Ingress BPF packet filter. This example only captures network activity on IPv4
# and ignore the host IP address.
--packet-filter="ip and not proto ipv6 and not ip multicast and not ether broadcast and not ether host ff:ff:ff:ff:ff:ff and not host 192.168.5.2"
# Start ntopng community edition.
--community
# Run ntopng in the background as a system daemon.
--daemon
```

3. With the configuration in place, run the following command to restart the service and apply the changes that you have made.

This command does not provide output, but once the service restarts, it runs with new configurations.

```bash
sudo systemctl restart ntopng
```

Copy

4. Now, open your preferred web browser and navigate to the server IP address followed by port 3000 (i.e., _http://192.168.5.100:3000_) to access.

5. Log in to via the default username (**admin**) and password (**admin**), as shown below.

![Logging in to ntopng](https://adamtheautomator.com/wp-content/uploads/2023/05/image-168.png)

Logging in to ntopng

6. When prompted, input a new password to secure the admin account, and click **Change Password** to confirm the change.

![Changing default password](https://adamtheautomator.com/wp-content/uploads/2023/05/image-169.png)

Changing default password

7. Once logged in, you will see the Traffic Dashboard showing the monitoring target of your network interface, as shown below.

> _ntopng is an open-source network traffic monitoring and flow tool. It’s a drop replacement of ntop, but with enhanced features, high performance, and low resource server._

![Overviewing the Traffic Dashboard](https://adamtheautomator.com/wp-content/uploads/2023/05/image-170.png)

Overviewing the Traffic Dashboard

8. Lastly, click on the interface dropdown menu (top-left) and choose **System** to access the System interface dashboard.

![Accessing the System interface](https://adamtheautomator.com/wp-content/uploads/2023/05/image-171.png)

Accessing the System interface

As shown below, the ntopng **System** interface displays the detailed status of the ntopng system, such as **CPU Load**, **Memory usage**, and **Last log trace**.

![Viewing the ntopng system status](https://adamtheautomator.com/wp-content/uploads/2023/05/image-172.png)

Viewing the ntopng system status

## Running Network Discovery with ntopng

Network Discovery is a process in which it tries to reach and contact all available hosts within the local network or target network interface. During the process, it uses multiple protocols such as ARP, SSDP, MDNS, and SNMP.

To see how it’s Network Discovery works:

1. Click on the **Dashboard** menu (left navigation pane) → **Network Discovery**, and click **Run Discovery** to run Network Discovery.

![Running Network Discovery](https://adamtheautomator.com/wp-content/uploads/2023/05/image-173.png)

Running Network Discovery

During the process, the Network Discovery icon appears at the top, as shown below, and information progress should be visible.

![Viewing the Network Discovery in progress](https://adamtheautomator.com/wp-content/uploads/2023/05/image-174.png)

Viewing the Network Discovery in progress

When finished, the live hosts within the local network should be displayed in the Network Discovery section. The information includes the **IP address**, **Manufacturer**, **MAC address**, and the **OS**.

![Viewing the list of hosts detected after Network Discovery](https://adamtheautomator.com/wp-content/uploads/2023/05/image-175.png)

Viewing the list of hosts detected after Network Discovery

2. Next, click on the **Hosts** menu and select **Hosts** to access the list of detected hosts.

![Accessing the list of hosts](https://adamtheautomator.com/wp-content/uploads/2023/05/image-176.png)

Accessing the list of hosts

3. Click on a host’s **IP address** to get its detailed information.

![Accessing a host’s detailed information](https://adamtheautomator.com/wp-content/uploads/2023/05/image-177.png)

Accessing a host’s detailed information

> _Note that the 192.168.5.2 host is not displayed since you have blacklisted it via the `--packet-filter=` parameter in your custom configuration._

Below, you can see a handful of information about the host.

![Overviewing a host’s detailed information](https://adamtheautomator.com/wp-content/uploads/2023/05/image-178.png)

Overviewing a host’s detailed information

4. Now, return to the All Hosts page, and click the Live Flow icon to see all live connections on the target host.

![Viewing all live connections](https://adamtheautomator.com/wp-content/uploads/2023/05/image-179.png)

Viewing all live connections

Similar to the one below, you will see all live connections, including the **Client** and **Server** addresses.

![Checking the Live Flow network activity](https://adamtheautomator.com/wp-content/uploads/2023/05/image-191.png)

Checking the Live Flow network activity

## Managing Host Pools

Managing IP addresses and networks on the target network interface can be tedious. Luckily, host pools come in handy when you are monitoring large networks. How? You will first create a host pool and group up multiple hosts in each department.

A host pool can include the following:

- An IP address – A single host or IP address.
- Mac address – One Mac address is equal to one host.
- Network address – This address can be a Network CIDR IPv4/IPv6.

To create a new host pool, follow the steps below:

1. Click the **Shortcut** menu → **Pools** – the **+** button, which opens a pop-up window where you can name a new host pool.

![Initiating creating a new host pool](https://adamtheautomator.com/wp-content/uploads/2023/05/image-180.png)

Initiating creating a new host pool

> _The original Host Pool management is available on the **System** interface dashboard._

2. Next, provide a descriptive pool name (i.e., **OfficeNet**), and click **Add** to confirm adding the new host pool.

![Creating and naming the new host pool](https://adamtheautomator.com/wp-content/uploads/2023/05/image-181.png)

Creating and naming the new host pool

3. Click the **Action** icon for the newly-created host pool, and select **Manage Pool** to manage the host pool members.

![Managing the host pool members](https://adamtheautomator.com/wp-content/uploads/2023/05/image-182.png)

Managing the host pool members

4. Now, on the **Host Pool Members** page, click the **+** button, and a pop-up window appears where you can configure the new host pool member.

![Initiating adding a new network address](https://adamtheautomator.com/wp-content/uploads/2023/05/image-183.png)

Initiating adding a new network address

5. On the pop-up window, configure the new host pool member as follows:

- **Member Type** – Select **Network** to add your network CIDR.
- **Network** – Provide the **Network** CIDR to assign (i.e., _**10.5.3.0/24**_).

Once configured, click **Add** to confirm adding the new network address.

![Configuring the new host pool member](https://adamtheautomator.com/wp-content/uploads/2023/05/image-184.png)

Configuring the new host pool member

If successful, you will see your network CIDR in the **Members** column, as shown below.

![Verifying the new network CIDR](https://adamtheautomator.com/wp-content/uploads/2023/05/image-185.png)

Verifying the new network CIDR

## Monitoring Multiple Network Interfaces

The best scenario for deploying ntopng is right behind your router device. With this deployment type, you can monitor multiple network interfaces and network activity from the top level.

To monitor multiple network interfaces with ntopng:

1. Open the ntopng custom configuration (_/etc/ntopng/ntopng.conf.d/custom.conf_), and add the new parameter (`--interface=enp0s9`) for the second network interface.

Change the interface name (`enp0s9`) to yours, save the changes, and close the editor.

2. Next, run the following command to `restart` the `ntopng` service and apply the changes.

This command does not provide output, but you will view the new interface in the following step.

```bash
sudo systemctl restart ntopng
```

Copy

3. Return to the ntopng dashboard, click the dropdown field (top-left), and select the new interface to monitor, as shown below.

![Switching to a different interface monitoring](https://adamtheautomator.com/wp-content/uploads/2023/05/image-190.png)

Switching to a different interface monitoring

4. Now, run the **Network Discovery** within the new interface to get the list of available hosts.

![Discovering all available hosts](https://adamtheautomator.com/wp-content/uploads/2023/05/image-189.png)

Discovering all available hosts

5. After Network Discovery, click the **Hosts** menu → **Hosts →** the host’s IP address (i.e., _**10.5.3.5**_) to get detailed information about the host.

![Accessing a host’s detailed information](https://adamtheautomator.com/wp-content/uploads/2023/05/image-188.png)

Accessing a host’s detailed information

Notice below that the host is part of the **OfficeNet** pool you created in step two of the “Managing Host Pools” section.

![Checking detailed host information](https://adamtheautomator.com/wp-content/uploads/2023/05/image-187.png)

Checking detailed host information

6. Lastly, click the Live Flow icon to view the current network activity on the host.

The **Live Flows** page below shows you the **ICMP** ping from the **ubuntu** host to the **10.5.3.4** host, and the HTTP request to the **10.5.3.5** server.

![Checking the Live Flow host](https://adamtheautomator.com/wp-content/uploads/2023/05/image-186.png)

Checking the Live Flow host

## Conclusion

Great job! Throughout this tutorial, you have learned to set up ntopng, create host pools and monitor multiple network interfaces. Now, you can add more network interfaces and watch over your real-time and historical traffic network activity.

But instead of just viewing network activity, why not store Live Flow network activity in external databases, like [Elasticsearch](https://www.ntop.org/ntopng/exploring-your-traffic-using-ntopng-with-elasticsearchkibana/), [ClickHouse](https://www.ntop.org/guides/ntopng/clickhouse/index.html), and MySQL/MariaDB (deprecated soon)?