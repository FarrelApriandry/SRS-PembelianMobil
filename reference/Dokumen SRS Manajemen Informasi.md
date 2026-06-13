1. **PENDAHULUAN**  
1. **Tujuan**

   Dokumen Software Requirements Specification ini bertujuan untuk mendefinisikan kebutuhan fungsional dan non-fungsional dalam pengembangan aplikasi **GarasiGo,** yaitu sistem digital untuk mendukung proses pembelian mobil di Indonesia, baik mobil baru maupun mobil bekas.

   Aplikasi ini dirancang untuk membantu pengguna dalam mencari kendaraan, melihat estimasi harga, menghitung pajak dan biaya administrasi, mengajukan pembelian tunai maupun kredit, mengunggah dokumen, melakukan verifikasi legalitas kendaraan, hingga memantau proses serah terima unit. Sistem ini juga mendukung kebutuhan pihak dealer dalam mengelola stok kendaraan, transaksi, dokumen legalitas, pembiayaan, dan status pengiriman kendaraan.

   Tujuan utama aplikasi ini adalah menciptakan proses pembelian mobil yang lebih transparan, aman, terdigitalisasi, dan sesuai dengan regulasi transaksi kendaraan bermotor di Indonesia.

   

2. **Audiens yang Dituju**

   Dokumen SRS ini ditujukan untuk:

1. Tim Pengembang Perangkat Lunak  
2. Quality Assurance Engineer  
3. Product Owner atau Project Manager  
4. Dealer atau Admin Penjualan  
5. Pengguna Akhir atau Konsumen  
3. **Ruang Lingkup**

   Ruang lingkup pengembangan aplikasi **GarasiGo** mencakup sistem pembelian mobil berbasis web dan mobile yang menghubungkan konsumen, dealer, admin, lembaga pembiayaan, dan pihak eksternal terkait.

   

   Fitur utama yang dikembangkan meliputi:

1. Katalog dan Pencarian Mobil  
2. Simulasi Harga dan Pajak Kendaraan  
3. Pengajuan Pembelian Tunai dan Kredit  
4. Manajemen Dokumen Legalitas  
5. Verifikasi Kendaraan Bekas atau Tukar Tambah  
6. Pre-Delivery Inspection dan Serah Terima Unit  
7. Tracking Status Transaksi  
8. Integrasi API Eksternal

   

   

4. **Definisi**

   

| Istilah | Definisi |
| :---- | :---- |
| SRS | Software Requirements Specification, yaitu dokumen yang menjelaskan kebutuhan perangkat lunak. |
| Dealer | Pihak penjual kendaraan yang mengelola stok, transaksi, dokumen, dan serah terima kendaraan. |
| Konsumen | Pengguna yang mencari, memesan, dan membeli mobil melalui aplikasi. |
| SPK | Surat Pemesanan Kendaraan sebagai dokumen awal transaksi pembelian mobil. |
| NJKB | Nilai Jual Kendaraan Bermotor yang menjadi dasar perhitungan pajak kendaraan. |
| PPN | Pajak Pertambahan Nilai yang dikenakan pada transaksi kendaraan. |
| PPnBM | Pajak Penjualan atas Barang Mewah. |
| PKB | Pajak Kendaraan Bermotor. |
| BBNKB | Bea Balik Nama Kendaraan Bermotor. |
| Opsen | Tambahan pungutan atas PKB atau BBNKB sesuai ketentuan pemerintah daerah. |
| SWDKLLJ | Sumbangan Wajib Dana Kecelakaan Lalu Lintas Jalan. |
| STNK | Surat Tanda Nomor Kendaraan. |
| TNKB | Tanda Nomor Kendaraan Bermotor. |
| BPKB | Buku Pemilik Kendaraan Bermotor. |
| PDI | Pre-Delivery Inspection, pemeriksaan kendaraan sebelum diserahkan kepada konsumen. |
| STCK | Surat Tanda Coba Kendaraan untuk penggunaan sementara sebelum STNK/TNKB asli selesai. |
| Fidusia | Jaminan hukum dalam pembiayaan kredit kendaraan. |
| API | Application Programming Interface untuk integrasi antar sistem. |

   

5. **Referensi**  
1. IEEE Recommended Practice for Software Requirements Specifications.  
2. Regulasi pajak kendaraan bermotor di Indonesia.  
3. Regulasi pembiayaan kendaraan oleh OJK.  
4. Regulasi dokumen kendaraan bermotor oleh Polri atau Samsat.  
5. Dokumen Analisis Sistem Transaksi, Struktur Fiskal, dan Manajemen Dokumen Kendaraan Bermotor di Indonesia.

   

6. **Ikhtisar Dokumen**

   Dokumen SRS ini disusun menjadi empat bagian utama:

1. BAB 1 Pendahuluan

   Mencakup tujuan, audiens, ruang lingkup, definisi, referensi, dan sistematika penulisan untuk memberikan pemahaman dasar tentang dokumen dan proyek pengembangan.

2. BAB 2 Deskripsi Umum

   Menyajikan perspektif produk, kegunaan, karakteristik pengguna, batasan-batasan, serta asumsi, dan ketergantungan dalam pengembangan.

3. BAB 3 Spesifikasi Kebutuhan

   Berisi kebutuhan fungsional, kebutuhan non-fungsional, kebutuhan antarmuka, kebutuhan performansi, kebutuhan keamanan, dan regulasi.

4. BAB 4 Desain dan Perancangan Sistem

   Mencakup rancangan ERD, alur informasi, manajemen informasi, dan rancangan tampilan aplikasi.

   

2. **DESKRIPSI UMUM**  
1. **Perspektif Produk**

   GarasiGo merupakan aplikasi pembelian mobil berbasis web dan mobile yang menghubungkan konsumen dengan dealer kendaraan. Aplikasi ini tidak hanya berfungsi sebagai katalog mobil, tetapi juga sebagai sistem transaksi yang mendukung proses pemesanan, simulasi biaya, pembayaran, verifikasi dokumen, pembiayaan, inspeksi kendaraan, dan serah terima unit.

   Dari perspektif bisnis, sistem ini membantu dealer mempercepat proses administrasi pembelian mobil dan mengurangi risiko kesalahan pencatatan biaya. Dari perspektif konsumen, sistem ini memberikan transparansi harga karena pengguna dapat melihat estimasi harga kendaraan beserta pajak dan biaya administrasi secara lebih jelas.

   Dari perspektif teknologi, sistem ini membutuhkan integrasi dengan database kendaraan, payment gateway, layanan pembiayaan, API verifikasi identitas, API perpajakan kendaraan, serta sistem administrasi internal dealer.

   	

2. **Kegunaan**  
1. Bagi Konsumen

   	Memudahkan konsumen dalam mencari mobil, membandingkan harga, melihat simulasi biaya, mengajukan kredit, mengunggah dokumen, melakukan pembayaran, dan memantau status pembelian.

2. Bagi Admin Dealer

   	Membantu admin mengelola stok kendaraan, memvalidasi dokumen konsumen, membuat SPK, memproses pembayaran, mengatur PDI, dan mengelola status pengiriman kendaraan.

3. Bagi Sales

   	Membantu sales mengelola prospek konsumen, memberikan penawaran, membuat simulasi kredit, dan memantau progres transaksi.

