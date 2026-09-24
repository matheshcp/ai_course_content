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

- **Single source of truth:** author each lab as one markdown lesson (body md +
  top-level ` ```python ` fences) in a staging file, then generate BOTH the
  `.ipynb` body cells and `lab-steps.html` from that source. Never hand-edit
  one artifact without regenerating the other — they must stay mirror images.
- Every lesson section must include real teaching prose, not one-line captions:
  a short **why it matters** paragraph, the code, then **What to notice** bullets
  citing actual expected numbers, plus a `> **Pitfall:**` callout where relevant.
- Lesson code and exercises must run on the **dataset file**, never on an inline
  copy of the data (load + coerce types at the top, e.g. blanks → `None`).
- Only top-level ` ```python ` fences become code cells; fences inside `<details>`
  hints and non-python fences (```text, diagrams) stay markdown.
- `<details>` hint blocks must stay complete inside a single markdown section
  (never split open/close across cells) so Jupyter and HTML both render them.
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

## 5. manifest.json (inside each lab's `bundle/`) — backend Lab-record format

Field order and meaning (match exactly; never add `api_launch` — retired):

```json
{
  "lab_id": "<lab-id-slug>",
  "title": "<lab title, from LAB_META>",
  "notebook": "<lab-id-slug>.ipynb",
  "colab_url": "https://colab.research.google.com/github/<org>/<repo>/blob/main/<course>/labs/<lab-id>/<lab-id-slug>.ipynb",
  "dataset_zip": "https://raw.githubusercontent.com/<org>/<repo>/main/<course>/labs/<lab-id>/bundle/dataset.zip",
  "images_zip": null,
  "files": ["<file1>", "<file2>"],
  "host_notes": "Hosted on <org>/<repo> (branch main).",
  "tags": ["<Tag1>", "<Tag2>", "<Tag3>"],
  "description": "<one-line lab summary>",
  "level": "<Beginner | Intermediate | Beginner–Intermediate | Intermediate Capstone, from LAB_META>",
  "overview_content": "<HTML slice from overview.html: <h2>Lab intro</h2> through the scenario blockquote, up to (excluding) the keep-going box. The Start Lab screenshot lives in the page .colab box (page chrome), not in this field.>",
  "steps_content": "<HTML slice from lab-steps.html: from <h3>Exercises (do these!)</h3> up to (excluding) <h3>Solutions</h3> — exercises only, solutions stay in the notebook>",
  "duration": "HH:MM:00 (from LAB_META time, e.g. ~45 min → 00:45:00)",
  "lab_duration": "<human label: 60 → 1 Hour, 75 → 1 Hour 15 Minutes, else N Minutes>",
  "session_timeout": "<duration_seconds + 1800>",
  "intro_walkthrough_iframe": "",
  "intro_walkthrough_hidden": false
}
```

Rules: `dataset_zip` always points at the **in-lab** `bundle/` path.
`images_zip` is `null` when the lab has no image assets. `overview_content` and
`steps_content` are extracted from the HTML files (single source of truth) —
never hand-written stubs. Timing fields derive from `LAB_META` time.

---

## 6. HTML pattern (lab-steps.html + overview.html)

Same shell for both: doctype, one `<style>` block, `<h1>` title, `.meta` box
(track/level/time/env + notebook pointer), `.colab` box (Start Lab + Open-in-Colab
link + dataset list), Learning goals, footer. No fancy styling.

- `lab-steps.html`: full lesson after the goals (headers, code, quotes, tables).
  Body headings: `##` → `<h2>`, `###` → `<h3>` (no H1 inside body). Working
  `<details>/<summary>` hints (never escaped as `&lt;details&gt;`). Shared CSS
  includes a yellow `details` rule for hint callouts.
- `overview.html`: **intro only** — track header + scenario blockquote under
  `Lab intro`, then a `.next` keep-going box (do the `.ipynb`, read `lab-steps.html`).
- **Start Lab screenshot (required on every overview):** inside the `.colab` box,
  Step 1 must show the hosted Start Lab screenshot so learners know how to launch:

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

  Keep the image URL exactly as above (hosted on `assetsvlab.codepurple.in`).
  Add CSS `.colab img { max-width: 100%; height: auto; display: block; margin: 0.3rem 0; }`
  to the shared style block. Do **not** put this screenshot on `lab-steps.html`
  (that page is the full lesson; the overview is the launch page).
- HTML must mirror the notebook: same run instructions, same exercise/follow-up
  text, same solution code. Sweep for stale wording after any notebook edit
  (`ml_lab`, `VLABS bootstrap`, upload/drag-and-drop instructions, `ORG/REPO`).
- After regenerating HTML, re-slice `steps_content` (and `overview_content` if the
  Lab intro block changed) in every `bundle/manifest.json` so the backend record
  matches the new pages.

---

## 7. Verification gates (every change, every lab)

1. **Structure**: nbformat 4, >5 cells, Colab instructions in first cell,
   `lab-steps.html` exists with ≥5 section headers; no escaped `<details>`.
2. **Smoke**: execute every code cell top-to-bottom with cwd = lab folder —
   must pass, which proves all solution + follow-up asserts.
3. **Answers**: new numbers are computed first, then baked in as asserts
   (never hand-typed blind). Exercise verifier must stay 100% pass.
4. **URL audit**: no placeholders (`ORG/REPO`), manifest ↔ notebook ↔ HTML agree
   on host/paths, `dataset.zip` URL matches the in-lab `bundle/` location,
   every `overview.html` contains the Start Lab screenshot URL
   (`assetsvlab.codepurple.in/Colab_lab/common_overview/...`).
5. **Cleanup**: delete regenerated artifacts after every run (see forbidden list §1).
6. **Fresh-dir proof** (for Cell 0 changes): run Cell 0 in an empty dir and confirm
   `NEED` files materialise from the live hosted `dataset.zip`.
7. **Content size sanity**: after a rewrite, HTML should grow substantially
   (roughly 2× for lesson prose expansions); python fence count and `assert`
   count must be unchanged from the prior baseline.
