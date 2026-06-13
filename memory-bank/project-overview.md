# Project Overview: Sistem Transaksi Pembelian Mobil Terintegrasi (Indonesia)

## Gambaran Umum Sistem

Sistem ini dirancang untuk memfasilitasi transaksi pembelian kendaraan roda empat, baik dalam kondisi baru maupun bekas, dengan mengintegrasikan seluruh aktor penting serta regulasi perpajakan dan pembiayaan yang berlaku di Indonesia.

## Model Bisnis & Alur Online-to-Offline (O2O)

Sistem ini mengadopsi model omnichannel yang mulus (O2O) untuk jual-beli dan tukar-tambah (trade-in) kendaraan bermotor:

1. **Estimasi Online:** Konsumen mendapatkan taksiran harga awal secara instan melalui sistem pricing engine di website/aplikasi.
2. **Inspeksi Fisik Digital:** Penjadwalan inspeksi kendaraan (di rumah konsumen atau di diler) berpedoman pada checklist digital terstandarisasi (150 titik hingga 188 titik pemeriksaan).
3. **Penawaran Final & Transparansi:** Sistem menganalisis laporan inspeksi fisik dan dokumen legalitas untuk merilis penawaran harga final yang objektif.
4. **Pencairan Instan (Instant Payout):** Sistem memproses transfer dana secara instan ke rekening penjual dalam waktu kurang dari 60 menit setelah kesepakatan dicapai dan dokumen dinyatakan lengkap serta valid.
5. **Aktivasi Garansi:** Sistem mengaktifkan garansi komponen utama (seperti garansi transmisi dan mesin selama 1 tahun) secara otomatis setelah unit diserahterimakan.

## Daftar Aktor Utama Sistem

* **Pembeli / Konsumen (Perorangan atau Badan Hukum):** Melakukan inisiasi pembelian, pengunggahan dokumen identitas, simulasi kredit, serta tanda tangan kontrak digital.
* **Sales Dealer / Agen:** Mengelola prospek pelanggan, membuat Surat Pemesanan Kendaraan (SPK), dan memantau status pembayaran.
* **Admin Dealer / Backoffice:** Melakukan verifikasi dokumen wajib, menginput data kendaraan ke sistem perpajakan Samsat, serta mengalokasikan biaya pengurusan STNK, BPKB, dan BBN.
* **Inspektor / Appraisal Officer:** Melakukan pengecekan fisik kendaraan di lapangan menggunakan aplikasi inspeksi modular.
* **Surveyor Leasing (Multifinance Partner):** Melakukan analisis kelayakan kredit pelanggan dan memproses persetujuan pembiayaan.
* **Teknisi PDI & Logistik:** Melakukan Pre-Delivery Inspection (PDI) dan memperbarui status pengiriman unit.
