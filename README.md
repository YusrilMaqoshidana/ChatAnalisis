# 🚀 Panduan Deployment ChatAnalisis (Nginx Zero-CORS Architecture)

Dokumen ini berisi panduan deployment aplikasi **ChatAnalisis** (Frontend Vue 3 + Backend FastAPI) menggunakan **Nginx Reverse Proxy di Container Frontend** untuk menjamin **100% Bebas Error CORS** dan keamanan backend privat.

---

## 🏗️ Topologi Arsitektur (Zero CORS)

```text
Browser Client / Internet
           │
           ▼
     Coolify Proxy
           │
           ▼
  Frontend Container (Port 80: Nginx)
   ├── File Statis Vue SPA (/)
   └── Reverse Proxy Internal (/api & /analysis)
           │
           ▼ (Private Docker Network)
  Backend Container (Port 8000: FastAPI) ──► Volume: backend_storage
```

---

## 📋 Prasyarat Server (VPS Requirements)

- **Spesifikasi Minimal**: 2 vCPU, 4 GB RAM.
- **Spesifikasi Direkomendasikan**: 4 vCPU, 8 GB RAM.
- **Swap Memory File (Wajib)**:
  Backend memuat PyTorch & IndoBERTweet saat build (`warmup.py`). Tambahkan **Swap 4GB** di VPS:

  ```bash
  # Buat file swap 4GB di VPS
  sudo fallocate -l 4G /swapfile
  sudo chmod 600 /swapfile
  sudo mkswap /swapfile
  sudo swapon /swapfile

  # Buat permanen saat reboot
  echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
  ```

---

## 🛠️ Langkah Deployment di Coolify

### 1️⃣ Deploy Backend Application (`ChatAnalisis-BE`)
1. Di Dashboard Coolify, buat Resource baru ➔ Pilih Repository Git **`ChatAnalisis-BE`**.
2. **Build Pack**: **Dockerfile**.
3. **Ports Expose**: **`8000`**.
4. **Domains (FQDN)**: **KOSONGKAN / KOSONG** (Backend tidak perlu diekspos ke publik sama sekali).
5. **Settings ➔ Build Timeout**: Set ke **`1200` detik** (20 menit).
6. **Storages**: Tambahkan Destination Volume:
   - **Name**: `backend_storage`
   - **Destination Path**: `/app/storage`
7. Klik **Deploy**.

---

### 2️⃣ Deploy Frontend Application (`ChatAnalisis-FE`)
1. Buat Resource baru di Project yang sama ➔ Pilih Repository Git **`ChatAnalisis-FE`**.
2. **Build Pack**: **Dockerfile**.
3. **Ports Expose**: **`80`**.
4. **Environment Variables**: `VITE_API_BASE_URL=/`
5. **Domains (FQDN)**: `https://chatanalisis.domainanda.com`
6. Klik **Deploy**.

---

## 🔥 Kenapa Pola Ini Bebas CORS 100%?

- Browser pengguna **HANYA** mengirim request ke domain Frontend (`https://chatanalisis.domainanda.com`).
- Semua panggilan API (`/api/results/...` dan `/analysis`) diterima oleh Nginx pada Frontend Container, lalu diteruskan secara internal ke `http://chatanalisis-backend:8000`.
- Browser melihat seluruh response berasal dari **Origin Domain yang Sama** (*Same-Origin*), sehingga browser **TIDAK PERNAH** memblokir request karena isu CORS.

---

## ✅ Checklist Deployment

| No | Komponen | Keterangan / Nilai |
|---|---|---|
| 1 | **Frontend FQDN** | `https://chatanalisis.domainanda.com` |
| 2 | **Backend FQDN** | **Kosong** (Privat di internal Docker network) |
| 3 | **Frontend Nginx** | Menggunakan `ChatAnalisis-FE/nginx.conf` |
| 4 | **Backend Ports Expose** | `8000` |
| 5 | **CORS Issue** | **0% (Bebas CORS secara alami)** |
| 6 | **Volume Storage** | `backend_storage` ter-mount di `/app/storage` |
