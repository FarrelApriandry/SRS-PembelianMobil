# Business Logic Mermaid.js Flowcharts
## SRS Reference: Sistem Transaksi Pembelian Kendaraan Bermotor Terintegrasi (Indonesia)

Dokumen ini berisi flowchart Mermaid.js yang menjelaskan logika bisnis, formula kalkulasi, dan aturan validasi untuk tim programmer dan Quality Assurance.

---

## 1. Kalkulasi Pajak - Pricing Engine (Sub-bab SRS: 3.4)

### 1.1 Flowchart Kalkulasi Pajak Lengkap

```mermaid
flowchart TD
    A[🚀 Mulai Kalkulasi Pajak] --> B[/Input: Tipe Kendaraan<br/>NJKB, Wilayah Pendaftaran<br/>Golongan Kendaraan/]
    
    B --> C{Apakah Kendaraan<br/>Bermotor Baru?}
    
    C -->|Ya| D[Verifikasi TKDN Unit]
    C -->|Tidak| E[Ambil NJKB dari<br/>Lembar STNK]
    
    D --> F{TKDN ≥ 40%?}
    F -->|Ya| G[✓ KBLBB Eligible<br/>PPN DTP 10%]
    F -->|Tidak| H[✗ Tidak Eligible<br/>PPN Normal]
    
    E --> I[Konfirmasi:<br/>NJKB Tercatat = PKB Pokok ÷ Tarif]
    
    G --> J[Hitung DPP PPN]
    H --> J
    I --> J
    
    J --> K["DPP PPN = 11/12 × NJKB"]
    K --> L["PPN = 12% × DPP PPN<br/>Atau: PPN = 11% × NJKB"]
    
    L --> M{Apakah Wilayah<br/>DKI Jakarta?}
    
    M -->|Ya| N["Opsen PKB = Rp0<br/>Opsen BBNKB = Rp0<br/>BBNKB Pokok = 12.5% × NJKB"]
    
    M -->|Tidak| O["BBNKB Pokok = 12% × NJKB<br/>PKB Pokok = Tarif × NJKB"]
    
    N --> P[Hitung Opsen]
    O --> P
    
    P --> Q["Opsen PKB = 66% × PKB Pokok<br/>Opsen BBNKB = 66% × BBNKB Pokok"]
    
    Q --> R{Cek Kepemilikan<br/>Progresif?}
    
    R -->|Ya| S["Hitung Urutan Kendaraan<br/>berdasarkan NIK/ALAMAT KK"]
    R -->|Tidak| T[Lewati Progresif]
    
    S --> U{Apakah kendaraan<br/>ke-?}
    U -->|1| V[Tarif Progresif = 2%]
    U -->|2| W[Tarif Progresif = 3%]
    U -->|3| X[Tarif Progresif = 4%]
    U -->|4| Y[Tarif Progresif = 5%]
    U -->|5+| Z[Tarif Progresif = 6% Maks]
    
    V --> AA[Rekalkulasi PKB dengan<br/>Tarif Progresif]
    W --> AA
    X --> AA
    Y --> AA
    Z --> AA
    
    T --> AB[Hitung PNBP]
    AA --> AB
    
    AB --> AC["SWDKLLJ = Rp143.000 - 150.000<br/>STNK Baru = Rp200.000<br/>TNKB = Rp100.000<br/>BPKB = Rp375.000"]
    
    AC --> AD[/Output: Rincian Tagihan Pajak<br/>PPN, PKB, Opsen PKB, BBNKB<br/>Opsen BBNKB, PNBP Total/]
    
    AD --> AE[✅ Selesai]
    
    style G fill:#90EE90
    style H fill:#FFB6C1
    style N fill:#87CEEB
```

### 1.2 Formula Matematika Kalkulasi Pajak

