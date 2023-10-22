# Docker Home Server

This is a Docker container stack for a home server. It enables torrents to be downloaded automatically in one click via the Overseerr interface.

For Overseerr to work properly, you need to install Plex, Radarr and/or Sonarr, Jackett, Transmission and finally Flaresolverr. These interconnected containers will allow you to download torrents from various pre-configured sites.

#### Features of the media server:
- **overseerr**: downloadable media library
- **jackett**: translates queries from apps (Sonarr, Radarr) into tracker-site-specific http queries
- **flaresolverr**: bypass Cloudflare and DDoS-GUARD protection
- **radarr**: monitor multiple RSS feeds for new movies
- **sonarr**: monitor multiple RSS feeds for new episodes of your favorite shows
- **transmission**: torrent client downloader
- **plex**: organizes video, music and photos from personal media libraries

#### Other features include
- **homepage**: create a homepage to access your different services with a single click (like the screenshot below)
- **pihole**: create a DNS server to block ads on your network
- **kuma**: allows you to monitor your various containers to obtain uptime statistics.
- **speedtest**: runs a speedtest at regular intervals for statistical purposes
- **unifi**: manages the Ubiquiti device suite.
- **wireguard**: VPN allowing access to your local network from the outside.

The configuration available in homepage config folder is a custom configuration that I made for my own use. It is not necessary to use it, you can use the default configuration of the homepage container and customize it.

![image](https://github.com/davidmonnom/docker-home-server/assets/63289800/e80c6626-da5b-4940-8c92-44cf51aa462e)

## Docker container
Docker containers present in this configuration:

#### Necessary for the media server
- [overseerr `linuxserver/overseerr:latest`](https://github.com/linuxserver/docker-overseerr) x86-64 - arm64
- [jackett `linuxserver/jackett:latest`](https://github.com/linuxserver/docker-jackett) x86-64 - arm64
- [flaresolverr `ghcr.io/flaresolverr/flaresolverr:latest`](https://github.com/FlareSolverr/FlareSolverr) x86 - x86-64 - arm32 - arm64
- [radarr `linuxserver/radarr:latest`](https://github.com/linuxserver/docker-radarr) x86-64 - arm64
- [sonarr `linuxserver/sonarr:latest`](https://github.com/linuxserver/docker-sonarr) x86-64 - arm64
- [transmission `linuxserver/transmission:latest`](https://github.com/linuxserver/docker-transmission) x86-64 - arm64
- [plex `linuxserver/plex:latest`](https://github.com/linuxserver/docker-plex) x86-64 - arm64

#### Optional
If you don't want some of the optional containers, you can delete them from the `docker-compose.yml` file.

- [homepage `ghcr.io/gethomepage/homepage:latest`](https://github.com/gethomepage/homepage) amd64 - arm64 - armV7 - armV6
- [pihole `pihole/pihole:latest`](https://hub.docker.com/r/pihole/pihole) x86 - x86-64 - arm64 - armV7 - armV6
- [kuma `louislam/uptime-kuma:latest`](https://github.com/louislam/uptime-kuma) x86-64 - arm64
- [speedtest `henrywhitaker3/speedtest-tracker:latest`](https://hub.docker.com/r/henrywhitaker3/speedtest-tracker)
- [unifi `jacobalberty/unifi:latest`](https://hub.docker.com/r/jacobalberty/unifi) amd64 - arm32 - arm64
- [wireguard `linuxserver/wireguard:latest`](https://github.com/linuxserver/docker-wireguard) x86-64 - arm64

## Configuration
First of all, the different containers will need different folders to store their files.
Create it with the following command:

```bash
mkdir -p ~/docker/{unifi,speedtest,kuma,portainer/data,homepage/config,jackett/config,overseerr/config,pihole/config,pihole/etc-dnsmasq.d,plex/transcode,plex/config,radarr/config,sonarr/config,wireguard/config,transmission/config,transmission/watch}
```

```bash
.
├── ...
├── torrents/                # Directory for Torrent files
├── docker/                  # Directory for Docker containers and configurations
│   ├── unifi/               # Unifi Docker files
│   ├── speedtest/           # Speedtest Docker files
│   ├── kuma/                # Kuma Docker files
│   ├── portainer/
│   │   └── data/            # Portainer data files
│   ├── homepage/
│   │   └── config/          # Configuration for the Homepage Docker container
│   ├── jackett/
│   │   └── config/          # Configuration for the Jackett Docker container
│   ├── overseerr/
│   │   └── config/          # Configuration for the Overseerr Docker container
│   ├── pihole/
│   │   ├── config/          # Configuration for the Pihole Docker container
│   │   └── etc-dnsmasq.d/   # Pihole DNS configuration files
│   ├── plex/
│   │   ├── transcode/       # Plex transcoding files
│   │   └── config/          # Configuration for the Plex Docker container
│   ├── radarr/
│   │   └── config/          # Configuration for the Radarr Docker container
│   ├── sonarr/
│   │   └── config/          # Configuration for the Sonarr Docker container
│   ├── wireguard/
│   │   └── config/          # Configuration for the Wireguard Docker container
│   └── transmission/
│       ├── config/          # Configuration for the Transmission Docker container
│       └── watch/           # Transmission watch directory
└── ...
```

## Installation
You have two choices, you can manage your installation via Portainer CE or via command line via docker directly.

#### Installation via Portainer
You must first initialize the Portainer container before initializing other containers directly in the Portainer application.

```yaml
version: '3'
services:
  portainer:
    image: portainer/portainer-ce:latest
    container_name: portainer
    ports:
      - "8000:8000"
      - "9443:9443"
    restart: always
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - /home/kadiix/docker/portainer/data:/data
```

Via your server's CLI, go to the Portainer folder and create a `docker-compose.yml` file and insert the above content into it.

Launch the container with the command `docker-compose up -d`.

Once Portainer is installed and launched, go to its interface and create a Stack with the contents of the repo's `docker-compose.yml` (don't forget to change {USERNAME} by your username).

#### Installation via CLI
Via your server's CLI, go to your Home directory and create a `docker-compose.yml` file and insert the contents of the repo's `docker-compose.yml` (don't forget to change {USERNAME} by your username) into it.

Launch the stack with the command `docker stack deploy --compose-file docker-compose.yml`.

Note that with this method, Portainer will not be installed.

### Configuration of the different containers
To configure all of your newly installed containers, you can use the different links available at the top of the page.
