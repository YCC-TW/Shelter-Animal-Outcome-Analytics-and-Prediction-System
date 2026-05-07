# Shelter Animal Outcome Analytics and Prediction System

## Overview

This project analyzes public animal shelter intake and outcome records to support data-driven shelter management. The goal is to identify patterns in animal outcomes, predict whether an animal is likely to have a live outcome, estimate shelter stay duration, classify specific outcome types, identify high-risk cases, and forecast future monthly live release rates.

Animal shelters often make operational decisions with limited space, staffing, and resources. By using machine learning and time-series forecasting, this project explores how historical shelter data can help estimate outcome probabilities, shelter length of stay, and future live release trends.

## Dataset

The project uses the **OAS Live Release Rate** dataset from Data.gov.

Dataset source:

```text
https://catalog.data.gov/dataset/oas-live-release-rate
```

The dataset contains shelter records with features such as:

* Animal identification number
* Impound type
* Intake subcategory
* Impound date and time
* Departure date and time
* Animal intake condition
* Outcome type
* Outcome subtype
* Animal type

After cleaning and feature engineering, the project used over **41,000 shelter records**.

## Project Objectives

This project focuses on the following machine learning and analytics tasks:

1. **Binary Classification**
   Predict whether an animal will have a live outcome or non-live outcome.

2. **Regression**
   Predict the expected number of days an animal will stay in the shelter.

3. **Multi-Class Classification**
   Predict the specific outcome type, such as adoption, transfer, euthanasia, or return to owner.

4. **Risk Scoring**
   Identify animals with higher risk of non-live outcomes.

5. **Time-Series Forecasting**
   Forecast future monthly live release rates using historical trends.

6. **Exploratory Data Analysis**
   Visualize intake patterns, outcome distributions, shelter stay length, and seasonal trends.

## Methods

### Data Cleaning and Feature Engineering

The dataset was cleaned by handling missing values, removing duplicates, and converting date/time columns into usable features.

A key engineered feature was **Length of Stay**, calculated from:

```text
Departure date and time - Impound date and time
```

Additional time-based features included:

* Month
* Year
* Day of week

These features were used to capture seasonal and temporal patterns in shelter outcomes.

### Outlier Handling

For the regression task, the project used the **Interquartile Range (IQR)** method to reduce the effect of extreme shelter-stay outliers.

The IQR process kept records with length of stay less than or equal to the calculated upper bound. After filtering:

```text
Rows before outlier removal: 41,037
Rows after outlier removal: 37,226
New average length of stay: 11.06 days
```

### Binary Classification

The binary classification model predicts whether an animal has a live outcome.

The project first tested a simpler model using only time-based features:

```text
Features: Month, Year, DayOfWeek
```

Then the model was improved by adding animal and intake-related features:

```text
Month, Year, DayOfWeek,
Animal type,
Animal intake condition,
Impound type,
Intake subcategory
```

Because the dataset was highly imbalanced, the model used class balancing to improve prediction of minority-class outcomes.

### Regression

The regression model predicts the number of days an animal may stay in the shelter.

The model used a Random Forest Regressor with preprocessing steps for categorical and numerical features. A log transformation was applied to the target variable to reduce skewness.

Result after outlier filtering:

```text
Mean Absolute Error (MAE): 7.64 days
```

This means the model’s average prediction error was approximately 7.64 days.

### Multi-Class Classification

The multi-class model predicts specific outcome categories:

```text
ADOPTION
TRANSFER
EUTH
RTO
```

The model used a Random Forest Classifier with preprocessing for categorical and numerical variables.

Result:

```text
Accuracy: 68.36%
```

### Risk Scoring

The project also created a risk-scoring workflow to identify animals with higher probability of non-live outcomes, especially cases related to euthanasia or death.

This can help shelters prioritize attention toward higher-risk cases.

### Time-Series Forecasting

The project used a SARIMAX model to forecast future monthly live release rates.

Model type:

```text
SARIMAX
Seasonal AutoRegressive Integrated Moving Average with External Variables
```

The model generated a 12-month forecast of live release rates using historical monthly trends.

## Technologies Used

* Python
* pandas
* NumPy
* scikit-learn
* statsmodels
* Matplotlib
* Seaborn
* Jupyter Notebook / Google Colab

## Repository Structure

Recommended project structure:

```text
shelter-animal-outcome-prediction/
│
├── README.md
├── requirements.txt
├── .gitignore
│
├── notebooks/
│   └── shelter_animal_outcome_analysis.ipynb
│
├── reports/
│   └── project_slides.pdf
│
├── images/
│   ├── length_distribution.png
│   ├── roc_curve.png
│   ├── outcome_distribution.png
│   ├── multiclass_results.png
│   └── live_release_forecast.png
│
└── data/
    └── README.md
```

## How to Run

### 1. Clone the Repository

```bash
git clone https://github.com/your-username/shelter-animal-outcome-prediction.git
cd shelter-animal-outcome-prediction
```

### 2. Install Dependencies

```bash
pip install -r requirements.txt
```

### 3. Open the Notebook

```bash
jupyter notebook notebooks/shelter_animal_outcome_analysis.ipynb
```

Alternatively, open the notebook in Google Colab.

### 4. Dataset Access

The original dataset is publicly available from Data.gov. If the dataset is not included in this repository, download it from:

```text
https://catalog.data.gov/dataset/oas-live-release-rate
```

Then place the CSV file in the `data/` folder or update the notebook path accordingly.

## Example Results

| Task                       | Model / Method           |                                                  Result |
| -------------------------- | ------------------------ | ------------------------------------------------------: |
| Binary Classification      | Random Forest Classifier | ROC AUC around 0.88 after adding animal/intake features |
| Regression                 | Random Forest Regressor  |                  MAE: 7.64 days after outlier filtering |
| Multi-Class Classification | Random Forest Classifier |                                        Accuracy: 68.36% |
| Time-Series Forecasting    | SARIMAX                  |                     12-month live release rate forecast |

## Key Findings

* Intake condition, animal type, impound type, and intake subcategory were important for predicting outcomes.
* The dataset was highly imbalanced, with many more live outcomes than non-live outcomes.
* Removing extreme shelter-stay outliers improved length-of-stay prediction.
* Multi-class classification was more challenging than binary classification because outcome categories such as adoption, transfer, euthanasia, and return to owner have different patterns.
* Time-series forecasting can provide shelters with a forward-looking estimate of live release rate trends.

## Limitations

* The project uses historical shelter data, so predictions may reflect patterns in past shelter operations.
* The dataset is imbalanced, making non-live outcome prediction more difficult.
* Length-of-stay predictions can be affected by extreme outliers and operational factors not included in the dataset.
* The model does not replace human judgment or shelter policy decisions.
* Additional features such as animal age, breed, medical treatment history, and shelter capacity could improve future predictions.

## Future Improvements

* Add a dashboard using Streamlit or Tableau to make results easier to explore.
* Improve feature engineering with breed, age, and seasonal intake patterns if available.
* Compare additional models such as XGBoost, LightGBM, and logistic regression baselines.
* Add model explainability using feature importance or SHAP values.
* Create a simple API endpoint for prediction.
* Deploy the project as a web application.

## Project Summary

This project demonstrates a full machine learning workflow using real-world public shelter data. It includes data cleaning, feature engineering, classification, regression, risk scoring, time-series forecasting, and model evaluation. The project shows how machine learning can support operational planning and outcome analysis in animal shelter management.