```mermaid
flowchart LR
    subgraph DPP["📊 DPP (Dasar Pengenaan Pajak)"]
        A["NJKB<br/>Nilai Jual Kendaraan"] -->|"÷ 12 × 11"| B["DPP PPN<br/>Nilai Lain"]
        B -->|"× 12%"| C["PPN Terutang"]
    end
    
    subgraph PKB["🚗 PKB & Opsen"]
        D["PKB Pokok<br/>Tarif × NJKB"] -->|"× 66%"| E["Opsen PKB"]
        F["BBNKB Pokok<br/>12-12.5% × NJKB"] -->|"× 66%"| G["Opsen BBNKB"]
    end
    
    subgraph PROGRESIF["📈 Pajak Progresif (DKI)"]
        H["Kendaraan ke-1"] -->|"2%"| I["Tarif"]
        H -->|"3%"| J["ke-2"]
        H -->|"4%"| K["ke-3"]
        H -->|"5%"| L["ke-4"]
        H -->|"6%"| M["ke-5+"]
    end
    
    style A fill:#FFE4B5
    style B fill:#FFE4B5
    style C fill:#FFD700
    style D fill:#E6E6FA
    style E fill:#E6E6FA
    style F fill:#E6E6FA
    style G fill:#E6E6FA
```

---

## 2. Logika Simulasi Kredit - Fintech & Leasing (Sub-bab SRS: 3.5)

### 2.1 Flowchart Penentuan DP Dinamis Berdasarkan NPF

```mermaid
flowchart TD
    A[🚀 Inisiasi Simulasi<br/>Kredit] --> B[/Input: NIK Pembeli<br/>Harga OTR Kendaraan/]
    
    B --> C["Query SLIK OJK<br/>Ambil Riwayat Kredit"]
    
    C --> D{"Status<br/>Kolektibilitas?"}
    
    D -->|"1-5 (Lancar)"| E["✓ Layak Kredit"]
    D -->|"6-9 (Diragukan)"| F["⚠️ Perlu Analisis<br/>Surveyor"]
    D -->|"Kol 1-2)| G["⚠️ Peringkat Baik<br/>Lanjut ke NPF"]
    
    E --> H["Query NPF Leasing<br/>Partner"]
    F --> H
    G --> H
    
    H --> I["Ambil NPF Net<br/>Perusahaan Pembiayaan"]
    
    I --> J{"NPF ≤ 1%?"}
    J -->|Ya| K["📋 DP Minimum = 0%<br/>Kredit Tanpa Uang Muka"]
    
    J -->|Tidak| L{"NPF 1-3%?"}
    L -->|Ya| M["📋 DP Minimum = 10%<br/>Uang Muka Ringan"]
    
    L -->|Tidak| N{"NPF 3-5%?"}
    N -->|Ya| O["📋 DP Minimum = 15%<br/>Uang Muka Sedang"]
    
    N -->|Tidak| P{"NPF > 5%?"}
    P -->|Ya| Q["📋 DP Minimum = 20%<br/>Uang Muka Tinggi"]
    
    P -->|Tidak| R["⚠️ Error NPF<br/>Hubungi Admin"]
    
    K --> S[Hitung Angsuran]
    M --> S
    O --> S
    Q --> S
    
    S --> T["Tenor: 12-72 bulan<br/>Bunga: Sesuai Kategori"]
    
    T --> U[/Output: Simulasi<br/>DP, Angsuran, Total Bayar/]
    
    U --> V[✅ Tampilkan ke<br/>Pembeli]
    
    style K fill:#90EE90
    style M fill:#98FB98
    style O fill:#FFFF99
    style Q fill:#FFD700
    style F fill:#FFA500
    style R fill:#FF6B6B
```

### 2.2 Matrix NPF vs DP Minimum

