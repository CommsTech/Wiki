---
title: ntfy
description: 
published: true
date: 2023-07-25T13:15:54.706Z
tags: 
editor: markdown
dateCreated: 2023-05-21T15:28:21.146Z
---
# NTFY

set topic in app for this example we will set our topic to commsnet

on the server type 
```
curl -d "test" 192.168.255.133:10222/commsnet
```
-d denoting the message enclosed in "" then the ip(port number only required if not using a standard port) followed by a forward slash / then the topic name, in this case its commsnet

terminal should output something similar to 
```
{"id":"FxfEoc0eKJHp","time":1693438769,"expires":1693481969,"event":"message","topic":"commsnet","message":"test"}
```

