# Deep Learning Forum 06: Indonesian Food Classification

**Student Name:** Yanlis Alim Sang Putra Lase  
**Student ID:** 2702751284  
**Program:** Master's in Informatics, BINUS Graduate Program

This repository contains the implementation and documentation for Forum 04, a deep learning assignment focused on multiclass Indonesian food image classification using Convolutional Neural Networks (CNN).

## 1. Project Overview

This project builds an end-to-end deep learning pipeline for multiclass food-image classification. The workflow starts from Kaggle dataset acquisition, continues through folder-based label extraction and image preprocessing, and ends with model training and evaluation on held-out validation and test splits.

The main objective is comparative and methodological: to evaluate how a custom CNN architecture performs against a transfer learning approach based on VGG16 when both models are trained on the same Indonesian food dataset.

The completed experiment in this repository shows that the VGG16-based model outperforms the custom CNN on both validation and test data.

## 2. Data Source

The image dataset is obtained from Kaggle:

- **Kaggle dataset:** `rizkyyk/dataset-food-classification`

The dataset contains 13 Indonesian food categories organized into class folders and three splits: training, validation, and test. After local caching, the project uses 6,490 images stored under the repository `dataset/` folder.

| No. | Food Category | Description |
|---|---|---|
| 1 | Ayam Goreng | Fried chicken dishes |
| 2 | Burger | Burger-style fast food |
| 3 | French Fries | Fried potato side dish |
| 4 | Gado-Gado | Indonesian vegetable salad with peanut sauce |
| 5 | Ikan Goreng | Fried fish dishes |
| 6 | Mie Goreng | Fried noodle dishes |
| 7 | Nasi Goreng | Indonesian fried rice |
| 8 | Nasi Padang | Padang-style rice meals |
| 9 | Pizza | Pizza dishes |
| 10 | Rawon | East Javanese beef soup |
| 11 | Rendang | Slow-cooked spiced beef |
| 12 | Sate | Skewered grilled meat |
| 13 | Soto Ayam | Indonesian chicken soup |

## 3. Function and Purpose

The notebook is designed to perform the following core functions:

1. Download and cache the Indonesian food dataset locally using KaggleHub.
2. Convert folder names into model-friendly labels.
3. Associate each image path with the correct class label.
4. Perform image-based exploratory data analysis covering class balance, split composition, image geometry, and qualitative sample inspection.
5. Standardize image sizes to `(224, 224, 3)` for CNN processing.
6. Train and evaluate two model families:

- **Model 01:** Custom CNN architecture
- **Model 02:** VGG16-based transfer learning model

7. Compare model performance using `loss`, `accuracy`, and `top_3_accuracy`.

## 4. Expected Output

The expected deliverables of this project are:

1. **A completed training notebook** for the Indonesian food classification task.
2. **An exploratory data analysis notebook** for the image dataset.
3. **An HTML EDA report** derived from the exploratory analysis.
4. **Saved Keras model files** for the best CNN and VGG16 variants.
5. **Training-history plots** showing loss, accuracy, and top-3 accuracy.
6. **A final model comparison table** summarizing validation and test metrics.

### Quick Access: EDA Report (HTML)

Click here to open the published EDA report directly:

- **EDA Report:** [https://yanlis-lase-ssg7.github.io/2521-deepLearning-forum04/EDA_Report_Indonesian_Food.html](https://yanlis-lase-ssg7.github.io/2521-deepLearning-forum04/EDA_Report_Indonesian_Food.html)

## 5. Step-by-Step Installation and Usage

Run the following commands in PowerShell from your preferred working directory.

### 5.1 Git Clone

```powershell
git clone https://github.com/yanlis-lase-SSG7/2521-deepLearning-forum06.git
cd 2521-deepLearning-forum04
```

### 5.2 Create Virtual Environment

```powershell
python -m venv venv
```

### 5.3 Activate Virtual Environment (Windows)

```powershell
.\venv\Scripts\activate
```

### 5.4 Set Kaggle Token

```powershell
$env:KAGGLE_API_TOKEN="YOUR_KAGGLE_API_TOKEN"
```

Alternative authentication methods supported by the notebooks:

- `~/.kaggle/kaggle.json`
- `~/.kaggle/access_token`
- Google Colab secret `KAGGLE_API_TOKEN`
- Interactive `kagglehub.login()` prompt

### 5.5 Install Dependencies

```powershell
pip install -r Forum06-requirements.txt
```

### 5.6 Run the Main Notebook

Open and execute:

- `Forum04-indonesian_food_question.ipynb`

For reproducible results, run notebook cells sequentially from top to bottom.

### 5.7 Run the EDA Notebook

Open and execute:

- `EDA_Report_Indonesian_Food.ipynb`

After the notebook is executed, export it to HTML if you want a refreshed static report that matches the latest notebook output.

## 6. Technical Requirements

The technical stack for this project includes:

- Python 3.10+
- TensorFlow 2.x
- NumPy
- Pandas
- Matplotlib
- Seaborn
- Pillow
- KaggleHub
- Jupyter Notebook / VS Code Notebook support

> Note (Windows): TensorFlow in this project is configured to run on CPU mode for native Windows compatibility.

## 7. Architecture Details

The following table summarizes the two image-classification models used in this assignment:

| Component | Model 01 | Model 02 |
|---|---|---|
| Backbone | Custom CNN | Pretrained VGG16 |
| Input shape | `224 x 224 x 3` | `224 x 224 x 3` |
| Feature extraction | Learned from scratch | Transfer learning from ImageNet |
| Head | Dense MLP classifier | Dense MLP classifier |
| Main purpose | Baseline architecture | Higher-capacity benchmark |

## 8. Workflow (How it Works)

The full pipeline is implemented as a structured sequence:

1. **Authentication and Dataset Retrieval**  
	Check local `dataset/` cache first and download from Kaggle only when images are missing.

2. **Folder-Based Label Extraction**  
	Read subfolder names from the training split and convert them into machine-friendly labels.

3. **Image Path Labeling**  
	Associate each image path from train, validation, and test folders with the correct category label.

4. **Exploratory Data Analysis**  
	Inspect label-count distribution, split composition, and image-size variation to understand class balance and preprocessing needs.

5. **Image Standardization**  
	Resize the shortest side to 224 pixels and apply center crop to produce a consistent `(224, 224, 3)` tensor.

6. **TensorFlow Dataset Construction**  
	Convert image paths and labels into efficient `tf.data.Dataset` pipelines for model training.

7. **Training**  
	Train both the custom CNN and the VGG16-based model with early stopping, learning-rate reduction, and checkpointing.

8. **Evaluation and Comparison**  
	Compare train, validation, and test performance using loss, top-1 accuracy, and top-3 accuracy.

## 9. Final Results Snapshot

The main notebook currently reports the following final comparison:

| Model | Validation Accuracy | Test Accuracy | Validation Top-3 Accuracy | Test Top-3 Accuracy |
|---|---:|---:|---:|---:|
| Custom CNN | 0.792 | 0.776 | 0.940 | 0.918 |
| VGG16 Transfer Learning | 0.885 | 0.895 | 0.971 | 0.977 |

These results indicate that the VGG16-based transfer learning model is the strongest model in the current experiment.

## Repository Structure

```text
2521-deepLearning-forum04/
├── dataset/
│   ├── train/
│   ├── valid/
│   └── test/
├── keras_model/
│   ├── indonesian_food_model.keras
│   └── vgg_indonesian_food_model.keras
├── EDA_Report_Indonesian_Food.html
├── EDA_Report_Indonesian_Food.ipynb
├── Forum04-indonesian_food_question.ipynb
├── Forum04-requirements.txt
└── README.md
```
