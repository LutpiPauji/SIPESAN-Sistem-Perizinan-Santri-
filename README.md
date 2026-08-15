SI-PESAN
Sistem Perizinan Santri
SI-PESAN adalah aplikasi berbasis Laravel 13 untuk mengelola proses perizinan santri secara terstruktur, mulai dari pengajuan, persetujuan, pencetakan surat, penggunaan izin, pengembalian surat, keterlambatan, sanksi, monitoring, hingga laporan.
Aplikasi dirancang untuk penggunaan internal pondok/pesantren dengan alur persetujuan yang dapat disesuaikan melalui master data dan pengaturan sistem.
---
1. Fitur Utama
Dashboard
Dashboard menampilkan ringkasan operasional yang terhubung langsung dengan transaksi, antara lain:
Total santri aktif
Pengajuan izin hari ini
Santri sedang izin
Lewat batas waktu
Menunggu persetujuan
Perlu perbaikan
Kembali hari ini
Siap dicetak
Ringkasan master data
---
Master Data
Master data yang tersedia meliputi:
Muroby
Asrama
Kamar
Ketua Kamar
Santri
Pengguna Sistem
Struktur Pengasuhan
Periode Penugasan
---
Perizinan
Modul perizinan meliputi:
Pengajuan Baru
Daftar Pengajuan
Perlu Perbaikan
Siap Dicetak
Pengembalian Surat
Lewat Batas
Pembatalan / Revisi
Cetak Ulang
Alur Umum Perizinan
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
Untuk jenis izin Keluar Kompleks, Ketua Asrama dapat menjadi approver final sesuai konfigurasi alur persetujuan.
---
Monitoring
Monitoring bersifat read-only dan digunakan untuk melihat kondisi transaksi tanpa mengubah status.
Menu monitoring:
Santri Sedang Izin
Kembali Hari Ini
Lewat Batas Waktu
Surat Belum Dikembalikan
Izin Sakit Aktif
Evaluasi Sakit
Izin Darurat
Sanksi Aktif
---
Pengaturan
Menu pengaturan meliputi:
Pengaturan Umum
Jenis Izin
Alasan Izin
Alasan Darurat
Alasan Keterlambatan
Kuota / Periode Izin
Aturan Sanksi
Alur Persetujuan
Format Nomor Surat
Backup & Restore
Format Nomor Surat
Nomor surat dapat dikonfigurasi melalui menu pengaturan.
Format yang digunakan:
```text
PREFIX / BULAN / TAHUN / NOMOR
```
Contoh:
```text
S-PZN/PND-ALFLH/08/2026/00001
```
---
Backup, Restore, dan Reset
Backup
Backup mencakup:
Data aplikasi
Transaksi
Pengguna
Konfigurasi
Activity log
File pada `storage/app/public`
Backup disimpan pada:
```text
storage/app/sipesan-backups
```
Sistem hanya menyimpan 3 backup terbaru.
Restore
Restore dapat dilakukan dengan mengunggah file backup SI-PESAN.
Sebelum restore dijalankan, sistem otomatis membuat backup keadaan saat ini.
Reset Data
Reset data:
Menghapus transaksi
Menghapus activity log
Menghapus master operasional
Menghapus file upload
Menghapus seluruh pengguna selain Admin Default
Menghapus session/cache/queue/token
Yang tetap dipertahankan:
Admin Default
Role dan permission
Pengaturan Umum
Jenis Izin
Alasan
Kuota / Periode
Aturan Sanksi
Alur Persetujuan
Format Nomor Surat
> Fitur Reset hanya tersedia untuk **Admin Default bawaan sistem**.
---
Laporan
Laporan dapat diekspor ke Excel dan PDF.
Menu laporan:
Rekap Perizinan
Keterlambatan
Kepatuhan
Sanksi
Izin Sakit
Izin Darurat
Audit Aktivitas
Nama lembaga pada file PDF dan Excel otomatis diambil dari:
```text
Pengaturan → Pengaturan Umum → Nama Lembaga
```
---
2. Role Pengguna
Role yang digunakan dalam sistem:
Role	Fungsi
Admin Pondok	Mengelola sistem, master data, pengaturan, laporan, dan administrasi
Keamanan	Mengelola operasional pengajuan dan pengembalian izin
Ketua Asrama	Melakukan tahap persetujuan sesuai alur
Muroby	Melakukan persetujuan final jika diperlukan
Pimpinan	Monitoring dan laporan
Pembuatan akun manual dari menu Pengguna Sistem hanya tersedia untuk:
```text
Admin Pondok
Keamanan
Pimpinan
```
Role Ketua Asrama dan Muroby mengikuti struktur/penugasan pengasuhan.
---
3. Teknologi
SI-PESAN menggunakan:
Laravel 13
PHP 8.5+
MySQL
Bootstrap 5
Bootstrap Icons
Vite
Spatie Laravel Permission
Spatie Activity Log
DomPDF
Endroid QR Code
PhpSpreadsheet
Environment pengembangan yang digunakan:
```text
PHP       : 8.5.x
Laravel   : 13.x
Composer  : 2.x
Node.js   : 26.x
Database  : MySQL
```
---
4. Persyaratan Instalasi Lokal
Pastikan komputer sudah memiliki:
PHP 8.5 atau versi yang kompatibel dengan project
Composer
MySQL / MariaDB
Node.js
NPM
Laragon direkomendasikan untuk Windows
Contoh lokasi project menggunakan Laragon:
```text
C:\laragon\www\sipesan
```
---
5. Instalasi Lokal Menggunakan Laragon
5.1. Letakkan Project
Salin folder project ke:
```text
C:\laragon\www\sipesan
```
Kemudian buka terminal:
```powershell
cd C:\laragon\www\sipesan
```
---
5.2. Install Dependency PHP
Jalankan:
```powershell
composer install
```
Jangan menggunakan:
```powershell
composer update
```
kecuali memang ingin memperbarui versi dependency.
---
5.3. Install Dependency Frontend
Jalankan:
```powershell
npm install
```
---
5.4. Buat File Environment
Jika `.env` belum ada:
```powershell
copy .env.example .env
```
Kemudian buka:
```powershell
code .env
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
Sesuaikan username dan password MySQL dengan komputer masing-masing.
---
5.5. Buat Database
Buka HeidiSQL, phpMyAdmin, atau terminal MySQL.
Buat database:
```sql
CREATE DATABASE sipesan
CHARACTER SET utf8mb4
COLLATE utf8mb4_unicode_ci;
```
---
5.6. Generate Application Key
Jalankan:
```powershell
php artisan key:generate
```
---
5.7. Jalankan Migration
Jalankan:
```powershell
php artisan migrate
```
> Jangan menggunakan `migrate:fresh`, `migrate:reset`, atau `db:wipe` pada database yang berisi data penting.
---
5.8. Jalankan Seeder
Jika project menyediakan seeder untuk role, permission, konfigurasi awal, dan Admin Default:
```powershell
php artisan db:seed
```
Jika project mempunyai seeder tertentu, jalankan sesuai nama seedernya.
Contoh:
```powershell
php artisan db:seed --class=DatabaseSeeder
```
Pastikan setelah seeding terdapat minimal:
```text
1 Admin Default
Role admin_pondok
Role keamanan
Role ketua_asrama
Role muroby
Role pimpinan
```
---
5.9. Storage Link
Jalankan:
```powershell
php artisan storage:link
```
Ini diperlukan agar file upload dapat diakses dari folder `public/storage`.
---
5.10. Build Frontend
Untuk development:
```powershell
npm run dev
```
Atau build production:
```powershell
npm run build
```
---
5.11. Bersihkan Cache
Setelah instalasi:
```powershell
php artisan optimize:clear
php artisan view:clear
php artisan route:clear
php artisan config:clear
```
---
6. Menjalankan Aplikasi
Opsi 1 — Artisan Serve
Jalankan:
```powershell
php artisan serve
```
Kemudian buka:
```text
http://127.0.0.1:8000
```
---
Opsi 2 — Laragon Virtual Host
Jika menggunakan Auto Virtual Hosts Laragon, biasanya aplikasi dapat dibuka melalui:
```text
http://sipesan.test
```
Pastikan konfigurasi:
```env
APP_URL=http://sipesan.test
```
Jika menggunakan `php artisan serve`, gunakan:
```env
APP_URL=http://127.0.0.1:8000
```
---
7. Urutan Setup Setelah Instalasi
Setelah berhasil login sebagai Admin Default, lakukan konfigurasi dengan urutan berikut:
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
Setelah master dan pengaturan selesai, modul Perizinan dapat digunakan.
---
8. Alur Operasional
Pengajuan
Petugas Keamanan membuat pengajuan melalui:
```text
Perizinan → Pengajuan Baru
```
Sistem akan melakukan pengecekan otomatis terhadap:
Kuota / periode izin
Jeda minimal izin
Sanksi tambahan
Pembatasan reguler
Alur persetujuan
---
Persetujuan
Tahap persetujuan menyesuaikan master:
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
---
Cetak Surat
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
---
Pengembalian
Pengembalian menggunakan waktu server.
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
---
9. QR Verifikasi
QR surat hanya digunakan untuk:
```text
Verifikasi keaslian surat
```
Scanning QR tidak mengubah:
Status izin
Waktu digunakan
Waktu kembali
Status transaksi lainnya
---
10. Nomor Surat
Nomor surat:
Unik
Tidak berubah setelah diterbitkan
Tetap sama ketika cetak ulang
QR versi lama dinonaktifkan ketika reprint
Contoh:
```text
S-PZN/PND-ALFLH/08/2026/00001
```
---
11. Backup dan Recovery
Sangat disarankan melakukan backup sebelum:
Perubahan besar pada master
Update aplikasi
Reset data
Restore
Migrasi server
Backup dapat dilakukan dari:
```text
Pengaturan → Backup & Restore
```
Folder backup lokal:
```text
storage/app/sipesan-backups
```
Hanya 3 backup terbaru yang disimpan otomatis.
---
12. Perintah Maintenance
Bersihkan cache
```powershell
php artisan optimize:clear
```
Bersihkan Blade cache
```powershell
php artisan view:clear
```
Lihat route
```powershell
php artisan route:list
```
Cek route laporan
```powershell
php artisan route:list --path=laporan
```
Cek route monitoring
```powershell
php artisan route:list --path=monitoring
```
Cek route backup
```powershell
php artisan route:list --path=pengaturan/backup-restore
```
---
13. Troubleshooting
View Not Found
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
---
Route Not Found
Jalankan:
```powershell
php artisan route:clear
php artisan route:list
```
Pastikan route sudah terdaftar sebelum Blade memanggil `route(...)`.
---
Perubahan Blade Tidak Muncul
Jalankan:
```powershell
php artisan view:clear
php artisan optimize:clear
```
Kemudian hard refresh browser:
```text
Ctrl + Shift + R
```
---
Favicon Lama Masih Muncul
Favicon SI-PESAN berada pada:
```text
public/favicon.ico
```
Jika browser masih menampilkan favicon lama:
```text
Ctrl + Shift + R
```
atau tutup tab lalu buka kembali.
---
File Upload Tidak Bisa Dibuka
Pastikan storage link sudah dibuat:
```powershell
php artisan storage:link
```
Kemudian cek:
```text
public/storage
```
---
Pagination Berukuran Besar
Project menggunakan Bootstrap.
Pastikan `AppServiceProvider` menggunakan:
```php
use Illuminate\Pagination\Paginator;

