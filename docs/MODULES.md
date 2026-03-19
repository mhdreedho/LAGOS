# Modules & Role Permission — Lagos Game Center

<!-- #region roles -->
## Role

| Role              | Deskripsi                                                           |
| ----------------- | ------------------------------------------------------------------- |
| **Administrator** | Pemilik usaha, akses penuh, mostly remote, mengelola fitur sensitif |
| **Operator**      | Staff di tempat, operasional harian, akses terbatas                 |

> Role default tidak bisa dihapus. Administrator bisa membuat custom role via GUI dengan permission per modul (CRUD).

<!-- #endregion -->

---

<!-- #region role-permission -->
## Role Permission Matrix

| Modul                        | Aksi                      | Administrator | Operator           |
| ---------------------------- | ------------------------- | ------------- | ------------------ |
| **Manajemen Unit PS**        | Lihat                     | ✓             | ✓                  |
|                              | Tambah/Edit/Hapus         | ✓             | ✗                  |
| **Manajemen Bilik**          | Lihat & Monitor           | ✓             | ✓                  |
|                              | Tambah/Edit/Hapus         | ✓             | ✗                  |
|                              | Kontrol Smart Plug Manual | ✓             | ✓                  |
| **Manajemen Harga & Paket**  | Lihat                     | ✓             | ✓                  |
|                              | Tambah/Edit/Hapus         | ✓             | ✗                  |
| **Sesi & Billing**           | Buka Sesi Prepaid         | ✓             | ✓                  |
|                              | Buka Open Billing         | ✓             | ✗                  |
|                              | Buka Close Billing        | ✓             | ✗                  |
|                              | Extend Waktu              | ✓             | ✓                  |
|                              | Tutup Sesi Normal         | ✓             | ✓                  |
|                              | Tutup Sesi Paksa          | ✓             | ✗                  |
|                              | Lihat Sesi Aktif          | ✓             | ✓                  |
|                              | Lihat Riwayat Sesi        | Semua         | Hari ini & kemarin |
| **Pembayaran**               | Terima Pembayaran         | ✓             | ✓                  |
|                              | Beri Diskon               | ✓             | ✗                  |
|                              | Void/Refund               | ✓             | ✗                  |
| **Member & Poin** *(Fase 2)* | Lihat                     | ✓             | ✓                  |
|                              | Tambah/Edit               | ✓             | ✓                  |
|                              | Hapus                     | ✓             | ✗                  |
| **Laporan Keuangan**         | Semua                     | ✓             | ✗                  |
| **Pengeluaran**              | Semua                     | ✓             | ✗                  |
| **Manajemen User & Role**    | Semua                     | ✓             | ✗                  |
| **Konfigurasi Sistem**       | Semua                     | ✓             | ✗                  |

<!-- #endregion -->

---

<!-- #region module-1 -->
## Modul 1 — Manajemen Unit PS

**Deskripsi:** Mendata inventaris unit PS beserta konfigurasi jaringan IoT.

**Fitur:**
- Tambah, edit, hapus unit PS
- Data inventaris: nama unit, jenis PS (PS4/PS5), nomor seri
- Data jaringan IoT: IP address smart plug, Device ID Tuya, Local Key Tuya
- Lihat daftar semua unit PS

**Role:** Administrator

<!-- #endregion -->

---

<!-- #region module-2 -->
## Modul 2 — Manajemen Bilik

**Deskripsi:** Mengelola data bilik, assign unit PS ke bilik, dan monitoring status IoT.

**Fitur:**
- Tambah, edit, hapus bilik
- Data bilik: nama/nomor bilik, tipe (Basic, Private, Premium, VIP)
- Assign unit PS ke bilik (1 bilik = 1 unit PS, bisa dikonfigurasi)
- Status bilik: Available, Occupied, Maintenance
- Monitor status smart plug real-time (TV nyala/mati)
- Deteksi anomali: TV nyala tapi tidak ada sesi aktif → flagged
- Log riwayat nyala/mati smart plug
- Kontrol smart plug manual (nyala/mati TV) — untuk kondisi darurat

**Role:** Administrator (kelola), Administrator & Operator (monitor & kontrol manual)

<!-- #endregion -->

---

<!-- #region module-3 -->
## Modul 3 — Manajemen Harga & Paket

**Deskripsi:** Konfigurasi harga paket per bilik dan harga open billing.

**Fitur:**
- Tambah, edit, hapus paket per bilik
- Paket durasi: 1 jam, 3 jam, 5 jam (bisa dikonfigurasi)
- Harga per paket per bilik
- Harga open billing per tipe bilik (harga per jam, bisa di-override saat buka sesi)
- Aktif/nonaktif paket tertentu

**Data Awal (seed):**

| Tipe Bilik | PS  | 1 Jam     | 3 Jam     | 5 Jam     |
| ---------- | --- | --------- | --------- | --------- |
| VIP        | PS5 | Rp 20.000 | Rp 54.000 | Rp 80.000 |
| Basic      | PS4 | Rp 8.000  | Rp 21.000 | Rp 30.000 |
| Private    | PS4 | Rp 10.000 | Rp 27.000 | Rp 40.000 |
| Premium    | PS4 | Rp 12.000 | Rp 33.000 | Rp 50.000 |

**Role:** Administrator

<!-- #endregion -->

---

<!-- #region module-4 -->
## Modul 4 — Sesi & Billing

**Deskripsi:** Mengelola sesi bermain pelanggan dari buka hingga tutup.

### Mode Sesi

