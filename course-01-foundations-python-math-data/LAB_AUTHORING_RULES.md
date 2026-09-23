# Lab Authoring Rules (Course 1 — and template for future courses)

These rules were settled while building the 12 Course-1 labs. Follow them exactly
when generating future labs so every lab is Colab direct-open ready and verifiable.

Repo host: `matheshcp/ai_course_content`, branch `main`.
Course dir example: `course-01-foundations-python-math-data`.

---

## 1. Per-lab folder layout

```
labs/lab-NN-<slug>/
  lab-NN-<slug>.ipynb   # lesson + executable lab (Colab-ready, output-free)
  lab-steps.html        # full lesson (plain HTML: header + content + code)
  overview.html         # intro only (meta + colab box + goals + lab intro + keep-going)
  <dataset files>       # source data / fixtures the code reads (csv, tsv, sqlite, json, html)
  bundle/               # Admin upload package (mirrors what is hosted)
    dataset.zip         # ALL data files above, at zip root (no subfolder)
    manifest.json       # lab record (see §5)
```

Forbidden in lab folders: `lesson.html`, `README.html`, `*.md` sources, test scripts,
and generated outputs (`*_clean.csv`, `*.png` charts, `*_findings.md`, `*_report.md`,
`api_cache.json`, `api_snapshot.csv`, `scraped.csv`, `top_tracks.csv`, `chart*.png`,
`attrition_*.png`). Clean these after every test run.

---

## 2. Hosting model — 1 source file, direct open

- One hosted notebook per lab, opened via the GitHub opener URL
  (`colab.research.google.com/github/<org>/<repo>/blob/main/...`).
- Student flow: **Start Lab → hosted URL opens in the student's own Google account**
  (Colab auto-saves a copy to their Drive) → run Cell 0 → Run all.
- No per-student Drive API `create`, no OAuth scope, no shared account.
- Repo must be public; notebook + `bundle/` must be pushed to `main`.

---

## 3. Notebook structure (in order)

1. **Header markdown cell**: `# Lab NN — Title`, track · level · time · env badges,
   Scenario pointer, numbered learning goals, Datasets list, How-to-run steps
   (Start Lab + direct `[Open in Colab](<colab_url>)` link).
2. **Setup markdown cell**: what Cell 0 does (wget + unzip, NEED file list).
3. **Cell 0 code — dataset first** (template in §4). Must run first, top-to-bottom safe.
4. **Lesson cells**: markdown teaching + code cells using cwd-relative bare filenames.
5. **Exercises**: markdown task + `*Expected: …*` + `**Follow-up:** …` (with stated check
   value) + Hint inside `<details>` (hint code fences stay markdown, never code cells).
6. **One solutions code cell** at the end: `# --- Solution N ---` blocks followed by
   `# --- Follow-up N ---` blocks that print the value **and `assert` it**
   (self-verifying notebooks — the smoke run proves every answer).

Content rules:

- Lesson code and exercises must run on the **dataset file**, never on an inline
  copy of the data (load + coerce types at the top, e.g. blanks → `None`).
- Only top-level ` ```python ` fences become code cells; fences inside `<details>`
  hints and non-python fences (```text, diagrams) stay markdown.
- Follow-up asserts must use exact values or tolerances proven by execution
  (compute-then-bake; beware float rounding, e.g. derive widths from rounded bounds).
- Follow-up code must be self-contained: recompute from base vars
  (`df`, `temps`, `sales`, `records`, …) and import what it needs; never rely on
  intermediate variable names from other cells. Reopen closed DB connections.
- Network-dependent labs (live APIs): follow-ups assert structure (keys, lengths),
  never live values.
- Ship output-free: strip all cell outputs before hosting.

---

## 4. Cell 0 template (wget + unzip)

```python
# Cell 0 — dataset first: wget dataset.zip + unzip (run this cell first).
import os, shutil, subprocess, urllib.request, zipfile

LAB_ID = "<lab-id-slug>"
DATASET_ZIP_URL = "https://raw.githubusercontent.com/<org>/<repo>/main/<course>/labs/<lab-id>/bundle/dataset.zip"
NEED = ["<file1>", "<file2>"]  # unzipped files used by the code below

def _have_files():
    return all(os.path.exists(f) for f in NEED)

def _wget_zip(url, dest):
    # shell equivalent: !wget -q <url> -O dataset.zip
    if shutil.which("wget"):
        subprocess.run(["wget", "-q", url, "-O", dest], check=True)
    else:  # plain Python without wget: stdlib fallback
        urllib.request.urlretrieve(url, dest)

if _have_files():
    print("dataset ready:", ", ".join(NEED))
else:
    _wget_zip(DATASET_ZIP_URL, "dataset.zip")
    # shell equivalent: !unzip -o -q dataset.zip
    with zipfile.ZipFile("dataset.zip") as z:
        z.extractall(".")
    print("downloaded + unzipped dataset.zip ->", ", ".join(NEED))
```

Rules: unzip to cwd (Colab cwd is `/content`), no `chdir`, no `/content/ml_lab`
subdir — lesson cells use relative paths. No `!wget`/`!unzip` magic (breaks plain
`exec` verification); the real `wget` binary is used when present.

---

## 5. manifest.json (inside each lab's `bundle/`)

```json
{
  "lab_id": "<lab-id-slug>",
  "notebook": "<lab-id-slug>.ipynb",
  "colab_url": "https://colab.research.google.com/github/<org>/<repo>/blob/main/<course>/labs/<lab-id>/<lab-id-slug>.ipynb",
  "dataset_zip": "https://raw.githubusercontent.com/<org>/<repo>/main/<course>/labs/<lab-id>/bundle/dataset.zip",
  "images_zip": null,
  "files": ["<file1>", "<file2>"],
  "host_notes": "Hosting + asset notes."
}
```

Rules: `dataset_zip` always points at the **in-lab** `bundle/` path.
`images_zip` is `null` when the lab has no image assets. No other top-level
fields (never add `api_launch` — retired).

---

## 6. HTML pattern (lab-steps.html + overview.html)

Same shell for both: doctype, one `<style>` block, `<h1>` title, `.meta` box
(track/level/time/env + notebook pointer), `.colab` box (Start Lab + Open-in-Colab
link + dataset list), Learning goals, footer. No fancy styling.

- `lab-steps.html`: full lesson after the goals (headers, code, quotes, tables).
- `overview.html`: **intro only** — track header + scenario blockquote under
  `Lab intro`, then a `.next` keep-going box (do the `.ipynb`, read `lab-steps.html`).
- HTML must mirror the notebook: same run instructions, same exercise/follow-up text,
  same solution code. Sweep for stale wording after any notebook edit
  (`ml_lab`, `VLABS bootstrap`, upload/drag-and-drop instructions, `ORG/REPO`).

---

## 7. Verification gates (every change, every lab)

1. **Structure**: nbformat 4, >5 cells, Colab instructions in first cell,
   `lab-steps.html` exists with ≥5 section headers.
2. **Smoke**: execute every code cell top-to-bottom with cwd = lab folder —
   must pass, which proves all solution + follow-up asserts.
3. **Answers**: new numbers are computed first, then baked in as asserts
   (never hand-typed blind).
4. **URL audit**: no placeholders (`ORG/REPO`), manifest ↔ notebook ↔ HTML agree
   on host/paths, `dataset.zip` URL matches the in-lab `bundle/` location.
5. **Cleanup**: delete regenerated artifacts after every run (see forbidden list §1).
6. **Fresh-dir proof** (for Cell 0 changes): run Cell 0 in an empty dir and confirm
   `NEED` files materialise from the live hosted `dataset.zip`.
