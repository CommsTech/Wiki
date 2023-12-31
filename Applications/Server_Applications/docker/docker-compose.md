---
title: Docker Compose
description: 
dateCreated: 
published: 
editor: markdown
tags:
  - Docker
  - Virtualization
dateModified: 
---
# Docker-Compose
...

## Networking
By default Docker-Compose will create a new network for the given compose file. You can change the behavior by defining custom networks in your compose file.
### Create and assign custom network
...
*Example:*
```yaml
networks:
  custom-network:

services:
  app:
    networks:
      - custom-network
```
### Use existing networks
If you want to use an existing Docker network for your compose files, you can add the `external: true` parameter in your compose file
*Example:*
```yaml
networks:
  existing-network:
    external: true
```

## Volumes
Volumes allow Docker containers to use persistent storage. In a compose file, you can create and map volumes like this:
```yaml
volumes:
  my-volume:

services:
  app:
    volumes:
      - my-volume:/path-in-container
```

These volumes are stored in `/var/lib/docker/volumes`.


Note: It's worth noting that regular cleanup of unused Docker images and containers is important for maintaining a clean and efficient Docker environment.

Note: The tags include "DiskSpaceOptimization" and "PerformanceOptimization" as cleaning up unused Docker resources can help free up disk space and improve system performance.