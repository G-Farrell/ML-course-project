# Diabetes Prediction & Fairness Analysis

**CS 1675 - Introduction to Machine Learning - Course Project**

This project builds diabetes prediction models using CDC BRFSS 2015 survey data and analyzes how removing different feature groups (demographics, lifestyle, healthcare access) affects both prediction accuracy and fairness across demographic subgroups.

## How to Run

Run notebooks in order:
1. `data_preparation.ipynb` — Load raw data, add Race variable
2. `eda_and_preprocessing.ipynb` — EDA, preprocessing, train/val/test splits
3. `baseline_models_and_fairness.ipynb` — Train and evaluate baseline models
4. `leave_out_*.ipynb` — Leave-out experiments
5. `results_aggregation.ipynb` — Cross-experiment visualizations

## Data Sources

The dataset was constructed from the raw BRFSS 2015 survey data by Alex Teboul. We extended his pipeline to include race/ethnicity (`_RACE`) sourced directly from the CDC's raw XPT file.

- [Teboul data preparation notebook](https://www.kaggle.com/code/alexteboul/diabetes-health-indicators-dataset-notebook)
- [CDC BRFSS 2015 data](https://www.cdc.gov/brfss/annual_data/annual_2015.html)
- [CDC BRFSS 2015 codebook](https://www.cdc.gov/brfss/annual_data/2015/pdf/codebook15_llcp.pdf)

## Authors

Gary Farrell, Ava Luu, Lalit More, Nithya Jayakaran
