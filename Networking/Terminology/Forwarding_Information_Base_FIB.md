---
title: Forwarding Information Base (FIB)
description: 
dateCreated: 
published: 
editor: markdown
tags: 
dateModified: 
---
# Forwarding Information Base (FIB)

The **Forwarding Information Base (FIB)** is a data structure used in routing protocols like Open Shortest Path First (OSPF) and Border Gateway Protocol (BGP) to store the results of routing calculations. It is essentially an optimized version of the IP routing table, specifically designed for use by forwarding planes in routers and switches.

The FIB contains information about the best next hop, interface, and other relevant data required for forwarding packets based on their destination addresses. The FIB is derived from the IP routing table and is optimized for maximum lookup throughput, making it more efficient for forwarding decisions compared to the traditional IP routing table.

The FIB is essential in modern networking as it enables faster packet forwarding by reducing the time required to look up the next hop for a given destination address. It also allows for better traffic engineering and policy-based routing, making it an important component of advanced network services like Quality of Service (QoS) and Multiprotocol Label Switching (MPLS).

In summary, the Forwarding Information Base (FIB) is a critical data structure used in modern networking for efficient packet forwarding based on destination addresses. It is derived from the IP routing table and optimized for maximum lookup throughput.