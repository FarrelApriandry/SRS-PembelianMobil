# SPESIFIKASI KEBUTUHAN PERANGKAT LUNAK (SRS)

## SISTEM TRANSAKSI PEMBELIAN KENDARAAN BERMOTOR INTEGRASI (INDONESIA)

**Versi:** 1.0  
**Tanggal:** 13 Juni 2026  
**Status:** Draft  
**Standar:** ISO/IEC/IEEE 29148:2018  

---

### BAB 1: PENDAHULUAN

*(Telah selesai - lihat versi sebelumnya)*

#### 1.1 Tujuan Dokumen SRS

Dokumen Spesifikasi Kebutuhan Perangkat Lunak (SRS) ini bertujuan untuk mendefinisikan secara lengkap dan terperinci seluruh kebutuhan sistem informasi yang akan dikembangkan untuk memfasilitasi transaksi pembelian kendaraan roda empat (mobil) di Indonesia.

#### 1.2 Ruang Lingkup Produk

Sistem Transaksi Pembelian Kendaraan Bermotor Terintegrasi dirancang untuk mendukung dua skenario transaksi utama: Transaksi Kendaraan Baru, Transaksi Kendaraan Bekas, dan Modul Financing (Pembiayaan Kredit).

#### 1.3 Definisi, Akronim, dan Singkatan (Glosarium)

(Telah terdefinisi 22 istilah resmi Indonesia: BBNKB, BPKB, DPP, DUKCAPIL, ETLE, Fidusia, iDeb, KBLBB, NJKB, NPF, Opsen, PKB, PNBP, PPnBM, PPN, SLIK, SRUT, STCK, STNK, SWDKLLJ, TKDN, TNKB, TTE)

#### 1.4 Referensi Regulatory dan Standar

(Telah terdefinisi: UU HKPD, PMK 131/2024, Perda Pajak Progresif, PP 21/2015, Perkapolri 7/2021, ISO/IEC/IEEE 29148:2018)

#### 1.5 Konvensi Dokumen

(Telah terdefinisi: Modal verbs shall/should/may, Penomoran REQ-F-XXX/REQ-NF-XXX/REQ-TAX-XXX/REQ-DOC-XXX/REQ-INT-XXX)

---

### BAB 2: DESKRIPSI KESELURUHAN

*(Telah selesai - lihat versi sebelumnya)*

#### 2.1 Perspektif Produk

Context Diagram, Use Case Diagram (14 UC), dan tabel hubungan sistem eksternal telah terdefinisi.

#### 2.2 Fungsi Produk

5 modul utama telah terdefinisi: pricing Engine, Inspeksi Fisik, Verifikasi Dokumen, Fintech/Leasing, dan Logistik/PDI.

#### 2.3 Karakteristik Pengguna (User Classes)

6 user classes telah terdefinisi dengan kebutuhan khusus (REQ-F-001 hingga REQ-F-006).

#### 2.4 Batasan Sistem

Batasan fungsional, teknis, dan regulatory telah terdefinisi.

#### 2.5 Asumsi dan Ketergantungan

7 asumsi operasional dan ketergantungan eksternal telah terdefinisi.

---

### BAB 3: KEBUTUHAN SPESIFIK SISTEM

Bab ini menjabarkan seluruh kebutuhan fungsional sistem yang dikategorikan berdasarkan modul bisnis utama. Setiap kebutuhan shall memiliki ID unik, deskripsi lengkap, input/proses/output, prakondisi, pascakondisi, dan kriteria penerimaan (acceptance criteria) sesuai standar IEEE 29148.

#### 3.1 Kebutuhan Modul Pricing Engine (Kalkulasi Pajak)

Modul pricing engine shall menyediakan fitur kalkulasi harga komprehensif yang mencakup seluruh komponen pajak dan retribusi sesuai regulasi yang berlaku di Indonesia.

**3.1.1 Kebutuhan Perhitungan PPN (Pajak Pertambahan Nilai)**

**REQ-TAX-001: Perhitungan DPP PPN Nilai Lain**  
Sistem shall menghitung Dasar Pengenaan Pajak (DPP) untuk PPN menggunakan mekanisme Nilai Lain dengan formula:

```
DPP PPN = (11/12) × NJKB
```

- **Input:** Nilai Jual Kendaraan Bermotor (NJKB) dari database harga atau hasil kalkulasi
- **Proses:** Implementasi formula matematika dengan presisi floating-point
- **Output:** Nilai DPP PPN dalam mata uang IDR dengan 2 desimal
- **Prakondisi:** NJKB valid dan lebih besar dari nol
- **Pascakondisi:** DPP PPN siap digunakan untuk kalkulasi PPN
- **Acceptance Criteria:** Hasil kalkulasi akurat hingga 2 desimal, perbedaan dengan perhitungan manual maksimal Rp1

**REQ-TAX-002: Perhitungan PPN Terutang**  
Sistem shall menghitung PPN terutang dengan tarif 12% dari DPP PPN Nilai Lain sesuai PMK 131/2024:

```
PPN = 12% × DPP PPN = 11% × NJKB
```

- **Input:** Nilai DPP PPN dari REQ-TAX-001
- **Proses:** Kalkulasi PPN = 12% × DPP
- **Output:** Nilai PPN dalam mata uang IDR dengan 2 desimal
- **Prakondisi:** DPP PPN sudah terhitung
- **Pascakondisi:** PPN siap diikutsertakan dalam total harga on-the-road
- **Acceptance Criteria:** Hasil sesuai formula, tariff 12% tidak dapat dimodifikasi

**REQ-TAX-003: Deteksi dan Penerapan Tarif PPN DTP untuk KBLBB**  
Sistem shall mendeteksi kendaraan yang memenuhi syarat untuk insentif PPN Ditanggung Pemerintah (PPN DTP) dan secara otomatis mengurangi beban PPN pembeli:

- **Input:** Data tipe kendaraan (BEV/Hybrid), nilai TKDN (persentase)
- **Proses:** Validasi TKDN ≥ 40% dan jenis kendaraan = KBLBB (bukan hybrid konvensional)
- **Output:** Nilai PPN yang dibayarkan konsumen (2% dari NJKB untuk KBLBB, 11% untuk non-KBLBB)
- **Prakondisi:** Tipe kendaraan dan nilai TKDN tersedia di database
- **Pascakondisi:** Sistem menampilkan harga dengan atau tanpa insentif PPN DTP
- **Acceptance Criteria:** Hanya kendaraan dengan TKDN ≥ 40% yang получит insentif, hybrid konvensional tidak mendapat insentif

**3.1.2 Kebutuhan Perhitungan PKB dan Opsen PKB**

**REQ-TAX-004: Perhitungan PKB Pokok**  
Sistem shall menghitung Pajak Kendaraan Bermotor (PKB) pokok dengan formula:

```
PKB Pokok = Tarif PKB × NJKB × Koefisien Kerusakan Jalan
```

- **Input:** NJKB, tarif PKB (default 2% untuk kepemilikan pertama), koefisien kerusakan jalan
- **Proses:** Kalkulasi PKB dengan pembulatan ke ribuan terdekat ke atas
- **Output:** Nilai PKB Pokok dalam IDR
- **Prakondisi:** NJKB valid dan tarif PKB diketahui
- **Pascakondisi:** PKB Pokok siap dikombinasikan dengan Opsen PKB
- **Acceptance Criteria:** PKB maksimal 2% dari NJKB untuk kepemilikan pertama

**REQ-TAX-005: Perhitungan Opsen PKB (UU HKPD)**  
Sistem shall menghitung Opsen PKB yang dipungut oleh pemerintah kabupaten/kota sebesar 66% dari PKB Pokok:

```
Opsen PKB = 66% × PKB Pokok
```

- **Input:** Nilai PKB Pokok dari REQ-TAX-004
- **Proses:** Kalkulasi 66% dari PKB Pokok
- **Output:** Nilai Opsen PKB yang terpisah dari PKB Pokok
- **Prakondisi:** PKB Pokok sudah terhitung
- **Pascakondisi:** Opsen PKB ditampilkan terpisah dalam breakdown harga
- **Acceptance Criteria:** Opsen dihitung terpisah dari PKB Pokok dan ditampilkan transparan

**3.1.3 Kebutuhan Perhitungan BBNKB dan Opsen BBNKB**

**REQ-TAX-006: Perhitungan BBNKB Pokok**  
Sistem shall menghitung Bea Balik Nama Kendaraan Bermotor (BBNKB) untuk penyerahan pertama dengan formula:

```
BBNKB Pokok = 12% × NJKB
```

- **Input:** NJKB kendaraan
- **Proses:** Kalkulasi 12% dari NJKB
- **Output:** Nilai BBNKB Pokok dalam IDR
- **Prakondisi:** NJKB valid
- **Pascakondisi:** BBNKB Pokok siap dikombinasikan dengan Opsen BBNKB
- **Acceptance Criteria:** BBNKB maksimal 12% dari NJKB

**REQ-TAX-007: Perhitungan Opsen BBNKB (UU HKPD)**  
Sistem shall menghitung Opsen BBNKB sebesar 66% dari BBNKB Pokok:

```
Opsen BBNKB = 66% × BBNKB Pokok
```

- **Input:** Nilai BBNKB Pokok dari REQ-TAX-006
- **Proses:** Kalkulasi 66% dari BBNKB Pokok
- **Output:** Nilai Opsen BBNKB yang terpisah dari BBNKB Pokok
- **Prakondisi:** BBNKB Pokok sudah terhitung
- **Pascakondisi:** Opsen BBNKB ditampilkan terpisah dalam breakdown harga
- **Acceptance Criteria:** Opsen dihitung terpisah dan transparan

**3.1.4 Kebutuhan Perhitungan Retribusi dan PNBP**

**REQ-TAX-008: Perhitungan SWDKLLJ**  
Sistem shall menghitung Sumbangan Wajib Dana Kecelakaan Lalu Lintas Jalan (SWDKLLJ) dengan tarif flat sesuai golongan kendaraan roda empat:

- **Input:** Golongan kendaraan (sedan, SUV, MPV, pickup, dll.)
- **Proses:** Mapping golongan ke tarif SWDKLLJ (Rp143.000 s.d. Rp150.000)
- **Output:** Nilai SWDKLLJ dalam IDR
- **Prakondisi:** Golongan kendaraan teridentifikasi
- **Pascakondisi:** SWDKLLJ diikutsertakan dalam total biaya
- **Acceptance Criteria:** SWDKLLJ sesuai tarif resmi PT Jasa Raharja

**REQ-TAX-009: Perhitungan Biaya PNBP STNK dan TNKB**  
Sistem shall menghitung biaya Penerbitan STNK Baru dan TNKB sesuai tarif PNBP resmi:

- **Input:** Jenis dokumen (STNK atau TNKB)
- **Proses:** Mapping jenis ke tarif PNBP (STNK: Rp200.000, TNKB: Rp100.000)
- **Output:** Nilai biaya PNBP dalam IDR
- **Prakondisi:** Jenis dokumen teridentifikasi
- **Pascakondisi:** Biaya PNBP diikutsertakan dalam total biaya pengurusan
- **Acceptance Criteria:** Sesuai tarif PNBP resmi Polri

**REQ-TAX-010: Perhitungan Biaya PNBP BPKB**  
Sistem shall menghitung biaya Penerbitan BPKB Baru sesuai tarif PNBP resmi:

- **Input:** Jenis layanan (BPKB Baru)
- **Proses:** Apply tarif PNBP BPKB Rp375.000
- **Output:** Nilai biaya BPKB dalam IDR
- **Prakondisi:** Transaksi melibatkan penerbitan BPKB baru
- **Pascakondisi:** Biaya BPKB diikutsertakan dalam total biaya pengurusan
- **Acceptance Criteria:** Sesuai tarif PNBP resmi Polri Rp375.000

