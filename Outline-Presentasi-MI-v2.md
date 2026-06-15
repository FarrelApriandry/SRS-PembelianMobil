# RANGKUMAN PRESENTASI SRS - SISTEM TRANSAKSI PEMBELIAN KENDARAAN BERMOTOR TERINTEGRASI

> **Tujuan:** Dokumen rangkuman presentasi untuk tim dengan 5 urutan materi berdasarkan slide deck, lengkap dengan referensi flowchart dan penjelasan detail.

---

# URUTAN 1: INTRODUKSI & KONTEKS SISTEM

## 1.1 About Our Team

**Anggota Tim:**
| Nama | NIM |
|------|-----|
| Fadlika Alfi Nurrochmah | 2410506010 |
| Muhammad Ramdhan | 2420506027 |
| Assep Wahid | 2420506028 |
| Ilyasa Abiyyu Wicaksono | 2420506035 |
| Farrel Apriandry Ciu | 2430506056 |

---

## 1.2 Latar Belakang Masalah

**Permasalahan yang Dihadapi:**
```
❌ Fragmentasi Proses
   → Kalkulasi pajak, verifikasi dokumen, kredit, dan PDI dilakukan manual & terpisah-pisah

❌ Risiko Kesalahan Administrasi Tinggi
   → Khususnya untuk hal yang terikat batas waktu hukum (contoh: fidusia maksimal 30 hari)

❌ Belum Ada Integrasi Real-Time
   → Diler belum terintegrasi dengan instansi pemerintah (Dukcapil, Samsat, SLIK OJK, Ditjen AHU)
```

**Tujuan Sistem:**
```
✅ Menyediakan kontrak formal antara developer dan stakeholder
✅ Menjadi acuan QA dan pedoman teknis arsitek/programmer
✅ Mencakup alur dari estimasi harga online hingga serah terima unit fisik
```

---

## 1.3 Batasan Sistem

| Batasan | Detail |
|---------|--------|
| **Geografis** | Hanya untuk wilayah hukum Republik Indonesia |
| **Mata Uang** | Seluruh kalkulasi dalam Rupiah (IDR), presisi 2 desimal |
| **SLA API** | Kecepatan verifikasi bergantung pada server instansi pemerintah (SIGNAL, Dukcapil, Ditjen AHU) |

**Asumsi & Ketergantungan:**
```
✓ API Dukcapil dapat diakses real-time untuk face matching
✓ SLIK/PEFINDO memberikan status kolektibilitas akurat
✓ Pendaftaran fidusia didukung notaris berlisensi aktif di SABH Ditjen AHU
```

---

## 1.4 Ruang Lingkup - 3 Skenario Transaksi

### A. Kendaraan Baru
```
📋 Fitur:
   ├── Kalkulasi harga OTR real-time
   ├── Validasi dokumen diler (Faktur, SRUT, Form A untuk unit CBU/impor)
   └── Integrasi TTE untuk tanda tangan SPK
```

### B. Kendaraan Bekas
```
📋 Fitur:
   ├── Inspeksi fisik digital 150–188 titik pemeriksaan
   ├── Verifikasi keaslian dokumen (STNK aktif, BPKB hologram & UV, kuitansi bermaterai, SPH)
   └── Pencairan dana instan ke penjual maksimal 60 menit
```

### C. Modul Financing
```
📋 Fitur:
   ├── Simulasi kredit dinamis (DP berbasis rasio NPF leasing)
   ├── Integrasi otomatis ke SLIK OJK/PEFINDO
   └── Pendaftaran jaminan fidusia elektronik ke Ditjen AHU
```

---

## 1.5 Perspektif Produk & Context Diagram

```
┌─────────────────────────────────────────────────────────────────────────┐
│                         PLATFORM OMNICHANNEL O2O                        │
│                                                                         │
│   ┌──────────────┐         ┌─────────────────┐         ┌─────────────┐  │
│   │   DEALER     │◄───────►│  SISTEM INTI    │◄───────►│   LEASING   │  │
│   │   MANAGEMENT │         │   (Integrated   │         │  (Multifin) │  │
│   │   SYSTEM     │         │    Platform)     │         │             │  │
│   └──────────────┘         └────────┬────────┘         └──────┬──────┘  │
│                                    │                          │        │
│                          ┌─────────┴──────────┐                │        │
│                          │    INSTANSI       │                │        │
│                          │    PEMERINTAH     │                │        │
│                          │                   │                │        │
│                          │  • Dukcapil       │                │        │
│                          │  • Samsat/SIGNAL  │                │        │
│                          │  • Ditjen AHU     │                │        │
│                          └───────────────────┘                │        │
│                                                                  │        │
│                          REAL-TIME INTEGRATION ──────────────────┘        │
└─────────────────────────────────────────────────────────────────────────┘
```

**Penjelasan:**
- Sistem adalah platform omnichannel terintegrasi (O2O)
- Menjadi penghubung antara dealer management system internal diler dengan jaringan pembiayaan pihak ketiga dan instansi pemerintah, secara real-time

---

## 1.6 Karakteristik Pengguna (User Classes)

| Peran | Deskripsi | Hak Akses Utama |
|-------|-----------|-----------------|
| **Pembeli Personal** | Konsumen perorangan pembeli/penjual kendaraan | Isi profil, simulasi, unggah KTP/KK, TTE |
| **Sales Dealer** | Staf pemasaran diler | Buat draf SPK, konfigurasi unit, pantau status kredit |
| **Admin Dealer** | Staf backoffice administrasi diler | Verifikasi dokumen, daftar ke Samsat, alokasi PNBP |
| **Inspektor Lapangan** | Teknisi penilai kondisi fisik mobil bekas | Checklist inspeksi mobile, foto/video 360° |
| **Surveyor Leasing** | Analis risiko perusahaan pembiayaan | Review aplikasi kredit, akses skor iDeb, approval limit |
| **Teknisi PDI** | Teknisi penyiap unit baru | Checklist PDI, daftar nomor rangka ke modul garansi |

---

## 1.7 Data Dictionary - Entitas Utama

### A. Entitas: Pembeli
| Atribut | Tipe | Atribut | Tipe |
|---------|------|---------|------|
| id | UUID | role | ENUM |
| nik | VARCHAR(16) | password_hash | VARCHAR(255) |
| nama_lengkap | VARCHAR(255) | foto_ktp_path | VARCHAR(512) |
| tanggal_lahir | DATE | foto_selfie_path | VARCHAR(512) |
| alamat | TEXT | face_match_score | DECIMAL(5,2) |
| nomor_hp | VARCHAR(15) | is_verified | BOOLEAN |
| email | VARCHAR(255) | created_at | TIMESTAMP |

### B. Entitas: Kendaraan
| Atribut | Tipe | Atribut | Tipe |
|---------|------|---------|------|
| id | UUID | model | VARCHAR(100) |
| nomor_rangka | VARCHAR(17) | tahun | INTEGER |
| nomor_mesin | VARCHAR(20) | warna | VARCHAR(50) |
| nomor_polisi | VARCHAR(12) | harga_dasar | DECIMAL(15,2) |
| jenis | ENUM | njkb | DECIMAL(15,2) |
| tipe | ENUM | tarif_pkb | DECIMAL(5,4) |
| merk | VARCHAR(100) | koefisian_road | DECIMAL(3,2) |
| pembeli_id | UUID | status | ENUM |

### C. Entitas: Dokumen Legalitas
| Atribut | Tipe | Atribut | Tipe |
|---------|------|---------|------|
| id | UUID | file_size | INTEGER |
| pembeli_id | UUID | is_verified | BOOLEAN |
| kendaraan_id | UUID | verification_notes | TEXT |
| jenis | ENUM | verified_by | UUID |
| file_path | VARCHAR(512) | uploaded_at | TIMESTAMP |
| file_hash | VARCHAR(64) | verified_at | TIMESTAMP |
| mime_type | VARCHAR(100) | expires_at | TIMESTAMP |

### D. Entitas: AplikasiKredit
| Atribut | Tipe | Atribut | Tipe |
|---------|------|---------|------|
| id | UUID | bunga_tahunan | DECIMAL(5,4) |
| submission_id | VARCHAR(50) | angsuran_bulanan | DECIMAL(15,2) |
| pembeli_id | UUID | total_bunga | DECIMAL(15,2) |
| kendaraan_id | UUID | total_pembayaran | DECIMAL(15,2) |
| jumlah_pinjaman | DECIMAL(15,2) | leasing_id | UUID |
| dp_amount | DECIMAL(15,2) | notaris_id | UUID |
| dp_persen | DECIMAL(5,2) | status | ENUM |
| tenor_bulan | INTEGER | ideb_data | JSONB |

### E. Entitas: SertifikatFidusia
| Atribut | Tipe | Atribut | Tipe |
|---------|------|---------|------|
| id | UUID | nilai_penjaminan | DECIMAL(15,2) |
| aplikasi_kredit_id | UUID | is_online_registered | BOOLEAN |
| notaris_id | UUID | tanggal_pendaftaran_ahu | DATE |
| nomor_sertifikat | VARCHAR(50) | kode_billing | VARCHAR(50) |
| nomor_akta | VARCHAR(50) | jumlah_pnbp | DECIMAL(15,2) |
| tanggal_akta | DATE | status_bayar | ENUM |
| batas_pendaftaran | DATE | status | ENUM |

### F. Entitas: PDI_Report
| Atribut | Tipe | Atribut | Tipe |
|---------|------|---------|------|
| id | UUID | gps_start_lat | DECIMAL(10,8) |
| kendaraan_id | UUID | gps_start_lng | DECIMAL(11,8) |
| teknisi_id | UUID | gps_end_lat | DECIMAL(10,8) |
| supervisor_id | UUID | gps_end_lng | DECIMAL(11,8) |
| dealer_id | UUID | gps_distance_km | DECIMAL(8,3) |
| dewaxing_passed | BOOLEAN | uji_jalan_passed | BOOLEAN |
| dewaxing_notes | TEXT | uji_jalan_notes | TEXT |
| fuse_installed | BOOLEAN | overall_passed | BOOLEAN |
| fuse_type | VARCHAR(50) | rejection_reason | TEXT |
| obd_scan_passed | BOOLEAN | document_path | VARCHAR(512) |
| dtc_count | INTEGER | teknisi_signature_path | VARCHAR(512) |
| dtc_codes | JSONB | supervisor_signature_path | VARCHAR(512) |
| fluid_check_passed | BOOLEAN | approved_at | TIMESTAMP |

---

## 1.8 Integrasi API Eksternal

| API | Tujuan Integrasi | Input | Output |
|-----|-----------------|-------|--------|
| **Dukcapil** | Verifikasi KTP & face matching biometrik | NIK, Foto e-KTP, Swafoto | Face matching score |
| **SLIK OJK / PEFINDO** | Kelayakan kredit instan | NIK, Nama, Tanggal Lahir | Skor kolektibilitas (iDeb) 1-5 |
| **Samsat / SIGNAL** | Verifikasi kendaraan & pajak | NRKB, NIK, 5 digit nomor rangka | Status ETLE, masa STNK, nilai PKB |
| **Ditjen AHU** | Pendaftaran fidusia elektronik | Data Akta Notaris, Nomor Rangka, Nilai | Kode Billing PNBP, Sertifikat Fidusia |
| **TTE (Privy/VIDA)** | Tanda tangan elektronik | Dokumen PDF, Nomor HP (OTP) | Sertifikat TTE bersertifikat Kominfo |

