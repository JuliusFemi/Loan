# Predictive Default Risk Modeling for Smarter Loan Approvals

## Project Summary

This project builds a logistic regression model using a dataset of 45,000 historical loan applications. The goal is to estimate the probability that a borrower will default, enabling smarter, data-driven loan approval decisions.

## Objective

- Balance recall (identify actual defaulters) and precision (avoid false positives).
- Optimize the F1-score on the “default” class as the primary evaluation metric.

## Dataset Overview
[Data Dictionary](https://www.kaggle.com/datasets/udaymalviya/bank-loan-data)
- Total Records: 45,000 loan applications  
- Features: 14 columns including demographics, financial profile, credit history, and loan details  
- Target Variable: `loan_status`  
  - 0 = Defaulted (77.8%)  
  - 1 = Repaid (22.2%)

## Key Features

### Demographics  
- `person_age`  
- `person_gender`  
- `person_education`

### Financial Profile  
- `person_income`  
- `person_emp_exp`  
- `person_home_ownership`

### Credit History  
- `credit_score`  
- `cb_person_cred_hist_length`  
- `previous_loan_defaults_on_file`

### Loan Details  
- `loan_amnt`  
- `loan_int_rate`  
- `loan_percent_income`  
- `loan_intent`  
- `loan_status`

## Data Preparation

| Column                          | Current Type | To Convert To | Reason |
|---------------------------------|--------------|---------------|--------|
| `person_gender`                   | object       | category      | Limited unique values |
| `person_education`               | object       | category      | one-hot encoding |
| `person_home_ownership`          | object       | category      | Nominal values |
| `loan_intent`                    | object       | category      | Categorical purpose |
| `previous_loan_defaults_on_file` | object       | int (Yes=1, No=0) | Binary conversion |
| `loan_status`                    | int64        | Already correct | Target variable |
| `person_age, person_income, etc.` | numeric      | No change     | Already numeric |

## Data  
- Target: `loan_status` (0 = repaid, 1 = default)


## Exploratory Data Analysis (EDA)

### Age Distribution
- Most applicants are aged between 20 and 35, with a peak in the mid-20s.
- Very few applicants are older than 60.

### Ethics & Regulatory Considerations for Using Age

- **Protected Characteristic:**  
  Under the Equal Credit Opportunity Act, age is a protected class lenders cannot use it to deny or price credit. Hence, age will be dropped from the feature.

Justification for removing age: [Reference](https://www.consumerfinance.gov/ask-cfpb/can-a-card-issuer-consider-my-age-when-deciding-whether-to-issue-a-credit-card-to-me-en-20/#:~:text=Under%20the%20Equal%20Credit%20Opportunity,used%20in%20the%20applicant's%20favor)


### Gender Distribution
- Male: ~55%  
- Female: ~45%

### Education Distribution
- Bachelor’s degree: most common (~30%)  
- Associate and High School: ~27% each  
- Master’s: ~15%  
- Doctorate: ~1–2%

### Education vs Loan Repayment

| Education Level | Default Rate (%) |
|-----------------|------------------|
| Master          | 78.24            |
| Associate       | 77.97            |
| High School     | 77.69            |
| Bachelor        | 77.48            |
| Doctorate       | 77.13            |

**Master** holders show the highest default rate (78.24%), followed by **Associate** with (77.97 %) default rate. **High school** with (77.69%), **Bachelor** with (77.48%) and **Doctorate** with 77.13% default rate

## Income Analysis

### Distribution
- Median income: approximately $67,048.

### Income vs Loan Status
- Defaulted (0): Median ≈ $72,928  
- Repaid (1): Median ≈ $50,629  
- Defaulters have slightly higher median income.

## Employment Experience

### Distribution
- Median experience: approximately 3 years.
- Right skewed distribution with outliers up to 125 years, which will be removed.

### Experience vs Loan Status
- Defaulted: Median ≈ 4 years  
- Repaid: Median ≈ 3 years  
- Slightly higher median tenure for defaulters.

**Outliers:**  
- Many extreme experince values above 60 years. These are considered errors and will be dropped.
[Reference](https://www.guardianlife.com/retirement/average-age)

## Home Ownership

- Renters: approximately 52%  
- Mortgage: approximately 41%  
- Own: approximately 7%  
- Other: less than 1%

**Insight:** Most applicants are either renters or mortgage holders.

## Loan Amount Analysis

### Distribution
- Common clusters at $5,000, $10,000, $15,000, $20,000.
- Median loan amount is around $8,000.

### Loan Amount vs Loan Status
- Defaulted: Median ≈ $8,000  
- Repaid: Median ≈ $9,500  
- Defaulters tend to request slightly lower amounts.

## Modeling 

### Data Dropped to Ensure Model Success
- **Protected Characteristic:**  
  Under the Equal Credit Opportunity Act, age is a protected class lenders cannot use it to deny or price credit. Hence, age will be dropped from the features.
`Refrence`: https://www.consumerfinance.gov/ask-cfpb/can-a-card-issuer-consider-my-age-when-deciding-whether-to-issue-a-credit-card-to-me-en-20/#:~:text=Under%20the%20Equal%20Credit%20Opportunity,used%20in%20the%20applicant's%20favor
- Dropped columns 'loan_int_rate' and 'loan_percent_income' to avoid data leakage.

### F1 Scores (base default rate 77.8)
| Model                         | F1 Score |
|------------------------------|----------|
| Vanilla Logistic Regression  | 0.917    |
| Tuned Logistic Regression (C=0.01) | 0.920    |
| Random Forest Classifier     | 0.940    |

###  Logistic Regression Coefficients For Interpretation

| Feature                             | Coefficient | Interpretation |
|-------------------------------------|-------------|----------------|
| previous_loan_defaults_on_file_Yes | **+5.06**    | Strongest predictor of default — having a prior default increases the odds substantially. |
| person_income                       | **+1.06**    | Higher income is associated with a higher chance of default (possibly counterintuitive; may reflect riskier borrowers with high reported income). |
| person_home_ownership_RENT         | **−0.83**    | Renters are less likely to default compared to the baseline category (possibly due to underwriting bias or data pattern). |
| loan_intent_VENTURE                | **+0.58**    | Loans intended for ventures carry higher default risk. |
| loan_amnt                           | **−0.57**    | Higher loan amounts are associated with lower default probability — potentially indicating trust in high-quality borrowers. |
| loan_intent_EDUCATION              | **+0.46**    | Educational loans tend to have higher default risk. |
| credit_score                        | **+0.35**    | Surprisingly positive; could be due to correlation with other riskier variables or multicollinearity. |
| loan_intent_PERSONAL               | **+0.25**    | Personal loan intents carry elevated risk of default. |
| person_home_ownership_OWN          | **+0.17**    | Homeowners (vs. baseline group) show slightly higher default risk in this model. |

## Conclusion
- The Random Forest model is more accurate than our Logistic Regression model; however, it is far less interpretable.
- We recommend using the models as appropriate: Logistic Regression for interpretability, and Random Forest for predictive accuracy.