**3.1.5 Kebutuhan Perhitungan Pajak Progresif**

**REQ-TAX-011: Deteksi Kepemilikan Kendaraan dengan API Samsat**  
Sistem shall melakukan query ke API Samsat/SIGNAL untuk mendeteksi jumlah kepemilikan kendaraan roda empat sejenis berdasarkan NIK pembeli dan alamat sesuai KTP/KK:

- **Input:** NIK pembeli (16 digit), alamat sesuai KTP
- **Proses:** Pengiriman request ke API Samsat untuk cek kepemilikan kendaraan
- **Output:** Jumlah kepemilikan kendaraan sejenis saat ini
- **Prakondisi:** API Samsat/SIGNAL tersedia dan NIK valid
- **Pascakondisi:** Sistem menentukan tarif progresif yang berlaku
- **Acceptance Criteria:** Query berhasil dan menghasilkan data kepemilikan akurat

**REQ-TAX-012: Penentuan Tarif Pajak Progresif**  
Sistem shall menentukan tarif progresif sesuai Perda DKI Jakarta Nomor 1 Tahun 2024:

| Urutan Kepemilikan | Tarif PKB |
|-------------------|-----------|
| Kendaraan ke-1 | 2,0% |
| Kendaraan ke-2 | 3,0% |
| Kendaraan ke-3 | 4,0% |
| Kendaraan ke-4 | 5,0% |
| Kendaraan ke-5+ | 6,0% (flat maksimal) |

- **Input:** Jumlah kepemilikan kendaraan sejenis dari REQ-TAX-011
- **Proses:** Mapping jumlah kepemilikan ke tarif progresif
- **Output:** Tarif progresif yang berlaku (2%-6%)
- **Prakondisi:** Data kepemilikan dari Samsat tersedia
- **Pascakondisi:** Tarif progresif diterapkan pada kalkulasi PKB
- **Acceptance Criteria:** Tarif sesuai tabel yang berlaku

**REQ-TAX-013: Perhitungan NJKB dari Data STNK**  
Sistem shall menghitung balik Nilai Jual Kendaraan Bermotor (NJKB) menggunakan data PKB Pokok dari STNK dengan formula:

```
NJKB = PKB Pokok / Tarif Berlaku
```

- **Input:** Nilai PKB Pokok dari data STNK, tarif yang berlaku
- **Proses:** Kalkulasi balik NJKB dari PKB
- **Output:** Nilai NJKB dalam IDR
- **Prakondisi:** PKB Pokok dan tarif tersedia dari data STNK
- **Pascakondisi:** NJKB dapat digunakan untuk kalkulasi pajak lainnya
- **Acceptance Criteria:** Hasil akurat dan dapat diverifikasi

**3.1.6 Kebutuhan Simulasi Kredit**

**REQ-TAX-014: Perhitungan DP Dinamis Berdasarkan NPF Leasing**  
Sistem shall menghitung uang muka (Down Payment) minimum dengan formula dinamis berdasarkan rasio NPF Net perusahaan pembiayaan:

| Rasio NPF Net | DP Minimum |
|---------------|------------|
| NPF ≤ 1% | 0% |
| 1% < NPF ≤ 3% | 10% |
| 3% < NPF ≤ 5% | 15% |
| NPF > 5% | 20% |

- **Input:** Rasio NPF Net leasing partner, harga kendaraan
- **Proses:** Determinasi DP berdasarkan tabel dan kalkulasi nominal
- **Output:** Nilai DP minimum dan persentase yang berlaku
- **Prakondisi:** Data NPF leasing partner tersedia di sistem
- **Pascakondisi:** DP ditampilkan dalam simulasi kredit
- **Acceptance Criteria:** DP sesuai tabel regulasi dan dapat dikonfigurasi

**REQ-TAX-015: Kalkulasi Angsuran Kredit**  
Sistem shall menghitung besaran angsuran bulanan dengan berbagai tenor dan suku bunga:

- **Input:** Harga kendaraan, DP, tenor (12-72 bulan), suku bunga tahunan
- **Proses:** Kalkulasi Angsuran = (Pokok Pinjaman + Total Bunga) / Tenor
- **Output:** Nilai angsuran bulanan dan total bunga
- **Prakondisi:** Harga, DP, tenor, dan bunga valid
- **Pascakondisi:** Simulasi kredit menampilkan breakdown lengkap
- **Acceptance Criteria:** Perhitungan akurat sesuai rumus anuitas

**3.1.7 Kebutuhan Pembuatan Rincian Harga On-The-Road**

**REQ-TAX-016: Generation Rincian Harga Lengkap**  
Sistem shall meng-generate rincian harga on-the-road yang transparan dengan breakdown terpisah untuk setiap komponen:

- **Input:** Data kendaraan, NJKB, data pembeli, leasing info
- **Proses:** Aggregasi seluruh komponen pajak dan biaya
- **Output:** Rincian harga lengkap dalam format terstruktur
- **Prakondisi:** Seluruh komponen pajak telah terhitung
- **Pascakondisi:** Rincian siap ditampilkan ke konsumen dan dicetak
- **Acceptance Criteria:** Breakdown lengkap, akurat, dan sesuai regulasi

#### 3.2 Kebutuhan Modul Verifikasi Dokumen

Modul verifikasi dokumen shall memvalidasi kelengkapan dan keaslian seluruh dokumen yang dipersyaratkan untuk transaksi kendaraan baru maupun bekas.

**3.2.1 Kebutuhan Validasi Input Standar**

**REQ-DOC-001: Validasi Format NIK**  
Sistem shall memvalidasi format Nomor Induk Kependudukan (NIK) dengan kriteria:

- Panjang exactly 16 digit
- Digit pertama tidak boleh 0
-通过了 checksum algorithm ( untuk NIK baru)
- Digit ke-6-7 merepresentasikan kode wilayah yang valid

- **Input:** String NIK dari form input
- **Proses:** Validasi format, panjang, dan digit validasi
- **Output:** Status validasi (true/false) dan pesan error jika tidak valid
- **Prakondisi:** NIK diinput oleh pengguna
- **Pascakondisi:** Sistem menerima atau menolak NIK
- **Acceptance Criteria:** NIK tidak valid langsung ditolak dengan pesan jelas

**REQ-DOC-002: Validasi Format NPWP**  
Sistem shall memvalidasi format Nomor Pokok Wajib Pajak (NPWP) dengan kriteria:

- Panjang 15 digit (untuk WNI) atau 20 digit (untuk WNA)
- Digit pertama-2: kode provinsi
- Digit ke-3: kode pajak
- Digit ke-4-8: nomor unik
- Digit ke-9-14: tanggal lahir (WNI)
- Digit ke-15: kode status

- **Input:** String NPWP dari form input
- **Proses:** Validasi format dan checksum NPWP
- **Output:** Status validasi dan pesan error
- **Prakondisi:** NPWP diinput oleh pengguna
- **Pascakondisi:** NPWP divalidasi dan disimpan
- **Acceptance Criteria:** NPWP tidak valid ditolak

**REQ-DOC-003: Validasi Kecocokan Data Identitas**  
Sistem shall memverifikasi kecocokan data antara dokumen yang diunggah:

- Kecocokan nama antara KTP dan KK
- Kecocokan NIK antara KTP dan KK
- Kecocokan alamat antara KTP dan KK
- Kecocokan NPWP dengan nama dan NIK (jika NPWP disediakan)

- **Input:** Data dari KTP, KK, dan NPWP yang diunggah
- **Proses:** Perbandingan string dengan toleranceCase-insensitive
- **Output:** Laporan kecocokan dengan highlight perbedaan
- **Prakondisi:** Minimal KTP dan KK tersedia
- **Pascakondisi:** Status verifikasi identitas ditampilkan
- **Acceptance Criteria:** Perbedaan apapun ditandai dan harus dikonfirmasi

**3.2.2 Kebutuhan Validasi Dokumen Kendaraan Baru**

**REQ-DOC-004: Verifikasi Faktur Kendaraan Asli**  
Sistem shall memverifikasi keaslian Faktur Kendaraan yang diterbitkan oleh diler/pabrikan:

- Validasi format nomor faktur sesuai standar pabrikan
- Verifikasi kecocokan data kendaraan (tipe, warna, nomor chassis)
- Konfirmasi tanggal faktur tidak melebihi tanggal transaksi
- Validasi nama penjual sesuai database diler terafiliasi

- **Input:** Upload scan/foto Faktur Kendaraan
- **Proses:** OCR dan validasi data dengan database
- **Output:** Status verifikasi dan data yang terekstrak
- **Prakondisi:** Faktur diunggah dalam format JPG/PDF
- **Pascakondisi:** Faktur divalidasi atau ditolak dengan alasan
- **Acceptance Criteria:** Faktur palsu atau tidak cocok langsung ditolak

**REQ-DOC-005: Validasi SRUT (Surat Tanda Registrasi Uji Tipe)**  
Sistem shall memvalidasi SRUT yang diterbitkan oleh Kementerian Perindustrian:

- Verifikasi nomor SRUT dengan format resmi
- Konfirmasi tipe kendaraan sesuai dengan yang dipesan
- Validasi masa berlaku SRUT
- Verifikasi keaslian via API Kementerian Perindustrian

- **Input:** Upload SRUT
- **Proses:** Validasi format dan integrasi API
- **Output:** Status validasi SRUT
- **Prakondisi:** SRUT diunggah
- **Pascakondisi:** SRUT valid atau ditolak
- **Acceptance Criteria:** SRUT kadaluarsa atau tidak valid ditolak

**REQ-DOC-006: Validasi Form A untuk Unit CBU Import**  
Sistem shall memvalidasi Form A yang diperlukan untuk kendaraan Completely Built Up (CBU) impor:

- Verifikasi Form A dari importir resmi
- Konfirmasi nomor HS Code sesuai jenis kendaraan
- Validasi dokumen kepabeanan (SPPB)
- Verifikasi keaslian via API bea cukai (jika tersedia)

- **Input:** Upload Form A dan dokumen pendukung
- **Proses:** Validasi kelengkapan dan keaslian
- **Output:** Status validasi Form A
- **Prakondisi:** Kendaraan adalah unit CBU import
- **Pascakondisi:** Form A valid atau transaksi tidak dapat proceed
- **Acceptance Criteria:** Unit CBU tanpa Form A valid tidak dapat diproses

**REQ-DOC-007: Validasi STCK (Surat Tanda Coba Kendaraan)**  
Sistem shall memvalidasi STCK untuk kendaraan baru sebelum STNK asli terbit:

- Verifikasi masa berlaku STCK (maksimal 1 bulan)
- Konfirmasi nomor registrasi sementara sesuai format resmi
- Validasi tujuan penggunaan (hanya untuk logistik diler-konsumen)
- Enforcement aturan: STCK dilarang untuk penggunaan pribadi

- **Input:** Upload STCK
- **Proses:** Validasi format, masa berlaku, dan enforcement aturan
- **Output:** Status validasi STCK
- **Prakondisi:** Kendaraan baru memerlukan STCK sementara
- **Pascakondisi:** STCK valid dan restriction dicatat
- **Acceptance Criteria:** STCK expired atau invalid ditolak

**3.2.3 Kebutuhan Validasi Dokumen Kendaraan Bekas**

**REQ-DOC-008: Verifikasi Hologram Tri Brata pada BPKB**  
Sistem shall memvalidasi keaslian hologram Tri Brata yang terdapat pada BPKB asli:

- Deteksi keberadaan hologram via image analysis
- Verifikasi warna hologram sesuai spesifikasi (merah, kuning, hijau)
- Validasi tekstur emboss pada hologram
- Flag untuk konfirmasi manual jika diperlukan

