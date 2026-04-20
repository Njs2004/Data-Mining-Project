# Week 5 — Baseline Evaluation

## Before Metrics

| Metric | Male | Female |
|---|---:|---:|
| Accuracy | 88.5% | 86.2% |
| Precision | 84.6% | 88.4% |
| Recall | 73.3% | 90.2% |
| F1-Score | 78.6% | 89.3% |
| ROC-AUC | Not calculated (baseline workflow) | Not calculated (baseline workflow) |

## Recall Gap

The recall gap between genders is 16.9 percentage points (90.2% - 73.3%).

## Bias Explanation

The baseline Week 4 pipeline demonstrates strong overall performance across standard evaluation metrics, including accuracy, precision, recall, and F1-score. However, when the results are broken down by gender, a clear disparity emerges. Male recall is approximately 73.3%, while female recall is significantly higher at 90.2%, resulting in a gap of about 16.9%. This indicates that the model is less effective at identifying heart disease in male patients, leading to a higher number of missed diagnoses for that group. In a clinical context, this is a serious issue because false negatives can delay treatment and increase health risks. Therefore, the baseline model exhibits a fairness problem that must be addressed to ensure more balanced and reliable predictions.
