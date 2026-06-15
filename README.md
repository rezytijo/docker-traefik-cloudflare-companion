# docker-traefik-cloudflare-companion

[![GitHub release](https://img.shields.io/github/v/tag/tiredofit/docker-traefik-cloudflare-companion?style=flat-square)](https://github.com/tiredofit/docker-traefik-cloudflare-companion/releases/latest)
[![Build Status](https://img.shields.io/github/actions/workflow/status/tiredofit/docker-traefik-cloudflare-companionmain.yml?branch=main&style=flat-square)](https://github.com/tiredofit/docker-traefik-cloudflare-companion.git/actions)
[![Docker Stars](https://img.shields.io/docker/stars/tiredofit/traefik-cloudflare-companion.svg?style=flat-square&logo=docker)](https://hub.docker.com/r/tiredofit/traefik-cloudflare-companion/)

## Deskripsi
Project ini akan mem-build *Docker image* yang berfungsi untuk secara otomatis memperbarui *Cloudflare DNS records* (berupa CNAME) saat *container* baru terdeteksi berjalan, dikhususkan untuk penggunaan bersama dengan [Traefik Reverse Proxy](https://github.com/traefik/traefik). Aplikasi ini memonitor kejadian (*events*) dari Docker, Docker Swarm, maupun Traefik Polling untuk membuat pembaruan ke sisi server Cloudflare.

## Requirements
- **Cloudflare Account**: Global API key atau Scoped API key aktif.
- **Docker**: Akses ke `/var/run/docker.sock` untuk deteksi kontainer, atau klaster Docker Swarm.
- **Traefik**: v1 atau v2 (sebagai proksi yang dideteksi labelnya).

## Quick Start
```bash
git clone https://github.com/tiredofit/docker-traefik-cloudflare-companion.git
cd docker-traefik-cloudflare-companion
cp .env.example .env

# Edit .env file sesuai dengan preferensi Anda, setidaknya isikan CF_TOKEN.
nano .env

# Menjalankan kontainer melalui Docker Compose
docker compose up -d
```

## Environment Variables
Detail konfigurasi (termasuk variabel *Docker Options*, *Cloudflare Options*, dan *Traefik Options*) dapat dilihat dengan menyalin dan merujuk ke berkas bawaan **`.env.example`** di _root directory_.

## Project Structure
Struktur utama dari proyek ini meliputi:
- `docs/` : Folder dokumentasi spesifik terkait arsitektur, testing, troubleshooting, dsb.
- `install/` : Skrip dan file konfigurasi s6-overlay serta inisiasi servis internal Docker Alpine.
- `examples/` : Contoh penggunaan dengan `docker-compose.yml`.
- `Dockerfile` : Blueprint pembentukan Alpine Linux container untuk script Python Cloudflare ini.

## Running Test
Aplikasi ini dapat dilakukan pengetesan sistem tanpa risiko mengubah data Cloudflare (*dry run*):

```bash
# Ubah pada berkas .env Anda untuk mengaktifkan:
DRY_RUN=TRUE

# Restarts & monitor log
docker compose up -d
docker compose logs -f
```

*(Penting: Saat skrip sudah membaca routing DNS dengan benar, ingat untuk kembalikan `DRY_RUN` ke `FALSE`)*.

## Documentation
Dokumentasi detail telah dipisah untuk mencegah *noise*. Segala informasi lanjut tersedia di folder `/docs`:
- [Overview](docs/overview.md)
- [Architecture & Discovery](docs/architecture.md)
- [Database & State](docs/database.md)
- [API Integration](docs/api.md)
- [Deployment](docs/deployment.md)
- [Testing](docs/testing.md)
- [Troubleshooting](docs/troubleshooting.md)

## Contributor
- [Dave Conroy](http://github/tiredofit/) - *Maintainer* & *Sponsor*
