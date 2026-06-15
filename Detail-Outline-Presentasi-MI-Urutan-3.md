# DETAIL OUTLINE URUTAN 3 - FLOWCHART LOGIKA BISNIS (Bagian 1)

> **Referensi:** SRS Section 3.4 (Pricing Engine), Section 3.5 (Fintech Leasing), Section 3.4 (Validasi Dokumen)
> **Referensi Flowchart:** BusinessLogic-MermaidJS.md - Sections 1, 2, 3

---

# BAGIAN 3.1: MODUL KALKULASI PAJAK - PRICING ENGINE

> **Lokasi di SRS:** Sub-bab 3.4
> **Lokasi Flowchart:** BusinessLogic-MermaidJS.md → Section 1: Kalkulasi Pajak

---

## 3.1.1 GLOSARIUM ISTILAH PAJAK KENDARAAN

```
╔══════════════════════════════════════════════════════════════════════════════╗
║                        GLOSARIUM ISTILAH PAJAK KENDARAAN                     ║
╠══════════════════════════════════════════════════════════════════════════════╣
║                                                                              ║
║  1. NJKB (Nilai Jual Kendaraan Bermotor)                                   ║
║  ───────────────────────────────────────────                                 ║
║     DEFINISI:                                                                ║
║     Nilai yang ditetapkan oleh pemerintah sebagai dasar pengenaan pajak      ║
║     kendaraan. NJKB BUKAN harga jual di dealer, melainkan nilai yang        ║
║     tercatat di sistem Samsat berdasarkan lembar STNK.                      ║
║                                                                              ║
║     SUMBER DATA:                                                             ║
║     • Lembar STNK (kolom NJKB)                                             ║
║     • API Samsat/SIGNAL                                                     ║
║                                                                              ║
║     RUMUS CARI NJKB DARI STNK:                                             ║
║     ┌──────────────────────────────────────────────────────────────────┐    ║
║     │  NJKB = PKB Pokok / Tarif Berlaku                                   │    ║
║     └──────────────────────────────────────────────────────────────────┘    ║
║                                                                              ║
╠══════════════════════════════════════════════════════════════════════════════╣
║                                                                              ║
║  2. DPP (Dasar Pengenaan Pajak)                                            ║
║  ─────────────────────────────────                                         ║
║     DEFINISI:                                                                ║
║     Nilai yang menjadi dasar perhitungan pajak. Untuk kendaraan bermotor,   ║
║     DPP menggunakan formula "Nilai Lain" yang ditetapkan pemerintah.        ║
║                                                                              ║
║     RUMUS:                                                                   ║
║     ┌──────────────────────────────────────────────────────────────────┐    ║
║     │  DPP PPN = (11/12) × NJKB                                         │    ║
║     └──────────────────────────────────────────────────────────────────┘    ║
║                                                                              ║
║     KENAPA 11/12?                                                          ║
║     Ini adalah aturan pemerintah agar taxable value = 11% × NJKB,           ║
║     sehingga PPN = 12% × DPP = 12% × (11/12 × NJKB) = 11% × NJKB          ║
║                                                                              ║
╠══════════════════════════════════════════════════════════════════════════════╣
║                                                                              ║
║  3. PPN (Pajak Pertambahan Nilai)                                          ║
║  ─────────────────────────────────                                         ║
║     DEFINISI:                                                                ║
║     Pajak yang dikenakan atas penjualan barang/jasa. Untuk kendaraan,        ║
║     PPN dihitung dari DPP dengan tarif 12% (sesuai PMK 131/2024).          ║
║                                                                              ║
║     RUMUS:                                                                   ║
║     ┌──────────────────────────────────────────────────────────────────┐    ║
║     │  PPN = 12% × DPP PPN = 12% × (11/12 × NJKB) = 11% × NJKB       │    ║
║     └──────────────────────────────────────────────────────────────────┘    ║
║                                                                              ║
║     PERUBAHAN TARIF (PMK 131/2024):                                        ║
║     • Sebelum Juli 2024: 11%                                               ║
║     • Mulai Juli 2024: 12%                                                 ║
║                                                                              ║
╠══════════════════════════════════════════════════════════════════════════════╣
║                                                                              ║
║  4. PKB (Pajak Kendaraan Bermotor)                                         ║
║  ─────────────────────────────────                                         ║
║     DEFINISI:                                                                ║
║     Pajak daerah yang dikenakan atas kepemilikan kendaraan bermotor.        ║
║     PKB dihitung berdasarkan NJKB dan bobot kendaraan.                      ║
║                                                                              ║
║     RUMUS:                                                                   ║
║     ┌──────────────────────────────────────────────────────────────────┐    ║
║     │  PKB Pokok = Tarif × NJKB                                         │    ║
║     │  dimana Tarif = 1.5% - 2% (tergantung bobot kendaraan)           │    ║
║     └──────────────────────────────────────────────────────────────────┘    ║
║                                                                              ║
║     CONTOH TARIF:                                                           ║
║     • Golongan I (sedan, SUV): 1.5%                                       ║
║     • Golongan II (pickup, dll): 1.75%                                    ║
║     • Golongan III (truck, dll): 2%                                       ║
║                                                                              ║
╠══════════════════════════════════════════════════════════════════════════════╣
║                                                                              ║
║  5. BBNKB (Bea Balik Nama Kendaraan Bermotor)                             ║
║  ─────────────────────────────────────────────                              ║
║     DEFINISI:                                                                ║
║     Pajak yang dikenakan saat terjadi perpindahan kepemilikan kendaraan.     ║
║                                                                              ║
║     RUMUS:                                                                   ║
║     ┌──────────────────────────────────────────────────────────────────┐    ║
║     │  BBNKB = Tarif × NJKB                                             │    ║
║     │  dimana Tarif = 12% (provinsi lain)                              │    ║
║     │                 12.5% (DKI Jakarta - Perda No.1/2024)           │    ║
║     └──────────────────────────────────────────────────────────────────┘    ║
║                                                                              ║
╠══════════════════════════════════════════════════════════════════════════════╣
║                                                                              ║
║  6. Opsen (Pajak Opsional)                                                ║
║  ─────────────────────────────                                             ║
║     DEFINISI:                                                               ║
║     Pungutan tambahan yang dihitung sebagai persentase dari pajak pokok.    ║
║     Opsen merupakan bagian dari UU HKPD (Hulu Kapal dan Pengeloala        ║
║     Dana).                                                                  ║
║                                                                              ║
║     RUMUS:                                                                   ║
║     ┌──────────────────────────────────────────────────────────────────┐    ║
║     │  Opsen PKB = 66% × PKB Pokok                                       │    ║
║     │  Opsen BBNKB = 66% × BBNKB Pokok                                  │    ║
║     └──────────────────────────────────────────────────────────────────┘    ║
║                                                                              ║
║     ⚠️ CATATAN KHUSUS:                                                     ║
║     DKI Jakarta TIDAK memiliki Opsen karena tidak memiliki pemerintahan     ║
║     kabupaten/kota otonom. Untuk DKI Jakarta:                               ║
║     • Opsen PKB = Rp0                                                     ║
║     • Opsen BBNKB = Rp0                                                   ║
║                                                                              ║
╠══════════════════════════════════════════════════════════════════════════════╣
║                                                                              ║
║  7. SWDKLLJ (Sumbangan Wajib Dana Kecelakaan Lalu Lintas Jalan)          ║
║  ────────────────────────────────────────────────────────────               ║
║     DEFINISI:                                                                ║
║     Iuran wajib yang dikelola oleh Jasa Raharja untuk memberikan           ║
║     perlindungan kecelakaan lalu lintas.                                     ║
║                                                                              ║
║     BESARAN:                                                                ║
║     ┌──────────────────────────────────────────────────────────────────┐    ║
║     │  Gol. I (≤1000cc): Rp35.000 - Rp43.000                          │    ║
║     │  Gol. II (1001-1500cc): Rp50.000 - Rp63.000                     │    ║
║     │  Gol. III (1501-2500cc): Rp62.000 - Rp83.000                    │    ║
║     │  Gol. IV (>2500cc): Rp82.000 - Rp106.000                        │    ║
║     └──────────────────────────────────────────────────────────────────┘    ║
║                                                                              ║
╠══════════════════════════════════════════════════════════════════════════════╣
║                                                                              ║
║  8. PNBP (Penerimaan Negara Bukan Pajak)                                  ║
║  ─────────────────────────────────────────────                              ║
║     DEFINISI:                                                                ║
║     Biaya administrasi yang dikenakan pemerintah saat registrasi            ║
║     kendaraan.                                                               ║
║                                                                              ║
║     KOMPONEN PNBP:                                                         ║
║     ┌──────────────────────────────────────────────────────────────────┐    ║
║     │  STNK 5 Tahun (perpanjangan): Rp200.000                          │    ║
║     │  TNKB (Plat Nomor): Rp100.000                                    │    ║
║     │  BPKB (Buku): Rp375.000                                          │    ║
║     │  SWDKLLJ: Rp35.000 - Rp150.000 (tergantung CC)                  │    ║
║     └──────────────────────────────────────────────────────────────────┘    ║
║                                                                              ║
╠══════════════════════════════════════════════════════════════════════════════╣
║                                                                              ║
║  9. TKDN (Tingkat Komponen Dalam Negeri)                                   ║
║  ─────────────────────────────────────────                                 ║
║     DEFINISI:                                                                ║
║     Persentase komponen lokal dalam kendaraan. TKDN menentukan              ║
║     kelayakan mendapat insentif PPN DTP untuk kendaraan listrik.           ║
║                                                                              ║
║     SYARAT INSENTIF KBLBB:                                                 ║
║     ┌──────────────────────────────────────────────────────────────────┐    ║
║     │  TKDN ≥ 40% → Eligible PPN DTP 10% (bayar PPN efektif 2%)      │    ║
║     │  TKDN < 40% → Tidak eligible insentif                           │    ║
║     └──────────────────────────────────────────────────────────────────┘    ║
║                                                                              ║
╠══════════════════════════════════════════════════════════════════════════════╣
║                                                                              ║
║  10. KBLBB (Kendaraan Bermotor Listrik Berbasis Baterai)                  ║
║  ─────────────────────────────────────────────────────────                   ║
║     DEFINISI:                                                                ║
║     Kendaraan yang menggunakan tenaga listrik dari baterai sebagai          ║
║     sumber energi utama (bukan hybrid).                                     ║
║                                                                              ║
║     CONTOH: Tesla, Hyundai Ioniq Electric, Wuling Air EV, dll               ║
║                                                                              ║
╠══════════════════════════════════════════════════════════════════════════════╣
║                                                                              ║
║  11. PPN DTP (PPN Ditanggung Pemerintah)                                  ║
║  ─────────────────────────────────────────────                             ║
║     DEFINISI:                                                                ║
║     Subsidi PPN dari pemerintah untuk kendaraan listrik dengan TKDN≥40%.    ║
║                                                                              ║
║     RUMUS:                                                                   ║
║     ┌──────────────────────────────────────────────────────────────────┐    ║
║     │  PPN Normal = 12% × DPP                                          │    ║
║     │  PPN DTP = 10% × DPP                                             │    ║
║     │  PPN Efektif = PPN Normal - PPN DTP = 2% × DPP                   │    ║
║     └──────────────────────────────────────────────────────────────────┘    ║
║                                                                              ║
╚══════════════════════════════════════════════════════════════════════════════╝
```

---

## 3.1.2 INPUT VARIABEL KALKULASI PAJAK

```
╔══════════════════════════════════════════════════════════════════════════════╗
║                        INPUT VARIABEL KALKULASI PAJAK                        ║
╠══════════════════════════════════════════════════════════════════════════════╣
║                                                                              ║
║  GRUP A: DATA KENDARAAN                                                     ║
║  ═════════════════════════════════════════                                  ║
║                                                                              ║
║  ┌─────────────────┬──────────────────┬─────────────────────────────────┐  ║
║  │ Variabel       │ Tipe Data        │ Deskripsi                       │  ║
║  ├─────────────────┼──────────────────┼─────────────────────────────────┤  ║
║  │ jenis_kendaraan │ ENUM             │ BARU atau BEKAS                 │  ║
║  │ tipe_kendaraan  │ ENUM             │ SEDAN, SUV, MPV, PICKUP, dll    │  ║
║  │ merk            │ VARCHAR(100)      │ Toyota, Honda, dll              │  ║
║  │ model           │ VARCHAR(100)      │ Avanza, CR-V, dll              │  ║
║  │ tahun           │ INTEGER           │ Tahun produksi                  │  ║
║  │ nomor_rangka    │ VARCHAR(17)      │ 17 digit VIN                    │  ║
║  │ cc_mesin        │ INTEGER           │ Kapasitas silinder (1500, 2000) │  ║
║  │ tkdn_persen     │ DECIMAL(5,2)     │ Persentase TKDN (0.00 - 100.00) │  ║
║  │ is_kblbb        │ BOOLEAN           │ Apakah kendaraan listrik?        │  ║
║  └─────────────────┴──────────────────┴─────────────────────────────────┘  ║
║                                                                              ║
╠══════════════════════════════════════════════════════════════════════════════╣
║                                                                              ║
║  GRUP B: DATA PAJAK                                                         ║
║  ═════════════════════════════════════════                                  ║
║                                                                              ║
║  ┌─────────────────┬──────────────────┬─────────────────────────────────┐  ║
║  │ Variabel       │ Tipe Data        │ Deskripsi                       │  ║
║  ├─────────────────┼──────────────────┼─────────────────────────────────┤  ║
║  │ njkb           │ DECIMAL(15,2)    │ Nilai Jual KB dalam Rupiah       │  ║
║  │ tarif_pkb      │ DECIMAL(5,4)     │ Tarif PKB (0.015 - 0.02)        │  ║
║  │ tarif_bbnkb    │ DECIMAL(5,4)     │ Tarif BBNKB (0.12 atau 0.125)   │  ║
║  │ koefisian_road  │ DECIMAL(3,2)     │ Bobot kerusakan jalan (1.00-2.00)│ ║
║  │ golongan_kb     │ ENUM             │ Golongan I, II, III, IV         │  ║
║  └─────────────────┴──────────────────┴─────────────────────────────────┘  ║
║                                                                              ║
╠══════════════════════════════════════════════════════════════════════════════╣
║                                                                              ║
║  GRUP C: DATA WILAYAH                                                       ║
║  ═════════════════════════════════════════                                  ║
║                                                                              ║
║  ┌─────────────────┬──────────────────┬─────────────────────────────────┐  ║
║  │ Variabel       │ Tipe Data        │ Deskripsi                       │  ║
║  ├─────────────────┼──────────────────┼─────────────────────────────────┤  ║
║  │ kode_provinsi  │ VARCHAR(2)        │ Kode provinsi (01=DKI, dll)     │  ║
║  │ nama_provinsi  │ VARCHAR(100)      │ Nama provinsi                   │  ║
║  │ is_dki         │ BOOLEAN          │ Apakah wilayah DKI Jakarta?     │  ║
║  │ kode_penerima   │ VARCHAR(4)        │ Kode daerah Samsat              │  ║
║  └─────────────────┴──────────────────┴─────────────────────────────────┘  ║
║                                                                              ║
╠══════════════════════════════════════════════════════════════════════════════╣
║                                                                              ║
║  GRUP D: DATA PEMILIK                                                       ║
║  ═════════════════════════════════════════                                  ║
║                                                                              ║
║  ┌─────────────────┬──────────────────┬─────────────────────────────────┐  ║
║  │ Variabel       │ Tipe Data        │ Deskripsi                       │  ║
║  ├─────────────────┼──────────────────┼─────────────────────────────────┤  ║
║  │ nik             │ VARCHAR(16)       │ NIK 16 digit pembeli           │  ║
║  │ alamat          │ TEXT              │ Alamat sesuai KK                │  ║
║  │ jumlah_kendaraan│ INTEGER           │ Jumlah kendaraan yang dimiliki   │  ║
║  │                 │                  │ (untuk pajak progresif)         │  ║
║  │ is_perusahaan   │ BOOLEAN          │ Apakah pembeli badan hukum?      │  ║
║  └─────────────────┴──────────────────┴─────────────────────────────────┘  ║
║                                                                              ║
╚══════════════════════════════════════════════════════════════════════════════╝
```

---

## 3.1.3 FLOWCHART DETAIL KALKULASI PAJAK

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                   FLOWCHART KALKULASI PAJAK LENGKAP (VERSI DETAIL)           │
└─────────────────────────────────────────────────────────────────────────────┘

═══════════════════════════════════════════════════════════════════════════════
STEP 1: VALIDASI INPUT
═══════════════════════════════════════════════════════════════════════════════

    ┌─────────────────────────────────────────────────────────────────────┐
    │  START: Inisiasi Kalkulasi Pajak                                   │
    └───────────────────────────────┬─────────────────────────────────────┘
                                    │
                                    ▼
    ┌─────────────────────────────────────────────────────────────────────┐
    │  INPUT_001: Tipe Kendaraan                                         │
    │  ┌───────────────────────────────────────────────────────────────┐  │
    │  │  jenis_kendaraan = "BARU" atau "BEKAS"                       │  │
    │  └───────────────────────────────────────────────────────────────┘  │
    └───────────────────────────────┬─────────────────────────────────────┘
                                    │
                                    ▼
    ┌─────────────────────────────────────────────────────────────────────┐
    │  DECISION_001: Apakah Kendaraan Bermotor Baru?                      │
    │  ┌───────────────────────────────────────────────────────────────┐  │
    │  │  IF jenis_kendaraan == "BARU" THEN                           │  │
    │  │     → PROCEED TO: STEP_2A (Verifikasi TKDN)                  │  │
    │  │  ELSE                                                         │  │
    │  │     → PROCEED TO: STEP_2B (Ambil NJKB dari STNK)             │  │
    │  └───────────────────────────────────────────────────────────────┘  │
    └───────────────────────────────┬─────────────────────────────────────┘
                                    │
              ┌─────────────────────┴─────────────────────┐
              │                                           │
              ▼                                           ▼
              
═══════════════════════════════════════════════════════════════════════════════
STEP 2A: VERIFIKASI TKDN (UNTUK KENDARAAN BARU)
═══════════════════════════════════════════════════════════════════════════════

    ┌─────────────────────────────────────────────────────────────────────┐
    │  STEP_2A: Verifikasi TKDN & Kelayakan KBLBB                       │
    │  ┌───────────────────────────────────────────────────────────────┐  │
    │  │  // Ambil data TKDN dari database/reference                   │  │
    │  │  tkdn_persen = GET_TKDN(nomor_rangka)                        │  │
    │  │                                                             │  │
    │  │  // Cek apakah kendaraan listrik                             │  │
    │  │  is_kblbb = CHECK_KBLBB(tipe_kendaraan)                     │  │
    │  └───────────────────────────────────────────────────────────────┘  │
    └───────────────────────────────┬─────────────────────────────────────┘
                                    │
                                    ▼
    ┌─────────────────────────────────────────────────────────────────────┐
    │  DECISION_002: TKDN ≥ 40%?                                        │
    │  ┌───────────────────────────────────────────────────────────────┐  │
    │  │  IF tkdn_persen >= 40 AND is_kblbb == TRUE THEN             │  │
    │  │     eligible_kblbb = TRUE                                     │  │
    │  │     → PROCEED TO: CALC_PPN_WITH_DTP                          │  │
    │  │  ELSE                                                         │  │
    │  │     eligible_kblbb = FALSE                                    │  │
    │  │     → PROCEED TO: CALC_PPN_NORMAL                            │  │
    │  └───────────────────────────────────────────────────────────────┘  │
    └───────────────────────────────┬─────────────────────────────────────┘
                                    │
              ┌─────────────────────┴─────────────────────┐
              │                                           │
              ▼                                           ▼

═══════════════════════════════════════════════════════════════════════════════
STEP 2B: AMBIL NJKB DARI STNK (UNTUK KENDARAAN BEKAS)
═══════════════════════════════════════════════════════════════════════════════

    ┌─────────────────────────────────────────────────────────────────────┐
    │  STEP_2B: Ambil NJKB dari Data STNK                               │
    │  ┌───────────────────────────────────────────────────────────────┐  │
    │  │  // Query API Samsat/SIGNAL untuk data STNK                   │  │
    │  │  api_response = CALL_SAMSAT_API(nomor_polisi, nik,           │  │
    │  │                              5_digit_nomor_rangka)            │  │
    │  │                                                             │  │
    │  │  // Ekstrak data dari response                               │  │
    │  │  pkb_pokok_stnk = api_response.pkb_pokok                    │  │
    │  │  tarif_berlaku = api_response.tarif_pkb                      │  │
    │  │                                                             │  │
    │  │  // Hitung NJKB dari data STNK                               │  │
    │  │  njkb = pkb_pokok_stnk / tarif_berlaku                      │  │
    │  └───────────────────────────────────────────────────────────────┘  │
    └───────────────────────────────┬─────────────────────────────────────┘
                                    │
                                    ▼

═══════════════════════════════════════════════════════════════════════════════
STEP 3: HITUNG DPP PAJAK
═══════════════════════════════════════════════════════════════════════════════

    ┌─────────────────────────────────────────────────────────────────────┐
    │  STEP_3: Hitung DPP PPN                                            │
    │  ┌───────────────────────────────────────────────────────────────┐  │
    │  │                                                             │  │
    │  │  VARIABEL:                                                  │  │
    │  │  ┌─────────────────────────────────────────────────────┐   │  │
    │  │  │  INPUT:                                                  │   │  │
    │  │  │  • njkb = Nilai Jual KB (dari STEP_2A atau 2B)       │   │  │
    │  │  │                                                          │   │  │
    │  │  │  KONSTANTA:                                             │   │  │
    │  │  │  • FAKTOR_DPP = 11/12 = 0.9166666667                   │   │  │
    │  │  │                                                          │   │  │
    │  │  │  OUTPUT:                                                 │   │  │
    │  │  │  • dpp_ppn = njkb × FAKTOR_DPP                         │   │  │
    │  │  └─────────────────────────────────────────────────────┘   │  │
    │  │                                                             │  │
    │  │  PSEUDOCODE:                                                │  │
    │  │  ┌─────────────────────────────────────────────────────┐   │  │
    │  │  │  dpp_ppn = njkb * (11 / 12)                          │   │  │
    │  │  │  // Atau equivalently:                                 │   │  │
    │  │  │  dpp_ppn = njkb * 0.9166666667                        │   │  │
    │  │  │  ROUND(dpp_ppn, 2)  // Presisi 2 desimal            │   │  │
    │  │  └─────────────────────────────────────────────────────┘   │  │
    │  │                                                             │  │
    │  └───────────────────────────────────────────────────────────────┘  │
    └───────────────────────────────┬─────────────────────────────────────┘
                                    │
                                    ▼

