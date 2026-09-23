# Course 6 — Lab Generation Prompts (12 labs)

Env: mostly 🟠 Colab + Helicone for LLM-eval labs; classical MLOps labs 🟢. Budget caps noted. Data: see `datasets/DATASETS.md`.

**Global preamble (prepend to every prompt):**

```
Generate a complete self-contained lab as GitHub-flavored Markdown (Jupyter-friendly).
Style contract: H1+H2 + blockquote header (Scenario / You will learn / Time / Level / Needs 🟢 or 🟠);
mental-map table; numbered sections with runnable Python; dataset cell = local datasets/<file> AND raw GitHub URL fallback;
exactly 3 exercises with expected answers + <details> hints; Solutions with runnable code + printed outputs;
"What to learn next"; fixed seeds; outputs under labs/.
🟠 labs: Helicone setup from Colab secrets, call budget, offline fixture fallback.
Stack: pandas, scikit-learn, shap/elida5 where possible on CPU; matplotlib; optional evidently/aif360 with pure-python fallbacks documented.
```

---

## Lab 1 — ML Project Lifecycle & Model Cards

```
Title: ML Lifecycle + Your First Model Card
Dataset: inline template; optionally peek german_credit_data.csv for a real model to describe (local | raw IBM URL in DATASETS.md). Budget: 0 API calls (🟢) or ≤5 🟠 for drafting help.
Teach: CRISP-DM-ish stages, model card sections (intended use, metrics, fairness, caveats), dataset datasheet concept.
Exercises: (1) fill model card for Telco churn from Course 2; (2) list 5 risks of deploying it as-is; (3) write datasheet for german_credit_data.
Output: labs/model_card_template.md. Level Beginner ~45min.
```

---

## Lab 2 — Reproducibility: Seeds, Versioning, Data Lineage

```
Title: Reproducibility Engineering
Dataset: german_credit_data.csv (local | raw) — train twice, must match. Budget: 🟢.
Teach: random_state everywhere, requirements pin, data hash (sha256), config-as-code, experiment naming, git hygiene for notebooks (nbstripout).
Exercises: (1) hash dataset file; (2) train logistic twice → assert identical metrics; (3) write labs/repro_checklist.md.
Level Beginner ~40min.
```

---

## Lab 3 — Fairness Metrics on Credit Data

```
Title: Fairness Metrics — Credit Risk Case
Dataset: local datasets/german_credit_data.csv (or german_credit_prepared.csv with sex/age) | raw URLs in DATASETS.md.
Teach: demographic parity, equal opportunity, disparate impact, calibration across groups; proxy variables; "fairness through unawareness" fallacy.
Exercises: (1) train baseline classifier; (2) compute TPR/FPR by Sex and Age group; (3) disparate impact ratio + interpretation (80% rule).
Seed 42. Output: labs/fairness_report.md. Level Intermediate ~65min. 🟢.
```

---

## Lab 4 — Explainability: SHAP & Permutation Importance

```
Title: Explainability — SHAP on German Credit
Dataset: german_credit_data.csv or german_credit_prepared.csv | raw URLs in DATASETS.md.
Teach: global vs local explanation, permutation importance, SHAP TreeExplainer (shap library) with waterfall/force plot fallback to coefficient bars if shap install fails.
Exercises: (1) top-10 global features; (2) explain 3 individual predictions; (3) contrast SHAP vs built-in feature_importances_ ranking.
Seed 42. Level Intermediate ~60min. 🟢.
```

---

## Lab 5 — Data Drift Monitoring

```
Title: Data Drift Detection — Housing & Metrics Series
Dataset: local datasets/housing.csv (ageron) + NAB ec2_cpu / ambient_temperature CSVs | raw URLs in DATASETS.md.
Teach: PSI, KS test, population stability alerts, sliding-window baselines, drift vs concept drift, alert tuning to limit false alarms.
Exercises: (1) split housing into "before/after" by median_income terciles — PSI; (2) rolling KS on temp series; (3) design an alert rule with ≤5% false positive target on NAB train segment.
Seed 42. Level Intermediate ~60min. 🟢.
```

---

## Lab 6 — Concept Drift & Retraining Triggers

