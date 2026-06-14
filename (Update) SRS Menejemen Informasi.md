**Universitas Tidar** 

June 2026 

# **Spesifikasi Kebutuhan Perangkat Lunak (SRS): Sistem Transaksi Pembelian Kendaraan Bermotor Terintegrasi (Indonesia)** 

Kelompok 4 Universitas Tidar 

**Universitas Tidar** 

June 2026 

## **About Our Team** 

Fadlika Alfi Nurrochmah (2410506010) Muhammad Ramdhan (2420506027) Assep Wahid (2420506028) Ilyasa Abiyyu Wicaksono (2420506035) Farrel Apriandry Ciu (2430506056) 

**Universitas Tidar** 

June 2026 

## **Latar Belakang** 

Proses pembelian mobil di Indonesia masih terfragmentasi kalkulasi pajak, verifikasi dokumen, kredit, hingga PDI dilakukan manual dan terpisah-pisah. 

Risiko kesalahan administrasi tinggi, terutama untuk hal yang terikat batas waktu hukum (contoh: fidusia maksimal 30 hari). 

Belum ada integrasi real-time antara diler dengan instansi pemerintah (Dukcapil, Samsat, SLI ~~K OJK, Ditjen AHU).~~ **Tujuan** 

kontrak formal antara developer dan stakeholder, acuan QA, dan pedoman teknis arsitek/programmer mencakup alur dari estimasi harga online hingga serah terima unit fisik. 

## **Batasan Sistem** 

Batasan geografis: hanya untuk wilayah hukum Republik Indonesia. Batasan mata uang: seluruh kalkulasi keuangan dalam Rupiah (IDR), presisi 2 desimal. SLA API pihak ketiga: kecepatan verifikasi bergantung pada server instansi pemerintah (SIGNAL, Dukcapil, Ditjen AHU). 

**Asumsi & Ketergantungan** 

API Dukcapil dapat diakses real-time untuk face matching SLIK/PEFINDO memberikan status kolektibilitas akurat Pendaftaran fidusia didukung notaris berlisensi aktif di SABH Ditjen AHU. 

**Universitas Tidar** 

June 2026 

## **Ruang Lingkup** 

**3 skenario transaksi utama yang dicakup:** 

## **Kendaraan Baru** 

Kalkulasi harga OTR real-time 

Validasi dokumen diler (Faktur, SRUT, Form A untuk unit CBU/impor) Integrasi TTE untuk tanda tangan SPK. 

## **Kendaraan Bekas** 

inspeksi fisik digital 150–188 titik pemeriksaan 

verifikasi keaslian dokumen (STNK aktif, BPKB hologram & UV, kuitansi bermaterai, SPH untuk eks-aset perusahaan) 

pencairan dana instan ke penjual maksimal 60 menit. 

## **Modul Financing** 

Simulasi kredit dinamis (DP berbasis rasio NPF leasing) IIntegrasi otomatis ke SLIK OJK/PEFINDO 

Pendaftaran jaminan fidusia elektronik ke Ditjen AHU. 

**Universitas Tidar** 

June 2016 

## **Perspektif Produk dan Context Diagram** 

Sistem adalah platform omnichannel terintegrasi (O2O) yang menjadi penghubung antara dealer management system internal diler dengan jaringan pembiayaan pihak ketiga dan instansi pemerintah, secara real-time. 

**Universitas Tidar** 

June 2026 

## **Karakteristik Pengguna (User Classes)** 

|**Peran**|**Deskripsi**|**Hak Akses Utama**|
|---|---|---|
|Pembeli Personal|Konsumen perorangan pembeli/penjual kendaraan|Isi profil, simulasi, unggah KTP/KK, TTE|
|Sales Dealer|Staf pemasaran diler|Buat draf SPK, konfigurasi unit, pantau status kredit|
|Admin Dealer|Staf backoffice administrasi diler|Verifikasi dokumen, daftar ke Samsat, alokasi PNBP|
|Inspektor Lapangan|Teknisi penilai kondisi fisik mobil bekas|Checklist inspeksi mobile, foto/video 360°|
|Surveyor Leasing|Analis risiko perusahaan pembiayaan|Review aplikasi kredit, akses skor iDeb, approval limit|
|Teknisi PDI|Teknisi penyiap unit baru|Checklist PDI, daftar nomor rangka ke modul garansi|



