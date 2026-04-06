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

Despite appearing healthy based on traditional risk factors, several key patterns emerged after isolating patients data who appear to have "perfect" health metrics. The most notable was ChestPainType, where 7 out of the 8 patients were classified as ASY (asymptomatic), indicating they did not exhibit typical chest pain symptoms. Only one patient had NAP (non-anginal pain). This suggests that many of these individuals have silent or hidden heart disease, making it harder to detect through standard symptoms.

In addition, other features help explain their diagnosis. Most patients showed Flat ST_Slope values, which are commonly associated with abnormal heart function and reduced blood flow. There were also elevated Oldpeak values in several cases, indicating exercise-induced stress on the heart that would not be visible through resting measurements. Another notable pattern is that all patients in this filtered group are male, which aligns with known trends of higher early cardiovascular risk in men.

Overall, these findings demonstrate that even when standard indicators such as blood pressure, cholesterol, and diabetes appear normal, heart disease can still be present due to underlying electrical abnormalities (ST_Slope), stress responses (Oldpeak), and lack of visible symptoms (ASY).

<img width="1356" height="306" alt="Screenshot 2026-04-06 064502" src="https://github.com/user-attachments/assets/4acdfd2c-d349-4caf-838f-3be0b28876ac" />

Workflow link: https://hub.knime.com/s/ioQksOKr8JcBcCpd 

---

### 4️⃣ Asymptomatic (ASY) Deep-Dive
- Given:
  - **79% of ASY patients have disease**
- Compare:
  - 104 healthy ASY patients
  - 392 diseased ASY patients

- 💡 Key Question:
  - What differentiates the 104 healthy ASY patients from the 392 sick ones?

---

### 5️⃣ Feature Engineering 
- Create a new feature:

```math
HR\ Reserve\ Ratio = \frac{MaxHR}{220 - Age}
```
- 💡 Key Question:
  - Does this engineered feature correlate more strongly with disease than raw $MaxHR$?
<img width="450" height="450" alt="Screenshot 2026-04-05 at 11 13 19 PM" src="https://github.com/user-attachments/assets/b958ed50-9f8b-4cdf-979e-45e4587ecaee" />
Workflow link: https://hub.knime.com/s/UWrFxYl5YW8yI49c
