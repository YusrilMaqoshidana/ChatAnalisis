# Panduan & Lembar Validasi Blackbox Testing (Manual Testing)
**Sistem Analisis Obrolan Grup WhatsApp (ChatAnalisis)**

---

## 📌 Informasi Dokumen Pengujian

| Parameter | Keterangan |
| :--- | :--- |
| **Nama Aplikasi** | ChatAnalisis (Sistem Skripsi Pemodelan Topik obrolan WhatsApp) |
| **Versi Platform** | Web Application (Vue 3 + Vite + Tailwind CSS + FastAPI) |
| **Metode Pengujian** | Blackbox Testing (*Equivalence Partitioning*, *Boundary Value Analysis*, *State Transition*) |
| **Lingkungan Uji** | Google Chrome / Mozilla Firefox / Microsoft Edge (Desktop & Mobile) |
| **Penguji (Tester)** | Penguji Manual (User / Evaluator) |
| **Tanggal Pengujian** | 25 Agustus 2026 |

---

## 🎯 Petunjuk Penggunaan Lembar Validasi
1. Jalankan server **Frontend** (`npm run dev` pada rute port `http://localhost:5173`) dan **Backend** FastAPI (`http://localhost:8000`).
2. Lakukan pengujian secara berurutan sesuai **Langkah-langkah Pengujian** di setiap skenario.
3. Berikan tanda centang `[x]` pada kolom **Status Validasi**:
   - **PASS**: Jika hasil aktual di layar sesuai dengan *Hasil yang Diharapkan*.
   - **FAIL**: Jika terjadi bug, error tampilan, atau tidak sesuai skenario.
4. Tuliskan temuan atau kejanggalan pada kolom **Catatan / Keterangan**.

---

## 📑 Daftar Tabel Skenario Pengujian Blackbox

### 1. Modul Landing Page & Navigasi Utama (`/` & `/about`)

| ID Pengujian | Fitur / Modul | Skenario Pengujian | Langkah-langkah Pengujian | Data Uji / Input | Hasil yang Diharapkan (Expected Result) | Status Validasi | Catatan / Keterangan |
| :---: | :--- | :--- | :--- | :--- | :--- | :---: | :--- |
| **TC-NAV-01** | Landing Page | Membuka rute utama aplikasi | 1. Buka browser.<br>2. Akses `http://localhost:5173/`. | URL: `/` | Halaman Beranda (*HomeView*) tampil dengan hero section, deskripsi fitur, dan tombol CTA "Mulai Analisis Obrolan". | `[ ] PASS`<br>`[ ] FAIL` | |
| **TC-NAV-02** | Navigasi CTA | Menekan tombol CTA "Mulai Analisis" | 1. Berada di halaman `/`.<br>2. Klik tombol "Mulai Analisis Obrolan". | Klik Tombol | Pengguna diarahkan secara mulus ke halaman wizard upload (`/upload`). | `[ ] PASS`<br>`[ ] FAIL` | |
| **TC-NAV-03** | Halaman About | Membuka rute tentang aplikasi | 1. Klik menu "Tentang" pada Navbar.<br>2. Atau akses `http://localhost:5173/about`. | URL: `/about` | Halaman *AboutView* tampil menjelaskan metodologi BERTopic, BIRCH Clustering, dan IndoBERTweet. | `[ ] PASS`<br>`[ ] FAIL` | |
| **TC-NAV-04** | Navbar Responsive | Pengujian tampilan navbar di layar HP/Mobile | 1. Buka DevTools (F12) -> Switch to Mobile View (width < 640px).<br>2. Amati elemen navbar. | Mobile Screen | Element navbar menyesuaikan layar (responsive layout), menu collapse/hamburger berjalan lancar. | `[ ] PASS`<br>`[ ] FAIL` | |

---

### 2. Modul Wizard Upload & Filter (`/upload`) - Step 1: Upload File

