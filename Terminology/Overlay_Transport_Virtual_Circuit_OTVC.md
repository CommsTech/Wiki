---
title: Overlay Transport Virtual Circuit (OTVC)
description: Overlay Transport Virtual Circuit (OTVC) Description
published: true
date: 2023-12-30T02:41:39.777Z
tags:
  - networking
  - Engineering
  - Tech
  - Cisco
  - routing
editor: markdown
dateCreated: 2023-12-30T02:41:39.777Z
---
## Overlay Transport Virtual Circuit (OTVC)

**Overlay Transport Virtual Circuit (OTVC)** is a technique used in Carrier Ethernet networks to provide Layer 2 connectivity across multiple physical links or sites. OTVC allows the creation of virtual circuits that span multiple physical connections, enabling seamless extension of Layer 2 domains over WANs and MPLS networks.

OTVC operates by encapsulating Ethernet frames within IP packets and transporting them over an underlying IP network. The encapsulation process preserves the original MAC addresses, ensuring that Layer 2 information is maintained across the virtual circuit. This allows for transparent bridging between sites, enabling applications to operate as if they were connected on a single LAN.

OTVC is particularly useful in service provider networks where multiple sites need to be interconnected with high-bandwidth, low-latency, and resilient connections while maintaining the benefits of Layer 2 connectivity. It can also simplify network management by reducing the number of physical links required between sites and enabling easier provisioning and maintenance of services.