---

# URUTAN 2: KEBUTUHAN & VALIDASI SISTEM

## 2.1 Kebutuhan Fungsional - 5 Modul Utama

### Modul 1: Pricing Engine
```
📊 Kalkulasi harga & pajak transparan berdasarkan:
   ├── NJKB (Nilai Jual Kendaraan Bermotor)
   ├── Status kepemilikan progresif
   └── Tipe mesin

📌 Output: Rincian OTR lengkap (PPN, PKB, BBNKB, Opsen, PNBP)
```

### Modul 2: Appraisal & Inspeksi Fisik
```
🔍 Pemandu inspeksi mobile:
   ├── 150 titik pemeriksaan (mobil biasa)
   └── 188 titik pemeriksaan (SUV/premium)

📌 Output: Laporan inspeksi digital dengan foto/video
```

### Modul 3: Document Verification Manager
```
📄 OCR & validasi keaslian dokumen secara otomatis:
   ├── Hologram Tri Brata (akurasi ≥ 95%)
   ├── Benang UV Sicherheit
   ├── Luhn Checksum NPWP
   └── Validasi kwitansi bermaterai
```

### Modul 4: Fintech Leasing Hub
```
💰 Pengajuan kredit asynchronous:
   ├── Simulasi DP dinamis (berdasarkan NPF)
   ├── Integrasi SLIK OJK / PEFINDO
   ├── Credit scoring otomatis
   └── Pendaftaran fidusia digital
```

### Modul 5: PDI & Logistics Tracker
```
🚗 Pelacakan unit & checklist PDI:
   ├── De-waxing, backup fuse
   ├── OBD scan & DTC deletion
   ├── Uji jalan GPS (minimal 10 km)
   ├── STCK management
   └── Aktivasi garansi
```

---

## 2.2 Kebutuhan Non-Fungsional

### A. Performa
| Metrik | Target |
|--------|--------|
| Response Time p50 | ≤ 2 detik |
| Response Time p99 | ≤ 3,5 detik |
| Response Time p99.9 | ≤ 5 detik |
| Pengajuan Kredit Async | Rata-rata 4,22 detik |
| Kapasitas | 10.000 pengguna aktif bersamaan |
| Throughput | 500 TPS |

### B. Keamanan
```
🔐 Enkripsi:
   ├── Data at Rest: AES-256
   └── Data in Transit: TLS 1.3

🔐 Autentikasi:
   ├── RBAC (Role-Based Access Control)
   └── MFA (OTP 6 digit via WhatsApp/Email, berlaku 5 menit)

   Role yang WAJIB MFA:
   • Admin Dealer
   • Surveyor Leasing
   • System Admin

🔐 Verifikasi Identitas:
   ├── TTE wajib bersertifikat Kominfo (Privy/VIDA)
   └── Liveness detection + face matching Dukcapil minimal skor 85%
```

### C. Keandalan
```
📊 Uptime: minimal 99,9% (maks. downtime 43 menit/bulan)

🔄 Graceful Degradation:
   Jika API pemerintah down:
   • Sistem tetap berjalan
   • Notifikasi ke user
   • Retry otomatis
   • Log aktivitas untuk audit
```

### D. Usabilitas & Portabilitas
```
📱 Aplikasi Mobile:
   ├── Android 10+
   └── iOS 14+

💻 Web Portal: WCAG 2.1 AA compliant

🌐 Browser Compatibility:
   ├── Chrome 90+
   ├── Safari 14+
   └── Firefox 88+
```

---

## 2.3 Verifikasi & Acceptance Criteria

### 4 Metode Verifikasi (ISO/IEC/IEEE 29148:2018)

| Metode | Penjelasan | Contoh |
|--------|------------|--------|
| **Test** | Pengujian dinamis | Load test, integrasi API, penetration test |
| **Demonstration** | Menunjukkan fungsi visual | Alur PDI oleh teknisi |
| **Inspection** | Pemeriksaan statis | Review kode, skema database |
| **Analysis** | Pembuktian matematis | Logika pricing engine |

### 4 Kriteria Penerimaan Akhir

```
✅ KRITERIA 1: Functional Requirement
   → 100% functional requirement lulus uji tanpa kesalahan logika bisnis

✅ KRITERIA 2: API Integration
   → Seluruh 5 API eksternal terkoneksi aman dengan error handling graceful

✅ KRITERIA 3: Stress Test
   → Throughput ≥ 500 TPS
   → Latency p99 ≤ 3,5 detik

✅ KRITERIA 4: Security Audit
   → Zero critical vulnerability
   → Enkripsi AES-256 berjalan baik
```

---

## 2.4 Matriks Pengujian RTM (Requirements Traceability Matrix)

### Bagian 1: Test Requirements

| Requirement ID | Nama Persyaratan | Use Case ID | Metode Verifikasi | Kriteria Kelulusan |
|----------------|------------------|-------------|-------------------|-------------------|
| REQ-TAX-001 | Hitung DPP PPN Nilai Lain | UC-01 | Analysis | Perhitungan DPP PPN tepat senilai 11/12 × NJKB tanpa selisih Rp1 |
| REQ-TAX-011 | Cek Pajak Progresif | UC-01 | Test | Query NIK ke API Samsat mengembalikan data unit aktif dengan respons < 5 detik |
| REQ-DOC-008 | Verifikasi Hologram BPKB | UC-06 | Test | Algoritma klasifikasi gambar mendeteksi hologram Tri Brata dengan akurasi ≥ 95% |
| REQ-FINT-008 | Batas Waktu Jaminan Fidusia | UC-09 | Analysis | Sistem menolak pengiriman data ke Ditjen AHU jika tanggal akta notaris > 30 hari |

### Bagian 2: Additional Test Requirements

| Requirement ID | Nama Persyaratan | Use Case ID | Metode Verifikasi | Kriteria Kelulusan |
|----------------|------------------|-------------|-------------------|-------------------|
| REQ-LOG-008 | Jarak Uji Jalan PDI | UC-10 | Test | Sensor GPS diler mencatat jarak tempuh uji jalan minimal tepat 10 kilometer |
| REQ-INT-001 | Face Matching Dukcapil | UC-04 | Test | Verifikasi identitas registrasi mengembalikan kecocokan biometrik wajah ≥ 85% |
| REQ-NF-002 | Latensi Async ESB Leasing | UC-03 | Test | Waktu respons pengiriman melalui broker antrean pesan rata-rata 4,22 detik |

---

## 2.5 Matriks Pengujian CRUD

| Entitas Data | Create (C) | Read (R) | Update (U) | Delete (D) |
|--------------|------------|----------|------------|------------|
| **Data Unit Mobil** | REQ-LOG-001 (Logistik) | REQ-TAX-016 (Breakdown OTR) | REQ-LOG-012 (Aktivasi Garansi) | Tidak Diizinkan (Hanya Soft-Delete) |
| **Berkas Jual-Beli** | REQ-DOC-014 (Upload Berkas) | REQ-DOC-003 (Pencocokan Admin) | REQ-INT-005 (TTE Dokumen SPK) | Tidak Diizinkan |
| **Aplikasi Kredit** | REQ-FINT-004 (Submission) | REQ-FINT-005 (Dasbor Surveyor) | REQ-FINT-005 (Webhook Callback) | Tidak Diizinkan |
| **Sertifikat Fidusia** | REQ-FINT-007 (Notaris SABH) | REQ-FINT-009 (Dasbor Admin) | REQ-FINT-008 (Monitoring Batas) | Tidak Diizinkan |

> ⚠️ **Catatan:** Seluruh entitas menggunakan **SOFT DELETE** - tidak ada penghapusan permanen untuk menjaga audit trail.

---

## 2.6 Aturan Validasi Sistem

### Tujuan
```
Menjamin proses verifikasi dokumen dan inspeksi kendaraan memenuhi standar 
yang ditetapkan sebelum transaksi dapat dilanjutkan.
```

### Aturan Validasi Utama

| Kode | Aturan Validasi | Kriteria |
|------|-----------------|----------|
| REQ-INT-001 | Face Matching Dukcapil | Tingkat kecocokan biometrik wajah minimal **85%** |
| REQ-DOC-008 | Verifikasi Hologram BPKB | Akurasi deteksi hologram Tri Brata minimal **95%** |
| REQ-LOG-008 | Validasi Uji Jalan PDI | Jarak uji jalan minimal **10 km** sebelum status "Lolos PDI" diterbitkan |

### Error Handling

| Kondisi | Tindakan Sistem |
|---------|-----------------|
| Face matching < 85% | Registrasi DITOLAK, pengguna diminta verifikasi ulang |
| Akurasi hologram < 95% | Dokumen BPKB ditandai **GAGAL VERIFIKASI** |
| Jarak uji jalan < 10 km | Status **"Lolos PDI"** TIDAK BISA diproses |

---

# URUTAN 3: FLOWCHART LOGIKA BISNIS (Bagian 1)

> 📌 **Referensi:** BusinessLogic-MermaidJS.md - Sections 1, 2, 3

---

## 3.1 Modul Kalkulasi Pajak - Pricing Engine

> **Referensi:** BusinessLogic-MermaidJS.md → Section 1: Kalkulasi Pajak

