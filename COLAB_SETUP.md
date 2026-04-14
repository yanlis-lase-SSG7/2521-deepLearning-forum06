# Panduan Menjalankan Forum06 di Google Colab

## 🎯 Alur Kerja Keseluruhan

```
VS Code (edit) → GitHub (push) → Colab GPU (run) → Drive (checkpoint) → GitHub (pull hasil)
```

---

## 🚀 Pilihan Eksekusi

Anda memiliki **2 opsi** untuk menjalankan notebook dengan GPU Colab:

| Opsi | Tools | Setup | Rekomendasi |
|------|-------|-------|-------------|
| **A** (Recommended) | VS Code + Colab Extension | 5 menit | ✅ Tetap di VS Code, lebih seamless |
| **B** | Browser Colab | 3-5 menit | Alternatif jika opsi A ada masalah |

---

## ✅ OPSI A: Google Colab in VS Code (RECOMMENDED)

### Keuntungan
- Tetap di VS Code (workflow utama Anda).
- GUI kernel selector yang elegan.
- Output langsung di VS Code.
- Tidak perlu buka tab browser terpisah.

### Step A1: Install Extension

1. Di VS Code, buka **Extensions** (Ctrl+Shift+X).
2. Cari: `Google Colab`
3. Install extension resmi dari **Google** (publisher name: Google).
4. Reload VS Code saat diminta.

### Step A2: Aktifkan Colab Kernel

1. Buka file notebook `Forum06-garbage_classification_question.ipynb` di VS Code.
2. Klik tombol **Select Kernel** (atas kanan, atau Ctrl+Shift+P → Python: Select Interpreter).
3. Pilih **Google Colab**.
4. Sign in dengan akun Google Anda (browser popup akan muncul).
5. Tunggu kernel initialize (1-2 menit).

### Step A3: Set GPU Runtime

Setelah kernel connected, Colab akan default ke CPU. Ubah ke GPU:
1. Di VS Code, buka Command Palette (Ctrl+Shift+P).
2. Ketik: `Colab: Change runtime type`.
3. Pilih GPU (T4 atau L4 jika ada).
4. Tunggu kernel restart (1-2 menit).

---

## 🔄 OPSI B: Browser Colab (Fallback)

Jika VS Code extension ada masalah, gunakan browser Colab secara langsung:

### Step B1: Buka Notebook di Colab

