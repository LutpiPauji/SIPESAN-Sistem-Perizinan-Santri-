# SI-PESAN

## Sistem Perizinan Santri

SI-PESAN adalah aplikasi berbasis **Laravel 13** untuk mengelola proses perizinan santri secara terstruktur, mulai dari pengajuan, persetujuan, pencetakan surat, penggunaan izin, pengembalian surat, keterlambatan, sanksi, monitoring, hingga laporan.

Aplikasi ini dirancang untuk penggunaan internal pondok/pesantren dengan alur persetujuan yang dapat disesuaikan melalui master data dan pengaturan sistem.

## Fitur Utama

### Dashboard

Dashboard menampilkan ringkasan operasional yang terhubung langsung dengan transaksi, antara lain:

- Total santri aktif
- Pengajuan izin hari ini
- Santri sedang izin
- Lewat batas waktu
- Menunggu persetujuan
- Perlu perbaikan
- Kembali hari ini
- Siap dicetak
- Ringkasan master data

### Master Data

Master data yang tersedia:

- Muroby
- Asrama
- Kamar
- Ketua Kamar
- Santri
- Pengguna Sistem
- Struktur Pengasuhan
- Periode Penugasan

### Perizinan

Modul perizinan meliputi:

- Pengajuan Baru
- Daftar Pengajuan
- Perlu Perbaikan
- Siap Dicetak
- Pengembalian Surat
- Lewat Batas
- Pembatalan / Revisi
- Cetak Ulang

Alur umum perizinan:

```text
Pengajuan Baru
      ↓
Ketua Asrama
      ↓
Muroby
      ↓
Siap Dicetak
      ↓
Cetak Surat
      ↓
Izin Aktif
      ↓
Pengembalian Surat
      ↓
Selesai / Terlambat
```

Untuk jenis izin **Keluar Kompleks**, Ketua Asrama dapat menjadi approver final sesuai konfigurasi alur persetujuan.

### Monitoring

Monitoring bersifat **read-only** dan digunakan untuk melihat kondisi transaksi tanpa mengubah status.

Menu monitoring:

- Santri Sedang Izin
- Kembali Hari Ini
- Lewat Batas Waktu
- Surat Belum Dikembalikan
- Izin Sakit Aktif
- Evaluasi Sakit
- Izin Darurat
- Sanksi Aktif

### Pengaturan

Menu pengaturan meliputi:

- Pengaturan Umum
- Jenis Izin
- Alasan Izin
- Alasan Darurat
- Alasan Keterlambatan
- Kuota / Periode Izin
- Aturan Sanksi
- Alur Persetujuan
- Format Nomor Surat
- Backup & Restore

## Format Nomor Surat

Nomor surat dapat dikonfigurasi melalui menu pengaturan.

Format:

```text
PREFIX / BULAN / TAHUN / NOMOR
```

Contoh:

```text
S-PZN/PND-ALFLH/08/2026/00001
```

Nomor surat:

- Unik
- Tidak berubah setelah diterbitkan
- Tetap sama ketika cetak ulang
- QR versi lama dinonaktifkan ketika reprint

## Backup, Restore, dan Reset

### Backup

Backup mencakup:

- Data aplikasi
- Transaksi
- Pengguna
- Konfigurasi
- Activity log
- File pada `storage/app/public`

Backup disimpan pada:

```text
storage/app/sipesan-backups
```

Sistem hanya menyimpan **3 backup terbaru**.

### Restore

Restore dapat dilakukan dengan mengunggah file backup SI-PESAN.

Sebelum proses restore dijalankan, sistem otomatis membuat backup kondisi saat ini.

### Reset Data

Reset data akan:

- Menghapus transaksi
- Menghapus activity log
- Menghapus master operasional
- Menghapus file upload
- Menghapus seluruh pengguna selain Admin Default
- Menghapus session/cache/queue/token

Data yang tetap dipertahankan:

- Admin Default
- Role dan permission
- Pengaturan Umum
- Jenis Izin
- Alasan
- Kuota / Periode
- Aturan Sanksi
- Alur Persetujuan
- Format Nomor Surat

> Fitur Reset hanya tersedia untuk **Admin Default bawaan sistem**.

## Laporan

Laporan dapat diekspor ke **Excel** dan **PDF**.

Menu laporan:

- Rekap Perizinan
- Keterlambatan
- Kepatuhan
- Sanksi
- Izin Sakit
- Izin Darurat
- Audit Aktivitas

Nama lembaga pada file PDF dan Excel otomatis diambil dari:

```text
Pengaturan → Pengaturan Umum → Nama Lembaga
```

## Role Pengguna

