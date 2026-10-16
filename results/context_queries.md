# Local context queries

Examples of queries to the local library (`data/papers/`, `memory/`, `results/`).
Each entry: prompt, result, verification.

---

## Query 1: average SOAP kernel formula

**Prompt:**
Find the formula for the average SOAP kernel in `shuaib2024_multikernel_amorphous.pdf`.

**Result:**
Eq. 9, p. 5:

    K_SOAP(A, B) = (1 / (N_A * N_B)) * sum_ij C_ij(A, B)
                 = [ l * <P(Q^A)>^T * <P(Q^B)> + c_0 ]^d

where `C_ij(A, B) = k(Q_i^A, Q_j^B)` is the pairwise SOAP similarity (Eq. 8),
`N_A`, `N_B` are atom counts, `P(Q_i^A)` is the SOAP power-spectrum vector,
and `l`, `c_0`, `d` are polynomial-kernel parameters (`d = 4` in this work).
The computation is cheap because only the average SOAP fingerprint
`(1 / N_A) * sum_i P(Q_i^A)` is retained for each structure.

**Verification:**
Opened p. 5 of the PDF. Formula matches Eq. 9. Cross-checked with
`memory/L1.md` (Models / Methodology section) — same formula, consistent.

---

## Query 2: EI vs UCB comparison

**Prompt:**
Find in `shahriari2016_bo_basics.pdf` the comparison of EI and UCB.
Write down pros and cons for each acquisition function and choose
a more appropriate one for our work plan.

**Result:**

EI (Expected Improvement) — Shahriari, p. 14, Eq. 44:
- Pros: incorporates the *amount* of improvement, not just its probability;
  works well with the adaptive target `tau` = best observed value;
  convergence rates proven.
- Cons: no provable regret bounds; requires choosing the target `tau`.

UCB (Upper Confidence Bound) — Shahriari, p. 14, Eq. 45:
- Pros: provable cumulative regret bounds (GP-UCB);
  theoretically motivated guidelines for setting/scheduling `beta_n`.
- Cons: performance depends on tuning `beta_n`;
  no explicit improvement target; less intuitive.

**Choice for the work plan:** EI as the primary acquisition function.
UCB kept as a valid alternative with provable regret bounds;
both are run and compared empirically.

**Verification:**
Opened pp. 13–15. Quotes match. Paper explicitly states
"no single acquisition strategy provides better performance over all
problem instances" (p. 15) — this justifies running both.
Added the pros/cons block to `memory/work_plan.md` §2.5.2.

---

## Query 3: SOAP power spectrum and polynomial kernel

**Prompt:**
Find in `bartok2013_soap.pdf` the definition of the SOAP power spectrum
and the formula for the polynomial kernel. Give page number and quote.


## Query 4: mean and variance pooling over node features

**Prompt:**
Find in `corso2020_stats.pdf` the formulas for mean pooling and variance
pooling over node features. Give page number and quote.


---

## Query 5: composition of base kernels in kernel search

**Prompt:**
Find in `duvenaud13_composition_kernel.pdf` how base kernels are composed
into sums and products during kernel search. Give page number and quote.