|||**Universitas**||**Tidar**|**Tidar**|June 2026|||||||||||||||
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|||||||**Data Dictionary**|||||||||||||||
|||Entitas : Pembeli|||||||||||||||||||
|||Atribut Data|||**Tipe Spesifikasi**|Atribut Data|||**Tipe Spesifikasi**|||||Atribut Data|||**Tipe Spesifikasi**||||
|||id|||UUID|role|||ENUM|||||updated_at|||TIMESTAMP||||
|||nik|||VARCHAR(16)|password_hash|||VARCHAR(255)|||||deleted_at|||TIMESTAMP||||
|||nama_lengkap|||VARCHAR(255)|foto_ktp_path|||VARCHAR(512)||||||||||||
|||tanggal_lahir|||DATE|foto_selfie_path|||VARCHAR(512)||||||||||||
|||alamat|||TEXT|face_match_score|||DECIMAL(5,2)||||||||||||
|||nomor_hp|||VARCHAR(15)|is_verified|||BOOLEAN||||||||||||
|||email|||VARCHAR(255)|created_at|||TIMESTAMP||||||||||||
||||||||||||||||||||||



**Universitas Tidar** 

June 2026 

## **Data Dictionary** 

Entitas : Kendaraan 

|Atribut Data|**Tipe Spesifikasi**|||Atribut Data|**Tipe Spesifikasi**|||Atribut Data|**Tipe Spesifikasi**|
|---|---|---|---|---|---|---|---|---|---|
|id|UUID|||model|VARCHAR(100)|||tkdn_persen|DECIMAL(5, 2)|
|nomor_rangka|VARCHAR(17)|||tahun|INTEGER|||is_kblbb|BOOLEAN|
|nomor_mesin|VARCHAR(20)|||warna|VARCHAR(50)|||pembeli_id|UUID|
|nomor_polisi|VARCHAR(12)|||harga_dasar|DECIMAL(15,2)|||status|ENUM|
|jenis|ENUM|||njkb|DECIMAL(15,2)|||created_at|TIMESTAMP|
|tipe|ENUM|||tarif_pkb|DECIMAL(5,4)|||updated_at|TIMESTAMP|
|merk|VARCHAR(100)|||koefisian_road|DECIMAL(3, 2)|||deleted_at|TIMESTAMP|



**Universitas Tidar** June 2026 

|Atribut Data<br>**Tipe Spesifikasi**<br>id<br>UUID<br>pembeli_id<br>UUID<br>kendaraan_id<br>UUID<br>jenis<br>ENUM<br>file_path<br>VARCHAR(512)<br>file_hash<br>VARCHAR(64)<br>mime_type<br>VARCHAR(100)<br>Entitas : Dokumen Legalitas|Atribut Data<br>**Tipe Spesifikasi**<br>id<br>UUID<br>pembeli_id<br>UUID<br>kendaraan_id<br>UUID<br>jenis<br>ENUM<br>file_path<br>VARCHAR(512)<br>file_hash<br>VARCHAR(64)<br>mime_type<br>VARCHAR(100)<br>Entitas : Dokumen Legalitas|**Data Dictionary**<br>Atribut Data<br>**Tipe Spesifikasi**<br>file_size<br>INTEGER<br>is_verified<br>BOOLEAN<br>verification_notes<br>TEXT<br>verified_by<br>UUID<br>uploaded_at<br>TIMESTAMP<br>verified_at<br>TIMESTAMP<br>expires_at<br>TIMESTAMP|**Data Dictionary**<br>Atribut Data<br>**Tipe Spesifikasi**<br>file_size<br>INTEGER<br>is_verified<br>BOOLEAN<br>verification_notes<br>TEXT<br>verified_by<br>UUID<br>uploaded_at<br>TIMESTAMP<br>verified_at<br>TIMESTAMP<br>expires_at<br>TIMESTAMP|**Data Dictionary**<br>Atribut Data<br>**Tipe Spesifikasi**<br>file_size<br>INTEGER<br>is_verified<br>BOOLEAN<br>verification_notes<br>TEXT<br>verified_by<br>UUID<br>uploaded_at<br>TIMESTAMP<br>verified_at<br>TIMESTAMP<br>expires_at<br>TIMESTAMP|||||
|---|---|---|---|---|---|---|---|---|
|Atribut Data|**Tipe Spesifikasi**||Atribut Data|**Tipe Spesifikasi**||Atribut Data|**Tipe Spesifikasi**||
|id|UUID||file_size|INTEGER||deleted_at|TIMESTAMP||
|pembeli_id|UUID||is_verified|BOOLEAN|||||
|kendaraan_id|UUID||verification_notes|TEXT|||||
|jenis|ENUM||verified_by|UUID|||||
|file_path|VARCHAR(512)||uploaded_at|TIMESTAMP|||||
|file_hash|VARCHAR(64)||verified_at|TIMESTAMP|||||
|mime_type|VARCHAR(100)||expires_at|TIMESTAMP|||||