4. Bagi Leasing atau Multifinance

   	Menyediakan data pengajuan kredit, dokumen pendukung, status pembayaran DP, dan informasi kendaraan sebagai objek pembiayaan.

5. Bagi Manajemen Dealer

   	Menyediakan laporan penjualan, laporan pembayaran, laporan stok, laporan status dokumen, dan rekap performa penjualan.

   

3. **Karakteristik Pengguna**

   

| Pengguna | Tugas | Hak Akses |
| :---- | :---- | :---- |
| Konsumen | Mencari mobil, melihat detail, simulasi harga, mengajukan pembelian, upload dokumen, pembayaran. | Katalog, simulasi biaya, booking, pembayaran, tracking status. |
| Sales Dealer | Membantu konsumen, membuat penawaran, mengelola prospek. | Data calon pembeli, SPK awal. |
| Admin Dealer | Validasi dokumen, kelola transaksi, kelola stok, update status. | Dashboard admin, data kendaraan, data transaksi, dokumen. |
| Admin Keuangan | Verifikasi pembayaran dan rekonsiliasi biaya. | Data pembayaran, invoice, laporan keuangan. |
| Petugas PDI | Melakukan checklist pemeriksaan kendaraan. | Modul PDI dan status kesiapan unit. |
| Leasing atau Multifinance | Memproses pengajuan kredit. | Data pengajuan kredit dan dokumen konsumen. |
| Super Admin | Mengelola sistem, user, role, master data. | Seluruh akses sistem. |

   

4. **Batasan-Batasan**  
1. **Batasan Teknis**  
1. Sistem membutuhkan koneksi internet stabil untuk mengakses katalog, pembayaran, tracking transaksi, dan integrasi API.  
2. Validasi data eksternal bergantung pada ketersediaan layanan API pihak ketiga.  
3. Perhitungan pajak kendaraan dapat berbeda antar wilayah sehingga sistem perlu menyediakan konfigurasi tarif berdasarkan provinsi/kabupaten/kota.  
4. Sistem harus mampu mengelola data sensitif seperti NIK, KTP, KK, NPWP, dokumen kendaraan, dan data pembiayaan.

   

2. **Batasan Operasional**  
1. Proses pengiriman kendaraan tetap bergantung pada kesiapan unit fisik di dealer.  
2. Dokumen STNK, TNKB, dan BPKB membutuhkan proses administrasi eksternal di Samsat/Polri.  
3. Proses kredit bergantung pada keputusan lembaga pembiayaan.  
4. Pemeriksaan mobil bekas membutuhkan inspeksi fisik secara langsung.

   

3. **Batasan Regulasi**  
1. Sistem harus mengikuti regulasi pajak kendaraan bermotor di Indonesia.  
2. Sistem harus mendukung pemisahan komponen pajak pusat, pajak daerah, opsen, PNBP, dan SWDKLLJ.  
3. Sistem harus mendukung ketentuan pembiayaan kendaraan, termasuk DP minimum dan jaminan fidusia.  
4. Sistem harus menjaga keamanan dan kerahasiaan data pribadi pengguna.

   

4. **Batasan Finansial**  
1. Integrasi API eksternal dan payment gateway membutuhkan biaya implementasi.  
2. Dealer membutuhkan biaya pelatihan admin dan sales.  
3. Sistem membutuhkan biaya server, maintenance, keamanan data, dan pembaruan regulasi.

   yan

5. **Asumsi dan Ketergantungan**  
1. Dealer menyediakan data kendaraan secara akurat dan berkala.  
2. Konsumen mengunggah dokumen pribadi dan dokumen kendaraan yang valid.  
3. Payment gateway dapat memproses pembayaran secara real-time.  
4. Lembaga pembiayaan menyediakan layanan API atau mekanisme pengiriman data kredit.  
5. Sistem Samsat/Bapenda dapat digunakan untuk validasi data pajak atau kendaraan jika integrasi tersedia.  
6. Admin dealer memperbarui status transaksi sesuai kondisi aktual.  
7. Petugas PDI mengisi checklist kendaraan sebelum unit diserahkan.

   

3. **SPESIFIKASI KEBUTUHAN**  
1. **Kebutuhan Fungsional**

   	Kebutuhan fungsional merupakan kebutuhan yang menjelaskan fungsi-fungsi utama yang harus dimiliki oleh sistem agar dapat berjalan sesuai tujuan pengembangan. Pada aplikasi **GarasiGo**, kebutuhan fungsional berfokus pada proses pencarian mobil, simulasi biaya, pemesanan kendaraan, pengajuan kredit, pembayaran, validasi dokumen, hingga serah terima kendaraan. 

   

1. **Deskripsi Kebutuhan Fungsional**

   

| ID | Kebutuhan Fungsional | Prioritas | Deskripsi |
| :---- | :---- | :---- | :---- |
| FR001 | Registrasi dan Login | Tinggi | Pengguna dapat membuat akun dan masuk ke sistem menggunakan email, nomor telepon, atau akun pihak ketiga.  |
| FR002 | Manajemen Profil Pengguna | Tinggi | Pengguna dapat mengelola data pribadi seperti nama, NIK, alamat, nomor telepon, dan dokumen identitas.  |
| FR003 | Katalog Mobil | Tinggi | Sistem menampilkan daftar mobil baru dan bekas yang tersedia di dealer.  |
| FR004 | Pencarian dan Filter Mobil | Tinggi | Pengguna dapat mencari mobil berdasarkan merek, model, harga, tahun, transmisi, lokasi dealer, dan status kendaraan.  |
| FR005 | Detail Kendaraan | Tinggi | Sistem menampilkan informasi detail mobil seperti spesifikasi, harga, warna, stok, promo, nomor rangka, nomor mesin, dan lokasi dealer.  |
| FR006 | Simulasi Pajak dan Total Biaya Kendaraan | Tinggi | Sistem dapat menghitung estimasi total biaya pembelian mobil berdasarkan NJKB, PPN, PPnBM, PKB, BBNKB, opsen, SWDKLLJ, biaya STNK, TNKB, BPKB, dan biaya administrasi.  |
| FR007 | Validasi Pajak Progresif | Tinggi | Sistem dapat memperkirakan pajak progresif berdasarkan urutan kepemilikan kendaraan pengguna melalui data NIK atau KK.  |
| FR008 | Booking Unit Kendaraan | Tinggi | Pengguna dapat melakukan pemesanan unit kendaraan dengan membayar booking fee.  |
| FR009 | Pembuatan SPK Digital | Tinggi | Sistem dapat membuat Surat Pemesanan Kendaraan berdasarkan data pengguna, kendaraan, harga, dan metode pembayaran.  |
| FR010 | Upload Dokumen Konsumen | Tinggi | Pengguna dapat mengunggah dokumen seperti KTP, KK, NPWP, slip gaji, rekening koran, atau dokumen pendukung lain.  |
| FR011 | Validasi Dokumen | Tinggi | Admin dapat memeriksa kelengkapan dan keabsahan dokumen konsumen maupun dokumen kendaraan.  |
| FR012 | Pembelian Tunai | Tinggi | Sistem dapat memproses pembelian mobil secara tunai mulai dari invoice, pembayaran, hingga status lunas.  |
| FR013 | Pengajuan Kredit | Tinggi | Pengguna dapat mengajukan pembelian kredit dengan memilih DP, tenor, leasing, dan melengkapi dokumen pembiayaan.  |
| FR014 | Simulasi Cicilan Kredit | Tinggi | Sistem dapat menghitung estimasi cicilan berdasarkan harga kendaraan, DP, tenor, bunga, asuransi, dan biaya administrasi kredit.  |
| FR015 | Verifikasi Kredit | Tinggi | Sistem dapat mengirim data pengajuan kredit kepada pihak leasing atau multifinance untuk proses persetujuan.  |
| FR016 | Pembayaran Terintegrasi | Tinggi | Sistem mendukung pembayaran melalui transfer bank, virtual account, e-wallet, atau payment gateway.  |
| FR017 | Rekonsiliasi Pembayaran | Tinggi | Sistem dapat mencatat pembayaran dan memisahkan komponen harga kendaraan, pajak, biaya administrasi, dan biaya pihak ketiga.  |
| FR018 | Manajemen Mobil Bekas/Tukar Tambah | Sedang | Pengguna dapat mengajukan mobil lama untuk inspeksi dan estimasi harga tukar tambah.  |
| FR019 | Inspeksi Kendaraan Bekas | Sedang | Petugas dapat mengisi checklist kondisi fisik, dokumen, pajak, nomor rangka, dan nomor mesin kendaraan bekas.  |
| FR020 | Pre-Delivery Inspection/PDI | Tinggi | Petugas dealer dapat mengisi checklist pemeriksaan kendaraan sebelum unit diserahkan kepada konsumen.  |
| FR021 | Tracking Status Transaksi | Tinggi | Pengguna dapat memantau status transaksi mulai dari booking, verifikasi dokumen, pembayaran, kredit, PDI, hingga serah terima.  |
| FR022 | Pengelolaan STCK | Sedang | Sistem dapat mencatat kebutuhan STCK atau pelat sementara sebelum STNK dan TNKB asli selesai.  |
| FR023 | Serah Terima Kendaraan | Tinggi | Sistem dapat menghasilkan berita acara serah terima dan mencatat status kendaraan telah diterima konsumen.  |
| FR024 | Notifikasi Real-time | Sedang | Sistem mengirim notifikasi terkait pembayaran, dokumen, kredit, PDI, pengiriman, dan status transaksi.  |
| FR025 | Dashboard Admin | Tinggi | Admin dapat mengelola data kendaraan, transaksi, dokumen, pembayaran, dan laporan penjualan. |

   

