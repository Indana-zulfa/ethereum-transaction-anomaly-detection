# Interpretable Feature-Scenario Analysis for Ethereum Transaction Anomaly Detection

An interpretable machine learning study for detecting anomalous transactions in Ethereum blockchain data using **Random Forest** and **XGBoost**, with feature-scenario analysis and explainability techniques.

## Overview

Blockchain transaction data contains large volumes of transactions with different behavioral characteristics. Detecting anomalous transactions is challenging because anomalous activities represent only a small portion of the dataset and transaction patterns may change over time.

This project investigates how different feature combinations affect Ethereum transaction anomaly detection performance. The study compares multiple machine learning models and feature scenarios while evaluating both conventional random split validation and **future-block validation** to assess model generalization to newer blockchain data.

> **Note:** The target variable `isError` is used as an anomaly-indicating proxy label. It does not represent verified fraud or confirmed malicious activity.

## Research Objectives

This project aims to:

* Analyze the contribution of different transaction feature groups to anomaly detection.
* Compare the performance of **Logistic Regression, Random Forest, and XGBoost**.
* Evaluate the effect of feature scenarios on model performance.
* Compare model performance using random split and future-block validation.
* Interpret model predictions using **SHAP** and other explainability techniques.
* Analyze false alerts and the practical trade-off between anomaly detection and false positives.

## Dataset

The dataset contains Ethereum transaction records collected from the Ethereum blockchain.

After transaction hash deduplication, the dataset contains approximately **4.6 million unique transactions**, consisting of:

* **4,439,278 normal transactions**
* **165,277 anomaly-indicating transactions**
* Anomaly proportion: approximately **3.59%**

The target variable used in this study is:

```text
isError
0 = normal / successful transaction
1 = anomaly-indicating transaction
```

Because `isError` is not a verified fraud label, the results should be interpreted as **transaction anomaly detection**, rather than confirmed fraud detection.

## Features

The main features used in the experiments include:

* `From_encoded` — encoded sender address
* `To_encoded` — encoded receiver address
* `Value` — transaction value
* `BlockHeight` — Ethereum blockchain block height
* `Hour` — transaction hour

## Feature Scenarios

Four feature scenarios were evaluated to investigate the contribution of different feature groups.

| Scenario        | Features                                      |
| --------------- | --------------------------------------------- |
| Baseline        | From_encoded, To_encoded, Value               |
| Full Feature    | Baseline + BlockHeight + Hour                 |
| Without Address | Value + BlockHeight + Hour                    |
| Without Hour    | From_encoded, To_encoded, Value + BlockHeight |

The scenario-based approach allows the study to examine whether address-related, temporal, and blockchain-position features contribute to anomaly detection performance.

## Machine Learning Models

The following models were evaluated:

* **Logistic Regression**
* **Random Forest**
* **XGBoost**

Random Forest and XGBoost were selected because they can model nonlinear relationships and interactions between transaction features, while Logistic Regression provides a simpler baseline for comparison.

## Experimental Design

The experiments use two validation strategies:

### 1. Random Stratified Split

The dataset is divided into training and testing sets while maintaining the class distribution.

* Training set: 80%
* Test set: 20%
* Random seed: 42

### 2. Future-Block Validation

Transactions are separated chronologically based on Ethereum block height.

Earlier blockchain blocks are used for training, while later blocks are kept as the test set.

This validation strategy provides a more realistic assessment of whether a model trained on historical blockchain transactions can generalize to future transaction patterns.

## Evaluation Metrics

Because the anomaly class is highly imbalanced, multiple evaluation metrics are used:

* Precision
* Recall
* F1-score
* PR-AUC
* Matthews Correlation Coefficient (MCC)
* False Positive Rate (FPR)
* Confusion Matrix

PR-AUC and MCC are particularly useful for evaluating performance under class imbalance.

## Interpretability

Model interpretability is performed to understand which features contribute most strongly to anomaly predictions.

The project uses explainability techniques such as:

* **SHAP (SHapley Additive exPlanations)**
* **LIME**, where applicable

SHAP analysis is used to examine global feature importance and the direction of feature contributions to model predictions.

## Key Findings

The experiments show that feature selection and validation strategy have a substantial impact on anomaly detection performance.

The best-performing configuration under the random-split experiments achieved strong anomaly recall, but its performance decreased considerably under future-block validation.

This indicates that high performance on a random split does not necessarily guarantee strong generalization to newer Ethereum transactions.

The results also highlight a trade-off between detecting a larger proportion of anomalies and generating false alerts. Therefore, model evaluation should not rely on a single metric such as accuracy or recall.

## Technologies

* Python
* Pandas
* NumPy
* Scikit-learn
* XGBoost
* SHAP
* LIME
* Matplotlib
* Seaborn
* Jupyter Notebook

## Project Structure

```text
ethereum-transaction-anomaly-detection/
│
├── README.md
├── ethereum_anomaly_detection.ipynb
└── requirements.txt
```

## Research Publication

This project is based on the research:

**Interpretable Feature-Scenario Analysis for Ethereum Transaction Anomaly Detection Using Random Forest and XGBoost**

Published: **August 2026**

## Author

**Indana Zulfa**

Bachelor's Degree in Informatics
Universitas Amikom Yogyakarta