| ID Pengujian | Fitur / Modul | Skenario Pengujian | Langkah-langkah Pengujian | Data Uji / Input | Hasil yang Diharapkan (Expected Result) | Status Validasi | Catatan / Keterangan |
| :---: | :--- | :--- | :--- | :--- | :--- | :---: | :--- |
| **TC-UPL-01** | Upload .txt Valid | Mengunggah berkas ekspor obrolan `.txt` | 1. Buka rute `/upload` (Step 1).<br>2. Drag & drop atau Browse file `.txt` ekspor WhatsApp valid. | File: `whatsapp_chat.txt` | File berhasil diuraikan (*parsed*), indikator nama file dan ukuran (KB) muncul, tombol "Selanjutnya" aktif. | `[ ] PASS`<br>`[ ] FAIL` | |
| **TC-UPL-02** | Upload .zip Valid | Mengunggah berkas ekspor obrolan terkompresi `.zip` | 1. Buka `/upload`.<br>2. Pilih file `.zip` yang berisi obrolan WhatsApp. | File: `WhatsApp_Chat.zip` | File `.zip` berhasil diekstrak di client-side, pesan terbaca, tombol "Selanjutnya" aktif. | `[ ] PASS`<br>`[ ] FAIL` | |
| **TC-UPL-03** | Format Tidak Valid | Mengunggah berkas dengan format selain `.txt` / `.zip` | 1. Buka `/upload`.<br>2. Pilih file format `.pdf`, `.png`, atau `.docx`. | File: `dokumen.pdf` | Tampil pesan peringatan merah *"Warning: Format berkas tidak didukung..."*, file ditolak, tombol "Selanjutnya" tetap disabled. | `[ ] PASS`<br>`[ ] FAIL` | |
| **TC-UPL-04** | File Korup / Kosong | Mengunggah file `.txt` kosong tanpa struktur obrolan | 1. Buat file `empty.txt` (0 byte).<br>2. Upload file tersebut ke DropZone. | File: `empty.txt` | Tampil pesan kesalahan *"Gagal memproses berkas chat / tidak ada pesan valid"*, sistem menolak file. | `[ ] PASS`<br>`[ ] FAIL` | |
| **TC-UPL-05** | Hapus File (Clear) | Batalkan/hapus pilihan file yang sudah diunggah | 1. Upload file `.txt` valid.<br>2. Klik ikon/tombol "Clear / Hapus File". | Klik Clear | Informasi file terhapus, DropZone kembali ke state kosong awal, tombol "Selanjutnya" kembali disabled. | `[ ] PASS`<br>`[ ] FAIL` | |
| **TC-UPL-06** | Navigasi Step 1 -> 2 | Berpindah ke Step 2 Filter Rentang Waktu | 1. Upload file `.txt` valid.<br>2. Klik tombol "Selanjutnya". | Klik Selanjutnya | Tampilan berpindah ke **Step 2 (Filter Tanggal)**. Header Stepper menunjukkan Step 2 aktif. | `[ ] PASS`<br>`[ ] FAIL` | |

---

### 3. Modul Wizard Upload & Filter (`/upload`) - Step 2: Filter Rentang Waktu