public function boot(): void
{
    Paginator::useBootstrapFive();
}
```
Kemudian:
```powershell
php artisan optimize:clear
```
---
14. Keamanan
Beberapa prinsip yang digunakan:
Role dan permission menggunakan Spatie Permission
Transaksi tidak menggunakan hard delete
Nomor surat bersifat immutable
QR lama dinonaktifkan saat reprint
Reset hanya tersedia untuk Admin Default
Restore meminta password admin
Backup diperiksa menggunakan checksum
Password tidak ditampilkan di activity log
Monitoring bersifat read-only
Untuk server produksi:
```env
APP_ENV=production
APP_DEBUG=false
```
Jangan menyimpan `.env` ke repository publik.
---
15. Struktur Folder Penting
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
---
16. Catatan Pengembangan
Pada pengembangan SI-PESAN:
Gunakan migration baru untuk perubahan struktur database.
Hindari mengubah data produksi dengan perintah destruktif.
Jangan gunakan `migrate:fresh` pada database yang sudah berisi transaksi.
Pastikan backup tersedia sebelum update besar.
Pastikan status transaksi dan timestamp server tetap konsisten.
Gunakan snapshot data transaksi untuk laporan historis.
---
17. Checklist Instalasi
Gunakan checklist berikut setelah instalasi:
```text
[ ] composer install
[ ] npm install
[ ] .env dibuat
[ ] APP_KEY dibuat
[ ] database dibuat
[ ] database dikonfigurasi di .env
[ ] php artisan migrate
[ ] php artisan db:seed (jika tersedia)
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
---
18. Lisensi
SI-PESAN merupakan aplikasi internal Sistem Perizinan Santri.
Penggunaan, distribusi, dan pengembangan lebih lanjut menyesuaikan kebijakan pemilik aplikasi/lembaga.
---
SI-PESAN — Sistem Perizinan Santri
Dikembangkan untuk membantu proses perizinan santri menjadi lebih:
Terstruktur
Terpantau
Terdokumentasi
Akuntabel
Mudah dievaluasi#   S I P E S A N - S i s t e m - P e r i z i n a n - S a n t r i -  
 