---
title: User Datagram Protocol (UDP)
description: UDP is a connectionless protocol that does not provide error checking or ordering guarantees. It sends data in datagrams, which are independent packets that can be sent and received without establishing a dedicated connection.
dateCreated: 
published: 
editor: markdown
tags: 
dateModified: 
---
# UDP (User Datagram Protocol)

**UDP** is a connectionless protocol that does not provide error checking or ordering guarantees. It sends data in datagrams, which are independent packets that can be sent and received without establishing a dedicated connection.

## Features
1. **Connectionless**: No need to establish a connection before sending data.
2. **Fast**: Does not perform error checking or retransmission of lost packets.
3. **Low latency**: Suitable for applications where low latency is essential.
4. **Unreliable**: Packets may be lost, duplicated, or delivered out of order.

## Use cases
1. Real-time applications (VoIP, video streaming)
2. Online gaming
3. DNS queries and responses
4. DHCP (Dynamic Host Configuration Protocol)