2. **Use Case Diagram**

   

3. **Fitur Simulasi Pajak dan Total Biaya Kendaraan**  
1. **Skenario: Menghitung estimasi total biaya pembelian mobil**  
1) **Deskripsi**

   	Fitur ini memungkinkan pengguna melihat estimasi total biaya pembelian mobil sebelum melanjutkan transaksi. Sistem menghitung biaya berdasarkan harga kendaraan, NJKB, jenis kendaraan, wilayah registrasi, pajak pusat, pajak daerah, opsen, SWDKLLJ, serta biaya penerbitan dokumen kendaraan. Fitur ini penting karena transaksi pembelian mobil di Indonesia memiliki struktur pajak berlapis, termasuk pajak pusat, pajak daerah, opsen, PNBP, dan SWDKLLJ. 

   	

2) **Kondisi Awal**  
* Pengguna telah memilih mobil.  
* Sistem memiliki data harga kendaraan dan NJKB.  
* Sistem memiliki konfigurasi tarif pajak berdasarkan wilayah.  
* Pengguna memilih lokasi registrasi kendaraan.

3) **Alur Kejadian Utama**  
1. Pengguna membuka halaman detail mobil.  
2. Pengguna menekan tombol **Simulasi Biaya**.  
3. Sistem mengambil data harga kendaraan dan NJKB.  
4. Sistem menghitung komponen pajak dan biaya administrasi.  
5. Sistem menampilkan rincian biaya kepada pengguna.  
6. Pengguna dapat menyimpan simulasi atau melanjutkan ke booking unit.

   

4) **Rumus Perhitungan**

   

| Komponen | Rumus |
| :---- | :---- |
| DPP PPN Nilai Lain | 11/12 × NJKB  |
| PPN Terutang | 12% × DPP PPN Nilai Lain  |
| PPN Sederhana | 11% × NJKB  |
| PPnBM | Tarif PPnBM × DPP PPnBM  |
| PKB Pokok | Tarif PKB × NJKB × Bobot Kendaraan  |
| Opsen PKB | 66% × PKB Pokok  |
| Total PKB | PKB Pokok \+ Opsen PKB  |
| BBNKB Pokok | Tarif BBNKB × NJKB  |
| Opsen BBNKB | 66% × BBNKB Pokok  |
| Total BBNKB | BBNKB Pokok \+ Opsen BBNKB  |
| Total Pajak dan Biaya | PPN \+ PPnBM \+ PKB Pokok \+ Opsen PKB \+ BBNKB Pokok \+ Opsen BBNKB \+ SWDKLLJ \+ STNK \+ TNKB \+ BPKB  |
| Total Transaksi Akhir | Harga Kendaraan \+ Total Pajak dan Biaya Administrasi  |

5) **Kondisi Akhir**  
* Sistem menampilkan estimasi total biaya pembelian mobil.  
* Rincian pajak dan biaya tersimpan dalam data simulasi.  
* Pengguna dapat melanjutkan proses booking atau mengubah parameter simulasi.


4. **Fitur Validasi Pajak Progresif**  
1. **Skenario: Mengecek estimasi pajak progresif kendaraan**  
1) **Deskripsi**

   	Fitur ini digunakan untuk memperkirakan tarif pajak progresif berdasarkan urutan kepemilikan kendaraan. Sistem akan mengecek apakah pengguna telah memiliki kendaraan roda empat lain berdasarkan NIK atau KK. Pajak progresif perlu dipertimbangkan karena referensi menjelaskan bahwa pajak progresif dikenakan kepada individu yang memiliki lebih dari satu kendaraan roda empat atas nama dan alamat yang sama.

   

2) **Kondisi Awal**  
* Pengguna telah mengisi NIK dan alamat.  
* Pengguna memilih kendaraan yang akan dibeli.  
* Sistem memiliki akses atau data referensi kepemilikan kendaraan.


3) **Alur Kejadian Utama**  
1. Sistem membaca NIK atau KK pengguna.  
2. Sistem mengecek jumlah kendaraan roda empat yang sudah terdaftar.  
3. Sistem menentukan urutan kepemilikan kendaraan.  
4. Sistem menyesuaikan tarif PKB progresif.  
5. Sistem menampilkan estimasi pajak progresif kepada pengguna.  
6. Sistem menyimpan hasil estimasi dalam simulasi biaya.

   

4) **Rumus Pajak Progresif**

   

| Komponen | Rumus |
| :---- | :---- |
| NJKB | PKB Pokok / Tarif Berlaku  |
| PKB Progresif | Tarif Progresif × NJKB × Bobot Kendaraan  |
| Opsen PKB Progresif | 66% × PKB Progresif  |
| Total PKB Progresif | PKB Progresif \+ Opsen PKB Progresif \+ SWDKLLJ  |

   

