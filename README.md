# Unsupervised Credit Card Fraud Detection

This repository presents a comparative study of **unsupervised anomaly detection methods** for **credit card fraud detection** under **extreme class imbalance** and **limited label availability**.  
The project evaluates a **Deep Autoencoder (AE)** and a **One-Class Support Vector Machine (OC-SVM)** trained exclusively on legitimate transactions and compares their effectiveness in detecting fraudulent behavior.

This work was developed as a final project and reported in *Anomaly Detection Final Report*.

---

## 📌 Project Overview

Credit card fraud detection is challenging due to:
- Extreme class imbalance (fraud < 1%)
- Delayed or unreliable fraud labels
- Continuously evolving fraud patterns (concept drift)

To address these challenges, this project adopts an **unsupervised anomaly detection paradigm**, where models learn the distribution of **normal (legitimate) transactions** and flag deviations as potential fraud.  
Two representative approaches are studied:

- **Reconstruction-based**: Deep Autoencoder (AE)
- **Boundary-based**: One-Class SVM (OC-SVM)

Both models are evaluated using an identical preprocessing, split, threshold selection, and evaluation protocol to enable a fair comparison :contentReference[oaicite:1]{index=1}.

---

## 📂 Dataset

- **Dataset**: European Credit Card Transactions (2013)
- **Source**: ULB / Kaggle
- **Features**:
  - 28 anonymized PCA components (V1–V28)
  - Time
  - Amount
- **Total features**: 30 numerical features

Fraudulent transactions account for **~0.55%** of the test data, reflecting real-world imbalance.

---

## 🧹 Preprocessing & Data Splits

### Preprocessing
- Duplicate transaction removal
- Feature standardization using **normal-only statistics** to prevent data leakage

### Split Strategy
- **Train + Validation (Dev)**: legitimate transactions only
- **Test**: includes both legitimate and fraudulent transactions
- Thresholds are selected **only on the dev split**
- Final metrics are reported on a **held-out test set**

This setup ensures unbiased evaluation in an unsupervised setting.

---

## 🧠 Models Implemented

### 1️⃣ Deep Autoencoder (Reconstruction-Based)

**Architecture**:
  30 → 16 → 8 → 4 → 8 → 16 → 30

- Symmetric bottleneck architecture
- Latent dimension: 4
- Optimizer: Adam (lr = 1e-3)
- Loss: Mean Squared Error (MSE)
- Early stopping and learning-rate reduction applied

**Anomaly Score**:
  s_AE(x) = ||x − x̂||^2

Transactions with reconstruction error above a selected threshold are flagged as fraud.

---

### 2️⃣ One-Class SVM (Boundary-Based)

- Kernel: RBF
- ν = 0.01 (reflecting rarity of anomalies)
- Learns a decision boundary enclosing normal transactions

**Anomaly Score**:

Higher scores indicate higher anomaly likelihood.

---

## 🎯 Threshold Selection & Metrics

Thresholds are selected on the dev set using:
- Percentile-based rules
- F1-score maximization

### Evaluation Metrics
- Precision
- Recall
- F1-score
- ROC-AUC
- PR-AUC (preferred under extreme imbalance)

Both threshold-dependent and threshold-independent metrics are reported.

---

## 📊 Results Summary (Final Test Set)

| Metric | Deep Autoencoder | One-Class SVM |
|------|------------------|---------------|
| Precision | 0.449 | 0.413 |
| Recall | 0.650 | 0.667 |
| F1-score | 0.531 | 0.510 |
| ROC-AUC | 0.961 | 0.946 |
| PR-AUC | 0.440 | 0.384 |
| False Positives | 189 | 225 |

**Key Findings**:
- The **Deep Autoencoder** achieves better overall performance, higher precision, and fewer false positives.
- The **One-Class SVM** attains slightly higher recall but generates more false alarms.
- Reconstruction-based anomaly detection provides a better balance for high-volume financial systems.

---

## 🗂 Repository Structure
.

├── data_preprocessing.ipynb

├── autoencoder_model.ipynb

├── one_class_svm.ipynb

├── evaluation_and_plots.ipynb

├── figures/

├── Anomaly_Detection_Final_Report.pdf

└── README.md


---

## 👥 Team Contributions

- **Zeynep Deniz** – Data handling, preprocessing, feature scaling, AE design  
- **Liza Berfin İnce** – Autoencoder architecture, training strategy, analysis  
- **Sait Kaçmaz** – One-Class SVM methodology, thresholding, evaluation  
- **Mohamad Bader** – Experimental design, unified evaluation protocol, comparative analysis and visualization

---

## 📝 Conclusion

This project demonstrates that **unsupervised anomaly detection** is a viable approach for credit card fraud detection when labels are scarce or delayed.  
Among the evaluated methods, the **Deep Autoencoder** consistently provides a stronger trade-off between fraud detection capability and false positive control, making it a robust baseline for real-world deployment.

---

## 📜 License

This repository is intended for **academic and educational use only**.

