# DESIGN.md: Portal SPBE Kota Bogor

Arah desain untuk antarmuka frontend. Sumber isi: folder `Praproses` (PRD, Buku Panduan Standarisasi Aplikasi, dan dokumen revisi). Folder `Design` tidak dipakai di sini. Bila dokumen ini berbeda dengan dokumen revisi, dokumen revisi yang berlaku.

Setiap butir diberi sumber. Butir bertanda **[belum diputuskan]** tidak ada di Praproses dan harus diputuskan pemilik desain, bukan diisi asal.

## 1. Identitas

- **Produk:** portal komunitas dan repositori pengetahuan SPBE (Sistem Pemerintahan Berbasis Elektronik) milik Dinas Komunikasi dan Informatika Kota Bogor. Isinya: Dokumen dan SOP, Forum, Pembelajaran, Berita, katalog aplikasi dan arsitektur, serta dasbor. (PRD)
- **Pemilik:** Pemerintah Kota Bogor. Bukan produk komersial; tidak ada penjualan, harga, atau konversi.
- **Tujuan:** satu sumber data arsitektur SPBE yang tervalidasi, wadah berbagi pengetahuan bagi ASN, dan sarana literasi SPBE bagi publik. (PRD)
- **Apa yang harus terasa:** resmi dan tepercaya sebagai situs pemerintah, tetapi tidak kaku. PRD menulis kelemahan web sebelumnya adalah "terlalu kaku" dan meminta "tampilan modern, ramah pengguna, visualisasi grafik yang menarik, menu yang mudah dipahami orang awam".

## 2. Pengguna

| Pengguna | Tujuan utama | Dampak ke desain |
|---|---|---|
| Umum (tanpa akun) | Membaca berita, mencari dokumen publik, melihat ringkasan statistik | Halaman depan harus langsung jelas tanpa login; menu hanya yang bisa diakses |
| ASN dan Akademisi | Mencari dokumen dan SOP, berdiskusi, belajar, mengikuti dasbor | Pencarian dan daftar yang cepat dipindai |
| Admin OPD | Mengelola aplikasi dan flowchart instansi, mengajukan validasi | Tabel, formulir, dan status yang tegas |
| Super Admin | Memutuskan validasi, moderasi, mengelola pengguna dan sistem | Padat informasi, antrean kerja jelas |

Akun hanya lewat undangan. Tidak ada pendaftaran mandiri. (Revisi K2)

## 3. Kepribadian dan nada

- **Kata kunci:** resmi, jelas, ramah, tertata. (turunan dari PRD, butir Usability)
- **Bahasa:** Bahasa Indonesia; istilah asing hanya yang sudah serapan. (Buku Panduan C.r)
- **Istilah yang sudah diputuskan:**
  - "Ajukan Semua RAA", bukan "Validasi Semua RAA" (K16, D14)
  - "Bukti penyelesaian", bukan "Sertifikasi Kompetensi"; tanpa poin SKP (K11, D17)
  - "Terverifikasi SSO", bukan "Terverifikasi SIASN BKN" (D3)
  - Status akun: Diundang, Aktif, Nonaktif (D15)
  - Footer: "© 2026 Pemerintah Kota Bogor", bukan "© 2026 SPBE" (D1)
  - "Login" dan "Thread" boleh dipertahankan sebagai istilah serapan; alternatifnya "Masuk" dan "Topik" (D12). **[belum diputuskan]**
