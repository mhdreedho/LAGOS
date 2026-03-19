# Database Schema — Lagos Game Center

> **Konvensi:**
> - Prefix tabel: `lgs_` (tabel Lagos) — tabel bawaan Laravel tidak diberi prefix
> - Format kolom: `snake_case`
> - Primary key: `id` auto increment
> - Semua tabel memiliki kolom `created_at` dan `updated_at` (Laravel default)
> - Soft delete (`deleted_at`) ditandai khusus per tabel

---

<!-- #region users -->
## Tabel Bawaan Laravel

### users
> Tabel user bawaan Laravel — digunakan untuk login Administrator & Operator

| Kolom          | Tipe            | Keterangan          |
| -------------- | --------------- | ------------------- |
| id             | bigint unsigned | Primary key         |
| name           | varchar(255)    | Nama user           |
| email          | varchar(255)    | Email user (unique) |
| password       | varchar(255)    | Password ter-hash   |
| remember_token | varchar(100)    | Token remember me   |
| created_at     | timestamp       | —                   |
| updated_at     | timestamp       | —                   |

<!-- #endregion -->

---

<!-- #region lgs_roles -->
## Modul 8 — User & Role (RBAC)

### lgs_roles
> Daftar role — default: Administrator & Operator (tidak bisa dihapus)

| Kolom      | Tipe            | Keterangan                              |
| ---------- | --------------- | --------------------------------------- |
| id         | bigint unsigned | Primary key                             |
| nama       | varchar(100)    | Nama role                               |
| deskripsi  | text            | Deskripsi role                          |
| is_default | boolean         | True = role default, tidak bisa dihapus |
| created_at | timestamp       | —                                       |
| updated_at | timestamp       | —                                       |

### lgs_permissions
> Daftar permission yang tersedia per modul

| Kolom      | Tipe            | Keterangan                               |
| ---------- | --------------- | ---------------------------------------- |
| id         | bigint unsigned | Primary key                              |
| modul      | varchar(100)    | Nama modul (misal: bilik, sesi, laporan) |
| aksi       | varchar(50)     | Aksi (lihat, tambah, edit, hapus)        |
| deskripsi  | text            | Deskripsi permission                     |
| created_at | timestamp       | —                                        |
| updated_at | timestamp       | —                                        |

### lgs_role_permissions
> Pivot table — relasi role dan permission

| Kolom         | Tipe            | Keterangan              |
| ------------- | --------------- | ----------------------- |
| id            | bigint unsigned | Primary key             |
| role_id       | bigint unsigned | FK → lgs_roles.id       |
| permission_id | bigint unsigned | FK → lgs_permissions.id |
| created_at    | timestamp       | —                       |
| updated_at    | timestamp       | —                       |

### lgs_user_roles
> Pivot table — relasi user dan role

| Kolom      | Tipe            | Keterangan        |
| ---------- | --------------- | ----------------- |
| id         | bigint unsigned | Primary key       |
| user_id    | bigint unsigned | FK → users.id     |
| role_id    | bigint unsigned | FK → lgs_roles.id |
| created_at | timestamp       | —                 |
| updated_at | timestamp       | —                 |

<!-- #endregion -->

---

<!-- #region lgs_unit_ps -->
## Modul 1 — Manajemen Unit PS

### lgs_unit_ps
> Inventaris unit PS beserta konfigurasi jaringan IoT

| Kolom          | Tipe              | Keterangan                                 |
| -------------- | ----------------- | ------------------------------------------ |
| id             | bigint unsigned   | Primary key                                |
| nama           | varchar(100)      | Nama unit (misal: PS5-01, PS4-01)          |
| jenis_ps       | enum('PS4','PS5') | Jenis PS                                   |
| nomor_seri     | varchar(100)      | Nomor seri PS (opsional, untuk inventaris) |
| ip_smart_plug  | varchar(50)       | IP address smart plug di jaringan lokal    |
| device_id_tuya | varchar(100)      | Device ID dari Tuya cloud                  |
| local_key_tuya | varchar(100)      | Local key enkripsi TinyTuya                |
| created_at     | timestamp         | —                                          |
| updated_at     | timestamp         | —                                          |
| deleted_at     | timestamp         | Soft delete                                |

<!-- #endregion -->

---

<!-- #region lgs_bilik -->
## Modul 2 — Manajemen Bilik

### lgs_bilik
> Data bilik/room beserta status dan relasi ke unit PS

| Kolom      | Tipe                                       | Keterangan                                 |
| ---------- | ------------------------------------------ | ------------------------------------------ |
| id         | bigint unsigned                            | Primary key                                |
| nama       | varchar(100)                               | Nama/nomor bilik (misal: VIP-01, Basic-01) |
| tipe       | enum('Basic','Private','Premium','VIP')    | Tipe bilik                                 |
| unit_ps_id | bigint unsigned                            | FK → lgs_unit_ps.id (nullable)             |
| status     | enum('available','occupied','maintenance') | Status bilik saat ini                      |
| created_at | timestamp                                  | —                                          |
| updated_at | timestamp                                  | —                                          |
| deleted_at | timestamp                                  | Soft delete                                |

