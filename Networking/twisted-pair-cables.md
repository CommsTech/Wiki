---
title: Twisted Pair Cables
description: Twisted pair cables are a type of copper cabling used for transmitting data in local area networks (LANs). They consist of two insulated wires twisted around each other to reduce electromagnetic interference. This document covers the basics of twisted pair cables, their status indicators, and troubleshooting common issues.
dateCreated: 2022-05-21T15:28:21.146Z
published: true
editor: markdown
tags:
  - networking
  - Cabling
dateModified: 
---
# Twisted Pair Cables Cheat-Sheet

Twisted pair cables are a type of copper cabling used for transmitting data in local area networks (LANs). They consist of two insulated wires twisted around each other to reduce electromagnetic interference. This document covers the basics of twisted pair cables, their status indicators, and troubleshooting common issues.
## Table of Contents

1. [Introduction to Twisted Pair Cables](http://192.168.255.11:8001/#introduction-to-twisted-pair-cables)
2. [Twisted Pair Cable Components and Construction](http://192.168.255.11:8001/#twisted-pair-cable-components-and-construction)
3. [Understanding Twisted Pair Cable Status Indicators](http://192.168.255.11:8001/#understanding-twisted-pair-cable-status-indicators)
    - [Down](http://192.168.255.11:8001/#down)
    - [OK](http://192.168.255.11:8001/#ok)
    - [No](http://192.168.255.11:8001/#no)
4. [Troubleshooting Twisted Pair Cables](http://192.168.255.11:8001/#troubleshooting-twisted-pair-cables)
5. [Best Practices for Installing and Maintaining Twisted Pair Cables](http://192.168.255.11:8001/#best-practices-for-installing-and-maintaining-twisted-pair-cables)

## Introduction to Twisted Pair Cables

In this section, we'll discuss the basics of twisted pair cables, their advantages, and common applications in LANs. We will also cover the different types of twisted pair cabling standards, such as UTP (Unshielded Twisted Pair) and STP (Shielded Twisted Pair).
## Cable Types

### Unshielded Twisted Pair (UTP)

As the title states, a UTP cable has no shielding. This is the most used and most basic type of cable. The cable contains pairs of wires twisted together to help reduce and prevent electromagnetic interference.

### Shielded Twisted Pair (STP)

STP cables are similar to UTP cables, where the wires are twisted together and then wrapped with a shielding or screening material which consists of foil wrapping or a copper braid jacket.

### Foil Twisted Pair (FTP)

With FTP cables, each twisted pair of cables is wrapped in a shielding of foil to protect the cable from EMI and crosstalk.

### Shielded Foil Twisted Pair (S/FTP)

A cable that is classified as S/FTP or Shielded Foil Twisted Pair is a combination of both FTP and STP shielding. The wires inside the cable are twisted and then shielded with a foil wrapping, then the 4-pair grouping of foiled wires are shielded by a wrapping of either foil or a flexible braided screening. This provides the highest level of protection against EMI and crosstalk.

## Wiring

### TIA/EIA 568A Wiring
PIN | COLOR | COLOR-TEXT
---|---|---
1 | <span style="color:green">█</span>█<span style="color:green">█</span>█<span style="color:green">█</span>█ | White and Green
2 | <span style="color:green">██████</span> | Green
3 | <span style="color:orange">█</span>█<span style="color:orange">█</span>█<span style="color:orange">█</span>█ | White and Orange
4 | <span style="color:blue">██████</span> | Blue
5 | <span style="color:blue">█</span>█<span style="color:blue">█</span>█<span style="color:blue">█</span>█ | White and Blue
6 | <span style="color:orange">██████</span> | Orange
7 | <span style="color:brown">█</span>█<span style="color:brown">█</span>█<span style="color:brown">█</span>█ | White and Brown
8 | <span style="color:brown">██████</span> | Brown
### TIA/EIA 568B Wiring
PIN | COLOR | COLOR-TEXT
---|---|---
1 | <span style="color:orange">█</span>█<span style="color:orange">█</span>█<span style="color:orange">█</span>█ | White and Orange
2 | <span style="color:orange">██████</span> | Orange
3 | <span style="color:green">█</span>█<span style="color:green">█</span>█<span style="color:green">█</span>█ | White and Green
4 | <span style="color:blue">██████</span> | Blue
5 | <span style="color:blue">█</span>█<span style="color:blue">█</span>█<span style="color:blue">█</span>█ | White and Blue
6 | <span style="color:green">██████</span> | Green
7 | <span style="color:brown">█</span>█<span style="color:brown">█</span>█<span style="color:brown">█</span>█ | White and Brown
8 | <span style="color:brown">██████</span> | Brown

## Categories

CATEGORY | MHz | Speed
---|---|---
CAT 3 UTP | 16MHz | 10Mps up to 100m
CAT 4 UTP | 20MHz | 16Mps up to 100m
CAT 5 UTP | 100MHz | 100Mbps up to 100m
CAT 5e UTP | 100MHz | 1000Mbps up to 100m
CAT 5e STP | 100MHz | 1000Mbps up to 100m
CAT 6 UTP | 250MHz | 10Gbps over to 33-55m
CAT 6a STP | 500MHz | 10Gbps over 100m
CAT 7 STP | 600MHz | 10Gbps over 100m
CAT 7a STP | 1000MHz | 10Gbps over 100m
CAT 8 STP | 2000MHz | 25/40Gps up to 30m