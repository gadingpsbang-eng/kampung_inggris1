# Kampung Inggris Karawaci II — Website

Website profil & pendaftaran **Kampung Inggris Karawaci II** (kursus bahasa Inggris), dibangun dengan Laravel (Blade + CSS3 murni, tanpa framework frontend/React).

## Fitur

- **4 halaman statis**: Beranda, Tentang Kami, Paket Kursus, Kontak
- **3 paket kursus** (data dari `database/seeders/CoursePackageSeeder.php`):
  - **Preschool** — 45 menit/sesi, maks. 5 peserta
  - **Anak-Anak (SD/SMP)** — 60 menit/sesi, maks. 8 peserta
  - **Dewasa (SMA/Umum/Mahasiswa)** — 90 menit/sesi, maks. 14 peserta
- Semua kelas **offline (tatap muka)**, pilihan jadwal **Weekday** & **Weekend**, 8× sesi per bulan
- **Harga tidak ditampilkan** di website — semua permintaan harga diarahkan ke **WhatsApp** (tombol `Hubungi Admin` / `Daftar Sekarang`)
- Tombol WhatsApp melayang di semua halaman
- Desain responsif mobile-first, CSS murni (`resources/css/app.css`), JavaScript vanilla (`resources/js/app.js`)
- SEO: `meta description`, Open Graph, Twitter Card, favicon

## Teknologi

- PHP 8.2+ & Laravel 13
- SQLite (default, tanpa konfigurasi database luar)
- Blade template, controller + seeder (tanpa data fiktif)

## Instalasi

> Catatan: pada jaringan yang lambat/terbatas (mis. saat `composer install` berhenti di paket dev `phpunit`/`faker`/`pint`), gunakan `--no-dev`. Paket dev hanya untuk testing/linting dan **tidak diperlukan** untuk menjalankan website.

```bash
# 1. Pindah ke folder proyek
cd kampung-inggris-karawaci-ii

# 2. Install dependensi PHP
composer install --no-dev

# 3. Buat .env dari contoh & generate app key
cp .env.example .env
php artisan key:generate

# 4. Sesuaikan file .env bila perlu (default sudah siap pakai)
#    DB_CONNECTION=sqlite  (database/database.sqlite sudah dibuat)

# 5. Jalankan migrasi + seed paket kursus
php artisan migrate --seed

# 6. Jalankan server
php artisan serve
```

Buka `http://127.0.0.1:8000`.

### Asset (CSS/JS) — tanpa Vite

File `resources/css/app.css` dan `resources/js/app.js` sudah berkas CSS/JS **murni** (tidak butuh build/kompilasi). Salinan keduanya sudah tersedia di `public/css/app.css` dan `public/js/app.js` dan langsung dipakai oleh layout (`layouts/app.blade.php`) melalui `asset('css/app.css')` / `asset('js/app.js')`.

Dengan begitu website **tidak memerlukan `npm install` / `npm run build`** untuk berjalan.

> Jika ingin menggunakan pipeline Vite (mis. untuk tailwind/mixing di masa depan), proyek ini tetap memiliki `package.json`, `vite.config.js`, dan direktori `resources/`. Ubah kembali layout ke `@vite([...])` lalu jalankan `npm install && npm run build`. Namun perlu diingat: Vite 7 (rolldown) saat ini memerlukan native binding; pastikan platform build Anda mendukungnya.

## Kontak & Pengaturan

Semua data identitas & kontak terpusat di **`config/site.php`** dan dibaca lewat helper di `app/helpers.php`:

| Data | Nilai default (`config/site.php`) |
|---|---|
| Nama situs | Kampung Inggris Karawaci II |
| WhatsApp | `6281958960010` (tampilan: `0819-5896-0010`) |
| Instagram | `@kampunginggristangerang` |
| TikTok | `@kampunginggristangerangg` |
| Helpers | `site_logo_url()`, `wa_url($pesan)` → `https://wa.me/...?text=...` |

### Logo & favicon

Ganti logo situs dengan menaruh file Anda di:

- **`public/images/logo.png`** — logo menu & footer
- **`public/images/favicon.png`** — ikon tab browser

File placeholder sudah disertakan. Bila file belum ada, gambar logo otomatis disembunyikan (`onerror`), sehingga halaman tetap rapi.

## Struktur Ringkas

```
routes/web.php                        → rute halaman
app/Http/Controllers/                 → HomeController, PaketController, TentangController, ContactController
app/Models/CoursePackage.php          → model paket kursus
app/helpers.php                       → site_logo_url(), wa_url(), dll.
config/site.php                       → identitas & kontak terpusat
database/migrations/...course_packages…→ tabel paket kursus
database/seeders/CoursePackageSeeder.php → 3 paket kursus
resources/views/                      → layout + komponen + halaman (Blade)
resources/css/app.css                 → stylesheet satu file
public/images/logo.png|favicon.png    → aset gambar
```

## Lisensi

Website ini merupakan proyek untuk Kampung Inggris Karawaci II. Framework Laravel dilisensikan di bawah [MIT license](https://opensource.org/licenses/MIT).
