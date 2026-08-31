# Rancang Bangun Sistem Informasi Konseling Siswa-Guru BK Di SMAN DARUSSHOLAH SINGOJURUH

## Install

```bash
composer install
cp .env.example .env
php artisan key:generate

# Set DB di .env (MySQL):
# DB_DATABASE=bk_system
# DB_USERNAME=root
# DB_PASSWORD=

php artisan migrate
php artisan db:seed          

php artisan storage:link
php artisan serve
```

**Sanctum** sudah ditambahkan di `composer.json`. Setelah `composer install` jalankan:

```bash
php artisan vendor:publish --provider="Laravel\Sanctum\SanctumServiceProvider"
php artisan migrate
```

Buka: http://127.0.0.1:8000

## Menjalankan test

Test otomatis (PHPUnit/Feature test) memakai SQLite in-memory lewat
`phpunit.xml` — tidak menyentuh database MySQL di `.env`, jadi aman
dijalankan kapan saja tanpa setup tambahan:

```bash
php artisan test
# atau
vendor/bin/phpunit
```
Penggunaan sebagai Siswa

Siswa dapat mengajukan konseling, melihat status konseling, melakukan chat dengan Guru BK untuk konseling daring, serta menggunakan chatbot AI.

Mengajukan Konseling
Login sebagai Siswa.
Masuk ke menu Konseling.
Pilih pengajuan konseling.
Isi data konseling.
Pilih metode konseling, yaitu daring atau luring.
Pilih Guru BK sesuai kebutuhan.
Kirim pengajuan.
Melihat Status Konseling

Buka menu Status untuk melihat perkembangan pengajuan konseling.

Chat Konseling

Untuk konseling daring yang telah dikonfirmasi:

Buka detail konseling.
Masuk ke ruang chat.
Kirim pesan kepada Guru BK.
Riwayat percakapan akan tersimpan pada sistem.
Chatbot AI

Chatbot dapat membantu siswa terkait enam kategori:

Akademik
Sosial
Pribadi
Karir
Bullying
Keluarga

Chatbot merupakan sistem pendukung dan tidak menggantikan peran Guru BK.

Penggunaan sebagai Guru BK

Guru BK dapat:

Melihat daftar pengajuan konseling.
Mengonfirmasi konseling.
Mencatat konseling luring atau walk-in.
Mengisi laporan hasil konseling.
Berkomunikasi melalui chat dengan siswa.
Mengelola data siswa.
Melakukan import data siswa.
Mengelola informasi BK.
Mengatur jadwal rutin.
Melihat notifikasi.
Mencetak laporan.
Konseling Walk-In

Untuk mencatat siswa yang datang langsung:

Login sebagai Guru BK.
Buka menu Konseling.
Pilih Walk-In.
Pilih data siswa.
Isi informasi konseling.
Simpan data.
Penggunaan sebagai Kepala Sekolah

Kepala Sekolah digunakan untuk melakukan monitoring terhadap layanan BK.

Fitur yang tersedia antara lain:

Dashboard.
Rekap Guru BK.
Monitoring data konseling.
Melihat detail konseling sesuai hak akses.
Statistik.
Informasi BK.
Penggunaan sebagai Admin

Admin digunakan untuk mengelola akun dan administrasi sistem.

Fitur utama:

Dashboard Admin.
Manajemen akun Guru BK.
Manajemen akun Kepala Sekolah.
## API Auth

```
POST /api/login
{ "role": "siswa", "nis": "<nis_siswa>", "password": "<password_siswa>" }
→ { "token": "...", "role": "siswa", "user": {...} }

Header: Authorization: Bearer <token>
```

(Nilai `<nis_siswa>` dan `<password_siswa>` di atas hanya placeholder format
request — bukan kredensial yang benar-benar aktif.)

## Struktur penting

```
app/Http/Controllers/Web/   # Auth, Dashboard, Konseling, ...
app/Http/Controllers/Api/   # Sanctum-protected API
app/Http/Controllers/Concerns/AuthorizesBk.php
app/Models/                 # Siswa, GuruBk, Konseling (+ HasApiTokens)
resources/views/            # Blade templates
routes/web.php              # Route web + role middleware
routes/api.php              # Route API + auth:sanctum
```