**Universitas Tidar** 

June 2026 

Entitas : AplikasiKredit 

## **Data Dictionary** 

||Atribut Data|**Tipe Spesifikasi**|||Atribut Data|**Tipe Spesifikasi**|||Atribut Data|**Tipe Spesifikasi**|
|---|---|---|---|---|---|---|---|---|---|---|
||id|UUID|||bunga_tahunan|DECIMAL(5,4)|||ideb_data|JSONB|
||submission_id|VARCHAR(50)|||angsuran_bulanan|DECIMAL(15,2)|||credit_score|INTEGER|
||pembeli_id|UUID|||total_bunga|DECIMAL(15,2)|||created_at|TIMESTAMP|
||kendaraan_id|UUID|||total_pembayaran|DECIMAL(15,2)|||approved_at|TIMESTAMP|
||jumlah_pinjaman|DECIMAL(15,2)|||leasing_id|UUID|||rejected_at|TIMESTAMP|
||dp_amount|DECIMAL(15,2)|||notaris_id|UUID|||documented_at|TIMESTAMP|
||dp_persen|DECIMAL(5,2)|||status|ENUM|||fidusia_complete_at|TIMESTAMP|
||tenor_bulan|INTEGER|||rejection_reason|TEXT|||deleted_at|TIMESTAMP|



**Universitas Tidar** June 2026 

|Atribut Data<br>**Tipe Spesifikasi**<br>id<br>UUID<br>aplikasi_kredit_id<br>UUID<br>notaris_id<br>UUID<br>nomor_sertifikat<br>VARCHAR(50)<br>nomor_akta<br>VARCHAR(50)<br>tanggal_akta<br>DATE<br>batas_pendaftaran<br>DATE<br>Entitas : SertifikatFidusa|Atribut Data<br>**Tipe Spesifikasi**<br>id<br>UUID<br>aplikasi_kredit_id<br>UUID<br>notaris_id<br>UUID<br>nomor_sertifikat<br>VARCHAR(50)<br>nomor_akta<br>VARCHAR(50)<br>tanggal_akta<br>DATE<br>batas_pendaftaran<br>DATE<br>Entitas : SertifikatFidusa|**Data Dictionary**<br>Atribut Data<br>**Tipe Spesifikasi**<br>nilai_penjaminan<br>DECIMAL(15,2)<br>is_online_registered<br>BOOLEAN<br>tanggal_pendaftaran_ahu<br>DATE<br>kode_billing<br>VARCHAR(50)<br>jumlah_pnbp<br>DECIMAL(15,2)<br>status_bayar<br>ENUM<br>status<br>ENUM||||
|---|---|---|---|---|---|
|Atribut Data|**Tipe Spesifikasi**|||Atribut Data|**Tipe Spesifikasi**|
|id|UUID|||sertifikat_file_path|VARCHAR(512)|
|aplikasi_kredit_id|UUID|||created_at|TIMESTAMP|
|notaris_id|UUID|||updated_at|TIMESTAMP|
|nomor_sertifikat|VARCHAR(50)|||deleted_at|TIMESTAMP|
|nomor_akta|VARCHAR(50)|||||
|tanggal_akta|DATE|||||
|batas_pendaftaran|DATE|||||



**Universitas Tidar** 

June 2026 

## Entitas : Notaris 

## **Data Dictionary** 

|Atribut Data|**Tipe Spesifikasi**|||Atribut Data|**Tipe Spesifikasi**|
|---|---|---|---|---|---|
|id|UUID|||provinsi|VARCHAR(100)|
|nama|VARCHAR(255)|||kota|VARCHAR(100)|
|sip_notaris|VARCHAR(50)|||is_active|BOOLEAN|
|npwp|VARCHAR(15)|||rating|DECIMAL(2,1)|
|alamat_kantor|TEXT|||total_fidusia|INTEGER|
|nomor_hp|VARCHAR(15)|||created_at|TIMESTAMP|
|email|VARCHAR(255)|||updated_at|TIMESTAMP|



