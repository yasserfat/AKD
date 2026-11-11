# Lab 02 – Data Preparation

Solutions for exercise sheet 2 (**Data Preparation**) of the course *Analytics and Knowledge Discovery* (WS 2026, FH Salzburg).

The lab covers how to handle missing data, how to judge imputation quality, how to select features and how to generate new ones.
Each exercise has its own Jupyter notebook. The code is kept simple, and every step is explained in Markdown.

## Contents

| Notebook | Topic | Main result |
|---|---|---|
| [`DP1_missing_data_removal.ipynb`](solutions/DP1_missing_data_removal.ipynb) | Removing rows with missing data | Values are missing only for people over 40, who give higher scores. Deleting rows underestimates the mean (4.53 instead of about 4.99). |
| [`DP2_timeseries_imputation.ipynb`](solutions/DP2_timeseries_imputation.ipynb) | Imputing a PM10 time series | Compares 4 methods: mean, last value, linear interpolation, day before |
| [`DP3_EDA_imputation_results.ipynb`](solutions/DP3_EDA_imputation_results.ipynb) | Evaluating the imputation with EDA | Linear interpolation is best (MAE 1.36), and its error is small compared with the data |
| [`DP4_regression_imputation.ipynb`](solutions/DP4_regression_imputation.ipynb) | Imputation with linear regression | Uses the neighbouring values as predictors. MAE 1.42, against 2.11 for the last-value method |
| [`DP5_feature_selection.ipynb`](solutions/DP5_feature_selection.ipynb) | Feature selection (correlation vs. mutual information) | Correlation works better. The best number of features is k ≈ 70 |
| [`DP6_feature_generation.ipynb`](solutions/DP6_feature_generation.ipynb) | Adding polynomial features to linear regression | R² goes from 0.42 to 1.0 and shows the true formula y = 3 + 2·x1 − x2 + x2² |

📄 **Summary:** [`Lab02_Data_Preparation_Summary.pdf`](solutions/Lab02_Data_Preparation_Summary.pdf) gives the key points of the lab with short explanations.

## Project structure

```
lab02/
├── AKD_UEB02 data preparation.pdf      # exercise sheet
├── dataIsRemovalAllowed.csv            # DP1: gender, age, preference
├── ex22PM10imputation.csv              # DP2–DP4: PM10 time series (with / without gaps)
├── dataFeatureGenerationForLinReg.csv  # DP6: x1, x2, y
├── DP1_missing_data_removal.ipynb      # first (longer) version of DP1
└── solutions/
    ├── DP1 … DP6 notebooks
    ├── Lab02_Data_Preparation_Summary.pdf
    └── imputation_results.csv          # created by DP2
```

## How to run

Requirements: Python 3.10 or newer.

```bash
pip install numpy pandas matplotlib scikit-learn jupyter
jupyter notebook
```

Open the notebooks in the `solutions/` folder. They load the data with relative paths (`../*.csv`), so start Jupyter from the
`lab02` folder or the `solutions` folder and keep the folder layout as it is.

## Key points

- **Missing data:** delete rows only if values are *missing completely at random* (MCAR). Otherwise the result is biased.
- **Imputation:** use the structure of the data. For a time series that means the neighbouring values; it can also mean correlated variables.
- **Evaluation:** you need a suitable graphic, an error measure (MAE, RMSE, relative error) and a reference to compare against.
- **Feature selection:** choose k by test error and fit the selector only on training data, so no test information leaks in.
- **Feature generation:** new features such as x², x·y or log(x) let a linear model fit non-linear relationships.
