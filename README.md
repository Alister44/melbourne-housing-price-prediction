# Melbourne Housing Price Prediction

End-to-end regression project: cleaning, EDA, feature engineering and
modeling to predict property sale prices in Melbourne.

## Business Problem
A real estate agency wants to estimate a fair listing price for a property
based on its characteristics and location. This project builds a model that
predicts the sale price and shows which factors drive it most.

**Key question:** Which property features most influence the sale price,
and how accurately can we predict it?

## Dataset
- Source: [Melbourne Housing Snapshot (Kaggle)](https://www.kaggle.com/datasets/dansbecker/melbourne-housing-snapshot)
- Full column descriptions: [Data Dictionary](docs/data_dictionary.md)
- File: `data/melb_data.csv`
- Target: `Price`
- Features: rooms, property type, distance to CBD, land size, year built,
  suburb/region, sale method and date
  
  - **Period:** 2016-01-28 to 2017-09-23 (based on `Date`)

## Tools
Python (pandas, NumPy, matplotlib, scikit-learn), Google Colab, GitHub

## Project Steps
1. **Data cleaning** – removed index column, parsed dates (day-first),
   replaced zero areas and impossible build years with missing values,
   checked duplicates
2. **EDA** – price distribution, price by property type, correlations
3. **Feature engineering** – property age at sale, sale year/month,
   missing-value flags, frequency encoding for suburbs
4. **Modeling** – preprocessing pipeline (imputation + one-hot encoding),
   train/test split, baseline, linear regression, random forest
5. **Evaluation** – MAE, RMSE, R²

## Results
| Model | MAE | RMSE | R² |
|---|---|---|---|
| Baseline (mean) | … | … | … |
| Linear Regression | … | … | … |
| Random Forest | … | … | … |

## Key Findings
- …
- …
- …

## How to Run
1. Open `notebooks/housing_price_prediction.ipynb` in Google Colab
2. Run all cells (data is loaded from the repository)

## Next Steps
- …
