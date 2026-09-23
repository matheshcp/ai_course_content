# Course 5 — Lab Generation Prompts (12 labs)

Env: 🟠 Colab + Helicone (where LLM needed); some labs pure-Python 🟢 tooling. Budget caps per lab. Data: see `datasets/DATASETS.md`.

**Global preamble (prepend to every prompt):**

```
Generate a complete self-contained lab as GitHub-flavored Markdown (Jupyter-friendly).
Style contract: H1+H2 + blockquote header (Scenario / You will learn / Time / Level / Needs 🟢 or 🟠);
mental-map table; numbered sections with runnable Python; dataset cell = local datasets/<file> AND raw GitHub URL fallback;
exactly 3 exercises with expected answers + <details> hints; Solutions with runnable code + printed outputs;
"What to learn next"; fixed seeds; outputs under labs/.
For 🟠 cells: Helicone env setup from Colab secrets, call-budget guard, offline fixture fallback.
Emphasise agent design patterns: tool schemas, planning loops, memory, guardrails, traces.
```

---

## Lab 1 — What Is an Agent? ReAct Loop from Scratch

```
Title: ReAct Agent Loop from Scratch
Dataset: none (inline tools: calculator, "search_local_faq" stub). Budget: ≤15 LLM calls 🟠 or scripted offline transcript fallback 🟢.
Teach: thought→action→observation cycle, stopping conditions, max-iterations guard, parsing action strings.
Exercises: (1) hand-trace a 3-step task; (2) implement loop with max_iter=5; (3) show failure when action parser is too loose.
Level Beginner ~50min.
```

---

## Lab 2 — Tool Design & Schemas

```
Title: Designing Tools for Agents
Dataset: customer_support_dataset.csv as task source (local | raw https://raw.githubusercontent.com/gakudo-ai/open-datasets/refs/heads/main/customer_support_dataset.csv)
Teach: JSON Schema for tools, naming, idempotency, error envelopes, least privilege, when NOT to expose a tool.
Exercises: (1) write schemas for lookup_ticket, refund_order, escalate; (2) validate bad calls with jsonschema; (3) classify 20 dataset rows → which tool fires.
Level Intermediate ~50min. 🟢 schema work; optional 🟠 for LLM tool-choice demo (≤10 calls).
```

---

## Lab 3 — Multi-Step Planning & Task Decomposition

```
Title: Planning — Decompose Before You Act
Dataset: ag_news_25k.csv rows as research prompts OR inline multi-step goals. Budget: ≤15 calls 🟠.
Teach: plan-then-execute vs interleaved, hierarchical plans, replan-on-failure, writing plans as checklists the agent updates.
Exercises: (1) decompose "summarise top 5 business headlines and tweet thread" into ≥6 steps; (2) run agent with plan cache; (3) inject a tool failure and show replan.
Level Intermediate ~60min.
```

---

## Lab 4 — Agent Memory (Short + Long Term)

```
Title: Agent Memory — Scratchpads & Vector Recall
Dataset: mqp.csv (medical question pairs) as long-term fact store (local | raw https://raw.githubusercontent.com/curai/medical-question-pair-dataset/master/mqp.csv); inline dialog history.
Teach: context window budget, summarising old turns, embedding-based recall, write-vs-read memory policies, forgetting curves.
Exercises: (1) implement rolling summary every 6 turns; (2) retrieve 3 relevant past facts for a new query; (3) measure token count before/after compression.
Level Intermediate ~60min. 🟠 ≤15 calls + local embeddings fallback.
```

---

## Lab 5 — Multi-Agent Orchestration

```
Title: Multi-Agent Patterns — Planner / Worker / Reviewer
Dataset: click_data.csv for a "analytics worker" tool (local | raw https://raw.githubusercontent.com/Abhishek-2307/Customer-Analytics-A-B-Testing/master/click_data.csv); inline worker prompts.
Teach: role separation, message buses (simple dict-based), when multi-agent is overkill, shared blackboard state, handoff protocols.
Exercises: (1) define 3 agent roles + allowed tools each; (2) run planner→worker→reviewer on an A/B summary task; (3) count total LLM calls — optimise to ≤ half.
Level Advanced ~75min. 🟠 ≤30 calls.
```

---

## Lab 6 — Human-in-the-Loop Approvals