### 3.1.1 Flowchart Kalkulasi Pajak Lengkap

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                        FLOWCHART KALKULASI PAJAK                            │
└─────────────────────────────────────────────────────────────────────────────┘

    ┌───────────────┐
    │ 🚀 MULAI       │
    │ KALKULASI PAJAK│
    └───────┬───────┘
            │
            ▼
    ┌───────────────────┐
    │ Input:            │
    │ • Tipe Kendaraan   │
    │ • NJKB             │
    │ • Wilayah          │
    │ • Golongan         │
    └───────┬───────────┘
            │
            ▼
    ┌───────────────────┐
    │ Apakah Kendaraan  │───────────YA──────────► ┌───────────────┐
    │ Bermotor Baru?    │                        │ Verifikasi TKDN│
    └───────┬───────────┘                        └───────┬───────┘
            │                                           │
        TIDAK                                         ▼
            │                              ┌───────────────────┐
            ▼                              │   TKDN ≥ 40%?    │
    ┌───────────────┐                     └───────┬───────────┘
    │ Ambil NJKB     │                      YA     │       TIDAK
    │ dari Lembar    │                     ▼      │         ▼
    │ STNK           │            ┌───────────────┐  ┌───────────────┐
    └───────┬───────┘            │ ✓ KBLBB        │  │ ✗ Tidak        │
            │                    │ Eligible       │  │   Eligible     │
            │                    │ PPN DTP 10%   │  │ PPN Normal     │
            │                    └───────┬───────┘  └───────────────┘
            │                            │                  │
            │     ┌──────────────────────┴──────────────────┘
            │     │
            ▼     ▼
    ┌───────────────────┐
    │ Hitung DPP PPN    │
    │ DPP = 11/12 × NJKB│
    └───────┬───────────┘
            │
            ▼
    ┌───────────────────┐
    │ PPN = 12% × DPP   │
    │ Atau: PPN = 11%   │
    │       × NJKB      │
    └───────┬───────────┘
            │
            ▼
    ┌───────────────────┐
    │ Apakah Wilayah    │───────YA──────► ┌───────────────────────┐
    │ DKI Jakarta?      │                  │ Opsen PKB = Rp0       │
    └───────┬───────────┘                  │ Opsen BBNKB = Rp0     │
            │                               │ BBNKB Pokok = 12.5%  │
        TIDAK                               └───────────┬───────────┘
            │                                           │
            ▼                                           │
    ┌───────────────────┐                                │
    │ BBNKB Pokok =     │◄───────────────────────────────┘
    │ 12% × NJKB        │
    │ PKB Pokok =       │         ┌──────────────────────┴──────────────────────┐
    │ Tarif × NJKB      │         │                                             │
    └───────┬───────────┘         │                                             │
            │                     ▼                                             │
            └──────────► ┌───────────────────┐                                  │
                         │ Hitung Opsen      │                                  │
                         │ Opsen PKB = 66%  │                                  │
                         │ × PKB Pokok      │                                  │
                         │ Opsen BBNKB =    │                                  │
                         │ 66% × BBNKB Pokok│                                  │
                         └───────┬───────────┘                                  │
                                 │                                              │
                                 ▼                                              │
                         ┌───────────────────┐                                │
                         │ Cek Kepemilikan    │─────YA─────► Hitung Progresif   │
                         │ Progresif?        │                 berdasarkan      │
                         └───────┬───────────┘                 Urutan Kendaraan   │
                                 │                                              │
                             TIDAK                                              │
                                 │                                              │
                                 ▼                                              │
                         ┌───────────┐                                         │
                         │ Lewati   │◄─────────────────────────────────────────┘
                         │ Progresif│
                         └───────────┘
                                 │
                                 ▼
                         ┌───────────────────┐
                         │ Hitung PNBP       │
                         │                   │
                         │ SWDKLLJ: Rp143-150│
                         │ STNK: Rp200.000   │
                         │ TNKB: Rp100.000   │
                         │ BPKB: Rp375.000   │
                         └───────┬───────────┘
                                 │
                                 ▼
                         ┌───────────────────┐
                         │ OUTPUT:           │
                         │ Rincian Tagihan:  │
                         │ • PPN             │
                         │ • PKB + Opsen     │
                         │ • BBNKB + Opsen   │
                         │ • PNBP Total      │
                         └───────┬───────────┘
                                 │
                                 ▼
                         ┌───────────────┐
                         │ ✅ SELESAI    │
                         └───────────────┘
```

### 3.1.2 Formula Matematika Kalkulasi Pajak

```
┌─────────────────────────────────────────────────────────────────┐
│                    FORMULA MATEMATIKA                           │
└─────────────────────────────────────────────────────────────────┘

╔═══════════════════════════════════════════════════════════════════╗
║  1. DPP (Dasar Pengenaan Pajak)                                  ║
║  ─────────────────────────────────────                            ║
║                                                                     ║
║       DPP PPN = (11/12) × NJKB                                     ║
║                                                                     ║
║  Contoh: NJKB = Rp150.000.000                                      ║
║          DPP PPN = (11/12) × 150.000.000                          ║
║                   = Rp137.500.000                                  ║
╠═══════════════════════════════════════════════════════════════════╣
║  2. PPN Terutang (PMK 131/2024)                                   ║
║  ─────────────────────────────────────                            ║
║                                                                     ║
║       PPN = 12% × DPP PPN                                          ║
║            = 12% × (11/12 × NJKB)                                 ║
║            = 11% × NJKB                                            ║
║                                                                     ║
║  Contoh: PPN = 11% × 150.000.000 = Rp16.500.000                    ║
╠═══════════════════════════════════════════════════════════════════╣
║  3. PKB & Opsen                                                    ║
║  ───────────────────────                                          ║
║                                                                     ║
║       PKB Pokok = Tarif × NJKB                                     ║
║       Opsen PKB = 66% × PKB Pokok                                 ║
║                                                                     ║
║       BBNKB Pokok = 12% × NJKB (provinsi lain)                     ║
║                    = 12.5% × NJKB (DKI Jakarta)                     ║
║       Opsen BBNKB = 66% × BBNKB Pokok                              ║
╠═══════════════════════════════════════════════════════════════════╣
║  4. Pajak Progresif (DKI Jakarta)                                 ║
║  ─────────────────────────────────                                ║
║                                                                     ║
║       Kendaraan ke-1: 2%                                           ║
║       Kendaraan ke-2: 3%                                           ║
║       Kendaraan ke-3: 4%                                           ║
║       Kendaraan ke-4: 5%                                           ║
║       Kendaraan ke-5+: 6% (maksimal)                               ║
╠═══════════════════════════════════════════════════════════════════╣
║  5. NJKB dari STNK                                                 ║
║  ─────────────────────                                              ║
║                                                                     ║
║       NJKB = PKB Pokok / Tarif Berlaku                             ║
╠═══════════════════════════════════════════════════════════════════╣
║  6. khusus jakarta                                                 ║
║  ─────────────────────────────────                                ║
║                                                                     ║
║       Opsen PKB = Rp0                                              ║
║       Opsen BBNKB = Rp0                                            ║
╚═══════════════════════════════════════════════════════════════════╝
```

### 3.1.3 Contoh Perhitungan Lengkap

```
┌─────────────────────────────────────────────────────────────────┐
│           CONTOH PERHITUNGAN PAJAK LENGKAP                      │
└─────────────────────────────────────────────────────────────────┘

📋 DATA:
   • NJKB = Rp150.000.000
   • Wilayah = Jawa Barat (bukan DKI)
   • Kendaraan pertama (bukan progresif)
   • TKDN < 40% (tidak eligible KBLBB)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📊 PERHITUNGAN:

1. PKB Pokok = 2% × Rp150.000.000 = Rp3.000.000
2. Opsen PKB = 66% × Rp3.000.000 = Rp1.980.000
   Total PKB = Rp3.000.000 + Rp1.980.000 = Rp4.980.000

3. BBNKB Pokok = 12% × Rp150.000.000 = Rp18.000.000
   Opsen BBNKB = 66% × Rp18.000.000 = Rp11.880.000
   Total BBNKB = Rp18.000.000 + Rp11.880.000 = Rp29.880.000

4. DPP PPN = (11/12) × Rp150.000.000 = Rp137.500.000
   PPN = 12% × Rp137.500.000 = Rp16.500.000

5. SWDKLLJ = Rp143.000
6. STNK = Rp200.000
7. TNKB = Rp100.000
8. BPKB = Rp375.000

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

💰 TOTAL TAGIHAN:

   • PKB + Opsen        : Rp4.980.000
   • BBNKB + Opsen      : Rp29.880.000
   • PPN                : Rp16.500.000
   • PNBP (STNK+TNPB+BPKB+SWDKLLJ): Rp818.000
   ─────────────────────────────────────────
   TOTAL                : Rp52.178.000
```

---

## 3.2 Modul Kalkulasi DP Dinamis (NPF-Based)

> **Referensi:** BusinessLogic-MermaidJS.md → Section 2: Logika Simulasi Kredit

### 3.2.1 Flowchart Penentuan DP Dinamis

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                    FLOWCHART SIMULASI KREDIT                                │
└─────────────────────────────────────────────────────────────────────────────┘

    ┌───────────────────────┐
    │ 🚀 INISIASI SIMULASI  │
    │ KREDIT                │
    └───────────┬───────────┘
                │
                ▼
    ┌───────────────────────┐
    │ Input:                │
    │ • NIK Pembeli         │
    │ • Harga OTR Kendaraan │
    └───────────┬───────────┘
                │
                ▼
    ┌───────────────────────┐
    │ Query SLIK OJK        │
    │ Ambil Riwayat Kredit  │
    └───────────┬───────────┘
                │
                ▼
    ┌───────────────────────┐
    │ Status Kolektibilitas│
    └───────────┬───────────┘
                │
       ┌────────┼────────┐
       │        │        │
       ▼        ▼        ▼
    ┌─────┐ ┌─────┐ ┌─────────────┐
    │ 1-5 │ │ 6-9 │ │ Kol 1-2     │
    │     │ │     │ │ (Baik)      │
    └─────┘ └─────┘ └──────┬──────┘
       │        │          │
       ▼        ▼          ▼
    ┌─────────┐ ┌─────────┐ ┌─────────────┐
    │ ✓ Layak │ │ ⚠️ Perlu│ │ ⚠️ Lanjut  │
    │ Kredit  │ │ Analisis│ │ ke NPF     │
    │         │ │ Surveyor│ │            │
    └────┬────┘ └────┬────┘ └──────┬──────┘
         │          │             │
         └──────────┼─────────────┘
                    │
                    ▼
    ┌───────────────────────┐
    │ Query NPF Leasing      │
    │ Partner                │
    └───────────┬───────────┘
                │
                ▼
    ┌───────────────────────┐
    │ NPF Net Leasing?      │
    └───────────┬───────────┘
                │
       ┌────────┼────────┐
       │        │        │
       ▼        ▼        ▼
    ┌─────┐ ┌─────┐ ┌─────┐
    │ ≤1% │ │1-3% │ │3-5% │
    └─────┘ └─────┘ └─────┘
       │        │        │
       ▼        ▼        ▼
    ┌─────┐ ┌─────┐ ┌─────┐
    │ 0%  │ │ 10% │ │ 15% │
    └─────┘ └─────┘ └─────┘
       │        │        │
       └────────┼────────┘
                │
                ▼
         ┌───────────┐
         │ NPF > 5%? │
         └─────┬─────┘
               │
          YA   │   TIDAK
          ▼    │    ▼
    ┌──────────┐ │  ┌────────────┐
    │ DP = 20% │ │  │ ⚠️ Error   │
    └──────────┘ │  │ Hubungi    │
                 │  │ Admin      │
                 │  └────────────┘
                 │
                 ▼
    ┌───────────────────────┐
    │ Hitung Angsuran:      │
    │                       │
    │ • Tenor: 12-72 bulan  │
    │ • Bunga: sesuai kat.  │
    └───────────┬───────────┘
                │
                ▼
    ┌───────────────────────┐
    │ OUTPUT:               │
    │ • DP Minimum           │
    │ • Angsuran/Bulan      │
    │ • Total Bayar         │
    └───────────────────────┘
                │
                ▼
         ┌───────────┐
         │ ✅ SELESAI│
         └───────────┘
```

### 3.2.2 Matrix NPF vs DP Minimum

