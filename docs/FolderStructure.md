# Folder & File Structure — Lagos Game Center

> Dokumen ini mendefinisikan konvensi struktur folder dan file project Lagos.
> Folder bawaan Laravel yang tidak dimodifikasi tidak dicantumkan di sini.

---

<!-- #region app -->
## `app/`

```
app/
├── Http/
│   └── Controllers/
│       └── (kosong — semua UI pakai Livewire, controller hanya untuk API jika diperlukan)
│
├── Livewire/                          # Komponen Livewire, diorganisir per modul
│   ├── Auth/                          # Login, logout
│   ├── Dashboard/                     # Halaman utama, monitoring bilik
│   ├── UnitPs/                        # Modul 1 — Manajemen Unit PS
│   ├── Bilik/                         # Modul 2 — Manajemen Bilik
│   ├── Paket/                         # Modul 3 — Manajemen Harga & Paket
│   ├── Sesi/                          # Modul 4 — Sesi & Billing
│   ├── Transaksi/                     # Modul 5 — Pembayaran & Transaksi
│   ├── Member/                        # Modul 6 — Member (Fase 2)
│   ├── Laporan/                       # Modul 7 — Laporan Keuangan
│   ├── Pengeluaran/                   # Modul 7 — Pengeluaran
│   ├── UserRole/                      # Modul 8 — Manajemen User & Role
│   └── Konfigurasi/                   # Modul 9 — Konfigurasi Sistem
│
├── Models/                            # Eloquent Models — flat, tidak per modul
│   ├── User.php                       # Bawaan Laravel
│   ├── Role.php                       # lgs_roles
│   ├── Permission.php                 # lgs_permissions
│   ├── RolePermission.php             # lgs_role_permissions
│   ├── UserRole.php                   # lgs_user_roles
│   ├── UnitPs.php                     # lgs_unit_ps
│   ├── Bilik.php                      # lgs_bilik
│   ├── LogSmartPlug.php               # lgs_log_smart_plug
│   ├── Paket.php                      # lgs_paket
│   ├── HargaOpenBilling.php           # lgs_harga_open_billing
│   ├── Member.php                     # lgs_member
│   ├── Sesi.php                       # lgs_sesi
│   ├── SesiExtend.php                 # lgs_sesi_extend
│   ├── Transaksi.php                  # lgs_transaksi
│   ├── KategoriPengeluaran.php        # lgs_kategori_pengeluaran
│   ├── Pengeluaran.php                # lgs_pengeluaran
│   ├── BuktiPengeluaran.php           # lgs_bukti_pengeluaran
│   └── Konfigurasi.php                # lgs_konfigurasi
│
├── Services/                          # Business logic, diorganisir per modul
│   ├── UnitPsService.php
│   ├── BilikService.php
│   ├── PaketService.php
│   ├── SesiService.php                # Logic buka/tutup/extend sesi, hitung timer
│   ├── TransaksiService.php           # Logic pembayaran, diskon, void
│   ├── MemberService.php              # Fase 2
│   ├── LaporanService.php             # Logic agregasi laporan, export PDF/Excel
│   ├── PengeluaranService.php
│   ├── UserRoleService.php
│   ├── KonfigurasiService.php
│   └── IotService.php                 # Komunikasi ke Python service (TinyTuya)
│
└── Traits/                            # Reusable traits
    └── HasPermission.php              # Trait untuk cek permission RBAC
```

<!-- #endregion -->

---

<!-- #region resources -->
## `resources/`

```
resources/
├── views/                             # Blade views, diorganisir per modul
│   ├── layouts/
│   │   ├── app.blade.php              # Layout utama aplikasi
│   │   └── guest.blade.php            # Layout halaman login
│   ├── components/                    # Blade components reusable
│   │   ├── bilik-card.blade.php       # Card status bilik di dashboard
│   │   ├── timer.blade.php            # Komponen timer sesi
│   │   └── alert.blade.php            # Komponen notifikasi/alert
│   ├── auth/
│   │   └── login.blade.php
│   ├── dashboard/
│   │   └── index.blade.php            # Halaman utama monitoring bilik
│   ├── unit-ps/
│   ├── bilik/
│   ├── paket/
│   ├── sesi/
│   ├── transaksi/
│   ├── member/                        # Fase 2
│   ├── laporan/
│   ├── pengeluaran/
│   ├── user-role/
│   └── konfigurasi/
│
└── js/
    └── app.js                         # Entry point JavaScript (Vite)
```

