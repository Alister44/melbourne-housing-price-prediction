# Melbourne Housing Price Prediction

End-to-end regression project: data cleaning, exploratory data analysis,
feature engineering and modeling to predict property sale prices in Melbourne.

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Alister44/melbourne-housing-price-prediction/blob/main/notebooks/housing_price_prediction.ipynb)

## Business Problem
A real estate agency wants to estimate a fair listing price for a property
based on its characteristics and location. This project builds a model that
predicts the sale price and shows which factors drive it most.

**Key question:** Which property features most influence the sale price,
and how accurately can we predict it?

## Dataset
- Source: [Melbourne Housing Snapshot (Kaggle)](https://www.kaggle.com/datasets/dansbecker/melbourne-housing-snapshot)
- File: `data/melb_data_raw.csv` (13,580 rows, 21 columns)
- Full column descriptions: [Data Dictionary](docs/data_dictionary.md)
- Period: 2016-01-28 to 2017-09-23
- Target: `Price` (sale price, AUD). The models are trained on `log10(Price)`
  because the price distribution is strongly right-skewed.

## Tools
Python (pandas, NumPy, matplotlib, scikit-learn), Google Colab, GitHub

## Project Steps
1. **Data cleaning** – removed sales with ambiguous outcomes (methods PI and VB,
   20% of rows), parsed dates (day-first), replaced zero and unrealistic building
   areas and an impossible build year with missing values, checked duplicates
2. **EDA** – price distribution and log transformation, price by property type
   and region, correlation of numerical features with price
3. **Feature engineering** – property age at sale, sale year and month,
   missing-value flags, frequency encoding for suburbs
4. **Modeling** – preprocessing pipeline (imputation + one-hot encoding),
   80/20 train/test split, baseline, linear regression and random forest
5. **Evaluation** – MAE, RMSE and R² on the test set (in AUD),
   feature importance, predicted vs. actual prices

## Results
| Model | MAE (AUD) | RMSE (AUD) | R² |
|---|---|---|---|
| Baseline (mean) | 405,065 | 596,901 | −0.043 |
| Linear Regression | 209,680 | 345,282 | 0.651 |
| **Random Forest** | **150,389** | **264,643** | **0.795** |

Random Forest is the best model: its predictions deviate from the actual price
by about AUD 150,000 on average, and it explains around 80% of the variance
in prices.

## Key Findings
- **Property type** is the most important single factor: units are clearly
  cheaper than houses and townhouses (median ≈ AUD 575,000 vs. ≈ 1.05 million
  for houses).
- **Location** is the strongest factor overall: region, distance to the city
  center and coordinates together account for more than half of the feature
  importance. Southern Metropolitan is the most expensive region.
- **Land size** has almost no linear correlation with price, but it is the
  fifth most important feature in the random forest, which points to a
  non-linear relationship that depends on property type.
- The model works well for properties up to about AUD 2 million, but
  **underestimates very expensive properties**, which are rare in the data.

## Repository Structure
```
melbourne-housing-price-prediction/
├── data/
│   └── melb_data_raw.csv
├── docs/
│   └── data_dictionary.md
├── notebooks/
│   └── housing_price_prediction.ipynb
└── README.md
```

## How to Run
1. Open the notebook in Google Colab using the badge above
2. Run all cells (the data is loaded directly from this repository)

## Next Steps
- Tune the random forest hyperparameters with `GridSearchCV`
- Try gradient boosting (XGBoost or LightGBM)
- Improve predictions for expensive properties, for example with more data
  or a separate model for the high-price segment
