# Strategy 2: Switching to Random Forest

## What We Did and Why

For our second fairness correction, we swapped out the Naive Bayes model and replaced it with a **Random Forest model**, while keeping everything else in the pipeline the same — including the SMOTE balancing from Strategy 1.

The problem with Naive Bayes is that it assumes all features in the data are completely independent of each other. In real life, especially with heart disease data, that is never true. Things like age, cholesterol, chest pain type, and exercise results all influence each other. Naive Bayes ignores those connections, which limits how well it can learn from the data.

Random Forest fixes this by building many decision trees at once and combining their results. This allows the model to pick up on patterns and relationships between features that Naive Bayes would miss entirely. We expected this to help close the performance gap between male and female patients by giving the model more flexibility to detect heart disease across both groups.

---

## What We Changed

We kept the Naive Bayes branch in the pipeline so we could directly compare the two models side by side. A new **Random Forest Learner and Predictor** branch was added in parallel. Both models were trained on the same SMOTE-balanced data and tested on the same normalized test data to keep the comparison fair.

The Random Forest was set up with:
- Split criterion: Information Gain Ratio
- Max tree depth: 10
- Number of trees: 100
- Prediction confidence scores enabled for ROC curve evaluation

We also added the same Row Filters, Scorers, and ROC Curve nodes to the Random Forest branch that already existed for Naive Bayes so we could measure the exact same metrics for both.

---

## Before and After Metrics

### Baseline — Naive Bayes (Before Strategy 2)

**Overall**
| Metric | Value |
|--------|-------|
| Accuracy | 86.59% |
| Correct Classified | 239 |
| Wrong Classified | 37 |
| Cohen's Kappa | 0.724 |

**By Gender**
| Metric | Male | Female |
|--------|------|--------|
| Accuracy | 85.71% | 90.38% |
| Recall | 90.2% | 93.3% |
| ROC-AUC | 0.840 | 0.913 |
| Cohen's Kappa | 0.687 | 0.779 |

**Baseline Recall Gap: 3.1 percentage points (93.3% - 90.2%)**

---

### After Strategy 2 — Random Forest (100 trees)

**Overall**
| Metric | Value |
|--------|-------|
| Accuracy | 89.13% |
| Correct Classified | 246 |
| Wrong Classified | 30 |
| Cohen's Kappa | 0.777 |

**By Gender**
| Metric | Male | Female |
|--------|------|--------|
| Accuracy | 88.84% | 90.38% |
| Recall | 91.6% | 86.7% |
| ROC-AUC | N/A* | 0.793 |
| Cohen's Kappa | 0.758 | 0.770 |

**Post-Strategy 2 Recall Gap: 4.9 percentage points (91.6% - 86.7%)**

*Male ROC-AUC is explained in the Limitations section below.

---

## Full Side-by-Side Comparison

| Metric | Naive Bayes | Random Forest | Change |
|--------|-------------|---------------|--------|
| Overall Accuracy | 86.59% | **89.13%** | +2.54% |
| Overall Correct | 239 | **246** | +7 |
| Male Accuracy | 85.71% | **88.84%** | +3.13% |
| Male Recall | 90.2% | **91.6%** | +1.4% |
| Male AUC | 0.840 | N/A* | - |
| Female Accuracy | 90.38% | 90.38% | 0% |
| Female Recall | **93.3%** | 86.7% | -6.6% |
| Female AUC | **0.913** | 0.793 | -0.12 |
| Recall Gap | 3.1% | 4.9% | +1.8% |

---

## What the Results Show

Switching to Random Forest produced meaningful improvements in overall accuracy and male performance. Overall accuracy went from 86.59% to **89.13%**, and the model correctly classified 7 more patients. Male accuracy improved from 85.71% to 88.84%, and male recall improved slightly from 90.2% to 91.6%.

However, the results also show a tradeoff. Female recall dropped from 93.3% to 86.7%, and female AUC went from 0.913 to 0.793. Female accuracy stayed the same at 90.38%. This means Random Forest made the model stronger overall and for male patients, but the Naive Bayes model was actually better at catching female heart disease cases specifically.

The recall gap also widened slightly from 3.1% to 4.9%, which tells us that while both models perform similarly for females, the gap grew because male recall improved more than female recall did.

It is important to note that both models still perform well overall, and the difference in female recall (93.3% vs 86.7%) represents only a small number of additional missed cases given the small size of the female test subset.

---

## Limitations

**Male ROC-AUC:** The male ROC curve gave an unexpected result even though the male scorer metrics were strong. This happened because after filtering down to just male patients, the test subset was heavily skewed toward positive cases (heart disease = 1). When one class dominates that heavily, the ROC curve becomes unreliable. The scorer metrics — accuracy, recall, and Cohen's Kappa of 0.758 — are used as the main performance indicators for the male group instead.

**Female Recall Tradeoff:** The drop in female recall from 93.3% to 86.7% is worth noting. Random Forest improved overall performance but at a slight cost to female sensitivity. In a clinical setting, female recall is especially important since missed diagnoses (false negatives) can delay treatment. Future work could explore threshold tuning or cost-sensitive learning to recover female recall while keeping the overall accuracy gains from Random Forest.

**Small Female Test Subset:** The female test group only had 52 patients, which means each individual misclassification has a large effect on the percentage metrics. The difference between 93.3% and 86.7% female recall represents just 1-2 additional missed cases, so the practical difference may be smaller than the numbers suggest.

---

## Conclusion

Replacing Naive Bayes with Random Forest (100 trees, max depth 10) improved overall model accuracy from 86.59% to 89.13% and strengthened male performance across all metrics. However, Naive Bayes performed better specifically on female recall (93.3% vs 86.7%) and female AUC (0.913 vs 0.793). The combination of SMOTE from Strategy 1 and Random Forest from Strategy 2 produced a stronger and more accurate model overall, but the results highlight an important tradeoff — optimizing for overall performance does not always mean optimizing for every subgroup equally. Future fairness work on this pipeline should consider threshold tuning as a third strategy to recover female recall while keeping the accuracy gains from Random Forest.

---