- **Input:** Upload foto halaman depan BPKB dengan resolusi minimal 2MP
- **Proses:** Image analysis untuk deteksi hologram
- **Output:** Status deteksi hologram (terdeteksi/tidak terdeteksi)
- **Prakondisi:** BPKB diunggah
- **Pascakondisi:** Hasil deteksi hologram ditandai untuk review
- **Acceptance Criteria:** Hologram tidak terdeteksi triggering alert

**REQ-DOC-009: Verifikasi Benang Pengaman UV pada Dokumen**  
Sistem shall memvalidasi benang pengaman ultraviolet (UV) yang terdapat pada dokumen resmi:

- Instruksi pengguna untuk menggunakan UV light
- Capture foto dokumen di bawah sinar UV
- Verifikasi keberadaan dan pola benang UV
- Validasi sesuai standar dokumen resmi

- **Input:** Foto dokumen di bawah sinar UV
- **Proses:** Image analysis untuk benang UV
- **Output:** Status verifikasi benang UV
- **Prakondisi:** UV light tersedia untuk verifikasi
- **Pascakondisi:** Hasil verifikasi benang UV disimpan
- **Acceptance Criteria:** Benang UV tidak sesuai triggering alert

**REQ-DOC-010: Konfirmasi STNK Aktif via API Samsat**  
Sistem shall melakukan konfirmasi keaktifan STNK melalui integrasi dengan API Samsat/SIGNAL:

- Query nomor registrasi kendaraan (NRKB)
- Verifikasi masa berlaku STNK
- Konfirmasi status tidak diblokir (ETLE aktif/non-aktif)
- Validasi data kendaraan sesuai dengan BPKB

- **Input:** NRKB (nomor polisi), 5 digit nomor rangka
- **Proses:** API call ke Samsat/SIGNAL
- **Output:** Status STNK, masa berlaku, status blokir
- **Prakondisi:** NRKB dan nomor rangka valid
- **Pascakondisi:** STNK aktif atau tidak aktif confirmed
- **Acceptance Criteria:** STNK expired atau terblokir ditolak

**REQ-DOC-011: Verifikasi Kwitansi Bermaterai**  
Sistem shall memvalidasi kelengkapan kwitansi untuk BBN-II (Balik Nama Kedua):

- Minimum 2-3 rangkap kwitansi kosong
- Kwitansi ditandatangani oleh pemilik sesuai BPKB
- Minimal 1 rangkap bermaterai untuk keperluan pajak
- Validasi keautentikan materai

- **Input:** Upload scan kwitansi (2-3 rangkap)
- **Proses:** Validasi kelengkapan dan keautentikan materai
- **Output:** Status kelengkapan kwitansi
- **Prakondisi:** Transaksi adalah BBN-II
- **Pascakondisi:** Kwitansi lengkap atau ditolak
- **Acceptance Criteria:** Kurang dari 2 rangkap atau tanpa materai ditolak

**REQ-DOC-012: Validasi SPH (Surat Pelepasan Hak)**  
Sistem shall memvalidasi Surat Pelepasan Hak untuk unit eks-aset perusahaan:

- Verifikasi identitas perusahaan (NPWP, akta pendirian)
- Konfirmasi pihak yang melepaskan hak
- Validasi data kendaraan sesuai dengan dokumen perusahaan
- Verifikasi tanda tangan authorized person

- **Input:** Upload SPH dan dokumen pendukung
- **Proses:** Validasi kelengkapan dan keaslian
- **Output:** Status validasi SPH
- **Prakondisi:** Kendaraan adalah eks-aset perusahaan
- **Pascakondisi:** SPH valid atau transaksi tidak dapat proceed
- **Acceptance Criteria:** Unit eks-perusahaan tanpa SPH ditolak

**REQ-DOC-013: Konfirmasi Histori Pelunasan Leasing**  
Sistem shall memvalidasi bukti pelunasan untuk kendaraan bekas yang sebelumnya dalam kredit leasing:

- Verifikasi surat pelunasan dari leasing lama
- Konfirmasi tidak ada hutang tersisa (clearance letter)
- Validasi histori pembayaran terakhir
- Verifikasi fotokopi BPKB terlegalisir

- **Input:** Upload surat pelunasan dan bukti bayar terakhir
- **Proses:** Validasi kelengkapan dokumen
- **Output:** Status clearance leasing
- **Prakondisi:** Kendaraan sebelumnya dalam kredit aktif
- **Pascakondisi:** Clearance confirmed atau blocking
- **Acceptance Criteria:** Dokumen tidak lengkap triggering blocking

**3.2.4 Kebutuhan Matriks Validasi Terpadu**

**REQ-DOC-014: Enforcement Matriks Validasi Berdasarkan Jenis Transaksi**  
Sistem shall menerapkan matriks validasi yang berbeda untuk setiap jenis transaksi sesuai tabel berikut:

| Jenis Transaksi | Dokumen Wajib | Aturan Validasi |
|-----------------|---------------|-----------------|
| **Mobil Baru** | Faktur Kendaraan, NIK, SRUT, STCK, Form A (CBU) | Semua dokumen wajib valid |
| **Mobil Bekas (Lunas)** | BPKB asli, STNK aktif, Faktur Pembelian, Kwitansi bermaterai, SPH (jika eks-perusahaan) | Semua dokumen wajib valid |
| **Mobil Bekas (Kredit Aktif)** | Kontrak leasing lama, fotokopi BPKB terlegalisir, histori setoran terakhir | Clearance dari leasing wajib |
| **Trade-In** | Dokumen kendaraan lama + dokumen pembeli baru | Double validation |

- **Input:** Jenis transaksi, data kendaraan
- **Proses:** Load matriks validasi dan apply rules
- **Output:** Checklist dokumen yang harus dipenuhi
- **Prakondisi:** Jenis transaksi teridentifikasi
- **Pascakondisi:** Transaksi hanya dapat proceed jika seluruh checklist terpenuhi
- **Acceptance Criteria:** Dokumen wajib yang tidak lengkap blocking transaksi

#### 3.3 Kebutuhan Modul Fintech dan Leasing

Modul fintech dan leasing shall mengintegrasikan proses pengajuan pembiayaan dengan leasing/multifinance partner menggunakan arsitektur asynchronous ESB.

**3.3.1 Kebutuhan Integrasi API SLIK OJK**

**REQ-FINT-001: Pengambilan Data iDeb via API SLIK**  
Sistem shall mengintegrasikan API SLIK OJK untuk pengambilan dokumen iDeb yang berisi riwayat kredit konsumen:

- Request data debitur berdasarkan NIK dan nama lengkap
- Handle response dan parse data iDeb
- Extract informasi kolektibilitas, total kewajiban, dan histori pembayaran
- Simpan data iDeb dalam format terstruktur

- **Input:** NIK, Nama Lengkap, Tanggal Lahir
- **Proses:** API call ke SLIK dengan autentikasi
- **Output:** Data iDeb dalam format JSON/struct
- **Prakondisi:** API SLIK tersedia dan user consent obtained
- **Pascakondisi:** Data iDeb siap dianalisis
- **Acceptance Criteria:** Data berhasil diambil dan parsed dengan benar

**REQ-FINT-002: Analisis Kelayakan Kredit Otomatis**  
Sistem shall menganalisis kelayakan pengajuan kredit berdasarkan data iDeb secara otomatis:

- Evaluasi skor kolektibilitas (1-5)
- Hitung rasio kewajiban terhadap pendapatan
- Validasi histori pembayaran (telat/tidak pernah telat)
- Generate preliminary approval/rejection

- **Input:** Data iDeb dari REQ-FINT-001, data aplikasi pengajuan
- **Proses:** Algoritma scoring berbasis rule
- **Output:** Hasil analisis kelayakan dan reason code
- **Prakondisi:** Data iDeb tersedia
- **Pascakondisi:** Analisis siap direview oleh surveyor
- **Acceptance Criteria:** Hasil analisis sesuai dengan parameter yang diset

**REQ-FINT-003: Integrasi Credit Score PEFINDO**  
Sistem shall melakukan supplement data dengan API PEFINDO untuk konsumen dengan keterbatasan data di SLIK:

- Request credit score PEFINDO
- Combine dengan data SLIK untuk scoring holistik
- Handle kasus consumer dengan thin file

- **Input:** NIK, Nama, Tanggal Lahir
- **Proses:** API call ke PEFINDO
- **Output:** Credit score PEFINDO
- **Prakondisi:** User consent obtained
- **Pascakondisi:** Credit score terintegrasi ke analisis
- **Acceptance Criteria:** Data berhasil diambil

**3.3.2 Kebutuhan Manajemen Pengajuan Kredit**

**REQ-FINT-004: Submission Aplikasi Kredit via ESB Async**  
Sistem shall mengirim data aplikasi kredit ke leasing partner menggunakan arsitektur ESB Asynchronous:

- Generate payload aplikasi kredit
- Kirim ke message queue (RabbitMQ/Apache Kafka)
- Receive acknowledgment dari leasing
- Track status submission

- **Input:** Data aplikasi kredit lengkap
- **Proses:** Serialisasi dan kirim ke message broker
- **Output:** Submission ID dan status queued
- **Prakondisi:** Data aplikasi lengkap dan valid
- **Pascakondisi:** Aplikasi masuk antrian leasing
- **Acceptance Criteria:** Message successfully published, acknowledgment received

**REQ-FINT-005: Tracking Status Persetujuan Real-Time**  
Sistem shall menyediakan mekanisme tracking status persetujuan kredit secara real-time:

- Polling periodic ke leasing via callback API
- Update status aplikasi berdasarkan response
- Generate notifikasi ke konsumen dan sales
- Log seluruh state changes

- **Input:** Submission ID dari leasing
- **Proses:** Polling atau webhook callback
- **Output:** Status terbaru aplikasi (Pending/Reviewing/Approved/Rejected)
- **Prakondisi:** Submission ID tersedia
- **Pascakondisi:** Status updated dan notifikasi terkirim
- **Acceptance Criteria:** Status updated maksimal 15 menit setelah perubahan

**3.3.3 Kebutuhan Manajemen Jaminan Fidusia**

**REQ-FINT-006: Generation Draft Akta Jaminan Fidusia**  
Sistem shall meng-generate draft akta jaminan fidusia berdasarkan data kendaraan dan pembiayaan:

- Template akta sesuai standar notaris
- Data variabel: pihak penjamin, objek fidusia, nilai penjaminan
- Generate dokumen dalam format yang dapat diedit
- Integration dengan sistem notaris mitra

- **Input:** Data kendaraan, data pembiayaan, data dealer, data leasing
- **Proses:** Merge data ke template akta
- **Output:** Draft akta fidusia dalam format DOCX/PDF
- **Prakondisi:** Data pembiayaan approved
- **Pascakondisi:** Draft akta siap untuk review notaris
- **Acceptance Criteria:** Draft sesuai template dan data akurat

**REQ-FINT-007: Pendaftaran Fidusia Elektronik ke Ditjen AHU**  
Sistem shall melakukan pendaftaran jaminan fidusia secara elektronik ke Ditjen AHU Kemenkumham:

- Submit aplikasi fidusia via API Ditjen AHU
- Upload dokumen pendukung (akta notaris, dll.)
- Generate kode billing PNBP
- Handle response dan simpan nomor sertifikat

- **Input:** Data fidusia lengkap, dokumen pendukung
- **Proses:** API call ke Ditjen AHU
- **Output:** Nomor pendaftaran dan kode billing
- **Prakondisi:** Akta fidusia sudah ditandatangani notaris
- **Pascakondisi:** Pendaftaran submitted dan kode billing generated
- **Acceptance Criteria:** Submission berhasil dengan response valid

**REQ-FINT-008: Monitoring Batas Waktu Pendaftaran Fidusia**  
Sistem shall secara ketat memonitor dan enforce batas waktu pendaftaran fidusia maksimal 30 hari kalender sesuai PP Nomor 21 Tahun 2015:

- Tracking tanggal pembuatan akta notaris
- Countdown 30 hari kalender
- Generate warning di hari ke-25, 28, dan 29
- Auto-blocking submission jika deadline tercapai

