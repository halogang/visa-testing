# Test Case: Form Pemesanan (Checkout Data Entry)

## Halaman: Formulir Detail Pemohon (`/checkout/details/{slug}`)

### US-03: Mengisi Form Data Pemohon (Checkout Form)
> **Sebagai** Customer (yang sudah login), **saya ingin** dihadapkan pada formulir yang jelas field-fieldnya, **sehingga** saya tidak salah memasukkan data untuk pesanan saya.

---

### Test Scenario 3.1: Kelengkapan Field Input Pemohon
**Tujuan:** Memastikan formulir checkout meminta semua field data yang diwajibkan oleh bisnis (Nama, Email, WA, Negara).

* **Given** Customer (yang sudah berhasil Login) baru saja mengklik "Pesan Sekarang" pada detail produk visa.
* **When** halaman "Form Detail Pemohon" terbuka dan selesai dimuat.
* **Then** sistem harus menampilkan *input fields* berikut yang bersifat WAJIB diisi:
  1. **Input Text:** "Nama Lengkap Sesuai Paspor"
  2. **Input Email:** "Alamat Email Aktif"
  3. **Input Phone:** "Nomor WhatsApp"
  4. **Select Dropdown:** "Negara Asal (Kewarganegaraan)"
* **And** harus terdapat tombol "Lanjut Tinjau Pesanan" (tombol navigasi lanjutan) di bagian bawah formulir.

---

### Test Scenario 3.2: Validasi Field Kosong Saat Klik Lanjut (Error State)
**Tujuan:** Memastikan *form validation* berfungsi mencegah pengiriman data jika ada field wajib yang belum diisi.

* **Given** Customer berada di halaman Formulir Pemesanan.
* **When** Customer dengan sengaja mengosongkan field "Nomor WhatsApp" (atau field wajib lainnya).
* **And** Customer menekan tombol "Lanjut Tinjau Pesanan".
* **Then** form tidak akan tersubmit dan tidak berpindah halaman.
* **And** *border* atau kotak pada kolom "Nomor WhatsApp" berubah warna menjadi peringatan (contoh: Merah).
* **And** muncul teks bantuan (*helper text*) berwarna merah tepat di bawah kolom tersebut: "Nomor WhatsApp wajib diisi." (atau pesan validasi sejenis).

---

### Test Scenario 3.3: Validasi Format Email
**Tujuan:** Memastikan field email hanya menerima format alamat email yang valid.

* **Given** Customer berada di halaman Formulir Pemesanan.
* **When** Customer memasukkan teks "bukan-email-yang-benar" pada kolom Alamat Email Aktif.
* **And** Customer menekan tombol "Lanjut Tinjau Pesanan".
* **Then** sistem akan mencegah form tersubmit.
* **And** memunculkan teks error validasi: "Format email tidak valid" di bawah field Email.

---

### Test Scenario 3.4: Form Tersubmit Sukses
**Tujuan:** Memastikan data yang valid diproses dengan benar dan memajukan *Customer* ke langkah selanjutnya.

* **Given** Customer berada di halaman Formulir Pemesanan.
* **When** Customer mengisi seluruh field wajib dengan data yang benar dan valid.
* **And** Customer menekan tombol "Lanjut Tinjau Pesanan".
* **Then** sistem akan memproses data tersebut secara sukses (tanpa error).
* **And** pengunjung langsung diarahkan (*redirect*) menuju halaman "Tinjauan Pesanan (Review Page)".
