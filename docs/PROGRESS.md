# Progress Development — Lagos Game Center

> Status: 🔴 Belum Mulai | 🟡 Sedang Dikerjakan | 🟢 Selesai | ⏸️ Ditunda (Fase 2)

---

<!-- #region setup -->
## Setup & Dokumentasi

| Task                          | Status | Keterangan            |
| ----------------------------- | ------ | --------------------- |
| Setup Laravel + Laravel Boost | 🟢      | Fresh install selesai |
| Setup PostgreSQL              | 🔴      | —                     |
| Setup VS Code + Extensions    | 🟢      | Selesai               |
| Setup Git & GitHub            | 🟢      | Repo LAGOS (private)  |
| TECHSTACK.md                  | 🟢      | —                     |
| SYSTEMFLOW.md                 | 🟢      | —                     |
| MODULES.md                    | 🟢      | —                     |
| SCHEMA.md                     | 🟢      | —                     |
| FOLDER_STRUCTURE.md           | 🟢      | —                     |
| PROGRESS.md                   | 🟢      | —                     |
| CLAUDE.md                     | 🟢      | —                     |

<!-- #endregion -->

---

<!-- #region migration -->
## Database Migration & Seeder

| Task                                | Status | Keterangan |
| ----------------------------------- | ------ | ---------- |
| Migration: users (bawaan Laravel)   | 🔴      | —          |
| Migration: lgs_roles                | 🔴      | —          |
| Migration: lgs_permissions          | 🔴      | —          |
| Migration: lgs_role_permissions     | 🔴      | —          |
| Migration: lgs_user_roles           | 🔴      | —          |
| Migration: lgs_unit_ps              | 🔴      | —          |
| Migration: lgs_bilik                | 🔴      | —          |
| Migration: lgs_log_smart_plug       | 🔴      | —          |
| Migration: lgs_paket                | 🔴      | —          |
| Migration: lgs_harga_open_billing   | 🔴      | —          |
| Migration: lgs_member               | 🔴      | —          |
| Migration: lgs_sesi                 | 🔴      | —          |
| Migration: lgs_sesi_extend          | 🔴      | —          |
| Migration: lgs_transaksi            | 🔴      | —          |
| Migration: lgs_kategori_pengeluaran | 🔴      | —          |
| Migration: lgs_pengeluaran          | 🔴      | —          |
| Migration: lgs_bukti_pengeluaran    | 🔴      | —          |
| Migration: lgs_konfigurasi          | 🔴      | —          |
| Seeder: RoleSeeder                  | 🔴      | —          |
| Seeder: PermissionSeeder            | 🔴      | —          |
| Seeder: AdminSeeder                 | 🔴      | —          |
| Seeder: BilikSeeder                 | 🔴      | —          |
| Seeder: PaketSeeder                 | 🔴      | —          |

<!-- #endregion -->

---

<!-- #region modul-1 -->
## Modul 1 — Manajemen Unit PS

| Task                          | Status | Keterangan |
| ----------------------------- | ------ | ---------- |
| Model: UnitPs                 | 🔴      | —          |
| Service: UnitPsService        | 🔴      | —          |
| Livewire: UnitPs/DaftarUnitPs | 🔴      | —          |
| Livewire: UnitPs/FormUnitPs   | 🔴      | —          |
| View: unit-ps/index.blade.php | 🔴      | —          |
| Route: unit-ps.php            | 🔴      | —          |

<!-- #endregion -->

---

<!-- #region modul-2 -->
## Modul 2 — Manajemen Bilik

| Task                         | Status | Keterangan |
| ---------------------------- | ------ | ---------- |
| Model: Bilik                 | 🔴      | —          |
| Model: LogSmartPlug          | 🔴      | —          |
| Service: BilikService        | 🔴      | —          |
| Service: IotService          | 🔴      | —          |
| Livewire: Bilik/DaftarBilik  | 🔴      | —          |
| Livewire: Bilik/FormBilik    | 🔴      | —          |
| Livewire: Bilik/MonitorBilik | 🔴      | —          |
| View: bilik/index.blade.php  | 🔴      | —          |
| Route: bilik.php             | 🔴      | —          |

<!-- #endregion -->

---

<!-- #region modul-3 -->
## Modul 3 — Manajemen Harga & Paket

| Task                             | Status | Keterangan |
| -------------------------------- | ------ | ---------- |
| Model: Paket                     | 🔴      | —          |
| Model: HargaOpenBilling          | 🔴      | —          |
| Service: PaketService            | 🔴      | —          |
| Livewire: Paket/DaftarPaket      | 🔴      | —          |
| Livewire: Paket/FormPaket        | 🔴      | —          |
| Livewire: Paket/HargaOpenBilling | 🔴      | —          |
| View: paket/index.blade.php      | 🔴      | —          |
| Route: paket.php                 | 🔴      | —          |

<!-- #endregion -->

---

<!-- #region modul-4 -->
## Modul 4 — Sesi & Billing

