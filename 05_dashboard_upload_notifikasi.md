# Test Case: Dashboard, Upload Dokumen & Notifikasi

## Halaman: Detail Pesanan Customer (`/customer/orders/{id}`)

### US-05: Mengunggah Dokumen Persyaratan di Dashboard
> **Sebagai** Customer yang sudah bayar, **saya ingin** melihat *uploader box* terpisah untuk masing-masing dokumen wajib, **sehingga** saya tidak bingung harus mengunggah file apa.

---

### Test Scenario 5.1: Merender Kotak Uploader (Slot File)
**Tujuan:** Memastikan sistem memunculkan uploader sejumlah syarat visa yang dibutuhkan.

* **Given** pesanan Customer berstatus `paid` (Telah dibayar).
* **When** Customer masuk ke halaman Detail Pesanan di Dashboard.
* **Then** sistem akan me-render bagian "Unggah Dokumen".
* **And** jumlah kotak uploader (*dropzone*) yang tampil HARUS sama persis dengan jumlah syarat dokumen visa tersebut (misal: Jika syaratnya 3, maka muncul 3 kotak).
* **And** setiap kotak harus menampilkan Judul Dokumen (Contoh: "1. Scan Paspor (Wajib)").

---

### Test Scenario 5.2: Reaksi Sistem Saat Upload File (Live Feedback)
**Tujuan:** Memastikan user mendapat respon instan bahwa filenya sedang atau berhasil diunggah ke *storage*.

* **Given** Customer memilih satu kotak uploader (misal: Scan Paspor).
* **When** Customer memasukkan *file* PDF/Gambar ke dalam kotak tersebut.
* **Then** sistem harus menampilkan *progress bar* atau status *uploading*.
* **And** setelah sukses tersimpan ke *storage*, kotak tersebut merubah status secara visual (misalnya memunculkan "Tanda Centang / Berhasil") dan menampilkan nama file (*filename*) yang diunggah.

---

### Test Scenario 5.3: Validasi Tombol "Submit Dokumen" (Penjagaan Kelengkapan)
**Tujuan:** Mencegah form tersubmit jika ada file wajib yang belum diunggah.

* **Given** terdapat 2 dokumen wajib (Paspor dan Foto) dan 1 dokumen opsional (Asuransi).
* **When** Customer HANYA mengunggah "Foto" dan "Asuransi".
* **And** Customer menekan tombol utama "Submit Dokumen ke Admin".
* **Then** sistem akan menahan aksi klik (*prevent default*).
* **And** sistem memunculkan toast error (pop-up merah): "Gagal Submit. Dokumen Paspor (Wajib) belum diunggah." (Atau kalimat serupa).

---

### Test Scenario 5.4: Submit Dokumen Sukses (Status `doc_received`)
**Tujuan:** Memastikan dokumen berhasil dikirim ke Admin dan *trigger* notifikasi bekerja.

* **Given** Customer telah mengunggah seluruh dokumen berstatus "Wajib".
* **When** Customer menekan tombol "Submit Dokumen".
* **Then** status pesanan berubah menjadi `doc_received` (Dokumen Diproses).
* **And** seluruh tombol/kotak uploader terkunci (*disabled*) tidak dapat diklik/diubah lagi oleh Customer.
* **And** (Background Job) Sistem mengirimkan In-App Notification dan Email ke Admin: "Ada dokumen pesanan baru masuk".
* **And** (Background Job) Sistem mengirimkan In-App Notification dan Email ke Customer: "Dokumen Anda sedang kami proses".

---

### US-06: Notifikasi Revisi dan Tautan WhatsApp

### Test Scenario 5.5: Tampilan Status Revisi di Layar Customer
**Tujuan:** Memastikan Customer tahu persis alasan ditolak dan dokumen mana yang harus diunggah ulang.

* **Given** Admin telah menolak dokumen "Paspor" dengan memberikan catatan "Gambar kabur" (Pesanan menjadi status `need_revision`).
* **When** Customer membuka halaman Detail Pesanannya.
* **Then** sistem akan menampilkan Badge Status "Perlu Revisi".
* **And** PADA BAGIAN UPLOADER, HANYA kotak "Paspor" yang terbuka kembali untuk diunggah ulang. (Dokumen lain yang sudah diterima tetap terkunci/disetujui).
* **And** tepat di bawah kotak "Paspor", muncul teks notifikasi (*Rejection Note*): "Alasan ditolak: Gambar kabur".
* **And** Customer menerima Email peringatan yang juga mencantumkan kalimat penolakan ("Gambar kabur") di dalamnya.

---

### Test Scenario 5.6: Interaksi Tombol Hubungi WhatsApp (Dynamic URL)
**Tujuan:** Memastikan *pre-filled text* WhatsApp berbeda-beda tergantung status pesanan saat ini.

* **Given** Customer berada di halaman Detail Pesanan #ORD-999 yang berstatus `waiting_payment`.
* **When** Customer menekan tombol "Hubungi Admin via WhatsApp".
* **Then** Browser membuka WhatsApp (API) dengan teks bawaan: "Halo Admin, saya mengalami kendala pembayaran untuk pesanan ORD-999..."
* **And** jika statusnya `need_revision`, teks bawaannya berubah menjadi: "Halo Admin, saya ingin bertanya tentang dokumen saya yang ditolak pada pesanan ORD-999..."

---

### Test Scenario 5.7: Mengunduh Visa Final (`completed`)
**Tujuan:** Memastikan URL atau Tombol unduh visa muncul di dalam Email saat visa selesai.

* **Given** Admin telah menyetujui seluruh dokumen dan mengunggah E-Visa final.
* **And** Admin mengubah status pesanan menjadi `completed`.
* **When** Customer menerima Email "Visa Anda Telah Terbit".
* **Then** Di dalam badan Email tersebut, HARUS terdapat tautan/tombol yang bisa langsung diklik untuk mengunduh e-visa.
* **And** di dalam Halaman Detail Pesanan Customer, tombol "Unduh E-Visa" (atau lampiran visa final) juga tersedia dengan jelas.
