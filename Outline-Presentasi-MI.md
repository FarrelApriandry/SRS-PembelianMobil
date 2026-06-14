# OUTLINE PENJELASAN DETAIL - SRS MANAJEMEN INFORMASI

> **Tujuan:** Dokumen referensi untuk tim agar memahami secara mendalam hal-hal teknis dan non-teknis yang jarang dipahami orang umum dalam sistem transaksi pembelian kendaraan bermotor terintegrasi di Indonesia.

---

## DAFTAR ISI

1. [Kalkulasi Pajak Kendaraan](#1-kalkulasi-pajak-kendaraan)
2. [Kredit & DP Dinamis (NPF-Based)](#2-kredit--dp-dinamis-npf-based)
3. [Jaminan Fidusia](#3-jaminan-fidusia)
4. [PDI (Pre-Delivery Inspection)](#4-pdi-pre-delivery-inspection)
5. [STCK (Plat Nomor Sementara)](#5-stck-plat-nomor-sementara)
6. [Integrasi API & SLA](#6-integrasi-api--sla)
7. [Dokumen Legalitas](#7-dokumen-legalitas)
8. [Insentif KBLBB (Kendaraan Listrik)](#8-insentif-kblbb-kendaraan-listrik)
9. [Validasi Sistem](#9-validasi-sistem)
10. [ERD & Relasi Database](#10-erd--relasi-database)
11. [Keamanan Data](#11-keamanan-data)

---

## 1. KALKULASI PAJAK KENDARAAN

### 1.1 Apa Itu NJKB?

**NJKB (Nilai Jual Kendaraan Bermotor)** adalah nilai yang ditetapkan oleh pemerintah sebagai dasar pengenaan pajak kendaraan. NJKB **bukan harga jual** di dealer, melainkan nilai yang tercatat di sistem Samsat.

> **Rumus Mencari NJKB dari STNK:**
> ```
> NJKB = PKB Pokok / Tarif Berlaku
> ```

### 1.2 DPP PPN Nilai Lain

**DPP (Dasar Pengenaan Pajak)** untuk PPN kendaraan bermotor dihitung dengan formula khusus:

```
DPP PPN = (11/12) × NJKB
```

**Mengapa 11/12?** Ini adalah aturan dari pemerintah untuk menyamakan taxable value dengan praktik pasar. Artinya, PPN yang dihitung dari DPP akan sama dengan **11% dari NJKB**.

### 1.3 PPN Terutang (PMK 131/2024)

```
PPN = 12% × DPP PPN
    = 12% × (11/12 × NJKB)
    = 11% × NJKB
```

| Komponen | Keterangan |
|----------|------------|
| Tarif PPN | 12% (sesuai PMK 131/2024) |
| DPP | 11/12 × NJKB |
| **PPN Efektif** | **11% × NJKB** |

### 1.4 Opsen (Pajak Opsional) - UU HKPD

Sesuai UU HKPD, ada pungutan tambahan bernama **Opsen** yang dihitung:

```
Opsen PKB = 66% × PKB Pokok
Opsen BBNKB = 66% × BBNKB Pokok
```

### 1.5 ⚠️ ATURAN KHUSUS DKI JAKARTA

DKI Jakarta **TIDAK MEMILIKI** opsen karena tidak memiliki pemerintahan kabupaten/kota otonom:

| Komponen | Provinsi Lain | DKI Jakarta |
|----------|---------------|-------------|
| Opsen PKB | 66% × PKB Pokok | **Rp0** |
| Opsen BBNKB | 66% × BBNKB Pokok | **Rp0** |
| Tarif Pokok BBNKB | 12% | **12,5%** (Perda DKI No.1/2024) |

### 1.6 Pajak Progresif (DKI Jakarta)

Jika NIK pembeli sudah terdaftar memiliki kendaraan roda empat:

| Urutan Kendaraan | Tarif PKB Progresif |
|------------------|---------------------|
| Kendaraan ke-1 | 2,0% |
| Kendaraan ke-2 | 3,0% |
| Kendaraan ke-3 | 4,0% |
| Kendaraan ke-4 | 5,0% |
| Kendaraan ke-5+ | 6,0% (maksimal) |

### 1.7 Contoh Perhitungan Lengkap

**Studi Kasus:**
- NJKB = Rp150.000.000
- Wilayah = Jawa Barat (bukan DKI)
- Kendaraan pertama

**Perhitungan:**

```
PKB Pokok = 2% × Rp150.000.000 = Rp3.000.000
Opsen PKB = 66% × Rp3.000.000 = Rp1.980.000
Total PKB = Rp3.000.000 + Rp1.980.000 = Rp4.980.000

BBNKB Pokok = 12% × Rp150.000.000 = Rp18.000.000
Opsen BBNKB = 66% × Rp18.000.000 = Rp11.880.000
Total BBNKB = Rp18.000.000 + Rp11.880.000 = Rp29.880.000

DPP PPN = (11/12) × Rp150.000.000 = Rp137.500.000
PPN = 12% × Rp137.500.000 = Rp16.500.000
```

---

## 2. KREDIT & DP DINAMIS (NPF-BASED)

### 2.1 Apa Itu NPF?

**NPF (Non-Performing Finance)** adalah rasio kredit macet perusahaan pembiayaan. Semakin rendah NPF, semakin sehat perusahaan leasing tersebut.

```
NPF = (Total Kredit Macet / Total Kredit Diberikan) × 100%
```

### 2.2 Matrix DP Minimum Berdasarkan NPF

| NPF Leasing | DP Minimum | Penjelasan |
|-------------|-------------|------------|
| NPF ≤ 1% | **0%** | Sangat sehat, boleh DP 0% |
| NPF 1% - 3% | **10%** | Sehat, DP minimal 10% |
| NPF 3% - 5% | **15%** | Cukup sehat, DP minimal 15% |
| NPF > 5% | **20%** | Perlu kehati-hatian, DP minimal 20% |

### 2.3 Apa Itu iDeb?

**iDeb (Indonesian Financial Data Exchange)** adalah platform berbagi data kredit yang dikelola oleh **OJK**. Melalui iDeb, leasing bisa mendapatkan:

- Skor kolektibilitas (1-5)
- Riwayat pembayaran kredit
- Status kolektibilitas saat ini

### 2.4 Komponen Kredit

| Komponen | Penjelasan |
|----------|------------|
| **DP (Uang Muka)** | Sebesar X% dari harga OTR, minimum sesuai NPF |
| **Jumlah Pinjaman** | Harga OTR - DP |
| **Bunga Tahunan** | Flat rate atau efektif |
| **Tenor** | Jangka waktu (12-72 bulan) |
| **Angsuran Bulanan** | (Pinjaman + Total Bunga) / Tenor |
| **Total Pembayaran** | DP + (Angsuran × Tenor) |

### 2.5 Simulasi Contoh

```
Harga OTR = Rp200.000.000
NPF Leasing = 2.5% → DP Minimum = 15%
DP = 15% × Rp200.000.000 = Rp30.000.000
Pinjaman = Rp200.000.000 - Rp30.000.000 = Rp170.000.000
Bunga = 8% per tahun (flat)
Tenor = 48 bulan
Total Bunga = Rp170.000.000 × 8% × 4 = Rp54.400.000
Angsuran = (Rp170.000.000 + Rp54.400.000) / 48 = Rp4.675.000/bulan
```

---

## 3. JAMINAN FIDUSIA

### 3.1 Apa Itu Fidusia?

**Fidusia** adalah pengikutan aset (kendaraan) sebagai jaminan pinjaman. Ketika seseorang kredit mobil, BPKB ditahan oleh leasing sebagai jaminan. Pendaftaran fidusia memastikan **hak preferen** leasing terhadap kendaraan tersebut.

### 3.2 Batas Waktu Kritis: 30 Hari Kalender

> ⚠️ **SANGAT PENTING:** Pendaftaran fidusia ke Ditjen AHU wajib dilakukan **MAKSIMAL 30 HARI KALENDER** sejak akta notaris ditandatangani.

```
Batas Pendaftaran = Tanggal Akta Notaris + 30 Hari Kalender
```

**Jika terlambat:**
- Sistem Kemenkumham akan **MENOLAK** permohonan secara otomatis
- Leasing kehilangan hak preferen
- Potensi kerugian besar jika debitur wanprestasi

### 3.3 Alur Pendaftaran Fidusia

```
1. Akta Notaris Jaminan Fidusia ditandatangani
2. Notaris upload ke SABH (Sistem Administrasi Badan Hukum)
3. Sistem generate Kode Billing PNBP
4. Pembayaran PNBP via bank/klikbca
5. Sertifikat Fidusia elektronik terbit
6. Sertifikat didownload & disimpan
```

### 3.4 Biaya PNBP Fidusia

| Jenis Biaya | Estimasi |
|-------------|----------|
| PNBP Pendaftaran Fidusia | Rp50.000 - Rp100.000 |
| Materai | Rp10.000 |
| Fee Notaris | Bervariasi |

---

## 4. PDI (PRE-DELIVERY INSPECTION)

### 4.1 Apa Itu PDI?

**PDI** adalah pemeriksaan kualitas terakhir sebelum mobil diserahkan ke konsumen. PDI memastikan unit dalam kondisi prima dan sesuai spesifikasi.

### 4.2 Checklist Wajib PDI

| No | Tahapan | Detail |
|----|---------|--------|
| 1 | **De-waxing** | Pembilasan lapisan wax pelindung awal dari proses produksi |
| 2 | **Backup Fuse** | Pemasangan fuse cadangan di kotak sekring |
| 3 | **OBD Scan** | Pemindaian port OBD-II untuk cek error codes (DTC) |
| 4 | **DTC Deletion** | Penghapusan DTC error pada ECU jika ada |
| 5 | **Fluid Check** | Pemeriksaan: Oli mesin, coolant, brake fluid, washer fluid |
| 6 | **Uji Jalan** | Test drive minimal **10 km** dengan tracking GPS |

### 4.3 Validasi Uji Jalan

> 📍 **GPS Tracking Wajib:** Sistem mencatat koordinat GPS start dan end untuk membuktikan uji jalan minimal 10 km.

**Jika jarak < 10 km:**
- Sistem akan **MENOLAK** status "Lolos PDI"
- Teknisi harus mengulang uji jalan

### 4.4 Documentasi PDI

| Komponen | Keterangan |
|----------|------------|
| Teknisi Signature | Tanda tangan teknisi yang mengerjakan |
| Supervisor Signature | Tanda tangan pengawas yang approve |
| Document Path | Path file laporan PDI digital |
| Overall Status | LULUS / TIDAK LULUS |

---

## 5. STCK (PLAT NOMOR SEMENTARA)

### 5.1 Apa Itu STCK?

**STCK (Surat Tanda Coba Keliling)** adalah plat nomor sementara berwarna merah-putih yang diberikan untuk kendaraan baru yang belum memiliki plat nomor definitif.

### 5.2 Aturan Kritis STCK

| Aspek | Aturan |
|-------|--------|
| **Masa Berlaku** | **1 bulan** sejak diterbitkan |
| **Keperluan** | Hanya untuk logistik diler-to-konsumen atau uji fisik |
| **Larangan #1** | ❌ Dilarang dikendarai untuk keperluan pribadi di jalan umum |
| **Larangan #2** | ❌ Dilarang digunakan untuk luar kota |
| **Sanksi** | Tilang oleh polisi lalu lintas |
| **Biaya PNBP** | Rp50.000 (resmi) |

### 5.3 Kapan STCK Digunakan?

```
Mobil Baru datang dari pabrik → STCK → Aktivasi plat definitif
                                ↓
                    Uji jalan internal diler
                    Pengiriman ke konsumen (sesama kota)
```

---

## 6. INTEGRASI API & SLA

### 6.1 Arsitektur Async vs Sync

| Aspek | Async (ESB) | Sync (Direct) |
|-------|-------------|---------------|
| **Response Time Rata-rata** | **4.22 detik** | 31.49 detik |
| **Error Rate** | Rendah | **49.83%** |
| **Keandalan** | Tinggi | Rentan timeout |

> 📌 **Keputusan Arsitektur:** Integrasi leasing **WAJIB** menggunakan arsitektur Async berbasis ESB untuk menghindari kegagalan koneksi.

### 6.2 Daftar API & Spesifikasi

#### A. API Dukcapil (Verifikasi Identitas)

| Aspek | Detail |
|-------|--------|
| **Tujuan** | Mencegah identity fraud |
| **Input** | NIK, Foto e-KTP, Swafoto biometrik |
| **Output** | Face matching score (persentase kecocokan) |
| **Threshold** | **≥ 85%** untuk approval |
| **SLA** | Real-time, depends on Dukcapil server |

#### B. API SLIK OJK / PEFINDO (Credit Scoring)

| Aspek | Detail |
|-------|--------|
| **Tujuan** | Analisis kelayakan kredit |
| **Input** | NIK, Nama Lengkap, Tanggal Lahir |
| **Output** | iDeb document, credit score, kolektibilitas |
| **Skala** | 1-5 (1=buruk, 5=baik) |
| **SLA** | Asynchronous via ESB |

#### C. API Samsat / SIGNAL (Verifikasi Kendaraan)

| Aspek | Detail |
|-------|--------|
| **Tujuan** | Validasi STNK & perhitungan pajak |
| **Input** | NRKB (Nomor Plat), NIK, 5 digit nomor rangka |
| **Output** | Status ETLE, masa berlaku STNK, nominal PKB |
| **Penggunaan** | Cek blokir, hitung pajak progresif |
| **SLA** | Depends on SIGNAL server |

#### D. API Ditjen AHU (Fidusia)

| Aspek | Detail |
|-------|--------|
| **Tujuan** | Pendaftaran fidusia elektronik |
| **Input** | Akta notaris, nomor rangka, nilai penjaminan |
| **Output** | Kode billing PNBP, Sertifikat fidusia |
| **Batas Waktu** | **30 hari kalender** |
| **SLA** | Depends on SABH server |

#### E. API TTE / Digital Signature (Privy/VIDA)

| Aspek | Detail |
|-------|--------|
| **Tujuan** | Tanda tangan elektronik tersertifikasi |
| **Input** | Dokumen PDF, Nomor HP (untuk OTP) |
| **Output** | Sertifikat tanda tangan digital |
| **Sertifikasi** | Wajib bersertifikat **Kominfo** |
| **Keabsahan** | Legal sesuai UU ITE |

### 6.3 Graceful Degradation

> 📌 Jika API pemerintah down, sistem tetap berjalan dengan:
> - Notifikasi ke user tentang keterbatasan layanan
> - Retry otomatis ketika server kembali online
> - Log aktivitas untuk audit

---

## 7. DOKUMEN LEGALITAS

### 7.1 Dokumen Mobil Baru

| Dokumen | Keterangan |
|---------|------------|
| **Faktur Kendaraan** | Bukti pembelian dari dealer (asli) |
| **SRUT** | Surat Registrasi Uji Tipe dari Gaikindo |
| **Form A** | Khusus CBU/impor (bukti lulus verifikasi Bea Cukai) |
| **KTP Asli** | Identitas pembeli |
| **KK** | Kartu Keluarga (untuk verifikasi alamat) |

### 7.2 Dokumen Mobil Bekas

#### A. BPKB (Buku Pemilik Kendaraan Bermotor)

| Fitur Keamanan | Penjelasan |
|----------------|------------|
| **Hologram Tri Brata** | Gambar 3D yang berubah warna (akurat ≥ 95%) |
| **Benang UV** | Benang khusus yang hanya terlihat di bawah sinar UV |
| **Watermark** | Tanda air resmi Polri |

> ⚠️ **Deteksi Hologram:** Sistem menggunakan algoritma image classification untuk mendeteksi hologram Tri Brata dengan akurasi minimal **95%**.

#### B. Dokumen Pendukung Mobil Bekas

| Dokumen | Kapan Diperlukan |
|---------|------------------|
| **Kwitansi Kosong (2-3 rangkap)** | Ditandatangani pemilik sesuai BPKB, bermaterai untuk BBN-II |
| **SPH (Surat Pelepasan Hak)** | Khusus eks-aset perusahaan |
| **Kontrak Leasing Lama** | Jika masih dalam kredit aktif |
| **Histori Pembayaran** | Bukti pelunasan dari leasing sebelumnya |

### 7.3 Algoritma Validasi NPWP (Luhn Checksum)

NPWP Indonesia memiliki 15 digit dengan format:
```
XX.XXX.XXX.X-XXX.XXX
```

**Validasi Luhn:**
```
1. Hapus semua titik dan strip
2. Iterasi dari kiri, kalikan digit dengan:
   - Posisi ganjil: dikali 1
   - Posisi genap: dikali 2
3. Jika hasil > 9, kurangi 9
4. Jumlahkan semua hasil
5. Jika habis dibagi 10 → NPWP VALID
```

---

## 8. INSENTIF KBLBB (KENDARAAN LISTRIK)

### 8.1 Apa Itu KBLBB?

**KBLBB (Kendaraan Bermotor Listrik Berbasis Baterai)** adalah kendaraan yang menggunakan tenaga listrik dari baterai.

### 8.2 Syarat Insentif PPN DTP

| Syarat | Detail |
|--------|--------|
| **TKDN** | Content本地 (Tingkat Komponen Dalam Negeri) ≥ **40%** |
| **Jenis** | Kendaraan listrik murni (bukan hybrid) |
| **PPN DTP** | Potongan PPN **10%** |
| **PPN Efektif** | Konsumen hanya bayar **2%** |

### 8.3 Perbandingan

| Tipe Kendaraan | PPN Normal | PPN DTP | Bayar |
|----------------|------------|---------|-------|
| Mobil Bensin | 11% | - | 11% |
| Mobil Hybrid | 11% | ❌ Tidak dapat | 11% |
| Mobil Listrik (TKDN≥40%) | 11% | ✅ 10% | **2%** |

---

## 9. VALIDASI SISTEM

### 9.1 Validasi Face Matching

| Kriteria | Nilai |
|----------|-------|
| **Threshold** | ≥ **85%** |
| **Method** | Biometric face comparison (Dukcapil) |
| **Jika Gagal** | Registrasi DITOLAK, minta verifikasi ulang |

### 9.2 Validasi Hologram BPKB

| Kriteria | Nilai |
|----------|-------|
| **Akurasi Deteksi** | ≥ **95%** |
| **Target** | Hologram Tri Brata |
| **Method** | Image classification (CNN/AlexNet-based) |
| **Jika Gagal** | Dokumen ditandai **GAGAL VERIFIKASI** |

### 9.3 Validasi Uji Jalan PDI

| Kriteria | Nilai |
|----------|-------|
| **Jarak Minimum** | **10 km** |
| **Tracking** | GPS (lat/long start & end) |
| **Jika < 10 km** | Status "Lolos PDI" **TIDAK BISA** diproses |

### 9.4 Validasi Batas Fidusia

| Kriteria | Nilai |
|----------|-------|
| **Batas Waktu** | **30 hari kalender** dari tanggal akta |
| **Method** | Auto-check sebelum submit ke AHU |
| **Jika > 30 hari** | Sistem **MENOLAK** submission |

---

## 10. ERD & RELASI DATABASE

### 10.1 Entitas Utama

```
┌─────────────────┐       ┌─────────────────┐
│     Pembeli     │       │    Kendaraan    │
├─────────────────┤       ├─────────────────┤
│ id (PK)         │──┐    │ id (PK)         │
│ nik             │  │    │ nomor_rangka    │
│ nama_lengkap    │  └───<│ nomor_mesin     │
│ ...             │       │ harga_dasar     │
└─────────────────┘       │ njkb            │
       │                   │ status          │
       │                   └─────────────────┘
       │                          │
       │                          │
       ▼                          ▼
┌─────────────────┐       ┌─────────────────┐
│ AplikasiKredit  │       │ DokumenLegalitas│
├─────────────────┤       ├─────────────────┤
│ id (PK)         │       │ id (PK)         │
│ jumlah_pinjaman │       │ kendaraan_id(FK)│
│ dp_amount       │       │ jenis           │
│ tenor_bulan     │       │ file_path       │
│ status          │       │ is_verified     │
└─────────────────┘       └─────────────────┘
       │
       │ 1:1
       ▼
┌─────────────────┐       ┌─────────────────┐
│ SertifikatFidusa│       │    Notaris      │
├─────────────────┤       ├─────────────────┤
│ id (PK)         │       │ id (PK)         │
│ nomor_sertifikat│       │ nama            │
│ nomor_akta      │       │ sip_notaris     │
│ batas_pendaftaran│      │ is_active       │
└─────────────────┘       └─────────────────┘
       │
       │ 1:1 (sebelum pencairan)
       ▼
┌─────────────────┐
│    PDI_Report   │
├─────────────────┤
│ id (PK)         │
│ kendaraan_id(FK)│
│ dewaxing_passed │
│ obd_scan_passed │
│ uji_jalan_passed│
│ overall_passed  │
└─────────────────┘
```

### 10.2 Penjelasan Relasi

| Relasi | Tipe | Penjelasan |
|--------|------|------------|
| Pembeli → Kendaraan | 1:N | 1 pembeli bisa punya banyak kendaraan |
| Kendaraan → Dokumen | 1:N | 1 kendaraan bisa punya banyak dokumen |
| AplikasiKredit → Notaris | N:1 | Banyak pengajuan ditangani 1 notaris |
| AplikasiKredit → Fidusia | 1:1 | 1 pengajuan = 1 sertifikat fidusia |
| Kendaraan → PDI | 1:1 | 1 kendaraan harus punya 1 laporan PDI |
| Fidusia → PDI | 1:1 | Fidusia baru bisa setelah PDI selesai |

### 10.3 Soft Delete

> ⚠️ **PENTING:** Seluruh entitas menggunakan **SOFT DELETE** - tidak ada penghapusan permanen.

```sql
-- Contoh soft delete
UPDATE Kendaraan SET deleted_at = NOW() WHERE id = 'xxx';

-- Query data aktif
SELECT * FROM Kendaraan WHERE deleted_at IS NULL;
```

**Tujuan:**
- Audit trail lengkap
- Ketertelusuran transaksi
- Kepatuhan regulasi

---

## 11. KEAMANAN DATA

### 11.1 Enkripsi

| Layer | Metode | Penggunaan |
|-------|--------|------------|
| **Data at Rest** | AES-256 | Data tersimpan di database |
| **Data in Transit** | TLS 1.3 | Data berjalan di jaringan |

### 11.2 Autentikasi Multi-Faktor (MFA)

| Role | MFA Required |
|------|--------------|
| Admin Dealer | ✅ Ya |
| Surveyor Leasing | ✅ Ya |
| System Admin | ✅ Ya |
| Pembeli | Opsional |
| Sales Dealer | Opsional |

**Spesifikasi OTP:**
- Format: 6 digit numerik
- Channel: WhatsApp / Email
- Masa berlaku: **5 menit**
- Max attempt: 3x

### 11.3 Role-Based Access Control (RBAC)

| Peran | Hak Akses Utama |
|-------|-----------------|
| **Pembeli Personal** | Isi profil, simulasi, upload KTP, TTE |
| **Sales Dealer** | Buat draf SPK, konfigurasi unit, pantau status |
| **Admin Dealer** | Verifikasi dokumen, daftar Samsat, alokasi PNBP |
| **Inspektor** | Checklist inspeksi mobile, foto/video 360° |
| **Surveyor** | Review aplikasi, akses iDeb, approval limit |
| **Teknisi PDI** | Checklist PDI, daftar rangka ke garansi |

### 11.4 TTE (Tanda Tangan Elektronik)

| Aspek | Detail |
|-------|--------|
| **Penyedia** | Privy / VIDA |
| **Sertifikasi** | Bersertifikat **Kominfo** |
| **Dasar Hukum** | UU ITE |
| **Keabsahan** | Setara tanda tangan basah |

---

## LAMPIRAN: GLOSARIUM

| Singkatan | Kepanjangan | Penjelasan Singkat |
|----------|-------------|-------------------|
| **NJKB** | Nilai Jual Kendaraan Bermotor | Nilai dasar pajak dari pemerintah |
| **DPP** | Dasar Pengenaan Pajak | Nilai yang menjadi dasar perhitungan pajak |
| **PPN** | Pajak Pertambahan Nilai | Pajak atas penjualan barang/jasa |
| **PKB** | Pajak Kendaraan Bermotor | Pajak daerah untuk kendaraan |
| **BBNKB** | Bea Balik Nama Kendaraan Bermotor | Pajak saat perpindahan kepemilikan |
| **Opsen** | Pajak Opsional | Pungutan tambahan 66% dari pajak pokok |
| **NPF** | Non-Performing Finance | Rasio kredit macet leasing |
| **Fidusia** | Jaminan Kebendaan | Hak preferen atas kendaraan |
| **PDI** | Pre-Delivery Inspection | Pemeriksaan kualitas sebelum serah terima |
| **STCK** | Surat Tanda Coba Keliling | Plat nomor sementara |
| **SLIK** | Sistem Layanan Informasi Keuangan | Database kredit OJK |
| **iDeb** | Indonesian Debit Exchange | Platform sharing data kredit |
| **Dukcapil** | Kependudukan dan Catatan Sipil | Instansi verifikasi NIK |
| **Samsat** | Samsat | Sistem pembayaran pajak kendaraan |
| **SIGNAL** | Sistem Informasi Pajak Daerah | Platform Samsat digital |
| **Ditjen AHU** | Direktorat Jenderal Administrasi Hukum Umum | Pendaftaran fidusia |
| **TTE** | Tanda Tangan Elektronik | Signature digital |
| **ESB** | Enterprise Service Bus | Arsitektur integrasi async |
| **TKDN** | Tingkat Komponen Dalam Negeri | Komponen lokal kendaraan |
| **KBLBB** | Kendaraan Bermotor Listrik Berbasis Baterai | Mobil listrik |
| **PPN DTP** | PPN Ditanggung Pemerintah | Subsidi PPN kendaraan listrik |
| **OBD** | On-Board Diagnostics | Port diagnosis kendaraan |
| **DTC** | Diagnostic Trouble Code | Kode error di ECU |
| **BPKB** | Buku Pemilik Kendaraan Bermotor | Buku registrasi kendaraan |
| **STNK** | Surat Tana Nomor Kendaraan | Surat nomor kendaraan |
| **SRUT** | Surat Registrasi Uji Tipe | Sertifikasi tipe kendaraan |
| **CBU** | Completely Built Up | Kendaraan impor utuh (vs CKD) |
| **SPH** | Surat Pelepasan Hak | Dokumen pelepasan aset perusahaan |
| **SWDKLLJ** | Sosial Dengan Jasa Raharja | Asuransi wajib |
| **PNBP** | Penerimaan Negara Bukan Pajak | Biaya administrasi pemerintah |
| **RBAC** | Role-Based Access Control | Kontrol akses berdasarkan peran |
| **MFA** | Multi-Factor Authentication | Autentikasi berlapis |
| **AES** | Advanced Encryption Standard | Standar enkripsi |
| **TLS** | Transport Layer Security | Protokol keamanan jaringan |
| **SABH** | Sistem Administrasi Badan Hukum | Portal Ditjen AHU |
| **ETLE** | Electronic Traffic Law Enforcement | Tilang elektronik (blokir kendaraan) |

---

*Document Version: 1.0*
*Last Updated: June 2026*
*Project: SRS Sistem Transaksi Pembelian Kendaraan Bermotor Terintegrasi*
