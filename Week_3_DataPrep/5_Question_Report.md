## Clinical Investigation 

Use **KNIME visualizations and statistics** (charts, histograms, box plots, etc.) to support your answers.

---

### 1️⃣ The Zero-Cholesterol Profile
- Compare the **172 patients with Cholesterol = 0** vs others
- Investigate:
  - Disease prevalence
  - Other health indicators
- 💡 Key Question:
  - What does a value of **0 cholesterol** likely represent clinically?

---

### 2️⃣ The Cholesterol Paradox
- Split patients into **quartiles based on cholesterol levels**
- Analyze disease rates across quartiles

- 💡 Key Question:
  - Does higher cholesterol always correspond to higher disease risk in this dataset?

---

### 3️⃣ The Outliers
- Identify patients:
  - Under age 50
  - Normal BP
  - Normal cholesterol
  - No diabetes
  - Yet still diagnosed with heart disease

- 💡 Key Question:
  - What **other feature(s)** explain their diagnosis?

---

### 4️⃣ Asymptomatic (ASY) Deep-Dive
- Given:
  - **79% of ASY patients have disease**
- Compare:
  - 104 healthy ASY patients
  - 392 diseased ASY patients

- 💡 Key Question:
  - What differentiates healthy vs diseased individuals in this group?

---

### 5️⃣ Feature Engineering (Addison)
- Create a new feature:

```math
HR\ Reserve\ Ratio = \frac{MaxHR}{220 - Age}
```
<img width="450" height="450" alt="Screenshot 2026-04-05 at 11 13 19 PM" src="https://github.com/user-attachments/assets/b958ed50-9f8b-4cdf-979e-45e4587ecaee" />
Workflow link: https://hub.knime.com/s/UWrFxYl5YW8yI49c
