# System Architecture & API Integration Map

## 1. Arsitektur Komunikasi Integrasi (Leasing B2B)

Untuk menghindari latensi tinggi dan kegagalan transaksi, integrasi sistem diler dengan pihak ketiga (seperti leasing ACC atau TAF) wajib menggunakan arsitektur **Asynchronous** berbasis Enterprise Service Bus (ESB):

* Sistem Asynchronous memiliki kinerja waktu respons rata-rata **4,22 detik per request** tanpa kegagalan koneksi.
* Sistem Synchronous dilarang karena memiliki waktu respons rata-rata **31,49 detik per request** dengan tingkat kegagalan koneksi (*error rate*) mencapai **49,83%**.

## 2. Spesifikasi Integrasi API Pihak Ketiga (External Interfaces)

Sistem dalam SRS wajib menjabarkan kebutuhan integrasi API eksternal berikut:

| Nama API | Tujuan Integrasi | Parameter Input | Output / Respons Data |
| --- | --- | --- | --- |
| **API Dukcapil** | Mencegah penipuan identitas (identity fraud) saat pendaftaran pembeli baru | NIK, Foto e-KTP, Swafoto biometrik pelanggan | Persentase kecocokan biometrik wajah (*face matching score*) |
| **API SLIK OJK / PEFINDO** | Analisis kelayakan kredit instan secara online | NIK, Nama Lengkap, Tanggal Lahir | Dokumen iDeb, riwayat kredit, status kolektibilitas nasabah |
| **API Samsat / SIGNAL** | Validasi STNK mobil bekas dan perhitungan pajak progresif | Nomor Registrasi Kendaraan (NRKB), NIK Pembeli, 5 digit nomor rangka | Status keaslian STNK, status blokir ETLE, nominal PKB, tarif progresif |
| **API Ditjen AHU (Fidusia)** | Pendaftaran jaminan fidusia diler/leasing | Data Akta Notaris, Nomor Rangka/Mesin, Nilai Penjaminan | Kode Bayar PNBP, Sertifikat Jaminan Fidusia Elektronik |
| **API Digital Signature** | Penandatanganan dokumen SPK diler dan kontrak kredit secara paperless | Dokumen PDF, Nomor Handphone (OTP) | Sertifikat tanda tangan digital tersertifikasi Kominfo (Privy/VIDA) |
