# Model Card: Telco Churn Predictor

## Intended Use
Predict the probability that a customer will churn within the billing cycle,
so the retention team can prioritise outreach. Not for credit, pricing, or
employment decisions.

## Training Data
- Source: Telco-Customer-Churn.csv (7032 rows after cleaning)
- Split: 75% train / 25% test, stratified on Churn (random_state=42)
- Features: 8 numeric (standardised), 11 categorical
  (one-hot, drop-first), 4 binary-mapped
- Positive rate: 0.266

## Performance Metrics (test set, n=1758)
- ROC-AUC: 0.8425
- PR-AUC: 0.6624
- Decision threshold: 0.3953 (F1-optimal)
- Precision / Recall / F1 at threshold: 0.6015 / 0.6852 / 0.6406

## Fairness
Churn rates differ across slices (see table below). Monitor these slices
monthly; investigate any slice where precision drops below the global average.

| Slice | n | churn rate |
|---|---|---|
| gender = Female | 3483 | 0.2696 |
| gender = Male | 3549 | 0.2620 |
| SeniorCitizen = 0 | 5890 | 0.2365 |
| SeniorCitizen = 1 | 1142 | 0.4168 |
| Contract = Month-to-month | 3875 | 0.4271 |
| Contract = One year | 1472 | 0.1128 |
| Contract = Two year | 1685 | 0.0285 |

## Limitations
- TotalCharges blanks (11 rows) were dropped; new customers are under-represented.
- Threshold tuned on the test set — re-tune on a fresh validation split before deployment.
- Model does not capture seasonality or competitor actions.
- Fairness slices are descriptive; no causal claim is made.