| Task                              | Status | Keterangan |
| --------------------------------- | ------ | ---------- |
| Model: Sesi                       | 🔴      | —          |
| Model: SesiExtend                 | 🔴      | —          |
| Service: SesiService              | 🔴      | —          |
| Livewire: Sesi/DashboardSesi      | 🔴      | —          |
| Livewire: Sesi/BukaSesi           | 🔴      | —          |
| Livewire: Sesi/ExtendSesi         | 🔴      | —          |
| Livewire: Sesi/TutupSesi          | 🔴      | —          |
| Livewire: Sesi/OpenBilling        | 🔴      | —          |
| Livewire: Sesi/CloseBilling       | 🔴      | —          |
| Livewire: Sesi/RiwayatSesi        | 🔴      | —          |
| Timer otomatis (JS/Alpine)        | 🔴      | —          |
| Warning 10 menit (visual + suara) | 🔴      | —          |
| Grace period 5 menit              | 🔴      | —          |
| View: sesi/index.blade.php        | 🔴      | —          |
| Route: sesi.php                   | 🔴      | —          |

<!-- #endregion -->

---

<!-- #region modul-5 -->
## Modul 5 — Pembayaran & Transaksi

| Task                                 | Status | Keterangan |
| ------------------------------------ | ------ | ---------- |
| Model: Transaksi                     | 🔴      | —          |
| Service: TransaksiService            | 🔴      | —          |
| Livewire: Transaksi/FormBayar        | 🔴      | —          |
| Livewire: Transaksi/RiwayatTransaksi | 🔴      | —          |
| View: transaksi/index.blade.php      | 🔴      | —          |
| Route: transaksi.php                 | 🔴      | —          |

<!-- #endregion -->

---

<!-- #region modul-6 -->
## Modul 6 — Member & Poin Reward ⏸️ Fase 2

| Task                   | Status | Keterangan                    |
| ---------------------- | ------ | ----------------------------- |
| Model: Member          | ⏸️      | Tabel disiapkan, fitur Fase 2 |
| Service: MemberService | ⏸️      | —                             |
| Livewire: Member/*     | ⏸️      | —                             |
| Sistem Poin & Promo    | ⏸️      | —                             |

<!-- #endregion -->

---

<!-- #region modul-7 -->
## Modul 7 — Laporan Keuangan & Pengeluaran

| Task                                    | Status | Keterangan |
| --------------------------------------- | ------ | ---------- |
| Model: KategoriPengeluaran              | 🔴      | —          |
| Model: Pengeluaran                      | 🔴      | —          |
| Model: BuktiPengeluaran                 | 🔴      | —          |
| Service: LaporanService                 | 🔴      | —          |
| Service: PengeluaranService             | 🔴      | —          |
| Livewire: Laporan/LaporanPendapatan     | 🔴      | —          |
| Livewire: Laporan/LaporanLabaRugi       | 🔴      | —          |
| Livewire: Pengeluaran/DaftarPengeluaran | 🔴      | —          |
| Livewire: Pengeluaran/FormPengeluaran   | 🔴      | —          |
| Export PDF                              | 🔴      | —          |
| Export Excel                            | 🔴      | —          |
| View: laporan/index.blade.php           | 🔴      | —          |
| View: pengeluaran/index.blade.php       | 🔴      | —          |
| Route: laporan.php                      | 🔴      | —          |
| Route: pengeluaran.php                  | 🔴      | —          |

<!-- #endregion -->

---

<!-- #region modul-8 -->
## Modul 8 — Manajemen User & Role (RBAC)

| Task                                     | Status | Keterangan |
| ---------------------------------------- | ------ | ---------- |
| Model: Role                              | 🔴      | —          |
| Model: Permission                        | 🔴      | —          |
| Model: RolePermission                    | 🔴      | —          |
| Model: UserRole                          | 🔴      | —          |
| Trait: HasPermission                     | 🔴      | —          |
| Service: UserRoleService                 | 🔴      | —          |
| Livewire: UserRole/DaftarUser            | 🔴      | —          |
| Livewire: UserRole/FormUser              | 🔴      | —          |
| Livewire: UserRole/DaftarRole            | 🔴      | —          |
| Livewire: UserRole/FormRole              | 🔴      | —          |
| Livewire: UserRole/KonfigurasiPermission | 🔴      | —          |
| View: user-role/index.blade.php          | 🔴      | —          |
| Route: user-role.php                     | 🔴      | —          |

<!-- #endregion -->

---

<!-- #region modul-9 -->
## Modul 9 — Konfigurasi Sistem

| Task                                  | Status | Keterangan |
| ------------------------------------- | ------ | ---------- |
| Model: Konfigurasi                    | 🔴      | —          |
| Service: KonfigurasiService           | 🔴      | —          |
| Livewire: Konfigurasi/FormKonfigurasi | 🔴      | —          |
| Upload logo                           | 🔴      | —          |
| View: konfigurasi/index.blade.php     | 🔴      | —          |
| Route: konfigurasi.php                | 🔴      | —          |

<!-- #endregion -->

---

<!-- #region iot -->
## IoT — Python Service

| Task                          | Status | Keterangan |
| ----------------------------- | ------ | ---------- |
| Setup TinyTuya                | 🔴      | —          |
| iot_service.py                | 🔴      | —          |
| Polling anomali (15 detik)    | 🔴      | —          |
| Integrasi Laravel ↔ Python    | 🔴      | —          |
| Test smart plug BARDI 16A-WEM | 🔴      | —          |

<!-- #endregion -->