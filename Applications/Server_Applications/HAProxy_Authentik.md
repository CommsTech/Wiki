---
title: 
description: 
dateCreated: 
published: 
editor: markdown
tags: 
dateModified: 
---
# HAProxy_Authentik

PFSENSE HAPROXY Authentik

![[Pasted image 20230923143719.png]]

```
http-request header set
name: X-Real-IP

, fmt: %[src]

http-request header set
name: X-Forwarded-Method

, fmt: %[method]

http-request header set
name: X-Forwarded-Proto

, fmt: %[var(req.scheme)]

http-request header set
name: X-Forwarded-Host

, fmt: %[req.hdr(Host)]

http-request header set
name: X-Original-URL

, fmt: %[url]
```

![[Pasted image 20230923143931.png]]
