# Spesifikasi Integrasi API & Pemodelan Arsitektur

## 1. Pemodelan UML Berbasis Mermaid

Setiap kali Anda merancang alur kerja, use case, state-machine dokumen, atau sekuens transaksi dalam SRS, Anda wajib menampilkan diagram visual menggunakan sintaks Mermaid UML. Pastikan diagram diletakkan di dalam blok kode `mermaid`.

## 2. Integrasi API Pihak Ketiga (Wajib Tercantum dalam SRS)

Kebutuhan antarmuka eksternal harus menjabarkan spesifikasi API berikut secara fungsional:

* **API Kemendagri (Dukcapil):** Melakukan verifikasi e-KTP pembeli secara real-time menggunakan face matching dan kecocokan data biometrik untuk memitigasi identity fraud.
* **API SLIK OJK / PEFINDO:** Melakukan credit checking otomatis untuk mengunduh dokumen iDeb konsumen guna menentukan kelayakan pengajuan kredit.
* **API Samsat / SIGNAL:** Melakukan verifikasi keaslian STNK (modul trade-in), cek nilai pajak progresif berdasarkan NIK, serta melacak status pemblokiran tilang elektronik (ETLE).
* **API Ditjen AHU (Fidusia Online):** Memungkinkan diler/leasing mengirimkan draf akta jaminan fidusia secara otomatis ke notaris mitra, membayar PNBP via e-payment, dan mengunduh sertifikat fidusia elektronik.
* **API Digital Signature (TTE Berbayar):** Menghubungkan sistem ke penyedia TTE tersertifikasi Kominfo (seperti Privy atau VIDA) untuk penandatanganan dokumen SPK diler dan kontrak perjanjian pembiayaan kredit secara digital dan sah secara hukum.

## 3. Desain Asynchronous B2B Leasing

Modul integrasi dengan leasing partner (seperti ACC atau TAFS) wajib menggunakan arsitektur Enterprise Service Bus (ESB) secara Asynchronous. Hal ini penting untuk memastikan performa waktu tunggu respons yang minimal (rata-rata kecepatan pemrosesan respons sistem asynchronous di Indonesia adalah 4,22 detik per request, jauh lebih stabil dibandingkan sistem synchronous yang memakan waktu hingga 31,49 detik dan rentan terhadap error kegagalan koneksi).