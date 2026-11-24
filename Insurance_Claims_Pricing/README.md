
# Motor Insurance Claims Frequency, Severity & Pure Premium Prediction

## Project Overview

This project develops an actuarial and machine learning framework for motor insurance pricing using the French Motor Third-Party Liability (freMTPL) dataset.

The objective is to estimate claim frequency, model claim severity, and calculate the expected pure premium for insurance policies.

## Objectives

- Explore and analyze motor insurance data.
- Identify factors associated with claim frequency and severity.
- Develop and compare statistical and machine learning models.
- Estimate expected claim costs.
- Segment the insurance portfolio according to predicted risk.

## Dataset

Two datasets are used:

- `freMTPL2freq`: policy information, exposure, and claim counts.
- `freMTPL2sev`: individual claim amounts.

The matched severity data covers approximately 73.25% of the claims recorded in the frequency dataset.

## Methodology

### 1. Data Preparation and Exploratory Data Analysis

- Data quality checks
- Missing values and duplicate analysis
- Descriptive statistics
- Claim frequency and severity analysis
- Risk factor exploration

### 2. Claim Frequency Modeling

A Poisson Generalized Linear Model (GLM) was developed using an exposure offset.

Evaluation metrics include MAE, RMSE, Poisson deviance, and aggregate claim-count calibration.

### 3. Claim Severity Modeling

Three approaches were compared:

- Gamma GLM with a log link
- Lognormal regression with a smearing adjustment
- Histogram-based Gradient Boosting

### 4. Pure Premium Estimation

Expected pure premium is calculated as:

**Pure Premium = Expected Claim Frequency × Expected Claim Severity**

The portfolio pure premium is calculated using total predicted claim cost divided by total exposure.

### 5. Portfolio Risk Segmentation

Policies were divided into ten risk deciles based on predicted pure premium.

## Key Results

| Metric | Result |
|---|---:|
| Frequency model | Poisson GLM |
| Frequency MAE | 0.0988 |
| Frequency RMSE | 0.2373 |
| Aggregate claim-count underprediction | Approximately 0.64% |
| Gamma GLM MAE (common test sample) | 2,020.18 |
| Gradient Boosting MAE (common test sample) | 1,961.40 |
| Gamma GLM RMSE (common test sample) | 8,433.56 |
| Gradient Boosting RMSE (common test sample) | 8,374.84 |
| Estimated portfolio pure premium | 223.27 per exposure unit |

Gradient Boosting achieved slightly lower MAE and RMSE than Gamma GLM on the common test sample. Gamma GLM remains an interpretable actuarial benchmark.

## Portfolio Risk Segmentation

Average predicted pure premium increased from approximately 91.08 in the lowest-risk decile to 601.62 in the highest-risk decile.

This indicates differentiation between policies according to predicted risk. It is not independent evidence of predictive calibration.

## Getting Started

1. Clone the repository and install the dependencies:

```bash
pip install -r requirements.txt
```

2. Download the two datasets (not included in the repository) and place them in the project root:

- `freMTPL2freq.csv` (OpenML dataset 41214)
- `freMTPL2sev.csv` (OpenML dataset 41215)

Both also come from the `CASdatasets` R package (`freMTPL2freq`, `freMTPL2sev`).

3. Run `Insurance_Claims_Pricing.ipynb` from top to bottom. It regenerates all result tables and the figure listed below. Random seed 42 is used for the train/test splits.

## Limitations

- Severity data covers approximately 73.25% of frequency claims after matching.
- Severity validation is based on matched observed claims and may not represent the complete portfolio.
- Extreme claim amounts remain difficult to predict.
- Calibration across risk deciles is imperfect.
- Pure premium represents expected claim cost only. It excludes expenses, commissions, taxes, reinsurance costs, and profit margins.
- Risk deciles are based on model predictions and do not constitute independent validation.

## Repository Structure

```text
Insurance_Claims_Pricing/
├── Insurance_Claims_Pricing.ipynb
├── Final_Model_Performance.csv
├── Frequency_Calibration.csv
├── Frequency_Model_Comparison.csv
├── Severity_Comparison_Common_Test.csv
├── Severity_Model_Comparison.csv
├── Portfolio_Risk_Segmentation.csv
├── Pure_Premium_By_Risk_Decile.png
├── requirements.txt
├── LICENSE
├── README.md
└── .gitignore

Note: `Portfolio_Pure_Premium_Predictions.csv` (about 20 MB) and the raw datasets are generated or downloaded locally and are excluded from Git.

## License

Released under the MIT License. See `LICENSE`.
