---
title: Plex Meta Manager
description: Fix metadata and collections with plex meta manager
dateCreated: 2022-08-30T04:27:17.152Z
published: true
editor: markdown
tags:
  - Server
  - Media
  - Management
dateModified: 
---
# Plex Meta Manager
Plex Meta Manager is an open source Python 3 project that has been designed to ease the creation and maintenance of metadata, collections, and playlists within a Plex Media Server. The script is designed to be run continuously and be able to update information based on sources outside your plex environment. Plex Meta Manager supports Movie/TV/Music libraries and Playlists.


[GitHub](https://github.com/meisnate12/Plex-Meta-Manager)
[Meta Manager Wiki](https://metamanager.wiki/)

[Setup Video](https://youtu.be/dF69MNoot3w)

Docker Compose File:
version: "3"
services:
  plex-meta-manager:
    image: meisnate12/plex-meta-manager
    container_name: plex-meta-manager
    environment:
      - TZ=America/Denver
    volumes:
      - /YourLocation/PlexMetaManager/config:/config:ro
    restart: unless-stopped