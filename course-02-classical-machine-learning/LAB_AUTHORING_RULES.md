# Lab Authoring Rules (Course 2 — Classical Machine Learning)

These rules extend Course 1's rules for ML-specific labs. Follow them exactly
when generating future ML labs so every lab is Colab direct-open ready and verifiable.

Repo host: `matheshcp/ai_course_content`, branch `main`.
Course dir: `course-02-classical-machine-learning`.

---

## 1. Per-lab folder layout

```
labs/lab-NN-<slug>/
  lab-NN-<slug>.ipynb   # lesson + executable lab (Colab-ready, output-free)
  lab-steps.html        # full lesson (plain HTML: header + content + code)
  overview.html         # intro only (meta + colab box + goals + lab intro + keep-going)
  <dataset files>       # source data the code reads (copied from datasets/)
  bundle/
    dataset.zip         # ALL data files above, at zip root
    manifest.json       # lab record (see §5)
```

Forbidden: `lesson.html`, `README.html`, `*.md` sources, test scripts,
generated outputs (`*.png`, `*_predictions.csv`, `*_scores.csv`, `*_segments.csv`,
`*_report.md`, `churn_model_card.md`). Clean after every test run.

---

## 2. Hosting model — 1 source file, direct open

Same as Course 1: one hosted notebook per lab, opened via GitHub opener URL.
Student flow: Start Lab → hosted URL → Cell 0 → Run all. No per-student Drive API.

---

## 3. Notebook structure (in order)

1. **Header markdown cell**: `# Lab NN — Title`, track · level · time · env,
   Scenario, learning goals, Datasets, How-to-run (Start Lab + Open in Colab link).
2. **Setup markdown cell**: what Cell 0 does.
3. **Cell 0 code — dataset first** (wget + unzip, NEED file list).
4. **Lesson cells**: markdown teaching + code cells using cwd-relative filenames.
5. **Exercises**: task + `*Expected: …*` + `**Follow-up:** …` + Hint in `<details>`.
6. **One solutions code cell**: `# --- Solution N ---` + `# --- Follow-up N ---` with asserts.

ML-specific content rules:

- **Single source of truth:** author each lab as one markdown lesson, then generate
  both `.ipynb` and `lab-steps.html` from it.
- Every lesson section: **why it matters** → code → **What to notice** → `> **Pitfall:**`.
- Code must run on the **dataset file**, never inline copies.
- Use `random_state=42` everywhere.
- Use `matplotlib.use("Agg")` before plotting.
- sklearn 1.8: use `df.map()` not `df.applymap()`; `pd.qcut(..., observed=True)`.
- Follow-up asserts use exact values proven by execution.
- Follow-up code is self-contained: recompute from base vars, import what it needs.
- Ship output-free: strip all cell outputs before hosting.

---

## 4. Cell 0 template (wget + unzip)

Same as Course 1 §4. `DATASET_ZIP_URL` points at the in-lab `bundle/` path.

---

## 5. manifest.json (inside each lab's `bundle/`)

Same 18-field schema as Course 1 §5. `dataset_zip` points at in-lab `bundle/`.
`images_zip` is `null`. No `api_launch`.

---

## 6. HTML pattern (lab-steps.html + overview.html)

Same shell as Course 1 §6. Key points:

- `lab-steps.html`: full lesson, `##` → `<h2>`, `###` → `<h3>`, working `<details>`.
- `overview.html`: intro only + **Start Lab screenshot** in `.colab` box:
  ```html
  <p style="margin:0.2rem 0 0.5rem"><strong>Step 1 — Start the lab:</strong>
  click <strong>Start Lab</strong>, then open the notebook with the button shown below.</p>
  <p style="margin:0 0 0.6rem"><img
    src="https://assetsvlab.codepurple.in/Colab_lab/common_overview/WhatsApp%20Image%202026-09-24%20at%2012.20.26%20PM.jpeg"
    alt="Start Lab — click Start Lab then Open in Colab"
    style="max-width:100%; height:auto; border:1px solid #ccc; border-radius:4px; display:block;"/>
  <span style="font-family:system-ui,sans-serif; font-size:0.82rem; color:#555">
  Screenshot: Start Lab → Open in Colab (hosted asset).</span></p>
  ```
- CSS: `.colab img { max-width: 100%; height: auto; display: block; margin: 0.3rem 0; }`
- After regenerating HTML, re-slice `steps_content` and `overview_content` in manifests.

---

## 7. Verification gates (every change, every lab)

1. **Structure**: nbformat 4, >5 cells, Colab instructions in first cell,
   `lab-steps.html` with ≥5 section headers, no escaped `<details>`.
2. **Smoke**: stage datasets into lab folder, then execute all code cells in
   sequence (Cell 0 sees files, skips download). Must pass with all asserts.
3. **Answers**: exercise expected values computed from actual data, baked as asserts.
4. **URL audit**: no placeholders, manifest ↔ notebook ↔ HTML agree,
   `dataset.zip` URL matches in-lab `bundle/`, every `overview.html` has
   the Start Lab screenshot URL.
5. **Cleanup**: delete regenerated artifacts after every run.
6. **Fresh-dir proof** (for Cell 0 changes): run Cell 0 in an empty dir.
7. **Content size sanity**: HTML should be substantial; python fence count and
   `assert` count unchanged from baseline after prose-only edits.
