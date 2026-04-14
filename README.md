# Deep Learning Forum 06: Garbage Classification dengan Vision Transformer (ViT)

**Student Name:** Yanlis Alim Sang Putra Lase  
**Student ID:** 2702751284  
**Program:** Master's in Informatics, BINUS Graduate Program

Repository ini berisi implementasi pipeline end-to-end untuk **klasifikasi sampah berbasis citra** menggunakan transfer learning dari model Vision Transformer (ViT) Google yang tersedia di Hugging Face.

## 1. Project Overview

Forum 06 mengimplementasikan pipeline end-to-end untuk **klasifikasi sampah** dengan focus pada:
1. **Akuisisi dataset** dari Kaggle (Garbage Classification Dataset).
2. **Labeling otomatis** berbasis struktur folder.
3. **EDA ringkas** untuk memahami distribusi kelas dan ukuran citra.
4. **Preprocessing** ke format ViT (224×224).
5. **Fine-tuning 2-tahap**: freeze backbone → unfreeze dengan learning rate lebih kecil.
6. **Checkpointing berkala** untuk mencegah kehilangan progres training.

**Sumber**: [Kaggle Garbage Classification Dataset](https://www.kaggle.com/datasets/asdasdasasdas/garbage-classification)

**Kategori** (6 kelas awal, difilter ke 5 kelas):
- `cardboard` (karton)
- `glass` (kaca)
- `metal` (logam)
- `paper` (kertas)
- `plastic` (plastik)
- ~~`trash`~~ (dibuang karena jumlah sampel sedikit)

**Ukuran**: ~2.500 gambar, masing-masing 512×384 px, distandarisasi ke 224×224 untuk ViT.

**Struktur**: Folder per kategori dengan subdivisi `train/val/test`.

## 2. Data Source (continued)

## 3. Quick Start

### ⚡ Recommended: Google Colab GPU

**Estimated time**: 30–60 menit untuk vit-base (~100-200 menit untuk vit-large).

1. **Push perubahan ke GitHub** (sudah selesai ✅):
   ```powershell
   git add .
   git commit -m "Forum06 setup + Colab integration"
   git push origin main
   ```

2. **Buka di Colab**:
   - Pergi ke [https://colab.research.google.com/](https://colab.research.google.com/)
   - File → Open notebook → GitHub
   - Paste URL repo → pilih notebook

3. **Setup Colab** (ikuti [COLAB_SETUP.md](COLAB_SETUP.md)):
   - Runtime → Change runtime type → GPU (T4 atau L4)
   - Tambah Colab Secrets: `KAGGLE_API_TOKEN`, `HF_TOKEN`

4. **Run Training**:
   - Cell → Run all
   - Tunggu training selesai
   - Checkpoint otomatis disimpan ke Google Drive

5. **Pull hasil** (opsional):
   ```powershell
   git pull origin main
   ```

## 4. Architecture & Configuration

| Komponen | Detail |
|----------|--------|
| **Backbone** | Google ViT Base / Large (dari Hugging Face) |
| **Input** | 224×224×3 RGB (normalized ke ViT mean/std) |
| **Training Strategy** | 2-stage: freeze backbone (5 epoch) → fine-tune (10 epoch) |
| **Loss** | SparseCategoricalCrossentropy |
| **Optimizer** | AdamW (LR 1e-4 → 1e-5, weight_decay 1e-4) |
| **Class Weighting** | Balanced (untuk handle imbalance) |
| **Augmentation** | RandomFlip, RandomRotation (0.05), RandomZoom (0.1) |
| **Callbacks** | EarlyStopping, ReduceLROnPlateau, ModelCheckpoint, BackupAndRestore, CSVLogger |

### Pilih Model ViT (Cell 43)

```python
VIT_VARIANT = "base"  # atau "large"
```

- **"base"**: Lebih cepat (~30-45 min), cocok untuk eksperimen
- **"large"**: Lebih akurat, training lebih lama (~2-3 jam)

## 5. Checkpoint & Safety

Notebook sudah disiapkan dengan checkpoint callbacks multi-layer:

| File | Keterangan |
|------|-----------|
| `best_vit_model.keras` | Model terbaik (berdasarkan val_loss terendah) |
| `vit_epoch_XX.weights.h5` | Weight per epoch (max 15 files sesuai total epoch) |
| `training_log.csv` | Log loss, accuracy per epoch |
| `backup/` | Auto-restore jika sesi terputus |

**Lokasi**: `/content/drive/MyDrive/forum06_checkpoints/vit/` (di Google Drive)

---

## 6. Dokumentasi & Setup

**📖 Panduan lengkap Colab setup** → [**COLAB_SETUP.md**](COLAB_SETUP.md)

## 7. Security: Token Management

**JANGAN hardcode token Kaggle atau Hugging Face di notebook.**

Notebook sudah disiapkan untuk membaca token dengan urutan prioritas:

### Kaggle Token
1. Colab Secret `KAGGLE_API_TOKEN`
2. Environment Variable `KAGGLE_API_TOKEN`
3. File `~/.kaggle/access_token`
4. File `~/.kaggle/kaggle.json`

### Hugging Face Token
1. Colab Secret `HF_TOKEN`
2. Environment Variable `HF_TOKEN`
3. File `cred/hf_token.json` (lokal)

Setup di Colab: Lihat [COLAB_SETUP.md](COLAB_SETUP.md) → Step 3.

## 8. Repository Structure

```
2521-deepLearning-forum06/
├── Forum06-garbage_classification_question.ipynb    # Main notebook
├── Forum06-requirements.txt                         # Dependencies
├── COLAB_SETUP.md                                  # 📖 Panduan Colab ✨
├── README.md                                       # File ini
├── .git/
├── .gitignore
├── dataset/
│   └── Garbage classification/
│       └── Garbage classification/
│           ├── cardboard/
│           ├── glass/
│           ├── metal/
│           ├── paper/
│           ├── plastic/
│           └── trash/
├── checkpoints/          # Local checkpoint (jika run lokal)
│   └── vit/
└── cred/                 # Kosong (tempat hf_token.json lokal)
    └── hf_token.json     # (jika ada, fallback saat tidak ada secret)
```

---

## 9. Troubleshooting Checklist

| Masalah | Solusi |
|---------|--------|
| GPU tidak terdeteksi di Colab | Runtime → Change runtime type → GPU |
| Token Kaggle error | Pastikan Colab Secret `KAGGLE_API_TOKEN` sudah ON |
| Token HF error | Pastikan Colab Secret `HF_TOKEN` sudah ON |
| Dataset tidak terdownload | Pastikan Kaggle token benar, internet stabil |
| Training timeout/disconnect | BackupAndRestore callback sudah aktif, ulang dari epoch terakhir |
| Out of Memory (OOM) | Kurangi BATCH_SIZE atau gunakan vit-base |

---

## 10. Pre-Training Checklist

Sebelum eksekusi training, pastikan:

- [ ] Notebook sudah di-push ke GitHub
- [ ] Buka di Colab dan ganti ke runtime GPU
- [ ] Colab Secret `KAGGLE_API_TOKEN` sudah ditambah & aktif
- [ ] Colab Secret `HF_TOKEN` sudah ditambah & aktif
- [ ] Cell 5 berhasil mendeteksi GPU
- [ ] Cell 15 berhasil download dataset dari Kaggle
- [ ] Cell 43 sudah tentukan `VIT_VARIANT` ("base" atau "large")
- [ ] Siap click "Run all"

---

## 11. Referensi

- [Hugging Face Vision Transformer (ViT) - Base](https://huggingface.co/google/vit-base-patch16-224)
- [Hugging Face Vision Transformer (ViT) - Large](https://huggingface.co/google/vit-large-patch16-224)
- [Vision Transformer (ViT) Paper](https://arxiv.org/abs/2010.11929)
- [Kaggle Garbage Classification Dataset](https://www.kaggle.com/datasets/asdasdasasdas/garbage-classification)
- [TensorFlow Keras Callbacks](https://www.tensorflow.org/api_docs/python/tf/keras/callbacks)
- [Transformers LibraryDocumentation](https://huggingface.co/docs/transformers/)

---

**Last Updated**: April 14, 2026  
**Status**: Ready for Colab GPU training ✅  
**Panduan Colab Lengkap**: [COLAB_SETUP.md](COLAB_SETUP.md) ✨