```
┌─────────────────────────────────────────────────────────────────┐
│                    MATRIX NPF vs DP MINIMUM                     │
└─────────────────────────────────────────────────────────────────┘

╔═══════════════════╦════════════════╦═══════════════════════════════╗
║   NPF Leasing     ║  DP Minimum   ║         Penjelasan            ║
╠═══════════════════╬════════════════╬═══════════════════════════════╣
║                   ║                ║                               ║
║   NPF ≤ 1%        ║     0%        ║  Sangat sehat                 ║
║                   ║                ║  Kredit Tanpa Uang Muka       ║
║                   ║                ║                               ║
╠═══════════════════╬════════════════╬═══════════════════════════════╣
║                   ║                ║                               ║
║   NPF 1% - 3%     ║     10%       ║  Sehat                        ║
║                   ║                ║  Uang Muka Ringan             ║
║                   ║                ║                               ║
╠═══════════════════╬════════════════╬═══════════════════════════════╣
║                   ║                ║                               ║
║   NPF 3% - 5%     ║     15%       ║  Cukup sehat                  ║
║                   ║                ║  Uang Muka Sedang            ║
║                   ║                ║                               ║
╠═══════════════════╬════════════════╬═══════════════════════════════╣
║                   ║                ║                               ║
║   NPF > 5%        ║     20%       ║  Perlu kehati-hatian          ║
║                   ║                ║  Uang Muka Tinggi             ║
║                   ║                ║                               ║
╚═══════════════════╩════════════════╩═══════════════════════════════╝

📌 CATATAN:
   • NPF = Non-Performing Finance (rasio kredit macet)
   • Semakin rendah NPF, semakin sehat perusahaan leasing
   • DP Minimum adalah batas BAWAH, boleh lebih tinggi
```

### 3.2.3 Simulasi Contoh Kredit

```
┌─────────────────────────────────────────────────────────────────┐
│                 SIMULASI CONTOH KREDIT                          │
└─────────────────────────────────────────────────────────────────┘

📋 DATA:
   • Harga OTR = Rp200.000.000
   • NPF Leasing = 2.5% → DP Minimum = 15%

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📊 PERHITUNGAN:

1. DP = 15% × Rp200.000.000 = Rp30.000.000
2. Pinjaman = Rp200.000.000 - Rp30.000.000 = Rp170.000.000
3. Bunga = 8% per tahun (flat)
4. Tenor = 48 bulan
5. Total Bunga = Rp170.000.000 × 8% × 4 = Rp54.400.000
6. Angsuran = (Rp170.000.000 + Rp54.400.000) / 48 
            = Rp224.400.000 / 48
            = Rp4.675.000/bulan

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

💰 RINGKASAN:

   • DP               : Rp30.000.000
   • Pinjaman         : Rp170.000.000
   • Angsuran/Bulan   : Rp4.675.000
   • Total Bunga      : Rp54.400.000
   • Total Pembayaran : Rp224.400.000
```

---

## 3.3 Validasi Dokumen

> **Referensi:** BusinessLogic-MermaidJS.md → Section 3: Validasi Dokumen

### 3.3.1 Flowchart Matriks Validasi Berdasarkan Jenis Kendaraan

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                 FLOWCHART VALIDASI DOKUMEN                                  │
└─────────────────────────────────────────────────────────────────────────────┘

    ┌───────────────────────┐
    │ 🚀 VALIDASI DOKUMEN   │
    └───────────┬───────────┘
                │
                ▼
    ┌───────────────────────┐
    │ Jenis Kendaraan?       │
    └───────────┬───────────┘
           ┌────┴────┐
           │         │
    ┌──────┴───┐ ┌───┴──────┐ ┌──────────────┐
    │  BARU    │ │ BEKAS    │ │ BEKAS KREDIT │
    └──────┬───┘ └─────┬────┘ └──────┬───────┘
           │          │              │
           ▼          ▼              ▼
    ┌────────────┐ ┌───────────┐ ┌────────────┐
    │📋 Checklist│ │📋 Checklist│ │📋 Checklist│
    │ Mobil Baru │ │Mobil Bekas│ │Bekas Kredit│
    └──────┬────┘ └─────┬────┘ └──────┬───────┘
           │          │              │
           ▼          ▼              ▼
    ┌────────────┐ ┌───────────┐ ┌────────────┐
    │✓ Faktur   │ │✓ BPKB Asli│ │✓ Kontrak   │
    │  Kendaraan│ │  Hologram │ │  Leasing   │
    │  Asli     │ │  Tri Brata│ │  Lama      │
    └──────┬────┘ └─────┬────┘ └──────┬───────┘
           │          │              │
           ▼          ▼              ▼
    ┌────────────┐ ┌───────────┐ ┌────────────┐
    │✓ NIK Valid │ │✓ Verifikasi│ │✓ Fotokopi │
    │  Checksum  │ │  Benang UV│ │  BPKB     │
    │           │ │           │ │  Terlegalis│
    └──────┬────┘ └─────┬────┘ └──────┬───────┘
           │          │              │
           ▼          ▼              ▼
    ┌────────────┐ ┌───────────┐ ┌────────────┐
    │✓ SRUT     │ │✓ STNK     │ │✓ Histori  │
    │  (Surat   │ │  Aktif    │ │  Setoran   │
    │  Registrasi│ │          │ │  Pembayaran│
    │  Uji Tipe)│ │          │ │  Terakhir  │
    └──────┬────┘ └─────┬────┘ └──────┬───────┘
           │          │              │
           ▼          ▼              ▼
    ┌────────────┐ ┌───────────┐ ┌────────────┐
    │✓ KTP Asli │ │✓ Faktur   │ │✓ Lunas    │──────► ⚠️ OVERDUE
    │  Pembeli  │ │  Pembelian│ │  Sebelumnya?│    │   BLOKIR!
    └──────┬────┘ └─────┬────┘ └──────┬───────┘    │
           │          │         ┌────┴────┐       │
           │          │         │         │       │
           │          │      YA │      TIDAK      │
           │          │         ▼         │       │
           │          │    ┌────────┐     │       │
           │          │    │✓ Surat│     │       │
           │          │    │  Keterangan│     │
           │          │    │  Lunas   │     │       │
           │          │    └────────┘     │       │
           │          │         │         │       │
           │          ▼         ▼         │       │
           │    ┌─────────────────────┐    │       │
           │    │ Eks Aset Perusahaan?│    │       │
           │    └─────────────────────┘    │       │
           │         │         │           │       │
           │      YA │      TIDAK          │       │
           │         │         │           │       │
           │         ▼         │           │       │
           │    ┌────────┐     │           │       │
           │    │✓ SPH   │     │           │       │
           │    │(Surat  │     │           │       │
           │    │Pelepasan│    │           │       │
           │    │  Hak)  │     │           │       │
           │    └────────┘     │           │       │
           │         │         │           │       │
           └─────────┼─────────┼───────────┘       │
                     │         │                   │
                     └────┬────┘                   │
                          │                       │
                          ▼                       ▼
                   ┌──────────────┐       ┌──────────────┐
                   │ ✅ LOLOS     │       │ ❌ DITOLAK   │
                   │ VALIDASI     │       │              │
                   └──────────────┘       └──────────────┘
```

### 3.3.2 Flowchart Validasi NPWP (Luhn Checksum)

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                 FLOWCHART VALIDASI NPWP (LUHN CHECKSUM)                      │
└─────────────────────────────────────────────────────────────────────────────┘

    ┌───────────────────────┐
    │ 🚀 VALIDASI NPWP     │
    └───────────┬───────────┘
                │
                ▼
    ┌───────────────────────┐
    │ Input: 15 Digit NPWP │
    │ Format: XX.XXX.XXX.X- │
    │        XXX.XXX        │
    └───────────┬───────────┘
                │
                ▼
    ┌───────────────────────┐
    │ Hapus Titik & Strip   │
    │ Ambil 15 Digit        │
    └───────────┬───────────┘
                │
                ▼
    ┌───────────────────────┐
    │ Inisialisasi Pengali: │
    │ 5,7,2,3,4,5,6,7,8,9,  │
    │ 2,3,4,5,6            │
    └───────────┬───────────┘
                │
                ▼
         ┌───────────────┐
         │ Loop: i=1 to 15│
         └───────┬───────┘
                 │
                 ▼
    ┌───────────────────────┐
    │ Hitung:               │
    │ Hasil += Digit[i] ×   │
    │ Pengali[i]            │
    └───────────┬───────────┘
                 │
                 ▼
    ┌───────────────────────┐
    │    i < 15?            │
    └───┬─────────┬─────────┘
       YA│       TIDAK│
         │         │
         ▼         │
    ┌─────────┐    │
    │ i++     │    │
    └────┬────┘    │
         │         │
         └────┬────┘
              │
              ▼
    ┌───────────────────────┐
    │ Mod 11 = Hasil mod 11 │
    └───────────┬───────────┘
                │
                ▼
    ┌───────────────────────┐
    │ Digit[15] =?          │
    │ (11 - Mod 11) mod 11  │
    └───────────┬───────────┘
           ┌────┴────┐
           │         │
          YA│        │TIDAK
           │         │
           ▼         ▼
    ┌──────────┐ ┌──────────┐
    │ ✅ NPWP  │ │ ❌ NPWP  │
    │ VALID    │ │ INVALID  │
    │ Checksum │ │ Checksum │
    │ Match    │ │ Error    │
    └──────────┘ └──────────┘
```

### 3.3.3 Flowchart Deteksi Hologram BPKB

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                 FLOWCHART DETEKSI HOLGRAM BPKB                              │
└─────────────────────────────────────────────────────────────────────────────┘

    ┌───────────────────────────┐
    │ 🚀 DETEKSI HOLGRAM BPKB  │
    └───────────┬───────────────┘
                │
                ▼
    ┌───────────────────────────┐
    │ Input: Foto/Scan          │
    │ Halaman BPKB              │
    └───────────┬───────────────┘
                │
                ▼
    ┌───────────────────────────┐
    │ Pre-processing:           │
    │ • Normalisasi Ukuran      │
    │ • Konversi Grayscale      │
    └───────────┬───────────────┘
                │
                ▼
    ┌───────────────────────────┐
    │ Edge Detection            │
    │ (Algoritma Canny)         │
    └───────────┬───────────────┘
                │
                ▼
    ┌───────────────────────────┐
    │ Feature Extraction        │
    └───────────┬───────────────┘
                │
       ┌────────┼────────┐
       │        │        │
       ▼        ▼        ▼
   ┌───────┐┌───────┐┌───────┐
   │Deteksi││Deteksi││Deteksi│
   │Pola   ││Benang ││Watermark│
   │Tri    ││UV     ││'BPKB'│
   │Brata  ││       ││       │
   └───┬───┘└───┬───┘└───┬───┘
       │        │        │
       ▼        ▼        ▼
   ┌───────┐┌───────┐┌───────┐
   │Cek    ││Verif  ││Cek    │
   │Simetri││Benang ││Tanda  │
   │3      ││Pelangi││Air    │
   │Segitiga│       ││       │
   └───┬───┘└───┬───┘└───┬───┘
       │        │        │
       └────────┼────────┘
                │
                ▼
    ┌───────────────────────────┐
    │   Semua Fitur Terdeteksi? │
    └───────────┬───────────────┘
           ┌────┴────┐
       3/3 │     2/3 │    1/3  │  0/3
           │     │   │     │   │
           ▼     ▼   ▼     ▼   ▼
       ┌─────┐┌─────┐┌─────┐┌─────┐
       │ 95- ││ 70- ││ 40- ││<10% │
       │100% ││ 85% ││ 60% ││     │
       └──┬──┘└──┬──┘└──┬──┘└──┬──┘
          │      │      │      │
          ▼      ▼      ▼      ▼
       ┌─────┐┌─────┐┌─────┐┌─────┐
       │✅   ││✅   ││⚠️   ││❌   │
       │HOLOG││HOLOG││RAGU-││HOLOG│
       │RAM  ││RAM  ││RAGU ││RAM  │
       │ASLI ││ASLI ││Verif││PALSU│
       │     ││     ││Manul││Blokir│
       └─────┘└─────┘└─────┘└─────┘

