# Testing

## Uji Coba Manual Secara Langsung (Dry Run Mode)

Proyek ini dibuat sangat terspesialisasi untuk berinteraksi dengan API Cloudflare di kondisi *production*. Namun demikian, saat pertama kali mengonfigurasikannya dengan proksi Anda, Anda disarankan untuk melakukan *testing* terlebih dahulu untuk mencegah penghapusan/perubahan tak terduga (*accidental replacement*) pada data *live* domain DNS Anda. 

Fitur utama untuk melakukan pengetesan (*testing*) ini dinamakan **Dry Run Mode**. Mode ini menginstruksikan Python untuk mensimulasikan pencarian domain dan proses *API Calls* ke server, mencetak seluruh laporan lognya, tetapi diblokir untuk memodifikasi apapun ke server asal Cloudflare.

### Menjalankan Test

Buka `.env` Anda atau *docker-compose* Anda dan berikan nilai *TRUE* pada variabel DRY_RUN.

```bash
DRY_RUN=TRUE
```

Kemudian di konsol aplikasi silakan cek baris log untuk memastikan bahwa variabel *host* Traefik berhasil dikenali dan divalidasi ke format DNS.

## Pengujian Terotomatisasi (Real-flow test)

Karena ini adalah *wrapper* Python untuk skrip integrasi Docker events dan layanan Cloudflare, jika Anda berkontribusi pada *codebase*, pembuatan *mock container* secara lokal tanpa perlu kredensial sungguhan Cloudflare dapat dilakukan dengan *unit test* spesifik menggunakan pustaka python bawaan di skrip pengujian (tidak dikemas secara bawaan pada berkas produksi Docker image).
