Infinity Void: Comprehensive Server Documentation

1. Technical Infrastructure (Server Admin)

The Infinity Void server is built for performance and reliability on a Dell PowerEdge T40 using a containerized approach within a WSL/Debian environment.

Component

Implementation

Host OS

Windows 11 with WSL (Debian)

Containerization

Podman

Server Core

PaperMC

Network Access

wisdom-forbidden.gl.joinmc.link

Configuration: compose.yaml

The infrastructure is managed via the following configuration:

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


2. Server Plugin Roadmap

The server leverages a modular plugin architecture to expand functionality.

Active: Simple Voice Chat (Proximity audio).

Pipeline (Planned Upgrades):

EssentialsX: Core server management.

LuckPerms: Permissions and rank management.

CoreProtect: Accountability logs for block placement and chest access.

BlueMap: 3D interactive web-based world viewer.

GeyserMC: Cross-platform support.

GriefProtection / Towny: Land ownership and nation-building tools.

DiscordSRV: Integration between in-game chat and community channels.

Automated Backups: Proactive system-level data protection.

3. Client-Side Onboarding

Participants must install the Simple Voice Chat mod for proximity audio.

Recommended Setup (Modrinth App)

Install the [Modrinth App](https://modrinth.com/app).

Create a new instance and select Fabric as your loader.

![Create Instance](/Create_Instance.png)
![Custom Setup](</Custom setup.png>)
![Instance details](</Instance details.png>)

Search for and install Simple Voice Chat.

![Browse Content](</Browse content.png>)
![Simple Voice Chat](</Simple Voice Chat.png>)

Worlds → Add a Server

![Add a server](</Add a Server.png>)

Launch the game - Play ▶️

![Play](/Play.png)

Manual Installation

Install the Fabric Loader.

Download the Simple Voice Chat .jar file matching Minecraft 26.1.2.

Place the file in your local mods folder:

Windows: %appdata%\.minecraft\mods

Linux: ~/.minecraft/mods

4. Community Guidelines

Respect the Build: Do not modify structures without owner consent.

Collaboration: Infinity Void is a space for collective architectural growth in El Valle de Antón.

Accountability: All block changes are logged for safety.

Respectful Communication: No hate speech, racism, sexism, homophobia, harassment, or personal attacks of any kind.

Appropriate Content: No NSFW/sexual, graphic, or otherwise inappropriate media or comments in chat.

No Spamming: Keep chat clean and readable. Do not spam messages, commands, or emojis.

Fair Play: No cheating, exploiting, client-side mods that give unfair advantages, or abusing bugs. Report any issues to admins.
