#  Toxicity Prediction Using Molecular Descriptors


## 📋 Project Overview
This project builds a machine learning model to predict whether chemical compounds are **Toxic** or **NonToxic** based on 1203 molecular descriptors. The dataset contains 171 chemical compounds with binary classification.

**Author**: Ndanu-eng
**Date**: March 2026

## 📊 Dataset
- **Samples**: 171 chemical compounds
- **Features**: 1203 molecular descriptors
- **Target**: 56 Toxic (32.7%), 115 NonToxic (67.3%)
- **Challenge**: High dimensionality with limited samples

## 🛠️ Methodology

### 1. Data Preprocessing
- Handled missing values using median imputation
- Removed constant features (none found)
- Scaled features using StandardScaler

### 2. Feature Selection
- Used SelectKBest with ANOVA F-value
- Tested k values from 20 to 100
- **Optimal**: 50 features (CV accuracy = 68.5%)

### 3. Model Building
- **Algorithm**: Random Forest Classifier
- **Cross-validation**: 5-fold Stratified K-Fold
- **Hyperparameters**: n_estimators=100, max_depth=10

## 🎯 Key Results

| Metric | Value |
|--------|-------|
| Cross-Validation Accuracy | 67.6% (±5.1%) |
| Training Accuracy | 100% |
| Test Accuracy | 65.7% |
| Optimal Features | 50 |


### Classification Report
| Class | Precision | Recall | F1-Score |
|-------|-----------|--------|----------|
| NonToxic | 0.70 | 0.88 | 0.78 |
| Toxic | 0.40 | 0.18 | 0.25 |

### Top 5 Most Important Features
| Rank | Feature | Importance | Description |
|------|---------|------------|-------------|
| 1 | ATSC7c | 0.0402 | Centered autocorrelation - lag 7 / weighted by charges |
| 2 | LipoaffinityIndex | 0.0377 | Lipophilicity affinity index |
| 3 | AATSC5m | 0.0372 | Average autocorrelation - lag 5 / weighted by mass |
| 4 | EE_Dt | 0.0361 | Estrada et al. descriptor |
| 5 | SpMin4_Bhs | 0.0340 | Smallest eigenvalue of Burden matrix |

## 📈 Key Insights
1. **Class Imbalance Impact**: Model detects NonToxic well (88% recall) but struggles with Toxic compounds (18% recall)
2. **Feature Reduction**: 50 features optimal; more features don't improve performance
3. **Important Features**: Charge distribution (ATSC7c) and lipophilicity (LipoaffinityIndex) are key toxicity indicators
4. **Overfitting**: Training accuracy (100%) vs Test accuracy (65.7%) indicates some overfitting

