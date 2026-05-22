---
title: "Technical Infrastructure"
description: "Server admin details, hardware, and Docker setup"
date: 2026-05-22
---

The Infinity Void server is built for performance and reliability on a Dell PowerEdge T40 using a containerized approach within a WSL/Debian environment.

| Component | Implementation |
|-----------|---------------|
| Host OS | Windows 11 with WSL (Debian) |
| Containerization | Podman |
| Server Core | PaperMC |
| Network Access | wisdom-forbidden.gl.joinmc.link |

## Configuration: compose.yaml

```yaml
networks:
  mc-net:
    driver: bridge

services:
  mc:
    image: docker.io/itzg/minecraft-server:latest
    container_name: paper-server
    networks:
      - mc-net
    environment:
      EULA: "TRUE"
      TYPE: "PAPER"
      MEMORY: "4G"
    volumes:
      - ./mc-data:/data:Z
    ports:
      - "25565:25565"
      - "24454:24454/udp"
    restart: unless-stopped

  playit:
    image: ghcr.io/playit-cloud/playit-agent:latest
    container_name: playit-tunnel
    networks:
      - mc-net
    volumes:
      - ./playit-data:/etc/playit:Z
    environment:
      - SECRET_KEY=secret-key
    restart: unless-stopped
```