- **Input:** Tanggal pembuatan akta notaris
- **Proses:** Kalkulasi hari kalender dan comparison
- **Output:** Status countdown dan warning flags
- **Prakondisi:** Akta fidusia sudah dibuat
- **Pascakondisi:** Sistem blocking atau warning sesuai timeline
- **Acceptance Criteria:** Submission setelah 30 hari secara teknis tidak mungkin

**REQ-FINT-009: Pengunduhan Sertifikat Fidusia Elektronik**  
Sistem shall menangani pengunduhan sertifikat fidusia elektronik dari Ditjen AHU setelah approval:

- Polling status pendaftaran
- Download sertifikat dalam format resmi
- Simpan ke document vault dengan metadata
- Generate notifikasi ke leasing dan dealer

- **Input:** Nomor pendaftaran fidusia
- **Proses:** API call dan download
- **Output:** File sertifikat fidusia
- **Prakondisi:** Pendaftaran approved oleh AHU
- **Pascakondisi:** Sertifikat tersimpan dan accessible
- **Acceptance Criteria:** Sertifikat berhasil didownload dan terverifikasi

**3.3.4 Kebutuhan Pembayaran PNBP Fidusia**

**REQ-FINT-010: Generate dan Manage Kode Billing PNBP**  
Sistem shall mengelola proses pembayaran PNBP untuk pendaftaran fidusia:

- Generate kode billing sesuai format resmi
- Integration dengan payment gateway
- Update status pembayaran
- Handle timeout dan retry

- **Input:** Data fidusia
- **Proses:** Request kode billing dari AHU
- **Output:** Kode billing dan jumlah PNBP
- **Prakondisi:** Pendaftaran fidusia initiated
- **Pascakondisi:** Kode billing tersedia untuk pembayaran
- **Acceptance Criteria:** Kode billing valid dan dapat dibayar

#### 3.4 Kebutuhan Modul Logistik dan PDI

Modul logistik dan PDI shall mengelola alur distribusi kendaraan dan quality control sebelum serah terima ke konsumen.

**3.4.1 Kebutuhan Manajemen Pengiriman**

**REQ-LOG-001: Tracking Unit dari Pabrik ke Gudang Diler**  
Sistem shall menyediakan mekanisme tracking shipment kendaraan dari pabrik ke gudang diler:

- Generate shipment ID untuk setiap unit
- Update status di setiap checkpoint
- Display tracking progress ke dealer
- Integration dengan sistem logistik pabrikan

- **Input:** Data unit kendaraan, tujuan pengiriman
- **Proses:** Create shipment record dan tracking
- **Output:** Shipment ID dan tracking status
- **Prakondisi:** Unit ready untuk dikirim
- **Pascakondisi:** Tracking aktif dan visible
- **Acceptance Criteria:** Tracking update setiap checkpoint

**REQ-LOG-002: Generation STCK (Surat Tanda Coba Kendaraan)**  
Sistem shall meng-generate STCK untuk kendaraan baru sebelum STNK asli terbit:

- Generate nomor STCK dengan format resmi
- Set masa berlaku exactly 1 bulan dari tanggal issuance
- Integration dengan sistem Samsat untuk registrasi
- Generate plat sementara (merah-putih) virtual

- **Input:** Data kendaraan, tanggal issuance
- **Proses:** Generate STCK dan register ke Samsat
- **Output:** STCK document dan plat sementara
- **Prakondisi:** Unit kendaraan baru siap kirim
- **Pascakondisi:** STCK generated dan valid untuk 1 bulan
- **Acceptance Criteria:** Masa berlaku akurat 1 bulan

**REQ-LOG-003: Enforcement Aturan Penggunaan STCK**  
Sistem shall secara ketat menerapkan aturan bahwa STCK dilarang untuk:

- Penggunaan kendaraan di jalan umum untuk keperluan pribadi
- Perjalanan keluar kota
- Penggunaan di luar konteks logistik diler-konsumen

Sistem shall menyediakan:

- Digital acknowledgment sebelum STCK di-issue
- Log penggunaan unit dengan GPS tracking (jika memungkinkan)
- Warning system jika terdeteksi pelanggaran
- Enforcement: blocking pengiriman jika aturan dilanggar

- **Input:** Data STCK, data shipment
- **Proses:** Apply business rules dan monitoring
- **Output:** Enforcement status dan warning flags
- **Prakondisi:** STCK issued
- **Pascakondisi:** Pelanggaran terdeteksi dan ditolak
- **Acceptance Criteria:** Tidak ada transaksi yang mem-bypass aturan STCK

**3.4.2 Kebutuhan Pre-Delivery Inspection (PDI)**

**REQ-LOG-004: Checklist PDI Wajib - De-waxing Bodi**  
Sistem shall memvalidasi bahwa teknisi telah melakukan pembilasan wax pelindung pada bodi kendaraan:

- Checklist item wajib dalam form PDI
- Validasi photo/video dokumentasi
- Timestamp dan geolocation
- Digital signature teknisi

- **Input:** Photo/video dokumentasi de-waxing
- **Proses:** Validasi checklist completion
- **Output:** Status checklist dan dokumentasi
- **Prakondisi:** Unit kendaraan tiba di diler
- **Pascakondisi:** Checklist completed dan logged
- **Acceptance Criteria:** Item wajib terisi dengan dokumentasi valid

**REQ-LOG-005: Checklist PDI Wajib - Pemasangan Backup Fuse**  
Sistem shall memvalidasi bahwa teknisi telah melakukan pemasangan backup fuse kelistrikan utama:

- Checklist item wajib
- Photo dokumentasi fuse yang terpasang
- Verifikasi jenis dan rating fuse
- Digital signature teknisi

- **Input:** Photo dokumentasi fuse
- **Proses:** Validasi checklist dan photo
- **Output:** Status checklist
- **Prakondisi:** PDI initiated
- **Pascakondisi:** Checklist completed
- **Acceptance Criteria:** Fuse terpasang sesuai spesifikasi

**REQ-LOG-006: Checklist PDI Wajib - Scan OBD-II dan Penghapusan DTC**  
Sistem shall memvalidasi bahwa teknisi telah melakukan scan OBD-II untuk mendeteksi dan menghapus Diagnostic Trouble Code (DTC) error pada ECU:

- Connection ke port OBD-II kendaraan
- Scan seluruh DTC yang tersimpan
- Delete DTC setelah perbaikan jika diperlukan
- Generate laporan scan
- Validation: Zero DTC aktif sebelum shipment

- **Input:** Connection ke OBD-II port
- **Proses:** Scan, identify, delete DTC
- **Output:** Laporan scan dengan status DTC
- **Prakondisi:** Kendaraan dapat diakses via OBD-II
- **Pascakondisi:** DTC cleared dan laporan generated
- **Acceptance Criteria:** Zero active DTC sebelum approval

**REQ-LOG-007: Checklist PDI Wajib - Pemeriksaan Cairan**  
Sistem shall memvalidasi pemeriksaan seluruh cairan kendaraan:

- Cairan mesin (oli) - level dan kondisi
- Cairan pendingin (coolant) - level dan颜色的
- Cairan rem (brake fluid) - level
- Cairan windshield washer
- Cairan transmisi (jika applicable)
- Dokumentasi photo setiap fluida

- **Input:** Photo dokumentasi setiap fluida
- **Proses:** Validasi checklist dan photo
- **Output:** Status setiap fluida
- **Prakondisi:** PDI initiated
- **Pascakondisi:** Seluruh fluida terverifikasi
- **Acceptance Criteria:** Setiap fluida dalam kondisi normal

**REQ-LOG-008: Checklist PDI Wajib - Uji Jalan Minimal 10 KM**  
Sistem shall memvalidasi bahwa teknisi telah melakukan uji jalan dengan jarak minimal 10 kilometer:

- GPS tracking selama uji jalan
- Record jarak tempuh
- Record durasi
- Catatan kondisi selama jalan (performance, suara, getaran)
- Digital signature teknisi

- **Input:** GPS track dan log uji jalan
- **Proses:** Calculate distance dan validate minimum
- **Output:** Report uji jalan dengan jarak tempuh
- **Prakondisi:** Unit siap untuk uji jalan
- **Pascakondisi:** Uji jalan completed minimal 10km
- **Acceptance Criteria:** Distance >= 10km dan no critical issues

**REQ-LOG-009: Generation Laporan PDI Digital**  
Sistem shall meng-generate laporan PDI digital yang komprehensif:

- Consolidate seluruh checklist items
- Photo dokumentasi
- Laporan scan OBD-II
- Report uji jalan
- Digital signature teknisi dan supervisor
- Timestamp dan geolocation

- **Input:** Seluruh data checklist dari REQ-LOG-004 hingga REQ-LOG-008
- **Proses:** Generate consolidated report
- **Output:** Laporan PDI lengkap dalam PDF
- **Prakondisi:** Seluruh checklist completed
- **Pascakondisi:** Laporan ready untuk approval
- **Acceptance Criteria:** Laporan lengkap dan akurat

**3.4.3 Kebutuhan Serah Terima Unit**

**REQ-LOG-010: Approval Shipment Setelah PDI Lulus**  
Sistem shall mem-block shipment unit ke konsumen jika PDI belum passed:

- Validation: seluruh checklist mandatory passed
- Approval workflow dengan multiple sign-off
- Digital signature supervisor PDI
- Update status shipment

- **Input:** Laporan PDI dari REQ-LOG-009
- **Proses:** Validation dan approval workflow
- **Output:** Approval/rejection status
- **Prakondisi:** Laporan PDI available
- **Pascakondisi:** Unit approved atau rejected untuk shipment
- **Acceptance Criteria:** Unit dengan PDI failed tidak dapat di-ship

**REQ-LOG-011: Generation Faktur dan Bukti Serah Terima**  
Sistem shall meng-generate faktur penjualan dan bukti serah terima unit:

- Faktur dengan breakdown harga lengkap
- Bukti serah terima dengan detail unit
- Checklist serah terima (unit, kunci, dokumen, aksesoris)
- Digital signature konsumen

- **Input:** Data transaksi, data unit, data konsumen
- **Proses:** Generate dokumen serah terima
- **Output:** Faktur dan bukti serah terima
- **Prakondisi:** Pembayaran lunas atau financing approved
- **Pascakondisi:** Dokumen generated dan signed
- **Acceptance Criteria:** Dokumen akurat dan legally binding

**REQ-LOG-012: Aktivasi Garansi Komponen Utama**  
Sistem shall mengaktifkan garansi komponen utama secara otomatis setelah unit diserahterimakan:

- Trigger: status serah terima = completed
- Activation date = tanggal serah terima
- Coverage: transmisi, mesin (1 tahun standar)
- Generate warranty card
- Notification ke konsumen dengan detail warranty

- **Input:** Confirmation serah terima
- **Proses:** Activation dan generation warranty
- **Output:** Warranty card dan confirmation
- **Prakondisi:** Serah terima completed
- **Pascakondisi:** Garansi aktif dan customer notified
- **Acceptance Criteria:** Garansi aktif dengan tanggal yang benar

#### 3.5 Kebutuhan Integrasi API Eksternal

**3.5.1 Kebutuhan API Dukcapil**

**REQ-INT-001: Verifikasi NIK dan Face Matching**  
Sistem shall mengintegrasikan API Dukcapil untuk verifikasi identitas pembeli:

- Request verifikasi NIK dengan data KTP
- Request face matching dengan swafoto
- Receive matching score dan status
- Store verification result

- **Input:** NIK, foto KTP, swafoto
- **Proses:** API call dan face matching
- **Output:** Match score dan verification status
- **Prakondisi:** User consent obtained
- **Pascakondisi:** Identitas terverifikasi atau flagged
- **Acceptance Criteria:** Face matching score >= 85% untuk approval

