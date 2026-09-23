# Course 2 — Lab Generation Prompts (13 labs)

Env: 🟢 Colab only. Data: see `datasets/DATASETS.md`.

**Global preamble (prepend to every prompt):**

```
Generate a complete self-contained lab as GitHub-flavored Markdown (Jupyter-friendly).
Style contract: H1+H2 header with blockquote (Scenario / You will learn / Time / Level / Needs 🟢);
mental-map table; numbered sections with runnable Python; dataset cell with local path + raw GitHub URL;
exactly 3 exercises with expected answers and <details> hints; Solutions with runnable code + printed outputs;
"What to learn next" bullets; fixed seeds (random_state=42); output files under labs/.
Stack: pandas, numpy, matplotlib/seaborn, scikit-learn as appropriate. Python 3.8+.
```

---

## Lab 1 — Linear Regression: Salary vs Experience

```
Title: Linear Regression — Salary vs Years of Experience
Dataset: local datasets/Salary_Data.csv | raw https://raw.githubusercontent.com/AnnaShestova/salary-years-simple-linear-regression/master/Salary_Data.csv
Columns: YearsExperience, Salary (30 rows)
Teach: simple OLS intuition, fit with sklearn LinearRegression, slope/intercept interpretation, R², residual plot.
Split: train_test_split(test_size=0.3, random_state=42).
Exercises: (1) predict salary at 5.5 years; (2) MSE on test; (3) what R²=0.95 does/doesn't mean.
Level Beginner ~40min.
```

---

## Lab 2 — Housing Price Regression (California)

```
Title: Housing Price Regression — Full Workflow
Dataset: local datasets/housing.csv | raw https://raw.githubusercontent.com/ageron/data/main/housing/housing.csv
Columns: longitude, latitude, housing_median_age, total_rooms, total_bedrooms, population, households, median_income, median_house_value, ocean_proximity
Teach: train/test split, preprocessing (impute total_bedrooms, one-hot ocean_proximity), LinearRegression vs RandomForest, cross-val, feature importance.
Exercises: (1) baseline mean-predictor MAE; (2) RF MAE vs LR MAE; (3) top 3 features by importance.
Seed 42. Save labs/housing_predictions.csv. Level Intermediate ~75min.
```

---

## Lab 3 — Logistic Regression: Telco Churn

```
Title: Logistic Regression — Telco Customer Churn
Dataset: local datasets/Telco-Customer-Churn.csv | raw https://raw.githubusercontent.com/IBM/telco-customer-churn-on-icp4d/master/data/Telco-Customer-Churn.csv
Columns: 21 cols; target Churn ∈ {Yes,No}; TotalCharges has blanks → coerce.
Teach: binary classification, encode categoricals, logistic coefficients as log-odds, threshold tuning, confusion matrix, precision/recall.
Exercises: (1) churn base rate; (2) model at threshold 0.5 — precision/recall; (3) threshold for recall≥0.8.
Seed 42. Level Intermediate ~60min.
```

---

## Lab 4 — KNN Classification on Iris

```
Title: K-Nearest Neighbors on Iris
Dataset: local datasets/iris.csv | raw https://raw.githubusercontent.com/mwaskom/seaborn-data/master/iris.csv
Teach: distance metrics, scaling necessity, k selection via cross_val, decision-boundary intuition.
Exercises: (1) accuracy for k=1..15 CV curve; (2) why unscaled features hurt KNN; (3) classify a hand-entered flower.
Seed 42. Level Beginner ~40min.
```

---

## Lab 5 — Credit Risk: German Credit (feature-engineered)

```
Title: Credit Risk Classification — German Credit
Dataset: local datasets/german_credit_data.csv | raw https://raw.githubusercontent.com/IBM/predict-credit-risk-with-jupyter-on-cloud-pak-for-data/main/data/german_credit_data.csv
Columns: CustomerID, CheckingStatus, LoanDuration, CreditHistory, LoanPurpose, LoanAmount, …, Risk
Teach: mixed categorical/numeric preprocessing, class imbalance (class_weight), ROC-AUC, threshold selection.
Exercises: (1) risk rate by CheckingStatus; (2) ROC-AUC; (3) top 5 features via permutation importance.
Seed 42. Save labs/german_credit_scores.csv. Level Intermediate ~60min.
```

---

## Lab 6 — Credit Risk Fairness Preview (bridge to Course 6)

```
Title: Credit Risk + Fairness Basics
Dataset: local datasets/german_credit_prepared.csv | raw https://raw.githubusercontent.com/Giskard-AI/giskard-examples/main/datasets/credit_scoring_classification_model_dataset/german_credit_prepared.csv (alt GermanCredit_r.csv)
Columns: default (target) + 21 features incl. sex, age
Teach: group-wise accuracy/TPR, disparate impact ratio, why "equal accuracy ≠ fair", teaser metrics used later in Course 6.
Exercises: (1) TPR by sex; (2) disparate impact ratio; (3) one mitigation idea (reweighing sketch).
Level Intermediate ~50min. Seed 42.
```