**Universitas Tidar** June 2026 

|Atribut Data<br>**Tipe Spesifikasi**<br>id<br>UUID<br>kendaraan_id<br>UUID<br>teknisi_id<br>UUID<br>supervisor_id<br>UUID<br>dealer_id<br>UUID<br>dewaxing_passed<br>BOOLEAN<br>dewaxing_notes<br>TEXT<br>fuse_installed<br>BOOLEAN<br>Entitas : PDI Report|Atribut Data<br>**Tipe Spesifikasi**<br>id<br>UUID<br>kendaraan_id<br>UUID<br>teknisi_id<br>UUID<br>supervisor_id<br>UUID<br>dealer_id<br>UUID<br>dewaxing_passed<br>BOOLEAN<br>dewaxing_notes<br>TEXT<br>fuse_installed<br>BOOLEAN<br>Entitas : PDI Report|**Data Dictionary**<br>Atribut Data<br>**Tipe Spesifikasi**<br>fuse_type<br>VARCHAR(50)<br>obd_scan_passed<br>BOOLEAN<br>dtc_count<br>INTEGER<br>dtc_codes<br>JSONB<br>fluid_check_passed<br>BOOLEAN<br>fluid_notes<br>JSONB<br>gps_start_lat<br>DECIMAL(10,8)<br>gps_start_lng<br>DECIMAL(11,8)|**Data Dictionary**<br>Atribut Data<br>**Tipe Spesifikasi**<br>fuse_type<br>VARCHAR(50)<br>obd_scan_passed<br>BOOLEAN<br>dtc_count<br>INTEGER<br>dtc_codes<br>JSONB<br>fluid_check_passed<br>BOOLEAN<br>fluid_notes<br>JSONB<br>gps_start_lat<br>DECIMAL(10,8)<br>gps_start_lng<br>DECIMAL(11,8)|**Data Dictionary**<br>Atribut Data<br>**Tipe Spesifikasi**<br>fuse_type<br>VARCHAR(50)<br>obd_scan_passed<br>BOOLEAN<br>dtc_count<br>INTEGER<br>dtc_codes<br>JSONB<br>fluid_check_passed<br>BOOLEAN<br>fluid_notes<br>JSONB<br>gps_start_lat<br>DECIMAL(10,8)<br>gps_start_lng<br>DECIMAL(11,8)||||
|---|---|---|---|---|---|---|---|
|||||||Atribut Data|**Tipe Spesifikasi**|
|Atribut Data|**Tipe Spesifikasi**||Atribut Data|**Tipe Spesifikasi**||||
|||||||gps_end_lat|DECIMAL(10,8)|
|id|UUID||fuse_type|VARCHAR(50)||||
|||||||gps_end_lng|DECIMAL(11,8)|
|kendaraan_id|UUID||obd_scan_passed|BOOLEAN||||
|||||||gps_distance_km|DECIMAL(8,3)|
|teknisi_id|UUID||dtc_count|INTEGER||||
|||||||uji_jalan_passed|BOOLEAN|
|supervisor_id|UUID||dtc_codes|JSONB||||
|||||||uji_jalan_notes|TEXT|
|dealer_id|UUID||fluid_check_passed|BOOLEAN||||
|||||||overall_passed|BOOLEAN|
|dewaxing_passed|BOOLEAN||fluid_notes|JSONB||||
|||||||rejection_reason|TEXT|
|dewaxing_notes|TEXT||gps_start_lat|DECIMAL(10,8)||||
|||||||document_path|VARCHAR(512)|
|fuse_installed|BOOLEAN||gps_start_lng|DECIMAL(11,8)||||
|||||||||



**Universitas Tidar** 

June 2026 

Entitas : PDI Report 

## **Data Dictionary** 

|Atribut Data|**Tipe Spesifikasi**|||Atribut Data|**Tipe Spesifikasi**|
|---|---|---|---|---|---|
|teknisi_signature_path|VARCHAR(512)|||approved_at|TIMESTAMP|
|supervisor_signature_path|VARCHAR(512)|||created_at|TIMESTAMP|
|inspection_start_at|TIMESTAMP|||updated_at|TIMESTAMP|
|inspection_end_at|TIMESTAMP|||deleted_at|TIMESTAMP|