5) **Kondisi Akhir**  
* Sistem menampilkan estimasi pajak progresif.  
* Sistem memberi informasi bahwa hasil akhir tetap mengikuti validasi resmi Samsat/Bapenda.  
* Data simulasi pajak tersimpan dalam transaksi.


5. **Fitur Pembelian Mobil Kredit**  
1. **Skenario: Mengajukan pembelian mobil secara kredit**  
1) **Deskripsi**

   	Fitur ini memungkinkan pengguna membeli mobil melalui skema kredit. Pengguna dapat memilih leasing, menentukan DP, memilih tenor, melihat estimasi cicilan, mengunggah dokumen pendukung, dan memantau status persetujuan kredit. 

   

2) **Kondisi Awal**  
* Pengguna telah login.  
* Pengguna telah memilih mobil.  
* Pengguna memilih metode pembayaran kredit.  
* Pengguna menyiapkan dokumen pendukung.


3) **Alur Kejadian Utama**  
1. Pengguna memilih opsi **Pembelian Kredit**.  
2. Sistem menampilkan pilihan DP, tenor, bunga, dan leasing.  
3. Pengguna memilih skema kredit.  
4. Sistem menghitung estimasi cicilan.  
5. Pengguna mengunggah dokumen seperti KTP, KK, NPWP, slip gaji, dan rekening koran.  
6. Admin memvalidasi kelengkapan dokumen.  
7. Sistem mengirim data pengajuan ke leasing.  
8. Leasing memberikan status pengajuan: disetujui, ditolak, atau butuh revisi.  
9. Jika disetujui, pengguna melakukan pembayaran DP.  
10. Sistem memperbarui status transaksi.

    

4) **Kondisi Akhir**  
* Pengajuan kredit tercatat dalam sistem.  
* Status kredit dapat dilihat oleh pengguna.  
* Jika disetujui, transaksi dilanjutkan ke pembayaran DP dan proses administrasi kendaraan.


6. **Fitur Validasi Dokumen dan PDI**  
1. **Skenario: Validasi dokumen dan pemeriksaan kendaraan sebelum serah terima kendaraan**  
1) **Deskripsi**

   	Fitur ini digunakan untuk memastikan dokumen konsumen dan kendaraan telah lengkap sebelum kendaraan diserahkan. Untuk mobil baru, sistem memeriksa dokumen pembelian dan administrasi kendaraan. Untuk mobil bekas, sistem juga memeriksa STNK, BPKB, faktur, nomor rangka, nomor mesin, status pajak, dan status kredit aktif. 

   

2) **Kondisi Awal**  
* Pengguna telah melakukan booking atau pembayaran.  
* Dokumen konsumen telah diunggah.  
* Data kendaraan telah tersedia di sistem.  
* Petugas PDI memiliki akses ke checklist kendaraan.


3) **Alur Kejadian Utama**  
1. Admin membuka data transaksi.  
2. Admin memeriksa dokumen konsumen.  
3. Admin memeriksa dokumen kendaraan.  
4. Jika dokumen belum lengkap, sistem mengirim notifikasi revisi kepada pengguna.  
5. Jika dokumen lengkap, transaksi dilanjutkan ke tahap PDI.  
6. Petugas PDI memeriksa kondisi kendaraan.  
7. Petugas mengisi checklist PDI pada sistem.  
8. Jika kendaraan lolos PDI, status unit berubah menjadi **Siap Serah Terima**.  
9. Sistem menghasilkan berita acara serah terima kendaraan.

   

4) **Kondisi Akhir**  
* Dokumen transaksi telah tervalidasi.  
* Hasil PDI tersimpan dalam sistem.  
* Kendaraan siap diserahkan kepada konsumen.


2. **Kebutuhan Non-Fungsional**  
1. **Deskripsi Kebutuhan Non-Fungsional**

   

| ID | Kebutuhan Non-Fungsional | Deskripsi |
| :---- | :---- | :---- |
| NFR001 | Keamanan Data | Sistem harus melindungi data pribadi pengguna, dokumen identitas, dan dokumen kendaraan.  |
| NFR002 | Autentikasi dan Otorisasi | Sistem harus membatasi akses berdasarkan role pengguna.  |
| NFR003 | Akurasi Perhitungan | Sistem harus menghitung pajak, cicilan, dan total biaya sesuai konfigurasi tarif yang berlaku. |
| NFR004 | Ketersediaan Sistem | Sistem dapat diakses 24/7 kecuali saat maintenance.  |
| NFR005 | Kemudahan Penggunaan | Tampilan sistem harus mudah dipahami oleh konsumen, admin, sales, dan petugas dealer.  |
| NFR006 | Audit Trail | Sistem harus mencatat riwayat perubahan data penting seperti harga, pembayaran, validasi dokumen, dan status transaksi.  |
| NFR007 | Skalabilitas | Sistem harus mampu menangani peningkatan jumlah pengguna, kendaraan, dan transaksi. |
| NFR008 | Backup Data | Sistem harus melakukan pencadangan data secara berkala.  |

   

2. **Antarmuka Penggguna**  
1. **User-Centered Design Principles**

   	Aplikasi GarasiGo mengadopsi prinsip user-centered design untuk memastikan sistem mudah digunakan oleh berbagai jenis pengguna, seperti konsumen, sales dealer, admin dealer, admin keuangan, petugas PDI, leasing, dan super admin. Antarmuka dirancang agar proses pembelian mobil dapat dilakukan secara jelas, bertahap, dan mudah dipahami, mulai dari pencarian mobil, simulasi biaya, pengajuan pembelian, pembayaran, hingga pelacakan status transaksi. 

    

2. **Design System**  
* **Color Palette:** Menggunakan warna yang profesional dan konsisten dengan tema otomotif, seperti warna netral, biru, abu-abu, putih, atau warna aksen yang menunjukkan status transaksi.  
* **Typography:** Menggunakan jenis dan ukuran font yang mudah dibaca pada perangkat mobile maupun web.  
* **Iconography:** Menggunakan ikon universal yang mudah dipahami, seperti ikon mobil untuk katalog, kalkulator untuk simulasi biaya, dokumen untuk upload berkas, dompet untuk pembayaran, dan checklist untuk PDI.  
* **Status Indicator:** Sistem menggunakan indikator status yang jelas, seperti “Menunggu Pembayaran”, “Dokumen Diverifikasi”, “Kredit Diproses”, “Unit Siap PDI”, dan “Siap Serah Terima”.  
* **Form Design:** Form input dibuat sederhana dan terstruktur agar pengguna mudah mengisi data pribadi, data kendaraan, dokumen, dan informasi pengajuan kredit.


3. **Navigation Structure**  
* Struktur navigasi dibuat jelas dan predictable agar pengguna dapat memahami posisi mereka di dalam aplikasi.  
* Menu utama untuk konsumen terdiri dari Beranda, Katalog Mobil, Simulasi Biaya, Transaksi Saya, Notifikasi, dan Profil.  
* Menu utama untuk admin terdiri dari Dashboard, Data Kendaraan, Transaksi, Dokumen, Pembayaran, PDI, Pengiriman, dan Laporan.  
* Sistem menyediakan breadcrumb navigation pada versi web untuk membantu admin mengetahui posisi halaman yang sedang diakses.  
* Sistem menyediakan progress step pada proses pembelian, seperti Pilih Mobil → Simulasi Biaya → Booking → Upload Dokumen → Pembayaran → PDI → Serah Terima.


