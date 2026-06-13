Untuk menyusun dokumen **Spesifikasi Kebutuhan Perangkat Lunak (SRS)** yang komprehensif, Anda harus memahami proses bisnis, aturan hukum, dan kebutuhan data di Indonesia secara mendalam.

Berikut adalah ringkasan informasi penting yang wajib Anda ketahui dan petakan ke dalam modul-modul sistem pada SRS Anda:

---

### 1. Perhitungan Harga & Perpajakan (Modul *Pricing Engine*)

Kalkulator di sistem Anda tidak boleh hanya menghitung harga unit, tetapi harus mengintegrasikan kalkulasi pajak berlapis secara dinamis:

* **Pajak Pertambahan Nilai (PPN):** Tarif PPN adalah 12%. Namun, Dasar Pengenaan Pajak (DPP) menggunakan mekanisme "Nilai Lain" yaitu sebesar 11/12 dari Nilai Jual Kendaraan Bermotor (NJKB). Formulasi matematisnya:



$$\text{DPP PPN} = \frac{11}{12} \times \text{NJKB}$$


* **Pajak Daerah & Sistem Opsen (UU HKPD):** Berdasarkan aturan terbaru, terdapat pengenaan **Opsen PKB** sebesar 66% dari nilai PKB Pokok, serta **Opsen BBNKB** sebesar 66% dari nilai BBNKB Pokok. Sistem keuangan Anda harus memisahkan porsi opsen ini untuk mempermudah pelaporan keuangan daerah.


* **Logika Pajak Progresif:** Pajak ini dikenakan jika pelanggan membeli kendaraan tambahan dengan NIK atau alamat Kartu Keluarga (KK) yang sama. Untuk memproyeksikan beban pajak tahunan pelanggan di dalam sistem, NJKB dicari menggunakan rumus:



$$\text{NJKB} \approx \frac{\text{PKB Pokok}}{\text{Tarif Berlaku}}$$



Setelah NJKB ditemukan, sistem akan mengalikan tarif eskalasi progresif (misalnya di Jakarta berkisar antara 2% hingga 6%) dan menambahkan komponen wajib SWDKLLJ (Rp143.000 hingga Rp150.000).



---

### 2. Aturan Validasi Dokumen (Modul *Document Management*)

Sistem Anda harus menerapkan aturan validasi input data (*input validation rules*) yang ketat berdasarkan jenis transaksi :

* **Kendaraan Baru (*New Car*):** Membutuhkan dokumen KTP, NIK, Surat Registrasi Uji Tipe (SRUT), Surat Tanda Coba Kendaraan (STCK), serta Form A (khusus kendaraan impor utuh/CBU).
* **Kendaraan Bekas (*Used Car*):** Aturan validasi menjadi lebih ketat untuk mencegah penipuan. Sistem wajib memverifikasi dokumen fisik asli seperti BPKB (memeriksa keberadaan hologram Tri Brata dan benang UV)  serta STNK yang aktif pajak.


* **Ketentuan Tambahan:**
* **Kwitansi kosong (2-3 rangkap):** Wajib ditandatangani oleh pemilik pertama sesuai BPKB (untuk proses balik nama BBN-II).


* **Surat Pelepasan Hak (SPH):** Wajib diunggah jika kendaraan bekas merupakan eks-aset operasional perusahaan.


* **Dokumen Kredit Aktif:** Jika mobil bekas masih dalam masa cicilan, sistem harus meminta salinan kontrak pembiayaan, fotokopi BPKB terlegalisir, dan histori setoran terakhir.





---

### 3. Kepatuhan Regulasi Kredit & Pembiayaan (Modul *Leasing & Finance*)

Jika sistem memfasilitasi pembayaran kredit, Anda harus memasukkan aturan bisnis (*business rules*) berdasarkan regulasi Otoritas Jasa Keuangan (OJK):

* **Batas Uang Muka (DP Minimum):** Besaran DP ditentukan secara dinamis oleh tingkat kesehatan keuangan (rasio NPF net) dari perusahaan leasing yang bekerja sama dengan sistem Anda, dengan tarif berkisar dari 0% hingga 20%.


* **Insentif Kendaraan Listrik (KBLBB):** Sistem pembiayaan harus memberikan opsi DP 0% untuk mobil listrik murni. Selain itu, ada pemotongan PPN Ditanggung Pemerintah (PPN DTP) sebesar 10% untuk mobil listrik yang memenuhi syarat TKDN minimal 40%, sehingga PPN efektif di diler hanya dihitung sebesar 2%.


* **Pendaftaran Jaminan Fidusia:** Untuk menjamin keamanan hukum diler/leasing, sistem harus melacak pendaftaran fidusia elektronik ke Ditjen AHU Kemenkumham. Hukum menetapkan batas waktu mutlak **maksimal 30 hari kalender** sejak pembuatan akta notaris fidusia. Jika lewat dari 30 hari, pendaftaran otomatis ditolak oleh sistem Kemenkumham.



---

### 4. Manajemen Logistik &QC (Modul *Pre-Delivery Inspection*)

Sebelum status kendaraan diubah menjadi "Siap Dikirim" di sistem, harus ada alur kerja (*workflow*) digital untuk pengiriman barang:

* **PDI Checklist:** Teknisi wajib mencentang Lembar Pemeriksaan PDI di sistem. Tindakan teknis meliputi de-waxing bodi mobil, pemasangan sekring cadangan (*backup fuse*), penghapusan kode eror (*Diagnostic Trouble Codes* - DTC) pada ECU menggunakan alat pemindai, hingga uji jalan sejauh minimal 10 km.


* **Regulasi Pengiriman & STCK:** Kendaraan baru yang dikirim wajib dilengkapi dengan STCK dan pelat sementara (merah-putih). Aturan bisnis sistem harus mencatat bahwa STCK hanya berlaku **1 bulan** , biaya administrasi PNBP resminya adalah Rp50.000 , dan kendaraan dilarang keras digunakan untuk aktivitas publik di luar pengiriman unit (tidak boleh dibawa ke luar kota).



---

### 5. Arsitektur Integrasi API Pihak Ketiga (Modul *System Integration*)

Agar sistem berjalan otomatis (Online-to-Offline / O2O) seperti platform e-commerce otomotif profesional (contoh: OLXmobbi atau Caroline.id), dokumen SRS Anda harus menspesifikasikan integrasi API eksternal berikut:

* **API Dukcapil:** Untuk memverifikasi NIK KTP konsumen menggunakan pencocokan wajah (*face matching*) demi keamanan data.
* **API SLIK OJK / PEFINDO:** Untuk melakukan pemeriksaan riwayat kredit (*credit checking*) konsumen secara instan saat pengajuan leasing.


* **API Samsat / Bapenda (SIGNAL):** Untuk melacak status blokir tilang elektronik (ETLE), keaslian STNK mobil bekas, serta cek nilai pajak progresif.
* **API Ditjen AHU (Fidusia Online):** Memungkinkan diler atau leasing mendaftarkan jaminan fidusia secara otomatis langsung ke database Kemenkumham.


* **API Digital Signature (Privy / Vida):** Untuk proses tanda tangan Surat Pemesanan Kendaraan (SPK) dan kontrak perjanjian kredit pembiayaan yang sah secara hukum tanpa kertas (*paperless*).