| ID Pengujian | Fitur / Modul | Skenario Pengujian | Langkah-langkah Pengujian | Data Uji / Input | Hasil yang Diharapkan (Expected Result) | Status Validasi | Catatan / Keterangan |
| :---: | :--- | :--- | :--- | :--- | :--- | :---: | :--- |
| **TC-FLT-01** | Visualisasi Grafik | Memeriksa grafik aktivitas obrolan harian | 1. Berada di Step 2.<br>2. Amati grafik *TimeRangeFilter*. | Data Obrolan | Grafik batang/garis keaktifan harian obrolan dari tanggal awal hingga akhir berhasil dirender. | `[ ] PASS`<br>`[ ] FAIL` | |
| **TC-FLT-02** | Slider Rentang Tanggal | Menggeser slider rentang tanggal obrolan | 1. Geser handle slider kiri dan kanan (misal 20% - 80%). | Slider Drag | Label tanggal awal dan tanggal akhir diperbarui secara real-time sesuai rentang yang dipilih. | `[ ] PASS`<br>`[ ] FAIL` | |
| **TC-FLT-03** | Navigasi Kembali | Menekan tombol "Kembali" ke Step 1 | 1. Berada di Step 2.<br>2. Klik tombol "Kembali". | Klik Kembali | Tampilan kembali ke **Step 1 (Upload File)** dengan file yang sudah dipilih sebelumnya tetap tersimpan. | `[ ] PASS`<br>`[ ] FAIL` | |
| **TC-FLT-04** | Mulai Analisis | Menekan tombol "Mulai Analisis" | 1. Tentukan rentang tanggal.<br>2. Klik tombol "Mulai Analisis". | Klik Mulai Analisis | Aplikasi melakukan pemotongan tanggal, anonimisasi pesan client-side, upload CSV ke server, dan pindah ke **Step 3 (Analisis AI)**. | `[ ] PASS`<br>`[ ] FAIL` | |

---

### 4. Modul Wizard Upload & Filter (`/upload`) - Step 3: Progress Analisis AI (SSE)

| ID Pengujian | Fitur / Modul | Skenario Pengujian | Langkah-langkah Pengujian | Data Uji / Input | Hasil yang Diharapkan (Expected Result) | Status Validasi | Catatan / Keterangan |
| :---: | :--- | :--- | :--- | :--- | :--- | :---: | :--- |
| **TC-ANL-01** | Terminal Progress | Memantau log proses analisis AI secara real-time | 1. Berada di Step 3.<br>2. Perhatikan *TerminalSimulator*. | SSE Event Stream | 8 tahapan proses (Preprocessing, IndoBERTweet, UMAP, BIRCH, metrik) berjalan berurutan dengan indikator running -> completed. | `[ ] PASS`<br>`[ ] FAIL` | |
| **TC-ANL-02** | Auto Redirection | Menguji perpindahan otomatis setelah analisis selesai | 1. Tunggu hingga tahapan ke-8 selesai (status `done: true`). | Auto Event | Setelah jeda singkat (~800ms), aplikasi secara otomatis melakukan redirect ke rute `/results`. | `[ ] PASS`<br>`[ ] FAIL` | |
| **TC-ANL-03** | Error Backend Handling | Menguji respon sistem jika server backend mati/error | 1. Matikan server Backend (FastAPI).<br>2. Klik "Mulai Analisis". | Backend Offline | Step 3 menampilkan pesan kesalahan merah *"Koneksi ke server terputus / Gagal mengunggah berkas"*, tombol "Coba Lagi" & "Kembali ke Filter" aktif. | `[ ] PASS`<br>`[ ] FAIL` | |
| **TC-ANL-04** | Tombol Coba Lagi | Mengulang proses analisis saat terjadi kegagalan | 1. Pada kondisi error (TC-ANL-03), nyalakan backend kembali.<br>2. Klik tombol "Coba Lagi". | Klik Coba Lagi | Proses analisis diulang kembali dari tahap awal tanpa perlu mengunggah ulang file. | `[ ] PASS`<br>`[ ] FAIL` | |

---

### 5. Modul Dashboard Hasil Analisis (`/results`)