```mermaid
flowchart LR
    subgraph INPUT["📥 Input Parameter"]
        A["NPF Net<br/>Leasing Partner"] --> B{Logika<br/>Penentuan DP}
    end
    
    subgraph MATRIX["📊 Matrix DP Minimum"]
        B --> C{"NPF ≤ 1%"}
        C -->|"YA"| D["DP = 0%"]
        C -->|"TIDAK"| E{"NPF 1-3%"}
        E -->|"YA"| F["DP = 10%"]
        E -->|"TIDAK"| G{"NPF 3-5%"}
        G -->|"YA"| H["DP = 15%"]
        G -->|"TIDAK"| I{"NPF > 5%"}
        I -->|"YA"| J["DP = 20%"]
    end
    
    subgraph OUTPUT["📤 Output"]
        D --> K["0% DP<br/>Tanpa Uang Muka"]
        F --> L["10% DP<br/>Uang Muka Ringan"]
        H --> M["15% DP<br/>Uang Muka Sedang"]
        J --> N["20% DP<br/>Uang Muka Tinggi"]
    end
    
    style D fill:#90EE90
    style L fill:#98FB98
    style M fill:#FFFF99
    style N fill:#FFD700
```

---

## 3. Validasi Dokumen (Sub-bab SRS: 3.4 - Duplikat)

### 3.1 Matriks Validasi Berdasarkan Jenis Kendaraan

```mermaid
flowchart TD
    A[🚀 Validasi<br/>Dokumen] --> B{/Input: Jenis Kendaraan<br/>Daftar Dokumen/}
    
    B --> C{Jenis<br/>Kendaraan?}
    
    C -->|"BARU"| D["📋 Checklist Mobil Baru"]
    C -->|"BEKAS| E["📋 Checklist Mobil Bekas"]
    C -->|"BEKAS<br/>KREDIT"| F["📋 Checklist Bekas Kredit"]
    
    subgraph BARU["🆕 Mobil Baru - Validasi"]
        D --> D1["✓ Faktur Kendaraan Asli"]
        D1 --> D2["✓ NIK Valid (Checksum)"]
        D2 --> D3["✓ SRUT (Surat Registrasi<br/>Uji Tipe)"]
        D3 --> D4["✓ KTP Asli Pembeli"]
        D4 --> D5{"Tipe CBU<br/>Impor?"}
        D5 -->|"Ya"| D6["✓ Form A<br/>Tingkat Komponen<br/>Dalam Negeri"]
        D5 -->|"Tidak"| D7["✓ Selesai<br/>Tanpa Form A"]
    end
    
    subgraph BEKAS["🔄 Mobil Bekas - Validasi"]
        E --> E1["✓ BPKB Asli<br/>Hologram Tri Brata"]
        E1 --> E2["✓ Verifikasi<br/>Benang UV Sicherheit"]
        E2 --> E3["✓ STNK Aktif"]
        E3 --> E4["✓ Faktur Pembelian"]
        E4 --> E5["✓ Kwitansi Kosong<br/>2-3 Rangkap<br/>Ditandatangani<br/>Sesuai BPKB"]
        E5 --> E6{"Eks Aset<br/>Perusahaan?"}
        E6 -->|"Ya"| E7["✓ SPH<br/>Surat Pelepasan Hak"]
        E6 -->|"Tidak"| E8["✓ Selesai<br/>Tanpa SPH"]
    end
    
    subgraph BEKAS_KREDIT["🏦 Mobil Bekas dalam Kredit"]
        F --> F1["✓ Kontrak Leasing Lama"]
        F1 --> F2["✓ Fotokopi BPKB<br/>Terlegalisir"]
        F2 --> F3["✓ Histori Setoran<br/>Pembayaran Terakhir"]
        F3 --> F4{"Lunas<br/>Sebelumnya?"}
        F4 -->|"Ya"| F5["✓ Surat Keterangan<br/>Lunas dari Leasing"]
        F4 -->|"Tidak"| F6["⚠️ OVERDUE<br/>Blokir Transaksi"]
    end
    
    D6 --> G[✅ Lolos Validasi]
    D7 --> G
    E7 --> G
    E8 --> G
    F5 --> G
    F6 --> H[❌ Ditolak]
    
    G --> I[/Output: Status Validasi<br/>Dokumen Lengkap/]
    H --> J[/Output: Alasan Penolakan/]
    
    style G fill:#90EE90
    style H fill:#FF6B6B
    style F6 fill:#FF6B6B
```

