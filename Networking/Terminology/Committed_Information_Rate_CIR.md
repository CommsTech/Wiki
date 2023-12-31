---
title: Cisco Service Providor Next-Generation Core Network Services (SPCORE)
description: Cisco Service Providor Next-Generation Core Network Services (SPCORE) Description
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
# Committed Information Rate (CIR)
The correct formula for determining the Committed Information Rate (CIR) in a traffic policing context is:

```markdown
CIR = Bc / (Tc / 1000)
```

Here's what each variable represents:
- `Bc`: The committed burst size, which is the maximum amount of data that can be transmitted during the measurement interval (Tc). It is measured in bits.
- `Tc`: The measurement interval or the time period over which Bc is measured and enforced. It is typically measured in milliseconds.

To convert Tc from milliseconds to seconds, divide it by 1000. For example, if Tc is 125 ms, then Tc = 125 / 1000 = 0.125 seconds.

So the formula becomes:
```markdown
CIR = Bc / (Tc / 1000)
= Bc * (1 / Tc)
= Bc / (1000 / Tc)
```

This formula calculates the CIR, which represents the maximum rate at which traffic can be transmitted during the measurement interval.