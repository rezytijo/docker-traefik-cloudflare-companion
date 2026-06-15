# API Integration

Proyek ini tidak mengekspos (*expose*) API internal untuk diakses dari luar. Namun, aplikasi ini secara konstan berkomunikasi dengan dua jenis API eksternal yang diotentikasi dan dikonfigurasi melalui *environment variables*.

## 1. Cloudflare API
Digunakan untuk membuat dan memodifikasi DNS records secara otomatis berdasarkan pembacaan rute terbaru dari Traefik. Terdapat dua jenis otentikasi yang disediakan:
- **Global API Key**: Membutuhkan `CF_EMAIL` yang berisi email Cloudflare Anda, dan `CF_TOKEN` yang berisi kode _Global_ token-nya.
- **Scoped API Token**: Merupakan praktik terbaik karena bisa dibatasi izinnya untuk mengelola hanya beberapa zona DNS saja. Cukup atur `CF_TOKEN` dengan token tersebut, kosongkan variabel `CF_EMAIL`.

## 2. Traefik API (Mode Polling)
Jika Anda menggunakan mode *Traefik Polling* (menggantikan mode *Docker socket events* default), *companion* akan mengotentikasi dan mengambil daftar rute domain dari REST API Traefik.
- Harus diatur `ENABLE_TRAEFIK_POLL=TRUE` dan `TRAEFIK_VERSION=2`.
- `TRAEFIK_POLL_URL` adalah URL alamat lengkap (misalnya `http://192.168.1.100:8080`) menuju rute *dashboard* API internal milik Traefik. 
- *Companion* secara asinkron memanggil API Traefik secara berkala berdasarkan variabel `TRAEFIK_POLL_SECONDS` (default: `60`).

## 3. Docker Secrets
Untuk menjaga integritas keamanan saat menjalankan sistem ini, kredensial koneksi seperti `CF_EMAIL` dan `CF_TOKEN` telah secara penuh mendukung penggunaan **Docker Secrets**.
- Berikan penamaan `CF_EMAIL` atau `cf_email` pada variabel konfigurasi rahasia saat memasang via *swarm*.
- Berikan penamaan `CF_TOKEN` atau `cf_token` untuk token rahasianya.
