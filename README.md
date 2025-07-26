# Compensation_prediction

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