📌 TARGET: Akurasi ≥ 95%
```

### 3.3.4 Flowchart Validasi Batas Waktu Fidusia (30 Hari)

```
┌─────────────────────────────────────────────────────────────────────────────┐
│              FLOWCHART VALIDASI BATAS WAKTU FIDUSIA (30 HARI)               │
└─────────────────────────────────────────────────────────────────────────────┘

    ┌───────────────────────────────────────┐
    │ 🚀 PENDAFTARAN JAMINAN FIDUSIA        │
    └───────────────────┬───────────────────┘
                        │
                        ▼
    ┌───────────────────────────────────────┐
    │ Input:                                │
    │ • Tanggal Akta Notaris                │
    │ • Tanggal Submit ke AHU               │
    └───────────────────┬───────────────────┘
                        │
                        ▼
    ┌───────────────────────────────────────┐
    │ Hitung Selisih Hari:                  │
    │ Tanggal Submit - Tanggal Akta         │
    └───────────────────┬───────────────────┘
                        │
                        ▼
    ┌───────────────────────────────────────┐
    │         Jumlah Hari = Selisih?        │
    └───────────────────┬───────────────────┘
                   ┌────┴────┐
                  ≤30 │      │ >30
                   ┌──┴──┐   ┌──┴──┐
                   │     │   │     │
                   ▼     │   │     ▼
              ┌──────────┐│   │ ┌─────────────────────┐
              │ ✅        ││   │ │ ❌ PENDAFTARAN       │
              │ DITERIMA ││   │ │ DITOLAK OTOMATIS    │
              └──────────┘│   │ │ oleh Sistem AHU     │
                   │      │   │ └──────────┬──────────┘
                   │      │   │            │
                   │      │   │            ▼
                   │      │   │ ┌─────────────────────┐
                   │      │   │ │ ⚠️ ALERT:            │
                   │      │   │ │ Terlambat 30 Hari   │
                   │      │   │ │ Blokir Transaksi    │
                   │      │   │ │ Kredit               │
                   │      │   │ └─────────────────────┘
                   │      │   │
                   │      │   ▼
                   │      │ ┌─────────────────────┐
                   │      │ │ 💡 PERINGATAN:       │
                   │      │ │ Batas 30 hari adalah │
                   │      │ │ MUTLAK, tidak bisa   │
                   │      │ │ diperpanjang!         │
                   │      │ └─────────────────────┘
                   │      │
                   ▼      ▼
              ┌───────────────────────────┐
              │ Generate Sertifikat Fidusia│
              │ Elektronik                │
              └───────────┬───────────────┘
                          │
                          ▼
              ┌───────────────────────────┐
              │ Simpan ke Database        │
              │ + Notification Reminder  │
              └───────────────────────────┘

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🔔 SISTEM REMINDER OTOMATIS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

    ┌───────────────────────────────────────┐
    │           COUNTDOWN PENGINGAT         │
    └───────────────────┬───────────────────┘
                        │
       ┌────────────────┼────────────────┐
       │                │                │
       ▼                ▼                ▼
  ┌─────────┐    ┌─────────┐    ┌─────────┐
  │ Day -30 │    │ Day -15 │    │  Day -7 │
  │ 📧 Email│    │ 📱 SMS  │    │ 🔴 Alert│
  │ ke Dealer│   │ ke Admin│    │ ke Mgr  │
  └─────────┘    └─────────┘    └─────────┘
       │
       ▼
  ┌─────────┐
  │ Day -1  │
  │ 🚨 FINAL│
  │ WARNING │
  │ Segera! │
  └─────────┘
```

---

# URUTAN 4: FLOWCHART LOGIKA BISNIS (Bagian 2)

> 📌 **Referensi:** BusinessLogic-MermaidJS.md → Sections 4, 5

---

## 4.1 Proses PDI Lengkap

> **Referensi:** BusinessLogic-MermaidJS.md → Section 4: Logistik & PDI

### 4.1.1 Flowchart Proses PDI Lengkap

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                     FLOWCHART PROSES PDI LENGKAP                            │
└─────────────────────────────────────────────────────────────────────────────┘

    ┌───────────────────────────────────────┐
    │ 🚀 MULAI PDI                          │
    │ Pre-Delivery Inspection               │
    └───────────────────┬───────────────────┘
                        │
                        ▼
    ┌───────────────────────────────────────┐
    │ Input:                                │
    │ • Data Unit                           │
    │ • Nomor Rangka                       │
    └───────────────────┬───────────────────┘
                        │
                        ▼
    ┌───────────────────────────────────────┐
    │ 📋 Download Checklist PDI Digital     │
    │    (150-188 Titik)                    │
    └───────────────────┬───────────────────┘
                        │
                        ▼
    ┌───────────────────────────────────────┐
    │ 🔍 Inspeksi Exterior:                 │
    │    • Bodi                             │
    │    • Cat                              │
    │    • Kaca                             │
    │    • Lampu                            │
    │    • Ban                              │
    └───────────────────┬───────────────────┘
                        │
                        ▼
    ┌───────────────────────────────────────┐
    │ 🔧 Inspeksi Interior:                 │
    │    • Dashboard                        │
    │    • Jok                              │
    │    • AC                               │
    │    • Elektrikal                       │
    └───────────────────┬───────────────────┘
                        │
                        ▼
    ┌───────────────────────────────────────┐
    │ ⚙️ Inspeksi Mekanik:                  │
    │    • Mesin                            │
    │    • Transmisi                        │
    │    • Rem                              │
    │    • Suspensi                         │
    └───────────────────┬───────────────────┘
                        │
                        ▼
    ┌───────────────────────────────────────┐
    │ 💧 Pemeriksaan Cairan:                │
    │    • Oli                               │
    │    • Coolant                           │
    │    • Rem                               │
    │    • Windscreen                        │
    └───────────────────┬───────────────────┘
                        │
                        ▼
    ┌───────────────────────────────────────┐
    │ 🔌 Koneksi OBD-II:                    │
    │    Scan Port ECU                      │
    └───────────────────┬───────────────────┘
                        │
                        ▼
    ┌───────────────────────────────────────┐
    │        DTC Error Terdeteksi?           │
    └───────────────────┬───────────────────┘
                   ┌────┴────┐
                  YA│        │TIDAK
                   ┌┴────┐    │
                   │⚠️   │    │
                   │Hapus│    │
                   │DTC  │    │
                   │Error│    │
                   └──┬──┘    │
                      │       │
                      └─┬─────┘
                        │
                        ▼
    ┌───────────────────────────────────────┐
    │ ✅ Tidak Ada DTC Error                │
    └───────────────────┬───────────────────┘
                        │
                        ▼
    ┌───────────────────────────────────────┐
    │ 🔧 De-waxing:                        │
    │    Pembilasan Wax Pelindung Bodi      │
    └───────────────────┬───────────────────┘
                        │
                        ▼
    ┌───────────────────────────────────────┐
    │ ⚡ Pasang Backup Fuse:                │
    │    Kelistrikan                        │
    └───────────────────┬───────────────────┘
                        │
                        ▼
    ┌───────────────────────────────────────┐
    │ 🚗 Uji Jalan:                         │
    │    Minimal 10 KM                       │
    └───────────────────┬───────────────────┘
                        │
                        ▼
    ┌───────────────────────────────────────┐
    │        Jarak Tempuh ≥ 10 KM?           │
    └───────────────────┬───────────────────┘
                   ┌────┴────┐
                  YA│        │TIDAK
                   ┌┴────┐    │
                   │✅   │    │
                   │LULUS│   ┌┴─────────────┐
                   │Uji  │   │⚠️ JANGAN KIRIM│
                   │Jalan│   │Lanjutkan Uji  │
                   └──┬──┘   │Jalan         │
                      │      └──────┬────────┘
                      │             │
                      │    ┌────────┘
                      │    │
                      └────┘
                        │
                        ▼
    ┌───────────────────────────────────────┐
    │ 📝 Isi Laporan PDI Digital            │
    │    (Online)                           │
    └───────────────────┬───────────────────┘
                        │
                        ▼
    ┌───────────────────────────────────────┐
    │ 📸 Upload Foto Unit Post-PDI          │
    └───────────────────┬───────────────────┘
                        │
                        ▼
    ┌───────────────────────────────────────┐
    │ ✅ Unit APPROVED                      │
    │    Siap Dikirim                        │
    └───────────────────┬───────────────────┘
                        │
                        ▼
    ┌───────────────────────────────────────┐
    │ OUTPUT:                              │
    │ • Laporan PDI                         │
    │ • Status: APPROVED                    │
    │ • Tanggal & Teknisi                   │
    └───────────────────────────────────────┘
```

### 4.1.2 Detail Checklist PDI per Tahapan

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                   DETAIL CHECKLIST PDI PER TAHAPAN                         │
└─────────────────────────────────────────────────────────────────────────────┘

