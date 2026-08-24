# Diabetes Risk Explorer — Data Science Portfolio Project

## Project question
**Can anonymous lifestyle, self-reported health, and demographic survey features classify whether a U.S. respondent reports prediabetes or diabetes?**

> **Educational project only.** This work is not a diagnostic system, cannot provide medical advice, and must never be used to make decisions about individual people.

## What I built
- Reproducible Python workflow using the official UCI **CDC Diabetes Health Indicators** dataset.
- Data-quality check, exploratory data analysis, and charts.
- Two classification models: logistic regression and histogram gradient boosting.
- Evaluation that goes beyond accuracy: precision, recall, F1, ROC-AUC, and average precision.
- Permutation-importance analysis and a written ethics/limitations review.

## Dataset
- **253,680** anonymous survey rows and **21** predictors.
- Target: `Diabetes_binary` — 0 for no reported diabetes and 1 for prediabetes or diabetes.
- Dataset source: UCI Machine Learning Repository, *CDC Diabetes Health Indicators* (2017), DOI [10.24432/C53919](https://doi.org/10.24432/C53919). The underlying survey is CDC BRFSS.
- CDC BRFSS data documentation: https://www.cdc.gov/brfss/annual_data/annual_data.htm

## Method
1. Stratified 80/20 train/test split (`random_state=42`), so the 14% positive-class rate remains consistent in both sets.
2. Logistic regression with standardized features and balanced class weights as an interpretable baseline.
3. Histogram gradient boosting as a nonlinear comparison model.
4. Metrics measured on **50,736 held-out test rows**. The 0.50 decision threshold is used for accuracy/precision/recall/F1; ROC-AUC and average precision are threshold-independent.

## Results
| Model | Accuracy | Precision | Recall | F1 | ROC-AUC | Average precision |
|---|---:|---:|---:|---:|---:|---:|
| Logistic regression | 73.2% | 31.1% | 76.1% | 44.1% | 0.820 | 0.393 |
| Histogram gradient boosting | 86.5% | 55.8% | 15.8% | 24.6% | 0.827 | 0.424 |

### Interpretation
The gradient-boosting model has the strongest ranking metrics (ROC-AUC **0.827**, average precision **0.424**), but its recall is low at the default 0.50 threshold. That is the key lesson of the project: **accuracy alone is misleading with imbalanced data.** The logistic baseline catches more reported cases but produces many false positives. A real deployment would choose a threshold using domain, ethics, and error-cost considerations—not a generic default—and this educational project must not be deployed.

### Exploratory finding
Average BMI was **27.81** in the no-reported-diabetes group and **31.94** in the prediabetes/diabetes group. This is an association in this survey, **not evidence that BMI causes diabetes**.

### Most influential predictors (permutation importance)
The top signals for the gradient-boosting model were: GenHlth, BMI, Age, HighBP, HighChol. Permutation importance measures how much model ranking performance falls when a feature is shuffled; it does **not** establish causality.

## Repository structure
```text
notebooks/
  01_setup_and_data_understanding.ipynb
  02_modelling_and_evaluation.ipynb
outputs/
  model_metrics.csv
  model_metrics.png
  evaluation_curves.png
  feature_importance.png
  permutation_importance.csv
  results.json
portfolio_report.html
docs/ethics-and-limitations.md
```

## Running it
1. Open either notebook in Google Colab.
2. Run cells from top to bottom.
3. The notebook installs its open-source dependencies and downloads the dataset from UCI.

## What I would improve next
- Choose decision thresholds on a validation set and report the trade-off transparently.
- Add calibration analysis.
- Audit aggregate error rates across relevant groups, only with careful interpretation.
- Publish the reproducible project to GitHub with a short video walkthrough.
