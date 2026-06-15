# Database

## State Management
Aplikasi `docker-traefik-cloudflare-companion` tidak memiliki sistem *database* konvensional (misal: MySQL, PostgreSQL, ataupun SQLite) untuk menyimpan data persistensi di internal sistemnya.

Sebagai gantinya, seluruh fungsi manajemen *state* dikendalikan berdasarkan:
1. **Docker Daemon** via `/var/run/docker.sock`: Sistem membaca status aktif (*running containers*), label yang terpasang pada suatu *container*, serta *events* yang dikirimkan dari mesin Docker. 
2. **Cloudflare DNS API**: Sistem memverifikasi eksistensi dan mencocokkan *DNS Records* (termasuk *Zone ID* dan nilai _Target_ proksi dari CNAME records). Status ini selalu tersinkronisasi dua arah via API.

## File Persistance Storage
Satu-satunya sumber berkas konstan pada direktori lokal yang diperlukan agar program ini berfungsi penuh adalah `socket` sistem yang dipasang di `/var/run/docker.sock`. Volume file ini wajib di-*bind mount* saat inisiasi *container* agar sistem punya otorisasi memantau jaringan Docker lokal.
