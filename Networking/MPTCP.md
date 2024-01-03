---
title: MPTCP
description: multi-path TCP/IP router
dateCreated: 2022-05-16T14:22:22.830Z
published: true
editor: markdown
tags: networking, Router
dateModified: 
---
# ***OpenMPTCPRouter*** 

OpenMPTCProuter is an open source solution to aggregate and encrypt multiple internet connections and terminates it over any VPS which make clients benefit security, reliability, net neutrality, as well as dedicated public IP.

The aggregation is based on Multipath TCP (MPTCP), which is ISP, WAN type, and latency independent "whether it was Fiber, VDSL, SHDSL, ADSL, 4G or even 5G", different scenarios can be configured to have either aggregation or failover based on MPTCP.

Aggregation via [Multi-link VPN (MLVPN)](https://github.com/markfoodyburton/MLVPN/commits/new-reorder) and [Glorytun UDP](https://github.com/angt/glorytun) with multipath support are also supported.

The solution takes advantage of the OpenWRT/LEDE system, which is user friendly and also adds the possibility of installing other packages like VPN, QoS, routing protocols, monitoring, etc. through web-interface or terminal.

Main website: [https://www.openmptcprouter.com/](https://www.openmptcprouter.com/)

Packages made for OpenMPTCProuter are available here: [https://github.com/Ysurac/openmptcprouter-feeds](https://github.com/Ysurac/openmptcprouter-feeds)

OpenMPTCProuter VPS script part: [https://github.com/Ysurac/openmptcprouter-vps](https://github.com/Ysurac/openmptcprouter-vps)

## Install from pre-compiled images

You can download precompiled images from [https://www.openmptcprouter.com/](https://www.openmptcprouter.com/)

Then copy it to a sdcard:

```sh
gunzip omr-*.img.gz
dd bs=4M if=omr-*.img of=/dev/sdX conv=fsync
```

## Install from source

[Create image](https://github.com/Ysurac/openmptcprouter/wiki/Create-image-for-unsupported-platform)

## Credits

Our solution is mainly based on:

* [OpenWRT](https://openwrt.org)

* [MultiPath TCP (MPTCP)](https://multipath-tcp.org)

* [Shadowsocks](https://shadowsocks.org)

* [Glorytun](https://github.com/angt/glorytun)

https://www.openmptcprouter.com/

https://github.com/Ysurac/openmptcprouter