**3.5.2 Kebutuhan API Samsat/SIGNAL**

**REQ-INT-002: Verifikasi Keaslian STNK**  
Sistem shall mengintegrasikan API Samsat/SIGNAL untuk verifikasi STNK kendaraan:

- Request verifikasi dengan NRKB dan nomor rangka
- Receive status STNK dan masa berlaku
- Check untuk blokir ETLE
- Return verification result

- **Input:** NRKB, 5 digit nomor rangka
- **Proses:** API call ke Samsat
- **Output:** Status STNK dan ETLE
- **Prakondisi:** NRKB valid
- **Pascakondisi:** STNK terverifikasi atau flagged
- **Acceptance Criteria:** STNK expired atau terblokir triggering rejection

**REQ-INT-003: Query Pajak Progresif**  
Sistem shall query API Samsat untuk mendapatkan data pajak progresif berdasarkan NIK:

- Request histori kepemilikan kendaraan
- Calculate progresive tax berdasarkan data
- Return jumlah kendaraan sejenis

- **Input:** NIK pembeli
- **Proses:** API call dan kalkulasi
- **Output:** Jumlah kepemilikan dan tarif progresif
- **Prakondisi:** NIK valid
- **Pascakondisi:** Tarif progresif determined
- **Acceptance Criteria:** Data akurat dari Samsat

**3.5.3 Kebutuhan API Digital Signature**

**REQ-INT-004: Tanda Tangan Digital pada SPK**  
Sistem shall mengintegrasikan API TTE (Privy/VIDA) untuk penandatanganan Surat Pemesanan Kendaraan:

- Generate dokumen SPK dalam PDF
- Request tanda tangan digital via OTP
- Receive signed document
- Store certificate dan timestamp

- **Input:** Draft SPK, nomor HP konsumen
- **Proses:** Generate PDF, request TTE, receive signed
- **Output:** Signed SPK dengan certificate
- **Prakondisi:** SPK content finalized
- **Pascakondisi:** SPK legally binding
- **Acceptance Criteria:** Signature valid dan terverifikasi

**REQ-INT-005: Tanda Tangan Digital pada Kontrak Kredit**  
Sistem shall mengintegrasikan API TTE untuk penandatanganan kontrak pembiayaan kredit:

- Generate kontrak dalam format PDF
- Multi-party signing (konsumen, dealer, leasing)
- Verification setiap signature
- Store complete signed document

- **Input:** Draft kontrak, data signers
- **Proses:** Multi-party signing process
- **Output:** Fully signed contract
- **Prakondisi:** Kontrak content finalized
- **Pascakondisi:** Kontrak legally binding
- **Acceptance Criteria:** All signatures valid

---

### BAB 4: KEBUTUHAN NON-FUNGSIONAL & KEAMANAN

bab ini menjabarkan seluruh kebutuhan non-fungsional (Quality of Service / QoS) yang wajib dipenuhi oleh Sistem Transaksi Pembelian Kendaraan Bermotor Terintegrasi. setiap kebutuhan dikuantifikasi dengan tolok ukur terukur agar dapat diverifikasi oleh tim QA. seluruh kebutuhan menggunakan modal verb **shall** yang menandakan kebutuhan wajib (mandatory) sesuai RFC 2119 / IEEE 29148.

---

## 4.1 Kebutuhan Performa (Performance Requirements)

### REQ-NF-001: Batas Waktu Respons Sistem Internal

**Description:** Sistem shall memproses seluruh request internal (kecuali operasi bulk) dengan batas waktu respons yang telah dikuantifikasi untuk memastikan pengalaman pengguna yang responsif.

**Spesifikasi Kuantitatif:**

| Metrik | Target | Kondisi |
|--------|--------|---------|
| p50 Response Time | ≤ 2 detik | Untuk seluruh endpoint API non-bulk |
| p99 Response Time | ≤ 3 detik | Untuk seluruh endpoint API non-bulk |
| p99.9 Response Time | ≤ 5 detik | Batas absolut sebelum timeout |

**Kebutuhan Teknis:**

- Sistem shall mengimplementasikan caching layer (Redis/Memcached) untuk query berulang ke database
- Sistem shall menggunakan lazy loading untuk komponen UI yang tidak kritis
- Database query shall dioptimasi dengan indexed columns pada field yang sering di-filter (NIK, NRKB, nomor_rangka)

**Input:** Request API dari client (web/mobile app)  
**Proses:** Routing → Authentication → Business Logic → Database Query → Response Serialization  
**Output:** JSON response dengan header `X-Response-Time` yang，记录 latency aktual  
**Prakondisi:** Server dalam kondisi healthy (CPU < 70%, Memory < 80%)  
**Pascakondisi:** Response terkirim ke client dalam batas waktu yang ditentukan  
**Acceptance Criteria:** Monitor Grafana/Prometheus menunjukkan p50 ≤ 2s dan p99 ≤ 3s selama peak hours

---

### REQ-NF-002: Performa Pemrosesan Transaksi Leasing via ESB Asynchronous

**Description:** Sistem shall memastikan seluruh transaksi pengajuan pembiayaan melalui ESB Asynchronous memiliki waktu respons rata-rata 4,22 detik per request. Constraint ini mutlak karena berdasarkan data benchmark industri Indonesia, sistem synchronous memiliki error rate 49,83% dengan latensi 31,49 detik.

**Spesifikasi Kuantitatif:**

| Metrik | Target | Threshold Alert |
|--------|--------|-----------------|
| Average Response Time | ≤ 4,22 detik | > 5 detik |
| Max Queue Depth | ≤ 1.000 message | > 2.000 message |
| Retry Success Rate | ≥ 95% | < 90% |
| Dead Letter Queue Rate | ≤ 0,1% | > 0,5% |

**Kebutuhan Teknis:**

- Integrasi leasing shall menggunakan arsitektur message queue (RabbitMQ/Apache Kafka) dengan acknowledgment pattern
- Sistem shall mengimplementasikan circuit breaker pattern untuk mencegah cascade failure
- Setiap message shall memiliki correlation ID untuk tracing end-to-end

**Input:** Payload pengajuan kredit (NIK, data kendaraan, tenor, DP)  
**Proses:** Serialize → Publish to Queue → Leasing Consumer → Process → Response → ACK  
**Output:** Response keputusan kredit dari leasing partner  
**Prakondisi:** Koneksi ESB healthy, leasing partner online  
**Pascakondisi:** Transaksi tercatat di audit log dengan status approved/rejected  
**Acceptance Criteria:** Load test 500 concurrent submissions menunjukkan average response ≤ 4,22 detik dengan 0% failed requests

---

### REQ-NF-003: Kapasitas Beban Konkuren (Concurrency Capacity)

**Description:** Sistem shall menangani beban konkuren pada saat peak sales hours tanpa degradasi layanan yang signifikan.

**Spesifikasi Kuantitatif:**

| Metrik | Target | Scaling Trigger |
|--------|--------|-----------------|
| Peak Concurrent Users | 10.000 pengguna aktif | Auto-scale out |
| Peak TPS (Transactions Per Second) | 500 transaksi/detik | Auto-scale out |
| Peak API Calls | 50.000 calls/menit | Rate limiting aktif |
| Session Memory | ≤ 512 MB per user session | Session timeout 30 menit |

**Kebutuhan Teknis:**

- Arsitektur microservices dengan horizontal scaling
- Load balancer (Nginx/AWS ALB) dengan round-robin atau least-connections algorithm
- Database connection pooling (min 10, max 100 connections per service)
- CDN untuk static assets dengan cache TTL 24 jam

**Input:** Request dari 10.000+ concurrent users  
**Proses:** Load Balancer → Auto-scaling Group → Service Instances → Database  
**Output:** Response yang konsisten untuk seluruh pengguna  
**Prakondisi:** Load balancer aktif, minimal 2 instance per service running  
**Pascakondisi:** Zero user-facing errors, p99 latency tetap ≤ 3 detik  
**Acceptance Criteria:** Load test dengan JMeter/Artillery menunjukkan 500 TPS sustained selama 30 menit dengan < 1% error rate

---

## 4.2 Kebutuhan Keamanan & Perlindungan Data (Safety & Security)

### REQ-NF-004: Enkripsi Data Sensitif

**Description:** Sistem shall mengenkripsi seluruh data pribadi sensitif (PII) menggunakan standar kriptografi yang kuat, baik saat penyimpanan maupun saat transmisi.

**Spesifikasi Enkripsi:**

| Layer | Standar | Aplikasi |
|-------|---------|----------|
| Encryption at Rest | AES-256-GCM | Database (MySQL/PostgreSQL TDE), File Storage, Backup |
| Encryption in Transit | TLS 1.3 | API calls, WebSocket, File upload/download |
| Key Management | AWS KMS / HashiCorp Vault | Rotasi kunci setiap 90 hari |

**Data yang Wajib Dienkripsi:**

- Data identitas: NIK, NPWP, nomor KK, foto KTP, swafoto
- Data finansial: iDeb report, skor kredit, nomor rekening
- Data kendaraan: nomor rangka, nomor mesin, nomor BPKB
- Data kontrak: dokumen SPK signed, kontrak kredit, sertifikat fidusia

**Input:** Data plaintext PII  
**Proses:** Identify PII → Classify sensitivity → Apply encryption → Store ciphertext  
**Output:** Data tersimpan dengan format terenkripsi, accessible via decryption key  
**Prakondisi:** Encryption key available di KMS, schema database mendukung varbinary/encrypted columns  
**Pascakondisi:** Data tidak dapat dibaca tanpa key yang valid  
**Acceptance Criteria:** Penetration test oleh pihak ketiga menunjukkan 0 vulnerabilitas terkait enkripsi; audit log mencatat semua akses ke data terenkripsi

---

### REQ-NF-005: Autentikasi & Otorisasi Multi-Faktor (RBAC + MFA)

**Description:** Sistem shall mengimplementasikan Role-Based Access Control (RBAC) dengan Multi-Faktor Authentication (MFA) wajib untuk peran dengan akses privilegi tinggi.

**Matriks Roles & Permissions:**

| Role | Login | MFA Required | Data Access | Actions |
|------|-------|--------------|--------------|---------|
| Pembeli | ✅ | ❌ | Own data only | Browse, simulate, sign |
| Sales Dealer | ✅ | ❌ | Own leads | Create SPK, update status |
| Admin Dealer | ✅ | ✅ | All dealer data | Verify docs, submit Samsat |
| Surveyor Leasing | ✅ | ✅ | Assigned cases | Credit analysis, approve/reject |
| Teknisi PDI | ✅ | ❌ | Own assigned units | Update PDI checklist |
| System Admin | ✅ | ✅ | Full system | Config, user management |

**Spesifikasi MFA:**

| Metrik | Spesifikasi |
|--------|------------|
| MFA Method | OTP via WhatsApp Business API atau Email |
| OTP Validity | 5 menit |
| OTP Length | 6 digit numerik |
| Failed Attempts | Max 3 kali, lockout 15 menit |
| Fallback | Backup codes (8 kode sekali pakai) |

**Input:** User credentials + MFA token  
**Proses:** Validate credential → Check MFA requirement → Verify OTP → Issue JWT token  
**Output:** JWT access token (15 menit) + refresh token (7 hari)  
**Prakondisi:** User registered, WhatsApp/Email verified  
**Pascakondisi:** Invalid OTP ≥ 3x menyebabkan account lockout dan notifikasi email ke user  
**Acceptance Criteria:** Akses tanpa MFA ke endpoint admin mengembalikan HTTP 401; brute force attack terblokir setelah 3 failed OTP attempts

---

### REQ-NF-006: Integrasi Tanda Tangan Elektronik Tersertifikasi Kominfo

**Description:** Setiap Tanda Tangan Elektronik (TTE) pada dokumen SPK dan kontrak kredit shall diintegrasikan dengan penyelenggara TTE bersertifikasi Kominfo untuk menjamin keabsahan hukum sesuai UU ITE Pasal 11.