| Role | Fungsi |
|---|---|
| Admin Pondok | Mengelola sistem, master data, pengaturan, laporan, dan administrasi |
| Keamanan | Mengelola operasional pengajuan dan pengembalian izin |
| Ketua Asrama | Melakukan tahap persetujuan sesuai alur |
| Muroby | Melakukan persetujuan final jika diperlukan |
| Pimpinan | Monitoring dan laporan |

Pembuatan akun manual dari menu **Pengguna Sistem** hanya tersedia untuk:

```text
Admin Pondok
Keamanan
Pimpinan
```

Role **Ketua Asrama** dan **Muroby** mengikuti struktur/penugasan pengasuhan.

## Teknologi

SI-PESAN menggunakan:

- Laravel 13
- PHP 8.5+
- MySQL
- Bootstrap 5
- Bootstrap Icons
- Vite
- Spatie Laravel Permission
- Spatie Activity Log
- DomPDF
- Endroid QR Code
- PhpSpreadsheet

Environment pengembangan:

```text
PHP       : 8.5.x
Laravel   : 13.x
Composer  : 2.x
Node.js   : 26.x
Database  : MySQL
```

## Instalasi Lokal

### Persyaratan

Pastikan komputer sudah memiliki:

- PHP 8.5 atau versi yang kompatibel
- Composer
- MySQL / MariaDB
- Node.js
- NPM
- Laragon direkomendasikan untuk Windows

Contoh lokasi project:

```text
C:\laragon\www\sipesan
```

### 1. Masuk ke Folder Project

```powershell
cd C:\laragon\www\sipesan
```

### 2. Install Dependency PHP

```powershell
composer install
```

> Hindari `composer update` kecuali memang ingin memperbarui versi dependency.

### 3. Install Dependency Frontend

```powershell
npm install
```

### 4. Buat File Environment

Jika `.env` belum ada:

```powershell
copy .env.example .env
```

Contoh konfigurasi lokal:

```env
APP_NAME="SI-PESAN"
APP_ENV=local
APP_KEY=
APP_DEBUG=true
APP_URL=http://127.0.0.1:8000

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=sipesan
DB_USERNAME=root
DB_PASSWORD=
```

### 5. Buat Database

```sql
CREATE DATABASE sipesan
CHARACTER SET utf8mb4
COLLATE utf8mb4_unicode_ci;
```

### 6. Generate Application Key

```powershell
php artisan key:generate
```

### 7. Jalankan Migration

```powershell
php artisan migrate
```

> Jangan gunakan `migrate:fresh`, `migrate:reset`, atau `db:wipe` pada database yang berisi data penting.

### 8. Jalankan Seeder

```powershell
php artisan db:seed
```

Pastikan minimal tersedia:

```text
1 Admin Default
Role admin_pondok
Role keamanan
Role ketua_asrama
Role muroby
Role pimpinan
```

### 9. Buat Storage Link

```powershell
php artisan storage:link
```

### 10. Build Frontend

Development:

```powershell
npm run dev
```

Production:

```powershell
npm run build
```

### 11. Bersihkan Cache

```powershell
php artisan optimize:clear
php artisan view:clear
php artisan route:clear
php artisan config:clear
```

## Menjalankan Aplikasi

### Artisan Serve

```powershell
php artisan serve
```

Buka:

```text
http://127.0.0.1:8000
```

### Laragon Virtual Host

Jika menggunakan Auto Virtual Hosts Laragon:

```text
http://sipesan.test
```

Gunakan:

```env
APP_URL=http://sipesan.test
```

## Urutan Setup Setelah Instalasi

Setelah login sebagai Admin Default, lakukan konfigurasi dengan urutan:

```text
1. Pengaturan Umum
2. Muroby
3. Asrama
4. Kamar
5. Ketua Kamar
6. Santri
7. Struktur Pengasuhan
8. Periode Penugasan
9. Pengguna Sistem
10. Jenis Izin
11. Alasan Izin
12. Alasan Darurat
13. Alasan Keterlambatan
14. Kuota / Periode Izin
15. Aturan Sanksi
16. Alur Persetujuan
17. Format Nomor Surat
```

Setelah master data dan pengaturan selesai, modul Perizinan dapat digunakan.

## Alur Operasional

### Pengajuan

Petugas Keamanan membuat pengajuan melalui:

```text
Perizinan → Pengajuan Baru
```

Sistem melakukan pengecekan otomatis terhadap:

- Kuota / periode izin
- Jeda minimal izin
- Sanksi tambahan
- Pembatasan reguler
- Alur persetujuan

### Persetujuan

Tahap persetujuan menyesuaikan:

```text
Pengaturan → Alur Persetujuan
```

Status umum:

```text
menunggu_ketua_asrama
menunggu_muroby
perlu_perbaikan
siap_dicetak
izin_aktif
menunggu_evaluasi_sakit
terlambat
selesai
ditolak
dibatalkan
```

### Cetak Surat

Jika pengajuan sudah mendapat persetujuan final:

