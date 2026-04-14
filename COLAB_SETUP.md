# Panduan Menjalankan Forum06 di Google Colab

## Alur Kerja Keseluruhan

```
VS Code (edit) → GitHub (push) → Colab (run GPU) → Drive (checkpoint) → GitHub (push hasil)
```

---

## Step 1: Buka Notebook di Colab

1. Buka browser, pergi ke [Google Colab](https://colab.research.google.com/).
2. Pilih "File" > "Open notebook" > "GitHub".
3. Paste URL repo Anda:
   ```
   https://github.com/USERNAME/2521-deepLearning-forum06
   ```
4. Cari dan klik `Forum06-garbage_classification_question.ipynb`.

---

## Step 2: Ubah Runtime ke GPU

1. Menu atas Colab: **Runtime** > **Change runtime type**.
2. Pilih:
   - **Runtime type**: Python 3
   - **Hardware accelerator**: GPU (T4 atau L4 jika tersedia)
3. Klik **Save**.

Colab akan restart kernel otomatis.

---

## Step 3: Set Colab Secrets

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

1. Mulai dari **Cell 1** (pembuka dokumentasi).
2. Jalankan cell per cell dengan Shift+Enter atau tombol ► di sebelah kanan cell.

### Alur Eksekusi Penting

| Cell | Isi | Keterangan |
|------|-----|-----------|
| 1 | Dokumentasi intro | Markdown, cukup dibaca |
| 2 | Kaggle token dari Secret | Akan auto-load dari Secrets |
| 3-5 | Setup hardware (GPU, mixed precision) | **Sangat penting**: pastikan GPU terdeteksi |
| 6-15 | Download dataset, labeling, EDA | Biarkan selesai, tunggu beberapa menit |
| 16-42 | Preprocessing, augmentation setup | Siapkan pipeline data |
| 43-44 | Hugging Face login + load ViT processor | HF token akan auto-load dari Secrets |
| 45 | Konfigurasi model ViT | **Ganti** `VIT_VARIANT = "base"` atau `"large"` di sini |
| 46+ | Training + evaluasi | Tunggu selesai (estimasi 30-60 menit untuk vit-base, lebih lama untuk vit-large) |

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

### GPU Tidak Terdeteksi

**Error**: Cell 5 menampilkan "GPU tidak terdeteksi..."

**Solusi**: 
- Pastikan Anda sudah ubah Runtime > Change runtime type > GPU.
- Coba restart runtime (Runtime > Restart session).
- Kalau tetap tidak ada, berarti sudah batas GPU Colab Anda hari ini, coba lagi besok.

### Token Kaggle/HF Error

**Error**: "Kaggle token belum terdeteksi..." atau "HF token tidak ditemukan..."

**Solusi**:
- Pastikan Colab Secret sudah ditambah dengan nama tepat: `KAGGLE_API_TOKEN`, `HF_TOKEN`.
- Pastikan "Notebook access" toggle-nya ON (biru).
- Reload notebook (F5 atau refresh).

### Koneksi Putus / Sesi Timeout

**Solusi Built-in**:
- Notebook punya `BackupAndRestore` callback yang auto-save state setiap epoch.
- Jika sesi putus, jalankan ulang cell training dengan `initial_epoch=CHECKPOINT_EPOCH`.
- Checkpoint file tetap aman di Google Drive.

### Training Terlalu Lambat

**Opsi**:
- Ubah `VIT_VARIANT = "base"` (lebih cepat).
- Kurangi `HEAD_EPOCHS` atau `FINE_TUNE_EPOCHS` di cell training.
- Kurangi `BATCH_SIZE` ke 16 jika OOM.

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
