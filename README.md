# 📋 DaftarHadirGuru

Aplikasi web untuk manajemen daftar hadir guru madrasah berbasis Supabase. Dibangun sebagai **single-file HTML** — tidak perlu server, tidak perlu instalasi, cukup buka di browser.

> Dikembangkan untuk **Pondok Pesantren Al Imam Asy-Syafi'i Tarakan**

---

## ✨ Fitur Utama

### 👨‍🏫 Untuk Guru
| Fitur | Keterangan |
|-------|------------|
| Isi Absensi Harian | Pilih status per sesi (Hadir, Sakit, Izin Dinas, Izin Pribadi, Alfa) |
| Kalender Sidebar | Navigasi tanggal dengan indikator kelengkapan absensi |
| Laporan Bulanan | Rekap JP (Jam Pelajaran) per pekan |
| Ajukan Izin Panjang | Pengajuan ketidakhadiran > 3 hari dengan persetujuan admin |
| Isi sebagai Pengganti | Singkat (per sesi) dan panjang (selama rentang izin) |
| Request Unlock | Minta akses edit untuk bulan yang sudah terkunci |

### 🔐 Untuk Admin
| Fitur | Keterangan |
|-------|------------|
| Dashboard | Ringkasan harian: total guru, hadir, pending request |
| Monitor Absensi | Pantau kondisi absensi semua guru secara real-time + edit langsung |
| Log Aktivitas | Timeline pengisian absensi hari ini |
| Data Pegawai | Tambah, edit, hapus guru + upload massal via Excel |
| Persetujuan | Approve/reject izin panjang, unlock, dan request pengganti |
| Jadwal | Atur jadwal baku per guru + jadwal libur lembaga |
| Laporan PDF/CSV | Cetak laporan JP bulanan dengan kop surat & tanda tangan |
| Pengaturan | Nama lembaga, logo, data kepala madrasah & operator |

---

## 🗓 Sesi yang Didukung

| ID | Label | Kelompok |
|----|-------|---------|
| H1 | Subuh | Ibadah |
| H2 | Dhuha | Ibadah |
| H3 | Dzuhur | Ibadah |
| J1 | Jam Pelajaran 1 | KBM |
| J2 | Jam Pelajaran 2 | KBM |
| J3 | Jam Pelajaran 3 | KBM |
| J4 | Jam Pelajaran 4 | KBM |
| S1 | Sore | Ekstra |
| S2 | Malam | Ekstra |

**Hari aktif:** Sabtu, Ahad, Senin, Selasa, Rabu, Kamis *(Jumat = libur otomatis)*

---

## 🚀 Cara Deploy

### Opsi 1 — Buka Langsung (Lokal)
1. Download file `index.html`
2. Double-click → langsung terbuka di browser
3. Butuh koneksi internet untuk sinkronisasi data ke Supabase

### Opsi 2 — Deploy ke Hosting (Akses dari mana saja)

**Netlify Drop** *(Termudah)*
1. Buka https://app.netlify.com/drop
2. Drag & drop file `index.html`
3. Selesai — dapat URL instan

**Vercel**
1. Buka https://vercel.com → daftar gratis
2. New Project → Upload folder berisi `index.html`
3. Deploy

**GitHub Pages**
1. Buat repository baru di https://github.com
2. Upload `index.html` → rename jadi `index.html`
3. Settings → Pages → Branch: main → Save
4. URL: `username.github.io/nama-repo`

**Cloudflare Pages**
1. Buka https://dash.cloudflare.com → Workers & Pages
2. Create application → Pages → Upload assets
3. Upload folder berisi `index.html`
4. Deploy

---

## 🗄 Setup Database (Supabase)

### Prasyarat
- Akun Supabase gratis di https://supabase.com
- Buat project baru

### Langkah Setup
1. Buka aplikasi → **Pengaturan → SQL Setup**
2. Copy seluruh SQL yang tersedia
3. Buka **Supabase → SQL Editor**
4. Paste dan jalankan

### Tabel yang Diperlukan

