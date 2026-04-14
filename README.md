# Deep Learning Forum 06: Garbage Classification with ViT-Large

**Student Name:** Yanlis Alim Sang Putra Lase  
**Student ID:** 2702751284  
**Program:** Master's in Informatics, BINUS Graduate Program

This repository contains the implementation and documentation for Forum 06, a deep learning assignment focused on multiclass garbage image classification using transfer learning with Vision Transformer Large (ViT-L/16).

## 1. Project Overview

This project builds an end-to-end image-classification pipeline for the Kaggle Garbage Classification dataset. The workflow covers dataset access, exploratory data analysis, class filtering, preprocessing, TensorFlow dataset construction, transfer learning, evaluation, and error analysis.

The main model in the notebook is the Hugging Face Vision Transformer checkpoint `google/vit-large-patch16-224`. The notebook does not position this task as a CNN-vs-CNN comparison. Instead, it focuses on how a large pre-trained ViT can be adapted to a relatively small material-recognition dataset and how its residual error patterns should be interpreted quantitatively.

The final executed notebook shows that the model reaches solid overall performance, but its dominant residual confusion is between `plastic` and `glass`, not `cardboard` and `paper` alone.

## 2. Data Source

The image dataset is obtained from Kaggle:

- **Kaggle dataset:** `asdasdasasdas/garbage-classification`

The raw dataset contains **2,527 images** across **6 categories**:

| No. | Category | Image Count |
|---|---|---:|
| 1 | cardboard | 403 |
| 2 | glass | 501 |
| 3 | metal | 410 |
| 4 | paper | 594 |
| 5 | plastic | 482 |
| 6 | trash | 137 |

Because `trash` is severely underrepresented, the main notebook excludes it from modelling. The effective modelling subset therefore contains:

- **2,390 images**
- **5 categories**: `cardboard`, `glass`, `metal`, `paper`, `plastic`
- **Uniform source resolution**: `512 x 384`

## 3. Function and Purpose

The main notebook is designed to perform the following tasks:

1. Download or resolve the garbage-classification dataset from local cache or Kaggle.
2. Inspect class balance and justify the exclusion of the `trash` category.
3. Build labeled path lists for training, validation, and testing.
4. Perform image-based exploratory data analysis on class counts, image geometry, file-size patterns, and channel statistics.
5. Standardize images for ViT input using resize, center crop, normalization, and tensor layout conversion.
6. Fine-tune `google/vit-large-patch16-224` in a two-stage training pipeline.
7. Evaluate the final model on the full test split.
8. Extend evaluation with a confusion matrix and classification report so the error narrative is supported by full-test quantitative evidence.

## 4. Expected Output

The main deliverables of this repository are:

1. **A completed main notebook** for garbage classification with ViT-Large.
2. **A standalone EDA notebook** summarising the exploratory analysis.
3. **A generated HTML EDA report** aligned with the main notebook conclusions.
4. **Training and evaluation outputs** produced from the notebook run.
5. **A final discussion section** covering challenges, solutions, conclusions, and recommendations.

### Quick Access: EDA Report (HTML)