**Spesifikasi TTE:**

| Aspek | Spesifikasi |
|-------|--------------|
| Penyedia TTE | PrivyID atau VIDA (sertifikasi Kominfo) |
| Tipe TTE | Tanda Tangan Elektronika Tersertifikasi (bukan signature image) |
| Algoritma Signing | RSASSA-PSS dengan SHA-256 atau ECDSA dengan P-256 |
| Certificate Validity | Min 1 tahun, auto-renewal |
| Long-term Validation | Include timestamp dan CRL/OCSP validation |

**Dokumen yang Wajib Ditandatangani Secara Elektronik:**

1. Surat Pemesanan Kendaraan (SPK) - oleh Pembeli dan Sales
2. Kontrak Pembiayaan Kredit - oleh Pembeli, Dealer, dan Leasing
3. Berita Acara Serah Terima - oleh Pembeli dan Teknisi PDI
4. Dokumen Fidusia - oleh Pemilik dan Notaris

**Input:** PDF dokumen, data signers (nama, email, nomor HP)  
**Proses:** Generate hash → Send to TTE provider → User approves via OTP → Sign → Attach certificate  
**Output:** Signed PDF dengan embedded digital certificate dan timestamp  
**Prakondisi:** User memiliki akun TTE provider, dokumen final (no further edits allowed)  
**Pascakondisi:** Signed document immutable, certificate traceable ke root CA Kominfo  
**Acceptance Criteria:** Signed document passes LTV (Long Term Validation) test; TTE certificate verifiable di portal Kominfo

---

### REQ-NF-007: Pencegahan Fraud Identitas (Liveness Detection & Face Matching)

**Description:** Sistem shall mengimplementasikan liveness detection dan face matching dengan skor minimal 85% saat registrasi pembeli baru melalui integrasi API Dukcapil untuk memitigasi identity fraud.

**Spesifikasi Liveness Detection:**

| Aspek | Spesifikasi |
|-------|--------------|
| Liveness Method | Passive (browser-based challenge-response) |
| Anti-spoofing | Wajib deteksi foto printed, video replay, deepfake |
| Challenge Types | Random head movement, blink detection, expression change |
| Min Liveness Score | 0.85 (85% confidence) |

**Spesifikasi Face Matching:**

| Aspek | Spesifikasi |
|-------|--------------|
| Min Match Score | 85% similarity |
| Reference Image | e-KTP photo dari API Dukcapil |
| Input Image | Live selfie dengan constraints (lighting, pose, glare) |
| Comparison Method | 1:1 verification (bukan 1:N identification) |

**Input:** Live selfie capture dari mobile web/app  
**Proses:** Liveness check → Extract face vector → Compare dengan e-KTP photo → Return match score  
**Output:** Match score (0-100%), liveness status (pass/fail)  
**Prakondisi:** API Dukcapil accessible, user consented to biometric processing  
**Pascakondisi:** Match score < 85% menyebabkan registration blocked dengan eskalasi ke manual review  
**Acceptance Criteria:** Test dengan Spoofing Attack Dataset menunjukkan > 99% detection rate; false acceptance rate < 0.1%

---

## 4.3 Kebutuhan Keandalan & Ketersediaan (Reliability & Availability)

### REQ-NF-008: Target Ketersediaan Sistem

**Description:** Sistem shall memiliki target ketersediaan minimal 99,9% uptime per tahun, yang berarti downtime maksimal 8,76 jam per tahun atau ~43 menit per bulan.

**SLA Matrix:**

| Tier | Uptime Target | Downtime/year | Downtime/month | Use Case |
|------|---------------|---------------|-----------------|----------|
| Tier 1 (Critical) | 99,99% | 52 menit | 4,3 menit | Payment, TTE, Auth |
| Tier 2 (Important) | 99,9% | 8,76 jam | 43 menit | Vehicle lookup, pricing |
| Tier 3 (Background) | 99,5% | 1,83 hari | 3,65 jam | Batch processing, reports |

**Strategi High Availability:**

- Deployment: Multi-AZ (minimal 2 Availability Zone)
- Database: Primary-replica dengan automatic failover
- Stateless services dengan session externalization (Redis)
- Health check endpoint setiap 30 detik dengan 3 consecutive failures = failover

**Input:** Continuous operations requirement 24/7/365  
**Proses:** Monitoring → Failover detection → Traffic rerouting → Recovery  
**Output:** Service tetap accessible selama failover  
**Prakondisi:** Load balancer, multi-AZ deployment configured  
**Pascakondisi:** User experience unchanged during failover (< 30 detik interruption)  
**Acceptance Criteria:** Monthly SLA report menunjukkan uptime ≥ 99,9% untuk Tier 1 dan Tier 2 services

---

### REQ-NF-009: Handling API Downtime (Graceful Degradation)

**Description:** Sistem shall menangani skenario downtime API eksternal (Samsat/SIGNAL, Dukcapil, Leasing) secara graceful tanpa crash, menggunakan pola Circuit Breaker dan Queue-based Retry dengan exponential backoff.

**Strategi Graceful Degradation:**

| API | Fallback Strategy | User Message |
|-----|-------------------|--------------|
| API Samsat/SIGNAL | Queue request, process when online | "Sistem Samsat sedang sibuk. Dokumen akan diproses otomatis saat koneksi pulih." |
| API Dukcapil | Allow registration with pending verification | "Verifikasi identitas dalam antrean. Konfirmasi via WhatsApp dalam 15 menit." |
| API Leasing | Show last cached result, disable new submissions | "Layanan verifikasi kredit sementara tidak tersedia. Gunakan kalkulator simulasi." |
| API TTE | Block signing, preserve draft | "Layanan tanda tangan digital sedang maintenance. Draft tersimpan aman." |

**Spesifikasi Retry Mechanism:**

| Parameter | Value |
|-----------|-------|
| Max Retry Attempts | 3 kali |
| Initial Backoff | 1 detik |
| Max Backoff | 60 detik |
| Backoff Strategy | Exponential (1s → 2s → 4s → 8s → 16s → 32s → 60s) |
| Circuit Breaker Threshold | 5 failures dalam 10 detik = OPEN state |

**Input:** API call gagal dengan error timeout/5xx  
**Proses:** Check circuit state → If CLOSED: retry with backoff → If OPEN: return fallback → If HALF-OPEN: probe health  
**Output:** Fallback response dengan estimated recovery time atau queued confirmation  
**Prakondisi:** Circuit breaker configured, fallback data available (cache/queue)  
**Pascakondisi:** Request queued atau returned dengan user-friendly message  
**Acceptance Criteria:** Simulasi downtime API menunjukkan 0% user-facing crash; queue processing resumes automatically upon API recovery

---

## 4.4 Kebutuhan Keterserapan & Antarmuka (Usability & Human Factors)

### REQ-NF-010: Kompatibilitas Mobile untuk Aplikasi Inspeksi

**Description:** Aplikasi inspeksi kendaraan shall berjalan lancar di perangkat mobile yang digunakan inspektor di lapangan, dengan navigasi ramah satu tangan dan dukungan offline terbatas.

**Spesifikasi Kompatibilitas Mobile:**

| Aspek | Minimum | Direkomendasikan |
|-------|---------|------------------|
| Android Version | Android 10 (API 29) | Android 13+ |
| iOS Version | iOS 14 | iOS 17+ |
| Screen Size | 5,5 inci | 6,1 - 6,7 inci |
| RAM | 3 GB | 6 GB+ |
| Storage Free | 200 MB | 500 MB+ |
| Camera Resolution | 12 MP | 48 MP+ |
| Network | 4G LTE | 5G |

**Persyaratan UX Mobile:**

- One-handed operation: seluruh fungsi kritis reachable dengan jempol (area bawah layar)
- Touch target minimum: 44x44 points (Apple HIG) atau 48x48 dp (Material Design)
- Offline capability: checklist PDI dapat diisi offline, sync saat online
- Battery efficient: no background location/GPS tracking yang tidak perlu

**Input:** Checklist inspeksi kendaraan (150-188 titik)  
**Proses:** Display question → Inspector tap/gps/photo → Validate input → Store locally  
**Output:** Completed inspection report dengan GPS coordinates dan timestamp  
**Prakondisi:** App installed, user logged in, GPS permission granted  
**Pascakondisi:** Data tersimpan lokal sampai sync berhasil ke server  
**Acceptance Criteria:** Usability test dengan 10 inspector menunjukkan SUS score ≥ 70; zero critical bugs di Android 10 dan iOS 14

---

### REQ-NF-011: Aksesibilitas Web Portal Pembeli

**Description:** Antarmuka web portal pembeli shall ramah aksesibilitas untuk mendukung pembeli personal (termasuk penyandang disabilitas) sesuai standar WCAG 2.1 Level AA.

**Spesifikasi Aksesibilitas:**

| Kriteria WCAG 2.1 | Level | Implementasi |
|-------------------|-------|--------------|
| Contrast Ratio | AA | Teks minimal 4.5:1, teks besar minimal 3:1 |
| Keyboard Navigation | AA | Seluruh fungsi accessible via keyboard |
| Screen Reader Support | AA | ARIA labels, landmark regions, alt text |
| Focus Indicator | AA | Visible focus ring 2px solid contrast color |
| Error Identification | AAA | Kesalahan form diidentifikasi secara programmatic |
| Resize Text | AA | Zoom up to 200% tanpa horizontal scroll |

**Checklist Aksesibilitas Wajib:**

- [ ] Color-blind safe: tidak mengandalkan warna saja untuk informasi kritis
- [ ] Form labels: setiap input memiliki associated label
- [ ] Error messages: descriptive error di bawah field yang bermasalah
- [ ] Skip links: "Skip to main content" link untuk screen reader users
- [ ] CAPTCHA alternative: honey pot fields sebagai alternatif CAPTCHA visual

**Input:** User interaction dari keyboard atau assistive technology  
**Proses:** Event capture → ARIA interpretation → Semantic response  
**Output:** User-friendly feedback sesuai assistive technology  
**Prakondisi:** Web portal deployed, no JavaScript errors on page load  
**Pascakondisi:** Screen reader user dapat menyelesaikan purchase flow tanpa bantuan manusia  
**Acceptance Criteria:** Automated accessibility audit (Axe-core) menunjukkan 0 violations; manual testing dengan NVDA/JAWS/VoiceOver passed

---

## 4.5 Kebutuhan Portabilitas & Kompatibilitas (Portability & Compatibility)

### REQ-NF-012: Kompatibilitas Web Browser

**Description:** Web portal pembeli dan admin dealer shall kompatibel dengan seluruh browser modern untuk memastikan aksesibilitas tanpa memandang preferensi perangkat pengguna.

**Spesifikasi Browser Compatibility:**

| Browser | Minimum Version | Catatan |
|---------|-----------------|---------|
| Google Chrome | 90+ | Primary development target |
| Safari (macOS/iOS) | 14+ / 14+ | Vendor prefix untuk flexbox/grid |
| Mozilla Firefox | 88+ | Full ES2020+ support |
| Microsoft Edge | 90+ | Chromium-based |
| Opera | 76+ | Chromium-based |
| Samsung Internet | 14+ | Android default |

**Spesifikasi OS Compatibility:**

| OS | Minimum Version | Browser Default |
|----|-----------------|-----------------|
| Windows | Windows 10 | Edge 90+ |
| macOS | macOS 11 (Big Sur) | Safari 14+ |
| Linux | Ubuntu 20.04 LTS | Chrome 90+ |
| Android | Android 10+ | Chrome 90+ |
| iOS | iOS 14+ | Safari 14+ |

**Feature Detection Strategy:**

- Use @supports() untuk progressive enhancement
- Polyfill untuk ES features yang belum widespread
- No user-agent sniffing; rely on feature detection

