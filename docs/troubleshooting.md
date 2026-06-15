# Troubleshooting

Jika Anda menghadapi kendala saat sistem ini tersambung ke jaringan *host* dan Cloudflare, ikuti prosedur berikut:

## 1. Memeriksa Sistem Log
Sistem log terintegrasi secara otomatis, dikendalikan oleh s6-overlay dan variabel lingkungan tertentu:
- `LOG_LEVEL`: Silakan ubah variabel ini menjadi `VERBOSE` atau `DEBUG` agar sistem mencetak semua langkah deteksi dan respons asisten. Default adalah `INFO`.
- `LOG_TYPE`: Secara bawaan `BOTH` (Console dan File log).
- Jika ada *error* komunikasi HTTP ke server Cloudflare, tingkat log `DEBUG` akan memberikan kode status HTTP yang bermanfaat dari API.

## 2. Shell Access (Akses ke dalam Container)
Bila Anda perlu memverifikasi apakah kontainer berhasil mengotentikasi, menjalankan perintah `curl` sendiri ke Cloudflare API, ataupun memeriksa konfigurasi file sementara:

```bash
docker exec -it <nama-container> bash
```

Anda dapat mengeksekusi `ps aux` atau menggunakan alat bawaan `nano` dan `less` dari direktori basis Alpine.

## 3. Masalah Umum (Common Issues)

### Permission Denied pada `/var/run/docker.sock`
Jika Anda menerima log peringatan bahwa program tidak bisa tersambung ke Docker, hal ini sering terjadi jika user *container* (`TCC_USER`) tidak diberikan hak memadai di dalam klaster host. Ubahlah nilai *environment* `TCC_USER` menjadi `root` kembali jika sebelumnya dimodifikasi. Pastikan Anda telah menempatkan volume bind `/var/run/docker.sock`.

### Record CNAME Tidak Berubah / Berganti
- Pastikan bahwa CNAME yang dimaksud bukan sudah ada dengan status proteksi yang lebih ketat di Cloudflare.
- Cobalah mengatur environment `REFRESH_ENTRIES=TRUE`. Secara standar sistem tidak menimpa pengaturan apabila ditemukan domain yang sama tapi parameternya berbeda (nilai default adalah `FALSE`).

## Dukungan Tambahan
Jika sistem tetap tidak bekerja setelah menyesuaikan *Log Level* dan pengecekan *Container Socket*:
- Silakan ajukan isu (Bug Report) via [Github Issues](https://github.com/tiredofit/docker-traefik-cloudflare-companion/issues/new).
- Temukan panduan komprehensif atau bantu menjawab kueri orang lain di fitur diskusi: [Discussions board](https://github.com/tiredofit/docker-traefik-cloudflare-companion/discussions).
