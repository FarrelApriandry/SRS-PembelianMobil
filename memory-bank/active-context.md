# Active Context: Penyempurnaan Dokumen SRS & Enhancement Academic

## Fokus Aktif Saat Ini

Menyempurnakan dokumen SRS dengan komponen akademis tambahan menggunakan **Mermaid.js flowchart** untuk visualisasi yang lebih intuitif.

## Enhancement Terakhir (13 Juni 2026)

1. **UC-01: Estimasi Harga Online (Pricing Engine)** - Flowchart Mermaid:
   - Alur Normal: 19 steps dari input hingga simpan ke database
   - Alur Alternatif: 4 exception flows (API timeout, KBLBB, pajak progresif, input invalid)

2. **UC-03: Pengajuan Kredit (SLIK OJK & Scoring)** - Flowchart Mermaid:
   - Alur Normal: 31 steps dari submit hingga approved
   - Alur Alternatif: 4 exception flows (SLIK timeout, kolektibilitas, ESB fail, duplikat)

3. **UC-09: Pendaftaran Jaminan Fidusia** - Flowchart Mermaid + Gantt:
   - Alur Normal: 31 steps dari login hingga pencairan dana
   - Alur Alternatif: 6 exception flows (countdown warning H-25/28/29, deadline exceeded, AHU rejection, payment timeout)
   - Timeline Gantt: Visualisasi 30 hari deadline dengan milestone warnings

## Keputusan Enhancement Terakhir

1. **Mermaid Flowchart:** Mengubah Use Case Naratif dari tabel step-by-step menjadi flowchart visual
   - Menggunakan `flowchart TD` untuk alur vertikal
   - Emoji icons untuk membedakan aktor (👤 👨‍⚖️ 💻)
   - Decision nodes dengan diamond shape
   - Clear END states

2. **Mermaid Gantt:** Menambahkan timeline countdown untuk UC-09 Fidusia
   - Visualisasi 30 hari deadline
   - Milestone markers untuk warning points

3. **Prioritas Requirements:** Kolom prioritas (TINGGI/SEDANG/RENDAH) pada seluruh 59 requirements

4. **RTM Update:** Traceability Matrix dengan kolom Metode Verifikasi dan Prioritas

## Langkah Kerja Berikutnya

1. [ ] Review keseluruhan dokumen SRS oleh stakeholder
2. [ ] Validasi seluruh 59 requirements dengan tim development
3. [ ] Legal review kepatuhan regulasi (UU ITE, UU PDP, POJK)
4. [ ] Persiapan User Acceptance Testing (UAT)
5. [ ] Transisi dokumen ke fase technical design/development

## Catatan Progress Enhancement

| Enhancement | Status | Tanggal |
|-------------|--------|---------|
| State Machine Diagram (Mermaid) | ✅ Selesai | 13 Juni 2026 |
| Use Case Naratif UC-01 (Mermaid Flowchart) | ✅ Selesai | 13 Juni 2026 |
| Use Case Naratif UC-03 (Mermaid Flowchart) | ✅ Selesai | 13 Juni 2026 |
| Use Case Naratif UC-09 (Mermaid Flowchart + Gantt) | ✅ Selesai | 13 Juni 2026 |
| Data Dictionary (7 entitas) | ✅ Selesai | 13 Juni 2026 |
| Prioritas Requirements | ✅ Selesai | 13 Juni 2026 |
| RTM Update | ✅ Selesai | 13 Juni 2026 |

## Format Dokumentasi SRS

- **Bab 1-2:** Pendahuluan & Deskripsi Umum (Use Case Diagram, State Machine)
- **Bab 3-4:** Kebutuhan Spesifik & Non-Fungsional (Kamus Data, ERD)
- **Bab 5:** Verifikasi & Traceability (RTM Matrix)
- **Total: 59 requirements** (47 functional + 12 non-functional)
- **Standar:** ISO/IEC/IEEE 29148:2018
