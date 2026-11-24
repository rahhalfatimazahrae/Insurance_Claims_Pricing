# Motor Insurance Claims Frequency, Severity & Pure Premium Prediction

**Actuarial modelling and machine learning on the French Motor Third-Party Liability (freMTPL) dataset.**

This project estimates motor insurance claim frequency and severity, combines both estimates into a pure premium, and segments policies by predicted risk.

## Project objectives

- Explore policy-level motor insurance data and claim amounts.
- Identify factors associated with claim frequency and claim severity.
- Compare statistical actuarial models with machine learning approaches.
- Estimate expected claim cost (pure premium).
- Segment the portfolio into ten predicted-risk deciles.

## Dataset

The project uses two French MTPL datasets:

- [freMTPL2freq — OpenML dataset 41214](https://www.openml.org/d/41214): policy characteristics, exposure and claim counts.
- [freMTPL2sev — OpenML dataset 41215](https://www.openml.org/d/41215): individual claim amounts.

Download the datasets as CSV files and place them in the repository root using these exact filenames: `freMTPL2freq.csv` and `freMTPL2sev.csv`. Raw datasets are not committed to this repository.

The matched severity data covers approximately 73.25% of the claims recorded in the frequency dataset.

## Methodology

1. **Data preparation and EDA:** data-quality checks, missing values, duplicates, descriptive statistics and risk-factor exploration.
2. **Claim frequency:** Poisson Generalized Linear Model (GLM) with an exposure offset. Evaluation includes MAE, RMSE, Poisson deviance and aggregate claim-count calibration.
3. **Claim severity:** Gamma GLM with log link, Lognormal regression with smearing adjustment, and Histogram-based Gradient Boosting.
4. **Pure premium:** expected claim frequency multiplied by expected claim severity. Portfolio pure premium is total predicted claim cost divided by total exposure.
5. **Portfolio segmentation:** policies grouped into ten deciles based on predicted pure premium.

## Key results

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

On the common test sample, Gradient Boosting achieved slightly lower MAE and RMSE than Gamma GLM. Gamma GLM remains an interpretable actuarial benchmark.

Average predicted pure premium ranges from approximately 91.08 in the lowest-risk decile to 601.62 in the highest-risk decile. This demonstrates model-based risk differentiation, not independent evidence of calibration.

## Repository contents

- [Jupyter Notebook](Insurance_Claims_Pricing.ipynb) — end-to-end analysis and modelling workflow.
- [Final model performance](Insurance_Claims_Pricing/Final_Model_Performance.csv)
- [Frequency model comparison](Insurance_Claims_Pricing/Frequency_Model_Comparison.csv)
- [Frequency calibration](Insurance_Claims_Pricing/Frequency_Calibration.csv)
- [Severity model comparison](Insurance_Claims_Pricing/Severity_Model_Comparison.csv)
- [Severity comparison on common test sample](Insurance_Claims_Pricing/Severity_Comparison_Common_Test.csv)
- [Portfolio risk segmentation](Insurance_Claims_Pricing/Portfolio_Risk_Segmentation.csv)
- [Pure premium by risk decile](Insurance_Claims_Pricing/Pure_Premium_By_Risk_Decile.png)

## Getting started

Clone the repository and install the dependencies:

```bash
git clone https://github.com/rahhalfatimazahrae/Insurance_Claims_Pricing.git
cd Insurance_Claims_Pricing
pip install -r requirements.txt
```

Download the two CSV datasets linked above and place them in the repository root. Then launch Jupyter:

```bash
jupyter notebook
```

Open `Insurance_Claims_Pricing.ipynb` and run all cells from top to bottom. The notebook reads the two source CSV files from the repository root and writes its exported tables and figure into `Insurance_Claims_Pricing/`. A fixed random seed (42) is used for the train/test splits.

## Limitations

- The matched severity sample covers approximately 73.25% of frequency claims and may not represent the complete portfolio.
- Extreme claim amounts remain difficult to predict.
- Calibration across risk deciles is imperfect.
- Pure premium represents expected claim cost only; it excludes expenses, commissions, taxes, reinsurance costs and profit margins.
- Risk deciles are derived from model predictions and do not constitute independent validation.

## License

Released under the [MIT License](LICENSE).
