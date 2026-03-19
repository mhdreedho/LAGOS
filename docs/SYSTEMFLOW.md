# System Flow — Lagos Game Center

> Dokumen ini menjelaskan alur kerja sistem per modul dari sisi pengguna (Administrator & Operator)
> dan sisi sistem (otomatisasi, IoT, timer).

---

<!-- #region flow-auth -->
## Alur Autentikasi

### Login
1. User buka aplikasi → tampil halaman login
2. Input email & password
3. Sistem validasi kredensial
4. Jika valid → redirect ke dashboard sesuai role
5. Jika tidak valid → tampil pesan error, tetap di halaman login

### Logout
1. User klik tombol logout
2. Sistem hapus session
3. Redirect ke halaman login

<!-- #endregion -->

---

<!-- #region flow-prepaid -->
## Alur Sesi Prepaid

> **Role:** Administrator & Operator

### Buka Sesi Prepaid
1. Operator lihat dashboard → tampil status semua bilik
2. Operator pilih bilik yang berstatus **available**
3. Sistem tampil form buka sesi:
   - Pilih pelanggan: **Member** (cari by nama/no HP) | **Non-member** (input nama manual) | **Anonymous**
   - Pilih paket durasi (tampil daftar paket aktif beserta harga)
   - Pilih metode bayar: **Tunai** | **QRIS**
4. Jika QRIS → operator tampilkan QR ke pelanggan → pelanggan bayar → operator konfirmasi manual
5. Jika Tunai → operator terima uang → konfirmasi
6. Sistem simpan data sesi & transaksi
7. Sistem kirim perintah ke TinyTuya → **TV nyala**
8. Status bilik berubah → **occupied**
9. Timer mulai berjalan

### Warning 10 Menit Sebelum Habis
1. Timer mencapai sisa 10 menit
2. Sistem trigger:
   - Indikator bilik di dashboard berubah warna **kuning**
   - **Suara alert** berbunyi di browser operator
3. Operator bisa tawarkan extend ke pelanggan

### Sesi Habis Otomatis
1. Timer mencapai 0
2. Sistem kirim perintah ke TinyTuya → **TV mati**
3. Status bilik berubah → **available**
4. Status sesi berubah → **selesai**
5. Indikator bilik di dashboard kembali normal

<!-- #endregion -->

---

<!-- #region flow-extend -->
## Alur Extend Waktu

> **Role:** Administrator & Operator

1. Operator pilih sesi yang sedang aktif
2. Sistem tampil form extend:
   - Pilih paket durasi tambahan
   - Pilih metode bayar: **Tunai** | **QRIS**
3. Jika QRIS → konfirmasi manual
4. Jika Tunai → konfirmasi
5. Sistem simpan data extend & transaksi
6. Timer bertambah sesuai durasi yang dipilih
7. Indikator bilik kembali normal (tidak kuning lagi jika sebelumnya warning)
8. Bisa dilakukan berkali-kali dalam satu sesi

<!-- #endregion -->

---

<!-- #region flow-close-manual -->
## Alur Tutup Sesi Manual

> **Role:** Operator (dengan konfirmasi) | Administrator (bisa paksa)

### Tutup Manual oleh Operator
1. Operator pilih sesi aktif → klik tombol **Tutup Sesi**
2. Sistem tampil dialog konfirmasi: *"Yakin ingin menutup sesi ini? Sisa waktu akan hangus."*
3. Operator konfirmasi
4. Status sesi berubah → **pending_close**
5. **Grace period 5 menit** dimulai — TV belum mati
6. Muncul indikator countdown grace period di dashboard
7. Operator masih bisa **batalkan** penutupan selama grace period
8. Jika grace period habis tanpa dibatalkan:
   - Sistem kirim perintah ke TinyTuya → **TV mati**
   - Status bilik → **available**
   - Status sesi → **selesai**

### Batalkan Tutup Sesi (selama grace period)
1. Operator klik tombol **Batalkan Penutupan**
2. Status sesi kembali → **aktif**
3. Timer lanjut dari sisa waktu yang ada
4. Indikator countdown grace period hilang

### Tutup Paksa oleh Administrator
1. Administrator pilih sesi aktif → klik **Tutup Paksa**
2. Sistem tampil dialog konfirmasi
3. Administrator konfirmasi
4. Sistem langsung kirim perintah ke TinyTuya → **TV mati**
5. Status bilik → **available**
6. Status sesi → **selesai**
7. Tidak ada grace period

<!-- #endregion -->

---

<!-- #region flow-open-billing -->
## Alur Open Billing

> **Role:** Administrator only

### Buka Open Billing
1. Administrator pilih bilik yang berstatus **available**
2. Sistem tampil form open billing:
   - Pilih pelanggan: Member | Non-member | Anonymous
   - Tampil harga per jam default untuk tipe bilik tersebut
   - Administrator bisa **override harga** jika perlu
3. Administrator konfirmasi
4. Sistem simpan data sesi (mode: open_billing)
5. Sistem kirim perintah ke TinyTuya → **TV nyala**
6. Status bilik → **occupied**
7. Timer mulai berjalan (akumulasi per jam)

### Tutup Open Billing
1. Administrator pilih sesi open billing aktif → klik **Tutup & Tagih**
2. Sistem hitung total: `(harga_per_jam / 60) × total_menit_aktual`
   - Dihitung per menit, tidak ada pembulatan
3. Sistem tampil total tagihan
4. Administrator pilih metode bayar: **Tunai** | **QRIS**
5. Konfirmasi pembayaran
6. Sistem simpan transaksi
7. Sistem kirim perintah ke TinyTuya → **TV mati**
8. Status bilik → **available**
9. Status sesi → **selesai**