4. **Responsive Interface**  
* Antarmuka aplikasi harus dapat menyesuaikan ukuran layar pada smartphone, tablet, laptop, dan komputer.  
* Tampilan mobile difokuskan untuk konsumen yang melakukan pencarian mobil, simulasi biaya, upload dokumen, pembayaran, dan tracking transaksi.  
* Tampilan web difokuskan untuk admin, sales, finance, dan petugas dealer dalam mengelola data kendaraan, transaksi, dokumen, pembayaran, dan laporan.


5. **Accessibility Support**  
* Sistem harus menyediakan tampilan yang mudah dibaca dengan kontras warna yang baik.  
* Tombol dan menu harus memiliki ukuran yang cukup besar agar mudah digunakan pada perangkat mobile.  
* Informasi penting seperti total biaya, status transaksi, dan peringatan dokumen harus ditampilkan secara jelas.  
* Pesan error harus menggunakan bahasa yang mudah dipahami pengguna, misalnya “Dokumen KTP belum diunggah” atau “Pembayaran belum berhasil diverifikasi”.


3. **Antarmuka Perangkat Keras**  
1. **Spesifikasi Minimum Android**  
* RAM: minimal 3 GB.  
* Storage: minimal tersedia 200 MB.  
* Processor: Quad-core 1.8 GHz atau setara.  
* Sistem operasi: Android 8.0 atau lebih baru.  
* Kamera belakang untuk mengambil foto dokumen, bukti pembayaran, kendaraan, dan dokumentasi serah terima.  
* Koneksi internet minimal 4G atau WiFi untuk mengakses katalog, upload dokumen, dan pembayaran.


2. **Spesifikasi Minimum iOS**  
* Perangkat: iPhone 8 atau lebih baru.  
* RAM: minimal 2 GB.  
* Storage: minimal tersedia 200 MB.  
* Sistem operasi: iOS 13.0 atau lebih baru.  
* Dukungan kamera untuk upload dokumen, foto kendaraan, dan bukti transaksi.  
* Dukungan tampilan responsif untuk iPad.


3. **Spesifikasi Minimum Web Admin**  
* Perangkat: laptop atau komputer.  
* RAM: minimal 4 GB.  
* Browser: Google Chrome, Microsoft Edge, Mozilla Firefox, atau Safari versi terbaru.  
* Koneksi internet stabil.  
* Resolusi layar minimal 1366 × 768 piksel.  
* Mendukung penggunaan scanner dan printer untuk kebutuhan administrasi dealer.


4. **Pemanfaatan Kapabilitas Perangkat**  
* **Kamera:** Digunakan untuk mengambil foto KTP, KK, NPWP, slip gaji, STNK, BPKB, kendaraan, bukti pembayaran, dan dokumentasi serah terima.  
* **GPS:** Digunakan untuk mendeteksi lokasi pengguna, lokasi dealer terdekat, dan alamat pengiriman kendaraan.  
* **Notification System:** Digunakan untuk mengirim notifikasi terkait status transaksi, pembayaran, dokumen, kredit, PDI, dan pengiriman.  
* **File Upload:** Digunakan untuk mengunggah dokumen pribadi dan dokumen kendaraan.  
* **Printer:** Digunakan oleh pihak dealer untuk mencetak SPK, invoice, kuitansi, checklist PDI, dan berita acara serah terima.  
* **Scanner:** Digunakan oleh pihak dealer untuk memindai dokumen fisik yang dibawa oleh konsumen.


5. **Optimasi Daya dan Konektivitas**  
* Sistem harus menggunakan proses sinkronisasi data secara efisien agar tidak membebani baterai perangkat mobile.  
* Sistem harus tetap dapat menyimpan draft data sementara apabila koneksi internet pengguna tidak stabil.  
* Sistem harus memberikan pesan peringatan apabila upload dokumen gagal karena koneksi internet terputus.


4. **Antarmuka Perangkat Lunak**  
1. **Operating System Support**

   	Aplikasi GarasiGo mendukung sistem operasi Android dan iOS untuk pengguna mobile. Untuk pengguna admin, sales, finance, petugas PDI, dan super admin, sistem dapat diakses melalui web browser pada perangkat desktop atau laptop.

   

2. **Database Management System**

   	Sistem membutuhkan database untuk menyimpan data pengguna, kendaraan, transaksi, pembayaran, dokumen, simulasi biaya, pengajuan kredit, hasil PDI, notifikasi, dan laporan penjualan. Database harus mampu menyimpan data secara aman, terstruktur, dan dapat diakses sesuai hak akses pengguna. 

   

3. **Payment Gateway Integration**

   	Sistem terintegrasi dengan payment gateway untuk memproses pembayaran booking fee, down payment, pelunasan, dan biaya administrasi. Payment gateway juga digunakan untuk memperbarui status pembayaran secara otomatis melalui callback atau webhook. 

   

4. **Dukcapil API Integration**

   	Sistem dapat terhubung dengan API Dukcapil untuk melakukan validasi data identitas konsumen, seperti NIK dan KTP. Integrasi ini digunakan untuk mengurangi risiko kesalahan data atau pemalsuan identitas dalam proses pembelian kendaraan. 

   

5. **Samsat atau Bapenda API Integration**

   	Sistem dapat terhubung dengan API Samsat atau Bapenda untuk membantu proses pengecekan data pajak kendaraan, status kendaraan, validasi STNK, serta perhitungan pajak progresif berdasarkan data kepemilikan kendaraan pengguna. 

   

6. **Leasing atau Multifinance API Integration**

   	Sistem dapat terhubung dengan sistem leasing atau multifinance untuk mengirim data pengajuan kredit kendaraan. Data yang dikirim meliputi data konsumen, data kendaraan, nilai DP, tenor, estimasi cicilan, dan dokumen pendukung pembiayaan. 

   

7. **Electronic Signature Integration**

   	Sistem dapat menggunakan layanan tanda tangan elektronik untuk mendukung penandatanganan dokumen digital, seperti SPK, dokumen pembiayaan, berita acara serah terima, dan dokumen persetujuan lainnya. 

   

8. **Notification Service Integration**

   Sistem menggunakan layanan notifikasi untuk mengirim informasi kepada pengguna melalui push notification, email, SMS, atau WhatsApp. Notifikasi digunakan untuk memberi tahu status pembayaran, validasi dokumen, pengajuan kredit, PDI, pengiriman, dan serah terima kendaraan. 

   

9. **Document Management System**

   	Sistem menyediakan modul manajemen dokumen untuk menyimpan, menampilkan, memvalidasi, dan mengarsipkan dokumen konsumen maupun dokumen kendaraan. Dokumen yang dikelola meliputi KTP, KK, NPWP, slip gaji, rekening koran, STNK, BPKB, faktur kendaraan, kwitansi, SPK, invoice, dan berita acara serah terima. 

   

5. **Antarmuka Komunikasi**  
1. **HTTPS Communication**

   	Seluruh komunikasi data antara pengguna, server aplikasi, dan layanan eksternal harus menggunakan protokol HTTPS untuk menjaga keamanan data yang dikirimkan. Hal ini penting karena sistem mengelola data sensitif seperti NIK, dokumen identitas, dokumen kendaraan, dan informasi pembayaran. 

   

