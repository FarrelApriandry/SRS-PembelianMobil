# SRS: Sistem Transaksi Pembelian Kendaraan Bermotor Terintegrasi (Indonesia)

> 📄 **Dokumen Lengkap (Google Docs):** [Klik di sini untuk mengakses](https://docs.google.com/document/d/1wV2ZnnF-E9WyW9EKluci5MDWzlN6vDZubKFPuVAUUMY/edit?usp=sharing)

---

## 📖 Tentang Dokumen SRS Ini

Dokumen **Spesifikasi Kebutuhan Perangkat Lunak (SRS)** ini merincikan seluruh kebutuhan sistem informasi untuk fasilitas transaksi pembelian kendaraan roda empat (mobil) di Indonesia, mengintegrasikan regulasi perpajakan, pembiayaan multifinance, dan legalitas dokumen.

### Informasi Dokumen

| Aspek | Detail |
|-------|--------|
| **Standar** | ISO/IEC/IEEE 29148:2018 |
| **Bahasa** | Bahasa Indonesia (Formal) |
| **Status** | ✅ Lengkap (13 Juni 2026) |
| **Total Requirements** | 59 kebutuhan (47 Fungsional + 12 Non-Fungsional) |
| **Struktur** | 5 BAB (Pendahuluan → Verifikasi & Traceability) |

### Regulasi yang Diintegrasikan

| Kategori | Regulasi |
|----------|----------|
| **Perpajakan** | UU HKPD, PMK 131/2024 (PPN 12%), Perda Pajak Progresif DKI 1/2024 |
| **Kredit** | POJK 35/2018, POJK 46/2024, PP 21/2015 (Fidusia) |
| **Dokumen** | Perkapolri 7/2021 (STCK), UU 42/1999 (Fidusia) |
| **Keamanan** | UU ITE, UU PDP (Perlindungan Data) |

---

## 📁 Struktur Dokumen SRS

### Dokumen Utama

```
SRS-PembelianMobil/
├── "Spesifikasi Kebutuhan Perangkat Lunak (SRS) Sistem Transaksi 
│    Pembelian Kendaraan Bermotor Integrasi (Indonesia).md"
│    └── Dokumen SRS lengkap dengan 5 BAB (BAB 1-5)
│
└── docs/
    └── srs-template.md
         └── Template kerangka SRS (sinkron dengan dokumen utama)
```

### Dokumen Pendukung (Sources)

```
SRS-PembelianMobil/
├── .clinerules/                    # Aturan kerja AI Agent
│   ├── 01-srs-workflow.md         # Standar penulisan SRS (IEEE 29148, RFC 2119)
│   ├── 02-indonesia-regulations.md # Aturan bisnis & regulasi hukum Indonesia
│   └── 03-tech-integration.md     # Spesifikasi API & arsitektur ESB Async
│
├── memory-bank/                     # Context knowledge untuk AI
│   ├── project-overview.md         # Gambaran bisnis & alur O2O
│   ├── conventions.md              # Aturan perpajakan & perhitungan matematis
│   ├── architecture.md             # Arsitektur API & integrasi sistem
│   ├── progress.md                # Tracker progress penulisan SRS
│   └── active-context.md           # Status aktif & langkah selanjutnya
│
├── research/                        # Data riset mentah
│   ├── RisetPembelianMobilIndonesiauntukSRS.md
│   │     └── Analisis komprehensif: pajak, dokumen, PDI, arsitektur O2O
│   └── InformasiPenting.md
│         └── Ringkasan informasi penting untuk SRS
│
└── AGENTS.md                       # Instruksi role AI Agent
```

---

## 🏗️ Arsitektur Sistem (Berdasarkan SRS)

### Model Bisnis: Online-to-Offline (O2O)

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   Estimasi  │────▶│  Inspeksi   │────▶│ Penawaran   │
│   Online    │     │   Fisik     │     │   Final     │
└─────────────┘     └─────────────┘     └─────────────┘
                                               │
                    ┌─────────────┐            ▼
                    │  Aktivasi   │◀────┌─────────────┐
                    │   Garansi   │     │  Pencairan  │
                    └─────────────┘     │   Instan    │
                                         └─────────────┘
```

### 5 Modul Utama Sistem

| Modul | Fungsi | Requirements |
|-------|--------|--------------|
| **Pricing Engine** | Kalkulasi PPN, PKB, BBNKB, Opsen, Pajak Progresif | 16 FR |
| **Verifikasi Dokumen** | Validasi KTP, BPKB, STNK, kwitansi, SPH | 14 FR |
| **Fintech & Leasing** | Integrasi SLIK, ESB Async, Fidusia AHU | 10 FR |
| **Logistik & PDI** | Tracking, Pre-Delivery Inspection, STCK | 12 FR |
| **Integrasi API** | Dukcapil, Samsat, TTE (Privy/VIDA) | 5 FR |

### Integrasi API Eksternal

| API | Tujuan | Output |
|-----|--------|--------|
| **API Dukcapil** | Verifikasi NIK & face matching biometrik | Face match score ≥ 85% |
| **API SLIK OJK / PEFINDO** | Credit checking & kelayakan kredit | Dokumen iDeb, skor kolektibilitas |
| **API Samsat / SIGNAL** | Validasi STNK & cek pajak progresif | Status STNK, ETLE, nominal PKB |
| **API Ditjen AHU (Fidusia)** | Pendaftaran fidusia elektronik | Sertifikat fidusia, kode PNBP |
| **API Digital Signature** | TTE pada SPK & kontrak kredit | Sertifikat TTE tersertifikasi Kominfo |

---

## 📊 Breakdown Requirements SRS

### BAB 3: Kebutuhan Fungsional (47 Total)

| Kategori | ID | Jumlah |
|----------|-----|--------|
| Pricing Engine (Kalkulasi Pajak) | REQ-TAX-001 s/d REQ-TAX-016 | 16 |
| Verifikasi Dokumen | REQ-DOC-001 s/d REQ-DOC-014 | 14 |
| Fintech & Leasing | REQ-FINT-001 s/d REQ-FINT-010 | 10 |
| Logistik & PDI | REQ-LOG-001 s/d REQ-LOG-012 | 12 |
| Integrasi API | REQ-INT-001 s/d REQ-INT-005 | 5 |

### BAB 4: Kebutuhan Non-Fungsional (12 Total)

| Kategori | ID | Jumlah |
|----------|-----|--------|
| Performa | REQ-NF-001 s/d REQ-NF-003 | 3 |
| Keamanan | REQ-NF-004 s/d REQ-NF-007 | 4 |
| Keandalan | REQ-NF-008 s/d REQ-NF-009 | 2 |
| Usabilitas | REQ-NF-010 s/d REQ-NF-011 | 2 |
| Portabilitas | REQ-NF-012 | 1 |

---

## 📐 Formula Perpajakan Utama (Berdasarkan SRS)

### Pajak Pusat
```
DPP PPN      = (11/12) × NJKB
PPN Terutang = 12% × DPP = 11% × NJKB
```

### Pajak Daerah (Opsen UU HKPD)
```
Opsen PKB    = 66% × PKB Pokok
Opsen BBNKB  = 66% × BBNKB Pokok
```

### Pajak Progresif (DKI Jakarta Perda 1/2024)

| Kepemilikan | Tarif PKB |
|-------------|-----------|
| Kendaraan ke-1 | 2,0% |
| Kendaraan ke-2 | 3,0% |
| Kendaraan ke-3 | 4,0% |
| Kendaraan ke-4 | 5,0% |
| Kendaraan ke-5+ | 6,0% (flat maksimal) |

### Subsidi Kendaraan Listrik (KBLBB)

| TKDN | Insentif | PPN Efektif |
|------|----------|--------------|
| ≥ 40% (BEV) | PPN DTP 10% | 2% |
| Hybrid | Tidak ada | 11% |

### DP Minimum Berbasis NPF Leasing (OJK)

| Rasio NPF Net | DP Minimum |
|---------------|------------|
| ≤ 1% | 0% |
| 1% - 3% | 10% |
| 3% - 5% | 15% |
| > 5% | 20% |

---

## 📜 Referensi Regulatory dalam SRS

1. **UU HKPD** — Hubungan Keuangan Pusat-Daerah
2. **PMK 131/2024** — Tarif PPN 12%
3. **Perda DKI 1/2024** — Pajak Progresif Kendaraan
4. **PP 21/2015** — Jaminan Fidusia (batas 30 hari)
5. **Perkapolri 7/2021** — STCK (masa berlaku 1 bulan)
6. **POJK 35/2018 & 46/2024** — DP Minimum Pembiayaan
7. **UU ITE & UU PDP** — Keamanan & Perlindungan Data

---

## 🔧 Spesifikasi Teknis (Berdasarkan SRS)

| Aspek | Spesifikasi |
|-------|-------------|
| **Performa API Internal** | p50 ≤ 2 detik, p99 ≤ 3 detik |
| **ESB Async Leasing** | ≤ 4,22 detik avg (vs sync: 31,49 detik) |
| **Kapasitas** | 10.000 concurrent users, 500 TPS |
| **Enkripsi** | AES-256 (at rest), TLS 1.3 (in transit) |
| **Face Matching** | Skor ≥ 85% via API Dukcapil |
| **Uptime** | 99,9% (downtime max 43 menit/bulan) |
| **Mobile Support** | Android 10+, iOS 14+ |
| **Web Browser** | Chrome 90+, Safari 14+, Firefox 88+ |

---

## 📝 Catatan Penggunaan

### Untuk AI Agent (Cline)

Dokumen ini dirancang agar AI Agent dapat:
1. **Membaca** `memory-bank/` untuk context pengetahuan proyek
2. **Mengikuti** `.clinerules/` untuk standar penulisan
3. **Merujuk** `research/` untuk data riset mentah
4. **Menulis** ke `docs/srs-template.md` dengan format IEEE 29148

### Standar Penulisan Requirements

- **shall** — Kebutuhan wajib (mandatory)
- **should** — Rekomendasi kuat
- **may** — Fitur opsional

Format ID: `REQ-[KATEGORI]-[NOMOR]` (contoh: `REQ-TAX-001`, `REQ-DOC-014`)

---

*Terakhir diperbarui: 13 Juni 2026*
