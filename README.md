# Integrated-Data-Engineering-and-Balanced-LSTM-Modeling-
Integrated Data Engineering and Balanced LSTM Modeling for Leak-Free Hypoglycemia Prediction During Ramadan
Abstract
Objective. To develop a leak‑free pipeline that integrates clinical, CGM, and wearable data to predict hourly hypoglycemia (≤70 mg/dL) during Ramadan and Shawwal, and to benchmark recurrent models under class imbalance.

Methods. We harmonized participant identifiers across seven clinical visits and intraday streams, programmatically tagged Ramadan/Shawwal windows, and ran a percent‑aware quality audit. From CGM we derived hourly statistics (min, max, mean, SD) and a 3‑component CGM PCA; from Huawei wearables we aggregated steps, energy, heart rate, SpO₂, and sleep, then projected to a 3‑component lifestyle PCA. The outcome label per hour was 1 if any CGM ≤70 mg/dL (sensitivity analyses consider ≤54 mg/dL and hypo‑minutes). To avoid information leakage, patients (not samples) were split before any scaling/PCA; scalers and PCA were fit on train patients and applied unchanged to test patients. Sequences used 24‑hour lookbacks with feature triplets [cgm_mean, cgm_std, pc1_activity_energy]. We trained five recurrent models (LSTM‑100/50/25‑L1/25‑L2, BiLSTM) with focal loss (γ=2, α=0.25), and compared six imbalance strategies (none, oversample, undersample, SMOTE, SMOTE+ENN, SMOTE+Tomek). Thresholds were tuned in 0.40–0.60 to maximize PR‑F1.

Results. The engineering pipeline yielded ≈19 k hourly windows; train/test sequences were (15 173, 24, 3) and (3 722, 24, 3), respectively. CGM PCA’s first three components explained ≈99% of variance; lifestyle PCA (activity/energy, physiology, sleep/rest) explained ≈64%. On the original (imbalanced) test, the best baseline (LSTM‑50) achieved weighted F1=0.9626, ROC‑AUC=0.886, accuracy=96.6%. On a balanced test, the leaders were BiLSTM/undersample and LSTM‑100/undersample with weighted F1≈0.820, ROC‑AUC≈0.89, PR‑AUC≈0.90; optimal decision thresholds concentrated near 0.41±0.01. Training was stable; each model converged within ≈60–80 s on a Tesla P100.

Conclusion. Leak‑free, patient‑split LSTM/BiLSTM models trained with focal loss and moderate undersampling provide robust, interpretable hour‑ahead hypoglycemia risk during fasting. The pipeline generalizes across Ramadan/Shawwal and complements a broader all‑day risk framework grounded in CGM + behavior signals and rigorous leakage prevention. Hourly construction, mg/dL conversion, and leakage‑avoidance conventions follow the study’s predefined methodology.


Leak-Free Hypoglycemia Prediction During Ramadan
Overview
This repository presents a leak-free, patient-aware machine learning pipeline for hour-ahead hypoglycemia prediction (≤70 mg/dL) using multimodal data collected during Ramadan and Shawwal.

The framework integrates CGM data, wearable signals (activity, heart rate, SpO₂, sleep), and clinical variables, and evaluates recurrent deep learning models under class imbalance.
Objective
To develop a robust system that predicts hypoglycemia 1 hour ahead, prevents data leakage, handles class imbalance, and integrates behavioral and physiological signals.
Data Sources
- CGM data (Medtronic systems)
- Wearable data (Huawei Band 6)
- Clinical visits across 7 time points
- Ramadan and Shawwal segmentation
Pipeline Summary
1. Data Engineering: ID harmonization, Ramadan tagging, quality audit
2. Feature Engineering: CGM + wearable PCA features
3. Label Definition: Binary hypoglycemia (≤70 mg/dL)
4. Leakage-Free Design: Patient-level split before preprocessing
5. Sequence Construction: 24-hour sliding windows
Models
LSTM (100, 50, 25), regularized LSTM (L1/L2), BiLSTM with focal loss.
Imbalance Handling
None, oversampling, undersampling, SMOTE, SMOTE+ENN, SMOTE+Tomek
Results
Imbalanced test: LSTM-50 → F1=0.9626, ROC-AUC=0.886
Balanced test: BiLSTM & LSTM-100 → F1≈0.82, ROC≈0.89
Threshold ≈ 0.41 ± 0.01
Training time: 60–80 seconds
Key Contributions
- Leak-free pipeline
- Multimodal integration
- Robust imbalance handling
- Ramadan-specific modeling
- Clinically interpretable features
Repository Structure
HMC Statistical Overview – exploration
HMC Ramadan datasets – balanced/imbalanced
HMC LSTM – models
article-1-geometric.ipynb – related work
How to Run
1. Prepare datasets
2. Run preprocessing
3. Generate features
4. Train models
5. Evaluate
Notes & Limitations
Model may be device-specific (Medtronic CGM, Huawei Band 6). External validation required.