### 3.2 Algoritma Validasi NPWP (Luhn Checksum)

```mermaid
flowchart TD
    A[🚀 Validasi NPWP] --> B[/Input: 15 Digit NPWP/]
    
    B --> C["Ambil 15 Digit<br/>Hilangkan Titik/Garis"]
    
    C --> D["Inisialisasi<br/>Pengali: 5,7,2,3,4,5,6,7,8,9,2,3,4,5,6"]
    
    D --> E["Loop: i = 1 to 15"]
    
    E --> F["Hitung:<br/>Hasil += Digit[i] × Pengali[i]"]
    
    F --> G{"i < 15?"}
    
    G -->|"Ya"| E
    
    G -->|"Tidak"| H["Mod 11 = Hasil mod 11"]
    
    H --> I{"Digit[15] =<br/>(11 - Mod 11) mod 11?"}
    
    I -->|"YA"| J["✅ NPWP VALID<br/>Checksum Match"]
    
    I -->|"TIDAK"| K["❌ NPWP INVALID<br/>Checksum Error"]
    
    style J fill:#90EE90
    style K fill:#FF6B6B
```

### 3.3 Deteksi Hologram BPKB (Akurasi 95%)

```mermaid
flowchart TD
    A[🚀 Deteksi Hologram<br/>BPKB] --> B[/Input: Foto/Scan<br/>Halaman BPKB/]
    
    B --> C["Pre-processing<br/>Normalisasi Ukuran<br/>Konversi Grayscale"]
    
    C --> D["Edge Detection<br/>Algoritma Canny"]
    
    D --> E["Feature Extraction"]
    
    E --> E1["Deteksi Pola<br/>Tri Brata"]
    E --> E2["Deteksi<br/>Benang UV"]
    E --> E3["Deteksi<br/>Watermark"]
    
    E1 --> F1["Cek Simetri<br/>3 Segitiga"]
    E2 --> F2["Verifikasi<br/>Benang Pelangi"]
    E3 --> F3["Cek Tanda Air<br/>'BPKB'"]
    
    F1 --> G{"Semua Fitur<br/>Terdeteksi?"}
    F2 --> G
    F3 --> G
    
    G -->|"Ya × 3"| H["Confidence = 95-100%"]
    G -->|"Ya × 2"| I["Confidence = 70-85%"]
    G -->|"Ya × 1"| J["Confidence = 40-60%"]
    G -->|"Ya × 0"| K["Confidence = <10%"]
    
    H --> L["✅ HOLOGRAM ASLI<br/>Terdeteksi"]
    I --> L
    J --> M["⚠️ RAGU-RAGU<br/>Verifikasi Manual"]
    K --> N["❌ HOLOGRAM PALSU<br/>Blokir Transaksi"]
    
    style L fill:#90EE90
    style M fill:#FFA500
    style N fill:#FF6B6B
```

### 3.4 Validasi Batas Waktu Fidusia (30 Hari)

```mermaid
flowchart TD
    A[🚀 Pendaftaran<br/>Jaminan Fidusia] --> B[/Input: Tanggal Akta Notaris<br/>Tanggal Submit ke AHU/]
    
    B --> C["Hitung Selisih Hari:<br/>Tanggal Submit - Tanggal Akta"]
    
    C --> D["Jumlah Hari = selisih"]
    
    D --> E{"Jumlah Hari ≤ 30?"}
    
    E -->|"YA| F["✅ PENDAFTARAN<br/>DITERIMA"]
    E -->|"TIDAK| G["❌ PENDAFTARAN<br/>DITOLAK OTOMATIS<br/>oleh Sistem AHU"]
    
    F --> H["Generate:<br/>Sertifikat Fidusia<br/>Elektronik"]
    
    H --> I["Simpan ke Database<br/> dengan Notifikasi<br/>Expired Reminder"]
    
    G --> J["⚠️ ALERT:<br/>Terlambat 30 Hari<br/>Blokir Transaksi Kredit"]
    
    style F fill:#90EE90
    style G fill:#FF6B6B
    style J fill:#FF6B6B
    
    subgraph REMINDER["🔔 Reminder System"]
        K["30 Hari Sebelum<br/>Batas"] -->|"Day-30"| L["📧 Notifikasi Email<br/> ke Dealer/Leasing"]
        K -->|"Day-15"| M["📱 SMS Reminder<br/> ke Admin"]
        K -->|"Day-7"| N["🔴 URGENT Alert<br/> ke Manager"]
        K -->|"Day-1"| O["🚨 FINAL WARNING<br/>Segera Daftarkan!"]
    end
```

