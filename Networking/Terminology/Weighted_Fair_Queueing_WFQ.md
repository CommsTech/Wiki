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
## Weighted Fair Queuing (WFQ)

**Weighted Fair Queueing (WFQ)** is a queuing algorithm used in computer networking to provide fair bandwidth allocation among multiple traffic classes or flows. WFQ ensures that each flow receives a fair share of the available bandwidth, preventing any single flow from monopolizing the network resources. It does this by assigning weights to different flows based on their priority levels.

WFQ divides the total available bandwidth into equal parts for each flow and maintains separate queues for each flow. When data arrives at a router or switch, it is sorted into the appropriate queue based on its flow identifier (e.g., IP address, port number, or VLAN ID). The router then services each queue in proportion to their respective weights, ensuring that high-priority flows receive more bandwidth than low-priority ones.

Weighted Fair Queueing is widely used in service provider networks and other environments where multiple traffic classes need to be prioritized based on their importance or requirements. It helps maintain Quality of Service (QoS) and ensures that critical applications and services receive the necessary resources while minimizing contention and congestion within the network.

WFQ is an improvement over traditional First-In-First-Out (FIFO) queuing, as it provides more control and fairness in handling multiple traffic classes or flows.