- **EDA Report:** [https://yanlis-lase-ssg7.github.io/2521-deepLearning-forum06/EDA_Report_Garbage.html](https://yanlis-lase-ssg7.github.io/2521-deepLearning-forum06/EDA_Report_Garbage.html)

## 5. Step-by-Step Installation and Usage

Run the following commands in PowerShell from your preferred working directory.

### 5.1 Git Clone

```powershell
git clone https://github.com/yanlis-lase-SSG7/2521-deepLearning-forum06.git
cd 2521-deepLearning-forum06
```

### 5.2 Create Virtual Environment

```powershell
python -m venv venv
```

### 5.3 Activate Virtual Environment (Windows)

```powershell
.\venv\Scripts\activate
```

### 5.4 Install Dependencies

```powershell
pip install -r Forum06-requirements.txt
```

### 5.5 Provide Access Tokens

The main notebook may require:

- **Kaggle token** for dataset access
- **Hugging Face token** for downloading `google/vit-large-patch16-224`

Common approaches supported by the notebook and Colab workflow:

- environment variables
- local credential files
- Colab Secrets
- interactive fallback prompt

For the Colab-based workflow used for training, see [COLAB_SETUP.md](COLAB_SETUP.md).

### 5.6 Run the Main Notebook

Open and execute:

- `Forum06-garbage_classification_question.ipynb`

For reproducible results, run the notebook sequentially from top to bottom.

### 5.7 Run the EDA Notebook

Open and execute:

- `EDA_Report_Garbage.ipynb`

### 5.8 Refresh the Static HTML EDA Report

To regenerate the HTML report from the Python generator:

```powershell
python generate_eda_report.py
```

## 6. Technical Requirements

The technical stack used in this project includes:

- Python 3.10+
- TensorFlow 2.16.x
- Transformers
- tf-keras
- NumPy
- Pandas
- Matplotlib
- Seaborn
- Pillow
- scikit-learn
- KaggleHub
- Jupyter Notebook / VS Code Notebook support

> Note: local Windows execution is suitable for setup, EDA, and notebook editing, but full ViT-Large training is intended for a GPU runtime such as Google Colab T4.

## 7. Model and Pipeline Summary

The notebook implements the following modelling setup:

| Component | Configuration |
|---|---|
| Backbone | `google/vit-large-patch16-224` |
| Model family | Vision Transformer Large (ViT-L/16) |
| Input size | `224 x 224 x 3` |
| Source resolution | `512 x 384` |
| Modelling classes | `cardboard`, `glass`, `metal`, `paper`, `plastic` |
| Excluded class | `trash` |
| Split strategy | Stratified `70 / 15 / 15` |
| Final split sizes | `1,673 train`, `358 validation`, `359 test` |
| Training stages | head warm-up + full fine-tuning |

## 8. Workflow (How it Works)

The full pipeline in the notebook follows this sequence:

1. **Environment and authentication setup**  
   Resolve package requirements and fetch Kaggle / Hugging Face credentials through a fallback chain.

2. **Dataset resolution**  
   Load the garbage dataset from local cache or Kaggle and inspect available files.

3. **Class inspection and filtering**  
   Confirm that `trash` is severely underrepresented, then remove it from all modelling steps.

4. **Exploratory data analysis**  
   Inspect class counts, image dimensions, file-size distribution, and sampled channel statistics.

5. **Preprocessing for ViT**  
   Resize the shortest side, apply center crop, normalize with ImageNet statistics, and convert tensors to the format expected by the Hugging Face ViT backbone.

6. **Dataset construction**  
   Build `tf.data.Dataset` pipelines for train, validation, and test splits.

7. **Two-stage fine-tuning**  
   Train the classifier head first, then unfreeze the backbone for full fine-tuning with conservative learning rates.

8. **Evaluation and quantitative error analysis**  
   Report loss and accuracy, then compute a confusion matrix and classification report over the entire test split.

## 9. Final Results Snapshot

The executed main notebook currently reports the following final results for the fine-tuned ViT-Large model:

| Metric | Value |
|---|---:|
| Test Accuracy | 0.8635 |
| Test Loss | 0.4224 |
| Macro F1 | 0.8635 |
| Weighted F1 | 0.8632 |
| Best Validation Accuracy | 0.8827 |
| Best Validation Loss | 0.3610 |

These results indicate that the model performs reasonably well across the five retained classes, with balanced aggregate performance rather than a narrow win on only one class.

## 10. Error Pattern Summary

The final notebook and synced EDA materials now align on the following interpretation:

1. `plastic` is the weakest class overall.
2. The dominant residual confusion is `plastic -> glass` and `glass -> plastic`.
3. `paper -> cardboard` and `cardboard -> paper` still occur, but at a lower rate.
4. The EDA report should therefore treat `cardboard` vs `paper` as an exploratory hypothesis, while the main notebook remains the source of truth for full-test quantitative error patterns.

Key quantitative confusion details from the main notebook:

- `plastic -> glass`: 12 cases, 16.44% of plastic samples
- `glass -> plastic`: 8 cases, 10.67% of glass samples
- `paper -> cardboard`: 4 cases
- `cardboard -> paper`: 3 cases
- `plastic` recall: 0.7671
- `plastic` F1-score: 0.7832

## Repository Structure

```text
2521-deepLearning-forum06/
├── dataset/
├── COLAB_SETUP.md
├── EDA_Report_Garbage.html
├── EDA_Report_Garbage.ipynb
├── Forum06-garbage_classification_question.ipynb
├── Forum06-requirements.txt
├── generate_eda_report.py
├── note.txt
└── README.md
```
