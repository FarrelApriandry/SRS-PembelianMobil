**SPESIFIKASI KEBUTUHAN PERANGKAT LUNAK (SRS)**  
**SISTEM TRANSAKSI PEMBELIAN KENDARAAN BERMOTOR INTEGRASI (INDONESIA)**

DISUSUN OLEH :  
Fadlika Alfi Nurrochmah 			(2410506010)  
Muhammad Ramdhan 			(2420506027)  
Assep Wahid 					(2420506028)  
Ilyasa Abiyyu Wicaksono 			(2420506035)  
Farrel Apriandry Ciu 				(2430506056)

**UNIVERSITAS TIDAR**   
**2026**  
DAFTAR ISI

[BAB 1: PENDAHULUAN	4](#heading=)

[1.1 Tujuan Dokumen SRS	4](#heading=)

[1.2 Ruang Lingkup Produk	4](#heading=)

[1.2.1 Transaksi Kendaraan Baru	4](#heading=)

[1.2.2 Transaksi Kendaraan Bekas	4](#heading=)

[1.2.3 Modul Financing (Pembiayaan Kredit)	4](#heading=)

[1.3 Definisi, Akronim, dan Singkatan (Glosarium)	5](#heading=)

[1.4 Referensi Regulatory dan Standar	6](#heading=)

[1.5 Konvensi Dokumen	7](#heading=)

[BAB 2](#heading=)  
[DESKRIPSI KESELURUHAN	8](#heading=)

[2.1 Perspektif Produk	8](#heading=)

[2.1.1 Context Diagram (Mermaid)	8](#heading=)

[2.2 Fungsi Produk	8](#heading=)

[2.3 Karakteristik Pengguna (User Classes)	9](#heading=)

[2.4 Batasan Sistem	9](#heading=)

[2.5 Asumsi dan Ketergantungan	9](#heading=)

[BAB 3](#heading=)  
[KEBUTUHAN SPESIFIK SISTEM	11](#heading=)

[3.1 Kebutuhan Modul Pricing Engine (Kalkulasi Pajak)	11](#heading=)

[3.1.1 Perhitungan Pajak Pertambahan Nilai (PPN)	11](#heading=)

[3.1.2 Perhitungan Pajak Daerah (PKB dan Opsen)	11](#heading=)

[3.1.3 Perhitungan Administrasi & PNBP Resmi Polri	12](#heading=)

[3.1.4 Perhitungan Pajak Progresif	12](#heading=)

[3.1.5 Simulasi Kredit	13](#heading=)

[3.2 Kebutuhan Modul Verifikasi Dokumen	13](#heading=)

[3.2.1 Validasi Identitas & Input	13](#heading=)

[3.2.2 Validasi Dokumen Transaksi Unit Baru	14](#heading=)

[3.2.3 Validasi Dokumen Transaksi Unit Bekas	14](#heading=)

[3.2.4 Matriks Penegakan Validasi	15](#heading=)

[3.3 Kebutuhan Modul Fintech dan Leasing	15](#heading=)

[3.3.1 Layanan Skor Kredit (OJK SLIK)	15](#heading=)

[3.3.2 Manajemen Pengajuan Pembiayaan	15](#heading=)

[3.3.3 Layanan Hukum Jaminan Fidusia (Kemenkumham)	16](#heading=)

[3.4 Kebutuhan Modul Logistik dan PDI	16](#heading=)

[3.4.1 Manajemen Logistik & STCK	16](#heading=)

[3.4.2 Pre-Delivery Inspection (PDI)	17](#heading=)

[3.4.3 Penyerahan Unit & Garansi	17](#heading=)

[3.5 Kebutuhan Integrasi API Eksternal	17](#heading=)

[BAB 4](#heading=)  
[KEBUTUHAN NON-FUNGSIONAL & KEAMANAN	19](#heading=)

[4.1 Kebutuhan Performa (Performance Requirements)	19](#heading=)

[4.2 Kebutuhan Keamanan & Perlindungan Data (Safety & Security)	19](#heading=)

[4.3 Kebutuhan Keandalan & Ketersediaan (Reliability & Availability)	20](#heading=)

[4.4 Kebutuhan Keterserapan & Antarmuka (Usability & Human Factors)	20](#heading=)

[4.5 Kebutuhan Portabilitas & Kompatibilitas (Portability & Compatibility)	20](#heading=)

[4.6 Ringkasan Kebutuhan Non-Fungsional	21](#heading=)

[BAB 5](#heading=)  
[VERIFIKASI & TRACEABILITY MATRIX	22](#heading=)

[5.1 Metode Verifikasi Kebutuhan	22](#heading=)

[5.2 Matriks Ketertelusuran Kebutuhan (Requirements Traceability Matrix)	22](#heading=)

[5.3 CRUD Matrix (Entity-Requirements Mapping)	23](#heading=)

[5.4 Kriteria Penerimaan Akhir (System Acceptance Criteria)	23](#heading=)

[Works cited	25](#heading=)

## 

## 

## **BAB 1: PENDAHULUAN**

### **1.1 Tujuan Dokumen SRS**

Dokumen Spesifikasi Kebutuhan Perangkat Lunak (SRS) ini bertujuan untuk mendefinisikan secara lengkap dan terperinci seluruh kebutuhan sistem informasi yang akan dikembangkan untuk memfasilitasi transaksi pembelian kendaraan roda empat (mobil) di Indonesia. Dokumen ini berfungsi sebagai kontrak formal antara tim pengembang dengan pemangku kepentingan (*stakeholders*), referensi utama bagi tim *Quality Assurance* (QA) untuk validasi pengujian, serta pedoman teknis bagi arsitek perangkat lunak dan programmer selama fase konstruksi kode.  
Sistem yang akan dibangun *shall* mencakup seluruh proses transaksi dari tahap estimasi harga online hingga serah terima unit fisik kepada konsumen, dengan mengintegrasikan berbagai pihak eksternal seperti Samsat, leasing multifinance, dan Ditjen AHU Kemenkumham.

### **1.2 Ruang Lingkup Produk**

Sistem Transaksi Pembelian Kendaraan Bermotor Terintegrasi dirancang untuk mendukung tiga skenario transaksi utama:

#### **1.2.1 Transaksi Kendaraan Baru**

Sistem *shall* memfasilitasi pembelian mobil baru dari diler resmi dengan fitur:

* Kalkulasi harga *On-The-Road* (OTR) secara *real-time* yang mencakup komponen PPN, PKB, BBNKB, SWDKLLJ, dan biaya administrasi PNBP.  
* Validasi dokumen wajib diler meliputi Faktur Kendaraan, SRUT (Surat Registrasi Uji Tipe), dan Form A untuk unit CBU (*Completely Built Up*).  
* Integrasi Tanda Tangan Elektronik (TTE) untuk penandatanganan Surat Pemesanan Kendaraan (SPK).

#### **1.2.2 Transaksi Kendaraan Bekas**

Sistem *shall* memfasilitasi jual-beli dan tukar-tambah (*trade-in*) kendaraan bekas dengan fitur:

* Inspeksi fisik digital menggunakan checklist 150 s.d. 188 titik pemeriksaan.  
* Verifikasi keaslian dokumen (STNK aktif, BPKB hologram Tri Brata & benang UV, kuitansi bermaterai, dan Surat Pelepasan Hak / SPH untuk eks-aset perusahaan).  
* Pencairan dana instan (*instant payout*) ke rekening penjual dalam waktu maksimal 60 menit setelah seluruh dokumen dinyatakan lengkap dan valid.

#### **1.2.3 Modul Financing (Pembiayaan Kredit)**

Sistem *shall* mengintegrasikan proses pengajuan kredit melalui perusahaan pembiayaan (*multifinance*) dengan ketentuan:

* Simulasi kredit dinamis yang menghitung uang muka (DP) berdasarkan rasio NPF (*Non-Performing Financing*) net dari perusahaan leasing partner.  
* Integrasi API SLIK OJK atau PEFINDO untuk pengambilan data iDeb secara otomatis.  
* Pendaftaran jaminan fidusia secara elektronik ke Ditjen AHU dengan batas waktu maksimal 30 hari kalender sesuai ketentuan regulasi.

### **1.3 Definisi, Akronim, dan Singkatan (Glosarium)**

Berikut adalah daftar istilah teknis dan legalitas otomotif di Indonesia yang digunakan dalam dokumen ini:

| Istilah | Definisi Resmi |
| ----- | ----- |
| **BBNKB** | Bea Balik Nama Kendaraan Bermotor \- Pajak daerah atas penyerahan hak kepemilikan kendaraan. |
| **BPKB** | Buku Pemilik Kendaraan Bermotor \- Dokumen bukti kepemilikan sah kendaraan yang diterbitkan oleh Polri. |
| **DPP** | Dasar Pengenaan Pajak \- Nilai dasar yang digunakan untuk menghitung pajak terutang. |
| **DUKCAPIL** | Dinas Kependudukan dan Pencatatan Sipil \- Instansi pengelola data kependudukan nasional. |
| **ETLE** | Electronic Traffic Law Enforcement \- Sistem tilang elektronik nasional. |
| **Fidusia** | Pengalihan hak kepemilikan benda dengan penguasaan fisik benda tetap pada pemberi fidusia. |
| **iDeb** | Informasi Debitur \- Kumpulan data fasilitas kredit nasabah yang tercatat dalam sistem informasi OJK. |
| **KBLBB** | Kendaraan Bermotor Listrik Berbasis Baterai \- Kendaraan yang digerakkan oleh energi listrik baterai. |
| **NJKB** | Nilai Jual Kendaraan Bermotor \- Nilai standar kendaraan yang ditetapkan oleh Dispenda/Bapenda. |
| **NPF** | Non-Performing Financing \- Rasio pembiayaan bermasalah pada perusahaan multifinance. |
| **Opsen** | Pungutan tambahan yang dikenakan oleh kabupaten/kota atas pajak provinsi (PKB dan BBNKB). |
| **PKB** | Pajak Kendaraan Bermotor \- Pajak atas kepemilikan dan/atau penguasaan kendaraan bermotor. |
| **PNBP** | Penerimaan Negara Bukan Pajak \- Pungutan pemerintah pusat di luar sektor pajak, dikelola oleh Polri. |
| **PPnBM** | Pajak Penjualan atas Barang Mewah \- Pajak pusat atas penyerahan barang kena pajak yang tergolong mewah. |
| **PPN** | Pajak Pertambahan Nilai \- Pajak atas konsumsi barang/jasa di dalam daerah pabean dengan tarif standar 12%. |
| **SLIK** | Sistem Layanan Informasi Keuangan \- Sistem informasi kelayakan kredit bentukan OJK. |
| **SRUT** | Surat Registrasi Uji Tipe \- Bukti kelayakan teknis dan jalan untuk tipe kendaraan baru. |
| **STCK** | Surat Tanda Coba Kendaraan \- Dokumen jalan sementara untuk kendaraan baru yang belum terbit STNK-nya. |
| **STNK** | Surat Tanda Nomor Kendaraan \- Bukti registrasi dan identifikasi kendaraan bermotor yang sah. |
| **SWDKLLJ** | Sumbangan Wajib Dana Kecelakaan Lalu Lintas Jalan \- Iuran wajib asuransi Jasa Raharja. |
| **TKDN** | Tingkat Komponen Dalam Negeri \- Persentase kandungan lokal pada perakitan kendaraan. |
| **TNKB** | Tanda Nomor Kendaraan Bermotor \- Pelat nomor resmi kendaraan yang diterbitkan oleh Polri. |
| **TTE** | Tanda Tangan Elektronik \- Tanda tangan digital yang memiliki kekuatan hukum setara tanda tangan basah. |

### **1.4 Referensi Regulatory dan Standar**

Sistem ini dirancang secara ketat untuk mematuhi regulasi hukum Indonesia berikut:

1. **Undang-Undang Nomor 1 Tahun 2022** tentang Hubungan Keuangan antara Pemerintah Pusat dan Pemerintahan Daerah (UU HKPD).  
2. **Peraturan Menteri Keuangan Nomor 131/PMK.010/2024** tentang Penyesuaian Tarif Pajak Pertambahan Nilai (PPN 12%).  
3. **Peraturan Pemerintah Nomor 21 Tahun 2015** tentang Tata Cara Pendaftaran Jaminan Fidusia dan Biaya Pembuatan Akta Jaminan Fidusia.  
4. **Peraturan Daerah DKI Jakarta Nomor 1 Tahun 2024** tentang Ketentuan Pajak Progresif Kendaraan Bermotor.  
5. **Peraturan Kepala Kepolisian Negara Republik Indonesia Nomor 7 Tahun 2021** tentang Registrasi dan Identifikasi Kendaraan Bermotor.  
6. **POJK Nomor 35/POJK.05/2018** dan **POJK Nomor 46 Tahun 2024** tentang Aturan Uang Muka Minimum Pembiayaan Kendaraan Bermotor.

### **1.5 Konvensi Dokumen**

Tingkat kewajiban persyaratan dalam dokumen ini mengacu pada standar RFC 2119:

* **shall:** Menunjukkan kebutuhan mutlak wajib (*mandatory*) yang harus dipenuhi oleh sistem.  
* **should:** Menunjukkan rekomendasi kuat yang sangat disarankan untuk diimplementasikan.  
* **may:** Menunjukkan fitur opsional yang bersifat tambahan.

Sistem penomoran persyaratan didefinisikan dengan format:

* REQ-TAX-XXX untuk modul perhitungan harga dan perpajakan (*pricing engine*).  
* REQ-DOC-XXX untuk modul manajemen dan verifikasi dokumen legalitas.  
* REQ-FINT-XXX untuk modul pembiayaan kredit dan jaminan fidusia.  
* REQ-LOG-XXX untuk modul Pre-Delivery Inspection (PDI) dan logistik pengiriman.  
* REQ-INT-XXX untuk kebutuhan integrasi API eksternal pihak ketiga.  
* REQ-NF-XXX untuk kebutuhan kualitas non-fungsional sistem.

## 

## **BAB 2** **DESKRIPSI KESELURUHAN**

### **2.1 Perspektif Produk**

Sistem ini merupakan platform omnichannel terintegrasi (*Online-to-Offline* / O2O) yang menghubungkan sistem internal diler (*dealer management system*) dengan jaringan pembiayaan pihak ketiga dan instansi pemerintah secara *real-time*.

#### **2.1.1 Context Diagram (Mermaid)**

![][image1]

### **2.2 Fungsi Produk**

Platform transaksi ini memiliki 5 modul fungsional utama:

1. **Pricing Engine:** Melakukan kalkulasi harga kendaraan secara transparan berdasarkan input NJKB, status kepemilikan progresif, tipe mesin (KBLBB/konvensional), dan opsi pembiayaan.  
2. **Appraisal & Inspeksi Fisik:** Memandu inspektor di lapangan untuk melakukan uji fisik kendaraan bekas di 150 s.d. 188 titik sensorik dan mekanis menggunakan aplikasi mobile.  
3. **Document Verification Manager:** Melakukan ekstraksi OCR dan validasi keaslian dokumen diler maupun dokumen kendaraan bekas secara otomatis.  
4. **Fintech Leasing Hub:** Mengirimkan aplikasi pengajuan kredit secara *asynchronous* ke leasing partner serta menangani pendaftaran hak jaminan fidusia secara digital.  
5. **PDI & Logistics Tracker:** Memantau tahapan penyiapan unit baru (PDI), pencetakan dokumen jalan sementara (STCK), hingga aktivasi garansi mesin setelah serah terima unit selesai.

### **2.3 Karakteristik Pengguna (User Classes)**

Sistem membedakan hak akses dan kewenangan pengguna berdasarkan matriks berikut:

| User Class | Peran & Deskripsi | Hak Akses Sistem |
| :---- | :---- | :---- |
| **Pembeli Personal** | Konsumen perorangan yang membeli atau menjual kendaraan. | Mengisi profil, melakukan simulasi, unggah KTP/KK, tanda tangan digital. |
| **Sales Dealer** | Staf pemasaran diler yang melayani prospek pelanggan. | Membuat draf SPK, mengonfigurasi rincian kendaraan, memantau draf kredit. |
| **Admin Dealer** | Staf *backoffice* pengurus administrasi diler. | Verifikasi dokumen wajib, mendaftarkan data kendaraan ke Samsat, alokasi biaya PNBP. |
| **Inspektor Lapangan** | Teknisi penilai kondisi fisik kendaraan bekas. | Mengisi checklist inspeksi mobile, unggah dokumentasi foto/video 360°. |
| **Surveyor Leasing** | Analis risiko dari perusahaan pembiayaan partner. | Meninjau aplikasi kredit, mengakses skor iDeb SLIK, melakukan persetujuan limit. |
| **Teknisi PDI** | Teknisi bengkel diler yang melakukan penyiapan unit baru. | Mengisi checklist pengujian PDI, mendaftarkan nomor rangka ke modul garansi. |

### **2.4 Batasan Sistem**

* **Batasan Geografis:** Sistem hanya dapat memproses perhitungan pajak daerah dan data pendaftaran Samsat untuk wilayah hukum Republik Indonesia.  
* **Batasan Mata Uang:** Seluruh modul kalkulasi keuangan *shall* menggunakan mata uang Rupiah Indonesia (IDR) secara eksklusif dengan presisi 2 angka di belakang desimal.  
* **SLA API Pihak Ketiga:** Ketersediaan dan kecepatan data verifikasi berada di bawah ketergantungan server instansi pemerintah ( SIGNAL, Dukcapil, Ditjen AHU).

### **2.5 Asumsi dan Ketergantungan**

Sistem dirancang dengan asumsi bahwa:

1. API Dukcapil Kemendagri dapat diakses secara *real-time* untuk pencocokan data KTP dan foto wajah (*face matching*).  
2. Layanan OJK iDebku atau PEFINDO memberikan respons status kolektibilitas kredit nasabah secara akurat.  
3. Setiap data pendaftaran jaminan fidusia yang dikirimkan didukung oleh notaris berlisensi aktif yang terdaftar di Sistem SABH Ditjen AHU Kemenkumham.

## 

## **BAB 3** **KEBUTUHAN SPESIFIK SISTEM**

### **3.1 Kebutuhan Modul Pricing Engine (Kalkulasi Pajak)**

#### **3.1.1 Perhitungan Pajak Pertambahan Nilai (PPN)**

* **REQ-TAX-001: Perhitungan DPP PPN Nilai Lain**

  Sistem *shall* menghitung nilai Dasar Pengenaan Pajak (DPP) untuk penyerahan kendaraan bermotor menggunakan tarif mekanisme Nilai Lain sesuai UU HPP dengan rumusan:

  ![][image2]

* **Input:** Nilai Jual Kendaraan Bermotor (NJKB) hasil query database Samsat.  
* **Proses:** Mengalikan nilai NJKB dengan koefisien ![][image3] secara presisi.  
* **Output:** Nilai DPP PPN dalam IDR.  
* **Acceptance Criteria:** Hasil perhitungan harus akurat tanpa adanya pembulatan prematur sebelum PPN terutang dihitung.

* **REQ-TAX-002: Perhitungan PPN Terutang Kendaraan Baru**

  Sistem *shall* menghitung nilai PPN terutang menggunakan tarif standar 12% sesuai PMK 131/2024 dari nilai DPP PPN Nilai Lain:

  ![][image4]

* **Input:** Nilai DPP PPN dari REQ-TAX-001.  
* **Proses:** Mengalikan DPP PPN dengan tarif 12%.  
* **Output:** Nilai PPN Terutang dalam IDR.  
* **Acceptance Criteria:** Hasil PPN terutang wajib bernilai tepat sebesar 11% dari nilai NJKB awal.

* **REQ-TAX-003: Kalkulasi Insentif PPN DTP untuk KBLBB murni**

  Sistem *shall* mendeteksi nilai Tingkat Komponen Dalam Negeri (TKDN) kendaraan listrik dan menerapkan potongan PPN Ditanggung Pemerintah (PPN DTP) jika memenuhi syarat:

* Jika Jenis Kendaraan \= Kendaraan Listrik Baterai (BEV) dan nilai TKDN ![][image5] 40%, maka PPN terutang yang dibayar konsumen diturunkan menjadi hanya 2% dari NJKB (potongan PPN DTP sebesar 10%) .  
* Jika Jenis Kendaraan \= Hybrid, sistem *shall* menolak pemberian insentif PPN DTP (tetap dikenakan tarif standar 11% dari NJKB) .

  #### **3.1.2 Perhitungan Pajak Daerah (PKB dan Opsen)**

* **REQ-TAX-004: Perhitungan Pokok Pajak Kendaraan Bermotor (PKB)**

  Sistem *shall* menghitung tarif dasar PKB kepemilikan pertama sebesar 1,2% s.d. 2% dari NJKB:

  ![][image6]

* **REQ-TAX-005: Perhitungan Opsen PKB Daerah**

  Sistem *shall* secara otomatis menghitung Opsen PKB tambahan sebesar 66% dari nilai PKB Pokok terutang untuk dialokasikan ke kas kabupaten/kota:

  ![][image7]

* **REQ-TAX-006: Perhitungan Pokok Bea Balik Nama (BBNKB)**

  Sistem *shall* menghitung tarif pokok BBNKB penyerahan pertama sebesar maksimal 12% dari nilai NJKB:

  ![][image8]

* **REQ-TAX-007: Perhitungan Opsen BBNKB Daerah**

  Sistem *shall* menghitung Opsen BBNKB tambahan sebesar 66% dari nilai BBNKB Pokok terutang:

  ![][image9]

  #### **3.1.3 Perhitungan Administrasi & PNBP Resmi Polri**

* **REQ-TAX-008: Kalkulasi SWDKLLJ Jasa Raharja**

  Sistem *shall* menetapkan nilai iuran asuransi SWDKLLJ secara flat berdasarkan golongan kendaraan pribadi roda empat sebesar Rp143.000 s.d. Rp150.000 sesuai regulasi Jasa Raharja .

* **REQ-TAX-009: Biaya Administrasi PNBP STNK dan TNKB**

  Sistem *shall* menjumlahkan biaya penerbitan dokumen kendaraan baru sesuai tarif PNBP resmi Polri: Penerbitan STNK baru sebesar Rp200.000 dan penerbitan TNKB baru sebesar Rp100.000 .

* **REQ-TAX-010: Biaya Administrasi PNBP BPKB**

  Sistem *shall* menerapkan biaya penerbitan buku BPKB baru sebesar Rp375.000 secara otomatis ke dalam tagihan diler .

  #### **3.1.4 Perhitungan Pajak Progresif**

* **REQ-TAX-011: Pengecekan NIK dan KK Berbasis API Samsat**

  Sistem *shall* mengirimkan NIK pembeli ke API Samsat/SIGNAL untuk mendeteksi jumlah kepemilikan mobil aktif pada alamat Kartu Keluarga (KK) yang sama.

* **REQ-TAX-012: Penerapan Tarif Pajak Progresif DKI Jakarta**

  Sistem *shall* mengalokasikan tarif PKB secara berjenjang berdasarkan urutan kepemilikan sesuai Perda DKI Jakarta No. 1 Tahun 2024:

* Kepemilikan Mobil Ke-1: Tarif PKB Pokok \= 2,0%.  
* Kepemilikan Mobil Ke-2: Tarif PKB Pokok \= 3,0%.  
* Kepemilikan Mobil Ke-3: Tarif PKB Pokok \= 4,0%.  
* Kepemilikan Mobil Ke-4: Tarif PKB Pokok \= 5,0%.  
* Kepemilikan Mobil Ke-5 dan seterusnya: Tarif PKB Pokok \= 6,0% (Flat Maksimal).

* **REQ-TAX-013: Formula Reverse-Calculation NJKB dari STNK**

  Sistem *shall* menyediakan fungsi kalkulasi balik untuk mencari nilai dasar NJKB mobil bekas berdasarkan nilai PKB tahunan yang tertera di STNK menggunakan formula:

  ![][image10]

  #### **3.1.5 Simulasi Kredit**

* **REQ-TAX-014: Kalkulasi DP Minimum Berbasis Rasio NPF Leasing (OJK)**

  Sistem *shall* menentukan batas uang muka (DP) minimal secara dinamis dengan mencocokkan profil rasio NPF Net dari perusahaan pembiayaan mitra sesuai aturan OJK:

  * Jika Rasio NPF Net leasing ![][image11] 1%: Batas DP Minimum \= 0% (Kebijakan DP Nol Persen).  
  * Jika 1% \< Rasio NPF Net leasing ![][image11] 3%: Batas DP Minimum \= 10%.  
  * Jika 3% \< Rasio NPF Net leasing ![][image11] 5%: Batas DP Minimum \= 15%.  
  * Jika Rasio NPF Net leasing \> 5%: Batas DP Minimum \= 20%.


* **REQ-TAX-015: Simulasi Angsuran dan Bunga Pembiayaan**

  Sistem *shall* menampilkan simulasi cicilan bulanan lengkap dengan bunga leasing, biaya administrasi fidusia, asuransi, dan biaya provisi secara transparan ke layar konsumen.

* **REQ-TAX-016: Konsolidasi Rincian Harga Akhir (On-The-Road)**

  Sistem *shall* menyajikan lembar rincian harga akhir (OTR) yang mengonsolidasikan harga dasar kendaraan, total pajak pusat, total pajak daerah (pokok & opsen), SWDKLLJ, dan PNBP dokumen secara rinci sebelum masuk ke tahap pembayaran.

### **3.2 Kebutuhan Modul Verifikasi Dokumen**

#### **3.2.1 Validasi Identitas & Input**

* **REQ-DOC-001: Validasi NIK Kependudukan**\\  
  	Sistem *shall* memvalidasi input NIK pembeli harus tepat berjumlah 16 digit numerik dan tidak mengandung karakter spesial atau spasi.  
* **REQ-DOC-002: Validasi Format NPWP**  
  	Sistem *shall* memvalidasi nomor NPWP pembeli harus berupa 15 digit numerik (format lama) atau 16 digit numerik (format NPWP-NIK baru) yang lolos verifikasi algoritma *Luhn Checksum*.  
* **REQ-DOC-003: Pencocokan Data Lintas Dokumen**  
  	Sistem *shall* melakukan pencocokan keselarasan data (nama lengkap, tanggal lahir, dan alamat) antara e-KTP, Kartu Keluarga, dan NPWP pembeli secara otomatis untuk mencegah ketidaksesuaian administrasi pendaftaran.

  #### **3.2.2 Validasi Dokumen Transaksi Unit Baru**

* **REQ-DOC-004: Ekstraksi Data Faktur Diler**  
  	Sistem *shall* melakukan pemindaian OCR terhadap Faktur Pembelian diler untuk mengekstrak dan mencocokkan nomor rangka (*VIN*) dan nomor mesin secara digital.  
* **REQ-DOC-005: Validasi Dokumen SRUT**  
  	Sistem *shall* memvalidasi dokumen kelayakan SRUT diler untuk memastikan kendaraan baru telah mengantongi izin kelayakan jalan dari instansi terkait.  
* **REQ-DOC-006: Pemeriksaan Form A Unit CBU Impor**  
  	Sistem *shall* menandai kolom Form A sebagai wajib diunggah apabila asal kendaraan diidentifikasi sebagai unit impor utuh (CBU) untuk membuktikan pelunasan bea masuk.  
* **REQ-DOC-007: Pemeriksaan STCK Aktif**  
  	Sistem *shall* mencatat dokumen STCK diler yang diterbitkan kepolisian sebagai dokumen perjalanan sementara diler ke konsumen .

  #### **3.2.3 Validasi Dokumen Transaksi Unit Bekas**

* **REQ-DOC-008: Deteksi Hologram Tri Brata BPKB**  
  	Sistem *shall* memverifikasi keaslian dokumen BPKB yang diunggah diler lewat analisis gambar untuk mendeteksi pantulan hologram Tri Brata Polri di halaman awal.  
* **REQ-DOC-009: Deteksi Benang Pengaman UV BPKB**  
  	Sistem *shall* menyajikan panduan bagi diler untuk memindai halaman BPKB di bawah sinar ultraviolet untuk memverifikasi pendaran benang pengaman ultraviolet.  
* **REQ-DOC-010: Verifikasi Status Blokir STNK (SIGNAL)**  
  	Sistem *shall* melakukan panggilan API ke Samsat/SIGNAL menggunakan parameter Nomor Polisi (NRKB) dan 5 digit nomor rangka terakhir untuk mendeteksi apakah mobil bekas dalam status terblokir tilang elektronik (ETLE) atau mati pajak.  
* **REQ-DOC-011: Pemeriksaan Kuitansi Kosong Ber-Tanda Tangan**  
  	Sistem *shall* mewajibkan pengunggahan salinan foto kuitansi kosong minimal 2 rangkap yang telah dibubuhi tanda tangan pemilik pertama sesuai BPKB (rangkap 1 bermaterai Rp10.000) guna memfasilitasi balik nama BBN-II di kemudian hari.  
* **REQ-DOC-012: Pemeriksaan Surat Pelepasan Hak (SPH)**  
  	Sistem *shall* mewajibkan pengunggahan berkas SPH yang dilengkapi dengan stempel basah diler/perusahaan apabila mobil bekas teridentifikasi sebagai eks-aset operasional perusahaan.  
* **REQ-DOC-013: Pemeriksaan Dokumen Kredit Aktif**  
  	Sistem *shall* mewajibkan pengunggahan salinan kontrak kredit, fotokopi BPKB terlegalisir, dan histori pembayaran terakhir apabila mobil bekas yang ditransaksikan masih dalam status angsuran aktif.

  #### **3.2.4 Matriks Penegakan Validasi**

* **REQ-DOC-014: Penegakan Aturan Dokumen Transaksi**  
  	Sistem *shall* secara ketat memblokir tombol "Selesaikan Transaksi" jika dokumen yang diunggah tidak memenuhi matriks kelengkapan berikut:

| Jenis Transaksi | Dokumen Wajib Lulus Validasi |
| :---- | :---- |
| **Mobil Baru (Konvensional)** | KTP, Kartu Keluarga, NPWP, Faktur Kendaraan, SRUT, STCK . |
| **Mobil Baru (Impor CBU)** | KTP, Kartu Keluarga, NPWP, Faktur Kendaraan, SRUT, STCK, Form A. |
| **Mobil Bekas (Milik Pribadi)** | KTP Penjual, BPKB Asli, STNK Aktif, Faktur Pembelian, Kuitansi Bermaterai. |
| **Mobil Bekas (Eks-Aset PT)** | KTP Penjual, BPKB Asli, STNK Aktif, Faktur Pembelian, Kuitansi, SPH Perusahaan. |
| **Mobil Bekas (Kredit Aktif)** | Salinan Kontrak Kredit, Kopi BPKB Terlegalisir, Histori Pembayaran Terakhir. |

### **3.3 Kebutuhan Modul Fintech dan Leasing**

#### **3.3.1 Layanan Skor Kredit (OJK SLIK)**

* **REQ-FINT-001: Penarikan Dokumen iDeb SLIK OJK**  
  	Sistem *shall* melakukan penarikan dokumen Informasi Debitur (iDeb) melalui integrasi API SLIK OJK/PEFINDO secara otomatis setelah konsumen membubuhkan persetujuan digital.  
* **REQ-FINT-002: Penilaian Skor Kolektibilitas Otomatis**  
  	Sistem *shall* menganalisis riwayat kredit dari dokumen iDeb dan mengelompokkan kolektibilitas nasabah ke dalam kategori kelayakan kredit (Skala 1 s.d. 5\) untuk menentukan kelayakan pengajuan kredit leasing.  
* **REQ-FINT-003: Alternatif Skor Kredit PEFINDO**  
  	Sistem *shall* mengalihkan penarikan informasi kredit ke API PEFINDO apabila data iDeb SLIK OJK nasabah teridentifikasi kosong (*thin-file*).

  #### **3.3.2 Manajemen Pengajuan Pembiayaan**

* **REQ-FINT-004: Pengiriman Aplikasi Kredit via ESB Asynchronous**  
  	Sistem *shall* memproses pengiriman data pengajuan kredit konsumen ke diler-diler multifinance (seperti ACC atau TAFS) menggunakan arsitektur antrean pesan *Asynchronous* .  
* **REQ-FINT-005: Penanganan Callback Status Kredit**  
  	Sistem *shall* memproses panggilan *callback webhook* dari server leasing multifinance untuk mengubah status pengajuan kredit nasabah secara *real-time* di dasbor diler.

  #### **3.3.3 Layanan Hukum Jaminan Fidusia (Kemenkumham)**

* **REQ-FINT-006: Pembuatan Draf Akta Notaris Fidusia**  
  	Sistem *shall* mengekstrak data nomor rangka, nomor mesin, dan nilai penjaminan kredit secara otomatis ke dalam templat draf akta jaminan fidusia untuk dikirim ke notaris mitra diler.  
* **REQ-FINT-007: Pendaftaran Hak Fidusia Elektronik (SABH)**  
  	Sistem *shall* mendaftarkan hak jaminan fidusia secara elektronik ke server Ditjen AHU Kemenkumham melalui akun notaris mitra untuk mendapatkan nomor pengesahan.  
* **REQ-FINT-008: Pembatasan Batas Waktu Fidusia 30 Hari**  
  	Sistem *shall* secara mutlak melacak tanggal pembuatan akta notaris fidusia dan menerapkan pemblokiran pengiriman permohonan (*auto-blocking*) jika tanggal pendaftaran telah melebihi batas waktu **30 hari kalender** sejak akta ditandatangani sesuai PP No. 21 Tahun 2015\.  
* **REQ-FINT-009: Penyimpanan Sertifikat Fidusia Digital**  
  	Sistem *shall* mengunduh dan menyimpan Sertifikat Jaminan Fidusia Elektronik yang diterbitkan Ditjen AHU ke dalam *document vault* diler setelah pembayaran PNBP terverifikasi.  
* **REQ-FINT-010: Pengurusan Kode Billing PNBP Fidusia**  
  	Sistem *shall* memfasilitasi pembuatan dan sinkronisasi masa berlaku kode bayar billing PNBP pendaftaran fidusia diler ke *payment gateway* bank mitra.

### **3.4 Kebutuhan Modul Logistik dan PDI**

#### **3.4.1 Manajemen Logistik & STCK**

* **REQ-LOG-001: Pelacakan Unit Diler dari Pabrik**  
  	Sistem *shall* melacak posisi fisik unit diler (*shipment tracking*) dari pabrik perakitan APM hingga tiba di gudang penyimpanan diler.  
* **REQ-LOG-002: Pembuatan Dokumen Jalan STCK**  
  	Sistem *shall* mencatat tanggal penerbitan STCK dan pelat sementara (merah-putih) oleh kepolisian serta membatasi masa berlaku dokumen hanya selama **1 bulan** di dalam basis data.  
* **REQ-LOG-003: Penegakan Hukum Larangan Penggunaan STCK**  
  	Sistem *shall* memunculkan klausul pernyataan digital yang wajib disetujui konsumen yang menyatakan bahwa STCK dilarang keras dikendarai untuk keperluan pribadi di jalan raya umum serta dilarang dikendarai ke luar kota diler.

  #### **3.4.2 Pre-Delivery Inspection (PDI)**

* **REQ-LOG-004: Validasi Tahapan PDI \- Dekonservasi Wax**  
  	Sistem *shall* mewajibkan teknisi diler mencentang checklist PDI digital yang memverifikasi bahwa bodi mobil telah dibersihkan dari lapisan *wax* pelindung pabrik sebelum dikirim ke konsumen.  
* **REQ-LOG-005: Validasi Tahapan PDI \- Aktivasi Kelistrikan**  
  	Sistem *shall* memverifikasi bahwa sekring utama kelistrikan (*backup fuse*) telah dipasang kembali oleh teknisi diler dan mendokumentasikannya ke dalam laporan.  
* **REQ-LOG-006: Pembersihan DTC Eror pada ECU**  
  	Sistem *shall* mewajibkan teknisi diler menghubungkan pemindai OBD-II ke mobil untuk memindai sistem dan membersihkan seluruh kode eror (*Diagnostic Trouble Codes* / DTC) yang terekam di ECU selama perakitan pabrik.  
* **REQ-LOG-007: Pemeriksaan Kuantitas Cairan Mobil**  
  	Sistem *shall* memandu pemeriksaan dan pengisian cairan wajib mobil (oli mesin, minyak rem, cairan pendingin, minyak power steering, air wiper) melalui checklist PDI .  
* **REQ-LOG-008: Validasi Uji Jalan PDI Jarak Jauh**  
  	Sistem *shall* melacak rute uji jalan diler (*road test*) menggunakan sensor GPS dan memblokir penerbitan status "Lolos PDI" jika jarak uji jalan belum mencapai minimal **10 kilometer** .  
* **REQ-LOG-009: Penerbitan Lembar Pemeriksaan PDI Digital**  
  	Sistem *shall* mengekspor laporan PDI digital (berisi checklist, foto, data DTC, dan GPS uji jalan) ke dalam format PDF untuk ditandatangani oleh diler dan diserahkan kepada pembeli saat serah terima unit.

  #### **3.4.3 Penyerahan Unit & Garansi**

* **REQ-LOG-010: Pencegahan Pengiriman Unit Tanpa PDI**  
  	Sistem *shall* secara ketat memblokir status unit menjadi "Siap Kirim" jika status laporan PDI belum dinyatakan "Lolos/Passed" oleh teknisi dan supervisor diler.  
* **REQ-LOG-011: Penerbitan Berita Acara Serah Terima (BAST)**  
  	Sistem *shall* meng-generate dokumen BAST elektronik lengkap dengan tanda tangan digital konsumen setelah unit tiba di lokasi rumah konsumen.  
* **REQ-LOG-012: Aktivasi Garansi Mesin & Transmisi Otomatis**  
  	Sistem *shall* mendaftarkan nomor rangka mobil secara otomatis ke database diler purnajual untuk mengaktifkan garansi mesin dan transmisi selama 1 tahun terhitung sejak tanggal BAST ditandatangani.

### **3.5 Kebutuhan Integrasi API Eksternal**

* **REQ-INT-001: Integrasi API Dukcapil Kemendagri**  
  	Sistem *shall* menghubungkan modul registrasi ke API Dukcapil untuk memverifikasi kesesuaian data NIK e-KTP dan foto wajah (*face matching*) biometrik dengan tingkat kecocokan minimum tertentu.  
* **REQ-INT-002: Integrasi API Samsat / SIGNAL**  
  	Sistem *shall* menghubungkan modul validasi ke API Samsat/SIGNAL untuk melakukan pengecekan keaktifan STNK mobil bekas diler serta melacak status pemblokiran tilang elektronik (ETLE).  
* **REQ-INT-003: Integrasi API Layanan Kredit SLIK OJK**  
  	Sistem *shall* menghubungkan modul simulasi kredit ke API SLIK OJK / PEFINDO untuk memproses otorisasi penarikan skor kredit nasabah secara otomatis.  
* **REQ-INT-004: Integrasi API Ditjen AHU Kemenkumham**  
  	Sistem *shall* menghubungkan diler dengan portal pendaftaran fidusia online Ditjen AHU untuk mengirimkan draf akta dan memproses kode billing pembayaran PNBP pendaftaran.  
* **REQ-INT-005: Integrasi API Tanda Tangan Elektronik (TTE)**  
  	Sistem *shall* mengintegrasikan dokumen elektronik SPK diler dan draf kontrak kredit ke API penyedia TTE tersertifikasi Kominfo (seperti Privy atau VIDA) untuk penerbitan sertifikat tanda tangan digital yang sah secara hukum.

## 

## **BAB 4** **KEBUTUHAN NON-FUNGSIONAL & KEAMANAN**

### **4.1 Kebutuhan Performa (Performance Requirements)**

* **REQ-NF-001: Batas Waktu Respons Sistem Internal**  
  Sistem *shall* memproses seluruh request internal (kecuali operasi bulk) dengan batas waktu respons yang telah dikuantifikasi untuk menjamin pengalaman pengguna yang responsif.

| Metrik | Target Maksimal | Kondisi Pengujian |
| :---- | :---- | :---- |
| **p50 Response Time** | **\<=** 2 detik | Untuk seluruh endpoint API non-bulk pada beban normal. |
| **p99 Response Time** | **\=\<**  3,5 detik | Untuk seluruh endpoint API non-bulk pada beban puncak. |
| **p99.9 Response Time** | **12 \=\<** 5 detik | Batas waktu toleransi terlama sebelum timeout server. |

* **REQ-NF-002: Performa Pemrosesan Transaksi Leasing via ESB Asynchronous**  
  * Sistem *shall* memastikan bahwa seluruh pengajuan pembiayaan melalui integrasi ESB Asynchronous ke leasing partner diselesaikan dengan rata-rata kecepatan pemrosesan respons di angka **4,22 detik per request**.  
  * Arsitektur integrasi B2B leasing *shall* mengimplementasikan pola pesan asinkronus menggunakan Message Queue (RabbitMQ/Apache Kafka) untuk menjaga keandalan transaksi tanpa kehilangan data transaksi.  
* **REQ-NF-003: Kapasitas Beban Konkuren Sistem**  
  		Sistem *shall* mampu menangani beban operasional konkuren pada jam sibuk penjualan diler tanpa mengalami penurunan kualitas layanan yang signifikan dengan spesifikasi minimum:  
  * Mampu melayani hingga **10.000 pengguna aktif** secara bersamaan (*concurrent users*).  
  * Mampu memproses beban minimal **500 transaksi per detik** (*Transactions Per Second* / TPS).

### **4.2 Kebutuhan Keamanan & Perlindungan Data (Safety & Security)**

* **REQ-NF-004: Enkripsi Data Sensitif Pribadi (PII)**  
  	Sistem *shall* melindungi seluruh data pribadi sensitif pembeli (nomor NIK, NPWP, foto KTP, data iDeb kredit) menggunakan standar kriptografi tingkat tinggi:  
  * **Encryption at Rest:** Data yang tersimpan pada database dienkripsi menggunakan standar **AES-256**.  
  * **Encryption in Transit:** Pengiriman data antar-aplikasi dienkripsi wajib menggunakan protokol **TLS 1.3**.  
* **REQ-NF-005: Autentikasi dan Otorisasi Multi-Faktor (MFA)**  
  	Sistem *shall* membatasi akses dasbor diler menggunakan *Role-Based Access Control* (RBAC) yang ketat. Khusus untuk peran Admin diler, Surveyor leasing, dan System Admin, sistem *shall* mewajibkan proses verifikasi login tambahan melalui *Multi-Factor Authentication* (MFA) berupa pengiriman kode OTP 6 digit melalui WhatsApp Business API atau Email dengan masa berlaku OTP selama 5 menit.  
* **REQ-NF-006: Keabsahan Hukum Tanda Tangan Elektronik (TTE)**  
  	Sistem *shall* menjamin bahwa seluruh tanda tangan digital yang dilakukan pada Surat Pemesanan Kendaraan (SPK) diler atau kontrak perjanjian kredit pembiayaan divalidasi dan diterbitkan oleh penyedia TTE resmi tersertifikasi Kominfo (seperti Privy atau VIDA) untuk memastikan keabsahan hukum penuh sesuai UU ITE.  
* **REQ-NF-007: Deteksi Liveness Biometrik Identitas**  
  	Sistem *shall* menyertakan algoritma *liveness detection* (deteksi keaktifan objek wajah) saat pengambilan swafoto pembeli baru untuk menghindari penipuan identitas berbasis foto cetak atau video palsu, dengan syarat pencocokan foto wajah (*face matching*) ke API Dukcapil memiliki skor kecocokan biometrik minimal **85%**.

### **4.3 Kebutuhan Keandalan & Ketersediaan (Reliability & Availability)**

* **REQ-NF-008: Target Ketersediaan Sistem**  
  	Sistem *shall* beroperasi dengan target ketersediaan layanan (*uptime*) minimal **99,9%** selama 24 jam sehari, 7 hari seminggu (maksimal waktu tidak aktif/*downtime* toleransi adalah 43 menit per bulan).  
* **REQ-NF-009: Mekanisme Penanganan Downtime API Eksternal**  
  	Apabila API instansi pemerintah (Samsat/SIGNAL, Dukcapil, Ditjen AHU) mengalami gangguan (*downtime*), sistem *shall* tetap berjalan normal (*graceful degradation*) dengan menampilkan notifikasi ramah ke pengguna (seperti *"Sistem Samsat sedang sibuk, pendaftaran akan diproses otomatis saat online kembali"*) dan mengantrekan permohonan menggunakan pola pengiriman ulang otomatis berbasis *exponential backoff retry*.36

### **4.4 Kebutuhan Keterserapan & Antarmuka (Usability & Human Factors)**

* **REQ-NF-010: Responsivitas Mobile Aplikasi Inspeksi**  
  	Aplikasi inspeksi diler untuk pengisian checklist 150 s.d. 188 titik *shall* berjalan dengan lancar pada perangkat mobile berspesifikasi Android 10+ dan iOS 14+, dengan ukuran navigasi tombol yang ramah dioperasikan menggunakan satu tangan di lapangan.  
* **REQ-NF-011: Standar Aksesibilitas Web Portal Pembeli**  
  	Tampilan antarmuka web portal pembeli *shall* mematuhi kriteria aksesibilitas standar **WCAG 2.1 Level AA** untuk memfasilitasi kemudahan pembeli dengan keterbatasan fisik, termasuk kontras warna tinggi dan dukungan pembaca layar (*screen reader*).

### **4.5 Kebutuhan Portabilitas & Kompatibilitas (Portability & Compatibility)**

* **REQ-NF-012: Kompatibilitas Web Browser**  
  	Web portal pembeli *shall* kompatibel secara penuh dan bebas kesalahan tampilan pada seluruh browser modern minimal versi Google Chrome 90+, Apple Safari 14+, dan Mozilla Firefox 88+ baik pada sistem operasi desktop maupun perangkat mobile.

### **4.6 Ringkasan Kebutuhan Non-Fungsional**

| ID Requirements | Kategori Kualitas | Target Utama / Tolok Ukur Kuantitatif |
| :---- | :---- | :---- |
| **REQ-NF-001** | Performa | p50 Response Time ![][image11] 2 detik, p99.9 ![][image11] 5 detik. |
| **REQ-NF-002** | Performa | Respons transaksi leasing asynchronous diler-leasing rata-rata 4,22 detik.10 |
| **REQ-NF-003** | Performa | Kapasitas 10.000 *concurrent users*, throughput 500 TPS. |
| **REQ-NF-004** | Keamanan | Enkripsi data pribadi sensitif AES-256 (*at rest*) dan TLS 1.3 (*in transit*). |
| **REQ-NF-005** | Keamanan | Kontrol RBAC \+ login MFA (OTP WA/Email) bagi peran diler & leasing.12 |
| **REQ-NF-006** | Keamanan | TTE SPK diler & kontrak leasing wajib bersertifikasi Kominfo.14 |
| **REQ-NF-007** | Keamanan | *Liveness detection* swafoto \+ skor *face matching* Dukcapil minimal 85%.8 |
| **REQ-NF-008** | Keandalan | Tingkat ketersediaan layanan (*uptime*) minimal 99,9% dalam 24/7/365. |
| **REQ-NF-009** | Keandalan | Penanganan *downtime* API pihak ketiga lewat retry queue & notifikasi ramah.36 |
| **REQ-NF-010** | Usabilitas | Aplikasi inspeksi mobile lancar di Android 10+ & iOS 14+, ramah satu tangan.28 |
| **REQ-NF-011** | Usabilitas | Tampilan web portal pembeli wajib ramah aksesibilitas WCAG 2.1 Level AA. |
| **REQ-NF-012** | Portabilitas | Berjalan mulus di Chrome 90+, Safari 14+, Firefox 88+ desktop & mobile. |

## 

## **BAB 5** **VERIFIKASI & TRACEABILITY MATRIX**

### **5.1 Metode Verifikasi Kebutuhan**

Sesuai standar ISO/IEC/IEEE 29148:2018, setiap kebutuhan yang terdefinisi dalam dokumen ini wajib dipasangkan secara transparan dengan salah satu dari empat metode verifikasi pengujian berikut untuk menjamin kualitas pemenuhan fitur:

1. **Test (T):** Pembuktian pemenuhan kebutuhan secara dinamis dengan mengoperasikan sistem menggunakan perangkat pengujian khusus (misalnya pengujian beban stres, pengujian integrasi API, atau uji penetrasi keamanan).  
2. **Demonstration (D):** Pembuktian dengan menunjukkan pengoperasian fungsionalitas sistem secara visual di hadapan pengguna tanpa memerlukan alat ukur khusus (misalnya demonstrasi alur kerja PDI oleh teknisi).  
3. **Inspection (I):** Pembuktian statis melalui pemeriksaan visual langsung pada dokumentasi, arsitektur kode, konfigurasi server, atau skema basis data tanpa mengeksekusi sistem (misalnya memverifikasi skema enkripsi kolom tabel database).  
4. **Analysis (A):** Pembuktian melalui penalaran logis, pemodelan data, atau kalkulasi matematis berbasis data masukan untuk membuktikan kebenaran sistem (misalnya verifikasi logika perhitungan pajak pada *pricing engine*).

### **5.2 Matriks Ketertelusuran Kebutuhan (Requirements Traceability Matrix)**

Tabel RTM berikut memetakan keterkaitan fungsionalitas penting, use case, metode verifikasi, dan kriteria kelulusannya:

| Requirement ID | Nama Persyaratan | Use Case ID | Metode Verifikasi | Kriteria Kelulusan Pengujian (Acceptance Criteria) |
| :---- | :---- | :---- | :---- | :---- |
| **REQ-TAX-001** | Hitung DPP PPN Nilai Lain | UC-01 | Analysis | Perhitungan DPP PPN tepat senilai 11/12 \* tanpa selisih Rp1. |
| **REQ-TAX-011** | Cek Pajak Progresif | UC-01 | Test | Query NIK ke API Samsat mengembalikan data unit aktif dengan respons \< 5 detik. |
| **REQ-DOC-008** | Verifikasi Hologram BPKB | UC-06 | Test | Algoritma klasifikasi gambar di atas mendeteksi hologram Tri Brata dengan akurasi ≥ 95%. |
| **REQ-FINT-008** | Batas Waktu Jaminan Fidusia | UC-09 | Analysis | Sistem menolak pengiriman data ke Ditjen AHU jika tanggal akta notaris \> 30 hari. |
| **REQ-LOG-008** | Jarak Uji Jalan PDI | UC-10 | Test | Sensor GPS diler mencatat jarak tempuh uji jalan diler minimal tepat 10 kilometer. |
| **REQ-INT-001** | Face Matching Dukcapil | UC-04 | Test | Verifikasi identitas registrasi mengembalikan kecocokan biometrik wajah  ≥ 85%. |
| **REQ-NF-002** | Latensi Async ESB Leasing | UC-03 | Test | Waktu respons pengiriman melalui broker antrean pesan diler rata-rata 4,22 detik. |

### **5.3 CRUD Matrix (Entity-Requirements Mapping)**

CRUD Matrix ini memetakan entitas data diler utama terhadap operasi data masukan beserta aturan *Requirements* yang mengendalikannya:

| Entitas Data | Create (C) | Read (R) | Update (U) | Delete (D) |
| :---- | :---- | :---- | :---- | :---- |
| **Data Unit Mobil** | REQ-LOG-001 (Logistik) | REQ-TAX-016 (Breakdown OTR) | REQ-LOG-012 (Aktivasi Garansi) | Tidak Diizinkan (Hanya Soft-Delete) |
| **Berkas Jual-Beli** | REQ-DOC-014 (Upload Berkas) | REQ-DOC-003 (Pencocokan Admin) | REQ-INT-005 (TTE Dokumen SPK) | Tidak Diizinkan |
| **Aplikasi Kredit** | REQ-FINT-004 (Submission) | REQ-FINT-005 (Dasbor Surveyor) | REQ-FINT-005 (Webhook Callback) | Tidak Diizinkan |
| **Sertifikat Fidusia** | REQ-FINT-007 (Notaris SABH) | REQ-FINT-009 (Dasbor Admin) | REQ-FINT-008 (Monitoring Batas) | Tidak Diizinkan |

### **5.4 Kriteria Penerimaan Akhir (System Acceptance Criteria)**

Sistem dinyatakan memenuhi kriteria untuk rilis produksi apabila memenuhi kriteria absolut sebagai berikut:

1. **Kelulusan Fungsional:** 100% Kebutuhan Fungsional (Bab 3\) lulus pengujian verifikasi metode Test, Demonstration, atau Analysis tanpa adanya kesalahan logika bisnis diler.  
2. **Ketersediaan API:** Seluruh 5 API pihak ketiga terkoneksi dengan aman pada lingkungan produksi dengan penanganan kegagalan (*error handling*) dan downtime yang graceful sesuai aturan REQ-NF-009.  
3. **Kepatuhan Performa:** Hasil stress testing membuktikan throughput server mencapai minimal 500 TPS dengan latency p99 respons sistem internal ≤ 3,5 detik.  
4. **Keamanan Tanpa Cela:** Audit keamanan mandiri diler membuktikan bahwa enkripsi AES-256 pada database berjalan sukses dan tidak ditemukan kerentanan tingkat kritis (*zero critical vulnerability*) pada pengujian penetrasi sistem.

#### 

#### **Works cited**

Cara Menghitung Pajak Kendaraan dan Jenis Pajaknya, accessed June 5, 2026, [https://klikpajak.id/blog/cara-menghitung-pajak-kendaraan/](https://klikpajak.id/blog/cara-menghitung-pajak-kendaraan/)

PMK 131/2024: Tarif PPN Sebelas-Dua Belas | Direktorat Jenderal Pajak, accessed June 5, 2026, [https://www.pajak.go.id/en/node/113453](https://www.pajak.go.id/en/node/113453)

Pajak Tinggi Bikin Orang RI Mikir-mikir Beli Mobil Baru, accessed June 5, 2026, [https://oto.detik.com/mobil/d-8452175/pajak-tinggi-bikin-orang-ri-mikir-mikir-beli-mobil-baru](https://oto.detik.com/mobil/d-8452175/pajak-tinggi-bikin-orang-ri-mikir-mikir-beli-mobil-baru)

Pengertian Pajak Progresif, Cara Menghitung, dan Contohnya \- CIMB Niaga, accessed June 5, 2026, [https://www.cimbniaga.co.id/id/inspirasi/perencanaan/cara-menghitung-pajak-progresif-kendaraan](https://www.cimbniaga.co.id/id/inspirasi/perencanaan/cara-menghitung-pajak-progresif-kendaraan)

Pajak Progresif Mobil dan Cara Menghitungnya \- Wuling, accessed June 5, 2026, [https://wuling.id/id/blog/lifestyle/pajak-progresif-mobil-dan-cara-menghitungnya](https://wuling.id/id/blog/lifestyle/pajak-progresif-mobil-dan-cara-menghitungnya)

Tarif Pajak Progresif Kendaraan Bermotor Terlengkap 2026 \- Auto2000, accessed June 5, 2026, [https://auto2000.co.id/berita-dan-tips/tarif-pajak-progresif](https://auto2000.co.id/berita-dan-tips/tarif-pajak-progresif)

Berlaku Tahun 2025, Beli Mobil Bayar 7 Komponen Pajak, Ini ..., accessed June 5, 2026, [https://www.cnbcindonesia.com/news/20241228200525-4-599262/berlaku-tahun-2025-beli-mobil-bayar-7-komponen-pajak-ini-simulasinya](https://www.cnbcindonesia.com/news/20241228200525-4-599262/berlaku-tahun-2025-beli-mobil-bayar-7-komponen-pajak-ini-simulasinya)

Orico Balimor Finance, accessed June 5, 2026, [https://www.obf.id/artikel-detail-7-Cara%20menghitung%20pajak%20progresif%20mobil.html](https://www.obf.id/artikel-detail-7-Cara%20menghitung%20pajak%20progresif%20mobil.html)

Bimtek Implementasi Verifikasi Dan Validasi Data Via Dukcapil API @bimtekedukasi, accessed June 5, 2026, [https://www.bimtekedukasi.com/jadwal/bimtek-implementasi-verifikasi-dan-validasi-data-via-dukcapil-api-bimtekedukasi/](https://www.bimtekedukasi.com/jadwal/bimtek-implementasi-verifikasi-dan-validasi-data-via-dukcapil-api-bimtekedukasi/)

Yth. Direksi Perusahaan Pembiayaan di tempat. SALINAN SURAT EDARAN OTORITAS JASA KEUANGAN NOMOR 47 /SEOJK.05/2016 TENTANG BESA, accessed June 5, 2026, [https://www.ojk.go.id/id/kanal/iknb/regulasi/lembaga-pembiayaan/surat-edaran-ojk/Documents/SEOJK%20DP%20Pembiayan%20Konvensional.pdf](https://www.ojk.go.id/id/kanal/iknb/regulasi/lembaga-pembiayaan/surat-edaran-ojk/Documents/SEOJK%20DP%20Pembiayan%20Konvensional.pdf)

Bagaimana Ketentuan DP Nol Persen Kendaraan Bermotor? \- Indonesiabaik.id, accessed June 5, 2026, [https://indonesiabaik.id/infografis/bagaimana-ketentuan-dp-nol-persen-kendaraan-bermotor](https://indonesiabaik.id/infografis/bagaimana-ketentuan-dp-nol-persen-kendaraan-bermotor)

FAQ Menjual Mobil Instan di OLXmobbi \- Pusat Bantuan, accessed June 5, 2026, [https://help.olxmobbi.co.id/hc/id/articles/29925615109529-FAQ-Menjual-Mobil-Instan-di-OLXmobbi](https://help.olxmobbi.co.id/hc/id/articles/29925615109529-FAQ-Menjual-Mobil-Instan-di-OLXmobbi)

Tthe Benefits of SLIK OJK for Creditors & Customers \- Itsjack Blog, accessed June 5, 2026, [https://blog.itsjack.com/insight-en/slik-ojk-for-creditors-and-customers/](https://blog.itsjack.com/insight-en/slik-ojk-for-creditors-and-customers/)

Step by Step Jual Mobil Bekas Aman Tanpa Perlu Calo \- Caroline, accessed June 5, 2026, [https://www.caroline.id/blog/article/step-by-step-jual-mobil-bekas-aman-tanpa-perlu-calo](https://www.caroline.id/blog/article/step-by-step-jual-mobil-bekas-aman-tanpa-perlu-calo)

6 Dokumen Jual Beli Mobil Bekas yang Perlu Disiapkan \- IBID, accessed June 5, 2026, [https://blog.ibid.astra.co.id/otomotif/dokumen-wajib-jual-beli-mobil-bekas](https://blog.ibid.astra.co.id/otomotif/dokumen-wajib-jual-beli-mobil-bekas)

Alur Proses Kendaraan Baru \- Bapenda Jatim, accessed June 5, 2026, [https://bapenda.jatimprov.go.id/p/alur-proses-kendaraan-baru](https://bapenda.jatimprov.go.id/p/alur-proses-kendaraan-baru)

Pre-delivery Inspection (PDI) Checklist for New Cars \- ACKO Drive, accessed June 5, 2026, [https://ackodrive.com/car-guide/pre-delivery-inspection-checklist-for-new-cars/](https://ackodrive.com/car-guide/pre-delivery-inspection-checklist-for-new-cars/)

Perhatikan 5 Dokumen Penting Ini saat Membeli Mobil Bekas, accessed June 5, 2026, [https://swara.tunaiku.com/perhatikan-5-dokumen-penting-ini-saat-membeli-mobil-bekas/](https://swara.tunaiku.com/perhatikan-5-dokumen-penting-ini-saat-membeli-mobil-bekas/)

Inilah 8 Dokumen Penting yang Diperlukan saat Transaksi Jual Beli Mobil \- JualMobilmu.id, accessed June 5, 2026, [https://jualmobilmu.id/article/dokumen-jual-beli-mobil](https://jualmobilmu.id/article/dokumen-jual-beli-mobil)

Temporary Registration Certificate for New Cars and White Plate ..., accessed June 5, 2026, [https://daihatsu.co.id/en/tips-and-event/tips-sahabat/detail-content/stnk-sementara-mobil-baru-dan-aturan-plat-putih/](https://daihatsu.co.id/en/tips-and-event/tips-sahabat/detail-content/stnk-sementara-mobil-baru-dan-aturan-plat-putih/)

PDI (pre-delivery inspection) process explained \- The OpenRoad Blog, accessed June 5, 2026, [https://blog.openroadautogroup.com/pdi-pre-delivery-inspection-process-explained/](https://blog.openroadautogroup.com/pdi-pre-delivery-inspection-process-explained/)

New Car Pre-Delivery Checklist Guide | PDF | Car | Automotive Technologies \- Scribd, accessed June 5, 2026, [https://www.scribd.com/document/74105831/New-Car-Pre-Delivery-Inspection-Checklist](https://www.scribd.com/document/74105831/New-Car-Pre-Delivery-Inspection-Checklist)

Understanding SLIK OJK and How to Check SLIK OJK Online \- Indonesian Cloud, accessed June 5, 2026, [https://indonesiancloud.com/understanding-slik-ojk-and-how-to-check-slik-ojk-online/](https://indonesiancloud.com/understanding-slik-ojk-and-how-to-check-slik-ojk-online/)

SLIK OJK: The New Credit Information System Explained \- Indonesian Cloud, accessed June 5, 2026, [https://indonesiancloud.com/slik-ojk-what-you-need-to-know-about-the-new-credit-information-system/](https://indonesiancloud.com/slik-ojk-what-you-need-to-know-about-the-new-credit-information-system/)

LEGALITAS AKTA JAMINAN FIDUSIA KENDARAAN BERMOTOR BERDASARKAN PERJANJIAN POKOK SETELAH KREDITUR DINYATAKAN BERSALAH MELANGGAR UU, accessed June 5, 2026, [https://jurnal.mediaakademik.com/index.php/jma/article/download/781/726](https://jurnal.mediaakademik.com/index.php/jma/article/download/781/726)

Pelaksanaan Pendaftaran Jaminan Fidusia Secara Elektronik Oleh Notaris Berdasarkan Peraturan Pemerintah Nomor 21 Tahun 2015 \- FH Unram, accessed June 5, 2026, [https://fh.unram.ac.id/wp-content/uploads/2022/01/ALIFA-CIKAL-YUANITA-D1A018022.pdf](https://fh.unram.ac.id/wp-content/uploads/2022/01/ALIFA-CIKAL-YUANITA-D1A018022.pdf)

Ahu Fidusia: Aset Bisnis & Kunci Keamanan Transaksi Anda \- YAPLegal.id, accessed June 5, 2026, [https://yaplegal.id/blog/ahu-fidusia-aset-bisnis-kunci-keamanan-transaksi-anda](https://yaplegal.id/blog/ahu-fidusia-aset-bisnis-kunci-keamanan-transaksi-anda)

Prosedur Inspeksi Pra-Pengiriman Mobil | PDF \- Scribd, accessed June 5, 2026, [https://id.scribd.com/document/867598882/001733-PDI-Pre-delivery-Inspection](https://id.scribd.com/document/867598882/001733-PDI-Pre-delivery-Inspection)

Ranking Marketplace Mobil Bekas 2026 di Indonesia, Mana Paling Efektif? \- Caroline, accessed June 5, 2026, [https://www.caroline.id/blog/article/ranking-marketplace-mobil-bekas-2026-di-indonesia-mana-paling-efektif](https://www.caroline.id/blog/article/ranking-marketplace-mobil-bekas-2026-di-indonesia-mana-paling-efektif)

Pre-Delivery Order Inspection, Cermat Terima Mobil Baru, accessed June 5, 2026, [https://hyundaimobil.co.id/news/details/pre-delivery-order-inspection-cermat-terima-mobil-baru](https://hyundaimobil.co.id/news/details/pre-delivery-order-inspection-cermat-terima-mobil-baru)

PDI \- Pre-Delivery Inspection \- VentaVid, accessed June 5, 2026, [https://www.ventavid.com/glossary/pdi-pre-delivery-inspection](https://www.ventavid.com/glossary/pdi-pre-delivery-inspection)

Permudah Transaksi Mobil Bekas, OLXmobbi Hadirkan Layanan Menyeluruh dan Terintegrasi \- Investortrust.id, accessed June 5, 2026, [https://investortrust.id/lifestyle/26916/permudah-transaksi-mobil-bekas-olxmobbi-hadirkan-layanan-menyeluruh-dan-terintegrasi](https://investortrust.id/lifestyle/26916/permudah-transaksi-mobil-bekas-olxmobbi-hadirkan-layanan-menyeluruh-dan-terintegrasi)

Hak Akses Hanya untuk Verifikasi Data Kependudukan Bukan Memberikan Data Penduduk, accessed June 5, 2026, [https://dukcapil.gunungkidulkab.go.id/2020/06/20/hak-akses-hanya-untuk-verifikasi-data-kependudukan-bukan-memberikan-data-penduduk/](https://dukcapil.gunungkidulkab.go.id/2020/06/20/hak-akses-hanya-untuk-verifikasi-data-kependudukan-bukan-memberikan-data-penduduk/)

Buku Petunjuk Pendaftaran Jaminan Fidusia Online \- Ditjen AHU, accessed June 5, 2026, [https://portal.ahu.go.id/uploads/45226\_PanduanFidusiaOnlineVer1.3.pdf](https://portal.ahu.go.id/uploads/45226_PanduanFidusiaOnlineVer1.3.pdf)

Press Release: OJK Launches Financial Information Services System (SLIK) to Enhance Debtor Information System, accessed June 5, 2026, [https://ojk.go.id/en/berita-dan-kegiatan/siaran-pers/Pages/Press-Release-OJK-Launches-Financial-Information-Services-System-(SLIK)-to-Enhance-Debtor-Information-System.aspx](https://ojk.go.id/en/berita-dan-kegiatan/siaran-pers/Pages/Press-Release-OJK-Launches-Financial-Information-Services-System-\(SLIK\)-to-Enhance-Debtor-Information-System.aspx)

OJK Izinkan DP 0 Persen, Ini Pilihan Mobil Listrik yang Bisa Dibeli \- Kompas Otomotif, accessed June 5, 2026, [https://otomotif.kompas.com/read/2023/01/06/064200515/ojk-izinkan-dp-0-persen-ini-pilihan-mobil-listrik-yang-bisa-dibeli](https://otomotif.kompas.com/read/2023/01/06/064200515/ojk-izinkan-dp-0-persen-ini-pilihan-mobil-listrik-yang-bisa-dibeli)

Penerapan Jaminan Fidusia dalam Leasing Kendaraan Bermotor : Analisis Terhadap Penyalagunaan dan Perlindungan Konsumen \- Jurnal yayasan Daarul Huda Kruengmane, accessed June 5, 2026, [https://ojs.daarulhuda.or.id/index.php/Socius/article/download/1335/1455](https://ojs.daarulhuda.or.id/index.php/Socius/article/download/1335/1455)

Fidusia \- Kemenkum Jatim, accessed June 5, 2026, [https://jatim.kemenkum.go.id/layanan-2/standar-layanan/adm-hukum-umum-2/fidusia](https://jatim.kemenkum.go.id/layanan-2/standar-layanan/adm-hukum-umum-2/fidusia)

[image1]: <data:image/png;base64,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>

[image2]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAJYAAAAoCAYAAAAcwQPnAAABq0lEQVR4Xu3WTysFURjH8SN2FrKjFF4BJUu5G3tF2dooG6WESFLeBStbC/IO5D2IpXeglC2e0zzTPU6NO/c48+/0/dTTnHnm3nPvnfndOWNMTCN+AwDql9KtKKXf0lFcAgBATKwrqdn3G//RRDya+EwMduw3gBgIFipBsFojrUU97WBVea2qnLvD5qQOpd69fuO4XhEEnsRnqRepntSdyQJyrccepFal9qTGpZ6k3vRYWTtSYzo+N9mc91JfUq/5i1CrwKgM58bbn3bGdmn71t6j1InUrnO8iPvF3fmWTH+5/HTGCFdLSEK4wdoyWRB6un8pdSC1IDWrvTxYV1pFJnRr57vVsQ3WmY5tsOwdrFGtvSqtEX6GsmD13+/fsfzehTPOrTuVO9KtfW8eQBusUx3bYC3rGAmywZpy9gcF68MZlzGjW/tMNW+yOddMFqz87gUEG/UbwCCLut341QUimfQbfyv/YFj+lSivO2d1xW8AMWz7DSCGTb+BpHRn7QQAAK3GQwXQfh34n3bgKyIyrnkAThqAFHFvAwCgAIskhkBcUAY5AdqN/yhQoR/Zxij+Ai/36AAAAABJRU5ErkJggg==>

[image3]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABMAAAAaCAYAAABVX2cEAAABhklEQVR4Xq2Uv0oEQQzGE/zDwVnINXJo4wOIpQoKVnYLoqW1cKDW1whiZykHPoC1XCG+iJ2Po18ymdnM7B8O3A9+O9lJ8l3mll0iERtBI3AJto2oNXCsMB1Ig7bkvdnGBFTgA0wFa1jH5QzRwpCaDg1llo+qly3wjngq+BMgngvUasalmSozS7tBYjRHbeUbcrnJOJmFY+ZF3sxtx4lK/z4zjsc0s+iRTAY1KyRP8wbJb6wPxi7A06Rz9HwKiN/AUd0m4k7TJJ/vGYJqs7LKbaawS753UDPTdYCX2F/SSmjtY5vZToRlZYlZ0XuX8/eKN8tN9VgjXNq+GifgSWG6Q+24cfrhzepRJ1x8Naxon+T/IdpQmO6xvrL9epqoZbLsDbAfk/hFcppnOsX9Fxj3mKk6PkFmG3jGMmv0Njb+Y5ZL03IMmJUvumYrAcFtLM6Omcv+s9IsDHWI5UIIO3yFdTO11nUx0KfZ/Gow7WH9Qe5XoMCiMVBhtpp8re8d1KxVvUnqyDP9AWsLOUfQKdz9AAAAAElFTkSuQmCC>

[image4]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAOUAAAAdCAYAAABVCKtiAAACPklEQVR4Xu3Yu2tUQRTH8REVFARFxQcIooi2EUEQFFKIna2NYGEjiIVgIQRSiNgHCxtB/wCxsrPR2jqFrSDY2aUNzs+c4z337CObfeWa/X7gMI87d+/s7pyZTUqZl325I9t2wA5M87UAYB7Yt9BZLE4AwF8cCADwX+jYdt2x6aArFmFhLMJ7xGx1YA0dq3G2xn4rFYdqnK5xMLRH9b7G8RoP8oXAn6nXF3+uPg6VJ6zf2wofd8rqmp/37cSKlW9bvW3xeV5qznq2Ss0vzguYiVdWHqlx0+ob1h7mo5WHa/yucdfi6r8R/T2vccnqP0K/nnmgNImptqjt467UeGP1YfNbKs2m8t36fLyeMYgSVxvCNWv7HORZ6rvgF4BpU5KIFq0vxuvWHsbvO1PjV40XpTnF3OvUFr/vUWmSTYmixa6E8MUeF7+Pu1G2ku1l2X5+fpp9tVIbwUOrR+dDfbnGt9KblH6yxz5tRsBM5UX+LrUzT66joW+txsXS/tn7OdTF77tfek/KKLbjOCXbKJtGTsp+Lqf2ctl6PzkpZT31PfELwKzkRf4ptTNPrtXS/AQWJcNJq/tPvkj33bb6OEl5zso83ywnZb+TTQn4NLTvWKmk3KzxM1zz9+vzuuUXgK66Z+WHVm93PLbyS6t3MP0DDPOkP14AAFgsnH4AsCDY8AEAmAhHacCHAexlZDiAiQzaRAb1Y0/ja0cXsA4BAOjRieNx3EmMex8AYHzsvZPaxU9wFx+NhO8CADBFox8rfwARsjvE2ex8zgAAAABJRU5ErkJggg==>

[image5]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAA4AAAAWCAYAAADwza0nAAABJElEQVR4Xp1SMW4CQQy0hZASRUqfF6A0tPRp09BEIh0v4AH8A4orU0SpaPO9jMf2npdLtZZm1zs7YxtuRQVhC6AaCeGRmTZ+Zlqkj3slNWy2x3ncWDU0ZVK54Fsz8iorsHsDuBcXduVS13VP+plQOQMXEJt2E/plLFh9AndC8hPY2i+zEiwy60aN1CbjdFw+EipHMDfsu6Z0ba1x392C2jXWT+y/BuSvZQBO06QlTLIDd0N+DNgkw0a/64yqK6jfDeC+wexBr+8ejT8jApcgDlH9LWAPhG37BmT0wyFfAP+9lNVJ+hg1xkd5CPgwzTbbU9kVXIZ1CcRxTqyAl7fl4JAJ3SZwEy4dkZNTuRrEHkAplItTpem/QzFGjc3Hg2e9yZOq+wNpehrLhQgaBwAAAABJRU5ErkJggg==>

[image6]: <data:image/png;base64,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>

[image7]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAALoAAAAfCAYAAABQ39esAAACB0lEQVR4Xu3YPWsUURTG8SMqKETEFzBRQQQLrQURrLUTgrEwkMJWGyu1FhUrO7UQGz+C+AWsLCQIgdhYSvpU6T0P917m7OzOkHU2u87u/weH+zK7dyazZ87erNlMHapPoAV3CwAAYCxsnwAAAOYQm7xiqT4BTMumx/v65AhPPR7k/kOPXx7XPO5b86Os+ZMeH/L4nMd2dXjAmsdjjyOWrueZx11L6y/n13yxdN7ieehfCH1gyKn6RIOV3Cr5VJmPhmOaG2XLUpIet5T0JzwuDrzC7HRtrLW13o0w9y23Sux4rpjowL5c8TjscdnjiaVEPm9VtVaiKzRezXPysdZXqCrLnqWKfjOPX1k6R5NLuV0Pc1qvXMNLj98ejzxeWEp0rX0rH7+aW/xPmr7vx9dppWO1sZL0u68ZE1hKRRdV5tth/C70ox2Pz7l/Pbevcyuq+KPc8/gaxru5LRX8j6WHSElf6JsgbmswPZ0ScFreWFVJRQl5x4YvPia6kkqJVmyEfqT9v5JWD4/e89b2t93Q9uRTGP/IbXmvHoKfeXzG0t5f4t8BDClV/aylpNT2YpK/kOifVtHDo23HpLVthwbVH18snPLLCDBLlKJ/U9037mAv8DFhXpHbANAz3Qt39xWAnmpP/vajAAAAk8XeAwuBRO8RPqyDxf0FMBaKBgAcGEosFhjpDyyYv8klLiG5sEMNAAAAAElFTkSuQmCC>

[image8]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAKoAAAAgCAYAAACCXeM8AAACJklEQVR4Xu2XvUsdQRTFbxCLoIWkSAgEFIlFCgOSwj5JI2KlXezzB1goavFIKrukiIFoIhYWaiEiFqay18JCexW1EbukUTQ5J3NX5y3PCL4Vdt3zg8PcN3fWnXXOfJkVggfpivtLiT61dGhs84XGQ4giopkrhBBCCCFEJrSkK4T4H4+gQagJmoMOoGHoIdQN9UBL0AU05c/sQKMek90o7o/iNN+gRmgeaoe+VKcv4bs6PWabPQvtX0ADpvtjfrnjkWmGZi0YtA96VZ3+x+8oZtuYrdTv66DhaNJ30FNoqDptT7x8GdWxP+vQRKpOlJAZLzehHxaM+tHrxrykUY88plE7LJhtzYJRabIGzy97yRWUSqBRyQL03cIESYjnIuufe5wY9bH/5nOipNAYvRZW1M8WjHrmudde0qg8JhAalVtwxeP0isrtvRaJUTe8ZLtnHle8JOzPisc0Kt/96Sp9+cwtuOO9SdTHDcNDY5xYMCpXSRq1K8rz8Vpb/7TH21GO8GxLrltRE/i+ZOVujerZn6/QiF2tqD+j/GoUi5LQZuECxcvUL2gfOvXce2gSOof+QMden+RpYF6qePlZhMahDxba1oL1PGawfGthQryJ8ry4kUMvuYpy1eX7eQRhnv27Yd6J/KEhM/0ThBBCCCGyp4gnrCL2WYickO30yfavCSGEEPlA+9v9R2MshKgbLSRCCCEypvhbS/G/oC5K/vlCCCGEEEJkwV+By1BdDKNxlgAAAABJRU5ErkJggg==>

[image9]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAANgAAAAeCAYAAABOkdBEAAAC1ElEQVR4Xu2YOagUQRCGSzxQFLweimCwHgiiIGLwMDEQDQQvVFAw00ADQTwQxOiJ4JF5gwiCsZmYCW5uoJm5qImgYGAmWL9dxdaO6+7bZWe2Z/w/+Knqmpma3umu6Z4VaQxzigFCyDhhieVGGBEOTj5wLAghhJD6wnWckOpgvdWZGo9ejbtOCBHZpJpfDP4DnLdGtcQEH7GVdnyValmweD3gHLSH4b7ZtV3RbjzvXPNxPwigPwssjv4tNB8C3i/vN5jtM2gu5b7My82eKYskTazjxQMd/nouKKytoX3O7NUQA5fMHpAeSQosVe1THVUtlnSPQcS8KJbV5j8z6/GN5rdDHOD6eYUYaQKDZluFPDWLLu1S7SnEn6tmVOetDTD5Y0H6hH4dYnE1wERGAfUD54BtZqdVp8wH/shinpgXhYRrwBOzHvdz2iEOYoE+MrtCUtHdsjY4FnxChiIWhXNP0tbLJ+aOcAygwA6F9l2zmKx7zd3d8f9MZEzcQZyRNLmnrI3t4YPOYbkcfBDzxgIDZ80+VL00vy3d77bbknKcsDZ+103p/O6Dqp3mT4SMXsRkRHwCrQ+xV6oNqnXW7lVgEf+uAd97+L46OVhhoOLK4KsmVrEt5sd7Lw8+iHljH8AHs4g/Nr9tdrNZv/6iWWwxr5j1eHwulcMCawbvVS/MvyFpe3fS2qdV71RHVPtVLdUn1Q9J30wfTXck/cGAbzqsRNhyvbHYN9VX6Q9WEdz3mrV9q4pcwP+82G4WeF4c+yKpTxck9RFzE/atpNyHVb8k9R3gT4/P5gNsc1uSVt2WXu3b3uuqn34ScSor/cpu1J9MukEIIY2G79oM4CAQQrJkrC+nsSYjWYAxre+41rfnhBBCCCGzZZQdzyjXDKCElGSscIRIrnBuDg0fWaZMZmAmc9fmwOf3H1Pe4JeXmRBCSPPgqkEIIYSQCTDSFmSkiwghhJCS+Q2Z8kNmBmEMhgAAAABJRU5ErkJggg==>

[image10]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAIEAAAApCAYAAAAS70rTAAAB4UlEQVR4Xu3YvUrEQBQF4CsqWFiIgouVVoI/naCNhQh22vgU9hY+gW8gWG8pCBaWNqkEsbGzUURQLKztvce5l53NiuySrJtMzgeXScawbjInmdmIEBERFTWW7/g/I/zXQ5PiOdGIMVRE/VnQmrLWy/v9PjqwlhK2rfWqNauVWd+G1qVtt62lhOGuRwggsxYhOLHtNifX9F1IJwSf1iIEbxJGH0+CSa1lrUf7O1XfwHcuBhwyrZaEEDifDjKtpU43pQZPgmsJA427/rcQHEmhJ8HAwaQCMIjxaj9e9Q9qLt9B9fClNS3hrvafdwjBhB9A6duU30MAi9ZS1ZUwe3oIbm3fX/5grt+1vrMCRTXgITi0fYRgXbrztVWgqAYQAqzkMR2sSQgBFnlY4d9Hx1HixvMdOZgarnJ9x1orWqdR37m1fH2coD3pfh8AD7l98GOGEoIS1j9UEAb4WcK7hXmtd+t/0rrT2rdjWhJCgCnFXzHTX2qUbgzwarQfD/CLhLUEjrnR+rB+hqBXjYa8FwYYU8CO7fsA46TwhPAQQNvOlCFIUD8pxlThZqJtKq6f60+l+LnUZVzvMj6DiIiIiIgoKfyhVCMcLKJ6qf49i29Y/W9Jo8N0EFFDNPpx1+iTJ6JG+AbowD/zJ1C+lgAAAABJRU5ErkJggg==>

[image11]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAbCAYAAAB1NA+iAAABDElEQVR4Xq1QOwrCQBCdAQtFsEllaRXwJrZWlh5AvIGVjWBppY3gSZ39zc4nhhDz4GX3zefNbAACMLMeGVp57UK+IEbsANZoW0YYcFQWDMaADSxcvQv06zV9dkLbEtdg9UCDcNREaLon4pXOVc3bARVNIubGokVfuZgNGtJhyi0RY5OD3PBvA8KMeMl8QHozw77SDORzEwn4JHWi+zJSlJRCPlRiAgMZaOn+jgxmAAvOgDV0sGls6RPMjpHFjAeWerdSAa+8jQT8kNr7ehcoGGDAm3QaZOiV5y7XBztA1yc1o/McCfgiLdipD6V9MgMP/eZ+6DeKh/7QXf9AFHjoAV7X6FgD26e16LKFGV8f4B+xHxMIbAAAAABJRU5ErkJggg==>