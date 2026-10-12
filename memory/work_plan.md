# Work Plan — BO & GPR for Li intercalation in graphite

Goal: encode local Li environments via SOAP + statistical pooling into a
fixed-length vector, train GPR, compare kernels locally (no cluster).

## 0. Environment setup (prerequisite)
- Create `venv` in project, install `numpy scipy scikit-learn dscribe gpytorch matplotlib`.
- Verify: `python -c "import dscribe, sklearn, gpytorch"` runs.
- Note: `data/structures/` is currently empty → prototype must generate
  synthetic graphite–Li POSCAR structures (stage 1) or read provided ones.

---

## 1. Fixed-length feature vector from SOAP for variable atom counts

**Mechanism (the core answer):** compute a per-atom SOAP power-spectrum
vector `P(env_i)` for every Li atom (and optionally C atoms in the cutoff),
then **average over atoms** to get one fixed-length fingerprint per structure:

    x(A) = (1/N_A) * Σ_i P(env_i^A)

This is the "average SOAP fingerprint" — the vector is fixed-length
regardless of how many atoms the structure contains.

**Justification (Shuaib et al., J. Am. Ceram. Soc. 2025, p.5, Eq. 9):**
> "we resort to the average kernel as implemented in DScribe to measure the
> average similarity between the two structures (configurations) as follows:
> K_SOAP(A,B) = 1/(N_A·N_B) Σ_ij C_ij(A,B) = [ l (1/N_A Σ_i P(env_i^A))^T ·
> (1/N_B Σ_j P(env_j^B)) + c0 ]^d"
> "In this implementation, the computation of K_SOAP is achieved at low cost
> by retaining the average SOAP fingerprint across all atom environments in
> both structures."

**Why fixed length is needed (Bartók et al., PRB 2013, p.2):**
> "the number of neighbours that contribute to the energetics of an atom has
> to be allowed to vary, with the symmetry invariant descriptors remaining
> continuous and differentiable. Even though it is possible to allow the
> dimensionality M to change with the number of neighbours, for the purpose
> of function fitting it is more practical that M remains the same."

**Implementation steps:**
1. Parse POSCAR → ASE `Atoms`.
2. `dscribe.descriptors.SOAP(species=['Li','C'], rcut, nmax, lmax)` → per-atom
   vectors.
3. Pool: mean (primary), also test max and sum (see §2) → fixed-length `x`.
4. Verify: (a) two structures with different N give same vector length;
   (b) permuting atom order in POSCAR gives identical pooled vector
   (permutation invariance); (c) self-similarity of a structure with itself = 1
   after kernel normalization (Shuaib p.5, Eq. 7).

---

## 2. Kernel selection experiments (local)

