# Aturan Penulisan Dokumen SRS (ISO/IEC/IEEE 29148)

## 1. Alur Kerja Analisis Berkelanjutan (DoD)

Setiap kali Anda diminta menulis atau mengedit bab dalam dokumen SRS, Anda wajib mengikuti 5 fase kerja berikut sebelum melakukan perubahan file:

1. **Fase Klarifikasi:** Ajukan pertanyaan kritis jika ada skenario bisnis yang ambigu.
2. **Fase Analisis & Pencarian:** Teliti dampak perubahan terhadap bab SRS lainnya (jaga konsistensi traceability matrix).
3. **Fase Validasi Rencana:** Tampilkan rencana implementasi bab kepada pengguna dan minta persetujuan.
4. **Fase Penulisan Bertahap:** Tulis kebutuhan sistem secara terperinci.
5. **Fase Penyempurnaan:** Berikan saran optimasi non-fungsional.

## 2. Standar Bahasa Persyaratan (RFC 2119 / IEEE 29148)

Anda wajib menggunakan modal verbs secara ketat dalam mendefinisikan kebutuhan sistem di dokumen SRS:

* **"shall"**: Digunakan untuk kebutuhan wajib (mandatory). Sistem harus memenuhinya untuk dianggap lulus verifikasi.
* **"should"**: Digunakan untuk rekomendasi kuat.
* **"may"**: Digunakan untuk fitur opsional yang bersifat tambahan.

## 3. Kriteria Kualitas Persyaratan

Setiap butir persyaratan (fungsional maupun non-fungsional) yang Anda tulis wajib memenuhi 4 kriteria ini:

* **Unambiguous**: Hanya memiliki satu interpretasi logis.
* **Testable**: Memiliki kriteria penerimaan (acceptance criteria) yang jelas dan bisa diukur.
* **Traceable**: Memiliki ID unik (misal: REQ-F-001) yang terhubung ke modul bisnis terkait.
* **Complete**: Menjabarkan input, proses, output, prakondisi, pascakondisi, dan penanganan eror (error handling).