- **Yang dihindari:** janji yang tidak ada di sistem. Contoh: "AI" untuk pencarian yang masih kata kunci, sertifikat kompetensi, integrasi BKN, push notification. (Revisi: Won't)

## 4. Batasan wajib dari Buku Panduan

| Ketentuan | Penerapan |
|---|---|
| Responsif di desktop, laptop, dan layar kecil (C.a, PRD) | Mulai dari 375px; tanpa geser horizontal; uji di 375, 768, dan desktop |
| Logo resmi Pemerintah Kota Bogor di setiap halaman (C.m) | Ada di navbar; halaman Masuk memakai tampilan polos dan wajib tetap memuat logo |
| Copyright atas nama Pemerintah Kota Bogor dengan tahun pembangunan di setiap halaman (C.m) | Footer; halaman Masuk wajib memuatnya juga |
| Menu Manajemen Pengetahuan: FAQ, SOP, Panduan, Video (C.n) | Halaman Bantuan dan SOP |
| Menu kontak kami dan Kritik & Saran (Dashboard, C) | Halaman Kritik & Saran |
| Laporan PDF; data pribadi di-mask (C.t) | Berlaku untuk setiap ekspor PDF |
| Pesan kesalahan tidak membuka detail sistem (Keamanan) | Pesan galat ramah dan umum |
| Captcha di login (Keycloak, bagian 1.2) | Di mode demo hanya simulasi dan harus diberi tanda demo |
| Auto logout 15 menit tanpa aktivitas | Beri peringatan sebelum sesi berakhir **[belum diputuskan]** |

## 5. Struktur dan navigasi

- Satu tata letak untuk semua halaman, navbar menyesuaikan role. (Rancangan Frontend, bagian 3)
- **Pengunjung:** Beranda, Berita, Dokumen, Kritik & Saran, Bantuan. SOP, Pembelajaran, Forum tidak ditampilkan karena tidak bisa diakses. (K13 mengizinkan gembok; keputusan sesi ini: sembunyikan)
- **ASN dan Akademisi:** Beranda, Dokumen, SOP, Pembelajaran, Forum.
- **Admin OPD:** menu ASN + dropdown **Kelola** (Data Aplikasi, Klasifikasi RAA, Flowchart, Dasbor Instansi).
- **Super Admin:** menu Admin OPD untuk semua instansi + Pengguna, Validasi, Repositori, Moderasi, Periode, Audit Trail, Backup.
- Filter, pencarian, dan halaman disimpan di URL agar bisa dibagikan dan tombol Kembali bekerja. (Rancangan Frontend, bagian 5)

## 6. Bahasa visual

### 6.1 Warna

- **Satu warna merek.** Biru indigo, dipakai di kode saat ini (`index.css`): 50 `#eef1fb`, 100 `#dce2f7`, 200 `#b9c4ef`, 500 `#3b4fc4`, 600 `#2f3fa8`, 700 `#263386`, 800 `#1e2869`, 900 `#161d4f`. Asalnya dari kode, bukan dari Praproses. **[belum diputuskan: cocokkan dengan warna lambang resmi Kota Bogor]**
- Netral: skala slate untuk teks, garis, dan latar.
- **Warna hanya untuk makna status.** Setiap status harus punya teks atau ikon, tidak boleh hanya warna. Status yang ada di Praproses:

| Kelompok | Nilai | Sumber |
|---|---|---|
| Validasi | Draft, Menunggu, Disetujui, Perlu Revisi, Ditolak (Ditolak final) | K6 |
| Kesesuaian RAA | Sesuai (80 ke atas), Perlu Penyesuaian (50 sampai 79), Belum (di bawah 50 atau kosong) | K14 |
| Dokumen | draft, review, berlaku, arsip | Class Diagram |
| Thread | aktif, terkunci, tersembunyi | Class Diagram |
| Akun | diundang, aktif, nonaktif | K2 |

- Pemetaan warna per status **[belum diputuskan]**. Usulan minimum: hijau untuk Disetujui dan Sesuai, kuning untuk Menunggu dan Perlu Penyesuaian, merah untuk Ditolak dan Belum, abu untuk Draft dan Nonaktif.
- Kontras teks minimal 4,5:1 (3:1 untuk teks besar). Hitung dulu bila teks berada di atas foto atau gradien.

### 6.2 Tipografi

- Font **[belum diputuskan]**. Saat ini tumpukan font sistem tanpa alasan tertulis.
- Syarat dari Praproses: terbaca di layar kecil dan untuk orang awam; angka di tabel dan statistik harus rata (tabular).
- Aturan yang sudah disepakati: badan teks minimal 16px di HP, tinggi baris 1,5, panjang baris 65 sampai 75 karakter di desktop.

### 6.3 Bentuk, jarak, dan kerapatan

- Dua kerapatan, sesuai pengguna: **longgar** untuk halaman publik dan baca (landing, berita, dokumen), **padat** untuk halaman kerja (tabel, validasi, moderasi, audit).
- Target sentuh minimal 44px di HP.
- Sudut membulat sedang dan konsisten; bayangan hanya untuk elemen yang melayang (dropdown, dialog).

### 6.4 Citra

- Lambang dan foto asli Balai Kota Bogor sebagai identitas visual. Tanpa ilustrasi generik, tanpa emoji, tanpa foto stok.
- Foto yang dipakai harus asli milik Pemkot atau berlisensi jelas **[perlu konfirmasi sumber dan izin]**.
- Ikon: satu pustaka (lucide-react), hanya untuk membantu makna, bukan hiasan. (Rancangan Frontend, bagian 1)

### 6.5 Grafik dan data

- Visualisasi yang diminta PRD: statistik di Beranda tiap role, status RAA, tren, status validasi, aktivitas 30 hari. (K1, 5.11)
- Grafik buatan sendiri dalam SVG: Donut, Bar, Line, selalu disertai tabel data untuk pembaca layar. (Rancangan Frontend, bagian 1)
- Setiap grafik menjawab satu pertanyaan, diberi judul, dan warnanya bermakna. Hanya satu warna merek kecuali untuk status.
- Angka contoh harus konsisten antar halaman (D9) dan diberi tanda bila data tiruan.

### 6.6 Gerak

- Gerak hanya untuk umpan balik (muat, berhasil, buka dialog). Hormati `prefers-reduced-motion`.
- Tidak ada animasi dekoratif berulang.

### 6.7 Referensi situs pemerintah dan Arah B

Diamati langsung (Oktober 2026): GOV.UK (gov.uk), Pemprov DKI Jakarta (jakarta.go.id), GovTech Singapura (tech.gov.sg).

| Referensi | Yang diambil | Yang tidak |
|---|---|---|
| GOV.UK | Pita warna solid penuh dengan judul besar rata kiri dan kolom cari sebagai aksi utama; daftar topik berupa tautan bergaris, bukan kartu; tipografi tebal dan rapat | Palet biru dan lambang Inggris |
| Jakarta | Hero foto penuh dengan lapisan gelap; panel "Populer / Terbaru" di samping hero | Jumlah menu yang padat |
| GovTech SG | Pita tipis "Situs resmi pemerintah" dengan petunjuk cara memastikan keasliannya | Hero bergaya acara |

**Arah B (berbeda dari Arah A dan dari mockup):**
- Warna utama hijau tua (Kota Hujan, hijau pada lambang), bukan biru indigo. Merah bendera tetap sebagai garis tipis.
- Hero foto Balai Kota penuh lebar dengan lapisan hijau tua, teks putih rata kiri, kolom cari lebar. Bukan hero pucat dengan foto di kanan.
- Konten halaman depan berupa daftar bergaris (topik, berita dengan tanggal di kiri, dokumen), bukan kartu berbingkai.
- Halaman internal memakai menu samping kiri, bukan menu atas.
- Judul memakai font serif untuk kesan resmi, isi memakai sans. Pilihan font **[belum diputuskan]**.
- Footer terang dengan garis hijau tebal di atas, bukan blok biru tua.

Status: berkas Figma Arah B baru berisi aset (lambang dan foto). Layarnya belum dibangun karena batas panggilan Figma MCP pada paket Starter sudah habis.

## 7. Pola antarmuka per jenis halaman

| Jenis | Halaman | Pola |
|---|---|---|
| Publik | Landing, Berita, Dokumen, Bantuan, Kritik & Saran, Cari | Pencarian menonjol di landing; kartu berita; baris dokumen dengan tombol unduh; hasil pencarian menandai bila memakai fallback (5.9) |
| Baca dan diskusi | Detail dokumen, SOP, Forum, Pembelajaran | Teks mudah dibaca; aksi (balas, nilai, laporkan) berdekatan dengan kontennya |
| Kerja | Data Aplikasi, RAA, Flowchart, Validasi, Pengguna, Repositori, Moderasi | Tabel dengan tab status, pencarian, dialog konfirmasi untuk aksi tak terbalik (tolak, nonaktifkan) |
| Dasbor | Beranda tiap role, Dasbor Instansi, Analitik | Widget statistik per role; embed Metabase hanya di dasbor lanjutan |
| Sistem | Periode, Audit Trail, Backup | Tabel dan formulir sederhana; hanya baca untuk Audit Trail |
| Masuk | `/masuk` | Halaman polos satu tugas; pesan jelas untuk akun belum terdaftar atau nonaktif |

Keadaan wajib di setiap halaman data: memuat, kosong, galat (dengan jalan keluar), dan tanpa hak akses (403). (Rancangan Frontend, bagian 6)

## 8. Aksesibilitas

- Tautan lompat ke konten, landmark, satu H1 per halaman, `lang="id"`.
- Semua kolom isian berlabel; kontrol tanpa teks punya nama aksesibel.
- Navigasi keyboard penuh; indikator fokus terlihat (minimal 3:1) di semua kontrol, termasuk kolom isian.
- Kursor pointer pada setiap kontrol yang bisa diklik.
- Judul tab berbeda di tiap halaman.
- Status tidak hanya lewat warna.

## 9. Dial (untuk antislop)

Nilai ini mendekati kondisi aplikasi sekarang dan alasan dari Praproses. Pemilik desain boleh mengubahnya.

| Dial | Nilai | Alasan |
|---|---|---|
| ENERGY | 1 sampai 2 | Situs pemerintah, pengguna kerja dan baca; "modern" tetapi bukan pamer |
| RHYTHM | 1 sampai 2 | Banyak tabel dan formulir; halaman publik boleh sedikit lebih bervariasi |
| MOTION | 1 | Hanya umpan balik |

## 10. Yang masih terbuka

1. Warna merek resmi (lambang Kota Bogor) dan pemetaan warna status.
2. Pilihan font.
3. Istilah "Login/Thread" atau "Masuk/Topik".
4. Sumber dan izin foto Balai Kota.
5. Peringatan sebelum sesi berakhir karena tidak aktif.
6. Cara menandai data demo selama backend belum ada.
7. Label pencarian: "Cari dengan AI" atau "Pencarian pintar", sampai Smart Search (pgvector) benar-benar berjalan.
