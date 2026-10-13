# STATUS

## Where we are
- Environment set up, agent installed, `ai4science doctor` passed.
- Project skeleton created: `AGENTS.md`, `literature/seed.md`, `INDEX.md`.
- Agent generated a work plan based on literature; reviewed and verified.
- Article compressed: `memory/L1.md`, `L2.md`, `L3.md`.
- 3 claims from L2 verified against the original (pages and quotes).
- `diary.md` contains all queries and verification checkpoints.

## Next
- Create `ontology.yaml` and `graph.md` (classes, relations, instances).
- Add `results/context_queries.md` with examples of local context queries.
- Write `report.md`: what was done, how verified, what did not work.
- Add git tag `hw1-submission`, push, share access with lecturer.

## Active constraints
- No cluster, DFT, MD, or MLIP calculations. Local Python only.
- `data/papers/` is read-only.
- All input data (POSCAR, .csv) is provided by the researcher — do not generate synthetic data.
- Agent role: literature analysis, code writing, verification. Not data generation.
- Do not read or output `.env` or any file with keys.
