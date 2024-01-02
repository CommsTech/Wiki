---
title: TCP vs UDP
description: TCP (Transmission Control Protocol) and UDP (User Datagram Protocol) are two primary transport protocols used for sending data over the internet. Both have their unique features, strengths, and weaknesses, making them suitable for different applications.
dateCreated: 2022-05-21T15:28:21.146Z
published: true
editor: markdown
tags: 
dateModified: 
---
# TCP vs UDP: Comparing Transmission Protocols

**[[Transmission_Control_Protocol_TCP|TCP (Transmission Control Protocol)]]** and **[[User_Datagram_Protocol_UDP|UDP (User Datagram Protocol)]]** are two primary transport protocols used for sending data over the internet. Both have their unique features, strengths, and weaknesses, making them suitable for different applications.

## Comparison

| TCP                   | UDP                |
|----------------------|--------------------|
| Reliable              | Connectionless     |
| Ordered delivery      | No order           |
| Error checking       | No error checking |
| Establishes a connection | No connection    |

### Some additional notes

Each frame goes through several buffers as you send it: The application buffer, The Protocol Buffer, The Software interface buffer and the Hardware interface buffer. As you start stressing the stack by sending high speed data you will fill up these buffers and either block or lose data. You also have strategies for timeliness and polling that can impact your performance. For example, by using a larger buffer and poll less often you can get much better performance while sacrificing latency.

TCP is optimized for high speed bulk transfers while UDP is optimized for low latency in the Linux kernel. This has an impact on buffer sizes and how data is polled and handed over. In addition to this, you frequently have offloading to hardware for TCP. I would expect considerably better performance for TCP compared to UDP.

Note that sending high speed data over UDP is usually a bad idea, unless you implement your own congestion control. TCP protects your network from congestion collapses. Use UDP when you have small amounts of data or high timeliness requirements.
