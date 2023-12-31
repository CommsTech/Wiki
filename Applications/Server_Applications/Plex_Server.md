---
title: Plex - Media Server and Streaming Platform
description: A comprehensive note on Plex, a popular media server and streaming platform that allows users to access their personal media collections from any device. This note covers features, setup instructions, and troubleshooting tips for using Plex in various browsers like Firefox, Brave, Chrome, and Safari.
dateCreated: 2022-08-30T04:27:17.152Z
published: true
editor: markdown
tags:
  - Server
  - Media
  - Management
  - Streaming
dateModified: 
---
# Plex

Mine is hosted at https://blackrifle.commsnet.org


I currently run my plex server in a TrueNas Scale application with GPU passthrough but i recently switched over. I was running my plex server on an Ubuntu 20.04 VM
	https://www.reddit.com/r/PleX/comments/kpfpfz/install_plex_on_ubuntu_connect_to_nas_gpu/
	- Here is a good walkthrough for Docker (p2000)
	https://jakeknows.tech/plex-hardware-transcoding/

	- 2 GPU Docker Linux Setup
	https://dabbler.dev/posts/2021/05/03/


[Plex transcoding GPU selection chart](https://www.elpamsoft.com/?p=Plex-Hardware-Transcoding)

Additional Guides 
- [TRaSH Guides - Highly Recommended](https://trash-guides.info/Plex/)
- [MediaClients.wiki - Great tips to tune your plex settings for optimal playback and server load](https://mediaclients.wiki/en/Plex)


## Baseline Plex on TrueNAS Scale

![[Plex_truenas_scale_Chart_1.png]]![[Plex_truenas_scale_Chart_2.png]]![[Plex_truenas_scale_Chart_3.png]]![[Plex_truenas_scale_Chart_4.png]]