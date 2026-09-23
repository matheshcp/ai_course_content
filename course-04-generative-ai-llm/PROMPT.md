# Course 4 — Lab Generation Prompts (12 labs)

Env: 🟠 Colab + Helicone (LLM API via proxy key from Colab secrets). Budget caps apply — note max tokens/calls per lab. Data: see `datasets/DATASETS.md`.

**Global preamble (prepend to every prompt):**

```
Generate a complete self-contained lab as GitHub-flavored Markdown (Jupyter-friendly).
Style contract: H1+H2 + blockquote header (Scenario / You will learn / Time / Level / Needs 🟠 Colab + Helicone);
mental-map table; numbered sections with runnable Python; dataset cell = local datasets/<file> AND raw GitHub URL fallback;
exactly 3 exercises with expected answers + <details> hints; Solutions with runnable code + printed outputs;
"What to learn next"; fixed seeds where applicable; outputs under labs/.
🟠 ENV SETUP (include as first code cell):
  - Read HELICONE_API_KEY and HELICONE_BASE_URL from Colab secrets (userdata.get).
  - OpenAI-compatible client with base_url=HELICONE_BASE_URL, api_key=HELICONE_API_KEY.
  - Print a budget guard: max N calls / M tokens for this lab; count calls in a variable and stop when exceeded.
  - Offline fallback: if secrets missing or call fails, load bundled fixture JSON from datasets/ so the notebook still runs.
Stack: openai>=1.x client, pandas, matplotlib. Keep temperature low (0–0.3) for deterministic-ish demos.
```

---

## Lab 1 — LLM Foundations: Tokens, Prompts, Temperature

```
Title: LLM Foundations — Tokens, Prompts, and Sampling
Dataset: inline strings only (no file). Budget: ≤10 API calls.
Teach: chat completions shape, system/user roles, temperature/top_p, token counting (tiktoken or len estimate), why LLMs "look ahead".
Exercises: (1) count tokens for a 200-word prompt; (2) same prompt at temp 0 vs 1 — 3 samples each, compare variance; (3) rewrite a vague prompt into a structured one.
Output: labs/prompt_examples.json. Level Beginner ~40min.
```

---

## Lab 2 — Prompt Engineering Patterns

```
Title: Prompt Engineering Patterns That Work
Dataset: reuse doc_prompt_engineering_guide.md as reading (local datasets/ | raw https://raw.githubusercontent.com/dair-ai/Prompt-Engineering-Guide/main/README.md) + inline tasks. Budget: ≤15 calls.
Teach: zero-shot, few-shot, chain-of-thought, role prompting, output format constraints (JSON schema), instruction hierarchy.
Exercises: (1) few-shot classify 10 support tickets into {login,refund,bug,billing,delivery}; (2) force valid JSON output + validate with json.loads; (3) CoT vs direct on a 3-step word problem — compare answers.
Level Beginner ~50min.
```

---

## Lab 3 — Structured Output & Function Calling

```
Title: Structured Output & Tool/Function Calling
Dataset: inline ticket snippets + optional customer_support_dataset from Course 5 URL as fixture. Budget: ≤20 calls.
Teach: tools=[{type:"function"...}], parsing tool_calls, validating with pydantic, error-retry loop.
Exercises: (1) extract {name, order_id, issue_type} from 5 messy messages; (2) force JSON-only mode without tools; (3) count parse failures across both methods.
Level Intermediate ~55min.
```

---

## Lab 4 — RAG Basics: Chunk, Embed, Retrieve

```
Title: RAG Basics — Chunk & Retrieve (FAQ corpus)
Dataset: local datasets/faq_covidbert.csv | raw https://raw.githubusercontent.com/deepset-ai/COVID-QA/master/data/faqs/faq_covidbert.csv (question, answer, …)
Teach: chunking strategies (fixed/recursive), embeddings (use free local: sentence-transformers if available, else hash-embedding fallback demo), cosine similarity, top-k retrieval, answering only from context.
Budget: embedding calls local if possible; ≤10 LLM calls for answer synthesis.
Exercises: (1) chunk 200 FAQs to ≤300-token pieces; (2) retrieve top-3 for 5 held-out questions; (3) generate answers + citation IDs; exact-match vs gold answer.
Level Intermediate ~60min.
```

---

## Lab 5 — RAG Evaluation

```
Title: RAG Evaluation — Retrieval & Answer Quality
Dataset: same faq_covidbert.csv split (80/20 train-retrieval vs held-out questions). Secondary docs: doc_huggingface_datasets.md | raw https://raw.githubusercontent.com/huggingface/datasets/main/README.md
Teach: hit@k, MRR, faithfulness check (does answer cite retrieved chunks?), answer relevancy via LLM-as-judge with rubric, common failure modes (chunk boundary, stale context).
Budget: ≤25 judge calls. 
Exercises: (1) hit@1 and hit@3 for k=1..5; (2) LLM-judge faithfulness on 10 answers; (3) improve chunk size and re-run metrics — before/after table.
Output: labs/rag_eval_report.md. Level Intermediate ~70min.
```