<!-- #endregion -->

---

<!-- #region routes -->
## `routes/`

```
routes/
├── web.php                            # Entry point — include semua route modul
├── auth.php                           # Route login/logout
└── modules/                           # Route per modul
    ├── dashboard.php
    ├── unit-ps.php
    ├── bilik.php
    ├── paket.php
    ├── sesi.php
    ├── transaksi.php
    ├── member.php                     # Fase 2
    ├── laporan.php
    ├── pengeluaran.php
    ├── user-role.php
    └── konfigurasi.php
```

> `web.php` hanya berisi require ke masing-masing file modul:
> ```php
> require __DIR__.'/auth.php';
> require __DIR__.'/modules/dashboard.php';
> // dst...
> ```

<!-- #endregion -->

---

<!-- #region database -->
## `database/`

```
database/
├── migrations/                        # Urutan migration sesuai dependency tabel
│   ├── 0001_create_users_table.php    # Bawaan Laravel
│   ├── 0002_create_lgs_roles_table.php
│   ├── 0003_create_lgs_permissions_table.php
│   ├── 0004_create_lgs_role_permissions_table.php
│   ├── 0005_create_lgs_user_roles_table.php
│   ├── 0006_create_lgs_unit_ps_table.php
│   ├── 0007_create_lgs_bilik_table.php
│   ├── 0008_create_lgs_log_smart_plug_table.php
│   ├── 0009_create_lgs_paket_table.php
│   ├── 0010_create_lgs_harga_open_billing_table.php
│   ├── 0011_create_lgs_member_table.php
│   ├── 0012_create_lgs_sesi_table.php
│   ├── 0013_create_lgs_sesi_extend_table.php
│   ├── 0014_create_lgs_transaksi_table.php
│   ├── 0015_create_lgs_kategori_pengeluaran_table.php
│   ├── 0016_create_lgs_pengeluaran_table.php
│   ├── 0017_create_lgs_bukti_pengeluaran_table.php
│   └── 0018_create_lgs_konfigurasi_table.php
│
└── seeders/                           # Data awal
    ├── DatabaseSeeder.php             # Entry point seeder
    ├── RoleSeeder.php                 # Seed role default (Administrator, Operator)
    ├── PermissionSeeder.php           # Seed permission per modul
    ├── AdminSeeder.php                # Seed user Administrator default
    ├── BilikSeeder.php                # Seed data bilik awal (12 unit)
    └── PaketSeeder.php                # Seed harga paket awal
```

<!-- #endregion -->

---

<!-- #region docs -->
## `docs/`

```
docs/
├── TECHSTACK.md                       # Tech stack & environment
├── SYSTEMFLOW.md                      # Alur kerja sistem per modul
├── MODULES.md                         # Daftar modul & role permission matrix
├── SCHEMA.md                          # Database schema lengkap
├── FOLDER_STRUCTURE.md                # Dokumen ini
└── PROGRESS.md                        # Tracking progress development
```

<!-- #endregion -->

---

<!-- #region python -->
## `python/` *(IoT Service)*

```
python/
├── iot_service.py                     # Entry point Python service (TinyTuya)
├── requirements.txt                   # Dependensi Python
└── README.md                          # Cara setup & jalankan Python service
```

> Python service berjalan terpisah dari Laravel.
> Laravel berkomunikasi ke Python service via HTTP request lokal.

<!-- #endregion -->

---

<!-- #region konvensi -->
## Konvensi Penamaan

| Tipe File          | Konvensi                    | Contoh                            |
| ------------------ | --------------------------- | --------------------------------- |
| Model              | PascalCase                  | `UnitPs.php`, `SesiExtend.php`    |
| Livewire Component | PascalCase                  | `BukaSesi.php`, `DaftarBilik.php` |
| Service            | PascalCase + suffix Service | `SesiService.php`                 |
| Blade View         | kebab-case                  | `buka-sesi.blade.php`             |
| Migration          | snake_case + nomor urut     | `0012_create_lgs_sesi_table.php`  |
| Route file         | kebab-case                  | `unit-ps.php`                     |

<!-- #endregion -->