---

## 4. Logistik & PDI - Pre-Delivery Inspection (Sub-bab SRS: 3.6)

### 4.1 Flowchart Proses PDI Lengkap

```mermaid
flowchart TD
    A[🚀 Mulai PDI<br/>Pre-Delivery Inspection] --> B[/Input: Data Unit<br/>Nomor Rangka/]
    
    B --> C["📋 Download Checklist<br/>PDI Digital<br/>150-188 Titik"]
    
    C --> D["🔍 Inspeksi Exterior:<br/>Bodi, Cat, Kaca,<br/>Lampu, Ban"]
    
    D --> E["🔧 Inspeksi Interior:<br/>Dashboard, Jok, AC,<br/>Elektrikal"]
    
    E --> F["⚙️ Inspeksi Mekanik:<br/>Mesin, Transmisi,<br/>Rem, Suspensi"]
    
    F --> G["💧 Pemeriksaan Cairan:<br/>Oli, Coolant, Rem,<br/>Windscreen"]
    
    G --> H["🔌 Koneksi OBD-II:<br/>Scan Port ECU"]
    
    H --> I{"DTC Error<br/>Terdeteksi?"}
    
    I -->|"Ya| J["⚠️ Hapus DTC Error<br/>via OBD-II Scanner"]
    I -->|"Tidak| K["✅ Tidak Ada<br/>DTC Error"]
    
    J --> K
    
    K --> L["🔧 De-waxing:<br/>Pembilasan Wax<br/>Pelindung Bodi"]
    
    L --> M["⚡ Pasang:<br/>Backup Fuse<br/>Kelistrikan"]
    
    M --> N["🚗 Uji Jalan:<br/>Minimal 10 KM"]
    
    N --> O{"Jarak Tempuh<br/>≥ 10 KM?"}
    
    O -->|"Ya| P["✅ Uji Jalan<br/>LULUS"]
    
    O -->|"Tidak| Q["⚠️ JANGAN KIRIM<br/>Lanjutkan Uji Jalan"]
    Q --> N
    
    P --> R["📝 Isi Laporan PDI<br/>Digital secara Online"]
    
    R --> S["📸 Upload Foto<br/>Unit Post-PDI"]
    
    S --> T["✅ Unit Approved<br/>Siap Dikirim"]
    
    T --> U[/Output: Laporan PDI<br/>Status: APPROVED<br/>Tanggal & Teknisi/]
    
    style P fill:#90EE90
    style T fill:#90EE90
    style Q fill:#FFA500
```

### 4.2 Detail Checklist PDI per Tahapan