### lgs_log_smart_plug
> Log riwayat nyala/mati smart plug per bilik

| Kolom      | Tipe                              | Keterangan                                    |
| ---------- | --------------------------------- | --------------------------------------------- |
| id         | bigint unsigned                   | Primary key                                   |
| bilik_id   | bigint unsigned                   | FK → lgs_bilik.id                             |
| status     | enum('on','off')                  | Status smart plug                             |
| trigger    | enum('sistem','manual','anomali') | Sumber perintah                               |
| user_id    | bigint unsigned                   | FK → users.id (nullable, jika trigger manual) |
| created_at | timestamp                         | Waktu kejadian                                |
| updated_at | timestamp                         | —                                             |

<!-- #endregion -->

---

<!-- #region lgs_paket -->
## Modul 3 — Manajemen Harga & Paket

### lgs_paket
> Paket durasi dan harga per bilik

| Kolom        | Tipe            | Keterangan                                     |
| ------------ | --------------- | ---------------------------------------------- |
| id           | bigint unsigned | Primary key                                    |
| bilik_id     | bigint unsigned | FK → lgs_bilik.id                              |
| durasi_menit | integer         | Durasi paket dalam menit (misal: 60, 180, 300) |
| harga        | integer         | Harga paket dalam rupiah                       |
| is_aktif     | boolean         | Status aktif/nonaktif paket                    |
| created_at   | timestamp       | —                                              |
| updated_at   | timestamp       | —                                              |
| deleted_at   | timestamp       | Soft delete                                    |

### lgs_harga_open_billing
> Harga per jam untuk mode open billing per tipe bilik

| Kolom         | Tipe                                    | Keterangan                         |
| ------------- | --------------------------------------- | ---------------------------------- |
| id            | bigint unsigned                         | Primary key                        |
| tipe_bilik    | enum('Basic','Private','Premium','VIP') | Tipe bilik                         |
| harga_per_jam | integer                                 | Harga default open billing per jam |
| created_at    | timestamp                               | —                                  |
| updated_at    | timestamp                               | —                                  |

<!-- #endregion -->

---

<!-- #region lgs_member -->
## Modul 6 — Member *(kolom poin disiapkan untuk Fase 2)*

### lgs_member
> Data member pelanggan

| Kolom        | Tipe            | Keterangan                             |
| ------------ | --------------- | -------------------------------------- |
| id           | bigint unsigned | Primary key                            |
| nama         | varchar(255)    | Nama member                            |
| no_hp        | varchar(20)     | Nomor HP (unique)                      |
| poin_balance | integer         | Saldo poin (default: 0) — aktif Fase 2 |
| created_at   | timestamp       | —                                      |
| updated_at   | timestamp       | —                                      |
| deleted_at   | timestamp       | Soft delete                            |

<!-- #endregion -->

---

<!-- #region lgs_sesi -->
## Modul 4 — Sesi & Billing

### lgs_sesi
> Data sesi bermain pelanggan

| Kolom          | Tipe                                           | Keterangan                                         |
| -------------- | ---------------------------------------------- | -------------------------------------------------- |
| id             | bigint unsigned                                | Primary key                                        |
| bilik_id       | bigint unsigned                                | FK → lgs_bilik.id                                  |
| member_id      | bigint unsigned                                | FK → lgs_member.id (nullable)                      |
| nama_pelanggan | varchar(255)                                   | Nama pelanggan (non-member/anonymous)              |
| mode           | enum('prepaid','open_billing','close_billing') | Mode sesi                                          |
| status         | enum('aktif','pending_close','selesai')        | Status sesi                                        |
| waktu_mulai    | timestamp                                      | Waktu sesi dimulai                                 |
| waktu_selesai  | timestamp                                      | Waktu sesi berakhir (nullable, diisi saat selesai) |
| durasi_menit   | integer                                        | Total durasi yang dibeli (prepaid)                 |
| harga_per_jam  | integer                                        | Harga per jam (open billing, nullable)             |
| harga_override | integer                                        | Override harga open billing (nullable)             |
| keterangan     | text                                           | Keterangan close billing (nullable)                |
| user_id        | bigint unsigned                                | FK → users.id (operator/admin yang buka sesi)      |
| created_at     | timestamp                                      | —                                                  |
| updated_at     | timestamp                                      | —                                                  |

### lgs_sesi_extend
> Log extend waktu per sesi

| Kolom               | Tipe            | Keterangan                           |
| ------------------- | --------------- | ------------------------------------ |
| id                  | bigint unsigned | Primary key                          |
| sesi_id             | bigint unsigned | FK → lgs_sesi.id                     |
| durasi_tambah_menit | integer         | Durasi tambahan dalam menit          |
| harga               | integer         | Harga extend                         |
| user_id             | bigint unsigned | FK → users.id (operator yang extend) |
| created_at          | timestamp       | —                                    |
| updated_at          | timestamp       | —                                    |

<!-- #endregion -->

---

<!-- #region lgs_transaksi -->
## Modul 5 — Pembayaran & Transaksi

