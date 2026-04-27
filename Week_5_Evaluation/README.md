# 📊 Week 5 — Final Pipeline (Fairness Correction)

## 📌 Overview

This project implements a **fairness-corrected machine learning pipeline** for heart disease prediction using **SMOTE + Naive Bayes** in KNIME.

The goal is to **reduce bias between male and female patients** while maintaining strong model performance.

---

## 🧠 Final Strategy

👉 **SMOTE (balancing) + Naive Bayes**

This approach was selected because it:

* Produces the **highest female recall (93.3%)**
* Reduces the **gender recall gap from 16.9% → 3.1%**
* Maintains strong overall performance

---

## 🖥️ Requirements

* KNIME Analytics Platform (v4.x or later)

### Required Extensions:

* KNIME Machine Learning
* KNIME Statistics
* KNIME Data Mining (SMOTE)
* KNIME ROC Curve

---

## 📂 How to Open the Workflow

1. Clone or download this repository
2. Open KNIME
3. Go to:
   `File → Import KNIME Workflow`
4. Select the `.knwf` file
5. Open it in your workspace

---

## ▶️ How to Run the Workflow

### Option 1 (Recommended)

* Right-click workflow → **Reset & Execute All**

### Option 2 (Manual Execution)

Run nodes from left → right:

1. CSV Reader (load dataset)
2. Data preprocessing
3. Partitioning (train/test split)
4. SMOTE (training data only)
5. Model training (Naive Bayes)
6. Prediction
7. Evaluation (Scorer + ROC)

---

## ⚙️ Important Nodes & Settings

### 📌 Partitioning

* 80% Training / 20% Testing
* Fixed random seed

### 📌 SMOTE

* Applied **ONLY to training data**
* Balances female (minority) class

### 📌 Normalizer

* Z-score normalization

### 📌 Naive Bayes Learner

* Default settings

### 📌 Evaluation

* Scorer → Accuracy, Recall, Precision, F1
* ROC Curve → AUC
* Row Filters → Male vs Female comparison

---

## 📸 Final Pipeline Screenshot

<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/5fdf144a-e60e-4682-bf80-204086aeb049" />

---

## 📊 Summary of Improvements

| Metric           | Baseline | Final Pipeline |
| ---------------- | -------- | -------------- |
| Overall Accuracy | 86.59%   | ~86.6%         |
| Female Recall    | 90.2%    | **93.3%**      |
| Male Recall      | 73.3%    | **90.2%**      |
| Recall Gap       | 16.9%    | **3.1%**       |

---

## ✅ Key Improvements

* Reduced recall gap by **~82%**
* Improved **female detection performance**
* Balanced predictions across genders
* Maintained strong overall accuracy

---

## ⚖️ Key Insight

* Data imbalance caused the bias
* **SMOTE fixed the imbalance → fairness improved**
* More complex models (Random Forest) improved accuracy, but **hurt fairness**
* Fairness ≠ accuracy → both must be evaluated

---

## 📝 Final Takeaway

This pipeline demonstrates that **balancing the dataset is the most effective way to reduce bias**.
The final model achieves **equitable performance across genders**, making it more suitable for real-world healthcare applications where missing a diagnosis is critical.