| File / Notebook                  | Description                    |
| -------------------------------- | ------------------------------ |
| `HMC Statistical Overview`       | Dataset exploration            |
| `HMC Ramadan balanced datasets`  | Balanced dataset experiments   |
| `HMC Ramadan Imbalance datasets` | Imbalanced dataset experiments |
| `HMC LSTM on all Features`       | Full multimodal model          |
| `Dynamic / Static notebooks`     | Feature-specific analyses      |
| `article-1-geometric.ipynb`      | Related geometric modeling     |

How to Run
Prepare datasets (CGM + wearable + clinical)
Run preprocessing pipeline
Generate hourly features + sequences
Train models (LSTM / BiLSTM)
Evaluate on patient-level split
--------------------------------------------------------------
Pipeline Summary
🔹 1. Data Engineering
Harmonized patient IDs across visits and modalities
Tagged Ramadan vs Shawwal periods
Performed percent-aware quality audit
--------------------------------------------------------------
🔹 2. Feature Engineering

CGM Features

Hourly: min, max, mean, standard deviation
PCA → 3 components (~99% variance explained)

Wearable Features

Steps, energy, HR, SpO₂, sleep
PCA → lifestyle representation (~64% variance)
🔹 3. Label Definition
Binary label per hour:
1 if any CGM ≤ 70 mg/dL
Sensitivity analysis:
≤ 54 mg/dL
hypoglycemia duration
🔹 4. Leakage-Free Design (CRITICAL)
Patient-level split BEFORE preprocessing
PCA + scaling fit ONLY on training patients
Applied unchanged to test patients

This ensures true generalization, not inflated results.
--------------------------------------------------------
Sequence Construction
24 then 36 -hour sliding windows
Input features example:
CGM mean
CGM variability
Activity-energy PCA

Shape:

Train: (15,173, 24, 3)
Test: (3,722, 24, 3)
🤖 Models
LSTM (100, 50, 25 units)
Regularized LSTM (L1 / L2)
BiLSTM

All trained with:

Focal loss (γ=2, α=0.25)
Optimized for imbalanced classification
----------------------------------------------------
Imbalance Handling

Compared 6 strategies:

None
Oversampling
Undersampling
SMOTE
SMOTE + ENN
SMOTE + Tomek

---------------------------------------------
 Results
🔹 Imbalanced Test Set
Best model: LSTM-50
Weighted F1: 0.9626
ROC-AUC: 0.886
Accuracy: 96.6%
🔹 Balanced Test Set
Top performers:

BiLSTM + undersampling
LSTM-100 + undersampling

Performance:

F1 ≈ 0.82
ROC-AUC ≈ 0.89
PR-AUC ≈ 0.90
### Threshold Optimization
Optimal threshold: ~0.41 ± 0.01
### Training Efficiency
Convergence: 60–80 seconds
Hardware: Tesla P100
----------------------------------------
## Key Contributions
Fully leak-free pipeline
Integration of multimodal physiological + behavioral data
Robust handling of class imbalance
Hour-ahead prediction tailored to Ramadan fasting context
Clinically interpretable feature design

License
Apache 2.0