**Input:** HTTP request dari berbagai browser/version combinations  
**Proses:** Feature detection → Apply polyfill if needed → Render appropriate UI  
**Output:** Consistent user experience across all supported browsers  
**Prakondisi:** Build system configured untuk multiple browser targets (Babel, Autoprefixer)  
**Pascakondisi:** Zero critical functionality difference antar browser  
**Acceptance Criteria:** Cross-browser test dengan BrowserStack/LambdaTest menunjukkan semua critical user flows functional di Chrome 90, Safari 14, Firefox 88, Edge 90

---

## 4.6 Ringkasan Kebutuhan Non-Fungsional

| ID | Kategori | Deskripsi Singkat | Target Kuantitatif |
|----|----------|-------------------|-------------------|
| REQ-NF-001 | Performa | Respons sistem internal | p50 ≤ 2s, p99 ≤ 3s |
| REQ-NF-002 | Performa | ESB Async leasing | ≤ 4,22 detik avg |
| REQ-NF-003 | Performa | Kapasitas konkuren | 10.000 users, 500 TPS |
| REQ-NF-004 | Keamanan | Enkripsi data sensitif | AES-256, TLS 1.3 |
| REQ-NF-005 | Keamanan | RBAC + MFA | MFA wajib untuk admin |
| REQ-NF-006 | Keamanan | Integrasi TTE | Privy/VIDA bersertifikasi |
| REQ-NF-007 | Keamanan | Face matching | Skor ≥ 85% |
| REQ-NF-008 | Keandalan | Ketersediaan sistem | 99,9% uptime |
| REQ-NF-009 | Keandalan | Graceful degradation | Circuit breaker + retry |
| REQ-NF-010 | Usabilitas | Mobile inspeksi | Android 10+, iOS 14+ |
| REQ-NF-011 | Usabilitas | Web aksesibilitas | WCAG 2.1 AA |
| REQ-NF-012 | Portabilitas | Browser compatibility | Chrome 90+, Safari 14+ |

---

### BAB 5: VERIFIKASI & TRACEABILITY MATRIX

Bab ini menjabarkan metode verifikasi untuk seluruh kebutuhan dalam dokumen SRS ini, matriks ketertelusuran kebutuhan (Requirements Traceability Matrix), matriks CRUD terhadap entitas data, serta kriteria penerimaan akhir sebelum sistem siap dideploy ke lingkungan produksi.

---

## 5.1 Metode Verifikasi Kebutuhan (Requirements Verification Methods)

Seluruh kebutuhan dalam Bab 3 dan Bab 4 shall diverifikasi menggunakan empat metode standar IEEE 29148:2018. Setiap kebutuhan shall memiliki minimal satu metode verifikasi yang sesuai.

### 5.1.1 Test (T) — Pengujian Fungsional dan Kuantitatif

**Deskripsi:** Verifikasi melalui eksekusi fungsi sistem secara langsung menggunakan alat ukur khusus untuk menghasilkan data kuantitatif yang objektif.

**Cakupan:**
- **Load Testing:** Simulasi beban konkuren menggunakan JMeter/Artillery/k6 untuk mengukur response time, throughput, dan error rate sesuai REQ-NF-001 dan REQ-NF-003.
- **Penetration Testing:** Pengujian keamanan oleh pihak ketiga independen untuk memvalidasi REQ-NF-004 (enkripsi), REQ-NF-005 (RBAC/MFA), dan REQ-NF-007 (face matching anti-spoofing).
- **API Integration Testing:** Verifikasi integrasi API eksternal (Dukcapil, Samsat, SLIK, AHU, TTE) dengan mock server dan test credentials.
- **End-to-End Testing:** skenario transaksi lengkap dari registrasi hingga serah terima unit.

**Acceptance:** Hasil test dievaluasi terhadap acceptance criteria spesifik (misalnya: p99 ≤ 3 detik, zero SQL injection vulnerability, face matching false acceptance rate < 0,1%).

### 5.1.2 Demonstration (D) — Demontrasi Visual Operasional

**Deskripsi:** Verifikasi dengan menunjukkan operasional visual sistem di depan pengguna atau stakeholder tanpa menggunakan instrumentasi otomatis.

**Cakupan:**
- **Demonstrasi Proses Inspeksi:** Inspektor lapangan mendemonstrasikan pengisian checklist 150-188 titik melalui aplikasi mobile.
- **Demonstrasi Pricing Engine:** Sales/Pembeli melihat simulasi kalkulasi pajak real-time dengan input variabel NJKB berbeda.
- **User Acceptance Testing (UAT):** Pembeli dan dealer mendemonstrasikan kelengkapan alur transaksi dalam environment staging.
- **Demo Signing Flow:** Prosedur penandatanganan SPK/kontrak kredit menggunakan API TTE (Privy/VIDA).

**Acceptance:** Stakeholder menandatangani dokumen UAT sign-off sheet yang mengonfirmasi bahwa sistem berjalan sesuai ekspektasi fungsional.

### 5.1.3 Inspection (I) — Pemeriksaan Statis

**Deskripsi:** Verifikasi statis dengan pemeriksaan artefak development tanpa menjalankan sistem.

**Cakupan:**
- **Code Review:** Pemeriksaan source code untuk memastikan kolom PII terenkripsi AES-256 (REQ-NF-004), RBAC terimplementasi dengan benar (REQ-NF-005), dan tidak ada hardcoded credentials.
- **Database Schema Audit:** Verifikasi bahwa field sensitif (NIK, NPWP, nomor_rangka) menggunakan tipe data varbinary atau encrypted storage.
- **Configuration Review:** Pemeriksaan konfigurasi server, TLS certificates, dan key rotation policy.
- **Security Audit:** Review OWASP Top 10 compliance, SSL/TLS configuration, dan backup encryption.

**Acceptance:** Laporan audit yang ditandatangani oleh Security Officer dan Lead Developer.

### 5.1.4 Analysis (A) — Analisis Logika dan Pemodelan

**Deskripsi:** Verifikasi melalui pembuktian logika, kalkulasi matematis, atau pemodelan formal.

**Cakupan:**
- **pricing Engine Calculation Proof:** Verifikasi manual bahwa formula DPP PPN = 11/12 × NJKB, PPN = 12% × DPP, Opsen PKB = 66% × PKB Pokok menghasilkan output akurat hingga 2 desimal.
- **Credit Scoring Logic:** Analisis rule-based scoring algorithm untuk pengajuan kredit (REQ-FINT-002) menggunakan sample data iDeb.
- **Fidusia Deadline Calculation:** Verifikasi bahwa sistem secara akurat menghitung countdown 30 hari kalender dan memblokir submission jika deadline tercapai (REQ-FINT-008).
- **Tax Progressive Simulation:** Pemodelan skenario kepemilikan kendaraan ke-1 hingga ke-5+ dengan tarif 2%-6% untuk validasi REQ-TAX-012.

**Acceptance:** Dokumen analisis matematis/logis yang ditandatangani oleh Technical Architect dan Business Analyst.

---

## 5.2 Matriks Ketertelusuran Kebutuhan (Requirements Traceability Matrix - RTM)

Matriks berikut memetakan Requirement ID dari Bab 3 dan Bab 4 ke Use Case ID (Bab 2), Metode Verifikasi, dan Acceptance Criteria spesifik.

| Requirement ID | Nama Kebutuhan | Use Case ID | Metode Verifikasi | Acceptance Criteria |
| --- | --- | --- | --- | --- |
| REQ-TAX-001 | Perhitungan DPP PPN Nilai Lain | UC-01 | Analysis | Hasil hitung DPP PPN = 11/12 × NJKB tepat hingga 2 desimal |
| REQ-TAX-002 | Perhitungan PPN Terutang (12%) | UC-01 | Test | PPN = 12% × DPP PPN menghasilkan nilai yang konsisten dengan manual calculator |
| REQ-TAX-003 | Deteksi PPN DTP untuk KBLBB | UC-01 | Test | Kendaraan dengan TKDN ≥ 40% menghasilkan PPN efektif 2%, non-KBLBB menghasilkan 11% |
| REQ-TAX-005 | Perhitungan Opsen PKB (66%) | UC-01 | Analysis | Opsen PKB = 66% × PKB Pokok akurat dan ditampilkan terpisah |
| REQ-TAX-007 | Perhitungan Opsen BBNKB (66%) | UC-01 | Analysis | Opsen BBNKB = 66% × BBNKB Pokok akurat dan ditampilkan terpisah |
| REQ-TAX-011 | Deteksi Pajak Progresif via API Samsat | UC-01 | Test | Query API Samsat sukses mengembalikan jumlah unit terdaftar atas NIK |
| REQ-TAX-012 | Penentuan Tarif Progresif (2%-6%) | UC-01 | Test | Kendaraan ke-1 = 2%, ke-2 = 3%, ke-3 = 4%, ke-4 = 5%, ke-5+ = 6% |
| REQ-TAX-014 | DP Dinamis Berdasarkan NPF Leasing | UC-03 | Test | NPF ≤ 1% → DP 0%, NPF 1-3% → DP 10%, NPF 3-5% → DP 15%, NPF > 5% → DP 20% |
| REQ-DOC-001 | Validasi Format NIK (16 digit) | UC-04 | Test | Input NIK invalid ditolak dengan error message spesifik |
| REQ-DOC-008 | Verifikasi Hologram Tri Brata pada BPKB | UC-06 | Test | Algoritma Image Analysis sukses mendeteksi hologram Tri Brata dengan akurasi ≥ 95% |
| REQ-DOC-009 | Verifikasi Benang UV pada Dokumen | UC-06 | Test | Benang UV terdeteksi pada dokumen asli, tidak terdeteksi pada dokumen palsu |
| REQ-DOC-010 | Konfirmasi STNK Aktif via API Samsat | UC-06 | Test | STNK expired atau terblokir ETLE triggering rejection |
| REQ-DOC-011 | Verifikasi Kwitansi Bermaterai (BBN-II) | UC-06 | Inspection | Kwitansi bermaterai tersedia minimal 1 rangkap untuk BBN-II |
| REQ-DOC-012 | Validasi SPH (Eks-Aset Perusahaan) | UC-06 | Inspection | Unit eks-aset perusahaan memiliki SPH yang valid dan ditandatangani authorized person |
| REQ-DOC-014 | Enforcement Matriks Validasi Transaksi | UC-04, UC-06 | Test | Transaksi hanya proceed jika seluruh checklist dokumen wajib terpenuhi |
| REQ-FINT-001 | Pengambilan Data iDeb via SLIK | UC-03 | Test | API SLIK mengembalikan data iDeb valid dalam format JSON |
| REQ-FINT-002 | Analisis Kelayakan Kredit Otomatis | UC-03 | Analysis | Preliminary approval/rejection sesuai dengan rule-based scoring algorithm |
| REQ-FINT-004 | Submission Kredit via ESB Async | UC-03 | Test | Message successfully published ke RabbitMQ/Kafka dengan acknowledgment |
| REQ-FINT-008 | Batas Waktu Jaminan Fidusia (30 Hari) | UC-09 | Analysis | Sistem memblokir submission jika input akta notaris melebihi 30 hari kalender |
| REQ-FINT-009 | Pengunduhan Sertifikat Fidusia | UC-09 | Test | Sertifikat fidusia elektronik berhasil didownload dan verified |
| REQ-LOG-002 | Generation STCK (Masa Berlaku 1 Bulan) | UC-10 | Test | Masa berlaku STCK exactly 30 hari dari tanggal issuance |
| REQ-LOG-003 | Enforcement Aturan Penggunaan STCK | UC-10 | Test | Sistem memblokir shipment jika STCK digunakan untuk keperluan pribadi |
| REQ-LOG-004 | Checklist PDI: De-waxing Bodi | UC-10 | Demonstration | Teknisi mendemonstrasikan de-waxing dengan photo/video dokumentasi |
| REQ-LOG-006 | Checklist PDI: Scan OBD-II (Zero DTC) | UC-10 | Test | Laporan scan OBD-II menunjukkan 0 active DTC sebelum shipment |
| REQ-LOG-008 | Checklist PDI: Uji Jalan Minimal 10 KM | UC-10 | Test | Log GPS mencatat jarak uji jalan ≥ 10 km |
| REQ-LOG-009 | Generation Laporan PDI Digital | UC-10 | Test | Laporan PDI lengkap dalam PDF dengan digital signature teknisi dan supervisor |
| REQ-LOG-010 | Approval Shipment Setelah PDI Lulus | UC-10 | Test | Unit dengan PDI failed tidak dapat di-ship ke konsumen |
| REQ-LOG-012 | Aktivasi Garansi Komponen Utama | UC-11 | Test | Garansi aktif dengan start date = tanggal serah terima |
| REQ-INT-001 | Face Matching Dukcapil (≥ 85%) | UC-04 | Test | Skor kecocokan biometrik face matching minimal 85% untuk registrasi approval |
| REQ-INT-002 | Verifikasi STNK via API Samsat | UC-06 | Test | API Samsat mengembalikan status STNK valid dan ETLE status |
| REQ-INT-004 | Tanda Tangan Digital SPK (Privy/VIDA) | UC-05 | Test | Signed SPK passes LTV validation dan certificate verifiable di portal Kominfo |
| REQ-NF-001 | Respons Sistem Internal (p50 ≤ 2s, p99 ≤ 3s) | All UC | Test | Load test menunjukkan p50 ≤ 2s dan p99 ≤ 3s selama peak hours |
| REQ-NF-002 | Latensi Async ESB Leasing (≤ 4,22 detik) | UC-03 | Test | Waktu respons pengiriman melalui message queue maksimal 4,22 detik avg |
| REQ-NF-003 | Kapasitas Konkuren (500 TPS) | All UC | Test | Load test 500 TPS sustained selama 30 menit dengan < 1% error rate |
| REQ-NF-004 | Enkripsi Data Sensitif (AES-256, TLS 1.3) | All UC | Inspection | Penetration test menunjukkan 0 vulnerabilitas enkripsi |
| REQ-NF-005 | RBAC + MFA untuk Admin | All UC | Test | Akses tanpa MFA ke endpoint admin mengembalikan HTTP 401 |
| REQ-NF-006 | Integrasi TTE Tersertifikasi Kominfo | UC-05, UC-09 | Inspection | Sertifikat TTE valid dan verifiable di portal Kominfo |
| REQ-NF-007 | Face Matching ≥ 85% (Anti-Spoofing) | UC-04 | Test | False acceptance rate < 0,1%; spoofing attack detection > 99% |
| REQ-NF-008 | Ketersediaan Sistem (99,9% Uptime) | All UC | Test | Monthly SLA report menunjukkan uptime ≥ 99,9% |
| REQ-NF-009 | Graceful Degradation (Circuit Breaker) | All UC | Test | Simulasi downtime API menunjukkan 0% user-facing crash |
| REQ-NF-010 | Kompatibilitas Mobile (Android 10+, iOS 14+) | UC-07 | Test | SUS score ≥ 70; zero critical bugs di Android 10 dan iOS 14 |
| REQ-NF-011 | Aksesibilitas Web (WCAG 2.1 AA) | UC-02 | Inspection | Automated accessibility audit (Axe-core) menunjukkan 0 violations |
| REQ-NF-012 | Browser Compatibility (Chrome 90+, Safari 14+) | UC-02 | Test | Semua critical user flows functional di Chrome 90, Safari 14, Firefox 88, Edge 90 |