╔═════════════════════════════════════════════════════════════════════════════╗
║  TAHAP 1: EXTERIOR (40 Titik)                                             ║
╠═════════════════════════════════════════════════════════════════════════════╣
║                                                                             ║
║   ┌─────────┐    ┌─────────┐    ┌─────────┐    ┌─────────┐               ║
║   │  Bod i  │    │  Bodi   │    │ Depan   │    │Belakang │               ║
║   │  Kiri   │───▶│ Kanan   │───▶│         │───▶│         │               ║
║   └─────────┘    └─────────┘    └─────────┘    └─────────┘               ║
║       │                                              │                      ║
║       │         ┌─────────┐    ┌─────────┐          │                      ║
║       └────────▶│  Atap   │◀───│  Kaca   │◀─────────┘                      ║
║                 │         │    │ Depan   │                                   ║
║                 └─────────┘    └─────────┘                                   ║
║                      │                                                    ║
║                      ▼                                                    ║
║                 ┌─────────┐    ┌─────────┐    ┌─────────┐                  ║
║                 │  Kaca   │    │ Velg &  │    │  Ban    │                  ║
║                 │Belakang │    │  Ban    │    │(Spare)  │                  ║
║                 └─────────┘    └─────────┘    └─────────┘                  ║
║                                                                             ║
╠═════════════════════════════════════════════════════════════════════════════╣
║  TAHAP 2: INTERIOR (35 Titik)                                             ║
╠═════════════════════════════════════════════════════════════════════════════╣
║                                                                             ║
║   ┌─────────────┐    ┌─────────────┐    ┌─────────────┐                   ║
║   │ Dashboard   │───▶│ Jok Depan   │───▶│ Jok Belakang│                   ║
║   │ (Indikator) │    │(2 titik)   │    │             │                   ║
║   └─────────────┘    └──────┬──────┘    └─────────────┘                   ║
║                             │                                             ║
║                             ▼                                             ║
║                 ┌─────────────┐    ┌─────────────┐                         ║
║                 │AC & Pendingin│───▶│ Kelistrikan │                        ║
║                 │             │    │ (Window,Lock)│                        ║
║                 └─────────────┘    └─────────────┘                         ║
║                                                                             ║
╠═════════════════════════════════════════════════════════════════════════════╣
║  TAHAP 3: MEKANIK (50 Titik)                                              ║
╠═════════════════════════════════════════════════════════════════════════════╣
║                                                                             ║
║   ┌─────────┐    ┌─────────┐    ┌─────────┐    ┌─────────┐              ║
║   │  Mesin  │───▶│Transmisi│───▶│   Rem   │───▶│Suspensi │              ║
║   │(Oli,Filter)│  │(Minyak)│    │(Pad,Disc)│   │(Shock,Arm)│             ║
║   └─────────┘    └─────────┘    └─────────┘    └─────────┘              ║
║       │                                                         │          ║
║       │         ┌─────────┐                                    │          ║
║       └────────▶│ Kopling │◀───────────────────────────────────┘          ║
║                 │         │                                                 ║
║                 └─────────┘                                                 ║
║                                                                             ║
╠═════════════════════════════════════════════════════════════════════════════╣
║  TAHAP 4: ELEKTRONIK (25 Titik)                                           ║
╠═════════════════════════════════════════════════════════════════════════════╣
║                                                                             ║
║   ┌─────────────┐    ┌─────────────┐    ┌─────────────┐                   ║
║   │  Head Unit  │───▶│  OBD-II     │───▶│  ECU Error  │                   ║
║   │(Audio,Navi) │    │   Scan      │    │   Codes     │                   ║
║   └─────────────┘    └──────┬──────┘    └─────────────┘                   ║
║                             │                                              ║
║                             ▼                                              ║
║                 ┌─────────────────────┐                                   ║
║                 │ Sensor & Actuator    │                                   ║
║                 │ (ABS, Airbag, dll)  │                                   ║
║                 └─────────────────────┘                                   ║
║                                                                             ║
╠═════════════════════════════════════════════════════════════════════════════╣
║  TAHAP 5: UJI JALAN (Minimal 10 KM)                                      ║
╠═════════════════════════════════════════════════════════════════════════════╣
║                                                                             ║
║   ┌─────────────┐    ┌─────────────┐    ┌─────────────┐                   ║
║   │ Akselerasi  │───▶│ Pengereman  │───▶│  Handling   │                   ║
║   │             │    │             │    │ (Tikungan)  │                   ║
║   └─────────────┘    └─────────────┘    └──────┬──────┘                   ║
║                                                │                           ║
║                                                ▼                           ║
║                                      ┌─────────────────┐                    ║
║                                      │   Suara Mesin  │                    ║
║                                      │   & Getaran    │                    ║
║                                      └─────────────────┘                    ║
║                                                                             ║
╚═════════════════════════════════════════════════════════════════════════════╝

📊 TOTAL TITIK PEMERIKSAAN: 150-188 Titik
   • Mobil Biasa: 150 Titik
   • SUV/Premium: 188 Titik
```

---

## 4.2 Alur STCK (Plat Nomor Sementara)

> **Referensi:** BusinessLogic-MermaidJS.md → Section 4.3

### Flowchart Alur STCK

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                      FLOWCHART ALUR STCK                                    │
└─────────────────────────────────────────────────────────────────────────────┘

    ┌───────────────────────────────────────┐
    │ 🚀 PROSES STCK                         │
    │ (Surat Tanda Coba Keliling)           │
    └───────────────────┬───────────────────┘
                        │
                        ▼
    ┌───────────────────────────────────────┐
    │ Input:                                │
    │ • Data Pembeli                        │
    │ • Data Kendaraan                      │
    └───────────────────┬───────────────────┘
                        │
                        ▼
    ┌───────────────────────────────────────┐
    │ Ajukan STCK ke Samsat                 │
    └───────────────────┬───────────────────┘
                        │
                        ▼
    ┌───────────────────────────────────────┐
    │ PNBP Bayar: Rp50.000                  │
    └───────────────────┬───────────────────┘
                        │
                        ▼
    ┌───────────────────────────────────────┐
    │ Terbit STCK                            │
    │ Masa Berlaku: 1 Bulan                  │
    └───────────────────┬───────────────────┘
                        │
                        ▼
    ┌───────────────────────────────────────┐
    │       Tanggal Sekarang?                │
    └───────────────────┬───────────────────┘
                        │
              ┌─────────┴─────────┐
              │                   │
         Dalam │                Expired
        Periode│                   │
              │                   │
              ▼                   ▼
    ┌─────────────────┐   ┌─────────────────┐
    │ Digunakan       │   │ ❌ STCK Expired │
    │ Untuk?          │   │ Harus Perpanjang│
    └────────┬────────┘   └─────────────────┘
             │
    ┌────────┼────────┬─────────────────┐
    │        │        │                 │
    ▼        ▼        ▼                 ▼
┌───────┐┌───────┐┌───────────┐┌───────────┐
│Logistik││ Uji   ││ Keperluan ││ Keluar    │
│Diler → ││Fisik  ││  Pribadi  ││  Kota     │
│Konsumen││Samsat ││           ││           │
└───┬───┘└───┬───┘└─────┬─────┘└─────┬─────┘
    │        │          │             │
    │        │          │             │
    ▼        ▼          ▼             ▼
┌───────┐┌───────┐┌───────────┐┌───────────┐
│✅ BOLEH││✅ BOLEH││ ❌ DILARANG││ ❌ DILARANG│
│       ││       ││ Bisa      ││ Bisa      │
│       ││       ││ Ditilang! ││ Ditilang! │
└───────┘└───────┘└───────────┘└───────────┘

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📋 ATURAN KRITIS STCK
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

╔═══════════════════╦═══════════════════════════════════════════════╗
║  ASPEK            ║  ATURAN                                     ║
╠═══════════════════╬═══════════════════════════════════════════════╣
║                   ║                                              ║
║  Masa Berlaku     ║  1 BULAN sejak diterbitkan                   ║
║                   ║                                              ║
╠═══════════════════╬═══════════════════════════════════════════════╣
║                   ║                                              ║
║  Keperluan        ║  • Logistik diler-to-konsumen               ║
║                   ║  • Uji fisik Samsat                          ║
║                   ║                                              ║
╠═══════════════════╬═══════════════════════════════════════════════╣
║                   ║                                              ║
║  ❌ LARANGAN #1    ║  Dilarang untuk keperluan PRIBADI di jalan   ║
║                   ║  umum (bisa ditilang)                        ║
║                   ║                                              ║
╠═══════════════════╬═══════════════════════════════════════════════╣
║                   ║                                              ║
║  ❌ LARANGAN #2    ║  Dilarang digunakan untuk LUAR KOTA          ║
║                   ║  (bisa ditilang)                             ║
║                   ║                                              ║
╠═══════════════════╬═══════════════════════════════════════════════╣
║                   ║                                              ║
║  Sanksi           ║  Tilang oleh polisi lalu lintas               ║
║                   ║                                              ║
╠═══════════════════╬═══════════════════════════════════════════════╣
║                   ║                                              ║
║  Biaya PNBP       ║  Rp50.000 (resmi)                            ║
║                   ║                                              ║
╚═══════════════════╩═══════════════════════════════════════════════╝
```

---

## 4.3 Ringkasan Aturan Blokir Sistem

> **Referensi:** BusinessLogic-MermaidJS.md → Section 5

### Flowchart Ringkasan Aturan Blokir

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                   FLOWCHART RINGKASAN ATURAN BLOKIR                         │
└─────────────────────────────────────────────────────────────────────────────┘

                          ┌─────────────────────┐
                          │ 🚫 ATURAN BLOKIR    │
                          │    OTOMATIS         │
                          └──────────┬──────────┘
                                     │
        ┌────────────┬────────────────┼────────────────┬────────────┐
        │            │                │                │            │
        ▼            ▼                ▼                ▼            ▼
   ┌─────────┐  ┌─────────┐    ┌─────────┐    ┌─────────┐   ┌─────────┐
   │❌       │  │❌       │    │❌       │    │❌       │   │❌       │
   │Dokumen │  │NPWP     │    │Hologram│    │Fidusia │   │NPF      │
   │Tidak   │  │Invalid  │    │BPKB    │    │Terlambat│  │Leasing  │
   │Lengkap │  │Checksum │    │Tidak   │    │> 30 Hari│  │Tidak    │
   │        │  │        │    │Terdeteksi│   │        │   │Valid    │
   └────┬────┘  └────┬────┘    └────┬────┘    └────┬────┘   └────┬────┘
        │            │                │                │            │
        │            │                │                │            │
        │            │                │                │            │
        │            │                │                │            │
        ▼            ▼                ▼                ▼            ▼
   ┌─────────┐  ┌─────────┐    ┌─────────┐    ┌─────────┐   ┌─────────┐
   │❌       │  │❌       │    │❌       │    │❌       │   │❌       │
   │DTC Error│  │Uji Jalan│    │STCK    │    │Overdue  │   │         │
   │ECU      │  │< 10 KM  │    │Expired │    │Kredit   │   │         │
   │Belum    │  │         │    │        │    │Bekas    │   │         │
   │Dihapus  │  │         │    │        │    │        │   │         │
   └────┬────┘  └────┬────┘    └────┬────┘    └────┬────┘   └────┬────┘
        │            │                │                │            │
        │            │                │                │            │
        │            │                │                │            │
        │            │                │                │            │
        └────────────┴────────────────┼────────────────┴────────────┘
                                     │
                                     ▼
                          ┌─────────────────────┐
                          │ ⚠️ TAMPILKAN PESAN  │
                          │ DOKUMEN/TINDAKAN    │
                          │ YANG KURANG         │
                          └──────────┬──────────┘
                                     │
                                     ▼
                          ┌─────────────────────┐
                          │ 🔒 BLOKIR TRANSAKSI│
                          │ SAMPAI VALID        │
                          └─────────────────────┘

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📋 DETAIL SETIAP ATURAN BLOKIR
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

