# SPMB / PPDB SMK Bakti Nusantara 666

Sistem informasi pendaftaran peserta didik baru berbasis Laravel untuk proses SPMB/PPDB SMK Bakti Nusantara 666. Aplikasi ini mencakup portal publik, registrasi peserta dengan OTP email, formulir pendaftaran multi-step, monitoring verifikasi, validasi pembayaran, seleksi kepala sekolah, peta sebaran pendaftar, dan export dokumen PDF.

## Fitur Utama

- Landing page dan halaman informasi publik: beranda, akademik, informasi, FAQ, kontak, dan pengumuman.
- Registrasi akun peserta dengan verifikasi OTP via email.
- Login peserta, login admin, dan dashboard berbasis role.
- Form pendaftaran multi-step untuk data siswa, orang tua, sekolah asal, pilihan jurusan, dan upload berkas.
- Validasi NISN dan email, plus pengecekan pendaftaran yang masih pending.
- API wilayah untuk provinsi, kabupaten, kecamatan, kelurahan, dan koordinat kecamatan.
- Pengumuman hasil, upload bukti pembayaran, re-upload berkas, dan export kartu pendaftaran PDF.
- Panel admin untuk master data jurusan, gelombang, biaya, wilayah, akun, monitoring, laporan, dan log aktivitas.
- Panel verifikator untuk verifikasi administrasi dan tindak lanjut pendaftar.
- Panel keuangan untuk validasi bukti pembayaran dan laporan pembayaran.
- Panel kepala sekolah untuk seleksi akhir, hasil seleksi, statistik, dan export PDF/print.
- Integrasi WhatsApp opsional melalui Fonnte untuk notifikasi status.

## Role Sistem

- `pendaftar`: peserta yang membuat akun dan mengisi formulir pendaftaran.
- `admin`: mengelola master data, laporan, monitoring, akun, dan log aktivitas.
- `verifikator_adm`: memeriksa kelengkapan administrasi pendaftar.
- `keuangan`: memvalidasi pembayaran dan laporan keuangan pendaftar.
- `kepsek`: melakukan seleksi akhir dan melihat dashboard ringkasan hasil.

## Tech Stack

- PHP 8.2
- Laravel 12
- Blade
- Vite
- Tailwind CSS
- MySQL atau SQLite
- `barryvdh/laravel-dompdf` untuk export PDF
- Fonnte untuk WhatsApp opsional

## Struktur Fitur Utama

- `routes/web.php`
  Semua alur publik, peserta, admin, verifikator, keuangan, dan kepsek.
- `app/Http/Controllers/AuthController.php`
  Registrasi peserta, login, OTP email, login admin.
- `app/Http/Controllers/PendaftaranController.php`
  Alur pendaftaran dan upload berkas.
- `app/Http/Controllers/PengumumanController.php`
  Cek status hasil, upload bukti pembayaran, re-upload berkas, export kartu PDF.
- `app/Http/Controllers/Admin/*`
  Master data, monitoring, laporan, peta sebaran, user management, log aktivitas.
- `app/Http/Controllers/Verifikator/*`
  Dashboard, data pendaftar, verifikasi administrasi, laporan.
- `app/Http/Controllers/Keuangan/*`
  Dashboard, validasi pembayaran, laporan.
- `app/Http/Controllers/Kepsek/KepsekController.php`
  Dashboard kepsek, hasil seleksi, seleksi akhir, export PDF/print.

## Setup Lokal

### Prasyarat

- PHP 8.2+
- Composer
- Node.js 18+ dan npm
- Database MySQL/MariaDB atau SQLite

### Instalasi

```bash
composer install
npm install
copy .env.example .env
php artisan key:generate
php artisan migrate --seed
php artisan storage:link
npm run build
php artisan serve
```

Untuk development dengan Vite:

```bash
npm run dev
```

Jika ingin menjalankan stack dev bawaan Composer:

```bash
composer run dev
```

## Konfigurasi Environment

Minimal sesuaikan nilai berikut di `.env`:

- `APP_NAME`
- `APP_URL`
- `DB_CONNECTION`, `DB_HOST`, `DB_PORT`, `DB_DATABASE`, `DB_USERNAME`, `DB_PASSWORD`
- `MAIL_MAILER`, `MAIL_HOST`, `MAIL_PORT`, `MAIL_USERNAME`, `MAIL_PASSWORD`, `MAIL_FROM_ADDRESS`, `MAIL_FROM_NAME`
- `FONNTE_TOKEN` jika ingin mengaktifkan pengiriman WhatsApp

Catatan:

- Registrasi peserta memakai OTP email, jadi konfigurasi mail perlu valid agar alur verifikasi berjalan normal.
- Project ini memakai upload berkas dan export file, jadi `php artisan storage:link` penting.

## Seeder

Seeder default menyiapkan data dasar berikut:

- wilayah
- wilayah Indonesia
- jurusan
- gelombang

Jalankan dummy data opsional untuk testing:

```bash
php artisan db:seed --class=DummyDataSeeder
```

## Menyiapkan Akun Admin Pertama

Repo ini tidak menyiapkan akun backend default secara otomatis. Cara cepat membuat akun admin pertama:

```bash
php artisan tinker
```

```php
use App\Models\Pengguna;
use Illuminate\Support\Facades\Hash;

Pengguna::updateOrCreate(
    ['email' => 'admin@example.com'],
    [
        'nama' => 'Administrator',
        'password_hash' => Hash::make('password123'),
        'role' => 'admin',
        'aktif' => 1,
        'is_verified' => 1,
    ]
);
```

Setelah itu login melalui `/admin/login`.

## Alur Singkat Pengguna

1. Peserta membuat akun dari landing page.
2. Sistem mengirim OTP ke email peserta.
3. Peserta verifikasi OTP lalu login.
4. Peserta mengisi formulir pendaftaran multi-step.
5. Peserta submit pendaftaran dan menerima nomor pendaftaran.
6. Peserta memantau hasil di halaman pengumuman.
7. Jika diperlukan, peserta upload bukti pembayaran atau re-upload berkas.

## Alur Singkat Backend

1. `admin` menyiapkan jurusan, gelombang, biaya, wilayah, dan akun petugas.
2. `verifikator_adm` memeriksa data dan status administrasi pendaftar.
3. `keuangan` memvalidasi bukti pembayaran.
4. `kepsek` melakukan seleksi akhir dan menerbitkan hasil.

## File Tambahan

- `USER GUIDE SPMB.pdf`
  Panduan penggunaan sistem yang disimpan di root project.

## Testing

Jalankan test suite Laravel dengan:

```bash
php artisan test
```

## Catatan

- Repository GitHub saat ini menggunakan branch utama `master`.
- File video besar di repo masih berada di Git biasa, jadi bila aset media terus bertambah sebaiknya pertimbangkan Git LFS.
