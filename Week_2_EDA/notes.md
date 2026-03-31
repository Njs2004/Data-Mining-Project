# Week 2 - Data Understanding & EDA
Completed by: Patrick Bonna, Addison Amadi

## Tasks Completed
- Generated histograms for Age, RestingBP, Cholesterol, and MaxHR
- Generated a box plot for continuous variables
- Reviewed dataset statistics in KNIME
- Identified anomalies and patterns
- Encoded categorical variables using Category to Number node
- Removed original string columns using Column Filter node
- Normalized continuous variables using Min-Max normalization
- Produced a clean feature matrix suitable for supervised learning

<a href="https://hub.knime.com/s/UWrFxYl5YW8yI49c" target="_blank">View Workflow on KNIME Hub</a>
- **CSV Reader** — Loads the heart disease dataset (heart.csv) into KNIME as a data table for processing
- **Histogram (x4)** — Visualizes the distribution of Age, RestingBP, Cholesterol, and MaxHR to identify patterns, skewness, and anomalies
- **Box Plot** — Displays the spread and outliers of continuous variables side by side to identify extreme values
- **Category to Number** — Converts categorical string columns (Sex, ChestPainType, RestingECG, ExerciseAngina, ST_Slope) into numeric values for supervised learning
- **Column Filter** — Removes original string columns after encoding to avoid duplicates in the feature matrix
- **Normalizer** — Scales continuous variables (Age, RestingBP, Cholesterol, MaxHR, Oldpeak) to a 0-1 range using Min-Max normalization
- **Interactive Table** — Displays the final cleaned and normalized feature matrix for verification

## Key Findings
### Anomalies
- Cholesterol contains multiple values of 0, which are not clinically realistic and likely represent missing data
- RestingBP contains a value of 0, which is also not realistic

### Outliers
- Cholesterol shows extreme outliers above 500
- RestingBP includes high-value outliers near 200

### Distributions
- Age is relatively normally distributed with moderate spread
- MaxHR shows moderate variability with a fairly clean distribution
- Cholesterol has the greatest variability among all variables

## Notes for Team
- Basic preprocessing was performed to produce a clean feature matrix (encoding and normalization)
- Anomalous 0 values in Cholesterol and RestingBP were identified but not removed — deeper cleaning should be addressed in Week 3
- These findings should guide Week 3 data cleaning and preprocessing
