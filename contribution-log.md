# Project Contribution Log

**Project Title:** Clinical Diagnostic Intelligence & Fairness 
**Group:** Team 4 | **Course:** Introduction to Data Mining

---

## 1. Contribution Log

| Member Name | Key Responsibilities | Deliverables / Nodes Owned | Percentage Effort |
|---|---|---|---|
| Morgan Simmons | Team Lead, Introduction & Problem Overview | Title slide, problem overview, project goals | 20% |
| Anique Carey | Dataset & Preprocessing | CSV Reader, One to Many, Normalizer, Table Partitioner, SMOTE | 20% |
| Addison Amadi | Model Explanation | Naive Bayes Learner/Predictor, Random Forest Learner/Predictor, model comparison slide | 20% |
| Patrick Bonna | Data Insights & Fairness Analysis | Scorer nodes, Row Filters, ROC Curve, bias analysis, final report | 20% |
| Nehemiah Shannon | Final Pipeline, Limitations & Contributions | KNIME workflow documentation, limitations slide, contribution log | 20% |

---

## 2. Workflow Documentation

**KNIME Workflow Overview:**
The pipeline loads a 918-patient heart disease dataset, applies one-hot encoding, normalization, and a 70/30 train/test split. SMOTE is applied to the training partition to correct gender imbalance. Two models are trained and compared — Naive Bayes and Random Forest — and evaluated overall and by gender subgroup using Scorer and ROC Curve nodes.

**Key Annotations (Top 3 Metanodes or Annotations):**
1. Preprocessing Metanode — handles encoding, type conversion, normalization, and train/test split
2. SMOTE + Model Training Metanode — applies SMOTE to training data and trains both Naive Bayes and Random Forest
3. Fairness Evaluation Metanode — filters by gender and scores each subgroup separately using Scorer and ROC Curve nodes

**Known Limitations:**
- Male ROC-AUC for Random Forest is unreliable due to heavy class skew in the male test subset
- The female test subset contains only 52 patients, making percentage metrics sensitive to individual misclassifications
- Cholesterol values of 0 were not removed and may affect model reliability for those patients

---

## 3. Group Agreement & Attestation

By signing below, all team members confirm that the work contained within this repository is the result of a collaborative effort and that all individual contributions are fairly represented in the log above.

| Member Name | Signature | Date |
|---|---|---|
| Addison Amadi | AA | 04/01/2026 |
| Nehemiah Shannon | NS | 04/01/2026 |
| Morgan Simmons | MS | 04/01/2026 |
| Anique Carey | AC | 04/01/2026 |
| Patrick Bonna | PB | 04/01/2026 |

---