**Universitas Tidar** 

June 2026 

## **Kebutuhan Fungsional** 

Terdapat 5 modul fungsional utama : **Pricing Engine :** kalkulasi harga & pajak transparan berdasarkan NJKB, status kepemilikan progresif, dan tipe mesin. 

**Appraisal & Inspeksi Fisik :** pemandu inspeksi mobile untuk 150–188 titik pemeriksaan mobil bekas. **Document Verification Manager :** OCR & validasi keaslian dokumen secara otomatis. **Fintech Leasing Hub :** pengajuan kredit asynchronous & pendaftaran fidusia digital. **PDI & Logistics Tracker** : pelacakan unit, checklist PDI, STCK, hingga aktivasi garansi. 

**Universitas Tidar** 

June 2026 

## **Integrasi API Eksternal** 

|**API**|**Tujuan Integrasi**|**Output**|
|---|---|---|
|Dukcapil|Verifikasi KTP & face matching biometrik|Kesesuaian NIK, skor face matching|
|SLIK OJK /<br>PEFINDO|Kelayakan kredit instan|Skor kolektibilitas (iDeb) skala 1–5|
|Samsat / SIGNAL|Verifikasi kendaraan bekas & pajak|Status blokir ETLE, masa STNK, nilai PKB|
|Ditjen AHU|Pendaftaran jaminan fidusia elektronik|Sertifikat fidusia elektronik|
|TTE (Privy/VIDA)|Keabsahan kontrak tanpa kertas|Tanda tangan tersertifikasi Kominfo|



**Universitas Tidar** 

June 2026 

## **Kebutuhan Non-Fungsional** 

**Performa** : 

- Response time: p50 ≤2 detik, p99 ≤3,5 detik, p99.9 ≤5 detik. 

- Pengajuan kredit asynchronous rata-rata 4,22 detik. 

- Kapasitas: 10.000 pengguna aktif bersamaan, throughput 500 TPS. 

## **Keamanan** : 

- Enkripsi AES-256 (data tersimpan) + TLS 1.3 (data berjalan). 

- RBAC + MFA (OTP 6 digit via WhatsApp/Email, berlaku 5 menit) untuk Admin, Surveyor, System Admin. TTE wajib bersertifikat Kominfo (Privy/VIDA). 

Liveness detection + face matching Dukcapil minimal skor 85%. 

**Keandalan** : uptime minimal 99,9% (maks. downtime 43 menit/bulan). Jika API pemerintah down, sistem tetap berjalan (graceful degradation) dengan notifikasi & retry otomatis. 

**Usabilitas & Portabilitas:** aplikasi inspeksi Android 10+/iOS 14+; web portal WCAG 2.1 AA; kompatibel Chrome 90+, Safari 14+, Firefox 88+. 

**Universitas Tidar** 

June 2026 

## **Verifikasi & Acceptance Criteria** 

**4 metode verifikasi (ISO/IEC/IEEE 29148:2018): Test :** pengujian dinamis (load test, integrasi API, pen-test). **Demonstration :** menunjukkan fungsi secara visual (misal: alur PDI oleh teknisi). **Inspection :** pemeriksaan statis kode/skema database. **Analysis :** pembuktian via kalkulasi matematis (misal: logika pricing engine). **4 kriteria penerimaan akhir:** 100% functional requirement lulus uji tanpa kesalahan logika bisnis. Seluruh 5 API eksternal terkoneksi aman dengan error handling graceful. **Stress test:** throughput ≥500 TPS, latency p99 ≤3,5 detik. **Audit keamanan:** zero critical vulnerability, enkripsi AES-256 berjalan baik. 

**Universitas Tidar** 

June 2026 

## **Matriks Pengujian RTM** 

||**Requirement ID**|**Nama Persyaratan**|**Use Case ID**|**Metode Verifikasi**|**Kriteria Kelulusan Pengujian**<br>**(Acceptance Criteria)**|
|---|---|---|---|---|---|
||REQ-TAX-001|Hitung DPP PPN Nilai Lain|UC-01|Analysis|Perhitungan DPP PPN tepat senilai 11/12 *<br>tanpa selisih Rp1.|
||REQ-TAX-011|Cek Pajak Progresif|UC-01|Test|Query NIK ke API Samsat<br>mengembalikan data unit aktif dengan<br>respons < 5 detik.|
||REQ-DOC-008|Verifikasi Hologram BPKB|UC-06|Test|Algoritma klasifikasi gambar di atas<br>mendeteksi hologram Tri Brata dengan<br>akurasi ≥ 95%.|
||REQ-FINT-008|Batas Waktu Jaminan Fidusia|UC-09|Analysis|Sistem menolak pengiriman data ke<br>Ditjen AHU jika tanggal akta notaris > 30<br>hari.|