2. **RESTful API**

   	Sistem menggunakan RESTful API untuk komunikasi antar modul, seperti modul katalog kendaraan, simulasi biaya, transaksi, pembayaran, dokumen, kredit, PDI, pengiriman, dan notifikasi. RESTful API juga digunakan untuk integrasi dengan layanan eksternal seperti payment gateway, Dukcapil, Samsat atau Bapenda, leasing, dan tanda tangan elektronik. 

   

3. **Webhook Payment Gateway**

   	Sistem menerima webhook dari payment gateway untuk memperbarui status pembayaran secara otomatis. Jika pembayaran berhasil, sistem akan mengubah status transaksi menjadi “Dibayar” atau “Menunggu Verifikasi Finance”. Jika pembayaran gagal, sistem akan menampilkan notifikasi kepada pengguna. 

   

4. **Push Notification**

   	Push notification digunakan untuk mengirim pembaruan status transaksi secara real-time kepada pengguna. Notifikasi dapat berupa informasi pembayaran berhasil, dokumen perlu diperbaiki, kredit disetujui, unit sedang PDI, kendaraan siap dikirim, atau transaksi selesai. 

   

5. **Email Gateway**

   	Email gateway digunakan untuk mengirim informasi resmi kepada pengguna, seperti bukti registrasi, invoice, SPK digital, bukti pembayaran, status pengajuan kredit, dan berita acara serah terima kendaraan.

   

6. **SMS atau WhatsApp Gateway**

   	SMS atau WhatsApp gateway digunakan untuk mengirim kode OTP, pengingat pembayaran, status dokumen, status kredit, jadwal serah terima, dan informasi penting lainnya secara cepat kepada pengguna. 

   

7. **Audit Log Communication**

   	Setiap komunikasi penting yang terjadi di dalam sistem harus dicatat dalam audit log. Data yang dicatat meliputi waktu akses, pengguna yang melakukan aksi, jenis aksi, status perubahan data, dan hasil komunikasi dengan layanan eksternal. 

   

3. **Kebutuhan Performansi**  
1. **Kinerja Umum**

   	Aplikasi GarasiGo dirancang untuk tetap stabil dalam menangani proses pembelian mobil secara digital, baik pada kondisi penggunaan normal maupun ketika terjadi peningkatan akses, seperti saat periode promo dealer, peluncuran model kendaraan baru, atau akhir bulan ketika aktivitas penjualan meningkat. Sistem harus mampu memberikan layanan yang andal tanpa penurunan kualitas yang signifikan, terutama pada fitur katalog mobil, simulasi biaya, pembayaran, upload dokumen, dan tracking status transaksi.

   

2. **Waktu Respons**

   	Waktu respons sistem ditargetkan maksimal 3 detik untuk menampilkan katalog mobil, 2 detik untuk membuka detail kendaraan, dan 5 detik untuk menampilkan hasil simulasi pajak dan total biaya pembelian. Pada proses pembayaran, sistem harus dapat menerima pembaruan status dari payment gateway secara otomatis melalui webhook dalam waktu maksimal 10 detik setelah pembayaran diproses. Untuk fitur tracking transaksi, pembaruan status seperti dokumen diverifikasi, pembayaran diterima, kredit diproses, unit PDI, dan kendaraan siap serah terima harus dapat ditampilkan secara real-time atau mendekati real-time. 

   

3. **Throughput & Skalabilitas**

   	Aplikasi harus mampu menangani minimal 1.000 pengguna aktif secara bersamaan pada tahap awal implementasi. Sistem juga harus mampu memproses minimal 100 permintaan transaksi per menit, termasuk pencarian mobil, simulasi biaya, upload dokumen, dan pengecekan status transaksi. Query database umum, seperti pencarian data kendaraan dan riwayat transaksi, ditargetkan selesai dalam waktu maksimal 1 detik. Sistem harus mendukung skalabilitas horizontal agar kapasitas server dapat ditingkatkan apabila jumlah pengguna, data kendaraan, dan transaksi mengalami pertumbuhan. 

   

4. **Optimasi Memori dan Penyimpanan**

   	Sistem harus menerapkan strategi caching untuk mempercepat akses data yang sering dibuka, seperti katalog kendaraan, daftar merek, daftar model, lokasi dealer, dan konfigurasi tarif pajak. Gambar kendaraan dan dokumen yang diunggah harus dikompresi secara otomatis tanpa mengurangi keterbacaan dokumen. Sistem juga harus membatasi ukuran file upload agar penyimpanan tetap efisien, dengan batas maksimal 10 MB per file untuk dokumen pengguna. Data transaksi, pembayaran, dokumen, dan audit trail harus disimpan secara terstruktur agar mudah dicari kembali saat dibutuhkan. 

   

5. **Keandalan Sistem**

   	Sistem harus tetap dapat menyimpan data penting meskipun terjadi gangguan koneksi sementara. Pada saat pengguna mengisi formulir pengajuan pembelian, upload dokumen, atau simulasi kredit, sistem perlu menyediakan mekanisme penyimpanan draft agar data tidak hilang. Apabila layanan eksternal seperti payment gateway, API leasing, atau API Samsat sedang tidak tersedia, sistem harus menampilkan pesan kegagalan yang jelas dan menyimpan status permintaan untuk diproses ulang. 

   

6. **Backup dan Recovery**

   	Sistem harus melakukan pencadangan data secara berkala untuk mencegah kehilangan data transaksi, dokumen, pembayaran, dan status pembelian kendaraan. Backup data dilakukan minimal satu kali setiap hari. Sistem juga harus memiliki mekanisme recovery agar data dapat dipulihkan apabila terjadi kerusakan server, kegagalan database, atau kesalahan sistem. 

   

4. **Kebutuhan Regulasi**  
1. **Standar Compliance**  
1. **Kepatuhan Keamanan Pembayaran (Payment Compliance)**

   	Aplikasi harus mematuhi standar keamanan pembayaran digital untuk menjaga keamanan data transaksi pengguna. Seluruh proses pembayaran, baik booking fee, down payment, pelunasan, maupun biaya administrasi, harus dilakukan melalui jalur yang aman. Sistem harus menggunakan enkripsi pada proses pengiriman data pembayaran dan tidak menyimpan informasi sensitif seperti PIN, CVV, atau data kartu secara langsung di server aplikasi.

   

2. **Kepatuhan Keamanan Informasi**

   	Aplikasi harus menjaga kerahasiaan, integritas, dan ketersediaan data pengguna. Data pribadi seperti NIK, KTP, KK, NPWP, alamat, nomor telepon, dokumen pendukung kredit, serta data transaksi harus dilindungi dari akses pihak yang tidak berwenang. Setiap pengguna hanya dapat mengakses data sesuai dengan role dan hak akses yang telah ditentukan. 

   

3. **Kepatuhan Perlindungan Data Pribadi**

   	Sistem harus menerapkan prinsip perlindungan data pribadi dalam pengumpulan, penyimpanan, penggunaan, dan penghapusan data pengguna. Pengguna harus mengetahui data apa saja yang dikumpulkan dan untuk tujuan apa data tersebut digunakan. Sistem juga harus menyediakan pengaturan keamanan akun, autentikasi, dan audit trail untuk mencegah penyalahgunaan data. 

   

