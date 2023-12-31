---
title: Rancher
description: 
published: true
date: 2022-07-25T13:15:54.706Z
tags: 
editor: markdown
dateCreated: 2022-05-21T15:28:21.146Z
aliases:
---
# Rancher
**Rancher**, the open-source multi-cluster orchestration platform, lets operations teams deploy, manage and secure enterprise **Kubernetes ([[kubernetes]])**.

Project Homepage: [Rancher Homepage](https://www.rancher.com)

---
## Remove Installation

```
kubectl delete validatingwebhookconfiguration rancher.cattle.io
kubectl delete mutatingwebhookconfiguration rancher.cattle.io
```

