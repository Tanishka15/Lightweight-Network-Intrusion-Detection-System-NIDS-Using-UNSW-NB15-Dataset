
# Lightweight Network Intrusion Detection System (NIDS) Using UNSW-NB15 Dataset

## Project Overview

This project implements a lightweight and efficient machine learning pipeline to detect malicious network activity using the UNSW-NB15 dataset. The system classifies traffic into two categories: normal and attack. It includes robust preprocessing, feature selection, class balancing, and performance evaluation using multiple machine learning models.

## 1. Dataset Description

**UNSW-NB15 Dataset** is a modern and comprehensive dataset for evaluating intrusion detection systems. It includes a mix of normal and malicious traffic with 49 features per record, categorized as:

- Flow features (e.g., duration, bytes, packets)
- Content features (e.g., TCP window size)
- Time features
- Additional aggregated features
- Labels: 
  - `label = 0`: Normal traffic
  - `label = 1`: Malicious traffic

## 2. Project Workflow

### 2.1 Data Loading
- Two CSV files (`UNSW-NB15_1.csv`, `UNSW-NB15_2.csv`) are loaded with correct column headers.
- Concatenated and downsampled to 5,000 rows for faster experimentation.

### 2.2 Data Validation
- Duplicate rows are identified.
- Column names are checked against the expected schema.
- Dropped non-informative identifiers:
  - `srcip`, `sport`, `dstip`, `dsport`, `Stime`, `Ltime`
- Ensured presence of the `label` column.

## 3. Preprocessing Steps

### 3.1 Target Transformation
- Binary encoding of `label`: 
  - `0` for normal 
  - `1` for all types of attacks

### 3.2 Handling Categorical Variables
- Label encoding applied to:
  - `proto`, `state`, `service`, `ct_ftp_cmd`, etc.

### 3.3 Missing Values
- Categorical: Unknown values handled with placeholder labels.
- Numerical: Imputed with median values.

## 4. Feature Engineering

### 4.1 Standardization
- Used `StandardScaler` to normalize numerical features.

### 4.2 Feature Selection
- Applied `SelectKBest` with `mutual_info_classif` to select the most informative features.
- Retained top 15–20 features depending on the pipeline.

## 5. Modeling Approaches

### 5.1 Baseline: Decision Tree Classifier
- Simple tree with `max_depth=5` to establish initial performance benchmarks.

### 5.2 Final Model: Random Forest with SMOTE
- Random Forest with:
  - `n_estimators=100`
  - `max_depth=8`
  - `class_weight='balanced_subsample'`
- SMOTE applied within an `imblearn.pipeline.Pipeline` for oversampling the minority class.
- Cross-validation used to assess generalization.

## 6. Evaluation Metrics

### Cross-Validation
- 5-fold cross-validation using ROC-AUC as the scoring metric.
- Reported mean ROC-AUC with standard deviation.

### Final Testing
- Evaluation metrics:
  - Accuracy
  - ROC-AUC Score
  - Confusion Matrix
  - Classification Report (Precision, Recall, F1-score)

### Example Results
- Accuracy: 0.9910
- ROC-AUC: 0.9989
- Confusion Matrix:
  ```
  [[2817   24]
   [   3  156]]
  ```

## 7. Feature Importance

Top contributing features (example output from Random Forest):
- `ct_state_ttl`
- `sttl`
- `Dload`
- `dmeansz`
- `Dpkts`
- `dttl`
- `dbytes`
- `dur`
- `ackdat`
- `synack`

## 8. Next Steps and Future Enhancements

- Visualizations of ROC and Precision-Recall curves.
- Hyperparameter tuning using GridSearchCV.
- Comparison with other classifiers (e.g., XGBoost, LightGBM).
- Real-time deployment via Flask or FastAPI.
- Integration with packet capture tools (e.g., Scapy, Zeek).
- Stream ingestion using Kafka or RabbitMQ.

## 9. Dependencies

```bash
pandas
numpy
scikit-learn
imblearn
matplotlib (for future visualizations)
```

## 10. File Structure

```
nids_unsw_nb15/
│
├── unsw_pipeline.py              # Main script with full ML pipeline
├── UNSW-NB15_1.csv               # Partial dataset 1
├── UNSW-NB15_2.csv               # Partial dataset 2
├── README.md                     # Project documentation
└── models/                       # (Optional) Saved models for deployment
```

## 11. Conclusion

This project demonstrates an effective and lightweight machine learning approach to building a network intrusion detection system using the UNSW-NB15 dataset. The system balances detection accuracy with performance efficiency, making it suitable for practical deployment scenarios with moderate resource constraints.
