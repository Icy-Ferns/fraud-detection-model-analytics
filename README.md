# Fraud Detection Model (Bank A Transactions)

A machine learning classification project that detects fraudulent bank transactions using an XGBoost model trained on a large, highly imbalanced synthetic dataset.

## Project highlights

- Best model: **XGBoost**, selected after comparing Logistic Regression, KNN, Random Forest, LightGBM, and XGBoost using cross-validation.
- Dataset used in the original project: `bank_A_transactions.csv` (500,000 synthetically generated transactions; ~98% genuine / ~2% fraud).
- Note: the dataset file is **not included in this GitHub repository** due to GitHub file size limits. The instructions below show how to run the analysis locally with a CSV that follows the same schema.

## Disclaimer (synthetic data + ASB case study)

This project was built on a **synthetically generated** dataset created for academic/portfolio purposes.
It does **not** contain real customer, merchant, or bank transaction records, and some fields are explicitly synthetic (e.g., `MerchantLocation`).
The ASB case study material in this repository is included as **industry context** for how banks apply analytics; it does **not** imply the dataset originates from ASB or reflects ASB customer data or systems.

## Business problem

Banks face evolving fraud patterns and must detect suspicious transactions quickly to reduce financial losses and protect customer trust.
This project’s objective is to enhance “Bank A” fraud detection by building and evaluating machine learning models, then proposing an operational response framework based on risk tiers.

## Data

### Dataset characteristics (original project dataset)

- Size and imbalance: 500,000 transactions with ~2% labeled as fraud.
- Signals highlighted in the project poster: fraud frequency increases with customer age and transaction amount, with higher observed fraud occurrence on Wednesdays; ATM withdrawals are highlighted as higher risk for seniors.

### Data dictionary (required columns)

To run the analysis locally, prepare a CSV matching these core fields:

- `TransactionID`: Unique identifier per transaction.
- `CustomerID`: Unique identifier per customer.
- `TransactionDate`: Date-time of the transaction.
- `TransactionAmount`: Transaction value.
- `MerchantID`: Merchant identifier (can be missing).
- `MerchantLocation`: Merchant location (synthetic in the original dataset).
- `TransactionType`: e.g., purchase, refund, withdrawal.
- `FraudLabel`: 0 = not fraud, 1 = fraud.
- `Age`: Customer age.
- `AccountID`: Account identifier.
- `jointflag`: Joint account indicator.
- `agent`: Device/user agent (ATM, browser, mobile, etc.).
- `balance`: Account balance.

## Methodology

### Feature engineering and preprocessing

The Quarto analysis (`Guardian_of_the_vault_Analytics.qmd`) performs the following transformations before modeling:

- Parse `TransactionDate` into `Date` and `Time`, and derive day-of-week features.
- Create `agentcategory` (ATM / Android / iOS / Browser / Offline / Unknown) with missing agents treated as offline.
- Create a `withdrawal_atm` indicator for withdrawal + ATM interactions.
- Normalize numeric predictors, one-hot encode categorical predictors, and remove zero-variance predictors.
- Handle class imbalance using up-sampling to a 1:1 ratio inside the training recipe.

### Data splitting and validation

- Train/test split: 75% training, 25% testing (stratified by FraudLabel).
- Cross-validation: 10-fold CV on the training set with stratification.

### Evaluation metrics

Metrics include accuracy, ROC AUC, sensitivity, specificity, balanced accuracy, PPV, NPV, and recall.

## Results

XGBoost is selected as the best-performing model. The project poster reports ROC AUC ~0.87, sensitivity ~0.78, and specificity ~0.77 (test-based).
The analysis also produces ROC curves, a confusion matrix, and feature importance plots.

## Recommendations (operational)

- Tiered response: classify transactions into lower vs higher risk and trigger different actions.
- User-linked delay option: allow customers to enable a short delay (e.g., 30 minutes) for high-risk transactions for confirmation.
- Customer communication: lower-risk push notifications vs higher-risk calls to customers.

## How to run

### Prerequisites

- R (and ideally RStudio).
- Quarto (to render the `.qmd` into HTML/PDF).
- update the dataset path in `Guardian_of_the_vault_Analytics.qmd` to match your chosen location.

## Repository contents

- `Guardian_of_the_vault_Analytics.qmd`: End-to-end analysis (data prep → modeling → evaluation → feature importance).
- `Poster-Guardian-of-the-vault.pdf`: One-page project summary and recommendations.
- `Project-metadata.pdf`: Data dictionary for Bank A transaction fields.
- `ASB_casestudy.pdf`: Industry context / case-study material on analytics in banking (ASB).
- `Readme.md` : Thats me :)

### Please note that this project was achieved through collaborative efforts.