4. **Kepatuhan Transaksi Elektronik**

   	Dokumen digital seperti SPK, invoice, bukti pembayaran, dokumen kredit, dan berita acara serah terima harus dikelola sebagai dokumen elektronik yang sah dan dapat ditelusuri. Sistem harus mencatat waktu pembuatan dokumen, pihak yang menyetujui, serta status dokumen agar dapat digunakan sebagai bukti transaksi apabila diperlukan. 

   

2. **Regulasi Pajak dan Administrasi Kendaraan**  
1. **Pajak Pusat**

   	Sistem harus mendukung perhitungan pajak pusat yang berlaku dalam transaksi pembelian mobil, seperti PPN dan PPnBM. Perhitungan pajak harus menggunakan data kendaraan, NJKB, jenis kendaraan, dan tarif yang berlaku. Sistem juga harus memungkinkan pembaruan tarif oleh admin apabila terjadi perubahan regulasi. 

   

2. **Pajak Daerah**

   	Sistem harus mendukung perhitungan pajak daerah seperti PKB, BBNKB, Opsen PKB, dan Opsen BBNKB. Karena tarif pajak dapat berbeda berdasarkan wilayah, sistem harus menyediakan konfigurasi tarif berdasarkan provinsi atau daerah registrasi kendaraan. Sistem juga harus mampu menampilkan rincian pajak secara terpisah agar proses pencatatan dan rekonsiliasi lebih jelas. 

   

3. **Pajak Progresif Kendaraan**

   	Sistem harus mendukung estimasi pajak progresif berdasarkan urutan kepemilikan kendaraan pengguna. Validasi dilakukan berdasarkan data NIK, KK, dan alamat pengguna apabila integrasi dengan sistem terkait tersedia. Hasil perhitungan pajak progresif pada aplikasi bersifat estimasi dan dapat berubah sesuai hasil validasi resmi dari Samsat atau Bapenda. 

   

4. **PNBP, SWDKLLJ, STNK, TNKB, dan BPKB**

   	Sistem harus mencatat biaya administrasi kendaraan seperti SWDKLLJ, penerbitan STNK, penerbitan TNKB, penerbitan BPKB, serta biaya administrasi lain yang berkaitan dengan proses legalisasi kendaraan. Setiap komponen biaya harus ditampilkan secara transparan kepada pengguna pada halaman simulasi total biaya. 

   

5. **STCK dan Plat Nomor Sementara**

   	Untuk kendaraan baru yang belum memiliki STNK dan TNKB asli, sistem harus mendukung pencatatan penggunaan STCK atau plat nomor sementara. Sistem harus menampilkan informasi bahwa STCK hanya digunakan untuk keperluan terbatas, seperti pengiriman kendaraan dari dealer ke konsumen atau keperluan administrasi tertentu, bukan untuk penggunaan kendaraan pribadi secara bebas. 

   

3. **Regulasi Pembiayaan Kendaraan**  
1. **Pengajuan Kredit Kendaraan**

   	Sistem harus mendukung proses pengajuan kredit kendaraan sesuai ketentuan lembaga pembiayaan atau leasing. Data yang dikelola meliputi data konsumen, data kendaraan, nilai down payment, tenor, estimasi cicilan, bunga, asuransi, dan dokumen pendukung pengajuan kredit. 

   

2. **Validasi Kelayakan Kredit**

   	Sistem dapat terintegrasi dengan layanan pengecekan kredit seperti OJK iDebku atau PEFINDO apabila tersedia. Validasi ini digunakan untuk membantu pihak leasing dalam menilai kelayakan kredit calon pembeli berdasarkan riwayat fasilitas kredit, performa pembayaran, dan status kolektibilitas. 

   

3. **Ketentuan Down Payment**

   	Sistem harus menyediakan konfigurasi batas minimum down payment sesuai kebijakan lembaga pembiayaan. Besaran DP dapat berbeda berdasarkan jenis kendaraan, status kendaraan, kebijakan leasing, dan kondisi risiko pembiayaan. Sistem harus menolak pengajuan kredit apabila nilai DP yang dipilih tidak memenuhi batas minimum yang telah ditentukan. 

   

4. **Regulasi Fidusia**  
1. **Pendaftaran Jaminan Fidusia**

   	Untuk transaksi pembelian mobil secara kredit, sistem harus mendukung proses pendaftaran jaminan fidusia. Data transaksi kredit, data kendaraan, data debitur, dan data leasing harus dapat digunakan sebagai dasar pembuatan dokumen fidusia. 

   

2. **Integrasi AHU Fidusia Online**

   	Sistem dapat terhubung dengan layanan AHU Fidusia Online untuk membantu proses pendaftaran jaminan fidusia secara elektronik. Integrasi ini digunakan untuk mengirim data fidusia, memperoleh kode bayar PNBP, dan mencatat status sertifikat fidusia elektronik. 

   

3. **Batas Waktu Pendaftaran Fidusia**

   	Sistem harus menyediakan pengingat atau notifikasi kepada pihak dealer, leasing, atau admin apabila pendaftaran fidusia belum dilakukan. Hal ini bertujuan agar proses pendaftaran dilakukan sesuai batas waktu yang berlaku dan tidak menghambat legalitas pembiayaan kendaraan.

   

5. **Regulasi Dokumen Kendaraan**  
1. **Dokumen Kendaraan Baru**

   	Sistem harus mendukung pengelolaan dokumen kendaraan baru seperti faktur kendaraan, invoice, kuitansi pembayaran, STCK, polis asuransi, buku garansi, buku manual kendaraan, STNK, TNKB, dan BPKB. Dokumen harus dapat dilacak statusnya mulai dari proses pengajuan hingga selesai diterima konsumen. 

   

2. **Dokumen Kendaraan Bekas**

   	Untuk transaksi mobil bekas atau tukar tambah, sistem harus mendukung validasi dokumen seperti STNK, BPKB, faktur kendaraan, kwitansi pembelian, nomor rangka, nomor mesin, status pajak, dan status kredit aktif. Jika kendaraan berasal dari perusahaan, impor, lelang, atau masih dalam masa kredit, sistem harus menyediakan kolom dokumen tambahan yang sesuai. 

   

3. **Validasi Legalitas Kendaraan**

   	Sistem harus membantu proses validasi legalitas kendaraan untuk mengurangi risiko transaksi kendaraan bermasalah. Validasi dapat mencakup pengecekan kesesuaian nomor rangka dan nomor mesin, masa berlaku STNK, status pajak, status blokir, dan kelengkapan dokumen kepemilikan. 

   

6. **Lisensi dan Keamanan**  
1. **Lisensi Perangkat Lunak**

   	Setiap library, framework, API, dan layanan pihak ketiga yang digunakan dalam aplikasi harus memiliki lisensi yang jelas dan sesuai dengan kebutuhan pengembangan sistem. Penggunaan layanan pihak ketiga harus disertai dokumentasi teknis dan ketentuan penggunaan yang berlaku. 

   