**Universitas Tidar** 

June 2026 

## **Matriks Pengujian RTM (2)** 

|**Requirement ID**|**Nama Persyaratan**|**Use Case ID**|**Metode Verifikasi**|**Kriteria Kelulusan Pengujian**<br>**(Acceptance Criteria)**|
|---|---|---|---|---|
|REQ-LOG-008|Jarak Uji Jalan PDI|UC-10|Test|Sensor GPS diler mencatat jarak tempuh<br>uji jalan diler minimal tepat 10 kilometer.|
|REQ-INT-001|Face Matching Dukcapil|UC-04|Test|Verifikasi identitas registrasi<br>mengembalikan kecocokan biometrik<br>wajah  ≥ 85%.|
|REQ-NF-002|Latensi Async ESB Leasing|UC-03|Test|Waktu respons pengiriman melalui<br>broker antrean pesan diler rata-rata 4,22<br>detik.|



**Universitas Tidar** June 2026 

## **Matriks Pengujian CRUD** 

|**Entitas Data**|**Create (C)**|**Read (R)**|**Update (U)**|**Delete (D)**|
|---|---|---|---|---|
|Data Unit Mobil|REQ-LOG-001 (Logistik)|REQ-TAX-016<br>(Breakdown OTR)|REQ-LOG-012 (Aktivasi Garansi)|Tidak Diizinkan<br>(Hanya Soft-Delete)|
|Berkas Jual-Beli|REQ-DOC-014 (Upload Berkas)|REQ-DOC-003<br>(Pencocokan Admin)|REQ-INT-005 (TTE Dokumen SPK)|Tidak Diizinkan|
|Aplikasi Kredit|REQ-FINT-004 (Submission)|REQ-FINT-005<br>(Dasbor Surveyor)|REQ-FINT-005 (Webhook Callback)|Tidak Diizinkan|
|Sertifikat Fidusia|REQ-FINT-007 (Notaris SABH)|REQ-FINT-009<br>(Dasbor Admin)|REQ-FINT-008 (Monitoring Batas)|Tidak Diizinkan|



**Universitas Tudar** 

June 2026 

## **Aturan Validasi Sistem** 

## **Tujuan** 

Menjamin proses verifikasi dokumen dan inspeksi kendaraan memenuhi standar yang ditetapkan sebelum transaksi dapat dilanjutkan. 

## **Aturan Validasi Utama** 

|**Kode Requirements**|**Aturan Validasi**|**Kriteria**|
|---|---|---|
|REQ-INT-001|Face Matching Dukcapil|Tingkat kecocokan biometrik wajah minimal 85%|
|REQ-DOC-008|Verifikasi Hologram BPKB|Akurasi deteksi hologram Tri Brata minimal 95%|
|REQ-LOG-008|Validasi Uji Jalan PDI|Jarak uji jalan minimal 10 km sebelum status Lolos PDI diterbitkan|



## **Error Handling** 

Face matching < 85% → Registrasi ditolak dan pengguna diminta melakukan verifikasi ulang. . Akurasi deteksi hologram tidak memenuhi standar → Dokumen BPKB ditandai **gagal verifikasi** Jarak uji jalan < 10 km → Status **“Lolos PDI”** tidak dapat diproses oleh sistem. 

**Universitas Tidar** 

June 2026 

## **ERD (Diagram Relasi Entitas)** 

Sistem memiliki 7 entitas data inti yang saling berelasi mengikuti alur transaksi 

Pembeli terhubung ke Kendaraan (1:N) dan AplikasiKredit (1:N) 

Kendaraan memiliki koleksi DokumenLegalitas (1:N) setiap AplikasiKredit ditangani oleh satu Notaris (N:1) 

bermuara pada satu SertifikatFidusia (1:1) wajib didahului satu PDI_Report (1:1) sebelum pencairan dana. 

Seluruh entitas menggunakan mekanisme soft-delete (tidak ada penghapusan permanen) untuk menjaga auditabilitas dan ketertelusuran data transaksi. 

**Universitas Tidar** 

