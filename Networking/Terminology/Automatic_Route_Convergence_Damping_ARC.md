---
title: Automatic Route Convergence Distance (ARC)
description: Automatic Route Convergence Distance (ARC) Description
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
# Automatic Route Convergence Distance (ARC)

**Automatic Route Convergence Damping (ARC)** is a mechanism used in routing protocols like Open Shortest Path First (OSPF) and Border Gateway Protocol (BGP) to control the rate at which routers exchange routing information during network convergence. ARC helps prevent frequent route flaps, which can cause instability and suboptimal routing.

When a router detects a change in its neighbor's routing table, it sends an update with the new information. With ARC, the router will not immediately propagate this change to all its other neighbors but instead waits for some time before doing so. This delay helps prevent unnecessary updates and minimizes the impact of routing instability on the network.