╔═══════════════════╦═══════════════════════════════════════════════╗
║  KODE             ║  ATURAN & AKSI                              ║
╠═══════════════════╬═══════════════════════════════════════════════╣
║                   ║                                              ║
║  BLOKIR-001       ║  Dokumen Tidak Lengkap                      ║
║                   ║  → Tampilkan list dokumen yang kurang       ║
║                   ║                                              ║
╠═══════════════════╬═══════════════════════════════════════════════╣
║                   ║                                              ║
║  BLOKIR-002       ║  NPWP Invalid Checksum                     ║
║                   ║  → Validasi Luhn gagal                      ║
║                   ║                                              ║
╠═══════════════════╬═══════════════════════════════════════════════╣
║                   ║                                              ║
║  BLOKIR-003       ║  Hologram BPKB Tidak Terdeteksi             ║
║                   ║  → Akurasi < 95%                           ║
║                   ║                                              ║
╠═══════════════════╬═══════════════════════════════════════════════╣
║                   ║                                              ║
║  BLOKIR-004       ║  Fidusia Terlambat > 30 Hari                ║
║                   ║  → Sistem AHU tolak otomatis                 ║
║                   ║                                              ║
╠═══════════════════╬═══════════════════════════════════════════════╣
║                   ║                                              ║
║  BLOKIR-005       ║  NPF Leasing Tidak Valid                    ║
║                   ║  → Tidak bisa hitung DP minimum             ║
║                   ║                                              ║
╠═══════════════════╬═══════════════════════════════════════════════╣
║                   ║                                              ║
║  BLOKIR-006       ║  DTC Error ECU Belum Dihapus                ║
║                   ║  → OBD scan masih ada error codes           ║
║                   ║                                              ║
╠═══════════════════╬═══════════════════════════════════════════════╣
║                   ║                                              ║
║  BLOKIR-007       ║  Uji Jalan < 10 KM                          ║
║                   ║  → GPS tracking kurang                       ║
║                   ║                                              ║
╠═══════════════════╬═══════════════════════════════════════════════╣
║                   ║                                              ║
║  BLOKIR-008       ║  STCK Expired / Digunakan Tidak Sesuai      ║
║                   ║  → Plat sementara tidak valid               ║
║                   ║                                              ║
╠═══════════════════╬═══════════════════════════════════════════════╣
║                   ║                                              ║
║  BLOKIR-009       ║  Overdue Kredit Bekas                       ║
║                   ║  → Ada tunggakan leasing sebelumnya        ║
║                   ║                                              ║
╚═══════════════════╩═══════════════════════════════════════════════╝
```

---

# URUTAN 5: USE CASES & KESIMPULAN

## 5.1 UC-01: Estimasi Harga Online

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                        UC-01: ESTIMASI HARGA ONLINE                        │
└─────────────────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────────────────┐
│ AKTOR UTAMA:                                                                │
│   • Pembeli                                                                │
│   • Sales Dealer                                                            │
├─────────────────────────────────────────────────────────────────────────────┤
│ AKTOR PENDUKUNG:                                                           │
│   • Pricing Engine                                                          │
│   • API Samsat/SIGNAL                                                      │
├─────────────────────────────────────────────────────────────────────────────┤
│ DESKRIPSI:                                                                  │
│   Kalkulasi estimasi harga final kendaraan (OTR) secara real-time berdasarkan │
│   regulasi perpajakan otomotif Indonesia.                                   │
├─────────────────────────────────────────────────────────────────────────────┤
│ PRAKONDISI:                                                                 │
│   ✓ Pengguna telah login                                                    │
│   ✓ Data spesifikasi unit tersedia                                          │
│   ✓ Koneksi internet aktif                                                 │
├─────────────────────────────────────────────────────────────────────────────┤
│ ALUR:                                                                       │
│   1. Pembeli memilih tipe dan spesifikasi kendaraan                        │
│   2. Sistem mengambil NJKB dari database/reference                         │
│   3. Jika kendaraan bekas → ambil NJKB dari STNK                            │
│   4. Hitung PPN (DPP × 12% = 11% × NJKB)                                  │
│   5. Hitung PKB dan Opsen PKB (66%)                                        │
│   6. Hitung BBNKB dan Opsen BBNKB (66%)                                    │
│   7. Jika wilayah DKI → Opsen = 0, BBNKB = 12.5%                           │
│   8. Cek pajak progresif via API Samsat                                    │
│   9. Hitung PNBP (SWDKLLJ, STNK, TNKB, BPKB)                              │
│   10. Tampilkan rincian OTR lengkap                                        │
├─────────────────────────────────────────────────────────────────────────────┤
│ PASKAKONDISI (SUKSES):                                                     │
│   ✓ Sistem menyajikan rincian biaya OTR                                     │
│   ✓ Mencatat riwayat penelusuran                                           │
│   ✓ Pembeli siap lanjut ke reservasi unit                                  │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 5.2 UC-03: Pengajuan Kredit (SLIK OJK & Scoring)

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                    UC-03: PENGAJUAN KREDIT (SLIK OJ K)                      │
└─────────────────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────────────────┐
│ AKTOR UTAMA:                                                                │
│   • Pembeli                                                                │
│   • Surveyor Leasing                                                        │
├─────────────────────────────────────────────────────────────────────────────┤
│ AKTOR PENDUKUNG:                                                           │
│   • Sistem Fintech                                                          │
│   • API SLIK OJK                                                            │
│   • API PEFINDO                                                             │
├─────────────────────────────────────────────────────────────────────────────┤
│ DESKRIPSI:                                                                  │
│   Memfasilitasi pengajuan kredit mobil secara asynchronous melalui          │
│   integrasi data riwayat keuangan SLIK OJK dan credit scoring otomatis.     │
├─────────────────────────────────────────────────────────────────────────────┤
│ PRAKONDISI:                                                                 │
│   ✓ Pembeli lolos verifikasi biometrik (face matching ≥85%)              │
│   ✓ Status kendaraan = BOOKED                                               │
│   ✓ Koneksi ESB aktif                                                      │
├─────────────────────────────────────────────────────────────────────────────┤
│ ALUR:                                                                       │
│   1. Pembeli memilih paket kredit (DP, tenor)                              │
│   2. Sistem query API SLIK OJK dengan NIK                                  │
│   3. Dapatkan data iDeb (kolektibilitas, riwayat)                          │
│   4. Sistem query NPF leasing partner                                      │
│   5. Hitung DP minimum berdasarkan NPF (0%, 10%, 15%, 20%)                 │
│   6. Validasi DP yang diajukan ≥ DP minimum                                │
│   7. Hitung angsuran dan total pembayaran                                  │
│   8. Submit aplikasi → status PENDING                                       │
│   9. Generate submission ID                                                 │
│   10. Kirim notifikasi ke surveyor leasing                                  │
├─────────────────────────────────────────────────────────────────────────────┤
│ PASKAKONDISI (SUKSES):                                                     │
│   ✓ Aplikasi kredit tersimpan status PENDING                               │
│   ✓ ID pengajuan diterbitkan                                               │
│   ✓ Riwayat iDeb tertarik                                                   │
│   ✓ Notifikasi terkirim ke surveyor                                        │
├─────────────────────────────────────────────────────────────────────────────┤
│ ALUR ASYNC (ESB):                                                          │
│   ┌─────────┐    ┌─────────┐    ┌─────────┐    ┌─────────┐                  │
│   │ Diler   │───▶│ Message │───▶│ Leasing │───▶│ Response│                  │
│   │ Request │    │  Broker │    │  ESB    │    │Callback │                  │
│   └─────────┘    └─────────┘    └─────────┘    └─────────┘                  │
│       │                                                        │            │
│       │  Response Time Rata-rata: 4.22 detik                 │            │
│       │  Error Rate: Rendah                                   │            │
│       ▼                                                        ▼            │
│   ┌──────────────────────────────────────────────────────────────┐          │
│   │                   STATUS APLIKASI                            │          │
│   │  PENDING → APPROVED → DOCUMENTED → FIDUSIA_COMPLETE         │          │
│   └──────────────────────────────────────────────────────────────┘          │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 5.3 UC-09: Pendaftaran Jaminan Fidusia

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                   UC-09: PENDAFTARAN JAMINAN FIDUSIA                       │
└─────────────────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────────────────┐
│ AKTOR UTAMA:                                                                │
│   • Admin Dealer                                                            │
│   • Notaris Rekanan                                                         │
├─────────────────────────────────────────────────────────────────────────────┤
│ AKTOR PENDUKUNG:                                                           │
│   • API Ditjen AHU Kemenkumham                                             │
│   • Sistem Fintech                                                           │
├─────────────────────────────────────────────────────────────────────────────┤
│ DESKRIPSI:                                                                  │
│   Pengikatan dan pendaftaran jaminan fidusia secara elektronik ke          │
│   Ditjen AHU melalui notaris rekanan diler.                                 │
├─────────────────────────────────────────────────────────────────────────────┤
│ PRAKONDISI:                                                                 │
│   ✓ Aplikasi Kredit berstatus DOCUMENTED                                   │
│   ✓ Akta fidusia sudah ditandatangani notaris                              │
│   ✓ Portal AHU online normal                                                │
├─────────────────────────────────────────────────────────────────────────────┤
│ ALUR:                                                                       │
│   1. Notaris upload akta fidusia ke SABH                                   │
│   2. Sistem generate tanggal batas pendaftaran (akta + 30 hari)             │
│   3. Sistem generate kode billing PNBP                                     │
│   4. Admin Bayar PNBP via bank/klikbca                                    │
│   5. Sistem submit ke Ditjen AHU                                           │
│   6. Ditjen AHU proses & generate sertifikat                              │
│   7. Sertifikat fidusia elektronik terbit                                  │
│   8. Admin download & simpan sertifikat                                     │
│   9. Update status aplikasi → FIDUSIA_COMPLETE                            │
├─────────────────────────────────────────────────────────────────────────────┤
│ ⚠️ VALIDASI KRITIS:                                                        │
│   ┌─────────────────────────────────────────────────────────────────────┐   │
│   │                                                                     │   │
│   │   SISTEM WAJIB CEK SEBELUM SUBMIT:                                 │   │
│   │                                                                     │   │
│   │   Tanggal Submit - Tanggal Akta ≤ 30 Hari?                        │   │
│   │                                                                     │   │
│   │   ┌──────────────────┐    ┌──────────────────┐                    │   │
│   │   │      ≤ 30 Hari   │    │     > 30 Hari    │                    │   │
│   │   │    ✅ DITERIMA   │    │    ❌ DITOLAK    │                    │   │
│   │   │  Lanjut Proses  │    │ Sistem AHU Tolak │                    │   │
│   │   └──────────────────┘    └──────────────────┘                    │   │
│   │                                                                     │   │
│   │   ⚠️ BATAS 30 HARI ADALAH MUTLAK - TIDAK BISA DIKURANGI!          │   │
│   │                                                                     │   │
│   └─────────────────────────────────────────────────────────────────────┘   │
├─────────────────────────────────────────────────────────────────────────────┤
│ PASKAKONDISI (SUKSES):                                                     │
│   ✓ Sertifikat fidusia berhasil diunduh elektronik                        │
│   ✓ Status kredit berubah menjadi FIDUSIA_COMPLETE                        │
│   ✓ Leasing memiliki hak preferen atas kendaraan                          │
├─────────────────────────────────────────────────────────────────────────────┤
│ NOTIFIKASI REMINDER:                                                        │
│   • Day -30: Email ke Dealer/Leasing                                       │
│   • Day -15: SMS reminder ke Admin                                         │
│   • Day -7: Alert urgent ke Manager                                        │
│   • Day -1: Final warning                                                  │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 5.4 ERD (Entity Relationship Diagram)

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                     ERD: SISTEM TRANSAKSI KENDARAAN                        │
└─────────────────────────────────────────────────────────────────────────────┘

    ┌──────────────────┐         ┌──────────────────┐
    │     Pembeli      │         │    Kendaraan     │
    ├──────────────────┤         ├──────────────────┤
    │ id (PK)          │──┐  ┌──▶│ id (PK)          │
    │ nik               │  │  │   │ nomor_rangka     │
    │ nama_lengkap     │  │  │   │ nomor_mesin     │
    │ tanggal_lahir    │  │  │   │ jenis           │
    │ alamat           │  │  │   │ tipe            │
    │ nomor_hp         │  │  │   │ merk            │
    │ email            │  │  │   │ harga_dasar     │
    │ role             │  │  │   │ njkb            │
    │ created_at       │  │  │   │ status          │
    └──────────────────┘  │  │   └────────┬─────────┘
           │               │  │            │
           │               │  │            │
           │               │  │            │
           │               │  │            │
           ▼               │  │            ▼
    ┌──────────────────┐   │  │   ┌──────────────────┐
    │  AplikasiKredit  │   │  │   │DokumenLegalitas │
    ├──────────────────┤   │  │   ├──────────────────┤
    │ id (PK)          │───┘  │   │ id (PK)          │
    │ pembeli_id (FK)  │──────┘   │ pembeli_id (FK)  │
    │ kendaraan_id(FK) │─────────▶│ kendaraan_id(FK) │
    │ jumlah_pinjaman  │          │ jenis            │
    │ dp_amount        │          │ file_path        │
    │ tenor_bulan      │          │ is_verified      │
    │ bunga_tahunan    │          │ verified_at       │
    │ status           │          └──────────────────┘
    │ notaris_id (FK)  │──┐
    └────────┬─────────┘  │            ┌──────────────────┐
             │              │            │    Notaris       │
             │              │            ├──────────────────┤
             │ 1:1          │            │ id (PK)          │
             │              │            │ nama             │
             ▼              │            │ sip_notaris      │
    ┌──────────────────┐   │            │ is_active        │
    │ SertifikatFidusia │   │            └──────────────────┘
    ├──────────────────┤   │
    │ id (PK)          │───┘
    │ aplikasi_kredit  │───┐
    │ _id (FK)         │   │
    │ notaris_id (FK)  │───┤
    │ nomor_sertifikat │   │          ┌──────────────────┐
    │ nomor_akta       │   │          │   PDI_Report     │
    │ tanggal_akta     │   │          ├──────────────────┤
    │ batas_pendaftaran│   │          │ id (PK)          │
    │ status           │   │          │ kendaraan_id(FK) │
    └──────────────────┘   │          │ teknisi_id       │
                           │          │ dewaxing_passed  │
                           │          │ fuse_installed   │
                           │          │ obd_scan_passed   │
                           │          │ gps_distance_km   │
                           │          │ uji_jalan_passed │
                           │          │ overall_passed   │
                           │          └──────────────────┘
                           │
                           │ 1:1 (sebelum pencairan)
                           │
                           └─────────────────────────────▶

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📋 PENJELASAN RELASI
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

╔═══════════════════╦════════════╦═══════════════════════════════════════╗
║  RELASI           ║  TIPE      ║  PENJELASAN                          ║
╠═══════════════════╬════════════╬═══════════════════════════════════════╣
║                   ║            ║                                       ║
║  Pembeli →       ║   1:N      ║  1 pembeli bisa punya banyak          ║
║  Kendaraan       ║            ║  kendaraan                            ║
║                   ║            ║                                       ║
╠═══════════════════╬════════════╬═══════════════════════════════════════╣
║                   ║            ║                                       ║
║  Kendaraan →     ║   1:N      ║  1 kendaraan bisa punya banyak         ║
║  Dokumen         ║            ║  dokumen legalitas                     ║
║                   ║            ║                                       ║
╠═══════════════════╬════════════╬═══════════════════════════════════════╣
║                   ║            ║                                       ║
║  AplikasiKredit → ║   N:1      ║  Banyak pengajuan ditangani           ║
║  Notaris         ║            ║  oleh 1 notaris                       ║
║                   ║            ║                                       ║
╠═══════════════════╬════════════╬═══════════════════════════════════════╣
║                   ║            ║                                       ║
║  AplikasiKredit → ║   1:1      ║  1 pengajuan = 1 sertifikat fidusia   ║
║  Fidusia         ║            ║                                       ║
║                   ║            ║                                       ║
╠═══════════════════╬════════════╬═══════════════════════════════════════╣
║                   ║            ║                                       ║
║  Kendaraan →      ║   1:1      ║  1 kendaraan harus punya 1 laporan   ║
║  PDI              ║            ║  PDI sebelum serah terima            ║
║                   ║            ║                                       ║
╠═══════════════════╬════════════╬═══════════════════════════════════════╣
║                   ║            ║                                       ║
║  Fidusia →        ║   1:1      ║  Fidusia baru bisa setelah PDI       ║
║  PDI              ║            ║  selesai (sebelum pencairan)          ║
║                   ║            ║                                       ║
╚═══════════════════╩════════════╩═══════════════════════════════════════╝

⚠️ CATATAN PENTING:
   • Seluruh entitas menggunakan SOFT DELETE (deleted_at)
   • Tidak ada penghapusan permanen
   • Bertujuan untuk audit trail & ketertelusuran
```

