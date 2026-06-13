# Analisis Sistem Transaksi, Struktur Fiskal, dan Manajemen Dokumen Kendaraan Bermotor di Indonesia

Panduan Komprehensif Penyusunan Dokumen Spesifikasi Kebutuhan Perangkat Lunak (SRS)

---

## Daftar Isi

1. [Analisis Regulasi Fiskal & Struktur Pajak](#1-analisis-regulasi-fiskal--struktur-pajak)
2. [Tata Kelola Pajak Progresif](#2-tata-kelola-pajak-progresif)
3. [Manajemen Dokumen Legalitas](#3-manajemen-dokumen-legalitas)
4. [Pre-Delivery Inspection (PDI) & Logistik](#4-pre-delivery-inspection-pdi--logistik)
5. [Regulasi STCK & Pelat Nomor](#5-regulasi-stck--pelat-nomor)
6. [Arsitektur Pembiayaan & OJK](#6-arsitektur-pembiayaan--ojk)
7. [Integrasi API & Arsitektur O2O](#7-integrasi-api--arsitektur-o2o)

---

## 1. Analisis Regulasi Fiskal & Struktur Pajak

### 1.1 Komponen Perpajakan Kendaraan Baru

Pembelian kendaraan bermotor roda empat baru di Indonesia melibatkan struktur perpajakan berlapis yang menggabungkan instrumen pajak pemerintah pusat dan pemerintah daerah. Berdasarkan **UU HKPD** (UU No. 1 Tahun 2022) serta **PMK 131/2024**, pembeli dibebankan setidaknya **tujuh komponen biaya fiskal**.

### 1.2 Dasar Pengenaan Pajak (DPP) & PPN

Penetapan nilai pajak kendaraan mengacu pada **Nilai Jual Kendaraan Bermotor (NJKB)**, yaitu nilai standar yang ditetapkan oleh Dispenda melalui data keagenan APM.

#### Formula DPP PPN Nilai Lain

```
DPP PPN Nilai Lain = (11/12) × NJKB
```

#### Formula PPN Terutang (Tarif 12%)

```
PPN Terutang = 12% × DPP PPN Nilai Lain
PPN Terutang = 12% × (11/12 × NJKB) = 11% × NJKB
```

### 1.3 Sistem Opsen (UU HKPD)

Di tingkat daerah, PKB dan BBNKB dikenakan biaya tambahan berupa **Opsen** dengan tarif **66%** dari nilai pajak pokok:

```
Total PKB = PKB Pokok + Opsen PKB = PKB Pokok + (66% × PKB Pokok)
Total BBNKB = BBNKB Pokok + Opsen BBNKB = BBNKB Pokok + (66% × BBNKB Pokok)
```

### 1.4 Tabel Komponen Biaya & Pajak

| Komponen | Tarif/Ketentuan | DPP | Otoritas |
|----------|----------------|-----|----------|
| **PPN** | 12% (Mekanisme Nilai Lain) | (11/12) × NJKB | DJP (Pusat) |
| **PPnBM** | Variatif (contoh: 15%) | DPP Nilai Lain | DJP (Pusat) |
| **PKB Pokok** | 1,2% - 2% (kepemilikan pertama) | NJKB × Koefisien Jalan | Pemerintah Provinsi |
| **Opsen PKB** | 66% dari PKB Pokok | PKB Pokok Terutang | Pemerintah Kab/Kota |
| **BBNKB Pokok** | Maksimal 12% | NJKB | Pemerintah Provinsi |
| **Opsen BBNKB** | 66% dari BBNKB Pokok | BBNKB Pokok Terutang | Pemerintah Kab/Kota |
| **SWDKLLJ** | Rp143.000 - Rp150.000 | Flat | Jasa Raharja |
| **STNK** | Rp200.000 | Per penerbitan | Polri (PNBP) |
| **TNKB** | Rp100.000 | Per penerbitan | Polri (PNBP) |
| **BPKB** | Rp375.000 | Per penerbitan | Polri (PNBP) |

### 1.5 Simulasi Perhitungan

#### Parameter Perhitungan

| Parameter | Simulasi A (NJKB Rp500 juta) | Simulasi B (NJKB Rp175 juta) |
|-----------|-------------------------------|-------------------------------|
| **NJKB** | Rp500.000.000 | Rp175.000.000 |
| **DPP PPN Nilai Lain** | Rp458.333.333 | Rp160.416.667 |
| **DPP PPnBM (15%)** | Tidak diaplikasikan | Rp183.750.000 |
| **PPN Terutang (12%)** | Rp55.000.000 | Rp22.050.000 |
| **PPnBM (15% × DPP)** | Rp0 | Rp27.562.500 |
| **PKB Pokok (1,2%)** | Rp6.000.000 | Rp2.100.000 |
| **Opsen PKB (66%)** | Rp3.960.000 | Rp1.386.000 |
| **BBNKB Pokok (12%)** | Rp60.000.000 | Rp21.000.000 |
| **Opsen BBNKB (66%)** | Rp39.600.000 | Rp13.860.000 |
| **Biaya PNBP & SWDKLLJ** | Rp300.000 | Rp818.000 |
| **Total Pajak & Biaya** | Rp109.860.000 | Rp39.164.000 |
| **Total Transaksi Akhir** | Rp664.860.000 | Rp263.776.500 |

---

## 2. Tata Kelola Pajak Progresif

### 2.1 Definisi & Latar Belakang

Pajak progresif merupakan kebijakan fiskal daerah yang mengenakan tarif pajak lebih tinggi secara bertahap bagi individu yang memiliki **lebih dari satu kendaraan roda empat** terdaftar atas nama dan alamat tempat tinggal yang sama.

**Tujuan kebijakan:**
- Mengendalikan kepadatan lalu lintas
- Menekan polusi udara
- Menciptakan keadilan distribusi beban infrastruktur jalan

**Catatan penting:** Pajak progresif **tidak berlaku** lintas jenis kendaraan berbeda (mobil vs motor).

### 2.2 Perbandingan Tarif Perda

| Urutan Kepemilikan | Perda 2/2015 | Perda 1/2024 (Terbaru) |
|--------------------|---------------|-------------------------|
| Kendaraan Ke-1 | 2,0% | 2,0% |
| Kendaraan Ke-2 | 2,5% | 3,0% |
| Kendaraan Ke-3 | 3,0% | 4,0% |
| Kendaraan Ke-4 | 3,5% | 5,0% |
| Kendaraan Ke-5 dst. | Eskalasi +0,5% s.d. 10% | 6,0% (Flat Maksimal) |

### 2.3 Formula Pencarian NJKB dari STNK

```
NJKB = PKB Pokok / Tarif Berlaku
```

### 2.4 Simulasi Pajak Progresif

*Contoh: 3 kendaraan dengan NJKB sama Rp75.000.000, SWDKLLJ Rp150.000*

| Urutan Mobil | Komponen | Perda 2/2015 | Perda 1/2024 |
|--------------|----------|--------------|--------------|
| **Mobil Pertama** | PKB Pokok (2%) | Rp1.500.000 | Rp1.500.000 |
| | SWDKLLJ | Rp150.000 | Rp150.000 |
| | **Total** | **Rp1.650.000** | **Rp1.650.000** |
| **Mobil Kedua** | PKB Pokok | Rp1.875.000 (2,5%) | Rp2.250.000 (3%) |
| | SWDKLLJ | Rp150.000 | Rp150.000 |
| | **Total** | **Rp2.025.000** | **Rp2.400.000** |
| **Mobil Ketiga** | PKB Pokok | Rp2.250.000 (3%) | Rp3.000.000 (4%) |
| | SWDKLLJ | Rp150.000 | Rp150.000 |
| | **Total** | **Rp2.400.000** | **Rp3.150.000** |

---

## 3. Manajemen Dokumen Legalitas

### 3.1 Klasifikasi Dokumen

Dokumen legalitas diklasifikasikan berdasarkan **status transaksi** kendaraan:

- **Kendaraan Baru (Brand New)**
- **Kendaraan Bekas (Pre-owned)**

### 3.2 Verifikasi BPKB Asli

Pada transaksi mobil bekas, keaslian **BPKB** diverifikasi melalui:

- ✅ Keberadaan hologram **Tri Brata Polri** di halaman awal
- ✅ Nomor BPKB tercetak vertikal di sisi kanan
- ✅ Benang pengaman **ultraviolet** (terlihat di bawah sinar UV)
- ✅ Warna cokelat kehijauan dengan **20 halaman**

### 3.3 Matriks Validasi Dokumen

| Nama Dokumen | Jenis Transaksi | Aturan Validasi | Status |
|--------------|-----------------|-----------------|--------|
| **Faktur Kendaraan** | Baru & Bekas | Pencocokan nomor rangka/mesin | Wajib |
| **BPKB** | Bekas | Verifikasi hologram Tri Brata & benang UV | Wajib (Asli) |
| **STNK** | Bekas | Verifikasi masa aktif pajak & STNK 5 tahunan | Wajib |
| **Kwitansi Kosong (2 rangkap)** | Bekas | Rangkap 1: tanda tangan, Rangkap 2: materai | Wajib |
| **Surat Pelepasan Hak (SPH)** | Bekas | Validasi tanda tangan & stempel basah | Wajib (eks-perusahaan) |
| **Form A** | Bekas (CBU) | Pencocokan bea masuk dengan Ditjen Bea Cukai | Wajib |
| **Risalah Lelang** | Bekas | Dari balai lelang resmi | Wajib (via lelang) |
| **Buku KEUR** | Bekas (Niaga) | Masa berlaku KIR | Wajib |
| **Dokumen Kredit Aktif** | Bekas | Fotokopi BPKB, kontrak, histori bayar | Wajib (kredit aktif) |

### 3.4 Dokumen Tambahan untuk Kredit Aktif

Jika mobil bekas masih dalam **cicilan/kredit**:

1. Fotokopi BPKB terlegalisir
2. Kontrak perjanjian pembiayaan dari leasing
3. Fotokopi histori pembayaran terakhir
4. Bukti setoran terakhir

---

## 4. Pre-Delivery Inspection (PDI) & Logistik

### 4.1 Definisi & Tujuan

**Pre-Delivery Inspection (PDI)** adalah prosedur pemeriksaan kualitas berlapis yang wajib dijalankan diler sebelum unit diserahkan kepada konsumen.

**Tujuan:**
- Mitigasi risiko cacat produksi bawaan pabrik
- Mitigasi kerusakan selama transportasi logistik
- Memastikan kendaraan aman untuk jalan raya
- Menjaga warranty compliance

### 4.2 Checklist PDI Terstandarisasi

#### Tahap 1: Dekonservasi Eksterior
Pembersihan lapisan **wax pelindung** yang disemprotkan di pabrik untuk melindungi bodi mobil dari korosi selama penyimpanan di pelabuhan/gudang.

#### Tahap 2: Aktivasi Kelistrikan
Pemasangan kembali **backup fuse** kelistrikan utama yang dilepas saat pengiriman untuk mencegah kebocoran daya baterai.

#### Tahap 3: Penghapusan DTC (Diagnostic Trouble Codes)
Penggunaan perangkat pemindai sistem ke **port OBD-II** untuk:
- Memindai DTC yang terekam di ECU
- Memastikan ketiadaan malfungsi
- Menghapus kode eror

#### Tahap 4: Pemeriksaan Kompartemen Mesin & Cairan
Pengisian dan pemeriksaan:
- Oli mesin
- Minyak rem
- Cairan pendingin
- Minyak power steering
- Air wiper
- Pengunci kap mesin
- Terminal baterai

#### Tahap 5: Inspeksi Interior & Aksesoris
Pengujian:
- Freeplay pedal
- Fungsionalitas AC
- Pelurusan kemudi
- Radio/audio presets
- Wiper
- Lampu kabin
- Sabuk pengaman
- Kunci cadangan

#### Tahap 6: Uji Jalan Komprehensif
Pengujian selama **minimal 10 kilometer**:
- Akselerasi
- Pengereman
- Transmisi
- Sasis
- Wheel alignment
- Deteksi getaran/bunyi aneh

### 4.3 Dokumen Penyerahan Unit

Bersamaan dengan penyerahan kendaraan:

| Dokumen | Keterangan |
|---------|------------|
| Faktur penjualan | Invoice resmi dari diler |
| Kuitansi pembayaran | Bukti pembayaran sah |
| STCK | Surat coba kendaraan sementara |
| Polis asuransi | Perlindungan kendaraan |
| Buku garansi | Garansi resmi pabrikan |
| Buku manual | Owner's manual |
| Sertifikat PUC | Pollution Under Control (jika required) |

---

## 5. Regulasi STCK & Pelat Nomor

### 5.1 Definisi STCK

**Surat Tanda Coba Kendaraan (STCK)** adalah dokumen jalan sementara untuk kendaraan baru yang belum memiliki STNK asli.

### 5.2 Ketentuan Operasional

Berdasarkan **Perkapolri 7/2021**:

| Aspek | Ketentuan |
|-------|----------|
| **Fungsi** | Pemindahan pabrik→diler, diler→konsumen, uji fisik Samsat |
| **Masa berlaku** | 1 (satu) bulan |
| **Pelanggaran** | Tilang jika digunakan untuk keperluan pribadi di jalan raya |
| **Larangan** | Dilarang dikendarai keluar kota |
| **Biaya PNBP** | Rp50.000 (standar) |
| **Biaya tambahan** | Rp1.500.000 (pelat hitam sementara) |

### 5.3 Karakteristik Pelat Nomor Sementara

- Latar **putih** dengan tulisan **merah**
- Tidak dianggap dokumen jalan raya resmi untuk publik
- Hanya untuk konteks logistik diler-konsumen

---

## 6. Arsitektur Pembiayaan & OJK

### 6.1 Regulasi Batas Minimum DP

Berdasarkan **POJK 35/2018** dan **POJK 46/2024**:

| Rasio NPF Net | DP Motor (Roda 2/3) | DP Mobil Non-Produktif | DP Mobil Produktif |
|---------------|---------------------|------------------------|-------------------|
| ≤ 1% | 0% | 0% | 0% |
| 1% - 3% | 10% | 10% | 10% |
| 3% - 5% | 15% | 15% | 15% |
| > 5% | 20% | 20% | 15% |

### 6.2 Subsidi Kendaraan Listrik (KBLBB)

| TKDN | Insentif PPN DTP | PPN Efektif |
|------|------------------|-------------|
| ≥ 40% (BEV) | 10% | 2% |
| 20% - 40% (Bus) | 5% | 7% |
| Hybrid | Tidak ada | 11% |

### 6.3 Prosedur Jaminan Fidusia

Berdasarkan **UU 42/1999** dan **PP 21/2015**:

| Aspek | Ketentuan |
|-------|----------|
| **Batas pendaftaran** | Maksimal **30 hari kalender** sejak akta notaris |
| **Konsekuensi terlambat** | Penolakan otomatis oleh sistem AHU |
| **Hak preferen** | Kreditur didahulukan dalam pembagian piutang |
| **Larangan** | Debitur dilarang mengalihkan/gadai objek fidusia tanpa persetujuan kreditur |

---

## 7. Integrasi API & Arsitektur O2O

### 7.1 Spesifikasi API Eksternal

| API | Tujuan | Output |
|-----|--------|--------|
| **Dukcapil** | Verifikasi NIK & face matching | Face match score |
| **SLIK OJK / PEFINDO** | Credit checking | Dokumen iDeb, kolektibilitas |
| **Samsat / Bapenda** | Validasi STNK, ETLE, pajak progresif | Status STNK, nominal PKB |
| **Ditjen AHU (Fidusia)** | Pendaftaran fidusia elektronik | Sertifikat fidusia, kode PNBP |
| **Payment Gateway** | VA dinamis, instant settlement | Konfirmasi pembayaran |
| **Digital Signature (TTE)** | Tanda tangan digital tersertifikasi | Sertifikat Kominfo |

### 7.2 Alur Kerja O2O Terintegrasi

```
┌──────────────────────────────────────────────────────────────────────┐
│                     MODEL O2O (ONLINE-TO-OFFLINE)                    │
└──────────────────────────────────────────────────────────────────────┘

[TAHAP 1] ──────▶ [TAHAP 2] ──────▶ [TAHAP 3] ──────▶ [TAHAP 4]
 Inisiasi          Inspeksi          Penawaran        Pembayaran
 Online            Fisik             Final            Instan
     │                │                │                │
     ▼                ▼                ▼                ▼
 Estimasi         Jadwalkan         Analisis         Transfer
 harga awal      inspeksi (150-    inspeksi &       dana <60
 via algorithm    188 titik)         dokumen          menit
```

#### Tahap 1: Inisiasi Transaksi & Estimasi Online
- Konsumen memilih unit baru atau upload spesifikasi mobil bekas
- Sistem rilis estimasi harga awal berdasarkan pricing algorithm

#### Tahap 2: Penjadwalan Inspeksi Fisik Digital
- **Home Inspection** atau datang ke diler
- Inspektor periksa 150-188 titik (standar Caroline.id/OLXmobbi)
- Upload hasil real-time ke sistem

#### Tahap 3: Penawaran Harga Final & Transparansi
- Sistem analisis data inspeksi + dokumen legalitas
- Kalau lolos verifikasi → harga penawaran final objektif
- Generate draf dokumen transaksi otomatis

#### Tahap 4: Pembayaran Instan & Serah Terima (Instant Payout)
- Pencairan dana ke rekening penjual **maksimal 60 menit**
- Serah terima dokumen asli (BPKB, STNK, Faktur) setelah dana confirmed

#### Tahap 5: Aktivasi Garansi & Layanan Purnajual
- Aktivasi otomatis **garansi mesin & transmisi 1 tahun**
- Integrasi CRM untuk pengingat servis berkala

---

## Referensi

| No | Sumber |
|----|--------|
| 1 | cnbcindonesia.com - Berlaku Tahun 2025, Beli Mobil Bayar 7 Komponen Pajak |
| 2 | oto.detik.com - Pajak Tinggi Bikin Orang RI Mikir-mikir Beli Mobil Baru |
| 3 | klikpajak.id - Cara Menghitung Pajak Kendaraan dan Jenis Pajaknya |
| 4 | wuling.id - Pajak Progresif Mobil dan Cara Menghitungnya |
| 5 | cimbniaga.co.id - Pengertian Pajak Progresif, Cara Menghitung |
| 6 | pajak.go.id - PMK 131/2024: Tarif PPN Sebelas-Dua Belas |
| 7 | auto2000.co.id - Tarif Pajak Progresif Kendaraan Bermotor Terlengkap 2026 |
| 8 | bimtekedukasi.com - Bimtek Implementasi Verifikasi Dan Validasi Data Via Dukcapil API |
| 9 | blog.ibid.astra.co.id - 6 Dokumen Jual Beli Mobil Bekas yang Perlu Disiapkan |
| 10 | jualmobilmu.id - 8 Dokumen Penting Transaksi Jual Beli Mobil |
| 11 | caroline.id - Step by Step Jual Mobil Bekas Aman Tanpa Calo |
| 12 | help.olxmobbi.co.id - FAQ Menjual Mobil Instan di OLXmobbi |
| 13 | ackodrive.com - Pre-delivery Inspection (PDI) Checklist for New Cars |
| 14 | ventavid.com - PDI - Pre-Delivery Inspection |
| 15 | hyundaimobil.co.id - Pre-Delivery Order Inspection |
| 16 | daihatsu.co.id - Temporary Registration Certificate for New Cars |
| 17 | ojk.go.id - Press Release OJK Launches Financial Information Services System (SLIK) |
| 18 | blog.itsjack.com - The Benefits of SLIK OJK for Creditors & Customers |
| 19 | indonesiabaik.id - Ketentuan DP Nol Persen Kendaraan Bermotor |
| 20 | ojk.go.id - POJK Nomor 46 Tahun 2024 Pengembangan dan Penguatan Perusahaan Pembiayaan |
| 21 | otomotif.kompas.com - OJK Izinkan DP 0 Persen, Pilihan Mobil Listrik |
| 22 | fh.unram.ac.id - Pelaksanaan Pendaftaran Jaminan Fidusia Secara Elektronik |
| 23 | yaplegal.id - Ahu Fidusia: Aset Bisnis & Kunci Keamanan Transaksi |
| 24 | indonesiancloud.com - Understanding SLIK OJK and How to Check SLIK Online |
| 25 | portal.ahu.go.id - Buku Petunjuk Pendaftaran Jaminan Fidusia Online |
| 26 | caroline.id - Ranking Marketplace Mobil Bekas 2026 di Indonesia |

---

*Dokumen ini digunakan sebagai referensi riset untuk penyusunan SRS Sistem Transaksi Pembelian Kendaraan Bermotor Terintegrasi (Indonesia)*