---

## Lab 7 — Neural Net from Scratch on MNIST Subset

```
Title: Feed-Forward Neural Net from Scratch (MNIST 5k)
Dataset: local datasets/mnist_train.csv | raw https://raw.githubusercontent.com/albrow/golearn-digit-recognition/master/data/mnist_train.csv (pixel0..pixel783, 5000 rows; if label col missing use albrow layout — document assumed label column or merge with known labels)
Teach: perceptron → MLP, forward pass with numpy, ReLU/softmax, cross-entropy, one training loop epoch, accuracy curve.
Exercises: (1) 200-epoch training curve plot; (2) effect of learning rate 0.1 vs 0.01; (3) confusion matrix for digits 3 vs 8.
Seed 42. CPU-only, <2min runtime. Level Intermediate–Advanced ~75min.
```

---

## Lab 8 — Customer Segmentation with K-Means

```
Title: Customer Segments — Mall K-Means
Dataset: local datasets/Mall_Customers.csv | raw https://raw.githubusercontent.com/krishnaik06/DBSCAN-Algorithm/master/Mall_Customers.csv
Columns: CustomerID, Genre, Age, Annual Income (k$), Spending Score (1-100)
Teach: unsupervised learning, elbow method, silhouette score, StandardScaler, interpreting centroids into personas.
Exercises: (1) elbow k=1..10; (2) best-k silhouette; (3) name each segment with a marketing persona.
Seed 42. Save labs/mall_segments.csv. Level Intermediate ~50min.
```

---

## Lab 9 — Association Rules: Grocery Baskets

```
Title: Market Basket Analysis — Groceries
Dataset: local datasets/groceries.csv | raw https://raw.githubusercontent.com/shreyaskhadse/data_files/master/groceries.csv
Format: headerless CSV, variable-length item rows (9835 baskets)
Teach: transaction encoding, support/confidence/lift, Apriori-style candidate generation (or mlxtend if available with pure-python fallback), top rules.
Exercises: (1) top 10 itemsets by support; (2) rules for "whole milk" consequent; (3) lift>2 rules only.
Level Intermediate ~60min.
```

---

## Lab 10 — Classification with Gradient Boosting (Gapminder)

```
Title: Predicting Life Expectancy Bands — Gradient Boosting
Dataset: local datasets/gapminderDataFiveYear.csv | raw https://raw.githubusercontent.com/plotly/datasets/master/gapminderDataFiveYear.csv
Columns: country, year, pop, lifeExp, gdpPercap, continent
Task: bin lifeExp into Low/Mid/High (terciles) and classify continent→ or gdp features → band.
Teach: GradientBoostingClassifier / HistGradientBoosting, feature importance, leakage caution when using year.
Exercises: (1) tercile thresholds; (2) test accuracy; (3) SHAP-free partial dependence sketch on gdpPercap.
Seed 42. Level Intermediate ~55min.
```

---

## Lab 11 — Regularisation: Housing Revisited (Ridge/Lasso)

```
Title: Regularisation — Ridge & Lasso on Housing
Dataset: local datasets/housing.csv | raw https://raw.githubusercontent.com/ageron/data/main/housing/housing.csv
Teach: overfitting demo (high-degree poly vs ridge), alpha path, lasso sparsity, coefficient standardisation.
Exercises: (1) train vs val MAE for degree 1..10; (2) best alpha via GridSearchCV; (3) non-zero lasso coefs.
Seed 42. Level Intermediate ~55min.
```

---

## Lab 12 — Model Selection & Evaluation Deep-Dive (Default credit)

```
Title: Model Selection & Metrics — Credit Default
Dataset: local datasets/Default.csv | raw https://raw.githubusercontent.com/vincentarelbundock/Rdatasets/master/csv/ISLR/Default.csv
Columns: rownames, default, student, balance, income (10000 rows)
Teach: accuracy paradox, ROC/PR curves, k-fold CV, comparing Logistic vs RF vs GBM fairly, calibration.
Exercises: (1) majority-class baseline; (2) PR-AUC for 3 models; (3) reliability diagram sketch.
Seed 42. Level Intermediate ~60min.
```

---

## Lab 13 — Capstone: End-to-End Churn Model Card

```
Title: Capstone — Telco Churn End-to-End + Model Card
Dataset: local datasets/Telco-Customer-Churn.csv (+ iris/titanic as secondary EDA warmup)
Deliverable: full pipeline (clean → encode → train 2 models → select → threshold → explain) plus a written model card in labs/churn_model_card.md covering intended use, metrics, limitations, fairness slice by gender.
Exercises: (1) reproduce Lab 3 metrics with new encoding; (2) fairness slice table; (3) fill model card template.
Seed 42. Level Capstone ~90min.
```
