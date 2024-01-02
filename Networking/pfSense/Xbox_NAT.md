---
title: Xbox NAT
description: 
dateCreated: 2022-05-21T15:28:21.146Z
published: true
editor: markdown
tags: 
dateModified: 
---
# Xbox_NAT
## pfSense – How to fix STRICT NAT

There are several ways to fix the STRICT NAT situation.

Placing the XBox One in a **[DMZ (DeMilitarized Zone)](https://en.wikipedia.org/wiki/DMZ_(computing))**, means that your XBox will be exposed to the Internet without any protection – which actually may be fine. I used a small computer with 4 Ethernet port (network) ports. One port used for WAN (Internet) and one for LAN (my devices). I could use one of the remaining ports specifically for DMZ purposes. If you’re interested in this approach then consider reading this article: [How to create a DMZ with pfSense 2.4.2](https://www.ceos3c.com/pfsense/how-to-create-a-dmz-with-pfsense-2-4-2/).

Personally I try to avoid using the DMZ approach if I can. Just feels like I’m opening more than I should to make things work. But … it most certainly is an option.

My preferred method is by setting the appropriate rules and only allow and open what is really needed – there is no need to leave the door wide open.

## pfSense – OPEN NAT for your XBox One

The following method should work for the XBox One to get rid of **STRICT NAT** and end up with an **OPEN NAT**, and can be applied for multiple XBox One devices.  
Unfortunately, I do not have other consoles like the Play Station 4 or the Nintendo Switch (nasty thing with money – you can spend only once).  
From what I have seen; this most likely works with other consoles as well. Your mileage may vary.

Just a warning: I’m most certainly **not** a firewall or a pfSense expert.  
Everything presented here is from what I have read and tested on my own setup.  
Suggestions, and improvements are most welcome.

## Step 1: Give your XBox One a fixed IP address in pfSense

We are going to be adding some rules to the pfSense firewall. To make sure these rules apply to the right devices, we must have a known IP address for our XBox One device(s).

This can be done it two ways: either you assign a static IP address to your XBox One or you reserved the IP address for you XBox One in the [DHCP](https://en.wikipedia.org/wiki/Dynamic_Host_Configuration_Protocol) of your [pfSense](https://www.pfsense.org/) setup.

Since I use DHCP for my network, I decided to use the most obvious: tell my DHCP to use a fixed IP address for my XBox One. You can apply this to all your XBox One devices in case you have multiple.

### Determine an IP Address for your XBox One

_Note_: I assume that your LAN connection is called “LAN” in your pfSense environment.

In pfSense go to **Services**  **DHCP Server**  **LAN**.

Go to the “General Options” and take note of the range used by your DHCP – we will need this to pick an IP address.

[![pfSense - IP range used by your DHCP](https://www.tweaking4all.com/wp-content/uploads/2018/08/pfsense-dhcp-range.png)](https://www.tweaking4all.com/wp-content/uploads/2018/08/pfsense-dhcp-range.png)

pfSense – IP range used by your DHCP

You will have to determine what the fixed IP address of your XBox One should be.  
**Make sure you pick an IP address that does not fall in the range used by your DHCP**!

_As example:_  
The example DHCP uses the range 192.168.2.10 – 192.168.2.150.  
So for our XBox we should pick an IP address lower than 192.168.2.10, greater than 192.168.2.150, and not yet in use by another device.  
In my example I picked **192.168.2.239**.

_Note_: If you have more than one XBox One, pick a unique IP address for those as well.  
_Note_: If the range prevents you from picking one outside of the range, then please change your DHCP range to make some room.

### Define a fixed IP Address for your XBox One

Next; scroll all the way to the bottom (under “**DHCP Static Mappings for this Interface**“) and click the “**Add**” button. A new page will load.

Here we will need the MAC address of your XBox One – you can find this in the network details of your XBox One, or in the DHCP log of pfSense (menu: **Status**  **DHCP Leases**).

Fill in the form as shown below, and make sure you pick the IP address you selected for your XBox One.

1.  The **MAC address** of your XBox One,
2.  A name or **Client identifier** for your XBox One (avoid using single or double quotes!!),
3.  The **IP address** you picked for your XBox One (192.168.2.239 in my example),
4.  A **Hostname** for your XBox One (this can be anything, just do not use special characters or spaces, and keep it short),
5.  _Optional_: **description** so you can recognize the device in pfSense lists and log. For example “XBox One X Livingroom”.
6.  Click the “**Save**” button.

[![pfSense - Define Fixed IP Address for your XBox One](https://www.tweaking4all.com/wp-content/uploads/2018/08/pfsense-fixed-ip-xboxone.png)](https://www.tweaking4all.com/wp-content/uploads/2018/08/pfsense-fixed-ip-xboxone.png)

pfSense – Define Fixed IP Address for your XBox One

After click the “Save” button you will get a message, stating that static mapping has changed. Click the “**Apply Changes**” button.

[![pfSense - Apply Changes](https://www.tweaking4all.com/wp-content/uploads/2018/08/pfsense-fixed-ip-xboxone-changed.png)](https://www.tweaking4all.com/wp-content/uploads/2018/08/pfsense-fixed-ip-xboxone-changed.png)

pfSense – Apply Changes

Repeat these steps for additional consoles devices.

## Step 2: Enable UPnP & NAT-PMP in pfSense

The next step is to enable UPnP in your pfSense setup, to do this, go to: **Services**  **UPnP & NAT-PMP**.

In the image below, we did the following settings:

1.  Check “**Enable UPnP & NAT-PMP**“,
2.  Check “**Allow UPnP Port Mapping**“,
3.  Check “**Allow NAT-PMP Port Mapping**“,
4.  Select your **WAN** at the “External Interface“,
5.  Select your **LAN** at the “Interfaces” list,
6.  Check “**Deny access to UPnP & NAT-PMP by default**“
7.  At “**ACL Entries**” we will need to add an entry for each of your XBox Device in the following format, where **a.b.c.d** should be replaced with the IP address we just set for our XBox One:  
    `allow 53-65535 a.b.c.d/32 53-65535`.  
    So in my example this is:  
    `allow 53-65535 192.168.2.239/32 53-65535`.  
    This says:  
    for the specific IP address 192.168.2.239, UPnP can be used for any target (/32) and for the external ports “53-65535” and internal ports “53-65535”.
8.  Click the “**Add**” button,
9.  Click “**Save**” when done.

_Note:_ repeat steps **7** and **8** for each additional XBox One you have.

[![pfSense - Enable UPnP for your XBox One](https://www.tweaking4all.com/wp-content/uploads/2018/08/pfsense-enable-upnp-for-xboxone.png)](https://www.tweaking4all.com/wp-content/uploads/2018/08/pfsense-enable-upnp-for-xboxone.png)

pfSense – Enable UPnP for your XBox One

## Step 3: Configure Outbound NAT for pfSense

We’re almost done, we just need to modify our NAT settings a little bit.

In pfSense go to **[[Firewall]]**  **NAT**  **Outbound**. Don’t forget to click “Outbound”!

First we need to set our outbound NAT to **Hybrid**:

[![pfSense - Set NAT to Hybrid](https://www.tweaking4all.com/wp-content/uploads/2018/08/pfsense-hybrid-outbound-nat.png)](https://www.tweaking4all.com/wp-content/uploads/2018/08/pfsense-hybrid-outbound-nat.png)

pfSense – Set NAT to Hybrid

We additionally need to add a so called mapping rule: click under “**Mappings**” the “**Add**” button that points up.

_Note_: Make sure you did **NOT** check “Disable this rule”.

1.  Select **WAN** at the “Interface” field,
2.  Set “Protocol” to “**any**“.
3.  Set “Source” to “**Network**” and **enter the IP address of your Xbox One**, and the following field to “**/32**“,
4.  Set “Destination” to “**any**” and leave the other fields as they are,
5.  Set “Address” to “**Interface Address**“,
6.  Check “**Static Port**” (so the pfSense NAT will not use a different port number),
7.  Enter some kind of **description** (so you can find it again later, and recall why you’ve added this rule),
8.  and finally click the “**Save**” button.

_Note:_ For additional XBox One devices, rinse an repeat these 8 steps for each console you’d like to add.

[![pfSense - Outbound NAT rule for XBox One](https://www.tweaking4all.com/wp-content/uploads/2018/08/pfsense-outbound-nat-rule-for-xboxone.png)](https://www.tweaking4all.com/wp-content/uploads/2018/08/pfsense-outbound-nat-rule-for-xboxone.png)

pfSense – Outbound NAT rule for XBox One

## Step 4: Reboot your devices

Now this may or may not be required, but I did it anyway.

1.  Shutdown your XBox One – completely so remove the power cord after doing a console shutdown.
2.  Reboot your pfSense Firewall – this may not be required.
3.  After reboot verify your XBox One Network details – You should have an OPEN NAT now and STRICT NAT should be an issue of the past.

### Tip: Alternative to rebooting … 

A great tip from Charles (below) as an alternative to rebooting your Firewall:  
You can just flush the active connections: **Firewall**  **Diagnostics**  **States Reset**.

I did get another tip on this, related to Universal PnP: you can restart the service.

Personally, I’m a little paranoid when it comes to things like that and choose to reboot – it takes only a few seconds on my setup.