═══════════════════════════════════════════════════════════════════════════════
STEP 4: HITUNG PPN (DENGAN / TANPA PPN DTP)
═══════════════════════════════════════════════════════════════════════════════

    ┌─────────────────────────────────────────────────────────────────────┐
    │  DECISION_003: Eligible KBLBB (PPN DTP)?                           │
    │  ┌───────────────────────────────────────────────────────────────┐  │
    │  │  IF eligible_kblbb == TRUE THEN                             │  │
    │  │     → PROCEED TO: CALC_PPN_WITH_DTP                         │  │
    │  │  ELSE                                                         │  │
    │  │     → PROCEED TO: CALC_PPN_NORMAL                            │  │
    │  └───────────────────────────────────────────────────────────────┘  │
    └───────────────────────────────┬─────────────────────────────────────┘
                                    │
              ┌─────────────────────┴─────────────────────┐
              │                                           │
              ▼                                           ▼
              
    ┌─────────────────────────────┐     ┌─────────────────────────────┐
    │  CALC_PPN_WITH_DTP:         │     │  CALC_PPN_NORMAL:          │
    │  ┌───────────────────────┐   │     │  ┌───────────────────────┐ │
    │  │                       │   │     │  │                       │ │
    │  │ VARIABEL:             │   │     │  │ VARIABEL:             │ │
    │  │ INPUT:                │   │     │  │ INPUT:                │ │
    │  │ • dpp_ppn             │   │     │  │ • dpp_ppn             │ │
    │  │                       │   │     │  │                       │ │
    │  │ KONSTANTA:           │   │     │  │ KONSTANTA:           │ │
    │  │ • TARIF_PPN = 12%    │   │     │  │ • TARIF_PPN = 12%    │ │
    │  │ • TARIF_DTP = 10%     │   │     │  │                       │ │
    │  │                       │   │     │  │ KALKULASI:           │ │
    │  │ KALKULASI:           │   │     │  │ • ppn_normal =       │ │
    │  │ • ppn_normal =        │   │     │  │     dpp_ppn × 12%    │ │
    │  │     dpp_ppn × 12%    │   │     │  │ • ppn_dtp = 0        │ │
    │  │ • ppn_dtp =           │   │     │  │ • ppn_terutang =     │ │
    │  │     dpp_ppn × 10%    │   │     │  │     ppn_normal       │ │
    │  │ • ppn_terutang =     │   │     │  │                       │ │
    │  │     ppn_normal -      │   │     │  │                       │ │
    │  │     ppn_dtp           │   │     │  │                       │ │
    │  │                     │   │     │  │                       │ │
    │  │ HASIL:               │   │     │  │ HASIL:               │ │
    │  │ • ppn_terutang =     │   │     │  │ • ppn_terutang =     │ │
    │  │     2% × dpp_ppn     │   │     │  │     11% × njkb       │ │
    │  │                     │   │     │  │                       │ │
    │  │ CONTOH:              │   │     │  │ CONTOH:              │ │
    │  │ NJKB = 500.000.000  │   │     │  │ NJKB = 150.000.000   │ │
    │  │ DPP = 458.333.333   │   │     │  │ DPP = 137.500.000    │ │
    │  │ PPN Normal = 55.000.000│ │     │  │ PPN = 16.500.000    │ │
    │  │ PPN DTP = 45.833.333│   │     │  │                       │ │
    │  │ Bayar = 9.166.667   │   │     │  │                       │ │
    │  └───────────────────────┘   │     │  └───────────────────────┘ │
    └─────────────────────────────┘     └─────────────────────────────┘
              │                                           │
              └─────────────────────┬─────────────────────┘
                                    │
                                    ▼

═══════════════════════════════════════════════════════════════════════════════
STEP 5: HITUNG PKB & OPSEN PKB
═══════════════════════════════════════════════════════════════════════════════

    ┌─────────────────────────────────────────────────────────────────────┐
    │  STEP_5: Hitung PKB Pokok                                          │
    │  ┌───────────────────────────────────────────────────────────────┐  │
    │  │                                                             │  │
    │  │  VARIABEL:                                                  │  │
    │  │  ┌─────────────────────────────────────────────────────┐   │  │
    │  │  │  INPUT:                                                  │   │  │
    │  │  │  • njkb = Nilai Jual KB                               │   │  │
    │  │  │  • golongan_kb = Golongan kendaraan (I, II, III, IV)  │   │  │
    │  │  │  • koefisian_road = Bobot kerusakan jalan            │   │  │
    │  │  │                                                          │   │  │
    │  │  │  KONSTANTA TARIF PKB:                                  │   │  │
    │  │  │  • Golongan I (sedan, SUV): 1.5%                      │   │  │
    │  │  │  • Golongan II (pickup): 1.75%                        │   │  │
    │  │  │  • Golongan III (truck ringan): 1.85%                 │   │  │
    │  │  │  • Golongan IV (truck): 2.0%                          │   │  │
    │  │  │                                                          │   │  │
    │  │  │  FORMULA:                                               │   │  │
    │  │  │  ┌─────────────────────────────────────────────────┐   │  │  │
    │  │  │  │  pkb_pokok = njkb × tarif_pkb × koefisian_road │   │  │  │
    │  │  │  └─────────────────────────────────────────────────┘   │   │  │
    │  │  │                                                          │   │  │
    │  │  │  CONTOH:                                                │   │  │
    │  │  │  NJKB = 150.000.000, Gol I, Koef = 1.00              │   │  │
    │  │  │  tarif = 1.5% = 0.015                                 │   │  │
    │  │  │  pkb_pokok = 150.000.000 × 0.015 × 1.00             │   │  │
    │  │  │           = 2.250.000                                  │   │  │
    │  │  └─────────────────────────────────────────────────────┘   │  │
    │  │                                                             │  │
    │  └───────────────────────────────────────────────────────────────┘  │
    └───────────────────────────────┬─────────────────────────────────────┘
                                    │
                                    ▼
    ┌─────────────────────────────────────────────────────────────────────┐
    │  STEP_5B: Hitung Opsen PKB                                        │
    │  ┌───────────────────────────────────────────────────────────────┐  │
    │  │                                                             │  │
    │  │  DECISION: Apakah wilayah DKI Jakarta?                       │  │
    │  │  ┌─────────────────────────────────────────────────────┐   │  │
    │  │  │  IF is_dki == TRUE THEN                            │   │  │
    │  │  │     opsen_pkb = 0                                    │   │  │
    │  │  │     → PROCEED TO: STEP_6                           │   │  │
    │  │  │  ELSE                                                │   │  │
    │  │  │     // Hitung opsen                                  │   │  │
    │  │  │     ┌─────────────────────────────────────────────┐ │   │  │
    │  │  │     │  KONSTANTA:                                 │ │   │  │
    │  │  │     │  • TARIF_OPSEN = 66% = 0.66                 │ │   │  │
    │  │  │     │                                              │ │   │  │
    │  │  │     │  FORMULA:                                   │ │   │  │
    │  │  │     │  ┌─────────────────────────────────────────┐ │ │   │  │
    │  │  │     │  │  opsen_pkb = pkb_pokok × 0.66          │ │ │   │  │
    │  │  │     │  └─────────────────────────────────────────┘ │ │   │  │
    │  │  │     │                                              │ │   │  │
    │  │  │     │  CONTOH:                                     │ │   │  │
    │  │  │     │  pkb_pokok = 2.250.000                      │ │   │  │
    │  │  │     │  opsen_pkb = 2.250.000 × 0.66 = 1.485.000   │ │   │  │
    │  │  │     └─────────────────────────────────────────────┘ │   │  │
    │  │  │  → PROCEED TO: STEP_6                             │   │  │
    │  │  └─────────────────────────────────────────────────────┘   │  │
    │  └───────────────────────────────────────────────────────────────┘  │
    └───────────────────────────────┬─────────────────────────────────────┘
                                    │
                                    ▼

═══════════════════════════════════════════════════════════════════════════════
STEP 6: HITUNG BBNKB & OPSEN BBNKB
═══════════════════════════════════════════════════════════════════════════════

    ┌─────────────────────────────────────────────────────────────────────┐
    │  STEP_6: Hitung BBNKB Pokok                                       │
    │  ┌───────────────────────────────────────────────────────────────┐  │
    │  │                                                             │  │
    │  │  VARIABEL:                                                  │  │
    │  │  ┌─────────────────────────────────────────────────────┐   │  │
    │  │  │  INPUT:                                                  │   │  │
    │  │  │  • njkb = Nilai Jual KB                               │   │  │
    │  │  │  • is_dki = Apakah wilayah DKI?                        │   │  │
    │  │  │                                                          │   │  │
    │  │  │  KONSTANTA TARIF BBNKB:                                │   │  │
    │  │  │  • Provinsi lain: 12% = 0.12                           │   │  │
    │  │  │  • DKI Jakarta: 12.5% = 0.125 (Perda DKI No.1/2024)   │   │  │
    │  │  │                                                          │   │  │
    │  │  │  FORMULA:                                               │   │  │
    │  │  │  ┌─────────────────────────────────────────────────┐   │  │  │
    │  │  │  │  IF is_dki THEN                                  │   │  │  │
    │  │  │  │     tarif_bbnkb = 0.125                          │   │  │  │
    │  │  │  │  ELSE                                             │   │  │  │
    │  │  │  │     tarif_bbnkb = 0.12                           │   │  │  │
    │  │  │  │  END IF                                           │   │  │  │
    │  │  │  │                                                     │   │  │  │
    │  │  │  │  bbnkb_pokok = njkb × tarif_bbnkb                │   │  │  │
    │  │  │  └─────────────────────────────────────────────────┘   │   │  │
    │  │  │                                                          │   │  │
    │  │  │  CONTOH NON-DKI:                                       │   │  │
    │  │  │  NJKB = 150.000.000, Non-DKI                          │   │  │
    │  │  │  tarif = 12%                                           │   │  │
    │  │  │  bbnkb_pokok = 150.000.000 × 0.12 = 18.000.000       │   │  │
    │  │  │                                                          │   │  │
    │  │  │  CONTOH DKI:                                            │   │  │
    │  │  │  NJKB = 150.000.000, DKI Jakarta                       │   │  │
    │  │  │  tarif = 12.5%                                         │   │  │
    │  │  │  bbnkb_pokok = 150.000.000 × 0.125 = 18.750.000      │   │  │
    │  │  └─────────────────────────────────────────────────────┘   │  │
    │  │                                                             │  │
    │  └───────────────────────────────────────────────────────────────┘  │
    └───────────────────────────────┬─────────────────────────────────────┘
                                    │
                                    ▼
    ┌─────────────────────────────────────────────────────────────────────┐
    │  STEP_6B: Hitung Opsen BBNKB                                        │
    │  ┌───────────────────────────────────────────────────────────────┐  │
    │  │                                                             │  │
    │  │  DECISION: Apakah wilayah DKI Jakarta?                       │  │
    │  │  ┌─────────────────────────────────────────────────────┐   │  │
    │  │  │  IF is_dki == TRUE THEN                            │   │  │
    │  │  │     opsen_bbnkb = 0                                  │   │  │
    │  │  │     → PROCEED TO: STEP_7                             │   │  │
    │  │  │  ELSE                                                │   │  │
    │  │  │     ┌─────────────────────────────────────────────┐ │   │  │
    │  │  │     │  FORMULA:                                   │ │   │  │
    │  │  │     │  ┌─────────────────────────────────────────┐ │ │   │  │
    │  │  │     │  │  opsen_bbnkb = bbnkb_pokok × 0.66     │ │ │   │  │
    │  │  │     │  └─────────────────────────────────────────┘ │ │   │  │
    │  │  │     │                                              │ │   │  │
    │  │  │     │  CONTOH:                                     │ │   │  │
    │  │  │     │  bbnkb_pokok = 18.000.000                   │ │   │  │
    │  │  │     │  opsen_bbnkb = 18.000.000 × 0.66           │ │   │  │
    │  │  │     │                    = 11.880.000             │ │   │  │
    │  │  │     └─────────────────────────────────────────────┘ │   │  │
    │  │  │  → PROCEED TO: STEP_7                             │   │  │
    │  │  └─────────────────────────────────────────────────────┘   │  │
    │  └───────────────────────────────────────────────────────────────┘  │
    └───────────────────────────────┬─────────────────────────────────────┘
                                    │
                                    ▼

═══════════════════════════════════════════════════════════════════════════════
STEP 7: CEK PAJAK PROGRESIF (KHUSUS WILAYAH DKI)
═══════════════════════════════════════════════════════════════════════════════

    ┌─────────────────────────────────────────────────────────────────────┐
    │  STEP_7: Cek Pajak Progresif                                       │
    │  ┌───────────────────────────────────────────────────────────────┐  │
    │  │                                                             │  │
    │  │  DECISION: Apakah wilayah menerapkan pajak progresif?        │  │
    │  │  ┌─────────────────────────────────────────────────────┐   │  │
    │  │  │  // Pajak progresif diterapkan di:                   │   │  │
    │  │  │  // - DKI Jakarta (Perda No.1/2024)                 │   │  │
    │  │  │  // - Beberapa provinsi lain yang menerapkan        │   │  │
    │  │  │  //                                                         │   │  │
    │  │  │  IF is_dki == TRUE THEN                             │   │  │
    │  │  │     → PROCEED TO: CHECK_PROGRESSIVE                  │   │  │
    │  │  │  ELSE                                                 │   │  │
    │  │  │     tarif_progresif = 0                               │   │  │
    │  │  │     → PROCEED TO: STEP_8                             │   │  │
    │  │  └─────────────────────────────────────────────────────┘   │  │
    │  │                                                             │  │
    │  └───────────────────────────────────────────────────────────────┘  │
    └───────────────────────────────┬─────────────────────────────────────┘
                                    │
                                    ▼
    ┌─────────────────────────────────────────────────────────────────────┐
    │  CHECK_PROGRESSIVE: Query Unit Aktif via API Samsat                 │
    │  ┌───────────────────────────────────────────────────────────────┐  │
    │  │                                                             │  │
    │  │  // Query API Samsat untuk cek jumlah kendaraan NIK          │  │
    │  │  ┌─────────────────────────────────────────────────────┐   │  │
    │  │  │  api_response = CALL_SAMSAT_API(                    │   │  │
    │  │  │                       endpoint = "cek_kepemilikan",  │   │  │
    │  │  │                       nik = nik_pembeli,             │   │  │
    │  │  │                       jenis = "RODA_4"              │   │  │
    │  │  │                    )                                │   │  │
    │  │  │                                                      │   │  │
    │  │  │  // Hitung jumlah kendaraan aktif                    │   │  │
    │  │  │  jumlah_kendaraan = COUNT(api_response.unit_aktif)  │   │  │
    │  │  │                                                      │   │  │
    │  │  │  // +1 karena kendaraan yang akan dibeli             │   │  │
    │  │  │  urutan = jumlah_kendaraan + 1                        │   │  │
    │  │  └─────────────────────────────────────────────────────┘   │  │
    │  │                                                             │  │
    │  └───────────────────────────────────────────────────────────────┘  │
    └───────────────────────────────┬─────────────────────────────────────┘
                                    │
                                    ▼
    ┌─────────────────────────────────────────────────────────────────────┐
    │  DETERMINE_TARIF_PROGRESIF:                                        │
    │  ┌───────────────────────────────────────────────────────────────┐  │
    │  │                                                             │  │
    │  │  MATRIX TARIF PROGRESIF (DKI Jakarta):                       │  │
    │  │  ┌─────────────────────────────────────────────────────┐   │  │
    │  │  │  urutan_kendaraan    │    tarif_pkb_progresif      │   │  │
    │  │  │  ─────────────────────────────────────────────────│   │  │
    │  │  │        1             │         2.0%               │   │  │
    │  │  │        2             │         3.0%               │   │  │
    │  │  │        3             │         4.0%               │   │  │
    │  │  │        4             │         5.0%               │   │  │
    │  │  │       5+             │         6.0% (maksimal)     │   │  │
    │  │  └─────────────────────────────────────────────────────┘   │  │
    │  │                                                             │  │
    │  │  PSEUDOCODE:                                                │  │
    │  │  ┌─────────────────────────────────────────────────────┐   │  │
    │  │  │  SELECT tarif_progresif FROM matrix_progresif      │   │  │
    │  │  │  WHERE urutan = LEAST(jumlah_kendaraan + 1, 5)    │   │  │
    │  │  └─────────────────────────────────────────────────────┘   │  │
    │  │                                                             │  │
    │  │  CONTOH:                                                    │  │
    │  │  NIK sudah punya 2 kendaraan, mau beli kendaraan ke-3       │  │
    │  │  → urutan = 3 → tarif = 4%                               │  │
    │  │                                                             │  │
    │  │  NJKB = 150.000.000                                       │  │
    │  │  PKB Progresif = 150.000.000 × 4% = 6.000.000            │  │
    │  │                                                             │  │
    │  └───────────────────────────────────────────────────────────────┘  │
    └───────────────────────────────┬─────────────────────────────────────┘
                                    │
                                    ▼

═══════════════════════════════════════════════════════════════════════════════
STEP 8: HITUNG PNBP (BIAYA ADMINISTRASI)
═══════════════════════════════════════════════════════════════════════════════

    ┌─────────────────────────────────────────────────────────────────────┐
    │  STEP_8: Hitung PNBP                                               │
    │  ┌───────────────────────────────────────────────────────────────┐  │
    │  │                                                             │  │
    │  │  KOMPONEN PNBP:                                             │  │
    │  │  ┌─────────────────────────────────────────────────────┐   │  │
    │  │  │  1. SWDKLLJ (Sumbangan Wajib Dana KJLLJ)           │   │  │
    │  │  │     ┌─────────────────────────────────────────────┐ │   │  │
    │  │  │     │  // Berdasarkan golongan CC mesin            │ │   │  │
    │  │  │     │  IF cc_mesin <= 1000 THEN                    │ │   │  │
    │  │  │     │       swdkllj = 35000 - 43000                │ │   │  │
    │  │  │     │  ELSIF cc_mesin <= 1500 THEN                 │ │   │  │
    │  │  │     │       swdkllj = 50000 - 63000                │ │   │  │
    │  │  │     │  ELSIF cc_mesin <= 2500 THEN                 │ │   │  │
    │  │  │     │       swdkllj = 62000 - 83000                │ │   │  │
    │  │  │     │  ELSE                                        │ │   │  │
    │  │  │     │       swdkllj = 82000 - 106000              │ │   │  │
    │  │  │     │  END IF                                       │ │   │  │
    │  │  │     └─────────────────────────────────────────────┘ │   │  │
    │  │  │                                                      │   │  │
    │  │  │  2. STNK 5 Tahun = Rp200.000                       │   │  │
    │  │  │  3. TNKB (Plat Nomor) = Rp100.000                  │   │  │
    │  │  │  4. BPKB (Buku) = Rp375.000                         │   │  │
    │  │  │                                                      │   │  │
    │  │  │  TOTAL_PNBP = swdkllj + 200000 + 100000 + 375000   │   │  │
    │  │  └─────────────────────────────────────────────────────┘   │  │
    │  │                                                             │  │
    │  │  CONTOH (CC 1500):                                        │  │
    │  │  SWDKLLJ = 50.000                                         │  │
    │  │  STNK = 200.000                                           │  │
    │  │  TNKB = 100.000                                           │  │
    │  │  BPKB = 375.000                                          │  │
    │  │  ─────────────────────────                                │  │
    │  │  TOTAL_PNBP = 725.000                                    │  │
    │  │                                                             │  │
    │  └───────────────────────────────────────────────────────────────┘  │
    └───────────────────────────────┬─────────────────────────────────────┘
                                    │
                                    ▼

