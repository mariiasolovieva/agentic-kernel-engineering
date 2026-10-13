# AGENTS.md — Application of an AI agent in the early stages of research on a new scientific topic

Project name: Application of Bayesian optimization with Gaussian process regression to find the global minimum in the space of possible matrix–intercalant configurations on a model graphite–lithium structure

General task: Learn to quickly find the lowest-energy configurations for n atoms intercalated into the matrix

## Project goal
Understand how to encode the local environment of Li atoms in graphite anodes using SOAP and statistical pooling into a fixed-dimension vector suitable for Bayesian optimization (BO) and Gaussian process regression (GPR). Build a code prototype that accepts structures with different numbers of intercalated atoms, trains GPR, and allows comparing the efficiency of different kernels. No cluster calculations are performed; only literature analysis, drafting of a work plan and tests, and analysis of provided results.

## Data and materials
- `data/papers/` — PDF articles, read-only.
- `data/structures/` — example structures from the training set (in POSCAR format). All input data (POSCAR files, .csv training sets) is provided by me.
  Do not generate synthetic structures, do not create toy datasets, do not fabricate any data. If a step requires data that is not in
  data/structures/ yet, ask me to provide it.
- `results/` — tables, plots, prototype artifacts.
- `src/` — prototype code.

## How to work
- Code goes in `src/`, run locally before reporting results.
- Every conclusion must be accompanied by a way to verify it.
- For statements from literature, indicate page and a short quote.
- Do not run DFT, MD, MLIP, or other external calculations.
- Summaries and notes for future sessions — in `memory/`.
- Your role: literature analysis, code writing, verification of results.
  No data generation, no running cluster calculations.

## Memory between sessions
- At the start of each session, read `STATUS.md`.
- After each block of work, update `STATUS.md`: what is done, what is next, active constraints.

## Journal
- After each task — an entry in `diary.md`: date, request, what was done,
  result, verification point, verification outcome. Do not modify old entries.

## Prohibited
- Delete or overwrite files in `data/`.
- Read and output the contents of `.env` and other files with keys.
- Execute `git push`, `rm -rf`, `sudo` without confirmation.
- Run calculations on a cluster or HPC.
