# Project Progress Tracker: Dokumen SRS Transaksi Mobil

## Fase 1: Inisiasi Proyek & Workspace Setup

* [x] Membuat folder `.clinerules/` beserta aturan modularnya
* [x] Membuat folder `memory-bank/` untuk persistent context
* [x] Memindahkan data riset mentah ke folder `research/`
* [x] Membuat file template/skeleton SRS di `docs/srs-template.md`

## Fase 2: Penulisan Bab Dokumen SRS

* [x] ~~Bab 1: Pendahuluan (Tujuan, Ruang Lingkup, Glosarium, Konvensi Dokumen)~~
* [x] ~~Bab 2: Deskripsi Keseluruhan (Perspektif Produk, Fungsi Produk, Karakteristik Pengguna, Batasan Sistem)~~
* [x] ~~Bab 3: Kebutuhan Spesifik (Antarmuka Eksternal, Kebutuhan Fungsional, pricing Engine, Dokumen, Logistik, Kredit)~~
* [x] ~~Bab 4: Kebutuhan Non-Fungsional (Performa, Keamanan, Usabilitas, Ketersediaan)~~
* [x] ~~Bab 5: Verifikasi & Traceability Matrix (Metode Pengujian, Matriks Ketertelusuran Kebutuhan)~~

## Fase 3: Refinement & Pelengkapan Gaps (Rekomendasi Audit & Standardisasi)

* [x] Perbaikan Perhitungan Pricing Engine DKI Jakarta (Bebas Opsen & Tarif Pokok BBNKB 12,5%)
* [x] Penambahan Perhitungan PPN Kendaraan Bekas (Tarif Efektif PPN Besaran Tertentu 1,1% dari Harga Transaksi)
* [x] Pembuatan Diagram ERD Fisik Menggunakan Mermaid UML pada Bab 3
* [x] Penjabaran Detail Use Cases Utama (Pre/Post-conditions, Alur Normal & Alternatif untuk UC-03 & UC-09)
* [x] Penulisan Spesifikasi Perangkat Keras Minimum (Uji Fisik Kamera 12MP AF untuk Verifikasi Biometrik & OCR)
* [x] Penulisan Panduan UI/UX & Status Indicator di Bab 3
* [x] Perbaikan Alur Kerja Pendaftaran Jaminan Fidusia Melalui Notaris Rekanan (Ditjen AHU)
* [x] Penambahan Kolom Prioritas (Tinggi, Sedang, Rendah) pada Seluruh Tabel Requirements Bab 3 & 4
* [x] Sinkronisasi Seluruh Perubahan di RTM Bab 5

## Catatan Progress

| Milestone | Tanggal | Status |
|-----------|---------|--------|
| Setup workspace & folder structure | 13 Juni 2026 | ✅ Selesai |
| Penulisan BAB 1 & BAB 2 | 13 Juni 2026 | ✅ Selesai |
| Penulisan BAB 3 (Kebutuhan Spesifik) | 13 Juni 2026 | ✅ Selesai |
| Penulisan BAB 4 (Non-Fungsional) | 13 Juni 2026 | ✅ Selesai |
| Penulisan BAB 5 (Traceability) | 13 Juni 2026 | ✅ Selesai |
| Refinement & Pelengkapan Gaps (Fase 3) | - | ⏳ Pending |
| **PROYEK SRS LENGKAP** | - | ⏳ Pending |

## Ringkasan Kebutuhan SRS

| BAB | Kategori | Jumlah | ID Requirements |
|-----|----------|--------|----------------|
| BAB 3 | Pricing Engine | 17 | REQ-TAX-001 s/d REQ-TAX-017 |
| BAB 3 | Verifikasi Dokumen | 14 | REQ-DOC-001 s/d REQ-DOC-014 |
| BAB 3 | Fintech & Leasing | 10 | REQ-FINT-001 s/d REQ-FINT-010 |
| BAB 3 | Logistik & PDI | 12 | REQ-LOG-001 s/d REQ-LOG-012 |
| BAB 3 | Integrasi API | 5 | REQ-INT-001 s/d REQ-INT-005 |
| **BAB 3 TOTAL** | | **48** | |
| BAB 4 | Performa | 3 | REQ-NF-001 s/d REQ-NF-003 |
| BAB 4 | Keamanan | 4 | REQ-NF-004 s/d REQ-NF-007 |
| BAB 4 | Keandalan | 2 | REQ-NF-008 s/d REQ-NF-009 |
| BAB 4 | Usabilitas | 2 | REQ-NF-010 s/d REQ-NF-011 |
| BAB 4 | Portabilitas | 1 | REQ-NF-012 |
| **BAB 4 TOTAL** | | **12** | |
| **GRAND TOTAL** | | **60** | |

## Checklist Kualitas (IEEE 29148)

- [x] BAB 1: Dokumen memiliki tujuan yang jelas (1.1)
- [x] BAB 1: Ruang lingkup produk terdefinisi lengkap (1.2)
- [x] BAB 1: Glosarium dengan 22 istilah resmi Indonesia (1.3)
- [x] BAB 1: Referensi regulatory tercantum (1.4)
- [x] BAB 1: Konvensi dokumen dengan modal verbs (1.5)
- [x] BAB 2: Context diagram menggunakan Mermaid (2.1)
- [x] BAB 2: Use case diagram dengan 14 UC (2.1)
- [x] BAB 2: Fungsi produk dengan 5 modul utama (2.2)
- [x] BAB 2: User classes dengan 6 tipe pengguna (2.3)
- [x] BAB 2: Batasan sistem (fungsional, teknis, regulatory) (2.4)
- [x] BAB 2: Asumsi dan ketergantungan eksternal (2.5)
- [x] BAB 3: Pricing Engine dengan 16 FR (3.1)
- [x] BAB 3: Verifikasi dokumen dengan 14 FR (3.2)
- [x] BAB 3: Fintech dengan 10 FR (3.3)
- [x] BAB 3: Logistik dengan 12 FR (3.4)
- [x] BAB 3: Integrasi API dengan 5 FR (3.5)
- [x] BAB 4: Performa requirements (REQ-NF-001, 002, 003)
- [x] BAB 4: Keamanan requirements (REQ-NF-004, 005, 006, 007)
- [x] BAB 4: Keandalan requirements (REQ-NF-008, 009)
- [x] BAB 4: Usabilitas requirements (REQ-NF-010, 011)
- [x] BAB 4: Portabilitas requirements (REQ-NF-012)
- [x] BAB 5: Metode verifikasi dan acceptance testing (5.1)
- [x] BAB 5: Traceability matrix (REQ ke Use Case ke Test Case) (5.2)
- [x] BAB 5: CRUD Matrix (5.3)
- [x] BAB 5: Kriteria Penerimaan Akhir & Deployment Checklist (5.4)

## Status Akhir: TAHAP REFINEMENT (FASE 3) ACTIVE ⏳

Dokumen SRS masuk ke dalam tahap penyempurnaan gap analisis untuk standar audit profesional sebelum dinyatakan final development-ready.
