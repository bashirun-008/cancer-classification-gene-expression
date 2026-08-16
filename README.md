Cancer Classification from Gene Expression Data
A machine learning pipeline that classifies leukemia subtype (ALL vs. AML) from gene expression profiles, using the classic Golub et al. leukemia dataset.
Overview
This project applies dimensionality reduction and classification to high-dimensional gene expression data — a core bioinformatics problem where the number of features (genes) far exceeds the number of samples (patients).
Dataset
Source: Gene Expression Dataset (Golub et al.) via Kaggle
Samples: 72 patients total (38 in the original training file, 34 in the independent test file, recombined and re-split for this project), each labeled as ALL (Acute Lymphoblastic Leukemia, 47 samples) or AML (Acute Myeloid Leukemia, 25 samples)
Features: 7,129 gene expression measurements per patient
Approach
Preprocessing: merged the training and independent test expression files, transposed the data so patients are rows and genes are columns, and matched each patient to their diagnosis label
Scaling: standardized all gene expression values before dimensionality reduction
Dimensionality reduction: applied PCA to reduce ~7,000 gene features down to a small number of principal components, since the number of genes far exceeds the number of patients
Classification: trained a Random Forest classifier on the reduced feature space to predict ALL vs. AML
Interpretation: examined which genes contribute most heavily to the top principal components, to connect the model back to biologically meaningful features rather than treating it as a black box
Results
Dataset size: 72 patients total (47 ALL, 25 AML) — 57 for training, 15 held out for testing
PCA: reduced 7,129 gene features to 20 principal components, capturing ~64% of total variance
Test accuracy: 80% (12/15 correct)
Class
Precision
Recall
F1-score
Support
ALL
0.82
0.90
0.86
10
AML
0.75
0.60
0.67
5
The model performs noticeably better on ALL than AML, likely reflecting the class imbalance in the dataset (47 ALL vs. 25 AML samples) and the very small test set (only 15 samples) — with this few samples, each misclassification shifts the reported metrics substantially, so these numbers should be read as a first estimate rather than a precise measure of real-world performance.
Tech Stack
Python
pandas, NumPy
scikit-learn (PCA, Random Forest, train/test split, evaluation metrics)
matplotlib, seaborn (visualization)
Why PCA Before Classification
With far more genes (~7,000) than patients (~72), training a classifier directly on raw gene expression risks severe overfitting. PCA compresses the feature space into a smaller number of components that capture most of the variance in the data, making the classification problem tractable and reducing the risk of the model memorizing noise.
Next Steps
Try feature selection methods (e.g., selecting genes by variance or differential expression) as an alternative or complement to PCA
Cross-validate rather than relying on a single train/test split, given the small sample size
Compare Random Forest against simpler models (e.g., logistic regression, SVM) which are often used in the genomics literature for small-sample, high-dimensional problems
How to Run
Get a Kaggle API token (kaggle.json) from your Kaggle account settings
Open cancer_classification_gene_expression.ipynb in Google Colab
Upload your kaggle.json when prompted
Run all cells in order
Author
Md Bashirun Sultana