═══════════════════════════════════════════════════════════════════════════════
STEP 9: OUTPUT RINCIAN TAGIHAN
═══════════════════════════════════════════════════════════════════════════════

    ┌─────────────────────────────────────────────────────────────────────┐
    │  STEP_9: Generate Output Rincian Tagihan                           │
    │  ┌───────────────────────────────────────────────────────────────┐  │
    │  │                                                             │  │
    │  │  STRUKTUR OUTPUT JSON:                                       │  │
    │  │  ┌─────────────────────────────────────────────────────┐   │  │
    │  │  │  {                                                      │   │  │
    │  │  │    "status": "SUCCESS",                               │   │  │
    │  │  │    "data": {                                          │   │  │
    │  │  │      "njkb": 150000000,                              │   │  │
    │  │  │      "dpp_ppn": 137500000,                          │   │  │
    │  │  │      "ppn": {                                        │   │  │
    │  │  │      │   "normal": 16500000,                       │   │  │
    │  │  │      │   "dtp": 0,                                   │   │  │
    │  │  │      │   "terutang": 16500000                       │   │  │
    │  │  │      "},                                              │   │  │
    │  │  │      "pkb": {                                        │   │  │
    │  │  │      │   "pokok": 2250000,                          │   │  │
    │  │  │      │   "opsen": 1485000,                          │   │  │
    │  │  │      │   "total": 3735000                          │   │  │
    │  │  │      "},                                              │   │  │
    │  │  │      "bbnkb": {                                      │   │  │
    │  │  │      │   "pokok": 18000000,                         │   │  │
    │  │  │      │   "opsen": 11880000,                         │   │  │
    │  │  │      │   "total": 29880000                          │   │  │
    │  │  │      "},                                              │   │  │
    │  │  │      "pnbp": {                                       │   │  │
    │  │  │      │   "swdkllj": 50000,                          │   │  │
    │  │  │      │   "stnk": 200000,                            │   │  │
    │  │  │      │   "tnkb": 100000,                            │   │  │
    │  │  │      │   "bpkb": 375000,                            │   │  │
    │  │  │      │   "total": 725000                            │   │  │
    │  │  │      "},                                              │   │  │
    │  │  │      "total_tagihan": 50855000                      │   │  │
    │  │  │    }                                                   │   │  │
    │  │  │  }                                                      │   │  │
    │  │  └─────────────────────────────────────────────────────┘   │  │
    │  │                                                             │  │
    │  └───────────────────────────────────────────────────────────────┘  │
    └───────────────────────────────┬─────────────────────────────────────┘
                                    │
                                    ▼
                            ┌───────────────┐
                            │ ✅ SELESAI   │
                            └───────────────┘
```

---

## 3.1.4 CONTOH PERHITUNGAN LENGKAP

```
╔══════════════════════════════════════════════════════════════════════════════╗
║                   CONTOH PERHITUNGAN PAJAK KENDARAAN LENGKAP                ║
╠══════════════════════════════════════════════════════════════════════════════╣
║                                                                              ║
║  STUDI KASUS 1: KENDARAAN BARU (NON-KBLBB, NON-DKI)                        ║
║  ═══════════════════════════════════════════════════════                    ║
║                                                                              ║
║  📋 DATA INPUT:                                                              ║
║  ┌────────────────────────────────────────────────────────────────────────┐  ║
║  │  Jenis Kendaraan     : BARU                                           │  ║
║  │  Merk/Model          : Toyota Avanza 1.5 G AT                        │  ║
║  │  Tahun                : 2024                                           │  ║
║  │  CC Mesin             : 1496                                          │  ║
║  │  NJKB                 : Rp185.000.000                                │  ║
║  │  TKDN                 : 75% (TIDAK eligible KBLBB karena >40%)       │  ║
║  │  is_kblbb             : FALSE                                         │  ║
║  │  Wilayah               : Jawa Barat (NON-DKI)                         │  ║
║  │  is_dki                : FALSE                                         │  ║
║  │  Golongan KB           : Golongan I                                   │  ║
║  │  Koefisian Road        : 1.00                                          │  ║
║  │  Tarif PKB             : 1.5%                                         │  ║
║  │  Pemilik               : Personal (kendaraan pertama)                  │  ║
║  └────────────────────────────────────────────────────────────────────────┘  ║
║                                                                              ║
║  ─────────────────────────────────────────────────────────────────────────── ║
║                                                                              ║
║  📊 LANGKAH 1: Hitung DPP PPN                                               ║
║  ┌────────────────────────────────────────────────────────────────────────┐  ║
║  │                                                                         │  ║
║  │     DPP PPN = (11/12) × NJKB                                          │  ║
║  │              = (11/12) × Rp185.000.000                                │  ║
║  │              = 0.9166667 × Rp185.000.000                              │  ║
║  │              = Rp169.583.333                                           │  ║
║  │                                                                         │  ║
║  └────────────────────────────────────────────────────────────────────────┘  ║
║                                                                              ║
║  📊 LANGKAH 2: Hitung PPN                                                   ║
║  ┌────────────────────────────────────────────────────────────────────────┐  ║
║  │                                                                         │  ║
║  │     Karena TKDN = 75% tapi is_kblbb = FALSE,                         │  ║
║  │     maka TIDAK eligible PPN DTP (harus kendaraan listrik)              │  ║
║  │                                                                         │  ║
║  │     PPN Normal = 12% × DPP PPN                                        │  ║
║  │               = 12% × Rp169.583.333                                   │  ║
║  │               = Rp20.350.000                                           │  ║
║  │                                                                         │  ║
║  │     ATAU (formula singkat):                                            │  ║
║  │     PPN = 11% × NJKB = 11% × Rp185.000.000 = Rp20.350.000             │  ║
║  │                                                                         │  ║
║  └────────────────────────────────────────────────────────────────────────┘  ║
║                                                                              ║
║  📊 LANGKAH 3: Hitung PKB                                                  ║
║  ┌────────────────────────────────────────────────────────────────────────┐  ║
║  │                                                                         │  ║
║  │     PKB Pokok = NJKB × Tarif PKB × Koef Road                         │  ║
║  │              = Rp185.000.000 × 1.5% × 1.00                           │  ║
║  │              = Rp185.000.000 × 0.015 × 1.00                          │  ║
║  │              = Rp2.775.000                                            │  ║
║  │                                                                         │  ║
║  │     Opsen PKB = 66% × PKB Pokok                                       │  ║
║  │              = 66% × Rp2.775.000                                       │  ║
║  │              = 0.66 × Rp2.775.000                                     │  ║
║  │              = Rp1.831.500                                            │  ║
║  │                                                                         │  ║
║  │     TOTAL PKB = PKB Pokok + Opsen PKB                                 │  ║
║  │              = Rp2.775.000 + Rp1.831.500                              │  ║
║  │              = Rp4.606.500                                            │  ║
║  │                                                                         │  ║
║  └────────────────────────────────────────────────────────────────────────┘  ║
║                                                                              ║
║  📊 LANGKAH 4: Hitung BBNKB                                                 ║
║  ┌────────────────────────────────────────────────────────────────────────┐  ║
║  │                                                                         │  ║
║  │     Karena NON-DKI, maka:                                              │  ║
║  │     BBNKB Pokok = 12% × NJKB                                          │  ║
║  │                 = 12% × Rp185.000.000                                 │  ║
║  │                 = 0.12 × Rp185.000.000                                │  ║
║  │                 = Rp22.200.000                                         │  ║
║  │                                                                         │  ║
║  │     Opsen BBNKB = 66% × BBNKB Pokok                                   │  ║
║  │                = 66% × Rp22.200.000                                   │  ║
║  │                = 0.66 × Rp22.200.000                                  │  ║
║  │                = Rp14.652.000                                         │  ║
║  │                                                                         │  ║
║  │     TOTAL BBNKB = BBNKB Pokok + Opsen BBNKB                           │  ║
║  │                = Rp22.200.000 + Rp14.652.000                          │  ║
║  │                = Rp36.852.000                                          │  ║
║  │                                                                         │  ║
║  └────────────────────────────────────────────────────────────────────────┘  ║
║                                                                              ║
║  📊 LANGKAH 5: Hitung PNBP                                                  ║
║  ┌────────────────────────────────────────────────────────────────────────┐  ║
║  │                                                                         │  ║
║  │     CC Mesin = 1496 → Golongan II (1001-1500cc)                       │  ║
║  │     SWDKLLJ = Rp50.000 - Rp63.000 → kita pakai Rp55.000               │  ║
║  │                                                                         │  ║
║  │     Komponen PNBP:                                                     │  ║
║  │     • SWDKLLJ : Rp55.000                                              │  ║
║  │     • STNK     : Rp200.000                                            │  ║
║  │     • TNKB     : Rp100.000                                            │  ║
║  │     • BPKB     : Rp375.000                                            │  ║
║  │     ─────────────────────────────────                                 │  ║
║  │     TOTAL PNBP : Rp730.000                                            │  ║
║  │                                                                         │  ║
║  └────────────────────────────────────────────────────────────────────────┘  ║
║                                                                              ║
║  ═══════════════════════════════════════════════════════════════════════════ ║
║                                                                              ║
║  💰 TOTAL TAGIHAN PAJAK KENDARAAN                                           ║
║  ┌────────────────────────────────────────────────────────────────────────┐  ║
║  │                                                                         │  ║
║  │     RINCIAN                          JUMLAH                            │  ║
║  │     ─────────────────────────────────────────────────────────────     │  ║
║  │     PPN (PPN 11% × NJKB)           Rp20.350.000                       │  ║
║  │     PKB + Opsen                     Rp4.606.500                        │  ║
║  │     BBNKB + Opsen                   Rp36.852.000                       │  ║
║  │     PNBP (SWDKLLJ+STNK+TNKB+BPKB)  Rp730.000                          │  ║
║  │     ─────────────────────────────────────────────────────────────     │  ║
║  │                                                                         │  ║
║  │     TOTAL TAGIHAN                  Rp62.538.500                        │  ║
║  │                                                                         │  ║
║  └────────────────────────────────────────────────────────────────────────┘  ║
║                                                                              ║
╠══════════════════════════════════════════════════════════════════════════════╣
║                                                                              ║
║  STUDI KASUS 2: KENDARAAN LISTRIK KBLBB (ELIGIBLE PPN DTP)                  ║
║  ═══════════════════════════════════════════════════════════════════════    ║
║                                                                              ║
║  📋 DATA INPUT:                                                              ║
║  ┌────────────────────────────────────────────────────────────────────────┐  ║
║  │  Jenis Kendaraan     : BARU                                           │  ║
║  │  Merk/Model          : Hyundai Ioniq 5                               │  ║
║  │  Tahun                : 2024                                           │  ║
║  │  Tipe                 : KBLBB (Kendaraan Listrik)                    │  ║
║  │  TKDN                 : 45% (≥40%, ELIGIBLE!)                         │  ║
║  │  is_kblbb             : TRUE                                          │  ║
║  │  NJKB                 : Rp500.000.000                                  │  ║
║  │  Wilayah               : Jawa Barat (NON-DKI)                           │  ║
║  │  is_dki                : FALSE                                         │  ║
║  └────────────────────────────────────────────────────────────────────────┘  ║
║                                                                              ║
║  ─────────────────────────────────────────────────────────────────────────── ║
║                                                                              ║
║  📊 PERHITUNGAN:                                                            ║
║  ┌────────────────────────────────────────────────────────────────────────┐  ║
║  │                                                                         │  ║
║  │     DPP PPN = (11/12) × Rp500.000.000 = Rp458.333.333                │  ║
║  │                                                                         │  ║
║  │     PPN Normal = 12% × Rp458.333.333 = Rp55.000.000                    │  ║
║  │     PPN DTP = 10% × Rp458.333.333 = Rp45.833.333                      │  ║
║  │     ─────────────────────────────────────────────                       │  ║
║  │     PPN Yang Dibayar = Rp55.000.000 - Rp45.833.333 = Rp9.166.667      │  ║
║  │                                                                         │  ║
║  │     ATAU (PPN Efektif):                                               │  ║
║  │     PPN = 2% × DPP = 2% × Rp458.333.333 = Rp9.166.667 ✓              │  ║
║  │                                                                         │  ║
║  │     PKB + Opsen = Rp500.000.000 × 1.5% × 1.66 = Rp12.450.000         │  ║
║  │     BBNKB + Opsen = Rp500.000.000 × 12% × 1.66 = Rp99.600.000       │  ║
║  │     PNBP = Rp730.000                                                  │  ║
║  │     ─────────────────────────────────────────────────────────────     │  ║
║  │                                                                         │  ║
║  │     TOTAL TAGIHAN = Rp9.166.667 + Rp12.450.000 +                      │  ║
║  │                      Rp99.600.000 + Rp730.000                          │  ║
║  │                    = Rp121.946.667                                    │  ║
║  │                                                                         │  ║
║  │     💡 BERHASIL HEMAT PPN Rp45.833.333!                               │  ║
║  │                                                                         │  ║
║  └────────────────────────────────────────────────────────────────────────┘  ║
║                                                                              ║
╠══════════════════════════════════════════════════════════════════════════════╣
║                                                                              ║
║  STUDI KASUS 3: KENDARAAN BEKAS DI DKI JAKARTA (PAJAK PROGRESIF)             ║
║  ═══════════════════════════════════════════════════════════════════════    ║
║                                                                              ║
║  📋 DATA INPUT:                                                              ║
║  ┌────────────────────────────────────────────────────────────────────────┐  ║
║  │  Jenis Kendaraan     : BEKAS                                          │  ║
║  │  NJKB (dari STNK)    : Rp150.000.000                                 │  ║
║  │  Wilayah               : DKI Jakarta                                   │  ║
║  │  is_dki                : TRUE                                          │  ║
║  │  Pemilik               : Personal                                      │  ║
║  │  Jumlah kendaraan NIK  : 2 unit (sudah punya 2 mobil)                  │  ║
║  │  Kendaraan ke-         : 3 (progresif 4%)                             │  ║
║  └────────────────────────────────────────────────────────────────────────┘  ║
║                                                                              ║
║  ─────────────────────────────────────────────────────────────────────────── ║
║                                                                              ║
║  📊 PERHITUNGAN:                                                            ║
║  ┌────────────────────────────────────────────────────────────────────────┐  ║
║  │                                                                         │  ║
║  │     DPP PPN = (11/12) × Rp150.000.000 = Rp137.500.000                 │  ║
║  │     PPN = 11% × Rp150.000.000 = Rp16.500.000                          │  ║
║  │                                                                         │  ║
║  │     ⚠️ PAJAK PROGRESIF (Kendaraan ke-3 = 4%):                         │  ║
║  │     PKB Pokok = Rp150.000.000 × 4% = Rp6.000.000                      │  ║
║  │     Opsen PKB = Rp0 (DKI TIDAK punya opsen!)                          │  ║
║  │     TOTAL PKB = Rp6.000.000                                            │  ║
║  │                                                                         │  ║
║  │     ⚠️ BBNKB KHUSUS DKI (12.5%):                                      │  ║
║  │     BBNKB Pokok = 12.5% × Rp150.000.000 = Rp18.750.000                │  ║
║  │     Opsen BBNKB = Rp0 (DKI TIDAK punya opsen!)                        │  ║
║  │     TOTAL BBNKB = Rp18.750.000                                        │  ║
║  │                                                                         │  ║
║  │     PNBP = Rp730.000                                                   │  ║
║  │     ─────────────────────────────────────────────────────────────     │  ║
║  │                                                                         │  ║
║  │     TOTAL TAGIHAN = Rp16.500.000 + Rp6.000.000 +                       │  ║
║  │                      Rp18.750.000 + Rp730.000                          │  ║
║  │                    = Rp41.980.000                                     │  ║
║  │                                                                         │  ║
║  └────────────────────────────────────────────────────────────────────────┘  ║
║                                                                              ║
╚══════════════════════════════════════════════════════════════════════════════╝
```

---

# BAGIAN 3.2: MODUL SIMULASI KREDIT (NPF-BASED)

> **Lokasi di SRS:** Sub-bab 3.5
> **Lokasi Flowchart:** BusinessLogic-MermaidJS.md → Section 2: Logika Simulasi Kredit

---

## 3.2.1 GLOSARIUM ISTILAH KREDIT

```
╔══════════════════════════════════════════════════════════════════════════════╗
║                         GLOSARIUM ISTILAH KREDIT                            ║
╠══════════════════════════════════════════════════════════════════════════════╣
║                                                                              ║
║  1. NPF (Non-Performing Finance)                                            ║
║  ─────────────────────────────────                                         ║
║     DEFINISI:                                                                ║
║     Rasio kredit macet perusahaan pembiayaan. NPF menunjukkan seberapa       ║
║     sehat perusahaan leasing dalam mengelola portofolio kreditnya.         ║
║                                                                              ║
║     RUMUS:                                                                   ║
║     ┌──────────────────────────────────────────────────────────────────┐    ║
║     │  NPF = (Total Kredit Macet / Total Kredit Diberikan) × 100%   │    ║
║     └──────────────────────────────────────────────────────────────────┘    ║
║                                                                              ║
║     KLASIFIKASI KREDIT MACET:                                              ║
║     • Lancar (Kol 1): Tepat waktu                                          ║
║     • DPK (Kol 2): 1-30 hari terlambat                                    ║
║     • Kurang Lancar (Kol 3): 31-90 hari                                   ║
║     • Diragukan (Kol 4): 91-120 hari                                       ║
║     • Macet (Kol 5): >120 hari                                             ║
║                                                                              ║
╠══════════════════════════════════════════════════════════════════════════════╣
║                                                                              ║
║  2. DP (Uang Muka / Down Payment)                                          ║
║  ─────────────────────────────────                                         ║
║     DEFINISI:                                                                ║
║     Sejumlah uang yang dibayarkan di muka oleh pembeli saat kredit.         ║
║     DP merupakan bagian dari harga yang TIDAK dibiayai leasing.             ║
║                                                                              ║
║     RUMUS:                                                                   ║
║     ┌──────────────────────────────────────────────────────────────────┐    ║
║     │  DP = Persentase_DP × Harga_OTR                                  │    ║
║     │  Pinjaman = Harga_OTR - DP                                      │    ║
║     └──────────────────────────────────────────────────────────────────┘    ║
║                                                                              ║
║     ⚠️ DP MINIMUM BERDASARKAN NPF:                                          ║
║     • NPF ≤ 1% → DP Minimum 0%                                             ║
║     • NPF 1-3% → DP Minimum 10%                                            ║
║     • NPF 3-5% → DP Minimum 15%                                            ║
║     • NPF > 5% → DP Minimum 20%                                            ║
║                                                                              ║
╠══════════════════════════════════════════════════════════════════════════════╣
║                                                                              ║
║  3. iDeb (Indonesian Financial Data Exchange)                              ║
║  ───────────────────────────────────────────                                ║
║     DEFINISI:                                                                ║
║     Platform berbagi data kredit yang dikelola oleh OJK. Melalui iDeb,      ║
║     leasing dapat mengakses riwayat kredit seseorang.                        ║
║                                                                              ║
║     DATA YANG DIDAPAT:                                                       ║
║     • Skor kolektibilitas (1-5)                                           ║
║     • Riwayat pembayaran kredit sebelumnya                                  ║
║     • Status kolektibilitas saat ini                                       ║
║     • Jumlah kredit aktif                                                   ║
║                                                                              ║
║     SKALA KOLEKTIBILITAS:                                                   ║
║     ┌──────────────────────────────────────────────────────────────────┐    ║
║     │  1 = Lancar (tidak pernah terlambat)                            │    ║
║     │  2 = Dalam perhatian khusus (terlambat 1-30 hari)               │    ║
║     │  3 = Kurang lancar (terlambat 31-90 hari)                       │    ║
║     │  4 = Diragukan (terlambat 91-120 hari)                          │    ║
║     │  5 = Macet (terlambat >120 hari)                                 │    ║
║     └──────────────────────────────────────────────────────────────────┘    ║
║                                                                              ║
╠══════════════════════════════════════════════════════════════════════════════╣
║                                                                              ║
║  4. SLIK (Sistem Layanan Informasi Keuangan)                               ║
║  ─────────────────────────────────────────────                             ║
║     DEFINISI:                                                                ║
║     Sistem database yang dikelola OJK untuk mencatat informasi borrower.     ║
║     SLIK mencakup data kredit dari seluruh lembaga keuangan di Indonesia.    ║
║                                                                              ║
║     KETERKAITAN:                                                            ║
║     • SLIK adalah sistem utama                                             ║
║     • iDeb adalah platform distribusi data SLIK                             ║
║     • PEFINDO adalah salah satu akses SLIK                                 ║
║                                                                              ║
╠══════════════════════════════════════════════════════════════════════════════╣
║                                                                              ║
║  5. Tenor                                                                   ║
║  ──────                                                                     ║
║     DEFINISI:                                                                ║
║     Jangka waktu pinjaman kredit. Untuk kredit mobil, tenor umumnya         ║
║     berkisar 12-72 bulan.                                                   ║
║                                                                              ║
║     PENGARUH TENOR:                                                         ║
║     • Tenor pendek → Angsuran tinggi, total bunga rendah                   ║
║     • Tenor panjang → Angsuran rendah, total bunga tinggi                   ║
║                                                                              ║
╠══════════════════════════════════════════════════════════════════════════════╣
║                                                                              ║
║  6. Bunga Kredit                                                            ║
║  ─────────────                                                               ║
║     DEFINISI:                                                                ║
║     Bunga adalah biaya tambahan yang harus dibayarkan borrower di atas      ║
║     pokok pinjaman.                                                         ║
║                                                                              ║
║     JENIS BUNGA:                                                            ║
║     ┌──────────────────────────────────────────────────────────────────┐    ║
║     │  A. Flat Rate (Bunga Flat)                                      │    ║
║     │     → Bunga dihitung dari pokok AWAL saja                       │    ║
║     │     → Angsuran tetap tiap bulan                                │    ║
║     │     → CONTOH: Pinjaman Rp100jt, Bunga 10%/th flat, 12 bulan    │    ║
║     │        Bunga per bulan = 100.000.000 × 10% / 12 = Rp833.333   │    ║
║     │        Angsuran = (100.000.000 / 12) + 833.333 = Rp9.166.667  │    ║
║     │                                                                  │    ║
║     │  B. Effective Rate (Bunga Efektif)                              │    ║
║     │     → Bunga dihitung dari sisa pokok                            │    ║
║     │     → Angsuran menurun tiap bulan                                │    ║
║     │     → LEBIH ADIL karena bunga mengikuti sisa pinjaman          │    ║
║     └──────────────────────────────────────────────────────────────────┘    ║
║                                                                              ║
╠══════════════════════════════════════════════════════════════════════════════╣
║                                                                              ║
║  7. Angsuran                                                                ║
║  ─────────                                                                  ║
║     DEFINISI:                                                                ║
║     Jumlah pembayaran bulanan yang harus dibayarkan borrower ke leasing.    ║
║     Angsuran = Pokok per bulan + Bunga per bulan                           ║
║                                                                              ║
║     RUMUS (Flat Rate):                                                      ║
║     ┌──────────────────────────────────────────────────────────────────┐    ║
║     │  Angsuran = (Pinjaman / Tenor) + (Pinjaman × Bunga Tahunan)     │    ║
║     │  Angsuran = (Pinjaman / Tenor) + Bunga_Bulanan                 │    ║
║     └──────────────────────────────────────────────────────────────────┘    ║
║                                                                              ║
║     RUMUS (Effective Rate):                                                ║
║     ┌──────────────────────────────────────────────────────────────────┐    ║
║     │  Angsuran = Pinjaman × [i × (1+i)^n] / [(1+i)^n - 1]          │    ║
║     │  dimana: i = bunga per bulan, n = jumlah bulan                  │    ║
║     └──────────────────────────────────────────────────────────────────┘    ║
║                                                                              ║
╠══════════════════════════════════════════════════════════════════════════════╣
║                                                                              ║
║  8. ESB (Enterprise Service Bus)                                           ║
║  ─────────────────────────────────                                         ║
║     DEFINISI:                                                                ║
║     Arsitektur integrasi asynchronous yang menghubungkan sistem dealer     ║
║     dengan sistem leasing. ESB menggunakan message broker untuk             ║
║     komunikasi.                                                              ║
║                                                                              ║
║     KEUNTUNGAN ESB:                                                         ║
║     • Response time lebih cepat: ~4.22 detik (vs 31.49 detik sync)         ║
║     • Error rate rendah                                                    ║
║     • Tidak memblokir user saat menunggu response                          ║
║                                                                              ║
╠══════════════════════════════════════════════════════════════════════════════╣
║                                                                              ║
║  9. Fidusia                                                                 ║
║  ────────                                                                   ║
║     DEFINISI:                                                                ║
║     Jaminan kebendaan berupa pengikatan kendaraan sebagai agunan kredit.    ║
║     Fidusia memberikan hak preferen leasing atas kendaraan jika borrower   ║
║     wanprestasi.                                                             ║
║                                                                              ║
║     BATAS WAKTU:                                                            ║
║     ⚠️ Pendaftaran fidusia WAJIB dilakukan dalam 30 hari kalender sejak    ║
║        akta notaris ditandatangani.                                         ║
║                                                                              ║
╠══════════════════════════════════════════════════════════════════════════════╣
║                                                                              ║
║  10. Leasing / Multifinance                                                ║
║  ─────────────────────────────                                              ║
║     DEFINISI:                                                                ║
║     Perusahaan pembiayaan yang memberikan dana untuk pembelian kendaraan.   ║
║     Leasing akan menahan BPKB sebagai jaminan hingga kredit lunas.         ║
║                                                                              ║
║     CONTOH LEASING DI INDONESIA:                                            ║
║     • Astra Credit Companies (ACC)                                         ║
║     • Federal International Finance (FIF)                                   ║
║     • Toyota Financial Service (TFS)                                       ║
║     • Wom Finance                                                           ║
║     • Adira Finance                                                         ║
║     • Mitsubishi Finance                                                    ║
║                                                                              ║
╚══════════════════════════════════════════════════════════════════════════════╝
```

---

## 3.2.2 INPUT VARIABEL SIMULASI KREDIT

```
╔══════════════════════════════════════════════════════════════════════════════╗
║                      INPUT VARIABEL SIMULASI KREDIT                          ║
╠══════════════════════════════════════════════════════════════════════════════╣
║                                                                              ║
║  GRUP A: DATA PEMBELI                                                       ║
║  ═════════════════════════════════════════                                  ║
║                                                                              ║
║  ┌─────────────────┬──────────────────┬─────────────────────────────────┐  ║
║  │ Variabel       │ Tipe Data        │ Deskripsi                       │  ║
║  ├─────────────────┼──────────────────┼─────────────────────────────────┤  ║
║  │ nik             │ VARCHAR(16)       │ NIK 16 digit                   │  ║
║  │ nama_lengkap    │ VARCHAR(255)      │ Nama sesuai KTP                 │  ║
║  │ tanggal_lahir   │ DATE             │ Tanggal lahir                   │  ║
║  │ email           │ VARCHAR(255)      │ Email aktif                     │  ║
║  │ nomor_hp        │ VARCHAR(15)      │ No HP aktif (untuk OTP)         │  ║
║  │ pekerjaan       │ VARCHAR(100)      │ Pekerjaan                       │  ║
║  │ penghasilan     │ DECIMAL(15,2)    │ Penghasilan bulanan             │  ║
║  └─────────────────┴──────────────────┴─────────────────────────────────┘  ║
║                                                                              ║
╠══════════════════════════════════════════════════════════════════════════════╣
║                                                                              ║
║  GRUP B: DATA KENDARAAN                                                    ║
║  ═════════════════════════════════════════                                  ║
║                                                                              ║
║  ┌─────────────────┬──────────────────┬─────────────────────────────────┐  ║
║  │ Variabel       │ Tipe Data        │ Deskripsi                       │  ║
║  ├─────────────────┼──────────────────┼─────────────────────────────────┤  ║
║  │ merk            │ VARCHAR(100)      │ Merk kendaraan                  │  ║
║  │ model           │ VARCHAR(100)      │ Model kendaraan                 │  ║
║  │ tahun           │ INTEGER           │ Tahun produksi                  │  ║
║  │ harga_otr       │ DECIMAL(15,2)    │ On The Road price               │  ║
║  │ jenis           │ ENUM             │ BARU atau BEKAS                 │  ║
║  │ status          │ ENUM             │ DRAFT, BOOKED, dll             │  ║
║  └─────────────────┴──────────────────┴─────────────────────────────────┘  ║
║                                                                              ║
╠══════════════════════════════════════════════════════════════════════════════╣
║                                                                              ║
║  GRUP C: PARAMETER KREDIT                                                   ║
║  ═════════════════════════════════════════                                  ║
║                                                                              ║
║  ┌─────────────────┬──────────────────┬─────────────────────────────────┐  ║
║  │ Variabel       │ Tipe Data        │ Deskripsi                       │  ║
║  ├─────────────────┼──────────────────┼─────────────────────────────────┤  ║
║  │ dp_persen       │ DECIMAL(5,2)     │ Persentase DP yang diminta (%)  │  ║
║  │ tenor_bulan     │ INTEGER           │ Tenor kredit (12-72)            │  ║
║  │ bunga_tahunan   │ DECIMAL(5,4)     │ Suku bunga tahunan (% decimal) │  ║
║  │ jenis_bunga     │ ENUM             │ FLAT atau EFEKTIF               │  ║
║  │ leasing_id      │ UUID             │ ID leasing partner               │  ║
║  └─────────────────┴──────────────────┴─────────────────────────────────┘  ║
║                                                                              ║
╠══════════════════════════════════════════════════════════════════════════════╣
║                                                                              ║
║  GRUP D: DATA DARI API EXTERNAL                                            ║
║  ═════════════════════════════════════════                                  ║
║                                                                              ║
║  ┌─────────────────┬──────────────────┬─────────────────────────────────┐  ║
║  │ Variabel       │ Tipe Data        │ Sumber                          │  ║
║  ├─────────────────┼──────────────────┼─────────────────────────────────┤  ║
║  │ kolektibilitas │ INTEGER           │ iDeb / SLIK (1-5)              │  ║
║  │ riwayat_kredit  │ JSONB             │ iDeb (array histori)           │  ║
║  │ npf_leasing     │ DECIMAL(5,2)     │ API Leasing Partner            │  ║
║  │ credit_score    │ INTEGER           │ Scoring dari PEFINDO (300-850) │  ║
║  └─────────────────┴──────────────────┴─────────────────────────────────┘  ║
║                                                                              ║
╚══════════════════════════════════════════════════════════════════════════════╝
```

---

## 3.2.3 FLOWCHART DETAIL SIMULASI KREDIT

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                   FLOWCHART SIMULASI KREDIT (VERSI DETAIL)                   │
└─────────────────────────────────────────────────────────────────────────────┘

═══════════════════════════════════════════════════════════════════════════════
STEP 1: AMBIL DATA & VALIDASI AWAL
═══════════════════════════════════════════════════════════════════════════════

    ┌─────────────────────────────────────────────────────────────────────┐
    │  START: Inisiasi Simulasi Kredit                                   │
    └───────────────────────────────┬─────────────────────────────────────┘
                                    │
                                    ▼
    ┌─────────────────────────────────────────────────────────────────────┐
    │  INPUT: Data Pembeli & Kendaraan                                    │
    │  ┌───────────────────────────────────────────────────────────────┐  │
    │  │  • nik_pembeli = "3275XXXXXXXXXXXXX"                         │  │
    │  │  • nama_pembeli = "John Doe"                                 │  │
    │  │  • harga_otr = 200.000.000                                   │  │
    │  │  • dp_persen_request = 20 (diingikan user)                   │  │
    │  │  • tenor_request = 48                                         │  │
    │  │  • leasing_id = "uuid-leasing-partner"                        │  │
    │  └───────────────────────────────────────────────────────────────┘  │
    └───────────────────────────────┬─────────────────────────────────────┘
                                    │
                                    ▼
    ┌─────────────────────────────────────────────────────────────────────┐
    │  VALIDATION_001: Validasi Input                                     │
    │  ┌───────────────────────────────────────────────────────────────┐  │
    │  │  IF nik_pembeli LENGTH != 16 THEN                            │  │
    │  │     RETURN ERROR("NIK harus 16 digit")                       │  │
    │  │  END IF                                                       │  │
    │  │                                                              │  │
    │  │  IF harga_otr <= 0 THEN                                      │  │
    │  │     RETURN ERROR("Harga OTR tidak valid")                    │  │
    │  │  END IF                                                       │  │
    │  │                                                              │  │
    │  │  IF tenor_request NOT IN [12,24,36,48,60,72] THEN           │  │
    │  │     RETURN ERROR("Tenor harus 12,24,36,48,60, atau 72")      │  │
    │  │  END IF                                                       │  │
    │  └───────────────────────────────────────────────────────────────┘  │
    └───────────────────────────────┬─────────────────────────────────────┘
                                    │
                                    ▼

═══════════════════════════════════════════════════════════════════════════════
STEP 2: QUERY SLIK OJ K - AMBIL DATA KOLEKTIBILITAS
═══════════════════════════════════════════════════════════════════════════════

    ┌─────────────────────────────────────────────────────────────────────┐
    │  CALL_API_SLIK: Query SLIK OJK untuk data riwayat kredit            │
    │  ┌───────────────────────────────────────────────────────────────┐  │
    │  │  // Request ke API SLIK/iDeb                                 │  │
    │  │  api_request = {                                             │  │
    │  │      "endpoint": "/v1/ideb/query",                           │  │
    │  │      "nik": nik_pembeli,                                     │  │
    │  │      "nama": nama_pembeli,                                   │  │
    │  │      "tanggal_lahir": tanggal_lahir                          │  │
    │  │  }                                                            │  │
    │  │                                                               │  │
    │  │  api_response = HTTP_POST(                                    │  │
    │  │      url = "https://api.ojk.go.id/ideb",                     │  │
    │  │      body = api_request,                                     │  │
    │  │      headers = {                                             │  │
    │  │          "Authorization": "Bearer " + ACCESS_TOKEN,         │  │
    │  │          "Content-Type": "application/json"                  │  │
    │  │      }                                                        │  │
    │  │  )                                                            │  │
    │  │                                                               │  │
    │  │  // Parse response                                           │  │
    │  │  ideb_data = PARSE_JSON(api_response.body)                   │  │
    │  │  kolektibilitas = ideb_data.kolektibilitas_terbaik           │  │
    │  │  credit_score = ideb_data.score                               │  │
    │  │  riwayat_pembayaran = ideb_data.riwayat                       │  │
    │  └───────────────────────────────────────────────────────────────┘  │
    └───────────────────────────────┬─────────────────────────────────────┘
                                    │
                                    ▼
    ┌─────────────────────────────────────────────────────────────────────┐
    │  DECISION_001: Cek Kolektibilitas                                   │
    │  ┌───────────────────────────────────────────────────────────────┐  │
    │  │                                                               │  │
    │  │  IF kolektibilitas IN [1, 2] THEN                            │  │
    │  │     // Lancar atau Dalam Perhatian Khusus                    │  │
    │  │     status_kredit = "LAYAK"                                  │  │
    │  │     → PROCEED TO: STEP_3                                    │  │
    │  │                                                               │  │
    │  │  ELSIF kolektibilitas IN [3, 4] THEN                        │  │
    │  │     // Kurang lancar atau Diragukan                          │  │
    │  │     status_kredit = "PERLU_ANALISIS"                         │  │
    │  │     → PROCEED TO: STEP_3                                    │  │
    │  │                                                               │  │
    │  │  ELSE (kolektibilitas = 5)                                  │  │
    │  │     // Macet                                                   │  │
    │  │     status_kredit = "TIDAK_LAYAK"                            │  │
    │  │     → PROCEED TO: REJECT_APPLICATION                         │  │
    │  │  END IF                                                       │  │
    │  │                                                               │  │
    │  └───────────────────────────────────────────────────────────────┘  │
    └───────────────────────────────┬─────────────────────────────────────┘
                                    │
                                    ▼

═══════════════════════════════════════════════════════════════════════════════
STEP 3: QUERY NPF LEASING PARTNER
═══════════════════════════════════════════════════════════════════════════════

    ┌─────────────────────────────────────────────────────────────────────┐
    │  CALL_API_NPF: Ambil NPF leasing partner                          │
    │  ┌───────────────────────────────────────────────────────────────┐  │
    │  │                                                               │  │
    │  │  // Request ke API Leasing untuk data NPF                     │  │
    │  │  api_request = {                                             │  │
    │  │      "endpoint": "/v1/leasing/npf",                          │  │
    │  │      "leasing_id": leasing_id                                │  │
    │  │  }                                                            │  │
    │  │                                                               │  │
    │  │  api_response = HTTP_GET(                                     │  │
    │  │      url = "https://api.leasing.com/npf/" + leasing_id,       │  │
    │  │      headers = {"Authorization": "Bearer " + TOKEN}           │  │
    │  │  )                                                            │  │
    │  │                                                               │  │
    │  │  npf_data = PARSE_JSON(api_response.body)                    │  │
    │  │  npf_leasing = npf_data.npf_net                              │  │
    │  │  // npf_net = NPF setelah dikurangi cadangan piutang ragu     │  │
    │  │                                                               │  │
    │  │  // Catat ke log untuk audit                                  │  │
    │  │  LOG.info("NPF Leasing " + leasing_id + " = " + npf_leasing) │  │
    │  │                                                               │  │
    │  └───────────────────────────────────────────────────────────────┘  │
    └───────────────────────────────┬─────────────────────────────────────┘
                                    │
                                    ▼

═══════════════════════════════════════════════════════════════════════════════
STEP 4: TENTUKAN DP MINIMUM BERDASARKAN NPF
═══════════════════════════════════════════════════════════════════════════════

    ┌─────────────────────────────────────────────────────────────────────┐
    │  DETERMINE_DP_MIN: Matrix NPF vs DP Minimum                        │
    │  ┌───────────────────────────────────────────────────────────────┐  │
    │  │                                                               │  │
    │  │  MATRIX PENENTUAN DP:                                         │  │
    │  │  ┌─────────────────────────────────────────────────────┐   │  │
    │  │  │  NPF Leasing        │  DP Minimum  │  Kategori       │   │  │
    │  │  │  ─────────────────────────────────────────────────│   │  │
    │  │  │  NPF ≤ 1%          │  0%          │  Sangat Sehat   │   │  │
    │  │  │  1% < NPF ≤ 3%    │  10%         │  Sehat          │   │  │
    │  │  │  3% < NPF ≤ 5%    │  15%         │  Cukup Sehat    │   │  │
    │  │  │  NPF > 5%         │  20%         │  Hati-hati      │   │  │
    │  │  └─────────────────────────────────────────────────────┘   │  │
    │  │                                                               │  │
    │  │  PSEUDOCODE:                                                  │  │
    │  │  ┌─────────────────────────────────────────────────────┐   │  │
    │  │  │  IF npf_leasing <= 0.01 THEN                        │   │  │
    │  │  │     dp_minimum = 0.00                               │   │  │
    │  │  │  ELSIF npf_leasing <= 0.03 THEN                     │   │  │
    │  │  │     dp_minimum = 0.10                               │   │  │
    │  │  │  ELSIF npf_leasing <= 0.05 THEN                     │   │  │
    │  │  │     dp_minimum = 0.15                               │   │  │
    │  │  │  ELSE                                                │   │  │
    │  │  │     dp_minimum = 0.20                               │   │  │
    │  │  │  END IF                                              │   │  │
    │  │  │                                                       │   │  │
    │  │  │  // Konversi ke persen untuk display                 │   │  │
    │  │  │  dp_minimum_persen = dp_minimum * 100                │   │  │
    │  │  └─────────────────────────────────────────────────────┘   │  │
    │  │                                                               │  │
    │  └───────────────────────────────────────────────────────────────┘  │
    └───────────────────────────────┬─────────────────────────────────────┘
                                    │
                                    ▼
    ┌─────────────────────────────────────────────────────────────────────┐
    │  VALIDATION_002: Cek DP yang diminta vs Minimum                    │
    │  ┌───────────────────────────────────────────────────────────────┐  │
    │  │                                                               │  │
    │  │  dp_persen_request = input.dp_persen / 100  // convert       │  │
    │  │                                                               │  │
    │  │  IF dp_persen_request < dp_minimum THEN                       │  │
    │  │     // User meminta DP lebih kecil dari minimum                │  │
    │  │     // Beri warning tapi tetap proses                         │  │
    │  │     ┌─────────────────────────────────────────────────────┐ │  │
    │  │     │  warning = "DP yang diminta (" + (dp_minimum*100) +  │ │  │
    │  │     │         "%) lebih kecil dari minimum. Sistem akan    │ │  │
    │  │     │         menggunakan DP minimum " + (dp_minimum*100)+  │ │  │
    │  │     │         "% untuk simulasi."                          │ │  │
    │  │     │  dp_final_persen = dp_minimum                        │ │  │
    │  │     └─────────────────────────────────────────────────────┘ │  │
    │  │  ELSE                                                         │  │
    │  │     dp_final_persen = dp_persen_request                      │  │
    │  │  END IF                                                        │  │
    │  │                                                               │  │
    │  └───────────────────────────────────────────────────────────────┘  │
    └───────────────────────────────┬─────────────────────────────────────┘
                                    │
                                    ▼

═══════════════════════════════════════════════════════════════════════════════
STEP 5: HITUNG KOMPONEN KREDIT
═══════════════════════════════════════════════════════════════════════════════

    ┌─────────────────────────────────────────────────────────────────────┐
    │  CALC_COMPONENTS: Hitung komponen kredit                            │
    │  ┌───────────────────────────────────────────────────────────────┐  │
    │  │                                                               │  │
    │  │  // Hitung DP                                                 │  │
    │  │  ┌─────────────────────────────────────────────────────┐   │  │
    │  │  │  dp_amount = harga_otr × dp_final_persen              │   │  │
    │  │  │  dp_amount = 200.000.000 × 0.15 = 30.000.000        │   │  │
    │  │  └─────────────────────────────────────────────────────┘   │  │
    │  │                                                               │  │
    │  │  // Hitung Pinjaman                                          │  │
    │  │  ┌─────────────────────────────────────────────────────┐   │  │
    │  │  │  jumlah_pinjaman = harga_otr - dp_amount            │   │  │
    │  │  │  jumlah_pinjaman = 200.000.000 - 30.000.000          │   │  │
    │  │  │                        = 170.000.000                 │   │  │
    │  │  └─────────────────────────────────────────────────────┘   │  │
    │  │                                                               │  │
    │  │  // Ambil bunga dari rate tabel leasing                     │  │
    │  │  ┌─────────────────────────────────────────────────────┐   │  │
    │  │  │  bunga_tahunan = GET_BUNGA_RATE(                     │   │  │
    │  │  │      leasing_id = leasing_id,                        │   │  │
    │  │  │      tenor = tenor_request,                           │   │  │
    │  │  │      jenis_kendaraan = jenis_kendaraan               │   │  │
    │  │  │  )                                                    │   │  │
    │  │  │  // Contoh: bunga_tahunan = 0.08 (8% flat)          │   │  │
    │  │  └─────────────────────────────────────────────────────┘   │  │
    │  │                                                               │  │
    │  └───────────────────────────────────────────────────────────────┘  │
    └───────────────────────────────┬─────────────────────────────────────┘
                                    │
                                    ▼

═══════════════════════════════════════════════════════════════════════════════
STEP 6: HITUNG ANGSURAN (FLAT RATE)
═══════════════════════════════════════════════════════════════════════════════

    ┌─────────────────────────────────────────────────────────────────────┐
    │  CALC_ANGGARAN_FLAT: Perhitungan Angsuran Flat Rate                 │
    │  ┌───────────────────────────────────────────────────────────────┐  │
    │  │                                                               │  │
    │  │  VARIABEL:                                                    │  │
    │  │  ┌─────────────────────────────────────────────────────┐   │  │
    │  │  │  INPUT:                                                  │   │  │
    │  │  │  • jumlah_pinjaman = 170.000.000                       │   │  │
    │  │  │  • bunga_tahunan = 0.08 (8%)                          │   │  │
    │  │  │  • tenor_bulan = 48                                    │   │  │
    │  │  │                                                          │   │  │
    │  │  │  KONSTANTA:                                             │   │  │
    │  │  │  • BUNGA_BULAN_FLAT = bunga_tahunan / 12               │   │  │
    │  │  │                                                          │   │  │
    │  │  │  KALKULASI:                                             │   │  │
    │  │  │  ┌─────────────────────────────────────────────────┐   │  │  │
    │  │  │  │  // 1. Hitung bunga bulanan (flat)             │   │  │  │
    │  │  │  │  bunga_bulanan_flat =                           │   │  │  │
    │  │  │  │      jumlah_pinjaman × (bunga_tahunan / 12)      │   │  │  │
    │  │  │  │  bunga_bulanan_flat =                           │   │  │  │
    │  │  │  │      170.000.000 × (0.08 / 12)                  │   │  │  │
    │  │  │  │  bunga_bulanan_flat = 170.000.000 × 0.006667   │   │  │  │
    │  │  │  │  bunga_bulanan_flat = Rp1.133.333              │   │  │  │
    │  │  │  └─────────────────────────────────────────────────┘   │  │  │
    │  │  │                                                          │   │  │
    │  │  │  ┌─────────────────────────────────────────────────┐   │  │  │
    │  │  │  │  // 2. Hitung pokok per bulan                  │   │  │  │
    │  │  │  │  pokok_per_bulan = jumlah_pinjaman / tenor      │   │  │  │
    │  │  │  │  pokok_per_bulan = 170.000.000 / 48            │   │  │  │
    │  │  │  │  pokok_per_bulan = Rp3.541.667                │   │  │  │
    │  │  │  └─────────────────────────────────────────────────┘   │   │  │
    │  │  │                                                          │   │  │
    │  │  │  ┌─────────────────────────────────────────────────┐   │  │  │
    │  │  │  │  // 3. Hitung total angsuran                  │   │  │  │
    │  │  │  │  angsuran_bulanan =                           │   │  │  │
    │  │  │  │      pokok_per_bulan + bunga_bulanan_flat       │   │  │  │
    │  │  │  │  angsuran_bulanan = 3.541.667 + 1.133.333     │   │  │  │
    │  │  │  │  angsuran_bulanan = Rp4.675.000              │   │  │  │
    │  │  │  └─────────────────────────────────────────────────┘   │   │  │
    │  │  │                                                          │   │  │
    │  │  │  ┌─────────────────────────────────────────────────┐   │  │  │
    │  │  │  │  // 4. Hitung total pembayaran                 │   │  │  │
    │  │  │  │  total_bunga = bunga_bulanan_flat × tenor      │   │  │  │
    │  │  │  │  total_bunga = 1.133.333 × 48 = 54.400.000   │   │  │  │
    │  │  │  │                                                 │   │  │  │
    │  │  │  │  total_pembayaran =                            │   │  │  │
    │  │  │  │      dp_amount + jumlah_pinjaman + total_bunga │   │  │  │
    │  │  │  │  total_pembayaran =                            │   │  │  │
    │  │  │  │      30.000.000 + 170.000.000 + 54.400.000   │   │  │  │
    │  │  │  │  total_pembayaran = 254.400.000              │   │  │  │
    │  │  │  └─────────────────────────────────────────────────┘   │   │  │
    │  │  └─────────────────────────────────────────────────────┘   │  │
    │  │                                                               │  │
    │  └───────────────────────────────────────────────────────────────┘  │
    └───────────────────────────────┬─────────────────────────────────────┘
                                    │
                                    ▼

═══════════════════════════════════════════════════════════════════════════════
STEP 7: OUTPUT HASIL SIMULASI
═══════════════════════════════════════════════════════════════════════════════

    ┌─────────────────────────────────────────────────────────────────────┐
    │  GENERATE_OUTPUT: Generate JSON hasil simulasi                       │
    │  ┌───────────────────────────────────────────────────────────────┐  │
    │  │  {                                                            │  │
    │  │    "status": "SUCCESS",                                       │  │
    │  │    "data": {                                                 │  │
    │  │      "harga_otr": 200000000,                                 │  │
    │  │      "dp": {                                                 │  │
    │  │      │   "persen": 15,                                       │  │
    │  │      │   "jumlah": 30000000,                                 │  │
    │  │      │   "minimum_npf": 15                                   │  │
    │  │      │   },                                                  │  │
    │  │      │   "pinjaman": {                                       │  │
    │  │      │   "jumlah": 170000000,                                │  │
    │  │      │   "bunga_tahunan": 8,                                 │  │
    │  │      │   "tenor_bulan": 48                                   │  │
    │  │      │   },                                                  │  │
    │  │      │   "angsuran": {                                       │  │
    │  │      │   "per_bulan": 4675000,                               │  │
    │  │      │   "pokok_per_bulan": 3541667,                        │  │
    │  │      │   "bunga_per_bulan": 1133333                         │  │
    │  │      │   },                                                  │  │
    │  │      │   "total": {                                          │  │
    │  │      │   "bunga": 54400000,                                  │  │
    │  │      │   "pembayaran": 254400000                            │  │
    │  │      │   },                                                  │  │
    │  │      │   "metadata": {                                       │  │
    │  │      │   "leasing_id": "uuid",                              │  │
    │  │      │   "npf_leasing": 2.5,                                │  │
    │  │      │   "kolektibilitas_ideb": 1,                          │  │
    │  │      │   "credit_score": 750                                 │  │
    │  │      │   "jenis_bunga": "FLAT"                              │  │
    │  │      │   }                                                   │  │
    │  │    }                                                          │  │
    │  │  }                                                            │  │
    │  └───────────────────────────────────────────────────────────────┘  │
    └───────────────────────────────┬─────────────────────────────────────┘
                                    │
                                    ▼
                            ┌───────────────┐
                            │ ✅ SELESAI   │
                            └───────────────┘
```

---

## 3.2.4 CONTOH SIMULASI KREDIT LENGKAP

```
╔══════════════════════════════════════════════════════════════════════════════╗
║                   CONTOH SIMULASI KREDIT KENDARAAN LENGKAP                  ║
╠══════════════════════════════════════════════════════════════════════════════╣
║                                                                              ║
║  STUDI KASUS: BELI MOBIL TOYOTA AVANZA via KREDIT                           ║
║  ═══════════════════════════════════════════════════════════════════════════ ║
║                                                                              ║
║  📋 DATA INPUT:                                                              ║
║  ┌────────────────────────────────────────────────────────────────────────┐  ║
║  │  PEMBELI:                                                             │  ║
║  │  • NIK                 : 3275XXXXXXXXXXXXX                           │  ║
║  │  • Nama                : John Doe                                     │  ║
║  │  • Tanggal Lahir       : 01-01-1990                                   │  ║
║  │                                                                         │  ║
║  │  KENDARAAN:                                                           │  ║
║  │  • Merk/Model          : Toyota Avanza 1.5 G AT                        │  ║
║  │  • Tahun               : 2024                                          │  ║
║  │  • Harga OTR           : Rp200.000.000                                 │  ║
║  │                                                                         │  ║
║  │  PARAMETER KREDIT:                                                    │  ║
║  │  • DP Request          : 15%                                           │  ║
║  │  • Tenor Request       : 48 bulan                                     │  ║
║  │  • Leasing             : ACC (Astra Credit Companies)                  │  ║
║  └────────────────────────────────────────────────────────────────────────┘  ║
║                                                                              ║
║  ─────────────────────────────────────────────────────────────────────────── ║
║                                                                              ║
║  📊 HASIL QUERY DATA:                                                        ║
║  ┌────────────────────────────────────────────────────────────────────────┐  ║
║  │                                                                         │  ║
║  │  SLIK/iDeb Response:                                                  │  ║
║  │  ┌───────────────────────────────────────────────────────────────┐   │  ║
║  │  │  kolektibilitas = 1 (LANCAR)                                │   │  ║
║  │  │  credit_score = 750                                         │   │  ║
║  │  │  // Skor baik, eligible kredit                              │   │  ║
║  │  └───────────────────────────────────────────────────────────────┘   │  ║
║  │                                                                         │  ║
║  │  NPF Leasing ACC:                                                     │  ║
║  │  ┌───────────────────────────────────────────────────────────────┐   │  ║
║  │  │  NPF Net = 2.5%                                              │   │  ║
║  │  │  // Dalam range 1% < NPF ≤ 3% → DP Minimum = 10%           │   │  ║
║  │  │  // Tapi user minta 15% > 10%, jadi DP 15% OK              │   │  ║
║  │  └───────────────────────────────────────────────────────────────┘   │  ║
║  │                                                                         │  ║
║  └────────────────────────────────────────────────────────────────────────┘  ║
║                                                                              ║
║  ─────────────────────────────────────────────────────────────────────────── ║
║                                                                              ║
║  📊 KALKULASI KREDIT:                                                        ║
║  ┌────────────────────────────────────────────────────────────────────────┐  ║
║  │                                                                         │  ║
║  │  1. DP (Uang Muka)                                                    │  ║
║  │     ┌─────────────────────────────────────────────────────────────┐  │  ║
║  │     │  DP = 15% × Rp200.000.000 = Rp30.000.000                   │  │  ║
║  │     └─────────────────────────────────────────────────────────────┘  │  ║
║  │                                                                         │  ║
║  │  2. Pinjaman                                                          │  ║
║  │     ┌─────────────────────────────────────────────────────────────┐  │  ║
║  │     │  Pinjaman = Rp200.000.000 - Rp30.000.000                   │  │  ║
║  │     │             = Rp170.000.000                                 │  │  ║
║  │     └─────────────────────────────────────────────────────────────┘  │  ║
║  │                                                                         │  ║
║  │  3. Bunga (Flat Rate 8% per tahun)                                   │  ║
║  │     ┌─────────────────────────────────────────────────────────────┐  │  ║
║  │     │  Bunga/Bulan = Rp170.000.000 × 8% / 12                      │  │  ║
║  │     │               = Rp170.000.000 × 0.006667                     │  │  ║
║  │     │               = Rp1.133.333/bulan                           │  │  ║
║  │     │                                                               │  │  ║
║  │     │  Total Bunga = Rp1.133.333 × 48 bulan                       │  │  ║
║  │     │               = Rp54.400.000                                 │  │  ║
║  │     └─────────────────────────────────────────────────────────────┘  │  ║
║  │                                                                         │  ║
║  │  4. Angsuran Bulanan                                                  │  ║
║  │     ┌─────────────────────────────────────────────────────────────┐  │  ║
║  │     │  Pokok/Bulan = Rp170.000.000 / 48 = Rp3.541.667            │  │  ║
║  │     │  Bunga/Bulan = Rp1.133.333                                  │  │  ║
║  │     │  ───────────────────────────────────────                    │  │  ║
║  │     │  Angsuran = Rp4.675.000/bulan                               │  │  ║
║  │     └─────────────────────────────────────────────────────────────┘  │  ║
║  │                                                                         │  ║
║  └────────────────────────────────────────────────────────────────────────┘  ║
║                                                                              ║
║  ═══════════════════════════════════════════════════════════════════════════ ║
║                                                                              ║
║  💰 RINGKASAN TOTAL PEMBAYARAN:                                              ║
║  ┌────────────────────────────────────────────────────────────────────────┐  ║
║  │                                                                         │  ║
║  │     KOMPONEN                    JUMLAH                                  │  ║
║  │     ─────────────────────────────────────────────────────────────     │  ║
║  │     DP (Uang Muka)            Rp30.000.000                           │  ║
║  │     Pokok Pinjaman            Rp170.000.000                           │  ║
║  │     Total Bunga (48 bulan)    Rp54.400.000                           │  ║
║  │     ─────────────────────────────────────────────────────────────     │  ║
║  │                                                                         │  ║
║  │     TOTAL PEMBAYARAN KREDIT  Rp254.400.000                           │  ║
║  │                                                                         │  ║
║  │     Angsuran/Bulan          Rp4.675.000                              │  ║
║  │                                                                         │  ║
║  └────────────────────────────────────────────────────────────────────────┘  ║
║                                                                              ║
║  📊 PERBANDINGAN JIKA NPF BERBEDA:                                           ║
║  ┌────────────────────────────────────────────────────────────────────────┐  ║
║  │                                                                         │  ║
║  │     ┌──────────────┬───────────┬───────────────┬───────────────────┐   │  ║
║  │     │ NPF Leasing  │ DP Min   │ DP (Rp)       │ Total Pembayaran │   │  ║
║  │     ├──────────────┼───────────┼───────────────┼───────────────────┤   │  ║
║  │     │ ≤ 1%         │ 0%       │ Rp0           │ Rp224.400.000    │   │  ║
║  │     │ 1% - 3%      │ 10%      │ Rp20.000.000  │ Rp244.400.000    │   │  ║
║  │     │ 3% - 5%      │ 15%      │ Rp30.000.000  │ Rp254.400.000    │   │  ║
║  │     │ > 5%         │ 20%      │ Rp40.000.000  │ Rp264.400.000    │   │  ║
║  │     └──────────────┴───────────┴───────────────┴───────────────────┘   │  ║
║  │                                                                         │  ║
║  │     💡 Semakin rendah NPF leasing, semakin kecil DP minimum!           │  ║
║  │                                                                         │  ║
║  └────────────────────────────────────────────────────────────────────────┘  ║
║                                                                              ║
╚══════════════════════════════════════════════════════════════════════════════╝
```

---

# BAGIAN 3.3: VALIDASI DOKUMEN

> **Lokasi di SRS:** Sub-bab 3.4 (Duplikat)
> **Lokasi Flowchart:** BusinessLogic-MermaidJS.md → Section 3: Validasi Dokumen

---

## 3.3.1 GLOSARIUM ISTILAH VALIDASI DOKUMEN

```
╔══════════════════════════════════════════════════════════════════════════════╗
║                    GLOSARIUM ISTILAH VALIDASI DOKUMEN                      ║
╠══════════════════════════════════════════════════════════════════════════════╣
║                                                                              ║
║  1. Hologram Tri Brata                                                     ║
║  ─────────────────────                                                      ║
║     DEFINISI:                                                                ║
║     Fitur keamanan pada BPKB berupa gambar 3D yang berubah warna dilihat    ║
║     dari sudut berbeda. Hologram ini diproduksi oleh Peruri.               ║
║                                                                              ║
║     CIRI KHAS:                                                               ║
║     • Terdiri dari 3 segitiga yang tersusun membentuk logo                  ║
║     • Berwarna pelangi (rainbow effect)                                     ║
║     • Berubah warna saat dilihat dari sudut berbeda                        ║
║     • Terdapat text "BPKB" yang tersembunyi                                ║
║                                                                              ║
║     AKURASI DETEKSI:                                                        ║
║     Target sistem: ≥ 95% akurasi deteksi hologram                          ║
║                                                                              ║
╠══════════════════════════════════════════════════════════════════════════════╣
║                                                                              ║
║  2. Benang UV Sicherheit                                                   ║
║  ───────────────────────────────                                           ║
║     DEFINISI:                                                                ║
║     Benang pengaman yang tertanam di kertas BPKB. Benang ini hanya         ║
║     terlihat di bawah sinar ultraviolet.                                   ║
║                                                                              ║
║     CARA VERIFIKASI:                                                        ║
║     1. Nyalakan lampu UV (panjang gelombang 365nm atau 395nm)             ║
║     2. Arahkan ke halaman BPKB                                             ║
║     3. Benang akan berpendar hijau/kuning                                   ║
║     4. Jika tidak ada benang → kemungkinan palsu                           ║
║                                                                              ║
╠══════════════════════════════════════════════════════════════════════════════╣
║                                                                              ║
║  3. Luhn Checksum (Validasi NPWP)                                         ║
║  ─────────────────────────────────                                         ║
║     DEFINISI:                                                                ║
║     Algoritma matematika untuk memvalidasi nomor identifikasi. NPWP          ║
║     Indonesia menggunakan Luhn checksum untuk digit terakhir.                ║
║                                                                              ║
║     FORMAT NPWP:                                                            ║
║     XX.XXX.XXX.X-XXX.XXX (15 digit)                                       ║
║     ┌──────────────────────────────────────────────────────────────────┐    ║
║     │  Digit 1-2 : Kode Provinsi                                      │    ║
║     │  Digit 3-5 : Kode KPP (Kantor Pajak)                           │    ║
║     │  Digit 6-9 : Nomor Seri                                        │    ║
║     │  Digit 10  : Kode Status (Wajib Pajak)                         │    ║
║     │  Digit 11-15: Digits acak dengan Luhn checksum di digit 15      │    ║
║     └──────────────────────────────────────────────────────────────────┘    ║
║                                                                              ║
╠══════════════════════════════════════════════════════════════════════════════╣
║                                                                              ║
║  4. Faktur Kendaraan                                                        ║
║  ─────────────────                                                         ║
║     DEFINISI:                                                                ║
║     Dokumen bukti pembelian kendaraan dari dealer. Faktur asli diperlukan   ║
║     untuk proses balik nama.                                                 ║
║                                                                              ║
║     KANDUNGAN FAKTUR:                                                       ║
║     • Nama dealer & alamat                                                  ║
║     • Nama pembeli                                                         ║
║     • Detail kendaraan (merk, model, nomor rangka, mesin)                ║
║     • Harga jual                                                           ║
║     • Tanggal                                                             ║
║     • Tanda tangan & cap dealer                                            ║
║                                                                              ║
╠══════════════════════════════════════════════════════════════════════════════╣
║                                                                              ║
║  5. SRUT (Surat Registrasi Uji Tipe)                                      ║
║  ─────────────────────────────────────────                                  ║
║     DEFINISI:                                                                ║
║     Sertifikat dari Gaikindo yang menyatakan kendaraan telah lulus uji     ║
║     tipe dan boleh dijual di Indonesia.                                      ║
║                                                                              ║
║     KEGUNAAN:                                                               ║
║     • Wajib untuk kendaraan baru                                            ║
║     • Bukti legalitas kendaraanimport                                     ║
║                                                                              ║
╠══════════════════════════════════════════════════════════════════════════════╣
║                                                                              ║
║  6. Form A (Tingkat Komponen Dalam Negeri)                                  ║
║  ─────────────────────────────────────────────                              ║
║     DEFINISI:                                                                ║
║     Dokumen dari Bea Cukai yang menyatakan tingkat komponen dalam negeri   ║
║     (TKDN) kendaraan impor CBU (Completely Built Up).                      ║
║                                                                              ║
║     KAPAN DIPERLUKAN:                                                       ║
║     • Kendaraan impor CBU (unit utuh dari luar negeri)                     ║
║     • Untuk hitung TKDN gabungan (lokal + impor)                           ║
║                                                                              ║
╠══════════════════════════════════════════════════════════════════════════════╣
║                                                                              ║
║  7. SPH (Surat Pelepasan Hak)                                              ║
║  ─────────────────────────────────                                         ║
║     DEFINISI:                                                                ║
║     Dokumen pelepasan aset dari perusahaan. Diperlukan saat kendaraan       ║
║     bekas merupakan eks-aset perusahaan.                                    ║
║                                                                              ║
║     KAPAN DIPERLUKAN:                                                       ║
║     • Kendaraan bekas milik perusahaan/pt                                    ║
║     • Aset yang sudah di-depresiasi                                         ║
║                                                                              ║
╠══════════════════════════════════════════════════════════════════════════════╣
║                                                                              ║
║  8. Kwitansi Bermaterai                                                    ║
║  ────────────────────────                                                   ║
║     DEFINISI:                                                                ║
║     Bukti pembayaran yang materai untuk transaksi di atas Rp5.000.000.       ║
║     Untuk BBN-II, diperlukan kwitansi bermaterai cukup.                     ║
║                                                                              ║
║     SYARAT KWITANSI BEKAS:                                                  ║
║     • 2-3 rangkap                                                          ║
║     • Ditandatangani sesuai nama di BPKB                                   ║
║     • Bermaterai cukup                                                     ║
║                                                                              ║
╠══════════════════════════════════════════════════════════════════════════════╣
║                                                                              ║
║  9. OBR (Otorisasi Berex)                                                  ║
║  ─────────────────                                                         ║
║     DEFINISI:                                                                ║
║     Surat persetujuan export dari Ditjen Bea Cukai untuk kendaraan impor.   ║
║                                                                              ║
║  10. STCK (Surat Tanda Coba Keliling)                                      ║
║  ─────────────────────────────────────                                      ║
║     DEFINISI:                                                                ║
║     Plat nomor sementara berwarna merah-putih untuk kendaraan yang belum   ║
║     memiliki plat definitif.                                                 ║
║                                                                              ║
║     ATURAN:                                                                 ║
║     • Masa berlaku: 1 bulan                                                ║
║     • Hanya untuk logistik & uji fisik                                      ║
║     • Dilarang untuk keperluan pribadi                                     ║
║     • Dilarang keluar kota                                                  ║
║                                                                              ║
╚══════════════════════════════════════════════════════════════════════════════╝
```

---

## 3.3.2 FLOWCHART MATRIKS VALIDASI BERDASARKAN JENIS KENDARAAN

```
┌─────────────────────────────────────────────────────────────────────────────┐
│        FLOWCHART MATRIKS VALIDASI DOKUMEN (VERSI DETAIL)                    │
└─────────────────────────────────────────────────────────────────────────────┘

═══════════════════════════════════════════════════════════════════════════════
KATEGORI A: KENDARAAN BARU
═══════════════════════════════════════════════════════════════════════════════

    ┌─────────────────────────────────────────────────────────────────────┐
    │  START: Validasi Kendaraan Baru                                     │
    └───────────────────────────────┬─────────────────────────────────────┘
                                    │
                                    ▼
    ┌─────────────────────────────────────────────────────────────────────┐
    │  CHECKLIST DOKUMEN MOBIL BARU:                                     │
    │  ┌───────────────────────────────────────────────────────────────┐  │
    │  │                                                               │  │
    │  │  DOKUMEN WAJIB #1: Faktur Kendaraan                         │  │
    │  │  ┌─────────────────────────────────────────────────────┐   │  │
    │  │  │  STATUS: ❗ HARUS ADA (ASLI)                         │   │  │
    │  │  │                                                       │   │  │
    │  │  │  VALIDASI:                                           │   │  │
    │  │  │  ✓ Cek apakah nomer faktur ada di database Gaikindo  │   │  │
    │  │  │  ✓ Cek tanggal faktur (tidak boleh expired)          │   │  │
    │  │  │  ✓ Cocokkan nama dealer dengan sistem                 │   │  │
    │  │  │  ✓ Cocokkan harga dengan NJKB                        │   │  │
    │  │  │                                                       │   │  │
    │  │  │  IF faktur_valid THEN                                 │   │  │
    │  │  │     → PROCEED TO: DOKUMEN #2                          │   │  │
    │  │  │  ELSE                                                  │   │  │
    │  │  │     RETURN ERROR("Faktur tidak valid")                 │   │  │
    │  │  │  END IF                                                │   │  │
    │  │  └─────────────────────────────────────────────────────┘   │  │
    │  │                                                               │  │
    │  └───────────────────────────────────────────────────────────────┘  │
    └───────────────────────────────┬─────────────────────────────────────┘
                                    │
                                    ▼
    ┌─────────────────────────────────────────────────────────────────────┐
    │  DOKUMEN WAJIB #2: SRUT (Surat Registrasi Uji Tipe)                 │
    │  ┌───────────────────────────────────────────────────────────────┐  │
    │  │                                                               │  │
    │  │  STATUS: ❗ WAJIB                                              │  │
    │  │  (Surat dari Gaikindo)                                        │  │
    │  │                                                               │  │
    │  │  VALIDASI:                                                   │  │
    │  │  ✓ Cek apakah SRUT terdaftar di database Gaikindo             │  │
    │  │  ✓ Cocokkan nomor SRUT dengan kendaraan                      │  │
    │  │  ✓ Cek tanggal berlaku                                        │  │
    │  │                                                               │  │
    │  │  IF srut_valid THEN                                          │  │
    │  │     → PROCEED TO: DOKUMEN #3                                  │  │
    │  │  ELSE                                                         │  │
    │  │     RETURN ERROR("SRUT tidak ditemukan")                      │  │
    │  │  END IF                                                       │  │
    │  │                                                               │  │
    │  └───────────────────────────────────────────────────────────────┘  │
    └───────────────────────────────┬─────────────────────────────────────┘
                                    │
                                    ▼
    ┌─────────────────────────────────────────────────────────────────────┐
    │  DOKUMEN WAJIB #3: KTP Pembeli                                     │
    │  ┌───────────────────────────────────────────────────────────────┐  │
    │  │                                                               │  │
    │  │  STATUS: ❗ WAJIB                                             │  │
    │  │                                                               │  │
    │  │  VALIDASI:                                                   │  │
    │  │  ✓ Cek format NIK (16 digit)                                 │  │
    │  │  ✓ Validasi Luhn Checksum NIK                                │  │
    │  │  ✓ Face matching dengan swafoto                               │  │
    │  │  ✓ Masa berlaku KTP (belum expired)                          │  │
    │  │                                                               │  │
    │  │  IF face_match_score >= 85% THEN                             │  │
    │  │     → PROCEED TO: DOKUMEN #4                                  │  │
    │  │  ELSE                                                         │  │
    │  │     RETURN ERROR("Face matching gagal, kecocokan < 85%")     │  │
    │  │  END IF                                                       │  │
    │  │                                                               │  │
    │  └───────────────────────────────────────────────────────────────┘  │
    └───────────────────────────────┬─────────────────────────────────────┘
                                    │
                                    ▼
    ┌─────────────────────────────────────────────────────────────────────┐
    │  DECISION: Apakah Kendaraan CBU/Impor?                               │
    │  ┌───────────────────────────────────────────────────────────────┐  │
    │  │                                                               │  │
    │  │  IF jenis_import == "CBU" THEN                              │  │
    │  │     → PROCEED TO: DOKUMEN #4A (FORM A)                       │  │
    │  │  ELSE                                                         │  │
    │  │     → PROCEED TO: VALIDATION_SUCCESS                         │  │
    │  │  END IF                                                        │  │
    │  │                                                               │  │
    │  └───────────────────────────────────────────────────────────────┘  │
    └───────────────────────────────┬─────────────────────────────────────┘
                                    │
                                    ▼
    ┌─────────────────────────────────────────────────────────────────────┐
    │  DOKUMEN #4A: Form A (KHUSUS CBU/IMPOR)                            │
    │  ┌───────────────────────────────────────────────────────────────┐  │
    │  │                                                               │  │
    │  │  STATUS: ❗ WAJIB (KHUSUS IMPOR CBU)                          │  │
    │  │  (Surat dari Bea Cukai)                                       │  │
    │  │                                                               │  │
    │  │  KANDUNGAN:                                                   │  │
    │  │  • TKDN gabungan (lokal + impor)                              │  │
    │  │  • Nomor formulario                                           │  │
    │  │  • Tanggal                                                    │  │
    │  │                                                               │  │
    │  │  VALIDASI:                                                   │  │
    │  │  ✓ Cek apakah Form A terdaftar di sistem                      │  │
    │  │  ✓ Cocokkan dengan nomor chassis                              │  │
    │  │  ✓ Verifikasi nilai TKDN                                      │  │
    │  │                                                               │  │
    │  │  IF form_a_valid THEN                                         │  │
    │  │     → PROCEED TO: VALIDATION_SUCCESS                          │  │
    │  │  ELSE                                                         │  │
    │  │     RETURN ERROR("Form A tidak valid untuk unit ini")         │  │
    │  │  END IF                                                       │  │
    │  │                                                               │  │
    │  └───────────────────────────────────────────────────────────────┘  │
    └───────────────────────────────┬─────────────────────────────────────┘
                                    │
                                    ▼

═══════════════════════════════════════════════════════════════════════════════
KATEGORI B: KENDARAAN BEKAS (LUNAS)
═══════════════════════════════════════════════════════════════════════════════

    ┌─────────────────────────────────────────────────────────────────────┐
    │  START: Validasi Kendaraan Bekas (Lunas)                           │
    └───────────────────────────────┬─────────────────────────────────────┘
                                    │
                                    ▼
    ┌─────────────────────────────────────────────────────────────────────┐
    │  CHECKLIST DOKUMEN MOBIL BEKAS LUNAS:                             │
    │  ┌───────────────────────────────────────────────────────────────┐  │
    │  │                                                               │  │
    │  │  DOKUMEN WAJIB #1: BPKB Asli                                 │  │
    │  │  ┌─────────────────────────────────────────────────────┐   │  │
    │  │  │  STATUS: ❗ WAJIB (ASLI)                               │   │  │
    │  │  │                                                       │   │  │
    │  │  │  VALIDASI 1: Cek Fisik                                 │   │  │
    │  │  │  ✓ Hologram Tri Brata terdeteksi (akurasi ≥ 95%)       │   │  │
    │  │  │  ✓ Benang UV Sicherheit terlihat                       │   │  │
    │  │  │  ✓ Watermark "BPKB" ada                                │   │  │
    │  │  │  ✓ Kertas asli (bukan fotokopian)                      │   │  │
    │  │  │                                                       │   │  │
    │  │  │  VALIDASI 2: Cek Data                                   │   │  │
    │  │  │  ✓ Nomor Rangka cocok dengan fisik                      │   │  │
    │  │  │  ✓ Nomor Mesin cocok dengan fisik                       │   │  │
    │  │  │  ✓ Nama pemilik cocok dengan KTP                        │   │  │
    │  │  │  ✓ Tidak ada catatan blokir                            │   │  │
    │  │  │                                                       │   │  │
    │  │  │  IF bpkb_valid THEN                                    │   │  │
    │  │  │     → PROCEED TO: DOKUMEN #2                            │   │  │
    │  │  │  ELSE                                                   │   │  │
    │  │  │     RETURN ERROR("BPKB tidak valid/palsu")               │   │  │
    │  │  │  END IF                                                 │   │  │
    │  │  └─────────────────────────────────────────────────────┘   │  │
    │  │                                                               │  │
    │  └───────────────────────────────────────────────────────────────┘  │
    └───────────────────────────────┬─────────────────────────────────────┘
                                    │
                                    ▼
    ┌─────────────────────────────────────────────────────────────────────┐
    │  DOKUMEN WAJIB #2: STNK Aktif                                      │
    │  ┌───────────────────────────────────────────────────────────────┐  │
    │  │                                                               │  │
    │  │  STATUS: ❗ WAJIB (AKTIF)                                       │  │
    │  │                                                               │  │
    │  │  VALIDASI:                                                   │  │
    │  │  ✓ Masa berlaku belum expired                                 │  │
    │  │  ✓ Pajak tahun berjalan lunas (cek Samsat)                    │  │
    │  │  ✓ Tidak ada blokir ETLE                                     │  │
    │  │  ✓ Nomor polisi cocok dengan STNK                             │  │
    │  │                                                               │  │
    │  │  api_response = CALL_SAMSAT_API(                             │  │
    │  │      endpoint = "/cek_status",                               │  │
    │  │      nomor_polisi = nomor_polisi,                            │  │
    │  │      nik = nik_pemilik                                      │  │
    │  │  )                                                            │  │
    │  │                                                               │  │
    │  │  IF stnk_valid AND api_response.status == "AKTIF" THEN      │  │
    │  │     → PROCEED TO: DOKUMEN #3                                  │  │
    │  │  ELSE                                                         │  │
    │  │     RETURN ERROR("STNK tidak aktif atau ada blokir")         │  │
    │  │  END IF                                                       │  │
    │  │                                                               │  │
    │  └───────────────────────────────────────────────────────────────┘  │
    └───────────────────────────────┬─────────────────────────────────────┘
                                    │
                                    ▼
    ┌─────────────────────────────────────────────────────────────────────┐
    │  DOKUMEN WAJIB #3: Faktur Pembelian                                 │
    │  ┌───────────────────────────────────────────────────────────────┐  │
    │  │                                                               │  │
    │  │  STATUS: ❗ WAJIB (ASLI)                                       │  │
    │  │  (Bukti pembelian sebelumnya)                                │  │
    │  │                                                               │  │
    │  │  VALIDASI:                                                   │  │
    │  │  ✓ Faktur asli atas nama penjual                             │  │
    │  │  ✓ Tanggal sesuai (tidak expired)                           │  │
    │  │  ✓ Detail kendaraan cocok dengan BPKB                       │  │
    │  │                                                               │  │
    │  │  IF faktur_valid THEN                                        │  │
    │  │     → PROCEED TO: DOKUMEN #4                                  │  │
    │  │  ELSE                                                         │  │
    │  │     RETURN ERROR("Faktur tidak valid")                        │  │
    │  │  END IF                                                       │  │
    │  │                                                               │  │
    │  └───────────────────────────────────────────────────────────────┘  │
    └───────────────────────────────┬─────────────────────────────────────┘
                                    │
                                    ▼
    ┌─────────────────────────────────────────────────────────────────────┐
    │  DOKUMEN WAJIB #4: Kwitansi Kosong Bermaterai                       │
    │  ┌───────────────────────────────────────────────────────────────┐  │
    │  │                                                               │  │
    │  │  STATUS: ❗ WAJIB (2-3 RANGKAP)                                │  │
    │  │                                                               │  │
    │  │  SYARAT:                                                     │  │
    │  │  ✓ 2-3 lembar kwitansi kosong                                │  │
    │  │  ✓ Ditandatangani sesuai nama di BPKB                        │  │
    │  │  ✓ Bermaterai cukup (Rp10.000)                              │  │
    │  │  ✓一股Ditandatangani di atas materai                         │  │
    │  │                                                               │  │
    │  │  IF kwitansi_valid THEN                                      │  │
    │  │     → PROCEED TO: DECISION_EKS_PERUSAHAAN                     │  │
    │  │  ELSE                                                         │  │
    │  │     RETURN ERROR("Kwitansi tidak sesuai format")             │  │
    │  │  END IF                                                       │  │
    │  │                                                               │  │
    │  └───────────────────────────────────────────────────────────────┘  │
    └───────────────────────────────┬─────────────────────────────────────┘
                                    │
                                    ▼
    ┌─────────────────────────────────────────────────────────────────────┐
    │  DECISION: Eks Aset Perusahaan?                                     │
    │  ┌───────────────────────────────────────────────────────────────┐  │
    │  │                                                               │  │
    │  │  IF is_eks_perusahaan == TRUE THEN                           │  │
    │  │     → PROCEED TO: DOKUMEN #5A (SPH)                           │  │
    │  │  ELSE                                                         │  │
    │  │     → PROCEED TO: VALIDATION_SUCCESS                          │  │
    │  │  END IF                                                       │  │
    │  │                                                               │  │
    │  └───────────────────────────────────────────────────────────────┘  │
    └───────────────────────────────┬─────────────────────────────────────┘
                                    │
                                    ▼
    ┌─────────────────────────────────────────────────────────────────────┐
    │  DOKUMEN #5A: SPH (Surat Pelepasan Hak)                            │
    │  ┌───────────────────────────────────────────────────────────────┐  │
    │  │                                                               │  │
    │  │  STATUS: ❗ WAJIB (KHUSUS EKS ASET PERUSAHAAN)                 │  │
    │  │                                                               │  │
    │  │  VALIDASI:                                                   │  │
    │  │  ✓ Surat asli dari perusahaan                                 │  │
    │  │  ✓ Tanda tangan direksi/perusahaan                            │  │
    │  │  ✓ Cap perusahaan                                            │  │
    │  │  ✓ Detail kendaraan cocok dengan BPKB                        │  │
    │  │                                                               │  │
    │  │  IF sph_valid THEN                                            │  │
    │  │     → PROCEED TO: VALIDATION_SUCCESS                          │  │
    │  │  ELSE                                                         │  │
    │  │     RETURN ERROR("SPH tidak valid untuk unit ini")            │  │
    │  │  END IF                                                       │  │
    │  │                                                               │  │
    │  └───────────────────────────────────────────────────────────────┘  │
    └───────────────────────────────┬─────────────────────────────────────┘
                                    │
                                    ▼

═══════════════════════════════════════════════════════════════════════════════
KATEGORI C: KENDARAAN BEKAS (MASIH KREDIT)
═══════════════════════════════════════════════════════════════════════════════

    ┌─────────────────────────────────────────────────────────────────────┐
    │  START: Validasi Kendaraan Bekas (Masih Kredit)                      │
    └───────────────────────────────┬─────────────────────────────────────┘
                                    │
                                    ▼
    ┌─────────────────────────────────────────────────────────────────────┐
    │  CHECKLIST TAMBAHAN (SELAIN DOKUMEN LUNAS):                         │
    │  ┌───────────────────────────────────────────────────────────────┐  │
    │  │                                                               │  │
    │  │  DOKUMEN WAJIB #1: Kontrak Leasing Lama                       │  │
    │  │  ┌─────────────────────────────────────────────────────┐   │  │
    │  │  │  STATUS: ❗ WAJIB                                       │   │  │
    │  │  │                                                       │   │  │
    │  │  │  VALIDASI:                                           │   │  │
    │  │  │  ✓ Kontrak asli dari leasing lama                     │   │  │
    │  │  │  ✓ Nomor kontrak cocok dengan data leasing            │   │  │
    │  │  │  ✓ Identitas borrower cocok                          │   │  │
    │  │  │                                                       │   │  │
    │  │  │  IF kontrak_valid THEN                                │   │  │
    │  │  │     → PROCEED TO: DOKUMEN #2                          │   │  │
    │  │  │  ELSE                                                 │   │  │
    │  │  │     RETURN ERROR("Kontrak leasing tidak ditemukan")   │   │  │
    │  │  │  END IF                                               │   │  │
    │  │  └─────────────────────────────────────────────────────┘   │  │
    │  │                                                               │  │
    │  └───────────────────────────────────────────────────────────────┘  │
    └───────────────────────────────┬─────────────────────────────────────┘
                                    │
                                    ▼
    ┌─────────────────────────────────────────────────────────────────────┐
    │  CEK STATUS PELUNASAN:                                               │
    │  ┌───────────────────────────────────────────────────────────────┐  │
    │  │                                                               │  │
    │  │  api_response = CALL_LEASING_LAMA(                           │  │
    │  │      endpoint = "/cek_status_kredit",                        │  │
    │  │      nomor_kontrak = nomor_kontrak                           │  │
    │  │  )                                                            │  │
    │  │                                                               │  │
    │  │  IF api_response.status == "LUNAS" THEN                      │  │
    │  │     // Dapat proceed dengan dokumen lunas                    │  │
    │  │     → PROCEED TO: VALIDATION_SUCCESS                          │  │
    │  │  ELSIF api_response.status == "AKTIF" THEN                   │  │
    │  │     // Ada kredit aktif, cek overdue                        │  │
    │  │     → PROCEED TO: CHECK_OVERDUE                               │  │
    │  │  END IF                                                       │  │
    │  │                                                               │  │
    │  └───────────────────────────────────────────────────────────────┘  │
    └───────────────────────────────┬─────────────────────────────────────┘
                                    │
                                    ▼
    ┌─────────────────────────────────────────────────────────────────────┐
    │  CHECK_OVERDUE: Cek Tunggakan                                        │
    │  ┌───────────────────────────────────────────────────────────────┐  │
    │  │                                                               │  │
    │  │  IF api_response.overdue_days > 0 THEN                       │  │
    │  │     // Ada tunggakan                                           │  │
    │  │     ┌─────────────────────────────────────────────────────┐ │  │
    │  │     │  RETURN ERROR({                                       │ │  │
    │  │     │      "code": "OVERDUE_ACTIVE",                       │ │  │
    │  │     │      "message": "Kredit sebelumnya masih memiliki    │ │  │
    │  │     │               "tunggakan. Tidak dapat melanjutkan."  │ │  │
    │  │     │      "overdue_days": api_response.overdue_days,      │ │  │
    │  │     │      "overdue_amount": api_response.overdue_amount   │ │  │
    │  │     │  })                                                    │ │  │
    │  │     └─────────────────────────────────────────────────────┘ │  │
    │  │  ELSE                                                         │  │
    │  │     → PROCEED TO: VALIDATION_SUCCESS                         │  │
    │  │  END IF                                                        │  │
    │  │                                                               │  │
    │  └───────────────────────────────────────────────────────────────┘  │
    └───────────────────────────────┬─────────────────────────────────────┘
                                    │
                                    ▼
                            ┌───────────────┐
                            │ ✅ LOLOS     │
                            │ VALIDASI    │
                            └───────────────┘
```

---

## 3.3.3 ALGORITMA VALIDASI LUHN CHECKSUM NPWP (STEP-BY-STEP)

```
╔══════════════════════════════════════════════════════════════════════════════╗
║              ALGORITMA LUHN CHECKSUM VALIDASI NPWP (DETAIL)                ║
╠══════════════════════════════════════════════════════════════════════════════╣
║                                                                              ║
║  PENDAHULUAN:                                                               ║
║  ══════════════                                                             ║
║  NPWP Indonesia terdiri dari 15 digit dengan digit terakhir adalah          ║
║  checksum Luhn. Algoritma ini memvalidasi apakah nomor NPWP valid.          ║
║                                                                              ║
╠══════════════════════════════════════════════════════════════════════════════╣
║                                                                              ║
║  FORMAT NPWP:                                                               ║
║  ══════════════                                                             ║
║                                                                              ║
║     1  2  3  4  5  6  7  8  9  10 11 12 13 14 15                          ║
║     ↓  ↓  ↓  ↓  ↓  ↓  ↓  ↓  ↓  ↓   ↓   ↓   ↓   ↓   ↓                     ║
║    [XX][XXX][XXX][X]-[XXX][XXX]                                            ║
║     │   │    │    │      │     │                                             ║
║     │   │    │    │      │     └─ Digit ke-11 s/d 15 (Luhn checksum)        ║
║     │   │    │    │      └─ Digit ke-10 (Status Wajib Pajak)               ║
║     │   │    │    └─ Digit ke-6 s/d 9 (Nomor Seri)                         ║
║     │   │    └─ Digit ke-3 s/d 5 (Kode KPP)                               ║
║     └───────┴─ Digit ke-1 s/d 2 (Kode Provinsi)                            ║
║                                                                              ║
╠══════════════════════════════════════════════════════════════════════════════╣
║                                                                              ║
║  ALGORITMA LUHN:                                                            ║
║  ══════════════                                                             ║
║                                                                              ║
║  STEP 1: Preprocessing                                                      ║
║  ─────────────────                                                          ║
║  ┌──────────────────────────────────────────────────────────────────────┐   ║
║  │                                                                       │   ║
║  │  INPUT:                                                              │   ║
║  │  npwp_string = "02.123.456.7-890.123"                               │   ║
║  │                                                                       │   ║
║  │  STEP 1A: Hapus semua karakter non-digit                              │   ║
║  │  ┌──────────────────────────────────────────────────────────────┐  │   ║
║  │  │  digits_only = REMOVE_NON_DIGITS(npwp_string)                │  │   ║
║  │  │  // Result: "021234567890123"                               │  │   ║
║  │  └──────────────────────────────────────────────────────────────┘  │   ║
║  │                                                                       │   ║
║  │  STEP 1B: Validasi jumlah digit                                       │   ║
║  │  ┌──────────────────────────────────────────────────────────────┐  │   ║
║  │  │  IF LENGTH(digits_only) != 15 THEN                          │  │   ║
║  │  │     RETURN FALSE // NPWP harus 15 digit                     │  │   ║
║  │  │  END IF                                                       │  │   ║
║  │  └──────────────────────────────────────────────────────────────┘  │   ║
║  │                                                                       │   ║
║  │  STEP 1C: Konversi ke array integer                                   │   ║
║  │  ┌──────────────────────────────────────────────────────────────┐  │   ║
║  │  │  digits = [0, 2, 1, 2, 3, 4, 5, 6, 7, 8, 9, 0, 1, 2, 3]   │  │   ║
║  │  │  // Array of integers                                        │  │   ║
║  │  └──────────────────────────────────────────────────────────────┘  │   ║
║  │                                                                       │   ║
║  └──────────────────────────────────────────────────────────────────────┘   ║
║                                                                              ║
║  STEP 2: Inisialisasi Pengali                                              ║
║  ──────────────────────────────                                             ║
║  ┌──────────────────────────────────────────────────────────────────────┐   ║
║  │                                                                       │   ║
║  │  Untuk validasi NPWP Indonesia, digunakan pengali tetap:              │   ║
║  │                                                                       │   ║
║  │  ┌──────────────────────────────────────────────────────────────┐  │   ║
║  │  │  multiplier = [5, 7, 2, 3, 4, 5, 6, 7, 8, 9, 2, 3, 4, 5, 6]  │  │   ║
║  │  │   positions:   [1, 2, 3, 4, 5, 6, 7, 8, 9,10,11,12,13,14,15] │  │   ║
║  │  └──────────────────────────────────────────────────────────────┘  │   ║
║  │                                                                       │   ║
║  │  CATATAN: Pengali untuk posisi 1-9 sama untuk semua NPWP            │   ║
║  │            Pengali posisi 10-15 mengikuti pola (2,3,4,5,6)          │   ║
║  │                                                                       │   ║
║  └──────────────────────────────────────────────────────────────────────┘   ║
║                                                                              ║
║  STEP 3: Iterasi Perhitungan                                                ║
║  ───────────────────────────────                                             ║
║  ┌──────────────────────────────────────────────────────────────────────┐   ║
║  │                                                                       │   ║
║  │  Inisialisasi:                                                       │   ║
║  │  ┌──────────────────────────────────────────────────────────────┐  │   ║
║  │  │  total_sum = 0                                                │  │   ║
║  │  └──────────────────────────────────────────────────────────────┘  │   ║
║  │                                                                       │   ║
║  │  Iterasi untuk setiap digit (kecuali digit terakhir):              │   ║
║  │  ┌──────────────────────────────────────────────────────────────┐  │   ║
║  │  │                                                               │  │   ║
║  │  │  FOR i = 0 TO 13 (14 iterasi, digit 1-14):                  │  │   ║
║  │  │                                                               │  │   ║
║  │  │      product = digits[i] × multiplier[i]                     │  │   ║
║  │  │                                                               │  │   ║
║  │  │      // Jika hasil > 9, kurangi 9                           │  │   ║
║  │  │      IF product > 9 THEN                                     │  │   ║
║  │  │          product = product - 9                               │  │   ║
║  │  │      END IF                                                   │  │   ║
║  │  │                                                               │  │   ║
║  │  │      total_sum = total_sum + product                         │  │   ║
║  │  │                                                               │  │   ║
║  │  │  END FOR                                                      │  │   ║
║  │  │                                                               │  │   ║
║  │  └──────────────────────────────────────────────────────────────┘  │   ║
║  │                                                                       │   ║
║  └──────────────────────────────────────────────────────────────────────┘   ║
║                                                                              ║
║  STEP 4: Hitung Checksum                                                    ║
║  ──────────────────────                                                     ║
║  ┌──────────────────────────────────────────────────────────────────────┐   ║
║  │                                                                       │   ║
║  │  STEP 4A: Hitung Modulo 11                                            │   ║
║  │  ┌──────────────────────────────────────────────────────────────┐  │   ║
║  │  │  mod_11 = total_sum MOD 11                                   │  │   ║
║  │  └──────────────────────────────────────────────────────────────┘  │   ║
║  │                                                                       │   ║
║  │  STEP 4B: Hitung Expected Checksum                                   │   ║
║  │  ┌──────────────────────────────────────────────────────────────┐  │   ║
║  │  │  expected_checksum = (11 - mod_11) MOD 11                   │  │   ║
║  │  │  // Jika hasilnya 10, maka checksum = 0                     │  │   ║
║  │  │  IF expected_checksum == 10 THEN                            │  │   ║
║  │  │      expected_checksum = 0                                  │  │   ║
║  │  │  END IF                                                      │  │   ║
║  │  └──────────────────────────────────────────────────────────────┘  │  │   ║
║  │                                                                       │   ║
║  │  STEP 4C: Bandingkan dengan Digit Terakhir                           │   ║
║  │  ┌──────────────────────────────────────────────────────────────┐  │   ║
║  │  │  actual_checksum = digits[14]  // Digit ke-15               │  │   ║
║  │  │                                                               │  │   ║
║  │  │  IF expected_checksum == actual_checksum THEN               │  │   ║
║  │  │      RETURN TRUE // NPWP VALID                               │  │   ║
║  │  │  ELSE                                                        │  │   ║
║  │  │      RETURN FALSE // NPWP INVALID                            │  │   ║
║  │  │  END IF                                                       │  │   ║
║  │  └──────────────────────────────────────────────────────────────┘  │   ║
║  │                                                                       │   ║
║  └──────────────────────────────────────────────────────────────────────┘   ║
║                                                                              ║
╠══════════════════════════════════════════════════════════════════════════════╣
║                                                                              ║
║  CONTOH PERHITUNGAN LENGKAP:                                                ║
║  ══════════════════════════════════════════                                ║
║                                                                              ║
║  NPWP: 02.123.456.7-890.123                                                ║
║  ─────────────────────────────────────────                                 ║
║                                                                              ║
║  Step 1: Hapus karakter non-digit                                            ║
║  ┌──────────────────────────────────────────────────────────────────────┐   ║
║  │  Input: "02.123.456.7-890.123"                                    │   ║
║  │  Output: "021234567890123"                                         │   ║
║  └──────────────────────────────────────────────────────────────────────┘   ║
║                                                                              ║
║  Step 2: Konversi ke array digits                                            ║
║  ┌──────────────────────────────────────────────────────────────────────┐   ║
║  │  digits = [0, 2, 1, 2, 3, 4, 5, 6, 7, 8, 9, 0, 1, 2, 3]         │   ║
║  │                1  2  3  4  5  6  7  8  9 10 11 12 13 14 15       │   ║
║  └──────────────────────────────────────────────────────────────────────┘   ║
║                                                                              ║
║  Step 3: Iterasi Perhitungan (digit 1-14)                                    ║
║  ┌──────────────────────────────────────────────────────────────────────┐   ║
║  │  ┌────┬────────┬────────────┬────────────┬───────────┐               │   ║
║  │  │ i  │ digit  │ multiplier │  product  │ > 9?     │ total_sum    │   ║
║  │  ├────┼────────┼────────────┼────────────┼───────────┤               │   ║
║  │  │ 0  │   0    │     5      │    0×5=0   │    -      │      0       │   ║
║  │  │ 1  │   2    │     7      │    2×7=14  │ 14-9=5   │      5       │   ║
║  │  │ 2  │   1    │     2      │    1×2=2   │    -      │      7       │   ║
║  │  │ 3  │   2    │     3      │    2×3=6   │    -      │     13       │   ║
║  │  │ 4  │   3    │     4      │    3×4=12  │ 12-9=3   │     16       │   ║
║  │  │ 5  │   4    │     5      │    4×5=20  │ 20-9=11  │     27       │   ║
║  │  │ 6  │   5    │     6      │    5×6=30  │ 30-9=21  │     48       │   ║
║  │  │ 7  │   6    │     7      │    6×7=42  │ 42-9=33  │     81       │   ║
║  │  │ 8  │   7    │     8      │    7×8=56  │ 56-9=47  │    128       │   ║
║  │  │ 9  │   8    │     9      │    8×9=72  │ 72-9=63  │    191       │   ║
║  │  │10  │   9    │     2      │    9×2=18  │ 18-9=9   │    200       │   ║
║  │  │11  │   0    │     3      │    0×3=0   │    -      │    200       │   ║
║  │  │12  │   1    │     4      │    1×4=4   │    -      │    204       │   ║
║  │  │13  │   2    │     5      │    2×5=10  │ 10-9=1   │    205       │   ║
║  └────┴────────┴────────────┴────────────┴───────────┴───────────┘               │   ║
║                                                                              ║
║  Step 4: Hitung Checksum                                                    ║
║  ┌──────────────────────────────────────────────────────────────────────┐   ║
║  │                                                                       │   ║
║  │  total_sum = 205                                                    │   ║
║  │  mod_11 = 205 MOD 11 = 7                                            │   ║
║  │                                                                       │   ║
║  │  expected_checksum = (11 - 7) MOD 11 = 4                            │   ║
║  │  actual_checksum = digits[14] = 3                                    │   ║
║  │                                                                       │   ║
║  │  4 != 3                                                              │   ║
║  │                                                                       │   ║
║  │  ❌ HASIL: NPWP INVALID (Checksum tidak cocok)                        │   ║
║  │                                                                       │   ║
║  └──────────────────────────────────────────────────────────────────────┘   ║
║                                                                              ║
╠══════════════════════════════════════════════════════════════════════════════╣
║                                                                              ║
║  CONTOH NPWP VALID:                                                         ║
║  ══════════════════                                                        ║
║                                                                              ║
║  NPWP: 01.234.567.8-123.000                                                ║
║  ─────────────────────────────────────────                                 ║
║                                                                              ║
║  digits = [0, 1, 2, 3, 4, 5, 6, 7, 8, 1, 2, 3, 0, 0, 0]                  ║
║                                                                              ║
║  Perhitungan:                                                               ║
║  ┌──────────────────────────────────────────────────────────────────────┐   ║
║  │  total_sum = (0×5) + (1×7) + (2×2) + (3×3) + (4×4) + (5×5) +     │   ║
║  │             (6×6) + (7×7) + (8×8) + (1×9) + (2×2) + (3×3) +     │   ║
║  │             (0×4) + (0×5)                                           │   ║
║  │                                                                       │   ║
║  │           = 0 + 7 + 4 + 9 + 16 + 25 + 36 + 49 + 64 + 9 + 4 + 9 + 0 + 0   │   ║
║  │           = 232                                                      │   ║
║  │                                                                       │   ║
║  │  mod_11 = 232 MOD 11 = 1                                            │   ║
║  │  expected_checksum = (11 - 1) MOD 11 = 10 MOD 11 = 10              │   ║
║  │  expected_checksum = (expected_checksum == 10) ? 0 : expected_checksum    │   ║
║  │  expected_checksum = 0                                               │   ║
║  │                                                                       │   ║
║  │  actual_checksum = digits[14] = 0                                    │   ║
║  │                                                                       │   ║
║  │  0 == 0                                                              │   ║
║  │                                                                       │   ║
║  │  ✅ HASIL: NPWP VALID                                                │   ║
║  │                                                                       │   ║
║  └──────────────────────────────────────────────────────────────────────┘   ║
║                                                                              ║
╚══════════════════════════════════════════════════════════════════════════════╝
```

---

## 3.3.4 FLOWCHART DETEKSI HOLGRAM BPKB

```
╔══════════════════════════════════════════════════════════════════════════════╗
║                    FLOWCHART DETEKSI HOLGRAM BPKB (DETAIL)                  ║
╠══════════════════════════════════════════════════════════════════════════════╣
║                                                                              ║
║  TUJUAN: Mendeteksi hologram Tri Brata pada BPKB dengan akurasi ≥ 95%      ║
║                                                                              ║
╠══════════════════════════════════════════════════════════════════════════════╣
║                                                                              ║
║  STEP 1: AKUISISI GAMBAR                                                    ║
║  ══════════════════════════                                                ║
║                                                                              ║
║  ┌──────────────────────────────────────────────────────────────────────┐   ║
║  │                                                                       │   ║
║  │  INPUT:                                                              │   ║
║  │  • Foto/Scan halaman depan BPKB                                     │   ║
║  │  • Resolusi minimal: 300 DPI                                        │   ║
║  │  • Format: JPEG/PNG                                                  │   ║
║  │                                                                       │   ║
║  │  SUMBER GAMBAR:                                                      │   ║
║  │  • Kamera smartphone (dengan flash)                                  │   ║
║  │  • Scanner flatbed                                                   │   ║
║  │  • Kamera dokumen                                                     │   ║
║  │                                                                       │   ║
║  └──────────────────────────────────────────────────────────────────────┘   ║
║                                                                              ║
║  STEP 2: PRE-PROCESSING GAMBAR                                              ║
║  ═══════════════════════════════                                           ║
║                                                                              ║
║  ┌──────────────────────────────────────────────────────────────────────┐   ║
║  │                                                                       │   ║
║  │  SUB-STEP 2A: Normalisasi Ukuran                                      │   ║
║  │  ┌──────────────────────────────────────────────────────────────┐  │   ║
║  │  │  target_width = 1024                                         │  │   ║
║  │  │  target_height = 768                                        │  │   ║
║  │  │  resized_image = RESIZE(original_image, target_width,       │  │   ║
║  │  │                                   target_height)            │  │   ║
║  │  └──────────────────────────────────────────────────────────────┘  │   ║
║  │                                                                       │   ║
║  │  SUB-STEP 2B: Konversi Grayscale                                     │   ║
║  │  ┌──────────────────────────────────────────────────────────────┐  │   ║
║  │  │  grayscale = RGB_TO_GRAYSCALE(resized_image)                │  │   ║
║  │  │  // Menggunakan formula: Y = 0.299R + 0.587G + 0.114B       │  │   ║
║  │  └──────────────────────────────────────────────────────────────┘  │   ║
║  │                                                                       │   ║
║  │  SUB-STEP 2C: Contrast Enhancement                                  │   ║
║  │  ┌──────────────────────────────────────────────────────────────┐  │   ║
║  │  │  enhanced = HISTOGRAM_EQUALIZATION(grayscale)              │  │   ║
║  │  │  // Meningkatkan kontras untuk feature extraction           │  │   ║
║  │  └──────────────────────────────────────────────────────────────┘  │   ║
║  │                                                                       │   ║
║  └──────────────────────────────────────────────────────────────────────┘   ║
║                                                                              ║
║  STEP 3: EDGE DETECTION (ALGORITMA CANNY)                                  ║
║  ═════════════════════════════════════════════                             ║
║                                                                              ║
║  ┌──────────────────────────────────────────────────────────────────────┐   ║
║  │                                                                       │   ║
║  │  SUB-STEP 3A: Gaussian Blur (Reduce Noise)                            │   ║
║  │  ┌──────────────────────────────────────────────────────────────┐  │   ║
║  │  │  blurred = GAUSSIAN_BLUR(enhanced, kernel_size=5)          │  │   ║
║  │  └──────────────────────────────────────────────────────────────┘  │   ║
║  │                                                                       │   ║
║  │  SUB-STEP 3B: Gradient Calculation                                   │   ║
║  │  ┌──────────────────────────────────────────────────────────────┐  │   ║
║  │  │  gx = SOBEL_FILTER_X(blurred)  // Gradient horizontal     │  │   ║
║  │  │  gy = SOBEL_FILTER_Y(blurred)  // Gradient vertical       │  │   ║
║  │  │  magnitude = SQRT(gx² + gy²)                              │  │   ║
║  │  │  direction = ATAN2(gy, gx)                                 │  │   ║
║  │  └──────────────────────────────────────────────────────────────┘  │   ║
║  │                                                                       │   ║
║  │  SUB-STEP 3C: Non-Maximum Suppression                                │   ║
║  │  ┌──────────────────────────────────────────────────────────────┐  │   ║
║  │  │  suppressed = NON_MAX_SUPPRESSION(magnitude, direction)    │  │   ║
║  │  │  // Menghapus piksel yang bukan edge lokal                  │  │   ║
║  │  └──────────────────────────────────────────────────────────────┘  │   ║
║  │                                                                       │   ║
║  │  SUB-STEP 3D: Double Threshold & Edge Tracking                     │   ║
║  │  ┌──────────────────────────────────────────────────────────────┐  │   ║
║  │  │  high_threshold = 100                                        │  │   ║
║  │  │  low_threshold = 50                                          │  │   ║
║  │  │                                                               │  │   ║
║  │  │  edges = DOUBLE_THRESHOLD(enhanced, high, low)              │  │   ║
║  │  │  // edge_pixels = STRONG_EDGES or (WEAK_EDGES connected    │  │   ║
║  │  │  //                       to STRONG_EDGES)                  │  │   ║
║  │  └──────────────────────────────────────────────────────────────┘  │   ║
║  │                                                                       │   ║
║  └──────────────────────────────────────────────────────────────────────┘   ║
║                                                                              ║
║  STEP 4: FEATURE EXTRACTION                                                 ║
║  ══════════════════════════════                                            ║
║                                                                              ║
║  ┌──────────────────────────────────────────────────────────────────────┐   ║
║  │                                                                       │   ║
║  │  FEATURE 1: Deteksi Pola Tri Brata                                   │   ║
║  │  ┌──────────────────────────────────────────────────────────────┐  │   ║
║  │  │                                                               │  │   ║
║  │  │  // Tri Brata = 3 segitiga berbentuk logo                      │  │   ║
║  │  │  ┌──────────────────────────────────────────────────────┐  │  │   ║
║  │  │  │  SHAPE DETECTION:                                      │  │  │   ║
║  │  │  │  1. Find contours dari edges                           │  │  │   ║
║  │  │  │  2. Classify shape (triangle detection)               │  │  │   ║
║  │  │  │  3. Count triangles yang terdeteksi                    │  │  │   ║
║  │  │  │  4. Cek geometri (sudut, rasio)                       │  │  │   ║
║  │  │  │  5. Cek rainbow color effect                           │  │  │   ║
║  │  │  │                                                         │  │  │   ║
║  │  │  │  RESULT:                                                 │  │  │   ║
║  │  │  │  • deteksi_tri_brata = TRUE/FALSE                      │  │  │   ║
║  │  │  │  • triangle_count = 0-3                                 │  │  │   ║
║  │  │  │  • rainbow_detected = TRUE/FALSE                       │  │  │   ║
║  │  │  └──────────────────────────────────────────────────────┘  │  │   ║
║  │  │                                                               │  │   ║
║  │  └──────────────────────────────────────────────────────────────┘  │   ║
║  │                                                                       │   ║
║  │  FEATURE 2: Deteksi Benang UV                                        │   ║
║  │  ┌──────────────────────────────────────────────────────────────┐  │   ║
║  │  │                                                               │  │   ║
║  │  │  // Benang UV hanya terlihat di bawah sinar UV              │  │   ║
║  │  │  ┌──────────────────────────────────────────────────────┐  │  │   ║
║  │  │  │  IMAGE ANALYSIS:                                      │  │  │   ║
║  │  │  │  1. Detect glowing threads (bright green/yellow)     │  │  │   ║
║  │  │  │  2. Analyze pattern (zigzag/straight)               │  │  │   ║
║  │  │  │  3. Verify thread location (embedded in paper)       │  │  │   ║
║  │  │  │                                                         │  │  │   ║
║  │  │  │  RESULT:                                                 │  │  │   ║
║  │  │  │  • benang_uv_detected = TRUE/FALSE                     │  │  │   ║
║  │  │  │  • thread_pattern = "VALID" / "INVALID"               │  │  │   ║
║  │  │  └──────────────────────────────────────────────────────┘  │  │   ║
║  │  │                                                               │  │   ║
║  │  └──────────────────────────────────────────────────────────────┘  │   ║
║  │                                                                       │   ║
║  │  FEATURE 3: Deteksi Watermark "BPKB"                                  │   ║
║  │  ┌──────────────────────────────────────────────────────────────┐  │   ║
║  │  │                                                               │  │   ║
║  │  │  // Watermark tersembunyi di kertas                          │  │   ║
║  │  │  ┌──────────────────────────────────────────────────────┐  │  │   ║
║  │  │  │  OCR + WATERMARK DETECTION:                           │  │  │   ║
║  │  │  │  1. High-contrast scan                                │  │  │   ║
║  │  │  │  2. Detect text-like patterns                         │  │  │   ║
║  │  │  │  3. Match with "BPKB" template                        │  │  │   ║
║  │  │  │                                                         │  │  │   ║
║  │  │  │  RESULT:                                                 │  │  │   ║
║  │  │  │  • watermark_detected = TRUE/FALSE                    │  │  │   ║
║  │  │  │  • text_match_score = 0.0 - 1.0                       │  │  │   ║
║  │  │  └──────────────────────────────────────────────────────┘  │  │   ║
║  │  │                                                               │  │   ║
║  │  └──────────────────────────────────────────────────────────────┘  │   ║
║  │                                                                       │   ║
║  └──────────────────────────────────────────────────────────────────────┘   ║
║                                                                              ║
║  STEP 5: KALKULASI AKURASI & DECISION                                       ║
║  ═══════════════════════════════════════════                               ║
║                                                                              ║
║  ┌──────────────────────────────────────────────────────────────────────┐   ║
║  │                                                                       │   ║
║  │  BERAT FITUR:                                                         │   ║
║  │  ┌──────────────────────────────────────────────────────────────┐  │   ║
║  │  │  weight_tri_brata = 0.50   // Feature paling penting       │  │   ║
║  │  │  weight_benang_uv = 0.30   // Feature pendukung              │  │   ║
║  │  │  weight_watermark = 0.20   // Feature tambahan              │  │   ║
║  │  └──────────────────────────────────────────────────────────────┘  │   ║
║  │                                                                       │   ║
║  │  HITUNG SKOR:                                                         │   ║
║  │  ┌──────────────────────────────────────────────────────────────┐  │   ║
║  │  │                                                               │  │   ║
║  │  │  // Feature scoring                                          │  │   ║
║  │  │  IF deteksi_tri_brata AND rainbow_detected THEN              │  │   │   ║
║  │  │      score_tri_brata = 1.0  // 100%                        │  │   │   ║
║  │  │  ELSIF deteksi_tri_brata THEN                               │  │   │   ║
║  │  │      score_tri_brata = 0.7   // Tanpa rainbow effect        │  │   │   ║
║  │  │  ELSE                                                         │  │   │   ║
║  │  │      score_tri_brata = 0.0   // Tidak terdeteksi           │  │   │   ║
║  │  │  END IF                                                       │  │   │   ║
║  │  │                                                               │  │   │   ║
║  │  │  IF benang_uv_detected THEN                                 │  │   │   ║
║  │  │      score_benang_uv = 1.0                                  │  │   │   ║
║  │  │  ELSE                                                         │  │   │   ║
║  │  │      score_benang_uv = 0.0                                  │  │   │   ║
║  │  │  END IF                                                       │  │   │   ║
║  │  │                                                               │  │   │   ║
║  │  │  score_watermark = text_match_score                         │  │   │   ║
║  │  │                                                               │  │   │   ║
║  │  │  // Weighted sum                                             │  │   │   ║
║  │  │  total_score = (score_tri_brata × 0.50) +                   │  │   │   ║
║  │  │                 (score_benang_uv × 0.30) +                  │  │   │   ║
║  │  │                 (score_watermark × 0.20)                    │  │   │   ║
║  │  │                                                               │  │   │   ║
║  │  │  // Konversi ke persentase                                   │  │   │   ║
║  │  │  accuracy_percent = total_score × 100                       │  │   │   ║
║  │  │                                                               │  │   │   ║
║  │  └──────────────────────────────────────────────────────────────┘  │   ║
║  │                                                                       │   ║
║  │  DECISION:                                                            │   ║
║  │  ┌──────────────────────────────────────────────────────────────┐  │   ║
║  │  │                                                               │  │   ║
║  │  │  IF accuracy_percent >= 95 THEN                              │  │   │   ║
║  │  │      RETURN {                                                 │  │   │   ║
║  │  │          "status": "VERIFIED",                                │  │   │   ║
║  │  │          "accuracy": accuracy_percent,                       │  │   │   ║
║  │  │          "message": "Hologram Tri Brata terdeteksi"          │  │   │   ║
║  │  │      }                                                        │  │   │   ║
║  │  │  ELSIF accuracy_percent >= 70 THEN                           │  │   │   ║
║  │  │      RETURN {                                                 │  │   │   ║
║  │  │          "status": "SUSPECT",                                │  │   │   ║
║  │  │          "accuracy": accuracy_percent,                       │  │   │   ║
║  │  │          "message": "Verifikasi manual diperlukan"           │  │   │   ║
║  │  │      }                                                        │  │   │   ║
║  │  │  ELSE                                                         │  │   │   ║
║  │  │      RETURN {                                                 │  │   │   ║
║  │  │          "status": "REJECTED",                               │  │   │   ║
║  │  │          "accuracy": accuracy_percent,                       │  │   │   ║
║  │  │          "message": "BPKB tidak valid - Hologram tidak      │  │   │   ║
║  │  │                      terdeteksi"                              │  │   │   ║
║  │  │      }                                                        │  │   │   ║
║  │  │  END IF                                                       │  │   │   ║
║  │  │                                                               │  │   │   ║
║  │  └──────────────────────────────────────────────────────────────┘  │   ║
║  │                                                                       │   ║
║  └──────────────────────────────────────────────────────────────────────┘   ║
║                                                                              ║
╠══════════════════════════════════════════════════════════════════════════════╣
║                                                                              ║
║  CONTOH OUTPUT:                                                             ║
║  ═════════════                                                               ║
║                                                                              ║
║  ┌──────────────────────────────────────────────────────────────────────┐   ║
║  │                                                                       │   ║
║  │  {                                                                    │   ║
║  │    "status": "VERIFIED",                                             │   ║
║  │    "accuracy": 97.5,                                                 │   ║
║  │    "features": {                                                     │   ║
║  │      "tri_brata": {                                                  │   ║
║  │        "detected": true,                                             │   ║
║  │        "triangle_count": 3,                                          │   ║
║  │        "rainbow_effect": true                                        │   ║
║  │      },                                                               │   ║
║  │      "benang_uv": {                                                  │   ║
║  │        "detected": true,                                             │   ║
║  │        "pattern": "VALID"                                           │   ║
║  │      },                                                               │   ║
║  │      "watermark": {                                                  │   ║
║  │        "detected": true,                                             │   ║
║  │        "match_score": 0.95                                          │   ║
║  │      }                                                               │   ║
║  │    },                                                                │   ║
║  │    "timestamp": "2024-01-15T10:30:00Z"                              │   ║
║  │  }                                                                    │   ║
║  │                                                                       │   ║
║  └──────────────────────────────────────────────────────────────────────┘   ║
║                                                                              ║
╚══════════════════════════════════════════════════════════════════════════════╝
```

---

## 3.3.5 FLOWCHART VALIDASI BATAS WAKTU FIDUSIA (30 HARI)

```
╔══════════════════════════════════════════════════════════════════════════════╗
║              FLOWCHART VALIDASI BATAS WAKTU FIDUSIA (DETAIL)                 ║
╠══════════════════════════════════════════════════════════════════════════════╣
║                                                                              ║
║  TUJUAN: Memastikan pendaftaran fidusia dilakukan dalam 30 hari kalender  ║
║           sejak akta notaris ditandatangani.                                  ║
║                                                                              ║
╠══════════════════════════════════════════════════════════════════════════════╣
║                                                                              ║
║  PENDAHULUAN:                                                                ║
║  ══════════════                                                            ║
║                                                                              ║
║  Fidusia adalah pengikatan kendaraan sebagai jaminan kredit. Pendaftaran    ║
║  fidusia ke Ditjen AHU (Kementerian Hukum dan HAM) harus dilakukan         ║
║  secara elektronik melalui SABH (Sistem Administrasi Badan Hukum).         ║
║                                                                              ║
║  ⚠️ BATAS WAKTU:                                                            ║
║  Maksimal 30 hari kalender sejak tanggal akta notaris ditandatangani.       ║
║                                                                              ║
║  ⚠️ KONSEKUENSI TERLAMBAT:                                                 ║
║  • Sistem AHU akan MENOLAK secara otomatis                                  ║
║  • Leasing kehilangan hak preferen atas kendaraan                          ║
║  • Potensi kerugian besar jika debitur wanprestasi                          ║
║                                                                              ║
╠══════════════════════════════════════════════════════════════════════════════╣
║                                                                              ║
║  INPUT VARIABEL:                                                            ║
║  ═══════════════                                                            ║
║                                                                              ║
║  ┌──────────────────────────────────────────────────────────────────────┐   ║
║  │                                                                       │   ║
║  │  ┌───────────────────┬────────────┬────────────────────────────────┐  │   ║
║  │  │ Variabel         │ Tipe       │ Deskripsi                      │  │   ║
║  │  ├───────────────────┼────────────┼────────────────────────────────┤  │   ║
║  │  │ tanggal_akta      │ DATE       │ Tanggal akta notaris           │  │   ║
║  │  │                   │            │ ditandatangani                  │  │   ║
║  │  │ nomor_akta       │ VARCHAR(50)│ Nomor akta fidusia              │  │   ║
║  │  │ notaris_id       │ UUID       │ ID notaris yang membuat         │  │   ║
║  │  │                   │            │ akta                             │  │   ║
║  │  │ tanggal_submit   │ DATE       │ Tanggal rencana submit          │  │   ║
║  │  │                   │            │ ke Ditjen AHU                   │  │   ║
║  │  │ batas_pendaftaran│ DATE       │ Tanggal batas maksimal          │  │   ║
║  │  │                   │            │ (tanggal_akta + 30 hari)        │  │   ║
║  │  └───────────────────┴────────────┴────────────────────────────────┘  │   ║
║  │                                                                       │   ║
║  └──────────────────────────────────────────────────────────────────────┘   ║
║                                                                              ║
╠══════════════════════════════════════════════════════════════════════════════╣
║                                                                              ║
║  FLOWCHART LENGKAP:                                                         ║
║  ═════════════════                                                           ║
║                                                                              ║
    ┌─────────────────────────────────────────────────────────────────────┐
    │  START: Validasi Batas Waktu Fidusia                                │
    └───────────────────────────────┬─────────────────────────────────────┘
                                    │
                                    ▼
    ┌─────────────────────────────────────────────────────────────────────┐
    │  INPUT: Data Aplikasi Fidusia                                       │
    │  ┌───────────────────────────────────────────────────────────────┐  │
    │  │  • tanggal_akta = "2024-01-15"                               │  │
    │  │  • nomor_akta = "123/akta/fid/2024"                          │  │
    │  │  • notaris_id = "uuid-notaris"                               │  │
    │  │  • tanggal_submit = "2024-02-10"                             │  │
    │  └───────────────────────────────────────────────────────────────┘  │
    └───────────────────────────────┬─────────────────────────────────────┘
                                    │
                                    ▼
    ┌─────────────────────────────────────────────────────────────────────┐
    │  STEP 1: Hitung Batas Pendaftaran                                    │
    │  ┌───────────────────────────────────────────────────────────────┐  │
    │  │                                                               │  │
    │  │  VARIABEL:                                                    │  │
    │  │  ┌─────────────────────────────────────────────────────┐   │  │
    │  │  │  INPUT:                                                  │   │  │
    │  │  │  • tanggal_akta = DATE                                 │   │  │
    │  │  │                                                          │   │  │
    │  │  │  KONSTANTA:                                             │   │  │
    │  │  │  • BATAS_HARI = 30 (hari kalender)                     │   │  │
    │  │  │                                                          │   │  │
    │  │  │  FORMULA:                                               │   │  │
    │  │  │  ┌─────────────────────────────────────────────────┐   │  │  │
    │  │  │  │  batas_pendaftaran =                             │   │  │  │
    │  │  │  │      ADD_DAYS(tanggal_akta, BATAS_HARI)         │   │  │  │
    │  │  │  │  // Tambah 30 hari dari tanggal akta            │   │  │  │
    │  │  │  │  // Termasuk weekend & holiday                   │   │  │  │
    │  │  │  │  // 30 hari kalender, BUKAN 30 hari kerja        │   │  │  │
    │  │  │  └─────────────────────────────────────────────────┘   │   │  │
    │  │  │                                                          │   │  │
    │  │  │  CONTOH:                                                │   │  │
    │  │  │  tanggal_akta = 2024-01-15                             │   │  │
    │  │  │  batas_pendaftaran = 2024-01-15 + 30 hari              │   │  │
    │  │  │                        = 2024-02-14                    │   │  │
    │  │  └─────────────────────────────────────────────────────┘   │  │
    │  │                                                               │  │
    │  │  // Simpan ke database untuk tracking                      │  │
    │  │  ┌─────────────────────────────────────────────────────┐   │  │
    │  │  │  SAVE_TO_DATABASE(                                    │   │  │
    │  │  │      aplikasi_kredit.batas_pendaftaran =             │   │  │
    │  │  │          ADD_DAYS(tanggal_akta, 30)                  │   │  │
    │  │  │  )                                                    │   │  │
    │  │  └─────────────────────────────────────────────────────┘   │  │
    │  │                                                               │  │
    │  └───────────────────────────────────────────────────────────────┘  │
    └───────────────────────────────┬─────────────────────────────────────┘
                                    │
                                    ▼
    ┌─────────────────────────────────────────────────────────────────────┐
    │  STEP 2: Hitung Selisih Hari                                         │
    │  ┌───────────────────────────────────────────────────────────────┐  │
    │  │                                                               │  │
    │  │  VARIABEL:                                                    │  │
    │  │  ┌─────────────────────────────────────────────────────┐   │  │
    │  │  │  INPUT:                                                  │   │  │
    │  │  │  • tanggal_submit = "2024-02-10"                      │   │  │
    │  │  │  • batas_pendaftaran = "2024-02-14"                   │   │  │
    │  │  │                                                          │   │  │
    │  │  │  KALKULASI:                                             │   │  │
    │  │  │  ┌─────────────────────────────────────────────────┐   │  │  │
    │  │  │  │  // Hitung selisih hari                         │   │  │  │
    │  │  │  │  selisih_hari = DATEDIFF(tanggal_submit,        │   │  │  │
    │  │  │  │                          tanggal_akta)          │   │  │  │
    │  │  │  │  // Menghitung hari dari tanggal_akta            │   │  │  │
    │  │  │  │  // hingga tanggal_submit                        │   │  │  │
    │  │  │  │                                                      │   │  │  │
    │  │  │  │  CONTOH:                                           │   │  │  │
    │  │  │  │  tanggal_akta = 2024-01-15                         │   │  │  │
    │  │  │  │  tanggal_submit = 2024-02-10                       │   │  │  │
    │  │  │  │  selisih_hari = 26 hari                           │   │  │  │
    │  │  │  │                                                      │   │  │  │
    │  │  │  │  // Tanggal 15 Jan ke 10 Feb = 26 hari            │   │  │  │
    │  │  │  │  // (16 hari Jan + 10 hari Feb)                   │   │  │  │
    │  │  │  └─────────────────────────────────────────────────┘   │   │  │
    │  │  └─────────────────────────────────────────────────────┘   │  │
    │  │                                                               │  │
    │  └───────────────────────────────────────────────────────────────┘  │
    └───────────────────────────────┬─────────────────────────────────────┘
                                    │
                                    ▼
    ┌─────────────────────────────────────────────────────────────────────┐
    │  DECISION: Apakah Dalam Batas?                                        │
    │  ┌───────────────────────────────────────────────────────────────┐  │
    │  │                                                               │  │
    │  │  IF selisih_hari <= 30 THEN                                  │  │
    │  │     → PROCEED TO: STEP_3 (Generate Kode Billing)             │  │
    │  │  ELSE                                                         │  │
    │  │     → PROCEED TO: REJECT_SUBMISSION                           │  │
    │  │  END IF                                                        │  │
    │  │                                                               │  │
    │  └───────────────────────────────────────────────────────────────┘  │
    └───────────────────────────────┬─────────────────────────────────────┘
                                    │
              ┌─────────────────────┴─────────────────────┐
              │                                           │
              ▼                                           ▼
              
═══════════════════════════════════════════════════════════════════════════════
BRANCH A: DALAM BATAS (≤ 30 HARI)
═══════════════════════════════════════════════════════════════════════════════

    ┌─────────────────────────────────────────────────────────────────────┐
    │  STEP 3: Generate Kode Billing PNBP                                 │
    │  ┌───────────────────────────────────────────────────────────────┐  │
    │  │                                                               │  │
    │  │  // Request ke SABH untuk generate kode billing               │  │
    │  │  ┌─────────────────────────────────────────────────────┐   │  │
    │  │  │  api_request = {                                     │   │  │
    │  │  │      "endpoint": "/v1/ahu/generate-billing",        │   │  │
    │  │  │      "akta_number": nomor_akta,                     │   │  │
    │  │  │      "notaris_id": notaris_id,                       │   │  │
    │  │  │      "jenis_perubahan": "FIDUSIA",                   │   │  │
    │  │  │      "nilai_penjaminan": jumlah_pinjaman             │   │  │
    │  │  │  }                                                    │   │  │
    │  │  │                                                        │   │  │
    │  │  │  api_response = HTTP_POST(                            │   │  │
    │  │  │      url = "https://sabh.kemenkumham.go.id/api",     │   │  │
    │  │  │      body = api_request,                             │   │  │
    │  │  │      headers = {"Authorization": "Bearer " + TOKEN} │   │  │
    │  │  │  )                                                    │   │  │
    │  │  │                                                        │   │  │
    │  │  │  kode_billing = api_response.kode_billing            │   │  │
    │  │  │  jumlah_pnbp = api_response.jumlah_pnbp              │   │  │
    │  │  │  // Estimasi PNBP: Rp50.000 - Rp100.000              │   │  │
    │  │  └─────────────────────────────────────────────────────┘   │  │
    │  │                                                               │  │
    │  │  // Simpan ke database                                       │  │
    │  │  ┌─────────────────────────────────────────────────────┐   │  │
    │  │  │  UPDATE aplikasi_kredit SET (                       │   │  │
    │  │  │      kode_billing = api_response.kode_billing,      │   │  │
    │  │  │      jumlah_pnbp = api_response.jumlah_pnbp,        │   │  │
    │  │  │      status_bayar = "PENDING"                       │   │  │
    │  │  │  ) WHERE id = aplikasi_kredit_id;                   │   │  │
    │  │  └─────────────────────────────────────────────────────┘   │  │
    │  │                                                               │  │
    │  └───────────────────────────────────────────────────────────────┘  │
    └───────────────────────────────┬─────────────────────────────────────┘
                                    │
                                    ▼
    ┌─────────────────────────────────────────────────────────────────────┐
    │  STEP 4: Pembayaran PNBP                                              │
    │  ┌───────────────────────────────────────────────────────────────┐  │
    │  │                                                               │  │
    │  │  // User bayar PNBP via bank                                 │  │
    │  │  ┌─────────────────────────────────────────────────────┐   │  │
    │  │  │  PAYMENT_METHODS:                                     │   │  │
    │  │  │  • Bank Transfer (BCA, Mandiri, BRI, dll)          │   │  │
    │  │  │  • KlikBCA / KlikBCA Bisnis                           │   │  │
    │  │  │  • ATM (semua bank)                                   │   │  │
    │  │  │  • Mobile Banking                                     │   │  │
    │  │  │                                                       │   │  │
    │  │  │  INSTRUKSI:                                           │   │  │
    │  │  │  ┌───────────────────────────────────────────────┐ │   │  │
    │  │  │  │  1. Pilih "Pembayaran PNBP"                    │ │   │  │
    │  │  │  │  2. Masukkan Kode Billing: [kode_billing]     │ │   │  │
    │  │  │  │  3. Jumlah: Rp[jumlah_pnbp]                   │ │   │  │
    │  │  │  │  4. Selesai                                       │ │   │  │
    │  │  │  └───────────────────────────────────────────────┘ │   │  │
    │  │  │                                                       │   │  │
    │  │  │  // Update status setelah konfirmasi                  │   │  │
    │  │  │  IF payment_confirmed THEN                           │   │  │
    │  │  │      status_bayar = "PAID"                            │   │  │
    │  │  │      paid_at = CURRENT_TIMESTAMP                       │   │  │
    │  │  │  END IF                                                │   │  │
    │  │  └─────────────────────────────────────────────────────┘   │  │
    │  │                                                               │  │
    │  └───────────────────────────────────────────────────────────────┘  │
    └───────────────────────────────┬─────────────────────────────────────┘
                                    │
                                    ▼
    ┌─────────────────────────────────────────────────────────────────────┐
    │  STEP 5: Submit ke Ditjen AHU                                        │
    │  ┌───────────────────────────────────────────────────────────────┐  │
    │  │                                                               │  │
    │  │  // Kirim data ke SABH untuk diproses                        │  │
    │  │  ┌─────────────────────────────────────────────────────┐   │  │
    │  │  │  api_request = {                                     │   │  │
    │  │  │      "endpoint": "/v1/ahu/submit-fidusia",           │   │  │
    │  │  │      "akta_number": nomor_akta,                       │   │  │
    │  │  │      "tanggal_akta": tanggal_akta,                   │   │  │
    │  │  │      "kode_billing": kode_billing,                   │   │  │
    │  │  │      "bukti_bayar": base64_payment_receipt,          │   │  │
    │  │  │      "data_kendaraan": {                              │   │  │
    │  │  │          "nomor_rangka": nomor_rangka,               │   │  │
    │  │  │          "nomor_mesin": nomor_mesin,                 │   │  │
    │  │  │          "nama_pemilik": nama_pembeli                │   │  │
    │  │  │      },                                                │   │  │
    │  │  │      "nilai_penjaminan": jumlah_pinjaman             │   │  │
    │  │  │  }                                                      │   │  │
    │  │  │                                                           │   │  │
    │  │  │  api_response = HTTP_POST(                             │   │  │
    │  │  │      url = "https://sabh.kemenkumham.go.id/api",       │   │  │
    │  │  │      body = api_request,                              │   │  │
    │  │  │      timeout = 60 seconds                             │   │  │
    │  │  │  )                                                      │   │  │
    │  │  │                                                           │   │  │
    │  │  │  IF api_response.status == "SUCCESS" THEN              │   │  │
    │  │  │      nomor_sertifikat = api_response.sertifikat_number │   │  │
    │  │  │      → PROCEED TO: SUCCESS                             │   │  │
    │  │  │  ELSE                                                    │   │  │
    │  │  │      error_message = api_response.error                 │   │  │
    │  │  │      → PROCEED TO: ERROR_HANDLING                       │   │  │
    │  │  │  END IF                                                  │   │  │
    │  │  └─────────────────────────────────────────────────────┘   │  │
    │  │                                                               │  │
    │  └───────────────────────────────────────────────────────────────┘  │
    └───────────────────────────────┬─────────────────────────────────────┘
                                    │
                                    ▼
                            ┌───────────────┐
                            │ ✅ BERHASIL  │
                            │ PENDAFTARAN  │
                            └───────────────┘

═══════════════════════════════════════════════════════════════════════════════
BRANCH B: MELEBIHI BATAS (> 30 HARI)
═══════════════════════════════════════════════════════════════════════════════

    ┌─────────────────────────────────────────────────────────────────────┐
    │  REJECT_SUBMISSION: Tolak Pendaftaran                                │
    │  ┌───────────────────────────────────────────────────────────────┐  │
    │  │                                                               │  │
    │  │  // Sistem SABH akan menolak secara otomatis                  │  │
    │  │  // Tapi kita juga block dari sisi aplikasi                  │  │
    │  │  ┌─────────────────────────────────────────────────────┐   │  │
    │  │  │  LOG_ERROR({                                        │   │  │
    │  │  │      "code": "FIDUCIA_EXPIRED",                     │   │  │
    │  │  │      "message": "Pendaftaran fidusia tidak dapat     │   │  │
    │  │  │                 "dilakukan karena sudah melewati      │   │  │
    │  │  │                 "batas waktu 30 hari kalender",       │   │  │
    │  │  │      "tanggal_akta": tanggal_akta,                   │   │  │
    │  │  │      "batas_pendaftaran": batas_pendaftaran,         │   │  │
    │  │  │      "tanggal_submit": tanggal_submit,               │   │  │
    │  │  │      "selisih_hari": selisih_hari,                  │   │  │
    │  │  │      "lewat_batas": selisih_hari - 30               │   │  │
    │  │  │  })                                                    │   │  │
    │  │  │                                                        │   │  │
    │  │  │  RETURN ERROR({                                        │   │  │
    │  │  │      "status": "REJECTED",                            │   │  │
    │  │  │      "code": "FIDUCIA_30_DAYS_EXCEEDED",              │   │  │
    │  │  │      "message": "Batas pendaftaran fidusia adalah     │   │  │
    │  │  │               "30 hari kalender. Pendaftaran tidak   │   │  │
    │  │  │               "dapat dilanjutkan.",                   │   │  │
    │  │  │      "batas_tanggal": DATE_FORMAT(batas_pendaftaran,│   │  │
    │  │  │                          "dd MMMM yyyy")             │   │  │
    │  │  │  })                                                    │   │  │
    │  │  └─────────────────────────────────────────────────────┘   │  │
    │  │                                                               │  │
    │  │  // Update status aplikasi                                     │  │
    │  │  ┌─────────────────────────────────────────────────────┐   │  │
    │  │  │  UPDATE aplikasi_kredit SET (                       │   │  │
    │  │  │      status = "FIDUCIA_EXPIRED",                    │   │  │
    │  │  │      rejection_reason = "Melebihi batas 30 hari"   │   │  │
    │  │  │      rejected_at = CURRENT_TIMESTAMP                  │   │  │
    │  │  │  ) WHERE id = aplikasi_kredit_id;                   │   │  │
    │  │  └─────────────────────────────────────────────────────┘   │  │
    │  │                                                               │  │
    │  │  // Kirim notifikasi ke admin                                 │  │
    │  │  ┌─────────────────────────────────────────────────────┐   │  │
    │  │  │  SEND_NOTIFICATION(                                 │   │  │
    │  │  │      to = "admin@dealer.com",                       │   │  │
    │  │  │      subject = "URGENT: Fidusia Melebihi Batas",    │   │  │
    │  │  │      body = "Aplikasi kredit ID [xxx] fidusianya    │   │  │
    │  │  │            sudah melebihi batas 30 hari dan tidak     │   │  │
    │  │  │            dapat didaftarkan."                      │   │  │
    │  │  │  )                                                    │   │  │
    │  │  └─────────────────────────────────────────────────────┘   │  │
    │  │                                                               │  │
    │  └───────────────────────────────────────────────────────────────┘  │
    └───────────────────────────────┬─────────────────────────────────────┘
                                    │
                                    ▼
                            ┌───────────────┐
                            │ ❌ DITOLAK   │
                            │ 30 HARI      │
                            └───────────────┘

═══════════════════════════════════════════════════════════════════════════════
SISTEM REMINDER OTOMATIS
═══════════════════════════════════════════════════════════════════════════════

    ┌─────────────────────────────────────────────────────────────────────┐
    │  SCHEDULED_REMINDER: Sistem Pengingat Otomatis                       │
    │  ┌───────────────────────────────────────────────────────────────┐  │
    │  │                                                               │  │
    │  │  // Cron job berjalan setiap hari                             │  │
    │  │  ┌─────────────────────────────────────────────────────┐   │  │
    │  │  │  daily_job = SCHEDULE("0 9 * * *") // Setiap jam 9 │   │  │
    │  │  │                                                      │   │  │
    │  │  │  FOR EACH aplikasi_kredit IN aplikasi_kredit_list:   │   │  │
    │  │  │      hari_sekarang = TODAY()                         │   │  │
    │  │  │      hari_ke = DATEDIFF(batas_pendaftaran,          │   │  │
    │  │  │                           hari_sekarang)             │   │  │
    │  │  │                                                       │   │  │
    │  │  │      IF hari_ke == 30 THEN                           │   │  │
    │  │  │          SEND_EMAIL("dealer@company.com",            │   │  │
    │  │  │                    "Fidusia: 30 hari lagi",          │   │  │
    │  │  │                    "Harap segera daftarkan...")      │   │  │
    │  │  │      END IF                                           │   │  │
    │  │  │                                                       │   │  │
    │  │  │      IF hari_ke == 15 THEN                            │   │  │
    │  │  │          SEND_SMS("admin_phone",                       │   │  │
    │  │  │                    "REMINDER: 15 hari tersisa untuk    │   │  │
    │  │  │                    "pendaftaran fidusia")             │   │  │
    │  │  │      END IF                                           │   │  │
    │  │  │                                                       │   │  │
    │  │  │      IF hari_ke == 7 THEN                             │   │  │
    │  │  │          SEND_ALERT("manager",                        │   │  │
    │  │  │                    "URGENT: 7 hari tersisa",          │   │  │
    │  │  │                    "Segera selesaikan fidusia!")       │   │  │
    │  │  │      END IF                                           │   │  │
    │  │  │                                                       │   │  │
    │  │  │      IF hari_ke == 1 THEN                             │   │  │
    │  │  │          SEND_URGENT_NOTIFICATION("all_admins",       │   │  │
    │  │  │                    "FINAL WARNING: Besok adalah        │   │  │
    │  │  │                    "batas terakhir!")                 │   │  │
    │  │  │      END IF                                           │   │  │
    │  │  │                                                       │   │  │
    │  │  │      IF hari_ke < 0 THEN                               │   │  │
    │  │  │          MARK_AS_EXPIRED(aplikasi_kredit)             │   │  │
    │  │  │      END IF                                           │   │  │
    │  │  │                                                       │   │  │
    │  │  │  END FOR                                              │   │  │
    │  │  └─────────────────────────────────────────────────────┘   │  │
    │  │                                                               │  │
    │  └───────────────────────────────────────────────────────────────┘  │
    └─────────────────────────────────────────────────────────────────────┘
```

---

*Dokumen ini adalah detail lengkap untuk Urutan 3 dari Outline Presentasi SRS*
*Versi: 1.0 | Last Updated: June 2026*
*Referensi: SRS, BusinessLogic-MermaidJS.md, memory-bank/conventions.md*