**Candidate kernels** (all on the pooled fixed-length vector `x`):
- RBF / squared-exponential (SE) — baseline (Bartók p.3, Eq. 4).
- Matérn 5/2, Matérn 3/2 — smoother/rougher alternatives.
- Linear / dot-product (Bartók p.2-3, Eq. 2).
- Rational quadratic (multi-scale; Duvenaud p.2 lists RQ as base kernel).
- **Compositional / hybrid**: sum and product of base kernels
  (Duvenaud p.2: "Positive semidefinite kernels are closed under addition and
  multiplication"); and the **hybrid SOAP+Coulomb average kernel**
  K_hybrid = (1-δ)·K_SOAP + δ·K_Coulomb (Shuaib p.6, Eq. 11) — directly
  motivated by the amorphous/disordered setting.

**Justification for comparing multiple kernels (Duvenaud et al., ICML 2013, p.1):**
> "choosing the structural form of the kernel in nonparametric regression
> remains a black art... the kernel determines the pattern of inductive
> generalization."

**Protocol:**
- Train GPR on pooled vectors; targets = provided energies (or synthetic
  surrogate energies for the prototype).
- Compare via: (a) 5-fold cross-validated RMSE/MAE; (b) test-set log marginal
  likelihood (Seeger p.10: "The marginal likelihood P(y|θ) is Gaussian and can
  be computed easily"); (c) predictive log-likelihood (calibration).
- Hyperparameters (lengthscale, variance, noise, δ) via marginal-likelihood
  maximization (Seeger p.10) with random restarts.
- Verify: reproduce a known result — e.g. on a synthetic 1-D function, RBF
  recovers the true function; cross-check kernel matrix is positive definite.

---

## 2.5 Bayesian optimization loop

Goal: use the trained GPR surrogate to sequentially propose new Li–graphite
configurations that minimize the (unknown) energy, without a cluster.

### 2.5.1 Constructing the BO loop on top of the GPR surrogate

The loop is the standard sequential BO procedure: at each iteration, select
the next query point by maximizing an acquisition function, evaluate the
objective, augment the data, and refit the surrogate.

**Justification (Shahriari et al., Proc. IEEE 2016, p.3, Algorithm 1):**
> "select new x_{n+1} by optimizing acquisition function α: x_{n+1} = arg max_x α(x; D_n); query objective function to obtain y_{n+1}; augment data D_{n+1} = {D_n, (x_{n+1}, y_{n+1})}; update statistical model."

The surrogate is the GPR trained in §2; the "query" is evaluating the energy
of a candidate configuration (locally, from the provided dataset / a
surrogate energy model — see §4). The acquisition function is cheap to
evaluate relative to the black box (Shahriari p.3: "it is critical that the
acquisition functions be cheap to evaluate or approximate: cheap in relation
to the expense of evaluating the black box f").

### 2.5.2 Acquisition function choice (EI vs UCB vs PI)

We compare at least two acquisition functions. The paper gives the analytic
forms and a clear qualitative comparison:

- **PI** (probability of improvement), Shahriari p.13, Eq. 42:
  > "probability of improvement (PI) [91], measures the probability that a point x leads to an improvement upon τ... α_PI(x; D_n) := P[v > τ] = Φ((μ_n(x) − τ)/σ_n(x))"
  and its weakness (p.13): "in general the heuristic used for an unknown target τ causes PI to exploit quite aggressively [81]."

- **EI** (expected improvement), Shahriari p.14, Eq. 44:
  > "One could instead measure the EI [115] which incorporates the amount of improvement... α_EI(x; D_n) := E[I(x; v, θ)] = (μ_n(x) − τ) Φ((μ_n(x) − τ)/σ_n(x)) + σ_n(x) φ((μ_n(x) − τ)/σ_n(x))"
  and the key practical advantage (p.14): "whereas for PI this heuristic can lead to an overly greedy optimization [81], it works reasonably with EI in practice [143]."

- **UCB** (upper confidence bound), Shahriari p.14, Eq. 45:
  > "α_UCB(x; D_n) := μ_n(x) + β_n σ_n(x)"
  with "theoretically motivated guidelines for setting and scheduling the hyperparameter β_n to achieve optimal regret [146]."

**Recommendation:** use **EI** as the primary acquisition function, because
the paper explicitly states it works well with the adaptive target
τ = best observed value (p.14: "in practice τ is adaptively set to the best
observed value y^+ = max_{i=1:n} y_i"), whereas PI is overly greedy. UCB is a
valid alternative with provable regret bounds; we will run EI and UCB and
compare their convergence (see §2.5.5). Note: the paper also warns that "no
single acquisition strategy provides better performance over all problem
instances" (p.15), so the comparison is empirical.

### 2.5.3 Initialization of the BO loop

The Shahriari paper does **not** give explicit guidance on the number of
initial points or a specific space-filling scheme (e.g., Latin hypercube).
This claim is **not supported** by the papers in `data/papers/`; we state it
explicitly rather than guess a citation.

Proposed default (to be validated empirically, not from the paper): seed the
loop with a small random/space-filling initial design (e.g., 5–10
configurations) drawn from the provided dataset, so the GPR has enough data
to fit a meaningful posterior before acquisition-driven queries begin. The
paper supports the general principle that the surrogate must be updated
sequentially (Algorithm 1, p.3) and that hyperparameters are fit from data
(p.16: "fit the hyperparameter to observed data using a point estimate θ̂_ML
or θ̂_MAP, corresponding to type II maximum likelihood or maximum a posteriori
estimates").

### 2.5.4 Stopping criterion (cluster-free)

For our local setup, the natural stopping criterion is a **predetermined
evaluation budget** — a fixed number of BO iterations/queries — rather than a
convergence threshold on the acquisition value.

**Justification (Shahriari et al., p.9):**
> "for other tasks such as model selection, we typically have a predetermined evaluation budget for optimization and only the performance of the final recommended model should be assessed by the loss function."

Since each "query" here is a local evaluation (not an expensive cluster
calculation), the budget is set by the size of the provided dataset and the
number of candidate configurations we are willing to evaluate. We will also
record the simple regret / best-found value at each iteration to show
convergence behavior (Shahriari p.9, Eq. 21: "the experiment is then judged
via the simple regret, r_n(w) = f*_w − f_w(x̂_a)").

### 2.5.5 Outputs of the BO loop

1. **Table of queried configurations** — for each BO iteration: the pooled
   SOAP feature vector (or its index), the predicted mean μ_n(x) and
   predictive variance σ_n²(x) from the GPR, and the acquisition value.
   Justification: the GPR posterior provides both mean and variance at any
   point (Shahriari p.14, Eq. 44-45 use μ_n and σ_n), and the acquisition
   value is the quantity being maximized (p.3, Algorithm 1).
2. **Convergence plot** — best-found energy (simple regret) vs number of BO
   iterations, for EI and UCB. Justification: the paper frames BO as
   sequential improvement and evaluates via regret (p.9, Eq. 21).
3. **Best configuration artifact** — the configuration with the lowest
   predicted mean energy at the end of the budget, saved as a POSCAR file
   plus its predicted mean/variance. Justification: "after N queries, the
   algorithm makes a final recommendation x̂_N, which represents the
   algorithm's best estimate of the optimizer" (Shahriari p.2).

---

## 3. Output tables and plots

**Tables (`results/`):**
- `kernels_comparison.csv` — per kernel: CV-RMSE, CV-MAE, test log-marginal
  likelihood, #hyperparams, fit time.
- `pooling_comparison.csv` — mean vs max vs sum pooling, same metrics.
- `hyperparams.csv` — optimized lengthscale/variance/noise/δ per kernel.
- `soap_params.csv` — rcut, nmax, lmax, vector length, #structures.

**Plots (`results/`):**
- `soap_pooling_invariance.png` — pooled vector length vs N_atoms (flat line)
  + permutation-invariance check (identical vectors for permuted POSCAR).
- `kernel_cv_rmse.png` — bar chart of CV-RMSE per kernel (with error bars).
- `gpr_fit.png` — GPR mean ± 2σ vs true energy on a held-out slice.
- `bo_convergence.png` — best-found energy vs #BO iterations for the top kernel
  (EI acquisition; Shahriari p.3: EI/UCB/TS "trade off exploration and
  exploitation").
- `kernel_matrix.png` — heatmap of the average-SOAP similarity matrix
  (Shuaib p.5-6: "average SOAP similarity matrix of dimension N×N").

---

## 4. Verification without a cluster

Every step has a local, deterministic check:
1. **Descriptor/pooling**: unit tests — vector length constant across N;
   permutation invariance; self-similarity = 1 (Shuaib Eq. 7).
2. **Kernel correctness**: kernel matrix is symmetric positive definite;
   RBF reproduces a known synthetic function (sanity regression).
3. **GPR**: cross-validation RMSE on held-out split; marginal likelihood
   increases on training data (Seeger p.10); predictive variance shrinks near
   training points.
4. **BO**: on a synthetic 2-D test function with known global minimum, BO
   (EI) finds it within a fixed budget — validates the acquisition loop
   (Shahriari p.3).
5. **Reproducibility**: fixed random seeds; results tables regenerated by a
   single `run_all.py`; all numbers in `results/` traceable to a script.
6. **Literature cross-check**: every claim in `memory/` carries page + quote
   (as above).

---

## Deliverables
- `src/` — `features.py` (SOAP+pooling), `gpr.py` (kernel comparison),
  `bo.py` (BO loop), `run_all.py`, `tests/`.
- `results/` — tables + plots above.
- `memory/` — literature notes with page/quote.
- `diary.md` — append-only journal after each task.