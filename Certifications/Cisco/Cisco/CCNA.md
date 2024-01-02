---
title: CCNA
description: Cisco Certified Network Associate
dateCreated: 2022-09-09T04:44:01.149Z
published: true
editor: markdown
tags: Networking, Cisco, Certification
dateModified: 
---
# CCNA![[CCNA_Notes.pdf]]
- Stacked switches reduce management complexity and have a single management interface 
- Port Filter ACL's are applied first 
- Link Local addresses must be configured on all IPV 6 interfaces
- Accept, Reject, Error and continue are all responses from a TACACS daemon 
- APIC-EM path trace ACL'S check ingress and egress interfaces 
- Ospf uses Dijkstra, Rip uses Bellman-Ford and EIGRP uses DUAL 
- Link state protocol uses instant updates 
- BGP goes through active, idle and open sent states when establishing peer sessions 
- `show Snmp group` can show current SNMP Security model 
- If proxy ARP is configured on multiple devices, the internal L2 network becomes vulnerable to DDoS 
- QOS provides checksum and inspection. 
- CGMP is not compatible with HSRPv1 
- IaaS and paaS may require network infrastructure redesign 
- CHAP uses MD5 for peer authentication 
- MPLS can provide Authentication header and VPN 
- `debug ppp negotation` and `debug dialer packet` help troubleshoot a failed pppoe link
- QOS can be marked by ip precedence, DSCP and discard Class 
- Remote longing can be enabled With "terminal-monitor" and "logging host ip address" 
- Port priority value can be modified to create a preferred forwarding interface 
- 1 ACL per direction, per layer 3 "protocol" , per interface 
- Router-id must be configured to Enable EIGRPV6 
- Switch access ports drop packets with 802.10 tags
- Double tagging attack was mitigated by changing the native VAN to an unused VLAN 
- HSRP action router is chosen by highest ip add and configured priority 
- RSTP significantly reduces topology recovering time after a link failure
- RTP expands the STP port roles by adding the alternate and backup roles 
- RSTP provides a faster transition to the Forwarding state on point to point links than STP does 
- HSRP ip address acts as a default route for that interface. 
- Load value in `show interface port- chanel 1 etherchannel` is the number of source-destruction pairs
- PVST+ uses 802.1q to tunnel information.
- RSTP Root ports point towards the root bridge connection
- HSRP produces a virtual mac address starting with 00:00(ipv4) or 00:05(ipv6)
- `Show ip interface` can show you interfaces affected by ACL's
- in QOS ports are untrusted by default. 
- When an active HSRP router is preempted by a higher priority router it goes into "Spark" state
- DHCP snooping Can validate address requests and filter out invalid messages
-  APIC- EM can verify ACL's 
- Poison reverse is a learned neighbor with an infinite metric on the route 
- A Switch must be in VTP Server or Transparent mode before configuring a VLAN 
- ICMP packet TTL is default 255 and is decremented by 1 on every hop 
- SNMP view records can be used to restrict OID Groups
- Default port security mode is shutdown 
- QoS can mark ip precedence, DSCP and discard class 
- TACACS+ allows for separate authentication 
- APIC-EM requires source address and Destination address to run 
- APIC-EM automates network actions and makes network Functions programmable 
- Pinging the remote network is the best way to verify a host path 
- Enterprise Managed VPN Saves Money while Securing WAN 
- EIGRP internal routes show up as "D" 
