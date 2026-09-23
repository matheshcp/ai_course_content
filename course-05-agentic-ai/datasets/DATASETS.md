# Course 5 — Datasets Manifest

Local path prefix: `course-05-agentic-ai/datasets/`

| Local file | Raw GitHub URL | Schema / notes |
|---|---|---|
| `customer_support_dataset.csv` | https://raw.githubusercontent.com/gakudo-ai/open-datasets/refs/heads/main/customer_support_dataset.csv | prior_tickets, account_age_days, text, label — 50 classified tickets |
| `ag_news_25k.csv` | https://raw.githubusercontent.com/nomic-ai/maps/main/data/ag_news_25k.csv | id, text, label — 25k AG News |
| `mqp.csv` | https://raw.githubusercontent.com/curai/medical-question-pair-dataset/master/mqp.csv | Medical question pairs |
| `click_data.csv` | https://raw.githubusercontent.com/Abhishek-2307/Customer-Analytics-A-B-Testing/master/click_data.csv | A/B click experiment log |
| `simpsons_AB_test_mock_data.csv` | https://raw.githubusercontent.com/MallikaDey/SimpsonsParadox/refs/heads/main/data/simpsons_AB_test_mock_data.csv | Simpson's-paradox A/B mock |

Most agentic labs use small inline fixtures (tool schemas, traces, memory). These files feed classification/tool-calling and analysis agents.
