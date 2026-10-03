# Cancer Classification from Gene Expression Data

A machine learning pipeline that classifies leukemia subtypes (**ALL vs. AML**) from gene expression profiles using the classic Golub et al. dataset.

---

## Overview

This project applies dimensionality reduction and machine learning classification to high-dimensional gene expression data. It addresses a fundamental bioinformatics challenge where the feature space (number of genes) vastly outnumbers the sample size (number of patients).

---

## Dataset

* **Source:** [Gene Expression Dataset (Golub et al.)](https://www.kaggle.com) via Kaggle
* **Samples:** 72 total patients
  * **ALL** (Acute Lymphoblastic Leukemia): 47 samples
  * **AML** (Acute Myeloid Leukemia): 25 samples
  * *Note: The original training (38) and test (34) datasets were recombined and re-split for this pipeline.*
* **Features:** 7,129 gene expression measurements per patient

---

## Methodology & Pipeline

1. **Preprocessing:** Merged training and test expression matrices, transposed the data matrix so samples represent rows and genes represent columns, and aligned each patient to their diagnostic label.
2. **Scaling:** Standardized gene expression values ($\mu = 0, \sigma = 1$) prior to feature reduction.
3. **Dimensionality Reduction:** Applied Principal Component Analysis (**PCA**) to compress ~7,000 features into a lower-dimensional subspace while preserving maximal variance.
4. **Classification:** Trained a **Random Forest Classifier** on the top principal components to predict leukemia subtype (ALL vs. AML).
5. **Interpretability:** Analyzed gene feature loadings on the top principal components to identify biological markers driving model predictions.

---

## Results

* **Data Split:** 57 training samples / 15 test samples
* **PCA Variance:** Reduced 7,129 gene features to 20 principal components, capturing **~64% of total variance**.
* **Overall Test Accuracy:** **80%** (12/15 correct predictions)

### Classification Report

| Class | Precision | Recall | F1-Score | Support |
| :--- | :---: | :---: | :---: | :---: |
| **ALL** | 0.82 | 0.90 | 0.86 | 10 |
| **AML** | 0.75 | 0.60 | 0.67 | 5 |

*Performance Note:* The model demonstrates higher predictive accuracy on the ALL class, primarily due to class imbalance (47 ALL vs. 25 AML) and the limited test set size (15 samples). 

---

## Tech Stack

* **Language:** Python
* **Data Processing:** `pandas`, `NumPy`
* **Machine Learning:** `scikit-learn` (`PCA`, `RandomForestClassifier`, `train_test_split`, `metrics`)
* **Visualization:** `matplotlib`, `seaborn`

---

## Why PCA Before Classification?

Training classifiers directly on ~7,000 continuous features with only 72 instances leads to severe overfitting (the *curse of dimensionality*). Using PCA compresses the dataset into orthogonal components capturing dominant expression variance, enabling robust classification while reducing noise sensitivity.

---

## Future Improvements

* **Feature Selection:** Experiment with variance thresholding or differential expression analysis (ANOVA, $t$-tests) as alternatives or pre-filters for PCA.
* **Cross-Validation:** Implement Stratified $K$-Fold cross-validation to get a more reliable performance estimate given the small sample size.
* **Model Benchmarking:** Evaluate lightweight models common in genomic literature, such as Logistic Regression (L1/L2 regularized) or Support Vector Machines (SVM).

---

## How to Run

1. Download your Kaggle API token (`kaggle.json`) from your Kaggle account settings.
2. Open [`cancer_classification_gene_expression.ipynb`](cancer_classification_gene_expression.ipynb) in Google Colab.
3. Upload `kaggle.json` when prompted by the notebook interface.
4. Run all cells sequentially.

---

## Author

**Md Bashirun Sultana**
