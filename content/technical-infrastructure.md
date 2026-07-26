---
title: "Technical Infrastructure"
description: "Server admin details, hardware, and Docker setup"
date: 2026-07-26
---

The Infinity Void server is built for performance and reliability on a Dell PowerEdge T40 using a containerized approach within a WSL/Debian environment.

| Component | Implementation |
|-----------|---------------|
| Host OS | Windows 11 with WSL (Debian) |
| Containerization | Podman |
| Server Core | PaperMC |
| Network Access | wisdom-forbidden.gl.joinmc.link |

## Hardware Specifications

- **CPU**: Intel(R) Core(TM) i3-8300 CPU @ 3.70GHz (4 Cores)
- **Total System RAM**: 16 GB
- **Allocated Memory**: The Minecraft server container is explicitly allocated 10GB of RAM.

## Logs and Updates

### Minecraft Runtime Logs
To view the live console log of players joining, server errors, or plugin activity, run the following command on the host machine:
```bash
podman logs paper-server
```
You can also tail the logs continuously by adding the `-f` flag (`podman logs -f paper-server`).

### Game Updates
The server uses the `docker.io/itzg/minecraft-server:latest` image and the `PAPER` type. It automatically pulls and applies the latest PaperMC updates for the supported Minecraft version when the container is restarted. You can verify the current version and any update processes during startup by checking the runtime logs mentioned above.

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
      MEMORY: "10G"
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