1. Buka browser, pergi ke [Google Colab](https://colab.research.google.com/).
2. Pilih "File" > "Open notebook" > "GitHub".
3. Paste URL repo Anda:
   ```
   https://github.com/USERNAME/2521-deepLearning-forum06
   ```
4. Cari dan klik `Forum06-garbage_classification_question.ipynb`.

### Step B2: Ubah Runtime ke GPU

1. Menu atas Colab: **Runtime** > **Change runtime type**.
2. Pilih:
   - **Runtime type**: Python 3
   - **Hardware accelerator**: GPU (T4 atau L4 jika tersedia)
3. Klik **Save**.

Colab akan restart kernel otomatis.

---

## Step 3: Set Colab Secrets (SAMA untuk Opsi A & B)

Notebook Anda sudah disiapkan untuk membaca token dari Colab Secrets. Ikuti langkah di bawah:

### 3a. Tambah `KAGGLE_API_TOKEN`

1. Di sidebar kiri Colab, cari ikon **kunci** (Secrets).
   - Jika tidak terlihat, buka menu "Tools" tiga titik > "Manage secrets".
2. Klik **+ Add new secret**.
3. **Name**: `KAGGLE_API_TOKEN`
4. **Value**: Paste token Kaggle Anda dari [https://www.kaggle.com/settings/account](https://www.kaggle.com/settings/account).
5. **Notebook access**: Toggle ON (biru).
6. Klik **Save**.

### 3b. Tambah `HF_TOKEN`

1. Klik **+ Add new secret** lagi.
2. **Name**: `HF_TOKEN`
3. **Value**: Paste token Hugging Face Anda dari [https://huggingface.co/settings/tokens](https://huggingface.co/settings/tokens).
   - Buat token baru dengan scope "Read" jika belum ada.
4. **Notebook access**: Toggle ON.
5. Klik **Save**.

---

## Step 4: Jalankan Notebook

### Untuk Opsi A (VS Code + Extension)
1. Cell sudah siap untuk dijalankan dari VS Code.
2. Klik tombol ▶ (Play) di sebelah kiri cell, atau Shift+Enter.
3. Output akan muncul langsung di VS Code.

### Untuk Opsi B (Browser Colab)
1. Klik tombol ▶ (Play) di sebelah cell.
2. Output akan muncul di browser.

### Alur Eksekusi Penting (Sama untuk A & B)

| Cell # | Isi | Keterangan |
|--------|-----|-----------|
| 1 | Dokumentasi intro | Markdown, cukup dibaca |
| 2 | Kaggle token dari Secret | Akan auto-load dari Colab Secrets |
| 3-5 | Setup hardware (GPU, mixed precision) | **Sangat penting**: pastikan GPU terdeteksi ✅ |
| 6-15 | Download dataset, labeling, EDA | Biarkan selesai, tunggu beberapa menit |
| 16-42 | Preprocessing, augmentation setup | Siapkan pipeline data |
| 43-44 | Hugging Face login + load ViT processor | HF token akan auto-load dari Colab Secrets |
| 45 | Konfigurasi model ViT | **PENTING**: Ganti `VIT_VARIANT = "base"` atau `"large"` |
| 46+ | Training + evaluasi | Tunggu selesai (~30-60 menit vit-base, lebih lama vit-large) |

---

## Step 5: Checkpoint Tersimpan Otomatis

Notebook sudah terlebih dahulu disiapkan dengan checkpoint callbacks:

- **Model terbaik**: `best_vit_model.keras` (berdasarkan validation loss terendah)
- **Weight per epoch**: `vit_epoch_XX.weights.h5`
- **Training log**: `training_log.csv` (loss, accuracy per epoch)
- **Backup for recovery**: `backup/` folder (untuk auto-restore jika terputus)

Semua file disimpan ke: `/content/drive/MyDrive/forum06_checkpoints/vit/`

---

## Step 6: Pull Hasil dari Colab ke VS Code

Setelah training selesai:

1. Download file checkpoint dari Google Drive (atau git pull jika sudah commit ke repo).
2. Di terminal VS Code, folder forum06:
   ```powershell
   git pull origin main
   ```
3. Model terbaik siap untuk inferensi atau analisis lebih lanjut.

---

## Tips & Troubleshooting

### Untuk VS Code + Colab Extension (Opsi A)

| Masalah | Solusi |
|---------|--------|
| Extension tidak terdeteksi di Marketplace | Upate VS Code ke versi terbaru (v1.90+) |
| Kernel tidak bisa connect ke Colab | Cek internet, clear cache VS Code, reinstall extension |
| GPU tidak terdeteksi | Jalankan command `Colab: Change runtime type` pilih GPU ulang |
| Auth/Sign-in error | Clear cookies browser, sign out, sign in ulang |

### Untuk Browser Colab (Opsi B)

| GPU Tidak Terdeteksi | Opsi A: Jalankan `Colab: Change runtime type` ulang. Opsi B: Runtime > Change runtime type > pilih GPU |
| Token Kaggle/HF Error | Pastikan Colab Secret sudah ditambah dengan nama tepat & toggle-nya ON (biru) |

| Koneksi Putus / Sesi Timeout | Notebook punya `BackupAndRestore` callback yang auto-save state. Jalankan ulang cell training dengan `initial_epoch=CHECKPOINT_EPOCH`. |
| Training Terlalu Lambat | Ubah `VIT_VARIANT = "base"` (lebih cepat). Kurangi `HEAD_EPOCHS` atau `FINE_TUNE_EPOCHS`. Kurangi `BATCH_SIZE` ke 16 jika OOM. |

---

## Checklist Pre-Run

Sebelum eksekusi training, pastikan:

- [ ] Runtime sudah GPU (lihat indikator di atas kanan Colab).
- [ ] Colab Secret `KAGGLE_API_TOKEN` sudah ada & aktif.
- [ ] Colab Secret `HF_TOKEN` sudah ada & aktif.
- [ ] Cell 3-5 berjalan tanpa error (hardware terdeteksi).
- [ ] Cell 15 berjalan tanpa error (dataset terdownload).
- [ ] Cell 43-44 berhasil login ke Kaggle & Hugging Face.
- [ ] Cell 45 sudah tentukan `VIT_VARIANT` ("base" atau "large").

---

## Contact / Help

Jika ada masalah yang tidak tertera di atas, silakan:
1. Cek error message yang tampil di cell.
2. Copy error message lengkap.
3. Cek file `training_log.csv` di folder checkpoint untuk melihat loss/accuracy history.
4. Restart Colab session dan coba ulang.

---

**Happy Training! 🚀**
