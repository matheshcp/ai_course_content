# Course 1 — Lab Generation Prompts (12 labs)

Env: 🟢 Colab only. Style reference: `labs/lab-01-python-refresher-for-data-people.md`. Data: see `datasets/DATASETS.md`.

**Global preamble to prepend to every prompt below:**

```
Generate a complete self-contained lab as GitHub-flavored Markdown (Jupyter-friendly).
Follow this style contract exactly:
- H1 title + H2 track subtitle, then a blockquote with Scenario / You will learn / Time / Level / Needs (include env badge).
- Include a "mental map" table mapping prior knowledge to new concepts.
- Numbered H2 or H3 sections with runnable Python blocks; short ">" callouts for why it matters.
- Dataset cell: load from local datasets/<file> if present, else download via raw GitHub URL (give both paths).
- Exactly 3 exercises with expected answers and <details> hints; then Solutions section with runnable code and printed expected outputs.
- End with "What to learn next" bullets.
- Use fixed random seeds; write any output CSVs to labs/.
- Python 3.8+; prefer stdlib + pandas/matplotlib as appropriate for the course level.
```

---

## Lab 1 — Python Refresher for Data People ✅ (already written)

*Skip generation; file exists: `labs/lab-01-python-refresher-for-data-people.md`.*

---

## Lab 2 — Sensor Time Series with Pure Python

```
Title: Sensor Time Series with Pure Python
Track: Data wrangling without pandas
Scenario: Ops hands you daily-min-temperatures.csv (Date, Temp — 3651 rows, 1981–1990 Melbourne min temps). You must answer: hottest day, coldest day, monthly averages, and detect 3-day cold streaks — using only the stdlib (csv, datetime, statistics).
Dataset: local datasets/daily-min-temperatures.csv | raw https://raw.githubusercontent.com/jbrownlee/Datasets/master/daily-min-temperatures.csv
Columns: Date (YYYY-MM-DD), Temp (float °C)
Teach: csv.DictReader, datetime.date parsing, list/dict aggregation, statistics.mean, simple anomaly detection (|z|>2).
Exercises: (1) monthly mean temps dict; (2) count days Temp > 25; (3) find longest run of consecutive days Temp < 10.
Output: print summary table; no file required.
Time ~45min Level Beginner Needs Python 3.8+ only. Badge 🟢.
```

---

## Lab 3 — Messy Retail Cleanup Pipeline

```
Title: Messy Retail Cleanup Pipeline
Track: Data quality
Scenario: warehouse_messy_data.csv has 1000 rows of product inventory with mixed-case names, Quantity as "two hundred", NaN restock dates, inconsistent Status. Clean it and report stock value by Category.
Dataset: local datasets/warehouse_messy_data.csv | raw https://raw.githubusercontent.com/eyowhite/Messy-dataset/main/warehouse_messy_data.csv
Columns: Product ID, Product Name, Category, Warehouse, Location, Quantity, Price, Supplier, Status, Last Restocked
Teach: string normalisation, numeric coercion with fallbacks, date parsing (dd/mm/yyyy), validation rules, writing cleaned CSV to labs/warehouse_clean.csv.
Exercises: (1) fix Quantity word-numbers; (2) flag Status not in {In Stock, Out of Stock}; (3) total inventory value per Category.
Time ~60min Level Beginner–Intermediate Needs pandas optional. Badge 🟢.
```

---

## Lab 4 — Exploratory Data Analysis: Advertising Channels

```
Title: EDA — TV, Radio, Newspaper vs Sales
Track: First real EDA
Scenario: Marketing asks which channel drives Sales. Load Advertising.csv (200 rows), compute correlations, plot scatter matrices, and write a 5-bullet findings memo.
Dataset: local datasets/Advertising.csv | raw https://raw.githubusercontent.com/justmarkham/scikit-learn-videos/master/data/Advertising.csv
Columns: (index), TV, Radio, Newspaper, Sales
Teach: pandas read_csv, describe/corr, matplotlib scatter, interpreting correlation vs causation.
Exercises: (1) corr matrix; (2) highest single-feature R vs Sales; (3) bin TV into quartiles, mean Sales per bin.
Output: optional labs/eda_findings.md. Time ~45min Level Beginner. Badge 🟢.
```

---

## Lab 5 — A/B Test Analysis

```
Title: A/B Test Analysis — Old vs New Page
Track: Experimentation basics
Scenario: ab_test.csv logs 294k sessions with con_treat (control/treat), page, converted (0/1). Compute conversion rates, a two-proportion z-test, and a 95% CI; conclude whether to ship.
Dataset: local datasets/ab_test.csv | raw https://raw.githubusercontent.com/TimileyinSamuel/A-B-Testing-for-E-Commerce-Website/main/ab_test.csv
Columns: id, time, con_treat, page, converted
Teach: groupby rates, standard error, z-test (scipy or manual), power sketch, common pitfalls (peeking).
Exercises: (1) conversion by group; (2) z-test + p-value; (3) minimum detectable effect at 80% power.
Time ~50min Level Intermediate. Badge 🟢.
```

---

## Lab 6 — Naive Bayes SMS Spam Classifier from Scratch

```
Title: SMS Spam Classifier (Naive Bayes, no sklearn)
Track: Probability meets text
Scenario: sms.tsv has 5574 labeled SMS (ham/spam). Build a multinomial Naive Bayes classifier with Laplace smoothing using only Python collections.
Dataset: local datasets/sms.tsv | raw https://raw.githubusercontent.com/justmarkham/DAT8/master/data/sms.tsv
Format: tab-separated label<TAB>text (no header)
Teach: tokenisation, stopwords, word counts, log-priors, Bayes rule, train/test split with seed=42, confusion matrix by hand.
Exercises: (1) top-10 spam-indicative tokens; (2) accuracy on 20% holdout; (3) why "free" alone misclassifies marketing ham.
Time ~60min Level Intermediate. Badge 🟢.
```

