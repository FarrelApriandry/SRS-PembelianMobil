# Agent Role: Senior Automotive System Analyst & SRS Architect

## Project Goal

Membangun dokumen Spesifikasi Kebutuhan Perangkat Lunak (SRS) yang komprehensif untuk "Sistem Transaksi Pembelian Kendaraan Bermotor (Mobil) Terintegrasi di Indonesia".

## Project Directory & Context Map

* Informasi dasar bisnis & alur: Lihat `memory-bank/project-overview.md`
* Integrasi API & Sistem eksternal: Lihat `memory-bank/architecture.md`
* Aturan hukum perpajakan, OJK, Dokumen & PDI: Lihat `memory-bank/conventions.md`
* Output Target: Tulis dan perbarui dokumen SRS di `docs/srs-template.md`

## Operational Rules for AI

1. Jangan membuat asumsi tentang aturan pajak di Indonesia. Selalu rujuk `memory-bank/conventions.md`.
2. Jika ada perintah penulisan, lakukan secara bertahap (incremental). Tulis satu bab SRS, minta feedback, lalu lanjutkan ke bab berikutnya.
3. Gunakan standar IEEE 830 untuk format dokumen SRS di `docs/srs-template.md`.

#### 2. `memory-bank/project-overview.md` (Gambaran Umum Proses Bisnis)

File ini mendefinisikan proses bisnis pembelian mobil di Indonesia yang akan dimasukkan ke dalam SRS (Aktor, Kasus Penggunaan, dan Alur Kerja).

**Informasi penting yang harus Anda masukkan:**

* **Aktor Utama:** Pembeli (Perorangan/Perusahaan), Sales Dealer, Admin Dealer, Surveyor Leasing (*Multifinance*), dan Petugas PDI/Logistik.


* **Alur Kerja Online-to-Offline (O2O):** Estimasi harga online -> inspeksi fisik (di diler atau rumah) -> penawaran harga final -> tanda tangan digital -> pencairan dana instan -> serah terima unit.


* **Skema Pembayaran:** Pembayaran Tunai (*Cash*) dan Kredit (*Leasing/Multifinance*).



#### 3. `memory-bank/architecture.md` (Peta Integrasi API Sistem)

AI membutuhkan panduan arsitektur sistem agar kebutuhan fungsional antarmuka eksternal (*External Interface Requirements*) dalam SRS tidak salah tulis.

**Informasi API wajib Indonesia yang harus dimasukkan:**

| Nama API | Tujuan Integrasi | Output Data |
| --- | --- | --- |
| **API Dukcapil** | Verifikasi KTP Pembeli | Kesesuaian NIK, foto wajah (*face matching*). |
| **API SLIK OJK / PEFINDO** | Kelayakan kredit instan | Skor kolektibilitas kredit (iDeb). |
| **API Samsat / SIGNAL** | Verifikasi mobil bekas & pajak | Status blokir ETLE, masa berlaku STNK, nilai PKB. |
| **API Ditjen AHU (Fidusia)** | Pendaftaran hak preferen diler | Sertifikat jaminan fidusia elektronik. |
| **API Digital Signature (Privy/VIDA)** | Keabsahan kontrak tanpa kertas | Tanda tangan tersertifikasi Kominfo. |

#### 4. `memory-bank/conventions.md` (Aturan Bisnis & Regulasi Hukum Terkeras)

Ini adalah "senjata utama" agar AI Anda tidak mengalami *lack of information*. Semua perhitungan matematis, kebijakan OJK, dan denda harus ditulis secara deklaratif di sini.

**Rumus Perpajakan Indonesia yang Harus Dicantumkan:**

* **Dasar Pengenaan Pajak (DPP) PPN Nilai Lain:**
`$\text{DPP PPN}=\frac{11}{12}\times\text{NJKB}$` 


* **PPN Terutang (Tarif 12% sesuai PMK 131/2024):**
`$\text{PPN}=12\%\times\text{DPP PPN}=11\%\times\text{NJKB}$` 


* **Sistem Opsen UU HKPD:**
`$\text{Opsen PKB}=66\%\times\text{PKB Pokok}$` 
`$\text{Opsen BBNKB}=66\%\times\text{BBNKB Pokok}$` 


* **Pajak Progresif (DKI Jakarta Perda 1/2024):** Mobil ke-1 (2%), ke-2 (3%), ke-3 (4%), ke-4 (5%), ke-5 dst (6%). Rumus pencarian NJKB standar dari lembar STNK:
`$\text{NJKB}=\frac{\text{PKB Pokok}}{\text{Tarif Berlaku}}$` 



**Aturan Validasi Dokumen:**

* Untuk mobil bekas, wajib memvalidasi ketersediaan kwitansi kosong (2 rangkap ditandatangani sesuai BPKB) , Surat Pelepasan Hak (SPH) jika eks-perusahaan , dan Form A jika mobil CBU.



**Regulasi OJK & Kredit:**

* DP Minimum konvensional dihitung berdasarkan rasio NPF perusahaan pembiayaan (NPF $\le$ 1% = DP 0%, NPF 1-3% = DP 10%, NPF 3-5% = DP 15%, NPF > 5% = DP 20%).


* Pendaftaran Jaminan Fidusia ke Kemenkumham memiliki batas waktu mutlak **maksimal 30 hari kalender** sejak pembuatan akta notaris.


* Subsidi mobil listrik (PPN DTP) hanya berlaku untuk unit dengan TKDN $\ge$ 40% (potongan PPN 10%, sehingga pembeli hanya membayar PPN efektif 2%).



**Prosedur QC & Pengiriman:**

* **Pre-Delivery Inspection (PDI):** Mobil harus melewati de-waxing bodi , pemasangan *backup fuse* , penghapusan DTC eror pada ECU , dan uji jalan minimal 10 km sebelum serah terima.


* **STCK (Pelat Sementara):** Masa berlaku hanya **1 bulan**. Hanya boleh digunakan untuk logistik diler-ke-konsumen atau uji fisik, dilarang dikendarai untuk keperluan pribadi di jalan umum (bisa ditilang) dan dilarang ke luar kota. Cost PNBP resmi Rp50.000.