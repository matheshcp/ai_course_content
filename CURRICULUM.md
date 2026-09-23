# AI Lab Curriculum — 6 Courses / 74 Labs

Copy-paste generation prompts live in each course's `PROMPT.md`. Datasets and schemas in each course's `datasets/DATASETS.md`.

## Structure

```
course-01-foundations-python-math-data/     (12 labs) 🟢 Colab only
course-02-classical-machine-learning/       (13 labs) 🟢 Colab only
course-03-deep-learning-computer-vision-nlp/(13 labs) 🟢 Colab only
course-04-generative-ai-llm/                (12 labs) 🟠 Colab + Helicone
course-05-agentic-ai/                       (12 labs) 🟠 Colab + Helicone
course-06-mlops-evaluation-responsible-ai/  (12 labs) 🟠 mixed / 🟢
```

Each course folder:
- `datasets/` — local files + `DATASETS.md` (raw GitHub URL fallback for Colab)
- `labs/` — generated labs as Markdown notebooks
- `PROMPT.md` — enhanced copy-paste prompts for generating all labs in the course

## Style contract (all labs)

Modeled on `course-01-foundations-python-math-data/labs/lab-01-python-refresher-for-data-people.md`:

1. Header: `# Title` / `## Track subtitle` then a `>` block with **Scenario**, **You will learn**, **Time / Level / Needs** (env badge 🟢/🟠).
2. Mental-map table early (maps prior knowledge → new concept).
3. Sections with `###` and runnable `python` code blocks; brief "why" callouts (`>`).
4. Dataset access: prefer local path `datasets/<file>`; include a Colab download cell with raw GitHub URL fallback.
5. 3 numbered **Exercises** with expected answers + `<details>` hints; then **Solutions** with runnable code and printed expected output.
6. Closing **What to learn next** bullet list.
7. Fixed seed (`random_state=42` / `np.random.seed(42)`) wherever sampling occurs.
8. Output artifacts written under `labs/` when producing files (e.g. `labs/cleaned_sales.csv`).
9. 🟠 labs must include env setup cell (Helicone base URL/key from Colab secrets), budget cap note, and graceful offline fallback (cached fixtures).

## Usage

Open a course `PROMPT.md`, copy one lab's prompt block, paste into your generator, save the result as `labs/lab-NN-<slug>.md` in the matching course folder.

## Lab index

| Course | Labs | Env |
|---|---|---|
| 1 Foundations | 12 | 🟢 |
| 2 Classical ML | 13 | 🟢 |
| 3 DL/CV/NLP | 13 | 🟢 |
| 4 GenAI/LLM | 12 | 🟠 |
| 5 Agentic | 12 | 🟠 |
| 6 MLOps/Eval/Responsible | 12 | 🟠/🟢 |
| **Total** | **74** | |
