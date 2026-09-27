# Test Case: Review Pesanan & Pembayaran (Checkout Review)

## Halaman: Tinjauan Pesanan & Midtrans (`/checkout/review`)

### US-04: Tinjauan Pesanan (Review Page)
> **Sebagai** Customer, **saya ingin** meninjau pesanan untuk terakhir kali dengan rincian biaya yang terurai (breakdown), **sehingga** tidak ada biaya tersembunyi.

---

### Test Scenario 4.1: Kelengkapan Tampilan Halaman Review
**Tujuan:** Memastikan halaman ulasan menampilkan seluruh rekap data pesanan dengan jelas sebelum customer diarahkan ke pembayaran.

* **Given** Customer berhasil melewati form data pemohon.
* **When** halaman Review Pesanan terbuka dan selesai dimuat.
* **Then** halaman harus secara eksplisit memuat blok informasi berikut:
  1. **Kotak "Detail Visa"**: Menampilkan nama visa (Contoh: "Single Entry Tourist") dan gambar negara.
  2. **Kotak "Data Pemohon"**: Menampilkan ringkasan input nama, email, WA, dan negara yang tadi diisi.
  3. **Kotak "Persiapan Anda Nanti (Checklist Dokumen)"**: Menampilkan ulang list syarat-syarat yang WAJIB diunggah setelah pembayaran.
  4. **Kotak "Rincian Biaya (Breakdown)"**: Menampilkan `Subtotal`, `Biaya Layanan/Admin`, dan `Total Bayar`.
* **And** harus ada tombol utama (Primary Button) "Bayar Sekarang".

---

### Test Scenario 4.2: Reaksi Klik "Bayar Sekarang" (Integrasi Midtrans)
**Tujuan:** Memastikan tombol bayar sukses membuat pesanan di database dan memanggil antarmuka Midtrans Snap.

* **Given** Customer berada di halaman Review Pesanan.
* **When** Customer menekan "Bayar Sekarang".
* **Then** tombol berubah menjadi state *loading* ("Memproses...").
* **And** sistem membuat record `orders` di database dengan status `waiting_payment`.
* **And** *interface* Snap Modal Midtrans muncul di tengah layar (*overlay*) untuk memasukkan data metode pembayaran.
* **And** sistem mengirim Notifikasi Email kepada Customer berisi Instruksi Pembayaran.

---

### Test Scenario 4.3: Perubahan Status Pasca Pembayaran (Webhook Midtrans)
**Tujuan:** Memastikan sistem ELVISA bisa merespons status sukses dari Midtrans.
*(Catatan QA: Test ini bisa disimulasikan menggunakan API testing (Postman) yang menembak endpoint webhook, atau membayar langsung di Sandbox Midtrans)*

* **Given** Customer telah berhasil menyelesaikan pembayaran di modal Midtrans (mendapat status Success).
* **When** Midtrans mengirimkan notifikasi Webhook (*Settlement*) ke sistem ELVISA.
* **Then** status order Customer di database berubah secara otomatis menjadi `paid`.
* **And** Customer otomatis diarahkan kembali (redirect) ke halaman Detail Pesanan di Dashboard Customer.