```
Title: Human-in-the-Loop — Policy-Gated Actions
Dataset: inline high-risk actions (refund > $500, delete user, publish). Budget: ≤10 calls 🟠 + simulated human approve/deny.
Teach: risk tiers, approval queues, dry-run vs execute, audit log schema, time-boxed approvals.
Exercises: (1) classify 12 actions into auto/approve/deny tiers; (2) implement gate function + simulate 5 flows; (3) write audit log to labs/agent_audit.jsonl and verify schema.
Level Intermediate ~55min.
```

---

## Lab 7 — Evaluating Agents (Trajectories + Task Success)

```
Title: Agent Evaluation — Task Success & Trajectory Quality
Dataset: reuse support tickets as 10 fixed tasks with scripted tool simulators (no live LLM required for scoring skeleton). Budget: ≤20 calls if using LLM judge 🟠.
Teach: task success rate, step efficiency, tool-error rate, LLM-as-judge on traces, regression sets for agents.
Exercises: (1) build 10-task suite with expected final state; (2) score a canned transcript pass/fail; (3) add trajectory lint rules (max steps, forbidden tools).
Output: labs/agent_eval.json. Level Intermediate ~65min.
```

---

## Lab 8 — Guardrails & Sandboxing for Agents

```
Title: Agent Guardrails — Output Filters, Sandboxing, Secrets
Dataset: inline adversarial prompts + pii samples from Course 6 URLs optional. Budget: ≤15 calls 🟠.
Teach: allow-list tools, deny shell-by-default, secret redaction before logging, output moderation sketch, kill-switch.
Exercises: (1) write policy.yaml equivalent dict (allowed_tools, max_steps, spend_cap); (2) run 8 attack prompts through guard layer; (3) force a kill-switch at step 4 and verify clean stop.
Level Intermediate ~55min.
```

---

## Lab 9 — Retrieval Tools for Agents (Agentic RAG)

```
Title: Agentic RAG — When the Agent Chooses to Search
Dataset: faq_covidbert.csv or doc_*.md corpus (see Course 4 DATASETS URLs for raw fallback). Budget: ≤25 calls 🟠.
Teach: search as a tool, query rewriting, stop-when-confident, citation obligations, avoiding infinite retrieve loops.
Exercises: (1) expose search_documents tool; (2) answer 5 questions with citations; (3) count redundant searches — add a “already searched” memory guard.
Level Intermediate ~70min.
```

---

## Lab 10 — Browser/Code Execution Agents (conceptual + safe local)

```
Title: Code-Executing Agents — Safe Local Sandbox
Dataset: none — run generated code in subprocess with timeout + no-network flag. Budget: ≤15 code-exec LLM calls 🟠.
Teach: dangerous APIs blocklist, resource limits, verify-before-run, human approval for rm/network.
Exercises: (1) implement run_python_sandbox(code, timeout=5); (2) agent solves 3 data tasks (mean, plot, clean CSV); (3) attempt a forbidden call — assert blocked.
Level Advanced ~70min.
```

---

## Lab 11 — Observability: Tracing & Debugging Agents

```
Title: Tracing & Debugging Agent Runs
Dataset: synthesise 20 traces (JSONL: steps, tool, latency, tokens) inline or from earlier labs; simpsons_AB_test_mock_data.csv optional analysis prop (local | raw https://raw.githubusercontent.com/MallikaDey/SimpsonsParadox/refs/heads/main/data/simpsons_AB_test_mock_data.csv).
Teach: span model, structured logs, waterfall visualisation (matplotlib), slowest-step analysis, replay debugging.
Exercises: (1) write trace schema; (2) plot latency waterfall for worst trace; (3) identify root cause of a canned failure from trace alone.
Output: labs/trace_analysis.md. Level Intermediate ~55min. 🟢 mostly.
```

---

## Lab 12 — Capstone: Support-Ticket Resolution Agent

```
Title: Capstone — Support Resolution Agent
Dataset: customer_support_dataset.csv (local | raw URL above) + Chinook-style fake order tool stubs inline.
Deliverable: agent that classifies ticket → looks up account (stub) → proposes resolution → escalates if refund>threshold (HITL gate) → logs trace; eval on 10 held-out tickets.
Budget: ≤50 calls 🟠. Exercises: (1) wire classify+tool path; (2) run eval suite — success rate; (3) labs/capstone_agent_report.md with cost estimate + failure analysis.
Level Capstone ~120min.
```
