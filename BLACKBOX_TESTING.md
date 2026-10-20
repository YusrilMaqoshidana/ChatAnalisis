# 🧪 Panduan Pengujian Blackbox Testing (Fitur MVP Sistem ChatAnalisis)

Dokumen ini berisi panduan pengujian fungsional berbasis *Minimum Viable Product* (MVP) untuk sistem analisis topik percakapan WhatsApp **ChatAnalisis** (*ChatAnalisis-FE* dan *ChatAnalisis-BE*). Pengujian dilakukan secara *End-to-End* pada alur fungsional inti sistem tanpa melihat struktur kode internal program.

---

### 1. Modul Unggah & Validasi Berkas Obrolan (`/upload` - Step 1)

| ID Pengujian | Fitur / Modul | Skenario Pengujian | Langkah-langkah Pengujian | Data Uji / Input | Hasil yang Diharapkan (Expected Result) | Status Validasi | Catatan / Keterangan |
| :---: | :--- | :--- | :--- | :--- | :--- | :---: | :--- |
| **TC-MVP-01** | Unggah Berkas Valid | Mengunggah berkas ekspor obrolan WhatsApp valid (`.txt` / `.zip`) | 1. Buka rute `/upload`.<br>2. Pilih atau seret berkas chat valid. | Berkas `.txt` / `.zip` chat | Berkas berhasil diurai (*parsed*), detail nama dan ukuran berkas muncul, serta tombol "Selanjutnya" aktif. | `[x] PASS`<br>`[ ] FAIL` | Valid |
| **TC-MVP-02** | Validasi Format Berkas | Mengunggah berkas dengan format di luar spesifikasi obrolan | 1. Buka rute `/upload`.<br>2. Pilih berkas non-chat (`.pdf` / `.png`). | Berkas `dokumen.pdf` | Sistem menolak berkas, memunculkan notifikasi peringatan format tidak didukung, dan tombol tetap dinonaktifkan. | `[x] PASS`<br>`[ ] FAIL` | Valid |

---

### 2. Modul Pemfilteran Rentang Waktu, Mode Analisis & Privasi Data (`/upload` - Step 2)

| ID Pengujian | Fitur / Modul | Skenario Pengujian | Langkah-langkah Pengujian | Data Uji / Input | Hasil yang Diharapkan (Expected Result) | Status Validasi | Catatan / Keterangan |
| :---: | :--- | :--- | :--- | :--- | :--- | :---: | :--- |
| **TC-MVP-03** | Filter Rentang Tanggal | Menyesuaikan rentang waktu pesan yang akan dianalisis | 1. Berada di Step 2.<br>2. Geser bilah rentang tanggal (slider). | Geser Slider Tanggal | Tanggal awal dan akhir diperbarui secara *real-time*, dan hanya data pesan dalam rentang yang diproses. | `[x] PASS`<br>`[ ] FAIL` | Valid |
| **TC-MVP-04** | Seleksi Mode Analisis | Memilih mode pemrosesan (*Dual-Mode Selector*) | 1. Berada di Step 2.<br>2. Pilih "Mode Cepat" atau "Mode Riset Lengkap". | Klik Kartu Mode | Kartu mode aktif ditandai secara visual, dan parameter konfigurasi mode tersimpan ke penyimpanan sesi. | `[x] PASS`<br>`[ ] FAIL` | Valid |
| **TC-MVP-05** | Anonimisasi Sisi Klien | Menjalankan anonimisasi otomatis sebelum pengiriman berkas | 1. Klik tombol "Mulai Analisis".<br>2. Amati transmisi jaringan (*Network devtools*). | Klik Mulai Analisis | Skrip sisi klien menyamarkan identitas dan nomor pengirim menjadi token SHA-256 bergaram sebelum dikirim ke backend. | `[x] PASS`<br>`[ ] FAIL` | Valid |

---

### 3. Modul Pemrosesan AI & Pemantauan Komputasi (`/upload` - Step 3)

| ID Pengujian | Fitur / Modul | Skenario Pengujian | Langkah-langkah Pengujian | Data Uji / Input | Hasil yang Diharapkan (Expected Result) | Status Validasi | Catatan / Keterangan |
| :---: | :--- | :--- | :--- | :--- | :--- | :---: | :--- |
| **TC-MVP-06** | Pemrosesan Asinkron AI | Menjalankan pemodelan topik pada utas latar belakang | 1. Backend menerima permintaan analisis.<br>2. Amati eksekusi pemodelan topik. | Payload CSV Anonim | Backend FastAPI memproses alur IndoBERTweet, *ImprovedBirch*, dan BM25 secara asinkron tanpa memblokir server. | `[x] PASS`<br>`[ ] FAIL` | Valid |
| **TC-MVP-07** | Pemantauan SSE *Real-Time* | Memantau aliran log proses komputasi pada simulator terminal | 1. Berada di Step 3.<br>2. Amati konsol log *TerminalSimulator*. | Aliran Event SSE | Perkembangan setiap tahapan analisis terpancar secara langsung ke antarmuka hingga status selesai (*completed*). | `[x] PASS`<br>`[ ] FAIL` | Valid |
| **TC-MVP-08** | Pengalihan Otomatis | Melakukan pengalihan rute secara otomatis pasca-analisis | 1. Tunggu hingga tahapan akhir selesai (*done: true*). | Sinyal Selesai SSE | Aplikasi secara otomatis mengalihkan tampilan dari Step 3 menuju halaman dasbor hasil (`/results`). | `[x] PASS`<br>`[ ] FAIL` | Valid |

