# Project Plan: Melbourne Housing Price Prediction

## Objective
I build an end-to-end regression project: I clean a real housing dataset,
explore it, engineer features and train a model that predicts the sale price
of a property. The goal is to show the full analyst workflow:
cleaning → EDA → feature engineering → modeling → conclusions.

**Problem type:** regression (the target is a number: `Price`).

## Dataset
- **Name:** Melbourne Housing Snapshot
- **Source:** [Kaggle](https://www.kaggle.com/datasets/dansbecker/melbourne-housing-snapshot)
- **File:** `melb_data.csv`
- **Target:** `Price` (sale price)
- **Main columns:** number of rooms (`Rooms`), property type (`Type`),
  distance to the city center (`Distance`), land size (`Landsize`),
  year built (`YearBuilt`), location (`Suburb`, `Regionname`, `CouncilArea`),
  sale method and date (`Method`, `Date`)

## Tools
Python (pandas, NumPy, matplotlib, scikit-learn), Google Colab, GitHub

## Repository Setup
I keep the project in a public GitHub repository with this structure:

```
melbourne-housing-price-prediction/
├── data/
│   └── melb_data.csv
├── notebooks/
│   └── housing_price_prediction.ipynb
└── README.md
```

I load the data in Colab directly from the raw GitHub link, so the notebook
runs without manual file uploads. When I finish, I save the notebook
back to the repository.

## My Steps

### Step 1. Load the data
I read the CSV from the raw GitHub URL into a DataFrame and check the
shape of the table (rows and columns) to make sure the file loaded correctly.

### Step 2. First look at the data
Before changing anything, I check:
- table size, first rows and column types
- descriptive statistics of numeric columns
- the share of missing values in each column

I look for: a technical index column, columns with many missing values,
wrongly detected types (for example, dates read as text) and suspicious
minimum or maximum values.

### Step 3. Data cleaning
I work on a copy of the original data and fix the following:
- **Index column:** I drop it if it only contains row numbers.
- **Date:** I convert text to a real date type. The format is day/month/year,
  so I set `dayfirst=True`.
- **Fake zeros:** a property cannot have an area of 0, so I replace such
  zeros with missing values.
- **Impossible values:** I treat very old build years (for example, 1196)
  as entry errors and replace them with missing values.
- **Duplicates:** I check for duplicate rows and remove them.
- **Missing values:** I drop columns with more than half of values missing.
  I fill the rest later inside the pipeline, so no information from the
  test set leaks into training.

### Step 4. Exploratory data analysis (EDA)
I explore the data with simple charts and write down what I see:
- distribution of `Price` (histogram, and log scale for comparison),
  because prices are usually right-skewed
- price by property type (boxplot)
- correlation of numeric features with `Price`
- pairs of almost identical features (for example, `Rooms` and `Bedroom2`)

I summarize 2–3 key findings in words: which property type is more
expensive, how distance affects price and which features overlap.

### Step 5. Feature engineering
I create new features that give the model more useful signal:
- **Property age at sale** = sale year − year built
  (negative values are set to missing)
- **Sale year and month**, because the market changes over time
- **Missing-value flags** for `BuildingArea` and `YearBuilt`,
  because a missing value can carry information
- **Frequency encoding for `Suburb`**, because one-hot encoding would
  create too many columns

I do not create features from `Price` (for example, "price per room").
This would be target leakage: the price is not known at prediction time.

I also remove columns that add little value: `Bedroom2` (duplicates `Rooms`),
and text columns I do not encode, such as `Address` and `SellerG`.

### Step 6. Preparing for modeling
- I define the target `y` (`Price`) and features `X` (everything else).
- I split columns into numeric and categorical.
- **Missing values:** median for numeric columns, most frequent value
  for categorical columns.
- **Encoding:** one-hot encoding for categorical columns.
- I put all preprocessing steps into one scikit-learn `Pipeline`, so the
  same transformations are learned on the training data and applied to the
  test data.
- I split the data into train and test sets (80/20, `random_state=42`).

### Step 7. Models and evaluation
I compare three models on the test set:
1. **Baseline:** always predicts the mean price (the minimum level to beat)
2. **Linear Regression:** simple and easy to interpret
3. **Random Forest:** more complex, usually more accurate on this kind of data

**Metrics:**
- MAE: average error in dollars
- RMSE: like MAE, but penalizes large errors more
- R²: share of price variation explained by the model

I look at feature importance of the Random Forest to see which factors
influence price the most. Finally, I predict the price of one test example
and compare it with the real price.

### Step 8. Conclusions and README
I summarize the project in the README: the cleaning steps, what the analysis
showed, which model performed best, its error and 2–3 ideas for improvement.

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

## Next Steps
- …
