# Overview

## Deskripsi

Project ini adalah sebuah _companion container_ untuk [Traefik](https://github.com/traefik/traefik) Reverse Proxy. Fungsi utamanya adalah untuk secara otomatis memperbarui DNS record (khususnya CNAME) di [Cloudflare](https://www.cloudflare.com/) ketika *container* target baru dijalankan dan diarahkan melalui Traefik. 

Hal ini memudahkan pengelolaan domain dan subdomain yang diproksi secara otomatis tanpa harus masuk ke dashboard Cloudflare untuk melakukan pembaruan DNS manual.

## Fitur Utama
- Deteksi dinamis dan sinkronisasi otomatis *routing* dari Traefik menuju Cloudflare.
- Dukungan untuk beberapa domain sekaligus dalam satu instansi.
- Mendukung Traefik versi 1 dan versi 2.
- Menggunakan mode *discovery* yang beragam: Docker events, Docker Swarm, atau Traefik Polling.
- Opsi `DRY_RUN` untuk uji coba tanpa benar-benar memodifikasi DNS records Cloudflare.

## Prerequisites and Assumptions
- Mengasumsikan Anda memiliki *Global* API key atau *Scoped* API key dari akun Cloudflare.
- Mengasumsikan Anda menggunakan Traefik sebagai *reverse proxy*.