| Mode              | Deskripsi                                          | Role                     |
| ----------------- | -------------------------------------------------- | ------------------------ |
| **Prepaid**       | Bayar di awal, paket durasi                        | Administrator & Operator |
| **Open Billing**  | Bayar di akhir, harga per jam, bisa override harga | Administrator only       |
| **Close Billing** | Harga Rp 0, internal use, wajib isi keterangan     | Administrator only       |

### Input Pelanggan
- Member → pilih dari data member
- Non-member → input nama manual
- Anonymous → isi nama "anonymous" (tidak di-hardcode)

### Alur Buka Sesi Prepaid
1. Operator pilih bilik yang available
2. Pilih paket durasi
3. Input nama pelanggan (member/non-member/anonymous)
4. Terima pembayaran (tunai/QRIS manual konfirmasi)
5. TV nyala otomatis via smart plug
6. Timer mulai berjalan

### Alur Buka Open Billing
1. Administrator pilih bilik
2. Input nama pelanggan
3. Konfirmasi harga per jam (bisa override)
4. TV nyala otomatis via smart plug
5. Timer mulai berjalan
6. Bayar di akhir saat sesi ditutup

### Alur Buka Close Billing
1. Administrator pilih bilik
2. Input keterangan (wajib)
3. TV nyala otomatis via smart plug
4. Timer mulai berjalan

### Extend Waktu
- Bisa dilakukan berkali-kali dalam satu sesi
- Bayar saat extend (tunai/QRIS)
- Waktu hangus saat sesi ditutup (tidak carry over)
- Member fase 2: saldo waktu tersimpan di akun *(Fase 2)*

### Tutup Sesi
- **Otomatis** — timer habis → TV mati → sesi selesai
- **Manual oleh Operator** — konfirmasi 2 langkah → grace period 5 menit → TV mati
- **Paksa/Override** — Administrator only, langsung tutup tanpa grace period

### Warning Waktu Habis
- 10 menit sebelum habis → indikator bilik berubah warna kuning + suara alert
- Saat habis → TV mati otomatis

### Riwayat Sesi
- Operator: lihat hari ini & kemarin saja
- Administrator: lihat semua history, export PDF

**Role:** Administrator (semua), Operator (terbatas — lihat tabel permission)

<!-- #endregion -->

---

<!-- #region module-5 -->
## Modul 5 — Pembayaran & Transaksi

**Deskripsi:** Mengelola pembayaran sesi dan extend waktu.

**Metode Pembayaran:**
- **Tunai** — langsung diterima operator
- **QRIS** — manual konfirmasi oleh operator (cek notif di HP)

**Fitur:**
- Terima pembayaran prepaid & extend
- Terima pembayaran open billing saat sesi ditutup
- Beri diskon (Administrator only)
- Void/refund transaksi (Administrator only)
- History transaksi

**Role:** Administrator (semua), Operator (terima pembayaran saja)

<!-- #endregion -->

---

<!-- #region module-6 -->
## Modul 6 — Member & Poin Reward *(Fase 2)*

**Deskripsi:** Manajemen data member dan sistem poin reward.

**Fitur:**
- Data member: nama, no HP, history transaksi
- Poin reward per transaksi
- Tukar poin dengan diskon
- Konfigurasi rules poin & promo via GUI oleh Administrator

**Catatan Database:**
> Kolom `poin_balance` di tabel `members` dan kolom `poin_earned`, `poin_used` di tabel `transactions` disiapkan dari awal dengan nilai 0. Tabel rules poin & promo dibuat saat fase 2.

**Role:** Administrator & Operator (lihat & tambah/edit), Administrator (hapus)

<!-- #endregion -->

---

<!-- #region module-7 -->
## Modul 7 — Laporan Keuangan

**Deskripsi:** Laporan pendapatan, pengeluaran, dan laba rugi usaha.

### Laporan Pendapatan
- Harian, mingguan, bulanan
- Per bilik, per tipe bilik
- Rekap metode pembayaran (tunai vs QRIS)
- Export PDF & Excel

### Pengeluaran
- Input manual oleh Administrator
- Field: kategori, keterangan, nominal, tanggal bayar (DD/MM/YYYY), tanggal periode (DD/MM/YYYY)
- Kategori bisa dikonfigurasi/ditambah sendiri
- Bukti pengeluaran: upload multiple foto (JPG/PNG)

### Laporan Laba Rugi
- Pendapatan - Pengeluaran per periode
- Filter berdasarkan Tanggal Periode
- Export PDF & Excel

**Catatan:**
> Sistem menggunakan pendekatan Cash Basis dengan field Tanggal Periode untuk fleksibilitas pencatatan beban.

**Role:** Administrator only

<!-- #endregion -->

---

<!-- #region module-8 -->
## Modul 8 — Manajemen User & Role (RBAC)

**Deskripsi:** Mengelola user dan konfigurasi role permission.

**Fitur:**
- Tambah, edit, hapus user
- Reset password user
- Assign role ke user
- Role default: Administrator & Operator (tidak bisa dihapus)
- Buat custom role baru
- Konfigurasi permission per modul per role via GUI (CRUD: Lihat, Tambah, Edit, Hapus)

**Role:** Administrator only

<!-- #endregion -->

---

<!-- #region module-9 -->
## Modul 9 — Konfigurasi Sistem

**Deskripsi:** Pengaturan identitas usaha dan konfigurasi struk.

**Fitur:**
- Identitas usaha: nama usaha, logo, slogan, alamat, no HP/WhatsApp
- Pengaturan struk: header (nama usaha, alamat, no HP) & footer (pesan penutup)

**Role:** Administrator only

<!-- #endregion -->
