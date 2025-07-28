# Loan repayment_prediction

Predictive Default Risk Modeling for Smarter Loan Approvals

Problem Statement
The bank needs to approve as many high‑quality loans as possible while minimizing defaults. Using 45 000 historical loan applications, we will build a regularized logistic regression classifier that:

Estimates each applicant’s probability of default via .predict_proba() (to support both binary decisions and flexible interest‑rate/term adjustments)

Leverages applicant features:

Demographics: age, gender, education

Financial profile: income, employment tenure, home‑ownership

Credit history: credit score, length of credit history, past defaults

Loan details: amount, purpose, interest rate, payment‑to‑income ratio

Target Metric
We will optimize and report the F₁‑Score on the “default” (positive) class:

Balances precision (of the loans we flag as risky, how many truly default)
and recall (of all actual defaulters, how many we catch)

Ensures we both avoid false alarms on good borrowers and catch genuine high‑risk applicants

Interpretability & Calibration

Regularized logistic regression (with default L2 penalty) provides feature coefficients—so we can see how regularization shifts each coefficient and understand feature importance.

Using .predict_proba(), we obtain well‑calibrated default probabilities that can drive tiered interest‑rate or term‑adjustment policies.

Business Impact

Reduce credit losses by pre‑emptively flagging high‑risk applicants

Increase volume by fast‑tracking low‑risk approvals

Optimize pricing and underwriting workflows through data‑driven probability thresholds and adjustable loan terms

Dictionary : https://www.kaggle.com/datasets/udaymalviya/bank-loan-data


# Predictive Default Risk Modeling

## Project Overview  
Build a regularized logistic regression model on 45,000 historical loan applications to estimate each applicant’s probability of default. The model supports automated pre‑screening of low‑risk borrowers and flagging of high‑risk applicants, reducing credit losses and accelerating approval workflows.

## Data  
Raw data is stored in `data/raw/` (from Kaggle). Key fields include:  
- Demographics: `person_age`, `person_gender`, `person_education`  
- Financial profile: `person_income`, `person_emp_exp`, `person_home_ownership`  
- Loan details: `loan_amnt`, `loan_intent`, `loan_int_rate`, `loan_percent_income`  
- Credit history: `cb_person_cred_hist_length`, `credit_score`, `previous_loan_defaults_on_file`  
- Target: `loan_status` (0 = repaid, 1 = default)

