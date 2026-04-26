# Project Context

## What this project is
CS 1675 (Intro to Machine Learning) course project. We are building diabetes prediction models using the CDC BRFSS 2015 survey dataset and analyzing fairness across demographic subgroups.

**Core question:** How does training on different subsets of features affect prediction accuracy overall and across demographic groups?

---

## File structure

```
data_preparation.ipynb            # Loads brfss_2015_raw.xpt, adds Race, saves CSV
eda_and_preprocessing.ipynb       # EDA, standardization, train/val/test splits
baseline_models_and_fairness.ipynb # Baseline model training, tuning, evaluation, fairness analysis
leave_out_*.ipynb                 # Leave-out experiment notebooks
results_aggregation.ipynb         # Cross-experiment visualizations and comparisons
brfss_2015_raw.xpt                # Raw CDC BRFSS 2015 data (~1.1 GB, gitignored)

data/
  column_key.md                                            # Data dictionary
  diabetes_binary_health_indicators_BRFSS2015_with_race.csv  # Main dataset (251,467 rows, 22 features + target)
  fig_*.png                                                # EDA figures
  processed/
    train.csv / val.csv / test.csv                         # 70/15/15 split, standardized continuous features

results/
  baseline/ no_lifestyle/ no_demographics/ no_healthcare/ health_only/
    performance.csv                                        # Model performance metrics
    fairness_*.csv                                         # Fairness metrics by demographic
    importance_*.csv                                       # Feature importance scores
```

---

## Dataset

**Primary file:** `data/diabetes_binary_health_indicators_BRFSS2015_with_race.csv`
- 251,467 rows, 23 columns (22 features + `Diabetes_binary` target)
- 251,467 vs 253,680: 2,213 rows dropped where `_RACE = 9` (don't know/refused)
- Target: `Diabetes_binary` — 0 = no diabetes, 1 = prediabetes or diabetes (~15.7% positive rate — class imbalance)

**Feature groups** (from proposal):
- Health Conditions (10): HighBP, HighChol, CholCheck, BMI, Stroke, HeartDiseaseorAttack, GenHlth, MentHlth, PhysHlth, DiffWalk
- Lifestyle Behaviors (5): Smoker, PhysActivity, Fruits, Veggies, HvyAlcoholConsump
- Demographics (5): Sex, Race, Age, Education, Income
- Healthcare Access (2): AnyHealthcare, NoDocbcCost

See `data/column_key.md` for full encoding of every column.

---

## Key decisions made

**Race variable:** Used `_RACE` (8-category computed variable) rather than `_RACEG21` (binary White/non-White). Groups 5 (Native HI/Pacific Islander, ~1,400 rows) and 6 (Other, ~1,900 rows) are small — flag results for these in fairness analysis.

**Race encoding:** One-hot encoded for LR and NN (7 dummy columns, drop first). Integer 1-8 for Random Forest (trees handle nominal fine).

**Class imbalance:** ~84% negative / 16% positive (5.4:1 ratio). Handled with `class_weight='balanced'` for LR and RF, `pos_weight` in BCEWithLogitsLoss for NN. This trades accuracy for recall — appropriate for medical screening.

**Splits:** Stratified 70/15/15 train/val/test, seed=42.

**Preprocessing:** Continuous features (BMI, MentHlth, PhysHlth) standardized with StandardScaler. Ordinal and binary features left as integers.

**Primary metric:** F1 score (not accuracy) because of class imbalance. Accuracy is misleading — a model predicting all negatives gets 84% accuracy but 0% recall.

---

## Baseline model results (full feature set)

| Model | Best Hyperparams | Test F1 | Test ROC-AUC |
|-------|------------------|---------|--------------|
| Logistic Regression | C=100 | 0.473 | 0.821 |
| Random Forest | n_estimators=200, max_depth=15 | 0.483 | 0.817 |
| Neural Network | lr=0.001, 20 epochs | 0.472 | 0.826 |

**Feature importance (RF):** GenHlth, BMI, HighBP, Age, HighChol are top 5.

**Fairness findings:**
- Race recall disparity: 0.33 (Asian 43% vs Native HI/Pacific Islander 76%)
- Sex recall gap: <1% (fair)
- Age recall disparity: 0.77 (young people rarely predicted positive due to low base rate)

---

## Leave-out experiments

Each model trained on 5 feature sets:
1. Full feature set (baseline)
2. Remove Demographics (Sex, Race, Age, Education, Income)
3. Remove Healthcare Access (AnyHealthcare, NoDocbcCost)
4. Remove Lifestyle (Smoker, PhysActivity, Fruits, Veggies, HvyAlcoholConsump)
5. Remove all 3 groups above (keep only Health Conditions)

Health Conditions stays in all experiments — strongest predictive features. Goal: test if accuracy and fairness hold when removing features that correlate with demographics.

---

## Fairness metrics

We report three disparity metrics for each demographic group (Race, Sex, Age), grounded in fairness literature:

| Metric | Definition | Why it matters |
|--------|------------|----------------|
| **Recall disparity** (max - min) | Gap in true positive rates across groups | Equal opportunity — all groups should have equal chance of being correctly identified if positive |
| **FPR disparity** (max - min) | Gap in false positive rates across groups | Equalized odds — groups shouldn't differ in false alarm rates |
| **Precision disparity** (max - min) | Gap in positive predictive value across groups | Predictive parity — positive predictions should be equally reliable across groups |

**Key references:**
- **Hardt, Price, & Srebro (2016)** — "Equality of Opportunity in Supervised Learning" — defines equalized odds (equal recall AND equal FPR)
- **Chouldechova (2017)** — "Fair Prediction with Disparate Impact" — impossibility theorem: with different base rates, can't satisfy equalized odds AND predictive parity simultaneously

Fairness is evaluated for all 3 models (LR, RF, NN) on the test set.

---

## Notes / watch-outs
- The `data/processed/` splits can be regenerated by re-running `eda_and_preprocessing.ipynb`
- `brfss_2015_raw.xpt` is gitignored (too large). If missing, download from https://www.cdc.gov/brfss/annual_data/annual_2015.html (SAS Transport Format)
- `baseline_models_and_fairness.ipynb` is structured to make leave-out experiments easy — mostly just change which features go into X