---

### 4. Modul Dasbor Hasil Analisis & Visualisasi Topik (`/results`)

| ID Pengujian | Fitur / Modul | Skenario Pengujian | Langkah-langkah Pengujian | Data Uji / Input | Hasil yang Diharapkan (Expected Result) | Status Validasi | Catatan / Keterangan |
| :---: | :--- | :--- | :--- | :--- | :--- | :---: | :--- |
| **TC-MVP-09** | Kartu Metrik Evaluasi | Menyajikan ringkasan metrik kualitas pemodelan topik | 1. Berada di `/results`.<br>2. Periksa baris kartu metrik evaluasi. | Data Metrik Server | Dasbor menampilkan metrik jumlah topik, diversitas ($TD$), koherensi ($C_{\text{NPMI}}$), kepadatan ($ED$), dan kekohesifan ($ITS$). | `[x] PASS`<br>`[ ] FAIL` | Valid |
| **TC-MVP-10** | Grafik Keaktifan Obrolan | Menyajikan linimasa distribusi volume percakapan | 1. Berada di `/results`.<br>2. Amati grafik aktivitas tanggal (*ActiveDates*). | Data Agregasi Waktu | Visualisasi grafik linimasa menyajikan tren frekuensi obrolan per hari dengan interaksi *tooltip* yang responsif. | `[x] PASS`<br>`[ ] FAIL` | Valid |
| **TC-MVP-11** | Daftar Klaster Topik | Menyajikan kartu klaster topik dan kata kunci representatif | 1. Gulir ke daftar klaster topik.<br>2. Periksa kartu klaster dan kata kunci. | Data Klaster Topik | Setiap kartu klaster menyajikan label topik, jumlah pesan, proporsi korpus, dan 10 kata kunci teratas. | `[x] PASS`<br>`[ ] FAIL` | Valid |

---

### 5. Modul Eksplorasi Detail Topik & Konteks Pesan (`/results/topics/:topicId`)

| ID Pengujian | Fitur / Modul | Skenario Pengujian | Langkah-langkah Pengujian | Data Uji / Input | Hasil yang Diharapkan (Expected Result) | Status Validasi | Catatan / Keterangan |
| :---: | :--- | :--- | :--- | :--- | :--- | :---: | :--- |
| **TC-MVP-12** | Penelusuran Pesan Topik | Membuka rute detail topik untuk meninjau seluruh pesan anggota | 1. Klik salah satu kartu topik di `/results`.<br>2. Amati daftar pesan di halaman detail. | Klik Kartu Topik | Halaman detail menyajikan seluruh daftar pesan yang terpetakan ke dalam topik tersebut beserta stempel waktu. | `[x] PASS`<br>`[ ] FAIL` | Valid |
| **TC-MVP-13** | Modal Konteks Obrolan | Menampilkan percakapan utuh di sekitar pesan terpilih | 1. Pada detail topik, klik tombol "Lihat Konteks". | Klik Konteks Pesan | Dialog modal terbuka menyajikan percakapan sebelum dan sesudah pesan terpilih secara kronologis (*thread context*). | `[x] PASS`<br>`[ ] FAIL` | Valid |
| **TC-MVP-14** | Ekspor Laporan Analisis | Mengunduh ringkasan hasil pemodelan topik ke dokumen lokal | 1. Di halaman `/results`, klik tombol "Ekspor Laporan". | Klik Ekspor Laporan | Berkas laporan hasil pemodelan topik berhasil diunduh ke perangkat pengguna dengan data yang lengkap. | `[x] PASS`<br>`[ ] FAIL` | Valid |

---

## 📊 Lembar Rekapitulasi Hasil Pengujian Manual

*(Diisi setelah seluruh skenario pengujian di atas dilaksanakan)*

- **Total Skenario Uji**: 14 Test Cases (Fitur Fungsional Inti / MVP)
- **Jumlah PASS**: `14` Cases
- **Jumlah FAIL**: `0` Cases
- **Tingkat Keberhasilan (Pass Rate)**: `100 %`

### Catatan Kesimpulan Pengujian:
Seluruh 14 skenario kasus uji fungsional inti (MVP) yang merepresentasikan alur kerja aplikasi web ChatAnalisis dari tahap unggah berkas, pemfilteran & anonimisasi, pemrosesan topik asinkron dengan pemantauan SSE, dasbor visualisasi topik, hingga penelusuran konteks pesan dan ekspor laporan telah berhasil diuji dan dinyatakan **100% Valid**.
