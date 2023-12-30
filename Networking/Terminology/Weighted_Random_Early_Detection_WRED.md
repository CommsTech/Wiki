---
title: Weighted Fair Queueing (WFQ)
description: Weighted Fair Queueing (WFQ) Description
published: true
date: 2023-12-30T02:41:39.777Z
tags:
  - networking
  - Engineering
  - Tech
  - Cisco
editor: markdown
dateCreated: 2023-12-30T02:41:39.777Z
---
## Weighted Random Early Detection (WRED)

**Weighted Random Early Detection (WRED)** is a congestion avoidance mechanism used in networking to manage network congestion by selectively dropping packets based on their priority level. WRED extends the basic Random Early Deflection (RED) algorithm by using the IP Precedence or Differentiated Services Code Point (DSCP) bits in the IP packet header to determine which packets should be dropped when the queue is congested.

WRED assigns weights to different classes of traffic based on their priority level, and when the queue reaches a certain threshold, it starts dropping packets with lower weights or priorities before those with higher ones. This mechanism helps prevent network congestion by reducing the number of packets that need to be dropped during periods of heavy traffic load.

WRED is commonly used in service provider networks to ensure high-priority traffic, such as voice and video, receives preferential treatment over best-effort traffic, like email or web browsing, during network congestion events. This results in improved network performance, reduced packet loss, and better user experience for critical applications.