---

## 5.3 CRUD Matrix (Entity-Requirements Mapping)

Matriks berikut memetakan entitas data utama sistem terhadap operasi Create, Read, Update, Delete beserta Requirement ID fungsional (Bab 3) yang mengaturnya.

| Entitas Data | Create | Read | Update | Delete |
| --- | --- | --- | --- | --- |
| **Data Kendaraan** | REQ-LOG-001 (Shipment tracking: Sales/PDI) | REQ-TAX-016 (Rincian harga on-the-road: Pembeli/Sales) | REQ-LOG-012 (Aktivasi garansi: Sistem) | Tidak Diizinkan (Soft Delete Only) |
| **Dokumen Jual-Beli** | REQ-DOC-014 (Enforcement matriks validasi: Pembeli/Sales) | REQ-DOC-003 (Validasi kecocokan identitas: Admin) | REQ-INT-004 (TTE SPK signed: Pembeli/Sales) | Tidak Diizinkan |
| **Aplikasi Kredit** | REQ-FINT-004 (Submission via ESB: Pembeli) | REQ-FINT-005 (Tracking status: Surveyor/Sales) | REQ-FINT-005 (Callback status update: Leasing) | Tidak Diizinkan |
| **Jaminan Fidusia** | REQ-FINT-007 (Pendaftaran ke Ditjen AHU: Notaris/Leasing) | REQ-FINT-009 (Pengunduhan sertifikat: Leasing/Admin) | REQ-FINT-008 (Monitoring 30 hari: Sistem) | Tidak Diizinkan |
| **Data Inspeksi (PDI)** | REQ-LOG-004 s/d REQ-LOG-008 (Checklist: Inspektor) | REQ-LOG-009 (Laporan PDI: Admin/Teknisi) | REQ-LOG-010 (Approval/rejection: Supervisor) | Tidak Diizinkan (Audit trail only) |
| **Data Identitas Pembeli** | REQ-INT-001 (Registrasi + face matching: Pembeli) | REQ-DOC-001 (Validasi NIK: Admin) | REQ-DOC-003 (Update setelah verifikasi: Sistem) | Tidak Diizinkan (Regulatory retention) |
| **Dokumen iDeb (SLIK)** | REQ-FINT-001 (Pengambilan dari SLIK: Sistem) | REQ-FINT-002 (Analisis kelayakan: Surveyor) | Tidak Diizinkan | Tidak Diizinkan |
| **Serah Terima Unit** | REQ-LOG-011 (Generation faktur: Admin) | REQ-LOG-011 (Bukti serah terima: Pembeli) | Tidak Diizinkan | Tidak Diizinkan |

**Catatan:** Seluruh entitas menggunakan soft delete pattern (flagging `deleted_at` timestamp) untuk memenuhi regulatory retention requirement. Hard delete hanya dapat dilakukan oleh System Admin dengan audit log komprehensif.

---

## 5.4 Kriteria Penerimaan Akhir (System Acceptance Criteria)

Sebelum sistem siap dideploy ke lingkungan produksi, seluruh kriteria berikut shall terpenuhi dan divalidasi oleh Project Manager, Business Owner, dan IT Security Officer.

### 5.4.1 Kriteria Kelengkapan Fungsional

| ID | Kriteria | Target | Metode Verifikasi |
|----|----------|--------|-------------------|
| F-001 | Seluruh Functional Requirements (REQ-F, REQ-TAX, REQ-DOC, REQ-FINT, REQ-LOG, REQ-INT) telah lulus verifikasi | 100% (59/59 requirements) | Test, Analysis |
| F-002 | Seluruh Use Case (14 UC dari Bab 2) dapat dieksekusi end-to-end tanpa error | 100% (14/14 UC) | Demonstration (UAT) |
| F-003 | Seluruh API eksternal (Dukcapil, SLIK, Samsat, AHU, TTE) terintegrasi dan berfungsi | 5/5 API integrated | Test |

### 5.4.2 Kriteria Performa

| ID | Kriteria | Target | Metode Verifikasi |
|----|----------|--------|-------------------|
| P-001 | Respons sistem internal | p50 ≤ 2 detik, p99 ≤ 3 detik | Load Test (JMeter/k6) |
| P-002 | Respons ESB Async leasing | Average ≤ 4,22 detik | Load Test |
| P-003 | Kapasitas konkuren | 500 TPS sustained dengan < 1% error rate | Load Test |
| P-004 | Database query performance | < 500ms untuk single entity lookup | Performance Test |

### 5.4.3 Kriteria Keamanan

| ID | Kriteria | Target | Metode Verifikasi |
|----|----------|--------|-------------------|
| S-001 | Zero critical security vulnerability | CVSS Score 0 untuk Critical | Penetration Test |
| S-002 | Data encryption at rest | AES-256-GCM terimplementasi | Inspection (Code Review) |
| S-003 | Data encryption in transit | TLS 1.3 untuk seluruh endpoints | Inspection (SSL Labs) |
| S-004 | RBAC + MFA enforcement | Akses tanpa MFA mengembalikan HTTP 401 | Test |
| S-005 | Face matching anti-spoofing | False Acceptance Rate < 0,1% | Test (Spoofing Dataset) |

### 5.4.4 Kriteria Keandalan

| ID | Kriteria | Target | Metode Verifikasi |
|----|----------|--------|-------------------|
| R-001 | Uptime availability | ≥ 99,9% per bulan | Monitoring (Grafana) |
| R-002 | Graceful degradation | Zero user-facing crash saat API downstream down | Chaos Engineering Test |
| R-003 | Backup and recovery | Recovery Point Objective (RPO) ≤ 1 jam, Recovery Time Objective (RTO) ≤ 4 jam | Disaster Recovery Test |

### 5.4.5 Kriteria Dokumentasi dan Compliance

| ID | Kriteria | Target | Metode Verifikasi |
|----|----------|--------|-------------------|
| C-001 | Dokumen SRS lengkap | Seluruh BAB terisi sesuai IEEE 29148:2018 | Inspection |
| C-002 | Traceability matrix | REQ → Use Case → Test Case | Inspection |
| C-003 | Security assessment report | Approved oleh Security Officer | Inspection |
| C-004 | Regulatory compliance (UU ITE, UU PDP, POJK) | Zero violation | Legal Review |
| C-005 | SLA document dengan leasing partner | Signed oleh kedua belah pihak | Inspection |

### 5.4.6 Deployment Readiness Checklist

```
□ 100% Functional Requirements lulus verifikasi (Test/Analysis/Demonstration/Inspection)
□ 100% Non-Functional Requirements tercapai (Load Test, Security Test, etc.)
□ Zero Critical/High severity bugs terbuka
□ Kontrak SLIK, Samsat, Dukcapil, AHU, TTE production credentials configured
□ Database migration scripts tested di staging
□ Rollback plan documented dan tested
□ Monitoring dashboards (Grafana/Datadog) configured
□ Alert rules configured (PagerDuty/Slack integration)
□ CDN cache purged, static assets versioned
□ CDN/WAF rules configured untuk production traffic
□ SSL certificates renewed dan auto-rotation enabled
□ Load balancer health checks configured
□ Penetration test report clean (0 Critical/High vulnerabilities)
□ UAT sign-off sheet signed oleh Business Owner
□ Legal review passed (UU ITE, UU PDP, POJK)
□ Go-Live announcement prepared
□ Support team trained dan on-call roster prepared
```

---

**Catatan Penyusun:**

Dokumen SRS ini telah lengkap ditulis pada tanggal 13 Juni 2026. Seluruh 5 BAB telah diisi sesuai standar ISO/IEC/IEEE 29148:2018 dengan total **59 requirements** yang terdistribusi sebagai berikut:

| Bab | Kategori | Jumlah |
|-----|----------|--------|
| BAB 3 | Functional Requirements (REQ-TAX, REQ-DOC, REQ-FINT, REQ-LOG, REQ-INT) | 47 |
| BAB 4 | Non-Functional Requirements (REQ-NF) | 12 |
| **Total** | | **59** |

Dokumen ini siap untuk dilakukan final review oleh seluruh stakeholder dan ditransisikan ke fase development setelah UAT sign-off.

**Last Updated:** 13 Juni 2026 - Dokumen SRS lengkap (BAB 1-5)

