# Course 6 — Datasets Manifest

Local path prefix: `course-06-mlops-evaluation-responsible-ai/datasets/`

| Local file | Raw GitHub URL | Schema / notes |
|---|---|---|
| `german_credit_data.csv` | https://raw.githubusercontent.com/IBM/predict-credit-risk-with-jupyter-on-cloud-pak-for-data/main/data/german_credit_data.csv | 5001 rows, Risk target — fairness (L3), SHAP (L9) |
| `german_credit_prepared.csv` | https://raw.githubusercontent.com/Giskard-AI/giskard-examples/main/datasets/credit_scoring_classification_model_dataset/german_credit_prepared.csv | 1000 rows model-ready |
| `housing.csv` | https://raw.githubusercontent.com/ageron/data/main/housing/housing.csv | 20640 CA housing — drift/regression base |
| `ec2_cpu_utilization_825cc2.csv` | https://raw.githubusercontent.com/numenta/NAB/master/data/realAWSCloudwatch/ec2_cpu_utilization_825cc2.csv | timestamp, value — anomaly/drift |
| `ambient_temperature_system_failure.csv` | https://raw.githubusercontent.com/numenta/NAB/master/data/realKnownCause/ambient_temperature_system_failure.csv | timestamp, value — anomaly/drift |
| `sample_pii.csv` | https://raw.githubusercontent.com/nightfallai/dlp-sample-data/main/sample-pci.csv | Name, Credit Card — 10 rows |
| `sample_pii_aws.csv` | https://raw.githubusercontent.com/aws-samples/sample-gen-ai-pii-masking/main/sample_pii_data.csv | AWS PII masking sample |
| `pii_toydata.csv` | https://raw.githubusercontent.com/CharlotteLoobyRTI/IFDTC_Materials/main/PII_toydata2.csv | name, dob, race, address, email… — 10000 rows |

**Lab map (12 labs):** L1–L2 mostly inline model cards/eval fixtures · L3/L9 `german_credit_*` · L4–L5 `housing.csv` + NAB · L6 prompt/trace fixtures inline · L7 NAB series · L8 reuse course model · L10 PII CSVs · L11–L12 packaging/CI fixtures inline.
