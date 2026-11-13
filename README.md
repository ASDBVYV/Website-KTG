# Kamil Ticket Global — Website

Website statis (HTML/CSS/JS), tanpa build step. Rute tunggal **Jakarta (CGK) → Jeddah (JED)**.
Semua penerbangan direct. Urutan daftar: **Saudia → Garuda Indonesia → Lion Air**.

## Halaman
| File | Isi |
|---|---|
| `index.html` | Beranda + pencarian |
| `flights.html` | Hasil penerbangan, filter, urutkan, strip tanggal |
| `services.html` | Layanan & cara pemesanan |
| `about.html` | Tentang kami |
| `support.html` | Kontak, FAQ, formulir pesan |
| `login.html` | Masuk / Daftar / Lupa kata sandi |
| `admin.html` | **Panel admin**: lihat pemesanan, cek bukti bayar & nominal, verifikasi/tolak, ekspor CSV (hanya akun admin) |
| `payment.html` | Pembayaran → unggah manifest penumpang → e-tiket (wajib login) |

Bahasa **English / Indonesia** bisa diganti dari menu globe di header (tersimpan di browser).
Semua teks ada di `assets/i18n.js`.

## Deploy (GitHub + Vercel)
1. Upload isi folder ini ke repo GitHub (file `index.html` harus di root).
2. Di Vercel: **Add New → Project → pilih repo**. Framework Preset: **Other**, tanpa build command. Deploy.

## Yang WAJIB diganti sebelum live
- `assets/core.js` bagian `CONFIG`: email, WhatsApp, telepon (sekarang placeholder).
- **Data penerbangan** (`TEMPLATES` di `assets/core.js`) adalah data contoh: jadwal, harga, bagasi. Sambungkan ke inventori/API maskapai atau isi harga asli.
- **Login** saat ini demo: akun disimpan di browser (localStorage). Untuk produksi pakai backend (mis. Supabase / Firebase Auth). Tombol Google belum terhubung.
- **Pembayaran** hanya menerima transfer bank **Mandiri** (0060011525478) dan **BSI** (7198461387). Nomor rekening dan nama pemilik ada di `CONFIG.banks` (`assets/core.js`); biaya di `CONFIG.fees.va`. Konfirmasi pembayaran masih simulasi (tombol demo), belum terhubung ke mutasi bank/payment gateway.
- Logo Saudia dan Lion Air memakai badge teks. Ganti dengan logo resmi bila sudah ada izin pakai.

## Admin
- Email admin: **kamilteknoglobalindo@gmail.com** (`CONFIG.adminEmail` di `assets/core.js`; email ini juga dipakai sebagai email kontak).
- Cara masuk: daftar/masuk di `login.html` memakai email tersebut → menu nama di header → **Panel Admin** (atau buka `admin.html`).
- Alur status: pelanggan upload bukti bayar + nominal → pemesanan berstatus **Menunggu** → admin **Verifikasi** (e-tiket jadi LUNAS & bisa dicetak) atau **Tolak** (dengan alasan).
- **Penting (demo):** semua data tersimpan di localStorage browser, jadi admin hanya melihat pemesanan yang dibuat di browser yang sama, dan siapa pun yang mendaftar dengan email admin bisa membuka panel. Untuk produksi wajib pakai backend (Supabase/Firebase: Auth + database + Storage untuk file bukti bayar) dan cek peran admin di server.

## Cicilan (bayar setengah dulu)
- Di halaman pembayaran pelanggan memilih **Bayar lunas** atau **Cicil — bayar setengah dulu** (minimal 50% dari total; nominal bisa diubah antara 50% s/d 100%).
- Sisa dibayar dari halaman e-tiket lewat tombol **Bayar sisa** (unggah bukti + nominal lagi, bisa berkali-kali sampai lunas).
- Tiap transfer tersimpan sebagai satu pembayaran (`booking.payments`) dan diverifikasi/ditolak sendiri-sendiri oleh admin. Status pemesanan: **Menunggu** (ada transfer belum dicek), **Cicilan** (sebagian terverifikasi, masih ada sisa), **Lunas**, **Ditolak**.
- E-tiket baru bisa dicetak setelah total pembayaran terverifikasi lunas.