| ID Pengujian | Fitur / Modul | Skenario Pengujian | Langkah-langkah Pengujian | Data Uji / Input | Hasil yang Diharapkan (Expected Result) | Status Validasi | Catatan / Keterangan |
| :---: | :--- | :--- | :--- | :--- | :--- | :---: | :--- |
| **TC-RES-01** | Ringkasan Metrik | Memeriksa nilai evaluasi statistik & clustering | 1. Akses halaman `/results` setelah analisis berhasil.<br>2. Periksa baris kartu metrik. | Data Metrik Server | Menampilkan kartu: *Topik Terdeteksi*, *Topic Diversity*, *C-NPMI Coherence*, *Embedding Density*, dan *Intra-topic Sim* dengan angka valid. | `[ ] PASS`<br>`[ ] FAIL` | |
| **TC-RES-02** | Active Dates Chart | Memeriksa chart keaktifan pesan per tanggal | 1. Amati komponen *ActiveDatesChart*. | Grafik Tanggal | Chart line/bar menampilkan grafik volume obrolan per tanggal dengan tooltip aktif saat dikursorkan. | `[ ] PASS`<br>`[ ] FAIL` | |
| **TC-RES-03** | Top Senders List | Memeriksa daftar pengirim pesan teraktif | 1. Amati bagian *TopSenderList*. | Data Pengirim | Menampilkan daftar nama/anonim pengirim teratas beserta jumlah pesan dan avatar inisial. | `[ ] PASS`<br>`[ ] FAIL` | |
| **TC-RES-04** | Active Hours Chart | Memeriksa grafik keaktifan jam obrolan (00-23) | 1. Amati bagian *ActiveHoursChart*. | Grafik Jam | Visualisasi grafik jam sibuk obrolan grup tampil dengan akurat. | `[ ] PASS`<br>`[ ] FAIL` | |
| **TC-RES-05** | Klaster Topik & Paginasi | Memeriksa kartu klaster topik & fungsi paginasi | 1. Scroll ke *Daftar Klaster Topik Obrolan*.<br>2. Uji tombol halaman paginasi (1, 2, next, prev). | Paginator Click | Kartu klaster topik tampil per halaman (default 6), paginasi berfungsi memperbarui daftar topik tanpa reload. | `[ ] PASS`<br>`[ ] FAIL` | |
| **TC-RES-06** | Analisis File Baru | Menguji penghapusan session & navigasi analisis baru | 1. Berada di `/results`.<br>2. Klik tombol "Analisis File Baru". | Klik Analisis File Baru | **1.** Request API `DELETE /api/results/{session_id}` dikirim.<br>**2.** `sessionStorage` dibersihkan.<br>**3.** State diset ulang ke awal.<br>**4.** Pengguna diarahkan ke `/upload` **Step 1 (Upload)** tanpa perlu di-refresh. | `[ ] PASS`<br>`[ ] FAIL` | |

---

### 6. Modul Detail Topik & Konteks Pesan (`/results/topics/:topicId`)

