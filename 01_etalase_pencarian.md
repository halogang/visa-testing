# Test Case: Landing Page & Search (Etalase)

## Halaman: Beranda (Landing Page) & Hasil Pencarian

### US-01: Menelusuri Etalase (Katalog) Produk Visa
> **Sebagai** Pengunjung (*Visitor*), **saya ingin** melihat katalog visa yang menampilkan informasi dasar secara lengkap di dalam satu kotak/kartu (*card*), **sehingga** saya bisa membandingkan harga dan mengetahui tipe visa tanpa harus masuk ke halaman detail.

---

### Test Scenario 1.1: Kelengkapan Data pada Kartu Produk Visa (Visa Card)
**Tujuan:** Memastikan semua informasi wajib (mandatory fields) tampil dengan benar pada setiap kotak/kartu visa di katalog.

* **Given** pengunjung berada di halaman Beranda atau Hasil Pencarian.
* **When** sistem memuat daftar visa yang tersedia.
* **Then** setiap kartu produk (Visa Card) HARUS menampilkan secara persis elemen berikut:
  1. Gambar latar belakang / thumbnail (Foto negara tujuan).
  2. Label Negara (Contoh: "Jepang").
  3. Nama Produk Visa (Contoh: "Single Entry Tourist Visa").
  4. Badge Kategori (Contoh: "Turis" / "Bisnis").
  5. Estimasi Waktu Proses (Contoh: "Proses 5-7 Hari Kerja").
  6. Harga Final dengan format Rupiah (Contoh: "Rp 1.200.000").
  7. Tombol interaktif "Lihat Detail".

---

### Test Scenario 1.2: Fungsionalitas Pencarian via Teks
**Tujuan:** Memastikan kolom pencarian mereturn data yang akurat.

* **Given** pengunjung berada di halaman Beranda (Landing Page).
* **When** pengunjung mengetikkan keyword "Jepang" di kolom pencarian utama.
* **And** pengunjung menekan tombol "Cari" (atau menekan Enter).
* **Then** sistem akan mengarahkan pengunjung ke halaman Hasil Pencarian.
* **And** sistem HANYA menampilkan grid kartu produk Visa yang relevan dengan keyword "Jepang".

---

### Test Scenario 1.3: Fungsionalitas Klik Destinasi Favorit
**Tujuan:** Memastikan fitur jalan pintas (shortcut) filter negara berjalan dengan baik.

* **Given** pengunjung berada di halaman Beranda.
* **When** pengunjung menggulir ke bagian "Destinasi Favorit".
* **And** pengunjung mengklik bendera/gambar negara "Australia".
* **Then** sistem akan langsung mengarahkan (*redirect*) pengunjung ke hasil pencarian yang terfilter khusus untuk negara "Australia".

---

### Test Scenario 1.4: Aksi Klik Kartu Produk (Navigasi)
**Tujuan:** Memastikan kartu produk dapat diklik dan mengarah ke detail yang benar.

* **Given** pengunjung berada di halaman katalog (Hasil Pencarian).
* **When** pengunjung mengklik kotak (kartu) visa "Tourist Visa Jepang".
* **Then** sistem menampilkan animasi *loading* singkat (progress bar / spinner).
* **And** URL berubah menuju slug produk tersebut (`/visa/tourist-visa-jepang`).
* **And** halaman Detail Visa untuk "Tourist Visa Jepang" termuat penuh.