---

## Lab 6 — RAG Failure Modes & Fixes

```
Title: RAG Failure Modes — Break It, Then Fix It
Dataset: faq_covidbert.csv + deliberately degraded index (wrong chunk size, no overlap, dropped metadata). Budget: ≤20 calls.
Teach: counterfactual experiments — tiny chunks, huge chunks, no embeddings normalisation, stuffing entire doc; diagnose from retrieval logs; apply fixes (overlap, metadata filter, hybrid keyword+vector sketch).
Exercises: (1) produce 3 failure cases with traces; (2) apply one fix each; (3) write a postmortem-style labs/rag_postmortem.md.
Level Intermediate ~65min.
```

---

## Lab 7 — Fine-Tuning vs Prompting Decision Lab

```
Title: When to Fine-Tune — Decision Lab (no heavy training)
Dataset: small few-shot exemplar sets built inline from sms.tsv labels or FAQ pairs; doc_openai_cookbook.md as reading (local | raw https://raw.githubusercontent.com/openai/openai-cookbook/main/README.md). Budget: ≤20 calls.
Teach: cost/latency/quality tradeoffs, distillation sketch, LoRA concept (theory + tiny illustrative notebook using peft only if GPU available — else conceptual), decision checklist.
Exercises: (1) build 20-shot prompt vs 3-shot for same task — accuracy/time; (2) fill decision matrix for 3 hypothetical products; (3) write fine-tune data card for a chosen task (format only).
Level Intermediate ~50min.
```

---

## Lab 8 — Vision-Language Model Basics

```
Title: Vision-Language Models — Describe & Reason over Images
Dataset: local datasets/bus.jpg | raw https://raw.githubusercontent.com/ultralytics/yolov5/master/data/images/bus.jpg (plus 2–3 inline base64 sample images or user-uploaded). Budget: ≤12 calls.
Teach: image tokens, system prompts for VLM, grounded QA ("what is on the sign?"), limits (OCR hallucination, small text).
Exercises: (1) caption bus.jpg; (2) answer 5 pointed questions about the image; (3) compare VLM answer vs YOLOv5 detections from Course 3 — agreement table.
Level Intermediate ~45min.
```

---

## Lab 9 — Safe Generation & Guardrails

```
Title: Guardrails — Refusals, PII, Prompt Injection
Dataset: local datasets/doc_harmbench.md as reading (local | raw https://raw.githubusercontent.com/centerforaisafety/harmbench/main/README.md); inline attack strings. Budget: ≤25 calls.
Teach: system-prompt injection patterns, input/output filters, PII redaction before send, refusal calibration, logging unsafe attempts.
Exercises: (1) run 10 injection strings — record block/allow; (2) redact PII from a sample with regex before API call; (3) write a 10-line policy snippet for your app.
Level Intermediate ~55min.
```

---

## Lab 10 — Cost, Latency & Caching

```
Title: Cost & Latency Engineering for LLM Apps
Dataset: inline request logs (generate synthetic 50-row log: prompt_tokens, completion_tokens, latency_ms). Budget: ≤10 calls for live timing.
Teach: token pricing math, streaming vs batch, prompt caching concepts, semantic caching (embedding similarity threshold), batching, when to downgrade model tier.
Exercises: (1) compute $/1k requests for 3 model tiers; (2) implement semantic cache hit rate on 50 repeated queries; (3) p50/p95 latency table streaming vs non-streaming.
Output: labs/cost_report.md. Level Intermediate ~50min.
```

---

## Lab 11 — Evaluation Harness for LLM Apps

```
Title: Building an LLM Eval Harness
Dataset: 30 golden (prompt, expected) pairs derived from FAQ CSV + inline tasks. Local reading: doc_langchain.md optional. Budget: ≤30 calls for grading.
Teach: dataset format, exact/regex/heuristic metrics, LLM-as-judge with rubric + pairwise compare, regression suite in CI (pytest), seed & temperature=0 for stability.
Exercises: (1) build eval dataset JSONL; (2) run harness — pass/fail table; (3) introduce a prompt regression and catch it in the suite.
Output: labs/eval_report.json + labs/eval_harness.py. Level Intermediate ~70min.
```

---

## Lab 12 — Capstone: Mini Q&A Product over Your Docs

```
Title: Capstone — Document Q&A Product Slice
Dataset: choose corpus: faq_covidbert.csv OR all datasets/doc_*.md READMEs (huggingface, openai-cookbook, prompt-engine-guide, langchain, harmbench — see DATASETS.md URLs). Budget: ≤40 calls total.
Deliverable: end-to-end — ingest → chunk → index → retrieve → answer with citations → eval harness smoke test → cost estimate; write labs/capstone_qa_report.md.
Exercises: (1) wire retrieval+answer+citation; (2) run 10-question smoke eval; (3) document 3 known limitations + $/month estimate at 1k queries/day.
Level Capstone ~100min. 🟠 budget cap must be printed.
```