| ID Pengujian | Fitur / Modul | Skenario Pengujian | Langkah-langkah Pengujian | Data Uji / Input | Hasil yang Diharapkan (Expected Result) | Status Validasi | Catatan / Keterangan |
| :---: | :--- | :--- | :--- | :--- | :--- | :---: | :--- |
| **TC-TPC-01** | Navigasi Ke Detail Topik | Membuka halaman detail topik dari dashboard | 1. Berada di `/results`.<br>2. Klik salah satu kartu topik (misal Topik #0). | Klik TopicCard | Pengguna diarahkan ke URL `/results/topics/0`. Halaman menampilkan label topik, kata kunci, dan daftar pesan klaster. | `[ ] PASS`<br>`[ ] FAIL` | |
| **TC-TPC-02** | Filter Search Pesan | Menguji pencarian kata kunci di dalam topik | 1. Berada di detail topik.<br>2. Ketik kata kunci pada kolom pencarian pesan. | Input: `tugas` | Daftar pesan terfilter secara otomatis menampilkan pesan yang mengandung kata `tugas`. | `[ ] PASS`<br>`[ ] FAIL` | |
| **TC-TPC-03** | Modal Konteks Pesan | Membuka modal konteks percakapan di sekitar pesan | 1. Klik ikon / tombol "Konteks" pada salah satu baris pesan.<br>2. Amati modal yang muncul. | Klik Konteks Pesan | Modal *MessageContextModal* terbuka menampilkan 4 pesan sebelum dan 4 pesan sesudah, dengan pesan pilihan di-highlight. | `[ ] PASS`<br>`[ ] FAIL` | |
| **TC-TPC-04** | Tutup Modal Konteks | Menutup modal konteks percakapan | 1. Klik ikon silang `(X)` atau area luar modal konteks. | Klik Close | Modal konteks tertutup dan pengguna kembali ke daftar pesan topik. | `[ ] PASS`<br>`[ ] FAIL` | |
| **TC-TPC-05** | Kembali ke Results | Menekan tombol "Kembali ke Hasil Analisis" | 1. Berada di detail topik.<br>2. Klik tombol "Kembali". | Klik Kembali | Pengguna kembali ke dashboard `/results` tanpa kehilangan data analisis session tersebut. | `[ ] PASS`<br>`[ ] FAIL` | |

---

### 7. Pengujian Keamanan Rute (Route Guard) & Session Persistence

| ID Pengujian | Fitur / Modul | Skenario Pengujian | Langkah-langkah Pengujian | Data Uji / Input | Hasil yang Diharapkan (Expected Result) | Status Validasi | Catatan / Keterangan |
| :---: | :--- | :--- | :--- | :--- | :--- | :---: | :--- |
| **TC-SEC-01** | Akses Direct `/results` Tanpa Session | Membuka URL `/results` langsung tanpa upload file | 1. Buka tab baru / hapus `sessionStorage`.<br>2. Ketik langsung URL `http://localhost:5173/results`. | Direct URL Access | Route guard memblokir akses dan otomatis me-redirect pengguna ke rute `/upload`. | `[ ] PASS`<br>`[ ] FAIL` | |
| **TC-SEC-02** | Akses Direct `/results/topics/0` Tanpa Session | Membuka URL detail topik tanpa session aktif | 1. Hapus `sessionStorage`.<br>2. Akses URL `http://localhost:5173/results/topics/0`. | Direct URL Access | Route guard memblokir akses dan me-redirect pengguna ke rute `/upload`. | `[ ] PASS`<br>`[ ] FAIL` | |
| **TC-SEC-03** | Akses Direct `/upload` Saat Session Aktif | Membuka URL `/upload` saat session analisis masih ada | 1. Lakukan analisis hingga berhasil di `/results`.<br>2. Ubah URL browser secara manual ke `/upload`. | Direct URL Access | Route guard mendeteksi `sessionStorage` aktif dan otomatis me-redirect kembali ke `/results`. | `[ ] PASS`<br>`[ ] FAIL` | |
| **TC-SEC-04** | Refresh Halaman (F5) di Dashboard | Menekan tombol F5 saat berada di `/results` | 1. Berada di `/results`.<br>2. Tekan F5 / Refresh browser. | Page Refresh (F5) | Aplikasi membaca `sessionId` dari `sessionStorage`, mengunduh ulang data overview dari backend, dan tetap berada di `/results`. | `[ ] PASS`<br>`[ ] FAIL` | |
| **TC-SEC-05** | Tombol Back Browser | Menekan tombol Back browser dari `/results` | 1. Dari `/results`, tekan tombol "Back" di browser. | Browser Back | Route guard mencegah tampilan bug dan menangani rute secara konsisten. | `[ ] PASS`<br>`[ ] FAIL` | |

---

## 📊 Lembar Rekapitulasi Hasil Pengujian Manual

*(Diisi setelah seluruh skenario pengujian di atas dilaksanakan)*

- **Total Skenario Uji**: 26 Test Cases
- **Jumlah PASS**: `_____` Cases
- **Jumlah FAIL**: `_____` Cases
- **Tingkat Keberhasilan (Pass Rate)**: `_____ %`

### Catatan Temuan Bug & Tindak Lanjut:
1. _________________________________________________________________________________
2. _________________________________________________________________________________
3. _________________________________________________________________________________

---
*Dokumen Blackbox Testing ini dibuat secara otomatis untuk Sistem Skripsi Pemodelan Topik ChatAnalisis.*
