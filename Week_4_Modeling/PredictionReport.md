## KNIME Workflow

[You can view the full interactive KNIME workflow here:

[View KNIME Workflow] https://hub.knime.com/s/7SeXaV320mF27GL5
](https://hub.knime.com/s/ON5LHn89EdafgAAF)

# Week 4 — Model Creation & Fairness Analysis

## Objective

The objective of this assignment was to build an end-to-end prediction pipeline in KNIME to detect heart disease and evaluate whether the model performs equally well for male and female patients. A key focus was identifying bias and proposing solutions to improve fairness while maintaining strong predictive performance.

---

## Part 1 — Pipeline Construction

An end-to-end KNIME workflow was developed to automate data processing, model training, and prediction generation.

### 📸 Figure 1 — Full KNIME Workflow Overview
<img width="360" height="172" alt="image" src="https://github.com/user-attachments/assets/4aae4e31-dfe1-4321-9eee-ec47bf3426e2" />

- Show entire pipeline (zoomed out)
---

### Pipeline Overview

The pipeline begins with loading the cleaned dataset using the CSV Reader node. Preprocessing steps were applied to prepare the data for modeling.

Categorical variables such as `Sex`, `ChestPainType`, `RestingECG`, `ExerciseAngina`, and `ST_Slope` were transformed using One-Hot Encoding via the One to Many node.

### 📸 Figure 2 — One-Hot Encoding Configuration (One to Many Node)

- Show included categorical columns
- Show excluded numeric columns
<img width="457" height="296" alt="image" src="https://github.com/user-attachments/assets/0978e982-9924-439f-b785-0970cd1eb475" />

---

Binary columns such as `FastingBS` and `HeartDisease` were converted into categorical format.

### 📸 Figure 3 — Number to String Configuration
<img width="463" height="298" alt="image" src="https://github.com/user-attachments/assets/57a6cdc1-934f-42c7-8de5-2c10cfac7f30" />

- Show selected columns (`FastingBS`, `HeartDisease`)

---

The dataset was split into training (70%) and testing (30%) sets using the Table Partitioner node.

### 📸 Figure 4 — Table Partitioner Configuration
<img width="461" height="301" alt="image" src="https://github.com/user-attachments/assets/f6d437c6-e95c-4f95-b05b-d86c201d9e73" />

- Show 70/30 split
- Show relative sampling selected

---

Continuous variables were normalized using Z-score normalization.

### 📸 Figure 5 — Normalizer Configuration
<img width="467" height="302" alt="image" src="https://github.com/user-attachments/assets/58a1ad91-285d-420d-a9de-db820aa4d855" />

- Show Z-score selected
- Show only numeric columns included

---

A Naive Bayes model was trained and used to generate predictions.

### 📸 Figure 6 — Naive Bayes Learner Configuration
<img width="463" height="310" alt="image" src="https://github.com/user-attachments/assets/e127a231-ea5b-473a-9f9e-bf4b9988d6cb" />

- Show class column = `HeartDisease`

---

## Part 2 — Gender-Based Evaluation

Model performance was evaluated using Scorer nodes.

### 📸 Figure 7 — Overall Model Performance (Scorer Output)
<img width="469" height="299" alt="image" src="https://github.com/user-attachments/assets/e699cd0f-afd2-469c-8e83-c0d5270a8bb7" />

- Confusion matrix
- Accuracy, precision, recall visible

---

### Overall Performance

- **Accuracy:** 87.7%  
- **Recall (Disease):** 88.6%  
- **Recall (No Disease):** 86.4%  

---

### 📸 Figure 8 — Row Filter Configuration (Male vs Female)
<img width="463" height="305" alt="image" src="https://github.com/user-attachments/assets/0dcaa912-0470-4e19-8e28-a489dde2a146" />

- Show:
  - `Sex = M`
  - `Sex = F`

---

### 📸 Figure 9 — Male Model Results (Scorer Output)
<img width="475" height="291" alt="image" src="https://github.com/user-attachments/assets/56011e2b-4672-4436-999b-242d109e13f3" />

- Confusion matrix for male patients

---

### Male Performance

- **Accuracy:** > 85%  
- **Precision:** High  
- **Recall:** 90.2%  
- **F1-Score:** > 85%  
- **ROC-AUC:** > 80%  

---

### 📸 Figure 10 — Female Model Results (Scorer Output)
<img width="464" height="302" alt="image" src="https://github.com/user-attachments/assets/ae062856-6fea-4f27-85f3-ac85eded44e7" />

- Confusion matrix for female patients

---

### Female Performance

- **Accuracy:** > 80%  
- **Precision:** High  
- **Recall:** 73.3%  
- **F1-Score:** Lower than male  
- **ROC-AUC:** Below target  

---

### Key Observation

Female recall (73.3%) is significantly lower than male recall (90.2%), indicating that the model misses more heart disease cases in female patients.

---

## Part 3 — Bias Investigation

### Key Finding

There is a significant recall gap between male and female patients, meaning female patients are more likely to be misclassified as healthy.

### 📸 Figure 11 — Female False Negative Cases (Optional but Strong)
> **Insert Screenshot Here**
- Filter:
  - `Sex = F`
  - `HeartDisease = 1`
  - `Prediction = 0`

---

### Causes of Bias

#### 1. Class Imbalance
The dataset contains significantly more male patients than female patients, leading the model to learn patterns primarily from male data.

#### 2. Feature Importance Differences
Important predictors such as:
- `ChestPainType`
- `ExerciseAngina`
- `ST_Slope`

are more indicative of heart disease in males than females.

#### 3. False Negative Female Cases
Female patients who were misclassified often present atypical symptoms and may not exhibit strong indicators such as asymptomatic chest pain (ASY) or abnormal ST slope patterns.

#### 4. Unequal Feature Effectiveness
The predictive features do not capture female-specific disease patterns effectively, reducing model performance for females.

---

## Part 4 — Proposed Fixes

### 1. Threshold Adjustment

Lowering the classification threshold for predicting heart disease in female patients increases sensitivity and reduces missed diagnoses.

**Tradeoff:**
- Higher recall
- Lower precision (more false positives)

---

### 2. Oversampling Female Patients

Using oversampling techniques such as random duplication or SMOTE increases representation of female patients in the training dataset.

**Tradeoff:**
- Improved fairness and recall
- Risk of overfitting

---

### 3. Separate Models by Sex

Training separate models for male and female patients allows each model to learn sex-specific patterns.

**Tradeoff:**
- Improved accuracy and fairness
- Increased complexity and reduced data per model

---

## Precision vs Recall in Clinical Context

- **Recall:** Ability to detect actual disease cases  
- **Precision:** Accuracy of predicted disease cases  

In healthcare screening:
- Recall is more important than precision  
- False negatives are more dangerous than false positives  

---

## Conclusion

The Naive Bayes model achieved strong overall performance but demonstrated bias against female patients, particularly in recall.

### Key Takeaways

- Female patients are underrepresented and more likely to be misclassified  
- Model features align more closely with male disease patterns  
- Improving recall is critical in clinical applications  

### Recommended Solutions

- Apply threshold adjustment  
- Use oversampling  
- Consider separate models  

---

## Deliverables

- KNIME Workflow (`.knwf`)  
- Written report  
- Screenshots included throughout report  
