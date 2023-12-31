---
title: Rancher
description: 
dateCreated: 2022-05-21T15:28:21.146Z
published: true
editor: markdown
tags: 
dateModified: 
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