```text
Siap Dicetak
↓
Cetak Surat
```

Saat konfirmasi pencetakan:

```text
printed_at   = waktu server
digunakan_at = waktu server
status       = izin_aktif
```

QR pada surat digunakan untuk verifikasi keaslian surat.

### Pengembalian

Jika kembali tepat waktu:

```text
status = selesai
terlambat_menit = 0
```

Jika terlambat:

```text
status = terlambat
terlambat_menit > 0
```

Selanjutnya petugas dapat memproses alasan keterlambatan dan sanksi.

## QR Verifikasi

QR surat hanya digunakan untuk:

```text
Verifikasi keaslian surat
```

Scanning QR **tidak mengubah**:

- Status izin
- Waktu digunakan
- Waktu kembali
- Status transaksi lainnya

## Perintah Maintenance

Bersihkan cache:

```powershell
php artisan optimize:clear
```

Bersihkan Blade cache:

```powershell
php artisan view:clear
```

Lihat route:

```powershell
php artisan route:list
```

Cek route laporan:

```powershell
php artisan route:list --path=laporan
```

Cek route monitoring:

```powershell
php artisan route:list --path=monitoring
```

Cek route backup:

```powershell
php artisan route:list --path=pengaturan/backup-restore
```

## Troubleshooting

### View Not Found

Contoh:

```text
View [laporan.izin-sakit.index] not found
```

Pastikan file berada pada:

```text
resources/views/laporan/izin-sakit/index.blade.php
```

Kemudian:

```powershell
php artisan view:clear
```

### Route Not Found

```powershell
php artisan route:clear
php artisan route:list
```

### Perubahan Blade Tidak Muncul

```powershell
php artisan view:clear
php artisan optimize:clear
```

Kemudian hard refresh:

```text
Ctrl + Shift + R
```

### File Upload Tidak Bisa Dibuka

```powershell
php artisan storage:link
```

Pastikan tersedia:

```text
public/storage
```

## Keamanan

Beberapa prinsip keamanan yang digunakan:

- Role dan permission menggunakan Spatie Permission
- Transaksi tidak menggunakan hard delete
- Nomor surat bersifat immutable
- QR lama dinonaktifkan saat reprint
- Reset hanya tersedia untuk Admin Default
- Restore meminta password admin
- Backup diperiksa menggunakan checksum
- Password tidak ditampilkan di activity log
- Monitoring bersifat read-only

Untuk server produksi:

```env
APP_ENV=production
APP_DEBUG=false
```

> Jangan menyimpan `.env` ke repository publik.

## Struktur Folder Penting

```text
app/
├── Http/
│   └── Controllers/
├── Models/
└── Services/

resources/
└── views/
    ├── dashboard.blade.php
    ├── master/
    ├── monitoring/
    ├── laporan/
    ├── perizinan/
    └── pengaturan/

routes/
└── web.php

storage/
├── app/
│   ├── public/
│   └── sipesan-backups/
└── logs/

public/
├── favicon.ico
└── storage/
```

## Checklist Instalasi

```text
[ ] composer install
[ ] npm install
[ ] .env dibuat
[ ] APP_KEY dibuat
[ ] database dibuat
[ ] database dikonfigurasi di .env
[ ] php artisan migrate
[ ] php artisan db:seed
[ ] php artisan storage:link
[ ] npm run build / npm run dev
[ ] php artisan optimize:clear
[ ] Admin Default dapat login
[ ] Pengaturan Umum sudah diisi
[ ] Role dan permission tersedia
[ ] Master data dapat digunakan
[ ] Pengajuan izin dapat dibuat
[ ] Persetujuan berjalan
[ ] Surat dapat dicetak
[ ] QR dapat diverifikasi
[ ] Pengembalian dapat diproses
[ ] Backup dapat dibuat
[ ] PDF dan Excel laporan dapat diunduh
```

## Catatan Pengembangan

Pada pengembangan SI-PESAN:

- Gunakan migration baru untuk perubahan struktur database.
- Hindari perintah destruktif pada database produksi.
- Jangan gunakan `migrate:fresh` pada database yang sudah berisi transaksi.
- Pastikan backup tersedia sebelum update besar.
- Pastikan status transaksi dan timestamp server tetap konsisten.
- Gunakan snapshot data transaksi untuk laporan historis.

## Lisensi

SI-PESAN merupakan aplikasi internal Sistem Perizinan Santri.

Penggunaan, distribusi, dan pengembangan lebih lanjut menyesuaikan kebijakan pemilik aplikasi/lembaga.

## SI-PESAN

**Sistem Perizinan Santri**

Dikembangkan untuk membantu proses perizinan santri menjadi lebih:

- Terstruktur
- Terpantau
- Terdokumentasi
- Akuntabel
- Mudah dievaluasi
