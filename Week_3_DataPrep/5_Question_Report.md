Part 1 — Data Preparation (The "Cleaning")

Imputation/Filtering: Decide on a strategy for the 172 Cholesterol = 0 patients. Will you impute the mean, use a regression to predict them, or flag them?

Transformation: Prepare the final feature matrix. This includes handling any remaining inconsistencies and ensuring the target variable is correctly formatted for classification.

Part 2 — Clinical Investigation (The "Analysis")

Answer these 5 questions using KNIME evidence (charts/stats):

The Zero-Cholesterol Profile: Compare the 172 "Zero" patients against the rest. Are they sicker? What does the "0" likely mean clinically?

The Cholesterol Paradox: Split patients into quartiles. Does higher cholesterol always equal higher disease risk in this specific dataset?

The Outliers: Identify patients under 50 with "perfect" stats (normal BP/Cholesterol/No Diabetes) who still have disease. What other feature explains their diagnosis?

Asymptomatic (ASY) Deep-Dive: 79% of ASY patients have disease. What differentiates the 104 healthy ASY patients from the 392 sick ones?

Feature Engineering: Calculate the HR Reserve Ratio (MaxHR / (220 - Age)). Does this engineered feature correlate more strongly with disease than raw $MaxHR$?
