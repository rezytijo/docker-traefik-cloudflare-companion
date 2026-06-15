# Architecture

## Teknologi yang Digunakan
Sistem berjalan di dalam sebuah *Docker container* berbasis **Alpine Linux**. Di dalam *container* ini:
- **Python 3** digunakan sebagai bahasa utama untuk mengatur logika sinkronisasi.
- **Docker-Py** digunakan untuk berinteraksi dengan API *Docker socket local*.
- **Cloudflare (via pip)** digunakan untuk berinteraksi dengan Cloudflare API (versi 4.x).
- **s6-overlay** digunakan sebagai sistem *init* untuk memastikan *services* berjalan stabil dan dapat dipantau performanya.
- Berbagai peralatan tambahan (seperti `msmtp`, `zabbix-agent`) disertakan dari `tiredofit/alpine` dasar untuk kapabilitas *monitoring* dan *logging* tambahan.

## Discovery Modes
`cloudflare-companion` mendukung tiga mode penemuan (*discovery modes*). Mode yang paling sering digunakan adalah **Docker**, yang aktif secara _default_. Ketika *host* target ditemukan, *companion* akan menambahkan atau memperbarui daftar _CNAME_ di Cloudflare menuju `TARGET_DOMAIN` yang telah dikonfigurasi.

### 1. Docker (Default)
Program memantau *container* Docker lokal menggunakan `/var/run/docker.sock` dan memeriksa *labels* dari tiap *container*. 
- **Traefik v1**: menggunakan parameter `traefik.normal.frontend.rule=Host:domain.tld`
- **Traefik v2**: menggunakan parameter `traefik.http.routers.example.rule=Host('domain.tld')`

### 2. Docker Swarm
Mode Docker Swarm dapat diaktifkan menggunakan parameter *environment* `SWARM_MODE=TRUE`.
Program kemudian akan memeriksa label dari layanan (*services*) di klaster Docker Swarm dengan aturan format label yang sama dengan mode Docker standar.

### 3. Traefik Polling
Alih-alih memantau *Docker Socket*, mode ini secara aktif menanyakan (*polling*) API Traefik secara berkala (misal: setiap 60 detik). Mode ini diaktifkan dengan `TRAEFIK_VERSION=2`, `ENABLE_TRAEFIK_POLL=TRUE`, dan URL tujuan `TRAEFIK_POLL_URL=http://<host>:<port>`.

*Companion* mencari parameter *host* yang memenuhi kondisi:
1. *Provider* bukan dari docker.
2. Status adalah *enabled*.
3. Nama tersedia.
4. *Rule* mengandung `Host(...)`.
5. Memenuhi kriteria parameter *Include* (default `.*`).
6. Tidak memenuhi kriteria parameter *Exclude*.

## Filtering & Rules
Fitur penyaringan (*filtering*) bisa dilakukan dengan mode yang spesifik.

### Polling Mode
- **Include Patterns**: Parameter `TRAEFIK_INCLUDED_HOST<XXX>` (misal: `.*-data\.foobar\.com`) untuk mengeksekusi sinkronisasi pada domain tertentu.
- **Exclude Patterns**: Parameter `TRAEFIK_EXCLUDED_HOST<XXX>` memberikan kontrol penyaringan domain apa saja yang dilarang sinkronisasi ke Cloudflare, parameter ini memiliki prioritas tertinggi di sistem _filter_.

### Docker Endpoint Mode
- **By Label**: Menggunakan parameter `TRAEFIK_FILTER_LABEL` dan `TRAEFIK_FILTER`. Digunakan khusus untuk mengeksekusi *container* tertentu. Sangat membantu jika terdapat berbagai instalasi Traefik berbeda, atau lebih dari satu *cloudflare-companion* dalam satu klaster.