<!-- #endregion -->

---

<!-- #region flow-close-billing -->
## Alur Close Billing

> **Role:** Administrator only

1. Administrator pilih bilik yang berstatus **available**
2. Sistem tampil form close billing:
   - Pilih pelanggan: Member | Non-member | Anonymous
   - Input keterangan (wajib diisi)
3. Administrator konfirmasi
4. Sistem simpan data sesi (mode: close_billing, harga: 0)
5. Sistem kirim perintah ke TinyTuya → **TV nyala**
6. Status bilik → **occupied**
7. Timer berjalan (untuk pencatatan durasi)
8. Sesi ditutup normal seperti prepaid (otomatis atau manual)

<!-- #endregion -->

---

<!-- #region flow-anomali -->
## Alur Deteksi Anomali Smart Plug

> Sistem otomatis — berjalan di background

1. Sistem polling status smart plug setiap **15 detik** via TinyTuya
2. Sistem bandingkan status smart plug dengan status sesi di database:
   - **TV nyala + ada sesi aktif** → normal, tidak ada aksi
   - **TV mati + tidak ada sesi** → normal, tidak ada aksi
   - **TV nyala + tidak ada sesi aktif** → **ANOMALI**
3. Jika anomali terdeteksi:
   - Status bilik di dashboard berubah → flagged **merah/warning**
   - Catat di `lgs_log_smart_plug` dengan trigger: **anomali**
   - Notifikasi muncul di dashboard operator & administrator
4. Administrator atau Operator bisa:
   - Matikan TV manual via dashboard
   - Atau buka sesi baru untuk bilik tersebut

<!-- #endregion -->

---

<!-- #region flow-member -->
## Alur Member

### Daftar Member Baru
1. Operator input data member: nama, no HP
2. Sistem cek no HP — harus unique
3. Jika sudah terdaftar → tampil pesan error
4. Jika belum → simpan data member, poin_balance = 0

### Gunakan Member saat Buka Sesi
1. Operator pilih **Member** saat buka sesi
2. Input nama atau no HP → sistem cari data member
3. Jika ditemukan → tampil nama & info member
4. Konfirmasi → lanjut proses sesi seperti biasa

<!-- #endregion -->

---

<!-- #region flow-pengeluaran -->
## Alur Input Pengeluaran

> **Role:** Administrator only

1. Administrator buka menu Pengeluaran → klik **Tambah Pengeluaran**
2. Sistem tampil form:
   - Pilih kategori (dropdown, bisa tambah kategori baru)
   - Input keterangan
   - Input nominal (Rupiah)
   - Input tanggal bayar (DD/MM/YYYY)
   - Input tanggal periode (DD/MM/YYYY)
   - Upload bukti (multiple JPG/PNG, opsional)
3. Administrator submit
4. Sistem simpan data pengeluaran & file bukti ke storage
5. Data langsung tercermin di laporan laba rugi sesuai tanggal periode

<!-- #endregion -->

---

<!-- #region flow-laporan -->
## Alur Laporan Keuangan

> **Role:** Administrator only

### Laporan Pendapatan
1. Administrator pilih periode (harian/mingguan/bulanan)
2. Sistem agregasi data dari `lgs_transaksi`
3. Tampil laporan: total pendapatan, breakdown per bilik, per tipe bilik, per metode bayar
4. Administrator bisa export **PDF** atau **Excel**

### Laporan Laba Rugi
1. Administrator pilih periode
2. Sistem agregasi:
   - **Pendapatan** → dari `lgs_transaksi` filter by `created_at`
   - **Pengeluaran** → dari `lgs_pengeluaran` filter by `tgl_periode`
3. Tampil laporan: pendapatan - pengeluaran = laba/rugi
4. Export **PDF** atau **Excel**

<!-- #endregion -->

---

<!-- #region flow-konfigurasi -->
## Alur Konfigurasi Sistem

> **Role:** Administrator only

1. Administrator buka menu Konfigurasi
2. Sistem tampil form dengan data saat ini:
   - Nama usaha, slogan, alamat, no HP/WhatsApp
   - Upload logo (JPG/PNG)
   - Header & footer struk
3. Administrator update → simpan
4. Perubahan langsung berlaku di semua tampilan & struk

<!-- #endregion -->

---

<!-- #region flow-iot -->
## Alur IoT — Smart Plug Control

> Semua perintah ke smart plug melalui **TinyTuya (Python)** via jaringan **WiFi dedicated** (tidak terhubung internet)

### Nyalakan TV (trigger dari sistem)
1. Sistem Laravel kirim request ke **Python service** (TinyTuya)
2. Python service cari device berdasarkan `ip_smart_plug` & `local_key_tuya` dari `lgs_unit_ps`
3. Kirim perintah **ON** ke smart plug
4. Smart plug nyalakan TV
5. Python service return status sukses/gagal ke Laravel
6. Jika gagal → sistem catat error, tampil notifikasi ke operator

### Matikan TV (trigger dari sistem)
1. Sistem Laravel kirim request ke Python service
2. Python service kirim perintah **OFF** ke smart plug
3. Smart plug matikan TV
4. Return status ke Laravel
5. Jika gagal → catat error, tampil notifikasi

### Kontrol Manual (dari dashboard)
1. Operator/Administrator klik tombol nyala/mati di dashboard bilik
2. Alur sama seperti trigger sistem di atas
3. Dicatat di `lgs_log_smart_plug` dengan trigger: **manual**

<!-- #endregion -->