# Aturan Bisnis & Regulasi Hukum Transaksi Otomotif Indonesia

## 1. pricing Engine & Perpajakan (Wajib)

Semua fungsi kalkulator biaya dalam SRS wajib mengadopsi struktur perpajakan legal Indonesia berikut:

* **Pajak Pertambahan Nilai (PPN):** Tarif PPN adalah 12% sesuai PMK 131/2024. Dasar Pengenaan Pajak (DPP) wajib menggunakan mekanisme Nilai Lain (11/12 dari NJKB).
Formula: $\text{DPP PPN}=\frac{11}{12}\times\text{NJKB}$
Formula: $\text{PPN}=12\%\times\text{DPP PPN}=11\%\times\text{NJKB}$
* **Sistem Opsen (UU HKPD):** Pemerintah kabupaten/kota memungut Opsen PKB dan Opsen BBNKB sebesar 66% dari pokok pajak terutang.
Formula: $\text{Opsen PKB}=66\%\times\text{PKB Pokok}$
Formula: $\text{Opsen BBNKB}=66\%\times\text{BBNKB Pokok}$
* **Pajak Progresif (DKI Jakarta Perda 1/2024):** Kepemilikan kendaraan roda empat sejenis dengan alamat KTP/KK yang sama dikenakan tarif: Mobil ke-1 (2%), ke-2 (3%), ke-3 (4%), ke-4 (5%), ke-5 dst (6%). Rumus pencarian NJKB dari data STNK wajib didefinisikan sebagai:
Formula: $\text{NJKB}=\frac{\text{PKB Pokok}}{\text{Tarif Berlaku}}$

## 2. Kepatuhan Kredit & Jaminan Fidusia (OJK)

Modul pengajuan pembiayaan pihak ketiga wajib mematuhi aturan OJK:

* **Uang Muka Minimum (DP):** Ditentukan dinamis berdasarkan rasio NPF net leasing mitra (NPF <= 1% = DP 0%, NPF 1-3% = DP 10%, NPF 3-5% = DP 15%, NPF > 5% = DP 20%).
* **Insentif Kendaraan Listrik (EV):** Unit dengan TKDN >= 40% berhak mendapatkan insentif PPN ditanggung pemerintah (PPN DTP) sebesar 10% (konsumen hanya membayar PPN efektif 2%). Mobil Hybrid TIDAK mendapatkan insentif ini.
* **Pendaftaran Fidusia:** Sesuai PP Nomor 21 Tahun 2015, pendaftaran jaminan fidusia secara elektronik ke Ditjen AHU Kemenkumham wajib diselesaikan dalam jangka waktu maksimal 30 hari kalender sejak pembuatan akta jaminan fidusia di hadapan notaris. Jika lewat, pendaftaran otomatis ditolak oleh sistem AHU.

## 3. Dokumen & Legalitas Jual-Beli (Matriks Validasi)

Sistem manajemen dokumen wajib melakukan validasi input (KTP, KK, NPWP) serta validasi spesifik:

* **Mobil Baru:** Faktur Kendaraan asli, NIK, SRUT, STCK, dan Form A (khusus CBU).
* **Mobil Bekas:** BPKB asli (verifikasi hologram Tri Brata dan benang UV), STNK aktif, Faktur Pembelian asli, 2-3 kuitansi kosong yang ditandatangani pemilik sesuai BPKB (rangkap bermaterai untuk BBN-II), dan Surat Pelepasan Hak (SPH) jika unit eks-aset perusahaan.
* **Mobil Bekas dalam Angsuran:** Salinan kontrak leasing, fotokopi BPKB terlegalisir, dan bukti histori setoran bulanan terakhir dari leasing lama.

## 4. QC Pre-Delivery Inspection (PDI) & Logistik Pengiriman

* **Alur Kerja PDI:** Unit diler hanya boleh dikirim ke konsumen jika teknisi telah mengisi checklist PDI (de-waxing bodi, pemasangan backup fuse utama, scan OBD-II dan penghapusan DTC eror pada ECU, pengisian cairan bodi/mesin, dan uji jalan jalan raya sejauh minimal 10 km).
* **Aturan Operasional STCK:** Kendaraan baru dikirim menggunakan STCK dan pelat sementara (merah-putih) dengan masa berlaku 1 bulan. Biaya administrasi PNBP resmi adalah Rp50.000. Aturan sistem harus melarang penggunaan unit di luar logistik diler-konsumen atau uji fisik jalan raya (dilarang keras untuk dikendarai pribadi atau keluar kota sebelum STNK asli terbit).