---

## Lab 7 — Iris: Your First Structured Dataset Deep-Dive

```
Title: Iris Deep-Dive — Stats + First Plot
Track: Statistics for analysts
Scenario: Classic iris.csv (150 rows, 4 numeric + species). Compute per-species stats, a one-way ANOVA by hand or scipy, and publish a pair-plot.
Dataset: local datasets/iris.csv | raw https://raw.githubusercontent.com/mwaskom/seaborn-data/master/iris.csv
Columns: sepal_length, sepal_width, petal_length, petal_width, species
Teach: groupby.agg, variance, F-stat intuition, seaborn pairplot (or matplotlib fallback).
Exercises: (1) mean petal_length by species; (2) which feature best separates setosa vs versicolor; (3) 95% CI for sepal_width of virginica.
Time ~40min Level Beginner. Badge 🟢.
```

---

## Lab 8 — Live Public API Lab (JSON + rate limits)

```
Title: Working with a Live Public API
Track: Data outside files
Scenario: Fetch a free public JSON API (e.g. Open-Meteo forecast or World Bank indicator) with urllib/requests, handle timeouts/429, cache response to labs/api_cache.json, and normalise to a DataFrame.
Dataset: none local — use a free no-key API; include offline fixture fallback embedded in the notebook so the lab runs without network.
Teach: urllib.request, JSON parsing, status codes, caching, schema flattening.
Exercises: (1) retry with exponential backoff; (2) flatten nested JSON to columns; (3) save labs/api_snapshot.csv.
Time ~40min Level Intermediate. Badge 🟢.
```

---

## Lab 9 — Relational Data with SQLite (Chinook)

```
Title: Relational Queries — Chinook Music DB
Track: SQL for analysts
Scenario: chinook.sqlite is a music store schema (artists, albums, tracks, invoices, customers). Answer revenue and top-customer questions with SQL, then load results into pandas.
Dataset: local datasets/chinook.sqlite | raw https://raw.githubusercontent.com/lerocha/chinook-database/master/ChinookDatabase/DataSources/Chinook_Sqlite.sqlite
Teach: sqlite3.connect, SELECT/JOIN/GROUP BY, window-lite via subqueries, pandas.read_sql.
Exercises: (1) top 5 tracks by revenue; (2) customers with no purchases; (3) monthly revenue trend.
Output: labs/top_tracks.csv optional. Time ~50min Level Beginner–Intermediate. Badge 🟢.
```

---

## Lab 10 — Web Scraping a Public Static Page

```
Title: Web Scraping a Static Public Page
Track: Data acquisition
Scenario: Scrape a simple static HTML page (e.g. a Wikipedia table or quotes.toscrape.com) with urllib + html.parser or BeautifulSoup, extract a table, and save labs/scraped.csv. Include offline HTML fixture so the lab runs without network.
Dataset: none local — ship inline HTML fixture; optional live fetch.
Teach: HTML structure, CSS selectors, polite scraping (User-Agent, delay), robots.txt note, anti-patterns warning.
Exercises: (1) parse table rows to dicts; (2) handle missing cells; (3) write CSV + row count assert.
Time ~45min Level Intermediate. Badge 🟢.
```

---

## Lab 11 — Visualisation Studio: Tips + Tickets

```
Title: Visualisation Studio
Track: Charts that communicate
Scenario: Two datasets, two briefs: tips.csv (restaurant bills — who tips more?) and tickets.csv (support tickets — SLA status mix, emails present so redact first). Produce 4 publication-quality charts.
Dataset: local datasets/tips.csv | raw https://raw.githubusercontent.com/mwaskom/seaborn-data/master/tips.csv ; local datasets/tickets.csv | raw https://raw.githubusercontent.com/vihar/datasets/master/tickets.csv
Teach: figure anatomy, appropriate chart choice, redaction of PII before plotting, subplots.
Exercises: (1) tip % distribution by day; (2) stacked status bar for tickets; (3) annotate insight on each chart.
Time ~45min Level Beginner. Badge 🟢.
```

---

## Lab 12 — HR Attrition Mini Case Study (capstone)

```
Title: HR Attrition Mini Case Study
Track: Course capstone
Scenario: People analytics shares HR-Employee-Attrition-synth.csv (2000 rows, IBM-style HR schema). Deliver an executive one-pager: attrition rate overall and by Department/Overtime, income vs attrition, and 3 actionable hypotheses. Clean, analyse, visualise, write labs/attrition_report.md.
Dataset: local datasets/HR-Employee-Attrition-synth.csv | raw https://raw.githubusercontent.com/aaubs/ds-master/main/apps/M1-attrition-streamlit/HR-Employee-Attrition-synth.csv (alt: emp_attrition.csv from IBM AIF360 repo)
Columns: Age, Attrition (Yes/No), BusinessTravel, Department, MonthlyIncome, OverTime, JobSatisfaction, … (35 cols)
Teach: end-to-end pipeline, EDA → insight → narrative, avoiding spurious correlations.
Exercises: (1) attrition by OverTime; (2) income quartiles vs attrition; (3) draft 3 hypotheses with supporting numbers.
Time ~75min Level Intermediate Capstone. Badge 🟢.
```
