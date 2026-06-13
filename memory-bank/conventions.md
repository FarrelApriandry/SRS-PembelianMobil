# Business Rules & Legal Conventions: Transaksi Otomotif Indonesia

## 1. pricing Engine & Struktur Perpajakan Kendaraan Baru

Sistem wajib mengimplementasikan formula perhitungan perpajakan yang mengacu pada UU HKPD (Hubungan Keuangan antara Pemerintah Pusat dan Pemerintahan Daerah) dan peraturan PPN terbaru:

### A. Pajak Pusat (Pemerintah Pusat)

* **Dasar Pengenaan Pajak (DPP) PPN Nilai Lain:** Dihitung sebesar 11/12 dari Nilai Jual Kendaraan Bermotor (NJKB).

$$\text{DPP PPN}=\frac{11}{12}\times\text{NJKB}$$


* **Pajak Pertambahan Nilai (PPN) Terutang:** Sebesar 12% dari DPP PPN Nilai Lain (efektif 11% dari NJKB) berdasarkan PMK 131/2024.

$$\text{PPN}=12\%\times\text{DPP PPN}=11\%\times\text{NJKB}$$


* **Pajak Penjualan atas Barang Mewah (PPnBM):** Dikenakan secara variatif berdasarkan tipe silinder dan jenis mesin, dikalikan dengan DPP Nilai Lain.

### B. Pajak Daerah & Opsen (Pemerintah Provinsi & Kabupaten/Kota)

Sesuai UU HKPD, sistem wajib memisahkan nilai pajak pokok dengan nilai opsen secara transparan:

* **Pajak Kendaraan Bermotor (PKB):** Tarif standar kepemilikan pertama maksimal 2% dari NJKB (dikali bobot koefisien kerusakan jalan).
* **Opsen PKB:** Pungutan tambahan sebesar 66% dari nilai PKB Pokok terutang.

$$\text{Opsen PKB}=66\%\times\text{PKB Pokok}$$

> **⚠️ ATURAN KHUSUS DKI JAKARTA:**
> Jika wilayah pendaftaran kendaraan adalah **DKI Jakarta**, maka:
> - **Opsen PKB = Rp0** (tidak dikenakan karena Jakarta tidak memiliki wilayah kabupaten/kota otonom)
> - **Opsen BBNKB = Rp0** (tidak dikenakan karena Jakarta tidak memiliki wilayah kabupaten/kota otonom)
> - **Tarif Pokok BBNKB Penyerahan Pertama = 12,5%** (sesuai Perda DKI No. 1 Tahun 2024)

* **Bea Balik Nama Kendaraan Bermotor (BBNKB):**
  - Provinsi Lain: Tarif maksimal penyerahan pertama **12%** dari NJKB
  - DKI Jakarta: Tarif maksimal penyerahan pertama **12,5%** dari NJKB (Perda DKI No. 1 Tahun 2024)
* **Opsen BBNKB:**
  - Provinsi Lain: Pungutan tambahan sebesar 66% dari nilai BBNKB Pokok
  - DKI Jakarta: **Opsen BBNKB = Rp0**

$$\text{Opsen BBNKB}=66\%\times\text{BBNKB Pokok (Provinsi Lain)}$$

$$\text{Opsen BBNKB}=Rp0 \text{ (DKI Jakarta)}$$



### C. Retribusi & Administrasi Wajib (PNBP)

* **SWDKLLJ (Jasa Raharja):** Rp143.000 s.d. Rp150.000 (flat berdasarkan golongan kendaraan roda empat).
* **Penerbitan STNK Baru:** Rp200.000 (PNBP Polri).
* **Penerbitan TNKB (Pelat Nomor) Baru:** Rp100.000 (PNBP Polri).
* **Penerbitan BPKB Baru:** Rp375.000 (PNBP Polri).

### D. Skema Perhitungan Pajak Progresif (DKI Jakarta Perda 1/2024)

Dikenakan jika NIK atau alamat KK pembeli sudah terdaftar memiliki kendaraan roda empat sejenis:

* **Tarif Progresif:** Kendaraan Ke-1 (2,0%), Ke-2 (3,0%), Ke-3 (4,0%), Ke-4 (5,0%), Ke-5 dst (6,0% flat maksimal).
* **Pencarian Nilai Dasar (NJKB) dari Lembar STNK:**

$$\text{NJKB}=\frac{\text{PKB Pokok}}{\text{Tarif Berlaku}}$$



## 2. Kepatuhan Kredit, Subsidi, & Jaminan Fidusia (OJK)

* **Uang Muka (DP) Minimum Konvensional:** Ditentukan dinamis berdasarkan rasio NPF Net perusahaan pembiayaan partner (NPF <= 1% = DP 0%, NPF 1-3% = DP 10%, NPF 3-5% = DP 15%, NPF > 5% = DP 20%).
* **Insentif Kendaraan Listrik (KBLBB):** Unit dengan TKDN >= 40% berhak atas insentif PPN ditanggung pemerintah (PPN DTP) sebesar 10% (konsumen hanya membayar PPN efektif 2%). Mobil Hybrid tidak mendapatkan insentif ini.
* **Aturan Jaminan Fidusia:** Sesuai PP Nomor 21 Tahun 2015, pendaftaran jaminan fidusia secara elektronik ke Ditjen AHU Kemenkumham wajib diajukan paling lambat **30 hari kalender** sejak pembuatan akta notaris jaminan fidusia. Keterlambatan pendaftaran akan menyebabkan sistem Kemenkumham menolak permohonan secara otomatis.

## 3. Matriks Validasi Dokumen Jual-Beli

Sistem wajib menolak proses transaksi apabila dokumen berikut tidak lengkap/tidak valid:

* **Mobil Baru:** Faktur Kendaraan asli, NIK, SRUT, KTP asli, dan Form A (khusus CBU).
* **Mobil Bekas:** BPKB asli (verifikasi hologram Tri Brata dan benang UV), STNK aktif, Faktur Pembelian, Kwitansi Kosong 2-3 rangkap ditandatangani pemilik sesuai BPKB (rangkap bermaterai untuk BBN-II), dan Surat Pelepasan Hak (SPH) jika unit eks-aset perusahaan.
* **Mobil Bekas dalam Kredit Aktif:** Kontrak leasing lama, fotokopi BPKB terlegalisir, dan histori setoran pembayaran terakhir dari leasing sebelumnya.

## 4. Prosedur QC PDI & Logistik Pengiriman

* **Checklist PDI Wajib:** Teknisi diler harus mengisi laporan PDI digital (pembilasan wax pelindung, pemasangan backup fuse kelistrikan, pemindaian port OBD-II untuk menghapus DTC eror pada ECU, pemeriksaan cairan mesin, dan uji jalan sejauh minimal 10 km).
* **Operasional STCK (Pelat Sementara):** Berdasarkan Perkapolri 7/2021, STCK hanya berlaku selama **1 bulan** sejak diterbitkan. Penggunaan plat sementara merah-putih dilarang keras untuk dikendarai di jalan raya umum untuk keperluan pribadi (bisa ditilang) serta dilarang dikendarai ke luar kota. Biaya administrasi PNBP resmi adalah Rp50.000.
