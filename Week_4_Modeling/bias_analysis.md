# Heart Disease Bias Analysis — KNIME Naive Bayes Workflow

**Person 4 Assignment** | Comparing male vs. female model performance using Naive Bayes on heart disease data.

---

## Dataset

- **Rows:** 918 patients
- **Columns:** 12 features + 1 target (`HeartDisease`)
- **Split:** 70% training / 30% test

| Column | Type | Description |
|---|---|---|
| `Age` | Numeric | Patient age |
| `Sex` | String | M / F |
| `ChestPainType` | String | ATA, NAP, ASY, TA |
| `RestingBP` | Numeric | Resting blood pressure |
| `Cholesterol` | Numeric | Serum cholesterol |
| `FastingBS` | Numeric | Fasting blood sugar (0/1) |
| `RestingECG` | String | Normal, ST, LVH |
| `MaxHR` | Numeric | Maximum heart rate |
| `ExerciseAngina` | String | Y / N |
| `Oldpeak` | Numeric | ST depression |
| `ST_Slope` | String | Up, Flat, Down |
| `HeartDisease` | Numeric | Target — 0 = no disease, 1 = disease |

---

## Workflow Overview

```
CSV Reader
    ↓
One to Many
    ↓
Number to String
    ↓
Table Partitioner (70/30)
    ↓                    ↓
Normalizer          Normalizer (Apply)
    ↓                    ↓
Naive Bayes     ←→  Naive Bayes Predictor
Learner                  ↓
                    ┌────┴────┐────────┐
                 Scorer   Row Filter  Row Filter
               (Overall)  (Sex=M)     (Sex=F)
                              ↓           ↓
                           Scorer      Scorer
                           (Male)     (Female)
```

---

## Node Configuration

### 1. CSV Reader
- Load the heart disease `.csv` file
- No column configuration needed
- Output: all 12 columns

---

### 2. One to Many
One-hot encodes string categorical columns so Naive Bayes can process them.

| Side | Columns |
|---|---|
| Include | `Sex`, `ChestPainType`, `RestingECG`, `ExerciseAngina`, `ST_Slope` |
| Exclude | Everything else |

---

### 3. Number to String
Converts binary/label numeric columns to nominal so Naive Bayes treats them as categories.

| Side | Columns |
|---|---|
| Include | `FastingBS`, `HeartDisease` |
| Exclude | `RestingBP`, `Cholesterol`, `MaxHR`, `Oldpeak`, `Age` |

> The excluded columns are true continuous values — Naive Bayes needs them numeric to fit a Gaussian distribution.

---

### 4. Table Partitioner
Splits data into training and test sets.

| Setting | Value |
|---|---|
| First partition | 70% (training) |
| Second partition | 30% (test) |
| Method | Relative sampling |
| Seed | Set a fixed value for reproducibility |

- **First output** → Normalizer → Naive Bayes Learner
- **Second output** → Normalizer (Apply) → Naive Bayes Predictor

---

### 5. Normalizer
Scales continuous features so no single column dominates due to its units.

| Setting | Value |
|---|---|
| Method | Z-Score normalization |
| Include | `RestingBP`, `Cholesterol`, `MaxHR`, `Oldpeak`, `Age` |
| Exclude | Everything else |

---

### 6. Naive Bayes Learner

| Setting | Value |
|---|---|
| Class column | `HeartDisease` |
| Threshold | 0.001 (default — Laplace smoothing) |

- All string columns treated as nominal features
- All numeric columns treated as Gaussian continuous features

---

### 7. Normalizer (Apply)
- No manual configuration needed
- Top port: model output from Normalizer
- Bottom port: test partition from Table Partitioner
- Automatically applies the training normalization to test data

---

### 8. Naive Bayes Predictor
- No manual configuration needed
- Top port: model from Naive Bayes Learner
- Bottom port: output from Normalizer (Apply)
- Adds a `Prediction (HeartDisease)` column to the data

---

### 9. Scorer (Overall)
Connects directly from Naive Bayes Predictor output.

| Setting | Value |
|---|---|
| First column | `HeartDisease` (actual) |
| Second column | `Prediction (HeartDisease)` (predicted) |

---

### 10. Row Filter (Male) + Row Filter (Female)
Both connect from the same Naive Bayes Predictor output.

| Node | Filter condition |
|---|---|
| Row Filter 1 | `Sex = "M"` |
| Row Filter 2 | `Sex = "F"` |

---

### 11. Scorer (Male) + Scorer (Female)
Same configuration as the overall Scorer, applied to each filtered group.

| Setting | Value |
|---|---|
| First column | `HeartDisease` |
| Second column | `Prediction (HeartDisease)` |

---

## Results

### Overall Confusion Matrix

| | Predicted 0 | Predicted 1 |
|---|---|---|
| Actual 0 | 102 | 16 |
| Actual 1 | 18 | 140 |

- **Accuracy:** 87.7%
- **Recall (disease):** 88.6%
- **Recall (no disease):** 86.4%

---

### Male vs. Female Breakdown

| Metric | Male | Female |
|---|---|---|
| Total patients | 224 | 52 |
| True Negatives | 66 | 36 |
| False Positives | 15 | 1 |
| False Negatives | 14 | 4 |
| True Positives | 129 | 11 |
| Recall (disease) | 90.2% | 73.3% |
| Recall (no disease) | 81.5% | 97.3% |

---

## Bias Analysis

### Key Finding
The model detects heart disease in males at **90.2% recall** vs only **73.3% for females** — a 17-point gap.

### Misclassified Cases
- **False Negatives (missed disease):** 4 out of 15 diseased female patients were missed (26.7%) vs 14 out of 143 male patients (9.8%)
- Female patients with heart disease are the most at-risk group for misclassification — the model predicts them as healthy when they are not

### Why Differences Exist

**1. Class imbalance by sex**
The dataset contains 224 male vs 52 female test patients. With far fewer female examples, the model's probability estimates for women are less reliable. The prior probability for females skews heavily toward no disease (37 out of 52 females are healthy), so the model defaults to predicting healthy for women.

**2. Feature distributions differ by sex**
Features like `ChestPainType`, `ST_Slope`, and `ExerciseAngina` are distributed differently across sexes. The model's learned distributions are dominated by male presentation patterns since males make up the majority of training data.

**3. Clinical presentation**
Heart disease in women more often presents with atypical symptoms. The features in this dataset are better calibrated to capture male disease presentation, making the model less sensitive to female cases.

**4. Naive Bayes independence assumption**
Naive Bayes assumes all features are independent given the class. This assumption breaks down more severely for female patients where feature correlations may differ from the male-dominated training distribution.

---

## Workflow File

Download and open the KNIME workflow directly:

[Heart Disease Bias Analysis Workflow](https://hub.knime.com/s/ON5LHn89EdafgAAF) <!-- Add your .knwf file link here -->

> To use: download the `.knwf` file, open KNIME, go to **File → Import KNIME Workflow**, and select the file.

---

## Tools Used
- KNIME Analytics Platform
- Naive Bayes Learner / Predictor nodes
- Z-Score Normalizer
- Table Partitioner
- Row Filter + Scorer (for bias evaluation)