```
Title: Concept Drift & When to Retrain
Dataset: NAB series + housing with simulated label shift (shift median_house_value relationship in second half). Budget: 🟢.
Teach: accuracy-under-drift simulation, Page-Hinkley / ADWIN intuition, retrain triggers, champion/challenger, canary rollout sketch.
Exercises: (1) inject covariate shift and plot metric decay; (2) trigger retrain when rolling accuracy < baseline−3pp for 5 windows; (3) write labs/retrain_policy.md.
Level Intermediate ~60min. Seed 42.
```

---

## Lab 7 — Anomaly Monitoring in Production (NAB)

```
Title: Production Anomaly Monitoring
Dataset: local datasets/ec2_cpu_utilization_825cc2.csv + ambient_temperature_system_failure.csv | raw NAB URLs in DATASETS.md.
Teach: real-time vs batch monitors, threshold + statistical detectors, alert fatigue, runbooks, dashboard KPIs (latency, error rate proxy).
Exercises: (1) z-score + EWMA detectors; (2) tune threshold to hit ~1% alert rate; (3) draft runbook for a CPU anomaly.
Level Intermediate ~50min. 🟢.
```

---

## Lab 8 — Bias & Responsible Release Checklist

```
Title: Responsible Release Checklist
Dataset: reuse german_credit fairness outputs from Lab 3 + PII toydata for redaction drill (pii_toydata.csv | raw CharlotteLoobyRTI URL).
Teach: red-teaming checklist, dual-use review, privacy redaction (email/phone regex), impact assessment lite, rollback plan.
Exercises: (1) redact PII columns in toydata sample; (2) complete release checklist for churn model; (3) identify 3 misuse scenarios + mitigations.
Output: labs/release_checklist.md. Level Beginner–Intermediate ~50min. 🟢.
```

---

## Lab 9 — Privacy: PII Detection & Masking

```
Title: PII Detection & Masking
Dataset: local datasets/sample_pii.csv (Name, Credit Card) | raw nightfall URL; sample_pii_aws.csv | raw aws-samples URL; pii_toydata.csv (10k rows with email/dob/address) | raw URL in DATASETS.md.
Teach: regex for email/phone/card (Luhn check), named-entity PII, reversible tokenisation vs irreversible hashing, logging hygiene, why you never send raw PII to an LLM.
Exercises: (1) detect+mask all emails and card numbers in the three files; (2) Luhn-validate card numbers before masking; (3) measure false positives on pii_toydata free-text address field.
Output: labs/pii_masked_sample.csv. Level Intermediate ~60min. 🟢.
```

---

## Lab 10 — Privacy-Preserving LLM Calls (🟠)

```
Title: Sending Only What’s Necessary to an LLM
Dataset: reuse masked outputs from Lab 9 + inline redaction pipeline. Budget: ≤15 Helicone calls 🟠.
Teach: pre-send redaction pipeline, structured payloads, differential privacy lite (noise on aggregates), data retention policy, Helicone logging config (what NOT to log).
Exercises: (1) build redact→call→unredact pipeline; (2) verify logs contain no raw PII; (3) compute k-anonymity sketch on quasi-identifiers (age+zip+gender).
Level Intermediate ~55min.
```

---

## Lab 11 — Testing & CI for ML (tests + data contracts)

```
Title: ML Testing & CI
Dataset: german_credit or housing as fixture; write pytest suite. Budget: 🟢.
Teach: unit tests for features, data contract (schema asserts), model quality gates (metric threshold in CI), notebook execution in CI (papermill/nbclient), shadow deployment note.
Exercises: (1) schema test for housing.csv columns/dtypes; (2) quality gate test (AUC ≥ 0.7); (3) sketch GitHub Actions YAML for the suite.
Output: labs/test_features.py + labs/test_model_gate.py. Level Intermediate ~55min.
```

---

## Lab 12 — Capstone: Deploy-ish Notebook with Monitoring Loop

```
Title: Capstone — Serving Slice + Monitoring
Dataset: german_credit_data + NAB drift series + PII masker from Lab 9.
Deliverable: single notebook that (1) trains model with seed, (2) writes model card stub, (3) "serves" predict() on new batch, (4) computes drift vs training, (5) writes labs/monitoring_snapshot.json + alerts list, (6) includes PII-safe logging demo.
Exercises: (1) end-to-end pipeline function; (2) trigger a synthetic drift alert; (3) labs/capstone_report.md — what you'd need for real deployment (endpoint, auth, rollback).
Level Capstone ~100min. 🟢 core; optional 🟠 ≤10 calls for report drafting.
```