## ~~**e** ee~~ e ~~rere eee eee~~ June 2026 **Modul Kalkulasi Pajak Lengkap** 

**Universitas Tidar** 

June 2026 

## **Modul Kalkulasi Pajak** 

**Universitas Tidar** June 2026 

## **Penentuan DP Dinamis Berdasarkan NPF** 

**Universitas Tidar** 

June 2026 

## **Matrix NPF vs DP Minimum** 

**Universitas Tidar** 

## ~~eee~~ June 2026 **Matriks Validasi Berdasarkan Jenis Kendaraan** 

**Universitas Tidar** 

June 2026 

## **Algoritma Validasi NPWP (Luhn Checksum)** 

**Universitas Tidar** 

June 2026 

# **Deteksi Hologram BPKB (Akurasi 95%)** 

**Universitas Tidar** 

June 2026 

**Validasi Batas Waktu Fidusia (30 Hari)** 

**Universitas Tidar** 

June 2026 

## **Proses PDI Lengkap** 

**Universitas Tidar** 

June 2026 

## **Detail Checklist PDI per Tahapan** 

**Universitas Tidar** 

June 2026 

## **Alur STCK (Plat Nomor Sementara)** 

**Universitas Tidar** 

June 2026 

## **Ringkasan Aturan Blokir Sistem** 

**Univeersitas Tidar** 

June 2026 

**UC-01 : Estimasi Harga Online** 

**Aktor Utama:** Pembeli, Sales Dealer **Aktor Pendukung:** Pricing Engine, API Samsat/SIGNAL **Deskripsi:** Kalkulasi estimasi harga final kendaraan (OTR) secara real-time berdasarkan regulasi perpajakan otomotif Indonesia. **Prakondisi:** Pengguna telah login, data spesifikasi unit tersedia, koneksi internet aktif. 

**Pascakondisi (sukses):** Sistem menyajikan rincian biaya OTR, mencatat riwayat penelusuran, pembeli siap lanjut ke reservasi unit. 

**Universitas Tidar** 

June 2026 

## **UC-03: Pengajuan Kredit (SLIK OJK & Scoring)** 

**Aktor Utama:** Pembeli, Surveyor Leasing 

**Aktor Pendukung:** Sistem Fintech, API SLIK OJK, API PEFINDO **Deskripsi:** Memfasilitasi pengajuan kredit mobil secara asynchronous melalui integrasi data riwayat keuangan SLIK OJK dan credit scoring otomatis. 

- **Prakondisi:** Pembeli lolos verifikasi biometrik (face matching ≥85%), status kendaraan BOOKED, koneksi ESB aktif. 

- **Pascakondisi (sukses):** Aplikasi kredit tersimpan status PENDING, ID pengajuan diterbitkan, riwayat iDeb tertarik, notifikasi terkirim ke surveyor. 

## **Univeersitas Tidar** 

## June 2026 

## **UC-03: Pengajuan Kredit (SLIK OJK & Scoring)** 

**Universitas Tidar** 

June 2026 

## **UC-09: Pendaftaran Jaminan Fidusa** 

**Aktor Utama :** Admin Dealer, Notaris Rekanan **Aktor Pendukung :** API Ditjen AHU Kemenkumham, Sistem Fintech **Deskripsi :** Pengikatan dan pendaftaran jaminan fidusia secara elektronik ke Ditjen AHU melalui notaris rekanan diler. **Prakondisi :** Aplikasi Kredit berstatus DOCUMENTED, akta fidusia sudah ditandatangani notaris, portal AHU online normal. **Pascakondisi (sukses) :** Sertifikat fidusia berhasil diunduh elektronik, status kredit berubah menjadi FIDUSIA_COMPLETE. 

**Univeersitas Tidar** 

June 2026 

## **UC-09 : Pendaftaran Jaminan Fidusa** 

**Universitas Tudar** 

June 2026 

## **Kesimpulan** 

Efisiensi: proses berhari-hari menjadi satu alur terintegrasi real-time. Kepatuhan regulasi otomatis: sistem menegakkan aturan hukum (fidusia 30 hari, dokumen wajib, formula pajak) tanpa mengandalkan manual staf. Keamanan data: enkripsi berlapis, MFA, dan TTE bersertifikat. Transparansi: konsumen melihat rincian OTR lengkap sebelum membayar. 

**Universitas Tidar** 

June 2026 

# **Thank You** 