### lgs_transaksi
> Data transaksi pembayaran

| Kolom        | Tipe                                    | Keterangan                                      |
| ------------ | --------------------------------------- | ----------------------------------------------- |
| id           | bigint unsigned                         | Primary key                                     |
| sesi_id      | bigint unsigned                         | FK → lgs_sesi.id                                |
| tipe         | enum('prepaid','extend','open_billing') | Tipe transaksi                                  |
| metode_bayar | enum('tunai','qris')                    | Metode pembayaran                               |
| total        | integer                                 | Total pembayaran dalam rupiah                   |
| diskon       | integer                                 | Nominal diskon (default: 0)                     |
| total_bayar  | integer                                 | Total setelah diskon                            |
| status       | enum('lunas','void')                    | Status transaksi                                |
| poin_earned  | integer                                 | Poin yang didapat (default: 0) — aktif Fase 2   |
| poin_used    | integer                                 | Poin yang digunakan (default: 0) — aktif Fase 2 |
| user_id      | bigint unsigned                         | FK → users.id (operator yang proses)            |
| created_at   | timestamp                               | —                                               |
| updated_at   | timestamp                               | —                                               |

<!-- #endregion -->

---

<!-- #region lgs_pengeluaran -->
## Modul 7 — Laporan Keuangan

### lgs_kategori_pengeluaran
> Kategori pengeluaran usaha (bisa dikonfigurasi)

| Kolom      | Tipe            | Keterangan                                     |
| ---------- | --------------- | ---------------------------------------------- |
| id         | bigint unsigned | Primary key                                    |
| nama       | varchar(100)    | Nama kategori (misal: Listrik, PDAM, Internet) |
| created_at | timestamp       | —                                              |
| updated_at | timestamp       | —                                              |
| deleted_at | timestamp       | Soft delete                                    |

### lgs_pengeluaran
> Data pengeluaran usaha

| Kolom       | Tipe            | Keterangan                                        |
| ----------- | --------------- | ------------------------------------------------- |
| id          | bigint unsigned | Primary key                                       |
| kategori_id | bigint unsigned | FK → lgs_kategori_pengeluaran.id                  |
| keterangan  | text            | Keterangan pengeluaran                            |
| nominal     | integer         | Nominal pengeluaran dalam rupiah                  |
| tgl_bayar   | date            | Tanggal uang benar-benar dikeluarkan (DD/MM/YYYY) |
| tgl_periode | date            | Tanggal periode beban ditanggung (DD/MM/YYYY)     |
| user_id     | bigint unsigned | FK → users.id (admin yang input)                  |
| created_at  | timestamp       | —                                                 |
| updated_at  | timestamp       | —                                                 |
| deleted_at  | timestamp       | Soft delete                                       |

### lgs_bukti_pengeluaran
> Bukti pengeluaran (multiple files per pengeluaran)

| Kolom          | Tipe            | Keterangan              |
| -------------- | --------------- | ----------------------- |
| id             | bigint unsigned | Primary key             |
| pengeluaran_id | bigint unsigned | FK → lgs_pengeluaran.id |
| nama_file      | varchar(255)    | Nama file asli          |
| path           | varchar(255)    | Path file di storage    |
| created_at     | timestamp       | —                       |
| updated_at     | timestamp       | —                       |

<!-- #endregion -->

---

<!-- #region lgs_konfigurasi -->
## Modul 9 — Konfigurasi Sistem

### lgs_konfigurasi
> Pengaturan identitas usaha dan struk

| Kolom      | Tipe            | Keterangan                   |
| ---------- | --------------- | ---------------------------- |
| id         | bigint unsigned | Primary key                  |
| key        | varchar(100)    | Kunci konfigurasi (unique)   |
| value      | text            | Nilai konfigurasi            |
| keterangan | varchar(255)    | Keterangan kunci konfigurasi |
| created_at | timestamp       | —                            |
| updated_at | timestamp       | —                            |

> Contoh key: `nama_usaha`, `slogan`, `alamat`, `no_hp`, `logo_path`, `struk_header`, `struk_footer`

<!-- #endregion -->

---

<!-- #region relasi -->
## Relasi Antar Tabel

```
users
  ├── lgs_user_roles → lgs_roles → lgs_role_permissions → lgs_permissions
  ├── lgs_sesi (user_id = operator yang buka)
  ├── lgs_transaksi (user_id = operator yang proses)
  ├── lgs_pengeluaran (user_id = admin yang input)
  └── lgs_log_smart_plug (user_id = user yang kontrol manual)

lgs_unit_ps
  └── lgs_bilik (unit_ps_id)

lgs_bilik
  ├── lgs_paket (bilik_id)
  ├── lgs_sesi (bilik_id)
  └── lgs_log_smart_plug (bilik_id)

lgs_sesi
  ├── lgs_sesi_extend (sesi_id)
  ├── lgs_transaksi (sesi_id)
  └── lgs_member (member_id, nullable)

lgs_pengeluaran
  ├── lgs_kategori_pengeluaran (kategori_id)
  └── lgs_bukti_pengeluaran (pengeluaran_id)
```

<!-- #endregion -->