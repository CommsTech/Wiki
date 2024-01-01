---
title: Congestion Avoidance
description: Congestion Avoidance Description
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
# Congestion Avoidance

**Congestion Avoidance** is a set of techniques used in computer networking to prevent or minimize network congestion by controlling the rate at which data is transmitted into a network link or router buffer. The primary goal of congestion avoidance is to maintain optimal network performance, reduce packet loss, and minimize latency.

Congestion avoidance algorithms monitor network conditions and adjust the transmission rate based on observed traffic patterns and network load. Some common congestion avoidance techniques include:

1. **Random Early Detection (RED)**: A simple congestion avoidance algorithm that marks packets with a random probability based on queue length, allowing for early discard of packets before congestion occurs.
2. **[[Weighted_Random_Early_Detection_WRED|Weighted Random Early Detection (WRED)]]**: An extension of RED that uses IP Precedence bits to determine the probability of packet drop based on priority levels.
3. **Token Bucket Filtering**: A leaky bucket algorithm that regulates traffic flow by maintaining a token bucket and limiting the rate at which tokens are added to the bucket.
4. **Explicit Congestion Notification (ECN)**: A signaling mechanism used in TCP/IP networks to indicate congestion conditions to endpoints, allowing them to adjust their transmission rates accordingly.
5. **Traffic Shaping**: A [[Quality_of_Service_QoS|QoS ]]technique that regulates traffic flow by limiting the rate at which data is transmitted based on a predefined profile or contract with the network provider.

Congestion avoidance techniques help maintain network stability and ensure that available bandwidth is utilized efficiently while minimizing packet loss and delay.