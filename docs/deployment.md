# Deployment

Aplikasi `docker-traefik-cloudflare-companion` secara utuh dibangun di atas citra Docker (Docker image). Karena sifatnya yang *stateless* dan murni bereaksi terhadap *events* Traefik, distribusinya sangat mudah.

## Image Release
Terdapat dua *registry* yang dapat diakses:
- Docker Hub: `docker.io/tiredofit/traefik-cloudflare-companion`
- Github Container Registry: `ghcr.io/tiredofit/docker-traefik-cloudflare-companion`

Sistem secara penuh mendukung *Multi Architecture* (berjalan natif di arsitektur `amd64`, `arm/v6`, `arm/v7`, `arm64`). Tag bawaannya adalah `:latest` (berbasis Alpine).

## Environment Production

Pendekatan tercepat untuk mengimplementasikan di tahap *production* adalah menggunakan berkas `docker-compose.yml`.

### Contoh docker-compose.yml Dasar

```yaml
version: '3.8'

services:
  cloudflare-companion:
    image: tiredofit/traefik-cloudflare-companion:latest
    container_name: traefik-cloudflare-companion
    restart: always
    environment:
      - CF_TOKEN=super_rahasia_token_cloudflare_anda
      - TARGET_DOMAIN=server.domain.com
      - TRAEFIK_VERSION=2
      # Opsional
      # - DRY_RUN=FALSE
      # - LOG_LEVEL=INFO
    volumes:
      # Wajib diaktifkan untuk Discovery mode Docker Socket
      - /var/run/docker.sock:/var/run/docker.sock:ro
```

### Menjalankan Container

Pindahkan (copy) file `.env.example` ke `.env` terlebih dahulu agar variabel lokal dikenali. Setelah `docker-compose.yml` disesuaikan, jalankan aplikasi ini layaknya layanan Docker pada umumnya:

```bash
docker compose up -d
```

### Pertimbangan di Lingkungan Production
- Pastikan berkas `.env` atau `docker-compose.yml` Anda diamankan. Jika Anda berada di lingkungan kolaboratif atau klaster *Swarm*, manfaatkan Docker Secrets sebagai ganti *hardcode environment variables*.
- Mode *read-only* (`:ro`) pada *bind mount* berkas `/var/run/docker.sock` sangat direkomendasikan karena skrip ini sama sekali tidak perlu membuat atau memodifikasi *containers* baru secara manual, melainkan cukup membacanya saja.
