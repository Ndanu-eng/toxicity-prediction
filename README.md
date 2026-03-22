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
## 🎛️ Hyperparameter Tuning

### Hyperparameters Tuned
| Hyperparameter | Description | Range Tested |
|---------------|-------------|--------------|
| `n_estimators` | Number of trees in the forest | 50, 100, 150, 200 |
| `max_depth` | Maximum depth of each tree | 5, 8, 10, 12, 15 |
| `min_samples_split` | Minimum samples required to split a node | 2, 5, 7, 10 |
| `min_samples_leaf` | Minimum samples required at a leaf node | 1, 2, 3, 4 |
| `max_features` | Number of features to consider per split | 'sqrt', 'log2' |

### Tuning Methods

**Grid Search** (Exhaustive):
- Tried all 324 combinations of hyperparameters
- Computationally intensive: 1,620 model trainings (324 × 5-fold CV)
- Time: ~15-20 minutes (not feasible for all combinations)

**Random Search** (Efficient):
- Sampled 30 random combinations from the parameter space
- Only 150 model trainings (30 × 5-fold CV)
- Time: ~2-3 minutes

### Results Comparison

| Model | CV Accuracy | Test Accuracy | Best Parameters |
|-------|-------------|---------------|-----------------|
| Baseline (Default) | 67.6% | 65.7% | n_estimators=100, max_depth=10, default settings |
| Random Search | **69.2%** | **68.6%** | n_estimators=150, max_depth=8, min_samples_split=7, min_samples_leaf=2, max_features='sqrt' |

### Discussion: Hyperparameter Impact

**1. n_estimators (Number of Trees)**
- Default: 100 → Best: 150
- **Impact**: Increasing to 150 improved accuracy by ~1.6%. More trees create a more stable and robust ensemble.

**2. max_depth (Tree Depth)**
- Default: None (unlimited) → Best: 8
- **Impact**: Limiting depth to 8 prevented overfitting. Full-depth trees achieved 100% training accuracy but lower test accuracy (65.7%). Depth 8 balanced complexity and generalization.

**3. min_samples_split (Min Samples to Split)**
- Default: 2 → Best: 7
- **Impact**: Increasing to 7 created simpler, more generalized trees. Lower values (2-5) led to overfitting to training data.

**4. min_samples_leaf (Min Samples in Leaf)**
- Default: 1 → Best: 2
- **Impact**: Setting to 2 smoothed predictions and reduced noise. Values of 1 allowed overly specific rules.

**5. max_features (Features per Split)**
- Default: 'sqrt' → Best: 'sqrt'
- **Impact**: Using 'sqrt' increased tree diversity. Using all features ('None') made trees more correlated, reducing ensemble benefits.

### Key Takeaways from Hyperparameter Tuning

1. **Performance Improvement**: CV accuracy improved from 67.6% to 69.2% (+1.6%), test accuracy from 65.7% to 68.6% (+2.9%)

2. **Overfitting Reduction**: Training accuracy decreased from 100% to ~95%, indicating better generalization

3. **Computational Trade-off**: Random Search was 6-8× faster than exhaustive Grid Search while achieving comparable results

4. **Best Configuration**: Moderate tree depth (8), slightly higher split requirements (7), and limited features per split ('sqrt')

---
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
1. **Hyperparameter Tuning Matters**: Tuning improved test accuracy by 2.9%, demonstrating the value of optimizing model parameters
2.  **Class Imbalance Impact**: Model detects NonToxic well (88% recall) but struggles with Toxic compounds (18% recall)
3. **Feature Reduction**: 50 features optimal; more features don't improve performance
4. **Important Features**: Charge distribution (ATSC7c) and lipophilicity (LipoaffinityIndex) are key toxicity indicators
5. **Overfitting**: Training accuracy (100%) vs Test accuracy (65.7%) indicates some overfitting

