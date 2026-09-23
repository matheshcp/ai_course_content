# Course 1 — Datasets Manifest

Local path prefix for labs: `course-01-foundations-python-math-data/datasets/`

| Local file | Raw GitHub URL | Schema / notes |
|---|---|---|
| `sales_spreadsheet.csv` | (local only) | OrderID, Product, Quantity, Price, Region — 12 messy rows |
| `warehouse_messy_data.csv` | https://raw.githubusercontent.com/eyowhite/Messy-dataset/main/warehouse_messy_data.csv | Product ID, Product Name, Category, Warehouse, Location, Quantity, Price, Supplier, Status, Last Restocked — 1000 messy rows |
| `daily-min-temperatures.csv` | https://raw.githubusercontent.com/jbrownlee/Datasets/master/daily-min-temperatures.csv | Date, Temp — 3651 daily sensor readings |
| `Advertising.csv` | https://raw.githubusercontent.com/justmarkham/scikit-learn-videos/master/data/Advertising.csv | index, TV, Radio, Newspaper, Sales — 200 rows |
| `ab_test.csv` | https://raw.githubusercontent.com/TimileyinSamuel/A-B-Testing-for-E-Commerce-Website/main/ab_test.csv | id, time, con_treat, page, converted — ~294k rows |
| `sms.tsv` | https://raw.githubusercontent.com/justmarkham/DAT8/master/data/sms.tsv | tab: label, text — 5574 SMS spam/ham |
| `iris.csv` | https://raw.githubusercontent.com/mwaskom/seaborn-data/master/iris.csv | sepal_length, sepal_width, petal_length, petal_width, species |
| `tips.csv` | https://raw.githubusercontent.com/mwaskom/seaborn-data/master/tips.csv | total_bill, tip, sex, smoker, day, time, size |
| `tickets.csv` | https://raw.githubusercontent.com/vihar/datasets/master/tickets.csv | id, createdAt, user, updatedAt, description, status, priority, category, assignedTo — contains emails (PII demo) |
| `chinook.sqlite` | https://raw.githubusercontent.com/lerocha/chinook-database/master/ChinookDatabase/DataSources/Chinook_Sqlite.sqlite | full Chinook music DB (artists, albums, invoices…) |
| `HR-Employee-Attrition-synth.csv` | https://raw.githubusercontent.com/aaubs/ds-master/main/apps/M1-attrition-streamlit/HR-Employee-Attrition-synth.csv | 35 HR columns incl. Age, Attrition, Department, MonthlyIncome — 2000 rows |
| `emp_attrition.csv` | https://raw.githubusercontent.com/IBM/employee-attrition-aif360/master/data/emp_attrition.csv | same HR schema (IBM AIF360) — 1470 rows |
| `majors-list.csv` | https://raw.githubusercontent.com/fivethirtyeight/data/master/college-majors/majors-list.csv | FOD1P, Major, Major_Category |
| `baby-names.csv` | https://raw.githubusercontent.com/hadley/data-baby-names/master/baby-names.csv | year, name, percent, sex — 258k rows |

**Lab → dataset map (12 labs):** L1 `sales_spreadsheet.csv` · L2 `daily-min-temperatures.csv` · L3 `warehouse_messy_data.csv` · L4 `Advertising.csv` · L5 `ab_test.csv` · L6 `sms.tsv` · L7 `iris.csv` · L8 live API (no dataset) · L9 `chinook.sqlite` · L10 live scrape (no dataset) · L11 `tickets.csv`/`tips.csv` (viz/stats) · L12 `HR-Employee-Attrition-synth.csv` or `emp_attrition.csv`.
