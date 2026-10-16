# HW1 Report — Agentic Kernel Engineering

**Project:** Application of Bayesian optimization with Gaussian process
regression to find the global minimum in the space of possible
matrix–intercalant configurations on a model graphite–lithium structure.

**Repository:** https://github.com/mariiasolovieva/agentic-kernel-engineering

**Git tag:** `hw1-submission`

---

## 1. What was done

### 1.1 Environment
- Installed and configured the AI4Science agent.
- Verified environment with `ai4science doctor`: API key, folder id,
  model, MCP tools, Lean checker, budget.
- Project initialized as a private GitHub repository.

### 1.2 Project skeleton
- `AGENTS.md` — agent contract: goal, data policy, working rules,
  prohibitions, role of the agent.
- `INDEX.md` — map of the project (where things live).
- `STATUS.md` — current state, next steps, active constraints.
- `literature/seed.md` — starting list of literature, grouped by
  subtask (SOAP, set representation, GPR/BO, kernel selection).

### 1.3 Literature analysis with the agent
- Agent produced `memory/work_plan.md`: a step-by-step plan for the BO&GPR pipeline, with justifications from the literature (PDF filename, page, quote) for each step.
- Verified every "Justification" line: no hallucinations, quotes and equations match the sources.
- Found one gap — the Bayesian optimization loop was missing — and fixed it with a second query. The added section covers: BO loop on top of GPR, EI vs UCB comparison, initialization, stopping criterion, expected outputs.

### 1.4 Article compression
- Compressed `data/papers/shuaib2024_multikernel_amorphous.pdf` into
  three levels:
  - `memory/L1.md` — section-by-section summary
  - `memory/L2.md` — 5–10 key claims
  - `memory/L3.md` — one sentence
- At least 3 claims from L2 verified with page number and quote.
- Later added Results and Discussion sections to L1 (missing in the first pass because the prompt only listed part of the sections).

### 1.5 Ontology and concept graph
- Wrote `ontology.yaml` manually: 14 classes, 12 relations, 24 instances.
- Asked the agent to generate `graph.mmd` from the ontology.
- Verified: class list, relation list, instance list match the ontology; no cycles; every class has at least one instance.

### 1.6 Local context queries
- Documented in `results/context_queries.md`:
  - average SOAP kernel formula
  - EI vs UCB comparison
  - SOAP power spectrum
  - mean/variance pooling
  - kernel composition
- Each query has prompt, result, verification (page and quote).

### 1.7 Journal
- `diary.md` — every request, result, and verification checkpoint,
  appended after each task (old entries unchanged).

---

## 2. How it was verified

- **Plan.** Every "Justification" line in `memory/work_plan.md` was
  opened in the original PDF and checked: pages, quotes, formulas.
- **Compression.** Every quote in `memory/L2.md` was checked against
  the original. No hallucinations found; one formatting issue
  (`MLIPA(1)` vs `MLIP^A(1)`) noted.
- **Ontology.** Three rules from lecture 3:
  1. class ≠ instance — verified;
  2. no cycles — verified;
  3. every class has an instance — verified.
- **Graph.** `graph.mmd` compared against `ontology.yaml`:
  classes, relations, and instances all match.
- **Context queries.** Each of the two checked query's result was opened in the PDF and checked against the cited page and quote.

---

## 3. What did not work / what was fixed

- **First plan missed the BO section.** The agent produced a good plan for SOAP pooling and kernel selection but omitted the Bayesian optimization loop. Fixed with a second query; the new section is justified with `shahriari2016_bo_basics.pdf`.
- **L1 missed Results and Discussion.** The initial prompt listed only part of the article sections, so the agent followed the list strictly. Fixed by explicitly asking to add the missing sections.
- **L3 too long.** The one-sentence summary included too many numerical details. Noted for revision; optional.
- **Formatting in L2:** `MLIPA(1)` instead of `MLIP^A(1)`.

---

## 4. Impact of agent assistance

- **Time saved:** literature review and compression, which would normally take days of manual reading, were done in one hour. The agent produced a structured plan with citations on the first attempt (with one gap).
- **What worked well:**
  - literature extraction with page and quote;
  - structured plan generation;
  - code and file drafting from the plan.
- **What was harder than expected:**
  - the agent needs an explicit list of sections; otherwise it silently skips parts;
  - verification by the researcher is essential — the agent produced no hallucinations in this project, but the format of equations and indices needed correction;
  - keeping the context window under control (approve mode, `-no-lean`, `/compact`).

---

## 5. Artifacts delivered

| Artifact | Path |
|---|---|
| Agent contract | `AGENTS.md` |
| Project map | `INDEX.md` |
| Current state | `STATUS.md` |
| Journal | `diary.md` |
| Work plan | `memory/work_plan.md` |
| Article compression | `memory/L1.md`, `L2.md`, `L3.md` |
| Ontology | `ontology.yaml` |
| Concept graph | `graph.mmd` |
| Local context queries | `results/context_queries.md` |
| Seed literature | `literature/seed.md` |
| Report | `report.md` |

---

## 6. Git

- Repository: https://github.com/mariiasolovieva/agentic-kernel-engineering
- Tag: `hw1-submission`