```mermaid
flowchart LR
    subgraph EXTERIOR["🚗 Exterior (40 Titik)"]
        A1["Bodi Kiri"] --> A2["Bodi Kanan"]
        A2 --> A3["Depan"]
        A3 --> A4["Belakang"]
        A4 --> A5["Atap"]
        A5 --> A6["Kaca Depan"]
        A6 --> A7["Kaca Belakang"]
        A7 --> A8["Velg & Ban"]
    end
    
    subgraph INTERIOR["🚙 Interior (35 Titik)"]
        B1["Dashboard"] --> B2["Jok Depan"]
        B2 --> B3["Jok Belakang"]
        B3 --> B4["AC & Pendingin"]
        B4 --> B5["Kelistrikan"]
    end
    
    subgraph MEKANIK["⚙️ Mekanik (50 Titik)"]
        C1["Mesin"] --> C2["Transmisi"]
        C2 --> C3["Rem"]
        C3 --> C4["Suspensi"]
        C4 --> C5["Kopling"]
    end
    
    subgraph ELEKTRONIK["🔌 Elektronik (25 Titik)"]
        D1["Head Unit"] --> D2["OBD-II Scan"]
        D2 --> D3["ECU Error"]
        D3 --> D4["Sensor"]
    end
    
    subgraph UJI_JALAN["🛣️ Uji Jalan (Minimal 10 KM)"]
        E1["Akselerasi"] --> E2["Pengereman"]
        E2 --> E3["Handling"]
        E3 --> E4["Suara Mesin"]
    end
    
    style A1 fill:#87CEEB
    style B1 fill:#98FB98
    style C1 fill:#FFD700
    style D1 fill:#DDA0DD
    style E1 fill:#F0E68C
```

### 4.3 Alur STCK (Plat Nomor Sementara)

```mermaid
flowchart TD
    A[🚀 Proses STCK<br/>Pelat Nomor Sementara] --> B[/Input: Data Pembeli<br/>& Kendaraan/]
    
    B --> C["Ajukan STCK<br/>ke Samsat"]
    
    C --> D["PNBP Bayar:<br/>Rp50.000"]
    
    D --> E["Terbit STCK<br/>Berlaku 1 Bulan"]
    
    E --> F{"Tanggal Sekarang"}
    
    F --> G{"Dalam<br/>Periode STCK?"}
    
    G -->|"Ya| H{"Digunakan<br/>Untuk?"}
    
    H -->|"Logistik<br/>Diler-Konsumen"| I["✅ BOLEH<br/>Digunakan"]
    
    H -->|"Uji Fisik<br/>Samsat"| I
    
    H -->|"Keperluan<br/>Pribadi"| J["❌ DILARANG<br/>Bisa Ditilang"]
    
    H -->|"Keluar<br/>Kota"| J
    
    G -->|"Tidak| K["❌ STCK Expired<br/>Harus Perpanjang"]
    
    I --> L{"Tujuan dalam<br/>Kota yang Sama?"}
    
    L -->|"Ya| M["✅ BOLEH<br/>Digunakan"]
    
    L -->|"Tidak| J
    
    style I fill:#90EE90
    style M fill:#90EE90
    style J fill:#FF6B6B
    style K fill:#FF6B6B
```

---

## 5. Ringkasan Aturan Blokir Sistem

```mermaid
flowchart TD
    A[🚫 ATURAN BLOKIR<br/>OTOMATIS] --> B["❌ Dokumen Tidak Lengkap"]
    A --> C["❌ NPWP Invalid<br/>Checksum"]
    A --> D["❌ Hologram BPKB<br/>Tidak Terdeteksi"]
    A --> E["❌ Fidusia Terlambat<br/>> 30 Hari"]
    A --> F["❌ NPF Leasing<br/>Tidak Valid"]
    A --> G["❌ DTC Error ECU<br/>Belum Dihapus"]
    A --> H["❌ Uji Jalan<br/>< 10 KM"]
    A --> I["❌ STCK Expired<br/>Digunakan"]
    A --> J["❌ Overdue Kredit<br/>Bekas"]
    
    B --> K["⚠️ TAMPILKAN PESAN<br/>DOKUMEN YANG KURANG"]
    C --> K
    D --> K
    E --> K
    F --> K
    G --> K
    H --> K
    I --> K
    J --> K
    
    K --> L["🔒 BLOKIR TRANSAKSI<br/>SAMPAI VALID"]
    
    style A fill:#FF6B6B
    style B fill:#FFA500
    style C fill:#FFA500
    style D fill:#FFA500
    style E fill:#FFA500
    style F fill:#FFA500
    style G fill:#FFA500
    style H fill:#FFA500
    style I fill:#FFA500
    style J fill:#FFA500
    style K fill:#FFE4E1
    style L fill:#FF0000,color:#FFFFFF
```

