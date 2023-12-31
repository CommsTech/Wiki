---
title: Tdarr
description: Tdarr
dateCreated: 2022-08-30T04:27:17.152Z
published: true
editor: markdown
tags:
  - Server
  - Docker
  - Media
  - Management
dateModified: 
---
# Tdarr
## Distributed Transcoding System
**Tdarr is a conditional based transcoding application for automating media library transcode/remux management**


[Tdarr Website](https://tdarr.io/)

- Tdarr
	- https://tdarr.io/
	- This project came out and has a ton more options than my little batch script. (I run both)

Docker Compose (Server Only):
version: '3.4'
services:
  tdarr:
    image: haveagitgat/tdarr:latest
    restart: unless-stopped
    network_mode: bridge
    environment:
      PGID: '1000'
      PUID: '1000'
      UMASK_SET: '002'
      TZ: America/Denver
      serverIP: 0.0.0.0
      serverPort: '8266'
      webUIPort: '8265'
    ports:
     - 8265:8265
     - 8266:8266
    volumes:
     - /Your config location/Tdarr/Server:/app/server
     - /Your media location/Home:/mount
     - /Your Transcode location/Tdarr/Transcode_Cache:/temp
    deploy:
      restart_policy:
        condition: on-failure


## Tdarr-node setup on TrueNAS Scale

Walkthrough video located @ https://youtu.be/mDaApZ7ijmc