---

## 5.5 Kesimpulan

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                              KESIMPULAN                                     │
└─────────────────────────────────────────────────────────────────────────────┘

╔═════════════════════════════════════════════════════════════════════════════╗
║                                                                             ║
║  1. EFIISIENSI                                                             ║
║  ─────────────────────────────────────────────────────────────────────     ║
║     Proses yang berhari-hari menjadi satu alur terintegrasi real-time       ║
║                                                                             ║
║     DAHULU:                                                                 ║
║     ┌────────┐   ┌────────┐   ┌────────┐   ┌────────┐   ┌────────┐        ║
║     │Hitung  │──▶│Verif   │──▶│Kredit  │──▶│Fidusia │──▶│  PDI   │        ║
║     │Pajak   │   │Dokumen │   │Manual  │   │30 Hari │   │Manual  │        ║
║     │2-3 Hari│   │1-2 Hari│   │3-5 Hari│   │Risiko! │   │1 Hari  │        ║
║     └────────┘   └────────┘   └────────┘   └────────┘   └────────┘        ║
║     Total: 7-11 HARI                                                        ║
║                                                                             ║
║     SEKARANG:                                                               ║
║     ┌─────────────────────────────────────────────────────┐               ║
║     │            ONE INTEGRATED REAL-TIME FLOW             │               ║
║     │  Estimasi → Inspeksi → Kredit → Fidusia → PDI       │               ║
║     │       │          │         │         │         │      │               ║
║     │   < 1 Jam    < 2 Jam   < 1 Jam   < 1 Jam   < 2 Jam  │               ║
║     └─────────────────────────────────────────────────────┘               ║
║     Total: < 8 JAM                                                          ║
║                                                                             ║
╠═════════════════════════════════════════════════════════════════════════════╣
║                                                                             ║
║  2. KEPATUHAN REGULASI OTOMATIS                                            ║
║  ─────────────────────────────────────────────────────────────────────     ║
║     Sistem menegakkan aturan hukum tanpa mengandalkan manual staf           ║
║                                                                             ║
║     ✅ Fidusia 30 hari → auto-reject jika terlambat                         ║
║     ✅ Dokumen wajib → validasi checklist otomatis                          ║
║     ✅ Formula pajak → hitung sesuai UU HKPD                               ║
║     ✅ Face matching → ≥85% via API Dukcapil                               ║
║     ✅ Hologram BPKB → deteksi ≥95% via ML                                  ║
║                                                                             ║
╠═════════════════════════════════════════════════════════════════════════════╣
║                                                                             ║
║  3. KEAMANAN DATA                                                          ║
║  ─────────────────────────────────────────────────────────────────────     ║
║     ✅ Enkripsi AES-256 (data tersimpan)                                   ║
║     ✅ TLS 1.3 (data berjalan)                                              ║
║     ✅ MFA untuk Admin & Surveyor                                           ║
║     ✅ TTE bersertifikat Kominfo (Privy/VIDA)                              ║
║                                                                             ║
╠═════════════════════════════════════════════════════════════════════════════╣
║                                                                             ║
║  4. TRANSPARANSI                                                           ║
║  ─────────────────────────────────────────────────────────────────────     ║
║     Konsumen melihat rincian OTR lengkap sebelum membayar                  ║
║                                                                             ║
║     ┌──────────────────────────────────────────────────────────┐           ║
║     │                    RINCIAN OTR                           │           ║
║     │  ┌────────────────┐  ┌────────────────┐                 │           ║
║     │  │ Harga Mobil    │  │ PPN 11%        │                 │           ║
║     │  │ RpXXX.XXX.XXX  │  │ RpXX.XXX.XXX   │                 │           ║
║     │  ├────────────────┤  ├────────────────┤                 │           ║
║     │  │ PKB + Opsen    │  │ BBNKB + Opsen  │                 │           ║
║     │  │ RpXX.XXX.XXX   │  │ RpXX.XXX.XXX   │                 │           ║
║     │  ├────────────────┤  ├────────────────┤                 │           ║
║     │  │ SWDKLLJ        │  │ Admin & Plat   │                 │           ║
║     │  │ RpXXX.XXX      │  │ RpXXX.XXX      │                 │           ║
║     │  ├────────────────┴──┴────────────────┴─────────────────┤           ║
║     │  │                  TOTAL OTR                          │           ║
║     │  │                  RpXXX.XXX.XXX                       │           ║
║     │  └────────────────────────────────────────────────────┘           ║
║     └──────────────────────────────────────────────────────────┘           ║
║                                                                             ║
╚═════════════════════════════════════════════════════════════════════════════╝
```

---

## 5.6 Terima Kasih

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                                                                             │
│                                                                             │
│                                                                             │
│                              TERIMA KASIH                                   │
│                                                                             │
│                                                                             │
│                     Kelompok 4 - Universitas Tidar                          │
│                                                                             │
│                                                                             │
│                   "Sistem Transaksi Pembelian Kendaraan                     │
│                    Bermotor Terintegrasi Indonesia"                         │
│                                                                             │
│                                                                             │
│                                                                             │
│                                                                             │
│                              JUNE 2026                                      │
│                                                                             │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

*Dokumen ini adalah rangkuman dari SRS dan BusinessLogic-MermaidJS.md*
*Versi: 2.0 | Last Updated: June 2026*