2. **Keamanan Akses Sistem**

   	Sistem harus menerapkan role-based access control agar setiap pengguna hanya dapat mengakses fitur sesuai dengan perannya. Konsumen hanya dapat melihat data transaksi miliknya sendiri, sedangkan admin, finance, sales, petugas PDI, leasing, dan super admin memiliki akses sesuai tanggung jawab masing-masing. 

   

3. **Audit Trail**

   	Sistem harus mencatat seluruh aktivitas penting seperti login, perubahan data kendaraan, perubahan harga, upload dokumen, validasi dokumen, pembayaran, pengajuan kredit, hasil PDI, dan perubahan status transaksi. Audit trail digunakan untuk kebutuhan keamanan, evaluasi, dan penyelesaian sengketa. 

   

7. **Standar yang Berlaku**

   Standar dan ketentuan yang menjadi acuan dalam pengembangan aplikasi GarasiGo meliputi: 

1. Standar penyusunan dokumen Software Requirements Specification.  
2. Standar keamanan pembayaran digital.  
3. Standar perlindungan data pribadi.  
4. Regulasi pajak kendaraan bermotor di Indonesia.  
5. Regulasi administrasi STNK, TNKB, BPKB, dan STCK.  
6. Regulasi pembiayaan kendaraan oleh OJK dan lembaga pembiayaan.  
7. Regulasi jaminan fidusia untuk pembelian kendaraan secara kredit.  
8. Regulasi transaksi elektronik dan tanda tangan elektronik.  
9. Ketentuan internal dealer terkait penjualan, PDI, pengiriman, dan serah terima kendaraan.

   

   

4. **DESAIN DAN PERANCANGAN SISTEM**  
   1. **Entity Relationship Diagram**  
   2. **Alur Informasi**  
1. **Pembelian Mobil Tunai**

   	Alur pembelian mobil tunai dimulai ketika konsumen melakukan login ke aplikasi, mencari mobil melalui katalog, memilih unit kendaraan, dan melihat detail kendaraan. Setelah itu, sistem menampilkan simulasi total biaya pembelian yang terdiri dari harga kendaraan, pajak, biaya administrasi, dan biaya dokumen kendaraan. Apabila konsumen setuju, konsumen dapat melakukan booking unit dan mengunggah dokumen yang dibutuhkan.

   Admin dealer kemudian memvalidasi dokumen konsumen. Jika dokumen valid, sistem membuat SPK digital dan invoice pembayaran. Konsumen melakukan pembayaran melalui metode yang tersedia. Setelah pembayaran diverifikasi oleh admin finance, kendaraan masuk ke tahap PDI. Jika kendaraan lolos PDI, unit siap diserahkan kepada konsumen.

   

   Alur singkat:

   Login → Pilih Mobil → Simulasi Biaya → Booking Unit → Upload Dokumen → Validasi Dokumen → Buat SPK → Pembayaran → Verifikasi Pembayaran → PDI → Serah Terima → Transaksi Selesai.

   

2. **Pembelian Mobil Kredit**  
3. **Simulasi Pajak dan Total Biaya**  
4. **Mobil Bekas atau Tukar Tambah**  
5. **PDI dan Serah Terima**  
   3. **Manajemen Informasi**  
1. **Pengelolaan Data dan Informasi Pengguna**

   Sistem mengelola data pengguna berdasarkan role yang dimiliki. Data konsumen mencakup identitas pribadi, alamat, kontak, dokumen identitas, dan riwayat transaksi. Data sales, admin, finance, petugas PDI, leasing, dan super admin digunakan untuk mengatur hak akses serta tanggung jawab masing-masing pengguna. 

   

2. **Pengelolaan Informasi Kendaraan dan Inventori Dealer**

   Sistem mengelola data kendaraan yang tersedia di dealer, baik mobil baru maupun mobil bekas. Informasi kendaraan meliputi merek, model, tipe, tahun, warna, harga, stok, status unit, nomor rangka, nomor mesin, lokasi dealer, dan foto kendaraan. 

   

3. **Pengelolaan Informasi Transaksi dan SPK**

   	Sistem mencatat seluruh proses transaksi pembelian kendaraan, mulai dari booking, pembuatan SPK, pembayaran, pengajuan kredit, validasi dokumen, PDI, hingga serah terima kendaraan. SPK digital dibuat berdasarkan data kendaraan, data konsumen, metode pembayaran, harga kendaraan, dan ketentuan transaksi. 

   

4. **Pengelolaan Informasi Pajak dan Simulasi Biaya**

   	Sistem mengelola data simulasi biaya berdasarkan harga kendaraan, NJKB, wilayah registrasi, jenis kendaraan, dan tarif pajak yang berlaku. Komponen biaya seperti PPN, PPnBM, PKB, BBNKB, Opsen PKB, Opsen BBNKB, SWDKLLJ, STNK, TNKB, BPKB, dan biaya administrasi dicatat secara terpisah agar lebih transparan. 

   

5. **Pengelolaan Informasi Dokumen**

   	Sistem mengelola dokumen konsumen dan dokumen kendaraan. Dokumen konsumen meliputi KTP, KK, NPWP, slip gaji, rekening koran, dan dokumen pendukung kredit. Dokumen kendaraan meliputi STNK, BPKB, faktur, kwitansi, SPK, invoice, dokumen fidusia, dan berita acara serah terima. 

   

6. **Pengelolaan Informasi Pembayaran**

   	Sistem mencatat seluruh data pembayaran, seperti booking fee, down payment, pelunasan, biaya administrasi, dan biaya tambahan lainnya. Status pembayaran dapat berupa menunggu pembayaran, menunggu verifikasi, berhasil, gagal, atau dibatalkan. 

   

7. **Pengelolaan Informasi Kredit dan Fidusia**

   	Sistem mengelola informasi pengajuan kredit, seperti data konsumen, data kendaraan, pilihan leasing, jumlah DP, tenor, bunga, estimasi cicilan, status pengajuan, dan dokumen pendukung. Untuk transaksi kredit, sistem juga mencatat informasi fidusia sebagai bagian dari legalitas pembiayaan kendaraan. 

   

8. **Pengelolaan Informasi PDI dan Serah Terima**

   	Sistem mengelola hasil Pre-Delivery Inspection yang dilakukan oleh petugas dealer. Checklist PDI mencakup kondisi fisik kendaraan, komponen mesin, kelistrikan, interior, eksterior, aksesoris, dokumen, dan kelengkapan unit. Setelah kendaraan lolos PDI, sistem menyiapkan proses serah terima kendaraan. 

   

9. **Pengelolaan Informasi Keamanan dan Privasi**

   	Sistem harus menjaga keamanan informasi pengguna, kendaraan, transaksi, pembayaran, dan dokumen. Data sensitif harus dilindungi dengan mekanisme autentikasi, otorisasi, enkripsi, dan audit trail. 

   

10. **Pengelolaan Informasi Laporan**

    	Sistem menyediakan laporan untuk mendukung pengambilan keputusan oleh manajemen dealer. Laporan yang dihasilkan meliputi laporan penjualan, laporan stok kendaraan, laporan transaksi, laporan pembayaran, laporan pengajuan kredit, laporan PDI, dan laporan performa sales. 

    

    4. **Rancangan Desain Aplikasi**  
1. **Tampilan Mobile**  
2. **Tampilan Web**

