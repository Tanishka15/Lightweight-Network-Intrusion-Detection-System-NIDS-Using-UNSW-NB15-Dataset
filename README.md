
# Network Intrusion Detection System Using Machine Learning

This project implements a lightweight Machine Learning-based Intrusion Detection System (IDS) using the **UNSW-NB15 dataset**. It includes full preprocessing, feature selection, class balancing using SMOTE, and evaluation using Decision Tree and Random Forest classifiers.

---

##  Dataset

The [UNSW-NB15 dataset](https://www.unsw.adfa.edu.au/unsw-canberra-cyber/cybersecurity/ADFA-NB15-Datasets/) contains synthetic contemporary normal and attack activities, used to benchmark network intrusion detection systems.

In this pipeline:
- `UNSW-NB15_1.csv` and `UNSW-NB15_2.csv` are loaded with correct headers.
- Sampled down to 5,000 rows for fast prototyping.
- Columns like `srcip`, `sport`, `dstip`, `dsport`, `Stime`, and `Ltime` are removed.

---

## Pipeline Overview

### 1. Data Preprocessing
- Drop irrelevant identifiers and timestamps
- Handle missing values (numerical and categorical)
- Label encoding of categorical features
- Standardization using `StandardScaler`

### 2. Feature Selection
- `SelectKBest` with `mutual_info_classif` used to retain top `k=15` or `20` features

### 3. Class Balancing
- `SMOTE` used to balance classes and address data imbalance

### 4. Model Training
- Models:
  - `DecisionTreeClassifier` (max_depth=5)
  - `RandomForestClassifier` with `class_weight='balanced_subsample'`
- 5-fold cross-validation using ROC-AUC

### 5. Evaluation
- Accuracy, Confusion Matrix, ROC-AUC
- Classification report with precision, recall, f1-score
- Top important features based on `feature_importances_`

---

## Results

- **Cross-Validation ROC-AUC**: ~0.999  
- **Test Accuracy**: ~99.1%  
- **ROC-AUC on test set**: ~0.9989  

**Confusion Matrix**:
```
[[2817   24]
 [   3  156]]
```

---

## Key Features

| Feature          | Importance |
|------------------|------------|
| `ct_state_ttl`   | 0.2696     |
| `sttl`           | 0.2561     |
| `Dload`          | 0.1068     |
| `dmeansz`        | 0.0702     |
| `Dpkts`          | 0.0499     |

---

## What is SMOTE?

SMOTE (Synthetic Minority Over-sampling Technique) is a technique used to balance imbalanced datasets. It works by creating synthetic samples of the minority class to ensure classifiers are trained more fairly. This improves recall and F1 scores for the attack class.

---

## References

This project was inspired in part by:

- [Intrusion Detection System Using Machine Learning – Western OC2 Lab](https://github.com/Western-OC2-Lab/Intrusion-Detection-System-Using-Machine-Learning), particularly the [LCCDE_IDS_GlobeCom22.ipynb](https://github.com/Western-OC2-Lab/Intrusion-Detection-System-Using-Machine-Learning/blob/main/LCCDE_IDS_GlobeCom22.ipynb) notebook.

I thank the original authors for their valuable contribution. Their repository is licensed under the [MIT License](https://github.com/Western-OC2-Lab/Intrusion-Detection-System-Using-Machine-Learning/blob/main/LICENSE).
