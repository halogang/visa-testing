# Test Case: Visa Detail Page

## Halaman: Detail Produk Visa (`/visa/{slug}`)

### US-02: Transparansi Halaman Detail Visa
> **Sebagai** Pengunjung, **saya ingin** membaca detail lengkap mulai dari deskripsi, *Highlight* produk, hingga *List* persyaratan dokumen di halaman produk, **sehingga** saya tahu pasti apa yang saya beli.

---

### Test Scenario 2.1: Memuat Data Lengkap Halaman Detail
**Tujuan:** Memastikan halaman detail visa memuat semua komponen informasi yang dibutuhkan untuk membangun kepercayaan pelanggan.

* **Given** pengunjung berada di halaman Detail Visa (misalnya untuk produk "Visa Turis Jepang").
* **When** halaman selesai dirender sepenuhnya.
* **Then** halaman harus terbagi menjadi bagian-bagian berikut secara berurutan atau jelas:
  1. **Header/Hero Section**: Memuat Foto resolusi tinggi dari negara tersebut, Nama Produk Visa, dan Harga Final.
  2. **Bagian Deskripsi**: Memuat paragraf penjelasan produk.
  3. **Bagian Termasuk/Tidak Termasuk (Includes/Excludes)**: Memuat daftar (*bullet points*) fitur.
  4. **Bagian Persyaratan Dokumen**: Memuat daftar dokumen apa saja yang harus disiapkan.
  5. **Sticky Button**: Harus terdapat tombol utama "Pesan Sekarang" yang *sticky* (terus terlihat di bagian bawah atau sisi layar meski halaman di-scroll).

---

### Test Scenario 2.2: Tampilan Status Dokumen Wajib vs Opsional
**Tujuan:** Memastikan pengguna tahu pasti dokumen mana yang sifatnya wajib diunggah (mandatory) dan mana yang hanya opsional.

* **Given** pengunjung berada di halaman Detail Visa.
* **When** pengunjung menggulir dan melihat ke bagian "Persyaratan Dokumen".
* **Then** sistem harus menampilkan daftar `requirement_items` secara lengkap.
* **And** sistem HARUS memberikan label pembeda secara visual antara dokumen yang "Wajib" dan "Opsional". (Misalnya: Dokumen yang wajib memiliki label *badge* berwarna merah berbunyi "Wajib", atau memiliki tanda asteris `*` merah).

---

### Test Scenario 2.3: Reaksi Klik Tombol Pesan (Authentication Guard)
**Tujuan:** Memastikan hanya pengguna yang sudah terdaftar dan *login* yang bisa masuk ke proses checkout.

* **Given** pengunjung belum Login (berstatus Guest).
* **And** pengunjung berada di halaman Detail Visa.
* **When** pengunjung mengklik tombol "Pesan Sekarang".
* **Then** tombol tersebut akan memunculkan *loading spinner* sedetik.
* **And** sistem mengarahkan pengunjung (*Redirect*) ke halaman `/login`.
* **And** sistem memunculkan toast/alert message: "Silakan masuk (login) untuk melanjutkan pemesanan."
* **And** (Opsional/Sangat Disarankan) sistem mengingat halaman terakhir, sehingga jika pengunjung berhasil login, mereka akan otomatis dikembalikan ke form pemesanan visa tersebut.
