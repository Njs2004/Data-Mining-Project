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

No, while Q4 (highest cholesterol, 276–603 mg/dL) has the highest disease rate at 57.3%, Q1 patients with the lowest cholesterol still have a 41.7% disease rate, meaning cholesterol alone is a weak and inconsistent predictor in this dataset. This is likely because many high-risk patients are already on statins suppressing their cholesterol levels, and stronger predictors like Oldpeak, ST_Slope, and ASY chest pain type are doing most of the predictive work.


<img width="565" height="510" alt="image" src="https://github.com/user-attachments/assets/259acb8b-2337-44dd-b88a-0377c897a2fa" />


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

**Analysis of the 392 Diseased vs. 104 Healthy ASY Patients:**

**Key Question:** What differentiates the 104 healthy ASY patients from the 392 sick ones?

**Analysis & KNIME Evidence:**

Even though both groups are categorized as **Asymptomatic** (meaning they report no chest pain), the data shows they are clinically very different. Based on the **Heatmap** and **Normalizer** nodes in my workflow, the diseased group is differentiated by:

* **Fasting Blood Sugar (FastingBS):** My **Heatmap** shows that sick ASY patients are much more likely to have a value of **1** (high blood sugar). In contrast, the 104 healthy patients almost all have a **0**. This suggests that high blood sugar is a major "hidden" risk factor for this group.
  
* **ST_Slope & Oldpeak:** After processing the data through the **Category to Number** node, there is a clear trend: the 392 sick patients typically have a **Flat** or **Downsloping ST_Slope**, while the healthy patients have an **Upsloping** one.
* **Exercise Angina:** Many of the sick patients show a **"Y"** for **ExerciseAngina**. This means that even if they feel fine at rest, their bodies show signs of distress during physical activity.

**Conclusion:**
The main difference is that the 104 healthy patients have normal "hidden" metrics, while the 392 sick patients have
normal test results (High FastingBS and Flat ST_Slopes) that only show up through clinical data, not through how the patient feels.
<img width="1017" height="726" alt="Screenshot 2026-04-06 at 10 44 48 PM" src="https://github.com/user-attachments/assets/4bd89bd2-e1db-4992-9004-3a7179f8d785" />


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
