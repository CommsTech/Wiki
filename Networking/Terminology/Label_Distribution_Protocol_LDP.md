---
title: Label Distribution Protocol (LDP)
description: Label Distribution Protocol (LDP) Description
dateCreated: 2023-12-30T02:41:39.777Z
published: true
editor: markdown
tags:
  - networking
  - Engineering
  - Tech
  - Cisco
dateModified: 
---
# Label Distribution Protocol (LDP

**Label Distribution Protocol (LDP)** is a signaling protocol used in Multiprotocol Label Switching (MPLS) networks to exchange and distribute labels between routers. LDP enables routers to establish and maintain Label Switched Paths (LSPs), which are used for forwarding packets based on labels instead of routing based on IP addresses.

In an MPLS network, each router maintains a label information base (LIB) that stores the labels received from its neighbors. LDP uses multicast or unicast messages to distribute labels between routers and establish LSPs. Once established, these paths are used for forwarding packets based on the labels instead of routing them based on IP addresses.

LDP supports both unicast and multicast label distribution and is defined in RFC 5032. It is an essential component of MPLS networks, enabling efficient and scalable traffic engineering, traffic engineering, and VPN services.