---

## Legenda Warna

| Warna | Arti |
|-------|------|
| 🟢 Hijau | Validasi Lulus / Selesai |
| 🟡 Kuning | Warning / Perlu Perhatian |
| 🔴 Merah | Ditolak / Blokir / Error |

---

## 6. Entity Relationship Diagram (ERD) Fisik

```mermaid
erDiagram
    Pembeli ||--o{ Kendaraan : memesan
    Pembeli ||--o{ DokumenLegalitas : mengunggah
    Pembeli ||--o{ AplikasiKredit : mengajukan
    AplikasiKredit ||--o| Notaris : menggunakan_jasa
    AplikasiKredit ||--o| SertifikatFidusia : menghasilkan
    AplikasiKredit ||--o{ PDI_Report : memerlukan
    Kendaraan ||--o{ DokumenLegalitas : memiliki
    Kendaraan ||--o| PDI_Report : melalui_inspeksi
    
    Pembeli {
        uuid id PK
        string nik UK
        string nama_lengkap
        date tanggal_lahir
        string alamat
        string nomor_hp
        string email
        enum role "PERSONAL|BADAN_HUKUM"
        timestamp created_at
    }
    
    Kendaraan {
        uuid id PK
        string nomor_rangka UK
        string nomor_mesin UK
        string nomor_polisi FK
        enum jenis "BARU|BEKAS"
        enum tipe "SEDAN|SUV|MPV|PICKUP"
        decimal harga_dasar
        decimal njkb
        decimal tarif_pkb
        decimal tarif_ppn
        uuid pembeli_id FK
        enum status "DRAFT|BOOKED|VERIFIED|PAID|DELIVERED"
        timestamp created_at
    }
    
    DokumenLegalitas {
        uuid id PK
        uuid pembeli_id FK
        uuid kendaraan_id FK
        enum jenis "KTP|KK|NPWP|BPKB|STNK|FAKTUR|KWITANSI|SPH|SURAT_PELUNASAN"
        string file_path
        boolean is_verified
        timestamp uploaded_at
        timestamp verified_at
    }
    
    AplikasiKredit {
        uuid id PK
        uuid pembeli_id FK
        uuid kendaraan_id FK
        decimal jumlah_pinjaman
        decimal dp_amount
        integer tenor_bulan
        decimal bunga_tahunan
        enum status "PENDING|APPROVED|REJECTED|DOCUMENTED|FIDUSIA_COMPLETE"
        uuid notaris_id FK
        timestamp created_at
        timestamp approved_at
    }
    
    Notaris {
        uuid id PK
        string nama
        string sip_notaris UK
        string alamat
        string nomor_hp
        string email
        boolean is_active
    }
    
    SertifikatFidusia {
        uuid id PK
        uuid aplikasi_kredit_id FK
        uuid notaris_id FK
        string nomor_sertifikat UK
        string nomor_akta
        date tanggal_akta
        date batas_pendaftaran
        boolean is_online_registered
        date tanggal_pendaftaran_ahu
        string kode_billing
        decimal pnrp_amount
        enum status "DRAFT|SUBMITTED|APPROVED|REJECTED|EXPIRED"
        timestamp created_at
    }
    
    PDI_Report {
        uuid id PK
        uuid kendaraan_id FK
        uuid teknisi_id FK
        boolean dewaxing_passed
        boolean fuse_installed
        boolean obd_scan_passed
        boolean fluid_check_passed
        decimal gps_distance_km
        boolean overall_passed
        string document_path
        timestamp inspection_at
        timestamp approved_at
    }
```

*Document Version: 1.0*
*Last Updated: Juni 2026*
*References: IEEE 830 SRS, UU HKPD, PMK 131/2024, Perda DKI No. 1 Tahun 2024, PP 21/2015*