| Tabel | Fungsi |
|-------|--------|
| `users` | Data guru dan admin |
| `absensi` | Record kehadiran per sesi per tanggal |
| `jadwal_baku` | Jadwal mingguan per guru |
| `jadwal_khusus` | Jadwal libur lembaga |
| `unlock_requests` | Request buka kunci bulan lama |
| `request_pengganti` | Request penggantian sesi singkat |
| `izin_panjang` | Pengajuan izin > 3 hari |
| `request_pengganti_panjang` | Request penggantian untuk izin panjang |
| `notifikasi` | Notifikasi sistem |
| `settings` | Konfigurasi lembaga (nama, logo, dll) |

### Migrasi dari Database Lama (Firebase)
Gunakan tool `MigrasiFirebase.html` yang tersedia terpisah.

---

## 🔧 Konfigurasi

Konfigurasi Supabase tertanam langsung di dalam `index.html`:

```javascript
const DEFAULT_URL = 'https://[project-id].supabase.co';
const DEFAULT_KEY = 'eyJhbGci...'; // anon public key
```

Untuk mengganti ke project Supabase lain, edit kedua baris tersebut atau masukkan melalui kolom konfigurasi di halaman login.

---

## 👤 Akun Default

Setelah menjalankan SQL setup:

| Role | Username | Password |
|------|----------|---------|
| Admin | `admin` | `admin123` |

> ⚠️ **Segera ganti password admin** setelah pertama login melalui **Pengaturan → Akun Administrator**

Password guru default saat import massal: `guru123`

---

## 📊 Sistem JP (Jam Pelajaran)

| Status | JP |
|--------|-----|
| Hadir | +2 JP |
| Izin Dinas | +2 JP |
| Pengganti | +2 JP |
| Sakit | 0 JP |
| Izin Pribadi | 0 JP |
| Alfa | 0 JP |
| Libur Lembaga | +2 JP per sesi (otomatis) |

---

## 🔒 Sistem Keamanan

- **Kunci Bulan** — Absensi bulan lalu otomatis terkunci, hanya bisa diedit setelah admin approve unlock request (berlaku 15 menit)
- **Password** — Disimpan dengan hash bcrypt (salt rounds: 10)
- **Role** — `admin`, `guru`, `halaqah` (bisa kombinasi, misal `guru,halaqah`)
- **Status Pegawai** — `aktif` atau `cuti` (cuti tidak dihitung JP di hari libur)

---

## 📱 Kompatibilitas

- ✅ Desktop (Chrome, Firefox, Edge, Safari)
- ✅ Mobile (Android & iOS) — layout responsif
- ✅ PWA-ready (bisa di-install di HP)
- ❌ Tidak mendukung Internet Explorer

---

## 🛠 Teknologi

| Teknologi | Fungsi |
|-----------|--------|
| HTML/CSS/JS | Frontend (single file) |
| [Supabase](https://supabase.com) | Database PostgreSQL + API |
| [bcryptjs](https://github.com/dcodeIO/bcrypt.js) | Hash password |
| [SheetJS](https://sheetjs.com) | Import/export Excel |
| [Cloudflare Pages](https://pages.cloudflare.com) | Hosting (opsional) |

---

## 📁 Struktur File

```
/
├── index.html          # Aplikasi utama (semua-dalam-satu)
├── MigrasiFirebase.html # Tool migrasi dari Firebase ke Supabase
└── README.md           # Dokumentasi ini
```

---

## 🐛 Troubleshooting

**Login gagal**
- Pastikan koneksi internet aktif
- Cek URL dan Key Supabase di konfigurasi
- Jalankan SQL setup jika tabel belum dibuat

**Data tidak muncul di laporan**
- Pastikan **jadwal baku** sudah diatur di menu Jadwal
- JP hanya dihitung untuk sesi yang ada di jadwal baku

**Absensi bulan lalu tidak bisa diedit**
- Normal — bulan lalu otomatis terkunci
- Guru bisa request unlock → admin approve → edit tersedia 15 menit

**Izin panjang tidak muncul di Persetujuan**
- Pastikan tabel `izin_panjang` sudah dibuat (jalankan SQL migrasi jika belum)

---

## 📞 Informasi

Dikembangkan untuk kebutuhan internal madrasah.
Database: Supabase Project `daftarhadirpro`
# daftarhadir
