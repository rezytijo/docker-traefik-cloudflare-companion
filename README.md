# docker-traefik-cloudflare-companion

[![GitHub release](https://img.shields.io/github/v/tag/tiredofit/docker-traefik-cloudflare-companion?style=flat-square)](https://github.com/tiredofit/docker-traefik-cloudflare-companion/releases/latest)
[![Build Status](https://img.shields.io/github/actions/workflow/status/tiredofit/docker-traefik-cloudflare-companionmain.yml?branch=main&style=flat-square)](https://github.com/tiredofit/docker-traefik-cloudflare-companion.git/actions)
[![Docker Stars](https://img.shields.io/docker/stars/tiredofit/traefik-cloudflare-companion.svg?style=flat-square&logo=docker)](https://hub.docker.com/r/tiredofit/traefik-cloudflare-companion/)

Translations: **English** | [Bahasa Indonesia](docs/README.id.md) | [中文](docs/README.zh.md) | [Русский](docs/README.ru.md)

---

## Description
This project builds a *Docker image* that automatically updates *Cloudflare DNS records* (CNAME) when a new container is detected running, specifically designed for use with [Traefik Reverse Proxy](https://github.com/traefik/traefik). The application monitors events from Docker, Docker Swarm, or Traefik Polling to make updates to the Cloudflare server side.

## Requirements
- **Cloudflare Account**: Active Global API key or Scoped API key.
- **Docker**: Access to `/var/run/docker.sock` for container detection, or Docker Swarm cluster.
- **Traefik**: v1 or v2 (as the proxy whose labels are detected).

## Quick Start
```bash
git clone https://github.com/tiredofit/docker-traefik-cloudflare-companion.git
cd docker-traefik-cloudflare-companion
cp .env.example .env

# Edit .env file to your preferences, at minimum fill in CF_TOKEN.
nano .env

# Run the container via Docker Compose
docker compose up -d
```

## Environment Variables
Configuration details (including *Docker Options*, *Cloudflare Options*, and *Traefik Options*) can be found by copying and referring to the built-in **`.env.example`** file in the _root directory_.

## Project Structure
The main structure of this project includes:
- `docs/` : Folder for specific documentation on architecture, testing, troubleshooting, etc.
- `install/` : s6-overlay scripts and configuration files along with internal Docker Alpine service initialization.
- `examples/` : Usage examples with `docker-compose.yml`.
- `Dockerfile` : Blueprint for forming an Alpine Linux container for this Python Cloudflare script.

## Running Tests
This application can be tested without risk of changing Cloudflare data (*dry run*):

```bash
# Change in your .env file to enable:
DRY_RUN=TRUE

# Restart & monitor logs
docker compose up -d
docker compose logs -f
```

*(Important: Once the script has read the DNS routing correctly, remember to set `DRY_RUN` back to `FALSE`)*.

## Documentation
Detailed documentation has been separated to prevent noise. All further information is available in the `/docs` folder:
- [Overview](docs/overview.md)
- [Architecture & Discovery](docs/architecture.md)
- [Database & State](docs/database.md)
- [API Integration](docs/api.md)
- [Deployment](docs/deployment.md)
- [Testing](docs/testing.md)
- [Troubleshooting](docs/troubleshooting.md)

## Contributors
- [Dave Conroy](http://github/tiredofit/) - *Maintainer* & *Sponsor*

